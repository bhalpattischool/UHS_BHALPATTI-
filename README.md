<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <title>Study AI – Premium Smart Search</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    /* Reset & Base */
    * { margin:0; padding:0; box-sizing:border-box }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f0f4ff;
      color: #1a1a2e;
    }
    header {
      background: linear-gradient(90deg, #4b6cb7, #182848);
      color: white;
      padding: 1rem;
      text-align: center;
      font-size: 1.5rem;
      position: sticky; top:0; z-index:100;
      box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    }
    .btn {
      background: rgba(255,255,255,0.85);
      color: #182848;
      border: none;
      padding: 8px 14px;
      margin:0 4px;
      border-radius: 20px;
      cursor: pointer;
      font-weight: 600;
      transition: 0.2s;
    }
    .btn:hover { background: rgba(255,255,255,1); }
    .container {
      max-width: 900px;
      margin: 2rem auto;
      padding: 0 1rem;
    }
    /* Search Box */
    .search-bar {
      display: flex; gap: 8px; margin-bottom:1.5rem;
    }
    .search-bar input {
      flex: 1;
      padding:12px 16px;
      border-radius: 25px;
      border: 1px solid #ccc;
      font-size: 1rem;
      outline: none;
      background: rgba(255,255,255,0.9);
    }
    /* Loader */
    .loader {
      text-align:center; margin-bottom:1rem;
      color:#555; font-weight:500;
    }
    /* AI Summary */
    #aiSummarySec { margin-bottom:2rem; }
    #aiSummarySec h2 {
      color:#4b6cb7; margin-bottom:0.5rem;
      border-left:4px solid #4b6cb7; padding-left:8px;
      font-size:1.2rem;
    }
    .summary-box {
      background: rgba(75,108,183,0.1);
      padding:1rem 1.2rem;
      border-radius:8px;
      line-height:1.6;
      position:relative;
    }
    .summary-box p { margin-bottom:0.8rem; }
    .summary-box p:nth-child(odd) { font-weight:600; }
    /* Sources */
    #sourceList h2 {
      color:#182848; margin-bottom:0.5rem;
      border-left:4px solid #182848; padding-left:8px;
      font-size:1.2rem;
    }
    .card {
      background: white;
      margin-bottom:1rem;
      padding:1rem;
      border-radius:10px;
      box-shadow:0 2px 8px rgba(0,0,0,0.1);
      display:flex; flex-direction:column; gap:4px;
      transition:0.2s;
    }
    .card:hover {
      box-shadow:0 4px 16px rgba(0,0,0,0.15);
    }
    .card strong {
      color:#4b6cb7; font-size:1.05rem;
    }
    .card a {
      color:#182848; text-decoration:underline; font-size:0.9rem;
      margin-top:4px;
    }
    /* Settings Panel */
    .settings-panel {
      position:fixed; top:0; right:-360px;
      width:320px; height:100vh;
      background: #fff; box-shadow:-4px 0 12px rgba(0,0,0,0.15);
      padding:1.5rem; transition:right 0.3s ease;
      z-index:200;
    }
    .settings-panel.open { right:0; }
    .settings-panel h2 {
      margin-top:0; color:#4b6cb7; margin-bottom:1rem;
      font-size:1.2rem;
    }
    .settings-panel .close-btn {
      position:absolute; top:12px; right:12px;
      background:none; border:none; font-size:1.4rem; color:#4b6cb7;
      cursor:pointer;
    }
    .settings-panel label {
      margin-top:1rem; font-weight:600; display:block;
    }
    .settings-panel input, .settings-panel select {
      width:100%; padding:10px; margin-top:6px;
      border:1px solid #ccc; border-radius:6px;
      font-size:0.95rem;
    }
    .settings-panel .save-btn {
      margin-top:1.5rem; width:100%; text-align:center;
    }
    .status {
      margin-top:0.8rem; color:green; font-size:0.9rem;
      font-weight:600;
    }
    /* History Panel */
    .history-panel {
      position:fixed; top:4rem; left:-280px;
      width:260px; background:#fff;
      box-shadow:2px 0 12px rgba(0,0,0,0.12);
      padding:1rem; transition:left 0.3s ease; z-index:200;
      max-height:70vh; overflow-y:auto;
    }
    .history-panel.open { left:0; }
    .history-panel h3 {
      margin-top:0; color:#4b6cb7; margin-bottom:1rem;
    }
    .history-item {
      padding:6px 8px; border-bottom:1px solid #ececec;
      cursor:pointer; font-size:0.94rem;
    }
    .history-item:hover { background:#f0f8ff; }
  </style>
</head>
<body>
  <header>
    Study AI – Premium Smart Search
    <button class="btn" onclick="toggleHistory()">📜 History</button>
    <button class="btn" onclick="openSettings()">⚙️ Settings</button>
  </header>
  <div class="container">
    <div class="search-bar" id="searchBar">
      <input type="text" id="q" placeholder="अपना सवाल पूछें...">
      <button class="btn" onclick="handleSearch()">🔍</button>
    </div>
    <div id="loading" class="loader"></div>

    <div id="aiSummarySec">
      <h2>🧠 AI सारांश (Gemini)</h2>
      <div class="summary-box" id="aiSummary"><p>यहां सारांश दिखाई देगा...</p></div>
    </div>

    <div>
      <h2>🔗 स्रोत लिंक (Serper)</h2>
      <div id="sourceList"></div>
    </div>
  </div>

  <!-- Settings Panel -->
  <div class="settings-panel" id="settingsPanel">
    <button class="close-btn" onclick="closeSettings()">×</button>
    <h2>Settings</h2>
    <label>लक्ष्य परीक्षा</label>
    <select id="goal"><option>SSC CGL</option><option>UPSC</option></select>
    <label>स्थान</label>
    <input type="text" id="location" placeholder="जैसे: बिहार">
    <label>भाषा</label>
    <select id="lang"><option value="hi">हिन्दी</option><option value="en">English</option></select>
    <button class="btn save-btn" onclick="saveSettings()">Save Settings</button>
    <div id="statusText" class="status"></div>
  </div>

  <!-- History Panel -->
  <div class="history-panel" id="historyPanel">
    <h3>Search History</h3>
    <div id="historyList"></div>
  </div>

  <script>
    // API Keys & URLs
    const SERPER_KEY = "b74b5fa4a6751f6de080c7a5c2430e1169f18582";
    const SERPER_URL = "https://google.serper.dev/search";
    const GEMINI_KEY = "AIzaSyAeViJ0JXduMYGZJhP1zIzljKuaASdIkx4";
    const GEMINI_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${GEMINI_KEY}`;

    // Panels
    function openSettings(){document.getElementById('settingsPanel').classList.add('open');}
    function closeSettings(){document.getElementById('settingsPanel').classList.remove('open');document.getElementById('statusText').textContent='';}
    function toggleHistory(){document.getElementById('historyPanel').classList.toggle('open');showHistory();}

    // Save Settings
    function saveSettings(){
      const prefs = {
        goal: document.getElementById('goal').value,
        location: document.getElementById('location').value||"India",
        lang: document.getElementById('lang').value
      };
      localStorage.setItem('prefs', JSON.stringify(prefs));
      document.getElementById('statusText').textContent = "Settings saved ✅";
      closeSettings();handleSearch();
    }

    // History
    function saveHistory(q){
      if(!q) return;
      let h = JSON.parse(localStorage.getItem('hist')||'[]');
      h.unshift(q);h=[...new Set(h)].slice(0,10);
      localStorage.setItem('hist',JSON.stringify(h));
    }
    function showHistory(){
      let h=JSON.parse(localStorage.getItem('hist')||'[]');
      const list=h.map(q=>`<div class="history-item" onclick="useHistory('${q}')">${q}</div>`).join('');
      document.getElementById('historyList').innerHTML = list||'<p style="color:#555">No history</p>';
    }
    function useHistory(q){
      document.getElementById('q').value=q;
      toggleHistory();
      handleSearch();
    }

    // Main Search & Display
    async function handleSearch(){
      const query=document.getElementById('q').value.trim(); if(!query) return;
      saveHistory(query);
      document.getElementById('loading').textContent="Loading...";
      document.getElementById('aiSummary').innerHTML="<p>Loading AI summary...</p>";
      document.getElementById('sourceList').innerHTML="";

      const prefs=JSON.parse(localStorage.getItem('prefs')||'{}');
      const serperBody=JSON.stringify({q:query,location:prefs.location||"India",gl:"in",hl:prefs.lang||"hi"});
      let items=[];
      try{
        const r=await fetch(SERPER_URL,{method:'POST',headers:{'X-API-KEY':SERPER_KEY,'Content-Type':'application/json'},body:serperBody});
        const d=await r.json();
        items=[...(d.organic||[]),...(d.news||[])].slice(0,6);
      }catch(e){
        document.getElementById('loading').textContent="Serper fetch error";
        console.error(e);return;
      }
      document.getElementById('loading').textContent="";
      // show sources
      document.getElementById('sourceList').innerHTML = items.map((it,i)=>`
        <div class="card"><strong>${i+1}. ${it.title}</strong>
        <p>${it.snippet||''}</p>
        <a href="${it.link}" target="_blank">Link</a></div>`).join('');

      // prepare Gemini prompt
      const prompt = `User asked: "${query}"\n\nSources:\n`+
        items.map((it,i)=>`${i+1}. ${it.title}\n${it.snippet||''}\n${it.link}`).join("\n\n")+
        `\n\nProvide a concise Hindi summary answer based on these sources.`;
      try{
        const gres=await fetch(GEMINI_URL,{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({contents:[{parts:[{text:prompt}]}]})});
        const gdata=await gres.json();
        const sum = gdata?.candidates?.[0]?.content?.parts?.[0]?.text || "No AI answer.";
        document.getElementById('aiSummary').innerHTML = sum.split('\n').map(p=>`<p>${p}</p>`).join('');
      }catch(e){
        console.error(e);
        document.getElementById('aiSummary').innerHTML="<p>AI fetch error</p>";
      }
    }

    // Load initial
    window.onload=()=>{ if(!localStorage.getItem('prefs')) saveSettings(); handleSearch(); };
  </script>
</body>
</html>            margin: 10px;
            max-height: 85%;
            display: flex;
            flex-direction: column;
        }

        .dark-mode .chat-box {
            background-color: #2d2d2d;
            color: #f0f0f0;
        }

        .message {
            padding: 15px 20px;
            border-radius: 12px;
            margin-bottom: 15px;
            max-width: 80%;
            word-wrap: break-word;
            animation: fadeIn 0.3s ease-in-out;
            font-size: 17px;
        }

        .user-message {
            background-color: #f1f1f1;
            color: black;
            align-self: flex-end;
        }

        .dark-mode .user-message {
            background-color: #444444;
            color: #f0f0f0;
        }

        .bot-message {
            background-color: white;
            color: #333;
            align-self: flex-start;
            white-space: pre-wrap;
        }

        .dark-mode .bot-message {
            background-color: #444444;
            color:#f0f0f0 ;
        }

        .chat-input {
            display: flex;
            padding: 15px;
            background-color: #0077b6;
            position: fixed;
            bottom: 0;
            width: 100%;
            box-sizing: border-box;
        }

        .dark-mode .chat-input {
            background-color: #005f8a;
        }

        .chat-input input {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            outline: none;
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
        }

        .dark-mode .chat-input input {
            background-color: #444;
            color: #f0f0f0;
        }

        .chat-input button {
            padding: 10px 20px;
            background-color: #00f2fe;
            color: black;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            margin-left: 10px;
            transition: background-color 0.3s ease;
        }

        .dark-mode .chat-input button {
            background-color: #00c4cc;
            color: #f0f0f0;
        }

        .chat-input button:hover {
            background-color: #00c4cc;
        }

        .new-chat-button {
            position: absolute;
            bottom: 90px;
            right: 20px;
            cursor: pointer;
            font-size: 25px;
            color: #0077b6;
            background-color: #ffffff;
            border: 2px solid #0077b6;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            outline: none;
        }

        .dark-mode .new-chat-button {
            background-color: #3a3a3a;
            color: #f0f0f0;
            border-color: #f0f0f0;
        }

        .new-chat-button:hover {
            background-color: #0077b6;
            color: white;
        }

        .history-menu {
            position: absolute;
            top: 15px;
            right: 15px;
            cursor: pointer;
            font-size: 24px;
            color: white;
        }

        .history-popup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
            width: 80%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
            z-index: 1000;
        }

        .dark-mode .history-popup {
            background-color: #2d2d2d;
            color: #f0f0f0;
        }

        .history-popup h3 {
            margin-top: 0;
            color: #0077b6;
        }

        .dark-mode .history-popup h3 {
            color: #00c4cc;
        }

        .history-popup ul {
            list-style-type: none;
            padding: 0;
        }

        .history-popup li {
            padding: 10px;
            border-bottom: 1px solid #ddd;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .dark-mode .history-popup li {
            border-bottom: 1px solid #444;
        }

        .history-popup li:hover {
            background-color: #f0f4f8;
        }

        .dark-mode .history-popup li:hover {
            background-color: #444;
        }

        .delete-button, .edit-button {
            background-color: #ff4444;
            color: white;
            border: none;
            border-radius: 4px;
            padding: 5px 10px;
            cursor: pointer;
            font-size: 12px;
            margin-left: 5px;
        }

        .edit-button {
            background-color: #0077b6;
        }

        .delete-button:hover {
            background-color: #cc0000;
        }

        .edit-button:hover {
            background-color: #005f8a;
        }

        .close-button {
            position: absolute;
            top: 10px;
            right: 10px;
            cursor: pointer;
            font-size: 20px;
            color: #333;
        }

        .dark-mode .close-button {
            color: #f0f0f0;
        }

        .overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 999;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .loading-dots {
            display: inline-block;
            width: 80px;
            text-align: left;
        }

        .loading-dots::after {
            content: '';
            animation: dots 1.5s infinite;
        }

        @keyframes dots {
            0%, 20% { content: '.'; }
            40% { content: '..'; }
            60% { content: '...'; }
            80%, 100% { content: ''; }
        }

        .typing-animation {
            display: inline-block;
            overflow: hidden;
            white-space: pre-wrap;
        }

        @keyframes typing {
            from { width: 0; }
            to { width: 100%; }
        }

        .dark-mode-toggle {
            position: absolute;
            top: 10px;
            right: 45px;
            cursor: pointer;
            font-size: 26px;
            color: white;
        }

        .dark-mode-toggle:hover {
            color: #00c4cc;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="chat-header">
            Ajit's AI for Study 📚🤓
            <div class="history-menu" onclick="openHistory()">&#8942;</div>
            <div class="dark-mode-toggle" onclick="toggleDarkMode()">☀️</div>
        </div>
        <div class="chat-box" id="chat-box"></div>
    </div>

    <div class="chat-input">
        <input type="text" id="user-input" placeholder="Ask anything...">
        <button onclick="sendMessage()">Send</button>
    </div>

    <button class="new-chat-button" onclick="newChat()">+</button>

    <!-- चैट हिस्ट्री पॉपअप -->
    <div class="overlay" id="overlay"></div>
    <div class="history-popup" id="history-popup">
        <div class="close-button" onclick="closeHistory()">&times;</div>
        <h3>Chat history</h3>
        <ul id="history-list"></ul>
    </div>

    <script>
        // JavaScript कोड यहां
        const API_KEY = "AIzaSyBLyEG8u3WG-3ku7Z_SdMlndGqTBtwFXfo";
        let chatHistory = [];
        let currentSessionId = null;
        let db;
        let isDarkMode = false;

        // IndexedDB को इनिशियलाइज़ करें
        const initDB = () => {
            const request = indexedDB.open("ChatDB", 1);

            request.onupgradeneeded = (event) => {
                db = event.target.result;
                if (!db.objectStoreNames.contains("chatSessions")) {
                    db.createObjectStore("chatSessions", { keyPath: "id" });
                }
            };

            request.onsuccess = (event) => {
                db = event.target.result;
                loadChatHistory();
            };

            request.onerror = (event) => {
                console.error("IndexedDB error:", event.target.error);
            };
        };

        // चैट हिस्ट्री लोड करें
        const loadChatHistory = () => {
            const transaction = db.transaction(["chatSessions"], "readonly");
            const store = transaction.objectStore("chatSessions");
            const request = store.getAll();

            request.onsuccess = (event) => {
                const sessions = event.target.result;
                if (sessions.length > 0) {
                    currentSessionId = sessions[sessions.length - 1].id;
                    chatHistory = sessions[sessions.length - 1].history;
                    renderChatHistory();
                }
            };

            request.onerror = (event) => {
                console.error("Error loading chat history:", event.target.error);
            };
        };

        // चैट हिस्ट्री रेंडर करें
        const renderChatHistory = () => {
            const chatBox = document.getElementById("chat-box");
            chatBox.innerHTML = "";
            chatHistory.forEach(message => {
                const messageDiv = document.createElement("div");
                messageDiv.classList.add("message", message.role === "user" ? "user-message" : "bot-message");
                messageDiv.innerHTML = formatText(message.parts[0].text);
                chatBox.appendChild(messageDiv);
            });
            chatBox.scrollTop = chatBox.scrollHeight;
        };

        // मैसेज भेजें
        const sendMessage = async () => {
            const userInput = document.getElementById("user-input").value.trim();
            if (userInput === "") return;

            const chatBox = document.getElementById("chat-box");

            const userMessage = document.createElement("div");
            userMessage.classList.add("message", "user-message");
            userMessage.textContent = userInput;
            chatBox.appendChild(userMessage);

            const loadingMessage = document.createElement("div");
            loadingMessage.classList.add("message", "bot-message");
            loadingMessage.innerHTML = `<span class="loading-dots">wait</span>`;
            chatBox.appendChild(loadingMessage);

            chatBox.scrollTop = chatBox.scrollHeight;

            chatHistory.push({ role: "user", parts: [{ text: userInput }] });

            try {
                const botResponse = await getBotResponse(chatHistory);
                chatBox.removeChild(loadingMessage);

                const botMessage = document.createElement("div");
                botMessage.classList.add("message", "bot-message");
                botMessage.innerHTML = `<span class="typing-animation">${formatText(botResponse)}</span>`;
                chatBox.appendChild(botMessage);

                // टाइपिंग एनिमेशन लागू करें
                typeText(botMessage.querySelector(".typing-animation"), botResponse);

                chatHistory.push({ role: "model", parts: [{ text: botResponse }] });
                saveChatSession();
                chatBox.scrollTop = chatBox.scrollHeight;
            } catch (error) {
                chatBox.removeChild(loadingMessage);
                const errorMessage = document.createElement("div");
                errorMessage.classList.add("message", "bot-message");
                errorMessage.textContent = "मुझे माफ करें, मैं इस समय जवाब देने में असमर्थ हूँ!";
                chatBox.appendChild(errorMessage);
                chatBox.scrollTop = chatBox.scrollHeight;
            }

            document.getElementById("user-input").value = "";
        };

        // बॉट से रिस्पॉन्स प्राप्त करें
        const getBotResponse = async (history) => {
            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${API_KEY}`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        contents: history
                    })
                });

                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }

                const data = await response.json();
                return data.candidates[0].content.parts[0].text;
            } catch (error) {
                console.error("Error fetching bot response:", error);
            }
        };

        // टेक्स्ट फॉर्मेट करें
        const formatText = (text) => {
            return text.replace(/\*\*(.*?)\*\*/g, "<strong>$1</strong>")
                      .replace(/_(.*?)_/g, "<em>$1</em>")
                      .replace(/\n/g, "<br>");
        };

        // टेक्स्ट को धीरे-धीरे टाइप करें
        const typeText = (element, text) => {
            let index = 0;
            const speed = 1; // टाइपिंग स्पीड (मिलीसेकंड में)
            const formattedText = formatText(text);

            const type = () => {
                if (index < formattedText.length) {
                    element.innerHTML = formattedText.substring(0, index + 1);
                    index++;
                    setTimeout(type, speed);
                }
            };

            type();
        };

        // नई चैट शुरू करें
        const newChat = () => {
            if (chatHistory.length > 0 && confirm('क्या आप वाकई नई चैट शुरू करना चाहते हैं?')) {
                currentSessionId = Date.now().toString();
                chatHistory = [];
                document.getElementById("chat-box").innerHTML = "";
            }
        };

        // चैट सेशन को सेव करें
        const saveChatSession = () => {
            if (!currentSessionId) {
                currentSessionId = Date.now().toString();
            }

            const transaction = db.transaction(["chatSessions"], "readwrite");
            const store = transaction.objectStore("chatSessions");
            const title = generateChatTitle(chatHistory);
            const session = { id: currentSessionId, title: title, history: chatHistory };

            const request = store.put(session);

            request.onsuccess = () => {
                console.log("Chat session saved successfully");
            };

            request.onerror = (event) => {
                console.error("Error saving chat session:", event.target.error);
            };
        };

        // चैट टाइटल जेनरेट करें
        const generateChatTitle = (history) => {
            const firstMessage = history.find(message => message.role === "user")?.parts[0]?.text || "नई चैट";
            return firstMessage.split(" ").slice(0, 5).join(" ") + "...";
        };

        // चैट हिस्ट्री पॉपअप खोलें
        const openHistory = () => {
            document.getElementById("overlay").style.display = "block";
            document.getElementById("history-popup").style.display = "block";
            loadHistoryList();
        };

        // चैट हिस्ट्री पॉपअप बंद करें
        const closeHistory = () => {
            document.getElementById("overlay").style.display = "none";
            document.getElementById("history-popup").style.display = "none";
        };

        // चैट हिस्ट्री लिस्ट लोड करें
        const loadHistoryList = () => {
            const historyList = document.getElementById("history-list");
            historyList.innerHTML = "";

            const transaction = db.transaction(["chatSessions"], "readonly");
            const store = transaction.objectStore("chatSessions");
            const request = store.getAll();

            request.onsuccess = (event) => {
                const sessions = event.target.result;
                sessions.forEach(session => {
                    const li = document.createElement("li");
                    li.textContent = session.title || `चैट ${session.id}`;
                    li.onclick = () => loadChatSession(session.id);

                    const editButton = document.createElement("button");
                    editButton.textContent = "Edit";
                    editButton.classList.add("edit-button");
                    editButton.onclick = (e) => {
                        e.stopPropagation();
                        const newTitle = prompt("Enter new title:", session.title);
                        if (newTitle) {
                            editChatTitle(session.id, newTitle);
                        }
                    };

                    const deleteButton = document.createElement("button");
                    deleteButton.textContent = "Delete";
                    deleteButton.classList.add("delete-button");
                    deleteButton.onclick = (e) => {
                        e.stopPropagation();
                        deleteChatSession(session.id);
                    };

                    li.appendChild(editButton);
                    li.appendChild(deleteButton);
                    historyList.appendChild(li);
                });
            };

            request.onerror = (event) => {
                console.error("Error loading chat history:", event.target.error);
            };
        };

        // चैट सेशन डिलीट करें
        const deleteChatSession = (sessionId) => {
            const transaction = db.transaction(["chatSessions"], "readwrite");
            const store = transaction.objectStore("chatSessions");
            const request = store.delete(sessionId);

            request.onsuccess = () => {
                loadHistoryList();
                if (currentSessionId === sessionId) {
                    currentSessionId = null;
                    chatHistory = [];
                    document.getElementById("chat-box").innerHTML = "";
                }
            };

            request.onerror = (event) => {
                console.error("Error deleting chat session:", event.target.error);
            };
        };
      // चैट टाइटल एडिट करें
        const editChatTitle = (sessionId, newTitle) => {
            const transaction = db.transaction(["chatSessions"], "readwrite");
            const store = transaction.objectStore("chatSessions");
            const request = store.get(sessionId);

            request.onsuccess = (event) => {
                const session = event.target.result;
                if (session) {
                    session.title = newTitle;
                    const updateRequest = store.put(session);

                    updateRequest.onsuccess = () => {
                        loadHistoryList();
                    };

                    updateRequest.onerror = (event) => {
                        console.error("Error updating chat title:", event.target.error);
                    };
                }
            };

            request.onerror = (event) => {
                console.error("Error fetching chat session:", event.target.error);
            };
        };

        // चैट सेशन लोड करें
        const loadChatSession = (sessionId) => {
            const transaction = db.transaction(["chatSessions"], "readonly");
            const store = transaction.objectStore("chatSessions");
            const request = store.get(sessionId);

            request.onsuccess = (event) => {
                const session = event.target.result;
                if (session) {
                    currentSessionId = sessionId;
                    chatHistory = session.history;
                    renderChatHistory();
                    closeHistory();
                }
            };

            request.onerror = (event) => {
                console.error("Error loading chat session:", event.target.error);
            };
        };

        // डार्क मोड टॉगल करें
        const toggleDarkMode = () => {
            isDarkMode = !isDarkMode;
            document.body.classList.toggle("dark-mode", isDarkMode);
        };

        // इनिशियलाइज़ेशन
        initDB();

        // इनपुट फ़ील्ड में एंटर की प्रेस करने पर मैसेज भेजें
        document.getElementById("user-input").addEventListener("keypress", (e) => {
            if (e.key === "Enter") {
                sendMessage();
            }
        });

        // लोडिंग डॉट्स एनिमेशन
        const loadingDots = document.querySelector(".loading-dots");
        if (loadingDots) {
            setInterval(() => {
                const dots = loadingDots.textContent.split("").filter(char => char === ".");
                loadingDots.textContent =  ".".repeat((dots.length + 1) % 4);
            }, 500);
        }
    </script>
</body>
</html>
