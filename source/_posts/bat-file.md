title: bat-file
date: 2016-01-04 13:05:41
tags: .bat
---

1. what is bat file
.bat 是一种批处理文件,实质上是一系列的Dos命令.以下是proguard.bat的例子:

```
  @ECHO OFF

  REM Start-up script for ProGuard -- free class file shrinker, optimizer,
  REM obfuscator, and preverifier for Java bytecode.
  REM
  REM Note: when passing file names containing spaces to this script,
  REM       you'll have to add escaped quotes around them, e.g.
  REM       "\"C:/My Directory/My File.txt\""

  IF EXIST "%PROGUARD_HOME%" GOTO home
  SET PROGUARD_HOME=%~dp0\..
  :home

  java -jar "%PROGUARD_HOME%\lib\proguard.jar" %*
```

不难发现,.bat的实质就是Dos外部命令.

2. bat 基本构成
echo 表示显示该语句之后所运行的命令行
echo off 所有运行的命令不显示命令行本身
rem 表示其后面的是注释
call 表示调用另一个批处理文件
pause 暂停并且在命令行中出现'按任何键继续的提示'
