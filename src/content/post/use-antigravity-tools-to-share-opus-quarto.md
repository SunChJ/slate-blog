---
title: Use Antigravity Tools to Share Opus Quarto
description: 降低难度享受 Claude Code
tags:
  - Antigravity
  - ClaudeCode
pubDate: 2026-01-14
draft: false
---
![](images/use-antigravity-tools-to-share-opus-quarto.png)
*Photo by [Matt Artz](https://unsplash.com/@mattartz?utm_source=Obsidian%20Image%20Inserter%20Plugin&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=Obsidian%20Image%20Inserter%20Plugin&utm_medium=referral)*
## 安装 Antigravity Manager
[GitHub - lbjlaq/Antigravity-Manager: 为 Antigravity 提供一键无缝账号切换功能。](https://github.com/lbjlaq/Antigravity-Manager)
```bash
# 1. 订阅本仓库的 Tap
brew tap lbjlaq/antigravity-manager https://github.com/lbjlaq/Antigravity-Manager

# 2. 安装应用
brew install --cask antigravity-tools
```

## 登录&打开「API反代」
1. 登录![](images/use-antigravity-tools-to-share-opus-quarto-1.png)
   登录成功后：
   ![](images/use-antigravity-tools-to-share-opus-quarto-3.png)
2. 打开「API反代」![](images/Pasted%20image%2020260114161026.png)
   打开成功后：
   ![](images/use-antigravity-tools-to-share-opus-quarto-2.png)
## Terminal环境&插件配置
ANTHROPIC_AUTH_TOKEN&ANTHROPIC_BASE_URL来源：
![](images/use-antigravity-tools-to-share-opus-quarto-5.png)

### Terminal 环境
sk-... 根据实际来配置比如上图的 sk-0f20247129c7429fa69a8d0b818c1643
临时：
```bash
cd your-project-folder
export ANTHROPIC_AUTH_TOKEN=sk-...
export ANTHROPIC_BASE_URL=http://localhost:8045
claude
```
全局：
```bash
echo -e '\n export ANTHROPIC_AUTH_TOKEN=sk-...' >> ~/.bash_profile
echo -e '\n export ANTHROPIC_BASE_URL=http://localhost:8045' >> ~/.bash_profile
echo -e '\n export ANTHROPIC_AUTH_TOKEN=sk-...' >> ~/.bashrc
echo -e '\n export ANTHROPIC_BASE_URL=http://localhost:8045' >> ~/.bashrc
echo -e '\n export ANTHROPIC_AUTH_TOKEN=sk-...' >> ~/.zshrc
echo -e '\n export ANTHROPIC_BASE_URL=http://localhost:8045' >> ~/.zshrc
```

### VS Claude Code 插件配置
依次如图点击：
插件入口进设置：
![](images/use-antigravity-tools-to-share-opus-quarto-6.png)
设置页进配置 json：
![](images/use-antigravity-tools-to-share-opus-quarto-7.png)

json配置关键的配置项：
![](images/use-antigravity-tools-to-share-opus-quarto-8.png)
```json
"claudeCode.preferredLocation": "panel",

"claudeCode.allowDangerouslySkipPermissions": true,

"claudeCode.initialPermissionMode": "bypassPermissions",

"claudeCode.disableLoginPrompt": true,

"claudeCode.environmentVariables": [

{

"name": "ANTHROPIC_API_KEY",

"value": "sk-0f20247129c7429fa69a8d0b818c1643"

},

{

"name": "ANTHROPIC_BASE_URL",

"value": "http://localhost:8045"

},

{

"name": "ANTHROPIC_DEFAULT_HAIKU_MODEL",

"value": "claude-opus-4-5-thinking"

},

{

"name": "ANTHROPIC_DEFAULT_SONNET_MODEL",

"value": "claude-opus-4-5-thinking"

},

{

"name": "ANTHROPIC_DEFAULT_OPUS_MODEL",

"value": "claude-opus-4-5-thinking"

},

],

"claudeCode.selectedModel": "claude-opus-4-5-thinking",
```


## 体验 Google 的 Opus 额度
 `claude` 启动!!!
