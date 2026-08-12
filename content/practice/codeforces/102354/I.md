---
title: "CF 102354I - Từ mô-đun đến hợp lý"
description: "Đối với mỗi trường hợp kiểm thử, giám khảo đã chọn một số hữu tỉ dương [ x=frac{p}{q}, qquad 1le p,qle 10^9. ] Chúng ta không thể nhìn thấy (p) và (q) một cách trực tiếp. Thay vào đó, đối với mô đun nguyên tố (m) nằm giữa (10^9) và (10^{12}), chúng ta có thể yêu cầu phần dư [ yequiv p q^{-1}pmod m."
date: "2026-08-13T00:38:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "I"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 231
verified: false
draft: false
---

[CF 102354I - Từ mô-đun đến hợp lý](https://codeforces.com/problemset/problem/102354/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 51s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp kiểm thử, giám khảo đã chọn một số hữu tỷ dương 

[ 
x=\frac{p}{q}, 
\qquad 1\le p,q\le 10^9. 
] 

Chúng ta không thể nhìn thấy (p) và (q) một cách trực tiếp. Thay vào đó, đối với mô đun nguyên tố (m) nằm giữa (10^9) và (10^{12}), chúng ta có thể yêu cầu phần dư 

[ 
y\equiv p q^{-1}\pmod m. 
] 

Vì (m>10^9\ge q) nên nghịch đảo (q^{-1}\pmod m) luôn tồn tại. Nhiệm vụ của chúng ta là khôi phục bất kỳ cặp (P,Q) nào có (1\le P,Q\le10^9) đại diện cho cùng một giá trị hữu tỷ. Cặp này không cần phải giảm, do đó trả về (1/2) thay vì (2/4) đều có giá trị như nhau. Vấn đề ban đầu mang tính tương tác, do đó mẫu được hiển thị là cuộc đối thoại giữa chương trình và giám khảo chứ không phải là một cặp đầu vào/đầu ra cố định thông thường. 

Các giới hạn được thiết kế xoay quanh việc tái cấu trúc theo lý thuyết số thay vì liệt kê. Có thể có (10^5) trường hợp thử nghiệm, trong khi bản thân truy vấn chỉ cung cấp giá trị mô-đun. Việc thử tất cả (10^9) mẫu số có thể có cho mỗi trường hợp thử nghiệm sẽ yêu cầu tối đa (10^{14}) lần lặp, vượt xa giới hạn sáu giây. Thực tế hữu ích là mô đun cho phép có thể gần bằng (10^{12}), vì vậy hai truy vấn có thể cho chúng ta mô đun kết hợp khoảng (10^{24}). Nó lớn hơn rất nhiều so với tích có tỷ lệ (10^{18}) của tử số và mẫu số chưa biết. 

Có một số trường hợp việc triển khai đơn giản có thể âm thầm thất bại. Đầu tiên, một mô đun không nhất thiết xác định giá trị hợp lý. Với (m=10^9+7), hai số hữu tỷ khác nhau 

[ 
\frac{600000000}{1} 
\quad\text{và}\quad 
\frac{199999993}{2} 
] 

tạo ra dư lượng tương tự, bởi vì 

[ 
199999993\equiv 2\cdot600000000\pmod {10^9+7}. 
] 

Cả tử số và mẫu số đều thỏa mãn các giới hạn cần thiết, do đó, giả sử rằng một mô đun đủ lớn là đủ là không chính xác. 

Thứ hai, phần đầu vào không cần phải giảm. Mẫu chứa (2/4), có cùng giá trị với (1/2). Một cách thực hiện nhất quyết đòi khôi phục chính xác (p) và (q) ban đầu đang giải quyết một vấn đề mạnh hơn thẩm phán yêu cầu. Tái thiết hợp lý tự nhiên trả về biểu diễn đã giảm, điều này có thể chấp nhận được. 

Thứ ba, không nên từ chối một giá trị biên như (p=q=10^9) chỉ vì phân số rút gọn là (1/1). Đại diện ban đầu là hợp pháp và câu trả lời bắt buộc là bất kỳ đại diện pháp lý nào có cùng giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ chọn một mô đun, lấy phần dư (r) của nó và thử mọi mẫu số (q) từ (1) đến (10^9). Với mỗi ứng viên, chúng ta có thể tính toán 

[ 
p=(rq)\bmod m 
] 

và chấp nhận cặp if (1\le p\le10^9). Điều này đúng bất cứ khi nào nó tìm thấy một cặp hợp lệ vì phép kiểm tra chính xác là sự phù hợp do thẩm phán đưa ra. Vấn đề là số lần lặp lại. Trong trường hợp xấu nhất, một thử nghiệm yêu cầu (10^9) phép nhân mô-đun và (10^5) thử nghiệm sẽ cho (10^{14}) lần lặp. Thậm chí bỏ qua chi phí tương tác, điều đó là vô vọng. 

Quan sát quan trọng là truy vấn đưa ra sự đồng dư có dạng chính xác được sử dụng trong quá trình tái cấu trúc hợp lý: 

[ 
p\equiv rq\pmod M. 
] 

Nếu chúng ta có thể làm cho (M) lớn hơn (2\cdot10^9\cdot10^9=2\cdot10^{18}), thì có thể có nhiều nhất một số hữu tỉ rút gọn có tử số và mẫu số giới hạn bởi (10^9) thỏa mãn sự đồng dạng này. Thuật toán tái cấu trúc hợp lý tiêu chuẩn tìm thấy cặp đó bằng thuật toán Euclide mở rộng. Mối liên hệ của nó với Euclid ở đây đặc biệt tự nhiên vì phương trình mong muốn có thể được viết lại thành 

[ 
p=rq-kM 
] 

đối với một số nguyên (k). Do đó (p) và (q) xuất hiện dưới dạng phần dư nhỏ và hệ số tương ứng của nó trong thuật toán Euclide.

Một mô đun được phép là quá nhỏ so với giới hạn duy nhất. Tuy nhiên, hai số nguyên tố gần (10^{12}) sẽ cho tích gần (10^{24}). Đầu tiên chúng ta yêu cầu phần dư modulo của mỗi số nguyên tố và kết hợp hai phần dư đó với định lý phần dư Trung Hoa. Mô-đun thu được thỏa mãn sự tái thiết hợp lý bị ràng buộc bởi một biên độ rất lớn. Các ràng buộc vấn đề chính thức cho phép tối đa mười truy vấn, vì vậy việc sử dụng hai truy vấn là thoải mái trong giới hạn. 

Hai số nguyên tố cố định được sử dụng dưới đây là (999999999989) và (999999999997). Cả hai đều ở dưới (10^{12}), trên (10^9) và nguyên tố. Số đầu tiên là số nguyên tố lớn nhất có mười hai chữ số và số thứ hai là số nguyên tố đã biết trước đó gần (10^{12}). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(t\cdot10^9)) | (O(1)) | Quá chậm | 
| Hai truy vấn + CRT + tái thiết hợp lý | (O(t\log 10^{12})) bước số học | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hỏi thẩm phán về modulo dư (m_1=999999999989). Lưu trữ câu trả lời dưới dạng (r_1). Chúng tôi sử dụng số nguyên tố lớn hơn mọi mẫu số có thể có, do đó việc chia cho (q) modulo (m_1) được xác định rõ ràng. 
2. Yêu cầu modulo dư (m_2=999999999997), lưu trữ dưới dạng (r_2). Hai môđun này là các số nguyên tố riêng biệt nên chúng nguyên tố cùng nhau và có thể kết hợp với định lý số dư Trung Hoa. 
3. Xây dựng (R) duy nhất thỏa mãn (0\le R<M=m_1m_2) 

[ 
R\equiv r_1\pmod {m_1}, 
\qquad 
R\equiv r_2\pmod {m_2}. 
] 

Một công thức thuận tiện là 

[ 
R=r_1+m_1 
\left((r_2-r_1)m_1^{-1}\bmod m_2\right). 
] 

Điểm quan trọng là hai đồng dư ban đầu bây giờ được biểu diễn bằng một modulo đồng dư có môđun lớn hơn nhiều (M). 

1. Chạy thuật toán Euclide mở rộng trên (M) và (R). Duy trì hệ số (t) thỏa mãn 

[ 
\text{phần dư}=sM+tR. 
] 

Khi số dư lớn nhất là (10^9) thì dừng lại. Việc xây dựng lại hợp lý đảm bảo rằng hệ số tương ứng (t), sau khi thay đổi cả hai dấu nếu cần, là mẫu số của phân số rút gọn mong muốn, miễn là nó lớn nhất là (10^9). Thuật toán tái thiết hợp lý tiêu chuẩn chính xác là một thủ tục Euclide mở rộng với giới hạn tử số và mẫu số. 

1. Đặt cặp dương thu được là ((P,Q)). Xác minh về mặt khái niệm rằng 

[ 
P\equiv RQ\pmod M. 
] 

Vì (M>2\cdot10^{18}), không thể có hai phân số rút gọn khác nhau với cả tử số và mẫu số nhiều nhất là (10^9) thỏa mãn sự đồng dạng này. Do đó, phân số được xây dựng lại có cùng số hữu tỷ với phân số ẩn. 

1. Đầu ra`! P Q`. Cặp được xây dựng lại có thể được rút gọn ngay cả khi ban đầu trọng tài chọn một cặp không rút gọn, điều này được bài toán cho phép rõ ràng. 

### Tại sao nó hoạt động 

Giả sử hai phân số rút gọn (p/q) và (a/b), với tối đa cả bốn giá trị (10^9), tạo ra cùng một modulo dư (M). Sau đó 

[ 
pq^{-1}\equiv ab^{-1}\pmod M 
] 

ngụ ý 

[ 
pb-aq\equiv0\pmod M. 
] 

Nhưng 

[ 
|pb-aq| 
\le pb+aq 
\le2\cdot10^{18} 
<M. 
] 

Bội số duy nhất của (M) có giá trị tuyệt đối nhỏ hơn (M) là 0, do đó (pb=aq). Vì cả hai phân số đều giảm nên chúng bằng nhau. Việc xây dựng lại hợp lý tìm thấy cặp nhỏ tương ứng với mối quan hệ Euclide, vì vậy nó phải là phân số duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

M1 = 999999999989
M2 = 999999999997
LIMIT = 10**9

def crt(r1, r2):
    # R = r1 + M1 * k
    # R == r2 (mod M2)
    # M1 * k == r2-r1 (mod M2)
    inv = pow(M1, -1, M2)
    k = ((r2 - r1) * inv) % M2
    return r1 + M1 * k

def rational_reconstruct(r, mod):
    # We maintain:
    # rem = s * mod + t * r
    rem0, rem1 = mod, r
    t0, t1 = 0, 1

    while rem1 > LIMIT:
        q = rem0 // rem1
        rem0, rem1 = rem1, rem0 - q * rem1
        t0, t1 = t1, t0 - q * t1

    if t1 < 0:
        rem1 = -rem1
        t1 = -t1

    # The problem guarantees that a valid reconstruction exists.
    assert 1 <= rem1 <= LIMIT
    assert 1 <= t1 <= LIMIT
    assert (r * t1 - rem1) % mod == 0

    return rem1, t1

def ask(m):
    print("?", m, flush=True)
    return int(input())

def main():
    t = int(input())

    for _ in range(t):
        r1 = ask(M1)
        r2 = ask(M2)

        r = crt(r1, r2)
        p, q = rational_reconstruct(r, M1 * M2)

        print("!", p, q, flush=True)

if __name__ == "__main__":
    main()
```Hai hằng số ở trên cùng là các mô đun truy vấn pháp lý cố định. Việc sử dụng các số nguyên tố cố định sẽ tránh được việc tốn nhiều truy vấn hoặc tính toán khi kiểm tra tính nguyên tố trong quá trình tương tác. 

các`crt`hàm thực hiện trực tiếp định lý phần dư Trung Hoa. Chúng ta viết (R=r_1+m_1k), thay nó vào đồng dư thứ hai, và giải một đồng dư tuyến tính cho (k). Số nguyên Python có độ chính xác tùy ý, do đó tích (m_1m_2), ở khoảng (10^{24}), không bị tràn. 

các`rational_reconstruct`hàm là phần Euclide mở rộng. Ban đầu hai số dư là (M) và (R), có hệ số (0) và (1) của (R). Sau mỗi phép chia Euclide, hệ số được cập nhật bằng thương số chính xác như phần còn lại. Khi phần dư đầu tiên giảm xuống tối đa (10^9), việc xây dựng lại hợp lý cho chúng ta biết rằng phần dư này và hệ số của nó mã hóa tử số và mẫu số mong muốn. 

Việc hiệu chỉnh dấu có ý nghĩa quan trọng vì các hệ số Euclid thay đổi dấu. Nếu hệ số mẫu số âm thì cả hai giá trị đều bị phủ định cùng nhau. Xác nhận cuối cùng kiểm tra sự phù hợp được xác định và cũng phát hiện lỗi triển khai trong quá trình thử nghiệm cục bộ. 

Các cuộc gọi truy vấn sử dụng`flush=True`. Không cần xóa, chương trình có thể chờ phản hồi của người đánh giá trong khi người đánh giá đang chờ truy vấn vẫn được lưu vào bộ đệm trong luồng đầu ra. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là bản ghi tương tác cho mô-đun (10^9+7). Giải pháp của chúng tôi có chủ ý sử dụng hai số nguyên tố khác nhau, do đó, các dòng truy vấn và phần dư thực tế của nó khác với bản ghi, trong khi các giá trị hợp lý được khôi phục là như nhau. Ba giá trị ẩn của mẫu là (1/1), (1/2) và (2/1). 

Đối với (1/1), mọi dư lượng được truy vấn là (1). 

| Giá trị ẩn | (r_1) | (r_2) | Tái Cấu Trúc (P) | Tái Tạo (Q) | 
| --- | --- | --- | --- | --- | 
| (1/1) | 1 | 1 | 1 | 1 | 

Đối với (1/2), phần dư là nghịch đảo mô đun của (2). Vì cả hai số nguyên tố được chọn đều là số lẻ nên chúng là ((m+1)/2). Sau CRT, cặn kết hợp là 

[ 
R=\frac{M+1}{2} 
=4999999999930000000000017. 
] 

Thuật toán Euclide cuối cùng đạt được mối quan hệ 

[ 
1=2R-M, 
] 

vậy cặp dương nhỏ là (P=1,Q=2). 

| Giá trị ẩn | (R) sau CRT | Phần còn lại nhỏ | Hệ số | Đầu ra | 
| --- | --- | --- | --- | --- | 
| (1/2) | 4999999999930000000000017 | 1 | 2 | (1/2) | 

Đối với (2/1), số dư tổng hợp chỉ đơn giản là (2). Tối đa nó đã là (10^9) và hệ số (R) của nó là (1), do đó quá trình tái thiết dừng ngay lập tức. 

| Giá trị ẩn | (R) sau CRT | Phần còn lại nhỏ | Hệ số | Đầu ra | 
| --- | --- | --- | --- | --- | 
| (2/1) | 2 | 2 | 1 | (2/1) | 

Những dấu vết này chứng minh tại sao thuật toán cũng xử lý các tử số và mẫu số rất nhỏ mà không có trường hợp đặc biệt nào. Quá trình Euclide hoặc tìm ra câu trả lời ngay lập tức hoặc đạt được nó chỉ sau một số phép chia logarit. 

Như một ví dụ thứ hai, hãy xem xét phân số biên 

[ 
x=\frac{10^9}{10^9}=1. 
] 

Cả hai câu trả lời theo mô-đun đều là (1), do đó CRT cho (R=1). Thuật toán trả về (1/1), đây là một câu trả lời hợp lệ mặc dù nó không giống với cách trình bày mà thẩm phán có thể đã sử dụng. 

| Giá trị ẩn | (r_1) | (r_2) | (R) | Đầu ra | 
| --- | --- | --- | --- | --- | 
| (10^9/10^9) | 1 | 1 | 1 | (1/1) | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(t\log M)) | Mỗi thử nghiệm thực hiện CRT cộng với một thuật toán Euclide mở rộng trên các số nguyên tối đa khoảng (10^{24}). | 
| Không gian | (O(1)) | Chỉ một số lượng số nguyên lớn không đổi được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Ở đây (M=m_1m_2<10^{24}), do đó, thuật toán Euclide chỉ mất vài chục lần lặp cho mỗi trường hợp thử nghiệm. Ngay cả với (10^5) trường hợp thử nghiệm, công việc số học vẫn nhỏ so với chính sự tương tác. Các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ vấn đề tràn do mô-đun CRT tỷ lệ (10^{24}) gây ra. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề ban đầu có tính tương tác nên mẫu chữ không thể được chuyển sang mẫu thông thường.`run(input_string)`chức năng: các số hiển thị bên dưới đầu vào là phản hồi của đánh giá, không phải là định dạng đầu vào ngoại tuyến hoàn chỉnh. Khai thác thử nghiệm sau đây kiểm tra cốt lõi toán học hoàn chỉnh bằng cách tạo ra hai dư lượng hợp pháp từ các giá trị ẩn (p,q) và đưa chúng vào quy trình xây dựng lại.```python
import sys
import io
from math import gcd

M1 = 999999999989
M2 = 999999999997
LIMIT = 10**9

def crt(r1, r2):
    inv = pow(M1, -1, M2)
    k = ((r2 - r1) * inv) % M2
    return r1 + M1 * k

def rational_reconstruct(r, mod):
    rem0, rem1 = mod, r
    t0, t1 = 0, 1

    while rem1 > LIMIT:
        q = rem0 // rem1
        rem0, rem1 = rem1, rem0 - q * rem1
        t0, t1 = t1, t0 - q * t1

    if t1 < 0:
        rem1 = -rem1
        t1 = -t1

    assert 1 <= rem1 <= LIMIT
    assert 1 <= t1 <= LIMIT
    assert (r * t1 - rem1) % mod == 0

    return rem1, t1

def reconstruct(p, q):
    r1 = p * pow(q, -1, M1) % M1
    r2 = p * pow(q, -1, M2) % M2
    r = crt(r1, r2)
    return rational_reconstruct(r, M1 * M2)

def run(cases):
    out = []
    for p, q in cases:
        a, b = reconstruct(p, q)
        g = gcd(p, q)
        expected = (p // g, q // g)
        out.append((a, b))
        assert (a, b) == expected
    return out

# Provided sample values.
assert run([
    (1, 1),
    (1, 2),
    (2, 1),
]) == [
    (1, 1),
    (1, 2),
    (2, 1),
], "sample 1"

# Minimum-size values.
assert run([
    (1, 1),
]) == [
    (1, 1),
], "minimum values"

# Maximum numerator and denominator, with a reduced answer.
assert run([
    (10**9, 10**9),
]) == [
    (1, 1),
], "maximum equal values"

# Opposite boundaries.
assert run([
    (1, 10**9),
    (10**9, 1),
]) == [
    (1, 10**9),
    (10**9, 1),
], "boundary values"

# Non-reduced representation and large coprime values.
assert run([
    (999999999, 999999999),
    (999999937, 1000000000),
]) == [
    (1, 1),
    (999999937, 1000000000),
], "reduction and large values"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (1/1,\ 1/2,\ 2/1) | (1/1,\ 1/2,\ 2/1) | Các giá trị được thể hiện trong mẫu được cung cấp | 
| (1/1) | (1/1) | Giá trị tối thiểu và tái thiết ngay lập tức | 
| (10^9/10^9) | (1/1) | Giá trị bằng nhau tối đa và mức giảm | 
| (1/10^9,\ 10^9/1) | Phân số giảm tương tự | Cả ranh giới mẫu số và tử số | 
| (999999999/999999999,\ 999999937/10^9) | (1/1,\ 999999937/10^9) | Đầu vào không giảm và giá trị đồng nguyên tố lớn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là lỗi của một mô đun đơn. Với (m=10^9+7), các phân số (600000000/1) và (199999993/2) cho cùng một dư lượng. Do đó, việc triển khai một truy vấn không có cơ sở toán học để lựa chọn giữa chúng. Giải pháp hai truy vấn kết hợp các phần dư độc lập thành một mô đun khoảng (10^{24}), sau đó bất đẳng thức duy nhất 

[ 
2\cdot10^9\cdot10^9<M 
] 

loại trừ cả hai phân số là các bản dựng lại hợp lệ cùng một lúc. 

Trường hợp cạnh thứ hai là phân số ẩn không rút gọn. Đối với giá trị ẩn (2/4), mỗi phản hồi mô-đun giống hệt với phản hồi cho (1/2). Do đó CRT tái tạo lại cặp đã giảm (1/2). Điều này đúng vì kết quả đầu ra được yêu cầu mô tả giá trị hợp lý chứ không phải biểu diễn chính xác được giám khảo lựa chọn ban đầu. 

Trường hợp cạnh thứ ba là (p=q=10^9). Phân số ẩn là số (1) và phần dư mô đun của nó là (1) với mọi số nguyên tố được phép. CRT trả về (R=1), do đó việc tái cấu trúc hợp lý trả về (1/1). Thực tế là cả tử số và mẫu số ban đầu đều ở mức tối đa không yêu cầu bất kỳ xử lý đặc biệt nào. 

Trường hợp cạnh thứ tư là mẫu số chính xác (10^9), chẳng hạn như (1/10^9). Điều kiện tái thiết sử dụng`<= LIMIT`, không`< LIMIT`, bởi vì bài toán cho phép chính ranh giới đó. Điều tương tự cũng áp dụng cho tử số chính xác (10^9). 

Trường hợp cạnh thứ năm là dấu của hệ số Euclide. Trong Euclid mở rộng, hệ số liên quan đến thặng dư môđun đổi dấu. Ví dụ: đối với (1/2), mối quan hệ mong muốn có thể xuất hiện dưới dạng hệ số âm trước bước Euclide tiếp theo. Việc phủ định cả phần dư và hệ số sẽ bảo toàn sự đồng đẳng, cho ra tử số và mẫu số dương cần thiết. Việc bỏ qua việc hiệu chỉnh dấu hiệu này có thể tạo ra mẫu số âm ngay cả khi việc xây dựng lại cơ bản là hợp lệ.
