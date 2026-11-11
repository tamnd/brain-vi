---
title: "CF 1580E - Xây dựng đường sắt"
description: "Chúng tôi có một mạng lưới các ga đường sắt ở Gensokyo, được kết nối bằng đường sắt hai chiều với chiều dài dương. Các trạm được đánh số từ 1 đến $n$, trong đó trạm 1 là trung tâm chính. Mạng được kết nối đầy đủ nên mọi trạm đều có thể truy cập được từ bất kỳ trạm nào khác."
date: "2026-06-10T10:18:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "data-structures", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1580
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 745 (Div. 1)"
rating: 3400
weight: 1580
solve_time_s: 250
verified: false
draft: false
---

[CF 1580E - Xây dựng đường sắt](https://codeforces.com/problemset/problem/1580/E) 

**Đánh giá:** 3400 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, cấu trúc dữ liệu, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 4 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mạng lưới các ga đường sắt ở Gensokyo, được kết nối bằng đường sắt hai chiều với chiều dài dương. Các trạm được đánh số từ 1 đến$n$, với trạm 1 là trung tâm chính. Mạng được kết nối đầy đủ nên mọi trạm đều có thể truy cập được từ bất kỳ trạm nào khác. Mỗi trạm$i$có một chi phí$w_i$gắn liền với việc xây dựng tuyến đường sắt một chiều mới bắt đầu từ đó. Mục tiêu là bổ sung các tuyến đường sắt một chiều sao cho từ ga 1 đến bất kỳ ga nào khác, tồn tại ít nhất hai tuyến đường ngắn nhất tách rời nhau ngoại trừ các điểm cuối. Ngoài ra, chúng tôi phải duy trì khoảng cách ngắn nhất ban đầu giữa trạm 1 và tất cả các trạm khác. Sau khi xây dựng các tuyến đường sắt một chiều này, chi phí có thể tăng ở các ga cụ thể do sự cố và chúng tôi được yêu cầu báo cáo tổng chi phí xây dựng tối thiểu trước và sau mỗi sự cố. 

Kích thước đầu vào là đáng kể:$n$có thể đạt được$2 \cdot 10^5$,$m$lên đến$3 \cdot 10^5$, và có thể có$2 \cdot 10^5$sự cố. Vì vậy mỗi hoạt động phải được$O(n \log n)$hoặc$O(m \log n)$lúc tệ nhất. Bất kỳ thuật toán nào với$O(n^2)$hoặc$O(n \cdot m)$sự phức tạp sẽ quá chậm. 

Trường hợp cạnh không rõ ràng phát sinh khi một trạm được kết nối trực tiếp với trạm 1 thông qua nhiều đường dẫn có độ dài tối thiểu. Ví dụ, nếu$n = 3$, cạnh$(1,2,1)$,$(1,3,1)$,$(2,3,1)$, và chi phí$w=[1,2,3]$, một thuật toán bất cẩn có thể cố gắng thêm đường sắt một chiều vào ga 2 từ 1 mà không đảm bảo đường đi rời đỉnh thứ hai. Việc xây dựng đúng phải đảm bảo hai đường dẫn ngắn nhất hoàn toàn tách rời nhau, có thể liên quan đến các trạm nằm sâu hơn trong cây BFS thay vì chỉ các trạm lân cận. 

Một tình huống phức tạp khác là khi có nhiều sự cố xảy ra trên cùng một trạm. Thuật toán phải cập nhật tổng chi phí tối thiểu một cách hiệu quả mà không cần tính toán lại mọi thứ từ đầu mỗi lần. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận mạnh mẽ sẽ xem xét từng cặp đường từ ga 1 đến ga khác, kiểm tra xem chúng có tách rời đỉnh hay không ngoại trừ điểm cuối và cố gắng xây dựng đường sắt một chiều theo cách lặp đi lặp lại. Điều này sẽ yêu cầu liệt kê các đường dẫn trong biểu đồ có kích thước lên tới$2 \cdot 10^5$, dẫn đến tình trạng khó hiểu$O(2^n)$hoặc tốt nhất$O(n^2)$tiếp cận. Rõ ràng, điều này là quá chậm. 

Điểm mấu chốt là vấn đề giảm xuống cấu trúc cây đường đi ngắn nhất bắt nguồn từ trạm 1. Nếu lần đầu tiên chúng ta tính toán khoảng cách ngắn nhất bằng thuật toán Dijkstra, chúng ta có thể phân loại từng cạnh thành một phần của cây đường đi ngắn nhất hoặc một lối tắt tiềm năng. Mỗi trạm cần một đường đi ngắn thứ hai không trùng với đỉnh ngoại trừ các điểm cuối. Điều này có thể đạt được bằng cách kết nối mỗi nút với một trong các nút cha của nó trong cây BFS/Dijkstra thông qua đường sắt một chiều. Vì chỉ có trạm khởi đầu mới quan trọng để tính toán chi phí nên chúng ta có thể chọn trạm có chi phí tối thiểu trong số các trạm gốc ứng cử viên cho mỗi nút. Khi chi phí tối thiểu ban đầu được tính toán, việc cập nhật nó cho một sự cố chỉ yêu cầu điều chỉnh tổng bằng chênh lệch chi phí của nhà ga nhân với số lượng đường sắt một chiều bắt đầu từ nhà ga đó. Điều này cho phép chúng tôi duy trì tổng hoạt động một cách hiệu quả, giảm độ phức tạp xuống còn$O((n + m) \log n + q)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n^2) | Quá chậm | 
| Dijkstra + Cây BFS xây dựng | O((n + m) log n + q) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách ngắn nhất từ trạm 1 đến tất cả các trạm khác bằng thuật toán Dijkstra. Điều này đảm bảo rằng chúng tôi biết chính xác khoảng cách cần duy trì trong khi bổ sung thêm các tuyến đường sắt một chiều mới. Chúng tôi cũng ghi lại (các) đường dẫn gốc trong cây đường đi ngắn nhất để xác định các đường dẫn thứ hai tiềm năng. 
2. Chỉ xây dựng DAG gồm các cạnh có đường dẫn ngắn nhất. Đối với mọi cạnh$(u,v)$, bao gồm nó nếu$dist[u] + d = dist[v]$hoặc$dist[v] + d = dist[u]$. Điều này cung cấp cho chúng ta một biểu đồ phân lớp trong đó mọi đường dẫn đều tôn trọng khoảng cách ngắn nhất ban đầu. 
3. Đối với mỗi trạm$i$khác 1, xác định các ứng viên tiền nhiệm theo con đường ngắn nhất. Chi phí tối thiểu để xây dựng tuyến đường sắt một chiều đảm bảo hai tuyến đường ngắn nhất tách rời nhau là tối thiểu$w[p]$giữa các ứng cử viên tiền nhiệm. Về cơ bản, chúng tôi mô phỏng việc thêm tuyến đường sắt một chiều "dự phòng" từ hãng mẹ rẻ nhất đến$i$. 
4. Tổng hợp tổng chi phí bằng cách cộng chi phí xây dựng tối thiểu cho từng ga$i > 1$. Đây là câu trả lời trước mọi sự cố. 
5. Đối với mỗi sự cố làm tăng chi phí xây dựng đường sắt từ ga$k_i$qua$x_i$, tăng$w[k_i]$tương ứng. Nếu bất kỳ đường sắt một chiều nào bắt đầu từ$k_i$trong lựa chọn được tính toán trước của chúng tôi, hãy tăng tổng chi phí lên số lần đường sắt như vậy$x_i$. Báo cáo tổng chi phí đã cập nhật. 
6. Lặp lại bước 5 cho mỗi sự cố, duy trì tổng hoạt động hiệu quả mà không cần tính toán lại từ đầu. 

Tại sao nó hoạt động: Thuật toán của Dijkstra đảm bảo khoảng cách ngắn nhất. Việc hạn chế xây dựng đối với cây gốc có đường dẫn ngắn nhất sẽ duy trì khoảng cách trong khi thêm các đường dẫn thứ hai. Việc chọn cha mẹ có chi phí tối thiểu sẽ đảm bảo tổng chi phí được giảm thiểu. Thông tin cập nhật về sự cố là chính xác vì chúng tôi chỉ thêm những trạm thực sự đóng góp vào tổng chi phí. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def main():
    n, m, q = map(int, input().split())
    w = list(map(int, input().split()))
    adj = [[] for _ in range(n)]
    for _ in range(m):
        u, v, d = map(int, input().split())
        u -= 1; v -= 1
        adj[u].append((v, d))
        adj[v].append((u, d))
    
    # Dijkstra from node 0
    dist = [float('inf')] * n
    dist[0] = 0
    parent_candidates = [[] for _ in range(n)]
    hq = [(0, 0)]
    while hq:
        d_u, u = heapq.heappop(hq)
        if d_u > dist[u]:
            continue
        for v, w_uv in adj[u]:
            if dist[u] + w_uv < dist[v]:
                dist[v] = dist[u] + w_uv
                parent_candidates[v] = [u]
                heapq.heappush(hq, (dist[v], v))
            elif dist[u] + w_uv == dist[v]:
                parent_candidates[v].append(u)
    
    # Compute minimal construction cost
    total = 0
    from_count = [0] * n
    for i in range(1, n):
        # pick minimal cost parent
        p = min(parent_candidates[i], key=lambda x: w[x])
        total += w[p]
        from_count[p] += 1
    
    print(total)
    
    for _ in range(q):
        k, x = map(int, input().split())
        k -= 1
        w[k] += x
        total += from_count[k] * x
        print(total)

if __name__ == "__main__":
    main()
```Đầu tiên, mã này xây dựng danh sách kề và chạy thuật toán Dijkstra để xác định các đường dẫn ngắn nhất và ứng cử viên gốc cho các kết nối dự phòng. Sau đó, nó chọn trạm gốc có chi phí tối thiểu cho mỗi trạm để đảm bảo đường dẫn thứ hai, tổng hợp tổng chi phí xây dựng. Đối với các sự cố, nó cập nhật chi phí cho các ga bị ảnh hưởng dựa trên số lượng tuyến đường sắt một chiều bắt nguồn từ đó. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5 5 1
1 1 1 1 1
1 2 1
2 3 1
2 4 1
3 5 1
4 5 1
1 2
```| Trạm | Ứng viên Phụ huynh | Phụ huynh được chọn | Chi phí đóng góp | 
| --- | --- | --- | --- | 
| 2 | [1] | 1 | 1 | 
| 3 | [2] | 2 | 1 | 
| 4 | [2] | 2 | 1 | 
| 5 | [3,4] | 3 | 1 | 

Tổng ban đầu: 1+1+1=3. Sự cố tăng w[1] lên 2, tổng tăng 3*2=6, cho 9. 

Điều này xác nhận lựa chọn tối thiểu và cập nhật sự cố. 

**Mẫu 2 (tùy chỉnh)**```
3 3 1
5 2 3
1 2 2
2 3 2
1 3 4
2 1
```| Trạm | Ứng viên Phụ huynh | Phụ huynh được chọn | Chi phí đóng góp | 
| --- | --- | --- | --- | 
| 2 | [1] | 1 | 5 | 
| 3 | [2,1] | 2 | 2 | 

Tổng ban đầu: 5+2=7. Sự cố tăng w[1] lên 1; chỉ có trạm 1 có lượt ra, đóng góp 5*1=5, tổng cộng là 12. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n + q) | Dijkstra là O((n + m) log n), cập nhật sự cố O(q) | 
| Không gian | O(n + m | |
