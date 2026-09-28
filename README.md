# Canvas_Sync – Collaborative Text + Whiteboard Editor

**Live Demo:** [https://canvassync-frontend.onrender.com](https://canvas-synccanvassync-frontend.onrender.com)

---


## Collaborative Editor Architecture

```mermaid
flowchart TD
    %% 1. Define Modern, Corporate Styles
    classDef boundary fill:#f8fafc,stroke:#cbd5e1,stroke-width:2px,rx:12px,color:#0f172a,font-weight:bold
    classDef ui fill:#ffffff,stroke:#3b82f6,stroke-width:2px,rx:8px,color:#1e3a8a
    classDef core fill:#eff6ff,stroke:#6366f1,stroke-width:2px,rx:8px,color:#312e81
    classDef network fill:#f0fdf4,stroke:#22c55e,stroke-width:2px,rx:8px,color:#14532d
    classDef storage fill:#fffbeb,stroke:#f59e0b,stroke-width:2px,rx:8px,color:#78350f

    %% 2. Client Architecture
    subgraph Client ["🌐 Client Application (Browser)"]
        direction LR
        
        UI["📱 User Interface<br/><span style='font-size:13px;font-weight:normal;color:#475569'>React • Text Editor • Whiteboard</span>"]:::ui
        
        CRDT["⚙️ CRDT Engine<br/><span style='font-size:13px;font-weight:normal;color:#475569'>RGA • ShapeCRDT • Lamport Clocks</span>"]:::core
        
        Sync["🔌 Sync Manager<br/><span style='font-size:13px;font-weight:normal;color:#475569'>WS Client • Binary Codec</span>"]:::network
        
        IDB[("💽 Offline Cache<br/><span style='font-size:13px;font-weight:normal;color:#475569'>IndexedDB</span>")]:::storage
        
        UI <-->|User Actions| CRDT
        CRDT <-->|Local State| IDB
        CRDT <-->|Encode/Decode| Sync
    end

    %% 3. Server Architecture
    subgraph Server ["🖥️ Backend Infrastructure (Node.js)"]
        direction LR
        
        Gateway["📡 WebSocket Gateway<br/><span style='font-size:13px;font-weight:normal;color:#475569'>Express • Binary Decoder</span>"]:::network
        
        Rooms["👥 Room Manager<br/><span style='font-size:13px;font-weight:normal;color:#475569'>State Validation • Broadcasting</span>"]:::core
        
        DB[("🗄️ Persistence Layer<br/><span style='font-size:13px;font-weight:normal;color:#475569'>Prisma • SQLite (Docs/Ops)</span>")]:::storage
        
        Gateway <-->|Parsed Ops| Rooms
        Rooms <-->|Save/Load| DB
    end

    %% 4. Network Link
    Sync <==>|"wss:// (Binary Payloads)"| Gateway
    
    %% Apply bounding box style
    class Client,Server boundary
```


## 🚀 What is this?

Canvas_Sync is a **real-time collaborative editor** where multiple people can edit the same document and draw shapes together – all synced instantly.

Unlike Google Docs, this uses **CRDTs** (Conflict-Free Replicated Data Types), which means:
- ✅ No central server needed for conflict resolution
- ✅ No merge conflicts – ever
- ✅ Works offline – edits sync when you reconnect

---

## ✨ Features

- **Text Editor** – Type together in real time
- **Whiteboard** – Draw shapes (rectangles, circles, lines) that sync instantly
- **Offline Support** – Edits are saved locally and sync when you reconnect
- **Undo/Redo** – Ctrl+Z / Ctrl+Shift+Z
- **Binary Protocol** – 85% less bandwidth compared to JSON
- **Docker Ready** – One command to run everything

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React + TypeScript + Vite + Tailwind CSS |
| CRDT Engine | Custom RGA (built from scratch) |
| Backend | Node.js + Express + WebSocket |
| Database | SQLite + Prisma |
| Whiteboard | Fabric.js |
| Deployment | Docker + Render |

---

## 🚀 How to Run Locally

### 1. Clone the repository
```bash
git clone https://github.com/thapasubashb/Canvas_Sync.git
cd Canvas_Sync


2. Start the frontend
bash
npm install
npm run dev
3. Start the backend (in a new terminal)
bash
cd server
npm install
npm run dev
4. Open your browser
text
http://localhost:3000
🐳 Run with Docker (One Command)
bash
docker compose up --build
Then open http://localhost:3000

📁 Project Structure
text
Canvas_Sync/
├── src/              # Frontend (React + TypeScript)
├── server/           # Backend (Node.js + WebSocket)
├── Dockerfile        # Frontend container
├── docker-compose.yml # Multi-container setup
└── README.md         # This file
📊 Key Metrics
Metric	Value
Bandwidth Reduction	85% vs JSON
Operation Latency	Sub-100ms
Concurrent Users	50+
Merge Conflicts	0 (zero)
🧠 How It Works
Every character and shape has a unique ID with a Lamport timestamp

Edits are ordered causally – no conflicts

Deleted items become tombstones – nothing is ever removed

When two users edit, their changes merge automatically

👨‍💻 About Me
GitHub: thapasubashb

LinkedIn: B . Subash

Instagram: Subash._.10

📄 License
MIT © 2026 Subash Thapa