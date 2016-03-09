---
layout: post
title:  "JS 开发者常用的 10 个 Sublime Text 插件"
date:   2015-11-1 00:00:00 +0800
categories: JavaScript
---

翻译，原文链接 [http://www.sitepoint.com/essential-sublime-text-javascript-plugins/][0]

# JS 开发者常用的 10 个 Sublime Text 插件

# 10 Essential SublimeText Plugins for JavaScript Developers
**JavaScript 开发者常用的 10 个 Sublime Text 插件**

[Sublime Text][1] 是每个开发者工具箱中都应该有的一个强大的应用。它是一个跨平台的、高定制化的、先进的文本编辑器，在功能强大的 [集成开发环境][2]（众所周知地消耗资源）和类似于 [Vim][3] or [Emacs][4] 的命令行编辑器（学习成本非常高）之间取得了很好的平衡。

使得 Sublime 如此强大的原因之一就是它的可扩展的插件架构。开发者可以很容易地扩展 Sublime 的核心功能，添加一些新特性，比如补全代码、嵌入远程接口文档。Sublime Text 不带有可以启用的插件 - 它们通常通过第三方包管理器安装，简称为 [包管理器][5]。安装 Sublime Text 的包管理器，请按照 [官网的安装教程][6] 进行安装。

本文针对 JavaScript 开发者，简要介绍了十个 Sublime 插件，每一个都能够帮助你改善工作流并且提高工作效率。那么，就让我们一起来看看吧！

## 1. [Babel][7]

我首先要介绍的就是 [Babel][8] 插件。这个插件可以在你的 ES6/2015 和 React JSX 代码上添加语法高亮。安装插件之后，第一件要做的事就是把它设置为 `.es6`、 `.jsx`、 以及 `.js` 文件的默认语法。然而，如果你正在使用 ES3/5 工作，那你要注意最后一种文件，不要使用 Babel 编译你的代码。

如果你还没有享受过 [Babel][9] 的乐趣，我强烈建议你去尝试它。它允许你将 ES6/2015 和 JSX 代码编译成 ES5。并且它很好的整合了目前流行的构建工具和命令行工具。当然，它不支持传统浏览器，但是如果你想支持 IE10 或者更低版本可以在他们的 [附加文档页面][10] 查看相关说明。

不幸的是， Babel 插件不允许在 Sublime 内编译 ES6 代码。对于那些想实现此功能的人，我建议你查看 [Compile Selected ES6][11]。

[![Babel][12]][7]

## 2. [JSHint][13]

下一个就是 Sublime 的 JSHint 插件。JSHint 是一个 JavaScript 检测器，它会查看你的代码，并验证其是否具有正确的样式和语法，避免犯相关的常见错误。无论你是个新手还是老手，JSHint 都是必不可少的。查看 [JSHint about page][14] 浏览更多信息看它还可以为你做什么。

为了 JSHint 插件能够在 SublimeText 中正常工作，你需要通过 npm 全局安装 JSHint：

    npm install -g jshint

如果你不确定应该如何做这一步，请浏览 [getting started with the Node Package manager][15] 这里的教程。

一旦 npm 的 JSHint 模块和 SublimeText 的 JSHint 的插件安装好了，你就能够简便地使用 JSHint 了，只要打开 JavaScript 文件，然后按下 `Ctrl + J` （在 Linux/Windows 上是`Alt + J` ）。或者，你可以通过菜单打开 JSHint 功能。

如果你已经安装了插件，但是想要在有错误的地方有更明显的提示，请浏览 [JSHint Gutter][16]。或者，你想在安装 NPM 包和插件之前试一试 JSHint，[JSHint.com][17] 有一个很棒的在线交互工具，你可以把代码粘贴到里面得到实时的反馈。

[![Sublime Text JSHint Screenshot][18]][13]

## 3. [JsFormat][19]

JsFormat 基于 [JS Beautifier][20]，可以帮助你自动格式化 JavaScript 和 JSON。如果你只想用它格式化 JSON 字符串，那它值得你拥有。但是对于我来说，最大的优势来自于当我需要读其他开发人员的代码，甚至于是我很久以前写的代码。

这种代码通常可读性差，统一的格式化代码样式会非常有帮助。尽管格式化工具并不适合每个人，但它们在代码中使用统一的结构，这对开发者阅读代码是非常有用的。检查器会注意到这些，但是他们不需要做好每件事，不会自动格式化代码。代码格式化工具可以节约很多时间，解决很多令人头痛的问题。

安装好就要使用 JSFormat ，打开你的 JS 文件，然后在 Windows/Linux 上按下 `Ctrl + Alt + f` 或者在 Mac 上按下 `Ctrl + ⌥ + f`。或者，你也可以使用菜单栏。

你可能会想：“但是如果我不喜欢它格式化 JavaScript 的样式怎么办？”

好消息！JsFormat 是基于 [JS Beautifier settings][21] 高度配置化的。在 SublimeText 3 中 `Preferences -> Package Settings -> JsFormat -> Settings - Default` 可以调整这些配置。

然后可以设置你喜欢的 JSON 格式。

[![JsFormat - Before and After][22]][19]

## 4. [DocBlockr][23]

给代码添加注释有时候真是件痛苦地事情。我几乎不认识任何人会享受添加注释，但是这确实是非常必要的。DocBlockr 通过添加简洁的评论帮助我们减轻了痛苦。安装 DocBlockr 之后，你只需要在一行的开始输入 `/*` 或者 `/**` ，剩下的事情它会为你做好。如果你在一个方法上面使用 `/**`，它会生成 [JSDoc][24] 格式的注释。如果你从没有使用过类似的工具，DocBlockr 会让你觉得以前没有它是如何写代码的。

DocBlockr 也支持许多其他的语言，包括：CoffeeScript、 TypeScript、 PHP、 ActionScript、 Haxe、 Java、 Apex、 Groovy、 Objective C、 C、 C++ 和 Rust。

[![Docblockr demo][25]][23]

## 5. [SideBar Enhancements][26]

客观来说，SublimeText 在侧边文件树的位置只有很少的几个操作选项。简单来说，SideBarEnhancements 解决了这个问题。显然，这个插件提供了为文件和文件夹提供了 “移到垃圾箱” 选项、“用...打开” 选项以及剪贴板相关选项。它还可以让你用浏览器里打开文件，以 `data:uri base64`（可以方便地在CSS中嵌入图片） 格式拷贝文件，提供一系列的搜索操作。它很好地整合了 [SideBarGit][27] 可以在侧边栏直接提供 Git 命令，这是一个额外的功能。

随着 JavaScript 代码库的不断增大，以一种明智的方式浏览你的项目和操纵项目中的文件是很有必要的。因此这个插件必不可少。

[![Sublime Text Sidebar Enhancements][28]][26]

## 6. [AngularJS][29]

由 Angular-UI 团队开发，它可能是所有推荐插件列表上比较大的一个（但是也非常有用）。它的只要特征包括：

 + 自动完成 AngularJS 指令（ng-model, ng-repeat等）
 + 自动完成你的自定义指令
 + 快速查看 directives, controllers 和 filters 的面板
 + Angular 相关的片段
 + AngularJS 核心的指令文档
 + Angular 是一个如此大的库，我发现它非常有用。它有很多设置选项，可以在 [项目首页][30] 浏览。
 + 为了利用这个插件的语法高亮特性，你可以给你的 HTML 文件更改视图类型：`View -> Syntax -> HTML (Angular.js)`

[![AngularJS Logo][31]][29]

## 7. [TypeScript][32]

TypeScript 是 JavaScript 的超集，可以编译成普通的 JavaScript。这可能对普通开发者来说并不重要，除了今年三月的一个小公告，[Angular 2 将基于 typescript 构建][33]。这意味着如果你使用 Angular 并打算以后继续使用 [Angular2][34] ，这个插件必须安装。

这个插件由微软支持，添加了代码自动补全、适当的语法高亮、代码格式化和扩展 TypeScript 项目的导航能力。它还带有一个构建系统允许你将 TypeScript 编译成 JavaScript。

要进行编译，可以去 `Tools -> Build System` 选择 TypeScript。然后打开后缀名为 `.ts` 的文件，选择 `Tools -> Build`，或者按下 `Ctrl + B`。你可能会问构建的参数，之后插件会将编译后的 JavaScript 文件输出在相同的目录下。唯一要说明的就是需要安装 Node。

用插件自己的话说，它提供了“在 Sublime Text 下使用 TypeScript 编程的增强的体验”。这是确实是真的，它是的在前面提到的臃肿的 IDEs 上开发的人员眼前一亮。

[![Screengrab from TypeScript webpage][35]][32]

## 8. [Handlebars][36]

如果你正在使用 [Ember.js][37] 或者仅仅是使用 [Handlebars][38] 作为你的模板语言的选择，那么你绝对不能缺少它。没有它，你可能会把所有的语法高亮去掉。

除了语法高亮（它作用于单独的模板文件以及脚本标签中的内联模板）之外，它还提供了使用 tab 键转换变量为表达式。例如，输入 `x-temp` 并按下 `TAB` 键就会生成：

    <script type="text/x-handlebars" data-template-name=""></script>
    
或者，输入 `ifel` 然后按下 TAB 键，你就会得到：

    {{#if }}
    {{else}}
    {{/if}}>
    
相当的方便，对吗？

在 [项目首页][39] 有一个完整地代码片段列表。

[![Handlebars Logo][40]][36]

## 9. [Better CoffeeScript][41]

Better CoffeeScript 是原始插件 [CoffeeScript-Sublime-Plugin][42] （很不幸似乎它的开发者放弃了它，只在 SublimeText 2 上工作）的一个分支

这个插件提供了 CoffeeScript 开发人员急需的语法高亮功能，而且不止于此。它在 Sublime 里提供了一些命令（通过命令面板或者各种快捷键），比如进行语法检查、编译文件和显示编译后的文件。它还提供了大量的代码片段和可是使用 `cake` 的编译系统（CoffeeScript 中 [Make][43] 的简化版）。

你可以在 [项目首页][44] 仔细浏览插件的更多设置和选项。

[![CoffeeScript Logo][45]][41]

## 10. [jQuery][46]

我知道目前在很多地方 jQuery 看似已经落伍了，但是如果你不是建立一个交互性很强的网站或者你只是想在已有应用上添加功能，它仍然是非常有用的。

这个插件提供了额外的语法高亮功能和几乎所有的 jquery 方法作为代码片段。输入方法名字就可以选择匹配的代码片段 － 就是这么简单！我十分喜欢这个功能，因为它让我免于记忆所有方法的签名和不断查看 jQuery 的 API 文档。

比如，输入 `$.a` 就可以让我选择 `$.ajax()`，然后自动扩展成以下代码：

    $.ajax({
      url: '/path/to/file',
      type: 'default GET (Other values: POST)',
      dataType: 'default: Intelligent Guess (Other values: xml, json, script, or html)',
      data: {param1: 'value1'},
    })
    .done(function() {
      console.log("success");
    })
    .fail(function() {
      console.log("error");
    })
    .always(function() {
      console.log("complete");
    });
    
太棒了！

[![jQuery Logo][47]][46]

## 结语

到此为止，我们已经介绍了 JavaScript 开发必备的十个 Sublime 插件。我希望你可以选择其中一两个尝试一下，并且在评论里让我知道你使用得怎么样。或者我可能没有列举到你最喜欢的插件。请告诉我，我会考虑在这个列表上添上它。

在结束之前，提示一下 Sublime Text 不是免费的。它有无限试用的版本（屏幕时不时会出现的提示），但是价格是 [一个用户 70 美元][48]。如果你一天工作的大部分时间都要用这个文本编辑器工作，我觉得这是一笔非常值得的投资。

  [0]: http://www.sitepoint.com/essential-sublime-text-javascript-plugins/
  [1]: http://www.sublimetext.com/
  [2]: https://en.wikipedia.org/wiki/Integrated_development_environment
  [3]: http://www.vim.org/
  [4]: https://www.gnu.org/software/emacs/
  [5]: https://packagecontrol.io/
  [6]: https://packagecontrol.io/installation
  [7]: https://packagecontrol.io/packages/Babel
  [8]: https://babeljs.io/
  [9]: https://babeljs.io/
  [10]: http://babeljs.io/docs/advanced/caveats/
  [11]: https://packagecontrol.io/packages/Compile%20Selected%20ES6
  [12]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440676460babel.jpg
  [13]: https://packagecontrol.io/packages/JSHint
  [14]: http://jshint.com/about/
  [15]: http://www.sitepoint.com/beginners-guide-node-package-manager/
  [16]: https://packagecontrol.io/packages/JSHint%20Gutter
  [17]: http://jshint.com/
  [18]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440760551js_hint.jpg
  [19]: https://packagecontrol.io/packages/JsFormat
  [20]: https://github.com/beautify-web/js-beautify
  [21]: https://github.com/beautify-web/js-beautify#options
  [22]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440760115js_format_before_after.jpg
  [23]: https://packagecontrol.io/packages/DocBlockr
  [24]: http://usejsdoc.org/
  [25]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440679200docblockr.gif
  [26]: https://packagecontrol.io/packages/SideBarEnhancements
  [27]: https://github.com/titoBouzout/SideBarGit
  [28]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440761156sidebar_enhancements.jpg
  [29]: https://packagecontrol.io/packages/AngularJS
  [30]: https://github.com/angular-ui/AngularJS-sublime-package
  [31]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440764173angular-logo.jpg
  [32]: https://packagecontrol.io/packages/TypeScript
  [33]: http://blogs.msdn.com/b/typescript/archive/2015/03/05/angular-2-0-built-on-typescript.aspx
  [34]: https://angular.io/
  [35]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440761956typescript.jpg
  [36]: https://packagecontrol.io/packages/Handlebars
  [37]: http://www.sitepoint.com/essential-sublime-text-javascript-plugins/
  [38]: http://handlebarsjs.com/
  [39]: https://github.com/daaain/Handlebars
  [40]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440762271handlebars.jpg
  [41]: https://packagecontrol.io/packages/Better%20CoffeeScript
  [42]: https://github.com/Xavura/CoffeeScript-Sublime-Plugin
  [43]: http://www.gnu.org/software/make/
  [44]: http://aponxi.github.io/sublime-better-coffeescript/
  [45]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440762373coffeescript-logo.png
  [46]: https://packagecontrol.io/packages/jQuery
  [47]: http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/08/1440764020jquery-logo.jpg
  [48]: https://www.sublimetext.com/buy