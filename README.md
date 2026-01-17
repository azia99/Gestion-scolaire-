<!DOCTYPE html>
<html lang="fr">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestion d'Établissement Scolaire</title>
    <style>
        /* Styles CSS de base */
        body {
            font-family: sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }

        .container {
            display: flex;
            height: 100vh;
        }

        .sidebar {
            width: 200px;
            background-color: #333;
            color: white;
            padding: 20px;
        }

        .sidebar ul {
            list-style: none;
            padding: 0;
        }

        .sidebar li {
            margin-bottom: 10px;
        }

        .sidebar a {
            color: white;
            text-decoration: none;
            display: block;
            padding: 5px;
        }

        .sidebar a:hover {
            background-color: #555;
        }

        .content {
            flex: 1;
            padding: 20px;
        }

        /* Styles pour les tableaux */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }

        th,
        td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }

        th {
            background-color: #f2f2f2;
        }

        /* Styles pour les formulaires */
        form {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 5px;
        }

        input[type="text"],
        input[type="number"],
        select {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            box-sizing: border-box;
        }

        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 15px;
            border: none;
            cursor: pointer;
        }

        button:hover {
            background-color: #3e8e41;
        }

        /* Styles spécifiques pour les sections */
        #parametres,
        #classes,
        #eleves,
        #enseignants,
        #matieres,
        #notes,
        #bulletins {
            display: none;
        }

        #parametres.active,
        #classes.active,
        #eleves.active,
        #enseignants.active,
        #matieres.active,
        #notes.active,
        #bulletins.active {
            display: block;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="sidebar">
            <h2>Menu</h2>
            <ul>
                <li><a href="#" onclick="showSection('parametres')">Paramètres</a></li>
                <li><a href="#" onclick="showSection('classes')">Classes</a></li>
                <li><a href="#" onclick="showSection('eleves')">Élèves</a></li>
                <li><a href="#" onclick="showSection('enseignants')">Enseignants</a></li>
                <li><a href="#" onclick="showSection('matieres')">Matières</a></li>
                <li><a href="#" onclick="showSection('notes')">Notes</a></li>
                <li><a href="#" onclick="showSection('bulletins')">Bulletins</a></li>
                <li><a href="#" onclick="printBulletin()">Impression</a></li>
            </ul>
        </div>

        <div class="content">
            <!-- Sections de contenu -->
            <section id="parametres">
                <h2>Paramètres de l'Établissement</h2>
                <form id="formParametres">
                    <label for="nomEtablissement">Nom de l'établissement:</label>
                    <input type="text" id="nomEtablissement" name="nomEtablissement">

                    <label for="logoEtablissement">Logo de l'établissement:</label>
                    <input type="text" id="logoEtablissement" name="logoEtablissement">

                    <label for="adresseEtablissement">Adresse:</label>
                    <input type="text" id="adresseEtablissement" name="adresseEtablissement">

                    <label for="telephoneEtablissement">Téléphone:</label>
                    <input type="text" id="telephoneEtablissement" name="telephoneEtablissement">

                    <label for="emailEtablissement">Email:</label>
                    <input type="text" id="emailEtablissement" name="emailEtablissement">

                    <label for="anneeScolaire">Année Scolaire (ex: 2025-2026):</label>
                    <input type="text" id="anneeScolaire" name="anneeScolaire">

                    <label for="directeurGeneral">Directeur Général:</label>
                    <input type="text" id="directeurGeneral" name="directeurGeneral">

                    <label for="censeurProviseur">Censeur / Proviseur:</label>
                    <input type="text" id="censeurProviseur" name="censeurProviseur">

                    <label for="intendantEconome">Intendant / Econome:</label>
                    <input type="text" id="intendantEconome" name="intendantEconome">

                    <label for="deviseSlogan">Devise ou Slogan:</label>
                    <input type="text" id="deviseSlogan" name="deviseSlogan">

                    <button type="button" onclick="saveParameters()">Enregistrer</button>
                </form>
            </section>

            <section id="classes">
                <h2>Gestion des Classes</h2>
                <button onclick="ajouterClasse()">Ajouter Classe</button>
                <table>
                    <thead>
                        <tr>
                            <th>Nom</th>
                            <th>Série</th>
                            <th>Effectif Max</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="listeClasses">
                        <!-- Les classes seront ajoutées ici par JavaScript -->
                    </tbody>
                </table>
            </section>

            <section id="eleves">
                <h2>Gestion des Élèves</h2>
                <button onclick="ajouterEleve()">Ajouter Élève</button>
                <table id="eleveTable">
                    <thead>
                        <tr>
                            <th>Matricule</th>
                            <th>Nom</th>
                            <th>Prénom</th>
                            <th>Classe</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="listeEleves">
                        <!-- Les élèves seront ajoutés ici par JavaScript -->
                    </tbody>
                </table>
            </section>

            <section id="enseignants">
                <h2>Gestion des Enseignants</h2>
                <button onclick="ajouterEnseignant()">Ajouter Enseignant</button>
                <table id="enseignantTable">
                    <thead>
                        <tr>
                            <th>Nom</th>
                            <th>Prénom</th>
                            <th>Matière</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="listeEnseignants">
                        <!-- Les enseignants seront ajoutés ici par JavaScript -->
                    </tbody>
                </table>
            </section>

            <section id="matieres">
                <h2>Gestion des Matières</h2>
                <button onclick="ajouterMatiere()">Ajouter Matière</button>
                <table>
                    <thead>
                        <tr>
                            <th>Nom</th>
                            <th>Coefficient</th>
                            <th>Classe</th>
                            <th>Enseignant</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="listeMatieres">
                        <!-- Les matières seront ajoutées ici par JavaScript -->
                    </tbody>
                </table>
            </section>

            <section id="notes">
                <h2>Saisie des Notes</h2>
                <label for="classeNotes">Sélectionner la classe:</label>
                <select id="classeNotes" onchange="afficherElevesPourNotes()">
                    <!-- Les options de classe seront ajoutées ici par JavaScript -->
                </select>
                <table id="tableNotes">
                    <thead>
                        <tr>
                            <th>Nom de l'élève</th>
                            <th>Matière</th>
                            <th>Note 1</th>
                            <th>Note 2</th>
                            <th>Note 3</th>
                            <th>Moyenne</th>
                        </tr>
                    </thead>
                    <tbody id="listeNotes">
                        <!-- Les notes seront ajoutées ici par JavaScript -->
                    </tbody>
                </table>
            </section>

            <section id="bulletins">
                <h2>Bulletins Scolaires</h2>
                <label for="classeBulletins">Sélectionner la classe:</label>
                <select id="classeBulletins" onchange="afficherElevesPourBulletins()">
                    <!-- Les options de classe seront ajoutées ici par JavaScript -->
                </select>
                <table id="tableBulletins">
                    <thead>
                        <tr>
                            <th>Nom de l'élève</th>
                            <th>Moyenne Générale</th>
                            <th>Rang</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="listeBulletins">
                        <!-- Les bulletins seront ajoutés ici par JavaScript -->
                    </tbody>
                </table>
            </section>
        </div>
    </div>

    <script>
        // Fonctions JavaScript pour la logique de l'application
        let etablissement = {
            parametres: {
                nomEtablissement: "",
                logoEtablissement: "",
                adresseEtablissement: "",
                telephoneEtablissement: "",
                emailEtablissement: "",
                anneeScolaire: "",
                directeurGeneral: "",
                censeurProviseur: "",
                intendantEconome: "",
                deviseSlogan: ""
            },
            classes: [],
            eleves: [],
            enseignants: [],
            matieres: [],
            notes: []
        };

        // Fonction pour afficher une section et masquer les autres
        function showSection(sectionId) {
            const sections = document.querySelectorAll('section');
            sections.forEach(section => section.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
        }

        // Fonction pour enregistrer les paramètres de l'établissement
        function saveParameters() {
            etablissement.parametres.nomEtablissement = document.getElementById("nomEtablissement").value;
            etablissement.parametres.logoEtablissement = document.getElementById("logoEtablissement").value;
            etablissement.parametres.adresseEtablissement = document.getElementById("adresseEtablissement").value;
            etablissement.parametres.telephoneEtablissement = document.getElementById("telephoneEtablissement").value;
            etablissement.parametres.emailEtablissement = document.getElementById("emailEtablissement").value;
            etablissement.parametres.anneeScolaire = document.getElementById("anneeScolaire").value;
            etablissement.parametres.directeurGeneral = document.getElementById("directeurGeneral").value;
            etablissement.parametres.censeurProviseur = document.getElementById("censeurProviseur").value;
            etablissement.parametres.intendantEconome = document.getElementById("intendantEconome").value;
            etablissement.parametres.deviseSlogan = document.getElementById("deviseSlogan").value;

            alert("Paramètres enregistrés!");
        }

        //Fonction pour ajouter une class
        function ajouterClasse() {
            let nomClasse = prompt("Nom de la classe:");
            if (nomClasse) {
                let serieClasse = prompt("Série (A, C, D, etc.):");
                let effectifMax = prompt("Effectif maximum:");

                let nouvelleClasse = {
                    nom: nomClasse,
                    serie: serieClasse,
                    effectifMax: effectifMax
                };

                etablissement.classes.push(nouvelleClasse);
                afficherClasses();
            }
        }

        // Fonction pour afficher les classes dans le tableau
        function afficherClasses() {
            let listeClasses = document.getElementById("listeClasses");
            listeClasses.innerHTML = "";

            etablissement.classes.forEach((classe, index) => {
                let row = listeClasses.insertRow();
                let cellNom = row.insertCell(0);
                let cellSerie = row.insertCell(1);
                let cellEffectifMax = row.insertCell(2);
                let cellActions = row.insertCell(3);

                cellNom.innerHTML = classe.nom;
                cellSerie.innerHTML = classe.serie;
                cellEffectifMax.innerHTML = classe.effectifMax;
                cellActions.innerHTML = `<button onclick="supprimerClasse(${index})">Supprimer</button>`;
            });
        }

        //Fonction pour supprimer une class
        function supprimerClasse(index) {
            etablissement.classes.splice(index, 1);
            afficherClasses();
        }

        // Fonction pour ajouter un élève
        function ajouterEleve() {
            let matricule = prompt("Numéro matricule:");
            if (matricule) {
                let nom = prompt("Nom:");
                let prenom = prompt("Prénom:");
                let classe = prompt("Classe:");

                let nouvelEleve = {
                    matricule: matricule,
                    nom: nom,
                    prenom: prenom,
                    classe: classe
                };

                etablissement.eleves.push(nouvelEleve);
                afficherEleves();
            }
        }

        // Fonction pour afficher les élèves dans le tableau
        function afficherEleves() {
            let listeEleves = document.getElementById("listeEleves");
            listeEleves.innerHTML = "";

            etablissement.eleves.forEach((eleve, index) => {
                let row = listeEleves.insertRow();
                let cellMatricule = row.insertCell(0);
                let cellNom = row.insertCell(1);
                let cellPrenom = row.insertCell(2);
                let cellClasse = row.insertCell(3);
                let cellActions = row.insertCell(4);

                cellMatricule.innerHTML = eleve.matricule;
                cellNom.innerHTML = eleve.nom;
                cellPrenom.innerHTML = eleve.prenom;
                cellClasse.innerHTML = eleve.classe;
                cellActions.innerHTML = `<button onclick="supprimerEleve(${index})">Supprimer</button>`;
            });
        }

        // Fonction pour supprimer un élève
        function supprimerEleve(index) {
            etablissement.eleves.splice(index, 1);
            afficherEleves();
        }

        // Fonction pour ajouter un enseignant
        function ajouterEnseignant() {
            let nom = prompt("Nom de l'enseignant:");
            if (nom) {
                let prenom = prompt("Prénom de l'enseignant:");
                let matiere = prompt("Matière enseignée:");

                let nouvelEnseignant = {
                    nom: nom,
                    prenom: prenom,
                    matiere: matiere
                };

                etablissement.enseignants.push(nouvelEnseignant);
                afficherEnseignants();
            }
        }

        // Fonction pour afficher les enseignants dans le tableau
        function afficherEnseignants() {
            let listeEnseignants = document.getElementById("listeEnseignants");
            listeEnseignants.innerHTML = "";

            etablissement.enseignants.forEach((enseignant, index) => {
                let row = listeEnseignants.insertRow();
                let cellNom = row.insertCell(0);
                let cellPrenom = row.insertCell(1);
                let cellMatiere = row.insertCell(2);
                let cellActions = row.insertCell(3);

                cellNom.innerHTML = enseignant.nom;
                cellPrenom.innerHTML = enseignant.prenom;
                cellMatiere.innerHTML = enseignant.matiere;
                cellActions.innerHTML = `<button onclick="supprimerEnseignant(${index})">Supprimer</button>`;
            });
        }

        // Fonction pour supprimer un enseignant
        function supprimerEnseignant(index) {
            etablissement.enseignants.splice(index, 1);
            afficherEnseignants();
        }

        // Fonction pour ajouter une matière
        function ajouterMatiere() {
            let nom = prompt("Nom de la matière:");
            if (nom) {
                let coefficient = prompt("Coefficient de la matière:");
                let classe = prompt("Classe concernée:");
                let enseignant = prompt("Enseignant responsable:");

                let nouvelleMatiere = {
                    nom: nom,
                    coefficient: coefficient,
                    classe: classe,
                    enseignant: enseignant
                };

                etablissement.matieres.push(nouvelleMatiere);
                afficherMatieres();
            }
        }

        // Fonction pour afficher les matières dans le tableau
        function afficherMatieres() {
            let listeMatieres = document.getElementById("listeMatieres");
            listeMatieres.innerHTML = "";

            etablissement.matieres.forEach((matiere, index) => {
                let row = listeMatieres.insertRow();
                let cellNom = row.insertCell(0);
                let cellCoefficient = row.insertCell(1);
                let cellClasse = row.insertCell(2);
                let cellEnseignant = row.insertCell(3);
                let cellActions = row.insertCell(4);

                cellNom.innerHTML = matiere.nom;
                cellCoefficient.innerHTML = matiere.coefficient;
                cellClasse.innerHTML = matiere.classe;
                cellEnseignant.innerHTML = matiere.enseignant;
                cellActions.innerHTML = `<button onclick="supprimerMatiere(${index})">Supprimer</button>`;
            });
        }

        // Fonction pour supprimer une matière
        function supprimerMatiere(index) {
            etablissement.matieres.splice(index, 1);
            afficherMatieres();
        }

        // Fonction pour afficher les élèves pour la saisie des notes
        function afficherElevesPourNotes() {
            let classeSelectionnee = document.getElementById("classeNotes").value;
            let listeNotes = document.getElementById("listeNotes");
            listeNotes.innerHTML = "";

            // Filtrer les élèves par la classe sélectionnée
            let elevesDeLaClasse = etablissement.eleves.filter(eleve => eleve.classe === classeSelectionnee);

            elevesDeLaClasse.forEach(eleve => {
                // Pour chaque matière, afficher une ligne avec les notes
                etablissement.matieres.forEach(matiere => {
                    let row = listeNotes.insertRow();
                    let cellNomEleve = row.insertCell(0);
                    let cellMatiere = row.insertCell(1);
                    let cellNote1 = row.insertCell(2);
                    let cellNote2 = row.insertCell(3);
                    let cellNote3 = row.insertCell(4);
                    let cellMoyenne = row.insertCell(5);

                    cellNomEleve.innerHTML = eleve.nom + " " + eleve.prenom;
                    cellMatiere.innerHTML = matiere.nom;

                    // Créer des champs de saisie pour les notes
                    cellNote1.innerHTML = `<input type="number" onchange="calculerMoyenne(this)">`;
                    cellNote2.innerHTML = `<input type="number" onchange="calculerMoyenne(this)">`;
                    cellNote3.innerHTML = `<input type="number" onchange="calculerMoyenne(this)">`;
                    cellMoyenne.innerHTML = "0"; // Valeur par défaut
                });
            });
        }

        //Fonction pour calculer la moyenne
        function calculerMoyenne(input) {
            let row = input.parentNode.parentNode;
            let note1 = parseFloat(row.cells[2].firstChild.value) || 0;
            let note2 = parseFloat(row.cells[3].firstChild.value) || 0;
            let note3 = parseFloat(row.cells[4].firstChild.value) || 0;

            let moyenne = (note1 + note2 + note3) / 3;
            row.cells[5].innerHTML = moyenne.toFixed(2); // Afficher la moyenne avec 2 décimales
        }

        // Fonction pour afficher les élèves pour les bulletins
        function afficherElevesPourBulletins() {
            let classeSelectionnee = document.getElementById("classeBulletins").value;
            let listeBulletins = document.getElementById("listeBulletins");
            listeBulletins.innerHTML = "";

            let elevesDe
