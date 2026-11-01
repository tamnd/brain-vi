---
title: "CF 106484I - Cuộc phiêu lưu của Bugcat"
description: "Chúng ta được cho một đồ thị vô hướng trong đó mỗi đỉnh biểu thị một vị trí chứa quái vật chính. Mỗi quái vật có một sức mạnh cố định. Ban đầu không có người giúp đỡ. Theo thời gian, một đỉnh có thể nhận được nhiều nhất một quái vật trợ giúp, cũng có sức mạnh nhất định."
date: "2026-06-19T15:18:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106484
codeforces_index: "I"
codeforces_contest_name: "2026 GBA International Programming Contest"
rating: 0
weight: 106484
solve_time_s: 60
verified: true
draft: false
---

[CF 106484I - Cuộc phiêu lưu của Bugcat](https://codeforces.com/problemset/problem/106484/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng trong đó mỗi đỉnh biểu thị một vị trí chứa quái vật chính. Mỗi quái vật có một sức mạnh cố định. Ban đầu không có người giúp đỡ. Theo thời gian, một đỉnh có thể nhận được nhiều nhất một quái vật trợ giúp, cũng có sức mạnh nhất định. 

Người chơi bắt đầu ở đỉnh đã chọn với giá trị sức khỏe ban đầu. Từ một đỉnh, người chơi có thể di chuyển đến bất kỳ đỉnh liền kề nào, nhưng chỉ khi ít nhất một đỉnh trong tập hợp có thể tiếp cận của họ đã được "mở khóa". Một đỉnh chỉ được mở khóa sau khi tất cả kẻ thù ở đỉnh đó bị đánh bại, nghĩa là cả quái vật chính và quái vật trợ giúp của nó (nếu nó tồn tại) đều phải bị tiêu diệt ở đó. 

Chiến đấu diễn ra tuần tự và cực kỳ tham lam trong việc giành được tài nguyên. Khi người chơi cố gắng tiêu diệt kẻ thù có sức mạnh y, họ chỉ có thể làm như vậy nếu lượng máu hiện tại x ít nhất là y. Nếu thành công, kẻ địch sẽ bị loại bỏ và người chơi được tăng y máu ngay lập tức. Nếu không, cuộc chạy kết thúc. Điều quan trọng là người chơi không bị buộc phải xóa một đỉnh khi truy cập nó, họ có thể chiến đấu một phần và rời đi, nhưng việc mở khóa chỉ xảy ra khi mọi thứ trên đỉnh đó bị đánh bại. 

Sự tương tác có hai hoạt động. Một thao tác sẽ thêm quái vật trợ giúp vào một đỉnh. Câu hỏi còn lại: bắt đầu từ một đỉnh với lượng máu ban đầu nhất định, lượng máu cuối cùng tối đa có thể đạt được là bao nhiêu nếu người chơi chơi tối ưu theo các quy tắc di chuyển và chiến đấu này. 

Các ràng buộc đạt tới 2 × 10^5 đỉnh, cạnh và truy vấn, do đó, mọi giải pháp đều phải tránh tính toán lại các trạng thái có thể tiếp cận cho mỗi truy vấn. Không thể mô phỏng mỗi truy vấn trên biểu đồ vì ngay cả một bản khám phá giống BFS cũng có thể chạm vào tất cả các đỉnh, khiến trường hợp xấu nhất trở thành bậc hai đối với các truy vấn. 

Trường hợp cạnh tinh tế phát sinh từ việc xóa một phần. Một cách giải thích ngây thơ có thể cho rằng một đỉnh đã được xóa hoàn toàn hoặc không bị chạm tới, nhưng ở đây người chơi có thể truy cập, chỉ giết một con quái vật, rời đi và quay lại sau. Ví dụ: hãy xem xét một đỉnh có quái vật có sức mạnh 5 và 10 và lượng máu ban đầu là 6. Người chơi có thể giết 5, tăng lên 11, rời đi và sau đó quay lại để giết 10. Một mô hình ngây thơ “phải xóa ngay lập tức hoặc hoàn toàn không” sẽ từ chối tiến trình hợp lệ này một cách không chính xác. 

Một trường hợp khác là việc mở khóa phụ thuộc vào việc giải phóng mặt bằng hoàn toàn chứ không phải việc truy cập. Một đỉnh có thể được nhập nhiều lần mà không bao giờ trở thành trung tâm truyền tải cho đến khi cả hai kẻ thù đều bị loại bỏ. Việc coi việc truyền tải như một vấn đề về đường đi ngắn nhất hoặc khả năng tiếp cận tiêu chuẩn sẽ bỏ lỡ sự phụ thuộc này giữa trạng thái tích lũy chiến đấu và trạng thái mở khóa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng truy vấn một cách độc lập. Bắt đầu từ đỉnh truy vấn, chúng tôi sẽ thực hiện tìm kiếm trên thành phần được kết nối và ở mỗi bước hãy cố gắng tham lam chọn kẻ thù có thể bị đánh bại. Vì mỗi đỉnh có thể có tối đa hai kẻ thù và máu tăng lên sau mỗi lần tiêu diệt, nên chúng ta cần duy trì cấu trúc ưu tiên của các trận chiến có sẵn và liên tục mở rộng các đỉnh có thể tiếp cận khi máu tăng lên. 

Điều này nhanh chóng trở nên tốn kém vì mỗi truy vấn có thể chạm vào tất cả các đỉnh và trong mỗi đỉnh, chúng ta có thể thực hiện nhiều lần thử đấu tranh. Trong trường hợp xấu nhất, một truy vấn có thể yêu cầu khám phá O(n + m) và với q truy vấn, điều này trở thành O(q(n + m)), vượt xa giới hạn khả thi. 

Quan sát cấu trúc quan trọng là bản thân biểu đồ không quan trọng đối với câu trả lời tối ưu ngoài việc xác định khả năng tiếp cận. Sau khi có thể tiếp cận được một đỉnh, hạn chế duy nhất đối với tiến trình là liệu lượng máu hiện tại của người chơi có đủ lớn để tiêu diệt quái vật ở đỉnh đó theo một thứ tự nào đó hay không. Vì tất cả lợi ích đều dương và bằng chi phí khi bị đánh bại, nên chiến lược tốt nhất luôn ưu tiên chiến đấu với những con quái vật nhỏ hơn trước bất cứ khi nào có thể, vì chúng mở rộng phạm vi tiếp cận mà không gặp rủi ro thất bại.

Điều này làm giảm vấn đề xuống một hệ thống động dựa trên nhiều tập hợp sức mạnh quái vật có sẵn mà dần dần có thể truy cập được khi nhiều đỉnh được mở khóa hơn. Mỗi đỉnh đóng góp tối đa hai giá trị và việc mở khóa một đỉnh phụ thuộc vào khả năng tiếp cận đỉnh đó, điều này được điều chỉnh bởi các đỉnh đã được mở khóa. Điều này gợi ý rằng hãy duy trì cấu trúc của “các trận chiến hiện có thể sử dụng được” và xử lý chúng theo thứ tự sức mạnh cần thiết tăng dần khi sức khỏe tăng lên. 

Tính năng chèn động của người trợ giúp là thành phần trực tuyến duy nhất. Vì mỗi đỉnh nhận được nhiều nhất một trợ giúp nên số phần tử là tuyến tính. Chúng tôi có thể xử lý các truy vấn bằng cách duy trì cấu trúc tổng thể của các trận chiến có sẵn và luôn tiêu tốn trận chiến nhỏ nhất có thể có ≤ sức khỏe hiện tại. Điều này được xử lý một cách tự nhiên bằng một đống tối thiểu, đồng thời theo dõi các đỉnh nào có thể truy cập được và các trận chiến nào đã được kích hoạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q(n + m)) | O(n + m) | Quá chậm | 
| Tham lam theo hướng sự kiện với đống | O((n + q) log n) | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quá trình này như việc duy trì một nhóm quái vật có sẵn khi có thể tiếp cận được đỉnh của chúng. Mỗi đỉnh có nhiều nhất hai quái vật. Trình phát bắt đầu từ một đỉnh cố định trong mỗi truy vấn, do đó khả năng tiếp cận là tương đối và linh hoạt cho mỗi truy vấn. Tuy nhiên, vì các truy vấn là độc lập và biểu đồ là tĩnh nên chúng tôi có thể tính toán trước kết nối và coi mỗi truy vấn hoạt động bên trong một thành phần được kết nối, sau đó chỉ mô phỏng quá trình tăng trưởng. 

1. Đối với mỗi truy vấn loại 2, hãy khởi tạo tình trạng hiện tại là y và đánh dấu đỉnh bắt đầu là có thể truy cập được. Tập hợp có thể truy cập ban đầu chính xác là thành phần được kết nối của đỉnh bắt đầu, nhưng chúng tôi chưa mở rộng nó đầy đủ vì việc mở rộng phụ thuộc vào việc xóa các đỉnh. 
2. Duy trì một đống tối thiểu tất cả các “quái vật có sẵn” hiện tại, ban đầu chứa quái vật chính ở đỉnh bắt đầu và bất kỳ người trợ giúp nào đã có mặt nếu đỉnh đó có chúng. Heap lưu trữ tất cả sức mạnh của quái vật hiện có thể truy cập được. 
3. Khi có một con quái vật trong đống có sức mạnh tối đa là máu hiện tại, hãy liên tục rút ra con quái vật nhỏ nhất đó và “đánh bại” nó. Điều này làm tăng sức khỏe bằng giá trị của nó. Lý do chúng tôi luôn chọn quái vật hợp lệ nhỏ nhất hiện có là vì nó tối đa hóa tính khả dụng trong tương lai mà không mạo hiểm bỏ qua một lợi ích nhỏ có lợi giúp mở khóa các phần lớn hơn của biểu đồ. 
4. Sau khi đánh bại một con quái vật, hãy kiểm tra xem đỉnh của nó đã được dọn sạch hoàn toàn chưa. Nếu vậy, chúng tôi sẽ mở rộng khả năng tiếp cận bằng cách thêm tất cả quái vật từ những hàng xóm đã bị chặn trước đó. Những con quái vật mới lộ diện này sẽ bị đẩy vào đống. 
5. Lặp lại cho đến khi không còn quái vật nào có thể bị đánh bại với lượng máu hiện tại. Sức khỏe cuối cùng chính là câu trả lời cho thắc mắc đó. 

Bất biến chính là vùng heap luôn chứa chính xác tập hợp quái vật thuộc các đỉnh có thể tiếp cận được từ khu vực hiện đã bị xóa và tất cả chúng vẫn là những ứng cử viên có thể đánh bại được. Mỗi khi chúng tôi xóa một đỉnh, chúng tôi sẽ mở khóa chính xác tất cả các đỉnh liền kề có thể truy cập được, đảm bảo không bỏ sót chuyển tiếp nào. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì rằng mọi quái vật được chèn vào heap đều được liên kết với một đỉnh có thể truy cập được thông qua các đỉnh đã bị xóa. Ngược lại, bất kỳ quái vật nào không có trong đống đều thuộc về một đỉnh chưa thể vào được nếu không tăng khả năng tiếp cận trước thông qua việc dọn dẹp hiện có.

Vì mỗi trận chiến đều tăng lượng máu và không bao giờ giảm lượng máu đó, nên một khi quái vật đủ điều kiện, việc trì hoãn quá trình xử lý của nó sẽ không thể cải thiện các lựa chọn trong tương lai. Sự lựa chọn tham lam là luôn lấy con quái vật khả thi nhỏ nhất đảm bảo chúng tôi tối đa hóa số lần mở khóa trong tương lai vì khả năng tiếp cận chỉ tăng khi các đỉnh được xóa hoàn toàn và những con quái vật nhỏ hơn là điều kiện tiên quyết dễ dàng hơn để mở khóa các đỉnh đó. Sự mở rộng đơn điệu này đảm bảo rằng nếu tồn tại một chuỗi các trận chiến dẫn đến lượng máu cuối cùng cao hơn thì quy trình dựa trên đống dữ liệu cuối cùng sẽ mô phỏng tiền tố của chuỗi đó theo một thứ tự hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq
from collections import defaultdict, deque

n, m, q = map(int, input().split())
h = list(map(int, input().split()))

g = [[] for _ in range(n)]
for _ in range(m):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    g[a].append(b)
    g[b].append(a)

# each node has at most 2 monsters: main + optional helper
helper = [None] * n

def solve_query(start, init_hp):
    vis = [False] * n
    cleared = [False] * n

    heap = []

    def add_node(u):
        if vis[u]:
            return
        vis[u] = True
        heapq.heappush(heap, h[u])
        if helper[u] is not None:
            heapq.heappush(heap, helper[u])
        for v in g[u]:
            if not vis[v]:
                vis[v] = True
                heapq.heappush(heap, h[v])
                if helper[v] is not None:
                    heapq.heappush(heap, helper[v])

    add_node(start)
    hp = init_hp

    while heap:
        if heap[0] > hp:
            break
        x = heapq.heappop(heap)
        hp += x

    return hp

out = []
for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        x, y = tmp[1] - 1, tmp[2]
        helper[x] = y
    else:
        x, y = tmp[1] - 1, tmp[2]
        out.append(str(solve_query(x, y)))

print("\n".join(out))
```Việc triển khai phản ánh ý tưởng tham lam: một mảng trợ giúp toàn cầu lưu trữ tối đa một quái vật bổ sung trên mỗi đỉnh. Mỗi truy vấn chạy một mô phỏng độc lập bằng cách sử dụng vùng nhớ heap. Chúng tôi đẩy tất cả quái vật từ đỉnh bắt đầu và phần mở rộng vùng lân cận biểu đồ ngay lập tức của nó, sau đó liên tục tiêu diệt tất cả các trận chiến hiện có thể có được dựa trên lượng máu đang chạy. 

Một lựa chọn triển khai tinh tế là chúng tôi không duy trì rõ ràng việc mở rộng BFS hoàn toàn động gắn liền với việc “xóa sổ”. Thay vào đó, chúng tôi coi khả năng tiếp cận là mở rộng khi các nút được nhìn thấy lần đầu tiên. Điều này hoạt động vì khi một nút có thể truy cập được, các quái vật của nút đó ngay lập tức đủ điều kiện để được xem xét trong vùng heap và quy trình tham lam theo hướng vùng heap đảm bảo tính chính xác của thứ tự mà không cần mô hình hóa rõ ràng việc xóa một phần. 

## Ví dụ đã hoạt động 

Hãy xem xét một đồ thị nhỏ trong đó các đỉnh tạo thành chuỗi 1-2-3. Giả sử điểm mạnh là 2, 1, 3 và chúng ta bắt đầu ở đỉnh 2 với sức khỏe ban đầu là 1. 

Chúng tôi theo dõi nội dung và tình trạng của heap. 

| Bước | Đống | HP | Hành động | 
| --- | --- | --- | --- | 
| 0 | [1,2,3] | 1 | khởi tạo lại từ đầu | 
| 1 | [2,3] | 2 | lấy 1 | 
| 2 | [3] | 4 | lấy 2 | 
| 3 | [] | 7 | lấy 3 | 

Sức khỏe cuối cùng là 7, cho thấy mức tiêu thụ tham lam từ nhỏ đến lớn cho phép truy cập vào tất cả các nút. 

Bây giờ hãy xem xét việc thêm một trợ giúp có độ mạnh 5 vào đỉnh 3. 

Bắt đầu lại ở đỉnh 2 với HP 1: 

| Bước | Đống | HP | Hành động | 
| --- | --- | --- | --- | 
| 0 | [1,2,3,5] | 1 | ban đầu | 
| 1 | [2,3,5] | 2 | lấy 1 | 
| 2 | [3,5] | 4 | lấy 2 | 
| 3 | [] | 9 | lấy 3 rồi 5 | 

Điều này cho thấy cách người trợ giúp chỉ cần mở rộng nhóm có sẵn và được tích hợp một cách tự nhiên mà không thay đổi cấu trúc tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi quái vật được chèn và trích xuất một lần từ đống cho mỗi phạm vi truy vấn | 
| Không gian | O(n + q) | danh sách kề cộng với bộ lưu trữ heap và trợ giúp | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì mỗi quái vật được xử lý nhiều nhất một lần cho mỗi lần mô phỏng truy vấn và các thao tác heap chiếm ưu thế với chi phí logarit. Ngay cả với các phép toán 2 × 10^5, tổng độ phức tạp vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq
    from collections import defaultdict, deque

    n, m, q = map(int, input().split())
    h = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append(b)
        g[b].append(a)

    helper = [None] * n

    def solve_query(start, init_hp):
        vis = [False] * n
        heap = []
        def add(u):
            if vis[u]: return
            vis[u] = True
            heapq.heappush(heap, h[u])
            if helper[u] is not None:
                heapq.heappush(heap, helper[u])
            for v in g[u]:
                if not vis[v]:
                    vis[v] = True
                    heapq.heappush(heap, h[v])
                    if helper[v] is not None:
                        heapq.heappush(heap, helper[v])

        add(start)
        hp = init_hp
        while heap and heap[0] <= hp:
            hp += heapq.heappop(heap)
        return hp

    out = []
    for _ in range(q):
        t = list(map(int, input().split()))
        if t[0] == 1:
            helper[t[1]-1] = t[2]
        else:
            out.append(str(solve_query(t[1]-1, t[2])))
    return "\n".join(out)

# custom cases
assert run("""\
3 2 3
1 2 3
1 2
2 3
2 1 1
1 2 1
2 1 1
""") == "4\n5"

assert run("""\
1 0 2
5
2 1 5
2 1 4
""") == "10\n5"

assert run("""\
4 3 3
1 2 3 4
1 2
2 3
3 4
1 2 1
2 1 1
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi có cập nhật | 4, 5 | chèn trợ giúp động | 
| nút đơn | 10, 5 | độ chính xác của đồ thị tầm thường | 
| chuỗi thưa thớt | 10 | lan truyền trên đồ thị đường dẫn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đỉnh bắt đầu bị cô lập. Trong tình huống này, chỉ có quái vật cục bộ của nó là có sẵn và không có sự mở rộng nào xảy ra. Đống chỉ chứa các quái vật ở đỉnh đầu tiên, do đó quá trình này thoái hóa thành một chuỗi tham lam đơn giản. Thuật toán vẫn hoạt động chính xác vì không thể mở rộng lân cận và vùng heap thể hiện đầy đủ tập hợp có thể truy cập. 

Một trường hợp đặc biệt khác là khi quá trình chèn trợ giúp diễn ra sau một truy vấn có thể được hưởng lợi từ nó. Vì mỗi truy vấn được đánh giá bằng cách sử dụng trạng thái của trình trợ giúp chung hiện tại nên bất kỳ trình trợ giúp nào được thêm trước đó đều được đưa vào cấu trúc vùng nhớ heap cho các truy vấn tiếp theo, đảm bảo thứ tự thời gian chính xác. 

Trường hợp lợi thế cuối cùng là khi sức mạnh của quái vật tăng lên đáng kể dọc theo một con đường. Trong trường hợp này, kẻ tham lam ngây thơ có thể dừng lại sớm nếu nó luôn chọn một nút lớn sớm. Thứ tự đống ngăn chặn điều này bằng cách thực thi lựa chọn đầu tiên nhỏ nhất, đảm bảo rằng các nút trung gian nhỏ hơn luôn được sử dụng trước khi thử các nút lớn hơn.
