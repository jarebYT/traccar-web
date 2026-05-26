# ⚡ Traccar Web - Référence Rapide

## 🎯 Où modifier quoi? (Chemin court)

### 🎨 COULEURS
**Fichier:** `src/common/theme/palette.js`
```javascript
primary: { main: '#FF5722' },     // Votre couleur 1
secondary: { main: '#00BCD4' },   // Votre couleur 2
```
**OU:** Settings → Server → Remplir "Color Primary" et "Color Secondary"

---

### 🎨 LOGO
**Option 1 (Fichier):** `src/resources/images/logo.svg`
- Remplacer le SVG

**Option 2 (Interface):** Settings → Server → "Logo"
- Upload image PNG/JPG

---

### 📝 TEXTES
**Fichier:** `src/resources/l10n/fr.json` (pour français)
```json
"sharedDevice": "Mon Texte Ici",
"loginTitle": "Mon Titre",
```

---

### 🎨 FAVICON
**Fichier:** `public/favicon.ico`
- Remplacer pour icône navigateur

---

### 🎨 ICÔNES PWA
**Fichiers:**
- `public/logo.svg` - Logo PWA
- `public/pwa-*.png` - Icônes app mobile

---

### 🎨 STYLES GLOBAUX
**Fichier:** `public/styles.css`
- CSS global pour tout

---

### 🎨 THÈME MATERIAL-UI
**Fichier:** `src/common/theme/components.js`
```javascript
MuiButton: {
  styleOverrides: { ... }
}
```

---

### 🔧 CONFIGURATION API
**Fichier:** `vite.config.js`
```javascript
proxy: {
  '/api': 'http://localhost:8082',  // Adresse serveur
}
```

---

### 📱 PAGES PRINCIPALES
| Page | Fichier | Utilité |
|------|---------|---------|
| Carte | `src/main/MainPage.jsx` | Suivi temps réel |
| Login | `src/login/LoginPage.jsx` | Connexion |
| Settings | `src/settings/ServerPage.jsx` | Config serveur |
| Rapports | `src/reports/*.jsx` | Statistiques |
| Appareils | `src/settings/DevicesPage.jsx` | Gestion GPS |

---

## 📊 Pages par Type

### Pour **Conducteur/Utilisateur Simple**
- `MainPage` - Voir sa position sur carte
- `ReplayPage` - Voir historique
- `PreferencesPage` - Langue, unités

### Pour **Gestionnaire de Flotte**
- `MainPage` - Suivi tous véhicules
- `DevicesPage` - Ajouter véhicules
- `GeofencesPage` - Zones d'alerte
- `Reports/` - Tous les rapports
- `CommandsPage` - Envoyer ordres aux véhicules

### Pour **Admin Serveur**
- `ServerPage` - Config couleurs/logos
- `UsersPage` - Gérer utilisateurs
- `GroupsPage` - Hiérarchie
- Toutes autres pages

---

## 🔄 Structure Redux (Données)

```
Redux Store
├── session        → Utilisateur + Serveur (couleurs, logos)
├── devices        → Appareils GPS
├── groups         → Groupes
├── geofences      → Zones d'alerte
├── drivers        → Conducteurs
├── events         → Événements GPS
├── maintenances   → Maintenance
├── calendars      → Calendriers
└── motion         → Mouvement
```

**Accès en composant:**
```javascript
const user = useSelector(state => state.session.user);
const devices = useSelector(state => state.devices.items);
const dispatch = useDispatch();
dispatch(sessionActions.updateUser(newUser));
```

---

## 🚀 Commandes Utiles

### Développement
```bash
npm install          # Installer dépendances
npm start            # Lancer dev server (port 3000)
npm run lint         # Vérifier erreurs code
npm run lint:fix     # Corriger erreurs auto
```

### Production
```bash
npm run build        # Compiler pour production
# Fichiers dans: build/
```

---

## 📱 Responsive (Breakpoints Material-UI)

```javascript
theme.breakpoints.up('xs')    // ≥0px     (Mobile)
theme.breakpoints.up('sm')    // ≥600px   (Tablette)
theme.breakpoints.up('md')    // ≥960px   (Desktop)
theme.breakpoints.up('lg')    // ≥1280px  (Large)
theme.breakpoints.up('xl')    // ≥1920px  (XL)
```

**Utilisation:**
```javascript
const desktop = useMediaQuery(theme.breakpoints.up('md'));
```

---

## 🎨 Couleurs Material-UI Prédéfinies

```javascript
import { grey, blue, green, red, orange } from '@mui/material/colors';

grey[50]    // Très clair
grey[200]   // Clair
grey[500]   // Moyen
grey[800]   // Sombre
grey[900]   // Très sombre
```

---

## 🌐 API Endpoints

```
/api/session          - Info utilisateur connecté
/api/devices          - Liste appareils
/api/positions        - Positions GPS
/api/events           - Événements
/api/users            - Utilisateurs
/api/servers          - Config serveur
/api/groups           - Groupes
/api/geofences        - Géofences
/api/commands         - Commandes
/api/notifications    - Notifications
/api/reports/*        - Rapports
```

---

## 🔐 WebSocket

```
ws://localhost:8082/api/socket
```
- Connexion temps réel
- Mises à jour positions
- Événements GPS en direct

---

## 📦 Structure Fichier Compilé (Build)

```
build/
├── index.html        - Page principale
├── assets/
│   ├── index-*.js    - Code JavaScript
│   ├── index-*.css   - Styles CSS
│   └── *.woff2       - Polices
├── manifest.webmanifest
├── service-worker.js
└── ...
```

**À déployer sur serveur web.**

---

## 🎨 Palettes Couleurs Prêtes à l'Emploi

### Orange & Bleu (Technologie)
```javascript
primary: '#FF6B35',
secondary: '#004E89',
```

### Vert & Gris (Écologie)
```javascript
primary: '#2D6A4F',
secondary: '#6A7C7C',
```

### Rouge & Or (Luxe)
```javascript
primary: '#B91C1C',
secondary: '#D97706',
```

### Noir & Violet (Premium)
```javascript
primary: '#1F2937',
secondary: '#7C3AED',
```

### Bleu & Vert (Médical)
```javascript
primary: '#0369A1',
secondary: '#16A34A',
```

---

## 📝 Traductions - Clés Principales

```javascript
// Navigation
"sharedDevice"       // Nom générique "Device"
"sharedDevices"      // Pluriel
"sharedGroup"        // Groupe
"sharedUser"         // Utilisateur
"sharedSettings"     // Paramètres

// Actions
"sharedSave"         // Enregistrer
"sharedEdit"         // Modifier
"sharedRemove"       // Supprimer
"sharedAdd"          // Ajouter
"sharedCancel"       // Annuler

// Pages
"mainTitle"          // Titre page principale
"loginTitle"         // Titre login
"settingsTitle"      // Titre settings
"reportTitle"        // Titre rapports

// Unités
"sharedKm"           // Kilomètres
"sharedMi"           // Miles
"sharedKmh"          // km/h
"sharedMph"          // mi/h
"sharedHour"         // Heure
"sharedMinute"       // Minute
```

---

## 🔄 Flux de Données

```
Backend Traccar (Java/Spring)
    ↓
REST API (/api/*)
    ↓
Redux Store (state)
    ↓
React Components
    ↓
Affichage Utilisateur
```

**Communication inverse:**
```
Utilisateur clique
    ↓
React Event Handler
    ↓
Redux Action / Dispatch
    ↓
Fetch API vers Backend
    ↓
Backend traite
    ↓
WebSocket push mise à jour
    ↓
Store Redux update
    ↓
Re-render composants
```

---

## 🆘 Problèmes Courants

| Problème | Solution |
|----------|----------|
| Couleurs pas appliquées | Vérifier format hex (#RRGGBB), page rechargée, cache vide |
| Logo invisible | Vérifier format (PNG/JPG/SVG), URL accessible, permissions |
| Textes anglais | Vérifier fichier fr.json existe, langue sélectionnée = FR |
| API erreur 404 | Vérifier adresse proxy vite.config.js, serveur backend online |
| Map vide | Vérifier MapLibre token, serveur online, positions reçues |
| Page blanche | F12 console → voir erreurs, Redux DevTools → state |

---

## 🎯 Checklist Avant Production

- [ ] Couleurs personnalisées testées
- [ ] Logo affichage OK (bureau + mobile)
- [ ] Textes français complets
- [ ] Favicon visible navigateur
- [ ] Responsive mobile OK
- [ ] Mode sombre testé
- [ ] API proxy configuré
- [ ] Build sans erreurs (npm run lint)
- [ ] Build compile OK (npm run build)
- [ ] Fichiers build testés en statique
- [ ] Deploiement OK

---

## 📚 Ressources

- **React** https://react.dev
- **Material-UI** https://mui.com
- **Vite** https://vitejs.dev
- **Redux** https://redux.js.org
- **MapLibre** https://maplibre.org
- **Traccar** https://www.traccar.org

---

## 💾 Git Utile

```bash
# Voir changements
git status
git diff src/common/theme/palette.js

# Commit modifications
git add .
git commit -m "Personnaliser couleurs et logo"

# Revenir modification
git checkout src/common/theme/palette.js
```

---

## 🔐 Variables d'Environnement

**Fichier (à créer):** `.env`

```
VITE_API_URL=http://localhost:8082
VITE_MAP_BOX_TOKEN=your_token_here
```

**Utilisation:**
```javascript
const apiUrl = import.meta.env.VITE_API_URL;
```

---

**État 2026 | Traccar Web v6.13.3**

