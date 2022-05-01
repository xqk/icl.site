---
title: "Git分支命名规范"
description: "主分支 Master，代码库应该有一个、且仅有一个主分支。所有提供给用户使用的正式版本，都在这个主分支上发布。
主分支必须是可用的、稳定的、可直接发布的版本，不能直接在主分支上开发。"
date: 2022-05-01T17:31:06+08:00
slug: ""
tags: []
draft: false
---
## 分支规范

|分支|命名|说明|
|---|---|---|
|主分支|master|主分支，所有提供给用户使用的正式版本，都在这个主分支上发布|
|开发主分支|develop|开发分支，永远是功能最新最全的分支|
|功能分支|feature-*|新功能分支，某个功能点正在开发阶段|
|发布版本|release-*|发布定期要上线的功能|
|修复发布版本分支|bugfix-release-*|修复测试bug|
|紧急修复分支|bugfix-master-*|紧急修复线上代码的 bug|

<br>

### 主分支 Master
代码库应该有一个、且仅有一个主分支。所有提供给用户使用的正式版本，都在这个主分支上发布。
主分支必须是可用的、稳定的、可直接发布的版本，不能直接在主分支上开发。

<br>

### 开发主分支Dev
Master主分支只用来发布重大版本，日常开发应该在另一个分支上完成，我们把开发用的分支，叫做Dev。
这个分支可以用来生成代码的最新隔夜版本（nightly）。
如果想正式对外发布，就在Master分支上对Dev分支进行合并（merge）。
Dev分支代码永远是最新的，所有新功能以这个分支来创建自己的开发分支，该分支只做合并操作，不能直接在该分支上开发。

<br>

### 功能分支Feature
功能分支的名字，可以采用feature-*的形式命名，以自己开发的功能命名。
功能分支是分配开发不同的功能用的，从Dev创建功能分支，然后完成相应功能开发后合并回Dev分支并删除该功能分支

<br>

### 预发布分支Release
预发布分支名字，可以采用release-*的形式命名
预发布分支是指发布正式版本之前（即合并到Master分支之前），我们可能需要有一个预发布的版本进行测试。
预发布分支是从Dev分支上分出来的，预发布结束之后（即测试没有问题之后），必须合并进Dev和Master。

<br>

### 修复预发布分支Release-bug
修复预发布分支的bug，可以采用release-bug-*的形式命名
在预发布版本测试出现bug时，从release分支创建分支进行bug修复，bug修复完成后在合并会release分支

<br>

### 紧急修补分支Bug
修补分支的名字，可以采用bugfix-master-*的形式。
该分支是为了紧急修复线上的bug。
软件正式发布之后，难免会出现bug。这时就需要创建一个分支，进行bug修补。
修补bug分支是从Master分支上面分出来的。修补结束之后，再合并进Master和Dev分支。
尽量避免线上问题的出现

<br>

注：一个分支尽量开发一个功能模块，不要多个功能模块在一个分支上开发.
feature分支申请合并之前，最好先pull一下dev分支下来，看一下有没有冲突，如果有冲突就先解决冲突后再合并回dev