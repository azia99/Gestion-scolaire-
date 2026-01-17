<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestion Scolaire</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 0; }
        header { background: #4CAF50; color: white; padding: 15px; text-align: center; }
        nav { margin: 15px; }
        nav button { margin-right: 10px; }
        .container { padding: 15px; }
        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        table, th, td { border: 1px solid #ddd; }
        th, td { padding: 8px; text-align: left; }
        .hidden { display: none; }
    </style>
</head>
<body>

<header>
    <h1>Gestion de l'Établissement Scolaire</h1>
</header>

<nav>
    <button onclick="displaySection('params')">Paramètres</button>
    <button onclick="displaySection('classes')">Classes</button>
    <button onclick="displaySection('eleves')">Élèves</button>
    <button onclick="displaySection('enseignants')">Enseignants</button>
    <button onclick="displaySection('notes')">Notes</button>
    <button onclick="displaySection('bulletins')">Bulletins</button>
</nav>

<div class="container">

    <!-- Section Paramètres -->
    <div id="params" class="section hidden">
        <h2>Paramètres de l'établissement</h2>
        <form id="paramForm">
            <label for="schoolName">Nom de l'établissement:</label>
            <input type="text" id="schoolName" required /><br/>
            <label for="director">Directeur:</label>
            <input type="text" id="director" required /><br/>
            <label for="year">Année scolaire:</label>
            <input type="text" id="year" required /><br/>
            <button type="button" onclick="saveParameters()">Enregistrer</button>
        </form>
    </div>

    <!-- Section Classes -->
    <div id="classes" class="section hidden">
        <h2>Gestion des Classes</h2>
        <form id="classForm">
            <label for="className">Nom de la classe:</label>
            <input type="text" id="className" required />
            <button type="button" onclick="addClass()">Ajouter Classe</button>
        </form>
        <table>
            <thead>
                <tr>
                    <th>Classes ajoutées</th>
                </tr>
            </thead>
            <tbody id="classList"></tbody>
        </table>
    </div>

    <!-- Section Élèves -->
    <div id="eleves" class="section hidden">
        <h2>Gestion des Élèves</h2>
        <form id="studentForm">
            <label for="studentName">Nom de l'élève:</label>
            <input type="text" id="studentName" required /><br/>
            <label for="studentClass">Classe:</label>
            <select id="studentClass"></select><br/>
            <button type="button" onclick="addStudent()">Ajouter Élève</button>
        </form>
        <table>
            <thead>
                <tr>
                    <th>Élèves ajoutés</th>
                </tr>
            </thead>
            <tbody id="studentList"></tbody>
        </table>
    </div>

    <!-- Section Enseignants -->
    <div id="enseignants" class="section hidden">
        <h2>Gestion des Enseignants</h2>
        <form id="teacherForm">
            <label for="teacherName">Nom de l'enseignant:</label>
            <input type="text" id="teacherName" required /><br/>
            <label for="subject">Matière:</label>
            <input type="text" id="subject" required /><br/>
            <button type="button" onclick="addTeacher()">Ajouter Enseignant</button>
        </form>
        <table>
            <thead>
                <tr>
                    <th>Enseignants ajoutés</th>
                </tr>
            </thead>
            <tbody id="teacherList"></tbody>
        </table>
    </div>

    <!-- Section Notes -->
    <div id="notes" class="section hidden">
        <h2>Gestion des Notes</h2>
        <form id="noteForm">
            <label for="noteStudentSelect">Sélectionner Élève:</label>
            <select id="noteStudentSelect"></select><br/>
            <label for="noteSubject">Matière:</label>
            <input type="text" id="noteSubject" required /><br/>
            <label for="noteValue">Note:</label>
            <input type="number" id="noteValue" required />
            <button type="button" onclick="addNote()">Ajouter Note</button>
        </form>
        <table>
            <thead>
                <tr>
                    <th>Notes ajoutées</th>
                </tr>
            </thead>
            <tbody id="noteList"></tbody>
        </table>
    </div>

    <!-- Section Bulletins -->
    <div id="bulletins" class="section hidden">
        <h2>Bulletins Scolaires</h2>
        <button type="button" onclick="printBulletin()">Imprimer Bulletin (Fonctionnalité à implémenter)</button>
    </div>

</div>

<script>
    let classes = [];
    let students = [];
    let teachers = [];
    let notes = [];

    function displaySection(section) {
        const sections = document.querySelectorAll('.section');
        sections.forEach(s => s.classList.add('hidden'));
        document.getElementById(section).classList.remove('hidden');
    }

    function saveParameters() {
        // Placeholder pour sauvegarder les paramètres
        alert('Paramètres enregistrés: ' + document.getElementById('schoolName').value);
    }

    function addClass() {
        const className = document.getElementById('className').value;
        if (className) {
            classes.push(className);
            document.getElementById('classList').innerHTML += `<tr><td>${className}</td></tr>`;
            document.getElementById('studentClass').innerHTML += `<option>${className}</option>`;
            document.getElementById('className').value = '';
        }
    }

    function addStudent() {
        const studentName = document.getElementById('studentName').value;
        const studentClass = document.getElementById('studentClass').value;
        if (studentName) {
            students.push({ name: studentName, class: studentClass });
            document.getElementById('studentList').innerHTML += `<tr><td>${studentName} (${studentClass})</td></tr>`;
            document.getElementById('noteStudentSelect').innerHTML += `<option>${studentName}</option>`;
            document.getElementById('studentName').value = '';
        }
    }

    function addTeacher() {
        const teacherName = document.getElementById('teacherName').value;
        const subject = document.getElementById('subject').value;
        if (teacherName) {
            teachers.push({ name: teacherName, subject: subject });
            document.getElementById('teacherList').innerHTML += `<tr><td>${teacherName} (${subject})</td></tr>`;
            document.getElementById('teacherName').value = '';
            document.getElementById('subject').value = '';
        }
    }

    function addNote() {
        const studentName = document.getElementById('noteStudentSelect').value;
        const subject = document.getElementById('noteSubject').value;
        const noteValue = document.getElementById('noteValue').value;
        if (studentName && subject && noteValue) {
            notes.push({ student: studentName, subject: subject, value: noteValue });
            document.getElementById('noteList').innerHTML += `<tr><td>${studentName} - ${subject}: ${noteValue}</td></tr>`;
            document.getElementById('noteSubject').value = '';
            document.getElementById('noteValue').value = '';
        }
    }

    function printBulletin() {
        // Implémentation de l'impression du bulletin
        alert("Fonction d'impression des bulletins à implémenter.");
    }

    // Initial display
    displaySection('params');
</script>

</body>
</html>
