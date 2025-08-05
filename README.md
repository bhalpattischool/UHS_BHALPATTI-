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
    .history-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }
    .history-header h3 {
      margin: 0;
      color:#4b6cb7;
    }
    .history-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px 10px;
      border-radius: 6px;
      margin-bottom: 6px;
      background: #f8faff;
      transition: all 0.2s;
    }
    .history-item:hover {
      background: #eef2ff;
    }
    .history-text {
      flex: 1;
      cursor: pointer;
      padding: 4px 0;
    }
    .history-actions {
      display: flex;
      gap: 6px;
    }
    .delete-btn {
      background: #ffeded;
      color: #ff6b6b;
      border: none;
      border-radius: 50%;
      width: 26px;
      height: 26px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.2s;
    }
    .delete-btn:hover {
      background: #ffdbdb;
      transform: scale(1.1);
    }
    .no-history {
      text-align: center;
      color: #777;
      padding: 10px;
      font-style: italic;
    }
    .clear-all-btn {
      background: #ffeded;
      color: #ff6b6b;
      border: none;
      border-radius: 4px;
      padding: 4px 8px;
      font-size: 0.8rem;
      cursor: pointer;
      transition: all 0.2s;
    }
    .clear-all-btn:hover {
      background: #ffdbdb;
    }
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
      <input type="text" id="q" placeholder="अपना सवाल पूछें..." onkeypress="handleKeyPress(event)">
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
    <div class="history-header">
      <h3>Search History</h3>
      <button class="clear-all-btn" onclick="clearAllHistory()">Clear All</button>
    </div>
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
      closeSettings();
      handleSearch();
    }

    // History functions
    function saveHistory(q){
      if(!q) return;
      let h = JSON.parse(localStorage.getItem('hist')||'[]');
      // Add new item at beginning and remove duplicates
      h = [q, ...h.filter(item => item !== q)].slice(0,10);
      localStorage.setItem('hist',JSON.stringify(h));
    }
    
    function showHistory(){
      let h = JSON.parse(localStorage.getItem('hist')||'[]');
      const historyList = document.getElementById('historyList');
      
      if(h.length === 0) {
        historyList.innerHTML = '<div class="no-history">No search history found</div>';
        return;
      }
      
      let listHtml = '';
      h.forEach((q, index) => {
        listHtml += `
          <div class="history-item">
            <div class="history-text" onclick="useHistory('${q.replace(/'/g, "\\'")}')">${q}</div>
            <div class="history-actions">
              <button class="delete-btn" onclick="deleteHistoryItem(${index})">×</button>
            </div>
          </div>
        `;
      });
      
      historyList.innerHTML = listHtml;
    }
    
    function useHistory(q){
      document.getElementById('q').value = q;
      handleSearch();
    }
    
    function deleteHistoryItem(index){
      let h = JSON.parse(localStorage.getItem('hist')||'[]');
      if(index >= 0 && index < h.length) {
        h.splice(index, 1);
        localStorage.setItem('hist', JSON.stringify(h));
        showHistory();
      }
    }
    
    function clearAllHistory(){
      if(confirm("Are you sure you want to clear all search history?")) {
        localStorage.removeItem('hist');
        showHistory();
      }
    }

    // Handle Enter key press
    function handleKeyPress(event) {
      if (event.key === "Enter") {
        handleSearch();
      }
    }

    // Main Search & Display
    async function handleSearch(){
      const query=document.getElementById('q').value.trim(); 
      if(!query) return;
      
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
    window.onload=()=>{ 
      if(!localStorage.getItem('prefs')) saveSettings(); 
      showHistory();
    };
  </script>
</body>
</html>
