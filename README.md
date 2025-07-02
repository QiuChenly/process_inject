# macOS 远程进程注入

## 常规使用

```bash
process_inject <目标进程路径/目标进程名称> <本地dylib文件> [模式]
```

### Parameters:

- `<目标进程路径/目标进程名称>`: 可以是:
    - 目标进程PID
    - 进程名称 (使用 -name 模式)
    - 二进制程序路径 (使用 -spawn 模式)
- `<本地dylib文件>`: 本地dylib注入文件路径.
- `[模式]`: 选择不同的注入模式, 见下方.

### 模式:

- `-name`: 寻找并匹配当前Mac上名称完全匹配的进程名，并执行注入.
- `-spawn`: 自动启动目标指定进程并注入.

### 使用样例:

1. 直接注入PID为1234的进程:

```bash
process_inject 1234 "/path/to/library.dylib"
```

2. 注入一个已经运行的程序:

```bash
process_inject "Safari" "/path/to/library.dylib" -name
```

3. 自动拉起一个指定路径的新进程并注入 (spawn 模式):

```bash
process_inject "/Applications/Calculator.app/Contents/MacOS/Calculator" "/path/to/library.dylib" -spawn
```


**Note:** 如果需要注入一个root权限执行的程序，需要用sudo来获取root权限才可以操作.

修改目标进程需要持有一些特殊权限，请使用以下命令签名特殊权限。默认情况下CMake编译自动执行此命令。

> sudo codesign -f -s - --all-architectures --deep --entitlements "debug.entitlements" process_inject


## 引用参考资料

本仓库主要基于以下开发人员研究成果并略加修改。

1. 主要来源:

- <[https://github.com/notahacker8/MacInject](https://github.com/notahacker8/MacInject)>  

2. 增加arm64e修补:

- <[https://github.com/marlkiller/process_inject](https://github.com/marlkiller/process_inject)>  