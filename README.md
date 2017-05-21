# vscode-tslint(vnext) PREVIEW

This is a preview of a work in progress reimplementation of the [vscode-tslint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) extension. It is published to enable early feedback and testing. The implementation uses the new TypeScript language server plugin support introduced in TypeScript version [2.3](https://marketplace.visualstudio.com/items?itemName=eg2.tslint). It runs `tslint` as a plugin for the TypeScript language server.

The extension can be run side-by-side with the current `vscode-tslint` extension, use the Extensions Viewlet to disable the version of the extension you do not want to use.

# Limitations

This is a preview and many of the features provided by the vscode-tslint extension are not yet provided. See this issue for the list of known gaps between `vscode-tslint`see this [issue](https://github.com/angelozerr/tslint-language-service/issues/32).

Due to an issue in the tslint implementation of the `no-unused-variable rule` ([#2649](https://github.com/palantir/tslint/issues/2649)), this rule has to be disabled by the plugin. You can use the typescript compiler options `noUnusedLocals` and `noUnusedParameters` instead.

# Differences between vscode-tslint (vnext) and vscode-tslint

- enabling of tslint is now done inside the `tsconfig.json` by enabling a TypeScript server plugin (see below).
- the implementation as a TypeScript server plugin enables to shares the program representation with TypeScript. This is more efficient than the current vscode-tslint implementation. The current implementation needs to reanalyze a document that has already been analyzed by the TypeScript language server. 
- vscode-tslint can only lint one file a time. It therefore cannot support [semantic tslint rules](https://palantir.github.io/tslint/usage/type-checking/) that require the type checker. The language service plugin doesn't have this limitation. To overcome this limitation is a key motivation for reimplementing the extension.
- the extension bundles a tslint and typescript version. 

# Setting up linting

Enable the tslint-server-plugin in the `tsconfig.json` file of your project:

```json
{
  "compilerOptions": {
    "plugins": [
      { "name": "tslint-language-service"}
    ]
  }
}
```

You can define additional settings for a `plugin`
```json
{
    "compilerOptions": {
        "plugins": [
            {
                "name": "tslint-language-service",
                "alwaysShowRuleFailuresAsWarnings": false,
                "ignoreDefinitionFiles": true
                //"configFile": "../tslint.json",
                //"disableNoUnusedVariableRule": false
            }
        ],
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
