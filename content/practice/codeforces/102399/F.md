---
title: "CF 102399F - XOR \u0448\u0438\u0444\u0440\u043e\u0432\u0430\u043d\u0438\u0435"
description: "Chúng tôi duy trì một tập hợp động (A) gồm các số nguyên riêng biệt, ban đầu trống. Sau mỗi lần chèn hoặc xóa, chúng ta phải xác định MEX có thể trở nên nhỏ đến mức nào sau khi chọn một số mặt nạ XOR (x) với (0 le x le k)."
date: "2026-08-10T17:12:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "F"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 762
verified: true
draft: false
---

[CF 102399F - XOR \u0448\u0438\u0444\u0440\u043e\u0432\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/102399/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp động (A) gồm các số nguyên riêng biệt, ban đầu trống. Sau mỗi lần chèn hoặc xóa, chúng ta phải xác định MEX có thể trở nên nhỏ đến mức nào sau khi chọn một số mặt nạ XOR (x) với (0 \le x \le k). 

Đối với mặt nạ cố định (x), tập truyền đi thu được bằng cách thay thế mọi (a\in A) bằng (a\oplus x). MEX của nó là số nguyên không âm nhỏ nhất (y) mà (y\notin{a\oplus x\in A}). Chúng tôi cần mức tối thiểu của MEX này trên mọi (x) được phép. 

Tất cả các giá trị đều vừa với 20 bit, do đó chỉ có (2^{20}=1.048.576) giá trị có thể xảy ra trong (A). Số lượng cập nhật lớn hơn rất nhiều, lên tới (200.000). Điều này khiến giải pháp quét toàn bộ miền giá trị sau mỗi lần cập nhật trở nên bất khả thi. Ngay cả (O(2^{20}q)) cũng là khoảng (2.1\cdot10^{11}) phép toán, vượt xa những gì một giải pháp cuộc thi hai giây có thể thực hiện được. Chúng tôi cần quá trình xử lý trước phụ thuộc vào vũ trụ (20)-bit cố định, trong khi mỗi lần cập nhật sẽ tốn nhiều thời gian logarit nhất. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Tập trống là một trong số đó. Ví dụ,```
1 0
1 5
```có đầu ra```
0
```bởi vì tập được chuyển đổi vẫn trống, có MEX bằng (0). Một giải pháp giả định tồn tại một số giá trị được chuyển đổi sẽ mắc sai lầm này. 

Một trường hợp quan trọng khác là khi mọi giá trị từ (0) đến (k) đều đã có trong (A). Ví dụ,```
3 2
1 0
1 1
1 2
```có đầu ra```
0
0
1
```Sau lần cập nhật thứ ba, mọi mặt nạ XOR được phép là (0,1,2), vì vậy không có mặt nạ nào trong số chúng có thể làm cho (0) biến mất ngay lập tức. Câu trả lời trở thành (1). Việc triển khai bất cẩn chỉ kiểm tra xem (0) có vắng mặt trong (A) có bỏ sót tác dụng của XOR hay không. 

Ranh giới trên của phạm vi XOR cũng rất đáng kể. Nếu (k=2^{20}-1), mọi giá trị 20 bit có thể có đều là mặt nạ được phép. Vì có nhiều nhất (200.000) giá trị trong khi có (2^{20}) giá trị có thể có, nên một số giá trị (z) bị thiếu và chúng ta có thể chọn (x=z). Khi đó (z\oplus x=0), nên câu trả lời luôn là (0). Việc quên rằng (x=k) được cho phép sẽ tạo ra từng lỗi một ở đây. 

Cuối cùng, các giá trị trong (A) không nhất thiết phải gần bằng 0. Ví dụ,```
1 2
1 7
```có đầu ra```
0
```vì (0) bị thiếu và việc chọn (x=0) đã cho MEX (0). Giải pháp chỉ tìm kiếm xung quanh các giá trị hiện được lưu trữ là không cần thiết và có thể trở nên không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi mặt nạ được phép (x). Với mỗi (x), chúng ta có thể xây dựng hoặc kiểm tra một cách khái niệm tập đã biến đổi và tìm MEX của nó. Nếu (|A|=n), MEX của nó tối đa là (n), do đó việc triển khai đơn giản có thể kiểm tra tư cách thành viên của (0,1,\ldots,n). Trong trường hợp xấu nhất, chi phí này là (O((k+1)n)) cho mỗi lần cập nhật. Với (q=200.000), (k) gần với (2^{20}) và (n) gần với (200.000), trường hợp xấu nhất là đại khái 

[ 
200.000\cdot1.048.576\cdot200.000 
\khoảng 4,2\cdot10^{16} 
] 

kiểm tra thành viên. Ngay cả việc thay thế phép tính MEX bằng cấu trúc dữ liệu nhanh hơn cũng không lưu được phương pháp này, vì việc thử tất cả (2^{20}) mặt nạ sau mỗi lần cập nhật đã quá tốn kém. 

Quan sát quan trọng là ngừng suy nghĩ về MEX như một thứ phải được tính toán riêng cho mỗi mặt nạ. 

Sửa mặt nạ (x). MEX của nó là (y) nhỏ nhất sao cho (y) không thuộc tập được chuyển đổi. Điều kiện (y\notin{a\oplus x\in A}) tương đương với 

[ 
y\oplus x\notin A. 
] 

hãy để 

[ 
z=y\oplus x. 
] 

Khi đó (z) chỉ đơn giản là thiếu một số giá trị trong (A) và 

[ 
y=x\oplus z. 
] 

Do đó, 

\min_{z\notin A}(x\oplus z). 
] 

Lấy mức tốt nhất cho phép (x), 

\min_{z\notin A}\min_{0\le x\le k}(x\oplus z). 
] 

Điều này thay đổi vấn đề hoàn toàn. Đối với mọi giá trị có thể (z), hãy xác định hàm tĩnh 

[ 
g(z)=\min_{0\le x\le k}(x\oplus z). 
] 

Vậy thì câu trả lời chỉ đơn giản là 

[ 
\boxed{\min_{z\notin A}g(z)}. 
] 

Tập (A) là tập động nhưng (g) chỉ phụ thuộc vào (k). Chúng ta có thể xử lý trước các giá trị (g(z)), đếm xem mỗi giá trị có thể có (g) hiện đang thiếu bao nhiêu giá trị và duy trì nhóm không trống nhỏ nhất. 

Có một ràng buộc hữu ích hơn. Nếu (A) hiện chứa (n) giá trị thì mọi tập hợp được chuyển đổi cũng chứa (n) giá trị, do đó MEX của nó tối đa là (n). Vì vậy đáp án cuối cùng nhiều nhất là (n\le q). Chúng ta không bao giờ cần duy trì các nhóm với (g(z)>q). 

Nhiệm vụ còn lại là tính nhanh (g(z)). Điều này có thể được thực hiện trong thời gian không đổi bằng cách sử dụng biểu diễn nhị phân của (z) và (k). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(q^2 2^{20})) trong trường hợp xấu nhất | (O(q)) | Quá chậm | 
| Tối ưu | (O(2^{20}+q\log q)) | (O(q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Viết lại câu trả lời mong muốn thành 

[ 
\min_{z\notin A}g(z), 
\qquad 
g(z)=\min_{0\le x\le k}(z\oplus x). 
] 

Điều này có hiệu quả vì đối với một (x) cố định, mọi giá trị còn thiếu (z) của (A) tương ứng một cách khách quan với giá trị còn thiếu (y=z\oplus x) của (A\oplus x). 
2. Hãy quan sát rằng (g(z)=0) bất cứ khi nào (z\le k), bởi vì chúng ta có thể đơn giản chọn (x=z). 

Do đó tất cả các giá trị (0,1,\ldots,k) thuộc về cùng một nhóm (g=0). 
3. Với (z>k), so sánh (z) và (k) từ bit có ý nghĩa nhất trở xuống. Ở bit khác nhau cao nhất của chúng, (z) có (1) và (k) có (0), vì (z>k). 

Một (x) được phép không thể giữ bit đó bằng (z), vì vậy XOR phải chứa bit đó. Sau khi sửa nó, các bit thấp hơn sẽ gặp vấn đề tương tự với các phần dưới của (z) và (k). 
4. Hãy để 

[ 
d=z\oplus k. 
] 

Các bit trong đó (z) và (k) khác nhau và (k) chứa (1) chính xác là các bit được đặt của 

[ 
d\mathbin{&}k. 
] 

Nếu bit cao nhất đó là (r), quá trình tham lam sẽ dừng ở đó, vì tại bit đó (z) có (0) và (k) có (1), nên (x) đã chọn đã trở nên nhỏ hơn (k). Sau đó, tất cả các bit thấp hơn có thể được khớp một cách tự do. 

Vì vậy, nếu`bad = d & k`, sau đó 

[ 
g(z)=(d\mathbin{&}\neg k) 
\mathbin{&} 
\left(-2^{\operatorname{bit_length}(xấu)}\right). 
]

Nếu như`bad`bằng 0, không có bit dừng như vậy, vì vậy 

[ 
g(z)=d\mathbin{&}\neg k. 
] 
5. Tính toán trước có bao nhiêu giá trị (z\in[0,2^{20})) có mỗi giá trị (g(z)\le q). Các giá trị có (g(z)) lớn hơn có thể bị bỏ qua vì câu trả lời không bao giờ có thể vượt quá (q). 

Ban đầu mọi giá trị (z) đều bị thiếu trong (A), do đó số đếm này mô tả tập hợp bị thiếu hoàn chỉnh. 
6. Duy trì một heap tối thiểu chứa tất cả các giá trị nhóm có thể có từ (0) đến (q). Bên cạnh đó, duy trì`cnt[v]`, số lượng hiện đang thiếu (z) với (g(z)=v). 

Khi một giá trị được chèn vào (A), nhóm của nó sẽ mất một giá trị bị thiếu. Khi nó bị xóa khỏi (A), nhóm của nó sẽ nhận được một giá trị còn thiếu. 
7. Sau mỗi lần cập nhật, hãy xóa các mục heap có số nhóm bằng 0. Giá trị heap nhỏ nhất còn lại chính xác là 

[ 
\min_{z\notin A}g(z), 
] 

đó là MEX tối thiểu có thể được yêu cầu. 

Tính bất biến đó là`cnt[v]`luôn bằng số giá trị hiện không có trong (A) có XOR tốt nhất có thể với mặt nạ được phép chính xác là (v). Vùng heap đại diện cho các chỉ số nhóm ứng cử viên, trong khi các nhóm có số lượng bằng 0 bị loại bỏ một cách lười biếng. Do đó, mức tối thiểu của heap luôn là nhỏ nhất (g(z)) trong số tất cả (z) bị thiếu, mà phép biến đổi ở trên chính xác là câu trả lời cho vấn đề ban đầu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

LIMIT = 1 << 20

def solve():
    q, k = map(int, input().split())

    cnt = [0] * (q + 1)

    # Every z in [0, k] has g(z) = 0.
    cnt[0] = k + 1

    not_k = ~k

    # For z > k, compute g(z) in O(1).
    for z in range(k + 1, LIMIT):
        d = z ^ k
        bad = d & k

        g = d & not_k

        if bad:
            # Keep only bits strictly above the highest bit of bad.
            g &= -(1 << bad.bit_length())

        if g <= q:
            cnt[g] += 1

    # Initially every bucket is a candidate.
    heap = list(range(q + 1))
    heapq.heapify(heap)

    # True means that this bucket currently has an entry in the heap.
    in_heap = [True] * (q + 1)

    def get_g(z):
        if z <= k:
            return 0

        d = z ^ k
        bad = d & k

        g = d & not_k

        if bad:
            g &= -(1 << bad.bit_length())

        return g

    ans = []

    for _ in range(q):
        typ, z = map(int, input().split())
        g = get_g(z)

        if g <= q:
            if typ == 1:
                # z becomes present, so it is no longer missing.
                cnt[g] -= 1
            else:
                # z becomes missing again.
                if cnt[g] == 0 and not in_heap[g]:
                    heapq.heappush(heap, g)
                    in_heap[g] = True
                cnt[g] += 1

        while cnt[heap[0]] == 0:
            v = heapq.heappop(heap)
            in_heap[v] = False

        ans.append(str(heap[0]))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý bắt đầu bằng`cnt[0] = k + 1`, bởi vì mọi giá trị từ (0) đến (k) có khoảng cách XOR bằng 0 tính từ khoảng cho phép. Chỉ các giá trị lớn hơn (k) mới cần công thức bitwise. 

Đối với một giá trị như vậy,`d = z ^ k`ghi lại chính xác vị trí mà hai số khác nhau. biểu thức`d & ~k`giữ các vị trí khác nhau trong đó (k) có số 0, đó là các vị trí nhất thiết đóng góp vào XOR tối thiểu. Nếu như`bad = d & k`là khác 0, bit thiết lập cao nhất của nó xác định vị trí thấp hơn đầu tiên trong đó (z) có 0 và (k) có 1. biểu thức`-(1 << bad.bit_length())`xóa mọi bit bên dưới vị trí đó. 

Bản thân bản cập nhật chỉ cần một phép tính (g(z)). Phần chèn làm giảm nhóm của nó vì giá trị không còn bị thiếu. Việc xóa sẽ tăng nhóm của nó vì giá trị lại bị thiếu. 

Đống là lười biếng. Một nhóm không bị xóa ngay lập tức khi số lượng của nó bằng 0. Thay vào đó, vòng dọn dẹp chỉ loại bỏ các nhóm có số lượng bằng 0 khi chúng đạt đến đỉnh. các`in_heap`mảng ngăn chặn các mục nhập heap trùng lặp không cần thiết khi một nhóm lại không còn trống. 

Tất cả số học chỉ là số nguyên. Số nguyên Python không có vấn đề tràn ở đây và tất cả các giá trị có liên quan đều nằm gọn trong miền 20 bit. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, (k=2). Các giá trị liên quan của (g) là 

[ 
g(0)=g(1)=g(2)=0, 
\quad 
g(3)=1, 
\quad 
g(4)=g(5)=g(6)=4, 
\quad 
g(7)=5. 
] 

Dấu vết là: 

| Bước | Hoạt động | Giá trị đã thay đổi | (g(z)) | Thiếu số lượng (g=0) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | thêm 1 | 1 | 0 | 2 | 0 | 
| 2 | thêm 0 | 0 | 0 | 1 | 0 | 
| 3 | thêm 2 | 2 | 0 | 0 | 1 | 
| 4 | xóa 1 | 1 | 0 | 1 | 0 | 
| 5 | thêm 3 | 3 | 1 | 0 | 0 | 
| 6 | thêm 1 | 1 | 0 | 0 | 4 | 
| 7 | thêm 4 | 4 | 4 | 0 | 4 | 
| 8 | loại bỏ 3 | 3 | 1 | 0 | 1 | 

Hoạt động thứ ba là điểm đầu tiên có tất cả các giá trị (0,1,2). Nhóm 0 trở nên trống, do đó vùng heap tiến tới (g=1), tương ứng với giá trị còn thiếu (3). Ở thao tác thứ sáu, giá trị (1) được chèn lại, do đó các giá trị bị thiếu không còn chứa bất kỳ phần tử nào có (g=0) hoặc (g=1). Nhóm có sẵn tiếp theo là (g=4). 

Đối với Mẫu 2, (k=1). Ở đây (g(0)=g(1)=0), trong khi (g(2)=g(3)=2). 

| Bước | Hoạt động | Giá trị đã thay đổi | (g(z)) | Thiếu số lượng (g=0) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | thêm 2 | 2 | 2 | 2 | 0 | 
| 2 | thêm 1 | 1 | 0 | 1 | 0 | 
| 3 | thêm 3 | 3 | 2 | 1 | 0 | 
| 4 | loại bỏ 2 | 2 | 2 | 1 | 0 | 
| 5 | thêm 0 | 0 | 0 | 0 | 2 | 

Thao tác cuối cùng sẽ điền giá trị còn thiếu cuối cùng trong khoảng cho phép, cụ thể là (0). Khi đó, nhóm nhỏ nhất còn lại là (g=2), được tạo ra bởi giá trị còn thiếu (2), đưa ra câu trả lời cuối cùng (2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^{20}+q\log q)) | Mỗi giá trị 20 bit có thể được xử lý một lần trong quá trình tiền xử lý và mỗi bản cập nhật sẽ thực hiện một thao tác heap cộng với việc dọn dẹp từng phần. | 
| Không gian | (O(q)) | Số lượng nhóm, vùng heap và mảng trạng thái vùng heap phụ đều có kích thước (O(q)). | 

Miền giá trị cố định chỉ chứa (1.048.576) số nên việc xử lý trước là thực tế. Phần động chỉ thực hiện công việc heap (O(\log q)) cho mỗi bản cập nhật, cung cấp khoảng (200.000\log_2 200.000) hoạt động ở mức heap. Thuật toán không bao giờ xây dựng cây trie hoặc cây phân đoạn trên miền (2^{20}) đầy đủ, điều này giúp cho việc sử dụng bộ nhớ Python ở mức nhỏ. 

## Trường hợp thử nghiệm```python
import io
import sys
import heapq

LIMIT = 1 << 20

def run(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    q = int(next(it))
    k = int(next(it))

    cnt = [0] * (q + 1)
    cnt[0] = k + 1

    not_k = ~k

    for z in range(k + 1, LIMIT):
        d = z ^ k
        bad = d & k
        g = d & not_k

        if bad:
            g &= -(1 << bad.bit_length())

        if g <= q:
            cnt[g] += 1

    heap = list(range(q + 1))
    heapq.heapify(heap)
    in_heap = [True] * (q + 1)

    def get_g(z):
        if z <= k:
            return 0

        d = z ^ k
        bad = d & k
        g = d & not_k

        if bad:
            g &= -(1 << bad.bit_length())

        return g

    out = []

    for _ in range(q):
        typ = int(next(it))
        z = int(next(it))
        g = get_g(z)

        if g <= q:
            if typ == 1:
                cnt[g] -= 1
            else:
                if cnt[g] == 0 and not in_heap[g]:
                    heapq.heappush(heap, g)
                    in_heap[g] = True
                cnt[g] += 1

        while cnt[heap[0]] == 0:
            v = heapq.heappop(heap)
            in_heap[v] = False

        out.append(str(heap[0]))

    return "\n".join(out)

# Provided sample 1
assert run(
    """8 2
1 1
1 0
1 2
2 1
1 3
1 1
1 4
2 3
"""
) == """0
0
1
0
0
4
4
1""", "sample 1"

# Provided sample 2
assert run(
    """5 1
1 2
1 1
1 3
2 2
1 0
"""
) == """0
0
0
0
2""", "sample 2"

# Minimum-size case.
assert run(
    """1 0
1 0
"""
) == """1""", "minimum case"

# Boundary case: every 20-bit value is an allowed XOR mask.
# With only two stored values, many values remain missing, so the answer is 0.
assert run(
    """2 1048575
1 0
1 1048575
"""
) == """0
0""", "maximum k"

# Off-by-one case around k.
assert run(
    """3 2
1 0
1 1
1 2
"""
) == """0
0
1""", "complete allowed interval"

# Repeated insertion and deletion of the same value.
assert run(
    """4 0
1 1
2 1
1 1
2 1
"""
) == """0
0
0
0""", "toggle same value"

# Maximum number of updates, alternating the same value.
# The answer is always 0 because k = 0 and value 0 is never inserted.
q = 200000
parts = [f"{q} 0"]
for i in range(q):
    parts.append("1 1" if i % 2 == 0 else "2 1")

large_input = "\n".join(parts) + "\n"
large_output = run(large_input).split()

assert len(large_output) == q, "maximum q length"
assert all(x == "0" for x in large_output), "maximum q values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`, thêm vào`0`|`1`| Đầu vào nhỏ nhất có thể và câu trả lời tích cực đầu tiên | 
|`k = 1048575`, cộng hai giá trị |`0`,`0`| Ranh giới mặt nạ tối đa | 
| Thêm vào`0,1,2`với`k=2`|`0,0,1`| Chuyển đổi chính xác khi có toàn bộ khoảng thời gian được phép | 
| Chuyển đổi`1`nhiều lần với`k=0`|`0,0,0,0`| Chèn và xóa đúng giá trị giống nhau | 
| (q=200000), luân phiên`1`Và`2`về giá trị`1`| 200000 số không | Số lượng cập nhật tối đa và hành vi của đống lười biếng | 

## Vỏ cạnh 

Khi (A) trống, mọi giá trị đều bị thiếu. Cụ thể, các giá trị (0) đến (k) bị thiếu, do đó nhóm của chúng là (g=0). Vùng heap ngay lập tức báo cáo (0), khớp với thực tế là MEX của tập biến đổi trống là (0). 

Vì```
1 0
1 0
```mặt nạ được phép duy nhất là (x=0) và tập được chuyển đổi là ({0}), vì vậy câu trả lời là (1). Thuật toán bắt đầu bằng một giá trị bị thiếu trong nhóm (g=0), sau đó chèn (0) sẽ làm cho nhóm đó trống. Giá trị còn thiếu tiếp theo có (g=1), do đó vùng heap báo cáo (1). 

Vì```
3 2
1 0
1 1
1 2
```nhóm 0 ban đầu chứa (0,1,2). Mỗi lần chèn sẽ loại bỏ một trong các giá trị này khỏi tập hợp bị thiếu. Sau lần chèn thứ ba, số lượng của nó bằng không. Nhóm nhỏ nhất còn lại là (g(3)=1), do đó đầu ra là (1). 

Đối với ranh giới mặt nạ tối đa,```
2 1048575
1 0
1 1048575
```các mặt nạ được phép bao phủ toàn bộ vũ trụ 20 bit. Vẫn còn nhiều giá trị bị thiếu và bản thân bất kỳ (z) nào bị thiếu đều là mặt nạ được phép. Chọn (x=z) sẽ có (z\oplus z=0), do đó (g(z)=0). Do đó, nhóm (0) vẫn không trống và câu trả lời vẫn là (0). 

Sự khác biệt từng cái một giữa (x\le k) và (x<k) được xử lý theo điều kiện`z <= k`. Nếu (z=k), chọn (x=k) sẽ cho XOR (0), do đó (g(k)=0). Việc loại trừ (k) sẽ chuyển giá trị này vào nhóm dương một cách không chính xác. 

Công thức cho (z>k) cũng xử lý các trường hợp trong đó bit XOR bắt buộc đầu tiên không phải là bit đóng góp duy nhất. Với (k=2) và (z=7), biểu diễn nhị phân là (010) và (111). Tối thiểu là (7\oplus2=5), không phải (4). Đây`d = 5`,`bad = 0`, do đó công thức giữ cả hai bit đóng góp và thu được (g(7)=5). Với (z=5), ta có`d = 7`Và`bad = 2`, bit cao nhất của nó là bit dừng. Phần đóng góp thấp hơn bị xóa, cho ra (g(5)=4). Những trường hợp này chính xác là lý do tại sao chỉ sử dụng bit khác nhau cao nhất sẽ không chính xác.
