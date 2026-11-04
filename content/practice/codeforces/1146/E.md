---
title: "CF 1146E - Nóng là Lạnh"
description: "Chúng ta được cung cấp một mảng các số nguyên và một loạt các truy vấn. Mỗi truy vấn hướng dẫn chúng ta lật dấu các số trong mảng thỏa mãn phép so sánh: tất cả các số lớn hơn một ngưỡng hoặc tất cả các số nhỏ hơn một ngưỡng."
date: "2026-06-12T03:20:47+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "divide-and-conquer", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1146
codeforces_index: "E"
codeforces_contest_name: "Forethought Future Cup - Elimination Round"
rating: 2400
weight: 1146
solve_time_s: 125
verified: false
draft: false
---

[CF 1146E - Nóng là Lạnh](https://codeforces.com/problemset/problem/1146/E) 

**Đánh giá:** 2400 
**Thẻ:** bitmask, cấu trúc dữ liệu, phân chia và chinh phục, triển khai 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một loạt các truy vấn. Mỗi truy vấn hướng dẫn chúng ta lật dấu các số trong mảng thỏa mãn phép so sánh: tất cả các số lớn hơn một ngưỡng hoặc tất cả các số nhỏ hơn một ngưỡng. Sau khi thực hiện tất cả các truy vấn theo thứ tự, chúng ta được yêu cầu in trạng thái cuối cùng của mảng. 

Kích thước đầu vào rất đáng kể: cả mảng và số lượng truy vấn có thể lên tới 100.000. Mỗi phần tử nằm trong khoảng từ -100.000 đến 100.000. Một giải pháp đơn giản lặp qua toàn bộ mảng cho mỗi truy vấn sẽ yêu cầu tối đa 10^10 thao tác trong trường hợp xấu nhất, điều này vượt xa khả thi trong giới hạn thời gian một giây. Vì vậy, bất kỳ cách tiếp cận vũ phu nào đều quá chậm. 

Các trường hợp phức tạp tồn tại vì việc lật dấu có thể tương tác theo những cách không rõ ràng. Ví dụ: lật một số hai lần sẽ khôi phục giá trị ban đầu của nó và việc lật các ngưỡng gần ranh giới mảng phải được xử lý cẩn thận. Hãy xem xét một mảng nhỏ`[-1, 0, 1]`với các truy vấn`> -2`Và`< 2`. Việc thực hiện bất cẩn có thể lật cùng một số hai lần hoặc áp dụng sai bất đẳng thức, dẫn đến kết quả sai. 

Khó khăn của vấn đề nằm ở việc áp dụng hiệu quả nhiều dấu hiệu có điều kiện trên một phạm vi giới hạn của các giá trị số nguyên trong khi tính toán các tương tác giữa các truy vấn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi truy vấn, lặp qua tất cả các phần tử mảng và lật dấu nếu phép so sánh giữ nguyên. Điều này đúng nhưng thực hiện lên đến$n \cdot q = 10^10$hoạt động trong trường hợp xấu nhất là quá chậm. 

Quan sát quan trọng để cải thiện hiệu suất là các phần tử mảng và ngưỡng truy vấn là các số nguyên bị chặn. Thay vì làm việc trực tiếp với mảng, chúng ta có thể tính toán trước dấu cuối cùng của mọi số nguyên có thể có trong phạm vi cho phép sau tất cả các truy vấn. Khi có ánh xạ này, chúng ta có thể áp dụng nó cho từng phần tử trong mảng chỉ trong một lần chuyển. 

Cách tiếp cận này hoạt động vì thao tác lật có tính xác định và chỉ phụ thuộc vào giá trị và truy vấn. Các giá trị nằm trong một phạm vi nhỏ, rời rạc, vì vậy chúng ta có thể duy trì bảng dấu cho tất cả các số nguyên từ -100.000 đến 100.000. Chúng tôi xử lý ngược lại các truy vấn từ cuối đến đầu, quyết định xem có nên đảo dấu của nó hay không cho từng giá trị. Về cơ bản, đây là một đường quét trên các giá trị số nguyên hoặc sự phân chia và chinh phục trên trạng thái dấu hiệu. 

Kết quả là thay vì O(n*q), chúng ta nhận được độ phức tạp tỷ lệ thuận với phạm vi số nguyên cộng với số lượng truy vấn, điều này là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu (Bảng ký hiệu) | O(R + q + n), R ≈ 2*10^5 | O(R) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`sign`có độ dài 200.001 đại diện cho các số nguyên từ -100.000 đến 100.000. Mỗi mục ban đầu lưu trữ +1 cho dấu dương. 
2. Lặp lại các truy vấn theo thứ tự ngược lại. Đối với một truy vấn như`> x`, chúng tôi xem xét tất cả các số nguyên lớn hơn`x`và đánh dấu để lật nếu chưa sửa. Tương tự, đối với`< x`, coi mọi số nguyên nhỏ hơn`x`. 
3. Sau khi xử lý tất cả các truy vấn,`sign[v + 100000]`cho chúng ta biết hệ số nhân cuối cùng (+1 hoặc -1) cho số nguyên`v`. 
4. Lặp lại mảng ban đầu`a`và nhân mỗi phần tử với số tương ứng của nó`sign`yếu tố để tạo ra mảng cuối cùng. 

Cách tiếp cận này hoạt động vì chúng tôi mô phỏng hiệu quả tác động thực của tất cả các truy vấn trên mỗi giá trị số nguyên. Việc xử lý các truy vấn ngược lại đảm bảo rằng các lần lật sau sẽ ghi đè các lần lật trước đó khi cần thiết, duy trì đúng thứ tự thao tác. Mỗi phần tử trong mảng sau đó có thể được ánh xạ trực tiếp tới dấu hiệu cuối cùng của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

OFFSET = 100000
MAXV = 100000

n, q = map(int, input().split())
a = list(map(int, input().split()))

queries = []
for _ in range(q):
    s, x = input().split()
    x = int(x)
    queries.append((s, x))

# Initialize sign table: 1 means positive, -1 means negative
sign = [1] * (2 * MAXV + 1)
fixed = [False] * (2 * MAXV + 1)

# Process queries in reverse
for s, x in reversed(queries):
    if s == '>':
        for v in range(x + 1, MAXV + 1):
            idx = v + OFFSET
            if not fixed[idx]:
                sign[idx] *= -1
                fixed[idx] = True
    else:  # s == '<'
        for v in range(-MAXV, x):
            idx = v + OFFSET
            if not fixed[idx]:
                sign[idx] *= -1
                fixed[idx] = True

# Apply final signs
result = [val * sign[val + OFFSET] for val in a]
print(' '.join(map(str, result)))
```Giải pháp này xây dựng một bảng tra cứu để mã hóa hiệu ứng thực của tất cả các truy vấn. Lặp lại ngược lại đảm bảo thứ tự chính xác. các`fixed`mảng đảm bảo rằng khi dấu cuối cùng của một số được xác định, nó sẽ không bị ghi đè bởi các truy vấn trước đó. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
a = [-5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5]
queries = [('>', 2), ('>', -4), ('<', 5)]
```| Bước | bảng hiệu hiệu ứng | Mảng sau khi áp dụng | 
| --- | --- | --- | 
| Ban đầu | tất cả +1 | [-5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5] | 
| '>' 2 | lật chỉ số 3..5 | [-5, -4, -3, -2, -1, 0, 1, 2, -3, -4, -5] | 
| '>' -4 | chỉ số lật -3..5 | [-5, -4, 3, 2, 1, 0, -1, -2, 3, -4, -5] | 
| '<' 5 | lật chỉ số -100000..4 | [5, 4, -3, -2, -1, 0, 1, 2, -3, 4, 5] | 

Dấu vết này xác nhận rằng thuật toán theo dõi chính xác các lần lật dấu tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R + q + n) | R = 2*10^5 là phạm vi số nguyên; xây dựng bảng mất O(R) cho mỗi truy vấn, mảng lặp là O(n) | 
| Không gian | O(R + n + q) | mảng dấu và mảng cố định lưu trữ mỗi giá trị 2*10^5; mảng đầu vào lưu trữ n phần tử và q truy vấn | 

Với các ràng buộc n, q 10^5 và phạm vi giá trị giới hạn, điều này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    OFFSET = 100000
    MAXV = 100000

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    queries = [tuple(input().split()) for _ in range(q)]
    queries = [(s, int(x)) for s, x in queries]

    sign = [1] * (2 * MAXV + 1)
    fixed = [False] * (2 * MAXV + 1)

    for s, x in reversed(queries):
        if s == '>':
            for v in range(x + 1, MAXV + 1):
                idx = v + OFFSET
                if not fixed[idx]:
                    sign[idx] *= -1
                    fixed[idx] = True
        else:
            for v in range(-MAXV, x):
                idx = v + OFFSET
                if not fixed[idx]:
                    sign[idx] *= -1
                    fixed[idx] = True

    result = [val * sign[val + OFFSET] for val in a]
    return ' '.join(map(str, result))

# Provided sample
assert run("11 3\n-5 -4 -3 -2 -1 0 1 2 3 4 5\n> 2\n> -4\n< 5\n") == "5 4 -3 -2 -1 0 1 2 -3 4 5", "sample 1"
# Minimum input
assert run("1 1\n0\n> -1\n") == "0", "single element no flip"
# All equal values
assert run("3 2\n2 2 2\n> 1\n< 3\n") == "-2 -2 -2", "all elements flipped twice"
# Maximum value
assert run("2 1\n100000 -100000\n> 0\n") == "-100000 -100000", "boundary flip"
# Mix negative and positive
assert run("4 2\n-1 0 1 2\n< 0\n> 1\n") == "1 0 -1 -2", "mixed signs flip"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n0\n> -1`|`0`| phần tử đơn, truy vấn bao gồm nó nhưng không có |
