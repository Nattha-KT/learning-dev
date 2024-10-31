To integrate ESLint, Prettier, Husky, and lint-staged with TypeScript in Next.js 15, follow these steps for a smooth setup:

### 1. **Install Required Packages**

In the root directory of your project, install the necessary dependencies:

```bash
# Install ESLint, Prettier, TypeScript, lint-staged, and Husky
npm install --save-dev eslint prettier eslint-config-prettier eslint-plugin-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser husky lint-staged
```

### 2. **Configure ESLint**

Set up your ESLint configuration file (`.eslintrc.cjs` or `.eslintrc.js`) to work with TypeScript, Prettier, and Next.js:

```javascript
module.exports = {
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 2021,
    sourceType: 'module',
    ecmaFeatures: { jsx: true },
  },
  env: { browser: true, es2021: true, node: true },
  root: true,
  extends: [
    'next', 
    'eslint:recommended', 
    'plugin:@typescript-eslint/recommended', 
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'plugin:prettier/recommended'
  ],
  plugins: ['@typescript-eslint', 'react', 'react-hooks', 'prettier'],
  rules: {
    // Custom ESLint rules
    'prettier/prettier': 'warn',
    'react/react-in-jsx-scope': 'off',
    'no-unused-vars': 'warn',
  },
  settings: { react: { version: 'detect' } },
};
```

### 3. **Configure Prettier**

Add a `.prettierrc` file to define your Prettier settings:

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "printWidth": 80,
  "tabWidth": 2
}
```

### 4. **Setup Husky for Pre-Commit Hooks**

Initialize Husky and add a pre-commit hook in your project root:

```bash
npx husky install
npx husky add .husky/pre-commit "npx lint-staged"
```

This configures Husky to run `lint-staged` on each commit.

### 5. **Configure lint-staged**

Add a lint-staged configuration (`.lintstagedrc.js`) to format code and enforce linting only on staged files:

```javascript
import path from 'path';

const buildEslintCommand = (filenames) =>
  `next lint --fix --file ${filenames
    .map((f) => path.relative(process.cwd(), f))
    .join(' --file ')}`;

export default {
  '*.{js,jsx,ts,tsx}': [buildEslintCommand],
  '*.json': ['prettier --write'],
  '*.md': ['prettier --write']
};
```

### 6. **Add Scripts to `package.json`**

In `package.json`, add commands for linting and formatting:

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint",
  "format": "prettier --write .",
  "prepare": "husky install"
}
```

### 7. **Run Everything Together**

- **Start the Dev Server**: `npm run dev`
- **Run Linting**: `npm run lint`
- **Format Code**: `npm run format`
  
### Notes
- **Test the Integration**: Make a small change to a TypeScript file, stage it, and try committing to see if Husky runs `lint-staged` and fixes issues.
- **Continuous Integration**: Add ESLint and Prettier checks in CI/CD pipelines for code consistency.