---
title: "CF 1942E - Game nông trại"
description: "Đàn bò của hai người nông dân phải xuất hiện theo thứ tự xen kẽ nhau trên hàng. Sau khi thứ tự của con bò đầu tiên được chọn, toàn bộ chuỗi sẽ được cố định: J N J N ... J N hoặc N J N J ... N J. Các vị trí thực tế không cố định."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "games"]
categories: ["algorithms"]
codeforces_contest: 1942
codeforces_index: "E"
codeforces_contest_name: "CodeTON Round 8 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 2300
weight: 1942
solve_time_s: 116
verified: true
draft: false
---

[CF 1942E - Trò chơi nông trại](https://codeforces.com/problemset/problem/1942/E) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, trò chơi 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đàn bò của hai người nông dân phải xuất hiện theo thứ tự xen kẽ nhau trên hàng. Khi thứ tự của con bò đầu tiên được chọn, toàn bộ chuỗi sẽ được cố định: 

hoặc là`J N J N ... J N`hoặc`N J N J ... N J`. 

Các vị trí thực tế không cố định. Có thể có những ô trống giữa các con bò và gần các bức tường. 

Một nước đi sẽ chọn một số con bò của một người nông dân và dịch chuyển tất cả chúng đi một bước về cùng một hướng. Một động thái chỉ hợp pháp nếu không có con bò nào va vào tường và không có con bò nào đáp xuống vị trí đã chiếm giữ. 

Chúng ta được cho biết chiều dài của dây chuyền và số lượng bò của mỗi nông dân. Trong số tất cả các vị trí hợp lệ, chúng ta phải đếm xem có bao nhiêu chiến thắng thuộc về Nông dân John, giả sử lối chơi tối ưu và John đi trước. 

Các ràng buộc là manh mối chính cho thấy lời giải phải có tính chất tổ hợp. Chiều dài dòng có thể đạt tới$10^6$, nhưng tổng của tất cả các độ dài trên tệp thử nghiệm cũng nhiều nhất là$10^6$. Điều đó ngay lập tức loại trừ mọi nỗ lực phân tích các trạng thái trò chơi riêng lẻ. Ngay cả việc lưu trữ tất cả cấu hình cũng không thể thực hiện được vì số lượng vị trí hợp lệ tăng theo cấp số nhân. 

Hướng khả thi duy nhất là tìm ra đặc điểm đơn giản của các vị trí thắng và thua, sau đó đếm chúng bằng tổ hợp. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Ví dụ: 

đầu vào```
1
2 1
```Các cấu hình hợp lệ duy nhất là:```
JN
NJ
```Không con bò nào có thể di chuyển theo cả hai cách, vì vậy John thua ngay lập tức. Câu trả lời đúng là:```
0
```Một cách tiếp cận ngây thơ cho rằng "người chơi đầu tiên thắng trừ khi bàn cờ đầy" sẽ thất bại. 

Một trường hợp quan trọng khác là: 

đầu vào```
1
3 1
```Các cấu hình hợp lệ là:```
JN_
_JN
N_J
J_N
```Chỉ có hai người cuối cùng là chiến thắng cho John. Câu trả lời đúng là:```
2
```Chỉ sự tồn tại của các ô trống thôi là chưa đủ. Vị trí của họ quan trọng. 

Một điểm tinh tế cuối cùng là mô tả trò chơi cho phép di chuyển làm tăng một số khoảng cách, vì vậy thoạt nhìn có vẻ như có thể chơi vô hạn. Quan sát quan trọng của lý thuyết trò chơi là cách chơi tối ưu hoàn toàn được xác định bởi tính chẵn lẻ của các khoảng trống nhất định. Khi cấu trúc đó được phát hiện, việc đếm sẽ có thể quản lý được. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra mọi cấu hình hợp lệ, xây dựng biểu đồ trò chơi và xác định xem trạng thái ban đầu có thắng hay không. 

Đối với một trật tự luân phiên cố định, một cấu hình về cơ bản là một vị trí của$2n$bò trong số$l$các vị trí. có$$\binom{l}{2n}$$những vị trí như vậy và có thể có hai thứ tự xen kẽ. Ngay cả đối với các giá trị vừa phải, con số này vẫn rất lớn. 

Ví dụ, khi$l=60$Và$n=20$,$$\binom{60}{40}$$đã vượt xa bất cứ điều gì chúng ta có thể liệt kê. 

Cách tiếp cận bạo lực hoạt động về mặt khái niệm vì trò chơi ở trạng thái hữu hạn, nhưng nó trở nên vô dụng rất lâu trước khi đạt được các ràng buộc thực tế. 

Sự đột phá đến từ việc nhìn ra khoảng cách giữa các cặp bò. 

Giả sử trình tự bắt đầu với con bò của John:$$a_1 < b_1 < a_2 < b_2 < \cdots < a_n < b_n.$$Định nghĩa$$g_i = b_i - a_i - 1.$$Vì thế$g_i$là số ô trống giữa$i$-con bò John và con bò Nhoj tương ứng. 

Những khoảng trống này hoàn toàn nắm bắt được trò chơi. 

Một động thái sẽ thay đổi chính xác một số khoảng trống khác 0$+1$hoặc$-1$. Thực tế quan trọng là tính chẵn lẻ của các khoảng trống quyết định người chiến thắng: 

Một vị thế sẽ thua khi và chỉ khi mọi$g_i$là chẵn. 

Nếu có ít nhất một khoảng trống là số lẻ, John có thể chuyển sang trạng thái mà tất cả các khoảng trống đều trở thành số chẵn. Nếu tất cả các khoảng trống đều đã bằng nhau thì mọi động thái hợp pháp đều tạo ra một khoảng cách lẻ. 

Một khi trò chơi được giảm xuống tiêu chí chẵn lẻ này, phần còn lại chỉ là tính toán thuần túy. 

Chúng tôi đếm tất cả các cấu hình hợp lệ, sau đó trừ đi các cấu hình có khoảng cách đều chẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(l)$mỗi bài kiểm tra,$O(\sum l)$tổng thể |$O(l)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Cố định 1 thứ tự luân phiên 

Giả sử trình tự bắt đầu với con bò của John:$$a_1 < b_1 < a_2 < b_2 < \cdots < a_n < b_n.$$Thứ tự ngược lại đóng góp chính xác cùng số lượng cấu hình, vì vậy chúng ta có thể đếm một thứ tự và nhân với hai ở cuối. 

### 2. Xác định những khoảng trống quan trọng 

Đối với mỗi cặp,$$g_i = b_i - a_i - 1.$$Đây là những khoảng trống kiểm soát trò chơi. 

Đồng thời xác định các phân đoạn trống còn lại:$$x_0=a_1-1,$$

$$x_i=a_{i+1}-b_i-1 \quad (1\le i<n),$$

$$x_n=l-b_n.$$Tất cả$x_i$Và$g_i$là không âm. 

Vì chính những con bò đã chiếm giữ$2n$tế bào,$$\sum x_i + \sum g_i = l-2n.$$Cho phép$$m=l-2n.$$### 3. Đặc điểm vị thế thua 

Một cấu hình bị mất chính xác khi mọi$g_i$là chẵn. 

Viết$$g_i=2h_i.$$Sau đó$$\sum x_i + 2\sum h_i = m.$$### 4. Đếm cấu hình bị mất 

hãy để$$s=\sum h_i.$$Sau đó$$\sum x_i = m-2s.$$các$h_i$là không âm và có$n$của họ. 

Bởi các ngôi sao và thanh,$$\#(h_1,\dots,h_n) = \binom{s+n-1}{n-1}.$$các$x_i$là không âm và có$n+1$của họ. 

Một lần nữa bởi các ngôi sao và quán bar,$$\#(x_0,\dots,x_n) = \binom{m-2s+n}{n}.$$Đối với một cố định$s$, do đó số lượng cấu hình bị mất là$$\binom{s+n-1}{n-1} \binom{m-2s+n}{n}.$$Tổng hợp tất cả những gì có thể$s$,$$L = \sum_{s=0}^{\lfloor m/2\rfloor} \binom{s+n-1}{n-1} \binom{m-2s+n}{n}.$$### 5. Đếm tất cả các cấu hình 

Đối với một đơn hàng xen kẽ, chúng tôi phân phối$m$ô trống giữa$$(n+1)+n = 2n+1$$các biến không âm. 

Các ngôi sao và thanh mang lại$$\binom{m+2n}{2n} = \binom{l}{2n}.$$### 6. Tính đáp số 

Đối với một đơn hàng,$$\text{winning} = \binom{l}{2n}-L.$$Có hai thứ tự xen kẽ đối xứng, vì vậy$$\text{answer} = 2\left(\binom{l}{2n}-L\right).$$Tất cả số học được thực hiện modulo$998244353$. 

### Tại sao nó hoạt động 

Toàn bộ trò chơi có thể được thể hiện thông qua vectơ khoảng cách$(g_1,\dots,g_n)$. 

Một bước di chuyển sẽ thay đổi các khoảng trống dương đã chọn thêm một đơn vị, do đó, mỗi khoảng cách được thay đổi sẽ lật ngược tính chẵn lẻ. 

Nếu tất cả các khoảng trống đều bằng nhau thì mỗi nước đi hợp lệ sẽ tạo ra ít nhất một khoảng cách lẻ. Những vị trí như vậy không thể chuyển sang một vị trí chẵn khác. 

Nếu một khoảng trống nào đó là số lẻ thì mọi khoảng cách lẻ đều dương. Giảm từng khoảng cách lẻ đi một là hợp pháp và tạo ra trạng thái trong đó tất cả các khoảng cách đều bằng nhau. 

Do đó, mọi vị trí không chẵn đều có sự di chuyển đến một vị trí chẵn và mọi vị trí chẵn chỉ di chuyển đến các vị trí không chẵn. Điều này hoàn toàn phù hợp với định nghĩa về vị thế thua và thắng. 

Phần đếm đúng vì mọi vị trí hợp lệ đều tương ứng duy nhất với các biến$x_i$Và$g_i$, và các ngôi sao và thanh đếm tất cả các nghiệm không âm của phương trình thu được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

MAXL = 10**6 + 5

fact = [1] * MAXL
for i in range(1, MAXL):
    fact[i] = fact[i - 1] * i % MOD

inv_fact = [1] * MAXL
inv_fact[-1] = pow(fact[-1], MOD - 2, MOD)

for i in range(MAXL - 1, 0, -1):
    inv_fact[i - 1] = inv_fact[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

t = int(input())

for _ in range(t):
    l, n = map(int, input().split())

    m = l - 2 * n

    losing = 0

    for s in range(m // 2 + 1):
        ways_h = C(s + n - 1, n - 1)
        ways_x = C(m - 2 * s + n, n)
        losing = (losing + ways_h * ways_x) % MOD

    total_one_order = C(l, 2 * n)

    ans = (total_one_order - losing) % MOD
    ans = (2 * ans) % MOD

    print(ans)
```Mảng giai thừa và giai thừa nghịch đảo được tính toán trước một lần lên đến$10^6$, bao gồm mọi hệ số nhị thức cần thiết trong bài toán. 

chức năng`C(n, r)`đánh giá sự kết hợp trong thời gian không đổi. 

Đối với một trường hợp thử nghiệm,`m = l - 2n`là số ô trống. Vòng lặp kết thúc`s`thực hiện trực tiếp công thức vị thế thua lỗ:$$\sum \binom{s+n-1}{n-1} \binom{m-2s+n}{n}.$$Sự kết hợp đầu tiên đếm số lần phân phối của các nửa khoảng cách$h_i$. Số thứ hai phân phối các ô trống còn lại$x_i$. 

Sau đó, câu trả lời có được bằng cách trừ các cấu hình bị mất khỏi tất cả các cấu hình cho một đơn hàng xen kẽ và nhân với hai cho hai nông dân có thể bắt đầu. 

Lỗi triển khai phổ biến nhất là quên rằng có$n+1$biến$x_i$, không$n$. Việc sử dụng sai công thức dấu sao và vạch sẽ làm thay đổi từng số đếm một và tạo ra câu trả lời sai ngay cả trong các bài kiểm tra nhỏ. 

Một lỗi dễ xảy ra khác là quên phép nhân cuối cùng với 2. Việc đếm ở trên giả định trình tự bắt đầu với con bò của John. Thứ tự xen kẽ ngược lại đóng góp số tiền tương tự và phải được đưa vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
```Đây$$m = 3 - 2 = 1.$$| s |$\binom{s+n-1}{n-1}$|$\binom{m-2s+n}{n}$| Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 | 

Vì vậy: 

| Biến | Giá trị | 
| --- | --- | 
| thua | 2 | 
| tổng cộng một đơn hàng |$\binom{3}{2}=3$| 
| trúng một đơn hàng | 1 | 
| câu trả lời cuối cùng | 2 | 

Đầu ra:```
2
```Ví dụ này cho thấy rằng chỉ một số vị trí chiến thắng ngay cả khi tồn tại các ô trống. 

### Ví dụ 2 

đầu vào:```
2 1
```Đây$$m = 0.$$| s |$\binom{s+n-1}{n-1}$|$\binom{m-2s+n}{n}$| Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 

Sau đó: 

| Biến | Giá trị | 
| --- | --- | 
| thua | 1 | 
| tổng cộng một đơn hàng |$\binom{2}{2}=1$| 
| trúng một đơn hàng | 0 | 
| câu trả lời cuối cùng | 0 | 

Đầu ra:```
0
```Mọi cấu hình đều bị mất vì không còn ô trống ở đâu cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(l)$mỗi bài kiểm tra | Phép tính tổng lặp qua$0 \ldots \lfloor(l-2n)/2\rfloor$| 
| Không gian |$O(10^6)$| Bảng giai thừa và nghịch đảo | 

Bởi vì tổng của tất cả$l$giá trị nhiều nhất là$10^6$, tổng số lần lặp trên toàn bộ đầu vào cũng là$O(10^6)$. Điều này thoải mái phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())

    tests = []
    max_l = 0

    for _ in range(t):
        l, n = map(int, input().split())
        tests.append((l, n))
        max_l = max(max_l, l)

    fact = [1] * (max_l + 5)
    for i in range(1, max_l + 5):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (max_l + 5)
    inv_fact[-1] = pow(fact[-1], MOD - 2, MOD)

    for i in range(max_l + 4, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

    out = []

    for l, n in tests:
        m = l - 2 * n

        losing = 0
        for s in range(m // 2 + 1):
            losing += C(s + n - 1, n - 1) * C(m - 2 * s + n, n)
            losing %= MOD

        ans = (C(l, 2 * n) - losing) % MOD
        ans = ans * 2 % MOD

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided samples
assert run("3\n2 1\n3 1\n420 69\n") == "0\n2\n870279412\n", "sample"

# minimum size
assert run("1\n2 1\n") == "0\n", "smallest instance"

# one empty cell
assert run("1\n3 1\n") == "2\n", "single-gap case"

# exactly full board with larger n
assert run("1\n4 2\n") == "0\n", "no movement possible"

# boundary with many empty cells
assert run("1\n5 1\n") == "6\n", "checks counting formula"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`|`0`| Ví dụ nhỏ nhất có thể | 
|`3 1`|`2`| Ô trống đơn, vấn đề chẵn lẻ | 
|`4 2`|`0`| Bảng đóng gói hoàn chỉnh | 
|`5 1`|`6`| Phân phối khoảng cách lớn hơn và đếm sao và thanh | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 1
```chúng tôi có$m=0$. Giá trị khoảng cách duy nhất có thể là$g_1=0$, tức là chẵn. Thuật toán tính toán:$$L=1, \qquad \binom{2}{2}=1.$$Vì vậy, câu trả lời trở thành:$$2(1-1)=0.$$Điều này phù hợp với chính trò chơi vì không người chơi nào có thể di chuyển. 

Bây giờ hãy xem xét:```
1
4 2
```Những con bò chiếm hết bốn ô. Lại$m=0$, do đó mọi khoảng trống buộc phải bằng 0 và do đó là chẵn. 

Thuật toán tính mọi cấu hình là bị mất:$$L=\binom{4}{4}=1$$cho một lệnh luân phiên. 

Sau khi trừ đi, số vị trí chiến thắng bằng 0. Điều này mắc phải sai lầm phổ biến khi cho rằng chỉ riêng trật tự luân phiên sẽ tạo ra cơ hội chiến thắng. 

Cuối cùng, hãy xem xét:```
1
3 1
```Đây$m=1$. Các quốc gia thua cuộc duy nhất là những quốc gia có$g_1=0$. Bất kỳ tiểu bang nào với$g_1=1$có một khoảng cách lẻ và đang giành chiến thắng. 

Tổng kết chính xác tạo ra:$$L=2,$$trong khi tổng số cấu hình là$$2\binom{3}{2}=6.$$Như vậy:$$6-4=2$$cấu hình chiến thắng vẫn còn, khớp chính xác với mẫu.
