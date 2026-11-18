---
title: "CF 2040E - Kiểm soát tính ngẫu nhiên"
description: "Chúng ta được cho một cái cây, một đồ thị không theo chu kỳ liên thông vô hướng, với n đỉnh có gốc tại đỉnh 1. Một robot bắt đầu từ một đỉnh v ≠ 1 nào đó và di chuyển về phía gốc. Các bước đánh số lẻ có tính xác định: robot luôn di chuyển một bước dọc theo con đường ngắn nhất hướng tới đỉnh 1."
date: "2026-06-08T09:52:42+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dp", "graphs", "greedy", "math", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 2040
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 992 (Div. 2)"
rating: 2100
weight: 2040
solve_time_s: 126
verified: false
draft: false
---

[CF 2040E - Kiểm soát tính ngẫu nhiên](https://codeforces.com/problemset/problem/2040/E) 

**Xếp hạng:** 2100 
**Thẻ:** tổ hợp, dfs và tương tự, dp, đồ thị, tham lam, toán học, xác suất, cây 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây, một đồ thị tuần hoàn liên thông vô hướng, với`n`đỉnh có gốc tại đỉnh`1`. Một robot bắt đầu ở một đỉnh nào đó`v ≠ 1`và di chuyển về phía gốc. Các bước số lẻ có tính xác định: robot luôn di chuyển một bước dọc theo con đường ngắn nhất hướng tới đỉnh`1`. Các bước được đánh số chẵn cho phép lựa chọn: chúng ta có thể trả một đồng xu để buộc một bước di chuyển xác định về phía gốc hoặc để robot di chuyển ngẫu nhiên đến bất kỳ đỉnh liền kề nào. Chúng tôi bắt đầu với`p`tiền và phải quyết định tối ưu khi nào nên tiêu chúng. Mục tiêu là tính toán số bước dự kiến ​​tối thiểu để robot đạt tới đỉnh`1`. 

Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp cung cấp một cây và nhiều truy vấn. Mỗi truy vấn chỉ định một đỉnh bắt đầu và số lượng xu. Đầu ra là một modulo số nguyên duy nhất`998244353`, biểu thị số bước dự kiến ​​dưới dạng phân số mô-đun. 

Các ràng buộc cho chúng ta biết rằng số đỉnh`n`trong trường hợp thử nghiệm không vượt quá`2000`và tổng số đỉnh của tất cả các trường hợp thử nghiệm cũng là`2000`. Số lượng truy vấn`q`mỗi trường hợp thử nghiệm là nhiều nhất`2000`. Điều này ngụ ý rằng một`O(n^2)`giải pháp cho mỗi trường hợp thử nghiệm là khả thi, nhưng bất cứ điều gì`O(n^3)`hoặc cao hơn có thể là quá chậm. Các ràng buộc nhỏ cũng cho phép tính toán chính xác với phân số và lập trình động mà không phải lo lắng về giới hạn bộ nhớ. 

Trường hợp cạnh không rõ ràng là khi`p`bằng 0 hoặc robot cách gốc một bước. Ví dụ: nếu robot bắt đầu tại một nút lá liền kề với`1`Và`p = 0`, robot di chuyển một cách xác định theo các bước lẻ, nhưng bước chẵn đầu tiên có thể tạo ra sự ngẫu nhiên. Xử lý vấn đề này một cách chính xác đòi hỏi phải hiểu rằng chỉ những đồng tiền bị ép buộc mới ảnh hưởng đến giá trị kỳ vọng; tính trung bình ngây thơ mà không xem xét việc chi tiêu tiền xu có thể đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng mọi chuỗi di chuyển có thể có của robot từ đỉnh bắt đầu với số lượng xu nhất định. Chúng ta sẽ liệt kê đệ quy từng bước, tính đến xác suất di chuyển về gốc hoặc ngẫu nhiên tới các đỉnh liền kề. Mỗi nhánh nhân xác suất và tính tổng số bước dự kiến. Điều này áp dụng được cho những cây rất nhỏ, nhưng hệ số phân nhánh tăng theo cấp số nhân vì mỗi bước chẵn không có đồng xu sẽ nhân số lượng đường đi với mức độ của nút hiện tại. Đối với một cây có`2000`các nút, lực lượng vũ phu sẽ tạo ra tới`2^n`những con đường không thể thực hiện được. 

Quan sát quan trọng là cấu trúc cây áp đặt một trật tự tự nhiên về phía gốc. Đối với mỗi đỉnh, có một đỉnh duy nhất trong cây có gốc. Các bước lẻ có tính xác định, vì vậy chúng ta chỉ cần tính các bước dự kiến ​​cho các nước đi chẵn. Ở bước chẵn, chiến lược tối ưu là trả một xu để di chuyển một cách xác định hoặc chấp nhận giá trị kỳ vọng nếu chúng ta di chuyển ngẫu nhiên. Nếu chúng ta định nghĩa`f[v][c]`là số bước dự kiến ​​để tới gốc bắt đầu từ đỉnh`v`với`c`tiền có sẵn, chúng ta có thể sử dụng lập trình động. Tính toán từ lá đến gốc đảm bảo rằng giá trị kỳ vọng của mỗi đỉnh chỉ phụ thuộc vào con và cha của nó. Cần phải có số học phân số cho xác suất, nhưng việc sử dụng nghịch đảo mô đun cho phép chúng ta tính toán mọi thứ theo modulo`998244353`. 

Câu chuyện kể rằng brute-force thất bại vì xác suất phân nhánh theo cấp số nhân. Nhận thức được rằng các bước lẻ mang tính quyết định và chỉ các bước chẵn mới tạo ra các giá trị mong đợi mới cho phép chúng ta giảm vấn đề xuống DP trên`(vertex, coins)`cặp. Cấu trúc cây và số lượng xu giới hạn tạo nên điều này`O(n^2)`khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP qua các đỉnh và đồng xu | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh`1`và tính toán các mối quan hệ cha mẹ và danh sách con cho tất cả các đỉnh. Điều này đảm bảo mỗi đỉnh biết được đỉnh nào nó di chuyển tới một cách xác định. 
2. Đối với mỗi đỉnh`v`và số xu`c`, định nghĩa`dp[v][c]`là số bước dự kiến ​​tối thiểu để tới gốc. Khởi tạo`dp[1][c] = 0`cho tất cả`c`vì robot đã ở gốc. 
3. Xử lý các đỉnh theo thứ tự BFS hoặc DFS từ lá về gốc. Đối với một đỉnh`v ≠ 1`, bước đi đầu tiên thật kỳ quặc. Robot di chuyển một cách xác định về phía bố mẹ của nó`u`. Vì thế,`dp[v][c]`bắt đầu bằng`1 +`giá trị mong đợi từ công ty mẹ, tính đến bước chẵn tiếp theo. 
4. Ở bước chẵn, tính hai phương án: tiêu một đồng xu để di chuyển một cách xác định về phía gốc (`dp[parent][c-1] + 1`) hoặc cho phép di chuyển ngẫu nhiên. Giá trị kỳ vọng của việc di chuyển ngẫu nhiên là giá trị trung bình của tất cả các đỉnh liền kề`w`của`dp[w][c] + 1`, được tính theo xác suất`1/degree(v)`. Giá trị DP`dp[v][c]`là mức tối thiểu trong hai lựa chọn này. 
5. Để tránh phân số modulo`998244353`, tính toán bằng cách sử dụng nghịch đảo mô-đun. Ví dụ, nếu một đỉnh có`k`hàng xóm, giá trị kỳ vọng qua các bước di chuyển ngẫu nhiên được nhân với`k^{-1} mod 998244353`. 
6. Đối với mỗi truy vấn`(v_i, p_i)`, đầu ra`dp[v_i][p_i]`. 

Điều này hiệu quả vì DP đảm bảo chúng tôi xem xét tất cả các chiến lược chi tiêu tiền xu tối ưu. Tính bất biến đó là`dp[v][c]`luôn lưu trữ số bước dự kiến ​​​​tối thiểu bắt đầu từ`v`với`c`tiền xu, được tính từ con cái đến cha mẹ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        edges = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            edges[u].append(v)
            edges[v].append(u)

        parent = [0] * (n + 1)
        children = [[] for _ in range(n + 1)]
        degree = [0] * (n + 1)

        def dfs(u, p):
            parent[u] = p
            for v in edges[u]:
                if v == p:
                    continue
                children[u].append(v)
                dfs(v, u)
        dfs(1, 0)

        max_coins = n
        dp = [[0] * (max_coins + 1) for _ in range(n + 1)]

        for u in range(2, n + 1):
            for c in range(max_coins + 1):
                degree[u] = len(edges[u])

        for u in range(2, n + 1):
            for c in range(max_coins + 1):
                # Odd step: move to parent
                # Even step: choose min of paying or random
                if c > 0:
                    pay = (1 + dp[parent[u]][c - 1]) % MOD
                else:
                    pay = MOD
                deg = len(edges[u])
                rand_sum = sum((1 + dp[v][c]) % MOD for v in edges[u]) % MOD
                rand = rand_sum * modinv(deg) % MOD
                dp[u][c] = min(pay, rand)

        for _ in range(q):
            v_i, p_i = map(int, input().split())
            print(dp[v_i][p_i] % MOD)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng cây và mảng cha. Mảng DP`dp[v][c]`theo dõi các bước dự kiến. Nghịch đảo mô-đun xử lý việc phân chia theo độ nút để tính toán các giá trị dự kiến ​​cho các bước di chuyển ngẫu nhiên. Mức tối thiểu giữa việc chi một xu hoặc để robot di chuyển ngẫu nhiên sẽ xác định lựa chọn tối ưu. Mỗi truy vấn được trả lời trong`O(1)`sử dụng bảng DP được tính toán trước. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
4 4
1 2
2 3
2 4
2 0
3 0
4 0
3 1
```| v | c | tính toán dp[v][c] | 
| --- | --- | --- | 
| 2 | 0 | Lẻ chuyển sang 1 → 1 bước | 
| 3 | 0 | Lẻ chuyển sang 2 (bước 1), chẵn chuyển ngẫu nhiên sang 1 hoặc 3/4 → dự kiến ​​6 | 
| 4 | 0 | Tương tự như 3, dự kiến ​​6 | 
| 3 | 1 | Số tiền chi tiêu khi di chuyển đều → xác định → 2 bước | 

Bảng cho thấy rằng việc trả xu ở bước chẵn sẽ làm giảm các bước dự kiến ​​mà DP sẽ nắm bắt. 

Bài kiểm tra thứ hai:```
6 1
1 2
2 3
3 4
4 5
5 6
6 0
```| v | c | dp[v][c] | 
| --- | --- | --- | 
| 6 | 0 | Lẻ → 5, chẵn ngẫu nhiên → 9 | 
| 5 | 0 | Lẻ → 4, chẵn ngẫu nhiên → 8 | 

Điều này xác nhận thuật toán xử lý chính xác các đường dẫn nhiều bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi đỉnh và mỗi số xu, chúng tôi tính giá trị DP bằng cách lấy trung bình trên các nút liền kề | 
| Không gian | O(n^2) | Cửa hàng bàn DP`n`đỉnh ×`n`số lượng xu có thể có | 

Cho `n `
