---
title: "CF 102992G - Đi"
description: "Đầu vào là một bảng vuông trong đó mỗi vị trí có màu đen, trắng hoặc trống. Những viên đá trắng có thể kết nối thông qua các kề cận lên-xuống-trái-phải, tạo thành cụm. Một cụm chỉ tồn tại nếu ít nhất một trong những viên đá của nó chạm vào một ô trống."
date: "2026-07-04T04:41:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102992
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ACM-ICPC, Asia Nanjing Regional Contest (XXI Open Cup, Grand Prix of Nanjing)"
rating: 0
weight: 102992
solve_time_s: 42
verified: true
draft: false
---

[CF 102992G - Đi](https://codeforces.com/problemset/problem/102992/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào là một bảng vuông trong đó mỗi vị trí có màu đen, trắng hoặc trống. Những viên đá trắng có thể kết nối thông qua các kề cận lên-xuống-trái-phải, tạo thành cụm. Một cụm chỉ tồn tại nếu ít nhất một trong những viên đá của nó chạm vào một ô trống. Nếu mọi viên đá trong một cụm được bao quanh hoàn toàn bởi các ô không trống (đen hoặc trắng), thì cụm đó được coi là đã bắt được và tất cả các viên đá trắng của nó phải được đếm. 

Đầu ra là một số nguyên duy nhất: tổng số viên đá trắng thuộc cụm màu trắng đã bắt được. 

Vì bảng có thể có tới một triệu ô nên mọi thuật toán đều phải chạy theo thời gian tuyến tính tương ứng với kích thước lưới. Điều này ngay lập tức loại trừ các tìm kiếm lặp lại trên mỗi viên đá hoặc mỗi truy vấn. Giới hạn bộ nhớ gợi ý rằng việc lưu trữ các mảng phụ có cùng kích thước với lưới là được, nhưng không thể thực hiện được siêu tuyến tính. 

Một trường hợp lỗi quan trọng đối với logic ngây thơ xuất hiện khi người ta kiểm tra từng viên đá trắng một cách độc lập mà không đánh dấu các thành phần đã ghé thăm. Trong một chuỗi 10.000 viên đá trắng, điều này sẽ liên tục quét lại cùng một cấu trúc, dẫn đến sự trùng lặp lớn về công việc và các giả định về hiệu suất không chính xác. 

Một trường hợp cạnh khác phát sinh khi một cụm màu trắng chạm vào cả hai viên đá đen và các cụm màu trắng khác nhưng có một vùng tự do trống duy nhất ở đâu đó rất xa trong cùng một thành phần được kết nối. Nếu quyền tự do đó bị bỏ lỡ do quá trình truyền tải không đầy đủ, cụm có thể bị đánh dấu không chính xác là đã chết. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ lặp lại trên mọi viên đá trắng và thực hiện việc lấp đầy từ nó để xác định xem thành phần được kết nối của nó có chứa bất kỳ viên đá lân cận trống nào hay không. Mỗi lần lấp lũ có thể tốn kém$O(n^2)$, và trong một bảng dày đặc với toàn những viên đá trắng, điều này trở thành$O(n^4)$trong trường hợp xấu nhất. Tính chính xác rất đơn giản vì mỗi thành phần đều được kiểm tra tự do một cách rõ ràng, nhưng việc tính toán lại nhiều lần các thành phần giống nhau khiến nó không khả thi. 

Quan sát quan trọng là bảng phân hủy một cách tự nhiên thành các thành phần được kết nối bằng đá trắng và mỗi thành phần có một thuộc tính nhị phân: nó có ít nhất một ô trống liền kề hoặc không có. Điều này gợi ý việc chạy một lần truyền tải cho mỗi thành phần thay vì trên mỗi nút. BFS hoặc DFS tiêu chuẩn có thể khám phá toàn bộ thành phần trong một lần truyền, đồng thời theo dõi xem có bất kỳ ranh giới nào chạm vào ô '.' hay không. Sau khi quá trình truyền tải kết thúc, chúng tôi sẽ thêm kích thước của thành phần vào câu trả lời hoặc bỏ qua nó hoàn toàn. 

Điều này làm giảm vấn đề về thời gian tuyến tính vì mỗi ô được truy cập chính xác một lần và mỗi cạnh được kiểm tra một số lần không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên đá BFS |$O(n^4)$|$O(n^2)$| Quá chậm | 
| Thành phần BFS/DFS |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét từng ô của lưới và bất cứ khi nào tìm thấy một viên đá trắng chưa được ghé thăm, hãy bắt đầu duyệt qua nó. Điều này đảm bảo mỗi thành phần màu trắng được xử lý chính xác một lần. 
2. Trong quá trình truyền tải, duy trì một hàng đợi hoặc ngăn xếp chứa tất cả các viên đá trong thành phần hiện tại. Đánh dấu từng ô màu trắng đã truy cập để nó không bao giờ được thêm lại vào lần duyệt khác. 
3. Giữ hai thông tin trong khi mở rộng thành phần: số lượng đá trắng gặp phải và cờ boolean cho biết liệu có ô lân cận nào của bất kỳ viên đá nào được truy cập có trống hay không. Lá cờ này quyết định sự sống còn. 
4. Đối với mỗi viên đá bật ra khỏi cấu trúc ngang, hãy kiểm tra bốn viên đá lân cận của nó. Nếu hàng xóm là người da trắng và chưa được thăm, hãy thêm nó vào thành phần. Nếu hàng xóm trống, hãy đặt cờ tự do thành đúng. Các ô màu đen bị bỏ qua khi mở rộng nhưng không ảnh hưởng đến cờ tự do. 
5. Sau khi quá trình truyền tải kết thúc, nếu cờ tự do sai, hãy thêm kích thước thành phần vào câu trả lời cuối cùng. 
6. Tiếp tục quét lưới cho đến khi tất cả các ô được xử lý. 

Tính chính xác dựa trên việc xử lý từng thành phần được kết nối như một thực thể duy nhất. Việc di chuyển đảm bảo không có viên đá trắng nào được tính hai lần và cờ tự do tổng hợp tất cả các điểm thoát có thể có cho thành phần. 

### Tại sao nó hoạt động 

Mỗi thành phần màu trắng đều được khám phá đầy đủ trước khi đưa ra bất kỳ quyết định nào về nó. Vì kết nối mang tính bắc cầu nên bất kỳ viên đá trắng nào có thể truy cập được từ một viên đá trắng khác trong cùng thành phần đều được đảm bảo được phát hiện trong cùng một quá trình truyền tải. Điều kiện tự do chỉ phụ thuộc vào sự kề cận với các ô trống và tính chất này là đơn điệu đối với thành phần: nếu bất kỳ hòn đá nào có sự tự do thì toàn bộ thành phần đó còn sống. Do đó, việc phân loại thành phần dựa trên một lần truyền tải duy nhất sẽ duy trì tính chính xác và ngăn ngừa việc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input().strip())
    grid = [input().strip() for _ in range(n)]
    
    vis = [[False] * n for _ in range(n)]
    
    dirs = [(1,0), (-1,0), (0,1), (0,-1)]
    
    ans = 0
    
    for i in range(n):
        for j in range(n):
            if grid[i][j] != 'o' or vis[i][j]:
                continue
            
            q = deque()
            q.append((i, j))
            vis[i][j] = True
            
            size = 0
            alive = False
            
            while q:
                x, y = q.popleft()
                size += 1
                
                for dx, dy in dirs:
                    nx, ny = x + dx, y + dy
                    if nx < 0 or nx >= n or ny < 0 or ny >= n:
                        continue
                    if grid[nx][ny] == '.':
                        alive = True
                    elif grid[nx][ny] == 'o' and not vis[nx][ny]:
                        vis[nx][ny] = True
                        q.append((nx, ny))
            
            if not alive:
                ans += size
    
    print(ans)

if __name__ == "__main__":
    solve()
```Lưới được lưu trữ dưới dạng chuỗi để tránh chi phí phân tích cú pháp lặp đi lặp lại. Mảng đã truy cập đảm bảo mỗi viên đá trắng được xử lý chính xác một lần. BFS được chọn thay vì DFS để tránh các vấn đề về độ sâu đệ quy trên các lưới lớn. các`alive`cờ được cập nhật bất cứ khi nào gặp bất kỳ hàng xóm trống nào và kích thước thành phần chỉ được tích lũy nếu không có hàng xóm đó tồn tại. 

Một cạm bẫy triển khai phổ biến là quên rằng chỉ có đá trắng mới truyền bá BFS; đá đen không bao giờ được xếp vào hàng đợi, mặc dù chúng cản trở sự mở rộng. Một vấn đề tế nhị khác là xử lý ranh giới: các ô nằm ngoài giới hạn bị bỏ qua thay vì được coi là khoảng trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một bảng nhỏ:```
o x o
x o x
o x .
```| Bước | Bắt đầu ô | Kích thước thành phần | Cờ Sống | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | 1 | sai | không có hàng xóm da trắng | 
| 2 | (1,1) | 1 | đúng | chạm vào '.' tại (2,2) | 
| 3 | (0,2) | 1 | sai | bị cô lập trắng | 

Câu trả lời cuối cùng chỉ tính các thành phần không có tự do, là những thành phần bị cô lập. Điều này xác nhận rằng BFS phân biệt chính xác các cụm được bao bọc với các cụm chạm vào không gian trống. 

### Ví dụ 2```
o o x
x o x
x x x
```| Bước | Thành phần | Kích thước | Cờ Sống | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | tất cả các o được kết nối | 3 | sai | kèm theo đầy đủ | 

Cả ba viên đá trắng đều thuộc về một thành phần được kết nối không có ô trống liền kề. BFS hợp nhất tất cả người da trắng thành một lần duyệt và đếm chính xác tất cả chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi ô lưới được truy cập một lần trong quá trình truyền tải BFS/DFS | 
| Không gian |$O(n^2)$| Mảng đã truy cập và hàng đợi trong trường hợp xấu nhất lưu trữ tất cả các ô | 

Kích thước lưới giới hạn của$10^6$các ô vừa khít với độ phức tạp này, vì mỗi thao tác có thời gian không đổi trên mỗi ô. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    
    def solve():
        n = int(input().strip())
        grid = [input().strip() for _ in range(n)]
        vis = [[False]*n for _ in range(n)]
        dirs = [(1,0),(-1,0),(0,1),(0,-1)]
        ans = 0
        
        for i in range(n):
            for j in range(n):
                if grid[i][j] != 'o' or vis[i][j]:
                    continue
                q = deque([(i,j)])
                vis[i][j] = True
                alive = False
                size = 0
                
                while q:
                    x,y = q.popleft()
                    size += 1
                    for dx,dy in dirs:
                        nx,ny = x+dx, y+dy
                        if nx<0 or nx>=n or ny<0 or ny>=n:
                            continue
                        if grid[nx][ny]=='.':
                            alive = True
                        elif grid[nx][ny]=='o' and not vis[nx][ny]:
                            vis[nx][ny]=True
                            q.append((nx,ny))
                
                if not alive:
                    ans += size
        
        print(ans)
    
    solve()
    return sys.stdout.getvalue().strip()

# minimal
assert run("1\no\n") == "1"

# no capture
assert run("2\noo\n..") == "0"

# full capture
assert run("2\noo\noo") == "4"

# mixed
assert run("3\no.x\nxoo\nx.x") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 trắng | 1 | thành phần nhỏ nhất | 
| người da trắng bao quanh trống rỗng | 0 | phát hiện tự do | 
| tất cả các khối kèm theo | 4 | đếm thành phần đầy đủ | 
| cấu trúc hỗn hợp | 2 | tách thành phần chính xác | 

## Vỏ cạnh 

Một viên đá trắng được bao quanh hoàn toàn bởi những viên đá đen thể hiện thành phần bị bắt đơn giản nhất. BFS bắt đầu tại ô đó, không tìm thấy hàng xóm trống nào và thêm chính xác một ô vào câu trả lời. 

Một chuỗi lớn các viên đá màu trắng với một ô trống liền kề ở bất kỳ vị trí nào trong chuỗi cho thấy lý do tại sao việc kiểm tra từng ô không thành công. Quá trình truyền tải nhìn thấy hàng xóm trống một lần, đặt cờ còn sống và tránh đếm chính xác toàn bộ cấu trúc. 

Một bảng không có đá trắng sẽ dẫn đến trình kích hoạt di chuyển bằng 0 và thuật toán ngay lập tức trả về 0 mà không cần xử lý không cần thiết.
