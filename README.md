# typescript-notes

- [Backend Node JS](https://github.com/synle/typescript-notes#backend-node-js)
- [Frontend Node JS](https://github.com/synle/typescript-notes#frontend-node-js)

## Required package
```bash
npm i --save-dev \
  @types/jest \
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

### webpack.config.js
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


## Frontend Node JS
### typings/index.ts
Override global objects
```js
declare global {
  type AppEnv = {
    APP_MODE: string;
    APP_VERSION: string;
  };

  interface Window {
    appEnv:AppEnv
  }
}

export {};
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


### webpack.config.js
```
const path = require('path');
const appPackage = require('./package.json');

const externals = {};
const externalsDeps = [
  'fs',
  'path',
  'electron',
  ...Object.keys(appPackage.optionalDependencies || []),
  ...Object.keys(appPackage.dependencies || [])
];
for(const dep of externalsDeps){
  externals[dep] = `commonjs ${dep}`;
}

console.log('=====================')
console.log('App Version: ', appPackage.version)
console.log('App Mode: ', process.env.APP_MODE)
console.log('=====================')

module.exports = {
  entry: ['./src/renderer/index.tsx'],
  devtool: 'source-map',
  output: {
    filename: 'renderer.js',
    path: path.resolve(__dirname, 'build'),
  },
  mode: 'production',
  externals,
  module: {
    rules: [
      {
        test: /\.(js|jsx|ts|tsx)$/,
        use: [
          {
            loader: 'ts-loader',
            options: {
              configFile: 'tsconfig-ui.json',
            },
          },
        ],
        exclude: /node_modules/,
      },
      {
        test: /\.s[ac]ss$/i,
        use: [
          'style-loader', // Creates `style` nodes from JS strings
          'css-loader', // Translates CSS into CommonJS
          'sass-loader', // Compiles Sass to CSS
        ],
      },
      {
        test: /\.svg$/,
        use: ['@svgr/webpack'],
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js', '.jsx'],
    alias: {
      src: path.resolve(__dirname, 'src'),
    },
  },
};
```
## MUI

### Theme Provider

#### MUI 4.0 Theme Example Creator

Follow this: https://bareynol.github.io/mui-theme-creator/

#### MUI 5.0 Theme Example
```
import { Box } from '@mui/material';
import { ThemeProvider, createTheme } from '@mui/material/styles';

type AppThemeProviderProps = {
  children: React.ReactNode;
};

export default function AppThemeProvider(props: AppThemeProviderProps) {
  const theme = createTheme({
    palette: {
      // TODO: support dark mode
      mode: localStorage.getItem('theme') === 'dark' ? 'dark' : 'light',
    },
    components: {
      MuiButtonBase: {
        defaultProps: {
          disableRipple: true,
        },
      },
      MuiButton: {
        defaultProps: {
          size: 'small',
        },
      },
      MuiButtonGroup: {
        defaultProps: {
          size: 'small',
        },
      },
      MuiCheckbox: {
        defaultProps: {
          size: 'small',
        },
      },
      MuiFab: {
        defaultProps: {
          size: 'small',
        },
      },
      MuiFormControl: {
        defaultProps: {
          size: 'small',
          margin: 'dense',
        },
      },
      MuiFormHelperText: {
        defaultProps: { margin: 'dense' },
      },
      MuiIconButton: {
        defaultProps: {
          size: 'small',
        },
      },
      MuiInputBase: {
        defaultProps: { margin: 'dense' },
      },
      MuiInputLabel: {
        defaultProps: { margin: 'dense' },
      },
      MuiRadio: {
        defaultProps: {
          size: 'small',
        },
      },
      MuiSwitch: {
        defaultProps: {
          size: 'small',
        },
      },
      MuiTextField: {
        defaultProps: {
          margin: 'dense',
          size: 'small',
        },
      },
      MuiTooltip: {
        defaultProps: {
          arrow: true,
        },
      },
    },
  });

  theme.palette.info.dark = '#031e4a';

  return (
    <ThemeProvider theme={theme}>
      <Box
        sx={{
          bgcolor: 'background.default',
          color: 'text.primary',
          minHeight: '100vh',
        }}
      >
        {props.children}
      </Box>
    </ThemeProvider>
  );
}

```


#### Sublime Regex to Replace Props for React Component
```
Regex: 
\(props\:([ a-zA-Z0-9]+)\)

Replace:
(props:\1) : JSX.Element | null
```


#### JSX.Element vs ReactNode vs ReactElement
https://stackoverflow.com/questions/58123398/when-to-use-jsx-element-vs-reactnode-vs-reactelement

- ReactElement : is a react component with props
- ReactNode : generic type for ReactElement / String / undefined / nulll, etc...
- JSX.Element is a generic type that can be implemented by any library. For example preact / react can implement this.

### Zod validator

```typescript
import z from 'zod';

export const userValidator = z.object({
  id: z.string(),
  email: z.string().email().optional(),
  name: z.string().min(1).max(25)
})

export type User = z.infer<typeof userValidator>;


// to parse 
userValidator.parse(some_object); // will throw error if not conformed
```


#### Animation library
https://auto-animate.formkit.com/
