# 命令行 Terminal

摘要：Windows 终端是一个面向命令行工具和 shell（如命令提示符、PowerShell 和适用于 Linux 的 Windows 子系统 (WSL)）用户的新式终端应用程序。 它的主要功能包括多个选项卡、窗格、Unicode 和 UTF-8 字符支持、GPU 加速文本呈现引擎，你还可用它来创建你自己的主题并自定义文本、颜色、背景和快捷方式。
<!--more-->

# 命令行 Terminal
## 安装
```bash
$ git clone https://github.com/microsoft/Terminal.git
```

Windows应用商店：`Windows Terminal from the Microsoft Store`，搜索`Terminal`。
## 配置

### 1、全局配置
`Globals`：在配置文件最外侧大括号内的内容，会影响整个窗口的配置。如亮/暗主题，默认打开终端等。

### 2、环境配置
`Profiles`：profiles 内的内容，存放单个环境的配置。这里可以单独配置每个在 Windows Terminal 中打开的终端，如 PowerShell，CMD，Git Bash 等。

 `ssh`：输入以下信息，
```json
 "list":
 [
     {
         "guid": "{da0d1a9a-79de-403f-afe8-9f2353033a7b}",
         "name": "VPS1",
         "commandline": "powershell.exe ssh root@xxx.xxx.xxx.xxx"
     },
     ...
 ]
```

自定义背景图片：如下，

```json
 {
            "backgroundImage": "[[ Image Path ]]",
            "backgroundImageStretchMode": "uniformToFill",
            "backgroundImageOpacity": 0.6
 }
```

### 3、色彩配置
Schemes：schemes 内的内容，放置终端的主题色彩配置。这些配置可以在环境 Profiles 中被单独调用。
```json
  "schemes": [
    {
      "name": "Campbell",
      "foreground": "#CCCCCC",
      "background": "#0C0C0C",
      ...
```
```json
    "list": [
      {
        "colorScheme": "Campbell", //配色方案
        ...
```

### 4、快捷键配置
`Keybindings`：`Keybindings` 内的内容，适用于终端内的自定义快捷键配置。
```json
```json
// This file was initially generated by Windows Terminal 1.0.1811.0
// It should still be usable in newer versions, but newer versions might have additional
// settings, help text, or changes that you will not see unless you clear this file
// and let us generate a new one for you.

// To view the default settings, hold "alt" while clicking on the "Settings" button.
// For documentation on these settings, see: https://aka.ms/terminal-documentation
{
  "$schema": "https://aka.ms/terminal-profiles-schema",

  "defaultProfile": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}", //默认终端
  "alwaysShowTabs": true, //始终显示标签
  "initialCols": 100, //默认列数
  "initialRows": 30, //默认行数
  // You can add more global application settings here.
  // To learn more about global settings, visit https://aka.ms/terminal-global-settings

  // If enabled, selections are automatically copied to your clipboard.
  "copyOnSelect": false,
  // If enabled, formatted data is also copied to your clipboard
  "copyFormatting": false,
  "requestedTheme": "system", //主题，设为 light 亮色主题、dark 暗色主题、system 跟随系统。
  "showTabsInTitlebar": true, //在标题栏中显示终端窗口标签栏
  "showTerminalTitleInTitlebar": true, //在标签栏中显示终端标签
  // A profile specifies a command to execute paired with information about how it should look and feel.
  // Each one of them will appear in the 'New Tab' dropdown,
  //   and can be invoked from the commandline with `wt.exe -p xxx`
  // To learn more about profiles, visit https://aka.ms/terminal-profile-settings
  "profiles": {
    //环境配置包含两大块 defaults 和 lists ，前者为所有环境的默认配置，后者为单个环境的特殊配置。
    "defaults": {
      // Put settings here that you want to apply to all profiles.
      "hidden": false,
      "useAcrylic": true, //使用不透明度
      "acrylicOpacity": 0.65, //不透明度
      "closeOnExit": true, //退出后关闭
      "colorScheme": "Campbell", //配色方案
      "cursorColor": "#FFFFFF", //光标颜色
      // 光标类型，可选值 "vintage" ( ▃ ), "bar" ( ┃ ),"underscore" ( ▁ ), "filledBox" ( █ ), "emptyBox" ( ▯ )
      "cursorShape": "bar",
      "fontFace": "等距更纱黑体 SC", //字体
      "fontSize": 14,
      "historySize": 9001, //历史大小
      "padding": "0, 0, 0, 0",
      "snapOnInput": true, //嗅探输入
      "startingDirectory": "%Workspaces%", //初始目录
      "backgroundImage": "C:/Users/ly/Pictures/Wallpaper/wallhaven-ymojgd.jpg", // 背景图片
      "backgroundImageStretchMode": "uniformToFill", //背景图扩展模式
      "backgroundImageOpacity": 0.6 //背景图透明度
    },
    "list": [
      {
        // Make changes here to the powershell.exe profile.
        "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
        "name": "Windows PowerShell",
        "commandline": "powershell.exe"
      },
      {
        // Make changes here to the cmd.exe profile.
        "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
        "name": "cmd",
        "commandline": "cmd.exe"
      },
      {
        "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
        "hidden": false,
        "name": "Azure Cloud Shell",
        "source": "Windows.Terminal.Azure"
      },
      {
        // VPS
        "guid": "{da0d1a9a-79de-403f-afe8-9f2353033a7b}",
        "name": "ChangHongVPS",
        "commandline": "powershell.exe ssh 20323597@10.4.33.55"
      }
    ]
  },

  // Add custom color schemes to this array.
  // To learn more about color schemes, visit https://aka.ms/terminal-color-schemes
  "schemes": [
    {
      "name": "Campbell",
      "foreground": "#CCCCCC",
      "background": "#0C0C0C",
      "black": "#0C0C0C",
      "blue": "#0037DA",
      "brightBlack": "#767676",
      "brightBlue": "#3B78FF",
      "brightCyan": "#61D6D6",
      "brightGreen": "#16C60C",
      "brightPurple": "#B4009E",
      "brightRed": "#E74856",
      "brightWhite": "#F2F2F2",
      "brightYellow": "#F9F1A5",
      "cyan": "#3A96DD",
      "green": "#13A10E",
      "purple": "#881798",
      "red": "#C50F1F",
      "white": "#CCCCCC",
      "yellow": "#C19C00"
    },
    {
      "name": "UbuntuLegit",
      "foreground": "#EEEEEE",
      "background": "#2C001E",
      "black": "#4E9A06",
      "blue": "#3465A4",
      "brightBlack": "#555753",
      "brightBlue": "#729FCF",
      "brightCyan": "#34E2E2",
      "brightGreen": "#8AE234",
      "brightPurple": "#AD7FA8",
      "brightRed": "#EF2929",
      "brightWhite": "#EEEEEE",
      "brightYellow": "#FCE94F",
      "cyan": "#06989A",
      "foreground": "#EEEEEE",
      "green": "#300A24",
      "name": "UbuntuLegit",
      "purple": "#75507B",
      "red": "#CC0000",
      "white": "#D3D7CF",
      "yellow": "#C4A000"
    },
    {
      "name": "Dracula",
      "foreground": "#F8F8F2",
      "background": "#282A36",
      "black": "#21222C",
      "blue": "#F1FA8C",
      "brightBlack": "#6272A4",
      "brightBlue": "#D6ACFF",
      "brightCyan": "#A4FFFF",
      "brightGreen": "#69FF94",
      "brightPurple": "#FF92DF",
      "brightRed": "#FF6E6E",
      "brightWhite": "#FFFFFF",
      "brightYellow": "#FFFFA5",
      "cyan": "#8BE9FD",
      "green": "#50FA7B",
      "purple": "#BD93F9",
      "red": "#FF5555",
      "white": "#F8F8F2",
      "yellow": "#F1FA8C"
    },
    {
      "name": "One Half Dark",
      "foreground": "#DCDFE4",
      "background": "#282C34",
      "black": "#282C34",
      "blue": "#61AFEF",
      "brightBlack": "#5A6374",
      "brightBlue": "#61AFEF",
      "brightCyan": "#56B6C2",
      "brightGreen": "#98C379",
      "brightPurple": "#C678DD",
      "brightRed": "#E06C75",
      "brightWhite": "#DCDFE4",
      "brightYellow": "#E5C07B",
      "cyan": "#56B6C2",
      "green": "#98C379",
      "purple": "#C678DD",
      "red": "#E06C75",
      "white": "#DCDFE4",
      "yellow": "#E5C07B"
    },
    {
      "name": "Atom",
      "foreground": "#c5c8c6",
      "background": "#161719",
      "black": "#000000",
      "blue": "#85befd",
      "brightBlack": "#000000",
      "brightRed": "#fd5ff1",
      "brightGreen": "#94fa36",
      "brightYellow": "#f5ffa8",
      "brightBlue": "#96cbfe",
      "brightPurple": "#b9b6fc",
      "brightCyan": "#85befd",
      "brightWhite": "#e0e0e0",
      "cyan": "#85befd",
      "green": "#87c38a",
      "purple": "#b9b6fc",
      "red": "#fd5ff1",
      "white": "#e0e0e0",
      "yellow": "#ffd7b1"
    },
    {
      "name": "Git Flat UI",
      "foreground": "#DCDFE4",
      "background": "#1A1A1A",
      "black": "#282C34",
      "blue": "#61AFEF",
      "brightBlack": "#5A6374",
      "brightBlue": "#61AFEF",
      "brightCyan": "#56B6C2",
      "brightGreen": "#19A33A",
      "brightPurple": "#C678DD",
      "brightRed": "#E06C75",
      "brightWhite": "#DCDFE4",
      "brightYellow": "#E5C07B",
      "cyan": "#56B6C2",
      "green": "#19A33A",
      "purple": "#C678DD",
      "red": "#E06C75",
      "white": "#DCDFE4",
      "yellow": "#E5C07B"
    }
  ],

  // Add custom keybindings to this array.
  // To unbind a key combination from your defaults.json, set the command to "unbound".
  // To learn more about keybindings, visit https://aka.ms/terminal-keybindings
    "actions":
    [
        // Copy and paste are bound to Ctrl+Shift+C and Ctrl+Shift+V in your defaults.json.
        // These two lines additionally bind them to Ctrl+C and Ctrl+V.
        // To learn more about selection, visit https://aka.ms/terminal-selection
        { "command": {"action": "copy", "singleLine": false }, "keys": "ctrl+c" },
        { "command": "paste", "keys": "ctrl+v" },

        // Press Ctrl+Shift+F to open the search box
        //{ "command": "find", "keys": "ctrl+shift+f" },
        { "command": "find", "keys": "ctrl+f" },

        // Press Alt+Shift+D to open a new pane.
        // - "split": "auto" makes this pane open in the direction that provides the most surface area.
        // - "splitMode": "duplicate" makes the new pane use the focused pane's profile.
        // To learn more about panes, visit https://aka.ms/terminal-panes
        { "command": { "action": "splitPane", "split": "auto", "splitMode": "duplicate" }, "keys": "alt+shift+d" }

    ]
}
```

## 快捷鍵
```
Ctrl+d/exit 退出当前Termina1
Ctrl+l/clear 清除屏幕
ctrl+shift+w 关闭
Alt+Shift+=  左右分屏
Alt+Shift+-  上下分屏
Ctrl+Shift+t 打开新标签页
Alt + 方向键 光标移动
Ctrl + Shift + c	复制
Ctrl + Shift + v	粘贴
Shift + Insert      粘贴
F11	全屏切换
Ctrl + Win + 上下键 切换console控制台的最大化
Ctrl + Shift + PageUp/PageeDown 翻页
按住Ctrl + 鼠标滚轮 调整窗口显示大小
```

## 快捷打开Terminal
1. 微软 GitHub 地址下载 Windows Terminal 的图标到本地，[link](https://raw.githubusercontent.com/microsoft/terminal/master/res/terminal.ico) 。
2. 然后新建一个 `.reg` 的注册表文件，名字任意、位置任意，并填入：
```
 Windows Registry Editor Version 5.00

 [HKEY_CLASSES_ROOT\Directory\Background\shell\wt]
 @="Windows Terminal here"
 "Icon"="[[ 图标绝对位置 ]]"
 
 [HKEY_CLASSES_ROOT\Directory\Background\shell\wt\command]
 @="C:\\Users\\[[ 用户名 ]]\\AppData\\Local\\Microsoft\\WindowsApps\\wt.exe"
```

3.  Quicker 右键效率软件。

## 参考

 - [01-如何打造好看还好用的 Windows Terminal-ChrAlpha-知乎](https://zhuanlan.zhihu.com/p/133297347)
