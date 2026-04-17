---
name: vite-vue-primevue-setup
description: Use when the user says "请配置前端", asks to configure the frontend, or asks to set up or continue a Vite + Vue 3 + TypeScript frontend that should use PrimeVue, PrimeVue Forms, PrimeIcons, Tailwind CSS, Aura theming, vue-i18n, vue-router, local fonts, Prettier, and ESLint. Also use when standardizing this stack in a new project, adding missing parts of the workflow, or verifying that the frontend setup matches the shared project convention.
disable-model-invocation: true
---

# Vite Vue PrimeVue Setup

## Goal

Set up a reusable `frontend/` app with Vite, Vue 3, TypeScript, PrimeVue, Tailwind, Forms, Icons, i18n, routing, formatting, and linting.

Prefer the existing project command wrappers when present. In WSL/sandboxed projects, use project npm wrappers such as `.codex-scripts/npmw.sh` and `.codex-scripts/npxw.sh` instead of direct `npm` / `npx`.

## When To Use

- 当用户说“请配置前端”时使用
- 当用户说“请初始化前端”时使用

## Workflow

1. Read project docs first.
   - Check root `README.md`.
   - Check local task docs such as `.codex-project/tasks/README.md` if present.
   - Respect existing source layout and project-specific rules.

2. Create the Vite app.
   - Put the frontend in `frontend/` unless the project already has a different frontend root.
   - Use Vue + TypeScript:

```bash
.codex-scripts/npmw.sh create vite@latest frontend -- --template vue-ts
cd frontend
../.codex-scripts/npmw.sh install
```

3. Install PrimeVue stack.

```bash
../.codex-scripts/npmw.sh install primevue @primeuix/themes @primevue/forms primeicons
```

4. Install Tailwind integration.

```bash
../.codex-scripts/npmw.sh install -D tailwindcss @tailwindcss/vite tailwindcss-primeui
```

5. Install i18n and routing.

```bash
../.codex-scripts/npmw.sh install vue-i18n vue-router
```

6. Install formatting and linting.

```bash
../.codex-scripts/npmw.sh install -D prettier eslint @eslint/js typescript-eslint eslint-plugin-vue vue-eslint-parser eslint-config-prettier
```

7. Configure files as described below.

8. Verify:

```bash
../.codex-scripts/npmw.sh run format:check
../.codex-scripts/npmw.sh run lint:check
../.codex-scripts/npmw.sh run build
```

## Package Scripts

Use this script shape in `frontend/package.json`:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc -b && vite build",
    "preview": "vite preview",
    "format:check": "prettier . --check",
    "format:fix": "prettier . --write",
    "lint:check": "eslint .",
    "lint:fix": "eslint . --fix"
  }
}
```

## PrimeVue Theme

Configure PrimeVue in `frontend/src/main.ts`.

- Import PrimeIcons CSS.
- Register i18n and router before or alongside PrimeVue.
- Use `definePreset(Aura, ...)`.
- Set `semantic.primary` to violet.
- Set `semantic.colorScheme.light.surface` and `dark.surface` to neutral.
- Enable PrimeVue CSS layer before Tailwind utilities.

Use this pattern:

```ts
import { createApp } from 'vue'
import PrimeVue from 'primevue/config'
import { definePreset } from '@primeuix/themes'
import Aura from '@primeuix/themes/aura'
import 'primeicons/primeicons.css'
import './style.css'
import App from './App.vue'
import { i18n } from './i18n'
import { router } from './router'

const appTheme = definePreset(Aura, {
  semantic: {
    primary: {
      50: '{violet.50}',
      100: '{violet.100}',
      200: '{violet.200}',
      300: '{violet.300}',
      400: '{violet.400}',
      500: '{violet.500}',
      600: '{violet.600}',
      700: '{violet.700}',
      800: '{violet.800}',
      900: '{violet.900}',
      950: '{violet.950}'
    },
    colorScheme: {
      light: {
        surface: {
          0: '#ffffff',
          50: '{neutral.50}',
          100: '{neutral.100}',
          200: '{neutral.200}',
          300: '{neutral.300}',
          400: '{neutral.400}',
          500: '{neutral.500}',
          600: '{neutral.600}',
          700: '{neutral.700}',
          800: '{neutral.800}',
          900: '{neutral.900}',
          950: '{neutral.950}'
        }
      },
      dark: {
        surface: {
          0: '#ffffff',
          50: '{neutral.50}',
          100: '{neutral.100}',
          200: '{neutral.200}',
          300: '{neutral.300}',
          400: '{neutral.400}',
          500: '{neutral.500}',
          600: '{neutral.600}',
          700: '{neutral.700}',
          800: '{neutral.800}',
          900: '{neutral.900}',
          950: '{neutral.950}'
        }
      }
    }
  }
})

createApp(App)
  .use(i18n)
  .use(router)
  .use(PrimeVue, {
    theme: {
      preset: appTheme,
      options: {
        cssLayer: {
          name: 'primevue',
          order: 'theme, base, primevue'
        }
      }
    }
  })
  .mount('#app')
```

## Tailwind

Configure `frontend/vite.config.ts`:

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [vue(), tailwindcss()]
})
```

Configure `frontend/src/style.css`:

```css
@import "tailwindcss";
@import "tailwindcss-primeui";
```

Then use PrimeUI utilities such as `text-primary-700`, `from-surface-0`, `to-surface-100`, and `text-surface-950` to verify integration.

## Local Fonts

If the project provides fonts in `frontend/public/fonts/`, wire them with `@font-face` in `frontend/src/style.css`.

For Roboto variable fonts:

```css
@font-face {
  font-family: "Roboto";
  src: url("/fonts/Roboto-VariableFont_wdth,wght.ttf") format("truetype");
  font-style: normal;
  font-weight: 100 900;
  font-display: swap;
}

@font-face {
  font-family: "Roboto";
  src: url("/fonts/Roboto-Italic-VariableFont_wdth,wght.ttf") format("truetype");
  font-style: italic;
  font-weight: 100 900;
  font-display: swap;
}

:root {
  font-family: "Roboto", ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}
```

## i18n

Create:

```text
frontend/src/i18n/
  index.ts
  de.ts
  zh.ts
```

Use `vue-i18n` composition mode:

```ts
import { createI18n } from 'vue-i18n'
import { de } from './de'
import { zh } from './zh'

export type Sprache = 'de' | 'zh'

export const i18n = createI18n({
  legacy: false,
  locale: 'de',
  fallbackLocale: 'de',
  messages: { de, zh }
})
```

Move all visible copy into language files. Include at least:

- App name/title
- Language switch labels
- Disclaimer
- Privacy note
- Form labels, validation messages, and submit text

## Router

Create:

```text
frontend/src/router/index.ts
frontend/src/ansichten/Startseite.vue
```

Use German directory names if the project requires German file/folder naming:

```ts
import { createRouter, createWebHistory } from 'vue-router'
import Startseite from '../ansichten/Startseite.vue'

export const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      name: 'startseite',
      component: Startseite
    }
  ]
})
```

Keep `App.vue` as the app shell with Header, `RouterView`, and Footer. Put page content into `ansichten/`.

## PrimeVue Forms and Icons Smoke Test

Add a minimal form to the start page:

- Import `Form` from `@primevue/forms`.
- Import `InputText`, `Message`, and `Button`.
- Use a small custom `resolver`.
- Show validation error and success state.
- Use PrimeIcons with `pi pi-check-circle` or a Button `icon="pi pi-check"`.

Do not build full product forms during setup; keep this as a smoke test only.

## Prettier

Create `frontend/.prettierrc.json` exactly as:

```json
{
  "tabWidth": 2,
  "printWidth": 150,
  "semi": true,
  "singleQuote": true,
  "jsxSingleQuote": true,
  "bracketSameLine": false,
  "singleAttributePerLine": false,
  "trailingComma": "none",
  "htmlWhitespaceSensitivity": "ignore"
}
```

Create `frontend/.prettierignore`:

```text
node_modules
dist
coverage
*.local
package-lock.json
```

## ESLint

Create `frontend/eslint.config.js`:

```js
import js from '@eslint/js'
import prettier from 'eslint-config-prettier'
import tseslint from 'typescript-eslint'
import vue from 'eslint-plugin-vue'
import vueParser from 'vue-eslint-parser'

export default [
  {
    ignores: ['dist/**', 'node_modules/**', 'coverage/**']
  },
  js.configs.recommended,
  ...tseslint.configs.recommended,
  ...vue.configs['flat/recommended'],
  prettier,
  {
    files: ['**/*.ts', '**/*.vue'],
    languageOptions: {
      parser: vueParser,
      parserOptions: {
        parser: tseslint.parser,
        ecmaVersion: 'latest',
        sourceType: 'module',
        extraFileExtensions: ['.vue']
      }
    },
    rules: {
      'vue/multi-word-component-names': 'off'
    }
  }
]
```

## Project Structure

Create the common source directories when the project calls for them:

```text
frontend/src/
  ansichten/
  komponenten/
  berechnungen/
  konfiguration/
  typen/
  i18n/
  speicher/
  hilfen/
  router/
```

Keep calculation/domain logic out of UI components. Use pure functions for calculation modules.

## Validation

Always run:

```bash
../.codex-scripts/npmw.sh run format:check
../.codex-scripts/npmw.sh run lint:check
../.codex-scripts/npmw.sh run build
```

If the project has a dev server requirement, start:

```bash
../.codex-scripts/npmw.sh run dev -- --host 127.0.0.1
```

Report the local URL and whether the server remains running.
