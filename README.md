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
                <input type="text" id="loginUser" value="admin">
            </div>
            <div class="form-group">
                <label>Mot de passe :</label>
                <input type="password" id="loginPass" value="admin">
            </div>
            <button class="btn btn-primary" id="btnLogin">🔐 Se connecter</button>
            <div class="error-message" id="loginError"></div>
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
            const u = document.getElementById('loginUser').value;
            const p = document.getElementById('loginPass').value;
            
            if (u === DB.auth.username && p === DB.auth.password) {
                document.getElementById('loginPage').classList.add('hidden');
                document.getElementById('appPage').classList.remove('hidden');
                updateHeader();
                initApp();
            } else {
                document.getElementById('loginError').textContent = '❌ Identifiant ou mot de passe incorrect';
            }
        };

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
                matricule: document.getElementById('
