---
title: "CF 106251G - Máy bị lỗi của Hải ly bận"
description: "Chúng ta có một số nguyên dương $X$ được viết dưới dạng cơ số $B$ dưới dạng một dãy các chữ số. Nhiệm vụ là xác định xem có tồn tại hai số nguyên dương $Y$ và $Z$ sao cho: $$X + Y = Z$$ và biểu diễn cơ số $B$ của $Y$ và $Z$ chứa chính xác nhiều tập chữ số giống nhau hay không."
date: "2026-06-25T07:22:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106251
codeforces_index: "G"
codeforces_contest_name: "MITIT Winter 2025-26 Beginner Round"
rating: 0
weight: 106251
solve_time_s: 56
verified: true
draft: false
---

[CF 106251G - Máy bị lỗi của Hải ly bận](https://codeforces.com/problemset/problem/106251/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$X$được viết bằng cơ sở$B$dưới dạng một chuỗi các chữ số. 

Nhiệm vụ là xác định xem có tồn tại hai số nguyên dương hay không$Y$Và$Z$như vậy:$$X + Y = Z$$và cơ sở-$B$đại diện của$Y$Và$Z$chứa chính xác nhiều tập chữ số giống nhau. Thứ tự các chữ số có thể khác nhau, nhưng mỗi chữ số phải xuất hiện cùng số lần trong cả hai số. 

Nếu những con số đó tồn tại, chúng ta phải xuất ra một cặp hợp lệ. Nếu không, chúng tôi xuất`NO`. 

Kích thước đầu vào lớn. Một số có thể chứa tới$10^5$chữ số và tổng số chữ số trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng chuyển đổi toàn bộ giá trị thành kiểu số nguyên tích hợp đều không thể thực hiện được. Chúng ta cần một thuật toán hoạt động trực tiếp trên cơ sở$B$biểu diễn chữ số và chạy trong thời gian tuyến tính. 

Quan sát quan trọng nhất đến từ tổng chữ số. Hai số bất kỳ có nhiều chữ số giống nhau thì có tổng các chữ số bằng nhau. Trong căn cứ$B$,$$N \equiv \text{sum of digits of } N \pmod{B-1}.$$Từ$Y$Và$Z$sử dụng các chữ số giống nhau,$$Y \equiv Z \pmod{B-1}.$$sử dụng$Z-Y=X$,$$X \equiv 0 \pmod{B-1}.$$Đây là một điều kiện cần thiết. 

Việc triển khai bất cẩn có thể dừng ở đây và chỉ kiểm tra khả năng chia hết. Phần khó hơn là chứng minh rằng điều kiện này cũng đủ và xây dựng được câu trả lời hợp lệ. 

Hãy xem xét một ví dụ nhỏ trong cơ sở$10$:```
X = 36
```Tổng các chữ số là$3+6=9$, Vì thế$X$chia hết cho$9$. Một câu trả lời hợp lệ là```
Y = 105
Z = 150
```Cả hai đều sử dụng chữ số`{1,0,5}`, Và$$150 - 105 = 45 \neq 36.$$Có cùng chữ số là không đủ. Việc xây dựng phải tạo ra chính xác sự khác biệt cần thiết. 

Một trường hợp cạnh khác là khi$X$chỉ có một chữ số.```
B = 2
X = 1
```Từ$B-1=1$, mọi số đều chia hết cho$B-1$. Một giải pháp vẫn phải tồn tại. Bất kỳ cách tiếp cận nào giả sử thương số có nhiều chữ số sẽ thất bại ở đây. 

Trường hợp khó phát hiện thứ ba là khi thương chứa các số 0 đứng đầu sau phép chia. Ví dụ,```
B = 10
X = 18
```Sau đó$X/(B-1)=2$. Biểu diễn thương số chỉ đơn giản là`[2]`, không`[0,2]`. Phép chia dài phải loại bỏ các số 0 đứng đầu một cách chính xác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là tạo ra nhiều tập hợp chữ số ứng cử viên, xây dựng tất cả các hoán vị cho$Y$Và$Z$, và kiểm tra xem$Z-Y=X$. Điều này chỉ hoạt động đối với các đầu vào cực nhỏ vì số lượng hoán vị tăng theo giai thừa với số chữ số. Ngay cả độ dài 20 cũng đã khiến việc tìm kiếm hoàn toàn không khả thi. 

Quan sát quan trọng là số học mô-đun. 

Bởi vì$Y$Và$Z$chứa các chữ số giống nhau thì chúng có tổng các chữ số giống nhau. Trong căn cứ$B$, mọi số đều đồng dư với tổng các chữ số của nó theo modulo$B-1$. Kể từ đây$$Y \equiv Z \pmod{B-1}.$$Từ$X=Z-Y$,$$X \equiv 0 \pmod{B-1}.$$Điều này ngay lập tức đưa ra một điều kiện cần thiết. 

Phần đáng ngạc nhiên là nó cũng đủ. 

Cho phép$$K=\frac{X}{B-1}.$$Giả sử cơ sở-$B$chữ số của$K$là$$d_1,d_2,\ldots,d_m.$$xây dựng$$Y = (1,0,d_1,d_2,\ldots,d_m)_B$$Và$$Z = (1,d_1,d_2,\ldots,d_m,0)_B.$$Hai số rõ ràng chứa chính xác các chữ số giống nhau: cả hai đều chứa một chữ số`1`, một chữ số`0`, và tất cả các chữ số của$K$. 

Về mặt số lượng,$$Y = 1\cdot B^{m+1}+K,$$

$$Z = 1\cdot B^{m+1}+B\cdot K.$$Trừ,$$Z-Y=(B-1)K=X.$$Vì vậy mỗi$X$chia hết cho$B-1$thừa nhận một giải pháp. 

Toàn bộ vấn đề giảm xuống: 

1. Kiểm tra xem$X$chia hết cho$B-1$. 
2. Nếu không, xuất`NO`. 
3. Ngược lại tính toán$K=X/(B-1)$sử dụng phép chia dài trên mảng chữ số. 
4. Xuất kết quả trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cơ sở-$B$chữ số của$X$. 
2. Tính tổng các chữ số theo modulo$B-1$. 

Từ$B \equiv 1 \pmod{B-1}$, một số chia hết cho$B-1$chính xác khi tổng các chữ số của nó chia hết cho$B-1$. 
3. Nếu số dư khác 0, xuất ra`NO`. 

Không có cặp hợp lệ nào có thể tồn tại vì bất kỳ hai số nào có nhiều chữ số giống nhau đều là đồng dư modulo$B-1$. 
4. Chia mảng chữ số của$X$qua$B-1$sử dụng phép chia dài tiêu chuẩn trong cơ sở$B$. 

Duy trì phần còn lại. Với mỗi chữ số, hãy cập nhật$$cur = remainder \cdot B + digit,$$phát ra$$cur // (B-1),$$và giữ$$cur \bmod (B-1).$$5. Xóa các chữ số 0 ở đầu khỏi thương số. Các chữ số còn lại là số cơ sở$B$đại diện của$K$. 
6. Gọi các chữ số thương là$d_1,\ldots,d_m$. 
7. Đầu ra$$Y=(1,0,d_1,\ldots,d_m)$$Và$$Z=(1,d_1,\ldots,d_m,0).$$### Tại sao nó hoạt động 

Kiểm tra tính chia hết là cần thiết vì các tập hợp nhiều chữ số bằng nhau ngụ ý tổng các chữ số bằng nhau và các số đồng dạng với tổng chữ số của chúng theo modulo$B-1$. Do đó mọi giá trị hợp lệ$X$phải thỏa mãn$X \equiv 0 \pmod{B-1}$. 

Khi điều kiện này xảy ra, chúng ta xác định$K=X/(B-1)$. Việc xây dựng đặt các chữ số giống nhau vào$Y$Và$Z$, chỉ đơn thuần là thay đổi vị trí của họ. Chữ số đứng đầu của chúng luôn là`1`, vì vậy không có số nào có số 0 đứng đầu. 

Sự khác biệt về số lượng là$$(1\cdot B^{m+1}+BK) - (1\cdot B^{m+1}+K)
= (B-1)K
= X.$$Do đó, việc xây dựng đáp ứng cả hai đặc tính cần thiết, chứng tỏ sự đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    n, b = map(int, input().split())
    a = list(map(int, input().split()))

    mod = b - 1

    if sum(a) % mod != 0:
        out.append("NO")
        continue

    # Long division: X / (B - 1)
    q = []
    rem = 0

    for digit in a:
        cur = rem * b + digit
        q.append(cur // mod)
        rem = cur % mod

    idx = 0
    while idx < len(q) - 1 and q[idx] == 0:
        idx += 1
    q = q[idx:]

    m = len(q) + 2

    y = [1, 0] + q
    z = [1] + q + [0]

    out.append("YES")
    out.append(str(m))
    out.append(" ".join(map(str, y)))
    out.append(" ".join(map(str, z)))

sys.stdout.write("\n".join(out))
```Phần đầu tiên kiểm tra khả năng chia hết cho$B-1$dùng quy tắc cộng các chữ số. Điều này tránh việc tái tạo lại giá trị to lớn của$X$. 

Phần chia dài tính toán$K=X/(B-1)$trực tiếp từ mảng chữ số. Mọi giá trị trung gian nhiều nhất là$$(B-2)\cdot B + (B-1) < B^2,$$dễ dàng khớp với các số nguyên tiêu chuẩn. 

Sau khi chia, các số 0 đứng đầu sẽ bị loại bỏ. Điều này rất quan trọng vì thương số phải được biểu diễn dưới dạng chính tắc. 

Việc xây dựng cuối cùng chính xác là bằng chứng. Hai mảng chứa các chữ số giống nhau, không bắt đầu bằng 0 và hiệu của chúng bằng nhau$X$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 2, B = 10
X = [3, 6]
```Tổng chữ số là$9$, chia hết cho$9$. 

Phép chia dài cho$9$: 

| Vị trí | Chữ số | Giá trị hiện tại | Chữ số thương | Phần còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 3 | 
| 2 | 6 | 36 | 4 | 0 | 

Vì thế$K=[4]$. 

Xây dựng: 

| Biến | Chữ số | 
| --- | --- | 
| Y | [1, 0, 4] | 
| Z | [1, 4, 0] | 

Về mặt số lượng,$$140 - 104 = 36.$$Điều này thể hiện việc xây dựng cốt lõi. 

### Ví dụ 2 

đầu vào:```
N = 4, B = 5
X = [1, 4, 3, 4]
```Tổng chữ số:$$1+4+3+4=12.$$Từ$12 \equiv 0 \pmod 4$, có một giải pháp. 

Phép chia dài cho$4$: 

| Vị trí | Chữ số | Giá trị hiện tại | Chữ số thương | Phần còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 
| 2 | 4 | 9 | 2 | 1 | 
| 3 | 3 | 8 | 2 | 0 | 
| 4 | 4 | 4 | 1 | 0 | 

Các chữ số thương là`[2, 2, 1]`. 

Xây dựng: 

| Biến | Chữ số | 
| --- | --- | 
| Y | [1, 0, 2, 2, 1] | 
| Z | [1, 2, 2, 1, 0] | 

Hai số sử dụng chính xác các chữ số giống nhau và sự khác biệt của chúng là số gốc$X$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lần quét chia hết và một lần quét chia dài | 
| Không gian | O(N) | Lưu trữ các chữ số thương và đầu ra | 
| | | | 

Tổng số chữ số được xử lý trên tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, do đó độ phức tạp tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, b = map(int, input().split())
        a = list(map(int, input().split()))

        mod = b - 1

        if sum(a) % mod != 0:
            out.append("NO")
            continue

        q = []
        rem = 0

        for digit in a:
            cur = rem * b + digit
            q.append(cur // mod)
            rem = cur % mod

        idx = 0
        while idx < len(q) - 1 and q[idx] == 0:
            idx += 1
        q = q[idx:]

        m = len(q) + 2
        y = [1, 0] + q
        z = [1] + q + [0]

        out.append("YES")
        out.append(str(m))
        out.append(" ".join(map(str, y)))
        out.append(" ".join(map(str, z)))

    return "\n".join(out)

# sample NO case
assert run(
"""1
5 12
4 8 8 3 1
"""
) == "NO"

# base 2, minimum length
assert run(
"""1
1 2
1
"""
).startswith("YES")

# divisible by B-1
assert run(
"""1
2 10
3 6
"""
).startswith("YES")

# not divisible
assert run(
"""1
1 10
5
"""
) == "NO"

# all digits zero except leading digit
assert run(
"""1
3 10
9 0 0
"""
).startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 12 / 4 8 8 3 1`|`NO`| Thất bại trong điều kiện chia hết | 
|`1 2 / 1`|`YES`| Kích thước nhỏ nhất có thể | 
|`2 10 / 3 6`|`YES`| Trường hợp xây dựng cơ bản | 
|`1 10 / 5`|`NO`| Một chữ số không chia hết | 
|`3 10 / 9 0 0`|`YES`| Thương số có số 0 đứng đầu sau phép chia | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 10
5
```Tổng các chữ số là$5$. Từ$5 \not\equiv 0 \pmod 9$, thuật toán ngay lập tức xuất ra`NO`. Bất kỳ cặp hợp lệ nào cũng sẽ yêu cầu$X\equiv0\pmod9$, vậy bác bỏ là đúng. 

Coi như:```
1
1 2
1
```Đây$B-1=1$. Mọi số nguyên đều chia hết cho$1$. phép chia dài cho$K=1$. Việc xây dựng tạo ra```
Y = [1,0,1]
Z = [1,1,0]
```Và$$110_2 - 101_2 = 1.$$Đầu vào nhỏ nhất có thể được xử lý một cách tự nhiên. 

Coi như:```
1
2 10
1 8
```Chúng tôi có$18/9=2$. Phép chia dài ban đầu tạo ra các chữ số thương`[0,2]`. Số 0 đứng đầu bị loại bỏ, để lại`[2]`. Việc xây dựng sau đó sử dụng biểu diễn kinh điển của$K$, ngăn chặn các số 0 vô tình đứng đầu trong đầu ra.
