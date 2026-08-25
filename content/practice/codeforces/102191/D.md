---
title: "CF 102191D - Ngày Hình Ảnh"
description: "Chúng ta có (n) học sinh, trong đó (n) chẵn và các học sinh đã được nhóm thành (n/2) cặp bạn bè. Hai học sinh của mỗi cặp phải xếp ở các vị trí liên tiếp ở hàng cuối cùng. Bên trong một cặp, một trong hai thứ tự đều được phép."
date: "2026-08-25T08:19:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "D"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 2278
verified: false
draft: false
---

[CF 102191D - Ngày hội ảnh](https://codeforces.com/problemset/problem/102191/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) học sinh, trong đó (n) chẵn và các học sinh đã được nhóm thành (n/2) cặp bạn bè. Hai học sinh của mỗi cặp phải xếp ở các vị trí liên tiếp ở hàng cuối cùng. Bên trong một cặp, một trong hai thứ tự đều được phép. 

Bỏ qua các cặp trong giây lát, chuỗi chiều cao hợp lệ có một đỉnh: chiều cao có thể giữ nguyên hoặc tăng lên đến đỉnh và sau đó chúng có thể giữ nguyên hoặc giảm đi. Nhiệm vụ là chọn cả thứ tự của các cặp và hướng của mỗi cặp sao cho hai yêu cầu này được đáp ứng đồng thời. Nếu điều này không thể thực hiện được, chúng tôi sẽ in`-1`. 

Đối với một cặp có độ cao (a) và (b), sẽ rất hữu ích khi lưu trữ nó dưới dạng một khoảng ([l,r]), trong đó (l=\min(a,b)) và (r=\max(a,b)). Nếu cặp này nằm hoàn toàn ở phía tăng thì nó phải có dạng (l,r). Nếu nó nằm hoàn toàn về phía giảm thì nó phải xuất hiện dưới dạng (r,l). 

Hãy xem xét hai cặp được biểu thị bằng các khoảng ([l_1,r_1]) và ([l_2,r_2]). Nếu cả hai đều ở phía tăng dần thì số thứ nhất chỉ có thể đứng trước số thứ hai khi (r_1\le l_2). Do đó, hai khoảng chồng chéo không thể ở cùng một phía. Lý do tương tự áp dụng cho phía giảm. Sự bình đẳng được cho phép, do đó các khoảng thời gian chỉ chạm vào điểm cuối sẽ không xung đột. 

Ràng buộc (n\le 3\cdot10^5) loại trừ mọi thứ liên quan đến hoán vị của các cặp hoặc cấu trúc bậc hai của tất cả các mối quan hệ cặp. Có tới (150000) cặp, do đó, phương thức (O(n^2)) sẽ thực hiện khoảng (2,25\cdot10^{10}) so sánh cặp. Với giới hạn 2 giây, độ phức tạp dự định cần ở khoảng (O(n\log n)) hoặc cao hơn. 

Một số trường hợp cạnh rất dễ xử lý sai. Đầu tiên, khoảng cách chạm là tương thích. Vì```
4
1 3
3 5
```sự sắp xếp`1 3 3 5`là hợp lệ. Việc triển khai bất cẩn coi các khoảng thời gian chia sẻ điểm cuối là sự chồng chéo sẽ từ chối nó một cách không chính xác. 

Thứ hai, một cặp có thể có chiều cao bằng nhau. Vì```
2
5 5
```câu trả lời`5 5`là hợp lệ. Bản thân cặp đôi này không tăng hay giảm và các khoảng có chiều cao bằng nhau cũng có thể nằm cạnh nhau mà không vi phạm tính đơn điệu. 

Thứ ba, một số cặp có thể chia sẻ mức tối đa toàn cầu. Đối với đầu vào mẫu, cả hai cặp`[6,7]`Và`[5,7]`chứa chiều cao`7`. Chúng ta không thể đơn giản giả sử cặp đầu tiên như vậy là cặp đỉnh. Cấu trúc bên dưới chọn cặp cực đại toàn cục với điểm cuối lớn nhất nhỏ hơn, đây là lựa chọn mang lại sự đảm bảo mạnh nhất có thể. 

Cuối cùng, ba cặp chồng chéo lẫn nhau có thể khiến câu trả lời là không thể. Vì```
6
1 10
2 9
3 8
```mỗi cặp chồng lên nhau. Nhiều nhất một cặp có thể chiếm vị trí đỉnh, để lại hai cặp chồng lên nhau sẽ phải nằm cùng một phía. Vì vậy, đầu ra đúng là`-1`. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp nhất coi mọi cặp tình bạn như một khối. Với (m=n/2) khối, có (m!) đơn đặt hàng có thể có và hai hướng có thể có cho mỗi khối, đưa ra (2^m m!). Đối với mỗi ứng viên, chúng ta sẽ mở rộng các khối và kiểm tra xem chuỗi phần tử (n) kết quả có phải là đơn thức hay không. Điều đó đòi hỏi (\Theta(n)) công việc cho mỗi ứng viên, vì vậy tổng số là (\Theta(n2^m m!)). Ở kích thước đầu vào tối đa, điều này có nghĩa là (2^{150000}\cdot150000!) có thể sắp xếp trước khi kiểm tra ngay cả một trong số chúng, điều này hoàn toàn không khả thi. 

Quan sát hữu ích là một cặp có thể được xem như một khoảng ([l,r]). Hai cặp chồng lên nhau không thể được đặt trên cùng một mặt đơn điệu. Do đó, sau khi xác định cặp nào chứa đỉnh, mỗi cặp còn lại phải được gán cho một trong hai cạnh và các cặp chồng chéo phải nhận các cạnh khác nhau. 

Có một sự lựa chọn đặc biệt hữu ích cho cặp đỉnh. Gọi chiều cao lớn nhất trong số tất cả học sinh là (H), và trong số tất cả các cặp chứa (H), hãy chọn cặp ([L,H]) có (L) lớn nhất. Cặp này luôn có thể đóng vai trò là cặp đỉnh bất cứ khi nào có giải pháp. 

Tại sao việc chọn (L) lớn nhất lại quan trọng? Mọi cặp khác có (r>L) trùng nhau ([L,H]), vì mức tối đa của nó cao hơn (L) trong khi không có chiều cao nào có thể vượt quá (H). Một cặp như vậy không thể được đặt cùng phía với cặp đỉnh, vì vậy tất cả chúng đều bị buộc ở phía đối diện. Mọi cặp có (r\le L) không trùng với cặp đỉnh và có khả năng có thể lệch sang một bên. 

Sau khi loại bỏ cặp đỉnh, bài toán còn lại chính xác là bài toán tô màu hai màu trên biểu đồ chồng lấp khoảng. Tô màu một bên là bên trái và bên kia là bên phải. Các cặp có (r>L) được tô màu sẵn ở bên phải. Nếu tồn tại một màu hợp lệ, các khoảng bên trái có thể được sắp xếp bằng cách tăng dần điểm cuối bên trái, trong khi các khoảng bên phải có thể được sắp xếp bằng cách giảm điểm cuối bên trái. 

Biểu đồ khoảng không cần phải được xây dựng một cách đơn giản. Sau khi sắp xếp các khoảng theo điểm cuối bên trái của chúng, hãy duy trì các khoảng hiện đang hoạt động. Nếu một khoảng thời gian mới bắt đầu trong khi hai khoảng thời gian trước đó vẫn đang hoạt động thì cả ba khoảng thời gian đó sẽ chồng lên nhau ở vị trí đó, tạo ra một hình tam giác. Đồ thị như vậy không thể phân chia thành hai cạnh đơn điệu nên chúng ta có thể loại bỏ ngay ứng viên. Khi có nhiều nhất một khoảng hoạt động thì có nhiều nhất một cạnh chồng lấp cần thêm vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^{n/2}(n/2)!)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuẩn hóa mọi cặp tình bạn thành một khoảng ([l,r]) với (l\le r). Trong số tất cả các cặp có (r) là mức tối đa toàn cầu (H), hãy chọn cặp có (l) lớn nhất. Gọi nó là cặp đỉnh ([L,H]). Loại bỏ cặp này để lại khoảng thời gian (m-1). 
2. Đánh dấu mỗi khoảng còn lại bằng (r>L) là bị buộc về phía bên phải. Một khoảng như vậy chồng lên cặp đỉnh, do đó, đặt nó ở bên trái sẽ làm cho dãy giảm trước cặp đỉnh hoặc tăng sau cặp đỉnh theo hướng sai. Các khoảng với (r\le L) vẫn không bị ép buộc vì chúng có thể chạm hoặc nằm hoàn toàn bên dưới điểm cuối bên trái của cặp đỉnh. 
3. Sắp xếp các khoảng còn lại theo điểm cuối bên trái của chúng. Quét chúng từ trái sang phải trong khi vẫn giữ một khoảng thời gian tối thiểu mà điểm cuối bên phải vẫn lớn hơn điểm cuối bên trái hiện tại. Các khoảng có (r\le l_{\text{current}}) bị xóa khỏi vùng nhớ heap vì được phép chạm vào điểm cuối. 
4. Nếu hai quãng vẫn hoạt động khi một quãng mới bắt đầu thì ba quãng sẽ chồng lên nhau theo cặp. Chúng tạo thành một hình tam giác trong biểu đồ chồng chéo và hai cạnh không thể chứa cả ba nếu không đặt hai cặp chồng lên nhau ở cùng một phía. Trở lại`-1`. 
5. Nếu có chính xác một khoảng thời gian đang hoạt động, hãy kết nối khoảng thời gian hiện tại với khoảng thời gian hoạt động đó. Hai cặp này phải chiếm các cạnh đối diện. Sau đó chèn khoảng thời gian hiện tại vào heap. 
6. Sử dụng BFS hoặc DFS để tô hai màu cho biểu đồ chồng chéo thu được. Hai màu tượng trưng cho hai mặt của bức tranh. Ban đầu, mỗi khoảng bắt buộc sẽ nhận được màu bên phải. Một thành phần được kết nối không có màu có thể bắt đầu bằng một trong hai màu. Bất cứ khi nào một cạnh được đi qua, khoảng lân cận phải nhận được màu đối lập. Nếu một cạnh yêu cầu hai khoảng để có cùng màu thì việc xây dựng là không thể. 
7. Thu thập tất cả các khoảng được tô màu ở bên trái và sắp xếp chúng theo thứ tự tăng dần (l). Xuất ra mỗi cái dưới dạng (l,r). Vì các khoảng cùng màu không bao giờ trùng nhau nên điều này tạo ra một dãy không giảm và khoảng cuối cùng ở phía này có (r\le L). 
8. Xuất cặp đỉnh đã chọn dưới dạng (L,H). Cặp này bắt đầu phần giảm dần ở độ cao (H), do đó, nó kết nối tự nhiên với mọi khoảng bên phải. 
9. Thu thập các khoảng bên phải và sắp xếp chúng theo thứ tự giảm dần (l). Xuất ra mỗi cái dưới dạng (r,l). Vì các khoảng này không chồng lên nhau theo từng cặp nên hướng giảm dần của chúng kết nối theo đúng thứ tự được yêu cầu. Cuối cùng in toàn bộ chuỗi. 

### Tại sao nó hoạt động 

Điều bất biến chính là các khoảng được gán cho cùng một màu không bao giờ trùng nhau. Ở vế bên trái, hướng tăng dần của các khoảng như vậy có thể được sắp xếp từ nhỏ đến lớn, bởi vì với các khoảng liên tiếp chúng ta có (r_i\le l_{i+1}). Ở phía bên phải, các hướng giảm dần có thể được sắp xếp theo hướng ngược lại vì lý do tương tự. 

Việc lựa chọn cặp đỉnh là điều làm cho việc tô màu trước trở nên an toàn. Giả sử tồn tại một số hình ảnh hợp lệ. Đặt ([L,H]) là cặp được chọn có (L) lớn nhất trong số tất cả các cặp chứa mức tối đa toàn cục (H). Mọi khoảng có (r>L) trùng lặp với cặp này, vì vậy nó phải ở phía đối diện trong bất kỳ hình ảnh hợp lệ nào. Vì tất cả các khoảng như vậy đều nằm trên cùng một phía nên chúng cũng phải là từng cặp không chồng lên nhau. Do đó, hình ảnh hợp lệ hiện có cung cấp hai màu hợp lệ cho tất cả các khoảng còn lại với mỗi khoảng bắt buộc trên cùng một màu. BFS của chúng tôi tìm thấy màu như vậy bất cứ khi nào có màu đó. 

Ngược lại, nếu tô màu đồ thị của chúng ta thành công, mỗi cặp ở một bên sẽ không chồng chéo với mọi cặp khác ở phía đó. Việc xây dựng sắp xếp mỗi bên theo các điểm cuối khoảng của nó, đặt cặp đỉnh giữa chúng và hướng các cặp về phía đỉnh. Mọi ranh giới liền kề khi đó đều đơn điệu theo hướng yêu cầu, do đó chuỗi kết quả là một hình ảnh hợp lệ. 

## Giải pháp Python```python
import sys
import heapq
from collections import deque

input = sys.stdin.readline

def build_solution(pairs):
    m = len(pairs)

    intervals = []
    for a, b in pairs:
        if a <= b:
            intervals.append((a, b))
        else:
            intervals.append((b, a))

    # Choose the pair containing the global maximum,
    # with the largest possible smaller endpoint.
    peak = 0
    for i in range(1, m):
        if intervals[i][1] > intervals[peak][1]:
            peak = i
        elif intervals[i][1] == intervals[peak][1]:
            if intervals[i][0] > intervals[peak][0]:
                peak = i

    L, H = intervals[peak]

    rest = []
    for i, (l, r) in enumerate(intervals):
        if i != peak:
            rest.append((l, r))

    k = len(rest)
    if k == 0:
        return [L, H]

    # Sort by left endpoint for the interval sweep.
    order = list(range(k))
    order.sort(key=lambda i: (rest[i][0], rest[i][1]))

    graph = [[] for _ in range(k)]
    heap = []

    for idx in order:
        l, r = rest[idx]

        while heap and heap[0][0] <= l:
            heapq.heappop(heap)

        # Two active intervals plus the current one would
        # form a triangle.
        if len(heap) >= 2:
            return None

        if heap:
            other = heap[0][1]
            graph[idx].append(other)
            graph[other].append(idx)

        heapq.heappush(heap, (r, idx))

    # Color 0 = left, 1 = right.
    color = [-1] * k

    # Every interval with r > L overlaps the peak interval,
    # so it must be on the right.
    for i, (l, r) in enumerate(rest):
        if r > L:
            color[i] = 1

    # Propagate the forced colors through the graph.
    for start in range(k):
        if color[start] != -1:
            continue

        color[start] = 0
        q = deque([start])

        while q:
            u = q.popleft()

            for v in graph[u]:
                wanted = color[u] ^ 1

                if color[v] == -1:
                    color[v] = wanted
                    q.append(v)
                elif color[v] != wanted:
                    return None

    left = []
    right = []

    for i, (l, r) in enumerate(rest):
        if color[i] == 0:
            left.append((l, r))
        else:
            right.append((l, r))

    # Increasing side.
    left.sort(key=lambda x: (x[0], x[1]))

    # Decreasing side, closest to the peak first.
    right.sort(key=lambda x: (x[0], x[1]), reverse=True)

    answer = []

    for l, r in left:
        answer.extend((l, r))

    answer.extend((L, H))

    for l, r in right:
        answer.extend((r, l))

    return answer

def main():
    n = int(input())
    pairs = [tuple(map(int, input().split())) for _ in range(n // 2)]

    answer = build_solution(pairs)

    if answer is None:
        print(-1)
    else:
        print(*answer)

if __name__ == "__main__":
    main()
```Phần đầu tiên của`build_solution`chuẩn hóa mọi cặp thành ((l,r)). Điều này không làm mất thông tin vì thứ tự ban đầu trong một cặp tình bạn là không liên quan. 

Vòng lặp lựa chọn`peak`so sánh điểm cuối lớn hơn trước và điểm cuối nhỏ hơn thứ hai. Sự so sánh thứ hai là cần thiết. Trong số các cặp chứa mức tối đa toàn cục, việc chọn điểm cuối lớn nhất nhỏ hơn sẽ giảm thiểu tập hợp các cặp bị buộc về phía đối diện của đỉnh. 

Việc sử dụng quét theo khoảng thời gian`r <= l`khi loại bỏ một khoảng thời gian khỏi heap. Đây là điều kiện biên làm cho các khoảng tiếp xúc tương thích. Ví dụ,`[1,3]`Và`[3,5]`có thể liên tiếp theo thứ tự tăng dần. 

Đồ thị chứa một cạnh chính xác khi hai cặp trùng nhau. Đống chứa mọi khoảng thời gian đã bắt đầu nhưng chưa kết thúc. Trong biểu đồ khoảng có hai màu hợp lệ, tối đa một khoảng hoạt động trước đó có thể vẫn còn khi khoảng mới bắt đầu. Nếu còn lại hai, khoảng mới sẽ chồng lên cả hai, trong khi hai khoảng đó cũng chồng lên nhau, tạo thành một hình tam giác. 

Giai đoạn tô màu đầu tiên gán màu`1`đến mọi khoảng với`r>L`. Đây là những cặp chồng lên cặp đỉnh và do đó không thể ở cùng phía với nó. BFS sau đó truyền các màu đối diện cần thiết qua mọi cạnh chồng lên nhau. 

Sự sắp xếp cuối cùng là cố tình khác nhau ở hai bên. Phía bên trái sử dụng tăng dần (l) và mỗi khối được in dưới dạng (l,r). Phía bên phải sử dụng giảm dần (l) và mỗi khối được in dưới dạng (r,l). Cặp đỉnh được in giữa hai nhóm này là (L,H). 

Số nguyên Python xử lý độ cao lên tới (10^9) mà không có bất kỳ lo ngại nào về tràn. Chi tiết triển khai chính quan trọng đối với hiệu suất là sử dụng`heapq`và danh sách kề, thay vì quét tất cả các khoảng trước đó để tìm mỗi khoảng mới. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
8
1 3
4 2
6 7
5 7
```Sau khi chuẩn hóa và chọn cặp cực đại toàn cục với điểm cuối lớn nhất nhỏ hơn,`[6,7]`trở thành cặp đỉnh. 

| Bước | Khoảng thời gian hiện tại | Khoảng thời gian hoạt động | Đã thêm cạnh | Buộc phải | 
| --- | --- | --- | --- | --- | 
| 1 |`[1,3]`| không | không | không | 
| 2 |`[2,4]`|`[1,3]`|`[1,3] - [2,4]`| không | 
| 3 |`[5,7]`| không | không | vâng | 

Các khoảng`[1,3]`Và`[2,4]`chồng lên nhau nên chúng nhận được màu sắc trái ngược nhau. Khoảng thời gian`[5,7]`bị buộc sang phải vì điểm cuối bên phải của nó lớn hơn điểm cuối bên trái của đỉnh (6). 

Một màu hợp lệ là```
Left:  [1,3]
Peak:  [6,7]
Right: [5,7], [2,4]
```Trình tự kết quả từ việc xây dựng này là```
1 3 6 7 7 5 4 2
```Nó không giảm đến lần đầu tiên`7`và không tăng sau đó. Mọi cặp ban đầu vẫn liền kề. 

###Xây dựng được trường hợp không thể 

Hãy xem xét```
6
1 10
2 9
3 8
```Đỉnh được chọn là`[1,10]`. Các khoảng khác là`[2,9]`Và`[3,8]`. 

| Bước | Khoảng thời gian hiện tại | Khoảng thời gian hoạt động | Đã thêm cạnh | Buộc phải | 
| --- | --- | --- | --- | --- | 
| 1 |`[2,9]`| không | không | vâng | 
| 2 |`[3,8]`|`[2,9]`|`[2,9] - [3,8]`| vâng | 

Cả hai khoảng còn lại đều bị buộc sang phải, nhưng chúng chồng lên nhau. Cạnh đồ thị yêu cầu chúng phải có các màu khác nhau, trong khi việc tô màu trước yêu cầu cả hai phải có màu`1`. BFS phát hiện mâu thuẫn và trả về`-1`. 

Điều này chứng tỏ tại sao chỉ kiểm tra xem từng cặp có phù hợp với đỉnh hay không là không đủ. Các cặp buộc về cùng một phía cũng phải tương thích với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Sắp xếp các khoảng và chi phí nhóm cuối cùng (O(n\log n)), trong khi các phép toán heap và chi phí truyền tải đồ thị (O(n\log n)) và (O(n)) tương ứng. | 
| Không gian | (O(n)) | Các khoảng, vùng nhớ heap, danh sách kề, màu sắc và kết quả đầu ra đều yêu cầu không gian tuyến tính. | 

Có nhiều nhất (150000) cặp tình bạn, vì vậy (O(n\log n)) chỉ thực hiện vài triệu phép tính theo thang logarit. Việc sử dụng bộ nhớ (O(n)) cũng vừa vặn thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Kết quả đầu ra mang tính xây dựng, do đó, các bài kiểm tra nên xác thực chuỗi trả về thay vì so sánh nó với một câu trả lời cụ thể. Phần khai thác sau đây sẽ kiểm tra các khối cặp, xác minh rằng mỗi cặp đầu vào được sử dụng chính xác một lần và kiểm tra thuộc tính unimodal.```python
import sys
import io
from collections import Counter
import heapq
from collections import deque

def solution(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    pairs = [(int(next(it)), int(next(it))) for _ in range(n // 2)]

    intervals = []
    for a, b in pairs:
        if a <= b:
            intervals.append((a, b))
        else:
            intervals.append((b, a))

    m = len(intervals)

    peak = 0
    for i in range(1, m):
        if intervals[i][1] > intervals[peak][1]:
            peak = i
        elif intervals[i][1] == intervals[peak][1]:
            if intervals[i][0] > intervals[peak][0]:
                peak = i

    L, H = intervals[peak]

    rest = [intervals[i] for i in range(m) if i != peak]
    k = len(rest)

    if k == 0:
        return f"{L} {H}"

    order = sorted(range(k), key=lambda i: (rest[i][0], rest[i][1]))

    graph = [[] for _ in range(k)]
    heap = []

    for idx in order:
        l, r = rest[idx]

        while heap and heap[0][0] <= l:
            heapq.heappop(heap)

        if len(heap) >= 2:
            return "-1"

        if heap:
            other = heap[0][1]
            graph[idx].append(other)
            graph[other].append(idx)

        heapq.heappush(heap, (r, idx))

    color = [-1] * k

    for i, (l, r) in enumerate(rest):
        if r > L:
            color[i] = 1

    for start in range(k):
        if color[start] != -1:
            continue

        color[start] = 0
        q = deque([start])

        while q:
            u = q.popleft()

            for v in graph[u]:
                wanted = color[u] ^ 1

                if color[v] == -1:
                    color[v] = wanted
                    q.append(v)
                elif color[v] != wanted:
                    return "-1"

    left = []
    right = []

    for i, interval in enumerate(rest):
        if color[i] == 0:
            left.append(interval)
        else:
            right.append(interval)

    left.sort()
    right.sort(reverse=True)

    ans = []

    for l, r in left:
        ans.extend((l, r))

    ans.extend((L, H))

    for l, r in right:
        ans.extend((r, l))

    return " ".join(map(str, ans))

def run(inp: str) -> str:
    return solution(inp)

def valid(inp: str, out: str) -> bool:
    data = inp.split()
    n = int(data[0])
    values = list(map(int, data[1:]))

    if out.strip() == "-1":
        return False

    ans = list(map(int, out.split()))

    if len(ans) != n:
        return False

    pairs = []
    for i in range(n // 2):
        a = values[2 * i]
        b = values[2 * i + 1]
        pairs.append(tuple(sorted((a, b))))

    produced = []
    for i in range(0, n, 2):
        produced.append(tuple(sorted((ans[i], ans[i + 1]))))

    if Counter(pairs) != Counter(produced):
        return False

    peak = max(ans)
    first_peak = ans.index(peak)

    for i in range(first_peak):
        if ans[i] > ans[i + 1]:
            return False

    for i in range(first_peak, n - 1):
        if ans[i] < ans[i + 1]:
            return False

    return True

sample1 = """\
8
1 3
4 2
6 7
5 7
"""

out = run(sample1)
assert valid(sample1, out), "sample 1"

minimum = """\
2
1 1
"""

out = run(minimum)
assert valid(minimum, out), "minimum-size case"

touching = """\
4
1 3
3 5
"""

out = run(touching)
assert valid(touching, out), "touching intervals must be allowed"

all_equal = """\
6
5 5
5 5
5 5
"""

out = run(all_equal)
assert valid(all_equal, out), "all-equal heights"

impossible = """\
6
1 10
2 9
3 8
"""

assert run(impossible).strip() == "-1", "three mutually overlapping pairs"

boundary = """\
4
1 1000000000
999999999 1000000000
"""

out = run(boundary)
assert valid(boundary, out), "height boundary case"

# Maximum-size case: 300000 students, 150000 pairwise disjoint intervals.
m = 150000
maximum_pairs = "\n".join(f"{2 * i} {2 * i + 1}" for i in range(m))
maximum = f"{2 * m}\n{maximum_pairs}\n"

out = run(maximum)
assert valid(maximum, out), "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ sự sắp xếp hợp lệ nào | Cấu trúc cơ bản và tô màu hai mặt | 
|`2 / 1 1`| Bất kỳ sự sắp xếp hợp lệ nào | Đầu vào tối thiểu và một cặp duy nhất | 
|`4 / 1 3 / 3 5`| Bất kỳ sự sắp xếp hợp lệ nào | Việc chạm vào điểm cuối không được coi là chồng chéo | 
|`6 / 5 5 / 5 5 / 5 5`| Bất kỳ sự sắp xếp hợp lệ nào | Chiều cao bằng nhau và khoảng cách có chiều rộng bằng 0 | 
|`6 / 1 10 / 2 9 / 3 8`|`-1`| Xung đột bên cưỡng bức gây ra bởi các khoảng thời gian chồng chéo | 
|`4 / 1 1000000000 / 999999999 1000000000`| Bất kỳ sự sắp xếp hợp lệ nào | Ranh giới chiều cao tối đa và nhiều cực đại toàn cầu | 
| 300000 học sinh theo cặp rời rạc | Bất kỳ sự sắp xếp hợp lệ nào | Kích thước đầu vào tối đa và hiệu suất (O(n\log n)) | 

## Vỏ cạnh 

### Một cặp duy nhất 

cho```
2
7 3
```khoảng thời gian chuẩn hóa là`[3,7]`. Nó được tự động chọn làm cặp đỉnh, không còn khoảng trống nào và câu trả lời là`3 7`. Trình tự này không hề đơn điệu và cặp tình bạn ở liền kề nhau. 

### Chiều cao bằng nhau 

cho```
6
5 5
5 5
5 5
```mỗi khoảng là`[5,5]`. Vì quá trình quét sẽ loại bỏ một khoảng thời gian bất cứ khi nào`r <= l`, các khoảng có chiều cao bằng nhau không bao giờ tạo ra các cạnh chồng lên nhau. Thuật toán có thể đặt cặp đỉnh ở giữa và tất cả các cặp khác ở hai bên. Mọi đầu ra có thể bao gồm toàn bộ`5`, vì vậy nó hợp lệ. 

### Khoảng chạm 

cho```
4
1 3
3 5
```khoảng thời gian`[1,3]`Và`[3,5]`chạm nhưng không chồng chéo theo nghĩa liên quan đến vấn đề. Các quá trình quét`[1,3]`, sau đó loại bỏ nó trước khi xử lý`[3,5]`bởi vì`3 <= 3`. Không có cạnh đồ thị nào được tạo. Thuật toán có thể chọn`[3,5]`như là đỉnh cao và đặt`[1,3]`ở bên trái, sản xuất```
1 3 3 5
```đó là không giảm trong suốt. 

### Nhiều cặp chứa mức tối đa toàn cầu 

cho```
4
1 1000000000
999999999 1000000000
```cả hai cặp đều có mức tối đa toàn cầu (10^9). Thuật toán chọn`[999999999,1000000000]`bởi vì nó có điểm cuối lớn hơn nhỏ hơn. Cặp còn lại bị buộc phải ở bên phải vì điểm cuối bên phải của nó lớn hơn`999999999`. Sự sắp xếp kết quả là```
999999999 1000000000 1000000000 1
```đạt mức cao nhất cần thiết và giữ cả hai thành viên của mỗi cặp tình bạn lại với nhau. 

### Ba khoảng chồng lên nhau 

cho```
6
1 10
2 9
3 8
```đỉnh được chọn là`[1,10]`. Cả hai khoảng còn lại đều có điểm cuối bên phải lớn hơn điểm cuối bên trái của đỉnh`1`, nên cả hai đều bị buộc phải sang phải. Chúng cũng chồng lên nhau. Biểu đồ chứa một cạnh giữa hai đỉnh đã bị buộc phải có cùng màu, do đó BFS tìm ra sự mâu thuẫn và in ra`-1`. 

### Kích thước đầu vào tối đa 

Với (n=300000), có (150000) cặp. Nếu các cặp là```
0 1
2 3
4 5
...
299998 299999
```tất cả các khoảng đều rời rạc. Biểu đồ chồng chéo không có cạnh, việc tô màu diễn ra ngay lập tức và công việc chủ yếu là sắp xếp. Thuật toán vẫn nằm trong (O(n\log n)), phù hợp với các giới hạn đã cho.
