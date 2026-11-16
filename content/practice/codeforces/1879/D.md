---
title: "CF 1879D - Tổng các hàm XOR"
description: "Chúng ta được cho một mảng các số nguyên không âm. Với mỗi mảng con $[l,r]$, chúng tôi tính toán XOR của tất cả các phần tử bên trong mảng con đó. Đặt giá trị XOR đó là $f(l,r)$. Sự đóng góp của một mảng con không chỉ là XOR của nó."
date: "2026-06-08T22:47:06+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "divide-and-conquer", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1879
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 155 (Rated for Div. 2)"
rating: 1700
weight: 1879
solve_time_s: 143
verified: true
draft: false
---

[CF 1879D - Tổng các hàm XOR](https://codeforces.com/problemset/problem/1879/D) 

**Đánh giá:** 1700 
**Tags:** bitmasks, tổ hợp, chia để trị, dp, toán học 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Đối với mỗi mảng con$[l,r]$, chúng tôi tính toán XOR của tất cả các phần tử bên trong mảng con đó. Đặt giá trị XOR đó là$f(l,r)$. 

Sự đóng góp của một mảng con không chỉ là XOR của nó. Nó được nhân với độ dài mảng con:$$f(l,r)\cdot(r-l+1)$$Nhiệm vụ là tính tổng giá trị này trên tất cả các mảng con và đưa ra kết quả theo modulo$998244353$. 

Việc triển khai trực tiếp sẽ kiểm tra tất cả$O(n^2)$mảng con. Với$n\le 3\cdot10^5$, điều đó có nghĩa là đại khái$4.5\times10^{10}$mảng con, điều này hoàn toàn không thể thực hiện được trong vòng hai giây. Thậm chí một$O(n^2)$thuật toán chỉ thực hiện một vài thao tác trên mỗi mảng con đã vượt quá giới hạn. 

Các giá trị mảng nhiều nhất là$10^9$, vì vậy mọi số đều vừa với 30 bit. Bất cứ khi nào một vấn đề yêu cầu tổng các giá trị XOR, việc xử lý từng bit một cách độc lập thường có lợi vì XOR hoạt động độc lập trên mọi vị trí bit. 

Một trường hợp tinh tế là hệ số nhân chiều dài phụ thuộc vào cả hai điểm cuối. Ví dụ:```
2
1 1
```Mảng con:```
[1] -> 1 * 1 = 1
[1,1] -> 0 * 2 = 0
[1] -> 1 * 1 = 1
```Trả lời = 2. 

Một giải pháp chỉ đếm có bao nhiêu mảng con có bit XOR$1$sẽ bỏ lỡ trọng số chiều dài và tạo ra kết quả sai. 

Một sai lầm dễ mắc phải khác là quên rằng mảng con XOR được biểu diễn thông qua tiền tố XOR:```
1
5
```Mảng con duy nhất có XOR$5$, chiều dài$1$, trả lời$5$. 

Trường hợp này yêu cầu tiền tố XOR trước khi mảng bắt đầu, cụ thể là$pref_0=0$. Việc bỏ qua tiền tố này sẽ khiến tất cả các mảng con bắt đầu ở vị trí$1$biến mất khỏi cuộc đếm. 

Nguồn lỗi thứ ba là số học mô-đun. Tổng trọng số có thể trở nên cực kỳ lớn. Mặc dù số nguyên Python không bị tràn nhưng bài toán yêu cầu kết quả cuối cùng theo modulo$998244353$và các tích lũy trung gian cũng phải được giảm theo modulo giá trị này. 

## Phương pháp tiếp cận 

Giải pháp brute-force liệt kê từng cặp$(l,r)$. Sử dụng tiền tố XOR, mỗi mảng con XOR có thể được tính theo$O(1)$:$$f(l,r)=pref_r\oplus pref_{l-1}$$Sau đó chúng tôi thêm$$(pref_r\oplus pref_{l-1})(r-l+1)$$để trả lời. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Vấn đề là số lượng mảng con. có$$\frac{n(n+1)}2$$trong số đó đạt khoảng$4.5\times10^{10}$khi$n=3\cdot10^5$. 

Quan sát quan trọng là XOR có thể được phân tách từng chút một. 

Xem xét vị trí một bit$b$. Đóng góp của nó cho câu trả lời cuối cùng là$$2^b \times \sum_{\text{subarrays whose XOR has bit }b=1} (r-l+1)$$Vì vậy, nhiệm vụ thực sự trở thành: 

Với mỗi bit, hãy tìm tổng độ dài của tất cả các mảng con có XOR trên bit đó bằng$1$. 

Giới thiệu tiền tố XOR chẵn lẻ cho bit này. Cho phép$$p_i = \text{bit } b \text{ of } pref_i.$$Đối với một mảng con$[l,r]$,$$\text{bit } b \text{ of } f(l,r) = p_r \oplus p_{l-1}.$$Bit đóng góp chính xác khi hai giá trị chẵn lẻ này khác nhau. 

Bây giờ viết lại mảng con sử dụng các chỉ số tiền tố:$$j=l-1,\qquad i=r.$$Độ dài mảng con trở thành$$r-l+1=i-j.$$Vì vậy, đối với một bit cố định, chúng ta cần$$\sum_{0\le j<i\le n,\ p_i\ne p_j} (i-j).$$Đây là vấn đề đếm cặp trên các chẵn lẻ XOR tiền tố. 

Giả sử chúng ta xử lý các vị trí tiền tố từ trái sang phải. Khi chúng tôi đến vị trí$i$, chúng tôi muốn tất cả các vị trí trước đó$j$có tính chẵn lẻ khác với$p_i$. 

Đóng góp của họ là$$\sum (i-j) = (\text{count})\cdot i - \sum j.$$Vì vậy với mỗi chẵn lẻ$0$Và$1$, chúng ta chỉ cần: 

- có bao nhiêu tiền tố trước đó có tính chẵn lẻ đó, 
- tổng các chỉ số của chúng. 

Sau đó mọi vị trí tiền tố có thể được xử lý trong$O(1)$, cho$O(n)$làm việc mỗi bit. 

Vì chỉ có 30 bit liên quan nên tổng độ phức tạp trở thành$O(30n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|------|---| 

| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 

| Tối ưu |$O(30n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xây dựng XOR tiền tố 

Tạo$$pref_0=0, \qquad pref_i=pref_{i-1}\oplus a_i.$$Khi đó mọi mảng con XOR đều bằng$$pref_r\oplus pref_{l-1}.$$### 2. Xử lý từng bit một cách độc lập 

Đối với mỗi bit$b$từ$0$ĐẾN$29$, tính tổng chiều dài có trọng số của các mảng con có XOR có bit$b=1$. 

Đáp án cuối cùng nhận số lượng này nhân với$2^b$. 

### 3. Duy trì số liệu thống kê về các chẵn lẻ tiền tố trước đó 

Đối với bit hiện tại, xác định$$parity_i = (pref_i>>b)\&1.$$Duy trì:```
cnt[0], cnt[1]
sumPos[0], sumPos[1]
```Ở đâu`cnt[t]`là số vị trí tiền tố trước đó có tính chẵn lẻ`t`, Và`sumPos[t]`là tổng các chỉ số của chúng. 

Ban đầu chỉ có vị trí tiền tố$0$tồn tại:```
cnt[0] = 1
sumPos[0] = 0
```### 4. Xử lý tiền tố từ trái sang phải 

Tại vị trí$i$, coi tính chẵn lẻ hiện tại là$p$. 

Chúng tôi cần tất cả các vị trí trước đó có tính chẵn lẻ$p\oplus1$. 

Nếu như```
opp = p ^ 1
```thì tổng đóng góp của họ là$$cnt[opp]\cdot i - sumPos[opp].$$Đại lượng này bằng$$\sum(i-j)$$trên tất cả các vị trí hợp lệ trước đó$j$. 

Thêm nó vào phần đóng góp đang chạy của bit này. 

### 5. Chèn tiền tố hiện tại 

Sau khi sử dụng vị trí$i$làm điểm cuối bên phải, thêm nó vào cấu trúc dữ liệu:$$cnt[p] += 1$$

$$sumPos[p] += i$$### 6. Thêm phần đóng góp bit 

Nếu chiều dài có trọng số tích lũy cho bit$b$là$cur$, thêm vào$$cur\cdot 2^b$$để trả lời modulo$998244353$. 

### Tại sao nó hoạt động 

Đối với một bit cố định, một mảng con đóng góp chính xác khi các tiền tố chẵn lẻ tương ứng ở hai đầu của nó khác nhau. Mỗi mảng con$[l,r]$tương ứng duy nhất với một cặp chỉ số tiền tố$(l-1,r)$. Trong khi xử lý vị trí tiền tố$r$, thuật toán xem xét tất cả các vị trí tiền tố trước đó có tính chẵn lẻ ngược lại. biểu thức$$cnt\cdot r-\sum j$$chính xác là tổng độ dài$r-j$của tất cả các mảng con như vậy. Mỗi mảng con hợp lệ được tính một lần khi điểm cuối bên phải của nó được xử lý và các mảng con không hợp lệ không bao giờ được tính vì số chẵn lẻ của chúng bằng nhau. Tính tổng độc lập trên tất cả các bit sẽ tái tạo lại các giá trị XOR ban đầu, do đó câu trả lời cuối cùng bằng tổng có trọng số được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] ^ a[i]

    ans = 0

    for b in range(30):
        cnt = [1, 0]          # prefix 0 already exists
        sum_pos = [0, 0]

        cur = 0

        for i in range(1, n + 1):
            p = (pref[i] >> b) & 1
            opp = p ^ 1

            cur += cnt[opp] * i - sum_pos[opp]

            cnt[p] += 1
            sum_pos[p] += i

        ans = (ans + (cur % MOD) * ((1 << b) % MOD)) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng XOR tiền tố biến đổi mọi mảng con XOR thành XOR của hai tiền tố. Sau đó, mỗi bit có thể được xử lý độc lập. 

Đối với một bit, thông tin duy nhất quan trọng là liệu tiền tố XOR tại bit đó có phải là$0$hoặc$1$. Các mảng`cnt`Và`sum_pos`lưu trữ chính xác số liệu thống kê cần thiết để đánh giá$$\sum(i-j)$$cho tất cả các tiền tố trước đó có tính chẵn lẻ ngược lại. 

Thứ tự của các hoạt động quan trọng. Vị trí tiền tố hiện tại phải đóng góp trước khi được chèn vào cấu trúc dữ liệu. Nếu không, tiền tố sẽ ghép nối không chính xác với chính nó, tạo ra các mảng con có độ dài bằng 0. 

giá trị```
cnt[opp] * i - sum_pos[opp]
```đã bằng tổng độ dài của tất cả các mảng con hợp lệ mới được phát hiện kết thúc ở vị trí`i`. Không cần tính toán độ dài bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3 2
```Tiền tố XOR:```
pref = [0, 1, 2, 0]
```Hãy xem xét bit 0. 

| tôi | trước[i] | chẵn lẻ | đếm ngược lại | tổng ngược lại | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 0 | 1 | 
| 2 | 2 | 0 | 1 | 1 | 1 | 
| 3 | 0 | 0 | 1 | 1 | 2 | 

Tổng chiều dài có trọng số bit-0 = 4. 

Hãy xem xét bit 1. 

| tôi | trước[i] | chẵn lẻ | đếm ngược lại | tổng ngược lại | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 0 | 
| 2 | 2 | 1 | 2 | 1 | 3 | 
| 3 | 0 | 0 | 1 | 2 | 1 | 

Tổng chiều dài có trọng số bit-1 = 4. 

Đóng góp:```
4 * 1 + 4 * 2 = 12
```Trả lời:```
12
```Dấu vết này cho thấy độ dài được tích lũy tự động thông qua công thức$cnt\cdot i-\sum j$. 

### Ví dụ 2 

đầu vào:```
2
1 1
```Tiền tố XOR:```
[0,1,0]
```Xử lý bit 0: 

| tôi | chẵn lẻ | đếm ngược lại | tổng ngược lại | đã thêm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 
| 2 | 0 | 1 | 1 | 1 | 

Tổng chiều dài có trọng số:```
2
```Đóng góp bit:```
2 * 1 = 2
```Trả lời:```
2
```Ví dụ này chứng minh rằng mảng con`[1,1]`có XOR bằng 0 và không đóng góp gì mặc dù có độ dài bằng hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(30n)$| Mỗi bit trong số 30 bit quét tất cả các vị trí tiền tố một lần | 
| Không gian |$O(n)$| Mảng tiền tố XOR có kích thước$n+1$| 

Với$n=3\cdot10^5$, thuật toán thực hiện khoảng chín triệu lần lặp, dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ bị chi phối bởi mảng XOR tiền tố và vẫn ở dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    MOD = 998244353

    n = int(input())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] ^ a[i]

    ans = 0

    for b in range(30):
        cnt = [1, 0]
        sum_pos = [0, 0]
        cur = 0

        for i in range(1, n + 1):
            p = (pref[i] >> b) & 1
            opp = p ^ 1

            cur += cnt[opp] * i - sum_pos[opp]

            cnt[p] += 1
            sum_pos[p] += i

        ans = (ans + (cur % MOD) * ((1 << b) % MOD)) % MOD

    return str(ans) + "\n"

# provided sample
assert run("3\n1 3 2\n") == "12\n", "sample 1"

# minimum size
assert run("1\n5\n") == "5\n", "single element"

# all zeros
assert run("3\n0 0 0\n") == "0\n", "all xor values are zero"

# equal values
assert run("2\n1 1\n") == "2\n", "repeated numbers"

# off-by-one check involving prefix 0
assert run("2\n1 0\n") == "4\n", "subarrays starting at index 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`5`| Đầu vào hợp lệ nhỏ nhất | 
|`3 / 0 0 0`|`0`| Tất cả giá trị XOR đều biến mất | 
|`2 / 1 1`|`2`| Các giá trị bằng nhau tạo XOR bằng 0 trên mảng con dài hơn | 
|`2 / 1 0`|`4`| Xử lý đúng vị trí tiền tố 0 | 

## Vỏ cạnh 

Hãy xem xét:```
1
5
```Mảng XOR tiền tố là:```
[0,5]
```Đối với mỗi bit được đặt là 5, vị trí 1 có tính chẵn lẻ ngược lại với vị trí 0, tạo ra độ dài 1. Câu trả lời chính xác là 5. Điều này xác nhận rằng việc bao gồm tiền tố ban đầu XOR là cần thiết. 

Coi như:```
2
1 1
```Các mảng con duy nhất có XOR bằng 1 là các mảng con có hai phần tử đơn. Mảng con dài hơn có XOR 0. Trong quá trình xử lý, cặp tiền tố tương ứng với toàn bộ mảng có tính chẵn lẻ bằng nhau và không bao giờ được tính. Thuật toán xuất ra chính xác 2. 

Hãy xem xét:```
3
0 0 0
```Tất cả các XOR tiền tố đều bằng 0. Mỗi bit chẵn lẻ luôn bằng 0. Vì các tiền tố chẵn lẻ đối diện không bao giờ tồn tại nên mọi đóng góp đều bằng 0. Câu trả lời vẫn là 0, phù hợp với thực tế là mọi mảng con XOR đều bằng 0. 

Coi như:```
2
1 0
```Các mảng con là:```
[1]   -> 1 * 1 = 1
[1,0] -> 1 * 2 = 2
[0]   -> 0 * 1 = 0
```Tổng cộng:```
3
```Thuật toán đếm cả hai mảng con bắt đầu từ chỉ số 1 vì vị trí tiền tố ban đầu 0 tham gia vào quá trình so khớp chẵn lẻ. Đây chính xác là tình huống phá vỡ việc triển khai mà quên tiền tố trống.
