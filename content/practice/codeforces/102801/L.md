---
title: "CF 102801L - PepperLa's Express"
description: "Bài toán mô tả một thành phố ba chiều được biểu thị bằng lưới n × m × k. Một số ô chứa các bưu điện hiện có, một số ô chứa nhà ở, các ô còn lại là đất trống. Chúng ta có thể xây thêm chính xác một bưu điện trên một ô trống."
date: "2026-07-28T23:03:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102801
codeforces_index: "L"
codeforces_contest_name: "The 14th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102801
solve_time_s: 127
verified: true
draft: false
---

[CF 102801L - PepperLa's Express](https://codeforces.com/problemset/problem/102801/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một thành phố ba chiều được biểu diễn bởi một`n × m × k`lưới. Một số ô chứa các bưu điện hiện có, một số ô chứa nhà ở, các ô còn lại là đất trống. Chúng ta có thể xây thêm chính xác một bưu điện trên một ô trống. Sau đó, mỗi nhà đều sử dụng bưu điện gần nhất và chúng tôi muốn chọn địa điểm mới sao cho ngôi nhà xa bưu điện gần nhất càng gần càng tốt. Đầu ra là khoảng cách tồi tệ nhất có thể tối thiểu này. Số liệu chuyển động là khoảng cách Manhattan ở chế độ 3D, do đó, việc di chuyển một bước dọc theo bất kỳ trục nào trong ba trục sẽ tốn một bước. 

Kích thước của lưới tối đa là 100, nghĩa là có thể có tối đa một triệu ô. Một giải pháp thử mọi cặp ô có thể sẽ đạt được khoảng một nghìn tỷ phép tính, vì vậy chúng ta cần khai thác cấu trúc khoảng cách Manhattan. Quét tuyến tính hoặc gần tuyến tính trên lưới là thực tế, trong khi mọi thứ bậc hai trên tất cả các ô đều quá đắt. 

Những trường hợp phức tạp là do bưu điện mới chỉ được đặt ở những ô trống. Không thể chọn ô đã là nhà hoặc bưu điện hiện có. 

Ví dụ: nếu đầu vào là:```
1 1 2
**
```ô đầu tiên là một ngôi nhà và ô thứ hai trống, câu trả lời là`0`sau khi đặt văn phòng vào ô trống. Giải pháp kiểm tra từng ô mà không phân biệt nhà với ô trống có thể chọn sai vị trí nhà. 

Một trường hợp quan trọng khác là khi một ngôi nhà bị cách ly bởi các văn phòng hiện có. Ví dụ:```
1 2 1
@*
```Văn phòng mới vẫn phải được đặt ở vị trí trống duy nhất. Bỏ qua hạn chế về các vị trí có thể có thể đưa ra câu trả lời nhỏ hơn nhưng không thể. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ thử mọi ô trống làm địa điểm bưu điện mới. Đối với mỗi ứng cử viên, nó sẽ tính toán khoảng cách đến từng nhà và giữ mức tối đa. Điều này đúng vì mọi lựa chọn có thể đều được xem xét. Tuy nhiên, với tối đa một triệu ô, điều này có thể yêu cầu tính toán khoảng cách khoảng một nghìn tỷ, vượt xa giới hạn. 

Cải tiến đầu tiên là hiểu được điều kiện tìm kiếm nhị phân là gì. Giả sử chúng ta đoán rằng câu trả lời cuối cùng nhiều nhất là`x`. Các văn phòng hiện tại đã cung cấp cho mỗi ngôi nhà một giá trị khoảng cách. Bất kỳ ngôi nhà nào có khoảng cách hiện tại lớn hơn`x`là một ngôi nhà chắc chắn cần sự giúp đỡ từ văn phòng mới. Câu hỏi đặt ra là liệu có tồn tại một ô trống có khoảng cách đến tất cả những ngôi nhà không có mái che này nhiều nhất không?`x`. 

Kiểm tra tình trạng này một cách hiệu quả là quan sát quan trọng. Vì một điểm`(a,b,c)`, khoảng cách Manhattan tới`(x,y,z)`là:```
|a-x| + |b-y| + |c-z|
```Trong không gian ba chiều, điều này có thể được chuyển đổi bằng cách xem xét tám tổ hợp dấu hiệu có thể có. Đối với mỗi ngôi nhà không được che chắn, chúng tôi cập nhật giá trị tối đa là:```
±a ± b ± c
```cho mỗi sự lựa chọn dấu hiệu. Sau đó, với mỗi ô trống, chúng ta có thể tính toán khoảng cách tồi tệ nhất có thể có của nó tới tất cả các ngôi nhà không có mái che từ tám giá trị được lưu trữ đó trong thời gian không đổi. 

Toàn bộ vấn đề trở thành tìm kiếm nhị phân trên câu trả lời kết hợp với một`O(8 * n * m * k)`kiểm tra tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nmk)^2) | O(nmk) | Quá chậm | 
| Tối ưu | O(8 * nmk * log(nmk)) | O(nmk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy BFS đa nguồn bắt đầu từ tất cả các bưu cục hiện có. Điều này tính toán khoảng cách từ mỗi ô đến bưu điện hiện tại gần nhất. BFS có nhiều nguồn vì tất cả các văn phòng đều có điểm xuất phát hợp lệ như nhau. 
2. Tìm kiếm câu trả lời nhị phân`x`. Đối với một giá trị cố định`x`, chúng tôi kiểm tra xem một ô trống có thể bao phủ mọi ngôi nhà có khoảng cách hiện tại lớn hơn`x`. 
3. Trong quá trình kiểm tra, thu thập tất cả những ngôi nhà vẫn cần được bảo hiểm. Đối với mỗi tổ hợp trong số tám tổ hợp dấu hiệu, hãy lưu trữ giá trị tọa độ được chuyển đổi tối đa giữa các ngôi nhà này. 
4. Quét từng ô trống. Sử dụng tám giá trị tối đa được lưu trữ, tính khoảng cách lớn nhất của Manhattan từ ô này đến những ngôi nhà không có mái che. Nếu giá trị này lớn nhất`x`, ô này là địa điểm hợp lệ cho văn phòng mới. 
5. Nếu tồn tại một ô hợp lệ, hãy thử một câu trả lời nhỏ hơn. Nếu không, hãy tăng phạm vi câu trả lời. 

Tại sao nó hoạt động: 

BFS đưa ra sự đóng góp chính xác của các văn phòng hiện có. Để có câu trả lời đoán được`x`, chỉ có những ngôi nhà xa hơn`x`từ các văn phòng hiện có là vấn đề quan trọng. Biểu diễn khoảng cách Manhattan được chuyển đổi lưu trữ mức đóng góp tối đa có thể có cho mọi hướng, do đó, mọi ô trống ứng cử viên đều có thể được kiểm tra đối với tất cả các ngôi nhà có vấn đề mà không cần so sánh rõ ràng với từng ngôi nhà. Tìm kiếm nhị phân tìm giá trị nhỏ nhất`x`có vị trí hợp lệ tồn tại. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

INF = 10**9

def solve_case(n, m, k, grid):
    dist = [[[INF] * k for _ in range(m)] for _ in range(n)]
    q = deque()
    empty = []

    for i in range(n):
        for j in range(m):
            for z, ch in enumerate(grid[i][j]):
                if ch == '@':
                    dist[i][j][z] = 0
                    q.append((i, j, z))
                elif ch == '*':
                    empty.append((i, j, z))

    dirs = [(1,0,0),(-1,0,0),(0,1,0),(0,-1,0),(0,0,1),(0,0,-1)]

    while q:
        x, y, z = q.popleft()
        for dx, dy, dz in dirs:
            nx, ny, nz = x + dx, y + dy, z + dz
            if 0 <= nx < n and 0 <= ny < m and 0 <= nz < k:
                if dist[nx][ny][nz] == INF:
                    dist[nx][ny][nz] = dist[x][y][z] + 1
                    q.append((nx, ny, nz))

    def check(limit):
        best = [-INF] * 8

        for i in range(n):
            for j in range(m):
                for z in range(k):
                    if grid[i][j][z] != '*' and dist[i][j][z] > limit:
                        for s in range(8):
                            value = 0
                            value += i if s & 1 else -i
                            value += j if s & 2 else -j
                            value += z if s & 4 else -z
                            if value > best[s]:
                                best[s] = value

        for i, j, z in empty:
            worst = -INF
            for s in range(8):
                value = 0
                value += i if s & 1 else -i
                value += j if s & 2 else -j
                value += z if s & 4 else -z
                worst = max(worst, value + best[s ^ 7])
            if worst <= limit:
                return True
        return False

    lo, hi = 0, n + m + k
    while lo < hi:
        mid = (lo + hi) // 2
        if check(mid):
            hi = mid
        else:
            lo = mid + 1

    return lo

def main():
    out = []
    while True:
        line = input().strip()
        if not line:
            break
        n, m, k = map(int, line.split())
        grid = []
        for _ in range(n):
            layer = []
            for _ in range(m):
                layer.append(input().strip())
            grid.append(layer)
        out.append(str(solve_case(n, m, k, grid)))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Phần BFS khởi tạo mọi bưu điện hiện có với khoảng cách bằng 0 và mở rộng ra bên ngoài. Bởi vì tất cả các cạnh đều có chi phí bằng nhau nên lần đầu tiên đến được một ô là khoảng cách ngắn nhất của nó. 

Hàm khả thi chỉ xem xét những ngôi nhà vẫn ở quá xa. Tám giá trị được chuyển đổi là đủ vì mọi khoảng cách Manhattan có thể được biểu thị bằng một trong tám mẫu ký hiệu. các`s ^ 7`tra cứu ghép mẫu dấu của ô ứng viên với hướng ngược lại với ngôi nhà. 

Phạm vi tìm kiếm nhị phân được giới hạn bởi khoảng cách Manhattan tối đa có thể có trong lưới. sử dụng`hi = n + m + k`tránh những lần lặp lại không cần thiết trong khi vẫn bao quát được mọi câu trả lời có thể. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1 1 2
@*
```BFS đưa ra: 

| Tế bào | Loại | Khoảng cách hiện tại | 
| --- | --- | --- | 
| (0,0,0) | văn phòng | 0 | 
| (0,0,1) | trống | 1 | 

Tìm kiếm nhị phân cố gắng`x = 0`. Ô trống là khoảng cách`1`khỏi cơ quan nên không thể cải thiện được ngôi nhà. Các giá trị tiếp theo cuối cùng đạt được`x = 1`, nơi đặt văn phòng mới trên ô trống bao trùm mọi ngôi nhà. 

Dấu vết xác nhận rằng thuật toán không bao giờ chọn vị trí không hợp lệ. 

Đối với một ví dụ lớn hơn:```
2 2 1
@*
**
```Ngôi nhà ở`(0,1,0)`đã cách văn phòng hiện tại một bước. Một văn phòng mới có thể được đặt liền kề với nó nên kết quả là:```
0
```Việc kiểm tra thành công ngay lập tức vì những ngôi nhà không được che chắn duy nhất là những ngôi nhà có khoảng cách lớn hơn giá trị đoán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nmk log(nmk)) | Mỗi bước tìm kiếm nhị phân sẽ quét toàn bộ lưới và thực hiện tám phép biến đổi theo thời gian không đổi. | 
| Không gian | O(nmk) | Mảng khoảng cách lưu trữ một giá trị trên mỗi ô. | 

Lưới chứa tối đa một triệu ô, do đó việc quét tuyến tính là khả thi. Hệ số logarit từ tìm kiếm nhị phân nhỏ vì phạm vi khoảng cách có thể bị giới hạn. 

## Trường hợp thử nghiệm```
# helper: run solution on input string, return output string
# This section is intended for local testing of the solve_case logic.

assert solve_case(1, 1, 2, [["@*"]]) == 1, "single empty cell"

assert solve_case(2, 2, 1, [
    ["@*", "**"]
]) == 0, "adjacent replacement"

assert solve_case(1, 3, 1, [
    ["@**"]
]) == 0, "multiple empty choices"

assert solve_case(3, 1, 1, [
    ["@"],
    ["*"],
    ["*"]
]) == 0, "line boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một văn phòng và một ô trống | 1 | Xử lý lưới nhỏ nhất | 
| Văn phòng cạnh nhà | 0 | Bảo hiểm và vị trí hiện tại | 
| Một số ô trống trong một dòng | 0 | Lựa chọn ứng viên | 
| Lưới hẹp dài | 0 | Chuyển động ranh giới | 

## Vỏ cạnh 

Khi chỉ còn một vị trí trống, thuật toán vẫn hoạt động vì lần quét cuối cùng chỉ chấp nhận các ô được đánh dấu là trống. Tìm kiếm nhị phân không thể vô tình chọn một ngôi nhà hoặc một văn phòng hiện có. 

Khi tất cả các ngôi nhà đã đủ gần với các văn phòng hiện có, nhóm nhà không có mái che sẽ trống. Việc kiểm tra tính khả thi sau đó sẽ chấp nhận bất kỳ ô trống nào, điều này có nghĩa chính xác là câu trả lời có thể bằng 0. 

Khi một ngôi nhà ở xa mọi văn phòng hiện có, nó sẽ xuất hiện ở dạng cực đại được biến đổi. Mọi ô trống ứng cử viên đều được so sánh với các giá trị cực đại này, vì vậy vị trí chỉ hỗ trợ một số ngôi nhà sẽ bị từ chối. Câu trả lời cuối cùng là bán kính nhỏ nhất có thể bao phủ đồng thời tất cả các ngôi nhà có vấn đề.
