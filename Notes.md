# Working Notes

- the language server plugin does not work correctly when typescript is a run time dependency and bundled with the extension. In this case only lint rules that do not need an AST work, e.g. no trailing white space. This works when the extension does not have a dependency on the typescript module.
- when removing typescript as a runtime dependency from the extension, then `vsce package` correctly reports that there is a missing peer dependency for tslint-language-service, it has typescript as a peer dependency.
- when removing both tslint and typescript as a runtime dependency then the TS plugin cannot find ´tslint´, which is expected since ´tslint´ is only a peer dependency of the tslint-language-service (which is debatable).
- As a workaround the VSIX is produced with typescript and tslint as a runtime dependency but it is then manually removed published vsix is patched and typescript is removed from the VSIX and the VSIX is then published using `vsce publish --packagePath vsix`.
