# Git 相关

1. 简明教程：http://rogerdudler.github.io/git-guide/index.zh.html
2. 详细教程：http://iissnan.com/progit/

## `.gitignore` 文件
Windows 下不让手动建立此类文件，可用以下命令新建：

    cd .>.gitignore
    
内容如下：
```
# Node rules: Grunt intermediate storage
.grunt
# Dependency directory
node_modules
# Book build output
_book
# eBook build output
*.epub
*.mobi
*.pdf
# 其他文件
Private.md
```

## 警告
遇到以下警告：

    warning: LF will be replaced by CRLF in xxxx
 Windows 使用回车和换行两个字符来结束一行，而 Mac 和 Linux 只使用换行一个字符。Git 可以在你提交时自动地把行结束符 CRLF 转换成 LF，而在签出代码时把 LF 转换成 CRLF。用core.autocrlf来打开此项功能，如果是在 Windows 系统上，把它设置成true，这样当签出代码时，LF 会被转换成 CRLF，可做如下更改：
 
    $ git config --global core.autocrlf false

# npm 相关

## 更改镜像
npm 全称 Node Package Manager，是 node.js 的模块依赖管理工具。由于 npm 的源在国外，所以国内用户使用起来各种不方便。可以将其更改为淘宝的 npm 镜像 `https://registry.npm.taobao.org`：
1. 临时使用
 ```   
    npm --registry https://registry.npm.taobao.org install express
 ```
2. 持久使用
```
    npm config set registry=" https://registry.npm.taobao.org"

    // 配置后可通过下面方式来验证是否成功
    npm config get registry
    // 或
    npm info express
``` 

## 安装模块

    npm install xxx         # 在当前目录下安装模块

    npm install xxx -g      # 在全局环境下安装模块

# Visual Studio Code 相关
自动换行及字体配置：
```
{
    "editor.fontFamily": "YaHei Consolas Hybrid",
    "editor.wrappingColumn": 100
}
```
# Gitbook 相关

## 安装
    
    npm install gitbook-cli -g
## 初始化
初始化书籍目录：

    gitbook init
此命令会在目录下创建以下文件

    bookname/
    ├── README.md
    └── SUMMARY.md

这两个文件都为必须文件。其中 `README.md` 是对书籍的简单介绍；`SUMMARY.md` 是对书籍目录结构的描述。内容结构大致如下：

    $ cat bookname/SUMMARY.md
    # SUMMARY

    * [Chapter1](chapter1/README.md)
        * [Section1.1](chapter1/section1.1.md)
        * [Section1.2](chapter1/section1.2.md)
    * [Chapter2](chapter2/README.md)    
其中如 `chapter-n/section x.y.md` 并不表示文件路径上的从属，而表示仅表示文档结构上的从属。


## 插件
`mathjax` 总是安装不成功，用 `katex` 代替。（注：`mathjax-beta` 可以安装成功。）
```
{
    "plugins": ["katex","disqus"],
    "pluginsConfig": {
        "disqus": {
            "shortName": "introducetogitbook"
        }
}
```
配置好后需要运行命令：
    
    gitbook install

其他插件：
gitbook-plugin-yahei: 以微软雅黑字体显示
gitbook-plugin-chart: Using C3.js or Highcharts chart library in Gitbook.
gitbook-plugin-baidu: A gitbook plugin to add Baidu Analytics for your book
gitbook-plugin-word-count: A word counting plugin for Gitbook
gitbook-plugin-maxiang: 基于www.maxiang.info(马克飞象) 站点的 gitbook 主题插件
gitbook-plugin-search-pro: 支持中文搜索
gitbook-plugin-addcssjs: Adds external CSS and JS files to the gitbook

测试：$$\sum i = 5050$$

## 编译书籍

    gitbook build


## 编译和预览书籍
    
    gitbook serve
可以用浏览器打开 http://127.0.0.1:4000 查看书籍的效果。
此时编辑更改 `*md` 文件，可是实时的显示在浏览器上