title: android-studio-proguard
date: 2016-04-29 10:24:55
tags: android proguard
---

1. proguard的简介
```
1> proguard 主要的作用 混淆代码和压缩资源
```
2. proguard 的主要步骤
```
1> shrinking step 侦测和移除无用的class 字段 方法
2>optimization step 优化字节码
3>obfuscation step  混淆代码
4>preverification step 添加预认证信息（-target 1.6)
```
2. 文件过滤字符
```
1>*  匹配不含目录分隔符的文件名的任何部分（test*.class 以test开头的class文件）
2>** 匹配包含目录分隔符的文件名的任何部分（java\**.class java目录下的所有class文件）
3>? 代表单个字符（test?.class 可能是test1.class testa.class,但不会是test12.class）
4>！排除字符（！foo.class,**.class 任何class文件除了foo.class）
```
2. proguard 配置文件的书写（Options）
```
1>@filename 读取配置文件
2>-include filename 等同于@filename
3>-basedirectory directoryname
工程base目录，配置文件都是现对于它
4>-injars class_path
要处理的jar包,这其中包括（jar or aars, wars, ears, zips, apks, or directories)
5>-outjars class_path
处理后的生成新的jar包
6>-libraryjars class_path
7>-keepdirectories
指定写入到outjars的目录
8>
```
2. proguard 命令行的使用
```
1>java -jar proguard.jar options ...
2>把所有的配置放在@myconfig.pro文件中
java -jar proguard.jar @myconfig.pro
3>把基本的配置放在@myconfig.pro文件中并且可以添加其他配置选项
java -jar proguard.jar @myconfig.pro -verbose（详尽的展示）
```
