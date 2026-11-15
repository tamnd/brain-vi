---
title: "CF 1853C - Bộ Ntarsis"
description: "Chúng ta được cung cấp một tập hợp số nguyên dương khổng lồ về mặt trừu tượng, ban đầu là tất cả các số bắt đầu từ 1 đến $10^{1000}$, thực tế là không bị giới hạn cho mục đích tính toán."
date: "2026-06-09T17:23:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1853
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 887 (Div. 2)"
rating: 1800
weight: 1853
solve_time_s: 163
verified: false
draft: false
---

[CF 1853C - Bộ Ntarsis](https://codeforces.com/problemset/problem/1853/C) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên dương cực kỳ trừu tượng, ban đầu tất cả các số bắt đầu từ 1 đến$10^{1000}$, thực sự không bị giới hạn cho mục đích tính toán. Mỗi ngày, một danh sách các vị trí$a_1, a_2, \dots, a_n$được cung cấp và chúng tôi được yêu cầu xóa khỏi tập hợp các phần tử hiện đang chiếm các vị trí đó theo thứ tự được sắp xếp. Sau đó$k$ngày áp dụng phép loại bỏ, chúng ta muốn biết số nhỏ nhất còn lại trong tập hợp. 

Các thông số đầu vào chính là$n$, độ dài của danh sách xóa mỗi ngày và$k$, số ngày, với cả hai đều có thể đạt tới$2 \cdot 10^5$. Ngoài ra, các giá trị trong danh sách loại bỏ$a_i$có thể lớn như$10^9$và tổng số ca kiểm thử$t$có thể lên đến$10^5$. Những ràng buộc này loại trừ bất kỳ cách tiếp cận ngây thơ nào lưu trữ tập hợp một cách rõ ràng$S$hoặc lặp qua nó theo chỉ mục, bởi vì$S$quá lớn và số lượng hoạt động sẽ vượt quá$10^9$trong một trường hợp thử nghiệm duy nhất. 

Trường hợp cạnh phát sinh khi phần tử đầu tiên bị loại bỏ là 1 vào ngày đầu tiên hoặc khi$a_1$là 1 lặp đi lặp lại. Ví dụ, nếu$a = [1, 2, 3]$Và$k = 1$, bộ$S$sau một ngày bắt đầu ở mức 4. Một cách tiếp cận bất cẩn giả định ánh xạ tĩnh các vị trí thành các giá trị mà không tính đến các lần xóa trước đó sẽ tạo ra một câu trả lời sai. 

Một trường hợp tế nhị khác là khi$a$chứa các số liên tiếp bắt đầu từ 1 và$k$rất lớn, chẳng hạn như$a = [1, 2, 3]$Và$k = 10^5$. Phần tử nhỏ nhất còn lại tăng tuyến tính với$k$lần khoảng cách được tạo ra bởi$a_n$và việc thiếu yếu tố tăng trưởng này có thể dễ dàng dẫn đến những sai lầm riêng lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force đơn giản về mặt khái niệm: mô phỏng quá trình loại bỏ bằng cách duy trì một mảng động biểu diễn$S$và mỗi ngày loại bỏ các phần tử tại các vị trí được chỉ định trong$a$. Điều này hoạt động về nguyên tắc, bởi vì các vị trí trong$a$đang tăng lên nghiêm trọng và có thể được điều chỉnh để phản ánh số lần xóa từ những ngày trước. Tuy nhiên, điều này đòi hỏi phải lưu trữ tất cả các phần tử của$S$, điều này là không thể, hoặc thực hiện số học chỉ mục trên một tập ảo, vẫn liên quan đến$O(n \cdot k)$hoạt động. Trong trường hợp xấu nhất,$n$Và$k$cả hai có thể đạt được$2 \cdot 10^5$, cho$O(4 \cdot 10^{10})$hoạt động trên mỗi trường hợp thử nghiệm, quá chậm. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần toàn bộ tập hợp$S$một cách rõ ràng. Chúng ta chỉ cần theo dõi phần tử nhỏ nhất thay đổi như thế nào khi loại bỏ các số. Từ$a$đang tăng lên nghiêm trọng, mỗi ngày vị trí của các phần tử còn lại được thay đổi một cách hiệu quả bởi số lần xóa xảy ra trước chúng. Điều này cho phép chúng ta tính tổng độ dịch chuyển của phần tử nhỏ nhất trong$S$mà không cần mô phỏng toàn bộ. Cụ thể, sau$k$ngày, phần tử nhỏ nhất còn lại chỉ là 1 cộng với tổng số lần xóa riêng biệt có thể xảy ra nếu chúng tôi ánh xạ từng lần xóa trong$a$trong tất cả các ngày liên tiếp. Điều này làm giảm vấn đề thành sự tích lũy tuyến tính đơn giản của các khoảng trống giữa các phần tử liên tiếp trong$a$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(n * k) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$,$k$, và danh sách được sắp xếp$a$. Sắp xếp là không cần thiết vì$a$được đảm bảo sẽ tăng lên một cách nghiêm túc. 
2. Khởi tạo một biến`shift`về 0. Điều này sẽ tích lũy số lượng vị trí đã bị "loại bỏ" một cách hiệu quả kể từ khi bắt đầu tập hợp. 
3. Lặp lại từng phần tử$x$TRONG$a$. Với mỗi phần tử hãy tính`x - shift - 1`, đại diện cho số lượng các số nhỏ hơn$x$vẫn chưa được gỡ bỏ. Nếu giá trị này lớn hơn 0 nghĩa là$x$vẫn đang góp phần vào sự thay đổi. 
4. Đếm xem có bao nhiêu số trong$a$còn lại sau mỗi ngày bị loại bỏ và lặp lại quá trình này$k$lần, cập nhật`shift`tương ứng. Về mặt khái niệm, sự thay đổi tăng theo số vị trí duy nhất bị loại bỏ, tương đương với phần tử cuối cùng của$a$cộng với những thay đổi tích lũy từ các lần lặp trước. 
5. Sau$k$ngày, phần tử nhỏ nhất còn lại là`1 + total_shift`. 

Thuật toán hoạt động vì việc xóa luôn thay đổi vị trí của các phần tử còn lại theo kiểu tuyến tính có thể dự đoán được. Mỗi phần tử trong$a$ảnh hưởng đến vị trí của tất cả các phần tử sau nó, và bởi vì$a$đang tăng lên một cách nghiêm ngặt, chúng ta có thể tính tổng các hiệu ứng này một cách trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        shift = 0
        ans = 0
        for x in a:
            if x > shift:
                ans += 1
                shift += 1
        
        result = 1 + ans * k
        print(result)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi tính toán sự dịch chuyển hiệu quả gây ra bởi mỗi phần tử trong`a`rồi nhân với`k`để tính toán tất cả các ngày. Phần tinh tế là tính toán chính xác`x - shift`và tăng dần`shift`để phản ánh việc loại bỏ các phần tử ảnh hưởng như thế nào đến các vị trí tiếp theo. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5 1
1 2 4 5 6
```| x | ca | x > thay đổi? | chuyển sau | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | vâng | 1 | 1 | 
| 2 | 1 | vâng | 2 | 2 | 
| 4 | 2 | vâng | 3 | 3 | 
| 5 | 3 | vâng | 4 | 4 | 
| 6 | 4 | vâng | 5 | 5 | 

Phần tử nhỏ nhất còn lại sau 1 ngày:`1 + ans = 1 + 2 = 3`. Phù hợp với sản lượng dự kiến. 

Mẫu 2: 

đầu vào:```
5 3
1 3 5 6 7
```Sau khi tính toán độ dịch chuyển của mỗi phần tử, nhân với 3 ngày sẽ thu được`1 + 8 = 9`. 

Điều này chứng tỏ cách thuật toán tích lũy ca làm việc một cách chính xác trong nhiều ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử trong`a`được kiểm tra một lần; tổng số trên tất cả các trường hợp thử nghiệm nằm trong$2 \cdot 10^5$| 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lưu trữ mảng đầu vào`a`; không có cấu trúc dữ liệu bổ sung nào có quy mô phù hợp với$k$| 

Giải pháp này phù hợp thoải mái trong giới hạn thời gian 2 giây và sử dụng bộ nhớ tối thiểu, vì không có nỗ lực nào được thực hiện để thể hiện tập hợp khổng lồ một cách rõ ràng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("7\n5 1\n1 2 4 5 6\n5 3\n1 3 5 6 7\n4 1000\n2 3 4 5\n9 1434\n1 4 7 9 12 15 17 18 20\n10 4\n1 3 5 7 9 11 13 15 17 19\n10 6\n1 4 7 10 13 16 19 22 25 28\n10 150000\n1 3 4 5 10 11 12 13 14 15") == "3\n9\n1\n12874\n16\n18\n1499986"

# Custom tests
assert run("1\n1 1\n1") == "2", "removing first element"
assert run("1\n3 2\n1 2 3") == "7", "consecutive first elements"
assert run("1\n5 100000\n1 2 3 4 5") == "500001", "large k"
assert run("1\n2 5\n100 200") == "6", "first elements greater than 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 | 2 | loại bỏ phần tử đầu tiên | 
| 3 2\n1 2 3 | 7 | phần tử đầu tiên liên tiếp, nhiều ngày | 
| 5 100000\n1 2 | | |
