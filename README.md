# Visual Studio Code - Notes

This repository will contain notes, files(?), and other information pertaining *Visual Studio Code*. There is no code here. Except where it is related to using *Visual Studio Code*.

## Environment

OS: Windows 10 Professional

Application Type: Node.js + JavaScript

Visual Studio Code: v1.13.1

Node.js: v6.10.2, 8.9.0, 8.17.0

Problems occurred with newer versions of each Node.js and Visual Studio Code. A description of a *breakpoint failure bug* please look [here](vscode-brkpt_bug.md).

## Launch JSON File

This file describes one or more methods that ***vscode*** will use when starting your program. The *launch configurations* described here will be for non-typical use.

### 

### Attach to Process ID

This method is useful when your Node.js script is expecting input from ***stdin*** (*think "keyboard"*). It is best debugging an already runnin Node.js instance.  It is necessary to start the instance from within the same folder that vscode is referencing.

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "attach",
            "name": "Attach by Process ID",
            "processId": "${command:PickProcess}"
        }
     ]
}
```

#### ATTACH - Running Node.js 

The script `test.js` uses the following code to read from `stdin`:

```
process.stdin.on('data', function(inputStdin) {
    // check the EOLs for CRLF or just LF
    if(inputStdin.includes('\r')) {
        inputArray = inputStdin.split('\r\n');
    } else {
        inputArray = inputStdin.split('\n');
    }
    main();
});
```

Run with the following:

```
node --debug-brk test.js <input.txt
```

### External Console

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "externalConsole": true,
            "name": "Launch Program - test.js",
            "program": "${workspaceFolder}/test.js"
        }
     ]
}
```

### Remote Debug

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "attach",
            "name": "Attach to Remote",
            "address": "192.168.0.1000",
            "port": 9229,
            "localRoot": "${workspaceFolder}",
            "remoteRoot": "/volume1/NodeSrv/apps/test"
        }
    ]
}
```








Redirect input to `test.js`:

```
> node test.js <input.txt
```

Piping input to `test.js`:

```
type input.txt | node test.js
```


