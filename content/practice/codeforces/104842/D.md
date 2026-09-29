---
title: "CF 104842D - Số nguyên tố sâu"
description: "Chúng ta đang xem xét các số nguyên được viết ở dạng thập phân, nhưng ràng buộc khóa không chỉ ở giá trị số của chúng. Mỗi số được hiểu là một chuỗi và mọi khối chữ số liền kề bên trong chuỗi đó sẽ được chuyển trở lại thành số nguyên bằng cách loại bỏ các số 0 đứng đầu."
date: "2026-06-28T11:31:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104842
codeforces_index: "D"
codeforces_contest_name: "2020-2021 ICPC, Moscow Subregional"
rating: 0
weight: 104842
solve_time_s: 29
verified: false
draft: false
---

[CF 104842D - Số nguyên tố sâu](https://codeforces.com/problemset/problem/104842/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem xét các số nguyên được viết ở dạng thập phân, nhưng ràng buộc khóa không chỉ ở giá trị số của chúng. Mỗi số được hiểu là một chuỗi và mọi khối chữ số liền kề bên trong chuỗi đó sẽ được chuyển trở lại thành số nguyên bằng cách loại bỏ các số 0 đứng đầu. Những số nguyên dẫn xuất đó được yêu cầu phải thỏa mãn một điều kiện mạnh. 

Một số được gọi là hợp lệ nếu nó là số nguyên tố và ngoài ra, mọi chuỗi con biểu diễn số thập phân của nó đều tương ứng với một số nguyên tố sau khi chuyển đổi. Nhiệm vụ là đếm xem có bao nhiêu số như vậy nằm trong một khoảng nhất định$[n, m]$, trong đó cả hai điểm cuối có thể lớn bằng$10^{18}$. 

Ràng buộc ngay lập tức ngụ ý rằng việc kiểm tra vũ lực trên mỗi số là không thể. Ngay cả việc kiểm tra tính nguyên tố cũng đã tốn kém ở quy mô này và khoảng thời gian có thể chứa tới$10^{18}$ứng viên trong trường hợp xấu nhất. Thay vào đó, bất kỳ giải pháp nào cũng phải tạo trực tiếp tất cả các số hợp lệ. 

Vấn đề tế nhị đầu tiên xuất phát từ các chuỗi con có độ dài bằng một. Bản thân mỗi chữ số phải đại diện cho một số nguyên tố.
