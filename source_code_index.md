# ISRO Offline AI Assistant - Source Code Directory

This index lists the location of all source code files, static frontend assets, and databases for the **ISRO Offline AI Assistant** desktop application.

---

## 🏗️ Application Architecture

The standalone client is built as a native desktop wrapper wrapping a local Flask server backend:
1. **Desktop Client Frame**: PyWebView parses dynamic assets in frameless, borderless fullscreen mode. It isolates window drags to `.title-bar-drag-interface` to allow normal cursor text selection inside the chat windows.
2. **Web Server Backend**: Flask handles static routing, RAG telemetry queries (Local TF-IDF first, falling back to Wikipedia photo-sync lookups), voice assistant playback (STT/TTS), and administrative CRUD operations.
3. **Spacecraft Catalog**: A dedicated two-column view inside the new **Mission History** tab showcases the 220+ missions, retrieving live internet photos when connected and falling back to preloaded aerospace illustrations when offline.

---

## 📁 Source Code Files

### 1. Backend Server & Build Scripts
*   👉 **[app.py](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/app.py)**
    *   *Role*: Handles Flask routes, local TF-IDF search, voice synthesis, Google Translate, Wikipedia media extraction, and admin authentication endpoints.
*   👉 **[build_exe.py](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/build_exe.py)**
    *   *Role*: Bundles backend scripts, static frontend web assets, and databases into a single standalone `.exe` using PyInstaller.

### 2. Frontend Assets
*   👉 **[static/index.html](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/index.html)**
    *   *Role*: HTML layout containing the left navigation menu, the chat area, the new **Mission History** catalog layout, and the administrator Control Center modal with image uploader inputs.
*   👉 **[static/app.js](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/app.js)**
    *   *Role*: Javascript controller orchestrating chat transmissions, tab switches, dynamic mission catalog listings, Wikipedia photo fetching, voice helper playback (STT/TTS), and administrative image upload handlers.
*   👉 **[static/style.css](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/style.css)**
    *   *Role*: CSS styling system providing deep-space starry backgrounds, glassmorphic card telemetry grids, and animations.
*   👉 **[static/isro_history_reference.md](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/isro_history_reference.md)**
    *   *Role*: In-app reference document detailing the complete ISRO history and mission profiles.

### 3. Database & Populating Scripts
*   👉 **[data/knowledge_base.json](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/data/knowledge_base.json)**
    *   *Role*: Local JSON telemetry database containing 279 Q&A entries.
*   👉 **[data/populate_kb.py](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/data/populate_kb.py)**
    *   *Role*: Generator script that parses and populates the local knowledge base JSON with satellite and launcher flight metrics.

---

## ⚙️ Standalone Compiled Executable Binary
*   👉 **[ISRO-INTERNSHIP PROJECT/isro_chatbot.exe](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/ISRO-INTERNSHIP%20PROJECT/isro_chatbot.exe)**
*   👉 **[isro-chatbot-web/Antigravity-x64/isro_chatbot.exe](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/Antigravity-x64/isro_chatbot.exe)**
