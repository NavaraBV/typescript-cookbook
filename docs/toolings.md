## Toolings

There are several tools to make your life easier when you are using TypeScript.
As with every tool, it is up to you to find what works best for you and which tool is right for the job.

### Playground
The [TypeScript playground](https://www.typescriptlang.org/play) is the ideal way to quickly try something, with all the features you want from an IDE:
* Ability to switch between different TypeScript versions, including the `nightly` version
* Hovering over a value will give you code hints
* Allows you to interactively use the `tsconfig.json` file with explanation for each property
* You can directly load in new features that are implemented and experiment with them
* See the compiled `.js` variant of your TS code
* Share your code

### ts-node
Most common way to when developing your Angular, React or your other framework is using webpack in combination with `ts-loader` and `babel` to compile, run and watch your Typescript code.
But sometimes, you just want to run a single file and quickly test a couple of files.
For those specific scenario's, you could run `tsc` which comes with `npm install typescript` to compile it to `.js` and then run `node your_file.js`.
But [`ts-node`](https://github.com/TypeStrong/ts-node) provides you a way to directly run a TypeScript file.

You install it with `npm install ts-node` and run it with `./node_modules/.bin/ts-node your_file.ts`.
Or you can add `ts-node` to your `package.json` script section.

Note: `ts-node` has no watch functionality.

### eslint
Almost every frontend developers knows about `eslint`. But it was common to use `tslint` for creating and using user defined linting rules for your TypeScript files.
But with the introduction of the monorepo [`typescript-eslint`](https://github.com/typescript-eslint/typescript-eslint) it is no longer necessary to use `tslint`.

There are some conflicting rules between `eslint` and `typescript-eslint`, for example the [`semi`](https://github.com/typescript-eslint/typescript-eslint/blob/master/packages/eslint-plugin/docs/rules/semi.md).
But can quickly identifies those conflicting rule and fix them.

### DefinitelyTyped
Not all libraries include their `.d.ts` files or even have them.
Luckily the JavaScript/TypeScript community is quite large and almost every large library has a `@types/` equilevant written in the [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) repository.
Just search for `@types` and your library.