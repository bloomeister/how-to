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
/* eslint-disable no-undef */
// eslint-disable-next-line @typescript-eslint/no-var-requires
const plugin = require("tailwindcss/plugin");

module.exports = {
    content: ["./pages/**/*.{js,ts,jsx,tsx}", "./app/**/*.{js,ts,jsx,tsx}"],
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
                primary: [
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
                input: [
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
            gridTemplateColumns: {
                "character-col": "1fr 2fr 1.5fr 1.5fr 2fr",
                "rarity-col": "1fr 1fr 1.6fr 1.6fr 1.6fr 1.6fr",
            },
            colors: {
                primary: "#060616",
                secondary: "#1E212C",
                sidebar: "#2e2e2e",
                footer: "#222222",
                modal: "#222222",
                "modal-backdrop": "#000000",
                icon: "#ffffff",
                "icon-primary": "#ed3133",
                "icon-dark": "#222222",
                "icon-disabled": "#777777",
            },
            spacing: {
                "header-lg": "80px",
                header: "60px",
                branding: "48px",
                "branding-sm": "32px",
                footer: "315px",
                "icon-xs": "12px",
                "icon-sm": "18px",
                icon: "24px",
                "icon-lg": "32px",
                sidebar: "315px",
                modal: "480px",
            },
            inputSize: {
                xs: "10px",
                sm: "16px",
            },
        },
    },
    plugins: [
        require("@tailwindcss/line-clamp"),
        plugin(({ addComponents, addUtilities }) => {
            addComponents({
                ".input-border": {
                    borderStyle: "solid",
                    borderWidth: "1px",
                    borderColor: "rgba(23, 23, 23, 0.2)",
                    "&:hover": {
                        borderColor: "rgba(23, 23, 23, 0.3)",
                    },
                    "&:focus": {
                        borderColor: "rgba(23, 23, 23, 0.5)",
                    },
                    "&:disabled": {
                        borderColor: "rgba(23, 23, 23, 0.2)",
                    },
                },
                ".typed-disable1": {
                    color: "rgba(255, 255, 255, 0.3)",
                    backgroundColor: "#FFFFFF",
                },
                ".typed-disable2": {
                    backgroundColor: "rgba(23, 23, 23, 0.1)",
                },
                ".input-sm": {
                    // height: "32px",
                    fontSize: "13px",
                    padding: "6px 12px",
                },
                ".input-md": {
                    height: "48px",
                    borderRadius: "12px",
                    padding: "0 12px",
                    fontSize: "16px",
                },
                ".input-lg": {
                    height: "3rem",
                    borderRadius: "0.5rem",
                    padding: "0 1rem",
                    fontSize: "1.125rem",
                },
                ".btn-sm": {
                    height: "36px",
                    borderRadius: "8px",
                    fontSize: "12px",
                },
                ".btn-md": {
                    height: "40px",
                    borderRadius: "12px",
                    fontSize: "14px",
                },
                ".btn-lg": {
                    height: "52px",
                    borderRadius: "12px",
                    fontSize: "16px",
                },
                ".btn": {
                    display: "flex",
                    justifyContent: "center",
                    alignItems: "center",
                    cursor: "pointer",
                    transitionProperty:
                        "color, background-color, border-color, text-decoration-color, fill, stroke",
                    transitionTimingFunction: "cubic-bezier(0.4, 0, 0.2, 1)",
                    transitionDuration: "150ms",
                },
                ".selectBox": {
                    border: "2px solid rgba(23, 23, 23, 0.2)",
                    borderRadius: "12px",
                    transitionProperty:
                        "color, background-color, border-color, text-decoration-color, fill, stroke",
                    transitionTimingFunction: "cubic-bezier(0.4, 0, 0.2, 1)",
                    transitionDuration: "150ms",

                    "&:hover": {
                        borderColor: "rgba(23, 23, 23, 0.3)",
                    },
                    "&:active": {
                        borderColor: "rgba(23, 23, 23, 0.5)",
                    },
                },
                ".btn-primary": {
                    color: "#FFFFFF",
                    backgroundColor: "#DC413A",
                    "&:hover": {
                        backgroundColor: "#E36761",
                    },
                    "&:active": {
                        backgroundColor: "#DC413A",
                    },
                    "&:disabled": {
                        backgroundColor: "#EDA09C",
                    },
                },
                ".btn-secondary": {
                    color: "#DC413A",
                    border: "2px solid #DC413A",
                    "&:hover": {
                        backgroundColor: "#F8D9D8",
                    },
                    "&:active": {
                        color: "#DC413A",
                        border: "2px solid #DC413A",
                        backgroundColor: "#FFFFFF",
                    },
                    "&:disabled": {
                        backgroundColor: "#FFFFFF",
                        color: "#EDA09C",
                        border: "2px solid #EDA09C",
                    },
                },
                ".btn-mono": {
                    border: "2px solid rgba(23, 23, 23,0.25)",
                    color: "rgba(23, 23, 23,0.3)",
                    "&:hover": {
                        backgroundColor: "#e8e8e8",
                    },
                    "&:active": {
                        border: "2px solid rgba(23, 23, 23,0.25)",
                        color: "rgba(23, 23, 23,0.3)",
                        backgroundColor: "#FFFFFF",
                    },
                    "&:disabled": {
                        border: "2px solid rgba(23, 23, 23,0.15)",
                        backgroundColor: "#FFFFFF",
                    },
                },
                ".search-border": {
                    border: "2px solid #E8E8E8",
                    borderRadius: "100px",
                    "&+div div svg path": {
                        fill: "#E8E8E8",
                    },
                    "&:hover": {
                        borderColor: "#D1D1D1",
                    },
                    "&:hover+div div svg path": {
                        fill: "#D1D1D1",
                    },
                    "&:focus": {
                        borderColor: "#454545",
                    },
                    "&:focus+div div svg path": {
                        fill: "#454545",
                    },
                    "&.error": {
                        borderColor: "#E36761",
                    },
                    "&.error+div div svg path": {
                        fill: "#E36761",
                    },
                    "&.result": {
                        borderColor: "#454545",
                    },
                    "&.result+div div svg path": {
                        fill: "#454545",
                    },
                    "&.result+div+div div svg": {
                        display: "block",
                    },
                    "&:disabled": {
                        borderColor: "#E8E8E8",
                    },
                    "&:disabled+div div svg path": {
                        fill: "#E8E8E8",
                    },
                },
                ".border-err": {
                    borderColor: "#E36761 !important",
                },
                ".border-success": {
                    borderColor: "#27AE60 !important",
                },
                ".border-grid": {
                    "&>div": {
                        outline: "1px solid rgba(23, 23, 23, 0.1)",
                    },
                },
                ".active-route": {
                    position: "absolute",
                    left: 0,
                    top: 0,
                    bottom: 0,
                    backgroundColor: "#DC413A",
                    width: "5px",
                },
            });
            addUtilities({
                ".animate-pause": {
                    "animation-play-state": "paused",
                },
                ".scrollbar-none": {
                    "scrollbar-width": "none",
                    "&::-webkit-scrollbar": {
                        display: "none",
                    },
                },
                ".scrollbar-thin": {
                    "scrollbar-width": "thin",
                    "&::-webkit-scrollbar": {
                        width: "5px",
                        height: "8px",
                        background: "rgba(255,255,255,0.02)",
                    },
                    "&::-webkit-scrollbar-thumb": {
                        background: "rgba(0,0,0,0.3)",
                    },
                },
            });
        }),
    ],
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


## libssl1.1_1.1.1

```
$ wget http://nz2.archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1l-1ubuntu1.6_amd64.deb

$ sudo dpkg -i libssl1.1_1.1.1l-1ubuntu1.6_amd64.deb
```