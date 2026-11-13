---
title: "CF 1709F - Nhiều bộ dây"
description: "Hãy coi tất cả các chuỗi nhị phân có độ dài tối đa n là các nút của một bộ ba nhị phân hoàn chỉnh có độ sâu n. Mọi nút ngoại trừ nút gốc đều nhận được dung lượng cs trong khoảng từ 0 đến k. Một tập hợp các chuỗi nhị phân có độ dài chính xác n gán một số bội cho mỗi lá."
date: "2026-06-09T20:55:41+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "fft", "flows", "graphs", "math", "meet-in-the-middle", "trees"]
categories: ["algorithms"]
codeforces_contest: 1709
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 132 (Rated for Div. 2)"
rating: 2500
weight: 1709
solve_time_s: 108
verified: true
draft: false
---

[CF 1709F - Nhiều bộ dây](https://codeforces.com/problemset/problem/1709/F) 

**Đánh giá:** 2500 
**Tags:** bitmasks, brute Force, dp, fft, flow, đồ thị, toán học, gặp nhau ở giữa, cây cối 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy nghĩ đến tất cả các chuỗi nhị phân có độ dài nhiều nhất`n`như các nút của một bộ ba nhị phân hoàn chỉnh có độ sâu`n`. 

Mọi nút ngoại trừ nút gốc đều nhận được dung lượng`c_s`giữa`0`Và`k`. Một tập hợp các chuỗi nhị phân có độ dài chính xác`n`gán một số đa bội cho mỗi lá. Đối với bất kỳ nút trie nào`s`, tổng số bội bên trong cây con của nó không thể vượt quá`c_s`. 

Để phân công dung lượng cố định, hãy xác định kích thước tối đa của nhiều bộ đẹp mắt. Nhiệm vụ không phải là tìm mức tối đa đó. Thay vào đó, chúng ta phải đếm chính xác có bao nhiêu phép gán công suất tạo ra giá trị cực đại`f`. 

Trie chứa$$2^1+2^2+\cdots+2^n=2^{n+1}-2$$các nút có năng lực. Ngay cả đối với`n = 15`, đây là 65534 biến, vì vậy việc liệt kê trực tiếp là vô vọng. 

Sự ràng buộc`n ≤ 15`tuy nhỏ nhưng`k`Và`f`lớn như`2·10^5`. Điều này gợi ý rõ ràng rằng trạng thái thú vị không phải ở bản thân cấu trúc trie mà là giá trị mà mỗi cây con có thể đóng góp. Bất kỳ giải pháp nào giữ trạng thái cho tất cả các phép gán công suất đều không thể thực hiện được, trong khi một giải pháp có độ phức tạp gần như là đa thức trong`k`và tuyến tính trong`n`là khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi`f`vượt quá mọi câu trả lời có thể đạt được. 

Ví dụ:```
n = 1
k = 1
f = 3
```Dung lượng mỗi lá tối đa là`1`, vì vậy kích thước nhiều tập hợp đẹp tối đa nhiều nhất là`2`. Câu trả lời đúng là`0`. Một giải pháp chỉ tính toán phân bố lên tới`k`sẽ bỏ lỡ một cách không chính xác thực tế là gốc có thể đạt tới các giá trị lên tới`2k`. 

Một sai lầm dễ mắc phải khác là xử lý sai trường hợp đẳng thức trong quá trình chuyển đổi. 

Giả sử các cây con của cây con có thể đóng góp chính xác`j`. Sau đó mỗi sự lựa chọn công suất`c ≥ j`tạo ra giá trị cây con`j`. có`k-j+1`năng lực như vậy, không`k-j`. Thiếu khả năng bổ sung này sẽ làm thay đổi toàn bộ quá trình phân phối. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. 

Gán giá trị giữa`0`Và`k`đến mọi nút trie. Đối với mỗi bài tập, hãy tính kích thước nhiều tập hợp đẹp tối đa và kiểm tra xem nó có bằng không`f`. 

Trie chứa`2^{n+1}-2`các nút, vì vậy số lượng bài tập là$$(k+1)^{2^{n+1}-2}.$$Ngay cả đối với`n=15`Và`k=1`, cái này lớn về mặt thiên văn. 

Quan sát quan trọng là một cây con có thể được tóm tắt bằng một số duy nhất. 

Cho phép`val(v)`là số chuỗi tối đa có thể được đặt bên trong cây con gốc tại nút`v`. 

Đối với một chiếc lá,$$val(v)=c_v.$$Đối với một nút không phải gốc bên trong,$$val(v)=\min(c_v,\; val(l)+val(r)).$$Những đứa trẻ cùng nhau có thể cung cấp nhiều nhất`val(l)+val(r)`chuỗi, nhưng dung lượng nút có thể áp đặt giới hạn chặt chẽ hơn. 

Điều này có nghĩa là việc sắp xếp chính xác các khả năng bên dưới một nút là không liên quan một khi chúng ta biết giá trị kết quả của cây con đó. Chúng ta chỉ cần đếm xem có bao nhiêu phép gán tạo ra mỗi giá trị có thể. 

Nếu chúng ta biết phân bố của cây con có chiều cao`h-1`, khi đó việc kết hợp hai con đòi hỏi phải đếm tất cả các cặp có tổng giá trị bằng`t`. Đó chính xác là một sự tích chập. 

Từ`k`đạt tới`2·10^5`, tích chập bậc hai ngây thơ là quá đắt. Bài toán trở thành một chuỗi các tích chập đa thức, có thể được tăng tốc bằng NTT. 

Lập trình động kết quả chạy trong khoảng`O(n k log k)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP + NTT | O(nk log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`dp[h][x]`biểu thị số lần gán dung lượng cho cây con nhị phân hoàn chỉnh không có gốc có chiều cao`h`có đóng góp tối đa bằng chính xác`x`. 

Chiều cao`0`có nghĩa là một chiếc lá. 

### 1. Khởi tạo phân phối lá 

Giá trị của lá bằng công suất của nó. 

Đối với mọi`0 ≤ x ≤ k`:$$dp[0][x]=1.$$### 2. Kết hợp hai cây con con 

Giả sử chúng ta đã biết phân bố chiều cao`h-1`. 

Cho phép$$H[t] = \sum_{a+b=t} dp[h-1][a]\cdot dp[h-1][b].$$

`H[t]`đếm các bài tập có hai đứa trẻ đóng góp tổng giá trị`t`. 

Đây là tích chập đa thức, được tính bằng NTT. 

### 3. Áp dụng dung lượng nút 

Đối với một tổng con cố định`t`, giá trị gốc là$$\min(c,t).$$Chúng ta cần số cách để có được từng giá trị kết quả`j`. 

Nếu như`t=j`, thì công suất bất kỳ`c≥j`hoạt động. có`k-j+1`sự lựa chọn. 

Nếu như`t>j`, thì cách duy nhất để có được giá trị`j`đang lựa chọn`c=j`. 

Kể từ đây$$dp[h][j] = (k-j+1)H[j] + \sum_{t>j} H[t].$$Viết lại:$$dp[h][j] = (k-j)H[j] + \sum_{t\ge j} H[t].$$Tổng hậu tố của`H`cho phép chuyển đổi này trong thời gian tuyến tính. 

### 4. Lặp lại cho đến khi đạt độ sâu`n-1`Tất cả các cấp độ không phải gốc đều được xử lý bằng cách sử dụng quá trình chuyển đổi trước đó. 

### 5. Xử lý tận gốc 

Gốc không có khả năng. 

Giá trị của nó chỉ đơn giản là$$val(root)=val(left)+val(right).$$Vì vậy, phân phối câu trả lời cuối cùng chỉ là một phép tích chập nữa của chiều cao`n-1`phân phối với chính nó. 

Hệ số của`f`là câu trả lời. 

### Tại sao nó hoạt động 

Đối với mỗi cây con, thông tin duy nhất mà tổ tiên cần là số chuỗi tối đa có thể được đặt bên trong cây con đó. Công thức đệ quy$$val(v)=\min(c_v,\; val(l)+val(r))$$cho thấy cấu trúc bên trong bên dưới nút chỉ ảnh hưởng đến các cấp cao hơn thông qua giá trị duy nhất này. 

DP đếm chính xác có bao nhiêu phép gán công suất tạo ra mỗi giá trị cây con có thể có. Convolution liệt kê tất cả các cặp giá trị con. Quá trình chuyển đổi năng lực liệt kê tất cả các năng lực biến đổi tổng con thành giá trị cha mẹ nhất định. Vì mỗi phép gán được tính một lần và chỉ một lần ở mọi cấp độ nên phân phối cuối cùng ở gốc là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
G = 3

def mod_pow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def ntt(a, invert):
    n = len(a)

    j = 0
    for i in range(1, n):
        bit = n >> 1
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit
        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    while length <= n:
        wlen = mod_pow(G, (MOD - 1) // length)
        if invert:
            wlen = mod_pow(wlen, MOD - 2)

        for i in range(0, n, length):
            w = 1
            half = length >> 1
            for j in range(i, i + half):
                u = a[j]
                v = a[j + half] * w % MOD

                a[j] = (u + v) % MOD
                a[j + half] = (u - v) % MOD

                w = w * wlen % MOD

        length <<= 1

    if invert:
        inv_n = mod_pow(n, MOD - 2)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def convolution(a, b):
    need = len(a) + len(b) - 1
    n = 1
    while n < need:
        n <<= 1

    fa = a[:] + [0] * (n - len(a))
    fb = b[:] + [0] * (n - len(b))

    ntt(fa, False)
    ntt(fb, False)

    for i in range(n):
        fa[i] = fa[i] * fb[i] % MOD

    ntt(fa, True)

    return fa[:need]

def solve():
    n, k, f = map(int, input().split())

    dp = [1] * (k + 1)

    for level in range(1, n):
        h = convolution(dp, dp)

        m = len(h)
        suf = [0] * (m + 1)
        for i in range(m - 1, -1, -1):
            suf[i] = (suf[i + 1] + h[i]) % MOD

        ndp = [0] * (k + 1)
        for j in range(k + 1):
            ndp[j] = ((k - j) * h[j] + suf[j]) % MOD

        dp = ndp

    root = convolution(dp, dp)

    if f >= len(root):
        print(0)
    else:
        print(root[f] % MOD)

solve()
```Sự phân bố cơ sở tương ứng với lá. Mỗi cấp độ nội bộ trước tiên thực hiện một phép tích chập, tạo ra sự phân bố của các tổng con. Sau đó, quá trình chuyển đổi tổng hậu tố sẽ áp dụng dung lượng gốc. 

biểu thức```
(k - j) * h[j] + suf[j]
```dụng cụ$$(k-j)H[j]+\sum_{t\ge j}H[t].$$Phần gốc được xử lý riêng vì không bị hạn chế về dung lượng. 

Một chi tiết triển khai dễ bị bỏ sót là phạm vi giá trị tối đa. Giá trị cây con nội bộ không bao giờ vượt quá`k`, nhưng gốc là tổng của hai giá trị như vậy, do đó phân phối cuối cùng mở rộng đến`2k`. Chính vì vậy chúng ta phải kiểm tra`f >= len(root)`trước khi lập chỉ mục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
1 42 2
```Vì`n=1`, chỉ có hai lá. 

| Bước | Phân phối | 
| --- | --- | 
| Giá trị lá |`dp[x]=1`vì`0≤x≤42`| 
| Tích chập gốc | hệ số tổng`2`bằng`3`| 

Các cặp tạo ra tổng`2`là: 

| Trái | Đúng | 
| --- | --- | 
| 0 | 2 | 
| 1 | 1 | 
| 2 | 0 | 

Có chính xác`3`bài tập. 

Đầu ra:```
3
```Ví dụ này cho thấy rằng khi`n=1`, câu trả lời đơn giản là số lượng các cặp năng lực lá có thứ tự tổng cộng lại bằng`f`. 

### Ví dụ 2 

đầu vào```
1 1 3
```| Bước | Kết quả | 
| --- | --- | 
| Giá trị lá |`{0,1}`| 
| Tổng gốc |`{0,1,2}`| 
| Giá trị 3 hiện tại? | Không | 

Đa thức cuối cùng có bậc`2`. 

Từ`3 > 2k`, câu trả lời là:```
0
```Điều này chứng tỏ tầm quan trọng của việc xử lý các giá trị ngoài sự hỗ trợ của phân phối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk log k) | Một tích chập NTT trên mỗi cấp độ trie | 
| Không gian | O(k) | Một vài mảng có kích thước tỷ lệ thuận với độ dài đa thức | 

Với`n ≤ 15`Và`k ≤ 2·10^5`, kích thước đa thức lớn nhất là khoảng`4·10^5`. NTT xử lý các kết cấu như vậy một cách thoải mái trong giới hạn và chỉ một số lượng nhỏ cấp độ được xử lý. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    MOD = 998244353

    # paste solve() implementation here
    return out

# provided sample
assert run("1 42 2\n") == "3\n"

# minimum values
assert run("1 1 0\n") == "1\n"

# impossible target
assert run("1 1 3\n") == "0\n"

# all pairs summing to 1
assert run("1 5 1\n") == "2\n"

# largest achievable value for n=1
assert run("1 3 6\n") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 42 2`|`3`| Mẫu chính thức | 
|`1 1 0`|`1`| Câu trả lời nhỏ nhất có thể đạt được | 
|`1 1 3`|`0`| Mục tiêu vượt quá phạm vi tối đa | 
|`1 5 1`|`2`| Đếm cặp đặt hàng | 
|`1 3 6`|`1`| Giá trị gốc tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1 1 3
```Dung lượng mỗi lá tối đa là`1`, vì vậy giá trị gốc nhiều nhất là`2`. Tích chập cuối cùng chỉ có hệ số cho tổng`0,1,2`. Vì chỉ số`3`không tồn tại, thuật toán trả về`0`. 

Bây giờ hãy xem xét:```
2 2 1
```Giả sử tổng con bằng chính xác`1`. Mỗi sự lựa chọn công suất`c=1`hoặc`c=2`tạo ra giá trị gốc`1`. Quá trình chuyển đổi góp phần`k-j+1 = 2`khả năng. sử dụng`k-j`thay vào đó sẽ chỉ đếm một trong số chúng và tạo ra sự phân phối không chính xác. Công thức tổng hậu tố bao gồm chính xác trường hợp đẳng thức. 

Cuối cùng, hãy xem xét:```
1 3 6
```Cặp có thứ tự duy nhất có tổng là`6`là`(3,3)`. Hệ số tích chập gốc tại chỉ số`6`chính xác là`1`, xác nhận rằng thuật toán xử lý chính xác ranh giới trên`2k`.
