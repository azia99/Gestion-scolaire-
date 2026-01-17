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
            padding: 20px;
        }

        .app-container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 10px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            overflow: hidden;
        }

        .header {
            background: #2c3e50;
            color: white;
            padding: 20px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header h1 {
            font-size: 22px;
        }

        .user-info {
            font-size: 14px;
        }

        .content-wrapper {
            display: flex;
            min-height: 700px;
        }

        .sidebar {
            width: 220px;
            background: #34495e;
            padding: 15px;
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
            transition: all 0.3s;
            text-align: left;
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
            padding: 30px;
            background: #ecf0f1;
            overflow-y: auto;
            max-height: 700px;
        }

        .page-title {
            font-size: 26px;
            color: #2c3e50;
            margin-bottom: 25px;
            font-weight: bold;
            border-bottom: 3px solid #3498db;
            padding-bottom: 10px;
        }

        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: white;
            padding: 30px;
            border-radius: 10px;
            text-align: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            transition: transform 0.3s;
        }

        .stat-card:hover {
            transform: translateY(-5px);
        }

        .stat-card.success {
            border-top: 5px solid #27ae60;
        }

        .stat-card.primary {
            border-top: 5px solid #3498db;
        }

        .stat-card.warning {
            border-top: 5px solid #f39c12;
        }

        .stat-card.danger {
            border-top: 5px solid #e74c3c;
        }

        .stat-number {
            font-size: 42px;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 10px;
        }

        .stat-label {
            font-size: 16px;
            color: #7f8c8d;
            font-weight: 500;
        }

        .activity-box {
            background: white;
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .activity-box h3 {
            margin-bottom: 20px;
            color: #2c3e50;
            font-size: 20px;
        }

        .activity-item {
            padding: 12px;
            border-left: 4px solid #3498db;
            margin-bottom: 12px;
            background: #f8f9fa;
            font-size: 14px;
            border-radius: 4px;
        }

        .action-buttons {
            margin-bottom: 25px;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            margin-right: 10px;
            transition: all 0.3s;
            font-size: 14px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
        }

        .btn-success {
            background: #27ae60;
            color: white;
        }

        .btn-primary {
            background: #3498db;
            color: white;
        }

        .btn-warning {
            background: #f39c12;
            color: white;
        }

        .data-table {
            width: 100%;
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .data-table table {
            width: 100%;
            border-collapse: collapse;
        }

        .data-table th {
            background: #34495e;
            color: white;
            padding: 16px;
            text-align: left;
            font-weight: 600;
        }

        .data-table td {
            padding: 14px 16px;
            border-bottom: 1px solid #ecf0f1;
        }

        .data-table tr:hover {
            background: #f8f9fa;
        }

        .search-box {
            margin-bottom: 20px;
        }

        .search-box input {
            width: 350px;
            padding: 12px 16px;
            border: 2px solid #ddd;
            border-radius: 6px;
            font-size: 14px;
        }

        .search-box input:focus {
            outline: none;
            border-color: #3498db;
        }

        .info-section {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }

        .info-section h2 {
            color: #2c3e50;
            margin-bottom: 20px;
            font-size: 22px;
        }

        .features-list {
            list-style: none;
            padding: 0;
        }

        .features-list li {
            padding: 12px 15px;
            margin: 8px 0;
            background: #f8f9fa;
            border-left: 5px solid #3498db;
            border-radius: 4px;
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

        .form-group select,
        .form-group input {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 6px;
            font-size: 14px;
        }

        .stat-row {
            display: flex;
            justify-content: space-between;
            padding: 14px;
            background: #f8f9fa;
            margin: 10px 0;
            border-left: 5px solid #3498db;
            border-radius: 4px;
        }

        .stat-row strong {
            color: #2c3e50;
        }
    </style>
</head>
<body>
    <div class="app-container">
        <div class="header">
            <h1>🏫 SGS - Système de Gestion Scolaire</h1>
            <div class="user-info">Utilisateur: <strong>Administrateur</strong></div>
        </div>
        
        <div class="content-wrapper">
            <div class="sidebar">
                <button class="menu-btn active" onclick="showPage('dashboard')">📊 Tableau de Bord</button>
                <button class="menu-btn" onclick="showPage('classes')">🎓 Classes</button>
                <button class="menu-btn" onclick="showPage('eleves')">👨‍🎓 Élèves</button>
                <button class="menu-btn" onclick="showPage('enseignants')">👨‍🏫 Enseignants</button>
                <button class="menu-btn" onclick="showPage('matieres')">📚 Matières</button>
                <button class="menu-btn" onclick="showPage('notes')">📝 Notes</button>
                <button class="menu-btn" onclick="showPage('bulletins')">📋 Bulletins</button>
                <button class="menu-btn" onclick="showPage('statistiques')">📈 Statistiques</button>
                <button class="menu-btn" onclick="showPage('about')">ℹ️ À propos</button>
            </div>
            
            <div class="content" id="content">
                <!-- Content will be loaded here -->
            </div>
        </div>
    </div>

    <script>
        const data = {
            classes: [
                {id: 1, nom: '6ème A', niveau: 'Collège', effectif_max: 45, effectif: 42},
                {id: 2, nom: '6ème B', niveau: 'Collège', effectif_max: 45, effectif: 40},
                {id: 3, nom: '5ème A', niveau: 'Collège', effectif_max: 45, effectif: 38},
                {id: 4, nom: '4ème A', niveau: 'Collège', effectif_max: 45, effectif: 35},
                {id: 5, nom: '3ème A', niveau: 'Collège', effectif_max: 45, effectif: 33},
                {id: 6, nom: '2nde A', niveau: 'Lycée', effectif_max: 50, effectif: 45},
                {id: 7, nom: '1ère C', niveau: 'Lycée', effectif_max: 45, effectif: 38},
                {id: 8, nom: 'Terminale C', niveau: 'Lycée', effectif_max: 45, effectif: 36}
            ],
            eleves: [
                {id: 1, matricule: 'E001', nom: 'KOUASSI', prenoms: 'Jean-Pierre', sexe: 'M', classe: '6ème A', telephone: '+225 01 02 03 04 05'},
                {id: 2, matricule: 'E002', nom: 'DIALLO', prenoms: 'Fatou', sexe: 'F', classe: '6ème A', telephone: '+225 01 02 03 04 06'},
                {id: 3, matricule: 'E003', nom: 'TRAORE', prenoms: 'Mamadou', sexe: 'M', classe: '5ème A', telephone: '+225 01 02 03 04 07'},
                {id: 4, matricule: 'E004', nom: 'KONE', prenoms: 'Awa', sexe: 'F', classe: '4ème A', telephone: '+225 01 02 03 04 08'},
                {id: 5, matricule: 'E005', nom: 'N\'GUESSAN', prenoms: 'Konan', sexe: 'M', classe: '3ème A', telephone: '+225 01 02 03 04 09'}
            ],
            enseignants: [
                {id: 1, nom: 'AMANI', prenoms: 'Kouadio', specialite: 'Mathématiques', telephone: '+225 07 08 09 10 11', email: 'amani@sgs.edu'},
                {id: 2, nom: 'BAMBA', prenoms: 'Marie', specialite: 'Français', telephone: '+225 07 08 09 10 12', email: 'bamba@sgs.edu'},
                {id: 3, nom: 'COULIBALY', prenoms: 'Ibrahim', specialite: 'Anglais', telephone: '+225 07 08 09 10 13', email: 'coulibaly@sgs.edu'}
            ],
            matieres: [
                {id: 1, nom: 'Mathématiques', coefficient: 4, description: 'Sciences exactes'},
                {id: 2, nom: 'Français', coefficient: 4, description: 'Langues'},
                {id: 3, nom: 'Anglais', coefficient: 3, description: 'Langues'},
                {id: 4, nom: 'Sciences Physiques', coefficient: 3, description: 'Sciences'},
                {id: 5, nom: 'SVT', coefficient: 2, description: 'Sciences de la Vie'},
                {id: 6, nom: 'Histoire-Géographie', coefficient: 3, description: 'Sciences humaines'},
                {id: 7, nom: 'EPS', coefficient: 1, description: 'Sport'}
            ],
            notes: [
                {id: 1, eleve: 'KOUASSI Jean-Pierre', matiere: 'Mathématiques', note: 15.5, sur: 20, type: 'Devoir', periode: 'Trimestre 1', date: '15/01/2026'},
                {id: 2, eleve: 'DIALLO Fatou', matiere: 'Français', note: 14.0, sur: 20, type: 'Composition', periode: 'Trimestre 1', date: '14/01/2026'},
                {id: 3, eleve: 'TRAORE Mamadou', matiere: 'Anglais', note: 16.5, sur: 20, type: 'Contrôle', periode: 'Trimestre 1', date: '13/01/2026'}
            ]
        };

        function showPage(page) {
            document.querySelectorAll('.menu-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');

            switch(page) {
                case 'dashboard': showDashboard(); break;
                case 'classes': showClasses(); break;
                case 'eleves': showEleves(); break;
                case 'enseignants': showEnseignants(); break;
                case 'matieres': showMatieres(); break;
                case 'notes': showNotes(); break;
                case 'bulletins': showBulletins(); break;
                case 'statistiques': showStatistiques(); break;
                case 'about': showAbout(); break;
            }
        }

        function showDashboard() {
            const content = document.getElementById('content');
            content.innerHTML = `
                <h2 class="page-title">TABLEAU DE BORD</h2>
                
                <div class="stats-container">
                    <div class="stat-card success">
                        <div class="stat-number">${data.classes.length}</div>
                        <div class="stat-label">Classes</div>
                    </div>
                    <div class="stat-card primary">
                        <div class="stat-number">${data.eleves.length}</div>
                        <div class="stat-label">Élèves</div>
                    </div>
                    <div class="stat-card warning">
                        <div class="stat-number">${data.enseignants.length}</div>
                        <div class="stat-label">Enseignants</div>
                    </div>
                    <div class="stat-card danger">
                        <div class="stat-number">${data.notes.length}</div>
                        <div class="stat-label">Notes</div>
                    </div>
                </div>

                <div class="activity-box">
                    <h3>📋 Activités Récentes</h3>
                    <div class="activity-item">• ${new Date().toLocaleDateString('fr-FR')} ${new Date().toLocaleTimeString('fr-FR')} - Application démarrée</div>
                    <div class="activity-item">• Base de données: ${data.classes.length} classes actives</div>
                    <div class="activity-item">• Total élèves inscrits: ${data.eleves.length}</div>
                    <div class="activity-item">• Total notes enregistrées: ${data.notes.length}</div>
                    <div class="activity-item">• Système opérationnel - Aucune erreur détectée</div>
                </div>
            `;
        }

        function showClasses() {
            const content = document.getElementById('content');
            let rows = data.classes.map(c => `
                <tr>
                    <td>${c.id}</td>
                    <td><strong>${c.nom}</strong></td>
                    <td>${c.niveau}</td>
                    <td>${c.effectif_max}</td>
                    <td><strong style="color: #27ae60">${c.effectif}</strong></td>
                </tr>
            `).join('');

            content.innerHTML = `
                <h2 class="page-title">GESTION DES CLASSES</h2>
                
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="alert('➕ Ajouter une nouvelle classe')">➕ Nouvelle Classe</button>
                    <button class="btn btn-primary" onclick="showClasses()">🔄 Actualiser</button>
                </div>

                <div class="data-table">
                    <table>
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Nom</th>
                                <th>Niveau</th>
                                <th>Effectif Max</th>
                                <th>Effectif Actuel</th>
                            </tr>
                        </thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function showEleves() {
            const content = document.getElementById('content');
            
            function renderTable(eleves) {
                return eleves.map(e => `
                    <tr>
                        <td>${e.id}</td>
                        <td>${e.matricule}</td>
                        <td><strong>${e.nom}</strong></td>
                        <td>${e.prenoms}</td>
                        <td>${e.sexe}</td>
                        <td>${e.classe}</td>
                        <td>${e.telephone}</td>
                    </tr>
                `).join('');
            }

            content.innerHTML = `
                <h2 class="page-title">GESTION DES ÉLÈVES</h2>
                
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="alert('➕ Ajouter un nouvel élève')">➕ Nouvel Élève</button>
                    <button class="btn btn-primary" onclick="showEleves()">🔄 Actualiser</button>
                    <button class="btn btn-warning" onclick="alert('📥 Export CSV effectué!')">📥 Exporter CSV</button>
                </div>

                <div class="search-box">
                    <input type="text" placeholder="🔍 Rechercher un élève..." oninput="filterEleves(this.value)">
                </div>

                <div class="data-table">
                    <table>
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Matricule</th>
                                <th>Nom</th>
                                <th>Prénoms</th>
                                <th>Sexe</th>
                                <th>Classe</th>
                                <th>Téléphone</th>
                            </tr>
                        </thead>
                        <tbody id="elevesTableBody">${renderTable(data.eleves)}</tbody>
                    </table>
                </div>
            `;
        }

        function filterEleves(term) {
            const filtered = data.eleves.filter(e => 
                e.nom.toLowerCase().includes(term.toLowerCase()) ||
                e.prenoms.toLowerCase().includes(term.toLowerCase()) ||
                e.matricule.toLowerCase().includes(term.toLowerCase())
            );

            const tbody = document.getElementById('elevesTableBody');
            tbody.innerHTML = filtered.map(e => `
                <tr>
                    <td>${e.id}</td>
                    <td>${e.matricule}</td>
                    <td><strong>${e.nom}</strong></td>
                    <td>${e.prenoms}</td>
                    <td>${e.sexe}</td>
                    <td>${e.classe}</td>
                    <td>${e.telephone}</td>
                </tr>
            `).join('') || '<tr><td colspan="7" style="text-align:center; padding:20px; color:#999">Aucun élève trouvé</td></tr>';
        }

        function showEnseignants() {
            const content = document.getElementById('content');
            let rows = data.enseignants.map(e => `
                <tr>
                    <td>${e.id}</td>
                    <td><strong>${e.nom}</strong></td>
                    <td>${e.prenoms}</td>
                    <td>${e.specialite}</td>
                    <td>${e.telephone}</td>
                    <td>${e.email}</td>
                </tr>
            `).join('');

            content.innerHTML = `
                <h2 class="page-title">GESTION DES ENSEIGNANTS</h2>
                
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="alert('➕ Ajouter un nouvel enseignant')">➕ Nouvel Enseignant</button>
                    <button class="btn btn-primary" onclick="showEnseignants()">🔄 Actualiser</button>
                </div>

                <div class="data-table">
                    <table>
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Nom</th>
                                <th>Prénoms</th>
                                <th>Spécialité</th>
               
