作为一名热爱艺术的用户，能够随时关注 Pixiv 上画师的最新动态是一件令人愉悦的事情。然而，每天手动检查新图既费时又费力，有没有更高效的方法来推送这些图片，让每一张作品都能被及时欣赏呢？

本文将介绍如何通过 RSS 和 Telegram Bot 实现 Pixiv 新图的自动推送，帮助你轻松订阅画师的最新作品。

---

## 为什么选择 RSS 和 Telegram？

RSS 是一种常用的资讯订阅方式，而 Telegram 则是一个功能强大的通讯工具。将两者结合，可以实现自动化的内容推送。以下是实现的基本思路：

1. 使用 RSSHub 提供的 Pixiv 画师订阅接口获取数据。
2. 通过 Node.js 开发工具，将数据处理后推送到 Telegram 频道。

---

## 实现步骤

### 第一步：RSS 数据采集

为了简化 RSS 数据的处理，我们使用了 `rss-parser` 组件。它可以将 RSS 数据解析为 JavaScript 对象，方便后续操作。

安装组件：
bash
npm i rss-parser --save


使用示例：
javascript
const Parser = require('rss-parser');
const parser = new Parser();

(async () => {
  const feed = await parser.parseURL('RSSHub生成的Pixiv订阅链接');
  console.log(feed.items);
})();


通过解析后的数据，可以获取到画师的最新作品信息。

---

### 第二步：数据处理

Pixiv 的图片链接格式化后，通常形如：
- 单图链接：`https://pixiv.cat/*ArtworkID*.jpg`
- 多图链接：`https://pixiv.cat/*ArtworkID*-PicID.jpg`

我们可以使用正则表达式提取这些链接：
javascript
const picIdReg = /https:\/\/pixiv\.cat\/(\d+)-?(\d+)?\.(jpg|png|gif)/gi;
const artworks = [...item.content.matchAll(picIdReg)];


对于 Telegram 的图片大小限制，可以选择发送预览图，并提供完整图的下载链接。预览图地址可以通过 RSS 数据中的 `isoDate` 字段计算生成。

---

### 第三步：消息发送

使用 Telegram Bot API，可以将图片和相关信息推送到指定频道。以下是一个简单的 POST 请求示例：
javascript
const got = require('got');

const apiBaseUrl = `https://api.telegram.org/bot${confData.bot.token}`;
got.post('sendPhoto', {
  prefixUrl: apiBaseUrl,
  json: {
    chat_id: confData.bot.chat,
    photo: picItem.preview,
    caption: picItem.text,
    reply_markup: {
      inline_keyboard: [
        [
          { text: '🌏 查看原图', url: picItem.url },
          { text: '⤵ 下载图片', url: picItem.pic }
        ]
      ]
    }
  }
});


通过内联键盘功能，用户可以方便地查看原图或下载图片。

---

## 部署与运行

为了方便管理，可以使用 `pm2` 等工具运行 Bot：
bash
pm2 start bot.js --name phandream


完成配置后，稍等片刻，你就能在 Telegram 频道中看到画师的最新作品了！

---

## 小贴士

- Bot API Token 可以通过 [@BotFather](https://t.me/BotFather) 获取。
- Chat ID 可以直接使用 `@频道名`，无需复杂的频道 ID。

---

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)

通过以上方法，你可以轻松实现 Pixiv 新图的自动推送，享受更高效的艺术订阅体验！