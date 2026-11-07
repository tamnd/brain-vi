---
title: "CF 1316B - Sửa đổi chuỗi"
description: "Chúng ta được cấp một chuỗi có độ dài $n$ và Vasya muốn thực hiện một chuỗi đảo ngược chuỗi con để tạo ra chuỗi nhỏ nhất có thể về mặt từ điển. Hoạt động được điều khiển bởi một số nguyên $k$, xác định độ dài của mỗi chuỗi con liên tiếp cần đảo ngược."
date: "2026-06-11T17:00:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1316
codeforces_index: "B"
codeforces_contest_name: "CodeCraft-20 (Div. 2)"
rating: 1400
weight: 1316
solve_time_s: 128
verified: false
draft: false
---

[CF 1316B - Sửa đổi chuỗi](https://codeforces.com/problemset/problem/1316/B) 

**Đánh giá:** 1400 
**Tags:** bạo lực, thuật toán xây dựng, triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài$n$và Vasya muốn thực hiện một chuỗi đảo ngược chuỗi con để tạo ra chuỗi nhỏ nhất có thể về mặt từ điển. Hoạt động được điều khiển bởi một số nguyên$k$, chỉ định độ dài của mỗi chuỗi con liên tiếp cần đảo ngược. Đối với mỗi sự lựa chọn của$k$, chúng ta trượt một cửa sổ có chiều dài$k$từ đầu đến cuối chuỗi, đảo ngược từng chuỗi con khi chúng ta thực hiện. Sau khi áp dụng sửa đổi này cho một trường hợp cụ thể$k$, chúng ta thu được một chuỗi ứng cử viên. Nhiệm vụ là chọn giá trị của$k$giúp giảm thiểu chuỗi kết quả và nếu nhiều$k$đưa ra mức tối thiểu như nhau, chúng tôi chọn nhỏ nhất$k$. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi chuỗi có thể có tối đa 5000 ký tự và tổng độ dài trên tất cả các trường hợp thử nghiệm được giới hạn bởi 5000. Điều này ngụ ý rằng một giải pháp có độ phức tạp tỷ lệ thuận với$n^2$là khả thi, vì$5000^2$là 25 triệu thao tác, vừa vặn thoải mái trong giới hạn 1 giây đối với Python. 

Các trường hợp cạnh bao gồm các chuỗi có độ dài 1, các chuỗi trong đó tất cả các ký tự giống hệt nhau và các chuỗi đã tối thiểu về mặt từ điển. Ví dụ: một chuỗi ký tự đơn "p" sẽ tự trả về bằng$k=1$và một chuỗi như "aaaaa" cũng sẽ tự trả về$k=1$, mặc dù nhiều$k$về mặt kỹ thuật sẽ tạo ra cùng một chuỗi. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp lại tất cả các giá trị có thể có của$k$từ 1 đến$n$, áp dụng đảo ngược chuỗi con được mô tả cho mỗi$k$và theo dõi chuỗi kết quả nhỏ nhất về mặt từ điển cùng với chuỗi tương ứng$k$. Mỗi lần đảo chiều đòi hỏi phải xử lý cẩn thận cửa sổ trượt. Đối với một chuỗi có độ dài$n$và một nhất định$k$, có$n-k+1$đảo chiều dài$k$. Thực hiện mỗi lần đảo ngược dưới dạng đảo ngược chuỗi đầy đủ mang lại kết quả$O(nk)$mỗi$k$, và lặp lại tất cả$k$cho$O(n^3)$, quá chậm. 

Thông tin chi tiết quan trọng đến từ việc nhận thấy rằng chúng ta không cần phải mô phỏng từng lần đảo chiều riêng lẻ. Nếu chúng ta xem xét tác động của việc đảo ngược mọi chuỗi con có độ dài$k$theo trình tự, chuỗi cuối cùng có thể được xây dựng bằng cách sử dụng một sơ đồ đơn giản hơn: với một chuỗi đã cho$k$, chia chuỗi thành tiền tố có độ dài$k-1$và hậu tố còn lại. Nếu như$k$là số lẻ, tiền tố vẫn giữ nguyên thứ tự; nếu như$k$chẵn thì tiền tố bị đảo ngược. Sau đó nối thêm hậu tố đảo ngược. Điều này làm giảm đáng kể công việc$O(n)$mỗi$k$, dẫn đến tổng thể$O(n^2)$giải pháp khả thi với những hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Xây dựng tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`best_string`với chuỗi giữ chỗ lớn hơn bất kỳ ứng cử viên nào có thể và`best_k`bằng 1. Điều này theo dõi kết quả nhỏ nhất về mặt từ điển và kết quả tương ứng của nó$k$. 
2. Lặp lại`k`từ 1 đến`n`. Đối với mỗi`k`, thực hiện phép dựng sau: 

một. Lấy cái đầu tiên`k-1`các ký tự của chuỗi như`prefix`. Nếu như`k`chẵn, ngược lại`prefix`; nếu như`k`là kỳ lạ, hãy để nó như vậy. Điều này thể hiện tác dụng của việc đảo ngược các chuỗi con liên tục bao gồm ký tự đầu tiên của chuỗi. 

b. Lấy chuỗi con còn lại bắt đầu từ vị trí`k-1`đến cuối cùng như`suffix`. Luôn đảo ngược hậu tố này. Nối`prefix`Và`suffix`để tạo thành chuỗi ứng cử viên cho việc này`k`. 
3. So sánh chuỗi ứng viên với`best_string`. Nếu nó nhỏ hơn, hãy cập nhật`best_string`Và`best_k`đến các giá trị hiện tại. 
4. Sau khi kiểm tra tất cả`k`, đầu ra`best_string`Và`best_k`. 

Điều này có hiệu quả vì mô hình đảo ngược chuỗi con lặp đi lặp lại giảm xuống thành một đảo ngược duy nhất của hậu tố kết hợp với đảo ngược có điều kiện của tiền tố tùy thuộc vào tính chẵn lẻ của$k$. Không cần đảo ngược lồng nhau nữa và cấu trúc luôn khớp với chuỗi cuối cùng thu được bằng mô phỏng đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    best_string = None
    best_k = 1
    for k in range(1, n + 1):
        prefix = s[:k-1]
        suffix = s[k-1:]
        if k % 2 == 0:
            prefix = prefix[::-1]
        candidate = suffix + prefix
        if best_string is None or candida_
```
