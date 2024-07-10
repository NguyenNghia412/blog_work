---
layout: post
title: "Build file exe từ Python QT bằng cx_freeze"
date:   2024-07-10
categories: [Windows, Python, QT, exe, cx_freeze]
---

## Tham khảo

- [HD build trên trang chủ qt](https://doc.qt.io/qtforpython-6/deployment/deployment-cxfreeze.html#preparation){:target="_blank"}.

- [HD ẩn cmd khi click file exe](https://stackoverflow.com/questions/2880316/how-can-i-hide-the-console-window-when-freezing-wxpython-applications-with-cxfre){:target="_blank"}.

## Chuẩn bị

### Cài cx_freeze để build exe

`pip install cx_Freeze`

## Build exe

### TH Build app GUI 

- Tạo file **setup.py** ở ngoài cùng dự án có nội dung

```
import sys
from cx_Freeze import setup, Executable

base = None
if sys.platform == "win32":
    base = "Win32GUI"

setup(name = "Demo",
      version = "0.1",
      description = "Demo App",
      executables = [Executable("main.py", base = base)])
```

- Chạy lệnh build

`python setup.py build`

=> XONG. File **main.exe** nằm trong thư mục **build** vừa được sinh ra

### TH Build app console

- Chạy lệnh build

`cxfreeze .\main.py --target-dir dist`

=> XONG. File **main.exe** nằm trong thư mục **dist** vừa được sinh ra