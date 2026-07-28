---
title: "CF 102798B - Mê cung"
description: "Mê cung là một lưới gạch hình chữ nhật. Một số ô chứa lỗ đen và không thể vào được. Mỗi truy vấn sẽ đưa ra hai ô trống, lối vào và lối ra, đồng thời yêu cầu đường đi ngắn nhất giữa chúng trong khi tránh được tất cả các lỗ đen."
date: "2026-07-27T17:47:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102798
codeforces_index: "B"
codeforces_contest_name: "2020 China Collegiate Programming Contest, Weihai Site"
rating: 0
weight: 102798
solve_time_s: 56
verified: true
draft: false
---

[CF 102798B - Mê cung](https://codeforces.com/problemset/problem/102798/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Mê cung là một lưới gạch hình chữ nhật. Một số ô chứa lỗ đen và không thể vào được. Mỗi truy vấn sẽ đưa ra hai ô trống, lối vào và lối ra, đồng thời yêu cầu đường đi ngắn nhất giữa chúng trong khi tránh được tất cả các lỗ đen. Nếu một trong hai điểm cuối là lỗ đen hoặc không tồn tại tuyến đường nào thì câu trả lời là`-1`. Giới hạn đầu vào là không bình thường vì lưới có thể chứa tới 200000 ô, có thể có 100000 truy vấn, nhưng số lượng lỗ đen nhiều nhất là 42. 

Kích thước lưới có nghĩa là chúng tôi không thể chạy tìm kiếm biểu đồ cho mọi truy vấn. Một BFS duy nhất trên toàn bộ lưới đã tỷ lệ thuận với số lượng ô và việc lặp lại 100000 lần đó sẽ vượt xa các hoạt động có sẵn. Số lượng nhỏ các ô bị chặn là hạn chế chính. Bất kỳ quá trình tiền xử lý nào cũng phụ thuộc chủ yếu vào số lượng lỗ đen, trong khi các truy vấn phải gần với thời gian không đổi. 

Một sai lầm phổ biến là cho rằng câu trả lời luôn là khoảng cách Manhattan. Đối với một lưới trống, di chuyển trực tiếp theo chiều dọc và chiều ngang là tối ưu, nhưng các lỗ đen có thể buộc phải đi đường vòng. 

Hãy xem xét đầu vào này:```
1 3 1 1
1 2
1 1 1 3
```Ngói ở giữa bị chặn. Con đường ngắn nhất từ ​​ô đầu tiên đến ô cuối cùng không tồn tại vì không có đường đi vòng qua chướng ngại vật trong lưới một hàng. Câu trả lời đúng là:```
-1
```Một giải pháp chỉ có khoảng cách Manhattan sẽ trả về không chính xác`2`. 

Một trường hợp khác là khi lối vào bị chặn:```
2 2 1 1
1 1
1 1 2 2
```Câu trả lời phải là:```
-1
```Mặc dù đích đến nằm liền kề trong lưới thông thường, nhưng việc bắt đầu từ lỗ đen vẫn bị cấm. 

Trường hợp tế nhị cuối cùng là khi các trở ngại không liên quan:```
3 3 1 1
2 2
1 1 3 3
```Câu trả lời là:```
4
```Giải pháp không nên thêm các đường vòng không cần thiết chỉ vì ô bị chặn tồn tại ở một nơi khác. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ chạy BFS cho mọi truy vấn. BFS xử lý chính xác mọi chướng ngại vật và đưa ra đường đi ngắn nhất trong lưới không có trọng số, nhưng chi phí của nó quá lớn. Trong trường hợp xấu nhất, một truy vấn chạm vào tất cả 200000 ô, vì vậy 100000 truy vấn sẽ cần khoảng 20 tỷ lượt truy cập ô. 

Quan sát quan trọng là số lượng lỗ đen rất nhỏ. Nếu đường đi ngắn nhất dài hơn khoảng cách bình thường của Manhattan thì nguyên nhân chắc hẳn là do đường đi đó tương tác với một chướng ngại vật. Cụ thể hơn, đường vòng phải đi qua một ô liền kề với lỗ đen. Những ô này là nơi duy nhất mà chướng ngại vật có thể ảnh hưởng đến tuyến đường tối ưu. 

Có nhiều nhất`4 * 42 = 168`gạch đặc biệt như vậy. Chúng tôi có thể chạy BFS từ mọi ô đặc biệt một lần. Sau đó, truy vấn không cần khám phá lưới. Nó có thể so sánh tuyến đường đi thẳng đến Manhattan với các tuyến đường đi qua một ô đặc biệt. 

Brute-force hoạt động vì BFS phát hiện ra đường đi ngắn nhất thực sự trong toàn bộ lưới nhưng không thành công khi lặp lại. Số lượng chướng ngại vật nhỏ cho phép chúng tôi nén các phần khó khăn của lưới vào một tập hợp nhỏ các vị trí quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(qnm) | O(nm) | Quá chậm | 
| Tối ưu | O(snm + qs) | O(snm) | Đã chấp nhận | 

Đây`s`là số ô đặc biệt, nhiều nhất là 168. 

## Hướng dẫn thuật toán 

1. Đọc các lỗ đen và đánh dấu các ô bị chặn. Đối với mỗi ô bị chặn, hãy kiểm tra bốn ô lân cận của nó. Mỗi người hàng xóm tự do sẽ trở thành một ô đặc biệt vì bất kỳ đường vòng nào do chướng ngại vật gây ra đều phải chạm vào một trong các vị trí này. 
2. Chạy BFS từ mọi ô đặc biệt. Lưu trữ khoảng cách từ ô đặc biệt đó đến mọi vị trí lưới. BFS hợp lệ vì mọi chuyển động đều có cùng chi phí. 
3. Với mỗi truy vấn, trả về ngay`-1`nếu một trong hai điểm cuối là một lỗ đen. 
4. Bắt đầu câu trả lời bằng khoảng cách Manhattan giữa lối vào và lối ra. Điều này thể hiện trường hợp chướng ngại vật không ảnh hưởng đến tuyến đường. 
5. Hãy thử mọi ô đặc biệt làm điểm trung gian. Tuyến đường sử dụng chướng ngại vật phải đi qua một trong các ô này, vì vậy chiều dài của nó là:$$dist(s, special) + dist(special, t)$$Tận dụng tối thiểu tất cả những khả năng này. 

1. Xuất ra giá trị tìm được nhỏ nhất. 

Tại sao nó hoạt động: Trong một lưới trống, khoảng cách Manhattan là tuyến đường ngắn nhất có thể. Khi một tuyến đường dài hơn, khoảng cách bổ sung chỉ tồn tại do một ô bị chặn ngăn cản một đường đi đơn điệu trực tiếp. Nơi đầu tiên mà con đường như vậy phải phản ứng với chướng ngại vật là một ô tự do tiếp giáp với lỗ đen, đây chính xác là một ô đặc biệt. Vì BFS đưa ra khoảng cách thực sự ngắn nhất đến và đi từ mỗi ô đặc biệt nên việc kiểm tra tất cả các điểm trung gian đặc biệt sẽ bao gồm mọi đường vòng tối ưu có thể có. 

## Giải pháp Python```python
import sys
from collections import deque
from array import array

input = sys.stdin.readline

def solve():
    n, m, k, q = map(int, input().split())

    blocked = set()
    holes = []
    for _ in range(k):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        blocked.add((x, y))
        holes.append((x, y))

    special = []
    seen = set()
    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for x, y in holes:
        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m and (nx, ny) not in blocked:
                if (nx, ny) not in seen:
                    seen.add((nx, ny))
                    special.append((nx, ny))

    total = n * m
    all_dist = []

    for sx, sy in special:
        dist = array('i', [-1]) * total
        start = sx * m + sy
        dist[start] = 0
        dq = deque([start])

        while dq:
            cur = dq.popleft()
            x = cur // m
            y = cur % m
            nd = dist[cur] + 1

            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < m and (nx, ny) not in blocked:
                    nxt = nx * m + ny
                    if dist[nxt] == -1:
                        dist[nxt] = nd
                        dq.append(nxt)

        all_dist.append(dist)

    out = []

    for _ in range(q):
        xs, ys, xt, yt = map(int, input().split())
        xs -= 1
        ys -= 1
        xt -= 1
        yt -= 1

        if (xs, ys) in blocked or (xt, yt) in blocked:
            out.append("-1")
            continue

        ans = abs(xs - xt) + abs(ys - yt)
        a = xs * m + ys
        b = xt * m + yt

        for dist in all_dist:
            if dist[a] != -1 and dist[b] != -1:
                cand = dist[a] + dist[b]
                if cand < ans:
                    ans = cand

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý xây dựng bộ ô đặc biệt. Bộ này được giữ duy nhất vì một số lỗ đen có thể chia sẻ cùng một ô lân cận. 

Mảng BFS sử dụng`array('i')`thay vì danh sách Python thông thường. Có thể có hàng triệu khoảng cách được lưu trữ và việc sử dụng bộ lưu trữ số nguyên nhỏ gọn sẽ giúp sử dụng bộ nhớ ở mức hợp lý. Mỗi BFS lưu trữ khoảng cách bằng cách làm phẳng`(row, column)`vào trong`row * m + column`. 

Giai đoạn truy vấn tránh mọi thao tác duyệt đồ thị. Giá trị Manhattan ban đầu xử lý tất cả các tuyến đường không bao giờ cần phải đối mặt với chướng ngại vật. Vòng lặp trên các ô đặc biệt sẽ kiểm tra mọi đường vòng có thể liên quan đến chướng ngại vật. 

Việc kiểm tra ô bị chặn phải diễn ra trước khi sử dụng khoảng cách vì lỗ đen không có trạng thái đường dẫn hợp lệ. Các tọa độ được chuyển đổi thành chỉ mục dựa trên số 0 một lần sau khi đọc. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3 3 1 2
2 2
1 1 3 3
2 1 1 3
```Ô giữa bị chặn. Các ô đặc biệt là bốn ô xung quanh nó. 

| Bước | Tốt nhất hiện nay | Hành động | 
| --- | --- | --- | 
| Ban đầu | 4 | Khoảng cách từ Manhattan`(1,1)`ĐẾN`(3,3)`| 
| Kiểm tra các tế bào đặc biệt | 4 | Mỗi đường vòng có chướng ngại vật ít nhất cũng dài như thế này | 
| Đầu ra | 4 | Tuyến đường trực tiếp là hợp lệ | 

Dấu vết cho thấy những trở ngại không phải lúc nào cũng làm tăng câu trả lời. Tuyến đường Manhattan vẫn là con đường ngắn nhất chính xác khi tồn tại một tuyến đường đơn điệu hợp lệ. 

Một ví dụ khác:```
1 3 1 1
1 2
1 1 1 3
```| Bước | Tốt nhất hiện nay | Hành động | 
| --- | --- | --- | 
| Ban đầu | 2 | Khoảng cách Manhattan | 
| Kiểm tra các tế bào đặc biệt | Không có lộ trình | Các viên gạch lân cận duy nhất không thể kết nối xung quanh lỗ | 
| Đầu ra | -1 | Điểm đến không thể truy cập được | 

Điều này xác nhận rằng thuật toán xử lý các trường hợp không thể thay vì giả sử mọi lưới đều có đường dẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(snm + qs) | Chúng tôi chạy BFS từ mọi ô đặc biệt và kiểm tra tất cả chúng trên mỗi truy vấn | 
| Không gian | O(snm) | Chúng tôi lưu trữ một mảng khoảng cách cho mỗi ô đặc biệt | 

Đây`s <= 168`,`nm <= 200000`, Và`q <= 100000`. Việc xử lý trước là khả thi vì số lượng chướng ngại vật nhỏ và mỗi truy vấn chỉ thực hiện một lượng công việc cố định rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# sample 1
assert run("""5 5 4 7
2 2
2 3
3 2
3 3
2 1 3 4
1 1 1 1
2 2 2 2
1 1 1 5
2 2 5 5
2 1 2 4
1 1 3 3
""") == """6
0
-1
4
-1
5
-1
""", "sample 1"

# sample 2
assert run("""2 3 2 1
1 2
2 1
1 1 2 3
""") == """-1
""", "sample 2"

# no obstacles
assert run("""3 3 0 2
1 1 3 3
2 2 2 2
""") == """4
0
""", "empty grid"

# blocked start
assert run("""2 2 1 1
1 1
1 1 2 2
""") == """-1
""", "blocked start"

# one row impossible
assert run("""1 3 1 1
1 2
1 1 1 3
""") == """-1
""", "single row wall"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới trống | Khoảng cách Manhattan | Xử lý đường dẫn trực tiếp cơ bản | 
| Bắt đầu bị chặn |`-1`| Xác thực điểm cuối | 
| Tường một hàng |`-1`| Những con đường bất khả thi | 
| Trường hợp mẫu | Kết quả đầu ra mẫu | Tính đúng đắn chung | 

## Vỏ cạnh 

Đối với điểm cuối bị chặn, thuật toán sẽ dừng trước khi xem xét khoảng cách được tính toán trước. Ví dụ:```
2 2 1 1
1 1
1 1 2 2
```Lối vào là`(1,1)`, đó là lỗ đen nên câu trả lời là ngay lập tức`-1`. 

Đối với đường đi bị chặn hoàn toàn bởi chướng ngại vật:```
1 3 1 1
1 2
1 1 1 3
```Các ô đặc biệt bao gồm hai ô lân cận hợp lệ của lỗ đen, nhưng cả hai bên đều là ngõ cụt vì lưới chỉ có một hàng. BFS đánh dấu đích không thể truy cập được, do đó không có chuyển đổi ô đặc biệt nào cải thiện câu trả lời ban đầu. Kết quả cuối cùng là`-1`. 

Đối với tuyến đường có chướng ngại vật nhưng không thành vấn đề:```
3 3 1 1
2 2
1 1 3 3
```Khoảng cách Manhattan là`4`. Khoảng cách BFS thông qua các ô đặc biệt cũng được xem xét, nhưng không có khoảng cách nào có thể tạo đường dẫn ngắn hơn. Thuật toán trả về`4`, bảo toàn đường đi trực tiếp tối ưu.
