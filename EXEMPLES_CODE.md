# 💻 Traccar Web - Guide Pratique de Modifications

Guide avec **exemples de code réels** pour personnaliser l'application.

---

## 🎨 1. Changer les couleurs

### Localisation: `src/common/theme/palette.js`

**Avant:**
```javascript
export default (server, darkMode) => ({
  mode: darkMode ? 'dark' : 'light',
  background: {
    default: darkMode ? grey[900] : grey[50],
  },
  primary: {
    main:
      validatedColor(server?.attributes?.colorPrimary) || 
      (darkMode ? indigo[200] : indigo[900]),
  },
  secondary: {
    main:
      validatedColor(server?.attributes?.colorSecondary) || 
      (darkMode ? green[200] : green[800]),
  },
  neutral: {
    main: grey[500],
  },
  geometry: {
    main: '#3bb2d0',
  },
  alwaysDark: {
    main: grey[900],
  },
});
```

**Après (Orange et Bleu personnalisés):**
```javascript
export default (server, darkMode) => ({
  mode: darkMode ? 'dark' : 'light',
  background: {
    default: darkMode ? grey[900] : grey[50],
  },
  primary: {
    main:
      validatedColor(server?.attributes?.colorPrimary) || 
      (darkMode ? '#FF8C42' : '#E67E22'),  // 🔶 Orange chaud
  },
  secondary: {
    main:
      validatedColor(server?.attributes?.colorSecondary) || 
      (darkMode ? '#3498DB' : '#2980B9'),  // 🔵 Bleu
  },
  neutral: {
    main: grey[500],
  },
  geometry: {
    main: '#E74C3C',  // 🔴 Rouge pour géométrie
  },
  alwaysDark: {
    main: grey[900],
  },
});
```

### Palettes de couleurs suggérées:

**Couleur entreprise (exemple: Bleu & Or)**
```javascript
primary: { main: '#1E3A8A' },      // Bleu profond
secondary: { main: '#D97706' },    // Or/Amber
geometry: { main: '#10B981' },     // Vert (OK)
```

**Couleur entreprise (exemple: Noir & Vert)**
```javascript
primary: { main: '#1F2937' },      // Noir
secondary: { main: '#059669' },    // Vert
geometry: { main: '#F59E0B' },     // Ambre (alerte)
```

**Couleur neutre (exemple: Gris & Accent)**
```javascript
primary: { main: '#4B5563' },      // Gris bleuté
secondary: { main: '#7C3AED' },    // Violet
geometry: { main: '#06B6D4' },     // Cyan
```

---

## 🎨 2. Remplacer le logo

### Option A: Fichier SVG par défaut

**Localisation:** `src/resources/images/logo.svg`

**Remplacer complètement par:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 100">
  <!-- Votre logo SVG ici -->
  <circle cx="50" cy="50" r="40" fill="currentColor"/>
  <text x="100" y="50" font-size="24" fill="currentColor">
    MON LOGO
  </text>
</svg>
```

### Option B: Logo externe avec composant

**Fichier:** `src/login/LogoImage.jsx`

**Modifier pour utiliser URL externe:**
```javascript
import { useTheme, useMediaQuery } from '@mui/material';
import { useSelector } from 'react-redux';
import { makeStyles } from 'tss-react/mui';

const useStyles = makeStyles()((theme) => ({
  image: {
    alignSelf: 'center',
    maxWidth: '240px',
    maxHeight: '120px',
    width: 'auto',
    height: 'auto',
    margin: theme.spacing(2),
  },
}));

const LogoImage = ({ color }) => {
  const theme = useTheme();
  const { classes } = useStyles();

  const expanded = !useMediaQuery(theme.breakpoints.down('lg'));

  // Vos URLs de logos
  const LOGO_URL = 'https://votre-serveur.com/logo.png';
  const LOGO_INVERTED_URL = 'https://votre-serveur.com/logo-white.png';

  // Utiliser aussi les logos serveur si disponibles
  const logo = useSelector((state) => state.session.server.attributes?.logo) || LOGO_URL;
  const logoInverted = useSelector((state) => state.session.server.attributes?.logoInverted) || LOGO_INVERTED_URL;

  if (logo) {
    if (expanded && logoInverted) {
      return <img className={classes.image} src={logoInverted} alt="Logo" />;
    }
    return <img className={classes.image} src={logo} alt="Logo" />;
  }
  
  // Fallback: afficher texte coloré
  return <div className={classes.image} style={{ color, fontSize: '32px' }}>
    MA SOCIÉTÉ
  </div>;
};

export default LogoImage;
```

### Option C: Logo avec gradient personnalisé

**Créer composant personnalisé:**
```javascript
const LogoImage = ({ color }) => {
  const { classes } = useStyles();

  return (
    <svg 
      className={classes.image} 
      viewBox="0 0 200 100" 
      xmlns="http://www.w3.org/2000/svg"
    >
      <defs>
        <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" style={{ stopColor: '#FF6B6B', stopOpacity: 1 }} />
          <stop offset="100%" style={{ stopColor: '#4ECDC4', stopOpacity: 1 }} />
        </linearGradient>
      </defs>
      <rect width="200" height="100" fill="url(#grad1)" rx="10" />
      <text x="100" y="60" textAnchor="middle" fill="white" fontSize="32" fontWeight="bold">
        GPS TRACKER
      </text>
    </svg>
  );
};

export default LogoImage;
```

---

## 📝 3. Traduire textes

### Localisation: `src/resources/l10n/`

### Ajouter langue française complète

**Fichier:** `src/resources/l10n/fr.json`

**Exemple de contenu complet (excerpt):**
```json
{
  "sharedLoading": "Chargement...",
  "sharedHide": "Masquer",
  "sharedSave": "Enregistrer",
  "sharedSaved": "Enregistré",
  "sharedUpload": "Télécharger",
  "sharedSet": "Définir",
  "sharedAccept": "Accepter",
  "sharedCancel": "Annuler",
  "sharedCopy": "Copier",
  "sharedAdd": "Ajouter",
  "sharedEdit": "Modifier",
  "sharedRemove": "Supprimer",
  "sharedRemoveConfirm": "Supprimer cet élément?",
  "sharedNoData": "Aucune donnée",
  "sharedSubject": "Sujet",
  "sharedYes": "Oui",
  "sharedNo": "Non",
  
  "loginTitle": "Connexion",
  "loginEmail": "Email",
  "loginPassword": "Mot de passe",
  "loginRegister": "S'enregistrer",
  "loginReset": "Réinitialiser mot de passe",
  "loginLogin": "Se connecter",
  
  "deviceName": "Nom du véhicule",
  "deviceImei": "IMEI",
  "deviceStatus": "Statut",
  "deviceLatitude": "Latitude",
  "deviceLongitude": "Longitude",
  "deviceSpeed": "Vitesse",
  "deviceCourse": "Direction",
  "deviceAltitude": "Altitude",
  "deviceAccuracy": "Précision",
  
  "settingsTitle": "Paramètres",
  "settingsServer": "Serveur",
  "settingsUsers": "Utilisateurs",
  "settingsDevices": "Véhicules",
  "settingsGroups": "Groupes",
  "settingsGeofences": "Géofences",
  "settingsNotifications": "Notifications",
  
  "reportTitle": "Rapports",
  "reportTrips": "Trajets",
  "reportStops": "Arrêts",
  "reportEvents": "Événements",
  "reportPositions": "Positions",
  "reportSummary": "Résumé",
  
  "mainTitle": "Suivi GPS",
  "mainMap": "Carte",
  "mainList": "Liste"
}
```

### Créer nouvelle langue

1. **Copier** `src/resources/l10n/en.json`
2. **Vers** `src/resources/l10n/xx.json` (xx = code langue)
3. **Traduire** chaque valeur
4. **Enregistrer** et tester

**Codes langue courants:**
- `de.json` - Allemand
- `it.json` - Italien
- `es.json` - Espagnol
- `pt.json` - Portugais
- `ru.json` - Russe
- `zh.json` - Chinois
- `ja.json` - Japonais

### Modifier texte spécifique

**Exemple: Changer "Device" en "Vehicle"**

**Avant:**
```json
"sharedDevice": "Device",
"deviceName": "Device Name",
"deviceStatus": "Device Status"
```

**Après:**
```json
"sharedDevice": "Véhicule",
"deviceName": "Nom du Véhicule",
"deviceStatus": "Statut du Véhicule"
```

---

## 🎨 4. Modifier styles Material-UI

### Localisation: `src/common/theme/components.js`

**Changer couleurs des boutons:**
```javascript
export default {
  MuiButton: {
    styleOverrides: {
      // Bouton bleu (primary)
      containedPrimary: ({ theme }) => ({
        backgroundColor: theme.palette.primary.main,
        color: '#fff',
        '&:hover': {
          backgroundColor: theme.palette.primary.dark || theme.palette.primary.main,
          opacity: 0.9,
        },
      }),
      // Bouton vert (secondary)
      containedSecondary: ({ theme }) => ({
        backgroundColor: theme.palette.secondary.main,
        color: '#fff',
        '&:hover': {
          backgroundColor: theme.palette.secondary.dark || theme.palette.secondary.main,
          opacity: 0.9,
        },
      }),
      // Boutons outline
      outlined: {
        borderColor: '#ccc',
      },
      // Taille moyenne
      sizeMedium: {
        height: '40px',
        fontSize: '16px',
      },
      // Taille petite
      sizeSmall: {
        height: '32px',
        fontSize: '14px',
      },
    },
  },
  
  MuiTextField: {
    styleOverrides: {
      root: ({ theme }) => ({
        '& .MuiOutlinedInput-root': {
          backgroundColor: theme.palette.background.default,
          '&:hover fieldset': {
            borderColor: theme.palette.primary.main,
          },
        },
      }),
    },
  },
  
  MuiAppBar: {
    styleOverrides: {
      root: ({ theme }) => ({
        backgroundColor: theme.palette.primary.main,
        boxShadow: '0 2px 8px rgba(0,0,0,0.1)',
      }),
    },
  },
  
  MuiCard: {
    styleOverrides: {
      root: {
        borderRadius: '12px',
        boxShadow: '0 2px 12px rgba(0,0,0,0.1)',
      },
    },
  },
};
```

---

## 📄 5. Ajouter polices personnalisées

### Localisation: `public/styles.css`

**Ajouter Google Fonts:**
```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&family=Inter:wght@400;600;700&display=swap');

body {
  font-family: 'Inter', 'Roboto', sans-serif;
}

h1, h2, h3, h4, h5, h6 {
  font-family: 'Roboto', sans-serif;
  font-weight: 700;
}
```

**Ou personnalisée (TTF/WOFF):**
```css
@font-face {
  font-family: 'MaPolicePerso';
  src: url('/fonts/mapolice.woff2') format('woff2'),
       url('/fonts/mapolice.woff') format('woff');
  font-weight: 400;
  font-display: swap;
}

@font-face {
  font-family: 'MaPolicePerso';
  src: url('/fonts/mapolice-bold.woff2') format('woff2');
  font-weight: 700;
}

body {
  font-family: 'MaPolicePerso', sans-serif;
}
```

---

## 🔧 6. Configurer proxy API

### Localisation: `vite.config.js`

**Configuration actuelle:**
```javascript
export default defineConfig(() => ({
  server: {
    port: 3000,
    proxy: {
      '/api/socket': 'ws://10.0.0.222:8082',
      '/api': 'http://10.0.0.222:8082',
    },
  },
  // ...
}));
```

**Personnaliser serveur:**
```javascript
export default defineConfig(() => ({
  server: {
    port: 3000,
    proxy: {
      // WebSocket (temps réel)
      '/api/socket': {
        target: 'ws://localhost:8082',  // Remplacer par votre serveur
        ws: true,
      },
      // REST API
      '/api': {
        target: 'http://localhost:8082',  // Remplacer par votre serveur
        changeOrigin: true,
        rewrite: (path) => path,  // Garder le path
      },
    },
  },
  build: {
    outDir: 'build',
  },
  // ...
}));
```

**Pour production (Nginx proxy):**
```nginx
server {
    listen 80;
    server_name traccar.example.com;

    # Frontend (fichiers statiques)
    location / {
        root /var/www/traccar-web/build;
        try_files $uri /index.html;
    }

    # Backend API
    location /api {
        proxy_pass http://localhost:8082;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # WebSocket
    location /api/socket {
        proxy_pass http://localhost:8082;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}
```

---

## 🎨 7. Créer thème personnalisé complet

**Créer nouveau fichier:** `src/common/theme/myBrandTheme.js`

```javascript
import { createTheme } from '@mui/material/styles';
import { colors } from '@mui/material';

// Définir palette personnalisée
export const myBrandPalette = {
  primary: '#2C3E50',      // Bleu très foncé
  secondary: '#E74C3C',    // Rouge coral
  success: '#27AE60',      // Vert
  warning: '#F39C12',      // Orange
  error: '#E74C3C',        // Rouge
  info: '#3498DB',         // Bleu ciel
  background: '#ECF0F1',   // Gris très clair
  text: '#2C3E50',         // Texte foncé
};

export default createTheme({
  palette: {
    primary: {
      main: myBrandPalette.primary,
      light: '#34495E',
      dark: '#1A252F',
    },
    secondary: {
      main: myBrandPalette.secondary,
      light: '#EC7063',
      dark: '#C0392B',
    },
    success: {
      main: myBrandPalette.success,
    },
    warning: {
      main: myBrandPalette.warning,
    },
    error: {
      main: myBrandPalette.error,
    },
    background: {
      default: myBrandPalette.background,
      paper: '#FFFFFF',
    },
    text: {
      primary: myBrandPalette.text,
      secondary: '#7F8C8D',
    },
  },
  typography: {
    fontFamily: '"Roboto", "Helvetica", "Arial", sans-serif',
    h1: {
      fontSize: '32px',
      fontWeight: 700,
      color: myBrandPalette.text,
    },
    h2: {
      fontSize: '28px',
      fontWeight: 600,
      color: myBrandPalette.text,
    },
    body1: {
      fontSize: '14px',
      color: myBrandPalette.text,
    },
  },
  shape: {
    borderRadius: 8,
  },
});
```

---

## 🎨 8. Créer composant logo branding

**Créer:** `src/components/BrandLogo.jsx`

```javascript
import { makeStyles } from 'tss-react/mui';

const useStyles = makeStyles()((theme) => ({
  container: {
    display: 'flex',
    alignItems: 'center',
    gap: theme.spacing(1),
  },
  icon: {
    width: '40px',
    height: '40px',
    borderRadius: '8px',
    backgroundColor: theme.palette.primary.main,
    display: 'flex',
    alignItems: 'center',
    justifyContent: 'center',
    color: '#fff',
    fontWeight: 700,
    fontSize: '24px',
  },
  text: {
    display: 'flex',
    flexDirection: 'column',
  },
  title: {
    fontWeight: 700,
    fontSize: '16px',
    color: theme.palette.primary.main,
    lineHeight: 1,
  },
  subtitle: {
    fontSize: '12px',
    color: theme.palette.text.secondary,
    lineHeight: 1,
  },
}));

const BrandLogo = ({ showText = true }) => {
  const { classes } = useStyles();

  return (
    <div className={classes.container}>
      <div className={classes.icon}>📍</div>
      {showText && (
        <div className={classes.text}>
          <div className={classes.title}>GPS Tracker</div>
          <div className={classes.subtitle}>Real-time Tracking</div>
        </div>
      )}
    </div>
  );
};

export default BrandLogo;
```

---

## 🚀 9. Build et déploiement

### Script package.json

**Fichier:** `package.json`

```json
{
  "name": "traccar-web",
  "version": "6.13.3",
  "type": "module",
  "scripts": {
    "start": "vite --host",
    "build": "vite build",
    "build:prod": "vite build --mode production",
    "preview": "vite preview",
    "lint": "eslint --max-warnings 0 .",
    "lint:fix": "eslint --fix --max-warnings 0 .",
    "generate-pwa": "pwa-assets-generator --preset minimal public/logo.svg"
  }
}
```

### Build personnalisé

**Créer:** `build.sh`

```bash
#!/bin/bash

echo "🔨 Building Traccar Web..."

# Installer dépendances
npm ci

# Linter
npm run lint:fix

# Build production
npm run build:prod

# Optionnel: Copier vers serveur
# scp -r build/* user@server:/var/www/traccar-web/

echo "✅ Build terminé! Fichiers dans build/"
```

### Utiliser:
```bash
chmod +x build.sh
./build.sh
```

---

## 📦 10. Exemple complet de personnalisation

**Fichier:** `src/common/theme/customTheme.js`

```javascript
import { createTheme } from '@mui/material/styles';

export const customTheme = (server, darkMode) => createTheme({
  // 🎨 Couleurs personnalisées
  palette: {
    mode: darkMode ? 'dark' : 'light',
    primary: {
      main: server?.attributes?.colorPrimary || '#FF6B35',  // Orange
      light: '#FF8C5A',
      dark: '#E55100',
    },
    secondary: {
      main: server?.attributes?.colorSecondary || '#004E89',  // Bleu
      light: '#1A659E',
      dark: '#003A6F',
    },
    success: {
      main: '#06A77D',
    },
    warning: {
      main: '#F18F01',
    },
    error: {
      main: '#D62828',
    },
    info: {
      main: '#0077B6',
    },
    background: {
      default: darkMode ? '#121212' : '#F5F7FA',
      paper: darkMode ? '#1E1E1E' : '#FFFFFF',
    },
  },

  // 📝 Typographie
  typography: {
    fontFamily: '"Inter", "Roboto", sans-serif',
    h1: {
      fontSize: '32px',
      fontWeight: 700,
      letterSpacing: '-0.5px',
    },
    h2: {
      fontSize: '24px',
      fontWeight: 600,
    },
    h3: {
      fontSize: '20px',
      fontWeight: 600,
    },
    body1: {
      fontSize: '14px',
      lineHeight: 1.6,
    },
  },

  // 🎨 Overrides componants
  components: {
    MuiButton: {
      styleOverrides: {
        root: {
          textTransform: 'none',
          fontWeight: 600,
          borderRadius: '8px',
          transition: 'all 0.2s',
        },
        containedPrimary: {
          boxShadow: '0 4px 12px rgba(255,107,53,0.3)',
          '&:hover': {
            boxShadow: '0 6px 16px rgba(255,107,53,0.4)',
          },
        },
      },
    },
    MuiCard: {
      styleOverrides: {
        root: {
          borderRadius: '12px',
          boxShadow: '0 2px 8px rgba(0,0,0,0.08)',
          '&:hover': {
            boxShadow: '0 4px 16px rgba(0,0,0,0.12)',
          },
        },
      },
    },
    MuiAppBar: {
      styleOverrides: {
        root: {
          boxShadow: '0 2px 8px rgba(0,0,0,0.1)',
        },
      },
    },
  },

  // 📐 Dimensions
  dimensions: {
    drawerWidthDesktop: 320,
    drawerWidthTablet: 280,
  },
});

export default customTheme;
```

---

## ✨ Résumé des fichiers clés

| Fichier | Ligne de modification | Effet |
|---------|----------------------|--------|
| `src/common/theme/palette.js` | `primary.main`, `secondary.main` | Couleurs boutons, headers |
| `src/resources/images/logo.svg` | Tout le fichier | Logo partout dans app |
| `src/resources/l10n/fr.json` | Toutes valeurs | Textes français |
| `public/styles.css` | Ajouter CSS | Styles globaux |
| `public/favicon.ico` | Remplacer fichier | Icône navigateur |
| `vite.config.js` | `proxy` | Adresse API backend |
| `src/common/theme/components.js` | Overrides MUI | Style des composants MUI |

---

## 🎯 Plan d'action recommandé

1. **Jour 1:**
   - [ ] Modifier couleurs (palette.js)
   - [ ] Remplacer logo SVG
   - [ ] Tester en dev

2. **Jour 2:**
   - [ ] Traduire textes français
   - [ ] Changer favicon
   - [ ] Configurer proxy API

3. **Jour 3:**
   - [ ] Ajuster thème Material-UI
   - [ ] Tester responsive mobile
   - [ ] Tester mode sombre

4. **Jour 4:**
   - [ ] Build production
   - [ ] Déployer
   - [ ] Configurer serveur proxy (Nginx)

---

## 🐛 Debugging

### Vérifier couleurs appliquées
```javascript
// Dans console navigateur
const theme = document.body.style;
console.log(theme);  // Voir toutes les couleurs appliquées
```

### Voir styles compilés
```javascript
// F12 → Elements → Voir CSS appliqué
// Vérifier: color, background-color, border, etc.
```

### Voir fichier JSON chargé
```javascript
// En console Redux DevTools
// State → session → server → attributes
// Voir si colorPrimary, logo, etc. sont présents
```

---

**Bon courage avec vos modifications! 🚀**
