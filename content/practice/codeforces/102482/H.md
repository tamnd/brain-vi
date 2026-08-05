---
title: "CF 102482H - Lỗi một lần"
description: "Chúng ta có một hình chữ nhật tượng trưng cho một cánh cửa. Dây là những đoạn thẳng có điểm cuối nằm trên ranh giới và mỗi dây nối hai mặt khác nhau. Chúng ta cần đặt càng ít đoạn thẳng mới càng tốt để mỗi dây hiện có được cắt ngang bởi ít nhất một trong các đoạn của chúng ta."
date: "2026-08-05T19:01:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102482
codeforces_index: "H"
codeforces_contest_name: "2018 ACM-ICPC World Finals"
rating: 0
weight: 102482
solve_time_s: 121
verified: true
draft: false
---

[CF 102482H - Lỗi một lần](https://codeforces.com/problemset/problem/102482/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hình chữ nhật tượng trưng cho một cánh cửa. Dây là những đoạn thẳng có điểm cuối nằm trên ranh giới và mỗi dây nối hai mặt khác nhau. Chúng ta cần đặt càng ít đoạn thẳng mới càng tốt để mỗi dây hiện có được cắt ngang bởi ít nhất một trong các đoạn của chúng ta. 

Câu trả lời nhỏ đến mức đáng ngạc nhiên. Hai vết cắt luôn là đủ: hai đường chéo của hình chữ nhật giao nhau với mọi dây có thể có từ ranh giới này sang ranh giới khác. Thử thách thực sự là quyết định liệu một lần cắt đã đủ hay chưa. Quan sát lời giải chính thức là hình học có thể được chuyển đổi thành bài toán khoảng tròn và sự tồn tại của một vết cắt có thể được kiểm tra bằng cách quét tuyến tính sau khi sắp xếp các điểm cuối. 

Kích thước đầu vào là khó khăn chính. Có thể có tới một triệu dây, vì vậy bất cứ điều gì liên quan đến các cặp dây đều không thể thực hiện được. Thuật toán bậc hai sẽ thực hiện khoảng 10^12 so sánh trong trường hợp xấu nhất, vượt xa thời gian có sẵn. Chúng ta cần một phương pháp chỉ xử lý mỗi dây một số lần không đổi. 

Tọa độ lớn, lên tới 10^8, nhưng chúng chỉ được sử dụng để xác định thứ tự tuần hoàn của các điểm trên đường biên. Chúng ta không cần tính toán giao điểm hình học giữa các đoạn tùy ý. Cấu trúc của hình chữ nhật là thứ giúp giải quyết vấn đề. 

Một sai lầm phổ biến là tìm kiếm vết cắt bằng cách thử độ dốc hoặc thử nhiều đoạn có thể. Câu trả lời không được xác định bởi hình học hệ mét. Thông tin quan trọng chỉ là thứ tự các điểm cuối xung quanh hình chữ nhật. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi cách cắt có thể và kiểm tra xem nó giao nhau với dây nào. Vì một vết cắt hợp lệ có thể được di chuyển liên tục mà không thay đổi dây nào nó đi qua cho đến khi đến điểm cuối dây, nên người ta có thể hạn chế các ứng cử viên ở khoảng cách giữa các điểm cuối liên tiếp. Có O(n) những khoảng trống như vậy và việc kiểm tra một ứng cử viên dựa trên tất cả các dây có chi phí O(n), đưa ra các hoạt động O(n²). Với n = 10^6 thì điều này là không thể. 

Phép biến đổi hữu ích là đi vòng quanh ranh giới hình chữ nhật và mở nó thành một đường thẳng. Mỗi dây trở thành một khoảng giữa hai vị trí điểm cuối của nó theo thứ tự vòng tròn này. Một vết cắt cũng tương ứng với việc chọn một khoảng trên vòng tròn này. Một dây được cắt chéo chính xác khi khoảng đã chọn chứa đúng một trong hai điểm cuối của nó. 

Vấn đề là tìm xem liệu có tồn tại một khoảng tròn chứa một điểm cuối từ mỗi dây hay không. Điều này có thể được giải quyết bằng cách quét hai con trỏ trong khi vẫn duy trì số lượng dây hiện có 0, một hoặc hai điểm cuối trong khoảng hoạt động. 

Nếu một khoảng như vậy tồn tại thì hai điểm biên của nó sẽ tạo ra đường cắt đơn cần thiết. Nếu nó không tồn tại thì hai đường chéo hình chữ nhật là tối ưu vì hai đường cắt luôn là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi mọi điểm cuối của dây thành một tọa độ duy nhất trên chu vi của hình chữ nhật. Chúng ta bắt đầu ở góc dưới bên trái, di chuyển dọc theo cạnh dưới, sau đó là cạnh phải, cạnh trên và cuối cùng là cạnh trái. Điểm bắt đầu chính xác không quan trọng, nhưng cần có một trật tự tuần hoàn nhất quán. 
2. Sắp xếp tất cả các vị trí điểm cuối. Sao chép danh sách đã sắp xếp với độ dài chu vi được thêm vào để các khoảng thời gian tròn có thể được xử lý như các khoảng thời gian bình thường. 
3. Duy trì một cửa sổ trượt trên mảng nhân đôi này. Đầu bên trái và bên phải của cửa sổ biểu thị hai điểm cuối của một đường cắt có thể thực hiện được. Đối với mỗi dây, hãy duy trì số lượng hai điểm cuối của nó hiện đang ở bên trong cửa sổ. 
4. Di chuyển con trỏ bên phải về phía trước trong khi cửa sổ vẫn có thể được mở rộng mà không làm cho bất kỳ dây nào có cả hai điểm cuối bên trong. Dây có cả hai đầu bên trong không thể cắt ngang bằng đường cắt tương ứng. 
5. Bất cứ khi nào mỗi dây có chính xác một điểm cuối bên trong cửa sổ hiện tại, hai vị trí biên của cửa sổ sẽ mô tả một lần cắt hợp lệ. 
6. Nếu quá trình quét kết thúc mà không tìm thấy cửa sổ như vậy, hãy xuất ra hai đường chéo hơi ngắn của hình chữ nhật. 

Điều bất biến là cửa sổ trượt luôn biểu thị khoảng cắt ứng cử viên mà điểm cuối bên phải đã được mở rộng đến mức có thể cho điểm cuối bên trái hiện tại. Mọi khoảng thời gian hợp lệ có thể xuất hiện trong quá trình quét này vì con trỏ bên trái truy cập vào mọi khoảng cách có thể có giữa các điểm cuối và con trỏ bên phải chỉ di chuyển về phía trước xung quanh vòng tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, w, h = map(int, input().split())

    events = []
    wires = []

    per = 2 * (w + h)

    def pos(x, y):
        if y == 0:
            return x
        if x == w:
            return w + y
        if y == h:
            return w + h + (w - x)
        return w + h + w + (h - y)

    for i in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        a = pos(x1, y1)
        b = pos(x2, y2)
        wires.append((a, b))
        events.append((a, i))
        events.append((b, i))

    events.sort()

    m = len(events)
    coords = [x[0] for x in events]

    index_a = [0] * n
    index_b = [0] * n
    for i, (_, w_id) in enumerate(events):
        if index_a[w_id] == 0 and index_b[w_id] == 0:
            index_a[w_id] = i
        else:
            index_b[w_id] = i

    cnt = [0] * n
    one = 0
    two = 0

    def add(idx):
        nonlocal one, two
        w_id = events[idx % m][1]
        if cnt[w_id] == 0:
            cnt[w_id] = 1
            one += 1
        elif cnt[w_id] == 1:
            cnt[w_id] = 2
            one -= 1
            two += 1

    def remove(idx):
        nonlocal one, two
        w_id = events[idx % m][1]
        if cnt[w_id] == 1:
            cnt[w_id] = 0
            one -= 1
        else:
            cnt[w_id] = 1
            two -= 1
            one += 1

    def from_pos(p):
        p %= per
        if p < w:
            return 0.0, float(p)
        p -= w
        if p < h:
            return float(w), float(p)
        p -= h
        if p < w:
            return float(w - p), float(h)
        p -= w
        return 0.0, float(h - p)

    r = 0
    while r < m:
        add(r)
        r += 1

    for l in range(m):
        while r < l + m and two == 0 and one < n:
            add(r)
            r += 1

        if one == n and two == 0:
            a = coords[l]
            b = coords[r - 1] if r - 1 < m else coords[(r - 1) % m] + per
            if b - a < per:
                mid1 = a + 0.5
                mid2 = b - 0.5
                if mid2 > per:
                    mid2 -= per
                x1, y1 = from_pos(mid1)
                x2, y2 = from_pos(mid2)
                print(1)
                print(x1, y1, x2, y2)
                return

        remove(l)

    eps = 0.001
    print(2)
    print(eps, eps, w - eps, h - eps)
    print(w - eps, eps, eps, h - eps)

if __name__ == "__main__":
    solve()
```
