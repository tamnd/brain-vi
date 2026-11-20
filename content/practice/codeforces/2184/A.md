---
title: "CF 2184A - Thí nghiệm xã hội"
description: "Chúng tôi có $n$ người. Họ phải được chia thành các đội, mỗi đội có 2 hoặc 3 thành viên. Sau khi các đội được thành lập, mỗi đội độc lập chọn một trong hai nền văn minh."
date: "2026-06-07T21:36:15+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2184
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1072 (Div. 3)"
rating: 800
weight: 2184
solve_time_s: 108
verified: true
draft: false
---

[CF 2184A - Thử nghiệm xã hội](https://codeforces.com/problemset/problem/2184/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có$n$mọi người. Họ phải được chia thành các đội, mỗi đội có 2 hoặc 3 thành viên. Sau khi các đội được thành lập, mỗi đội độc lập chọn một trong hai nền văn minh. 

Số người được chỉ định vào một nền văn minh là tổng quy mô của tất cả các đội đã chọn nó. Chúng tôi muốn sắp xếp cả quy mô nhóm và các lựa chọn về nền văn minh sao cho sự khác biệt giữa hai quần thể nền văn minh càng nhỏ càng tốt. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi chỉ được cung cấp giá trị$n$, và chúng ta phải đưa ra mức chênh lệch tối thiểu có thể đạt được. 

Những hạn chế là rất nhỏ. Có tới$10^4$trường hợp thử nghiệm và mỗi$n$nhiều nhất là$10^4$. Thậm chí đơn giản$O(n)$giải pháp cho mỗi trường hợp thử nghiệm sẽ vượt qua một cách thoải mái. Tuy nhiên, cấu trúc của vấn đề cho phép$O(1)$giải pháp. 

Phần khó khăn là nhận ra rằng các nhóm chứ không phải từng cá nhân lựa chọn các nền văn minh. Chúng ta không thể tùy tiện phân bổ con người giữa hai nền văn minh. Mỗi đội đóng góp 2 hoặc 3 người cùng một lúc. 

Coi như$n=2$. Chỉ có một đội có quy mô 2. Một nền văn minh có 2 người và nền văn minh kia nhận được 0, vì vậy câu trả lời là 2. Một cách tiếp cận bất cẩn có thể cho rằng chúng ta có thể chia rẽ từng người và trả về 0 không chính xác. 

Hãy xem xét$n=5$. Chúng ta có thể thành lập các đội cỡ 2 và 3. Chỉ định đội cỡ 2 cho một nền văn minh và đội cỡ 3 cho nền văn minh kia. Sự khác biệt là 1. Bất kỳ giải pháp nào chỉ kiểm tra xem$n$chẵn hay lẻ sẽ bỏ lỡ điều này. 

Coi như$n=7$. Một phân vùng hợp lệ là$2+2+3$. Tổng số nền văn minh có thể trở thành$5$Và$2$, tạo ra sự khác biệt$3$, hoặc$4$Và$3$, tạo ra sự khác biệt$1$. Khả năng lựa chọn nhiệm vụ văn minh sau khi thành lập nhóm là điều cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các cách để thể hiện$n$bằng tổng của 2 và 3. Đối với mỗi đại diện, nó sẽ thử tất cả các nhiệm vụ có thể có của các đội cho hai nền văn minh và ghi lại sự khác biệt tối thiểu. 

Điều này có tác dụng vì mọi giải pháp hợp lệ đều tương ứng với một số phân tách nhóm và một số phân công nhóm. Vấn đề là số lượng các phép phân rã và phép gán tăng theo cấp số nhân. Ngay cả đối với các giá trị vừa phải của$n$, việc khám phá mọi khả năng trở nên không thực tế. 

Quan sát quan trọng là quy mô nhóm chỉ có 2 và 3. Vì$$3 - 2 = 1,$$chúng ta có thể xây dựng tổng số rất linh hoạt. Điều duy nhất quan trọng là phần còn lại của$n$modulo 2. 

Nếu$n$thậm chí, chúng ta luôn có thể chia đều dân số giữa hai nền văn minh. Một cách xây dựng đơn giản là chỉ thành lập các đội cỡ 2. Khi đó, mỗi nền văn minh có thể tiếp nhận chính xác một nửa số đội. Sự khác biệt trở thành 0. 

Nếu$n$là số lẻ, việc chia đều là không thể vì tổng dân số của nền văn minh phải bằng một số lẻ. Sự khác biệt tốt nhất có thể ít nhất là 1. 

Câu hỏi tiếp theo là liệu chênh lệch 1 có luôn đạt được đối với số lẻ hay không?$n$. 

Đối với mỗi lẻ$n \ge 5$, chúng ta có thể viết$$n = 3 + 2k.$$Tạo một nhóm có quy mô 3 và$k$các đội có quy mô 2. Vì phần còn lại$2k$mọi người tạo thành một tổng số chẵn, chúng ta có thể chia đều số đó. Đội ngũ bổ sung cỡ 3 đóng góp nhiều hơn một người cho nền văn minh này so với nền văn minh kia, tạo ra sự khác biệt đúng bằng 1. 

Giá trị lẻ duy nhất không phù hợp với cách xây dựng này là$n=3$. Với một đội cỡ 3, một nền văn minh có cả 3 người và nền văn minh kia có 0, nên sự khác biệt là 3. 

Điều này hoàn toàn xác định câu trả lời: 

-$n=3$cho 3. 
- Bất kỳ điều kỳ quặc nào khác$n$cho 1. 
- Dù sao đi nữa$n$cho 0, ngoại trừ$n=2$, mang lại 2. 

Những trường hợp này có thể được viết gọn hơn như sau$n \bmod 2$, ngoại trừ các giá trị đặc biệt$2$Và$3$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$. 
2. Nếu$n=2$, đầu ra 2. 

Chỉ có một đội cỡ 2 nên một nền văn minh tiếp nhận tất cả mọi người. 
3. Ngược lại, nếu$n=3$, đầu ra 3. 

Chỉ có một đội cỡ 3 nên một nền văn minh tiếp nhận tất cả mọi người. 
4. Ngược lại, xuất ra$n \bmod 2$. 

Nếu như$n$chẵn, tồn tại một sự phân chia hoàn hảo và câu trả lời là 0. 

Nếu$n$là số lẻ và ít nhất là 5, tồn tại sự phân chia có chênh lệch 1 và không thể có chênh lệch nhỏ hơn vì tổng dân số là số lẻ. 

### Tại sao nó hoạt động 

Thậm chí$n$, chỉ thành lập các đội cỡ 2 cho phép tổng dân số được chia đều giữa hai nền văn minh, do đó chênh lệch tối thiểu là 0. 

Đối với số lẻ$n \ge 5$, viết$n=3+2k$. Chỉ định đội cỡ 3 sang một bên và chia đều các đội cỡ 2. Các quần thể của nền văn minh khác nhau đúng 1. Vì tổng dân số là số lẻ nên không thể tồn tại chênh lệch bằng 0. 

Những trường hợp$n=2$Và$n=3$là đặc biệt vì chỉ có thể thành lập một đội. Một nền văn minh tiếp nhận tất cả mọi người và nền văn minh kia không tiếp nhận ai cả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

ans = []
for _ in range(t):
    n = int(input())

    if n == 2:
        ans.append("2")
    elif n == 3:
        ans.append("3")
    else:
        ans.append(str(n % 2))

sys.stdout.write("\n".join(ans))
```Hai điều kiện đầu tiên xử lý các giá trị ngoại lệ duy nhất. Vì$n=2$Và$n=3$, có chính xác một đội, vì vậy sự khác biệt bằng chính quy mô của đội. 

Đối với mọi giá trị lớn hơn, tính chẵn lẻ hoàn toàn xác định câu trả lời. Các giá trị chẵn trả về 0 vì có thể chia đều. Các giá trị lẻ trả về 1 vì tổng dân số là số lẻ, loại trừ chênh lệch 0 và việc xây dựng sử dụng một nhóm cỡ 3 đạt được chênh lệch 1. 

Không tồn tại mối lo ngại về tràn số học vì tất cả các giá trị đều rất nhỏ. Việc triển khai chỉ thực hiện một vài thao tác liên tục trong mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2
3
5
8
11
```| n | Trường hợp đặc biệt? | n % 2 | Trả lời | 
| --- | --- | --- | --- | 
| 2 | Có | 0 | 2 | 
| 3 | Có | 1 | 3 | 
| 5 | Không | 1 | 1 | 
| 8 | Không | 0 | 0 | 
| 11 | Không | 1 | 1 | 

Vì$2$Và$3$, chỉ có một đội tồn tại nên các quy tắc đặc biệt sẽ được áp dụng. Đối với các giá trị lớn hơn, chỉ riêng tính chẵn lẻ sẽ xác định kết quả. 

### Ví dụ 2 

đầu vào:```
3
5
12
13
```| n | Trường hợp đặc biệt? | n % 2 | Trả lời | 
| --- | --- | --- | --- | 
| 5 | Không | 1 | 1 | 
| 12 | Không | 0 | 0 | 
| 13 | Không | 1 | 1 | 

Vì$12$, có thể đạt được sự phân chia bằng nhau. Vì$5$Và$13$, tổng dân số là số lẻ, vì vậy chênh lệch tốt nhất có thể là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài so sánh và một phép toán modulo | 
| Không gian | O(1) | Sử dụng một lượng bộ nhớ bổ sung không đổi | 

Với nhiều nhất$10^4$trường hợp thử nghiệm, tổng công việc là không đáng kể. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        if n == 2:
            ans.append("2")
        elif n == 3:
            ans.append("3")
        else:
            ans.append(str(n % 2))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run("3\n2\n5\n12\n") == "2\n1\n0"

# minimum valid values
assert run("2\n2\n3\n") == "2\n3"

# smallest odd value with answer 1
assert run("1\n5\n") == "1"

# even value
assert run("1\n8\n") == "0"

# maximum constraint
assert run("1\n10000\n") == "0"

# large odd value
assert run("1\n9999\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2, 3`|`2, 3`| Cả hai trường hợp đặc biệt | 
|`5`|`1`| Giá trị lẻ đầu tiên có thể đạt được chênh lệch 1 | 
|`8`|`0`| Trường hợp chẵn tiêu chuẩn | 
|`10000`|`0`| Đầu vào chẵn tối đa | 
|`9999`|`1`| Hành vi kiểu kỳ quặc tối đa | 

## Vỏ cạnh 

Đối với đầu vào```
1
2
```thuật toán đi vào nhánh trường hợp đặc biệt đầu tiên và trả về 2. Chỉ có một nhóm có kích thước 2, vì vậy quần thể của nền văn minh là$2$Và$0$. Sự khác biệt chính xác là 2. 

Đối với đầu vào```
1
3
```thuật toán đi vào nhánh trường hợp đặc biệt thứ hai và trả về 3. Chỉ có một nhóm có kích thước 3, vì vậy quần thể của nền văn minh là$3$Và$0$. 

Đối với đầu vào```
1
5
```thuật toán đạt đến quy tắc chung và trả về$5 \bmod 2 = 1$. Một công trình hợp lệ là các đội cỡ 2 và 3 được giao cho các nền văn minh khác nhau, tạo ra các quần thể$2$Và$3$. 

Đối với đầu vào```
1
12
```thuật toán trả về$12 \bmod 2 = 0$. Chúng ta có thể thành lập sáu đội cỡ 2 và cử ba đội đến mỗi nền văn minh. Cả hai nền văn minh đều tiếp nhận 6 người, chênh lệch là 0.
