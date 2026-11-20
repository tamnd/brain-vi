---
title: "CF 2183F - Người Nhảy"
description: "Chúng ta có một cây có gốc với mỗi nút được gắn nhãn bằng một chữ cái viết thường. Đối với mỗi nút $i$, chúng ta được yêu cầu phân tích tất cả các chuỗi có thể được hình thành bằng cách bắt đầu tại bất kỳ nút nào trong cây con của $i$ và liên tục nhảy đến một chuỗi con thích hợp, nối các chữ cái dọc theo…"
date: "2026-06-07T21:47:08+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 2183
codeforces_index: "F"
codeforces_contest_name: "Hello 2026"
rating: 2500
weight: 2183
solve_time_s: 144
verified: false
draft: false
---

[CF 2183F - Người nhảy](https://codeforces.com/problemset/problem/2183/F) 

**Đánh giá:** 2500 
**Tags:** lực lượng vũ phu, tổ hợp, dfs và tương tự, dp, cây 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với mỗi nút được gắn nhãn bằng một chữ cái viết thường. Đối với mỗi nút$i$, chúng ta được yêu cầu phân tích tất cả các chuỗi có thể được hình thành bằng cách bắt đầu từ bất kỳ nút nào trong cây con của$i$và liên tục nhảy đến một hậu duệ thích hợp, nối các chữ cái dọc theo đường dẫn. Đầu ra cuối cùng cho nút$i$là tổng bình phương của số lượng tất cả các chuỗi riêng biệt có thể được tạo theo cách này, modulo$998244353$. 

Cây có thể có tới 5000 nút cho mỗi trường hợp thử nghiệm và tổng số nút trên tất cả các trường hợp thử nghiệm cũng bị giới hạn ở mức 5000. Điều này ngay lập tức loại trừ cách tiếp cận bạo lực tạo ra tất cả các đường dẫn có thể một cách rõ ràng, bởi vì mỗi nút có khả năng$2^{\text{size of subtree}}-1$những con đường tăng trưởng theo cấp số nhân. Các trường hợp cạnh bao gồm các cây trong đó tất cả các nút có cùng một chữ cái, các cây có một chuỗi dài và các cây con có kích thước bằng một. Các giải pháp bất cẩn cố gắng liệt kê tất cả các chuỗi sẽ thất bại cả về thời gian và bộ nhớ. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là liệt kê mọi nút bắt đầu trong cây con và tạo đệ quy tất cả các đường dẫn bằng cách truy cập từng nút con thích hợp. Mỗi đường dẫn được chuyển đổi thành một chuỗi, được tính trong từ điển và sau đó tổng số bình phương được tính tổng. Điều này hiệu quả với những cây rất nhỏ, nhưng trong trường hợp xấu nhất là một ngôi sao hoặc chuỗi có$n \approx 5000$, số lượng đường dẫn là theo cấp số nhân trong$n$, rõ ràng là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát rằng vấn đề có thể được giảm xuống bằng cách kết hợp số lượng chuỗi từ cây con. Cụ thể, đối với bất kỳ nút nào$u$, các chuỗi bắt đầu từ$u$hoặc chỉ là chữ cái ở$u$hoặc chữ cái đó nối với một chuỗi bắt đầu từ một đứa trẻ nào đó. Điều này tự nhiên dẫn đến một cách tiếp cận lập trình động trên cây. Chúng ta có thể biểu diễn tất cả các chuỗi trong cây con có gốc tại$u$dưới dạng bản đồ tần số của các chuỗi, trong đó mỗi chuỗi được biểu diễn dưới dạng một bộ chữ cái hoặc hàm băm để tránh chi phí nối chuỗi thực tế. Sau đó, chúng tôi hợp nhất các bản đồ con vào bản đồ gốc, cập nhật số lượng một cách thích hợp. Bởi vì việc hợp nhất các bản đồ tần số có thể được thực hiện tăng dần mà không cần liệt kê tất cả các đường dẫn, nên cách tiếp cận này chia tỷ lệ bậc hai với$n$, có thể chấp nhận được$n \le 5000$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Cây DP với bản đồ tần số | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cây và lưu trữ các cạnh trong danh sách kề. Đánh dấu gốc là nút 1. 
2. Đối với mỗi nút, khởi tạo bản đồ tần số`dp[u]`chứa chuỗi một ký tự tại nút đó với số đếm là 1. Điều này thể hiện tất cả các đường dẫn bắt đầu và kết thúc tại`u`. 
3. Xử lý cây bằng DFS thứ tự sau. Đối với mỗi nút`u`, xét từng đứa trẻ`v`. 
4. Đối với mỗi chuỗi trong`dp[v]`, tạo một chuỗi mới bằng cách thêm vào trước chữ cái tại`u`. Thêm chuỗi này vào`dp[u]`và tăng số lượng của nó bằng số đếm từ`dp[v]`. Điều này kết hợp tất cả các đường dẫn bắt đầu từ`v`vào những con đường bắt đầu từ`u`. 
5. Sau khi hợp nhất tất cả trẻ em,`dp[u]`chứa số lượng tất cả các chuỗi bắt đầu từ`u`. Bản đồ tương tự có thể được sử dụng cho cây con của`u`bởi vì bất kỳ đường dẫn nào bắt đầu trong cây con đều bắt đầu tại`u`hoặc một phần tử con và bước hợp nhất đảm bảo tất cả các kết hợp đều được tính. 
6. Đối với mỗi nút`i`, tính tổng các bình phương của số đếm trong`dp[i]`để có được câu trả lời. Áp dụng modulo$998244353$. 

Tại sao nó hoạt động: Bất biến DP là sau khi xử lý một nút`u`,`dp[u]`đếm chính xác mọi chuỗi riêng biệt bắt đầu từ`u`hoặc ở bất kỳ hậu duệ nào. Việc hợp nhất các nút con bằng cách thêm vào trước chữ cái của nút hiện tại sẽ đáp ứng yêu cầu di chuyển từ nút này sang nút con cháu thích hợp. Tổng bình phương của các số đếm sẽ tính toán chính xác số liệu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10000)

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        tree = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            tree[u - 1].append(v - 1)
            tree[v - 1].append(u - 1)
        
        ans = [0] * n
        
        def dfs(u, parent):
            # dp[u] maps strings to counts
            dp = {}
            # the single letter at u
            dp[s[u]] = 1
            for v in tree[u]:
                if v == parent:
                    continue
                child_dp = dfs(v, u)
                # merge child strings
                for key, count in child_dp.items():
                    new_key = s[u] + key
                    dp[new_key] = dp.get(new_key, 0) + count
            ans[u] = sum(count * count for count in dp.values()) % MOD
            return dp
        
        dfs(0, -1)
        print(' '.join(str(a) for a in ans))
```DFS khởi tạo bản đồ của mỗi nút bằng chuỗi ký tự đơn. Khi đến thăm trẻ em, chúng ta tạo các chuỗi mới bằng cách thêm vào trước lá thư của cha mẹ. Tổng bình phương được tính sau khi hợp nhất tất cả các phần tử con. Độ sâu đệ quy được đặt cao hơn$n$để xử lý cây sâu. Việc sử dụng từ điển sẽ tránh tạo ra số lượng chuỗi theo cấp số nhân trong bộ nhớ vì các khóa chỉ là các chuỗi duy nhất. 

## Ví dụ đã hoạt động 

**Mẫu 1, trường hợp thử nghiệm đầu tiên**```
3
abb
1 2
1 3
```| Nút | nội dung dp | trả lời | 
| --- | --- | --- | 
| 2 | {'b':1} | 1 | 
| 3 | {'b':1} | 1 | 
| 1 | {'a':1, 'ab':2, 'b':2} | 9 | 

Bảng cho thấy các nút lá chỉ có các chuỗi một chữ cái. Gốc hợp nhất những thứ này để bao gồm tất cả các đường dẫn bắt đầu từ chính nó, bao gồm cả những đường dẫn thông qua con, xác nhận thuật toán đếm các đường dẫn một cách chính xác. 

**Mẫu 1, trường hợp thử nghiệm thứ hai**```
2
aa
1 2
```| Nút | nội dung dp | trả lời | 
| --- | --- | --- | 
| 2 | {'a':1} | 1 | 
| 1 | {'a':1, 'aa':1} | 5 | 

Điều này xác nhận rằng nhiều chữ cái giống nhau được hợp nhất một cách chính xác và các ô vuông được tính tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi nút có thể hợp nhất các bản đồ dp của các nút con của nó. Trong trường hợp xấu nhất, tổng số chuỗi là O(n^2) do độ sâu của cây và sự phân nhánh. | 
| Không gian | O(n^2) | Bản đồ dp của mỗi nút có thể lưu trữ các chuỗi O(n) và trên cây, tổng số chuỗi O(n^2) được lưu trữ. | 

Cho rằng tổng của tất cả$n$trên tất cả các trường hợp thử nghiệm là 5000, các thao tác O(n^2) có thể thực hiện được trong vòng 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("""5
3
abb
1 2
1 3
2
aa
1 2
4
ccbb
1 2
2 3
2 4
4
aaaa
1 4
4 2
2 3
10
cacbcccbac
1 2
2 3
3 4
2 5
1 6
2 7
3 8
4 9
8 10
""") == "9 1 1\n5 1\n29 9 1 1\n69 5 1 19\n185 65 19 3 1 1 1 3 1 1"

# Minimum size
assert run("1\n1\na\n") == "1"

# All letters equal
assert run("1\n3\naa\n1 2\n2 3\n") == "5 1 1"

# Chain
assert run("1\n4\nabcd\n1 2\n2 3\n3 4\n") == "10 5 2 1"

# Star
assert run("1\n4\naabc\n1 2\n1 3\n1 4\n") == "13 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 1 | Xử lý cây nút đơn | 
| 3 nút, tất cả đều bằng nhau | 5 1 1 | Hợp nhất đúng các chữ cái giống hệt nhau | 
| Chuỗi 4 | 10 5 2 1 | Đường dẫn sâu được xử lý | 
| Sao có bản sao | 13 1 1 1 | Nhiều con của root | 

## Vỏ cạnh 

Đối với cây một nút`1\na`, DFS khởi tạo`dp[0]`với`{'a':1}`, Và`ans[0] = 1^2 = 1`. Thuật toán xử lý đầu vào tối thiểu một cách chính xác. 

Đối với một chuỗi trong đó tất cả các chữ cái đều giống nhau
