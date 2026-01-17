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
            ... <!-- Le contenu des sections dashboard, paramètres, classes, matières, enseignants, élèves, notes, bulletins reste inchangé -->
        </div>
    </div>
    <div id="modalClasse" class="modal">
        ... <!-- Modal classe -->
    </div>
    <div id="modalMatiere" class="modal">
        ... <!-- Modal matière -->
    </div>
    <div id="modalEnseignant" class="modal">
        ... <!-- Modal enseignant -->
    </div>
    <div id="modalEleve" class="modal">
        ... <!-- Modal élève -->
    </div>
    <script>
        class Database{constructor(){this.initializeData()}initializeData(){if(!localStorage.getItem('parametres'))localStorage.setItem('parametres',JSON.stringify({nomEtablissement:'Établissement Scolaire',anneeScolaire:'2025-2026',adresse:'',telephone:'',email:'',directeur:'',censeur:'',devise:'Excellence et Discipline'}));if(!localStorage.getItem('classes'))localStorage.setItem('classes',JSON.stringify([]));if(!localStorage.getItem('matieres'))localStorage.setItem('matieres',JSON.stringify([]));i
const matieres=db.get('matieres');const select=document.getElementById('enseignantMatiere');select.innerHTML='<option value="">-- Sélectionner --</option>';matieres.forEach(matiere=>{const option=document.createElement('option');option.value=matiere.nom;option.textContent=matiere.nom;select.appendChild(option)})}
        document.getElementById('formEnseignant').addEventListener('submit',function(e){e.preventDefault();const enseignant={id:Date.now(),nom:document.getElementById('enseignantNom').value,sexe:document.getElementById('enseignantSexe').value,matiere:document.getElementById('enseignantMatiere').value,contact:document.getElementById('enseignantContact').value,statut:document.getElementById('enseignantStatut').value};db.add('enseignants',enseignant);displayEnseignants();closeModal('modalEnseignant');this.reset();updateDashboard();alert('✅ Enseignant ajouté avec succès!')});
        function displayEnseignants(){const enseignants=db.get('enseignants');const tbody=document.querySelector('#tableEnseignants tbody');tbody.innerHTML='';enseignants.forEach((enseignant,index)=>{const tr=document.createElement('tr');tr.innerHTML=`<td>${enseignant.nom}</td><td>${enseignant.sexe==='M'?'Masculin':'Féminin'}</td><td>${enseignant.matiere}</td><td>${enseignant.contact}</td><td><span class="badge ${enseignant.statut==='Permanent'?'badge-success':'badge-warning'}">${enseignant.statut}</span></td><td class="action-buttons"><button class="btn btn-danger" onclick="deleteEnseignant(${index})">🗑️ Supprimer</button></td>`;tbody.appendChild(tr)})}
        ... <!-- Le reste du code JavaScript reste inchangé, incluant la gestion des élèves, notes et bulletins -->
        window.onload=init;
    </script>
</body>
</html>
