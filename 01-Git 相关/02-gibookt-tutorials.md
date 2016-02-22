# Gitbook 教程
## 安装

```
npm install gitbook-cli -g
```

## 初始化
初始化书籍目录：

```
gitbook init
```

此命令会在目录下创建以下文件

```
bookname/
├── README.md
└── SUMMARY.md
```

这两个文件都为必须文件。其中 `README.md` 是对书籍的简单介绍；`SUMMARY.md` 是对书籍目录结构的描述。内容结构大致如下：

```
$ cat bookname/SUMMARY.md
# SUMMARY

* [Chapter1](chapter1/README.md)
    * [Section1.1](chapter1/section1.1.md)
    * [Section1.2](chapter1/section1.2.md)
* [Chapter2](chapter2/README.md)
```

其中如 `chapter-n/section x.y.md` 并不表示文件路径上的从属，而表示仅表示文档结构上的从属。

## 插件
`mathjax` 总是安装不成功，用 `katex` 代替。（注：`mathjax-beta` 可以安装成功。）

```
{
    "plugins": ["disqus"],
    "pluginsConfig": {
        "disqus": {
            "shortName": "introducetogitbook"
        }
}
```

配置好后需要运行命令：

```
gitbook install
```

其他插件：
- yahei: 以微软雅黑字体显示，上传收 Gitbook 不能解析
- baidu: 使用百度统计
- word-count: 统计字数
- maxiang: 基于www.maxiang.info(马克飞象) 站点的 gitbook 主题插件
- search-pro: 支持中文搜索
- mcqx: 使用选择题
- fbqx: 使用填空题
- spoiler: 隐藏答案，当鼠标划过时才显示。
- sectionx: 分离各个段落，并提供一个展开收起的按钮
- ace: 插入代码高亮编辑器
- atoc: 插入 TOC 目录
- prism: 基于 Prism 的代码高亮
- mermaid: 使用流程图
- latex-codecogs: 使用数学方程式。
- katex: 数学公式
- sitemap: 生成站点地图。
- disqus: 添加 disqus 评论插件。
- chart: 使用 C3.js 图表。
- github: 在右上角显示 github 仓库的图标链接。
- splitter: 在左侧目录和右侧内容之间添加一个可以拖拽的栏，用来调整两边的宽度。
- anchor-navigation: 锚点导航。
- include-codeblock: 通过引用文件插入代码。
- tbfed-pagefooter: 自定义页脚，显示版权和最后修订时间。
- ga: 添加 Google 统计代码。
- anchors: 标题带有 github 样式的锚点。
- ad: 在每个页面顶部和底部添加广告或任何自定义内容。
- editlink: 内容顶部显示 编辑本页 链接。
- 参见：
- [Gitbook 的使用和常用插件][1]
- [GitBook Developers][2]
- [官网：GitBook Plugins][3]

## 编译书籍

```
gitbook build
```

## 编译和预览书籍

```
gitbook serve
```

可以用浏览器打开 [http://127.0.0.1:4000](http://127.0.0.1:4000) 查看书籍的效果。 此时编辑更改 `*md` 文件，可是实时的显示在浏览器上

[1]: http://zhaoda.net/2015/11/09/gitbook-plugins/
[2]: https://developer.gitbook.com/plugins/index.html
[3]: https://plugins.gitbook.com/
