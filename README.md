<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Forgive Me?</title>
  <style>
    body {
      margin: 0;
      font-family: 'Comic Sans MS', cursive, sans-serif;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      overflow: hidden;
      background-image: url('unknown.png') no-repeat center center fixed;
      background-size: cover;
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }

    canvas#rain {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 1;
      pointer-events: none;
    }

    .container {
      background: linear-gradient(145deg, rgba(255, 255, 255, 0.85), rgba(200, 200, 200, 0.7)); /* Soft gradient */
      border-radius: 20px;
      padding: 30px;
      max-width: 500px;
      text-align: center;
      box-shadow: 0 10px 25px rgba(0,0,0,0.3);
      z-index: 2;
    }

    .message {
      font-size: 1.4rem;
      margin-bottom: 20px;
      min-height: 100px;
      opacity: 0;
      transition: opacity 1s ease, transform 0.5s ease;
    }

    .message.show {
      opacity: 1;
      transform: translateY(0);
    }

    .message.hide {
      transform: translateY(20px);
    }

    .next-btn, .choice-btn {
      background-color: #f39c12;
      border: none;
      padding: 10px 20px;
      font-size: 1.2rem;
      border-radius: 10px;
      cursor: pointer;
      transition: all 0.3s ease; /* Smoother transition */
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Subtle shadow */
    }

    .next-btn:hover, .choice-btn:hover {
      background-color: #d35400;
      color: white;
      transform: scale(1.05); /* Slightly enlarges the button */
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4); /* Bigger shadow */
    }

    .choices {
      display: flex;
      justify-content: space-around;
      margin-top: 20px;
    }

    .emoji {
      font-size: 2rem;
      animation: pop 0.5s ease;
    }

    @keyframes pop {
      0% { transform: scale(0); opacity: 0; }
      100% { transform: scale(1); opacity: 1; }
    }

    .sparkle {
      position: absolute;
      width: 20px;
      height: 20px;
      background: radial-gradient(white, rgba(255, 255, 255, 0));
      border-radius: 50%;
      opacity: 0.8;
      animation: sparkle 2s ease-in-out infinite, shimmer 1.5s infinite alternate;
    }

    @keyframes shimmer {
      0% {
        transform: scale(0.5);
        opacity: 0.8;
        background: rgba(255, 255, 255, 0.7);
      }
      100% {
        transform: scale(1.5);
        opacity: 1;
        background: rgba(255, 215, 0, 0.9); /* Gold shimmer */
      }
    }

    @keyframes sparkle {
      0% { transform: scale(0.5); opacity: 0.8; }
      50% { transform: scale(1.5); opacity: 1; }
      100% { transform: scale(0); opacity: 0; }
    }

    .progress-bar-container {
      width: 100%;
      background-color: #ccc;
      border-radius: 10px;
      margin-top: 20px;
    }

    .progress-bar {
      height: 10px;
      width: 0%;
      background-color: #f39c12;
      border-radius: 10px;
      transition: width 0.5s ease-in-out;
    }
  </style>
</head>
<body>
  <canvas id="rain"></canvas>

  <div class="container">
    <div class="message" id="message"></div>
    <button class="next-btn" id="nextBtn">Next</button>
    <div class="choices" id="choices" style="display: none;">
      <button class="choice-btn" onclick="showResponse(true)">Yes, I forgive you ❤️</button>
      <button class="choice-btn" onclick="showResponse(false)">No, I’m still mad 😤</button>
    </div>
    <div class="emoji" id="emoji"></div>
    <div class="progress-bar-container">
      <div class="progress-bar" id="progressBar"></div>
    </div>
  </div>

  <!-- Pirate Music -->
  <audio autoplay loop>
    <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_8f7d2f9e7b.mp3?filename=pirate-tavern-110874.mp3" type="audio/mpeg">
  </audio>

  <!-- Rain Sound -->
  <audio autoplay loop>
    <source src="https://cdn.pixabay.com/download/audio/2021/08/04/audio_d7fa3c8615.mp3?filename=light-rain-ambient-110689.mp3" type="audio/mpeg">
  </audio>

  <script>
    const messages = [
      "Hey... I know this might be the last thing you want to see from me.",
      "But I’ve been thinking about that one hour we talked… it actually meant something to me.",
      "What happened after—that rude stuff? That wasn’t me. My friend used my account and messed up badly.",
      "I should’ve protected that moment. I take full responsibility, and I’m really sorry.",
      "So this is me, doing what I can... hoping you’ll hear the real me. Would you forgive me?"
    ];

    let index = 0;
    const messageDiv = document.getElementById("message");
    const nextBtn = document.getElementById("nextBtn");
    const choices = document.getElementById("choices");
    const emoji = document.getElementById("emoji");
    const progressBar = document.getElementById("progressBar");

    function showMessage() {
      messageDiv.classList.remove("show");
      messageDiv.classList.add("hide");

      setTimeout(() => {
        messageDiv.textContent = messages[index];
        messageDiv.classList.remove("hide");
        messageDiv.classList.add("show");
        index++;
        progressBar.style.width = `${(index / messages.length) * 100}%`;
        if (index === messages.length) {
          nextBtn.style.display = "none";
          choices.style.display = "flex";
        }
      }, 500);
    }

    function showResponse(forgiven) {
      choices.style.display = "none";
      if (forgiven) {
        messageDiv.textContent = "Thank you... this means a lot 💖";
        emoji.textContent = "🤗";
        addSparkles(20);
      } else {
        messageDiv.textContent = "It’s okay... I understand 😞";
        emoji.textContent = "💔";
      }
    }

    function addSparkles(count) {
      for (let i = 0; i < count; i++) {
        const sparkle = document.createElement("div");
        sparkle.className = "sparkle";
        sparkle.style.left = `${Math.random() * 100}%`;
        sparkle.style.top = `${Math.random() * 100}%`;
        document.body.appendChild(sparkle);
        setTimeout(() => sparkle.remove(), 3000);
      }
    }

    nextBtn.addEventListener("click", showMessage);
    showMessage();

    // Rain Animation
    const canvas = document.getElementById('rain');
    const ctx = canvas.getContext('2d');
    let drops = [];

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }

    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    function createDrops() {
      drops = [];
      for (let i = 0; i < 200; i++) {
        drops.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height,
          length: Math.random() * 20 + 10,
          speed: Math.random() * 5 + 2,
          color: `rgba(${Math.random() * 255}, ${Math.random() * 255}, ${Math.random() * 255}, 0.7)`
        });
      }
    }

    function drawRain() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.beginPath();
      for (let drop of drops) {
        ctx.strokeStyle = drop.color;
        ctx.moveTo(drop.x, drop.y);
        ctx.lineTo(drop.x, drop.y + drop.length);
      }
      ctx.stroke();
      updateRain();
      requestAnimationFrame(drawRain);
    }

    function updateRain() {
      for (let drop of drops) {
        drop.y += drop.speed;
        if (drop.y > canvas.height) {
          drop.y = -drop.length;
          drop.x = Math.random() * canvas.width;
        }
      }
    }

    createDrops();
    drawRain();
  </script>
</body>
</html>
