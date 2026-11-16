---
title: "CF 1891B - Deja Vu"
description: "Chúng ta có một mảng các số nguyên dương và một chuỗi truy vấn. Một truy vấn có giá trị x sẽ kiểm tra mọi phần tử mảng. Nếu một phần tử chia hết cho 2^x, chúng ta thêm 2^(x-1) vào phần tử đó."
date: "2026-06-08T21:59:11+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1891
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 907 (Div. 2)"
rating: 1100
weight: 1891
solve_time_s: 41
verified: false
draft: false
---

[CF 1891B - Deja Vu](https://codeforces.com/problemset/problem/1891/B) 

**Đánh giá:** 1100 
**Tags:** sức mạnh vũ phu, toán học, sắp xếp 
**Thời gian giải:** 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng các số nguyên dương và một chuỗi truy vấn. 

Một truy vấn có giá trị`x`kiểm tra mọi phần tử mảng. Nếu một phần tử chia hết cho`2^x`, chúng tôi thêm`2^(x-1)`tới phần tử đó. 

Các truy vấn được xử lý theo thứ tự và mỗi truy vấn sẽ nhìn thấy mảng sau tất cả các sửa đổi trước đó. Sau khi tất cả các truy vấn kết thúc, chúng ta phải xuất mảng cuối cùng. 

Thoạt nhìn, đây có vẻ như là một vấn đề mô phỏng đơn giản. Đối với mỗi truy vấn, quét toàn bộ mảng và cập nhật các phần tử thỏa mãn điều kiện chia hết. Khó khăn đến từ những ràng buộc. Trên tất cả các trường hợp thử nghiệm, cả tổng số phần tử mảng và tổng số truy vấn đều có thể đạt tới`2·10^5`. Một giải pháp quét toàn bộ mảng cho mọi truy vấn sẽ hoạt động gần đúng`n·q`hoạt động, có thể xung quanh`10^10`trong trường hợp xấu nhất. Điều đó vượt xa những gì phù hợp với giới hạn thời gian 2 giây. 

Thách thức chính là hiểu cách một bản cập nhật thay đổi khả năng chia hết cho lũy thừa của hai. 

Trường hợp đặc biệt không rõ ràng xuất hiện khi cùng một giá trị truy vấn xuất hiện nhiều lần. 

Coi như:```
a = [4]
queries = [2, 2]
```Truy vấn đầu tiên áp dụng vì`4`chia hết cho`4`.```
4 -> 6
```Hiện nay`6`không còn chia hết cho`4`, vì vậy truy vấn thứ hai không làm gì cả. 

Câu trả lời cuối cùng:```
[6]
```Việc triển khai bất cẩn xử lý mọi truy vấn một cách độc lập với mảng ban đầu có thể thêm sai`2`hai lần và sản xuất`8`. 

Một trường hợp tinh vi khác là khi một truy vấn nhỏ hơn xuất hiện sau một truy vấn lớn hơn.```
a = [8]
queries = [3, 1]
```Truy vấn đầu tiên thêm`4`:```
8 -> 12
```Truy vấn thứ hai
