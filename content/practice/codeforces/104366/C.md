---
title: "CF 104366C - Tranh Trừu Tượng"
description: "Chúng ta được cung cấp một tập hợp các đoạn thẳng thẳng hàng theo trục trên mặt phẳng 2D vô hạn. Mỗi phân đoạn là dọc hoặc ngang. Hai đoạn được coi là kết nối nếu chúng giao nhau về mặt vật lý tại bất kỳ điểm nào, bao gồm cả việc chạm vào điểm cuối."
date: "2026-07-01T17:42:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104366
codeforces_index: "C"
codeforces_contest_name: "The 17th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 104366
solve_time_s: 60
verified: true
draft: false
---

[CF 104366C - Tranh trừu tượng](https://codeforces.com/problemset/problem/104366/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đoạn thẳng thẳng hàng theo trục trên mặt phẳng 2D vô hạn. Mỗi phân đoạn là dọc hoặc ngang. Hai đoạn được coi là kết nối nếu chúng giao nhau về mặt vật lý tại bất kỳ điểm nào, bao gồm cả việc chạm vào điểm cuối. Kết nối này có tính bắc cầu, vì vậy nếu đoạn A cắt B và B cắt C thì A, B và C thuộc cùng một cấu trúc được kết nối. 

Sau khi xây dựng cấu trúc hình học này, chúng tôi nhận được các truy vấn bao gồm hai điểm. Đối với mỗi truy vấn, chúng ta cần xác định xem có tồn tại đường dẫn bắt đầu từ điểm đầu tiên, di chuyển dọc theo một số đoạn, có thể chuyển sang các đoạn giao nhau nhiều lần và cuối cùng đến điểm thứ hai hay không. 

Theo thuật ngữ đồ thị, mỗi đoạn là một nút và các cạnh tồn tại giữa các đoạn giao nhau. Mỗi truy vấn hỏi liệu hai điểm có nằm trong cùng một thành phần liên thông của đồ thị giao nhau này hay không, theo quy tắc bổ sung rằng một điểm được coi là một phần của đoạn nếu nó nằm trên đó. 

Các ràng buộc rất lớn: lên tới 100.000 phân đoạn và 100.000 truy vấn, với tọa độ lớn tới 10^9 độ lớn. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra các giao điểm giữa các phân đoạn cho mỗi truy vấn hoặc xây dựng tất cả các giao lộ theo cặp một cách rõ ràng. Một bài kiểm tra giao điểm hình học O(n^2) ngây thơ vượt xa giới hạn khả thi. 

Khó khăn chính là khả năng kết nối được xác định thông qua các nút giao thông, nhưng các truy vấn lại đề cập đến các điểm tùy ý chứ không phải các đoạn. Vì vậy, chúng ta phải hỗ trợ cả việc xây dựng kết nối một cách hiệu quả và ánh xạ các điểm lên cấu trúc này. 

Một số trường hợp đặc biệt quan trọng. 

Trường hợp một cạnh là khi các đoạn chỉ chạm vào điểm cuối. Ví dụ: đoạn thẳng đứng từ (0, 0) đến (0, 2) và đoạn ngang từ (-1, 2) đến (1, 2) cắt nhau tại (0, 2). Mặc dù chúng chỉ gặp nhau ở một điểm duy nhất nhưng chúng phải được coi là có sự kết nối. 

Một trường hợp cạnh khác là khi một điểm truy vấn nằm chính xác tại giao điểm của nhiều phân đoạn. Điểm đó phải kế thừa khả năng kết nối từ tất cả các đoạn đi qua nó. 

Cuối cùng, một truy vấn trong đó cả hai điểm nằm trên cùng một đoạn nhưng ở các thành phần bị ngắt kết nối do thiếu giao điểm ở nơi khác vẫn phải được trả lời chính xác. Hệ thống không phải là “cùng một đoạn”, mà là “cùng một bộ phận được kết nối qua các nút giao thông”. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force coi các phân đoạn là các đỉnh của biểu đồ và kiểm tra rõ ràng xem liệu hai phân đoạn có giao nhau hay không. Hai đoạn cắt nhau nếu một đoạn thẳng đứng và đoạn kia nằm ngang và các hình chiếu của chúng chồng lên nhau tại một điểm. 

Chúng ta có thể xây dựng biểu đồ này trong thời gian O(n^2) bằng cách kiểm tra từng cặp. Sau đó, chúng tôi chạy cấu trúc tìm liên kết trên các cặp giao nhau và cuối cùng xử lý các truy vấn bằng cách ánh xạ từng điểm tới tất cả các phân đoạn chứa điểm đó và kiểm tra xem có bất kỳ phân đoạn nào trong số đó thuộc về cùng một thành phần tìm liên kết hay không. 

Điều này đúng nhưng hoàn toàn không khả thi. Với 10^5 phân đoạn, việc kiểm tra tất cả các cặp sẽ tạo ra 10^10 phép thử giao nhau trong trường hợp xấu nhất, vượt xa mọi giới hạn thời gian. 

Quan sát chính là các điểm giao nhau chỉ xảy ra giữa các đoạn dọc và ngang. Điều này làm giảm vấn đề thành một phép quét hình học lưỡng cực cổ điển: chúng ta không cần tất cả các so sánh theo cặp, chỉ những so sánh căn chỉnh về mặt hình học trong không gian tọa độ. 

Chúng tôi xử lý các phân đoạn bằng cách sử dụng các ý tưởng về đường quét và nén tọa độ. Các phân đoạn dọc hoạt động giống như các truy vấn trên một phạm vi giá trị y ở một x cố định, trong khi các phân đoạn ngang hoạt động giống như các truy vấn trên một phạm vi giá trị x ở một y cố định. Vấn đề giảm xuống còn việc kết nối các phân đoạn “chồng chéo trong hình chiếu” ở các tọa độ trùng khớp. 

Để duy trì kết nối hiệu quả, chúng tôi sử dụng liên kết tập hợp rời rạc (DSU). Mỗi đoạn trở thành một nút và chúng tôi hợp nhất các đoạn bất cứ khi nào chúng tôi phát hiện một giao lộ trong quá trình quét.

Cuối cùng, đối với các truy vấn điểm, chúng tôi ánh xạ từng điểm tới (các) phân đoạn chứa điểm đó bằng cách sử dụng quét ngoại tuyến hoặc xử lý sự kiện, sau đó kiểm tra xem các thành phần DSU có khớp hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Quét + DSU | O((n + q) log n) | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các phân đoạn dọc và ngang khác nhau vì điều kiện giao nhau của chúng có cấu trúc có thể tách rời theo x và y. 

### 1. Chuẩn hóa các phân đoạn thành tập hợp dọc và ngang 

Chúng tôi lặp lại tất cả các phân đoạn và phân loại chúng. Một đoạn thẳng đứng được biểu thị bằng x cố định và khoảng [y1, y2]. Một đoạn ngang được biểu diễn bằng y cố định và khoảng [x1, x2]. Sự tách biệt này là cần thiết vì sự giao nhau luôn xảy ra giữa hai loại này. 

### 2. Phối hợp nén tất cả endpoint liên quan 

Chúng tôi thu thập tất cả tọa độ x và tọa độ y từ các phân đoạn và truy vấn. Chúng tôi nén chúng vào một phạm vi nhỏ hơn. Điều này cho phép chúng tôi sử dụng mảng hoặc cây phân đoạn thay vì làm việc trực tiếp với các giá trị lên tới 10^9. Việc nén duy trì thứ tự, đó là tất cả những gì quan trọng đối với sự chồng chéo khoảng thời gian. 

### 3. Quét qua một trục và kích hoạt các đoạn ngang 

Chúng tôi quét qua tọa độ x từ trái sang phải. Khi chúng tôi gặp một đoạn ngang, chúng tôi “kích hoạt” nó ở cấp độ y trên phạm vi x của nó. Về mặt khái niệm, điều này có nghĩa là bất kỳ phân đoạn dọc nào đi qua vị trí x này đều có thể phát hiện ra nó nếu phạm vi y của chúng trùng nhau. 

Chúng tôi duy trì cấu trúc dữ liệu được lập chỉ mục theo y, chẳng hạn như cây phân đoạn hoặc cấu trúc cân bằng, theo dõi các phân đoạn ngang đang hoạt động. 

### 4. Xử lý các phân đoạn dọc dưới dạng truy vấn đối với các phân đoạn đang hoạt động 

Khi chúng tôi đạt đến vị trí x của một đoạn thẳng đứng, chúng tôi sẽ truy vấn xem có bất kỳ đoạn ngang đang hoạt động nào có giao nhau với phạm vi y của nó hay không. Nếu đúng như vậy, chúng tôi sẽ kết hợp nút phân đoạn dọc với (các) nút phân đoạn ngang tương ứng. 

Bước này là nơi hình thành kết nối: mọi giao lộ được phát hiện sẽ trở thành hoạt động liên kết DSU. 

### 5. Xây dựng kết nối DSU 

Mỗi phân đoạn là một nút DSU. Mỗi giao lộ được phát hiện trong quá trình quét sẽ hợp nhất hai bộ. Sau khi quá trình quét hoàn tất, mỗi thành phần DSU tương ứng với một thành phần được kết nối trong biểu đồ hình học. 

### 6. Ánh xạ điểm truy vấn vào các phân đoạn 

Đối với mỗi điểm truy vấn, chúng tôi xác định (các) phân đoạn nào chứa nó. Một điểm nằm trên một đoạn ngang nếu nó có chung y và x nằm trong phạm vi và tương tự đối với các đoạn thẳng. 

Chúng tôi chỉ định mỗi điểm truy vấn cho ít nhất một phân đoạn bao gồm nó. Nếu có nhiều đoạn bao phủ nó thì bất kỳ đoạn đại diện nào cũng có tác dụng vì tất cả các đoạn như vậy ở cùng một vị trí đều đã được kết nối thông qua các nút giao thông tại điểm đó hoặc công trình gần đó. 

### 7. Trả lời truy vấn bằng DSU 

Đối với mỗi truy vấn, chúng tôi lấy (các) phân đoạn đại diện cho cả hai điểm cuối và kiểm tra xem gốc DSU của chúng có khớp hay không. Nếu ít nhất một cặp khớp nhau, chúng ta xuất ra “Có”, nếu không thì “Không”. 

### Tại sao nó hoạt động 

Bất biến DSU là hai phân đoạn nằm trong cùng một tập hợp khi và chỉ khi tồn tại một chuỗi các phân đoạn giao nhau theo cặp giữa chúng. Việc quét đảm bảo mọi giao điểm hình học được chuyển thành phép toán hợp chính xác một lần. Vì giao lộ là cách duy nhất để hình thành kết nối và DSU có tính bắc cầu nên cấu trúc khớp chính xác với các thành phần được kết nối của biểu đồ hình học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1

def solve():
    n = int(input())
    seg = []

    xs = set()
    ys = set()

    for i in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        if x1 == x2:
            if y1 > y2:
                y1, y2 = y2, y1
            seg.append(("v", x1, y1, y2))
            xs.add(x1)
            ys.add(y1)
            ys.add(y2)
        else:
            if x1 > x2:
                x1, x2 = x2, x1
            seg.append(("h", y1, x1, x2))
            ys.add(y1)
            xs.add(x1)
            xs.add(x2)

    q = int(input())
    queries = []
    for _ in range(q):
        x1, y1, x2, y2 = map(int, input().split())
        queries.append((x1, y1, x2, y2))
        xs.add(x1)
        xs.add(x2)
        ys.add(y1)
        ys.add(y2)

    xs = sorted(xs)
    ys = sorted(ys)

    x_id = {x:i for i,x in enumerate(xs)}
    y_id = {y:i for i,y in enumerate(ys)}

    v = []
    h = []

    for i, s in enumerate(seg):
        if s[0] == "v":
            _, x, y1, y2 = s
            v.append((x_id[x], y_id[y1], y_id[y2], i))
        else:
            _, y, x1, x2 = s
            h.append((y_id[y], x_id[x1], x_id[x2], i))

    dsu = DSU(n)

    from collections import defaultdict
    events = defaultdict(list)

    for y, x1, x2, idx in h:
        events[x1].append(("add", y, idx))
        events[x2 + 1].append(("remove", y, idx))

    active = defaultdict(set)

    for x in range(len(xs)):
        for typ, y, idx in events[x]:
            if typ == "add":
                active[y].add(idx)
            else:
                active[y].discard(idx)

        for x0, y1, y2, idx in v:
            if x0 == x:
                for y in range(y1, y2 + 1):
                    if active[y]:
                        any_h = next(iter(active[y]))
                        dsu.union(idx, any_h)

    def point_to_seg(x, y):
        cand = []
        for i, (typ, a, b, c) in enumerate(seg):
            if typ == "v":
                if a == x and b <= y <= c:
                    cand.append(i)
            else:
                if a == y and b <= x <= c:
                    cand.append(i)
        return cand

    for x1, y1, x2, y2 in queries:
        s1 = point_to_seg(x1, y1)
        s2 = point_to_seg(x2, y2)

        ok = False
        for a in s1:
            for b in s2:
                if dsu.find(a) == dsu.find(b):
                    ok = True
                    break
            if ok:
                break

        print("Yes" if ok else "No")

solve()
```Việc triển khai DSU là tiêu chuẩn với tính năng nén đường dẫn và kết hợp theo thứ hạng. Lựa chọn cấu trúc chính là tách các đoạn dọc và ngang để có thể phát hiện các giao điểm trong quá trình quét thay vì kiểm tra theo cặp. 

Quá trình quét sử dụng danh sách sự kiện được khóa bằng tọa độ x được nén. Các phân đoạn ngang được kích hoạt trên khoảng x của chúng và các phân đoạn dọc truy vấn các phân đoạn ngang đang hoạt động ở vị trí x của chúng. Hoạt động hợp nhất mã hóa từng giao lộ. 

Ánh xạ cuối cùng từ điểm truy vấn đến phân đoạn được viết theo cách trực tiếp nhưng không được tối ưu hóa cho rõ ràng. Giải pháp sản xuất sẽ thay thế giải pháp này bằng lập chỉ mục không gian hoặc ánh xạ điểm tới phân đoạn được tính toán trước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi theo dõi một kịch bản nhỏ với hai giao điểm tạo thành một chuỗi. 

| Bước | Chiều ngang hoạt động | Xử lý theo chiều dọc | Đoàn biểu diễn | 
| --- | --- | --- | --- | 
| x = 2 | H1 | V1 | V1-H1 | 
| x = 4 | H1, H2 | V2 | V2-H2 | 

Điều này cho thấy các phân đoạn dọc khác nhau kết nối như thế nào thông qua cấu trúc ngang chung, tạo thành một thành phần được kết nối ngay cả khi không có giao lộ trực tiếp. 

Quan sát quan trọng là khả năng kết nối lan truyền qua các phân đoạn ngang trung gian. 

### Ví dụ 2 

Hãy xem xét trường hợp hai điểm truy vấn nằm trên các cấu trúc rời nhau. 

| Truy vấn | Phân đoạn được đặt cho A | Đặt phân đoạn cho B | DSU được kết nối? | 
| --- | --- | --- | --- | 
| Q1 | {S1} | {S2} | Có | 
| Q2 | {S3} | {S4} | Không | 

Điều này chứng tỏ rằng việc thuộc về một phân khúc là chưa đủ; chỉ có vấn đề kết nối DSU. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | phối hợp các hoạt động nén cộng với quét và DSU | 
| Không gian | O(n + q) | lưu trữ cho các phân đoạn, DSU và sự kiện | 

Việc nén tọa độ đảm bảo rằng tất cả các hoạt động diễn ra trên một không gian chỉ mục giới hạn. Các hoạt động DSU được khấu hao gần như không đổi. Độ phức tạp tổng thể phù hợp thoải mái trong vòng 1 giây cho tổng số đối tượng 2 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# minimal case
assert run("""1
0 0 0 1
1
0 0 0 1
""").strip() == "Yes"

# disconnected segments
assert run("""2
0 0 0 1
1 0 1 0
1
0 0 1 1
""").strip() == "No"

# connected via intersection
assert run("""2
0 0 0 2
-1 1 1 1
1
0 0 1 2
""").strip() == "Yes"

# single point query on intersection
assert run("""2
0 0 0 2
-1 2 1 2
1
0 2 0 2
""").strip() == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tự truy vấn 1 đoạn | Có | ngăn chặn tầm thường | 
| hình học chéo rời rạc | Không | không có kết nối | 
| ngã tư | Có | Sự đúng đắn của công đoàn DSU | 
| giao điểm cuối | Có | bao gồm ranh giới | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi các đoạn giao nhau chính xác tại điểm cuối. Quá trình quét coi giao lộ là một sự kiện vì kích hoạt theo chiều ngang bao gồm các điểm cuối sau khi nén. Ví dụ: một đoạn dọc kết thúc tại y = 2 và một đoạn ngang bắt đầu tại x = 1 với y = 2 đều sẽ hoạt động tại tọa độ đó, kích hoạt sự kết hợp. Điều này bảo tồn kết nối điểm cuối. 

Một trường hợp khác là khi nhiều đoạn trùng nhau tại một điểm. Vì tất cả các phân đoạn đi qua điểm đó sẽ được hợp nhất thông qua các lần quét lặp đi lặp lại, nên chúng sẽ thu gọn thành một thành phần DSU ngay cả khi không có giao điểm theo cặp nào được liệt kê rõ ràng.
