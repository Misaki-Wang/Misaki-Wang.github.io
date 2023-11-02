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

  

