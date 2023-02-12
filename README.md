# ShellCheck for Visual Studio Code

Integrates [ShellCheck](https://github.com/koalaman/shellcheck) into VS Code, a linter for Shell scripts.

[![Latest version](https://badgen.net/github/release/vscode-shellcheck/vscode-shellcheck?label=Latest%20version)](https://github.com/vscode-shellcheck/vscode-shellcheck/releases/latest)
[![VS Marketplace installs](https://badgen.net/vs-marketplace/i/timonwong.shellcheck?label=VS%20Marketplace%20installs)](https://marketplace.visualstudio.com/items?itemName=timonwong.shellcheck)
[![VS Marketplace downloads](https://badgen.net/vs-marketplace/d/timonwong.shellcheck?label=VS%20Marketplace%20downloads)](https://marketplace.visualstudio.com/items?itemName=timonwong.shellcheck)
[![Open VSX downloads](https://badgen.net/open-vsx/d/timonwong/shellcheck?color=purple&label=Open%20VSX%20downloads)](https://open-vsx.org/extension/timonwong/shellcheck)

## Quick start

![Extension GIF](https://user-images.githubusercontent.com/29582865/106907134-c299c000-66b2-11eb-8d8b-ea1bd898cb3a.gif)

## Disclaimer

This VS Code extension requires [shellcheck] (the awesome static analysis tool for shell scripts) to work.

Precompiled [shellcheck] binaries are bundled in this extension for these platforms:

- Linux (x86_64, arm, arm64)
- macOS (x86_64, arm64)
- Windows (x86_64, arm)

## Requirements

1. Run [`Install Extension`](https://code.visualstudio.com/docs/editor/extension-gallery#_install-an-extension) command from [Command Palette](https://code.visualstudio.com/Docs/editor/codebasics#_command-palette).
2. Search and choose `shellcheck`.

## Options

There are various options that can be configured by making changes to your user or workspace preferences.

Default options are:

```jsonc
{
  "shellcheck.enable": true,
  "shellcheck.enableQuickFix": true,
  "shellcheck.run": "onType",
  "shellcheck.executablePath": "", // Priority: user defined > bundled shellcheck binary > "shellcheck"
  "shellcheck.exclude": [],
  "shellcheck.customArgs": [],
  "shellcheck.ignorePatterns": {
    "**/*.xonshrc": true,
    "**/*.xsh": true,
    "**/*.zsh": true,
    "**/*.zshrc": true,
    "**/zshrc": true,
    "**/*.zprofile": true,
    "**/zprofile": true,
    "**/*.zlogin": true,
    "**/zlogin": true,
    "**/*.zlogout": true,
    "**/zlogout": true,
    "**/*.zshenv": true,
    "**/zshenv": true,
    "**/*.zsh-theme": true
  },
  "shellcheck.ignoreFileSchemes": ["git", "gitfs", "output"]
}
```

### `shellcheck.ignorePatterns`

The `shellcheck.ignorePatterns` works exactly the same as `search.exclude`, read more about glob patterns [here](https://code.visualstudio.com/docs/editor/codebasics#_advanced-search-options)

For example:

```jsonc
{
  "shellcheck.ignorePatterns": {
    "**/*.zsh": true,
    "**/*.zsh*": true,
    "**/.git/*.sh": true,
    "**/folder/**/*.sh": true
  }
}
```

### Fix all errors on save

The auto-fixable errors can be fixed automatically on save by using the following configuration:

```jsonc
{
  "editor.codeActionsOnSave": {
    "source.fixAll.shellcheck": true
  }
}
```

Alternatively, you can fix all errors on demand by running the command _Fix All_ in the VS Code Command Palette.

### Lint onType or onSave

By default the linter will lint as you type. Alternatively, set `shellcheck.run` to `onSave` if you want to lint only when the file is saved (works best if auto-save is on).

```jsonc
{
  "shellcheck.run": "onType" // also: "onSave"
}
```

### Excluding Checks

By default all shellcheck checks are performed and reported on as necessary. To globally ignore certain checks in all files, add the "SC identifiers" to `shellcheck.exclude`. For example, to exclude [SC1017](https://github.com/koalaman/shellcheck/wiki/SC1017):

```jsonc
{
  "shellcheck.exclude": ["1017"]
}
```

### Using Docker version of shellcheck

In order to get it to work, you need a "shim" script, and then, just remember, do not try to construct command line arguments for shellcheck yourself.

Here is a simple "shim" script to get start with (See discussion: [#24](https://github.com/vscode-shellcheck/vscode-shellcheck/issues/24)):

```shell
#!/bin/bash

exec docker run --rm -i -v "$PWD:/mnt:ro" koalaman/shellcheck:latest "$@"
```

For example, you can place it at `shellcheck.sh` in the root of your workspace and ensure that it have execution permission with `chmod +x shellcheck.sh`. And then, you can configure the extension to use it:

```jsonc
// .vscode/settings.json
{
  // use the shim as shellcheck executable
  "shellcheck.executablePath": "${workspaceFolder}/shellcheck.sh",

  // you may also need to turn this option on, so shellcheck in the container
  // can access all the files in the workspace
  "shellcheck.useWorkspaceRootAsCwd": true
}
```

## Advanced usage

### Integrating other VS Code extensions

This extension provides a small API, which allows other VS Code extensions to interact with the ShellCheck extension. For details, see [API.md](./doc/API.md).

## Acknowledgements

This extension is based on [@hoovercj's Haskell Linter](https://github.com/hoovercj/vscode-haskell-linter).

### Contributors

- [@felipecrs](https://github.com/felipecrs)
- [@sylveon](https://github.com/sylveon)
- [@ralish](https://github.com/ralish)

## LICENSE

This extension is licensed under the [MIT license](./LICENSE).

Bundled [shellcheck] binaries are licensed under [GPLv3](https://github.com/koalaman/shellcheck/blob/master/LICENSE).

[shellcheck]: https://github.com/koalaman/shellcheck
