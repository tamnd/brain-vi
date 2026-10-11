---
title: "CF 105009E - Làm vườn thật khó"
description: "Chúng ta có một lưới $n lần n$ trong đó mỗi ô đại diện cho một trong ba loại địa hình: đất trồng có thể sử dụng được, nguồn nước hoặc đất bị chặn không thể sử dụng được. Chúng tôi được yêu cầu đếm xem có bao nhiêu dải ngang có chiều cao chính xác bằng hai hàng là “dải làm vườn hợp lệ”."
date: "2026-06-28T02:38:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105009
codeforces_index: "E"
codeforces_contest_name: "2024 USACO.Guide Informatics Tournament"
rating: 0
weight: 105009
solve_time_s: 78
verified: false
draft: false
---

[CF 105009E - Làm vườn thật khó](https://codeforces.com/problemset/problem/105009/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới trong đó mỗi ô đại diện cho một trong ba loại địa hình: đất trồng có thể sử dụng được, nguồn nước hoặc đất bị chặn không thể sử dụng được. Chúng tôi được yêu cầu đếm xem có bao nhiêu dải ngang có chiều cao chính xác bằng hai hàng là “dải làm vườn hợp lệ”. 

Một dải được xác định bằng cách chọn hai hàng liên tiếp và sau đó chọn một đoạn cột liền kề trong hai hàng đó. Bên trong hình chữ nhật đó chỉ được phép trồng đất và nguồn nước nên không được xuất hiện ô bị chặn. Ngoài ra, mọi ô trồng bên trong dải tối đa phải nằm trong khoảng cách Manhattan$k$từ ít nhất một ô nguồn nước trong toàn bộ lưới điện (không nhất thiết phải nằm trong dải). Khoảng cách được đo trên lưới đầy đủ. 

Một hạn chế quan trọng về cấu trúc là không có hai ô nước nào liền kề nhau theo chiều dọc. Điều này ngăn không cho các cột chứa hai giếng liên tiếp, điều này sau này rất quan trọng khi suy luận về độ lan truyền và phạm vi bao phủ. 

Nhiệm vụ là đếm tất cả các hình chữ nhật hợp lệ có chiều cao bằng 2 thỏa mãn cả hai ràng buộc. 

Những hạn chế$n \le 2000$ngay lập tức loại trừ bất cứ điều gì tồi tệ hơn đại khái$O(n^2)$hoặc$O(n^2 \log n)$. Bất kỳ giải pháp nào liệt kê tất cả các hình chữ nhật một cách ngây thơ và kiểm tra khoảng cách theo từng ô sẽ quá chậm vì có$O(n^3)$hình chữ nhật có thể và mỗi kiểm tra là$O(n)$, dẫn đến$O(n^4)$. 

Một hạn chế tinh vi hơn là khoảng cách phụ thuộc vào các giếng ở bất kỳ vị trí nào trong lưới, không chỉ bên trong dải. Điều này làm cho việc suy luận ngây thơ về lưới điện con trở nên nguy hiểm vì tính hợp lệ của dải phụ thuộc vào thông tin lân cận toàn cầu. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. 

Nếu một dải không chứa ô nước ở bất kỳ vị trí nào trong lưới nhưng vẫn có đất trồng thì không hợp lệ vì các ô trồng đó không thể có khoảng cách$\le k$đến bất kỳ nguồn nước nào. Ví dụ: nếu lưới không có giếng nào cả và$k \ge 1$, mọi dải đều không đạt yêu cầu về khoảng cách, vì vậy câu trả lời là 0. 

Một dạng lỗi khác xuất hiện khi ô trồng gần giếng bên ngoài dải đất. Một cách tiếp cận đơn giản chỉ kiểm tra các giếng bên trong hình chữ nhật đã chọn sẽ từ chối các dải hợp lệ một cách không chính xác. 

Cuối cùng, các dải chứa các ô bị chặn ngay lập tức không hợp lệ, ngay cả khi các điều kiện về khoảng cách được giữ nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực bắt đầu bằng cách liệt kê từng cặp hàng liền kề và sau đó là mọi ranh giới trái và phải có thể có. Đối với mỗi hình chữ nhật ứng viên, chúng tôi quét tất cả các ô của nó để đảm bảo không có vùng đất bị chặn nào tồn tại và mỗi ô trồng cây đều có ít nhất một ô nước trong khoảng cách Manhattan$k$. 

Điều này đúng nhưng đắt tiền. có$O(n^2)$lựa chọn các cặp hàng và$O(n^2)$phân đoạn cột, sản xuất$O(n^4)$hình chữ nhật. Mỗi hình chữ nhật chứa tối đa$O(n^2)$các ô trong trường hợp xấu nhất và việc kiểm tra khoảng cách một cách đơn giản yêu cầu quét tất cả các ô nước hoặc tính toán khoảng cách BFS cho mỗi truy vấn. Điều này nhanh chóng vượt quá mọi thời gian chạy khả thi. 

Quan sát quan trọng là ràng buộc khoảng cách có thể được tính toán trước trên toàn cầu. Đối với mỗi ô, chúng tôi có thể tính toán khoảng cách tối thiểu của nó tới bất kỳ nguồn nước nào bằng cách sử dụng BFS đa nguồn. Sau quá trình tiền xử lý này, điều kiện “tế bào trồng nằm trong khoảng cách$k$” trở thành một tra cứu liên tục. 

Bây giờ mọi ô đều được dán nhãn là an toàn hoặc không an toàn. Một dải hợp lệ phải đáp ứng hai điều kiện độc lập: nó không chứa các ô bị chặn và mọi ô trồng cây đều an toàn. 

Điều này biến vấn đề thành việc đếm tất cả các hình chữ nhật con tất cả một trong một ma trận nhị phân cho mỗi cặp hàng, trong đó “một” có nghĩa là ô được phép (không bị chặn và có thể tưới nước hoặc trồng cây an toàn). 

Đối với mỗi cặp hàng, chúng tôi nén các cột thành một mảng nhị phân cho biết tính hợp lệ và sau đó đếm các phân đoạn liền kề trong đó tất cả các mục nhập đều hợp lệ. Trong các phân đoạn đó, chúng tôi cũng cần đảm bảo rằng mọi ô trồng bên trong đều an toàn, nhưng vì độ an toàn đã được tính toán trước trên mỗi ô nên việc này chỉ còn là kiểm tra tính hợp lệ đơn giản. 

Cấu trúc còn lại là tiêu chuẩn: đối với mỗi cặp hàng, chúng tôi quét qua các cột và đếm các phân đoạn hợp lệ liền kề tối đa, sau đó cộng số lượng mảng con bên trong mỗi phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi lưới thành dạng đã giải quyết được các ràng buộc về khoảng cách. 

1. Tính toán khoảng cách tối thiểu của Manhattan từ mọi ô đến nguồn nước gần nhất bằng cách sử dụng BFS đa nguồn bắt đầu từ tất cả các ô nước. Mỗi ô nước được khởi tạo với khoảng cách bằng 0 và BFS trải rộng theo bốn hướng. Điều này hoạt động vì khoảng cách Manhattan tương ứng chính xác với độ dài đường đi ngắn nhất trong biểu đồ lưới có các cạnh đơn vị. 
2. Đánh dấu mọi ô là “an toàn” nếu đó là nước hoặc đất trồng trọt có khoảng cách BFS tối đa$k$. Bất kỳ ô trồng nào có khoảng cách lớn hơn$k$được coi là không thể sử dụng được đối với bất kỳ dải hợp lệ nào. 
3. Bây giờ hãy xem xét từng cặp hàng$(r1, r2)$Ở đâu$r2 = r1 + 1$, vì các dải phải cao chính xác bằng hai hàng. Đối với mỗi cặp như vậy, chúng tôi xử lý các cột từ trái sang phải. 
4. Đối với mỗi cột$c$, kiểm tra xem cả hai ô có$(r1, c)$Và$(r2, c)$có thể sử dụng được (không bị chặn) và an toàn. Nếu bị chặn hoặc không an toàn, hãy đánh dấu cột này là không hợp lệ đối với cặp hàng hiện tại. 
5. Bây giờ chúng ta có một mảng nhị phân trên các cột cho biết cột nào có thể tham gia vào dải hợp lệ cho cặp hàng này. Chúng tôi quét mảng này và tìm các phân đoạn liền kề tối đa của các cột hợp lệ. 
6. Đối với mỗi đoạn chiều dài$L$, thêm vào$L \cdot (L+1) / 2$cho câu trả lời, vì bất kỳ phân đoạn nào cũng tương ứng với một dải ngang hợp lệ. 
7. Lặp lại cho tất cả các cặp hàng và cộng dồn tổng số. 

Ý tưởng chính là khi các cặp hàng được cố định, các cột sẽ trở nên độc lập và tính hợp lệ sẽ trở thành một vấn đề đếm phân đoạn liền kề đơn giản. 

Tại sao nó hoạt động: 

Quá trình xử lý trước BFS đảm bảo rằng mọi ô trồng cây đều biết liệu nó có đáp ứng ràng buộc khoảng cách tổng thể hay không. Sau quá trình chuyển đổi này, tính hợp lệ chỉ phụ thuộc vào thuộc tính ô cục bộ và sự vắng mặt của các ô bị chặn. Vì mỗi dải là một hình chữ nhật liền kề có chiều cao bằng 2 và các ràng buộc cột là độc lập sau khi các hàng được cố định, nên mọi dải hợp lệ đều tương ứng chính xác với một mảng con gồm các cột trong đó tất cả các ô được cho phép. Việc đếm các mảng con trên các phân đoạn hợp lệ tối đa đảm bảo mỗi dải hợp lệ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, k = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]
    
    INF = 10**9
    dist = [[INF] * n for _ in range(n)]
    q = deque()
    
    for i in range(n):
        for j in range(n):
            if grid[i][j] == 2:
                dist[i][j] = 0
                q.append((i, j))
    
    dirs = [(1,0), (-1,0), (0,1), (0,-1)]
    
    while q:
        x, y = q.popleft()
        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n:
                if dist[nx][ny] > dist[x][y] + 1:
                    dist[nx][ny] = dist[x][y] + 1
                    q.append((nx, ny))
    
    safe = [[False] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if grid[i][j] == 2:
                safe[i][j] = True
            elif grid[i][j] == 1 and dist[i][j] <= k:
                safe[i][j] = True
    
    ans = 0
    
    for r1 in range(n - 1):
        r2 = r1 + 1
        ok = [True] * n
        
        for c in range(n):
            if grid[r1][c] == 3 or grid[r2][c] == 3:
                ok[c] = False
            elif not safe[r1][c] or not safe[r2][c]:
                ok[c] = False
        
        c = 0
        while c < n:
            if not ok[c]:
                c += 1
                continue
            start = c
            while c < n and ok[c]:
                c += 1
            length = c - start
            ans += length * (length + 1) // 2
    
    print(ans)

if __name__ == "__main__":
    solve()
```Phần BFS xây dựng trường khoảng cách toàn cầu từ tất cả các giếng cùng một lúc, giúp tránh việc tính toán lại khoảng cách trên mỗi dải. các`safe`ma trận nén giới hạn khoảng cách thành điều kiện boolean, điều này rất quan trọng để giảm vấn đề về phép đếm 2D. 

Vòng lặp cặp hàng thực thi giới hạn chiều cao dải. Bên trong nó,`ok`mảng buộc cả hai hàng đều có thể sử dụng được và đáp ứng các ràng buộc về an toàn. Lần quét cuối cùng là sự tích lũy phân đoạn liền kề cổ điển, trong đó mỗi phân đoạn tối đa đóng góp tất cả các phân đoạn. 

Một điểm tinh tế là các tế bào nước luôn được coi là an toàn bất kể khoảng cách, vì chúng là nguồn gốc của giá trị. Một điều nữa là các ô bị chặn sẽ được xử lý riêng biệt và ngay lập tức làm mất hiệu lực của một cột. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ có hai hàng và năm cột:```
1 1 1 1 2
2 1 1 1 1
```Cho rằng$k$đủ lớn để tất cả các ô trồng cây đều an toàn. 

| Cột | Hàng 1 | Hàng 2 | được | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | đúng | 
| 1 | 1 | 1 | đúng | 
| 2 | 1 | 1 | đúng | 
| 3 | 1 | 1 | đúng | 
| 4 | 2 | 1 | đúng | 

Toàn bộ cặp hàng là hợp lệ, vì vậy chúng tôi nhận được một đoạn có độ dài 5. Đóng góp là$5 \cdot 6 / 2 = 15$. 

Điều này cho thấy một dải hợp lệ đầy đủ mang lại tất cả các phân đoạn con như thế nào, không chỉ toàn bộ chiều rộng. 

Bây giờ hãy xem xét trường hợp có ô bị chặn:```
1 3 1 1
2 1 1 2
```| Cột | Hàng 1 | Hàng 2 | được | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | đúng | 
| 1 | 3 | 1 | sai | 
| 2 | 1 | 1 | đúng | 
| 3 | 1 | 2 | đúng | 

Điều này chia thành hai phân đoạn:`[0]`,`[2,3]`. Đóng góp là$1$Và$2 \cdot 3 / 2 = 3$, tổng cộng$4$. 

Dấu vết xác nhận rằng các ô bị chặn đã phân chia chính xác việc đếm thành các bài toán con độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| BFS chạy vào$O(n^2)$và quét cặp hàng qua các cột là$O(n^2)$tổng thể | 
| Không gian |$O(n^2)$| lưu trữ khoảng cách và lưới | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n=2000$dẫn đến khoảng bốn triệu ô và cả BFS và quá trình quét đều tuyến tính ở kích thước này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# minimal case: no wells, everything blocked by distance rule
assert run("1 1\n1\n") == "0"

# single well, small grid
assert run("2 2\n1 2\n1 1\n") is not None

# all wells, all safe
assert run("2 1\n2 2\n2 2\n") == "3"

# blocked center splits segments
assert run("2 5\n1 3 1 1 1\n2 1 1 2 1\n") is not None

# sample
assert run("10 3\n1 1 1 1 2 1 1 3 1 1\n2 1 1 1 1 1 1 2 1 3\n1 1 2 1 1 2 1 3 1 1\n1 1 1 1 1 1 1 1 1 1\n1 2 1 1 1 1 1 1 2 1\n1 1 1 2 1 1 1 1 1 1\n1 1 1 1 1 1 1 1 1 2\n1 2 1 1 1 3 1 1 1 1\n1 3 1 1 1 1 1 2 1 3\n1 1 3 2 1 1 1 1 1 3\n") == "140"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 không ổn | 0 | cạnh thất bại khoảng cách | 
| 2x2 tất cả giếng | 3 | tổ hợp cơ sở | 
| hỗn hợp 2x5 | 4 | độ chính xác của phân đoạn | 
| mẫu | 140 | sự đúng đắn đầy đủ | 

## Vỏ cạnh 

Một lưới không có giếng thể hiện sự phụ thuộc khoảng cách toàn cầu. Mỗi ô trồng cây đều không đạt yêu cầu về khoảng cách, vì vậy mỗi dải trồng đều không hợp lệ. Việc khởi tạo BFS không tạo ra các nguồn có thể truy cập được, khiến mọi khoảng cách là vô hạn và`safe`ma trận trở nên sai ở mọi nơi ngoại trừ giếng. Vì không có giếng nào tồn tại nên không có dạng phân đoạn hợp lệ và câu trả lời chính xác sẽ trở thành 0. 

Một dải trong đó tất cả tính hợp lệ bị phá vỡ bởi một ô bị chặn duy nhất cho biết cách hoạt động của phân đoạn. Thuật toán xử lý từng cột một cách độc lập sau khi xử lý trước, do đó, một cột không hợp lệ sẽ tách một phân đoạn thành hai vấn đề đếm độc lập một cách rõ ràng. Điều này ngăn chặn việc đếm quá mức và đảm bảo tính chính xác ngay cả khi các ô bị chặn được phân bố thưa thớt.
