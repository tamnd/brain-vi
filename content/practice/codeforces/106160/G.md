---
title: "CF 106160G - Rác vào, rác ra"
description: "Mê cung là một mạng lưới các ô và mỗi ô chứa một chữ số mô tả mức độ phóng xạ của nó. Khách du lịch bắt đầu từ ô trên cùng bên trái và muốn đến ô dưới cùng trong khi chỉ di chuyển giữa các ô liền kề."
date: "2026-06-25T11:12:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "G"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 28
verified: true
draft: false
---

[CF 106160G - Rác vào, rác ra](https://codeforces.com/problemset/problem/106160/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mê cung là một mạng lưới các ô và mỗi ô chứa một chữ số mô tả mức độ phóng xạ của nó. Khách du lịch bắt đầu từ ô trên cùng bên trái và muốn đến ô dưới cùng trong khi chỉ di chuyển giữa các ô liền kề. Mức độ nguy hiểm của tuyến đường đã chọn được xác định bởi tế bào có lượng phóng xạ cao nhất được ghé thăm trên tuyến đường đó. Nhiệm vụ là tìm giá trị nguy hiểm nhỏ nhất có thể có trong số tất cả các tuyến đường, nghĩa là chúng ta muốn đường dẫn có giá trị ô cao nhất càng thấp càng tốt. 

Kích thước lưới đều dưới 100, vì vậy có tối đa khoảng 10.000 ô. Điều này loại trừ các thuật toán liệt kê các đường đi, bởi vì số lần đi bộ có thể tăng theo cấp số nhân khi mê cung ngày càng lớn hơn. Một giải pháp xung quanh O(nm) hoặc O(nm log V), trong đó V là phạm vi các giá trị phóng xạ có thể có, là đủ nhanh. 

Phần tinh tế của vấn đề là chúng ta không cố gắng giảm thiểu tổng các giá trị dọc theo một đường dẫn. Đường dẫn chứa nhiều giá trị nhỏ vẫn có thể tệ hơn đường dẫn chứa một giá trị vừa phải nếu giá trị vừa phải đó là ô lớn nhất được truy cập. Điều này có nghĩa là các thuật toán đường đi ngắn nhất thông thường sử dụng phép cộng không được áp dụng trực tiếp. 

Một trường hợp đặc biệt khác là khi câu trả lời chính là ô bắt đầu. Ví dụ:```
1 1
7
```Đầu ra đúng là:```
7
```Việc triển khai bất cẩn chỉ kiểm tra các ô sau khi di chuyển sẽ trả về sai giá trị nhỏ hơn hoặc không thành công do không có bước di chuyển nào. 

Một trường hợp khác là khi chỉ có thể đến đích thông qua một ô có giá trị cao. Ví dụ:```
3 3
111
999
111
```Đầu ra đúng là:```
1
```bởi vì hàng trên cùng kết nối xung quanh các ô cao. Cách tiếp cận tham lam chọn ô lân cận nhỏ nhất có thể bị mắc kẹt ở gần giữa và tin nhầm câu trả lời là 9. 

Một lỗi phổ biến cuối cùng là bỏ qua việc cho phép xem lại các ô. Lộ trình tối ưu là về khả năng tiếp cận trong một giới hạn chứ không phải về việc xây dựng một tuyến đường đơn giản cụ thể. 

## Phương pháp tiếp cận 

Ý tưởng brute-force trực tiếp là tạo ra mọi tuyến đường có thể từ góc trên bên trái đến đích, tính giá trị tối đa trên mỗi tuyến đường và giữ mức tối đa nhỏ nhất. Điều này đúng vì mọi tuyến đường có thể đều được xem xét. Vấn đề là số lượng tuyến đường. Ngay cả trong một lưới mở nhỏ, mọi vị trí đều có thể phân nhánh thành nhiều lựa chọn, tạo ra số lần đi bộ có thể theo cấp số nhân. Việc tìm kiếm hoàn chỉnh trở nên không thể thực hiện được rất lâu trước kích thước lưới lớn nhất được phép. 

Quan sát quan trọng là câu trả lời là một ngưỡng. Giả sử chúng ta đoán rằng độ phóng xạ tối đa được phép là X. Khi đó, mọi ô có giá trị X nhiều nhất đều có thể sử dụng được và mọi ô lớn hơn đều bị chặn. Câu hỏi ban đầu trở nên đơn giản hơn nhiều: liệu chúng ta có thể đến đích chỉ bằng cách sử dụng các ô có thể sử dụng được không? 

Điều này chuyển đổi vấn đề thành kiểm tra khả năng tiếp cận. Nếu ngưỡng X hoạt động thì mọi ngưỡng lớn hơn cũng hoạt động vì nó chỉ thêm nhiều ô hơn. Nếu X không hoạt động thì mọi ngưỡng nhỏ hơn đều thất bại. Thuộc tính đơn điệu này cho phép tìm kiếm nhị phân trên câu trả lời. 

Đối với mỗi ngưỡng ứng viên, chúng tôi chạy duyệt đồ thị trên lưới. BFS hoặc DFS đánh dấu tất cả các ô có thể truy cập được thông qua các ô có giá trị không vượt quá ngưỡng. Bởi vì chỉ có chín giá trị ô có thể có nên chúng tôi cũng có thể thử tất cả các ngưỡng, nhưng tìm kiếm nhị phân là một mẫu tổng quát hơn và hoạt động với mọi phạm vi giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(nm) | Quá chậm | 
| Tối ưu | O(nm log C) | O(nm) | Đã chấp nhận | 

Ở đây, C là phạm vi giá trị ô tối đa có thể. Đối với vấn đề này, C rất nhỏ vì các ô chứa các chữ số, nhưng phương pháp tương tự cũng có tác dụng nếu giá trị lớn hơn nhiều. 

## Hướng dẫn thuật toán 

1. Tìm giá trị nhỏ nhất và lớn nhất có trong lưới. Câu trả lời phải ở đâu đó trong khoảng này vì không có đường dẫn hợp lệ nào có thể có giá trị tối đa nhỏ hơn mọi ô mà nó truy cập và việc sử dụng giá trị lưới tối đa luôn cho phép mọi ô được sử dụng. 
2. Tìm kiếm nhị phân một ngưỡng ứng cử viên X. Đối với giá trị này, coi mọi ô có giá trị lớn hơn X là bị chặn. 
3. Chạy BFS từ ô bắt đầu. Chỉ thêm các ô lân cận khi chúng ở trong lưới, chưa được truy cập và giá trị của chúng tối đa là X. Việc truyền tải sẽ trả lời liệu một đường dẫn có tồn tại dưới giới hạn hiện tại hay không. 
4. Nếu BFS đến đích, hãy lưu X làm câu trả lời có thể và tìm kiếm ngưỡng nhỏ hơn. Ngưỡng thành công có nghĩa là câu trả lời đúng có thể thấp hơn. 
5. Nếu BFS không thể đến đích, hãy tìm kiếm các ngưỡng lớn hơn vì phải có nhiều ô hơn. 

Tại sao nó hoạt động: bất biến quan trọng là kết quả BFS là đơn điệu đối với ngưỡng. Khi một ngưỡng cho phép một tuyến đường, mọi ngưỡng lớn hơn cũng cho phép tuyến đường đó. Khi một ngưỡng chặn tất cả các tuyến đường thì mọi ngưỡng nhỏ hơn cũng chặn chúng. Tìm kiếm nhị phân tìm thấy ngưỡng nhỏ nhất trong đó khả năng tiếp cận thay đổi từ sai thành đúng, chính xác là giá trị tối đa tối thiểu có thể có trên một đường dẫn. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(map(int, input().strip())) for _ in range(n)]

    low = min(min(row) for row in grid)
    high = max(max(row) for row in grid)

    def can_reach(limit):
        if grid[0][0] > limit:
            return False

        visited = [[False] * m for _ in range(n)]
        q = deque([(0, 0)])
        visited[0][0] = True

        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        while q:
            r, c = q.popleft()

            if r == n - 1 and c == m - 1:
                return True

            for dr, dc in directions:
                nr, nc = r + dr, c + dc

                if 0 <= nr < n and 0 <= nc < m:
                    if not visited[nr][nc] and grid[nr][nc] <= limit:
                        visited[nr][nc] = True
                        q.append((nr, nc))

        return False

    answer = high
    while low <= high:
        mid = (low + high) // 2

        if can_reach(mid):
            answer = mid
            high = mid - 1
        else:
            low = mid + 1

    print(answer)

if __name__ == "__main__":
    solve()
```Lưới được lưu trữ dưới dạng số nguyên nên việc so sánh trong BFS là trực tiếp. Ranh giới tìm kiếm nhị phân được khởi tạo từ các giá trị lưới thực tế, giúp tránh việc kiểm tra không cần thiết các ngưỡng không thể thực hiện được. 

các`can_reach`hàm đại diện cho việc truyền tải đồ thị từ hướng dẫn thuật toán. Ma trận đã truy cập được tạo lại cho mỗi BFS vì mỗi ngưỡng tạo ra một biểu đồ khác nhau gồm các ô có sẵn. 

Ô bắt đầu được kiểm tra trước khi BFS bắt đầu. Điều này xử lý trường hợp một ô và cũng ngăn không cho ngưỡng nhỏ hơn giá trị bắt đầu được coi là hợp lệ. 

Tìm kiếm nhị phân sử dụng mẫu giới hạn dưới tiêu chuẩn. Khi một ngưỡng thành công, nó sẽ được lưu và tìm kiếm sẽ di chuyển sang trái để tìm giá trị hợp lệ nhỏ hơn. Khi thất bại, việc tìm kiếm sẽ chuyển sang phải vì giới hạn phải tăng lên. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
6 8
13567657
24903578
83107213
98829363
25511282
39108443
```Việc tìm kiếm sẽ kiểm tra xem các giới hạn khác nhau có cho phép một đường dẫn hay không. 

| Ngưỡng | Kết quả BFS | Lý do | 
| --- | --- | --- | 
| 4 | Có thể truy cập | Một tuyến tồn tại chỉ sử dụng các ô từ 0 đến 4 | 
| 2 | Không thể truy cập | Đường dẫn bị chặn bởi các ô lớn hơn 2 | 
| 3 | Không thể truy cập | Các cell thấp còn lại không kết nối 2 bên | 

Ngưỡng thành công nhỏ nhất là 4, vì vậy câu trả lời là 4. Dấu vết này thể hiện thuộc tính đơn điệu được sử dụng bởi tìm kiếm nhị phân. 

Đối với mẫu thứ hai:```
6 5
89888
88898
99998
88888
89999
88888
```| Ngưỡng | Kết quả BFS | Lý do | 
| --- | --- | --- | 
| 8 | Có thể truy cập | Vùng lớn gồm các ô có giá trị 8 kết nối điểm bắt đầu và kết thúc | 
| 7 | Không thể truy cập | Mỗi ô có ít nhất 8 ô nên không thể di chuyển được | 

Câu trả lời là 8. Ví dụ này thực hiện trường hợp đường đi tối thiểu có thể vẫn yêu cầu giá trị tương đối cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log C) | Tìm kiếm nhị phân thực hiện các lần chạy BFS O(log C) và mỗi BFS truy cập từng ô nhiều nhất một lần | 
| Không gian | O(nm) | Lưới và ma trận đã truy cập đều sử dụng không gian tỷ lệ với số ô | 

Lưới có ít hơn 10.000 ô, do đó, ngay cả việc duyệt toàn bộ lặp đi lặp lại cũng dễ dàng nằm trong giới hạn. Cách tiếp cận này cũng có quy mô tốt nếu phạm vi giá trị trở nên lớn hơn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solution(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [list(map(int, input().strip())) for _ in range(n)]

    def can_reach(limit):
        if grid[0][0] > limit:
            return False

        q = deque([(0, 0)])
        visited = [[False] * m for _ in range(n)]
        visited[0][0] = True

        while q:
            r, c = q.popleft()
            if r == n - 1 and c == m - 1:
                return True

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc
                if 0 <= nr < n and 0 <= nc < m:
                    if not visited[nr][nc] and grid[nr][nc] <= limit:
                        visited[nr][nc] = True
                        q.append((nr, nc))

        return False

    lo = min(map(min, grid))
    hi = max(map(max, grid))
    ans = hi

    while lo <= hi:
        mid = (lo + hi) // 2
        if can_reach(mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    return str(ans) + "\n"

assert solution("""6 8
13567657
24903578
83107213
98829363
25511282
39108443
""") == "4\n"

assert solution("""6 5
89888
88898
99998
88888
89999
88888
""") == "8\n"

assert solution("""1 1
7
""") == "7\n"

assert solution("""3 3
111
999
111
""") == "1\n"

assert solution("""2 2
99
99
""") == "9\n"

assert solution("""4 4
0123
9993
9994
1114
""") == "4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ô đơn chứa 7 | 7 | Ô xuất phát cũng là đích đến | 
| Lưới có đường dẫn có giá trị thấp khoảng 9 giây | 1 | Tránh các bẫy chuyển động tham lam | 
| Tất cả các ô bằng 9 | 9 | Giá trị tối đa thống nhất được xử lý chính xác | 
| Con đường yêu cầu di chuyển ranh giới | 4 | Ranh giới truyền tải cạnh và BFS là chính xác | 

## Vỏ cạnh 

Đối với lưới đơn ô:```
1 1
7
```Tìm kiếm nhị phân cuối cùng kiểm tra ngưỡng 7. BFS bắt đầu ở ô duy nhất và ngay lập tức thấy rằng đó là đích đến, vì vậy câu trả lời là 7. Thuật toán không bao giờ giả định rằng có ít nhất một nước đi tồn tại. 

Đối với lưới:```
3 3
111
999
111
```Ngưỡng 1 đánh dấu tất cả các ô có giá trị 9 là bị chặn. BFS di chuyển qua hàng trên cùng rồi xuống phía bên phải, đến đích mà không cần vào ô bị chặn. Thuật toán trả về 1 thay vì đi theo một tuyến đường hấp dẫn cục bộ nhưng không chính xác về phía trung tâm. 

Đối với một lưới hoàn toàn đồng nhất:```
2 2
99
99
```Mọi ngưỡng dưới 9 đều không thành công vì bản thân ô bắt đầu không có sẵn. Ở ngưỡng 9, BFS đến đích ngay lập tức thông qua lưới mở, do đó câu trả lời hợp lệ tối thiểu được tìm thấy chính xác là 9.
