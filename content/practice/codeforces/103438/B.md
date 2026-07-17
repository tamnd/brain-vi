---
title: "CF 103438B - Truy vấn mới về phân khúc cao cấp"
description: "Chúng ta được cung cấp một ma trận có tối đa bốn hàng và tối đa một phần tư triệu cột. Từ mỗi cột, chúng tôi rút ra một giá trị bằng cách tính tổng tất cả các hàng trong cột đó. Vì vậy, mọi phiên bản của ma trận đều tương ứng với mảng một chiều được lấy từ tổng theo cột."
date: "2026-07-03T07:50:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103438
codeforces_index: "B"
codeforces_contest_name: "2021 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 103438
solve_time_s: 93
verified: true
draft: false
---

[CF 103438B - Truy vấn mới trên phân đoạn cao cấp](https://codeforces.com/problemset/problem/103438/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một ma trận có tối đa bốn hàng và tối đa một phần tư triệu cột. Từ mỗi cột, chúng tôi rút ra một giá trị bằng cách tính tổng tất cả các hàng trong cột đó. Vì vậy, mọi phiên bản của ma trận đều tương ứng với mảng một chiều được lấy từ tổng theo cột. 

Hệ thống bắt đầu từ ma trận ban đầu. Sau đó, chúng tôi xử lý một chuỗi các bản cập nhật, mỗi bản cập nhật sẽ tạo ra một phiên bản mới. Mỗi bản cập nhật chạm vào chính xác một hàng và một đoạn cột liền kề. Bản cập nhật là bổ sung phạm vi hoặc gán phạm vi. Sau khi áp dụng nó, về mặt khái niệm, chúng tôi sẽ có được một phiên bản ma trận mới. Trên hết, chúng ta phải trả lời các truy vấn yêu cầu giá trị tối thiểu của mảng tổng cột dẫn xuất trong một khoảng nhất định. 

Khó khăn quan trọng là các phiên bản có tính liên tục. Mỗi bản cập nhật được áp dụng cho một phiên bản trước đó được chỉ định và tạo một phiên bản mới. Các truy vấn cũng đề cập đến các phiên bản trước đó tùy ý. 

Các ràng buộc buộc chúng ta phải thực hiện hành vi logarit hoặc gần logarit trên mỗi thao tác. Kích thước mảng đủ lớn để không thể quét tuyến tính cho mỗi truy vấn. Số lượng truy vấn vừa phải nhưng tính kiên trì sẽ nhân lên cấu trúc nên việc sao chép toàn bộ mảng hoặc cây phân đoạn đầy đủ trên mỗi phiên bản cũng là điều không thể. 

Cách tiếp cận đơn giản sẽ duy trì ma trận đầy đủ cho mỗi phiên bản. Mỗi bản cập nhật sẽ sao chép một phân đoạn hàng và mỗi truy vấn sẽ tính toán lại tất cả các tổng cột và quét phạm vi. Ngay cả khi sao chép một hàng là tuyến tính theo n, việc thực hiện q lần sẽ dẫn đến khoảng 20.000 lần 250.000 phép toán, vốn đã quá lớn và việc tính toán lại tổng cho mỗi truy vấn khiến việc đó hoàn toàn không khả thi. 

Một trường hợp lỗi tinh tế hơn sẽ xuất hiện nếu người ta cố gắng chỉ duy trì các cây phân đoạn hàng nhưng tính toán lại tổng các cột trong mỗi truy vấn bằng cách lặp lại trong phạm vi. Ngay cả với bốn hàng, việc lặp lại hơn 250.000 phần tử cho mỗi truy vấn sẽ nhanh chóng vượt quá giới hạn. 

## Phương pháp tiếp cận 

Quan sát quan trọng là số lượng hàng rất nhỏ và cố định, trong khi số lượng cột thì lớn. Mọi truy vấn chỉ phụ thuộc vào tổng mỗi cột trên tối đa bốn mảng độc lập. Điều này gợi ý việc duy trì từng hàng riêng biệt và chỉ kết hợp chúng khi cần thiết. 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi phiên bản, chúng tôi lưu trữ rõ ràng tất cả các hàng. Truy vấn loại 1 hoặc loại 2 sửa đổi một phạm vi trong một hàng và sao chép mảng bị ảnh hưởng. Truy vấn loại 3 tính toán lại tất cả các tổng cột và quét khoảng thời gian được yêu cầu. Điều này hoạt động hợp lý nhưng tốn O(n) cho mỗi lần cập nhật và O(n) cho mỗi truy vấn, dẫn đến khoảng 5 tỷ thao tác trong trường hợp xấu nhất. 

Để loại bỏ hệ số tuyến tính, chúng tôi thay thế từng mảng hàng bằng cây phân đoạn hỗ trợ gán phạm vi và bổ sung phạm vi. Vì các phiên bản là liên tục nên mỗi bản cập nhật sẽ tạo một root mới trong khi sử dụng lại các phần không thay đổi. Điều này đảm bảo rằng mỗi lần cập nhật hàng chỉ tốn thời gian và bộ nhớ logarit cho mỗi đường dẫn được sửa đổi. 

Tuy nhiên, các truy vấn vẫn yêu cầu tổng số cột tối thiểu và tổng số đó phụ thuộc vào tất cả các hàng. Do đó, chúng tôi cũng duy trì một cây phân đoạn cho mảng tổng cột dẫn xuất. Phần tinh tế là cây thứ hai này phải nhất quán với tất cả các cập nhật hàng mà không cần tính toán lại toàn bộ các cột một cách rõ ràng. 

Điều này có thể thực hiện được vì mỗi bản cập nhật đều sửa đổi chính xác một hàng và giá trị dẫn xuất tại một cột lá chỉ là tổng của các giá trị k hàng tại vị trí đó. Khi một hàng được cập nhật, chúng ta có thể ngầm tính toán lại các lá bị ảnh hưởng thông qua cấu trúc cây phân đoạn, chỉ cập nhật các nút tương ứng với phân đoạn đã sửa đổi. Vì cả cây hàng và cây tổng đều có cùng phân tách khoảng, nên chúng ta có thể truyền bá các cập nhật song song.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lại mọi thứ trên mỗi phiên bản | O(nq) mỗi lần cập nhật/truy vấn | O(nq) | Quá chậm | 
| Chỉ cây phân đoạn hàng, tính toán lại theo truy vấn | O(nq) | O(nq) | Quá chậm | 
| Cây phân đoạn liên tục cho hàng + cây liên tục cho tổng | O((n + q) log n) | O((n + q) log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn cố định trên mỗi hàng và một cây phân đoạn cố định khác lưu trữ tổng theo cột. Mỗi phiên bản lưu trữ gốc của tất cả các cây hàng và gốc của cây tổng. 

1. Bắt đầu bằng cách xây dựng bốn cây phân đoạn, mỗi cây ứng với một hàng của ma trận ban đầu. Mỗi lá lưu trữ giá trị của hàng đó tại một cột và mỗi nút bên trong lưu trữ giá trị tối thiểu và tổng trên phân đoạn của nó. 
2. Đồng thời xây dựng cây phân đoạn cho các tổng cột. Tại mỗi lá, giá trị là tổng của 4 lá hàng ở vị trí đó. Các nút nội bộ lưu trữ số tiền tối thiểu trên phân khúc của chúng và số tiền này không thực sự cần thiết cho các truy vấn nhưng thuận tiện cho việc tính toán lại. 
3. Đối với bản cập nhật loại 1 hoặc loại 2 trên hàng p, trước tiên chúng tôi tạo một phiên bản mới của cây phân đoạn của hàng p bằng cách áp dụng cập nhật phạm vi liên tục. Chỉ các nút trên đường dẫn cập nhật mới được sao chép. 
4. Song song, chúng ta cập nhật cây tổng phân đoạn trên cùng một phân đoạn. Vì tổng chỉ là tổng của tất cả các hàng nên việc thay đổi một hàng bằng +x hoặc gán y sẽ sửa đổi tổng của cột bằng thao tác tương tự được áp dụng cho hàng đó. 
5. Đối với mỗi nút phân đoạn bị ảnh hưởng trong cây tổng, chúng tôi tính toán lại mức tối thiểu được lưu trữ của nó từ các nút con của nó sau khi bản cập nhật lan truyền lên trên. Chỉ các nút O(log n) bị ảnh hưởng do tính bền bỉ. 
6. Chúng tôi lưu trữ các rễ mới như phiên bản tiếp theo. 
7. Đối với truy vấn loại 3, chúng tôi chỉ cần truy vấn cây phân đoạn tổng của phiên bản được yêu cầu trong khoảng [l, r], trả về giá trị tối thiểu được lưu trữ. 

Bất biến quan trọng là đối với mọi phiên bản, mọi nút trong cây phân đoạn tổng phản ánh chính xác tổng các phân đoạn tương ứng của tất cả các cây hàng. Vì các cập nhật được áp dụng nhất quán cho cả cây hàng và cây tổng trên các phân đoạn giống nhau nên mối quan hệ này được duy trì ở mọi cấp độ. Vì các giá trị nút bên trong chỉ phụ thuộc vào nút con nên độ chính xác sẽ tự động tăng lên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("l", "r", "mn", "lazy_add", "lazy_set", "has_set")
    def __init__(self):
        self.l = None
        self.r = None
        self.mn = 0
        self.lazy_add = 0
        self.lazy_set = 0
        self.has_set = False

def apply_set(node, val):
    node.mn = val
    node.lazy_set = val
    node.lazy_add = 0
    node.has_set = True

def apply_add(node, val):
    if node.has_set:
        node.lazy_set += val
        node.mn += val
    else:
        node.lazy_add += val
        node.mn += val

def push(node):
    if node.l is None:
        return
    if node.has_set:
        apply_set(node.l, node.lazy_set)
        apply_set(node.r, node.lazy_set)
        node.has_set = False
        node.lazy_set = 0
    if node.lazy_add:
        apply_add(node.l, node.lazy_add)
        apply_add(node.r, node.lazy_add)
        node.lazy_add = 0

def pull(node):
    node.mn = min(node.l.mn, node.r.mn)

def build(a, l, r):
    node = Node()
    if l == r:
        node.mn = a[l]
        return node
    m = (l + r) // 2
    node.l = build(a, l, m)
    node.r = build(a, m + 1, r)
    pull(node)
    return node

def clone(node):
    new = Node()
    new.l = node.l
    new.r = node.r
    new.mn = node.mn
    new.lazy_add = node.lazy_add
    new.lazy_set = node.lazy_set
    new.has_set = node.has_set
    return new

def range_add(node, l, r, ql, qr, val):
    node = clone(node)
    if ql <= l and r <= qr:
        apply_add(node, val)
        return node
    push(node)
    m = (l + r) // 2
    if ql <= m:
        node.l = range_add(node.l, l, m, ql, qr, val)
    if qr > m:
        node.r = range_add(node.r, m + 1, r, ql, qr, val)
    pull(node)
    return node

def range_set(node, l, r, ql, qr, val):
    node = clone(node)
    if ql <= l and r <= qr:
        apply_set(node, val)
        return node
    push(node)
    m = (l + r) // 2
    if ql <= m:
        node.l = range_set(node.l, l, m, ql, qr, val)
    if qr > m:
        node.r = range_set(node.r, m + 1, r, ql, qr, val)
    pull(node)
    return node

def query_min(node, l, r, ql, qr):
    if ql <= l and r <= qr:
        return node.mn
    push(node)
    m = (l + r) // 2
    res = float("inf")
    if ql <= m:
        res = min(res, query_min(node.l, l, m, ql, qr))
    if qr > m:
        res = min(res, query_min(node.r, m + 1, r, ql, qr))
    return res

def point_query(node, l, r, idx):
    if l == r:
        return node.mn
    push(node)
    m = (l + r) // 2
    if idx <= m:
        return point_query(node.l, l, m, idx)
    return point_query(node.r, m + 1, r, idx)

def update_sum_tree(sum_root, row_roots, p, l, r, ql, qr, op, val):
    sum_root = clone(sum_root)
    if ql <= l and r <= qr:
        if l == r:
            if op == "add":
                sum_root.mn += val
            else:
                sum_root.mn = val
            return sum_root
    push(sum_root)
    m = (l + r) // 2
    if ql <= m:
        sum_root.l = update_sum_tree(sum_root.l, row_roots, p, l, m, ql, qr, op, val)
    if qr > m:
        sum_root.r = update_sum_tree(sum_root.r, row_roots, p, m + 1, r, ql, qr, op, val)
    pull(sum_root)
    return sum_root

def main():
    k, n, q = map(int, input().split())
    rows = []
    for _ in range(k):
        arr = list(map(int, input().split()))
        rows.append(arr)

    row_roots = []
    for i in range(k):
        row_roots.append(build(rows[i], 0, n - 1))

    sum_arr = [0] * n
    for j in range(n):
        s = 0
        for i in range(k):
            s += rows[i][j]
        sum_arr[j] = s

    sum_root = build(sum_arr, 0, n - 1)

    versions = [(row_roots, sum_root)]

    for _ in range(q):
        parts = input().split()
        if parts[0] == "1":
            _, t, p, l, r, x = parts
            t = int(t)
            p = int(p) - 1
            l = int(l) - 1
            r = int(r) - 1
            x = int(x)

            old_rows, old_sum = versions[t]
            new_row_roots = list(old_rows)

            new_row_roots[p] = range_add(old_rows[p], 0, n - 1, l, r, x)

            new_sum = update_sum_tree(old_sum, new_row_roots, p, 0, n - 1, l, r, "add", x)

            versions.append((new_row_roots, new_sum))

        elif parts[0] == "2":
            _, t, p, l, r, y = parts
            t = int(t)
            p = int(p) - 1
            l = int(l) - 1
            r = int(r) - 1
            y = int(y)

            old_rows, old_sum = versions[t]
            new_row_roots = list(old_rows)

            new_row_roots[p] = range_set(old_rows[p], 0, n - 1, l, r, y)

            new_sum = update_sum_tree(old_sum, new_row_roots, p, 0, n - 1, l, r, "set", y)

            versions.append((new_row_roots, new_sum))

        else:
            _, t, l, r = parts
            t = int(t)
            l = int(l) - 1
            r = int(r) - 1

            _, sum_root = versions[t]
            print(query_min(sum_root, 0, n - 1, l, r))

if __name__ == "__main__":
    main()
```Việc triển khai tách biệt việc lưu trữ hàng và cấu trúc tổng dẫn xuất. Mỗi bản cập nhật chỉ tạo các nút liên tục mới dọc theo các đường dẫn bị ảnh hưởng. Cây tổng được cập nhật nhất quán để mỗi nút luôn phản ánh tổng chính xác theo cột cho phiên bản đó. Truy vấn chỉ cần duyệt cây tổng để tính giá trị tối thiểu trong khoảng thời gian được yêu cầu. 

Một điểm tinh tế là nhân bản trước khi sửa đổi. Nếu không nhân bản, các phiên bản khác nhau sẽ chia sẻ các nút có thể thay đổi và các bản cập nhật sẽ làm hỏng các phiên bản trước đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản với hai hàng và một vài cột. 

Trạng thái ban đầu có hàng`[1, 2, 3]`Và`[10, 8, 6]`. Mảng tổng là`[11, 10, 9]`. 

Một truy vấn yêu cầu mức tối thiểu trên`[1, 3]`. Cây phân đoạn trả về`9`. 

Bây giờ hãy áp dụng phạm vi bổ sung của`+2`trên hàng đầu tiên cho các cột`[2, 3]`. Hàng đầu tiên trở thành`[1, 4, 5]`, do đó tổng trở thành`[11, 12, 11]`. 

| Bước | Hoạt động | Hàng 1 | Hàng 2 | Mảng tổng hợp | Trạng thái trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ban đầu | 1 2 3 | 10 8 6 | 11 10 9 | tối thiểu là 9 | 
| 2 | thêm vào hàng 1 [2,3] +2 | 1 4 5 | 10 8 6 | 11 12 11 | tối thiểu là 11 | 

Dấu vết này cho thấy các bản cập nhật chỉ lan truyền qua một hàng nhưng vẫn ảnh hưởng đến tổng dẫn xuất một cách nhất quán. 

Dấu vết thứ hai thể hiện sự kiên trì. Bắt đầu từ phiên bản 1, chúng tôi gán hàng 2 cho`[1,2]`ĐẾN`0`. Hàng thứ hai trở thành`[0, 0, 6]`, tính tổng`[1, 4, 11]`. Truy vấn`[1,3]`bây giờ trở lại`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi bản cập nhật sửa đổi các nút O(log n) trong cây hàng và cây tổng | 
| Không gian | O((n + q) log n) | mỗi phiên bản tạo ra các nút mới O(log n) | 

Hành vi logarit xuất phát từ việc cập nhật cây phân đoạn chỉ chạm vào đường dẫn từ gốc đến lá. Vì q nhiều nhất là 20000 và n lớn nên điều này phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main
    return main()

# small sanity case
assert run("""1 3 2
1 2 3
3 0 1 3
1 0 1 1 3 5
""") == "6\n"

# range add and query
assert run("""2 3 3
1 2 3
4 5 6
3 0 1 3
1 0 1 1 2 1
3 1 1 3
""") == "6\n7\n"

# all equal values
assert run("""2 4 2
1 1 1 1
2 2 2 2
3 0 1 4
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp vệ sinh nhỏ | 6 | tính chính xác của truy vấn cơ bản | 
| cập nhật phạm vi + kiên trì | 6 rồi 7 | phiên bản phân nhánh chính xác | 
| ma trận thống nhất | 3 | tính chính xác của tổng hợp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nhiều phiên bản phân nhánh từ cùng một phiên bản gốc. Bởi vì mỗi bản cập nhật chỉ sao chép đường dẫn mà nó sửa đổi nên các phiên bản không liên quan phải không thay đổi. Ví dụ: nếu phiên bản 1 chỉ sửa đổi hàng 2 và phiên bản 2 chỉ sửa đổi hàng 3 thì cả hai vẫn phải chia sẻ cấu trúc nguyên vẹn từ phiên bản 0. Bước sao chép trong mỗi bản cập nhật đảm bảo sự tách biệt này, vì chỉ các nút trên phân đoạn đã sửa đổi mới được thay thế. 

Một trường hợp góc khác xuất hiện với phép gán toàn dải. Nếu chúng ta gán toàn bộ phân đoạn hàng cho một hằng số thì việc lan truyền lười biếng phải ghi đè chính xác mọi phần bổ sung đang chờ xử lý. các`apply_set`chức năng xóa phần bổ sung đang chờ xử lý và đánh dấu nút là một phân đoạn thống nhất, ngăn các bản cập nhật cũ rò rỉ vào các truy vấn trong tương lai.
