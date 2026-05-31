# Matthew-s-games
Fun games
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Adventure Quest</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}

body{
    background:#111827;
    color:white;
    font-family:Arial,sans-serif;
    text-align:center;
}

header{
    background:#1f2937;
    padding:15px;
}

#ui{
    display:flex;
    justify-content:center;
    gap:20px;
    padding:10px;
    background:#374151;
}

.stat{
    background:#4b5563;
    padding:10px 20px;
    border-radius:8px;
}

canvas{
    background:#0f172a;
    border:3px solid #38bdf8;
    margin-top:20px;
}

button{
    padding:10px 20px;
    margin:10px;
    border:none;
    border-radius:8px;
    cursor:pointer;
}
</style>
</head>

<body>

<header>
    <h1>Adventure Quest RPG</h1>
</header>

<div id="ui">
    <div class="stat">❤️ HP: <span id="hp">100</span></div>
    <div class="stat">⭐ XP: <span id="xp">0</span></div>
    <div class="stat">💰 Coins: <span id="coins">0</span></div>
    <div class="stat">🏆 Level: <span id="level">1</span></div>
</div>

<button onclick="saveGame()">Save</button>
<button onclick="loadGame()">Load</button>

<canvas id="gameCanvas" width="900" height="500"></canvas>

<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

const player = {
    x:100,
    y:100,
    size:30,
    speed:5,
    hp:100,
    xp:0,
    coins:0,
    level:1
};

const keys = {};

const enemies = [];

function spawnEnemy(){
    enemies.push({
        x:Math.random()*850,
        y:Math.random()*450,
        size:25
    });
}

for(let i=0;i<5;i++){
    spawnEnemy();
}

document.addEventListener("keydown",e=>{
    keys[e.key]=true;
});

document.addEventListener("keyup",e=>{
    keys[e.key]=false;
});

function update(){

    if(keys["ArrowUp"]) player.y -= player.speed;
    if(keys["ArrowDown"]) player.y += player.speed;
    if(keys["ArrowLeft"]) player.x -= player.speed;
    if(keys["ArrowRight"]) player.x += player.speed;

    enemies.forEach((enemy,index)=>{

        let dx = player.x - enemy.x;
        let dy = player.y - enemy.y;

        if(Math.abs(dx)<25 && Math.abs(dy)<25){

            player.coins += 10;
            player.xp += 20;

            if(player.xp >= player.level*100){
                player.level++;
            }

            enemies.splice(index,1);
            spawnEnemy();
        }
    });

    document.getElementById("xp").textContent = player.xp;
    document.getElementById("coins").textContent = player.coins;
    document.getElementById("level").textContent = player.level;
}

function draw(){

    ctx.clearRect(0,0,canvas.width,canvas.height);

    ctx.fillStyle="#38bdf8";
    ctx.fillRect(player.x,player.y,player.size,player.size);

    ctx.fillStyle="#ef4444";

    enemies.forEach(enemy=>{
        ctx.fillRect(enemy.x,enemy.y,enemy.size,enemy.size);
    });
}

function gameLoop(){
    update();
    draw();
    requestAnimationFrame(gameLoop);
}

function saveGame(){
    localStorage.setItem("adventureSave",
        JSON.stringify(player));
    alert("Game Saved");
}

function loadGame(){
    const data =
        JSON.parse(localStorage.getItem("adventureSave"));

    if(data){
        Object.assign(player,data);
        alert("Game Loaded");
    }
}

gameLoop();
</script>

</body>
</html>
