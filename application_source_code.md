# ISRO Offline AI Assistant - Source Code View

The source code for the main files of the application is presented below. You can copy the code directly or click the file links to open the original source files on your computer.

````carousel
```python
# app.py (Backend Server & Launch Wrapper)
# File Location: C:/Users/Padmaj Rathod/.gemini/antigravity/scratch/isro-chatbot-web/app.py

import os
import sys
import json
import uuid
import urllib.request
import urllib.parse
from flask import Flask, request, jsonify, send_from_directory
import webview

app = Flask(__name__, static_folder="static")
ADMIN_PASSWORD = "isro123"

# Real Wikipedia image fetching helper
def get_wiki_image(query):
    try:
        search_url = f"https://en.wikipedia.org/w/api.php?action=query&list=search&srsearch={urllib.parse.quote(query)}&format=json"
        req = urllib.request.Request(search_url, headers={"User-Agent": "ISROBot/1.0"})
        with urllib.request.urlopen(req, timeout=3) as res:
            search_data = json.loads(res.read().decode("utf-8"))
        search_results = search_data.get("query", {}).get("search", [])
        if not search_results:
            return None
        title = search_results[0]["title"]
        image_url = f"https://en.wikipedia.org/w/api.php?action=query&prop=pageimages&piprop=original&titles={urllib.parse.quote(title)}&format=json&redirects=true"
        req2 = urllib.request.Request(image_url, headers={"User-Agent": "ISROBot/1.0"})
        with urllib.request.urlopen(req2, timeout=3) as res2:
            page_data = json.loads(res2.read().decode("utf-8"))
        pages = page_data.get("query", {}).get("pages", {})
        for page_id in pages:
            original = pages[page_id].get("original", {})
            if "source" in original:
                return original["source"]
        return None
    except Exception:
        return None

# serving image helper API
@app.route("/api/mission/image", methods=["GET"])
def api_mission_image():
    query = request.args.get("q", "").strip()
    if not query:
        return jsonify({"image_url": "static/gaganyaan.jpg"})
    real_url = get_wiki_image(query)
    if real_url:
        return jsonify({"image_url": real_url, "source": "Wikipedia"})
    # Offline fallbacks
    name_lower = query.lower()
    fallback = "static/gaganyaan.jpg"
    if "aryabhata" in name_lower: fallback = "static/aryabhata.jpg"
    elif "chandrayaan" in name_lower: fallback = "static/chandrayaan.jpg"
    elif "mangalyaan" in name_lower or "mars" in name_lower: fallback = "static/mangalyaan.jpg"
    elif "pslv" in name_lower: fallback = "static/pslv.jpg"
    elif "gslv" in name_lower or "lvm3" in name_lower: fallback = "static/gslv.jpg"
    elif "sslv" in name_lower: fallback = "static/sslv.jpg"
    return jsonify({"image_url": fallback, "source": "Offline Fallback"})

# upload custom image API
@app.route("/api/admin/upload_image", methods=["POST"])
def api_admin_upload_image():
    auth_header = request.headers.get("Authorization", "")
    if auth_header != ADMIN_PASSWORD:
        return jsonify({"success": False, "message": "Unauthorized"}), 403
    if 'file' not in request.files:
        return jsonify({"success": False, "message": "No file uploaded"}), 400
    file = request.files['file']
    if file.filename == '':
        return jsonify({"success": False, "message": "Empty filename"}), 400
    upload_dir = os.path.join(app.static_folder, "uploaded")
    os.makedirs(upload_dir, exist_ok=True)
    filename = f"{uuid.uuid4()[:8]}_{file.filename}"
    file.save(os.path.join(upload_dir, filename))
    return jsonify({"success": True, "image_url": f"static/uploaded/{filename}"})

if __name__ == "__main__":
    # Start native frame wrapper in full screen with easy_drag=False
    # to support normal text selection
    webview.create_window("ISRO AI Assistant", app, fullscreen=True, easy_drag=False)
    webview.start()
```
<!-- slide -->
```html
<!-- index.html (Main Layout) -->
<!-- File Location: C:/Users/Padmaj Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ISRO AI Assistant - Mission Control Console</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Orbitron:wght@600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="custom-title-bar">
        <div class="title-bar-drag-interface" id="title-drag-zone"></div>
        <div class="title-bar-content">
            <div class="title-bar-left">
                <span class="title-app-name">ISRO AI Assistant</span>
            </div>
            <div class="title-bar-right">
                <div class="window-controls">
                    <button onclick="minimizeWindow()">Min</button>
                    <button onclick="toggleMaximizeWindow()">Max</button>
                    <button onclick="closeWindow()">Close</button>
                </div>
            </div>
        </div>
    </div>
    <div class="app-container">
        <aside class="sidebar">
            <nav class="nav-menu">
                <button class="nav-btn active" id="btn-chat" onclick="switchTab('chat')">Mission Chat</button>
                <button class="nav-btn" id="btn-history" onclick="switchTab('history')">Mission History</button>
                <button class="nav-btn" id="btn-admin" onclick="switchTab('admin')">Control Center</button>
            </nav>
        </aside>
        <main class="main-content">
            <section id="panel-chat" class="panel active">
                <div class="chat-container">
                    <div class="message-list" id="message-list"></div>
                    <form id="chat-form" onsubmit="handleChatSubmit(event)">
                        <input type="text" id="chat-input" placeholder="Ask anything about ISRO missions..." required>
                        <button type="submit">Send</button>
                    </form>
                </div>
            </section>
            <section id="panel-history" class="panel">
                <div class="history-catalog-layout">
                    <div class="history-list-column card">
                        <input type="text" id="catalog-search" placeholder="Search 220+ missions..." oninput="filterCatalogMissions()">
                        <div class="catalog-list" id="catalog-missions-list"></div>
                    </div>
                    <div class="history-detail-column card" id="catalog-detail-card">
                        <img src="gaganyaan.jpg" id="catalog-detail-img">
                        <h3 id="catalog-detail-title">Gaganyaan Mission</h3>
                        <p id="catalog-detail-desc"></p>
                    </div>
                </div>
            </section>
        </main>
    </div>
    <script src="app.js"></script>
</body>
</html>
```
<!-- slide -->
```javascript
// app.js (Frontend Controller Logic)
// File Location: C:/Users/Padmaj Rathod/.gemini/antigravity/scratch/isro-chatbot-web/static/app.js

let currentLanguage = 'en';

async function selectCatalogMission(name) {
    const details = getMissionDetails(name);
    
    document.getElementById('catalog-detail-title').innerText = name;
    document.getElementById('catalog-detail-desc').innerText = details.desc;
    document.getElementById('catalog-detail-img').src = details.image;

    // Fetch real photo dynamically
    try {
        const res = await fetch(`/api/mission/image?q=${encodeURIComponent(name)}`);
        const data = await res.json();
        if (data.image_url) {
            document.getElementById('catalog-detail-img').src = data.image_url;
        }
    } catch(e) {
        console.warn("Using local illustration fallback:", e);
    }
}

async function uploadEntryImage(event) {
    const file = event.target.files[0];
    if (!file) return;
    
    const formData = new FormData();
    formData.append('file', file);
    
    try {
        const res = await fetch('/api/admin/upload_image', {
            method: 'POST',
            headers: { 'Authorization': adminToken },
            body: formData
        });
        const data = await res.json();
        if (data.success) {
            document.getElementById('entry-image-url').value = data.image_url;
            document.getElementById('entry-image-preview').innerText = `Uploaded: ${file.name}`;
        }
    } catch(err) {
        console.error("Upload error:", err);
    }
}

function appendChatMessage(sender, text, meta = {}) {
    const list = document.getElementById('message-list');
    const msgDiv = document.createElement('div');
    msgDiv.className = `message ${sender}`;
    
    let imgHtml = "";
    if (meta.image_url) {
        imgHtml = `<div class="chat-attached-image"><img src="${meta.image_url}"></div>`;
    }
    
    msgDiv.innerHTML = `
        <div class="message-bubble">
            <div class="body">${text}</div>
            ${imgHtml}
        </div>
    `;
    list.appendChild(msgDiv);
    list.scrollTop = list.scrollHeight;
}
```
````
