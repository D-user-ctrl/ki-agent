# ki-agent
ki-agent/
├── README.md
├── index.html
├── style.css
├── memory.js
└── agent.js
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <title>KI Agent</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <canvas id="canvas"></canvas>
  <script src="memory.js"></script>
  <script src="agent.js"></script>
</body>
</html>
body {
  margin: 0;
  overflow: hidden;
  background-color: #000;
}
canvas {
  display: block;
}
class Memory {
  constructor(maxSize = 1000) {
    this.maxSize = maxSize;
    this.data = [];
  }

  remember(observation) {
    this.data.push({ time: Date.now(), ...observation });
    if (this.data.length > this.maxSize) this.data.shift();
  }

  recallLatest(n = 10) {
    return this.data.slice(-n);
  }
}

const memory = new Memory();

const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let agent = {
  x: canvas.width / 2,
  y: canvas.height / 2,
  size: 20,
  color: 'white'
};

function draw() {
  ctx.fillStyle = 'rgba(0,0,0,0.2)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  ctx.beginPath();
  ctx.arc(agent.x, agent.y, agent.size, 0, Math.PI * 2);
  ctx.fillStyle = agent.color;
  ctx.fill();
}

canvas.addEventListener('mousemove', (e) => {
  agent.x += (e.clientX - agent.x) * 0.05;
  agent.y += (e.clientY - agent.y) * 0.05;
  memory.remember({ x: agent.x, y: agent.y });
});

function loop() {
  draw();
  requestAnimationFrame(loop);
}

loop();
