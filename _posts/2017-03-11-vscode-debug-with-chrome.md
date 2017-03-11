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
