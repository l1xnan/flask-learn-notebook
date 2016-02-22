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

## 快捷键
- `Ctrl+Shift+D`: 复制当前行
- `Ctrl+D`: 选择文档中与当前所选的单词相同的下一个单词
- `Ctrl+M`: 跳到光标下的括号所匹配的括号。如果没有，就跳到最近的后括号。
