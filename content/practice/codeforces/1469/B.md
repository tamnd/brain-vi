---
title: "CF 1469B - Đỏ và Xanh"
description: "Chúng ta có hai dãy số nguyên, một dãy biểu thị các phần tử được tô màu đỏ và dãy còn lại biểu thị các phần tử được tô màu xanh của dãy ban đầu."
date: "2026-06-11T01:08:42+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1469
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 101 (Rated for Div. 2)"
rating: 1000
weight: 1469
solve_time_s: 110
verified: true
draft: false
---

[CF 1469B – Đỏ và Xanh](https://codeforces.com/problemset/problem/1469/B) 

**Đánh giá:** 1000 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai dãy số nguyên, một dãy biểu thị các phần tử được tô màu đỏ và dãy còn lại biểu thị các phần tử được tô màu xanh của dãy ban đầu. Thứ tự trong mỗi màu được giữ nguyên nhưng chúng ta không biết cách xen kẽ các phần tử màu đỏ và xanh lam để tái tạo lại chuỗi ban đầu. Nhiệm vụ là tìm thứ tự của hai dãy con này để tối đa hóa tổng tiền tố lớn nhất, trong đó tổng tiền tố được xác định là tổng của phần tử đầu tiên, tổng của hai phần tử đầu tiên, v.v. Chúng tôi chỉ quan tâm đến mức tối đa trong số các khoản tiền này và số không. 

Kích thước đầu vào nhỏ, với mỗi chuỗi màu chứa tối đa 100 phần tử và tối đa 1000 trường hợp thử nghiệm. Điều này có nghĩa là giải pháp O(n + m) cho mỗi trường hợp thử nghiệm có thể chấp nhận được, nhưng bất kỳ thuật toán nào thử tất cả các lần xen kẽ có thể sẽ không khả thi vì số lượng các lần xen kẽ là tổ hợp và có thể cực kỳ lớn. 

Trường hợp cạnh tinh tế là khi tất cả các phần tử đều âm. Trong trường hợp đó, tổng tiền tố tối đa bằng 0, vì lấy bất kỳ phần tử nào sẽ làm giảm tổng. Một trường hợp khác là khi một chuỗi có tất cả các giá trị âm và chuỗi kia có giá trị dương. Thuật toán phải cẩn thận để có thể bỏ qua hoàn toàn các tiền tố phủ định. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra mọi sự xen kẽ hợp lệ của các chuỗi màu đỏ và màu xanh lam và tính tổng tiền tố tối đa cho mỗi chuỗi. Vì một dãy có độ dài$n + m$có$\binom{n+m}{n}$sự xen kẽ, ngay cả những ràng buộc lớn nhất (n = m = 100) làm cho phương pháp này hoàn toàn không thực tế, vì nó đòi hỏi$10^{58}$tính toán. 

Thông tin chi tiết quan trọng là tổng tiền tố tối đa chỉ phụ thuộc vào tổng tiền tố trong mỗi chuỗi màu một cách độc lập. Vì chúng ta có thể đặt tất cả các phần tử của một màu trước các phần tử của màu kia, nên chúng ta chỉ cần xem xét việc lấy riêng tổng tiền tố tốt nhất của chuỗi màu đỏ và tổng tiền tố tốt nhất của chuỗi màu xanh lam. Đặt tiền tố có tổng dương lớn nhất trước tiên không gây hại gì và các tổng này có tính cộng. Do đó, giải pháp giảm xuống việc tính tổng tiền tố tối đa của chuỗi màu đỏ, tổng tiền tố tối đa của chuỗi màu xanh và tính tổng chúng, nhưng đảm bảo chúng tôi không có đóng góp âm (chúng tôi sử dụng tối đa bằng 0). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n+m)) | O(n+m) | Quá chậm | 
| Tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`max_red`Và`max_blue`về không. Chúng sẽ lưu trữ tổng tiền tố tối đa tương ứng cho các chuỗi màu đỏ và xanh lam. 
2. Lặp lại chuỗi màu đỏ. Duy trì một khoản tiền đang chạy. Sau khi thêm từng phần tử, hãy cập nhật`max_red`nếu tổng số tiền hiện có vượt quá nó. Điều này tìm thấy số tiền lớn nhất có thể đạt được bằng cách lấy tiền tố của chuỗi màu đỏ. 
3. Lặp lại bước 2 cho chuỗi màu xanh lam, lưu kết quả vào`max_blue`. 
4. Giá trị tối đa có thể có của`f(a)`là`max_red + max_blue`. Cả hai tổng tiền tố đều đóng góp tích cực nếu chúng dương; số tiền âm được bỏ qua. 

Lý do điều này có hiệu quả là việc thêm bất kỳ phần tử bổ sung nào sau tổng tiền tố tối đa của một chuỗi không thể tăng mức tối đa vượt quá tổng tiền tố màu đỏ và xanh lam tốt nhất. Thứ tự trong màu sắc là cố định, vì vậy quyết định duy nhất là làm thế nào để xen kẽ các chuỗi. Bằng cách tận dụng sự đóng góp tích cực tối đa từ mỗi bên một cách độc lập, chúng tôi đảm bảo mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    r = list(map(int, input().split()))
    m = int(input())
    b = list(map(int, input().split()))
    
    max_red = max_blue = 0
    curr_sum = 0
    for val in r:
        curr_sum += val
        if curr_sum > max_red:
            max_red = curr_sum
    
    curr_sum = 0
    for val in b:
        curr_sum += val
        if curr_sum > max_blue:
            max_blue = curr_sum
    
    print(max_red + max_blue)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi chuỗi, chúng tôi duy trì tổng hiện hành và cập nhật tổng tiền tố tối đa, đảm bảo rằng tiền tố âm không làm giảm kết quả. Tổng tiền tố màu đỏ tốt nhất và tiền tố màu xanh tốt nhất sẽ đưa ra câu trả lời cho từng trường hợp thử nghiệm. Phải cẩn thận để thiết lập lại tổng trước khi xử lý chuỗi màu xanh. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

Đỏ: [6, -5, 7, -3] 

Xanh lam: [2, 3, -4] 

| Bước | Chạy Tổng Đỏ | Đỏ tối đa | Chạy Sum Xanh | Xanh tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 6 | 2 | 2 | 
| 2 | 1 | 6 | 5 | 5 | 
| 3 | 8 | 8 | 1 | 5 | 
| 4 | 5 | 8 | -3 | 5 | 

Tổng tiền tố tối đa màu đỏ = 8 

Tổng tiền tố tối đa màu xanh lam = 5 

Tổng cộng = 13 

**Ví dụ 2** 

Đỏ: [1, 1] 

Xanh lam: [10, -3, 2, 2] 

| Bước | Chạy Tổng Đỏ | Đỏ tối đa | Chạy Sum Xanh | Xanh tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 10 | 10 | 
| 2 | 2 | 2 | 7 | 10 | 

Tổng tiền tố tối đa màu đỏ = 2 

Tổng tiền tố tối đa màu xanh lam = 10 

Tổng cộng = 12 

Những dấu vết này xác nhận rằng thuật toán tích lũy chính xác các đóng góp tích cực trong khi bỏ qua các chuỗi sẽ làm giảm tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được truy cập chính xác một lần để tính tổng chạy | 
| Không gian | O(1) thêm | Chỉ số tiền đang chạy và giá trị tối đa mới được lưu trữ; mảng đầu vào được đưa ra | 

Với tối đa 1000 test case, tổng số thao tác nhiều nhất là$1000 \times (100 + 100) = 2 \times 10^5$, dễ dàng phù hợp với giới hạn thời gian 2 giây. Mức sử dụng bộ nhớ không đáng kể so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        r = list(map(int, input().split()))
        m = int(input())
        b = list(map(int, input().split()))
        
        max_red = max_blue = 0
        curr_sum = 0
        for val in r:
            curr_sum += val
            max_red = max(max_red, curr_sum)
        
        curr_sum = 0
        for val in b:
            curr_sum += val
            max_blue = max(max_blue, curr_sum)
        
        print(max_red + max_blue)
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n4\n6 -5 7 -3\n3\n2 3 -4\n2\n1 1\n4\n10 -3 2 2\n5\n-1 -2 -3 -4 -5\n1\n0\n1\n0\n") == "13\n13\n0\n0", "samples"

# Custom cases
assert run("1\n1\n-10\n1\n-5\n") == "0", "all negative"
assert run("1\n3\n1 2 3\n2\n-1 -2\n") == "6", "positive and negative mix"
assert run("1\n5\n0 0 0 0 0\n5\n1 1 1 1 1\n") == "5", "zeros and positives"
assert run("1\n1\n100\n1\n100\n") == "200", "single large positives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 -10 -5 | 0 | mọi số âm đều tạo ra số 0 | 
| 3 1 2 3 -1 -2 | 6 | đóng góp tích cực từ các khoản tiền đỏ và xanh | 
| 5 số không, 5 số một | 5 | bỏ qua đóng góp bằng không một cách chính xác | 
| 1 100, 1 100 | 200 | xử lý chính xác các tổng dương lớn có một phần tử | 

## Vỏ cạnh 

Khi tất cả các phần tử đều âm, ví dụ như đỏ = [-10], xanh lam = [-5], tổng hiện có không bao giờ vượt quá 0. Thuật toán khởi tạo`max_red`Và`max_blue`bằng 0, do đó đầu ra cuối cùng chính xác là 0. Điều này tránh việc chọn sai tiền tố âm. 

Khi các chuỗi chứa số 0, chúng không làm giảm tổng, vì vậy chúng được đưa vào một cách tự nhiên nếu có lợi. Ví dụ: red = [0, 0], blue = [1, 2], max_red = 0, max_blue = 3, Total = 3, phù hợp với cách đan xen tối ưu.
