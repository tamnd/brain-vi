---
title: "CF 2116B - Gellyfish và hơi thở của em bé"
description: "Cho hai hoán vị p và q của các số 0...n-1. Đối với mọi vị trí i, chúng ta phải đánh giá tất cả các cặp chỉ số (j, i-j) sao cho 0 ≤ j ≤ i và tính $$2^{pj}+2^{q{i-j}}$$ Trong số tất cả các ứng cử viên này, chúng ta lấy mức tối đa và lưu trữ nó trong r[i]."
date: "2026-06-09T04:01:15+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2116
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1028 (Div. 2)"
rating: 1300
weight: 2116
solve_time_s: 165
verified: false
draft: false
---

[CF 2116B - Gellyfish và hơi thở của em bé](https://codeforces.com/problemset/problem/2116/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 2m 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai hoán vị`p`Và`q`của những con số`0...n-1`. 

Đối với mọi vị trí`i`, chúng ta phải đánh giá tất cả các cặp chỉ số`(j, i-j)`như vậy`0 ≤ j ≤ i`, và tính toán$$2^{p_j}+2^{q_{i-j}}$$Trong số tất cả các ứng cử viên này, chúng tôi lấy mức tối đa và lưu trữ nó trong`r[i]`. 

Các giá trị trực tiếp trở nên rất lớn vì số mũ có thể lớn bằng`n-1`, vì vậy câu trả lời cuối cùng là bắt buộc theo modulo`998244353`. 

Điều đầu tiên cần chú ý là các số liên quan là lũy thừa của hai. Sức mạnh của hai người phát triển nhanh đến mức khi so sánh$$2^a+2^b$$số mũ lớn hơn chiếm ưu thế hoàn toàn. Chính thức hơn, nếu`a > c`, sau đó$$2^a+2^b > 2^c+2^d$$bất kể giá trị của`b`Và`d`. 

Điều này có nghĩa là việc tối đa hóa tổng tương đương với việc tối đa hóa số mũ lớn hơn đầu tiên trong cặp và chỉ khi các số mũ lớn hơn đó bằng nhau thì chúng ta mới so sánh số mũ nhỏ hơn. 

Những ràng buộc là thứ buộc chúng ta phải khai thác cấu trúc này. Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`. MỘT`O(n^2)`thuật toán sẽ thực hiện khoảng`10^{10}`trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần thứ gì đó xung quanh`O(n log n)`hoặc`O(n)`mỗi trường hợp thử nghiệm. 

Một điểm tinh tế là số mũ tối đa trong một cặp ứng cử viên có thể đến từ một trong hai mảng. 

Coi như:```
p = [2,0,1]
q = [0,2,1]
```Vì`i = 1`, các ứng cử viên là```
j=0: (2,2)
j=1: (0,0)
```Câu trả lời đến từ số mũ`2`và cả hai mảng đều đóng góp như nhau. Bất kỳ giải pháp nào chỉ theo dõi cực đại từ một mảng sẽ bỏ lỡ những trường hợp như vậy. 

Một sai lầm dễ mắc phải khác là tối đa hóa`p[j] + q[i-j]`. Mục tiêu không phải là tổng số mũ. 

Ví dụ:```
(5,0) -> 32+1 = 33
(3,3) -> 8+8 = 16
```Mặc dù`5+0 < 3+3`, cặp đầu tiên lớn hơn nhiều vì số mũ`5`thống trị. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. 

Đối với mọi vị trí`i`, lặp qua tất cả`j`từ`0`ĐẾN`i`, tính toán$$2^{p_j}+2^{q_{i-j}}$$và giữ ở mức tối đa. 

Điều này đúng vì nó kiểm tra rõ ràng mọi cặp hợp lệ. Vấn đề là sự phức tạp. Đối với một trường hợp thử nghiệm, số cặp được kiểm tra là$$1+2+\cdots+n = O(n^2)$$điều đó trở nên đại khái`5 × 10^9`hoạt động khi`n = 10^5`. 

Để làm tốt hơn, chúng ta cần khai thác thực tế rằng`p`Và`q`là những hoán vị. 

Cho phép$$M_i=\max\Big(\max_{0\le j\le i} p_j,\ \max_{0\le j\le i} q_j\Big)$$là số mũ lớn nhất xuất hiện ở tiền tố cho đến vị trí`i`. 

Bất kỳ cặp tối ưu nào cho`r[i]`phải chứa số mũ`M_i`. 

Tại sao? Nếu không bên nào đóng góp`M_i`, thì số mũ lớn nhất trong ứng cử viên đó hoàn toàn nhỏ hơn`M_i`, và một cặp chứa`M_i`sẽ luôn lớn hơn. 

Bởi vì`p`Và`q`là các hoán vị, mỗi số mũ xuất hiện đúng một lần. Giả sử số mũ`M_i`xuất hiện ở`p`ở vị trí`posP[M_i]`. 

Nếu như`posP[M_i] ≤ i`, thì cặp duy nhất chứa số mũ này là$$j = posP[M_i]$$điều này buộc số mũ khác phải là$$q_{i-posP[M_i]}.$$Tương tự, nếu`M_i`xuất hiện ở`q`, cặp phù hợp duy nhất có thể là$$p_{i-posQ[M_i]}.$$Vì vậy đối với mỗi`i`, có nhiều nhất hai ứng viên có thể chứa số mũ lớn nhất`M_i`. 

Khi số mũ lớn nhất được cố định, phép so sánh sẽ giảm xuống việc chọn số mũ thứ hai lớn hơn. Vì vậy chúng ta chỉ cần so sánh hai giá trị có thể. 

Điều này biến vấn đề thành quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước lũy thừa của hai modulo`998244353`lên đến`100000`. 
2. Xây dựng mảng`posP`Và`posQ`lưu trữ vị trí của mọi giá trị bên trong hoán vị`p`Và`q`. 
3. Tính tiền tố cực đại:`prefP[i] = max(p[0..i])`

`prefQ[i] = max(q[0..i])`4. Đối với mọi vị trí`i`, tính toán$$M=\max(prefP[i],prefQ[i]).$$Bất kỳ cặp tối ưu nào cũng phải chứa số mũ`M`. 
5. Nếu`M`xuất hiện bên trong tiền tố của`p`, đặt vị trí của nó là`k=posP[M]`. 

Sau đó, ứng viên tương ứng sử dụng$$(M,\ q_{i-k})$$cung cấp`k ≤ i`. 
6. Nếu`M`xuất hiện bên trong tiền tố của`q`, đặt vị trí của nó là`k=posQ[M]`. 

Sau đó, ứng viên tương ứng sử dụng$$(p_{i-k},\ M)$$cung cấp`k ≤ i`. 
7. Trong số các ứng viên hợp lệ, so sánh số mũ thứ hai. Vì cả hai đều chứa số mũ lớn nhất giống nhau`M`, số mũ thứ hai lớn hơn sẽ cho tổng lớn hơn. 
8. Hãy để cặp được chọn`(M,S)`. đầu ra$$2^M + 2^S \pmod{998244353}.$$### Tại sao nó hoạt động 

Đối với bất kỳ chỉ mục nào`i`, cho phép`M`là số mũ lớn nhất xuất hiện ở một trong hai tiền tố lên đến`i`. 

Mọi cặp ứng cử viên có số mũ lớn nhất nhỏ hơn`M`tự động tệ hơn bất kỳ ứng cử viên nào có chứa`M`, bởi vì lũy thừa của hai được sắp xếp chặt chẽ theo số mũ lớn nhất của chúng. 

Vì mỗi số mũ xuất hiện đúng một lần trong mỗi hoán vị nên chỉ có hai cách có thể để tạo thành một cặp chứa`M`: sử dụng sự xuất hiện của nó trong`p`hoặc sử dụng sự xuất hiện của nó trong`q`. Mọi giải pháp tối ưu đều phải là một trong những ứng cử viên này. 

Sau khi sửa số mũ lớn nhất`M`, so sánh$$2^M+2^a$$Và$$2^M+2^b$$giảm xuống để so sánh`a`Và`b`. Thuật toán chọn số mũ thứ hai lớn hơn, do đó nó chọn chính xác cặp có giá trị lớn nhất. 

Như vậy mọi`r[i]`được tính toán chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 100000

pow2 = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    pow2[i] = (pow2[i - 1] * 2) % MOD

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        q = list(map(int, input().split()))

        posP = [0] * n
        posQ = [0] * n

        for i, x in enumerate(p):
            posP[x] = i

        for i, x in enumerate(q):
            posQ[x] = i

        prefP = [0] * n
        prefQ = [0] * n

        prefP[0] = p[0]
        prefQ[0] = q[0]

        for i in range(1, n):
            prefP[i] = max(prefP[i - 1], p[i])
            prefQ[i] = max(prefQ[i - 1], q[i])

        ans = [0] * n

        for i in range(n):
            M = max(prefP[i], prefQ[i])

            best_second = -1

            k = posP[M]
            if k <= i:
                best_second = max(best_second, q[i - k])

            k = posQ[M]
            if k <= i:
                best_second = max(best_second, p[i - k])

            ans[i] = (pow2[M] + pow2[best_second]) % MOD

        print(*ans)

solve()
```Mảng vị trí là chi tiết triển khai chính. Chúng cho phép chúng ta xác định ngay vị trí một số mũ cụ thể xuất hiện trong một trong hai hoán vị. 

Tiền tố cực đại xác định số mũ lớn nhất có sẵn cho mỗi`i`. Nếu không có chúng, chúng ta sẽ phải tìm kiếm các tiền tố nhiều lần. 

Đối với một cố định`i`, chúng ta chỉ xem xét hai cách để bao gồm số mũ`M`. Số mũ thứ hai tương ứng được đọc trực tiếp từ mảng đối diện. 

Câu trả lời được xây dựng lại bằng cách sử dụng lũy ​​thừa mô-đun được tính toán trước của hai. Khả năng tính toán lặp đi lặp lại với lũy thừa nhanh vẫn có thể được chấp nhận, nhưng việc tính toán trước làm cho mỗi truy vấn có thời gian không đổi. 

Phải cẩn thận với chỉ số`i-k`. Séc`k <= i`đảm bảo rằng chỉ số này không âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
p = [0,2,1]
q = [1,2,0]
```| tôi | prefP | preQ | M | ứng cử viên từ p | ứng cử viên từ q | cặp đã chọn | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | (1,0) | (0,1) | (1,0) | 3 | 
| 1 | 2 | 2 | 2 | (2,1) | (0,2) | (2,1) | 6 | 
| 2 | 2 | 2 | 2 | (2,2) | (1,2) | (2,2) | 8 | 

Bảng này cho thấy ý tưởng trung tâm. Một lần số mũ`2`trở nên có sẵn, mọi cặp tối ưu đều phải chứa nó. Chúng tôi chỉ so sánh số mũ đi kèm. 

### Ví dụ 2 

đầu vào:```
n = 5
p = [0,1,2,3,4]
q = [4,3,2,1,0]
```| tôi | M | số mũ thứ hai | giá trị | 
| --- | --- | --- | --- | 
| 0 | 4 | 0 | 17 | 
| 1 | 4 | 1 | 18 | 
| 2 | 4 | 2 | 20 | 
| 3 | 4 | 3 | 24 | 
| 4 | 4 | 4 | 32 | 

Ở đây số mũ`4`đã có mặt trong phần tử đầu tiên của`q`, vì vậy nó chiếm ưu thế trong mọi câu trả lời. Chỉ có số mũ thứ hai thay đổi khi`i`lớn lên. 

Ví dụ này minh họa tại sao việc theo dõi số mũ lớn nhất có sẵn là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một số lần không đổi | 
| Không gian | O(n) | Vị trí mảng và tiền tố cực đại | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`, do đó thời gian chạy tổng thể là tuyến tính ở kích thước đầu vào. Điều này dễ dàng nằm gọn trong giới hạn một giây và mức sử dụng bộ nhớ vẫn thấp hơn nhiều so với mức 256 MB cho phép. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())

    MAXN = 100000
    pow2 = [1] * (MAXN + 1)
    for i in range(1, MAXN + 1):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    out = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        q = list(map(int, input().split()))

        posP = [0] * n
        posQ = [0] * n

        for i, x in enumerate(p):
            posP[x] = i
        for i, x in enumerate(q):
            posQ[x] = i

        prefP = [0] * n
        prefQ = [0] * n

        prefP[0] = p[0]
        prefQ[0] = q[0]

        for i in range(1, n):
            prefP[i] = max(prefP[i - 1], p[i])
            prefQ[i] = max(prefQ[i - 1], q[i])

        ans = []

        for i in range(n):
            M = max(prefP[i], prefQ[i])

            sec = -1

            k = posP[M]
            if k <= i:
                sec = max(sec, q[i - k])

            k = posQ[M]
            if k <= i:
                sec = max(sec, p[i - k])

            ans.append(str((pow2[M] + pow2[sec]) % MOD))

        out.append(" ".join(ans))

    return "\n".join(out)

# provided sample
assert run(
"""3
3
0 2 1
1 2 0
5
0 1 2 3 4
4 3 2 1 0
10
5 8 9 3 4 0 2 7 1 6
9 5 1 4 0 3 2 8 7 6
"""
) == (
"""3 6 8
17 18 20 24 32
544 768 1024 544 528 528 516 640 516 768"""
)

# n = 1
assert run(
"""1
1
0
0
"""
) == "2"

# maximum exponent appears only in p prefix
assert run(
"""1
2
1 0
0 1
"""
) == "3 4"

# maximum exponent appears only in q prefix
assert run(
"""1
2
0 1
1 0
"""
) == "3 4"

# tie on largest exponent, compare second exponent
assert run(
"""1
3
2 0 1
0 2 1
"""
) == "5 6 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`2`| Ví dụ nhỏ nhất có thể | 
|`[1,0]`Và`[0,1]`|`3 4`| Số mũ lớn nhất đến từ`p`| 
|`[0,1]`Và`[1,0]`|`3 4`| Số mũ lớn nhất đến từ`q`| 
|`[2,0,1]`Và`[0,2,1]`|`5 6 6`| Hòa đúng theo số mũ thứ hai | 

## Vỏ cạnh 

### Số mũ lớn nhất chỉ xuất hiện ở tiền tố của một mảng 

Hãy xem xét:```
n = 2
p = [1,0]
q = [0,1]
```Vì`i = 1`, số mũ lớn nhất có sẵn là`1`, đến từ`p[0]`. 

Thuật toán tìm`posP[1]=0`, đánh giá đối tác phù hợp`q[1]=1`, và thu được`(1,1)`. 

Câu trả lời là$$2^1+2^1=4.$$Không cần quét qua tất cả các cặp. 

### Số mũ lớn nhất xuất hiện ở cả hai tiền tố 

Hãy xem xét:```
n = 3
p = [2,0,1]
q = [0,2,1]
```Vì`i = 2`, số mũ`2`có sẵn từ cả hai mảng. 

Hai ứng cử viên đều```
(2,1)
(1,2)
```Cả hai đều tương ứng với giá trị`6`. 

Thuật toán kiểm tra cả hai lần xuất hiện và giữ số mũ thứ hai lớn hơn, tức là`1`, cho kết quả đúng 

### Số mũ tối đa lấn át tất cả số mũ nhỏ hơn 

Hãy xem xét:```
n = 3
p = [2,1,0]
q = [0,1,2]
```Vì`i = 2`, ứng viên bao gồm```
(2,2)
(1,1)
(0,0)
```Mặc dù`(1,1)`có một cặp cân bằng, nó không thể đánh bại`(2,2)`bởi vì số mũ`2`thống trị mọi ứng cử viên có số mũ lớn nhất là`1`. 

Thuật toán dựa chính xác vào thuộc tính này khi nó hạn chế sự chú ý đến các cặp chứa số mũ lớn nhất hiện có.
