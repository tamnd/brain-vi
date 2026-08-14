---
title: "CF 102354I - Từ mô-đun đến hợp lý"
description: "Đối với mọi trường hợp kiểm tra, giám khảo bí mật chọn một số hữu tỷ dương (x=p/q), trong đó cả (p) và (q) nhiều nhất là (10^9). Chúng ta không thể nhìn thấy (p) và (q) một cách trực tiếp."
date: "2026-08-14T12:28:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "I"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 160
verified: false
draft: false
---

[CF 102354I - Từ mô-đun đến hợp lý](https://codeforces.com/problemset/problem/102354/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đối với mọi trường hợp kiểm tra, giám khảo bí mật chọn một số hữu tỷ dương (x=p/q), trong đó cả (p) và (q) nhiều nhất là (10^9). Chúng ta không thể nhìn thấy (p) và (q) một cách trực tiếp. Thay vào đó, chúng ta có thể chọn một số nguyên tố lớn (m), yêu cầu giá trị của (p q^{-1}\pmod m) và sử dụng số dư được trả về để khôi phục số hữu tỉ. Câu trả lời có thể sử dụng tối đa bất kỳ tử số và mẫu số dương nào (10^9) đại diện cho cùng một giá trị hữu tỉ, vì vậy việc giảm phân số luôn được cho phép. Vấn đề có tính tương tác, nghĩa là chương trình phải in các truy vấn, xóa chúng, đọc câu trả lời của giám khảo và cuối cùng in phần đã tìm được. 

Giới hạn trên (10^9) của cả tử số và mẫu số là ràng buộc số quan trọng. Nó cho chúng ta biết rằng hai phân số rút gọn khác nhau thỏa mãn giới hạn có thể có tích chéo khác nhau nhiều nhất là (10^{18}). Mô-đun chỉ lớn hơn (10^9) một chút là không đủ để phân biệt chúng, do đó, một truy vấn nói chung không thể cung cấp đủ thông tin. Giới hạn truy vấn là 10 đủ lớn để kết hợp hai mô đun có tích lớn hơn nhiều so với (10^{18}). Số lượng trường hợp thử nghiệm có thể đạt tới (10^5), do đó tính toán cục bộ sau mỗi truy vấn phải theo logarit trong mô đun thay vì tỷ lệ với (10^9). Hai truy vấn và một phép tính Euclide mở rộng cho mỗi trường hợp thử nghiệm dễ dàng đủ nhỏ. 

Có một sự tinh tế khác do thực tế là phần đầu vào không cần phải giảm. Giả sử giá trị ẩn là (2/4). Giá trị toán học đúng là (1/2) và trả về`! 1 2`là hợp lệ. Thuật toán xây dựng lại chỉ tìm kiếm cặp ẩn chính xác (2,4) sẽ giải quyết được vấn đề mạnh hơn mức cần thiết và có thể tạo ra câu trả lời sai khi nó nhất quyết khôi phục biểu diễn ban đầu. 

Trường hợp cạnh thứ hai là (p=q=10^9). Phân số chính xác là (1), vì vậy câu trả lời đúng có thể là`! 1 1`, không nhất thiết`! 1000000000 1000000000`. Một thuật toán bất cẩn coi các giới hạn đã cho là yêu cầu tử số và mẫu số ban đầu sẽ phân biệt một cách không cần thiết các biểu diễn của cùng một số hữu tỷ. 

Trường hợp cạnh thứ ba xảy ra khi phần dư mô-đun rất lớn. Ví dụ: đối với (x=1/2) và số nguyên tố lẻ (m), số dư được trả về là ((m+1)/2), gần bằng (m/2), cao hơn nhiều (10^9). Do đó, việc cố gắng diễn giải giá trị được trả về là tử số không thành công. Tử số và mẫu số phải được xây dựng lại từ sự đồng đẳng (p\equiv rq\pmod M), thay vì đọc trực tiếp từ phần dư. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là truy vấn một mô đun và thử mọi mẫu số có thể có (q) từ (1) đến (10^9). Đối với mỗi (q), sự đồng dư xác định tử số ứng cử viên là (p=rq\bmod m). Nếu giá trị đó nằm giữa (1) và (10^9), chúng tôi đã tìm thấy biểu diễn hợp lệ. Điều này hiệu quả vì cặp ẩn được đảm bảo nằm trong số các ứng cử viên đó, nhưng trường hợp xấu nhất yêu cầu (10^9) phép nhân mô-đun cho một trường hợp thử nghiệm. Trên (10^5) trường hợp thử nghiệm trở thành (10^{14}) lần lặp, điều này hoàn toàn không thực tế. 

Quan sát quan trọng là một số câu trả lời theo mô-đun có thể được kết hợp. Hỏi hai số nguyên tố khác nhau (m_1,m_2), sau đó sử dụng Định lý số dư Trung Hoa để biến hai đáp án thành một thặng dư (r) modulo 

[ 
M=m_1m_2. 
] 

Chúng ta có thể chọn hai số nguyên tố sao cho (M>2\cdot 10^{18}). Các giá trị 

[ 
m_1=999999999989,\qquad m_2=1000000000039 
] 

đều là số nguyên tố và thỏa mãn phạm vi truy vấn được yêu cầu. Sản phẩm của họ có giá trị khoảng (10^{24}), vượt quá giới hạn yêu cầu một cách thoải mái. 

Bây giờ bài toán trở thành bài toán tái thiết hợp lý tiêu chuẩn. Chúng tôi biết 

[ 
r\equiv pq^{-1}\pmod M, 
] 

hoặc tương đương, 

[ 
rq\equiv p\pmod M. 
] 

Phân số ẩn rút gọn có (1\le p,q\le10^9). Nếu hai phân số rút gọn khác nhau (p_1/q_1) và (p_2/q_2) tạo ra cùng một dư lượng thì 

[ 
p_1q_2-p_2q_1\equiv0\pmod M. 
] 

Giá trị tuyệt đối của vế trái tối đa là (2\cdot10^{18}), trong khi (M) lớn hơn giá trị đó. Do đó, hiệu thực sự phải bằng 0, nên các phân số giống hệt nhau. Đây chính xác là điều kiện duy nhất đằng sau việc tái cấu trúc hợp lý, có thể được thực hiện bằng thuật toán Euclide mở rộng. 

Thuật toán Euclide đưa ra một chuỗi nhận dạng 

[ 
R_i=A_iM+B_ir. 
] 

Khi phần dư (R_i) lần đầu tiên lớn nhất là (10^9), hệ số tương ứng (B_i) là mẫu số của phân số được xây dựng lại, tối đa là dấu chung. Mô đun cố tình lớn hơn nhiều so với (2N^2), với (N=10^9), do đó tử số và mẫu số mong muốn phải xuất hiện tại điểm này trong dãy Euclide. 

Các phương pháp tiếp cận bạo lực và tối ưu có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^9)) mỗi trường hợp thử nghiệm | (O(1)) | Quá chậm | 
| CRT + tái thiết hợp lý | (O(\log M)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Yêu cầu modulo dư lượng ẩn (m_1=999999999989). Số này là số nguyên tố và nằm hoàn toàn giữa (10^9) và (10^{12}), vì vậy đây là một truy vấn pháp lý. 
2. Yêu cầu modulo dư (m_2=1000000000039). Sử dụng hai số nguyên tố phân biệt sẽ cho hai đồng dư mô tả cùng một số hữu tỉ. 
3. Kết hợp hai câu trả lời với Định lý số dư Trung Hoa. Nếu câu trả lời là (r_1) và (r_2), hãy viết 

[ 
r=r_1+m_1k. 
] 

Chúng ta chọn (k) sao cho (r\equiv r_2\pmod {m_2}). Như vậy 

[ 
k\equiv(r_2-r_1)m_1^{-1}\pmod {m_2}. 
] 

Kết quả (r) là modulo dư duy nhất (M=m_1m_2) đáp ứng cả hai truy vấn. 

1. Chạy thuật toán Euclide mở rộng trên (M) và (r), đồng thời theo dõi hệ số của (r). Ban đầu hai cặp liên quan là 

[ 
(M,0),\qquad(r,1). 
] 

Mỗi phép chuyển đổi Euclide đều bảo toàn sự đồng nhất về dạng 

[ 
R=AM+Br. 
]

1. Dừng lại ở phần dư Euclide đầu tiên (R) với (R\le10^9). Định lý tái thiết hợp lý nói rằng, bởi vì (M>2\cdot10^{18}), phần dư này và hệ số (r) của nó cho tử số và mẫu số rút gọn duy nhất trong giới hạn yêu cầu. 
2. Làm cho hệ số dương nếu cần thiết bằng cách đổi cả hai dấu. Trong việc tái cấu trúc dương hợp lệ cho vấn đề này, cặp cuối cùng là dương, nhưng việc chuẩn hóa dấu sẽ làm cho việc triển khai trở nên mạnh mẽ. 
3. In tử số và mẫu số được xây dựng lại làm đáp án cuối cùng. Phân số được xây dựng lại có cùng giá trị hữu tỷ với phân số ẩn, ngay cả khi ban đầu giám khảo chọn một biểu diễn không rút gọn. 

Bất biến trong suốt pha Euclide là mọi phần dư hiện tại (R) đều có một biểu diễn đã biết (R=AM+Br). Vì (r\equiv pq^{-1}\pmod M), nhân phương trình sau với (q) sẽ được (rq\equiv p\pmod M). Do đó, cặp ẩn chính là một trong những nghiệm nhỏ được biểu diễn trong mạng Euclide này. Giới hạn (M>2N^2) làm cho nghiệm rút gọn nhỏ đó là duy nhất, vì vậy khi Euclid đạt đến phần dư đủ nhỏ đầu tiên, nó không thể là một phân số hợp lệ khác. 

## Giải pháp Python 

Nhiệm vụ ban đầu mang tính tương tác, vì vậy chương trình bên dưới là giải pháp tương tác thực tế. Mẫu hiển thị trong câu lệnh là bản ghi tương tác chứ không phải đầu vào thông thường, do đó, mẫu này không thể được thực thi dưới dạng kiểm tra hàng loạt stdin-to-stdout thông thường.```python
import sys
input = sys.stdin.readline

P1 = 999999999989
P2 = 1000000000039
N = 10**9

def extended_gcd(a, b):
    old_r, r = a, b
    old_s, s = 1, 0
    old_t, t = 0, 1

    while r:
        q = old_r // r
        old_r, r = r, old_r - q * r
        old_s, s = s, old_s - q * s
        old_t, t = t, old_t - q * t

    return old_r, old_s, old_t

def crt(r1, r2):
    # r = r1 + P1 * k
    # P1 * k == r2 - r1 (mod P2)
    _, inv, _ = extended_gcd(P1, P2)
    inv %= P2

    k = ((r2 - r1) % P2) * inv % P2
    return r1 + P1 * k

def rational_reconstruct(r, mod):
    # Euclidean sequence:
    # remainder = A * mod + B * r
    old_r, cur_r = mod, r
    old_b, cur_b = 0, 1

    while cur_r > N:
        q = old_r // cur_r

        old_r, cur_r = cur_r, old_r - q * cur_r
        old_b, cur_b = cur_b, old_b - q * cur_b

    numerator = cur_r
    denominator = cur_b

    if denominator < 0:
        numerator = -numerator
        denominator = -denominator

    return numerator, denominator

def ask(m):
    print("?", m, flush=True)
    ans = int(input())

    if ans == -1:
        sys.exit(0)

    return ans

def main():
    t = int(input())

    for _ in range(t):
        r1 = ask(P1)
        r2 = ask(P2)

        r = crt(r1, r2)
        mod = P1 * P2

        p, q = rational_reconstruct(r, mod)

        print("!", p, q, flush=True)

if __name__ == "__main__":
    main()
```Các hằng số được cố định trước khi xử lý bất kỳ trường hợp thử nghiệm nào. Cả hai đều là số nguyên tố hợp lệ trong khoảng cho phép, do đó không cần phải tốn công truy vấn hoặc tính toán để tìm kiếm số nguyên tố. Tích của họ lớn hơn (2\cdot10^{18}), đây là điều kiện kích thước duy nhất cần thiết cho việc tái cấu trúc hợp lý. 

các`ask`hàm in truy vấn và ngay lập tức xóa thiết bị xuất chuẩn. Flushing là bắt buộc trong một bài toán tương tác vì trọng tài không thể trả lời câu hỏi mà họ chưa nhận được. Một câu trả lời của`-1`thông thường có nghĩa là sự tương tác đã thất bại, do đó chương trình sẽ kết thúc ngay lập tức thay vì gửi thêm đầu ra. 

các`crt`hàm tuân theo công thức CRT hai mô đun trực tiếp. Thuật toán Euclide mở rộng đưa ra nghịch đảo của (P_1) modulo (P_2) và phép nhân được rút gọn modulo (P_2) trước khi xây dựng phần dư cuối cùng. Số nguyên Python có độ chính xác tùy ý, điều này rất hữu ích vì (P_1P_2) nằm trong khoảng (10^{24}), vượt xa phạm vi 64-bit đã ký. 

các`rational_reconstruct`hàm chỉ theo dõi hệ số của phần dư được truy vấn. Không cần phải giữ lại hệ số của mô đun. Khi`cur_r`đầu tiên tối đa là (10^9), hệ số tương ứng là mẫu số và phần còn lại là tử số. Điều kiện dừng sử dụng`>`còn hơn là`>=`, vì phần dư bằng (10^9) đã là tử số hợp lệ và phải được chấp nhận. 

Thuật toán không bao giờ chia cho mẫu số ban đầu modulo mô đun kết hợp. Mẫu số tối đa là (10^9), trong khi cả hai số nguyên tố được truy vấn đều lớn hơn (10^9), do đó, nó tự động đảo ngược modulo cho mỗi số nguyên tố. Đây là điều làm cho việc biểu diễn mô-đun của số hữu tỉ được xác định rõ ràng. 

## Ví dụ đã hoạt động 

Mẫu của câu lệnh sử dụng một mô-đun và thể hiện ba giá trị ẩn, (1), (1/2) và (2). Thay vào đó, quá trình triển khai của chúng tôi yêu cầu hai số nguyên tố lớn hơn, nhưng giai đoạn tái thiết hoạt động theo cách giống hệt nhau. 

Đối với giá trị mẫu đầu tiên (x=1), cả hai câu trả lời đều là (1). 

| Bước | (r_1) | (r_2) | Dư lượng CRT (r) | Phần còn lại Euclide | Hệ số (r) | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Truy vấn 1 | 1 | | | | | | 
| Truy vấn 2 | 1 | 1 | | | | | 
| CRT | 1 | 1 | 1 | | | | 
| Tái thiết | | | 1 | 1 | 1 | (1/1) | 

Thuật toán Euclide đạt ngay (1) và hệ số tương ứng là (1). Đầu ra`! 1 1`đại diện cho cùng một giá trị với số ẩn. 

Đối với giá trị mẫu thứ hai (x=1/2), nghịch đảo của (2) modulo một số nguyên tố lẻ (m) là ((m+1)/2). Do đó, hai câu trả lời là (499999999995) và (500000000020). 

| Bước | (r_1) | (r_2) | Dư lượng CRT (r) | Phần còn lại Euclide | Hệ số (r) | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Truy vấn 1 | 499999999995 | | | | | | 
| Truy vấn 2 | 499999999995 | 500000000020 | | | | | 
| CRT | 499999999995 | 500000000020 | ((M+1)/2) | | | | 
| Euclid 1 | | | | ((M-1)/2) | (-1) | | 
| Euclid 2 | | | | 1 | 2 | (1/2) | 

Phần dư mô-đun lớn không bị nhầm lẫn với tử số lớn. Euclid chuyển đổi thông tin mô-đun thành cặp nhỏ (1,2), điều này chứng tỏ tại sao việc tái cấu trúc hợp lý là sự trừu tượng hóa chính xác. 

Là ví dụ tùy chỉnh thứ hai, hãy xem xét biểu diễn ẩn (10^9/10^9). Giá trị của nó là (1), do đó mọi truy vấn đều trả về phần dư (1), chính xác như trong dấu vết đầu tiên. Lợi nhuận tái thiết hợp lý (1/1). 

| Bước | Đại diện ẩn | (r_1) | (r_2) | Tử số được xây dựng lại | Mẫu số được xây dựng lại | 
| --- | --- | --- | --- | --- | --- | 
| Truy vấn | (1000000000/1000000000) | 1 | 1 | | | 
| CRT | (1) | 1 | 1 | | | 
| Tái thiết | (1) | | | 1 | 1 | 

Dấu vết này thực hiện tử số và mẫu số tối đa cho phép đồng thời xác nhận rằng câu trả lời được phép giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log M)) cho mỗi trường hợp thử nghiệm | CRT sử dụng một số lượng không đổi các phép toán Euclide mở rộng và việc tái cấu trúc hợp lý thực hiện một thuật toán Euclide | 
| Không gian | (O(1)) | Chỉ một số nguyên không đổi được lưu trữ | 

Với (M\approx10^{24}), thuật toán Euclide chỉ cần vài chục phép chia số nguyên cho mỗi trường hợp thử nghiệm. Ngay cả đối với (10^5) trường hợp kiểm thử, số bước số học rất nhỏ so với số lần lặp (10^{14}) mà phép liệt kê mẫu số yêu cầu. Các số nguyên có độ chính xác tùy ý của Python cũng xử lý trực tiếp mô-đun CRT có tỷ lệ (10^{24}), do đó không có vấn đề tràn. 

## Trường hợp thử nghiệm 

Vì đây là tính tương tác nên đầu vào mẫu bằng chữ không thể được đưa vào giải pháp dưới dạng chuỗi thông thường. Khai thác ngoại tuyến sau đây kiểm tra cốt lõi tái thiết xác định bằng cách mô phỏng hai câu trả lời mô-đun của thẩm phán từ một phần đã biết. Ba phần từ mẫu được cung cấp sẽ được đưa vào làm thử nghiệm đầu tiên.```python
import sys
import io
from math import gcd

P1 = 999999999989
P2 = 1000000000039
N = 10**9

def extended_gcd(a, b):
    old_r, r = a, b
    old_s, s = 1, 0
    old_t, t = 0, 1

    while r:
        q = old_r // r
        old_r, r = r, old_r - q * r
        old_s, s = s, old_s - q * s
        old_t, t = t, old_t - q * t

    return old_r, old_s, old_t

def crt(r1, r2):
    _, inv, _ = extended_gcd(P1, P2)
    inv %= P2
    k = ((r2 - r1) % P2) * inv % P2
    return r1 + P1 * k

def rational_reconstruct(r, mod):
    old_r, cur_r = mod, r
    old_b, cur_b = 0, 1

    while cur_r > N:
        q = old_r // cur_r
        old_r, cur_r = cur_r, old_r - q * cur_r
        old_b, cur_b = cur_b, old_b - q * cur_b

    p, q = cur_r, cur_b

    if q < 0:
        p = -p
        q = -q

    return p, q

def solve_fraction(p, q):
    # Simulate the two interactive replies.
    r1 = (p * pow(q, -1, P1)) % P1
    r2 = (p * pow(q, -1, P2)) % P2

    r = crt(r1, r2)
    return rational_reconstruct(r, P1 * P2)

def run(inp: str) -> str:
    out = []

    for line in inp.strip().splitlines():
        if not line.strip():
            continue

        p, q = map(int, line.split())
        a, b = solve_fraction(p, q)
        out.append(f"{a} {b}")

    return "\n".join(out) + "\n"

# Provided sample values: 1, 1/2, and 2.
assert run("""\
1 1
1 2
2 1
""") == """\
1 1
1 2
2 1
""", "provided sample values"

# Non-reduced representation. The correct rational value is 1/2.
assert run("""\
2 4
""") == """\
1 2
""", "non-reduced fraction"

# Minimum-size fraction.
assert run("""\
1 1
""") == """\
1 1
""", "minimum-size values"

# Maximum-size numerator and denominator. The value is exactly 1.
assert run("""\
1000000000 1000000000
""") == """\
1 1
""", "maximum-size equal values"

# Boundary numerator and denominator.
assert run("""\
1000000000 999999999
""") == """\
1000000000 999999999
""", "values at the upper bound")

# A fraction whose reduced denominator is large.
assert run("""\
999999999 1000000000
""") == """\
999999999 1000000000
""", "large reduced denominator")

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`,`1 2`,`2 1`|`1 1`,`1 2`,`2 1`| Các giá trị được biểu thị trong mẫu tương tác được cung cấp | 
|`2 4`|`1 2`| Biểu diễn đầu vào không giảm | 
|`1 1`|`1 1`| Giá trị tối thiểu được phép | 
|`1000000000 1000000000`|`1 1`| Giá trị tối đa và mức giảm | 
|`1000000000 999999999`|`1000000000 999999999`| Tử số giới hạn trên | 
|`999999999 1000000000`|`999999999 1000000000`| Mẫu số rút gọn lớn | 

## Vỏ cạnh 

Đối với trường hợp không rút gọn, hãy xem xét đầu vào chính xác`2 4`trong khai thác ngoại tuyến. Cả hai số nguyên tố đều có cùng giá trị mô-đun là (1/2), bởi vì 

[ 
2\cdot4^{-1}\equiv1\cdot2^{-1}\pmod m. 
] 

CRT kết hợp hai quan sát vào phần dư của (1/2) và trả về kết quả tái thiết hợp lý`1 2`. Điều này đúng vì đầu ra được yêu cầu là giá trị hợp lý, không phải mã hóa gốc. 

Đối với trường hợp bằng nhau nhất, hãy xem xét`1000000000 1000000000`. Cả hai câu trả lời mô-đun đều là (1), do đó CRT cho (r=1). Phần còn lại Euclide đầu tiên đã thỏa mãn giới hạn (10^9), với hệ số (1), cho`1 1`. Một giải pháp cố gắng bảo toàn biểu diễn ẩn sẽ trả về cặp không rút gọn một cách không cần thiết, nhưng thẩm phán chấp nhận bất kỳ biểu diễn hợp lệ nào của giá trị. 

Đối với (x=1/2), các câu trả lời mô-đun nằm trong khoảng (5\cdot10^{11}), mặc dù cả tử số và mẫu số nhiều nhất là (10^9). Dãy Euclide cuối cùng đạt đến phần dư (1) với hệ số (2). Điều này mắc phải lỗi phổ biến khi cho rằng giá trị mô-đun được trả về phải là tử số. 

Đối với phân số giới hạn trên`1000000000 999999999`, tử số nằm đúng giới hạn cho phép. Mô đun kết hợp vẫn lớn hơn nhiều so với mọi tích chéo có liên quan, do đó định lý tái thiết hợp lý được áp dụng mà không cần bất kỳ xử lý đặc biệt nào đối với đẳng thức ở giới hạn. Thuật toán trả về cùng một cặp rút gọn, xác nhận rằng điều kiện dừng phải chấp nhận phần dư bằng (10^9), không chỉ nhỏ hơn nó một phần. 

Ý tưởng cơ bản là dành hai trong số mười truy vấn có sẵn để tạo mô-đun xung quanh (10^{24}), sau đó ngừng suy nghĩ về sự tương tác và giải một bài toán số học thuần túy. CRT cung cấp cho một modulo đồng dư một mô đun đủ lớn và thuật toán Euclide mở rộng biến sự đồng dư đó trở lại thành số hữu tỷ nhỏ duy nhất.
