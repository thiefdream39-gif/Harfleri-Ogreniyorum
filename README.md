# Harfleri-Ogreniyorum[Harfleri_Ogreniyorum.html.TXT](https://github.com/user-attachments/files/22687735/Harfleri_Ogreniyorum.html.TXT)
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>📚 Harfleri Öğreniyorum - Tam Sürüm</title>
<style>
body { font-family: Arial; text-align: center; background: #f0f8ff; margin:0; padding:0;}
h1 { color: #ff5722; margin-top: 20px;}
button { padding: 10px 20px; margin: 5px; font-size: 18px; cursor: pointer; border-radius: 10px; border:none; transition: 0.2s; }
button:hover { transform: scale(1.1); }
.balloon { border-radius: 50%; width: 60px; height: 60px; font-size: 24px; margin: 5px; display: inline-block; line-height: 60px; cursor:pointer; transition: 0.2s; color: #fff; font-weight: bold; }
.balloon:hover { transform: scale(1.3); }
.section { margin: 20px 0; padding:10px; background:#fff; border-radius:15px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);}
input { padding: 10px; font-size: 16px; margin:5px;}
#loginBox { margin-top: 50px; }
.hidden { display:none; }
canvas { border:2px dashed #3498db; border-radius:10px; touch-action: none; margin-top:10px;}
</style>
</head>
<body>

<h1>Harfleri Öğreniyorum</h1>

<div id="loginBox">
  <h2>Giriş Yap</h2>
  <input type="text" id="userName" placeholder="İsminizi girin">
  <br>
  <button onclick="login('öğrenci')">Öğrenci Girişi</button>
  <button onclick="login('öğretmen')">Öğretmen Girişi</button>
</div>

<div id="appContent" class="hidden">
  <p id="welcome"></p>

  <div class="section">
    <h2>Balon Oyunu - Seviye: <span id="level">1</span></h2>
    <div id="balloons"></div>
    <p>Puan: <span id="score">0</span></p>
  </div>

  <div class="section">
    <h2>Hecelerle Oyun</h2>
    <button onclick="playSound('ba')">ba</button>
    <button onclick="playSound('be')">be</button>
    <button onclick="playSound('bi')">bi</button>
  </div>

  <div class="section">
    <h2>Kısa Metinler</h2>
    <p>Ali ata bak.</p>
    <button onclick="playText('Ali ata bak.')">Oku</button>
  </div>

  <div class="section">
    <h2>Çizgi Çalışmaları</h2>
    <p>Parmağınla veya fareyle çiz!</p>
    <canvas id="drawCanvas" width="300" height="200"></canvas>
    <br>
    <button onclick="clearCanvas()">Temizle</button>
  </div>
</div>

<script>
// Sesli okuma
function playSound(letter){
  const utter = new SpeechSynthesisUtterance(letter);
  utter.lang = 'tr-TR';
  speechSynthesis.speak(utter);
}
function playText(text){
  const utter = new SpeechSynthesisUtterance(text);
  utter.lang = 'tr-TR';
  speechSynthesis.speak(utter);
}

// Pop sesi
const popSound = new Audio('https://www.soundjay.com/button/beep-07.mp3');

// Giriş
function login(type){
  const name = document.getElementById('userName').value;
  if(type === 'öğretmen' && name==='ogretmen123'){
    alert('Öğretmen girişi başarılı!');
    document.getElementById('loginBox').classList.add('hidden');
    document.getElementById('appContent').classList.remove('hidden');
    document.getElementById('welcome').innerText = 'Hoşgeldiniz Öğretmen!';
  } else if(type === 'öğrenci' && name!==''){
    alert('Öğrenci girişi başarılı!');
    document.getElementById('loginBox').classList.add('hidden');
    document.getElementById('appContent').classList.remove('hidden');
    document.getElementById('welcome').innerText = 'Hoşgeldiniz ' + name + '!';
  } else {
    alert('Giriş bilgisi yanlış veya isim boş!');
  }
}

// Balon Oyunu
let score = 0;
let level = 1;
const balloonSets = [
  ['a','b','c','d','e'],
  ['f','g','h','ı','i'],
  ['j','k','l','m','n']
];
const colors = ['#e74c3c','#3498db','#f1c40f','#2ecc71','#9b59b6'];

function randomLetter(set){ return set[Math.floor(Math.random()*set.length)]; }
function randomColor(){ return colors[Math.floor(Math.random()*colors.length)]; }

function createBalloons(){
  const container = document.getElementById('balloons');
  container.innerHTML = '';
  const currentSet = balloonSets[level-1];
  for(let i=0;i<5;i++){
    const btn = document.createElement('div');
    btn.className = 'balloon';
    btn.innerText = randomLetter(currentSet);
    btn.style.backgroundColor = randomColor();
    btn.onclick = () => {
      score++;
      document.getElementById('score').innerText = score;
      btn.innerText = randomLetter(currentSet);
      btn.style.backgroundColor = randomColor();
      popSound.play();
      playSound(btn.innerText);
      if(score % 10 === 0 && level < balloonSets.length){
        level++;
        document.getElementById('level').innerText = level;
        alert('Tebrikler! Seviye '+level+' başladı!');
      }
    };
    container.appendChild(btn);
  }
}

createBalloons();

// Çizgi çalışmaları
const canvas = document.getElementById('drawCanvas');
const ctx = canvas.getContext('2d');
let drawing = false;

canvas.addEventListener('mousedown', ()=>drawing=true);
canvas.addEventListener('mouseup', ()=>drawing=false);
canvas.addEventListener('mouseout', ()=>drawing=false);

canvas.addEventListener('mousemove', draw);

canvas.addEventListener('touchstart', (e)=>{drawing=true; drawTouch(e); e.preventDefault();});
canvas.addEventListener('touchend', ()=>drawing=false);
canvas.addEventListener('touchmove', (e)=>{drawTouch(e); e.preventDefault();});

function draw(e){
  if(!drawing) return;
  ctx.fillStyle = "#ff5722";
  ctx.beginPath();
  ctx.arc(e.offsetX, e.offsetY, 5, 0, Math.PI*2);
  ctx.fill();
}

function drawTouch(e){
  if(!drawing) return;
  const rect = canvas.getBoundingClientRect();
  const touch = e.touches[0];
  ctx.fillStyle = "#ff5722";
  ctx.beginPath();
  ctx.arc(touch.clientX - rect.left, touch.clientY - rect.top, 5, 0, Math.PI*2);
  ctx.fill();
}

function clearCanvas(){ ctx.clearRect(0,0,canvas.width,canvas.height); }

</script>

</body>
</html>
