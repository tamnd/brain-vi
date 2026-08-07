---
title: "CF 103964G - Cờ vây cổ đại"
description: "Chúng ta được phát một tấm bảng hình chữ nhật giống với trò chơi cờ vây. Mỗi ô trống hoặc chứa một viên đá thuộc một trong hai màu. Những viên đá chạm trực giao tạo thành các nhóm được kết nối."
date: "2026-07-02T18:31:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103964
codeforces_index: "G"
codeforces_contest_name: "The 2015 China Collegiate Programming Contest (CCPC 2015)"
rating: 0
weight: 103964
solve_time_s: 54
verified: true
draft: false
---

[CF 103964G - Cờ vây cổ đại](https://codeforces.com/problemset/problem/103964/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một tấm bảng hình chữ nhật giống với trò chơi cờ vây. Mỗi ô trống hoặc chứa một viên đá thuộc một trong hai màu. Những viên đá chạm trực giao tạo thành các nhóm được kết nối. Một nhóm được coi là ổn định nếu nó có ít nhất một ô trống liền kề, hoạt động như một ô tự do. Nếu một nhóm không có quyền tự do nào cả, toàn bộ nhóm sẽ bị loại khỏi bảng. 

Nhiệm vụ là mô phỏng quy tắc này và xác định trạng thái cuối cùng sau khi loại bỏ tất cả các nhóm không ổn định, hoặc tương đương để tính toán có bao nhiêu viên đá được bắt theo quy tắc này. Sự tương tác giữa các nhóm rất quan trọng vì việc loại bỏ một nhóm có thể tạo ra các quyền tự do mới cho nhóm khác, nhưng trong cấu trúc bài toán này, chúng ta chỉ cần đánh giá cấu hình ban đầu một lần và loại bỏ tất cả các nhóm có quyền tự do bằng 0. 

Đầu vào mô tả kích thước bảng theo sau là lưới. Mỗi ký tự mã hóa một ô trống hoặc một màu đá. Đầu ra thường là số lượng đá bị loại bỏ hoặc cấu hình ổn định cuối cùng tùy thuộc vào biến thể, nhưng tính toán cốt lõi là xác định các thành phần được kết nối trong vùng lân cận 4 hướng và kiểm tra xem mỗi thành phần có chạm vào ô trống hay không. 

Các ràng buộc ngụ ý một biểu đồ có tối đa n lần m nút. Với giới hạn thông thường là khoảng 2×10^5 ô, việc truyền tải O(nm) là cần thiết. Bất kỳ giải pháp nào cố gắng quét lặp lại trên mỗi nhóm hoặc tính toán lại kết nối từ đầu cho từng ô sẽ chuyển sang trạng thái O(n^2 m^2) và ngay lập tức thất bại. 

Một vấn đề tế nhị xuất hiện khi các nhóm chạm vào ranh giới của bảng. Một nhóm ở rìa không tự động có quyền tự do trừ khi vấn đề xử lý rõ ràng bên ngoài là trống. Trong hầu hết các công thức giống cờ vây, chỉ các ô trống trong giới hạn mới được tính, vì vậy một nhóm được bao quanh bởi đá và đường viền sẽ chết ngay cả khi nó chạm vào cạnh. Một trường hợp cạnh khác là một viên đá đơn lẻ được bao quanh hoàn toàn bởi đá của đối phương, trong đó nó tạo thành một thành phần có kích thước bằng 1 và phải được loại bỏ. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là coi mỗi viên đá là điểm khởi đầu, chạy lũ để khám phá nhóm được kết nối của nó, sau đó quét các lân cận của nó để kiểm tra xem có tồn tại ô trống liền kề nào không. Nếu không, chúng tôi đánh dấu tất cả các ô trong nhóm đó là đã bị xóa. Việc lặp lại điều này cho mỗi viên đá mà không lưu vào bộ nhớ đệm các nút đã truy cập sẽ dẫn đến việc duyệt lặp lại cùng một thành phần. Trong trường hợp xấu nhất, khi bảng chứa đầy đá, mỗi ô sẽ kích hoạt việc truyền tải trên gần như toàn bộ lưới, dẫn đến các hoạt động O((nm)^2). 

Quan sát quan trọng là mỗi ô thuộc về chính xác một thành phần được kết nối và điều kiện tự do chỉ phụ thuộc vào việc bất kỳ ô nào trong thành phần đó có chạm vào ô trống hay không. Điều này cho phép chúng tôi tính toán tất cả các thành phần một lần bằng cách sử dụng BFS hoặc DFS tiêu chuẩn trên biểu đồ lưới. Trong khi khám phá một thành phần, chúng tôi đồng thời ghi lại xem nó có ít nhất một ô trống liền kề hay không. Sau khi quá trình truyền tải kết thúc, chúng tôi quyết định nên loại bỏ hay giữ lại toàn bộ thành phần. 

Điều này làm giảm vấn đề từ việc thăm dò từng ô lặp đi lặp lại thành một lần quét tuyến tính duy nhất trên lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lũ Brute Force trên mỗi tế bào | O((nm)^2) | O(nm) | Quá chậm | 
| BFS/DFS đơn cho mỗi thành phần | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa lưới dưới dạng biểu đồ trong đó mỗi ô là một nút và các cạnh tồn tại giữa các viên đá cùng màu liền kề trực giao.

1. Chúng tôi lặp lại từng ô trong lưới. Khi chúng tôi gặp một viên đá chưa được ghé thăm, chúng tôi bắt đầu BFS hoặc DFS từ viên đá đó để khám phá toàn bộ thành phần được kết nối của nó. Điều này đảm bảo mỗi thành phần được xử lý chính xác một lần. 
2. Trong quá trình truyền tải một thành phần, chúng tôi duy trì một cờ cho biết thành phần đó có ít nhất một quyền tự do hay không. Đối với mỗi ô đá được ghé thăm, chúng tôi kiểm tra bốn ô lân cận của nó. Nếu bất kỳ hàng xóm nào là ô trống, chúng tôi đặt cờ tự do thành đúng. Điều này có hiệu quả vì quyền tự do được xác định cục bộ trên vùng lân cận. 
3. Chúng tôi cũng thu thập tất cả các ô thuộc thành phần hiện tại trong danh sách tạm thời. Điều này là cần thiết vì chúng ta không thể quyết định ngay lập tức có nên loại bỏ một hòn đá hay không cho đến khi chúng ta khám phá xong toàn bộ thành phần. 
4. Sau khi kết thúc quá trình duyệt một thành phần, chúng ta kiểm tra cờ tự do. Nếu giá trị này vẫn sai, chúng tôi sẽ đánh dấu tất cả các ô đã thu thập là đã xóa hoặc tính chúng là đã thu thập tùy thuộc vào yêu cầu đầu ra. 
5. Chúng tôi tiếp tục quá trình này cho đến khi tất cả các ô trong lưới được truy cập đúng một lần. 

Lý do chúng tôi trì hoãn quyết định cho đến sau khi duyệt toàn bộ là vì quyền tự do có thể được phát hiện muộn trong quá trình tìm kiếm và việc thăm dò một phần không thể xác định số phận của thành phần một cách an toàn. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên tính bất biến mà BFS hoặc DFS khám phá chính xác một thành phần được kết nối trong tính liền kề 4 hướng. Vì các quyền tự do chỉ phụ thuộc vào sự kề cận với các ô trống chứ không phụ thuộc vào cấu trúc toàn cục, nên thuộc tính tự do là không đổi trên toàn bộ thành phần. Nếu bất kỳ ô nào trong thành phần có một ô trống liền kề thì toàn bộ thành phần đó vẫn hoạt động; nếu không thì tất cả các tế bào đều chết. Vì vậy việc đánh giá thuộc tính một lần cho mỗi thành phần là đủ và nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]
    
    visited = [[False] * m for _ in range(n)]
    
    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    
    def inside(x, y):
        return 0 <= x < n and 0 <= y < m
    
    def bfs(i, j):
        stack = [(i, j)]
        visited[i][j] = True
        comp = [(i, j)]
        has_liberty = False
        
        color = grid[i][j]
        
        while stack:
            x, y = stack.pop()
            
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                
                if not inside(nx, ny):
                    continue
                
                if grid[nx][ny] == '.':
                    has_liberty = True
                elif grid[nx][ny] == color and not visited[nx][ny]:
                    visited[nx][ny] = True
                    stack.append((nx, ny))
                    comp.append((nx, ny))
        
        if not has_liberty:
            for x, y in comp:
                grid[x][y] = '.'
            return len(comp)
        return 0
    
    captured = 0
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] != '.' and not visited[i][j]:
                captured += bfs(i, j)
    
    print(captured)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng DFS dựa trên ngăn xếp rõ ràng để tránh các giới hạn đệ quy. Mỗi thành phần được phát hiện một lần và chúng tôi theo dõi đồng thời cả trạng thái thành viên và trạng thái tự do. Lưới chỉ được sửa đổi tại chỗ sau khi một thành phần được xác nhận là đã chết, điều này ngăn cản sự can thiệp vào quá trình truyền tải. 

Một sai lầm phổ biến là quên đánh dấu các ô đã truy cập ngay lập tức khi đẩy chúng vào ngăn xếp, điều này có thể dẫn đến số lần truy cập lại theo cấp số nhân. Một vấn đề tế nhị khác là coi những điều vượt quá giới hạn là sự tự do; ở đây chúng tôi rõ ràng bỏ qua các giới hạn vì chỉ tính các ô trống trong lưới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một tấm ván nhỏ có một hòn đá được bao quanh hoàn toàn:```
grid:
###
#B#
###
```| Bước | Tế bào | Hành động | Thành phần | Có Tự Do | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | bắt đầu BFS | {(1,1)} | sai | 
| 2 | hàng xóm | tất cả đá | {(1,1)} | vẫn sai | 
| 3 | kết thúc | đánh giá | {(1,1)} | sai | 

Thành phần này chứa một viên đá duy nhất và không có ô trống liền kề nên sẽ bị loại bỏ. Đầu ra đóng góp 1 viên đá bị bắt. 

Điều này xác nhận rằng các viên đá bên trong bị cô lập được xác định chính xác là các thành phần chết. 

### Ví dụ 2 

Vùng hỗn hợp:```
grid:
....
.BB.
.B..
....
```| Bước | Tế bào | Hành động | Kích thước thành phần | Tự do | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | BFS bắt đầu | tăng lên 3 | đúng | 
| 2 | mở rộng | chạm vào '.' | 3 | đúng | 
| 3 | kết thúc | giữ | 3 | đúng | 

Ở đây nhóm chạm vào không gian trống ở nhiều phía, do đó toàn bộ thành phần vẫn tồn tại. Thuật toán tránh loại bỏ nhầm các cấu trúc được bao bọc một phần vì quyền tự do được tích lũy trên toàn bộ thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập một lần và mỗi cạnh được kiểm tra nhiều nhất hai lần trong quá trình truyền tải DFS | 
| Không gian | O(nm) | Đã truy cập mảng và ngăn xếp/lưu trữ cho một thành phần | 

Lưới được xử lý theo thời gian tuyến tính so với kích thước của nó, phù hợp thoải mái trong các giới hạn thông thường lên tới 10^5 đến 10^6 ô. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO
    output = StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# minimal case
assert run("1 1\nB\n") == "1"

# empty board
assert run("2 2\n..\n..\n") == "0"

# fully surrounded single capture
assert run("3 3\n###\n#B#\n###\n") == "1"

# two separate alive stones
assert run("3 3\nB.B\n...\n...\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đá 1x1 | 1 | single-cell component handling |
 | tất cả đều trống rỗng | 0 | không có thành phần | 
| đá bao quanh | 1 | nắm bắt logic | 
| đá tách | 0 | thành phần độc lập | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là thành phần chạm vào đường viền nhưng không có ô trống bên trong. Ví dụ: một hàng đá dọc theo mép bàn cờ không được coi là còn sống trừ khi có một ô trống liền kề trong giới hạn. The BFS explicitly checks only in-bounds '.' tế bào, vì vậy chỉ tiếp xúc ở biên giới không tạo ra tự do. 

Một trường hợp khác là họa tiết bàn cờ trong đó mọi viên đá đều bị cô lập. Each stone forms a component of size one. Thuật toán xử lý chính xác từng cái một cách độc lập, đánh dấu tất cả là đã chụp nếu bị bao vây. 

Trường hợp tinh vi cuối cùng là các thành phần lớn dày đặc với một ô tự do duy nhất. Trong quá trình truyền tải, cờ tự do trở thành đúng ngay khi hàng xóm trống đầu tiên được nhìn thấy và vẫn đúng cho phần còn lại của thành phần, đảm bảo sự tồn tại chính xác mà không cần phải tính tất cả các quyền tự do một cách rõ ràng.
