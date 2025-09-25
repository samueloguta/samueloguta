<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Samuel Oguta - Interactive Developer Profile</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'SF Mono', 'Monaco', 'Cascadia Code', monospace;
            background: linear-gradient(45deg, #0a0a0a, #1a0a2e, #16213e);
            color: white;
            overflow-x: hidden;
        }

        /* Floating particles background */
        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
        }

        /* Header with 3D rotating cube */
        .hero {
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }

        .hero-content {
            text-align: center;
            z-index: 10;
            max-width: 800px;
            padding: 2rem;
        }

        .hero h1 {
            font-size: 4rem;
            background: linear-gradient(45deg, #00f5ff, #ff00f5, #f5ff00);
            background-size: 400% 400%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: gradientShift 3s ease-in-out infinite;
            margin-bottom: 1rem;
        }

        @keyframes gradientShift {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }

        .subtitle {
            font-size: 1.5rem;
            margin-bottom: 2rem;
            opacity: 0.9;
        }

        .typewriter {
            border-right: 2px solid #00f5ff;
            animation: blink 1s infinite;
        }

        @keyframes blink {
            0%, 50% { border-color: transparent; }
            51%, 100% { border-color: #00f5ff; }
        }

        /* AI Daily Insight Widget */
        .ai-insight {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(0, 245, 255, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(0, 245, 255, 0.3);
            border-radius: 15px;
            padding: 1rem;
            max-width: 300px;
            z-index: 100;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }

        .insight-header {
            color: #00f5ff;
            font-weight: bold;
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .ai-icon {
            width: 20px;
            height: 20px;
            background: linear-gradient(45deg, #00f5ff, #ff00f5);
            border-radius: 50%;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }

        /* Terminal Interface */
        .terminal {
            background: rgba(0, 0, 0, 0.9);
            border: 2px solid #00f5ff;
            border-radius: 10px;
            margin: 2rem auto;
            max-width: 800px;
            font-family: 'Courier New', monospace;
            box-shadow: 0 0 30px rgba(0, 245, 255, 0.3);
        }

        .terminal-header {
            background: linear-gradient(90deg, #ff6b6b, #ffd93d, #6bcf7f);
            padding: 0.5rem;
            display: flex;
            gap: 0.5rem;
            border-radius: 8px 8px 0 0;
        }

        .terminal-button {
            width: 15px;
            height: 15px;
            border-radius: 50%;
        }

        .terminal-body {
            padding: 1rem;
            min-height: 300px;
        }

        .terminal-line {
            margin: 0.5rem 0;
        }

        .prompt {
            color: #00f5ff;
        }

        .command {
            color: #ffd93d;
        }

        .output {
            color: #6bcf7f;
            margin-left: 1rem;
        }

        /* 3D Project Gallery */
        .project-gallery {
            height: 600px;
            position: relative;
            margin: 4rem 0;
            overflow: hidden;
        }

        .gallery-controls {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 10;
            display: flex;
            gap: 1rem;
        }

        .gallery-btn {
            background: rgba(0, 245, 255, 0.2);
            border: 1px solid #00f5ff;
            color: white;
            padding: 0.5rem 1rem;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .gallery-btn:hover {
            background: rgba(0, 245, 255, 0.4);
            transform: scale(1.05);
        }

        /* Interactive Stats Cards */
        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 2rem;
            margin: 4rem 0;
            padding: 0 2rem;
        }

        .stat-card {
            background: linear-gradient(135deg, rgba(0, 245, 255, 0.1), rgba(255, 0, 245, 0.1));
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 20px;
            padding: 2rem;
            text-align: center;
            transform: translateY(0);
            transition: all 0.3s;
            cursor: pointer;
        }

        .stat-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(0, 245, 255, 0.3);
        }

        .stat-number {
            font-size: 3rem;
            font-weight: bold;
            background: linear-gradient(45deg, #00f5ff, #ff00f5);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* Voice Intro Button */
        .voice-intro {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 60px;
            height: 60px;
            background: linear-gradient(45deg, #ff6b6b, #ffd93d);
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 100;
            animation: breathe 2s infinite;
            box-shadow: 0 0 20px rgba(255, 107, 107, 0.5);
        }

        @keyframes breathe {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }

        /* Tech Radar */
        .tech-radar {
            margin: 4rem 0;
            text-align: center;
        }

        .radar-container {
            width: 400px;
            height: 400px;
            margin: 0 auto;
            position: relative;
            border: 2px solid rgba(0, 245, 255, 0.3);
            border-radius: 50%;
            background: radial-gradient(circle, rgba(0, 245, 255, 0.1), transparent);
        }

        /* Gaming Easter Eggs */
        .konami-hint {
            position: fixed;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 0.8rem;
            opacity: 0.3;
            z-index: 100;
        }

        /* Interactive Elements */
        .interactive-element {
            cursor: pointer;
            transition: all 0.3s;
        }

        .interactive-element:hover {
            transform: scale(1.05);
            filter: drop-shadow(0 0 10px rgba(0, 245, 255, 0.5));
        }

        /* Command Input */
        .command-input {
            background: transparent;
            border: none;
            color: white;
            font-family: inherit;
            font-size: inherit;
            outline: none;
            width: 100%;
        }

        /* Live Status Indicators */
        .status-bar {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(0, 0, 0, 0.8);
            padding: 0.5rem;
            display: flex;
            justify-content: center;
            gap: 2rem;
            font-size: 0.8rem;
        }

        .status-item {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #6bcf7f;
            animation: pulse 2s infinite;
        }

        .status-dot.warning { background: #ffd93d; }
        .status-dot.error { background: #ff6b6b; }

        /* Responsive Design */
        @media (max-width: 768px) {
            .hero h1 { font-size: 2.5rem; }
            .subtitle { font-size: 1.2rem; }
            .ai-insight { position: relative; margin: 1rem; }
            .stats-container { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>
    <!-- Floating Particles Background -->
    <div class="particles" id="particles"></div>

    <!-- AI Daily Insight Widget -->
    <div class="ai-insight">
        <div class="insight-header">
            <div class="ai-icon"></div>
            AI Daily Insight
        </div>
        <div id="daily-insight">
            "The best debugging tool is still a good understanding of your code." - Updated daily at 12:00 AM
        </div>
    </div>

    <!-- Hero Section -->
    <section class="hero">
        <div class="hero-content">
            <h1>Samuel Oguta</h1>
            <div class="subtitle">
                <span class="typewriter" id="typewriter">JavaScript Engineer · Full-Stack Developer · AI Enthusiast</span>
            </div>
            <p style="font-size: 1.2rem; margin: 2rem 0; opacity: 0.8;">
                Building the future with scalable web apps, AI tools, and virtual assistants
            </p>
        </div>
    </section>

    <!-- Interactive Terminal CV -->
    <section class="terminal">
        <div class="terminal-header">
            <div class="terminal-button" style="background: #ff6b6b;"></div>
            <div class="terminal-button" style="background: #ffd93d;"></div>
            <div class="terminal-button" style="background: #6bcf7f;"></div>
        </div>
        <div class="terminal-body">
            <div class="terminal-line">
                <span class="prompt">samuel@portfolio:~$</span> <span class="command">whoami</span>
            </div>
            <div class="terminal-line output">
                Full-Stack Developer passionate about clean code and elegant UX
            </div>
            <div class="terminal-line">
                <span class="prompt">samuel@portfolio:~$</span> <span class="command">ls skills</span>
            </div>
            <div class="terminal-line output">
                JavaScript TypeScript React Next.js Node.js Python AI/ML MongoDB PostgreSQL
            </div>
            <div class="terminal-line">
                <span class="prompt">samuel@portfolio:~$</span> <span class="command">cat location</span>
            </div>
            <div class="terminal-line output">
                📍 Kenya | 🌐 Building globally
            </div>
            <div class="terminal-line">
                <span class="prompt">samuel@portfolio:~$</span> 
                <input type="text" class="command-input" id="terminalInput" placeholder="Try: projects, contact, skills, or help">
            </div>
            <div id="terminalOutput"></div>
        </div>
    </section>

    <!-- 3D Project Gallery -->
    <section class="project-gallery">
        <div class="gallery-controls">
            <button class="gallery-btn interactive-element" onclick="rotateGallery('left')">← Prev</button>
            <button class="gallery-btn interactive-element" onclick="rotateGallery('auto')">🎮 Auto</button>
            <button class="gallery-btn interactive-element" onclick="rotateGallery('right')">Next →</button>
        </div>
        <div id="projectGallery3D"></div>
    </section>

    <!-- Interactive Stats Cards -->
    <section class="stats-container">
        <div class="stat-card interactive-element" onclick="showDetail('commits')">
            <div class="stat-number" id="commitCount">847</div>
            <div>Total Commits</div>
            <small>This Year</small>
        </div>
        <div class="stat-card interactive-element" onclick="showDetail('projects')">
            <div class="stat-number" id="projectCount">23</div>
            <div>Projects Built</div>
            <small>All Time</small>
        </div>
        <div class="stat-card interactive-element" onclick="showDetail('languages')">
            <div class="stat-number" id="languageCount">8</div>
            <div>Languages</div>
            <small>Actively Used</small>
        </div>
        <div class="stat-card interactive-element" onclick="showDetail('contributions')">
            <div class="stat-number" id="contributionCount">1.2k</div>
            <div>Contributions</div>
            <small>Past Year</small>
        </div>
    </section>

    <!-- Tech Radar -->
    <section class="tech-radar">
        <h2 style="margin-bottom: 2rem; color: #00f5ff;">🎯 Live Tech Radar</h2>
        <div class="radar-container" id="techRadar">
            <!-- Dynamic radar chart will be generated here -->
        </div>
        <p style="margin-top: 1rem; opacity: 0.7;">Most used technologies in the past 30 days</p>
    </section>

    <!-- Voice Intro Button -->
    <div class="voice-intro interactive-element" onclick="playVoiceIntro()" title="Play Voice Introduction">
        🎤
    </div>

    <!-- Gaming Easter Egg Hint -->
    <div class="konami-hint">
        🎮 Try the Konami Code for a surprise!
    </div>

    <!-- Live Status Bar -->
    <div class="status-bar">
        <div class="status-item">
            <div class="status-dot"></div>
            <span>TakaPlus: Online</span>
        </div>
        <div class="status-item">
            <div class="status-dot"></div>
            <span>Portfolio: Live</span>
        </div>
        <div class="status-item">
            <div class="status-dot warning"></div>
            <span>Nyaatha: Maintenance</span>
        </div>
        <div class="status-item">
            <div class="status-dot"></div>
            <span>API: Healthy</span>
        </div>
    </div>

    <script>
        // Typewriter Effect
        const phrases = [
            "JavaScript Engineer · Full-Stack Developer · AI Enthusiast",
            "Building Scalable Web Applications",
            "Creating AI-Powered Solutions",
            "Open Source Contributor",
            "Virtual Assistant Developer"
        ];
        let phraseIndex = 0;
        let charIndex = 0;
        let isDeleting = false;

        function typeWriter() {
            const current = phrases[phraseIndex];
            const typewriterElement = document.getElementById('typewriter');
            
            if (isDeleting) {
                typewriterElement.textContent = current.substring(0, charIndex - 1);
                charIndex--;
            } else {
                typewriterElement.textContent = current.substring(0, charIndex + 1);
                charIndex++;
            }

            if (!isDeleting && charIndex === current.length) {
                setTimeout(() => isDeleting = true, 2000);
            } else if (isDeleting && charIndex === 0) {
                isDeleting = false;
                phraseIndex = (phraseIndex + 1) % phrases.length;
            }

            setTimeout(typeWriter, isDeleting ? 50 : 100);
        }
        typeWriter();

        // 3D Particles Background
        let scene, camera, renderer, particles;

        function initParticles() {
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            renderer = new THREE.WebGLRenderer({ alpha: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            document.getElementById('particles').appendChild(renderer.domElement);

            // Create particles
            const geometry = new THREE.BufferGeometry();
            const particleCount = 1000;
            const positions = new Float32Array(particleCount * 3);

            for (let i = 0; i < particleCount * 3; i++) {
                positions[i] = (Math.random() - 0.5) * 1000;
            }

            geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
            
            const material = new THREE.PointsMaterial({
                color: 0x00f5ff,
                size: 2
            });

            particles = new THREE.Points(geometry, material);
            scene.add(particles);
            camera.position.z = 100;

            animate();
        }

        function animate() {
            requestAnimationFrame(animate);
            particles.rotation.x += 0.0005;
            particles.rotation.y += 0.001;
            renderer.render(scene, camera);
        }

        // 3D Project Gallery
        let galleryScene, galleryCamera, galleryRenderer, projectCubes = [];
        let autoRotate = false;

        function init3DGallery() {
            galleryScene = new THREE.Scene();
            galleryCamera = new THREE.PerspectiveCamera(75, 800 / 600, 0.1, 1000);
            galleryRenderer = new THREE.WebGLRenderer({ alpha: true });
            galleryRenderer.setSize(800, 600);
            document.getElementById('projectGallery3D').appendChild(galleryRenderer.domElement);

            // Create project cubes
            const projects = [
                { name: 'TakaPlus', color: 0x00ff00 },
                { name: 'Portfolio', color: 0x0000ff },
                { name: 'Echoshere', color: 0xff0000 },
                { name: 'Nyaatha', color: 0xffff00 },
                { name: 'COVID Tracker', color: 0xff00ff }
            ];

            projects.forEach((project, index) => {
                const geometry = new THREE.BoxGeometry(4, 4, 4);
                const material = new THREE.MeshLambertMaterial({ color: project.color });
                const cube = new THREE.Mesh(geometry, material);
                
                const angle = (index / projects.length) * Math.PI * 2;
                cube.position.x = Math.cos(angle) * 15;
                cube.position.z = Math.sin(angle) * 15;
                cube.userData = project;
                
                projectCubes.push(cube);
                galleryScene.add(cube);
            });

            // Add lighting
            const light = new THREE.DirectionalLight(0xffffff, 1);
            light.position.set(5, 5, 5);
            galleryScene.add(light);

            galleryCamera.position.set(0, 0, 25);
            animateGallery();
        }

        function animateGallery() {
            requestAnimationFrame(animateGallery);
            
            projectCubes.forEach((cube, index) => {
                cube.rotation.x += 0.01;
                cube.rotation.y += 0.01;
                
                if (autoRotate) {
                    const angle = Date.now() * 0.001 + (index / projectCubes.length) * Math.PI * 2;
                    cube.position.x = Math.cos(angle) * 15;
                    cube.position.z = Math.sin(angle) * 15;
                }
            });

            galleryRenderer.render(galleryScene, galleryCamera);
        }

        function rotateGallery(direction) {
            if (direction === 'auto') {
                autoRotate = !autoRotate;
            } else {
                autoRotate = false;
                const rotationSpeed = direction === 'left' ? -0.5 : 0.5;
                projectCubes.forEach(cube => {
                    const currentAngle = Math.atan2(cube.position.z, cube.position.x);
                    const newAngle = currentAngle + rotationSpeed;
                    cube.position.x = Math.cos(newAngle) * 15;
                    cube.position.z = Math.sin(newAngle) * 15;
                });
            }
        }

        // Terminal Commands
        const terminalCommands = {
            help: "Available commands: projects, skills, contact, experience, fun, clear",
            projects: "🚀 TakaPlus - Smart recycling platform\n📊 COVID Tracker - Data analysis\n🏠 Nyaatha - Apartment listing\n💼 Portfolio - Personal showcase\n🌐 Echoshere - Community platform",
            skills: "💻 JavaScript, TypeScript, React, Next.js\n🔧 Node.js, Express, MongoDB, PostgreSQL\n🤖 Python, AI/ML, TensorFlow, PyTorch\n🎨 HTML5, CSS3, Tailwind CSS",
            contact: "📧 ogutasamuel27@gmail.com\n💼 LinkedIn: samuel-oguta-7a8b492a8\n🐙 GitHub: samueloguta\n📍 Kenya",
            experience: "🌟 3+ years Full-Stack Development\n🤖 AI/ML Integration Specialist\n💡 Open Source Contributor\n🚀 Startup Experience",
            fun: "🎮 Try the Konami Code!\n🎵 I code better with music\n☕ Coffee > Sleep\n🌍 Building for global impact",
            clear: "CLEAR_TERMINAL"
        };

        document.getElementById('terminalInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                const command = e.target.value.toLowerCase().trim();
                const output = document.getElementById('terminalOutput');
                
                // Add command to terminal
                const commandLine = document.createElement('div');
                commandLine.className = 'terminal-line';
                commandLine.innerHTML = `<span class="prompt">samuel@portfolio:~$</span> <span class="command">${command}</span>`;
                output.appendChild(commandLine);
                
                // Add response
                const responseLine = document.createElement('div');
                responseLine.className = 'terminal-line output';
                
                if (command === 'clear') {
                    output.innerHTML = '';
                } else if (terminalCommands[command]) {
                    responseLine.innerHTML = terminalCommands[command].replace(/\n/g, '<br>');
                    output.appendChild(responseLine);
                } else {
                    responseLine.innerHTML = `Command '${command}' not found. Type 'help' for available commands.`;
                    output.appendChild(responseLine);
                }
                
                e.target.value = '';
                output.scrollTop = output.scrollHeight;
            }
        });

        // Voice Introduction
        function playVoiceIntro() {
            // In a real implementation, you'd play an actual audio file
            const utterance = new SpeechSynthesisUtterance(
                "Hi! I'm Samuel Oguta, a passionate full-stack developer from Kenya. I build scalable web applications and AI-powered solutions. Let's create something amazing together!"
            );
            speechSynthesis.speak(utterance);
        }

        // Konami Code Easter Egg
        let konamiCode = [];
        const konamiSequence = [
            'ArrowUp', 'ArrowUp', 'ArrowDown', 'ArrowDown',
            'ArrowLeft', 'ArrowRight', 'ArrowLeft', 'ArrowRight',
            'KeyB', 'KeyA'
        ];

        document.addEventListener('keydown', function(e) {
            konamiCode.push(e.code);
            if (konamiCode.length > konamiSequence.length) {
                konamiCode.shift();
            }
            
            if (JSON.stringify(konamiCode) === JSON.stringify(konamiSequence)) {
                activateEasterEgg();
            }
        });

        function activateEasterEgg() {
            document.body.style.transform = 'rotate(360deg)';
            document.body.style.transition = 'transform 2s';
            setTimeout(() => {
                document.body.style.transform = 'none';
                alert('🎮 Konami Code activated! You found the secret! 🚀');
            }, 2000);
        }

        // Dynamic Stats Updates
        function updateStats() {
            const stats = {
                commits: Math.floor(Math.random() * 100) + 800,
                projects: 23 + Math.floor(Math.random() * 5),
                languages: 8,
                contributions: Math.floor(Math.random() * 200) + 1000
            };

            document.getElementById('commitCount').textContent = stats.commits;
            document.getElementById('projectCount').textContent = stats.projects;
            document.getElementById('contributionCount').textContent = (stats.contributions / 1000).toFixed(1) + 'k';
        }

        function showDetail(type) {
            const details = {
                commits: 'Recent activity shows consistent daily contributions with focus on JavaScript and Python projects.',
                projects: 'From recycling platforms to AI tools, each project solves real-world problems.',
                languages: 'Polyglot developer comfortable across the full stack.',
                contributions: 'Active open source contributor helping build the developer community.'
            };
            
            alert(`📊 ${details[type]}`);
        }

        // Daily AI Insight Updater
        function updateDailyInsight() {
            const insights = [
                "Code is like humor. When you have to explain it, it's bad.",
                "The best debugging tool is still a good understanding of your code.",
                "First, solve the problem. Then, write the code.",
                "Programs must be written for people to read, and machines to execute.",
                "Simplicity is the ultimate sophistication in code."
            ];
            
            const today = new Date().getDate();
            const insight = insights[today % insights.length];
            document.getElementById('daily-insight').textContent = `"${insight}" - Updated daily at 12:00 AM`;
        }

        // Initialize everything
        window.addEventListener('load', function() {
            initParticles();
            init3DGallery();
            updateStats();
            updateDailyInsight();
            
            // Update stats every 30 seconds for demo
            setInterval(updateStats, 30000);
        });

        // Responsive handling
        window.addEventListener('resize', function() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
