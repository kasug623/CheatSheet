# VSCode
## combination with WSL
### `VSCode Server`
communicate between `VSCode` on `Windows Host` and one on `WSL`  
- [image of VSCode Server](https://novnote.com/vscode-wsl-cpp-env/511/#i)  

## setting.json
### example
```json
{
    "vim.insertModeKeyBindings": [
        {
            "before": [
                "j",
                "j"
            ],
            "after": [
                "<Esc>"
            ]
        }
    ],
    "python.analysis.extraPaths": [
        "/home/user/virtual_py3.10/lib/python3.10/site-packages"
    ],
    "python.autoComplete.extraPaths": [
        "/home/user/virtual_py3.10/lib/python3.10/site-packages"
    ],
}
```
### venv
If you search `venv` on setting windows, you can easily put a configuration to `setting.json` with background.  
But I perefer to write in `setting.json` for user manually.
- [shortcut](https://note.com/masato1230/n/nbc9be924edef)  

## how to install VSCode to WSL
- https://qiita.com/nj_ryoo0/items/a42c47436b77310f5430  
- https://blog.goo.ne.jp/yoossh/e/42255ea71b9a709d040c01d0274c04f7

## Auto Completion
- https://qiita.com/Sunrise98/items/af866502b06165c3ae40  
- https://www.yasunaga-lab.bio.kyutech.ac.jp/EosJ/index.php/Python%E3%81%AE%E3%81%9F%E3%82%81%E3%81%AEVisual_Studio_Code%E8%A8%AD%E5%AE%9A%E8%A6%9A%E6%9B%B8

## Debug
### setup for debug
Use launch.json.
- [format of launch.json](https://amateur-engineer-blog.com/vscode-launchjson/)  
- [how to load custom library for debug](https://www.yasunaga-lab.bio.kyutech.ac.jp/EosJ/index.php/Visual_Studio_Code%E3%81%A7Python%E3%83%87%E3%83%90%E3%83%83%E3%82%B0%E5%AE%9F%E8%A1%8C#.E3.83.87.E3.83.90.E3.83.83.E3.82.AC.E3.81.AB.E7.92.B0.E5.A2.83.E5.A4.89.E6.95.B0.E3.82.92.E6.B8.A1.E3.81.99)
``` json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File !!!",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "justMyCode": true,
            "env": {
                "PYTHONPATH": "/home/user/virtual_py3.10/lib/python3.10/site-packages"
            }
        }
    ]
}
```

### how to debug
Use a button on a left pane.  
Right button for debug is not considered for cutome configuration.
- [operation image](https://python-work.com/vscode-run/#i-5)