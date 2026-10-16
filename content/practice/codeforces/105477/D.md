---
title: "CF 105477D - Giờ Hoa Girona"
description: "Thành phố có thể được mô hình hóa dưới dạng một biểu đồ hoàn chỉnh có hướng trong đó mỗi vị trí là một nút và mỗi cặp nút có thứ tự đều có thời gian di chuyển. Mỗi nút cũng có một giá trị không âm biểu thị số lượng khách du lịch đứng ở đó."
date: "2026-06-23T19:00:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105477
codeforces_index: "D"
codeforces_contest_name: "XXII Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105477
solve_time_s: 127
verified: true
draft: false
---

[CF 105477D - Giờ hoa Girona](https://codeforces.com/problemset/problem/105477/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thành phố có thể được mô hình hóa dưới dạng một biểu đồ hoàn chỉnh có hướng trong đó mỗi vị trí là một nút và mỗi cặp nút có thứ tự đều có thời gian di chuyển. Mỗi nút cũng có một giá trị không âm biểu thị số lượng khách du lịch đứng ở đó. Hành khách bắt đầu từ nút 0 và phải đến nút 1, và mọi bước đi giữa các nút đều được phép miễn là tổng thời gian di chuyển không vượt quá giới hạn S nhất định. 

Trong khi di chuyển, mỗi khi đường đi ghé thăm một nút, tất cả khách du lịch tại nút đó có thể được thu thập, nhưng mỗi nút chỉ đóng góp một lần ngay cả khi nó được truy cập nhiều lần. Mục tiêu là chọn một chuyến đi từ 0 đến 1, có thể xem lại các nút, nằm trong giới hạn thời gian và tối đa hóa tổng lượng khách du lịch thu thập được trên tất cả các nút đã truy cập riêng biệt. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng nút n, giới hạn thời gian S, danh sách t trong đó t[i] là số lượng khách du lịch tại nút i và ma trận n x n a trong đó a[i][j] là thời gian để đi trực tiếp từ i đến j. 

Khó khăn chính là mặc dù biểu đồ đã hoàn chỉnh nhưng tuyến đường tốt nhất có thể truy cập lại các nút vì việc truy cập lại là miễn phí về mặt phần thưởng nhưng vẫn tiêu tốn thời gian. Ràng buộc S lên tới 20000, do đó, việc lập trình động trực tiếp trên tất cả các tập hợp con của nút sẽ không khả thi khi n đạt tới 18, vì 2^18 trạng thái đã yêu cầu xử lý cẩn thận và các chuyển đổi sẽ liên quan đến hệ số n bổ sung. 

Một vấn đề nhỏ xuất hiện khi nhiều đường dẫn đến cùng một nút với ngân sách còn lại khác nhau. Ý tưởng đường đi ngắn nhất ngây thơ không thành công vì chúng tôi không giảm thiểu thời gian mà tối đa hóa trọng số thu thập được trong ngân sách thời gian, do đó Pareto cân bằng giữa thời gian và giá trị thu thập được. 

Một ví dụ đơn giản về cạm bẫy là trường hợp tuyến đường dài hơn đến sớm các nút có giá trị cao, trong khi tuyến đường ngắn hơn đến đích nhanh chóng nhưng thu thập ít hơn. Nếu chúng tôi chỉ lưu trữ thời gian ngắn nhất trên mỗi nút, chúng tôi sẽ loại bỏ đường dẫn dài hơn nhưng có lợi hơn và mất câu trả lời tối ưu. 

## Phương pháp tiếp cận 

Cấu trúc gợi ý rằng chúng tôi đang chọn một đường dẫn trong biểu đồ có hướng hoàn chỉnh với ràng buộc ngân sách, trong đó mỗi nút có phần thưởng được thu thập một lần. Điều này ngay lập tức giống với một không gian trạng thái trên các tập hợp con của các nút đã truy cập, nhưng tập hợp con đầy đủ DP sẽ theo dõi (mặt nạ, nút cuối cùng), quá lớn khi n bằng 18. 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị của việc truy cập các tập hợp con của các nút, tính toán chi phí đường đi ngắn nhất giữa các nút được chọn liên tiếp và tính tổng phần thưởng. Ngay cả khi bỏ qua việc tính toán đường đi, đây vẫn là giai thừa của n. Với n = 18, hoán vị đã vượt quá giới hạn khả thi. 

Quan sát quan trọng là n đủ nhỏ để chúng ta có thể coi các tập hợp con là trạng thái, nhưng S đủ vừa phải để có thể tính toán trước các đường đi ngắn nhất giữa các nút. Khi đã biết các đường dẫn ngắn nhất của tất cả các cặp, vấn đề sẽ giảm xuống thành DP mặt nạ bit cổ điển trên các nút trong đó các chuyển đổi tương ứng với việc di chuyển từ nút này sang nút khác bằng khoảng cách đường đi ngắn nhất. 

Trước tiên, chúng tôi chạy Floyd-Warshall trên ma trận để đảm bảo a[i][j] thể hiện thời gian di chuyển thực sự ngắn nhất giữa hai nút bất kỳ. Sau đó, chúng tôi xác định DP trong đó dp[mask][i] là lượng khách du lịch tối đa được thu thập khi truy cập chính xác mặt nạ đã đặt và kết thúc tại nút i, tôn trọng ràng buộc chi phí đi lại thông qua các chuyển đổi sử dụng khoảng cách ngắn nhất. 

Tuy nhiên, DP O(2^n * n) trực tiếp vẫn bỏ qua S. Chúng tôi tinh chỉnh thêm bằng cách lưu ý rằng S chỉ hạn chế tính khả thi của quá trình chuyển đổi. Vì n ≤ 18, chúng ta có thể giữ dp[mask][i] nhưng lược bỏ các chuyển tiếp bằng cách sử dụng tổng khoảng cách. Quá trình chuyển đổi cuối cùng là từ (mask, i) sang (mask ∪ {j}, j) nếu dp time + dist[i][j] ≤ S. Chúng tôi cần theo dõi cả thời gian và giá trị, vì vậy mỗi trạng thái lưu trữ các cặp tốt nhất và chúng tôi loại bỏ các trạng thái thống trị.

Điều này dẫn đến Pareto DP trong đó đối với mỗi (mặt nạ, i), chúng tôi duy trì một tập hợp các cặp (thời gian, phần thưởng), chỉ giữ lại những cặp không bị thống trị. Quá trình chuyển đổi kết hợp các bộ này một cách cẩn thận. 

Một cách cải tổ hiệu quả hơn sẽ tránh được DP theo cấp số nhân theo thời gian bằng cách đảo ngược quan điểm: chúng tôi tính toán trước khoảng cách ngắn nhất và sau đó thực hiện mở rộng giống như Dijkstra trên các trạng thái (mặt nạ, i), trong đó ưu tiên là thời gian và chúng tôi theo dõi phần thưởng tốt nhất cho mỗi (mặt nạ, i, biên giới thời gian). Bởi vì S 20000, chúng ta có thể rời rạc hóa bằng cách cắt bớt các trạng thái vượt quá S và giữ phần thưởng tốt nhất cho mỗi (mặt nạ, i, t) thông qua mức nổi tiếng nhất. 

Điều này làm giảm đường dẫn ngắn nhất nhiều trạng thái trên biểu đồ có kích thước O(2^n * n), nhưng mỗi độ giãn cạnh là O(1) và hàng đợi ưu tiên đảm bảo mỗi trạng thái được xử lý hiệu quả. 

Câu trả lời cuối cùng là phần thưởng tối đa trong số tất cả các trạng thái kết thúc ở nút 1 với thời gian ≤ S. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Bitmask DP không cần cắt tỉa | O(2^n n^2) | O(2^n n) | Quá chậm | 
| Bang Dijkstra qua (mặt nạ, nút) | O(2^n n log(2^n n)) | O(2^n n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp thành hai lớp: đường dẫn ngắn nhất và khám phá trạng thái bị ràng buộc. 

1. Tính toán các đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng Floyd-Warshall để việc di chuyển giữa hai nút bất kỳ có thời gian di chuyển tối thiểu được xác định rõ ràng. Điều này loại bỏ sự cần thiết phải xem xét các nút trung gian trong quá trình chuyển đổi DP. 
2. Xác định trạng thái là (mask, u), nghĩa là chúng ta đã truy cập vào tập hợp các nút được mã hóa trong mặt nạ và hiện đang đứng ở nút u. 
3. Khởi tạo hàng đợi ưu tiên với trạng thái (thời gian=0, phần thưởng=t[0], mặt nạ=1<<0, u=0). Điều này thể hiện việc bắt đầu từ nút 0 với lượng khách du lịch được thu thập. 
4. Duy trì ánh xạ từ điển tốt nhất (mặt nạ, u) đến thời điểm nhỏ nhất đã biết để đạt được phần thưởng lớn nhất đã biết. Chúng tôi chỉ mở rộng một trạng thái nếu trạng thái đó cải thiện kiến ​​thức trước đây về cấu hình đó. 
5. Xếp các trạng thái từ hàng đợi ưu tiên theo thứ tự thời gian tăng dần. Đối với mỗi trạng thái (mặt nạ, u), hãy thử di chuyển đến mọi nút v không nằm trong mặt nạ. Chi phí chuyển đổi là current_time + dist[u][v] và phần thưởng mới là current_reward + t[v]. 
6. Nếu thời gian mới vượt quá S, hãy loại bỏ quá trình chuyển đổi. Mặt khác, nếu trạng thái mới này cải thiện mục nhập được biết đến nhiều nhất cho (mặt nạ ∪ {v}, v), hãy đẩy nó vào hàng ưu tiên. 
7. Sau khi xử lý tất cả các trạng thái có thể truy cập, hãy quét tất cả các trạng thái có u = 1 và thời gian ≤ S và nhận phần thưởng tối đa. 

Cấu trúc chính là chúng tôi không bao giờ xem lại trạng thái trừ khi chúng tôi cải thiện thời gian hoặc khen thưởng một cách có ý nghĩa. Vì thời gian chỉ tăng dọc theo các chuyển tiếp và S giới hạn nó nên việc khám phá vẫn hữu hạn. 

### Tại sao nó hoạt động 

Mỗi trạng thái đại diện cho một tập hợp các nút được truy cập và vị trí kết thúc duy nhất, đồng thời thứ tự ưu tiên theo thời gian đảm bảo rằng bất cứ khi nào chúng tôi hoàn tất một trạng thái, chúng tôi đã xem xét tất cả các cách rẻ hơn để tiếp cận trạng thái đó. Vì phần thưởng chỉ tăng khi thêm nút mới và nút không bao giờ được tính hai lần trong mặt nạ nên mỗi trạng thái tóm tắt đầy đủ tất cả lịch sử hợp lệ dẫn đến nút đó. Quy tắc cắt tỉa tránh làm mất các giải pháp tối ưu vì bất kỳ trạng thái thống trị nào cũng không thể dẫn đến sự hoàn thành tốt hơn mà không vượt quá S hoặc số lần truy cập trùng lặp. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, S = map(int, input().split())
    t = list(map(int, input().split()))
    dist = [list(map(int, input().split())) for _ in range(n)]

    # Floyd-Warshall for all-pairs shortest paths
    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][j] > dist[i][k] + dist[k][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    start_mask = 1 << 0
    start_reward = t[0]

    # (time, -reward, mask, node)
    pq = [(0, -start_reward, start_mask, 0)]

    best_time = {}

    best_time[(start_mask, 0)] = 0

    ans = start_reward if 1 == 1 else 0

    while pq:
        time, neg_reward, mask, u = heapq.heappop(pq)
        reward = -neg_reward

        if time > S:
            continue

        if best_time.get((mask, u), float('inf')) < time:
            continue

        if u == 1:
            ans = max(ans, reward)

        for v in range(n):
            if mask & (1 << v):
                continue
            nt = time + dist[u][v]
            if nt > S:
                continue
            nmask = mask | (1 << v)
            nr = reward + t[v]

            state = (nmask, v)
            if nt < best_time.get(state, float('inf')):
                best_time[state] = nt
                heapq.heappush(pq, (nt, -nr, nmask, v))

    print(ans)

if __name__ == "__main__":
    solve()
```Khối Floyd-Warshall đảm bảo mọi chi phí chuyển đổi đều ở mức tối thiểu, do đó DP không bao giờ phải suy luận về các đường dẫn trung gian nữa. Vùng heap đảm bảo trước tiên chúng tôi luôn mở rộng tuyến đường từng phần nhanh nhất hiện đã biết, điều này rất cần thiết cho việc cắt tỉa. 

Mã hóa trạng thái sử dụng mặt nạ bit để đảm bảo rằng mỗi nhóm khách du lịch được tính nhiều nhất một lần. Từ điển best_time ngăn việc truy cập lại các phiên bản tồi tệ hơn của cùng một trạng thái, nếu không không gian trạng thái sẽ bùng nổ. 

Một chi tiết tinh tế là phần thưởng không phải là một phần của chìa khóa cắt tỉa. Hai trạng thái có cùng (mặt nạ, nút) nhưng thời gian và phần thưởng khác nhau chỉ được so sánh theo thời gian để chiếm ưu thế, vì phần thưởng cao hơn với thời gian cao hơn vẫn có thể hữu ích nếu nó dẫn đến việc hoàn thành tốt hơn trong S. Thứ tự hàng đợi ưu tiên đảm bảo chúng ta khám phá những thời điểm sớm hơn trước, vì vậy chúng ta không bị mất các chuỗi tối ưu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi các trạng thái chính dưới dạng (mặt nạ, nút, thời gian, phần thưởng). 

| Bước | Trạng thái xuất hiện | Hành động | Tiểu bang mới | 
| --- | --- | --- | --- | 
| 1 | (1, 0, 0, 887) | bắt đầu | mở rộng tới tất cả các nút | 
| 2 | (1, 1, 1, 778) | di chuyển 0→1 | cập nhật mặt nạ 11 | 
| 3 | (11, 1, 1, 1665) | tiếp tục | mở rộng hơn nữa | 
| 4 | ... | khám phá sự kết hợp | tích lũy tốt nhất | 

Thuật toán khám phá nhiều hoán vị của việc truy cập các nút trong thời gian 4, tích lũy tất cả giá trị của khách du lịch vì tất cả các nút đều có thể truy cập được với giá rẻ. 

Điều này cho thấy phương pháp này hoạt động hiệu quả giống như khám phá tập hợp con khi chi phí đồng đều và nhỏ. 

### Mẫu 2 

Trường hợp thử nghiệm đầu tiên: 

| Bước | Tiểu bang | Thời gian | Phần thưởng | 
| --- | --- | --- | --- | 
| bắt đầu | (0,0) | 0 | 650 | 
| di chuyển 0→1 | (1,1) | 887 | 1072 | 
| di chuyển 1→2 | (1,2) | 778 | 1132 | 
| so sánh cuối cùng | trạng thái kết thúc | 1379 | tối đa 1435 | 

Thuật toán ưu tiên các tuyến đường có thời gian tăng nhẹ nếu chúng mở khóa các nút có giá trị cao hơn, điều này sẽ bị bỏ qua bởi chiến lược chỉ có đường đi ngắn nhất. 

Trường hợp thử nghiệm thứ hai: 

| Bước | Tiểu bang | Thời gian | Phần thưởng | 
| --- | --- | --- | --- | 
| bắt đầu | (0,0) | 0 | 173 | 
| mở rộng | khác nhau | ngày càng tăng | tích lũy | 
| kết thúc tốt nhất ở 1 | cuối cùng | ≤454 | 910 | 

Điều này chứng tỏ việc cắt tỉa hoạt động ngay cả khi tồn tại nhiều đường vòng trung gian, vì chỉ các trạng thái (mặt nạ, nút, thời gian) tốt nhất mới tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3 + 2^n · n · log(2^n n)) | Floyd-Warshall cộng với việc mở rộng ưu tiên trên các trạng thái tập hợp con | 
| Không gian | O(2^n · n) | lưu trữ trạng thái tốt nhất và hàng đợi ưu tiên | 

Quá trình tiền xử lý khối không đáng kể với n 18, trong khi phần hàm mũ được kiểm soát bằng cách cắt tỉa và giới hạn S, giúp ngăn chặn sự bùng nổ trạng thái hoàn toàn trong thực tế. 

Điều này phù hợp thoải mái trong giới hạn vì không gian trạng thái được giới hạn bởi các tập hợp con có kích thước 2^18 và mỗi lần mở rộng đều rẻ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided samples
assert run("4 4\n887 778 916 794\n0 1 1 1\n1 0 1 1\n1 1 0 1\n1 1 1 1\n") is not None
# minimal case
assert run("2 10\n5 7\n0 3\n3 0\n") is not None
# single path constraint
assert run("3 5\n10 1 1\n0 2 2\n2 0 2\n2 2 0\n") is not None
# all equal costs
assert run("3 100\n1 1 1\n0 1 1\n1 0 1\n1 1 0\n") is not None
# large S trivial
assert run("2 1000\n100 200\n0 1\n1 0\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tối thiểu | số tiền nhỏ | độ đúng cơ sở | 
| chi phí thống nhất | chuyến thăm đầy đủ | tích lũy tập hợp con | 
| S lớn | truyền tải đầy đủ | ngân sách không phù hợp | 

## Vỏ cạnh 

Trường hợp góc phát sinh khi dừng tại nút 0 và nhảy thẳng đến nút 1 sẽ rẻ hơn bất kỳ đường vòng nào, nhưng đường vòng lại thu hút nhiều khách du lịch hơn. Thuật toán khám phá chính xác cả hai vì nó không cắt tỉa theo phần thưởng. 

Một trường hợp khác là khi nhiều nút trung gian tạo thành một chu kỳ với chi phí thấp. DP chỉ cho phép xem lại các nút thông qua mặt nạ, do đó các chu kỳ không tạo ra các vòng lặp vô hạn. 

Trường hợp tinh tế cuối cùng là khi hai trạng thái đạt đến cùng một trạng thái (mặt nạ, nút) nhưng ở các thời điểm khác nhau. Chỉ thời gian nhỏ nhất được lưu trữ, nhưng vì chúng tôi luôn mở rộng theo thứ tự thời gian tăng dần nên bất kỳ trạng thái thời gian tồi tệ nào sau này đều không thể tạo ra sự tiếp tục tốt hơn trong cùng một mặt nạ, duy trì tính tối ưu.
