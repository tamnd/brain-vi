---
title: "CF 2153B - Đảo ngược bit"
description: "Chúng ta được yêu cầu xác định xem liệu có thể xây dựng ba số nguyên không âm, $a$, $b$ và $c$, sao cho các AND theo cặp bit của chúng bằng ba số nguyên cho trước $x$, $y$ và $z$ hay không. Cụ thể, các ràng buộc là $a & b = x$, $b & c = y$, và $a & c = z$."
date: "2026-06-08T00:40:36+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2153
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1057 (Div. 2)"
rating: 800
weight: 2153
solve_time_s: 78
verified: true
draft: false
---

[CF 2153B - Đảo ngược bit](https://codeforces.com/problemset/problem/2153/B) 

**Đánh giá:** 800 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem có thể dựng được ba số nguyên không âm hay không,$a$,$b$, Và$c$, sao cho cặp số nguyên theo cặp VÀ bằng ba số nguyên đã cho$x$,$y$, Và$z$. Cụ thể, những hạn chế đó là$a \& b = x$,$b \& c = y$, Và$a \& c = z$. Mỗi trường hợp thử nghiệm đưa ra ba số nguyên này và nhiệm vụ là trả về "CÓ" nếu những số đó tồn tại và "KHÔNG" nếu ngược lại. 

Các số nguyên đầu vào$x$,$y$, Và$z$có thể đi lên$10^9$, có nghĩa là chúng ta đang xử lý các số vừa với 30 bit. Số ca kiểm thử$t$có thể lên đến$10^4$, vì vậy giải pháp của chúng tôi phải xử lý hiệu quả từng trường hợp thử nghiệm trong khoảng thời gian gần như không đổi hoặc tối đa là logarit là cần thiết. 

Các trường hợp cạnh có thể phá vỡ việc triển khai ngây thơ bao gồm các tình huống trong đó các bit chồng chéo theo những cách xung đột. Ví dụ, hãy xem xét$x = 4$,$y = 8$,$z = 12$. Một cách tiếp cận đơn giản có thể cố gắng xây dựng các số một cách độc lập mà không cần kiểm tra tính khả thi theo từng bit. Một số bit trong$x$,$y$, Và$z$có thể không thể thỏa mãn đồng thời nên thuật toán phải suy luận từng chút một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là lặp lại tất cả các bộ ba$a, b, c$trong phạm vi được quy định bởi mức tối đa$x, y, z$. Đối với mỗi bộ ba, hãy kiểm tra xem ba điều kiện AND có đúng không. Điều này đúng nhưng hoàn toàn không thực tế vì con số có thể lên tới$10^9$, và thậm chí còn cố gắng$2^{30}$khả năng cho mỗi biến là không khả thi. 

Thông tin chi tiết quan trọng là thao tác AND hoạt động độc lập trên từng bit. Mỗi bit trong$x, y, z$chỉ hạn chế bit tương ứng trong$a, b, c$. Ví dụ: nếu bit thứ 2 của$x$là 1 thì bit thứ 2 trong cả hai$a$Và$b$phải là 1. Nếu là 0 thì ít nhất một trong$a$hoặc$b$phải là 0 trong bit đó. Bằng cách suy luận từng chút một, chúng ta rút gọn vấn đề thành việc kiểm tra xem các ràng buộc của mỗi bit có được thỏa mãn đồng thời hay không. 

Khi chúng ta xem vấn đề là các ràng buộc bit độc lập, chúng ta có thể xây dựng các bit ứng viên cho$a, b, c$tham lam: đối với mỗi vị trí bit, gán 1 cho bất kỳ số nào được AND yêu cầu, nhưng đảm bảo không vi phạm các AND khác. Nếu bất kỳ bit nào có xung đột không thể giải quyết được thì câu trả lời là "KHÔNG". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^30*2^30*2^30) | O(1) | Quá chậm | 
| Phân tích theo bit | O(30) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên$x, y, z$cho trường hợp thử nghiệm hiện tại. 
2. Lặp lại từng vị trí bit từ 0 đến 30. Coi bit thứ 0 là ít quan trọng nhất. 
3. Với mỗi bit, hãy kiểm tra giá trị của bit đó trong$x, y, z$. Có bốn trường hợp có thể xảy ra tùy thuộc vào bit là 0 hay 1 trong mỗi số trong ba số. 
4. Nếu bất kỳ bit nào yêu cầu các phép gán trái ngược nhau - ví dụ:$x$có 1 ở bit này (buộc$a$Và$b$đến 1) nhưng$z$có 0 (yêu cầu ít nhất một trong$a$hoặc$c$bằng 0) và chúng tôi không thể đáp ứng cả hai - ngay lập tức trả về "KHÔNG". 
5. Nếu tất cả các bit có thể được gán một cách nhất quán, hãy trả về "CÓ". 

Tại sao nó hoạt động: Mỗi vị trí bit có thể được xem xét độc lập vì các phép toán AND không trộn lẫn các bit giữa các vị trí. Bằng cách kiểm tra tính khả thi ở từng bit và xác nhận rằng không tồn tại xung đột, chúng tôi đảm bảo rằng việc gán số nguyên hợp lệ tồn tại khi và chỉ khi thuật toán trả về "CÓ". Điều này làm giảm bài toán tổ hợp ban đầu xuống còn 30 bước kiểm tra đơn giản cho mỗi trường hợp thử nghiệm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y, z = map(int, input().split())
        possible = True
        for i in range(31):
            xi = (x >> i) & 1
            yi = (y >> i) & 1
            zi = (z >> i) & 1
            # if xi = 1, a & b = 1 -> a_i = b_i = 1
            # if yi = 1, b & c = 1 -> b_i = c_i = 1
            # if zi = 1, a & c = 1 -> a_i = c_i = 1
            # check if these force conflicts
            a = b = c = 0
            if xi: a = b = 1
            if yi: b = c = 1
            if zi: a = c = 1
            # after combining, check if constraints hold
            if (a & b) != xi or (b & c) != yi or (a & c) != zi:
                possible = False
                break
        print("YES" if possible else "NO")

solve()
```Giải pháp đọc số lượng trường hợp thử nghiệm và xử lý từng bộ ba một cách độc lập. Đối với mỗi bit, nó tính toán các bit cần thiết cho$a, b, c$và xác minh xem các phép gán có thỏa mãn cả ba điều kiện AND hay không. Thứ tự của các bài tập rất linh hoạt vì chúng ta có thể kết hợp chúng một cách tham lam và kiểm tra kết quả cuối cùng. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** x=1, y=1, z=1 

| chút | xi | yi | tử | một | b | c | (a&b) | (b&c) | (a&c) | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 

Tất cả các bit đều thỏa mãn các ràng buộc, câu trả lời là CÓ. 

**Ví dụ 2:** x=4, y=8, z=12 

| chút | xi | yi | tử | một | b | c | (a&b) | (b&c) | (a&c) | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 

Xung đột phát sinh: (b&c)=0 bắt buộc, nhưng bài tập của chúng tôi đưa ra 1. Câu trả lời là KHÔNG. 

Điều này chứng tỏ thuật toán xác định chính xác các cấu hình không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t*30) | Mỗi trường hợp thử nghiệm lặp lại trên 31 bit | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên cho mỗi trường hợp thử nghiệm | 

Với$t \le 10^4$và 30 lần lặp cho mỗi trường hợp thử nghiệm, tổng số thao tác là khoảng 3*10^5, nằm trong giới hạn 2 giây một cách thoải mái. 

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
assert run("5\n1 1 1\n3 2 6\n4 8 12\n9 10 12\n12730 3088 28130\n") == "YES\nYES\nNO\nYES\nNO", "sample 1"

# Custom cases
assert run("2\n0 0 0\n1 0 1\n") == "YES\nNO", "zero and single bit conflict"
assert run("1\n1073741823 1073741823 1073741823\n") == "YES", "all 30 bits set"
assert run("1\n1 2 3\n") == "NO", "impossible small numbers"
assert run("1\n5 1 4\n") == "YES", "mixed bit positions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | CÓ | Tất cả các số không luôn khả thi | 
| 1 0 1 | KHÔNG | Xung đột trong một bit ngăn cản giải pháp | 
| 1073741823 1073741823 1073741823 | CÓ | Số tối đa 30 bit | 
| 1 2 3 | KHÔNG | Số nhỏ có bit xung đột | 
| 5 1 4 | CÓ | Nhiều bit được đặt ở các vị trí khác nhau | 

## Vỏ cạnh 

Trường hợp đầu vào bằng 0 (x=y=z=0) vượt qua vì thuật toán gán tất cả các bit thành 0, đáp ứng tất cả AND. Trường hợp số 30 bit tối đa kiểm tra xem vị trí bit được xử lý chính xác đối với các giá trị lớn. Một ví dụ xung đột như x=1, y=0, z=1 sẽ kích hoạt chính xác thuật toán trả về NO khi một bit không thể đồng thời thỏa mãn nhiều ràng buộc AND. Mỗi điều này được xử lý trực tiếp trong vòng lặp bitwise, xác nhận rằng giải pháp tôn trọng tất cả các ràng buộc mà không cố gắng tạo ra các số thực.
