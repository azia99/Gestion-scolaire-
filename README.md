<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Logiciel de Gestion Scolaire</title>
<style>
body {font-family: Arial, sans-serif; margin:0; padding:0; background:#f4f4f4;}
header {background:#2c3e50; color:#fff; padding:15px; text-align:center;}
#sidebar {width:250px; background:#34495e; height:100vh; position:fixed; top:0; left:0; padding-top:70px;}
#sidebar button {width:100%; padding:12px; border:none; background:none; color:#fff; text-align:left; cursor:pointer; font-size:16px;}
#sidebar button:hover {background:#1abc9c;}
#content {margin-left:250px; padding:20px;}
table {width:100%; border-collapse:collapse; margin-top:10px;}
table, th, td {border:1px solid #ddd;}
th, td {padding:8px; text-align:center;}
th {background:#2c3e50; color:#fff;}
input, select {padding:5px; margin:5px;}
button.add-btn {background:#1abc9c; color:#fff; border:none; padding:5px 10px; cursor:pointer; margin:5px 0;}
button.add-btn:hover {background:#16a085;}
.bulletin {border:1px solid #000; margin:10px 0; padding:10px;}
@media print {#sidebar, .add-btn {display:none;} body {margin:0;}}
</style>
</head>
<body>
<header><h1>Logiciel de Gestion Scolaire</h1></header>
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
<div id="dashboard-section" class="section"><h2>Tableau de bord</h2><p>Bienvenue dans le logiciel de gestion scolaire !</p></div>
<div id="parametres-section" class="section" style="display:none">
<h2>Paramètres de l'établissement</h2>
Nom: <input id="nomEtablissement"><br>
Année scolaire: <input id="anneeScolaire"><br>
Directeur: <input id="directeur"><br>
Censeur: <input id="censeur"><br>
Intendant: <input id="intendant"><br>
Devise: <input id="devise"><br>
<button class="add-btn" onclick="saveParametres()">Enregistrer</button>
</div>
<div id="classes-section" class="section" style="display:none">
<h2>Gestion des classes</h2>
<input id="newClassName" placeholder="Nom de la classe">
<button class="add-btn" onclick="addClass()">Ajouter Classe</button>
<table id="classesTable"><tr><th>Classe</th><th>Actions</th></tr></table>
</div>
<div id="eleves-section" class="section" style="display:none">
<h2>Gestion des élèves</h2>
<input id="eleveNom" placeholder="Nom et prénom">
<select id="eleveSexe"><option>Masculin</option><option>Féminin</option></select>
<input type="date" id="eleveDate">
<select id="eleveClasse"></select>
<input id="eleveParent" placeholder="Nom du parent">
<input id="eleveContact" placeholder="Contact parent">
<button class="add-btn" onclick="addEleve()">Ajouter Élève</button>
<table id="elevesTable"><tr><th>Nom</th><th>Sexe</th><th>Date de naissance</th><th>Classe</th><th>Parent</th><th>Contact</th></tr></table>
</div>
<div id="enseignants-section" class="section" style="display:none">
<h2>Gestion des enseignants</h2>
<input id="ensNom" placeholder="Nom et prénom">
<select id="ensSexe"><option>Masculin</option><option>Féminin</option></select>
<input id="ensMatiere" placeholder="Matière">
<input id="ensClasse" placeholder="Classe">
<select id="ensStatut"><option>Permanent</option><option>Vacataire</option></select>
<button class="add-btn" onclick="addEnseignant()">Ajouter Enseignant</button>
<table id="enseignantsTable"><tr><th>Nom</th><th>Sexe</th><th>Matière</th><th>Classe</th><th>Statut</th></tr></table>
</div>
<div id="matieres-section" class="section" style="display:none">
<h2>Gestion des matières</h2>
<input id="matiereNom" placeholder="Nom">
<input id="matiereCoef" type="number" placeholder="Coefficient">
<select id="matiereClasse"></select>
<input id="matiereEns" placeholder="Enseignant">
<button class="add-btn" onclick="addMatiere()">Ajouter Matière</button>
<table id="matieresTable"><tr><th>Nom</th><th>Coef</th><th>Classe</th><th>Enseignant</th></tr></table>
</div>
<div id="notes-section" class="section" style="display:none">
<h2>Gestion des notes</h2>
<select id="selectClasseNotes"></select>
<button class="add-btn" onclick="loadElevesNotes()">Charger Élèves</button>
<div id="notesContainer"></div>
</div>
<div id="bulletins-section" class="section" style="display:none">
<h2>Bulletins scolaires</h2>
<select id="selectClasseBulletins"></select>
<button class="add-btn" onclick="loadElevesBulletins()">Afficher Bulletins</button>
<button class="add-btn" onclick="window.print()">Imprimer Bulletins</button>
<div id="bulletinsContainer"></div>
</div>
<script>
let classes=[], eleves=[], enseignants=[], matieres=[], notes={};

function showSection(s){document.querySelectorAll('.section').forEach(sec=>sec.style.display='none'); document.getElementById(s+'-section').style.display='block'; updateClasseSelect('eleveClasse'); updateClasseSelect('matiereClasse'); updateClasseSelect('selectClasseNotes'); updateClasseSelect('selectClasseBulletins');}

function saveParametres(){localStorage.setItem('parametres', JSON.stringify({nom:document.getElementById('nomEtablissement').value, annee:document.getElementById('anneeScolaire').value, directeur:document.getElementById('directeur').value, censeur:document.getElementById('censeur').value, intendant:document.getElementById('intendant').value, devise:document.getElementById('devise').value})); alert('Paramètres enregistrés');}

function addClass(){let c=document.getElementById('newClassName').value; if(c){classes.push(c); localStorage.setItem('classes', JSON.stringify(classes)); renderClasses(); document.getElementById('newClassName').value='';}}
function renderClasses(){let t=document.getElementById('classesTable'); t.innerHTML='<tr><th>Classe</th><th>Actions</th></tr>'; classes.forEach((c,i)=>{let r=t.insertRow(); r.insertCell(0).innerText=c; let btn=document.createElement('button'); btn.innerText='Supprimer'; btn.onclick=()=>{classes.splice(i,1); localStorage.setItem('classes', JSON.stringify(classes)); renderClasses();}; r.insertCell(1).appendChild(btn);});}
function updateClasseSelect(id){let sel=document.getElementById(id); sel.innerHTML=''; classes.forEach(c=>{let o=document.createElement('option'); o.value=o.innerText=c; sel.appendChild(o);});}

function addEleve(){let e={nom:document.getElementById('eleveNom').value,sexe:document.getElementById('eleveSexe').value,date:document.getElementById('eleveDate').value,classe:document.getElementById('eleveClasse').value,parent:document.getElementById('eleveParent').value,contact:document.getElementById('eleveContact').value}; eleves.push(e); localStorage.setItem('eleves', JSON.stringify(eleves)); renderEleves();}
function renderEleves(){let t=document.getElementById('elevesTable'); t.innerHTML='<tr><th>Nom</th><th>Sexe</th><th>Date</th><th>Classe</th><th>Parent</th><th>Contact</th></tr>'; eleves.forEach(e=>{let r=t.insertRow(); r.insertCell(0).innerText=e.nom; r.insertCell(1).innerText=e.sexe; r.insertCell(2).innerText=e.date; r.insertCell(3).innerText=e.classe; r.insertCell(4).innerText=e.parent; r.insertCell(5).innerText=e.contact;});}

function addEnseignant(){let e={nom:document.getElementById('ensNom').value,sexe:document.getElementById('ensSexe').value,matiere:document.getElementById('ensMatiere').value,classe:document.getElementById('ensClasse').value,statut:document.getElementById('ensStatut').value}; enseignants.push(e); localStorage.setItem('enseignants', JSON.stringify(enseignants)); renderEnseignants();}
function renderEnseignants(){let t=document.getElementById('enseignantsTable'); t.innerHTML='<tr><th>Nom</th><th>Sexe</th><th>Matière</th><th>Classe</th><th>Statut</th></tr>'; enseignants.forEach(e=>{let r=t.insertRow(); r.insertCell(0).innerText=e.nom; r.insertCell(1).innerText=e.sexe; r.insertCell(2).innerText=e.matiere; r.insertCell(3).innerText=e.classe; r.insertCell(4).innerText=e.statut;});}

function addMatiere(){let m={nom:document.getElementById('matiereNom').value,coef:document.getElementById('matiereCoef').value,classe:document.getElementById('matiereClasse').value,enseignant:document.getElementById('matiereEns').value}; matieres.push(m); localStorage.setItem('matieres', JSON.stringify(matieres)); renderMatieres();}
function renderMatieres(){let t=document.getElementById('matieresTable'); t.innerHTML='<tr><th>Nom</th><th>Coef</th><th>Classe</th><th>Enseignant</th></tr>'; matieres.forEach(m=>{let r=t.insertRow(); r.insertCell(0).innerText=m.nom; r.insertCell(1).innerText=m.coef; r.insertCell(2).innerText=m.classe; r.insertCell(3).innerText=m.enseignant;});}

function loadElevesNotes(){let c=document.getElementById('selectClasseNotes').value; let cont=document.getElementById('notesContainer'); cont.innerHTML=''; eleves.filter(e=>e.classe===c).forEach(e=>{let d=document.createElement('div'); d.innerHTML='<h4>'+e.nom+'</h4>'; matieres.filter(m=>m.classe===c).forEach((m,i)=>{let v=notes[e.nom]?.[m.nom]||['','','']; d.innerHTML+=m.nom+': <input value='+v[0]+' placeholder="Note1"> <input value='+v[1]+' placeholder="Note2"> <input value='+v[2]+' placeholder="Note3"><br>';}); let btn=document.createElement('button'); btn.innerText='Enregistrer notes'; btn.onclick=()=>{saveNotes(e.nom,d);}; d.appendChild(btn); cont.appendChild(d);});}
function saveNotes(n,d){notes[n]={}; matieres.forEach((m,i)=>{if(m.classe===document.getElementById('selectClasseNotes').value){let inp=d.querySelectorAll('input'); notes[n][m.nom]=[inp[i*3].value, inp[i*3+1].value, inp[i*3+2].value];}}); localStorage.setItem('notes', JSON.stringify(notes)); alert('Notes enregistrées');}

function loadElevesBulletins(){let c=document.getElementById('selectClasseBulletins').value; let cont=document.getElementById('bulletinsContainer'); cont.innerHTML=''; eleves.filter(e=>e.classe===c).forEach(e=>{let d=document.createElement('div'); d.className='bulletin'; d.innerHTML='<h3>'+e.nom+'</h3>'; let total=0,count=0; matieres.filter(m=>m.classe===c).forEach(m=>{let n=notes[e.nom]?.[m.nom]?.map(Number)||[0,0,0]; let moy=n.reduce((a,b)=>a+b,0)/3; total+=moy*Number(m.coef); count+=Number(m.coef); d.innerHTML+=m.nom+' (Coef '+m.coef+'): '+n.join(', ')+' -> Moy: '+moy.toFixed(2)+'<br>';}); let mg=total/count; d.innerHTML+='<b>Moyenne générale: '+mg.toFixed(2)+'</b><br>'; let p=JSON.parse(localStorage.getItem('parametres')||'{}'); d.innerHTML+='<p>'+ (p.nom||'Établissement') +' - '+(p.annee||'')+'</p>'; d.innerHTML+='<p>Directeur: '+(p.directeur||'')+'</p>'; cont.appendChild(d);});

// load saved data
classes=JSON.parse(localStorage.getItem('classes')||'[]'); eleves=JSON.parse(localStorage.getItem('eleves')||'[]'); enseignants=JSON.parse(localStorage.getItem('enseignants')||'[]'); matieres=JSON.parse(localStorage.getItem('matieres')||'[]'); notes=JSON.parse(localStorage.getItem('notes')||'{}'); renderClasses(); renderEleves(); renderEnseignants(); renderMatieres();
</script>
</body>
</html>
