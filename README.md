# BgeWiki.github.io

之前一直在用hexo的博客存笔记，但是初心本来是用来写写日记、发发随想啥的，而且笔记一多就容易难找😡。

想整理笔记啥的还是得方便看，wiki类型感觉还是比较适合知识点的整理分类，目前把笔记都迁移到mkdocs上了，但是也有很多别扭的地方，之后有时间再整理吧



**mkdocs文档**

https://markdown-docs-zh.readthedocs.io/zh_CN/latest/#mkdocs

需要使用python安装相关库。



**启动**

mkdocs serve 

服务器启动后，当配置文件、文档目录或主题发生改变时，服务器就会自动加载变更并生成新的文档。



**创建一个新的 MkDocs 项目**

mkdocs new newproject

其中index.md 是默认的首页，mkdocs.yml是配置文件。



**生成和部署**

mkdocs build

mkdocs gh-deploy

MkDocs将构建您的文档并使用[ghp-import](https://github.com/davisp/ghp-import)工具将它们提交到gh-pages分支，然后将gh-pages分支推送到GitHub。
