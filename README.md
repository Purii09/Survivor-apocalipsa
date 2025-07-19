<!DOCTYPE html>
<html>
<head>
  <title>Supraviețuitor: Apocalipsa</title>
  <style>
    body {
      margin: 0;
      font-family: monospace;
      background: #111;
      color: white;
      text-align: center;
    }
    canvas {
      background: #222;
      image-rendering: pixelated;
      display: block;
      margin: auto;
    }
    #ui {
      padding: 10px;
      font-size: 14px;
      background: rgba(0,0,0,0.5);
    }
    #controls {
      display: flex;
      justify-content: center;
      gap: 20px;
      padding: 10px;
    }
    .btn {
      font-size: 20px;
      padding: 10px 20px;
      background: #333;
      color: white;
      border: none;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <h1>🧟‍♂️ Supraviețuitor: Apocalipsa</h1>
  <div id="stats">❤️ Viață: 100 | 💧 Apă: 100 | 🍖 Hrană: 100</div>
  <canvas id="game" width="300" height="300"></canvas>
  <div id="prada">🧰 Pradă: nimic</div>
  <div id="craft">🔧 Meșteșug: nimic</div>
  <div id="controls">
    <button class="btn" onclick="move(0,-1)">⬆️</button>
    <button class="btn" onclick="move(-1,0)">⬅️</button>
    <button class="btn" onclick="move(1,0)">➡️</button>
    <button class="btn" onclick="move(0,1)">⬇️</button>
  </div>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");
    const tileSize = 30;
    const map = [
      ['R','R','R','C','C','R','R','R','C','R'],
      ['R','S','R','R','R','R','R','R','C','R'],
      ['R','R','R','R','R','P','R','C','R','R'],
      ['R','R','R','B','R','R','R','R','R','R'],
      ['R','R','R','R','R','R','R','R','R','R'],
      ['R','P','R','R','R','C','C','R','R','R'],
      ['R','R','R','R','R','R','R','S','R','R'],
      ['R','R','C','C','R','R','R','R','R','B'],
      ['R','R','R','R','P','R','R','R','R','R'],
      ['R','R','R','R','R','R','R','R','R','R']
    ]; // C = casa, P = politie, S = supermarket, B = buncăr
    const lootTable = {
      'C': ['baterii', 'tigari', 'lanternă'],
      'P': ['pistol', 'muniție', 'cuțit'],
      'S': ['apă', 'mâncare', 'pastile'],
      'B': ['trusă medicală', 'conserve', 'ham']
    };
    const recipes = {
      "cuțit+lemn": "lance",
      "pistol+lanternă": "pistol tactic"
    };
    const player = {
      x: 0, y: 0, water: 100, food: 100, life: 100,
      gender: "bărbat",
      inventory: []
    };

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      for (let y = 0; y < 10; y++) {
        for (let x = 0; x < 10; x++) {
          const tile = map[y][x];
          ctx.fillStyle = tile === 'C' ? "#555" :
                          tile === 'S' ? "#4bb800" :
                          tile === 'P' ? "#0044ff" :
                          tile === 'B' ? "#888" : "#333";
          ctx.fillRect(x * tileSize, y * tileSize, tileSize-1, tileSize-1);
        }
      }
      ctx.fillStyle = player.gender === "femeie" ? "pink" : "white";
      ctx.fillRect(player.x * tileSize + 5, player.y * tileSize + 5, tileSize - 10, tileSize - 10);
    }

    function move(dx,
