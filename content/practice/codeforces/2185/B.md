---
title: "CF 2185B - Tiền tố tối đa"
description: "Chúng tôi được cung cấp một mảng. Đối với mọi vị trí, chúng tôi xem xét tiền tố kết thúc ở đó và lấy giá trị lớn nhất được thấy cho đến nay. Điểm của mảng là tổng của tất cả các cực đại tiền tố đó. Chúng tôi có thể hoán đổi bất kỳ hai phần tử nào nhiều nhất một lần hoặc chọn không hoán đổi chút nào."
date: "2026-06-07T21:28:30+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2185
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1074 (Div. 4)"
rating: 800
weight: 2185
solve_time_s: 105
verified: true
draft: false
---

[CF 2185B - Tiền tố tối đa](https://codeforces.com/problemset/problem/2185/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng. Đối với mọi vị trí, chúng tôi xem xét tiền tố kết thúc ở đó và lấy giá trị lớn nhất được thấy cho đến nay. Điểm của mảng là tổng của tất cả các cực đại tiền tố đó. 

Chúng tôi có thể hoán đổi bất kỳ hai phần tử nào nhiều nhất một lần hoặc chọn không hoán đổi chút nào. Nhiệm vụ là tối đa hóa số điểm đạt được. 

Để hiểu điểm số, hãy xem xét:```
[2, 1, 4, 5, 3]

prefix maxima:
2
2
4
5
5

score = 2 + 2 + 4 + 5 + 5 = 18
```Những hạn chế là cực kỳ nhỏ. Có tối đa 100 trường hợp thử nghiệm và mỗi mảng có độ dài tối đa là 50. Ngay cả một thuật toán thử mọi hoán đổi có thể cũng là thực tế. 

Với$n=50$, số lần hoán đổi có thể thực hiện được chỉ là$$\frac{50 \cdot 49}{2} = 1225$$và tính điểm của một mảng$O(n)$. Điều đó có nghĩa là một cuộc tìm kiếm toàn diện có chi phí khoảng$1225 \cdot 50 \approx 6 \times 10^4$hoạt động cho mỗi trường hợp thử nghiệm, rất nhỏ. 

Thách thức chính là nhận ra rằng các ràng buộc cho phép tìm kiếm trực tiếp thay vì tìm kiếm một quan sát tham lam phức tạp hơn. 

Một vài trường hợp đáng chú ý. 

Hãy xem xét một mảng trong đó giá trị lớn nhất đã ở đầu tiên:```
5 1
```Điểm của nó là:```
5 + 5 = 10
```Hoán đổi tạo ra:```
1 5
```có số điểm chỉ là:```
1 + 5 = 6
```Câu trả lời đúng là 10 vì chúng ta được phép thực hiện hoán đổi bằng 0. Một giải pháp luôn thực hiện chính xác một lần hoán đổi sẽ thất bại. 

Hãy xem xét một mảng có giá trị lặp lại:```
4 4 4
```Mỗi lần hoán đổi đều giữ nguyên mảng và điểm vẫn là 12. Một giải pháp phải đánh giá tất cả các khả năng bao gồm cả mảng tương đương. 

Hãy xem xét trường hợp di chuyển phần tử tối đa về phía trước là tối ưu:```
2 1 4 5 3
```Hoán đổi 2 và 5 cho:```
5 1 4 2 3
```Mỗi tiền tố tối đa trở thành 5, tạo ra điểm 25. Điều này chứng tỏ tại sao một giao dịch hoán đổi hấp dẫn cục bộ không nhất thiết phải là một giao dịch tốt nhất. Chúng ta phải so sánh tất cả các ứng cử viên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi hành động có thể. 

Với mỗi cặp chỉ số$(i,j)$, hoán đổi hai phần tử, tính điểm của mảng kết quả và giữ giá trị tốt nhất được tìm thấy. Chúng ta cũng nên xem xét khả năng không thực hiện trao đổi. 

Việc tính điểm của một mảng rất đơn giản. Chúng tôi quét từ trái sang phải trong khi duy trì mức tối đa tiền tố hiện tại. Tại mỗi vị trí, chúng tôi thêm mức tối đa đó vào câu trả lời. 

Phương pháp brute-force rõ ràng là đúng vì nó đánh giá rõ ràng mọi mảng có thể đạt được với nhiều nhất một lần hoán đổi. Nếu câu trả lời tối ưu tồn tại thì nó phải xuất hiện trong số các cấu hình được thử nghiệm. 

Trong nhiều vấn đề của Codeforce, việc thử tất cả các giao dịch hoán đổi là không thể vì$n$có thể là$10^5$hoặc lớn hơn. Đây$n\le 50$, do đó không gian tìm kiếm rất nhỏ. Nhận xét quan trọng không phải là một thủ thuật tham lam thông minh mà là nhận ra rằng các ràng buộc khiến cho việc tìm kiếm toàn diện hoàn toàn có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các giao dịch hoán đổi |$O(n^3)$|$O(1)$| Đã chấp nhận | 
| Bất kỳ tối ưu hóa phức tạp nào hơn | Không cần thiết | Không cần thiết | Được chấp nhận nhưng được thiết kế quá mức | 

Từ$n\le 50$,$O(n^3)$chỉ là$125000$hoạt động cho mỗi trường hợp thử nghiệm, trong giới hạn. 

## Hướng dẫn thuật toán 

1. Tính điểm của mảng ban đầu và lưu trữ dưới dạng câu trả lời đúng nhất hiện tại. 
2. Với mỗi cặp chỉ số$i<j$, hoán đổi hai phần tử. 
3. Quét mảng đã sửa đổi từ trái sang phải. 

Duy trì giá trị tối đa được thấy cho đến nay và tích lũy tổng cực đại tiền tố. 
4. Cập nhật câu trả lời đúng nhất nếu số điểm hiện tại lớn hơn. 
5. Hoàn tác hoán đổi để mảng trở về trạng thái ban đầu trước khi kiểm tra cặp tiếp theo. 
6. Sau khi kiểm tra tất cả các cặp, in ra điểm cao nhất tìm được. 

### Tại sao nó hoạt động 

Tập hợp các mảng có thể truy cập được với nhiều nhất một lần hoán đổi bao gồm mảng ban đầu cộng với mọi mảng thu được bằng cách hoán đổi một cặp vị trí. Thuật toán đánh giá điểm của từng mảng như vậy đúng một lần và ghi lại số điểm tối đa gặp phải. 

Vì mọi kết quả pháp lý đều được kiểm tra nên số điểm lớn nhất mà thuật toán tìm được chính xác là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def value(a):
    mx = 0
    total = 0
    for x in a:
        mx = max(mx, x)
        total += mx
    return total

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    ans = value(a)

    for i in range(n):
        for j in range(i + 1, n):
            a[i], a[j] = a[j], a[i]

            ans = max(ans, value(a))

            a[i], a[j] = a[j], a[i]

    print(ans)
```Chức năng trợ giúp`value()`tính điểm của một mảng. Nó duy trì mức tối đa tiền tố hiện tại và thêm nó vào tổng số tiền đang chạy ở mỗi vị trí. 

Vòng lặp chính đầu tiên đánh giá mảng ban đầu. Điều này tự động xử lý tùy chọn "không trao đổi". 

Mỗi cặp vị trí sau đó được hoán đổi tạm thời. Sau khi tính điểm, việc hoán đổi được hoàn tác ngay lập tức. Khôi phục thứ tự ban đầu là điều cần thiết vì mọi hoán đổi ứng cử viên phải được kiểm tra trên cùng một mảng bắt đầu. 

Điểm số có thể lớn đến mức$50 \times 10000 = 500000$, dễ dàng phù hợp với số nguyên Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:```
2 1 4 5 3
```Điểm gốc: 

| Vị trí | Giá trị | Tiền tố Max | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 
| 2 | 1 | 2 | 4 | 
| 3 | 4 | 4 | 8 | 
| 4 | 5 | 5 | 13 | 
| 5 | 3 | 5 | 18 | 

Bây giờ hãy kiểm tra vị trí hoán đổi 1 và 4:```
5 1 4 2 3
```| Vị trí | Giá trị | Tiền tố Max | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 | 5 | 
| 2 | 1 | 5 | 10 | 
| 3 | 4 | 5 | 15 | 
| 4 | 2 | 5 | 20 | 
| 5 | 3 | 5 | 25 | 

Điểm trở thành 25, đó là câu trả lời tối đa có thể. 

Ví dụ này cho thấy tại sao việc đưa một phần tử lớn sớm hơn có thể cải thiện nhiều tiền tố cùng một lúc. 

### Ví dụ 2 

Mảng đầu vào:```
5 1
```Điểm gốc: 

| Vị trí | Giá trị | Tiền tố Max | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 | 5 | 
| 2 | 1 | 5 | 10 | 

Hoán đổi hai vị trí:```
1 5
```| Vị trí | Giá trị | Tiền tố Max | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 5 | 5 | 6 | 

Điểm tốt nhất vẫn là 10. 

Ví dụ này chứng minh tại sao thuật toán phải bao gồm cấu hình không trao đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$|$O(n^2)$hoán đổi, chi phí tính toán mỗi điểm$O(n)$| 
| Không gian |$O(1)$| Chỉ có một vài biến được sử dụng | 

Với$n \le 50$, công việc trong trường hợp xấu nhất là đại khái$50^3 = 125000$hoạt động cho mỗi trường hợp thử nghiệm, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def value(a):
        mx = 0
        total = 0
        for x in a:
            mx = max(mx, x)
            total += mx
        return total

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = value(a)

        for i in range(n):
            for j in range(i + 1, n):
                a[i], a[j] = a[j], a[i]
                ans = max(ans, value(a))
                a[i], a[j] = a[j], a[i]

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided sample
assert run(
"""4
5
2 1 4 5 3
2
5 1
3
3 2 1
2
6 7
"""
) == """25
10
9
14
"""

# minimum size
assert run(
"""1
2
1 2
"""
) == """4
"""

# all equal
assert run(
"""1
4
7 7 7 7
"""
) == """28
"""

# maximum already first
assert run(
"""1
5
10 1 2 3 4
"""
) == """50
"""

# increasing array
assert run(
"""1
4
1 2 3 4
"""
) == """16
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`4`| Mảng nhỏ nhất được phép | 
|`7 7 7 7`|`28`| Giá trị lặp lại và hoán đổi dư thừa | 
|`10 1 2 3 4`|`50`| Không trao đổi là tối ưu | 
|`1 2 3 4`|`16`| Di chuyển giá trị lớn nhất về phía trước sẽ cải thiện nhiều tiền tố | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
5 1
```Điểm ban đầu là 10. Lần hoán đổi duy nhất có thể tạo ra điểm 6. Thuật toán bắt đầu với điểm ban đầu là câu trả lời hiện tại, sau đó kiểm tra lần hoán đổi và giữ giá trị lớn hơn. Câu trả lời cuối cùng là 10. 

Hãy xem xét:```
1
3
4 4 4
```Tỉ số ban đầu là:```
4 + 4 + 4 = 12
```Mỗi lần hoán đổi đều khiến mảng không thay đổi. Thuật toán đánh giá tất cả các khả năng và luôn đạt được 12, do đó kết quả đầu ra vẫn đúng. 

Coi như:```
1
5
2 1 4 5 3
```Cách tốt nhất là hoán đổi 2 và 5, mang lại:```
5 1 4 2 3
```có điểm là 25. Vì thuật toán kiểm tra từng cặp chỉ số nên việc hoán đổi này đảm bảo sẽ được kiểm tra và tìm thấy điểm tối đa.
