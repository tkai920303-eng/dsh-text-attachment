# dsh-text-attachment

DeepSeek Harness Web GUI（`@deepseek-ai/dsh` **0.1.0-rc.6**，npm 全局安装）的**文本文件附件**运行包补丁：输入栏 📎 按钮，把本地**文本文件**内容直接**注入对话提示词**，让模型看到文件全文——无需落盘附件系统支持。纯客户端单文件覆盖，无源码构建。

> 中文 | [English](README.en.md) · CI: [![CI](https://github.com/tkai920303-eng/dsh-text-attachment/actions/workflows/ci.yml/badge.svg)](https://github.com/tkai920303-eng/dsh-text-attachment/actions/workflows/ci.yml)

## 功能

- 支持 `text/*` MIME + 扩展名白名单：txt / md / markdown / csv / tsv / json / log / yaml / yml / toml / xml / ini / cfg / env / properties / py / js / ts / tsx / jsx / html / css / sh / ps1 / sql / r；
- 单文件 ≤ 512 KB，FileReader 读为 UTF-8；
- 选中后输入框上方显示"文件名（大小）"芯片（可移除）；
- 发送时文件内容以 `===== 附件文件：<名> =====` 分隔头拼入消息文本（历史可见、无二进制落盘）；
- 拖放/粘贴自动分流：图片 → 原图片流程、文本 → 文件、其他类型 → toast 提示；
- 中英文 locale。

## 文件清单（1 个）

| 文件 | 作用 | 生效方式 |
|---|---|---|
| `dsh-client-ui-conversation/lib/client.js` | 附件按钮 + 文件读取 + 注入逻辑 | 硬刷新（纯客户端） |

## 安装

```powershell
$root = "$env:APPDATA\npm\node_modules\@deepseek-ai\dsh\node_modules\@deepseek-ai"
Copy-Item files\dsh-client-ui-conversation\lib\client.js "$root\dsh-client-ui-conversation\lib\client.js" -Force
# 浏览器硬刷新即可使用
```

## 设计取舍 / 已知限制

- 内容进消息文本：模型可见、历史可回溯、无附件管理负担；代价是同一内容会占用上下文 token。
- Office / PDF / 图片等二进制**不支持**（图片走原生图片流程；其他类型 toast 拒绝）。
- 仅注入当次消息；如需反复引用请粘贴或再次选择文件。
- 目标版本 rc.6；卸载/回滚：`npm install -g @deepseek-ai/dsh@0.1.0-rc.6` 重装恢复原版（改动会被覆盖，先备份）。

## CI

`.github/workflows/ci.yml`：`node --check` 全部 JS + README 存在性检查。

## License

[MIT](LICENSE)
