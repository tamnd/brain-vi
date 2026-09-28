---
title: "CF 104834E - Mảnh ghép ngọt ngào nhất"
description: "Chúng ta có một lưới có kích thước $n nhân m$, trong đó mỗi ô có giá trị chiều cao. Xi-rô được đổ lên một số ô bắt đầu và từ mỗi điểm bắt đầu, nó sẽ lan ra khắp lưới theo quy tắc phụ thuộc vào các hạn chế về chiều cao và chuyển động."
date: "2026-06-28T11:50:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104834
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 12-01-23 Div. 1 (Advanced)"
rating: 0
weight: 104834
solve_time_s: 64
verified: true
draft: false
---

[CF 104834E - Mảnh ghép ngọt ngào nhất](https://codeforces.com/problemset/problem/104834/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới có kích thước$n \times m$, trong đó mỗi ô có một giá trị chiều cao. Xi-rô được đổ lên một số ô bắt đầu và từ mỗi điểm bắt đầu, nó sẽ lan ra khắp lưới theo quy tắc phụ thuộc vào các hạn chế về chiều cao và chuyển động. 

Từ bất kỳ ô nào, xi-rô có thể di chuyển đến bốn ô lân cận cụ thể: trực tiếp lên, trực tiếp xuống và hai hướng chéo di chuyển một hàng lên và một cột sang trái hoặc một hàng xuống và một cột sang phải. Tuy nhiên, việc di chuyển chỉ được phép nếu ô tiếp theo có chiều cao nhỏ hơn hoặc bằng ô hiện tại. Vì vậy, xi-rô luôn chảy dọc theo các đường dẫn có chiều cao không tăng, nhưng chỉ dọc theo tập hợp các cạnh bị hạn chế này. 

Mỗi lần đổ bắt đầu độc lập. Đối với mỗi ô bắt đầu, chúng tôi mô phỏng toàn bộ vùng có thể truy cập theo các quy tắc này. Mỗi khi có thể truy cập được một ô từ điểm bắt đầu, ô đó được coi là đã được phủ một lần cho lần đổ đó. Sau mỗi lần đổ xong, quy trình sẽ được đặt lại. 

Nhiệm vụ là xác định ô lưới nào được bao phủ bởi số lần đổ lớn nhất. Nếu nhiều ô liên kết, chúng ta chọn ô có chỉ số hàng nhỏ nhất và nếu vẫn bị ràng buộc, chỉ mục cột nhỏ nhất. 

Lưới nhiều nhất là$100 \times 100$, và có tới$1000$đổ. Mô phỏng trực tiếp cho mỗi lần đổ là khả thi về kích thước thô, nhưng chỉ khi mỗi mô phỏng hiệu quả. Một BFS đầy đủ cho mỗi truy vấn sẽ truy cập tối đa$10^4$nút, dẫn đến khoảng$10^7$chuyển đổi tổng thể, nằm ở ranh giới nhưng vẫn có thể chấp nhận được trong Python nếu được triển khai cẩn thận. Bất kỳ giải pháp nào tính toán lại khả năng tiếp cận trong DFS lặp lại đơn giản trên mỗi ô sẽ phát nổ thành$10^9$hoạt động. 

Một trường hợp thất bại tinh tế sẽ phát sinh nếu chúng ta cố gắng coi chuyển động là vô hướng hoặc bỏ qua tính định hướng ràng buộc về độ cao. Ví dụ, hãy xem xét:```
1 3
1 2 3
1 2 1
```Từ (1,3), chúng ta có thể giả định không chính xác rằng chúng ta có thể chảy vào các vùng lân cận thấp hơn mà không kiểm tra các ràng buộc về hướng, mở rộng phạm vi phủ sóng một cách không chính xác. 

Một vấn đề khác là tính toán lại: nếu chúng tôi cố gắng tính toán khả năng tiếp cận cho mỗi cặp (bắt đầu, ô) bằng cách sử dụng DFS lặp lại, chúng tôi sẽ tính toán lại các vùng có thể tiếp cận giống nhau nhiều lần, mặc dù nhiều lần đổ bắt đầu từ các vùng giống nhau hoặc tương tự nhau. 

Quan sát quan trọng là mỗi lần đổ là độc lập và chúng ta chỉ cần truyền khả năng tiếp cận về phía trước dọc theo biểu đồ có hướng được xác định bởi các cạnh lưới và các giới hạn về chiều cao. 

## Phương pháp tiếp cận 

Phương pháp bạo lực mô phỏng từng lần đổ một cách độc lập. Đối với mỗi ô bắt đầu, chúng tôi chạy BFS hoặc DFS và đánh dấu tất cả các ô có thể truy cập. Sau đó, chúng tôi tăng bộ đếm tổng thể cho mỗi ô được truy cập. Vì mỗi BFS có thể chạm tới$n \cdot m$tế bào và có$q$đổ, chi phí trong trường hợp xấu nhất là$O(qnm)$, đó là về$10^7$hoạt động. Điều này đã gần đến giới hạn nhưng vẫn khả thi. 

Một giải pháp thay thế đơn giản hơn sẽ tính toán trước khả năng tiếp cận từ mọi ô đến mọi ô khác bằng cách sử dụng khả năng tiếp cận tất cả các cặp hoặc lấp đầy lũ lặp đi lặp lại. Điều đó sẽ dẫn đến$O((nm)^2)$hoặc tệ hơn, điều này rõ ràng là không thể thực hiện được. 

Thông tin chi tiết quan trọng là chúng tôi không cần sử dụng lại kết quả qua các lần đổ khác nhau, nhưng chúng tôi nên đảm bảo mỗi BFS đều hoạt động hiệu quả và tránh phải truy cập lại các ô. Vì biểu đồ nhỏ và có hướng nên chỉ cần một BFS đơn giản cho mỗi truy vấn với một mảng đã truy cập là đủ. Cấu trúc chuyển động không cho phép các chu kỳ vi phạm các giới hạn về độ cao, do đó, mỗi BFS được giới hạn một cách tự nhiên bởi kích thước lưới. 

Do đó, chúng tôi có thể coi mỗi lần đổ là một vấn đề về khả năng tiếp cận nhiều nguồn nhưng được thực hiện độc lập, tích lũy số lượng bảo hiểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force cho mỗi truy vấn |$O(qnm)$|$O(nm)$| Đã chấp nhận | 
| Tính toán trước khả năng tiếp cận của tất cả các cặp |$O((nm)^2)$|$O((nm)^2)$| Quá chậm | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng lần đổ một cách độc lập bằng BFS/DFS. 

1. Khởi tạo mảng 2D toàn cục`cnt`kích thước$n \times m$về không. Điều này sẽ theo dõi số lượng đổ đến từng ô. 
2. Mỗi lần đổ bắt đầu từ ô$(i, j)$, chạy BFS: 

Chúng tôi đẩy ô bắt đầu vào hàng đợi và đánh dấu ô đó đã truy cập cho BFS cụ thể này. Chúng tôi chỉ tiến tới những người hàng xóm thỏa mãn ràng buộc về chiều cao. 
3. Trong BFS, bất cứ khi nào chúng tôi truy cập một ô, chúng tôi sẽ tăng`cnt[cell]`bởi một. Điều này đảm bảo mỗi lần đổ đóng góp chính xác một lần cho mỗi ô có thể tiếp cận. 
4. Từ mỗi ô được bật ra, cố gắng di chuyển đến bốn ô lân cận được phép: lên, xuống, chéo lên trên bên trái và chéo xuống dưới bên phải. Chúng tôi chỉ đưa hàng xóm vào hàng đợi nếu nó nằm trong lưới, chưa được truy cập trong BFS này và có chiều cao nhỏ hơn hoặc bằng ô hiện tại. 
5. Sau khi hoàn thành BFS cho lần đổ, hãy loại bỏ cấu trúc đã ghé thăm và chuyển sang lần đổ tiếp theo. 
6. Sau khi xử lý tất cả các lần đổ, quét toàn bộ lưới để tìm ô có giá trị lớn nhất trong`cnt`. Phá vỡ các mối quan hệ theo hàng, sau đó theo cột. 

Lý do chúng tôi tăng trong quá trình truyền tải thay vì sau khi BFS hoàn thành là vì chúng tôi muốn mỗi ô ghi lại mức tham gia mỗi lần đổ mà không lưu trữ đầy đủ các tập hợp có thể truy cập. 

### Tại sao nó hoạt động 

Đối với mỗi lần đổ, BFS khám phá chính xác tập hợp các ô có thể truy cập được theo ràng buộc có hướng là chiều cao không tăng dọc theo các bước di chuyển được phép. Mảng đã truy cập đảm bảo mỗi ô được tính nhiều nhất một lần mỗi lần đổ. Vì các lần đổ là độc lập nên việc tính tổng các đóng góp này sẽ mang lại số lần chính xác mà mỗi ô được bao phủ. Lần quét cuối cùng sẽ chọn ô tần số tối đa và việc ngắt liên kết đảm bảo đầu ra xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m, q = map(int, input().split())
    h = [list(map(int, input().split())) for _ in range(n)]

    cnt = [[0] * m for _ in range(n)]

    dirs = [(-1, 0), (1, 0), (-1, -1), (1, 1)]

    for _ in range(q):
        si, sj = map(int, input().split())
        si -= 1
        sj -= 1

        vis = [[False] * m for _ in range(n)]
        dq = deque()
        dq.append((si, sj))
        vis[si][sj] = True

        while dq:
            i, j = dq.popleft()
            cnt[i][j] += 1

            for di, dj in dirs:
                ni, nj = i + di, j + dj
                if 0 <= ni < n and 0 <= nj < m:
                    if not vis[ni][nj] and h[ni][nj] <= h[i][j]:
                        vis[ni][nj] = True
                        dq.append((ni, nj))

    best_i, best_j = 0, 0
    best_val = -1

    for i in range(n):
        for j in range(m):
            if cnt[i][j] > best_val:
                best_val = cnt[i][j]
                best_i, best_j = i, j

    print(best_i + 1, best_j + 1, best_val)

if __name__ == "__main__":
    solve()
```Vòng lặp BFS được cấu trúc sao cho mỗi ô được xử lý chính xác một lần cho mỗi truy vấn. Ma trận đã truy cập được tạo lại cho mỗi lần đổ để tránh ô nhiễm trong các mô phỏng. Mảng hướng mã hóa bốn chuyển tiếp được phép một cách chính xác như đã chỉ định. 

Lần quét cuối cùng là tuyến tính trên lưới và xử lý việc ngắt kết nối một cách an toàn bằng cách dựa vào thứ tự lặp lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5 3
7 9 9 9 9
6 6 9 2 8
5 9 5 2 8
4 3 5 2 8
3 9 5 2 8
1 1
3 3
1 5
```Chúng tôi chỉ theo dõi một số ô đại diện vì trạng thái BFS đầy đủ rất lớn. 

| Đổ | Bắt đầu | Hiệu ứng chính | Các tế bào được bảo hiểm (tóm tắt) | 
| --- | --- | --- | --- | 
| 1 | (1,1) | lan truyền theo đường giảm dần | vùng trên cùng bên trái | 
| 2 | (3,3) | lan rộng khắp lưu vực trung tâm | miền Trung | 
| 3 | (1,5) | trải dọc theo sườn núi bên phải | vùng cột bên phải | 

Sau tất cả các lần đổ, ô (2,4) cuối cùng có thể truy cập được trong cả ba lần truyền tải BFS do vị trí của nó kết nối nhiều đường dẫn giảm dần. 

Kết quả cuối cùng:```
2 4 3
```Điều này cho thấy mức độ chồng chéo của các khu vực có thể tiếp cận quyết định câu trả lời chứ không phải mức tối đa cục bộ của bất kỳ lần đổ nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(qnm)$| Mỗi BFS truy cập từng ô nhiều nhất một lần mỗi lần đổ | 
| Không gian |$O(nm)$| Lưới, mảng đã truy cập và bộ đếm | 

Với$n, m \le 100$Và$q \le 1000$, công lớn nhất là khoảng$10^7$thăm tế bào, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, q = map(int, input().split())
    h = [list(map(int, input().split())) for _ in range(n)]
    cnt = [[0] * m for _ in range(n)]
    dirs = [(-1, 0), (1, 0), (-1, -1), (1, 1)]

    for _ in range(q):
        si, sj = map(int, input().split())
        si -= 1
        sj -= 1
        vis = [[False] * m for _ in range(n)]
        dq = deque([(si, sj)])
        vis[si][sj] = True

        while dq:
            i, j = dq.popleft()
            cnt[i][j] += 1
            for di, dj in dirs:
                ni, nj = i + di, j + dj
                if 0 <= ni < n and 0 <= nj < m:
                    if not vis[ni][nj] and h[ni][nj] <= h[i][j]:
                        vis[ni][nj] = True
                        dq.append((ni, nj))

    best = (-1, -1, -1)
    for i in range(n):
        for j in range(m):
            if cnt[i][j] > best[2]:
                best = (i + 1, j + 1, cnt[i][j])

    return f"{best[0]} {best[1]} {best[2]}"

# sample
assert run("""5 5 3
7 9 9 9 9
6 6 9 2 8
5 9 5 2 8
4 3 5 2 8
3 9 5 2 8
1 1
3 3
1 5
""") == "2 4 3", "sample 1"

# minimum size
assert run("""1 1 1
5
1 1
""") == "1 1 1"

# flat grid
assert run("""2 2 2
1 1
1 1
1 1
2 2
""") == "1 1 2"

# increasing grid (no movement except self)
assert run("""2 2 2
1 2
3 4
2 2
1 1
""") == "1 1 2"

# chain-like flow
assert run("""3 3 1
3 2 1
3 2 1
3 2 1
1 1
""") == "1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 1 1 1 | ranh giới tối thiểu | 
| lưới phẳng đổ nhiều lần | 1 1 2 | đối xứng đầy đủ về khả năng tiếp cận | 
| tăng lưới | 1 1 q | không có chuyển động đi xuống sau khi bắt đầu | 
| lưới dạng chuỗi | 1 1 1 | hạn chế định hướng đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các độ cao đều bằng nhau. Trong tình huống đó, mọi ô đều có thể truy cập được từ bất kỳ điểm bắt đầu nào miễn là nó được kết nối thông qua các hướng cho phép. Ví dụ:```
2 2 1
5 5
5 5
1 1
```BFS từ (1,1) thăm tất cả các ô vì mọi bước di chuyển đều thỏa mãn điều kiện không tăng. Thuật toán tăng chính xác tất cả bốn ô một lần và mức tối đa cuối cùng được gắn. Quy tắc ràng buộc chọn (1,1), khớp với ô nhỏ nhất theo từ điển. 

Một trường hợp khác là khi chuyển động bị hạn chế rất nhiều do địa hình ngày càng tăng nghiêm trọng ngoại trừ lúc bắt đầu. TRONG:```
2 2 1
1 2
3 4
1 1
```Chỉ (1,1) có thể truy cập được vì tất cả hàng xóm đều vi phạm điều kiện về chiều cao. BFS chỉ đánh dấu ô bắt đầu và số đếm vẫn chính xác mà không có bất kỳ sự lan truyền nào. 

Trường hợp tinh vi cuối cùng là việc đổ lặp đi lặp lại bắt đầu từ cùng một ô. Vì mỗi BFS sử dụng một mảng được truy cập độc lập nên sự chồng chéo giữa các lần đổ không gây trở ngại. Cùng một vùng được tính nhiều lần và sự tích lũy phản ánh tần số chính xác thay vì các vị trí bắt đầu riêng biệt.
