# Working Notes

- tslint breaks when typescript is a run time dependency
- when removing typescript then there is a missing peer dependency for tslint

As a workaround the published vsix is patched and typescript is removed from the VSIX