# Setup NextJS app with TS, Tailwind, Redux

## NextJS

### **Installation**

```
npx create-next-app@latest --typescript
cd ./my-app
npm run dev
```

### **Deployment**

```
npm run build
npm run start
```

> Centos7 OS in which it does not support fully SWC:

1. Add `.babelrc` configuration file:

> `.babelrc`

```
{
    "presets": ["next/babel"]
}
```

2. Add `swcMinify: false` to `nextjs.config.js`

> `nextjs.config.js`

```
swcMinify: false
```

## Tailwind

### **Installation**

1. Install tailwindcss and its peer dependencies via `npm`, and then run the init command to generate both `tailwind.config.js` and `postcss.config.js`.

```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
npm install @tailwindcss/line-clamp
```

2. Add the Tailwind directives to your CSS:

> `./styles/globals.css`

```
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
    html {
        font-size: 14px;
    }
}

```

### **Configuration**

Add the paths to all of your template files in your `tailwind.config.js` file.

> `tailwind.config.js`

```
/* eslint-disable quotes */
module.exports = {
    content: [
        "./pages/**/*.{js,ts,jsx,tsx}",
        "./components/**/*.{js,ts,jsx,tsx}",
        "./classes/**/*.{js,ts,jsx,tsx}",
        "./contexts/**/*.{js,ts,jsx,tsx}",
    ],
    theme: {
        screens: {
            xs: "360px",
            sm: "640px",
            md: "768px",
            lg: "1024px",
            xl: "1280px",
            "2xl": "1366px",
            "3xl": "1440px",
            "4xl": "1600px",
            "5xl": "1920px",
            phone: "360px",
            "phone-2": "640px",
            tablet: "768px",
            "tablet-2": "1024px",
            desktop: "1280px",
            "desktop-2": "1366px",
            "desktop-3": "1440px",
            "desktop-4": "1600px",
            "desktop-5": "1920px",
        },
        extend: {
            fontFamily: {
                anton: ['"Anton"', "sans-serif"],
                body: [
                    '"Open Sans"',
                    "ui-sans-serif",
                    "system-ui",
                    "-apple-system",
                    "BlinkMacSystemFont",
                    '"Segoe UI"',
                    "Roboto",
                    '"Helvetica Neue"',
                    "Arial",
                    '"Noto Sans"',
                    "sans-serif",
                    '"Apple Color Emoji"',
                    '"Segoe UI Emoji"',
                    '"Segoe UI Symbol"',
                    '"Noto Color Emoji"',
                ],
            },
            fontSize: {
                "8xl": "6rem",
                "9xl": "7rem",
                "10xl": "8rem",
            },
        },
    },
    plugins: [require("@tailwindcss/line-clamp")],
};
```

## ESlint

### **Setup**

1. Typescript

```
npm i -D typescript @typescript-eslint/parser
npm i -D @typescript-eslint/eslint-plugin
```

2. React

```
npm i -D eslint-plugin-react
npm i -D eslint-plugin-react-hooks
```

### **Configuration**

> `.eslintrc.json`

```
{
    "ignorePatterns": ["*.json"],
    "rules": {
        "semi": ["warn", "always"],
        "quotes": ["warn", "double"]
    },
    "extends": [
        "next/core-web-vitals",
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:react-hooks/recommended",
        "plugin:react/jsx-runtime",
        "plugin:@typescript-eslint/recommended"
    ]
}
```

## Typescript

### **Configuration**

```
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "strictNullChecks": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "incremental": true,
    "noEmit": true,
    "jsx": "preserve"
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

## Redux

```
npm i -S redux react-redux @types/react-redux @reduxjs/toolkit
```

## ReactDOM

```
npm i -S react-dom @types/react-dom
```
