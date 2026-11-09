---
title: "CF 1470B - Định nghĩa lạ"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Xác định hai giá trị liền kề khi $$frac{operatorname{lcm}(x,y)}{gcd(x,y)}$$ là một hình vuông hoàn hảo. Tại mỗi giây, mỗi phần tử mảng được thay thế bằng tích của tất cả các phần tử hiện tại liền kề."
date: "2026-06-11T01:00:24+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "graphs", "hashing", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1470
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 694 (Div. 1)"
rating: 1900
weight: 1470
solve_time_s: 187
verified: true
draft: false
---

[CF 1470B - Định nghĩa kỳ lạ](https://codeforces.com/problemset/problem/1470/B) 

**Xếp hạng:** 1900 
**Tags:** bitmasks, đồ thị, hàm băm, toán học, lý thuyết số 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Xác định hai giá trị liền kề khi$$\frac{\operatorname{lcm}(x,y)}{\gcd(x,y)}$$là một hình vuông hoàn hảo 

Tại mỗi giây, mỗi phần tử mảng được thay thế bằng tích của tất cả các phần tử hiện tại liền kề. Số lượng chúng ta quan tâm là vẻ đẹp của mảng, đây là số lượng kề lớn nhất trong số tất cả các vị trí. 

Đối với mỗi giá trị truy vấn$w$, chúng ta phải báo cáo vẻ đẹp sau chính xác$w$giây. 

Thử thách đầu tiên là hiểu được sự liền kề. Thách thức thứ hai là hiểu các phép biến đổi lặp đi lặp lại ảnh hưởng như thế nào đến mảng. Các truy vấn yêu cầu thời gian lớn tùy ý, lên tới$10^{18}$, vì vậy mô phỏng trực tiếp là không thể. 

Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm nhiều nhất là$3 \cdot 10^5$và giới hạn tương tự được giữ cho tổng số truy vấn. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai trong$n$. MỘT$O(n^2)$đồ thị kề sẽ yêu cầu khoảng$9 \cdot 10^{10}$so sánh trong trường hợp xấu nhất Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log n)$mỗi trường hợp thử nghiệm. 

Bản thân các giá trị nhiều nhất là$10^6$. Nó đủ nhỏ để xử lý trước các thừa số nguyên tố cho mọi số bằng cách sử dụng sàng. 

Một lỗi phổ biến là làm việc với các số gốc thay vì các hạt nhân không có hình vuông của chúng. 

Coi như:```
1
3
12 3 48
1
0
```Cả hai$12$Và$48$giảm xuống$3$sau khi loại bỏ tất cả các thừa số nguyên tố bình phương:$$12 = 2^2 \cdot 3 \rightarrow 3$$

$$48 = 2^4 \cdot 3 \rightarrow 3$$Cả ba giá trị đều thuộc cùng một lớp kề, vì vậy câu trả lời là 3. Việc coi các số ban đầu là khác biệt sẽ tạo ra các nhóm nhỏ hơn một cách không chính xác. 

Một trường hợp tinh tế khác là giá trị 1.```
1
4
1 4 9 16
2
0
1
```Mọi số đều giảm xuống hạt nhân không có hình vuông 1. Ban đầu, kích thước nhóm lớn nhất là 4. Sau khi ổn định, nó vẫn còn 4. Việc quên rằng các ô vuông hoàn hảo thu gọn về hạt nhân 1 sẽ tạo ra các câu trả lời sai. 

Trường hợp cạnh quan trọng nhất liên quan đến lớn$w$.```
1
3
2 2 3
2
0
100
```Câu trả lời cho$w=0$khác với câu trả lời cho$w>0$. Quá trình ổn định sau một lần chuyển đổi, vì vậy tất cả các thời điểm tích cực đều có cùng một câu trả lời. Thời gian mô phỏng sẽ bỏ lỡ sự đơn giản hóa cấu trúc này. 

## Phương pháp tiếp cận 

Giải pháp brute-force trước tiên sẽ xây dựng biểu đồ kề. Đối với mỗi cặp$(i,j)$, tính xem$$\frac{\operatorname{lcm}(a_i,a_j)}{\gcd(a_i,a_j)}$$là một hình vuông hoàn hảo 

Điều này đòi hỏi$O(n^2)$kiểm tra cặp. Ngay cả khi mỗi lần kiểm tra là thời gian không đổi,$n=3\cdot10^5$khiến điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng đến từ hệ số nguyên tố. 

Cho phép$$x=\prod p_i^{\alpha_i}, \qquad y=\prod p_i^{\beta_i}.$$Từ$$\frac{\operatorname{lcm}(x,y)}{\gcd(x,y)} = \prod p_i^{|\alpha_i-\beta_i|},$$kết quả là một bình phương hoàn hảo chính xác khi mọi hiệu số mũ đều là số chẵn. 

Điều đó có nghĩa$$\alpha_i \equiv \beta_i \pmod 2$$cho mỗi số nguyên tố. 

Chỉ có vấn đề tương đương số mũ. 

Với mỗi số, hãy giữ chính xác các số nguyên tố có số mũ là số lẻ. Sản phẩm của họ được gọi là hạt nhân không có hình vuông. 

Ví dụ:$$12 = 2^2\cdot3 \rightarrow 3$$

$$18 = 2\cdot3^2 \rightarrow 2$$

$$72 = 2^3\cdot3^2 \rightarrow 2$$Hai số liền kề nhau khi và chỉ khi chúng có cùng một hạt nhân không có hình vuông. 

Điều này biến vấn đề thành việc đếm tần số của hạt nhân. 

Câu hỏi còn lại là sự phát triển lặp đi lặp lại của mảng. 

Giả sử một lớp kernel chứa$k$các phần tử. Mọi phần tử trong lớp đó nhân chính xác cùng một tập giá trị, cụ thể là tất cả các thành viên của lớp. Do đó, sau một giây, mọi phần tử của lớp sẽ trở nên giống hệt nhau. 

Phân tích sâu hơn cho thấy sau một bước, mọi lớp có kernel là 1 hoặc có kích thước chẵn sẽ thu gọn thành kernel 1. Các lớp có kích thước lẻ và không phải một kernel vẫn có sự khác biệt. 

Đây là quan sát trung tâm được sử dụng trong giải pháp chính thức. 

Cho phép:

-`mx0`là tần số lớn nhất trong số tất cả các hạt nhân. 
-`mx1`là tổng tần số của: 

- hạt nhân 1, 
- mọi hạt nhân không phải một có tần số chẵn. 

Sau đó, tất cả các hạt nhân không phải một tần số lẻ còn lại sẽ tách biệt. 

Đối với bất kỳ thời điểm tích cực nào, câu trả lời sẽ trở thành$$\max(mx1,\text{largest odd-frequency non-one class}).$$Giá trị này theo truyền thống được gọi là`mx_inf`. 

Quá trình ổn định ngay lập tức, vì vậy: 

-$w=0$công dụng`mx0`. 
-$w>0$công dụng`mx_inf`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n \log \log M + n)$sau khi tiền xử lý sàng |$O(n)$| Đã chấp nhận | 

Đây$M=10^6$. 

## Hướng dẫn thuật toán 

### Tiền xử lý 

Tính hệ số nguyên tố nhỏ nhất cho mọi số nguyên tối đa$10^6$. Điều này cho phép chúng ta tính nhanh từng giá trị mảng. 

### Mỗi trường hợp thử nghiệm 

1. Với mỗi giá trị mảng, hãy tính hạt nhân không có hình vuông của nó. 

Phân tích số đó thành nhân tử và chỉ giữ lại các số nguyên tố có số mũ là số lẻ. 
2. Đếm tần số của mỗi hạt nhân. 

Cho phép`cnt[k]`là số lần xuất hiện của kernel`k`. 
3. Tính toán`mx0`. 

Đây đơn giản là tần số tối đa trong số tất cả các hạt nhân. 
4. Tính nhóm lớn ổn định. 

Khởi tạo$$\text{good}=0.$$Đối với mỗi cặp tần số hạt nhân: 

- Nếu kernel bằng 1 thì cộng tần số của nó vào`good`. 
- Ngược lại nếu tần số chẵn thì cộng tần số của nó vào`good`. 

Các nhóm này hợp nhất thành thành phần kernel-1 sau một bước. 
5. Tính lớp không một tần số lẻ lớn nhất còn lại. 

Cho phép$$\text{oddmax}$$là tần số tối đa giữa các hạt nhân khác với 1 có tần số lẻ. 
6. Xác định$$mx\_inf=\max(\text{good},\text{oddmax}).$$Đây là câu trả lời cho mọi thời điểm tích cực. 
7. Đối với mỗi truy vấn: 

- Nếu$w=0$, in`mx0`. 
- Ngược lại thì in`mx_inf`. 

### Tại sao nó hoạt động 

Sự kề cận chỉ phụ thuộc vào tính chẵn lẻ của số mũ trong hệ số nguyên tố. Việc thay thế mỗi số bằng hạt nhân không có hình vuông của nó sẽ duy trì tất cả các mối quan hệ kề cận. 

Tất cả các số có cùng một hạt nhân tạo thành một thành phần hoàn chỉnh và các số có các hạt nhân khác nhau không bao giờ liền kề nhau. Sau một lần chuyển đổi, mọi phần tử bên trong một thành phần sẽ trở nên giống hệt nhau vì mỗi phần tử nhân chính xác cùng một tập hợp giá trị. 

Một thành phần có kích thước chẵn sẽ tạo ra sự đóng góp hệ số bình phương hoàn hảo và thu gọn vào kernel 1. Các thành phần đã có kernel 1 cũng thuộc về đó. Các thành phần không phải một có kích thước lẻ giữ lại kernel của chúng. Sau lần chuyển đổi đầu tiên này, phân vùng không còn thay đổi nữa nên quá trình sẽ ổn định mãi mãi. 

Do đó, vẻ đẹp tại thời điểm 0 là kích thước thành phần ban đầu lớn nhất, trong khi vẻ đẹp tại mọi thời điểm dương là kích thước thành phần lớn nhất sau quá trình ổn định này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 10 ** 6

spf = list(range(MAXA + 1))
for i in range(2, int(MAXA ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXA + 1, i):
            if spf[j] == j:
                spf[j] = i

def kernel(x):
    res = 1
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt ^= 1
        if cnt:
            res *= p
    return res

t = int(input())

for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))

    freq = {}

    for x in arr:
        k = kernel(x)
        freq[k] = freq.get(k, 0) + 1

    mx0 = max(freq.values())

    good = 0
    oddmax = 0

    for k, c in freq.items():
        if k == 1 or c % 2 == 0:
            good += c
        else:
            oddmax = max(oddmax, c)

    mx_inf = max(good, oddmax)

    q = int(input())

    for _ in range(q):
        w = int(input())
        if w == 0:
            print(mx0)
        else:
            print(mx_inf)
```Sàng tính hệ số nguyên tố nhỏ nhất cho mỗi số một lần. Vì tất cả các giá trị lớn nhất$10^6$, chi phí tiền xử lý này được chia sẻ cho mọi trường hợp thử nghiệm. 

các`kernel`hàm loại bỏ các đóng góp nguyên tố bình phương. Thay vì lưu trữ số mũ đầy đủ, nó theo dõi tính chẵn lẻ. Bất cứ khi nào một số nguyên tố xuất hiện với số lần lẻ thì số nguyên tố đó sẽ đóng góp vào kernel. 

Bản đồ tần số thể hiện các thành phần được kết nối trong biểu đồ kề. Mọi số có cùng kernel đều thuộc cùng một thành phần.`mx0`trả lời mảng ban đầu. Các biến`good`Và`oddmax`mã hóa trạng thái ổn định. Sự khác biệt giữa kernel 1, các nhóm có kích thước chẵn và các nhóm không phải là một có kích thước lẻ chính xác là đặc tính đã được chứng minh ở trên. 

Việc xử lý truy vấn cực kỳ đơn giản vì quy trình sẽ ổn định sau một bước. Mỗi truy vấn với$w>0$nhận được câu trả lời tương tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng đầu vào:```
6 8 4 2
```| Giá trị | Nhân tố hóa | Hạt nhân | 
| --- | --- | --- | 
| 6 |$2\cdot3$| 6 | 
| 8 |$2^3$| 2 | 
| 4 |$2^2$| 1 | 
| 2 |$2$| 2 | 

Bảng tần số: 

| Hạt nhân | Tần số | 
| --- | --- | 
| 6 | 1 | 
| 2 | 2 | 
| 1 | 1 | 

Sau đó: 

| Biến | Giá trị | 
| --- | --- | 
| mx0 | 2 | 
| tốt | 3 | 
| lẻmax | 1 | 
| mx_inf | 3 | 

Truy vấn là$w=0$, vậy câu trả lời là`mx0 = 2`. 

Ví dụ này cho thấy thời điểm 0 sử dụng kích thước thành phần ban đầu chứ không phải kích thước ổn định. 

### Mẫu 2 

Mảng đầu vào:```
12 3 20 5 80 1
```Giảm hạt nhân: 

| Giá trị | Hạt nhân | 
| --- | --- | 
| 12 | 3 | 
| 3 | 3 | 
| 20 | 5 | 
| 5 | 5 | 
| 80 | 5 | 
| 1 | 1 | 

Bảng tần số: 

| Hạt nhân | Tần số | 
| --- | --- | 
| 3 | 2 | 
| 5 | 3 | 
| 1 | 1 | 

Các giá trị được tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| mx0 | 3 | 
| tốt | 3 | 
| lẻmax | 3 | 
| mx_inf | 3 | 

Truy vấn là$w=1$, vậy câu trả lời là`mx_inf = 3`. 

Ví dụ này chứng minh rằng một thành phần không phải một có kích thước lẻ có thể vẫn là nhóm lớn nhất ngay cả sau khi ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log \log 10^6 + q)$tổng thể cho mỗi trường hợp thử nghiệm sau khi tiền xử lý | Tính toán hạt nhân gần tuyến tính bằng cách sử dụng hệ số SPF | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi hạt nhân riêng biệt | 

Tổng số phần tử mảng và truy vấn trên tất cả các trường hợp thử nghiệm chỉ$3 \cdot 10^5$. Sàng được tạo một lần và mọi giá trị được nhân nhanh chóng bằng cách sử dụng các thừa số nguyên tố nhỏ nhất của nó. Giải pháp thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from collections import defaultdict

    data = io.StringIO(inp)
    input = data.readline

    MAXA = 10**6

    spf = list(range(MAXA + 1))
    for i in range(2, int(MAXA ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXA + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def kernel(x):
        r = 1
        while x > 1:
            p = spf[x]
            parity = 0
            while x % p == 0:
                x //= p
                parity ^= 1
            if parity:
                r *= p
        return r

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        freq = defaultdict(int)
        for x in arr:
            freq[kernel(x)] += 1

        mx0 = max(freq.values())

        good = 0
        oddmax = 0

        for k, c in freq.items():
            if k == 1 or c % 2 == 0:
                good += c
            else:
                oddmax = max(oddmax, c)

        mx_inf = max(good, oddmax)

        q = int(input())
        for _ in range(q):
            w = int(input())
            out.append(str(mx0 if w == 0 else mx_inf))

    return "\n".join(out) + "\n"

# provided samples
assert run(
"""2
4
6 8 4 2
1
0
6
12 3 20 5 80 1
1
1
"""
) == """2
3
"""

# minimum size
assert run(
"""1
1
1
2
0
100
"""
) == """1
1
"""

# all equal values
assert run(
"""1
5
2 2 2 2 2
2
0
1
"""
) == """5
5
"""

# all perfect squares
assert run(
"""1
4
4 9 16 25
2
0
10
"""
) == """4
4
"""

# mixed parity frequencies
assert run(
"""1
5
2 8 18 50 3
2
0
1
"""
) == """4
4
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị đơn 1 | 1, 1 | Kích thước tối thiểu và xử lý kernel 1 | 
| Tất cả các giá trị bằng nhau | 5, 5 | Một thành phần lớn | 
| Tất cả các hình vuông hoàn hảo | 4, 4 | Mọi thứ sụp đổ vào kernel 1 | 
| Tần số chẵn lẻ hỗn hợp | 4, 4 | Xử lý đúng quy mô lớp chẵn và lẻ | 

## Vỏ cạnh 

### Tất cả các giá trị giảm về kernel 1 

đầu vào:```
1
4
4 9 16 25
1
100
```Các hạt nhân là:```
1 1 1 1
```Bảng tần số:```
{1: 4}
```Chúng tôi nhận được:```
mx0 = 4
good = 4
oddmax = 0
mx_inf = 4
```Trả lời: 4. 

Việc triển khai giữ các giá trị ban đầu riêng biệt sẽ tạo ra bốn nhóm đơn lẻ không chính xác. 

### Thời gian dương lớn 

đầu vào:```
1
3
2 2 3
2
1
1000000000000000000
```Tần số hạt nhân:```
2 -> 2
3 -> 1
```Sau đó:```
mx0 = 2
good = 2
oddmax = 1
mx_inf = 2
```Cả hai truy vấn thời gian tích cực đều trả về 2. 

Thuật toán không bao giờ mô phỏng thời gian. Nó sử dụng thực tế là quá trình ổn định sau một lần chuyển đổi. 

### Thành phần không phải một có kích thước lẻ 

đầu vào:```
1
3
20 5 80
2
0
1
```Cả ba số đều quy về kernel 5. 

Bảng tần số:```
5 -> 3
```Sau đó:```
mx0 = 3
good = 0
oddmax = 3
mx_inf = 3
```Câu trả lời:```
3
3
```Điều này xác minh rằng các nhóm không phải một có kích thước lẻ vẫn tách biệt sau khi ổn định và vẫn phải được xem xét khi tính toán`mx_inf`.
