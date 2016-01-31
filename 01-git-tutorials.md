## Git 相关
### `.gitignore` 文件
Windows 下不让手动建立此类文件，可用以下命令新建：

    cd .>.gitignore
    
内容如下：
```
# Node rules: Grunt intermediate storage
.grunt

## Dependency directory
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

### 警告
遇到一下警告：

    warning: LF will be replaced by CRLF in xxxx
 Windows 使用回车和换行两个字符来结束一行，而 Mac 和 Linux 只使用换行一个字符。Git 可以在你提交时自动地把行结束符 CRLF 转换成 LF，而在签出代码时把 LF 转换成 CRLF。用core.autocrlf来打开此项功能，如果是在 Windows 系统上，把它设置成true，这样当签出代码时，LF 会被转换成 CRLF，可做如下更改：
 
    $ git config --global core.autocrlf false

## gitbook 

### 安装
    
    npm install gitbook-cli -g
### 初始化
初始化书籍目录：

    gitbook init
此命令会在目录下创建以下文件

    bookname/
    ├── README.md
    └── SUMMARY.md

这两个文件都为必须文件。其中 `README.md` 是对书籍的简单介绍；`SUMMARY.md` 是对书籍目录结构的描述。内容结构如下：

    $ cat book/SUMMARY.md 
    # SUMMARY

    * [Chapter1](chapter1/README.md)
        * [Section1.1](chapter1/section1.1.md)
        * [Section1.2](chapter1/section1.2.md)
    * [Chapter2](chapter2/README.md)    

### 编辑书籍

    gitbook build


### 编译和预览书籍
    
    gitbook serve
可以用浏览器打开 http://127.0.0.1:4000 查看书籍的效果。