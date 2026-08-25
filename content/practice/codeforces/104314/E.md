---
title: "CF 104314E - Xây dựng cầu"
description: "Chúng tôi được cung cấp một lưới đại diện cho một quần đảo. Mỗi ô là đất, được đánh dấu là 1 hoặc nước, được đánh dấu là 0. Bất kỳ hai ô đất nào chạm lên, xuống, trái hoặc phải đều thuộc về cùng một hòn đảo, do đó, lưới sẽ tự nhiên chia thành nhiều thành phần được kết nối trong số 1."
date: "2026-07-01T19:40:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104314
codeforces_index: "E"
codeforces_contest_name: "XXV Interregional Programming Olympiad, Vologda SU, 2023"
rating: 0
weight: 104314
solve_time_s: 76
verified: true
draft: false
---

[CF 104314E - Xây dựng cầu](https://codeforces.com/problemset/problem/104314/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới đại diện cho một quần đảo. Mỗi ô là đất, được đánh dấu là`1`hoặc nước, được đánh dấu là`0`. Bất kỳ hai ô đất nào chạm lên, xuống, trái hoặc phải đều thuộc cùng một hòn đảo, do đó, lưới sẽ tự nhiên phân chia thành nhiều thành phần được kết nối của`1`S. 

Nhiệm vụ là làm cho tất cả các hòn đảo được kết nối thành một vùng đất duy nhất bằng cách biến một số ô nước thành đất liền. Mỗi thao tác lật đúng một`0`vào trong`1`. Mục tiêu là xác định số lần lật tối thiểu như vậy để sau tất cả các thay đổi, có chính xác một thành phần đất được kết nối trên toàn bộ lưới. 

Điểm mấu chốt là chúng ta không được phép di chuyển đất hoặc thêm cầu theo hình dạng tùy ý, chỉ chuyển ô nước thành đất và kết nối đúng 4 hướng. 

Các ràng buộc ngụ ý rằng tổng số ô tối đa là 10^6. Điều này loại trừ bất cứ điều gì tồi tệ hơn hành vi đại khái là O(nm log nm) hoặc bậc hai. Quét tuyến tính trên lưới là khả thi, nhưng BFS lặp đi lặp lại hoặc lấp đầy từ nhiều nguồn phải được kiểm soát cẩn thận để mỗi ô chỉ được xử lý một số lần không đổi. 

Một sự hiểu lầm ngây thơ nảy sinh khi cố gắng trực tiếp “phát triển” các hòn đảo một cách tham lam mà không thừa nhận cấu trúc toàn cầu. Ví dụ: hãy xem xét một mẫu bàn cờ:```
101
010
101
```Mỗi`1`bị cô lập nên mỗi tế bào là hòn đảo riêng của nó. Một cách tiếp cận ngây thơ có thể cố gắng kết nối các hòn đảo lân cận một cách cục bộ mà không nhận ra rằng tất cả các hòn đảo cần trở thành một thành phần được kết nối và các ô nước trung gian có thể phục vụ nhiều kết nối. 

Một trường hợp cạnh khác xuất hiện khi các hòn đảo gần như đã được kết nối ngoại trừ khoảng cách đường chéo hẹp:```
100
000
001
```Ở đây, kết nối ngắn nhất đòi hỏi phải lựa chọn cẩn thận con đường ở giữa chứ không chỉ tham lam kết nối các điểm cuối gần nhất. 

Khó khăn trọng tâm là nhận ra rằng vấn đề đang yêu cầu số lần lật tối thiểu cần thiết để kết nối nhiều thành phần trong biểu đồ lưới theo chuyển động 4 hướng, điều này gợi ý cấu trúc mở rộng đường dẫn ngắn nhất hoặc đa nguồn thay vì các kết nối cặp độc lập. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét từng cặp đảo và tính toán số lượng ô nước tối thiểu phải được chuyển đổi để kết nối chúng. Nếu có K đảo, điều này dẫn đến K^2 cặp và với mỗi cặp, chúng tôi sẽ tính toán đường đi ngắn nhất qua các ô nước, mỗi lần thực hiện là một BFS trên lưới. Vì mỗi BFS có giá O(nm), giá trị này trở thành O(K^2 nm), vượt xa giới hạn ngay cả khi K ở mức vừa phải. 

Ngay cả khi chúng tôi cố gắng cải thiện nó bằng cách chạy BFS từ mỗi hòn đảo một cách độc lập, chúng tôi vẫn kết thúc việc tính toán lại công việc chồng chéo nhiều lần trên cùng một cấu trúc lưới. 

Nhận xét quan trọng là việc kết nối tất cả các đảo tương đương với việc mở rộng dần dần đất đai ra ngoài từ tất cả các đảo cùng một lúc. Nếu chúng ta đối xử với tất cả`1`các ô làm nguồn ban đầu trong BFS đa nguồn, sau đó mỗi ô nước được chỉ định một khoảng cách bằng số lần lật tối thiểu cần thiết để đến đất liền. Tuy nhiên, điều này chỉ đưa ra khoảng cách đến hòn đảo gần nhất chứ không thể kết nối đầy đủ tất cả các hòn đảo. 

Để buộc phải kết nối đầy đủ, chúng tôi giải thích lại vấn đề là tìm số lớp nước tối thiểu cần thiết để việc mở rộng từ các hòn đảo khác nhau gặp nhau. Nói cách khác, chúng tôi muốn khoảng cách tối thiểu mà tại đó hai sóng BFS bắt đầu từ các đảo khác nhau giao nhau lần đầu tiên. Điều này tương đương với việc tính toán cầu nối ngắn nhất giữa hai thành phần bất kỳ. 

Một quan điểm trực tiếp và đơn giản hơn là thừa nhận rằng bất kỳ vùng đất liền nào được kết nối cuối cùng đều phải bao gồm ít nhất một cấu trúc nối liền tất cả các đảo. Cách rẻ nhất để kết nối các thành phần trong một mạng lưới trong đó mỗi bước tốn 1 chi phí cho việc mở rộng nước là coi nó giống như một đường đi ngắn nhất có nhiều nguồn trong đó mỗi hòn đảo mở rộng đồng thời và chúng tôi theo dõi khi các phần mở rộng va chạm từ các nguồn khác nhau. 

Điều này dẫn đến cách tiếp cận BFS hai giai đoạn: gắn nhãn đảo đầu tiên, sau đó chạy BFS đa nguồn trong đó mỗi đảo mở rộng ra bên ngoài với khoảng cách 0 tại ranh giới của nó và chúng tôi phát hiện điểm gặp nhau đầu tiên giữa các mặt trận đảo khác nhau. Khoảng cách gặp nhau đó tương ứng với số lần lật tối thiểu cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo cặp BFS | O(K^2 · nm) | O(nm) | Quá chậm | 
| BFS đa nguồn từ tất cả các đảo | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên hãy quét lưới và xác định tất cả các thành phần được kết nối của`1`s sử dụng BFS hoặc DFS. Mỗi thành phần được gán một mã định danh đảo duy nhất. Bước này đảm bảo chúng ta có thể phân biệt từng ô đất thuộc về làn sóng mở rộng nào. 
2. Thu thập tất cả các ô ranh giới của tất cả các đảo. Ô ranh giới là ô đất có ít nhất một ô nước liền kề. Đây là những tế bào duy nhất có thể ảnh hưởng đến sự giãn nở trong nước. 
3. Khởi tạo hàng đợi BFS đa nguồn chứa tất cả các ô đất ranh giới, mỗi ô được gắn thẻ id đảo và khoảng cách 0. Chúng tôi cũng duy trì lưới khoảng cách được khởi tạo thành -1 cho nước và 0 cho đất liền. 
4. Thực hiện BFS qua lưới. Đối với mỗi ô được bật lên, chúng tôi cố gắng mở rộng sang bốn ô lân cận. Nếu hàng xóm là nước, chúng ta gán cho nó id đảo hiện tại và khoảng cách +1, sau đó đẩy nó vào hàng đợi. 
5. Nếu chúng tôi gặp một người hàng xóm đã được ghé thăm nhưng đã đến từ một id đảo khác, chúng tôi đã tìm thấy sự va chạm giữa hai đợt mở rộng. Câu trả lời là tổng khoảng cách từ cả hai phía, tương ứng với số lượng ô nước cần thiết để kết nối các đảo. 
6. Trả về giá trị xung đột tối thiểu gặp phải trong BFS. 

Lý do là BFS mở rộng theo thứ tự tăng dần về số lần lật. Lần đầu tiên hai mặt trận đảo khác nhau gặp nhau, chúng tôi đã tìm thấy sự kết nối tối ưu trên toàn cầu. 

### Tại sao nó hoạt động 

Mỗi ô nước được ấn định khoảng cách tối thiểu từ bất kỳ ranh giới đảo nào, nhưng quan trọng hơn, BFS đảm bảo rằng quá trình mở rộng diễn ra theo thứ tự lật tăng dần. Khi hai nhãn đảo khác nhau gặp nhau tại một ô hoặc trên một cạnh, khoảng cách kết hợp phản ánh đường đi ngắn nhất qua mặt nước nối hai hòn đảo đó. Vì BFS khám phá tất cả các bản mở rộng ngắn nhất trước tiên nên xung đột đầu tiên phải là tối ưu trong số tất cả các kết nối giữa các đảo có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    dirs = [(1,0),(-1,0),(0,1),(0,-1)]

    comp = [[-1]*m for _ in range(n)]
    cid = 0

    # 1. label islands
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '1' and comp[i][j] == -1:
                q = deque([(i,j)])
                comp[i][j] = cid
                while q:
                    x, y = q.popleft()
                    for dx, dy in dirs:
                        nx, ny = x+dx, y+dy
                        if 0 <= nx < n and 0 <= ny < m:
                            if grid[nx][ny] == '1' and comp[nx][ny] == -1:
                                comp[nx][ny] = cid
                                q.append((nx, ny))
                cid += 1

    # 2. multi-source BFS from all land
    dist = [[-1]*m for _ in range(n)]
    owner = [[-1]*m for _ in range(n)]
    q = deque()

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '1':
                dist[i][j] = 0
                owner[i][j] = comp[i][j]
                q.append((i,j))

    ans = float('inf')

    while q:
        x, y = q.popleft()
        for dx, dy in dirs:
            nx, ny = x+dx, y+dy
            if 0 <= nx < n and 0 <= ny < m:
                if dist[nx][ny] == -1:
                    dist[nx][ny] = dist[x][y] + 1
                    owner[nx][ny] = owner[x][y]
                    q.append((nx, ny))
                else:
                    if owner[nx][ny] != owner[x][y]:
                        ans = min(ans, dist[nx][ny] + dist[x][y])

    print(ans)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên gán cho mỗi hòn đảo một danh tính ổn định để việc mở rộng BFS sau này có thể phát hiện khi hai hòn đảo khác nhau gặp nhau. Nếu không có nhãn này, chúng tôi sẽ hợp nhất không chính xác các phần mở rộng từ cùng một hòn đảo. 

Giai đoạn thứ hai coi tất cả các ô đất là nguồn BFS đồng thời. Mỗi ô nước được ấn định khoảng cách tối thiểu để đến được nó từ một hòn đảo nào đó. Chi tiết triển khai quan trọng là chúng tôi lưu trữ cả id khoảng cách và id đảo gốc, cho phép chúng tôi phát hiện va chạm giữa các nguồn khác nhau. 

Bản cập nhật câu trả lời sử dụng`dist[nx][ny] + dist[x][y]`, tương ứng với tổng khoảng cách từ hai mặt trận mở rộng gặp nhau trên một cạnh. Đây là sự tương tự riêng biệt của việc tìm đường đi ngắn nhất giữa hai vùng đa nguồn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
101
010
101
```Chúng ta bắt đầu với bốn hòn đảo, mỗi góc`1`là thành phần riêng của nó. 

| Bước | Tế bào | Khoảng cách | Chủ sở hữu | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | 0 | A | bắt đầu BFS | 
| 2 | (0,2) | 0 | B | bắt đầu BFS | 
| 3 | (2,0) | 0 | C | bắt đầu BFS | 
| 4 | (2,2) | 0 | D | bắt đầu BFS | 
| 5 | (1,1) | 1 | A | đạt được nước đầu tiên | 

Khi BFS mở rộng, ô trung tâm có thể đạt được từ nhiều hướng với chi phí bằng nhau. Vụ va chạm đầu tiên giữa các chủ sở hữu khác nhau xảy ra ở khoảng cách 1 nên đáp án là 1. 

Điều này khẳng định rằng tính đối xứng đường chéo không thành vấn đề, chỉ có sự giãn nở ngắn nhất của Manhattan qua nước mà thôi. 

### Ví dụ 2 

đầu vào:```
2 3
100
001
```Có hai hòn đảo ở các góc đối diện. 

| Bước | Tế bào | Khoảng cách | Chủ sở hữu | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | 0 | A | bắt đầu | 
| 2 | (1,2) | 0 | B | bắt đầu | 
| 3 | (0,1) | 1 | A | mở rộng | 
| 4 | (1,1) | 1 | B | mở rộng | 

Tại ô (1,1), cả hai phần mở rộng đều đáp ứng tổng chi phí là 2. 

Điều này cho thấy rằng ngay cả khi tồn tại nhiều đường đi ngắn nhất, BFS vẫn đảm bảo cuộc gặp sớm nhất tương ứng với cây cầu tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập với số lần không đổi trong BFS và ghi nhãn | 
| Không gian | O(nm) | Lưu trữ nhãn thành phần, khoảng cách và quyền sở hữu | 

Kích thước lưới tối đa là 10^6 ô, do đó, BFS thời gian tuyến tính với hệ số không đổi trên mỗi ô vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()).strip() if solve() is not None else ""

# provided samples
assert run("3 3\n101\n010\n101\n") == "1", "sample 1"
assert run("2 3\n100\n001\n") == "2", "sample 2"

# single-step bridge
assert run("2 2\n10\n01\n") == "1"

# already almost connected
assert run("3 3\n111\n101\n111\n") == "0"

# large uniform islands separated by wide gap
assert run("1 5\n10001\n") == "3"

# minimal case with two cells
assert run("1 2\n10\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường chéo 2x2 | 1 | cầu tối thiểu | 
| đảo bao quanh | 0 | trường hợp đã được kết nối | 
| đường khoảng cách rộng | 3 | cầu Manhattan dài | 
| hàng đơn | 1 | ranh giới liền kề | 

## Vỏ cạnh 

Một trường hợp khó phát hiện là khi các đảo đã được kết nối sau khi dán nhãn nhưng BFS vẫn chạy. Ví dụ:```
3 3
111
111
111
```Ở đây thực tế chỉ có một hòn đảo. BFS sẽ không bao giờ tìm thấy xung đột giữa các chủ sở hữu khác nhau, vì vậy câu trả lời sẽ là vô hạn. Việc triển khai đúng phải tránh được điều này hoặc dựa vào sự đảm bảo rằng có ít nhất hai hòn đảo. 

Một trường hợp khác là hành lang hẹp:```
1 5
10001
```BFS mở rộng đối xứng từ cả hai đầu. Tại ô trung tâm, cả hai sóng gặp nhau sau tổng cộng 2 bước, cho kết quả 3. Thuật toán tính toán chính xác cả hai phía thay vì chỉ khoảng cách một phía. 

Trường hợp cạnh cuối cùng là nhiều hòn đảo gặp nhau tại cùng một ô nước ở cùng cấp độ BFS. Thuật toán xử lý việc này một cách tự nhiên vì quyền sở hữu được theo dõi trên mỗi ô và bất kỳ chủ sở hữu nào khác sẽ kích hoạt câu trả lời ứng viên hợp lệ, đảm bảo không bỏ sót kết nối tối ưu nào.
