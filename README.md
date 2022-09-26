# vite-react-ts-starter

## Create React-Ts App

1. `npm create vite@latest`
2. clear unnecessary files (index.css favicons etc.)

## Eslint Setup

3. `npm i -D eslint`
4. `npx install-peerdeps --dev eslint-config-airbnb`
5. in .eslintrc.cjs find "extends" array, clear "eslint:recommended" and add  `"airbnb", "airbnb/hooks"`

it should be look like :
```js
  extends: [
    "airbnb",
    "airbnb/hooks",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
  ],
```
6. `npm i -D eslint-config-airbnb-typescript`
7. in .eslintrc.cjs find "extends" array, add `"airbnb-typescript"`

it should be like this:
```js
  extends: [
    "airbnb",
    "airbnb-typescript",
    "airbnb/hooks",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
  ],
```
8. in .eslintrc.cjs find "parserOptions" object, add `project: './tsconfig.json'`
9. in .eslintrc.cjs find "rules" array, add `'react/react-in-jsx-scope':0`

## Prettier Setup

10. `npm i -D prettier eslint-config-prettier eslint-plugin-prettier`
11. create `.prettierrc.cjs` at root folder
12. add those lines:
```js
module.exports = {
    trailingComma: 'es5',
    tabWidth: 2,
    semi: true,
    singleQuote: true,
};
```
13. in .eslintrc.cjs find "plugins" array, add `'prettier'`
14. in .eslintrc.cjs find "extends" array, add `'plugin:prettier/recommended'` to the **LAST LINE** to avoid conflicts with eslint

it should be look like:
```js
  extends: [
    'airbnb',
    'airbnb-typescript',
    'airbnb/hooks',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended',
  ],
```

## Testing Setup

15. `npm install -D vitest`
16. refactor vite.config.ts to be look like this:
```js
/* eslint-disable import/no-extraneous-dependencies */
/// <reference types="vitest" />
/// <reference types="vite/client" />

import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./src/setupTests.ts'],
  },
});
```
17. in tsconfig.json file, find "include" array, add `'vite.config.ts'`
18. `npm install --D @testing-library/react @testing-library/jest-dom`
19. create setupTests.ts inside /src folder and add those code:
```js
/* eslint-disable import/no-extraneous-dependencies */
import matchers from '@testing-library/jest-dom/matchers';
import { expect } from 'vitest';

expect.extend(matchers);
```
20. create App.test.tsx for first test
21. add those lines to check if DOM has a text with "Hello Word"
```js
import { describe, it } from 'vitest';
import { render, screen } from '@testing-library/react';

import App from './App';

describe('App', () => {
  it('Renders hello world', () => {
    // ARRANGE
    render(<App />);
    // ACT
    // EXPECT
    expect(
      screen.getAllByRole('heading', {
        level: 1,
      })
    ).toHaveTextContent('Hello Word');
  });
});
```
22. in package.json find "scripts" object, add `"test": "vitest"`
23. to run tests now you can type `npm test`
24. You can check queries for testing library from [here](https://testing-library.com/docs/queries/about/#priority)

## React Router Setup

25. `npm install react-router-dom@6`
26. in App.tsx add React Router imports and modify the file as:
```js
import { HashRouter, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import NotFound from './pages/NotFound';

export function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}

export function WrappedApp() {
  return (
    <HashRouter>
      <App />
    </HashRouter>
  );
}
```
27. in /src folder, create pages folder.
28. create your "Home" and "NotFound" components here


