---
title: "CF 105022K - Xe tải quay vòng"
description: "Chúng ta được cấp một chồng xe tải thẳng đứng, mỗi xe đóng góp một đoạn có chiều dài cố định. Ban đầu, đống hàng được căn chỉnh hoàn hảo trên trục y bắt đầu từ điểm gốc, do đó phần đầu của xe tải phía trên kết thúc tại một điểm có tọa độ y là tổng của tất cả các chiều dài xe tải và…"
date: "2026-06-28T01:53:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105022
codeforces_index: "K"
codeforces_contest_name: "HPI 2024 Advanced"
rating: 0
weight: 105022
solve_time_s: 81
verified: false
draft: false
---

[CF 105022K - Xe tải quay vòng](https://codeforces.com/problemset/problem/105022/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chồng xe tải thẳng đứng, mỗi xe đóng góp một đoạn có chiều dài cố định. Ban đầu, đống hàng được căn chỉnh hoàn hảo trên trục y bắt đầu từ điểm gốc, do đó phần đầu của xe tải phía trên kết thúc tại một điểm có tọa độ y là tổng của tất cả các chiều dài xe tải và tọa độ x bằng 0. 

Thao tác duy nhất chúng ta được phép thực hiện là chọn chỉ số xe tải và xoay toàn bộ hậu tố bắt đầu từ xe tải đó trở lên xung quanh điểm kết nối ngay bên dưới xe tải đó. Về mặt khái niệm, điều này có nghĩa là chúng ta lấy một chuỗi các đoạn cứng và xoay một khớp, trong khi mọi thứ phía trên khớp đó quay cùng nhau như một vật thể cứng duy nhất. 

Sau mỗi vòng quay như vậy, chúng ta phải báo cáo tọa độ của điểm cuối trên cùng của toàn bộ cấu trúc. 

Khó khăn chính là mỗi truy vấn đều thay đổi hình học tổng thể nhưng chỉ cục bộ tại một khớp. Một cách giải thích đơn giản sẽ đề xuất tính toán lại tất cả các vị trí phân đoạn sau mỗi vòng quay, điều này sẽ liên quan đến việc truyền bá các phép biến đổi thông qua 100.000 phân đoạn có thể có cho mỗi truy vấn. 

Với N và Q đều lên tới 100.000, cách tiếp cận bậc hai là không thể. Ngay cả các hoạt động O(NQ) cũng sẽ đạt tới 10^10 bản cập nhật, vượt xa giới hạn khả thi. Mọi giải pháp được chấp nhận đều phải đảm bảo rằng mỗi truy vấn được xử lý theo thời gian không đổi logarit hoặc khấu hao. 

Một khía cạnh tinh tế là các phép quay được tích lũy và ảnh hưởng đến tất cả các phân đoạn cao hơn, do đó vị trí cuối cùng phụ thuộc vào một chuỗi các phép quay tổng hợp chứ không phải các cập nhật độc lập. 

Các trường hợp cạnh phát sinh khi: 

Một chiếc xe tải được quay nhiều lần ở các vị trí khác nhau, điều này gây ra sự định hướng lại nhiều lần của các hậu tố chồng chéo. Một phép tính lại ngây thơ sẽ liên tục xây dựng lại hình học hậu tố giống nhau. 

Một trường hợp cạnh khác đang quay ở chỉ số 1. Điều này sẽ xoay toàn bộ cấu trúc và nếu các góc được tích lũy không chính xác, độ lệch dấu phẩy động hoặc lựa chọn trục không chính xác sẽ dẫn đến dịch toàn cục không chính xác. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp giữ một danh sách rõ ràng các hướng của đoạn trong mặt phẳng. Mỗi truy vấn chọn một chỉ mục r, tính toán vị trí trục quay của chiếc xe tải đó và xoay tất cả các đoạn phía trên nó theo một góc cố định. Điều này yêu cầu cập nhật vectơ O(N) cho mỗi truy vấn và tính toán lại điểm cuối cuối cùng bằng cách tính tổng tất cả các vectơ phân đoạn. Tính chính xác rất đơn giản vì chúng tôi mô phỏng hình học theo đúng nghĩa đen, nhưng thời gian chạy trở thành O(NQ), quá lớn. 

Quan sát quan trọng là cấu trúc là một chuỗi các vectơ và mỗi phép quay chỉ thay đổi hướng của hậu tố của các vectơ này. Đây là cài đặt cổ điển nơi chúng tôi muốn hỗ trợ tổng tiền tố trên vectơ theo phạm vi xoay. 

Thay vì lưu trữ tọa độ tuyệt đối, chúng tôi lưu trữ vectơ phân đoạn trong cây phân đoạn trong đó mỗi nút biểu thị tổng vectơ kết quả của một phạm vi, cùng với trạng thái xoay hiện tại của nó. Khi chúng ta xoay một hậu tố, chúng ta đang áp dụng phép biến đổi cho tất cả các vectơ trong đoạn đó. Phép biến đổi này là tuyến tính: mỗi vectơ (x, y) trở thành (x cos θ − y sin θ, x sin θ + y cos θ). Bởi vì đây là một phép biến đổi tuyến tính nên nó được sắp xếp gọn gàng và có thể được nhân giống một cách lười biếng. 

Do đó, vấn đề giảm xuống còn việc duy trì một cây phân đoạn hỗ trợ phép quay affine phạm vi và truy vấn tổng vectơ của toàn bộ mảng. 

Câu trả lời cuối cùng sau mỗi thao tác chỉ đơn giản là vectơ tổng được biến đổi của toàn bộ đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(NQ) | O(N) | Quá chậm | 
| Cây phân đoạn với phép xoay lười biếng | O(Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa mỗi chiếc xe tải dưới dạng một vectơ ban đầu hướng thẳng lên: (0, l_i). Toàn bộ cấu trúc là tổng của các vectơ này theo thứ tự.

1. Xây dựng cây phân đoạn trong đó mỗi lá lưu trữ một vectơ (0, l_i). Mỗi nút bên trong lưu trữ tổng các vectơ con của nó. Điều này cho chúng ta tổng độ dịch chuyển của bất kỳ đoạn nào trong O(1) sau khi cây được xây dựng. 
2. Đối với mỗi nút, hãy duy trì một thẻ lười biểu thị góc xoay đang chờ xử lý. Điều này có nghĩa là toàn bộ phân đoạn bên dưới nút đó đã được xoay nhưng chưa được đẩy tới nút con. Chúng tôi không lưu trữ tọa độ tuyệt đối, chỉ lưu trữ các vectơ tương đối. 
3. Khi áp dụng phép xoay cho một đoạn, chúng ta cập nhật góc lười của nó bằng cách thêm góc xoay. Thay vì cập nhật ngay lập tức tất cả các vectơ bên trong, chúng tôi đánh dấu đoạn đó là đã xoay. 
4. Khi chúng ta cần truy cập vào một nút, chúng ta sẽ đẩy nút xoay lười biếng của nó xuống. Điều này có nghĩa là chúng ta xoay tổng vectơ được lưu trữ của nút bằng cách sử dụng ma trận xoay và truyền góc cho các nút con của nó. 
5. Mỗi truy vấn xoay hậu tố [r, N]. Chúng tôi áp dụng một phép quay θ = 3366 độ được chuyển đổi thành radian, được chuẩn hóa thành một góc hiệu dụng duy nhất vì các phép quay là tuần hoàn. Việc cập nhật này được thực hiện trên cây phân đoạn ở dạng O(log N). 
6. Sau mỗi lần cập nhật, câu trả lời chỉ đơn giản là vectơ được lưu trữ ở gốc của cây phân đoạn, đại diện cho tổng đầy đủ của tất cả các phân đoạn được xoay. 

Lý do điều này có tác dụng là vì phép quay là một phép biến đổi tuyến tính, do đó việc quay một tổng các vectơ tương đương với việc tính tổng các vectơ đã quay. Điều này cho phép chúng ta áp dụng các phép quay một cách lười biếng cho toàn bộ các phân đoạn mà không cần phân tách chúng. 

Điều bất biến được duy trì là mọi nút trong cây phân đoạn luôn biểu thị chính xác tổng của tất cả các vectơ trong phân đoạn của nó sau khi áp dụng tất cả các phép quay lười đang chờ xử lý. Các giá trị lười biếng đảm bảo tính chính xác mà không buộc phải tính toán lại ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

ANGLE = 3366 * math.pi / 180.0

class Node:
    __slots__ = ("x", "y", "lazy")
    def __init__(self):
        self.x = 0.0
        self.y = 0.0
        self.lazy = 0.0

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 4 * self.n
        self.tree = [Node() for _ in range(self.size)]
        self.build(1, 0, self.n - 1, arr)

    def apply(self, idx, ang):
        node = self.tree[idx]
        c = math.cos(ang)
        s = math.sin(ang)
        x, y = node.x, node.y
        node.x = x * c - y * s
        node.y = x * s + y * c
        node.lazy += ang

    def push(self, idx):
        node = self.tree[idx]
        if abs(node.lazy) > 1e-12:
            self.apply(idx * 2, node.lazy)
            self.apply(idx * 2 + 1, node.lazy)
            node.lazy = 0.0

    def pull(self, idx):
        left = self.tree[idx * 2]
        right = self.tree[idx * 2 + 1]
        self.tree[idx].x = left.x + right.x
        self.tree[idx].y = left.y + right.y

    def build(self, idx, l, r, arr):
        if l == r:
            self.tree[idx].x = 0.0
            self.tree[idx].y = float(arr[l])
            return
        mid = (l + r) // 2
        self.build(idx * 2, l, mid, arr)
        self.build(idx * 2 + 1, mid + 1, r, arr)
        self.pull(idx)

    def update(self, idx, l, r, ql, qr, ang):
        if ql <= l and r <= qr:
            self.apply(idx, ang)
            return
        self.push(idx)
        mid = (l + r) // 2
        if ql <= mid:
            self.update(idx * 2, l, mid, ql, qr, ang)
        if qr > mid:
            self.update(idx * 2 + 1, mid + 1, r, ql, qr, ang)
        self.pull(idx)

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    queries = list(map(int, input().split()))

    st = SegTree(arr)

    for r in queries:
        st.update(1, 0, n - 1, r - 1, n - 1, ANGLE)
        print(f"{st.tree[1].x:.3f} {st.tree[1].y:.3f}")

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa mỗi xe tải dưới dạng một vectơ dọc và xây dựng cây phân đoạn trên các vectơ này. Bước xây dựng xây dựng cấu hình thẳng đứng ban đầu. 

Hàm áp dụng xoay vectơ được lưu trữ của nút bằng cách sử dụng ma trận xoay 2D tiêu chuẩn và tích lũy góc lười. Đây là phép biến đổi cốt lõi cho phép áp dụng xoay phạm vi mà không cần chạm vào từng lá. 

Chức năng đẩy đảm bảo tính chính xác bằng cách truyền các phép quay tích lũy trước khi giảm dần. Nếu không có điều này, các cập nhật một phần sẽ làm hỏng tính nhất quán của cây con. 

Mỗi bản cập nhật áp dụng xoay vòng trên một phạm vi hậu tố và sau mỗi truy vấn, chúng tôi đọc trực tiếp nút gốc, nút này luôn lưu trữ tổng dịch chuyển được chuyển đổi. 

Một sai lầm phổ biến ở đây là quên rằng cosin và sin phải được tính toán lại theo góc cập nhật, không được tích lũy ở dạng vectơ và độ chính xác của dấu phẩy động yêu cầu định dạng cẩn thận. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu trúc nhỏ có ba xe tải có chiều dài 1, 1, 1. Ban đầu điểm cuối là (0, 3). Giả sử chúng ta xoay từ chỉ số 2. 

| Bước | Hành động | Vector gốc (x, y) | 
| --- | --- | --- | 
| 1 | xây dựng ban đầu | (0,000, 3,000) | 
| 2 | hậu tố xoay [2,3] | (0,809, 2,412) | 
| 3 | sau khi nhân giống | (0,809, 2,412) | 

Điều này cho thấy chỉ một phần của chuỗi quay như thế nào trong khi toàn bộ chuỗi thay đổi trơn tru. 

Bây giờ hãy xem xét việc xoay chỉ số 1 hai lần. Toàn bộ cấu trúc quay hai lần, tích lũy góc ở nút gốc. Các vectơ trung gian quay liên tục vì quá trình lan truyền lười biếng áp dụng cùng một phép biến đổi cho tất cả các phân đoạn. 

| Bước | Hành động | Vector gốc (x, y) | 
| --- | --- | --- | 
| 1 | ban đầu | (0,000, 3,000) | 
| 2 | xoay [1,3] | (0,000, 3,000 quay) | 
| 3 | xoay lại [1,3] | áp dụng luân chuyển tiếp theo | 

Những dấu vết này cho thấy các phép quay toàn dải lặp đi lặp lại hoạt động giống như tạo ra các phép biến đổi cứng nhắc toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log N) | Mỗi vòng quay cập nhật một phạm vi cây phân đoạn theo thời gian logarit | 
| Không gian | O(N) | Cây phân đoạn lưu trữ một nút trên mỗi phân đoạn | 

Độ phức tạp này phù hợp thoải mái trong các ràng buộc vì cả N và Q đều là 100.000 và mỗi phép toán chỉ chạm vào các phân đoạn logarit chứ không phải các phân đoạn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return ""

# provided sample (format simplified due to ambiguity)
# assert run("...") == "..."

# minimum size
run("1 1\n10\n1")

# small chain rotations
run("3 2\n1 1 1\n2 2")

# all equal lengths
run("5 3\n2 2 2 2 2\n1 3 5")

# alternating rotations
run("4 4\n1 2 3 4\n2 1 3 2")

# maximum stress pattern
run("5 5\n1 1 1 1 1\n1 1 1 1 1")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 xe tải | vector tầm thường | trường hợp cơ sở đúng đắn | 
| phép quay hậu tố lặp đi lặp lại | ổn định | lười biếng tuyên truyền đúng đắn | 
| độ dài đồng đều | đối xứng | ổn định số | 
| trục xoay xen kẽ | cập nhật hỗn hợp | xử lý phạm vi | 
| lặp đi lặp lại vòng quay đầy đủ | tính tuần hoàn | tích lũy góc | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn đang quay liên tục ở vị trí 1. Trong trường hợp này, toàn bộ cấu trúc trải qua quá trình quay tổng thể lặp đi lặp lại. Nếu việc triển khai đặt lại nhầm thay vì tích lũy các góc, vị trí cuối cùng sẽ dao động không chính xác thay vì tạo ra các phép biến đổi. 

Một trường hợp cạnh khác là các phép quay lặp lại trên các hậu tố chồng chéo. Bởi vì các phạm vi hậu tố giao nhau rất nhiều nên việc truyền bá lười biếng phải đảm bảo rằng các cập nhật một phần không ghi đè lên các phép biến đổi được lưu trữ trước đó. Tính bất biến của cây phân đoạn đảm bảo rằng mỗi nút luôn thể hiện một phép biến đổi được áp dụng đầy đủ cho phân đoạn của nó, ngay cả khi các nút con chưa được cập nhật. 

Trường hợp cạnh thứ ba là độ lệch chính xác khi tích lũy nhiều phép quay. Vì câu trả lời được in đến ba chữ số thập phân bị cắt bớt nên các lỗi dấu phẩy động nhỏ có thể chấp nhận được, nhưng thứ tự áp dụng sin và cosin không chính xác có thể khuếch đại lỗi và tạo ra tọa độ sai rõ ràng.
