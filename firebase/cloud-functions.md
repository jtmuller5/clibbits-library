# Cloud Functions Version Comparison

## Overview

There are two versions of Cloud Functions for Firebase:
* **Cloud Functions (2nd gen)**, which deploys your functions as services on Cloud Run, allowing you to trigger them using Eventarc and Pub/Sub.
* **Cloud Functions (1st gen)**, the original version of functions with limited event triggers and configurability.

We recommend that you choose Cloud Functions (2nd gen) for new functions wherever possible. However, we plan to continue supporting Cloud Functions (1st gen).

This page describes features introduced in Cloud Functions and provides a comparison between the two product versions.

## Cloud Functions (2nd gen)

Cloud Functions is Firebase's next-generation Functions-as-a-Service offering. Built on Cloud Run and Eventarc, Cloud Functions (2nd gen) brings enhanced infrastructure and broader event coverage to Cloud Functions, including:

* **Built on Cloud Run**: Functions are built with Cloud Build and deployed as Cloud Run services using the default Cloud Run execution environment. This gives you the ability to customize your function as you would a Cloud Run service. Refer to Cloud Run documentation to explore options for configuring your service, such as memory limits, environment variables, and so forth.
* **Longer request processing times**: Run longer-request workloads such as processing large streams of data from Cloud Storage or BigQuery.
* **Larger instance sizes**: Run larger in-memory, compute-intensive, and parallel workloads.
* **Improved concurrency**: Handle multiple concurrent requests with a single function instance to minimize cold starts and improve latency.
* **Traffic management**: Split traffic between different function revisions or roll a function back to a prior version.
* **Eventarc integration**: Native support for Eventarc triggers, bringing all 90+ event sources supported by Eventarc to Cloud Functions.
* **Broader CloudEvents support**: Support for industry-standard CloudEvents in all language runtimes, providing a consistent developer experience.

See the comparison table for details.

Because Cloud Functions deploys functions as services on Cloud Run, Cloud Functions shares resource quotas and limits with Cloud Run. See Quotas.

## Comparison Table

| Feature | Cloud Functions (1st gen) | Cloud Functions |
| --- | --- | --- |
| Image registry | Container Registry or Artifact Registry | Artifact Registry only |
| Request timeout | Up to 9 minutes | <ul><li>Up to 60 minutes for HTTP-triggered functions</li><li>Up to 9 minutes for event-triggered functions</li></ul> |
| Instance size | Up to 8GB RAM with 2 vCPU | Up to 16GiB RAM with 4 vCPU |
| Concurrency | 1 concurrent request per function instance | Up to 1000 concurrent requests per function instance |

## Pricing

For pricing information, see Firebase pricing plans.

If you use Cloud Functions, you can view your costs associated with only Cloud Functions as follows:

1. Go to the Cloud Billing Reports page in the Google Cloud console.
2. If prompted, select the billing account associated with your Google Cloud project.
3. In the **Filters** panel, under **Labels**, add a label filter with the key `goog-managed-by` and the value `cloudfunctions`.

**Note:** If you are using Cloud Functions for Firebase (1st gen) and are interested in Cloud Functions, see Upgrade 1st gen Node.js functions to 2nd gen.

## Limitations

Cloud Functions for Firebase (2nd gen) does not provide support for Analytics events.

Though Cloud Functions for Firebase (2nd gen) supports authentication blocking events, it does not support the same set of basic Authentication events as 1st gen.

However, because 1st gen and 2nd gen functions can coexist side-by-side in the same source file, you can still develop and deploy Analytics and basic Authentication triggers in 1st gen together with 2nd gen functions.

I'll convert this document to markdown with proper formatting and labeled code fences.

# Upgrade 1st gen Node.js functions to 2nd gen

Apps currently using 1st gen functions should consider migrating to 2nd gen using the instructions in this guide. 2nd gen functions use Cloud Run to provide better performance, better configuration, better monitoring, and more.

> Note: Cloud Functions for Firebase (2nd gen) also provides Python support. You can get started with Python using our basic tutorial's snippets and instructions.

The examples in this page assume you're using JavaScript with CommonJS modules (require style imports), but the same principles apply to JavaScript with ESM (import … from style imports) and TypeScript.

## The migration process

1st gen and 2nd gen functions can coexist side-by-side in the same file. This allows for easy migration piece by piece, as you're ready. We recommend migrating one function at a time, performing testing and verification before proceeding.

### Verify Firebase CLI and firebase-functions versions

Make sure you're using at least Firebase CLI version 12.00 and firebase-functions version 4.3.0. Any newer version will support 2nd gen as well as 1st gen.

### Update imports

2nd gen functions import from the v2 subpackage in the firebase-functions SDK. This different import path is all the Firebase CLI needs to determine whether to deploy your function code as a 1st or 2nd gen function.

The v2 subpackage is modular, and we recommend only importing the specific module that you need.

Before: 1st gen

```javascript
const functions = require("firebase-functions/v1");
```

After: 2nd gen

```javascript
// explicitly import each trigger
const {onRequest} = require("firebase-functions/v2/https");
const {onDocumentCreated} = require("firebase-functions/v2/firestore");
```

### Update trigger definitions

Since the 2nd gen SDK favors modular imports, update trigger definitions to reflect the changed imports from the previous step.

The arguments passed to callbacks for some triggers have changed. In this example, note that the arguments to the onDocumentCreated callback have been consolidated into a single event object. Additionally, some triggers have convenient new configuration features, like the onRequest trigger's cors option.

Before: 1st gen

```javascript
const functions = require("firebase-functions/v1");

exports.date = functions.https.onRequest((req, res) => {
  // ...
});

exports.uppercase = functions.firestore
  .document("my-collection/{docId}")
  .onCreate((change, context) => {
    // ...
  });
```

After: 2nd gen

```javascript
const {onRequest} = require("firebase-functions/v2/https");
const {onDocumentCreated} = require("firebase-functions/v2/firestore");

exports.date = onRequest({cors: true}, (req, res) => {
  // ...
});

exports.uppercase = onDocumentCreated("my-collection/{docId}", (event) => {
  /* ... */
});
```

## Use parameterized configuration

2nd gen functions drop support for functions.config in favor of a more secure interface for defining configuration parameters declaratively inside your codebase. With the new params module, the CLI blocks deployment unless all parameters have a valid value, ensuring that a function isn't deployed with missing configuration.

### Migrate to the params subpackage

If you have been using environment configuration with functions.config, you can migrate your existing configuration to parameterized configuration.

Before: 1st gen

```javascript
const functions = require("firebase-functions/v1");

exports.date = functions.https.onRequest((req, res) => {
  const date = new Date();
  const formattedDate = date.toLocaleDateString(functions.config().dateformat);

  // ...
});
```

After: 2nd gen

```javascript
const {onRequest} = require("firebase-functions/v2/https");
const {defineString} = require("firebase-functions/params");

const dateFormat = defineString("DATE_FORMAT");

exports.date = onRequest((req, res) => {
  const date = new Date();
  const formattedDate = date.toLocaleDateString(dateFormat.value());

  // ...
});
```

### Set parameter values

The first time you deploy, the Firebase CLI prompts for all values of parameters, and save the values in a dotenv file. To export your functions.config values, run firebase functions:config:export.

For additional safety, you can also specify parameter types and validation rules.

### Special case: API Keys

The params module integrates with Cloud Secret Manager, which provides fine-grained access control to sensitive values like API keys. See secret parameters for more information.

Before: 1st gen

```javascript
const functions = require("firebase-functions/v1");

exports.getQuote = functions.https.onRequest(async (req, res) => {
  const quote = await fetchMotivationalQuote(functions.config().apiKey);
  // ...
});
```

After: 2nd gen

```javascript
const {onRequest} = require("firebase-functions/v2/https");
const {defineSecret} = require("firebase-functions/params");

// Define the secret parameter
const apiKey = defineSecret("API_KEY");

exports.getQuote = onRequest(
  // make the secret available to this function
  { secrets: [apiKey] },
  async (req, res) => {
    // retrieve the value of the secret
    const quote = await fetchMotivationalQuote(apiKey.value());
    // ...
  }
);
```

## Set runtime options

Configuration of runtime options has changed between 1st and 2nd gen. 2nd gen also adds a new capability to set options for all functions.

Before: 1st gen

```javascript
const functions = require("firebase-functions/v1");

exports.date = functions
  .runWith({
    // Keep 5 instances warm for this latency-critical function
    minInstances: 5,
  })
  // locate function closest to users
  .region("asia-northeast1")
  .https.onRequest((req, res) => {
    // ...
  });

exports.uppercase = functions
  // locate function closest to users and database
  .region("asia-northeast1")
  .firestore.document("my-collection/{docId}")
  .onCreate((change, context) => {
    // ...
  });
```

After: 2nd gen

```javascript
const {onRequest} = require("firebase-functions/v2/https");
const {onDocumentCreated} = require("firebase-functions/v2/firestore");
const {setGlobalOptions} = require("firebase-functions/v2");

// locate all functions closest to users
setGlobalOptions({ region: "asia-northeast1" });

exports.date = onRequest({
    // Keep 5 instances warm for this latency-critical function
    minInstances: 5,
  }, (req, res) => {
  // ...
});

exports.uppercase = onDocumentCreated("my-collection/{docId}", (event) => {
  /* ... */
});
```

## Use concurrency

A significant advantage of 2nd gen functions is the ability of a single function instance to serve more than one request at once. This can dramatically reduce the number of cold starts experienced by end users. By default, concurrency is set at 80, but you can set it to any value from 1 to 1000:

```javascript
const {onRequest} = require("firebase-functions/v2/https");

exports.date = onRequest({
    // set concurrency value
    concurrency: 500
  },
  (req, res) => {
    // ...
});
```

Tuning concurrency can improve performance and reduce cost of functions. Learn more about concurrency in Allow concurrent requests.

## Audit global variable usage

1st gen functions written without concurrency in mind might use global variables that are set and read on each request. When concurrency is enabled and a single instance starts handling multiple requests at once, this may introduce bugs in your function as concurrent requests start setting and reading global variables simultaneously.

While upgrading, you can set your function's CPU to gcf_gen1 and set concurrency to 1 to restore 1st gen behavior:

```javascript
const {onRequest} = require("firebase-functions/v2/https");

exports.date = onRequest({
    // TEMPORARY FIX: remove concurrency
    cpu: "gcf_gen1",
    concurrency: 1
  },
  (req, res) => {
    // ...
});
```

However, this is not recommended as a long-term fix, as it forfeits the performance advantages of 2nd gen functions. Instead, audit usage of global variables in your functions, and remove these temporary settings when you're ready.

## Migrate traffic to the new 2nd gen functions

Just as when changing a function's region or trigger type, you'll need to give the 2nd gen function a new name and slowly migrate traffic to it.

It is not possible to upgrade a function from 1st to 2nd gen with the same name and run firebase deploy. Doing so will result in the error:

```
Upgrading from GCFv1 to GCFv2 is not yet supported. Please delete your old function or wait for this feature to be ready.
```

Before you follow these steps, first ensure that your function is idempotent, since both the new version and the old version of your function will be running at the same time during the change. For example, if you have a 1st gen function that responds to write events in Firestore, ensure that responding to a write twice, once by the 1st gen function and once by the 2nd gen function, in response to those events leaves your app in a consistent state.

1. Rename the function in your functions code. For example, rename resizeImage to resizeImageSecondGen.
2. Deploy the function, so that both the original 1st gen function and 2nd gen function are running.
3. In the case of callable, Task Queue, and HTTP triggers, begin pointing all clients to the 2nd gen function by updating client code with the 2nd gen function's name or URL.
4. With background triggers, both the 1st gen and 2nd gen functions will respond to every event immediately upon deploy.
5. When all traffic is migrated off, delete the 1st gen function using the firebase CLI's firebase functions:delete command.
6. Optionally, rename the 2nd gen function to match the name of the 1st gen function.

# Use TypeScript for Cloud Functions

For developers who prefer to write functions in TypeScript, Cloud Functions provides two types of support:

* Create and configure TypeScript projects for automatic transpilation at initialization (`firebase init functions`).
* Transpile existing TypeScript source to JavaScript at deploy time via a predeploy hook.

Following instructions in this guide, you can migrate an existing JavaScript project to TypeScript and continue deploying functions using a predeploy hook to transpile your source code. TypeScript offers many benefits over vanilla JavaScript when writing functions:

* TypeScript supports latest JavaScript features like async/await, simplifying promise management
* A Cloud Functions linter highlights common problems while you're coding
* Type safety helps you avoid runtime errors in deployed functions

If you're new to TypeScript, see TypeScript in 5 minutes.

## Initializing a new Cloud Functions project with TypeScript

Run `firebase init functions` in a new directory. The tool gives you options to build the project with JavaScript or TypeScript. Choose TypeScript to output the following project structure:

```
myproject
 +- functions/     # Directory containing all your functions code
      |
      +- package.json  # npm package file describing your Cloud Functions code
      |
      +- tsconfig.json
      |
      +- .eslintrc.js # Optional file if you enabled ESLint
      +- tsconfig.dev.json # Optional file that references .eslintrc.js
      |
      +- src/     # Directory containing TypeScript source
      |   |
      |   +- index.ts  # main source file for your Cloud Functions code
      |
      +- lib/
          |
          +- index.js  # Built/transpiled JavaScript code
          |
          +- index.js.map # Source map for debugging
```

Once initialization is complete, uncomment the sample in `index.ts` and run `npm run serve` to see a "Hello World" function in action.

## Using an existing TypeScript project

If you have an existing TypeScript project, you can add a predeploy hook to make sure your project is transpiled every time you deploy your code to Cloud Functions for Firebase. You'll need a properly formed `tsconfig.json` file and a Firebase project, and you'll need to make the following modifications to your Firebase configuration:

Edit `package.json` to add a bash script to build your TypeScript project. For example:

```json
{
  "name": "functions",
  "scripts": {
    "build": "npm run lint && tsc"
  }
...
```

Edit `firebase.json` to add a predeploy hook to run the build script. For example:

```json
{
  "functions": {
    "predeploy": "npm --prefix functions run build",
  }
}
```

With this configuration, a `firebase deploy --only functions` command builds your TypeScript code and deploys it as functions.

## Migrating an existing JavaScript project to TypeScript

If you have an existing Cloud Functions project that you initialized and developed in JavaScript, you can migrate it to TypeScript. You're strongly encouraged to create a git checkpoint or other backup before starting.

To migrate an existing JavaScript Cloud Functions project:

1. Create a git checkpoint and save copies of your existing JavaScript source files.
2. In the project directory, run `firebase init functions` and select TypeScript when prompted for a language for writing functions.
3. When prompted whether to overwrite the existing `package.json` file, select No unless you are sure you don't want to keep the existing file.
4. Delete `index.ts` in the directory `functions/src`, replacing it with your existing source code.
5. In the `tsconfig.json` file created at initialization, set the compiler options to allow JavaScript: `"allowJs": true`.
6. Copy your saved `package.json` file into the functions directory, and edit it to set "main" to "lib/index.js".
7. Also in `package.json`, add a build script for TypeScript like the following:

```json
{
  "name": "functions",
  "scripts": {
    "build": "npm run lint && tsc"
  }
...
```

8. Add "typescript" as a dev dependency by running `npm install --save-dev typescript @typescript-eslint/eslint-plugin @typescript-eslint/parser`.
9. For all dependencies, run `npm install --save @types/<dependency>`.
10. Rewrite source code from `.js` to `.ts` as desired.

## Emulating TypeScript functions

To test TypeScript functions locally, you can use the emulation tools described in Run functions locally. It's important to compile your code before using these tools, so make sure to run `npm run build` inside your functions directory before running `firebase emulators:start` or `firebase functions:shell`. Alternatively, run `npm run serve` or `npm run shell` as a shortcut; these commands both run the build and serve/start the functions shell.

## Functions logs for TypeScript projects

During `firebase deploy`, your project's `index.ts` is transpiled to `index.js`, meaning that the Cloud Functions log will output line numbers from the `index.js` file and not the code you wrote. To make it easier for you to find the corresponding paths and line numbers in `index.ts`, `firebase deploy` creates `functions/lib/index.js.map`. You can use this source map in your preferred IDE or via a node module.

# Configure your Environment

## Overview

Often you'll need additional configuration for your functions, such as third-party API keys or tuneable settings. The Firebase SDK for Cloud Functions offers built-in environment configuration to make it easy to store and retrieve this type of data for your project.

You can choose between these options:

* **Parameterized configuration** (recommended for most scenarios). This provides strongly-typed environment configuration with parameters that are validated at deploy time, which prevents errors and simplifies debugging.
* **File-based configuration of environment variables**. With this approach, you manually create a dotenv file for loading environment variables.

For most use cases, parameterized configuration is recommended. This approach makes configuration values available both at runtime and deploy time, and deployment is blocked unless all parameters have a valid value. Conversely, configuration with environment variables is not available at deploy time.

> Note: Environment configuration with functions.config was deprecated in version 6.0.0, and will no longer be supported in the next major release. If you are using functions.config, you are strongly recommended to migrate to environment variables.

## Parameterized Configuration

Cloud Functions for Firebase provides an interface for defining configuration parameters declaratively inside your codebase. The value of these parameters is available both during function deployment, when setting deployment and runtime options, and during execution. This means that the CLI will block deployment unless all parameters have a valid value.

### Node.js

```javascript
const { onRequest } = require('firebase-functions/v2/https');
const { defineInt, defineString } = require('firebase-functions/params');

// Define some parameters
const minInstancesConfig = defineInt('HELLO_WORLD_MININSTANCES');
const welcomeMessage = defineString('WELCOME_MESSAGE');

// To use configured parameters inside the config for a function, provide them
// directly. To use them at runtime, call .value() on them.
export const helloWorld = onRequest(
  { minInstances: minInstancesConfig },
(req, res) => {
    res.send(`${welcomeMessage.value()}! I am a function.`);
  }
);
```

When deploying a function with parameterized configuration variables, the Firebase CLI first attempts to load their values from local .env files. If they are not present in those files and no default is set, the CLI will prompt for the values during deployment, and then automatically save their values to a .env file named .env.<project_ID> in your functions/ directory:

```
$ firebase deploy
i  functions: preparing codebase default for deployment
? Enter a string value for ENVIRONMENT: prod
i  functions: Writing new parameter values to disk: .env.projectId
…
$ firebase deploy
i  functions: Loaded environment variables from .env.projectId
```

Depending on your development workflow, it may be useful to add the generated .env.<project_ID> file to version control.

### Using Parameters in Global Scope

During deployment, your functions code is loaded and inspected before your parameters have actual values. This means that fetching parameter values during global scope results in deployment failure. For cases where you want to use a parameter to initialize a global value, use the initialization callback `onInit()`. This callback runs before any functions run in production but is not be called during deploy time, so it is a safe place to access a parameter's value.

```javascript
const { GoogleGenerativeAI } = require('@google/generative-ai');
const { defineSecret } = require('firebase-functions/params');
const { onInit } = require('firebase-functions/v2/core');

const apiKey = defineSecret('GOOGLE_API_KEY');

let genAI;
onInit(() => {
  genAI = new GoogleGenerativeAI(apiKey.value());
})
```

If you use parameters of the type Secret, note that they are available only in the process of functions that have bound the secret. If a secret is bound only in some functions, check whether secret.value() is falsy before using it.

### Configure CLI Behavior

Parameters can be configured with an Options object that controls how the CLI will prompt for values. The following example sets options to validate the format of a phone number, to provide a simple selection option, and to populate a selection option automatically from the Firebase project:

```javascript
const { defineString } = require('firebase-functions/params');

const welcomeMessage = defineString('WELCOME_MESSAGE', {
  default: 'Hello World',
  description: 'The greeting that is returned to the caller of this function'
});

const onlyPhoneNumbers = defineString('PHONE_NUMBER', {
  input: {
    text: {
      validationRegex: /\d{3}-\d{3}-\d{4}/,
      validationErrorMessage: "Please enter a phone number in the format XXX-YYY-ZZZZ"
    },
  },
});

const selectedOption = defineString('PARITY', {
  input: params.select(["odd", "even"])
});

const memory = defineInt("MEMORY", {
  description: "How much memory do you need?",
  input: params.select({ "micro": 256, "chonky": 2048 }),
});

const extensions = defineList("EXTENSIONS", {
  description: "Which file types should be processed?",
  input: params.multiSelect(["jpg", "tiff", "png", "webp"]),
});

const storageBucket = defineString('BUCKET', {
  description: "This will automatically populate the selector field with the deploying Cloud Project's storage buckets",
  input: params.PICK_STORAGE_BUCKET,
});
```

### Parameter Types

Parameterized configuration provides strong typing for parameter values, and also support secrets from Cloud Secret Manager. Supported types are:

* Secret
* String
* Boolean
* Integer
* Float
* List (Node.js)

### Parameter Values and Expressions

Firebase evaluates your parameters both at deploy time and while your function is executing. Due to these dual environments, some extra care must be taken when comparing parameter values, and when using them to set runtime options for your functions.

To pass a parameter to your function as a runtime option, pass it directly:

```javascript
const { onRequest } = require('firebase-functions/v2/https');
const { defineInt } = require('firebase-functions/params');
const minInstancesConfig = defineInt('HELLO_WORLD_MININSTANCES');

export const helloWorld = onRequest(
  { minInstances: minInstancesConfig },
  (req, res) => {
    //…
```

Additionally, if you need to compare against a parameter in order to know what option to pick, you'll need to use built-in comparators instead of checking the value:

```javascript
const { onRequest } = require('firebase-functions/v2/https');
const environment = params.defineString('ENVIRONMENT', {default: 'dev'});

// use built-in comparators
const minInstancesConfig = environment.equals('PRODUCTION').thenElse(10, 1);
export const helloWorld = onRequest(
  { minInstances: minInstancesConfig },
  (req, res) => {
    //…
```

Parameters and parameter expressions that are only used at runtime can be accessed with their value function:

```javascript
const { onRequest } = require('firebase-functions/v2/https');
const { defineString } = require('firebase-functions/params');
const welcomeMessage = defineString('WELCOME_MESSAGE');

// To use configured parameters inside the config for a function, provide them
// directly. To use them at runtime, call .value() on them.
export const helloWorld = onRequest(
(req, res) => {
    res.send(`${welcomeMessage.value()}! I am a function.`);
  }
);
```

### Built-in Parameters

The Cloud Functions SDK offers three pre-defined parameters, available from the firebase-functions/params subpackage:

* **projectID** — the Cloud project in which the function is running.
* **databaseURL** — the URL of the Realtime Database instance associated with the function (if enabled on the Firebase project).
* **storageBucket** — the Cloud Storage bucket associated with the function (if enabled on the Firebase project).

These function like user-defined string parameters in all respects, except that, since their values are always known to the Firebase CLI, their values will never be prompted for on deployment nor saved to .env files.

### Secret Parameters

Parameters of type Secret, defined using defineSecret(), represent string parameters which have a value stored in Cloud Secret Manager. Instead of checking against a local .env file and writing a new value to the file if missing, secret parameters check against existence in Cloud Secret Manager, and interactively prompt for the value of a new secret during deployment.

Secret parameters defined in this way must be bound to individual functions that should have access to them:

```javascript
const { onRequest } = require('firebase-functions/v2/https');
const { defineSecret } = require('firebase-functions/params');
const discordApiKey = defineSecret('DISCORD_API_KEY');

export const postToDiscord = onRequest(
  { secrets: [discordApiKey] },
  (req, res) => {
  const apiKey = discordApiKey.value();
    //…
```

Because the values of secrets are hidden until execution of the function, you cannot use them while configuring your function.

## Environment Variables

Cloud Functions for Firebase supports the dotenv file format for loading environment variables specified in a .env file to your application runtime. Once deployed, the environment variables can be read via the process.env interface (in Node.js-based projects) or os.environ (in Python-based projects).

To configure your environment this way, create a .env file in your project, add the desired variables, and deploy:

1. Create a .env file in your functions/ directory:

```
# Directory layout:
#   my-project/
#     firebase.json
#     functions/
#       .env
#       package.json
#       index.js
```

2. Open the .env file for edit, and add the desired keys. For example:

```
PLANET=Earth
AUDIENCE=Humans
```

3. Deploy functions and verify that environment variables were loaded:

```
firebase deploy --only functions
# ...
# i functions: Loaded environment variables from .env.
# ...
```

Once your custom environment variables are deployed, your function code can access them:

```javascript
// Responds with "Hello Earth and Humans"
exports.hello = onRequest((request, response) => {
  response.send(`Hello ${process.env.PLANET} and ${process.env.AUDIENCE}`);
});
```

### Deploying Multiple Sets of Environment Variables

If you need an alternative set of environment variables for your Firebase projects (such as staging vs production), create a .env.<project or alias> file and write your project-specific environment variables there. The environment variables from .env and project-specific .env files (if they exist) will be included in all deployed functions.

For example, a project could include these three files containing slightly different values for development and production:

| .env | .env.dev | .env.prod |
|------|----------|-----------|
| PLANET=Earth<br>AUDIENCE=Humans | AUDIENCE=Dev Humans | AUDIENCE=Prod Humans |

Given the values in those separate files, the set of environment variables deployed with your functions will vary depending on your target project:

```
$ firebase use dev
$ firebase deploy --only functions
i functions: Loaded environment variables from .env, .env.dev.
# Deploys functions with following user-defined environment variables:
#   PLANET=Earth
#   AUDIENCE=Dev Humans

$ firebase use prod
$ firebase deploy --only functions
i functions: Loaded environment variables from .env, .env.prod.
# Deploys functions with following user-defined environment variables:
#   PLANET=Earth
#   AUDIENCE=Prod Humans
```

### Reserved Environment Variables

Some environment variable keys are reserved for internal use. Do not use any of these keys in your .env files:

* All keys starting with X_GOOGLE_
* All keys starting EXT_
* All keys starting with FIREBASE_
* Any key from the following list:
  * CLOUD_RUNTIME_CONFIG
  * ENTRY_POINT
  * GCP_PROJECT
  * GCLOUD_PROJECT
  * GOOGLE_CLOUD_PROJECT
  * FUNCTION_TRIGGER_TYPE
  * FUNCTION_NAME
  * FUNCTION_MEMORY_MB
  * FUNCTION_TIMEOUT_SEC
  * FUNCTION_IDENTITY
  * FUNCTION_REGION
  * FUNCTION_TARGET
  * FUNCTION_SIGNATURE_TYPE
  * K_SERVICE
  * K_REVISION
  * PORT
  * K_CONFIGURATION

## Store and Access Sensitive Configuration Information

Environment variables stored in .env files can be used for function configuration, but you should not consider them a secure way to store sensitive information such as database credentials or API keys. This is especially important if you check your .env files into source control.

To help you store sensitive configuration information, Cloud Functions for Firebase integrates with Google Cloud Secret Manager. This encrypted service stores configuration values securely, while still allowing easy access from your functions when needed.

> Note: Secret Manager is a paid service, with a free tier. See How secrets are billed for more information.

### Create and Use a Secret

To create a secret, use the Firebase CLI.

> Note: Make sure not to use any reserved environment variable names for your secrets.

To create and use a secret:

1. From the root of your local project directory, run the following command:

```
firebase functions:secrets:set SECRET_NAME
```

2. Enter a value for SECRET_NAME.

The CLI echoes a success message and warns that you must deploy functions for the change to take effect.

3. Before deploying, make sure your functions code allows the function to access the secret using the runWith parameter:

```javascript
const { onRequest } = require('firebase-functions/v2/https');

exports.processPayment = onRequest(
  { secrets: ["SECRET_NAME"] },
  (req, res) => {
    const myBillingService = initializeBillingService(
      // reference the secret value
      process.env.SECRET_NAME
    );
    // Process the payment
  }
);
```

4. Deploy Cloud Functions:

```
firebase deploy --only functions
```

Now you'll be able to access it like any other environment variable. Conversely, if another function that does not specify the secret in runWith tries to access the secret, it receives an undefined value:

```javascript
exports.anotherEndpoint = onRequest((request, response) => {
  response.send(`The secret API key is ${process.env.SECRET_NAME}`);
  // responds with "The secret API key is undefined" because the `runWith` parameter is missing
});
```

Once your function is deployed, it will have access to the secret value. Only functions that specifically include a secret in their runWith parameter will have access to that secret as an environment variable. This helps you make sure that secret values are only available where they're needed, reducing the risk of accidentally leaking a secret.

### Managing Secrets

Use the Firebase CLI to manage your secrets. While managing secrets this way, keep in mind that some CLI changes require you to modify and/or redeploy associated functions. Specifically:

* Whenever you set a new value for a secret, you must redeploy all functions that reference that secret for them to pick up the latest value.
* If you delete a secret, make sure that none of your deployed functions references that secret. Functions that use a secret value that has been deleted will fail silently.

Here's a summary of the Firebase CLI commands for secret management:

```
# Change the value of an existing secret
firebase functions:secrets:set SECRET_NAME

# View the value of a secret
functions:secrets:access SECRET_NAME

# Destroy a secret
functions:secrets:destroy SECRET_NAME

# View all secret versions and their state
functions:secrets:get SECRET_NAME

# Automatically clean up all secrets that aren't referenced by any of your functions
functions:secrets:prune
```

For the access and destroy commands, you can provide the optional version parameter to manage a particular version. For example:

```
functions:secrets:access SECRET_NAME[@VERSION]
```

For more information about these operations, pass -h with the command to view CLI help.

### How Secrets Are Billed

Secret Manager allows 6 active secret versions at no cost. This means that you can have 6 secrets per month in a Firebase project at no cost.

By default, the Firebase CLI attempts to automatically destroy unused secret versions where appropriate, such as when you deploy functions with a new version of the secret. Also, you can actively clean up unused secrets using functions:secrets:destroy and functions:secrets:prune.

Secret Manager allows 10,000 unbilled monthly access operations on a secret. Function instances read only the secrets specified in their runWith parameter every time they cold start. If you have a lot of function instances reading a lot of secrets, your project may exceed this allowance, at which point you'll be charged $0.03 per 10,000 access operations.

For more information, see Secret Manager Pricing.

## Emulator Support

Environment configuration with dotenv is designed to interoperate with a local Cloud Functions emulator.

When using a local Cloud Functions emulator, you can override environment variables for your project by setting up a .env.local file. Contents of .env.local take precedence over .env and the project-specific .env file.

For example, a project could include these three files containing slightly different values for development and local testing:

| .env | .env.dev | .env.local |
|------|----------|------------|
| PLANET=Earth<br>AUDIENCE=Humans | AUDIENCE=Dev Humans | AUDIENCE=Local Humans |

When started in the local context, the emulator loads the environment variables as shown:

```
$ firebase emulators:start
i  emulators: Starting emulators: functions
# Starts emulator with following environment variables:
#  PLANET=Earth
#  AUDIENCE=Local Humans
```

### Secrets and Credentials in the Cloud Functions Emulator

The Cloud Functions emulator supports the use of secrets to store and access sensitive configuration information. By default, the emulator will try to access your production secrets using application default credentials. In certain situations like CI environments, the emulator may fail to access secret values due to permission restrictions.

Similar to Cloud Functions emulator support for environment variables, you can override secrets values by setting up a .secret.local file. This makes it easy for you to test your functions locally, especially if you don't have access to the secret value.

## Migrating from Environment Configuration

If you have been using environment configuration with functions.config, you can migrate your existing configuration as environment variables (in dotenv format). The Firebase CLI provides an export command that outputs the configuration of each alias or project listed in your directory's .firebaserc file (in the example below, local, dev, and prod) as .env files.

To migrate, export your existing environment configurations using the firebase functions:config:export command:

```
firebase functions:config:export
i  Importing configs from projects: [project-0, project-1]
⚠  The following configs keys could not be exported as environment variables:

⚠  project-0 (dev):
    1foo.a => 1FOO_A (Key 1FOO_A must start with an uppercase ASCII letter or underscore, and then consist of uppercase ASCII letters, digits, and underscores.)

Enter a PREFIX to rename invalid environment variable keys: CONFIG_
✔  Wrote functions/.env.prod
✔  Wrote functions/.env.dev
✔  Wrote functions/.env.local
✔  Wrote functions/.env
```

Note that, in some cases, you will be prompted to enter a prefix to rename exported environment variable keys. This is because not all configurations can be automatically transformed since they may be invalid or may be a reserved environment variable key.

We recommend that you carefully review the contents of the generated .env files before you deploy your functions or check the .env files into source control. If any values are sensitive and should not be leaked, remove them from your .env files and store them securely in Secret Manager instead.

You'll also need to update your functions code. Any functions that use functions.config will now need to use process.env instead, as shown in Environment variables.

# Cloud Firestore Triggers

## Overview

With Cloud Functions, you can handle events in Cloud Firestore with no need to update client code. You can make Cloud Firestore changes via the document snapshot interface or via the Admin SDK.

In a typical lifecycle, a Cloud Firestore function does the following:

- Waits for changes to a particular document
- Triggers when an event occurs and performs its tasks
- Receives a data object that contains a snapshot of the data stored in the specified document. For write or update events, the data object contains two snapshots that represent the data state before and after the triggering event

Distance between the location of the Firestore instance and the location of the function can create significant network latency. To optimize performance, consider specifying the function location where applicable.

## Cloud Firestore Function Triggers

The Cloud Functions for Firebase SDK exports the following Cloud Firestore event triggers to let you create handlers tied to specific Cloud Firestore events:

| Event Type | Trigger |
|------------|---------|
| onDocumentCreated | Triggered when a document is written to for the first time. |
| onDocumentUpdated | Triggered when a document already exists and has any value changed. |
| onDocumentDeleted | Triggered when a document is deleted. |
| onDocumentWritten | Triggered when onDocumentCreated, onDocumentUpdated or onDocumentDeleted is triggered. |
| onDocumentCreatedWithAuthContext | onDocumentCreated with additional authentication information |
| onDocumentWrittenWithAuthContext | onDocumentWritten with additional authentication information |
| onDocumentDeletedWithAuthContext | onDocumentDeleted with additional authentication information |
| onDocumentUpdatedWithAuthContext | onDocumentUpdated with additional authentication information |

Cloud Firestore events trigger only on document changes. An update to a Cloud Firestore document where data is unchanged (a no-op write) does not generate an update or write event. It is not possible to add events to specific fields.

If you don't have a project enabled for Cloud Functions for Firebase yet, then read Get started with Cloud Functions for Firebase (2nd gen) to configure and set up your Cloud Functions for Firebase project.

## Writing Cloud Firestore-triggered Functions

### Define a Function Trigger

To define a Cloud Firestore trigger, specify a document path and an event type:

```javascript
import {
  onDocumentWritten,
  onDocumentCreated,
  onDocumentUpdated,
  onDocumentDeleted,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.myfunction = onDocumentWritten("my-collection/{docId}", (event) => {
   /* ... */ 
});
```

Document paths can reference either a specific document or a wildcard pattern.

### Specify a Single Document

If you want to trigger an event for any change to a specific document then you can use the following function:

```javascript
import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.myfunction = onDocumentWritten("users/marie", (event) => {
  // Your code here
});
```

### Specify a Group of Documents Using Wildcards

If you want to attach a trigger to a group of documents, such as any document in a certain collection, then use a {wildcard} in place of the document ID:

```javascript
import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.myfunction = onDocumentWritten("users/{userId}", (event) => {
  // If we set `/users/marie` to {name: "Marie"} then
  // event.params.userId == "marie"
  // ... and ...
  // event.data.after.data() == {name: "Marie"}
});
```

In this example, when any field on any document in users is changed, it matches a wildcard called userId.

If a document in users has subcollections and a field in one of those subcollections' documents is changed, the userId wildcard is not triggered.

Wildcard matches are extracted from the document path and stored into event.params. You may define as many wildcards as you like to substitute explicit collection or document IDs, for example:

```javascript
import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.myfunction = onDocumentWritten("users/{userId}/{messageCollectionId}/{messageId}", (event) => {
    // If we set `/users/marie/incoming_messages/134` to {body: "Hello"} then
    // event.params.userId == "marie";
    // event.params.messageCollectionId == "incoming_messages";
    // event.params.messageId == "134";
    // ... and ...
    // event.data.after.data() == {body: "Hello"}
});
```

Your trigger must always point to a document, even if you're using a wildcard. For example, users/{userId}/{messageCollectionId} is not valid because {messageCollectionId} is a collection. However, users/{userId}/{messageCollectionId}/{messageId} is valid because {messageId} will always point to a document.

## Event Triggers

### Trigger a Function When a New Document is Created

You can trigger a function to fire any time a new document is created in a collection. This example function triggers every time a new user profile is added:

```javascript
import {
  onDocumentCreated,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.createuser = onDocumentCreated("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const snapshot = event.data;
    if (!snapshot) {
        console.log("No data associated with the event");
        return;
    }
    const data = snapshot.data();

    // access a particular field as you would any JS property
    const name = data.name;

    // perform more operations ...
});
```

For additional authentication information, use onDocumentCreatedWithAuthContext.

### Trigger a Function When a Document is Updated

You can also trigger a function to fire when a document is updated. This example function fires if a user changes their profile:

```javascript
import {
  onDocumentUpdated,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.updateuser = onDocumentUpdated("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const newValue = event.data.after.data();

    // access a particular field as you would any JS property
    const name = newValue.name;

    // perform more operations ...
});
```

For additional authentication information, use onDocumentUpdatedWithAuthContext.

### Trigger a Function When a Document is Deleted

You can also trigger a function when a document is deleted. This example function fires when a user deletes their user profile:

```javascript
import {
  onDocumentDeleted,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.deleteuser = onDocumentDeleted("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const snap =  event.data;
    const data =  snap.data();

    // perform more operations ...
});
```

For additional authentication information, use onDocumentDeletedWithAuthContext.

### Trigger a Function for All Changes to a Document

If you don't care about the type of event being fired, you can listen for all changes in a Cloud Firestore document using the "document written" event trigger. This example function fires if a user is created, updated, or deleted:

```javascript
import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.modifyuser = onDocumentWritten("users/{userId}", (event) => {
    // Get an object with the current document values.
    // If the document does not exist, it was deleted
    const document =  event.data.after.data();

    // Get an object with the previous document values
    const previousValues =  event.data.before.data();

    // perform more operations ...
});
```

For additional authentication information, use onDocumentWrittenWithAuthContext.

## Reading and Writing Data

When a function is triggered, it provides a snapshot of the data related to the event. You can use this snapshot to read from or write to the document that triggered the event, or use the Firebase Admin SDK to access other parts of your database.

### Event Data

#### Reading Data

When a function is triggered, you might want to get data from a document that was updated, or get the data prior to update. You can get the prior data by using event.data.before, which contains the document snapshot before the update. Similarly, event.data.after contains the document snapshot state after the update.

```javascript
exports.updateuser2 = onDocumentUpdated("users/{userId}", (event) => {
    // Get an object with the current document values.
    // If the document does not exist, it was deleted
    const newValues =  event.data.after.data();

    // Get an object with the previous document values
    const previousValues =  event.data.before.data();
});
```

You can access properties as you would in any other object. Alternatively, you can use the get function to access specific fields:

```javascript
// Fetch data using standard accessors
const age = event.data.after.data().age;
const name = event.data.after.data()['name'];

// Fetch data using built in accessor
const experience = event.data.after.data.get('experience');
```

#### Writing Data

Each function invocation is associated with a specific document in your Cloud Firestore database. You can access that document in the snapshot returned to your function.

The document reference includes methods like update(), set(), and remove() so you can modify the document that triggered the function.

```javascript
import { onDocumentUpdated } from "firebase-functions/v2/firestore";

exports.countnamechanges = onDocumentUpdated('users/{userId}', (event) => {
  // Retrieve the current and previous value
  const data = event.data.after.data();
  const previousData = event.data.before.data();

  // We'll only update if the name has changed.
  // This is crucial to prevent infinite loops.
  if (data.name == previousData.name) {
    return null;
  }

  // Retrieve the current count of name changes
  let count = data.name_change_count;
  if (!count) {
    count = 0;
  }

  // Then return a promise of a set operation to update the count
  return data.after.ref.set({
    name_change_count: count + 1
  }, {merge: true});
});
```

> **Warning**: Any time you write to the same document that triggered a function, you are at risk of creating an infinite loop. Use caution and ensure that you safely exit the function when no change is needed.

### Access User Authentication Information

If you use one of the of the following event types, you can access user authentication information about the principal that triggered the event. This information is in addition to the information returned in the base event.

- onDocumentCreatedWithAuthContext
- onDocumentWrittenWithAuthContext
- onDocumentDeletedWithAuthContext
- onDocumentUpdatedWithAuthContext

For information about the data available in the authentication context, see Auth Context. The following example demonstrates how to retrieve authentication information:

```javascript
import { onDocumentWrittenWithAuthContext } from "firebase-functions/v2/firestore"

exports.syncUser = onDocumentWrittenWithAuthContext("users/{userId}", (event) => {
    const snapshot = event.data.after;
    if (!snapshot) {
        console.log("No data associated with the event");
        return;
    }
    const data = snapshot.data();

    // retrieve auth context from event
    const { authType, authId } = event;

    let verified = false;
    if (authType === "system") {
      // system-generated users are automatically verified
      verified = true;
    } else if (authType === "unknown" || authType === "unauthenticated") {
      // admin users from a specific domain are verified
      if (authId.endsWith("@example.com")) {
        verified = true;
      }
    }

    return data.after.ref.set({
        created_by: authId,
        verified,
    }, {merge: true}); 
}); 
```

### Data Outside the Trigger Event

Cloud Functions execute in a trusted environment. They are authorized as a service account on your project, and you can perform reads and writes using the Firebase Admin SDK:

```javascript
const { initializeApp } = require('firebase-admin/app');
const { getFirestore, Timestamp, FieldValue } = require('firebase-admin/firestore');

initializeApp();
const db = getFirestore();

exports.writetofirestore = onDocumentWritten("some/doc", (event) => {
    db.doc('some/otherdoc').set({ ... });
});

exports.writetofirestore = onDocumentWritten('users/{userId}', (event) => {
  db.doc('some/otherdoc').set({
    // Update otherdoc
  });
});
```

> **Note**: Reads and writes performed in Cloud Functions are not controlled by your security rules, they can access any part of your database.

## Limitations

Note the following limitations for Cloud Firestore triggers for Cloud Functions:

- Cloud Functions (1st gen) prerequisites an existing "(default)" database in Firestore native mode. It does not support Cloud Firestore named databases or Datastore mode. Please use Cloud Functions (2nd gen) to configure events in such cases.
- Ordering is not guaranteed. Rapid changes can trigger function invocations in an unexpected order.
- Events are delivered at least once, but a single event may result in multiple function invocations. Avoid depending on exactly-once mechanics, and write idempotent functions.
- Cloud Firestore in Datastore mode requires Cloud Functions (2nd gen). Cloud Functions (1st gen) does not support Datastore mode.
- A trigger is associated with a single database. You cannot create a trigger that matches multiple databases.
- Deleting a database does not automatically delete any triggers for that database. The trigger stops delivering events but continues to exist until you delete the trigger.
- If a matched event exceeds the maximum request size, the event might not be delivered to Cloud Functions (1st gen).
- Events not delivered because of request size are logged in platform logs and count towards the log usage for the project.

You can find these logs in the Logs Explorer with the message "Event cannot deliver to Cloud function due to size exceeding the limit for 1st gen..." of error severity. You can find the function name under the functionName field. If the receiveTimestamp field is still within an hour from now, you can infer the actual event content by reading the document in question with a snapshot before and after the timestamp.

To avoid such cadence, you can:
- Migrate and upgrade to Cloud Functions (2nd gen)
- Downsize the document
- Delete the Cloud Functions in question

You can turn off the logging itself using exclusions but note that the offending events will still not be delivered.