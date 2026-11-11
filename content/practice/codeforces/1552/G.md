---
title: "CF 1552G - Trọng tài nghiêm túc"
description: "Chúng ta được yêu cầu xác định xem liệu một chuỗi sắp xếp từng phần cụ thể có thể đảm bảo rằng mọi mảng có độ dài $n$ sẽ được sắp xếp đầy đủ ở cuối hay không. Dữ liệu đầu vào chỉ định $n$, kích thước của mảng và $k$, số bước sắp xếp mà Andrea thực hiện."
date: "2026-06-10T13:11:26+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dfs-and-similar", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1552
codeforces_index: "G"
codeforces_contest_name: "Codeforces Global Round 15"
rating: 3000
weight: 1552
solve_time_s: 74
verified: false
draft: false
---

[CF 1552G - Một trọng tài nghiêm túc](https://codeforces.com/problemset/problem/1552/G) 

**Đánh giá:** 3000 
**Thẻ:** bitmasks, brute Force, dfs và các loại tương tự, sắp xếp 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một chuỗi sắp xếp từng phần cụ thể có thể đảm bảo rằng bất kỳ mảng có độ dài nào$n$sẽ được sắp xếp đầy đủ vào cuối. Đầu vào chỉ định$n$, kích thước của mảng và$k$, số bước sắp xếp mà Andrea thực hiện. Mỗi bước được xác định bởi một chuỗi các chỉ số; trong bước đó, chỉ các phần tử tại các chỉ mục đó được sắp xếp lại theo thứ tự tăng dần trong khi tất cả các phần tử khác vẫn cố định. 

Đầu ra là nhị phân: chúng tôi in CHẤP NHẬN nếu chuỗi sắp xếp một phần sắp xếp mọi mảng ban đầu có thể và TỪ CHỐI nếu không. 

Nhìn vào những hạn chế,$n$nhiều nhất là 40 và$k$nhiều nhất là 10. Số nhỏ$n$cho phép chúng ta xem xét các hoạt động liên quan đến việc lặp qua mảng hoặc kiểm tra tất cả các cặp chỉ mục mà không đạt đến giới hạn hiệu suất. Tuy nhiên,$k$nhỏ cho thấy rằng việc ép buộc mọi hoán vị ban đầu có thể là không khả thi vì số lượng hoán vị tăng theo giai thừa với$n$. Chúng ta cần một giải pháp giải thích về cấu trúc của việc sắp xếp từng phần thay vì mô phỏng tất cả các mảng. 

Trường hợp đặc biệt xảy ra khi một bước chỉ sắp xếp một phần tử duy nhất hoặc khi chỉ số của các bước chồng chéo kém. Ví dụ, nếu$n=3$và các bước là$[1,3]$theo sau là$[2]$, một số mảng như$[3,1,2]$sẽ không bao giờ được sắp xếp đầy đủ. Việc triển khai đơn giản chỉ kiểm tra xem một mảng có được sắp xếp hay không sau khi áp dụng các bước có thể đưa ra câu trả lời sai vì tính chính xác đòi hỏi phải sắp xếp mảng _any_. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra mọi hoán vị của một mảng có kích thước$n$và mô phỏng thuật toán trên mỗi cái. Sau khi áp dụng tất cả$k$các bước sắp xếp cho từng hoán vị, chúng tôi sẽ kiểm tra xem kết quả có được sắp xếp hay không. Điều này đảm bảo tính chính xác vì nó thực sự kiểm tra mọi đầu vào có thể, nhưng số lượng hoán vị là$n!$, xung quanh$8 \times 10^{47}$vì$n=40$. Ngay cả với cái nhỏ bé$k$, điều này là không thể tính toán được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xem xét các giá trị; thuật toán chỉ sắp xếp các chuỗi con. Chúng ta có thể mô hình hóa vấn đề dưới dạng biểu đồ trong đó mỗi chỉ mục là một nút và một cạnh kết nối hai chỉ mục nếu có một bước sắp xếp cả hai. Bất kỳ cặp chỉ số nào được kết nối thông qua một chuỗi các bước sắp xếp đều có thể trao đổi giá trị của chúng. Do đó, chúng ta cần đảm bảo rằng thứ tự tương đối của mọi cặp vị trí mảng liền kề trong mảng được sắp xếp cuối cùng có thể được sắp xếp trực tiếp trong một bước hoặc được kết nối thông qua một chuỗi các bước. 

Cụ thể, chúng ta có thể biểu diễn sự kết nối bằng cách sử dụng một$n \times n$ma trận kề, khởi tạo các cạnh giữa bất kỳ cặp nào trong cùng một bước sắp xếp. Sau đó, chúng tôi tính toán việc đóng bắc cầu để xác định chỉ số nào có thể trao đổi giá trị gián tiếp. Cuối cùng, chúng tôi kiểm tra xem, đối với mỗi cặp vị trí liền kề$i$Và$i+1$, chúng thuộc cùng một thành phần được kết nối. Nếu chúng thực hiện cho tất cả các cặp thì bất kỳ mảng nào cũng có thể được sắp xếp, nếu không thì không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * k * n) | O(n) | Quá chậm | 
| Tối ưu | O(k*n^2 + n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một$n \times n$ma trận kề để thể hiện khả năng kết nối giữa các chỉ số mảng. Theo mặc định, mỗi chỉ mục chỉ được kết nối với chính nó. 
2. Đối với mỗi$k$các bước sắp xếp, thêm các cạnh giữa mỗi cặp chỉ số trong bước đó. Điều này mã hóa ý tưởng rằng bất kỳ
