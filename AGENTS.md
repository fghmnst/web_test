# AGENTS.md

个人像素风网站，当前处于 **prototype 阶段**（index.html 顶部有 PROTOTYPE 标记）。纯 HTML/CSS/JS，无构建工具、无 package.json、无测试框架。所有设计决策只存在于与用户的访谈对话中，没有文档——改动前先与用户确认。

## 运行与验证

- 本地预览：VS Code **Live Server 插件**（默认端口 5500）——右键 `index.html` → "Open with Live Server"，或状态栏 "Go Live"；以 `/home/fgh/web_test` 为工作区根目录打开，否则 5500 根目录不对；保存自动刷新
  - 旧方案已弃用：`python3 -m http.server 8000` 进程不抗重启且易因启动目录错误导致 404（排查：`pgrep -f http.server` + `readlink /proc/<pid>/cwd`）；不要再启动 8000 端口服务器
- JS 语法检查：`sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d' > /tmp/x.js && node --check /tmp/x.js`
- 无 lint/typecheck/test，浏览器手动验证。

## 结构

- `index.html` — 单文件原型，内嵌全部 CSS/JS。3 个 UI 变体（A 经典标题菜单 / B 像素窗口面板 / C 对话选项），通过 `?variant=a|b|c` 或底部浮动栏（含 ←/→ 键盘）切换。变体胜出后需折进正式实现并移除切换条。
- `fonts/` — 两套 Nerd Font（各 2.3MB，上线前需裁剪子集）：
  - `BigBlueTerm437NerdFont-Regular.ttf` — 仅用于拉丁标题（font-family: `BigBlue`）
  - `MononokiNerdFont-Regular.ttf` — 正文（font-family: `Mononoki`）
  - 中文一律回退系统字体（两套字体均无中文字形）
  - 字体源文件在 `/mnt/d/OhMyPosh/fonts/`（Windows D 盘挂载），含 LICENSE 已随附。

## 已锁定的设计决策（访谈确认，勿擅自更改）

- 技术栈：纯三件套；多页结构已让位于"开场 + 菜单"单页交互
- 交互：开场居中头像 + 网名 + 闪烁"点击任意处开始" → 点击后头像右移、左侧浮现菜单（欢迎/选项二/选项三，选项内容未定）
- 视觉：整体如像素画（非游戏 UI）；像素素材全部代码生成（JS 里 `AVATAR_MAP` 等字符串数组 + `pixelSvg()` 渲染），改素材改代码，不用图片文件
- 配色：**待定**——当前 :root 变量是临时柔和低饱和占位
- 头像、网名（FGH）均为占位，等用户提供
- 动效：轻量 CSS 为主 + 少量招牌动效（云漂移、待机晃动、打字机）
- 移动端：只需"能看"级别

## 笔记约定

- `obsidian_web/` — 存放 Obsidian 笔记（markdown 格式）。一切笔记性质的内容创建到该文件夹
- 纯文本编辑直接改 markdown 原文；涉及 Obsidian 特色功能（wikilink、properties、bases、canvas 等）优先使用 obsidian cli 相关 skills

## 工程惯例

- 本仓库由 git 追踪（`git init -b main` 已初始化），改动以 commit 记录，同步至 GitHub 远程仓库
- GitHub 远程仓库：**public**，账号 fghmnst（gh CLI 已登录，SSH 协议）
- 涉及 sudo 的操作用户自行完成，代理不要执行
- 提交前检查 `git status` / `git diff`，只暂存预期文件

## 日志约定

- 用户回复"日志"时：阅读当天全部 session 内容，总结"今天干了什么 / 明天该干什么"，写入 `obsidian_web/日志/`（markdown）
- 子文件夹 `obsidian_web/日志/` 已存在，日志文件名建议带日期（如 `2026-08-23.md`）

## 待办状态

- 菜单选项点击后的行为未实现（用户选"演示优先"）
- 用户将自行决定爱好内容、正式配色、网名、头像
- 暂不上线（部署方式后定）
