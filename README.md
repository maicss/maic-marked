# maic-marked

This program is forked from [chjj/marked](https://github.com/chjj/marked).

I modify and add some functions, to cover my blog needs.

## 注意⚠️

`marked.min.js`还是作者原来的版本，~~我没有对自己的代码进行压缩。等之后再弄。~~ gulp不支持ES6的压缩，等我写完这个重写的时候，用webpack构建的时候再压缩吧。

## 修改的地方
 - 删除了一些兼容非ES5的语法什么的。可能对IE浏览器的支持不好，在乎兼容性的请不要使用这个。
 - 支持 `tags`, 格式: `tags: ` 多个标签必须用空格分开。
 - 支持 `date`, 格式: `date: YYYY-MM-DD HH:mm`
 - 支持 `toc`, 格式: `[toc]`
 - 返回数据变化了。作者原来的库只返回了渲染后的HTML字符串，现在返回的是一个对象`tags: [], date: '', html: '', abstract:'', title '', more: '''`
 - 支持Hexo类似的more，格式：`<!--more-->`, 在任意地方添加这个标签，会使渲染结构变成两个部分，完整的markdown文件会赋值给`HTML`，`<!--more-->`之前的部分会赋值给`more`。必须在[toc]标签后面使用
 - 支持摘要，格式：`<abstract><abstract>`，作为文章摘要。注意标签不是闭合的，为了跟原来的HTML标签判断方式区分开来。替换的方式是把这个标签删掉之后把内容拿出来，一个是给`res.absrtact`，一个是加上`blockquote`添加到文档中
 - ~~妈蛋，简直就是把hexo实现了一遍，好气啊~~
 
## depends

- [highlight.js](https://github.com/isagalaev/highlight.js) 但是还是需要在外面setOption中添加highlight属性。

## 修改的方法

**1，** 添加一个想要渲染的标签，分四步。1⃣️先添加对应的正则表达式，分为两种，一个是块元素一个是行间元素，2⃣️在对应的词法分析中添加类型和裁剪文档，分别是`Lexer`和`InlineLexer`，3⃣️添加解析函数，就是`Renderer`了，4⃣️最后就是渲染了，就是`Parser`。

**2，** 需要注意的是，自己添加的方法在词法中尽量放在前面，因为为了保证MD文件被全部解析，后面有个兜底的正则，写在这个后面的就解析不到了。

**3，** 我添加的`date`和`tags`都是常规的添加，但是`toc`就又点不一样了。~~这个写的又点丑，有时间再改改~~

**4，** 标题的添加方法又两种，分别是[Setext](http://docutils.sourceforge.net/mirror/setext.html)和[atx](http://www.aaronsw.com/2002/atx/)，简单点来说就是第一个是在标题下面添加-和=来区分，只能区分一级和二级标题，第二个在标题前面添加#，按#的个数来区分，又6个级别，详细的你们自己再去了解一下吧。我直接禁用掉了第一种～～～，你们有需要的话自己再修改，源码里的`heading`是`atx`，`lheading`是`Setext`。

**5，** **2017-04-05 0.3.18** 版本修改：用ES6的class重写了一遍，也没改动什么源码，只是更改了格式。现在new的时候，直接带上options即可，没有了`setOptions`方法。增加了一个exec方法(我也不知道叫什么名字好，因为`render`和`parser`里面的函数都有这个名字了)。添加了`highlight.js`的依赖，直接写在了源码和`package.json`里面。综上，渲染的时候，用`new marked(options).exec(markdownText)`这个方法就好了。

**6，** **2017-08-31 0.3.20** 版本修改：添加了`abstract`，另外把自己添加的所有东西都独立了出来，不再往原来的HTML结构中添加东西。这样更方便外面控制和渲染。

## todo

等写线上编辑器的时候，把highlight集成进来，就不用现在的模块加载方式了。

再添加一点样式，变得更好看一点。

