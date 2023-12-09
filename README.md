
# NPM Installable Node Module Package

Creating a simple npm installable Node module package with a "Hello, World!" example.
 

##   Install from Local Source and Build

If you want to test your module locally before publishing or after making changes:

1. In your consumer app's root directory, link your module using a relative path:

   ```bash
   npm link ../path/to/my-node-module
   ```

2. Whenever you make changes to your module, rebuild it:

   ```bash
   npm run build
   ```

   If a build step is required.

3. In your consumer app, install the locally linked module:

   ```bash
   npm install my-node-module
   ```

   Now, you can use the locally linked module in your consumer app as usual.

Remember to rebuild your module after making changes (`npm run build` in the module project). After rebuilding, the changes will be reflected in the consumer app upon the next rebuild or refresh.


# Advanced : Recommended file structure
If you want to include additional folders with other JavaScript files in your npm installable Node module package, you can follow a common convention for organizing your project. Here's a suggested file structure:

```
my-node-module/
|-- lib/
|   |-- index.js        # Entry point for the module
|   |-- utils/
|       |-- helper.js   # Additional utility file
|-- src/
|   |-- index.js        # Source code for the module
|   |-- utils/
|       |-- helper.js   # Additional utility source file
|-- test/
|   |-- test.js         # Test file
|-- .gitignore          # Git ignore file
|-- package.json        # Package configuration
|-- README.md           # Project documentation
```

Explanation:

- **`lib/`**: Created on build. This folder contains the compiled code that will be distributed when users install your package. It includes the main entry point (`index.js`) and any additional files or subdirectories.

- **`src/`**: This folder contains the source code of your module. It includes the main entry point (`index.js`) and any additional files or subdirectories.

- **`test/`**: This folder contains test files for your module. It's a good practice to include tests to ensure the correctness of your module.

- **`.gitignore`**: This file specifies files and directories that should be ignored by version control (Git). It typically includes `node_modules/` and files generated during the build process.

- **`package.json`**: The package configuration file includes information about your module, dependencies, scripts, and other metadata.

- **`README.md`**: Project documentation that provides information on how to use your module.

Adjust the structure based on your specific needs, but organizing your code in a clear and standardized manner can make it easier for others to understand and use your module. Make sure to update the `"main"` entry in your `package.json` to point to the correct entry point (e.g., `"main": "lib/index.js"`).

Remember to update your build process and scripts accordingly, especially if you are using tools like Babel for transpilation or bundlers like Rollup. The compiled code in the `lib/` directory should be what gets distributed when users install your package via npm.

Certainly! Below is an example code snippet for each file in the suggested file structure:
 

**1. `src/index.js`:**
```javascript
// src/index.js
const helloWorld = require('./utils/helper');

module.exports = helloWorld;
```

**2. `src/utils/helper.js`:**
```javascript
// src/utils/helper.js
function helper() {
  return 'Helper function';
}

module.exports = helper;
```

**3. `test/test.js`:**
```javascript
// test/test.js
const helloWorld = require('../lib');

console.log(helloWorld()); // Output: Helper function
```

**4. `.gitignore`:**
```
node_modules/
lib/
```

**5. `package.json`:**
```json
{
  "name": "my-node-module",
  "version": "1.0.0",
  "description": "A simple npm installable Node module package",
  "main": "lib/index.js",
  "scripts": {
    "test": "node test/test.js"
  },
  "author": "Your Name",
  "license": "MIT",
  "devDependencies": {
    "mocha": "^10.0.1"
  }
} 
```

# Making it buildable

I apologize for the repetition in the example. The duplication is there to illustrate the source (`src/`) and distribution (`lib/`) directories. During development, you typically work in the `src/` directory, and the built or transpiled code goes into the `lib/` directory. When you publish your module to npm, it's the code in the `lib/` directory that gets distributed.

Here's how you might set up a build process using a tool like Babel:

**1. Install Babel:**
```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

**2. Create a Babel configuration file:**
Create a file named `.babelrc` in the root of your project with the following content:
```json
{
  "presets": ["@babel/preset-env"]
}
```

**3. Modify your `package.json` scripts:**
Update your `package.json` file to include build scripts:
```json
{
  "scripts": {
    "test": "node test/test.js",
    "build": "babel src --out-dir lib",
    "prepublishOnly": "npm run build"
  }
}
```

Now, when you run `npm run build`, it will use Babel to transpile the code from the `src/` directory to the `lib/` directory.

**File structure after build:**
```
my-node-module/
|-- lib/
|   |-- index.js        # Transpiled entry point
|   |-- utils/
|       |-- helper.js   # Transpiled utility file
|-- src/
|   |-- index.js        # Source entry point
|   |-- utils/
|       |-- helper.js   # Source utility file
...
```

During development, you modify the code in the `src/` directory. After making changes, you run `npm run build` to transpile the code using Babel, and the resulting code goes into the `lib/` directory.

This way, you keep your development code separate from the distribution-ready code, and users who install your package get the compiled code in the `lib/` directory.