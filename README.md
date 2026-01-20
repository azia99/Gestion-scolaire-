<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>SGS - Système de Gestion Scolaire</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
body{margin:0;font-family:Segoe UI;background:#ecf0f1}
.hidden{display:none}
.header{background:#2c3e50;color:#fff;padding:15px;display:flex;justify-content:space-between}
.sidebar{width:230px;background:#34495e;color:#fff;height:100vh;position:fixed}
.sidebar button{width:100%;padding:14px;border:none;background:none;color:#fff;text-align:left;cursor:pointer}
.sidebar button:hover,.sidebar .active{background:#27ae60}
.main{margin-left:230px;padding:20px}
.card{background:#fff;padding:20px;border-radius:10px;margin-bottom:15px}
.stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:15px}
.stat{padding:20px;border-radius:10px;color:#fff;font-size:22px}
.green{background:#27ae60}.blue{background:#3498db}.orange{background:#f39c12}.red{background:#e74c3c}
table{width:100%;border-collapse:collapse}
th,td{padding:10px;border-bottom:1px solid #ddd}
th{background:#34495e;color:#fff}
button{cursor:pointer}
.modal{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,.6);display:none;align-items:center;justify-content:center}
.modal-content{background:#fff;padding:20px;width:90%;max-width:400px;border-radius:10px}
input,select{width:100%;padding:8px;margin-bottom:10px}
.login{display:flex;justify-content:center;align-items:center;height:100vh}
.login-box{background:#fff;padding:30px;border-radius:10px;width:300px}
</style>
</head>
<body>

<!-- LOGIN -->
<div class="login" id="login">
  <div class="login-box">
    <h2>🏫 SGS</h2>
    <input id="user" placeholder="Identifiant">
    <input id="pass" type="password" placeholder="Mot de passe">
    <button onclick="login()">Se connecter</button>
    <p id="error" style="color:red"></p>
  </div>
</div>

<!-- APP -->
<div id="app" class="hidden">
  <div class="header">
    <b>SGS | Année 2025-2026</b>
    <button onclick="logout()">Déconnexion</button>
  </div>

  <div class="sidebar">
    <button class="active" onclick="page('dashboard',this)">📊 Tableau de bord</button>
    <button onclick="page('classes',this)">🎓 Classes</button>
    <button onclick="page('eleves',this)">👨‍🎓 Élèves</button>
    <button onclick="page('notes',this)">📝 Notes</button>
    <button onclick="page('bulletins',this)">📋 Bulletins</button>
  </div>

  <div class="main" id="content"></div>
</div>

<!-- MODAL -->
<div class="modal" id="modal">
  <div class="modal-content" id="modalContent"></div>
</div>

<script>
const DATA={
user:"admin",pass:"admin",
classes:["6ème A","5ème A"],
eleves:[
{mat:"E001",nom:"KOUASSI",prenom:"Jean",classe:"6ème A"}
],
notes:[
{eleve:"E001",matiere:"Maths",note:15,coef:4}
]
};

function login(){
 if(user.value==DATA.user&&pass.value==DATA.pass){
  loginDiv.style.display="none";app.classList.remove("hidden");dashboard();
 } else error.innerText="Accès refusé";
}
function logout(){location.reload()}

function page(p,btn){
 document.querySelectorAll(".sidebar button").forEach(b=>b.classList.remove("active"));
 btn.classList.add("active");
 window[p]();
}

function dashboard(){
 content.innerHTML=`
 <h2>TABLEAU DE BORD</h2>
 <div class="stats">
  <div class="stat green">${DATA.classes.length}<br>Classes</div>
  <div class="stat blue">${DATA.eleves.length}<br>Élèves</div>
  <div class="stat orange">${DATA.notes.length}<br>Notes</div>
 </div>`;
}

function classes(){
 content.innerHTML=`
 <h2>CLASSES</h2>
 <button onclick="addClasse()">➕ Classe</button>
 <ul>${DATA.classes.map(c=>"<li>"+c+"</li>").join("")}</ul>`;
}

function eleves(){
 content.innerHTML=`
 <h2>ÉLÈVES</h2>
 <button onclick="addEleve()">➕ Élève</button>
 <table>
 <tr><th>Mat</th><th>Nom</th><th>Classe</th></tr>
 ${DATA.eleves.map(e=>`<tr><td>${e.mat}</td><td>${e.nom} ${e.prenom}</td><td>${e.classe}</td></tr>`).join("")}
 </table>`;
}

function notes(){
 content.innerHTML=`
 <h2>NOTES</h2>
 <table>
 <tr><th>Élève</th><th>Matière</th><th>Note</th></tr>
 ${DATA.notes.map(n=>`<tr><td>${n.eleve}</td><td>${n.matiere}</td><td>${n.note}</td></tr>`).join("")}
 </table>`;
}

function bulletins(){
 content.innerHTML=`
 <h2>BULLETIN (Preview)</h2>
 <div class="card">
 Élève : ${DATA.eleves[0].nom} ${DATA.eleves[0].prenom}<br>
 Moyenne : ${(DATA.notes[0].note*DATA.notes[0].coef)/DATA.notes[0].coef}/20
 </div>`;
}

function addClasse(){
 showModal(`<h3>Nouvelle classe</h3>
 <input id="c"><button onclick="DATA.classes.push(c.value);close()">Enregistrer</button>`);
}
function addEleve(){
 showModal(`<h3>Nouvel élève</h3>
 <input id="m" placeholder="Matricule">
 <input id="n" placeholder="Nom">
 <input id="p" placeholder="Prénom">
 <select id="cl">${DATA.classes.map(c=>`<option>${c}</option>`)}</select>
 <button onclick="DATA.eleves.push({mat:m.value,nom:n.value,prenom:p.value,classe:cl.value});close()">Enregistrer</button>`);
}
function showModal(h){modal.style.display="flex";modalContent.innerHTML=h}
function close(){modal.style.display="none";eleves()}
</script>
</body>
</html>
