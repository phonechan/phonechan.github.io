---
layout: post
title:  "在Android Studio中进行单元测试和UI测试"
date:   2016-03-15 16:52:05 +0800
categories: jekyll update
---

## 概述
> 相信大家在Android开发过程中都会遇到需要单元测试和UI测试的时候，可是对于初学者又经常找不到头绪。今天我们就来讲讲Android studio里面是怎么进行测试的。
* 编写运行在本机Java虚拟机上的单元测试
* 编写运行在手机或者模拟器上的Espresso测试


### 配置支持单元测试的工程

> 在写测试之前，让我们做下简单的检查，确保工程配置正确。

> 首先，确认在Build Variants窗口内的Test Artifact中选择了"Unit Tests"。

![](/image/img20160315001.png)

>如果找不到Build Variants窗口的同学，可以按下图进行搜索。

![](/image/img20160315000.png)

> 最后，打开工程的`build.gradle`文件，添加JUnit4依赖。


* build.gradle

```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:22.0.0'
    compile 'com.android.support:design:22.2.0'
}
```
> 当你同步gradle配置是，可能需要联网下载JUnit依赖。

### 创建一个单元测试

> 现在，工程目录是这个样子。

![](/image/img20160315002.png)

> 万事俱备，让我们开始写一个测试吧。首先，创建一个简单的被测试类`Calculate`类。

![](/image/img20160315004.png)

* Calculate.java

```java
public class Calculator {

    public double sum(double a, double b) {
        return 0;
    }

    public double substract(double a, double b) {
        return 0;
    }

    public double divide(double a, double b) {
        return 0;
    }

    public double multiply(double a, double b) {
        return 0;
    }
}
```

> 接下来，创建一个测试类。Android studio提供了一个快速创建测试类的方法。只需要在编辑器内右击，选择 `Go to > Test`,然后`Create a new test...`

![](/image/img20160315003.png)
