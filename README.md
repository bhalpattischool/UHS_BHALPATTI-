<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ajit's AI for Study</title>
    <style>
        body {
            font-family: 'Open Sans', Arial, sans-serif;
            background-color: #f0f4f8;
            color: #333;
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            flex-direction: column;
        }

        .dark-mode {
            background-color: #1a1a1a;
            color: #f0f0f0;
        }

        .chat-container {
            display: flex;
            flex-direction: column;
            height: 100%;
            overflow: hidden;
            padding-bottom: 80px;
            position: relative;
        }

        .chat-header {
            background-color: #0077b6;
            color: white;
            padding: 15px;
            font-size: 20px;
            text-align: center;
            font-weight: bold;
            border-bottom: 2px solid #fff;
            flex-shrink: 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .chat-box {
            padding: 20px;
            background-color: #ffffff;
            border-radius: 12px;
            overflow-y: auto;
            flex-grow: 1;
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
            margin: 10px;
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
