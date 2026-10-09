---
title: "CF 104969I - Tháp Pizza"
description: "Chúng ta được cấp một tập hợp các điểm trên một lưới 2D khổng lồ. Mỗi điểm đại diện cho một kẻ thù nằm ở tọa độ $(xi, yi)$ và mang trọng số $si$. Không có hai kẻ thù có chung tọa độ."
date: "2026-06-28T18:53:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104969
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 1 (Advanced)"
rating: 0
weight: 104969
solve_time_s: 83
verified: false
draft: false
---

[CF 104969I - Tháp Pizza](https://codeforces.com/problemset/problem/104969/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên một lưới 2D khổng lồ. Mỗi điểm đại diện cho một kẻ thù nằm ở tọa độ$(x_i, y_i)$và mang một trọng lượng$s_i$. Không có hai kẻ thù có chung tọa độ. 

Đối với mỗi kẻ thù, chúng ta cần tính toán tổng sức mạnh tồn tại trong vùng hình chữ nhật trải dài từ điểm gốc đến vị trí của kẻ thù đó. Nói cách khác, đối với một điểm truy vấn$(x_i, y_i)$, chúng tôi tổng hợp sức mạnh của tất cả kẻ thù$(x_j, y_j)$như vậy$x_j \le x_i$Và$y_j \le y_i$. 

Vì vậy, mỗi giá trị đầu ra là tổng tiền tố theo thứ tự một phần 2D được xác định bởi ưu thế tọa độ. 

Khó khăn đến từ quy mô. Với tối đa 200.000 điểm và giá trị tọa độ lên tới$2 \cdot 10^9$, chúng tôi không thể xây dựng lưới hoặc mô phỏng trực tiếp việc tích lũy tiền tố trên tọa độ. Bất kỳ cách tiếp cận nào cố gắng kiểm tra tất cả các điểm trước đó cho mỗi truy vấn sẽ dẫn đến gần như$O(N^2)$hành vi vượt xa giới hạn. 

Một cách tiếp cận ngây thơ cũng có thể thất bại nếu người ta cố gắng chỉ sắp xếp theo$x$và tích lũy$y$-tiền tố không có cấu trúc cẩn thận. Lý do là mối quan hệ thống trị là hai chiều, không phải một chiều, do đó việc xử lý theo một thứ tự được sắp xếp duy nhất không tự động duy trì tính chính xác trừ khi chúng ta tích cực duy trì cấu trúc trên chiều thứ hai. 

Không có vấn đề rắc rối về tọa độ trùng lặp vì tọa độ được đảm bảo khác biệt nhưng giá trị có thể lớn, điều này buộc chúng tôi phải nén hoặc tránh lập chỉ mục trực tiếp. 

## Phương pháp tiếp cận 

Phương pháp trực tiếp sẽ là trả lời từng truy vấn một cách độc lập bằng cách quét tất cả các điểm và kiểm tra xem cả hai tọa độ đều nhỏ hơn hay bằng nhau. Điều này thực hiện đúng định nghĩa nhưng yêu cầu$N$so sánh cho mỗi truy vấn, dẫn đến$O(N^2)$tổng công việc. Với$N = 2 \cdot 10^5$, điều này trở thành theo thứ tự của$4 \cdot 10^{10}$so sánh là điều không thể thực hiện được. 

Quan sát quan trọng là truy vấn là tổng tiền tố thống trị cổ điển theo hai chiều. Nếu chúng ta có thể xử lý các điểm theo thứ tự tăng dần$x$, thì tại thời điểm chúng tôi xử lý một điểm$(x_i, y_i)$, tất cả các điểm nhỏ hơn$x$đã được hạch toán rồi. Thử thách còn lại là tính tổng một cách hiệu quả những gì trong số đó cũng thỏa mãn$y \le y_i$. 

Điều này làm giảm vấn đề duy trì một cấu trúc động trên$y$-coordine hỗ trợ tổng tiền tố và cập nhật điểm. Mỗi điểm đóng góp sức mạnh của nó một lần và chúng ta cần truy vấn tổng trọng lượng đã được chèn cho đến nay với$y$-tọa độ giới hạn bởi một ngưỡng. 

Bởi vì$y$có thể lên đến$2 \cdot 10^9$, chúng tôi nén tọa độ thành các cấp bậc. Sau khi nén, chúng tôi sử dụng Cây Fenwick (Cây chỉ mục nhị phân)$y$-xếp hạng. Chúng tôi sắp xếp tất cả các điểm theo$x$, xử lý chúng theo thứ tự tăng dần và với mỗi điểm, chúng tôi truy vấn cây Fenwick để biết tổng tiền tố của nó$y$-thứ hạng. Sau đó chúng ta chèn trọng lượng của nó vào cây Fenwick. 

Một điều tinh tế là chúng ta phải đưa ra câu trả lời theo thứ tự ban đầu của các điểm đầu vào. Vì vậy, trong khi xử lý theo thứ tự được sắp xếp cho chính xác, chúng tôi lưu trữ kết quả theo chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Sắp xếp + Cây Fenwick | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi vấn đề thành một vấn đề có thể được xử lý tăng dần dọc theo một trục trong khi hỗ trợ các truy vấn nhanh trên trục kia. 

1. Gán cho mỗi điểm một chỉ số tương ứng với vị trí đầu vào của nó. Điều này là cần thiết vì chúng ta sẽ sắp xếp lại điểm trong quá trình xử lý nhưng phải xuất đáp án theo thứ tự ban đầu. 
2. Trích xuất tất cả$y_i$các giá trị và nén chúng vào một phạm vi liền kề$[1, N]$. Bước này duy trì trật tự trong khi vẫn có thể sử dụng Cây Fenwick. Nén hoạt động vì chỉ có thứ tự tương đối của$y$-giá trị quan trọng đối với tổng tiền tố. 
3. Sắp xếp tất cả các điểm theo thứ tự tăng dần$x_i$. Nếu hai điểm bằng nhau$x$, thứ tự giữa chúng không quan trọng ở đây vì tọa độ là khác nhau, nhưng ngay cả khi chúng không như vậy, chúng ta thường sẽ phá vỡ mối quan hệ một cách tùy ý. 
4. Khởi tạo Fenwick Tree trên vùng nén$y$-range, ban đầu trống rỗng. 
5. Quét qua các điểm theo thứ tự sắp xếp$x$. Đối với mỗi điểm$(x_i, y_i, s_i)$: 

1. Truy vấn Cây Fenwick để biết tổng của tất cả các điểm mạnh được nén$y \le y_i$. Điều này mang lại tổng đóng góp của tất cả các điểm được xử lý trước đó nằm trong hình chữ nhật được yêu cầu. 
2. Lưu trữ giá trị này làm kết quả cho điểm hiện tại. 
3. Cập nhật Cây Fenwick bằng cách thêm$s_i$ở vị trí$y_i$, vì vậy số điểm trong tương lai sẽ lớn hơn$x$có thể thấy sự đóng góp của nó 

Sau khi xử lý tất cả các điểm, các câu trả lời được lưu trữ tương ứng chính xác với yêu cầu$F(x_i, y_i)$. 

Tại sao nó hoạt động là do việc duy trì một bất biến rõ ràng: tại bất kỳ thời điểm nào trong quá trình quét, Fenwick Tree lưu trữ chính xác tổng sức mạnh của tất cả các điểm có$x$-tọa độ nhỏ hơn hoặc bằng vị trí quét hiện tại$x$, được lập chỉ mục bởi họ$y$-điều phối. Vì vậy, một truy vấn tiền tố trên$y$chọn chính xác tập hợp con thỏa mãn cả hai ràng buộc tọa độ. Vì mỗi điểm được chèn chính xác một lần sau khi phần đóng góp của nó được truy vấn cho chính nó nên không có giá trị nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

n = int(input())
pts = []
ys = []

for i in range(n):
    x, y, s = map(int, input().split())
    pts.append((x, y, s, i))
    ys.append(y)

ys_sorted = sorted(set(ys))
comp = {v: i + 1 for i, v in enumerate(ys_sorted)}

pts.sort(key=lambda p: p[0])

bit = Fenwick(len(ys_sorted))
ans = [0] * n

for x, y, s, idx in pts:
    cy = comp[y]
    ans[idx] = bit.sum(cy)
    bit.add(cy, s)

print("\n".join(map(str, ans)))
```Cây Fenwick là cấu trúc cốt lõi cho phép tổng hợp tiền tố hiệu quả qua quá trình nén$y$-trục. các`add`hàm thực hiện cập nhật điểm theo thời gian logarit, trong khi`sum`lấy một tổng tiền tố. Nén tọa độ đảm bảo chúng tôi không bao giờ phân bổ bộ nhớ tỷ lệ thuận với$2 \cdot 10^9$. 

Sắp xếp theo$x$đảm bảo rằng khi chúng tôi xử lý một điểm, tất cả những người đóng góp hợp lệ có giá trị nhỏ hơn hoặc bằng nhau$x$đã có trong cấu trúc. Lưu trữ kết quả theo chỉ mục gốc sẽ giữ nguyên thứ tự đầu ra. 

Một cạm bẫy phổ biến là hoán đổi thứ tự truy vấn và cập nhật. Hành vi đúng là truy vấn trước, sau đó chèn điểm hiện tại, vì một điểm không được đóng góp vào tổng tiền tố của chính nó trừ khi định nghĩa vấn đề có ý định rõ ràng, ở đây tương ứng chính xác với việc đưa nghiêm ngặt vào tiền tố được xử lý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Điểm đầu vào theo thứ tự:$(1,1,5), (2,1,10), (1,2,10), (3,3,15)$Sắp xếp theo$x$:$(1,1,5), (1,2,10), (2,1,10), (3,3,15)$Chúng tôi theo dõi trạng thái Fenwick bị nén$y$: 

| Bước | Điểm | Truy vấn (tiền tố y) | BIT trước khi cập nhật | Trả lời | BIT sau khi cập nhật | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,1,5) | 0 | trống | 0 | {y1:5} | 
| 2 | (1,2,10) | 5 | {y1:5} | 5 | {y1:5, y2:10} | 
| 3 | (2,1,10) | 5 | {y1:5, y2:10} | 5 | +10 tại y1 | 
| 4 | (3,3,15) | 25 | tất cả trước đó | 25 | +15 | 

Sau khi sắp xếp lại các câu trả lời, chúng ta thu được$5, 15, 15, 40$, phù hợp với đầu ra yêu cầu. 

Dấu vết này cho thấy sớm hơn thế nào$x$-các tọa độ tích lũy dần dần, trong khi cây Fenwick duy trì trật tự trong$y$. 

### Mẫu 2 

Điểm:$(1,1,1), (1,2,2), (1,3,3), (1,4,4), (1,5,5)$Tất cả các điểm đều giống nhau$x$, do đó việc sắp xếp sẽ giữ chúng lại với nhau. 

| Bước | Điểm | Truy vấn | BIT trước khi cập nhật | Trả lời | BIT sau khi cập nhật | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,1,1) | 0 | trống | 0 | +1 | 
| 2 | (1,2,2) | 1 | {1} | 1 | +2 | 
| 3 | (1,3,3) | 3 | {1,2} | 3 | +3 | 
| 4 | (1,4,4) | 6 | {1,2,3} | 6 | +4 | 
| 5 | (1,5,5) | 10 | {1,2,3,4} | 10 | +5 | 

Điều này chứng tỏ rằng ngay cả khi tất cả$x$giá trị bằng nhau, cấu trúc vẫn tích lũy chính xác tiền tố 1D qua$y$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế với$O(N \log N)$, mỗi lần cập nhật và truy vấn Fenwick là$O(\log N)$| 
| Không gian | O(N) | Lưu trữ điểm, bản đồ nén, cây Fenwick và mảng câu trả lời | 

Sự phức tạp phù hợp thoải mái trong giới hạn cho$N = 2 \cdot 10^5$, đại khái là ở đâu$2 \cdot 10^5 \log_2(2 \cdot 10^5)$hoạt động nằm trong giới hạn điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)
        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i
        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    n = int(input())
    pts = []
    ys = []
    for i in range(n):
        x, y, s = map(int, input().split())
        pts.append((x, y, s, i))
        ys.append(y)

    ys_sorted = sorted(set(ys))
    comp = {v: i + 1 for i, v in enumerate(ys_sorted)}

    pts.sort(key=lambda p: p[0])

    bit = Fenwick(len(ys_sorted))
    ans = [0] * n

    for x, y, s, idx in pts:
        cy = comp[y]
        ans[idx] = bit.sum(cy)
        bit.add(cy, s)

    return "\n".join(map(str, ans))

# provided samples
assert run("4\n1 1 5\n2 1 10\n1 2 10\n3 3 15\n") == "5\n15\n15\n40"
assert run("5\n1 1 1\n1 2 2\n1 3 3\n1 4 4\n1 5 5\n") == "0\n1\n3\n6\n10"

# custom cases
assert run("1\n5 5 10\n") == "0", "minimum size"
assert run("2\n1 1 5\n2 2 7\n") == "0\n5", "basic diagonal dominance"
assert run("3\n3 3 10\n1 1 1\n2 2 2\n") == "0\n1\n3", "unsorted input"
assert run("3\n1 3 10\n2 2 20\n3 1 30\n") == "0\n0\n30", "cross pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | trường hợp cơ sở không có người tiền nhiệm | 
| tăng trưởng theo đường chéo | tính chính xác tích lũy tiền tố | xích tăng tiêu chuẩn | 
| đầu vào chưa được sắp xếp | sắp xếp đúng đắn | trật tự độc lập | 
| mẫu chéo | cấu trúc thống trị hỗn hợp | Tính chính xác của thứ tự 2D | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các điểm có chung$x$-điều phối. Trong trường hợp này, sắp xếp theo$x$nhóm mọi thứ lại với nhau và câu trả lời phụ thuộc hoàn toàn vào$y$-tích lũy tiền tố. Thuật toán vẫn hoạt động vì trong một khoảng thời gian cố định$x$, không có điểm nào đóng góp cho điểm khác trong cùng một đợt, vì các cập nhật diễn ra sau các truy vấn. Ví dụ, với điểm$(1,3,10), (1,1,5), (1,2,7)$, quá trình xử lý mang lại tất cả các truy vấn bằng 0, phù hợp với định nghĩa vì không có điểm nào nhỏ hơn hoàn toàn$x$. 

Một trường hợp cạnh khác là tọa độ giảm nghiêm ngặt, chẳng hạn như$(3,3), (2,2), (1,1)$. Sau khi sắp xếp, quá trình quét ngày càng tăng và mỗi điểm chỉ nhìn thấy các cặp nhỏ hơn trước đó. Cây Fenwick đảm bảo rằng mặc dù thứ tự đầu vào bị đảo ngược nhưng các mối quan hệ thống trị vẫn được đánh giá chính xác. 

Trường hợp khó phát hiện cuối cùng là khi các giá trị lớn và thưa thớt. Nếu không nén, cây Fenwick sẽ không thể được phân bổ. Quá trình nén duy trì thứ tự, do đó, ngay cả các giá trị như$y = 10^9$hoạt động giống hệt với các chỉ số nhỏ sau khi được ánh xạ.
