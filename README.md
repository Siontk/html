# WhatsApp 链接预览卡片

发 WhatsApp 消息时带上网址，对方消息里会自动显示「文字 + 预览图卡片」，点卡片跳网站。

## 文件说明

```
预览卡片/
├── index.html      # 页面 + Open Graph 标签（决定预览卡片长啥样）
├── preview.png     # 预览图（你自己放进项目）
└── README.md       # 本文件
```

> ⚠️ 预览图 **必须**叫 `preview.png` 放在项目根目录，或者你改 `index.html` 里所有 `preview.png` 的引用名。

---

## 一、本地预览（看页面长啥样）

直接双击 `index.html` 用浏览器打开即可。本地看不到 WhatsApp 预览效果，那需要部署到公网。

---

## 二、关键：Open Graph 标签

`index.html` 的 `<head>` 里这些标签决定了 WhatsApp 抓出来的卡片内容：

| 标签 | 作用 | 改哪里 |
|------|------|--------|
| `og:title` | 卡片标题（粗体） | 改 `content` |
| `og:description` | 卡片描述 | 改 `content` |
| `og:image` | 预览图 URL | 改成你的**公网绝对地址** |
| `og:url` | 网页地址 | 改成你的域名 |

**务必把所有 `https://YOUR_DOMAIN/` 替换成你的真实域名。**

---

## 三、部署到公网（必须 HTTPS）

WhatsApp 爬虫只认 HTTPS 公网地址。三种推荐方式，按你情况选：

### 方式 A：Vercel（最省事，推荐）

1. 注册 https://vercel.com（用 GitHub 账号登录）
2. 把这个文件夹 push 到一个 GitHub 仓库
3. Vercel 点 "New Project" → 选这个仓库 → Deploy
4. 几秒后拿到 `https://你的项目.vercel.app`
5. 把 `index.html` 里所有 `https://YOUR_DOMAIN/` 改成这个地址，重新部署

### 方式 B：Netlify

1. 注册 https://netlify.com
2. 把整个文件夹拖进 Netlify 的部署区
3. 拿到地址后同上修改 `og:image`、`og:url`

### 方式 C：Cloudflare Pages

1. 注册 https://pages.cloudflare.com
2. 连接 Git 仓库或直接上传
3. 同上

---

## 四、验证预览效果

部署好后，**不要直接发给别人测**，先用工具验证：

### 1. WhatsApp 官方调试器（最准）
- https://www.opengraph.xyz/
- 输入你的网址 → 选 WhatsApp → 看预览效果

### 2. Facebook 调试器（同套 OG 协议，可看抓取详情）
- https://developers.facebook.com/tools/debug/
- 输入网址 → 点 "Debug"，能看到 WhatsApp 读到的标签

### 3. 直接发给自己测试
- 在 WhatsApp 给自己发一条带网址的消息
- 看预览是否正常

---

## 五、常见坑

1. **改了 og:image 但预览没更新** → WhatsApp 有缓存。用 Facebook 调试器点 "Scrape Again" 强制刷新，或在网址后面加 `?v=2` 之类参数绕缓存。

2. **预览图不显示** → 检查：
   - 图是不是 HTTPS 地址
   - 图是不是公网可访问（浏览器无痕模式直接打开图链接能看到）
   - 尺寸是不是够大（至少 300x300，推荐 1200x630）
   - 格式是不是 jpg/png

3. **改了代码没生效** → 部署平台有 CDN 缓存，等 1-2 分钟或强制重新部署。

4. **localhost 测不出预览** → 正常，WhatsApp 抓不到本机地址，必须部署。

---

## 六、消息发送方式

部署完成后，在 WhatsApp 里这样发：

```
你的文字内容 https://你的域名/
```

文字和网址之间留个空格，WhatsApp 会自动识别网址并生成预览卡片。
