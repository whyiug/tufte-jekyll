---
layout: post
title: "powershell 入门"
description: ""
date: 2016-10-13
tags: [powershell]
comments: true
share: true
---

## tips

### 管理进程
1 Get-Process  
2 Get-Process | Out-GridView

# 执行脚本

## 策略

1 Get-ExecutionPolicy: Restricted  
2 Set-ExecutionPolicy RemoteSigned

## hello world.ps1

```ps1
$a = "hello world"
echo $a
```