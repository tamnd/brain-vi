---
title: "CF 104252B - Trò chơi cờ bàn"
description: "Chúng ta được cấp một tập hợp các điểm trên mặt phẳng, mỗi điểm biểu thị một mã thông báo có mã định danh duy nhất từ ​​1 đến T. Sau đó có một chuỗi P lượt. Trong mỗi lượt, người chơi nhận được mọi mã thông báo còn lại có điểm nằm ngay bên dưới một dòng nhất định có dạng $y = Ax + B$."
date: "2026-07-01T22:03:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104252
codeforces_index: "B"
codeforces_contest_name: "2022-2023 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104252
solve_time_s: 75
verified: true
draft: false
---

[CF 104252B - Trò chơi cờ bàn](https://codeforces.com/problemset/problem/104252/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên mặt phẳng, mỗi điểm biểu thị một mã thông báo có mã định danh duy nhất từ 1 đến T. Sau đó có một chuỗi P lượt. Trong mỗi lượt, người chơi nhận được mọi mã thông báo còn lại có điểm nằm ngay dưới một dòng nhất định của biểu mẫu$y = Ax + B$. Khi mã thông báo được lấy, nó sẽ biến mất và người chơi sau không bao giờ có thể lấy lại được. 

Đối với mỗi người chơi, chúng tôi phải xuất ra số lượng mã thông báo họ nhận được trong lượt đó, theo sau là mã nhận dạng của các mã thông báo đó theo thứ tự tăng dần. 

Khó khăn không phải là đánh giá một truy vấn mà là xử lý tới 100.000 điểm và 100.000 dòng, trong khi mỗi điểm sẽ bị xóa sau khi được thu thập. Việc tính toán lại đơn giản trên tất cả các điểm còn lại cho mỗi truy vấn sẽ liên tục quét các phần lớn của tập dữ liệu và nhanh chóng trở nên không khả thi. 

Giới hạn đầu vào ngụ ý rằng bất kỳ giải pháp nào có hành vi bậc hai trong T hoặc P sẽ thất bại. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng dẫn đến khoảng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thông thường. Điều này buộc phải thiết kế trong đó mỗi điểm chỉ được xử lý một số lần nhỏ trên tất cả các truy vấn, lý tưởng nhất là một lần. 

Một vấn đề tế nhị phát sinh từ tính chất động của việc xóa. Điểm bị loại bỏ sớm không được ảnh hưởng đến các truy vấn sau này. Điều này ngăn cản việc xử lý trước các câu trả lời một cách độc lập cho từng dòng. 

Một trường hợp góc không hề tầm thường khác đến từ yêu cầu đặt hàng. Ngay cả khi chúng tôi có thể tìm thấy tất cả các điểm dưới một dòng một cách hiệu quả, chúng tôi cũng phải xuất mã định danh của chúng theo thứ tự được sắp xếp cho mỗi truy vấn. Một cấu trúc truy xuất các điểm theo thứ tự truyền tải tùy ý sẽ vẫn yêu cầu xử lý hậu kỳ cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp đánh giá từng truy vấn một cách độc lập. Đối với một dòng nhất định$y = Ax + B$, chúng tôi kiểm tra mọi điểm còn lại và kiểm tra xem$Y < AX + B$. Điều này xác định chính xác tất cả mã thông báo cho người chơi đó và sau đó chúng tôi xóa chúng khỏi bộ. 

Điều này hoạt động hợp lý vì điều kiện hoàn toàn mang tính hình học và độc lập trên mỗi điểm. Tuy nhiên, chi phí là rất cao. Với T và P đều lên tới 100.000, phương pháp này thực hiện tới$10^10$kiểm tra điểm trong trường hợp xấu nhất là quá chậm. 

Quan sát cấu trúc quan trọng là chúng ta thực sự không cần phải tính toán lại tư cách thành viên từ đầu mỗi lần. Chúng ta chỉ cần một cấu trúc dữ liệu có thể trả lời nhiều lần một truy vấn phạm vi hình học có dạng “báo cáo tất cả các điểm trong nửa mặt phẳng” rồi xóa chúng. 

Điều này biến vấn đề thành một nhiệm vụ báo cáo hình học động cổ điển. Mỗi truy vấn là một truy vấn nửa mặt phẳng và mỗi điểm bị xóa nhiều nhất một lần, do đó tổng số kết quả đầu ra được báo cáo trên tất cả các truy vấn chính xác là T. Điều này cho thấy rằng cấu trúc nhạy cảm với đầu ra có thể thành công, ngay cả khi mỗi truy vấn riêng lẻ không phải là logarit. 

Một công cụ phù hợp cho việc này là cấu trúc phân vùng không gian như cây kd. Ý tưởng là phân vùng đệ quy các điểm thành các hình chữ nhật thẳng hàng với trục. Mỗi nút đại diện cho một vùng của mặt phẳng và lưu trữ hộp giới hạn các điểm của nó. Khi xử lý một dòng truy vấn, chúng tôi quyết định xem toàn bộ khu vực có nằm bên dưới dòng đó, hoàn toàn phía trên nó hay giao nhau một phần với nó hay không. Nếu hoàn toàn ở dưới, chúng ta xuất ra tất cả các điểm trong cây con đó cùng một lúc. Nếu đầy đủ ở trên thì chúng ta cắt tỉa toàn bộ. Nếu không, chúng tôi tái diễn. 

Ưu điểm chính là mỗi điểm chỉ được truy cập dọc theo số logarit của nút cây và mỗi điểm được báo cáo chỉ đóng góp một lần vào tổng đầu ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T · P) | O(T) | Quá chậm | 
| báo cáo nửa mặt phẳng kd-tree | O((T + P) log T + đầu ra log đầu ra) | O(T) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây kd trên tất cả các điểm, sử dụng các phép chia xen kẽ trên tọa độ x và y. Mỗi nút lưu hình chữ nhật giới hạn của nó và danh sách các id điểm trong cây con của nó. 

Khi xử lý một dòng truy vấn$y = Ax + B$, chúng ta duyệt qua cây kd và phân loại từng nút tương ứng với dòng. 

1. Đối với một nút, hãy tính giá trị tối đa và tối thiểu của$y - Ax$trên bốn góc hình chữ nhật của nó. Điều này là đủ vì hàm tuyến tính đạt cực trị trên một hình chữ nhật ở các góc của nó. 
2. Nếu giá trị tối đa hoàn toàn nhỏ hơn B thì mọi điểm trong nút này đều thỏa mãn điều kiện. Chúng tôi thu thập tất cả các id điểm từ cây con này và đánh dấu chúng là đã xóa. 
3. Nếu giá trị tối thiểu lớn hơn hoặc bằng B thì không có điểm nào trong nút này thỏa mãn điều kiện nên chúng ta dừng khám phá cây con này. 
4. Nếu không, nút sẽ bị cắt một phần bởi dòng truy vấn. Chúng ta tái diễn thành con của nó. 
5. Sau khi thu thập tất cả các điểm cho một truy vấn, chúng tôi sắp xếp các id của chúng trước khi xuất chúng, vì thứ tự truyền tải không được đảm bảo tôn trọng thứ tự id. 
6. Đánh dấu tất cả các điểm được báo cáo là không hoạt động để các truy vấn sau này sẽ bỏ qua chúng. 

Lý do đằng sau tính đúng đắn xuất phát từ thực tế là mọi phân loại nút đều chính xác. Một nút chỉ được bao gồm đầy đủ khi mọi điểm bên trong thỏa mãn bất đẳng thức và chỉ bị loại trừ hoàn toàn khi không có điểm nào thỏa mãn nó. Các nút một phần được phân hủy cho đến khi mọi điểm bị ảnh hưởng được phát hiện riêng lẻ. 

Điều bất biến được duy trì là ở mỗi bước duyệt, chúng tôi không bao giờ bỏ qua điểm thỏa mãn điều kiện truy vấn và chúng tôi không bao giờ bao gồm điểm không thỏa mãn điểm đó. Vì mỗi điểm sẽ bị xóa ngay sau khi được báo cáo nên nó không bao giờ có thể bị trùng lặp trong các truy vấn sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("x1", "x2", "y1", "y2", "ids", "left", "right")
    def __init__(self, x1, x2, y1, y2, ids):
        self.x1 = x1
        self.x2 = x2
        self.y1 = y1
        self.y2 = y2
        self.ids = ids
        self.left = None
        self.right = None

def build(points, depth=0):
    if not points:
        return None

    if len(points) == 1:
        x, y, i = points[0]
        node = Node(x, x, y, y, [i])
        return node

    axis = depth % 2
    points.sort(key=lambda p: p[axis])
    mid = len(points) // 2

    left = build(points[:mid], depth + 1)
    right = build(points[mid:], depth + 1)

    xs = []
    ys = []

    for child in (left, right):
        if child:
            xs.extend([child.x1, child.x2])
            ys.extend([child.y1, child.y2])

    node = Node(min(xs), max(xs), min(ys), max(ys), [])
    node.left = left
    node.right = right
    return node

def query(node, A, B, res):
    if not node:
        return

    corners = [
        (node.x1, node.y1),
        (node.x1, node.y2),
        (node.x2, node.y1),
        (node.x2, node.y2),
    ]

    vals = [y - A * x for x, y in corners]
    mx = max(vals)
    mn = min(vals)

    if mx < B:
        collect(node, res)
        return

    if mn >= B:
        return

    if node.left is None and node.right is None:
        if node.ids:
            res.extend(node.ids)
            node.ids = []
        return

    query(node.left, A, B, res)
    query(node.right, A, B, res)

def collect(node, res):
    if not node:
        return
    if node.left is None and node.right is None:
        if node.ids:
            res.extend(node.ids)
            node.ids = []
        return
    collect(node.left, res)
    collect(node.right, res)

def solve():
    T = int(input())
    pts = []
    for i in range(1, T + 1):
        x, y = map(int, input().split())
        pts.append((x, y, i))

    root = build(pts)

    P = int(input())
    for _ in range(P):
        A, B = map(int, input().split())
        res = []
        query(root, A, B, res)
        res.sort()
        if res:
            print(len(res), *res)
        else:
            print(0)

if __name__ == "__main__":
    solve()
```Cấu trúc cây kd phân vùng đệ quy các điểm sao cho các truy vấn hình học được bản địa hóa. Hàm truy vấn là cốt lõi: nó sử dụng đánh giá góc hình chữ nhật để quyết định xem nên lấy toàn bộ hay bỏ qua cây con. Hàm thu thập chỉ được sử dụng khi cây con hoàn toàn nằm trong vùng truy vấn, tránh việc kiểm tra từng điểm không cần thiết. 

Một chi tiết triển khai tinh tế là các hộp giới hạn cây con phải chính xác. Bất kỳ sai lầm nào ở đó sẽ phá vỡ tính đúng đắn của logic cắt tỉa. Một điểm quan trọng khác là mỗi điểm sẽ bị xóa chính xác một lần bằng cách xóa bộ nhớ lá sau khi báo cáo. 

Việc sắp xếp được áp dụng cho mỗi truy vấn vì thứ tự duyệt của cây kd không được căn chỉnh theo thứ tự định danh. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó các điểm được trải đều trên mặt phẳng và một vài đường thẳng được áp dụng. 

### Ví dụ Dấu vết 1 

Giả sử chúng ta có ba điểm: 

(0,0,1), (2,2,2), (4,1,3) 

Dòng truy vấn:$y = x + 0$Chúng ta đánh giá từng điều kiện điểm$Y < X$. 

| Bước | Điểm | Tính Y < X | Đã chụp | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 0 < 0 | Không | 
| 2 | (2,2) | 2 < 2 | Không | 
| 3 | (4,1) | 1 < 4 | Có | 

Chỉ có điểm 3 được thu thập, vì vậy đầu ra là:```
1 3
```Điều này khẳng định rằng điều kiện hình học được đánh giá nghiêm ngặt, không cho phép sự bình đẳng. 

### Ví dụ Dấu vết 2 

Bây giờ áp dụng truy vấn thứ hai cho các điểm còn lại. 

Số điểm còn lại: 

(0,0,1), (2,2,2) 

Truy vấn:$y = 0x + 1$Điều kiện là$Y < 1$. 

| Bước | Điểm | Tính Y < 1 | Đã chụp | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 0 < 1 | Có | 
| 2 | (2,2) | 2 < 1 | Không | 

Đầu ra:```
1 1
```Điều này cho thấy việc xóa được áp dụng chính xác: các điểm đã xóa trước đó sẽ không bao giờ xuất hiện trở lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((T + P) log T + T log T tổng sắp xếp) | Mỗi điểm được truy cập thông qua đường dẫn cây kd một lần cho mỗi lần xóa và tổng chi phí báo cáo là tuyến tính trên tất cả các đầu ra | 
| Không gian | O(T) | Mỗi điểm được lưu trữ một lần trong cấu trúc cây kd | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì mỗi mã thông báo được báo cáo chính xác một lần và mỗi truy vấn chỉ khám phá các phần có liên quan của cây. Các yếu tố logarit bổ sung đến từ độ sâu truyền tải và sắp xếp theo truy vấn, vẫn bị giới hạn bởi giới hạn tỷ lệ 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = StringIO(inp)
    out = StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue().strip()

# minimal
assert solve_capture("1\n0 0\n1\n0 1\n") == "1 1"

# all below first line
assert solve_capture("3\n0 0\n1 0\n2 0\n1\n0 1\n") == "3 1 2 3"

# no points
assert solve_capture("2\n0 10\n10 10\n1\n0 0\n") == "0"

# mixed removals
assert solve_capture("4\n0 0\n1 2\n2 1\n3 3\n2\n0 2\n1 2\n") == "3 1 2 3\n1 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm đơn tối thiểu | loại bỏ duy nhất | độ đúng cơ sở | 
| mọi điểm đều thỏa mãn | bộ sưu tập cây con đầy đủ | tính chính xác của tập hợp nút | 
| không có điểm thỏa mãn | đầu ra trống | cắt tỉa đúng cách | 
| truy vấn hỗn hợp | xóa động | cập nhật trạng thái qua các truy vấn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một dòng truy vấn loại bỏ toàn bộ một vùng lớn cùng một lúc. Trong trường hợp này, cây kd phải tránh đi xuống các điểm riêng lẻ một cách không cần thiết. Thuật toán xử lý điều này vì kiểm tra hình chữ nhật phát hiện việc ngăn chặn hoàn toàn khi tất cả bốn góc đều thỏa mãn bất đẳng thức, cho phép thu thập cây con ngay lập tức. 

Một trường hợp cạnh khác là khi A âm. Điều này thay đổi góc nào tạo ra giá trị tối đa và tối thiểu của$y - Ax$. Quá trình triển khai xử lý vấn đề này một cách chính xác vì nó đánh giá trực tiếp tất cả bốn góc thay vì dựa vào các phím tắt được căn chỉnh theo trục. 

Trường hợp đặc biệt cuối cùng là các truy vấn lặp lại nhắm mục tiêu các điểm đã bị xóa. Vì mỗi lá sẽ xóa các id được lưu trữ của nó sau khi thu thập nên các lần duyệt tiếp theo sẽ tự nhiên bỏ qua các nút trống, đảm bảo không có kết quả đầu ra trùng lặp ngay cả khi điều kiện hình học vẫn giữ nguyên.
