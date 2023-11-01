# Introduction of Static Analysis

**Homepage:**[Static Program Analysis | Tai-e (pascal-lab.net)](https://tai-e.pascal-lab.net/lectures.html)

**Video:**[Videos-2020-bilibili](https://space.bilibili.com/2919428/channel/collectiondetail?sid=342930&ctype=0)

**Notes:**[Static Program Analysis Book (gitbook.io)](https://ranger-nju.gitbook.io/static-program-analysis-book/)          [Github-reop](https://github.com/RangerNJU/Static-Program-Analysis-Book)

**Homework:**[Tai-e](http://tai-e.pascal-lab.net/intro/overview.html)  [PASCAL OJ (pascal-lab.net)](https://oj.pascal-lab.net/problem)   

## Overview of Static Analysis

**Perfect static: Sound & Complete**
![sound & complete](https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202310301512736.png)

**Useful static analysis: Soundness is preferable and critical**
![useful-static](https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202310301514280.png)

**Static Analysis**: ensure (or get close to) **soundness**, while making good trade-offs between analysis **precision** and analysis **speed**.

**Two Key words:**

* **Abstraction**
* **Over-approximation:**
  * **Transfer function**
  * **Control Flow**

> **Q: What are the differences between static analysis and (dynamic) testing?**
>
> **A: **
> **Static analysis and dynamic testing (often just referred to as "testing") are two principal methods used in the software development lifecycle to ensure code correctness, security, and quality. However, they operate on different principles and at different stages of development. Here's a summary of their key differences:**
>
> 1. **Nature of Analysis**:
>    * **Static Analysis**:
>      * **It examines the program's code, structure, and documentation without executing it.**
>      * **It analyzes possible execution paths, data flows, and configurations to detect potential issues.**
>    * **Dynamic Testing**:
>      * **It involves actually executing the program.**
>      * **It observes the program's behavior during runtime using specific inputs and under specific conditions.**
> 2. **Time of Execution**:
>    * **Static Analysis**: Performed before the program is run, often during the coding phase or as part of the build process.
>    * **Dynamic Testing**: Performed after the code has been compiled and run, typically during the testing or deployment phase.
> 3. **Issues Detected**:
>    * **Static Analysis**:
>      * **Detects issues like syntax errors, coding standards violations, potential security vulnerabilities, unreachable code, and potential null pointer dereferences.**
>      * **Can identify issues that might not manifest during typical program execution.**
>    * **Dynamic Testing**:
>      * **Detects issues that manifest at runtime, such as logic errors, incorrect outputs for given inputs, memory leaks, and runtime exceptions.**
>      * **Requires the issue to manifest during a test to be detected.**
> 4. **Tools & Techniques**:
>    * **Static Analysis**: Uses tools like linters (e.g., `ESLint` for JavaScript, `Flake8` for Python) and advanced static analyzers (e.g., `Coverity`, `Cppcheck`, `Clang Static Analyzer`).
>    * **Dynamic Testing**: Uses testing frameworks (e.g., `JUnit` for Java, `pytest` for Python), performance testing tools (e.g., `JMeter`), and runtime monitoring tools.
> 5. **Coverage**:
>    * **Static Analysis**: Has the potential to cover all code paths since it examines the entire codebase.
>    * **Dynamic Testing**: Only covers the paths that are exercised by the tests. If a test case doesn't cover a particular scenario, that path remains untested.
> 6. **Feedback Time**:
>    * **Static Analysis**: Can provide immediate feedback as developers write or commit code.
>    * **Dynamic Testing**: Feedback is usually provided after the test cases are executed, which could be after development.
> 7. **Limitations**:
>    * **Static Analysis**:
>      * **May produce false positives.**
>      * **Can't detect runtime-specific issues or environment-specific behaviors.**
>    * **Dynamic Testing**:
>      * **Can miss defects if not covered by test cases.**
>      * **Requires setting up the test environment, writing test cases, and sometimes complex infrastructure.**
> 8. **Goal**:
>    * **Static Analysis**: Ensure code quality, maintainability, and conformance to coding standards. It also helps in early detection of potential issues.
>    * **Dynamic Testing**: Validate functional correctness, performance, and runtime behavior of the program.
>
> **Both static analysis and dynamic testing are vital in the software development process. Static analysis helps catch issues early in the development lifecycle, reducing the cost of fixing defects. Dynamic testing ensures that the software behaves as expected in real-world scenarios. Using both methods in conjunction offers a more holistic approach to software quality assurance.**



## Syllabus & Assignment

![image-20231101192747712](https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202311011927775.png)



<img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202311011927542.png" alt="image-20210902201618713"  />
