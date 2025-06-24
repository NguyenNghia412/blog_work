---
layout: post
title:  "Cài flutter chạy với vscode ko dùng Android Studio"
date:   2024-06-17
categories: [Flutter, VSCode, VSC, Command line]
---

## Các bước
- [Các bước chủ yếu ở đây. Ấn vào đây để xem](https://www.youtube.com/watch?v=MruPYtw9Lk0&ab_channel=0xPanda){:target="_blank"}.

## Tạo máy ảo

- Download android về cho máy ảo. Đang cài theo v31 thì chọn 31 luôn
`sdkmanager "system-images;android-31;google_apis;x86_64"`

`sdkmanager "emulator"`


- Câu này tạo máy ảo
`avdmanager -s create avd -n pixel -k "system-images;android-29;google_apis;x86_64"`

- Hoặc là Create emulator using existing devices features
`avdmanager -s create avd -n pixel -k "system-images;android-29;google_apis;x86_64" -d 19`

- List máy ảo
`avdmanager list`

## Lệnh chạy

```
flutter emulators --launch Pixel
flutter run
```