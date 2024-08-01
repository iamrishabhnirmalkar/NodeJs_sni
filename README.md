# Node Js Backend - Production Setup

-   [Node Project](#node-project)
-   [Git & Github](#git--github)
-   [Husky](#husky)
-   [TypeScript](#typescript)
-   [Folder Structure](#folder-structure)
-   [Commit Lint](#commit-lint)
-   [ES Lint](#es-lint)
-   [Prettier](#prettier)
-   [Project Environment](#project-environment)
-   [Express Js](#express-js)
-   [Global Error Handler](#global-error-handler)
-   [404 Handler](#404-handler)
-   [Logger](#logger)
-   [Source Map](#source-map)
-   [Colorful Terminal](#colorful-terminal)
-   [MongoDB](#mongodb)
-   [Database Log Storage](#database-log-storage)
-   [Database Migration](#database-migration)
-   [Health Endpoint]
-   [Security - Helmet JS]
-   [Security - CORS]
-   [Security - Rate Limiting]
-   [Dependency Updates]
-   [Docker]

## Node Project

Initialize a Node.js project:

```bash
npm init -y
```

After that, the package.json file will be created. Replace the test script with start so that you can easily run the server.js file with _npm start_.

```json
   "test": "echo \"Error: no test specified\" && exit 1"
  "start": "node server.js"
```

## Git & Github

Run the following command to initialize Git:

```bash
git init
```

This will create the .git folder in your project. Next, add and commit your files:

```bash
git add .
git commit -am 'feat: Node project setup'
```

Create a repository on GitHub and push your code:

```bash
git remote add origin https://github.com/your-username/your-repo.git
git push origin master
```

Create a `.gitignore` file to exclude files from being pushed to GitHub:

```sh
node_modules
dist
.env
```

## Husky

### About Husky

Husky is a popular tool used in Git repositories to manage and enforce Git hooks. Git hooks are scripts that Git executes before or after events such as committing, pushing, and receiving changes.

Install Husky and lint-staged:

```bash
npm i husky lint-staged -D
```

Initialize Husky:

```bash
npx husky init
```

After this, the .husky folder will be created. Create a pre-commit hook:

## TypeScript

Install TypeScript as a dev dependency:

```bash
npm i typescript -D
```

Initialize TypeScript configuration:

```bash
npx tsc --init
```

In tsconfig.json, enable the following options:

```json
{
    "compilerOptions": {
        "target": "es2016",
        "module": "commonjs",
        "rootDir": "./src",
        "outDir": "./dist",
        "removeComments": true,
        "esModuleInterop": true,
        "forceConsistentCasingInFileNames": true,
        "strict": true,
        "noImplicitAny": true,
        "strictFunctionTypes": true,
        "strictNullChecks": true,
        "strictPropertyInitialization": true,
        "noUnusedParameters": true,
        "noUnusedLocals": true,
        "alwaysStrict": true,
        "noImplicitReturns": true
    }
}
```

Install type definitions for Node.js:

```bash
npm i -D @types/node
```

Install nodemon and ts-node:

```bash
npm i nodemon ts-node -D

```

Add the following scripts to your package.json:

```json
  "scripts": {
  "start": "nodemon src/server.ts",
  "build": "tsc",
  "dev": "nodemon dist/server.js"
}
```

## Folder Structure

First, run the following command in the root directory to create the primary folders:

```sh
mkdir docker logs nginx scripts public test

```

Next, navigate to the src directory and create its subdirectories:

```sh
mkdir -p src/config src/constants src/controllers src/middlewares src/models src/routes src/services src/types src/utils src/views

```

### Folder Descriptions

1. docker: Contains Docker-related files and configurations, such as Dockerfiles and docker-compose.yml.
2. logs: Stores log files generated by the application. Helps in debugging and monitoring.
3. nginx: Contains Nginx configuration files if you're using Nginx as a reverse proxy or for static file serving.
4. scripts: Holds utility scripts for tasks like database seeding, backups, or other automation tasks.
5. public: Used for serving static files like images, CSS, and JavaScript to the client.
6. test: Contains test files for unit tests, integration tests, and end-to-end tests.

### src Directory Structure

1. config: Configuration files for the application. This can include database configurations, environment variables, and other setup-related files.
2. constants: Defines constant values used throughout the application, such as status codes, messages, or configuration keys.
3. controllers: Contains the controllers that handle incoming requests and return responses. Controllers coordinate between models and views.
4. middlewares: Middleware functions that process requests before they reach the controllers. Useful for authentication, logging, or error handling.
5. models: Defines the data models or schemas for interacting with the database.
6. routes: Manages the routing of requests to the appropriate controllers.
7. services: Contains business logic and service layer functions. Services interact with models and perform operations needed by controllers.
8. types: Defines TypeScript types and interfaces used throughout the application for type checking and consistency.
9. utils: Utility functions and helpers that can be reused across the application.
10. views: Stores template files or view-related code if using server-side rendering.1.

## Commit Lint

### About

Commit Lint is a tool used to enforce consistent commit message formats in a Git repository. It helps ensure that commit messages follow specified guidelines, which can be particularly useful in maintaining a readable and meaningful commit history.

Install Commit Lint and the conventional config package:

```bash
npm i @commitlint/cli @commitlint/config-conventional -D
```

#### Setup

Create a .husky/commit-msg file to add the commit-msg hook:

```json
#!/user/bin/env sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit "$1"

```

Create a commitlint.config.js file in the root directory of your project with the following content:

```json

module.exports = {
  extends: ["@commitlint/cli", "@commitlint/config-conventional"],
  rules: {
    "type-enum": [
      2,
      "always",
      [
        "feat",
        "fix",
        "docs",
        "style",
        "refactor",
        "perf",
        "test",
        "build",
        "ci",
        "chore",
        "revert",
      ],
    ],

    "subject-case": [2, "always", "sentence-case"],
  },
};

```

### Commit Message Guidelines

By using this configuration, you should always commit in a proper way. Here are some examples:

-   Feature:

```bash
git commit -m "feat: This is a new feature"
```

-   Bug Fix:

```bash
git commit -m "fix: This is a bug fix"
```

1. feat: A new feature for the user.
2. fix: A bug fix for the user.
3. docs: Documentation only changes.
4. style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc.).
5. refactor: A code change that neither fixes a bug nor adds a feature.
6. perf: A code change that improves performance.
7. test: Adding missing tests or correcting existing tests.
8. build: Changes that affect the build system or external dependencies.
9. ci: Changes to CI configuration files and scripts.
10. chore: Other changes that don't modify src or test files.
11. revert: Reverts a previous commit.

## ES Lint

### About

ESLint statically analyzes your code to quickly find problems. It is built into most text editors, and you can run ESLint as part of your continuous integration pipeline. Use this link [TypeScript ESLint](https://typescript-eslint.io/)

Install ESLint and the necessary dependencies:

```bash
npm install --save-dev eslint @eslint/js @types/eslint__js typescript-eslint
```

Create a configuration file named eslint.config.mjs and paste the following content:

```sh
import eslint from "@eslint/js";
import tseslint from "typescript-eslint";

export default tseslint.config({
  languageOptions: {
    parserOptions: {
      project: true,
      tsconfigRootDir: import.meta.dirname,
    },
  },
  files: ["**/*.ts"],
  extends: [
    eslint.configs.recommended,
    ...tseslint.configs.recommendedTypeChecked,
  ],
  rules: {
    "no-console": "error",
    quotes: ["error", "single", { allowTemplateLiterals: true }],
  },
});

```

Install the following extensions in VS Code:

[ESLint by Microsoft] (https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
[Error Lens by Alexander] (https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens)

Add the following scripts to your package.json:

```json
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix"
}
```

Add the following configuration for lint-staged in your package.json:

```json
 "lint-staged": {
  "*.ts": [
    "npm run lint:fix"
  ]
}
```

In your Husky folder, create a pre-commit file with the following content:

```sh
#!/usr/bin/env sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged


```

To run ESLint and fix issues based on the rules in your eslint.config.mjs file, use the following commands:

```bash
npm run lint
npm run lint:fix
```

## Prettier

### About

Prettier is an opinionated code formatter that enforces a consistent style by parsing your code and reprinting it with its own rules that take the maximum line length into account, wrapping code when necessary. It supports many languages and integrates with most editors.
[Prettier](https://prettier.io/)

Install Prettier as a dev dependency:

```bash
npm install --save-dev --save-exact prettier


```

Also, use the Prettier code formatter extension for VS Code:

[Prettier - Code formatter by Prettier] (https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

Create a `.prettierrc` file in the root directory of your project and define your preferred Prettier rules. Here’s an example configuration:

```json
{
    "trailingComma": "none",
    "tabWidth": 4,
    "semi": false,
    "singleQuote": true,
    "printWidth": 150,
    "singleAttributePerLine": true,
    "endOfLine": "crlf"
}
```

### ESLint (and other linters)

If you use ESLint, install eslint-config-prettier to make ESLint and Prettier work well together. This turns off all ESLint rules that are unnecessary or might conflict with Prettier.

```bash
npm install --save-dev eslint-config-prettier


```

Update your eslint.config.mjs file to include Prettier’s configuration:

```js

import eslintconfigPrettier from 'eslint-config-prettier'
  extends: [ eslintconfigPrettier],

```

Add the following scripts to your package.json to check and fix code formatting:

```json
"scripts": {
 "format:check": "prettier . --check",
        "format:fix": "prettier . --fix"
}

```

Add the following configuration for lint-staged in your package.json:

```json
"lint-staged": {
  "*.ts": [
    "npm run format:fix"
  ]
}
```

## Project Environment

### About ENV

Environment variables (env) are used to configure applications without hardcoding values into the source code. They provide flexibility and security by storing configuration settings, such as database credentials and API keys, separately from the codebase. This allows for easy changes across different environments (development, staging, production) without modifying the code. Using environment variables helps keep sensitive information secure and supports best practices for clean, maintainable code.

### cross env

cross-env is a tool that helps you set environment variables across different platforms (like Windows, macOS, and Linux) in a consistent way. In many cases, environment variables are used to configure different settings for your application, such as API keys, environment modes, or build configurations.

First, install `dotenv-flow` and `cross-env`:

```sh
npm i dotenv-flow
npm i corss-env

```

Write script in package.json file

```json
"scripts": {
  "dist": "npx tsc",
  "start": "cross-env NODE_ENV=production nodemon dist/server.js",
  "dev": "cross-env NODE_ENV=development nodemon src/server.ts"
}
```

Create a configuration file in the config folder and set up the configuration for the environment variables.

`src/config/config.ts`

```ts
import DotenvFlow from 'dotenv-flow'

DotenvFlow.config()
// console.log(process.env)

export default {
    ENV: process.env.ENV,
    PORT: process.env.PORT,
    SERVEL_URL: process.env.SERVAL_URL,
    DATABASE_URL: process.env.DATABASE_URL
}
```

for write script in the package.json file

```json
"dist": "npx tsc",
"start": "cross-env NODE_ENV=production nodemon src/server.ts",
"dev": "cross-env NODE_ENV=development nodemon dis/server.js",
```

To test this configuration, import the config.ts file in your server.ts file and log the configuration object:

```ts
import config from './config/config'
console.log(config)
```

after run this by npm run start you will see the object of the Env prodcution

## Express Js

### About

Express.js is a tool that helps you build web applications and APIs using Node.js. It simplifies the process of handling different web requests (like GET, POST, etc.) and managing server responses.

First, install Express.js and its type definitions:

```bash
npm install express
npm install --save-dev @types/express
```

#### Entry Point

Create an app.ts file to initialize the Express application. Make sure to define its type as Application from Express:

```ts
import express, { Application } from 'express'

const app: Application = express()

export default app
```

#### Server Setup

Create a server.ts file to import and configure the app, incorporating environment variables from a config.ts file:

```ts
import config from './config/config'
import app from './app'

const server = app.listen(config.PORT)
```

An Immediately Invoked Function Expression (IIFE) is a JavaScript function that runs as soon as it is defined. It's useful in several scenarios, particularly in the context of your Express application

```ts
import config from './config/config'
import app from './app'

const server = app.listen(config.PORT)

;(() => {
    try {
        console.info(`APPLICATION_STARTED`, {
            meta: {
                PORT: config.PORT,
                SERVAL_URL: config.SERVEL_URL
            }
        })
    } catch (err) {
        console.error(`APPLICATION_ERROR`, {
            meta: err
        })
        server.close((error) => {
            if (error) {
                console.error(`APPLICATION_ERROR`, {
                    meta: err
                })
            }
            process.exit(1)
        })
    }
})()
```

#### Running the Server

Check whether your server is running with:

```bash
npm run dev

```

If the server is working correctly, you should see:

APPLICATION_STARTED { meta: { PORT: '3000', SERVER_URL: 'http://localhost:3000' } }

If not, check the error messages and resolve any issues.

#### Middleware

In app.ts, add middleware to handle JSON data and serve static files:

```ts
import path from 'path'
app.use(express.json())
app.use(express.static(path.join(__dirname, '../', 'public')))
```

#### Router

Create a router in the routes folder. In apiRouter.ts, define your routes and bind them to controllers:

```ts
import { Router } from 'express'

const router = Router()

router.route('/self').get(apiController.self)
export default router
```

Then, call the router in app.ts:

```ts
import router from './routes/apiRouter'

app.use('/api/v1', router)
```

#### Controller

Create a controller in controllers folder. In apiController.ts, define the controller actions:

```ts
import { Request, Response } from 'express'
export default {
    self: (_: Request, res: Response) => {
        try {
            res.sendStatus(200)
        } catch (error) {
            res.sendStatus(500)
        }
    }
}
```

Test the endpoint with Postman or Thunder Client. You should see Status: 200 OK.
If not, check the error messages and resolve any issues.

#### Types

Define types for HTTP responses and errors in the types folder. Create a types.ts file:

```ts
export type THttpResponse = {
    success: boolean
    statusCode: number
    request: {
        ip?: string | null
        method: string
        url: string
    }
    message: string
    data: unknown
}
export type THttpError = {
    success: boolean
    statusCode: number
    request: {
        ip?: string | null
        method: string
        url: string
    }
    message: string
    data: unknown
    trace?: object | null
}
```

-   success: Indicates whether the response is successful (true) or failed (false).
-   statusCode: HTTP status code (e.g., 200 for OK, 500 for Server Error, etc.).
-   request: An object containing details about the request:
    -   ip: The IP address of the user (string or null).
    -   method: The HTTP method (GET, POST, DELETE, PATCH).
    -   url: The requested URL.
-   message: Custom message for the response.
-   data: Contains the response data, which can be of any type (JSON).
-   trace: An optional object for error tracing, used to trace the error in which line of code.

#### Context Variables

Create context variables for global use. Create application.ts and responseMessage.ts files.

`responseMessage.ts`

```ts
export default {
    SUCCESS: `The Operation has been successful`,
    SOMETHING_WENT_WRONG: `error`,
    NOT_FOUND: (entity: string) => `${entity} not found`
}
```

`application.ts`

```ts
export enum EApplicationEnvironment {
    PRODUCTION = 'production',
    DEVELOPMENT = 'development'
}
```

#### Utils

HTTP Response

Create a httpResponse.ts file in the utils folder to handle structured JSON responses:

```ts
import { Request, Response } from 'express'
import { THttpResponse } from '../@types/types'
import config from '../config/config'
import { EApplicationEnvironment } from '../constants/application'

export default (req: Request, res: Response, responseStatusCode: number, responseMessage: string, data: unknown = null): void => {
    const response: THttpResponse = {
        success: true,
        statusCode: responseStatusCode,
        request: {
            ip: req.ip || null,
            method: req.method,
            url: req.originalUrl
        },
        message: responseMessage,
        data: data
    }

    //log
    console.info(`CONTROLLER_RESPONSE`, {
        meta: response
    })

    //Production Env check
    if (config.ENV === EApplicationEnvironment.PRODUCTION) {
        delete response.request.ip
    }

    res.status(responseStatusCode).json(response)
}
```

Error Handling

Create an errorObjects.ts file to handle error objects:

```ts
import { Request } from 'express'
import { THttpError } from '../@types/types'
import responseMessage from '../constants/responseMessage'
import config from '../config/config'
import { EApplicationEnvironment } from '../constants/application'

// eslint-disable-next-line @typescript-eslint/no-redundant-type-constituents
export default (err: Error | unknown, req: Request, errorStatusCode: number = 500): THttpError => {
    const errorObj: THttpError = {
        success: false,
        statusCode: errorStatusCode,
        request: {
            ip: req.ip || null,
            method: req.method,
            url: req.originalUrl
        },
        message: err instanceof Error ? err.message || responseMessage.SOMETHING_WENT_WRONG : responseMessage.SOMETHING_WENT_WRONG,
        data: null,
        trace: err instanceof Error ? { error: err.stack } : null
    }

    //log
    console.info(`CONTROLLER_ERROR`, {
        meta: errorObj
    })

    //Production Env check
    if (config.ENV === EApplicationEnvironment.PRODUCTION) {
        delete errorObj.request.ip
        delete errorObj.trace
    }

    return errorObj
}
```

Create an httpError.ts file to handle error response:

```ts
import { NextFunction, Request } from 'express'
import errorObjects from './errorObjects'

// eslint-disable-next-line @typescript-eslint/no-redundant-type-constituents
export default (nextFunc: NextFunction, err: Error | unknown, req: Request, errorStatusCode: number = 500): void => {
    const errorObj = errorObjects(err, req, errorStatusCode)
    return nextFunc(errorObj)
}
```

Import all utils in the apiController file

```ts
import { NextFunction, Request, Response } from 'express'
import httpResponse from '../utils/httpResponse'
import responseMessage from '../constants/responseMessage'
import httpError from '../utils/httpError'

export default {
    self: (req: Request, res: Response, next: NextFunction) => {
        try {
            httpResponse(req, res, 200, responseMessage.SUCCESS, {})
        } catch (error) {
            httpError(next, error, req, 500)
        }
    }
}
```

## Global Error Handler

Create a folder named middleware and add a file globalErrorHandler.ts. This middleware will handle errors globally and ensure consistent error responses in JSON format instead of HTML.

`middleware/globalErrorHandler.ts`

```ts

import { Request, Response, NextFunction } from 'express'
import { THttpError } from '../@types/types'

// eslint-disable-next-line @typescript-eslint/no-unused-vars
export default (err: THttpError, \_: Request, res: Response, \_\_: NextFunction) => {
res.status(err.statusCode).json(err)
}

```

To use the global error handler, import it and apply it as middleware in your app.ts file:

```ts
//Global Error handler
app.use(globalErrorHandler)
```

## 404 Handler

To handle 404 errors properly and respond in a structured way, add the following code to your app.ts file:

```ts
import { Request, Response, NextFunction } from 'express'
// 404 Error Handler
app.use((req: Request, _: Response, next: NextFunction) => {
    try {
        throw new Error(responseMessage.NOT_FOUND('route'))
    } catch (error) {
        httpError(next, error, req, 500)
    }
})
```

This code snippet throws a 404 error when a route does not exist and uses the httpError utility to handle the error properly.

## Logger

### About

A logger is a tool used in software development to record log messages. These messages are typically used for debugging, monitoring, and auditing purposes. In the context of a Node.js/Express.js application, a logger can help track the application's behavior and identify issues.

To use a logger in your Express.js application, you can use popular logging libraries such as winston or morgan. Here's an example using winston:

Install winston
First, install winston:

```bash
npm install winston

```

Configure winston
Create a file logger.ts in a utils or middleware directory:

Make custom log format for console

`utils/logger.ts`

```ts
import { createLogger, format, transports } from 'winston'
import { ConsoleTransportInstance } from 'winston/lib/winston/transports'
import util from 'util'

// Custom log format for console
const consoleLogFormat = format.printf((info) => {
    // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
    const { level, message, timestamp, meta = {} } = info
    const customLevel = level.toUpperCase()
    // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
    const customTimestamp = timestamp
    // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
    const customMessage = message
    // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
    const customMeta = util.inspect(meta, {
        showHidden: false,
        depth: null
    })

    const customLog = `${customLevel} [${customTimestamp}] ${customMessage}\n${'META'}: ${customMeta}\n`
    return customLog
})

// Console transport configuration
const consoleTransport = (): Array<ConsoleTransportInstance> => {
    return [
        new transports.Console({
            level: 'info',
            format: format.combine(format.timestamp(), consoleLogFormat)
        })
    ]
}

// Create and export the logger
export default createLogger({
    defaultMeta: {
        meta: {}
    },
    transports: consoleTransport()
})
```

Replace all `console.info` into the `logger.info`

Make Custom log format for file

```ts
import { createLogger, format, transports } from 'winston'
import { FileTransportInstance } from 'winston/lib/winston/transports'
import util from 'util'
import config from '../config/config'
import { EApplicationEnvironment } from '../constants/application'
import path from 'path'

// Custom log format for file
const fileLogFormat = format.printf((info) => {
    // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
    const { level, message, timestamp, meta = {} } = info
    const logMeta: Record<string, unknown> = {}

    // eslint-disable-next-line @typescript-eslint/no-unsafe-argument
    for (const [key, value] of Object.entries(meta)) {
        if (value instanceof Error) {
            logMeta[key] = {
                name: value.name,
                message: value.message,
                trace: value.stack || ''
            }
        } else {
            logMeta[key] = value
        }
    }

    const logData = {
        level: level.toUpperCase(),
        // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
        message,
        // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
        timestamp,
        meta: logMeta
    }

    return JSON.stringify(logData, null, 4)
})

// File transport configuration
const fileTransport = (): Array<FileTransportInstance> => {
    return [
        new transports.File({
            filename: path.join(__dirname, '../', '../', 'logs', `${config.ENV}.log`),
            level: 'info',
            format: format.combine(format.timestamp(), fileLogFormat)
        })
    ]
}

// Create and export the logger
export default createLogger({
    defaultMeta: {
        meta: {}
    },
    transports: [...fileTransport()]
})
```

  <!-- ====================================================================================== -->

## Source Map

### About

A source map is a file that maps transformed or compiled code (such as minified JavaScript) back to the original source code (like TypeScript, SCSS, or ES6). This mapping allows developers to debug their code more effectively by letting them see the original source code in the browser's developer tools, even though the running code has been minified or compiled into a different format.

When you build a TypeScript project, the build process generates JavaScript files for production. Errors that occur will be shown in these JavaScript files, which can make debugging difficult. However, by using source maps, you can trace back errors to the original TypeScript files, making it easier to debug.

Install source-map-support and its type definitions:

```sh
npm i source-map-support
npm i @types/source-map-support -D

```

Enable source maps in your tsconfig.json file:

```json
{
    "compilerOptions": {
        "sourceMap": true
    }
}
```

After enabling source maps, a .map file will be generated in the dist folder during the build process. This file binds the generated JavaScript back to the original TypeScript code.

To utilize source maps in your code for better error tracing, add the following in your logger.ts file:

```ts
import * as sourcemapsupport from 'source-map-support'

// Linking Trace support
sourcemapsupport.install()
```

## Colorful Terminal

### About

Using color in the terminal can help you easily distinguish errors, warnings, and other log messages according to your preferences. In this project, we use Colorette to colorize terminal output, making it easier to identify different log levels and metadata.

To install Colorette, run:

```sh
npm i colorette

```

To customize colors or styles according to your preferences, add the following code to your logger.ts file:

```ts
import { red, blue, yellow, green, magenta } from 'colorette'

const colorizelevel = (level: string) => {
    switch (level) {
        case 'ERROR':
            return red(level)
        case 'INFO':
            return blue(level)
        case 'WARN':
            return yellow(level)
        default:
            return level
    }
}
```

To apply these colors in your logger, use the colorizeLevel function:

```ts
const customLevel = colorizelevel(level.toUpperCase())
```

For example, to colorize the META information in your logs, you can use:

```ts
const customLog = `${customLevel} [${customTimestamp}] ${customMessage}\n${magenta('META')}: ${customMeta}\n`
```

To apply multiple styles or colors, you can combine them like this:

```ts
import { red, bold } from 'colorette'
const styledLevel = red(bold(level))
```

## MongoDB

### About

MongoDB is a NoSQL database that can be used to store data. While MySQL is also commonly used, MongoDB is particularly popular for its flexibility and scalability. You can use MongoDB or any other database according to your requirements. In this guide, we'll focus on using MongoDB with Mongoose for managing database connections in a Node.js application.

Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. To install Mongoose, run:

```sh
npm install mongoose
```

Create a .env.production or .env.development file in the root directory of your project and add the MongoDB connection URL. The URL includes the database name after the last /.

```env
DATABASE_URL = "mongodb://localhost:27017/node-js-snippet"
```

In your services folder, create a file named databaseService.ts for handling the database connection:

```ts
import mongoose from 'mongoose'
import config from '../config/config'

export default {
    connect: async () => {
        try {
            await mongoose.connect(config.DATABASE_URL as string)
            return mongoose.connection
        } catch (error) {
            throw error
        }
    }
}
```

In your server.ts file, import and use the databaseService to connect to the database. Make sure to handle async/await correctly:

```ts
import databaseService from './services/databaseService'
//DataBase Connection
const connection = await databaseService.connect()
logger.info(`DATABASE_CONNECTION`, {
    meta: {
        CONNECTION_NAME: connection.name
    }
})
```

If you encounter issues with async/await and ESLint, you may need to adjust your ESLint configuration. Add the following rule to your .eslintrc.js or .eslintrc.json file to disable the no-useless-catch rule:

```json
"rules": {
    "no-useless-catch": "off"
}
```

## Database Log Storage

### About

In scenarios where file-based log storage is limited, storing logs in a database can be a better alternative. For this purpose, we use winston-mongodb, which is an extension of the winston logger that allows logs to be stored in a MongoDB database.

To install the winston-mongodb package, use the following command:

```sh
npm i winston-mongodb
```

After installing the package, configure MongoDB as a transport for winston in your logger.ts file:

```ts
import 'winston-mongodb'
import { MongoDBTransportInstance } from 'winston-mongodb'

// MongoDB transport configuration
const mongodbTransport = (): Array<MongoDBTransportInstance> => {
    return [
        new transports.MongoDB({
            level: 'info',
            db: config.DATABASE_URL as string,
            metaKey: 'meta',
            // expire in 30 days
            expireAfterSeconds: 3600 * 24 * 30,
            options: {
                useUnifiedTopology: true
            },
            collection: 'application-logs'
        })
    ]
}

export default createLogger({
    defaultMeta: {
        meta: {}
    },
    transports: [...mongodbTransport()]
})
```

Make sure to replace config.DATABASE_URL with your actual MongoDB connection URL. This configuration sets up a MongoDB transport that logs all messages at the 'info' level and above, storing them in the application-logs collection. The logs will automatically expire after 30 days.

## Database Migration

### About

Database migration is the process of moving data from one database to another or changing the structure of an existing database.

```sh
npm i ts-migrate-mongoose
```

Create a migration.js file in the scripts folder with the following content:

```js
/*        eslint-disable no-console     */
const { exec } = require('child_process')

// Command line Argument
const command = process.argv[2]
const migrationName = process.argv[3]

//  Migration Commands
const validCommands = ['create', 'up', 'down', 'list', 'prune']
if (!validCommands.includes(command)) {
    console.error(`Invalid command:command must be one of ${validCommands}`)
    process.exit(0)
}

const commandWithoutMigrationNameRequired = ['list', 'prune']
if (!commandWithoutMigrationNameRequired.includes(command)) {
    if (!migrationName) {
        console.error(`migration name is required`)
        process.exit(0)
    }
}

function runNpmScript() {
    return new Promise((resolve, reject) => {
        let execCommand = ``
        if (commandWithoutMigrationNameRequired.includes(command)) {
            execCommand = `migrate ${command}`
        } else {
            execCommand = `migrate ${command} ${migrationName}`
        }

        const childProcess = exec(execCommand, (error, stdout) => {
            if (error) {
                reject(`Error runnig script: ${error}`)
            } else {
                resolve(stdout)
            }
        })
        childProcess.stderr.on('data', (data) => {
            console.error(data)
        })
    })
}

// Example Usage:
runNpmScript()
    .then((output) => {
        console.info(output)
    })
    .catch((error) => {
        console.info('Error:', error)
    })
```

Create a migration folder in the root of your repository.

Create a .env file in the root of your project with the following content:

```env
# Migration
MIGRATE_MONGO_URI = "mongodb://localhost:27017/node-js-snippet"
MIGRATE_AUTOSYNC = "true"
```

Add the migration scripts to your package.json file:

```json
"migrate:dev": "cross-env MIGRATE_MODE=development node scripts/migration.js",
"migrate:prod": "cross-env MIGRATE_MODE=production node scripts/migration.js"
```
