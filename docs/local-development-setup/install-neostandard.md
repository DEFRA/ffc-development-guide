# Install neostandard

[neostandard](https://github.com/neostandard/neostandard) is [Defra's agreed linting tool for JavaScript](https://defra.github.io/software-development-standards/standards/javascript_standards) and should be used on all new JavaScript projects.

It should also be added as a `devDependency` to new Node.js projects and all JavaScript code should be validated against it.

## Installation

Run the following command to install neostandard globally:

```bash
npm install -g neostandard
```

### VS Code

These settings should be added to your `settings.json` file to disable the default JavaScript validation and automatically fix linting issues when saving files

```json
{
  "javascript.validate.enable": false,
  "eslint.format.enable": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": "explicit"
  }
}
```

## Standard JS

Standard JS was previously Defra's choice of linter, however this is no longer maintained as it once was.

neostandard is the spiritual successor to Standard JS and is maintained by many of the same people.

Whilst there is no immediate requirement that teams migrate existing codebases to neostandard, new projects should use neostandard.

As many existing FCP projects will be configured to use Standard JS, it may be beneficial to also install the Standard JS CLI and [VS Code extension](https://marketplace.visualstudio.com/items/?itemName=standard.vscode-standard).

```bash
npm install -g standard
```

### VS Code

VS Code can support both neostandard and Standard JS.  The decision will automatically be based on what is included within the project's `package.json` file.

These settings should be added to your `settings.json` file to disable the default JavaScript validation and automatically fix linting issues when saving files

```json
{
  "javascript.validate.enable": false,
  "standard.autoFixOnSave": true
}
```
