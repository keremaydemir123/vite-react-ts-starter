vite-react-ts-starter
Create React-Ts App
npm create vite@latest
clear unnecessary files (index.css favicons etc.)
Eslint Setup
npm i -D eslint
npx install-peerdeps --dev eslint-config-airbnb
in .eslintrc.cjs find "extends" array, clear "eslint:recommended" and add "airbnb", "airbnb/hooks"
it should be look like :

content_copy
  extends: [
    "airbnb",
    "airbnb/hooks",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
  ],
npm i -D eslint-config-airbnb-typescript
in .eslintrc.cjs find "extends" array, add "airbnb-typescript"
it should be like this:

content_copy
  extends: [
    "airbnb",
    "airbnb-typescript",
    "airbnb/hooks",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
  ],
in .eslintrc.cjs find "parserOptions" object, add project: './tsconfig.json'
in .eslintrc.cjs find "rules" array, add 'react/react-in-jsx-scope':0
Prettier Setup
npm i -D prettier eslint-config-prettier eslint-plugin-prettier
create .prettierrc.cjs at root folder
add those lines:
content_copy
module.exports = {
    trailingComma: 'es5',
    tabWidth: 2,
    semi: true,
    singleQuote: true,
};
in .eslintrc.cjs find "plugins" array, add 'prettier'
in .eslintrc.cjs find "extends" array, add 'plugin:prettier/recommended' to the LAST LINE to avoid conflicts with eslint
it should be look like:

content_copy
  extends: [
    'airbnb',
    'airbnb-typescript',
    'airbnb/hooks',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended',
  ],
Testing Setup
npm install -D vitest
refactor vite.config.ts to be look like this:
content_copy
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
in tsconfig.json file, find "include" array, add 'vite.config.ts'
npm install --D @testing-library/react @testing-library/jest-dom
create setupTests.ts inside /src folder and add those code:
content_copy
/* eslint-disable import/no-extraneous-dependencies */
import matchers from '@testing-library/jest-dom/matchers';
import { expect } from 'vitest';
 
expect.extend(matchers);
create App.test.tsx for first test
add those lines to check if DOM has a text with "Hello Word"
content_copy
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
in package.json find "scripts" object, add "test": "vitest"
to run tests now you can type npm test
You can check queries for testing library from here
React Router Setup
npm install react-router-dom@6
in App.tsx add React Router imports and modify the file as:
content_copy
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
in /src folder, create pages folder.
create your "Home" and "NotFound" components here
