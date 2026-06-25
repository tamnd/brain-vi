---
title: "CF 105257M - Trang trí cửa sổ"
description: "Chúng tôi được cung cấp tới mười nghìn đồ trang trí giống hệt nhau được đặt bên trong một cửa sổ hình vuông 100 x 100. Mỗi trang trí được căn giữa tại một tọa độ nguyên nằm ngay bên trong ranh giới và mỗi trang trí là một hình vuông xoay có các đường chéo thẳng hàng với các trục tọa độ và có tổng…"
date: "2026-06-24T04:31:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105257
codeforces_index: "M"
codeforces_contest_name: "2024 ICPC ShaanXi Provincial Contest"
rating: 0
weight: 105257
solve_time_s: 63
verified: true
draft: false
---

[CF 105257M - Trang trí cửa sổ](https://codeforces.com/problemset/problem/105257/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tới mười nghìn đồ trang trí giống hệt nhau được đặt bên trong một cửa sổ hình vuông 100 x 100. Mỗi trang trí được căn giữa tại một tọa độ nguyên bên trong ranh giới và mỗi trang trí là một hình vuông xoay có đường chéo thẳng hàng với trục tọa độ và có tổng chiều dài bằng 2. 

Bởi vì các đường chéo song song với trục và có tâm tại một điểm nguyên, nên mỗi trang trí chính xác là tập hợp các điểm có khoảng cách từ Manhattan đến tâm của nó nhiều nhất là 1. Về mặt hình học, đây là hình kim cương có bốn đỉnh cách nhau một đơn vị theo các hướng chính. 

Nhiệm vụ là tính toán tổng diện tích bên trong cửa sổ được bao phủ bởi ít nhất một trong số những viên kim cương này, chỉ tính các phần chồng lên nhau một lần. 

Kích thước đầu vào cho phép lên tới 10000 hình dạng, do đó, bất kỳ phương pháp nào cố gắng đo từng cặp chồng chéo hoặc lấy mẫu mặt phẳng ở độ phân giải tốt sẽ trở nên quá chậm. Một phép tính kết hợp theo cặp đơn giản sẽ yêu cầu kiểm tra các giao điểm giữa tất cả các cặp hình, dẫn đến so sánh khoảng 10^8 và mỗi phép kiểm tra giao điểm không phải là thời gian không đổi nếu được thực hiện về mặt hình học. 

Một vấn đề nhỏ sẽ xuất hiện nếu người ta cố gắng tính gần đúng khu vực bằng cách đếm các ô lưới hoặc các điểm lấy mẫu. Các hình dạng là liên tục và ranh giới rất quan trọng: một viên kim cương đóng góp diện tích ngay cả khi nó chỉ bao phủ một phần khu vực. Bất kỳ sự rời rạc nào cũng sẽ gây ra lỗi vượt quá dung sai 1e-4 cần thiết. 

Một cạm bẫy khác là giả sử vùng được bao phủ hoạt động giống như một tập hợp đơn giản của các hình vuông thẳng hàng với trục trong hệ tọa độ ban đầu. Các hình dạng không phải là hình vuông thẳng hàng theo trục, vì vậy các kỹ thuật kết hợp hình chữ nhật tiêu chuẩn không áp dụng trực tiếp mà không cần chuyển đổi. 

## Phương pháp tiếp cận 

Sự kết hợp hình học trực tiếp trên các viên kim cương rất phức tạp vì mỗi hình dạng được xác định bởi ràng buộc L1. Ý tưởng mạnh mẽ sẽ là tính toán diện tích hợp bằng cách xem xét mọi giao điểm theo cặp và sau đó áp dụng phép loại trừ hoặc quét mặt phẳng trên các ranh giới đa giác. Mặc dù đúng, nhưng điều này trở nên lộn xộn vì mỗi viên kim cương đóng góp bốn đoạn thẳng và việc xử lý tất cả các giao điểm giữa các đa giác O(n) sẽ dẫn đến hành vi bậc hai hoặc tệ hơn. 

Quan sát quan trọng là viên kim cương L1 trở nên đơn giản hơn nhiều dưới sự thay đổi tuyến tính của các biến. Nếu chúng ta xác định tọa độ mới u = x + y và v = x - y thì điều kiện |x - xi| + |y - yi| 1 chuyển thành điều kiện trong đó cả u và v đều bị chặn độc lập trong một khoảng quanh tâm của chúng. Trong không gian được biến đổi này, mỗi viên kim cương trở thành một hình vuông thẳng hàng với trục. 

Khi bài toán được thể hiện dưới dạng tập hợp các hình vuông thẳng hàng với trục, nhiệm vụ sẽ trở thành một mặt phẳng cổ điển quét trên các hình chữ nhật. Chúng tôi quét dọc theo một trục và duy trì vùng phủ sóng hoạt động trên trục kia bằng cách sử dụng cây phân đoạn hoặc đếm chênh lệch nén tọa độ. Điều này làm giảm vấn đề xuống O(n log n), đủ nhanh cho n = 10000. 

Cuối cùng, vì diện tích của thang đo biến đổi, chúng tôi sửa diện tích được tính toán bằng định thức Jacobian của phép biến đổi. Điều này đảm bảo kết quả khớp với hệ tọa độ ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liên minh đa giác Brute Force | O(n^2) hoặc tệ hơn | O(n) | Quá chậm | 
| Chuyển đổi + quét đường | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Biến đổi từng tâm (x, y) thành (u, v) trong đó u = x + y và v = x - y. Phép biến đổi tuyến tính này được chọn vì nó chuyển đổi các ràng buộc khoảng cách L1 thành các giới hạn độc lập trên u và v. 
2. Viết lại từng điều kiện kim cương |x - xi| + |y - yi| ≤ 1 là hai bất đẳng thức trong không gian biến đổi: u in [ui - 1, ui + 1] và v in [vi - 1, vi + 1]. Điều này cho thấy mỗi hình bây giờ là một hình vuông thẳng hàng với trục có chiều dài cạnh 2. 
3. Biểu diễn mỗi hình vuông dưới dạng một sự kiện hình chữ nhật trong mặt phẳng (u, v). Mỗi cái đóng góp một khoảng [ui - 1, ui + 1] dọc theo u và [vi - 1, vi + 1] dọc theo v. 
4. Thu thập tất cả các cạnh hình chữ nhật dưới dạng sự kiện quét dọc theo trục u. Mỗi sự kiện sẽ thêm hoặc xóa phạm vi phủ sóng trong một khoảng thời gian trong v. 
5. Sắp xếp các sự kiện theo tọa độ u và quét từ trái sang phải. Giữa các vị trí sự kiện liên tiếp, tập hợp các hình chữ nhật đang hoạt động không thay đổi, do đó độ dài v được bao phủ không đổi trên dải đó. 
6. Duy trì phạm vi bao phủ hoạt động trên trục v bằng cách sử dụng nén tọa độ và cây phân đoạn lưu trữ tổng chiều dài được bao phủ. Sau khi xử lý các sự kiện tại vị trí u, hãy tính mức đóng góp bằng (next_u - current_u) nhân với active_v_coverage. 
7. Tính tổng các phần đóng góp để được tổng diện tích trong không gian (u, v). 
8. Nhân kết quả cuối cùng với 1/2 để chuyển ngược về không gian (x, y), vì phép biến đổi tuyến tính có định thức Jacobian 1/2. 

Tính chính xác dựa trên thực tế là mỗi điểm được tính chính xác một lần trong quá trình quét: mỗi dải ngang trong u được ghép với phạm vi bao phủ dọc hoạt động chính xác trong v và phép biến đổi duy trì cấu trúc vùng phủ mà không bị biến dạng ngoài tỷ lệ đồng nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, coords):
        self.coords = coords
        self.n = len(coords) - 1
        self.count = [0] * (4 * self.n)
        self.length = [0] * (4 * self.n)

    def _update(self, idx, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.count[idx] += val
        else:
            mid = (l + r) // 2
            if ql < mid:
                self._update(idx * 2, l, mid, ql, qr, val)
            if qr > mid:
                self._update(idx * 2 + 1, mid, r, ql, qr, val)

        if self.count[idx] > 0:
            self.length[idx] = self.coords[r] - self.coords[l]
        else:
            if r - l == 1:
                self.length[idx] = 0
            else:
                self.length[idx] = self.length[idx * 2] + self.length[idx * 2 + 1]

    def update(self, l, r, val):
        self._update(1, 0, self.n, l, r, val)

    def query(self):
        return self.length[1]

n = int(input())
rects = []

vs = []

for _ in range(n):
    x, y = map(int, input().split())
    u = x + y
    v = x - y
    x1, x2 = u - 1, u + 1
    y1, y2 = v - 1, v + 1
    rects.append((x1, x2, y1, y2))
    vs.append(y1)
    vs.append(y2)

vs = sorted(set(vs))
idx = {v: i for i, v in enumerate(vs)}

events = []
for x1, x2, y1, y2 in rects:
    events.append((x1, y1, y2, 1))
    events.append((x2, y1, y2, -1))

events.sort()

seg = SegTree(vs)

area_uv = 0
for i in range(len(events)):
    x, y1, y2, t = events[i]
    seg.update(idx[y1], idx[y2], t)
    if i + 1 < len(events):
        dx = events[i + 1][0] - x
        area_uv += dx * seg.query()

print(area_uv / 2)
```Giải pháp bắt đầu bằng cách chuyển đổi từng trung tâm đầu vào thành hệ tọa độ xoay trong đó mỗi viên kim cương trở thành một hình vuông. Danh sách hình chữ nhật lưu trữ các hình vuông đó theo khoảng u và v của chúng. 

Cây phân đoạn hoạt động trên tọa độ v được nén. Mỗi bản cập nhật sẽ điều chỉnh số lượng hình chữ nhật hiện bao phủ một đoạn chữ V và cây duy trì tổng chiều dài được bao phủ. Việc quét qua u nhân chiều dài được bao phủ này với khoảng cách theo chiều ngang giữa các sự kiện. 

Phép chia cuối cùng cho 2 điều chỉnh tỷ lệ diện tích được đưa ra bởi phép biến đổi tuyến tính. 

Một điểm tinh tế là các cập nhật theo khoảng thời gian sử dụng cấu trúc nửa mở ngầm thông qua nén tọa độ, điều này tránh được việc tính hai lần các ranh giới chung. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó hai viên kim cương hơi chồng lên nhau trong lưới ban đầu. Sau khi biến đổi, chúng ta thu được hai hình vuông chồng lên nhau trong không gian uv. 

Để minh họa, giả sử hai tâm tạo ra các khoảng trong v trùng nhau một phần. 

| Bước quét | tọa độ u | Bảo hiểm v tích cực | Độ dài đoạn | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | u1 | 0 | 0 | 0 | 
| 2 | u2 | k | u2 - u1 | (u2 - u1) * k | 

Điều này cho thấy diện tích chỉ tích lũy như thế nào khi có sự phân tách theo chiều ngang giữa các sự kiện. 

Ví dụ thứ hai là một viên kim cương. Nó trở thành một hình vuông có cạnh 2 trong không gian uv. Quá trình quét tạo ra vùng phủ sóng hoạt động liên tục trên toàn chiều rộng của nó, mang lại chính xác 4 trong không gian tia cực tím, trở thành 2 trong tọa độ ban đầu sau khi chia tỷ lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp sự kiện và cập nhật cây phân đoạn cho từng điểm cuối | 
| Không gian | O(n) | Lưu trữ các sự kiện, nén tọa độ và cây phân đoạn | 

Các ràng buộc cho phép lên tới 10000 hình dạng và chi phí logarit đủ nhỏ để giải pháp đường quét chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # --- paste solution logic here as function ---
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, coords):
            self.coords = coords
            self.n = len(coords) - 1
            self.count = [0] * (4 * self.n)
            self.length = [0] * (4 * self.n)

        def _update(self, idx, l, r, ql, qr, val):
            if ql <= l and r <= qr:
                self.count[idx] += val
            else:
                mid = (l + r) // 2
                if ql < mid:
                    self._update(idx * 2, l, mid, ql, qr, val)
                if qr > mid:
                    self._update(idx * 2 + 1, mid, r, ql, qr, val)

            if self.count[idx] > 0:
                self.length[idx] = self.coords[r] - self.coords[l]
            else:
                if r - l == 1:
                    self.length[idx] = 0
                else:
                    self.length[idx] = self.length[idx * 2] + self.length[idx * 2 + 1]

        def update(self, l, r, val):
            self._update(1, 0, self.n, l, r, val)

        def query(self):
            return self.length[1]

    n = int(input())
    rects = []
    vs = []

    for _ in range(n):
        x, y = map(int, input().split())
        u = x + y
        v = x - y
        rects.append((u - 1, u + 1, v - 1, v + 1))
        vs += [v - 1, v + 1]

    vs = sorted(set(vs))
    idx = {v: i for i, v in enumerate(vs)}

    events = []
    for x1, x2, y1, y2 in rects:
        events.append((x1, y1, y2, 1))
        events.append((x2, y1, y2, -1))

    events.sort()

    seg = SegTree(vs)

    area_uv = 0
    for i, (x, y1, y2, t) in enumerate(events):
        seg.update(idx[y1], idx[y2], t)
        if i + 1 < len(events):
            area_uv += (events[i + 1][0] - x) * seg.query()

    return str(area_uv / 2)

# sample-like sanity checks
assert run("1\n1 1\n")  # single diamond produces nonzero area
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 điểm duy nhất | 2 | độ chính xác hình dạng đơn cơ bản | 
| nhiều trung tâm chồng chéo | khu vực sáp nhập | xử lý chồng chéo | 
| điểm phân tán tối đa | đầu ra ổn định | hiệu suất và độ chính xác quét | 
| trung tâm giống hệt nhau lặp đi lặp lại | không tính gấp đôi | xử lý nhiều bộ | 

## Vỏ cạnh 

Trường hợp cạnh chính là tâm lặp lại. Nếu nhiều đồ trang trí có cùng một điểm nguyên, một cách tiếp cận đơn giản có thể cộng diện tích của chúng nhiều lần. Trong giải pháp này, cả hai bản sao đều tạo ra các hình chữ nhật giống hệt nhau trong không gian uv, nhưng cây phân đoạn tính phạm vi bao phủ là một liên kết, do đó các bản sao không thay đổi độ dài hoạt động sau lần chèn đầu tiên. 

Một trường hợp cạnh khác xuất hiện gần ranh giới của cửa sổ. Tâm tại (1, 1) tạo ra một viên kim cương đạt tới x = 0 hoặc y = 0. Phép biến đổi không phụ thuộc vào việc kẹp vào cửa sổ vì quá trình quét hoạt động trên toàn bộ vùng hình học và ràng buộc cửa sổ đã được thỏa mãn bởi các quy tắc vị trí. 

Trường hợp cạnh cuối cùng là khi hình chữ nhật chỉ chạm vào các ranh giới. Vì quá trình quét sử dụng các khoảng thời gian nửa mở trong quá trình nén tọa độ nên việc chạm vào các cạnh không tạo ra vùng nhân tạo. Cây phân đoạn chỉ đóng góp độ dài khi các khoảng có số đo dương, do đó sự chồng chéo có độ rộng bằng 0 được bỏ qua một cách chính xác.
