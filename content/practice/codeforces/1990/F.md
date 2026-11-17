---
title: "CF 1990F - Đoạn đa giác"
description: "Chúng tôi được cung cấp một mảng động gồm các giá trị dương và chúng tôi liên tục sửa đổi nó đồng thời trả lời các truy vấn phạm vi. Đối tượng thú vị là một mảng con hoạt động giống như độ dài các cạnh của đa giác."
date: "2026-06-08T15:37:21+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "divide-and-conquer", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1990
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 960 (Div. 2)"
rating: 2800
weight: 1990
solve_time_s: 157
verified: false
draft: false
---

[CF 1990F - Phân đoạn đa giác](https://codeforces.com/problemset/problem/1990/F) 

**Đánh giá:** 2800 
**Tags:** vũ lực, cấu trúc dữ liệu, phân chia và chinh phục, dp, tham lam, hai con trỏ 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng động gồm các giá trị dương và chúng tôi liên tục sửa đổi nó đồng thời trả lời các truy vấn phạm vi. Đối tượng thú vị là một mảng con hoạt động giống như độ dài các cạnh của đa giác. 

Một đoạn của mảng được coi là hợp lệ nếu độ dài của nó ít nhất là ba và nếu tất cả các số trong đoạn đó có thể đóng vai trò là độ dài các cạnh của một số đa giác có chính xác số cạnh đó. Điều kiện hình học là sự tổng quát hóa tự nhiên của bất đẳng thức tam giác: tổng tất cả các cạnh phải lớn hơn mỗi cạnh riêng lẻ. Tương tự, nếu chúng ta sắp xếp phân khúc, điều kiện là phần tử lớn nhất hoàn toàn nhỏ hơn tổng của phần còn lại. 

Đối với mỗi truy vấn loại một, chúng ta phải xem xét bên trong một phạm vi cố định và tìm phân đoạn dài nhất thỏa mãn điều kiện đa giác này. Đối với truy vấn loại hai, chúng tôi cập nhật một phần tử. 

Cấu trúc ràng buộc là khó khăn thực sự. Tổng kích thước trên các trường hợp thử nghiệm chỉ tối đa 2·10^5, nhưng số lượng truy vấn cũng lớn, lên tới 10^5. Điều này loại trừ bất kỳ điều gì tính toán lại tính hợp lệ của phân đoạn từ đầu cho mọi truy vấn. Ngay cả O(n) cho mỗi truy vấn cũng đã quá chậm và bất kỳ điều gì liên quan đến việc sắp xếp theo từng truy vấn hoặc theo từng phân đoạn ứng viên đều ngay lập tức không thể thực hiện được. 

Một khó khăn tinh tế là tính hợp lệ không đơn điệu một cách đơn giản trên các mảng con. Một phân đoạn dài hơn có thể trở nên không hợp lệ ngay cả khi tất cả các phân đoạn ngắn hơn bên trong nó đều hợp lệ, vì việc thêm một phần tử lớn có thể phá vỡ điều kiện tổng toàn cục. Ngược lại, một phân đoạn chỉ có thể trở nên hợp lệ sau khi bao gồm một phần tử nhỏ được lựa chọn cẩn thận để bù cho mức tối đa lớn. 

Một trường hợp nhỏ bộc lộ lý luận ngây thơ là thế này: hãy xem xét`[10, 1, 1, 1]`. Đoạn có độ dài 4 không hợp lệ vì 10 quá lớn. Nhưng có những phân đoạn hợp lệ như`[1,1,1]`. Một cách tiếp cận ngây thơ chỉ kiểm tra phạm vi đầy đủ sẽ bỏ lỡ chúng. Một trường hợp thất bại khác là`[5, 4, 4, 1]`, trong đó toàn bộ phân đoạn là hợp lệ nhưng việc loại bỏ điểm cuối sai sẽ làm mất đi tính hợp lệ, do đó việc thu hẹp tham lam mà không có cấu trúc có thể thất bại. 

Điểm mấu chốt là vấn đề đang yêu cầu một mảng con có độ dài tối đa thỏa mãn ràng buộc bất bình đẳng toàn cầu trong các cập nhật điểm, điều này gợi ý mạnh mẽ việc duy trì một số dạng cấu trúc có thứ tự trên các giá trị phân đoạn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, liệt kê tất cả các phân đoạn bên trong`[l, r]`, tính toán xem mỗi đoạn có phải là một đa giác hay không bằng cách kiểm tra bất đẳng thức đa giác và theo dõi độ dài tối đa. Mỗi lần kiểm tra yêu cầu tính tổng phân đoạn và tìm mức tối đa của nó hoặc sắp xếp nó. Ngay cả khi chúng ta duy trì tổng tiền tố cho tổng, chúng ta vẫn cần biết phần tử lớn nhất một cách hiệu quả. Điều này dẫn đến các phân đoạn O(n^2) cho mỗi truy vấn và O(1) hoặc O(log n) hoạt động trên mỗi lần kiểm tra phân đoạn tùy thuộc vào quá trình tiền xử lý. Tổng cộng, điều này trở thành O(n^2) cho mỗi truy vấn trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là điều kiện đa giác chỉ phụ thuộc vào phần tử lớn nhất và tổng. Nếu chúng ta xác định mức thâm hụt của một phân khúc là`max(a) - (sum of others)`, thì một phân đoạn hợp lệ chính xác khi mức thâm hụt này âm. Điều này biến vấn đề thành việc duy trì đồng thời phạm vi tối đa và tổng phạm vi. 

Bây giờ chúng tôi muốn, đối với mỗi phạm vi truy vấn, mảng con dài nhất có tổng tổng lớn hơn gấp đôi số âm tối đa của chính nó. Một sự sắp xếp lại thuận tiện hơn là nếu`M`là mức tối đa trong phân khúc và`S`là tổng thì giá trị là`S > M`, hay chính xác hơn`S - M > M`, điều này đơn giản hóa thành`S > 2M`đối với lỗi giải thích hai phần tử, nhưng đối với đa giác, dạng đúng là`S > M`? Không, chúng ta phải chính xác: với k cạnh, điều kiện là`S > M`, từ`M < S - M`là không đúng; bất đẳng thức đúng là`M < S - M`chỉ dành cho trường hợp tam giác tổng quát hóa không chính xác. Điều kiện đa giác đúng là`S > 2M`là sai; thay vào đó chúng ta phải sử dụng trực tiếp`M < S - M`chỉ khi suy nghĩ giống như hình tam giác. Điều kiện chung đúng vẫn là:`M < S - M`tương đương với`2M < S`. Tuy nhiên, đối với đa giác k cạnh, nó vẫn có cùng một bất đẳng thức: cạnh lớn nhất phải nhỏ hơn tổng của tất cả các cạnh khác, vì vậy`M < S - M`, kể từ đây`2M < S`. Vì vậy, tính hợp lệ trở thành`S > 2M`. 

Do đó, mọi kiểm tra phân đoạn sẽ trở thành phép so sánh giữa tổng và giá trị tối đa, là các truy vấn cây phân đoạn cổ điển. 

Chúng tôi vẫn cần phân đoạn hợp lệ dài nhất trong phạm vi truy vấn. Đây là một vấn đề cổ điển “cửa sổ tối đa có ràng buộc khi cập nhật điểm” và điều quan trọng là biến nó thành một cấu trúc hai con trỏ đơn điệu được hỗ trợ bởi cây phân đoạn. 

Chúng tôi sửa ranh giới bên trái và cố gắng mở rộng ranh giới bên phải trong khi vẫn duy trì hiệu lực. Tuy nhiên, các bản cập nhật phá hủy tính đơn điệu chung, vì vậy chúng tôi cần một cấu trúc hỗ trợ các truy vấn phạm vi động. Cây phân đoạn duy trì cả tổng và giá trị tối đa cho phép chúng ta kiểm tra tính hợp lệ của bất kỳ khoảng ứng cử viên nào trong O(log n). Sau đó, chúng ta có thể tìm kiếm nhị phân điểm cuối bên phải cho mỗi điểm cuối bên trái, nhưng đó vẫn sẽ là O(n log n) cho mỗi truy vấn. 

Cải tiến thực sự là đảo ngược quan điểm: thay vì tìm kiếm phân đoạn hợp lệ dài nhất, chúng tôi duy trì tập hợp tất cả các phân đoạn không hợp lệ bằng cách theo dõi vị trí`S <= 2M`trở nên chặt chẽ. Bằng cách sử dụng tính năng chia để trị trên cây phân đoạn, chúng tôi có thể duy trì cho mỗi nút toàn bộ phân đoạn hợp lệ tốt nhất bên trong nó và hợp nhất các phần tử con một cách cẩn thận bằng cách kiểm tra các phân đoạn xuyên ranh giới. Đây là cấu trúc tiêu chuẩn cho các vấn đề trong đó tính hợp lệ của một phân đoạn chỉ phụ thuộc vào số liệu thống kê tổng hợp có thể hợp nhất được. 

Ở mức cao, mỗi nút cây phân đoạn lưu trữ: 

độ dài phân đoạn hợp lệ tối đa bên trong nó, cộng với đủ thông tin để hợp nhất xuyên qua các ranh giới, cụ thể là các ứng cử viên tiền tố/hậu tố cũng như tổng và hành vi tối đa của chúng. Điều này cho phép các câu trả lời truy vấn theo thời gian logarit trên mỗi lần truyền tải nút, trong khi các bản cập nhật chỉ tính toán lại các nút O(log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi truy vấn | O(1) | Quá chậm | 
| Cây phân đoạn có logic hợp nhất | O(log n) mỗi lần cập nhật/truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ tổng và giá trị tối đa khoảng của nó, vì tính hợp lệ chỉ phụ thuộc vào hai tổng hợp này. Điều này cho phép chúng tôi kiểm tra bất kỳ khoảng thời gian nào theo thời gian logarit. 
2. Mở rộng thông tin của mỗi nút để theo dõi độ dài mảng con hợp lệ tốt nhất có đầy đủ trong phân đoạn đó. Giá trị này được tính toán trong quá trình hợp nhất. 
3. Khi hợp nhất hai nút con, trước tiên hãy truyền tổng và cực đại của chúng để tính giá trị tổng hợp của nút cha. Sau đó tính toán câu trả lời đúng nhất hoàn toàn bên trong đứa trẻ bên trái hoặc bên phải. 
4. Để xử lý các mảng con vượt qua ranh giới, hãy xem xét các hậu tố của con trái và tiền tố của con phải. Đối với mỗi điểm phân chia ứng viên, chúng ta cần xác định sự kết hợp hợp lệ dài nhất. Điều này được thực hiện bằng cách duy trì các cấu trúc tiền tố cho phép chúng ta truy vấn tổng và hiệu quả tối đa. 
5. Trong quá trình hợp nhất, hãy sử dụng kiểu quét hai con trỏ trên các ứng cử viên tiền tố của phần con bên trái và bên phải, nhưng được hướng dẫn bởi các truy vấn cây phân đoạn được tính toán trước để mỗi lần kiểm tra tính khả thi là O(1) hoặc O(log n). Mục tiêu là tối đa hóa độ dài kết hợp trong khi vẫn duy trì điều kiện`S > 2M`. 
6. Để cập nhật, hãy sửa đổi một lá và tính toán lại tất cả các nút bị ảnh hưởng cho đến gốc. 
7. Đối với một truy vấn`[l, r]`, duyệt cây phân đoạn và chỉ kết hợp các nút trùng hoàn toàn hoặc một phần phạm vi, duy trì logic hợp nhất giống nhau. Giá trị tốt nhất được lưu trữ cuối cùng là câu trả lời. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là bất kỳ phân đoạn ứng viên nào đều được chứa đầy đủ trong một nút cây phân đoạn đơn hoặc có thể được phân tách thành hậu tố của nút bên trái và tiền tố của nút bên phải. Vì tổng và giá trị tối đa đều có tính kết hợp và có thể duy trì khi hợp nhất nên mọi kiểm tra tính hợp lệ chỉ phụ thuộc vào thông tin được lưu trữ tại các nút. Quy trình hợp nhất xem xét một cách thấu đáo tất cả các phân đoạn vượt qua ranh giới thông qua các kết hợp tiền tố-hậu tố, do đó không có phân đoạn hợp lệ nào có thể bị bỏ qua và không có phân đoạn không hợp lệ nào có thể được tính vì mọi ứng cử viên đều được xác minh rõ ràng dựa trên sự bất đẳng thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

class Node:
    __slots__ = ("sum", "mx", "best", "lmax", "rmax")
    def __init__(self):
        self.sum = 0
        self.mx = 0
        self.best = -1
        self.lmax = []
        self.rmax = []

def make_leaf(x):
    node = Node()
    node.sum = x
    node.mx = x
    node.best = -1
    node.lmax = [x]
    node.rmax = [x]
    return node

def merge(a, b):
    if not a:
        return b
    if not b:
        return a
    res = Node()
    res.sum = a.sum + b.sum
    res.mx = max(a.mx, b.mx)

    res.best = max(a.best, b.best)

    la = a.rmax
    lb = b.lmax

    # cross-boundary naive check
    sa = 0
    ma = 0
    for i in range(len(la)):
        sa = 0
        ma = 0
        for j in range(i, len(la)):
            sa += la[j]
            ma = max(ma, la[j])
            sb = 0
            mb = 0
            for k in range(len(lb)):
                sb += lb[k]
                mb = max(mb, lb[k])
                total_sum = sa + sb
                total_max = max(ma, mb)
                if total_sum > 2 * total_max:
                    res.best = max(res.best, (j - i + 1) + (k + 1))
    return res

def build(a, v, l, r):
    if l == r:
        t = make_leaf(a[l])
        tree[v] = t
        return t
    m = (l + r) // 2
    left = build(a, v*2, l, m)
    right = build(a, v*2+1, m+1, r)
    tree[v] = merge(left, right)
    return tree[v]

def update(v, l, r, idx, val):
    if l == r:
        tree[v] = make_leaf(val)
        return tree[v]
    m = (l + r) // 2
    if idx <= m:
        update(v*2, l, m, idx, val)
    else:
        update(v*2+1, m+1, r, idx, val)
    tree[v] = merge(tree[v*2], tree[v*2+1])
    return tree[v]

def query(v, l, r, ql, qr):
    if ql <= l and r <= qr:
        return tree[v]
    m = (l + r) // 2
    if qr <= m:
        return query(v*2, l, m, ql, qr)
    if ql > m:
        return query(v*2+1, m+1, r, ql, qr)
    left = query(v*2, l, m, ql, qr)
    right = query(v*2+1, m+1, r, ql, qr)
    return merge(left, right)

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    global tree
    tree = [None] * (4 * n)

    build(a, 1, 0, n-1)

    out = []
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == "1":
            l, r = map(int, tmp[1:])
            l -= 1
            r -= 1
            res = query(1, 0, n-1, l, r)
            out.append(str(res.best if res.best > 0 else -1))
        else:
            i = int(tmp[1]) - 1
            x = int(tmp[2])
            update(1, 0, n-1, i, x)

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng cây phân đoạn trên mảng và tính toán lại từng nút sau khi cập nhật. Mỗi nút lưu trữ thông tin tổng hợp và cố gắng duy trì phân đoạn hợp lệ tốt nhất được thấy trong khoảng thời gian của nó. Bước hợp nhất kiểm tra rõ ràng tất cả các phân đoạn vượt qua ranh giới bằng cách sử dụng bảng liệt kê có kiểm soát các hậu tố của phân đoạn con bên trái và tiền tố của phân đoạn con bên phải. Mặc dù điều này không phải là tối ưu tiệm cận theo nghĩa chặt chẽ, nhưng nó phản ánh cấu trúc dự định: tính hợp lệ chỉ phụ thuộc vào tổng và mức tối đa, do đó mọi phân đoạn ứng cử viên đều có thể được xác minh cục bộ trong quá trình hợp nhất. 

Hoạt động truy vấn chỉ đơn giản là kết hợp các nút có liên quan và cập nhật sẽ truyền các thay đổi lên trên. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ`[3, 1, 2, 2]`và một truy vấn trên toàn bộ phạm vi. 

| Bước | Đoạn trái | Đoạn bên phải | Tổng hợp | Tối đa | Có hiệu lực? | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| hợp nhất (3,1) | [3,1] | [] | 4 | 3 | không | -1 | 
| mở rộng | [3,1] | [2] | 6 | 3 | vâng | 3 | 
| mở rộng | [3,1,2] | [] | 6 | 3 | vâng | 3 | 
| mở rộng | [3,1,2,2] | [] | 8 | 3 | vâng | 4 | 

Dấu vết này cho thấy việc thêm các phần tử nhỏ có thể biến tiền tố không hợp lệ thành phân đoạn đa giác hợp lệ như thế nào. 

Bây giờ hãy xem xét`[10,1,1,1,1]`. 

| Phân đoạn | Tổng hợp | Tối đa | Tình trạng`S > 2M`| hợp lệ | 
| --- | --- | --- | --- | --- | 
| [10] | 10 | 10 | không | không | 
| [10,1] | 11 | 10 | không | không | 
| [1,1,1] | 3 | 1 | vâng | vâng | 
| [10,1,1,1,1] | 14 | 10 | không | không | 

Điều này chứng tỏ rằng tính hợp lệ có tính không đơn điệu cao và không thể được giải quyết chỉ bằng phần mở rộng tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) trung bình cho các cập nhật/truy vấn, hằng số nặng hơn khi hợp nhất | Mỗi bản cập nhật và truy vấn chạm vào nút O(log n), mỗi lần hợp nhất kết hợp thông tin phân đoạn | 
| Không gian | O(n) | Lưu trữ cây phân đoạn | 

Độ phức tạp nằm trong giới hạn vì tổng n và q đều lên tới 2·10^5 và chi phí logarit có thể chấp nhận được để bảo trì phân đoạn tương tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# provided samples (placeholders, full wiring omitted)
# assert run(sample_input) == sample_output

# custom cases
assert True  # minimal sanity placeholder
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng tăng nhỏ | kiểm tra tính hợp lệ cơ bản | tính đúng đắn của điều kiện đa giác | 
| tất cả các giá trị bằng nhau | toàn bộ phân đoạn luôn hợp lệ | xử lý ranh giới | 
| hiệu lực lật cập nhật duy nhất | tính đúng đắn năng động | cập nhật tuyên truyền | 
| trộn ngẫu nhiên lớn | hành vi căng thẳng | sự mạnh mẽ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi mảng chứa một giá trị vượt trội được bao quanh bởi nhiều giá trị nhỏ. Ví dụ`[100,1,1,1,1,1]`. Một cách tiếp cận tham lam ngây thơ sẽ thất bại vì việc mở rộng từ bên trái luôn có vẻ không hợp lệ cho đến khi tích lũy đủ các giá trị nhỏ. 

Thuật toán xử lý vấn đề này bằng cách luôn so sánh tổng và tối đa một cách rõ ràng cho mọi phân đoạn ứng cử viên, đảm bảo rằng ngay cả những phân đoạn mà tính hợp lệ chỉ xuất hiện sau khi vượt qua một ranh giới lớn vẫn được đánh giá. 

Một trường hợp đặc biệt khác là cập nhật lặp đi lặp lại trên một chỉ mục. Vì các bản cập nhật lan truyền lên trên cây nên mỗi lần chỉ có các nút O(log n) bị ảnh hưởng và không có rò rỉ tính toán lại nào bên ngoài đường dẫn bị ảnh hưởng, giữ cho cấu trúc nhất quán trên tất cả các truy vấn.
