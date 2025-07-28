# Claude Generated Code

**Task**: Objective:
Identify and upgrade all outdated dependencies, tools, and configurations in the project, ensuring compatibility and stability...

Tasks:

📦 Dependency Upgrades
Check for outdated packages (package.json, requirements.txt, etc.).

Upgrade to latest stable versions.

Flag any breaking changes and suggest necessary code updates.

⚙️ Tooling & Runtime
Review build tools, linters, and runtime environments (e.g., Node.js, Python).

Suggest config updates (e.g., .eslintrc, tsconfig.json) if needed.

🧪 Validation
Ensure changes don’t break the app — suggest or generate tests if applicable.

Recommend manual test areas if automation isn't possible.



**Generated on**: Mon Jul 28 15:39:50 UTC 2025

## Generated Content

```
I'll analyze the codebase and provide a comprehensive upgrade plan for your Ionic React TypeScript government billing/invoicing application.

## 📦 DEPENDENCY UPGRADES

### Major Issues Found:
1. **Capacitor 5.x** - Should upgrade to 6.x (latest stable)
2. **Ionic 7.0.0** - Should upgrade to 8.x (latest stable)
3. **React Router 5.x** - Should upgrade to 6.x (breaking changes)
4. **Ethers 5.6.0** - Should upgrade to 6.x (breaking changes)
5. **Outdated React types** - Need latest versions
6. **Missing ESLint configuration** - No .eslintrc file found
7. **Vite plugins** - Some outdated/deprecated

## FILES_TO_CREATE

### 1. .eslintrc.js
```javascript
module.exports = {
  root: true,
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'plugin:@typescript-eslint/recommended',
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: [
    'react',
    '@typescript-eslint',
    'react-hooks',
  ],
  rules: {
    'react/react-in-jsx-scope': 'off',
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/no-explicit-any': 'warn',
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'warn',
  },
  settings: {
    react: {
      version: 'detect',
    },
  },
};
```

### 2. .prettierrc.js
```javascript
module.exports = {
  semi: true,
  trailingComma: 'es5',
  singleQuote: true,
  printWidth: 80,
  tabWidth: 2,
  useTabs: false,
};
```

### 3. cypress.config.ts
```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    supportFile: false,
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
  },
  component: {
    devServer: {
      framework: 'vite',
      bundler: 'vite',
    },
  },
});
```

## FILES_TO_MODIFY

### 1. package.json
```json
{
  "name": "GovtInvoiceNew",
  "private": true,
  "version": "0.0.1",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "test.e2e": "cypress run",
    "test.unit": "vitest",
    "lint": "eslint src --ext .ts,.tsx --report-unused-disable-directives --max-warnings 0",
    "lint:fix": "eslint src --ext .ts,.tsx --fix",
    "format": "prettier --write src/**/*.{ts,tsx}",
    "type-check": "tsc --noEmit",
    "generate-pwa-assets": "pwa-assets-generator"
  },
  "dependencies": {
    "@capacitor/android": "^6.1.2",
    "@capacitor/app": "^6.0.1",
    "@capacitor/core": "^6.1.2",
    "@capacitor/haptics": "^6.0.1",
    "@capacitor/keyboard": "^6.0.2",
    "@capacitor/preferences": "^6.0.2",
    "@capacitor/status-bar": "^6.0.1",
    "@ionic/react": "^8.3.1",
    "@ionic/react-router": "^8.3.1",
    "@metamask/sdk-react": "^0.28.4",
    "@metamask/sdk-react-ui": "^0.28.4",
    "capacitor-email-composer": "^6.0.2",
    "ethers": "^6.13.2",
    "ionicons": "^7.4.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router": "^6.26.1",
    "react-router-dom": "^6.26.1"
  },
  "devDependencies": {
    "@capacitor/cli": "^6.1.2",
    "@eslint/js": "^9.9.1",
    "@testing-library/dom": "^10.4.0",
    "@testing-library/jest-dom": "^6.5.0",
    "@testing-library/react": "^16.0.1",
    "@testing-library/user-event": "^14.5.2",
    "@types/react": "^18.3.5",
    "@types/react-dom": "^18.3.0",
    "@typescript-eslint/eslint-plugin": "^8.4.0",
    "@typescript-eslint/parser": "^8.4.0",
    "@vite-pwa/assets-generator": "^0.2.6",
    "@vitejs/plugin-legacy": "^5.4.2",
    "@vitejs/plugin-react": "^4.3.1",
    "cypress": "^13.14.2",
    "eslint": "^9.9.1",
    "eslint-plugin-react": "^7.35.2",
    "eslint-plugin-react-hooks": "^4.6.2",
    "jsdom": "^25.0.0",
    "prettier": "^3.3.3",
    "typescript": "^5.5.4",
    "vite": "^5.4.3",
    "vite-plugin-pwa": "^0.20.5",
    "vitest": "^2.0.5"
  }
}
```

### 2. vite.config.ts
```typescript
import legacy from '@vitejs/plugin-legacy';
import react from '@vitejs/plugin-react';
import { defineConfig } from 'vite';
import { VitePWA } from 'vite-plugin-pwa';

export default defineConfig({
  plugins: [
    react(),
    legacy({
      targets: ['defaults', 'not IE 11'],
    }),
    VitePWA({
      registerType: 'autoUpdate',
      workbox: {
        globPatterns: ['**/*.{js,css,html,ico,png,svg}'],
      },
      includeAssets: ['favicon.ico', 'apple-touch-icon.png', 'maskable-icon.png'],
      manifest: {
        name: 'Government Invoice Suite',
        short_name: 'GovInvoice',
        description: 'Government billing and invoicing application',
        theme_color: '#ffffff',
        icons: [
          {
            src: 'pwa-192x192.png',
            sizes: '192x192',
            type: 'image/png',
          },
          {
            src: 'pwa-512x512.png',
            sizes: '512x512',
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
  define: {
    global: 'globalThis',
  },
  server: {
    port: 3000,
  },
  build: {
    outDir: 'dist',
  },
});
```

### 3. tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "useDefineForClassFields": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "include": [
    "src",
    "vite.config.ts"
  ],
  "references": [
    {
      "path": "./tsconfig.node.json"
    }
  ]
}
```

### 4. tsconfig.node.json
```json
{
  "compilerOptions": {
    "composite": true,
    "skipLibCheck": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "types": ["node"]
  },
  "include": ["vite.config.ts"]
}
```

### 5. capacitor.config.ts
```typescript
import { CapacitorConfig } from '@capacitor/cli';

const config: CapacitorConfig = {
  appId: 'io.ionic.govtinvoice',
  appName: 'GovtInvoiceNew',
  webDir: 'dist',
  server: {
    androidScheme: 'https'
  },
  plugins: {
    SplashScreen: {
      launchShowDuration: 3000,
      launchAutoHide: true,
      androidSplashResourceName: 'splash',
      androidScaleType: 'CENTER_CROP',
      showSpinner: false,
      androidSpinnerStyle: 'large',
      iosSpinnerStyle: 'small',
      spinnerColor: '#999999',
      splashFullScreen: true,
      splashImmersive: true,
    },
  },
};

export default config;
```

### 6. src/App.tsx (Updated for Router v6)
```typescript
import { IonApp, IonRouterOutlet, setupIonicReact } from '@ionic/react';
import { IonReactRouter } from '@ionic/react-router';
import { Route, Routes } from 'react-router-dom';
import Home from './pages/Home';

/* Core CSS required for Ionic components to work properly */
import '@ionic/react/css/core.css';

/* Basic CSS for apps built with Ionic */
import '@ionic/react/css/normalize.css';
import '@ionic/react/css/structure.css';
import '@ionic/react/css/typography.css';

/* Optional CSS utils that can be commented out */
import '@ionic/react/css/padding.css';
import '@ionic/react/css/float-elements.css';
import '@ionic/react/css/text-alignment.css';
import '@ionic/react/css/text-transformation.css';
import '@ionic/react/css/flex-utils.css';
import '@ionic/react/css/display.css';

/* Theme variables */
import './theme/variables.css';

setupIonicReact();

const App: React.FC = () => (
  <IonApp>
    <IonReactRouter>
      <IonRouterOutlet>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/home" element={<Home />} />
        </Routes>
      </IonRouterOutlet>
    </IonReactRouter>
  </IonApp>
);

export default App;
```

### 7. src/main.tsx (Updated for new ethers)
```typescript
import React from 'react';
import { createRoot } from 'react-dom/client';
import { MetaMaskProvider } from '@metamask/sdk-react';
import App from './App';

const container = document.getElementById('root');
const root = createRoot(container!);

root.render(
  <React.StrictMode>
    <MetaMaskProvider
      debug={false}
      sdkOptions={{
        dappMetadata: {
          name: 'Government Invoice Suite',
          url: window.location.href,
        },
        infuraAPIKey: import.meta.env.VITE_INFURA_API_KEY,
      }}
    >
      <App />
    </MetaMaskProvider>
  </React.StrictMode>
);
```

### 8. src/components/Storage/LocalStorage.ts (Updated for Capacitor 6)
```typescript
import { Preferences } from '@capacitor/preferences';

export class File {
  created: string;
  modified: string;
  name: string;
  content: string;
  billType: number;

  constructor(
    created: string,
    modified: string,
    content: string,
    name: string,
    billType: number
  ) {
    this.created = created;
    this.modified = modified;
    this.content = content;
    this.name = name;
    this.billType = billType;
  }
}

export class Local {
  _saveFile = async (file: File): Promise<void> => {
    try {
      const data = {
        created: file.created,
        modified: file.modified,
        content: file.content,
        billType: file.billType,
      };
      
      await Preferences.set({
        key: file.name,
        value: JSON.stringify(data),
      });
    } catch (error) {
      console.error('Error saving file:', error);
      throw new Error('Failed to save file');
    }
  };

  _getFile = async (name: string): Promise<File | null> => {
    try {
      const { value } = await Preferences.get({ key: name });
      
      if (!value) {
        return null;
      }
      
      const data = JSON.parse(value);
      return new File(
        data.created,
        data.modified,
        data.content,
        name,
        data.billType
      );
    } catch (error) {
      console.error('Error getting file:', error);
      return null;
    }
  };

  _deleteFile = async (name: string): Promise<void> => {
    try {
      await Preferences.remove({ key: name });
    } catch (error) {
      console.error('Error deleting file:', error);
      throw new Error('Failed to delete file');
    }
  };

  _getAllFiles = async (): Promise<File[]> => {
    try {
      const { keys } = await Preferences.keys();
      const files: File[] = [];
      
      for (const key of keys) {
        const file = await this._getFile(key);
        if (file) {
          files.push(file);
        }
      }
      
      return files;
    } catch (error) {
      console.error('Error getting all files:', error);
      return [];
    }
  };
}
```

### 9. vitest.config.ts (New test configuration)
```typescript
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./src/test/setup.ts'],
  },
});
```

### 10. src/test/setup.ts (New test setup)
```typescript
import '@testing-library/jest-dom';
```

## 🚨 BREAKING CHANGES & MIGRATION NOTES

### 1. React Router 6 Migration
- Replace `useHistory` with `useNavigate`
- Replace `<Switch>` with `<Routes>`
- Replace `<Route component={Component} />` with `<Route element={<Component />} />`

### 2. Ethers v6 Migration
- `ethers.utils` is now `ethers`
- `ethers.providers` is now `ethers.providers`
- Contract instantiation syntax changed

### 3. Capacitor 6 Migration
- Some plugin APIs have changed
- Update Android/iOS native projects

### 4. Ionic 8 Migration
- Some component props have changed
- CSS updates may be needed

## 🧪 VALIDATION & TESTING

### Test Areas to Manually Validate:
1. **Spreadsheet functionality** - Test all invoice templates
2. **File storage** - Create, save, load, delete files
3. **MetaMask integration** - Connect wallet, sign transactions
4. **Email composer** - Send invoices via email
5. **PWA functionality** - Install, offline usage
6. **Mobile responsiveness** - Test on different screen sizes
7. **Print functionality** - Test invoice printing

### Automated Tests to Add:
```typescript
// src/components/__tests__/LocalStorage.test.ts
import { describe, it, expect, beforeEach } from 'vitest';
import { Local, File } from '../Storage/LocalStorage';

describe('LocalStorage', () => {
  let storage: Local;

  beforeEach(() => {
    storage = new Local();
  });

  it('should save and retrieve files', async () => {
    const file = new File(
      new Date().toString(),
      new Date().toString(),
      'test content',
      'test-file',
      1
    );

    await storage._saveFile(file);
    const retrieved = await storage._getFile('test-file');

    expect(retrieved).toBeTruthy();
    expect(retrieved?.name).toBe('test-file');
    expect(retrieved?.content).toBe('test content');
  });
});
```

## 📋 INSTRUCTIONS

### 1. Pre-upgrade Steps:
```bash
# Backup current project
cp -r . ../project-backup

# Clear node_modules and lock files
rm -rf node_modules package-lock.json yarn.lock

# Clear Capacitor cache
npx cap clean
```

### 2. Install Dependencies:
```bash
npm install
```

### 3. Update Capacitor:
```bash
npx cap sync
npx cap update
```

### 4. Update Android/iOS Projects:
```bash
# Android
npx cap open android
# Update Android Studio and dependencies

# iOS
npx cap open ios
# Update Xcode project settings
```

### 5. Run Tests:
```bash
npm run lint
npm run type-check
npm run test.unit
npm run build
```

### 6. Test on Device:
```bash
npm run dev
# Test in browser first

npx cap run android
npx cap run ios
# Test on actual devices
```

This upgrade plan addresses all major outdated dependencies while maintaining backward compatibility where possible. The breaking changes are clearly documented with migration paths provided.
```
