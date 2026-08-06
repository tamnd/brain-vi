---
title: "CF 102500D - Công tắc dùng một lần"
description: "Chúng ta có một mạng vô hướng trong đó mỗi sợi cáp đều có độ dài xác định, nhưng thời gian truyền thực tế của một sợi cáp phụ thuộc vào hai tham số toàn cầu chưa xác định. Đối với cáp có chiều dài l, thời gian của nó là l / v + c, trong đó v và c áp dụng giống nhau cho mọi cáp."
date: "2026-08-05T18:01:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 88
verified: true
draft: false
---

[CF 102500D - Công tắc dùng một lần](https://codeforces.com/problemset/problem/102500/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng vô hướng trong đó mỗi sợi cáp đều có độ dài xác định, nhưng thời gian truyền thực tế của một sợi cáp phụ thuộc vào hai tham số toàn cầu chưa xác định. Đối với cáp có chiều dài`l`, đã đến lúc rồi`l / v + c`, ở đâu giống nhau`v`Và`c`áp dụng cho mọi cáp. Chúng ta cần tìm những switch không bao giờ có thể xuất hiện trên tuyến đường nhanh nhất từ ​​switch`1`chuyển đổi`n`, bất kể hai tham số này lấy giá trị hợp lệ nào. 

Các thông số chưa biết là khó khăn chính. Một đường dẫn không chỉ được so sánh bởi tổng chiều dài của nó. Đường dẫn sử dụng ít cáp hơn có thể trở nên tốt hơn khi chi phí đầu vào`c`lớn, trong khi đường dẫn có tổng chiều dài cáp ngắn hơn có thể giành chiến thắng khi sự lan truyền chiếm ưu thế. 

Những ràng buộc mang lại`n`đến năm 2000 và`m`lên tới 10000. Một thuật toán có thời gian bậc ba sẽ quá lớn vì`2000^3`hoạt động là khoảng tám tỷ. Giải pháp dự định phải gần với`O(n(n+m))`, tức là khoảng bốn mươi triệu phép tính đồ thị cho các giới hạn này. 

Một lỗi thường gặp là chạy Dijkstra một lần với công thức gốc. Điều đó không thể thực hiện được vì công thức chứa các giá trị không xác định. Một sai lầm khác là chỉ xét đường đi có độ dài ngắn nhất hoặc đường đi có ít cạnh nhất. Không thống trị người khác. Ví dụ, hãy xem xét:```
4 4
1 2 1
2 4 100
1 3 10
3 4 10
```Đối với một chi phí rất nhỏ, đường dẫn`1-3-4`thắng vì chiều dài của nó là`20`. Đối với chi phí rất lớn, đường dẫn`1-2-4`thắng vì nó có ít cạnh hơn. Một phương pháp chỉ xem xét một mục tiêu sẽ loại bỏ một cách không chính xác một đường đi tối ưu có thể có. 

Một trường hợp khác là cà vạt. Một công tắc hợp lệ nếu nó xuất hiện trên bất kỳ đường dẫn tối ưu nào cho bất kỳ lựa chọn tham số nào. Nếu hai tuyến đường khác nhau có cùng chi phí cho một tham số nào đó thì cả hai tuyến đường đều phải đóng góp các đỉnh của chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ thử mọi giá trị có thể có của các tham số chưa biết và tính toán các đường đi ngắn nhất. Điều này là không thể vì không gian tham số là liên tục và có thể có vô số thay đổi đường đi ngắn nhất khác nhau. 

Quan sát hữu ích đến từ việc viết lại chi phí đường đi. Nhân mỗi chi phí cạnh với`v`không thay đổi đường đi nào là tối ưu, bởi vì nó nhân mọi đường đi có thể với cùng một giá trị dương. Chi phí của một con đường trở thành:```
total_length + number_of_edges * x
```Ở đâu`x = v*c`, Và`x`có thể là bất kỳ số thực không âm nào. 

Bây giờ mọi đường dẫn được biểu thị bằng một dòng:```
y = edges * x + length
```Độ dốc là số cạnh và phần chặn là tổng chiều dài cáp. 

Với mọi số cạnh có thể có`k`, chúng ta chỉ quan tâm đến đường đi có độ dài ngắn nhất sử dụng chính xác`k`các cạnh. Bất kỳ con đường nào khác có cùng số cạnh đều có điểm chặn lớn hơn và không bao giờ có thể giành chiến thắng. 

Vì vậy, vấn đề trở thành: 

Tìm tất cả`k`dòng ở đâu```
y = k*x + best[k]
```là một phần của bao lồi dưới của`x >= 0`. 

Đó chính xác là số cạnh có thể xảy ra trên một tuyến đường tối ưu. 

Nhiệm vụ còn lại là khôi phục các đỉnh xuất hiện trên các đường dẫn có số cạnh đó. Chúng tôi tính toán các bảng lập trình động:`forward[k][v]`lưu trữ độ dài tối thiểu cần thiết để đạt được`v`từ công tắc`1`sử dụng chính xác`k`các cạnh.`backward[k][v]`lưu trữ độ dài tối thiểu cần thiết để tiếp cận công tắc`n`từ`v`sử dụng chính xác`k`các cạnh. 

Một đỉnh`v`có thể xuất hiện trên một con đường tối ưu với`k`các cạnh nếu đường dẫn có thể được phân chia tại`v`:```
prefix edges + suffix edges = k
```Và```
forward[prefix][v] + backward[suffix][v] = best[k]
```Đáp án cuối cùng là mọi đỉnh không bao giờ thỏa mãn điều kiện này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt quá các thông số | Không thể | Không thể | Quá chậm | 
| Liệt kê tất cả số cạnh bằng lập trình động và bao lồi | O(n(n+m)) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chiều dài cáp ngắn nhất có thể cho mỗi số cạnh. 

Số cạnh tối đa trong một đường dẫn hữu ích là`n-1`, bởi vì bất kỳ đường dẫn nào chứa chu trình đều có thể loại bỏ chu trình đó và trở nên tốt hơn. Chúng tôi chạy một quy trình lập trình động theo lớp. Mỗi lớp đại diện cho số cạnh chính xác được sử dụng cho đến nay. 

Đối với mọi`k`, chúng tôi có được`best[k]`, tổng chiều dài nhỏ nhất trong số tất cả`1`ĐẾN`n`đường dẫn sử dụng chính xác`k`các cạnh. 

1. Chuyển đổi các đường dẫn có thể thành dòng. 

Đối với mỗi số cạnh hợp lệ`k`, tạo dòng:```
y = k*x + best[k]
```Chỉ những dòng nhìn thấy từ bên dưới mới có thể ngắn nhất đối với một số số không âm`x`. Chúng tôi loại bỏ tất cả các dòng khác bằng cách tính toán thân dưới. 

1. Tìm tất cả số cạnh có thể tạo ra tuyến đường tối ưu. 

Mỗi dòng còn lại tương ứng với ít nhất một giá trị của`x`trong đó các đường dẫn sử dụng số cạnh đó là tối ưu. Chỉ những số cạnh này cần được kiểm tra các đỉnh. 

1. Tính toán lập trình động lớp tương tự từ đích. 

Bảng đảo ngược cho phép chúng ta kiểm tra xem một đỉnh có thể được đặt ở giữa đường đi tối ưu hay không. Nó tránh liệt kê mọi tuyến đường hoàn chỉnh. 

1. Đánh dấu mọi đỉnh xuất hiện trên đường đi tối ưu. 

Đối với mỗi số cạnh trên thân dưới, hãy kết hợp trạng thái tiến và lùi. Nếu hai phần cùng nhau có độ dài tối thiểu có thể cho số cạnh đó thì đỉnh đó có thể sử dụng được. 

1. Xuất ra mọi đỉnh không được đánh dấu. 

Đây chính xác là các công tắc không thể xuất hiện theo lộ trình tối ưu cho bất kỳ giá trị có thể có nào của các tham số chưa xác định. 

### Tại sao nó hoạt động 

Mọi thời gian truyền có thể được biểu diễn bằng cách chọn một giá trị của`x`trong họ dòng`k*x + best[k]`. Một đường bên ngoài thân dưới luôn kém hơn một đường khác, vì vậy không có đường đi nào sử dụng số cạnh đó có thể là tối ưu. Một đường ở thân dưới là tối ưu cho một số giá trị tham số, vì vậy tất cả các đường đi ngắn nhất được biểu thị bằng đường đó phải được xem xét. 

Các bảng lập trình động tiến và lùi chứa độ dài tối thiểu có thể có cho mỗi độ dài tiền tố và hậu tố. Nếu chúng kết hợp với nhau`best[k]`, tuyến đường kết quả là tuyến đường ngắn nhất trong số tất cả các tuyến đường sử dụng`k`các cạnh. Vì mọi tuyến đường tối ưu có thể có đều có số cạnh trên thân tàu, nên mọi đỉnh có thể sử dụng đều được đánh dấu và mọi đỉnh không được đánh dấu là không thể. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

INF = 10**30

def solve():
    n, m = map(int, input().split())

    adj = [[] for _ in range(n)]
    for _ in range(m):
        a, b, l = map(int, input().split())
        a -= 1
        b -= 1
        adj[a].append((b, l))
        adj[b].append((a, l))

    size = n * n

    def build(start):
        dp = array('q', [INF]) * size
        dp[start] = 0

        for k in range(n - 1):
            base = k * n
            nxt = (k + 1) * n
            for u in range(n):
                cur = dp[base + u]
                if cur == INF:
                    continue
                for v, w in adj[u]:
                    val = cur + w
                    if val < dp[nxt + v]:
                        dp[nxt + v] = val
        return dp

    forward = build(0)

    rev_adj = [[] for _ in range(n)]
    for u in range(n):
        for v, w in adj[u]:
            rev_adj[v].append((u, w))

    old = adj
    adj = rev_adj
    backward = build(n - 1)
    adj = old

    best = [forward[k * n + n - 1] for k in range(n)]

    hull = []
    for k in range(n):
        if best[k] == INF:
            continue
        while len(hull) >= 2:
            a, b = hull[-2], hull[-1]
            # intersection(a,b) >= intersection(b,k)
            if (best[b] - best[a]) * (k - b) >= (best[k] - best[b]) * (b - a):
                hull.pop()
            else:
                break
        hull.append(k)

    possible = [False] * n

    for k in hull:
        target = best[k]
        for v in range(n):
            f = forward[v]
            if f == INF:
                continue
            for i in range(k + 1):
                if i * n + v >= size:
                    break
                if forward[i * n + v] + backward[(k - i) * n + v] == target:
                    possible[v] = True
                    break
            if possible[v]:
                continue

    ans = [str(i + 1) for i, ok in enumerate(possible) if not ok]

    print(len(ans))
    if ans:
        print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Việc thực hiện lưu trữ các bảng lập trình động trong`array('q')`thay vì danh sách Python thông thường. Bảng chứa bốn triệu giá trị trong trường hợp tối đa và số nguyên Python sẽ tiêu tốn quá nhiều bộ nhớ. 

Quá trình chuyển đổi DP là sự thư giãn theo lớp trực tiếp. chỉ số`k*n+v`tượng trưng cho việc đạt tới đỉnh`v`sau chính xác`k`các cạnh. 

Phép tính bao lồi chỉ sử dụng số học số nguyên. Việc so sánh các điểm giao nhau tránh được các vấn đề về độ chính xác của dấu phẩy động vì chiều dài cáp có thể lớn bằng`10^9`. 

DP ngược được tính bằng cách đảo ngược tất cả các cạnh. Vì biểu đồ là vô hướng nên điều này tương đương với việc chạy cùng một quy trình từ đích. 

Vòng đánh dấu kiểm tra xem một đỉnh có thể phân chia một đường đi tối ưu hay không. So sánh đẳng thức chỉ sử dụng các giá trị số nguyên, do đó các tuyến có chi phí bằng nhau được xử lý chính xác.
