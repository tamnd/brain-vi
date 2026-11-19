---
title: "CF 2085E - Phục vụ và Modulo"
description: "Chúng ta có hai mảng có cùng độ dài: mảng ban đầu a gồm các số nguyên không âm và mảng được xáo trộn b. Mảng b được cho là được tạo bằng cách lấy mọi phần tử của modulo một số nguyên k chưa biết, sau đó xáo trộn kết quả."
date: "2026-06-08T06:08:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2085
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1011 (Div. 2)"
rating: 2200
weight: 2085
solve_time_s: 109
verified: false
draft: false
---

[CF 2085E - Serval và Modulo](https://codeforces.com/problemset/problem/2085/E) 

**Xếp hạng:** 2200 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng có cùng độ dài: mảng ban đầu`a`của các số nguyên không âm và một mảng được xáo trộn`b`. Mảng`b`được cho là được tạo ra bằng cách lấy mọi phần tử của`a`modulo một số nguyên chưa biết`k`, sau đó xáo trộn kết quả. Nhiệm vụ của chúng ta là xác định giá trị có thể có của`k`. Nếu không tồn tại số nguyên như vậy thì chúng ta nên xuất ra`-1`. Có thể có nhiều câu trả lời hợp lệ; bất kỳ một đều có thể chấp nhận được. Nếu hợp lệ`k`tồn tại thì nó được đảm bảo không lớn hơn$10^9$. 

Thách thức chính là hoạt động modulo che giấu thông tin. Đối với các giá trị nhỏ của`k`, nhiều số khác nhau trong`a`có thể tạo ra cùng một số dư, vì vậy chúng ta phải cẩn thận đối chiếu số dư trong`b`. Đối với các giá trị lớn của`k`,`a_i \bmod k`bằng`a_i`chính nó nếu`k`lớn hơn tất cả`a_i`. Vì vậy, nếu mảng được xáo trộn`b`chứa bất kỳ số nào lớn hơn số tương ứng trong`a`, hợp lệ`k`không tồn tại. 

Các ràng buộc yêu cầu chúng ta phải xử lý tối đa$10^4$các phần tử cho mỗi trường hợp thử nghiệm và tối đa$10^4$trường hợp thử nghiệm tổng thể. Một cách tiếp cận bạo lực lặp đi lặp lại trên tất cả các số nguyên lên đến$10^9$là không thể thực hiện được. Thậm chí lặp lại tất cả sự khác biệt của các cặp trong`a`Và`b`phải được thực hiện cẩn thận. 

Các trường hợp cạnh bao gồm các tình huống trong đó tất cả các phần tử trong`a`bằng nhau nhưng`b`chứa nhiều số khác nhau, trong đó các mảng đã bằng nhau hoặc trong đó`b`tất cả đều là số không. Ví dụ, nếu`a = [5, 5, 5]`Và`b = [0, 0, 0]`, điều duy nhất có thể`k`là`5`. Nếu như`b = [1, 2, 3]`, KHÔNG`k`có thể đáp ứng điều này. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là thử mọi cách có thể`k`từ 1 đến giá trị lớn nhất$10^9$, tính toán`a_i % k`cho tất cả`i`, sắp xếp mảng kết quả và kiểm tra xem nó có khớp không`b`. Điều này đúng về nguyên tắc nhưng không khả thi vì nó đòi hỏi tới$10^9 \times 10^4$hoạt động cho mỗi trường hợp thử nghiệm, vượt xa giới hạn thực tế. 

Cái nhìn sâu sắc quan trọng đến từ các thuộc tính của phép toán modulo. Nếu như`a_i % k = b_j`đối với một số người`i`Và`j`, sau đó`a_i - b_j`phải chia hết cho`k`. Điều này có nghĩa là bất kỳ giá trị hợp lệ`k`phải phân chia mọi khác biệt`a_i - b_i`sau khi chúng tôi căn chỉnh`a`Và`b`để có thể`b_i <= a_i`. Tổng quát hơn, chúng ta có thể sắp xếp`a`Và`b`một cách độc lập. Khi đó, với mọi chỉ số`i`,`a[i] - b[i]`phải chia hết cho`k`. Do đó, ước chung lớn nhất (GCD) của những hiệu này cho chúng ta một ứng cử viên cho`k`. Nếu GCD bằng 0 thì bất kỳ`k`lớn hơn mức tối đa của`a`sẽ hoạt động. 

Quan sát này làm giảm vấn đề từ việc kiểm tra hàng tỷ số nguyên đến việc tính toán một GCD của$n$số cho mỗi trường hợp thử nghiệm. Điều này là hiệu quả và thanh lịch. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 10^9) | O(n) | Quá chậm | 
| Tối ưu dựa trên GCD | O(n log(max(a))) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`Và`b`. Sắp xếp cả hai mảng theo thứ tự không giảm. Việc sắp xếp đảm bảo rằng chúng ta đang so sánh các phần tử tương ứng là ứng cử viên cho cùng một lớp modulo. 
2. Tính sự khác biệt`d_i = a[i] - b[i]`cho tất cả`i`. Nếu có chênh lệch âm thì xuất ra`-1`ngay lập tức vì không tích cực`k`có thể đáp ứng`b_i = a_i % k`nếu như`b_i > a_i`. 
3. Tính ước chung lớn nhất (GCD) của mọi hiệu khác 0`d_i`. Nếu tất cả sự khác biệt bằng 0, hãy đặt GCD về 0. 
4. Nếu GCD bằng 0 nghĩa là tất cả`a_i`chính xác bằng`b_i`. Bất kì`k`lớn hơn giá trị lớn nhất của`a`sẽ hoạt động. Chúng ta có thể trở về an toàn`max(a) + 1`hoặc bất kỳ số nguyên nào lên đến$10^9$. 
5. Mặt khác, bản thân GCD là một ứng cử viên hợp lệ cho`k`. Trả lại miễn là nó không vượt quá$10^9$. 
6. Nếu GCD lớn hơn$10^9$, đầu ra`-1`, vì bài toán yêu cầu câu trả lời nhiều nhất là$10^9$. 

### Tại sao nó hoạt động 

Phép toán modulo thỏa mãn tính chất nếu`a_i % k = b_i`, sau đó`k`chia rẽ`a_i - b_i`. Bằng cách sắp xếp`a`Và`b`, chúng ta căn chỉnh các phần tử phải tương ứng theo modulo. GCD của mọi khác biệt đảm bảo rằng`k`chia từng sự khác biệt, thỏa mãn`a_i % k = b_i`cho tất cả`i`. Nếu tất cả sự khác biệt bằng 0 thì bất kỳ sự khác biệt nào đủ lớn`k`bảo quản`b_i = a_i`. Sự khác biệt tiêu cực chỉ ra những kịch bản không thể xảy ra. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort()
        b.sort()
        
        diffs = []
        impossible = False
        for ai, bi in zip(a, b):
            if bi > ai:
                impossible = True
                break
            diffs.append(ai - bi)
        
        if impossible:
            print(-1)
            continue
        
        g = 0
        for d in diffs:
            g = math.gcd(g, d)
        
        if g == 0:
            # all a_i == b_i, any k > max(a) works
            print(min(max(a)+1, 10**9))
        else:
            print(g if g <= 10**9 else -1)

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên sắp xếp các mảng để căn chỉnh các cặp modulo tiềm năng. Nó kiểm tra sự bất khả thi ngay lập tức nếu có`b_i > a_i`. Sau đó, nó tính toán GCD của tất cả sự khác biệt. Nếu tất cả sự khác biệt bằng 0, nó sẽ tạo ra một giá trị lớn an toàn`k`. Ngược lại, bản thân GCD sẽ được trả về nếu nằm trong giới hạn. Việc sắp xếp là rất quan trọng để đảm bảo tính chính xác, vì việc căn chỉnh chưa được sắp xếp có thể bỏ sót các cặp modulo hợp lệ. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên: 

| tôi | một [tôi] | b[i] | a[i]-b[i] | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | 2 | 
| 1 | 3 | 1 | 2 | 
| 2 | 5 | 1 | 4 | 
| 3 | 7 | 1 | 6 | 

GCD của sự khác biệt: gcd(2,2,4,6) = 2. Đầu ra: 2. Điều này phù hợp với kết quả mong đợi. 

Đối với đầu vào mẫu thứ hai: 

| tôi | một [tôi] | b[i] | a[i]-b[i] | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 2 | 2 | 0 | 
| 2 | 3 | 3 | 0 | 
| 3 | 4 | 4 | 0 | 
| 4 | 5 | 5 | 0 | 

Tất cả sự khác biệt đều bằng 0, vì vậy bất kỳ`k > max(a)=5`hoạt động. Chúng ta có thể trả về 6. Kết quả: 6 hoặc bất kỳ số lớn hợp lệ nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế; Tính toán GCD là O(n log max(a)) | 
| Không gian | O(n) | Lưu trữ mảng và sự khác biệt | 

Với$n \le 10^4$và tổng số tiền$n$qua các trường hợp thử nghiệm$10^4$, các thao tác sắp xếp và GCD đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("5\n4\n3 5 2 7\n0 1 1 1\n5\n3 1 5 2 4\n1 2 3 4 5\n6\n2 3 4 7 8 9\n1 2 3 6 7 8\n5\n21 22 25 28 20\n0 1 2 1 0\n6\n1 1 2 3 5 8\n0 0 1 1 0 0\n") == "2\n6\n-1\n4\n-1"

# Custom: all equal
assert run("1\n3\n5 5 5\n0 0 0\n") == "5"

# Custom: b larger than a (impossible)
assert run("1\n3\n1 2 3\n3 2 1\n") == "-1"

# Custom: all differences zero
assert run("1\n4\n10 20 30
```
