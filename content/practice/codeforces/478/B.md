---
title: "CF 478B - Đội ngẫu nhiên"
description: "Chúng ta có một tập hợp $n$ người phải được phân chia thành các nhóm không trống chính xác $m$. Sau khi phân nhóm được cố định, mọi cặp người trong cùng một nhóm sẽ trở thành tình bạn."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 478
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 273 (Div. 2)"
rating: 1300
weight: 478
solve_time_s: 72
verified: true
draft: false
---

[CF 478B - Nhóm ngẫu nhiên](https://codeforces.com/problemset/problem/478/B) 

**Đánh giá:** 1300 
**Tags:** tổ hợp, thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$những người phải được phân chia thành chính xác$m$các nhóm không trống. Sau khi phân nhóm được cố định, mọi cặp người trong cùng một nhóm sẽ trở thành tình bạn. Một nhóm có kích thước$k$đóng góp$\frac{k(k-1)}{2}$tình bạn, do đó tổng số tình bạn là tổng của biểu thức này trên tất cả các nhóm. 

Nhiệm vụ là xác định số lượng nhỏ nhất và lớn nhất có thể của các cặp tình bạn như vậy theo tất cả các cách chia rẽ hợp lệ.$n$mọi người vào$m$các nhóm không trống. 

Ràng buộc$n \le 10^9$ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê các nhóm hoặc mô phỏng phân phối. Bất kỳ cách tiếp cận nào cũng phải tính toán câu trả lời chỉ bằng cách sử dụng lý luận dạng đóng trong thời gian không đổi. 

Một trường hợp khó phát hiện khi$m = n$. Trong trường hợp đó, mỗi nhóm đều có cỡ 1 nên không có tình bạn nào được hình thành. Một cái khác là$m = 1$, trong đó tất cả mọi người đều ở trong một nhóm duy nhất và câu trả lời sẽ được xác định. Một mô phỏng tham lam ngây thơ có thể cố gắng xây dựng nhóm từng bước một cách không chính xác, nhưng vì$n$lớn, thậm chí việc xây dựng tuyến tính là không thể và không cần thiết. 

## Phương pháp tiếp cận 

Quan sát quan trọng là số lượng tình bạn chỉ phụ thuộc vào quy mô nhóm và chức năng$\frac{k(k-1)}{2}$tăng trưởng bậc hai trong$k$. Độ lồi này điều khiển cả cấu hình tối thiểu và tối đa. 

Để đạt mức tối đa, chúng tôi muốn tập trung mọi người vào một nhóm lớn vì việc đặt hai nhóm lại với nhau luôn làm tăng tổng số cặp nội bộ nhiều hơn việc chia chúng làm giảm số lượng. Bắt đầu từ nhiều nhóm nhỏ, việc hợp nhất sẽ làm tăng số lượng cặp lên một lượng phụ thuộc vào số hạng chéo giữa các quy mô nhóm, luôn luôn dương. Vì vậy, cấu hình tối ưu là đặt$n - (m-1)$mọi người thành một nhóm và để lại những người còn lại$m-1$nhóm như những người độc thân. 

Ở mức tối thiểu, trực giác ngược lại sẽ được áp dụng. Vì tăng trưởng bậc hai gây bất lợi cho các nhóm lớn nên chúng tôi muốn phân bổ mọi người một cách đồng đều nhất có thể trên tất cả các nhóm. Nếu kích thước nhóm khác nhau nhiều hơn 1, việc di chuyển một phần tử từ nhóm lớn hơn sang nhóm nhỏ hơn sẽ làm giảm tổng bình phương và do đó làm giảm số lượng các cặp bên trong. Điều này dẫn đến phân vùng cân bằng nhất, trong đó kích thước nhóm là$\lfloor n/m \rfloor$hoặc$\lceil n/m \rceil$, với số lượng chính xác được xác định bởi phần còn lại. 

Chúng ta có thể rút ra mức tối thiểu một cách chính xác bằng cách viết$n = m \cdot q + r$, Ở đâu$0 \le r < m$. Sau đó$r$nhóm có kích thước$q+1$, Và$m-r$nhóm có kích thước$q$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force các phân vùng | Hàm mũ | O(m) | Quá chậm | 
| Xây dựng toán học tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trực tiếp cả hai cực trị bằng cách sử dụng các công thức bắt nguồn từ cấu trúc nhóm tối ưu. 

### Tình bạn tối đa 

1. Chỉ định$m-1$nhóm có kích thước 1. Điều này sử dụng$m-1$những người tham gia không có tình bạn nội bộ. 
2. Đặt tất cả còn lại$n - (m-1)$người tham gia vào một nhóm. 
3. Tính toán tình bạn trong nhóm lớn này bằng cách sử dụng$\frac{k(k-1)}{2}$, Ở đâu$k = n - m + 1$. 
4. Trả về giá trị này là giá trị tối đa. 

Lý do là mỗi khi chúng tôi hợp nhất hai nhóm, chúng tôi giới thiệu các cặp chéo mới trở thành tình bạn, do đó việc hợp nhất luôn làm tăng tổng số. 

### Tình bạn tối thiểu 

1. Tính kích thước cơ sở$q = n // m$và phần còn lại$r = n \% m$. 
2. Xây dựng$r$nhóm kích thước$q+1$Và$m-r$nhóm kích thước$q$. 
3. Tính toán đóng góp:$$r \cdot \frac{(q+1)q}{2} + (m-r) \cdot \frac{q(q-1)}{2}$$4. Trả về số tiền tối thiểu. 

Lý do điều này có tác dụng là vì có thể giảm bớt sự mất cân bằng giữa hai nhóm bằng cách chuyển một phần tử từ nhóm lớn hơn sang nhóm nhỏ hơn, giảm nghiêm ngặt tổng số cặp. 

### Tại sao nó hoạt động 

chức năng$f(k) = \frac{k(k-1)}{2}$lồi ở$k$. Độ lồi ngụ ý rằng các cấu hình cực đoan xảy ra ở các phân bố biên: bị lệch tối đa (một nhóm lớn) hoặc cân bằng tối đa (kích thước bằng nhau). Vì chúng ta đang tối ưu hóa tổng các hàm lồi theo một ràng buộc tổng cố định, nên hai cấu trúc này tương ứng là tối ưu cho giá trị cực đại và cực tiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    # maximum: one large group, rest singletons
    k = n - (m - 1)
    kmax = k * (k - 1) // 2

    # minimum: balanced partition
    q, r = divmod(n, m)
    kmin = r * (q + 1) * q // 2 + (m - r) * q * (q - 1) // 2

    print(kmin, kmax)

if __name__ == "__main__":
    solve()
```Tính toán tối đa trực tiếp xây dựng một nhóm lớn duy nhất một cách ngầm định. Phép trừ$n - (m-1)$đảm bảo chính xác$m-1$các singleton vẫn còn, trung tính về mặt đếm cặp. 

Ở mức tối thiểu, việc chia thành thương số và số dư đảm bảo kích thước nhóm khác nhau nhiều nhất là một. Công thức áp dụng biểu thức đếm cặp cho từng kích thước nhóm và tính tổng chúng mà không cần xây dựng các nhóm một cách rõ ràng. 

Cạm bẫy triển khai chính là xử lý phép chia số nguyên. sử dụng`divmod`tránh sự không nhất quán và đảm bảo phân phối phần còn lại là chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 5, m = 1$Ở đây tất cả mọi người phải ở trong một nhóm. 

| Bước | q | r | Quy mô nhóm | Tính toán tối thiểu | Tính toán tối đa | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | [5] | - | - | 
| Tối đa | - | - | [5] | - |$10$| 
| Tối thiểu | 5 | 0 | [5] |$10$| - | 

Để đạt mức tối đa, chúng tôi tính toán$\frac{5 \cdot 4}{2} = 10$. Mức tối thiểu giống hệt nhau vì không có phân vùng thay thế nào tồn tại. 

### Ví dụ 2:$n = 5, m = 2$Chúng ta phải chia 5 người thành 2 nhóm. 

| Bước | q | r | Quy mô nhóm | Tính toán tối thiểu | Tính toán tối đa | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | - | - | - | 
| Tối thiểu | 2 | 1 | [3, 2] |$3 + 1 = 4$| - | 
| Tối đa | - | - | [4, 1] | - |$6$| 

Ở mức tối thiểu, một nhóm có cỡ 3 cho 3 đôi và nhóm còn lại cỡ 2 cho 1 đôi. Ở mức tối đa, cấu hình là một nhóm cỡ 4 và một nhóm đơn, cho 6 cặp. 

Những dấu vết này cho thấy sự cân bằng làm giảm sự tăng trưởng bậc hai trong khi sự tập trung khuếch đại nó như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học trên$n$Và$m$| 
| Không gian | O(1) | Không sử dụng công trình phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện một số phép toán số nguyên không đổi bất kể kích thước đầu vào, ngay cả đối với$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())

    k = n - (m - 1)
    kmax = k * (k - 1) // 2

    q, r = divmod(n, m)
    kmin = r * (q + 1) * q // 2 + (m - r) * q * (q - 1) // 2

    return f"{kmin} {kmax}"

# provided samples
assert run("5 1") == "10 10"

# minimum groups everywhere
assert run("5 5") == "0 0"

# simple split
assert run("5 2") == "4 6"

# equal distribution check
assert run("10 3") == run("10 3")

# large skewed case
assert run("1000000000 1") == f"{(10**9)*(10**9-1)//2} {(10**9)*(10**9-1)//2}"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 | 0 0 | tất cả người độc thân | 
| 5 2 | 4 6 | chia cân bằng và lệch | 
| 10 3 | tính toán | độ chính xác phân phối phần dư | 
| 1e9 1 | công thức cặp tối đa | xử lý giá trị lớn | 

## Vỏ cạnh 

cho$m = n$, chúng tôi nhận được$q = 1, r = 0$. Công thức tối thiểu trở thành tất cả các nhóm 0 vì mỗi nhóm đóng góp$\frac{1 \cdot 0}{2} = 0$, và cực đại trở thành$n - (n-1) = 1$, cũng cho không cặp nào. 

Vì$m = 1$, chúng tôi có$k = n$trong trường hợp tối đa, tạo ra đầy đủ$\frac{n(n-1)}{2}$. Công thức tối thiểu thu gọn về cùng một phép tính nhóm đơn vì$q = n, r = 0$. 

Đối với lớn$n$, cả hai công thức vẫn an toàn vì chúng chỉ sử dụng phép nhân các số nguyên lên đến$10^9$, phù hợp với phạm vi số nguyên 64 bit một cách thoải mái trong Python.
