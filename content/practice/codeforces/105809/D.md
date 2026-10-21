---
title: "CF 105809D - Sắp xếp mã thông báo riêng biệt"
description: "Chúng tôi có hai loại mã thông báo. Loại thứ nhất gồm các nhãn 1, 2, ..., m. Mỗi mã thông báo này là duy nhất, vì vậy nhãn có thể xuất hiện nhiều nhất một lần trong một sắp xếp duy nhất. Loại thứ hai bao gồm n bản sao của nhãn 0."
date: "2026-06-25T15:27:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105809
codeforces_index: "D"
codeforces_contest_name: "Code Rush 2025"
rating: 0
weight: 105809
solve_time_s: 47
verified: true
draft: false
---

[CF 105809D - Sắp xếp mã thông báo riêng biệt](https://codeforces.com/problemset/problem/105809/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai loại mã thông báo. 

Loại đầu tiên bao gồm các nhãn`1, 2, ..., m`. Mỗi mã thông báo này là duy nhất, vì vậy nhãn có thể xuất hiện nhiều nhất một lần trong một sắp xếp duy nhất. 

Loại thứ hai gồm`n`bản sao của nhãn`0`. Những bản sao này không thể phân biệt được nên sự sắp xếp chỉ quan tâm đến giá trị ở đâu`0`xuất hiện, không phải mã thông báo vật lý nào đã được sử dụng. 

Một sự sắp xếp là một trình tự có thứ tự có độ dài`L`có thể là bất kỳ giá trị nào từ`1`ĐẾN`n`. 

Với mỗi độ dài hợp lệ, chúng ta phải đếm xem có bao nhiêu chuỗi khác nhau có thể được hình thành theo quy tắc mỗi nhãn khác 0 được sử dụng nhiều nhất một lần. Câu trả lời là bắt buộc theo modulo`10^9 + 7`. 

Các ràng buộc đủ nhỏ để gợi ý một giải pháp đếm tổ hợp. Cả hai`m`Và`n`nhiều nhất là`1000`, vì vậy một`O(nm)`thuật toán thực hiện khoảng một triệu lần lặp, đủ nhanh. Bất kỳ sự sắp xếp nào bằng vũ lực đều không thể thực hiện được vì số lượng các chuỗi hợp lệ tăng theo cấp số nhân. 

Một số trường hợp khó xử lý. 

Giả định`m = 1`Và`n = 1`.```
Input
1 1
```Các sắp xếp hợp lệ là`[0]`Và`[1]`, vậy câu trả lời là`2`. 

Một giải pháp chỉ tính các sắp xếp chứa mã thông báo khác 0 sẽ trả về không chính xác`1`. 

Coi như:```
Input
2 2
```Chiều dài`2`chứa các sắp xếp như`[0,0]`,`[0,1]`,`[1,0]`,`[1,2]`, Và`[2,1]`. Sự sắp xếp`[1,1]`là bất hợp pháp vì mã thông báo`1`là duy nhất và không thể tái sử dụng được. Công thức đếm coi các nhãn khác 0 là có thể tái sử dụng sẽ bị tính quá mức. 

Một trường hợp quan trọng khác là khi độ dài mong muốn vượt quá số lượng nhãn duy nhất.```
m = 2, L = 5
```Nhiều nhất hai nhãn khác 0 có thể xuất hiện. Bất kỳ công thức nào cho phép`k > m`các mã thông báo khác 0 sẽ tính các sắp xếp không thể thực hiện được. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là tạo ra mọi cách sắp xếp có độ dài cho phép và chèn nó vào một tập hợp. Điều này đúng vì mọi chuỗi hợp lệ đều được xây dựng và tính một cách rõ ràng một lần. 

Vấn đề là ngay cả đối với các giá trị vừa phải, số lượng cách sắp xếp vẫn trở nên rất lớn. Không gian tìm kiếm tăng theo cấp số nhân theo chiều dài, khiến cho việc sử dụng vũ lực hoàn toàn không thể thực hiện được. 

Thay vì tạo ra các chuỗi, chúng ta có thể đếm chúng. 

Cố định độ dài`L`. Cho phép`k`là số lượng mã thông báo khác 0 được sử dụng trong sắp xếp. 

Kể từ khi các nhãn`1..m`chính xác là duy nhất`k`các nhãn khác nhau phải được chọn và mỗi nhãn được chọn sẽ xuất hiện một lần. 

Đầu tiên hãy chọn cái nào`k`vị trí trong số`L`các vị trí sẽ chứa các nhãn khác 0. Điều này góp phần$$\binom{L}{k}$$cách. 

Bây giờ hãy điền vào những cái đó`k`các vị trí có nhãn riêng biệt từ`1..m`. 

Chúng ta cần một sự lựa chọn có thứ tự`k`nhãn riêng biệt, đó là$$P(m,k)=m(m-1)\cdots(m-k+1)$$cách. 

Nhân các lựa chọn độc lập mang lại$$\binom{L}{k} P(m,k).$$Tính tổng mọi giá trị hợp lệ của`k`đếm tất cả các cách sắp xếp độ dài`L`.$$\text{count}(L)
=
\sum_{k=0}^{\min(L,m)}
\binom{L}{k} P(m,k).$$Cuối cùng, tổng hợp tất cả các độ dài cho phép:$$\text{answer}
=
\sum_{L=1}^{n}
\sum_{k=0}^{\min(L,m)}
\binom{L}{k} P(m,k).$$Từ`m,n ≤ 1000`, chúng ta có thể đánh giá số tiền gấp đôi này trong`O(nm)`thời gian sử dụng giai thừa được tính toán trước và giai thừa nghịch đảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(nm) | O(m+n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`m`Và`n`. 
2. Tính toán trước các giai thừa lên đến`max(m,n)`modulo`10^9 + 7`. 
3. Tính trước các giai thừa nghịch đảo bằng định lý Fermat. 
4. Đối với mọi chiều dài`L`từ`1`ĐẾN`n`, lặp lại`k`từ`0`ĐẾN`min(L,m)`. 
5. Tính toán$$\binom{L}{k}
=
\frac{L!}{k!(L-k)!}$$sử dụng giai thừa và giai thừa nghịch đảo. 
6. Tính toán$$P(m,k)
=
\frac{m!}{(m-k)!}.$$7. Thêm$$\binom{L}{k}P(m,k)$$để trả lời modulo`10^9 + 7`. 
8. In số tiền cuối cùng. 

### Tại sao nó hoạt động 

Mỗi cách sắp xếp độ dài`L`chứa một số số`k`của các nhãn khác 0. 

Một lần`k`là cố định, sự sắp xếp được xác định duy nhất bởi hai quyết định: vị trí nào chứa các nhãn khác 0 và tập hợp các nhãn riêng biệt có thứ tự chiếm các vị trí đó. 

yếu tố`C(L,k)`đếm sự lựa chọn vị trí. yếu tố`P(m,k)`đếm sự phân công theo thứ tự của các nhãn riêng biệt. Mọi cách sắp xếp hợp lệ đều tương ứng với chính xác một cặp lựa chọn như vậy và mỗi cặp như vậy tạo ra một cách sắp xếp hợp lệ. 

Tổng hợp tất cả hợp lệ`k`giá trị tính mọi sự sắp xếp chính xác một lần. Tính tổng tất cả các độ dài sẽ tính mọi cách sắp xếp hợp lệ trong bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

m, n = map(int, input().split())

mx = max(m, n)

fact = [1] * (mx + 1)
for i in range(1, mx + 1):
    fact[i] = fact[i - 1] * i % MOD

inv_fact = [1] * (mx + 1)
inv_fact[mx] = pow(fact[mx], MOD - 2, MOD)

for i in range(mx, 0, -1):
    inv_fact[i - 1] = inv_fact[i] * i % MOD

def comb(a, b):
    if b < 0 or b > a:
        return 0
    return fact[a] * inv_fact[b] % MOD * inv_fact[a - b] % MOD

ans = 0
fact_m = fact[m]

for L in range(1, n + 1):
    limit = min(L, m)
    for k in range(limit + 1):
        perm = fact_m * inv_fact[m - k] % MOD
        ans = (ans + comb(L, k) * perm) % MOD

print(ans)
```Mảng giai thừa và nghịch đảo giai thừa cho phép mọi giá trị kết hợp được tính toán trong thời gian không đổi. 

Số hoán vị$$P(m,k)=\frac{m!}{(m-k)!}$$cũng thu được trong thời gian không đổi từ cùng một bảng. 

Các vòng lặp lồng nhau lặp qua mọi cặp hợp lệ`(L,k)`. Từ`L`không bao giờ vượt quá`1000`, tổng số lần lặp là khoảng một triệu, nằm trong giới hạn thoải mái. 

Một sai lầm thường gặp là quên vụ án`k = 0`. Thuật ngữ đó thể hiện sự sắp xếp bao gồm toàn bộ số không, chẳng hạn như`[0]`hoặc`[0,0,0]`. Những sự sắp xếp đó là hợp lệ và phải được tính. 

Một lỗi phổ biến khác là cho phép`k > m`. chỉ có`m`có sẵn các nhãn khác 0 nên những trạng thái như vậy là không thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
```Đối với mỗi chiều dài: 

| L | k | C(L,k) | P(2,k) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 1 | 
| 1 | 1 | 1 | 2 | 2 | 
| 2 | 0 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 2 | 4 | 
| 2 | 2 | 1 | 2 | 2 | 

Tổng cộng:$$1+2+1+4+2=10$$Đầu ra:```
10
```Ví dụ này cho thấy cách sắp xếp chỉ có số 0 được đưa vào một cách tự nhiên thông qua`k=0`thuật ngữ. 

### Ví dụ 2 

đầu vào:```
1 3
```| L | k | C(L,k) | P(1,k) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 1 | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 0 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 1 | 2 | 
| 3 | 0 | 1 | 1 | 1 | 
| 3 | 1 | 3 | 1 | 3 | 

Tổng cộng:$$1+1+1+2+1+3=9$$Đầu ra:```
9
```Dấu vết này cho thấy tình trạng`m`nhỏ hơn độ dài chuỗi. Chúng tôi không bao giờ cho phép`k > 1`bởi vì chỉ tồn tại một mã thông báo khác 0 duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Tổng gấp đôi theo độ dài và số lượng mã thông báo khác 0 có thể có | 
| Không gian | O(max(n,m)) | Bảng giai thừa và nghịch đảo | 

Với`m,n ≤ 1000`, thuật toán thực hiện khoảng một triệu lần lặp và chỉ lưu trữ một vài mảng có độ dài khoảng một nghìn, dễ dàng phù hợp với giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    m, n = map(int, input().split())

    mx = max(m, n)

    fact = [1] * (mx + 1)
    for i in range(1, mx + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (mx + 1)
    inv_fact[mx] = pow(fact[mx], MOD - 2, MOD)

    for i in range(mx, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    ans = 0

    for L in range(1, n + 1):
        for k in range(min(L, m) + 1):
            c = fact[L] * inv_fact[k] % MOD * inv_fact[L - k] % MOD
            p = fact[m] * inv_fact[m - k] % MOD
            ans = (ans + c * p) % MOD

    return str(ans) + "\n"

# provided sample
assert run("2 2\n") == "10\n", "sample 1"

# custom cases
assert run("1 1\n") == "2\n", "minimum size"
assert run("1 2\n") == "5\n", "single unique token"
assert run("2 1\n") == "3\n", "length one"
assert run("3 2\n") == "17\n", "all transition cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`2`| Trường hợp hợp lệ nhỏ nhất | 
|`1 2`|`5`| Xử lý đúng các số 0 lặp đi lặp lại | 
|`2 1`|`3`| Chỉ có độ dài 1 tồn tại | 
|`3 2`|`17`| Nhiều giá trị của`k`đóng góp | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
```Vì`L = 1`, thuật toán đánh giá`k = 0`Và`k = 1`.`k = 0`góp phần sắp xếp`[0]`.`k = 1`góp phần sắp xếp`[1]`. 

Câu trả lời trở thành`2`, điều đó đúng. Bất kỳ quá trình triển khai nào bắt đầu`k`từ`1`sẽ nhớ`[0]`. 

Coi như:```
2 2
```Vì`L = 2`, thuật toán chỉ cho phép`k = 0, 1, 2`. 

Khi`k = 2`, phần đóng góp là$$C(2,2)\cdot P(2,2)=1\cdot2.$$Đây chính xác là`[1,2]`Và`[2,1]`. 

Trình tự không hợp lệ`[1,1]`Và`[2,2]`không bao giờ xuất hiện vì`P(2,2)`đếm các lựa chọn có thứ tự của các nhãn riêng biệt. 

Coi như:```
2 5
```Mặc dù chiều dài là năm, vòng lặp hạn chế$$k \le \min(L,m)=2.$$Thuật toán không bao giờ cố gắng sử dụng ba nhãn khác 0 riêng biệt vì chỉ tồn tại hai nhãn. Điều này thực thi chính xác ràng buộc về tính duy nhất.
