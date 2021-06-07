# Yarn 2 workspaces monorepo example

This project demonstrates how to set up a monorepo with Yarn 2.

You can either:

-   clone the repository or
-   set the project up from scratch

## 1. Cloning the repo

In your work directory, run:

```bash
git clone https://github.com/marqetintl/yarn-2-workspaces-monorepo-example.git
```

Install the required dependencies:

```bash
cd yarn-2-workspaces-monorepo-example
yarn
```

Jump to [step 3](https://github.com/marqetintl/yarn-2-workspaces-monorepo-example#3-to-run-the-project)

## 2. Setup from scratch

> Always refer to the [original documentation](https://yarnpkg.com/getting-started/install) to get the latest instructions on how to install Yarn.

1. Install Yarn globally, if you haven't:

```bash
npm install -g yarn
```

2. Create project folder:

```bash
mkdir yarn-2-workspaces-monorepo-example && cd $_
```

3. Set Yarn version to latest:

```bash
yarn set version berry
yarn set version latest
```

4. Initialize project:

```bash
yarn init -w
```

If I were to run `tree -a -L 1` , the folder tree would look like this:

```bash
.
├── .editorconfig
├── .git
├── .gitattributes
├── .gitignore
├── .yarn
├── .yarnrc.yml
├── README.md
├── package.json
├── packages
└── yarn.lock
```

5. Add two packages `myapp-1` and `myapp-2` to our monorepo:

```bash
cd packages/
mkdir myapp-1 myapp-2
```

6. Navigate to `packages/myapp-2/` and add an `index.js` file:

```bash
cd myapp-2/
yarn init -yp
touch index.js
```

7. Update `packages/myapp-2/index.js`:

```js
const defaultName = "Michaël";

module.exports = {
    defaultName,
    logName(name) {
        return console.log(`Hello! I am ${name}`);
    },
};
```

8. Update `packages/myapp-2/package.json`:

```json
{
    "name": "@miq/myapp-2",
    "main": "index.js"
}
```

9. Navigate to `packages/myapp-1` and add an `index.js` file:

```bash
cd ../myapp-1/
yarn init -yp
touch index.js
```

10. Update `packages/myapp-1/index.js`:

```js
const { defaultName, logName } = require("@miq/myapp-2");

logName(defaultName);
logName("Jean-Michel");
```

11. Then update `packages/myapp-1/package.json`:

```json
{
    "name": "@miq/myapp-1",
    "dependencies": {
        "@miq/myapp-2": "workspace:*"
    },
    "scripts": {
        "start": "node ."
    }
}
```

12. Update the project's `package.json`:

```json
{
    "name": "@miq/yarn-2-workspaces-monorepo-example",
    "private": true,
    "workspaces": ["packages/*"],
    "scripts": {
        "start:myapp1": "yarn workspace @miq/myapp-1 start"
    }
}
```

13. From the root folder, run `yarn` to install the dependencies:

```bash
cd ../../
yarn
```

## 3. To run the project

```bash
yarn start:myapp1
```

You should see the output:

```bash
...

Hello! I am Michaël
Hello! I am Jean-Michel

...

```

## 4. Good to know

-   List all workspaces:

```
yarn workspaces list
```

-   Create a new private package and define it as a workspace root with a `packages/` directory:

```
yarn init -w
```

-   Initialize a private package in the local directory:

```
yarn init -p
```
