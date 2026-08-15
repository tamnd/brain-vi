---
title: "CF 102354I - Từ mô-đun đến hợp lý"
description: "Đối với mỗi trường hợp kiểm tra, giám khảo đã chọn một số hữu tỷ [ x=frac pq, qquad 1le p,qle 10^9. ] Chúng tôi không thể yêu cầu (x) một cách trực tiếp. Thay vào đó, đối với số nguyên tố (m10^9), chúng ta yêu cầu số dư (y) thỏa mãn [ yequiv p q^{-1}pmod m. ] Tương đương, [ pequiv yqpmod m."
date: "2026-08-15T17:45:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "I"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 182
verified: false
draft: false
---

[CF 102354I - Từ mô-đun đến hợp lý](https://codeforces.com/problemset/problem/102354/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp kiểm tra, thẩm phán đã chọn một số hữu tỷ 

[ 
x=\frac pq, 
\qquad 1\le p,q\le 10^9. 
] 

Chúng ta không thể yêu cầu (x) một cách trực tiếp. Thay vào đó, đối với số nguyên tố (m>10^9), chúng ta yêu cầu dư lượng (y) thỏa mãn 

[ 
y\equiv p q^{-1}\pmod m. 
] 

Tương đương, 

[ 
p\equiv yq\pmod m. 
] 

Mục tiêu là khôi phục bất kỳ cặp hợp lệ nào (p,q) mô tả cùng một số hữu tỷ. Cặp này không nhất thiết phải là cặp chính xác được chọn ban đầu, bởi vì (1/2), (2/4) và (500000004/1000000008) đại diện cho cùng một số hữu tỷ và giao thức chấp nhận bất kỳ cặp nào trong giới hạn đại diện cho giá trị đó. Mẫu chính thức chứng minh rõ ràng điều này với (1/2) được trả lời là (2/4). 

Giới hạn (10) truy vấn đủ rộng để xây dựng lại lý thuyết số, nhưng quá nhỏ để thử nhiều mô đun hoặc tìm kiếm thông qua các mẫu số có thể. Vì có thể có (10^5) trường hợp thử nghiệm, nên ngay cả thuật toán hoạt động (O(10^9)) cho mỗi trường hợp cũng có nghĩa là (10^{14}) hoạt động trong trường hợp xấu nhất. Giải pháp dự định chỉ sử dụng hai truy vấn cho mỗi trường hợp thử nghiệm và sau đó thực hiện số bước thuật toán Euclide theo logarit. 

Cách hữu ích nhất để suy nghĩ về vấn đề này là một câu trả lời mô-đun cho chúng ta sự đồng dạng tuyến tính giữa tử số và mẫu số chưa biết. Một mô đun xung quanh (10^9) không đủ lớn để làm cho sự đồng dạng đó là duy nhất, nhưng tích của hai số nguyên tố cho phép có thể xấp xỉ (10^{24}). Nó lớn hơn nhiều so với (2\cdot10^{18}), là thang đo cần thiết để phân biệt hai phân số khác nhau có tử số và mẫu số được giới hạn bởi (10^9). 

Có ba trường hợp khó khăn dễ dẫn đến giải pháp sai. 

Đầu tiên, một mô-đun là không đủ. Lấy (m=1000000007) và giả sử phản hồi là (500000004). Phân số (1/2) tạo ra phần dư này vì (2\cdot500000004\equiv1\pmod m). Nhưng phân số nguyên (500000004/1) tạo ra cùng một lượng dư. Cả tử số và mẫu số đều thỏa mãn giới hạn, do đó, giải pháp chỉ sử dụng mô đun này không thể biết số hữu tỉ nào đã được chọn. Hai mô-đun loại bỏ sự mơ hồ này. 

Thứ hai, cặp ẩn không cần phải giảm. Ví dụ: biểu diễn ẩn (1000000000/1000000000) chỉ đơn giản là số hữu tỷ (1). Một thuật toán tái thiết đòi hỏi phải khôi phục chính xác tử số và mẫu số ban đầu sẽ giải quyết được một vấn đề mà thẩm phán không yêu cầu. Câu trả lời đúng có thể là (1/1). 

Thứ ba, cặp phục hồi từ việc tái thiết hợp lý dự kiến ​​sẽ giảm đi. Giả sử giá trị ẩn là (2/4). Các phương trình mô-đun mô tả giá trị hữu tỷ (1/2) và việc tái cấu trúc Euclide tự nhiên tìm thấy (1/2), chứ không phải (2/4). Điều này hợp lệ vì cả hai cặp đều đại diện cho cùng một giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ truy vấn một số nguyên tố (m), sau đó thử mọi mẫu số có thể có (q) từ (1) đến (10^9). Với mỗi (q), chúng tôi tính toán 

[ 
p=(yq\bmod m) 
] 

và kiểm tra xem (1\le p\le10^9). Mẫu số thực phải vượt qua bài kiểm tra này, vì vậy phương pháp này đúng nếu chúng ta chấp nhận bất kỳ cặp hợp lệ nào. 

Vấn đề là mẫu số có thể có (10^9). Với (10^5) trường hợp kiểm thử, trường hợp xấu nhất đạt tới (10^{14}) phép nhân và rút gọn theo mô-đun. Ngay cả một trường hợp thử nghiệm cũng đã quá lớn so với giới hạn sáu giây. 

Quan sát quan trọng là hai câu trả lời mô-đun có thể được kết hợp thành một câu trả lời theo mô-đun tích của hai số nguyên tố. Đây chính xác là Định lý số dư Trung Hoa. Sau khi kết hợp các phần dư, chúng ta biết một số nguyên (r) sao cho 

[ 
r\equiv pq^{-1}\pmod M, 
] 

ở đâu 

[ 
M=m_1m_2. 
] 

Tương đương, 

[ 
p\equiv rq\pmod M. 
] 

Hai số nguyên tố cố định 

[ 
m_1=999999999989,\qquad 
m_2=1000000000039 
] 

đều là số nguyên tố hợp lệ trong phạm vi được yêu cầu. Chúng được liệt kê dưới dạng các số nguyên tố liên tiếp trong khoảng (10^{12}) trong các bảng nguyên tố tiêu chuẩn. Tích của họ xấp xỉ (10^{24}), vì vậy 

[ 
M>2\cdot10^{18}. 
]

Bây giờ giả sử hai phân số giới hạn khác nhau (p_1/q_1) và (p_2/q_2) tạo ra cùng một dư lượng. Sau đó 

[ 
p_1q_2\equiv p_2q_1\pmod M, 
] 

nên (M) chia 

[ 
p_1q_2-p_2q_1. 
] 

Nhưng mỗi tử số và mẫu số nhiều nhất là (10^9), vì vậy 

[ 
|p_1q_2-p_2q_1|\le2\cdot10^{18<M. 
] 

Bội số duy nhất của (M) trong khoảng đó bằng 0. Do đó 

[ 
p_1q_2=p_2q_1, 
] 

nghĩa là hai phân số bằng nhau. 

Vì vậy, sau CRT, bài toán trở thành bài toán tái thiết hợp lý tiêu chuẩn. Tái cấu trúc hợp lý được kết nối chặt chẽ với thuật toán Euclide mở rộng và các phân số tiếp tục. 

Thuật toán Euclide mở rộng tạo ra phần dư có dạng 

[ 
r_i=s_iM+t_ir. 
] 

Do đó mọi cặp trung gian đều thỏa mãn 

[ 
r_i\equiv t_ir\pmod M. 
] 

Khi phần dư đầu tiên đạt tối đa (10^9), điều kiện mô đun lớn đảm bảo rằng hệ số tương ứng (t_i) cũng nằm trong giới hạn yêu cầu và cho tử số và mẫu số mong muốn. Đây là cách tiếp cận thuật toán Euclide cổ điển để tái thiết hợp lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^9)) mỗi trường hợp thử nghiệm | (O(1)) | Quá chậm | 
| Tối ưu | (O(\log M)) bước số học cho mỗi ca kiểm thử | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Yêu cầu modulo dư lượng (m_1=999999999989), sau đó yêu cầu modulo dư lượng (m_2=10000000000039). Cả hai số đều là số nguyên tố và nằm hoàn toàn giữa (10^9) và (10^{12}), vì vậy cả hai truy vấn đều hợp pháp. Việc sử dụng các số nguyên tố lớn như vậy làm cho tích của chúng vượt xa giới hạn tích chéo (10^{18}). 
2. Kết hợp hai thặng dư với Định lý số dư Trung Hoa. Đặt câu trả lời là (y_1) và (y_2). Viết 

[ 
r=y_1+m_1k. 
] 

Chúng tôi cần 

[ 
y_1+m_1k\equiv y_2\pmod {m_2}, 
] 

vậy 

[ 
k\equiv(y_2-y_1)m_1^{-1}\pmod {m_2}. 
] 

Việc chọn số không âm (k) nhỏ nhất sẽ cho phần dư (r) modulo duy nhất 

[ 
M=m_1m_2. 
] 

Bước CRT biến hai phương trình mô đun nhỏ thành một phương trình có mô đun xung quanh (10^{24}). 

1. Chạy thuật toán Euclide mở rộng trên (M) và (r), đồng thời theo dõi hệ số của (r). Khởi tạo 

[ 
R_0=M,\quad R_1=r, 
] 

và 

[ 
T_0=0,\quad T_1=1. 
] 

Bất biến là 

[ 
R_i=S_iM+T_ir. 
] 

Do đó, 

[ 
R_i\equiv T_ir\pmod M. 
] 

Hệ số (T_i) là mẫu số ứng viên tương ứng với tử số ứng cử viên (R_i). 

1. Thực hiện nhiều lần phép chia Euclide thông thường 

[ 
R_{i-1}=a_iR_i+R_{i+1}, 
] 

và cập nhật 

[ 
T_{i+1}=T_{i-1}-a_iT_i. 
] 

Dừng khi số dư hiện tại lớn nhất là (10^9). Phần còn lại đang giảm chính xác như trong thuật toán Euclide thông thường, do đó chỉ cần lặp lại (O(\log M)). 

1. Chuẩn hóa các dấu sao cho mẫu số dương. Đối với lý trí tích cực được thẩm phán che giấu, việc tái thiết thành công có tử số và mẫu số dương. 
2. Xuất ra tử số và mẫu số. Nếu cặp ẩn không bị rút gọn, thuật toán Euclide sẽ đưa ra biểu diễn rút gọn của cùng một số hữu tỷ, đây vẫn là một câu trả lời hợp lệ. 

### Tại sao nó hoạt động 

Bất biến trung tâm là 

[ 
R_i=S_iM+T_ir, 
] 

vì vậy mọi trạng thái Euclide đại diện cho một ứng cử viên hợp lý (R_i/T_i) có giá trị mô-đun là (r). Phân số ẩn rút gọn thực tế (p/q) thỏa mãn 

[ 
p\equiv rq\pmod M. 
] 

Bởi vì (M>2\cdot10^{18}), hai giá trị hữu tỉ khác nhau có tử số và mẫu số nhiều nhất là (10^9) không thể thỏa mãn cùng một sự đồng dạng. Tái cấu trúc hợp lý cổ điển nói rằng phần dư Euclide đầu tiên vượt qua giới hạn (10^9) chính xác là nghiệm bị chặn. 

Lập luận về tính duy nhất đặc biệt đơn giản. Nếu tồn tại hai ứng cử viên, tích chéo của họ sẽ là bội số của (M), nhưng giá trị tuyệt đối của nó tối đa là (2\cdot10^{18}), hoàn toàn nhỏ hơn (M). Do đó tích chéo phải bằng 0, nên cả hai ứng cử viên đều đại diện cho cùng một số hữu tỷ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

M1 = 999999999989
M2 = 1000000000039
LIMIT = 10**9

def crt(a1, a2):
    # x = a1 (mod M1)
    # x = a2 (mod M2)
    inv = pow(M1, -1, M2)
    k = ((a2 - a1) * inv) % M2
    return a1 + M1 * k

def reconstruct(r, mod):
    # Extended Euclidean algorithm.
    #
    # rem = s * mod + t * r
    # so rem == t * r (mod mod).
    old_rem, rem = mod, r
    old_t, t = 0, 1

    while rem > LIMIT:
        q = old_rem // rem

        old_rem, rem = rem, old_rem - q * rem
        old_t, t = t, old_t - q * t

    numerator = rem
    denominator = t

    if denominator < 0:
        numerator = -numerator
        denominator = -denominator

    # The original fraction may not have been reduced.
    # The Euclidean reconstruction is normally already reduced,
    # but reducing here also makes the returned representation explicit.
    g = __import__("math").gcd(numerator, denominator)
    numerator //= g
    denominator //= g

    return numerator, denominator

def ask(m):
    print("?", m, flush=True)
    return int(input())

def main():
    t = int(input())

    for _ in range(t):
        y1 = ask(M1)
        y2 = ask(M2)

        r = crt(y1, y2)
        p, q = reconstruct(r, M1 * M2)

        print("!", p, q, flush=True)

if __name__ == "__main__":
    main()
```Hai hằng số ở trên cùng là các số nguyên tố pháp lý cố định nên không có kiểm tra tính nguyên tố trong quá trình tương tác. Tích của các số nguyên tố này nằm trong khoảng (10^{24}), cao hơn ngưỡng yêu cầu (2\cdot10^{18}). 

các`crt`hàm thực hiện phương trình 

[ 
r=y_1+m_1k. 
] 

Sự đồng đẳng thứ hai xác định (k) và`pow(M1, -1, M2)`tính toán nghịch đảo mô đun. Ba đối số của Python`pow`hỗ trợ số mũ âm cho nghịch đảo mô-đun trong Python 3.8 trở lên. 

các`reconstruct`hàm chỉ lưu trữ phần dư và hệ số của nó là (r). Hệ số của mô đun là không cần thiết sau khi bất biến đã được thiết lập, điều này khiến việc thực hiện ở mức nhỏ. 

Điều kiện vòng lặp là`rem > LIMIT`, không`rem >= LIMIT`. Phần dư bằng (10^9) đã hợp pháp và phải được chấp nhận. Đây là một điều kiện biên dễ mắc sai lầm. 

Số nguyên Python có độ chính xác tùy ý, do đó sản phẩm CRT xung quanh (10^{24}) không bị tràn. Việc triển khai C++ sử dụng số nguyên 64-bit đã ký cũng sẽ phải tránh lưu trữ sản phẩm này trong một số nguyên đã ký.`long long`, bởi vì (10^{24}) nằm ngoài phạm vi của nó. Python không có vấn đề này. 

Việc rút gọn gcd cuối cùng xử lý thực tế là cặp ban đầu (p,q) không nhất thiết phải nguyên tố cùng nhau. Ví dụ: giá trị ẩn (2/4) được xây dựng lại thành (1/2), đây là một câu trả lời hợp lệ. 

Mọi truy vấn đều được xóa ngay lập tức. Không có`flush=True`, một giải pháp tương tác có thể chờ vô thời hạn vì người đánh giá có thể không nhận được truy vấn vẫn còn trong bộ đệm đầu ra. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp sử dụng một truy vấn cho mỗi trường hợp thử nghiệm và thể hiện giao thức tương tác. Với (m=1000000007), câu trả lời (500000004) tương ứng với (1/2), trong khi giám khảo chấp nhận câu trả lời (2/4). Giải pháp tối ưu được mô tả ở đây yêu cầu hai số nguyên tố lớn hơn thay vào đó, vì một mô đun đơn lẻ không cung cấp đủ thông tin. 

Đối với dấu vết tái thiết đầu tiên, hãy xem xét cùng một giá trị hữu tỉ (x=1/2), sử dụng hai số nguyên tố từ nghiệm. Dư lượng là 

[ 
y_1=\frac{m_1+1}{2}=499999999995 
] 

và 

[ 
y_2=\frac{m_2+1}{2}=500000000020. 
] 

Kết quả CRT là phần dư của (1/2) modulo (M=m_1m_2), cụ thể là 

[ 
r=\frac{M+1}{2}. 
] 

| Trạng thái Euclide | Phần còn lại | Hệ số (r) | Ý nghĩa | 
| --- | --- | --- | --- | 
| Ban đầu | (M) | (0) | (M=1\cdot M+0\cdot r) | 
| Ban đầu | ((M+1)/2) | (1) | (r=0\cdot M+1\cdot r) | 
| Sau (q=1) | ((M-1)/2) | (-1) | (R=-M+r) | 
| Sau (q=1) | (1) | (2) | (1=2r-M) | 

Hàng cuối cùng có phần dư (1) và hệ số (2), do đó phân số được xây dựng lại là (1/2). Bất biến trực tiếp xác minh mối quan hệ mô đun vì (2r\equiv1\pmod M). 

Đối với dấu vết thứ hai, sử dụng (x=2/3). Vì (M\equiv2\pmod3) nên dư lượng của (2/3) modulo (M) là 

[ 
r=\frac{M+2}{3}. 
] 

Trình tự Euclide trở nên đặc biệt ngắn. 

| Trạng thái Euclide | Phần còn lại | Hệ số (r) | Ý nghĩa | 
| --- | --- | --- | --- | 
| Ban đầu | (M) | (0) | (M=1\cdot M+0\cdot r) | 
| Ban đầu | ((M+2)/3) | (1) | (r=0\cdot M+1\cdot r) | 
| Sau (q=2) | ((M-4)/3) | (-2) | (R=M-2r) | 
| Sau (q=1) | (2) | (3) | (2=3r-M) | 

Số dư đầu tiên bên dưới (10^9) là (2), có hệ số (3). Do đó kết quả là (2/3). Điều này chứng tỏ tại sao việc theo dõi hệ số Euclide là đủ và tại sao chúng ta không cần phải tìm kiếm các mẫu số có thể có. 

Mẫu chính thức cũng bao gồm các trường hợp (1/1), (1/2) và (2/1), đồng thời chỉ ra cụ thể rằng câu trả lời không rút gọn như (2/4) hợp lệ cho (1/2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(t\log M)) bước số học | Hai thao tác CRT và một thuật toán Euclide mở rộng cho mỗi trường hợp thử nghiệm | 
| Không gian | (O(1)) | Chỉ một số nguyên không đổi được lưu trữ | 

Ở đây (M=m_1m_2) là khoảng (10^{24}), do đó độ dài bit của nó chỉ khoảng (80). Do đó, thuật toán Euclide chỉ mất vài chục lần lặp cho mỗi trường hợp thử nghiệm. Trong (10^5) trường hợp, điều này vẫn thực tế, trong khi cách tiếp cận bạo lực sẽ yêu cầu kiểm tra mẫu số lên tới (10^{14}). 

Bản thân sự tương tác sử dụng chính xác hai truy vấn cho mỗi trường hợp thử nghiệm, thấp hơn nhiều so với giới hạn mười. 

## Trường hợp thử nghiệm 

Vì tác vụ ban đầu có tính tương tác nên đầu vào mẫu của nó không phải là một tệp đầu vào hoàn chỉnh thông thường. Các dòng hiển thị dưới dạng đầu vào là phản hồi từ giám khảo ẩn, do đó, ngoại tuyến`run()`chức năng không thể phát lại mẫu theo nghĩa đen. Cách hữu ích để kiểm tra đơn vị logic đã gửi là kiểm tra quy trình xây dựng lại thuần túy bằng cách tạo ra hai phản hồi mô-đun từ các giá trị hợp lý đã biết.```python
import sys
import io
from math import gcd

M1 = 999999999989
M2 = 1000000000039
LIMIT = 10**9

def crt(a1, a2):
    inv = pow(M1, -1, M2)
    k = ((a2 - a1) * inv) % M2
    return a1 + M1 * k

def reconstruct(r, mod):
    old_rem, rem = mod, r
    old_t, t = 0, 1

    while rem > LIMIT:
        q = old_rem // rem
        old_rem, rem = rem, old_rem - q * rem
        old_t, t = t, old_t - q * t

    if t < 0:
        rem = -rem
        t = -t

    g = gcd(rem, t)
    return rem // g, t // g

def modular_image(p, q, m):
    return (p * pow(q, -1, m)) % m

def run(inp: str) -> str:
    # Offline test harness.
    # Each line contains the hidden p and q.
    out = []

    for line in inp.strip().splitlines():
        if not line.strip():
            continue

        p, q = map(int, line.split())

        y1 = modular_image(p, q, M1)
        y2 = modular_image(p, q, M2)

        r = crt(y1, y2)
        a, b = reconstruct(r, M1 * M2)

        out.append(f"{a} {b}")

    return "\n".join(out)

# Values represented by the provided interactive sample.
assert run("1 1\n1 2\n2 1\n") == (
    "1 1\n"
    "1 2\n"
    "2 1"
), "provided sample values"

# Minimum-size numerator and denominator.
assert run("1 1\n") == "1 1", "minimum-size values"

# Maximum allowed numerator and denominator, both equal.
# The rational value is 1, so 1/1 is the canonical answer.
assert run("1000000000 1000000000\n") == "1 1", "maximum equal values"

# Numerator and denominator are both close to the upper boundary
# and are coprime.
assert run("999999999 1000000000\n") == "999999999 1000000000", (
    "upper-bound coprime fraction"
)

# Small denominator and maximum numerator.
assert run("1000000000 1\n") == "1000000000 1", (
    "maximum numerator with denominator 1"
)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`,`1 2`,`2 1`|`1 1`,`1 2`,`2 1`| Các giá trị được biểu thị trong mẫu được cung cấp | 
|`1 1`|`1 1`| Ranh giới kích thước tối thiểu | 
|`1000000000 1000000000`|`1 1`| Biểu diễn không rút gọn và giá trị bằng nhau tối đa | 
|`999999999 1000000000`|`999999999 1000000000`| Cả hai giới hạn đồng thời, với phần giảm | 
|`1000000000 1`|`1000000000 1`| Hành vi ranh giới tử số và mẫu số tối đa | 

## Vỏ cạnh 

Va chạm một mô đun là dạng hư hỏng cơ bản nhất. Với (m=1000000007), phản hồi (500000004) được tạo ra bởi (1/2), bởi vì 

[ 
2\cdot500000004=1000000008\equiv1\pmod m. 
] 

Phản hồi tương tự cũng được tạo ra bởi (500000004/1). Một giải pháp chỉ sử dụng mô đun này không có cách toán học nào để phân biệt hai giá trị. Giải pháp tối ưu yêu cầu một số nguyên tố độc lập thứ hai, kết hợp các câu trả lời thành một mô đun ở trên (10^{18}) và làm cho việc xung đột như vậy là không thể. 

Biểu diễn không rút gọn là một trường hợp tế nhị khác. Đối với giá trị đầu vào (1000000000/1000000000), cả hai số gốc đều ở kích thước tối đa cho phép, nhưng bản thân số hữu tỷ là (1). Phản hồi mô-đun là (1) cho mọi số nguyên tố được truy vấn. Việc tái cấu trúc hợp lý trả về (1/1) và câu trả lời được chấp nhận vì giao thức yêu cầu biểu thị giá trị hợp lý chứ không phải khôi phục cặp ẩn chính xác. 

Đối với một giá trị chẳng hạn như (2/4), các quan sát mô-đun tương tự được tạo ra như đối với (1/2). Việc tái cấu trúc Euclide tìm thấy cặp rút gọn (1/2). Việc chia cả hai thành phần cho gcd của chúng không làm thay đổi giá trị hợp lý được biểu diễn, do đó kết quả vẫn hợp lệ. 

Cuối cùng, số dư chính xác bằng (10^9) phải được chấp nhận. Vòng lặp tái thiết sử dụng`rem > LIMIT`, không`rem >= LIMIT`. Việc coi đẳng thức quá lớn sẽ bỏ qua tử số hợp pháp và có thể chuyển quá trình Euclide qua quá trình tái thiết chính xác. 

Do đó, ý tưởng hoàn chỉnh khá nhỏ gọn: hai truy vấn nguyên tố lớn cung cấp một mô-đun khổng lồ thông qua CRT và mô-đun khổng lồ biến việc khôi phục hợp lý mô-đun thành một vấn đề tái thiết hợp lý giới hạn tiêu chuẩn được giải quyết bằng Euclid mở rộng. Vấn đề ban đầu và các bài xã luận độc lập mô tả cùng quan điểm tái thiết CRT và Euclide.
