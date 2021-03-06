---
layout: post
title:  "Markdown 语法手册"
date:   2016-03-25 13:52:05 +0800
categories: note
---


### 一. 简单功能


| 功能 | 效果 | Markdown代码 | 备注 |
|--|--|--|--|
| 粗体 | **粗体** |  `**粗体**`  |  两边加** |
| 斜体 | *斜体* | `*斜体*` | 两边加* |
| 中划线 | <S>中划线</S> | `<S>中划线</S>` | html标签\<S\> |
| 单行代码 | `Log.i("Hello World!")` | \`Log.i("Hello World!")\` | 两边加`` |
| 插入图片 | ![Image](http://www.trinea.cn/rss.png)| `![Image](http://www.trinea.cn/rss.png)` | [] 中间为占位符,() 中间为图片链接 |
| 链接 | [Visit Github](http://www.github.com) | `[Visit Github](http://www.github.com)` | [] 中间为显示文字,() 中间为链接 |


### 二. 其他功能

#### 1. 换行

在需要换行的地方敲击两次空格和一个回车键即可，如:

```
这是第一行(空格)(空格)(回车)
这是第二行
```

Tip: 多条新行都会被视为一行

#### 2. 标题

Markdown 提供了六种规格的标题，分别对应 Html 标签中的`<h1>`-`<h6>`，通过添加不同数量的`#`字符可以实现不同大小的标题，如:

```
# 最大的标题(相当于一个<h1>标签)

## 次大的标题(相当于一个<h2>标签)

...

###### 最小的标题(相当于一个<h6>标签)

```

Tip: \# 越多，标题越小

#### 3. 列表   

#### 3.1 有序列表

数字 + . + 空格即可，以下代码：

```
1. 项目1  
2. 项目2  
3. 项目3  
```  

效果为：  
1. 项目1  
2. 项目2  
3. 项目3  

#### 3.2 无序列表  

以 * 和空格开头即可，以下代码:

```
* 项目1
* 项目2
* 项目3
```  

效果为：

* 项目1
* 项目2
* 项目3

#### 3.3 子项目表示：

子项目缩进一个 tab 并加 * 和 空格表示，以下代码：  

```
* 项目1
    * 子项目1
    * 子项目2
* 项目2
    * 子项目1
```  

效果为：

* 项目1
    * 子项目1
    * 子项目2
* 项目2
    * 子项目1

#### 4. 代码块

以 \`\`\` 和 \`\`\` 包含，以下代码：  
<pre><code>
```
def hello():
    print 'Hello World!'
```
</code></pre>
效果为：  

```
def hello():
    print 'Hello World!'
```

GFM 扩展了 Markdown 的代码块功能，通过在上面第一个 \`\`\` 后添加语言名称，使不同语言展现不同的代码高亮风格，以下代码:  
<pre><code>
```java
public static void main(String[] args){
    System.out.println("Hello World!");
}
```
</code></pre>
效果为：

```java
public static void main(String[] args){
    System.out.println("Hello World!");
}
```

以下代码：  
<pre><code>

```python
def hello():
    print 'Hello World!'
```

</code></pre>

效果为：  

```python
def hello():
    print 'Hello World!'
```

#### 5. 表格

标准的 Markdown 中并不支持表格，但 GFM 可以，表头前空一行，以 `---`  做为表头分隔，以 `|` 做为列分隔，以下代码：

```
表头1|表头2
---|---
单元格1|单元格2
单元格3|单元格4
```

效果为：  

表头1|表头2
---|---
单元格1|单元格2
单元格3|单元格4

而且还可以通过在表头分隔符中添加 `:` 来决定单元格的对齐方向，以下代码：  

```
表头1|表头2|表头3
:--|:--:|--:
左1|中1|右1
左2|中2|右2
```

效果为：  

表头1|表头2|表头3
:--|:--:|--:
左1|中1|右1
左2|中2|右2

#### 6. 特殊字符转义

如果需要在正文里使用特殊字符的话，可以用 `\` 来转义  
最后 GFW 支持 Html 标签

> 注意 ： 表格例子中使用了css样式：

* css/main.scss

```
table {
  border-collapse: collapse;
  border-spacing: 0;
  width:100%;
  padding-bottom: 16px;
}

table, th, td {
  font-size:1em;
  border:1px solid black;
  padding:3px 7px 2px 7px;
}

table th {
  font-size:1.1em;
  text-align:left;
  padding-top:5px;
  padding-bottom:4px;
  background-color:#808080;
  color:#ffffff;
}

table td {
  color:#000000;
  background-color:white;
}
```
