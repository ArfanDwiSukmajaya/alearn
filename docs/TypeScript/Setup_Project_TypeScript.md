---
sidebar_position: 1
---

# Setup Project Typescript

## Membuat Project

### Init Project

```jsx
  npm init
```

Kemudian buka package.json trus tambahin type module

```jsx
{
  "name": "ok",
  "version": "1.0.0",
  "description": "Typescript",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "arfan",
  "license": "ISC",
  "type": "module"
}
```

### Menambahkan Library Jest Untuk Unit Test

npm install --save-dev jest @types/jest

https://www.npmjs.com/package/jest

Kemudian edit package jsonnya, ikuti link diatas

```jsx
  {
  "name": "ok",
  "version": "1.0.0",
  "description": "Typescript",
  "main": "index.js",
  "scripts": {
    "test": "jest"
  },
  "jest": {
    "transform": {
      "^.+\\.[t|j]sx?$": "babel-jest"
    }
  },
  "author": "arfan",
  "license": "ISC",
  "type": "module"
}

```

npm install @babel/preset-env --save-dev

Kemudian create file babel.config.json

```jsx

  {
    "presets": ["@babel/preset-env"]
  }

```

### Menambahkan Library Babel

npm install --save-dev babel-jest @babel/preset-env

https://babeljs.io/setup#installation

## Setup Typescript Project

### Menambahkan Typescript

npm install --save-dev typescript

https://www.npmjs.com/package/typescript

### Setup Typescript Project

npx tsc --init

Semua konfigurasi akan dibuat di file tsconfig.json

Ubah “module” dari “commonjs” menjadi “ES6

```jsx
  {
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig to read more about this file */
    "target": "es2016" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,
    "module": "ES6" /* Specify what module code is generated. */, -- ini yang diubah
    "esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
    "forceConsistentCasingInFileNames": true /* Ensure that casing is correct in imports. */,
    "strict": true /* Enable all strict type-checking options. */,
    "skipLibCheck": true /* Skip type checking all .d.ts files. */
  }
}

```

## Setup Typescript Untuk Jest

https://jestjs.io/docs/getting-started#using-typescript (ini docsnya)

npm install --save-dev babel-jest @babel/core @babel/preset-env

Edit di file babel.config.json

```jsx
  {
    "presets": ["@babel/preset-env", "@babel/preset-typescript"]
}
```

npm install --save-dev @babel/preset-typescript

npm install --save-dev ts-jest

npm install --save-dev @jest/globals

### Kemudian coba test dengan membuat file test

test/hello.test.ts

```jsx
describe("hello", function () {
  it("Should say Hello", function () {
    const name = "Hello";
    expect(name).toBe("Hello");
  });
});
```

kemudian jalankan di terminal npx jest

### Kompilasi TypeScript
