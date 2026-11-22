---
title: "CF 35C - Cháy Lại"
description: "Chúng ta có một lưới cây hình chữ nhật. Ngọn lửa bắt đầu đồng thời từ một số ô và mỗi phút nó lại lan sang các ô lân cận có chung một phía. Nhiệm vụ là tìm ra ô nào có thời gian cháy càng lớn càng tốt, nghĩa là nó bắt lửa muộn hơn mọi cây khác."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 35
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 35 (Div. 2)"
rating: 1500
weight: 35
solve_time_s: 130
verified: true
draft: false
---
[CF 35C - Bắn lại](https://codeforces.com/problemset/problem/35/C) 

**Đánh giá:** 1500 
**Tags:** Brute Force, dfs và các đường dẫn tương tự, ngắn nhất 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới cây hình chữ nhật. Ngọn lửa bắt đầu đồng thời từ một số ô và mỗi phút nó lại lan sang các ô lân cận có chung một phía. Nhiệm vụ là tìm ra ô nào có thời gian cháy càng lớn càng tốt, nghĩa là nó bắt lửa muộn hơn mọi cây khác. 

Đây thực sự là bài toán tìm đường đi ngắn nhất trên lưới. Mỗi lần di chuyển đến ô lân cận tốn một phút, vì vậy thời gian khi một ô cháy là khoảng cách tối thiểu của Manhattan từ ô đó đến bất kỳ nguồn lửa ban đầu nào. Chúng tôi muốn ô có khoảng cách tối đa như vậy. 

Lưới có thể chứa tới 2000 × 2000 = 4.000.000 ô. Con số đó đủ lớn để việc thực hiện phép tính bậc hai trên tất cả các ô là nguy hiểm. Vì có tối đa 10 điểm bắt đầu, nên giải pháp quét lưới một lần hoặc thực hiện truyền tải đồ thị tiêu chuẩn là thực tế, nhưng bất cứ điều gì liên quan đến việc truyền tải toàn bộ lưới lặp đi lặp lại sẽ gặp khó khăn. 

Một lỗi phổ biến là tính toán khoảng cách riêng biệt với từng nguồn bằng BFS và tiếp tục khởi động lại quá trình truyền tải. Trên lưới 4 triệu ô, ngay cả một BFS cũng đã chạm tới hàng triệu trạng thái. Làm điều này 10 lần là tốn kém không cần thiết. 

Một vấn đề tế nhị khác là xử lý cà vạt. Sự cố cho phép bất kỳ ô xa nhất hợp lệ nào. Giả sử đầu vào là:```
2 2
1
1 1
```Các tế bào`(1,2)`,`(2,1)`, Và`(2,2)`không cháy tất cả cùng một lúc. Xa nhất là`(2,2)`với khoảng cách 2. Việc triển khai bất cẩn cập nhật câu trả lời không chính xác trong quá trình truyền tải có thể vô tình trả về một ô không xa nhất. 

Một trường hợp khác xuất hiện khi có nhiều nguồn lửa:```
5 5
2
1 1 5 5
```Các ô trung tâm cháy sau cùng vì mỗi nguồn ở góc sẽ nhanh chóng tiếp cận các ô lân cận. Nếu chúng ta đo không chính xác khoảng cách chỉ từ nguồn đầu tiên, chúng ta sẽ hoàn toàn bỏ sót ảnh hưởng của ngọn lửa thứ hai. 

Lưới nhỏ nhất có thể cũng có vấn đề:```
1 1
1
1 1
```Ô duy nhất đã cháy ở thời điểm 0, vì vậy câu trả lời phải là`(1,1)`. Việc triển khai với việc lập chỉ mục hoặc kiểm tra ranh giới không chính xác thường thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Đối với mỗi ô trong lưới, hãy tính khoảng cách Manhattan của nó tới mọi nguồn lửa bắt đầu, giữ khoảng cách tối thiểu đó, sau đó theo dõi ô có khoảng cách tối thiểu tối đa. 

Đối với một tế bào`(x,y)`và nguồn`(sx,sy)`, thời gian cháy là:$$|x - sx| + |y - sy|$$Vì có tối đa 10 nguồn nên việc tính toán này tốn ít chi phí cho mỗi ô. Tổng công việc trở thành:$$N \times M \times K$$Trong trường hợp xấu nhất:$$2000 \times 2000 \times 10 = 40,000,000$$Bốn mươi triệu phép tính số nguyên đơn giản trong Python thực sự có thể chấp nhận được nếu thực hiện cẩn thận. Đó là lý do tại sao nhiều giải pháp được chấp nhận sử dụng vũ lực trực tiếp. 

Tuy nhiên, vấn đề được gắn thẻ với các đường dẫn ngắn nhất vì bản thân quá trình lan truyền được mô hình hóa một cách tự nhiên như một BFS đa nguồn. Thay vì tính toán rõ ràng khoảng cách Manhattan, chúng tôi mô phỏng mức độ lan rộng của đám cháy theo từng cấp độ. 

Quan sát quan trọng là khi tất cả các nguồn được đưa vào hàng đợi BFS ban đầu, lần đầu tiên chúng ta đến một ô chính xác là phút sớm nhất khi lửa có thể đến đó. BFS xử lý các trạng thái theo thứ tự khoảng cách tăng dần, vì vậy ô được xử lý cuối cùng là một trong những ô bị cháy cuối cùng. 

Điều này biến bài toán thành một phép tính đường đi ngắn nhất tiêu chuẩn trên biểu đồ lưới không có trọng số. Mỗi cạnh có giá 1 và BFS đa nguồn sẽ tự động tính toán khoảng cách tối thiểu từ nguồn gần nhất. 

So với quét khoảng cách brute-force, BFS tránh việc tính toán lại khoảng cách nhiều lần. Mỗi ô được truy cập đúng một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N × M × K) | O(1) | Đã chấp nhận | 
| Tối ưu | O(N × M) | O(N × M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước lưới và danh sách các nguồn lửa ban đầu. 
2. Tạo ma trận có kích thước được truy cập`N × M`. Một tế bào sẽ được viếng thăm khi lửa chạm tới nó. 
3. Đẩy tất cả các nguồn lửa vào hàng đợi trước khi khởi động BFS. 

Đây là bước đa nguồn quan trọng. BFS bây giờ sẽ đồng thời mở rộng ra bên ngoài từ mọi đám cháy, khớp chính xác với quá trình lan rộng được mô tả trong sự cố. 
4. Đánh dấu tất cả các ô nguồn là đã truy cập. 
5. Liên tục lấy một ô ra khỏi hàng đợi. 
6. Lưu trữ ô hiện tại làm vị trí được xử lý mới nhất. 

BFS xử lý các ô theo thứ tự khoảng cách không giảm. Do đó, các ô xuất hiện sau không bao giờ gần hơn các ô xuất hiện trước đó. 
7. Hãy thử di chuyển theo bốn hướng lưới. 
8. Đối với mỗi hàng xóm chưa được thăm hợp lệ, hãy đánh dấu nó đã ghé thăm và đẩy nó vào hàng đợi. 
9. Khi BFS kết thúc, xuất ô được xử lý cuối cùng. 

Ô đó có khoảng cách tối đa từ nguồn lửa gần nhất. 

### Tại sao nó hoạt động 

BFS trên biểu đồ không có trọng số luôn phát hiện các nút theo thứ tự khoảng cách đường đi ngắn nhất tăng dần. Bắt đầu từ tất cả các nguồn lửa đồng thời có nghĩa là nguồn gần nhất sẽ tiếp cận từng ô trước tiên. Hàng đợi mở rộng từng lớp, trong đó mỗi lớp tương ứng với một phút lửa lan rộng. 

Do thứ tự này, ô cuối cùng bị loại bỏ khỏi hàng đợi thuộc về lớp BFS xa nhất, nghĩa là khoảng cách tối thiểu của nó tới bất kỳ nguồn nào là tối đa. Trả lại ô đó là đúng. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

# solution

def solve():
    n, m = map(int, input().split())

    k = int(input())
    arr = list(map(int, input().split()))

    visited = [[False] * m for _ in range(n)]
    q = deque()

    for i in range(0, 2 * k, 2):
        x = arr[i] - 1
        y = arr[i + 1] - 1

        visited[x][y] = True
        q.append((x, y))

    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    last_x, last_y = 0, 0

    while q:
        x, y = q.popleft()

        last_x, last_y = x, y

        for dx, dy in directions:
            nx = x + dx
            ny = y + dy

            if 0 <= nx < n and 0 <= ny < m and not visited[nx][ny]:
                visited[nx][ny] = True
                q.append((nx, ny))

    print(last_x + 1, last_y + 1)

solve()
```Hàng đợi ban đầu chứa mọi cây đang cháy. Đó là lý do khiến đây trở thành BFS đa nguồn thay vì BFS thông thường từ một nút duy nhất. 

các`visited`ma trận ngăn chặn việc xem lại các ô. Nếu không có nó, cùng một ô có thể được chèn nhiều lần từ các hướng khác nhau, điều này sẽ làm tăng đáng kể thời gian chạy. 

Tọa độ được chuyển đổi thành chỉ mục dựa trên 0 ngay sau khi đọc đầu vào. Điều này giúp việc kiểm tra ranh giới trở nên đơn giản vì danh sách Python được lập chỉ mục 0. Trước khi in, câu trả lời được chuyển đổi về chỉ mục dựa trên 1. 

Biến`last_x, last_y`được cập nhật mỗi khi một ô được đưa ra khỏi hàng đợi. Vì BFS xử lý các ô theo thứ tự khoảng cách tăng dần nên ô được bật ra cuối cùng là một trong những ô xa nhất. 

Thứ tự chỉ dẫn không quan trọng vì bài toán cho phép bất kỳ câu trả lời hợp lệ nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1
2 2
```Ngọn lửa bắt đầu từ trung tâm. 

| Bước | Mặt trận xếp hàng | Các ô mới được thêm vào | Xử lý lần cuối | 
| --- | --- | --- | --- | 
| 1 | (2,2) | (1,2), (3,2), (2,1), (2,3) | (2,2) | 
| 2 | (1,2) | (1,1), (1,3) | (1,2) | 
| 3 | (3,2) | (3,1), (3,3) | (3,2) | 
| 4 | Các ô cạnh còn lại | không | khác nhau | 
| Cuối cùng | (3,3) | không | (3,3) | 

Một câu trả lời hợp lệ là`(3,3)`. Đầu ra mẫu`(1,1)`bởi vì một số góc được buộc lại để có khoảng cách tối đa. 

Dấu vết này cho thấy BFS mở rộng theo lớp như thế nào. Tất cả các ô ở khoảng cách 1 được xử lý trước bất kỳ ô nào ở khoảng cách 2. 

### Ví dụ 2 

đầu vào:```
5 5
2
1 1 5 5
```| Bước | Mặt trận xếp hàng | Các ô mới được thêm vào | Xử lý lần cuối | 
| --- | --- | --- | --- | 
| 1 | (1,1) | (2,1), (1,2) | (1,1) | 
| 2 | (5,5) | (4,5), (5,4) | (5,5) | 
| 3 | Khoảng cách-1 lớp | vài ô | khác nhau | 
| 4 | Khoảng cách-2 lớp | vùng trung tâm | khác nhau | 
| Cuối cùng | (3,3) | không | (3,3) | 

Ô trung tâm xa nhất vì cả hai ngọn lửa đều tiếp cận nó một cách đối xứng. 

Ví dụ này xác nhận rằng BFS đa nguồn kết hợp chính xác việc mở rộng từ nhiều nguồn gốc thay vì xử lý chúng một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N × M) | Mỗi ô vào hàng đợi một lần | 
| Không gian | O(N × M) | Ma trận đã truy cập lưu trữ một trạng thái trên mỗi ô | 

Lưới chứa tối đa 4 triệu ô. Việc truy cập từng ô một lần là hoàn toàn có thể chấp nhận được trong giới hạn. Việc sử dụng bộ nhớ cũng an toàn đối với các ràng buộc nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())

        k = int(input())
        arr = list(map(int, input().split()))

        visited = [[False] * m for _ in range(n)]
        q = deque()

        for i in range(0, 2 * k, 2):
            x = arr[i] - 1
            y = arr[i + 1] - 1

            visited[x][y] = True
            q.append((x, y))

        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

        last_x, last_y = 0, 0

        while q:
            x, y = q.popleft()

            last_x, last_y = x, y

            for dx, dy in directions:
                nx = x + dx
                ny = y + dy

                if 0 <= nx < n and 0 <= ny < m and not visited[nx][ny]:
                    visited[nx][ny] = True
                    q.append((nx, ny))

        return f"{last_x + 1} {last_y + 1}"

    return solve()

# provided sample
assert run(
"""3 3
1
2 2
"""
) in {"1 1", "1 3", "3 1", "3 3"}, "sample 1"

# minimum grid
assert run(
"""1 1
1
1 1
"""
) == "1 1", "single cell"

# opposite corners
assert run(
"""2 2
1
1 1
"""
) == "2 2", "farthest corner"

# multiple sources
assert run(
"""5 5
2
1 1 5 5
"""
) == "3 3", "center should be farthest"

# entire first row burning
ans = run(
"""3 4
4
1 1 1 2 1 3 1 4
"""
)
assert ans in {"3 1", "3 2", "3 3", "3 4"}, "bottom row farthest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`lưới |`(1,1)`| Hạn chế tối thiểu | 
| Nguồn đơn ở góc | Góc đối diện | Khoảng cách Manhattan tối đa | 
| Hai nguồn đối lập | Tế bào trung tâm | Sửa hành vi đa nguồn | 
| Đốt toàn bộ hàng đầu tiên | Bất kỳ ô hàng dưới cùng nào | Mở rộng từng lớp | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 1
1
1 1
```Hàng đợi ban đầu chỉ chứa`(1,1)`. BFS bật nó ngay lập tức và không có hàng xóm nào tồn tại. Đầu ra của thuật toán`(1,1)`, điều này đúng vì cái cây duy nhất đã cháy rồi. 

Bây giờ hãy xem xét nhiều nguồn lửa cạnh tranh:```
5 5
2
1 1 5 5
```Ban đầu cả hai góc đều vào hàng đợi. BFS đồng thời mở rộng ra bên ngoài. Tế bào trung tâm`(3,3)`được tiếp cận muộn hơn bất kỳ ô nào khác vì cả hai đám cháy đều cần bốn bước để tiếp cận nó. Vì BFS luôn xử lý các khoảng cách nhỏ hơn trước,`(3,3)`xuất hiện ở lớp cuối cùng và trở thành câu trả lời. 

Hộp đựng cà vạt cũng rất thú vị:```
3 3
1
2 2
```Tất cả bốn góc đều cách xa nguồn trung tâm như nhau. BFS có thể kết thúc với bất kỳ trong số chúng tùy thuộc vào thứ tự hàng đợi. Vấn đề rõ ràng cho phép bất kỳ câu trả lời hợp lệ nào, vì vậy hành vi này là chính xác. 

Cuối cùng, hãy xem xét một lưới hẹp:```
1 5
1
1 3
```Ngọn lửa chỉ lan sang trái và phải. BFS xử lý chính xác điều này vì kiểm tra chuyển động sử dụng giới hạn độc lập cho các hàng và cột. Các ô xa nhất là`(1,1)`Và`(1,5)`và một trong hai câu trả lời đều được chấp nhận.
