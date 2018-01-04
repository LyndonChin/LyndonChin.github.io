---
title: Hexo 踩坑记
date: 2016-01-25 13:54:00
tags: Hexo
---

备份
---

除了通过 `hexo generate` 生成 **github pages** 之外，还需要备份整个博客。

分支 `master` 用于发布 **public** 文件夹下的内容，`hexo` 用于备份整个博客（不包括 **public**）

用一个简单的脚本实现备份和发布：

```bash
git add --all
git commit -m'update'
git push

hexo g
hexo d
```

添加 disquse
---

```bash
hexo config disqus_shortname liangfeizc
hexo --config themes/light/_config.yml config comment_provider disqus
```

添加 About
---

0. 生成一个 page
    ```bash
    hexo new page about
    ```
0. 编辑 `source/about/index.md`
0. 修改 `themes/light/_config.yml`，添加链接
    ```yml
    menu:
      Home: null
      Archives: archives
      About: about
    ```

Header 添加链接
---

修改 `themes/light/layout/_partial/header.ejs`

```html
<nav id="main-nav" class="alignright">
  <ul>
    <% for (var i in theme.menu){ %>
      <li><a href="<%- config.root %><%- theme.menu[i] %>"><%= i %></a></li>
    <% } %>
    <li><a href="https://github.com/lyndonchin">Github</a></li>
  </ul>
  <div class="clearfix"></div>
</nav>
```
