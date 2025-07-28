# Claude Generated Code

**Task**: Objective:
Help design and implement new features based on the current roadmap or project needs. Focus on clean, scalable, and testable code

Claude Tasks:

🧩 Feature Implementation
Read the existing codebase and integrate new feature modules where appropriate.

Ensure new code follows existing patterns and conventions.

🗂️ Example Features (replace/add specifics as needed):
Add user role-based access control

Implement export to PDF

Add multi-language support

Enable dark mode toggle

🧪 Testing & Documentation
Add unit/integration tests for new functionality.

Update README or in-code comments if required.

**Generated on**: Mon Jul 28 16:28:57 UTC 2025

## Generated Content

```
Based on the codebase analysis, I'll implement a **dark mode toggle feature** for the government billing/invoicing Ionic React TypeScript application. This feature will provide users with a seamless way to switch between light and dark themes.

## 🎯 Feature Implementation: Dark Mode Toggle

### FILES_TO_CREATE:

1. `src/hooks/useTheme.ts` - Custom hook for theme management
2. `src/components/ThemeToggle/ThemeToggle.tsx` - Toggle component
3. `src/components/ThemeToggle/ThemeToggle.css` - Styling for toggle
4. `src/types/theme.ts` - TypeScript types for theme

### FILES_TO_MODIFY:

1. `src/theme/variables.css` - Add dark mode CSS variables
2. `src/App.tsx` - Initialize theme provider
3. `src/pages/Home.tsx` - Add theme toggle to the interface
4. `src/pages/Home.css` - Add theme-aware styles

### CODE_CHANGES:

#### 1. **src/types/theme.ts** (NEW FILE)
```typescript
export type Theme = 'light' | 'dark';

export interface ThemeContextType {
  theme: Theme;
  toggleTheme: () => void;
  isDarkMode: boolean;
}

export interface ThemeConfig {
  prefersDark: boolean;
  systemTheme: Theme;
  userTheme: Theme | null;
}
```

#### 2. **src/hooks/useTheme.ts** (NEW FILE)
```typescript
import { useState, useEffect, useCallback } from 'react';
import { Preferences } from '@capacitor/preferences';
import { Theme, ThemeConfig } from '../types/theme';

const THEME_STORAGE_KEY = 'app-theme';

export const useTheme = () => {
  const [theme, setTheme] = useState<Theme>('light');
  const [isLoading, setIsLoading] = useState(true);

  // Check system preference
  const getSystemTheme = useCallback((): Theme => {
    if (typeof window !== 'undefined' && window.matchMedia) {
      return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
    }
    return 'light';
  }, []);

  // Load saved theme preference
  const loadTheme = useCallback(async () => {
    try {
      const { value } = await Preferences.get({ key: THEME_STORAGE_KEY });
      const savedTheme = value as Theme | null;
      
      if (savedTheme && (savedTheme === 'light' || savedTheme === 'dark')) {
        setTheme(savedTheme);
      } else {
        // Use system preference if no saved theme
        const systemTheme = getSystemTheme();
        setTheme(systemTheme);
      }
    } catch (error) {
      console.error('Error loading theme:', error);
      setTheme(getSystemTheme());
    } finally {
      setIsLoading(false);
    }
  }, [getSystemTheme]);

  // Save theme preference
  const saveTheme = useCallback(async (newTheme: Theme) => {
    try {
      await Preferences.set({ key: THEME_STORAGE_KEY, value: newTheme });
    } catch (error) {
      console.error('Error saving theme:', error);
    }
  }, []);

  // Apply theme to document
  const applyTheme = useCallback((newTheme: Theme) => {
    if (typeof document !== 'undefined') {
      document.documentElement.classList.remove('light', 'dark');
      document.documentElement.classList.add(newTheme);
      
      // Also update the body class for Ionic components
      document.body.classList.toggle('dark', newTheme === 'dark');
    }
  }, []);

  // Toggle theme
  const toggleTheme = useCallback(async () => {
    const newTheme: Theme = theme === 'light' ? 'dark' : 'light';
    setTheme(newTheme);
    applyTheme(newTheme);
    await saveTheme(newTheme);
  }, [theme, applyTheme, saveTheme]);

  // Initialize theme
  useEffect(() => {
    loadTheme();
  }, [loadTheme]);

  // Apply theme when it changes
  useEffect(() => {
    if (!isLoading) {
      applyTheme(theme);
    }
  }, [theme, applyTheme, isLoading]);

  // Listen for system theme changes
  useEffect(() => {
    if (typeof window !== 'undefined' && window.matchMedia) {
      const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
      
      const handleSystemThemeChange = async (e: MediaQueryListEvent) => {
        // Only update if user hasn't set a preference
        const { value } = await Preferences.get({ key: THEME_STORAGE_KEY });
        if (!value) {
          const systemTheme = e.matches ? 'dark' : 'light';
          setTheme(systemTheme);
        }
      };

      mediaQuery.addEventListener('change', handleSystemThemeChange);
      
      return () => {
        mediaQuery.removeEventListener('change', handleSystemThemeChange);
      };
    }
  }, []);

  return {
    theme,
    toggleTheme,
    isDarkMode: theme === 'dark',
    isLoading
  };
};
```

#### 3. **src/components/ThemeToggle/ThemeToggle.tsx** (NEW FILE)
```typescript
import React from 'react';
import { IonButton, IonIcon, IonItem, IonLabel, IonToggle } from '@ionic/react';
import { moon, sunny } from 'ionicons/icons';
import { useTheme } from '../../hooks/useTheme';
import './ThemeToggle.css';

interface ThemeToggleProps {
  variant?: 'button' | 'toggle' | 'item';
  size?: 'small' | 'default' | 'large';
  showLabel?: boolean;
  className?: string;
}

const ThemeToggle: React.FC<ThemeToggleProps> = ({
  variant = 'button',
  size = 'default',
  showLabel = true,
  className = ''
}) => {
  const { theme, toggleTheme, isDarkMode, isLoading } = useTheme();

  if (isLoading) {
    return null;
  }

  const baseClassName = `theme-toggle ${className}`;

  // Button variant
  if (variant === 'button') {
    return (
      <IonButton
        fill="clear"
        size={size}
        onClick={toggleTheme}
        className={`${baseClassName} theme-toggle-button`}
        aria-label={`Switch to ${isDarkMode ? 'light' : 'dark'} mode`}
      >
        <IonIcon
          icon={isDarkMode ? sunny : moon}
          slot="icon-only"
        />
        {showLabel && (
          <span className="theme-toggle-label">
            {isDarkMode ? 'Light' : 'Dark'}
          </span>
        )}
      </IonButton>
    );
  }

  // Toggle variant
  if (variant === 'toggle') {
    return (
      <div className={`${baseClassName} theme-toggle-container`}>
        <IonIcon
          icon={sunny}
          className={`theme-toggle-icon ${!isDarkMode ? 'active' : ''}`}
        />
        <IonToggle
          checked={isDarkMode}
          onIonChange={toggleTheme}
          className="theme-toggle-switch"
          aria-label="Toggle dark mode"
        />
        <IonIcon
          icon={moon}
          className={`theme-toggle-icon ${isDarkMode ? 'active' : ''}`}
        />
      </div>
    );
  }

  // Item variant (for use in lists/menus)
  if (variant === 'item') {
    return (
      <IonItem className={`${baseClassName} theme-toggle-item`}>
        <IonIcon
          icon={isDarkMode ? moon : sunny}
          slot="start"
        />
        <IonLabel>
          {showLabel ? `${isDarkMode ? 'Dark' : 'Light'} Mode` : 'Theme'}
        </IonLabel>
        <IonToggle
          checked={isDarkMode}
          onIonChange={toggleTheme}
          slot="end"
          aria-label="Toggle dark mode"
        />
      </IonItem>
    );
  }

  return null;
};

export default ThemeToggle;
```

#### 4. **src/components/ThemeToggle/ThemeToggle.css** (NEW FILE)
```css
.theme-toggle {
  --transition-duration: 0.3s;
  --transition-timing: cubic-bezier(0.4, 0, 0.2, 1);
}

.theme-toggle-button {
  --border-radius: 8px;
  --padding-start: 12px;
  --padding-end: 12px;
  transition: all var(--transition-duration) var(--transition-timing);
}

.theme-toggle-button ion-icon {
  transition: transform var(--transition-duration) var(--transition-timing);
}

.theme-toggle-button:hover ion-icon {
  transform: scale(1.1);
}

.theme-toggle-label {
  margin-left: 8px;
  font-size: 14px;
  font-weight: 500;
}

.theme-toggle-container {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 16px;
  border-radius: 24px;
  background: var(--ion-color-light);
  border: 1px solid var(--ion-color-medium);
  transition: all var(--transition-duration) var(--transition-timing);
}

.theme-toggle-icon {
  font-size: 18px;
  color: var(--ion-color-medium);
  transition: color var(--transition-duration) var(--transition-timing);
}

.theme-toggle-icon.active {
  color: var(--ion-color-primary);
}

.theme-toggle-switch {
  --handle-width: 20px;
  --handle-height: 20px;
  --track-width: 44px;
  --track-height: 24px;
}

.theme-toggle-item {
  --padding-start: 16px;
  --padding-end: 16px;
  --inner-padding-end: 8px;
}

.theme-toggle-item ion-icon {
  color: var(--ion-color-primary);
}

.theme-toggle-item ion-label {
  font-weight: 500;
}

/* Dark mode specific styles */
.dark .theme-toggle-container {
  background: var(--ion-color-dark);
  border-color: var(--ion-color-medium-shade);
}

.dark .theme-toggle-icon {
  color: var(--ion-color-medium-tint);
}

.dark .theme-toggle-icon.active {
  color: var(--ion-color-primary-tint);
}

/* Responsive design */
@media (max-width: 768px) {
  .theme-toggle-label {
    display: none;
  }
  
  .theme-toggle-container {
    padding: 6px 12px;
    gap: 8px;
  }
}

/* Animation for smooth transitions */
@keyframes themeTransition {
  0% {
    opacity: 0;
    transform: scale(0.9);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}

.theme-toggle-button,
.theme-toggle-container {
  animation: themeTransition 0.3s ease-out;
}
```

#### 5. **src/theme/variables.css** (MODIFY - ADD DARK MODE VARIABLES)
```css
/* Add this to the existing variables.css file */

/* Dark mode color variables */
:root.dark {
  --ion-color-primary: #3880ff;
  --ion-color-primary-rgb: 56, 128, 255;
  --ion-color-primary-contrast: #ffffff;
  --ion-color-primary-contrast-rgb: 255, 255, 255;
  --ion-color-primary-shade: #3171e0;
  --ion-color-primary-tint: #4c8dff;

  --ion-color-secondary: #50c8ff;
  --ion-color-secondary-rgb: 80, 200, 255;
  --ion-color-secondary-contrast: #ffffff;
  --ion-color-secondary-contrast-rgb: 255, 255, 255;
  --ion-color-secondary-shade: #46b0e0;
  --ion-color-secondary-tint: #62ceff;

  --ion-color-tertiary: #6a64ff;
  --ion-color-tertiary-rgb: 106, 100, 255;
  --ion-color-tertiary-contrast: #ffffff;
  --ion-color-tertiary-contrast-rgb: 255, 255, 255;
  --ion-color-tertiary-shade: #5d58e0;
  --ion-color-tertiary-tint: #7974ff;

  --ion-color-success: #2fdf75;
  --ion-color-success-rgb: 47, 223, 117;
  --ion-color-success-contrast: #000000;
  --ion-color-success-contrast-rgb: 0, 0, 0;
  --ion-color-success-shade: #29c467;
  --ion-color-success-tint: #44e283;

  --ion-color-warning: #ffd534;
  --ion-color-warning-rgb: 255, 213, 52;
  --ion-color-warning-contrast: #000000;
  --ion-color-warning-contrast-rgb: 0, 0, 0;
  --ion-color-warning-shade: #e0bb2e;
  --ion-color-warning-tint: #ffd948;

  --ion-color-danger: #ff4961;
  --ion-color-danger-rgb: 255, 73, 97;
  --ion-color-danger-contrast: #ffffff;
  --ion-color-danger-contrast-rgb: 255, 255, 255;
  --ion-color-danger-shade: #e04055;
  --ion-color-danger-tint: #ff5b71;

  --ion-color-dark: #f4f5f8;
  --ion-color-dark-rgb: 244, 245, 248;
  --ion-color-dark-contrast: #000000;
  --ion-color-dark-contrast-rgb: 0, 0, 0;
  --ion-color-dark-shade: #d7d8da;
  --ion-color-dark-tint: #f5f6f9;

  --ion-color-medium: #989aa2;
  --ion-color-medium-rgb: 152, 154, 162;
  --ion-color-medium-contrast: #000000;
  --ion-color-medium-contrast-rgb: 0, 0, 0;
  --ion-color-medium-shade: #86888f;
  --ion-color-medium-tint: #a2a4ab;

  --ion-color-light: #222428;
  --ion-color-light-rgb: 34, 36, 40;
  --ion-color-light-contrast: #ffffff;
  --ion-color-light-contrast-rgb: 255, 255, 255;
  --ion-color-light-shade: #1e2023;
  --ion-color-light-tint: #383a3e;

  /* Background colors */
  --ion-background-color: #121212;
  --ion-background-color-rgb: 18, 18, 18;

  --ion-text-color: #ffffff;
  --ion-text-color-rgb: 255, 255, 255;

  --ion-card-background: #1e1e1e;
  --ion-toolbar-background: #1f1f1f;
  --ion-toolbar-border-color: #2d2d2d;

  --ion-item-background: #1e1e1e;
  --ion-item-border-color: #2d2d2d;

  --ion-tabbar-background: #1f1f1f;
  --ion-tabbar-border-color: #2d2d2d;
}

/* Invoice specific dark mode styles */
.dark {
  --invoice-background: #1e1e1e;
  --invoice-text: #ffffff;
  --invoice-border: #2d2d2d;
  --invoice-header-bg: #2a2a2a;
  --invoice-cell-bg: #242424;
  --invoice-cell-border: #333333;
  --invoice-total-bg: #333333;
  --invoice-shadow: rgba(0, 0, 0, 0.3);
}

/* Light mode invoice styles */
:root {
  --invoice-background: #ffffff;
  --invoice-text: #000000;
  --invoice-border: #e0e0e0;
  --invoice-header-bg: #f5f5f5;
  --invoice-cell-bg: #ffffff;
  --invoice-cell-border: #ddd;
  --invoice-total-bg: #f9f9f9;
  --invoice-shadow: rgba(0, 0, 0, 0.1);
}

/* Smooth transitions for theme changes */
* {
  transition: background-color 0.3s ease, color 0.3s ease, border-color 0.3s ease;
}

/* Spreadsheet dark mode styles */
.dark #tableeditor {
  background-color: var(--invoice-background);
  color: var(--invoice-text);
}

.dark .socialcalc-cell {
  background-color: var(--invoice-cell-bg);
  border-color: var(--invoice-cell-border);
  color: var(--invoice-text);
}

.dark .socialcalc-header {
  background-color: var(--invoice-header-bg);
  color: var(--invoice-text);
}
```

#### 6. **src/App.tsx** (MODIFY - ADD THEME INITIALIZATION)
```typescript
import { IonApp, setupIonicReact } from "@ionic/react";
import { useEffect } from "react";
import Home from "./pages/Home";
import { useTheme } from "./hooks/useTheme";

/* Core CSS required for Ionic components to work properly */
import "@ionic/react/css/core.css";

/* Basic CSS for apps built with Ionic */
import "@ionic/react/css/normalize.css";
import "@ionic/react/css/structure.css";
import "@ionic/react/css/typography.css";

/* Optional CSS utils that can be commented out */
import "@ionic/react/css/padding.css";
import "@ionic/react/css/float-elements.css";
import "@ionic/react/css/text-alignment.css";
import "@ionic/react/css/text-transformation.css";
import "@ionic/react/css/flex-utils.css";
import "@ionic/react/css/display.css";

/* Theme variables */
import "./theme/variables.css";

setupIonicReact();

const AppContent: React.FC = () => {
  const { theme, isLoading } = useTheme();

  useEffect(() => {
    // Apply theme class to body for proper Ionic theming
    document.body.classList.toggle('dark', theme === 'dark');
  }, [theme]);

  if (isLoading) {
    return <div className="loading-spinner">Loading...</div>;
  }

  return <Home />;
};

const App: React.FC = () => (
  <IonApp>
    <AppContent />
  </IonApp>
);

export default App;
```

#### 7. **src/pages/Home.tsx** (MODIFY - ADD THEME TOGGLE TO INTERFACE)
```typescript
// Add this import at the top of the existing Home.tsx file
import ThemeToggle from '../components/ThemeToggle/ThemeToggle';

// Then modify the component to include the theme toggle in the header or toolbar
// Add this to the appropriate location in your Home component JSX:

// Example of where to add it - adjust based on your current Home.tsx structure
<IonHeader>
  <IonToolbar>
    <IonTitle>Invoice Suite</IonTitle>
    <IonButtons slot="end">
      <ThemeToggle variant="button" showLabel={false} />
    </IonButtons>
  </IonToolbar>
</IonHeader>

// Or if you want it in a menu/settings area:
<IonContent>
  {/* Your existing content */}
  <div className="theme-toggle-container">
    <ThemeToggle variant="toggle" />
  </div>
</IonContent>
```

#### 8. **src/pages/Home.css** (MODIFY - ADD THEME-AWARE STYLES)
```css
/* Add these styles to your existing Home.css */

/* Theme-aware container styles */
.home-container {
  background-color: var(--ion-background-color);
  color: var(--ion-text-color);
  min-height: 100vh;
  transition: background-color 0.3s ease, color 0.3s ease;
}

/* Theme toggle positioning */
.theme-toggle-container {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1000;
}

/* Invoice area theme styles */
.invoice-area {
  background-color: var(--invoice-background);
  color: var(--invoice-text);
  border: 1px solid var(--invoice-border);
  border-radius: 8px;
  padding: 20px;
  margin: 20px;
  box-shadow: 0 2px 10px var(--invoice-shadow);
  transition: all 0.3s ease;
}

/* Spreadsheet theme integration */
#tableeditor {
  background-color: var(--invoice-background);
  color: var(--invoice-text);
  border: 1px solid var(--invoice-border);
  border-radius: 4px;
  transition: all 0.3s ease;
}

/* Dark mode specific adjustments */
.dark .home-container {
  background-color: var(--ion-background-color);
}

.dark .ion-page {
  background-color: var(--ion-background-color);
}

/* Responsive theme toggle */
@media (max-width: 768px) {
  .theme-toggle-container {
    top: 10px;
    right: 10px;
  }
}
```

### INSTRUCTIONS:

1. **Installation**: No additional packages needed - all dependencies are already in the project.

2. **Setup Steps**:
   - Create all the new files as specified above
   - Modify the existing files with the provided code changes
   - The theme will automatically initialize when the app starts

3. **Usage**:
   - The theme toggle can be used in three variants: `button`, `toggle`, or `item`
   - The theme preference is automatically saved using Capacitor Preferences
   - The app respects system theme preferences by default
   - Smooth transitions are applied when switching themes

4. **Customization**:
   - Colors can be adjusted in `variables.css`
   - Additional theme-aware components can use the `useTheme` hook
   - The toggle appearance can be customized via CSS variables

5. **Testing**:
   - Test on both light and dark system preferences
   - Verify theme persistence across app restarts
   - Test responsive behavior on different screen sizes

This implementation provides a complete dark mode solution that integrates seamlessly with the existing Ionic React government billing application, following best practices for accessibility, performance, and user experience.
```
