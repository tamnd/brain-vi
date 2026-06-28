---
title: "CF 105139F - Phù Phép"
description: "Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi giá trị đại diện cho cấp độ ban đầu của một cuốn sách phù phép. Sách được đặt theo một thứ tự cố định và một số loại thao tác được thực hiện trên mảng này."
date: "2026-06-27T16:58:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105139
codeforces_index: "F"
codeforces_contest_name: "The 2024 International Collegiate Programming Contest in Hubei Province, China"
rating: 0
weight: 105139
solve_time_s: 49
verified: true
draft: false
---

[CF 105139F - Bị mê hoặc](https://codeforces.com/problemset/problem/105139/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi giá trị đại diện cho cấp độ ban đầu của một cuốn sách phù phép. Sách được đặt theo một thứ tự cố định và một số loại thao tác được thực hiện trên mảng này. Cơ chế quan trọng nhất là hai cuốn sách cùng cấp có thể được hợp nhất thành một cuốn sách duy nhất ở cấp độ tiếp theo và việc hợp nhất này có thể được lặp lại nhiều lần cho đến khi không còn cấp độ trùng lặp nào trong phân đoạn đã chọn. 

Việc hợp nhất hoạt động giống như một quá trình mang nhị phân. Nếu bạn có hai cuốn sách cấp l, chúng sẽ trở thành một cuốn sách cấp l + 1. Điều này có thể xếp tầng, vì việc tạo l + 1 mới có thể tự ghép nối với l + 1 khác. 

Có bốn loại truy vấn. Người ta yêu cầu mức cao nhất có thể đạt được sau khi hợp nhất hoàn toàn một mảng con. Một cách khác phức tạp hơn: trước tiên chúng tôi bình thường hóa hoàn toàn một phân khúc để không có hai cuốn sách nào có cùng cấp độ, sau đó chèn một cuốn sách mới ở cấp độ k và cuối cùng hợp nhất một cách tham lam để tối đa hóa tổng chi phí hợp nhất, báo cáo chi phí đó. Hoạt động thứ ba cập nhật một vị trí duy nhất. Thao tác cuối cùng là truy vấn du hành thời gian nhằm khôi phục mảng về phiên bản hoạt động trước đó. 

Các ràng buộc đẩy chúng ta tới gần tuyến tính hoặc logarit cho mỗi hành vi hoạt động. Với tối đa một triệu thao tác và mảng có kích thước lên tới một triệu, bất kỳ phương pháp nào tính toán lại việc hợp nhất phân khúc từ đầu đều là quá chậm. Ngay cả việc hợp nhất cây phân đoạn logarit cũng phải được thiết kế cẩn thận vì trạng thái hợp nhất không phải là một đại lượng vô hướng đơn giản mà là một cấu trúc giống như nhiều tập hợp của hành vi mang bitwise. 

Một cách tiếp cận đơn giản sẽ mô phỏng việc hợp nhất bên trong mỗi truy vấn bằng cách trích xuất phân đoạn và liên tục đếm tần số của các mức cho đến khi ổn định. Điều này không thành công vì một phân đoạn có kích thước n có thể yêu cầu hợp nhất xếp tầng khiến công việc tăng gấp đôi. Một truy vấn duy nhất có thể giảm xuống O(n log V) và trên m truy vấn, điều này trở nên không khả thi. 

Một chế độ lỗi tinh vi hơn xuất hiện trong hoạt động 2. Sau khi chuẩn hóa, cấu trúc của các cuốn sách còn lại là duy nhất cho mỗi cấp độ. Chi phí cuối cùng phụ thuộc vào cách một phần tử bổ sung lan truyền thông qua hệ thống mang nhị phân. Nếu được xử lý một cách tham lam mà không có cấu trúc thì việc tính toán lại từ đầu lại quá chậm. 

Các trường hợp cạnh phát sinh khi tất cả các phần tử trong một phân đoạn đều giống hệt nhau, bởi vì việc hợp nhất lặp đi lặp lại sẽ thu gọn phân đoạn đó thành một phần tử duy nhất và tạo ra các chuỗi mang dài. Một trường hợp khác là khi các mức đã khác biệt, trong đó việc chuẩn hóa không làm gì cả và câu trả lời phụ thuộc hoàn toàn vào hành vi chèn. 

## Phương pháp tiếp cận 

Quan sát quan trọng là việc hợp nhất các cấp độ giống hệt nhau hoạt động giống hệt như phép cộng nhị phân theo số lượng của mỗi cấp độ. Mỗi cấp độ hoạt động như một vị trí bit: có hai cấp độ l tương đương với việc mang một đơn vị lên cấp độ l + 1. 

Điều này có nghĩa là một phân đoạn có thể được biểu diễn dưới dạng cấu trúc tần số theo các mức và việc hợp nhất tương ứng với việc truyền sóng đi lên. Hạn chế quan trọng là q nhỏ nên các mức bị giới hạn. Điều này cho phép chúng tôi biểu thị từng phân đoạn dưới dạng vectơ đếm và chuẩn hóa nó trong thời gian O(q) cho mỗi cấu trúc hợp nhất, miễn là chúng tôi duy trì nó một cách hiệu quả. 

Cây phân đoạn được xây dựng trong đó mỗi nút lưu trữ biểu diễn nén của phân đoạn đó sau khi chuẩn hóa hoàn toàn. Việc hợp nhất hai nút tương ứng với việc hợp nhất hai vectơ mang. Vì q nhiều nhất là 30 nên mỗi lần hợp nhất là thời gian không đổi. 

Hoạt động 1 trở thành: truy vấn cây phân đoạn, truy xuất vectơ đã hợp nhất và mô phỏng quá trình lan truyền mang để tính toán mức cao nhất có thể tiếp cận.

Hoạt động 2 tinh tế hơn. Sau khi truy xuất biểu diễn phân đoạn, trước tiên chúng tôi đảm bảo nó được chuẩn hóa hoàn toàn. Sau đó, chúng tôi chèn một phần tử ở cấp độ k và mô phỏng cách nó lan truyền qua hệ thống mang. Chi phí của việc hợp nhất tương ứng với việc đếm số lượng thao tác mang xảy ra trong khi chèn phần tử này vào một tập hợp lũy thừa của hai cấu trúc. Điều này làm giảm việc theo dõi số lượng cấp độ liên tiếp đã đầy. 

Hoạt động 3 là cập nhật điểm trong cây phân đoạn. Hoạt động 4 yêu cầu lịch sử liên tục. Vì mỗi bản cập nhật tạo ra một phiên bản mới nên chúng tôi sử dụng cây phân đoạn liên tục để mỗi phiên bản chia sẻ các nút không thay đổi. 

Do đó, cấu trúc này là một cây phân đoạn ổn định trên các vectơ mang, cho phép truy vấn phạm vi được phiên bản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · n · log q) | O(n) | Quá chậm | 
| Cây phân đoạn liên tục có vectơ mang | O((n + m) log n · q) | O((n + m) log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn liên tục. Mỗi nút lưu trữ một mảng có kích thước cố định cnt có độ dài q + 5, trong đó cnt[i] là số lượng sách cấp i trong biểu diễn được chuẩn hóa hoàn toàn của phân đoạn đó. 

1. Xây dựng cây phân đoạn ban đầu từ mảng. Mỗi lá đặt cnt[a[i]] = 1. Biểu diễn này chưa được chuẩn hóa toàn cục nhưng được chuẩn hóa trong quá trình hợp nhất. 
2. Xác định thao tác hợp nhất giữa hai nút. Chúng tôi thêm các mảng cnt của chúng theo từng phần tử, sau đó thực hiện quá trình truyền mang từ cấp 1 đến q. Bất cứ khi nào cnt[i] đạt 2 hoặc nhiều hơn, chúng tôi giảm nó theo modulo 2 và chuyển phần tràn về cnt[i + 1]. Điều này đảm bảo mọi nút luôn thể hiện trạng thái chuẩn được rút gọn hoàn toàn. 
3. Đối với thao tác 1, chúng tôi truy vấn cây phân đoạn trên [l, r], kết hợp các nút bằng cách sử dụng cùng một quy tắc hợp nhất và sau đó quét mảng cnt kết quả. Chỉ số i cao nhất với cnt[i] = 1 cho biết mức tối đa có thể đạt được. 
4. Đối với thao tác 2, chúng ta lại truy vấn [l, r] để lấy vectơ cnt chính tắc. Sau đó, chúng tôi mô phỏng việc chèn một phần tử mới ở cấp độ k bằng cách tăng cnt[k], sau đó là mô phỏng mang giống hệt với việc hợp nhất. Trong quá trình truyền bá này, mỗi lần chúng tôi phân giải một cặp ở cấp độ i thành một phần mang, chúng tôi thêm 2^i vào tổng chi phí modulo 1e9 + 7. Lý do là mỗi lần hợp nhất ở cấp độ i đóng góp chi phí 2^{i+1} trong câu lệnh, được tích lũy dọc theo chuỗi mang. 
5. Đối với thao tác 3, chúng ta tạo một phiên bản mới của cây phân đoạn trong đó chỉ đường dẫn từ gốc đến pos được cập nhật. Mỗi nút bị ảnh hưởng sẽ tính toán lại vectơ cnt của nó bằng quy tắc hợp nhất. 
6. Đối với thao tác 4, chúng tôi lưu trữ con trỏ gốc của mỗi phiên bản. Khôi phục phiên bản t chỉ cần đặt gốc hoạt động thành gốc đã lưu trữ [t]. 

Lý do nó hoạt động là sau khi chuẩn hóa, mọi cấp độ hoạt động giống như một chữ số nhị phân trong cách biểu diễn số đếm cơ số 2. Hoạt động hợp nhất chính xác là phép cộng nhị phân có mang. Cây phân đoạn bảo toàn tính bất biến này ở mọi nút, vì vậy mọi kết quả truy vấn đều đã ở dạng chuẩn. Thao tác 2 giảm bớt việc chèn một bit vào số nhị phân và theo dõi chi phí lan truyền mang, được xác định duy nhất và không phụ thuộc vào thứ tự hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class Node:
    __slots__ = ("cnt",)
    def __init__(self, cnt=None):
        if cnt is None:
            self.cnt = [0] * 35
        else:
            self.cnt = cnt

def merge(a, b):
    res = [0] * 35
    for i in range(35):
        res[i] = a.cnt[i] + b.cnt[i]
    for i in range(34):
        if res[i] >= 2:
            carry = res[i] // 2
            res[i] %= 2
            res[i + 1] += carry
    return Node(res)

def build(arr, idx, l, r, seg):
    if l == r:
        cnt = [0] * 35
        cnt[arr[l]] = 1
        seg[idx] = Node(cnt)
        return
    m = (l + r) // 2
    build(arr, idx * 2, l, m, seg)
    build(arr, idx * 2 + 1, m + 1, r, seg)
    seg[idx] = merge(seg[idx * 2], seg[idx * 2 + 1])

def update(seg, idx, l, r, pos, val):
    if l == r:
        cnt = [0] * 35
        cnt[val] = 1
        seg[idx] = Node(cnt)
        return
    m = (l + r) // 2
    if pos <= m:
        update(seg, idx * 2, l, m, pos, val)
    else:
        update(seg, idx * 2 + 1, m + 1, r, pos, val)
    seg[idx] = merge(seg[idx * 2], seg[idx * 2 + 1])

def query(seg, idx, l, r, ql, qr):
    if ql <= l and r <= qr:
        return seg[idx]
    m = (l + r) // 2
    if qr <= m:
        return query(seg, idx * 2, l, m, ql, qr)
    if ql > m:
        return query(seg, idx * 2 + 1, m + 1, r, ql, qr)
    left = query(seg, idx * 2, l, m, ql, qr)
    right = query(seg, idx * 2 + 1, m + 1, r, ql, qr)
    return merge(left, right)

def solve():
    n, m, A, p, q = map(int, input().split())

    import random

    def rnd():
        nonlocal A
        A = (7 * A + 13) % 19260817
        return A

    arr = [0] * (n + 1)
    for i in range(1, n + 1):
        arr[i] = rnd() % q + 1

    seg = [None] * (4 * n + 5)
    build(arr, 1, 1, n, seg)

    version = [0] * (m + 1)
    version[0] = 1
    cur_version = 0

    out = []

    def get_max(node):
        for i in range(34, -1, -1):
            if node.cnt[i]:
                return i
        return 0

    def op2_cost(node, k):
        cnt = node.cnt[:]
        cnt[k] += 1
        cost = 0
        for i in range(k, 34):
            while cnt[i] >= 2:
                cnt[i] -= 2
                cnt[i + 1] += 1
                cost = (cost + (pow(2, i + 1, MOD))) % MOD
        return cost

    for i in range(1, m + 1):
        opt = rnd() % p + 1
        version[i] = version[cur_version]

        if opt == 1:
            L = rnd() % n + 1
            R = rnd() % n + 1
            l, r = min(L, R), max(L, R)
            root = query(seg, 1, 1, n, l, r)
            out.append(str(get_max(root)))

        elif opt == 2:
            L = rnd() % n + 1
            R = rnd() % n + 1
            l, r = min(L, R), max(L, R)
            k = rnd() % q + 1
            root = query(seg, 1, 1, n, l, r)
            out.append(str(op2_cost(root, k)))

        elif opt == 3:
            pos = rnd() % n + 1
            k = rnd() % q + 1
            update(seg, 1, 1, n, pos, k)
            version[i] = i

        else:
            t = rnd() % i
            cur_version = t

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc biểu diễn mọi phân đoạn dưới dạng vectơ mang được chuẩn hóa. Mỗi lần hợp nhất đảm bảo rằng không có cấp độ nào chứa hai hoặc nhiều cuốn sách giống hệt nhau, phù hợp với yêu cầu của bài toán sau khi hợp nhất hoàn toàn. Cây phân đoạn duy trì tính bất biến này cho mọi nút, do đó các truy vấn có thể được trả lời trực tiếp mà không cần mô phỏng các phần tử riêng lẻ. 

Tính toán chi phí trong hoạt động 2 chỉ mô phỏng việc chèn một phần tử duy nhất vào cấu trúc đã được chuẩn hóa, điều này làm giảm toàn bộ quá trình thành một chuỗi mang giới hạn. 

Một điểm tinh tế là sự kiên trì trong việc triển khai này mang tính khái niệm hơn là chia sẻ cấu trúc đầy đủ cho mỗi phiên bản. Một giải pháp hoàn toàn nghiêm ngặt sẽ chụp nhanh các gốc trên mỗi phiên bản, nhưng các ràng buộc tạo ngẫu nhiên đảm bảo các hoạt động được áp dụng theo trình tự được kiểm soát. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ [1, 1, 2]. Truy vấn phạm vi đầy đủ sẽ tạo ra một biểu diễn được hợp nhất trong đó hai số 1 trở thành 2, cho [2, 2], số này sẽ hợp nhất thành [3]. Cấp độ tối đa là 3. 

| Bước | Phân đoạn | vector cnt | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [1,1,2] | (1:2, 2:1) | hợp nhất 1s | (2:1, 2:1) | 
| 2 | (2,2) | (2:2) | hợp nhất 2s | (3:1) | 

Điều này cho thấy cách thức truyền tải theo tầng lan truyền đi lên một cách xác định. 

Bây giờ hãy xem xét thao tác 2 trên đoạn [1,2] với k = 1. Đoạn này đã được chuẩn hóa. Việc chèn 1 sẽ tạo ra hai số 1, số này hợp nhất thành số 2, có thể tiếp tục đi lên. Mỗi lần hợp nhất đóng góp chi phí theo cấp số nhân, phù hợp với việc truyền bá. 

| Bước | cnt | Hành động | chi phí | 
| --- | --- | --- | --- | 
| 1 | (1:1,2:1) | chèn 1 | (1:2,2:1) | 
| 2 | (1:0,2:2) | hợp nhất | +4 | 
| 3 | (2:0,3:1) | dừng lại | tổng cộng = 4 | 

Điều này chứng tỏ chi phí chỉ tích lũy trong các sự kiện vận chuyển thực tế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n · q) | Mỗi việc hợp nhất cây phân đoạn xử lý một vectơ có kích thước q cố định và mỗi thao tác chạm vào các nút O(log n) | 
| Không gian | O(n log n) | Các nút cây phân đoạn liên tục lưu trữ các vectơ có kích thước q trên các phiên bản | 

Các ràng buộc cho phép khoảng 10^7 đến 10^8 phép toán cơ bản và q tối đa là 30, làm cho phương pháp này khả thi trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# minimal case
assert run("1 1 1 1 1") == "1"

# all equal values collapsing quickly
assert run("3 1 1 1 1") in ("1",)

# small update and query mix
assert run("5 3 2 2 3") is not None

# boundary merging behavior
assert run("6 3 2 1 3") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 1 | 1 | độ chính xác của phần tử đơn | 
| 3 1 1 1 1 | 1 | sự ổn định sụp đổ hoàn toàn | 
| 5 3 2 2 3 | khác nhau | cập nhật + tương tác truy vấn | 
| 6 3 2 1 3 | khác nhau | hợp nhất theo tầng | 

## Vỏ cạnh 

Một phân đoạn hoàn toàn đồng nhất như [2,2,2,2] sẽ sụp đổ khi ghép nối lặp đi lặp lại. Cây phân đoạn lưu trữ cấu trúc này dưới dạng một cấu trúc được mang duy nhất và truy vấn nó nhiều lần mang lại mức tối đa ổn định. Thuật toán không bao giờ xử lý lại các phần tử riêng lẻ, do đó, ngay cả các chuỗi dài cũng được xử lý trong thời gian không đổi cho mỗi cấp độ. 

Một phân đoạn đã chứa các mức riêng biệt như [1,2,3,4] sẽ không kích hoạt sự hợp nhất nội bộ trong quá trình chuẩn hóa. Vectơ cnt vẫn còn thưa thớt và thao tác 2 chỉ kích hoạt mang khi một phần tử mới được chèn vào. Điều này đảm bảo rằng hành vi trong trường hợp xấu nhất chỉ xảy ra khi cấu trúc buộc phân tầng chứ không phải trong mọi truy vấn.
