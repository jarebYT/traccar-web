# 📍 Traccar Web - Guide Complet de Personnalisation

## 🎯 Table des Matières
1. [Vue d'ensemble du projet](#vue-densemble)
2. [Architecture générale](#architecture-générale)
3. [Structure des dossiers](#structure-des-dossiers)
4. [Pages et leurs fonctionnalités](#pages-et-leurs-fonctionnalités)
5. [Comment personnaliser](#comment-personnaliser)
6. [Fichiers clés à modifier](#fichiers-clés-à-modifier)
7. [Guide étape par étape](#guide-étape-par-étape)
8. [Déploiement](#déploiement)

---

## 🔍 Vue d'ensemble

Traccar Web est une **interface web de suivi GPS** utilisée pour visualiser et gérer les appareils de localisation GPS en temps réel. C'est une application React moderne avec Material-UI.

### Caractéristiques principales:
- **Carte interactive** en temps réel avec MapLibre
- **Gestion des appareils** (véhicules, objets, etc.)
- **Suivi en direct** et historique des positions
- **Rapports** (trajets, arrêts, statistiques, événements)
- **Géofences** (zones de géolocalisation)
- **Notifications** et alertes
- **Paramètres utilisateur** et admin
- **Support multilingue** (60+ langues)

---

## 🏗️ Architecture générale

```
┌─────────────────────────────────────────────────────────┐
│             APPLICATION REACT (Vite)                     │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  ┌─────────────────────────────────────────────────┐    │
│  │      AppThemeProvider (Thème & Couleurs)       │    │
│  └─────────────────────────────────────────────────┘    │
│       ↓                                                   │
│  ┌─────────────────────────────────────────────────┐    │
│  │  LocalizationProvider (Langues & Traductions)  │    │
│  └─────────────────────────────────────────────────┘    │
│       ↓                                                   │
│  ┌─────────────────────────────────────────────────┐    │
│  │        Redux Store (État global)                │    │
│  │   - Devices, Groups, Users, Sessions, etc.     │    │
│  └─────────────────────────────────────────────────┘    │
│       ↓                                                   │
│  ┌─────────────────────────────────────────────────┐    │
│  │      Navigation (Router React)                  │    │
│  │   - Login, Main, Settings, Reports, etc.       │    │
│  └─────────────────────────────────────────────────┘    │
│                                                           │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────┐   │
│  │  SocketIO    │  │  REST API    │  │  WebSocket │   │
│  │  (WebSocket) │  │  (/api)      │  │  (Real-time)   │
│  └──────────────┘  └──────────────┘  └────────────┘   │
│         ↓                  ↓                  ↓         │
├─────────────────────────────────────────────────────────┤
│           BACKEND TRACCAR (Java/Spring)                 │
└─────────────────────────────────────────────────────────┘
```

---

## 📁 Structure des dossiers

```
traccar-web/
│
├── 📄 index.html                    # Point d'entrée HTML
├── 📄 vite.config.js               # Configuration du build (Vite)
├── 📄 package.json                 # Dépendances npm
├── 📄 eslint.config.js             # Règles linting
│
├── 📁 public/                       # Ressources statiques publiques
│   ├── favicon.ico                  # Icône du navigateur
│   ├── logo.svg                     # Logo principal (SVG)
│   ├── styles.css                   # CSS global
│   └── pwa-*.png                    # Icônes PWA (Progressive Web App)
│
├── 📁 src/                          # Code source principal
│
│   ├── 📄 index.jsx                 # Point d'entrée React
│   ├── 📄 App.jsx                   # Composant principal
│   ├── 📄 AppThemeProvider.jsx      # Fournisseur de thème
│   ├── 📄 Navigation.jsx            # Routes et navigation
│   │
│   ├── 📁 common/                   # Composants et utilitaires réutilisables
│   │   ├── attributes/              # Gestion des attributs (Device, User, Server)
│   │   ├── components/              # Composants partagés (Buttons, Forms, etc.)
│   │   │   ├── LocalizationProvider.jsx  # Gestion des langues
│   │   │   ├── PageLayout.jsx           # Layout des pages
│   │   │   ├── SideNav.jsx              # Navigation latérale
│   │   │   ├── NavBar.jsx               # Barre de navigation
│   │   │   ├── BottomMenu.jsx           # Menu inférieur (mobile)
│   │   │   └── ...autres composants
│   │   ├── theme/                   # 🎨 Thème et couleurs
│   │   │   ├── index.js             # Création du thème MUI
│   │   │   ├── palette.js           # 🎨 PALETTES DE COULEURS
│   │   │   ├── components.js        # Overrides MUI
│   │   │   └── dimensions.js        # Dimensions et spacing
│   │   └── util/                    # Utilitaires (helpers, fetch, etc.)
│   │
│   ├── 📁 login/                    # Pages d'authentification
│   │   ├── LoginPage.jsx            # Page de connexion
│   │   ├── RegisterPage.jsx         # Page d'enregistrement
│   │   ├── ResetPasswordPage.jsx    # Réinitialisation mot de passe
│   │   ├── LoginLayout.jsx          # Layout de login
│   │   ├── LogoImage.jsx            # 🎨 Composant du logo
│   │   ├── ChangeServerPage.jsx     # Changement de serveur
│   │   └── LogoImage.jsx            # Affichage du logo
│   │
│   ├── 📁 main/                     # Page principale
│   │   ├── MainPage.jsx             # 🗺️ Page de suivi (Carte + Liste)
│   │   ├── MainMap.jsx              # 🗺️ Composant cartographique
│   │   ├── MainToolbar.jsx          # Barre d'outils
│   │   ├── DeviceList.jsx           # 📱 Liste des appareils
│   │   ├── DeviceRow.jsx            # 📱 Ligne d'appareil
│   │   ├── EventsDrawer.jsx         # 📋 Panneau des événements
│   │   ├── MotionController.jsx     # Contrôle du mouvement
│   │   └── useFilter.js             # Hook de filtrage
│   │
│   ├── 📁 map/                      # Logique cartographique
│   │   ├── core/                    # Cœur de la carte (MapLibre)
│   │   ├── control/                 # Contrôles de la carte
│   │   ├── draw/                    # Dessin sur la carte (Géofences)
│   │   ├── overlay/                 # Calques (overlays)
│   │   └── main/                    # Composants principaux de carte
│   │
│   ├── 📁 settings/                 # ⚙️ Pages de configuration
│   │   ├── ServerPage.jsx           # 🎨 Configuration serveur (logos, couleurs)
│   │   ├── UsersPage.jsx            # Gestion des utilisateurs
│   │   ├── UserPage.jsx             # Édition d'utilisateur
│   │   ├── DevicesPage.jsx          # 📱 Liste des appareils
│   │   ├── DevicePage.jsx           # 📱 Édition d'appareil
│   │   ├── GroupsPage.jsx           # Groupes d'appareils
│   │   ├── GroupPage.jsx            # Édition de groupe
│   │   ├── GeofencesPage.jsx        # Géofences
│   │   ├── GeofencePage.jsx         # Édition géofence
│   │   ├── NotificationsPage.jsx    # Notifications
│   │   ├── NotificationPage.jsx     # Édition notification
│   │   ├── CommandsPage.jsx         # Commandes
│   │   ├── CommandPage.jsx          # Édition commande
│   │   ├── DriversPage.jsx          # 👥 Conducteurs
│   │   ├── DriverPage.jsx           # Édition conducteur
│   │   ├── MaintenancesPage.jsx     # Maintenance
│   │   ├── MaintenancePage.jsx      # Édition maintenance
│   │   ├── CalendarsPage.jsx        # Calendriers
│   │   ├── PreferencesPage.jsx      # Préférences utilisateur
│   │   ├── AnnouncementPage.jsx     # Annonces
│   │   ├── components/              # Composants partagés settings
│   │   │   ├── SettingsMenu.jsx     # Menu de settings
│   │   │   ├── EditAttributesAccordion.jsx
│   │   │   └── ...
│   │   └── common/                  # Utilitaires settings
│   │
│   ├── 📁 reports/                  # 📊 Pages de rapports
│   │   ├── PositionsReportPage.jsx  # Rapport de positions
│   │   ├── EventReportPage.jsx      # Rapport d'événements
│   │   ├── TripReportPage.jsx       # Rapport de trajets
│   │   ├── StopReportPage.jsx       # Rapport d'arrêts
│   │   ├── SummaryReportPage.jsx    # Résumé
│   │   ├── ChartReportPage.jsx      # Graphiques
│   │   ├── StatisticsPage.jsx       # Statistiques
│   │   ├── GeofenceReportPage.jsx   # Rapport géofence
│   │   ├── LogsPage.jsx             # Journaux
│   │   ├── AuditPage.jsx            # Audit
│   │   ├── ScheduledPage.jsx        # Rapports programmés
│   │   ├── CombinedReportPage.jsx   # Rapport combiné
│   │   ├── components/              # Composants rapports
│   │   └── common/                  # Utilitaires rapports
│   │
│   ├── 📁 other/                    # 🔧 Pages diverses
│   │   ├── PositionPage.jsx         # Détail position
│   │   ├── EventPage.jsx            # Détail événement
│   │   ├── GeofencesPage.jsx        # Géofences (autre vue)
│   │   ├── NetworkPage.jsx          # État réseau
│   │   ├── ReplayPage.jsx           # 🎬 Lecture (replay) de trajets
│   │   ├── StreamPage.jsx           # Diffusion en direct
│   │   └── EmulatorPage.jsx         # Émulateur d'appareil
│   │
│   ├── 📁 resources/                # 🎨 Ressources (Images, Traductions)
│   │   ├── images/                  # Images et SVG
│   │   │   ├── logo.svg             # 🎨 Logo par défaut
│   │   │   ├── background.svg       # 🎨 Arrière-plan
│   │   │   ├── icon/                # Icônes
│   │   │   └── data/                # Données géométriques
│   │   └── l10n/                    # 🌍 Traductions (60 langues)
│   │       ├── en.json              # Anglais
│   │       ├── fr.json              # Français
│   │       ├── de.json              # Allemand
│   │       └── ...autres langues
│   │
│   ├── 📁 store/                    # 🔴 Redux (Gestion d'état)
│   │   ├── index.js                 # Store principal
│   │   ├── session.js               # État session/utilisateur
│   │   ├── devices.js               # État appareils
│   │   ├── groups.js                # État groupes
│   │   ├── drivers.js               # État conducteurs
│   │   ├── geofences.js             # État géofences
│   │   ├── events.js                # État événements
│   │   ├── calendars.js             # État calendriers
│   │   ├── maintenances.js          # État maintenance
│   │   ├── motion.js                # État mouvement
│   │   ├── errors.js                # État erreurs
│   │   └── throttleMiddleware.js    # Middleware throttle
│   │
│   ├── 📄 SocketController.jsx      # Gère WebSocket (temps réel)
│   ├── 📄 CachingController.jsx     # Gère le cache
│   ├── 📄 UpdateController.jsx      # Gère les mises à jour
│   ├── 📄 ErrorBoundary.jsx         # Gestion des erreurs
│   ├── 📄 ServerProvider.jsx        # Fournisseur de serveur
│   ├── 📄 reactHelper.js            # Helpers React
│   └── 📄 index.jsx                 # Point d'entrée

└── 📁 simple/                       # Version simple (alternative)
    └── index.html
```

---

## 📄 Pages et leurs fonctionnalités

### 🔐 Authentification (`src/login/`)

#### **LoginPage.jsx** - Page de Connexion
- Formulaire de connexion avec email/mot de passe
- Sélecteur de langue
- Option QR Code
- Option nom d'utilisateur/mot de passe
- Bouton d'enregistrement
- **À personnaliser:** Logos, textes, couleurs

#### **RegisterPage.jsx** - Enregistrement
- Formulaire d'inscription
- Acceptation des conditions d'utilisation
- Acceptation de la politique de confidentialité

#### **ResetPasswordPage.jsx** - Réinitialisation
- Lien de réinitialisation de mot de passe

---

### 🗺️ Page Principale (`src/main/MainPage.jsx`)

La **page la plus importante** - interface de suivi GPS.

**Composants:**
- `MainMap.jsx` - 🗺️ Affiche la carte avec les appareils
- `DeviceList.jsx` - 📱 Liste des appareils à gauche/bas
- `MainToolbar.jsx` - Barre de recherche et filtres
- `EventsDrawer.jsx` - Panneau des événements

**Fonctionnalités:**
- Vue cartographique temps réel (MapLibre)
- Clic sur appareil = centrage + infos
- Barre de recherche par appareil
- Filtres par groupe, statut, etc.
- Affichage événements
- Zoom, navigation, couches cartographiques

---

### ⚙️ Paramètres (`src/settings/`)

#### **ServerPage.jsx** - 🎨 CONFIGURATION SERVEUR
**C'est ici qu'on configure les logos et couleurs!**

Paramètres modifiables:
- **Titre du serveur** (`title`)
- **Description** (`description`)
- **Couleur primaire** (`colorPrimary`) - Code hex
- **Couleur secondaire** (`colorSecondary`) - Code hex
- **Logo** (`logo`) - URL/upload image
- **Logo inversé** (`logoInverted`) - Pour mode sombre
- Mode sombre automatique
- Limite d'enregistrement
- Mapbox access key
- Autres attributs serveur

#### **DevicesPage.jsx/DevicePage.jsx** - 📱 Gestion Appareils
- Liste tous les appareils GPS
- Ajouter/modifier/supprimer appareil
- Configuration appareil (nom, attributs, etc.)

#### **UsersPage.jsx/UserPage.jsx** - 👥 Gestion Utilisateurs
- Liste utilisateurs
- Ajouter/modifier permissions utilisateur
- Réinitialiser mot de passe

#### **GroupsPage.jsx/GroupPage.jsx** - 📦 Groupes d'Appareils
- Organiser appareils en groupes
- Hierarchy: Serveur → Groupes → Appareils

#### **GeofencesPage.jsx/GeofencePage.jsx** - 🚧 Géofences
- Créer zones de géolocalisation
- Déclencher alertes quand appareil rentre/sort

#### **NotificationsPage.jsx/NotificationPage.jsx** - 🔔 Notifications
- Configurer alertes (email, SMS, Telegram, etc.)
- Déclencher sur événements (Vitesse, Géofence, etc.)

#### **CommandsPage.jsx/CommandPage.jsx** - 📨 Commandes
- Envoyer commandes aux appareils (ex: couper moteur)

#### **PreferencesPage.jsx** - 🎨 Préférences Utilisateur
- Langue, fuseau horaire
- Unités (km/mi, m/ft, etc.)
- Thème (clair/sombre)
- Zoom, affichage, etc.

---

### 📊 Rapports (`src/reports/`)

#### **PositionsReportPage.jsx**
- Export positions des appareils
- Filtre par date, appareil
- Format Excel/CSV

#### **TripReportPage.jsx**
- Rapports de trajets
- Distance, durée, vitesse moyenne

#### **StopReportPage.jsx**
- Rapport d'arrêts
- Durée, localisation

#### **EventReportPage.jsx**
- Rapport des événements GPS
- Alarmes, vitesse excessive, etc.

#### **ChartReportPage.jsx**
- Graphiques (vitesse, carburant, etc.)

#### **StatisticsPage.jsx**
- Statistiques générales
- Temps de conduite, distance totale

---

### 🔧 Pages Diverses (`src/other/`)

#### **ReplayPage.jsx** - 🎬 Lecture de Trajet
- Rejouer trajectoire d'appareil
- Accélération, pause, recul temps
- Visualise mouvement en temps accéléré

#### **StreamPage.jsx**
- Diffusion vidéo (si appareil supporte)

#### **EmulatorPage.jsx**
- Émulateur d'appareil GPS
- Tester l'application

---

## 🎨 Comment Personnaliser

### 1. COULEURS ET THÈME

**Fichier clé:** `src/common/theme/palette.js`

```javascript
// Défini les couleurs primaire et secondaire
// Utilise les attributs serveur si disponibles

export default (server, darkMode) => ({
  mode: darkMode ? 'dark' : 'light',
  background: {
    default: darkMode ? grey[900] : grey[50],
  },
  primary: {
    main: validatedColor(server?.attributes?.colorPrimary) 
          || (darkMode ? indigo[200] : indigo[900]),
  },
  secondary: {
    main: validatedColor(server?.attributes?.colorSecondary)
          || (darkMode ? green[200] : green[800]),
  },
});
```

**Option 1: Modification directe (Hardcodé)**
```javascript
primary: {
  main: '#FF5722',  // Orange personnalisé
},
secondary: {
  main: '#00BCD4',  // Cyan personnalisé
},
```

**Option 2: Via l'interface (ServerPage.jsx)**
- Aller à Paramètres → Serveur
- Remplir "Couleur primaire" et "Couleur secondaire"
- Format: Code hex (#RRGGBB)

---

### 2. LOGOS ET IMAGES

**Fichier clé:** `src/login/LogoImage.jsx`

```javascript
// Affiche logo depuis:
// 1. Logo serveur (server.attributes.logo)
// 2. Logo inversé pour mode sombre
// 3. Logo par défaut (Logo.svg)

const logo = useSelector((state) => state.session.server.attributes?.logo);
const logoInverted = useSelector((state) => state.session.server.attributes?.logoInverted);
```

**Option 1: Modification fichier SVG par défaut**
- Fichier: `src/resources/images/logo.svg`
- Remplacer le SVG par votre logo
- Auto-utilisé si pas de logo serveur

**Option 2: Upload via interface**
- Aller à Paramètres → Serveur
- Champ "Logo" - charger image
- Champ "Logo inversé" - pour mode sombre

**Autre image:** Favicon et icônes PWA
- `public/favicon.ico` - Icône navigateur
- `public/logo.svg` - Logo PWA
- `public/pwa-*.png` - Icônes app mobile

---

### 3. TEXTES ET TRADUCTIONS

**Dossier:** `src/resources/l10n/`

Fichiers JSON pour 60+ langues. Exemple `en.json`:

```json
{
  "sharedLoading": "Loading...",
  "sharedSave": "Save",
  "loginTitle": "Login",
  "deviceName": "Device Name",
  ...
}
```

**Personnaliser textes:**

1. **Langue par défaut (anglais)** - Modifier `en.json`
2. **Ajouter langue** - Copier `en.json` vers `xx.json`
3. **Pour toutes les pages:**
   - Rechercher clé dans fichier
   - Remplacer texte

**Exemple - Changer "Device" en "Vehicle":**
```json
// Avant:
"sharedDevice": "Device",

// Après:
"sharedDevice": "Vehicle",
```

---

### 4. TITRE ET DESCRIPTION

**Fichier:** `index.html`

```html
<title>${title}</title>
<meta name="description" content="${description}" />
<meta name="theme-color" content="${colorPrimary}" />
```

Ces variables viennent du **Backend Traccar**. À la configuration du serveur:
- **Titre:** Onglet du navigateur et PWA
- **Description:** Métadonnée SEO
- **Theme-color:** Couleur barre navigateur Android

---

### 5. CSS GLOBAL

**Fichier:** `public/styles.css`

```css
html, body {
  height: 100%;
}

body {
  margin: 0;
  padding: 0;
}

/* Personnaliser: ajouter polices, animations, etc. */
```

**Ajouter police personnalisée:**
```css
@font-face {
  font-family: 'MaPolice';
  src: url('/fonts/mapolice.woff2') format('woff2');
}

body {
  font-family: 'MaPolice', sans-serif;
}
```

---

### 6. COMPOSANTS MATERIAL-UI

**Fichier:** `src/common/theme/components.js`

Override les styles Material-UI par défaut:

```javascript
export default {
  MuiButton: {
    styleOverrides: {
      sizeMedium: {
        height: '40px',
      },
    },
  },
  MuiOutlinedInput: {
    styleOverrides: {
      root: ({ theme }) => ({
        backgroundColor: theme.palette.background.default,
      }),
    },
  },
};
```

---

## 📝 Fichiers clés à modifier

| Fichier | Utilité | À modifier |
|---------|---------|-----------|
| `src/common/theme/palette.js` | 🎨 Couleurs | Couleurs primaire/secondaire |
| `src/resources/images/logo.svg` | 🎨 Logo | Logo par défaut |
| `src/resources/l10n/en.json` | 📝 Textes | Tous les textes app |
| `src/resources/l10n/fr.json` | 📝 Textes français | Traduction française |
| `public/favicon.ico` | 🎨 Icône | Favicon navigateur |
| `public/styles.css` | 🎨 CSS | Styles globaux |
| `src/common/theme/index.js` | 🎨 Thème | Configuration thème MUI |
| `vite.config.js` | ⚙️ Build | Proxy API, configuration |
| `src/settings/ServerPage.jsx` | ⚙️ Interface | Où configurer couleurs/logos |

---

## 🚀 Guide étape par étape

### Étape 1: Préparer l'environnement

```bash
# Installer dépendances
npm install

# Ou avec Yarn
yarn install
```

### Étape 2: Démarrer en développement

```bash
# Lancer le serveur dev (port 3000)
npm start

# L'app sera accessible à http://localhost:3000
```

### Étape 3: Modifier les couleurs

**Méthode simple (Interface web):**
1. Se connecter à l'app
2. Aller à Settings → Server
3. Remplir "Color Primary" (ex: `#FF5722`)
4. Remplir "Color Secondary" (ex: `#00BCD4`)
5. Cliquer Save

**Méthode directe (Code):**
1. Ouvrir `src/common/theme/palette.js`
2. Remplacer les valeurs:
```javascript
primary: {
  main: '#FF5722',  // Votre couleur
},
secondary: {
  main: '#00BCD4',  // Votre couleur
},
```
3. Sauvegarder et voir les changements en temps réel

### Étape 4: Changer le logo

**Option A: Via interface**
1. Settings → Server
2. "Logo" - Upload image PNG/JPG
3. "Logo Inverted" - Pour mode sombre
4. Save

**Option B: Remplacer fichier SVG**
1. Ouvrir `src/resources/images/logo.svg`
2. Remplacer le contenu par votre SVG
3. Sauvegarder

**Option C: Favicon**
1. Remplacer `public/favicon.ico`
2. Générer depuis: https://favicon-generator.org/

### Étape 5: Traduire/Modifier textes

**Changer texte français:**
1. Ouvrir `src/resources/l10n/fr.json`
2. Rechercher le texte à changer
3. Exemple:
```json
"sharedDevice": "Mon Véhicule",
"loginTitle": "Ma Super App GPS",
```

**Ajouter nouvelle langue:**
1. Copier `src/resources/l10n/en.json` vers `xx.json`
2. Traduire tous les textes
3. Automatiquement disponible dans l'app

### Étape 6: Build pour production

```bash
# Compiler application optimisée
npm run build

# Fichiers générés dans: build/
```

### Étape 7: Déployer

```bash
# Les fichiers à déployer:
# - build/      (fichiers compilés)
# - public/     (ressources statiques)

# Copier le contenu de build/ vers serveur web
# Configurer proxy /api vers serveur Traccar backend
```

---

## 📋 Structure CSS et Thème

### Material-UI avec Emotion

Traccar utilise **Material-UI v5** avec système de thème:

```javascript
// Thème global: src/common/theme/index.js
createTheme({
  palette: { /* couleurs */ },
  typography: { /* polices */ },
  dimensions: { /* tailles */ },
  components: { /* overrides */ },
})

// Utilisation dans composants:
const useStyles = makeStyles()((theme) => ({
  button: {
    backgroundColor: theme.palette.primary.main,
    width: theme.dimensions.drawerWidthDesktop,
  }
}))
```

### Modifier styles d'un composant

Exemple: Changer couleur bouton primary

```javascript
// Dans src/common/theme/components.js
export default {
  MuiButton: {
    styleOverrides: {
      containedPrimary: {
        backgroundColor: '#FF5722',  // Orange
        color: '#fff',
      }
    }
  }
}
```

---

## 🔗 Connexion Backend

**Fichier:** `vite.config.js`

```javascript
server: {
  port: 3000,
  proxy: {
    '/api/socket': 'ws://10.0.0.222:8082',  // WebSocket
    '/api': 'http://10.0.0.222:8082',       // REST API
  },
},
```

**À adapter:**
- Remplacer `10.0.0.222:8082` par adresse serveur Traccar
- `ws://` = WebSocket (temps réel)
- `http://` = REST API (requêtes HTTP)

**En production:**
- Configurer proxy au niveau serveur web (Nginx, Apache)
- Ou déployer frontend sur même serveur que backend

---

## 📱 Pages par utilité

### Pour le **driver/utilisateur simple**
- **MainPage** - Voir véhicule sur carte ✅
- **ReplayPage** - Voir historique ✅
- **PreferencesPage** - Langue, fuseau ✅

### Pour le **gestionnaire de flotte**
- **MainPage** - Suivi temps réel ✅
- **DevicesPage** - Ajouter/gérer véhicules ✅
- **GeofencesPage** - Zones géofence ✅
- **Reports** - Tous rapports ✅
- **CommandsPage** - Envoyer ordres ✅

### Pour l'**administrateur**
- **ServerPage** - Configurer serveur ✅
- **UsersPage** - Gestion utilisateurs ✅
- **GroupsPage** - Hiérarchie appareils ✅
- **NotificationsPage** - Alertes ✅
- Toutes autres pages de settings ✅

---

## 🎯 Checklist Personnalisation

- [ ] Modifier couleurs primaire/secondaire
- [ ] Remplacer logo
- [ ] Changer favicon
- [ ] Traduire textes (ou créer nouvelle langue)
- [ ] Configurer titre/description serveur
- [ ] Ajouter polices personnalisées si besoin
- [ ] Tester sur mobile (responsive)
- [ ] Tester modes clair et sombre
- [ ] Build pour production
- [ ] Déployer sur serveur

---

## 🆘 Dépannage

### Les couleurs ne changent pas
- **Vérifier:** Format hex (#RRGGBB)
- **Vérifier:** Server page sauvegardé (bouton Save)
- **Vérifier:** Page rechargée (F5)
- **Vérifier:** Cache navigateur (Ctrl+Shift+Delete)

### Le logo ne s'affiche pas
- **Vérifier:** Format image (PNG, JPG, SVG)
- **Vérifier:** URL accessible
- **Vérifier:** Permissions fichier
- **Fallback:** Utilise logo.svg par défaut

### Textes en anglais au lieu du français
- **Vérifier:** Fichier `src/resources/l10n/fr.json` existe
- **Vérifier:** Langue sélectionnée = français
- **Vérifier:** LocalizationProvider charge la langue

### Proxy API ne fonctionne pas
- **Vérifier:** Adresse serveur correcte dans `vite.config.js`
- **Vérifier:** Serveur Traccar backend en route
- **Vérifier:** Port 8082 accessible
- **Production:** Configurer proxy Nginx/Apache

---

## 📚 Structure React/Redux

### État global (Redux Store)
```
store/
├── session.js         # Utilisateur, serveur
├── devices.js         # Liste appareils
├── groups.js          # Groupes
├── geofences.js       # Géofences
├── events.js          # Événements
├── drivers.js         # Conducteurs
├── maintenances.js    # Maintenance
├── calendars.js       # Calendriers
└── motion.js          # Mouvement
```

### Accès état dans composants
```javascript
// Hook Redux
const state = useSelector((state) => state.session.server);
const dispatch = useDispatch();

// Modifier état
dispatch(sessionActions.updateUser(newUser));
```

---

## 🔐 Communication Backend

### WebSocket (Temps réel)
- `/api/socket` - Mise à jour temps réel
- Positions en direct
- Événements GPS

### REST API
- `/api/session` - Session utilisateur
- `/api/devices` - Appareils
- `/api/positions` - Positions
- `/api/events` - Événements

### Exemple appel API
```javascript
// Fetch utilisateur
const response = await fetch('/api/users/123');
const user = await response.json();

// Mettre à jour
await fetch('/api/users/123', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(updatedUser),
});
```

---

## ✅ Conclusion

Traccar Web est une application **complexe mais modulaire**. Les personnalisations principales sont:

1. **Couleurs** → `palette.js` ou ServerPage
2. **Logos** → `logo.svg` ou upload
3. **Textes** → `l10n/*.json`
4. **Styles** → `components.js` ou CSS
5. **Backend** → `vite.config.js` proxy

**N'hésitez pas à explorer** les fichiers. React et Material-UI sont bien documentés. Bon développement! 🚀

---

## 📖 Ressources

- [React Docs](https://react.dev)
- [Material-UI Docs](https://mui.com)
- [Vite Docs](https://vitejs.dev)
- [Redux Docs](https://redux.js.org)
- [MapLibre GL](https://maplibre.org/maplibre-gl-js/docs/)
- [Traccar Official](https://www.traccar.org)

