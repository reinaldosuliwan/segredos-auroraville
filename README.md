<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Jogo da Memória - Auroraville</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: linear-gradient(#0a0a0a, #1a001a);
      color: white;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      min-height: 100vh;
      overflow-x: hidden;
    }

    h1 {
      margin-top: 20px;
      font-size: 2em;
      text-shadow: 2px 2px 5px red;
    }

    .game-board {
      display: grid;
      grid-template-columns: repeat(4, 80px);
      grid-gap: 15px;
      margin: 30px;
    }

    .card {
      width: 80px;
      height: 80px;
      background-color: #444;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 30px;
      border-radius: 10px;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    .card.revealed {
      background-color: #eee;
      color: black;
    }

    .controls {
      margin-bottom: 20px;
    }

    button {
      background: #550000;
      color: white;
      border: none;
      padding: 10px;
      margin: 10px;
      border-radius: 5px;
      cursor: pointer;
    }

    button:hover {
      background: #770000;
    }

    audio {
      display: none;
    }
  </style>
</head>
<body>
  <h1>Segredos de Auroraville</h1>

  <div class="controls">
    <button onclick="startGame()">Reiniciar</button>
    <button onclick="toggleMusic()">🔊 Música</button>
  </div>

  <div class="game-board" id="board"></div>

  <audio id="flip-sound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_d7d9979df0.mp3?filename=click-124467.mp3"></audio>
  <audio id="bg-music" loop src="https://cdn.pixabay.com/download/audio/2023/06/07/audio_2d0e5dc320.mp3?filename=dark-vampire-horror-music-144193.mp3"></audio>

  <script>
    const icons = ["🦇", "🧛‍♂️", "🌕", "🕸️", "🪦", "🩸", "🕯️", "⚰️"];
    let cards = [];
    let firstCard = null;
    let lock = false;

    const board = document.getElementById('board');
    const flipSound = document.getElementById('flip-sound');
    const bgMusic = document.getElementById('bg-music');

    function startGame() {
      cards = [...icons, ...icons].sort(() => 0.5 - Math.random());
      board.innerHTML = '';
      firstCard = null;
      lock = false;

      cards.forEach((icon, i) => {
        const card = document.createElement('div');
        card.className = 'card';
        card.dataset.icon = icon;
        card.dataset.index = i;
        card.addEventListener('click', revealCard);
        board.appendChild(card);
      });
    }

    function revealCard(e) {
      if (lock) return;
      const card = e.currentTarget;
      const icon = card.dataset.icon;
      const index = card.dataset.index;

      if (card.classList.contains('revealed')) return;

      card.textContent = icon;
      card.classList.add('revealed');
      flipSound.currentTime = 0;
      flipSound.play();

      if (!firstCard) {
        firstCard = { card, icon, index };
      } else {
        if (firstCard.icon === icon && firstCard.index !== index) {
          firstCard = null;
        } else {
          lock = true;
          setTimeout(() => {
            card.classList.remove('revealed');
            card.textContent = '';
            firstCard.card.classList.remove('revealed');
            firstCard.card.textContent = '';
            firstCard = null;
            lock = false;
          }, 1000);
        }
      }
    }

    function toggleMusic() {
      if (bgMusic.paused) {
        bgMusic.volume = 0.4;
        bgMusic.play();
      } else {
        bgMusic.pause();
      }
    }

    startGame();
  </script>
</body>
</html>
