<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>Particles Galaxy</title>
<style>
  html, body {
    margin: 0;
    padding: 0;
    overflow: hidden;
    background: #000;
    font-family: sans-serif;
  }
  #info {
    position: absolute;
    left: 10px;
    top: 10px;
    padding: 8px 14px;
    border-radius: 10px;
    background: rgba(255,255,255,0.08);
    color: #d0eaff;
    backdrop-filter: blur(8px);
  }
</style>
</head>
<body>
<canvas id="c"></canvas>
<div id="info">🌌 Galaxy Particles — Move Mouse</div>

<script>
const canvas = document.getElementById("c");
const ctx = canvas.getContext("2d");

let w, h;
function resize() {
  w = canvas.width = window.innerWidth;
  h = canvas.height = window.innerHeight;
}
resize();
window.addEventListener("resize", resize);

let particles = [];
const count = 900;

for (let i = 0; i < count; i++) {
  let angle = Math.random() * Math.PI * 2;
  let radius = Math.random() * 2;
  particles.push({
    x: 0, 
    y: 0,
    angle,
    radius,
    speed: 0.001 + Math.random() * 0.002,
    dist: Math.random() * 280 + 40,
    color: `hsl(${200 + Math.random()*100}, 80%, 70%)`
  });
}

let mouse = {x:0, y:0};
window.addEventListener("mousemove", e => {
  mouse.x = e.clientX - w/2;
  mouse.y = e.clientY - h/2;
});

function draw() {
  ctx.fillStyle = "rgba(0,0,20,0.25)";
  ctx.fillRect(0,0,w,h);

  ctx.save();
  ctx.translate(w/2, h/2);

  for (let p of particles) {
    p.angle += p.speed;

    let mx = mouse.x * 0.002;
    let my = mouse.y * 0.002;

    let x = Math.cos(p.angle + mx) * (p.dist + p.radius);
    let y = Math.sin(p.angle + my) * (p.dist + p.radius * 3);

    ctx.fillStyle = p.color;
    ctx.beginPath();
    ctx.arc(x, y, p.radius, 0, Math.PI*2);
    ctx.fill();
  }

  ctx.restore();
  requestAnimationFrame(draw);
}

draw();
</script>
</body>
</html>
