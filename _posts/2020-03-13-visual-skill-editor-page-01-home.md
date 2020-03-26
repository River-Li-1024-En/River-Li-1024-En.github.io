---
layout: post
title: Introduction
categories: Combat
description: 
keywords: combat,skill,editor
---

**A skill editor based on instruction set: abstract skills as the main line based on the timeline (control logic and jumps are also supported), express skill logic by attaching skill instructions on the timeline.**

Project [Address](https://github.com/River-Li-1024/VisualSkillEditor)
Engineering [document wiki](https://github.com/River-Li-1024/VisualSkillEditor/wiki)

## Table of contents

-[Background](#Background)
-[Documents](#Documents)
-[Compile](#Compile)

## Background

** Why do you need such an editor **
> In many types of games, good skill performance is a big selling point of the game. The structure of the skill system is complex, involving almost all of the basic subsystems in the game, while providing support for the upper-level logic. Planning teams often expect to be able to organize skill logic in an intuitive and independent way. They want to be able to freely combine skill logic, but do not want configuration too complicated. Such contradictions often make program design go to two extremes:
1. One way is to provide a parallel scalable way, constantly close the logic to meet personalized needs. This method often performs well in the early stage, but when the project scale swells to a certain degree, it is very likely that the complexity will increase and cause out of control;
2. Another way is to split the planning requirements into sub-logics that are as independent as possible. This approach places high demands on the programmer's business abstraction and code design capabilities. The code needs to be maintained continually and cautiously. At the same time, solving redundant and complex configuration methods is also a major challenge.

In response to the above problems, we introduce a skill system based on instruction set: abstract skills as the main line based on the time axis (also supports control logic and jumps), and express skill logic by attaching skill instructions on the time axis.

**Provides the following features:**

1. Provide 30+ instructions, all of which are instant instructions, basically meet the orthogonality, are independent and easy to expand;
2. Use xml format to save skill logic, while providing a standardized set of parsing and execution runtimes (extra);
3. Visual editing, drag and drop instructions and control structures in the editor at a glance;

## Documentation

-[Editor Help](https://river-li-1024-en.github.io/2020/03/13/visual-skill-editor-page-04-editor)
> Easy editor manual

-[Skill Structure and Instruction Set](https://river-li-1024-en.github.io/)
> Introduce skill paragraphs, instruction sets and related key designs

-[Skill Runtime](https://river-li-1024-en.github.io/2020/03/13/visual-skill-editor-page-02-instructions/)
> Skill parser and execution engine


## Compile

Download the generated version:
[Win32 version](https://github.com/River-Li-1024/VisualSkillEditor/tree/master/Versions)

*****
Visual Studio compilation:
1. Installation environment, refer to [Qt Environment Building (Visual Studio)](https://blog.csdn.net/liang19890820/article/details/49874033)
2. Visual Studio opens the project file SkillEditor.sln.
3. Compile the project and export it to the Bin directory.
4. (Optional) If you need to migrate to another environment, run the deployment file Bin/Deploy.cmd (you need to specify the Qt installation directory).

*****
Qt Creator compilation:
1. Install Qt, all Qt versions [download](http://download.qt.io/archive/qt/).
2. Qt opens the project file SkillEditor.pro.
3. Compile the project and export it to the Bin directory.
4. (Optional) If you need to migrate to another environment, run the deployment file Bin/Deploy.cmd (you need to specify the Qt installation directory).
