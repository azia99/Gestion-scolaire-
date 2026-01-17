import React, { useState, useEffect } from 'react';

const SGE = () => {
  const [currentSection, setCurrentSection] = useState('dashboard');
  const [activeModal, setActiveModal] = useState(null);
  const [parametres, setParametres] = useState({
    nomEtablissement: 'Établissement Scolaire',
    anneeScolaire: '2025-2026',
    adresse: '',
    telephone: '',
    email: '',
    directeur: '',
    censeur: '',
    devise: 'Excellence et Discipline'
  });
  const [classes, setClasses] = useState([]);
  const [matieres, setMatieres] = useState([]);
  const [enseignants, setEnseignants] = useState([]);
  const [eleves, setEleves] = useState([]);
  const [notes, setNotes] = useState([]);
  const [filterClasse, setFilterClasse] = useState('');
  const [searchEleve, setSearchEleve] = useState('');
  const [noteClasse, setNoteClasse] = useState('');
  const [noteMatiere, setNoteMatiere] = useState('');
  const [bulletinClasse, setBulletinClasse] = useState('');
  const [bulletinEleve, setBulletinEleve] = useState('');
  const [bulletinData, setBulletinData] = useState(null);

  useEffect(() => {
    const now = new Date();
    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    document.getElementById('currentDate').textContent = now.toLocaleDateString('fr-FR', options);
  }, []);

  const generateMatricule = () => {
    const year = new Date().getFullYear();
    const random = Math.floor(Math.random() * 10000).toString().padStart(4, '0');
    return `${year}${random}`;
  };

  const handleParametresSubmit = (e) => {
    e.preventDefault();
    const formData = {
      nomEtablissement: e.target.nomEtablissement.value,
      anneeScolaire: e.target.anneeScolaire.value,
      adresse: e.target.adresse.value,
      telephone: e.target.telephone.value,
      email: e.target.email.value,
      directeur: e.target.directeur.value,
      censeur: e.target.censeur.value,
      devise: e.target.devise.value
    };
    setParametres(formData);
    alert('✅ Paramètres enregistrés avec succès !');
  };

  const handleClasseSubmit = (e) => {
    e.preventDefault();
    const newClasse = {
      id: Date.now(),
      niveau: e.target.classeNiveau.value,
      serie: e.target.classeSerie.value,
      effectifMax: parseInt(e.target.classeEffectifMax.value),
      effectifActuel: 0
    };
    setClasses([...classes, newClasse]);
    setActiveModal(null);
    e.target.reset();
    alert('✅ Classe ajoutée avec succès !');
  };

  const handleMatiereSubmit = (e) => {
    e.preventDefault();
    const selectedClasses = Array.from(e.target.matiereClasses.selectedOptions).map(opt => opt.value);
    const newMatiere = {
      id: Date.now(),
      nom: e.target.matiereNom.value,
      coefficient: parseFloat(e.target.matiereCoef.value),
      classes: selectedClasses
    };
    setMatieres([...matieres, newMatiere]);
    setActiveModal(null);
    e.target.reset();
    alert('✅ Matière ajoutée avec succès !');
  };

  const handleEnseignantSubmit = (e) => {
    e.preventDefault();
    const newEnseignant = {
      id: Date.now(),
      nom: e.target.enseignantNom.value,
      sexe: e.target.enseignantSexe.value,
      matiere: e.target.enseignantMatiere.value,
      contact: e.target.enseignantContact.value,
      statut: e.target.enseignantStatut.value
    };
    setEnseignants([...enseignants, newEnseignant]);
    setActiveModal(null);
    e.target.reset();
    alert('✅ Enseignant ajouté avec succès !');
  };

  const handleEleveSubmit = (e) => {
    e.preventDefault();
    const classeId = e.target.eleveClasse.value;
    const classe = classes.find(c => c.id === parseInt(classeId));
    
    if (classe && classe.effectifActuel >= classe.effectifMax) {
      alert('❌ Cette classe a atteint son effectif maximum !');
      return;
    }

    const newEleve = {
      id: Date.now(),
      matricule: generateMatricule(),
      nom: e.target.eleveNom.value,
      sexe: e.target.eleveSexe.value,
      dateNaissance: e.target.eleveDateNaissance.value,
      classe: classeId,
      parent: e.target.eleveParent.value,
      contactParent: e.target.eleveContactParent.value
    };
    
    setEleves([...eleves, newEleve]);
    setClasses(classes.map(c => 
      c.id === parseInt(classeId) 
        ? { ...c, effectifActuel: c.effectifActuel + 1 }
        : c
    ));
    setActiveModal(null);
    e.target.reset();
    alert('✅ Élève ajouté avec succès !');
  };

  const deleteClasse = (id) => {
    if (confirm('Êtes-vous sûr de vouloir supprimer cette classe ?')) {
      setClasses(classes.filter(c => c.id !== id));
    }
  };

  const deleteMatiere = (id) => {
    if (confirm('Êtes-vous sûr de vouloir supprimer cette matière ?')) {
      setMatieres(matieres.filter(m => m.id !== id));
    }
  };

  const deleteEnseignant = (id) => {
    if (confirm('Êtes-vous sûr de vouloir supprimer cet enseignant ?')) {
      setEnseignants(enseignants.filter(e => e.id !== id));
    }
  };

  const deleteEleve = (id) => {
    if (confirm('Êtes-vous sûr de vouloir supprimer cet élève ?')) {
      const eleve = eleves.find(e => e.id === id);
      setEleves(eleves.filter(e => e.id !== id));
      setClasses(classes.map(c => 
        c.id === parseInt(eleve.classe) 
          ? { ...c, effectifActuel: c.effectifActuel - 1 }
          : c
      ));
    }
  };

  const getClasseName = (classeId) => {
    const classe = classes.find(c => c.id === parseInt(classeId));
    return classe ? `${classe.niveau} ${classe.serie}` : '';
  };

  const getMatiereName = (matiereId) => {
    const matiere = matieres.find(m => m.id === parseInt(matiereId));
    return matiere ? matiere.nom : '';
  };

  const filteredEleves = eleves.filter(eleve => {
    const matchClasse = !filterClasse || eleve.classe === filterClasse;
    const matchSearch = !searchEleve || eleve.nom.toLowerCase().includes(searchEleve.toLowerCase()) || eleve.matricule.includes(searchEleve);
    return matchClasse && matchSearch;
  });

  const saveNote = (eleveId, value) => {
    const noteIndex = notes.findIndex(n => 
      n.eleveId === eleveId && 
      n.classeId === noteClasse && 
      n.matiereId === noteMatiere
    );

    if (noteIndex >= 0) {
      const newNotes = [...notes];
      newNotes[noteIndex].note = parseFloat(value) || 0;
      setNotes(newNotes);
    } else {
      setNotes([...notes, {
        id: Date.now(),
        eleveId,
        classeId: noteClasse,
        matiereId: noteMatiere,
        note: parseFloat(value) || 0
      }]);
    }
  };

  const getNote = (eleveId) => {
    const note = notes.find(n => 
      n.eleveId === eleveId && 
      n.classeId === noteClasse && 
      n.matiereId === noteMatiere
    );
    return note ? note.note : '';
  };

  const generateBulletin = () => {
    if (!bulletinClasse || !bulletinEleve) {
      alert('❌ Veuillez sélectionner une classe et un élève');
      return;
    }

    const eleve = eleves.find(e => e.id === parseInt(bulletinEleve));
    const classe = classes.find(c => c.id === parseInt(bulletinClasse));
    const matieresClasse = matieres.filter(m => m.classes.includes(bulletinClasse));
    
    const notesEleve = matieresClasse.map(matiere => {
      const note = notes.find(n => 
        n.eleveId === parseInt(bulletinEleve) && 
        n.matiereId === matiere.id.toString()
      );
      return {
        matiere: matiere.nom,
        coefficient: matiere.coefficient,
        note: note ? note.note : 0,
        total: note ? note.note * matiere.coefficient : 0
      };
    });

    const totalPoints = notesEleve.reduce((sum, n) => sum + n.total, 0);
    const totalCoefficients = notesEleve.reduce((sum, n) => sum + n.coefficient, 0);
    const moyenne = totalCoefficients > 0 ? (totalPoints / totalCoefficients).toFixed(2) : 0;

    setBulletinData({
      eleve,
      classe,
      notes: notesEleve,
      moyenne,
      totalPoints,
      totalCoefficients
    });
  };

  return (
    <div style={{ display: 'flex', height: '100vh', fontFamily: "'Segoe UI', Tahoma, Geneva, Verdana, sans-serif" }}>
      {/* Sidebar */}
      <div style={{ width: '260px', background: '#2c3e50', color: 'white', padding: '20px', overflowY: 'auto' }}>
        <div style={{ textAlign: 'center', marginBottom: '30px', paddingBottom: '20px', borderBottom: '2px solid #34495e' }}>
          <h1 style={{ fontSize: '24px', color: '#3498db', marginBottom: '5px' }}>🎓 SGE</h1>
          <p style={{ fontSize: '12px', color: '#95a5a6' }}>Système de Gestion d'Établissement</p>
        </div>
        {[
          { id: 'dashboard', icon: '📊', label: 'Tableau de bord' },
          { id: 'parametres', icon: '⚙️', label: 'Paramètres' },
          { id: 'classes', icon: '🏫', label: 'Classes' },
          { id: 'matieres', icon: '📚', label: 'Matières' },
          { id: 'enseignants', icon: '👨‍🏫', label: 'Enseignants' },
          { id: 'eleves', icon: '👨‍🎓', label: 'Élèves' },
          { id: 'notes', icon: '📝', label: 'Notes' },
          { id: 'bulletins', icon: '📄', label: 'Bulletins' }
        ].map(item => (
          <div
            key={item.id}
            onClick={() => setCurrentSection(item.id)}
            style={{
              padding: '15px',
              margin: '8px 0',
              background: currentSection === item.id ? '#3498db' : '#34495e',
              borderRadius: '8px',
              cursor: 'pointer',
              display: 'flex',
              alignItems: 'center',
              gap: '10px',
              transition: 'all 0.3s'
            }}
          >
            <span style={{ fontSize: '20px' }}>{item.icon}</span>
            <span>{item.label}</span>
          </div>
        ))}
      </div>

      {/* Main Content */}
      <div style={{ flex: 1, padding: '30px', overflowY: 'auto', background: '#ecf0f1' }}>
        {/* Dashboard */}
        {currentSection === 'dashboard' && (
          <div>
            <div style={{ background: 'white', padding: '20px 30px', borderRadius: '10px', marginBottom: '30px', boxShadow: '0 2px 10px rgba(0,0,0,0.1)', display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
              <h2 style={{ color: '#2c3e50', fontSize: '28px' }}>Tableau de bord</h2>
              <div id="currentDate"></div>
            </div>
            <div style={{ display: 'grid', gridTemplateColumns: 'repeat(auto-fit, minmax(250px, 1fr))', gap: '20px', marginBottom: '30px' }}>
              {[
                { label: 'Total Élèves', value: eleves.length, gradient: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)' },
                { label: 'Total Classes', value: classes.length, gradient: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)' },
                { label: 'Total Enseignants', value: enseignants.length, gradient: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)' },
                { label: 'Total Matières', value: matieres.length, gradient: 'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)' }
              ].map((stat, idx) => (
                <div key={idx} style={{ background: stat.gradient, color: 'white', padding: '25px', borderRadius: '10px', boxShadow: '0 4px 15px rgba(0,0,0,0.2)' }}>
                  <h4 style={{ fontSize: '14px', opacity: 0.9, marginBottom: '10px' }}>{stat.label}</h4>
                  <div style={{ fontSize: '36px', fontWeight: 'bold' }}>{stat.value}</div>
                </div>
              ))}
            </div>
            <div style={{ background: 'white', padding: '25px', borderRadius: '10px', boxShadow: '0 2px 10px rgba(0,0,0,0.1)' }}>
              <h3 style={{ color: '#2c3e50', marginBottom: '20px', paddingBottom: '10px', borderBottom: '2px solid #3498db' }}>Aperçu rapide</h3>
              <p>Bienvenue dans votre système de gestion scolaire. Utilisez le menu à gauche pour naviguer entre les différentes sections.</p>
            </div>
          </div>
        )}

        {/* Paramètres */}
        {currentSection === 'parametres' && (
          <div>
            <div style={{ background: 'white', padding: '20px 30px', borderRadius: '10px', marginBottom: '30px', boxShadow: '0 2px 10px rgba(0,0,0,0.1)' }}>
              <h2 style={{ color: '#2c3e50', fontSize: '28px' }}>Paramètres de l'établissement</h2>
            </div>
            <div style={{ background: 'white', padding: '25px', borderRadius: '10px', boxShadow: '0 2px 10px rgba(0,0,0,0.1)' }}>
              <h3 style={{ color: '#2c3e50', marginBottom: '20px', paddingBottom: '10px', borderBottom: '2px solid #3498db' }}>Informations générales</h3>
              <form onSubmit={handleParametresSubmit}>
                <div style={{ display: 'grid', gridTemplateColumns: 'repeat(auto-fit, minmax(250px, 1fr))', gap: '20px' }}>
                  <div style={{ marginBottom: '20px' }}>
                    <label style={{ display: 'block', marginBottom: '8px', color: '#2c3e50', fontWeight: '600' }}>Nom de l'établissement</label>
                    <input type="text" name="nomEtablissement" defaultValue={parametres.nomEtablissement} placeholder="Ex: Collège Moderne" style={{ width: '100%', padding: '12px', border: '2px solid #ddd', borderRadius: '6px', fontSize: '14px' }} />
                  </div>
                  <div style={{ marginBottom: '20px' }}>
                    <label style={{ display: 'block', marginBottom: '8px', color: '#2c3e50', fontWeight: '600' }}>Année scolaire</label>
                    <input type="text" name="anneeScolaire" defaultValue={parametres.anneeScolaire} placeholder="Ex: 2025-2026" style={{ width: '100%', padding: '12px', border: '2px solid #ddd', borderRadius: '6px', fontSize: '14px' }} />
                  </div>
                  <div style={{ marginBottom: '20px' }}>
                    <label style={{ display: 'block', marginBottom: '8px', color: '#2c3e50', fontWeight: '600' }}>Adresse</label>
                    <input type="text" name="adresse" defaultValue={parametres.adresse} placeholder="Adresse complète" style={{ width: '100%', padding: '12px', border: '2px solid #ddd', borderRadius: '6px', fontSize: '14px' }} />
                  </div>
                  <div style={{ marginBottom: '20px' }}>
                    <label style={{ display: 'block', marginBottom: '8px', color: '#2c3e50', fontWeight: '600' }}>Téléphone</label>
                    <input type="tel" name="telephone" defaultValue={parametres.telephone} placeholder="+225 XX XX XX XX" style={{ width: '100%', padding: '12px', border: '2px solid #ddd', borderRadius: '6px', fontSize: '14px' }} />
                  </div>
                  <div style={{ marginBottom: '20px' }}>
                    <label style={{ display: 'block', marginBottom: '8px', color: '#2c3e50', fontWeight: '600' }}>Email</label>
                    <input type="email" name="email" defaultValue={parametres.email} placeholder="contact@etablissement.com" style={{ width: '100%', padding: '12px', border: '2px solid #ddd', borderRadius: '6px', fontSize: '14px' }} />
                  </div>
                  <div style={{ marginBottom: '20px' }}>
                    <label style={{ display: 'block', marginBottom: '8px', color: '#2c3e50', fontWeight: '600' }}>Directeur Général</label>
                    <input type="text" name="directeur" defaultValue={parametres.directeur} placeholder="Nom du directeur" style={{ width: '100%', padding: '12px', border: '2px solid #ddd', borderRadius: '6px', fontSize: '14px' }} />
                  </div>
                  <div style={{ marginBottom: '20px' }}>
                    <label style={{ display: 'block', marginBottom: '8px', color: '#2c3e50', fontWeight: '600' }}>Censeur/Proviseur</label>
                    <input type="text" name="censeur" defaultValue={parametres.censeur} placeholder="Nom du censeur" style={{ width: '100%', padding: '12px', border: '2px solid #ddd', borderRadius: '6px', fontSize: '14px' }} />
                  </div>
                  <div style={{ marginBottom: '20px' }}>
                    <label style={{ display: 'block', marginBottom: '8px', color: '#2c3e50', fontWeight: '600' }}>Devise de l'établissement</label>
                    <input type="text" name="devise" defaultValue={parametres.devise} placeholder="Ex: Excellence et Discipline" style={{ width: '100%', padding: '12px', border: '2px solid #ddd', borderRadius: '6px', fontSize: '14px' }} />
                  </div>
                </div>
                <button type="submit" style={{ padding: '12px 30px', border: 'none', borderRadius: '6px', cursor: 'pointer', fontSize: '14px', fontWeight: '600', background: '#3498db', color: 'white' }}>💾 Enregistrer les paramètres</button>
              </form>
            </div>
          </div>
        )}

        {/* Classes */}
        {currentSection === 'classes' && (
          <div>
            <div style={{ background: 'white', padding: '20px 30px', borderRadius: '10px', marginBottom: '30px', boxShadow: '0 2px 10px rgba(0,0,0,0.1)', display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
              <h2 style={{ color: '#2c3e50', fontSize: '28px' }}>Gestion des classes</h2>
              <button onClick={() => setActiveModal('classe')} style={{ padding: '12px 30px', border: 'none', borderRadius: '6px', cursor: 'pointer', fontSize: '14px', fontWeight: '600', background: '#27ae60', color: 'white' }}>➕ Ajouter une classe</button>
            </div>
            <div style={{ background: 'white', padding: '25px', borderRadius: '10px', boxShadow: '0 2px 10px rgba(0,0,0,0.1)' }}>
              <h3 style={{ color: '#2c3e50', marginBottom: '20px', paddingBottom: '10px', borderBottom: '2px solid #3498db' }}>Liste des classes</h3>
              <div style={{ overflowX: 'auto' }}>
                <table style={{ width: '100%', borderCollapse: 'collapse', marginTop: '20px' }}>
                  <thead>
                    <tr>
                      <th style={{ background: '#34495e', color: 'white', padding: '12px', textAlign: 'left' }}>Niveau</th>
                      <th style={{ background: '#34495e', color: 'white', padding: '12px', textAlign: 'left' }}>Série</th>
                      <th style={{ background: '#34495e', color: 'white', padding: '12px', textAlign: 'left' }}>Effectif max</th>
                      <th style={{ background: '#34495e', color: 'white', padding: '12px', textAlign: 'left' }}>Effectif actuel</th>
                      <th style={{ background: '#34495e', color: 'white', padding: '12px', textAlign: 'left' }}>Actions</th>
                    </tr>
                  </thead>
                  <tbody>
                    {classes.map(classe => (
                      <tr key={classe.id} style={{ borderBottom: '1px solid #ddd' }}>
                        <td style={{ padding: '12px' }}>{classe.niveau}</td>
                        <td style={{ padding: '12px' }}>{classe.serie}</td>
                        <td style={{ padding: '12px' }}>{classe.effectifMax}</td>
                        <td style={{ padding: '12px' }}>{classe.effectifActuel}</td>
                        <td style={{ padding: '12px' }}>
                          <button onClick={() => deleteClasse(classe.id)} style={{ padding: '8px 16px', border: 'none', borderRadius: '6px', cursor: 'pointer', background: '#e74c3c', color: 'white', fontSize: '12px' }}>🗑️ Supprimer</button>
                        </td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        )}

        {/* Matières */}
        {currentSection === 'matieres' && (
          <div>
            <div style={{ background: 'white', padding: '20px 30px', borderRadius: '10px', marginBottom: '30px', boxShadow: '0 2px 10px rgba(0,0,0,0.1)', display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
              <h2 style={{ color: '#2c3e50', fontSize: '28px' }}>Gestion des matières</h2>
              <button onClick={() => setActiveModal('matiere')} style={{
