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
## Git Push 避免用户名和密码方法

### 方法一

**第一步**：创建文件存储 GIT 用户名和密码
在 %HOMEPATH% 目录中，用 `cd .>.git-credentials` 命令新建 `.git-credentials` 文件，输入内容格式：

    https://{username}:{password}@github.com

**第二步**：添加 Git Config 内容
进入 git bash 终端， 输入如下命令：

    $ git config --global credential.helper store
重新开启 git bash 会发现 git push 时不用再输入用户名和密码    

### 方法二

**第一步**：添加环境变量
在 windows 中添加一个 `HOME` 环境变量，变量名: `HOME`, 变量值：`%USERPROFILE%`。

**第二步**：创建 git 用户名和密码存储文件
进入 `%HOME%` 目录，新建一个名为 `_netrc` 的文件，文件中内容格式如下：
```
machine github.com
login your-username
password your-password
```
重新打开 git bash 即可，无需再输入用户名和密码。

### 方法三

**第一步**：进入 git bash 终端， 输入如下命令：

    $ ssh-keygen -t rsa -C "youremail@example.com"
你需要把 `youremail@example.com` 换成你自己的邮件地址，然后一路回车，使用默认值即可，无需再设置密码。
如果一切顺利的话，可以在用户主目录里找到 `.ssh` 目录，里面有 `id_rsa` 和 `id_rsa.pub` 两个文件，这两个就是 SSH Key 的秘钥对，`id_rsa` 是私钥，不能泄露出去，`id_rsa.pub` 是公钥，可以放心地告诉任何人。

**第二步**：登陆 GitHub，打开 “Account settings”，“SSH Keys” 页面：
然后，点 `Add SSH Key`，填上任意 Title，在 Key 文本框里粘贴 `id_rsa.pub` 文件的内容，然后保存。

**第三步**：在本地的learngit仓库下运行命令：

    $ git remote add origin git@github.com:username/your-repository-name.git
添加后，远程库的名字就是 origin，这是 Git 默认的叫法，也可以改成别的，但是 origin 这个名字一看就知道是远程库。

**第四步**：把本地库的所有内容推送到远程库上：

    $ git push -u origin master


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
    "plugins": ["disqus"],
    "pluginsConfig": {
        "disqus": {
            "shortName": "introducetogitbook"
        }
}
```
配置好后需要运行命令：
    
    gitbook install

其他插件：

* yahei: 以微软雅黑字体显示，上传收 Gitbook 不能解析
* baidu: 使用百度统计
* word-count: 统计字数
* maxiang: 基于www.maxiang.info(马克飞象) 站点的 gitbook 主题插件
* search-pro: 支持中文搜索
* mcqx: 使用选择题
* fbqx: 使用填空题
* spoiler: 隐藏答案，当鼠标划过时才显示。
* sectionx: 分离各个段落，并提供一个展开收起的按钮
* ace: 插入代码高亮编辑器
* atoc: 插入 TOC 目录
* prism: 基于 Prism 的代码高亮
* mermaid: 使用流程图
* latex-codecogs: 使用数学方程式。
* katex: 数学公式
* sitemap: 生成站点地图。
* disqus: 添加 disqus 评论插件。
* chart: 使用 C3.js 图表。
* github: 在右上角显示 github 仓库的图标链接。
* splitter: 在左侧目录和右侧内容之间添加一个可以拖拽的栏，用来调整两边的宽度。
* anchor-navigation: 锚点导航。
* include-codeblock: 通过引用文件插入代码。
* tbfed-pagefooter: 自定义页脚，显示版权和最后修订时间。
* ga: 添加 Google 统计代码。
* anchors: 标题带有 github 样式的锚点。
* ad: 在每个页面顶部和底部添加广告或任何自定义内容。
* editlink: 内容顶部显示 编辑本页 链接。
参见：
1. [Gitbook 的使用和常用插件][1]
2. [GitBook Developers][2]
3. [官网：GitBook Plugins][3]

[1]: http://zhaoda.net/2015/11/09/gitbook-plugins/
[2]: https://developer.gitbook.com/plugins/index.html
[3]: https://plugins.gitbook.com/
测试：$$\sum i = 5050$$

## 编译书籍

    gitbook build


## 编译和预览书籍
    
    gitbook serve
可以用浏览器打开 http://127.0.0.1:4000 查看书籍的效果。
此时编辑更改 `*md` 文件，可是实时的显示在浏览器上