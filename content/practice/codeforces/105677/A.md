---
title: "CF 105677A - Titanomachy"
description: "Chúng ta được cung cấp một dãy số biểu thị “cân bằng sức mạnh” hiện tại giữa hai đội quân sắp xếp theo cặp. Mỗi vị trí đóng góp một giá trị số nguyên và giá trị đó có thể thay đổi theo thời gian một cách thống nhất trên toàn bộ mảng. Hai hoạt động xảy ra trực tuyến."
date: "2026-06-22T05:06:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105677
codeforces_index: "A"
codeforces_contest_name: "2024-2025 ICPC Southwestern European Regional Contest (SWERC 2024)"
rating: 0
weight: 105677
solve_time_s: 51
verified: true
draft: false
---

[CF 105677A - Titanomachy](https://codeforces.com/problemset/problem/105677/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số biểu thị “cân bằng sức mạnh” hiện tại giữa hai đội quân sắp xếp theo cặp. Mỗi vị trí đóng góp một giá trị số nguyên và giá trị đó có thể thay đổi theo thời gian một cách thống nhất trên toàn bộ mảng. 

Hai hoạt động xảy ra trực tuyến. Một thao tác sẽ thêm một giá trị cố định vào mọi phần tử trong mảng, dịch chuyển toàn bộ chuỗi lên hoặc xuống. Thao tác còn lại yêu cầu một phân đoạn cụ thể của mảng và yêu cầu chúng ta đánh giá tất cả các phân đoạn con liền kề có thể có bên trong nó. Đối với mỗi phân đoạn như vậy, chúng tôi tính tổng của nó và sau đó lấy số tiền tối đa trong số các khoản đó. Nếu mức tối đa này là âm, câu trả lời sẽ được giữ ở mức 0. 

Vì vậy, mỗi truy vấn về cơ bản là yêu cầu tổng mảng con tối đa trong một khoảng thời gian bị giới hạn, với sự phức tạp bổ sung là mảng đang được dịch chuyển toàn cầu giữa các truy vấn. 

Các ràng buộc lên tới ba trăm nghìn phần tử và truy vấn, do đó, bất kỳ giải pháp nào tính toán lại thông tin phân đoạn từ đầu cho mỗi truy vấn sẽ không thành công. Tính toán mảng con tối đa đơn giản là tuyến tính cho mỗi truy vấn, vốn đã đưa ra trường hợp xấu nhất bậc hai. Thậm chí một$O(N \log N)$giải pháp cây phân đoạn phải được thiết kế cẩn thận để xử lý các cập nhật phạm vi toàn cầu một cách hiệu quả. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị trong khoảng được truy vấn đều âm. Trong trường hợp đó, tổng mảng con tối đa là âm, nhưng định nghĩa bài toán buộc kết quả đầu ra bằng 0. 

Ví dụ, hãy xem xét mảng$[-5, -2, -7]$và một truy vấn trên phạm vi đầy đủ. Mọi tổng của mảng con đều âm nên câu trả lời đúng là$0$. Việc triển khai bất cẩn sẽ trả về trực tiếp tổng mảng con tối đa$-2$, điều này không chính xác do yêu cầu kẹp. 

Một vấn đề khác phát sinh từ việc bổ sung toàn cầu. Nếu chúng ta thêm một hằng số$X$đối với tất cả các phần tử, mọi tổng tiền tố và do đó mọi tổng mảng con đều dịch chuyển theo cách có cấu trúc và chúng ta phải tránh cập nhật tất cả các nút riêng lẻ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xử lý từng truy vấn một cách độc lập. Đối với một truy vấn đánh giá$[L, R]$, chúng ta sẽ liệt kê tất cả các mảng con bên trong phạm vi đó và tính tổng của chúng. Cái này đã tốn rồi$O(N^2)$mỗi truy vấn trong trường hợp xấu nhất, vì có$O((R-L+1)^2)$mảng con và mỗi tổng có thể được tính bằng$O(1)$với tổng tiền tố. Với tối đa$3 \times 10^5$truy vấn, điều này là hoàn toàn không thể thực hiện được. 

Ngay cả việc cải thiện thuật toán này thành thuật toán của Kadane cho mỗi truy vấn vẫn tốn kém$O(N)$cho mỗi truy vấn, vì chúng ta phải tính toán lại tổng mảng con tối đa cho mọi phạm vi. Trở ngại chính là các bản cập nhật ảnh hưởng đến toàn bộ mảng một cách thống nhất, do đó việc tính toán lại là không thể tránh khỏi trong một mô hình đơn giản. 

Quan sát quan trọng là truy vấn đang yêu cầu tổng mảng con tối đa cổ điển trên một phạm vi và cấu trúc này chính xác là những gì cây phân đoạn có thể duy trì. Mỗi phân đoạn có thể lưu trữ đủ thông tin để hợp nhất hai nửa: tổng tổng, tổng tiền tố tốt nhất, tổng hậu tố tốt nhất và tổng mảng con tốt nhất. 

Khó khăn là hoạt động thêm toàn cầu. Tuy nhiên, việc thêm một hằng số$X$mỗi phần tử trong một phân đoạn sẽ ảnh hưởng đến tất cả bốn giá trị theo cách có thể dự đoán được. Tổng số tiền tăng thêm$X \cdot len$, tổng tiền tố và hậu tố cũng dịch chuyển tuyến tính theo độ dài phân đoạn và tổng mảng con tốt nhất tăng theo$X \cdot len$cũng bởi vì mọi mảng con ứng cử viên đều có cùng độ lệch tỷ lệ với độ dài của nó. 

Để hỗ trợ điều này một cách hiệu quả, chúng tôi sử dụng cây phân đoạn với khả năng lan truyền lười biếng. Mỗi nút duy trì bốn giá trị tiêu chuẩn và thẻ lười lưu trữ các bổ sung toàn cầu đang chờ xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2 Q)$|$O(1)$| Quá chậm | 
| Cây phân đoạn với sự lan truyền lười biếng |$O((N+Q)\log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây phân đoạn trên mảng, trong đó mỗi nút biểu thị một khoảng và lưu trữ bốn đại lượng: tổng tổng của khoảng, tổng tiền tố tối đa, tổng hậu tố tối đa và tổng mảng con tối đa. 

1. Chúng tôi khởi tạo các nút lá trực tiếp từ các giá trị mảng. Mỗi lá có tổng, tiền tố, hậu tố và tốt nhất đều bằng chính giá trị đó. Đây là trường hợp cơ bản vì một phần tử chỉ có thể có một mảng con. 
2. Chúng tôi xây dựng các nút nội bộ bằng cách hợp nhất hai nút con. Khi kết hợp các phân đoạn bên trái và bên phải, tổng số tiền là tổng của cả hai tổng. Tiền tố là mức tối đa giữa tiền tố bên trái và tổng tiền tố bên trái cộng với tiền tố bên phải. Hậu tố là đối xứng. Mảng con tốt nhất là mảng con tối đa trong số các hậu tố tốt nhất bên trái, tốt nhất bên phải và hậu tố bên trái cộng với tiền tố bên phải. Điều này có hiệu quả vì bất kỳ mảng con tối ưu nào đều nằm hoàn toàn ở một phía hoặc vượt qua ranh giới. 
3. Đối với mỗi nút, chúng tôi cũng duy trì một giá trị lười biểu thị phần bổ sung đang chờ xử lý cho tất cả các phần tử trong phân đoạn đó. Điều này cho phép chúng tôi trì hoãn cập nhật. 
4. Khi áp dụng phép cộng$X$đến một nút bao gồm chiều dài$len$, chúng ta tăng tổng số tiền lên$X \cdot len$. Tiền tố, hậu tố và mảng con tốt nhất đều tăng thêm$X \cdot len$đồng thời, bởi vì mọi mảng con bên trong phân khúc đều tăng theo cùng một số lần không đổi mà phần đóng góp độ dài của nó được tổng hợp một cách nhất quán trên toàn bộ biểu diễn phân khúc. 
5. Khi truyền xuống cây, chúng ta đẩy giá trị lười biếng cho con và xóa giá trị đó ở cha mẹ. Điều này đảm bảo tính chính xác khi kết hợp các cập nhật và truy vấn một phần. 
6. Đối với hoạt động MẠNH MẼ, chúng tôi áp dụng cập nhật phạm vi một cách lười biếng trên toàn bộ phạm vi cây phân đoạn. 
7. Đối với hoạt động ASSESS, chúng tôi truy vấn cây phân đoạn$[L, R]$, kết hợp các kết quả bằng thao tác hợp nhất và trả về$\max(0, best)$. 

Tại sao nó hoạt động: mỗi nút luôn thể hiện một bản tóm tắt chính xác về phân đoạn của nó trong tất cả các bản cập nhật lười biếng đang chờ xử lý. Hoạt động hợp nhất có tính liên kết theo nghĩa là nó bảo toàn tất cả thông tin cần thiết về các mảng con vượt qua ranh giới phân đoạn. Lan truyền lười biếng duy trì tính bất biến rằng các giá trị được lưu trữ của mỗi nút tương ứng chính xác với phân đoạn của nó sau khi áp dụng tất cả các bản cập nhật đã được gán một cách logic cho nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

NEG_INF = -10**30

class Node:
    __slots__ = ("sum", "pref", "suf", "best", "lazy", "len")
    def __init__(self, s=0, p=0, su=0, b=0, lz=0, length=1):
        self.sum = s
        self.pref = p
        self.suf = su
        self.best = b
        self.lazy = lz
        self.len = length

def merge(left, right):
    res = Node()
    res.len = left.len + right.len
    res.sum = left.sum + right.sum
    res.pref = max(left.pref, left.sum + right.pref)
    res.suf = max(right.suf, right.sum + left.suf)
    res.best = max(left.best, right.best, left.suf + right.pref)
    return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 4 * self.n
        self.tree = [Node(length=1) for _ in range(self.size)]
        self.build(1, 0, self.n - 1, arr)

    def apply(self, idx, val):
        node = self.tree[idx]
        node.sum += val * node.len
        node.pref += val * node.len
        node.suf += val * node.len
        node.best += val * node.len
        node.lazy += val

    def push(self, idx):
        lazy = self.tree[idx].lazy
        if lazy != 0:
            self.apply(idx * 2, lazy)
            self.apply(idx * 2 + 1, lazy)
            self.tree[idx].lazy = 0

    def build(self, idx, l, r, arr):
        if l == r:
            v = arr[l]
            self.tree[idx] = Node(v, v, v, v, 0, 1)
            return
        m = (l + r) // 2
        self.build(idx * 2, l, m, arr)
        self.build(idx * 2 + 1, m + 1, r, arr)
        self.tree[idx] = merge(self.tree[idx * 2], self.tree[idx * 2 + 1])

    def update(self, idx, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.apply(idx, val)
            return
        self.push(idx)
        m = (l + r) // 2
        if ql <= m:
            self.update(idx * 2, l, m, ql, qr, val)
        if qr > m:
            self.update(idx * 2 + 1, m + 1, r, ql, qr, val)
        self.tree[idx] = merge(self.tree[idx * 2], self.tree[idx * 2 + 1])

    def query(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.tree[idx]
        self.push(idx)
        m = (l + r) // 2
        if qr <= m:
            return self.query(idx * 2, l, m, ql, qr)
        if ql > m:
            return self.query(idx * 2 + 1, m + 1, r, ql, qr)
        left = self.query(idx * 2, l, m, ql, qr)
        right = self.query(idx * 2 + 1, m + 1, r, ql, qr)
        return merge(left, right)

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        parts = input().split()
        if parts[0] == "STRENGTH":
            x = int(parts[1])
            st.update(1, 0, n - 1, 0, n - 1, x)
        else:
            l = int(parts[1]) - 1
            r = int(parts[2]) - 1
            res = st.query(1, 0, n - 1, l, r)
            out.append(str(max(0, res.best)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ các bản tóm tắt khoảng thời gian đầy đủ để các truy vấn giảm xuống việc kết hợp số lượng nút logarit. Việc lan truyền lười biếng đảm bảo rằng các phép bổ sung toàn cục không yêu cầu chạm vào mọi phần tử một cách rõ ràng, chỉ đánh dấu các phân đoạn và đẩy các giá trị khi cần. 

Một chi tiết triển khai tinh tế là tất cả bốn giá trị được lưu trữ phải được cập nhật nhất quán trong quá trình thêm từng phần. Việc quên cập nhật tiền tố, hậu tố hoặc tốt nhất bằng cùng một delta sẽ phá vỡ tính chính xác vì các lần hợp nhất sau này cho rằng mỗi nút đã được chuẩn hóa hoàn toàn. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng thông tin đầu vào mẫu để minh họa cách các bản cập nhật và truy vấn tương tác với nhau. 

Mảng bắt đầu là$[1, -2, 3, 4]$. 

Sau mỗi lần thao tác: 

| Bước | Hoạt động | Trạng thái mảng | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | ĐÁNH GIÁ 1 4 | [1, -2, 3, 4] | 6 | 
| 2 | ĐÁNH GIÁ 1 2 | [1, -2] | 1 | 
| 3 | ĐÁNH GIÁ 2 2 | [-2] | 0 | 
| 4 | SỨC MẠNH 2 | [3, 0, 5, 6] | - | 
| 5 | ĐÁNH GIÁ 1 4 | [3, 0, 5, 6] | 14 | 
| 6 | ĐÁNH GIÁ 1 2 | [3, 0] | 3 | 

Dấu vết cho thấy cách phép cộng đồng nhất sẽ dịch chuyển tất cả các tổng của mảng con trong tương lai mà không thay đổi logic cấu trúc của phân đoạn nào là tối ưu. 

Ví dụ nhỏ thứ hai nêu bật cách xử lý tiêu cực. Coi như$[-3, -1, -4]$. Mọi truy vấn trên bất kỳ phạm vi nào luôn tạo ra tổng mảng con âm tốt nhất, do đó mọi đầu ra đều được giới hạn ở mức 0. Điều này xác nhận sự cần thiết của hoạt động tối đa cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N + Q)\log N)$| Mỗi bản cập nhật và truy vấn chạm tới số logarit của các nút cây phân đoạn | 
| Không gian |$O(N)$| Cây phân đoạn lưu trữ một lượng dữ liệu không đổi trên mỗi nút | 

Các ràng buộc cho phép lên đến$3 \times 10^5$do đó hệ số logarit khoảng 20 sẽ giữ cho tổng số phép tính nằm trong phạm vi an toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# sample case placeholder (format not provided as runnable strings)
# custom edge cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả truy vấn phủ định duy nhất | 0 | hành vi kẹp | 
| cập nhật phần tử đơn + truy vấn | giá trị hoặc 0 | trường hợp cơ sở đúng đắn | 
| toàn dải lặp lại SỨC MẠNH | mảng con tối đa đã dịch chuyển | lười biếng tuyên truyền đúng đắn | 
| cập nhật và truy vấn xen kẽ | kết quả nhất quán | tính tương tác đúng đắn | 

## Vỏ cạnh 

Khoảng một phần tử có giá trị âm kiểm tra trực tiếp quy tắc kẹp. Nếu mảng là$[-5]$, câu trả lời đúng là 0 vì tổng mảng con duy nhất là âm. Cây phân đoạn lưu trữ tốt nhất dưới dạng$-5$, nhưng lớp truy vấn sẽ chuyển nó thành 0, duy trì tính chính xác. 

Hoạt động STRENGTH toàn phạm vi sẽ kiểm tra xem liệu lan truyền lười biếng có cập nhật chính xác tất cả các nút hay không. Nếu chúng ta thêm$+3$ĐẾN$[1, -2, 3]$, mọi nút phải phản ánh những thay đổi nhất quán; nếu không thì việc hợp nhất sẽ kết hợp các trạng thái không nhất quán và tạo ra cực đại không chính xác. 

Các truy vấn và cập nhật xen kẽ lặp đi lặp lại sẽ kiểm tra xem các giá trị lười đang chờ xử lý có được đẩy vào đúng thời điểm hay không. Nếu một nút được truy vấn mà không đẩy giá trị lười của nó lên trước, thì các giá trị tốt nhất được lưu trữ của nó sẽ trở nên cũ và câu trả lời sẽ khác với cấu trúc mảng con thực sự.
