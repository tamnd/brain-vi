---
title: "CF 1288C - Hai mảng"
description: "Chúng ta cần đếm các cặp mảng $(a,b)$, cả hai đều có độ dài $m$, có giá trị nằm trong khoảng từ $1$ đến $n$. Mảng $a$ phải không giảm, mảng $b$ không tăng và ở mọi vị trí chúng ta đều yêu cầu $ai le bi$. Câu trả lời có thể rất lớn, vì vậy chúng tôi xuất nó theo modulo $10^9+7$."
date: "2026-06-11T19:02:18+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 1288
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 80 (Rated for Div. 2)"
rating: 1600
weight: 1288
solve_time_s: 211
verified: true
draft: false
---

[CF 1288C - Hai mảng](https://codeforces.com/problemset/problem/1288/C) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 3 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các cặp mảng$(a,b)$, cả hai đều có chiều dài$m$, có giá trị nằm giữa$1$Và$n$. 

Mảng$a$phải không giảm, mảng$b$phải không tăng và tại mọi vị trí chúng ta yêu cầu$a_i \le b_i$. 

Câu trả lời có thể rất lớn, vì vậy chúng tôi xuất nó theo modulo$10^9+7$. 

Điều đầu tiên cần chú ý là$m$nhiều nhất là rất nhỏ$10$, trong khi$n$có thể đạt được$1000$. Bất kỳ giải pháp nào có độ phức tạp là đa thức trong$n$Và$m$được rồi. Giải pháp cố gắng liệt kê các mảng là vô vọng vì số lượng mảng có thể có là theo cấp số nhân. Ngay cả đối với$n=1000$Và$m=10$, có$1000^{10}$mảng có thể. 

Phần thú vị là sự tương tác giữa các ràng buộc đơn điệu và điều kiện$a_i \le b_i$. Các mảng không độc lập. 

Hãy xem xét một ví dụ nhỏ:```
n = 2, m = 2
```Các cặp hợp lệ chính xác là năm cặp được liệt kê trong câu lệnh. 

Một lỗi phổ biến là đếm mảng không giảm$a$và mảng không tăng$b$riêng biệt và sau đó cố gắng kết hợp chúng. Điều đó bỏ qua ràng buộc về vị trí$a_i \le b_i$. Ví dụ,$$a=[1,2],\qquad b=[1,1]$$thỏa mãn yêu cầu về tính đơn điệu nhưng lại vi phạm$a_2 \le b_2$. 

Một trường hợp tế nhị khác là khi$n=1$.```
1 10
```Mỗi phần tử phải$1$, vậy có đúng một cặp hợp lệ:$$a=[1,1,\ldots,1],\quad b=[1,1,\ldots,1].$$Bất kỳ công thức nào vô tình thừa nhận có nhiều giá trị sẽ bị tính quá mức. 

Trường hợp cạnh thứ ba xảy ra khi sự bình đẳng xảy ra thường xuyên. Ví dụ:```
2 1
```Các cặp hợp lệ là$$(1,1),\ (1,2),\ (2,2).$$Câu trả lời là$3$, không$2$. Các giải pháp điều trị$a_i<b_i$thay vì$a_i\le b_i$mất tất cả các cặp bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo mọi mảng không giảm$a$, tạo ra mọi mảng không tăng$b$, và kiểm tra xem$a_i\le b_i$cho mọi vị trí. 

Số lượng mảng có chiều dài không giảm$m$vượt quá giá trị$1\ldots n$là$$\binom{n+m-1}{m}.$$Vì$n=1000$Và$m=10$, cái này đã có sẵn rồi$2.8\times10^{23}$. Việc liệt kê chúng là hoàn toàn không thể. 

Quan sát chính là hai mảng có thể được hợp nhất thành một chuỗi đơn điệu. 

Viết các giá trị của$a$từ trái sang phải:$$a_1 \le a_2 \le \cdots \le a_m.$$Bây giờ viết các giá trị của$b$theo thứ tự ngược lại:$$b_m \le b_{m-1} \le \cdots \le b_1.$$Bởi vì$a_i \le b_i$, kết nối ở giữa cũng tôn trọng thứ tự:$$a_m \le b_m.$$Kết hợp mọi thứ mang lại$$a_1 \le a_2 \le \cdots \le a_m \le b_m \le b_{m-1} \le \cdots \le b_1.$$Đây chỉ đơn giản là một chuỗi có độ dài không giảm$2m$giá trị của nó nằm ở$1\ldots n$. 

Ngược lại, mọi dãy có độ dài không giảm$2m$xác định duy nhất một cặp hợp lệ$(a,b)$:$$a_i = s_i, \qquad b_i = s_{2m-i+1}.$$Vì vậy, bài toán ban đầu hoàn toàn tương đương với việc đếm các chuỗi có độ dài không giảm$2m$qua$n$các giá trị. 

Đó là số lượng sao và thanh cổ điển:$$\binom{n+2m-1}{2m}.$$Vì mô đun là số nguyên tố và$n+2m-1 \le 1019$, chúng ta có thể tính hệ số nhị thức này một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để$$N = n + 2m - 1, \qquad K = 2m.$$2. Quan sát rằng mọi cặp hợp lệ$(a,b)$tương ứng với một dãy không giảm$$a_1,\ldots,a_m,b_m,\ldots,b_1$$chiều dài$2m$. 
3. Quan sát thấy mọi dãy có độ dài không giảm$2m$vượt quá giá trị$1\ldots n$xây dựng lại chính xác một cặp hợp lệ. 

Sự tương ứng là tính từ, vì vậy việc đếm các cặp cũng giống như việc đếm các chuỗi đó. 
4. Đếm các dãy có độ dài không giảm$2m$sử dụng các ngôi sao và thanh. 

Số lượng nhiều bộ kích thước$2m$được chọn từ$n$giá trị bằng$$\binom{n+2m-1}{2m}.$$5. Tính hệ số nhị thức modulo$10^9+7$. 

Vì mô đun là số nguyên tố nên hãy sử dụng giai thừa và nghịch đảo mô đun:$$\binom{N}{K} = \frac{N!}{K!(N-K)!}.$$6. Xuất kết quả. 

### Tại sao nó hoạt động 

Toàn bộ giải pháp dựa trên sự song song. 

Cho một cặp hợp lệ$(a,b)$, trình tự$$a_1,\ldots,a_m,b_m,\ldots,b_1$$không giảm vì$a$không giảm,$b$không tăng và điều kiện$a_m\le b_m$kết nối hai nửa. 

Cho bất kỳ dãy có độ dài không giảm nào$2m$, gán nửa đầu của nó cho$a$và nửa sau của nó đảo ngược thành$b$tự động tạo ra một giá trị không giảm$a$, không tăng$b$, Và$a_i\le b_i$cho mọi vị trí. Hai cấu trúc này là nghịch đảo của nhau, do đó ánh xạ là một đối một và trên. 

Do đó, câu trả lời chính xác là số dãy có độ dài không giảm$2m$qua$n$những giá trị có thể có, đó là$\binom{n+2m-1}{2m}$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, m = map(int, input().split())

N = n + 2 * m - 1
K = 2 * m

fact = [1] * (N + 1)
for i in range(1, N + 1):
    fact[i] = fact[i - 1] * i % MOD

inv_fact = [1] * (N + 1)
inv_fact[N] = pow(fact[N], MOD - 2, MOD)

for i in range(N, 0, -1):
    inv_fact[i - 1] = inv_fact[i] * i % MOD

ans = fact[N]
ans = ans * inv_fact[K] % MOD
ans = ans * inv_fact[N - K] % MOD

print(ans)
```Mã trực tiếp thực hiện công thức tổ hợp. 

giá trị$N=n+2m-1$là đỉnh của hệ số nhị thức và$K=2m$là đáy. 

Giai thừa được tính toán trước modulo$10^9+7$. Vì môđun là số nguyên tố nên định lý Fermat cho$$x^{-1} \equiv x^{MOD-2} \pmod{MOD}.$$Tính giai thừa nghịch đảo của$N$đầu tiên và sau đó điền ngược các giai thừa nghịch đảo còn lại là kỹ thuật thời gian tuyến tính tiêu chuẩn. 

Lớn nhất có thể$N$là$$1000 + 20 - 1 = 1019,$$vì vậy các mảng rất nhỏ và vừa vặn thoải mái trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
```Chúng tôi tính toán:$$N = 2 + 4 - 1 = 5, \qquad K = 4.$$| Biến | Giá trị | 
| --- | --- | 
| n | 2 | 
| m | 2 | 
| N | 5 | 
| K | 4 | 
| Trả lời |$\binom{5}{4}=5$| 

Đầu ra:```
5
```Ví dụ này thể hiện sự song ánh. Các cặp hợp lệ tương ứng chính xác với năm chuỗi không giảm có độ dài bằng bốn giá trị$\{1,2\}$. 

### Ví dụ 2 

đầu vào:```
1 3
```Chúng tôi tính toán:$$N = 1 + 6 - 1 = 6, \qquad K = 6.$$| Biến | Giá trị | 
| --- | --- | 
| n | 1 | 
| m | 3 | 
| N | 6 | 
| K | 6 | 
| Trả lời |$\binom{6}{6}=1$| 

Đầu ra:```
1
```Chỉ có giá trị$1$tồn tại nên mọi vị trí trong cả hai mảng đều bị ép buộc. Công thức trả về chính xác một cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Xây dựng mảng giai thừa và nghịch đảo giai thừa lên đến$n+2m-1$| 
| Không gian | O(n + m) | Lưu trữ cho bảng giai thừa | 

Từ$n+2m-1\le1019$, thời gian chạy thực tế là rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys

def solve():
    MOD = 10**9 + 7

    n, m = map(int, input().split())

    N = n + 2 * m - 1
    K = 2 * m

    fact = [1] * (N + 1)
    for i in range(1, N + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (N + 1)
    inv_fact[N] = pow(fact[N], MOD - 2, MOD)

    for i in range(N, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    ans = fact[N]
    ans = ans * inv_fact[K] % MOD
    ans = ans * inv_fact[N - K] % MOD

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("2 2\n") == "5\n", "sample 1"

# minimum size
assert run("1 1\n") == "1\n", "minimum"

# single position
assert run("2 1\n") == "3\n", "all valid pairs"

# only one value available
assert run("1 10\n") == "1\n", "forced arrays"

# another small hand-checkable case
assert run("2 3\n") == "7\n", "C(7,6)=7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Ví dụ nhỏ nhất có thể | 
|`2 1`|`3`| Xử lý đúng sự bình đẳng | 
|`1 10`|`1`| Chỉ có một giá trị | 
|`2 3`|`7`| Công thức sao và thanh đúng | 

## Vỏ cạnh 

Hãy xem xét:```
1 10
```Thuật toán tính toán$$\binom{1+20-1}{20} = \binom{20}{20} = 1.$$Chỉ có một dãy có độ dài không giảm$20$tồn tại, cụ thể là tất cả những cái một. Điều này ánh xạ tới cặp mảng hợp lệ duy nhất. 

Coi như:```
2 1
```Thuật toán tính toán$$\binom{2+2-1}{2} = \binom{3}{2} = 3.$$Ba cặp đó là$(1,1)$,$(1,2)$, Và$(2,2)$. Các trường hợp đẳng thức được tính một cách tự nhiên vì các ngôi sao và thanh tính nhiều tập hợp thay vì tăng dần các chuỗi. 

Coi như:```
2 2
```Trình tự hợp nhất có độ dài$4$. Số lượng chuỗi không giảm trên các giá trị$\{1,2\}$là$$\binom{5}{4}=5.$$Tái thiết$a$từ hai vị trí đầu tiên và$b$từ hai vị trí cuối cùng bị đảo ngược sẽ mang lại chính xác năm cặp hợp lệ. Phép đối chiếu đảm bảo rằng không có cặp hợp lệ nào bị bỏ sót và không có cặp nào được tính hai lần.
