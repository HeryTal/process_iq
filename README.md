# Process IQ - Rush School

Application full-stack pour la gestion des candidatures, du suivi administratif et de la génération de documents (CERFA, fiche de renseignements, conventions, etc.).

## Sommaire

1. Présentation
2. Stack technique
3. Structure du projet
4. Prérequis
5. Installation
6. Configuration des variables d’environnement
7. Lancement en local
8. Scripts utiles
9. Endpoints principaux
10. Déploiement

## 1) Présentation

Le projet est organisé en 2 applications principales :

- `frontend` : interface utilisateur React + Vite
- `backend` : API Node.js/Express TypeScript (Airtable + MongoDB optionnel)

Fonctionnalités clés :

- Gestion des candidats et entreprises
- Génération de documents administratifs PDF
- Authentification + rôles
- Support / signalement de bugs
- Intégration signature (DocuSign / UDocSign)

## 2) Stack technique

- Frontend : React 19, TypeScript, Vite, Zustand, React Hook Form
- Backend : Node.js, Express, TypeScript, Airtable, MongoDB/Mongoose, Swagger
- Outils : ts-node-dev, ESLint, Jest

## 3) Structure du projet

```txt
process_iq/
├── frontend/
│   ├── components/
│   ├── services/
│   ├── store/
│   ├── api/
│   └── package.json
└── backend/
    ├── src/
    │   ├── routes/
    │   ├── services/
    │   ├── controllers/
    │   ├── models/
    │   └── config/
    ├── assets/templates_pdf/
    └── package.json
4) Prérequis
Node.js >= 18
npm >= 9
Un compte Airtable (token + base) pour les fonctionnalités admission
MongoDB (optionnel mais recommandé pour les routes protégées /api/candidates, /api/students, etc.)
5) Installation
bash

# depuis la racine du repo
cd frontend
npm install

cd ../backend
npm install
6) Configuration des variables d’environnement
Frontend (frontend/.env.local)
env

VITE_BASE_API_URL=http://localhost:3001/api
GEMINI_API_KEY=
Notes :

VITE_BASE_API_URL doit pointer vers l’API backend (avec /api).
GEMINI_API_KEY est utilisé côté build Vite si nécessaire.
Backend (backend/.env)
env

# Serveur
PORT=3001
NODE_ENV=development
CORS_ORIGIN=*

# Airtable
AIRTABLE_API_TOKEN=pat_xxxxx
AIRTABLE_BASE_ID=app_xxxxx
AIRTABLE_TABLE_ETUDIANTS=Étudiants
AIRTABLE_TABLE_CANDIDATS=Liste des candidats
AIRTABLE_TABLE_ENTREPRISE=Fiche entreprise
AIRTABLE_SUPPORT_TABLE=Support Bugs

# Auth
JWT_SECRET=change_me

# Upload
UPLOAD_DIR=uploads
MAX_FILE_SIZE=10485760

# MongoDB (optionnel)
MONGODB_URI=
MONGODB_DATABASE=processiq

# Logging
LOG_LEVEL=info
PUBLIC_BASE_URL=
Variables optionnelles signature :

DOCUSIGN_ENABLED, DOCUSIGN_AUTH_SERVER, DOCUSIGN_BASE_PATH, DOCUSIGN_INTEGRATION_KEY, DOCUSIGN_USER_ID, DOCUSIGN_ACCOUNT_ID, DOCUSIGN_PRIVATE_KEY ou DOCUSIGN_PRIVATE_KEY_FILE
UDOCSIGN_ENABLED, UDOCSIGN_BASE_URL, UDOCSIGN_API_KEY, etc.
7) Lancement en local
Ouvrir 2 terminaux :

Terminal 1 - Backend
bash

cd backend
npm run dev
API disponible sur :

http://localhost:3001
Swagger : http://localhost:3001/api-docs
OpenAPI JSON : http://localhost:3001/api-docs.json
Terminal 2 - Frontend
bash

cd frontend
npm run dev
App disponible sur :

http://localhost:3000
8) Scripts utiles
Frontend
npm run dev : démarrage Vite
npm run build : build production
npm run preview : prévisualisation build
Backend
npm run dev : API en mode développement
npm run build : compilation TypeScript
npm start : lancement buildé (dist)
npm run test : tests Jest
npm run migrate : migration Airtable -> Mongo
npm run migrate:all : migration complète
npm run seed:test-data : seed de test
npm run seed:signature-docs : seed docs signature
npm run udocsign:batch : batch création demandes signature
9) Endpoints principaux
Santé :

GET /api/health
Admission :

GET /api/admission/candidats
GET /api/admission/candidats/:id
POST /api/admission/candidats
PATCH /api/admission/candidats/:id
GET /api/admission/entreprises
POST /api/admission/entreprises
Documents :

POST /api/admission/candidats/:id/fiche-renseignement
POST /api/admission/candidats/:id/cerfa
POST /api/admission/candidats/:id/atre
POST /api/admission/candidats/:id/compte-rendu
POST /api/admission/candidats/:id/convention-apprentissage
POST /api/admission/candidats/:id/livret-apprentissage
POST /api/admission/candidats/:id/certificat-scolarite
10) Déploiement
Backend prêt pour Render (fichiers présents) :

backend/render.yaml
backend/Procfile
backend/deploy-to-render.sh
backend/DEPLOYMENT_RENDER.md
Frontend :

build Vite standard (npm run build)
config Vercel disponible (frontend/vercel.json)
Conseils de mise en route rapide
Démarrer le backend sur 3001.
Configurer frontend/.env.local avec VITE_BASE_API_URL=http://localhost:3001/api.
Vérifier http://localhost:3001/api/health.
Démarrer le frontend et tester un flux admission.

