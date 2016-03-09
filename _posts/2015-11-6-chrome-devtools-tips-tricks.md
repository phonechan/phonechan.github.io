---
layout: post
title:  "Chrome 开发者工具使用技巧"
date:   2015-11-6 00:00:00 +0800
categories: JavaScript
---

翻译，原文链接 [http://mo.github.io/2015/10/19/chrome-devtools.html][0]

# Chrome 开发者工具使用技巧

最近我用 Chrome 开发者工具的时间比平时多了不少。我发现了几个不错的功能，之前忽略了它们（或者至少没有足够的深入探究；比如，blackboxing 和异步堆栈跟踪）。基于以上原因，我想总结一下这款开发工具中我喜欢的几个特性。

 - 这个小的放大镜图标显示了是在哪个 CSS 文件中的哪个 CSS 类/选择器最终决定了这个特定元素的样式和 CSS 属性。比如，可以在任意 DOM 元素中选择  “inspect” ，然后选择右边的 “Computed” 子菜单。找到你想要选择的 CSS 属性，单击放大镜图标直接显示出在哪一个 .css 文件中的 CSS 类/选择器规定了该属性（如果你刚开始接触一个大型的 web 应用，这个特性非常有用）：

![screenshot][1]

 - 想要查看 web 应用发送的 XHRs，你可以在 “Settings” 下勾选 “Log XMLHttpRequests” ，然后打开控制台页面就可以查看了。在我知道这一点之前，我需要设置我的浏览器通过类似 Burp 组件的 HTTP 拦截代理，但是如果你只需要一个快速的预览，现在这种方式就方便多了。当然，如果使用拦截代理，你就有机会在 XHRs 发送到服务器之前去修改它，这对于安全测试是非常有用的。一个轻量级的替换方案是去查看 “Sources :: XHR Breakpoints” ，并且选中激活 “Any XHR” 为断点。

![screenshot][2]

 - 现在，假设你的 web 应用正在定时请求 XHR （比如，保持当前视图是最新的），你想知道这个定时器是在哪儿设置的（即在哪里调用了 `settimeout()` 或 `setinterval()` ）。你可以切换到 “Sources” 标签页，并选中 “Async” 复选框，就能够看到结果了。这将使所有的堆栈路径不断深入 `settimeout()` 和同层方法，甚至多层深度的调用。同样的，`requestAnimationFrame()` 和 `addEventListener()` 等方法也有类似的选项。你可以在这里找到那个复选框：

![screenshot][3]

 - 如果你想单击了某个按钮或者链接后快速定位正在运行的代码，可以在单击这个按钮之前选中 “Event listener breakpoint” 下的 Mouse（鼠标）下的 Click （单击）事件（当你开始接触一个大型 web 应用时的另一个杀手锏）：

 ![screenshot][4]

 - 当你使用 “Event listener breakpoint :: Mouse :: Click” 功能时，可能一开始会跳转到想 jQuery 这样的第三方库代码中，所以你不得不在调试器中跳过几步，才能找到 “真正” 的处理事件的代码。为了避免这个问题，可以把第三方的脚本放到 “黑匣子” 里面去。调试器不会在运行黑匣子里的代码时停下来，它会运行下去直到遇到不在黑匣子里的代码才中断。右击第三方库的文件名，在弹出菜单中选中 “Blackbox Script” ，就可以把脚本放入黑匣子中了：

![screenshot][5]

 - 使用快捷键 `ctrl-p`，可以根据文件名快速打开文件（和在 atom 中一样）：
 
![screenshot][6]

 - 使用快捷键 `ctrl-shift-p`，可以根据函数名快速定位函数（但是仅限在打开的文件中）：
 
![screenshot][7]

 - 使用快捷键 `ctrl-shift-f` 可以搜索全部文件：
 
![screenshot][8]

 - 你可以选择一个词然后按 `ctrl-d` 几次选择那个词，多个游标同时选中这些词，你可以一起编辑它们（也像在 atom 中一样）。在重命名变量的时候非常有用：
 
![screenshot][9]

 - 当运行一个本地储存的网站，应该可以在工具中编辑文件并将更改直接保存到磁盘。要做到这一点，切换到源选项卡，右击 “Sources” 选项卡，然后选择 “添加文件夹到工作区” ，最后选择你存放项目的本地文件夹。之后，右击你网站上的一些文件的本地副本，并选择 “映射到网络资源…” ，然后选择相应的 “网络” 文件：

![screenshot][10]

一些其他的小技巧：

 - 在控制台输入 `$0` 可以返回你在元素视图中选中的元素。

 - 你可以使用 `$x("//p")` 测试 XPath 表达式（如果你写的 selenium 测试用例或者 CSS 选择器总是不能正确执行，这个就能派上用场了）。

我还要推荐你安装两个 Chrome 扩展程序：

 - [JSONView][11] 能够对 JSON 缩进和语法高亮（甚至允许你展开/折叠块）。它还能使 JSON 中的 URLs 是单击的链接，这可以使得用浏览器打开 JSON 格式里的接口成为可能。比如，在安装之前和之后（更好的格式化）分别浏览 [`http://omahaproxy.appspot.com/all.json`][12] ，还有 [`https://api.github.com/`][13] （可点击的 URLs 使得浏览 API 更加便捷）。
 
 - [JS Error Notifier (non-"spyware" version)][14] 在 Javascript 在控制台打印错误的时候弹出提示。不过，这个插件的主要版本将私人 “使用数据” 提交给一个第三方服务平台（详见 [issue #28][15] 的讨论）。但无论如何，这个扩展插件已经帮助我注意到几个问题并修复了它们。

总的来说我非常喜欢这个开发工具，我能想到的唯一不尽如人意的就是不能自定义快捷键：

 - [Allow to customize keyboard shortcuts/key bindings][16]

  [0]: http://mo.github.io/2015/10/19/chrome-devtools.html
  [1]: http://mo.github.io/assets/devtools-css-magnifier-icon.png
  [2]: http://mo.github.io/assets/devtools-settings-log-xhr.png
  [3]: http://mo.github.io/assets/devtools-async-stacktraces.png
  [4]: http://mo.github.io/assets/devtools-event-listener-breakpoints.png
  [5]: http://mo.github.io/assets/devtools-blackbox-third-party-script.png
  [6]: http://mo.github.io/assets/devtools-open-file-ctrl-o.png
  [7]: http://mo.github.io/assets/devtools-go-to-member.png
  [8]: http://mo.github.io/assets/devtools-search-all-files-ctrl-shift-f.png
  [9]: http://mo.github.io/assets/devtools-multiple-cursors-ctrl-d.gif
  [10]: http://mo.github.io/assets/devtools-workspace-map-network-resource.png
  [11]: https://www.google.se/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CCAQFjAAahUKEwje6JvErs_IAhVI_iwKHSwaALo&url=https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en&usg=AFQjCNH3ET5JyRh_aKGH_G5Ws5MXENK5bA&sig2=JD7IupIQ8cZJwE_05USbwg
  [12]: http://omahaproxy.appspot.com/all.json
  [13]: https://api.github.com/
  [14]: https://chrome.google.com/webstore/detail/javascript-errors-notifie/fhbooopdkjpkogooopbmabepipljagfn
  [15]: https://github.com/barbushin/javascript-errors-notifier/issues/28
  [16]: https://code.google.com/p/chromium/issues/detail?id=174309