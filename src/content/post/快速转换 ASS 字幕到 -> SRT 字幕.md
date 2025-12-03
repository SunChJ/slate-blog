---
title: 快速转换 ASS 字幕到 -> srt 字幕
description: 百度网盘无法识别 ASS 字幕的替代方案
tags:
  - 字幕
  - ffmpeg
pubDate: 2025-12-03
draft: false
---
![](images/快速转换%20ASS%20字幕到%20->%20SRT%20字幕.png)
*Photo by [Monica Flores](https://unsplash.com/@monicadear?utm_source=Obsidian%20Image%20Inserter%20Plugin&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=Obsidian%20Image%20Inserter%20Plugin&utm_medium=referral)*

## 痛点
最近在重温「向美好的世界献上祝福」，资源是没有内嵌字幕的BD视频，配套的字幕是ASS格式（歌曲的动感字幕非常牛逼）。在家里喜欢存在百度网盘上，通过电视机观看。实际观影中，发现 ASS 字幕无法识别，遂打算快速转换成 SRT 格式。

## 解决
询问了 GPT，发现 ffmpeg 支持快速批量转换格式：
### 1. 安装ffmep
```
brew install ffmpeg
```

### 2. 转换 ASS -> SRT
```
ffmpeg -i input.ass output.srt

```
但是会自动去掉样式，仅保留纯文本。

**也支持批量转换**
```
for f in *.ass; do ffmpeg -i "$f" "${f%.ass}.srt"; done

```

Enjoy it!