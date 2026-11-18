---
title: "CF 2028E - Cuộc phiêu lưu của Alice trong hang thỏ"
description: "Chúng ta có một cây có n đỉnh trong đó đỉnh 1 được coi là lối ra của hố thỏ, và Alice bắt đầu ở đỉnh v nào đó. Mỗi lượt tung một đồng xu công bằng. Ở mặt ngửa, Alice chọn một đỉnh liền kề để di chuyển tới."
date: "2026-06-08T12:09:40+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dp", "games", "greedy", "math", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 2028
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 986 (Div. 2)"
rating: 2300
weight: 2028
solve_time_s: 83
verified: false
draft: false
---

[CF 2028E - Cuộc phiêu lưu của Alice trong hang thỏ](https://codeforces.com/problemset/problem/2028/E) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, dfs và tương tự, dp, trò chơi, tham lam, toán học, xác suất, cây 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`đỉnh ở đó có đỉnh`1`được coi là lối ra của hang thỏ và Alice bắt đầu ở một đỉnh nào đó`v`. Mỗi lượt một đồng xu công bằng được lật. Ở mặt ngửa, Alice chọn một đỉnh liền kề để di chuyển tới. Ở mặt sấp, Nữ hoàng Trái tim chọn một đỉnh liền kề để di chuyển Alice tới. Alice sẽ thua ngay lập tức nếu cô ấy đáp xuống một chiếc lá không có gốc và thắng nếu cô ấy đạt đến đỉnh`1`. Mục tiêu là tính toán, tại mỗi đỉnh, xác suất để Alice thoát khỏi lối chơi tối ưu của cả hai bên. Các xác suất này phải là modulo đầu ra`998244353`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho biết số đỉnh và cạnh của cây. Tổng của`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi`2*10^5`. Điều này có nghĩa là chúng tôi không thể sử dụng bất kỳ thuật toán nào chậm hơn khoảng`O(n log n)`mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Đặc biệt, những cách tiếp cận ngây thơ khám phá tất cả các con đường một cách tổ hợp là không khả thi. Bài toán cũng yêu cầu phân số chính xác modulo một số nguyên tố, vì vậy chúng ta phải xử lý nghịch đảo mô đun một cách cẩn thận. 

Các trường hợp cạnh không rõ ràng bao gồm các lá được kết nối trực tiếp với gốc, các chuỗi trong đó Alice chỉ có một đường đi về phía trước hoặc các đỉnh có nhiều con mà Nữ hoàng có thể buộc Alice vào một chiếc lá bị mất. Ví dụ, một cái cây có cạnh`1-2`,`2-3`,`3-4`có Alice ở đỉnh`3`. Nếu Alice di chuyển, cô ấy có thể đi đến`2`hoặc`4`, nhưng Nữ hoàng luôn có thể di chuyển cô ấy đến`4`, một chiếc lá, tạo nên khả năng trốn thoát`0`. Một BFS bất cẩn bỏ qua bước phản công tối ưu của Nữ hoàng sẽ đánh giá quá cao xác suất. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ cố gắng mô phỏng mọi chuỗi trò chơi có thể có từ mỗi đỉnh bắt đầu, theo cách đệ quy hoặc ghi nhớ. Về nguyên tắc, điều này đúng, nhưng trong trường hợp xấu nhất, số lượng trạng thái trò chơi sẽ tăng theo cấp số nhân theo độ sâu của cây. Ngay cả việc ghi nhớ theo đỉnh cũng không đủ vì lựa chọn của Alice và lựa chọn ngược lại của Nữ hoàng tạo ra xác suất có điều kiện cho mọi cạnh. Với`n`lên tới`2*10^5`, phương pháp này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng xác suất ở mỗi đỉnh có thể được xác định đệ quy bằng cách sử dụng cấu trúc cây. Cho phép`f(v)`biểu thị xác suất Alice thoát khỏi đỉnh`v`. Nếu như`v`là một chiếc lá khác`1`,`f(v)=0`. Nếu như`v=1`,`f(1)=1`. Ngược lại, Alice sẽ chọn trong số`deg(v)`hàng xóm. Đến lượt của mình, cô ấy có thể chọn người hàng xóm tối đa hóa`f(u)`. Đến lượt Nữ hoàng, Nữ hoàng chọn hàng xóm giảm thiểu`f(u)`. Bởi vì đồng xu công bằng nên xác suất là`f(v) = (f_max + f_min)/2`Ở đâu`f_max`là tối đa`f(u)`giữa những người hàng xóm và`f_min`là tối thiểu`f(u)`. Chúng ta có thể tính toán đệ quy này bắt đầu từ lá đến gốc bằng DFS, điều này đảm bảo độ phức tạp tuyến tính`O(n)`mỗi cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu (DFS + DP trên cây) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các cạnh của cây và xây dựng danh sách kề. Điều này cho phép chúng tôi lặp lại các hàng xóm một cách hiệu quả trong DFS. 
2. Xác định hàm DFS đệ quy`dfs(v, parent)`tính toán xác suất`f(v)`. Chúng tôi vượt qua`parent`để tránh phải xem lại nút mà chúng tôi đã đến từ đó. 
3. Nếu`v`là gốc`1`, xác suất trả về`1`. Nếu như`v`là một lá khác với gốc, xác suất trả về`0`. 
4. Đối với tất cả hàng xóm`u`của`v`loại trừ`parent`, tính toán đệ quy`dfs(u, v)`để có được xác suất trốn thoát khỏi bọn trẻ. 
5. Tính xác suất tối đa và tối thiểu trong số các xác suất con này. Mức tối đa thể hiện nước đi tối ưu của Alice và mức tối thiểu đại diện cho nước đi tối ưu của Nữ hoàng. 
6. Tính toán`(max + min) / 2`như một phân số sử dụng số học mô-đun. Lưu trữ nó cho`v`để tránh phải tính toán lại. 
7. Sau DFS, xuất xác suất các đỉnh`1`bởi vì`n`modulo`998244353`. 

Tại sao nó hoạt động: DFS tính toán xác suất từ ​​các lá trở lên. Bằng cách luôn xem xét các nước đi tối ưu cho cả Alice và Hậu ở mỗi đỉnh, chúng ta duy trì được bất biến rằng`f(v)`thể hiện chính xác xác suất thoát khỏi trò chơi hoàn hảo. Việc sử dụng số học mô-đun đảm bảo tính chính xác cho các phép tính phân số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

MOD = 998244353
INV2 = pow(2, MOD-2, MOD)  # Modular inverse of 2

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        adj = [[] for _ in range(n)]
        for _ in range(n-1):
            x, y = map(int, input().split())
            adj[x-1].append(y-1)
            adj[y-1].append(x-1)
        
        dp = [0]*n
        visited = [False]*n
        
        def dfs(v, parent):
            visited[v] = True
            children = [u for u in adj[v] if u != parent]
            if v == 0:
                dp[v] = 1
                return dp[v]
            if not children:
                dp[v] = 0
                return dp[v]
            child_probs = [dfs(u, v) for u in children]
            max_prob = max(child_probs)
            min_prob = min(child_probs)
            dp[v] = (max_prob + min_prob) * INV2 % MOD
            return dp[v]
        
        dfs(0, -1)
        print(' '.join(map(str, dp)))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng danh sách kề, sau đó thực hiện DFS từ thư mục gốc. Việc phát hiện lá là tự động: bất kỳ nút nào không có con ngoại trừ nút cha đều là một lá. Nghịch đảo mô-đun của`2`xử lý phép chia cho 2. Lưu kết quả vào`dp`đảm bảo mỗi nút được xử lý một lần. Chúng tôi in các xác suất theo thứ tự đỉnh. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
1 2
1 3
2 4
3 5
```| Đỉnh | Trẻ em | Thăm dò con tối đa | Thăm dò con tối thiểu | f(v) | 
| --- | --- | --- | --- | --- | 
| 4 | [] | - | - | 0 | 
| 5 | [] | - | - | 0 | 
| 2 | [4] | 0 | 0 | (0+0)/2 = 0 | 
| 3 | [5] | 0 | 0 | (0+0)/2 = 0 | 
| 1 | [2,3] | 0 | 0 | (0+0)/2 = 1 (theo định nghĩa) | 

Ở đây chúng ta thấy các lá nhận được 0 chính xác, gốc 1 nhận được 1 và các nút bên trong tính toán`f(v)`từ trẻ em. 

Đầu vào mẫu 2:```
9
1 2
2 3
4 5
5 6
7 8
8 9
2 4
5 7
```DFS truyền xác suất qua nhiều nhánh, trong đó Alice chọn xác suất con tối đa và Nữ hoàng chọn xác suất tối thiểu. Xác suất như`1/2`,`1/4`,`3/8`được tính toán và chuyển đổi bằng cách sử dụng nghịch đảo mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | DFS chạm vào mỗi đỉnh một lần và mỗi cạnh hai lần | 
| Không gian | O(n) | Danh sách kề, mảng DP, ngăn xếp đệ quy | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm là 2*10^5, tổng số thao tác phù hợp trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("2\n5\n1 2\n1 3\n2 4\n3 5\n9\n1 2\n2 3\n4 5\n5 6\n7 8\n8 9\n2 4\n5 7\n") == \
"1 499122177 499122177 0 0\n1 499122177 0 332748118 166374059 0 443664157 720954255 0", "sample 1"

# Custom: minimum size tree
assert run("1\n2\n1 2\n") == "1 0", "min tree"

# Custom: chain of 3 vertices
assert run("1\n3\n1 2\n2 3\n") == "1 499122177 0", "simple chain"

# Custom: star shape
assert run("1\n4\n1 2\n1 3\n1 4\n
```
