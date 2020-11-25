# VScode使用教程

### vs code整段右移或者左移

选中按TAB右移，按SHIFT+TAB左移

## 在VScode中使用C#开发
**环境**
- VSC本体
- .NET Core SDK，可以访问dot.net下载
- VSC扩展：C#、Code Runner

**操作步骤**
- 建立一个文件夹
- 打开终端（TERMINAL） 输入``` dotnet new console``` 会自动生成Hello World文件
- 按F5即可运行 
- 还需要做一点修改。左边点开.vscode文件夹：
```
launch.json："console": "integratedTerminal", "internalConsoleOptions": "neverOpen";  .NET Core Attach可以删掉。
tasks.json：给build加上"group": { "kind": "build", "isDefault": true },（打group就>会自动提示）
```

如使用runner 需要设置环境变量
```
csc是编译C#的程序。安装VS，把 安装路径下\Microsoft VisualStudio
\2019\Community\MSBuild\Current\Bin\Roslyn\加到Path里。
```

## VS code 同步设置和扩展插件
https://www.jianshu.com/p/0a273bf2a986
vscode_plugin token ebc81cc8b0dac29cdbc8e98d5eef49094f5d7e5c
gist :e813379a55dab2b9a6d2bc13e1bbf8f0



int是带符号的，表示范围是：-2147483648到2147483648，即-2^31到2^31次方。

uint则是不带符号的，表示范围是：2^32即0到4294967295。