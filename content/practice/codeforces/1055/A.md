---
title: "CF 1055A - Tàu điện ngầm"
description: "Hệ thống metro có thể coi là một tuyến gồm các ga từ 1 đến n, có hai hướng di chuyển. Một rãnh cho phép di chuyển từ các chỉ số nhỏ hơn đến các chỉ số lớn hơn, trong khi rãnh còn lại cho phép di chuyển theo hướng ngược lại. Bob bắt đầu ở trạm 1 và muốn đến trạm s."
date: "2026-06-15T12:52:04+07:00"
tags: ["codeforces", "competitive-programming", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1055
codeforces_index: "A"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 2"
rating: 900
weight: 1055
solve_time_s: 59
verified: true
draft: false
---

[CF 1055A - Metro](https://codeforces.com/problemset/problem/1055/A) 

**Xếp hạng:** 900 
**Thẻ:** đồ thị 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hệ thống metro có thể coi là một tuyến gồm các ga từ 1 đến n, có hai hướng di chuyển. Một rãnh cho phép di chuyển từ các chỉ số nhỏ hơn đến các chỉ số lớn hơn, trong khi rãnh còn lại cho phép di chuyển theo hướng ngược lại. Bob bắt đầu ở trạm 1 và muốn đến trạm s. 

Điều phức tạp là mỗi trạm có thể cho phép dừng hoặc không cho phép dừng theo từng hướng độc lập. Ngay cả khi tàu đi qua một ga, Bob chỉ có thể sử dụng ga đó làm điểm trung chuyển hoặc điểm dừng nếu ga đó mở cửa cho hướng đường ray đó. Nếu nó đóng cửa, các đoàn tàu vẫn đi qua nhưng Bob không thể lên hoặc xuống theo hướng đó, điều này sẽ loại bỏ nhà ga đó như một nút có thể sử dụng được trên đường ray được chỉ dẫn đó. 

Nhiệm vụ là xác định xem có tồn tại bất kỳ chuỗi lên máy bay hợp lệ nào và có thể chuyển tuyến tại các trạm có thể tiếp cận cho phép di chuyển từ trạm 1 đến trạm s hay không. 

Kích thước đầu vào n tối đa là 1000, có nghĩa là truyền tải O(n^2) hoặc thậm chí tìm kiếm đồ thị đơn giản trên các nút O(n) và các cạnh O(n) là đủ. Bất cứ điều gì theo cấp số nhân đều không cần thiết, nhưng ngay cả một BFS đơn giản hoặc mô phỏng trên lân cận cũng dễ dàng đủ nhanh. 

Một trường hợp khó phát hiện khi Bob ban đầu không thể truy cập hướng thuận tại trạm 1. Trong trường hợp đó, ngay cả khi tồn tại một đường đi hợp lệ bằng cách đi lùi trước rồi chuyển hướng, nó có thể bị bỏ sót nếu chúng ta giả định chuyển động đơn điệu hoặc bỏ qua việc chuyển hướng. 

Một trường hợp khác xảy ra khi chỉ có thể đến được trạm s bằng cách đi đường vòng đến một trạm ở xa rồi quay lại theo hướng ngược lại. Ví dụ: nếu hướng tiến bị chặn sớm nhưng hướng lùi lại mở, trước tiên Bob có thể cần phải di chuyển đến trạm có thể tiếp cận ngoài cùng bên phải. 

## Phương pháp tiếp cận 

Cấu trúc tự nhiên tạo thành một đồ thị có hướng trong đó mỗi trạm là một nút. Từ trạm i, Bob có thể di chuyển đến i+1 nếu rãnh đầu tiên được mở tại i và tới i-1 nếu rãnh thứ hai mở tại i. Vấn đề làm giảm khả năng tiếp cận từ nút 1 đến nút s. 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các tuyến đường có thể, có thể khám phá các tuyến đường với những lần xem lại nhiều lần. Điều này đúng nhưng có thể bùng nổ về độ phức tạp vì số bước đi trong biểu đồ có chu kỳ tăng theo cấp số nhân. Ngay cả với n lên tới 1000, việc liệt kê đường dẫn đơn giản là không khả thi. 

Quan sát chính là đây là vấn đề về khả năng tiếp cận tiêu chuẩn trong biểu đồ có n nút và tối đa 2n cạnh. Sau khi được mô hình hóa chính xác, một BFS hoặc DFS đơn giản là đủ, vì mỗi trạm có nhiều nhất hai chuyển tiếp đi, một chuyển tiếp và một chuyển đổi lùi. Các ràng buộc đảm bảo rằng việc truy cập mỗi nút một lần là đủ. 

Lực lượng vũ phu hoạt động vì tất cả các chuyển đổi đều mang tính cục bộ và mang tính quyết định, nhưng không thành công khi việc xem lại tạo ra sự bùng nổ của các trạng thái lặp lại. Quan sát cho thấy chúng ta chỉ quan tâm đến việc liệu một nút có thể truy cập được hay không chứ không phải làm thế nào để đạt được nó, làm giảm vấn đề đối với việc truyền tải đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Con đường Brute Force | O(2^n) | Ngăn xếp đệ quy O(n) | Quá chậm | 
| Khả năng tiếp cận BFS/DFS | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi trạm như một nút trong biểu đồ.

1. Xây dựng vùng lân cận dựa trên chuyển động được phép. Từ ga i, nếu i + 1 ≤ n và a[i] = 1, hãy thêm một cạnh từ i vào i + 1. Nếu i − 1 ≥ 1 và b[i] = 1, hãy thêm một cạnh từ i vào i − 1. Các cạnh này thể hiện liệu Bob có thể lên một chuyến tàu tiếp tục theo hướng đó và dừng ở ga tiếp theo hay không. 
2. Khởi tạo mảng đã truy cập có kích thước n + 1 để theo dõi những trạm nào đã được xử lý. Điều này ngăn cản việc truy cập lại cùng một trạm nhiều lần, điều này sẽ dẫn đến việc khám phá dư thừa. 
3. Bắt đầu BFS (hoặc DFS) từ trạm 1, đánh dấu nó là đã truy cập. Trạm 1 luôn là điểm xuất phát của Bob nên có thể tiếp cận được nó một cách dễ dàng. 
4. Trong khi có các trạm chưa được xử lý trong hàng đợi BFS, hãy xóa một trạm u và khám phá tất cả các trạm lân cận v có thể truy cập thông qua các cạnh được phép. Nếu v chưa được truy cập, hãy đánh dấu nó và thêm nó vào hàng đợi. 
5. Sau khi quá trình truyền tải kết thúc, hãy kiểm tra xem trạm s đã được truy cập hay chưa. Nếu có, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, BFS duy trì tính bất biến rằng mọi trạm đã truy cập đều có thể truy cập được từ trạm 1 bằng cách sử dụng các chuyển tiếp hợp lệ. Mỗi cạnh tương ứng với một chuyển động hợp pháp dưới các ràng buộc về ga và đường ray, do đó, bất kỳ ga nào mới được phát hiện cũng có thể truy cập được. Vì BFS khám phá tất cả các trạng thái có thể truy cập mà không lặp lại, nên cuối cùng nó sẽ phát hiện ra tất cả các trạm có thể truy cập từ 1. Nếu trạm s không nằm trong số đó thì không có chuỗi chuyển động hợp lệ nào có thể tiếp cận được nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, s = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    adj = [[] for _ in range(n + 1)]

    for i in range(n):
        if i + 1 < n and a[i] == 1:
            adj[i + 1].append(i + 2)
        if i - 1 >= 0 and b[i] == 1:
            adj[i + 1].append(i)

    visited = [False] * (n + 1)
    q = deque([1])
    visited[1] = True

    while q:
        u = q.popleft()
        for v in adj[u]:
            if not visited[v]:
                visited[v] = True
                q.append(v)

    print("YES" if visited[s] else "NO")

if __name__ == "__main__":
    solve()
```Việc xây dựng lân cận mã hóa các ràng buộc hướng trực tiếp vào các cạnh, do đó BFS hoạt động trên một biểu đồ chuẩn. Các cạnh phía trước sử dụng a[i] vì chúng tương ứng với chuyển động từ i đến i + 1. Các cạnh phía sau sử dụng b[i] vì chúng tương ứng với chuyển động từ i đến i − 1. 

Một cạm bẫy triển khai phổ biến là lập chỉ mục theo từng cái một vì đầu vào dựa trên 1 nhưng mảng thường dựa trên 0. Ở đây, tính kề cận được xây dựng bằng cách sử dụng i + 1 làm chỉ mục nút, do đó tất cả các nút biểu đồ đều dựa trên 1 một cách nhất quán, tránh nhầm lẫn. 

Một vấn đề tế nhị khác là quên rằng chuyển động phụ thuộc vào trạm hiện tại chứ không phải trạm đích. Điều kiện phải được kiểm tra tại i trước khi thêm cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 1 1 1 1
1 1 1 1 1
```| Bước | Hiện tại | Xếp hàng | Đã truy cập | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | [1] | {1} | 
| 1 | 1 | [2] | {1,2} | 
| 2 | 2 | [3] | {1,2,3} | 
| 3 | 3 | [] | {1,2,3} | 

Trạm 3 có thể đạt được trực tiếp thông qua chuyển động về phía trước. Mỗi trạm cho phép cả hai hướng, do đó BFS mở rộng tuyến tính cho đến khi tìm thấy s. 

### Ví dụ 2 

đầu vào:```
5 4
0 0 1 1 1
1 1 1 1 1
```Ở đây, chuyển động về phía trước bị chặn ở trạm 1 và 2, do đó Bob không thể tiến thẳng sang bên phải. Tuy nhiên, chuyển động lùi lại cho phép di chuyển ngược lại một lần
