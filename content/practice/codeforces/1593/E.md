---
title: "CF 1593E - Người làm vườn và cây cối"
description: "Chúng tôi được cấp một cây không có rễ và phải thực hiện quá trình cắt tỉa lặp đi lặp lại. Trong một thao tác, mọi đỉnh hiện có nhiều nhất một bậc sẽ bị loại bỏ đồng thời."
date: "2026-06-10T09:08:16+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dfs-and-similar", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1593
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 748 (Div. 3)"
rating: 1600
weight: 1593
solve_time_s: 95
verified: true
draft: false
---

[CF 1593E - Người làm vườn và cái cây](https://codeforces.com/problemset/problem/1593/E) 

**Đánh giá:** 1600 
**Tags:** bạo lực, cấu trúc dữ liệu, dfs và tương tự, tham lam, triển khai, cây cối 
**Thời gian giải:** 1 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một cây không có rễ và phải thực hiện quá trình cắt tỉa lặp đi lặp lại. Trong một thao tác, mọi đỉnh hiện có nhiều nhất một bậc sẽ bị loại bỏ đồng thời. Điều đó có nghĩa là tất cả các lá đều biến mất và trong trường hợp suy biến, một đỉnh hoặc một cạnh còn lại cũng biến mất vì điểm cuối của chúng cũng được coi là các lá theo định nghĩa. 

Chúng ta lặp lại thao tác này chính xác k lần và muốn biết còn lại bao nhiêu đỉnh sau tất cả các vòng loại bỏ lá đồng thời. 

Nhận xét quan trọng từ các ràng buộc là tổng số đỉnh trên tất cả các trường hợp thử nghiệm lên tới 4 · 10^5. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào thực hiện duyệt đồ thị đầy đủ lặp đi lặp lại cho mỗi thao tác đều quá chậm, vì bản thân k có thể lớn tới 2 · 10^5 cho mỗi lần kiểm tra. Một mô phỏng đơn giản sẽ tính toán lại các lá và loại bỏ chúng từng lớp, có khả năng chạm vào tất cả các cạnh trên mỗi bước, dẫn đến hành vi O(nk) trong trường hợp xấu nhất. 

Một điểm tinh tế là quá trình này không phải là sắp xếp lại cấu trúc của các cạnh ngoài việc xóa. Khi một đỉnh bị loại bỏ, nó sẽ không bao giờ quay trở lại và việc loại bỏ xảy ra theo từng lớp từ bên ngoài về phía trung tâm. 

Trường hợp Edge phá vỡ suy nghĩ ngây thơ là những cây nhỏ: 

Một nút có k ≥ 1 sẽ ngay lập tức trống sau thao tác đầu tiên, không duy trì ở mức 1. 

Cây hai nút cũng trở nên trống sau thao tác đầu tiên, vì cả hai điểm cuối đều là các lá đồng thời. 

Cây hình ngôi sao sụp đổ nhanh chóng: sau một thao tác, chỉ còn lại phần trung tâm; sau lần thứ hai, nó biến mất. 

Những trường hợp này cho thấy rằng quá trình này về cơ bản là về khoảng cách đến “lõi” của cây chứ không chỉ về cập nhật mức độ động. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì các cấp độ, liên tục thu thập tất cả các lá hiện tại, loại bỏ chúng và cập nhật các lá lân cận. Điều này đúng vì mỗi thao tác khớp chính xác với định nghĩa. Tuy nhiên, việc tính toán lại lá được thiết lập tới k lần có thể suy biến thành việc quét liên tục các phần lớn của biểu đồ. Trong một đường đi dài, mỗi vòng chỉ loại bỏ hai đỉnh, do đó quá trình này mất O(n) vòng và mỗi vòng có thể có chi phí là O(n), tổng cộng là O(n^2). 

Thông tin chi tiết về cấu trúc quan trọng là một đỉnh tồn tại chính xác cho đến khi tất cả các đỉnh trong khoảng cách k tính từ nó bị loại bỏ. Nói cách khác, mỗi đỉnh có một “thời gian chết” bằng khoảng cách từ nó đến lá gần nhất, được đo bằng các lớp loại bỏ đồng thời. 

Nếu đảo ngược quá trình, thay vì bóc lá ra ngoài, chúng ta coi lá là điểm bắt đầu và lan truyền vào trong. Tất cả các lá chết vào thời điểm 1, các lá lân cận của chúng chết vào thời điểm 2, v.v. Đây là BFS đa nguồn trong đó tất cả các lá ban đầu bắt đầu đồng thời với khoảng cách 1. 

Do đó, bài toán trở thành tính toán cho mỗi nút khoảng cách đến lá gần nhất. Sau đó, bất kỳ nút nào có khoảng cách lớn hơn k vẫn còn. 

Điều này biến đổi vấn đề từ việc cập nhật cấu trúc lặp đi lặp lại thành một BFS duy nhất trên cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng loại bỏ lá lặp đi lặp lại | O(nk) trường hợp xấu nhất | O(n) | Quá chậm | 
| BFS đa nguồn từ lá | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính bậc của mỗi đỉnh. Điều này là cần thiết để xác định các lá ban đầu, vì các lá chính xác là các nút có bậc ≤ 1. 
2. Khởi tạo một hàng đợi với tất cả các lá và gán khoảng cách cho chúng là 1. Những đỉnh này biểu thị các đỉnh bị loại bỏ trong thao tác đầu tiên. 
3. Chạy BFS từ hàng đợi này. Khi xử lý một nút u, hãy truyền tới các nút lân cận v chưa được ấn định khoảng cách. Gán dist[v] = dist[u] + 1. Mô hình này v trở thành một chiếc lá sau khi tất cả các đỉnh ở các lớp bên ngoài đã bị loại bỏ. 
4. Sau khi BFS hoàn thành, mỗi đỉnh có một giá trị dist[v], đại diện cho vòng mà nó bị loại bỏ. 
5. Đếm xem có bao nhiêu đỉnh thỏa mãn dist[v] > k. Đây là các đỉnh tồn tại sau k thao tác cắt tỉa đầy đủ. 

Lý do điều này có tác dụng là vì mỗi lớp BFS tương ứng chính xác với một đợt loại bỏ lá. Ban đầu, chỉ có lá thật mới có thể tháo rời được. Sau khi chúng bị loại bỏ, một số đỉnh bên trong sẽ trở thành các lá mới, chính xác là biên giới BFS tiếp theo. Vì tất cả các lần xóa xảy ra đồng thời trong mỗi vòng, BFS mô hình hóa chính xác việc mở rộng song song độ sâu loại bỏ. 

Điều bất biến là khi bắt đầu BFS cấp d, tất cả các đỉnh có dist ≤ d đã được xác định là có thể tháo rời trong các phép toán d và tất cả các đỉnh chưa được thăm dò còn lại đều nằm sâu hơn trong lõi của cây. Điều này đảm bảo tính chính xác vì cấu trúc cây đảm bảo không tồn tại đường dẫn loại bỏ ngắn hơn thay thế nào khác ngoài việc di chuyển vào trong từ một lá. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    t = int(input())
    for _ in range(t):
        line = input().strip()
        while line == "":
            line = input().strip()
        n, k = map(int, line.split())

        adj = [[] for _ in range(n)]
        deg = [0] * n

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)
            deg[u] += 1
            deg[v] += 1

        dist = [-1] * n
        q = deque()

        for i in range(n):
            if deg[i] <= 1:
                dist[i] = 1
                q.append(i)

        remaining = n

        while q:
            u = q.popleft()
            if dist[u] > k:
                continue
            remaining -= 1
            for v in adj[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)

        print(remaining)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cấu trúc cây, trong khi độ xác định các lá ban đầu. Hàng đợi BFS được gieo tất cả các lá cùng một lúc, điều này đảm bảo lớp đầu tiên tương ứng với thao tác cắt tỉa đầu tiên. 

Mảng khoảng cách mã hóa thời gian loại bỏ. Giá trị 1 có nghĩa là bị xóa sau thao tác đầu tiên, 2 sau thao tác thứ hai, v.v. 

Bộ đếm còn lại theo dõi xem có bao nhiêu đỉnh vẫn chưa bị xóa sau k thao tác. Chúng tôi chỉ giảm nó khi chúng tôi xử lý các nút trong BFS cho đến thời điểm k. 

Một điểm tinh tế là xử lý các dòng trống giữa các trường hợp thử nghiệm; định dạng đầu vào bao gồm chúng, vì vậy chúng tôi bỏ qua các dòng trống trước khi đọc n và k. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một cây đường dẫn nhỏ: 1-2-3-4. 

Ban đầu, lá là 1 và 4. 

| Bước | Xếp hàng | bài tập dist | đã xóa | 
| --- | --- | --- | --- | 
| ban đầu | [1,4] | 1:1, 4:1 | 0 | 
| bật 1 | [4,2] | 2:2 | 1 | 
| bật 4 | [2,3] | 3:2 | 2 | 
| bật 2 | [3] | - | 3 | 
| bật 3 | [] | - | 4 | 

Ở đây dist cho thấy 1 và 4 chết ở vòng 1, 2 và 3 ở vòng 2. Nếu k = 1 thì chỉ còn lại 2 đỉnh. 

Bây giờ hãy xem xét một ngôi sao có tâm 1 và các lá 2,3,4,5. 

| Bước | Xếp hàng | bài tập dist | đã xóa | 
| --- | --- | --- | --- | 
| ban đầu | [2,3,4,5] | tất cả các lá dist=1 | 0 | 
| lá pop | [1] | 1 được dist=2 | 4 | 
| bật 1 | [] | - | 5 | 

Điều này khẳng định tâm tồn tại lâu hơn chính xác một vòng so với lá. 

Những dấu vết này cho thấy các lớp BFS tương ứng chính xác với các vòng cắt tỉa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi đỉnh được xếp hàng đợi và xử lý một lần, còn mỗi cạnh được truy cập với số lần không đổi | 
| Không gian | O(n) | danh sách kề, mảng khoảng cách và hàng đợi | 

Trên tất cả các trường hợp thử nghiệm, tổng n được giới hạn bởi 4 · 10^5, do đó giải pháp vừa vặn trong giới hạn. BFS tuyến tính tránh việc quét toàn bộ cây nhiều lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            line = input().strip()
            while line == "":
                line = input().strip()
            n, k = map(int, line.split())

            adj = [[] for _ in range(n)]
            deg = [0] * n

            for _ in range(n - 1):
                u, v = map(int, input().split())
                u -= 1
                v -= 1
                adj[u].append(v)
                adj[v].append(u)
                deg[u] += 1
                deg[v] += 1

            dist = [-1] * n
            q = deque()

            for i in range(n):
                if deg[i] <= 1:
                    dist[i] = 1
                    q.append(i)

            remaining = n

            while q:
                u = q.popleft()
                if dist[u] > k:
                    continue
                remaining -= 1
                for v in adj[u]:
                    if dist[v] == -1:
                        dist[v] = dist[u] + 1
                        q.append(v)

            out.append(str(remaining))
        return "\n".join(out)

    return solve()

# sample 1
assert run("""6

14 1
1 2
2 3
2 4
4 5
4 6
2 7
7 8
8 9
8 10
3 11
3 12
1 13
13 14

2 200000
1 2

3 2
1 2
2 3

5 1
5 1
3 2
2 1
5 4

6 2
5 1
2 5
5 6
4 2
3 4

7 1
4 3
5 1
1 3
6 1
1 7
2 1
""") == """7
0
0
3
1
2"""

# minimum size
assert run("""1

1 10
""") == "0"

# two nodes large k
assert run("""1

2 5
1 2
""") == "0"

# line
assert run("""1

4 1
1 2
2 3
3 4
""") == "2"

# star
assert run("""1

5 2
1 2
1 3
1 4
1 5
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | loại bỏ ngay lập tức trong lần vận hành đầu tiên | 
| hai nút | 0 | cả hai điểm cuối cùng bị loại bỏ | 
| cây dòng | 2 | bong tróc lớp trong cấu trúc đường dẫn | 
| ngôi sao | 0 | trung tâm sống sót đúng một hiệp | 

## Vỏ cạnh 

Một cây đỉnh duy nhất cho thấy hành vi suy biến của hoạt động. BFS khởi tạo đỉnh đó là một lá có khoảng cách 1 và vì k ≥ 1 nên nó được tính là bị loại bỏ ngay lập tức, tạo ra đầu ra 0. 

Cây hai đỉnh cũng tương tự: ban đầu cả hai nút đều là lá, cả hai đều nhận được khoảng cách 1 và không còn đỉnh nào sau thao tác đầu tiên. BFS xử lý đồng thời cả hai, đảm bảo không có sự bất đối xứng giữa các điểm cuối. 

Một chuỗi dài thể hiện tính chất phân lớp của quá trình. Các lá ở cả hai đầu bắt đầu ở khoảng cách 1, các lá lân cận của chúng trở thành khoảng cách 2, v.v. cho đến tâm. Thuật toán chỉ định khoảng cách tăng dần một cách chính xác mà không cần mô phỏng rõ ràng từng vòng cắt tỉa.
