<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SGS - Système de Gestion Scolaire</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }

        .login-container {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .login-box {
            background: white;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
            width: 100%;
            max-width: 400px;
        }

        .login-box h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 10px;
            font-size: 28px;
        }

        .login-box p {
            text-align: center;
            color: #7f8c8d;
            margin-bottom: 30px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            color: #2c3e50;
            font-weight: 600;
        }

        .form-group input, .form-group select {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 6px;
            font-size: 14px;
        }

        .form-group input:focus, .form-group select:focus {
            outline: none;
            border-color: #3498db;
        }

        .btn {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s;
        }

        .btn-primary {
            background: #3498db;
            color: white;
        }

        .btn-primary:hover {
            background: #2980b9;
        }

        .btn-success {
            background: #27ae60;
            color: white;
        }

        .btn-danger {
            background: #e74c3c;
            color: white;
        }

        .error-message {
            color: #e74c3c;
            text-align: center;
            margin-top: 15px;
            font-size: 14px;
        }

        .hidden {
            display: none !important;
        }

        .app-container {
            width: 100%;
            height: 100vh;
            background: white;
            display: flex;
            flex-direction: column;
        }

        .header {
            background: #2c3e50;
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header-info h1 {
            font-size: 20px;
        }

        .header-details {
            font-size: 12px;
            color: #bdc3c7;
            margin-top: 5px;
        }

        .logout-btn {
            padding: 8px 16px;
            background: #e74c3c;
            border: none;
            color: white;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }

        .content-wrapper {
            display: flex;
            flex: 1;
            overflow: hidden;
        }

        .sidebar {
            width: 250px;
            background: #34495e;
            padding: 15px;
            overflow-y: auto;
        }

        .menu-btn {
            width: 100%;
            padding: 14px;
            margin: 5px 0;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 600;
            text-align: left;
            transition: all 0.3s;
        }

        .menu-btn:hover {
            background: #2980b9;
            transform: translateX(5px);
        }

        .menu-btn.active {
            background: #27ae60;
        }

        .content {
            flex: 1;
            padding: 20px;
            background: #ecf0f1;
            overflow-y: auto;
        }

        .page-title {
            font-size: 24px;
            color: #2c3e50;
            margin-bottom: 20px;
            font-weight: bold;
            border-bottom: 3px solid #3498db;
            padding-bottom: 10px;
        }

        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: white;
            padding: 30px;
            border-radius: 10px;
            text-align: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .stat-card.success { border-top: 5px solid #27ae60; }
        .stat-card.primary { border-top: 5px solid #3498db; }
        .stat-card.warning { border-top: 5px solid #f39c12; }
        .stat-card.danger { border-top: 5px solid #e74c3c; }

        .stat-number {
            font-size: 48px;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 10px;
        }

        .stat-label {
            font-size: 16px;
            color: #7f8c8d;
        }

        .action-buttons {
            margin-bottom: 20px;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .data-table {
            background: white;
            border-radius: 10px;
            overflow-x: auto;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .data-table table {
            width: 100%;
            border-collapse: collapse;
        }

        .data-table th {
            background: #34495e;
            color: white;
            padding: 15px;
            text-align: left;
        }

        .data-table td {
            padding: 12px 15px;
            border-bottom: 1px solid #ecf0f1;
        }

        .data-table tr:hover {
            background: #f8f9fa;
        }

        .info-section {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }

        .info-section h2 {
            color: #2c3e50;
            margin-bottom: 20px;
            font-size: 20px;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            z-index: 10000;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            border-bottom: 2px solid #3498db;
            padding-bottom: 10px;
        }

        .modal-header h2 {
            font-size: 22px;
            color: #2c3e50;
        }

        .close-btn {
            background: #e74c3c;
            color: white;
            border: none;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <div class="login-container" id="loginPage">
        <div class="login-box">
            <h1>🏫 SGS</h1>
            <p>Système de Gestion Scolaire</p>
            <div class="form-group">
                <label>Identifiant :</label>
                <input type="text" id="loginUser" placeholder="admin">
            </div>
            <div class="form-group">
                <label>Mot de passe :</label>
                <input type="password" id="loginPass" placeholder="admin">
            </div>
            <button class="btn btn-primary" id="btnLogin">🔐 Se connecter</button>
            <div class="error-message" id="loginError"></div>
            <p style="text-align: center; margin-top: 15px; font-size: 12px; color: #95a5a6;">
                Par défaut : admin / admin
            </p>
        </div>
    </div>

    <div class="app-container hidden" id="appPage">
        <div class="header">
            <div class="header-info">
                <h1>🏫 SGS</h1>
                <div class="header-details" id="headerInfo"></div>
            </div>
            <button class="logout-btn" id="btnLogout">🚪 Déconnexion</button>
        </div>
        
        <div class="content-wrapper">
            <div class="sidebar" id="sidebar"></div>
            <div class="content" id="mainContent"></div>
        </div>
    </div>

    <div class="modal" id="modal">
        <div class="modal-content" id="modalContent"></div>
    </div>

    <script>
        const DB = {
            auth: { username: 'admin', password: 'admin' },
            settings: { etablissement: 'Mon Établissement', annee: '2025-2026', trimestre: 'Trimestre 1' },
            classes: [{id: 1, nom: '6ème A', niveau: 'Collège', effectif_max: 45, effectif: 42}],
            eleves: [{id: 1, matricule: 'E001', nom: 'KOUASSI', prenoms: 'Jean', sexe: 'M', classe: '6ème A', tel: '+225 01 02 03 04 05'}],
            enseignants: [{id: 1, nom: 'AMANI', prenoms: 'Kouadio', specialite: 'Mathématiques', tel: '+225 07 08 09 10 11', email: 'amani@sgs.edu'}],
            matieres: [{id: 1, nom: 'Mathématiques', coef: 4, desc: 'Sciences exactes'}],
            notes: [{id: 1, eleve: 'KOUASSI Jean', matiere: 'Mathématiques', note: 15.5, type: 'Devoir', date: '15/01/2026'}]
        };

        let currentPage = 'dashboard';

        // Connexion
        document.getElementById('btnLogin').onclick = function() {
            const u = document.getElementById('loginUser').value.trim();
            const p = document.getElementById('loginPass').value.trim();
            
            console.log('Tentative de connexion avec:', u, p);
            console.log('Identifiants attendus:', DB.auth.username, DB.auth.password);
            
            if (u === DB.auth.username && p === DB.auth.password) {
                console.log('Connexion réussie !');
                document.getElementById('loginPage').classList.add('hidden');
                document.getElementById('appPage').classList.remove('hidden');
                updateHeader();
                initApp();
            } else {
                console.log('Échec de connexion');
                document.getElementById('loginError').textContent = '❌ Identifiant ou mot de passe incorrect';
            }
        };

        // Permettre la connexion avec la touche Entrée
        document.getElementById('loginPass').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                document.getElementById('btnLogin').click();
            }
        });

        document.getElementById('loginUser').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                document.getElementById('btnLogin').click();
            }
        });

        // Déconnexion
        document.getElementById('btnLogout').onclick = function() {
            if (confirm('Voulez-vous vraiment vous déconnecter ?')) {
                document.getElementById('appPage').classList.add('hidden');
                document.getElementById('loginPage').classList.remove('hidden');
                document.getElementById('loginUser').value = '';
                document.getElementById('loginPass').value = '';
                document.getElementById('loginError').textContent = '';
            }
        };

        function updateHeader() {
            document.getElementById('headerInfo').innerHTML = 
                DB.settings.etablissement + ' | ' + DB.settings.annee + ' | ' + DB.settings.trimestre;
        }

        function initApp() {
            renderSidebar();
            renderDashboard();
        }

        function renderSidebar() {
            const menus = [
                {id: 'dashboard', label: '📊 Tableau de bord'},
                {id: 'classes', label: '🎓 Gestion des classes'},
                {id: 'eleves', label: '👨‍🎓 Gestion des élèves'},
                {id: 'enseignants', label: '👨‍🏫 Gestion des enseignants'},
                {id: 'matieres', label: '📚 Gestion des matières'},
                {id: 'notes', label: '📝 Gestion des notes'},
                {id: 'bulletins', label: '📋 Gestion des bulletins'},
                {id: 'statistiques', label: '📈 Statistiques'},
                {id: 'parametres', label: '⚙️ Paramètres'}
            ];

            let html = '';
            menus.forEach(m => {
                html += `<button class="menu-btn ${m.id === currentPage ? 'active' : ''}" onclick="goTo('${m.id}')">${m.label}</button>`;
            });
            document.getElementById('sidebar').innerHTML = html;
        }

        function goTo(page) {
            currentPage = page;
            renderSidebar();
            
            if (page === 'dashboard') renderDashboard();
            else if (page === 'classes') renderClasses();
            else if (page === 'eleves') renderEleves();
            else if (page === 'enseignants') renderEnseignants();
            else if (page === 'matieres') renderMatieres();
            else if (page === 'notes') renderNotes();
            else if (page === 'bulletins') renderBulletins();
            else if (page === 'statistiques') renderStatistiques();
            else if (page === 'parametres') renderParametres();
        }

        function renderDashboard() {
            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">📊 TABLEAU DE BORD</h2>
                <div class="stats-container">
                    <div class="stat-card success">
                        <div class="stat-number">${DB.classes.length}</div>
                        <div class="stat-label">Classes</div>
                    </div>
                    <div class="stat-card primary">
                        <div class="stat-number">${DB.eleves.length}</div>
                        <div class="stat-label">Élèves</div>
                    </div>
                    <div class="stat-card warning">
                        <div class="stat-number">${DB.enseignants.length}</div>
                        <div class="stat-label">Enseignants</div>
                    </div>
                    <div class="stat-card danger">
                        <div class="stat-number">${DB.notes.length}</div>
                        <div class="stat-label">Notes</div>
                    </div>
                </div>
                <div class="info-section">
                    <h2>📋 Informations</h2>
                    <p><strong>Établissement :</strong> ${DB.settings.etablissement}</p>
                    <p><strong>Année :</strong> ${DB.settings.annee}</p>
                    <p><strong>Période :</strong> ${DB.settings.trimestre}</p>
                </div>
            `;
        }

        function renderClasses() {
            let rows = '';
            DB.classes.forEach(c => {
                rows += `<tr><td>${c.id}</td><td><strong>${c.nom}</strong></td><td>${c.niveau}</td><td>${c.effectif_max}</td><td>${c.effectif}</td></tr>`;
            });

            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">🎓 GESTION DES CLASSES</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="addClasse()">➕ Nouvelle Classe</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead><tr><th>ID</th><th>Nom</th><th>Niveau</th><th>Max</th><th>Actuel</th></tr></thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function addClasse() {
            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvelle Classe</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <div class="form-group">
                    <label>Nom :</label>
                    <input type="text" id="nom" placeholder="Ex: 6ème B">
                </div>
                <div class="form-group">
                    <label>Niveau :</label>
                    <select id="niveau">
                        <option value="Collège">Collège</option>
                        <option value="Lycée">Lycée</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Effectif Max :</label>
                    <input type="number" id="max" value="45">
                </div>
                <div class="form-group">
                    <label>Effectif Actuel :</label>
                    <input type="number" id="actuel" value="0">
                </div>
                <button class="btn btn-success" onclick="saveClasse()">✅ Enregistrer</button>
            `);
        }

        function saveClasse() {
            DB.classes.push({
                id: DB.classes.length + 1,
                nom: document.getElementById('nom').value,
                niveau: document.getElementById('niveau').value,
                effectif_max: parseInt(document.getElementById('max').value),
                effectif: parseInt(document.getElementById('actuel').value)
            });
            closeModal();
            renderClasses();
            alert('✅ Classe ajoutée !');
        }

        function renderEleves() {
            let rows = '';
            DB.eleves.forEach(e => {
                rows += `<tr><td>${e.matricule}</td><td><strong>${e.nom}</strong></td><td>${e.prenoms}</td><td>${e.sexe}</td><td>${e.classe}</td></tr>`;
            });

            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">👨‍🎓 GESTION DES ÉLÈVES</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="addEleve()">➕ Nouvel Élève</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead><tr><th>Matricule</th><th>Nom</th><th>Prénoms</th><th>Sexe</th><th>Classe</th></tr></thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function addEleve() {
            let classOptions = '';
            DB.classes.forEach(c => {
                classOptions += `<option value="${c.nom}">${c.nom}</option>`;
            });

            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvel Élève</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <div class="form-group">
                    <label>Matricule :</label>
                    <input type="text" id="matricule">
                </div>
                <div class="form-group">
                    <label>Nom :</label>
                    <input type="text" id="nom">
                </div>
                <div class="form-group">
                    <label>Prénoms :</label>
                    <input type="text" id="prenoms">
                </div>
                <div class="form-group">
                    <label>Sexe :</label>
                    <select id="sexe">
                        <option value="M">Masculin</option>
                        <option value="F">Féminin</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Classe :</label>
                    <select id="classe">${classOptions}</select>
                </div>
                <div class="form-group">
                    <label>Téléphone :</label>
                    <input type="tel" id="tel">
                </div>
                <button class="btn btn-success" onclick="saveEleve()">✅ Enregistrer</button>
            `);
        }

        function saveEleve() {
            DB.eleves.push({
                id: DB.eleves.length + 1,
                matricule: document.getElementById('matricule').value,
                nom: document.getElementById('nom').value,
                prenoms: document.getElementById('prenoms').value,
                sexe: document.getElementById('sexe').value,
                classe: document.getElementById('classe').value,
                tel: document.getElementById('tel').value
            });
            closeModal();
            renderEleves();
            alert('✅ Élève ajouté !');
        }

        function renderEnseignants() {
            let rows = '';
            DB.enseignants.forEach(e => {
                rows += `<tr><td><strong>${e.nom}</strong></td><td>${e.prenoms}</td><td>${e.specialite}</td><td>${e.email}</td></tr>`;
            });

            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">👨‍🏫 GESTION DES ENSEIGNANTS</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="addEnseignant()">➕ Nouvel Enseignant</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead><tr><th>Nom</th><th>Prénoms</th><th>Spécialité</th><th>Email</th></tr></thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function addEnseignant() {
            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvel Enseignant</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <div class="form-group">
                    <label>Nom :</label>
                    <input type="text" id="nom">
                </div>
                <div class="form-group">
                    <label>Prénoms :</label>
                    <input type="text" id="prenoms">
                </div>
                <div class="form-group">
                    <label>Spécialité :</label>
                    <input type="text" id="specialite">
                </div>
                <div class="form-group">
                    <label>Téléphone :</label>
                    <input type="tel" id="tel">
                </div>
                <div class="form-group">
                    <label>Email :</label>
                    <input type="email" id="email">
                </div>
                <button class="btn btn-success" onclick="saveEnseignant()">✅ Enregistrer</button>
            `);
        }

        function saveEnseignant() {
            DB.enseignants.push({
                id: DB.enseignants.length + 1,
                nom: document.getElementById('nom').value,
                prenoms: document.getElementById('prenoms').value,
                specialite: document.getElementById('specialite').value,
                tel: document.getElementById('tel').value,
                email: document.getElementById('email').value
            });
            closeModal();
            renderEnseignants();
            alert('✅ Enseignant ajouté !');
        }

        function renderMatieres() {
            let rows = '';
            DB.matieres.forEach(m => {
                rows += `<tr><td>${m.id}</td><td><strong>${m.nom}</strong></td><td>${m.coef}</td><td>${m.desc}</td></tr>`;
            });

            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">📚 GESTION DES MATIÈRES</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="addMatiere()">➕ Nouvelle Matière</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead><tr><th>ID</th><th>Matière</th><th>Coef.</th><th>Description</th></tr></thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function addMatiere() {
            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvelle Matière</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <div class="form-group">
                    <label>Nom :</label>
                    <input type="text" id="nom">
                </div>
                <div class="form-group">
                    <label>Coefficient :</label>
                    <input type="number" id="coef" min="1" max="10">
                </div>
                <div class="form-group">
                    <label>Description :</label>
                    <input type="text" id="desc">
                </div>
                <button class="btn btn-success" onclick="saveMatiere()">✅ Enregistrer</button>
            `);
        }

        function saveMatiere() {
            DB.matieres.push({
                id: DB.matieres.length + 1,
                nom: document.getElementById('nom').value,
                coef: parseInt(document.getElementById('coef').value),
                desc: document.getElementById('desc').value
            });
            closeModal();
            renderMatieres();
            alert('✅ Matière ajoutée !');
        }

        function renderNotes() {
            let rows = '';
            DB.notes.forEach(n => {
                rows += `<tr><td>${n.eleve}</td><td>${n.matiere}</td><td><strong style="color: #27ae60">${n.note}/20</strong></td><td>${n.type}</td><td>${n.date}</td></tr>`;
            });

            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">📝 GESTION DES NOTES</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="addNote()">➕ Nouvelle Note</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead><tr><th>Élève</th><th>Matière</th><th>Note</th><th>Type</th><th>Date</th></tr></thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function addNote() {
            let eleveOptions = '';
            DB.eleves.forEach(e => {
                eleveOptions += `<option value="${e.nom} ${e.prenoms}">${e.nom} ${e.prenoms}</option>`;
            });

            let matiereOptions = '';
            DB.matieres.forEach(m => {
                matiereOptions += `<option value="${m.nom}">${m.nom}</option>`;
            });

            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvelle Note</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <div class="form-group">
                    <label>Élève :</label>
                    <select id="eleve">${eleveOptions}</select>
                </div>
                <div class="form-group">
                    <label>Matière :</label>
                    <select id="matiere">${matiereOptions}</select>
                </div>
                <div class="form-group">
                    <label>Note :</label>
                    <input type="number" id="note" min="0" max="20" step="0.5">
                </div>
                <div class="form-group">
                    <label>Type :</label>
                    <select id="type">
                        <option value="Devoir">Devoir</option>
                        <option value="Contrôle">Contrôle</option>
                        <option value="Composition">Composition</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Date :</label>
                    <input type="date" id="date">
                </div>
                <button class="btn btn-success" onclick="saveNote()">✅ Enregistrer</button>
            `);
        }

        function saveNote() {
            const dateInput = new Date(document.getElementById('date').value);
            DB.notes.push({
                id: DB.notes.length + 1,
                eleve: document.getElementById('eleve').value,
                matiere: document.getElementById('matiere').value,
                note: parseFloat(document.getElementById('note').value),
                type: document.getElementById('type').value,
                date: dateInput.toLocaleDateString('fr-FR')
            });
            closeModal();
            renderNotes();
            alert('✅ Note ajoutée !');
        }

        function renderBulletins() {
            let classOptions = '';
            DB.classes.forEach(c => {
                classOptions += `<option value="${c.nom}">${c.nom}</option>`;
            });

            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">📋 GESTION DES BULLETINS</h2>
                <div class="info-section">
                    <h2>📋 Génération de Bulletins</h2>
                    <div class="form-group">
                        <label>Classe :</label>
                        <select>${classOptions}</select>
                    </div>
                    <div class="form-group">
                        <label>Période :</label>
                        <select>
                            <option>Trimestre 1</option>
                            <option>Trimestre 2</option>
                            <option>Trimestre 3</option>
                        </select>
                    </div>
                    <button class="btn btn-success" onclick="alert('✅ Bulletin généré !')">📄 Générer</button>
                </div>
            `;
        }

        function renderStatistiques() {
            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">📈 STATISTIQUES</h2>
                <div class="stats-container">
                    <div class="stat-card success">
                        <div class="stat-number">267</div>
                        <div class="stat-label">Total Élèves</div>
                    </div>
                    <div class="stat-card primary">
                        <div class="stat-number">12.8</div>
                        <div class="stat-label">Moyenne Générale</div>
                    </div>
                    <div class="stat-card warning">
                        <div class="stat-number">92%</div>
                        <div class="stat-label">Taux de Réussite</div>
                    </div>
                    <div class="stat-card danger">
                        <div class="stat-number">${DB.classes.length}</div>
                        <div class="stat-label">Classes</div>
                    </div>
                </div>
                <div class="info-section">
                    <h2>📊 Détails</h2>
                    <p><strong>Année :</strong> ${DB.settings.annee}</p>
                    <p><strong>Période :</strong> ${DB.settings.trimestre}</p>
                    <p><strong>Matières :</strong> ${DB.matieres.length}</p>
                    <p><strong>Enseignants :</strong> ${DB.enseignants.length}</p>
                    <p><strong>Notes :</strong> ${DB.notes.length}</p>
                </div>
            `;
        }

        function renderParametres() {
            document.getElementById('mainContent').innerHTML = `
                <h2 class="page-title">⚙️ PARAMÈTRES</h2>
                
                <div class="info-section">
                    <h2>🏫 Paramètres de l'établissement</h2>
                    <div class="form-group">
                        <label>Nom de l'établissement :</label>
                        <input type="text" id="setEtab" value="${DB.settings.etablissement}">
                    </div>
                    <div class="form-group">
                        <label>Année académique :</label>
                        <input type="text" id="setAnnee" value="${DB.settings.annee}">
                    </div>
                    <div class="form-group">
                        <label>Trimestre :</label>
                        <select id="setTrim">
                            <option value="Trimestre 1" ${DB.settings.trimestre === 'Trimestre 1' ? 'selected' : ''}>Trimestre 1</option>
                            <option value="Trimestre 2" ${DB.settings.trimestre === 'Trimestre 2' ? 'selected' : ''}>Trimestre 2</option>
                            <option value="Trimestre 3" ${DB.settings.trimestre === 'Trimestre 3' ? 'selected' : ''}>Trimestre 3</option>
                        </select>
                    </div>
                    <button class="btn btn-success" onclick="saveSettings()">✅ Enregistrer</button>
                </div>

                <div class="info-section">
                    <h2>🔐 Modifier les identifiants</h2>
                    <div class="form-group">
                        <label>Nouvel identifiant :</label>
                        <input type="text" id="newUser" value="${DB.auth.username}">
                    </div>
                    <div class="form-group">
                        <label>Nouveau mot de passe :</label>
                        <input type="password" id="newPass" value="${DB.auth.password}">
                    </div>
                    <div class="form-group">
                        <label>Confirmer :</label>
                        <input type="password" id="confirmPass">
                    </div>
                    <button class="btn btn-primary" onclick="saveAuth()">🔑 Modifier</button>
                </div>
            `;
        }

        function saveSettings() {
            DB.settings.etablissement = document.getElementById('setEtab').value;
            DB.settings.annee = document.getElementById('setAnnee').value;
            DB.settings.trimestre = document.getElementById('setTrim').value;
            updateHeader();
            alert('✅ Paramètres enregistrés !');
        }

        function saveAuth() {
            const pass = document.getElementById('newPass').value;
            const confirm = document.getElementById('confirmPass').value;
            
            if (pass !== confirm) {
                alert('❌ Les mots de passe ne correspondent pas !');
                return;
            }
            
            DB.auth.username = document.getElementById('newUser').value;
            DB.auth.password = pass;
            alert('✅ Identifiants modifiés ! Utilisez-les à la prochaine connexion.');
        }

        function showModal(content) {
            document.getElementById('modalContent').innerHTML = content;
            document.getElementById('modal').classList.add('active');
        }

        function closeModal() {
            document.getElementById('modal').classList.remove('active');
        }

        document.getElementById('modal').onclick = function(e) {
            if (e.target.id === 'modal') closeModal();
        };
    </script>
</body>
</html>
