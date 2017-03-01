# maic-marked

This program is forked from [chjj/marked](https://github.com/chjj/marked).

I modify and add some functions, to cover my blog needs.

## 修改的地方
 - 删除了一些兼容非ES5的语法什么的。可能对IE浏览器的支持不好，在乎兼容性的请不要使用这个。
 - 支持 `tags`, 格式: `tags: ` (must has a space)
 - 支持 `date`, 格式: `date: YYYY-MM-DD HH:mm`
 - 支持 `toc`, 格式: `[toc]`
 - 返回数据变化了。作者原来的库只返回了渲染后的HTML字符串，现在返回的是一个对象`tags: [], date: '', html: '', abstract:''`
 - 支持 `<!--more-->`, 格式： 在任意地方添加这个标签，会使渲染结构变成两个部分，完整的markdown文件会赋值给`HTML`，`<!--more-->`之前的部分会赋值给`abstract`
 - ~~妈蛋，简直就是把hexo实现了一遍，好气啊~~
 
## depends

- [highlight.js](https://github.com/isagalaev/highlight.js) 但是还是需要在外面setOption中添加highlight属性。
- [semantic-ui](http://semantic-ui.com/), **注意⚠️：**这个并没有在`package.json`中表明依赖，因为这个只需要在网页中引用即可，跟项目没啥关系，没必要每次`npm install`的时候安装全部源码什么的。

## 修改的方法

**1，** 所有要使用样式的（我用的semantic），我直接在渲染函数里面写了class，直接输出了我想要的，如果你想改成你自己的，直接在js里面搜索class就能找到了。

**2，** 添加一个想要渲染的标签，分四步。1⃣️先添加对应的正则表达式，分为两种，一个是块元素一个是行间元素，2⃣️在对应的词法分析中添加类型和裁剪文档，分别是`Lexer`和`InlineLexer`，3⃣️添加解析函数，就是`Renderer`了，4⃣️最后就是渲染了，就是`Parser`。

**3，** 需要注意的是，自己添加的方法在词法中尽量放在前面，因为为了保证MD文件被全部解析，后面有个兜底的正则，写在这个后面的就解析不到了。

**4，** 我添加的`date`和`tags`都是常规的添加，但是`toc`就又点不一样了。~~这个写的又点丑，有时间再改改~~

**5，** 标题的添加方法又两种，分别是[Setext](http://docutils.sourceforge.net/mirror/setext.html)和[atx](http://www.aaronsw.com/2002/atx/)，简单点来说就是第一个是在标题下面添加-和=来区分，只能区分一级和二级标题，第二个在标题前面添加#，按#的个数来区分，又6个级别，详细的你们自己再去了解一下吧。我直接禁用掉了第一种～～～，你们有需要的话自己再修改，源码里的`heading`是`atx`，`lheading`是`Setext`。

## todo

等写线上编辑器的时候，把highlight集成进来，就不用现在的模块加载方式了。

再添加一点样式，变得更好看一点。

