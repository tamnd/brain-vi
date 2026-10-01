---
title: "CF 104871H - Nhân sự"
description: "Chúng ta được cung cấp một hệ thống phân cấp công ty tạo thành một cây có gốc. Mỗi nhân viên ngoại trừ một người đều có chính xác một người quản lý và mỗi người quản lý có một danh sách theo thứ tự các báo cáo trực tiếp được xếp hạng từ được ưu tiên nhất đến ít được ưu tiên nhất."
date: "2026-06-28T10:38:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104871
codeforces_index: "H"
codeforces_contest_name: "2023-2024 ICPC Central Europe Regional Contest (CERC 23)"
rating: 0
weight: 104871
solve_time_s: 39
verified: false
draft: false
---

[CF 104871H - Nhân sự](https://codeforces.com/problemset/problem/104871/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống phân cấp công ty tạo thành một cây có gốc. Mỗi nhân viên ngoại trừ một người đều có chính xác một người quản lý và mỗi người quản lý có một danh sách theo thứ tự các báo cáo trực tiếp được xếp hạng từ được ưu tiên nhất đến ít được ưu tiên nhất. Đầu vào mô tả cây này ở dạng văn bản có cấu trúc (chế độ ENCODE) hoặc dưới dạng chuỗi nhị phân nhỏ gọn cộng với danh sách tên nhân viên (chế độ DECODE). 

Trong chế độ ENCODE, nhiệm vụ là nén toàn bộ hệ thống phân cấp thành hai phần. Đầu tiên, chúng ta phải xuất tất cả tên nhân viên theo thứ tự bất kỳ. Thứ hai, chúng ta phải tạo ra một chuỗi nhị phân mã hóa toàn bộ cấu trúc cây, bao gồm cả mối quan hệ cha-con và thứ tự các con cho mỗi người quản lý. 

Ở chế độ DECODE, chúng ta được cung cấp chính xác kết quả đầu ra đó: danh sách nam không có thứ tự
