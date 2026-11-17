# git-study

![React](https://img.shields.io/badge/React-17.0.2-149ECA)
![react-scripts](https://img.shields.io/badge/react--scripts-4.0.3-blue)
![Commitizen](https://img.shields.io/badge/Commitizen-4.2.4-green)
![git-cz](https://img.shields.io/badge/git--cz-4.7.6-orange)

这是一份 **git-cz / Commitizen 约定式提交** 的学习记录。页面由 Create React App 初始化后保持默认模板，没有另外的业务功能。

仓库从 `Initialize project using Create React App` 开始，随后加入本说明和 `git-cz`。提交历史里能看到学习过程中的约定式提交，例如 `feat: 🎸 全局添加git-cz`。

## 仓库里有什么

- **默认 React 页面**：深色全屏背景、旋转的 React logo、文案 “Edit `src/App.js` and save to reload.”，以及指向 `https://reactjs.org` 的 “Learn React” 链接。浏览器标签标题是 `React App`。
- **一条组件测试**：`src/App.test.js` 断言页面上存在 “learn react” 链接。
- **约定式提交**：`commitizen` 把适配器指到 `git-cz`，根目录的 `changelog.config.js` 规定类型、emoji 和提问项。`npm run commit` 进入交互式提交。
- **Web Vitals 入口**：`src/index.js` 调用了 `reportWebVitals()`，但没有传入回调，因此当前不会把指标打印出来或上报。

仓库里没有应用截图。`public/` 与 `src/logo.svg` 里的图片是 Create React App 自带的图标和 logo。

## 技术栈

版本来自 `package.json` 的范围，以及 `package-lock.json` / `yarn.lock` 共同锁定的具体版本。

| 依赖 | package.json | 锁定版本 | 作用 |
| --- | --- | --- | --- |
| react | ^17.0.2 | 17.0.2 | 界面 |
| react-dom | ^17.0.2 | 17.0.2 | 渲染到 `#root` |
| react-scripts | 4.0.3 | 4.0.3 | 开发服务器、测试、生产构建（Webpack 4） |
| web-vitals | ^1.0.1 | 1.1.2 | 性能指标采集函数，当前未接回调 |
| commitizen | ^4.2.4 | 4.2.4 | 约定式提交命令行 |
| git-cz | ^4.7.6 | 4.7.6 | Commitizen 适配器，读取 `changelog.config.js` |
| @testing-library/react | ^11.1.0 | 11.2.7 | 组件渲染测试 |
| @testing-library/jest-dom | ^5.11.4 | 5.14.1 | DOM 断言 |
| @testing-library/user-event | ^12.1.10 | 12.8.3 | 已安装，现有测试未使用 |

`package.json` 中的脚本：

| 脚本 | 命令 | 做什么 |
| --- | --- | --- |
| `start` | `react-scripts start` | 开发服务器，默认 `http://localhost:3000` |
| `build` | `react-scripts build` | 产出 `build/` |
| `test` | `react-scripts test` | Jest，默认进入监听模式 |
| `commit` | `git-cz` | 交互式约定式提交 |
| `eject` | `react-scripts eject` | 把构建配置弹到项目里，不可逆。本仓库没有执行过 |

## 目录结构

```text
.
├── changelog.config.js   # git-cz 的类型、emoji、提问项
├── package.json          # 依赖、脚本、commitizen 适配器路径
├── package-lock.json     # npm 锁定文件（含已失效的镜像地址，见下方安装说明）
├── yarn.lock             # Yarn 锁定文件（同样指向这些镜像）
├── public/               # CRA 静态资源：index.html、favicon、PWA 图标、manifest
└── src/
    ├── index.js          # 挂载 <App />，并调用 reportWebVitals()
    ├── App.js            # 默认页面
    ├── App.css           # 页面与 logo 旋转动画
    ├── App.test.js       # “Learn React” 链接测试
    ├── index.css         # 全局字体
    ├── logo.svg
    ├── reportWebVitals.js
    └── setupTests.js     # 引入 @testing-library/jest-dom
```

## 环境要求

本文在 **Node.js 22.23.4**、**npm 10.9.9**、**Yarn 1.22.22** 上核对过。

`react-scripts` 4.0.3 使用 Webpack 4。在 Node.js 17 及以上，OpenSSL 3 禁用了 MD4，直接执行 `npm start` 或 `npm run build` 会报 `ERR_OSSL_EVP_UNSUPPORTED`。启动和构建前设置：

```bash
export NODE_OPTIONS=--openssl-legacy-provider
```

`npm test` 在本环境不设置该变量也能通过。

## 安装

两个 lock 文件把不少包的 `resolved` 记在 `registry.nlark.com` 和 `registry.npm.taobao.org`。2026-11-17 在本环境实测：

- `npm ci` 失败：npm 10 认为 lock 与当前解析结果不一致。
- 带着现有 `package-lock.json` 执行 `npm install` 失败：`registry.nlark.com` 无法解析，`registry.npm.taobao.org` 的 TLS 证书已过期。
- `yarn install --frozen-lockfile` 失败：同样是镜像证书过期。

下面的安装方式已在本环境跑通。它按 `package.json` 的版本范围从 `registry.npmjs.org` 安装，并避免把新的 lock 写回仓库。装好的具体版本可能和旧 lock 里钉住的版本不完全相同。

```bash
mkdir -p /tmp/git-study-locks
mv package-lock.json yarn.lock /tmp/git-study-locks/
npm install --no-package-lock
```

安装结束后如果还要保留仓库里原来的 lock 文件，把它们从 `/tmp/git-study-locks/` 移回来即可。`node_modules/` 已被 `.gitignore` 忽略。

## 运行

```bash
export NODE_OPTIONS=--openssl-legacy-provider
npm start
```

浏览器打开 `http://localhost:3000`。开发服务器会在保存 `src/` 后重新编译。

生产构建：

```bash
export NODE_OPTIONS=--openssl-legacy-provider
npm run build
```

成功后静态文件在 `build/`。`package.json` 没有 `homepage` 字段，构建按站点根路径 `/` 生成资源地址。

只跑一次测试（不进入监听）：

```bash
npm test -- --watchAll=false
```

在 `CI=true` 时，`npm test` 也会只跑一轮。

约定式提交需要先暂存文件：

```bash
git add <文件>
npm run commit
```

没有暂存内容时，命令会退出并提示 `No files added to staging! Did you forget to run git add?`。这是交互式流程，需要在终端里逐项填写。

## 约定式提交怎么配的

`package.json` 把适配器设为本地的 git-cz：

```json
"config": {
  "commitizen": {
    "path": "./node_modules/git-cz"
  }
}
```

`npm run commit` 执行的是 `node_modules/.bin/git-cz`。按上一节从 npmjs 安装后，这个可执行文件链接到 **Commitizen** 自带的 `git-cz`，Commitizen 再按 `config.commitizen.path` 加载 `git-cz` 适配器。`commitizen` 与 `git-cz` 都提供同名命令，其他安装方式下链接到哪一个，取决于安装顺序。两条路径都会用到根目录的 `changelog.config.js`（适配器也会识别 `.git-cz.json`、`changelog.config.cjs`、`changelog.config.json`，本仓库只用了第一种）。

`disableEmoji` 为 `false`，主题至少 3 个字符、至多 64 个字符。交互提问顺序是 `type`、`scope`、`subject`、`body`、`breaking`、`issues`、`lerna`。其中：

- `scopes` 是空数组，scope 这一问会被跳过。
- 本仓库不是 Lerna 项目，包选择这一问会被跳过。

实际会问到的是类型、简短说明、更长描述、BREAKING CHANGE，以及关闭的 issue（例如 `#123`）。

可选类型（`list` 中的顺序）：

| 类型 | Emoji | 含义（配置原文） |
| --- | --- | --- |
| test | 💍 | Adding missing tests |
| feat | 🎸 | A new feature |
| fix | 🐛 | A bug fix |
| chore | 🤖 | Build process or auxiliary tool changes |
| docs | ✏️ | Documentation only changes |
| refactor | 💡 | A code change that neither fixes a bug or adds a feature |
| style | 💄 | Markup, white-space, formatting, missing semi-colons... |
| ci | 🎡 | CI related changes |
| perf | ⚡️ | A code change that improves performance |

`changelog.config.js` 的 `types` 里还写了 `release`（🏹），但它不在 `list` 中，交互菜单不会出现这一项。

## 本环境已核对的步骤

2026-11-17，Node.js 22.23.4 上：

- 按上一节移开 lock 后，`npm install --no-package-lock` 成功。
- `npm test -- --watchAll=false` 通过：`src/App.test.js` 1 个测试。
- 设置 `NODE_OPTIONS=--openssl-legacy-provider` 后，`npm start` 编译成功，`http://localhost:3000` 返回 CRA 的 `index.html`，并加载 `/static/js/bundle.js` 等脚本。页面文案由客户端渲染，链接是否存在由上述测试覆盖。
- 同样的环境变量下，`npm run build` 编译成功。
- 工作区没有暂存文件时，`npm run commit` 以退出码 1 结束，并打印暂存提示。没有继续走完交互提问，也没有因此产生提交。

未执行 `npm run eject`。

## 参考

- [Create React App](https://create-react-app.dev/)
- [Commitizen](https://github.com/commitizen/cz-cli)
- [git-cz](https://github.com/streamich/git-cz)
