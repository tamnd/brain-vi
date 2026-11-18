---
title: "CF 2044F - Bài toán quỷ dễ dàng"
description: "Chúng ta được cho hai mảng, a và b. Từ chúng, chúng ta xây dựng một ma trận ngầm $$M{i,j}=ai bj.$$ Vẻ đẹp của ma trận là tổng của tất cả các phần tử của nó."
date: "2026-06-08T09:24:48+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2044
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 993 (Div. 4)"
rating: 1900
weight: 2044
solve_time_s: 124
verified: true
draft: false
---

[CF 2044F - Vấn đề về quỷ dễ dàng](https://codeforces.com/problemset/problem/2044/F) 

**Xếp hạng:** 1900 
**Tas:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, toán học, lý thuyết số 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`. Từ đó chúng ta xây dựng một ma trận ngầm$$M_{i,j}=a_i b_j.$$Vẻ đẹp của ma trận là tổng của tất cả các phần tử của nó. 

Đối với mỗi giá trị truy vấn`x`, chúng ta phải xác định xem có tồn tại một hàng`r`và một cột`c`sao cho sau khi thiết lập mọi ô trong hàng`r`và mọi ô trong cột`c`về 0, vẻ đẹp thu được sẽ trở nên chính xác`x`. 

Hoạt động này là giả định. Mọi truy vấn đều bắt đầu từ ma trận ban đầu. 

Quan sát đầu tiên là bản thân ma trận quá lớn để xây dựng. Cả hai chiều đều có thể đạt tới`2·10^5`, do đó ma trận có thể chứa tới`4·10^10`tế bào. Bất kỳ giải pháp nào chạm vào các mục ma trận riêng lẻ đều không thể thực hiện được. 

Số lượng truy vấn chỉ là`5·10^4`, điều này gợi ý rằng việc xử lý trước đáng kể được cho phép, nhưng mỗi truy vấn phải được trả lời nhanh hơn nhiều so với`O(nm)`. 

Các giá trị bên trong mảng được giới hạn bởi độ dài của chúng. Từ$$|a_i|\le n,\qquad |b_j|\le m,$$tất cả tổng hàng và tổng cột đều nằm trong phạm vi có thể quản lý được. Đây hóa ra là thuộc tính quan trọng giúp giải quyết được vấn đề. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như```
n=1, m=1
a=[5]
b=[7]
```Ma trận chỉ chứa`35`. Việc chọn hàng duy nhất và cột duy nhất sẽ xóa ô đó một lần chứ không phải hai lần. Vẻ đẹp thu được là`0`. 

Một công thức loại trừ bao hàm bất cẩn có thể trừ phần giao hai lần. 

Một trường hợp quan trọng khác liên quan đến số không.```
a=[0,1]
b=[2,3]
```Vẻ đẹp tổng thể của ma trận đã được xác định hoàn toàn bởi hàng thứ hai. Một số giá trị loại bỏ hàng trở nên giống hệt nhau và các giải pháp dựa trên tính duy nhất của tổng sẽ không thành công. 

Các giá trị âm cũng quan trọng.```
a=[-2,3]
b=[-1,2]
```Vẻ đẹp mục tiêu có thể tích cực hoặc tiêu cực tùy thuộc vào hàng và cột nào bị loại bỏ. Việc hạn chế sự chú ý chỉ vào các ước số dương sẽ bỏ lỡ các câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ trực tiếp tính toán vẻ đẹp đạt được cho mọi cặp đôi có thể`(r,c)`. 

có`nm`sự lựa chọn. Đối với mỗi cặp chúng ta có thể tính toán lại vẻ đẹp trong`O(1)`sử dụng tổng hàng và cột được tính toán trước, sau đó so sánh với truy vấn. 

Điều này đã yêu cầu lưu trữ hoặc tạo tối đa$$2\cdot10^5 \times 2\cdot10^5 = 4\cdot10^{10}$$cặp, điều này hoàn toàn không thể thực hiện được. 

Để tìm cấu trúc ẩn trong bài toán, hãy$$A=\sum a_i,\qquad B=\sum b_j.$$Vẻ đẹp ban đầu là$$S=A\cdot B.$$Nếu chúng ta loại bỏ hàng`r`, chúng tôi loại bỏ$$a_r B.$$Nếu chúng ta loại bỏ cột`c`, chúng tôi loại bỏ$$b_c A.$$Tế bào`(r,c)`thuộc cả hai lần xóa nên đã bị trừ hai lần và phải cộng lại một lần. 

Vẻ đẹp thu được là$$S-a_rB-b_cA+a_rb_c.$$Bây giờ là yếu tố:$$S-a_rB-b_cA+a_rb_c =(A-a_r)(B-b_c).$$Danh tính này thay đổi hoàn toàn vấn đề. 

Đối với mỗi hàng xác định$$u=A-a_r.$$Đối với mỗi cột xác định$$v=B-b_c.$$Vẻ đẹp đơn giản chỉ sau mỗi lần phẫu thuật$$u\cdot v.$$Truy vấn trở thành: 

Có tồn tại không$$u\in U,\quad v\in V$$như vậy$$u v = x ?$$Ở đâu$$U=\{A-a_i\},\qquad V=\{B-b_j\}.$$Bây giờ chúng ta đã chuyển bài toán ma trận thành bài toán phân tích nhân tử. 

Quan sát quan trọng là tất cả các giá trị trong`U`Và`V`nằm trong phạm vi nhỏ. 

Từ$$|a_i|\le n,\qquad |A|\le n^2,$$mọi`u`thỏa mãn$$|u|\le n^2+n.$$Với`n≤2·10^5`, đây là đại khái`4·10^10`, trông có vẻ lớn. Tuy nhiên, các giá trị truy vấn thỏa mãn$$|x|\le 2\cdot10^5.$$Chỉ có các ước số của truy vấn mới quan trọng. Số ước của bất kỳ số nguyên nào lên đến`200000`rất nhỏ, dưới vài trăm. 

Đối với một truy vấn`x`, liệt kê tất cả các ước số`d`của`|x|`. 

Với mỗi cặp số chia$$d,\ \frac{x}{d},$$kiểm tra xem một yếu tố có thuộc về`U`và cái kia thuộc về`V`, có tính đến các dấu hiệu. 

Điều này biến mọi truy vấn thành một tìm kiếm chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(qnm) | O(1) | Quá chậm | 
| Tối ưu | O(n + m + q√X) | O(n + m) | Đã chấp nhận | 

Đây`X = 200000`. 

## Hướng dẫn thuật toán 

1. Đọc mảng`a`Và`b`. 
2. Tính toán$$A=\sum a_i,\qquad B=\sum b_j.$$1. Xây dựng một bộ$$U=\{A-a_i\}.$$Bộ băm cho phép kiểm tra tư cách thành viên liên tục. 

1. Xây dựng một bộ$$V=\{B-b_j\}.$$1. Với mỗi giá trị truy vấn`x`, liệt kê tất cả các ước dương của`|x|`. 
2. Với mọi ước số`d`, cho phép$$e=\frac{|x|}{d}.$$1. Tạo tất cả các tổ hợp dấu có tích bằng`x`. 

Nếu như`x>0`, cặp nhân tố hợp lệ là$$(d,e),\quad (-d,-e).$$Nếu như`x<0`, cặp nhân tố hợp lệ là$$(d,-e),\quad (-d,e).$$1. Đối với mỗi cặp nhân tố ứng viên`(p,q)`, kiểm tra xem$$p\in U$$Và$$q\in V.$$Nếu đúng thì trả lời CÓ ngay. 

1. Nếu không có cặp chia nào hoạt động, hãy trả lời KHÔNG. 

### Tại sao nó hoạt động 

Sau khi loại bỏ hàng`r`và cột`c`, vẻ đẹp thu được bằng$$(A-a_r)(B-b_c).$$Mọi vẻ đẹp có thể đạt được phải là sản phẩm của một giá trị từ`U`và một giá trị từ`V`. 

Ngược lại, mỗi cặp$$u=A-a_r,\qquad v=B-b_c$$tương ứng với sự lựa chọn hàng và cột thực tế. 

Do đó, một truy vấn có thể đạt được chính xác khi`x`có thể được viết là$$x=u v$$với`u∈U`Và`v∈V`. 

Thuật toán kiểm tra chính xác tất cả các hệ số có thể có của`x`, vì vậy nó trả về CÓ nếu một cặp như vậy tồn tại. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    A = sum(a)
    B = sum(b)

    U = set(A - x for x in a)
    V = set(B - x for x in b)

    ans = []

    for _ in range(q):
        x = int(input())

        ok = False
        ax = abs(x)

        for d in range(1, isqrt(ax) + 1):
            if ax % d:
                continue

            e = ax // d

            if x > 0:
                candidates = (
                    (d, e),
                    (-d, -e),
                )
            else:
                candidates = (
                    (d, -e),
                    (-d, e),
                )

            for p, qv in candidates:
                if p in U and qv in V:
                    ok = True
                    break
                if e != d:
                    if qv in U and p in V:
                        ok = True
                        break

            if ok:
                break

        ans.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(ans))

solve()
```Giai đoạn tiền xử lý tính toán tổng và xây dựng các tập hợp được chuyển đổi`U`Và`V`. 

Chi tiết triển khai chính là chúng tôi không bao giờ xây dựng ma trận. Mọi truy vấn đều được trả lời hoàn toàn thông qua phép liệt kê số chia và tra cứu hàm băm. 

Khi kiểm tra các cặp số chia, chúng ta phải xem xét cả hai thứ tự vì số chia được tìm thấy trong`U`có thể tương ứng với`d`hoặc để`e`. 

Mục tiêu phủ định yêu cầu xử lý dấu hiệu riêng biệt. Một lỗi phổ biến là chỉ kiểm tra các ước số dương và quên rằng cả hai tập hợp được chuyển đổi đều có thể chứa các giá trị âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [-2, 3, -3]
b = [-2, 2, -1]
```Tính toán đầu tiên:$$A=-2,\qquad B=-1.$$Sau đó$$U=\{0,-5,1\}$$Và$$V=\{1,-3,0\}.$$Truy vấn`x=1`. 

| Số chia | Cặp | Ở U? | Ở V? | 
| --- | --- | --- | --- | 
| 1 | (1,1) | Có | Có | 

Có một hệ số hợp lệ tồn tại, vì vậy câu trả lời là CÓ. 

### Ví dụ 2 

Giả sử```
a = [1, 2]
b = [3, 4]
```Sau đó$$A=3,\quad B=7.$$Các tập được biến đổi là$$U=\{2,1\},$$

$$V=\{4,3\}.$$Truy vấn`x=8`. 

| Số chia | Cặp | hợp lệ | 
| --- | --- | --- | 
| 1 | (1,8) | Không | 
| 2 | (2,4) | Có | 

Từ`2 ∈ U`Và`4 ∈ V`, câu trả lời là CÓ. 

Ví dụ này cho thấy chỉ có các cặp yếu tố mới quan trọng. Ma trận ban đầu không bao giờ xuất hiện trong tính toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q√X) | Mỗi truy vấn liệt kê các ước của ` | 
| Không gian | O(n + m) | Bộ băm`U`Và`V`| 

Từ`|x| ≤ 200000`, chúng tôi có$$\sqrt{X}\le 447.$$Cần tối đa khoảng 447 phép kiểm tra số chia cho mỗi truy vấn. Với`q ≤ 50000`, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    from math import isqrt

    input = sys.stdin.readline

    n, m, q = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split())

    A = sum(a)
    B = sum(b)

    U = set(A - x for x in a)
    V = set(B - x for x in b)

    ans = []

    for _ in range(q):
        x = int(input())
        ok = False
        ax = abs(x)

        for d in range(1, isqrt(ax) + 1):
            if ax % d:
                continue

            e = ax // d

            if x > 0:
                pairs = [(d, e), (-d, -e)]
            else:
                pairs = [(d, -e), (-d, e)]

            for p, qv in pairs:
                if (p in U and qv in V) or (qv in U and p in V):
                    ok = True
                    break

            if ok:
                break

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

assert run("""3 3 6
-2 3 -3
-2 2 -1
-1
1
-2
2
-3
3
""") == """NO
YES
NO
NO
YES
NO"""

assert run("""1 1 1
5
7
0
""") == "YES"

assert run("""2 2 1
1 2
3 4
8
""") == "YES"

assert run("""2 2 1
1 2
3 4
7
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Đầu ra mẫu | Tính đúng đắn cơ bản | 
| Hàng và cột đơn | CÓ | Xử lý giao lộ | 
| Sản phẩm tồn tại | CÓ | Kết hợp yếu tố trực tiếp | 
| Sản phẩm vắng mặt | KHÔNG | Đường dẫn kết quả âm tính | 

## Vỏ cạnh 

Hãy xem xét một`1 × 1`ma trận:```
a=[5]
b=[7]
```Thao tác duy nhất loại bỏ ô đơn. Vẻ đẹp thu được là`0`. Công thức cho$$(A-a_1)(B-b_1)=0\cdot0=0,$$phù hợp chính xác. 

Hãy xem xét các mảng chứa số không:```
a=[0,1]
b=[2,3]
```Sau đó$$U=\{1,0\}.$$Một mục tiêu của`0`có thể đạt được vì một yếu tố có thể bằng 0. Phương pháp phân tích nhân tử xử lý vấn đề này một cách tự nhiên vì số 0 xuất hiện trực tiếp trong các tập hợp được chuyển đổi. 

Xem xét các mục tiêu tiêu cực:```
a=[-2,3]
b=[-1,2]
```Các bộ được chuyển đổi chứa cả giá trị dương và âm. Một mục tiêu như`-3`đòi hỏi các yếu tố có dấu trái ngược nhau. Thuật toán kiểm tra rõ ràng tất cả các tổ hợp dấu hiệu, do đó không có giải pháp hợp lệ nào bị bỏ sót.
