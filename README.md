        // Suite et fin du script SGE

        // --- GESTION DES ENSEIGNANTS ---
        function submitEnseignant(event) {
            event.preventDefault();
            const form = event.target;
            const matieres = Array.from(form.matieres.selectedOptions).map(opt => opt.value);
            const classes = Array.from(form.classes.selectedOptions).map(opt => opt.value);
            
            const enseignant = {
                nom: form.nomEnseignant.value,
                prenom: form.prenomEnseignant.value,
                telephone: form.telEnseignant.value,
                email: form.emailEnseignant.value,
                matieres: matieres,
                classes: classes
            };
            
            db.add('enseignants', enseignant);
            form.reset();
            updateAllTables();
            alert("Enseignant ajouté avec succès !");
        }

        // --- GESTION DES ÉLÈVES ---
        function submitEleve(event) {
            event.preventDefault();
            const form = event.target;
            const eleve = {
                matricule: 'MAT' + Date.now().toString().slice(-6),
                nom: form.nomEleve.value,
                prenom: form.prenomEleve.value,
                dateNaissance: form.dateNaiss.value,
                classe: form.classeEleve.value,
                sexe: form.sexeEleve.value,
                parent: form.parentEleve.value,
                telParent: form.telParent.value
            };
            
            db.add('eleves', eleve);
            form.reset();
            updateAllTables();
            updateStats();
            alert("Élève inscrit avec succès !");
        }

        // --- GESTION DES NOTES ---
        function chargerElevesPourNotes() {
            const classeSelect = document.getElementById('classeNoteSelect');
            const matiereSelect = document.getElementById('matiereNoteSelect');
            const container = document.getElementById('elevesNotesList');
            
            const eleves = db.get('eleves').filter(e => e.classe === classeSelect.value);
            
            container.innerHTML = eleves.map(eleve => `
                <div class="card">
                    <h4>${eleve.nom} ${eleve.prenom}</h4>
                    <div class="notes-grid">
                        <input type="number" step="0.25" placeholder="Note 1" id="n1_${eleve.matricule}">
                        <input type="number" step="0.25" placeholder="Note 2" id="n2_${eleve.matricule}">
                        <input type="number" step="0.25" placeholder="Examen" id="ex_${eleve.matricule}">
                    </div>
                    <button class="btn btn-primary" onclick="enregistrerNote('${eleve.matricule}')">Enregistrer</button>
                </div>
            `).join('');
        }

        function enregistrerNote(matricule) {
            const n1 = parseFloat(document.getElementById(`n1_${matricule}`).value) || 0;
            const n2 = parseFloat(document.getElementById(`n2_${matricule}`).value) || 0;
            const ex = parseFloat(document.getElementById(`ex_${matricule}`).value) || 0;
            const matiere = document.getElementById('matiereNoteSelect').value;
            const periode = document.getElementById('periodeNote').value;

            const notesDB = db.get('notes');
            if(!notesDB[matricule]) notesDB[matricule] = {};
            if(!notesDB[matricule][periode]) notesDB[matricule][periode] = {};
            
            notesDB[matricule][periode][matiere] = { n1, n2, ex, moyenne: (n1 + n2 + (ex * 2)) / 4 };
            
            db.set('notes', notesDB);
            alert("Note enregistrée !");
        }

        // --- GÉNÉRATION DE BULLETIN ---
        function genererBulletin() {
            const matricule = document.getElementById('bulletinSearch').value;
            const periode = document.getElementById('periodeBulletin').value;
            const eleve = db.get('eleves').find(e => e.matricule === matricule);
            const notes = db.get('notes')[matricule]?.[periode] || {};
            const params = db.get('parametres');

            if(!eleve) return alert("Élève non trouvé");

            let html = `
                <div class="bulletin-header">
                    <h2>${params.nomEtablissement}</h2>
                    <p>${params.devise}</p>
                    <hr>
                    <h3>BULLETIN DE NOTES - ${periode.toUpperCase()}</h3>
                </div>
                <div class="bulletin-info">
                    <div><strong>Nom:</strong> ${eleve.nom} ${eleve.prenom}</div>
                    <div><strong>Classe:</strong> ${eleve.classe}</div>
                    <div><strong>Année:</strong> ${params.anneeScolaire}</div>
                    <div><strong>Matricule:</strong> ${eleve.matricule}</div>
                </div>
                <table class="notes-table">
                    <thead>
                        <tr><th>Matière</th><th>Note 1</th><th>Note 2</th><th>Examen</th><th>Moyenne</th></tr>
                    </thead>
                    <tbody>
            `;

            let totalMoyennes = 0;
            let countMatieres = 0;

            for(let mat in notes) {
                const n = notes[mat];
                totalMoyennes += n.moyenne;
                countMatieres++;
                html += `
                    <tr>
                        <td>${mat}</td>
                        <td>${n.n1}</td>
                        <td>${n.n2}</td>
                        <td>${n.ex}</td>
                        <td><strong>${n.moyenne.toFixed(2)}</strong></td>
                    </tr>`;
            }

            const moyGen = countMatieres > 0 ? (totalMoyennes / countMatieres).toFixed(2) : "0.00";
            
            html += `</tbody></table>
                <div class="moyenne-display">MOYENNE GÉNÉRALE: ${moyGen} / 20</div>
                <div class="appreciation">Observations: ${moyGen >= 10 ? 'Admis(e)' : 'Insuffisant'}</div>
                <button class="btn btn-warning no-print" onclick="window.print()" style="margin-top:20px">Imprimer le Bulletin</button>
            `;

            document.getElementById('bulletinContainer').innerHTML = html;
        }

        // --- NAVIGATION ET MISE À JOUR ---
        function showSection(id) {
            document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.menu-item').forEach(m => m.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            updateAllTables();
        }

        function updateStats() {
            const eleves = db.get('eleves');
            const classes = db.get('classes');
            const enseignants = db.get('enseignants');
            
            if(document.getElementById('statEleves')) {
                document.getElementById('statEleves').innerText = eleves.length;
                document.getElementById('statClasses').innerText = classes.length;
                document.getElementById('statProfs').innerText = enseignants.length;
            }
        }

        function updateAllTables() {
            // Mise à jour des listes déroulantes et tableaux (classes, matières, etc.)
            const classes = db.get('classes');
            const matieres = db.get('matieres');
            
            // Exemple pour les selects de classes
            const classeSelects = ['classeEleve', 'classeNoteSelect'];
            classeSelects.forEach(id => {
                const el = document.getElementById(id);
                if(el) el.innerHTML = classes.map(c => `<option value="${c.nom}">${c.nom}</option>`).join('');
            });
            
            updateStats();
        }

        // Initialisation au chargement
        window.onload = () => {
            updateAllTables();
            updateStats();
        };
    </script>
</body>
</html>
