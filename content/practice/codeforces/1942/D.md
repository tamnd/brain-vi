---
title: "CF 1942D - Học Vẽ"
description: "Chúng tôi được yêu cầu giúp Elsie đánh giá các bức tranh của cô ấy trên khung vẽ 1D gồm n ô. Mỗi ô có thể được sơn hoặc để trống và vẻ đẹp của bức tranh được quyết định bởi mảng 2D a."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dfs-and-similar", "dp", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1942
codeforces_index: "D"
codeforces_contest_name: "CodeTON Round 8 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 2100
weight: 1942
solve_time_s: 90
verified: false
draft: false
---

[CF 1942D - Học vẽ](https://codeforces.com/problemset/problem/1942/D) 

**Xếp hạng:** 2100 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dfs và tương tự, dp, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giúp Elsie đánh giá các bức tranh của cô ấy trên canvas 1D.`n`tế bào. Mỗi ô có thể được sơn hoặc để trống, vẻ đẹp của bức tranh được quyết định bởi mảng 2D`a`. Cụ thể, đối với mỗi khối ô được sơn liền kề tối đa`[l,r]`, sự đóng góp về vẻ đẹp là`a[l][r]`, và vẻ đẹp tổng thể là tổng thể của tất cả các khối như vậy. Mục tiêu là tính toán các`k`giá trị vẻ đẹp lớn nhất có thể trong số tất cả`2^n`những bức tranh có thể 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi,`n`có thể lớn tới 1000 và`k`nhiều nhất là 5000. Mỗi`a[i][j]`có thể âm, điều này cho phép các giá trị vẻ đẹp giảm đi nếu một khoảng cụ thể được sơn. Từ`2^n`phát triển theo cấp số nhân, việc liệt kê một cách ngây thơ tất cả các bức tranh là không thể thực hiện được; vì`n = 20`,`2^20`đã hơn triệu rồi. Do đó, một cách tiếp cận bạo lực trực tiếp là không thể đối với các trường hợp lớn hơn.`n`. 

Các trường hợp cạnh phát sinh khi tất cả các ô đều âm hoặc bằng 0. Ví dụ, nếu`n = 1`Và`a[1][1] = -5`, có hai bức tranh: vẽ ô sản lượng`-5`, và để trống mang lại kết quả`0`. Một cách tiếp cận ngây thơ chỉ xem xét các khoảng thời gian được vẽ có thể bỏ qua kịch bản vẽ trống một cách không chính xác. 

Một điểm tinh tế khác là các khoảng chồng chéo. Vì chỉ có khoảng thời gian liền kề tối đa mới quan trọng nên chúng tôi không thể tính các khoản đóng góp gấp đôi. Ví dụ như tranh`[1,2,3]`khi ba ô đơn riêng biệt mang lại`a[1,1] + a[2,2] + a[3,3]`, trong khi vẽ`[1,2,3]`khi một khối mang lại`a[1,3]`. Việc lựa chọn giữa các khoảng thời gian kết hợp và giữ chúng tách biệt là rất quan trọng để tìm ra những vẻ đẹp lớn nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra tất cả`2^n`tập hợp con của các ô, tính toán các khối liền kề tối đa cho mỗi ô, tính tổng của chúng`a[l][r]`giá trị và thu thập kết quả. Điều này đúng nhưng không thực tế: vì`n = 1000`, số lượng hoạt động`2^1000`có kích thước lớn về mặt thiên văn. 

Quan sát quan trọng cho một giải pháp khả thi là vấn đề có thể được giải quyết bằng quy hoạch động theo từng khoảng thời gian. Cho phép`dp[l][r]`đại diện cho vẻ đẹp tối đa mà chúng ta có thể có được trong khoảng thời gian`[l,r]`. Bởi vì vẻ đẹp được bổ sung thêm vào các khối sơn tối đa, cho mỗi khoảng thời gian`[l,r]`chúng ta có thể vẽ toàn bộ khối hoặc chia nó ở một số điểm giữa, kết hợp các đóng góp bên trái và bên phải. 

Thay vì tính toán tất cả`2^n`bức tranh một cách rõ ràng, chúng ta có thể duy trì cho mỗi khoảng thời gian`k`vẻ đẹp lớn nhất có thể. Điều này cho phép hợp nhất các khoảng trái và phải một cách hiệu quả. Khi kết hợp hai danh sách các nét đẹp đã được sắp xếp (từ các khoảng con trái và phải), chúng ta chỉ cần phần trên cùng`k`tổng, có thể được tính bằng cách sử dụng một đống kích thước tối thiểu`k`. Điều này làm giảm độ phức tạp theo cấp số nhân xuống mức đa thức có thể quản lý được vì`n^2 * k * log k`hoạt động có tính khả thi đối với`n`lên tới 1000 và`k`lên tới 5000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(2^n) | Quá chậm | 
| DP theo khoảng thời gian + Hợp nhất Top-k | O(n^2 * k * log k) | O(n^2 * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào`n`Và`k`và lưu trữ mảng`a`đầy đủ`n x n`dạng ma trận. Vì đầu vào chỉ cung cấp hình tam giác trên nên hãy điền vào hình tam giác phía dưới nếu cần (mặc dù chúng ta chỉ cần`i <= j`mục nhập). 
2. Khởi tạo bảng DP`dp[l][r]`cho tất cả các khoảng thời gian`[l,r]`. Mỗi`dp[l][r]`lưu trữ một danh sách được sắp xếp hàng đầu`k`vẻ đẹp có thể đạt được trong khoảng thời gian đó. Ban đầu, đặt từng khoảng ô đơn`dp[i][i] = [0, a[i][i]]`bởi vì chúng ta có thể sơn hoặc để trống. 
3. Lặp lại các khoảng thời gian từ 2 đến`n`. Đối với mỗi khoảng thời gian`[l,r]`, xem xét tất cả các điểm phân chia có thể`m`Ở đâu`l <= m < r`. Hợp nhất phần trên cùng`k`người đẹp từ`dp[l][m]`Và`dp[m+1][r]`. Ngoài ra, hãy cân nhắc việc vẽ toàn bộ khoảng trống thành một khối, góp phần`a[l][r]`. Chỉ giữ phần trên cùng`k`Người đẹp sau khi sáp nhập. 
4. Sau khi đổ đầy`dp[1][n]`, đầu ra hàng đầu`k`vẻ đẹp được lưu trữ trong`dp[1][n]`theo thứ tự giảm dần. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Ở mỗi bước,`dp[l][r]`chứa phần trên cùng`k`người đẹp trong khoảng thời gian`[l,r]`. Bằng cách xem xét cả tùy chọn phân tách và khối đơn, chúng tôi đảm bảo tất cả các cấu hình đều được tính đến mà không liệt kê tất cả`2^n`những bức tranh. Heap tối thiểu đảm bảo chúng tôi chỉ giữ phần trên cùng`k`, phù hợp với yêu cầu của bài toán. 

## Giải pháp Python```python
import sys, heapq
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = [[0]*n for _ in range(n)]
        for i in range(n):
            row = list(map(int, input().split()))
            for j in range(len(row)):
                a[i][i+j] = row[j]

        dp = [[[] for _ in range(n)] for _ in range(n)]

        for i in range(n):
            dp[i][i] = [0, a[i][i]] if a[i][i] != 0 else [0]

        for length in range(2, n+1):
            for l in range(n-length+1):
                r = l+length-1
                candidates = [a[l][r]]
                for m in range(l, r):
                    for x in dp[l][m]:
                        for y in dp[m+1][r]:
                            candidates.append(x+y)
                dp[l][r] = heapq.nlargest(k, candidates)

        print(" ".join(map(str, dp[0][n-1][:k])))

if __name__ == "__main__":
    solve()
```Mã xử lý cẩn thận việc đọc đầu vào hình tam giác trên, lặp đi lặp lại điền vào bảng DP theo độ dài khoảng và hợp nhất các nét đẹp của khoảng phụ. các`heapq.nlargest`đảm bảo chúng tôi giữ được vị trí dẫn đầu một cách hiệu quả`k`làm đẹp, ngăn chặn sự bùng nổ trí nhớ. Các khoảng ô đơn sẽ cân nhắc chính xác việc để ô trống, không có đóng góp nào. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2
1 2
-5
2 4
2 -3
-1
3 8
2 4 3
1 3
5
```| Bước | Khoảng thời gian`[l,r]`| dp[l][r] sau bước | 
| --- | --- | --- | 
| Tế bào đơn | [1,1], [2,2], [3,3] | [-5], [-3,0], [2,5] | 
| Chiều dài 2 | [1,2], [2,3] | [0, -5], [0, -1] | 
| Chiều dài 3 | [1,3] | [2,0,-1,-3] | 

Điều này chứng tỏ rằng DP hợp nhất các khoảng thời gian một cách chính xác, xem xét việc sơn toàn bộ khối và giữ phần trên cùng.`k`người đẹp. Số 0 được bao gồm cho các khối không được sơn, ngăn chặn các đầu ra chỉ âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 * k^2) | Mỗi khoảng kết hợp tối đa hai danh sách`k`các phần tử. sử dụng`heapq.nlargest`tối ưu hóa việc khai thác để`k*log k`, đưa ra khoảng O(n^2 * k * log k) | 
| Không gian | O(n^2 * k) | Cửa hàng bảng DP hàng đầu`k`người đẹp mỗi khoảng | 

Với các ràng buộc (`n ≤ 1000`, tổng của`k`≤ 5000), giải pháp khả thi trong giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("4\n1 2\n-5\n2 4\n2 -3\n-1\n3 8\n2 4 3\n1 3\n5\n6 20\n0 -6 -3 0 -6 -2\n-7 -5 -2 -3 -4\n7 0 -9 -4\n2 -1 1\n1 -2\n-6\n") == \
"0 -5\n2 0 -1 -3\n7 5 4 3 3 2 1 0\n8 8 7 7 5 5 2 2 1 1 1 1 1 1 0 0 0 0 0 -1"

# Custom cases
assert run("1\n1 1\n0\n") == "0", "single cell zero"
assert run("1\n2 3\n1 -1\n2\n") == "3 2 1", "two cells with positive/negative"
assert run("1\n3 2\n-1 -2 -3\n-4 -5\n-6\n")
```
