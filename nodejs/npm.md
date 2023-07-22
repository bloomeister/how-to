# npm

## Update `nodejs` to latest stable version

```
npm cache clean -f
npm install -g n
n stable
```

> If `node --version` shows the old version then start a new shell, or reset the location hash with: `hash -r` (for `bash`, `zsh`, `ash`, `dash`, and `ksh`) [or] `rehash` (for `csh` and `tcsh`)

## TL;DR

-   Use `npm outdated` to discover dependencies that are out of date
-   Use `npm update` to perform safe dependency upgrades
-   Use `npm install <packagename>@latest` to upgrade to the latest major version of a package
-   Use `npx npm-check-updates -u` and `npm install` to upgrade all dependencies to their latest major versions

## What does the `^` and `~` mean?

A version often has a `^` in front of it (e.g. `^16.8.6`). This means that the latest **minor version** can be safely installed. So in this example, `^16.12.1` can be safely installed if this was the newest version in `16.x`.

Sometimes a version has a `~` in front of it (e.g. `~16.8.6`). This means that only the latest **patch version** can be safely installed. So in this example, `^16.8.12` can be safely installed if this was the newest version in `16.8.x`.

## So, `npm install` installs the latest safe version of the dependencies?

Yes and no!

If the packages have already been installed into the `node_modules` folder, then `npm install` **won’t** update any packages.

If the packages haven’t been installed and a `package-lock.json` file exists, then `npm install` **will** install the **EXACT** dependency versions specified in `package-lock.json`.

`npm install` will install the latest **safe version** of the dependencies if they don’t exist in the `node_modules` folder and, there is **no** `package-lock.json` file. However, you may think the latest **safe version** hasn’t been installed because `package.json` is unchanged, but if you check the packages in the `node_modules` folder, the latest safe version will have been installed.

## So, how do I safely update all the dependencies?

Firstly, the dependencies that are out of date can be discovered by running the following command:

```
npm outdated
```

The **wanted version** is the latest **safe version** that can be taken (according to the **semantic version** and the `^` or `~` prefix). The latest version is the latest version available in the `npm` registry.

All the dependencies can be safely `updated` to the **wanted version** by using the following command:

```
npm update
```

As well as updating the packages in the `node_modules` folder, the `package.json` and `package-lock.json` files **will be updated**.

If we **don’t** want to update **all** of the packages, then the package names can be specified at the end of the command:

```
npm update "react" "react-dom"
```

`React` is updated in the above example.

## Updating all dependencies with major changes

So, how do we upgrade dependencies when there has been a major version change?

Perhaps the safest way is as follows:

-   Check the changelog of the dependent package for breaking changes that could affect our app
-   If we think we are safe to do the upgrade, run the following command:

```
npm install <packagename>@latest
```

If multiple packages go together, you can list them all out. The example below will update React to the latest version:

```
npm install react@latest react-dom@latest
```

-   Verify the app isn’t broken by doing some tests
-   Repeat the process for other packages where there is a major version change

## Is there a quicker way of just updating all the dependencies, including major version changes?

So, like `npm update` but for major version updates as well?

```
npx npm-check-updates -u
```

This will update the dependencies to the latest versions (including major version changes) in the `package.json` file. If we are happy to go ahead with the upgrades we need to run the following command:

```
npm install
```

This will then upgrade the packages in the `node_modules` folder, and the `package-lock.json` file will be updated as well.
