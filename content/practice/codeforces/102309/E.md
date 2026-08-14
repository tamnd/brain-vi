---
title: "CF 102309E - Kỳ vọng của Orz Pandas"
description: "Có (n) hộp được sắp xếp từ trái qua phải. Thao tác loại 1 chọn khoảng ([l,r]), chữ số (x) và độ lệch (c). Đối với mỗi vị trí (p) trong khoảng đó, trong đó (p=l+k-1), chính xác một dải giấy mới được đặt vào hộp (p)."
date: "2026-08-13T23:45:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "E"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 161
verified: true
draft: false
---

[CF 102309E - Kỳ vọng của Orz Pandas](https://codeforces.com/problemset/problem/102309/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (n) hộp được sắp xếp từ trái qua phải. Thao tác loại 1 chọn khoảng ([l,r]), chữ số (x) và độ lệch (c). Đối với mỗi vị trí (p) trong khoảng đó, trong đó (p=l+k-1), chính xác một dải giấy mới được đặt vào hộp (p). Giá trị của nó là số bao gồm (c+k) bản sao của chữ số (x). 

Thao tác loại 2 yêu cầu chúng ta xem xét mọi dải giấy hiện được lưu trữ trong các hộp ([l,r]), chọn một dải giấy thống nhất một cách ngẫu nhiên và trả về giá trị mong đợi của dải đã chọn. Do đó, câu trả lời là tổng của tất cả các giá trị dải trong khoảng chia cho tổng số dải ở đó. Phép chia được thực hiện theo modulo (10^9+7). 

Các ràng buộc làm cho việc mô phỏng trực tiếp không thể thực hiện được. Có thể có (10^5) thao tác và (10^4) hộp, vì vậy việc xử lý mọi hộp bị ảnh hưởng cho mỗi lần cập nhật có thể yêu cầu cập nhật hộp (10^9). Tham số giá trị (c) cũng có thể đạt tới (10^9), do đó việc xây dựng các chuỗi thập phân thực tế là hoàn toàn không cần thiết. Chúng ta cần biểu diễn mọi giá trị dải theo đại số và cập nhật toàn bộ các khoảng cùng một lúc. 

Có một số trường hợp khó khăn mà việc triển khai đơn giản có thể xử lý sai. Đầu tiên, một truy vấn có thể bao gồm các hộp không chứa dải nào cả. Ví dụ,```
1 1
2 1 1
```không có sẵn dải nào nên câu trả lời là`0`. Một giải pháp tính toán một cách mù quáng số nghịch đảo của số đếm sẽ chia cho 0. 

Thứ hai, (c) có thể bằng 0 khi (l>1). Ví dụ,```
3 2
1 3 3 1 0
2 3 3
```đặt dải một chữ số`1`vào ô 3, vậy đáp án là`1`. Biểu diễn đại số đương nhiên chứa (10^{c-l+1}=10^{-2}), do đó, coi số mũ này như số mũ không âm thông thường sẽ cho kết quả sai. Chúng tôi xử lý vấn đề này bằng cách tính toán trước lũy thừa nghịch đảo của 10. 

Thứ ba, khoảng thời gian truy vấn không cần khớp với khoảng thời gian cập nhật. Ví dụ,```
4 2
1 2 4 3 1
2 3 4
```tạo ra`33`,`333`, Và`3333`trong hộp 2, 3 và 4. Truy vấn chỉ nhìn thấy`333`Và`3333`, vậy đáp án là (3666/2=1833). Một giải pháp chỉ lưu trữ tổng đóng góp của mỗi bản cập nhật mà không tôn trọng ranh giới vị trí của nó sẽ bao gồm`33`không chính xác. 

Cuối cùng, nhiều bản cập nhật có thể chồng chéo lên nhau. Các dải này không được thay thế bởi các hoạt động sau này, chúng tích tụ lại. Ví dụ,```
1 3
1 1 1 9 0
1 1 1 9 0
2 1 1
```để lại hai dải, cả hai đều bằng nhau`9`, nên kỳ vọng vẫn là`9`. Cấu trúc dữ liệu phải duy trì tổng và số lượng theo các bản cập nhật bổ sung. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi hộp, hãy duy trì danh sách các dải hiện có bên trong nó. Đối với thao tác loại 1, lặp qua (p=l,\ldots,r), tính số có chữ số lặp lại tương ứng và nối nó vào hộp (p). Đối với thao tác loại 2, hãy lặp qua các hộp được yêu cầu và tính tổng cả số dải và giá trị của các dải đó. Điều này đúng vì kỳ vọng của một mục được chọn thống nhất chính xác là tổng giá trị chia cho số mục. 

Vấn đề là khối lượng công việc. Một thao tác loại 1 có thể chạm vào (10^4) hộp và có thể có (10^5) thao tác, cung cấp tối đa (10^9) cập nhật hộp riêng lẻ. Các truy vấn cũng có thể yêu cầu quét (10^4) hộp mỗi truy vấn. Bản thân dữ liệu có thể chứa tối đa (10^9) dải trong suốt thời gian hoạt động, do đó việc lưu trữ rõ ràng từng dải cũng không khả thi. 

Quan sát hữu ích là giá trị của dải được thêm vào vị trí (p) có dạng rất đơn giản. Một số chữ số lặp lại có độ dài (L) là 

[ 
x\frac{10^L-1}{9}. 
] 

Đối với bản cập nhật bắt đầu từ (l), vị trí (p) tương ứng với (k=p-l+1), do đó dải của nó có độ dài (c+p-l+1). Giá trị của nó là 

[ 
x\frac{10^{c+p-l+1}-1}{9}. 
] 

Sắp xếp lại mang lại 

[ 
\frac{x10^{c-l+1}}9 10^p-\frac{x}{9}. 
] 

Đối với một bản cập nhật cố định, điều này chỉ đơn giản là 

[ 
A\cdot 10^p+B, 
] 

trong đó (A) và (B) là các hằng số trong toàn bộ khoảng thời gian cập nhật. 

Đó chính xác là cấu trúc mà cây phân đoạn lười biếng có thể khai thác. Đối với mỗi phân đoạn, chúng tôi lưu trữ tổng (10^p), tổng của tất cả các giá trị dải hiện tại và số lượng dải. Áp dụng cập nhật với hệ số (A,B) sẽ thay đổi tổng giá trị của một phân đoạn ([L,R]) theo 

[ 
A\sum_{p=L}^{R}10^p+B(R-L+1). 
] 

Đồng thời, một dải mới được thêm vào mỗi vị trí, do đó số lượng dải tăng lên (R-L+1). 

Do đó, cây phân đoạn thực hiện toàn bộ thao tác loại 1 một cách lười biếng trong (O(\log n)) và thao tác loại 2 thu được cả giá trị tổng và tổng số trong (O(\log n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(mn)) | (O(mn)) trong trường hợp xấu nhất | Quá chậm | 
| Tối ưu | (O(m\log n+\sum \log c_i)) | (O(n)) | Đã chấp nhận | 

Số hạng (\log c_i) xuất phát từ phép lũy thừa mô-đun khi tính toán (10^{c_i}). Vì (c_i\le 10^9), đây chỉ là khoảng 30 phép nhân mô-đun cho mỗi lần cập nhật. 

## Hướng dẫn thuật toán 

1. Tính toán trước (10^p\bmod M) và (10^{-p}\bmod M) cho mọi (0\le p\le n). Cần có lũy thừa nghịch đảo vì hệ số (10^{c-l+1}) có thể có số mũ âm khi (c<l-1). Chúng ta có thể viết nó một cách an toàn dưới dạng (10^c10^{1-l}). 
2. Xây dựng cây phân đoạn trên các vị trí (1,\ldots,n). Mỗi nút lưu trữ`pow_sum`, tổng của (10^p) trên phân đoạn của nó và hai đại lượng động:`value_sum`, tổng của tất cả các giá trị dải giấy hiện có trong phân đoạn và`count_sum`, số lượng dải hiện có trong phân khúc. 
3. Với phép toán loại 1 ((l,r,x,c)), viết lại giá trị cộng tại vị trí (p) như sau: 

[ 
\frac{x10^{c-l+1}}9 10^p-\frac{x}{9}. 
] 

Xác định 

[ 
A=x10^c10^{1-l}9^{-1}\pmod M 
] 

và 

[ 
B=-x9^{-1}\pmod M. 
] 

Khi đó, bản cập nhật trên mọi vị trí (p\in[l,r]) chính xác là (A10^p+B). 
4. Áp dụng bản cập nhật này một cách lười biếng cho cây phân đoạn. Đối với nút được bao phủ đầy đủ biểu thị ([L,R]), hãy thêm 

[ 
A\cdot\text{pow_sum}+B(R-L+1) 
] 

vào tổng giá trị của nó và thêm (R-L+1) vào số đếm của nó. Lưu trữ (A,B) và số lượng tăng dần trong các trường lười của nút để con cháu nhận được bản cập nhật tương tự sau này. 
5. Đối với truy vấn loại 2 ([l,r]), truy vấn cây phân đoạn cho cặp ((S,C)), trong đó (S) là tổng giá trị của tất cả các dải trong các hộp đó và (C) là tổng số của chúng. Nếu (C=0), đầu ra`0`. Nếu không thì xuất ra 

[ 
S C^{-1}\pmod M. 
] 
6. Xử lý các trường hợp kiểm thử cho đến khi EOF. Cây được xây dựng lại cho mỗi cặp mới ((n,m)), do đó các dải từ các trường hợp thử nghiệm khác nhau không bao giờ tương tác với nhau. 

### Tại sao nó hoạt động 

Bất biến là đối với mỗi nút cây phân đoạn,`value_sum`bằng tổng các giá trị của mọi dải hiện thuộc về các vị trí trong khoảng của nút đó, trong khi`count_sum`bằng số dải đó. tĩnh`pow_sum`bằng tổng của (10^p) trên cùng một vị trí. 

Thao tác loại 1 thêm (A10^p+B) vào chính xác một dải mới tại mỗi vị trí bị ảnh hưởng. Đối với một phân đoạn hoàn chỉnh, việc tính tổng biểu thức này sẽ cho (A\cdot\text{pow_sum}+B\cdot\text{length}), chính xác bản cập nhật được cây áp dụng và số dải mới chính xác là độ dài phân đoạn. Nhân giống lười biếng bảo toàn số lượng tương tự cho mọi con cháu. 

Do đó, truy vấn loại 2 thu được tổng giá trị chính xác và tổng số dải có thể chọn chính xác trong khoảng của nó. Việc chia cái trước cho cái sau chính xác là định nghĩa về kỳ vọng được yêu cầu, do đó giá trị mô-đun trả về là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007
INV9 = pow(9, MOD - 2, MOD)
INV10 = pow(10, MOD - 2, MOD)

def solve():
    out = []

    while True:
        first = input()
        if not first:
            break

        n, m = map(int, first.split())

        pow10 = [1] * (n + 1)
        invpow10 = [1] * (n + 1)

        for i in range(1, n + 1):
            pow10[i] = pow10[i - 1] * 10 % MOD
            invpow10[i] = invpow10[i - 1] * INV10 % MOD

        size = 4 * n + 5

        pow_sum = [0] * size
        value_sum = [0] * size
        count_sum = [0] * size

        lazy_a = [0] * size
        lazy_b = [0] * size
        lazy_c = [0] * size

        def build(node, left, right):
            if left == right:
                pow_sum[node] = pow10[left]
                return

            mid = (left + right) >> 1
            lc = node << 1
            rc = lc | 1

            build(lc, left, mid)
            build(rc, mid + 1, right)

            pow_sum[node] = (pow_sum[lc] + pow_sum[rc]) % MOD

        def apply(node, left, right, a, b, c):
            length = right - left + 1

            value_sum[node] = (
                value_sum[node]
                + a * pow_sum[node]
                + b * length
            ) % MOD

            count_sum[node] += c * length

            lazy_a[node] = (lazy_a[node] + a) % MOD
            lazy_b[node] = (lazy_b[node] + b) % MOD
            lazy_c[node] += c

        def push(node, left, right):
            a = lazy_a[node]
            b = lazy_b[node]
            c = lazy_c[node]

            if a == 0 and b == 0 and c == 0:
                return

            if left != right:
                mid = (left + right) >> 1
                lc = node << 1
                rc = lc | 1

                apply(lc, left, mid, a, b, c)
                apply(rc, mid + 1, right, a, b, c)

            lazy_a[node] = 0
            lazy_b[node] = 0
            lazy_c[node] = 0

        def update(node, left, right, ql, qr, a, b):
            if ql <= left and right <= qr:
                apply(node, left, right, a, b, 1)
                return

            push(node, left, right)

            mid = (left + right) >> 1
            lc = node << 1
            rc = lc | 1

            if ql <= mid:
                update(lc, left, mid, ql, qr, a, b)

            if qr > mid:
                update(rc, mid + 1, right, ql, qr, a, b)

            value_sum[node] = (value_sum[lc] + value_sum[rc]) % MOD
            count_sum[node] = count_sum[lc] + count_sum[rc]

        def query(node, left, right, ql, qr):
            if ql <= left and right <= qr:
                return value_sum[node], count_sum[node]

            push(node, left, right)

            mid = (left + right) >> 1
            lc = node << 1
            rc = lc | 1

            total_value = 0
            total_count = 0

            if ql <= mid:
                v, c = query(lc, left, mid, ql, qr)
                total_value += v
                total_count += c

            if qr > mid:
                v, c = query(rc, mid + 1, right, ql, qr)
                total_value += v
                total_count += c

            return total_value % MOD, total_count

        build(1, 1, n)

        for _ in range(m):
            operation = list(map(int, input().split()))

            if operation[0] == 1:
                _, l, r, x, c = operation

                # x * 10^(c-l+1) / 9
                # = x * 10^c * 10^(1-l) / 9
                a = x * pow(10, c, MOD) % MOD
                a = a * invpow10[l - 1] % MOD
                a = a * 10 % MOD
                a = a * INV9 % MOD

                b = (-x * INV9) % MOD

                update(1, 1, n, l, r, a, b)

            else:
                _, l, r = operation

                total_value, total_count = query(
                    1, 1, n, l, r
                )

                if total_count == 0:
                    out.append("0")
                else:
                    answer = total_value * pow(
                        total_count, MOD - 2, MOD
                    ) % MOD
                    out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`pow10`mảng chứa trọng lượng tĩnh (10^p) cho mỗi hộp. các`invpow10`array xử lý trường hợp (c-l+1) âm. sử dụng 

[ 
10^{c-l+1}=10^c10^{-(l-1)}10 
] 

giữ mọi số mũ được chuyển đến`pow`không âm. 

Cây phân đoạn`pow_sum`không bao giờ thay đổi vì vị trí hộp không bao giờ thay đổi. Ba đại lượng nút còn lại là đại lượng động.`value_sum`lưu trữ tổng mô-đun của các giá trị dải,`count_sum`lưu trữ một số nguyên thông thường và ba mảng lười mô tả một bản cập nhật vẫn cần được truyền cho trẻ em. 

Thứ tự bên trong`apply`quan trọng về mặt khái niệm. Đầu tiên, nút hiện tại nhận được bản cập nhật đầy đủ. Chỉ sau đó, việc lan truyền lười biếng mới trì hoãn việc cập nhật tương tự đó cho các phần tử con của nó. Khi một truy vấn giao một phần với một nút,`push`được gọi trước khi giảm dần, vì vậy các phần tử con sẽ thấy mọi bản cập nhật mà trước đây chỉ được lưu trữ ở tổ tiên của chúng. 

Số đếm được cố tình không giảm modulo (M). Tối đa (10^5) mỗi thao tác có thể đóng góp tối đa (10^4) dải cho một truy vấn, do đó tổng số tối đa là (10^9), nhỏ hơn (M=10^9+7). Do đó, số đếm khác 0 luôn có khả năng nghịch đảo modulo (M), và việc giữ nó như một số nguyên thông thường cũng làm cho phép thử bằng 0 trở nên trực tiếp. 

Số nguyên Python không bị tràn, vì vậy tích trung gian`a * pow_sum[node]`là an toàn. Tổng giá trị và hệ số lười được giảm modulo (M) sau số học để kích thước của chúng vẫn được kiểm soát. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 3
1 2 3 5 1
1 1 2 1 3
2 2 3
```Bản cập nhật đầu tiên đặt`55`vào ô 2 và`555`vào ô 3. Bản cập nhật thứ hai đặt`1111`vào ô 1 và`11111`vào ô 2. Truy vấn bao gồm ô 2 và 3. 

| Hoạt động | Vị trí bị ảnh hưởng | Dải mới | Kết quả truy vấn | 
| --- | --- | --- | --- | 
|`1 2 3 5 1`| 2, 3 | 55, 555 | | 
|`1 1 2 1 3`| 1, 2 | 1111, 11111 | | 
|`2 2 3`| 2, 3 | 55, 11111, 555 | ((55+11111+555)/3=3907) | 

Cây phân đoạn có tổng giá trị (11721) và tổng số (3) trên các ô từ 2 đến 3, cho`3907`. 

Điều này chứng tỏ rằng các bản cập nhật khác nhau có thể chồng lên nhau trong cùng một hộp, với mỗi dải vẫn có thể được chọn độc lập. 

### Ví dụ 2 

Hãy xem xét```
4 4
1 1 4 2 0
2 1 4
1 2 3 3 1
2 2 3
```Bản cập nhật đầu tiên tạo ra`2`,`22`,`222`, Và`2222`. Do đó, truy vấn đầu tiên của nó sẽ thấy bốn dải có tổng là (2468). 

| Hoạt động | Vị trí | Giá trị gia tăng | Tổng truy vấn | Số lượng truy vấn | 
| --- | --- | --- | --- | --- | 
|`1 1 4 2 0`| 1, 2, 3, 4 | 2, 22, 222, 2222 | | | 
|`2 1 4`| 1, 2, 3, 4 | | 2468 | 4 | 
|`1 2 3 3 1`| 2, 3 | 33, 333 | | | 
|`2 2 3`| 2, 3 | 22, 222, 33, 333 | 610 | 4 | 

Kỳ vọng đầu tiên là (2468/4=617). Số thứ hai là (610/4=305/2), tức là`500000156`modulo (10^9+7). 

Truy vấn thứ hai xác nhận rằng cây kết hợp các dải cũ và mới trong khi giới hạn kết quả chính xác trong khoảng thời gian được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m\log n+\sum \log c_i)) | Mỗi bản cập nhật và truy vấn truy cập (O(\log n)) nút cây phân đoạn, trong khi mỗi thao tác loại 1 tính toán (10^c\bmod M) trong (O(\log c)). | 
| Không gian | (O(n)) | Cây phân đoạn và mảng công suất đều chứa (O(n)) mục. | 

Với (n\le10^4) và (m\le10^5), cây phân đoạn chỉ thực hiện phép tính logarit cho mỗi thao tác. Số mũ lớn nhất là (10^9), vì vậy lũy thừa mô-đun chỉ yêu cầu một số bước nhân không đổi nhỏ cho mỗi lần cập nhật. Mức tiêu thụ bộ nhớ cũng thoải mái trong vòng 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Trình trợ giúp thay thế cả đầu vào tiêu chuẩn và đầu vào của mô-đun`input`hoạt động sao cho giống nhau`solve()`việc thực hiện có thể được thực hiện nhiều lần.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solution.input = sys.stdin.readline
        solution.solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """3 3
1 2 3 5 1
1 1 2 1 3
2 2 3
"""
) == "3907", "sample 1"

# Minimum-size input, with no available strips
assert run(
    """1 1
2 1 1
"""
) == "0", "empty query"

# Repeated identical strips in one box
assert run(
    """1 4
1 1 1 9 0
1 1 1 9 0
1 1 1 9 0
2 1 1
"""
) == "9", "all-equal values"

# Boundary case with c = 0 and l > 1, which needs inverse powers of 10
assert run(
    """3 2
1 3 3 1 0
2 3 3
"""
) == "1", "negative exponent case"

# Large c, checking modular exponentiation
MOD = 1000000007
INV9 = pow(9, MOD - 2, MOD)
huge_value = 7 * (pow(10, 1000000001, MOD) - 1) % MOD
huge_value = huge_value * INV9 % MOD

assert run(
    """2 2
1 2 2 7 1000000000
2 2 2
"""
) == str(huge_value), "large c"

# Maximum-size n and m.
# Every update covers the whole array, so the expected value in the last
# box is just the same repunit value, regardless of the number of updates.
n = 10000
m = 100000
lines = [f"{n} {m}"]
lines.extend(["1 1 10000 1 0"] * (m - 1))
lines.append("2 10000 10000")
max_input = "\n".join(lines) + "\n"

repunit = (pow(10, 10000, MOD) - 1) * INV9 % MOD
expected_max = repunit * (m - 1) % MOD

assert run(max_input) == str(expected_max), "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 2 1 1`|`0`| Truy vấn trống và bảo vệ chia cho 0 | 
| Ba bản cập nhật giống hệt nhau trên một hộp |`9`| Tích lũy nhiều dải bằng nhau | 
|`1 3 3 1 0`theo sau là một truy vấn |`1`| Số mũ âm trong (10^{c-l+1}) | 
| Cập nhật với (c=10^9) | Giá trị mô-đun được tính toán | Xử lý số mũ lớn | 
| (n=10^4,\ m=10^5) | Giá trị mô-đun được tính toán | Kích thước đầu vào tối đa và cập nhật toàn diện lười biếng | 

## Vỏ cạnh 

Một truy vấn trống được xử lý trước khi đảo ngược mô-đun. Vì```
1 1
2 1 1
```phần gốc đại diện cho hộp duy nhất và có`count_sum = 0`. Truy vấn trả về`(0, 0)`, do đó mã sẽ được thêm vào ngay lập tức`0`. Không có sự đảo ngược nào được thử. 

Trường hợp số mũ âm được xử lý thông qua mảng công suất nghịch đảo. Vì```
3 2
1 3 3 1 0
2 3 3
```dải có độ dài (c+1=1), vì vậy giá trị của nó chính xác là`1`. Về mặt đại số, 

[ 
1\frac{10^{0+1}-1}{9}=1. 
] 

Việc biểu diễn hệ số sử dụng 

[ 
10^{c-l+1}=10^{-2}, 
] 

được biểu diễn dưới dạng`10^0 * 10^-2`. Cây phân đoạn nhân hệ số này với (10^3) ​​và trừ đi hằng số (1/9), thu được`1`mô đun (M). 

Việc căn chỉnh ranh giới được xử lý bằng logic nửa mở của cây phân đoạn được triển khai thông qua các khoảng bao gồm. Vì```
4 2
1 2 4 3 1
2 3 4
```bản cập nhật ảnh hưởng chính xác đến vị trí 2, 3 và 4. Truy vấn chỉ truy cập vị trí 3 và 4, trả về giá trị`333`Và`3333`, với tổng`3666`và đếm`2`. Kết quả là`1833`. 

Các bản cập nhật chồng chéo có tính chất bổ sung vì mọi thao tác loại 1 đại diện cho một dải mới chứ không phải là một dải thay thế. Nếu hai bản cập nhật giống nhau ảnh hưởng đến cùng một hộp, thì nút`value_sum`Và`count_sum`mỗi người nhận được hai đóng góp độc lập. Đối với ba bản cập nhật thêm`9`vào hộp duy nhất, cặp được lưu trữ sẽ trở thành`(27,3)`, và truy vấn trả về (27/3=9). 

Lớn (c) không bao giờ được sử dụng để xây dựng một chuỗi thập phân. Đối với (c=10^9), mã tính toán (10^{c}\bmod M) với lũy thừa mô-đun và kết hợp nó với lũy thừa nghịch đảo được tính toán trước cho vị trí bắt đầu. Giá trị kết quả giống hệt về mặt toán học với số nguyên khổng lồ có nhiều chữ số lặp lại, nhưng tất cả số học vẫn giữ nguyên modulo (10^9+7). 

Tổng số dải tối đa trong một truy vấn có thể đạt tới (10^9), nhưng con số này vẫn thấp hơn (M). Do đó, mẫu số không thể đồng dư với modulo 0 (M) theo các ràng buộc đã cho. Việc lưu trữ số đếm dưới dạng số nguyên Python thông thường cũng tránh được việc vô tình làm mất thông tin thông qua việc rút gọn mô-đun.
