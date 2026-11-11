# Python 项目封装：源码 → EXE → 安装程序

本文档介绍如何使用 **PyInstaller + Inno Setup** 将 Python
项目封装成独立的 Windows EXE 程序，并进一步制作 Windows 安装包。

## 1. 项目目录结构

``` text
YourProject/
│
├── .venv/                  # Python 虚拟环境
├── src/                    # 项目源代码
│   └── main.py
├── assets/                 # 项目资源文件
│   └── app.ico             # 应用程序图标
├── build/                  # 构建临时目录
├── dist/                   # EXE 输出目录
├── YourProject.spec        # PyInstaller 配置文件
├── YourProject.iss         # Inno Setup 安装脚本
└── README.md
```

## 2. 确认 Python 环境

检查虚拟环境中的 Python 版本：

``` powershell
.\\.venv\\Scripts\\python.exe --version
```

确认应用程序图标文件是否存在：

``` powershell
Test-Path .\\assets\\app.ico
```

创建构建目录：

``` powershell
New-Item -ItemType Directory -Force -Path .\\build
```

## 3. 生成应用程序图标（可选）

如果还没有 ICO 图标，可以使用 Pillow 生成。

安装 Pillow：

``` powershell
.\\.venv\\Scripts\\python.exe -m pip install Pillow
```

示例：

``` powershell
@'
from PIL import Image, ImageDraw

size = 256
img = Image.new("RGBA", (size, size), (0, 0, 0, 0))
draw = ImageDraw.Draw(img)

# 在这里绘制自己的应用程序图标
draw.rounded_rectangle(
    (16, 16, 240, 240),
    radius=40,
    fill=(30, 125, 205)
)

# 保存 ICO 文件
img.save(
    "assets/app.ico",
    format="ICO",
    sizes=[
        (16, 16),
        (32, 32),
        (48, 48),
        (64, 64),
        (128, 128),
        (256, 256)
    ]
)
'@ | .\\.venv\\Scripts\\python.exe -
```

## 4. 安装 PyInstaller

``` powershell
.\\.venv\\Scripts\\python.exe -m pip install pyinstaller
```

检查安装版本：

``` powershell
.\\.venv\\Scripts\\python.exe -m PyInstaller --version
```

## 5. 打包 EXE 程序

使用 PyInstaller 配置文件进行构建：

``` powershell
.\\.venv\\Scripts\\python.exe -m PyInstaller --clean --noconfirm .\\YourProject.spec
```

参数说明：

  参数                 作用
  -------------------- ------------------------
  `--clean`            清理临时构建缓存
  `--noconfirm`        不询问是否覆盖已有输出
  `YourProject.spec`   PyInstaller 配置文件

构建完成后，通常可以在 `dist/` 目录中找到生成的程序。

## 6. 检查 EXE 程序

确认 EXE 文件是否存在：

``` powershell
Test-Path .\\dist\\YourProject.exe
```

运行程序进行测试：

``` powershell
Start-Process .\\dist\\YourProject.exe
```

确认程序能够正常启动后，再进行安装包制作。

## 7. 确认 Inno Setup 环境

检查 Inno Setup 编译器是否存在：

``` powershell
Test-Path "C:\\Program Files\\Inno Setup 7\\ISCC.exe"
```

如果没有安装，需要先安装 Inno Setup。

## 8. 编译 Windows 安装程序

使用 Inno Setup 编译器编译安装脚本：

``` powershell
& "C:\\Program Files\\Inno Setup 7\\ISCC.exe" ".\\YourProject.iss"
```

编译完成后，根据 `.iss` 脚本中指定的输出目录查找安装程序。

## 9. 完整构建流程

``` text
Python Source Code
       │
       ▼
Python Virtual Environment
       │
       ▼
PyInstaller
       │
       ▼
YourProject.exe
       │
       ▼
EXE Runtime Testing
       │
       ▼
Inno Setup
       │
       ▼
Windows Installer (.exe)
```

## 10. 常用命令汇总

``` powershell
# 1. 检查 Python 环境
.\\.venv\\Scripts\\python.exe --version

# 2. 检查图标文件
Test-Path .\\assets\\app.ico

# 3. 创建构建目录
New-Item -ItemType Directory -Force -Path .\\build

# 4. 安装 PyInstaller
.\\.venv\\Scripts\\python.exe -m pip install pyinstaller

# 5. 检查 PyInstaller 版本
.\\.venv\\Scripts\\python.exe -m PyInstaller --version

# 6. 打包 EXE
.\\.venv\\Scripts\\python.exe -m PyInstaller --clean --noconfirm .\\YourProject.spec

# 7. 检查 EXE
Test-Path .\\dist\\YourProject.exe

# 8. 运行 EXE
Start-Process .\\dist\\YourProject.exe

# 9. 检查 Inno Setup
Test-Path "C:\\Program Files\\Inno Setup 7\\ISCC.exe"

# 10. 编译安装程序
& "C:\\Program Files\\Inno Setup 7\\ISCC.exe" ".\\YourProject.iss"
```

------------------------------------------------------------------------

## Notes

-   将 `YourProject` 替换为实际项目名称。
-   根据实际项目修改 `.spec` 和 `.iss` 文件。
-   确保虚拟环境、图标文件和构建配置文件存在。
-   Inno Setup 的安装路径可能因版本和安装方式不同而变化。
-   本文档中的目录结构为通用示例，实际项目可以根据需要调整。
