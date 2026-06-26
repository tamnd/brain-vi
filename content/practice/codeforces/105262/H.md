---
title: "CF 105262H - Cappuccino Nóng"
description: "Chúng ta được cung cấp một lưới biểu thị một thành phố được chia thành n x m khối. Mỗi khối có thể có một quán cà phê cung cấp cà phê cappuccino, sô cô la nóng, cả hai loại đồ uống hoặc không có gì."
date: "2026-06-24T02:34:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "H"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 56
verified: true
draft: false
---

[CF 105262H - Cappuccino nóng](https://codeforces.com/problemset/problem/105262/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới biểu thị một thành phố được chia thành n x m khối. Mỗi khối có thể có một quán cà phê cung cấp cà phê cappuccino, sô cô la nóng, cả hai loại đồ uống hoặc không có gì. Hai người bắt đầu ở các góc đối diện của lưới này: một người bắt đầu ở ô trên cùng bên trái (1, 1) và người kia bắt đầu ở ô dưới cùng bên phải (n, m). 

Cả hai đều có thể di chuyển từng bước một theo bốn hướng chính. Mỗi lần di chuyển thường tốn 1 đơn vị công sức, nhưng có một quy tắc đặc biệt có thể giảm chi phí này tùy thuộc vào loại quán cà phê trên ô hiện tại. Nếu một người đang đứng trên ô phục vụ đồ uống ưa thích của họ thì họ có thể di chuyển đến bất kỳ ô nào liền kề mà không tốn công sức cho việc di chuyển đó. Ngược lại, chi phí di chuyển 1. 

Người đầu tiên thích sôcôla nóng, vì vậy các tế bào chứa loại 2 hoặc 3 đóng vai trò là “nguồn di chuyển tự do” cho họ. Người thứ hai thích cappuccino, vì vậy các tế bào chứa loại 1 hoặc 3 là nguồn di chuyển tự do của họ. 

Mục tiêu là tính toán tổng công sức tối thiểu mà cả hai người phải bỏ ra để có thể đến cùng một ô ở đâu đó trong lưới. 

Ràng buộc n · m ≤ 10^6 trong tất cả các trường hợp thử nghiệm có nghĩa là tổng số ô lưới trên tất cả các đầu vào có kích thước tuyến tính, do đó mọi giải pháp đều phải gần với O(nm) hoặc O(nm log nm). Bất cứ điều gì như khám phá trạng thái theo cặp đối với cả hai người cùng một lúc, có quy mô là (nm)^2 hoặc tệ hơn, đều không thể thực hiện được trong giới hạn. 

Một ý tưởng ngây thơ nhưng hấp dẫn là mô phỏng cả hai chuyển động cùng nhau, theo dõi vị trí của cả hai người. Điều đó dẫn đến một không gian trạng thái có kích thước nm × nm, vượt xa khả năng thực hiện. 

Một trường hợp thất bại tinh vi hơn sẽ phát sinh nếu một người cố gắng tham lam gặp nhau trong ô “nhìn gần nhất” về mặt hình học. Ví dụ: một ô gần trung tâm có thể không tối ưu nếu một trong hai ô không thể truy cập được với giá rẻ do thiếu vùng di chuyển tự do gần đó. Điểm gặp mặt tối ưu phụ thuộc vào cấu trúc đường đi ngắn nhất chứ không phải khoảng cách Manhattan. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực của vấn đề coi nó như một vấn đề về đường đi ngắn nhất của hai tác nhân. Chúng tôi sẽ theo dõi trạng thái (x1, y1, x2, y2) và mỗi bước sẽ di chuyển một hoặc cả hai người chơi tùy thuộc vào quy tắc đồng bộ hóa. Ngay cả khi chúng ta đơn giản hóa việc chuyển động sang các đường dẫn độc lập, việc thử tất cả các điểm gặp nhau vẫn yêu cầu tính toán các đường đi ngắn nhất từ ​​cả hai điểm xuất phát đến mọi ô. Nếu không tối ưu hóa, việc chạy tìm kiếm đường dẫn ngắn nhất trên mỗi ô hoặc trên mỗi cặp sẽ dẫn đến hành vi gần như O((nm)^2) trong trường hợp xấu nhất, vì mỗi bản mở rộng BFS hoặc Dijkstra sẽ truy cập lại hầu hết lưới. 

Điều quan trọng nhất là hai người không tương tác với nhau cho đến điểm gặp gỡ cuối cùng. Đường đi của chúng độc lập và cấu trúc chi phí giống hệt nhau về hình thức cho cả hai, chỉ khác nhau ở chỗ các ô cung cấp chuyển đổi không tốn phí. Điều này có nghĩa là chúng tôi có thể tính toán trước chi phí tối thiểu để tiếp cận từng ô từ từng điểm bắt đầu một cách riêng biệt. Khi chúng ta có hai lưới khoảng cách này, câu trả lời chỉ đơn giản là giá trị tối thiểu trên tất cả các ô trong tổng của chúng. 

Cấu trúc của hàm chi phí là rất quan trọng. Di chuyển ra khỏi ô là miễn phí hoặc tốn 1 chi phí chỉ tùy thuộc vào loại ô. Đây chính xác là bài toán đường đi ngắn nhất trên biểu đồ lưới có trọng số cạnh trong {0, 1}, có thể được giải một cách hiệu quả bằng cách sử dụng BFS 0-1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái chung | O((nm)^2) | O((nm)^2) | Quá chậm | 
| Hai lần chạy BFS 0-1 độc lập | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tính toán hai lưới khoảng cách riêng biệt, một lưới cho mỗi người.

1. Xây dựng lưới khoảng cách cho người đầu tiên bắt đầu từ (1, 1). Khởi tạo tất cả khoảng cách thành giá trị lớn và đặt ô bắt đầu thành 0. 
2. Chạy BFS 0-1 từ (1, 1). Khi mở rộng từ một ô, hãy xác định xem việc di chuyển sang ô lân cận tốn 0 hay 1. Chi phí chỉ phụ thuộc vào việc ô hiện tại có chứa sôcôla nóng hay không (loại 2 hay 3). Nếu có, tất cả các nước đi đều miễn phí; mặt khác, chúng có giá 1. Cập nhật khoảng cách bằng cách sử dụng deque: đẩy về phía trước để có các cạnh có giá 0 và về phía sau để có các cạnh có giá 1. 
3. Lặp lại quy trình tương tự cho người thứ hai bắt đầu từ (n, m), nhưng bây giờ điều kiện di chuyển tự do phụ thuộc vào tính sẵn có của cappuccino (loại 1 hoặc 3). 
4. Sau khi tính cả hai lưới khoảng cách, lặp lại từng ô trong lưới và tính distA[i][j] + distB[i][j]. Theo dõi số tiền tối thiểu. 
5. Trả lại kết quả tối thiểu này. 

Điều tinh tế duy nhất là điều kiện “di chuyển tự do” áp dụng cho ô bạn đang rời khỏi chứ không phải ô bạn đang nhập. Điều này làm cho cạnh biểu đồ chỉ phụ thuộc vào nút nguồn, đây chính xác là điều cho phép 0-1 BFS hoạt động rõ ràng. 

### Tại sao nó hoạt động 

Mỗi trạng thái trong BFS thể hiện việc ở một ô cụ thể với chi phí tối thiểu đã biết để tiếp cận nó. Vì trọng số cạnh chỉ là 0 hoặc 1 và chỉ phụ thuộc vào loại ô hiện tại nên thuộc tính đường dẫn ngắn nhất được giữ nguyên theo thứ tự BFS 0-1 tiêu chuẩn. Deque đảm bảo rằng mọi trạng thái đạt được với chi phí 0 đều được xử lý trước khi đạt đến trạng thái có chi phí 1, duy trì thứ tự thăm dò không giảm chính xác. Vì cả hai tác nhân đều tính toán độc lập chi phí đường đi ngắn nhất thực sự đến từng ô, nên bất kỳ điểm gặp gỡ nào cũng kết hợp hai đường dẫn độc lập tối ưu và việc lấy mức tối thiểu trên tất cả các ô sẽ đảm bảo tính tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

INF = 10**18

def bfs(grid, n, m, sx, sy, good_set):
    dist = [[INF] * m for _ in range(n)]
    dq = deque()
    dist[sx][sy] = 0
    dq.append((sx, sy))

    while dq:
        x, y = dq.popleft()
        d = dist[x][y]

        # determine cost of moving out of (x, y)
        w = 0 if grid[x][y] in good_set else 1

        for dx, dy in ((1,0), (-1,0), (0,1), (0,-1)):
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m:
                nd = d + w
                if nd < dist[nx][ny]:
                    dist[nx][ny] = nd
                    if w == 0:
                        dq.appendleft((nx, ny))
                    else:
                        dq.append((nx, ny))
    return dist

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]

        dist1 = bfs(grid, n, m, 0, 0, {2, 3})
        dist2 = bfs(grid, n, m, n - 1, m - 1, {1, 3})

        ans = INF
        for i in range(n):
            row1 = dist1[i]
            row2 = dist2[i]
            for j in range(m):
                ans = min(ans, row1[j] + row2[j])

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp này tách biệt hoàn toàn hai hành khách và giảm bài toán xuống còn hai phép tính đường đi ngắn nhất. Mỗi BFS sử dụng một deque để duy trì thứ tự BFS 0-1, đảm bảo độ phức tạp tuyến tính về số cạnh lưới. 

Một cạm bẫy triển khai phổ biến là áp dụng không chính xác điều kiện đồ uống cho ô đích thay vì ô nguồn. Điều đó làm thay đổi mô hình biểu đồ và phá vỡ độ chính xác 0-1 BFS, vì trọng số cạnh sẽ không còn nhất quán trên mỗi trạng thái gửi đi. 

Một điểm tinh tế khác là bố cục bộ nhớ: việc lưu trữ toàn bộ lưới cho cả hai khoảng cách là cần thiết nhưng vẫn an toàn vì tổng số ô trong các lần kiểm tra được giới hạn bởi 10^6. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ trong đó một hoặc hai ô cung cấp cơ hội di chuyển tự do. 

### Ví dụ 1 

Lưới:```
2 0
0 1
```Ở đây, người đầu tiên được hưởng lợi từ các tế bào sô cô la nóng (2 hoặc 3) và người thứ hai từ các tế bào cappuccino (1 hoặc 3). 

Chúng tôi tính toán cả hai bản đồ khoảng cách. 

| Bước | Tế bào | Quận 1 (từ 1,1) | Quận 2 (từ 2,2) | 
| --- | --- | --- | --- | 
| ban đầu | (1,1) / (2,2) | 0/INF | INF/0 | 
| BFS1 | khai triển (1,1) | hàng xóm cập nhật | | 
| BFS2 | khai triển (2,2) | | hàng xóm cập nhật | 

Sau khi chạy cả hai BFS, chúng tôi kiểm tra tất cả các ô và chọn tổng tối thiểu dist1 + dist2. Điểm gặp gỡ xuất hiện một cách tự nhiên như một tế bào nơi cả hai có thể đến với giá rẻ, không nhất thiết phải là trung tâm về mặt hình học. 

Điều này cho thấy rằng các đường dẫn ngắn nhất độc lập là đủ và không cần logic đồng bộ hóa. 

### Ví dụ 2 

Lưới:```
3 3 3
0 0 0
1 0 2
```Hàng trên cùng cuối cùng cho phép cả hai khách du lịch di chuyển tự do (vì hàng 3 giúp ích cho cả hai), tạo ra một hành lang chuyển tiếp không tốn phí. 

BFS từ mỗi góc nhanh chóng lan truyền dọc theo hành lang đó với việc mở rộng với chi phí bằng 0, xác nhận rằng khi đạt được một ô tốt, các phần lớn của lưới sẽ trở nên rẻ khi đi qua. 

Mức tối thiểu cuối cùng xảy ra tại một ô có thể tiếp cận được thông qua các vùng chi phí thấp chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) mỗi lần kiểm tra | Mỗi ô được xử lý một số lần không đổi trong hai lần chạy BFS 0-1 | 
| Không gian | O(nm) | Hai lưới khoảng cách cộng với bộ nhớ deque | 

Vì tổng số ô trên tất cả các trường hợp thử nghiệm tối đa là 10^6 nên giải pháp này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    from collections import deque

    INF = 10**18

    def bfs(grid, n, m, sx, sy, good_set):
        dist = [[INF] * m for _ in range(n)]
        dq = deque()
        dist[sx][sy] = 0
        dq.append((sx, sy))

        while dq:
            x, y = dq.popleft()
            d = dist[x][y]
            w = 0 if grid[x][y] in good_set else 1
            for dx, dy in ((1,0),(-1,0),(0,1),(0,-1)):
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < m:
                    nd = d + w
                    if nd < dist[nx][ny]:
                        dist[nx][ny] = nd
                        if w == 0:
                            dq.appendleft((nx, ny))
                        else:
                            dq.append((nx, ny))
        return dist

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]

        dist1 = bfs(grid, n, m, 0, 0, {2,3})
        dist2 = bfs(grid, n, m, n-1, m-1, {1,3})

        INF = 10**18
        ans = INF
        for i in range(n):
            for j in range(m):
                ans = min(ans, dist1[i][j] + dist2[i][j])

        out.append(str(ans))

    return "\n".join(out)

# provided sample placeholders (not real strings here)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 với cả hai đồ uống | 0 | cả hai bắt đầu gặp nhau ngay lập tức | 
| lưới tất cả số không | cuộc họp ngắn nhất ở Manhattan | không có hiệu ứng cạnh tự do | 
| lưới đầy 3s | tuyên truyền không tốn phí | cả BFS đều trở nên tầm thường | 
| hành lang hẹp | ràng buộc đường dẫn | Tính chính xác của BFS trong tình trạng tắc nghẽn | 

## Vỏ cạnh 

Lưới đơn ô là trường hợp ranh giới rõ ràng nhất. Cả hai người đều bắt đầu và kết thúc trên cùng một ô, vì vậy câu trả lời phải bằng 0 bất kể loại ô nào. BFS khởi tạo ô bắt đầu một cách chính xác, do đó cả hai mảng khoảng cách đều chứa số 0 tại vị trí đó và tổng tối thiểu bằng 0. 

Một lưới không có ô đặc biệt (tất cả đều bằng 0) sẽ giảm bài toán thành hai phép tính đường đi ngắn nhất độc lập với các cạnh chi phí đồng nhất. BFS thoái hóa thành đường đi ngắn nhất gồm nhiều bước tiêu chuẩn và cả hai khoảng cách đều phản ánh chi phí di chuyển giống như Manhattan tùy thuộc vào chướng ngại vật, đảm bảo tính chính xác mà không cần dựa vào bất kỳ chuyển đổi tự do nào. 

Một lưới đầy đủ các ô loại 3 tạo ra sự tự do tối đa. Mọi ô đều cho phép di chuyển với chi phí bằng 0, vì vậy cả hai lần chạy BFS đều truyền ô bắt đầu với chi phí bằng 0 cho toàn bộ lưới. Mỗi ô sẽ trở thành một điểm gặp mặt hợp lệ với tổng chi phí bằng 0 và thuật toán trả về 0 một cách chính xác bằng cách đánh giá tất cả các ô có thể gặp nhau.
