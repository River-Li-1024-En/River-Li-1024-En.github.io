---
layout: post
title: Skill Editor 02 - Skill Structure and Instruction Set
categories: Combat
description: 
keywords: combat,skill,editor
---

## Table of contents

-[Section](#Section)
-[Instruction](#Instruction)
-[Parameter](#Parameter)
-[Q&A](#Q&A)

## Paragraph

The logic of skills is divided into 7 sections, which are:
+ Section-start: onstart, which indicates the start of the skill, only execute once, and then start executing other sections after completion;
+ Section-frame trigger: onframe, trigger every frame, you can set the maximum total duration;
+ Section-interval triggering: ontick, which triggers every fixed interval, and can set the maximum total duration;
+ Section-end: onfinish, the end of the skill is triggered, and the interruption will still trigger the end section;
+ Section-break: onbreak, triggered when the skill is interrupted, and the end section will be triggered after the interruption;
+ Section-Custom: id can be customized and triggered by gotosection, which is equivalent to procedure call;
+ Section-custom event: id can be customized, triggered by sendevent, and executed concurrently with other sections;

![Section structure](/images/posts/visualskilleditor/guild-sections.png)

## Instruction

**Event instruction** abstracts the atomic events that need to be executed in the skill logic, which basically meets the orthogonality. A certain degree of compromise has been made in instruction granularity and convenience. The latter is the first principle, but redundancy has been avoided as a whole.
All event instructions are divided into 7 instruction groups, which are:
+ Instruction-control: used to implement control logic, such as process control, time control, logic jump, etc.
+ Instruction-Performance: Client-side performance related instructions, including animation, special effects, camera, sound, bullet time effects, etc .;
+ Instruction-Move: Position and orientation related instructions, including both movement and steering instructions, as well as selecting points and calculating orientation;
+ Instruction-scan: Various scanning instructions, providing multiple range search instructions;
+ Instruction-role: role operation instruction;
+ Instruction-effect: operation effect (impact);
+ Instruction-tool: auxiliary instruction, only for editor;

![Event instruction list 1](/images/posts/visualskilleditor/guild-actions1.png)
![Event instruction list 1](/images/posts/visualskilleditor/guild-actions2.png)

**Condition instruction** abstracts some possible judgment conditions in logic control, and is used to modify the execution trajectory of skills during runtime.
> Conditional instructions only return true/false for control instructions in event instructions;
> Only the individual necessary conditions are independent instructions, so the number of conditional instructions needs to be strictly controlled.
In project practice, it is found that most planning is difficult to manage overly complex control logic, so in the design tends to build conditions into event instructions (as a parameter).

![Conditional instruction list](/images/posts/visualskilleditor/guild-conds.png)

## Parameter

Each instruction has multiple parameters, which are described by key-value. Take scancircle as an example:
![Parameter Description](/images/posts/visualskilleditor/guild-params.png)
As shown in the figure, the parameters specify the data type and scope of this instruction, as well as information such as whether to force configuration.

The above screenshot is from the [instruction set description file](https://github.com/River-Li-1024/VisualSkillEditor/blob/master/Bin/Config/SkillSpec.xml)
> This file is used as the instruction parameter description file and the editor's interface configuration file. Developers can edit it freely when they need to add instructions.


## Q&A:

### How to control time
> Either absolute time (relative to skill start time) or relative time (relative to the previous instruction).
There is no essential difference between the two, but I personally recommend the latter. Although the absolute time is simple in logic when executing instructions, subsequent skills cannot be automatically postponed when the conditional branch exists.
More importantly, once the bullet time (slow-speed close-up) function needs to be implemented, relative time is the only choice. Just provide a wait (wait for a period of time) instruction to achieve the time control function, of course, also provide waitfinish (waiting for the completion of the previous instruction) to provide dynamic effects.

### How to express conditional control statements
> The conditional statement itself is a node in an xml, but there are two ways to express the condition: One is to save the condition as a node's attribute. This method is feasible and concise, but the meaning of the condition parameter is difficult to describe. (General attribute); Another way is to save the condition as a child of the node. I choose this method here because the way of child nodes is more conducive to expansion and organization.
> An if/elseif/else structure is represented as (corresponding to a select instruction):
> [select statement](/images/posts/visualskilleditor/select.png)

### How to implement goto to a custom section
> Put a custom section into the section node, as shown in the "Skill logic structure" above. The execution flow is jumped by the gotosection instruction (the logic of the original execution flow is no longer executed).
Use the following instructions to jump to the custom logic;
```xml
<action id = "gotosection" eventid = "sec_name" />
```
> Custom sections have multiple uses, which are equivalent to functions or procedures in the language, which can express the logic of jumps and reuse local logic;

### How to implement parallel logic
> Sometimes you want your skills to run in multiple timelines in parallel, as shown below:
>![Parallel](/images/posts/visualskilleditor/concurrent.png)
> The sendevent (trigger event) instruction can achieve this function, and the logic that needs to run concurrently is organized as an on node, as shown in the "skill logic structure" in the figure above. This instruction can also be used to send various instructions to the game to trigger other game systems, such as scene effects, plot logic, etc .; the following instructions can be used to trigger parallel logic in parallel;
```xml
<action id = "sendevent" eventid = "evt_name" />
```