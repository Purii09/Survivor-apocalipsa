<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Survivor: Apocalipsa</title>
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
      text-align: left;
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
  <h2>🎮 Survivor: Apocalipsa</h2>
  <canvas id="game" width="300" height="300"></canvas>
  <div id="ui">
    <div id="stats">❤️ Viață: 100 | 💧 Apă: 100 | 🍖 Hrană: 100</div>
    <div id="loot">📦 Loot: nimic</div>
    <div id="craft">🔧 Craft: nimic</div>
  </div>
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
      ['R','R','S','R','R','C','C','R','R','R'],
      ['R','R','R','R','C','R','R','R','P','R'],
      ['R','R','R','C','R','R','R','R','R','R'],
      ['C','R','R','R','R','C','C','R','R','R'],
      ['R','R','B','R','R','R','R','R','C','R'],
      ['R','C','R','R','C','R','S','R','R','R'],
      ['R','R','R','C','R','R','R','R','R','R'],
      ['S','R','C','R','R','P','R','C','R','R'],
      ['R','R','R','R','C','R','R','R','R','B'],
      ['C','R','R','R','R','R','C','R','S','R'],
    ];

    const lootTable = {
      S: ["apă", "mâncare"],
      P: ["pistol", "muniție", "cuțit"],
      B: ["trusă medicală", "pastile", "lemn"],
      C: ["baterii", "țigări", "lanternă"]
    };

    const recipes = {
      "cuțit+lemn": "lance",
      "pastile+apă": "ser"
    };

    const player = {
      x: 0,
      y: 0,
      hp: 100,
      water: 100,
      food: 100,
      inventory: [],
      gender: prompt("Alege personajul (bărbat/femeie):") === "femeie" ? "femeie" : "bărbat"
    };

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      for (let y = 0; y < 10; y++) {
        for (let x = 0; x < 10; x++) {
          const tile = map[y][x];
          switch(tile) {
            case 'R': ctx.fillStyle = "#444"; break; // drum
            case 'S': ctx.fillStyle = "#2e8b57"; break; // supermarket
            case 'P': ctx.fillStyle = "#1e90ff"; break; // politie
            case 'B': ctx.fillStyle = "#8b0000"; break; // buncăr
            case 'C': ctx.fillStyle = "#555"; break; // casa
          }
          ctx.fillRect(x * tileSize, y * tileSize, tileSize - 1, tileSize - 1);
        }
      }

      // Desenează playerul
      ctx.fillStyle = player.gender === "femeie" ? "pink" : "skyblue";
      ctx.fillRect(player.x * tileSize + 5, player.y * tileSize + 5, tileSize - 10, tileSize - 10);
    }

    function move(dx, dy) {
      const nx = player.x + dx;
      const ny = player.y + dy;
      if (nx >= 0 && nx < 10 && ny >= 0 && ny < 10) {
        player.x = nx;
        player.y = ny;
        checkTile();
        draw();
      }
    }

    function checkTile() {
      const tile = map[player.y][player.x];
      const items = lootTable[tile];
      if (items) {
        const found = items[Math.floor(Math.random() * items.length)];
        player.inventory.push(found);
        document.getElementById("loot").innerText = "📦 Loot: " + found;
        updateStats();
        checkCrafting();
      }
    }

    function checkCrafting() {
      const inv = player.inventory;
      for (let recipe in recipes) {
        const [item1, item2] = recipe.split("+");
        if (inv.includes(item1) && inv.includes(item2)) {
          const newItem = recipes[recipe];
          inv.splice(inv.indexOf(item1), 1);
          inv.splice(inv.indexOf(item2), 1);
          inv.push(newItem);
          document.getElementById("craft").innerText = "🔧 Craft: " + newItem;
          break;
        }
      }
    }

    function updateStats() {
      document.getElementById("stats").innerHTML =
        `❤️ Viață: ${player.hp} | 💧 Apă: ${player.water} | 🍖 Hrană: ${player.food}`;
    }

    draw();
    updateStats();
  </script>
</body>
</html>
