title: "Willin: 贡献文章"
date: 2016-06-02 13:33:12
categories: Notice
tags: [git, hexo]
sticky: true
---
{% cq %}
如何正确地贡献文章
{% endcq %}

## 1. 版本控制

Fork项目： <https://github.com/WulianCC/WulianCC.github.io>

然后`git clone`到本地。

Clone到本地之后，需要添加远程源：

```bash
git remote add wulian git@github.com:WulianCC/WulianCC.github.io.git
```
<!--more-->

和切换到源码分支：

```bash
git checkout source
```

以后每次发表新文章之前，通过：

```bash
git pull wulian source
```

进行本地代码的更新。

Push到自己的项目后，提交一个`Pull Request`，合并完成即自动部署。

## 2.本地调试

```bash
npm i -g hexo-cli
npm install
hexo serve
```

## 3.撰写文章

路径位于：`source/_posts/`目录下。

### 规范

1.文件名： `[日期]-[作者]-[文章标题].md`

全英文小写和数字，所有连接用 `-` 表示，禁止使用中文和其他符号如 `_`。

示例：

```
2016-06-03-author-my-post-title.md
```

2.内容： `Markdown`

推荐使用英文撰写，或英文撰写提供中文翻译。

2.1摘要分隔

```
<!--more-->
```

2.2居中引用

```
{% cq %}
如何正确地贡献文章
{% endcq %}
```

其他特性自己挖掘，参考 `Markdown` 语法 和 `Hexo` 语法。
