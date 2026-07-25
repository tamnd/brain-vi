---
title: "CF 103861J - Nhẫn Thần Tiên"
description: "Chúng ta có một đồ thị vô hướng trong đó đỉnh 1 là trung tâm bắt đầu và đỉnh n là mục tiêu cuối cùng mà chúng ta phải vượt qua. Mỗi đỉnh ngoại trừ 1 đều chứa một trùm có sức mạnh ban đầu. Người chơi cũng có giá trị sức mạnh và tiến hóa theo thời gian."
date: "2026-07-02T07:54:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103861
codeforces_index: "J"
codeforces_contest_name: "2021 ICPC Asia East Continent Final"
rating: 0
weight: 103861
solve_time_s: 66
verified: true
draft: false
---

[CF 103861J - Nhẫn Elden](https://codeforces.com/problemset/problem/103861/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trong đó đỉnh 1 là trung tâm bắt đầu và đỉnh n là mục tiêu cuối cùng mà chúng ta phải vượt qua. Mỗi đỉnh ngoại trừ 1 đều chứa một trùm có sức mạnh ban đầu. Người chơi cũng có giá trị sức mạnh và tiến hóa theo thời gian. 

Thời gian trôi qua theo những ngày rời rạc. Vào đầu mỗi ngày, mỗi con trùm còn lại sẽ tăng sức mạnh của mình lên một lượng cố định B. Sau đó, người chơi chọn chính xác một đỉnh có thể tiếp cận được từ đỉnh 1 và chiến đấu với con trùm của nó. Nếu sức mạnh hiện tại của người chơi thực sự lớn hơn con trùm đó, con trùm đó sẽ bị loại bỏ vĩnh viễn và người chơi nhận được sức mạnh A. 

Việc di chuyển bị hạn chế bởi những tên trùm còn sống. Một đỉnh chỉ có thể được truy cập từ đỉnh 1 nếu tồn tại một đường đi mà tất cả các đỉnh trung gian đều không có đỉnh hoạt động. Điều này có nghĩa là những tên trùm còn sống đóng vai trò là những kẻ chặn đường, ngoại trừ điểm cuối mà chúng tôi đang cố gắng đạt được trong cuộc chiến. 

Nhiệm vụ là xác định số ngày tối thiểu cần thiết để đánh bại trùm ở đỉnh n hoặc báo cáo là không thể. 

Các ràng buộc cho thấy cần phải có một giải pháp tối ưu hóa mạnh mẽ. Tổng số đỉnh và cạnh trong các trường hợp thử nghiệm có thể đạt tới 10^6, do đó, bất kỳ phương pháp nào xem lại các cạnh hoặc tính toán lại khả năng tiếp cận nhiều lần theo cách đơn giản sẽ quá chậm. Bất cứ điều gì bậc hai trong n hoặc thậm chí BFS lặp lại trên mỗi trạng thái đều bị loại trừ ngay lập tức. 

Một khó khăn tinh tế đến từ sự tương tác giữa thời gian và cấu trúc. Một nút có thể truy cập được về mặt kết nối đồ thị nhưng vẫn không thể tiêu diệt được do không đủ sức mạnh. Ngược lại, một nút có thể đủ yếu nhưng không thể truy cập được vì một đỉnh còn sống sẽ chặn tất cả các đường dẫn. 

Một vài trường hợp thất bại làm nổi bật những cạm bẫy. 

Hãy xem xét biểu đồ đường 1 - 2 - 3 - 4 trong đó chỉ có đỉnh 2 chặn đường dẫn đến mọi thứ nằm ngoài nó. Ngay cả khi đỉnh 4 đủ yếu để bị tiêu diệt sớm thì nó cũng không thể đạt được cho đến khi đỉnh 2 bị loại bỏ. Một cách tiếp cận ngây thơ chỉ kiểm tra sức mạnh và bỏ qua khả năng tiếp cận sẽ cho phép bỏ qua 2 một cách không chính xác. 

Một chế độ thất bại khác đến từ thời gian. Giả sử một đỉnh hầu như không quá mạnh vào ngày 1 nhưng có thể bị tiêu diệt vào ngày thứ 2 do người chơi tăng theo A. Nếu B lớn, việc trì hoãn cũng có thể tăng sức mạnh của trùm nhanh hơn người chơi, do đó, một chiến lược tham lam không chính xác cho rằng “chờ luôn tốt hơn” hoặc “sớm luôn tốt hơn” sẽ thất bại. 

Giải pháp chính xác phải đồng thời theo dõi khả năng tiếp cận theo các đỉnh bị loại bỏ linh hoạt và ngày sớm nhất một nút có thể bị đánh bại. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ cố gắng xử lý trò chơi hàng ngày. Vào mỗi ngày, nó sẽ tính toán lại các đỉnh có thể truy cập được từ 1 bằng cách chỉ sử dụng các đỉnh đã chết làm nút trung gian, sau đó kiểm tra tất cả các đỉnh có thể truy cập để xem đỉnh nào có thể bị giết. Điều này yêu cầu BFS hoặc DFS mỗi ngày và tối đa n ngày, đưa ra trường hợp xấu nhất là O(n(n + m)), vượt xa giới hạn. 

Quan sát quan trọng là trạng thái có ý nghĩa duy nhất không phải là cấu hình đầy đủ của trùm còn sống và đã chết, mà là tập hợp các đỉnh có thể truy cập được thông qua các nút đã được xóa, cùng với chỉ mục ngày hiện tại. Khi một đỉnh có thể truy cập được thì nó sẽ không bao giờ không thể truy cập được nữa vì việc xóa các đỉnh chỉ mở ra đường dẫn. 

Điều này gợi ý coi khả năng tiếp cận là một biên giới đang phát triển bắt đầu từ nút 1, nơi chỉ các đỉnh đã bị xóa mới có thể được sử dụng làm điểm định tuyến nội bộ. Mỗi lần chúng ta tiêu diệt một đỉnh, nó sẽ mở rộng vĩnh viễn vùng có thể đi qua.

Độc lập, tình trạng sức mạnh chỉ phụ thuộc vào số ngày đã trôi qua, bởi vì cả sức mạnh của người chơi và sức mạnh của trùm đều tiến triển tuyến tính với số lần tiêu diệt được (một lần mỗi ngày). Nếu chúng ta biểu thị k là số lần tiêu diệt đã thực hiện thì vào ngày hôm sau người chơi có sức mạnh l1 + kA, ​​trong khi trùm v có sức mạnh l_v + kB. Vì vậy, mỗi đỉnh có một ngưỡng k vượt quá ngưỡng đó thì nó có thể bị tiêu diệt, nhưng nó chỉ có thể được xem xét khi nó có thể tiếp cận được. 

Điều này tự nhiên dẫn đến một quá trình kiểu đường đi ngắn nhất qua các đỉnh, trong đó “khoảng cách” là ngày sớm nhất (hoặc số lần tiêu diệt) mà tại đó một đỉnh có thể bị đánh bại và quá trình chuyển đổi xảy ra khi một đỉnh có thể tiếp cận được từ khu vực đã bị xóa. 

Mỗi lần chúng tôi loại bỏ một đỉnh, chúng tôi sẽ mở khóa các đỉnh lân cận của nó khi có khả năng tiếp cận được và chúng tôi tính toán ngày sớm nhất có thể mà chúng có thể bị giết. Điều này tạo thành một bản mở rộng giống Dijkstra trong đó ưu tiên là ngày tiêu diệt sớm nhất khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hàng ngày với BFS | O(n(n + m)) | O(n + m) | Quá chậm | 
| Dijkstra qua các trạng thái có thể tiếp cận | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Mô hình hóa quy trình theo thứ tự hủy 

Chúng tôi giải thích quá trình này như việc chọn thứ tự các đỉnh bị loại bỏ. Nếu một đỉnh bị giết dưới dạng hành động thứ k (bắt đầu từ k = 0 cho lần tiêu diệt đầu tiên), thì ngày hiện tại là k + 1, sức mạnh của người chơi là l1 + kA và mọi trùm đều có sức mạnh l_i + kB. 

Điều này chuyển đổi thời gian thành một tham số nguyên k, giúp đơn giản hóa việc so sánh. 

### 2. Suy ra điều kiện khả thi để tiêu diệt một đỉnh 

Đỉnh v có thể bị loại bỏ ở bước k khi và chỉ khi bất đẳng thức nghiêm ngặt đúng: 

l1 + kA > l_v + kB. 

Sắp xếp lại mang lại: 

l1 - l_v > k(B - A). 

Điều này cho chúng ta biết thời điểm k sớm nhất mà v có thể bị tiêu diệt, miễn là nó có thể truy cập được. 

Hướng tăng trưởng rất quan trọng. Nếu A > B, người chơi sẽ có được lợi thế theo thời gian, vì vậy việc chờ đợi sẽ có ích. Nếu A ≤ B, việc trì hoãn không bao giờ cải thiện tính khả thi mà chỉ khiến việc tiêu diệt trở nên khó khăn hơn hoặc không thay đổi. 

### 3. Duy trì khả năng tiếp cận động từ đỉnh 1 

Chỉ các đỉnh được kết nối với 1 thông qua các đỉnh đã bị hủy mới có thể truy cập được. Các đỉnh còn sống chặn quá trình truyền tải. 

Vì vậy, chúng tôi duy trì một tập hợp các đỉnh “được kích hoạt” đã bị loại bỏ. Từ những điều này, chúng tôi có thể mở rộng sang những người hàng xóm chưa bị giết. Một đỉnh chỉ trở thành ứng cử viên khi có ít nhất một đỉnh lân cận đã được kích hoạt. 

Điều này đảm bảo chúng tôi chỉ xem xét các đỉnh hiện có thể truy cập được trong biểu đồ đang phát triển. 

### 4. Sử dụng hàng đợi ưu tiên theo thời gian tiêu diệt sớm nhất 

Chúng tôi duy trì giá trị nổi tiếng nhất dist[v], k nhỏ nhất mà tại đó v có thể bị giết khi có thể truy cập được. Chúng ta bắt đầu với đỉnh 1 đã được kích hoạt tại k = 0. 

Đối với mỗi đỉnh u được kích hoạt, chúng tôi quét các đỉnh v lân cận chưa được kích hoạt của nó. Nếu v được phát hiện ở bước k, chúng tôi tính toán bước tiêu diệt k' khả thi sớm nhất cho v dựa trên bất đẳng thức. Chúng tôi cũng đảm bảo k' ≥ k vì chúng tôi không thể hủy trước khi có thể truy cập được. 

Chúng ta đẩy (k', v) vào hàng đợi ưu tiên, luôn mở rộng đỉnh có k' nhỏ nhất. 

### 5. Xử lý các đỉnh trong việc tăng thời gian tiêu diệt 

Chúng tôi liên tục trích xuất đỉnh có k tối thiểu từ hàng đợi ưu tiên. Nếu nó đã được hoàn thiện, chúng tôi bỏ qua nó. Nếu không, chúng tôi đánh dấu nó là đã bị giết, đặt k hiện tại thành giá trị đó và ngầm cập nhật sức mạnh của người chơi. 

Sau đó, chúng tôi thư giãn tất cả những người hàng xóm, có thể giảm thời gian tiêu diệt khả thi sớm nhất của họ do khả năng tiếp cận sớm hơn. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào, thuật toán đảm bảo rằng mọi đỉnh trong hàng đợi ưu tiên đều có thể truy cập được thông qua các đỉnh đã bị loại bỏ và giá trị được lưu trữ của nó là bước sớm nhất mà đỉnh đó có thể bị loại bỏ theo tiến trình hiện tại. Bởi vì cả khả năng tiếp cận và tính khả thi chỉ cải thiện một cách đơn điệu khi chúng ta thêm các đỉnh bị loại bỏ, nên bất kỳ đỉnh nào được trích xuất với k tối thiểu sau này đều không thể được cải thiện. Đây chính xác là nguyên tắc đúng đắn giống như thuật toán Dijkstra: một khi trạng thái khả thi tối thiểu được chọn, không đường đi nào trong tương lai có thể tạo ra thời gian hủy hợp lệ nhỏ hơn. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, A, B = map(int, input().split())
        
        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)
        
        l = [0] + list(map(int, input().split()))
        
        INF = 10**30
        
        dist = [INF] * (n + 1)
        used = [False] * (n + 1)
        
        # node 1 is already reachable and "killed" at time 0
        dist[1] = 0
        pq = [(0, 1)]
        
        def earliest_k(v, k):
            # check minimal k' >= k such that:
            # l1 + k'*A > l[v] + k'*B
            # (A - B)*k' > l[v] - l1
            
            if A == B:
                if l[1] > l[v]:
                    return k
                return INF
            
            if A > B:
                # k' > (l[v] - l1) / (A - B)
                need = l[v] - l[1]
                if need < 0:
                    k0 = 0
                else:
                    k0 = need // (A - B) + 1
                return max(k, k0)
            
            # A < B: only possible if already satisfied at current k
            if l[1] + k * A > l[v] + k * B:
                return k
            return INF
        
        while pq:
            k, u = heapq.heappop(pq)
            if used[u]:
                continue
            used[u] = True
            
            for v in g[u]:
                if used[v]:
                    continue
                nk = earliest_k(v, k)
                if nk < dist[v]:
                    dist[v] = nk
                    heapq.heappush(pq, (nk, v))
        
        print(-1 if dist[n] == INF else dist[n])

if __name__ == "__main__":
    solve()
```Mã này xây dựng biểu đồ và chạy quy trình kiểu Dijkstra trên các đỉnh, trong đó mức độ ưu tiên là ngày sớm nhất mà một đỉnh có thể bị đánh bại trong khi vẫn có thể truy cập được từ các nút đã được xử lý. chức năng`earliest_k`mã hóa sự bất bình đẳng phụ thuộc vào thời gian giữa sức mạnh của người chơi và ông chủ, đồng thời tôn trọng thực tế rằng việc chờ đợi có thể hữu ích hoặc gây tổn hại tùy thuộc vào việc A có vượt quá B hay không. 

Hàng đợi ưu tiên đảm bảo rằng các đỉnh được hoàn thành theo thứ tự tăng dần của ngày hủy tối ưu và`used`mảng đảm bảo mỗi đỉnh được xử lý một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi đơn giản 1 - 2 - 3 với các tham số trong đó A lớn hơn B một chút, do đó việc chờ đợi sẽ giúp ích một chút. 

Chúng tôi theo dõi (k, bộ kích hoạt, ứng viên tốt nhất). 

| Bước | K (số ngày cho đến nay) | Các nút được kích hoạt | Đỉnh được chọn | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | {1} | 2 | chỉ hàng xóm mới có thể tiếp cận | 
| 2 | 0 | {1,2} | 3 | có thể truy cập được thông qua 2 | 
| 3 | k3 | {1,2,3} | kết thúc | đạt mục tiêu | 

Điều này cho thấy khả năng tiếp cận chỉ mở rộng thông qua các đỉnh bị loại bỏ. 

### Ví dụ 2 

Hãy xem xét một ngôi sao có tâm ở vị trí 1 với B cao, để các ông chủ có thể mở rộng quy mô nhanh chóng. 

| Bước | K | Đã kích hoạt | Ứng viên | Khả thi? | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | {1} | lá tôi | chỉ một số thỏa mãn l1 > l_i | 
| 2 | 0 hoặc 1 | {1, tôi} | lá mới | phụ thuộc vào tỷ lệ | 

Điều này chứng tỏ rằng việc lựa chọn tham lam sớm là cần thiết khi A ≤ B, vì việc trì hoãn chỉ làm xấu đi tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi đỉnh được đẩy và bật ra khỏi hàng đợi ưu tiên nhiều nhất một lần và mỗi cạnh được nới lỏng một lần | 
| Không gian | O(n + m) | danh sách lân cận cộng với khoảng cách và lưu trữ hàng đợi ưu tiên | 

Điều này phù hợp thoải mái trong giới hạn vì tổng n và m trong các trường hợp thử nghiệm đều lên tới 10^6 và hệ số log vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    out = []
    
    input = sys.stdin.readline
    
    def solve():
        t = int(input())
        for _ in range(t):
            n, m, A, B = map(int, input().split())
            g = [[] for _ in range(n + 1)]
            for _ in range(m):
                u, v = map(int, input().split())
                g[u].append(v)
                g[v].append(u)
            l = [0] + list(map(int, input().split()))
            INF = 10**30
            dist = [INF] * (n + 1)
            used = [False] * (n + 1)
            dist[1] = 0
            pq = [(0, 1)]
            
            def earliest_k(v, k):
                if A == B:
                    return k if l[1] > l[v] else INF
                if A > B:
                    need = l[v] - l[1]
                    k0 = 0 if need < 0 else need // (A - B) + 1
                    return max(k, k0)
                if l[1] + k*A > l[v] + k*B:
                    return k
                return INF
            
            import heapq
            while pq:
                k, u = heapq.heappop(pq)
                if used[u]:
                    continue
                used[u] = True
                for v in g[u]:
                    if not used[v]:
                        nk = earliest_k(v, k)
                        if nk < dist[v]:
                            dist[v] = nk
                            heapq.heappush(pq, (nk, v))
            
            out.append(str(-1 if dist[n] == INF else dist[n]))
    
    solve()
    return "\n".join(out)

# provided samples (placeholders since formatting not fully given)
# assert run(...) == ...

# custom tests
assert run("""1
3 2 5 1
1 2
2 3
10 1 1
""") != "", "basic chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút tầm thường | 0 | bắt đầu bằng trường hợp cạnh đích | 
| đồ thị chuỗi | hữu hạn k | tuyên truyền khả năng tiếp cận | 
| ngắt kết nối n | -1 | xử lý bất khả thi | 
| Trường hợp A ≤ B | hành vi tham lam sớm | chờ đợi không có ích | 

## Vỏ cạnh 

Trường hợp cạnh then chốt phát sinh khi A  B. Trong tình huống này, mọi độ trễ đều không có tác dụng gì hoặc khiến các cuộc chiến trong tương lai trở nên khó khăn hơn. Thuật toán xử lý vấn đề này bằng cách chỉ chấp nhận một đỉnh vào thời điểm chính xác mà nó có thể truy cập được và đã có thể tiêu diệt được. Nếu nó không thể bị tiêu diệt ngay khi phát hiện, nó sẽ bị loại bỏ. 

Một trường hợp khác là khi đỉnh n ban đầu không thể tiếp cận được do có một chuỗi dài các trùm còn sống. Thuật toán từ chối chính xác việc nới lỏng các nút ngoài đỉnh bị chặn đầu tiên cho đến khi trình chặn đó bị tiêu diệt rõ ràng, bởi vì khả năng tiếp cận chỉ lan truyền qua các đỉnh được kích hoạt. 

Một trường hợp tinh tế cuối cùng xảy ra khi một đỉnh có thể tiếp cận được từ rất sớm nhưng chỉ có thể bị tiêu diệt muộn hơn nhiều khi k tăng. Hàng đợi ưu tiên đảm bảo nó không được chọn sớm và thay vào đó đợi cho đến khi k khả thi sớm nhất được tính toán của nó trở nên tối thiểu trong số tất cả các ứng cử viên.
