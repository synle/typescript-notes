# typescript-notes

### Declare global typings
#### typings/index.ts
##### for the node js
```ts
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

`tsconfig`, best to use commonjs module for node js code

```ts
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


##### for the browser
Override global objects
```ts
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

For frontend code, use ``

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
