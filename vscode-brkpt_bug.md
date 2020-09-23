# Visual Studio Code Breakpoint Failure

This document will describe a *breakpoint bug* in Visual Studio Code that manifests as such:

* When debugging Node.js applications a breakpoint is set but execution does not stop, or appear to start in some cases. The bug appears with specific combinations of Node.js and Visual Studio Code versions.

## Test Platform

OS Version: Windows 10 Pro(2004)

Node.js Versions: all are managed with `nvm`

* 12.18.4
* 12.16.1
* 10.22.0
* 10.13.0
* 8.17.0 
* 8.9.0  
* 6.10.2

Visual Studio Code Versions: versions were not installed concurrently

* 1.11.1
* 1.13.1
* 1.20.1
* 1.29.1
* 1.49.1

## Tests

The basic procedure was:

1. Use `nvm` to install and use the necessary versions of Node.js
2. Uninstall Visual Studio Code if present.
3. Install Visual Studio Code. Start with the most recent version.
4. Select and use a Node.js version
5. In an appropriate folder create an `app.js` or `index.js` file.
6. Execute `npm init`, follow prompts.
7. Open folder with Visual Studio Code (*folder right-click context menu*)
8. Set 1 or more breakpoints.
9. Run.
10. Observe results, execution should stop on the first breakpoint.

### Files

**`app.js`** or **`index.js`**
```
console.log('here i am\n');

let x = 100;

console.log('x = '+x);

process.exit(0);
```

**`package.json`**
```
{
  "name": "vscode_bug",
  "version": "1.0.0",
  "description": "demonstrates breakpoint failure in vscode 1.49",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "debug": "node inspect index.js",
    "run-script": "node index.js"
  },
  "author": "jxmot",
  "license": "MIT"
}
```

**`.vscode/launch.json`**
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch workspaceFolder",
            "program": "${workspaceFolder}/index.js"
        },
        {
            "type": "node",
            "request": "launch",
            "name": "Launch workspaceRoot",
            "program": "${workspaceRoot}\\index.js"
        }
    ]
}
```

You will get 2 launch options, **Launch workspaceFolder** and **Launch workspaceRoot**. Either one is sufficient.

## Test Results

**v1.49.1**

* 12.18.4 **FAIL**
* 12.16.1 **FAIL**
* 10.22.0 **FAIL**
* 10.13.0 **FAIL**
* 8.17.0  **FAIL**
* 8.9.0   **FAIL**
* 6.10.2  **FAIL**

In all failed tests the application starting was attempted, however no output was seen on the console.

**v1.29.1**

* 12.18.4 **FAIL**
* 12.16.1 **FAIL**
* 10.22.0 **FAIL**
* 10.13.0 **FAIL**
* 8.17.0  **FAIL**
* 8.9.0   **FAIL**
* 6.10.2  **FAIL**

In all failed tests the application starting was attempted, however no output was seen on the console.

**v1.20.1**

* 12.18.4 FAIL
* 12.16.1 FAIL
* 10.22.0 FAIL
* 10.13.0 FAIL
* 8.17.0  **OK**
* 8.9.0   **OK**
* 6.10.2  **OK**

In all failed tests the application starting was attempted, however no output was seen on the console.

In all passing tests the application was started and it ran until completion. Output was seen on the console and break points were triggered.

**v1.13**

* 12.18.4 FAIL
* 12.16.1 FAIL
* 10.22.0 FAIL
* 10.13.0 FAIL
* 8.17.0  **OK**
* 8.9.0   **OK**
* 6.10.2  **OK**

In all failed tests the application starting was attempted, however no output was seen on the console.

In all passing tests the application was started and it ran until completion. Output was seen on the console and break points were triggered.

