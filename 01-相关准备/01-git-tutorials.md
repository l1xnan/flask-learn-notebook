# Git 相关
1. [简明教程](http://rogerdudler.github.io/git-guide/index.zh.html)
2. [详细教程](http://iissnan.com/progit/)

## `.gitignore` 文件
Windows 下不让手动建立此类文件，可用以下命令新建：

```
cd .>.gitignore
```

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
**第一步**：创建文件存储 GIT 用户名和密码 在 %HOMEPATH% 目录中，用 `cd .>.git-credentials` 命令新建 `.git-credentials` 文件，输入内容格式：

```
https://{username}:{password}@github.com
```

**第二步**：添加 Git Config 内容 进入 git bash 终端， 输入如下命令：

```
$ git config --global credential.helper store
```

重新开启 git bash 会发现 git push 时不用再输入用户名和密码    

### 方法二
**第一步**：添加环境变量 在 windows 中添加一个 `HOME` 环境变量，变量名: `HOME`, 变量值：`%USERPROFILE%`。

**第二步**：创建 git 用户名和密码存储文件 进入 `%HOME%` 目录，新建一个名为 `_netrc` 的文件，文件中内容格式如下：

```
machine github.com
login your-username
password your-password
```

重新打开 git bash 即可，无需再输入用户名和密码。

### 方法三
**第一步**：进入 git bash 终端， 输入如下命令：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把 `youremail@example.com` 换成你自己的邮件地址，然后一路回车，使用默认值即可，无需再设置密码。 如果一切顺利的话，可以在用户主目录里找到 `.ssh` 目录，里面有 `id_rsa` 和 `id_rsa.pub` 两个文件，这两个就是 SSH Key 的秘钥对，`id_rsa` 是私钥，不能泄露出去，`id_rsa.pub` 是公钥，可以放心地告诉任何人。

**第二步**：登陆 GitHub，打开 "Account settings"，"SSH Keys" 页面： 然后，点 `Add SSH Key`，填上任意 Title，在 Key 文本框里粘贴 `id_rsa.pub` 文件的内容，然后保存。

**第三步**：在本地的learngit仓库下运行命令：

```
$ git remote add origin git@github.com:username/your-repository-name.git
```

添加后，远程库的名字就是 origin，这是 Git 默认的叫法，也可以改成别的，但是 origin 这个名字一看就知道是远程库。

**第四步**：把本地库的所有内容推送到远程库上：

```
$ git push -u origin master
```

## 警告
遇到以下警告：

```
warning: LF will be replaced by CRLF in xxxx
```

 Windows 使用回车和换行两个字符来结束一行，而 Mac 和 Linux 只使用换行一个字符。Git 可以在你提交时自动地把行结束符 CRLF 转换成 LF，而在签出代码时把 LF 转换成 CRLF。用core.autocrlf来打开此项功能，如果是在 Windows 系统上，把它设置成true，这样当签出代码时，LF 会被转换成 CRLF，可做如下更改：

```
$ git config --global core.autocrlf false
```

# npm 相关
## 更改镜像
npm 全称 Node Package Manager，是 node.js 的模块依赖管理工具。由于 npm 的源在国外，所以国内用户使用起来各种不方便。可以将其更改为淘宝的 npm 镜像 `https://registry.npm.taobao.org`：
1. 临时使用
2. npm --registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org) install express
- 持久使用

  ```
   npm config set registry=" https://registry.npm.taobao.org"

   // 配置后可通过下面方式来验证是否成功
   npm config get registry
   // 或
   npm info express
  ```

## 安装模块

```
npm install xxx         # 在当前目录下安装模块

npm install xxx -g      # 在全局环境下安装模块
```

# Visual Studio Code 相关
自动换行及字体配置：

```
{
    "editor.fontFamily": "YaHei Consolas Hybrid",
    "editor.wrappingColumn": 100
}
```

# Atom 相关
## 错误解决
安装包的时候出现错误：

```
gyp info it worked if it ends with ok
gyp info using node-gyp@2.0.2
gyp info using node@0.10.40 | win32 | ia32
gyp http GET https://atom.io/download/atom-shell/v0.34.5/node-v0.34.5.tar.gz
gyp http 404 https://atom.io/download/atom-shell/v0.34.5/node-v0.34.5.tar.gz
gyp
```

解决办法：

```
set ATOM_NODE_URL=http://gh-contractor-zcbenz.s3.amazonaws.com/atom-shell/dist
```

## 插件推荐
安装方法：

```
apm install <package-name>
```

### Markdown 相关推荐：

Name                  | Description
--------------------- | :-----------------
markdown-scroll-sync  | 同步预览
tidy-markdown         | 格式化 Markdown，特别是表格
markdown-preview-puls | 带数学公式预览
markdown-writer       | 全能

### Python 相关推荐

Name                | Description
------------------- | :-------------
python-tools        | 选择特定符号、跳转到定义处等
autocomplete-python | 自动补全等
