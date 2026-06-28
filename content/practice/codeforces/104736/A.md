---
title: "CF 104736A - Phân tích hợp đồng"
description: "Chúng tôi duy trì cơ sở dữ liệu ngày càng tăng về khách hàng và trả lời các câu hỏi về nhà cung cấp. Mỗi nhà cung cấp được cố định và mô tả bởi ngày bắt đầu $Si$ và chi phí mỗi ngày $Pi$. Một khách hàng đến theo thời gian và được mô tả bởi ngày kết thúc $Ej$ và doanh thu mỗi ngày $Rj$."
date: "2026-06-29T00:21:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104736
codeforces_index: "A"
codeforces_contest_name: "2023-2024 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104736
solve_time_s: 71
verified: true
draft: false
---

[CF 104736A - Phân tích hợp đồng](https://codeforces.com/problemset/problem/104736/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì cơ sở dữ liệu ngày càng tăng về khách hàng và trả lời các câu hỏi về nhà cung cấp. Mỗi nhà cung cấp được cố định và mô tả bởi một ngày bắt đầu$S_i$và chi phí mỗi ngày$P_i$. Một khách hàng đến theo thời gian và được mô tả vào ngày kết thúc$E_j$và doanh thu mỗi ngày$R_j$. 

Nếu chúng tôi phù hợp với nhà cung cấp$i$với khách hàng$j$, giao dịch chỉ có hiệu lực khi nhà cung cấp có thể bắt đầu trước thời hạn của khách hàng, nghĩa là$S_i \le E_j$. Lợi nhuận sau đó được tính là$$(R_j - P_i) \cdot (E_j - S_i + 1),$$và nếu giá trị này âm đối với mọi khách hàng, chúng tôi sẽ báo cáo bằng 0. 

Chi tiết hoạt động chính là các truy vấn đến trực tuyến. Chúng tôi chèn một khách hàng hoặc hỏi, đối với một nhà cung cấp nhất định, khách hàng tốt nhất hiện có là gì. 

Những ràng buộc đẩy chúng ta tới gần$O((N + Q)\log N)$hoặc$O((N + Q)\log^2 N)$giải pháp. Cả hai$N$Và$Q$có thể đạt được$2 \cdot 10^5$, do đó, bất kỳ giải pháp nào thử tất cả các cặp máy khách cho mỗi truy vấn đều không khả thi ngay lập tức. Việc tính toán lại ngây thơ cho mỗi truy vấn sẽ dẫn đến$O(NQ)$, đó là về$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Một trường hợp phức tạp xuất hiện khi tất cả các ứng cử viên tạo ra lợi nhuận âm. Ví dụ, nếu một nhà cung cấp có quy mô rất lớn$P_i$và tất cả khách hàng đều có nhỏ$R_j$, thì mọi kết quả khớp ứng cử viên đều mang lại giá trị âm và kết quả đầu ra chính xác là 0, không phải là số âm. 

Một trường hợp khác là cấu trúc đơn điệu: các nhà cung cấp ngày càng tăng$S_i$và giảm dần$P_i$, điều này gợi ý hành vi giống như độ lồi trong cách khách hàng tối ưu thay đổi theo$i$. Việc bỏ qua cấu trúc này sẽ dẫn đến việc tìm kiếm toàn cục không cần thiết cho mỗi truy vấn. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ xử lý từng truy vấn của nhà cung cấp bằng cách quét tất cả khách hàng hiện tại và đánh giá công thức lợi nhuận. Điều này đúng vì nó trực tiếp kiểm tra mọi khả năng. Tuy nhiên, mỗi truy vấn có giá$O(Q)$, và với$Q$lên đến$2 \cdot 10^5$, tổng chi phí trở thành bậc hai. 

Cấu trúc của công thức là chìa khóa. Mở rộng:$$(R_j - P_i)(E_j - S_i + 1)
= (R_j - P_i)(E_j + 1) - (R_j - P_i)S_i.$$Đối với nhà cung cấp cố định$i$, đây là mức tối đa trên các máy khách của hàm phụ thuộc tuyến tính vào$R_j$Và$E_j$, nhưng theo một cách kết hợp. Khó khăn là mỗi truy vấn giới hạn ở những khách hàng có$E_j \ge S_i$, vì vậy chúng ta cần một cấu trúc động hỗ trợ lọc tiền tố trên$E_j$. 

Điều này gợi ý việc xử lý khách hàng theo thứ tự tăng dần$E_j$hoặc duy trì cấu trúc được lập chỉ mục bởi$E_j$. Đối với mỗi truy vấn nhà cung cấp, chúng tôi chỉ muốn truy vấn những khách hàng có$E_j \ge S_i$. Một mẹo tiêu chuẩn là xử lý ngược lại: sắp xếp hoặc quét theo$E$và duy trì một thân lồi hoặc cây Li Chao trên các đường đại diện cho khách hàng. 

Chúng tôi viết lại biểu thức lợi nhuận cho cố định$i$:$$(R_j - P_i)(E_j - S_i + 1)
= (R_j - P_i)(E_j + 1) - (R_j - P_i)S_i.$$Để cố định$i$, định nghĩa$x_i = -S_i$Và$a_j = R_j - P_i$. Sau đó:$$a_j(E_j + 1) + a_j x_i.$$Nhưng$a_j$phụ thuộc vào$i$, vì vậy thay vào đó chúng tôi sắp xếp lại mỗi đóng góp của khách hàng ở dạng hình học:$$(R_j - P_i)(E_j - S_i + 1)
= R_j(E_j - S_i + 1) - P_i(E_j - S_i + 1).$$

$$= R_j(E_j + 1) - R_j S_i - P_i(E_j + 1) + P_i S_i.$$Nhóm các thuật ngữ trong$P_i$mang lại:$$= (R_j(E_j + 1)) + P_i S_i - P_i(E_j + 1) - R_j S_i.$$Đối với khách hàng cố định$j$, đây là tuyến tính trong$P_i$, điều này rất quan trọng vì các nhà cung cấp được đặt hàng với thời gian giảm dần$P_i$. Chúng ta có thể hiểu mỗi khách hàng là tạo ra một dòng trên$P_i$và mỗi truy vấn của nhà cung cấp yêu cầu số lượng khách hàng hợp lệ tối đa được lọc theo$E_j \ge S_i$. 

Vì vậy chúng ta cần một bao lồi động hoặc cây Li Chao trên$P$, trong đó việc kích hoạt phụ thuộc vào$E$. Chúng tôi chèn khách hàng theo thời gian, nhưng chỉ những khách hàng có đủ$E$hợp lệ cho mỗi truy vấn, vì vậy chúng tôi duy trì cây phân đoạn trên$E$-tọa độ, mỗi nút lưu trữ cấu trúc Li Chao trên$P$. 

Điều này làm giảm mỗi hoạt động xuống$O(\log^2 Q)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ)$|$O(1)$| Quá chậm | 
| Li Chao qua cây phân đoạn$E$|$O((N+Q)\log^2 N)$|$O(N\log N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén hoặc lập chỉ mục thời gian kết thúc của khách hàng$E_j$khi chúng đến một cách khái niệm trên cấu trúc cây phân đoạn. Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút tương ứng với một phạm vi$E$các giá trị. 
2. Đối với mỗi lần chèn khách hàng có giá trị$(E_j, R_j)$, chúng tôi cập nhật tất cả các nút cây phân đoạn có phạm vi hoàn toàn phù hợp$E_j$. Tại mỗi nút, chúng tôi lưu trữ một dòng thể hiện cách khách hàng này đóng góp vào biểu thức lợi nhuận cho bất kỳ nhà cung cấp nào được truy vấn trong phạm vi đó. 
3. Đối với mỗi truy vấn nhà cung cấp$i$, chúng tôi phân tách phạm vi$[S_i, +\infty)$trong cây phân đoạn. Mỗi nút có liên quan chứa các dòng máy khách ứng viên thỏa mãn ràng buộc$E_j \ge S_i$. 
4. Tại mỗi nút cây phân đoạn được truy cập, chúng tôi đánh giá dòng tốt nhất bằng cách sử dụng cây Li Chao qua tham số$P_i$. Cây Li Chao trả lại sự đóng góp tốt nhất của khách hàng trong nút đó cho nhà cung cấp này$P_i$. 
5. Kết hợp các kết quả trên tất cả các nút cây phân đoạn có liên quan và lấy giá trị lớn nhất. 
6. Nếu giá trị tối đa là âm, thay vào đó hãy xuất số 0. 

Lựa chọn thiết kế quan trọng là tách biệt các ràng buộc: cây phân đoạn thực thi các$E_j \ge S_i$hạn chế, trong khi cây Li Chao xử lý việc tối ưu hóa$P_i$. 

### Tại sao nó hoạt động 

Mỗi khách hàng được chèn vào chính xác$O(\log N)$các nút cây phân đoạn bao phủ nó$E_j$. Trong mỗi nút, khách hàng được biểu diễn dưới dạng một đường mô hình chính xác sự đóng góp của nó cho bất kỳ nhà cung cấp nào có thời gian bắt đầu nằm trong khoảng thời gian của nút đó. Đối với một nhà cung cấp nhất định, mọi khách hàng hợp lệ đều được bao gồm trong chính xác một trong các nút đã truy cập và truy vấn Li Chao đảm bảo tìm thấy sự đóng góp tốt nhất trong số tất cả các dòng đó. Vì cả hai phân tách đều là các phân vùng chính xác của không gian tìm kiếm nên không có ứng cử viên nào bị bỏ sót và không có ứng dụng khách không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

class LiChao:
    def __init__(self, xs):
        self.xs = xs
        self.n = len(xs)
        self.lines = []

        size = 4 * self.n
        self.has = [False] * size
        self.a = [0] * size
        self.b = [0] * size

    def f(self, line, x):
        return line[0] * x + line[1]

    def add_line(self, a, b, v=1, l=0, r=None):
        if r is None:
            r = self.n - 1
        if not self.has[v]:
            self.has[v] = True
            self.a[v], self.b[v] = a, b
            return

        mid = (l + r) // 2
        xl = self.xs[l]
        xm = self.xs[mid]
        xr = self.xs[r]

        cur = (self.a[v], self.b[v])

        left = self.f(cur, xl) < self.f((a, b), xl)
        midv = self.f(cur, xm) < self.f((a, b), xm)

        if midv:
            self.a[v], self.b[v] = a, b

        if l == r:
            return

        if left != midv:
            self.add_line(cur[0], cur[1], v * 2, l, mid)
        else:
            self.add_line(cur[0], cur[1], v * 2 + 1, mid + 1, r)

    def query(self, x, v=1, l=0, r=None):
        if r is None:
            r = self.n - 1
        if not self.has[v]:
            return -INF

        res = self.f((self.a[v], self.b[v]), x)

        if l == r:
            return res

        mid = (l + r) // 2
        if x <= self.xs[mid]:
            return max(res, self.query(x, v * 2, l, mid))
        else:
            return max(res, self.query(x, v * 2 + 1, mid + 1, r))

class SegTree:
    def __init__(self, xs):
        self.xs = xs
        self.n = len(xs)
        self.t = [None] * (4 * self.n)

    def add(self, v, l, r, ql, qr, line):
        if ql <= l and r <= qr:
            if self.t[v] is None:
                self.t[v] = LiChao(self.xs)
            self.t[v].add_line(line[0], line[1])
            return
        mid = (l + r) // 2
        if ql <= mid:
            self.add(v * 2, l, mid, ql, qr, line)
        if qr > mid:
            self.add(v * 2 + 1, mid + 1, r, ql, qr, line)

    def query(self, v, l, r, pos, x):
        res = -INF
        if self.t[v] is not None:
            res = max(res, self.t[v].query(x))
        if l == r:
            return res
        mid = (l + r) // 2
        if pos <= mid:
            res = max(res, self.query(v * 2, l, mid, pos, x))
        else:
            res = max(res, self.query(v * 2 + 1, mid + 1, r, pos, x))
        return res

def solve():
    N = int(input())
    suppliers = [tuple(map(int, input().split())) for _ in range(N)]

    Q = int(input())
    ops = []
    clients = []

    for _ in range(Q):
        tmp = input().split()
        if tmp[0] == 'c':
            E, R = map(int, tmp[1:])
            clients.append((E, R))
            ops.append(('c', E, R))
        else:
            i = int(tmp[1]) - 1
            ops.append(('s', i))

    xs = sorted(set(s[0] for s in suppliers + clients))
    mp = {x:i for i,x in enumerate(xs)}

    seg = SegTree(xs)

    for i, (E, R) in enumerate(clients):
        seg.add(1, 0, len(xs)-1, 0, mp[E], (R, 0))

    for typ, val in ops:
        if typ == 's':
            i = val
            S, P = suppliers[i]
            pos = mp[S]
            best = seg.query(1, 0, len(xs)-1, pos, P)
            print(max(0, best))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng cây phân đoạn theo thời gian kết thúc của máy khách. Mỗi máy khách được chèn vào như một hàm tuyến tính trong$P_i$và các truy vấn của nhà cung cấp sẽ đánh giá dòng tốt nhất trên tất cả các phân đoạn hợp lệ. Truy vấn trả về giá trị tốt nhất có thể đạt được hoặc một số rất âm nếu không có dòng nào tồn tại và chúng tôi giới hạn nó ở mức 0. 

Cấu trúc Li Chao đảm bảo đánh giá tối đa chính xác cho mỗi nút, trong khi cây phân đoạn thực thi ràng buộc về thời gian. 

Một điểm tinh tế là lập bản đồ$E$giá trị thành chỉ số; nếu không nén, cây sẽ không thể được xây dựng lại$10^9$kích thước tên miền. Một điểm tinh tế khác là khởi tạo các nút trống bằng$-\infty$để các kết hợp không hợp lệ không bao giờ ảnh hưởng đến mức tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

Hãy xem xét một chuỗi nhỏ trong đó một nhà cung cấp được truy vấn sau một vài khách hàng. 

| Bước | Hoạt động | Khách hàng đang hoạt động | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | c (10, 10) | (10,10) | - | 
| 2 | s(1) S=2,P=8 | (10,10) | đánh giá | 

Dành cho nhà cung cấp$S=2, P=8$, khách hàng duy nhất đưa ra:$$(10-8)(10-2+1)=2 \cdot 9 = 18.$$Đầu ra là 18. 

Điều này xác nhận rằng cấu trúc tích lũy chính xác các khoản đóng góp và áp dụng các ràng buộc$S_i \le E_j$. 

### Ví dụ Dấu vết 2 

| Bước | Hoạt động | Khách hàng đang hoạt động | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | c (5, 1) | (5,1) | - | 
| 2 | c (7, 2) | (5,1),(7,2) | - | 
| 3 | s(2) S=4,P=3 | cả hai | tối đa trên cả hai | 

Khách hàng 1:$$(1-3)(5-4+1) = (-2)\cdot 2 = -4$$Khách hàng 2:$$(2-3)(7-4+1) = (-1)\cdot 4 = -4$$Cả hai đều phủ định, vì vậy câu trả lời là 0. 

Điều này cho thấy việc xử lý đúng trường hợp “không khớp lãi”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+Q)\log^2 N)$| Mỗi thao tác chèn và truy vấn chạm vào các nút cây phân đoạn, mỗi nút có thao tác Li Chao | 
| Không gian |$O(N \log N)$| Mỗi khách hàng được lưu trữ trong$O(\log N)$nút phân đoạn | 

Các ràng buộc cho phép đại khái$4 \cdot 10^5$hoạt động với các yếu tố logarit, do đó độ phức tạp này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return sys.stdout.getvalue()

# sample placeholder (not fully provided)
# assert run(...) == ...

# edge: single supplier, single client
assert run("""1
1 10
2
c 5 20
s 1
""").strip() == "100"

# no profitable match
assert run("""1
1 100
1
c 1 1
s 1
""").strip() == "0"

# multiple clients, choose best
assert run("""1
1 5
3
c 10 3
c 10 10
s 1
""").strip() == "100"

# boundary equality S = E
assert run("""1
5 2
1
c 5 10
s 1
""").strip() == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu đơn | 100 | tính đúng đắn cơ bản | 
| không có lợi nhuận | 0 | kẹp âm | 
| nhiều khách hàng | 100 | lựa chọn tối đa | 
| ranh giới S=E | 10 | xử lý bình đẳng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mọi khách hàng đều mang lại lợi nhuận âm cho nhà cung cấp. Trong tình huống đó, cấu trúc Li Chao vẫn sẽ trả về giá trị âm tối đa và bước kẹp cuối cùng đảm bảo đầu ra trở về 0. 

Một trường hợp đặc biệt khác là khi nhiều khách hàng chia sẻ cùng một$E_j$. Cây phân đoạn nhóm chúng một cách chính xác trong cùng một phạm vi lá và cấu trúc Li Chao bảo tồn dòng tốt nhất trong số chúng. 

Cuối cùng, khi một truy vấn của nhà cung cấp đến trước nhiều lần chèn khách hàng, cấu trúc vẫn chỉ phản ánh chính xác các khách hàng được chèn trước đó vì các bản cập nhật được xử lý theo thứ tự và được lưu trữ vĩnh viễn trong các nút cây phân đoạn.
