<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SGE - Système de Gestion Établissement</title>
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #34495e;
            --accent: #3498db;
            --success: #27ae60;
            --danger: #e74c3c;
            --light: #ecf0f1;
            --dark: #2c3e50;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background-color: var(--light); display: flex; height: 100vh; overflow: hidden; }

        /* Sidebar */
        .sidebar { width: 260px; background: var(--primary); color: white; display: flex; flex-direction: column; transition: 0.3s; }
        .logo-area { padding: 20px; text-align: center; background: rgba(0,0,0,0.1); border-bottom: 1px solid rgba(255,255,255,0.1); }
        .nav-menu { flex: 1; padding: 10px; }
        .nav-item { 
            padding: 12px 15px; margin: 5px 0; border-radius: 8px; cursor: pointer; 
            display: flex; align-items: center; gap: 10px; transition: 0.2s;
        }
        .nav-item:hover { background: var(--accent); }
        .nav-item.active { background: var(--accent); font-weight: bold; }

        /* Main Content */
        .main-content { flex: 1; display: flex; flex-direction: column; overflow-y: auto; }
        header { 
            background: white; padding: 15px 30px; display: flex; justify-content: space-between; 
            align-items: center; box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .container { padding: 25px; max-width: 1200px; margin: 0 auto; width: 100%; }
        .section { display: none; animation: fadeIn 0.3s; }
        .section.active { display: block; }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* Cards & UI Elements */
        .card { background: white; padding: 20px; border-radius: 10px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); margin-bottom: 20px; }
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; margin-bottom: 30px; }
        .stat-card { background: white; padding: 20px; border-radius: 10px; border-left: 5px solid var(--accent); }
        
        /* Forms */
        .form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 15px; }
        input, select, textarea { width: 100%; padding: 10px; border: 1px solid #ddd; border-radius: 5px; margin-top: 5px; }
        button { 
            padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; 
            font-weight: 600; transition: 0.3s; 
        }
        .btn-add { background: var(--success); color: white; }
        .btn-print { background: var(--primary); color: white; }
        .btn-danger { background: var(--danger); color: white; padding: 5px 10px; font-size: 12px; }

        /* Tables */
        table { width: 100%; border-collapse: collapse; margin-top: 15px; background: white; }
        table th { background: var(--secondary); color: white; padding: 12px; text-align: left; }
        table td { padding: 12px; border-bottom: 1px solid #eee; }

        /* Bulletin Design */
        #bulletin-print-zone { display: none; }
        @media print {
            body * { visibility: hidden; }
            #bulletin-print-zone, #bulletin-print-zone * { visibility: visible; }
            #bulletin-print-zone { position: absolute; left: 0; top: 0; width: 100%; display: block !important; padding: 40px; }
            .no-print { display: none !important; }
        }

        .bulletin-header { display: flex; justify-content: space-between; border-bottom: 2px solid #000; padding-bottom: 10px; }
        .moyenne-generale { font-size: 1.5em; font-weight: bold; text-align: right; margin-top: 20px; }
    </style>
</head>
<body>

    <aside class="sidebar">
        <div class="logo-area">
            <h2 id="side-school-name">SGE</h2>
            <p>Gestion Scolaire</p>
        </div>
        <nav class="nav-menu">
            <div class="nav-item active" onclick="showSection('dashboard')">🏠 Tableau de Bord</div>
            <div class="nav-item" onclick="showSection('params')">⚙️ Paramètres</div>
            <div class="nav-item" onclick="showSection('classes')">🏫 Classes</div>
            <div class="nav-item" onclick="showSection('matieres')">📚 Matières</div>
            <div class="nav-item" onclick="showSection('profs')">👨‍🏫 Enseignants</div>
            <div class="nav-item" onclick="showSection('eleves')">🎓 Élèves</div>
            <div class="nav-item" onclick="showSection('notes')">📝 Saisie des Notes</div>
            <div class="nav-item" onclick="showSection('bulletins')">📄 Bulletins</div>
        </nav>
    </aside>

    <main class="main-content">
        <header>
            <h2 id="current-title">Tableau de Bord</h2>
            <div id="header-info">Année Scolaire: <span id="display-annee">2025-2026</span></div>
        </header>

        <div class="container">
            
            <!-- Dashboard -->
            <section id="dashboard" class="section active">
                <div class="stats-grid">
                    <div class="stat-card"><h3>Élèves</h3><p id="count-eleves">0</p></div>
                    <div class="stat-card"><h3>Classes</h3><p id="count-classes">0</p></div>
                    <div class="stat-card"><h3>Profs</h3><p id="count-profs">0</p></div>
                    <div class="stat-card"><h3>Moyenne G.</h3><p>--/20</p></div>
                </div>
            </section>

            <!-- Paramètres -->
            <section id="params" class="section">
                <div class="card">
                    <h3>Informations de l'Établissement</h3>
                    <div class="form-grid">
                        <input type="text" id="p-nom" placeholder="Nom de l'école">
                        <input type="text" id="p-annee" placeholder="Année Scolaire (ex: 2025-2026)">
                        <input type="text" id="p-directeur" placeholder="Nom du Directeur">
                        <input type="text" id="p-adresse" placeholder="Adresse">
                        <input type="email" id="p-email" placeholder="Email">
                        <input type="text" id="p-devise" placeholder="Devise">
                    </div>
                    <button class="btn-add" onclick="saveParams()">Enregistrer les paramètres</button>
                </div>
            </section>

            <!-- Classes -->
            <section id="classes" class="section">
                <div class="card">
                    <h3>Ajouter une Classe</h3>
                    <div class="form-grid">
                        <input type="text" id="cl-nom" placeholder="Nom (ex: 3ème A)">
                        <select id="cl-niveau">
                            <option value="Collège">Collège</option>
                            <option value="Lycée">Lycée</option>
                        </select>
                        <button class="btn-add" onclick="addData('classes')">Ajouter</button>
                    </div>
                </div>
                <div class="card">
                    <table>
                        <thead><tr><th>Nom</th><th>Niveau</th><th>Action</th></tr></thead>
                        <tbody id="list-classes"></tbody>
                    </table>
                </div>
            </section>

            <!-- Matières -->
            <section id="matieres" class="section">
                <div class="card">
                    <h3>Ajouter une Matière</h3>
                    <div class="form-grid">
                        <input type="text" id="mat-nom" placeholder="Nom (ex: Mathématiques)">
                        <input type="number" id="mat-coef" placeholder="Coefficient" min="1">
                        <button class="btn-add" onclick="addData('matieres')">Ajouter</button>
                    </div>
                </div>
                <div class="card">
                    <table>
                        <thead><tr><th>Matière</th><th>Coefficient</th><th>Action</th></tr></thead>
                        <tbody id="list-matieres"></tbody>
                    </table>
                </div>
            </section>

            <!-- Profs -->
            <section id="profs" class="section">
                <div class="card">
                    <h3>Ajouter un Enseignant</h3>
                    <div class="form-grid">
                        <input type="text" id="prof-nom" placeholder="Nom complet">
                        <select id="prof-mat"></select>
                        <button class="btn-add" onclick="addData('profs')">Ajouter</button>
                    </div>
                </div>
                <div class="card">
                    <table>
                        <thead><tr><th>Nom</th><th>Matière</th><th>Action</th></tr></thead>
                        <tbody id="list-profs"></tbody>
                    </table>
                </div>
            </section>

            <!-- Élèves -->
            <section id="eleves" class="section">
                <div class="card">
                    <h3>Inscrire un Élève</h3>
                    <div class="form-grid">
                        <input type="text" id="el-nom" placeholder="Nom complet">
                        <select id="el-sexe"><option value="M">Masculin</option><option value="F">Féminin</option></select>
                        <select id="el-classe"></select>
                        <button class="btn-add" onclick="addData('eleves')">Inscrire</button>
                    </div>
                </div>
                <div class="card">
                    <table>
                        <thead><tr><th>Matricule</th><th>Nom</th><th>Classe</th><th>Action</th></tr></thead>
                        <tbody id="list-eleves"></tbody>
                    </table>
                </div>
            </section>

            <!-- Saisie des Notes -->
            <section id="notes" class="section">
                <div class="card">
                    <h3>Filtrer pour la saisie</h3>
                    <div class="form-grid">
                        <select id="note-classe" onchange="refreshNoteTable()"></select>
                        <select id="note-matiere" onchange="refreshNoteTable()"></select>
                    </div>
                </div>
                <div class="card">
                    <table>
                        <thead><tr><th>Élève</th><th>Note 1</th><th>Note 2</th><th>Note 3</th><th>Moyenne</th><th>Action</th></tr></thead>
                        <tbody id="list-saisie-notes"></tbody>
                    </table>
                </div>
            </section>

            <!-- Bulletins -->
            <section id="bulletins" class="section">
                <div class="card">
                    <h3>Générer les Bulletins</h3>
                    <select id="bull-classe" onchange="refreshBulletinList()"></select>
                </div>
                <div id="list-bulletins-eleves"></div>
            </section>

        </div>
    </main>

    <!-- Zone d'impression masquée -->
    <div id="bulletin-print-zone"></div>

    <script>
        // --- BASE DE DONNÉES LOCALE ---
        let db = JSON.parse(localStorage.getItem('sge_db')) || {
            params: { nom: "ÉCOLE EXEMPLE", annee: "2025-2026", directeur: "Jean Dupont", adresse: "Lomé, Togo", devise: "Discipline - Travail" },
            classes: [],
            matieres: [],
            profs: [],
            eleves: [],
            notes: {} // Format: { matricule: { matiere: [n1, n2, n3] } }
        };

        function saveDB() {
            localStorage.setItem('sge_db', JSON.stringify(db));
            updateUI();
        }

        // --- NAVIGATION ---
        function showSection(id) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            event.currentTarget.classList.add('active');
            document.getElementById('current-title').innerText = event.currentTarget.innerText;
            updateUI();
        }

        // --- LOGIQUE GÉNÉRIQUE ---
        function addData(type) {
            if (type === 'classes') {
                db.classes.push({ id: Date.now(), nom: document.getElementById('cl-nom').value, niveau: document.getElementById('cl-niveau').value });
            } else if (type === 'matieres') {
                db.matieres.push({ id: Date.now(), nom: document.getElementById('mat-nom').value, coef: document.getElementById('mat-coef').value });
            } else if (type === 'profs') {
                db.profs.push({ id: Date.now(), nom: document.getElementById('prof-nom').value, mat: document.getElementById('prof-mat').value });
            } else if (type === 'eleves') {
                let matricule = "MAT" + Math.floor(1000 + Math.random() * 9000);
                db.eleves.push({ matricule, nom: document.getElementById('el-nom').value, classe: document.getElementById('el-classe').value, sexe: document.getElementById('el-sexe').value });
            }
            saveDB();
        }

        function deleteData(type, index) {
            db[type].splice(index, 1);
            saveDB();
        }

        function saveParams() {
            db.params = {
                nom: document.getElementById('p-nom').value,
                annee: document.getElementById('p-annee').value,
                directeur: document.getElementById('p-directeur').value,
                adresse: document.getElementById('p-adresse').value,
                email: document.getElementById('p-email').value,
                devise: document.getElementById('p-devise').value
            };
            saveDB();
            alert("Paramètres enregistrés !");
        }

        // --- GESTION DES NOTES ---
        function saveNote(matricule) {
            let n1 = parseFloat(document.getElementById(`n1-${matricule}`).value) || 0;
            let n2 = parseFloat(document.getElementById(`n2-${matricule}`).value) || 0;
            let n3 = parseFloat(document.getElementById(`n3-${matricule}`).value) || 0;
            let matiere = document.getElementById('note-matiere').value;

            if(!db.notes[matricule]) db.notes[matricule] = {};
            db.notes[matricule][matiere] = [n1, n2, n3];
            
            let moy = ((n1 + n2 + n3) / 3).toFixed(2);
            document.getElementById(`moy-${matricule}`).innerText = moy;
            saveDB();
        }

        // --- MISE À JOUR DE L'INTERFACE ---
        function updateUI() {
            // Dashboard
            document.getElementById('count-eleves').innerText = db.eleves.length;
            document.getElementById('count-classes').innerText = db.classes.length;
            document.getElementById('count-profs').innerText = db.profs.length;
            document.getElementById('display-annee').innerText = db.params.annee;
            document.getElementById('side-school-name').innerText = db.params.nom;

            // Listes
            renderTable('list-classes', db.classes, (item, i) => `<td>${item.nom}</td><td>${item.niveau}</td><td><button class="btn-danger" onclick="deleteData('classes', ${i})">Supprimer</button></td>`);
            renderTable('list-matieres', db.matieres, (item, i) => `<td>${item.nom}</td><td>${item.coef}</td><td><button class="btn-danger" onclick="deleteData('matieres', ${i})">Supprimer</button></td>`);
            renderTable('list-profs', db.profs, (item, i) => `<td>${item.nom}</td><td>${item.mat}</td><td><button class="btn-danger" onclick="deleteData('profs', ${i})">Supprimer</button></td>`);
            renderTable('list-eleves', db.eleves, (item, i) => `<td>${item.matricule}</td><td>${item.nom}</td><td>${item.classe}</td><td><button class="btn-danger" onclick="deleteData('eleves', ${i})">Supprimer</button></td>`);

            // Remplissage des Selects
            fillSelect('prof-mat', db.matieres.map(m => m.nom));
            fillSelect('el-classe', db.classes.map(c => c.nom));
            fillSelect('note-classe', db.classes.map(c => c.nom));
            fillSelect('note-matiere', db.matieres.map(m => m.nom));
            fillSelect('bull-classe', db.classes.map(c => c.nom));

            // Champs paramètres
            document.getElementById('p-nom').value = db.params.nom;
            document.getElementById('p-annee').value = db.params.annee;
        }

        function renderTable(id, data, rowFn) {
            let tbody = document.getElementById(id);
            if(!tbody) return;
            tbody.innerHTML = data.map((item, i) => `<tr>${rowFn(item, i)}</tr>`).join('');
        }

        function fillSelect(id, list) {
            let select = document.getElementById(id);
            if(!select) return;
            let val = select.value;
            select.innerHTML = list.map(item => `<option value="${item}">${item}</option>`).join('');
            if(val) select.value = val;
        }

        function refreshNoteTable() {
            let classe = document.getElementById('note-classe').value;
            let matiere = document.getElementById('note-matiere').value;
            let eleves = db.eleves.filter(e => e.classe === classe);
            
            let html = eleves.map(e => {
                let notes = (db.notes[e.matricule] && db.notes[e.matricule][matiere]) ? db.notes[e.matricule][matiere] : [0,0,0];
                let moy = ((notes[0] + notes[1] + notes[2])/3).toFixed(2);
                return `
                    <tr>
                        <td>${e.nom}</td>
                        <td><input type="number" id="n1-${e.matricule}" value="${notes[0]}" step="0.25" onchange="saveNote('${e.matricule}')"></td>
                        <td><input type="number" id="n2-${e.matricule}" value="${notes[1]}" step="0.25" onchange="saveNote('${e.matricule}')"></td>
                        <td><input type="number" id="n3-${e.matricule}" value="${notes[2]}" step="0.25" onchange="saveNote('${e.matricule}')"></td>
                        <td id="moy-${e.matricule}">${moy}</td>
                        <td>✅</td>
                    </tr>
                `;
            }).join('');
            document.getElementById('list-saisie-notes').innerHTML = html;
        }

        function refreshBulletinList() {
            let classe = document.getElementById('bull-classe').value;
            let eleves = db.eleves.filter(e => e.classe === classe);
            let html = eleves.map(e => `
                <div class="card" style="display:flex; justify-content:space-between; align-items:center;">
                    <span>${e.nom} (${e.matricule})</span>
                    <button class="btn-print" onclick="printBulletin('${e.matricule}')">Générer Bulletin</button>
                </div>
            `).join('');
            document.getElementById('list-bulletins-eleves').innerHTML = html;
        }

        // --- GÉNÉRATION DU BULLETIN ---
        function printBulletin(matricule) {
            let eleve = db.eleves.find(e => e.matricule === matricule);
            let p = db.params;
            let html = `
                <div class="bulletin-header">
                    <div><h1>${p.nom}</h1><p>${p.adresse}</p><p>${p.email}</p></div>
                    <div style="text-align:right"><h2>BULLETIN DE NOTES</h2><p>Année: ${p.annee}</p><strong>${p.devise}</strong></div>
                </div>
                <div style="margin: 20px 0; border: 1px solid #000; padding: 10px;">
                    <p><strong>Nom & Prénoms :</strong> ${eleve.nom}</p>
                    <p><strong>Classe :</strong> ${eleve.classe} | <strong>Matricule :</strong> ${eleve.matricule}</p>
                </div>
                <table>
                    <thead>
                        <tr><th>Matières</th><th>Coef</th><th>Moy/20</th><th>Moy Coef</th><th>Appréciation</th></tr>
                    </thead>
                    <tbody>
            `;

            let totalPoints = 0;
            let totalCoef = 0;

            db.matieres.forEach(m => {
                let notes = (db.notes[matricule] && db.notes[matricule][m.nom]) ? db.notes[matricule][m.nom] : [0,0,0];
                let moy = (notes[0] + notes[1] + notes[2]) / 3;
                let moyCoef = moy * m.coef;
                totalPoints += moyCoef;
                totalCoef += parseInt(m.coef);

                html += `
                    <tr>
                        <td>${m.nom}</td>
                        <td>${m.coef}</td>
                        <td>${moy.toFixed(2)}</td>
                        <td>${moyCoef.toFixed(2)}</td>
                        <td>${moy >= 10 ? 'Bien' : 'Insuffisant'}</td>
                    </tr>
                `;
            });

            let moyGen = totalPoints / totalCoef;

            html += `
                    </tbody>
                </table>
                <div class="moyenne-generale">MOYENNE GÉNÉRALE : ${moyGen.toFixed(2)} / 20</div>
                <div style="margin-top:50px; display:flex; justify-content:space-between">
                    <div>L'Intendant</div>
                    <div>Le Parent</div>
                    <div>Le Directeur<br><br><strong>${p.directeur}</strong></div>
                </div>
            `;

            document.getElementById('bulletin-print-zone').innerHTML = html;
            window.print();
        }

        // Init
        window.onload = updateUI;
    </script>
</body>
</html>
