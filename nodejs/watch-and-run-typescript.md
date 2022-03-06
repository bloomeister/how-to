# Watch and run TypeScript projects

1. Install `ts-node-dev`:

```
npm i --save-dev ts-node-dev
```

2. Modify `package.json` config:

```
{
  [...]

  "scripts": {
    "start": "tsc && node dist/main.js",
    "watch-and-run": "ts-node-dev --respawn -- src/main.ts"
  }

  [...]
}
```
> One thing to note is that `ts-node-dev` does not output the `transpiled.js` files into a `dist` folder. This is because it makes use of `ts-node` under the hood.