# ISRO Offline AI Assistant - Source Code Index

This index outlines the complete architecture and lists the location of all source code files for the standalone **ISRO Offline AI Assistant** desktop application.

---

## 🏗️ Architecture Overview

The application is structured as a native desktop web wrapper client wrapping a local Flask server backend:
1. **Desktop Client Frame**: PyWebView parses dynamic assets in frameless, borderless fullscreen mode. It exposes window action APIs (minimize, toggle fullscreen, close) directly to the window context.
2. **Web Server Backend**: Flask handles static routing, RAG routing (Local TF-IDF first, falling back to duckduckgo web-search updates and local Ollama context endpoints), administrative CRUD operations, and translation APIs.
3. **Retrieval Engine**: Scikit-Learn TF-IDF vectorizer matches queries against a local JSON database and ranks responses based on cosine similarity scores.

---

## 📁 Source Code Files

### 1. Backend Server & Launcher
*   👉 **[app.py](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/app.py)**
    *   *Role*: Flask endpoints, local TF-IDF search bot class, DuckDuckGo search queries, Wikipedia image extractors, model selector routes, and PyWebView native launch.

### 2. Frontend Assets
*   👉 **[static/index.html](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/index.html)**
    *   *Role*: HTML layout containing the Apple Vision Pro glass sidebar navigation, collapsible suggested cards grid, and custom Windows 11 title bar controls.
*   👉 **[static/app.js](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/app.js)**
    *   *Role*: Dynamic controller mapping sidebar alerts, settings checkbox toggles, chat transmissions, on-demand Hindi translation API callbacks, and titlebar window actions.
*   👉 **[static/style.css](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/style.css)**
    *   *Role*: Premium dark color theme tokens (`#090B14`, `#161F31`), glassmorphic panels, glowing AI speech bubbles, custom drag boundaries, and flexbox auto-fitting.
*   👉 **[static/splash.html](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/splash.html)**
    *   *Role*: Space loading screen displaying the central glowing logo and two rotating saffron/green 3D orbiting rings.

### 3. Packaging & Specification
*   👉 **[build_exe.py](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/build_exe.py)**
    *   *Role*: PyInstaller build configuration script to compile the python files into a console-less standalone Windows binary.
*   👉 **[requirements.txt](file:///C:/Users/Padmaj%20Rathod/.gemini/antigravity/scratch/isro-chatbot-web/requirements.txt)**
    *   *Role*: Defines Python dependencies (`flask`, `pywebview`, `scikit-learn`).

---

## 🛠️ How to Recompile After Editing
To compile your changes into a new executable:
1. Open a PowerShell terminal.
2. Run:
   ```powershell
   cd "C:\Users\Padmaj Rathod\.gemini\antigravity\scratch\isro-chatbot-web"
   & "C:\Users\Padmaj Rathod\AppData\Local\Python\bin\python.exe" build_exe.py
   ```
3. Copy the compiled file `dist/isro_chatbot.exe` to your target directory folders.
