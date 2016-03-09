---
layout: post
title:  "JavaScript 的柯里化函数"
date:   2015-10-29 00:00:00 +0800
categories: JavaScript
---

翻译，原文链接 [http://www.sitepoint.com/currying-in-functional-javascript/][0]

柯里化是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。

#【新手教程】JavaScript的柯里化函数

柯里化，或者说部分应用，是一种函数式编程的技术，对于熟悉以传统方式编写 JavaScript 代码的人来说可能会很费解。但如果使用得当，它可以使你的 JavaScript 函数更具可读性。

## 更具可读性和灵活性

函数式 JavaScript 被吹捧的优点之一就是拥有短小紧凑的代码风格，可以用最少行数、更少重复的代码得到正确的结果。有时这会以牺牲可读性为代价；如果你还不熟悉函数式编程的方法，这种方法写的代码会很难阅读和理解。

如果之前你遇到过柯里化这个术语，但是不知道它是什么意思，把它当做奇怪的、难理解的技术而不去理会，这也是可以理解的。但是事实上柯里化是一个非常简单的概念，它在处理函数参数的时候，涉及到一些常见的问题，同时为开发人员提供了灵活的选择范围。

## 什么是柯里化

简单来说，柯里化是一种允许使用部分函数参数构造函数的方式。也就是意味着，你在调用一个函数时，可以传入需要的全部参数并获得返回结果，也可以传入部分参数并的得到一个返回的函数，它需要传入的就是其余的参数。它真的就是那么简单。

在像 Haskell 和 Scala 这样的函数式编程语言中，柯里化是其基本原理。 JavaScript 具有函数式编程的能力，但是并没有默认支持柯里化函数（至少目前的版本没有支持）。但是我们已经知道了一些函数式的技巧，那我们就也可以让在 JavaScript 里实现柯里化。

为了让你理解柯里化的原理，我们开始来写第一个柯里化的 JavaScript 函数，使用熟悉的语法去新建一个我们想要的柯里化函数。比如说，我们假设有个函数是问候某个人的名字。我都很容易想到，创建一个简单的函数，接受问候语和一个名字作为参数，然后在控制台打印出整个问候的句子：


    var greet = function(greeting, name) {
      console.log(greeting + ", " + name);
    };
    greet("Hello", "Heidi"); //"Hello, Heidi"

这个函数需要把名字和问候语两个参数全部提供，才能够正确运行。但是我们可以使用简单的嵌套的柯里化方法重写这个函数，这样基本的函数只需要一个参数问候语，并返回以需要问候的名字为参数的另一个方法。

## 第一个柯里化函数

    var greetCurried = function(greeting) {
      return function(name) {
        console.log(greeting + ", " + name);
      };
    };
    
我们刚刚所写的这种小小的调整，使我们获得了一个可以接受任何问候语为参数的新函数，并且返回一个以我们想要问候的人名为参数新函数：

    var greetHello = greetCurried("Hello");
    greetHello("Heidi"); //"Hello, Heidi"
    greetHello("Eddie"); //"Hello, Eddie"
    
我们也可以直接调用原始的柯里化函数，只需要把每个参数分别加上小括号，一个接着一个：

    greetCurried("Hi there")("Howard"); //"Hi there, Howard"

为什么不在你的浏览器里试试？

## 全部都柯里化

炫酷的事情来了，现在我们已经学会了如何用这种方法处理传统函数的参数，我们可以处理尽可能多的参数：

    var greetDeeplyCurried = function(greeting) {
      return function(separator) {
        return function(emphasis) {
          return function(name) {
            console.log(greeting + separator + name + emphasis);
          };
        };
      };
    };
    
当有四个参数时和两个参数相比，具有同样地灵活性。无论嵌套多少层，我们都能够写出一个新的定制化的问候函数，无论我们选择了多少人、以多少种想要的方式。

    var greetAwkwardly = greetDeeplyCurried("Hello")("...")("?");
    greetAwkwardly("Heidi"); //"Hello...Heidi?"
    greetAwkwardly("Eddie"); //"Hello...Eddie?"

更重要的是，在原始的柯里化函数的基础上，我们可以传入需要的参数来创建一个新的变异函数，这个新的函数能够继续接受其余的参数，每个参数分别在一个圆括号里：

    var sayHello = greetDeeplyCurried("Hello")(", ");
    sayHello(".")("Heidi"); //"Hello, Heidi."
    sayHello(".")("Eddie"); //"Hello, Eddie."

并且我们可以很容易地定义再下一级的变异函数：

    var askHello = sayHello("?");
    askHello("Heidi"); //"Hello, Heidi?"
    askHello("Eddie"); //"Hello, Eddie?"

## 柯里化经典函数

你可以看到这种方法是多么的强大，尤其是当你需要许多复杂的定制化的函数时。唯一的问题就是语法。当你建立这些柯里化函数的时候，需要保证嵌套的返回子函数，并且调用它们的时候需要多组圆括号，每个都包含一个自己独立的参数。这会变得一团糟。

为了解决这个问题，一种方法就是去新建一个快速的脏柯里化函数，它以一个已经存在的没有嵌套返回的函数为参数。这个柯里化函数需要得到传入函数的参数列表，并用这些函数返回原始函数的柯里化版本：

    var curryIt = function(uncurried) {
      var parameters = Array.prototype.slice.call(arguments, 1);
      return function() {
        return uncurried.apply(this, parameters.concat(
          Array.prototype.slice.call(arguments, 0)
        ));
      };
    };
    
为了使用这种方式，我们传入带有任意个参数的函数名字，以及我们想预填充的若干个参数。我们得到的就是一个需要其余参数的函数：

    var greeter = function(greeting, separator, emphasis, name) {
      console.log(greeting + separator + name + emphasis);
    };
    var greetHello = curryIt(greeter, "Hello", ", ", ".");
    greetHello("Heidi"); //"Hello, Heidi."
    greetHello("Eddie"); //"Hello, Eddie."
    
正如之前，我们在用柯里化函数构建子函数的时候，并不会限制参数的个数：

    var greetGoodbye = curryIt(greeter, "Goodbye", ", ");
    greetGoodbye(".", "Joe"); //"Goodbye, Joe."

## 认真思考柯里化

我们这个小小的柯里化函数可能无法处理所有的边缘情况，比如丢失或可选参数，但只要我们严格按照语法传递参数，它就能够有效工作。

一些函数式的 JavaScript 库，如 [Ramda][1] 拥有更灵活的柯里化功能，它可以打乱一个函数需要的参数，并允许你单独或分组地传入参数，以创建一个定制化的柯里化的变形函数。如果你想广泛地柯里化，这可能是一个方向。

无论你选择如何对程序进行柯里化，是只用嵌套的括号还是更倾向于包括一个更稳健的柯里化函数，使用统一的命名规范有助于使你的代码更具可读性。每个派生出的函数都应该有一个可以清楚表明它行为和期望参数的名字。

## 参数顺序

进行柯里化的时候，一定要记住参数的顺序很重要。使用我们刚刚讲的方法，你很明显想要原始函数的最后一个的参数，是从柯里化函数一层层变形得到的函数的参数。

提前考虑的参数顺序会使柯里化更容易地应用到你的项目中。并且为了适应更多地情况，在设计函数时，考虑按照容易变化的程度排列参数的顺序不是一个坏习惯。

## 结语

柯里化对于函数式 JavaScript 是一种极其有用的技术。它允许你生成一个简洁、易配置、表现统一的库，而且使用起来上手快、具有可读性。在你的编码实践中加入柯里化会激励你在全部代码中的部分函数上应用它，这样避免了很多潜在的重复工作，并可以帮助你养成关于函数命名和处理函数参数的好习惯。

如果你喜欢这篇文章，你可能也会喜欢这个系列的其他文章：

* [An Introduction to Functional JavaScript][2]
* [Higher-Order Functions in JavaScript][3]
* [Recursion in Functional JavaScript][4]

  [0]: http://www.sitepoint.com/currying-in-functional-javascript/
  [1]: http://ramdajs.com/0.18.0/index.html
  [2]: http://www.sitepoint.com/introduction-functional-javascript/
  [3]: http://www.sitepoint.com/higher-order-functions-javascript/
  [4]: http://www.sitepoint.com/recursion-functional-javascript/