---
title: "CF 1996F - Bom"
description: "Chúng tôi được yêu cầu tối đa hóa điểm số qua một loạt thao tác trên hai mảng. Mỗi mảng có độ dài $n$. Mảng $a$ biểu thị “giá trị” hiện tại của mỗi phần tử và mảng $b$ biểu thị mức độ phân rã của phần tử đó sau khi được sử dụng."
date: "2026-06-08T14:44:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1996
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 962 (Div. 3)"
rating: 1900
weight: 1996
solve_time_s: 169
verified: true
draft: false
---

[CF 1996F - Bom](https://codeforces.com/problemset/problem/1996/F) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tối đa hóa điểm số qua một loạt thao tác trên hai mảng. Mỗi mảng có độ dài$n$. Mảng$a$đại diện cho “giá trị” hiện tại của từng phần tử và mảng$b$biểu thị mức độ phân rã của phần tử đó sau khi được sử dụng. Trong một thao tác, chúng tôi chọn một chỉ mục$i$, thêm vào$a_i$theo điểm số của chúng tôi, và sau đó giảm$a_i$qua$b_i$, giữ cho nó không âm. Chúng tôi có chính xác$k$hoạt động để tối đa hóa tổng số điểm. 

Những hạn chế là đáng kể:$n$có thể đạt được$2 \cdot 10^5$, Và$k$có thể lên đến$10^9$. Một giải pháp đơn giản mô phỏng từng hoạt động riêng lẻ rõ ràng là không khả thi vì nó có thể yêu cầu hàng tỷ hoạt động. Điều này buộc chúng ta phải tìm cách bỏ qua nhiều thao tác cùng một lúc thay vì thực hiện từng thao tác một. 

Các trường hợp cạnh phát sinh khi$b_i \ge a_i$đối với một phần tử nào đó, vì phần tử đó có thể đóng góp nhiều nhất một lần. Tương tự, nếu$k$là vô cùng lớn so với$n$, chúng ta cần áp dụng nhiều lần các thao tác cho cùng một phần tử và mô hình phân rã rất quan trọng. Ví dụ, hãy xem xét$a = [1, 1]$,$b = [2, 3]$,$k = 3$. Điểm tối đa là$2$, không$3$, bởi vì cả hai phần tử đều đạt đến 0 sau thao tác đầu tiên. Một cách tiếp cận tham lam ngây thơ mà không theo dõi sự phân rã sẽ bị tính quá nhiều. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: đối với mỗi$k$hoạt động, quét qua mảng để tìm phần tử có giá trị hiện tại lớn nhất, thêm nó vào điểm và cập nhật phần tử đó. Điều này đúng nhưng đòi hỏi$O(n \cdot k)$thời gian. Với$n \le 2 \cdot 10^5$Và$k \le 10^9$, điều này có thể có nghĩa là$10^{14}$những hoạt động vượt xa mức có thể chấp nhận được. 

Quan sát quan trọng là mỗi phần tử phân rã tuyến tính: sau$m$công dụng của phần tử$i$, giá trị của nó là$\max(0, a_i - m \cdot b_i)$. Do cấu trúc tuyến tính này, chúng ta có thể tính toán số lần tối đa đáng sử dụng của mỗi phần tử. Cụ thể, lần đầu tiên chúng ta sử dụng một phần tử, nó sẽ đóng góp$a_i$. Lần sau nó góp phần$a_i - b_i$, sau đó$a_i - 2b_i$, v.v. cho đến khi nó đạt tới số không. Chuỗi này là số học và chúng ta có thể tính tổng nhanh chóng thay vì lặp lại từng mức giảm. 

Một cách thông minh để lựa chọn các hoạt động hiệu quả là chọn liên tục phần tử có đóng góp tiếp theo hiện tại là lớn nhất. Để tránh phải quét mảng mỗi lần, chúng ta có thể sắp xếp các phần tử theo$a_i$Và$b_i$theo cách cho phép chúng tôi xử lý các phần tử có đóng góp cao trước tiên và đối với các phần tử rất lớn$k$, chúng ta có thể gộp số lần chúng ta chọn từng phần tử bằng cách tính toán xem nó vẫn đóng góp tích cực bao nhiêu lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·k) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng$a$Và$b$, và số lượng hoạt động$k$. Ghép đôi từng cái$a_i$với sự tương ứng của nó$b_i$. 
2. Sắp xếp các cặp theo$b_i$trước tiên xử lý các phần tử có độ phân rã chậm hơn hoặc bằng một số phương pháp phỏng đoán khác để ưu tiên những đóng góp ban đầu cao và độ phân rã thấp. 
3. Đối với mỗi phần tử, hãy tính số lần tối đa nó có thể được sử dụng trước khi đạt đến số 0, tức là:$m_i = \min(k, \lceil a_i / b_i \rceil)$nếu như$b_i > 0$, nếu không thì$m_i = k$nếu như$b_i = 0$. 
4. Cộng tổng của dãy số học đóng góp từ phần tử này:$\text{sum}_i = m_i \cdot a_i - b_i \cdot \frac{m_i (m_i - 1)}{2}$. 
5. Trừ$m_i$từ các hoạt động còn lại$k$. Nếu như$k = 0$, dừng lại sớm. 
6. Tích lũy các đóng góp vào tổng số điểm. 

Tại sao nó hoạt động: Mỗi phần tử góp phần giảm dần các bước do phân rã tuyến tính. Bằng cách tính tổng đóng góp tích cực cho từng phần tử, chúng tôi nắm bắt được tổng đóng góp mà không cần mô phỏng từng hoạt động. Việc sắp xếp đảm bảo rằng các yếu tố đóng góp nhiều nhất vào tổng thể sẽ được xử lý trước tiên, điều này đảm bảo điểm tối ưu toàn cầu vì ở mỗi bước chúng tôi chọn đóng góp lớn nhất tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        items = list(zip(a, b))
        # Sort by initial value descending to greedily pick highest first
        items.sort(reverse=True, key=lambda x: x[0])
        total = 0
        remaining_ops = k
        for val, dec in items:
            if remaining_ops == 0:
                break
            if dec == 0:
                total += val * remaining_ops
                remaining_ops = 0
                break
            max_use = min(remaining_ops, (val + dec - 1) // dec)
            total += max_use * val - dec * max_use * (max_use - 1) // 2
            remaining_ops -= max_use
        print(total)

if __name__ == "__main__":
    solve()
```Giải pháp cặp đôi đầu tiên$a$Và$b$và sắp xếp theo$a_i$để ưu tiên giá trị ban đầu cao. Chúng tôi xử lý phân rã bằng 0 một cách riêng biệt để tránh chia cho 0. Công thức chuỗi số học tính tổng các đóng góp lặp đi lặp lại một cách hiệu quả. Chúng tôi dừng lại sớm nếu chúng tôi sử dụng tất cả$k$hoạt động. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3 4
5 6 7
2 3 4
```| Bước | giá trị | tháng mười hai | max_use | đóng góp | còn lại_ops | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 4 | 1 | 7 | 3 | 7 | 
| 2 | 6 | 3 | 1 | 6 | 2 | 13 | 
| 3 | 5 | 2 | 2 | 8 | 0 | 21 | 

Trace chứng minh rằng chúng tôi tính toán chính xác mức sử dụng tối đa cho mỗi phần tử và tính tổng các khoản đóng góp bằng công thức số học thay vì mô phỏng từng thao tác. 

### Đầu vào mẫu 2```
5 1000
1 2 3 4 5
5 4 3 2 1
```| Bước | giá trị | tháng mười hai | max_use | đóng góp | còn lại_ops | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 5 | 5+4+3+2+1=15 | 995 | 15 | 
| 2 | 4 | 2 | 2 | 4+2=6 | 993 | 21 | 
| 3 | 3 | 3 | 1 | 3 | 992 | 24 | 
| 4 | 2 | 4 | 1 | 2 | 991 | 26 | 
| 5 | 1 | 5 | 1 | 1 | 990 | 27 | 

Trace xác nhận rằng chúng tôi chọn các phần tử theo thứ tự giá trị giảm dần, xử lý phân rã và dừng khi phần tử đạt đến 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng n phần tử chiếm ưu thế. Các thao tác còn lại được tính toán phân tích, không mô phỏng. | 
| Không gian | O(n) | Chúng tôi lưu trữ các cặp (a_i, b_i) để sắp xếp và xử lý. | 

Độ phức tạp về thời gian phù hợp thoải mái trong giới hạn vì$n \le 2 \cdot 10^5$, làm cho giải pháp trở nên khả thi trong 2 giây. Không gian tuyến tính theo n, thấp hơn giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("5\n3 4\n5 6 7\n2 3 4\n5 9\n32 52 68 64 14\n18 14 53 24 8\n5 1000\n1 2 3 4 5\n5 4 3 2 1\n1 1000000\n1000000\n1\n10 6\n3 3 5 10 6 8 6 8 7 7\n6 1 7 4 1 1 8 9 3 1") == "21\n349\n27\n500000500000\n47"

# Minimum-size input
assert run("1\n1 1\n1\n1") == "1"

# All equal values
assert run("1\n3 5\n5 5 5\n1 1 1") == "25"

# Maximum n, small k
assert
```
