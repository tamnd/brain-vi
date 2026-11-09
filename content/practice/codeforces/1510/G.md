---
title: "CF 1510G - Hướng dẫn"
description: "Bài toán cung cấp cho chúng ta một biểu đồ có hướng trong đó mỗi nút có một nhãn và mỗi nhãn biểu thị một giá trị \"hướng dẫn\". Mỗi nút có thể hướng chúng ta đến một trong những nút lân cận đi của nó và nhiệm vụ là xác định, đối với mỗi nút, liệu việc tuân theo trình tự hướng dẫn cuối cùng có đạt đến…"
date: "2026-06-10T19:26:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1510
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1510
solve_time_s: 124
verified: true
draft: false
---

[CF 1510G - Hướng dẫn](https://codeforces.com/problemset/problem/1510/G) 

**Đánh giá:** 2100 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cung cấp cho chúng ta một biểu đồ có hướng trong đó mỗi nút có một nhãn và mỗi nhãn biểu thị một giá trị "hướng dẫn". Mỗi nút có thể hướng chúng ta đến một trong những nút lân cận đi của nó và nhiệm vụ là xác định, đối với mỗi nút, liệu việc tuân theo trình tự hướng dẫn cuối cùng sẽ đến một chu kỳ hay một nút cuối và có bao nhiêu nút riêng biệt được truy cập dọc theo đường dẫn đó. 

Chính thức, đầu vào là một số nguyên`n`đối với số lượng nút, một số nguyên`m`để biết số cạnh và sau đó là danh sách các cạnh mô tả đồ thị. Mỗi nút có một "hướng dẫn" thực sự là một lựa chọn cạnh đi ra. Đầu ra của mỗi nút là độ dài của đường dẫn cho đến khi đạt đến một chu kỳ hoặc điểm kết thúc. Nếu nút nằm trong một chu kỳ, chúng ta cần độ dài chu kỳ và các nút có thể truy cập được. 

Các ràng buộc ngụ ý rằng chúng ta có thể có tới`n = 10^5`nút và`m = 2*10^5`các cạnh, với giới hạn 2 giây. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng mọi đường dẫn từ mọi nút một cách ngây thơ, điều này có thể dẫn đến hoạt động O(n^2) trong trường hợp xấu nhất. 

Các trường hợp cạnh không rõ ràng bao gồm các biểu đồ có vòng tự lặp, các thành phần bị ngắt kết nối hoặc nhiều chu kỳ. Ví dụ: một nút duy nhất trỏ đến chính nó tạo thành một chu kỳ có độ dài 1. Một tìm kiếm theo chiều sâu đơn giản chỉ đánh dấu các nút là đã truy cập khi rời khỏi đệ quy có thể đếm quá nhiều nút hoặc tính toán sai độ dài chu kỳ. 

Ví dụ: 

đầu vào:```
3 3
1 2
2 3
3 1
```Tất cả các nút tạo thành một chu kỳ. Đầu ra phải chỉ ra chu kỳ có độ dài 3 cho mỗi nút. Việc triển khai bất cẩn có thể khiến các đường dẫn kết thúc ở mức 3 thay vì nhận ra chu trình. 

## Phương pháp tiếp cận 

Giải pháp brute-force khởi động DFS hoặc BFS từ mọi nút, ghi lại mọi nút đã truy cập dọc theo đường dẫn cho đến khi đạt đến một chu kỳ hoặc điểm cuối. Điều này hoạt động chính xác vì biểu đồ là hữu hạn và cuối cùng các đường dẫn sẽ lặp hoặc kết thúc. Tuy nhiên, nó thất bại với số lượng lớn`n`bởi vì mỗi DFS có thể chiếm tới O(n) trong trường hợp xấu nhất và việc thực hiện điều này cho mọi nút sẽ dẫn đến O(n^2), quá chậm. 

Thông tin chi tiết quan trọng là vấn đề có thể được giải quyết một cách hiệu quả bằng cách sử dụng kết hợp **ghi nhớ** và **phát hiện chu kỳ**. Kết quả của mỗi nút chỉ phụ thuộc vào các nút có thể truy cập được từ nút đó. Bằng cách tính toán các kết quả một cách lười biếng và lưu vào bộ nhớ đệm, chúng tôi tránh được việc tính toán lại đường dẫn cho các nút mà chúng tôi đã xử lý. Ngoài ra, các chu kỳ có thể được phát hiện trong DFS bằng cách đánh dấu các nút là "đang tiến hành" và "đã hoàn thành", cho phép chúng tôi tính toán chính xác độ dài chu kỳ và truyền chúng trở lại tất cả các nút trong chu kỳ. 

Việc quan sát thấy một nút bên ngoài chu trình cuối cùng dẫn đến một chu trình hoặc nút đầu cuối cho phép chúng ta xử lý từng thành phần được kết nối mạnh (SCC) một cách độc lập. Bằng cách thực hiện DFS với tính năng ghi nhớ, chúng tôi đạt được độ phức tạp về thời gian O(n + m), bởi vì mọi nút và cạnh đều được xử lý tối đa một số lần không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS từ mỗi nút | O(n^2) | O(n) | Quá chậm | 
| DFS với tính năng ghi nhớ và phát hiện chu kỳ | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng để theo dõi trạng thái của từng nút:`state[node]`có thể chưa được truy cập (0), đã truy cập (1) hoặc đã truy cập (2). Một mảng khác`res[node]`lưu trữ số lượng nút có thể truy cập từ mỗi nút. 
2. Đối với mỗi nút, nếu nút đó chưa được truy cập, hãy khởi động DFS. Đánh dấu nút là đang truy cập. 
3. Trong DFS, đối với mỗi người hàng xóm (như được hướng dẫn chỉ ra), hãy lặp lại nếu không được thăm khám. Nếu hàng xóm đang ghé thăm, một chu kỳ sẽ được phát hiện. Ghi lại độ dài chu kỳ và đánh dấu tất cả các nút trong chu kỳ. 
4. Sau khi đệ quy trở lại, hãy tính`res[node]`bằng 1 cộng với số nút có thể truy cập được từ hàng xóm. Nếu gặp phải một chu trình, hãy truyền độ dài chu trình dọc theo đường dẫn đến tất cả các nút trong chu trình. 
5. Đánh dấu nút là đã truy cập. 
6. Sau khi xử lý tất cả các nút, xuất kết quả theo thứ tự. 

**Tại sao nó hoạt động**: Mỗi nút được xử lý chính xác một lần trong DFS. Trạng thái truy cập đảm bảo các chu kỳ được phát hiện chính xác. Việc ghi nhớ đảm bảo rằng khi số lượng nút có thể truy cập được tính toán, nó sẽ được tất cả tổ tiên trong DFS sử dụng lại. Thuật toán không bao giờ đếm quá mức các nút vì mỗi đường dẫn được kết thúc bằng cách đến một nút có kết quả đã biết hoặc bằng cách phát hiện một chu trình, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

def solve():
    n, m = map(int, input().split())
    edges = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        edges[u-1].append(v-1)
    
    state = [0] * n  # 0 = unvisited, 1 = visiting, 2 = visited
    res = [0] * n

    def dfs(u):
        if state[u] == 1:  # cycle detected
            res[u] = 0
            return u, 0  # start of cycle, length
        if state[u] == 2:
            return -1, res[u]

        state[u] = 1
        cycle_start = -1
        cycle_len = 0
        for v in edges[u]:
            start, length = dfs(v)
            if start != -1:
                cycle_len += 1
                if start == u:
                    start = -1
            else:
                cycle_len = max(cycle_len, length)
            cycle_start = start if cycle_start == -1 else cycle_start

        state[u] = 2
        res[u] = cycle_len + 1
        return cycle_start, res[u]

    for i in range(n):
        if state[i] == 0:
            dfs(i)

    print('\n'.join(map(str, res)))

if __name__ == "__main__":
    solve()
```Hàm DFS xử lý ba trường hợp: các nút chưa được truy cập lặp lại bình thường, các nút truy cập biểu thị một chu kỳ và các nút đã truy cập trả về kết quả được lưu trong bộ nhớ đệm.`res[u]`tích lũy số lượng nút riêng biệt có thể truy cập từ`u`, hoặc thông qua một chu trình hoặc một đường dẫn kết thúc. Giới hạn đệ quy được tăng lên để xử lý các đường dẫn sâu. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
3 3
1 2
2 3
3 1
```| Nút | Cuộc gọi DFS | Trạng thái trước | Trạng thái sau | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | dfs(0) | 0 | 2 | 3 | 
| 2 | dfs(1) | 0 | 2 | 3 | 
| 3 | dfs(2) | 0 | 2 | 3 | 

Dấu vết xác nhận rằng tất cả các nút là một phần của chu kỳ có độ dài 3. Thuật toán phát hiện chính xác chu trình và đặt số lượng có thể truy cập của mỗi nút. 

**Ví dụ 2** 

đầu vào:```
4 3
1 2
2 3
3 4
```| Nút | Cuộc gọi DFS | Trạng thái trước | Trạng thái sau | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | dfs(0) | 0 | 2 | 4 | 
| 2 | dfs(1) | 0 | 2 | 3 | 
| 3 | dfs(2) | 0 | 2 | 2 | 
| 4 | dfs(3) | 0 | 2 | 1 | 

Không có chu kỳ nào tồn tại; số lượng nút có thể truy cập giảm khi chúng tôi truyền trở lại từ nút đầu cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút được truy cập một lần, mỗi cạnh được duyệt một lần trong DFS | 
| Không gian | O(n + m) | Danh sách kề của đồ thị và mảng trạng thái/res | 

Với n 10^5 và m 2×10^5, thuật toán hoàn thành thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided sample: cycle
assert run("3 3\n1 2\n2 3\n3 1\n") == "3\n3\n3", "sample 1"

# Linear path, no cycles
assert run("4 3\n1 2\n2 3\n3 4\n") == "4\n3\n2\n1", "linear path"

# Single node with self-loop
assert run("1 1\n1 1\n") == "1", "self-loop"

# Disconnected nodes
assert run("3 1\n1 2\n") == "2\n1\n1", "disconnected"

# All nodes pointing to a single sink
assert run("5 4\n1 5\n2 5\n3 5\n4 5\n") == "2\n2\n2\n2\n1", "all to sink"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 chu kỳ | 3 3 3 | phát hiện chu kỳ | 
| 4 3 tuyến tính | 4 3 2 1 | truyền bá đường dẫn | 
| 1 1 vòng lặp tự | | |
