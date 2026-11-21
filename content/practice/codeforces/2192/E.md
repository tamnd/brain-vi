---
title: "CF 2192E - Hoán đổi để sắp xếp lại"
description: "Mỗi vị trí i chứa một cặp giá trị (ai, bi). Đối với vị trí đó, chúng tôi có chính xác hai sự lựa chọn. Nếu chúng ta không làm gì thì giá trị ai vẫn ở mảng a và bi vẫn ở mảng b. Nếu chúng ta hoán đổi vị trí đó, giá trị bi sẽ chuyển sang mảng a và ai sẽ chuyển sang mảng b."
date: "2026-06-07T20:58:56+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graph-matchings", "graphs", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2192
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1081 (Div. 2)"
rating: 2100
weight: 2192
solve_time_s: 152
verified: false
draft: false
---

[CF 2192E - Hoán đổi để sắp xếp lại](https://codeforces.com/problemset/problem/2192/E) 

**Xếp hạng:** 2100 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, so khớp đồ thị, đồ thị, tham lam, chuỗi 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi vị trí`i`chứa một cặp giá trị`(a_i, b_i)`. Đối với vị trí đó, chúng tôi có chính xác hai sự lựa chọn. 

Nếu chúng ta không làm gì, giá trị`a_i`vẫn ở trong mảng`a`Và`b_i`vẫn ở trong mảng`b`. 

Nếu chúng ta hoán đổi vị trí đó, giá trị`b_i`di chuyển đến mảng`a`Và`a_i`di chuyển đến mảng`b`. 

Sau khi thực hiện những lựa chọn độc lập này, chúng tôi muốn có nhiều giá trị trong`a`hoàn toàn giống với tập hợp nhiều giá trị trong`b`. 

Kích thước của một thử nghiệm có thể đạt tới một triệu phần tử trong tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào so sánh các vị trí với nhau, thực hiện so khớp giữa các lần xuất hiện một cách rõ ràng hoặc cập nhật liên tục các bảng tần số cho mỗi thao tác đều quá tốn kém. Độ phức tạp của mục tiêu về cơ bản là tuyến tính trong tổng kích thước đầu vào. 

Quan sát tinh tế đầu tiên là mỗi lần xuất hiện của một giá trị đều thuộc về chính xác một vị trí. Đối với một giá trị cố định`x`, giả sử nó xuất hiện`k`lần trên cả hai mảng với nhau. Trong cấu hình cuối cùng, số lượng bản sao của`x`TRONG`a`phải bằng số lượng bản sao của`x`TRONG`b`. Vì bên nhau chúng vẫn chứa đựng`k`bản sao,`k`phải chẵn. 

Một ví dụ nhỏ đã cho thấy tại sao điều kiện này lại cần thiết:```
a = [1]
b = [2]
```Giá trị`1`xuất hiện một lần và có giá trị`2`xuất hiện một lần. Không có chuỗi hoán đổi nào có thể làm cho hai mảng có nhiều tập hợp giống hệt nhau. 

Một trường hợp dễ bỏ sót khác là tự ghép đôi:```
a = [5]
b = [5]
```Trao đổi không làm gì cả. Cặp đôi đóng góp một bản sao của`5`cho từng mảng bất kể quyết định của chúng tôi. Việc xây dựng đồ thị phải xử lý các cạnh như vậy một cách chính xác. 

Bẫy thứ ba là giả định rằng tần số chẵn là đủ mà không cần xây dựng một tập hợp hoán đổi thực tế. Ví dụ:```
a = [1,2,3]
b = [2,3,1]
```Mỗi giá trị xuất hiện hai lần về tổng thể, do đó tồn tại nghiệm. Thách thức là tạo ra sự lựa chọn nhất quán cho mọi vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con vị trí. Mỗi vị trí có thể được hoán đổi hoặc không được hoán đổi, vì vậy
