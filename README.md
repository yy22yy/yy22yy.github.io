# yy22yy 的个人主页

目标网址：[https://yy22yy.github.io/](https://yy22yy.github.io/)。是否已上线，以 [Pages 设置](https://github.com/yy22yy/yy22yy.github.io/settings/pages)和 [Actions 部署结果](https://github.com/yy22yy/yy22yy.github.io/actions)为准。

## 文件与发布方式

- `index.html`：中文首页及 GitHub 入口。
- `styles.css`：布局、配色与窄屏适配。
- `.nojekyll`：空文件，跳过 Jekyll 处理。
- `README.md`：本维护说明。

这是公开仓库，首页及仓库内容均可公开访问。

Pages 发布来源设置为 **Deploy from a branch → main → /(root)**。HTML/CSS 本身就是发布文件，不需要 Node.js、npm 或 Vite 构建；GitHub 仍会自动运行 Pages 部署任务。

## 更新网站

编辑相应文件后，在仓库目录执行：

```sh
git add index.html styles.css
git diff --cached --check
git commit -m "Update homepage"
git push
```

只暂存本次需要公开的文件；修改本说明时单独加入 `README.md`。推送到 `main` 后会触发重新部署，网站更新可能需要约 10 分钟。

## 检查发布

1. 在 Actions 查看当前提交对应的 Pages 任务，确认成功。
2. 在 Settings → Pages 确认 `main` 和 `/(root)`，通过 Visit site 查看网址。
3. 可通过终端检查公网响应：

```sh
curl --fail --location --output /dev/null --write-out '%{http_code}\n' https://yy22yy.github.io/
curl --fail --location --output /dev/null --write-out '%{http_code}\n' https://yy22yy.github.io/styles.css
```

两次应返回 `200`；还要确认页面内容是当前版本。推送成功不代表部署完成。如果 404，先检查发布来源、根目录的 `index.html` 和部署日志；图片或样式缺失时检查路径大小写。

## 回退和撤下

已有正常历史版本时，撤销引入问题的那次提交并推送：

```sh
git log --oneline
git revert <引入问题的提交SHA>
git push
```

将占位符替换为实际 SHA，再次检查自动部署结果；不要强制推送改写历史。

首次发布尚无正常历史版本，或需要立即撤下站点时，打开 Settings → Pages，选择 **Unpublish site**。撤下网站不会让公开仓库变成私有仓库。

## 以后使用 React/Vite

需要组件和复杂交互时再引入 Vite，构建静态产物，并将 Pages Source 改为 GitHub Actions。最终部署包顶层应有 `index.html`。Pages 不运行后端服务，密钥不能放进前端代码。
