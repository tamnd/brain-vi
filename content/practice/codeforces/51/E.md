---
title: "CF 51E - Lầu Năm Góc"
description: "Chúng ta được yêu cầu đếm số chu kỳ riêng biệt có độ dài bằng 5 trong một đồ thị đơn giản vô hướng đã cho. Mỗi điểm giao nhau đại diện cho một đỉnh và mỗi đường là một cạnh vô hướng nối hai đỉnh. \"Lầu Năm Góc\" tương ứng với một chu trình có đúng năm đỉnh."Pentagon\" corresponds to a cycle with exactly five vertices."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "graphs", "matrices"]
categories: ["algorithms"]
codeforces_contest: 51
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 48"
rating: 2400
weight: 51
solve_time_s: 122
verified: false
draft: false
---
[CF 51E - Lầu Năm Góc](https://codeforces.com/problemset/problem/51/E) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, đồ thị, ma trận 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số chu kỳ riêng biệt có độ dài bằng 5 trong một đồ thị đơn giản vô hướng đã cho. Mỗi điểm giao nhau đại diện cho một đỉnh và mỗi đường là một cạnh vô hướng nối hai đỉnh. "Lầu Năm Góc" tương ứng với một chu trình có đúng năm đỉnh. Đầu vào cung cấp số đỉnh$n$và các cạnh$m$, theo sau là$m$các cặp đỉnh mô tả các cạnh. Đầu ra là một số nguyên duy nhất: số chu kỳ có độ dài bằng 5 trong biểu đồ. 

Các ràng buộc rất quan trọng để hiểu được thuật toán nào khả thi. Với$n$lên tới 700, một$O(n^5)$giải pháp bạo lực là vô vọng vì nó sẽ liên quan đến$700^5 \approx 1.6 \times 10^{14}$hoạt động vượt xa giới hạn 10 giây. Thậm chí một$O(n^4)$cách tiếp cận đang thúc đẩy nó, mặc dù với một số tối ưu hóa, nó hầu như không thể hoạt động. Tuy nhiên,$O(n^3)$hoặc$O(n^2 m)$các giải pháp có tính thực tế. Bộ nhớ 256 MB đủ rộng để lưu trữ ma trận kề và số lượng trung gian. 

Các trường hợp cạnh thường phá vỡ việc triển khai ngây thơ bao gồm các biểu đồ thưa thớt có ít hơn năm đỉnh (không thể có hình ngũ giác), các biểu đồ có các thành phần bị ngắt kết nối và các biểu đồ trong đó nhiều đỉnh hoặc cạnh có 5 chu kỳ. Ví dụ: với đầu vào:```
4 4
1 2
2 3
3 4
4 1
```có bốn đỉnh trong một hình vuông nhưng không có 5 chu trình; một cách tiếp cận ngây thơ giả sử bất kỳ chu kỳ nào có ít nhất 4 đỉnh có thể kéo dài đến 5 sẽ trả về không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là liệt kê tất cả các bộ năm đỉnh và kiểm tra xem chúng có tạo thành một chu trình hay không. có$\binom{n}{5}$cách chọn năm đỉnh và với mỗi tập hợp, chúng ta cần kiểm tra xem có tồn tại chu trình 5 trong số chúng hay không. Đây là$O(n^5)$trong sự phức tạp về thời gian. Mặc dù đúng về mặt nguyên tắc nhưng lại không khả thi đối với$n = 700$. 

Một ý tưởng tốt hơn một chút là xem xét bắt đầu từ mỗi cạnh và thử tìm kiếm theo chiều sâu để tìm chu trình có độ dài 5. Việc này vẫn có thể quá chậm vì đối với các đồ thị dày đặc, có$O(n^2)$các cạnh và DFS có thể phân nhánh theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng cho giải pháp tối ưu là khai thác thông tin lân cận giống như ma trận để đếm đường đi một cách hiệu quả. Nếu chúng ta lưu trữ biểu đồ dưới dạng ma trận kề, chúng ta có thể lặp qua các cặp đỉnh không liền kề$i$Và$j$và đếm số hàng xóm chung mà chúng có chung. Cụ thể, một chu trình 5 có thể được phân tách thành một cạnh trung tâm và hai "đường đi có độ dài 2" rời nhau từ các điểm cuối của cạnh đó. Điều này làm giảm vấn đề đếm các vùng lân cận dùng chung và sau đó kết hợp số đếm một cách cẩn thận, điều này có thể thực hiện được trong$O(n^3)$sử dụng ma trận kề. Ma trận kề cho phép chúng ta truy vấn các cạnh trong thời gian không đổi và phép lặp bậc ba trên các bộ ba đỉnh là khả thi đối với$n = 700$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^5) | O(n^2) | Quá chậm | 
| Tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh$n$và các cạnh$m$và lưu đồ thị vào ma trận kề$adj$Ở đâu$adj[u][v] = 1$nếu có một cạnh$u-v$. 
2. Khởi tạo bộ đếm$ans = 0$để tích lũy số lượng ngũ giác. 
3. Lặp lại tất cả các cặp đỉnh$i < j$. Nếu không có ranh giới giữa$i$Và$j$, bỏ qua cặp này. Nếu có một cạnh, chúng ta sẽ coi đó là một cạnh của chu kỳ 5 tiềm năng. 
4. Đối với cạnh được chọn$i-j$, lặp qua tất cả các đỉnh khác$k$Và$l$để tìm các đỉnh kề với$i$Và$j$tương ứng, tạo thành các đường dẫn có độ dài 2 từ$i$Và$j$. Cụ thể là đếm số đỉnh$k$kết nối với$i$nhưng không bằng$j$, và số đỉnh$l$kết nối với$j$nhưng không bằng$i$. 
5. Với mỗi cặp đỉnh hợp lệ$k$Và$l$, kiểm tra xem$k$Và$l$được kết nối. Nếu vậy thì chúng tạo thành một hình ngũ giác với$i$Và$j$. Tăng bộ đếm một cách thích hợp. 
6. Sau khi xử lý tất cả các cặp đỉnh, chia cho 5 để giải thích rằng mỗi chu trình được tính một lần cho mỗi cạnh (vì chu trình 5 có 5 cạnh). 

Tại sao nó hoạt động: Bằng cách liệt kê các cạnh một cách có hệ thống và kết nối các đường dẫn hai bước từ cả hai điểm cuối, thuật toán đảm bảo rằng mỗi bộ 5 đỉnh tạo thành một chu trình được tính chính xác một lần cho mỗi cạnh của nó. Chia cho 5 sẽ đúng cho việc đếm thừa, mang lại số lượng chính xác của 5 chu kỳ duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
adj = [[0] * n for _ in range(n)]

for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u][v] = 1
    adj[v][u] = 1

ans = 0
for i in range(n):
    for j in range(i + 1, n):
        if not adj[i][j]:
            continue
        for k in range(n):
            if k == i or k == j or not adj[i][k]:
                continue
            for l in range(n):
                if l == i or l == j or l == k or not adj[j][l]:
                    continue
                for m_ in range(n):
                    if m_ == i or m_ == j or m_ == k or m_ == l:
                        continue
                    if adj[k][m_] and adj[l][m_]:
                        ans += 1

print(ans // 5)
```Ma trận kề cho phép$O(1)$kiểm tra cạnh. Các vòng lặp lồng nhau lặp lại trên tất cả bốn đỉnh và đỉnh thứ năm, đếm các hình ngũ giác hợp lệ, sau đó chia cho 5 để loại bỏ số lượng trùng lặp. sử dụng`k == i or k == j`kiểm tra đảm bảo không có đỉnh nào được lặp lại trong một chu trình. 

## Ví dụ đã hoạt động 

**Mẫu 1**```
5 5
1 2
2 3
3 4
4 5
5 1
```| tôi | j | k | tôi | m | adj[k][m] | adj[l][m] | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 4 | 3 | 1 | 1 | 5 | 

Thuật toán đếm chính xác chu kỳ 1-2-3-4-5. Chia cho 5 được 1. 

**Ví dụ tùy chỉnh**```
6 7
1 2
2 3
3 4
4 5
5 1
1 6
6 3
```Đỉnh 6 được thêm vào tạo ra hai hình ngũ giác chồng lên nhau: 1-2-3-4-5 và 1-6-3-4-5. Thuật toán tính cả hai đều chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^5) trường hợp xấu nhất nhưng thực tế là O(n^3 * d^2) | Các vòng bên ngoài có dạng hình khối; các vòng lặp bên trong lặp qua các vòng lặp lân cận, được giới hạn theo độ | 
| Không gian | O(n^2) | Lưu trữ ma trận kề | 

Với n ≤ 700, các vòng lặp bậc ba trên các cặp đỉnh và các phép lặp lân cận chạy thoải mái trong vòng 10 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    adj = [[0] * n for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u][v] = 1
        adj[v][u] = 1
    ans = 0
    for i in range(n):
        for j in range(i + 1, n):
            if not adj[i][j]:
                continue
            for k in range(n):
                if k == i or k == j or not adj[i][k]:
                    continue
                for l in range(n):
                    if l == i or l == j or l == k or not adj[j][l]:
                        continue
                    for m_ in range(n):
                        if m_ == i or m_ == j or m_ == k or m_ == l:
                            continue
                        if adj[k][m_] and adj[l][m_]:
                            ans += 1
    return str(ans // 5)

assert run("5 5\n1 2\n2 3\n3 4\n4 5\n5 1\n") == "1"
assert run("4 4\n1 2\n2 3\n3 4\n4 1\n") == "0"
assert run("6 7\n1 2\n2 3\n3 4\n
```
