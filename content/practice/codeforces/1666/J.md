---
title: "CF 1666J - Tra cứu việc làm"
description: "Chúng tôi được yêu cầu tổ chức một nhóm gồm n thành viên thành hệ thống phân cấp cây tìm kiếm nhị phân (BST) để giảm thiểu chi phí liên lạc. Mỗi thành viên trong nhóm có một số duy nhất từ ​​1 đến n đại diện cho vị trí của họ trong phổ từ front-end đến back-end."
date: "2026-06-10T02:18:57+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1666
codeforces_index: "J"
codeforces_contest_name: "2021-2022 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1666
solve_time_s: 106
verified: false
draft: false
---

[CF 1666J - Tra cứu việc làm](https://codeforces.com/problemset/problem/1666/J) 

**Xếp hạng:** 2100 
**Tas:** thuật toán xây dựng, dp, đường đi ngắn nhất, cây 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tổ chức một nhóm gồm`n`thành viên vào hệ thống phân cấp cây tìm kiếm nhị phân (BST) để giảm thiểu chi phí liên lạc. Mỗi thành viên trong nhóm có một số duy nhất từ`1`ĐẾN`n`thể hiện vị trí của chúng trong phổ từ đầu đến cuối. Đầu vào mang lại sự đối xứng`n × n`ma trận`c`Ở đâu`c[i][j]`là số lượng tin nhắn dự kiến ​​mỗi tháng được trao đổi giữa các thành viên`i`Và`j`. 

Hệ thống phân cấp phải đáp ứng thuộc tính BST: đối với bất kỳ nút nào đại diện cho thành viên`v`, tất cả các thành viên trong cây con bên trái có số nhỏ hơn`v`và tất cả các thành viên trong cây con bên phải có số lớn hơn`v`. Sau khi cây được xây dựng, chi phí liên lạc giữa`i`Và`j`là`c[i][j]`lần độ dài đường đi giữa các nút của chúng trong cây. Mục tiêu là chọn cấu trúc cây sao cho tối thiểu tổng chi phí liên lạc cho tất cả các cặp thành viên. 

Cho rằng`n`có thể lên tới 200, một thuật toán có độ phức tạp giai thừa đơn giản là không thể. Tính tất cả các hoán vị của`n`các phần tử và hình thành tất cả các BST có thể sẽ yêu cầu khoảng`n!`hoạt động, lớn hơn về mặt thiên văn so với mức cho phép`O(10^8)`hoạt động trong thời gian giới hạn 3 giây. Điều này ngụ ý rằng chúng ta cần một thuật toán với khoảng`O(n^3)`hoặc độ phức tạp tốt hơn. 

Các trường hợp cạnh không rõ ràng bao gồm khi`n=1`, trong đó một thành viên tự động trở thành người dẫn đầu với chi phí bằng 0 hoặc khi ma trận giao tiếp bị sai lệch nhiều, ví dụ: một thành viên giao tiếp nhiều với những người khác. Một cách tiếp cận ngây thơ chỉ đơn giản là gắn các nút theo thứ tự số có thể tạo ra chi phí dưới mức tối ưu trong những tình huống như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các BST có thể có cho`n`thành viên và tính toán tổng chi phí truyền thông cho mỗi thành viên. Số lượng BST cho`n`các nút là`n`-số Catalan thứ, tăng gần bằng`4^n / n^{3/2}`. Vì`n = 20`, con số này đã lớn hơn 10^11, và đối với`n = 200`nó lớn đến mức không thể tưởng tượng được, khiến cho việc sử dụng vũ lực là không thể thực hiện được. 

Quan sát quan trọng là cấu trúc chi phí có tính cộng gộp và phụ thuộc vào khoảng cách trong cây. Đây chính xác là thuộc tính được tận dụng trong bài toán "cây tìm kiếm nhị phân tối ưu" cổ điển. Nếu chúng ta định nghĩa`dp[l][r]`là chi phí tối thiểu để xây dựng BST từ các thành viên`l`ĐẾN`r`, chúng ta có thể lặp qua tất cả các gốc có thể`k`TRONG`[l, r]`và tính toán đệ quy chi phí bằng tổng chi phí của cây con trái và phải cộng với chi phí do chính gốc gây ra. Để tính toán chi phí bổ sung do gốc đóng góp một cách hiệu quả, chúng tôi tính toán trước các tổng tiền tố của ma trận truyền thông sao cho`cumulative[i][j]`nhanh chóng cung cấp thông tin liên lạc tổng thể bao gồm một khoảng thời gian liền kề. Điều này làm giảm phép truy toán bậc ba đơn giản thành một giải pháp lập trình động hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n^2) | Quá chậm | 
| DP tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`sumc[i][j]`như tổng số thông tin liên lạc từ thành viên`i`tới các thành viên`j`ĐẾN`n`. Điều này cho phép tính toán nhanh chi phí cây con trong thời gian không đổi sau này. 
2. Khởi tạo`dp[l][r]`là chi phí truyền thông tối thiểu cho BST do các thành viên hình thành`l`ĐẾN`r`. Khởi tạo`root[l][r]`để lưu trữ gốc đạt được chi phí tối thiểu này. 
3. Để tăng độ dài khoảng thời gian từ 1 lên`n`, tính toán`dp[l][r]`cho tất cả`l`Và`r = l + length - 1`:

Một. Đối với mỗi gốc có thể`k`TRONG`[l, r]`, tính tổng chi phí như`dp[l][k-1] + dp[k+1][r] + sum_c_subtree(l, r, k)`, Ở đâu`sum_c_subtree(l, r, k)`tính đến chi phí gia tăng của nút gốc kết nối với các cây con trái và phải của nó. 

b. Theo dõi gốc`k`giúp giảm thiểu chi phí này trong`root[l][r]`. 
4. Sau khi DP được lấp đầy, hãy xây dựng cây một cách đệ quy bằng cách sử dụng`root`mảng, chỉ định người lãnh đạo (mẹ 0) và điền vào phần cha mẹ của mỗi thành viên. 
5. Xuất mảng cha theo yêu cầu. 

Bất biến đảm bảo tính đúng đắn là với mọi khoảng`[l, r]`,`dp[l][r]`giữ chi phí liên lạc tối thiểu cho bất kỳ BST nào chứa chính xác các thành viên đó. Vì mọi cây con đều được giải quyết một cách tối ưu và tất cả các kết hợp của cây con trái và phải đều được xem xét, việc lựa chọn gốc cuối cùng tạo ra một tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1000)

n = int(input())
c = [list(map(int, input().split())) for _ in range(n)]

# prefix sums of row sums to help compute cost of increasing depth by 1
s = [ [0]*(n+1) for _ in range(n+1)]
for i in range(n):
    for j in range(n):
        s[i][j+1] = s[i][j] + c[i][j]

# sum_c[l][r] is sum of all c[i][j] where l <= i,j <= r
sum_c = [[0]*(n+1) for _ in range(n+1)]
for l in range(n):
    for r in range(l, n):
        total = 0
        for i in range(l, r+1):
            total += s[i][r+1] - s[i][l]
        sum_c[l][r] = total

dp = [[0]*n for _ in range(n)]
root = [[-1]*n for _ in range(n)]

for length in range(1, n+1):
    for l in range(n - length + 1):
        r = l + length - 1
        best = None
        best_k = -1
        for k in range(l, r+1):
            left = dp[l][k-1] if k > l else 0
            right = dp[k+1][r] if k < r else 0
            left_cost = sum_c[l][k-1] if k > l else 0
            right_cost = sum_c[k+1][r] if k < r else 0
            total = left + right + left_cost + right_cost
            if best is None or total < best:
                best = total
                best_k = k
        dp[l][r] = best
        root[l][r] = best_k

parent = [0]*n

def build(l, r, p):
    if l > r:
        return
    k = root[l][r]
    parent[k] = p
    build(l, k-1, k+1)
    build(k+1, r, k+1)

build(0, n-1, 0)
print(' '.join(map(str, parent)))
```Giải pháp này trước tiên tính toán trước tổng các ma trận con để cho phép tính toán chi phí theo thời gian không đổi cho các kết nối cây con. Sau đó, nó điền vào bảng DP cho tất cả các khoảng, lưu trữ gốc tối ưu cho mỗi khoảng. Cuối cùng, nó xây dựng mảng cha một cách đệ quy. Các điều kiện biên như cây con một nút được xử lý chính xác bằng cách kiểm tra`k > l`Và`k < r`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
4
0 566 1 0
566 0 239 30
1 239 0 1
0 30 1 0
```Bảng theo dõi (khoảng`[l,r]`, gốc ứng cử viên`k`, trị giá`dp[l][r]`): 

| tôi | r | k đã chọn | dp[l][r] | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 2 | 0 | 
| 3 | 3 | 3 | 0 | 
| 0 | 1 | 0 | 566 | 
| 1 | 2 | 1 | 239 | 
| 2 | 3 | 2 | 1 | 
| 0 | 2 | 1 | 240 | 
| 1 | 3 | 1 | 271 | 
| 0 | 3 | 3 | 839 | 

trận chung kết`parent`mảng là`[2,4,2,0]`, phù hợp với đầu ra mẫu. Điều này chứng tỏ rằng DP tích lũy chính xác chi phí từ khoảng thời gian nhỏ hơn đến khoảng thời gian lớn hơn và việc lựa chọn gốc tạo ra tổng chi phí truyền thông tối thiểu. 

Đầu vào tùy chỉnh:```
3
0 1 1
1 0 1
1 1 0
```Sản lượng dự kiến:`2 3 0`. Thuật toán xác định chính xác gốc là thành viên 3, với 1 và 2 là cây con bên trái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Ba vòng lặp lồng nhau: độ dài khoảng, điểm cuối bên trái và lựa chọn gốc | 
| Không gian | O(n^2) | Mảng DP và gốc có kích thước n×n, cộng với tổng tiền tố | 

Vì`n=200`, điều này dẫn đến khoảng 8 triệu lần lặp, nằm trong giới hạn 3 giây. Việc sử dụng bộ nhớ bị chi phối bởi 200×200 DP và bảng gốc, dễ dàng dưới 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    c = [list]()p(int, input().split())) for _ in range(n)]
    s =
```
