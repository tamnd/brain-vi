---
title: "CF 1312C - Thêm quyền hạn"
description: "Chúng ta có một mảng các số nguyên không âm và cơ số $k ge 2$. Bắt đầu từ một mảng các số 0 có cùng độ dài, chúng ta có thể liên tục chọn một bước $i$ và thêm $k^i$ vào bất kỳ phần tử đơn lẻ nào của mảng hoặc bỏ qua bước này."
date: "2026-06-11T17:11:42+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "implementation", "math", "number-theory", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1312
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 83 (Rated for Div. 2)"
rating: 1400
weight: 1312
solve_time_s: 68
verified: false
draft: false
---

[CF 1312C - Thêm quyền hạn](https://codeforces.com/problemset/problem/1312/C) 

**Đánh giá:** 1400 
**Tags:** bitmasks, tham lam, thực hiện, toán học, lý thuyết số, tìm kiếm bậc ba 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm và một cơ số$k \ge 2$. Bắt đầu từ một mảng các số 0 có cùng độ dài, chúng ta có thể liên tục chọn một bước$i$và thêm$k^i$tới bất kỳ phần tử đơn lẻ nào của mảng hoặc bỏ qua bước này. Mục đích là để xác định xem có tồn tại một chuỗi các phép cộng như vậy để biến mảng 0 thành mảng đích hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi, chúng tôi có độ dài mảng$n$lên tới 30 và các phần tử mảng mục tiêu lên tới$10^{16}$. Căn cứ$k$có thể lên tới 100. Vì độ dài mảng tối đa nhỏ nên chúng ta có thể thực hiện các phép toán tuyến tính theo$n$hoặc logarit với số lớn nhất. Tuy nhiên, bất kỳ phương pháp nào liệt kê tất cả các tập con lũy thừa cho tất cả các vị trí đều không khả thi vì mỗi số có thể có tới$\log_k(10^{16})\approx 27$quyền lực, và chúng ta có$n$những con số. Điều đó sẽ bùng nổ kết hợp. 

Các trường hợp cạnh không rõ ràng phát sinh khi nhiều số yêu cầu cùng một lũy thừa$k$. Ví dụ, nếu mảng là`[1,1]`Và$k=2$, chúng ta sẽ cần hai lần xuất hiện của$2^0=1$. Vì mỗi sức mạnh chỉ có thể được gán cho một vị trí trên mỗi bước nên điều này là không thể. Một trường hợp cạnh khác là mảng chứa các số 0, có thể được thỏa mãn một cách đơn giản bằng cách bỏ qua tất cả các bước. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là tạo ra tất cả các chuỗi phép cộng của$k$tới các vị trí. Đối với mỗi quyền lực$k^i$, chúng ta có thể thử thêm nó vào bất kỳ vị trí nào hoặc bỏ qua. Điều này có tác dụng vì mọi dãy đều hợp lệ nhưng cực kỳ kém hiệu quả: với tối đa 27 lũy thừa cho mỗi số và$n$số lượng, chúng tôi có khoảng$n^{\log_k(a_{max})} \sim 30^{27}$sự kết hợp, có quy mô lớn về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng đến từ việc biểu diễn các con số trong một cơ sở đã sửa đổi-$k$hệ thống. Bất kỳ số nào cũng có thể được biểu diễn duy nhất dưới dạng tổng lũy ​​thừa của$k$, trong đó mỗi lũy thừa xuất hiện nhiều nhất một lần ở mỗi vị trí. Vì chúng ta có thể gán từng lũy ​​thừa của$k$đối với nhiều nhất một phần tử của mảng, chúng ta có thể lặp lại các số và phân tách chúng thành lũy thừa của$k$. Nếu bất kỳ nguồn điện nào cần được sử dụng nhiều lần trên mảng thì điều đó là không thể. Điều này biến bài toán thành bài toán đếm số mũ của$k$ở mỗi số. 

Quan sát này biến vấn đề thành việc lặp lại từng số, chia liên tục cho$k$và ghi lại phần còn lại. Phần còn lại phải là 0 hoặc 1 cho mỗi lũy thừa; mặt khác, con số này yêu cầu cùng một công suất nhiều lần. Tổng hợp số lũy thừa của từng lũy ​​thừa trên mảng, nếu bất kỳ số nào vượt quá 1 thì chúng ta không thể dựng được mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^max_số mũ) | O (n * max_số mũ) | Quá chậm | 
| Tối ưu (phân rã base-k tham lam) | O(n * log_k(a_max)) | O(log_k(a_max)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc độ dài mảng$n$và căn cứ$k$và mảng mục tiêu$a$. 
2. Khởi tạo một bản đồ (hoặc từ điển) trống`count`để theo dõi mỗi sức mạnh của$k$được sử dụng trên
