# pandaa
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Il Panda Affamato 🐼🎋</title>
    <style>
        :root {
            --bg-color: #e8f5e9;
            --font-family: 'Comic Sans MS', 'Chalkboard SE', sans-serif;
        }

        body {
            margin: 0;
            padding: 0;
            background-color: var(--bg-color);
            font-family: var(--font-family);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            overflow: hidden;
            user-select: none;
        }

        h1 {
            color: #2e7d32;
            margin: 5px 0;
            text-shadow: 2px 2px #c8e6c9;
            font-size: 28px;
        }

        #game-container {
            position: relative;
            width: 400px;
            height: 500px;
            background: linear-gradient(to bottom, #bbdefb, #e8f5e9);
            border: 8px solid #81c784;
            border-radius: 20px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.15);
            overflow: hidden;
        }

        #ui {
            position: absolute;
            top: 10px;
            left: 10px;
            right: 10px;
            display: flex;
            justify-content: space-between;
            font-size: 18px;
            font-weight: bold;
            color: #1b5e20;
            z-index: 10;
        }

        /* Il nostro Panda fatto in CSS */
        #panda {
            position: absolute;
            bottom: 20px;
            left: 170px;
            width: 60px;
            height: 60px;
            background-color: #fff;
            border-radius: 50%;
            border: 3px solid #333;
            transition: width 0.2s, height 0.2s, bottom 0.2s;
            box-shadow: inset -5px -5px 0px #eee;
        }

        /* Orecchie del Panda */
        #panda::before, #panda::after {
            content: '';
            position: absolute;
            top: -5px;
            width: 18px;
            height: 18px;
            background-color: #333;
            border-radius: 50%;
        }
        #panda::before { left: -2px; }
        #panda::after { right: -2px; }

        /* Occhi e dettagli del viso */
        .face {
            position: relative;
            width: 100%;
            height: 100%;
        }
        .face::before, .face::after {
            content: '';
            position: absolute;
            top: 15px;
            width: 14px;
            height: 18px;
            background-color: #333;
            border-radius: 50%;
        }
        .face::before { left: 8px; transform: rotate(-15px); }
        .face::after { right: 8px; transform: rotate(15px); }

        .eyes {
            position: absolute;
            top: 20px;
            left: 13px;
            width: 4px;
            height: 4px;
            background-color: #fff;
            border-radius: 50%;
            box-shadow: 26px 0 0 #fff;
        }
        .blush {
            position: absolute;
            top: 30px;
            left: 4px;
            width: 8px;
            height: 6px;
            background-color: #ff8a80;
            border-radius: 50%;
            box-shadow: 40px 0 0 #ff8a80;
        }

        /* Elementi cadenti */
        .falling-item {
            position: absolute;
            font-size: 32px;
            line-height: 1;
            user-select: none;
        }

        /* Schermate */
        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.85);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 20;
        }

        button {
            background-color: #4caf50;
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 18px;
            font-family: var(--font-family);
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.1s;
            margin-top: 15px;
        }

        button:hover {
            background-color: #45a049;
            transform: scale(1.05);
        }

        .hidden {
            display: none !important;
        }

        #instructions {
            font-size: 14px;
            color: #555;
            margin-top: 10px;
            text-align: center;
        }
    </style>
</head>
<body>

    <h1>Il Panda Affamato 🐼</h1>
    
    <div id="game-container">
        <div id="ui">
            <div id="score-display">Bambù: 0</div>
            <div id="weight-display">Peso: Morbidoso</div>
        </div>

        <div id="panda">
            <div class="face">
                <div class="eyes"></div>
                <div class="blush"></div>
            </div>
        </div>

        <div id="start-screen" class="overlay">
            <h2>Aiuta il panda a mangiare!</h2>
            <p style="text-align:center; padding: 0 20px;">Muoviti a destra e sinistra. Mangia il bambù 🎋 ma evita la lisca di pesce 🐟!</p>
            <button onclick="startGame()">Gioca</button>
            <div id="instructions">Usa le Frecce (← / →), i tasti A/D o clicca sui lati dello schermo</div>
        </div>

        <div id="game-over-screen" class="overlay hidden">
            <h2 style="color: #d32f2f; margin: 0;">Game Over! 😵</h2>
            <p>Il panda ha mangiato una lisca di pesce!</p>
            <p id="final-score" style="font-weight: bold; font-size: 20px;"></p>
            <button onclick="startGame()">Riprova 🔄</button>
        </div>
    </div>

    <script>
        const container = document.getElementById('game-container');
        const panda = document.getElementById('panda');
        const scoreDisplay = document.getElementById('score-display');
        const weightDisplay
