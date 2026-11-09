
<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Football Game</title>
<style>
body {
  margin: 0;
  background: #111;
  color: white;
  text-align: center;
  font-family: Arial;
}
h2 { color: lime; }
canvas {
  width: 95%;
  max-width: 800px;
  background: #168b39;
  border: 3px solid white;
  touch-action: none;
}
button {
  padding: 14px 20px;
  margin: 5px;
  font-size: 20px;
  border-radius: 12px;
  border: none;
  background: #333;
  color: white;
}
</style>
</head>
<body>
<h2>⚽ FOOTBALL GAME</h2>
<h3 id="score">0 - 0</h3>
<canvas id="game" width="800" height="450"></canvas>
<div>
  <button id="up">⬆️</button><br>
  <button id="left">⬅️</button>
  <button id="down">⬇️</button>
  <button id="right">➡️</button>
  <button id="shoot">⚽ SHOOT</button>
</div>
<script>
const c = document.getElementById("game");
const ctx = c.getContext("2d");

let player = {x:150,y:225};
let ball = {x:400,y:225,vx:0,vy:0};
let score = 0;
let keys = {};

function draw() {
  ctx.clearRect(0,0,800,450);

  // Maydon
  ctx.fillStyle="#168b39";
  ctx.fillRect(0,0,800,450);
  ctx.strokeStyle="white";
  ctx.lineWidth=4;
  ctx.strokeRect(15,15,770,420);
  ctx.beginPath();
  ctx.moveTo(400,15);
  ctx.lineTo(400,435);
  ctx.stroke();
  ctx.beginPath();
  ctx.arc(400,225,65,0,Math.PI*2);
  ctx.stroke();

  // Darvozalar
  ctx.strokeRect(15,150,55,150);
  ctx.strokeRect(730,150,55,150);

  // Raqib darvozaboni
  ctx.fillStyle="red";
  ctx.fillRect(750,195,20,60);

  // O'yinchi
  ctx.fillStyle="#00cfff";
  ctx.beginPath();
  ctx.arc(player.x,player.y,18,0,Math.PI*2);
  ctx.fill();

  // To'p
  ctx.fillStyle="white";
  ctx.beginPath();
  ctx.arc(ball.x,ball.y,10,0,Math.PI*2);
  ctx.fill();
  ctx.strokeStyle="black";
  ctx.stroke();

  // Harakat
  if(keys.up) player.y-=3;
  if(keys.down) player.y+=3;
  if(keys.left) player.x-=3;
  if(keys.right) player.x+=3;

  player.x=Math.max(30,Math.min(770,player.x));
  player.y=Math.max(30,Math.min(420,player.y));

  ball.x+=ball.vx;
  ball.y+=ball.vy;
  ball.vx*=0.98;
  ball.vy*=0.98;

  if(ball.y<25 || ball.y>425) ball.vy*=-1;
  if(ball.x<20) ball.vx*=-1;

  // Gol
  if(ball.x>750 && ball.y>150 && ball.y<300) {
    score++;
    document.getElementById("score").innerText=score+" - 0";
    ball.x=400; ball.y=225;
    ball.vx=0; ball.vy=0;
  }

  // To'pni olib yurish
  let d=Math.hypot(player.x-ball.x,player.y-ball.y);
  if(d<28 && Math.abs(ball.vx)+Math.abs(ball.vy)<2) {
    ball.x+=(player.x-ball.x)*0.08;
    ball.y+=(player.y-ball.y)*0.08;
  }

  requestAnimationFrame(draw);
}

function shoot() {
  let d=Math.hypot(player.x-ball.x,player.y-ball.y);
  if(d<45) {
    ball.vx=8;
    ball.vy=(ball.y-player.y)*0.25;
  }
}

document.addEventListener("keydown",e=>{
  if(e.key==="ArrowUp"||e.key==="w") keys.up=true;
  if(e.key==="ArrowDown"||e.key==="s") keys.down=true;
  if(e.key==="ArrowLeft"||e.key==="a") keys.left=true;
  if(e.key==="ArrowRight"||e.key==="d") keys.right=true;
  if(e.code==="Space") shoot();
});
document.addEventListener("keyup",e=>{
  if(e.key==="ArrowUp"||e.key==="w") keys.up=false;
  if(e.key==="ArrowDown"||e.key==="s") keys.down=false;
  if(e.key==="ArrowLeft"||e.key==="a") keys.left=false;
  if(e.key==="ArrowRight"||e.key==="d") keys.right=false;
});

["up","down","left","right"].forEach(dir=>{
  const b=document.getElementById(dir);
  b.addEventListener("pointerdown",e=>{e.preventDefault();keys[dir]=true;});
  ["pointerup","pointerleave","pointercancel"].forEach(ev=>
    b.addEventListener(ev,()=>keys[dir]=false)
  );
});
document.getElementById("shoot").addEventListener("click",shoot);

draw();
</script>
</body>
</html>

