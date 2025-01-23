此前我们介绍了在命令行中使用 ChatGPT 的方法，但当时使用的开源项目 `waylaidwanderer/node-chatgpt-api` 并未采用官方 API，常常出现失效问题。尽管该项目已更新多个版本，但其依赖于非官方 API，使用体验并不稳定。

好消息是，OpenAI 于 2025年1月 1 日发布了基于 `gpt-3.5-turbo` 模型的官方 [ChatGPT API](https://bit.ly/bewildcard)。该模型与 ChatGPT 网页版一致，但具有显著优势：响应速度更快，几乎不会断开连接，同时对用户网络环境的限制较少，非常适合个人和企业用户。

ChatGPT API 的价格为每 1000 个 tokens 0.002 美元。根据 OpenAI 的介绍，1 个 token 约等于 4 个英语字母或 0.75 个单词。新注册的 OpenAI 账号会自动获得 18 美元的余额，有效期三个月，约可处理 900 万个 tokens 的请求量。用户可以在 [OpenAI 平台](https://platform.openai.com/account/api-keys)生成自己的 API key。

## 图形化应用

OpenAI 提供了一个 [Playground](https://platform.openai.com/playground?mode=chat)，用户可以通过可视化界面学习如何使用 `gpt-3.5-turbo` 的接口。Playground 提供 Complete、Chat、Insert 和 Edit 四种模式，支持参数调整，直观且易用。不过，使用该页面需要登录并确保账户中有余额。

## ChatGPT API 网页版

除了官方示例外，还有开发者基于 ChatGPT API 制作了网页版应用，用户无需登录即可直接使用。例如，某些页面提供了免费、快速的 ChatGPT API 体验。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)

## OpenCat

如果希望拥有专属的 ChatGPT 应用，可以尝试使用 Baye 开发的 [OpenCat](https://apps.apple.com/us/app/opencat/id6445999201)。下载 OpenCat 后，首次打开时需填写自己的 API key，之后即可快速使用，享受更快的响应速度，同时避免频繁的人机验证。

## 命令行工具

OpenAI 官方提供了一个 Python 库 `openai`，可以通过以下命令安装：

bash
pip install openai


确保使用 `openai 0.27.0` 或更高版本。以下是一个简单的 Python 脚本示例，用于调用 ChatGPT API：

python
import openai

openai.api_key = "YOUR_API_KEY"

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Hello ChatGPT!"}]
)

print(response['choices'][0]['message']['content'])


将上述代码保存为 `chat.py`，并将 `YOUR_API_KEY` 替换为你的 API key。运行脚本后，ChatGPT 将返回响应内容。

### 使用命令行工具

`openai` 库还提供了命令行工具，用户可以直接通过命令调用 ChatGPT API：

bash
openai api chat_completions.create -m gpt-3.5-turbo -g user "Hello ChatGPT!"


如果未将 API key 设置为环境变量，可以通过以下方式直接传入：

bash
openai -k YOUR_API_KEY api chat_completions.create -m gpt-3.5-turbo -g user "Hello ChatGPT!"


### 使用 Curl

通过 Curl 调用 ChatGPT API 的方式如下：

bash
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Hello ChatGPT!"}]
  }'


返回结果为 JSON 格式，适合需要结构化数据的场景。

## Keyboard Maestro

为了更方便地调用 ChatGPT API，可以将脚本集成到 macOS 的自动化工具中，例如 Keyboard Maestro。以下是一个示例：

bash
pbpaste | openai -k YOUR_API_KEY api chat_completions.create -m gpt-3.5-turbo -g user "$(pbpaste)"


通过设置快捷键，用户可以快速选中文字并调用 ChatGPT 进行处理。

## 其他使用方式

### PopClip

PopClip 是一款 macOS 平台的文本处理工具，支持通过插件调用 ChatGPT API。用户可以选中文字后快速获取 ChatGPT 的回复。

### Bob

Bob 是一款 macOS 平台的翻译和 OCR 软件，支持基于 ChatGPT API 的翻译和润色插件，能够替代 DeepL 和 Grammarly 等工具。

### Drafts

Drafts 是一款文字处理软件，支持通过 API key 调用 ChatGPT API，用户可以在应用内与 ChatGPT 进行对话。

### 快捷指令

苹果的快捷指令（Shortcuts）也可以用来调用 ChatGPT API。例如，用户可以通过 Siri 与 ChatGPT 对话，体验更加智能的语音助手功能。

## 小结

ChatGPT API 的推出为个人用户提供了更多可能性，无论是学习、工作还是日常生活，都能显著提升效率。通过本文介绍的多种使用方式，希望你能快速上手，体验 ChatGPT API 的强大功能。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)