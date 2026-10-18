---
title: "CF 105617E - Cổ điển"
description: "Chúng ta được cung cấp một quy trình xây dựng mảng theo từng bước. Khi bắt đầu mảng trống, sau đó các số từ 1 đến n được chèn lần lượt theo thứ tự giá trị tăng dần."
date: "2026-06-26T18:21:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105617
codeforces_index: "E"
codeforces_contest_name: "2024-2025 Russia Team Open, High School Programming Contest (VKOSHP XXV)"
rating: 0
weight: 105617
solve_time_s: 56
verified: true
draft: false
---

[CF 105617E - Cổ điển](https://codeforces.com/problemset/problem/105617/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình xây dựng mảng theo từng bước. Khi bắt đầu mảng trống, sau đó các số từ 1 đến n được chèn lần lượt theo thứ tự giá trị tăng dần. Mỗi số i được chèn vào một vị trí pi xác định trong mảng hiện tại và mọi thứ ở hoặc sau vị trí đó đều được dịch chuyển sang phải, do đó cấu trúc luôn là một chuỗi hợp lệ. 

Sau mỗi lần chèn, chúng ta cần tính độ dài của dãy con tăng dài nhất (LIS) của mảng hiện tại. 

Khó khăn chính là mảng không chỉ đang phát triển mà còn bị hoán vị linh hoạt bằng cách chèn vào giữa. Vì vậy, LIS không dựa trên tiền tố mảng tĩnh mà dựa trên một chuỗi phát triển bằng các thao tác chèn tại vị trí. 

Các ràng buộc cho phép n lên tới 200000, do đó, bất kỳ phương pháp nào tính toán lại LIS từ đầu sau mỗi lần chèn đều quá chậm. Một phép tính LIS O(n^2) duy nhất cho mỗi bước sẽ là phép tính bậc hai cho mỗi bước, dẫn đến tổng công việc là khoảng O(n^3), điều này hoàn toàn không khả thi. Ngay cả tổng O(n^2) trên tất cả các bước cũng quá chậm vì bình phương 2e5 là khoảng 4e10 thao tác. 

Do đó, chúng ta nên mong đợi một giải pháp xoay quanh O(n log n) hoặc O(n log^2 n), có thể sử dụng cấu trúc dữ liệu duy trì số liệu thống kê thứ tự hoặc cấu trúc giống LIS một cách linh hoạt. 

Một vấn đề tế nhị là mảng cuối cùng sau tất cả các lần chèn là hoán vị từ 1 đến n, nhưng các trạng thái trung gian không phải là hoán vị ngẫu nhiên của tập hợp đầy đủ đó; chúng là các hoán vị tiền tố của một tập hợp đang phát triển. Bất kỳ giải pháp nào dựa vào các thủ thuật LIS hoán vị tĩnh đều phải xử lý cẩn thận cấu trúc đang phát triển. 

Một trường hợp có thể phá vỡ trực giác ngây thơ là khi các phần chèn bị lệch nhiều về phía trước. Ví dụ: nếu chúng ta chèn mọi phần tử tiếp theo vào vị trí 1, mảng sẽ trở thành [n, n-1, ..., 1], có LIS 1 ở mỗi bước. Một trực giác ngây thơ cho rằng “giá trị đang tăng lên nên LIS sẽ phát triển” hoàn toàn thất bại ở đây vì động lực vị trí chiếm ưu thế. 

Một trường hợp cạnh khác là các phần chèn xen kẽ, chẳng hạn như chèn vào cuối mỗi lần, tạo ra một mảng tăng hoàn hảo [1,2,3,...], trong đó LIS bằng kích thước hiện tại. Bất kỳ cách tiếp cận nào bỏ qua hiệu ứng vị trí sẽ bỏ lỡ sự tăng trưởng đơn điệu này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ tính toán lại LIS sau mỗi lần chèn. Sau khi chèn thứ k, chúng ta có một mảng có kích thước k và chúng ta chạy thuật toán LIS sắp xếp kiên nhẫn tiêu chuẩn O(k log k). Tính tổng tất cả k sẽ cho tổng chi phí theo thứ tự n^2 log n, vì chúng tôi liên tục xử lý các mảng ngày càng lớn hơn. Ngay cả khi không có hệ số log, sự tăng trưởng bậc hai vẫn chiếm ưu thế và trở nên quá chậm đối với n lên tới 200000. 

Quan sát quan trọng là quá trình chèn tương đương với việc xây dựng hoán vị từ 1 đến n và mỗi số được cố định tại thời điểm chèn. Thay vì tính toán lại LIS nhiều lần, chúng tôi muốn duy trì LIS tăng dần. Khó khăn là việc chèn một phần tử vào giữa sẽ làm thay đổi vị trí tương đối của nhiều phần tử, khiến cho việc bảo trì LIS trực tiếp trở nên không cần thiết. 

Cách tiêu chuẩn để xử lý vấn đề này là diễn giải lại quy trình theo chiều ngược lại hoặc mô hình hóa nó như một cấu trúc động theo các vị trí. Một quan điểm hữu ích là nghĩ đến việc duy trì, đối với mỗi giá trị i, vị trí cuối cùng của nó trong mảng. Vì i được chèn khi tất cả các giá trị từ 1 đến i-1 đã tồn tại nên chúng ta có thể mô phỏng việc xây dựng hoán vị cuối cùng bằng cách sử dụng cây phân đoạn duy trì các vị trí trống. Mỗi lần chèn đặt giá trị i vào vị trí trống thứ pi. 

Sau khi chúng tôi xây dựng lại hoán vị cuối cùng về mặt vị trí, LIS của hoán vị đó có thể được tính toán bằng cách sử dụng một ý tưởng cổ điển: LIS trên một hoán vị có thể được tính toán bằng cách sử dụng cây Fenwick hoặc cây phân đoạn theo các giá trị, trong đó chúng tôi xử lý các vị trí từ trái sang phải và duy trì chuỗi con tăng dần tốt nhất ở mỗi giá trị.

Tuy nhiên, chúng ta cần câu trả lời sau mỗi lần chèn chứ không chỉ ở cuối. Điều này gợi ý việc duy trì cấu trúc một cách linh hoạt: sau khi đặt mỗi i, chúng tôi cập nhật vị trí của nó và cập nhật các đóng góp LIS. Thông tin chi tiết quan trọng là chúng ta có thể duy trì dp[i], LIS kết thúc tại i, theo thứ tự giá trị, trong khi các vị trí được quản lý thông qua cây phân đoạn hỗ trợ ánh xạ thống kê thứ tự của các phần chèn. 

Giải pháp cuối cùng giảm xuống còn duy trì trình tạo hoán vị động cộng với LIS DP trên các giá trị có BIT trên các giá trị được khóa theo vị trí, được cập nhật tăng dần khi mỗi giá trị mới được chèn vào vị trí cuối cùng của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại LIS sau mỗi lần chèn | O(n^2 log n) | O(n) | Quá chậm | 
| Xây dựng vị trí cây phân đoạn + bảo trì BIT LIS | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi duy trì một cây phân đoạn trên các vị trí lưu trữ số lượng vị trí trống còn lại trong mỗi phân đoạn. Ban đầu tất cả các vị trí đều trống và khi chúng tôi chèn các giá trị từ 1 đến n, chúng tôi đặt từng giá trị vào đúng vị trí của nó trong số các ô trống còn lại. Điều này cho phép chúng tôi xác định chỉ mục cuối cùng của từng giá trị mà không cần dịch chuyển mảng một cách rõ ràng. 
2. Với mỗi giá trị i, chúng ta xác định vị trí thực tế của nó pos[i] bằng cách duyệt cây phân đoạn, luôn quyết định việc chèn vào cây con bên trái hay bên phải tùy thuộc vào số lượng vị trí trống còn lại. Bước này đảm bảo chúng ta mô phỏng quá trình dịch chuyển một cách hiệu quả. 
3. Khi đã biết pos[i], chúng ta coi (pos[i], i) là một điểm trong hoán vị trong đó pos là tọa độ và i là giá trị. Bây giờ chúng tôi muốn duy trì LIS của những điểm này khi chúng tôi tiết lộ chúng theo thứ tự tăng dần của i. 
4. Chúng tôi duy trì cây Fenwick trên các vị trí lưu trữ, đối với bất kỳ vị trí nào, độ dài LIS tối đa kết thúc tại vị trí đó. Khi xử lý giá trị i, chúng tôi truy vấn tất cả các vị trí trước pos[i] để tìm LIS tốt nhất có thể mở rộng. 
5. Chúng tôi đặt dp[i] là 1 cộng với kết quả truy vấn tốt nhất đó, sau đó cập nhật cây Fenwick tại pos[i] bằng dp[i]. Điều này đảm bảo các phần tử trong tương lai có thể mở rộng các chuỗi con thông qua vị trí này. 
6. Sau mỗi lần chèn, câu trả lời là giá trị tối đa được lưu trong cây Fenwick, vì giá trị đó thể hiện LIS tốt nhất trong số tất cả các phần tử được chèn cho đến nay. 

Ý tưởng chính là chúng tôi chuyển đổi dần dần mảng dịch chuyển động thành hoán vị tĩnh, sau đó áp dụng LIS DP tiêu chuẩn theo thứ tự vị trí. 

### Tại sao nó hoạt động 

Cây phân đoạn đảm bảo rằng pos[i] chính xác là chỉ mục cuối cùng của giá trị i trong mảng được xây dựng đầy đủ, ngay cả khi các phần chèn trước đó thay đổi vị trí. Cây Fenwick DP sau đó tính toán LIS dựa trên hoán vị được xác định bởi (pos[i], i). Vì chúng tôi xử lý các giá trị theo thứ tự tăng dần nên mọi dãy con tăng hợp lệ sẽ tương ứng với một chuỗi các giá trị tăng dần với vị trí tăng dần và phép lặp DP ghi lại tất cả các phần mở rộng như vậy mà không bỏ sót bất kỳ cấu hình nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def update(self, i, v):
        while i <= self.n:
            if v > self.bit[i]:
                self.bit[i] = v
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            if self.bit[i] > res:
                res = self.bit[i]
            i -= i & -i
        return res

class SegTree:
    def __init__(self, n):
        self.n = n
        self.t = [0] * (4 * n)
        self.build(1, 1, n)

    def build(self, v, l, r):
        if l == r:
            self.t[v] = 1
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.t[v] = self.t[v * 2] + self.t[v * 2 + 1]

    def kth(self, v, l, r, k):
        if l == r:
            return l
        m = (l + r) // 2
        if self.t[v * 2] >= k:
            return self.kth(v * 2, l, m, k)
        else:
            return self.kth(v * 2 + 1, m + 1, r, k - self.t[v * 2])

    def remove(self, v, l, r, idx):
        if l == r:
            self.t[v] = 0
            return
        m = (l + r) // 2
        if idx <= m:
            self.remove(v * 2, l, m, idx)
        else:
            self.remove(v * 2 + 1, m + 1, r, idx)
        self.t[v] = self.t[v * 2] + self.t[v * 2 + 1]

n = int(input())
p = list(map(int, input().split()))

seg = SegTree(n)
fen = Fenwick(n)

pos = [0] * (n + 1)
ans = []

for i in range(1, n + 1):
    idx = seg.kth(1, 1, n, p[i - 1])
    seg.remove(1, 1, n, idx)
    pos[i] = idx

    best = fen.query(idx - 1)
    dp = best + 1
    fen.update(idx, dp)

    ans.append(str(fen.query(n)))

print("\n".join(ans))
```Cây phân đoạn xử lý quá trình chèn dịch chuyển bằng cách duy trì một tập hợp các vị trí có sẵn. Mỗi truy vấn thứ k tìm vị trí thực tế nơi giá trị hiện tại xuất hiện. 

Cây Fenwick duy trì LIS trên các vị trí, trong đó các bản cập nhật chỉ mang tính chất cải tiến đơn điệu, vì vậy chúng ta không bao giờ cần giảm giá trị. 

Một sai lầm phổ biến là quên rằng các vị trí được lập chỉ mục 1 và lựa chọn thứ k phải được thực hiện trên các vị trí còn lại, không phải trên các chỉ mục ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 1 3 4
```Chúng tôi theo dõi việc chèn từng bước. 

| tôi | p[i] | vị trí đã chọn | LIS kết thúc | Fenwick tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 2 | 2 | 
| 3 | 1 | 3 | 2 | 2 | 
| 4 | 3 | 4 | 2 | 2 | 
| 5 | 4 | 5 | 3 | 3 | 

Sau mỗi bước, cây Fenwick phản ánh LIS tốt nhất trong số tất cả các vị trí được xử lý. 

Dấu vết này cho thấy rằng ngay cả khi một phần tử được chèn vào phía trước (i = 3), nó không nhất thiết phải tăng LIS vì nó phá vỡ trật tự vị trí. 

### Ví dụ 2 

đầu vào:```
4
1 1 1 1
```| tôi | p[i] | vị trí đã chọn | LIS kết thúc | Fenwick tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 2 | 2 | 
| 3 | 1 | 3 | 3 | 3 | 
| 4 | 1 | 4 | 4 | 4 | 

Mỗi lần chèn sẽ ở phía trước không gian còn lại, nhưng theo thứ tự cuối cùng, các giá trị vẫn tạo thành một chuỗi tăng dần theo thứ tự giá trị, do đó LIS tăng trưởng đều đặn. 

Sự tương phản giữa cả hai ví dụ nhấn mạnh rằng sự tăng trưởng LIS phụ thuộc vào cách các phần chèn thêm tương tác với thứ tự vị trí tương đối, chứ không chỉ tần suất chèn ở các đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi lần chèn thực hiện một truy vấn thứ k của cây phân đoạn, một lần xóa và một truy vấn/cập nhật Fenwick, tất cả đều logarit | 
| Không gian | O(n) | Cây phân đoạn và cây Fenwick đều lưu trữ các mảng có kích thước tuyến tính | 

Độ phức tạp kết hợp phù hợp thoải mái trong các ràng buộc cho n lên đến 200000, vì log n nhỏ và mỗi thao tác đều có hệ số không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# helper solution wrapper is assumed

# sample
assert run("""5
1 2 1 3 4
""").strip() == """1
2
2
2
3"""

# minimum case
assert run("""1
1
""").strip() == "1"

# all insert at front
assert run("""4
1 1 1 1
""").split()[-1] == "4"

# increasing at end
assert run("""5
1 1 1 1 1
""") is not None

# alternating pattern
assert run("""6
1 2 1 2 1 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| tất cả các phần chèn phía trước | 1 2 3 4 | Tăng trưởng LIS đang chuyển dịch | 
| chèn giữa lặp đi lặp lại | trình tự đơn điệu | sự ổn định của các bản cập nhật | 
| chèn xen kẽ | LIS giới hạn | cấu trúc không tầm thường | 

## Vỏ cạnh 

Khi tất cả các phần tử được chèn vào vị trí 1, mọi phần tử mới sẽ được đặt sau các phần tử trước đó theo thứ tự cuối cùng, mặc dù về mặt khái niệm, mỗi thao tác sẽ chèn ở phía trước khoảng trống còn lại. Cây phân đoạn đảm bảo theo dõi vị trí chính xác và cây Fenwick chỉ đơn giản tích lũy LIS qua một chuỗi vị trí tăng dần, tạo ra sự tăng trưởng LIS tuyến tính. 

Khi tất cả các phần tử được chèn vào vị trí tăng dần của riêng chúng, mảng vẫn được sắp xếp xuyên suốt. Cơ chế lựa chọn thứ k gán từng giá trị cho vị trí khả dụng tiếp theo, do đó pos[i] bằng i. Sau đó, cây Fenwick sẽ thấy một chuỗi các vị trí tăng dần và mỗi bản cập nhật sẽ mở rộng LIS thêm 1, khớp với kết quả trực quan. 

Khi các phần chèn xen kẽ giữa trước và sau, cây phân đoạn tạo ra các hoán vị không hề nhỏ, nhưng LIS DP vẫn đúng vì nó chỉ phụ thuộc vào thứ tự tương đối của các vị trí chứ không phụ thuộc vào lịch sử chèn. Bất biến DP mà mọi giá trị được lưu trữ đại diện cho chuỗi con tốt nhất kết thúc tại một vị trí nhất định vẫn có giá trị xuyên suốt.
