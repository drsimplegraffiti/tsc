Typescript Core Types:

- number: 1, 5.4, -99
- string: 'hello world', "hello world", `hello world`
- boolean: true, false
- object: {name: "joseph"}
- Array [1,2,3]
- tuples [1,2]
- enum {NEW, OLD}
- any \*
- void
- unknown
- never

## Working with numbers

```typescript
function add(n1: number, n2: number) {
  return n1 + n2;
}

const number1 = 5;
const number2 = 2.8;

const result = add(number1, number2);
console.log(result);
```

## Working with booleans

```typescript
function add(n1: number, n2: number, showResult: boolean) {
  if (showResult) {
    console.log(n1 + n2);
  }
  return n1 + n2;
}

const number1 = 5;
const number2 = 2.8;
const printResult = true;

add(number1, number2, printResult);
```

---

## Mixing data types

```typescript
function add(n1: number, n2: number, showResult: boolean, phrase: string) {
  const result = n1 + n2;
  if (showResult) {
    console.log(phrase + result);
  }
  return result;
}

const number1 = 5;
const number2 = 2.8;
const printResult = true;
const resultPhrase = "Result is: ";

add(number1, number2, printResult, resultPhrase);
```

## Note:

all numbers are treated as floats in tsc

## Best practice

```typescript
let number1: number;
number1 = 7;
```

---

## Object Data Types

- Generally assigning an object

```typescript
const person: object = {
  name: "joe",
  age: 4,
};

console.log(person);
```

- Specifically adding objects

```typescript
const person: {
  name: string;
  age: number;
} = {
  name: "joe",
  age: 4,
};

console.log(person.name);
```

---

- Arrays Data types @tsc

```typescript
const person = {
  name: "joe",
  age: 4,
  hobbies: ["Sports", "Cooking"],
};

let myHobbies: string[];
myHobbies = ["Sports"];
console.log(person.name);
```

```typescript
const person = {
  name: "joe",
  age: 4,
  hobbies: ["Sports", "Cooking"],
};

let myHobbies: string[];
myHobbies = ["Sports"];
console.log(person.name);

for (const hobby of person.hobbies) {
  console.log(hobby.toLowerCase());
}
```

- Tuples
  to avoid mutabilty of certain data e.g

```typescript
const person = {
  name: "joe",
  age: 4,
  hobbies: ["Sports", "Cooking"],
};

let myHobbies: string[];
myHobbies = ["Sports"];
console.log(person.name);

for (const hobby of person.hobbies) {
  console.log(hobby.toLowerCase());
}
```

- We use tuples:

```typescript
const person: {
  name: string;
  age: number;
  hobbies: string[];
  role: [number, string]; //exactly two elements, first being a number , second string
} = {
  name: "joe",
  age: 4,
  hobbies: ["Sports", "Cooking"],
  role: [3, "author"],
};

person.role = [0, "admin"];
```

- Enum

```typescript
// Define global constants
const ADMIN = 0;
const READ_ONLY = 1;
const AUTHOR = 2;

const person = {
  name: "joe",
  age: 4,
  hobbies: ["Sports", "Cooking"],
  role: ADMIN,
};
if (person.role === ADMIN) {
  console.log(" is read only");
}
```

Revamping the above code bas with Enum

```typescript
enum Role {
  ADMIN,
  READ_ONLY,
  AUTHOR,
}

const person = {
  name: "joe",
  age: 4,
  hobbies: ["Sports", "Cooking"],
  role: Role.ADMIN,
};
if (person.role === Role.ADMIN) {
  console.log(" is read only");
}
```

- Compiled result of enum

```typescript
var Role;
(function (Role) {
  Role[(Role["ADMIN"] = 0)] = "ADMIN";
  Role[(Role["READ_ONLY"] = 1)] = "READ_ONLY";
  Role[(Role["AUTHOR"] = 2)] = "AUTHOR";
})(Role || (Role = {}));
var person = {
  name: "joe",
  age: 4,
  hobbies: ["Sports", "Cooking"],
  role: Role.ADMIN,
};
if (person.role === Role.ADMIN) {
  console.log(" is read only");
}
```

---

- Assigning values to enum

```typescript
// Assigning Values to enums

enum Role {
  ADMIN = "ADMIN",
  READ_ONLY = 100,
  AUTHOR = "AUTHOR",
}

const person = {
  name: "joe",
  age: 4,
  hobbies: ["Sports", "Cooking"],
  role: Role.ADMIN,
};
if (person.role === Role.ADMIN) {
  console.log(" is read only");
}
```

---

- Any

```typescript
let myHobbies: any;
let myFavHobbies: string[];

myHobbies = true; // correct
myFavHobbies = 7; //wrong
```

---

- Union

```typescript
function combine(input1: number, input2: number) {
  const result = input1 + input2;
  return result;
}

const combineAges = combine(67, 34);
console.log(combineAges);

const combinedNames = combine("joe", "jane");
```

The combinedNames will fail, to fix use a union types:

```typescript
function combine(input1: number | string, input2: number | string) {
  let result;
  if (typeof input1 === "number" && typeof input2 === "number") {
    result = input1 + input2;
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;
}

const combineAges = combine(67, 34);
console.log(combineAges);

const combinedNames = combine("joe", "jane");
console.log(combinedNames);
```

## Union example 2

```typescript
function combine(
  input1: number | string,
  input2: number | string,
  resultConversion: string
) {
  let result;
  if (typeof input1 === "number" && typeof input2 === "number") {
    result = input1 + input2;
  } else {
    result = input1.toString() + input2.toString();
  }
  if (resultConversion === "as-number") {
    return +result;
  } else {
    return result.toString();
  }
}

const combineAges = combine(67, 34, "as-number");
console.log(combineAges);

const combineStringAges = combine("67", "34", "as-number");
console.log(combineStringAges);

const combinedNames = combine("joe", "jane", "as-text");
console.log(combinedNames);
```

Ex 2

```typescript
function combine(
  input1: number | string,
  input2: number | string,
  resultConversion: string
) {
  let result;
  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversion === "as-number"
  ) {
    result = +input1 + +input2;
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;
}

const combineAges = combine(67, 34, "as-number");
console.log(combineAges);

const combineStringAges = combine("67", "34", "as-number");
console.log(combineStringAges);

const combinedNames = combine("joe", "jane", "as-text");
console.log(combinedNames);
```

---

- Literal types

```typescript
function combine(
  input1: number | string,
  input2: number | string,
  resultConversion: "as-number" | "as-text"
) {
  let result;
  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversion === "as-number"
  ) {
    result = +input1 + +input2;
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;
}

const combineAges = combine(67, 34, "as-number");
console.log(combineAges);

const combineStringAges = combine("67", "34", "as-number");
console.log(combineStringAges);

const combinedNames = combine("joe", "jane", "as-text");
console.log(combinedNames);
```

---

- Type Alias

```typescript
type Combinable = number | string; //type alias: Reusable type values
type ConversionDescriptor = "as-number" | "as-text";

function combine(
  input1: Combinable,
  input2: Combinable,
  resultConversion: ConversionDescriptor
) {
  let result;
  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversion === "as-number"
  ) {
    result = +input1 + +input2;
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;
}

const combineAges = combine(67, 34, "as-number");
console.log(combineAges);

const combineStringAges = combine("67", "34", "as-number");
console.log(combineStringAges);

const combinedNames = combine("joe", "jane", "as-text");
console.log(combinedNames);
```

---

- Return types

```typescript
function add(n1: number, n2: number): number {
  return n1 + n2;
}
```

---

Note: always allow tsc to types inference

- Use void if you don't have a return statement

```typescript
function add(n1: number, n2: number): number {
  return n1 + n2;
}
function printResult(num: number): void {
  //use void if the function returns nothing
  console.log("Result: ", +num);
}

printResult(add(4, 5));
```

---

```typescript
function add(n1: number, n2: number): number {
  return n1 + n2;
}
function printResult(num: number): undefined {
  //use void if the function returns nothing
  console.log("Result: ", +num);
  return;
}

printResult(add(4, 5));
```

---

- Function

```typescript
function add(n1: number, n2: number) {
  return n1 + n2;
}

let combineValues: Function; // this prevent us from mistakenly typing a value in place of a function
combineValues = add;
console.log(combineValues(4, 5));
```

The above code will work while the one beneath will not work

```typescript
function add(n1: number, n2: number) {
  return n1 + n2;
}

let combineValues: Function; // this prevent us from mistakenly typing a value in place of a function
combineValues = 3;
console.log(combineValues(4, 5));
```

---

## Function types:

- Describes specifically how and what a function should look like

```typescript
function add(n1: number, n2: number) {
  return n1 + n2;
}

// let combineValues:()=>number; //means the combineValues accepts any functions that takes in no parameter and returns a number
let combineValues: (a: number, b: number) => number; //means the combineValues accepts any functions that takes in two parameters, where each parameter is a number,  and the overall function returns a number

combineValues = add;
console.log(combineValues(4, 5));
```

---

## Call Back

```typescript
function add(n1: number, n2: number) {
  return n1 + n2;
}

function addAndHandle(n1: number, n2: number, cb: (num: number) => void) {
  const result = n1 + n2;
  cb(result);
}

let combineValues: (a: number, b: number) => number; //means the combineValues accepts any functions that takes in two parameters, where each parameter is a number,  and the overall function returns a number

combineValues = add;
console.log(combineValues(4, 5));

addAndHandle(22, 34, (result) => {
  console.log(result);
});
```

---

## Unknown types

```typescript
let userInput: unknown;
let username = "hat";
userInput = 5;
userInput = "Joe";

if (typeof userInput === "string") {
  //add extra checks
  username = userInput;
}

console.log(username);
```

- Note: Use union over unknown, and unknown over any

---

## Never

- This function never returned a result since an error is thrown

```typescript
function generateError(message: string, code: number): never {
  throw { message: message, errorCode: code };
}

generateError("An Error occurred", 500);
```

## Typescript compilers

- To avoid running `tsc filename` at every instance we use tsc compiler by using the watch mode.

<kbd>tsc app.ts -w</kbd> or <kbd>tsc app.ts --watch</kbd>.
Note: The watch mode should be kept opened during development

## General watch node

<kbd>tsc --init</kbd> to initial it as a tsc project

The command above create the `tsconfig.json` file.

To run a tsc file in the project, you simply run : <kbd>tsc</kbd> or <kbd>tsc -w</kbd> for watch mode.

## Configuring the tsconfig

- Exclude options

```typescript
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Projects */
    // "incremental": true,                              /* Enable incremental compilation */
    // "composite": true,                                /* Enable constraints that allow a TypeScript project to be used with project references. */
    // "tsBuildInfoFile": "./",                          /* Specify the folder for .tsbuildinfo incremental compilation files. */
    // "disableSourceOfProjectReferenceRedirect": true,  /* Disable preferring source files instead of declaration files when referencing composite projects */
    // "disableSolutionSearching": true,                 /* Opt a project out of multi-project reference checking when editing. */
    // "disableReferencedProjectLoad": true,             /* Reduce the number of projects loaded automatically by TypeScript. */

    /* Language and Environment */
    "target": "es2016",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
    // "lib": [],                                        /* Specify a set of bundled library declaration files that describe the target runtime environment. */
    // "jsx": "preserve",                                /* Specify what JSX code is generated. */
    // "experimentalDecorators": true,                   /* Enable experimental support for TC39 stage 2 draft decorators. */
    // "emitDecoratorMetadata": true,                    /* Emit design-type metadata for decorated declarations in source files. */
    // "jsxFactory": "",                                 /* Specify the JSX factory function used when targeting React JSX emit, e.g. 'React.createElement' or 'h' */
    // "jsxFragmentFactory": "",                         /* Specify the JSX Fragment reference used for fragments when targeting React JSX emit e.g. 'React.Fragment' or 'Fragment'. */
    // "jsxImportSource": "",                            /* Specify module specifier used to import the JSX factory functions when using `jsx: react-jsx*`.` */
    // "reactNamespace": "",                             /* Specify the object invoked for `createElement`. This only applies when targeting `react` JSX emit. */
    // "noLib": true,                                    /* Disable including any library files, including the default lib.d.ts. */
    // "useDefineForClassFields": true,                  /* Emit ECMAScript-standard-compliant class fields. */

    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    // "rootDir": "./",                                  /* Specify the root folder within your source files. */
    // "moduleResolution": "node",                       /* Specify how TypeScript looks up a file from a given module specifier. */
    // "baseUrl": "./",                                  /* Specify the base directory to resolve non-relative module names. */
    // "paths": {},                                      /* Specify a set of entries that re-map imports to additional lookup locations. */
    // "rootDirs": [],                                   /* Allow multiple folders to be treated as one when resolving modules. */
    // "typeRoots": [],                                  /* Specify multiple folders that act like `./node_modules/@types`. */
    // "types": [],                                      /* Specify type package names to be included without being referenced in a source file. */
    // "allowUmdGlobalAccess": true,                     /* Allow accessing UMD globals from modules. */
    // "resolveJsonModule": true,                        /* Enable importing .json files */
    // "noResolve": true,                                /* Disallow `import`s, `require`s or `<reference>`s from expanding the number of files TypeScript should add to a project. */

    /* JavaScript Support */
    // "allowJs": true,                                  /* Allow JavaScript files to be a part of your program. Use the `checkJS` option to get errors from these files. */
    // "checkJs": true,                                  /* Enable error reporting in type-checked JavaScript files. */
    // "maxNodeModuleJsDepth": 1,                        /* Specify the maximum folder depth used for checking JavaScript files from `node_modules`. Only applicable with `allowJs`. */

    /* Emit */
    // "declaration": true,                              /* Generate .d.ts files from TypeScript and JavaScript files in your project. */
    // "declarationMap": true,                           /* Create sourcemaps for d.ts files. */
    // "emitDeclarationOnly": true,                      /* Only output d.ts files and not JavaScript files. */
    // "sourceMap": true,                                /* Create source map files for emitted JavaScript files. */
    // "outFile": "./",                                  /* Specify a file that bundles all outputs into one JavaScript file. If `declaration` is true, also designates a file that bundles all .d.ts output. */
    // "outDir": "./",                                   /* Specify an output folder for all emitted files. */
    // "removeComments": true,                           /* Disable emitting comments. */
    // "noEmit": true,                                   /* Disable emitting files from a compilation. */
    // "importHelpers": true,                            /* Allow importing helper functions from tslib once per project, instead of including them per-file. */
    // "importsNotUsedAsValues": "remove",               /* Specify emit/checking behavior for imports that are only used for types */
    // "downlevelIteration": true,                       /* Emit more compliant, but verbose and less performant JavaScript for iteration. */
    // "sourceRoot": "",                                 /* Specify the root path for debuggers to find the reference source code. */
    // "mapRoot": "",                                    /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                          /* Include sourcemap files inside the emitted JavaScript. */
    // "inlineSources": true,                            /* Include source code in the sourcemaps inside the emitted JavaScript. */
    // "emitBOM": true,                                  /* Emit a UTF-8 Byte Order Mark (BOM) in the beginning of output files. */
    // "newLine": "crlf",                                /* Set the newline character for emitting files. */
    // "stripInternal": true,                            /* Disable emitting declarations that have `@internal` in their JSDoc comments. */
    // "noEmitHelpers": true,                            /* Disable generating custom helper functions like `__extends` in compiled output. */
    // "noEmitOnError": true,                            /* Disable emitting files if any type checking errors are reported. */
    // "preserveConstEnums": true,                       /* Disable erasing `const enum` declarations in generated code. */
    // "declarationDir": "./",                           /* Specify the output directory for generated declaration files. */
    // "preserveValueImports": true,                     /* Preserve unused imported values in the JavaScript output that would otherwise be removed. */

    /* Interop Constraints */
    // "isolatedModules": true,                          /* Ensure that each file can be safely transpiled without relying on other imports. */
    // "allowSyntheticDefaultImports": true,             /* Allow 'import x from y' when a module doesn't have a default export. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables `allowSyntheticDefaultImports` for type compatibility. */
    // "preserveSymlinks": true,                         /* Disable resolving symlinks to their realpath. This correlates to the same flag in node. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */

    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                            /* Enable error reporting for expressions and declarations with an implied `any` type.. */
    // "strictNullChecks": true,                         /* When type checking, take into account `null` and `undefined`. */
    // "strictFunctionTypes": true,                      /* When assigning functions, check to ensure parameters and the return values are subtype-compatible. */
    // "strictBindCallApply": true,                      /* Check that the arguments for `bind`, `call`, and `apply` methods match the original function. */
    // "strictPropertyInitialization": true,             /* Check for class properties that are declared but not set in the constructor. */
    // "noImplicitThis": true,                           /* Enable error reporting when `this` is given the type `any`. */
    // "useUnknownInCatchVariables": true,               /* Type catch clause variables as 'unknown' instead of 'any'. */
    // "alwaysStrict": true,                             /* Ensure 'use strict' is always emitted. */
    // "noUnusedLocals": true,                           /* Enable error reporting when a local variables aren't read. */
    // "noUnusedParameters": true,                       /* Raise an error when a function parameter isn't read */
    // "exactOptionalPropertyTypes": true,               /* Interpret optional property types as written, rather than adding 'undefined'. */
    // "noImplicitReturns": true,                        /* Enable error reporting for codepaths that do not explicitly return in a function. */
    // "noFallthroughCasesInSwitch": true,               /* Enable error reporting for fallthrough cases in switch statements. */
    // "noUncheckedIndexedAccess": true,                 /* Include 'undefined' in index signature results */
    // "noImplicitOverride": true,                       /* Ensure overriding members in derived classes are marked with an override modifier. */
    // "noPropertyAccessFromIndexSignature": true,       /* Enforces using indexed accessors for keys declared using an indexed type */
    // "allowUnusedLabels": true,                        /* Disable error reporting for unused labels. */
    // "allowUnreachableCode": true,                     /* Disable error reporting for unreachable code. */

    /* Completeness */
    // "skipDefaultLibCheck": true,                      /* Skip type checking .d.ts files that are included with TypeScript. */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  },
  "exclude":["basics.ts"]
}
```

- The exclude function stops the `basics.ts` from compiling

## Wild card

<mark> "exclude":["*.dev.ts"]</mark> means excludes all files ending with `.dev`.

<mark> "exclude":["**/*.dev.ts"]</mark> means excludes all files ending with `**/*.dev.ts` pattern.

## Exclude node modules

<mark> "exclude":["node_modules"]</mark> means excludes node_modules.

- Note:
  If you don't specify exclude node_modules option tsc automatically excludes it

## Include node modules

```typescript
"include":[
  "app.ts"
]
```

The above code only includes `app.ts` file and ignores the rest. So this should not be used

## Compiler Options

- Target
  You can change the compiler options to <kbd>"target": "es6"</kbd> or any other recent Js by using the autocompletion Ctrl + space

-Lib
The lib options unlock all the dom element in tsc

```typescript
"lib":[
  "dom",
  "es6",
  "dom.iterable",
  "scripthost"
]

The above code is equal to setting target:es6
```

---

## Outdir and rootDir
The normal project structure is to have the `src` folder and the `dist` folder.

* The dist folder hold the output of your project
* The source folder hold the output of your tsc files

We can tell tsc to output where the created JS files should be stored.
Basically we set out rootDir: ./src and outDir: ./dist
```javascript
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Projects */
    // "incremental": true,                              /* Enable incremental compilation */
    // "composite": true,                                /* Enable constraints that allow a TypeScript project to be used with project references. */
    // "tsBuildInfoFile": "./",                          /* Specify the folder for .tsbuildinfo incremental compilation files. */
    // "disableSourceOfProjectReferenceRedirect": true,  /* Disable preferring source files instead of declaration files when referencing composite projects */
    // "disableSolutionSearching": true,                 /* Opt a project out of multi-project reference checking when editing. */
    // "disableReferencedProjectLoad": true,             /* Reduce the number of projects loaded automatically by TypeScript. */

    /* Language and Environment */
    "target": "es2016",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
    // "lib": [],                                        /* Specify a set of bundled library declaration files that describe the target runtime environment. */
    // "jsx": "preserve",                                /* Specify what JSX code is generated. */
    // "experimentalDecorators": true,                   /* Enable experimental support for TC39 stage 2 draft decorators. */
    // "emitDecoratorMetadata": true,                    /* Emit design-type metadata for decorated declarations in source files. */
    // "jsxFactory": "",                                 /* Specify the JSX factory function used when targeting React JSX emit, e.g. 'React.createElement' or 'h' */
    // "jsxFragmentFactory": "",                         /* Specify the JSX Fragment reference used for fragments when targeting React JSX emit e.g. 'React.Fragment' or 'Fragment'. */
    // "jsxImportSource": "",                            /* Specify module specifier used to import the JSX factory functions when using `jsx: react-jsx*`.` */
    // "reactNamespace": "",                             /* Specify the object invoked for `createElement`. This only applies when targeting `react` JSX emit. */
    // "noLib": true,                                    /* Disable including any library files, including the default lib.d.ts. */
    // "useDefineForClassFields": true,                  /* Emit ECMAScript-standard-compliant class fields. */

    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    "rootDir": "./src",                                  /* Specify the root folder within your source files. */
    // "moduleResolution": "node",                       /* Specify how TypeScript looks up a file from a given module specifier. */
    // "baseUrl": "./",                                  /* Specify the base directory to resolve non-relative module names. */
    // "paths": {},                                      /* Specify a set of entries that re-map imports to additional lookup locations. */
    // "rootDirs": [],                                   /* Allow multiple folders to be treated as one when resolving modules. */
    // "typeRoots": [],                                  /* Specify multiple folders that act like `./node_modules/@types`. */
    // "types": [],                                      /* Specify type package names to be included without being referenced in a source file. */
    // "allowUmdGlobalAccess": true,                     /* Allow accessing UMD globals from modules. */
    // "resolveJsonModule": true,                        /* Enable importing .json files */
    // "noResolve": true,                                /* Disallow `import`s, `require`s or `<reference>`s from expanding the number of files TypeScript should add to a project. */

    /* JavaScript Support */
    // "allowJs": true,                                  /* Allow JavaScript files to be a part of your program. Use the `checkJS` option to get errors from these files. */
    // "checkJs": true,                                  /* Enable error reporting in type-checked JavaScript files. */
    // "maxNodeModuleJsDepth": 1,                        /* Specify the maximum folder depth used for checking JavaScript files from `node_modules`. Only applicable with `allowJs`. */

    /* Emit */
    // "declaration": true,                              /* Generate .d.ts files from TypeScript and JavaScript files in your project. */
    // "declarationMap": true,                           /* Create sourcemaps for d.ts files. */
    // "emitDeclarationOnly": true,                      /* Only output d.ts files and not JavaScript files. */
    // "sourceMap": true,                                /* Create source map files for emitted JavaScript files. */
    // "outFile": "./",                                  /* Specify a file that bundles all outputs into one JavaScript file. If `declaration` is true, also designates a file that bundles all .d.ts output. */
    "outDir": "./dist",                                   /* Specify an output folder for all emitted files. */
    // "removeComments": true,                           /* Disable emitting comments. */
    // "noEmit": true,                                   /* Disable emitting files from a compilation. */
    // "importHelpers": true,                            /* Allow importing helper functions from tslib once per project, instead of including them per-file. */
    // "importsNotUsedAsValues": "remove",               /* Specify emit/checking behavior for imports that are only used for types */
    // "downlevelIteration": true,                       /* Emit more compliant, but verbose and less performant JavaScript for iteration. */
    // "sourceRoot": "",                                 /* Specify the root path for debuggers to find the reference source code. */
    // "mapRoot": "",                                    /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                          /* Include sourcemap files inside the emitted JavaScript. */
    // "inlineSources": true,                            /* Include source code in the sourcemaps inside the emitted JavaScript. */
    // "emitBOM": true,                                  /* Emit a UTF-8 Byte Order Mark (BOM) in the beginning of output files. */
    // "newLine": "crlf",                                /* Set the newline character for emitting files. */
    // "stripInternal": true,                            /* Disable emitting declarations that have `@internal` in their JSDoc comments. */
    // "noEmitHelpers": true,                            /* Disable generating custom helper functions like `__extends` in compiled output. */
    // "noEmitOnError": true,                            /* Disable emitting files if any type checking errors are reported. */
    // "preserveConstEnums": true,                       /* Disable erasing `const enum` declarations in generated code. */
    // "declarationDir": "./",                           /* Specify the output directory for generated declaration files. */
    // "preserveValueImports": true,                     /* Preserve unused imported values in the JavaScript output that would otherwise be removed. */

    /* Interop Constraints */
    // "isolatedModules": true,                          /* Ensure that each file can be safely transpiled without relying on other imports. */
    // "allowSyntheticDefaultImports": true,             /* Allow 'import x from y' when a module doesn't have a default export. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables `allowSyntheticDefaultImports` for type compatibility. */
    // "preserveSymlinks": true,                         /* Disable resolving symlinks to their realpath. This correlates to the same flag in node. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */

    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                            /* Enable error reporting for expressions and declarations with an implied `any` type.. */
    // "strictNullChecks": true,                         /* When type checking, take into account `null` and `undefined`. */
    // "strictFunctionTypes": true,                      /* When assigning functions, check to ensure parameters and the return values are subtype-compatible. */
    // "strictBindCallApply": true,                      /* Check that the arguments for `bind`, `call`, and `apply` methods match the original function. */
    // "strictPropertyInitialization": true,             /* Check for class properties that are declared but not set in the constructor. */
    // "noImplicitThis": true,                           /* Enable error reporting when `this` is given the type `any`. */
    // "useUnknownInCatchVariables": true,               /* Type catch clause variables as 'unknown' instead of 'any'. */
    // "alwaysStrict": true,                             /* Ensure 'use strict' is always emitted. */
    // "noUnusedLocals": true,                           /* Enable error reporting when a local variables aren't read. */
    // "noUnusedParameters": true,                       /* Raise an error when a function parameter isn't read */
    // "exactOptionalPropertyTypes": true,               /* Interpret optional property types as written, rather than adding 'undefined'. */
    // "noImplicitReturns": true,                        /* Enable error reporting for codepaths that do not explicitly return in a function. */
    // "noFallthroughCasesInSwitch": true,               /* Enable error reporting for fallthrough cases in switch statements. */
    // "noUncheckedIndexedAccess": true,                 /* Include 'undefined' in index signature results */
    // "noImplicitOverride": true,                       /* Ensure overriding members in derived classes are marked with an override modifier. */
    // "noPropertyAccessFromIndexSignature": true,       /* Enforces using indexed accessors for keys declared using an indexed type */
    // "allowUnusedLabels": true,                        /* Disable error reporting for unused labels. */
    // "allowUnreachableCode": true,                     /* Disable error reporting for unreachable code. */

    /* Completeness */
    // "skipDefaultLibCheck": true,                      /* Skip type checking .d.ts files that are included with TypeScript. */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  },
  "exclude":["node_modules"]
}

```


