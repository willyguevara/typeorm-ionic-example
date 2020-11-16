# Using TypeORM in an Ionic project
You can use TypeORM in connection with the `cordova-sqlite-storage` plugin in your Ionic app.
This project demonstrates how that would work.

## Installation

To run this example in production or development mode you have to make sure, `ionic` and `cordova` are installed globally on your machine. After that you can install all necessary dependencies for running this example.

0. Check if `npm` is installed. Otherwise please [install `node.js` and `npm`](https://nodejs.org/en/download/package-manager/).
```bash
npm -v
```

1. Install ionic and cordova command line interface globally.
```bash
npm install -g cordova ionic
```

2. Install all dependencies listed in [`package.json`](/package.json).
```bash
npm install
```

### Running the example in your browser
```bash
ionic serve
```

### Running the example on your device
3. Add an iOS or Android to the project.
```bash
ionic cordova platform add ios 
# or 
ionic cordova platform add android
```

4. Run the app on your device.
```bash
ionic cordova run ios
# or
ionic cordova run android
```

*For further information please read [ionic's deployment guide](https://ionicframework.com/docs/intro/deploying/).*

![screenshot](./screenshot.png)

### Using TypeORM in your own app
1. Install the plugin
```bash
ionic cordova plugin add cordova-sqlite-storage --save
```

2. Install TypeORM
```bash
npm install typeorm --save
```

3. Install node.js-Types
```bash
npm install @types/node --save-dev
```

4. Add `"typeRoots": ["node_modules/@types"]` to your `tsconfig.json` under `compilerOptions`

5. Create a custom webpack config file like the one [included in this project](config/webpack.config.js) to use the correct TypeORM version and add the config file to your [`package.json`](package.json#L12-14) (Required with TypeORM >= 0.1.7)

### Limitations to TypeORM when using production builds

Since Ionic make a lot of optimizations while building for production, the following limitations will occur:

1. Entities have to be marked with the table name (eg `@Entity('table_name')`)

2. `getRepository()` has to be called with the name of the entity instead of the class (*eg `getRepository('post') as Repository<Post>`*)

3. Date fields are **not supported**:
```ts
@Column()
birthdate: Date;
```

### Problems building?

Error:

```bash
Installing "cordova-sqlite-storage" for android
Failed to install 'cordova-sqlite-storage': CordovaError: Using "requireCordovaModule" to load non-cordova module "q" is not supported. Instead, add this module to your dependencies and use regular "require" to load it.
    at Context.requireCordovaModule (/Users/user/.nvm/versions/node/v14.4.0/lib/node_modules/cordova/node_modules/cordova-lib/src/hooks/Context.js:57:15)
    at module.exports (/Users/user/Documents/Development/Cordova/typeorm-ionic-example/plugins/cordova-sqlite-storage/scripts/beforePluginInstall.js:13:21)
    at runScriptViaModuleLoader (/Users/user/.nvm/versions/node/v14.4.0/lib/node_modules/cordova/node_modules/cordova-lib/src/hooks/HooksRunner.js:157:32)
    at runScript (/Users/user/.nvm/versions/node/v14.4.0/lib/node_modules/cordova/node_modules/cordova-lib/src/hooks/HooksRunner.js:136:12)
    at /Users/user/.nvm/versions/node/v14.4.0/lib/node_modules/cordova/node_modules/cordova-lib/src/hooks/HooksRunner.js:108:40
    at processTicksAndRejections (internal/process/task_queues.js:97:5)
Using "requireCordovaModule" to load non-cordova module "q" is not supported. Instead, add this module to your dependencies and use regular "require" to load it.
[ERROR] An error occurred while running subprocess cordova.
        
        cordova platform add android exited with exit code 1.
        
        Re-running this command with the --verbose flag may provide more
        information.
```

try these steps:

```bash
ionic cordova platform rm ios; ionic cordova platform rm android; ionic cordova plugin rm cordova-sqlite-storage; npm i cordova-sqlite-storage@latest && ionic cordova plugin add cordova-sqlite-storage; ionic cordova platform add ios; ionic cordova platform add android
```
