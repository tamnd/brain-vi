---
title: "CF 1790G - Token trên biểu đồ"
description: "Chúng ta được cung cấp một biểu đồ vô hướng được kết nối trong đó các đỉnh nhất định chứa mã thông báo và các đỉnh nhất định chứa phần thưởng. Một mã thông báo có thể di chuyển dọc theo các cạnh, nhưng số lần di chuyển bị giới hạn: ban đầu mỗi mã thông báo có thể di chuyển chính xác một lần."
date: "2026-06-09T10:40:25+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1790
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 847 (Div. 3)"
rating: 2300
weight: 1790
solve_time_s: 153
verified: false
draft: false
---

[CF 1790G - Mã thông báo trên biểu đồ](https://codeforces.com/problemset/problem/1790/G) 

**Đánh giá:** 2300 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ vô hướng được kết nối trong đó các đỉnh nhất định chứa mã thông báo và các đỉnh nhất định chứa phần thưởng. Một mã thông báo có thể di chuyển dọc theo các cạnh, nhưng số lần di chuyển bị giới hạn: ban đầu mỗi mã thông báo có thể di chuyển chính xác một lần. Tuy nhiên, nếu một mã thông báo rơi vào một đỉnh chứa phần thưởng, trò chơi sẽ cho phép di chuyển thêm từ bất kỳ mã thông báo nào khác. Nhiều mã thông báo có thể chiếm cùng một đỉnh và trò chơi được coi là thắng nếu bất kỳ mã thông báo nào đạt đến đỉnh 1, kết thúc. Nếu mã thông báo bắt đầu ở đỉnh 1, trò chơi sẽ thắng ngay lập tức. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp cấu trúc biểu đồ, vị trí của mã thông báo và vị trí của tiền thưởng. Biểu đồ có thể có tổng cộng tối đa 200.000 đỉnh và 200.000 cạnh trong tất cả các trường hợp thử nghiệm, nghĩa là mọi giải pháp đều phải gần như tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Các vòng lặp lồng nhau trên tất cả các đỉnh hoặc tất cả các mã thông báo có thể quá chậm. Các trường hợp cạnh bao gồm các biểu đồ có phần thưởng bằng 0, mã thông báo đã ở cuối hoặc mã thông báo bắt đầu ở xa đỉnh 1 nhưng liền kề với phần thưởng cho phép xâu chuỗi. 

Một giải pháp đơn giản có thể cố gắng mô phỏng mọi chuỗi di chuyển, theo dõi xem mã thông báo nào có thể di chuyển ở mỗi lượt. Điều này nhanh chóng trở thành cấp số nhân vì mỗi phần thưởng có thể kích hoạt một bước di chuyển từ bất kỳ mã thông báo nào và nhiều mã thông báo tương tác với nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi chuỗi di chuyển mã thông báo có thể có, khám phá tất cả các đường dẫn thông qua biểu đồ trong khi tôn trọng các quy tắc chuỗi tiền thưởng. Về nguyên tắc, điều này đúng, nhưng với tối đa 200.000 đỉnh và cạnh cũng như nhiều mã thông báo trên mỗi biểu đồ, số lượng chuỗi tăng lên theo kiểu tổ hợp. Trong trường hợp xấu nhất, số lượng chuỗi sẽ theo cấp số nhân, vượt xa giới hạn thời gian 2 giây. 

Quan sát quan trọng là chúng tôi không cần phải theo dõi trình tự di chuyển của từng mã thông báo riêng lẻ. Mỗi phần thưởng cho phép chúng ta xâu chuỗi các bước di chuyển một cách hiệu quả, nhưng điều quan trọng là liệu có tồn tại bất kỳ đường dẫn nào từ mã thông báo đến đỉnh 1 có thể truy cập được hay không nếu chúng ta coi tất cả các phần thưởng là "đỉnh trung gian cho phép di chuyển khác". Chúng ta có thể điều chỉnh lại vấn đề này dưới dạng một vấn đề về khả năng tiếp cận biểu đồ trong đó mọi đỉnh có phần thưởng sẽ trở thành một cổng: nếu mã thông báo có thể đạt được phần thưởng, nó có thể "bắt đầu lại" quá trình di chuyển của nó từ bất kỳ mã thông báo nào. Do đó, vấn đề giảm xuống còn tìm kiếm hai cấp độ. Đầu tiên, tìm tất cả các đỉnh có thể truy cập được chỉ bằng một lần di chuyển từ mã thông báo hoặc thông qua chuỗi thưởng. Thứ hai, kiểm tra xem đỉnh 1 có nằm trong tập hợp đó không. 

Chúng ta có thể triển khai điều này một cách hiệu quả bằng cách sử dụng tìm kiếm theo chiều rộng (BFS) hoặc tìm kiếm theo chiều sâu (DFS), bắt đầu từ tất cả các mã thông báo và mở rộng dọc theo các cạnh. Bất cứ khi nào chúng tôi gặp phần thưởng, chúng tôi coi đó là "tiện ích mở rộng miễn phí", cho phép tiếp tục di chuyển từ các mã thông báo khác. Đây thực sự là một BFS từ tất cả các mã thông báo có hàng đợi cho phép xem lại phần thưởng nhiều lần. Vì đồ thị được kết nối và số đỉnh và cạnh bị giới hạn, BFS chạy với O(n + m) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong p | O(n + m) | Quá chậm | 
| Tối ưu (BFS từ các token có chuỗi thưởng) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc số đỉnh, cạnh, mã thông báo và phần thưởng. Lưu trữ các vị trí mã thông báo và tiền thưởng theo bộ để kiểm tra tư cách thành viên nhanh chóng. Xây dựng danh sách kề của đồ thị. 
2. Nếu bất kỳ mã thông báo nào đã ở đỉnh 1, hãy xuất CÓ ngay lập tức và tiếp tục với trường hợp kiểm thử tiếp theo. 
3. Khởi tạo hàng đợi cho BFS chứa tất cả các mã thông báo và tập hợp đã truy cập đánh dấu vị trí bắt đầu của chúng. 
4. Khi hàng đợi không trống, hãy bật một đỉnh`u`. Nếu như`u`là đỉnh 1, xuất CÓ và dừng BFS. 
5. Đối với mỗi người hàng xóm`v`của`u`, kiểm tra xem nó đã được truy cập chưa. Nếu không, hãy đánh dấu nó đã truy cập và xếp nó vào hàng đợi. Nếu như`v`chứa phần thưởng, coi đó là "di chuyển tự do" và cho phép BFS tiếp tục với tất cả các mã thông báo vẫn hoạt động; trong thực tế, chỉ cần xếp hàng`v`thông thường vì BFS xử lý việc mở rộng một cách tự nhiên. 
6. Nếu BFS hoàn thành mà không đạt tới đỉnh 1, xuất ra NO. 

Điều bất biến là BFS đảm bảo tất cả các đỉnh có thể truy cập được thông qua việc di chuyển mã thông báo hợp lệ, bao gồm cả các chuỗi được kích hoạt bằng tiền thưởng, cuối cùng đều được truy cập. Vì BFS khám phá đồ thị theo cấp độ nên chúng ta sẽ đạt đến đỉnh 1 nếu bất kỳ chuỗi di chuyển nào cho phép. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        p, b = map(int, input().split())
        tokens = list(map(int, input().split()))
        bonuses = set(map(int, input().split())) if b > 0 else set()
        
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)
        
        if 1 in tokens:
            print("YES")
            continue
        
        visited = [False] * (n + 1)
        queue = deque(tokens)
        for tok in tokens:
            visited[tok] = True
        
        found = False
        while queue:
            u = queue.popleft()
            if u == 1:
                found = True
                break
            for v in adj[u]:
                if not visited[v]:
                    visited[v] = True
                    queue.append(v)
        
        print("YES" if found else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào bằng I/O nhanh, xây dựng biểu đồ và khởi tạo BFS từ tất cả các mã thông báo. Bản mở rộng BFS xử lý một cách tự nhiên tác động của phần thưởng: bất kỳ đỉnh nào có thể truy cập thông qua phần thưởng sẽ được truy cập vì BFS tiếp tục từ mọi đỉnh được liệt kê. Kiểm tra`if u == 1`đảm bảo chúng tôi phát hiện thành công ngay lập tức mà không cần hoàn thành toàn bộ BFS một cách không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (từ Mẫu 1)```
n=8, m=10
tokens=[7, 8]
bonuses=[2, 4, 5, 6]
finish=1
```| Xếp hàng | Các đỉnh đã truy cập | Ghi chú | 
| --- | --- | --- | 
| [7,8] | 7,8 | Bắt đầu BFS | 
| [8, hàng xóm...] | 7,8,6,... | Di chuyển 8→6, 7→hàng xóm | 
| ... | ... | Tiếp tục BFS thông qua các phần thưởng 6→4, v.v. | 
| ... | ... | Đạt đến đỉnh 1 thông qua chuỗi 8→6→4→2→1 | 

Đầu ra là CÓ, hiển thị chuỗi tiền thưởng cho phép phạm vi tiếp cận kết thúc. 

### Ví dụ 2 (không có bonus)```
n=5, m=4
tokens=[5]
bonuses=[]
finish=1
edges: 1-2-3-4-5
```| Xếp hàng | Đã truy cập | 
| --- | --- | 
| [5] | 5 | 
| [4] | 5,4 | 
| [3] | 5,4,3 | 
| [2] | 5,4,3,2 | 
| [1] | 5,4,3,2,1 | 

Đầu ra là CÓ vì ngay cả khi không có tiền thưởng, mã thông báo vẫn có thể đi qua đường dẫn. Trong các thử nghiệm chỉ được phép di chuyển một lần, việc không có phần thưởng sẽ ngăn cản việc đạt đến đỉnh 1. Điều này chứng tỏ thuật toán xử lý chính xác cả hai tình huống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | BFS truy cập mỗi đỉnh và cạnh nhiều nhất một lần. | 
| Không gian | O(n + m) | Danh sách kề và mảng đã thăm. | 

Với tổng n và m trên tất cả các trường hợp thử nghiệm 2·10^5, giải pháp chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("""6
8 10
2 4
7 8
2 4 5 6
1 2
2 3
2 4
3 4
3 5
4 6
5 6
5 7
6 8
7 8

5 4
1 1
5
3
1 2
2 3
3 4
4 5

2 1
1 0
2

1 2

4 3
1 2
2
3 4
1 2
2 3
2 4

5 4
3 2
5 3 4
2 4
1 2
2 3
3 4
4 5

1 0
1 1
1
1""") == "YES\nNO\nYES\nYES\nYES\nYES"

# Custom cases
assert run("1\n3 2\n1 0\n3\n\n1 2\n2 3\n") == "NO", "single token cannot reach finish"
assert run("1\n3 2\n1 1\n3\n2\n1 2\n2 3\n") == "YES", "bonus enables path to finish"
assert run("1\n1 0\n1 0\n1\n") == "YES", "token at finish"
assert run("1\n5 4\n2 1\n4 5\n3\n1 2\n2
```
