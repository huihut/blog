---

title: VSCode 的 C/C++ 调试环境的 launch.json、 tasks.json 文件
subtitle: VSCode 的 C/C++ 调试环境的 launch.json、 tasks.json 文件
date: 2018-06-12 21:32:13
author: huihut
tags:
	- C/C++
categories: 
	- CS
	
---

##  launch.json

```cpp
// Configuring tasks.json for C/C++ debugging
// author: huihut
// repo: https://gist.github.com/huihut/9548fe7e1084cf8e844120c5668b8177

// Available variables which can be used inside of strings.
// ${workspaceRoot}: the root folder of the team        
// ${file}: the current opened file                     
// ${fileBasename}: the current opened file's basename 
// ${fileDirname}: the current opened file's dirname    
// ${fileExtname}: the current opened file's extension  
// ${cwd}: the current working directory of the spawned process

{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "preLaunchTask": "build",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "gdb.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }]
}
```

<!-- more -->

## tasks.json

```cpp
// Configuring tasks.json for C/C++ debugging 
// author: huihut
// repo: https://gist.github.com/huihut/887d3c28db92617bd5148c20a5ff112a

// Available variables which can be used inside of strings.
// ${workspaceRoot}: the root folder of the team        
// ${file}: the current opened file                     
// ${fileBasename}: the current opened file's basename 
// ${fileDirname}: the current opened file's dirname    
// ${fileExtname}: the current opened file's extension  
// ${cwd}: the current working directory of the spawned process


{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared"
            },
            "windows": {
                "command": "g++",
                "args": [
                    "-ggdb",
                    "\"${file}\"",
                    "--std=c++11",
                    "-o",
                    "\"${fileDirname}\\${fileBasenameNoExtension}.exe\""
                ]
            }
        }
    ]
}
```