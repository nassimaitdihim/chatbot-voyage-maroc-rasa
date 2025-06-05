# 🇲🇦 Chatbot Agence de Voyage Marocaine avec RASA

> Chatbot intelligent en arabe pour une agence de voyage marocaine, développé avec RASA Framework

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://python.org)
[![Rasa](https://img.shields.io/badge/Rasa-3.x-orange.svg)](https://rasa.com)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📋 Table des Matières

- [🎯 Objectif du Projet](#-objectif-du-projet)
- [✨ Fonctionnalités](#-fonctionnalités)
- [🏗️ Architecture](#️-architecture)
- [🚀 Installation](#-installation)
- [📖 Guide d'Utilisation](#-guide-dutilisation)
- [🔧 Configuration](#-configuration)
- [🧪 Tests](#-tests)
- [📺 Démonstration](#-démonstration)
- [🤝 Contribution](#-contribution)
- [📝 Licence](#-licence)

## 🎯 Objectif du Projet

Ce projet consiste à développer un chatbot intelligent pour une agence de voyage marocaine, capable de :

- Dialoguer en langue arabe avec les clients
- Gérer les réservations de vols et d'hôtels
- S'intégrer facilement sur un site web
- Proposer des options en temps réel via des APIs externes

## ✨ Fonctionnalités

### 🎯 Intents Personnalisés
- **`book_flight`** - Réservation de vols
- **`book_hotel`** - Réservation d'hôtels
- **`select_option`** - Sélection d'options
- **`change_option`** - Modification de réservations
- **`confirm_reservation`** - Confirmation de réservations

### 🏷️ Entités Reconnues

#### Pour les Vols ✈️
- `departure_city` - Ville de départ
- `destination_city` - Ville de destination
- `departure_date` - Date de départ
- `return_date` - Date de retour
- `flight_class` - Classe de vol (économique, affaires, première)
- `trip_type` - Type de billet (aller-retour, aller simple)

#### Pour les Hôtels 🏨
- `hotel_category` - Catégorie de l'hôtel (3, 4, 5 étoiles)
- `hotel_city` - Ville
- `hotel_district` - Quartier/Zone
- `number_of_people` - Nombre de personnes

### 🔗 Intégrations API
- **Amadeus API** pour les vols (avec fallback)
- **Booking.com API** pour les hôtels (avec fallback)
- Données de démonstration incluses

## 🏗️ Architecture

```
chatbot-voyage-maroc/
├── 📁 rasa/
│   ├── actions.py          # Actions personnalisées
│   ├── config.yml          # Configuration du pipeline
│   ├── domain.yml          # Domaine et réponses
│   ├── nlu.yml            # Données d'entraînement NLU
│   ├── rules.yml          # Règles de conversation
│   └── stories.yml        # Histoires de conversation
├── 📁 web/
│   └── index.html         # Interface web
├── 📁 docs/
│   ├── INSTALLATION.md    # Guide d'installation
│   ├── USAGE.md          # Guide d'utilisation
│   └── API_SETUP.md      # Configuration des APIs
├── 📁 tests/
│   └── test_conversations.yml
├── 📁 models/             # Modèles entraînés
├── requirements.txt       # Dépendances Python
├── README.md             # Ce fichier
└── .env.example          # Variables d'environnement
```

## 🚀 Installation

### Prérequis
- Python 3.8+
- pip ou conda
- Git

### Installation Rapide

1. **Cloner le repository**
```bash
git clone https://github.com/votre-username/chatbot-voyage-maroc.git
cd chatbot-voyage-maroc
```

2. **Créer un environnement virtuel**
```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate     # Windows
```

3. **Installer les dépendances**
```bash
pip install -r requirements.txt
```

4. **Configurer les variables d'environnement**
```bash
cp .env.example .env
# Éditer .env avec vos clés API
```

5. **Entraîner le modèle**
```bash
cd rasa
rasa train
```

6. **Lancer le serveur**
```bash
# Terminal 1 - Actions
rasa run actions

# Terminal 2 - Rasa server
rasa run --enable-api --cors "*"
```

7. **Ouvrir l'interface web**
```bash
# Ouvrir web/index.html dans votre navigateur
# ou utiliser un serveur local
python -m http.server 8000
```

## 📖 Guide d'Utilisation

### 💬 Exemples de Conversations

#### Réservation de Vol
```
Utilisateur: أريد حجز رحلة من الرباط إلى باريس
Bot: من أي مدينة ترغب بالمغادرة؟
Utilisateur: الرباط
Bot: إلى أي مدينة ترغب بالسفر؟
Utilisateur: باريس
Bot: ما هو تاريخ المغادرة المفضل لديك؟
Utilisateur: 15 يونيو
...
```

#### Réservation d'Hôtel
```
Utilisateur: أريد حجز فندق في مراكش
Bot: ما هي فئة الفندق التي تفضلها؟
Utilisateur: 5 نجوم
Bot: كم عدد الأشخاص؟
Utilisateur: شخصين
...
```

### 🎛️ Actions Rapides
L'interface web propose des boutons pour :
- 🛫 حجز رحلة (Réserver un vol)
- 🏨 حجز فندق (Réserver un hôtel)
- ❓ مساعدة (Aide)

## 🔧 Configuration

### Configuration des APIs

#### Amadeus API (Vols)
1. Créer un compte sur [Amadeus for Developers](https://developers.amadeus.com/)
2. Obtenir `CLIENT_ID` et `CLIENT_SECRET`
3. Ajouter dans `.env`:
```env
AMADEUS_CLIENT_ID=votre_client_id
AMADEUS_CLIENT_SECRET=votre_client_secret
```

#### Booking.com API (Hôtels)
1. S'inscrire sur [Booking.com Partner Hub](https://www.booking.com/content/affiliates.html)
2. Obtenir la clé API
3. Ajouter dans `.env`:
```env
BOOKING_API_KEY=votre_api_key
```

### Personnalisation

#### Ajouter de nouvelles villes
Modifiez `actions.py`, fonction `_extract_city_from_message()`:
```python
cities = [
    "مراكش", "الرباط", "الدار البيضاء", "فاس", "طنجة", "أكادير",
    "باريس", "لندن", "مدريد", "روما", "دبي", "القاهرة",
    # Ajoutez vos villes ici
    "نيويورك", "اسطنبول"
]
```

#### Modifier les réponses
Éditez `domain.yml` section `responses`:
```yaml
utter_greet:
- text: "مرحباً! أنا المساعد الآلي لوكالة السفر..."
```

## 🧪 Tests

### Tests Manuels
```bash
cd rasa
rasa shell
```

### Tests Automatisés
```bash
rasa test
```

### Tests des Actions
```bash
python -m pytest tests/
```

## 📺 Démonstration

### Captures d'Écran

**Interface Web**

<br>

<img src="https://github.com/user-attachments/assets/b036cd88-5ae2-49cf-8fbc-0fc726ed4d02" alt="Interface Web" width="600">

<br><br>

**Conversation de Réservation**

<br>

<img src="https://github.com/user-attachments/assets/c69203f0-c01e-4963-a781-25ebfc515c54" alt="Conversation de Réservation" width="500">

### Vidéo de Démonstration
🎥 [Voir la démonstration complète](https://drive.google.com/file/d/1veFqsDuJ_TOyXJfy5b-XFUEY7w4qqtqN/view?usp=sharing)

## 🛠️ Développement

### Structure du Code

#### Actions Principales
- `ActionSearchFlight` - Recherche de vols
- `ActionSearchHotel` - Recherche d'hôtels
- `ActionProcessUserInput` - Traitement des entrées utilisateur
- `ActionConfirmReservation` - Confirmation des réservations
- `ActionUpdateBooking` - Modification des réservations

#### Pipeline NLU
```yaml
language: ar
pipeline:
  - name: WhitespaceTokenizer
  - name: RegexFeaturizer
  - name: LexicalSyntacticFeaturizer
  - name: CountVectorsFeaturizer
  - name: DIETClassifier
    epochs: 100
    entity_recognition: true
    intent_classification: true
```

### Métriques de Performance
- **Précision Intent Classification**: 95%+
- **Précision Entity Recognition**: 90%+
- **Temps de Réponse Moyen**: <2s

## 🚀 Déploiement

### Déploiement Local
Suivez les instructions d'installation ci-dessus.

### Déploiement Production

#### Docker
```bash
# Construire l'image
docker build -t chatbot-voyage .

# Lancer le conteneur
docker run -p 5005:5005 chatbot-voyage
```

#### Serveur Web
1. Copier `web/index.html` sur votre serveur
2. Modifier l'URL de l'API dans le JavaScript
3. Configurer HTTPS pour la production

## 🤝 Contribution

Les contributions sont les bienvenues ! Merci de :

1. Fork le projet
2. Créer une branche feature (`git checkout -b feature/nouvelle-fonctionnalite`)
3. Commit vos changements (`git commit -m 'Ajout nouvelle fonctionnalité'`)
4. Push vers la branche (`git push origin feature/nouvelle-fonctionnalite`)
5. Ouvrir une Pull Request

### Guidelines de Contribution
- Suivre les conventions de code Python (PEP 8)
- Ajouter des tests pour les nouvelles fonctionnalités
- Documenter les changements dans le README
- Tester en arabe uniquement

## 📝 Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

## 🙏 Remerciements

- [Rasa](https://rasa.com/) pour le framework
- [Amadeus](https://developers.amadeus.com/) pour l'API des vols
- [Booking.com](https://www.booking.com/) pour l'API des hôtels
- La communauté open source
---

**Développé avec ❤️ pour les voyageurs marocains**
