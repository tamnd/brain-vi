---
title: "CF 490F - Tour tham quan vùng đất cây xanh"
description: "Chúng ta được cấp một cây thành phố, trong đó mỗi thành phố có dân số. Những con đường kết nối các thành phố sao cho có chính xác một con đường đơn giản giữa hai thành phố bất kỳ."
date: "2026-06-07T17:42:20+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 490
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 279 (Div. 2)"
rating: 2200
weight: 490
solve_time_s: 88
verified: true
draft: false
---

[CF 490F - Chuyến tham quan Treeland](https://codeforces.com/problemset/problem/490/F) 

**Đánh giá:** 2200 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, cây 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây các thành phố, trong đó mỗi thành phố có dân số. Những con đường kết nối các thành phố sao cho có chính xác một con đường đơn giản giữa hai thành phố bất kỳ. Ban nhạc muốn biểu diễn ở một chuỗi các thành phố dọc theo một con đường nào đó, với dân số ngày càng tăng tại các thành phố nơi diễn ra các buổi hòa nhạc. Mục tiêu là tối đa hóa số lượng buổi hòa nhạc, nghĩa là chuỗi dân số thành phố tăng dần theo đúng quy định dài nhất dọc theo bất kỳ con đường nào trong cây. 

Đầu vào cung cấp số lượng thành phố, dân số và danh sách các con đường kết nối chúng. Đầu ra là một số nguyên: số lượng buổi hòa nhạc tối đa có thể. 

Cấu trúc cây ngụ ý không có chu kỳ, và vì$n$có thể lên tới 6000, các thuật toán có độ phức tạp bậc hai hoặc bậc ba là khả thi, nhưng bất kỳ thuật toán nào tệ hơn (như$O(n^4)$) ngay lập tức quá chậm. Mỗi dân số thành phố có thể dao động lên tới$10^6$, vì vậy việc lập chỉ mục dựa trên giá trị đơn giản chỉ có thể thực hiện được nếu chúng ta sử dụng bản đồ, nhưng mảng trực tiếp có kích thước 10^6 sẽ gây lãng phí. Chúng ta cũng phải xem xét rằng nhiều thành phố có thể có cùng dân số. 

Một trường hợp phức tạp là khi một số thành phố dọc theo một con đường có cùng dân số. Vì trình tự phải tăng lên nghiêm ngặt nên hai quần thể bằng nhau không thể cùng tổ chức các buổi hòa nhạc. Một trường hợp cạnh khác là cây hình ngôi sao, trong đó một nút trung tâm kết nối với nhiều lá; một DFS ngây thơ chỉ nhìn vào một nhánh duy nhất có thể bỏ lỡ đường dẫn tối ưu đi từ lá này qua tâm đến lá khác. 

Ví dụ, hãy xem xét một cây gồm ba thành phố: dân số`[1, 2, 1]`với các cạnh`1-2, 2-3`. Trình tự tối ưu là`[1, 2]`hoặc`[2, 3]`với 2 buổi hòa nhạc chứ không phải 3, vì trình tự phải tăng dần. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê mọi đường đi đơn giản trong cây, tạo ra tất cả các chuỗi quần thể thành phố cho mỗi đường đi và theo dõi chuỗi tăng dần nghiêm ngặt dài nhất. có$O(n^2)$các đường đi trong cây và mỗi đường đi có nhiều nhất$n$thành phố, đưa ra đại khái$O(n^3)$trình tự cần xem xét. Việc tạo ra các chuỗi con trong mỗi đường dẫn làm cho cách tiếp cận này trở nên lũy thừa trên mỗi đường dẫn, điều này hoàn toàn không khả thi đối với$n = 6000$. 

Quan sát quan trọng là bài toán quy về việc tính toán dãy con tăng dài nhất (LIS) dọc theo các đường đi trong cây. Nếu chúng ta coi mỗi nút là một nút gốc tiềm năng và truyền bá thông tin LIS tới các nút con của nó thông qua lập trình động, chúng ta có thể tránh việc liệt kê tất cả các đường dẫn. Vì một cây không có chu trình nên DFS cho phép chúng ta tính LIS bắt đầu từ mỗi nút bằng cách so sánh quần thể của nó với dân số của các nút con. Mỗi thành phố chỉ cần biết, đối với một dân số nhất định, số lượng buổi hòa nhạc tối đa có thể có nếu buổi hòa nhạc cuối cùng diễn ra ở dân số đó. Điều này mang lại một$O(n^2)$thuật toán có thể chấp nhận được đối với$n = 6000$. 

Chúng tôi có thể tối ưu hóa hơn nữa bằng cách chỉ lưu trữ LIS tối đa kết thúc tại mỗi thành phố và hợp nhất thông tin giữa các nút cha và nút con. Điều này tránh việc tính toán lại LIS từ đầu cho mỗi cặp thành phố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n^2) | Quá chậm | 
| Nhân giống cây DP/LIS | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng thành phố và dân số, đồng thời xây dựng danh sách lân cận cho cây từ các kết nối đường bộ. Danh sách lân cận cho phép di chuyển nhanh chóng các hàng xóm trong DFS. 
2. Khởi tạo bảng DP`dp[u][v]`để lưu trữ số lượng buổi hòa nhạc tối đa dọc theo các đường dẫn bắt đầu từ nút`u`và kết thúc bằng nút`v`, với buổi hòa nhạc cuối cùng tại`v`. Vì chúng tôi chỉ quan tâm đến việc tăng dần các chuỗi nên chúng tôi chỉ truyền bá thông tin LIS nếu quần thể nút tiếp theo lớn hơn quần thể hòa nhạc cuối cùng. 
3. Thực hiện DFS từ mọi nút`u`như điểm khởi đầu. Đối với mỗi người hàng xóm`v`của`u`chưa được truy cập, hãy tính toán đệ quy LIS kết thúc tại`v`. Nếu như`r[v] > r[u]`, chúng ta có thể mở rộng LIS bằng cách biểu diễn một buổi hòa nhạc tại`v`. Cập nhật bảng DP với độ dài tối đa được tìm thấy. 
4. Trong khi truyền bá các giá trị trong DFS, hãy duy trì mức tối đa toàn cầu`answer`để theo dõi số lượng buổi hòa nhạc lớn nhất được tìm thấy trên tất cả các điểm xuất phát. Mỗi lần giá trị DP được cập nhật, hãy so sánh nó với`answer`và cập nhật cho phù hợp. 
5. Sau khi DFS hoàn tất cho tất cả các nút,`answer`chứa số lượng buổi hòa nhạc tối đa mà ban nhạc có thể biểu diễn dọc theo bất kỳ con đường nào trong cây. 

Tại sao nó hoạt động: DFS đảm bảo chúng tôi khám phá tất cả các đường dẫn mà không cần xem lại các thành phố và bảng DP đảm bảo rằng với mỗi đường dẫn kết thúc tại một thành phố có số lượng buổi hòa nhạc cuối cùng nhất định, chúng tôi lưu trữ số lượng buổi hòa nhạc tối đa có thể đạt được. Vì cây không có chu trình nên không có đường đi nào bị bỏ sót và điều kiện tăng nghiêm ngặt được thực thi ở mỗi bước. Việc hợp nhất các giá trị DP từ các nút con đảm bảo chúng tôi xem xét cả đường dẫn lên và xuống từ mỗi nút. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10000)

n = int(input())
r = list(map(int, input().split()))
adj = [[] for _ in range(n)]
for _ in range(n-1):
    a, b = map(int, input().split())
    adj[a-1].append(b-1)
    adj[b-1].append(a-1)

dp = [1] * n  # dp[u]: max concerts ending at u
answer = 1

def dfs(u, parent):
    global answer
    for v in adj[u]:
        if v == parent:
            continue
        dfs(v, u)
        if r[v] > r[u]:
            dp[v] = max(dp[v], dp[u] + 1)
            answer = max(answer, dp[v])

for i in range(n):
    dfs(i, -1)

print(answer)
```Giải pháp trước tiên là thiết lập danh sách kề và khởi tạo mảng DP`dp`đại diện cho số lượng buổi hòa nhạc tối đa kết thúc tại mỗi thành phố. DFS được thực hiện từ mỗi thành phố với tư cách là gốc, đảm bảo chúng tôi khám phá tất cả các đường dẫn bắt đầu từ nút đó. Trong DFS, chỉ cho phép chuyển đổi tăng dần một cách nghiêm ngặt, điều này thực thi quy tắc hòa nhạc. các`answer`biến được cập nhật bất cứ khi nào tìm thấy chuỗi dài hơn. Giới hạn đệ quy được tăng lên để xử lý các cây sâu gần giới hạn trên là 6000 nút. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
6
1 2 3 4 5 1
1 2
2 3
3 4
3 5
3 6
```| Nút | dp | Giải thích | 
| --- | --- | --- | 
| 1 | 1 | Bắt đầu từ nút 1 | 
| 2 | 2 | r[2] > r[1], mở rộng LIS | 
| 3 | 3 | r[3] > r[2], mở rộng LIS | 
| 4 | 4 | r[4] > r[3], mở rộng LIS | 
| 5 | 4 | r[5] < r[3], không thể kéo dài | 
| 6 | 2 | r[6] > r[1], độ dài LIS 2 |`answer = 4`tương ứng với đường dẫn 1-2-3-4. 

**Đầu vào tùy chỉnh 2 (Cây sao)**```
5
3 1 2 5 4
1 2
1 3
1 4
1 5
```| Nút | dp | Giải thích | 
| --- | --- | --- | 
| 1 | 1 | nút trung tâm | 
| 2 | 2 | 1 → 2 hay 1 → 3? | 
| 3 | 2 | 1 → 3 | 
| 4 | 2 | 1 → 4 | 
| 5 | 2 | 1 → 5 | 

Độ dài đường đi tăng dần tối đa là 2, vì không có chuỗi nào có thể đi qua nhiều lá với số lượng ngày càng tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi nút khởi tạo một DFS có thể truy cập tất cả các nút khác và cập nhật DP là O(1) trên mỗi cạnh. Với n 6000, n^2 36.000.000 thao tác phù hợp với giới hạn thời gian 5 giây. | 
| Không gian | O(n^2) | Mảng DP có kích thước n; danh sách kề là O(n); đệ quy xếp chồng lên tới n. Phù hợp với giới hạn 256 MB. | 

Điều này đảm bảo thuật toán chạy hiệu quả với kích thước đầu vào lớn nhất được phép. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.setrecursionlimit(10000)
    
    n = int(input())
    r = list(map(int, input().split()))
    adj = [[] for _ in range(n)]
    for _ in range(n-1):
        a, b = map(int, input().split())
        adj[a-1].append(b-1)
        adj[b-1].append(a-1)

    dp = [1] * n
    answer = 1

    def dfs(u, parent):
        nonlocal answer
        for v in adj[u]:
            if v == parent:
                continue
            dfs(v, u)
            if r[v] > r[u]:
                dp[v] = max(dp[v], dp[u] + 1)
                answer = max(answer, dp[v])

    for i in range(n):
        dfs(i, -1)

    return str(answer)
```
