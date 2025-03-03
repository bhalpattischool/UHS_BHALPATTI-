<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Local Group Chat</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css"></link>
    <style>
        body {
            font-family: 'Arial', sans-serif;
        }
    </style>
</head>
<body class="bg-gradient-to-r from-purple-500 to-blue-500 text-white text-center p-5">
    <h1 class="text-4xl mb-8">Local Group Chat</h1>
    <div class="container mx-auto max-w-md bg-white bg-opacity-10 p-6 rounded-lg shadow-lg">
        <div id="group-creation" class="mb-6">
            <input type="text" id="group-name" placeholder="Enter Group Name" class="w-full p-3 mb-4 rounded bg-white bg-opacity-80 text-gray-800">
            <input type="password" id="group-password" placeholder="Enter Group Password (Optional)" class="w-full p-3 mb-4 rounded bg-white bg-opacity-80 text-gray-800">
            <button onclick="createGroup()" class="w-full p-3 rounded bg-blue-600 hover:bg-purple-600 transition duration-300">Create Group</button>
        </div>
        <div id="join-group" class="mb-6">
            <input type="text" id="group-id" placeholder="Enter Group ID" class="w-full p-3 mb-4 rounded bg-white bg-opacity-80 text-gray-800">
            <input type="password" id="join-password" placeholder="Enter Group Password (If Any)" class="w-full p-3 mb-4 rounded bg-white bg-opacity-80 text-gray-800">
            <button onclick="joinGroup()" class="w-full p-3 rounded bg-blue-600 hover:bg-purple-600 transition duration-300">Join Group</button>
        </div>
        <div id="chat-box" class="hidden mt-6">
            <div id="chat-messages" class="h-72 overflow-y-auto bg-white bg-opacity-80 rounded p-4 mb-4 text-gray-800 text-left"></div>
            <input type="text" id="message" placeholder="Type a message..." class="w-full p-3 mb-4 rounded bg-white bg-opacity-80 text-gray-800">
            <button onclick="sendMessage()" class="w-full p-3 rounded bg-blue-600 hover:bg-purple-600 transition duration-300">Send</button>
        </div>
    </div>

    <script>
        let db;
        let currentGroupId = "";
        let username = "";

        // Initialize IndexedDB
        const initDB = () => {
            const request = indexedDB.open("GroupChatDB", 1);

            request.onupgradeneeded = (event) => {
                db = event.target.result;
                if (!db.objectStoreNames.contains("groups")) {
                    db.createObjectStore("groups", { keyPath: "id" });
                }
                if (!db.objectStoreNames.contains("messages")) {
                    const messagesStore = db.createObjectStore("messages", { keyPath: "id", autoIncrement: true });
                    messagesStore.createIndex("groupId", "groupId", { unique: false });
                }
            };

            request.onsuccess = (event) => {
                db = event.target.result;
                console.log("IndexedDB initialized successfully");
            };

            request.onerror = (event) => {
                console.error("Error initializing IndexedDB:", event.target.error);
            };
        };

        // Create Group
        const createGroup = () => {
            const groupName = document.getElementById("group-name").value;
            const groupPassword = document.getElementById("group-password").value;
            if (!groupName) return alert("Please enter a group name");

            const groupId = "group" + Math.random().toString(36).substring(7); // Generate unique group ID
            const transaction = db.transaction("groups", "readwrite");
            const store = transaction.objectStore("groups");
            store.add({ id: groupId, name: groupName, password: groupPassword, members: [] });

            alert(`Group created! Share this ID: ${groupId}`);
            joinGroup(groupId, groupPassword);
        };

        // Join Group
        const joinGroup = (groupId = "", password = "") => {
            if (!groupId) groupId = document.getElementById("group-id").value;
            if (!password) password = document.getElementById("join-password").value;

            const transaction = db.transaction("groups", "readonly");
            const store = transaction.objectStore("groups");
            const request = store.get(groupId);

            request.onsuccess = (event) => {
                const group = event.target.result;
                if (!group) return alert("Group not found");
                if (group.password && group.password !== password) return alert("Incorrect password");

                username = prompt("Enter your name");
                if (!username) return alert("Please enter your name");

                currentGroupId = groupId;
                document.getElementById("chat-box").classList.remove("hidden");

                // Load previous messages
                loadMessages();
            };

            request.onerror = (event) => {
                console.error("Error joining group:", event.target.error);
            };
        };

        // Load Messages
        const loadMessages = () => {
            const transaction = db.transaction("messages", "readonly");
            const store = transaction.objectStore("messages");
            const index = store.index("groupId");
            const request = index.getAll(currentGroupId);

            request.onsuccess = (event) => {
                const messages = event.target.result;
                const chatMessages = document.getElementById("chat-messages");
                chatMessages.innerHTML = "";
                messages.forEach((message) => {
                    chatMessages.innerHTML += `<div class="message mb-2"><strong class="text-blue-600">${message.user}:</strong> ${message.text}</div>`;
                });
            };

            request.onerror = (event) => {
                console.error("Error loading messages:", event.target.error);
            };
        };

        // Send Message
        const sendMessage = () => {
            const message = document.getElementById("message").value;
            if (!message) return;

            const transaction = db.transaction("messages", "readwrite");
            const store = transaction.objectStore("messages");
            store.add({ groupId: currentGroupId, user: username, text: message, timestamp: Date.now() });

            document.getElementById("message").value = "";
            loadMessages(); // Refresh messages
        };

        // Initialize DB on page load
        initDB();
    </script>
</body>
</html>
