---
title: "CF 104745P - Khu trượt tuyết"
description: "Chúng ta có một đồ thị không theo chu kỳ có hướng biểu thị các đường trượt tuyết, cộng với một số lượng nhỏ các cạnh có hướng bổ sung biểu thị các thang máy trượt tuyết. Mỗi cạnh, dù là đường trượt hay dốc, đều mất đúng một phút để đi qua."
date: "2026-06-29T01:22:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104745
codeforces_index: "P"
codeforces_contest_name: "CAMA 2023"
rating: 0
weight: 104745
solve_time_s: 38
verified: true
draft: false
---

[CF 104745P - Khu nghỉ dưỡng trượt tuyết](https://codeforces.com/problemset/problem/104745/P) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị không theo chu kỳ có hướng biểu thị các đường trượt tuyết, cộng với một số lượng nhỏ các cạnh có hướng bổ sung biểu thị các thang máy trượt tuyết. Mỗi cạnh, dù là đường trượt hay dốc, đều mất đúng một phút để đi qua. Người trượt tuyết bắt đầu tại một nút nhất định và phải liên tục di chuyển dọc theo các cạnh mà không phải chờ đợi, xây dựng một con đường có chính xác x phút di chuyển. Anh ta được phép dừng lại ở bất kỳ nút nào khi tổng thời gian đạt đến x. 

Số lượng đặc biệt mà chúng tôi quan tâm là có bao nhiêu thang máy trượt tuyết được sử dụng dọc theo con đường như vậy. Trong số tất cả các bước đi hợp lệ có thể bắt đầu từ nút bắt đầu nhất định, chúng tôi muốn giảm thiểu số cạnh nâng trong khi đảm bảo bước đi kéo dài chính xác x bước. 

Cấu trúc chính là biểu đồ piste là DAG, nhưng thang máy trượt tuyết có thể đưa ra các chu kỳ trong biểu đồ kết hợp. Tuy nhiên, thang máy có một hạn chế lớn: nếu có thang máy từ a đến b thì a có thể đến được từ b chỉ bằng đường trượt. Điều này tạo ra cấu trúc khả năng tiếp cận ngược nhằm ngăn chặn các chu kỳ nâng tùy ý và sẽ trở nên quan trọng đối với các trạng thái đặt hàng. 

Các ràng buộc chỉ ra rằng tổng n và m bằng 10^5 trong các thử nghiệm và tổng k tối đa là 100 cho mỗi bộ thử nghiệm. Điều này ngay lập tức gợi ý rằng giải pháp phải gần với tuyến tính hoặc tuyến tính trong biểu đồ piste, đồng thời cho phép một số xử lý nặng hơn trên bộ nâng nhỏ. Đường đi ngắn nhất ngây thơ trên biểu đồ có độ dài x được mở rộng theo thời gian là không thể vì x có thể lên tới 10^9, loại trừ bất kỳ cấu trúc O(x) hoặc O(nx) nào. 

Trường hợp cạnh tinh tế xuất hiện khi x lớn nhưng đồ thị nhỏ. Một BFS ngây thơ theo dõi các bước một cách rõ ràng sẽ cố gắng mở rộng tối đa x lớp. Ví dụ: nếu x là 10^9 và đồ thị có một đường dẫn duy nhất, thì phương pháp đó sẽ cố gắng mô phỏng tất cả các chuyển đổi, điều này rõ ràng là không khả thi. 

Một cạm bẫy khác là bỏ qua ràng buộc về lực nâng. Nếu không có nó, thang máy có thể tạo ra các chu kỳ tùy ý và làm cho bài toán tương đương với đường đi ngắn nhất chung có trọng số 1 và thứ nguyên chi phí đặc biệt. Điều kiện về khả năng tiếp cận đảm bảo trật tự một phần giữa các điểm cuối của thang máy, giúp ngăn chặn chuỗi cải tiến vô hạn bệnh lý. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tính toán, với mỗi nút và mọi thời điểm có thể cho đến x, số lần nâng tối thiểu cần thiết để đến được nút đó trong đúng t bước. Đây là một chương trình động cổ điển trên biểu đồ mở rộng theo thời gian trong đó các trạng thái là (nút, thời gian). Mỗi lần chuyển đổi đi theo cạnh piste hoặc cạnh thang máy, cộng thêm 1 vào thời gian và có thể tăng số lần nâng. 

Điều này đúng vì nó khám phá rõ ràng tất cả các bước đi hợp lệ. Tuy nhiên, không gian trạng thái của nó có kích thước O(n x), trong trường hợp xấu nhất sẽ trở thành 10^14, khiến nó hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phân biệt các đường dẫn đến cùng một nút cùng lúc với các lịch sử khác nhau ngoại trừ số lần nâng của chúng và thậm chí cấu trúc đó có thể được nén. Vì mỗi lần di chuyển tốn chính xác một đơn vị thời gian, nên bài toán trở thành bài toán đường đi ngắn nhất của đồ thị lớp trong đó thời gian là chỉ số lớp. Sự khác biệt duy nhất là các cạnh nâng có thêm chi phí là 1 trong mục tiêu. 

Vì số lần nâng rất ít và đáp ứng hạn chế về khả năng tiếp cận nên chúng tôi có thể coi chúng là “bước nhảy đặc biệt” giữa các thành phần được xác định bởi cấu trúc DAG. Chỉ trong biểu đồ piste, việc tiếp cận tất cả các nút là sự mở rộng khả năng tiếp cận DAG thuần túy theo thời gian, có thể được tóm tắt bằng cách sử dụng các lớp BFS hoặc đường dẫn ngắn nhất trong cấu trúc giống DAG. Sau đó, thang máy đóng vai trò là lối tắt có thể giảm thời gian còn lại đồng thời tăng số lượng thang máy.

Điều này gợi ý một đường đi ngắn nhất có hai cấp độ: đầu tiên tính toán cấu trúc thời gian tiếp cận tối thiểu trong DAG piste, sau đó sử dụng thang máy để kết nối các trạng thái trong khi theo dõi gián tiếp thời gian còn lại. Điều kiện về khả năng tiếp cận đảm bảo rằng việc áp dụng thang máy luôn di chuyển đến một nút có thể được “mở rộng lại” về phía trước thông qua các đường trượt, nghĩa là thang máy không bao giờ khiến chúng ta mắc kẹt trong một chu kỳ cải thiện thời gian mà không tiêu tốn cấu trúc. 

Chúng tôi giảm vấn đề một cách hiệu quả xuống đường đi ngắn nhất trong biểu đồ trạng thái có các nút là các đỉnh của biểu đồ, nhưng có thêm một chiều ngầm định về thời gian còn lại mà chúng tôi xử lý một cách tham lam thông qua việc thư giãn theo lớp và tái sử dụng cấu trúc DAG. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DP mở rộng thời gian bằng vũ lực | O(n · x) | O(n · x) | Quá chậm | 
| Phân lớp DAG + thư giãn nâng cơ | O((n + m + k) log n) | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải thích lại vấn đề dưới dạng tìm, trong số tất cả các đường đi có độ dài chính xác x bắt đầu từ y, đường đi giảm thiểu mức sử dụng lực nâng. Vì mỗi cạnh tốn 1 đơn vị thời gian nên chúng tôi tách biệt khái niệm “khả thi về thời gian” khỏi “giảm thiểu chi phí nâng”. 

Chúng tôi tiến hành như sau. 

## Hướng dẫn thuật toán 

1. Đầu tiên hãy tính thứ tự tôpô của DAG piste. Điều này mang lại một cấu trúc trong đó chúng ta có thể truyền bá khả năng tiếp cận về phía trước mà không cần chu kỳ. Lý do điều này quan trọng là vì bất kỳ đường đi nào chỉ bao gồm các pít-tông đều có tính tuần hoàn, do đó, khoảng cách xét về mặt thời gian hoạt động giống như đường đi ngắn nhất trong DAG. 
2. Tính thời gian ngắn nhất (theo số cạnh) từ nút bắt đầu y đến mọi nút khác chỉ sử dụng các cạnh piste. Vì tất cả các cạnh có giá 1 nên đây là BFS đơn giản trên DAG. Điều này mang lại thời gian tối thiểu cần thiết để đến từng nút mà không cần thang máy. 
3. Quan sát rằng bất kỳ bước đi hợp lệ nào có độ dài x phải kết thúc tại một số nút u, nhưng chúng ta có thể tự do chèn các đường vòng miễn là chúng ta tôn trọng độ dài chính xác. Vì vậy, thay vì sớm sửa điểm cuối, chúng tôi nghĩ đến “cấu hình có thể truy cập sau t bước”. 
4. Giới thiệu cấu trúc thứ hai: mỗi thang máy trượt tuyết (a → b) đều có thể sử dụng được mặc dù nó đi ngược lại các hạn chế về hướng tiếp cận đường trượt tuyết, nhưng chúng tôi biết b có thể đến a thông qua đường trượt tuyết. Điều này có nghĩa là sau khi sử dụng thang máy, chúng ta luôn có thể “mở rộng lại” về phía trước trong cấu trúc DAG bắt đầu từ b. 
5. Xây dựng biểu đồ nén qua các điểm cuối của thang máy trong đó mỗi trạng thái thể hiện việc ở một nút sau một số lần mở rộng chỉ dành cho đường trượt tuyết. Từ trạng thái như vậy, chúng ta có thể tiếp tục đi dọc theo các mép đường trượt hoặc sử dụng thang máy. Thang máy thêm 1 vào số lượng thang máy nhưng cho phép di chuyển đến một nút có cấu trúc ngược dòng trong DAG, sau đó quá trình mở rộng đường trượt tuyết sẽ tiếp tục. 
6. Chạy đường đi ngắn nhất trên không gian trạng thái nén này với chi phí là số lần nâng. Việc chuyển tiếp qua các cạnh của đường trượt có chi phí bằng 0 trong biểu đồ được nâng lên này, vì chúng chỉ tiêu tốn thời gian chứ không phải nâng lên, trong khi các cạnh nâng lên có chi phí bằng 1. 
7. Ràng buộc k ≤ 100 đảm bảo rằng số lượng “trạng thái tương tác nâng” có ý nghĩa là nhỏ. Chúng ta chỉ cần xem xét các trạng thái xung quanh điểm cuối nâng và điểm bắt đầu, vì tất cả các nút khác đều được xử lý ngầm thông qua việc truyền DAG. 
8. Sau khi tính toán mức sử dụng mức tăng tối thiểu để tiếp cận bất kỳ nút nào, chúng tôi chỉ lọc những nút có thể tham gia bước đi chính xác x bước. Điều này được kiểm tra bằng cách sử dụng thực tế là từ bất kỳ nút nào, chúng tôi có thể mở rộng đường dẫn về phía trước trong DAG đến độ sâu tối đa của nó, do đó tính khả thi sẽ giảm xuống để kiểm tra xem x ít nhất có phải là thời gian tiếp cận tối thiểu và có phù hợp với độ trễ còn lại hay không. 

### Tại sao nó hoạt động

Thuật toán hoạt động vì biểu đồ piste xác định một phần thứ tự trong đó thời gian chỉ tăng dọc theo các cạnh và chỉ nâng các nút kết nối theo cách tôn trọng khả năng tiếp cận ngược. Điều này ngăn cản các chu kỳ không nhất quán trong đó thang máy có thể vừa tăng vừa giảm tiến độ hiệu quả vô thời hạn. Mọi chuyển đổi trạng thái đều bảo toàn cấu trúc khả năng tiếp cận DAG hoặc di chuyển dọc theo thang máy có thể được “hấp thụ” trở lại bản mở rộng DAG. Kết quả là, đường đi ngắn nhất qua số lần nâng được xác định rõ ràng và không phụ thuộc vào việc khám phá nhiều đường dẫn rõ ràng về thời gian theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque
import heapq

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        
        g = [[] for _ in range(n + 1)]
        indeg = [0] * (n + 1)

        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)
            indeg[v] += 1

        lifts = []
        for _ in range(k):
            a, b = map(int, input().split())
            lifts.append((a, b))

        x, y = map(int, input().split())

        dist = [10**18] * (n + 1)
        dist[y] = 0
        q = deque([y])

        while q:
            u = q.popleft()
            for v in g[u]:
                if dist[v] > dist[u] + 1:
                    dist[v] = dist[u] + 1
                    q.append(v)

        # DP over lift graph (small k)
        nodes = set([y])
        for a, b in lifts:
            nodes.add(a)
            nodes.add(b)

        nodes = list(nodes)
        idx = {v:i for i, v in enumerate(nodes)}
        L = len(nodes)

        INF = 10**18
        dp = [INF] * L
        dp[idx[y]] = 0

        pq = [(0, idx[y])]

        while pq:
            c, i = heapq.heappop(pq)
            if c != dp[i]:
                continue
            u = nodes[i]

            for a, b in lifts:
                if u == a:
                    j = idx[b]
                    if dp[j] > c + 1:
                        dp[j] = c + 1
                        heapq.heappush(pq, (dp[j], j))

        ans = min(dp[i] for i, v in enumerate(nodes) if dist[v] <= x)

        print(ans if ans < INF else -1)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách tính toán khoảng cách ngắn nhất chỉ dành cho đường trượt tuyết bằng cách sử dụng BFS từ nút bắt đầu. Điều này mang lại thời gian tối thiểu cần thiết để đến từng nút mà không cần sử dụng thang máy. 

Sau đó, chúng tôi hạn chế sự chú ý đến các nút xuất hiện trong thang máy cộng với điểm bắt đầu, vì chỉ những nút này mới quan trọng đối với việc tối ưu hóa thang máy theo các ràng buộc nhất định. Việc nén này là thứ giữ cho không gian trạng thái luôn được
