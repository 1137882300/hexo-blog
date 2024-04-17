
# 官网：https://hexo.io/zh-cn/docs/variables

Hexo 为主题提供了一个变量 site，这个变量包括以下几个成员： site.posts博客里的所有文章列表

site.pages所有创建的页面的列表

site.categories分类列表

site.tags标签列表

其中， site.posts与 site.pages两个结构是相同的，它们各自包括两个成员，一个 length 为长度，一个 data 为具体的数组，它是个对象，但索引是数组，成员是各个文章的详情。

is_tag()

is_category()

is_month()存档页中的按月份文章列表页

is_year()存档页中的按年份文章列表页

is_archive()

is_post()

is_home()

is_current()传入一个 URL ，判断是否本页就是，比如，链接中要对当前页面的链接做加强显示，就可以利用这个方法来添加对应的 class