title: gradle-exercise
date: 2016-01-07 17:48:01
tags: gradle_proguard
---

build.gradle
```
apply plugin: 'java'

repositories{
    mavenCentral()
}

version='1.0.2'

dependencies{

}


sourceCompatibility= 1.8
targetCompatibility=1.8

//编译生成jar包的名字一般为：工程名-版本号
archivesBaseName="test"
//自定义生成jar包的Manifest文件
jar{
    manifest{
        attributes 'Implemention-Version': version,'Implemention-Title':'Gradle QuickStart'
    }
}
//改变工程的布局 要编译为jar的源码和资源
sourceSets {
    main {
        java {
            srcDir 'src/java'//方法名为srcDir
        }
        resources {
            srcDir 'src/resources'
        }

        output.resourcesDir='build/output/res'//属性为resourcesDir
        output.classesDir='build/output/bin'
        println name
        println compileClasspath.getAsPath()

    }
    println name

}


//加载Proguard的库
buildscript {
    repositories {
        flatDir dirs: '/home/xujianhua/Documents/tool/proguard5.2.1/lib'
    }
    dependencies {
        classpath ':proguard:'
    }
}

//自定义ProguardTask的任务
task proguard(type: proguard.gradle.ProGuardTask) {
        version '1.0.4'
        injars  'build/libs/test-1.0.4.jar'//要进行proguard的处理的jar
        outjars 'build/apk/test-1.0.4.jar'//proguard处理后的jar
        libraryjars "${System.getProperty('java.home')}/lib/rt.jar"//所要依赖的lib
        println "${System.getProperty('java.home')}"
        keepclassmembers allowshrinking:true, 'enum * { \
        public static **[] values(); \
        public static ** valueOf(java.lang.String); \
        }'
        keep 'class * extends main.java.Person'
        printmapping 'out.map'
}

task end(){
    println '------------------------>end'
}









```
