# TabTape - 标签页录音

[English](../README.md)

把任意浏览器标签页的声音录成 MP3 或 WAV。面向网课、研讨会、播客和网页会议——并修掉了现有录音工具的痛点：

- **不限时长** —— 边录边按约 1 秒分块编码写盘，内存占用恒定。免费单次 20 分钟；Pro 不限。
- **文件小** —— MP3 可选 128 kbps（标准）或 64 kbps（体积减半，录人声足够）；要无损就选 WAV。
- **录音永不丢** —— 停止即存入内置录音库（IndexedDB）并自动下载文件，关标签页也吃不掉你的录音。
- **录音时继续听** —— 捕获会把声音重连到扬声器，而不是静音。
- **录音库** —— 播放、重命名、下载、删除，显示已用空间。
- **Pro 裁剪** —— 选起止点、试听选区、导出（可同时存为新录音）。WAV 字节级精确切割，MP3 帧级切割；不做整段解码，长文件也安全。
- **权限极简** —— `storage`、`tabCapture`、`offscreen`、`downloads`、`alarms`、`unlimitedStorage`、`activeTab`。无 `<all_urls>`、无内容脚本、无账号、无云端。录音不出本机。

## 免费 vs Pro

| | 免费 | Pro |
|---|---|---|
| 录制标签页音频（MP3/WAV） | ✅ | ✅ |
| 单次时长 | 20 分钟 | 不限 |
| 录音库 + 自动下载 | ✅ | ✅ |
| 裁剪并导出选区 | — | ✅ |

一个授权码激活两个浏览器，移除即释放名额。验证约每 2 小时节流一次，离线可用缓存；验证失败绝不锁功能。

## 使用方法

1. 打开要录的标签页，点 TabTape 图标，按**录制此标签页**（或 Alt+Shift+R）。
2. 标签页继续有声播放，图标显示红色 REC 角标。
3. 按**停止**（或同一快捷键）。文件进入录音库和 Downloads/TabTape/。
4. 打开录音库播放、重命名、下载、删除——Pro 可裁剪。

## 隐私

- 全部本地：录音在 IndexedDB，设置与授权状态在 `chrome.storage.local`。无分析、无遥测。
- 唯一网络请求是向 `api.annmax1983.com` 激活/验证授权。
- MP3 编码使用内置的 [lamejs](https://github.com/zhuker/lamejs)（LGPL，见 `vendor/lamejs-LICENSE.txt`；未压缩源码保留在 `vendor/lame.js`）。

## 权限

| 权限 | 用途 |
|---|---|
| `tabCapture` | 捕获当前标签页音频（仅在你点击后） |
| `offscreen` | MV3 要求用 offscreen 文档持有媒体流 |
| `downloads` | 自动保存完成的文件 |
| `alarms` | 免费时长到点时强制停止录音（即使后台 worker 已休眠） |
| `unlimitedStorage` | 长录音存进本地库时不撞存储配额 |
| `storage` | 设置 + 授权状态 |
| `activeTab` | 调用扩展时读取当前标签页标题/URL |

## 支持浏览器

Chrome 与 Edge（Manifest V3，Chrome 116+）。不支持 Firefox：它没有 `tabCapture` 等价能力。

## 开发

```bash
npm test    # node --test：纯逻辑、MP3 编码器冒烟、background mock、i18n 覆盖
```

把仓库根目录作为未打包扩展加载（`chrome://extensions` → 开发者模式）即可试用。

## 链接

- 官网：https://annmax1983.com
- 授权与定价：https://annmax1983.com/checkout?plugin=tabtape