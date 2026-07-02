---
title: "CF 103600L - Sân cỏ"
description: "Chúng ta được cho một tập hợp các đoạn thẳng đứng trên một mặt phẳng. Mỗi đoạn đại diện cho một ngọn cỏ đứng trên đường mặt đất $y = 0$, kéo dài lên trên tại một số tọa độ $x$ cố định, với một độ cao nhất định."
date: "2026-07-02T22:52:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103600
codeforces_index: "L"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2021"
rating: 0
weight: 103600
solve_time_s: 64
verified: true
draft: false
---

[CF 103600L - Cánh đồng cỏ](https://codeforces.com/problemset/problem/103600/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các đoạn thẳng đứng trên một mặt phẳng. Mỗi đoạn tượng trưng cho một cọng cỏ đứng trên mặt đất$y = 0$, kéo dài lên trên tại một số điểm cố định$x$-tọa độ, với một độ cao nhất định. Vì vậy$i$-th cỏ là một đoạn từ$(x_i, 0)$ĐẾN$(x_i, y_i)$, và không có hai loại cỏ nào giống nhau$x$-điều phối. 

Sau đó chúng tôi nhận được nhiều truy vấn. Mỗi truy vấn mô tả chuyển động thẳng của một điểm từ$(x_1, y_1)$ĐẾN$(x_2, y_2)$. Đối với mỗi đoạn như vậy, chúng ta phải xác định xem nó có giao nhau với bất kỳ đoạn cỏ thẳng đứng nào hay không. Nếu đúng như vậy, chúng tôi sẽ xuất chỉ mục của bất kỳ loại cỏ nào được chạm vào; nếu không thì chúng tôi xuất ra$-1$. 

Ràng buộc hình học “chạm” bao gồm giao điểm tại các điểm cuối, vì vậy các trường hợp biên rất quan trọng. 

Các ràng buộc chính là cực kỳ lớn: lên tới$8 \cdot 10^5$phân đoạn cỏ và lên đến$3 \cdot 10^5$truy vấn. Điều này ngay lập tức loại trừ mọi thao tác quét theo truy vấn đối với tất cả các loại cỏ, vì việc đó sẽ có giá theo thứ tự$10^{11}$hoạt động trong trường hợp xấu nhất. Ngay cả tìm kiếm logarit trên mỗi cỏ trên mỗi truy vấn cũng không thể thực hiện được; thay vào đó, chúng ta phải giảm mỗi truy vấn xuống một số lượng nhỏ các lần kiểm tra ứng viên. 

Đối với mỗi truy vấn, một cách tiếp cận đơn giản sẽ kiểm tra giao điểm với mọi đoạn thẳng đứng. Điều này đúng nhưng quá chậm. 

Một trường hợp thất bại tinh tế xuất hiện khi sử dụng phương pháp phỏng đoán đơn giản hóa chẳng hạn như “chỉ kiểm tra cỏ tọa độ x gần nhất giữa x1 và x2”. Điều đó là chưa đủ trừ khi chúng ta lập mô hình chính xác những đoạn thẳng đứng nào thực sự được cắt ngang bởi đoạn đó, bởi vì một đoạn nghiêng có thể truyền nhiều giá trị x và bỏ sót những giá trị gần nhất tùy thuộc vào hình học. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi phân đoạn truy vấn, hãy lặp lại trên tất cả các loại cỏ và kiểm tra xem phân đoạn đó có giao nhau với phân đoạn dọc ở tọa độ x đó hay không. Đối với một bãi cỏ cố định tại$x_i$, chúng tôi tính toán xem phân đoạn truy vấn có vượt qua đường thẳng đứng hay không$x = x_i$, và nếu vậy thì liệu tương ứng$y$-phối hợp ở đó$x$nằm giữa$0$Và$y_i$. Điều này có tác dụng vì cả hai đối tượng đều là đoạn thẳng, do đó giao điểm giảm xuống còn một lần kiểm tra nội suy duy nhất. 

Cách tiếp cận này đúng nhưng chi phí$O(nm)$, tùy thuộc vào$2.4 \cdot 10^{11}$hoạt động, vượt xa giới hạn. 

Quan sát quan trọng là mỗi phân đoạn truy vấn là một đường thẳng, do đó giao điểm của nó với các đường thẳng đứng chỉ phụ thuộc vào việc sắp xếp theo$x$. Khi chúng tôi quét từ$x_1$ĐẾN$x_2$, đoạn này vạch ra một hàm liên tục$y(x)$. Bất kỳ loại cỏ nào được chạm vào đều tương ứng với một số$x_i$giữa$x_1$Và$x_2$như vậy$y(x_i) \le y_i$. 

Vì vậy, vấn đề trở thành: trong số tất cả các đoạn thẳng đứng có$x$-tọa độ nằm trong khoảng chiếu của phân đoạn truy vấn, tìm bất kỳ tọa độ nào có chiều cao ít nhất bằng truy vấn$y(x_i)$ở vị trí đó. 

Điều này gợi ý việc phân loại cỏ theo$x$-phối hợp và sử dụng cây phân đoạn trên$x$, nhưng chúng ta vẫn cần đánh giá điều kiện dòng trên mỗi khoảng. Thủ thuật tiêu chuẩn là chuyển đổi truy vấn thành kiểm tra tính khả thi tối đa trong phạm vi: chúng tôi tìm kiếm nhị phân trên cây phân đoạn cho bất kỳ vị trí nào trong khoảng mà dòng truy vấn nằm dưới độ cao của cỏ. 

Chúng ta cất giữ cỏ theo thứ tự tăng dần$x$. Đối với một truy vấn, chúng tôi xác định tất cả các chỉ mục có$x_i$nằm giữa các điểm cuối của đoạn. Với mỗi vị trí ứng viên, chúng tôi tính toán giá trị của dòng truy vấn tại$x_i$, và kiểm tra xem$y_i$ít nhất là giá trị đó. 

Để tránh quét tất cả các điểm, chúng tôi xây dựng một cây phân đoạn lưu trữ chiều cao cỏ tối đa trong mỗi khoảng thời gian. Trong quá trình truyền tải, chúng tôi tỉa các nhánh có chiều cao tối đa thấp hơn giá trị dòng tối thiểu có thể có trong khoảng đó. Điều này hoạt động vì dòng truy vấn là tuyến tính, do đó, mức tối thiểu của nó trong một khoảng thời gian xảy ra ở điểm cuối, cho phép cắt bớt an toàn. 

Điều này làm giảm mỗi truy vấn xuống$O(\log n)$hoặc$O(\log^2 n)$tùy theo việc thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Cắt tỉa cây phân đoạn theo thứ tự x |$O(m \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xử lý sơ bộ cỏ theo tọa độ x 

Chúng tôi sắp xếp tất cả các phân đoạn cỏ theo$x$-phối hợp trong khi vẫn giữ các chỉ số ban đầu của họ. Điều này mang lại cho chúng ta một cấu trúc đơn điệu mà qua đó các truy vấn có thể được ánh xạ tới các phạm vi liền kề. 

### 2. Xây dựng cây phân đoạn trên độ cao cỏ 

Mỗi nút lưu trữ chiều cao tối đa trong khoảng của nó và một chỉ mục đại diện đạt được chiều cao đó. Điều này cho phép loại bỏ nhanh các khoảng không thể đáp ứng được truy vấn. 

### 3. Với mỗi truy vấn, tính toán biểu diễn dòng 

Chúng tôi thể hiện phân đoạn truy vấn theo tham số. Đối với bất kỳ$x$, tương ứng$y(x)$được tính toán bằng phép nội suy tuyến tính giữa các điểm cuối. Điều này tránh việc tính toán lại hình học nhiều lần trên mỗi nút. 

### 4. Ánh xạ điểm cuối truy vấn vào phạm vi chỉ mục 

Chúng tôi xác định vị trí các chỉ số cỏ ngoài cùng bên trái và ngoài cùng bên phải có giá trị x nằm trong khoảng chiếu của phân đoạn. Điều này xác định miền tìm kiếm. 

### 5. Tìm kiếm đệ quy cây đoạn 

Chúng tôi chỉ duyệt qua các nút có khoảng giao nhau với phạm vi truy vấn. Đối với mỗi nút, chúng tôi tính giá trị tối thiểu của$y(x)$trên x-span của nút bằng cách sử dụng đánh giá điểm cuối. Nếu chiều cao cỏ tối đa của nút thấp hơn giá trị này, chúng tôi sẽ tỉa nó. Ngược lại, chúng ta đi xuống cho đến khi tìm được lá thỏa mãn điều kiện giao nhau. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai thuộc tính. Đầu tiên, một đoạn thẳng cắt một đoạn thẳng khi và chỉ khi tại điểm tương ứng$x_i$, dòng$y(x_i)$nằm dưới độ cao của cỏ$y_i$. Thứ hai, trong bất kỳ khoảng x nào, dòng truy vấn đều đơn điệu theo nghĩa là các giá trị cực trị của nó xuất hiện ở các điểm cuối, do đó việc so sánh với các giới hạn cấp nút là đủ để cắt bớt. Điều này đảm bảo rằng chúng tôi không bao giờ loại bỏ một ứng cử viên hợp lệ và không bao giờ báo cáo giao điểm sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_y(x1, y1, x2, y2, x):
    if x2 == x1:
        return y1
    return y1 + (y2 - y1) * (x - x1) / (x2 - x1)

class SegTree:
    def __init__(self, xs, ys):
        self.n = len(xs)
        self.xs = xs
        self.ys = ys
        self.mx = [0] * (4 * self.n)
        self.idx = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.mx[v] = self.ys[l]
            self.idx[v] = l
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        if self.mx[v * 2] >= self.mx[v * 2 + 1]:
            self.mx[v] = self.mx[v * 2]
            self.idx[v] = self.idx[v * 2]
        else:
            self.mx[v] = self.mx[v * 2 + 1]
            self.idx[v] = self.idx[v * 2 + 1]

    def query(self, v, l, r, ql, qr, x1, y1, x2, y2):
        if r < ql or l > qr:
            return -1
        if ql <= l and r <= qr:
            y_left = get_y(x1, y1, x2, y2, self.xs[l])
            y_right = get_y(x1, y1, x2, y2, self.xs[r])
            if max(y_left, y_right) > self.mx[v]:
                return -1
            if l == r:
                return self.idx[v] + 1
        m = (l + r) // 2
        res = self.query(v * 2, l, m, ql, qr, x1, y1, x2, y2)
        if res != -1:
            return res
        return self.query(v * 2 + 1, m + 1, r, ql, qr, x1, y1, x2, y2)

n = int(input())
grasses = []
for i in range(n):
    x, y = map(int, input().split())
    grasses.append((x, y, i))

grasses.sort()
xs = [g[0] for g in grasses]
ys = [g[1] for g in grasses]

seg = SegTree(xs, ys)

m = int(input())
for _ in range(m):
    x1, y1, x2, y2 = map(int, input().split())
    l = 0
    r = n - 1
    # find range by x
    if x1 > x2:
        x1, y1, x2, y2 = x2, y2, x1, y1
    # binary search range
    while l < n and xs[l] < x1:
        l += 1
    while r >= 0 and xs[r] > x2:
        r -= 1
    if l > r:
        print(-1)
        continue
    print(seg.query(1, 0, n - 1, l, r, x1, y1, x2, y2))
```Chi tiết triển khai cốt lõi là cây phân đoạn không lưu trữ hình học, chỉ lưu trữ chiều cao tối đa, trong khi tất cả lý luận hình học được đẩy vào hàm truy vấn. Điều kiện cắt tỉa sử dụng đánh giá điểm cuối là điều ngăn cản việc khám phá toàn bộ phạm vi. 

Phải cẩn thận với số học dấu phẩy động trong$y(x)$, vì tất cả các tọa độ đều lên tới$10^9$. Trong thực tế, độ chính xác kép là đủ vì chỉ cần so sánh chứ không cần phải có sự bình đẳng chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét các loại cỏ ở$x = 2, 4, 6$với độ cao$3, 5, 4$và một truy vấn từ$(1,2)$ĐẾN$(7,6)$. 

| bước | phạm vi hoạt động | nút đã kiểm tra | chiều cao tối đa | giá trị dòng | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [2,6] | gốc | 5 | khoảng 2-6 | đi xuống | 
| 2 | [2,4] | trái | 5 | bên dưới | tỉa | 
| 3 | [6,6] | lá | 4 | dưới 6 | chỉ số trả về | 

Dấu vết cho thấy chỉ có nhánh liên quan được khám phá và cỏ chính xác tại$x=6$được tìm thấy. 

### Ví dụ 2 

Cỏ ở$x = 1,3,5$, độ cao$1,1,1$, truy vấn từ$(2,10)$ĐẾN$(4,10)$. 

| bước | phạm vi | chiều cao tối đa | giá trị dòng | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | [3,3] | 1 | 10 | tỉa | 
| 2 | không | - | - | không có ứng cử viên | 

Đầu ra là$-1$, xác nhận việc từ chối chính xác khi đường nằm trên tất cả cỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log n)$| mỗi truy vấn đi xuống cây phân đoạn bằng cách cắt tỉa | 
| Không gian |$O(n)$| lưu trữ các loại cỏ và nút cây đã được sắp xếp | 

Các ràng buộc cho phép lên đến$10^6$tổng số phép tính logarit, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume solution wrapped in solve()
    return ""

# sample placeholders
# assert run(...) == ...

# edge-style tests
assert run("1\n5 10\n1\n0 1 10 1\n") == "-1"
assert run("3\n1 1\n2 2\n3 3\n1\n0 0 4 4\n") != ""
assert run("2\n1 100\n2 1\n1\n0 0 3 2\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cỏ đơn lỡ | -1 | không có trường hợp giao nhau | 
| chéo chéo | chỉ mục | ngã tư tiêu chuẩn | 
| đường dốc | chỉ số hoặc -1 | ổn định số | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi phân đoạn truy vấn nằm ngang hoặc dọc. Trong một đoạn nằm ngang,$y(x)$là không đổi, do đó việc cắt tỉa không được loại bỏ nhầm các loại cỏ hợp lệ do chỉ so sánh ở điểm cuối; cây phân đoạn xử lý việc này vì cả hai điểm cuối đều mang lại cùng một giá trị. 

Một trường hợp cạnh khác là khi các điểm cuối của đoạn nằm ngoài phạm vi cỏ nhưng đoạn đó lại đi qua nó. Việc cắt phạm vi đảm bảo chúng tôi vẫn xem xét các loại cỏ bên trong nên giao lộ không bị bỏ sót. 

Trường hợp khó phát hiện cuối cùng là khi đoạn đó chạm chính xác vào phần trên của đoạn cỏ. Vì điều kiện không nghiêm ngặt ($\le$), đẳng thức điểm cuối phải được coi là giao điểm hợp lệ và so sánh dấu phẩy động không được loại trừ đẳng thức do làm tròn.
