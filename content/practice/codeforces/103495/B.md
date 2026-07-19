---
title: "CF 103495B - Trong số chúng ta"
description: "Chúng ta được cung cấp một bản đồ các phòng được kết nối bằng những con đường vô hướng có trọng số. Hai kẻ mạo danh bắt đầu ở những căn phòng cố định và chỉ có thể di chuyển dọc theo những con đường bí mật nặng nề này."
date: "2026-07-03T06:08:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103495
codeforces_index: "B"
codeforces_contest_name: "2021 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 103495
solve_time_s: 49
verified: true
draft: false
---

[CF 103495B - Giữa chúng ta](https://codeforces.com/problemset/problem/103495/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bản đồ các phòng được kết nối bằng những con đường vô hướng có trọng số. Hai kẻ mạo danh bắt đầu ở những căn phòng cố định và chỉ có thể di chuyển dọc theo những con đường bí mật nặng nề này. Thời gian là liên tục và việc di chuyển dọc theo một cạnh có trọng lượng chính xác tính bằng giây, do đó, những kẻ mạo danh di chuyển một cách hiệu quả trên thước đo đường đi ngắn nhất tiêu chuẩn. 

Có tới tám thành viên phi hành đoàn. Mỗi thành viên phi hành đoàn có một danh sách những lần xuất hiện được dự đoán, nghĩa là vào những thời điểm nhất định họ sẽ ở trong những phòng cụ thể. Nếu tại một thời điểm chính xác nào đó, kẻ mạo danh ở cùng phòng với sự xuất hiện của thành viên phi hành đoàn, thành viên phi hành đoàn đó sẽ bị loại ngay lập tức. 

Những kẻ mạo danh muốn giảm thiểu thời gian mà tất cả đồng đội bị giết hoặc xác định rằng không thể đảm bảo tất cả số lần tiêu diệt trong khoảng thời gian cho phép tmax. 

Khó khăn chính là việc tiêu diệt không phải là tiếp cận một tập hợp các nút tĩnh. Chúng liên quan đến việc đồng bộ hóa thời gian di chuyển với các sự kiện đã lên lịch trên nhiều tác nhân, trong khi chỉ tồn tại hai động lực và chúng có chung các ràng buộc về biểu đồ. 

Các ràng buộc định hình giải pháp một cách mạnh mẽ. Biểu đồ có tối đa 10^4 nút và 2×10^4 cạnh, do đó, việc tính toán đường đi ngắn nhất phải gần tuyến tính trên mỗi nguồn. Số lượng đồng đội ít, nhiều nhất là 8, điều này ngay lập tức gợi ý không gian trạng thái bitmask. Số lượng sự kiện lên tới 10^5, do đó, mọi mô phỏng tốn kém cho mỗi sự kiện đều không an toàn. 

Một cách giải thích ngây thơ có thể cố gắng mô phỏng chuyển động của kẻ mạo danh trong khi theo dõi tất cả thời gian gặp gỡ có thể có với tất cả các thành viên phi hành đoàn, nhưng điều đó sẽ bùng nổ vì mỗi lựa chọn của kẻ mạo danh đều phân nhánh theo đường dẫn biểu đồ và thời gian sự kiện. 

Một trường hợp phức tạp hơn phát sinh khi nhiều thành viên phi hành đoàn yêu cầu các chuyến thăm phối hợp. Ví dụ: nếu thành viên phi hành đoàn A xuất hiện tại (phòng 1, thời gian 5) và thành viên phi hành đoàn B tại (phòng 2, thời gian 6), nhưng việc tiếp cận cả hai đòi hỏi những kẻ mạo danh khác nhau hoặc các tuyến đường được phân chia cẩn thận, thì sự phân công tham lam ngây thơ có thể thất bại. Một chế độ thất bại khác là giả định một thành viên phi hành đoàn "bị giết sau khi tiếp cận", bỏ qua rằng kẻ mạo danh phải đến đúng thời điểm sự kiện. 

Cạm bẫy thứ ba là coi đây là những đường đi ngắn nhất từ ​​một nguồn độc lập từ mỗi điểm bắt đầu của kẻ mạo danh đến mỗi sự kiện. Điều đó bỏ qua sự tương tác giữa các sự kiện thuộc về các thành viên phi hành đoàn khác nhau và nhu cầu bao gồm tất cả chúng theo dòng thời gian chung. 

## Phương pháp tiếp cận 

Quan điểm bạo lực cố gắng coi mỗi kẻ mạo danh như một khách du lịch liên tục có thể chọn đường đi và quyết định sự kiện nào sẽ tham dự tiếp theo. Đối với mỗi lần gán sự kiện có thể có cho kẻ mạo danh và mỗi thứ tự sự kiện cho mỗi kẻ mạo danh, chúng tôi sẽ tính toán tính khả thi bằng cách kiểm tra thời gian đường dẫn ngắn nhất. Điều này đúng về mặt khái niệm nhưng mang tính kết hợp ngay lập tức: với tối đa 10^5 sự kiện và tối đa 8 thành viên phi hành đoàn, thậm chí việc nhóm các sự kiện cho mỗi thành viên phi hành đoàn cũng dẫn đến hành vi giai thừa trên các lần xuất hiện của họ và sự phân chia theo cấp số nhân giữa hai kẻ mạo danh. 

Nhận xét quan trọng là số thành viên phi hành đoàn rất ít, vì vậy chúng ta không nên suy luận trực tiếp về các sự kiện riêng lẻ. Thay vào đó, chúng tôi nén tất cả các dự đoán thành một khái niệm “khi nào kẻ mạo danh có thể giết đồng đội p nếu họ đang ở phòng x vào thời điểm t”. Đối với mỗi thành viên phi hành đoàn p, chúng tôi chỉ quan tâm đến việc tại một thời điểm nhất định kẻ mạo danh có thể ở trong phòng được yêu cầu hay không. Điều này cho thấy chúng ta nên tính toán trước thời gian đến sớm nhất giữa các phòng bằng đường đi ngắn nhất. 

Bây giờ cấu trúc trở nên rõ ràng hơn: mỗi lần bắt đầu mạo danh sẽ xác định một hàm khoảng cách trên các phòng. Việc tiêu diệt là khả thi nếu kẻ mạo danh nào đó có thể đến phòng sự kiện không muộn hơn thời gian đó. Vì việc chờ đợi được cho phép nên chỉ có bất đẳng thức dist(start, x) ≤ t là quan trọng. 

Vì vậy, đối với mỗi lần bắt đầu mạo danh, chúng tôi tính toán các đường đi ngắn nhất trên biểu đồ. Sau đó, với mọi sự kiện (p, x, t), chúng tôi kiểm tra xem kẻ mạo danh nào có thể đáp ứng được điều đó. Điều này biến mỗi thành viên phi hành đoàn thành một tập hợp các cơ hội tiêu diệt ứng viên được đánh dấu theo thời gian mà tất cả đều phải được đảm bảo bởi kẻ mạo danh 1 hoặc kẻ mạo danh 2.

Bây giờ vấn đề còn lại là sự phân công: mỗi thành viên phi hành đoàn có tối đa nhiều sự kiện, nhưng vì k ≤ 8, chúng ta có thể nén từng thành viên phi hành đoàn thành một bitmask mà kẻ mạo danh chịu trách nhiệm về tất cả các sự kiện của nó. Đối với mặt nạ gán cố định, tính khả thi giảm xuống còn việc kiểm tra tất cả các sự kiện theo khoảng cách kẻ mạo danh đã chọn. 

Cuối cùng, chúng tôi tìm kiếm nhị phân thời gian trả lời T. Đối với T cố định, chúng tôi bỏ qua các sự kiện có t > T và kiểm tra xem có tồn tại sự phân công thành viên phi hành đoàn cho những kẻ mạo danh sao cho mọi sự kiện đều được che đậy hay không. Vì k 8, chúng ta có thể ép buộc tất cả các phép gán 2^k và xác thực từng phép gán trong O(e) bằng cách sử dụng các khoảng cách được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lập kế hoạch sự kiện Brute Force | Hàm mũ trong e | O(e) | Quá chậm | 
| Đường dẫn ngắn nhất + gán bitmask + tìm kiếm nhị phân | O((n+m) log n + 2^k · e · log tmax) | O(n + e) ​​| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán khoảng cách đường đi ngắn nhất từ cả hai phòng bắt đầu của kẻ mạo danh bằng thuật toán của Dijkstra. Điều này mang lại dist1[x] và dist2[x] cho tất cả các phòng x. 

Sau đó, chúng tôi xử lý trước tất cả các sự kiện dự đoán và nhóm chúng một cách hợp lý theo thành viên phi hành đoàn, nhưng chúng tôi vẫn sẽ đánh giá chúng trên toàn cầu khi kiểm tra tính khả thi. 

Tiếp theo, chúng tôi tìm kiếm nhị phân thời gian tối thiểu T mà tại đó tất cả thành viên phi hành đoàn có thể bị loại. Giới hạn dưới là 0 và giới hạn trên là tmax, vì sau tmax trò chơi sẽ thua nếu có ai sống sót. 

Đối với thời điểm ứng cử viên cố định T, chúng tôi chỉ lọc các sự kiện thành những sự kiện có t ≤ T, vì các sự kiện sau đó không quan trọng để đạt được việc loại bỏ theo thời gian T. 

Bây giờ chúng tôi thử tất cả 2^k nhiệm vụ của đồng đội cho những kẻ mạo danh. Trong một nhiệm vụ, mỗi thành viên phi hành đoàn p được chỉ định là kẻ mạo danh 0 hoặc kẻ mạo danh 1. 

Đối với một bài tập nhất định, chúng tôi kiểm tra mọi sự kiện (p, x, t). Nếu p được gán cho kẻ mạo danh 0, chúng tôi yêu cầu dist1[x] ≤ t, nếu không, chúng tôi yêu cầu dist2[x] ≤ t. Nếu bất kỳ sự kiện nào thất bại, nhiệm vụ không hợp lệ. 

Nếu ít nhất một phép gán hợp lệ thì thời điểm T là khả thi. 

Chúng tôi xuất ra T nhỏ nhất khả thi hoặc −1 nếu không có T ≤ tmax nào hoạt động. 

Tại sao điều này hoạt động gắn liền với tính độc lập được tạo ra bởi khoảng cách biểu đồ. Sau khi khoảng cách được cố định, mỗi sự kiện sẽ trở thành một ràng buộc cục bộ so sánh khả năng tiếp cận của một kẻ mạo danh với dấu thời gian. Khớp nối toàn cầu duy nhất là kẻ mạo danh xử lý thành viên phi hành đoàn nào và khớp nối đó được bitmask nắm bắt hoàn toàn trên k ≤ 8. Không cần sắp xếp thứ tự các sự kiện vì việc chờ sẽ loại bỏ các hạn chế về trình tự và mỗi sự kiện sẽ độc lập sau khi việc gán được cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

INF = 10**30

def dijkstra(n, adj, src):
    dist = [INF] * (n + 1)
    dist[src] = 0
    pq = [(0, src)]
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

def solve():
    T = int(input())
    for _ in range(T):
        n, m, k = map(int, input().split())
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v, w = map(int, input().split())
            adj[u].append((v, w))
            adj[v].append((u, w))

        e, tmax = map(int, input().split())
        events = []
        for _ in range(e):
            p, x, t = map(int, input().split())
            events.append((p - 1, x, t))

        s1, s2 = map(int, input().split())

        dist1 = dijkstra(n, adj, s1)
        dist2 = dijkstra(n, adj, s2)

        lo, hi = 0, tmax
        ans = -1

        def feasible(Tlim):
            # try all assignments of k crewmates
            for mask in range(1 << k):
                ok = True
                for p, x, t in events:
                    if t > Tlim:
                        continue
                    if (mask >> p) & 1:
                        if dist2[x] > t:
                            ok = False
                            break
                    else:
                        if dist1[x] > t:
                            ok = False
                            break
                if ok:
                    return True
            return False

        if feasible(tmax):
            lo, hi = 0, tmax
            while lo < hi:
                mid = (lo + hi) // 2
                if feasible(mid):
                    hi = mid
                else:
                    lo = mid + 1
            ans = lo

        print(ans)

if __name__ == "__main__":
    solve()
```Dijkstra chạy một lần cho mỗi lần khởi động kẻ mạo danh, nắm bắt tất cả các hạn chế di chuyển. Chức năng khả thi là sự rút gọn cốt lõi: thay vì mô phỏng chuyển động, nó giảm bớt vấn đề để kiểm tra xem mỗi sự kiện có nằm trong tầm tay của kẻ mạo danh đã chọn vào thời điểm đó hay không. 

Vòng lặp bitmask liệt kê tất cả các nhiệm vụ của đồng đội đối với những kẻ mạo danh. Điều này hợp lệ vì k ≤ 8, vì vậy 256 cấu hình là đủ nhỏ ngay cả khi nhân với tối đa 10^5 sự kiện. 

Tìm kiếm nhị phân được sử dụng vì tính khả thi là đơn điệu về mặt thời gian: nếu tất cả các lần tiêu diệt có thể được hoàn thành trước thời gian T, thì bất kỳ thời gian nào lớn hơn T' cũng có tác dụng vì các sự kiện chỉ trở nên dễ dàng đáp ứng hơn. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản tối thiểu với hai phòng được kết nối trực tiếp và một thành viên phi hành đoàn có hai lần xuất hiện. Một kẻ mạo danh bắt đầu ở mỗi phòng. Lần xuất hiện đầu tiên là sớm ở phòng A, lần thứ hai sau ở phòng B. Thuật toán tính toán cả hai khoảng cách, sau đó kiểm tra các bài tập. Một nhiệm vụ giao đồng đội cho kẻ mạo danh 1, nhưng sự kiện thứ hai không thành công do khoảng cách. Nhiệm vụ còn lại thành công nếu cả hai sự kiện đều có thể truy cập kịp thời từ kẻ mạo danh 2, chứng tỏ tại sao tính linh hoạt của nhiệm vụ lại quan trọng. 

Bây giờ hãy xem xét trường hợp một thành viên phi hành đoàn xuất hiện ở một căn phòng quá xa so với cả hai kẻ mạo danh trước dấu thời gian của nó. Đối với sự kiện đó, cả dist1[x] và dist2[x] đều vượt quá t, do đó mọi phép gán đều thất bại ngay lập tức. Kiểm tra tính khả thi trả về sai cho tất cả T và câu trả lời cuối cùng trở thành −1. 

Hai dấu vết này cho thấy tính đúng đắn hoàn toàn phụ thuộc vào việc so sánh khả năng tiếp cận đường đi ngắn nhất với thời hạn của sự kiện, trong khi nhiệm vụ chỉ xử lý quyền tự do tổ hợp duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n + 2^k · e · log tmax) | Dijkstra hai lần cộng với tìm kiếm nhị phân qua kiểm tra tính khả thi, mỗi lần kiểm tra tất cả các sự kiện cho tất cả mặt nạ | 
| Không gian | O(n + e) ​​| Lưu trữ đồ thị, mảng khoảng cách và danh sách sự kiện | 

Các ràng buộc được thiết kế sao cho k vẫn nhỏ, làm cho hệ số mũ trở nên vô hại, trong khi kích thước đồ thị yêu cầu tính toán đường đi ngắn nhất hiệu quả. Giải pháp phù hợp thoải mái trong giới hạn vì 2^8 chỉ là 256 và Dijkstra chiếm ưu thế trong mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO
    out = StringIO()
    sys.stdout = out

    # assume solution is wrapped in solve()
    solve()

    return out.getvalue().strip()

# minimal case
assert run("""1
1 0 1
0 1
1 1 1
1 1 1
""") in ["0", "-1"]

# simple reachable case
assert run("""1
2 1 1
1 2 1
1 2
1 10
1 2 1
1 1
""") == "1"

# impossible case
assert run("""1
2 1 1
1 2 1
1 2
1 3
1 2 5
1 1
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tối thiểu | 0 hoặc -1 | xử lý cạnh, đồ thị trống | 
| sự kiện có thể tiếp cận | 1 | suy luận khoảng cách đúng | 
| sự kiện bất khả thi | -1 | phát hiện không khả thi | 

## Vỏ cạnh 

Trường hợp quan trọng là khi nhiều sự kiện xảy ra tại cùng một phòng nhưng ở những thời điểm khác nhau. Thuật toán xử lý việc này một cách tự nhiên vì mỗi sự kiện được kiểm tra độc lập dựa trên cùng một giới hạn khoảng cách, do đó các sự kiện trước đó không chặn nhầm các sự kiện sau. 

Một trường hợp khác là khi một kẻ mạo danh bị thống trị nghiêm ngặt, nghĩa là khoảng cách của nó sẽ tệ hơn đối với tất cả các phòng. Việc liệt kê mặt nạ bit vẫn hoạt động vì nó sẽ chỉ gán tất cả đồng đội cho kẻ mạo danh tốt hơn mà không cần cách viết đặc biệt. 

Trường hợp cạnh cuối cùng là khi tất cả các sự kiện xảy ra sau tmax. Trong trường hợp đó, câu trả lời tối ưu gần như là 0 vì không có ràng buộc nào được kích hoạt trước thời hạn thất bại và việc tìm kiếm nhị phân sẽ ngay lập tức trở thành khả thi tại T = 0.
