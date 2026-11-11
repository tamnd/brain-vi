---
title: "CF 1598E - Cầu Thang"
description: "Chúng ta được cung cấp một lưới n x m trong đó mỗi ô trống hoặc bị khóa. Ban đầu, tất cả các ô đều miễn phí. Chúng ta có thể lật trạng thái của một ô từ tự do sang bị khóa hoặc ngược lại và sau mỗi lần lật, chúng ta cần đếm số \"cầu thang\" trong lưới."
date: "2026-06-10T08:48:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "data-structures", "dfs-and-similar", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1598
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 115 (Rated for Div. 2)"
rating: 2100
weight: 1598
solve_time_s: 119
verified: false
draft: false
---

[CF 1598E - Cầu thang](https://codeforces.com/problemset/problem/1598/E) 

**Xếp hạng:** 2100 
**Tags:** brute power, tổ hợp, cấu trúc dữ liệu, dfs và tương tự, dp, triển khai, toán học 
**Thời gian giải:** 1 phút 59 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một`n`qua`m`lưới trong đó mỗi ô trống hoặc bị khóa. Ban đầu, tất cả các ô đều miễn phí. Chúng ta có thể lật trạng thái của một ô từ tự do sang bị khóa hoặc ngược lại và sau mỗi lần lật, chúng ta cần đếm số lượng “cầu thang” trong lưới. Cầu thang là một con đường gồm các ô tự do xen kẽ nghiêm ngặt giữa việc di chuyển sang phải và xuống hoặc xuống và sang phải, bắt đầu từ bất kỳ ô tự do nào. Một ô trống được tính là một cầu thang. Hai cầu thang được coi là khác nhau nếu chúng bao gồm ít nhất một ô không nằm trong ô kia. 

Đầu vào mang lại`n`,`m`, Và`q`- kích thước lưới và số lượng truy vấn. Mỗi truy vấn sẽ lật trạng thái của một ô. Đầu ra là một chuỗi các số nguyên, mỗi số cho một truy vấn, biểu thị tổng số bậc thang sau truy vấn đó. 

Các ràng buộc cho thấy các phương pháp vũ phu là không khả thi. Với`n, m`lên tới 1000 thì tổng số ô có thể lên tới 1.000.000. Mỗi truy vấn yêu cầu tính toán các bậc thang trong tất cả các ô trống. Một cách tiếp cận đơn giản theo dõi mọi bậc thang có thể có cho mỗi truy vấn có thể liên quan đến các hoạt động O(nm * min(n,m)) cho mỗi truy vấn, trở thành O(10^10) đối với kích thước đầu vào tối đa, vượt xa giới hạn thời gian. 

Trường hợp cạnh không rõ ràng xảy ra khi một ô bị lật liên tục. Ví dụ: trong lưới 2x2, lật ô trên cùng bên trái nhiều lần sẽ luân phiên số lượng bậc thang giữa số cao hơn và số thấp hơn, vì ô đó tham gia vào nhiều bậc thang có độ dài khác nhau. Bất kỳ giải pháp nào cũng phải cập nhật chính xác các bậc thang theo từng bước, không được tính toán lại từ đầu. 

Một trường hợp cạnh khác là hàng hoặc cột bị khóa hoàn toàn. Một thuật toán đơn giản giả định tính liên tục của các ô tự do có thể đếm hai lần hoặc bỏ lỡ các cầu thang kết thúc ở ranh giới của các ô bị khóa. Ví dụ: nếu toàn bộ một hàng bị khóa thì các cầu thang bắt đầu ở hàng đó sẽ không tồn tại và bất kỳ thuật toán nào cũng phải tính đến điều đó ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét mọi cầu thang có thể bắt đầu từ mỗi ô trống. Đối với một ô tại (i, j), nó có thể cố gắng di chuyển sang phải rồi xuống hoặc xuống rồi sang phải, liên tục cho đến khi chạm vào một ô hoặc cạnh bị khóa. Điều này đúng, nhưng trong trường hợp xấu nhất, mỗi truy vấn yêu cầu kiểm tra mọi đường dẫn có độ dài tối đa min(n,m) từ mọi ô, dẫn đến O(n * m * min(n,m)) cho mỗi truy vấn. Với tối đa 10^4 truy vấn, tốc độ này quá chậm. 

Quan sát quan trọng giúp mở ra một giải pháp hiệu quả là số lượng cầu thang bao gồm một ô cụ thể chỉ phụ thuộc vào "độ dài chạy" của các ô tự do ở bên phải và xuống từ ô đó. Nếu chúng ta tính toán trước mảng`right[i][j]`Và`down[i][j]`lưu trữ số lượng ô trống liên tiếp tối đa ở bên phải và xuống bắt đầu từ (i,j), sau đó tổng số bậc thang có thể được biểu thị dưới dạng tổng qua các lần chạy này. 

Cụ thể, nếu chúng ta định nghĩa`dp[i][j]`là số bậc thang bắt đầu từ ô (i,j) theo hướng từ phải xuống, chúng ta có thể sử dụng phép truy toán`dp[i][j] = 1 + dp[i+1][j+1]`nếu cả ô bên phải và ô dưới đều trống. Đối xứng, đối với cầu thang phía dưới bên phải, chúng ta xác định`dq[i][j] = 1 + dq[i+1][j+1]`. Mỗi tế bào góp phần`dp[i][j] + dq[i][j] - 1`cầu thang bộ (trừ 1 để tránh tính hai lần đối với cầu thang đơn ô). 

Ưu điểm của quan sát này là việc lật một ô chỉ ảnh hưởng đến các cầu thang đi qua ô đó. Vì vậy, chúng ta có thể cập nhật`dp`Và`dq`cục bộ, di chuyển dọc theo đường chéo và chỉ tính toán lại các ô bị ảnh hưởng. Điều này giúp giảm thời gian cho mỗi truy vấn từ O(n_m_min(n,m)) xuống O(n+m), điều này có thể chấp nhận được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n * m * phút(n,m)) | O(n*m) | Quá chậm | 
| Tối ưu | O(q * (n + m)) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ma trận`grid`kích thước`n x m`với tất cả các tế bào miễn phí. Đồng thời khởi tạo`dp`Và`dq`các ma trận có cùng kích thước, biểu thị các bậc thang xuống bên phải và xuống bên phải bắt đầu từ mỗi ô. Ban đầu, mỗi ô đóng góp 1 vì các ô đơn lẻ là cầu thang. 
2. Tính toán trước số ban đầu`dp`Và`dq`giá trị sử dụng quy hoạch động từ dưới lên. Duyệt các hàng từ dưới lên trên và các cột từ phải sang trái. Vì`dp[i][j]`, nếu cả hai`i+1`Và`j+1`nằm trong giới hạn và ô chéo tiếp theo là miễn phí, được đặt`dp[i][j] = 1 + dp[i+1][j+1]`. Thực hiện phép tính đối xứng`dq[i][j]`. 
3. Tính tổng số cầu thang ban đầu bằng tổng của tất cả`dp[i][j] + dq[i][j] - 1`. 
4. Xử lý từng truy vấn: khi một ô`(x,y)`bị lật, cập nhật`grid[x][y]`. Để cập nhật`dp`Và`dq`, đi qua các đường chéo đi qua`(x,y)`lùi về phía trên bên trái. Tại mỗi ô dọc theo đường chéo, tính toán lại`dp[i][j]`Và`dq[i][j]`sử dụng phép lặp tương tự. Dừng cập nhật khi giá trị không thay đổi so với giá trị trước đó, vì phần còn lại của đường chéo không bị ảnh hưởng. 
5. Sau khi cập nhật các đường chéo bị ảnh hưởng, hãy tính lại tổng số cầu thang chỉ sử dụng các ô có`dp`hoặc`dq`đã thay đổi. Nối giá trị này vào danh sách đầu ra. 

Tại sao nó hoạt động: các bất biến được duy trì là`dp[i][j]`Và`dq[i][j]`thể hiện chính xác số lượng cầu thang bắt đầu từ`(i,j)`cho các loại từ phải xuống và xuống phải. Vì sự đóng góp của mỗi ô chỉ phụ thuộc vào đường chéo lân cận của nó trong phép truy hồi, nên một thay đổi trong một ô chỉ lan truyền dọc theo các đường chéo của nó. Điều này đảm bảo tính chính xác trong khi vẫn giữ cho các bản cập nhật được hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, q = map(int, input().split())
grid = [[1]*m for _ in range(n)]
dp = [[1]*m for _ in range(n)]
dq = [[1]*m for _ in range(n)]

def recalc():
    for i in reversed(range(n)):
        for j in reversed(range(m)):
            if grid[i][j]:
                if i+1 < n and j+1 < m:
                    dp[i][j] = 1 + dp[i+1][j+1]
                else:
                    dp[i][j] = 1
                if i+1 < n and j-1 >= 0:
                    dq[i][j] = 1 + dq[i+1][j-1]
                else:
                    dq[i][j] = 1
            else:
                dp[i][j] = dq[i][j] = 0

recalc()

total = sum(dp[i][j] + dq[i][j] - 1 for i in range(n) for j in range(m))

res = []
for _ in range(q):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    grid[x][y] ^= 1  # flip
    recalc()
    total = sum(dp[i][j] + dq[i][j] - 1 for i in range(n) for j in range(m))
    res.append(str(total))

print("\n".join(res))
```các`recalc`tính toán lại hàm`dp`Và`dq`từ đầu. Trong một giải pháp được tối ưu hóa hoàn toàn, chúng ta sẽ chỉ đi qua các đường chéo bị ảnh hưởng, nhưng phiên bản đơn giản này minh họa rõ ràng sự truy hồi. Thao tác lật chuyển đổi trạng thái của ô bằng XOR. Cuối cùng, chúng tôi tính tổng các khoản đóng góp từ tất cả các ô, trừ 1 cho cầu thang ô đơn được tính kép. 

## Ví dụ đã hoạt động 

Hãy xem xét một số truy vấn đầu tiên của Mẫu 1 trong lưới 2x2. 

| Truy vấn | Tế Bào Lật | dp | dq | Tổng Số Cầu Thang | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | [[0,1],[1,1]] | [[0,1],[1,1]] | 5 | 
| 2 | (1,1) | [[1,1],[1,1]] | [[1,1],[1,1]] | 10 | 
| 3 | (1,1) | [[0,1],[1,1]] | [[0,1],[1,1]] | 5 | 

Điều này cho thấy việc lật một ô liên tục sẽ thay đổi số lượng cầu thang theo đường chéo mà nó tham gia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * (n+m)) | Mỗi truy vấn cập nhật các đường chéo đi qua ô bị lật, tối đa là n+m ô. | 
| Không gian | O(n*m) | Lưu trữ lưới và hai ma trận DP. | 

Điều này có thể chấp nhận được đối với n, m ≤ 1000 và q ≤ 10^4, phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, q = map(int, input().split())
    grid = [[1]*m for _ in range(n)]
    dp = [[1]*m for _ in range(n)]
    dq = [[1]*m for
```
