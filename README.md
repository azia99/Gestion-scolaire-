<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gestion Scolaire</title>
<style>
body {font-family: Arial, sans-serif; margin:0; padding:0; background-color:#f4f4f4;}
header {background:#2c3e50; color:#fff; padding:10px 20px; text-align:center;}
#sidebar {width:250px; background:#34495e; height:100vh; position:fixed; top:0; left:0; padding-top:60px; color:#fff;}
#sidebar button {width:100%; padding:10px; border:none; background:none; color:#fff; text-align:left; cursor:pointer;}
#sidebar button:hover {background:#1abc9c;}
#content {margin-left:250px; padding:20px;}
table {width:100%; border-collapse:collapse; margin-top:10px;}
table, th, td {border:1px solid #ddd;}
th, td {padding:8px; text-align:center;}
th {background:#2c3e50; color:#fff;}
input, select {padding:5px; margin:5px;}
button.add-btn {background:#1abc9c; color:#fff; border:none; padding:5px 10px; cursor:pointer;}
button.add-btn:hover {background:#16a085;}
</style>
</head>
<body>
<header>
<h1>Logiciel de Gestion Scolaire</h1>
</header>
<div id="sidebar">
<button onclick="showSection('dashboard')">Tableau de bord</button>
<button onclick="showSection('parametres')">Paramètres</button>
<button onclick="showSection('classes')">Classes</button>
<button onclick="showSection('eleves')">Élèves</button>
<button onclick="showSection('enseignants')">Enseignants</button>
<button onclick="showSection('matieres')">Matières</button>
<button onclick="showSection('notes')">Notes</button>
<button onclick="showSection('bulletins')">Bulletins</button>
</div>
<div id="content">
<div id="dashboard-section" class="section">
<h2>Tableau de bord</h2>
<p>Bienvenue dans le logiciel de gestion scolaire !</p>
</div>

<div id="parametres-section" class="section" style="display:none">
<h2>Paramètres de l'établissement</h2>
<form id="paramForm">
<label>Nom de l'établissement:</label><input type="text" id="nomEtablissement"><br>
<label>Année scolaire:</label><input type="text" id="anneeScolaire"><br>
<label>Directeur Général:</label><input type="text" id="directeur"><br>
<label>Censeur / Proviseur:</label><input type="text" id="censeur"><br>
<label>Intendant / Économe:</label><input type="text" id="intendant"><br>
<label>Devise / Slogan:</label><input type="text" id="devise"><br>
<button type="button" onclick="saveParametres()" class="add-btn">Enregistrer</button>
</form>
</div>

<div id="classes-section" class="section" style="display:none">
<h2>Gestion des classes</h2>
<input type="text" id="newClassName" placeholder="Nom de la classe">
<button onclick="addClass()" class="add-btn">Ajouter Classe</button>
<table id="classesTable">
<tr><th>Nom de la classe</th><th>Actions</th></tr>
</table>
</div>

<div id="eleves-section" class="section" style="display:none">
<h2>Gestion des élèves</h2>
<input type="text" id="eleveNom" placeholder="Nom et prénom">
<select id="eleveSexe">
<option value="Masculin">Masculin</option>
<option value="Féminin">Féminin</option>
</select>
<input type="date" id="eleveDate">
<select id="eleveClasse"></select>
<input type="text" id="eleveParent" placeholder="Nom du parent">
<input type="text" id="eleveContact" placeholder="Contact parent">
<button onclick="addEleve()" class="add-btn">Ajouter Élève</button>
<table id="elevesTable">
<tr><th>Nom</th><th>Sexe</th><th>Date de naissance</th><th>Classe</th><th>Parent</th><th>Contact</th></tr>
</table>
</div>

<div id="enseignants-section" class="section" style="display:none">
<h2>Gestion des enseignants</h2>
<input type="text" id="ensNom" placeholder="Nom et prénom">
<select id="ensSexe">
<option value="Masculin">Masculin</option>
<option value="Féminin">Féminin</option>
</select>
<input type="text" id="ensMatiere" placeholder="Matière">
<input type="text" id="ensClasse" placeholder="Classes affectées">
<select id="ensStatut">
<option value="Permanent">Permanent</option>
<option value="Vacataire">Vacataire</option>
</select>
<button onclick="addEnseignant()" class="add-btn">Ajouter Enseignant</button>
<table id="enseignantsTable">
<tr><th>Nom</th><th>Sexe</th><th>Matière</th><th>Classe</th><th>Statut</th></tr>
</table>
</div>

<div id="matieres-section" class="section" style="display:none">
<h2>Gestion des matières</h2>
<input type="text" id="matiereNom" placeholder="Nom de la matière">
<input type="number" id="matiereCoef" placeholder="Coefficient">
<select id="matiereClasse"></select>
<input type="text" id="matiereEns" placeholder="Enseignant">
<button onclick="addMatiere()" class="add-btn">Ajouter Matière</button>
<table id="matieresTable">
<tr><th>Nom</th><th>Coefficient</th><th>Classe</th><th>Enseignant</th></tr>
</table>
</div>

<div id="notes-section" class="section" style="display:none">
<h2>Gestion des notes</h2>
<select id="selectClasseNotes"></select>
<button onclick="loadElevesNotes()" class="add-btn">Charger Élèves</button>
<div id="notesContainer"></div>
</div>

<div id="bulletins-section" class="section" style="display:none">
<h2>Bulletins scolaires</h2>
<select id="selectClasseBulletins"></select>
<button onclick="loadElevesBulletins()" class="add-btn">Afficher Bulletins</button>
<div id="bulletinsContainer"></div>
</div>

<script>
let classes = [];
let eleves = [];
let enseignants = [];
let matieres = [];
let notes = {};

function showSection(section) {
  document.querySelectorAll('.section').forEach(s => s.style.display='none');
  document.getElementById(section+'-section').style.display='block';
  if(section==='eleves') updateClasseSelect('eleveClasse');
  if(section==='matieres') updateClasseSelect('matiereClasse');
  if(section==='notes') updateClasseSelect('selectClasseNotes');
  if(section==='bulletins') updateClasseSelect('selectClasseBulletins');
}

function saveParametres(){
  let param = {
    nom: document.getElementById('nomEtablissement').value,
    annee: document.getElementById('anneeScolaire').value,
    directeur: document.getElementById('directeur').value,
    censeur: document.getElementById('censeur').value,
    intendant: document.getElementById('intendant').value,
    devise: document.getElementById('devise').value
  };
  localStorage.setItem('parametres', JSON.stringify(param));
  alert('Paramètres enregistrés !');
}

function addClass(){
  let cname = document.getElementById('newClassName').value;
  if(cname){
    classes.push(cname);
    localStorage.setItem('classes', JSON.stringify(classes));
    renderClasses();
    updateClasseSelect('eleveClasse');
    updateClasseSelect('matiereClasse');
    updateClasseSelect('selectClasseNotes');
    updateClasseSelect('selectClasseBulletins');
    document.getElementById('newClassName').value='';
  }
}

function renderClasses(){
  let table = document.getElementById('classesTable');
  table.innerHTML='<tr><th>Nom de la classe</th><th>Actions</th></tr>';
  classes.forEach((c,i)=>{
    let row = table.insertRow();
    row.insertCell(0).innerText=c;
    let delCell = row.insertCell(1);
    let btn = document.createElement('button');
    btn.innerText='Supprimer';
    btn.onclick=()=>{classes.splice(i,1); localStorage.setItem('classes', JSON.stringify(classes)); renderClasses();};
    delCell.appendChild(btn);
  });
}

function updateClasseSelect(id){
  let sel = document.getElementById(id);
  sel.innerHTML='';
  classes.forEach(c=>{
    let opt=document.createElement('option'); opt.value=c; opt.innerText=c; sel.appendChild(opt);
  });
}

function addEleve(){
  let e={
    nom:document.getElementById('eleveNom').value,
    sexe:document.getElementById('eleveSexe').value,
    date:document.getElementById('eleveDate').value,
    classe:document.getElementById('eleveClasse').value,
    parent:document.getElementById('eleveParent').value,
    contact:document.getElementById('eleveContact').value
  };
  eleves.push(e);
  localStorage.setItem('eleves', JSON.stringify(eleves));
  renderEleves();
}

function renderEleves(){
  let table = document.getElementById('elevesTable');
  table.innerHTML='<tr><th>Nom</th><th>Sexe</th><th>Date de naissance</th><th>Classe</th><th>Parent</th><th>Contact</th></tr>';
  eleves.forEach((e,i)=>{
    let row=table.insertRow();
    row.insertCell(0).innerText=e.nom;
    row.insertCell(1).innerText=e.sexe;
    row.insertCell(2).innerText=e.date;
    row.insertCell(3).innerText=e.classe;
    row.insertCell(4).innerText=e.parent;
    row.insertCell(5).innerText=e.contact;
  });
}

function addEnseignant(){
  let e={
    nom:document.getElementById('ensNom').value,
    sexe:document.getElementById('ensSexe').value,
    matiere:document.getElementById('ensMatiere').value,
    classe:document.getElementById('ensClasse').value,
    statut:document.getElementById('ensStatut').value
  };
  enseignants.push(e);
  localStorage.setItem('enseignants', JSON.stringify(enseignants));
  renderEnseignants();
}

function renderEnseignants(){
  let table=document.getElementById('enseignantsTable');
  table.innerHTML='<tr><th>Nom</th><th>Sexe</th><th>Matière</th><th>Classe</th><th>Statut</th></tr>';
  enseignants.forEach(e=>{
    let row=table.insertRow();
    row.insertCell(0).innerText=e.nom;
    row.insertCell(1).innerText=e.sexe;
    row.insertCell(2).innerText=e.matiere;
    row.insertCell(3).innerText=e.classe;
    row.insertCell(4).innerText=e.statut;
  });
}

function addMatiere(){
  let m={
    nom:document.getElementById('matiereNom').value,
    coef:document.getElementById('matiereCoef').value,
    classe:document.getElementById('matiereClasse').value,
    enseignant:document.getElementById('matiereEns').value
  };
  matieres.push(m);
  localStorage.setItem('matieres', JSON.stringify(matieres));
  renderMatieres();
}

function renderMatieres(){
  let table=document.getElementById('matieresTable');
  table.innerHTML='<tr><th>Nom</th><th>Coefficient</th><th>Classe</th><th>Enseignant</th></tr>';
  matieres.forEach(m=>{
    let row=table.insertRow();
    row.insertCell(0).innerText=m.nom;
    row.insertCell(1).innerText=m.coef;
    row.insertCell(2).innerText=m.classe;
    row.insertCell(3).innerText=m.enseignant;
  });
}

function loadElevesNotes(){
  let classe=document.getElementById('selectClasseNotes').value;
  let container=document.getElementById('notesContainer');
  container.innerHTML='';
  let classeEleves=eleves.filter(e=>e.classe===classe);
  classeEleves.forEach((e,i)=>{
    let div=document.createElement('div');
    div.innerHTML=`<h4>${e.nom}</h4>`;
    matieres.filter(m=>m.classe===classe).forEach(m=>{
      let val=notes[e.nom]?.[m.nom]||['','',''];
      div.innerHTML+=`${m.nom}: <input type='number' value='${val[0]}' placeholder='Note1'> <input type='number' value='${val[1]}' placeholder='Note2'> <input type='number' value='${val[2]}' placeholder='Note3'><br>`;
    });
    let saveBtn=document.createElement('button');
    saveBtn.innerText='Enregistrer notes';
    saveBtn.onclick=()=>{saveNotes(e.nom, div)};
    div.appendChild(saveBtn);
    container.appendChild(div);
  });
}

function saveNotes(eleveNom, div){
  notes[eleveNom]={};
  div.querySelectorAll('h4')[0];
  matieres.forEach((m,i)=>{
    if(m.classe===document.getElementById('selectClasseNotes').value){
      let inputs = div.querySelectorAll('input');
      notes[eleveNom][m.nom]=[inputs[i*3].value, inputs[i*3+1].value, inputs[i*3+2].value];
    }
  });
  localStorage.setItem('notes', JSON.stringify(notes));
  alert('Notes enregistrées !');
}

function loadElevesBulletins(){
  let classe=document.getElementById('selectClasseBulletins').value;
  let container=document.getElementById('bulletinsContainer');
  container.innerHTML='';
  let classeEleves=eleves.filter(e=>e.classe===classe);
  classeEleves.forEach(e=>{
    let div=document.createElement('div');
    div.style.border='1px solid #000'; div.style.margin='10px'; div.style.padding='10px';
    div.innerHTML=`<h3>${e.nom}</h3>`;
    let total=0; let count=0;
    matieres.filter(m=>m.classe===classe).forEach(m=>{
      let n=notes[e.nom]?.[m.nom]?.map(Number)||[0,0,0];
      let moy = n.reduce((a,b)=>a+b,0)/3;
      total+=moy*Number(m.coef); count+=Number(m.coef);
      div.innerHTML+=`${m.nom} (Coef ${m.coef}): ${n.join(', ')} -> Moy: ${moy.toFixed(2)}<br>`;
    });
    let moyenneGen = total/count;
    div.innerHTML+=`<b>Moyenne Générale: ${moyenneGen.toFixed(2)}</b><br>`;
    container.appendChild(div);
  });
}

// Load saved data
if(localStorage.getItem('classes')) classes=JSON.parse(localStorage.getItem('classes'));
if(localStorage.getItem('eleves')) eleves=JSON.parse(localStorage.getItem('eleves'));
if(localStorage.getItem('enseignants')) enseignants=JSON.parse(localStorage.getItem('enseignants'));
if(localStorage.getItem('matieres')) matieres=JSON.parse(localStorage.getItem('matieres'));
if(localStorage.getItem('notes')) notes=JSON.parse(localStorage.getItem('notes'));
renderClasses(); renderEleves(); renderEnseignants(); renderMatieres();
</script>
</body>
</html>
