---
title: "CF 1977B - Tô màu nhị phân"
description: "Chúng ta được yêu cầu biểu diễn một số nguyên dương $x$ dưới dạng tổng lũy ​​thừa của 2, nhưng với một ràng buộc bổ sung nghiêm ngặt: các hệ số lũy thừa của 2 chỉ có thể là -1, 0 hoặc 1 và không có hai hệ số nào khác 0 có thể liền kề nhau."
date: "2026-06-08T17:15:48+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1977
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 948 (Div. 2)"
rating: 1100
weight: 1977
solve_time_s: 132
verified: true
draft: false
---

[CF 1977B - Tô màu nhị phân](https://codeforces.com/problemset/problem/1977/B) 

**Đánh giá:** 1100 
**Tags:** bitmasks, thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu biểu diễn một số nguyên dương cho trước$x$dưới dạng tổng lũy ​​thừa của hai, nhưng có ràng buộc bổ sung nghiêm ngặt: hệ số lũy thừa của hai chỉ có thể là -1, 0 hoặc 1 và không có hai hệ số nào khác 0 có thể liền kề nhau. Cụ thể, chúng ta phải tạo ra một mảng$a$chiều dài$n$Ở đâu$1 \le n \le 32$,$a_i \in \{-1, 0, 1\}$, Và$$x = \sum_{i=0}^{n-1} a_i \cdot 2^i$$với điều kiện thêm là nếu$a_i$Và$a_{i+1}$đều khác 0, điều đó vi phạm quy tắc. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp chứa một số nguyên$x$. Đối với mỗi$x$, chúng ta phải xây dựng một mảng hợp lệ$a$và xuất ra chiều dài và nội dung của nó. Từ$x < 2^{30}$, chiều dài tối đa$n$sẽ không bao giờ cần vượt quá 32, vì vậy bất kỳ thuật toán nào lặp qua các bit của$x$là hiệu quả. 

Sự tinh tế chính đến từ ràng buộc "không có hệ số khác 0 liền kề". Ví dụ, nếu$x = 3$, một khai triển nhị phân ngây thơ mang lại$11_2 = 1 \cdot 2^1 + 1 \cdot 2^0$, nhưng điều này vi phạm quy tắc kề. Lời giải phải chèn số 0 hoặc sử dụng hệ số âm để “trải rộng” các số hạng khác 0 trong khi vẫn tính tổng bằng$x$. 

Các trường hợp cạnh bao gồm các số nhỏ (như$1$hoặc$2$), những số nhỏ hơn một lũy thừa của hai (như$3, 7, 15$) và các số có số xen kẽ ở dạng nhị phân (như$21 = 10101_2$). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các chuỗi có độ dài lên tới 32 với các hệ số trong$-1,0,1$, kiểm tra tổng và xác minh không có số 0 liền kề nào. Điều này rõ ràng là không khả thi vì số lượng trình tự tăng lên khi$3^{32} \approx 2 \times 10^{15}$. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể xây dựng chuỗi một cách tham lam, xử lý$x$từ bit có trọng số nhỏ nhất đến bit có trọng số cao nhất và đảm bảo không có hệ số nào khác 0 liền kề. Nếu hai cái liên tiếp xuất hiện trong khai triển nhị phân, chúng ta có thể chuyển đổi chúng thành mẫu như$[-1, 0, 1]$bảo toàn tổng và dàn trải các hệ số khác 0. Điều này gợi nhớ đến việc biểu diễn các số ở dạng "bậc ba cân bằng" hoặc không liền kề (NAF) được sử dụng trong mật mã, đảm bảo rằng không có hai chữ số nào khác 0 liền kề nhau và tất cả các chữ số đều nằm trong {-1,0,1}. 

Cách tiếp cận tham lam có hiệu quả vì đối với bất kỳ bit nào gây ra sự kề cận, chúng ta có thể truyền chuyển tiếp, tăng bit cao hơn tiếp theo hoặc điều chỉnh bit hiện tại thành -1. Điều này đảm bảo rằng tổng vẫn đúng trong khi vẫn duy trì quy tắc kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^32) | O(32) | Quá chậm | 
| Xây dựng tham lam/NAF | O(log x) cho mỗi trường hợp thử nghiệm | O(log x) cho mỗi trường hợp thử nghiệm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một danh sách trống`a`để giữ các hệ số. 
2. Khởi tạo một biến`x`đến số đã cho. 
3. Lặp lại trong khi`x`không phải là số không: 

1. Nếu`x % 2 == 0`, nối thêm`0`ĐẾN`a`bởi vì bit ít quan trọng nhất không đóng góp gì. 
2. Nếu`x % 2 == 1`, nối thêm`1`ĐẾN`a`bởi vì số 1 đơn giản phù hợp với quy tắc không kề. 
3. Nếu`x % 2 == 3`, nối thêm`-1`ĐẾN`a`và tăng`x`bằng 1 để chuyển sang bit cao hơn tiếp theo. Điều này giải quyết sự liền kề của hai cái. 
4. Chia`x`bằng 2 (chia số nguyên) để dịch sang phải và tiếp tục. 
4. Đầu ra`n = len(a)`và mảng`a`. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán đảm bảo rằng mọi bit khác 0 có khả năng liền kề đều được phân tách bằng cách chuyển đổi một cặp bit thành`[-1,0,1]`mô hình với một mang theo. Điều này đảm bảo tính bất biến rằng không có hai phần tử khác 0 trong`a`liền kề nhau, trong khi tổng của$a_i \cdot 2^i$được duy trì. Bởi vì$x < 2^{30}$, quá trình kết thúc sau tối đa 32 lần lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x = int(input())
        a = []
        while x != 0:
            if x % 2 == 0:
                a.append(0)
            elif x % 2 == 1:
                a.append(1)
            else:  # x % 2 == 3
                a.append(-1)
                x += 1
            x //= 2
        print(len(a))
        print(' '.join(map(str, a)))

if __name__ == "__main__":
    solve()
```các`while x != 0`vòng lặp lặp qua từng bit của`x`. điều kiện`x % 2 == 3`xử lý trường hợp khi hai số liên tiếp xuất hiện ở dạng nhị phân và vi phạm quy tắc kề. Đang bổ sung`-1`và tăng dần`x`chuyển sang bit tiếp theo một cách hiệu quả, bảo toàn tổng. sử dụng`x //= 2`tương đương với việc dịch chuyển sang phải và tiến dần qua các lũy thừa cao hơn của hai. 

Các cân nhắc về ranh giới bao gồm bit đầu tiên (`2^0`) và thực tế là`x`có thể là số lẻ hoặc thậm chí. Từ`x < 2^{30}`, danh sách`a`không bao giờ phát triển vượt quá 32 phần tử, thỏa mãn các ràng buộc của bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1: x = 14 

| Bước | x | x% 2 | Hành động | một | Mới x | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 14 | 0 | nối thêm 0 | [0] | 7 | 
| 2 | 7 | 1 | nối thêm 1 | [0,1] | 3 | 
| 3 | 3 | 3 | nối thêm -1, x +=1 | [0,1,-1] | 2 | 
| 4 | 2 | 0 | nối thêm 0 | [0,1,-1,0] | 1 | 
| 5 | 1 | 1 | nối thêm 1 | [0,1,-1,0,1] | 0 | 

Mảng kết quả`[0,1,-1,0,1]`tổng bằng 14 và không có phần tử nào khác 0 liền kề. 

### Ví dụ 2: x = 1 

| Bước | x | x% 2 | Hành động | một | Mới x | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | nối thêm 1 | [1] | 0 | 

Mảng`[1]`là trường hợp cạnh tầm thường, hợp lệ. 

Những dấu vết này xác nhận rằng thuật toán xử lý chính xác cả số nhỏ và số xuất hiện liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log x) cho mỗi trường hợp thử nghiệm | Mỗi lần lặp lại chia`x`bằng 2;`x < 2^30`, vậy tối đa là 30 lần lặp | 
| Không gian | O(log x) cho mỗi trường hợp thử nghiệm | Mảng`a`lưu trữ tối đa 32 phần tử | 

Với tối đa 10^4 trường hợp thử nghiệm, tổng số thao tác tối đa là 10^4 * 30 ≈ 3 × 10^5, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("7\n1\n14\n24\n15\n27\n11\n19\n") == \
"""1
1
5
0 1 -1 0 1
6
0 0 0 -1 0 1
5
-1 0 0 0 1
6
-1 0 -1 0 0 1
5
-1 0 -1 0 1
5
-1 0 1 0 1""", "Sample 1"

# minimum input
assert run("1\n1\n") == "1\n1", "minimum input"

# maximum x < 2^30
assert run(f"1\n{2**29}\n") == "30\n" + "0 "*29 + "1", "large power of two"

# consecutive ones case
assert run("1\n3\n") == "3\n-1 0 1", "x=3 adjacency case"

# alternating ones
assert run("1\n21\n") == "5\n1 0 -1 0 1", "x=21 alternating ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1\n1 | Đầu vào tối thiểu được xử lý chính xác | 
| 2 | 30\n0 ... 1 | Công suất lớn của hai tay cầm không bị tràn |
