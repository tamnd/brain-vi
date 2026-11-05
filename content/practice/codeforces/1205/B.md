---
title: "CF 1205B - ​​Chu kỳ ngắn nhất"
description: "Chúng ta được cung cấp một tập hợp các số nguyên và chúng ta hiểu mỗi số nguyên là một nút trong biểu đồ. Hai nút được kết nối khi bit AND của chúng khác 0, nghĩa là chúng chia sẻ ít nhất một bit chung được đặt thành 1."
date: "2026-06-11T23:34:25+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1205
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 580 (Div. 1)"
rating: 1900
weight: 1205
solve_time_s: 101
verified: true
draft: false
---

[CF 1205B - Chu kỳ ngắn nhất](https://codeforces.com/problemset/problem/1205/B) 

**Xếp hạng:** 1900 
**Tags:** bitmasks, brute Force, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên và chúng ta hiểu mỗi số nguyên là một nút trong biểu đồ. Hai nút được kết nối khi bit AND của chúng khác 0, nghĩa là chúng chia sẻ ít nhất một bit chung được đặt thành 1. 

Nhiệm vụ là xác định độ dài của chu kỳ ngắn nhất trong biểu đồ này hoặc báo cáo rằng không tồn tại chu kỳ nào. 

Đồ thị có thể rất lớn xét về các cạnh thế năng, vì mọi cặp số đều có thể được kết nối. Tuy nhiên, cấu trúc khóa bị ẩn trong biểu diễn bit: mỗi số có tối đa 60 bit liên quan và các cạnh chỉ tồn tại thông qua các bit được chia sẻ. Điều này ngay lập tức cho thấy rằng việc suy luận trực tiếp trên tất cả các cặp là không thể và chúng ta phải nén cấu trúc bằng cách sử dụng các bit. 

Ràng buộc n lên tới 100.000 sẽ loại bỏ mọi cấu trúc O(n²) hoặc BFS trên các cạnh rõ ràng. Ngay cả O(n√n) cũng quá lớn nếu được triển khai một cách đơn giản với các hệ số hằng số nặng. Một giải pháp đúng phải làm giảm đồ thị một cách đáng kể, thường bằng cách khai thác thực tế là các cạnh được tạo ra bởi một vũ trụ bit nhỏ. 

Một trường hợp cạnh tinh tế xuất hiện khi có một tam giác được hình thành bởi ba số chia sẻ các bit khác nhau theo cặp, mặc dù cả ba số này không có bit nào được chia sẻ. Một tình huống phức tạp khác là khi nhiều số chia sẻ cùng một bit đơn, có thể ngay lập tức tạo thành một chu kỳ rất nhỏ có độ dài 3 nếu tồn tại ít nhất ba số như vậy. 

Một cách tiếp cận đơn giản là xây dựng biểu đồ đầy đủ hoặc thậm chí kiểm tra tất cả các cặp để tìm AND ≠ 0 sẽ không thành công không chỉ do giới hạn thời gian mà còn do hết bộ nhớ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi xây dựng biểu đồ một cách rõ ràng: với mỗi cặp nút i và j, chúng tôi kiểm tra xem a_i AND a_j có khác 0 hay không và nếu vậy thì chúng tôi thêm một cạnh. Sau đó, chúng tôi chạy BFS từ mọi nút để tính chu kỳ ngắn nhất, coi mỗi BFS là đường đi ngắn nhất quay lại chính nó. Điều này đúng vì BFS trên biểu đồ không có trọng số tìm thấy các chu kỳ ngắn nhất qua các cạnh sau. 

Vấn đề là quy mô. Việc kiểm tra tất cả các cặp có chi phí O(n²), trong trường hợp xấu nhất đã là 10¹⁰ hoạt động. Ngay cả khi chúng tôi đã tối ưu hóa lưu trữ kề bằng cách nào đó, BFS từ mỗi nút vẫn dẫn đến việc duyệt đồ thị lặp đi lặp lại có thể chứa các cạnh O(n2). Cách tiếp cận bạo lực bị sụp đổ dưới những hạn chế về cả thời gian và bộ nhớ. 

Quan sát quan trọng là các cạnh không phải là tùy ý. Mỗi cạnh chỉ tồn tại nhờ các bit được chia sẻ và chỉ có khoảng 60 bit. Thay vì suy nghĩ theo các nút được kết nối với các nút, chúng ta có thể nghĩ theo các bit kết nối các nút. 

Nếu một bit nào đó xuất hiện trong ba số trở lên, ba nút đó sẽ ngay lập tức tạo thành một chu kỳ có độ dài 3: mỗi cặp chia sẻ bit đó, vì vậy chúng tạo thành một hình tam giác. Do đó, bất kỳ bit nào có tần số ≥ 3 đều cho câu trả lời tối ưu ngay lập tức là 3. 

Nếu không có bit nào xuất hiện ba lần thì mỗi bit xuất hiện tối đa hai số. Điều đó hạn chế nghiêm trọng đồ thị: mỗi số có tối đa 60 bit, do đó, mỗi nút có giới hạn độ là 60 và tổng số cạnh có thể quản lý được, khoảng O(60n). Trong chế độ này, chúng ta có thể xây dựng rõ ràng danh sách kề. 

Khi chúng tôi có biểu đồ thưa thớt, chúng tôi tính toán chu kỳ ngắn nhất bằng cách chạy BFS từ mọi nút, nhưng dừng sớm khi khoảng cách vượt quá câu trả lời tốt nhất được tìm thấy cho đến nay. Vì mức độ bị giới hạn nên điều này trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các cặp + BFS) | O(n2 + n·(n+m)) | O(n²) | Quá chậm | 
| Giảm dựa trên bit + BFS | O(60² · n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đếm số lần xuất hiện của từng bit trên tất cả các số. Nếu bất kỳ bit nào xuất hiện trong ít nhất ba số, hãy trả về 3 ngay lập tức, vì ba nút đó tạo thành một hình tam giác thông qua bit đó. 
2. Xóa tất cả các số 0. Chúng không đóng góp vào cạnh nào và không thể ảnh hưởng đến chu kỳ. 
3. Xây dựng danh sách kề bằng cách lặp lại từng số và kết nối nó với các số đã thấy trước đó có chung một chút. Vì mỗi bit xuất hiện nhiều nhất hai lần nên mỗi cặp được thêm vào nhiều nhất một lần. 
4. Đối với mỗi nút, hãy chạy BFS để tính toán đường đi ngắn nhất tới tất cả các nút, nhưng dừng lại nếu khoảng cách hiện tại vượt quá độ dài chu kỳ tốt nhất được tìm thấy cho đến nay. 
5. Trong BFS từ nút u, bất cứ khi nào chúng tôi gặp một nút v đã được truy cập mà không phải là nút cha, chúng tôi sẽ tìm thấy một chu trình có độ dài là dist[u][x] + dist[u][y] + 1. Cập nhật câu trả lời. 
6. Trả về độ dài chu kỳ tối thiểu được tìm thấy hoặc -1 nếu không tồn tại. 

Tại sao nó hoạt động được gắn liền với cấu trúc do bit tạo ra. Hoặc một bit được chia sẻ nhiều, ngay lập tức tạo ra một hình tam giác hoặc mỗi bit đủ thưa thớt để đồ thị cảm ứng trở nên cấp độ nhỏ và hoàn toàn có thể duyệt được. Bất kỳ chu trình nào trong biểu đồ phải xuất hiện dưới dạng một chu trình trong khám phá BFS này và BFS đảm bảo khoảng cách đường đi ngắn nhất, do đó, chu trình được phát hiện đầu tiên qua mỗi cạnh sẽ cung cấp chu trình tối thiểu liên quan đến cạnh đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque, defaultdict

INF = 10**9

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    bit_count = [0] * 61
    for x in a:
        for b in range(61):
            if x >> b & 1:
                bit_count[b] += 1
    
    if any(c >= 3 for c in bit_count):
        print(3)
        return
    
    nums = [x for x in a if x != 0]
    n = len(nums)
    
    adj = [[] for _ in range(n)]
    seen = [[] for _ in range(61)]
    
    for i, x in enumerate(nums):
        for b in range(61):
            if x >> b & 1:
                for j in seen[b]:
                    adj[i].append(j)
                    adj[j].append(i)
                seen[b].append(i)
    
    ans = INF
    
    def bfs(start):
        dist = [INF] * n
        parent = [-1] * n
        q = deque([start])
        dist[start] = 0
        
        best = INF
        
        while q:
            u = q.popleft()
            if dist[u] >= best:
                continue
            for v in adj[u]:
                if dist[v] == INF:
                    dist[v] = dist[u] + 1
                    parent[v] = u
                    q.append(v)
                elif parent[u] != v:
                    best = min(best, dist[u] + dist[v] + 1)
        return best
    
    for i in range(n):
        ans = min(ans, bfs(i))
        if ans == 3:
            break
    
    print(ans if ans < INF else -1)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách kiểm tra tình trạng tam giác tức thời thông qua tần số bit, xử lý trường hợp lỗi thực sự dày đặc duy nhất. Sau đó, nó nén biểu đồ bằng cách chỉ xây dựng danh sách kề thông qua các bit được chia sẻ, đảm bảo mỗi cạnh được phát hiện chính xác một lần thông qua “danh sách bit đã nhìn thấy” trung gian. 

Hàm BFS duy trì cả mảng khoảng cách và mảng cha. Kiểm tra gốc là cần thiết: nếu không có nó, mọi cạnh vô hướng sẽ ngay lập tức trông giống như một chu trình có độ dài 2. Vì các chu trình đơn giản phải có độ dài ít nhất là 3 trong biểu đồ vô hướng, nên việc bỏ qua việc quay lui trực tiếp là cần thiết để tránh kết quả dương tính giả. 

Mỗi BFS trả về chu kỳ ngắn nhất liên quan đến nút bắt đầu và mức tối thiểu toàn cầu được tích lũy trên tất cả các nguồn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
3 6 28 9
```Tần số bit hiển thị không có bit nào xuất hiện ba lần, vì vậy chúng tôi tiến hành xây dựng biểu đồ. 

| Bước | Nút | Bit | Đã thêm các cạnh mới | 
| --- | --- | --- | --- | 
| 0 | 3 | {0,1} | không | 
| 1 | 6 | {1,2} | (3,6) | 
| 2 | 28 | {2,3,4} | (6,28) | 
| 3 | 9 | {0,3} | (3,9), (28,9) | 

Khám phá BFS tìm thấy một chu trình liên quan đến cả bốn nút: 9 → 3 → 6 → 28 → 9. 

Điều này xác nhận rằng các chu trình ở đây không nhất thiết phải là hình tam giác và BFS nắm bắt chính xác cấu trúc bước đi khép kín tối thiểu. 

### Mẫu 2 (đã thi công) 

đầu vào:```
3
5 12 9
```Cấu trúc bit tạo thành một hình tam giác thông qua các bit được chia sẻ một cách gián tiếp. 

| Bước | Nút | Bit | Đã thêm các cạnh mới | 
| --- | --- | --- | --- | 
| 0 | 5 | {0,2} | không | 
| 1 | 12 | {2,3} | (5,12) | 
| 2 | 9 | {0,3} | (5,9), (12,9) | 

Điều này ngay lập tức tạo thành 3 chu kỳ và BFS từ bất kỳ nút nào sẽ phát hiện ra nó có độ dài 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(60n + 60m BFS) | đếm bit cộng với BFS thưa thớt trên biểu đồ mức độ giới hạn | 
| Không gian | O(n + m) | danh sách kề và mảng BFS | 

Cấu trúc đảm bảo rằng mỗi nút chỉ đóng góp tối đa 60 cạnh, giữ cho biểu đồ đủ thưa để các lần chạy BFS lặp lại vượt qua trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque, defaultdict
    INF = 10**9

    n = int(input())
    a = list(map(int, input().split()))
    
    bit_count = [0]*61
    for x in a:
        for b in range(61):
            if x >> b & 1:
                bit_count[b] += 1
    if any(c >= 3 for c in bit_count):
        return "3"
    
    nums = [x for x in a if x != 0]
    n = len(nums)
    
    adj = [[] for _ in range(n)]
    seen = [[] for _ in range(61)]
    
    for i, x in enumerate(nums):
        for b in range(61):
            if x >> b & 1:
                for j in seen[b]:
                    adj[i].append(j)
                    adj[j].append(i)
                seen[b].append(i)
    
    def bfs(s):
        dist = [INF]*n
        parent = [-1]*n
        dist[s] = 0
        q = deque([s])
        best = INF
        
        while q:
            u = q.popleft()
            if dist[u] >= best:
                continue
            for v in adj[u]:
                if dist[v] == INF:
                    dist[v] = dist[u] + 1
                    parent[v] = u
                    q.append(v)
                elif parent[u] != v:
                    best = min(best, dist[u] + dist[v] + 1)
        return best
    
    ans = INF
    for i in range(n):
        ans = min(ans, bfs(i))
    
    return str(ans if ans < INF else -1)

# provided samples
assert run("4\n3 6 28 9\n") == "4"
assert run("3\n5 12 9\n") == "3"

# custom cases
assert run("1\n0\n") == "-1", "single node"
assert run("4\n1 2 4 8\n") == "-1", "no edges"
assert run("3\n1 1 1\n") == "3", "all same bit creates triangle"
assert run("5\n3 6 28 9 1\n") in {"3","4"}, "cycle presence check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút 0 đơn | -1 | trường hợp không có chu kỳ tầm thường | 
| sức mạnh rời rạc của hai | -1 | không có cạnh nào cả | 
| bit giống hệt nhau lặp lại | 3 | phát hiện tam giác ngay lập tức | 
| chu kỳ thưa thớt hỗn hợp | 3 hoặc 4 | Độ mạnh phát hiện chu trình BFS | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một bit xuất hiện trong ba số. Ví dụ, đầu vào`[1, 1, 1]`tạo ra một tam giác hoàn chỉnh mặc dù không nhìn thấy được cấu trúc chu trình rõ ràng. Thuật toán nắm bắt điều này trước khi xây dựng biểu đồ, tránh BFS không cần thiết và đảm bảo tính chính xác. 

Một trường hợp tinh tế khác là khi các số khác 0 nhưng không chia sẻ các bit theo cặp, chẳng hạn như`[1, 2, 4, 8]`. Biểu đồ được xây dựng không có cạnh, BFS không bao giờ phát hiện ra cạnh sau và kết quả vẫn chính xác`-1`. 

Một trường hợp thú vị hơn là khi chu kỳ dài hơn 3, như trong`[3, 6, 28, 9]`. Ở đây không có bit nào có tần số ≥ 3, vì vậy chúng tôi dựa hoàn toàn vào BFS. Sự kề cận được xây dựng thông qua các bit được chia sẻ tạo ra một chu trình vô hướng hợp lệ và BFS phát hiện chính xác chu trình ngắn nhất, trả về 4.
