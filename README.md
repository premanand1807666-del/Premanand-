<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sound Hub</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
:root {
    --bg-color: #121212;
    --header-bg: #000;
    --card-bg: #181818;
    --card-hover: #282828;
    --accent-color: #1db954;
    --accent-hover: #1ed760;
    --text-primary: #fff;
    --text-secondary: #b3b3b3;
    --border-radius: 12px;
}
* { box-sizing:border-box; margin:0; padding:0; }
body { font-family:'Segoe UI', sans-serif; background-color:var(--bg-color); color:var(--text-primary); min-height:100vh; display:flex; flex-direction:column; }
header { background-color:var(--header-bg); padding:20px; position:sticky; top:0; z-index:100; box-shadow:0 4px 12px rgba(0,0,0,0.5); }
.header-content { max-width:1200px; margin:0 auto; display:flex; flex-direction:column; gap:15px; }
h1 { font-size:1.8rem; display:flex; align-items:center; gap:10px; }
h1 i { color:var(--accent-color); }
.search-container { position:relative; width:100%; }
.search-container i { position:absolute; left:15px; top:50%; transform:translateY(-50%); color:var(--text-secondary); }
.search-bar { width:100%; padding:12px 20px 12px 45px; border-radius:50px; border:none; background-color:#242424; color:var(--text-primary); font-size:1rem; outline:none; transition:0.3s; }
.search-bar:focus { background-color:#2a2a2a; border:1px solid #333; }
.categories { display:flex; gap:10px; overflow-x:auto; padding-bottom:5px; scrollbar-width:none; }
.categories::-webkit-scrollbar { display:none; }
.category-btn { background-color:#242424; color:var(--text-primary); border:none; padding:8px 16px; border-radius:20px; cursor:pointer; white-space:nowrap; font-weight:600; transition:all 0.3s ease; }
.category-btn:hover { background-color:#333; }
.category-btn.active { background-color:var(--text-primary); color:var(--bg-color); }
main { max-width:1200px; margin:0 auto; padding:20px; flex:1; width:100%; }
.sound-grid { display:grid; grid-template-columns:repeat(auto-fill, minmax(160px, 1fr)); gap:20px; }
@media(max-width:480px){.sound-grid{grid-template-columns:repeat(2,1fr); gap:12px;}}
.card { background-color:var(--card-bg); border-radius:var(--border-radius); padding:20px; display:flex; flex-direction:column; align-items:center; text-align:center; transition:0.3s; box-shadow:0 4px 6px rgba(0,0,0,0.1); position:relative; }
.card:hover { background-color:var(--card-hover); transform:translateY(-4px); }
.card-icon { width:60px; height:60px; background-color:#333; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:1.5rem; margin-bottom:15px; color:var(--text-secondary); }
.card-title { font-size:1rem; font-weight:600; margin-bottom:5px; display:-webkit-box; -webkit-line-clamp:2; -webkit-box-orient:vertical; overflow:hidden; }
.card-category { font-size:0.8rem; color:var(--text-secondary); margin-bottom:20px; }
.controls { display:flex; gap:15px; width:100%; justify-content:center; margin-top:auto; }
.btn { border:none; border-radius:50%; width:45px; height:45px; display:flex; align-items:center; justify-content:center; cursor:pointer; transition:0.2s ease; text-decoration:none; }
.btn-play { background-color:var(--accent-color); color:#000; font-size:1.2rem; }
.btn-play:hover { background-color:var(--accent-hover); transform:scale(1.05); }
.btn-download { background-color:transparent; color:var(--text-secondary); font-size:1.2rem; border:1px solid var(--text-secondary); }
.btn-download:hover { color:var(--text-primary); border-color:var(--text-primary); }
.no-results { grid-column:1 / -1; text-align:center; padding:40px; color:var(--text-secondary); }
</style>
</head>
<body>
<header>
<div class="header-content">
<h1><i class="fas fa-headphones-alt"></i> Sound Hub</h1>
<div class="search-container">
<i class="fas fa-search"></i>
<input type="text" id="searchInput" class="search-bar" placeholder="Search sounds (e.g., Aayein, Bruh)...">
</div>
<div class="categories" id="categoryContainer"></div>
</div>
</header>
<main>
<div class="sound-grid" id="soundGrid"></div>
</main>
<script>
const soundsData=[
{id:1,title:"Aayein",category:"Meme Sounds",icon:"fa-face-surprise",url:"https://www.myinstants.com/media/sounds/aayein.mp3"},
{id:2,title:"Vine Boom",category:"Effects",icon:"fa-bomb",url:"https://www.myinstants.com/media/sounds/vine-boom.mp3"},
{id:3,title:"Baigan",category:"Meme Sounds",icon:"fa-leaf",url:"https://www.myinstants.com/media/sounds/baigan.mp3"},
{id:4,title:"Bruh",category:"Reaction Sounds",icon:"fa-face-rolling-eyes",url:"https://www.myinstants.com/media/sounds/movie_1.mp3"},
{id:5,title:"Emotional Damage",category:"Reaction Sounds",icon:"fa-heart-crack",url:"https://www.myinstants.com/media/sounds/emotional-damage.mp3"},
{id:6,title:"Fart with Reverb",category:"Effects",icon:"fa-wind",url:"https://www.myinstants.com/media/sounds/fart-with-reverb.mp3"},
{id:7,title:"Wow Anime",category:"Reaction Sounds",icon:"fa-star",url:"https://www.myinstants.com/media/sounds/anime-wow.mp3"},
{id:8,title:"Sad Meow Song",category:"Meme Sounds",icon:"fa-cat",url:"https://www.myinstants.com/media/sounds/sad-meow.mp3"},
{id:9,title:"Ruko Jaraa",category:"Meme Sounds",icon:"fa-hand-paper",url:"https://www.myinstants.com/media/sounds/ruko-zara-sabar-karo.mp3"},
{id:10,title:"Illuminati Confirmed",category:"Effects",icon:"fa-eye",url:"https://www.myinstants.com/media/sounds/illuminati.mp3"},
{id:11,title:"Helicopter Helicopter",category:"Meme Sounds",icon:"fa-helicopter",url:"https://www.myinstants.com/media/sounds/helicopter.mp3"},
{id:12,title:"Buzzer",category:"Effects",icon:"fa-exclamation-circle",url:"https://www.myinstants.com/media/sounds/wrong-answer-sound-effect.mp3"}
];
let currentAudio=null;
let currentPlayingBtn=null;
const grid=document.getElementById('soundGrid');
const searchInput=document.getElementById('searchInput');
const categoryContainer=document.getElementById('categoryContainer');
const categories=["All",...new Set(soundsData.map(sound=>sound.category))];
let activeCategory="All";
function init(){renderCategories();renderSounds(soundsData);searchInput.addEventListener('input',e=>{filterSounds(e.target.value,activeCategory);});}
function renderCategories(){categoryContainer.innerHTML='';categories.forEach(cat=>{const btn=document.createElement('button');btn.className=`category-btn ${cat===activeCategory?'active':''}`;btn.textContent=cat;btn.addEventListener('click',()=>{document.querySelectorAll('.category-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');activeCategory=cat;filterSounds(searchInput.value,activeCategory);});categoryContainer.appendChild(btn);});}
function filterSounds(searchTerm,category){const term=searchTerm.toLowerCase();const filtered=soundsData.filter(sound=>{const matchesSearch=sound.title.toLowerCase().includes(term);const matchesCategory=category==="All"||sound.category===category;return matchesSearch&&matchesCategory;});renderSounds(filtered);}
function renderSounds(sounds){grid.innerHTML='';if(sounds.length===0){grid.innerHTML=`<div class="no-results"><h2>No sounds found</h2><p>Try searching for something else.</p></div>`;return;}sounds.forEach(sound=>{const card=document.createElement('div');card.className='card';card.innerHTML=`<div class="card-icon"><i class="fas ${sound.icon}"></i></div><div class="card-title" title="${sound.title}">${sound.title}</div><div class="card-category">${sound.category}</div><div class="controls"><button class="btn btn-play" onclick="playSound('${sound.url}',this)"><i class="fas fa-play"></i></button><a href="${sound.url}" download="${sound.title}.mp3" target="_blank" class="btn btn-download" title="Download"><i class="fas fa-download"></i></a></div>`;grid.appendChild(card);});}
window.playSound=function(url,btnElement){if(currentAudio&&currentAudio.src===url&&!currentAudio.paused){currentAudio.pause();resetIcon(currentPlayingBtn);return;}if(currentAudio){currentAudio.pause();resetIcon(currentPlayingBtn);}currentAudio=new Audio(url);currentPlayingBtn=btnElement;const icon=btnElement.querySelector('i');icon.classList.remove('fa-play');icon.classList.add('fa-pause');currentAudio.play().catch(error=>{console.log("Audio playback failed:",error);alert("Audio file not found or blocked.");resetIcon(btnElement);});currentAudio.onended=()=>{resetIcon(btnElement);};};
function resetIcon(btnElement){if(btnElement){const icon=btnElement.querySelector('i');icon.classList.remove('fa-pause');icon.classList.add('fa-play');}}
init();
</script>
</body>
</html>
