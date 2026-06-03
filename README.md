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
            -webkit-user-select: none;
        }

        h1 {
            color: #2e7d32;
            margin: 5px 0;
            text-shadow: 2px 2px #c8e6c9;
            font-size: 28px;
            text-align: center;
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
            touch-action: none; /* Impedisce lo zoom da telefono */
        }

        #ui {
            position: absolute;
            top: 12px;
            left: 12px;
            right: 12px;
            display: flex;
            justify-content: space-between;
            font-size: 18px;
            font-weight: bold;
            color: #1b5e20;
            z-index: 10;
            background: rgba(255, 255, 255, 0.6);
            padding: 5px 10px;
            border-radius: 10px;
        }

        /* Il nostro Panda Super Carino in CSS */
        #panda {
            position: absolute;
            bottom: 20px;
            left: 170px;
            width: 60px;
            height: 60px;
            background-color: #ffffff;
            border-radius: 50%;
            border: 3px solid #222;
            box-shadow: inset -4px -4px 0px #e0e0e0;
            z-index: 5;
        }

        /* Orecchie */
        #panda::before, #panda::after {
            content: '';
            position: absolute;
            top: -6px;
            width: 20px;
            height: 20px;
            background-color: #222;
            border-radius: 50%;
        }
        #panda::before { left: -4px; }
        #panda::after { right: -4px; }

        .face {
            position: relative;
            width: 100%;
            height: 100%;
        }

        /* Guance Rosa Cicciotte */
        .blush {
            position: absolute;
            top: 32px;
            left: 4px;
            width: 10px;
            height: 7px;
            background-color: #ff8a80;
            border-radius: 50%;
            box-shadow: 36px 0 0 #ff8a80;
            z-index: 2;
        }

        /* Macchie nere occhi */
        .eye-patches {
            position: absolute;
            top: 14px;
            left: 6px;
            width: 16px;
            height: 20px;
            background-color: #222;
            border-radius: 50%;
            transform: rotate(-15deg);
            box-shadow: 26px -7px 0 #222; /* Secondo occhio specchiato virtualmente */
        }

        /* Pupille Bianche Luccicanti */
        .eyes-white {
            position: absolute;
            top: 20px;
            left: 12px;
            width: 5px;
            height: 5px;
            background-color: #fff;
            border-radius: 50%;
            box-shadow: 26px 0 0 #fff;
            z-index: 3;
        }

        /* Nasino */
        .nose {
            position: absolute;
            top: 28px;
            left: 26px;
            width: 8px;
            height: 5px;
            background-color: #222;
            border-radius: 40% 40% 50% 50%;
        }

        /* Elementi cadenti */
        .falling-item {
            position: absolute;
            font-size: 35px;
            line-height: 1;
            z-index: 4;
            user-select: none;
            -webkit-user-select: none;
        }

        /* Schermate
