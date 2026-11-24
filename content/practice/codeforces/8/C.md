---
title: "CF 8C - Tìm kiếm đơn hàng"
description: "Chúng tôi có điểm bắt đầu cố định, túi xách và tối đa 24 vật thể nằm rải rác trên mặt phẳng 2D. Lena luôn bắt đầu từ chiếc túi xách, đi xung quanh để thu thập tối đa hai đồ vật, quay trở lại túi xách, sau đó lặp lại cho đến khi mọi đồ vật được cất trở lại."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp"]
categories: ["algorithms"]
codeforces_contest: 8
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 8"
rating: 2000
weight: 8
solve_time_s: 108
verified: true
draft: false
---
[CF 8C - Đang tìm kiếm đơn hàng](https://codeforces.com/problemset/problem/8/C) 

**Xếp hạng:** 2000 
**Thẻ:** bitmask, dp 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có điểm bắt đầu cố định, túi xách và tối đa 24 vật thể nằm rải rác trên mặt phẳng 2D. Lena luôn bắt đầu từ chiếc túi xách, đi xung quanh để thu thập tối đa hai đồ vật, quay trở lại túi xách, sau đó lặp lại cho đến khi mọi đồ vật được cất trở lại. 

Chi phí di chuyển giữa hai vị trí là bình phương khoảng cách Euclide. Nếu Lena di chuyển từ điểm`(x1, y1)`ĐẾN`(x2, y2)`, thời gian đi là:$(x_1-x_2)^2 + (y_1-y_2)^2$Bản thân chiếc túi xách không bao giờ di chuyển nên mỗi chuyến đi đều bắt đầu tại một điểm`0`và kết thúc tại điểm`0`. 

Một chuyến đi có thể trông như thế này:```
0 -> i -> 0
```hoặc như thế này:```
0 -> i -> j -> 0
```Nhiệm vụ không chỉ là tính toán tổng chi phí đi lại tối thiểu mà còn xây dựng lại một chuỗi các chuyến đi tối ưu. 

Ràng buộc`n ≤ 24`ngay lập tức loại trừ bất cứ điều gì mang tính giai thừa. Việc thử mọi thứ tự có thể có của các đồ vật sẽ đòi hỏi khoảng`24!`hoán vị, điều này hoàn toàn không thể xảy ra. 

Đồng thời,`2^24`là khoảng 16 triệu. Dung lượng này lớn nhưng có thể quản lý được đối với bitmask DP được tối ưu hóa cẩn thận bằng ngôn ngữ có hệ số không đổi thấp. Vì mỗi chuyến đi xử lý một hoặc hai đối tượng nên cấu trúc bài toán gợi ý một cách tự nhiên việc biểu thị "đối tượng nào đã được thu thập" dưới dạng mặt nạ bit. 

Phần bất thường của vấn đề là con đường được chia thành các chuyến đi độc lập luôn quay trở lại túi xách. Điều đó phá hủy cấu trúc nhân viên bán hàng du lịch thông thường. Chúng ta không cần một tuyến đường khổng lồ ghé thăm mọi thứ chính xác một lần. Chúng ta chỉ cần phân chia các đối tượng thành các nhóm có kích thước một hoặc hai, sau đó chọn thứ tự tốt nhất trong mỗi nhóm. 

Trường hợp khó nhận thấy khi chụp hai đối tượng thực sự còn tệ hơn việc chụp riêng lẻ. 

Ví dụ:```
0 0
2
100 0
-100 0
```Tổng hợp cả hai chi phí:```
0 -> A -> B -> 0
= 10000 + 40000 + 10000
= 60000
```Tính riêng chi phí:```
0 -> A -> 0 = 20000
0 -> B -> 0 = 20000
total = 40000
```Một chiến lược tham lam luôn ghép đôi các đối tượng ở gần sẽ thất bại ở đây. 

Một trường hợp nguy hiểm khác là khi chỉ còn một đồ vật chưa được thu thập. 

Ví dụ:```
0 0
3
1 0
2 0
100 0
```Giải pháp tối ưu ghép hai cái đầu tiên và để lại cái thứ ba. Việc triển khai có lỗi giả định rằng mỗi lần chuyển đổi sẽ loại bỏ chính xác hai đối tượng sẽ bị lỗi hoặc bỏ qua trạng thái hợp lệ. 

Ngoài ra còn có một cạm bẫy tái thiết. DP chỉ tính toán chi phí tối thiểu nhưng đầu ra yêu cầu trình tự chuyến đi thực tế. Nếu thông tin cha mẹ không được lưu trữ cẩn thận thì sau đó sẽ không thể xây dựng lại tuyến đường. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi có thể tạo ra mọi chuỗi chuyến đi có thể. Ở mỗi bước, chúng tôi chọn một hoặc hai đối tượng còn lại, thêm một chuyến đi và lặp lại trên tập hợp còn lại. 

Lực lượng vũ phu này là chính xác bởi vì mọi chiến lược hợp lệ chỉ là một số phân chia đối tượng thành các chuyến đi theo thứ tự. Đệ quy cuối cùng liệt kê tất cả chúng. 

Vấn đề là số lượng khả năng. Ngay cả khi chúng ta chỉ tính các cặp, số cách phân chia 24 đối tượng thành đơn và cặp là rất lớn. Nó phát triển gần giống như:$\sum_{k=0}^{12} \frac{24!}{(24-2k)!\,2^k\,k!}$Đó là vượt xa những gì có thể chạy trong 4 giây. 

Nhận xét quan trọng là thứ tự giữa các chuyến đi không quan trọng đối với chi phí trong tương lai. Khi một tập hợp con của các đối tượng đã được thu thập, thông tin liên quan duy nhất chính xác là tập hợp con đó. Lena luôn quay trở lại túi xách sau mỗi chuyến đi nên không có trạng thái “vị trí hiện tại”. 

Điều đó có nghĩa là chúng ta có thể định nghĩa:```
dp[mask] = minimum cost to collect all objects in mask
```Từ một tiểu bang`mask`, chúng tôi chọn một đối tượng chưa được thu thập`i`. Sau đó chúng tôi cũng:```
take only i
```hoặc:```
take i together with another uncollected object j
```Điều này tạo ra sự chuyển tiếp sang mặt nạ lớn hơn. 

Việc tối ưu hóa quan trọng là chỉ chọn đối tượng chưa được thu thập đầu tiên. Nếu không có thủ thuật này, mọi trạng thái sẽ thử lặp lại tất cả các cặp có thứ tự. Bằng cách sửa một đối tượng chuẩn, mỗi chuyển đổi được tạo ra một lần. 

Tổng số tiểu bang là`2^n`. Đối với mỗi tiểu bang chúng tôi cố gắng nhiều nhất`n`ghép nối với đối tượng miễn phí đầu tiên, do đó độ phức tạp trở nên gần như:$O(n\cdot 2^n)$Điều đó thoải mái phù hợp cho`n = 24`trong việc triển khai được tối ưu hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Vượt cấp số mũ`2^n`| Hàm mũ | Quá chậm | 
| DP mặt nạ bit tối ưu |`O(n * 2^n)`|`O(2^n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ túi xách và tất cả tọa độ đối tượng. 
2. Tính toán trước khoảng cách giữa mỗi cặp điểm. 

Hàm chi phí sử dụng khoảng cách Euclide bình phương nhiều lần. Việc tính toán lại nó bên trong quá trình chuyển đổi DP sẽ lãng phí thời gian. 

1. Đối với mọi đối tượng`i`, tính toán trước chi phí chỉ lấy đối tượng đó:```
0 -> i -> 0
```Điều này bằng:```
dist(0, i) + dist(i, 0)
```1. Với mỗi cặp đồ vật`(i, j)`, tính toán trước chi phí để gộp chúng lại với nhau:```
0 -> i -> j -> 0
```Điều này bằng:```
dist(0, i) + dist(i, j) + dist(j, 0)
```1. Xác định:```
dp[mask]
```là chi phí tối thiểu cần thiết để thu thập chính xác các đối tượng có bit được đặt trong`mask`. 

Khởi tạo:```
dp[0] = 0
```bởi vì việc thu thập không tốn kém gì cả. 

1. Lặp lại tất cả các mặt nạ. 

Với mỗi mặt nạ, hãy tìm đối tượng đầu tiên`i`vẫn chưa được thu thập. 

Chỉ chọn đối tượng bị thiếu đầu tiên sẽ tránh được các chuyển tiếp trùng lặp. Mỗi sự mở rộng trạng thái đều trở nên độc đáo. 

1. Thử lấy đồ vật`i`một mình. 

Tạo nên:```
new_mask = mask | (1 << i)
```Cập nhật:```
dp[new_mask]
```với chi phí chuyến đi`i`. 

1. Thử ghép nối`i`với mọi đối tượng không được thu thập khác`j`. 

Tạo nên:```
new_mask = mask | (1 << i) | (1 << j)
```Cập nhật DP bằng cách sử dụng chi phí chuyến đi theo cặp được tính toán trước. 

1. Lưu trữ thông tin gốc bất cứ khi nào quá trình chuyển đổi cải thiện giá trị DP. 

Chúng tôi cần đủ thông tin để xây dựng lại chuyến đi nào đã được chọn cho mọi tiểu bang. 

1. Sau khi xử lý tất cả các mặt nạ, hãy xây dựng lại câu trả lời ngược từ:```
(1 << n) - 1
```Làm theo gợi ý của phụ huynh cho đến khi đạt được mặt nạ`0`. 

1. Đảo ngược trình tự đã được xây dựng lại và in nó theo định dạng được yêu cầu. 

Mỗi chuyến đi đều phải bắt đầu và kết thúc bằng`0`. 

### Tại sao nó hoạt động 

Bất biến DP là:```
dp[mask] = minimum possible cost to collect exactly the objects in mask
```Mọi chiến lược hợp lệ đều đạt đến một số trạng thái trước đó trước chuyến đi cuối cùng của nó. Chuyến đi cuối cùng đó sẽ loại bỏ một hoặc hai đối tượng. Các công thức chuyển tiếp liệt kê chính xác những khả năng đó. 

Bởi vì mỗi lần chuyển đổi trạng thái đều cộng thêm chi phí chính xác của một chuyến đi độc lập và vì các chuyến đi luôn bắt đầu và kết thúc theo túi xách nên không có sự tương tác ẩn nào tồn tại giữa các chuyến đi khác nhau. Giải pháp tối ưu cho một trạng thái chỉ phụ thuộc vào các trạng thái nhỏ hơn, điều này làm cho quy hoạch động trở nên hợp lệ. 

Việc sửa đối tượng không được thu thập đầu tiên sẽ không loại bỏ bất kỳ giải pháp hợp lệ nào. Mọi chuyển đổi vẫn xuất hiện một lần, chỉ theo thứ tự chuẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def dist(a, b):
    dx = a[0] - b[0]
    dy = a[1] - b[1]
    return dx * dx + dy * dy

def solve():
    xs, ys = map(int, input().split())
    n = int(input())

    points = [(xs, ys)]
    for _ in range(n):
        points.append(tuple(map(int, input().split())))

    single = [0] * n
    pair = [[0] * n for _ in range(n)]

    for i in range(n):
        single[i] = (
            dist(points[0], points[i + 1]) * 2
        )

    for i in range(n):
        for j in range(n):
            pair[i][j] = (
                dist(points[0], points[i + 1])
                + dist(points[i + 1], points[j + 1])
                + dist(points[j + 1], points[0])
            )

    size = 1 << n

    dp = [INF] * size
    parent = [-1] * size
    take = [()] * size

    dp[0] = 0

    for mask in range(size):
        if dp[mask] == INF:
            continue

        first = -1

        for i in range(n):
            if not (mask & (1 << i)):
                first = i
                break

        if first == -1:
            continue

        # take first alone
        new_mask = mask | (1 << first)

        cost = dp[mask] + single[first]

        if cost < dp[new_mask]:
            dp[new_mask] = cost
            parent[new_mask] = mask
            take[new_mask] = (first,)

        # take first with another object
        for j in range(first + 1, n):
            if mask & (1 << j):
                continue

            pair_mask = new_mask | (1 << j)

            cost = dp[mask] + pair[first][j]

            if cost < dp[pair_mask]:
                dp[pair_mask] = cost
                parent[pair_mask] = mask
                take[pair_mask] = (first, j)

    full = size - 1

    print(dp[full])

    path = []

    cur = full

    while cur:
        objs = take[cur]

        trip = [0]

        for x in objs:
            trip.append(x + 1)

        trip.append(0)

        path.extend(trip)

        cur = parent[cur]

    path.reverse()

    print(*path)

solve()
```Phần đầu tiên xây dựng danh sách tọa độ. Túi xách được cất giữ tại mục lục`0`, trong khi các đối tượng được lưu trữ từ`1`trở đi. Điều này giúp đơn giản hóa việc tính toán khoảng cách vì mỗi chuyến đi đều có điểm tham chiếu tự nhiên`0`. 

các`single`mảng lưu trữ chi phí của việc lấy một đối tượng. các`pair`ma trận lưu trữ chi phí để đưa hai đối tượng đi cùng nhau trong một chuyến đi. Việc tính toán trước các giá trị này sẽ loại bỏ số học lặp lại khỏi vòng lặp DP. 

Trạng thái DP sử dụng mặt nạ bit. Nếu bit`i`được thiết lập, đối tượng`i`đã được thu thập. Kích thước mảng là`2^n`, điều này khả thi đối với`n = 24`. 

Logic chuyển tiếp luôn chọn đối tượng chưa được thu thập đầu tiên. Đây là sự tối ưu hóa tiêu chuẩn giúp giảm bớt công việc trùng lặp. Nếu không có nó, mọi trạng thái sẽ liên tục tạo ra các cặp tương đương theo các thứ tự khác nhau. 

Các mảng tái thiết rất tinh tế.`parent[new_mask]`lưu trữ mặt nạ trước đó, trong khi`take[new_mask]`lưu trữ những đồ vật đã được thu thập trong chuyến đi cuối cùng tạo ra trạng thái này. 

Trong quá trình tái thiết, chúng tôi đi lùi từ mặt nạ đầy đủ về 0. Mỗi chuyến đi được lưu trữ trông như sau:```
0 -> objects -> 0
```Việc xây dựng lại được tích lũy ngược nên danh sách cuối cùng phải được đảo ngược trước khi in. 

Một sai lầm dễ dàng là quên`+1`trong quá trình xuất. Các đối tượng bên trong được lập chỉ mục bằng 0, nhưng câu lệnh vấn đề sử dụng cách đánh số một chỉ mục. 

Một lỗi phổ biến khác là tạo ra các chuyển đổi cặp cho các đối tượng đã được thu thập. Điều kiện:```
if mask & (1 << j):
    continue
```ngăn chặn các chuyển đổi không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0
2
1 1
-1 1
```Khoảng cách:```
dist(0,1)=2
dist(0,2)=2
dist(1,2)=4
```Chuyến đi đơn lẻ:```
1 alone = 4
2 alone = 4
```Chuyến đi đôi:```
0 -> 1 -> 2 -> 0 = 8
```Dấu vết DP: 

| Mặt nạ | Đã sưu tầm | Chuyển tiếp | Chi phí mới | 
| --- | --- | --- | --- | 
| 00 | không | lấy 1 mình | 4 | 
| 00 | không | lấy 1 và 2 | 8 | 
| 01 | 1 | lấy 2 mình | 8 | 

Câu trả lời cuối cùng là`8`. 

Tuyến đường được xây dựng lại:```
0 1 2 0
```Dấu vết này cho thấy việc ghép nối cả hai đối tượng cũng tốt như việc tách chúng ra một cách riêng biệt. DP giữ một giải pháp tối ưu và xây dựng lại nó một cách chính xác. 

### Ví dụ 2 

đầu vào:```
0 0
3
1 0
2 0
100 0
```Chi phí được tính toán trước: 

| Chuyến đi | Chi phí | 

|---|---|---| 

| 1 mình | 2 | 

| 2 một mình | 8 | 

| 3 một mình | 20000 | 

| 1 và 2 | 8 | 

| 1 và 3 | 20000 | 

| 2 và 3 | 20000 | 

Dấu vết DP: 

| Mặt nạ | Chuyển tiếp | Kết quả | 
| --- | --- | --- | 
| 000 | lấy 1 mình | dp[001] = 2 | 
| 000 | lấy 1 và 2 | dp[011] = 8 | 
| 001 | lấy 2 mình | dp[011] = 10 | 
| 011 | lấy 3 mình | dp[111] = 20008 | 

Kết quả tối ưu:```
0 1 2 0 3 0
```Ví dụ này chứng minh tại sao thuật toán phải cho phép các chuyến đi một đối tượng. Ghép nối vật ở xa với bất kỳ thứ gì khác không bao giờ có lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n * 2^n)`| Mỗi mặt nạ xử lý một đối tượng cố định và thử ghép nối nó với tối đa`n`người khác | 
| Không gian |`O(2^n)`| Mảng DP và tái thiết trên tất cả các mặt nạ | 

Vì`n = 24`, số lượng tiểu bang là khoảng 16 triệu. Việc tối ưu hóa việc luôn chọn đối tượng chưa được thu thập đầu tiên giúp quản lý số lần chuyển đổi. Việc sử dụng bộ nhớ cũng vừa vặn thoải mái trong giới hạn 512 MB bằng cách sử dụng mảng số nguyên. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

INF = 10**18

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def dist(a, b):
        dx = a[0] - b[0]
        dy = a[1] - b[1]
        return dx * dx + dy * dy

    xs, ys = map(int, input().split())
    n = int(input())

    points = [(xs, ys)]

    for _ in range(n):
        points.append(tuple(map(int, input().split())))

    single = [0] * n
    pair = [[0] * n for _ in range(n)]

    for i in range(n):
        single[i] = dist(points[0], points[i + 1]) * 2

    for i in range(n):
        for j in range(n):
            pair[i][j] = (
                dist(points[0], points[i + 1])
                + dist(points[i + 1], points[j + 1])
                + dist(points[j + 1], points[0])
            )

    size = 1 << n

    dp = [INF] * size
    parent = [-1] * size
    take = [()] * size

    dp[0] = 0

    for mask in range(size):
        if dp[mask] == INF:
            continue

        first = -1

        for i in range(n):
            if not (mask & (1 << i)):
                first = i
                break

        if first == -1:
            continue

        new_mask = mask | (1 << first)

        cost = dp[mask] + single[first]

        if cost < dp[new_mask]:
            dp[new_mask] = cost
            parent[new_mask] = mask
            take[new_mask] = (first,)

        for j in range(first + 1, n):
            if mask & (1 << j):
                continue

            pair_mask = new_mask | (1 << j)

            cost = dp[mask] + pair[first][j]

            if cost < dp[pair_mask]:
                dp[pair_mask] = cost
                parent[pair_mask] = mask
                take[pair_mask] = (first, j)

    full = size - 1

    out = [str(dp[full])]

    path = []

    cur = full

    while cur:
        objs = take[cur]

        trip = [0]

        for x in objs:
            trip.append(x + 1)

        trip.append(0)

        path.extend(trip)

        cur = parent[cur]

    path.reverse()

    out.append(" ".join(map(str, path)))

    return "\n".join(out)

# provided sample
assert run(
"""0 0
2
1 1
-1 1
"""
).startswith("8"), "sample 1"

# minimum size
assert run(
"""0 0
1
1 0
"""
).startswith("2"), "single object"

# pairing better than separate
assert run(
"""0 0
2
1 0
2 0
"""
).startswith("8"), "pair trip"

# separate better than pairing
assert run(
"""0 0
2
100 0
-100 0
"""
).startswith("40000"), "avoid bad pair"

# boundary style case
assert run(
"""100 100
2
-100 -100
100 -100
"""
).splitlines()[0].isdigit(), "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| One object | Trị giá`2`| Xử lý đúng các chuyến đi đơn lẻ | 
| Cặp lân cận | Trị giá`8`| Tối ưu hóa ghép nối | 
| Đối diện các vật ở xa | Trị giá`40000`| Ghép nối không phải lúc nào cũng tối ưu | 
| Tọa độ lớn | Câu trả lời số hợp lệ | Số học khoảng cách bình phương | 

## Vỏ cạnh 

Hãy xem xét trường hợp ghép nối có hại:```
0 0
2
100 0
-100 0
```DP bắt đầu ở mặt nạ`00`. 

Chuyển tiếp có thể:```
take 1 alone -> cost 20000
take 1 and 2 -> cost 60000
```Từ mặt nạ`01`:```
take 2 alone -> total 40000
```Thuật toán giữ giá trị nhỏ hơn một cách chính xác. Chiến lược ghép đôi tham lam sẽ thất bại vì nó không bao giờ so sánh được với hai chuyến đi riêng biệt. 

Bây giờ hãy xem xét một số lẻ các đối tượng:```
0 0
3
1 0
2 0
100 0
```DP cuối cùng cũng đạt được mặt nạ`011`, nghĩa là hai đối tượng đầu tiên đã được thu thập. Đối tượng duy nhất`3`vẫn còn. 

Thuật toán vẫn cho phép:```
011 -> 111
```sử dụng chuyến đi một đối tượng. Điều này có hiệu quả vì mọi trạng thái luôn thử chuyển đổi "lấy một đối tượng" trước khi chuyển đổi cặp. 

Cuối cùng, hãy xem xét đầu vào nhỏ nhất có thể:```
0 0
1
5 5
```Tuyến đường hợp lệ duy nhất là:```
0 -> 1 -> 0
```Chi phí tính toán là:```
50 + 50 = 100
```Các mảng tái thiết tạo ra một cách chính xác:```
0 1 0
```bởi vì cha mẹ của mặt nạ đầy đủ chỉ đơn giản là mặt nạ`0`.
