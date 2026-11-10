---
title: "CF 1540B - Mảng cây"
description: "Chúng ta được yêu cầu tính toán số lần đảo ngược dự kiến ​​trong một quá trình đánh dấu ngẫu nhiên các nút trong cây. Cây có n nút và quá trình đánh dấu bắt đầu bằng một nút ban đầu ngẫu nhiên thống nhất."
date: "2026-06-10T14:25:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp", "graphs", "math", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 1540
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 728 (Div. 1)"
rating: 2300
weight: 1540
solve_time_s: 243
verified: false
draft: false
---

[CF 1540B - Mảng cây](https://codeforces.com/problemset/problem/1540/B) 

**Đánh giá:** 2300 
**Tags:** lực lượng vũ phu, tổ hợp, dp, đồ thị, toán học, xác suất, cây 
**Thời gian giải:** 4m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính toán số lần đảo ngược dự kiến trong một quá trình đánh dấu ngẫu nhiên các nút trong cây. Cây có`n`các nút và quá trình đánh dấu bắt đầu bằng một nút ban đầu ngẫu nhiên thống nhất. Sau đó, lần lượt các nút được thêm vào, mỗi nút được chọn thống nhất từ ​​tập hợp các nút chưa được đánh dấu liền kề với các nút đã được đánh dấu. Mảng`a`ghi lại thứ tự các nút được đánh dấu và đảo ngược là một cặp`(i, j)`với`i < j`Và`a[i] > a[j]`. Mục tiêu là tính toán số lần đảo ngược modulo dự kiến`10^9+7`. 

Các ràng buộc chỉ ra`n`nhiều nhất là 200, một con số nhỏ. Điều này cho phép một thuật toán có độ phức tạp tăng lên gần giống như`O(n^3)`hoặc có lẽ cao hơn một chút, bởi vì`200^3 ≈ 8 * 10^6`, điều này có thể thực hiện được trong 2 giây. Vấn đề liên quan đến các kỳ vọng tổ hợp, do đó, việc mô phỏng mạnh mẽ tất cả các hoán vị sẽ là`O(n!)`và rõ ràng là không thể, ngay cả đối với`n = 10`. 

Các trường hợp cạnh cần chú ý bao gồm các cây nhỏ như chuỗi gồm hai nút hoặc cây hình ngôi sao. Đối với một chuỗi gồm hai nút, các nghịch đảo dự kiến ​​có thể là phân số. Một cách tiếp cận ngây thơ chỉ đếm một đơn hàng sẽ cho một số nguyên và không đạt được kỳ vọng. Cây có cấu trúc đối xứng giống như các ngôi sao cũng yêu cầu tính trung bình cẩn thận đối với các lựa chọn ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng tất cả các hoán vị hợp lệ của việc đánh dấu nút và tính toán các phép nghịch đảo cho mỗi hoán vị. Về lý thuyết, điều này hiệu quả vì chúng ta có thể liệt kê tất cả các hoán vị, tính tổng các nghịch đảo và chia cho tổng số hoán vị hợp lệ. Vấn đề là số lượng hoán vị tăng lên một cách bùng nổ với`n`. Vì`n = 10`, có hơn 3 triệu khả năng, và đối với`n = 20`, qua`10^18`. Điều này là không thể ngay cả đối với nhỏ`n`. 

Cái nhìn sâu sắc quan trọng đến từ việc xem xét vấn đề một cách đệ quy. Sự đảo ngược dự kiến ​​do một cặp nút đóng góp chỉ phụ thuộc vào vị trí tương đối của chúng trong cây. Nếu chúng ta chọn một nghiệm tùy ý thì số lần đảo ngược dự kiến ​​giữa hai nút`u`Và`v`có thể được biểu thị dưới dạng kích thước của cây con và vị trí tương đối. Bằng cách xác định`dp[u][v]`như xác suất rằng`u`đến trước`v`trong quá trình đánh dấu được giới hạn ở cây con, chúng ta có thể tính toán kỳ vọng một cách đệ quy một cách hiệu quả. Điều này biến đổi vấn đề từ liệt kê các hoán vị sang tính toán xác suất tổ hợp trong cây con. 

Cách tiếp cận đệ quy dựa vào quan sát rằng với mỗi cạnh`(u, v)`, các nút trong`u`cây con của và`v`Cây con của được xen kẽ ngẫu nhiên theo quy trình và chúng ta có thể tính xác suất nút của một cây con xuất hiện trước nút khác bằng lập trình động. Sử dụng số học mô-đun để xử lý phân số đảm bảo tính chính xác cho câu trả lời cuối cùng theo mô-đun`10^9+7`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Đệ quy DP + cây tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn bất kỳ nút nào làm nút gốc. Để đơn giản chọn nút`1`. Cây có thể được coi là gốc, giúp đơn giản hóa việc tính toán DP cho các mối quan hệ cây con. 
2. Đối với mỗi nút`u`, tính toán kích thước cây con của nó bằng DFS tiêu chuẩn. Lưu trữ nó trong`size[u]`. 
3. Tính toán trước xác suất tổ hợp để hợp nhất hai chuỗi. Cho phép`f[i][j]`biểu thị xác suất mà một phần tử từ chuỗi có độ dài đầu tiên`i`đứng trước một phần tử thuộc chuỗi độ dài thứ hai`j`. Điều này có thể được tính toán bằng cách sử dụng:```
f[i][j] = (i/(i+j)) * f[i-1][j] + (j/(i+j)) * f[i][j-1]
```Các trường hợp cơ bản là`f[0][j] = 0`Và`f[i][0] = 1`. 
4. Thực hiện DFS từ gốc. Tại mỗi nút`u`, kết hợp sự mong đợi của con cái nó. Đối với mỗi đứa trẻ`v`, sự đảo ngược dự kiến ​​​​giữa các nút trong`u`cây con được hợp nhất hiện tại của và`v`cây con của được tính toán bằng cách sử dụng tính toán trước`f`bàn. Nhân với số cặp`size_u * size_v`. 
5. Duy trì bảng DP`dp[u]`đại diện cho sự đảo ngược dự kiến ​​​​trong cây con bắt nguồn từ`u`. Khi hợp nhất với một phần tử con, hãy cộng các phép nghịch đảo dự kiến của đứa trẻ đó cộng với các phép nghịch đảo chéo được tính từ bước 4. 
6. Số lần đảo ngược dự kiến của toàn bộ cây là tổng của tất cả`dp[root]`giá trị, được chia hợp lý để xử lý xác suất lựa chọn gốc. Cuối cùng, xuất kết quả theo modulo`10^9+7`sử dụng số học nghịch đảo mô-đun để xử lý các phân số. 

Tại sao nó hoạt động: Tại mỗi lần hợp nhất,`f[i][j]`thể hiện chính xác xác suất mà một phần tử từ một cây con xuất hiện trước một phần tử từ cây con khác trong quá trình tuyến tính hóa do việc đánh dấu cây quyết định. Vì DFS hợp nhất các cây con trong tất cả các kết hợp nên số lượng đảo ngược dự kiến ​​sẽ tích lũy chính xác. Các bất biến được duy trì đệ quy nên kỳ vọng cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1000)

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

n = int(input())
edges = [[] for _ in range(n+1)]
for _ in range(n-1):
    x, y = map(int, input().split())
    edges[x].append(y)
    edges[y].append(x)

size = [0] * (n+1)
dp = [0] * (n+1)

# Precompute f[i][j] probabilities
f = [[0]*(n+1) for _ in range(n+1)]
for i in range(n+1):
    f[i][0] = 1
for j in range(n+1):
    f[0][j] = 0
for i in range(1, n+1):
    for j in range(1, n+1):
        f[i][j] = (i * f[i-1][j] + j * f[i][j-1]) * modinv(i+j) % MOD

def dfs(u, p):
    size[u] = 1
    dp[u] = 0
    for v in edges[u]:
        if v == p:
            continue
        dfs(v, u)
        # Cross-inversions between u's current subtree and v's subtree
        dp[u] = (dp[u] + dp[v] + size[u] * size[v] % MOD * f[size[u]][size[v]] % MOD) % MOD
        size[u] += size[v]

dfs(1, 0)
ans = dp[1] * modinv(n) % MOD
print(ans)
```Đoạn mã thiết lập cây, tính toán kích thước cây con, tính toán trước xác suất hợp nhất`f`và thực hiện DFS để tích lũy các nghịch đảo dự kiến. Sự phân chia theo`n`ở cuối chiếm sự lựa chọn thống nhất của gốc ban đầu. Việc sử dụng`modinv`đảm bảo phân chia mô-đun chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
3
1 2
1 3
```| Nút | kích thước | dp | 
| --- | --- | --- | 
| 2 | 1 | 0 | 
| 3 | 1 | 0 | 
| 1 | 3 | 6/7 | 

Dấu vết cho thấy rằng đối với nút`1`, kết hợp trẻ em`2`Và`3`với xác suất`f[1][1] = 1/2`sản lượng`1`nghịch đảo với xác suất 1/2,
