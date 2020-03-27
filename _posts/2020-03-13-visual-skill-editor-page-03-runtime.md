---
layout: post
title: Skill Editor 03 - Runtime
categories: Combat
description: 
keywords: combat,skill,editor
---

***Skill Runtime***refers to a complete set of skill loading and running logic structure. As the core of the skill system, it has high requirements for the code's operating efficiency, robustness, and scalability.

There are often several issues that need to be considered in advance when implementing a set of skills:
> 1. Do you want the skill runtime to run on the server or client?
> 2. Does the operating environment support fast loading of data from xml? Is there an efficient xml loading library?
> 3. In the operating environment, which of the memory and CPU is the more scarce resource? Do I need to enable the caching mechanism?
> 4. Do I need to support hot updates and hot reloads of skill files?
> 5. Is it necessary to build a unit test environment for such core code?

## Table of contents
-[Implementation](#Implementation)  
-[Source](#Source)  
-[New-Implementation](#New-Implementation)  

## Implementation
The following is a complete set of UML class diagrams. As shown in the figure, the entire structure is divided into 3 parts, which are:

**Load Skill Data**
1. Load skill instruction data from the xml file, perform syntax check and basic semantic analysis, and get a tree-like hierarchical structure (somewhat similar to the abstract syntax tree in a compilation system);
2. This stage will ensure that the skills data is configured correctly and that there are no obvious semantic errors;
3. All loaded skill data will be cached in SkillInstFactory to improve execution efficiency;

**Skill Runtime**
1. The core part of the skill, centered on SkillInst, associates skill instruction nodes (SkillNode) with context data (SkillInstContext). To be
2. All instructions have 4 execution phases, which are: Load (Load), Prepare (Initialize), Execute (Finish), Finish (End)
3. The context data stores intermediate data during the execution of the skill, and it can also mirror the external environment data or the intermediate transition interval of data transmission;
4. Two extension bits are provided, which are inherited from the SkillNode custom event instruction and inherited from the SkillCond custom condition instruction. When inheriting, you only need to implement the callback interface (Template Method mode).

**Call Skill**
1. The external environment only needs to call the interface on SkillInst to drive all the skills.

![Static logic implementation](/images/posts/visualskilleditor/runtime.png)

## Source
is coming ...

## New-Implementation

The above runtime environment is suitable for most situations, but there may be special requirements and restrictions in some environments, such as:
> 1. It takes a long time to load or parse XML, or it takes a long time for the operating system to load files into virtual memory. For example, under Android, files placed in StreamingAssets need to be asynchronously extracted from the jar package;
> 2. The memory is extremely scarce and cannot cache a large amount of skill data;
> 3. The CPU is extremely scarce. The above standard runtime libraries still feel complicated and want a simpler library;

#### 1. An implementation similar to the processor instruction set:
Before the processor executes the CPU instruction, the compiler flattens its control statement into a sequential instruction queue for the abstract syntax tree, and completes the symbol resolution and relocation on it, so that the execution engine only needs to execute the sequence one by one without parsing. Data (similar to assembly code). This allows subsequent instructions to be executed one by one in order to manage the configuration data and recursively execute the syntax tree. Convert the xml file to a two-dimensional sequential instruction queue in txt format, as shown in the figure below:

![Sequence instruction file](/images/posts/visualskilleditor/sequences.png)

> About CPU instruction set [reference](https://en.wikipedia.org/wiki/Instruction_set_architecture)
> The compiler link process can be [reference] (https://en.wikipedia.org/wiki/Linker_ (computing) #Relocation)
> CPU reference can be [reference](https://en.wikipedia.org/wiki/Instruction_pipelining)

#### 2. Implementation process (all processes are done by the skill editor tool)
**2.1 Flatten Control Statements**

All if/while/for/switch control statements can be implemented with goto. The following are the jump instruction versions of if, do-while, while, for, and switch:
![Control statement](/images/posts/visualskilleditor/control-flow.png)

The implementation algorithm can refer to the editor source code
```cpp
bool serializeNode (SkillScriptNodePtr nodePtr, SkillScriptNodePtr and nextNodePtr, QList <SkillScriptNodePtr> & list);
```

**2.2 Symbol resolution and relocation**

Symbol analysis: analyze all command parameters and skill dynamic parameters, and extract their values ​​as command parameters;
Relocation: Generate a unique id (similar to an absolute address) for all instructions, and replace the destination address of the jump instruction (the unique id of the instruction) with an absolute address (the unique id);
The final result is shown in the figure above. The implementation algorithm of the txt file can refer to the source code of the editor.
```cpp
void syncCurSkillAction (SkillInstDataPtr instDataPtr);
```

**2.3 Skill Runtime**

The implementation is similar to the CPU execution instruction process, which can be divided into the following stages:

**Fetching fetch:**
> 1. Read the instruction line from the instruction queue according to the value of the program counter (PC);
> 2. Calculate the address of the next instruction

**decode:**
> 1. Parse the command line data and locate the specific instruction execution body to execute:

**Execution instruction body:**
> 1. Update the status data into the context structure;
> 2. For transfer instructions and jump instructions, the next instruction address may be updated;

**Update update:**
> 1. Calculate the address of the next instruction

This algorithm is very simple, the code does not exceed 200 lines, and the following is the fetching process (C # implementation)
![runtime implementation](/images/posts/visualskilleditor/runtime-code.png)

**2.4 Further optimizations**
Saving the sequential instruction queue in a binary format can make the instruction load faster, and at the same time, can avoid a lot of unnecessary empty data in the file.