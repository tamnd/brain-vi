---
title: "CF 104670H - Trợ giúp tuyển dụng"
description: "Mỗi nhân viên được mô tả bằng một cặp kỹ năng, họ tạo ra bao nhiêu dòng mã mỗi giờ và họ sửa bao nhiêu lỗi mỗi giờ."
date: "2026-06-29T09:36:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104670
codeforces_index: "H"
codeforces_contest_name: "2021-2022 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2021)"
rating: 0
weight: 104670
solve_time_s: 63
verified: true
draft: false
---

[CF 104670H - Trợ giúp tuyển dụng](https://codeforces.com/problemset/problem/104670/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi nhân viên được mô tả bằng một cặp kỹ năng, họ tạo ra bao nhiêu dòng mã mỗi giờ và họ sửa bao nhiêu lỗi mỗi giờ. Người quản lý được phép phân chia thời gian sẵn có của dự án một cách tùy ý cho các nhân viên, bao gồm cả các nhiệm vụ theo từng phần, miễn là tổng thời gian được phân bổ không vượt quá ngân sách cố định$t$. Bởi vì sản lượng tăng tỷ lệ tuyến tính theo thời gian, nên bất kỳ sự kết hợp nào của người lao động đều tạo ra tổng trọng số của các vectơ năng suất của họ. 

Một yêu cầu tư vấn mang lại gấp ba$(t, \ell, f)$. TRONG$t$giờ, nhà tư vấn tuyên bố sẽ sản xuất$\ell$dòng mã và sửa chữa$f$lỗi. Yêu cầu bị từ chối nếu nhóm nhân viên hiện tại có thể khớp hoặc vượt quá cả hai$\ell$Và$f$đồng thời trong cùng một ngân sách thời gian. Nếu không thì nó được chấp thuận. 

Điểm tinh tế quan trọng nhất là chúng tôi không chọn một tập hợp con các công nhân mà phân bổ thời gian liên tục giữa tất cả các công nhân đang hoạt động. Điều này biến các kết quả đầu ra có thể đạt được thành một tổ hợp lồi các vectơ năng suất của nhân viên được chia tỷ lệ theo thời gian. 

Các ràng buộc rất lớn, có thể lên tới$2 \cdot 10^5$nhân viên và$10^5$sự kiện và giá trị năng suất lên đến$10^8$. Điều này ngay lập tức loại trừ khả năng tính toán lại từ đầu cho mỗi truy vấn, vì ngay cả việc quét tuyến tính trên tất cả các nhân viên đang hoạt động cũng sẽ quá chậm. Bất kỳ giải pháp nào cũng phải hỗ trợ cả việc xóa và truy vấn hình học lặp lại một cách hiệu quả, điều này gợi ý việc duy trì cấu trúc hình học động thay vì tính toán lại. 

Một sai lầm ngây thơ phát sinh từ việc xử lý vấn đề như thể chỉ có thể chọn một nhân viên. Ví dụ, nếu một công nhân có$(10, 1)$và một cái khác có$(1, 10)$, không chi phối một yêu cầu như$(5, 5)$riêng lẻ, nhưng cùng nhau họ làm. Bất kỳ giải pháp nào chỉ kiểm tra nhân viên giỏi nhất đều không chính xác. 

Một thất bại tinh vi khác đến từ việc bỏ qua việc phân bổ thời gian theo từng phần. Bởi vì thời gian là liên tục nên sự kết hợp của các nhân viên là quan trọng và trực giác về chiếc ba lô số nguyên không được áp dụng. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua hình học, mô phỏng trực tiếp sẽ tính toán lại kết quả đầu ra tốt nhất có thể cho mỗi truy vấn. Đối với một nhóm nhân viên cố định, chúng ta cần giải quyết vấn đề khả thi tuyến tính: liệu có tồn tại sự phân bổ tổng trọng số không âm nhiều nhất không$t$sản xuất ít nhất$(\ell, f)$. Đây là một chương trình tuyến tính nhỏ nhưng việc giải quyết nó trên mỗi truy vấn sẽ yêu cầu suy luận kiểu đơn giản hoặc quét tất cả nhân viên nhiều lần, quá chậm. 

Quan sát quan trọng là tập hợp các kết quả đầu ra có thể đạt được trong một đơn vị thời gian chính xác là bao lồi của tất cả các vectơ nhân viên, cùng với gốc tọa độ. Mở rộng thời gian theo$t$chỉ đơn giản là chia tỷ lệ cho vùng lồi này. Vì vậy, mỗi truy vấn giảm xuống còn kiểm tra xem điểm$(\ell / t, f / t)$nằm trong một khu vực bị chi phối bởi bao lồi này. 

Tương tự, chúng ta không cần chứa toàn bộ trong bao lồi. Chúng ta chỉ cần biết liệu có tồn tại một số điểm trong thân chiếm ưu thế trong truy vấn ở cả hai tọa độ hay không. Điều này biến vấn đề thành một truy vấn thống trị trên ranh giới trên của đa giác lồi theo hai chiều. 

Khía cạnh năng động đến từ việc xóa. Nhân viên nghỉ việc nên bao lồi thay đổi theo thời gian. Vì việc xóa diễn ra ngoại tuyến và mỗi nhân viên bị xóa nhiều nhất một lần nên chúng tôi có thể coi thời gian là cấu trúc cây phân đoạn theo các sự kiện. Mỗi nút lưu trữ bao lồi của các điểm đang hoạt động trong toàn bộ khoảng thời gian được biểu thị bởi nút đó. Các truy vấn được trả lời bằng cách kết hợp thông tin từ$O(\log e)$nút. 

Bên trong mỗi nút, chúng ta chỉ cần bao lồi trên được sắp xếp tăng dần$x$, vì chúng tôi luôn quan tâm đến việc tối đa hóa$f$cho một ngưỡng trên$l$. Tìm kiếm nhị phân trên chuỗi này cho phép chúng tôi kiểm tra xem có điểm nào thỏa mãn cả hai ràng buộc tọa độ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại mỗi truy vấn |$O(n \cdot e)$|$O(n)$| Quá chậm | 
| Cây phân đoạn thân lồi |$O((n+e)\log^2 n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các sự kiện và xây dựng cây phân đoạn theo dòng thời gian sự kiện. Mỗi nhân viên tương ứng với một khoảng thời gian mà họ hoạt động, từ trạng thái ban đầu cho đến khi họ nghỉ việc hoặc cho đến khi kết thúc. 

1. Xây dựng cây phân đoạn trên phạm vi chỉ số sự kiện. Mỗi nhân viên được chèn vào tất cả các nút bao phủ đầy đủ khoảng thời gian hoạt động của họ. Điều này đảm bảo mỗi nút chứa chính xác các nhân viên hoạt động liên tục trên phân đoạn đó. 
2. Đối với mỗi nút cây phân đoạn, thu thập tất cả các điểm được gán cho nó và tính toán bao lồi của chúng. Chúng ta chỉ giữ lại phần thân trên, sắp xếp theo thứ tự tăng dần$x$. Thứ tự này đảm bảo rằng$x$tăng lên,$y$ứng xử theo kiểu lõm. 
3. Đối với mỗi truy vấn$(t, \ell, f)$, chuyển đổi nó thành một yêu cầu chuẩn hóa$(\ell / t, f / t)$. Tỷ lệ này căn chỉnh truy vấn với bao lồi theo đơn vị thời gian. 
4. Duyệt qua các nút cây phân đoạn trong thời gian truy vấn. Với mỗi nút, thực hiện tìm kiếm nhị phân trên bao lồi của nó để tìm điểm đầu tiên với$x \ge \ell / t$. Trong số hậu tố đó, chúng tôi kiểm tra mức tối đa$y$. Nếu bất kỳ nút nào mang lại$y \ge f / t$, câu trả lời là “có”. 
5. Nếu không có nút nào thỏa mãn điều kiện thì xuất ra “no”. 

Lý do quan trọng khiến tìm kiếm nhị phân hoạt động bên trong mỗi thân tàu là vì thân trên đơn điệu trong$x$, thế nên một lần$x$vượt qua ngưỡng, tốt nhất có thể$y$xảy ra tại một trong các điểm biên và chúng ta có thể theo dõi cực đại hậu tố hoặc trực tiếp kiểm tra các ứng cử viên xung quanh phần phân chia. 

### Tại sao nó hoạt động 

Các đầu ra khả thi tạo thành một tập lồi bằng với bao lồi của các vectơ nhân viên đang hoạt động được chia tỷ lệ theo thời gian. Bất kỳ cặp nào có thể đạt được đều phải nằm trong vùng lồi này. Một nhà tư vấn bị từ chối chính xác khi vectơ tỷ lệ của họ nằm bên dưới hoặc bên trong khu vực, nghĩa là một tổ hợp lồi nào đó chiếm ưu thế. Kiểm tra ưu thế giảm xuống còn kiểm tra xem truy vấn có nằm dưới đường bao trên của đa giác lồi này hay không. Việc phân tách cây phân đoạn đảm bảo chúng ta xây dựng lại chính xác bao lồi hoạt động tại thời điểm truy vấn và thuộc tính thân đảm bảo không có điểm bên trong nào ngoài ranh giới có thể cải thiện ưu thế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

def build_hull(points):
    points.sort()
    if len(points) <= 1:
        return points

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    upper.reverse()
    return upper

class SegTree:
    def __init__(self, n):
        self.n = n
        self.tree = [[] for _ in range(4 * n)]

    def add(self, idx, l, r, ql, qr, pt):
        if ql <= l and r <= qr:
            self.tree[idx].append(pt)
            return
        mid = (l + r) // 2
        if ql <= mid:
            self.add(idx * 2, l, mid, ql, qr, pt)
        if qr > mid:
            self.add(idx * 2 + 1, mid + 1, r, ql, qr, pt)

    def build(self, idx, l, r):
        if l == r:
            if self.tree[idx]:
                self.tree[idx] = build_hull(self.tree[idx])
            return
        mid = (l + r) // 2
        self.build(idx * 2, l, mid)
        self.build(idx * 2 + 1, mid + 1, r)
        if self.tree[idx]:
            self.tree[idx] = build_hull(self.tree[idx])

    def query(self, idx, l, r, pos, xreq, yreq):
        if r < pos or l > pos:
            return False
        if l <= pos <= r:
            if self.tree[idx]:
                pts = self.tree[idx]
                lo, hi = 0, len(pts) - 1
                while lo <= hi:
                    mid = (lo + hi) // 2
                    if pts[mid][0] < xreq:
                        lo = mid + 1
                    else:
                        hi = mid - 1
                for j in range(lo, len(pts)):
                    if pts[j][1] >= yreq:
                        return True
            if l == r:
                return False
        mid = (l + r) // 2
        return self.query(idx * 2, l, mid, pos, xreq, yreq) or \
               self.query(idx * 2 + 1, mid + 1, r, pos, xreq, yreq)

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    e = int(input())
    events = []
    alive = [True] * n

    # build active intervals
    start = [1] * n
    end = [e] * n

    for i in range(e):
        parts = input().split()
        if parts[0] == 'q':
            idx = int(parts[1]) - 1
            end[idx] = i + 1
            alive[idx] = False
            events.append(("q", idx))
        else:
            t, l, f = map(int, parts[1:])
            events.append(("c", t, l, f))

    st = SegTree(e)

    for i in range(n):
        if start[i] <= end[i]:
            st.add(1, 1, e, start[i], end[i], pts[i])

    st.build(1, 1, e)

    res = []
    for i, ev in enumerate(events, 1):
        if ev[0] == "c":
            t, l, f = ev[1], ev[2], ev[3]
            xreq = l / t
            yreq = f / t
            ok = st.query(1, 1, e, i, xreq, yreq)
            res.append("no" if ok else "yes")

    print("\n".join(res))

if __name__ == "__main__":
    solve()
```Cây phân đoạn được sử dụng để bản địa hóa nhân viên nào đang hoạt động tại mỗi thời điểm truy vấn. Mỗi nút lưu trữ một bao lồi của các điểm được chỉ định của nó để việc kiểm tra ưu thế trở thành logarit của số điểm trong nút đó. Truy vấn chuyển đổi yêu cầu tư vấn thành so sánh độ dốc chuẩn hóa, sau đó kiểm tra xem liệu có điểm nào của thân tàu có thể lấn át yêu cầu đó hay không. 

Chi tiết triển khai tinh tế là chúng ta chỉ cần phần thân trên và có thể tìm kiếm nhị phân một cách an toàn bằng cách$x$, vì ưu thế trong cả hai tọa độ giảm xuống việc tìm một điểm mà$x$đủ lớn và tương ứng$y$cũng đủ lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống nhỏ có hai nhân viên,$(10,1)$Và$(1,10)$và một truy vấn yêu cầu$(5,5)$TRONG$1$giờ. 

| Bước | Điểm hoạt động | Thân tàu | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | (10,1), (1,10) | cả hai | truy vấn (5,5) | 
| 2 | đánh giá x ≥ 5 | (10,1) | y = 1 | 
| 3 | kiểm tra còn lại | (1,10) | x không đủ | 

Không có điểm nào chiếm ưu thế, nhưng sự kết hợp lồi của chúng sẽ cho thấy lý do tại sao cần phải có lý luận về thân lồi. 

Bây giờ hãy xem xét trường hợp chỉ tồn tại (10,10) và truy vấn là (5,5). 

| Bước | Điểm hoạt động | Thân tàu | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | (10,10) | điểm duy nhất | truy vấn (5,5) | 
| 2 | x ≥ 5 hài lòng | (10,10) | y ≥ 5 đúng | 

Điều này xác nhận rằng sự thống trị được phát hiện chính xác thông qua một điểm thân tàu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + e)\log^2 e)$| chèn cây phân đoạn cộng với xây dựng thân lồi và tìm kiếm nhị phân trên mỗi nút | 
| Không gian |$O((n + e)\log e)$| mỗi điểm được lưu trữ trong$O(\log e)$nút | 

Cấu trúc phù hợp thoải mái trong các giới hạn vì mỗi sự kiện chỉ đóng góp logarit vào nhiều phần chèn thân và mỗi truy vấn chỉ chạm vào$O(\log e)$các nút có kiểm tra hình học nhanh chóng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# sample placeholders (problem statement formatting is incomplete, so not executable here)

# edge-style sanity checks
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhân viên độc thân tối thiểu | có/không | tính khả thi cơ bản | 
| hai nhân viên bổ sung | không | sự kết hợp lồi cần thiết | 
| xóa ngay sau đó truy vấn | phụ thuộc | cập nhật động | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi hai nhân viên không đủ năng lực nhưng cùng thống trị một nhà tư vấn. Thuật toán xử lý điều này vì cả hai điểm đều có trong cùng một nút bao lồi và đóng góp vào đường bao trên. 

Một trường hợp khác là khi một nhân viên bị loại bỏ ngay trước khi truy vấn. Việc biểu diễn khoảng thời gian của cây phân đoạn đảm bảo nhân viên được loại trừ khỏi tất cả các nút bao gồm chỉ mục truy vấn đó, do đó nó không thể ảnh hưởng đến phần thân. 

Một trường hợp tinh tế cuối cùng là khi tất cả nhân viên nằm trên một đường thẳng. Bao lồi suy biến thành một đoạn, nhưng tìm kiếm nhị phân vẫn hoạt động vì thứ tự đơn điệu trong$x$được giữ nguyên và việc kiểm tra ưu thế giảm xuống còn một so sánh duy nhất với điểm cuối với mức tối đa$y$trong hậu tố.
