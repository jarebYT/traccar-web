# 🎨 GUIDE COMPLET DE PERSONNALISATION TRACCAR WEB

## 📋 Table des matières

1. [Thème Global](#-thème-global)
2. [Logos et Images](#-logos-et-images)
3. [Textes et Traductions](#-textes-et-traductions)
4. [Attributs Serveur](#-attributs-serveur)
5. [Configuration Vite](#-configuration-vite)
6. [Styles CSS](#-styles-css)
7. [Composants UI](#-composants-ui)
8. [Carte et Géofences](#-carte-et-géofences)
9. [Pages Personnalisables](#-pages-personnalisables)
10. [Workflow Complet](#-workflow-complet)

---

## 🎨 THÈME GLOBAL

### Fichiers clés
```
src/common/theme/
├── index.js              # Configuration thème MUI
├── palette.js            # Couleurs primaire/secondaire
├── components.js         # Overrides composants MUI
└── dimensions.js         # Dimensions/espacements
```

### Personnaliser les couleurs

#### Option 1: Via l'interface (Recommandé)
```
1. Aller à Paramètres → Serveur
2. Section "Preferences":
   - Couleur primaire: #FF5722 (hex)
   - Couleur secondaire: #00BCD4 (hex)
   - Mode sombre: cocher/décocher
3. Cliquer Save
4. Actualiser (F5)
```

#### Option 2: Modification du fichier palette.js
**Fichier:** `src/common/theme/palette.js`

```javascript
import { grey, green, indigo } from '@mui/material/colors';

const validatedColor = (color) => (/^#([0-9A-Fa-f]{3}){1,2}$/.test(color) ? color : null);

export default (server, darkMode) => ({
  mode: darkMode ? 'dark' : 'light',
  background: {
    default: darkMode ? '#121212' : '#FAFAFA',
  },
  primary: {
    main: validatedColor(server?.attributes?.colorPrimary) 
          || (darkMode ? '#BB86FC' : '#6200EE'),  // ← Modifier ici
  },
  secondary: {
    main: validatedColor(server?.attributes?.colorSecondary)
          || (darkMode ? '#03DAC6' : '#03DAC6'),  // ← Modifier ici
  },
  neutral: { main: grey[500] },
  geometry: { main: '#3bb2d0' },
  alwaysDark: { main: grey[900] },
  error: { main: '#CF6679' },
  warning: { main: '#FFA500' },
  success: { main: '#4CAF50' },
  info: { main: '#2196F3' },
});
```

### Personnaliser les dimensions

**Fichier:** `src/common/theme/dimensions.js`

```javascript
export default {
  sidebarWidth: '28%',                    // Largeur barre latérale
  sidebarWidthTablet: '52px',             // Mode compact tablette
  drawerWidthDesktop: '360px',            // Largeur drawer desktop
  drawerWidthTablet: '320px',             // Largeur drawer mobile
  drawerHeightPhone: '250px',             // Hauteur drawer téléphone
  filterFormWidth: '160px',               // Largeur formulaire filtrage
  eventsDrawerWidth: '320px',             // Largeur panneau événements
  bottomBarHeight: 56,                    // Hauteur barre inférieure
  popupMapOffset: 300,                    // Décalage popup carte
  popupMaxWidth: 288,                     // Largeur max popup
  popupImageHeight: 144,                  // Hauteur image popup
  cardContentMaxHeight: '40vh',           // Hauteur max contenu carte
  qrCodeSize: 192,                        // Taille code QR
};
```

### Personnaliser les composants MUI

**Fichier:** `src/common/theme/components.js`

```javascript
export default {
  MuiUseMediaQuery: {
    defaultProps: { noSsr: true },
  },
  MuiOutlinedInput: {
    styleOverrides: {
      root: ({ theme }) => ({
        backgroundColor: theme.palette.background.default,
        borderRadius: '8px',                    // ← Coins arrondis
      }),
    },
  },
  MuiButton: {
    styleOverrides: {
      sizeMedium: {
        height: '40px',
        borderRadius: '8px',
      },
    },
  },
  MuiFormControl: {
    defaultProps: { size: 'small' },
  },
  MuiSnackbar: {
    defaultProps: {
      anchorOrigin: { vertical: 'bottom', horizontal: 'center' },
    },
  },
  MuiTooltip: {
    defaultProps: {
      enterDelay: 500,
      enterNextDelay: 500,
    },
  },
  MuiTableCell: {
    styleOverrides: {
      root: ({ theme }) => ({
        '@media print': {
          color: theme.palette.alwaysDark.main,
        },
      }),
    },
  },
};
```

---

## 🎨 LOGOS ET IMAGES

### Fichiers

```
src/resources/images/
├── logo.svg                    # Logo SVG par défaut
├── icon/                       # Icônes SVG
└── data/                       # Géométries

public/
├── favicon.ico                 # Icône navigateur 16x16
├── apple-touch-icon-180x180.png # Icône iOS
├── logo.svg                    # Logo PWA
└── pwa-*.png                   # Icônes PWA (64x64, 192x192, 512x512)
```

### Personnaliser le logo

#### Option A: Via l'interface
```
1. Paramètres → Serveur
2. Section "Attributes":
   - Logo: URL externe ou upload
   - Logo inversé: Pour mode sombre
```

Formats supportés:
- URLs: `https://example.com/logo.png`
- Upload direct depuis interface

#### Option B: Remplacer fichier SVG par défaut
**Fichier:** `src/resources/images/logo.svg`

Remplacer contenu SVG par votre logo. Auto-utilisé si pas de logo serveur.

#### Option C: Icône navigateur
**Fichier:** `public/favicon.ico`

- Format: 16x16 pixels ou 32x32
- Générer avec outils online: `favicon-generator.org`

#### Option D: Icônes PWA
Pour application mobile native:

```
public/apple-touch-icon-180x180.png  → iOS
public/pwa-64x64.png                 → Petite icône
public/pwa-192x192.png               → Icône app
public/pwa-512x512.png               → Grande icône

Générer avec: npm run generate-pwa-assets
```

### Personnaliser le login layout

**Fichier:** `src/login/LoginLayout.jsx`

```javascript
const useStyles = makeStyles()((theme) => ({
  root: {
    display: 'flex',
    height: '100%',
  },
  sidebar: {
    display: 'flex',
    justifyContent: 'center',
    alignItems: 'center',
    background: theme.palette.primary.main,
    backgroundImage: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',  // Gradient
    backgroundSize: 'cover',
    paddingBottom: theme.spacing(5),
    width: theme.dimensions.sidebarWidth,
  },
  paper: {
    display: 'flex',
    flexDirection: 'column',
    justifyContent: 'center',
    alignItems: 'center',
    flex: 1,
    boxShadow: '-2px 0px 16px rgba(0, 0, 0, 0.25)',
  },
  form: {
    maxWidth: theme.spacing(52),
    padding: theme.spacing(5),
    width: '100%',
  },
}));
```

---

## 📝 TEXTES ET TRADUCTIONS

### Dossier
```
src/resources/l10n/
├── en.json    # Anglais (par défaut)
├── fr.json    # Français
├── de.json    # Allemand
├── es.json    # Espagnol
└── ... 50+ autres langues
```

### Modifier les textes

**Fichier:** `src/resources/l10n/en.json`

```json
{
  "sharedLoading": "Loading...",
  "sharedSave": "Save",
  "sharedCancel": "Cancel",
  "sharedDelete": "Delete",
  
  // Login
  "loginTitle": "Login",
  "loginEmail": "Email",
  "loginPassword": "Password",
  
  // Terminologie
  "sharedDevice": "Device",        // Changer en "Vehicle", "Tracker", etc.
  "sharedGroup": "Group",
  "sharedLocation": "Location",
  "sharedPosition": "Position",
  
  // Menu
  "mainTitle": "Tracking",
  "deviceName": "Device Name",
  
  // Settings
  "settingsTitle": "Settings",
  "settingsServer": "Server",
  "settingsUsers": "Users",
  "settingsDevices": "Devices",
  
  // Server attributes
  "serverName": "Server Name",
  "serverDescription": "Description",
  "serverLogo": "Logo",
  "serverLogoInverted": "Inverted Logo",
  "serverColorPrimary": "Primary Color",
  "serverColorSecondary": "Secondary Color",
}
```

### Ajouter une langue

1. Copier `src/resources/l10n/en.json`
2. Renommer `src/resources/l10n/xx.json` (xx = code langue)
   - `es.json` pour Espagnol
   - `it.json` pour Italien
   - `pt.json` pour Portugais
3. Traduire tous les textes
4. La langue apparaît dans le sélecteur

### Codes langue (ISO 639-1)

```
en → English
fr → Français
de → Deutsch
es → Español
it → Italiano
pt → Português
ru → Русский
zh → 中文
ja → 日本語
ar → العربية
```

---

## ⚙️ ATTRIBUTS SERVEUR

### Fichier
`src/common/attributes/useServerAttributes.js`

### Tous les attributs modifiables

Via **Paramètres → Serveur** dans l'interface:

```javascript
{
  // ═══ PERSONNALISATION VISUELLE ═══
  logo: "URL ou upload",                    // Logo principal
  logoInverted: "URL ou upload",           // Logo mode sombre
  colorPrimary: "#RRGGBB",                 // Couleur primaire
  colorSecondary: "#RRGGBB",               // Couleur secondaire
  darkMode: true/false,                    // Mode sombre auto
  
  // ═══ INFORMATIONS ═══
  title: "Nom du serveur",
  description: "Description courte",
  support: "contact@example.com",
  
  // ═══ URLS LÉGALES ═══
  termsUrl: "https://...",
  privacyUrl: "https://...",
  
  // ═══ CARTE ═══
  map: "locationIqStreets",                // Provider par défaut
  mapUrl: "https://...",                   // URL carte personnalisée
  overlayUrl: "https://...",               // Couche overlay
  poiLayer: "Couche POI custom",
  
  // ═══ UNITÉS ═══
  speedUnit: "kn|kmh|mph",                 // Vitesse
  distanceUnit: "km|mi|nmi",               // Distance
  altitudeUnit: "m|ft",                    // Altitude
  volumeUnit: "ltr|usGal|impGal",          // Volume
  timezone: "UTC|Europe/Paris|...",        // Fuseau horaire
  coordinateFormat: "dd|ddm|dms",          // Format coordonnées
  
  // ═══ PERMISSIONS ═══
  registration: true,                      // Permettre enregistrement
  readonly: false,                         // Lecture seule serveur
  deviceReadonly: false,                   // Appareils lecture seule
  disableChange: false,                    // Désactiver changement serveur
  disableShare: false,                     // Désactiver partage
  
  // ═══ SÉCURITÉ 2FA ═══
  totpEnable: true,
  totpForce: false,
  
  // ═══ LOCALISATION ═══
  latitude: 48.8566,                       // Par défaut
  longitude: 2.3522,                       // Par défaut
  zoom: 11,                                // Zoom par défaut
  
  // ═══ ANNONCES ═══
  announcement: "Texte annonce",
  forceSettings: false,                    // Forcer prefs tous utilisateurs
  
  // ═══ PWA ═══
  serviceWorkerUpdateInterval: 3600000,
  
  // ═══ UI ═══
  ui.disableLoginLanguage: false,
}
```

### Accéder depuis composant

```javascript
import { useSelector } from 'react-redux';

const MyComponent = () => {
  const server = useSelector((state) => state.session.server);
  
  const primaryColor = server.attributes?.colorPrimary;
  const isDarkMode = server.attributes?.darkMode;
  const title = server.title;
  
  return <div style={{ color: primaryColor }}>{title}</div>;
};
```

---

## ⚙️ CONFIGURATION VITE

**Fichier:** `vite.config.js`

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react-swc';
import svgr from 'vite-plugin-svgr';
import { VitePWA } from 'vite-plugin-pwa';
import { viteStaticCopy } from 'vite-plugin-static-copy';

export default defineConfig(() => ({
  server: {
    port: 3000,
    proxy: {
      '/api/socket': 'ws://10.0.0.222:8082',  // ← WebSocket URL
      '/api': 'http://10.0.0.222:8082',       // ← API REST URL
    },
  },
  build: {
    outDir: 'build',                          // ← Dossier sortie
  },
  plugins: [
    svgr(),
    react(),
    VitePWA({
      includeAssets: ['favicon.ico', 'apple-touch-icon-180x180.png'],
      workbox: {
        navigateFallbackDenylist: [/^\/api/],
        globPatterns: ['**/*.{js,css,html,woff,woff2,mp3}'],
      },
      manifest: {
        short_name: '${title}',              // ← Remplacé au build
        name: '${description}',              // ← Remplacé au build
        theme_color: '${colorPrimary}',      // ← Remplacé au build
        icons: [
          {
            src: 'pwa-64x64.png',
            sizes: '64x64',
            type: 'image/png',
          },
          {
            src: 'pwa-192x192.png',
            sizes: '192x192',
            type: 'image/png',
          },
          {
            src: 'pwa-512x512.png',
            sizes: '512x512',
            type: 'image/png',
            purpose: 'any maskable',
          },
        ],
      },
    }),
  ],
}));
```

### À personnaliser

**Remplacer l'URL du serveur:**
```javascript
// Avant
'/api/socket': 'ws://10.0.0.222:8082',
'/api': 'http://10.0.0.222:8082',

// Après
'/api/socket': 'ws://votre-serveur.com:8082',
'/api': 'http://votre-serveur.com:8082',
```

---

## 🎨 STYLES CSS

**Fichier:** `public/styles.css`

```css
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: 'Roboto', 'Segoe UI', sans-serif;
}

body {
  margin: 0;
  padding: 0;
  background-color: #FAFAFA;
}

/* Mode sombre */
@media (prefers-color-scheme: dark) {
  body {
    background: #121212;
  }
}

/* Layout principal */
.root {
  display: flex;
  flex-direction: column;
  height: 100%;
}

/* Impression */
@media print {
  * {
    height: auto !important;
    overflow: visible !important;
  }
}

/* Ajouter ci-dessous vos styles perso */

/* Police personnalisée */
@font-face {
  font-family: 'MaPolice';
  src: url('/fonts/mapolice.woff2') format('woff2');
}

/* Animations personnalisées */
@keyframes slideIn {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}

.side-menu {
  animation: slideIn 0.3s ease-out;
}

/* Scrollbar personnalisée */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #555;
}
```

---

## 🧩 COMPOSANTS UI

### PageLayout (Layout principal)

**Fichier:** `src/common/components/PageLayout.jsx`

Composant de base pour toutes les pages avec drawer et toolbar.

```javascript
import PageLayout from '../common/components/PageLayout';
import SettingsMenu from './components/SettingsMenu';

export default function MyPage() {
  return (
    <PageLayout 
      menu={<SettingsMenu />}
      breadcrumbs={['settingsTitle', 'myPage']}
    >
      {/* Contenu page */}
    </PageLayout>
  );
}
```

### StatusCard (Popup statut appareil)

**Fichier:** `src/common/components/StatusCard.jsx`

Popup affichant les infos appareil sur la carte.

Styles modifiables:
```javascript
const useStyles = makeStyles()((theme) => ({
  card: {
    pointerEvents: 'auto',
    width: theme.dimensions.popupMaxWidth,  // ← Largeur max
  },
  media: {
    height: theme.dimensions.popupImageHeight,  // ← Hauteur image
  },
  content: {
    maxHeight: theme.dimensions.cardContentMaxHeight,  // ← Hauteur contenu
  },
}));
```

### NavBar et SideNav

**Fichiers:**
- `src/common/components/NavBar.jsx` - Barre navigation
- `src/common/components/SideNav.jsx` - Menu latéral

```javascript
// NavBar
<AppBar position="sticky" color="inherit">
  <Toolbar>
    <IconButton>...</IconButton>
    <Typography variant="h6">{title}</Typography>
  </Toolbar>
</AppBar>

// SideNav
<List>
  {routes.map(route => (
    <ListItemButton component={Link} to={route.href}>
      <ListItemIcon>{route.icon}</ListItemIcon>
      <ListItemText primary={route.name} />
    </ListItemButton>
  ))}
</List>
```

---

## 🗺️ CARTE ET GÉOFENCES

### Couleurs géofences

**Fichier:** `src/map/draw/theme.js`

```javascript
export default [
  {
    id: 'gl-draw-polygon-fill-inactive',
    type: 'fill',
    paint: {
      'fill-color': '#3bb2d0',          // ← Couleur inactive
      'fill-outline-color': '#3bb2d0',
      'fill-opacity': 0.1,
    },
  },
  {
    id: 'gl-draw-polygon-fill-active',
    type: 'fill',
    paint: {
      'fill-color': '#fbb03b',          // ← Couleur active
      'fill-outline-color': '#fbb03b',
      'fill-opacity': 0.1,
    },
  },
  {
    id: 'gl-draw-polygon-stroke-inactive',
    type: 'line',
    paint: {
      'line-color': '#3bb2d0',          // ← Bordure inactive
      'line-width': 2,
    },
  },
  {
    id: 'gl-draw-polygon-stroke-active',
    type: 'line',
    paint: {
      'line-color': '#fbb03b',          // ← Bordure active
      'line-dasharray': [0.2, 2],       // Tirets
      'line-width': 2,
    },
  },
];
```

### Styles de carte

**Fichier:** `src/map/core/useMapStyles.js`

Configure les différents styles de carte (OpenStreetMap, Google Maps, Mapbox, etc.)

### URL carte personnalisée

Dans **Paramètres → Serveur → Preferences**:
- **Map Custom Label**: URL TMS
- **Map Overlay Custom**: URL couche overlay

Format: `https://tiles.provider.com/{z}/{x}/{y}.png`

Exemples:
```
OpenStreetMap:
https://tile.openstreetmap.org/{z}/{x}/{y}.png

USGS:
https://basemap.nationalmap.gov/arcrest/services/USGSTopo/MapServer/tile/{z}/{y}/{x}

Stamen:
https://tile.openstreetmap.de/tiles/osmde/{z}/{x}/{y}.png
```

---

## 📄 PAGES PERSONNALISABLES

### Login Pages

**Fichiers:**
- `src/login/LoginPage.jsx` - Formulaire connexion
- `src/login/RegisterPage.jsx` - Enregistrement
- `src/login/ResetPasswordPage.jsx` - Réinitialisation
- `src/login/LoginLayout.jsx` - Layout (sidebar + form)

### Main Page

**Fichiers:**
- `src/main/MainPage.jsx` - Page de suivi
- `src/main/MainMap.jsx` - Composant cartographique
- `src/main/MainToolbar.jsx` - Barre d'outils
- `src/main/DeviceList.jsx` - Liste appareils
- `src/main/EventsDrawer.jsx` - Panneau événements

### Settings Pages

**Fichiers:**
- `src/settings/ServerPage.jsx` - **Configuration serveur (couleurs, logos)**
- `src/settings/DevicesPage.jsx` - Gestion appareils
- `src/settings/UsersPage.jsx` - Gestion utilisateurs
- `src/settings/GroupsPage.jsx` - Groupes
- `src/settings/NotificationsPage.jsx` - Notifications
- `src/settings/CommandsPage.jsx` - Commandes

### Reports Pages

**Fichiers:**
- `src/reports/ChartReportPage.jsx` - Graphiques (utilise couleurs thème)
- `src/reports/PositionsReportPage.jsx` - Positions
- `src/reports/TripReportPage.jsx` - Trajets
- `src/reports/StopReportPage.jsx` - Arrêts
- `src/reports/EventReportPage.jsx` - Événements
- `src/reports/StatisticsPage.jsx` - Statistiques

### Other Pages

**Fichiers:**
- `src/other/ReplayPage.jsx` - Lecture trajectoire
- `src/other/StreamPage.jsx` - Diffusion vidéo
- `src/other/EmulatorPage.jsx` - Émulateur GPS
- `src/other/GeofencesPage.jsx` - Géofences

---

## 🚀 WORKFLOW COMPLET

### Scénario 1: Personnalisation simple (Interface)

```
1. Se connecter (admin)
2. Aller à Paramètres → Serveur
3. Remplir:
   - Nom du serveur
   - Couleur primaire: #FF5722
   - Couleur secondaire: #00BCD4
   - Logo: https://example.com/logo.png
4. Cliquer Save
5. Actualiser (F5)
```

### Scénario 2: Personnalisation code (Développeur)

```bash
# 1. Modifier fichiers
vim src/common/theme/palette.js
vim public/styles.css
vim src/resources/images/logo.svg

# 2. Tester en développement
npm start

# 3. Builder
npm run build

# 4. Déployer dossier 'build/' sur serveur web
# Ex: copier le contenu dans /var/www/traccar-web/

# 5. Actualiser navigateur
```

### Scénario 3: Personnalisation complète (Combiné)

```
Interface:
  ✓ Modifier couleurs, logos, titre
  ✓ Ajouter langue
  ✓ Configurer carte

Code:
  ✓ Modifier dimensions (dimensions.js)
  ✓ Ajouter animations (styles.css)
  ✓ Personnaliser composants (components.js)
  ✓ Modifier geofence colors (map/draw/theme.js)

Build:
  npm run build
  
Deploy:
  Copier 'build/' sur serveur
```

---

## ✅ CHECKLIST PERSONNALISATION

### Visuels
- [ ] Modifier couleur primaire
- [ ] Modifier couleur secondaire
- [ ] Upload logo
- [ ] Upload logo inversé (mode sombre)
- [ ] Remplacer favicon
- [ ] Remplacer icônes PWA
- [ ] Ajouter css personnalisé

### Textes
- [ ] Modifier titre serveur
- [ ] Modifier description serveur
- [ ] Personnaliser traductions
- [ ] Ajouter nouvelle langue

### Configuration
- [ ] Configurer API URL (vite.config.js)
- [ ] Configurer carte par défaut
- [ ] Configurer fuseau horaire
- [ ] Configurer unités

### Géofences & Carte
- [ ] Modifier couleurs géofences
- [ ] Ajouter URL carte custom
- [ ] Ajouter URL overlay custom

### Testing
- [ ] Tester en mode clair
- [ ] Tester en mode sombre
- [ ] Tester sur mobile
- [ ] Tester export Excel
- [ ] Tester toutes les langues

### Déploiement
- [ ] Build production
- [ ] Déployer sur serveur web
- [ ] Vérifier CORS si API distante
- [ ] Tester depuis https

---

## 📚 FICHIERS DE RÉFÉRENCE

| Élément | Fichier | Type |
|---------|---------|------|
| Couleurs | `src/common/theme/palette.js` | JS |
| Dimensions | `src/common/theme/dimensions.js` | JS |
| Composants MUI | `src/common/theme/components.js` | JS |
| Logo défaut | `src/resources/images/logo.svg` | SVG |
| Favicon | `public/favicon.ico` | ICO |
| Styles global | `public/styles.css` | CSS |
| Traductions | `src/resources/l10n/*.json` | JSON |
| HTML | `index.html` | HTML |
| Géofences | `src/map/draw/theme.js` | JS |
| LoginLayout | `src/login/LoginLayout.jsx` | JSX |
| Server Config | `src/settings/ServerPage.jsx` | JSX |
| Vite Config | `vite.config.js` | JS |

---

## 🔧 COMMANDES UTILES

```bash
# Installation
npm install

# Développement
npm start
# Accès: http://localhost:3000

# Build production
npm build
# Sortie: dossier 'build/'

# Linting
npm run lint

# Fixer linting
npm run lint:fix

# Générer icônes PWA
npm run generate-pwa-assets

# Déployer
# 1. npm run build
# 2. Copier contenu 'build/' vers serveur web
# 3. Configurer proxy API vers Traccar backend
```

---

## 💡 CONSEILS ET BONNES PRATIQUES

1. **Testez en développement d'abord** - Utilisez `npm start`
2. **Sauvegardez avant de modifier** - Versionnez vos changements
3. **Utilisez l'interface pour simple perso** - Plus facile pour couleurs/logos
4. **Modifiez le code pour changes avancés** - Dimensions, animations, etc.
5. **Testez sur mobile** - Utilisez DevTools (F12)
6. **Testez en mode sombre** - Vérifiez contraste et lisibilité
7. **Validez couleurs hex** - Format: #RRGGBB ou #RGB
8. **Optimisez images** - Logos doivent être < 100KB
9. **Testez toutes les langues** - Vérifiez textes UI
10. **Documentez vos changements** - Pour futures maintenances

---

## 📞 TROUBLESHOOTING

**Les couleurs ne changent pas?**
- Vérifier format hex (#RRGGBB)
- Actualiser navigateur (F5 ou Ctrl+Shift+Del)
- Vider cache navigateur
- Vérifier que colorPrimary/colorSecondary dans attributes

**Logo ne s'affiche pas?**
- Vérifier URL accessible (https si page https)
- Format: PNG, SVG, JPG
- Taille < 500KB
- Vérifier CORS si URL externe

**Modifications code ne prennent pas effet?**
- Arrêter serveur dev (Ctrl+C)
- `npm run build` (si production)
- Actualiser navigateur
- Vérifier pas d'erreurs console (F12)

**API non accessible?**
- Vérifier adresse serveur dans vite.config.js
- Vérifier CORS sur backend
- Vérifier firewall/ports

---

Créé pour Traccar Web v6.13.3
Dernière mise à jour: Novembre 2024
