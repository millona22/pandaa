<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Il Panda Affamato 🐼🎋</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background-color: #e8f5e9;
            font-family: 'Comic Sans MS', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            user-select: none;
        }
        h1 {
            color: #2e7d32;
            margin: 10px 0;
        }
        #canvas-container {
            position: relative;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
            border-radius: 15px;
            overflow: hidden;
        }
        canvas {
            background: linear-gradient(to bottom, #bbdefb, #e8f5e9);
            display: block;
        }
        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.9);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        button {
            background-color: #4caf50;
            color: white;
            border: none;
            padding: 12px 30px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 25px;
            cursor: pointer;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-top: 15px;
        }
        button:hover { background-color: #45a049; }
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <h1>Il Panda Affamato 🐼</h1>

    <div id="canvas-container">
        <canvas id="gameCanvas" width="400" height="500"></canvas>

        <div id="startScreen" class="overlay">
            <h2>Nutri il Panda!</h2>
            <p>Usa le FRECCE o tasti A/D per muoverti.</p>
            <p>Mangia il bambù 🎋 evita i pesci 🐟!</p>
            <button id="startBtn">Gioca 🚀</button>
        </div>

        <div id="gameOverScreen" class="overlay hidden">
            <h2 style="color: #d32f2f;">Game Over! 😵</h2>
            <p id="finalScore"></p>
            <button id="restartBtn">Riprova 🔄</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        
        const startScreen = document.getElementById('startScreen');
        const gameOverScreen = document.getElementById('gameOverScreen');
        const finalScore = document.getElementById('finalScore');
        
        // Stato del Gioco
        let gameActive = false;
        let score = 0;
        let panda = { x: 170, y: 420, w: 60, h: 60, speed: 7 };
        let items = [];
        let keys = {};
        let spawnTimer = 0;

        // Controlli
        window.addEventListener('keydown', e => keys[e.key] = true);
        window.addEventListener('keyup', e => keys[e.key] = false);

        // Controlli Mouse/Touch sul Canvas
        canvas.addEventListener('pointerdown', e => {
            if (!gameActive) return;
            const rect = canvas.getBoundingClientRect();
            const clickX = e.clientX - rect.left;
            panda.x = clickX < canvas.width / 2 ? Math.max(0, panda.x - 40) : Math.min(canvas.width - panda.w, panda.x + 40);
        });

        document.getElementById('startBtn').addEventListener('click', initGame);
        document.getElementById('restartBtn').addEventListener('click', initGame);

        function initGame() {
            score = 0;
            panda.w = 60;
            panda.h = 60;
            panda.x = 170;
            items = [];
            spawnTimer = 0;
            keys = {};
            
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            gameActive = true;
            
            requestAnimationFrame(update);
        }

        function drawPanda(x, y, w, h) {
            // Orecchie
            ctx.fillStyle = '#222';
            ctx.beginPath(); ctx.arc(x + w*0.15, y + h*0.1, w*0.18, 0, Math.PI * 2); ctx.fill();
            ctx.beginPath(); ctx.arc(x + w*0.85, y + h*0.1, w*0.18, 0, Math.PI * 2); ctx.fill();
            
            // Corpo/Testa
            ctx.fillStyle = '#ffffff';
            ctx.strokeStyle = '#222';
            ctx.lineWidth = 3;
            ctx.beginPath(); ctx.arc(x + w/2, y + h/2, w/2, 0, Math.PI * 2); ctx.fill(); ctx.stroke();
            
            // Macchie occhi
            ctx.fillStyle = '#222';
            ctx.beginPath(); ctx.ellipse(x + w*0.3, y + h*0.42, w*0.13, h*0.16, Math.PI/12, 0, Math.PI * 2); ctx.fill();
            ctx.beginPath(); ctx.ellipse(x + w*0.7, y + h*0.42, w*0.13, h*0.16, -Math.PI/12, 0, Math.PI * 2); ctx.fill();
            
            // Pupille
            ctx.fillStyle = '#fff';
            ctx.beginPath(); ctx.arc(x + w*0.32, y + h*0.42, w*0.04, 0, Math.PI * 2); ctx.fill();
            ctx.beginPath(); ctx.arc(x + w*0.68, y + h*0.42, w*0.04, 0, Math.PI * 2); ctx.fill();
            
            // Guance
            ctx.fillStyle = '#ff8a80';
            ctx.beginPath(); ctx.arc(x + w*0.16, y + h*0.6, w*0.08, 0, Math.PI * 2); ctx.fill();
            ctx.beginPath(); ctx.arc(x + w*0.84, y + h*0.6, w*0.08, 0, Math.PI * 2); ctx.fill();

            // Naso
            ctx.fillStyle = '#222';
            ctx.beginPath(); ctx.arc(x + w/2, y + h*0.55, w*0.06, 0, Math.PI * 2); ctx.fill();
        }

        function update() {
            if (!gameActive) return;

            // Cancella canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Movimento Panda
            if (keys['ArrowLeft'] || keys['a'] || keys['A']) panda.x = Math.max(0, panda.x - panda.speed);
            if (keys['ArrowRight'] || keys['d'] || keys['D']) panda.x = Math.min(canvas.width - panda.w, panda.x + panda.speed);

            // Disegna Panda
            drawPanda(panda.x, panda.y, panda.w, panda.h);

            // Generazione Oggetti
            spawnTimer++;
            if (spawnTimer > 45) {
                let isBamboo = Math.random() > 0.3;
                items.push({
                    x: Math.random() * (canvas.width - 30),
                    y: -30,
                    size: 30,
                    type: isBamboo ? '🎋' : '🐟',
                    isFish: !isBamboo,
                    speed: 3 + Math.random() * 3 + (score * 0.1)
                });
                spawnTimer = 0;
            }

            // Gestione Oggetti
            for (let i = items.length - 1; i >= 0; i--) {
                let item = items[i];
                item.y += item.speed;

                // Mostra Emoji
                ctx.font = "30px Arial";
                ctx.fillText(item.type, item.x, item.y);

                // Collisione
                if (item.y + 10 >= panda.y && item.y - 20 <= panda.y + panda.h &&
                    item.x + 25 >= panda.x && item.x <= panda.x + panda.w) {
                    
                    if (item.isFish) {
                        endGame();
                        return;
                    } else {
                        score++;
                        // Ingrassa il panda
                        if (panda.w < 130) {
                            panda.w += 3;
                            panda.h += 2.5;
                            panda.y = 480 - panda.h; // Mantieni i piedi a terra
                        }
                        items.splice(i, 1);
                        continue;
                    }
                }

                // Fuori schermo
                if (item.y > canvas.height + 30) items.splice(i, 1);
            }

            // Testi UI interni al Canvas
            ctx.fillStyle = '#1b5e20';
            ctx.font = 'bold 16px "Comic Sans MS"';
            ctx.fillText(`Bambù: ${score}`, 15, 30);
            
            let status = "Morbidoso ✨";
            if (score >= 20) status = "Unità Assoluta 👑";
            else if (score >= 12) status = "Mega Cicciotto 🐻";
            else if (score >= 5) status = "Paffuto 🐹";
            ctx.fillText(`Stato: ${status}`, canvas.width - 170, 30);

            requestAnimationFrame(update);
        }

        function endGame() {
            gameActive = false;
            finalScore.innerText = `Punteggio totale: ${score} pezzi di bambù!`;
            gameOverScreen.classList.remove('hidden');
        }
    </script>
</body>
</html>
