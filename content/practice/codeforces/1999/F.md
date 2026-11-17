---
title: "CF 1999F - Trung bình dự kiến"
description: "Chúng ta được cung cấp một mảng nhị phân chỉ chứa 0 và 1. Với mỗi dãy con có độ dài k, trong đó k là số lẻ, chúng ta tính trung vị của nó và cộng tất cả các trung vị này lại với nhau. Mảng là nhị phân, làm thay đổi hoàn toàn bản chất của số trung vị."
date: "2026-06-08T14:22:34+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1999
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 964 (Div. 4)"
rating: 1500
weight: 1999
solve_time_s: 128
verified: true
draft: false
---

[CF 1999F - Trung bình dự kiến](https://codeforces.com/problemset/problem/1999/F) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhị phân chỉ chứa`0`Và`1`. Với mọi dãy con có độ dài`k`, Ở đâu`k`là số lẻ, chúng ta tính số trung vị của nó và cộng tất cả các số trung vị này lại với nhau. 

Mảng là nhị phân, làm thay đổi hoàn toàn bản chất của số trung vị. Trung vị của chuỗi nhị phân có độ dài lẻ chỉ có thể là`0`hoặc`1`. Sau khi sắp xếp thứ tự như vậy, số trung vị là`1`chính xác khi hơn một nửa số phần tử của nó được`1`. 

Cho phép

-`z`là số số 0 trong mảng ban đầu. 
-`o`là số đơn vị trong mảng ban đầu. 

Nhiệm vụ là đếm có bao nhiêu chiều dài-`k`dãy sau có số trung vị`1`, bởi vì mọi dãy con như vậy đều đóng góp chính xác`1`tính tổng, trong khi mọi dãy con có số trung vị`0`đóng góp`0`. 

Những hạn chế là lớn. Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2·10^5`. Việc liệt kê tất cả các dãy con là không thể bởi vì ngay cả`C(200000,100000)`có kích thước lớn về mặt thiên văn. Bất kỳ giải pháp nào liên quan đến việc tạo ra các chuỗi con rõ ràng đều bị loại trừ ngay lập tức. 

Giới hạn gợi ý một giải pháp xung quanh`O(n)`hoặc`O(n log MOD)`cho mỗi trường hợp thử nghiệm sau một số tiền xử lý toàn cầu. Vì môđun là số nguyên tố và các kết hợp xuất hiện một cách tự nhiên nên việc tính toán trước các giai thừa và giai thừa nghịch đảo là một gợi ý rõ ràng. 

Một vài trường hợp đáng chú ý. 

Coi như:```
n = 5, k = 1
array = [1,1,1,1,1]
```Bản thân mỗi phần tử đều là một dãy con. Câu trả lời là`5`. Bất kỳ công thức nào giả định ít nhất hai phần tử được chọn từ dãy con sẽ không thành công ở đây. 

Coi như:```
n = 5, k = 5
array = [0,1,0,1,0]
```Chỉ có một dãy con duy nhất là toàn bộ mảng. Nó chứa hai số một và ba số không, nên trung vị của nó là`0`. Câu trả lời là`0`. Việc thực hiện bất cẩn có thể tính toán mọi cách để chọn đa số mà không tôn trọng số lượng sẵn có. 

Coi như:```
n = 4, k = 3
array = [1,0,1,1]
```Mỗi dãy con có độ dài 3 đều có ít nhất hai dãy số 1, nên mọi trung vị đều bằng`1`. Câu trả lời là`4`. Trường hợp này kiểm tra xem ngưỡng cho đa số có được tính toán chính xác hay không`(k+1)/2`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tạo ra mọi độ dài-`k`dãy con, sắp xếp nó, tính trung vị của nó và tích lũy kết quả. 

Điều này đúng vì nó tuân theo định nghĩa chính xác. Thật không may, số lượng các chuỗi con là$$\binom{n}{k}$$vốn đã rất lớn đối với các giá trị vừa phải của`n`. Ngay cả đối với`n = 50`Và`k = 25`, số lượng vượt quá$10^{14}$. Cách tiếp cận vũ phu là hoàn toàn không khả thi. 

Quan sát quan trọng là vị trí của số 0 và số 1 không quan trọng. Chỉ có bao nhiêu số không và số một được chọn là quan trọng. 

Vì mảng là nhị phân nên trung vị của độ dài-`k`dãy sau là`1`khi và chỉ khi dãy con chứa ít nhất$$\frac{k+1}{2}$$những cái đó. 

Cho phép$$m=\frac{k+1}{2}.$$Giả sử một dãy con chứa chính xác`i`những cái đó. Sau đó nó chứa`k-i`số không. Những dãy con như vậy tồn tại trong$$\binom{o}{i}\binom{z}{k-i}$$theo nhiều cách, bởi vì chúng tôi độc lập chọn cái nào và số 0 nào tham gia. 

Mỗi dãy con với`i ≥ m`đóng góp`1`để trả lời. Mỗi dãy con với`i < m`đóng góp`0`. 

Vì vậy, câu trả lời trở thành$$\sum_{i=m}^{k} \binom{o}{i} \binom{z}{k-i}.$$Bây giờ vấn đề được rút gọn thành việc đánh giá một tổng tổ hợp. 

Từ`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2·10^5`, chúng ta có thể tính toán trước các giai thừa và giai thừa nghịch đảo một lần và trả lời mọi truy vấn kết hợp trong`O(1)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O\left(\binom{n}{k}\cdot k\log k\right)$|$O(k)$| Quá chậm | 
| Tối ưu |$O(n + k)$mỗi trường hợp kiểm thử sau khi tiền xử lý |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số đơn vị`o`trong mảng. 
2. Tính số số 0 là`z = n - o`. 
3. Hãy để$$m=\frac{k+1}{2}.$$Bất kỳ dãy con nào có ít nhất`m`những cái có trung bình`1`. 
4. Tính toán trước giai thừa và nghịch đảo giai thừa modulo`10^9+7`lên tới`2·10^5`. 

Điều này cho phép mọi giá trị kết hợp được đánh giá trong thời gian không đổi. 
5. Lặp lại`i`từ`m`ĐẾN`k`. 
6. Đối với mỗi`i`, giải thích nó là số lượng người được chọn. 
7. Số dãy con hợp lệ chính xác`i`những cái bằng$$\binom{o}{i}\binom{z}{k-i}.$$Nếu một trong hai sự kết hợp là không thể, hãy coi nó là số không. 
8. Thêm số lượng này vào câu trả lời modulo`10^9+7`. 
9. Xuất số tiền cuối cùng. 

### Tại sao nó hoạt động 

Một dãy con nhị phân có trung vị`1`chính xác khi hơn một nửa số phần tử của nó được`1`. Từ`k`là số lẻ, điều kiện này tương đương với việc có ít nhất`(k+1)/2`những cái đó. 

Mỗi dãy con hợp lệ có thể được phân loại duy nhất theo số chính xác`i`của những cái nó chứa. Đối với một cố định`i`, việc chọn dãy con tương đương với việc chọn`i`các phần tử từ`o`những cái có sẵn và`k-i`các phần tử từ`z`số không có sẵn. Điều này mang lại chính xác$$\binom{o}{i}\binom{z}{k-i}$$các chuỗi tiếp theo. 

Các loại cho các giá trị khác nhau của`i`rời rạc và bao gồm tất cả các chuỗi con có trung vị là`1`. Tổng hợp tất cả`i ≥ (k+1)/2`đếm mọi dãy con đóng góp chính xác một lần, đó chính xác là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAXN = 200000

fact = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

inv_fact = [1] * (MAXN + 1)
inv_fact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)

for i in range(MAXN, 0, -1):
    inv_fact[i - 1] = inv_fact[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

t = int(input())

for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    ones = sum(a)
    zeros = n - ones

    need = (k + 1) // 2

    ans = 0

    for i in range(need, k + 1):
        ans = (ans + C(ones, i) * C(zeros, k - i)) % MOD

    print(ans)
```Quá trình tiền xử lý tính toán giai thừa và giai thừa nghịch đảo một lần. Vì môđun là số nguyên tố nên định lý nhỏ Fermat cho$$x^{-1}=x^{MOD-2}\pmod{MOD}.$$Chức năng trợ giúp`C(n,r)`trả về 0 bất cứ khi nào sự kết hợp là không thể. Điều này rất quan trọng vì các giá trị như`C(3,5)`không được đóng góp gì vào số tiền đó. 

Đối với mỗi trường hợp thử nghiệm, chỉ có số lượng số 0 và số 1 là quan trọng. Sự sắp xếp thực tế của các phần tử không bao giờ xuất hiện trong công thức. 

Vòng lặp kết thúc`i`bắt đầu lúc`(k+1)//2`bởi vì đây là số nhỏ nhất có thể làm cho số trung vị bằng`1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
k = 3
array = [1,0,0,1]
```Chúng tôi có: 

-`ones = 2`-`zeros = 2`-`need = 2`| tôi | C(những cái,i) | C(số không,k-i) | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | 1 | 2 | 2 | 
| 3 | 0 | 1 | 0 | 

Trả lời =`2`. 

Điều này phù hợp với mẫu. Có đúng hai dãy con có độ dài 3 có trung vị bằng`1`. 

### Ví dụ 2 

đầu vào:```
n = 6
k = 3
array = [1,0,1,0,1,1]
```Chúng tôi có: 

-`ones = 4`-`zeros = 2`-`need = 2`| tôi | C(4,i) | C(2,3-i) | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | 6 | 2 | 12 | 
| 3 | 4 | 1 | 4 | 
| 4 | 1 | 0 | 0 | 

Tổng số câu trả lời:$$12 + 4 = 16.$$Điều này bằng với đầu ra mẫu. 

Ví dụ này thể hiện ý tưởng trung tâm: đếm các chuỗi con theo số lượng chuỗi con chứa thay vì liệt kê chúng riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(200000) + O(k)$mỗi trường hợp thử nghiệm | Tiền xử lý một lần, sau đó là một vòng lặp tổng | 
| Không gian |$O(200000)$| Mảng giai thừa và nghịch đảo | 

Quá trình tiền xử lý toàn cầu tốn khoảng hai trăm nghìn thao tác. Trên tất cả các trường hợp thử nghiệm, tổng kích thước đầu vào tối đa là`2·10^5`, do đó lời giải vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 7
MAXN = 200000

fact = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

inv_fact = [1] * (MAXN + 1)
inv_fact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)

for i in range(MAXN, 0, -1):
    inv_fact[i - 1] = inv_fact[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        ones = sum(a)
        zeros = n - ones
        need = (k + 1) // 2

        ans = 0
        for i in range(need, k + 1):
            ans = (ans + C(ones, i) * C(zeros, k - i)) % MOD

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run(
"""8
4 3
1 0 0 1
5 1
1 1 1 1 1
5 5
0 1 0 1 0
6 3
1 0 1 0 1 1
4 3
1 0 1 1
5 3
1 0 1 1 0
2 1
0 0
34 17
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
"""
) == """2
5
0
16
4
7
0
333606206"""

# minimum size
assert run(
"""1
1 1
0
"""
) == "0"

# single one
assert run(
"""1
1 1
1
"""
) == "1"

# all ones
assert run(
"""1
5 3
1 1 1 1 1
"""
) == "10"

# all zeros
assert run(
"""1
5 3
0 0 0 0 0
"""
) == "0"

# boundary majority
assert run(
"""1
3 3
1 1 0
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`0`| Ví dụ nhỏ nhất có thể | 
|`1 1 / 1`|`1`| Chuỗi con dài 1 | 
| Năm cái,`k=3`|`10`| Mỗi chuỗi con đều đóng góp | 
| Năm số không,`k=3`|`0`| Không có phần tiếp theo nào đóng góp | 
|`[1,1,0]`,`k=3`|`1`| Ngưỡng đa số bằng`(k+1)/2`| 

## Vỏ cạnh 

Hãy xem xét:```
1
5 1
1 1 1 1 1
```Đây`need = 1`. Tổng hợp chỉ chứa`i = 1`:$$\binom{5}{1}\binom{0}{0}=5.$$Thuật toán trả về`5`, phù hợp với thực tế là mọi phần tử đều tạo thành một dãy con hợp lệ. 

Coi như:```
1
5 5
0 1 0 1 0
```Chúng tôi có`ones = 2`,`zeros = 3`, Và`need = 3`. 

Mỗi thuật ngữ yêu cầu chọn ít nhất ba thuật ngữ, điều này là không thể:$$\binom{2}{3}=0.$$Mọi đóng góp đều biến mất, tạo ra câu trả lời`0`. Thuật toán xử lý việc này một cách tự nhiên thông qua hàm kết hợp. 

Coi như:```
1
4 3
1 0 1 1
```Chúng tôi có`ones = 3`,`zeros = 1`, Và`need = 2`. 

Việc tính toán là$$\binom{3}{2}\binom{1}{1} + \binom{3}{3}\binom{1}{0} = 3+1 = 4.$$Tất cả bốn dãy con có độ dài 3 đều đóng góp, vì vậy câu trả lời là`4`. Điều này xác nhận rằng ngưỡng đa số được triển khai chính xác và mọi thành phần hợp lệ đều được tính chính xác một lần.
