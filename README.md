# typescript-notes

## Required package
```bash
npm i --save-dev @types/jest \
  @types/node \
  jest \
  ts-jest \
  ts-loader \
  typescript \
  webpack \
  webpack-cli
```

## Backend Node JS
### typings/index.ts
```js
declare global {
  // declare global variables
  var countSkipped: number;
  var countProcessed: number;
  var countLibUsedByFile: Record<string, number>;

  // how to override prototype interface with wtuffs
  interface String {
      blue() : string;
      yellow() : string;
      green() : string;
      red() : string;
  }
}

export { };
```

### tsconfig.json
`tsconfig`, best to use commonjs module for node js code

```js
{
  "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "baseUrl": "./",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "isolatedModules": false,
    "module": "commonjs",
    "moduleResolution": "node",
    "noFallthroughCasesInSwitch": false,
    "resolveJsonModule": true,
    "skipLibCheck": true,
    "sourceMap": true,
    "strict": true,
    "target": "es6"
  },
  "exclude": ["node_modules"]
}
```

### Webpack
```
const path = require('path');
const webpack = require('webpack');

module.exports = {
  mode: 'production',
  target: ['node'],
  entry: './src/index.ts',
  output: {
    filename: 'main.js',
    libraryTarget: 'this',
    path: path.resolve(__dirname, './'),
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx|ts|tsx)$/,
        use: [
          {
            loader: 'ts-loader',
            options: {
              configFile: 'tsconfig.json',
            },
          },
        ],
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js', '.jsx'],
    alias: {
      src: path.resolve(__dirname, 'src'),
      'package.json': path.resolve(__dirname, 'package.json'),
    },
  },
};

```


## Backend Node JS
### typings/index.ts
Override global objects
```js
import Electron from 'electron';
declare global {
  interface Window {
    isElectron: boolean;
    toggleElectronMenu: (visible: boolean, menus: any[]) => void;
    openBrowserLink: (link: string) => void;
    ipcRenderer?: Electron.IpcRenderer;
  }
}
```

### tsconfig.json
For frontend code, use `esNext` for module

```js
{
  "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "baseUrl": "./",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "isolatedModules": false,
    "jsx": "react-jsx",
    "lib": ["dom", "dom.iterable", "esnext"],
    "module": "esNext",
    "moduleResolution": "node",
    "noFallthroughCasesInSwitch": false,
    "resolveJsonModule": true,
    "skipLibCheck": true,
    "sourceMap": true,
    "strict": true,
    "target": "es6"
  },
  "exclude": ["node_modules"]
}
```
