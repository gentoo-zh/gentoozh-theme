# gentoozh-theme

**Gentoo 中文社区官网（[gentoo.org.cn](https://www.gentoo.org.cn/)）的 Hextra 补丁包。**

这**不是**从零写的主题，也**不是** fork——而是在 [Hextra](https://github.com/imfing/hextra) 之上叠加本站定制的一层「补丁包」：用 Hugo Module 的查找顺序，让这里的文件覆盖/扩展 Hextra，同时**仍然跟随上游 Hextra 的更新与 bug 修复**。

## 里面有什么

只放「皮」（表现层），都是本站相对 Hextra 的覆盖或新增：

- **首页 bento 布局** —— `layouts/shortcodes/home-bento.html`（hero + 入口磁贴 + 社区/最新文章/贡献者带）
- **贡献者页** —— `layouts/contributors/{list,single}.html` + `layouts/partials/home-avatar.html`（头像墙）
- **点击复制** —— `layouts/shortcodes/copy.html` + `layouts/_partials/custom/head-end.html`（无脚本可降级）
- **定制 404、footer、navbar 标题** —— `layouts/404.html`、`layouts/_partials/{footer,navbar-title}.html`
- **文章/列表卡片** —— `layouts/_default/list.html`、`layouts/blog/list.html`
- **Gentoo 品牌紫等站点样式** —— `assets/css/custom.css`（叠在 Hextra 编译产物之上）
- **简繁 UI 字串覆盖** —— `i18n/zh-cn.yaml`、`i18n/zh-tw.yaml`

> 站点**内容**（`content/`，含 sync 生成的繁体）、配置（菜单/语言/params）、`sync_to_tw.sh`、贡献者抓取脚本等都留在站点仓库——那些不是「皮」。

## 怎么用

依赖链：**站点 → gentoozh-theme → Hextra**（前者覆盖后者）。站点的 `hugo.toml`：

```toml
[module]
  [[module.imports]]
    path = "github.com/Gentoo-zh/gentoozh-theme"
```

## 升级上游 Hextra

在**本仓库**做（不在站点）：

```bash
hugo mod get -u github.com/imfing/hextra
hugo mod tidy
git commit -am "bump hextra"
git tag vX.Y.Z          # 打个新版本
```

然后到站点仓库把本模块 pin 到新版本（`hugo mod get github.com/Gentoo-zh/gentoozh-theme@vX.Y.Z`）。

## 本地开发

在站点 `go.mod` 加 `replace`，指向本仓库工作副本，免发版即可联调：

```
replace github.com/Gentoo-zh/gentoozh-theme => ../gentoozh-theme
```

## 升级 Hextra 时要复查的「影子覆盖」文件

这几个文件**复制并改写了 Hextra 同名文件**，升级 Hextra 后建议 diff 一下上游是否有变动：
`layouts/404.html`、`layouts/_default/list.html`、`layouts/blog/list.html`、`layouts/_partials/footer.html`、`layouts/_partials/navbar-title.html`。
其余（`home-bento`、`copy`、`home-avatar`、`contributors/*`）是全新增、`custom/head-end` 与 `custom.css` 走官方扩展点，均不与上游冲突。

## 许可

代码 [MIT](LICENSE)。
