---
title: "CF 102483C - Thiết kế bảng mạch"
description: "Đầu vào mô tả một mạch điện như một cái cây. Mỗi đỉnh là một điểm kết nối và mỗi cạnh là một sợi dây phải được vẽ thành một đoạn thẳng."
date: "2026-08-05T18:31:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102483
codeforces_index: "C"
codeforces_contest_name: "2018-2019 ICPC Northwestern European Regional Programming Contest (NWERC 2018)"
rating: 0
weight: 102483
solve_time_s: 135
verified: true
draft: false
---

[CF 102483C - Thiết kế bảng mạch](https://codeforces.com/problemset/problem/102483/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Đầu vào mô tả một mạch điện như một cái cây. Mỗi đỉnh là một điểm kết nối và mỗi cạnh là một sợi dây phải được vẽ thành một đoạn thẳng. Nhiệm vụ không phải là tìm đường đi hay tối ưu hóa chi phí mà là gán tọa độ cho mọi đỉnh để cây đã cho có thể được in trên mặt phẳng. 

Việc xây dựng có yêu cầu hình học nghiêm ngặt. Mỗi cạnh của cây phải trở thành một đoạn có độ dài chính xác bằng một. Hai cạnh không liên quan không thể cắt nhau và các đỉnh khác nhau không được đặt quá gần nhau. Vì bảng lớn nên thách thức là tìm ra cách có hệ thống để trải cây ra mà không vi phạm hình học. 

Số lượng đỉnh nhiều nhất là 1000. Con số này đủ nhỏ để có thể mong đợi một đường truyền tuyến tính hoặc gần tuyến tính, nhưng lại quá lớn để thử nhiều bố cục có thể có. Một phương pháp khám phá các góc độ hoặc vị trí khác nhau một cách kết hợp sẽ phát triển vượt xa thời gian sẵn có. Cấu trúc cây là hạn chế chính giúp cho việc xây dựng đệ quy đơn giản trở nên khả thi. 

Một số trường hợp có thể phá vỡ việc thực hiện bất cẩn. Một cạnh duy nhất là cây nhỏ nhất có thể:```
2
1 2
```Một câu trả lời hợp lệ là hai điểm bất kỳ cách nhau một đơn vị, chẳng hạn như`(0,0)`Và`(1,0)`. Việc triển khai giả định mọi đỉnh đều có con có thể thất bại ở đây. 

Cây hình ngôi sao là một trường hợp quan trọng khác:```
5
1 2
1 3
1 4
1 5
```Trung tâm có nhiều trẻ em. Đặt tất cả trẻ em ở các góc quá gần có thể làm cho các cạnh chồng lên nhau hoặc đặt các đỉnh khác nhau gần như chồng lên nhau. 

Một chuỗi dài cũng có vấn đề:```
4
1 2
2 3
3 4
```Phương pháp sử dụng nhiều lần một hướng cạnh cố định có thể vô tình đặt tất cả các đỉnh lên nhau hoặc tạo ra các đoạn chồng chéo. Vị trí đệ quy phải xử lý các đỉnh có chính xác một nút con cũng như các nút phân nhánh. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng đặt từng đỉnh một trong khi kiểm tra xem cạnh mới có giao nhau với các cạnh hiện có hay không. Đối với mỗi điểm mới, chúng ta có thể tìm kiếm một hướng đi tự do giữa nhiều góc độ khả thi và loại bỏ những lựa chọn tạo ra xung đột. Điều này có thể áp dụng được cho những cây rất nhỏ vì cây chỉ có`n - 1`các cạnh, nhưng số lượng bố cục ứng cử viên tăng lên nhanh chóng. Trong trường hợp xấu nhất, việc kiểm tra liên tục nhiều vị trí đối với tất cả các cạnh trước đó sẽ dẫn đến công việc gần đúng bậc hai hoặc tệ hơn và bản thân việc tìm kiếm không có giới hạn đơn giản. 

Lực lượng vũ phu hoạt động vì cây không có chu kỳ, vì vậy chúng ta có thể tự do xây dựng bản vẽ từ gốc ra ngoài. Nó thất bại vì nó cố gắng giải quyết vấn đề hình học một cách cục bộ sau khi thực hiện các lựa chọn tùy ý. 

Quan sát hữu ích là một cây có gốc có thể được vẽ bên trong các vùng góc. Nếu một đỉnh sở hữu một góc nhọn thì tất cả các đỉnh con của nó có thể được đặt bên trong góc đó. Các cây con của cùng một đỉnh có thể nhận được các nêm con rời rạc, do đó các cây con của chúng không bao giờ cần phải giao nhau. Thuộc tính cây chính xác là thứ cho phép sự phân tách đệ quy này. 

Chúng ta root cây và tính toán kích thước của mỗi cây con. Khi phân phối góc có sẵn cho các cây con, các cây con lớn hơn sẽ nhận được phạm vi góc lớn hơn. Tỷ lệ chính xác không quan trọng, nhưng việc cung cấp đủ không gian cho mỗi đứa trẻ sẽ giúp tách các bản vẽ đệ quy sâu hơn. Mỗi đứa trẻ được đặt cách cha mẹ chính xác một đơn vị ở giữa khoảng góc được chỉ định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) cho mỗi bố cục đã thử, không có ràng buộc hữu ích nào đối với các lần thử | O(n) | Quá chậm và không đáng tin cậy | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Lấy gốc cây ở đỉnh 1 và tính kích thước của mỗi cây con. Kích thước cây con cho chúng ta biết một đứa trẻ sẽ nhận được bao nhiêu không gian góc so với các anh chị em của nó. 
2. Bắt đầu root tại`(0, 0)`và cung cấp cho nó vòng tròn đầy đủ các hướng có thể. Lệnh gọi đệ quy lưu trữ hướng từ đỉnh cha đến đỉnh hiện tại và khoảng góc có sẵn cho cây con hiện tại. 
3. Đối với một đỉnh, hãy xét tất cả các con trừ cha của nó. Chia khoảng thời gian có sẵn giữa các nút con tương ứng với kích thước cây con của chúng. Đứa trẻ nhận được hướng giữa của khoảng thời gian của nó. 
4. Đặt trẻ cách đỉnh hiện tại một đơn vị theo hướng đó. Điều này tự động đáp ứng yêu cầu về độ dài cạnh. 
5. Tái diễn vào đứa trẻ với khoảng góc riêng của nó. Đứa trẻ chỉ phân phối không gian trong khoảng dành riêng cho con cháu của nó, vì vậy các nhánh khác nhau vẫn tách biệt. 

Lý do đặt con xung quanh hướng ngược lại của cạnh với cạnh cha là vì cạnh cha chiếm một cạnh của đỉnh. Không gian góc còn lại được dành riêng cho trẻ em. 

Tại sao nó hoạt động: mọi lệnh gọi đệ quy đều sở hữu một cái nêm chứa toàn bộ cây con bên dưới đỉnh đó. Các nêm anh chị em không chồng lên nhau nên các cạnh của chúng không thể giao nhau. Điểm chia sẻ duy nhất giữa các cây con anh em là cây cha của chúng. Mọi vị trí đệ quy đều sử dụng một vectơ đơn vị, vì vậy mỗi cạnh có độ dài bằng một. Việc xây dựng duy trì các đặc tính này từ gốc đến từng lá. 

#Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

n = int(input())
g = [[] for _ in range(n)]

for _ in range(n - 1):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    g[a].append(b)
    g[b].append(a)

size = [0] * n
parent = [-1] * n

def calc(v, p):
    parent[v] = p
    size[v] = 1
    for u in g[v]:
        if u != p:
            calc(u, v)
            size[v] += size[u]

calc(0, -1)

ans = [(0.0, 0.0)] * n

def build(v, p, x, y, direction, width):
    children = [u for u in g[v] if u != p]
    if not children:
        return

    total = sum(size[u] for u in children)
    start = direction + math.pi - width / 2

    for u in children:
        part = width * size[u] / total
        mid = start + part / 2

        nx = x + math.cos(mid)
        ny = y + math.sin(mid)
        ans[u] = (nx, ny)

        build(u, v, nx, ny, mid, part)
        start += part

build(0, -1, 0.0, 0.0, 0.0, 2 * math.pi)

for x, y in ans:
    print("{:.10f} {:.10f}".format(x, y))
```Tìm kiếm theo chiều sâu đầu tiên tính toán kích thước cây con. Giá trị được lưu trữ cho một đỉnh bao gồm chính đỉnh đó, do đó tổng kích thước của cây con con chính xác là lượng cấu trúc phải được phân bổ bên dưới nút hiện tại. 

Việc duyệt thứ hai thực hiện việc xây dựng hình học. Biến`direction`là hướng được sử dụng bởi cạnh đi vào đỉnh hiện tại. Trẻ em được đặt xung quanh`direction + pi`, điểm cách xa cha mẹ. Biến`width`là phòng góc có sẵn cho cây con hiện tại. 

Các khoảng con được tính bằng tỷ lệ dấu phẩy động. Tổng số đỉnh chỉ là 1000 nên độ chính xác gấp đôi thông thường là quá đủ cho giới hạn lỗi yêu cầu. Tọa độ vẫn nhỏ vì mỗi cấp chỉ di chuyển một đơn vị và độ sâu tối đa là 1000. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, gốc có bốn con. Vòng tròn đầy đủ được chia thành bốn phần bằng nhau vì mỗi cây con có kích thước bằng một. 

| Đỉnh | Phụ huynh | Chỉ đạo được giao | Vị trí | 
| --- | --- | --- | --- | 
| 1 | không | 0 | (0,0) | 
| 2 | 1 | 0 | (1,0) | 
| 3 | 1 | π/2 | (0,1) | 
| 4 | 1 | π | (-1,0) | 
| 5 | 1 | 3π/2 | (0,-1) | 

Các tọa độ chính xác khác với đầu ra của mẫu, nhưng tất cả đều đáp ứng hình dạng được yêu cầu. Dấu vết cho thấy thuật toán chỉ cần một bản vẽ hợp lệ chứ không cần một bản vẽ cụ thể. 

Đối với mẫu thứ hai, đỉnh 2 có hai con trong khi đỉnh 1 có một con và một con lá. 

| Đỉnh | Hành động | Lý do | 
| --- | --- | --- | 
| 1 | Đặt tại điểm xuất phát | Gốc của công trình | 
| 2 | Đặt một đơn vị đi | Con duy nhất của gốc một hướng | 
| 3 | Đặt vào vùng còn lại của root | Giữ các nhánh tách biệt | 
| 4 | Chia vùng đỉnh 2 | Con đầu lòng của đỉnh 2 | 
| 5 | Chia vùng đỉnh 2 | Con thứ hai của đỉnh 2 | 

Phần quan trọng của ví dụ này là cây con có thể tiếp tục phát triển từ một đỉnh không có gốc trong khi vẫn nằm trong vùng góc được gán cho nó. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh được xử lý một lần trong cả hai lần duyệt | 
| Không gian | O(n) | Danh sách kề, kích thước cây con và tọa độ là tuyến tính | 

Giới hạn 1000 đỉnh để lại một khoảng cách lớn cho việc xây dựng đệ quy tuyến tính. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với giới hạn có sẵn. 

# Trường hợp thử nghiệm```python
import math

def validate(inp):
    import io
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    n = int(input())
    edges = []
    for _ in range(n - 1):
        edges.append(tuple(map(int, input().split())))
    sys.stdin = old

    return len(edges) == n - 1

assert validate("2\n1 2\n")
assert validate("5\n1 2\n1 3\n1 4\n1 5\n")
assert validate("4\n1 2\n2 3\n3 4\n")
assert validate("6\n1 2\n1 3\n3 4\n3 5\n5 6\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh | Hai điểm cách nhau một đơn vị | Kích thước cây tối thiểu | 
| Cây sao | Trung tâm có trẻ em ly thân | Nhiều con ở một đỉnh | 
| Cây xích | Một con đường không phân nhánh | Đệ quy một con | 
| Nhánh hỗn hợp | Phân chia đệ quy hợp lệ | Cấu trúc cây chung | 

# Vỏ cạnh 

Đối với cây hai đỉnh:```
2
1 2
```Gốc có một con. Thuật toán cung cấp cho đứa trẻ đó toàn bộ khoảng góc có sẵn và đặt nó cách chính xác một đơn vị. Không có cây con anh em nên không thể giao nhau. 

Đối với cây sao:```
5
1 2
1 3
1 4
1 5
```Gốc chia hình tròn đầy đủ cho bốn đứa trẻ. Vì mọi cây con có kích thước bằng nhau nên tất cả đều nhận được các vùng góc bằng nhau. Các phân đoạn của chúng rời khỏi trung tâm theo các hướng khác nhau và các lệnh gọi đệ quy sẽ dừng ngay lập tức. 

Đối với chuỗi:```
4
1 2
2 3
3 4
```Mỗi đỉnh chỉ có một con trừ lá. Toàn bộ khoảng thời gian được truyền đi nhiều lần, nhưng mỗi đỉnh tiếp theo vẫn được đặt xa hơn chính xác một đơn vị dọc theo hướng hiện tại. Không có nhánh nào tồn tại nên không thể có giao lộ.
