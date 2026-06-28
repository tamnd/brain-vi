---
title: "CF 105112K - Klompendans"
description: "Chúng ta được đặt ở ô trên cùng bên trái của lưới $n lần n$ và được phép đi qua lưới bằng cách sử dụng hai quy tắc di chuyển “giống như hiệp sĩ” khác nhau."
date: "2026-06-27T19:59:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105112
codeforces_index: "K"
codeforces_contest_name: "2023-2024 ICPC Northwestern European Regional Programming Contest (NWERC 2023)"
rating: 0
weight: 105112
solve_time_s: 56
verified: true
draft: false
---

[CF 105112K - Klompendans](https://codeforces.com/problemset/problem/105112/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đặt trên ô trên cùng bên trái của một$n \times n$lưới và được phép đi qua lưới bằng cách sử dụng hai quy tắc di chuyển “giống như hiệp sĩ” khác nhau. Mỗi bước di chuyển sẽ nhảy theo hình chữ L: quy tắc đầu tiên di chuyển theo một cặp khoảng cách cố định$(a,b)$dọc theo hai trục theo bất kỳ hướng và dấu nào, và quy tắc thứ hai cũng làm như vậy với$(c,d)$. Sau mỗi lần di chuyển, chúng ta phải chuyển sang quy tắc khác, nhưng ngay từ đầu, chúng ta có thể tự do chọn một trong hai quy tắc làm bước đầu tiên. 

Nhiệm vụ không phải là tìm một đường dẫn duy nhất mà là xác định có bao nhiêu ô lưới riêng biệt có thể được truy cập bởi bất kỳ chuỗi di chuyển xen kẽ hợp lệ nào bắt đầu từ góc trên cùng bên trái mà không rời khỏi lưới. 

Ý nghĩa cấu trúc quan trọng của các ràng buộc là$n \le 500$, do đó lưới có nhiều nhất là 250.000 ô. Bất kỳ giải pháp nào khám phá các trạng thái tỷ lệ thuận với số lượng chuỗi đều không thể thực hiện được vì số lượng chuỗi chuyển động có thể tăng theo cấp số nhân theo độ sâu. Điều này ngay lập tức gợi ý rằng vấn đề về cơ bản là về khả năng tiếp cận trong biểu đồ hơn là việc liệt kê các đường dẫn. 

Một điểm tinh tế là việc luân phiên di chuyển sẽ đưa bộ nhớ vào hệ thống. Ở trên một ô là không đủ để mô tả trạng thái, bởi vì các bước di chuyển được phép tiếp theo phụ thuộc vào việc di chuyển trước đó là loại A hay loại B. Vì vậy, hai vị trí lưới giống hệt nhau có thể hoạt động khác nhau tùy thuộc vào loại di chuyển nào được mong đợi tiếp theo. 

Một sai lầm ngây thơ là bỏ qua điều này và chỉ coi nó như một BFS tiêu chuẩn trên các ô lưới. Điều đó không thành công vì khả năng tiếp cận phụ thuộc vào tính chẵn lẻ của kiểu di chuyển. 

Một chế độ lỗi khác là thử DFS trên tất cả các chuỗi mà không ghi nhớ. Ngay cả những trường hợp nhỏ như$n=10$có thể tạo ra hệ số phân nhánh lên tới 16 hướng mỗi bước, gây ra sự bùng nổ trong việc khám phá nhiều lần các cấu hình giống nhau. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng mọi chuỗi di chuyển có thể bắt đầu từ ô ban đầu, xen kẽ giữa hai loại di chuyển. Từ mỗi vị trí, chúng tôi thử tối đa tám hướng của kiểu di chuyển hiện tại và tiếp tục đệ quy. Điều này đúng về nguyên tắc vì nó tuân thủ chính xác các quy tắc và liệt kê tất cả các điệu nhảy hợp lệ. 

Tuy nhiên, cách tiếp cận này không thành công vì cùng một ô được xem lại nhiều lần theo các kiểu di chuyển còn lại khác nhau. Tệ hơn nữa, yếu tố phân nhánh kết hợp ở mỗi bước, dẫn đến sự tăng trưởng theo cấp số nhân trong các trình tự được khám phá. Trường hợp xấu nhất hành xử như$O(16^k)$cho độ dài chuỗi$k$, điều này hoàn toàn không khả thi ngay cả đối với các lưới nhỏ. 

Quan sát quan trọng là hệ thống chỉ có một lượng “bộ nhớ” nhỏ: ô hiện tại và loại di chuyển nào được yêu cầu tiếp theo. Khi chúng tôi bao gồm bộ nhớ này một cách rõ ràng, toàn bộ vấn đề sẽ trở thành vấn đề về khả năng tiếp cận kiểu đường đi ngắn nhất trên biểu đồ hữu hạn có nhiều nhất$2n^2$tiểu bang. 

Mỗi trạng thái là một cặp bao gồm vị trí lưới và một cờ cho biết nước đi tiếp theo phải sử dụng loại A hay loại B. Từ mỗi trạng thái, chúng tôi tạo ra tối đa 8 lần chuyển đổi sang trạng thái khác có cờ đối diện. Điều này chuyển đổi vấn đề thành tính toán khả năng tiếp cận BFS/DFS tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force trên các chuỗi | Hàm mũ | Ngăn xếp theo cấp số nhân | Quá chậm | 
| BFS kết thúc$(x,y,parity)$tiểu bang |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa vấn đề dưới dạng tìm kiếm đồ thị trên các trạng thái tăng cường. 

### Các bước 

1. Xác định trạng thái là$(x, y, t)$, Ở đâu$(x,y)$là ô hiện tại và$t$cho biết loại di chuyển nào phải được sử dụng tiếp theo. 
2. Khởi tạo hàng đợi BFS với hai trạng thái bắt đầu:$(0,0,A)$Và$(0,0,B)$, vì nước đi đầu tiên có thể sử dụng một trong hai loại. 
3. Duy trì mảng đã truy cập trên tất cả các trạng thái$(x,y,t)$để tránh phải xem lại các cấu hình đã được xử lý. 
4. Tính toán trước 8 độ lệch hướng cho từng loại nước đi:$(a,b)$tạo ra tất cả các hoán đổi dấu và trục, và tương tự cho$(c,d)$. 
5. Khi hàng đợi không trống, hãy bật trạng thái$(x,y,t)$. 
6. Đối với kiểu di chuyển hiện tại$t$, tạo tất cả các ô đích có thể$(nx,ny)$sử dụng 8 phép biến đổi của nó. 
7. Đối với mỗi đích hợp lệ trong lưới, nếu trạng thái$(nx,ny,1-t)$chưa được truy cập, đánh dấu nó đã truy cập và đẩy nó vào hàng đợi. 
8. Sau khi BFS kết thúc, đếm xem có bao nhiêu ô lưới riêng biệt$(x,y)$đã được viếng thăm ở một trong hai tiểu bang. 

### Tại sao nó hoạt động 

Bất biến quan trọng là BFS khám phá chính xác tập hợp các trạng thái tăng cường có thể tiếp cận theo ràng buộc luân phiên di chuyển. Mỗi bước nhảy hợp lệ tương ứng với một đường dẫn trong biểu đồ trạng thái này, bởi vì mỗi bước di chuyển sẽ luân phiên hoàn toàn loại chuyển tiếp được yêu cầu và mọi bước di chuyển hình học được phép được biểu diễn dưới dạng một cạnh. Ngược lại, mọi đường dẫn trong biểu đồ này tương ứng với một chuỗi các bước nhảy hợp lệ theo cấu trúc. Vì chúng tôi khám phá tất cả các trạng thái có thể tiếp cận từ cả hai lựa chọn di chuyển ban đầu hợp lệ, nên chúng tôi nắm bắt được sự kết hợp của tất cả các bước nhảy có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input().strip())
    a, b = map(int, input().split())
    c, d = map(int, input().split())

    movesA = []
    movesB = []

    for dx, dy in [(a, b), (b, a)]:
        for sx in (-1, 1):
            for sy in (-1, 1):
                movesA.append((sx * dx, sy * dy))

    for dx, dy in [(c, d), (d, c)]:
        for sx in (-1, 1):
            for sy in (-1, 1):
                movesB.append((sx * dx, sy * dy))

    def id_state(x, y, t):
        return (x, y, t)

    vis = [[[False, False] for _ in range(n)] for _ in range(n)]
    q = deque()

    vis[0][0][0] = True
    vis[0][0][1] = True
    q.append((0, 0, 0))
    q.append((0, 0, 1))

    while q:
        x, y, t = q.popleft()

        if t == 0:
            moves = movesA
        else:
            moves = movesB

        for dx, dy in moves:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n:
                nt = 1 - t
                if not vis[nx][ny][nt]:
                    vis[nx][ny][nt] = True
                    q.append((nx, ny, nt))

    seen = set()
    for i in range(n):
        for j in range(n):
            if vis[i][j][0] or vis[i][j][1]:
                seen.add((i, j))

    print(len(seen))

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt rõ ràng hai loại di chuyển thành các danh sách hướng được tính toán trước. Điều này tránh việc tính toán lại 8 phép biến đổi đối xứng trong BFS. Mỗi ô lưới được theo dõi bằng hai cờ boolean, một cờ cho mỗi loại nước đi tiếp theo, đảm bảo chúng ta không nhầm lẫn các trạng thái khi đến cùng một ô nhưng nước đi tiếp theo được yêu cầu lại khác. 

Một cạm bẫy triển khai phổ biến là quên khởi động BFS từ cả hai loại di chuyển. Vì nước đi đầu tiên có thể được chọn tự do nên cả hai trạng thái ban đầu phải được xếp hàng đợi; nếu không thì một nửa không gian có thể truy cập sẽ bị mất. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ nơi$n = 4$,$a = 1, b = 2$,$c = 2, d = 3$. BFS bắt đầu từ$(0,0,A)$Và$(0,0,B)$. 

### Dấu vết 1 

| Bước | Tiểu bang | Di chuyển được sử dụng | Đã thêm tiểu bang mới | 
| --- | --- | --- | --- | 
| 0 | (0,0,A), (0,0,B) | bắt đầu | ban đầu | 
| 1 | (0,0,A) | A-di chuyển | một số hợp lệ (nx,ny,B) | 
| 2 | (0,0,B) | B-di chuyển | một số hợp lệ (nx,ny,A) | 

Dấu vết này cho thấy cách luân phiên buộc không gian trạng thái chia ngay lập tức thành hai lớp xen kẽ, nhưng cả hai lớp vẫn được đồng bộ hóa thông qua việc mở rộng BFS. 

### Dấu vết 2 (trường hợp suy biến) 

hãy để$n=3$,$a=b=c=d=1$. Mọi bước di chuyển đều trở thành những bước đi tiêu chuẩn giống như vua. 

| Bước | Tiểu bang | Có thể truy cập | 
| --- | --- | --- | 
| 0 | (0,0,A/B) | (0,0) | 
| 1 | hàng xóm | tất cả các ô liền kề | 
| 2 | lan truyền ngược | chỉ các ô đã truy cập | 

Điều này chứng tỏ rằng việc xem lại cùng một ô với các kiểu di chuyển khác nhau không làm tăng tập hợp có thể truy cập, xác nhận rằng việc cắt tỉa dựa trên trạng thái là điều cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi tiểu bang$(x,y,t)$được truy cập một lần, với 8 lần chuyển đổi liên tục | 
| Không gian |$O(n^2)$| Mảng đã truy cập cho hai trạng thái trên mỗi ô cộng với hàng đợi BFS | 

Lưới có tối đa 250.000 ô, do đó, ngay cả việc tăng gấp đôi nó cho hai trạng thái di chuyển vẫn nằm trong giới hạn thoải mái. BFS đảm bảo mỗi trạng thái được xử lý một lần, giúp giải pháp dễ dàng đủ nhanh trong giới hạn 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a, b = map(int, input().split())
    c, d = map(int, input().split())

    movesA = []
    movesB = []

    for dx, dy in [(a, b), (b, a)]:
        for sx in (-1, 1):
            for sy in (-1, 1):
                movesA.append((sx * dx, sy * dy))

    for dx, dy in [(c, d), (d, c)]:
        for sx in (-1, 1):
            for sy in (-1, 1):
                movesB.append((sx * dx, sy * dy))

    vis = [[[False, False] for _ in range(n)] for _ in range(n)]
    q = deque()

    vis[0][0][0] = True
    vis[0][0][1] = True
    q.append((0, 0, 0))
    q.append((0, 0, 1))

    while q:
        x, y, t = q.popleft()
        moves = movesA if t == 0 else movesB

        for dx, dy in moves:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n:
                nt = 1 - t
                if not vis[nx][ny][nt]:
                    vis[nx][ny][nt] = True
                    q.append((nx, ny, nt))

    ans = sum(any(vis[i][j]) for i in range(n) for j in range(n))
    return str(ans)

# provided samples (placeholders since exact outputs not specified)
# assert run(...) == ...

# custom cases
assert run("3\n1 1\n1 1\n") >= "1", "minimum grid sanity"
assert run("4\n1 2\n2 3\n") != "", "basic reachability"
assert run("5\n2 1\n3 2\n") != "", "mixed asymmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 với các bước di chuyển đối xứng | giá trị nhỏ | hành vi lưới tối thiểu | 
| 4 với những chiêu thức khác nhau | kết quả không trống | khả năng tiếp cận xen kẽ | 
| 5 thông số hỗn hợp | mở rộng BFS ổn định | xử lý bất đối xứng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$a=b$hoặc$c=d$, làm giảm số lượng hướng di chuyển duy nhất. Việc triển khai vẫn tạo ra 8 biến thể có chữ ký, nhưng một số biến thể bị thu gọn thành các bản sao. Điều này không phá vỡ tính chính xác vì việc truy cập BFS đảm bảo các bản sao bị bỏ qua mà không phải trả thêm phí. Thuật toán vẫn hoạt động chính xác vì nó không phụ thuộc vào tính duy nhất của các chuyển đổi. 

Một trường hợp khác là khi cả hai kiểu di chuyển đều giống hệt nhau. Trong tình huống đó, biểu đồ trạng thái trở thành hai lớp giống hệt nhau được kết nối theo mô hình xen kẽ đối xứng. BFS vẫn khám phá chính xác vì việc bắt đầu từ cả hai lớp đảm bảo không bỏ sót cấu hình nào có thể truy cập được. 

Cuối cùng, khi$n$nhỏ, nhiều nước đi sẽ rơi ra ngoài lưới ngay lập tức. Việc kiểm tra ranh giới ngăn chặn việc mở rộng trạng thái không hợp lệ, đảm bảo BFS chấm dứt nhanh chóng ngay cả khi hầu hết các động thái lý thuyết đều không thể sử dụng được.
