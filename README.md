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
    .btn {
      padding: 10px 20px;
      margin: 5px;
      font-size: 18px;
      background: #444;
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
  <div>
    <button class="btn" onclick="manualCraft()">🔨 Craft</button>
    <button class="btn" onclick="move(0,-1)">⬆️</button>
    <br>
    <button class="btn" onclick="move(-1,0)">⬅️</button>
    <button class="btn" onclick="move(1,0)">➡️</button>
    <br>
    <button class="btn" onclick="move(0,1)">⬇️</button>
  </div>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");
    const tileSize = 30;

    const map = [
      ['C','R','R','R','Z','R','P','R','S','R'],
      ['R','R','R','R','R','R','R','R','R','R'],
      ['R','R','P','R','C','R','Z','R','S','R'],
      ['R','R','R','R','R','R','R','R','R','R'],
      ['S','R','R','C','R','Z','R','P','R','R'],
      ['R','R','R','R','R','R','R','R','R','R'],
      ['C','R','Z','R','S','R','C','R','R','R'],
      ['R','R','R','R','R','R','R','R','R','R'],
      ['R','P','R','R','Z','R','S','R','C','R'],
      ['R','R','R','R','R','R','R','R','R','R']
    ];

    const lootTable = {
      'C': ['cuțit', 'lemn', 'lanternă'],
      'P': ['pistol', 'mitralieră', 'muniție'],
      'S': ['apă', 'mâncare', 'conserve']
    };

    const recipes = {
      "cuțit+lemn": "lance",
      "pistol+lanternă": "pistol tactic"
    };

    let lootCooldowns = JSON.parse(localStorage.getItem("cooldowns")) || {};

    const player = {
      x: 0, y: 0,
      life: 100,
      water: 100,
      food: 100,
      inventory: []
    };

    const zombies = [
      { x: 4, y: 0 },
      { x: 6, y: 2 },
      { x: 5, y: 4 },
      { x: 2, y: 6 },
      { x: 4, y: 8 }
    ];

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      for (let y = 0; y < 10; y++) {
        for (let x = 0; x < 10; x++) {
          let tile = map[y][x];
          ctx.fillStyle = tile === 'C' ? "#777" :
                          tile === 'P' ? "#5588ff" :
                          tile === 'S' ? "#33aa33" :
                          tile === 'Z' ? "#aa0000" : "#333";
          ctx.fillRect(x * tileSize, y * tileSize, tileSize-1, tileSize-1);
        }
      }

      // zombii
      for (let z of zombies) {
        ctx.fillStyle = "red";
        ctx.fillRect(z.x * tileSize + 8, z.y * tileSize + 8, 14, 14);
      }

      // jucător
      ctx.fillStyle = "white";
      ctx.fillRect(player.x * tileSize + 6, player.y * tileSize + 6, 18, 18);
    }

    function updateStats() {
      document.getElementById("stats").innerText =
        `❤️ Viață: ${player.life} | 💧 Apă: ${player.water} | 🍖 Hrană: ${player.food}`;
    }

    function move(dx, dy) {
      const nx = player.x + dx;
      const ny = player.y + dy;
      if (nx < 0 || ny < 0 || nx > 9 || ny > 9) return;

      player.x = nx;
      player.y = ny;

      player.water -= 1;
      player.food -= 1;

      checkTile();
      checkZombie();
      updateStats();
      draw();
    }

    function checkTile() {
      const tile = map[player.y][player.x];
      const key = `${player.x},${player.y}`;
      const now = Date.now();
      const cooldown = 5 * 60 * 1000;

      if (lootTable[tile]) {
        const last = lootCooldowns[key];
        if (!last || now - last > cooldown) {
          const loot = lootTable[tile];
          const item = loot[Math.floor(Math.random() * loot.length)];
          player.inventory.push(item);
          document.getElementById("prada").innerText = `🧰 Pradă: + ${item}`;
          lootCooldowns[key] = now;
          localStorage.setItem("cooldowns", JSON.stringify(lootCooldowns));
        } else {
          document.getElementById("prada").innerText = "⏳ Clădirea a fost jefuită recent!";
        }
      } else {
        document.getElementById("prada").innerText = "🔍 Nimic aici...";
      }
    }

    function checkZombie() {
      for (let i = 0; i < zombies.length; i++) {
        const z = zombies[i];
        if (z.x === player.x && z.y === player.y) {
          const hasWeapon = player.inventory.includes("cuțit") || player.inventory.includes("pistol") || player.inventory.includes("mitralieră");
          if (hasWeapon) {
            document.getElementById("prada").innerText = "⚔️ Ai ucis un zombi!";
            zombies.splice(i, 1);
          } else {
            player.life -= 20;
            document.getElementById("prada").innerText = "💀 Ai fost atacat de un zombi!";
          }
        }
      }
    }

    function manualCraft() {
      const inv = player.inventory.join('+');
      for (let key in recipes) {
        const parts = key.split('+');
        if (parts.every(p => player.inventory.includes(p))) {
          player.inventory.push(recipes[key]);
          document.getElementById("prada").innerText = `🔧 Craft: ${recipes[key]}`;
          return;
        }
      }
      document.getElementById("prada").innerText = "❌ Nimic de combinat!";
    }

    draw();
    updateStats();
  </script>
</body>
</html>
