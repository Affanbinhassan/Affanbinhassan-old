<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Batcomputer - Affan bin Hassan</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #000;
            color: #00ffff;
            font-family: 'Courier New', monospace;
            overflow-x: hidden;
            position: relative;
            min-height: 100vh;
        }

        .grid-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: 
                linear-gradient(rgba(0, 255, 255, 0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 255, 255, 0.03) 1px, transparent 1px);
            background-size: 20px 20px;
            pointer-events: none;
            z-index: 1;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
            position: relative;
            z-index: 2;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            border-bottom: 2px solid #00ffff;
            padding-bottom: 20px;
            animation: borderPulse 2s infinite;
        }

        @keyframes borderPulse {
            0%, 100% { box-shadow: 0 0 10px rgba(0, 255, 255, 0.3); }
            50% { box-shadow: 0 0 20px rgba(0, 255, 255, 0.6); }
        }

        .bat-logo {
            width: 120px;
            height: 120px;
            margin: 20px auto;
            display: block;
            filter: drop-shadow(0 0 15px #00ffff);
            animation: batPulse 1.5s infinite;
        }

        @keyframes batPulse {
            0%, 100% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.05); opacity: 0.8; }
        }

        .system-time {
            font-size: 56px;
            font-weight: bold;
            text-shadow: 0 0 20px #00ffff;
            margin: 15px 0;
            letter-spacing: 3px;
        }

        .system-date {
            font-size: 22px;
            color: #ffd43b;
            margin-bottom: 15px;
            text-transform: uppercase;
        }

        .greeting {
            font-size: 20px;
            color: #ffd43b;
            margin: 10px 0;
            animation: flicker 3s infinite;
        }

        @keyframes flicker {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.9; }
            52% { opacity: 0.5; }
            54% { opacity: 0.9; }
        }

        .profile-section {
            text-align: center;
            margin: 40px 0;
            padding: 30px;
            border: 1px solid #00ffff;
            border-radius: 10px;
            background: rgba(0, 255, 255, 0.05);
            box-shadow: 0 0 30px rgba(0, 255, 255, 0.2);
        }

        .profile-name {
            font-size: 36px;
            color: #ffd43b;
            margin-bottom: 10px;
            text-shadow: 0 0 10px #ffd43b;
        }

        .profile-title {
            font-size: 20px;
            color: #00ffff;
            margin-bottom: 20px;
        }

        .skills-container {
            display: flex;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
            margin-top: 25px;
        }

        .skill-badge {
            padding: 12px 25px;
            background: rgba(255, 212, 59, 0.15);
            border: 2px solid #ffd43b;
            border-radius: 25px;
            color: #ffd43b;
            font-weight: bold;
            transition: all 0.3s;
            cursor: default;
        }

        .skill-badge:hover {
            background: #ffd43b;
            color: #000;
            box-shadow: 0 0 20px #ffd43b;
            transform: translateY(-3px);
        }

        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 25px;
            margin-top: 30px;
        }

        .widget {
            background: rgba(0, 20, 40, 0.9);
            border: 2px solid #00ffff;
            border-radius: 10px;
            padding: 25px;
            box-shadow: 0 0 25px rgba(0, 255, 255, 0.3);
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }

        .widget::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, #00ffff, #ffd43b, #00ffff);
            border-radius: 10px;
            opacity: 0;
            z-index: -1;
            transition: opacity 0.3s;
        }

        .widget:hover::before {
            opacity: 0.3;
        }

        .widget:hover {
            transform: translateY(-5px);
            box-shadow: 0 0 40px rgba(0, 255, 255, 0.5);
        }

        .widget-title {
            font-size: 26px;
            color: #ffd43b;
            margin-bottom: 20px;
            border-bottom: 2px solid #00ffff;
            padding-bottom: 12px;
            text-transform: uppercase;
            text-shadow: 0 0 10px #ffd43b;
        }

        .widget-content {
            font-size: 16px;
            line-height: 2;
        }

        .stat-row {
            display: flex;
            justify-content: space-between;
            margin: 12px 0;
            padding: 12px;
            background: rgba(0, 255, 255, 0.1);
            border-radius: 5px;
            border-left: 3px solid #00ffff;
            transition: all 0.3s;
        }

        .stat-row:hover {
            background: rgba(0, 255, 255, 0.2);
            transform: translateX(5px);
        }

        .stat-label {
            color: #00ffff;
            font-weight: bold;
        }

        .stat-value {
            color: #ffd43b;
            font-weight: bold;
        }

        .quick-links {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-top: 20px;
        }

        .quick-link {
            display: inline-block;
            padding: 12px 25px;
            background: rgba(0, 255, 255, 0.2);
            border: 2px solid #00ffff;
            color: #00ffff;
            text-decoration: none;
            border-radius: 5px;
            transition: all 0.3s;
            cursor: pointer;
            font-weight: bold;
            text-transform: uppercase;
        }

        .quick-link:hover {
            background: #00ffff;
            color: #000;
            box-shadow: 0 0 20px #00ffff;
            transform: scale(1.05);
        }

        .mission-status {
            display: flex;
            align-items: center;
            gap: 12px;
            margin: 15px 0;
            padding: 10px;
            background: rgba(0, 255, 255, 0.05);
            border-radius: 5px;
            transition: all 0.3s;
        }

        .mission-status:hover {
            background: rgba(0, 255, 255, 0.15);
            transform: translateX(5px);
        }

        .status-indicator {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #32cd32;
            animation: blink 1.5s infinite;
            box-shadow: 0 0 10px #32cd32;
        }

        @keyframes blink {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.4; transform: scale(0.9); }
        }

        .terminal {
            background: rgba(0, 0, 0, 0.95);
            border: 2px solid #00ffff;
            border-radius: 10px;
            padding: 20px;
            font-family: 'Courier New', monospace;
            height: 350px;
            overflow-y: auto;
            box-shadow: inset 0 0 20px rgba(0, 255, 255, 0.2);
        }

        .terminal-line {
            margin: 8px 0;
            opacity: 0;
            animation: fadeIn 0.5s forwards;
            font-size: 14px;
        }

        @keyframes fadeIn {
            to { opacity: 1; }
        }

        .terminal-prompt {
            color: #ffd43b;
            font-weight: bold;
        }

        .terminal-text {
            color: #00ffff;
        }

        .repo-card {
            background: rgba(0, 255, 255, 0.05);
            border: 1px solid #00ffff;
            border-radius: 8px;
            padding: 15px;
            margin: 10px 0;
            transition: all 0.3s;
            cursor: pointer;
        }

        .repo-card:hover {
            background: rgba(0, 255, 255, 0.15);
            transform: translateX(5px);
            box-shadow: 0 0 15px rgba(0, 255, 255, 0.3);
        }

        .repo-name {
            color: #ffd43b;
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .repo-desc {
            color: #00ffff;
            font-size: 14px;
            opacity: 0.9;
        }

        .repo-stats {
            display: flex;
            gap: 15px;
            margin-top: 8px;
            font-size: 12px;
            color: #ffd43b;
        }

        footer {
            text-align: center;
            margin-top: 50px;
            padding: 25px;
            border-top: 2px solid #00ffff;
            color: #00ffff;
            font-size: 14px;
            text-shadow: 0 0 10px #00ffff;
        }

        .loading {
            text-align: center;
            padding: 20px;
            color: #ffd43b;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.6; }
        }

        @media (max-width: 768px) {
            .system-time { font-size: 36px; }
            .dashboard { grid-template-columns: 1fr; }
            .profile-name { font-size: 28px; }
        }

        ::-webkit-scrollbar { width: 10px; }
        ::-webkit-scrollbar-track { background: #000; }
        ::-webkit-scrollbar-thumb { background: #00ffff; border-radius: 5px; }
        ::-webkit-scrollbar-thumb:hover { background: #ffd43b; }
    </style>
</head>
<body>
    <div class="grid-overlay"></div>
    <div class="container">
        
        <!-- HEADER -->
        <div class="header">
            <svg class="bat-logo" viewBox="0 0 100 100" fill="#00ffff" xmlns="http://www.w3.org/2000/svg">
                <path d="M50 10 C30 20, 20 30, 15 35 C10 30, 5 25, 2 28 C5 35, 12 40, 18 45 C12 52, 8 60, 5 65 C15 62, 25 55, 35 48 C40 55, 45 62, 50 65 C55 62, 60 55, 65 48 C75 55, 85 62, 95 65 C92 60, 88 52, 82 45 C88 40, 95 35, 98 28 C95 25, 90 30, 85 35 C80 30, 70 20, 50 10 Z"/>
            </svg>
            <div class="greeting">⚡ GOOD EVENING, SIR</div>
            <div class="system-time" id="clock">00:00:00</div>
            <div class="system-date" id="date">Loading...</div>
            <div style="color: #ffd43b; margin-top: 15px; font-size: 16px;">🦇 BATCOMPUTER SYSTEMS ONLINE</div>
        </div>

        <!-- PROFILE SECTION -->
        <div class="profile-section">
            <div class="profile-name">🦇 AFFAN BIN HASSAN</div>
            <div class="profile-title">AI & Data Science Developer | Engineering Student</div>
            <div style="color: #00ffff; margin: 20px 0; font-style: italic; font-size: 18px;">
                "It's not who I am underneath, but what I code that defines me."
            </div>
            <div class="skills-container">
                <span class="skill-badge">🐍 Python</span>
                <span class="skill-badge">📊 Data Science</span>
                <span class="skill-badge">🤖 AI/ML</span>
                <span class="skill-badge">🔧 Git/GitHub</span>
                <span class="skill-badge">💡 Problem Solving</span>
            </div>
        </div>

        <!-- DASHBOARD -->
        <div class="dashboard">
            
            <!-- System Status Widget -->
            <div class="widget">
                <div class="widget-title">⚙️ SYSTEM STATUS</div>
                <div class="widget-content">
                    <div class="stat-row"><span class="stat-label">MACHINE:</span><span class="stat-value">HP Pavilion x360</span></div>
                    <div class="stat-row"><span class="stat-label">PROCESSOR:</span><span class="stat-value">Intel i5-1235U</span></div>
                    <div class="stat-row"><span class="stat-label">MEMORY:</span><span class="stat-value">8GB RAM</span></div>
                    <div class="stat-row"><span class="stat-label">OS:</span><span class="stat-value">Windows 11</span></div>
                    <div class="stat-row"><span class="stat-label">STATUS:</span><span class="stat-value" style="color:#32cd32">● ONLINE</span></div>
                    <div class="stat-row"><span class="stat-label">UPTIME:</span><span class="stat-value" id="uptime">00:00:00</span></div>
                </div>
            </div>

            <!-- Missions Widget -->
            <div class="widget">
                <div class="widget-title">🚀 ACTIVE MISSIONS</div>
                <div class="widget-content">
                    <div class="mission-status"><div class="status-indicator"></div><span>Mastering Python Programming</span></div>
                    <div class="mission-status"><div class="status-indicator"></div><span>Data Science Foundations</span></div>
                    <div class="mission-status"><div class="status-indicator"></div><span>System Integrity (Git/GitHub)</span></div>
                    <div class="mission-status"><div class="status-indicator"></div><span>AI/ML Model Development</span></div>
                    <div class="mission-status"><div class="status-indicator"></div><span>Building Portfolio Projects</span></div>
                </div>
            </div>

            <!-- GitHub Repos Widget -->
            <div class="widget" style="grid-column: span 2;">
                <div class="widget-title">💻 GITHUB REPOSITORIES</div>
                <div id="repos-container" class="widget-content">
                    <div class="loading">🔄 Connecting to GitHub...</div>
                </div>
            </div>

            <!-- Quick Links Widget -->
            <div class="widget">
                <div class="widget-title">🔗 QUICK ACCESS</div>
                <div class="quick-links">
                    <a href="https://github.com/Affanbinhassan" class="quick-link" target="_blank">GitHub</a>
                    <a href="https://linkedin.com/in/Affan-Bin-Hassan" class="quick-link" target="_blank">LinkedIn</a>
                    <a href="https://instagram.com/affan.bin.hassan" class="quick-link" target="_blank">Instagram</a>
                    <a href="mailto:affanbinhassan17@gmail.com" class="quick-link">Email</a>
                </div>
                <div style="margin-top:25px; padding-top:20px; border-top:1px solid #00ffff">
                    <div class="stat-row"><span class="stat-label">FOLLOWERS:</span><span class="stat-value" id="followers">--</span></div>
                    <div class="stat-row"><span class="stat-label">FOLLOWING:</span><span class="stat-value" id="following">--</span></div>
                    <div class="stat-row"><span class="stat-label">PUBLIC REPOS:</span><span class="stat-value" id="public-repos">--</span></div>
                </div>
            </div>

            <!-- Terminal Widget -->
            <div class="widget" style="grid-column: span 2;">
                <div class="widget-title">💻 BATCOMPUTER TERMINAL</div>
                <div class="terminal" id="terminal">
                    <div class="terminal-line"><span class="terminal-prompt">BATCOMPUTER></span> <span class="terminal-text">Initializing secure connection...</span></div>
                </div>
            </div>

        </div>

        <!-- FOOTER -->
        <footer>
            <p>🦇 BATCOMPUTER v3.0 | SYSTEMS OPERATIONAL | <span id="year">2026</span></p>
            <p style="margin-top:10px">Monitored by Alfred | Developed by Affan bin Hassan</p>
            <p style="margin-top:5px; font-size:12px; opacity:0.8">"I won't kill you, but I don't have to save you" - Batman Begins</p>
        </footer>

    </div>

    <script>
        // === REAL-TIME CLOCK ===
        function updateClock() {
            const now = new Date();
            document.getElementById('clock').textContent = now.toLocaleTimeString('en-US', {hour12:false, hour:'2-digit', minute:'2-digit', second:'2-digit'});
            document.getElementById('date').textContent = now.toLocaleDateString('en-US', {weekday:'long', year:'numeric', month:'long', day:'numeric'});
            document.getElementById('year').textContent = now.getFullYear();
        }
        setInterval(updateClock, 1000);
        updateClock();

        // === UPTIME COUNTER ===
        let startTime = Date.now();
        function updateUptime() {
            const elapsed = Math.floor((Date.now() - startTime) / 1000);
            const h = Math.floor(elapsed/3600).toString().padStart(2,'0');
            const m = Math.floor((elapsed%3600)/60).toString().padStart(2,'0');
            const s = (elapsed%60).toString().padStart(2,'0');
            document.getElementById('uptime').textContent = `${h}:${m}:${s}`;
        }
        setInterval(updateUptime, 1000);

        // === GITHUB API FETCH ===
        async function fetchGitHubData() {
            try {
                const userRes = await fetch('https://api.github.com/users/Affanbinhassan');
                const userData = await userRes.json();
                
                document.getElementById('followers').textContent = userData.followers || 0;
                document.getElementById('following').textContent = userData.following || 0;
                document.getElementById('public-repos').textContent = userData.public_repos || 0;

                const reposRes = await fetch('https://api.github.com/users/Affanbinhassan/repos?sort=updated&per_page=6');
                const repos = await reposRes.json();
                displayRepos(repos);
            } catch(err) {
                console.error('GitHub API Error:', err);
                document.getElementById('repos-container').innerHTML = '<div style="color:#ff6b6b">⚠ Could not load GitHub data. Check internet connection.</div>';
            }
        }

        function displayRepos(repos) {
            const container = document.getElementById('repos-container');
            container.innerHTML = '';
            if(!repos || repos.length === 0) {
                container.innerHTML = '<div style="color:#ffd43b">No public repositories found.</div>';
                return;
            }
            repos.forEach(repo => {
                const card = document.createElement('div');
                card.className = 'repo-card';
                card.onclick = () => window.open(repo.html_url, '_blank');
                card.innerHTML = `
                    <div class="repo-name">📁 ${repo.name}</div>
                    <div class="repo-desc">${repo.description || 'No description'}</div>
                    <div class="repo-stats">
                        <span>⭐ ${repo.stargazers_count}</span>
                        <span>🍴 ${repo.forks_count}</span>
                        <span>🔧 ${repo.language || 'Unknown'}</span>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        // === TERMINAL ANIMATION ===
        const terminalLines = [
            "🔐 Establishing secure connection...",
            "✅ Identity verified: AFFAN BIN HASSAN",
            "🖥️ Loading Bat-OS v3.0...",
            "🐍 Python environment: READY",
            "📊 Data Science modules: ONLINE",
            "🤖 AI/ML frameworks: LOADED",
            "🔧 Git sync: COMPLETE",
            "🌐 GitHub API: CONNECTED",
            "✨ All systems operational. Welcome back, sir."
        ];

        let lineIdx = 0;
        function addTerminalLine() {
            if(lineIdx < terminalLines.length) {
                const term = document.getElementById('terminal');
                const line = document.createElement('div');
                line.className = 'terminal-line';
                line.innerHTML = `<span class="terminal-prompt">BATCOMPUTER></span> <span class="terminal-text">${terminalLines[lineIdx]}</span>`;
                term.appendChild(line);
                term.scrollTop = term.scrollHeight;
                lineIdx++;
                setTimeout(addTerminalLine, 500);
            }
        }
        setTimeout(addTerminalLine, 800);

        // Random terminal messages
        const randomMsgs = [
            "🔍 Scanning network...", "📈 Analyzing patterns...", 
            "⚡ Optimizing performance...", "🛡️ Security: ACTIVE",
            "💾 Memory: OPTIMAL", "🌡️ CPU: NORMAL",
            "🔐 Encrypting data...", "📡 Backup: STANDBY"
        ];
        setInterval(() => {
            const term = document.getElementById('terminal');
            const msg = randomMsgs[Math.floor(Math.random() * randomMsgs.length)];
            const line = document.createElement('div');
            line.className = 'terminal-line';
            line.innerHTML = `<span class="terminal-prompt">BATCOMPUTER></span> <span class="terminal-text">${msg}</span>`;
            term.appendChild(line);
            term.scrollTop = term.scrollHeight;
            while(term.children.length > 15) term.removeChild(term.firstChild);
        }, 10000);

        // === INITIALIZE ===
        window.onload = function() {
            fetchGitHubData();
        };

        // Keyboard shortcut: F5 to refresh terminal
        document.addEventListener('keydown', (e) => {
            if(e.key === 'F5') {
                e.preventDefault();
                const term = document.getElementById('terminal');
                const line = document.createElement('div');
                line.className = 'terminal-line';
                line.innerHTML = `<span class="terminal-prompt">BATCOMPUTER></span> <span class="terminal-text">🔄 System refresh initiated...</span>`;
                term.appendChild(line);
                term.scrollTop = term.scrollHeight;
            }
        });
    </script>
</body>
</html>
