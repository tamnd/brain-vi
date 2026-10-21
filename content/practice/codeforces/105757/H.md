---
title: "CF 105757H - Câu đố của Klein Moretti"
description: "Chúng ta được cung cấp một mảng có độ dài n và kích thước dãy con cố định k. Với mỗi giá trị truy vấn x, chúng ta phải đếm xem có bao nhiêu chuỗi con chứa chính xác k phần tử có bitwise OR bằng x."
date: "2026-06-25T23:22:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105757
codeforces_index: "H"
codeforces_contest_name: "Insomnia 2025"
rating: 0
weight: 105757
solve_time_s: 50
verified: true
draft: false
---

[CF 105757H - Câu đố của Klein Moretti](https://codeforces.com/problemset/problem/105757/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài`n`và kích thước chuỗi con cố định`k`. Đối với mỗi giá trị truy vấn`x`, chúng ta phải đếm xem có bao nhiêu dãy con chứa chính xác`k`các phần tử có bitwise HOẶC bằng`x`. 

Một chuỗi con ở đây được xác định bởi các vị trí đã chọn, vì vậy nếu cùng một giá trị xuất hiện nhiều lần thì các lần xuất hiện khác nhau sẽ được tính riêng. 

Những hạn chế là thách thức chính. Cả hai`n`và số lượng truy vấn có thể lên tới một triệu và mỗi giá trị mảng cũng như mọi giá trị truy vấn nhiều nhất là một triệu. Từ`10^6 < 2^20`, mọi giá trị đều nằm trong 20 bit. Giải pháp xử lý từng truy vấn một cách độc lập sẽ bị loại trừ ngay lập tức. Thậm chí một`O(q log n)`Cách tiếp cận quá đắt khi`q = 10^6`. Hướng thực tế duy nhất là xử lý trước các câu trả lời cho mỗi mặt nạ 20 bit có thể một lần, sau đó trả lời từng truy vấn trong thời gian không đổi. 

Một lỗi phổ biến là nghĩ trực tiếp về giá trị OR của các dãy con. Số lượng các dãy con có thể có là rất lớn. 

Coi như:```
n = 3, k = 2
a = [1, 2, 4]
```Các dãy tiếp theo là:```
{1,2} -> 3
{1,4} -> 5
{2,4} -> 6
```Cố gắng tạo các giá trị OR một cách rõ ràng sẽ không mở rộng được. 

Một trường hợp tinh vi khác xuất hiện khi có nhiều giá trị giống hệt nhau:```
n = 4, k = 2
a = [1,1,1,1]
```OR luôn là`1`, nhưng câu trả lời là không`1`. Đó là:```
C(4,2) = 6
```Chúng tôi tính các lựa chọn vị trí, không tính các tập hợp giá trị riêng biệt. 

Trường hợp cạnh thứ ba xảy ra khi mặt nạ truy vấn chứa bit không bao giờ xuất hiện trong bất kỳ phần tử được chọn nào:```
n = 3, k = 2
a = [1,2,2]
query = 7
```Không có chuỗi con nào có thể tạo ra bit`4`, vậy câu trả lời là`0`. Bất kỳ cách tiếp cận nào chỉ theo dõi các tập hợp con của mặt nạ truy vấn mà không tôn trọng tần số thực tế đều có thể tạo ra kết quả khác 0 một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi dãy con có kích thước`k`, tính OR của nó và đếm số lần mỗi giá trị OR xuất hiện. 

Điều này đúng vì mọi dãy con hợp lệ đều được kiểm tra chính xác một lần. 

Vấn đề là số lượng các chuỗi:```
C(n,k)
```Ngay cả đối với các giá trị vừa phải của`n`, điều này trở nên lớn về mặt thiên văn. Với`n`lên đến một triệu, vũ phu là hoàn toàn không thể. 

Quan sát quan trọng đến từ việc đảo ngược câu hỏi. 

Thay vì hỏi:```
How many subsequences have OR exactly x?
```hỏi:```
How many subsequences have OR contained inside x?
```Một dãy con có OR chứa bên trong`x`nếu mọi phần tử được chọn chỉ sử dụng các bit đã có trong`x`. 

Đối với một mặt nạ`x`, cho phép:```
g[x] = number of array elements whose bit set is a subset of x
```Mỗi kích thước-`k`dãy sau được chọn từ đây`g[x]`phần tử có nhiều nhất OR`x`. 

Kể từ đây:```
F[x] = C(g[x], k)
```đếm các dãy con có OR là tập con của`x`. 

Bây giờ hãy:```
ans[x] = number of subsequences whose OR equals x
```Mỗi dãy con được tính vào`F[x]`đóng góp vào đúng một giá trị OR`y`với`y ⊆ x`. 

Vì thế:```
F[x] = Σ ans[y]
       y⊆x
```Đây là mối quan hệ biến đổi zeta tập hợp con cổ điển. 

Một lần tất cả`F[x]`đã biết giá trị, chúng tôi khôi phục các câu trả lời chính xác bằng cách sử dụng phép đảo ngược Möbius tập hợp con. 

Công việc còn lại là tính toán`g[x]`. 

Cho phép`freq[v]`là tần số của giá trị`v`trong mảng. 

Sau đó:```
g[x] = Σ freq[s]
       s⊆x
```đó chính xác là tập hợp con SOS DP. 

Vì chỉ có`2^20 = 1,048,576`mặt nạ, phép biến đổi SOS 20 chiều là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n,k) · k) | O(số giá trị OR) | Quá chậm | 
| Tối ưu | O(20 · 2^20 + n + q) | O(2^20) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và xây dựng mảng tần số`freq`, Ở đâu`freq[v]`lưu trữ giá trị bao nhiêu lần`v`xuất hiện. 
2. Tính trước giai thừa và nghịch đảo giai thừa modulo`10^9+7`lên đến`n`, cho phép tính toán các kết hợp theo thời gian không đổi. 
3. Sao chép`freq`thành một mảng`g`. 
4. Chạy biến đổi SOS tập hợp con trên`g`. 

Sau sự biến đổi này:```
g[x] = Σ freq[s]
       s⊆x
```bằng với số phần tử mảng có mặt nạ là tập con của`x`. 
5. Đối với mỗi chiếc mặt nạ`x`, tính:```
F[x] = C(g[x], k)
```Nếu như`g[x] < k`, giá trị bằng 0. 
6. Áp dụng tập con nghịch đảo Möbius cho`F`. 

Sau khi đảo ngược:```
F[x] = Σ ans[y]
       y⊆x
```trở thành```
ans[x]
```trực tiếp. 
7. Với mỗi giá trị truy vấn`x`, đầu ra`ans[x]`. 

### Tại sao nó hoạt động 

Đối với mặt nạ cố định`x`, phần tử duy nhất có thể tham gia vào một dãy con có OR được chứa bên trong`x`là các phần tử có mặt nạ riêng là tập hợp con của`x`. Biến đổi SOS tính toán chính xác có bao nhiêu phần tử như vậy tồn tại. 

Chọn bất kỳ`k`trong số các phần tử đó cho ra một dãy con có OR cũng là tập con của`x`, Vì thế`F[x] = C(g[x],k)`đếm tất cả các chuỗi con có OR nhiều nhất`x`. 

Mỗi dãy con đóng góp vào đúng một giá trị OR. Do đó,`F[x]`là tổng số OR chính xác trên tất cả các mặt nạ có trong`x`. Đảo ngược Möbius tập hợp con chính xác là phép toán nghịch đảo của phép tính tổng này, do đó, nó phục hồi chính xác số lượng các chuỗi con có OR bằng mỗi mặt nạ. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 1000000007
B = 20
M = 1 << B

def main():
    n, k = map(int, input().split())

    freq = array('I', [0]) * M
    arr = list(map(int, input().split()))
    for x in arr:
        freq[x] += 1

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    ifact = [1] * (n + 1)
    ifact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        ifact[i - 1] = ifact[i] * i % MOD

    def comb(nn, rr):
        if nn < rr:
            return 0
        return fact[nn] * ifact[rr] % MOD * ifact[nn - rr] % MOD

    g = array('I', freq)

    for bit in range(B):
        step = 1 << bit
        for mask in range(M):
            if mask & step:
                g[mask] += g[mask ^ step]

    dp = [0] * M
    for mask in range(M):
        dp[mask] = comb(g[mask], k)

    for bit in range(B):
        step = 1 << bit
        for mask in range(M):
            if mask & step:
                dp[mask] -= dp[mask ^ step]
                dp[mask] %= MOD

    q = int(input())
    out = []
    for _ in range(q):
        x = int(input())
        out.append(str(dp[x]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Mảng tần số lưu trữ số lần mỗi giá trị xuất hiện trong đầu vào. Vì tất cả các giá trị đều ở dưới`2^20`, mọi mặt nạ có thể đều có một khe chuyên dụng. 

Biến đổi SOS chuyển đổi tần số thành số lượng tập hợp con. Sau khi nó kết thúc,`g[x]`chứa số phần tử mảng có thể xuất hiện trong một dãy con có OR không vượt quá`x`. 

Các giá trị kết hợp được tính bằng cách sử dụng giai thừa và giai thừa nghịch đảo. Vì môđun là số nguyên tố nên định lý Fermat đưa ra nghịch đảo môđun. 

Mảng`dp`cửa hàng ban đầu`F[x] = C(g[x],k)`. Thẻ kiểu SOS thứ hai thực hiện đảo ngược Möbius. Bước trừ là nghịch đảo chính xác của việc tích lũy tập hợp con và biến số đếm "OR được chứa bên trong x" thành số đếm "OR bằng x". 

Phép toán modulo sau mỗi phép trừ là cần thiết vì các giá trị trung gian có thể trở thành âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 3, k = 2
a = [1, 2, 3]
```Mặt nạ liên quan: 

| Mặt nạ | g[mặt nạ] | F[mặt nạ] = C(g,k) | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 1 | 0 | 
| 2 | 1 | 0 | 
| 3 | 3 | 3 | 

Sau khi đảo ngược Möbius: 

| Mặt nạ | Số lượng HOẶC chính xác | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 3 | 

Ba dãy con đó là:```
{1,2} -> 3
{1,3} -> 3
{2,3} -> 3
```Tất cả đều góp phần tạo nên mặt nạ`3`. 

### Ví dụ 2```
n = 4, k = 2
a = [1,1,1,1]
```| Mặt nạ | g[mặt nạ] | F[mặt nạ] | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 4 | 6 | 

Sau khi đảo ngược: 

| Mặt nạ | Số lượng HOẶC chính xác | 
| --- | --- | 
| 0 | 0 | 
| 1 | 6 | 

Ví dụ này chứng tỏ rằng các chuỗi con được tính theo vị trí. Mặc dù chỉ có một giá trị riêng biệt nhưng có sáu cách khác nhau để chọn hai vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(20 · 2^20 + n + q) | Hai phép biến đổi kiểu SOS chiếm ưu thế | 
| Không gian | O(2^20) | Mảng tần số và DP trên tất cả các mặt nạ |`2^20`là khoảng một triệu, vì vậy mỗi biến đổi SOS thực hiện khoảng hai mươi triệu cập nhật. Điều này vừa vặn một cách thoải mái trong giới hạn cho giải pháp C++ 2 giây và cũng là mức độ phức tạp dự kiến ​​của vấn đề. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    MOD = 1000000007
    B = 20
    M = 1 << B

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    from array import array

    freq = array('I', [0]) * M
    for x in arr:
        freq[x] += 1

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    ifact = [1] * (n + 1)
    ifact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        ifact[i - 1] = ifact[i] * i % MOD

    def C(nn, rr):
        if nn < rr:
            return 0
        return fact[nn] * ifact[rr] % MOD * ifact[nn - rr] % MOD

    g = array('I', freq)

    for bit in range(B):
        b = 1 << bit
        for mask in range(M):
            if mask & b:
                g[mask] += g[mask ^ b]

    dp = [0] * M
    for mask in range(M):
        dp[mask] = C(g[mask], k)

    for bit in range(B):
        b = 1 << bit
        for mask in range(M):
            if mask & b:
                dp[mask] = (dp[mask] - dp[mask ^ b]) % MOD

    q = int(input())
    ans = []
    for _ in range(q):
        ans.append(str(dp[int(input())]))
    return "\n".join(ans)

# custom cases
assert run("1 1\n1\n2\n1\n2\n") == "1\n0"
assert run("4 2\n1 1 1 1\n2\n1\n3\n") == "6\n0"
assert run("3 2\n1 2 4\n4\n3\n5\n6\n7\n") == "1\n1\n1\n0"
assert run("3 3\n7 7 7\n2\n7\n1\n") == "1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn |`1, 0`| Trường hợp kích thước tối thiểu | 
| Tất cả các giá trị bằng nhau |`6, 0`| Đếm tổ hợp đúng | 
| Sức mạnh riêng biệt của hai |`1,1,1,0`| Chính xác HOẶC tái thiết | 
|`k = n`|`1,0`| Ranh giới chỉ tồn tại một dãy con | 

## Vỏ cạnh 

Hãy xem xét:```
n = 4
k = 2
a = [1,1,1,1]
query = 1
```Phép biến đổi SOS mang lại:```
g[1] = 4
```Vì thế:```
F[1] = C(4,2) = 6
```Lá đảo ngược của Mobius`ans[1] = 6`. Thuật toán tính các lựa chọn vị trí thay vì các giá trị riêng biệt, đó chính xác là những gì vấn đề yêu cầu. 

Coi như:```
n = 3
k = 2
a = [1,2,2]
query = 7
```Không có giá trị mảng nào chứa bit`4`, do đó mọi dãy con OR đều được chứa bên trong`3`. 

Giai đoạn SOS chỉ tính toán số lượng hợp lệ từ các mặt nạ hiện có. Trong quá trình đảo ngược, mặt nạ`7`không nhận được sự đóng góp nào, để lại:```
ans[7] = 0
```cái nào đúng. 

Coi như:```
n = 3
k = 3
a = [1,2,4]
query = 7
```Chỉ tồn tại một dãy con sử dụng tất cả các vị trí. 

Thuật toán thu được:```
g[7] = 3
F[7] = C(3,3) = 1
```và nghịch đảo tạo ra:```
ans[7] = 1
```trong khi mọi mặt nạ khác đều không nhận được. Điều này xác nhận việc xử lý đúng trường hợp cực đoan`k = n`.
