# ana-gerador-de-looks-aleatorios-
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bananas de Pijamas - O Jogo! 🍌</title>
  <style>
    /* 🎨 ESTILOS (CSS) */
    body {
      background: linear-gradient(135deg, #fff176, #ffb74d, #81d4fa);
      font-family: 'Comic Sans MS', 'Poppins', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
    }

    .container {
      background: rgba(255, 255, 255, 0.95);
      padding: 25px;
      border-radius: 25px;
      box-shadow: 0 10px 20px rgba(0,0,0,0.15);
      text-align: center;
      max-width: 450px;
      width: 90%;
    }

    h1 {
      color: #fbc02d;
      margin-bottom: 5px;
      text-shadow: 2px 2px #333;
    }

    .controls {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin: 15px 0;
    }

    #music-btn, #restart-btn {
      background-color: #0288d1;
      color: white;
      border: none;
      padding: 10px 15px;
      border-radius: 20px;
      font-weight: bold;
      cursor: pointer;
      transition: 0.2s;
    }

    #music-btn:hover, #restart-btn:hover {
      transform: scale(1.05);
      background-color: #01579b;
    }

    .memory-game {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
      margin-bottom: 20px;
    }

    .card {
      height: 80px;
      background-color: #0288d1;
      background-image: repeating-linear-gradient(45deg, #0288d1, #0288d1 10px, #ffffff 10px, #ffffff 20px);
      border-radius: 12px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 35px;
      cursor: pointer;
      user-select: none;
      transition: transform 0.3s;
    }

    .card.flipped {
      background: #fff;
      transform: rotateY(180deg);
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>🍌 Bananas Match! 🍌</h1>
    <p>Você está pensando o que eu estou pensando, B1?</p>

    <div class="controls">
      <button id="music-btn">🎵 Tocar Música</button>
      <div class="stats">
        <span>⏱️ Tempo: <b id="timer">0</b>s</span>
        <span>🎯 Jogadas: <b id="moves">0</b></span>
      </div>
    </div>

    <div class="memory-game" id="game-board"></div>

    <button id="restart-btn">🔄 Reiniciar Jogo</button>
  </div>

  <audio id="bg-music" loop src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3"></audio>

  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>

  <script>
    const items = ['🍌', '🍌', '👔', '👔', '🐻', '🐻', '🧸', '🧸', '🎩', '🎩', '⭐', '⭐'];

    let flippedCards = [];
    let matchedPairs = 0;
    let moves = 0;
    let timer = 0;
    let timerInterval = null;

    const board = document.getElementById('game-board');
    const movesDisplay = document.getElementById('moves');
    const timerDisplay = document.getElementById('timer');

    function initGame() {
      board.innerHTML = '';
      flippedCards = [];
      matchedPairs = 0;
      moves = 0;
      timer = 0;
      movesDisplay.innerText = moves;
      timerDisplay.innerText = timer;
      
      clearInterval(timerInterval);
      timerInterval = setInterval(() => {
        timer++;
        timerDisplay.innerText = timer;
      }, 1000);

      const shuffled = items.sort(() => Math.random() - 0.5);

      shuffled.forEach((item) => {
        const card = document.createElement('div');
        card.classList.add('card');
        card.dataset.value = item;
        card.addEventListener('click', flipCard);
        board.appendChild(card);
      });
    }

    function flipCard() {
      if (flippedCards.length < 2 && !this.classList.contains('flipped')) {
        this.classList.add('flipped');
        this.innerText = this.dataset.value;
        flippedCards.push(this);

        if (flippedCards.length === 2) {
          moves++;
          movesDisplay.innerText = moves;
          checkMatch();
        }
      }
    }

    function checkMatch() {
      const [card1, card2] = flippedCards;

      if (card1.dataset.value === card2.dataset.value) {
        matchedPairs++;
        flippedCards = [];
        if (matchedPairs === items.length / 2) {
          clearInterval(timerInterval);
          if (typeof confetti === 'function') confetti();
          setTimeout(() => alert(`Você venceu em ${timer}s e ${moves} jogadas! 🍌`), 300);
        }
      } else {
        setTimeout(() => {
          card1.classList.remove('flipped');
          card2.classList.remove('flipped');
          card1.innerText = '';
          card2.innerText = '';
          flippedCards = [];
        }, 800);
      }
    }

    // Controle de Música
    const musicBtn = document.getElementById("music-btn");
    const bgMusic = document.getElementById("bg-music");

    musicBtn.addEventListener("click", () => {
      if (bgMusic.paused) {
        bgMusic.play();
        musicBtn.innerText = "⏸️ Pausar Música";
      } else {
        bgMusic.pause();
        musicBtn.innerText = "🎵 Tocar Música";
      }
    });

    document.getElementById('restart-btn').addEventListener('click', initGame);

    // Inicia o jogo automaticamente
    initGame();
  </script>
</body>
</html>
