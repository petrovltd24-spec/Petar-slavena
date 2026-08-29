
<title>Петър + Славена ❤️</title>

<style>
*{box-sizing:border-box}

body{
margin:0;
min-height:100vh;
font-family:Arial,sans-serif;
background:linear-gradient(135deg,#160914,#681536,#210817);
color:white;
display:flex;
justify-content:center;
align-items:center;
padding:18px;
}

.container{
width:100%;
max-width:650px;
background:rgba(255,255,255,.10);
border:1px solid rgba(255,255,255,.2);
backdrop-filter:blur(15px);
border-radius:30px;
padding:30px 22px;
text-align:center;
box-shadow:0 20px 70px rgba(0,0,0,.55);
}

.heart{
font-size:55px;
animation:pulse 1.5s infinite;
}

@keyframes pulse{
50%{transform:scale(1.15)}
}

h1{font-size:38px}
h2{font-size:28px}

p{
font-size:18px;
line-height:1.6;
}

button,.upload{
display:inline-block;
border:0;
border-radius:50px;
padding:15px 25px;
margin-top:15px;
background:linear-gradient(90deg,#ff477e,#ff1761);
color:white;
font-size:17px;
font-weight:bold;
cursor:pointer;
}

.answers{
display:flex;
flex-direction:column;
gap:12px;
margin-top:20px;
}

.answers button{
width:100%;
background:rgba(255,255,255,.13);
border:1px solid rgba(255,255,255,.2);
}

input{display:none}

.puzzle{
width:min(420px,90vw);
aspect-ratio:1;
margin:25px auto;
display:grid;
grid-template-columns:repeat(3,1fr);
gap:4px;
}

.tile{
border-radius:8px;
background-repeat:no-repeat;
border:2px solid rgba(255,255,255,.3);
cursor:pointer;
transition:.2s;
}

.tile.selected{
border:4px solid white;
transform:scale(.94);
}

.birthday{
text-align:left;
font-size:18px;
line-height:1.7;
max-height:65vh;
overflow-y:auto;
padding:5px;
}

.birthday h2{
text-align:center;
}

.small{
opacity:.75;
font-size:14px;
}

</style>
</head>

<body>

<div class="container" id="app"></div>

<script>

const app=document.getElementById("app");

let question=0;
let photo=null;
let puzzle=[];
let selected=null;


/* НАЧАЛО */

function home(){

app.innerHTML=`

<div class="heart">❤️</div>

<h1>ПЕТЪР + СЛАВЕНА ❤️</h1>

<p>
Едно малко нещо от мен за теб 🥹❤️
</p>

<button onclick="ready()">
Готова ли си? ❤️
</button>

`;

}


/* ГОТОВА ЛИ Е */

function ready(){

app.innerHTML=`

<div class="heart">🥹❤️</div>

<h2>Готова ли си?</h2>

<p>
Имам нещо специално за теб...
</p>

<button onclick="start()">
ЗАПОЧВАМЕ ❤️
</button>

}


/* ВЪПРОСИ */

const questions=[

"Как се запознахме? ❤️",

"Къде беше първата ни среща? ❤️",

"Какво обичам? ❤️",

"Без какво не мога? ❤️"

];


function start(){

question=0;

showQuestion();

}


function showQuestion(){

app.innerHTML=`

<div class="small">
${question+1} / ${questions.length}
</div>

<h2>${questions[question]}</h2>

<p>
Избери своя отговор ❤️
</p>

<div class="answers">

<button onclick="next()">
Отговорът, който знаеш ❤️
</button>

<button onclick="next()">
Може би този 😍
</button>

<button onclick="next()">
Този със сигурност 🥹
</button>

</div>

`;

}


function next(){

question++;

if(question<questions.length){

showQuestion();

}else{

upload();

}

}


/* КАЧВАНЕ НА СНИМКА */

function upload(){

app.innerHTML=`

<div class="heart">📸</div>

<h2>Сега е време за вашата снимка ❤️</h2>

<p>
Избери една ваша любима снимка.
<br>
След това тя автоматично ще се превърне в пъзел. 🧩
</p>

<label class="upload">

КАЧИ СНИМКА ❤️

<input
type="file"
id="photo"
accept="image/*"
>

</label>

`;

document
.getElementById("photo")
.addEventListener("change",getPhoto);

}


function getPhoto(e){

const file=e.target.files[0];

if(!file)return;

const reader=new FileReader();

reader.onload=function(event){

photo=event.target.result;

createPuzzle();

};

reader.readAsDataURL(file);

}


/* ПЪЗЕЛ */

function createPuzzle(){

puzzle=[0,1,2,3,4,5,6,7,8];

/* Разбъркваме */

do{

puzzle.sort(()=>Math.random()-.5);

}while(solved());

selected=null;

app.innerHTML=`

<h2>Нашата снимка 🧩❤️</h2>

<p>
Натисни две части една след друга,
за да ги размениш.
</p>

<div class="puzzle" id="puzzle"></div>

`;

draw();

}


function draw(){

const box=document.getElementById("puzzle");

box.innerHTML="";

puzzle.forEach((piece,pos)=>{

const tile=document.createElement("div");

tile.className="tile";

if(selected===pos)
tile.classList.add("selected");

const x=piece%3;
const y=Math.floor(piece/3);

tile.style.backgroundImage=
`url("${photo}")`;

tile.style.backgroundSize="300% 300%";

tile.style.backgroundPosition=
`${x*50}% ${y*50}%`;

tile.onclick=()=>select(pos);

box.appendChild(tile);

});

}


function select(pos){

if(selected===null){

selected=pos;

draw();

return;

}

if(selected===pos){

selected=null;

draw();

return;

}

[puzzle[selected],puzzle[pos]]
=
[puzzle[pos],puzzle[selected]];

selected=null;

draw();

if(solved()){

setTimeout(finalQuestion,700);

}

}


function solved(){

return puzzle.every(
(piece,index)=>piece===index
);

}


/* ФИНАЛЕН ВЪПРОС */

function finalQuestion(){

app.innerHTML=`

<div class="heart">🥹❤️</div>

<h2>
Какво обичам да правя с теб? ❤️
</h2>

<div class="answers">

<button onclick="unlock()">
ВСИЧКО ИЗБРОЕНО ❤️
</button>

<button onclick="wrong()">
Не знам 😄
</button>

<button onclick="wrong()">
Нищо 😂
</button>

</div>

`;

}


function wrong(){

alert("Помисли още малко, любов ❤️🥹");

}


/* ОТКЛЮЧВАНЕ */

function unlock(){

app.innerHTML=`

<div class="heart">❤️</div>

<h2>
Искаш ли да видиш… ❤️
</h2>

<p>
Последното нещо е специално само за теб.
</p>

<button onclick="birthday()">
ДА 🥹❤️
</button>

`;

}


/* ПОЖЕЛАНИЕ */

function birthday(){

app.innerHTML=`

<div class="heart">🎂❤️</div>

<div class="birthday">

<h2>
❤️ Честит рожден ден любов моя ❤️
</h2>

<p>
Пожелавам ти от цялото си сърце да бъдеш здрава щастлива и винаги да имаш усмивка на лицето 🥹❤️ Пожелавам ти всеки твой ден да бъде изпълнен с любов топлина и красиви моменти
</p>

<p>
Искам да ти благодаря за нещо което може би никога няма да мога да ти опиша напълно с думи ❤️ Ти ме научи отново да вярвам в любовта Показа ми че мога да бъда обичан истински и че любовта може да бъде красива искрена и истинска ❤️
</p>

<p>
През тези 8 месеца ти винаги си била до мен Помагала си ми за всичко подкрепяла си ме и си била човекът на когото мога да разчитам 🫶🏻 Никога няма да забравя това и винаги ще го ценя
</p>

<p>
Благодаря ти че те има в живота ми ❤️ Благодаря ти за всяка усмивка всяка прегръдка всеки разговор и всеки момент който сме споделили Ти направи дните ми по хубави и живота ми по смислен ❤️
</p>

<p>
Искам да продължим да създаваме още безброй спомени заедно 🥹 Да се смеем да мечтаем да се подкрепяме и да преминаваме през всичко заедно ❤️
</p>

<p>
Бъди винаги себе си защото точно такава те обичам ❤️
</p>

<p>
Честит рожден ден мое момиче 🎂❤️ Благодаря ти за тези прекрасни 8 месеца и за това че ми показа какво означава да обичаш и да бъдеш обичан
</p>

<p>
Обичам те безкрайно много ❤️🥹😘
</p>

</div>

`;

}


home();

</script>

</body>
</html>
