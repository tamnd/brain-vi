---
title: "CF 1931D - Cặp chia được"
description: "Chúng ta được cho một mảng các số nguyên và hai số $x$ và $y$. Một cặp chỉ số $(i, j)$ với $i < j$ được coi là \"đẹp\" nếu tổng của hai phần tử $ai + aj$ chia hết cho $x$ và hiệu $ai - aj$ chia hết cho $y$."
date: "2026-06-08T18:28:04+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1931
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 925 (Div. 3)"
rating: 1300
weight: 1931
solve_time_s: 210
verified: true
draft: false
---

[CF 1931D - Cặp chia hết](https://codeforces.com/problemset/problem/1931/D) 

**Đánh giá:** 1300 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và hai số,$x$Và$y$. Cặp chỉ số$(i, j)$với$i < j$được coi là "đẹp" nếu tổng của hai phần tử$a_i + a_j$chia hết cho$x$và sự khác biệt$a_i - a_j$chia hết cho$y$. Đối với mỗi trường hợp thử nghiệm, chúng ta cần đếm xem tồn tại bao nhiêu cặp như vậy. 

Các ràng buộc rất quan trọng để giải thích chính xác. Mỗi mảng có thể lên tới$2 \cdot 10^5$các phần tử và có thể có tới$10^4$trường hợp thử nghiệm. Tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, vì vậy chúng ta có thể điều trị$n$hiệu quả$2 \cdot 10^5$mỗi đợt. Một cách tiếp cận mạnh mẽ để kiểm tra trực tiếp tất cả các cặp sẽ yêu cầu$O(n^2)$hoạt động, có thể lớn như$4 \cdot 10^{10}$và quá chậm so với giới hạn 2 giây. Chúng ta cần một giải pháp có quy mô tuyến tính hoặc gần tuyến tính với$n$. 

Một trường hợp đặc biệt có thể phá vỡ các lời giải ngây thơ là khi tất cả các số đều bằng nhau hoặc khi$x$hoặc$y$chia sự khác biệt một cách tầm thường. Ví dụ, nếu$x = y = 1$, cặp nào cũng đẹp. Việc triển khai bất cẩn có thể cố gắng thực hiện các phép tính modulo mà không xem xét các số âm, dẫn đến các lỗi sai lệch hoặc ký hiệu nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ lặp đi lặp lại trên mỗi cặp$(i, j)$và kiểm tra hai điều kiện chia hết. Điều này đúng vì nó trực tiếp thực hiện phát biểu vấn đề, nhưng nó đòi hỏi$O(n^2)$séc. Vì$n \approx 2 \cdot 10^5$, điều này sẽ tạo ra khoảng$2 \cdot 10^10$lặp đi lặp lại, vượt xa giới hạn tính toán khả thi. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa xuất phát từ việc xem xét các điều kiện đồng dạng modulo$x$Và$y$. Cho phép$a_i + a_j \equiv 0 \pmod{x}$Và$a_i - a_j \equiv 0 \pmod{y}$. Ta có thể giải đồng thời hai phương trình này. Cộng và trừ chúng cho thấy rằng$2a_i \equiv 0 \pmod{\text{lcm}(x, y)}$Và$2a_j \equiv 0 \pmod{\text{lcm}(x, y)}$lên đến một số ca. Cụ thể, chúng ta có thể chuyển bài toán sang việc đếm xem có bao nhiêu số trước đó$a_j$thỏa mãn sự đồng đẳng tuyến tính so với số hiện tại$a_i$, điều này có thể được thực hiện bằng bản đồ băm (từ điển) của dư lượng modulo một mô đun dẫn xuất. Điều này làm giảm việc kiểm tra cặp từ bậc hai sang tuyến tính theo chiều dài mảng. 

Câu chuyện ở đây là các điều kiện tổng và hiệu thoạt nhìn có vẻ phức tạp, nhưng bằng cách diễn đạt lại chúng dưới dạng số học mô-đun, chúng trở thành một vấn đề khi đếm các số dư phù hợp. Việc sử dụng từ điển để lưu trữ tần số của từng phần dư cho phép chúng ta đếm dần các cặp đẹp khi chúng ta lặp lại mảng chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Bản đồ băm mô-đun | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước ước chung lớn nhất (gcd) của$x$Và$y$, điều này sẽ giúp giảm hai sự đồng dư thành một mô đun duy nhất. Cho phép$g = \gcd(x, y)$. 
2. Lặp lại mảng từ trái sang phải. Đối với mỗi phần tử$a_i$, tính hai giá trị:$r_i = a_i \bmod x$Và$s_i = a_i \bmod y$. Chúng đại diện cho phần còn lại của phần tử hiện tại so với$x$Và$y$. 
3. Xây dựng khóa dư kết hợp$(r_i, s_i)$và lưu trữ bản đồ tần số về số lần mỗi phím như vậy xuất hiện. Điều này cho phép kiểm tra xem có bao nhiêu phần tử trước đó thỏa mãn các điều kiện mô đun cần thiết để tạo thành một cặp đẹp với$a_i$. 
4. Đối với mỗi phần tử$a_i$, tính dư lượng bổ sung tương ứng$(r_j, s_j)$điều đó sẽ làm cho cặp đôi$(a_j, a_i)$đẹp, sử dụng công thức rút ra từ sự đồng dư. Thêm tần số của phím đó trong bản đồ vào câu trả lời. 
5. Sau khi xử lý$a_i$, tăng khóa của nó trong bản đồ tần số để các phần tử trong tương lai có thể tạo thành cặp với nó. 

Tại sao nó hoạt động: Ở mỗi bước, bản đồ lưu trữ số lượng tất cả các phần tử đã thấy trước đó được nhóm theo modulo dư lượng của chúng$x$Và$y$. Mỗi khi chúng tôi gặp một phần tử mới, chúng tôi tính toán chính xác phần dư nào có thể ghép với nó để đáp ứng các điều kiện về tổng và hiệu. Vì chúng tôi chỉ lặp lại một lần và bản đồ theo dõi tất cả các phần tử có liên quan trước đó nên chúng tôi đếm chính xác mọi cặp hợp lệ một lần mà không kiểm tra các cặp dự phòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict
from math import gcd

def count_beautiful_pairs(n, x, y, a):
    freq = defaultdict(int)
    ans = 0
    g = gcd(x, y)
    lcm = x * y // g
    
    for val in a:
        # target residues for previous elements
        target = ((-val) % x, (val) % y)
        ans += freq[target]
        # current residue key
        key = (val % x, val % y)
        freq[key] += 1
    return ans

t = int(input())
for _ in range(t):
    n, x, y = map(int, input().split())
    a = list(map(int, input().split()))
    print(count_beautiful_pairs(n, x, y, a))
```Giải pháp xây dựng từ điển tần số được khóa theo cặp dư lượng modulo$x$Và$y$. Đối với mỗi phần tử, chúng tôi tính toán phần dư thỏa mãn các ràng buộc về tổng và hiệu với các phần tử trước đó. Tăng tần số sau khi đếm đảm bảo rằng không có cặp nào được tính hai lần. Việc sử dụng`%`đảm bảo xử lý chính xác các số âm, điều này rất tinh tế vì toán tử modulo của Python luôn trả về kết quả không âm. 

## Ví dụ đã hoạt động 

Lấy đầu vào`6 5 2; 1 2 7 4 9 6`. Lặp qua mảng: 

| tôi | một [tôi] | giá trị%x | giá trị%y | mục tiêu | bản đồ tần số | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | (4,1) | {} | 0 | 
| 1 | 2 | 2 | 0 | (3,0) | {(1,1):1} | 0 | 
| 2 | 7 | 2 | 1 | (3,1) | {(1,1):1,(2,0):1} | 0 | 
| 3 | 4 | 4 | 0 | (1,0) | ... | 0 | 
| 4 | 9 | 4 | 1 | (1,1) | ... | 1 | 
| 5 | 6 | 1 | 0 | (4,0) | ... | 1 | 

Câu trả lời cuối cùng là 2. Bảng này cho thấy rằng chỉ khi khóa mục tiêu tồn tại trong bản đồ tần số thì số đếm mới tăng lên, xác nhận ánh xạ mô-đun nắm bắt chính xác các cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần và các thao tác từ điển được khấu hao O(1) | 
| Không gian | O(n) | Từ điển tần số lưu trữ tối đa n cặp dư lượng riêng biệt | 

Với$n$lên đến$2 \cdot 10^5$và đạt tuyến tính cho mỗi trường hợp thử nghiệm, giải pháp chạy thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided samples
assert run("7\n6 5 2\n1 2 7 4 9 6\n7 9 5\n1 10 15 3 8 12 15\n9 4 10\n14 10 2 2 11 11 13 5 6\n9 5 6\n10 7 6 7 9 7 7 10 10\n9 6 2\n4 9 7 1 2 2 13 3 15\n9 2 3\n14 6 1 15 12 15 8 2 15\n10 5 7\n13 3 3 2 12 11 3 7 13 14") == "2\n0\n1\n3\n5\n7\n0"

# minimum input
assert run("1\n2 1 1\n1 1") == "1"

# all equal
assert run("1\n5 2 3\n6 6 6 6 6") == "10"

# no valid pairs
assert run("1\n3 7 5\n1 2 3") == "0"

# large numbers
assert run("1\n3 1000000000 999999
```
