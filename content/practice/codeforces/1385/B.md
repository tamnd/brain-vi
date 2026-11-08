---
title: "CF 1385B - Khôi phục hoán vị bằng cách sáp nhập"
description: "Chúng ta được cung cấp một mảng có độ dài $2n$. Mảng này được tạo bằng cách lấy một hoán vị $p$ của các số từ $1$ đến $n$, tạo bản sao thứ hai của cùng một hoán vị và xen kẽ hai bản sao trong khi vẫn giữ nguyên thứ tự tương đối bên trong mỗi bản sao."
date: "2026-06-11T10:42:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1385
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 656 (Div. 3)"
rating: 800
weight: 1385
solve_time_s: 175
verified: false
draft: false
---

[CF 1385B - Khôi phục hoán vị bằng cách hợp nhất](https://codeforces.com/problemset/problem/1385/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$2n$. Mảng này được tạo bằng cách thực hiện một hoán vị$p$của những con số$1$bởi vì$n$, tạo bản sao thứ hai của cùng một hoán vị và xen kẽ hai bản sao trong khi vẫn giữ nguyên thứ tự tương đối bên trong mỗi bản sao. 

Mọi giá trị từ$1$ĐẾN$n$xuất hiện chính xác hai lần trong mảng cuối cùng. Nhiệm vụ của chúng ta là xây dựng lại hoán vị ban đầu. 

Chi tiết quan trọng là câu trả lời được đảm bảo tồn tại và được đảm bảo là duy nhất. 

Những hạn chế là rất nhỏ. Mỗi ca kiểm thử có$n \le 50$, do đó mảng đầu vào chứa tối đa 100 số. Thậm chí một$O(n^2)$giải pháp sẽ chạy thoải mái trong giới hạn. Thách thức không phải là hiệu suất, mà là nhận biết cấu trúc được tạo ra bằng cách hợp nhất hai hoán vị giống hệt nhau. 

Một lỗi phổ biến là cố gắng xây dựng lại chính quá trình hợp nhất. Điều đó là không cần thiết và có thể dễ dàng trở nên phức tạp. Việc đảm bảo rằng cả hai bản sao đều có cùng hoán vị sẽ mang lại sự quan sát đơn giản hơn nhiều. 

Xem xét đầu vào```
1 3 1 4 3 4 2 2
```Câu trả lời đúng là```
1 3 4 2
```Một cách tiếp cận bất cẩn có thể cố gắng ghép các lần xuất hiện lại với nhau và suy ra từng bản sao đến từ đâu. Không có thông tin nào là cần thiết. Lần đầu tiên mỗi số xuất hiện đã cho thấy hoán vị. 

Một trường hợp cạnh khác xảy ra khi hai bản sao hoàn toàn tách biệt:```
1 2 3 1 2 3
```Câu trả lời vẫn là```
1 2 3
```Giải pháp dự kiến ​​hai lần xuất hiện của mỗi giá trị liền kề sẽ không thành công. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các hoán vị có thể có của độ dài$n$, mô phỏng tất cả các sự hợp nhất hợp lệ của hai bản sao và kiểm tra xem bản nào tạo ra mảng đã cho. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Thật không may, có$n!$hoán vị, trở nên rất lớn ngay cả đối với các giá trị khiêm tốn của$n$. 

Cấu trúc của việc hợp nhất làm cho một giải pháp đơn giản hơn nhiều có thể thực hiện được. 

Lấy bất kỳ giá trị nào$x$trong hoán vị ban đầu. Vì cả hai bản sao đều giữ nguyên trật tự bên trong nên lần xuất hiện đầu tiên của$x$trong mảng đã hợp nhất phải xuất hiện chính xác khi nào$x$đầu tiên có sẵn từ một trong hai bản sao. Trước thời điểm đó,$x$chưa bao giờ xuất hiện. 

Vì hoán vị ban đầu chứa mỗi số đúng một lần nên chuỗi xuất hiện đầu tiên trong mảng được hợp nhất chính xác là hoán vị ban đầu. 

Ví dụ:```
2 3 2 4 1 3 4 1
```Đọc từ trái sang phải, lần xuất hiện đầu tiên là:```
2, 3, 4, 1
```đó là câu trả lời 

Vấn đề giảm xuống việc thu thập số theo thứ tự
