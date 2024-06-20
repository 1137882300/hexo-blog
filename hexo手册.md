# 官网：https://hexo.io/zh-cn/docs/variables

Hexo 为主题提供了一个变量 site，这个变量包括以下几个成员： site.posts博客里的所有文章列表

site.pages所有创建的页面的列表

site.categories分类列表

site.tags标签列表

其中， site.posts与 site.pages两个结构是相同的，它们各自包括两个成员，一个 length 为长度，一个 data
为具体的数组，它是个对象，但索引是数组，成员是各个文章的详情。

is_tag()

is_category()

is_month()存档页中的按月份文章列表页

is_year()存档页中的按年份文章列表页

is_archive()

is_post()

is_home()

is_current()传入一个 URL ，判断是否本页就是，比如，链接中要对当前页面的链接做加强显示，就可以利用这个方法来添加对应的 class

```text
动态相册：
https://meuicat.com/blog/84/
```

```text
hexo 约定点：
site.data.link ：指的是 /source/_data/link.yml 文件

添加页面有两点：
1. /source 目录下文件夹需要创建
2. themes/anzhiyu/layout/page.pug 需要新增类型
```

```text 
.pug 语法：

!= 是 Pug 的特殊语法，用于输出变量的值。
partial 是 Pug 提供的一个函数，用于包含并渲染其他模板文件。
在.pug 不能用驼峰命名，要用下划线
在Pug中， - 字符用于指示JavaScript代码块。它被称为“代码块”或“JavaScript块”。当您在一行前面加上 - 时，它告诉Pug将代码作为JavaScript执行，而不是将其视为模板指令。
- console.log("---------------") 是在 hexo g 的时候才打印

```

```shell
名称：罗布斯
链接：https://blog.crab888.cloudns.org/
头像：https://blog.crab888.cloudns.org/img/favicon2.png
描述：永远不要相信苦难是值得的，苦难就是苦难，它不会带来成功，也不值得追求，磨炼意志是因为苦难无法逃避。

名称：罗布斯
链接：https://blog.funning.top
头像：https://blog.funning.top/img/favicon2.png
描述：永远不要相信苦难是值得的，苦难就是苦难，它不会带来成功，也不值得追求，磨炼意志是因为苦难无法逃避。
```

```js
let durl = i.url ?? false
// 如果 i.url 的值不为 null 或 undefined，则将 durl 的值设置为 i.url。
// 否则，将 durl 的值设置为 false。
```

# hexo 中文网

https://wizardforcel.gitbooks.io/hexo-doc/content/47.html

```text
hexo.extend.tag.register 是 Hexo 中用于注册标签插件的方法。它允许开发者在文章中快速插入自定义内容1。具体来说，这个方法接受三个参数：
```