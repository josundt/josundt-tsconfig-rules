# @josundt/tsconfig-rules

CHANGES:
- v5.2.x:   Change lib from ES2019 to ES2022, target from ES2019 to ES2021 for both Web and Node
- V4.7.x:   Use module/moduleResolution NodeNext for tsconfig.base.json
- V4.7.x:   target ES2019 for web projects
- V4.5.x:   Removed ES5 configs
- V4.5.x:   `"esModuleInterop": true` is set to default for better ES Module interoperability.  
            ___NB!__ node_modules namespace import syntax must be changed from..._
    ```javascript
    import * as foo from "my-package";
    ```
    ...to...
    ```javascript
    import foo from "my-package";
    ```

             
- v4.3.x:   Added `"noImplicitOverride": true` (to all rules).  
            When extending a super class and overriding methods or fields, the new TS 4.3 `override` keyword is required.

- v4.2.x:   Added `"noPropertyAccessFromIndexSignature": true` (to all rules).  
            Index signature objects like `Record<string, T>` now need to use square bracket notation:  
            `myRecord["prop"]`  
            ...instead of dot notation:  
            `myRecord.prop`

- v3.8.x:   Compile everything to ES2017 (for Node ES2019)

Contains the standard base compiler options/rules to be used in all TypeScript source code in different projects.

Add this package as a dependency of your project, and ensure that your project's *tsconfig.json* file extends the appropriate
file based on your project type:

*PS! These compiler settings require that you have added the **"tslib"** package added as a development depdency of your project.*

## Web projects

### A - For webpack projects
For web projects with Webpack:  
In the top of your project's tsconfig.json file, add:
```json
{
    "extends": "./node_modules/@josundt/tsconfig-rules/tsconfig.base.web.esm.json"
}
```

### B - AMD projects
In the top of your project's tsconfig.json file, add:
```json
{
    "extends": "./node_modules/@josundt/tsconfig-rules/tsconfig.base.web.amd.json"
}
```

## NPM package projects - multi-targeting
When building multi-targeting npm packages, we commonly compile 3 different versions, with different entry point references in the package.json of the npm package:

1.  **ES5 target with UMD modules**  
Works in both node and web, and the  compiled entrypoint file path is set in the npm package's ***main*** property in package.json:
```json
{
    "extends": "./node_modules/@josundt/tsconfig-rules/tsconfig.base.es5.umd.json"
}
```

2. **ES5 target with ES modules**
Compiles to ES5 but uses ES2017 module import syntax. The compiled entrypoint file path is set in the npm package's ***module*** property in package.json. *PS! Webpack prefers this entrypoint (module instead of main) in the bundle module dependency crawler*:
```json
{
    "extends": "./node_modules/@josundt/tsconfig-rules/tsconfig.base.web.esm.json"
}
```

3. **ES2017 target with ES modules**
Compiles to ES2017 and uses ES2017 module import syntax. The compiled entrypoint file path is set in the npm package's ***ES2017*** property in package.json. *This is for future fully ES2017 and ES2017 modules compliant browsers/node engines.**
```json
{
    "extends": "./node_modules/@josundt/tsconfig-rules/tsconfig.base.es2017.esm.json"
}
```

## Node projects

In your project's tsconfig.json file:
```json
{
    "extends": "./node_modules/@josundt/tsconfig-rules/tsconfig.base.node.json"
}
```