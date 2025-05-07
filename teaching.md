# 教程

## 目录
1. [ADB（Android Debug Bridge）概述](#adbandroid-debug-bridge概述)
2. [Control Hub & Expansion Hub接口概述](#control-hub--expansion-hub接口概述)
3. [WiFi Direct](#wifi-direct)

---

## 1. ADB（Android Debug Bridge）概述

### a) 简介
ADB 是 Android SDK 中的命令行工具，用于与 Android 设备通信并执行调试功能，包含以下组件：
- **客户端**：通过命令行调用的程序  
- **服务器**：管理客户端与设备间的通信  
- **守护进程 (adbd)**：运行在设备上执行命令  

### b) 安装 & 启动
1. 下载 Android SDK Platform Tools：
   - 前往 [https://dl.google.com/android/repository/platform-tools-latest-windows.zip](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) 下载。
2. 配置环境变量：
   - 将解压后的 `platform-tools` 目录添加到系统 `PATH` 中。
3. 设备端配置：
   - 在 Android 设备上启用开发者模式，并打开 USB 调试。

### c) 用法

#### i. 设备管理
```bash
# 列出连接的设备
adb devices

# 连接无线设备
adb connect <IP:端口>

# 断开设备
adb disconnect
```

#### ii. 应用管理
```bash
# 安装 APK
adb install [选项] <本地路径.apk>
# 示例：-r 覆盖安装，-s 安装到 SD 卡

# 卸载应用
adb uninstall <包名>

# 启动应用
adb shell am start -n <包名/活动名>
```

#### iii. 文件操作
```bash
# 推送文件到设备
adb push <本地路径> <设备路径>

# 从设备拉取文件
adb pull <设备路径> <本地路径>
```

#### iv. 调试 & 日志
```bash
# 查看设备日志（支持过滤）
adb logcat [-s TAG] [-c 清空日志]

# 进入设备 Shell
adb shell
```

#### v. 系统操作
```bash
# 重启设备（可选 recovery/bootloader 模式）
adb reboot [选项]

# 截图
adb shell screencap <设备文件路径>

# 录屏
adb shell screenrecord <设备文件路径>

# 备份与恢复
adb backup -apk -shared -all -f backup.ab
adb restore backup.ab
```

#### vi. 权限管理
```bash
# 获取 root 权限
adb root

# 挂载系统分区为可写
adb remount
```

#### vii. Shell 子命令
```bash
# 包管理
pm list packages
pm path <包名>

# 强制停止应用
am force-stop <包名>
```

#### viii. 其他
```bash
# 启用无线 ADB
adb tcpip <端口>

# 查看帮助
adb -help
```

### d) 练习
> **问题**：已通过有线连接 Android 设备，如何启用无线 ADB 连接？  
> **答案**：  
> 1. `adb tcpip 5555`  
> 2. `adb connect <Android设备IP>:5555`  

---

## 2. Control Hub & Expansion Hub接口概述

### a) UART
- **特点**：
  - 异步通信（无时钟信号，依赖波特率）
  - 点对点直连（仅支持双设备）
  - 全双工（TX/RX 独立）
- **常见应用**：
  - 网络芯片与处理器通信
  - 传感器数据读取
  - 开发板调试

### b) RS485
- UART 的物理层增强版，支持长距离通信和多设备总线。

### c) I²C（Inter-Integrated Circuit）
- **特点**：
  - 半双工
  - 同步通信（时钟线 SCL）
  - 两线制（SDA/SCL）
  - 低速（100kbps ~ 3.4Mbps）
- **常见应用**：
  - EEPROM、温度传感器、RTC 时钟、IMU

### d) SPI（Serial Peripheral Interface）
- **特点**：
  - 同步通信
  - 全双工
  - 四线制（SCK, MOSI, MISO, SS/CS）
  - 高速（可达 10Mbps+）
- **常见应用**：
  - SD 卡、显示屏、高速外设

### e) 舵机接口
- **引脚定义**：
  - 信号线（白色/黄色）
  - 电源（红色/橙色）
  - GND（黑色/褐色）

### f) 电机接口
- **功能**：
  - 电源输入
  - 编码器（Encoder）连接

### g) 电源口（XT30）
- **说明**：
  - Control Hub 和 Expansion Hub 各有一个公口和母口。
  - 支持 12V 电池（3S，即3个3.7V电池串联，标称电压3.7*3V，满电电压4.2*3V）。
  - 使用大田宫接口转接线连接。

### h) MINI USB
- **用途**：
  - 连接 Expansion Hub 至 Control Hub。
  - 在 REV Hardware Client 中始终识别为 Expansion Hub。

### i) USB OTG & HOST
- **区别**：
  - **USB HOST**：作为主设备接入外设（如键盘、鼠标）。
  - **USB OTG**：动态切换主从模式（如连接电脑传输文件）。

### j) HDMI
- 标准视频输出接口，用于连接显示器。

### k) SD Card
- 支持 TF 卡扩展存储或系统文件传输。

---

## 3. WiFi Direct

### a) 特性
1. **无需路由器**：设备直连（Peer-to-Peer）。
2. **高速通信**：与普通 WiFi 速度相同。
3. **跨平台支持**：兼容不同操作系统。
4. **灵活组网**：支持 P2P 和 Group 模式。

---

## 4. Git版本管理

### **1. Git 简介**
Git 是一个**分布式版本控制系统**，用于跟踪文件变化（如代码、文档），支持多人协作开发。核心功能包括：
- **版本管理**：记录每次修改，可回溯到任意历史版本。
- **分支管理**：创建独立分支开发新功能，不影响主代码。
- **协作开发**：多人共享代码仓库，合并修改。

### **2. 安装 Git**
- **Windows**：下载 [Git for Windows](https://git-scm.com/download/win)，默认选项安装。
- **Mac**：使用 Homebrew：`brew install git` 或下载 [Git for Mac](https://git-scm.com/download/mac)。
- **Linux**：使用包管理器安装，如 Ubuntu：`sudo apt-get install git`。

安装后验证：
```bash
git --version
```

### **3. 初始配置**
配置用户名和邮箱（用于提交记录）：
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### **4. 基础操作**
#### **4.1 创建仓库**
```bash
# 初始化新仓库
git init

# 克隆远程仓库（如 GitHub 项目）
git clone https://github.com/user/repo.git
```

#### **4.2 提交更改**
```bash
# 查看当前文件状态
git status

# 添加文件到暂存区
git add filename          # 添加单个文件
git add .                 # 添加所有修改

# 提交到本地仓库
git commit -m "提交说明"
```

#### **4.3 查看历史记录**
```bash
git log                   # 查看完整提交历史
git log --oneline         # 简洁模式
```

### **5. 分支管理**
#### **5.1 创建与切换分支**
```bash
git branch                  # 查看所有分支
git branch new-branch       # 创建新分支
git checkout new-branch    # 切换到新分支
git switch new-branch       # 同上（Git 2.23+）

# 创建并切换分支（简写）
git checkout -b new-branch
```

#### **5.2 合并分支**
```bash
git checkout main          # 切换到主分支
git merge new-branch       # 将 new-branch 合并到当前分支
```

#### **5.3 解决冲突**
当合并冲突时，手动编辑文件中的 `<<<<<<<` 标记，解决后提交：
```bash
git add resolved-file.txt
git commit -m "解决合并冲突"
```

### **6. 远程仓库（以 GitHub 为例）**
#### **6.1 关联远程仓库**
```bash
# 添加远程仓库（命名为 origin）
git remote add origin https://github.com/user/repo.git

# 查看远程仓库列表
git remote -v
```

#### **6.2 推送与拉取**
```bash
# 推送本地分支到远程仓库
git push origin main

# 拉取远程仓库最新代码
git pull origin main
```

#### **6.3 从远程仓库更新本地**
```bash
# 获取远程分支更新（不自动合并）
git fetch origin

# 拉取并合并到当前分支
git pull origin main
```

### **7. 撤销操作**
#### **7.1 撤销未提交的修改**
```bash
git checkout -- filename    # 丢弃某个文件的修改
git restore filename        # Git 2.23+ 新语法
```

#### **7.2 撤销提交**
```bash
# 撤销上一次提交，保留修改
git reset --soft HEAD~1

# 彻底删除上一次提交（谨慎使用！）
git reset --hard HEAD~1
```

### **8. 高级技巧**
#### **8.1 储藏修改（Stash）**
临时保存未提交的修改：
```bash
git stash           # 储藏当前修改
git stash pop       # 恢复最近储藏的修改
```

#### **8.2 标签（Tag）**
标记重要版本：
```bash
git tag v1.0.0               # 创建标签
git push origin v1.0.0       # 推送标签到远程
```

#### **8.3 忽略文件（.gitignore）**
创建 `.gitignore` 文件，列出需忽略的文件/文件夹：
```
# 示例
node_modules/
*.log
.DS_Store
```

### **9. 图形化工具（可选）**
- **GitKraken**：跨平台 Git 图形客户端。
- **Sourcetree**：免费的 Git 和 Mercurial 客户端。
- **VS Code 内置 Git 工具**：编辑器直接操作。

### **10. 学习资源**
- **官方文档**：[Git Book](https://git-scm.com/book/)
- **交互式练习**：[Learn Git Branching](https://learngitbranching.js.org)
- **GitHub Guides**：[GitHub 指南](https://guides.github.com/)