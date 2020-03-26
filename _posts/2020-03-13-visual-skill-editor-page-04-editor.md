---
layout: post
title: Skill Editor 04 - Editor
categories: Combat
description: Instruction set-based skill editor: editor
keywords: combat,skill,editor
---

## Table of contents

-[Main](#Main)
-[Instruction Set](#Instruction-Set)

## Main
! [Main interface](/images/posts/visualskilleditor/editor-main.png)

The role of each part in the editing interface is as follows:
+ ** Skill List: **
Load all skill rows from the SkillInst.txt table for quick indexing;

+ ** Skill Tree: **
The xml file of a single skill is represented as a tree structure, and nodes can be added and deleted and the hierarchical relationship can be adjusted;

+ ** Instruction parameters: **
Parameter information of a single skill node, all parameters are obtained from the configuration file (SkillConfig.xml), and the dynamic parameters can be indexed;

+ ** Skill dynamic parameters: **
Some data independent and skill logic is used to separate skill logic and data. The data is stored in the dynamic parameter area of ​​SkillInst.txt according to the skill id;


## Instruction-Set
[Instruction Set Description File](https://github.com/River-Li-1024/VisualSkillEditor/blob/master/Bin/Config/SkillSpec.xml)
> This file is used as the instruction parameter description file and the editor's interface configuration file. Developers can edit it freely when they need to add instructions.