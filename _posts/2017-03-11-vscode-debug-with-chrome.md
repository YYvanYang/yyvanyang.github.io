---
layout: post
title:  "vscode debug with chrome"
categories: vscode
---

## 安装Debugger for Chrome插件
* [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)

## 添加配置文件
* 切换到调试窗口，“添加配置”，然后选择“chrome”，vscode将会创建一个launch.json配置文件

## 启动调试
1. 启动服务（如npm run dev）
2. 在代码中设置断点，F5启动调试

## From official sites notes
>### Other optional launch config fields
>* diagnosticLogging: When true, the adapter logs its own diagnostic info to the console, and to this file: ~/.vscode/extensions/msjsdiag.debugger-for-chrome/vscode-chrome-debug.txt. This is often useful info to include when filing an issue on GitHub.
>* runtimeExecutable: Workspace relative or absolute path to the runtime executable to be used. If not specified, Chrome will be used from the default install location
>* runtimeArgs: Optional arguments passed to the runtime executable
>* **userDataDir: Can be set to a temp directory, then Chrome will use that directory as the user profile directory. If Chrome is already running when you start debugging with a launch config, then the new instance won't start in remote debugging mode. If you don't want to close the original instance, you can set this property and the new instance will correctly be in remote debugging mode.**
>* url: Required for a 'launch' config. For an attach config, the debugger will search for a tab that has that URL. It can also contain wildcards, for example, "localhost:*/app" will match either "http://localhost:123/app" or "http://localhost:456/app", but not "http://stackoverflow.com".
>* sourceMaps: By default, the adapter will use sourcemaps and your original sources whenever possible. You can disable this by setting sourceMaps to false.
>* pathMapping: This property takes a mapping of URL paths to local paths, to give you more flexibility in how URLs are resolved to local files. "webRoot": "${workspaceRoot}" is just shorthand for a pathMapping like { "/": "${workspaceRoot}" }.



Issues:

```bash
Cannot connect to runtime process, timeout after 10000 ms - (reason: Cannot connect to the target: connect ECONNREFUSED 127.0.0.1:9222).
```
solution： 退出chrome, 或者杀死chrome进程，再按F5启动调试

例子：
https://github.com/Microsoft/vscode-chrome-debug/wiki/Examples


launch.json配置例子代码
```json
{
	"version": "0.2.0",
	"configurations": [{
			"name": "Launch localhost with sourcemaps",
			"type": "chrome",
			"request": "launch",
			"url": "http://localhost:3000",
			"sourceMaps": true,
			"webRoot": "${workspaceRoot}"
      // 使用该选项可以启用新的Chrome实例而不用事先退出之前已启动的chrome
			// Uncomment this to run easily alongside another running instance of Chrome
			// "userDataDir": "${workspaceRoot}/.vscode/chrome",

			// Uncomment this to get diagnostic logs in the console
			// "diagnosticLogging": true
		},
		{
			"name": "Attach with sourcemaps",
			"type": "chrome",
			"request": "attach",
			"port": 9222,
			"sourceMaps": true,
			"webRoot": "${workspaceRoot}"
		}
	]
}
```
