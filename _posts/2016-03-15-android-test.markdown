---
layout: post
title:  "在Android Studio中进行单元测试和UI测试"
date:   2016-03-15 16:52:05 +0800
categories: android
---

## 概述
> 相信大家在 Android 开发过程中都会遇到需要单元测试和 UI 测试的时候，可是对于初学者又经常找不到头绪。今天我们就来讲讲 Android studio 里面是怎么进行测试的。

> * 编写运行在本机 Java 虚拟机上的单元测试
> * 编写运行在手机或者模拟器上的 Espresso 测试


### 一、配置支持单元测试的工程

> 在写测试之前，让我们做下简单的检查，确保工程配置正确。

> 首先，确认在 Build Variants 窗口内的 Test Artifact 中选择了 "Unit Tests" 。

![](/image/img20160315001.png)

>如果找不到 Build Variants 窗口的同学，可以按下图进行搜索。

![](/image/img20160315000.png)

> 最后，打开工程的 `build.gradle` 文件，添加 JUnit4 依赖。


* build.gradle

```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:22.0.0'
    compile 'com.android.support:design:22.2.0'
}
```
> 当你同步 gradle 配置是，可能需要联网下载 JUnit 依赖。

#### 1、创建一个单元测试

> 现在，工程目录是这个样子。

![](/image/img20160315002.png)

> 万事俱备，让我们开始写一个测试吧。首先，创建一个简单的被测试类 `Calculate` 类。

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

> 接下来，创建一个测试类。Android studio 提供了一个快速创建测试类的方法。只需要在编辑器内右击，选择 `Go to > Test` ,然后 `Create a new test...`

![](/image/img20160315003.png)

> 在打开的对话窗口中，选择 JUnit4 和 "setUp/@Before" ，同时为所有的方法生成测试方法。

![](/image/img20160315005.png)

> 这样，就会在正确的文件夹内生成测试类框架，在框架内填入测试方法即可。下面是一个示例：


* CalculatorTest.java

```java
public class CalculatorTest {

    private Calculator calculator;

    @Before
    public void setUp() throws Exception {

        calculator = new Calculator();
    }

    @Test
    public void testSum() throws Exception {

        assertEquals(6d, calculator.sum(1d, 5d), 0);

    }

    @Test
    public void testSubstract() throws Exception {

        assertEquals(1d, calculator.substract(5d, 4d), 0);
    }

    @Test
    public void testDivide() throws Exception {

        assertEquals(4d, calculator.divide(20d, 5d), 0);
    }

    @Test
    public void testMultiply() throws Exception {

        assertEquals(10d, calculator.multiply(2d, 5d), 0);
    }
}
```

#### 2、运行单元测试

> 在运行单元测试之前，记得把 `Calculator.java` 里面的方法实现。

* Calculator.java

```java
public class Calculator {

    public double sum(double a, double b) {
        return a + b;
    }

    public double substract(double a, double b) {
        return a - b;
    }

    public double divide(double a, double b) {
        return a / b;
    }

    public double multiply(double a, double b) {
        return a * b;
    }
}
```

>
可能你已经注意到了 Android Studio 从来没有让你连接设备或者启动模拟器来运行测试。那是因为，位于 src/tests 目录下的测试是运行在本地电脑 Java 虚拟机上的单元测试。编写测试，实现功能使测试通过，然后再添加更多的测试...这种工作方式使快速迭代成为可能，我们称之为测试驱动开发。值得注意的是，当在本地运行测试时，Gradle为你在环境变量中提供了包含 Android 框架的 android.jar 包。但是它们功能不完整（所以，打个比方，你不能单纯调用 Activity 的方法并指望它们生效）。推荐使用 [Mockito][1] 等 mocking 框架来 mock 你需要使用的任何 Android 方法。对于运行在设备上，并充分利用 Android 框架的测试，请继续阅读本篇教程的下个部分。

### 二、配置支持Instrumentation测试的工程

#### 1、添加依赖包
>
虽然在 Android 框架内支持运行 instrumentation 测试，但是目前开发重心主要集中在刚刚发布的作为 Android Testing Support Library 一部分的新的 `AndroidJUnitRunner` 。测试库包含 Espresso ，用于运行功能UI测试的框架。让我们通过编辑 `build.gradle` 的相关部分来把它们添加进我们的工程。

```
defaultConfig {
        applicationId "com.chenfeng.crash"
        minSdkVersion 15
        targetSdkVersion 22
        versionCode 1
        versionName "1.0"

        //add this line
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
```

```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:22.0.0'
    compile 'com.android.support:design:22.2.0'

    //add these lines
    androidTestCompile 'com.android.support.test:runner:0.2'
    androidTestCompile 'com.android.support.test:rules:0.2'
    androidTestCompile 'com.android.support.test.espresso:espresso-core:2.1'
}
```
> **重要：** 由于一些依赖版本冲突，你需要确认com.android.support:appcompat-v7库的版本号是`22.0.0`，像上面的代码片段一样。另外，Android Studio可能会提醒你Build Tools 22.0.1没有安装。你应该接受修复建议，Studio会为你安装Build Tools或者在build.gradle中把这行修改成已经安装在你电脑的版本。

> 如果报 design 包与 annotations 相关的冲突时，到 `最外层的 build.gradle` 文件里添加下面几行代码。

```
allprojects {
    repositories {
        jcenter()
    }

    //add these lines
    configurations.all {
        resolutionStrategy.force 'com.android.support:support-annotations:23.0.0'
    }
}
```

上面的工作完成后，在 **Build Variants** 窗口内切换成 **Android Instrumentation Tests**，你的工程应该自动同步。如果没有，点击 **Gradle sync** 按钮。

#### 2、添加简单的交互

>
在使用Espresso进行UI测试前，让我们为app添加一些Views和简单的交互。我们使用一个用户可以输入名字的 EditText ，欢迎用户的 Button 和用于输出的 TextView 。

![](/image/img20160315006.png)

> 打开  `res/layout/activity_test.xml` ，粘贴如下代码：

* activity_test.xml

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context=".TestActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="hello world" />

    <EditText
        android:id="@+id/editText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/textView"
        android:hint="Enter your name here" />

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/editText"
        android:onClick="sayHello"
        android:text="Say hello!" />
</RelativeLayout>
```

> 还需要在TestActivity.java中添加onClick handler：

* TestActivity.java

```java
public class TestActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_test);
    }

    public void sayHello(View v) {
        TextView textView = (TextView) findViewById(R.id.textView);
        EditText editText = (EditText) findViewById(R.id.editText);
        textView.setText("Hello, " + editText.getText().toString() + "!");
    }
}
```

#### 3、创建并运行Espresso测试

> 在 `androidTest` 里创建一个新的 Java 类。可以将它命名为 TestActivityInstrumentationTest.java ,将如下代码粘贴过去。

* TestActivityInstrumentationTest.java

```java
@RunWith(AndroidJUnit4.class)
@LargeTest
public class TestActivityInstrumentationTest {


    @Rule
    public ActivityTestRule<TestActivity> mActivityRule = new ActivityTestRule<>(
            TestActivity.class);

    @Test
    public void sayHello() {

        for (int i = 0; i < 10; i++) {
            String STRING_TO_BE_TYPED = "Peter " + i;
            onView(withId(R.id.editText)).perform(typeText(STRING_TO_BE_TYPED), closeSoftKeyboard()); //line 1
            onView(withText("Say hello!")).perform(click()); //line 2

            String expectedText = "Hello, " + STRING_TO_BE_TYPED + "!";
            onView(withId(R.id.textView)).check(matches(withText(expectedText))); //line 3
            onView(withId(R.id.editText)).perform(clearText());
        }

    }
}
```
> 测试类通过 `AndroidJUnitRunner` 运行，并执行 `sayHello()` 方法。下面将逐行解释都做了什么：

> * 首先，找到 ID 为 editText 的 view ，输入 Peter ，然后关闭键盘；
> * 接下来，点击 Say hello! 的 View ，我们没有在布局的XML中为这个 Button 设置 id ，因此，通过搜索它上面的文字来找到它；
> * 最后，将 TextView 上的文本同预期结果对比，如果一致则测试通过；

> 你也可以右键点击域名运行测试，选择 `Run>MainActivityInstrume...`。

![](/image/img20160315007.png)

> 这样就会在模拟器或者连接的设备上运行测试，你可以在手机屏幕上看到被执行的动作（比如在 EditText 上打字）。最后会在 Android Studio 输出通过和失败的测试结果。

![](/image/img20160315008.png)

[1]:http://mockito.org/
