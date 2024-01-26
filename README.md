# <center>Node TypeScript Express MongoDB Starter</center>

## <center>This repository provides an initial structure for a professional Node application using TypeScript and Express, with MongoDB and Mongoose for the database. You can follow the following steps or directly clone this repository. </center>

### <center>[If you clone this repository, make sure to run `npm install` to install all the dependencies and devDependencies]</center>

## 1. <u> Create the Project </u>

```bash
mkdir first-project
cd first-project
npm init -y
code .
```

Now the VSCode will be opened.

## 2. <u> Install these packages(dependencies): </u>

```bash
npm i express mongoose cors dotenv
```

## 3. <u> Install TypeScript and Types(devDependencies): </u>

```bash
npm i typescript ts-node-dev @types/node @types/express @types/cors --save-dev
```


## 4. <u> Modify the tsconfig file: </u>

```bash
tsc -init
```

Now __`tsconfig.json`__ will be created at the root.

Now open the `tsconfig.json` file and make these changes (use CTRL + F to find these properties):

```json
  "rootDir": "./src",
```

```json
  "outDir": "./dist",
```

also at the top -->

```json
{
  "include": ["src"],
  "exclude": ["node_modules"]

```
}


Remember - these two properties above will be outside of __`"compilerOptions"`__. That means there should be total three properties in the main json object - __`"include"`__, __`"exclude"`__ and __`"compilerOptions"`__.

## <u>5. Install and Configure ESLint</u>

```bash
npm install eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev
npm init @eslint/config
```

After the installation is complete -->

```bash
npm init @eslint/config
```

#### Now you'll need to answer some questions. Don't blindly follow these answers if your project is different. Nevertheless here is my answers --

```bash
  How would you like to use ESLint -- To check syntax and find problems

  What type of modules does your project use -- JavaScript modules (import/export)

  Which framework does your project use -- None of these

  Does your project use Typescript? -- Yes

  Where does your code run? -- Node

  What format do you want your config file to be in? -- JSON
  
  @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest
  Would you like to install them now? -- No [Notice you have already installed them. So you can select No]

```

### Now, a file will be created named eslintrc.json at the root. In this file - set these rules:


```json
{
  "rules": {
    "no-unused-expressions": "error",
    "prefer-const": "error",
    "no-console": "warn",
    "no-undef": "error"
  },
  "globals": {
    "process": "readonly"
  }
}
```


## 5. <u> Update `package.json` scripts: </u>

  ```json
  "scripts": {
    "start:prod": "node ./dist/server.js",
    "start:dev": "ts-node-dev --respawn --transpile-only src/server.ts",
    "build": "tsc",
    "lint": "eslint src --ignore-path .eslintignore --ext .ts",
    "lint:fix": "npx eslint src --fix"
    "test": "echo \"Error: no test specified\" && exit 1",
  },
  ```


## 6. <u> Create `.gitignore`, `.eslintignore`, `.env` in the root: </u>

In the root of the application, create three files - `.gitignore`, `.eslintignore`, `.env`

`.gitignore`:

```text
node_modules
dist
.env
```

`.eslintignore`:

```text
node_modules
dist
.env
```

`.env`:
```text
NODE_ENV=development
PORT=5000
DATABASE_URL=mongodb://localhost:27017
```

[Here the database url is for locally connecting the mongodb server. This is the default URI of MongoDBCompass]

# Folder Structure

### Follow the folder structure of this directory or use the following steps:

## Setp 1:

- Create a __folder__ named `src` in the __root__.
- In the `src` folder, create another __folder__ named `app`.
- In the `app` folder, create a __folder__ named `config`.
- In `config` folder, create a __file__ named `index.ts`.


</u> Now in the `src/app/config/index.ts`:</u>

```typescript
import dotenv from 'dotenv';
import path from 'path';

dotenv.config({ path: path.join(process.cwd(), '.env') })

export default {
  port: process.env.PORT,
  database_url: process.env.DATABASE_URL
}
```

### Explanation:

- `dotenv.config` takes an object as the parameter, where the `path` property takes the pathname of the file as a string.
- `process.cwd()` gives the current directory, and `path.join` joins two paths. `path` is a NodeJs default module.


## Step 2:

In the `src` folder, create two __files__ named `app.ts` and `server.ts`.

### In `src/app.ts`:

```typescript
import express, { Application, Request, Response } from 'express';
import cors from 'cors';
const app: Application = express()

// parsers

app.use(express.json());
app.use(cors());

app.get('/', (req: Request, res: Response) => {
  res.send('Hello World!')
})

export default app;
```

### In `src/server.ts`:

```typescript
/* eslint-disable no-console */
import app from "./app";
import mongoose from "mongoose";
import config from "./app/config";

const { port, database_url } = config;

async function main() {
  try {
    await mongoose.connect(database_url as string);

    app.listen(port, () => {
      console.log(`Server listening on port ${port}`)
    })
  } catch (err) {
    console.log(err)
  }
}

main();
```


Your setup is complete. 

### <u>To start the application:</u>

```bash
npm run start:dev
```
You should see `Server listening on port 5000` on the console. And everytime you make any change, the server will restart automatically.


### <u>To build the file:</u>

```bash
npm run build
```

Now a new folder named `dist` will appear in the root. Here all of your TypeScript codes will be converted to JavaScript. For deploying, you will use the `dist` folder.

### <u>To start the production server:</u>

To run this, you should __build__ the application first.

```bash
npm run start:prod
```
You should see `Server listening on port 5000` on the console.

### Now if you run `npm run start:dev`, your development server will start. And everytime you make any change, the server will restart automatically. 
