---
title: "CF 104835F - Mảnh ghép ngọt ngào nhất"
description: "Chúng ta có một lưới hình chữ nhật có kích thước $n nhân m$, trong đó mỗi ô chứa một giá trị chiều cao. Hãy coi lưới này như một cảnh quan gồm các mảnh baklava với độ cao khác nhau. Zeynep liên tục đổ xi-rô lên một số tế bào ban đầu."
date: "2026-06-28T11:47:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104835
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 12-01-23 Div. 2 (Beginner)"
rating: 0
weight: 104835
solve_time_s: 86
verified: true
draft: false
---

[CF 104835F - Mảnh ghép ngọt ngào nhất](https://codeforces.com/problemset/problem/104835/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$n \times m$, trong đó mỗi ô chứa một giá trị chiều cao. Hãy coi lưới này như một cảnh quan gồm các mảnh baklava với độ cao khác nhau. Zeynep liên tục đổ xi-rô lên một số tế bào ban đầu. Từ mỗi điểm xuất phát, xi-rô sẽ lan sang các ô lân cận, nhưng chỉ dọc theo những chuyển động mà nó được phép “chảy xuống dốc hoặc giữ nguyên”. 

Các bước di chuyển được phép từ một ô$(i, j)$đi đến bốn vị trí lân cận: trực tiếp lên, trực tiếp xuống, chéo lên bên trái và chéo xuống bên phải. Trong các ký hiệu, đây là những$(i-1, j)$,$(i+1, j)$,$(i-1, j-1)$, Và$(i+1, j+1)$. Xi-rô chỉ có thể di chuyển đến hàng xóm nếu chiều cao của hàng xóm nhỏ hơn hoặc bằng chiều cao của ô hiện tại. Sau khi xi-rô được đổ vào điểm truy vấn, nó sẽ lan rộng nhất có thể theo các quy tắc này và chúng tôi ghi lại mọi ô mà nó tiếp cận. Sau khi kết thúc một lần đổ, quy trình sẽ được đặt lại và lần đổ tiếp theo sẽ bắt đầu mới. 

Nhiệm vụ là xác định, trong tất cả các lần đổ, ô lưới nào được truy cập nhiều lần nhất. Nếu nhiều ô bị ràng buộc thì chúng ta phải chọn ô có chỉ số hàng nhỏ nhất, còn nếu vẫn bị ràng buộc thì chỉ số cột nhỏ nhất. 

Lưới có tối đa 100 x 100 ô, do đó có tối đa 10.000 nút. Mỗi truy vấn có thể bắt đầu một quá trình duyệt giống như lũ lụt và có tới 1.000 truy vấn. Điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng thực hiện tính toán lại tốn kém trên toàn bộ lưới cho mỗi truy vấn ngoài công việc tuyến tính hoặc gần tuyến tính trong kích thước lưới. Một giải pháp gần như$O(q \cdot n \cdot m)$có thể chấp nhận được vì nó theo thứ tự$10^7$hoạt động. 

Một trường hợp thất bại tinh vi đối với việc triển khai ngây thơ là quên rằng chuyển động bị hạn chế bởi độ cao. Ví dụ: nếu tất cả các độ cao đều tăng dần dọc theo một đường dẫn nhưng bạn bỏ qua ràng buộc, thì bạn sẽ coi lưới là được kết nối đầy đủ một cách không chính xác. 

Một cạm bẫy khác là trộn lẫn khả năng kết nối giữa các lần đổ khác nhau. Mỗi truy vấn phải được xử lý độc lập. Nếu một người nhầm lẫn giữ trạng thái đã truy cập trong các truy vấn thì kết quả BFS trước đó sẽ làm ô nhiễm các kết quả sau đó và làm tăng số lượng một cách giả tạo. 

## Phương pháp tiếp cận 

Việc giải thích trực tiếp vấn đề gợi ý thực hiện việc lấp đầy từ mỗi ô truy vấn. Từ vị trí bắt đầu, chúng tôi khám phá tất cả các ô có thể truy cập bằng cách sử dụng hàng đợi hoặc ngăn xếp, chỉ thực hiện các bước di chuyển hợp lệ thỏa mãn giới hạn chiều cao. Mỗi khi chúng tôi đến một ô, chúng tôi sẽ tăng bộ đếm toàn cầu của ô đó. 

Chiến lược bạo lực này đã gần đạt đến mức tối ưu do có những hạn chế. Mỗi BFS hoặc DFS chỉ khám phá tối đa$n \cdot m$các ô và mỗi ô được xử lý một số lần không đổi do có thể di chuyển bốn lần. Với tối đa 1000 truy vấn, trường hợp xấu nhất là về$1000 \times 10000 = 10^7$các chuyến thăm trạng thái, có thể chấp nhận được trong Python khi được triển khai cẩn thận. 

Không cần xử lý trước nâng cao hơn vì cấu trúc biểu đồ phụ thuộc vào truy vấn theo nghĩa là khả năng tiếp cận phụ thuộc vào điểm bắt đầu và điều kiện độ cao đơn điệu. Việc tính toán trước khả năng tiếp cận giữa tất cả các cặp sẽ không cần thiết và tốn kém hơn. 

Quan sát quan trọng là mỗi truy vấn độc lập và đóng góp một tần số cộng đơn giản trên một lưới cố định. Điều này biến vấn đề thành việc đếm khả năng tiếp cận bị hạn chế lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force cho mỗi truy vấn |$O(q \cdot n \cdot m)$|$O(n \cdot m)$| Đã chấp nhận | 
| Bất kỳ tính toán trước tất cả các cặp toàn cầu nào |$O((nm)^2)$hoặc tệ hơn |$O((nm)^2)$| Quá chậm | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng từng lần đổ xi-rô một cách độc lập và tích lũy số lượng bao phủ. 

1. Khởi tạo mảng 2D`cnt`kích thước$n \times m$với số không. Điều này sẽ lưu trữ số lần đạt được mỗi ô trong tất cả các lần đổ. 
2. Đối với mỗi ô bắt đầu truy vấn, hãy chạy BFS hoặc DFS. Chúng tôi cũng duy trì một mảng được truy cập cục bộ để đảm bảo chúng tôi không xử lý cùng một ô hai lần trong một lần đổ. Điều này là cần thiết vì nhiều đường dẫn có thể đến cùng một ô nhưng chỉ được tính một lần cho mỗi truy vấn. 
3. Từ ô bắt đầu, đẩy ô đó vào hàng đợi và đánh dấu ô đã truy cập. Trong khi xử lý một ô$(i, j)$, chúng tôi thử tất cả bốn hướng cho phép. Đối với mỗi người hàng xóm$(ni, nj)$, chúng tôi kiểm tra hai điều kiện: nó nằm trong lưới và chiều cao của nó nhỏ hơn hoặc bằng$h[i][j]$. Nếu cả hai đều giữ và nó chưa được truy cập trong truy vấn này, chúng tôi đánh dấu nó đã truy cập và thêm nó vào hàng đợi. 
4. Mỗi lần chúng tôi đánh dấu một ô đã ghé thăm trong một truy vấn, chúng tôi sẽ tăng`cnt[i][j]`bởi một. Điều này đảm bảo mỗi tế bào đóng góp tối đa một lần cho mỗi lần đổ. 
5. Sau khi xử lý tất cả các truy vấn, chúng tôi quét`cnt`mảng để tìm giá trị lớn nhất. Nếu nhiều ô có cùng mức tối đa thì chúng ta chọn ô có hàng nhỏ nhất, sau đó là cột nhỏ nhất. 

### Tại sao nó hoạt động 

BFS từ mỗi truy vấn liệt kê chính xác tất cả các ô có thể truy cập theo giới hạn độ cao không tăng đơn điệu bằng cách sử dụng các bước di chuyển được phép. Vì chúng tôi đánh dấu đã truy cập cho mỗi truy vấn nên mỗi ô được tính nhiều nhất một lần cho mỗi lần đổ, phù hợp với định nghĩa “được bao phủ bởi xi-rô”. Tổng hợp các truy vấn tích lũy đóng góp độc lập. Vì chúng tôi đánh giá tất cả các trạng thái có thể truy cập cho mỗi truy vấn nên không có ô nào có thể truy cập bị bỏ sót và không có ô nào không thể truy cập được đưa vào. 

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
        cnt[si][sj] += 1

        while dq:
            i, j = dq.popleft()
            for di, dj in dirs:
                ni, nj = i + di, j + dj
                if 0 <= ni < n and 0 <= nj < m:
                    if not vis[ni][nj] and h[ni][nj] <= h[i][j]:
                        vis[ni][nj] = True
                        cnt[ni][nj] += 1
                        dq.append((ni, nj))

    best_i, best_j = 0, 0
    for i in range(n):
        for j in range(m):
            if cnt[i][j] > cnt[best_i][best_j]:
                best_i, best_j = i, j
            elif cnt[i][j] == cnt[best_i][best_j]:
                if i < best_i or (i == best_i and j < best_j):
                    best_i, best_j = i, j

    print(best_i + 1, best_j + 1, cnt[best_i][best_j])

if __name__ == "__main__":
    solve()
```Lưới được lưu trữ trực tiếp dưới dạng số nguyên và ma trận được truy cập mới được tạo cho mỗi truy vấn vì việc sử dụng lại trên các truy vấn sẽ hợp nhất không chính xác các phần lấp đầy độc lập. BFS sử dụng deque để đảm bảo truyền tải theo thời gian tuyến tính cho mỗi truy vấn. 

Danh sách hướng mã hóa chính xác bốn bước di chuyển được phép. Việc kiểm tra độ cao được áp dụng khi mở rộng các cạnh, đảm bảo chúng tôi chỉ tuân theo các chuyển tiếp xuống dốc hoặc bằng phẳng hợp lệ. 

Cuối cùng, việc lựa chọn ô tốt nhất được thực hiện bằng một lần quét trên lưới, tôn trọng cả tần số tối đa và quy tắc ràng buộc từ điển. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi quá trình trên một phiên bản đơn giản của đầu vào mẫu. 

Hãy xem xét một lưới nhỏ: 

| Bước | Bắt đầu truy vấn | Các ô đã truy cập | Cập nhật chính | 
| --- | --- | --- | --- | 
| 1 | (1,1) | vùng có thể truy cập từ (1,1) | tăng đều đạt | 
| 2 | (3,3) | vùng có thể truy cập từ (3,3) | tăng đều đạt | 
| 3 | (1,5) | vùng có thể truy cập từ (1,5) | tăng đều đạt | 

Sau tất cả các truy vấn, mỗi ô có số lượng bằng số vùng BFS được bao gồm trong đó. 

Điều này chứng tỏ rằng mỗi lần đổ đóng góp độc lập và sự chồng chéo đó tích lũy một cách tự nhiên trong`cnt`. 

Bây giờ hãy xem xét trường hợp suy biến trong đó tất cả các độ cao đều bằng nhau. Mỗi BFS trở thành truyền tải toàn lưới, do đó mỗi ô nhận được chính xác$q$số gia tăng. Sau đó, quy tắc ràng buộc sẽ chọn ô (1,1), xác nhận việc xử lý từ điển chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot n \cdot m)$| Mỗi truy vấn thực hiện BFS trên hầu hết tất cả các ô và mỗi ô được xử lý một lần cho mỗi truy vấn | 
| Không gian |$O(n \cdot m)$| Lưới, bộ đếm và ma trận đã truy cập cho mỗi truy vấn | 

Với$n, m \le 100$Và$q \le 1000$, tổng công việc là khoảng$10^7$hoạt động phù hợp thoải mái trong giới hạn điển hình của Python khi sử dụng các hoạt động xếp hàng hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

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
        cnt[si][sj] += 1

        while dq:
            i, j = dq.popleft()
            for di, dj in dirs:
                ni, nj = i + di, j + dj
                if 0 <= ni < n and 0 <= nj < m:
                    if not vis[ni][nj] and h[ni][nj] <= h[i][j]:
                        vis[ni][nj] = True
                        cnt[ni][nj] += 1
                        dq.append((ni, nj))

    best_i, best_j = 0, 0
    for i in range(n):
        for j in range(m):
            if cnt[i][j] > cnt[best_i][best_j]:
                best_i, best_j = i, j
            elif cnt[i][j] == cnt[best_i][best_j]:
                if i < best_i or (i == best_i and j < best_j):
                    best_i, best_j = i, j

    return f"{best_i+1} {best_j+1} {cnt[best_i][best_j]}"

assert run("""5 5 3
7 9 9 9 9
6 6 9 2 8
5 9 5 2 8
4 3 5 2 8
3 9 5 2 8
1 1
3 3
1 5
""") == "2 4 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới ô đơn | 1 1 1 | xử lý kích thước tối thiểu | 
| tất cả các chiều cao bằng nhau | 1 1 q | khả năng tiếp cận đầy đủ và ràng buộc | 
| chuỗi giảm nghiêm ngặt | 1 1 q | truyền bá theo hướng hạn chế | 
| đỉnh được bao quanh bởi các tế bào phía dưới | tọa độ đỉnh | hạn chế xuống dốc đúng cách | 

## Vỏ cạnh 

Hãy xem xét một lưới trong đó tất cả các giá trị đều bằng nhau và mọi ô đều có thể truy cập được từ mọi điểm bắt đầu. Mỗi BFS sẽ tràn ngập toàn bộ lưới, vì vậy mỗi ô sẽ tích lũy chính xác$q$số gia tăng. Thuật toán sẽ quét lưới sau đó và chọn tọa độ nhỏ nhất theo từ điển là (1,1), khớp với quy tắc ràng buộc bắt buộc. 

Bây giờ hãy xem xét trường hợp có đỉnh cao nghiêm ngặt ở (2,2) được bao quanh bởi các lân cận thấp hơn. Nếu truy vấn bắt đầu ở ô thấp hơn, truy vấn đó không thể leo lên đỉnh vì tất cả các bước di chuyển đều yêu cầu chiều cao không tăng. Nếu một truy vấn bắt đầu ở mức cao nhất, BFS có thể mở rộng ra bên ngoài tới tất cả các ô được kết nối bằng hoặc thấp hơn tùy thuộc vào mẫu. Kiểm tra đã truy cập trên mỗi truy vấn đảm bảo mức cao nhất chỉ được tính một lần cho mỗi điểm bắt đầu hợp lệ và hạn chế BFS đảm bảo không xảy ra chuyển đổi đi lên bất hợp pháp.
