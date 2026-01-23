import React, { useState, useEffect } from 'react';
import { User, Calendar, Phone, MapPin, Users } from 'lucide-react';

export default function FormApp() {
  const [formData, setFormData] = useState({
    nom: '',
    prenom: '',
    sexe: '',
    dateNaissance: '',
    telephone: '',
    pays: ''
  });
  
  const [savedData, setSavedData] = useState([]);
  const [showSuccess, setShowSuccess] = useState(false);

  const pays = [
    'Togo', 'Bénin', 'Mali', 'Côte d\'Ivoire', 'Sénégal', 
    'Burkina Faso', 'Niger', 'Ghana', 'Nigeria', 'Cameroun',
    'France', 'USA', 'UK', 'Canada', 'Allemagne'
  ];

  useEffect(() => {
    const stored = localStorage.getItem('utilisateurs');
    if (stored) {
      setSavedData(JSON.parse(stored));
    }
  }, []);

  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleSubmit = () => {
    if (!formData.nom || !formData.prenom || !formData.sexe || !formData.dateNaissance || !formData.telephone || !formData.pays) {
      alert('Veuillez remplir tous les champs');
      return;
    }
    
    const newEntry = {
      ...formData,
      id: Date.now(),
      dateEnregistrement: new Date().toLocaleString('fr-FR')
    };
    
    const updatedData = [...savedData, newEntry];
    setSavedData(updatedData);
    localStorage.setItem('utilisateurs', JSON.stringify(updatedData));
    
    setFormData({
      nom: '',
      prenom: '',
      sexe: '',
      dateNaissance: '',
      telephone: '',
      pays: ''
    });
    
    setShowSuccess(true);
    setTimeout(() => setShowSuccess(false), 3000);
  };

  const handleDelete = (id) => {
    const updatedData = savedData.filter(item => item.id !== id);
    setSavedData(updatedData);
    localStorage.setItem('utilisateurs', JSON.stringify(updatedData));
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 py-8 px-4">
      <div className="max-w-4xl mx-auto">
        <div className="bg-white rounded-lg shadow-xl p-8 mb-8">
          <h1 className="text-3xl font-bold text-gray-800 mb-6 text-center">
            Formulaire d'inscription
          </h1>
          
          {showSuccess && (
            <div className="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded mb-4">
              ✓ Données enregistrées avec succès !
            </div>
          )}

          <div className="space-y-6">
            <div className="grid md:grid-cols-2 gap-6">
              <div>
                <label className="flex items-center text-gray-700 font-medium mb-2">
                  <User className="w-5 h-5 mr-2" />
                  Nom
                </label>
                <input
                  type="text"
                  name="nom"
                  value={formData.nom}
                  onChange={handleChange}
                  className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  placeholder="Votre nom"
                />
              </div>

              <div>
                <label className="flex items-center text-gray-700 font-medium mb-2">
                  <User className="w-5 h-5 mr-2" />
                  Prénom
                </label>
                <input
                  type="text"
                  name="prenom"
                  value={formData.prenom}
                  onChange={handleChange}
                  className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  placeholder="Votre prénom"
                />
              </div>
            </div>

            <div>
              <label className="flex items-center text-gray-700 font-medium mb-2">
                <Users className="w-5 h-5 mr-2" />
                Sexe
              </label>
              <div className="flex gap-6">
                <label className="flex items-center cursor-pointer">
                  <input
                    type="radio"
                    name="sexe"
                    value="Homme"
                    checked={formData.sexe === 'Homme'}
                    onChange={handleChange}
                    className="mr-2"
                  />
                  Homme
                </label>
                <label className="flex items-center cursor-pointer">
                  <input
                    type="radio"
                    name="sexe"
                    value="Femme"
                    checked={formData.sexe === 'Femme'}
                    onChange={handleChange}
                    className="mr-2"
                  />
                  Femme
                </label>
                <label className="flex items-center cursor-pointer">
                  <input
                    type="radio"
                    name="sexe"
                    value="Autre"
                    checked={formData.sexe === 'Autre'}
                    onChange={handleChange}
                    className="mr-2"
                  />
                  Autre
                </label>
              </div>
            </div>

            <div>
              <label className="flex items-center text-gray-700 font-medium mb-2">
                <Calendar className="w-5 h-5 mr-2" />
                Date de naissance
              </label>
              <input
                type="date"
                name="dateNaissance"
                value={formData.dateNaissance}
                onChange={handleChange}
                className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              />
            </div>

            <div>
              <label className="flex items-center text-gray-700 font-medium mb-2">
                <Phone className="w-5 h-5 mr-2" />
                Numéro de téléphone
              </label>
              <input
                type="tel"
                name="telephone"
                value={formData.telephone}
                onChange={handleChange}
                className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                placeholder="+228 XX XX XX XX"
              />
            </div>

            <div>
              <label className="flex items-center text-gray-700 font-medium mb-2">
                <MapPin className="w-5 h-5 mr-2" />
                Pays
              </label>
              <select
                name="pays"
                value={formData.pays}
                onChange={handleChange}
                className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              >
                <option value="">Sélectionnez votre pays</option>
                {pays.map(p => (
                  <option key={p} value={p}>{p}</option>
                ))}
              </select>
            </div>

            <div className="pt-4">
              <button
                onClick={handleSubmit}
                className="w-full bg-blue-600 text-white py-4 px-6 rounded-lg font-bold text-lg hover:bg-blue-700 transition duration-200 shadow-lg"
              >
                SOUMETTRE
              </button>
            </div>
          </div>
        </div>

        {savedData.length > 0 && (
          <div className="bg-white rounded-lg shadow-xl p-8">
            <h2 className="text-2xl font-bold text-gray-800 mb-4">
              Données enregistrées ({savedData.length})
            </h2>
            <div className="space-y-4">
              {savedData.map(item => (
                <div key={item.id} className="border border-gray-200 rounded-lg p-4 hover:shadow-md transition">
                  <div className="grid md:grid-cols-2 gap-2 text-sm">
                    <p><strong>Nom:</strong> {item.nom} {item.prenom}</p>
                    <p><strong>Sexe:</strong> {item.sexe}</p>
                    <p><strong>Date de naissance:</strong> {item.dateNaissance}</p>
                    <p><strong>Téléphone:</strong> {item.telephone}</p>
                    <p><strong>Pays:</strong> {item.pays}</p>
                    <p className="text-gray-500"><strong>Enregistré le:</strong> {item.dateEnregistrement}</p>
                  </div>
                  <button
                    onClick={() => handleDelete(item.id)}
                    className="mt-3 text-red-600 hover:text-red-800 text-sm font-medium"
                  >
                    Supprimer
                  </button>
                </div>
              ))}
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
