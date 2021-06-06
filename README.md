# Yarn 2 workspaces monorepo example

This project demonstrates how to set up a monorepo with Yarn 2.

You can either:

-   clone the repository or
-   set the project up from scratch

## 1. Cloning the repo

In your work directory, run:

```
git clone https://github.com/marqetintl/yarn-2-workspaces-monorepo-example.git
```

Install the required dependencies:

```
cd yarn-2-workspaces-monorepo-example
yarn
```

Jump to [step 3](https://github.com/marqetintl/yarn-2-workspaces-monorepo-example#3-to-run-the-project)

## 2. Setup from scratch

> Always refer to the [original documentation](https://yarnpkg.com/getting-started/install) to get the latest instructions on how to install Yarn.

1. Install Yarn globally, if you haven't:

```
npm install -g yarn
```

2. Setup project folder

```
mkdir yarn-2-workspaces-monorepo-example && cd $_
```

3. Set Yarn version to latest

```
yarn set version berry
yarn set version latest
```

```
yarn init -w
```

Here is the current folder tree(`tree -a -L 1`)

```
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

Now, let's add two packages to our monorepo.

```
cd packages/
mkdir myapp-1 myapp-2
```

Navigate to `myapp-2` and add an `index.js` file.

```
cd myapp-2/
yarn init -yp
touch index.js
```

Update `packages/myapp-2/index.js`.

```
export const log = console.log;
```

Update `packages/myapp-2/package.json`.

```
{
    "name": "@miq/myapp-2",
    "main": "index.js"
}
```

Navigate to `myapp-1` and add an `index.js` file.

```
cd ../myapp-1/
yarn init -yp
touch index.js
```

Update `packages/myapp-1/index.js`

```
const { defaultName, logName } = require("@miq/myapp-2");

logName(defaultName);
logName("John");
```

Then update `packages/myapp-1/package.json`

```
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

Update `package.json`

```
{
    "name": "@miq/yarn-2-workspaces-monorepo-example",
    "private": true,
    "workspaces": [
        "packages/*"
    ],
    "scripts": {
        "start:myapp1": "yarn workspace @miq/myapp-1 start"
    }
}
```

From the root folder, run `yarn` to install the dependencies:

```
cd ../../
yarn
```

## 3. To run the project

```
yarn start:myapp1
```

You should see the output:

```
...

Hello! I am Michaël
Hello! I am John

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
