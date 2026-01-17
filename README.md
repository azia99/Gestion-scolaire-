```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Système de Gestion Scolaire - SGE</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); min-height: 100vh; }
        .container { display: flex; height: 100vh; }
        .sidebar { width: 260px; background: #2c3e50; color: white; padding: 20px; overflow-y: auto; }
        .logo-section { text-align: center; margin-bottom: 30px; padding-bottom: 20px; border-bottom: 2px solid #34495e; }
        .logo-section h1 { font-size: 24px; color: #3498db; margin-bottom: 5px; }
        .logo-section p { font-size: 12px; color: #95a5a6; }
        .menu-item { padding: 15px; margin: 8px 0; background: #34495e; border-radius: 8px; cursor: pointer; transition: all 0.3s; display: flex; align-items: center; gap: 10px; }
        .menu-item:hover { background: #3498db; transform: translateX(5px); }
        .menu-item.active { background: #3498db; }
        .menu-icon { font-size: 20px; }
        .main-content { flex: 1; padding: 30px; overflow-y: auto; background: #ecf0f1; }
        .header { background: white; padding: 20px 30px; border-radius: 10px; margin-bottom: 30px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); display: flex; justify-content: space-between; align-items: center; }
        .header h2 { color: #2c3e50; font-size: 28px; }
        .content-section { display: none; }
        .content-section.active { display: block; }
        .card { background: white; padding: 25px; border-radius: 10px; margin-bottom: 20px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        .card h3 { color: #2c3e50; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 2px solid #3498db; }
        .form-group { margin-bottom: 20px; }
        .form-group label { display: block; margin-bottom: 8px; color: #2c3e50; font-weight: 600; }
        .form-group input, .form-group select, .form-group textarea { width: 100%; padding: 12px; border: 2px solid #ddd; border-radius: 6px; font-size: 14px; transition: border 0.3s; }
        .form-group input:focus, .form-group select:focus, .form-group textarea:focus { outline: none; border-color: #3498db; }
        .btn { padding: 12px 30px; border: none; border-radius: 6px; cursor: pointer; font-size: 14px; font-weight: 600; transition: all 0.3s; }
        .btn-primary { background: #3498db; color: white; }
        .btn-primary:hover { background: #2980b9; transform: translateY(-2px); }
        .btn-success { background: #27ae60; color: white; }
        .btn-success:hover { background: #229954; }
        .btn-danger { background: #e74c3c; color: white; }
        .btn-danger:hover { background: #c0392b; }
        .btn-warning { background: #f39c12; color: white; }
        .btn-warning:hover { background: #e67e22; }
        .table-container { overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        table th { background: #34495e; color: white; padding: 12px; text-align: left; }
        table td { padding: 12px; border-bottom: 1px solid #ddd; }
        table tr:hover { background: #f8f9fa; }
        .action-buttons { display: flex; gap: 10px; }
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin-bottom: 30px; }
        .stat-card { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 25px; border-radius: 10px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); }
        .stat-card h4 { font-size: 14px; opacity: 0.9; margin-bottom: 10px; }
        .stat-card .stat-number { font-size: 36px; font-weight: bold; }
        .form-row { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; }
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 1000; justify-content: center; align-items: center; }
        .modal.active { display: flex; }
        .modal-content { background: white; padding: 30px; border-radius: 10px; max-width: 600px; width: 90%; max-height: 90vh; overflow-y: auto; }
        .modal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 2px solid #3498db; }
        .close-modal { font-size: 28px; cursor: pointer; color: #e74c3c; }
        .bulletin { background: white; padding: 40px; max-width: 800px; margin: 0 auto; }
        .bulletin-header { text-align: center; margin-bottom: 30px; border-bottom: 3px solid #2c3e50; padding-bottom: 20px; }
        .bulletin-header h1 { color: #2c3e50; margin-bottom: 5px; }
        .bulletin-info { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 30px; }
        .notes-table { margin: 20px 0; }
        .notes-table th { background: #34495e; color: white; padding: 10px; }
        .notes-table td { padding: 8px; text-align: center; }
        .appreciation { margin-top: 20px; padding: 15px; background: #ecf0f1; border-left: 4px solid #3498db; }
        @media print { .sidebar, .header, .no-print { display: none; } .main-content { padding: 0; } .bulletin { box-shadow: none; } }
        .search-box { padding: 10px; border: 2px solid #ddd; border-radius: 6px; width: 300px; font-size: 14px; }
        .badge { display: inline-block; padding: 5px 10px; border-radius: 20px; font-size: 12px; font-weight: 600; }
        .badge-success { background: #27ae60; color: white; }
        .badge-warning { background: #f39c12; color: white; }
        .badge-danger { background: #e74c3c; color: white; }
        .notes-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 15px; }
        .notes-grid input { padding: 8px; border: 2px solid #ddd; border-radius: 4px; text-align: center; }
        .moyenne-display { background: #3498db; color: white; padding: 10px; border-radius: 6px; text-align: center; font-weight: bold; font-size: 18px; }
    </style>
</head>
<body>
    <div class="container">
        <div class="sidebar">
            <div class="logo-section"><h1>🎓 SGE</h1><p>Système de Gestion d'Établissement</p></div>
            <div class="menu-item active" onclick="showSection('dashboard')"><span class="menu-icon">📊</span><span>Tableau de bord</span></div>
            <div class="menu-item" onclick="showSection('parametres')"><span class="menu-icon">⚙️</span><span>Paramètres</span></div>
            <div class="menu-item" onclick="showSection('classes')"><span class="menu-icon">🏫</span><span>Classes</span></div>
            <div class="menu-item" onclick="showSection('matieres')"><span class="menu-icon">📚</span><span>Matières</span></div>
            <div class="menu-item" onclick="showSection('enseignants')"><span class="menu-icon">👨‍🏫</span><span>Enseignants</span></div>
            <div class="menu-item" onclick="showSection('eleves')"><span class="menu-icon">👨‍🎓</span><span>Élèves</span></div>
            <div class="menu-item" onclick="showSection('notes')"><span class="menu-icon">📝</span><span>Notes</span></div>
            <div class="menu-item" onclick="showSection('bulletins')"><span class="menu-icon">📄</span><span>Bulletins</span></div>
        </div>
        <div class="main-content">
            <div id="dashboard" class="content-section active">
                <div class="header"><h2>Tableau de bord</h2><div id="currentDate"></div></div>
                <div class="stats-grid">
                    <div class="stat-card"><h4>Total Élèves</h4><div class="stat-number" id="totalEleves">0</div></div>
                    <div class="stat-card"><h4>Total Classes</h4><div class="stat-number" id="totalClasses">0</div></div>
                    <div class="stat-card"><h4>Total Enseignants</h4><div class="stat-number" id="totalEnseignants">0</div></div>
                    <div class="stat-card"><h4>Total Matières</h4><div class="stat-number" id="totalMatieres">0</div></div>
                </div>
                <div class="card"><h3>Aperçu rapide</h3><p>Bienvenue dans votre système de gestion scolaire. Utilisez le menu à gauche pour naviguer entre les différentes sections.</p></div>
            </div>
            <div id="parametres" class="content-section">
                <div class="header"><h2>Paramètres de l'établissement</h2></div>
                <div class="card"><h3>Informations générales</h3>
                    <form id="formParametres">
                        <div class="form-row">
                            <div class="form-group"><label>Nom de l'établissement</label><input type="text" id="nomEtablissement" placeholder="Ex: Collège Moderne"></div>
                            <div class="form-group"><label>Année scolaire</label><input type="text" id="anneeScolaire" placeholder="Ex: 2025-2026"></div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Adresse</label><input type="text" id="adresse" placeholder="Adresse complète"></div>
                            <div class="form-group"><label>Téléphone</label><input type="tel" id="telephone" placeholder="+225 XX XX XX XX"></div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Email</label><input type="email" id="email" placeholder="contact@etablissement.com"></div>
                            <div class="form-group"><label>Directeur Général</label><input type="text" id="directeur" placeholder="Nom du directeur"></div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Censeur/Proviseur</label><input type="text" id="censeur" placeholder="Nom du censeur"></div>
                            <div class="form-group"><label>Devise de l'établissement</label><input type="text" id="devise" placeholder="Ex: Excellence et Discipline"></div>
                        </div>
                        <button type="submit" class="btn btn-primary">💾 Enregistrer les paramètres</button>
                    </form>
                </div>
            </div>
            <div id="classes" class="content-section">
                <div class="header"><h2>Gestion des classes</h2><button class="btn btn-success" onclick="openModal('modalClasse')">➕ Ajouter une classe</button></div>
                <div class="card"><h3>Liste des classes</h3><div class="table-container"><table id="tableClasses"><thead><tr><th>Niveau</th><th>Série</th><th>Effectif max</th><th>Effectif actuel</th><th>Actions</th></tr></thead><tbody></tbody></table></div></div>
            </div>
            <div id="matieres" class="content-section">
                <div class="header"><h2>Gestion des matières</h2><button class="btn btn-success" onclick="openModal('modalMatiere')">➕ Ajouter une matière</button></div>
                <div class="card"><h3>Liste des matières</h3><div class="table-container"><table id="tableMatieres"><thead><tr><th>Matière</th><th>Coefficient</th><th>Classes concernées</th><th>Actions</th></tr></thead><tbody></tbody></table></div></div>
            </div>
            <div id="enseignants" class="content-section">
                <div class="header"><h2>Gestion des enseignants</h2><button class="btn btn-success" onclick="openModal('modalEnseignant')">➕ Ajouter un enseignant</button></div>
                <div class="card"><h3>Liste des enseignants</h3><div class="table-container"><table id="tableEnseignants"><thead><tr><th>Nom complet</th><th>Sexe</th><th>Matière</th><th>Contact</th><th>Statut</th><th>Actions</th></tr></thead><tbody></tbody></table></div></div>
            </div>
            <div id="eleves" class="content-section">
                <div class="header"><h2>Gestion des élèves</h2><button class="btn btn-success" onclick="openModal('modalEleve')">➕ Ajouter un élève</button></div>
                <div class="card"><h3>Liste des élèves</h3>
                    <div style="margin-bottom: 20px;">
                        <select id="filterClasse" onchange="filterEleves()" style="padding: 10px; border-radius: 6px; border: 2px solid #ddd;"><option value="">Toutes les classes</option></select>
                        <input type="text" id="searchEleve" class="search-box" placeholder="🔍 Rechercher un élève..." onkeyup="filterEleves()">
                    </div>
                    <div class="table-container"><table id="tableEleves"><thead><tr><th>Matricule</th><th>Nom complet</th><th>Sexe</th><th>Date naissance</th><th>Classe</th><th>Parent/Tuteur</th><th>Contact</th><th>Actions</th></tr></thead><tbody></tbody></table></div>
                </div>
            </div>
            <div id="notes" class="content-section">
                <div class="header"><h2>Gestion des notes</h2></div>
                <div class="card"><h3>Saisie des notes</h3>
                    <div class="form-row">
                        <div class="form-group"><label>Sélectionner la classe</label><select id="noteClasse" onchange="loadElevesForNotes()"><option value="">-- Choisir une classe --</option></select></div>
                        <div class="form-group"><label>Sélectionner la matière</label><select id="noteMatiere" onchange="loadElevesForNotes()"><option value="">-- Choisir une matière --</option></select></div>
                    </div>
                    <div id="notesContainer"></div>
                </div>
            </div>
            <div id="bulletins" class="content-section">
                <div class="header"><h2>Bulletins scolaires</h2></div>
                <div class="card"><h3>Générer un bulletin</h3>
                    <div class="form-row">
                        <div class="form-group"><label>Sélectionner la classe</label><select id="bulletinClasse" onchange="loadElevesForBulletin()"><option value="">-- Choisir une classe --</option></select></div>
                        <div class="form-group"><label>Sélectionner l'élève</label><select id="bulletinEleve"><option value="">-- Choisir un élève --</option></select></div>
                    </div>
                    <button class="btn btn-primary" onclick="generateBulletin()">📄 Générer le bulletin</button>
                </div>
                <div id="bulletinDisplay" class="no-print"></div>
            </div>
        </div>
    </div>
    <div id="modalClasse" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Ajouter une classe</h3><span class="close-modal" onclick="closeModal('modalClasse')">&times;</span></div>
            <form id="formClasse">
                <div class="form-group"><label>Niveau</label><select id="classeNiveau" required><option value="">-- Sélectionner --</option><option value="Sixième">Sixième</option><option value="Cinquième">Cinquième</option><option value="Quatrième">Quatrième</option><option value="Troisième">Troisième</option><option value="Seconde">Seconde</option><option value="Première">Première</option><option value="Terminale">Terminale</option></select></div>
                <div class="form-group"><label>Série</label><input type="text" id="classeSerie" placeholder="Ex: A, C, D" required></div>
                <div class="form-group"><label>Effectif maximum</label><input type="number" id="classeEffectifMax" placeholder="Ex: 50" required></div>
                <button type="submit" class="btn btn-success">✅ Enregistrer</button>
            </form>
        </div>
    </div>
    <div id="modalMatiere" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Ajouter une matière</h3><span class="close-modal" onclick="closeModal('modalMatiere')">&times;</span></div>
            <form id="formMatiere">
                <div class="form-group"><label>Nom de la matière</label><input type="text" id="matiereNom" placeholder="Ex: Mathématiques" required></div>
                <div class="form-group"><label>Coefficient</label><input type="number" id="matiereCoef" placeholder="Ex: 3" step="0.5" required></div>
                <div class="form-group"><label>Classes concernées</label><select id="matiereClasses" multiple style="height: 150px;"></select><small>Maintenez Ctrl pour sélectionner plusieurs classes</small></div>
                <button type="submit" class="btn btn-success">✅ Enregistrer</button>
            </form>
        </div>
    </div>
    <div id="modalEnseignant" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Ajouter un enseignant</h3><span class="close-modal" onclick="closeModal('modalEnseignant')">&times;</span></div>
            <form id="formEnseignant">
                <div class="form-group"><label>Nom complet</label><input type="text" id="enseignantNom" placeholder="Nom et prénom" required></div>
                <div class="form-group"><label>Sexe</label><select id="enseignantSexe" required><option value="">-- Sélectionner --</option><option value="M">Masculin</option><option value="F">Féminin</option></select></div>
                <div class="form-group"><label>Matière enseignée</label><select id="enseignantMatiere" required><option value="">-- Sélectionner --</option></select></div>
                <div class="form-group"><label>Contact</label><input type="tel" id="enseignantContact" placeholder="+225 XX XX XX XX" required></div>
                <div class="form-group"><label>Statut</label><select id="enseignantStatut" required><option value="">-- Sélectionner --</option><option value="Permanent">Permanent</option><option value="Vacataire">Vacataire</option></select></div>
                <button type="submit" class="btn btn-success">✅ Enregistrer</button>
            </form>
        </div>
    </div>
    <div id="modalEleve" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Ajouter un élève</h3><span class="close-modal" onclick="closeModal('modalEleve')">&times;</span></div>
            <form id="formEleve">
                <div class="form-group"><label>Matricule</label><input type="text" id="eleveMatricule" placeholder="Auto-généré" readonly></div>
                <div class="form-group"><label>Nom complet</label><input type="text" id="eleveNom" placeholder="Nom et prénom" required></div>
                <div class="form-group"><label>Sexe</label><select id="eleveSexe" required><option value="">-- Sélectionner --</option><option value="M">Masculin</option><option value="F">Féminin</option></select></div>
                <div class="form-group"><label>Date de naissance</label><input type="date" id="eleveDateNaissance" required></div>
                <div class="form-group"><label>Classe</label><select id="eleveClasse" required><option value="">-- Sélectionner --</option></select></div>
                <div class="form-group"><label>Nom du parent/tuteur</label><input type="text" id="eleveParent" placeholder="Nom du parent" required></div>
                <div class="form-group"><label>Contact du parent</label><input type="tel" id="eleveContactParent" placeholder="+225 XX XX XX XX" required></div>
                <button type="submit" class="btn btn-success">✅ Enregistrer</button>
            </form>
        </div>
    </div>
    <script>
        class Database{constructor(){this.initializeData()}initializeData(){if(!localStorage.getItem('parametres'))localStorage.setItem('parametres',JSON.stringify({nomEtablissement:'Établissement Scolaire',anneeScolaire:'2025-2026',adresse:'',telephone:'',email:'',directeur:'',censeur:'',devise:'Excellence et Discipline'}));if(!localStorage.getItem('classes'))localStorage.setItem('classes',JSON.stringify([]));if(!localStorage.getItem('matieres'))localStorage.setItem('matieres',JSON.stringify([]));i


```javascript
const matieres=db.get('matieres');const select=document.getElementById('enseignantMatiere');select.innerHTML='<option value="">-- Sélectionner --</option>';matieres.forEach(matiere=>{const option=document.createElement('option');option.value=matiere.nom;option.textContent=matiere.nom;select.appendChild(option)})}
        document.getElementById('formEnseignant').addEventListener('submit',function(e){e.preventDefault();const enseignant={id:Date.now(),nom:document.getElementById('enseignantNom').value,sexe:document.getElementById('enseignantSexe').value,matiere:document.getElementById('enseignantMatiere').value,contact:document.getElementById('enseignantContact').value,statut:document.getElementById('enseignantStatut').value};db.add('enseignants',enseignant);displayEnseignants();closeModal('modalEnseignant');this.reset();updateDashboard();alert('✅ Enseignant ajouté avec succès!')});
        function displayEnseignants(){const enseignants=db.get('enseignants');const tbody=document.querySelector('#tableEnseignants tbody');tbody.innerHTML='';enseignants.forEach((enseignant,index)=>{const tr=document.createElement('tr');tr.innerHTML=`<td>${enseignant.nom}</td><td>${enseignant.sexe==='M'?'Masculin':'Féminin'}</td><td>${enseignant.matiere}</td><td>${enseignant.contact}</td><td><span class="badge ${enseignant.statut==='Permanent'?'badge-success':'badge-warning'}">${enseignant.statut}</span></td><td class="action-buttons"><button class="btn btn-danger" onclick="deleteEnseignant(${index})">🗑️ Supprimer</button></td>`;tbody.appendChild(tr)})}
        function deleteEnseignant(index){if(confirm('Êtes-vous sûr de vouloir supprimer cet enseignant?')){db.delete('enseignants',index);displayEnseignants();updateDashboard();alert('✅ Enseignant supprimé!')}}
        function generateMatricule(){const year=new Date().getFullYear();const random=Math.floor(Math.random()*10000).toString().padStart(4,'0');document.getElementById('eleveMatricule').value=`${year}${random}`}
        function loadClassesForEleve(){const classes=db.get('classes');const select=document.getElementById('eleveClasse');select.innerHTML='<option value="">-- Sélectionner --</option>';classes.forEach(classe=>{const option=document.createElement('option');option.value=`${classe.niveau} ${classe.serie}`;option.textContent=`${classe.niveau} ${classe.serie}`;select.appendChild(option)})}
        document.getElementById('formEleve').addEventListener('submit',function(e){e.preventDefault();const eleve={id:Date.now(),matricule:document.getElementById('eleveMatricule').value,nom:document.getElementById('eleveNom').value,sexe:document.getElementById('eleveSexe').value,dateNaissance:document.getElementById('eleveDateNaissance').value,classe:document.getElementById('eleveClasse').value,parent:document.getElementById('eleveParent').value,contactParent:document.getElementById('eleveContactParent').value};db.add('eleves',eleve);displayEleves();closeModal('modalEleve');this.reset();updateDashboard();alert('✅ Élève ajouté avec succès!')});
        function displayEleves(){const eleves=db.get('eleves');const tbody=document.querySelector('#tableEleves tbody');const filterClasse=document.getElementById('filterClasse');const classes=db.get('classes');filterClasse.innerHTML='<option value="">Toutes les classes</option>';classes.forEach(classe=>{const option=document.createElement('option');option.value=`${classe.niveau} ${classe.serie}`;option.textContent=`${classe.niveau} ${classe.serie}`;filterClasse.appendChild(option)});tbody.innerHTML='';eleves.forEach((eleve,index)=>{const tr=document.createElement('tr');tr.innerHTML=`<td>${eleve.matricule}</td><td>${eleve.nom}</td><td>${eleve.sexe==='M'?'Masculin':'Féminin'}</td><td>${new Date(eleve.dateNaissance).toLocaleDateString('fr-FR')}</td><td><span class="badge badge-success">${eleve.classe}</span></td><td>${eleve.parent}</td><td>${eleve.contactParent}</td><td class="action-buttons"><button class="btn btn-danger" onclick="deleteEleve(${index})">🗑️ Supprimer</button></td>`;tbody.appendChild(tr)})}
        function filterEleves(){const filterValue=document.getElementById('filterClasse').value.toLowerCase();const searchValue=document.getElementById('searchEleve').value.toLowerCase();const rows=document.querySelectorAll('#tableEleves tbody tr');rows.forEach(row=>{const classe=row.cells[4].textContent.toLowerCase();const nom=row.cells[1].textContent.toLowerCase();const matricule=row.cells[0].textContent.toLowerCase();const matchClasse=filterValue===''||classe.includes(filterValue);const matchSearch=searchValue===''||nom.includes(searchValue)||matricule.includes(searchValue);if(matchClasse&&matchSearch){row.style.display=''}else{row.style.display='none'}})}
        function deleteEleve(index){if(confirm('Êtes-vous sûr de vouloir supprimer cet élève?')){const eleves=db.get('eleves');const eleveId=eleves[index].id;const notes=db.get('notes');delete notes[eleveId];db.set('notes',notes);db.delete('eleves',index);displayEleves();updateDashboard();alert('✅ Élève supprimé!')}}
        function loadClassesForNotes(){const classes=db.get('classes');const select=document.getElementById('noteClasse');select.innerHTML='<option value="">-- Choisir une classe --</option>';classes.forEach(classe=>{const option=document.createElement('option');option.value=`${classe.niveau} ${classe.serie}`;option.textContent=`${classe.niveau} ${classe.serie}`;select.appendChild(option)})}
        function loadMatieresForNotes(){const matieres=db.get('matieres');const select=document.getElementById('noteMatiere');select.innerHTML='<option value="">-- Choisir une matière --</option>';matieres.forEach(matiere=>{const option=document.createElement('option');option.value=matiere.nom;option.textContent=matiere.nom;select.appendChild(option)})}
        function loadElevesForNotes(){const classe=document.getElementById('noteClasse').value;const matiere=document.getElementById('noteMatiere').value;const container=document.getElementById('notesContainer');if(!classe||!matiere){container.innerHTML='';return}const eleves=db.get('eleves').filter(e=>e.classe===classe);const notes=db.get('notes');container.innerHTML='<h3 style="margin-top: 20px;">Élèves et notes</h3>';eleves.forEach(eleve=>{const eleveNotes=notes[eleve.id]||{};const matiereNotes=eleveNotes[matiere]||{note1:'',note2:'',note3:'',moyenne:0};const div=document.createElement('div');div.className='card';div.style.marginBottom='15px';div.innerHTML=`<h4>${eleve.nom} - ${eleve.matricule}</h4><div class="notes-grid"><div><label>Note 1</label><input type="number" min="0" max="20" step="0.5" value="${matiereNotes.note1}" onchange="updateNote(${eleve.id},'${matiere}','note1',this.value)"></div><div><label>Note 2</label><input type="number" min="0" max="20" step="0.5" value="${matiereNotes.note2}" onchange="updateNote(${eleve.id},'${matiere}','note2',this.value)"></div><div><label>Note 3</label><input type="number" min="0" max="20" step="0.5" value="${matiereNotes.note3}" onchange="updateNote(${eleve.id},'${matiere}','note3',this.value)"></div></div><div class="moyenne-display" id="moyenne-${eleve.id}-${matiere.replace(/\s/g,'-')}">Moyenne: ${matiereNotes.moyenne.toFixed(2)} / 20</div>`;container.appendChild(div)})}
        function updateNote(eleveId,matiere,noteType,value){const notes=db.get('notes');if(!notes[eleveId])notes[eleveId]={};if(!notes[eleveId][matiere])notes[eleveId][matiere]={note1:'',note2:'',note3:'',moyenne:0};notes[eleveId][matiere][noteType]=parseFloat(value)||0;const n1=parseFloat(notes[eleveId][matiere].note1)||0;const n2=parseFloat(notes[eleveId][matiere].note2)||0;const n3=parseFloat(notes[eleveId][matiere].note3)||0;const count=(n1>0?1:0)+(n2>0?1:0)+(n3>0?1:0);if(count>0){notes[eleveId][matiere].moyenne=(n1+n2+n3)/count}else{notes[eleveId][matiere].moyenne=0}db.set('notes',notes);const moyenneDiv=document.getElementById(`moyenne-${eleveId}-${matiere.replace(/\s/g,'-')}`);if(moyenneDiv){moyenneDiv.textContent=`Moyenne: ${notes[eleveId][matiere].moyenne.toFixed(2)} / 20`}}
        function loadClassesForBulletin(){const classes=db.get('classes');const select=document.getElementById('bulletinClasse');select.innerHTML='<option value="">-- Choisir une classe --</option>';classes.forEach(classe=>{const option=document.createElement('option');option.value=`${classe.niveau} ${classe.serie}`;option.textContent=`${classe.niveau} ${classe.serie}`;select.appendChild(option)})}
        function loadElevesForBulletin(){const classe=document.getElementById('bulletinClasse').value;const select=document.getElementById('bulletinEleve');select.innerHTML='<option value="">-- Choisir un élève --</option>';if(!classe)return;const eleves=db.get('eleves').filter(e=>e.classe===classe);eleves.forEach(eleve=>{const option=document.createElement('option');option.value=eleve.id;option.textContent=`${eleve.nom} - ${eleve.matricule}`;select.appendChild(option)})}
        function generateBulletin(){const eleveId=parseInt(document.getElementById('bulletinEleve').value);if(!eleveId){alert('⚠️ Veuillez sélectionner un élève');return}const eleves=db.get('eleves');const eleve=eleves.find(e=>e.id===eleveId);const notes=db.get('notes')[eleveId]||{};const matieres=db.get('matieres');const params=db.get('parametres');const matieresClasse=matieres.filter(m=>m.classes.includes(eleve.classe));let totalPoints=0;let totalCoef=0;matieresClasse.forEach(matiere=>{const matiereNotes=notes[matiere.nom]||{moyenne:0};totalPoints+=matiereNotes.moyenne*matiere.coefficient;totalCoef+=matiere.coefficient});const moyenneGenerale=totalCoef>0?(totalPoints/totalCoef):0;const elevesClasse=eleves.filter(e=>e.classe===eleve.classe);const allNotes=db.get('notes');const moyennes=elevesClasse.map(e=>{const eNotes=allNotes[e.id]||{};let pts=0;let coef=0;matieresClasse.forEach(m=>{const mNotes=eNotes[m.nom]||{moyenne:0};pts+=mNotes.moyenne*m.coefficient;coef+=m.coefficient});return{id:e.id,moyenne:coef>0?pts/coef:0}});moyennes.sort((a,b)=>b.moyenne-a.moyenne);const rang=moyennes.findIndex(m=>m.id===eleveId)+1;let appreciation='';if(moyenneGenerale>=16)appreciation='Excellent travail! Continuez ainsi.';else if(moyenneGenerale>=14)appreciation='Très bon travail. Félicitations!';else if(moyenneGenerale>=12)appreciation='Bon travail. Peut mieux faire.';else if(moyenneGenerale>=10)appreciation='Travail passable. Encouragements.';else appreciation='Travail insuffisant. Doit redoubler d\'efforts.';const bulletinHTML=`<div class="card bulletin"><div class="bulletin-header"><h1>${params.nomEtablissement}</h1><p>${params.adresse}</p><p>Tél: ${params.telephone} | Email: ${params.email}</p><p style="font-style: italic; margin-top: 10px;">${params.devise}</p><h2 style="margin-top: 20px; color: #3498db;">BULLETIN SCOLAIRE</h2><p>Année scolaire: ${params.anneeScolaire}</p></div><div class="bulletin-info"><div><p><strong>Nom de l'élève:</strong> ${eleve.nom}</p><p><strong>Matricule:</strong> ${eleve.matricule}</p><p><strong>Date de naissance:</strong> ${new Date(eleve.dateNaissance).toLocaleDateString('fr-FR')}</p></div><div><p><strong>Classe:</strong> ${eleve.classe}</p><p><strong>Effectif:</strong> ${elevesClasse.length}</p><p><strong>Rang:</strong> ${rang}/${elevesClasse.length}</p></div></div><table class="notes-table"><thead><tr><th>Matières</th><th>Coefficient</th><th>Note 1</th><th>Note 2</th><th>Note 3</th><th>Moyenne</th><th>Points</th></tr></thead><tbody>${matieresClasse.map(matiere=>{const matiereNotes=notes[matiere.nom]||{note1:'-',note2:'-',note3:'-',moyenne:0};const points=matiereNotes.moyenne*matiere.coefficient;return`<tr><td style="text-align: left;"><strong>${matiere.nom}</strong></td><td>${matiere.coefficient}</td><td>${matiereNotes.note1||'-'}</td><td>${matiereNotes.note2||'-'}</td><td>${matiereNotes.note3||'-'}</td><td><strong>${matiereNotes.moyenne.toFixed(2)}</strong></td><td>${points.toFixed(2)}</td></tr>`}).join('')}</tbody><tfoot><tr style="background: #34495e; color: white; font-weight: bold;"><td colspan="5" style="text-align: right;">TOTAUX:</td><td>${moyenneGenerale.toFixed(2)} / 20</td><td>${totalPoints.toFixed(2)}</td></tr></tfoot></table><div class="appreciation"><h4>Appréciation du conseil de classe:</h4><p>${appreciation}</p></div><div style="margin-top: 40px; display: grid; grid-template-columns: 1fr 1fr; gap: 40px;"><div><p><strong>Le Directeur</strong></p><p style="margin-top: 40px;">${params.directeur}</p></div><div><p><strong>Le Censeur</strong></p><p style="margin-top: 40px;">${params.censeur}</p></div></div><div style="text-align: center; margin-top: 40px;"><button class="btn btn-primary" onclick="window.print()">🖨️ Imprimer le bulletin</button><button class="btn btn-warning" onclick="document.getElementById('bulletinDisplay').innerHTML=''">❌ Fermer</button></div></div>`;document.getElementById('bulletinDisplay').innerHTML=bulletinHTML}
        window.onload=init;
    </script>
</body>
</html>
```
