# ⚡ NEON RPC v2

> Multi-app Discord Rich Presence Engine
> **Python + VSCode Extension + Smart Detection**

<p align="center">
<img src="./assets/banner.png" width="900">
</p>

<p align="center">

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![Discord RPC](https://img.shields.io/badge/Discord-Rich%20Presence-5865F2?logo=discord)
![VSCode](https://img.shields.io/badge/VSCode-Extension-007ACC?logo=visualstudiocode)
![Status](https://img.shields.io/badge/Status-Active-success)

</p>

---

# ✨ Overview

**NEON RPC** là Discord Rich Presence engine đa ứng dụng.

Không dùng window-title hack, không scan file kiểu brute-force.

NEON sử dụng:

* Process detection
* WebSocket bridge
* Discord IPC mirror
* Plugin priority system

để hiển thị **Rich Presence realtime** cho:

* 💻 VSCode
* 🎮 FiveM
* 🎨 Blender
* 💤 Idle mode

---

# ⚡ Features

| Plugin  | Priority | Detection                        |
| ------- | -------: | -------------------------------- |
| FiveM   |        1 | Process + Discord IPC mirror     |
| Blender |        2 | Process + `.blend` cmdline parse |
| VSCode  |        3 | WebSocket bridge                 |
| Idle    |       99 | Fallback                         |

## 🚀 Core Features

### VSCode Smart Presence

Hiển thị realtime:

* 📄 File name
* 🐍 Language detect
* 📍 Cursor line / column
* ❌ Errors
* ⚠ Warnings
* 🌿 Git branch
* ✏ Dirty status
* 📏 Line count

**Zero window-title hacks.**

---

### 🎮 FiveM Mirror RPC

NEON đọc Discord IPC để mirror:

* server name
* details
* state
* image assets

Giữ nguyên vibe server RP.

---

### 🎨 Blender Detection

Detect:

* Scene editing
* Python scripting mode
* `.blend` filename
* Script asset mode

---

### ⚙ Plugin Priority System

Plugin có priority cao nhất đang active sẽ:

**own Discord Presence**

Ví dụ:

FiveM > Blender > VSCode > Idle

---

### 🔄 Robust RPC Connection

Built-in:

* Pipe retry
* Auto reconnect
* Watchdog
* Heartbeat
* Update throttle

Không spam Discord RPC.

---

### 🪵 Structured Logging

Colour logger format:

```txt
[TAG] LEVEL message
```

Example:

```txt
12:34:56 [RPC]     INFO    Connected to Discord
12:34:57 [VSCode]  INFO    main.py L84 | ❌2 ⚠1
12:35:00 [FiveM]   INFO    Mirrored RPC from Mini City RP
```

---

# 🧠 VSCode Language Detection

NEON v2 hiện **ngôn ngữ code thật** thay vì chỉ file name.

Ví dụ:

| File       | Language   | Asset         |
| ---------- | ---------- | ------------- |
| main.py    | Python     | python_logo   |
| app.ts     | TypeScript | ts_logo       |
| index.js   | JavaScript | js_logo       |
| style.css  | CSS        | css_logo      |
| server.lua | Lua        | lua_logo      |
| README.md  | Markdown   | markdown_logo |

Example Presence:

## Python

```txt
DETAILS  📝 main.py | 534 lines
STATE    🐍 Python | ❌2 ⚠1 | L84
LARGE    python_logo
TEXT     Coding with NeonRPC
```

## Lua

```txt
DETAILS  📝 server.lua
STATE    🌙 Lua | L120 [main]
LARGE    lua_logo
```

## TypeScript

```txt
DETAILS  📝 app.ts
STATE    ⚡ TypeScript | Clean
LARGE    ts_logo
```

Language được detect từ:

* VSCode `languageId`
* extension bridge payload
* asset mapper

Không parse extension thủ công.

---

# 📁 Project Structure

```txt
neon-rpc-v2/
│
├── main.py
├── rpc_client.py
├── bridge_server.py
├── logger.py
├── config.json
├── requirements.txt
│
├── plugins/
│   ├── vscode_bridge.py
│   ├── blender_bridge.py
│   ├── fivem_bridge.py
│   └── idle.py
│
├── helpers/
│   ├── asset_mapper.py
│   ├── discord_pipe_reader.py
│   └── utils.py
│
└── extension/
    └── neon-vscord/
        ├── src/
        │   └── extension.ts
        ├── package.json
        └── tsconfig.json
```

---

# ⚙ Setup

## 1. Install Python Dependencies

```bash
pip install -r requirements.txt
```

---

## 2. Create Discord Application

1. Open Discord Developer Portal
2. Create app
3. Copy Application ID
4. Paste into `config.json`
5. Upload RPC assets

Example:

```json
{
  "client_id": "YOUR_APP_ID"
}
```

---

## 3. Configure

`config.json`

```json
{
  "client_id": "YOUR_APP_ID",
  "scan_interval": 3,
  "update_throttle": 5,
  "bridge_port": 7878
}
```

---

## 4. Run Engine

```bash
python main.py
```

---

## 5. VSCode Extension

```bash
cd extension/neon-vscord
npm install
npm run compile
```

Run:

* Press **F5**
* or package with:

```bash
vsce package
```

Extension auto-connects via:

```txt
ws://127.0.0.1:7878
```

Status bar:

```txt
NEON ● Connected
```

---

# ⚙ Config Reference

| Key             | Default | Description          |
| --------------- | ------: | -------------------- |
| client_id       |       — | Discord app ID       |
| scan_interval   |       3 | Plugin poll interval |
| update_throttle |       5 | RPC update cooldown  |
| bridge_port     |    7878 | WebSocket port       |
| small_image     |  online | Status asset         |
| plugins.*       |       — | Enable / priority    |

---

# 🔌 Custom Plugin API

Create:

```txt
plugins/myplugin.py
```

Example:

```python
PRIORITY = 5

def is_running():
    return True

def get_rpc():
    return {
        "details": "My App",
        "state": "Running",
        "large_image": "my_logo"
    }
```

Restart:

```bash
python main.py
```

Plugin auto-discovery enabled.

---

# 💻 Example Presence

## VSCode

```txt
DETAILS  📝 main.py | 534 lines
STATE    🐍 Python | ❌2 ⚠1 | main
```

## FiveM

```txt
DETAILS  🎮 Mini City RP
STATE    🚓 Roleplaying
```

## Blender

```txt
DETAILS  🎨 city.blend
STATE    🐍 Blender Python
```

## Idle

```txt
DETAILS  💻 Coding by Roshi
STATE    🧋 Đang Uống Bạc Xỉu
```

---

# 🛣 Roadmap

* [x] VSCode bridge
* [x] FiveM mirror
* [x] Blender detect
* [x] Language assets
* [ ] Spotify plugin
* [ ] Browser plugin
* [ ] GUI settings panel
* [ ] Plugin marketplace

---

# 👤 Credits

Built with ☕ + vibe coding by **Roshi**

**NEON RPC v2**
Discord Presence — but smarter.
