# vscode-ts-tslint (PREVIEW)

Same as [vscode-tslint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) this extension integrates the [tslint](https://github.com/palantir/tslint) linter for the TypeScript language into VS Code. This integration uses the new TypeScript language server plugin support introduced in TypeScript version [2.3](https://marketplace.visualstudio.com/items?itemName=eg2.tslint). It runs `tslint` as a plugin for the TypeScript language server.

Please refer to the tslint [documentation](https://github.com/palantir/tslint) for how to configure the linting rules.

# Limitations

This is a barebones preview and many of the features provided by the vscode-tslint extension are not yet provided. The currently implementation only supports linting and to auto fix a single rule failure, if the rule provides an autofix. There is no support for:
- auto fix on save
- revalidation when the `tslint.json` changes and you have to run the `Reload Window` command.

Due to an issue in the tslint implementation of the `no-unused-variable rule` ([#2649](https://github.com/palantir/tslint/issues/2649)), this rule has to be disabled by the plugin. You can use the typescript compiler options `noUnusedLocals` and `noUnusedParameters` instead.

# Differences between vscode-tslint and vscode-ts-tslint

- the plugin shares the program representation with TypeScript. This is more efficient than the vscode-tslint extension which needs to reanalyze the document. 
- Since vscode-tslint lints one file a time only, it cannot support tslint rules that require the type checker. The language service plugin doesn't have this limitation.
- The vsocde-ts-tslint extension bundles a tslint version. 

# Setting up linting

- if you are already using vscode-tslint, then disable it first, otherwise linting warnings will be reported twice.
- enable the tslint-server-plugin in the `tsconfig.json` file of your project:

```json
{
  "compilerOptions": {
    "plugins": [
      { "name": "tslint-language-service"}
    ]
  }
}
```

# Using a different version of tslint than the version that is bundled with the extension

The extension comes with a particular version of tslint. If you want to use a different version then you have to use `npm` install the `tslint-languageservice` module and `tslint` as a peer to the TypeScript version you want to use. 
The modules folder should have the following layout:
- node_modules
  - typescript
  - tslint
  - tslint-language-service

If you are extending the TypeScript version that is installed into your workspace then make sure to use this version and not the one that comes with VS Code. Otherwise you will have configured the setting `typescript.tsdk` already and things should just work. 
