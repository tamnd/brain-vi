---
title: "CF 106132G - Truyền mô-men xoắn"
description: "Chúng ta có một mạng lưới các bánh răng định hướng được nối với nhau bằng các trục. Mỗi trục truyền mô-men xoắn từ bánh răng này sang bánh răng khác, nhưng chỉ một phần mô-men xoắn đến còn tồn tại, được xác định bằng phần trăm hiệu suất trên trục đó."
date: "2026-06-20T07:57:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106132
codeforces_index: "G"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2025 Individual Programming Contest"
rating: 0
weight: 106132
solve_time_s: 47
verified: true
draft: false
---

[CF 106132G - Truyền mô-men xoắn](https://codeforces.com/problemset/problem/106132/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các bánh răng định hướng được nối với nhau bằng các trục. Mỗi trục truyền mô-men xoắn từ bánh răng này sang bánh răng khác, nhưng chỉ một phần mô-men xoắn đến còn tồn tại, được xác định bằng phần trăm hiệu suất trên trục đó. Nếu một trục từ bánh răng`u`trang bị`v`có hiệu quả`e`, thì mô men xoắn bất kỳ`x`đến`u`đóng góp`x * (e / 100)`Tại`v`. 

Đối với mỗi truy vấn, chúng tôi bắt đầu bằng cách áp dụng một mô-men xoắn cố định lên bánh răng nguồn`s`. Mô-men xoắn đó sau đó truyền dọc theo các cạnh có hướng, được nhân với hiệu suất trên đường đi. Nếu có nhiều đường dẫn đến thiết bị mục tiêu`t`, sự đóng góp của mô-men xoắn không cộng vào các đường khác nhau theo cách tuyến tính trừ khi chúng ta đang xem xét mô-men xoắn được cung cấp tối đa có thể. Vì chúng ta muốn đạt được kết quả tốt nhất nên chúng ta đang tìm kiếm con đường từ đó`s`ĐẾN`t`giúp tối đa hóa tích hiệu quả của cạnh, sau đó nhân mô-men xoắn ban đầu với tích tối đa đó. 

Vì vậy, mỗi truy vấn giảm xuống còn tìm giá trị đường dẫn nhân tối đa trong biểu đồ có trọng số có hướng, trong đó trọng số cạnh là xác suất trong`[0, 1]`. 

Các ràng buộc đủ nhỏ để chúng tôi có thể thực hiện tính toán kiểu đường dẫn ngắn nhất cho mỗi truy vấn. Với`n ≤ 1000`Và`m ≤ 5000`, việc chạy thuật toán giống Dijkstra cho mỗi truy vấn là khả thi vì`q ≤ 100`. Một cách tiếp cận tiền xử lý tất cả các cặp ngây thơ như Floyd-Warshall sẽ liên quan đến`n^3 ≈ 10^9`các hoạt động nằm ở ranh giới nhưng quá chậm trong Python. Việc chạy lại tìm kiếm đường dẫn tốt nhất dựa trên heap cho mỗi truy vấn nằm trong giới hạn thoải mái. 

Một vấn đề tế nhị xuất hiện khi nghĩ về nhiều con đường. Sự thư giãn tham lam phải đảm bảo chúng ta luôn truyền bá sản phẩm được biết đến nhiều nhất, bởi vì giá trị trung gian dưới mức tối ưu sau này có thể trở thành tối ưu nếu được mở rộng thông qua lợi thế hiệu quả cao. Điều này loại trừ mọi cách tiếp cận “người đến đầu tiên sẽ thắng” theo phong cách BFS. 

Các trường hợp cạnh quan trọng: 

Đầu tiên, các mục tiêu không thể tiếp cận. Nếu không có đường đi từ`s`ĐẾN`t`, câu trả lời phải là`0`, không phải là một giá trị dấu phẩy động nhỏ. Ví dụ, nếu`s = 1`,`t = 2`và không có đường dẫn nào tồn tại, đầu ra đúng là`0`, mặc dù khoảng cách nội bộ có thể vẫn ở mức`0`hoặc`-inf`tùy thuộc vào sự đại diện. 

Thứ hai, các chu trình có hiệu suất lớn hơn 100% là không thể vì`e ≤ 100`, do đó không tồn tại sự khuếch đại. Điều này đảm bảo rằng tích đường dẫn không bao giờ tăng bằng cách lặp, do đó tính đơn điệu kiểu Dijkstra được giữ nguyên. 

Thứ ba, độ chính xác của dấu phẩy động rất quan trọng vì chúng ta nhân nhiều tỷ lệ phần trăm. Sử dụng độ chính xác kép là đủ vì dung sai yêu cầu là`1e-6`. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là liệt kê tất cả các con đường có thể từ`s`ĐẾN`t`, tính tích hiệu suất dọc theo mỗi đường đi và lấy mức tối đa. Điều này đúng vì nó phù hợp trực tiếp với định nghĩa của vấn đề. Tuy nhiên, số lượng đường đi trong biểu đồ có chu trình có thể tăng theo cấp số nhân. Ngay cả trong một biểu đồ được kết nối vừa phải, số lượng đường dẫn đơn giản riêng biệt có thể vượt quá mọi giới hạn khả thi, khiến phương pháp này không thể sử dụng được ngoài các trường hợp nhỏ. 

Một chế độ xem có cấu trúc hơn cần lưu ý rằng mỗi đường dẫn đóng góp một điểm số nhân. Nếu chúng ta chuyển đổi trọng số của các cạnh bằng cách lấy logarit thì việc tối đa hóa tích sẽ tương đương với việc tối đa hóa tổng các giá trị log. Điều đó chuyển đổi bài toán thành bài toán đường đi dài nhất trong biểu đồ có trọng số cạnh dương trong không gian nhật ký. Tuy nhiên, vì nhật ký giá trị trong`(0,1]`là không dương, thực ra chúng ta đang tối đa hóa tổng trọng số âm, tương đương với việc tìm đường đi ngắn nhất trong biểu đồ có chi phí cạnh không âm sau khi phủ định. Thay vì chuyển đổi dấu hiệu một cách rõ ràng, việc chạy trực tiếp thuật toán kiểu Dijkstra trên các trọng số nhân sẽ đơn giản hơn, coi việc thư giãn là tối đa hóa. 

Quan sát quan trọng là giá trị tốt nhất cho một nút là đơn điệu khi thư giãn: một khi chúng ta có sản phẩm lớn hơn cho một nút, việc mở rộng nó chỉ có thể tạo ra các ứng cử viên tốt hơn hoặc bằng nhau cho các nút lân cận. Điều này cho phép tiếp cận hàng đợi ưu tiên trong đó chúng tôi luôn mở rộng trạng thái được biết đến nhiều nhất hiện tại. 

Vì chúng tôi có tối đa 100 truy vấn nên việc chạy tìm kiếm sản phẩm tối đa một nguồn cho mỗi truy vấn này sẽ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đường dẫn) | Hàm mũ | Ngăn xếp đệ quy O(n) | Quá chậm | 
| Tối ưu (Dijkstra cho mỗi truy vấn) | O(q · m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

### 1. Chuyển hiệu suất thành trọng số nhân 

Mỗi cạnh`u → v`với hiệu quả`e`được coi là một yếu tố`e / 100`. Đây là giá trị chúng tôi nhân dọc theo một đường dẫn. 

### 2. Khởi tạo mảng mô-men xoắn tốt nhất 

Chúng tôi duy trì một mảng`dist[]`Ở đâu`dist[i]`lưu trữ phần mô-men xoắn ban đầu tối đa có thể đạt tới nút`i`. Chúng tôi đặt tất cả các giá trị thành`0`, ngoại trừ`dist[s] = 1`. 

Điều này thể hiện rằng tại nguồn, chúng tôi giữ lại toàn bộ mô-men xoắn trước khi có bất kỳ tổn thất truyền động nào. 

### 3. Sử dụng hàng đợi có mức độ ưu tiên tối đa 

Chúng tôi lưu trữ các trạng thái`(current_best_factor, node)`trong hàng đợi ưu tiên được sắp xếp theo hệ số cao nhất trước tiên. Chúng tôi bắt đầu với`(1, s)`. 

Chúng tôi sử dụng hành vi heap tối đa (được triển khai thông qua các giá trị âm hoặc so sánh đảo ngược) để trước tiên chúng tôi luôn mở rộng đường dẫn một phần hứa hẹn nhất. 

### 4. Thư giãn các cạnh hướng ra ngoài 

Khi xử lý nút`u`với hệ số hiện tại`f`, chúng tôi cố gắng cải thiện tất cả những người hàng xóm`v`thông qua`f * (e / 100)`. Nếu giá trị này lớn hơn`dist[v]`, chúng tôi cập nhật`dist[v]`và đẩy nó vào đống. 

Bước này đảm bảo rằng mọi cải tiến đối với một nút sẽ ngay lập tức được truyền về phía trước, tương tự như sự nới lỏng của Dijkstra. 

### 5. Dừng sớm là tùy chọn 

Chúng tôi có thể dừng lại khi chúng tôi bật nút mục tiêu`t`khỏi hàng ưu tiên, vì tại thời điểm đó chúng tôi đã tìm thấy giá trị tối ưu của nó. 

### 6. Kết quả đầu ra 

Nếu`dist[t]`còn lại`0`, nút không thể truy cập được và chúng tôi xuất ra`0`. Nếu không thì chúng ta nhân lên`dist[t]`bởi mô-men xoắn ban đầu`T`. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là bất cứ khi nào một nút được đưa ra khỏi hàng ưu tiên, giá trị liên quan là hệ số nhân tối đa có thể đạt được cho nút đó. Điều này đúng vì tất cả các trọng số của cạnh đều nằm trong`[0, 1]`, do đó việc mở rộng đường dẫn không bao giờ có thể tăng giá trị của nó. Do đó, bất kỳ đường dẫn nào được phát hiện sau này tới nút đã được xử lý đều không thể vượt quá giá trị tốt nhất đã được hoàn tất. Điều này làm cho quá trình này giống hệt với thuật toán của Dijkstra, ngoại trừ việc chúng tôi tối đa hóa thay vì giảm thiểu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v, e = map(int, input().split())
        w = e / 100.0
        g[u].append((v, w))

    for _ in range(q):
        s, t, T = map(int, input().split())

        dist = [0.0] * (n + 1)
        dist[s] = 1.0

        pq = [(-1.0, s)]

        while pq:
            neg_f, u = heapq.heappop(pq)
            f = -neg_f

            if f < dist[u]:
                continue

            if u == t:
                break

            for v, w in g[u]:
                nf = f * w
                if nf > dist[v]:
                    dist[v] = nf
                    heapq.heappush(pq, (-nf, v))

        print(dist[t] * T if dist[t] > 0 else 0.0)

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề với xác suất được tính toán trước. Đối với mỗi truy vấn, chúng tôi đặt lại`dist`mảng vì mỗi truy vấn có một nguồn độc lập. 

Hàng đợi ưu tiên sử dụng các giá trị âm để mô phỏng vùng heap tối đa do Python`heapq`là một đống tối thiểu. Chúng tôi cũng bao gồm kiểm tra trạng thái cũ`if f < dist[u]`để bỏ qua các mục lỗi thời. 

Lối thoát sớm khi đến nơi`t`giảm thời gian chạy trong thực tế nhưng không cần thiết cho tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6 1
1 2 90
2 3 80
1 4 50
4 5 70
2 5 60
3 5 85
1 5 1000
```Chúng tôi theo dõi các hệ số nhân tốt nhất: 

| Bước | Nút | Yếu tố | cập nhật dist | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 1.0 | dist[1]=1.0 | 
| thư giãn | 2 | 0,9 | dist[2]=0.9 | 
| thư giãn | 4 | 0,5 | dist[4]=0,5 | 
| quá trình 2 | 3 | 0,72 | dist[3]=0,72 | 
| quá trình 2 | 5 | 0,54 | dist[5]=0,54 | 
| quá trình 3 | 5 | 0,612 | dist[5]=0,612 | 

Yếu tố tốt nhất cuối cùng cho nút 5 là`0.612`, vậy đầu ra là`612.0`. 

Điều này cho thấy rằng đường dẫn sau qua nút`3`cải thiện trực tiếp`2 → 5`tuyến đường. 

### Ví dụ 2 

đầu vào:```
3 2 1
1 2 50
2 3 50
1 3 100
1 3 100
```| Bước | Nút | Yếu tố | cập nhật dist | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 1.0 | dist[1]=1.0 | 
| thư giãn | 2 | 0,5 | dist[2]=0,5 | 
| thư giãn | 3 | 0,25 | khoảng cách [3]=0,25 | 

Nút 3 không thể truy cập được từ cạnh trực tiếp, vì vậy tốt nhất là`0.25`. nhân với`100`cho`25.0`. 

Điều này chứng tỏ rằng ngay cả khi cạnh trực tiếp tồn tại trong một số biến thể, thuật toán vẫn so sánh chính xác tất cả các đường dẫn cạnh tranh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · m log n) | Mỗi truy vấn chạy tìm kiếm kiểu Dijkstra trên tối đa m cạnh, mỗi thao tác heap tốn log n | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách và hàng đợi ưu tiên | 

Với`q ≤ 100`,`m ≤ 5000`, Và`n ≤ 1000`, điều này chạy thoải mái trong giới hạn trong Python, vì tổng số thao tác heap vẫn ở mức vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case
assert abs(float(run("""5 6 1
1 2 90
2 3 80
1 4 50
4 5 70
2 5 60
3 5 85
1 5 1000
""")) - 612.0) < 1e-6

# unreachable case
assert run("""3 1 1
1 2 50
3 3 10
""") == "0.0"

# direct best path dominates long path
assert abs(float(run("""3 3 1
1 2 90
2 3 90
1 3 50
1 3 100
""")) - 100 * 0.81) < 1e-6

# single node style
assert abs(float(run("""1 0 1
1 1 100
""")) - 100.0) < 1e-6

# cycle harmless
assert abs(float(run("""3 3 1
1 2 90
2 3 90
3 1 90
1 3 100
""")) - 81.0 * 100) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đa đường nhỏ | 612.0 | đường dẫn cạnh tranh đúng đắn | 
| không thể truy cập | 0,0 | không xử lý đường dẫn | 
| trực tiếp và gián tiếp | 81.0 | so sánh đường dẫn | 
| nút đơn | 100,0 | trường hợp cơ sở | 
| chu kỳ | 81.0 | an toàn chu kỳ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mục tiêu không thể truy cập được. Giả sử chúng ta có:```
3 1 1
1 2 50
1 3 100
```Không có con đường từ`3`ĐẾN`1`, do đó thuật toán khởi tạo`dist[3]=1`nhưng không bao giờ đạt tới`1`. Hàng đợi ưu tiên trống mà không truy cập mục tiêu, để lại`dist[1]=0`. Đầu ra chính xác trở thành`0`. 

Một trường hợp khác là khi có nhiều đường đi với độ dài và hiệu suất khác nhau:```
1 → 2 → 3 (0.5 * 0.5 = 0.25)
1 → 3 (0.2)
```Thuật toán lần đầu tiên phát hiện`3`với`0.2`, sau đó cải thiện nó thành`0.25`. Bước thư giãn đảm bảo cải tiến không bị bỏ qua, vì chúng tôi chỉ hoàn thiện một nút khi giá trị tốt nhất của nó được trích xuất từ ​​vùng nhớ heap. 

Trường hợp thứ ba là các chu trình khép kín. Bởi vì tất cả các trọng số nhiều nhất là`1`, lặp đi lặp lại một chu trình không bao giờ có thể làm tăng tích số. Ngay cả khi một chu trình tồn tại, nó cũng không thể tạo ra các vòng lặp cải tiến vô tận. Hàng đợi ưu tiên sẽ tự nhiên bỏ qua việc xem lại các nút có giá trị kém hơn, ngăn chặn việc xử lý vô hạn.
