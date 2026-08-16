# dsh-text-attachment

Runtime patch for the **DeepSeek Harness Web GUI** (`@deepseek-ai/dsh` **0.1.0-rc.6**, npm global install): a 📎 button in the composer that injects a local **text file**'s content directly into the conversation prompt, so the model sees the whole file — no attachment-storage support required. Client-only, single-file overwrite, no source build.

> [中文](README.md) | English · CI: [![CI](https://github.com/tkai920303-eng/dsh-text-attachment/actions/workflows/ci.yml/badge.svg)](https://github.com/tkai920303-eng/dsh-text-attachment/actions/workflows/ci.yml)

## Features

- Supports `text/*` MIME + extension allowlist: txt / md / markdown / csv / tsv / json / log / yaml / yml / toml / xml / ini / cfg / env / properties / py / js / ts / tsx / jsx / html / css / sh / ps1 / sql / r;
- Single file ≤ 512 KB, read as UTF-8 via FileReader;
- A "filename (size)" chip appears above the composer (removable);
- On send, the content is prepended with a `===== 附件文件：<name> =====` header into the message text (visible in history, no binary storage);
- Drag-drop / paste auto-routes: images → native image flow, text → file, others → toast;
- zh / en locale.

## Files (1)

| File | Role | Effect |
|---|---|---|
| `dsh-client-ui-conversation/lib/client.js` | attach button + file reading + injection logic | hard refresh (client-only) |

## Install

```powershell
$root = "$env:APPDATA\npm\node_modules\@deepseek-ai\dsh\node_modules\@deepseek-ai"
Copy-Item files\dsh-client-ui-conversation\lib\client.js "$root\dsh-client-ui-conversation\lib\client.js" -Force
# hard-refresh the browser
```

## Design notes / known limits

- Content goes into the message text: model-visible, traceable in history, no attachment management; the trade-off is the token cost of the content in context.
- Office / PDF / image binaries are **not** supported (images use the native flow; others get a toast).
- Injects into the current message only; re-select the file to reference it again.
- Target rc.6; rollback: `npm install -g @deepseek-ai/dsh@0.1.0-rc.6` restores the pristine release (overwrites everything — back up first).

## CI

`.github/workflows/ci.yml`: `node --check` on every JS file + README presence check.

## License

[MIT](LICENSE)
