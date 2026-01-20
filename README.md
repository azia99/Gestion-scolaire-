ap(c => `
                <tr>
                    <td>${c.id}</td>
                    <td><strong>${c.matricule}</strong></td>
                    <td>${c.nom} ${c.prenoms}</td>
                    <td>${c.sexe}</td>
                    <td>${c.classe}</td>
                    <td>${c.tel}</td>
                </tr>
            `).join('');

            document.getElementById('content').innerHTML = `
                <h2 class="page-title">👨‍🎓 GESTION DES ÉLÈVES</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="openAddEleve()">➕ Nouvel Élève</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead>
                            <tr><th>ID</th><th>Matricule</th><th>Nom</th><th>Sexe</th><th>Classe</th><th>Téléphone</th></tr>
                        </thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function openAddEleve() {
            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvel Élève</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <form onsubmit="saveEleve(event)">
                    <div class="form-group">
                        <label>Matricule :</label>
                        <input type="text" id="matricule" required>
                    </div>
                    <div class="form-group">
                        <label>Nom :</label>
                        <input type="text" id="nomE" required>
                    </div>
                    <div class="form-group">
                        <label>Prénoms :</label>
                        <input type="text" id="prenoms" required>
                    </div>
                    <div class="form-group">
                        <label>Sexe :</label>
                        <select id="sexe" required>
                            <option value="">Sélectionner</option>
                            <option value="M">Masculin</option>
                            <option value="F">Féminin</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Classe :</label>
                        <input type="text" id="classe" required>
                    </div>
                    <div class="form-group">
                        <label>Téléphone :</label>
                        <input type="text" id="tel" required>
                    </div>
                    <div class="action-buttons">
                        <button type="submit" class="btn btn-success">✅ Enregistrer</button>
                        <button type="button" class="btn btn-danger" onclick="closeModal()">❌ Annuler</button>
                    </div>
                </form>
            `);
        }

        function saveEleve(e) {
            e.preventDefault();
            const newId = Math.max(...APP_DATA.eleves.map(e => e.id), 0) + 1;
            APP_DATA.eleves.push({
                id: newId,
                matricule: document.getElementById('matricule').value,
                nom: document.getElementById('nomE').value,
                prenoms: document.getElementById('prenoms').value,
                sexe: document.getElementById('sexe').value,
                classe: document.getElementById('classe').value,
                tel: document.getElementById('tel').value
            });
            closeModal();
            renderEleves();
            alert('✅ Élève ajouté avec succès !');
        }

        // Gestion des enseignants
        function renderEnseignants() {
            const rows = APP_DATA.enseignants.map(e => `
                <tr>
                    <td>${e.id}</td>
                    <td>${e.nom} ${e.prenoms}</td>
                    <td>${e.specialite}</td>
                    <td>${e.tel}</td>
                    <td>${e.email}</td>
                </tr>
            `).join('');

            document.getElementById('content').innerHTML = `
                <h2 class="page-title">👨‍🏫 GESTION DES ENSEIGNANTS</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="openAddEnseignant()">➕ Nouvel Enseignant</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead>
                            <tr><th>ID</th><th>Nom complet</th><th>Spécialité</th><th>Téléphone</th><th>Email</th></tr>
                        </thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function openAddEnseignant() {
            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvel Enseignant</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <form onsubmit="saveEnseignant(event)">
                    <div class="form-group">
                        <label>Nom :</label>
                        <input type="text" id="nomEns" required>
                    </div>
                    <div class="form-group">
                        <label>Prénoms :</label>
                        <input type="text" id="prenomsEns" required>
                    </div>
                    <div class="form-group">
                        <label>Spécialité :</label>
                        <input type="text" id="specialite" required>
                    </div>
                    <div class="form-group">
                        <label>Téléphone :</label>
                        <input type="text" id="telEns" required>
                    </div>
                    <div class="form-group">
                        <label>Email :</label>
                        <input type="email" id="emailEns" required>
                    </div>
                    <div class="action-buttons">
                        <button type="submit" class="btn btn-success">✅ Enregistrer</button>
                        <button type="button" class="btn btn-danger" onclick="closeModal()">❌ Annuler</button>
                    </div>
                </form>
            `);
        }

        function saveEnseignant(e) {
            e.preventDefault();
            const newId = Math.max(...APP_DATA.enseignants.map(e => e.id), 0) + 1;
            APP_DATA.enseignants.push({
                id: newId,
                nom: document.getElementById('nomEns').value,
                prenoms: document.getElementById('prenomsEns').value,
                specialite: document.getElementById('specialite').value,
                tel: document.getElementById('telEns').value,
                email: document.getElementById('emailEns').value
            });
            closeModal();
            renderEnseignants();
            alert('✅ Enseignant ajouté avec succès !');
        }

        // Gestion des matières
        function renderMatieres() {
            const rows = APP_DATA.matieres.map(m => `
                <tr>
                    <td>${m.id}</td>
                    <td>${m.nom}</td>
                    <td>${m.coef}</td>
                    <td>${m.desc}</td>
                </tr>
            `).join('');

            document.getElementById('content').innerHTML = `
                <h2 class="page-title">📚 GESTION DES MATIÈRES</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="openAddMatiere()">➕ Nouvelle Matière</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead>
                            <tr><th>ID</th><th>Nom</th><th>Coefficient</th><th>Description</th></tr>
                        </thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function openAddMatiere() {
            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvelle Matière</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <form onsubmit="saveMatiere(event)">
                    <div class="form-group">
                        <label>Nom de la matière :</label>
                        <input type="text" id="nomMatiere" required>
                    </div>
                    <div class="form-group">
                        <label>Coefficient :</label>
                        <input type="number" id="coef" required>
                    </div>
                    <div class="form-group">
                        <label>Description :</label>
                        <input type="text" id="desc" required>
                    </div>
                    <div class="action-buttons">
                        <button type="submit" class="btn btn-success">✅ Enregistrer</button>
                        <button type="button" class="btn btn-danger" onclick="closeModal()">❌ Annuler</button>
                    </div>
                </form>
            `);
        }

        function saveMatiere(e) {
            e.preventDefault();
            const newId = Math.max(...APP_DATA.matieres.map(m => m.id), 0) + 1;
            APP_DATA.matieres.push({
                id: newId,
                nom: document.getElementById('nomMatiere').value,
                coef: parseInt(document.getElementById('coef').value),
                desc: document.getElementById('desc').value
            });
            closeModal();
            renderMatieres();
            alert('✅ Matière ajoutée avec succès !');
        }

        // Gestion des notes
        function renderNotes() {
            const rows = APP_DATA.notes.map(n => `
                <tr>
                    <td>${n.id}</td>
                    <td>${n.eleve}</td>
                    <td>${n.matiere}</td>
                    <td>${n.note}</td>
                    <td>${n.type}</td>
                    <td>${n.date}</td>
                </tr>
            `).join('');

            document.getElementById('content').innerHTML = `
                <h2 class="page-title">📝 GESTION DES NOTES</h2>
                <div class="action-buttons">
                    <button class="btn btn-success" onclick="openAddNote()">➕ Nouvelle Note</button>
                </div>
                <div class="data-table">
                    <table>
                        <thead>
                            <tr><th>ID</th><th>Élève</th><th>Matière</th><th>Note</th><th>Type</th><th>Date</th></tr>
                        </thead>
                        <tbody>${rows}</tbody>
                    </table>
                </div>
            `;
        }

        function openAddNote() {
            showModal(`
                <div class="modal-header">
                    <h2>➕ Nouvelle Note</h2>
                    <button class="close-btn" onclick="closeModal()">×</button>
                </div>
                <form onsubmit="saveNote(event)">
                    <div class="form-group">
                        <label>Élève :</label>
                        <input type="text" id="eleveNote" placeholder="Nom complet" required>
                    </div>
                    <div class="form-group">
                        <label>Matière :</label>
                        <input type="text" id="matiereNote" placeholder="Nom de la matière" required>
                    </div>
                    <div class="form-group">
                        <label>Note :</label>
                        <input type="number" id="note" required>
                    </div>
                    <div class="form-group">
                        <label>Type :</label>
                        <input type="text" id="type" placeholder="Devoir, Examen, etc." required>
                    </div>
                    <div class="form-group">
                        <label>Date :</label>
                        <input type="date" id="dateNote" required>
                    </div>
                    <div class="action-buttons">
                        <button type="submit" class="btn btn-success">✅ Enregistrer</button>
                        <button type="button" class="btn btn-danger" onclick="closeModal()">❌ Annuler</button>
                    </div>
                </form>
            `);
        }

        function saveNote(e) {
            e.preventDefault();
            const newId = Math.max(...APP_DATA.notes.map(n => n.id), 0) + 1;
            APP_DATA.notes.push({
                id: newId,
                eleve: document.getElementById('eleveNote').value,
                matiere: document.getElementById('matiereNote').value,
                note: parseFloat(document.getElementById('note').value),
                type: document.getElementById('type').value,
                date: document.getElementById('dateNote').value
            });
            closeModal();
            renderNotes();
            alert('✅ Note ajoutée avec succès !');
        }

        // Gestion des bulletins
        function renderBulletins() {
            document.getElementById('content').innerHTML = `
                <h2 class="page-title">📋 GESTION DES BULLETINS</h2>
                <p>Fonctionnalité à venir...</p>
            `;
        }

        // Statistiques
        function renderStatistiques() {
            document.getElementById('content').innerHTML = `
                <h2 class="page-title">📈 STATISTIQUES</h2>
                <p>Fonctionnalité à venir...</p>
            `;
        }

        // Paramètres
        function renderParametres() {
            document.getElementById('content').innerHTML = `
                <h2 class="page-title">⚙️ PARAMÈTRES</h2>
                <div class="info-section">
                    <h3>Paramètres de l'établissement</h3>
                    <form onsubmit="saveParameters(event)">
                        <div class="form-group">
                            <label>Nom de l'établissement :</label>
                            <input type="text" id="etablissement" value="${APP_DATA.settings.etablissement}" required>
                        </div>
                        <div class="form-group">
                            <label>Année académique :</label>
                            <input type="text" id="annee" value="${APP_DATA.settings.annee}" required>
                        </div>
                        <div class="form-group">
                            <label>Trimestre :</label>
                            <select id="trimestre" required>
                                <option value="Trimestre 1" ${APP_DATA.settings.trimestre === 'Trimestre 1' ? 'selected' : ''}>Trimestre 1</option>
                                <option value="Trimestre 2" ${APP_DATA.settings.trimestre === 'Trimestre 2' ? 'selected' : ''}>Trimestre 2</option>
                                <option value="Trimestre 3" ${APP_DATA.settings.trimestre === 'Trimestre 3' ? 'selected' : ''}>Trimestre 3</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>Modifier Identifiant :</label>
                            <input type="text" id="newUsername" value="${APP_DATA.auth.username}" required>
                        </div>
                        <div class="form-group">
                            <label>Modifier Mot de passe :</label>
                            <input type="password" id="newPassword" required>
                        </div>
                        <div class="action-buttons">
                            <button type="submit" class="btn btn-success">✅ Enregistrer Modifications</button>
                        </div>
                    </form>
                </div>
            `;
        }

        function saveParameters(e) {
            e.preventDefault();
            APP_DATA.settings.etablissement = document.getElementById('etablissement').value;
            APP_DATA.settings.annee = document.getElementById('annee').value;
            APP_DATA.settings.trimestre = document.getElementById('trimestre').value;
            APP_DATA.auth.username = document.getElementById('newUsername').value;
            APP_DATA.auth.password = document.getElementById('newPassword').value;

            updateHeader();
            closeModal(); // Close modal if open
            renderParametres(); // Re-render the parameters to show updated info
            alert('✅ Paramètres mis à jour avec succès !');
        }

        // Modal functions
        function showModal(content) {
            document.getElementById('modalContent').innerHTML = content;
            document.getElementById('modal').classList.add('active');
        }

        function closeModal() {
            document.getElementById('modal').classList.remove('active');
        }
    </script>
</body>
</html>
