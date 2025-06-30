---
layout: post
title:  "Chạy app flask api dưới dạng Windows Service rồi public qua IIS"
date:   2025-06-24
categories: [Python, Flask, IIS, Windows Server, nssm]
---

## Lưu ý

- Ưu: Dễ thực hiện

- Nhược: Port trên windows service và port iis không được trùng nhau

- [Link tham khảo](https://nagasudhir.blogspot.com/2022/09/run-python-flask-server-as-windows.html){:target="_blank"}

## Cách làm

### Tổng quan

- Tạo 1 file bat để gọi tới flask app (vd tên file là app.py)

- Tạo file bat cài flask app dưới dạng service

- Tạo reverse proxy trên IIS trỏ về Windows Service

### Tạo file bat gọi flask app

- Cài đặt python

- Tạo 1 file bất kỳ có đuôi `.bat` (VD: `run_server.bat`)

- Nội dung: 

```
python app.py
```

### Tạo file bat cài flask app dạng windows service

#### Cài nssm

- [Tải nssm tại đây](https://nssm.cc/download){:target="_blank"}

- Chọn x64 để cài đặt

- Cài xong thì thêm đường dẫn tới nssm vào file path

- Chạy thử `nssm` trên cmd để kiểm tra đã cài ok chưa

#### Tạo file bat cài service

- Tạo file bất kỳ đuôi `.bat` gọi tới file bat tạo ở bước trên (VD: `install_service.bat`)

- *Lưu ý: Trong bài viết thì các file đều nằm ở `C:\PUBLISH\API_TEST`*

- Tạo thư mục logs để chứa log của service

- Nội dung:

```
call nssm.exe install my_flask_app "C:\PUBLISH\API_TEST\run_server.bat"
call nssm.exe set my_flask_app AppStdout "C:\PUBLISH\API_TEST\logs\my_flask_app_logs.log"
call nssm.exe set my_flask_app AppStderr "C:\PUBLISH\API_TEST\logs\my_flask_app_logs.log"
call nssm set my_flask_app AppRotateFiles 1
call nssm set my_flask_app AppRotateOnline 1
call nssm set my_flask_app AppRotateSeconds 86400
call nssm set my_flask_app AppRotateBytes 1048576
call sc start my_flask_app
```

#### Tạo thêm file bat gỡ service

- Nội dung:

```
sc delete my_flask_app
```

#### Cài service

- Mở cmd, chạy file `install_service.bat`

- Cài xong => Windows + R => Gõ `services.msc` => Enter. Tìm service tên **my_flask_app** xem trạng thái

- Ok thì mở trình duyệt lên check url 

### Các lệnh phụ

- Xóa service: `sc delete my_flask_app`

- Chạy giao diện sửa cấu hình nssm với service: `nssm edit my_flask_app`

