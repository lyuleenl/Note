# Win

## 如何快速格式化硬盘 `Win`

1. 打开cmd
2. 输入diskpart
3.  ![list disk](https://img2020.cnblogs.com/blog/2130168/202011/2130168-20201104185107275-2099770035.png)

# Mac



## 显示隐藏文件
### 快捷键

cmd+shift+.

### 命令行

//显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool true
//不显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool false

需要重启Finder：窗口左上角的苹果标志-->强制退出-->Finder-->重新启动

## 显示文件路径
使用终端命令行显示完整路径
```
defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE;killall Finder
```
隐藏完整路径
```
defaults delete com.apple.finder _FXShowPosixPathInTitle;killall Finder
```

## 程序坞放大效果

程序坞设置 勾选放大

## 键盘映射

键盘->修饰键  option和command互换

## 触发角

系统偏好设定——> Mission Control——> 触发脚...——> 在右下角选择“桌面” ——> 搞定。

## 自动挂载EFI文件

使用命令`diskutil list`找到EFI所在的分区

![image.png](https://i.loli.net/2020/11/24/FlZaNjwgGMW3Oms.png)

获取EFI分区UUID`sudo diskutil info disk0s1 | grep 'Partition UUID'`

![diskutil info](https://i.loli.net/2020/11/24/xLbvwNAD9hMd5T1.png)

F9399EFF-E7E7-42CB-A352-A285E6FC61FA

打开系统自带的自动操作程序，依次点击应用程序 -> 选取 -> 运行 shell 脚本
```
#!/bin/bash

mountEFI=$(echo '你的密码' | sudo -S diskutil info 你的UUID | grep 'Device Node')
echo '你的密码' | sudo -S diskutil mount '/'${mountEFI#*/}
open /Volumes/EFI/EFI/OC
```
将以上代码写入，保存即可
![image.png](https://i.loli.net/2020/11/24/UTRn57Et3i4brLp.png)

参考：https://www.bugprogrammer.me/2019/12/03/mountEFI.html