# Yarn 2 workspaces simple monorepo example

Either clone the repository or follow the following steps to setup the project

### 1. Install Yarn and project setup

A project is

Always refer to the [original documentation](https://yarnpkg.com/getting-started/install) to get the latest instructions.

```
npm install -g yarn
mkdir yarn-2-workspaces-monorepo-example && cd $_
yarn set version berry
yarn set version latest
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

Now, let's add two apps to our monorepo

```
cd packages/
mkdir myapp-1 myapp-2
```

```
cd myapp-2/
yarn init -yp
touch index.js
```

Update `packages/myapp-2/index.js`

```
export const log = console.log;
```

Update `packages/myapp-2/package.json`

```
{
    "name": "@miq/myapp-2",
    "main": "index.js"
}
```

Navigate to `myapp-1`

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

From the root folder, run:

```
cd ../../
yarn
```

To test, run

```
yarn
```

You should see the output:

```
yarn start:myapp1
...

Hello! I am Michaël
Hello! I am John

...

```

## Good to know

1. List all workspaces:

```
yarn workspaces list
```

2. Create a new private package and define it as a workspace root with a `packages/` directory:

```
yarn init -w
```

3. Initialize a private package in the local directory:

```
yarn init -p
```
