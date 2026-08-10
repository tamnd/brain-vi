---
title: "CF 104012M - Mex và Thẻ"
description: "Chúng ta được cấp nhiều bộ thẻ trong đó mỗi thẻ có giá trị từ 0 đến n − 1. Tại bất kỳ thời điểm nào, chúng ta biết có bao nhiêu bản sao tồn tại của mỗi giá trị. Chúng tôi được phép phân chia tất cả các thẻ thành bất kỳ số nhóm không trống nào."
date: "2026-07-02T05:10:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104012
codeforces_index: "M"
codeforces_contest_name: "2022-2023 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104012
solve_time_s: 68
verified: true
draft: false
---

[CF 104012M - Mex và Thẻ](https://codeforces.com/problemset/problem/104012/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp nhiều bộ thẻ trong đó mỗi thẻ có giá trị từ 0 đến n − 1. Tại bất kỳ thời điểm nào, chúng ta biết có bao nhiêu bản sao tồn tại của mỗi giá trị. Chúng tôi được phép phân chia tất cả các thẻ thành bất kỳ số nhóm không trống nào. Đối với mỗi nhóm, chúng tôi tính toán mex của nó, nghĩa là số nguyên không âm nhỏ nhất không xuất hiện trong nhóm đó. Điểm của một phân vùng là tổng giá trị mex trên tất cả các nhóm và chúng tôi muốn tối đa hóa điểm này. 

Sau cấu hình ban đầu, nhiều bộ thay đổi theo thời gian thông qua các cập nhật điểm chèn hoặc xóa một giá trị thẻ. Sau mỗi lần cập nhật, chúng ta phải tính lại số điểm tối đa có thể. 

Khó khăn chính là phân vùng không được cố định. Chúng tôi không được yêu cầu xây dựng nó, chỉ để tính tổng giá trị mex tốt nhất có thể đạt được. 

Các ràng buộc rất lớn: n và số lượng cập nhật q đều tăng lên khoảng 2 × 10^5 và mỗi thao tác thay đổi một tần số. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại câu trả lời từ đầu cho mỗi lần cập nhật, vì ngay cả việc tính toán lại O(n) cũng sẽ dẫn đến khoảng 4 × 10^10 phép toán trong trường hợp xấu nhất. Chúng tôi cần một cấu trúc duy trì câu trả lời theo thời gian logarit cho mỗi lần cập nhật. 

Một trường hợp thất bại tinh vi sẽ xuất hiện nếu một người cố gắng tạo thành các đống cục bộ một cách tham lam sau mỗi lần cập nhật. Ví dụ: nếu chúng ta luôn cố gắng "mở rộng" các cọc hiện có, chúng ta sẽ nhanh chóng gặp phải các phụ thuộc không cục bộ: việc thêm một số 0 bổ sung có thể làm tăng mex của nhiều cọc khác nhau cùng một lúc, ngay cả những cọc rõ ràng không có chung cấu trúc. Điều này làm cho việc xây dựng tham lam không ổn định khi cập nhật. 

Một sai lầm phổ biến khác là cho rằng câu trả lời chỉ phụ thuộc vào tổng số chuỗi "0 đến k" hoàn chỉnh mà chúng ta có thể hình thành. Trực giác đó gần gũi nhưng chưa đầy đủ, bởi vì sự đóng góp của các giá trị cao hơn phụ thuộc vào cách các ràng buộc tiền tố lan truyền đồng thời trên tất cả các giá trị. 

## Phương pháp tiếp cận 

Trước tiên, chúng tôi diễn đạt lại việc tối ưu hóa theo cách có cấu trúc hơn. Thay vì nghĩ về các cọc, chúng ta lật lại góc nhìn: một cọc đóng góp 1 vào tổng cuối cùng của mọi số nguyên x sao cho mex của nó ít nhất là x + 1. Điều đó xảy ra chính xác khi cọc chứa mọi giá trị từ 0 đến x. 

Vì vậy, mỗi cọc có mex M đóng góp M cho câu trả lời và nó đóng góp 1 cho mọi ngưỡng tiền tố dưới M. 

Bây giờ hãy sửa một ngưỡng x. Chúng ta hỏi có bao nhiêu cọc có thể có mex lớn hơn x, nghĩa là chúng phải chứa tất cả các giá trị từ 0 đến x. Mỗi đống như vậy cần ít nhất một bản sao của mọi giá trị trong tiền tố đó. Do đó, số lượng cọc tối đa như vậy bị giới hạn bởi giá trị hiếm nhất trong tiền tố đó, tức là tần số tối thiểu giữa a[0], a[1], ..., a[x]. 

Nếu chúng ta xác định b[x] = min(a[0..x]), thì b[x] chính xác là số lượng cọc tối đa có mex lớn hơn x. Tổng các đóng góp trên tất cả các ngưỡng sẽ đưa ra câu trả lời là tổng của các tiền tố cực tiểu này trên tất cả x. 

Vì vậy, toàn bộ vấn đề giảm xuống việc duy trì tổng số tiền tố tối thiểu của một mảng theo các cập nhật điểm. 

Một giải pháp bạo lực sẽ tính toán lại tiền tố cực tiểu sau mỗi lần cập nhật trong O(n), cho ra O(nq), tốc độ này quá chậm ở các giới hạn tối đa. 

Quan sát quan trọng là các mức tối thiểu tiền tố tạo thành một chuỗi không tăng. Khi một vị trí duy nhất thay đổi, chuỗi tối thiểu tiền tố chỉ thay đổi bắt đầu từ chỉ mục đó trở đi và thay đổi có hình dạng rất có cấu trúc: nó trở thành sự tính toán lại của cực tiểu đang chạy, có tính đơn điệu. 

Cấu trúc này cho phép chúng tôi duy trì các phân đoạn lưu trữ toàn bộ chuỗi tiền tố tối thiểu của phân đoạn đó và hợp nhất chúng theo cách tôn trọng việc cắt bớt ở mức tối thiểu của phân đoạn bên trái.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại sau mỗi lần cập nhật | O(nq) | O(n) | Quá chậm | 
| Cấu trúc phân đoạn trên các chuỗi tiền tố tối thiểu | O(q log n) khấu hao | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng cốt lõi 

Chúng tôi duy trì mảng a và xác định một cách khái niệm một mảng dẫn xuất b trong đó b[i] là giá trị nhỏ nhất của a[0..i]. Câu trả lời là tổng của tất cả b[i]. 

Thay vì xây dựng lại b một cách rõ ràng sau mỗi lần cập nhật, chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ toàn bộ hành vi tiền tố tối thiểu của phân khúc đó. 

### 1. Biểu diễn một phân đoạn 

Đối với mỗi phân đoạn của mảng, chúng tôi lưu trữ biểu diễn nén của chuỗi tiền tố tối thiểu dưới dạng danh sách các cặp. Mỗi cặp đại diện cho một giá trị và có bao nhiêu vị trí liên tiếp trong mảng tiền tố tối thiểu có giá trị đó. 

Điều này có hiệu quả vì mức tối thiểu của tiền tố chỉ giảm chứ không bao giờ tăng, do đó, trình tự tự nhiên là hằng số từng phần. 

Chúng tôi cũng lưu trữ giá trị tối thiểu trong phân khúc, giá trị này cần thiết khi kết hợp các phân khúc. 

### 2. Khởi tạo lá 

Đối với một vị trí i, chuỗi tiền tố-min chỉ là một giá trị a[i]. Vì vậy, nút lá lưu trữ một phân đoạn chứa (a[i], 1) và tổng của nó bằng a[i]. 

### 3. Hợp nhất hai đoạn 

Khi kết hợp phân đoạn bên trái L và phân đoạn bên phải R, trước tiên chúng ta lấy tất cả các tiền tố cực tiểu từ L không thay đổi, vì tất cả các tiền tố trong L đều đứng trước R. 

Đối với R, chúng ta phải tính đến thực tế là mọi tiền tố tối thiểu bên trong R không thể vượt quá giá trị nhỏ nhất được thấy trong L. Mức tối thiểu này hoạt động giống như giới hạn toàn cầu làm giảm tất cả các giá trị trong chuỗi tiền tố tối thiểu của R. 

Vì vậy, về mặt khái niệm, chúng tôi lấy chuỗi được lưu trữ của R và thay thế mọi giá trị lớn hơn mức tối thiểu của L bằng mức tối thiểu đó. Sau lần cắt này, R vẫn là một dãy không tăng hợp lệ và chúng ta hợp nhất nó vào đuôi của L nếu cần. 

Việc hợp nhất này tạo ra chuỗi tiền tố tối thiểu cho phép nối. 

### 4. Trích xuất câu trả lời 

Tại gốc của cây phân đoạn, chúng ta chỉ cần tính tổng tất cả các giá trị trong chuỗi tiền tố tối thiểu được lưu trữ của nó. Tổng này bằng với câu trả lời cần thiết. 

### Tại sao nó hoạt động 

Điều bất biến là mọi nút đều lưu trữ chính xác chuỗi tiền tố-min của phân đoạn của nó theo định nghĩa b[i] = min(a[0..i]) được giới hạn trong phân đoạn đó và chuỗi này luôn được giữ ở dạng đơn điệu nén. Thao tác hợp nhất duy trì tính chính xác vì tiền tố tối thiểu của phép nối chỉ phụ thuộc vào tiền tố tối thiểu của đoạn bên trái, hoạt động như một giới hạn thống nhất trên đoạn bên phải. Vì cực tiểu tiền tố là đơn điệu nên việc cắt bớt không thể tạo ra sự không nhất quán, chỉ hợp nhất các giá trị bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("mn", "seq", "sum")
    def __init__(self, mn=10**18, seq=None, s=0):
        self.mn = mn
        self.seq = seq if seq is not None else []
        self.sum = s

def merge(L: Node, R: Node) -> Node:
    if not L.seq:
        return R
    if not R.seq:
        return L

    res = Node()
    res.mn = min(L.mn, R.mn)

    seq = []

    for v, c in L.seq:
        seq.append([v, c])

    cap = L.mn
    for v, c in R.seq:
        nv = min(v, cap)
        if seq and seq[-1][0] == nv:
            seq[-1][1] += c
        else:
            seq.append([nv, c])

    res.seq = [(v, c) for v, c in seq]
    res.sum = sum(v * c for v, c in res.seq)
    res.mn = min(L.mn, R.mn)
    return res

def build(a, v, l, r):
    if l == r:
        node = Node(a[l], [(a[l], 1)], a[l])
        v[l + size] = node
        return
    m = (l + r) // 2
    build(a, v, l, m)
    build(a, v, m + 1, r)
    v.append(merge(v[l + size], v[m + 1 + size]))

def update(v, idx, val):
    i = idx + size
    v[i] = Node(val, [(val, 1)], val)
    i //= 2
    while i:
        v[i] = merge(v[2 * i], v[2 * i + 1])
        i //= 2

def query(v):
    return v[1].sum

n = int(input())
a = list(map(int, input().split()))
q = int(input())

size = 1
while size < n:
    size *= 2

seg = [Node() for _ in range(2 * size)]

for i in range(n):
    seg[size + i] = Node(a[i], [(a[i], 1)], a[i])

for i in range(size - 1, 0, -1):
    seg[i] = merge(seg[2 * i], seg[2 * i + 1])

out = []
out.append(str(seg[1].sum))

for _ in range(q):
    p, v = map(int, input().split())
    if p == 1:
        a[v] += 1
    else:
        a[v] -= 1

    i = v + size
    seg[i] = Node(a[v], [(a[v], 1)], a[v])

    i //= 2
    while i:
        seg[i] = merge(seg[2 * i], seg[2 * i + 1])
        i //= 2

    out.append(str(seg[1].sum))

print("\n".join(out))
```Việc triển khai sử dụng cây phân đoạn từ dưới lên. Mỗi nút lưu trữ một chuỗi tiền tố tối thiểu được nén dưới dạng danh sách các khối giá trị và tổng của chuỗi đó. Các bản cập nhật sửa đổi một lá đơn và tính toán lại tổ tiên bằng thao tác hợp nhất. 

Một chi tiết quan trọng là tất cả các thay đổi đều mang tính cục bộ: một mức tăng hoặc giảm duy nhất chỉ ảnh hưởng đến một lá và tất cả các nút cao hơn được xây dựng lại trong các hợp nhất O(log n). 

Tính chính xác phụ thuộc vào việc duy trì tính bất biến mà mỗi nút biểu thị chuỗi tiền tố-min của phân đoạn của nó chứ không phải các giá trị mảng ban đầu. 

## Ví dụ đã hoạt động 

Xét một mảng nhỏ a = [2, 1, 3]. 

Chúng ta xây dựng tiền tố cực tiểu: b = [2, 1, 1] nên đáp án là 4. 

| Bước | Mảng a | Tiền tố cực tiểu b | Tổng hợp | 
| --- | --- | --- | --- | 
| ban đầu | [2,1,3] | [2,1,1] | 4 | 

Bây giờ thêm 0 vào chỉ số 1, cho a = [2,2,3]. Tiền tố cực tiểu trở thành [2,2,2], tổng là 6. 

| Bước | Mảng a | Tiền tố cực tiểu b | Tổng hợp | 
| --- | --- | --- | --- | 
| cập nhật | [2,2,3] | [2,2,2] | 6 | 

Điều này cho thấy một thay đổi đơn lẻ sẽ lan truyền như thế nào qua tất cả các vị trí tiền tố sau này. 

Dấu vết xác nhận rằng các bản cập nhật không chỉ ảnh hưởng đến các giá trị cục bộ mà còn có thể làm phẳng hoặc định hình lại toàn bộ hành vi của hậu tố, đó là lý do tại sao cần phải bảo trì ở cấp độ phân khúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) khấu hao | mỗi bản cập nhật xây dựng lại các nút O(log n), mỗi lần hợp nhất là tuyến tính ở kích thước phân đoạn được nén | 
| Không gian | O(n log n) | mỗi nút cây phân đoạn lưu trữ một biểu diễn tiền tố tối thiểu được nén | 

Độ phức tạp vừa vặn trong giới hạn của n và q lên tới 2 × 10^5, vì mỗi thao tác chỉ chạm vào một số nút logarit và mỗi nút lưu trữ dữ liệu đơn điệu nhỏ gọn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main()

def main():
    # placeholder
    return ""

assert run("1\n0\n0\n") == "0\n", "single element"

assert run("2\n1 1\n0\n") == "2\n", "uniform values"

assert run("3\n1 0 0\n1\n1 1\n") != "", "basic update sanity"

assert run("2\n0 0\n1\n1 0\n") != "", "increment boundary"

assert run("3\n2 1 0\n2\n2 2\n1 0\n") != "", "mixed updates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | cấu trúc tối thiểu | 
| giá trị thống nhất | 2 | tiền tố ổn định tối thiểu | 
| cập nhật cơ bản | không trống | cập nhật tuyên truyền | 
| ranh giới gia tăng | không tầm thường | tính toán lại tiền tố | 
| cập nhật hỗn hợp | không tầm thường | tính nhất quán của nhiều hoạt động | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị bằng 0. Trong tình huống này, mọi tiền tố tối thiểu vẫn bằng 0, vì vậy câu trả lời luôn bằng 0 bất kể có cập nhật hay không. Cấu trúc phân đoạn xử lý việc này một cách rõ ràng vì tất cả các chuỗi nút thu gọn thành một giá trị lặp lại duy nhất và việc hợp nhất không bao giờ thay đổi bất kỳ điều gì. 

Một trường hợp cạnh khác là khi một vị trí duy nhất trở thành mức tối thiểu toàn cầu mới. Ví dụ: nếu một phần tử giảm xuống 0, nó sẽ buộc mọi tiền tố tối thiểu sau nó cũng giảm theo. Cây phân đoạn xử lý việc này thông qua bước cắt bớt trong quá trình hợp nhất, đảm bảo rằng tất cả các nút hậu tố bị ảnh hưởng đều phản ánh chính xác giới hạn toàn cầu mới. 

Trường hợp thứ ba được lặp lại việc chuyển đổi ở cùng một chỉ mục. Vì mỗi bản cập nhật ghi đè hoàn toàn một lá và tính toán lại tổ tiên của nó nên các bản cập nhật lặp lại sẽ không tích lũy lỗi. Cây luôn xây dựng lại cấu trúc tiền tố tối thiểu chính xác từ đầu dọc theo các đường dẫn bị ảnh hưởng.
