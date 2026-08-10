# 🎓 LectureNote Agent

> 课堂实时同声传译 + 中英双语结构化大纲笔记 — Eazo Global Youth AI Agent Hackathon (Singapore)

**Frontend**: [https://lecture-note-tau.vercel.app](https://lecture-note-tau.vercel.app)  
**Backend**: `wss://lecture-note-2we1.onrender.com`

---

## 📸 Screenshots

### Start Screen
Dark themed landing page with PPT upload and history access.

### Recording View (Dual Panel)

| Left: Real-time Bilingual Subtitles | Right: AI-Generated Outline |
|-------------------------------------|------------------------------|
| Live English speech recognition → Chinese translation | Auto-generated structured lecture notes |
| Pause / Resume / Stop controls | Editable rich-text with highlight toolbar |
| Yellow banner when paused | Practice questions after class |

### Replay View
Tabbed view: outline, subtitles, practice questions — with Word/PDF export.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🎙️ **Real-time ASR** | Azure Speech Services, `en-IN` model optimized for Indian English |
| 🌐 **Simultaneous Translation** | DeepSeek API: real-time English → Chinese, partial-text streaming for low-latency同传 |
| 🧹 **Filler Word Filtering** | Automatically filters hesitation words (yeah, OK, um…) from the subtitle feed |
| 📝 **AI Outline** | Auto-generated bilingual structured outline every 30 seconds, merges with manual edits |
| 📎 **Courseware Upload** | Upload `.pptx` / `.pdf` files to provide lecture context; image-based PDFs auto-OCR |
| ✏️ **15 Exam Questions** | End-of-class: 15 core quiz questions prioritized by exam hints (MCQ / short answer / true-false) |
| ⏯️ **Pause / Resume** | Adapt to lecture breaks and discussions |
| 🎨 **Rich Text Editing** | 4-color highlighting (重点/理解/考点/公式), H1/H2/H3 headings, Bold/Italic/Underline |
| 🔍 **Smart Auto-Scroll** | Auto-follows new subtitles; pauses when user scrolls up to review past content |
| 📥 **Export** | Word document (`.doc`), PDF print, copy to clipboard |
| 📚 **Session History** | Auto-save lectures to localStorage (max 20), browse and replay past sessions |
| 🇬🇧 **English-Only Filter** | Automatically skips Chinese speech / pinyin to avoid noise |
| 🛡️ **Auto Wake-Up** | Pings Render backend on page load to wake from free-tier sleep |

---

## 🏗️ Architecture

```
Browser (React + Vite + TailwindCSS)
    │  WebSocket (audio binary + JSON messages)
    │  HTTP POST (courseware upload)
    ▼
Node.js Backend (Render)
    ├── Azure Speech Services (ASR, en-IN, 16kHz PCM)
    ├── DeepSeek API (translation + outline + questions)
    ├── adm-zip (PPTX text extraction)
    └── pdfjs-dist + Tesseract.js (PDF text + OCR)

Frontend Deployed on: Vercel
Backend Deployed on:  Render
```

---

## 📖 使用方式（User Guide）

### 第一步：打开网页

浏览器访问 **[https://lecture-note-tau.vercel.app](https://lecture-note-tau.vercel.app)**（无需安装，支持 Chrome / Edge / Safari）。

> ⚠️ 如果页面打开后一直显示"服务器正在唤醒"，请等待 30-60 秒。这是因为后端托管在 Render 免费版，闲置时会休眠。

### 第二步：上传课件（可选）

在首页点击 📎 区域，上传这节课的 **PPTX** 或 **PDF** 课件。AI 会参考课件内容生成更准确的课程大纲。

- 支持格式：`.pptx`、`.pdf`
- 图片型 PDF（扫描件）会自动 OCR 识别文字
- 课件不会被存储到服务器，用完即删

### 第三步：开始听课

点击 **🎙️ 开始听课** 按钮，浏览器会请求麦克风权限（首次使用需要点击"允许"）。

进入课堂后，页面分为左右两栏：

| 左侧：实时字幕 | 右侧：AI 大纲 |
|---------------|-------------|
| 英语原文（黑色） | 课程标题（可编辑） |
| 中文翻译（深色加粗） | 章节 + 知识点（自动分级） |
| 新内容自动滚动，上翻查看历史时暂停滚动 | 知识点可手动编辑/高亮/加粗/删除 |

### 第四步：课堂操作

- **暂停 ⏸️**：课间休息或讨论时暂停，暂停期间不录音不翻译
- **继续 ▶️**：恢复录音和翻译
- **结束 ⏹️**：下课点结束，AI 自动出练习题

### 第五步：编辑笔记

在大纲面板中：

- **选中文字** → 弹出工具栏，支持 4 色高亮（🟡重点 / 🟢理解 / 🔴考点 / 🔵公式）、H1/H2/H3 标题、加粗/斜体/下划线
- **用户修改过的内容** 不会被之后的 AI 更新覆盖（标记 👤 来源）
- **添加章节/知识点**：点击底部的虚框按钮

### 第六步：导出笔记

下课后，大纲面板底部出现导出按钮：

- **📄 Word 文档**：保留完整格式，`.doc` 直接下载
- **🖨️ PDF 打印**：浏览器打印成 PDF
- **📋 复制**：纯文本大纲复制到剪贴板

### 第七步：查看历史

主页点击 **📚 历史课程**，可以回放之前保存的课程（最多 20 条），查看：

- 📝 课程大纲（只读）
- 💬 双语字幕（只读）
- ✏️ 练习题（含答案）

> 💡 历史数据保存在浏览器本地（localStorage），清除浏览器数据会丢失。

### 使用场景

| 场景 | 建议 |
|------|------|
| 印度裔教授的大课 | 上传 PPT → 开始听课 → 课后导出 Word 笔记 |
| 英文讲座/研讨会 | 不上传课件 → 直接听课 → AI 自动整理大纲 |
| 课后复习 | 打开历史课程 → 查看大纲 + 做练习题 |
| 小组讨论 | 暂停功能跳过讨论时间，回来继续记录 |

---

## 🚀 Quick Start (Local Development)

### Prerequisites
- Node.js 18+
- Azure Speech Services API key (japaneast region)
- DeepSeek API key

### Setup

```bash
# Clone
git clone https://github.com/Stevenchen213/lecture-note.git
cd lecture-note

# Frontend
npm install
npm run dev          # → http://localhost:3000

# Backend (in another terminal)
cd server
npm install
cp ../.env.example .env   # then edit .env with your API keys
node index.js        # → ws://localhost:8080
```

### Environment Variables

Create `server/.env`:

```env
AZURE_SPEECH_KEY=your_azure_speech_key
AZURE_SPEECH_REGION=japaneast
DEEPSEEK_API_KEY=your_deepseek_api_key
```

For production frontend:

```env
VITE_WS_URL=wss://your-render-backend.onrender.com
```

---

## 📦 Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18, Vite 5, TailwindCSS 3 |
| Backend | Node.js 20, WebSocket (`ws`) |
| ASR | Azure Speech Services (en-IN, continuous recognition) |
| LLM | DeepSeek API (OpenAI-compatible) |
| PPT Parsing | `adm-zip` (PPTX = ZIP of XML) |
| PDF Parsing | `pdfjs-dist` + `canvas` (text extraction + render) |
| OCR | `tesseract.js` (image-based PDF fallback) |
| Frontend Hosting | Vercel (free) |
| Backend Hosting | Render (free) |

---

## 📁 Project Structure

```
lecture-note/
├── index.html                    # Entry HTML
├── vite.config.js                # Vite config
├── tailwind.config.js            # TailwindCSS config
├── vercel.json                   # Vercel deployment config
├── src/
│   ├── main.jsx                  # React entry
│   ├── App.jsx                   # Root component + routing
│   ├── index.css                 # Global styles + animations
│   ├── components/
│   │   ├── StartScreen.jsx       # Landing page + courseware upload
│   │   ├── SubtitlePanel.jsx     # Real-time bilingual subtitles
│   │   ├── OutlinePanel.jsx      # Editable outline + questions
│   │   ├── RichTextEditor.jsx    # contentEditable + floating toolbar
│   │   ├── HistoryPanel.jsx      # Past sessions list
│   │   └── ReplayView.jsx        # View past session
│   ├── hooks/
│   │   ├── useAudioRecorder.js   # Browser mic → Int16 PCM
│   │   └── useWebSocket.js       # WebSocket connection manager
│   └── utils/
│       ├── outlineMerge.js       # AI + user edit merge logic
│       ├── export.js             # Word / PDF / clipboard export
│       └── sessionStore.js       # localStorage session persistence
├── server/
│   ├── index.js                  # WebSocket + HTTP server
│   ├── azure-speech.js           # Azure ASR integration
│   ├── deepseek.js               # DeepSeek API (translate + outline + questions)
│   ├── pdf-utils.js               # PDF text extraction + OCR fallback
│   └── package.json              # Server dependencies
└── .env.example                  # Environment variables template
```

---

## 🏆 Hackathon

- **Event**: Eazo Global Youth AI Agent Hackathon
- **Track**: Personal Agent Challenge
- **Location**: Singapore
- **Deadline**: August 10, 2026, 22:00 (UTC+8)

---

## 👤 Author

**Steven Chen** — [GitHub](https://github.com/Stevenchen213)
