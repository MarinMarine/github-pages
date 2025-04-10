<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fourmilière Panic!</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      font-family: Arial, sans-serif;
      background: url('https://hebbkx1anhila5yf.public.blob.vercel-storage.com/ChatGPT%20Image%2010%20avr.%202025%2C%2009_27_04-idmprwf0GRRe3XxcAwV79x1OOx3FES.png') no-repeat center center fixed;
      background-size: cover;
    }
    
    h1 {
      position: absolute;
      top: 10px;
      left: 50%;
      transform: translateX(-50%);
      color: white;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
      margin: 0;
      padding: 10px 20px;
      background-color: rgba(0, 0, 0, 0.5);
      border-radius: 8px;
      z-index: 2000;
    }
    
    #score, #topScores, #bestPlayer, #playerCount {
      position: absolute;
      top: 10px;
      left: 10px;
      font-size: 18px;
      background: rgba(255, 255, 255, 0.9);
      padding: 8px 12px;
      border-radius: 5px;
      margin-bottom: 5px;
      display: none;
      z-index: 2000;
    }
    
    #bestPlayer {
      top: 50px;
    }
    
    #playerCount {
      top: 90px;
    }
    
    #timer {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 24px;
      background: rgba(255, 255, 255, 0.9);
      padding: 8px 12px;
      border-radius: 5px;
      display: none;
      z-index: 2000;
      color: #ff3333;
      font-weight: bold;
    }
    
    #timer-bar {
      position: absolute;
      top: 50px;
      right: 10px;
      width: 200px;
      height: 20px;
      background-color: rgba(255, 255, 255, 0.5);
      border-radius: 10px;
      overflow: hidden;
      display: none;
      z-index: 2000;
    }
    
    #timer-progress {
      height: 100%;
      width: 100%;
      background-color: #ff3333;
      border-radius: 10px;
      transition: width 1s linear;
    }
    
    #topScores {
      position: absolute;
      bottom: 10px;
      left: 10px;
      top: auto;
      font-size: 16px;
      background: rgba(255, 255, 255, 0.9);
      padding: 8px 12px;
      border-radius: 5px;
      display: none;
      z-index: 2000;
    }
    
    .ant {
      position: absolute;
      width: 60px;
      height: 60px;
      cursor: pointer;
      z-index: 3000;
      transition: transform 0.1s;
    }
    
    .ant-good {
      background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="%23000000"/><circle cx="50" cy="50" r="35" fill="%23333333"/><circle cx="35" cy="40" r="5" fill="%23ffffff"/><circle cx="65" cy="40" r="5" fill="%23ffffff"/><path d="M40 65 Q50 75 60 65" stroke="%23ffffff" stroke-width="3" fill="none"/></svg>');
      background-size: contain;
      background-repeat: no-repeat;
    }
    
    .ant-bad {
      background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="%23ff6600"/><circle cx="50" cy="50" r="35" fill="%23ff8800"/><circle cx="35" cy="40" r="5" fill="%23ffffff"/><circle cx="65" cy="40" r="5" fill="%23ffffff"/><path d="M40 70 Q50 60 60 70" stroke="%23ffffff" stroke-width="3" fill="none"/></svg>');
      background-size: contain;
      background-repeat: no-repeat;
    }
    
    .ant:hover {
      transform: scale(1.2);
    }
    
    .player-form {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      text-align: center;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      z-index: 2000;
    }
    
    .player-form input {
      padding: 8px;
      margin-top: 10px;
      width: 200px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    
    .player-form button, .start-button {
      margin-top: 20px;
      padding: 10px 20px;
      background-color: #4caf50;
      color: white;
      border: none;
      border-radius: 4px;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    
    .player-form button:hover, .start-button:hover {
      background-color: #45a049;
    }
    
    .game-over {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      text-align: center;
      font-size: 24px;
      display: none;
      z-index: 2000;
    }
    
    .game-over img {
      width: 200px;
      height: 200px;
      margin: 15px 0;
      object-fit: contain;
    }
    
    #rules {
      font-size: 14px;
      margin-top: 10px;
      color: #555;
    }
    
    .difficulty {
      margin-top: 15px;
    }
    
    .difficulty button {
      margin: 5px;
      padding: 8px 15px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    
    .difficulty button.easy {
      background-color: #4caf50;
      color: white;
    }
    
    .difficulty button.medium {
      background-color: #ff9800;
      color: white;
    }
    
    .difficulty button.hard {
      background-color: #f44336;
      color: white;
    }
    
    .swarm-warning {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: rgba(255, 0, 0, 0.7);
      color: white;
      padding: 20px;
      border-radius: 10px;
      font-size: 24px;
      font-weight: bold;
      display: none;
      z-index: 2500;
      animation: pulse 1s infinite;
    }
    
    @keyframes pulse {
      0% { transform: translate(-50%, -50%) scale(1); }
      50% { transform: translate(-50%, -50%) scale(1.1); }
      100% { transform: translate(-50%, -50%) scale(1); }
    }
    
    /* Styles pour l'image de trophée */
    .trophy-ant {
      width: 200px;
      height: 200px;
      background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200"><circle cx="100" cy="100" r="80" fill="%23000000"/><circle cx="100" cy="100" r="60" fill="%23333333"/><circle cx="70" cy="80" r="10" fill="%23ffffff"/><circle cx="130" cy="80" r="10" fill="%23ffffff"/><path d="M80 130 Q100 150 120 130" stroke="%23ffffff" stroke-width="5" fill="none"/><path d="M100 40 L120 20 L140 40 L120 60 Z" fill="%23ffd700"/><circle cx="130" cy="30" r="5" fill="%23ffffff"/></svg>');
      background-size: contain;
      background-repeat: no-repeat;
      margin: 15px auto;
    }
  </style>
</head>
<body>
  <audio id="horn" src="https://www.soundjay.com/transportation/train-horn-01.mp3"></audio>
  
  <h1>Fourmilière Panic!</h1>
  
  <div class="player-form" id="playerForm">
    <h2>Bienvenue dans Fourmilière Panic! 🐜</h2>
    <p>Entrez votre prénom ou pseudo :</p>
    <input type="text" id="playerName" placeholder="Ex : Lulu, Marco...">
    <div class="difficulty">
      <p>Choisissez la difficulté :</p>
      <button class="easy" onclick="setDifficulty('easy')">Facile</button>
      <button class="medium" onclick="setDifficulty('medium')">Moyen</button>
      <button class="hard" onclick="setDifficulty('hard')">Difficile</button>
    </div>
    <br>
    <button onclick="startGame()">Jouer</button>
    <div id="rules">Clique sur les fourmis noires pour marquer des points.<br>Ne clique pas sur les fourmis oranges !</div>
  </div>
  
  <div id="score">Score : 0</div>
  <div id="bestPlayer">Meilleur joueur : ???</div>
  <div id="playerCount">Joueurs totaux : 0</div>
  <div id="topScores">Chargement du classement...</div>
  <div id="timer">60</div>
  <div id="timer-bar"><div id="timer-progress"></div></div>
  
  <div class="swarm-warning" id="swarmWarning">ATTENTION ! INVASION DE FOURMIS !</div>
  
  <div class="game-over" id="gameOver">
    <div id="finalScore"></div>
    <div class="trophy-ant" id="trophyAnt"></div>
    <button class="start-button" onclick="startGame()">Rejouer</button>
  </div>
  
  <script>
    const scoreElement = document.getElementById('score');
    const gameOverElement = document.getElementById('gameOver');
    const finalScoreElement = document.getElementById('finalScore');
    const playerFormElement = document.getElementById('playerForm');
    const topScoresElement = document.getElementById('topScores');
    const bestPlayerElement = document.getElementById('bestPlayer');
    const playerCountElement = document.getElementById('playerCount');
    const trophyAntElement = document.getElementById('trophyAnt');
    const hornSound = document.getElementById('horn');
    const timerElement = document.getElementById('timer');
    const timerBarElement = document.getElementById('timer-bar');
    const timerProgressElement = document.getElementById('timer-progress');
    const swarmWarningElement = document.getElementById('swarmWarning');
    
    // For demo purposes, we'll use local storage instead of Google Apps Script
    // In a real implementation, you would use the fetch API to connect to your backend
    const scriptURL = "https://script.google.com/macros/s/AKfycbzMI5tBY8n7pA2AHurjIGYDqmq0Gh4zrXtmPMKh9eHMqM0euSXXPDSXsJQAABmvSXRvNg/exec";
    
    let score = 0;
    let gameInterval;
    let isGameRunning = false;
    let playerName = "";
    let spawnDelay = 800;
    let timer;
    let timeLeft = 60;
    let timerInterval;
    let topPlayers = [];
    let swarmInterval;
    let difficulty = 'medium'; // Default difficulty
    
    // Difficulty settings
    const difficultySettings = {
      easy: {
        initialDelay: 1000,
        minDelay: 500,
        accelerationStep: 100,
        accelerationInterval: 20000,
        antsPerSpawn: 1,
        swarmInterval: 30000,
        swarmSize: 5
      },
      medium: {
        initialDelay: 800,
        minDelay: 300,
        accelerationStep: 150,
        accelerationInterval: 15000,
        antsPerSpawn: 2,
        swarmInterval: 20000,
        swarmSize: 8
      },
      hard: {
        initialDelay: 600,
        minDelay: 200,
        accelerationStep: 200,
        accelerationInterval: 10000,
        antsPerSpawn: 3,
        swarmInterval: 15000,
        swarmSize: 12
      }
    };
    
    // Mock data for leaderboard (replace with actual API calls in production)
    let leaderboardData = [
      { name: "Emma", score: 25 },
      { name: "Lucas", score: 22 },
      { name: "Sophie", score: 18 },
      { name: "Thomas", score: 15 },
      { name: "Julie", score: 12 }
    ];
    
    // Set difficulty
    function setDifficulty(level) {
      difficulty = level;
      
      // Highlight selected button
      document.querySelectorAll('.difficulty button').forEach(btn => {
        btn.style.opacity = '0.7';
        btn.style.transform = 'scale(1)';
      });
      
      document.querySelector(`.difficulty button.${level}`).style.opacity = '1';
      document.querySelector(`.difficulty button.${level}`).style.transform = 'scale(1.1)';
    }
    
    // Start the game
    function startGame() {
      playerName = document.getElementById("playerName").value.trim();
      if (!playerName) {
        alert("Entrez un prénom ou pseudo !");
        return;
      }
      
      // Get settings for selected difficulty
      const settings = difficultySettings[difficulty];
      spawnDelay = settings.initialDelay;
      
      // Play horn sound
      hornSound.play().catch(e => console.log("Audio playback failed:", e));
      
      // Reset game state
      score = 0;
      timeLeft = 60;
      scoreElement.textContent = "Score : 0";
      timerElement.textContent = timeLeft;
      timerProgressElement.style.width = "100%";
      timerElement.style.color = "#ff3333";
      timerProgressElement.style.backgroundColor = "#ff3333";
      
      gameOverElement.style.display = 'none';
      playerFormElement.style.display = 'none';
      scoreElement.style.display = 'block';
      topScoresElement.style.display = 'block';
      bestPlayerElement.style.display = 'block';
      playerCountElement.style.display = 'block';
      timerElement.style.display = 'block';
      timerBarElement.style.display = 'block';
      
      // Remove any existing ants
      const existingAnts = document.querySelectorAll('.ant');
      existingAnts.forEach(ant => ant.remove());
      
      isGameRunning = true;
      
      // Create ants at random intervals
      clearInterval(gameInterval);
      gameInterval = setInterval(() => {
        // Spawn multiple ants based on difficulty
        for (let i = 0; i < settings.antsPerSpawn; i++) {
          spawnAnt();
        }
      }, spawnDelay);
      
      // Accelerate the game periodically
      setTimeout(() => accelerate(settings), settings.accelerationInterval);
      
      // Set up swarm events
      clearInterval(swarmInterval);
      swarmInterval = setInterval(() => {
        triggerSwarm(settings.swarmSize);
      }, settings.swarmInterval);
      
      // Set game timer (60 seconds)
      clearInterval(timerInterval);
      timerInterval = setInterval(updateTimer, 1000);
      clearTimeout(timer);
      timer = setTimeout(endGame, 60000);
      
      // Update leaderboard
      fetchTopScores();
    }
    
    // Trigger a swarm of ants
    function triggerSwarm(size) {
      if (!isGameRunning) return;
      
      // Show warning
      swarmWarningElement.style.display = 'block';
      setTimeout(() => {
        swarmWarningElement.style.display = 'none';
      }, 2000);
      
      // Spawn a swarm of ants
      for (let i = 0; i < size; i++) {
        setTimeout(() => {
          if (isGameRunning) spawnAnt();
        }, i * 100); // Stagger the spawns slightly
      }
    }
    
    // Update timer display
    function updateTimer() {
      timeLeft--;
      timerElement.textContent = timeLeft;
      
      // Update progress bar
      const percentage = (timeLeft / 60) * 100;
      timerProgressElement.style.width = percentage + "%";
      
      // Change color based on time remaining
      if (timeLeft <= 10) {
        timerElement.style.color = "#ff0000";
        timerProgressElement.style.backgroundColor = "#ff0000";
      } else if (timeLeft <= 30) {
        timerElement.style.color = "#ff6600";
        timerProgressElement.style.backgroundColor = "#ff6600";
      }
    }
    
    // Accelerate the game by decreasing spawn delay
    function accelerate(settings) {
      if (spawnDelay > settings.minDelay && isGameRunning) {
        spawnDelay -= settings.accelerationStep;
        if (spawnDelay < settings.minDelay) spawnDelay = settings.minDelay;
        
        clearInterval(gameInterval);
        gameInterval = setInterval(() => {
          // Spawn multiple ants based on difficulty
          for (let i = 0; i < settings.antsPerSpawn; i++) {
            spawnAnt();
          }
        }, spawnDelay);
        
        setTimeout(() => accelerate(settings), settings.accelerationInterval);
      }
    }
    
    // Create a new ant at a random position
    function spawnAnt() {
      if (!isGameRunning) return;
      
      const ant = document.createElement('div');
      ant.className = 'ant';
      
      // Determine if this is a good ant (40% chance) or bad ant (60% chance)
      const isGood = Math.random() < 0.4;
      
      // Use SVG-based ants with better visibility
      if (isGood) {
        ant.classList.add('ant-good'); // Black ant (good)
      } else {
        ant.classList.add('ant-bad'); // Orange ant (bad)
      }
      
      // Random position within the window
      const maxX = window.innerWidth - 60;
      const maxY = window.innerHeight - 60;
      const randomX = Math.floor(Math.random() * maxX);
      const randomY = Math.floor(Math.random() * maxY);
      
      ant.style.left = `${randomX}px`;
      ant.style.top = `${randomY}px`;
      
      // Random rotation
      const randomRotation = Math.floor(Math.random() * 360);
      ant.style.transform = `rotate(${randomRotation}deg)`;
      
      // Click event to squash the ant
      ant.onclick = () => {
        if (!isGameRunning || ant.clicked) return;
        
        ant.clicked = true;
        
        if (isGood) {
          score -= 1; // Lose a point for squashing good ants (black)
        } else {
          score += 1; // Gain a point for squashing bad ants (orange)
        }
        
        scoreElement.textContent = "Score : " + score;
        
        // Add squash effect
        ant.style.transform = 'scale(0.5)';
        ant.style.opacity = '0';
        setTimeout(() => ant.remove(), 300);
      };
      
      // Auto-remove after 1.5 seconds
      setTimeout(() => {
        if (ant.parentNode === document.body) {
          ant.style.opacity = '0';
          setTimeout(() => ant.remove(), 300);
        }
      }, 1500);
      
      document.body.appendChild(ant);
    }
    
    // End the game
    function endGame() {
      isGameRunning = false;
      clearInterval(gameInterval);
      clearInterval(timerInterval);
      clearInterval(swarmInterval);
      clearTimeout(timer);
      
      // Hide timer and warnings
      timerElement.style.display = 'none';
      timerBarElement.style.display = 'none';
      swarmWarningElement.style.display = 'none';
      
      // Remove any remaining ants
      document.querySelectorAll('.ant').forEach(ant => {
        ant.style.opacity = '0';
        setTimeout(() => ant.remove(), 300);
      });
      
      finalScoreElement.textContent = `Fin du jeu !\n${playerName}, score final : ${score}`;
      
      // Ensure trophy ant is visible
      trophyAntElement.style.display = 'block';
      
      // Show game over screen
      gameOverElement.style.display = 'block';
      
      // Save score to leaderboard
      postScore();
    }
    
    // Save score to leaderboard
    function postScore() {
      // If you have a Google Apps Script backend:
      if (scriptURL) {
        fetch(scriptURL, {
          method: 'POST',
          body: JSON.stringify({ name: playerName, score: score })
        }).then(() => fetchTopScores())
        .catch(error => {
          console.error("Error posting score:", error);
          // Fallback to local storage if API fails
          saveScoreLocally();
        });
      } else {
        saveScoreLocally();
      }
    }
    
    // Save score locally (fallback)
    function saveScoreLocally() {
      leaderboardData.push({ name: playerName, score: score });
      leaderboardData.sort((a, b) => b.score - a.score);
      leaderboardData = leaderboardData.slice(0, 5); // Keep only top 5
      updateLeaderboard();
    }
    
    // Fetch top scores from API
    function fetchTopScores() {
      if (scriptURL) {
        fetch(scriptURL)
          .then(res => res.json())
          .then(data => {
            const sorted = data.sort((a, b) => b.score - a.score);
            topPlayers = sorted;
            const top = sorted.slice(0, 5);
            updateLeaderboardWithData(top, sorted[0], sorted.length);
          })
          .catch(error => {
            console.error("Error fetching scores:", error);
            // Fallback to local data if API fails
            updateLeaderboard();
          });
      } else {
        updateLeaderboard();
      }
    }
    
    // Update the leaderboard with API data
    function updateLeaderboardWithData(topFive, bestPlayer, playerCount) {
      let html = "<strong>🏆 Classement :</strong><br>";
      topFive.forEach((entry, i) => {
        html += `${i + 1}. ${entry.name} — ${entry.score}<br>`;
      });
      topScoresElement.innerHTML = html;
      
      // Update best player
      if (bestPlayer) {
        bestPlayerElement.innerText = `Meilleur joueur : ${bestPlayer.name} — ${bestPlayer.score}`;
      }
      
      // Update player count
      playerCountElement.innerText = `Joueurs totaux : ${playerCount}`;
    }
    
    // Update the leaderboard with local data
    function updateLeaderboard() {
      let html = "<strong>🏆 Classement :</strong><br>";
      leaderboardData.forEach((entry, i) => {
        html += `${i + 1}. ${entry.name} — ${entry.score}<br>`;
      });
      topScoresElement.innerHTML = html;
      
      // Update best player
      if (leaderboardData.length > 0) {
        bestPlayerElement.innerText = `Meilleur joueur : ${leaderboardData[0].name} — ${leaderboardData[0].score}`;
      }
      
      // Update player count
      playerCountElement.innerText = `Joueurs totaux : ${leaderboardData.length}`;
    }
    
    // Initialize leaderboard and set default difficulty
    updateLeaderboard();
    setDifficulty('medium');
  </script>
</body>
</html>
