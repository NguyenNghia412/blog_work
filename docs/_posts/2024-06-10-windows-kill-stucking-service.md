---
layout: post
title:  "Tiêu diệt service bị kẹt ở status starting hoặc stopping"
date:   2024-06-10
categories: [Windows, Services, Cmd, Taskkill]
---

Cứ không tắt được service theo cách bình thường thì dùng cách sau

## Thao tác

Mở cmd 

`sc queryex metricbeat`

![]({{ site.baseurl }}/assets/images/sc-queryex-service.png)

Copy pid vào **5188** của câu dưới là oke

`taskkill /PID 5188 /F`