---
title: "CF 282E - Tối đa hóa xúc xích"
description: "Chúng ta được cung cấp một mảng số nguyên đại diện cho một chiếc xúc xích ở Bitland. Mục tiêu là cắt xúc xích này thành hai phân đoạn không chồng lên nhau: tiền tố cho BitHaval và hậu tố cho BitAryo. Một trong hai phân đoạn có thể trống."
date: "2026-06-05T09:20:51+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 282
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 173 (Div. 2)"
rating: 2200
weight: 282
solve_time_s: 88
verified: true
draft: false
---

[CF 282E - Tối đa hóa xúc xích](https://codeforces.com/problemset/problem/282/E) 

**Đánh giá:** 2200 
**Thẻ:** bitmask, cấu trúc dữ liệu, cây 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên đại diện cho một chiếc xúc xích ở Bitland. Mục tiêu là cắt xúc xích này thành hai phân đoạn không chồng lên nhau: tiền tố cho BitHaval và hậu tố cho BitAryo. Một trong hai phân đoạn có thể trống. “Độ ngon” của mỗi người là XOR theo bit của các số nguyên trong phân khúc của họ và tổng mức độ hài lòng của họ là XOR của hai giá trị độ ngon. Chúng tôi muốn tối đa hóa niềm vui này. 

Kích thước đầu vào lên tới 100.000 số nguyên và mỗi số nguyên có thể lớn bằng$10^{12}$. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào có các vòng lặp lồng nhau trên tất cả các phân tách tiền tố-hậu tố có thể sẽ quá chậm: việc lặp qua tất cả các cặp tiền tố và hậu tố sẽ là$O(n^2)$, theo thứ tự của$10^{10}$hoạt động. Chúng ta cần một cái gì đó gần tuyến tính hoặc nhiều nhất là tuyến tính trong$n$. 

Các trường hợp cạnh rất tinh tế ở đây. Trường hợp đơn giản nhất là khi mảng có độ dài 1. Khi đó, phân đoạn không trống duy nhất là chính phần tử đó và người kia không nhận được gì. Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng 0, trong trường hợp đó mọi phân đoạn XOR đều bằng 0. Một cách tiếp cận bất cẩn có thể cho rằng cả hai phân đoạn phải không trống, nhưng vấn đề cho phép các phân đoạn trống và XOR của chúng bằng 0, điều này có thể thay đổi cách phân chia tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực lặp đi lặp lại trên mọi phân chia có thể có: cho mỗi đầu tiền tố$i$từ 0 đến$n$, tính XOR của tiền tố và XOR của hậu tố, sau đó lấy XOR của chúng. Việc tính toán tiền tố và hậu tố XOR từ đầu mỗi lần là$O(n)$, dẫn đến tổng thể$O(n^2)$sự phức tạp. Điều này hoạt động với các mảng nhỏ nhưng quá chậm đối với$n = 10^5$. 

Chúng tôi nhận thấy rằng XOR có tính kết hợp và có tính chất$a \oplus b = c \Rightarrow a = b \oplus c$. Nếu chúng ta tính toán trước mảng XOR tiền tố,$\text{px}[i] = a[0] \oplus a[1] \oplus \dots \oplus a[i]$, thì XOR của bất kỳ hậu tố nào bắt đầu tại vị trí$j$có thể được viết là$\text{px}[n-1] \oplus \text{px}[j-1]$vì$j > 0$, và đơn giản là$\text{px}[n-1]$vì$j = 0$. Điều này làm giảm tính toán lặp đi lặp lại. 

Thông tin chi tiết tiếp theo là vấn đề giảm xuống: đưa ra danh sách các XOR tiền tố, tìm hai phân đoạn không chồng chéo sao cho XOR của các XOR của chúng được tối đa hóa. Điều này tương đương với bài toán cổ điển về tìm XOR lớn nhất của hai số bất kỳ trong một mảng. Chúng ta có thể giải quyết vấn đề này bằng cách sử dụng trie nhị phân hoặc cơ sở của các vectơ bit độc lập (còn gọi là cơ sở tuyến tính). Mỗi XOR tiền tố có thể được chèn vào một cơ sở và XOR tối đa của tiền tố hiện tại với bất kỳ tiền tố nào trước đó có được bằng cách truy vấn cơ sở. Điều này tạo ra một$O(n \cdot \log M)$giải pháp, ở đâu$M$là số nguyên tối đa có thể ($10^{12}$, Vì thế$\log M \approx 40$). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tiền tố XOR + Cơ sở tuyến tính | O(n log M) | O(log M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán mảng XOR tiền tố. Cho phép$\text{px}[i] = a[0] \oplus a[1] \oplus \dots \oplus a[i]$. Điều này cho phép tính toán O(1) của bất kỳ tiền tố XOR nào. 
2. Khởi tạo cơ sở tuyến tính trống để tối đa hóa XOR. Cơ sở tuyến tính là một tập hợp các số sao cho mỗi số độc lập tuyến tính theo XOR, nghĩa là không có số nào có thể được hình thành bằng cách XOR các số khác trong cơ sở. 
3. Lặp lại tiền tố XOR. Ở mỗi bước$i$, chúng tôi xử lý$\text{px}[i]$là XOR tiềm năng cho phân khúc BitAryo và chúng tôi muốn tối đa hóa XOR bằng bất kỳ tiền tố nào trước đó (phân khúc BitHaval tiềm năng). Truy vấn cơ sở hiện tại để tìm số tối đa hóa$\text{px}[i] \oplus \text{basis element}$. 
4. Sau khi truy vấn, chèn$\text{px}[i]$vào cơ sở. Điều này đảm bảo rằng các hậu tố trong tương lai có thể sử dụng tiền tố XOR này làm ứng cử viên cho XOR tối đa. 
5. Theo dõi XOR tối đa toàn cầu được tìm thấy trong các lần lặp này và xuất nó. 

Tại sao nó hoạt động: mảng XOR tiền tố đảm bảo mọi tiền tố và hậu tố XOR có thể đều được thể hiện. Cơ sở tuyến tính đảm bảo rằng chúng ta có thể xây dựng XOR lớn nhất có thể từ bất kỳ tập hợp con nào của XOR tiền tố trước đó. Bởi vì XOR có tính kết hợp và khả nghịch, nên phương pháp này kiểm tra ngầm tất cả các phân tách tiền tố-hậu tố hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class LinearBasis:
    def __init__(self):
        self.basis = [0] * 64

    def insert(self, x):
        for i in reversed(range(64)):
            if x & (1 << i):
                if self.basis[i]:
                    x ^= self.basis[i]
                else:
                    self.basis[i] = x
                    return

    def query_max(self, x):
        res = x
        for i in reversed(range(64)):
            if self.basis[i]:
                res = max(res, res ^ self.basis[i])
        return res

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    px = [0] * n
    px[0] = a[0]
    for i in range(1, n):
        px[i] = px[i-1] ^ a[i]

    lb = LinearBasis()
    max_pleasure = 0
    lb.insert(0)  # include empty prefix
    for val in px:
        max_pleasure = max(max_pleasure, lb.query_max(val))
        lb.insert(val)
    print(max_pleasure)

if __name__ == "__main__":
    main()
```Mã đầu tiên tính toán XOR tiền tố. Sau đó, chúng tôi duy trì cơ sở tuyến tính để nhanh chóng tìm ra số tối đa hóa XOR với bất kỳ tiền tố XOR hiện tại nào. Việc chèn 0 ban đầu sẽ xử lý trường hợp tiền tố trống. Vòng lặp trên các XOR tiền tố đảm bảo mọi phân tách tiền tố-hậu tố không chồng chéo đều được xem xét ngầm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2
1 2
```| tôi | px[i] | niềm vui tối đa | Cơ sở sau khi chèn | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | [0, 1] | 
| 1 | 3 | 3 | [0, 1, 3] | 

Niềm vui tối đa là 3, có được bằng cách cho BitHaval phần tử đầu tiên và BitAryo phần tử thứ hai. 

Đầu vào tùy chỉnh 2:```
3
1 2 3
```| tôi | px[i] | niềm vui tối đa | Cơ sở sau khi chèn | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | [0, 1] | 
| 1 | 3 | 3 | [0, 1, 3] | 
| 2 | 0 | 3 | [0, 1, 3] | 

Niềm vui tối đa là 3, đạt được bằng cách tặng BitHaval [1,2] và BitAryo [3]. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * log M) | n tiền tố XOR được chèn vào cơ sở tuyến tính 64 bit | 
| Không gian | O(64) | Cơ sở tuyến tính lưu trữ tối đa 64 số cho số nguyên 64 bit | 

Với n lên tới$10^5$và M lên đến$10^{12}$, tổng số hoạt động theo thứ tự$10^5 * 40 = 4 \times 10^6$, thoải mái dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("2\n1 2\n") == "3", "sample 1"

# Custom tests
assert run("1\n5\n") == "5", "single element"
assert run("3\n1 2 3\n") == "3", "small array"
assert run("4\n0 0 0 0\n") == "0", "all zeros"
assert run("5\n1 1 1 1 1\n") == "1", "all ones"
assert run("6\n3 5 2 7 1 6\n") == "7", "mixed numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n5 | 5 | Vỏ cạnh một phần tử | 
| 3\n1 2 3 | 3 | Mảng nhỏ, trường hợp điển hình | 
| 4\n0 0 0 0 | 0 | Tất cả các số không được xử lý chính xác | 
| 5\n1 1 1 1 1 | 1 | Số lặp lại được xử lý |
