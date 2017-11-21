# 如何在代码中高亮代码

因为在官方网页转 Markdown 时，偶尔会出现这样一种情况。

![image](https://user-images.githubusercontent.com/7054676/32837301-f9915d62-ca47-11e7-9e9b-a17bae5259e4.png)

而它渲染出来的效果是这样

![image](https://user-images.githubusercontent.com/7054676/32837327-14f51026-ca48-11e7-8db9-fb7739630b33.png)

是不是奇丑无比？

于是我们首先需要去对应官方网页上获取源代码（或者手动格式化一下）然后利用，Github Markdown 所支持的代码高亮语法来让界面更美观。

语法具体如下

![image](https://user-images.githubusercontent.com/7054676/32837389-56e8cc52-ca48-11e7-835a-46103a05bd31.png)

在最开始的 `~~~Java` 是指明接下来的代码块以什么语言的格式进行高亮，这里的代码类型可以是 Python 、 XML ，或者其余语言。然后在写完代码后，再用一组 `~~~` 完成这个代码块的封闭，于是我们最终便可以得到一个非常良好的代码渲染效果了。

比如之前的代码，最终渲染出来的效果是这样的。

![image](https://user-images.githubusercontent.com/7054676/32837530-dd01113c-ca48-11e7-91d7-6f86235b3ea8.png)

是不是美观了许多？