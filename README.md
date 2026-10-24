# QQ Agent 脑洞社区（GitHub 版）

一个给 QQ Agent 用户交流 **人设 / 插件 / 技能** 的社区网站。
完全用 GitHub 免费功能搭起来：**GitHub Pages** 展示网页、**GitHub Issues** 收投稿和审核、**GitHub Actions** 自动把审核通过的内容同步到网页数据。

不需要买服务器，不需要注册别的平台，一分钱不花。

---

## 这个网站长什么样

- 顶部公告区（自定义的两句话）
- 三个分区：🎭 人设、🧩 插件、🛠️ 技能
- 任何人可以点「我要投稿」→ 跳到 GitHub 填表提交
- 管理员（就是你）在 GitHub 的 Issues 页面审核：打上「已通过」标签就显示，关闭 Issue 就下架
- 底部有免责声明，说明本站与 QQ Agent 官方无关

---

## 部署步骤（跟着点就行）

### 第一步：建仓库

1. 打开 <https://github.com/new>，登录你的 GitHub 账号
2. 仓库名填 `qqagent-community`（也可以自己起名，但后面要改配置，先按这个来）
3. 选 **Public（公开）**，这样别人才能访问网页
4. 不要勾选任何初始化选项（README、.gitignore 都不要勾），直接点 **Create repository**

### 第二步：上传文件

1. 打开 <https://github.com/你的用户名/qqagent-community/upload>
   （把「你的用户名」换成你自己的）
2. 把 `qqagent-community` 文件夹里的**所有内容**拖进去上传：
   - `index.html`
   - `data.json`
   - `.github` 文件夹（里面还有两层，一起拖）
   - `.gitignore`
   - `README.md`（可传可不传）
3. 点 **Commit changes** 提交

> ⚠️ 如果拖拽时看不到 `.github` 这种以点开头的隐藏文件夹，
> 就在网页上手动新建：进入仓库 → 点 **Add file → Create new file**，
> 文件名直接输入 `.github/workflows/sync.yml`，把文件内容粘贴进去保存。
> `.github/ISSUE_TEMPLATE/submit.yml` 同理。

### 第三步：建两个标签

1. 进仓库 → 点 **Issues** → 右侧 **Labels** → **New label**
2. 新建第一个：名字填 `待审核`，颜色随便（建议灰色）
3. 新建第二个：名字填 `已通过`，颜色建议绿色

> 注意：标签名字必须一字不差是「待审核」和「已通过」。

### 第四步：开启 GitHub Pages

1. 进仓库 → **Settings** → 左侧 **Pages**
2. **Source** 选 **Deploy from a branch**，分支选 **main**，目录选 **/ (root)**
3. 点 **Save**
4. 等一两分钟，页面上方会出现你的网址：`https://你的用户名.github.io/qqagent-community/`
5. 打开这个网址，就能看到网站了

### 第五步：修改配置（必须！）

网站首页 `index.html` 里有两行配置要改成你自己的：

```js
const GITHUB_USER = 'YOUR_GITHUB_USERNAME';   // ← 改成你的 GitHub 用户名
const REPO_NAME   = 'YOUR_REPO_NAME';          // ← 改成你的仓库名
```

改法：进仓库 → 点 `index.html` → 点右上角 ✏️ 铅笔图标编辑 → 找到这两行改掉 →
**Commit changes**。改完后 Pages 会自动更新，几分钟后生效。

> 不改成你自己的名字，「我要投稿」按钮跳转的 GitHub 链接就是错的。

---

## 怎么用（日常）

**用户投稿**：打开你的网站 → 点「我要投稿」→ 选分区 → 跳到 GitHub 登录填表 → 提交。
提交的 Issue 自动带「待审核」标签。

**你审核**：
1. 进仓库 → **Issues** → 看到带「待审核」标签的投稿
2. 点进去看内容，觉得不错 → 右侧标签区点 ✏️，删掉「待审核」，加上「已通过」
3. 保存后 Actions 会自动把这条内容同步进 `data.json`，网页刷新就能看到（约 1 分钟内）
4. 觉得不行 → 点 **Close issue** 关闭，它就不会出现在网页上
5. 想下架已通过的 → 去对应 Issue 点 **Close issue**

---

## 目录结构

```
qqagent-community/
├─ index.html                    网站首页（浏览 + 投稿入口，含配置）
├─ data.json                     网页显示的数据（自动生成，别手动改）
├─ .github/
│  ├─ ISSUE_TEMPLATE/
│  │  └─ submit.yml              投稿表单模板（用户填的就是这个）
│  └─ workflows/
│     └─ sync.yml                自动同步：把「已通过」Issue → data.json
├─ .gitignore
└─ README.md
```

工作原理：用户提交 Issue → 你打「已通过」标签 → GitHub Actions 自动把带
「已通过」标签的未关闭 Issue 解析成 `data.json` 并推送 → 网页读取 `data.json` 展示。

---

## 常见问题

**网页打开是 404 / 白屏**
检查第四步 Pages 是否开启、分支是否选对。开启后网址里的项目名要和仓库名完全一致。

**审核通过后网页没更新**
Actions 需要约 1 分钟。另外确认标签名字是「已通过」一字不差，且 Issue 是打开状态（没被关闭）。

**投稿跳转的链接不对**
说明第五步配置没改，`index.html` 里的用户名和仓库名还是 `YOUR_...`。

**想改顶部两句话 / 免责声明**
都在 `index.html` 里：顶部两句话在 `.notice` 区域，免责声明在底部 `<footer>` 的 `.disclaimer`，直接编辑提交即可。
