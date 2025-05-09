# Install Visual Studio Code

Developers are free to use their own IDE/text editor of choice.  However, this guide will assume [Visual Studio Code](https://code.visualstudio.com/) will be used as that is the most commonly used tool in FCP and Defra.

If not using VS Code, this guide should still be read to ensure that the equivalent installation steps are applied.

Visual Studio Code is a source code editor developed by Microsoft for Windows, Linux and Mac. It includes support for debugging, embedded Git control and GitHub, syntax highlighting, intelligent code completion, snippets, and code refactoring.

## Installation VS Code

Install for your operating system following [Microsoft's guidance](https://code.visualstudio.com/download)

### Configure autosave

If you prefer your code to autosave, add the following to settings.json.

```json
"files.autoSave": "afterDelay"
"files.autoSaveDelay": 600
```

### Indentation

VS Code supports multiple indentation settings for each language by updating the `settings.json` file.

The below example sets a default indentation to two spaces, but uses four spaces for C# files as per Microsoft standards.

```json
{
  "editor.tabSize": 2,
  "[csharp]": {
    "editor.tabSize": 4,
  }
}
```

## WSL Configuration
### Install Remote WSL extension

The [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) allows VS Code installed in Windows to be used in WSL.

This means when code is source controlled in WSL, VS Code can read, debug and interact with it without workarounds.

In WSL2 it is recommended to always use the remote WSL approach.  A current directory can be opened in VS Code with `code .`.

## Turn on signed commits

To turn on signed commits (see section on signed commits for more information) in the UI make sure the `Enable commit signing with GPG or X.509` is selected. 

For the JSON version of the settings use `"git.enableCommitSigning": true`
