# Goodbye2025-Hello2026
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title> Goodbye 2025, Hello 2026</title>

<!-- EmailJS -->
<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>

<style>
/* (Your entire CSS remains unchanged) */
body {
  margin: 0;
  font-family: 'Georgia', serif;
  background: linear-gradient(180deg, #f6efe9, #fdfaf7);
  color: #3b2f2f;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  overflow: hidden;
}

.page { display: none; max-width: 720px; padding: 2.5rem; text-align: center; animation: fadeIn 0.9s ease; }
.page.active { display: block; }
h1 { font-size: 2.2rem; margin-bottom: 1rem; }
p { font-size: 1.1rem; line-height: 1.6; margin-bottom: 1.8rem; }
textarea { width: 100%; height: 120px; padding: 1rem; border-radius: 14px; border: 1px solid #d8cfc8; font-family: inherit; font-size: 1rem; background: #fffaf6; resize: none; }
button { margin-top: 1.6rem; padding: 0.8rem 2.2rem; font-size: 1rem; border: none; border-radius: 30px; background: #c7a17a; color: white; cursor: pointer; transition: all 0.3s ease; }
button:hover { background: #b08968; transform: translateY(-2px); }
.emoji { font-size: 2.2rem; margin-bottom: 0.8rem; }

.letter { background: #fffaf6; padding: 1.6rem; border-radius: 14px; border: 1px solid #e3d9d2; margin-bottom: 1.8rem; position: relative; cursor: pointer; transition: all 2s ease; }
.flames { position: absolute; bottom: -10px; left: 50%; transform: translateX(-50%); font-size: 2.8rem; opacity: 0; animation: erupt 0.5s infinite alternate; }
.letter.ignite { background: radial-gradient(circle at bottom, #ffb703, #fb8500, #9d0208); color: #fff; box-shadow: 0 0 40px rgba(255, 87, 34, 0.6); transform: scale(1.05) rotate(-2deg); }
.letter.ignite .flames { opacity: 1; }
.letter.ash { opacity: 0; filter: blur(8px); transform: scale(0.5) translateY(40px); transition: all 2s ease; }

.seed { font-size: 3rem; cursor: pointer; }
.sprout { font-size: 3rem; animation: grow 1.2s ease forwards; }
.plant { font-size: 3.2rem; animation: grow 1.2s ease forwards; }
.garden { display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; margin-top: 20px; }
.garden span { font-size: 2rem; opacity: 0; animation: bloom 1s forwards; }

.affirmation { background: #fffaf6; padding: 1.4rem; border-radius: 14px; margin-bottom: 1rem; animation: float 3s ease-in-out infinite; }

@keyframes fadeIn { from { opacity: 0; transform: translateY(12px); } to { opacity: 1; transform: translateY(0); } }
@keyframes erupt { from { transform: translateX(-50%) scale(1); } to { transform: translateX(-50%) scale(1.15); } }
@keyframes grow { from { transform: scale(0.6); opacity: 0; } to { transform: scale(1); opacity: 1; } }
@keyframes float { 0% { transform: translateY(0); } 50% { transform: translateY(-6px); } 100% { transform: translateY(0); } }
@keyframes bloom { from { opacity: 0; transform: scale(0.5); } to { opacity: 1; transform: scale(1); } }
</style>
</head>

<body>

<!-- Intro -->
<div class="page active" id="intro">
  <div class="emoji">🌙</div>
  <h1 id="greeting">Goodbye 2025, Goodbye 2026</h1>
  <p>A gentle journey to reflect, release, and begin again.</p>
  <button onclick="goTo('bad')">Start Journey</button>

  <p style="font-size:0.85rem; opacity:0.6;">
    This space holds your words with care. Your responses are not collected or stored anywhere. Your privacy is respected so feel free to write whatever you feel.
  </p>
</div>

<!-- Bad Memories -->
<div class="page" id="bad">
  <div class="emoji">📝</div>
  <h1>What memories you want to hold onto?</h1>
  <textarea id="badText" placeholder="Your memory here..."></textarea>
  <button onclick="goTo('burn')">Continue</button>
</div>

<!-- Burning Page -->
<div class="page" id="burn">
  <div class="emoji">🔥</div>
  <h1>Burning the Past</h1>
  <textarea id="burnText" placeholder="Write what you want to let go of…"></textarea>
  <div class="letter" id="letter">
    <span id="letterText">Your words will appear here.</span>
    <div class="flames">🔥🔥🔥</div>
  </div>
  <button id="burnButton">Burn 🔥</button>
</div>

<!-- Wishes -->
<div class="page" id="wish">
  <div class="emoji">✨</div>
  <h1>Your Wish for 2026</h1>
  <textarea></textarea>
  <button onclick="goTo('seed')">Plant This Wish</button>
</div>

<!-- Garden -->
<div class="page" id="seed">
  <h1>Planting Your Intention</h1>
  <p>Every intention begins small.</p>
  <div id="plant" class="seed">🌱</div>
  <div id="garden" class="garden"></div>
  <button onclick="growPlant()">Nurture</button>
</div>

<!-- Hello -->
<div class="page" id="hello">
  <div class="emoji">🌸</div>
  <h1>Hello, 2026</h1>
  <button onclick="goTo('affirm')">Continue</button>
</div>

<!-- Affirmations -->
<div class="page" id="affirm">
  <h1>Gentle Affirmations</h1>
  <div class="affirmation"> 2026 is my year!.</div>
  <div class="affirmation">I do not need to rush my growth.</div>
  <div class="affirmation">I trust myself to handle what comes next.</div>
</div>

<script>
/* INIT EMAILJS */
emailjs.init("w7ex9jjwOEEOU7MId");

/* USER GREETING */
let username = localStorage.getItem("username");
if (!username) {
  username = prompt("Hey 💛 what should I call you?");
  if (username) localStorage.setItem("username", username);
}
if (username) {
  document.getElementById("greeting").innerText = `Welcome, ${username} ✨`;
}

/* NAVIGATION */
function goTo(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

/* BURN + EMAIL */
const letter = document.getElementById('letter');
const burnButton = document.getElementById('burnButton');
const burnText = document.getElementById('burnText');
const letterText = document.getElementById('letterText');

function ignite(){
  const input = burnText.value.trim();
  if (!input) return alert("Write something first.");

  letterText.textContent = input;
  letter.classList.add('ignite');

  emailjs.send("service_cbkrxci","template_3qpuuvo",{
    from_name: username || "Anonymous",
    message: input,
    time: new Date().toLocaleString()
  });

  setTimeout(()=>letter.classList.add('ash'),1200);
  setTimeout(()=>goTo('wish'),3000);
}

letter.addEventListener('click',ignite);
burnButton.addEventListener('click',ignite);

/* GARDEN */
const plant = document.getElementById('plant');
const garden = document.getElementById('garden');

function growPlant(){
  if(plant.classList.contains('seed')){
    plant.textContent='🌿';
    plant.className='sprout';
  } else {
    plant.textContent='🌳';
    plant.className='plant';
    const flowers=['🌸','🌷','🌼','🌻','🌺','🌹'];
    for(let i=0;i<20;i++){
      const f=document.createElement('span');
      f.textContent=flowers[Math.floor(Math.random()*flowers.length)];
      f.style.animationDelay=Math.random()+'s';
      garden.appendChild(f);
    }
    setTimeout(()=>goTo('hello'),2500);
  }
}
</script>

</body>
</html>
