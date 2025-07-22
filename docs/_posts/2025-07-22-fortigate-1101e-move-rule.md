---
layout: post
title:  "Fortigate 1101e: Di chuyển vị trí rule trong SD WAN Rle"
date:   2025-07-22
categories: [Firewall, Fortigate, SD WAN, Rule, Cli]
---

## Hoàn cảnh

- Cần định nghĩa để server đi ra internet theo 1 đường WAN duy nhất trong nhiều đường

- Rule tạo sau sẽ nằm ở dưới, bị ưu tiên thấp hơn nên muốn kéo lên trên. Trên WEB GUI không di chuyển được, phải dùng cli

## Thực hiện

- Bật cli lên rồi gõ lệnh. 3 lệnh đầu để xem id của rule. 20 và 19 trong câu lệnh cuối là id của rule.

```
config system sdwan
config service 
show
move 20 after 19
```