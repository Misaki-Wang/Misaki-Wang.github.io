# New LLVM PM

![img](https://pic4.zhimg.com/80/v2-a6f95b10e2ba37959a37ea8af8d6eb1f_1440w.webp)

## 示范

###  编写你的第一个新PM Pass

- **创建Pass结构体**：新PM的Pass通常是一个结构体，它包含一个`run`方法。

  ```cpp
  struct MyPass : public PassInfoMixin<MyPass> {
      PreservedAnalyses run(Function &F, FunctionAnalysisManager &FAM);
  };
  ```

- **实现`run`方法**：这个方法是Pass的核心，它定义了Pass对IR的操作。

  ```cpp
  PreservedAnalyses MyPass::run(Function &F, FunctionAnalysisManager &FAM) {
      // ... Pass逻辑 ...
      return PreservedAnalyses::all();
  }
  ```

- **分析和转换**：决定你的Pass是纯分析Pass还是会修改IR的转换Pass。这会影响你返回的`PreservedAnalyses`。

### 注册Pass

- **插件接口**：使用`llvm::PassPluginLibraryInfo`来注册你的Pass。

  ```cpp
  extern "C" LLVM_ATTRIBUTE_WEAK llvm::PassPluginLibraryInfo llvmGetPassPluginInfo() {
      return { LLVM_PLUGIN_API_VERSION, "MyPass", LLVM_VERSION_STRING, [](PassBuilder &PB) {
          PB.registerPipelineParsingCallback([](StringRef Name, FunctionPassManager &FPM, ArrayRef<PassBuilder::PipelineElement>) {
              if (Name == "my-pass") {
                  FPM.addPass(MyPass());
                  return true;
              }
              return false;
          });
      }};
  }
  ```

### 编译和测试Pass

- **编译Pass**：将你的Pass编译为一个动态库（如`.so`，`.dylib`，或`.dll`文件）。

- **测试Pass**：使用`opt`工具加载和运行你的Pass。

  ```bash
  opt -load-pass-plugin=./MyPass.so -passes="my-pass" -disable-output <input-llvm-file>
  ```

## Compare with legacy PM

https://zhuanlan.zhihu.com/p/338837812

在LLVM中，Pass Manager负责运行和管理各种编译器优化（passes）。LLVM随着时间的推移引入了新的Pass Manager，以解决旧Pass Manager的一些设计限制和性能问题。以下是新旧两种Pass Manager之间的主要区别：

### 旧Pass Manager (Legacy Pass Manager)

- **架构**：旧Pass Manager的架构比较简单，但它不够灵活，难以支持复杂的依赖关系和优化策略。
- **分析结果的重用**：在旧系统中，分析结果的重用依赖于手动管理。如果一个Pass修改了IR，那么所有依赖于被修改部分的分析结果都需要被重新计算。
- **性能**：因为缺乏对并行化的支持和分析结果管理的问题，旧Pass Manager在性能方面可能不如新系统。

### 新Pass Manager

- **模块化和重用**：新Pass Manager采用了更加模块化的设计，使得Pass之间的依赖关系和分析结果的重用更加清晰和自动化。
- **性能优化**：新Pass Manager被设计为支持更好的并发性和缓存，可以更高效地处理分析结果和Pass的执行。
- **API设计**：新Pass Manager的API被设计为更加现代和C++友好，使用了模板和类继承，这使得编写新Pass更加直观。
- **更好的依赖管理**：新Pass Manager自动处理Pass之间的依赖关系，如果一个Pass的结果被另一个Pass所依赖，它会自动保持更新。
- **易用性**：新Pass Manager简化了Pass的注册和调用，提供了更高级的Pass管理功能。

### 代码迁移

如果你有一个使用旧Pass Manager的LLVM Pass，你可能需要对它进行重写以适应新Pass Manager。新的`PassInfoMixin`提供了一个标准化的方式来定义Pass的入口点，这通常意味着需要更新Pass的实现以匹配新的API。

### 使用

- **旧Pass Manager**的Pass通常在`opt`工具中使用类似`-legacy-hello-world`这样的命令行选项。
- **新Pass Manager**的Pass使用类似`-passes="hello-world"`这样的新语法。

### 转换和兼容性

LLVM提供了一些工具和方法来帮助开发者从旧Pass Manager迁移到新Pass Manager，但需要注意的是，新旧两个系统在某些情况下可能不完全兼容，特别是在分析结果管理和Pass调度策略方面。

总结来说，新Pass Manager提供了一个更现代化和高效的框架来组织和运行编译器优化，尽管它需要一些学习和迁移工作，但它提供了性能和易用性方面的显著改进。

## Example

```cpp
//=============================================================================
// FILE:
//    HelloWorld.cpp
//
// DESCRIPTION:
//    Visits all functions in a module, prints their names and the number of
//    arguments via stderr. Strictly speaking, this is an analysis pass (i.e.
//    the functions are not modified). However, in order to keep things simple
//    there's no 'print' method here (every analysis pass should implement it).
//
// USAGE:
//    1. Legacy PM
//      opt -enable-new-pm=0 -load libHelloWorld.dylib -legacy-hello-world -disable-output `\`
//        <input-llvm-file>
//    2. New PM
//      opt -load-pass-plugin=libHelloWorld.dylib -passes="hello-world" `\`
//        -disable-output <input-llvm-file>
//
//
// License: MIT
//=============================================================================
#include "llvm/IR/LegacyPassManager.h"
#include "llvm/Passes/PassBuilder.h"
#include "llvm/Passes/PassPlugin.h"
#include "llvm/Support/raw_ostream.h"

using namespace llvm;

//-----------------------------------------------------------------------------
// HelloWorld implementation
//-----------------------------------------------------------------------------
// No need to expose the internals of the pass to the outside world - keep
// everything in an anonymous namespace.
namespace {

// This method implements what the pass does
void visitor(Function &F) {
    errs() << "(llvm-tutor) Hello from: "<< F.getName() << "\n";
    errs() << "(llvm-tutor)   number of arguments: " << F.arg_size() << "\n";
}

// New PM implementation
struct HelloWorld : PassInfoMixin<HelloWorld> {
  // Main entry point, takes IR unit to run the pass on (&F) and the
  // corresponding pass manager (to be queried if need be)
  PreservedAnalyses run(Function &F, FunctionAnalysisManager &) {
    visitor(F);
    return PreservedAnalyses::all();
  }

  // Without isRequired returning true, this pass will be skipped for functions
  // decorated with the optnone LLVM attribute. Note that clang -O0 decorates
  // all functions with optnone.
  static bool isRequired() { return true; }
};

// Legacy PM implementation
struct LegacyHelloWorld : public FunctionPass {
  static char ID;
  LegacyHelloWorld() : FunctionPass(ID) {}
  // Main entry point - the name conveys what unit of IR this is to be run on.
  bool runOnFunction(Function &F) override {
    visitor(F);
    // Doesn't modify the input unit of IR, hence 'false'
    return false;
  }
};
} // namespace

//-----------------------------------------------------------------------------
// New PM Registration
//-----------------------------------------------------------------------------
llvm::PassPluginLibraryInfo getHelloWorldPluginInfo() {
  return {LLVM_PLUGIN_API_VERSION, "HelloWorld", LLVM_VERSION_STRING,
          [](PassBuilder &PB) {
            PB.registerPipelineParsingCallback(
                [](StringRef Name, FunctionPassManager &FPM,
                   ArrayRef<PassBuilder::PipelineElement>) {
                  if (Name == "hello-world") {
                    FPM.addPass(HelloWorld());
                    return true;
                  }
                  return false;
                });
          }};
}

// This is the core interface for pass plugins. It guarantees that 'opt' will
// be able to recognize HelloWorld when added to the pass pipeline on the
// command line, i.e. via '-passes=hello-world'
extern "C" LLVM_ATTRIBUTE_WEAK ::llvm::PassPluginLibraryInfo
llvmGetPassPluginInfo() {
  return getHelloWorldPluginInfo();
}

//-----------------------------------------------------------------------------
// Legacy PM Registration
//-----------------------------------------------------------------------------
// The address of this variable is used to uniquely identify the pass. The
// actual value doesn't matter.
char LegacyHelloWorld::ID = 0;

// This is the core interface for pass plugins. It guarantees that 'opt' will
// recognize LegacyHelloWorld when added to the pass pipeline on the command
// line, i.e.  via '--legacy-hello-world'
static RegisterPass<LegacyHelloWorld>
    X("legacy-hello-world", "Hello World Pass",
      true, // This pass doesn't modify the CFG => true
      false // This pass is not a pure analysis pass => false
    );

```



# Legacy LLVM PM

## LLVM

[19_LLVM.pdf (xiongyingfei.github.io)](https://xiongyingfei.github.io/SA/2017/19_LLVM.pdf) 

### LLVM Basic

LLVM is **a compiler infrastructure** designed as a set of reusable libraries with well-defined interfaces.

Big pictures of LLVM:

- LLVM implements the entire compilation flow
  <img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202310311432785.png" alt="compilation" style="zoom:50%;" />
- off-the-shell optimizations
  - Virtual Register Allocation
  - Constant Propagation



### LLVM IR

LLVM Core Hierarchy
<img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202310311448380.png" alt="LLVM-core-hierarchy" style="zoom:50%;" />

- Module
- Function
- Value
  - local start with `%`, global with $@$
  - Type
- Basic & Instruction



### LLVM Pass

[LLVM: llvm::Pass Class Reference](https://llvm.org/doxygen/classllvm_1_1Pass.html)

- Passes perform the **transformations and optimizations** that make up the compiler, 
  they **build the analysis results** that are used by these transformations, 
  and they are, above all, **a structuring technique** for compiler code.
- One of the main features of the LLVM Pass Framework is that it schedules passes to run in an efficient way based on the constraints that your pass meets (which are indicated by which class they derive from).

- LLVM applies a chain of analyses and transformations on the  target program.
  Each of these analyses or transformations is called a **pass**. 
  Some passes, which are machine independent, are invoked by  *opt*.
  A pass may require information provided by other passes.  Such dependencies must be explicitly stated.

- A pass is an instance of the LLVM class `Pass`, `Pass` is the **base class** of all the pass
  <img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202310311524152.png" alt="Pass Class" style="zoom:50%;" />

  



