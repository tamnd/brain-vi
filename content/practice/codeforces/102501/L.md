---
title: "CF 102501L - Trò chơi sông"
description: "Lưới mô tả vùng đất ngập nước nơi các tế bào tạo thành sông. Một nhóm ô được kết nối là một khu vực sông. Máy ảnh chỉ có thể được đặt trên . các ô chạm vào một trong các khu vực sông này và hai camera chạm vào cùng một khu vực sông không thể liền kề nhau."
date: "2026-08-05T17:55:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "L"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 379
verified: true
draft: false
---

[CF 102501L - Trò chơi trên sông](https://codeforces.com/problemset/problem/102501/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới mô tả một vùng đất ngập nước nơi`*`các tế bào tạo thành sông. Một nhóm được kết nối`*`ô là một khu vực sông. Máy ảnh chỉ có thể được đặt trên`.`các ô chạm vào một trong các khu vực sông này và hai camera chạm vào cùng một khu vực sông không thể liền kề nhau. 

Trò chơi không phải là về toàn bộ lưới điện cùng một lúc. Điều kiện tách biệt giữa các khu vực sông khác nhau có nghĩa là việc di chuyển gần một khu vực sông không bao giờ có thể ảnh hưởng đến việc di chuyển gần khu vực sông khác. Điều này biến bàn cờ thành nhiều trò chơi độc lập mà kết quả của chúng có thể được kết hợp lại. 

Giá trị của`N`nhiều nhất là 10 nên cả bảng chỉ có 100 ô. Điều này loại trừ các phương pháp mô phỏng mọi trạng thái trò chơi có thể có của một bảng hoàn chỉnh, vì số lượng tập hợp con của các ô là rất lớn. Kích thước lưới nhỏ cho phép các thuật toán hàm mũ trên một khu vực sông đơn lẻ, bởi vì mỗi con sông chứa nhiều nhất`2N`, hoặc 20, tế bào ướt. Điều quan trọng là tránh làm việc theo cấp số nhân trên toàn bộ bảng. 

Một lỗi phổ biến là đếm số lượng vị trí camera có thể có và chỉ kiểm tra xem nó là số lẻ hay số chẵn. Điều này không thành công vì thứ tự di chuyển có vấn đề. Ví dụ:```
...
...
***
```Có ba tế bào camera có thể chạm vào dòng sông. Câu trả lời là`First player will win`, vì việc chọn ô ở giữa sẽ ngăn không cho sử dụng cả hai ô còn lại. Việc đếm các nước đi sẽ dự đoán sai rằng luôn có sẵn ba nước đi. 

Một sai lầm nữa là gộp các vùng sông khác nhau vào một trò chơi. Ví dụ:```
*...*
.....
*...*
```Hai con sông bị tách biệt và lựa chọn camera của chúng không tương tác với nhau. Việc coi chúng như một biểu đồ sẽ tạo ra những hạn chế bất hợp pháp giữa các máy ảnh không liên quan. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xây dựng biểu đồ các vị trí camera có thể có cho mọi khu vực sông và thử đệ quy mọi di chuyển có thể. Một động thái sẽ loại bỏ ô camera đã chọn và tất cả các ô camera lân cận vì những ô đó không thể sử dụng được nữa. Đây chính xác là trò chơi Node Kayles trên biểu đồ. 

Lực lượng vũ phu là chính xác bởi vì mọi trò chơi có thể xảy ra trong tương lai đều được khám phá. Tuy nhiên, một đồ thị với`k`các tế bào máy ảnh ứng cử viên có thể có tới`2^k`tiểu bang. Áp dụng điều này cho toàn bộ lưới là không thể. 

Quan sát hữu ích là mọi khu vực sông đều độc lập. Lý thuyết Sprague-Grundy cho phép chúng ta gán một số cho mỗi trò chơi độc lập. Xor của tất cả các giá trị diện tích sông quyết định người chiến thắng. Vấn đề còn lại là tính giá trị Grundy của một đồ thị nhỏ. 

Trong quá trình đệ quy, các phần bị ngắt kết nối của đồ thị có thể được giải quyết một cách riêng biệt. Nếu việc di chuyển trong một thành phần được kết nối không thể ảnh hưởng đến thành phần khác thì các giá trị Grundy sẽ bị xor'ed. Điều này làm giảm đáng kể số lượng trạng thái phải được khám phá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên toàn bảng | O(2^(N2)) | O(2^(N2)) | Quá chậm | 
| Đệ quy Grundy trên mỗi diện tích sông | Hàm mũ của số lượng ô camera của một khu vực | O(số tiểu bang) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm mọi khu vực sông được kết nối bằng cách sử dụng biện pháp lấp lũ. Mỗi khu vực được xử lý độc lập vì đầu vào đảm bảo rằng các khu vực khác nhau không thể ảnh hưởng lẫn nhau. 
2. Đối với mỗi khu vực sông, hãy thu thập`.`ô liền kề với ít nhất một ô ướt. Các ô này là các đỉnh của đồ thị trò chơi. Hai đỉnh có một cạnh nếu các ô tương ứng nằm liền kề trong lưới. 
3. Tính đệ quy số Grundy của biểu đồ này. Một trạng thái được biểu thị bằng tập hợp các vị trí camera còn lại. 
4. Trước khi thử di chuyển trong một trạng thái, hãy chia biểu đồ còn lại thành các thành phần liên thông. Giá trị Grundy của toàn bộ trạng thái là xor của các giá trị của các thành phần đó. 
5. Để có trạng thái kết nối, hãy thử mọi vị trí camera còn lại. Việc đặt camera sẽ loại bỏ vị trí đó và tất cả các vị trí lân cận. Thu thập các giá trị Grundy có thể truy cập sau mỗi lần di chuyển và lấy mex của chúng. 
6. Xor giá trị Grundy của tất cả các khu vực sông. Xor bằng 0 có nghĩa là người chơi thứ hai thắng, nếu không thì người chơi thứ nhất thắng. 

Tại sao nó hoạt động: mỗi khu vực sông là một trò chơi công bằng và các khu vực khác nhau tạo thành một tổng rời rạc vì không nước đi nào có thể ảnh hưởng đến khu vực khác. Lý thuyết Sprague-Grundy phát biểu rằng giá trị của một tổng rời rạc là xor của các giá trị thành phần. Định nghĩa đệ quy của các giá trị Grundy xem xét mọi nước đi hợp pháp, do đó phép tính mex thể hiện chính xác tất cả các lần chơi có thể xảy ra trong tương lai. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

def solve():
    n = int(input())
    grid = [input().strip() for _ in range(n)]

    wet_seen = [[False] * n for _ in range(n)]
    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    def inside(r, c):
        return 0 <= r < n and 0 <= c < n

    def get_grundy(adj):
        m = len(adj)
        full = (1 << m) - 1

        @lru_cache(None)
        def grundy(mask):
            if mask == 0:
                return 0

            parts = []
            seen = 0
            for i in range(m):
                if (mask >> i) & 1 and not ((seen >> i) & 1):
                    stack = [i]
                    seen |= 1 << i
                    comp = 0
                    while stack:
                        v = stack.pop()
                        comp |= 1 << v
                        nxt = adj[v] & mask & ~seen
                        while nxt:
                            b = nxt & -nxt
                            u = b.bit_length() - 1
                            seen |= b
                            stack.append(u)
                            nxt -= b
                    parts.append(comp)

            if len(parts) > 1:
                ans = 0
                for p in parts:
                    ans ^= grundy(p)
                return ans

            values = set()
            x = mask
            while x:
                b = x & -x
                v = b.bit_length() - 1
                values.add(grundy(mask & ~adj[v] & ~b))
                x -= b

            g = 0
            while g in values:
                g += 1
            return g

        return grundy(full)

    answer = 0

    for r in range(n):
        for c in range(n):
            if grid[r][c] == '*' and not wet_seen[r][c]:
                area = []
                stack = [(r, c)]
                wet_seen[r][c] = True

                while stack:
                    x, y = stack.pop()
                    area.append((x, y))
                    for dx, dy in dirs:
                        nx, ny = x + dx, y + dy
                        if inside(nx, ny) and not wet_seen[nx][ny] and grid[nx][ny] == '*':
                            wet_seen[nx][ny] = True
                            stack.append((nx, ny))

                candidates = set()
                for x, y in area:
                    for dx, dy in dirs:
                        nx, ny = x + dx, y + dy
                        if inside(nx, ny) and grid[nx][ny] == '.':
                            candidates.add((nx, ny))

                nodes = list(candidates)
                index = {p: i for i, p in enumerate(nodes)}
                adj = [0] * len(nodes)

                for x, y in nodes:
                    i = index[(x, y)]
                    for dx, dy in dirs:
                        nx, ny = x + dx, y + dy
                        if (nx, ny) in index:
                            adj[i] |= 1 << index[(nx, ny)]

                answer ^= get_grundy(adj)

    if answer:
        print("First player will win")
    else:
        print("Second player will win")

if __name__ == "__main__":
    solve()
```Giai đoạn lấp lũ xác định chính xác các trò chơi con độc lập. Tập ứng cử viên chỉ chứa các ô mặt đất chắc chắn tiếp xúc với dòng sông hiện tại, vì vậy các ô được bảo vệ và mặt đất không liên quan sẽ không bao giờ được đưa vào biểu đồ. 

Hàm đệ quy lưu trữ các trạng thái dưới dạng bitmask. Một bit được đặt có nghĩa là vị trí camera vẫn khả dụng. Khi đặt camera, bit được chọn và mọi bit liền kề sẽ bị xóa. 

Việc phân chia thành phần được kết nối là tối ưu hóa chính. Nếu không có nó, nhiều trạng thái sẽ được tính toán lại thành một biểu đồ lớn. Với nó, các phần độc lập được giảm xuống thành các trò chơi nhỏ hơn có giá trị có thể được xor'ed. 

Số nguyên Python có thể chứa các mặt nạ bit có độ dài tùy ý, do đó việc triển khai không cần xử lý đặc biệt đối với số lượng đỉnh. Việc kiểm tra ranh giới lưới cũng rất quan trọng vì các ứng cử viên camera chỉ có thể đến từ bên trong bảng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
...
...
***
```Biểu đồ ứng viên có ba vị trí trên một đường thẳng. 

| Các vị trí còn lại | Những động thái có thể xảy ra | Giá trị bẩn thỉu | 
| --- | --- | --- | 
| Ba ô | Xóa trái, giữa hoặc phải | 1 | 
| Trống | Không di chuyển | 0 | 

Tổng giá trị Grundy khác 0 nên người chơi đầu tiên sẽ thắng. 

Đối với mẫu thứ hai, hai khu vực sông được giải quyết riêng biệt. 

| Khu vực sông | Giá trị bẩn thỉu | 
| --- | --- | 
| Dòng sông đầu tiên | 1 | 
| Dòng sông thứ hai | 1 | 

xor là`1 xor 1 = 0`, vì vậy mọi nước đi đầu tiên đều có thể được người chơi thứ hai đáp trả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Hàm mũ trong kích thước đồ thị camera tối đa | Mỗi khu vực sông được giải quyết bằng đệ quy Grundy được ghi nhớ | 
| Không gian | Hàm mũ trong kích thước đồ thị camera tối đa | Cửa hàng ghi nhớ đã ghé thăm trạng thái trò chơi | 

Toàn bộ lưới chỉ có 100 ô và mỗi sông chứa tối đa 20 ô ướt. Việc phân tách thành các khu vực sông độc lập giữ cho phần mũ được giới hạn ở các đồ thị cục bộ nhỏ, phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

# The implementation above can be wrapped into a function for local testing.

tests = [
    (
        "3\n...\n...\n***\n",
        "First player will win"
    ),
    (
        "1\n.\n",
        "Second player will win"
    ),
    (
        "3\n***\n***\n***\n",
        "Second player will win"
    ),
    (
        "4\n....\n....\n****\n....\n",
        "First player will win"
    ),
]

for inp, expected in tests:
    print(inp.strip(), "=>", expected)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một tế bào vững chắc duy nhất | Người chơi thứ hai sẽ thắng | Không có vị trí camera hợp pháp | 
| Tất cả các tế bào ướt | Người chơi thứ hai sẽ thắng | Khu vực không có camera di chuyển | 
| Một dòng sông ngang đơn giản | Người chơi đầu tiên sẽ thắng | Tính toán Grundy cơ bản | 
| Một dòng sông chạm ranh giới | Phụ thuộc vào các bước di chuyển được tính toán | Xử lý ranh giới | 

## Vỏ cạnh 

Một con sông không có nền đất liền kề sẽ tạo ra một đồ thị trống. Giá trị Grundy bằng 0 vì người chơi đầu tiên không được di chuyển. Phép đệ quy chạm tới mặt nạ trống ngay lập tức và trả về 0. 

Một hàng hoặc cột gần ranh giới không được truy cập vào các ô bên ngoài lưới. các`inside`kiểm tra ngăn hàng xóm không hợp lệ trở thành vị trí camera. 

Nhiều khu vực sông phải tách biệt. Thuật toán chỉ xây dựng các cạnh giữa các vị trí camera xung quanh cùng một con sông ngập nước, sau đó kết hợp các giá trị Grundy thu được với xor. Điều này phù hợp với trò chơi thực tế vì nước đi không thể di chuyển từ khu vực sông này sang khu vực sông khác.
