<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Study Master</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
        body {
            font-family: 'Roboto', sans-serif;
        }
        .priority-high { background-color: #fecaca; }
        .priority-medium { background-color: #fed7aa; }
        .priority-low { background-color: #bbf7d0; }
    </style>
</head>
<body class="bg-gradient-to-r from-orange-400 to-blue-300 min-h-screen p-5 text-gray-800">
    <div class="max-w-3xl mx-auto bg-white bg-opacity-95 p-8 rounded-2xl shadow-lg">
        <!-- Header -->
        <div class="flex justify-between items-center mb-8">
            <h1 class="text-4xl font-bold text-gray-800">📖 Study Guide</h1>
            <button class="bg-blue-400 text-white w-10 h-10 rounded-full flex items-center justify-center transform transition-transform duration-300 hover:rotate-360" onclick="showHelp()">?</button>
        </div>

        <!-- Task Input Section -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
            <input type="text" id="topic" placeholder="Add topic" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200">
            <input type="text" id="subject" placeholder="Add subject" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200">
            <select id="taskType" onchange="showTips()" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200">
                <option value="Revision">🔁 Revision</option>
                <option value="Notes">📝 Make notes</option>
                <option value="Reading">📖 Reading</option>
            </select>
            <input type="date" id="dueDate" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200">
            <select id="taskPriority" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200">
                <option value="high">🚨 High Priority</option>
                <option value="medium">⚠️ Medium Priority</option>
                <option value="low">✅ Low Priority</option>
            </select>
            <button onclick="addTask()" class="col-span-1 md:col-span-2 bg-orange-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-orange-500 transform transition-transform duration-300">Add task</button>
        </div>

        <!-- Task Tips Section -->
        <div class="bg-yellow-100 p-4 rounded-lg border-l-4 border-yellow-500 mb-6 hidden" id="taskTips"></div>

        <!-- Progress Section -->
        <div class="bg-white p-6 rounded-lg shadow-md mb-6">
            <p class="mb-2">Progress: <span id="progressText">0%</span></p>
            <progress id="progressBar" value="0" max="100" class="w-full h-5 rounded-lg"></progress>
        </div>

        <!-- Task List Section -->
        <div class="max-h-96 overflow-y-auto pr-2 mb-6" id="taskList"></div>
        <button onclick="completeAllTasks()" class="bg-teal-400 text-white p-3 rounded-lg mb-6 w-full uppercase tracking-wider hover:bg-teal-500 transform transition-transform duration-300">Complete all tasks</button>

        <!-- Pomodoro Timer Section -->
        <div class="bg-white p-6 rounded-lg shadow-md mb-6">
            <h2 class="text-2xl font-bold mb-4">Pomodoro Timer</h2>
            <div class="flex justify-center items-center mb-4">
                <span id="timer" class="text-4xl font-bold">25:00</span>
            </div>
            <div class="flex justify-center space-x-4 mb-4">
                <button onclick="startTimer()" class="bg-green-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-green-500 transform transition-transform duration-300">Start</button>
                <button onclick="pauseTimer()" class="bg-yellow-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-yellow-500 transform transition-transform duration-300">Pause</button>
                <button onclick="resetTimer()" class="bg-red-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-red-500 transform transition-transform duration-300">Reset</button>
            </div>
            <div class="flex justify-center space-x-4">
                <input type="number" id="customMinutes" placeholder="Minutes" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 w-24">
                <button onclick="setCustomTimer()" class="bg-blue-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-blue-500 transform transition-transform duration-300">Set Timer</button>
            </div>
        </div>

        <!-- Study Notes Section -->
        <div class="bg-white p-6 rounded-lg shadow-md mb-6">
            <h2 class="text-2xl font-bold mb-4">Study Notes</h2>
            <input type="text" id="noteTitle" placeholder="Note Title" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-2">
            <input type="text" id="noteSubject" placeholder="Subject" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-2">
            <textarea id="notes" rows="5" class="w-full p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-4" placeholder="Write your notes here..."></textarea>
            <button onclick="saveNote()" class="bg-blue-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-blue-500 transform transition-transform duration-300">Save Note</button>
            <div id="notesList" class="mt-4"></div>
        </div>

        <!-- Flashcards Section -->
        <div class="bg-white p-6 rounded-lg shadow-md mb-6">
            <h2 class="text-2xl font-bold mb-4">Flashcards</h2>
            <input type="text" id="flashcardQuestion" placeholder="Question" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-2">
            <input type="text" id="flashcardAnswer" placeholder="Answer" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-2">
            <button onclick="addFlashcard()" class="bg-green-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-green-500 transform transition-transform duration-300 mb-4">Add Flashcard</button>
            <div id="flashcardList" class="space-y-2"></div>
        </div>

        <!-- Daily Planner Section -->
        <div class="bg-white p-6 rounded-lg shadow-md mb-6">
            <h2 class="text-2xl font-bold mb-4">Daily Planner</h2>
            <input type="text" id="plannerTask" placeholder="Task" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-2">
            <input type="time" id="plannerTime" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-2">
            <button onclick="addPlannerTask()" class="bg-orange-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-orange-500 transform transition-transform duration-300 mb-4">Add Task</button>
            <div id="plannerList" class="space-y-2"></div>
        </div>

        <!-- Study Tracker Section -->
        <div class="bg-white p-6 rounded-lg shadow-md mb-6">
            <h2 class="text-2xl font-bold mb-4">Study Tracker</h2>
            <input type="date" id="studyDate" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-2">
            <input type="number" id="studyHours" placeholder="Hours Studied" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200 mb-2">
            <button onclick="addStudyLog()" class="bg-blue-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-blue-500 transform transition-transform duration-300 mb-4">Add Log</button>
            <div id="studyLogList" class="space-y-2"></div>
        </div>

        <!-- Motivational Quotes Section -->
        <div class="bg-white p-6 rounded-lg shadow-md mb-6">
            <h2 class="text-2xl font-bold mb-4">Motivational Quotes</h2>
            <div id="quote" class="text-center text-lg italic mb-4">"The only way to achieve the impossible is to believe it is possible."</div>
            <button onclick="generateQuote()" class="bg-green-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-green-500 transform transition-transform duration-300">New Quote</button>
        </div>

        <!-- Resource Links Section -->
        <div class="bg-white p-6 rounded-lg shadow-md">
            <h2 class="text-2xl font-bold mb-4">Resource Links</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                <input type="text" id="resourceTitle" placeholder="Resource Title" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200">
                <input type="url" id="resourceLink" placeholder="Resource Link" class="p-3 border-2 border-gray-300 rounded-lg focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-200">
            </div>
            <button onclick="addResource()" class="bg-green-400 text-white p-3 rounded-lg uppercase tracking-wider hover:bg-green-500 transform transition-transform duration-300 mb-4">Add Resource</button>
            <div id="resourceList" class="space-y-2"></div>
        </div>
    </div>

    <script>
      // IndexedDB setup
let db;
const request = indexedDB.open("StudyMasterDB", 1);

request.onupgradeneeded = function (event) {
    db = event.target.result;

    // Notes object store
    const notesStore = db.createObjectStore("notes", { keyPath: "id", autoIncrement: true });
    notesStore.createIndex("title", "title", { unique: false });
    notesStore.createIndex("subject", "subject", { unique: false });
    notesStore.createIndex("content", "content", { unique: false });

    // Tasks object store
    const tasksStore = db.createObjectStore("tasks", { keyPath: "id", autoIncrement: true });
    tasksStore.createIndex("topic", "topic", { unique: false });
    tasksStore.createIndex("subject", "subject", { unique: false });
    tasksStore.createIndex("taskType", "taskType", { unique: false });
    tasksStore.createIndex("dueDate", "dueDate", { unique: false });
    tasksStore.createIndex("priority", "priority", { unique: false });

    // Flashcards object store
    const flashcardsStore = db.createObjectStore("flashcards", { keyPath: "id", autoIncrement: true });
    flashcardsStore.createIndex("question", "question", { unique: false });
    flashcardsStore.createIndex("answer", "answer", { unique: false });

    // Planner object store
    const plannerStore = db.createObjectStore("planner", { keyPath: "id", autoIncrement: true });
    plannerStore.createIndex("task", "task", { unique: false });
    plannerStore.createIndex("time", "time", { unique: false });

    // Study Logs object store
    const studyLogsStore = db.createObjectStore("studyLogs", { keyPath: "id", autoIncrement: true });
    studyLogsStore.createIndex("date", "date", { unique: false });
    studyLogsStore.createIndex("hours", "hours", { unique: false });

    // Resources object store
    const resourcesStore = db.createObjectStore("resources", { keyPath: "id", autoIncrement: true });
    resourcesStore.createIndex("title", "title", { unique: false });
    resourcesStore.createIndex("link", "link", { unique: false });
};

request.onsuccess = function (event) {
    db = event.target.result;
    loadNotes();
    loadTasks();
    loadFlashcards();
    loadPlannerTasks();
    loadStudyLogs();
    loadResources();
};

request.onerror = function (event) {
    console.error("Database error: ", event.target.error);
};

// Notes functions
function saveNote() {
    const title = document.getElementById('noteTitle').value;
    const subject = document.getElementById('noteSubject').value;
    const content = document.getElementById('notes').value;

    if (!title || !subject || !content) return;

    const transaction = db.transaction(["notes"], "readwrite");
    const objectStore = transaction.objectStore("notes");
    objectStore.add({ title, subject, content });

    transaction.oncomplete = function () {
        clearNoteInputs();
        loadNotes();
    };
}

function loadNotes() {
    const transaction = db.transaction(["notes"], "readonly");
    const objectStore = transaction.objectStore("notes");
    const request = objectStore.getAll();

    request.onsuccess = function (event) {
        const notes = event.target.result;
        const notesList = document.getElementById('notesList');
        notesList.innerHTML = '';
        notes.forEach(note => {
            const noteItem = document.createElement('div');
            noteItem.className = 'note-item bg-gray-100 p-3 rounded-lg flex justify-between items-center mb-2';
            noteItem.innerHTML = `
                <div>
                    <h3 class="font-semibold">${note.title} (${note.subject})</h3>
                    <p>${note.content}</p>
                </div>
                <div class="note-actions flex space-x-2">
                    <button onclick="editNote(${note.id})" class="bg-orange-400 text-white p-2 rounded-lg">✎</button>
                    <button onclick="deleteNote(${note.id})" class="bg-red-400 text-white p-2 rounded-lg">✖</button>
                </div>
            `;
            notesList.appendChild(noteItem);
        });
    };
}

function editNote(id) {
    const transaction = db.transaction(["notes"], "readonly");
    const objectStore = transaction.objectStore("notes");
    const request = objectStore.get(id);

    request.onsuccess = function (event) {
        const note = event.target.result;
        document.getElementById('noteTitle').value = note.title;
        document.getElementById('noteSubject').value = note.subject;
        document.getElementById('notes').value = note.content;

        const saveButton = document.querySelector('button[onclick="saveNote()"]');
        saveButton.setAttribute('onclick', `updateNote(${id})`);
        saveButton.innerText = 'Update Note';
    };
}

function updateNote(id) {
    const title = document.getElementById('noteTitle').value;
    const subject = document.getElementById('noteSubject').value;
    const content = document.getElementById('notes').value;

    const transaction = db.transaction(["notes"], "readwrite");
    const objectStore = transaction.objectStore("notes");
    objectStore.put({ id, title, subject, content });

    transaction.oncomplete = function () {
        clearNoteInputs();
        loadNotes();
        const saveButton = document.querySelector('button[onclick^="updateNote"]');
        saveButton.setAttribute('onclick', 'saveNote()');
        saveButton.innerText = 'Save Note';
    };
}

function deleteNote(id) {
    const transaction = db.transaction(["notes"], "readwrite");
    const objectStore = transaction.objectStore("notes");
    objectStore.delete(id);

    transaction.oncomplete = function () {
        loadNotes();
    };
}

function clearNoteInputs() {
    document.getElementById('noteTitle').value = '';
    document.getElementById('noteSubject').value = '';
    document.getElementById('notes').value = '';
}

// Tasks functions
function addTask() {
    const topic = document.getElementById('topic').value;
    const subject = document.getElementById('subject').value;
    const taskType = document.getElementById('taskType').value;
    const dueDate = document.getElementById('dueDate').value;
    const priority = document.getElementById('taskPriority').value;

    if (!topic || !subject || !taskType || !dueDate) return;

    const task = { topic, subject, taskType, dueDate, priority };
    const transaction = db.transaction(["tasks"], "readwrite");
    const objectStore = transaction.objectStore("tasks");
    objectStore.add(task);

    transaction.oncomplete = function () {
        displayTasks();
        clearTaskInputs();
    };
}

function loadTasks() {
    const transaction = db.transaction(["tasks"], "readonly");
    const objectStore = transaction.objectStore("tasks");
    const request = objectStore.getAll();

    request.onsuccess = function (event) {
        tasks = event.target.result;
        displayTasks();
    };
}

function displayTasks() {
    const taskList = document.getElementById('taskList');
    taskList.innerHTML = '';
    tasks.forEach(task => {
        const taskItem = document.createElement('div');
        taskItem.className = `task-item bg-gray-100 p-3 rounded-lg flex justify-between items-center mb-2 ${task.priority === 'high' ? 'priority-high' : task.priority === 'medium' ? 'priority-medium' : 'priority-low'}`;
        taskItem.innerHTML = `
            <div>
                <h3 class="font-semibold">${task.topic} (${task.subject})</h3>
                <p>${task.taskType} - Due: ${task.dueDate}</p>
            </div>
            <button onclick="deleteTask(${task.id})" class="bg-red-400 text-white p-2 rounded-lg">✖</button>
        `;
        taskList.appendChild(taskItem);
    });
}

function deleteTask(id) {
    const transaction = db.transaction(["tasks"], "readwrite");
    const objectStore = transaction.objectStore("tasks");
    objectStore.delete(id);

    transaction.oncomplete = function () {
        loadTasks();
    };
}

function clearTaskInputs() {
    document.getElementById('topic').value = '';
    document.getElementById('subject').value = '';
    document.getElementById('dueDate').value = '';
}

