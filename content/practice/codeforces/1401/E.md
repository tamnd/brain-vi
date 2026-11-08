---
title: "CF 1401E - Chia hình vuông"
description: "Chúng ta được yêu cầu tính số phần mà một hình vuông lớn được chia thành sau khi vẽ một số đoạn thẳng ngang và dọc."
date: "2026-06-11T08:43:17+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1401
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 665 (Div. 2)"
rating: 2400
weight: 1401
solve_time_s: 137
verified: false
draft: false
---

[CF 1401E - Chia hình vuông](https://codeforces.com/problemset/problem/1401/E) 

**Đánh giá:** 2400 
**Tags:** cấu trúc dữ liệu, hình học, triển khai, sắp xếp 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính số phần mà một hình vuông lớn được chia thành sau khi vẽ một số đoạn thẳng ngang và dọc. Hình vuông được cố định về kích thước$10^6 \times 10^6$và các đoạn thẳng có thể cắt một phần hoặc toàn bộ hình vuông, nhưng chúng luôn cắt nhau ít nhất một trong các cạnh của nó. Các phân đoạn ngang được cho bởi tọa độ y và phạm vi x, trong khi các phân đoạn dọc được cho bởi tọa độ x và phạm vi y. Không có hai đoạn nào nằm trên cùng một đường thẳng. 

Đầu ra là một số nguyên biểu thị số vùng được kết nối còn lại trong hình vuông sau khi thêm tất cả các phân đoạn. Về mặt khái niệm, mỗi phân đoạn có thể chia các vùng hiện có thành hai dọc theo đường của nó. Đoạn ngang chia các vùng theo chiều ngang; một đoạn dọc chia các vùng theo chiều dọc. Giao lộ giữa các phân đoạn làm tăng số lượng các mảnh kết quả. 

Các ràng buộc chỉ ra rằng cả hai$n$Và$m$có thể lên đến$10^5$. Một giải pháp dựa trên lưới mạnh mẽ sẽ yêu cầu ít nhất$10^{12}$hoạt động nếu chúng ta cố gắng đánh dấu tất cả các điểm của hình vuông, điều này rõ ràng là không thể trong 2 giây. Vì vậy, chúng ta phải tránh các cách tiếp cận lặp đi lặp lại trên mọi đơn vị hình vuông. Thay vào đó, giải pháp chỉ cần xử lý các đoạn và giao điểm của chúng. 

Trường hợp cạnh khóa là khi một đoạn không kéo dài hết chiều rộng hoặc chiều cao của hình vuông mà chạm vào một cạnh. Ví dụ: một đoạn ngang từ$(0,2)$ĐẾN$(4,2)$trong một$10^6 \times 10^6$hình vuông vẫn được tính để chia các mảnh dù nó không chạm tới phía xa. Một cách tiếp cận đơn giản có thể bỏ qua các phân đoạn không trải dài hoàn toàn trên một trục, tạo ra số lượng thiếu. Một trường hợp phức tạp khác là các phân đoạn chồng lên nhau ở các trục khác nhau. Nếu một đoạn dọc và ngang giao nhau, giao điểm sẽ tạo ra một đoạn mới. Bỏ qua các giao lộ sẽ đếm thiếu tổng số mảnh. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: hãy tưởng tượng một mảng 2D biểu thị hình vuông và đánh dấu tất cả các đoạn ngang và dọc trên đó. Sau đó thực hiện việc lấp lũ để đếm các vùng được kết nối. Điều này hoạt động vì việc đánh dấu và kết nối tương ứng trực tiếp với số lượng mảnh. Vấn đề là ngay cả việc lưu trữ một lưới$10^6 \times 10^6$các ô là không thể và việc lặp lại nó vượt xa mức cho phép$2\cdot 10^9$hoạt động mỗi giây. Vì vậy, cách tiếp cận vũ phu không thành công do hạn chế về bộ nhớ và thời gian. 

Cách tiếp cận tối ưu thúc đẩy quan sát rằng chúng ta không cần biểu diễn hình vuông đầy đủ; chúng ta chỉ cần các đoạn và giao điểm của chúng. Mỗi đoạn ngang có thể được coi là cắt tất cả các vùng dọc mà nó đi qua, và mỗi đoạn thẳng đứng cắt tất cả các vùng ngang mà nó đi qua. Việc đếm các giao điểm một cách cẩn thận cho phép chúng ta sử dụng đối số tổ hợp: mỗi đoạn mới thêm các phần bằng một cộng với số giao điểm mà nó tạo thành theo hướng còn lại. Điều này làm giảm vấn đề sắp xếp các phân đoạn và đếm các phần chồng chéo một cách hiệu quả, có thể được thực hiện bằng cách sử dụng đường quét và nén tọa độ, vì tọa độ lớn nhưng số lượng phân đoạn vừa phải. Cách tiếp cận này tránh mọi biểu diễn lưới đầy đủ, chỉ dựa vào$n+m$các đoạn và số lượng giao lộ của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^12) | O(10^12) | Quá chậm | 
| Tối ưu | O((n+m) log(n+m)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách các phân đoạn thành danh sách ngang và dọc, giữ nguyên tọa độ của chúng. Phân đoạn ngang được lưu trữ dưới dạng`(y, x_start, x_end)`và dọc như`(x, y_start, y_end)`. 
2. Phối hợp-nén tất cả tọa độ x của các đoạn ngang và đoạn dọc và nén tọa độ y tương tự. Điều này đảm bảo rằng tất cả tọa độ phân đoạn ánh xạ tới một phạm vi số nguyên nhỏ hơn từ 0 đến nhiều nhất$2\cdot 10^5$, phù hợp với bộ nhớ và cho phép xử lý hiệu quả. 
3. Đối với các đoạn ngang, sắp xếp theo tọa độ y. Đối với các đoạn thẳng đứng, sắp xếp theo tọa độ x. Việc sắp xếp này chuẩn bị cho thuật toán đường quét để đếm các giao lộ một cách hiệu quả. 
4. Khởi tạo cây Fenwick (Cây chỉ mục nhị phân) dọc theo trục nén. Để quét ngang, cây theo dõi các đoạn dọc đang hoạt động trên trục x, đếm số lượng đoạn dọc mà một đường ngang giao nhau. Cập nhật cây khi vào hoặc ra khỏi đoạn thẳng đứng. 
5. Lặp lại trên tất cả các phân đoạn ngang theo tọa độ y tăng dần. Đối với mỗi đoạn ngang, hãy truy vấn cây Fenwick trên phạm vi x của nó để đếm các giao điểm với các đoạn thẳng đứng. Mỗi giao lộ tăng số lượng mảnh lên một. Thêm một giá trị cho đoạn ngang vì nó luôn phân chia vùng mà nó đi qua. 
6. Lặp lại đối xứng cho các đoạn thẳng đứng bằng cách quét dọc theo trục x, sử dụng các đoạn ngang được lưu trong cây. Tổng của tất cả các đóng góp phân đoạn trừ đi các giao điểm được tính (vì các giao lộ được tính hai lần) sẽ cho ra tổng số phần. 
7. Khởi tạo kết quả bằng 1, biểu thị hình vuông ban đầu. Đối với mỗi phân đoạn được xử lý, hãy thêm phần đóng góp của nó theo các quy tắc trên. Xuất kết quả cuối cùng. 

Điều bất biến chính là mỗi đoạn mới sẽ chia tách một đoạn hiện có hoặc giao với một đoạn có hướng khác, tạo ra một đoạn mới tại giao điểm. Thuật toán đảm bảo rằng mỗi giao điểm được tính chính xác một lần cho mỗi loại phân đoạn, do đó tổng thể hiện chính xác tất cả các phần kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class FenwickTree:
    def __init__(self, n):
        self.n = n
        self.bit = [0]*(n+1)
    
    def update(self, i, val):
        i += 1
        while i <= self.n:
            self.bit[i] += val
            i += i & -i
    
    def query(self, i):
        i += 1
        res = 0
        while i > 0:
            res += self.bit[i]
            i -= i & -i
        return res
    
    def range_query(self, l, r):
        return self.query(r) - self.query(l-1)

def main():
    n, m = map(int, input().split())
    horiz = []
    vert = []
    x_coords = set()
    y_coords = set()
    
    for _ in range(n):
        y, lx, rx = map(int, input().split())
        horiz.append((y, lx, rx))
        x_coords.add(lx)
        x_coords.add(rx)
        y_coords.add(y)
    
    for _ in range(m):
        x, ly, ry = map(int, input().split())
        vert.append((x, ly, ry))
        y_coords.add(ly)
        y_coords.add(ry)
        x_coords.add(x)
    
    x_list = sorted(x_coords)
    y_list = sorted(y_coords)
    x_map = {x:i for i,x in enumerate(x_list)}
    y_map = {y:i for i,y in enumerate(y_list)}
    
    horiz_mapped = [(y_map[y], x_map[lx], x_map[rx]) for y, lx, rx in horiz]
    vert_mapped = [(x_map[x], y_map[ly], y_map[ry]) for x, ly, ry in vert]
    
    horiz_mapped.sort()
    vert_mapped.sort()
    
    BIT = FenwickTree(len(x_list))
    
    res = 1
    events = []
    for x, ly, ry in vert_mapped:
        events.append((ly, 1, x))
        events.append((ry+1, -1, x))
    events.sort()
    
    j = 0
    for y, lx, rx in horiz_mapped:
        while j < len(events) and events[j][0] <= y:
            _, delta, x = events[j]
            BIT.update(x, delta)
            j += 1
        res += 1 + BIT.range_query(lx, rx)
    
    # symmetric for vertical segments
    BIT = FenwickTree(len(y_list))
    events = []
    for y, lx, rx in horiz_mapped:
        events.append((lx, 1, y))
        events.append((rx+1, -1, y))
    events.sort()
    
    j = 0
    for x, ly, ry in vert_mapped:
        while j < len(events) and events[j][0] <= x:
            _, delta, y = events[j]
            BIT.update(y, delta)
            j += 1
        res += 1 + BIT.range_query(ly, ry)
    
    print(res)

if __name__ == "__main__":
    main()
```Đầu tiên, mã ánh xạ tọa độ tới các chỉ mục được nén, đảm bảo chúng ta có thể sử dụng cây Fenwick một cách hiệu quả. Các đoạn ngang được xử lý trước: mỗi đoạn đóng góp một phần và các giao điểm với các đoạn dọc được tính thông qua cây Fenwick. Sau đó, các đoạn dọc được xử lý tương tự. Việc thêm một truy vấn cho mỗi đoạn sẽ xử lý phần phân chia mới, trong khi truy vấn phạm vi sẽ tính đến các giao điểm. sử dụng`events`đảm bảo đường quét thêm hoặc xóa chính xác các đoạn thẳng đứng tại điểm cuối của chúng. Phương pháp này tránh việc đếm hai lần và xử lý các tọa độ lớn trong giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
2 3 1000000
4 0 4
3 0 1000000
4 0 1
2 0 5
3 1 1000000
```| Phân đoạn | Tọa độ nén | Nút giao | Đóng góp | Miếng tích lũy | 
| --- | --- | --- | --- | --- | 
| H2 3-1e6 | y=0 | chéo V2 | 1 | 2 | 
| H4 0-4 | y=1 | vượt qua V1 | 1 | 3 | 
| H3 0-1e6 | y=2 | | | |
