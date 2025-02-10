# Tailwind Config ( Version : Juin 2024)

```javascript
const plugin = require('tailwindcss/plugin');

/** @type {import('tailwindcss').Config} */
module.exports = {
  // Spécifie les chemins des fichiers contenant les classes Tailwind à purger
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],

  theme: {
    // Définition des points de rupture (breakpoints) personnalisés
    screens: {
      'sm': '320px',   // Petits écrans
      'mm': '375px',   // Écrans moyens-petits
      'tm': '425px',   // Écrans moyens
      'tab': '769px',  // Tablettes
      'md': '900px',   // Écrans moyens
      'ls': '1024px',  // Écrans larges
      'lg': '1200px',  // Grands écrans
      'xl': '1440px'   // Très grands écrans
    },

    // Définition des palettes de couleurs personnalisées
    colors: {
      white: '#ffff',

      // Nuances de gris
      gray: {
        50: '#fcfcfc',
        100: '#f3f4f6',
        200: '#e5e7eb',
        300: '#d1d5db',
        400: '#9ca3af',
        500: '#6b7280',
        600: '#4b5563',
        700: '#374151',
        800: '#1f2937',
        900: '#111827'
      },

      // Nuances de rouge
      red: {
        50: '#fef2f2',
        100: '#fee2e2',
        200: '#fecaca',
        300: '#fca5a5',
        400: '#f87171',
        500: '#ef4444',
        600: '#dc2626',
        700: '#b91c1c',
        800: '#991b1b',
        900: '#7f1d1d'
      },

      // Nuances de jaune
      yellow: {
        50: '#fff6eb',
        100: '#ffe4c1',
        200: '#ffc986',
        300: '#ffb65f',
        400: '#ffa43d',
        500: '#ff9600',
        600: '#d77a1e',
        700: '#975612',
        800: '#6b3d0a',
        900: '#412504'
      },

      // Nuances de bleu
      blue: {
        50: '#eff6ff',
        100: '#dbeafe',
        200: '#bfdbfe',
        300: '#93c5fd',
        400: '#60a5fa',
        500: '#3b82f6',
        600: '#2563eb',
        700: '#1d4ed8',
        800: '#1e40af',
        900: '#1e3a8a'
      },

      // Nuances de vert
      green: {
        50: '#ecfdf5',
        100: '#d1fae5',
        200: '#a7f3d0',
        300: '#6ee7b7',
        400: '#34d399',
        500: '#10b981',
        600: '#059669',
        700: '#047857',
        800: '#065f46',
        900: '#064e3b'
      },
    },

    // Extension du thème pour ajouter des propriétés personnalisées
    extend: {
      maxWidth: {
        '8xl': '88rem',
        '9xl': '96rem',
      },
      fontFamily: {
        Poppins: ['Poppins', 'sans-serif'],
      },
      keyframes: {
        // Animation pour faire glisser et disparaître vers la gauche
        slideFadeOutLeft: {
          '0%': { opacity: '1' },
          '90%': { opacity: '0', transform: 'translate(100%, 0)' },
          '100%': { display: 'none' },
        },
        // Animation pour faire glisser vers la droite
        slideRight: {
          '0%': { transform: 'translate(100%, 0)' },
          '100%': { transform: 'translate(0, 0)' },
        },
        // Animation pour faire apparaître progressivement
        fadeInFrame: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        }
      },
      animation: {
        slideFadeOutL: 'slideFadeOutLeft 0.5s ease-in-out forwards',
        slideR: 'slideRight 0.2s ease-in-out forwards',
        fadeIn: 'fadeInFrame 0.3s ease-in-out',
      },
    },
  },

  // Plugins Tailwind personnalisés
  plugins: [
    // Plugin pour masquer la barre de défilement
    plugin(({ addUtilities }) => {
      addUtilities({
        '.no-scrollbar::-webkit-scrollbar': { display: 'none' },
        '.no-scrollbar': {
          '-ms-overflow-style': 'none',
          'scrollbar-width': 'none',
        }
      });
    }),

    // Plugin pour ajouter une variante personnalisée pour l'expansion de la barre latérale
    plugin(({ addVariant, e }) => {
      addVariant('sidebar-expanded', ({ modifySelectors, separator }) => {
        modifySelectors(({ className }) => `.sidebar-expanded .${e(`sidebar-expanded${separator}${className}`)}`);
      });
    }),

    // Plugin pour ajouter des composants personnalisés
    plugin(function ({ addComponents }) {
      addComponents({
        // Styles de texte
        '.ft-xs': { fontFamily: 'Poppins', fontSize: '12px', fontStyle: 'normal', lineHeight: '16px' },
        '.ft-sm': { fontFamily: 'Poppins', fontSize: '14px', fontStyle: 'normal', lineHeight: '20px' },
        '.ft-b': { fontFamily: 'Poppins', fontSize: '16px', fontStyle: 'normal', lineHeight: '24px' },
        '.ft-lg': { fontFamily: 'Poppins', fontSize: '18px', fontStyle: 'normal', lineHeight: '28px' },
        '.ft-xl': { fontFamily: 'Poppins', fontSize: '20px', fontStyle: 'normal', lineHeight: '28px' },
        '.ft-2xl': { fontFamily: 'Poppins', fontSize: '24px', fontStyle: 'normal', lineHeight: '32px' },
        '.ft-4xl': { fontFamily: 'Poppins', fontSize: '36px', fontStyle: 'normal', lineHeight: '40px' },

        // Styles d'ombre portée
        '.sh-sm': { boxShadow: '0px 1px 2px rgba(0, 0, 0, 0.05)' },
        '.sh-md': { boxShadow: '0px 4px 8px -4px rgba(22, 34, 51, 0.08), 0px 16px 24px rgba(22, 34, 51, 0.08)' },
        '.sh-lg': { boxShadow: '0px 4px 12px -4px rgba(22, 34, 51, 0.12), 0px 16px 32px rgba(22, 34, 51, 0.16)' },
        '.sh-xl': { boxShadow: '0px 120px 120px rgba(22, 34, 51, 0.08), 0px 64px 64px rgba(22, 34, 51, 0.12), 0px 32px 32px rgba(22, 34, 51, 0.04), 0px 24px 24px rgba(22, 34, 51, 0.04), 0px 4px 24px rgba(22, 34, 51, 0.04), 0px 4px 4px rgba(22, 34, 51, 0.04)' }
      });
    })
  ],
}

```