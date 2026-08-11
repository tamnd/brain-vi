---
title: "CF 102452H - Giữ dây"
description: "Chúng tôi có một mảng (N) rãnh. Một chiến hào ban đầu trống và mỗi chiến hào chỉ có thể tiếp nhận một người lính nhiều nhất một lần. Khi một người lính được đặt ở vị trí (x), vị trí đó vĩnh viễn có chiều cao (h). Một truy vấn đưa ra khoảng vị trí ([L,R]) và chiều cao của kẻ thù (H)."
date: "2026-08-10T06:22:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 388
verified: true
draft: false
---

[CF 102452H - Giữ vững đường dây](https://codeforces.com/problemset/problem/102452/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng (N) rãnh. Một chiến hào ban đầu trống và mỗi chiến hào chỉ có thể tiếp nhận một người lính nhiều nhất một lần. Khi một người lính được đặt ở vị trí (x), vị trí đó vĩnh viễn có chiều cao (h). 

Một truy vấn đưa ra khoảng vị trí ([L,R]) và chiều cao của kẻ thù (H). Trong số tất cả binh lính hiện có mặt trong khoảng thời gian đó, chúng ta cần giá trị tối thiểu là (|h-H|). Nếu khoảng thời gian đó không có người lính nào hiện có thể sử dụng được thì câu trả lời là (-1). 

Có hai thứ tự khác nhau ẩn trong vấn đề. Vị trí xác định liệu một người lính có thuộc khoảng thời gian được yêu cầu hay không, trong khi chỉ số sự kiện xác định xem người lính đó đã được đặt hay chưa khi truy vấn xảy ra. Một giải pháp phải tôn trọng cả hai điều kiện cùng một lúc. 

Những hạn chế làm cho việc quét trực tiếp không thể thực hiện được. Tổng cộng có thể có (5\cdot10^5) chiến hào và (10^6) sự kiện. Trong trường hợp xấu nhất, khoảng (5\cdot10^5) sự kiện có thể là truy vấn sau khi tất cả (5\cdot10^5) chiến hào đã nhận được binh lính, do đó, việc quét khoảng thời gian cho mỗi truy vấn có thể thực hiện khoảng (2,5\cdot10^{11}) kiểm tra vị trí. Ngay cả một cây phân đoạn thông thường có các nút chứa các tập hợp có thứ tự cân bằng cũng cho (O((N+M)\log^2N)), nhưng bài xã luận chính thức chỉ ra cụ thể rằng giải pháp đơn giản này có quá nhiều chi phí không đổi so với các giới hạn chặt chẽ. 

Trang vấn đề Codeforces hiện tại đưa ra giới hạn thời gian 4,5 giây và giới hạn bộ nhớ 512 MB. Điều này làm cho cả độ phức tạp tiệm cận và cách biểu diễn cấu trúc dữ liệu đều có liên quan. Việc triển khai bên dưới sử dụng mảng số nguyên nhỏ gọn cho hàng đợi đơn điệu lớn thay vì hàng triệu đối tượng Python. 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Đầu tiên, một truy vấn có thể xảy ra trước khi mọi người lính trong phạm vi của nó được chèn vào.```
1
1 1
1 1 1 5
```Câu trả lời là```
-1
```Vẫn chưa có người lính nào. Một thuật toán ngoại tuyến chèn mọi người lính cuối cùng mà không kiểm tra thời gian chèn sẽ trả về một giá trị không chính xác. 

Vấn đề thứ hai là một người lính chỉ có thể được chèn vào điểm cuối bên phải sau truy vấn trước đó.```
1
2 3
1 1 2 5
0 2 5
1 1 2 5
```Đầu ra là```
-1
0
```Truy vấn đầu tiên không được nhìn thấy phần chèn sau. Truy vấn thứ hai có thể nhìn thấy nó. 

Trường hợp ranh giới thứ ba là khoảng một vị trí. Bài kiểm tra vị trí phải bao gồm cả hai đầu.```
1
3 5
0 1 100
0 3 1
1 1 1 50
1 2 2 50
1 3 3 50
```Đầu ra là```
50
-1
49
```Truy vấn ở giữa chỉ kiểm tra vị trí 2, trống. Việc triển khai phạm vi vô tình xử lý ([L,R)) thay vì ([L,R]) có thể âm thầm thất bại trong những trường hợp như vậy. 

Cuối cùng, chiều cao bằng nhau phải được xử lý bình thường. Nếu một số binh sĩ có chiều cao 7 và kẻ địch có chiều cao 7, câu trả lời chính xác là 0, không phải khoảng cách đến một số độ cao khác biệt. 

## Phương pháp tiếp cận 

Giải pháp brute-force duy trì độ cao hiện tại ở mọi rãnh. Đối với một truy vấn ([L,R,H]), nó quét mọi vị trí từ (L) đến (R), bỏ qua các vị trí trống và giữ chênh lệch tuyệt đối nhỏ nhất so với (H). Điều này đúng vì mọi người lính đủ điều kiện đều được kiểm tra và mức tối thiểu đối với tất cả họ chính là câu trả lời bắt buộc. 

Vấn đề là số lần quét. Một truy vấn bao gồm toàn bộ mảng có giá (O(N)) và có thể có (O(M)) các truy vấn như vậy. Theo giới hạn tổng hợp nhất định, điều này đạt tới khoảng (2,5\cdot10^{11}) kiểm tra vị trí, vượt xa giới hạn thời gian cho phép. 

Một cải tiến tự nhiên là tạo một cây phân đoạn trên các vị trí, với tập hợp chiều cao được sắp xếp theo thứ tự ở mỗi nút. Truy vấn phạm vi phân tách ([L,R]) thành các nút (O(\log N)) và mỗi nút có thể tìm thấy nút trước và nút kế tiếp của (H) trong (O(\log N)). Điều này mang lại (O((N+M)\log^2N)), nhưng việc duy trì một cây cân bằng riêng biệt trong mỗi nút phân đoạn sẽ tốn kém cả về bộ nhớ và hệ số không đổi. Bài xã luận chính thức mô tả cách tiếp cận đơn giản này và sau đó thay thế nó bằng cấu trúc ngoại tuyến. 

Quan sát chính là các truy vấn có thể được xử lý theo thứ tự tăng dần của điểm cuối bên phải (R). Giả sử chúng ta hiện đang xử lý tất cả các truy vấn có điểm cuối bên phải là (R). Chúng ta có thể chèn tối đa mọi người lính ở vị trí (R) vào cấu trúc dữ liệu. Điều này loại bỏ hoàn toàn ranh giới bên phải khỏi truy vấn. 

Các điều kiện hiệu lực còn lại đối với một người lính ở vị trí (j) là 

[ 
j\ge L 
] 

và 

[ 
v_j < tôi, 
] 

trong đó (v_j) là chỉ mục sự kiện mà người lính được chèn vào và (i) là chỉ mục sự kiện của truy vấn hiện tại. Điều kiện đầu tiên là vị trí, còn điều kiện thứ hai là tạm thời. 

Bây giờ hãy xây dựng cây phân đoạn theo giá trị chiều cao thay vì vị trí. Một nút biểu thị một khoảng độ cao và lưu trữ các sự kiện chèn thuộc khoảng độ cao đó. 

Có một quy tắc thống trị hữu ích bên trong một nút. Các vị trí được chèn theo thứ tự tăng dần do quá trình quét bên ngoài (R=1,2,\ldots,N). Giả sử hai lính được lưu trữ có vị trí (j<k), nhưng thời gian chèn của chúng thỏa mãn (v_j>v_k). Người lính (j) không bao giờ hữu ích. Bất cứ khi nào người lính (j) đủ tuổi để sẵn sàng, người lính (k) cũng có sẵn và (k) ở xa hơn về bên phải. Do đó, với mọi điều kiện giới hạn dưới (j\ge L), người lính (k) ít nhất cũng tốt bằng (j). 

Do đó, chúng tôi có thể loại bỏ các phần tử thống trị như vậy khỏi phía sau hàng đợi của mỗi nút. Các chỉ số sự kiện sống sót tăng dần từ trước ra sau và vị trí của chúng cũng tăng dần từ trước ra sau. Đây là hàng đợi đơn điệu được mô tả bởi bài xã luận chính thức. 

Sự đơn điệu đó làm cho bài kiểm tra tính hợp lệ nhỏ đi một cách đáng ngạc nhiên. Đưa ra một truy vấn có chỉ mục sự kiện (i) và điểm cuối bên trái (L), tìm kiếm nhị phân hàng đợi của nút để tìm chỉ mục sự kiện đầu tiên ít nhất (i). Phần tử trước đó là sự kiện chèn lớn nhất nhỏ hơn (i). Bởi vì cả chỉ số sự kiện và vị trí đều tăng cùng nhau nên phần tử này cũng có vị trí lớn nhất trong số tất cả các binh sĩ được chèn trước truy vấn. Nếu vị trí của nó ít nhất là (L), nút đó chứa một người lính hợp lệ. Nếu vị trí của nó nhỏ hơn (L) thì không có phần tử nào trước đó có thể thỏa mãn điều kiện vị trí. 

Khi một nút có thể được kiểm tra theo cách này, việc tìm kiếm độ cao sẽ trở thành điều hướng cây phân đoạn thông thường. Chúng tôi tìm kiếm chiều cao hợp lệ lớn nhất tối đa (H) và độc lập tìm kiếm chiều cao hợp lệ nhỏ nhất ít nhất (H). Hai ứng cử viên đó là đủ vì mọi giá trị bên dưới (H) đều ở xa hơn giá trị lớn nhất bên dưới nó và mọi giá trị trên (H) đều ở xa hơn giá trị nhỏ nhất phía trên nó.

Độ phức tạp thu được là (O(N\log N+M\log^2N)), phù hợp với cách tiếp cận chính thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(NM)) | (O(N)) | Quá chậm | 
| Cây phân đoạn với tập hợp thứ tự | (O((N+M)\log^2N)) | (O(N\log N)) | Quá nhiều chi phí | 
| Cây phân đoạn ngoại tuyến với hàng đợi đơn điệu | (O(N\log N+M\log^2N)) | (O(N\log N+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các sự kiện và ghi nhớ từng vị trí lính cắm vào chiến hào của mình. Đối với mọi truy vấn, hãy nhớ điểm cuối bên trái, chiều cao của kẻ thù, chỉ mục sự kiện và điểm cuối bên phải. Chúng tôi nhóm các truy vấn theo (R), vì quá trình quét ngoại tuyến sẽ xử lý các vị trí từ trái sang phải. 
2. Nén chiều cao của tất cả binh lính. Chỉ có chiều cao của người lính cần phải là lá của cây phân đoạn. Đối với chiều cao truy vấn (H),`bisect_left`Và`bisect_right`xác định phạm vi chiều cao nén gần nhất ở hai bên. 
3. Xây dựng cây phân đoạn trên độ cao của người lính đã nén. Một người lính có cấp bậc chiều cao (p) thuộc về lá (p) và mọi tổ tiên của lá đó. Do đó, mỗi nút đại diện cho tất cả những người lính có chiều cao nằm trong khoảng giá trị của nút đó. 
4. Trước khi xử lý quá trình quét, hãy tính toán xem mỗi nút cây phân đoạn có thể cần bao nhiêu mục lính. Mỗi người lính đóng góp một mục nhập tiềm năng cho mỗi nút trên đường dẫn từ gốc đến lá của nó. Việc triển khai sử dụng số đếm này để phân bổ một mảng số nguyên toàn cầu nhỏ gọn, tránh danh sách Python riêng cho mỗi nút. 
5. Quét các vị trí rãnh từ (1) đến (N). Khi đến vị trí (R), hãy đưa người lính vào (R), nếu có. Chỉ mục sự kiện của nó được thêm vào mọi nút cây phân đoạn trên đường dẫn chiều cao của nó. 
6. Trong khi thêm chỉ mục sự kiện (i) vào một nút, hãy xóa các mục ở phía sau trong khi chỉ mục sự kiện của chúng lớn hơn (i). Vị trí hiện tại lớn hơn mọi vị trí được chèn trước đó vào nút đó, do đó, mục nhập bị xóa có vị trí sớm hơn nhưng thời gian chèn muộn hơn. Nó không bao giờ có thể là nhân chứng tốt nhất cho việc kiểm tra tính hợp lệ trong tương lai. 
7. Xử lý tất cả các truy vấn có điểm cuối bên phải là hiện tại (R). Đối với mỗi truy vấn, tìm kiếm cây phân đoạn chiều cao hai lần. Tìm kiếm đầu tiên tìm thấy chiều cao lớn nhất (H) có nút chứa một người lính hợp lệ. Cái thứ hai tìm thấy chiều cao nhỏ nhất ít nhất (H) có nút chứa một người lính hợp lệ. 
8. Đối với mỗi nút được tìm kiếm theo chiều cao truy cập, hãy kiểm tra xem nút đó có chứa một người lính có vị trí ít nhất (L) và chỉ mục sự kiện chèn nhỏ hơn chỉ mục truy vấn hiện tại hay không. Tìm kiếm nhị phân cho ra chỉ mục sự kiện cuối cùng nhỏ hơn truy vấn. Bởi vì vị trí của hàng đợi tăng lên theo chỉ số sự kiện của nó nên việc kiểm tra từng người lính là đủ. 
9. Chuyển hai chiều cao ứng cử viên thành chênh lệch tuyệt đối từ (H). Nếu không có bên nào tồn tại, trả về (-1). Nếu không thì trả lại số chênh lệch nhỏ hơn. Chiều cao chính xác tạo ra sự khác biệt 0 và đương nhiên chiến thắng mọi ứng cử viên khác. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ nút cây phân đoạn nào và hàng đợi còn sót lại của nó. Các chỉ số sự kiện của nó đang tăng lên một cách nghiêm ngặt và vị trí của nó cũng đang tăng lên một cách nghiêm ngặt. Đối với truy vấn tại sự kiện (i), mọi mục hàng đợi có chỉ mục sự kiện ít nhất (i) là quá muộn. Trong số các mục có chỉ số sự kiện nhỏ hơn (i), mục cuối cùng có vị trí lớn nhất. Do đó, nút chứa một người lính hợp lệ chính xác khi sự kiện cuối cùng trước đó có vị trí ít nhất (L). 

Quy tắc loại bỏ không bao giờ xóa một người lính có khả năng hữu ích. Nếu một người lính ở vị trí sớm hơn có thời gian chèn muộn hơn một người lính ở vị trí sau, thì người lính ở vị trí sau sẽ có sẵn không muộn hơn và đáp ứng mọi điều kiện giới hạn bên trái mà người lính ở vị trí trước đó có thể đáp ứng. Do đó, người lính bị loại bỏ sẽ bị thống trị. 

Quá trình quét bên ngoài đã chèn tối đa mọi vị trí (R), vì vậy mọi người lính được xem xét bởi một nút đều nằm ở phía bên phải của điểm cuối bên phải. Kiểm tra chỉ số sự kiện sẽ loại bỏ những người lính xuất hiện sau truy vấn hiện tại. Cùng với nhau, hai điều kiện này để lại chính xác những người lính trong ([L,R]) đã có mặt tại thời điểm truy vấn. 

Cuối cùng, trong số các độ cao hợp lệ, độ cao gần nhất với (H) phải là độ cao lớn nhất không vượt quá (H) hoặc độ cao nhỏ nhất không nhỏ hơn (H). Hai tìm kiếm trên cây phân đoạn tìm thấy chính xác những ứng cử viên đó, vì vậy khoảng cách tối thiểu của chúng là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right
from array import array

def solve():
    input = sys.stdin.readline
    T = int(input())
    output = []

    for _ in range(T):
        N, M = map(int, input().split())

        # For every position, store its unique insertion event and height.
        update_id_at_pos = array('i', [0]) * (N + 1)
        update_height_at_pos = array('i', [0]) * (N + 1)

        # Queries are linked by their right endpoint.
        query_head = array('i', [-1]) * (N + 1)
        query_next = array('i', [-1]) * (M + 1)
        query_left = array('i', [0]) * (M + 1)
        query_height = array('i', [0]) * (M + 1)
        is_query = bytearray(M + 1)

        # Position of every update event, indexed by event id.
        update_pos = array('i', [0]) * (M + 1)

        update_heights = []

        for event_id in range(1, M + 1):
            parts = input().split()
            typ = int(parts[0])

            if typ == 0:
                x = int(parts[1])
                h = int(parts[2])

                update_id_at_pos[x] = event_id
                update_height_at_pos[x] = h
                update_pos[event_id] = x
                update_heights.append(h)
            else:
                L = int(parts[1])
                R = int(parts[2])
                H = int(parts[3])

                is_query[event_id] = 1
                query_left[event_id] = L
                query_height[event_id] = H

                query_next[event_id] = query_head[R]
                query_head[R] = event_id

        answer = array('i', [-1]) * (M + 1)

        if not update_heights:
            for event_id in range(1, M + 1):
                if is_query[event_id]:
                    output.append("-1\n")
            continue

        # Coordinate compression only needs actual soldier heights.
        values = sorted(set(update_heights))
        K = len(values)

        # Rank of the soldier height at every position.
        rank_at_pos = array('i', [0]) * (N + 1)

        # Use an iterative segment tree with K leaves.
        S = 1
        while S < K:
            S <<= 1

        node_count = 2 * S

        # cnt[node] is the maximum number of queue entries needed by
        # that node. Every update contributes once to every ancestor.
        cnt = array('i', [0]) * node_count

        for x in range(1, N + 1):
            event_id = update_id_at_pos[x]
            if event_id:
                rank = bisect_left(values, update_height_at_pos[x]) + 1
                rank_at_pos[x] = rank

                node = S + rank - 1
                while node:
                    cnt[node] += 1
                    node >>= 1

        # Give every node a fixed slice of one global queue array.
        base = array('i', [0]) * node_count
        tail = array('i', [0]) * node_count

        total = 0
        for node in range(1, node_count):
            base[node] = total
            tail[node] = total
            total += cnt[node]

        # Each entry is an event id, so 32 bits are enough.
        queue = array('i', [0]) * total

        def check(node, qid, left):
            """Does this node contain a valid soldier?"""
            b = base[node]
            t = tail[node]

            if b == t:
                return False

            # queue[b:t] contains strictly increasing event ids.
            p = bisect_left(queue, qid, b, t)

            if p == b:
                return False

            candidate = queue[p - 1]
            return update_pos[candidate] >= left

        def find_left(rank, qid, left):
            """Largest valid height rank <= rank, or 0."""
            if rank <= 0:
                return 0

            node = S + rank - 1

            if check(node, qid, left):
                return rank

            while node > 1:
                # node is a right child, so its left sibling is
                # completely inside the prefix we are searching.
                if node & 1:
                    sibling = node - 1

                    if check(sibling, qid, left):
                        node = sibling

                        # Find the rightmost valid leaf in this subtree.
                        while node < S:
                            right = node * 2 + 1
                            if check(right, qid, left):
                                node = right
                            else:
                                node *= 2

                        return node - S + 1

                node >>= 1

            return 0

        def find_right(rank, qid, left):
            """Smallest valid height rank >= rank, or 0."""
            if rank > K:
                return 0

            node = S + rank - 1

            if check(node, qid, left):
                return rank

            while node > 1:
                # node is a left child, so its right sibling is
                # completely inside the suffix we are searching.
                if (node & 1) == 0:
                    sibling = node + 1

                    if check(sibling, qid, left):
                        node = sibling

                        # Find the leftmost valid leaf in this subtree.
                        while node < S:
                            left_child = node * 2
                            if check(left_child, qid, left):
                                node = left_child
                            else:
                                node = left_child + 1

                        return node - S + 1

                node >>= 1

            return 0

        # Sweep the right endpoint.
        for R in range(1, N + 1):
            event_id = update_id_at_pos[R]

            if event_id:
                rank = rank_at_pos[R]
                node = S + rank - 1

                # Add the event to every node on the root-to-leaf path.
                # The queue is monotone in event id.
                while node:
                    t = tail[node]
                    b = base[node]

                    while t > b and queue[t - 1] > event_id:
                        t -= 1

                    queue[t] = event_id
                    tail[node] = t + 1
                    node >>= 1

            # All these queries have exactly this R as their right endpoint.
            qid = query_head[R]

            while qid != -1:
                L = query_left[qid]
                H = query_height[qid]

                # Greatest compressed value <= H.
                right_rank = bisect_right(values, H)

                # Smallest compressed value >= H.
                left_rank = bisect_left(values, H) + 1

                best = -1

                if right_rank:
                    rank = find_left(right_rank, qid, L)
                    if rank:
                        best = H - values[rank - 1]

                if left_rank <= K:
                    rank = find_right(left_rank, qid, L)
                    if rank:
                        diff = values[rank - 1] - H
                        if best == -1 or diff < best:
                            best = diff

                answer[qid] = best
                qid = query_next[qid]

        # Restore the original event order.
        for event_id in range(1, M + 1):
            if is_query[event_id]:
                output.append(str(answer[event_id]) + "\n")

    sys.stdout.write("".join(output))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai lưu trữ các bản cập nhật theo vị trí và truy vấn theo điểm cuối bên phải. Việc biểu diễn danh sách liên kết cho các truy vấn tránh tạo ra một bộ dữ liệu Python cho mọi sự kiện, điều này quan trọng khi (M) đạt tới (10^6). 

Việc nén chiều cao chỉ được thực hiện ở độ cao thực sự xảy ra ở binh lính. Chiều cao truy vấn không cần lá cây phân đoạn riêng của nó.`bisect_right(values, H)`đưa ra cấp bậc chiều cao người lính cuối cùng có thể là cấp bậc tiền nhiệm, trong khi`bisect_left(values, H) + 1`đưa ra cấp bậc kế thừa đầu tiên có thể. 

Cây phân đoạn sử dụng gốc lũy thừa hai lá`S`. Xếp hạng nén từ 1 đến (K) ánh xạ tới lá`S + rank - 1`. Cây chứa các lá trống khi (K) không phải là lũy thừa của 2, nhưng các hàm tìm kiếm không bao giờ trả về các lá đó vì`check`ở đó là sai. 

Việc lưu trữ hàng đợi đáng được quan tâm đặc biệt trong Python. Một danh sách thông thường cho mỗi nút cây phân đoạn có thể tiêu tốn hàng trăm megabyte vì số mục nhập hàng đợi là (O(N\log N)). Thay vào đó, trước tiên mã sẽ tính dung lượng tối đa cần thiết cho mỗi nút, gán cho mỗi nút một lát liền kề của một nút.`array('i')`và lưu trữ các chỉ mục sự kiện trong bộ đệm chung đó. Tổng dung lượng lưu trữ hàng đợi vẫn là (O(N\log N)), nhưng mỗi chỉ mục sự kiện chiếm bốn byte. 

Khi một người lính được chèn vào, mã sẽ hiển thị các chỉ số sự kiện lớn hơn từ phần đuôi trước khi thêm sự kiện mới. Vị trí đã tăng lên vì vòng lặp bên ngoài truy cập các vị trí theo thứ tự tăng dần. Đây chính xác là quy tắc thống trị đằng sau hàng đợi đơn điệu. 

các`check`công dụng chức năng`bisect_left`trên khoảng thời gian id sự kiện của nút. Nếu sự kiện chèn ngay trước truy vấn ở vị trí ít nhất (L), nút đó hợp lệ. Nếu không, không có sự kiện nào trước đó có thể hoạt động vì vị trí hàng đợi tăng cùng với id sự kiện. 

Các tìm kiếm trước và sau sử dụng đường dẫn từ lá liên quan đến gốc. Bất cứ khi nào đường dẫn hiện tại đến từ một cây con bên phải, anh em bên trái của nó là một cây con ứng cử viên được khám phá hoàn toàn cho việc tìm kiếm tiền thân. Việc tìm kiếm kế tiếp là đối xứng. Sau khi tìm được anh em khả thi, mã lần lượt đi xuống lá khả thi ngoài cùng bên phải hoặc ngoài cùng bên trái. 

Thứ tự sự kiện cũng tinh tế. Một bản cập nhật tại vị trí (R) được chèn trước khi xử lý các truy vấn có điểm cuối bên phải là (R), ngay cả khi bản cập nhật đó xảy ra sau đó theo thứ tự sự kiện. Đây là cố ý. Kiểm tra id sự kiện của hàng đợi đơn điệu sẽ từ chối nó bất cứ khi nào id sự kiện của nó không nhỏ hơn id truy vấn. Do đó, lệnh quét xử lý điều kiện vị trí, trong khi hàng đợi xử lý điều kiện thời gian. 

Tất cả chiều cao và chênh lệch đều khớp với số nguyên 32 bit đã ký vì chiều cao tối đa là (10^9). Các chỉ số sự kiện cũng phù hợp với số nguyên 32 bit đã ký. Bản thân Python có các số nguyên có độ chính xác tùy ý, nhưng các mảng nhỏ gọn có chủ ý sử dụng bộ lưu trữ 32 bit. 

## Ví dụ đã hoạt động 

Mẫu chính thức có một trường hợp thử nghiệm. Trình tự sự kiện là:```
1
3 5
1 1 3 2
0 1 1
0 3 3
1 1 2 2
1 2 3 2
```Truy vấn đầu tiên xảy ra trước khi một trong hai người lính được chèn vào. Quét ngoại tuyến sau này cuối cùng sẽ chèn cả hai người lính, nhưng chỉ số sự kiện của chúng lớn hơn chỉ mục sự kiện của truy vấn đầu tiên, do đó cả hai đều không thể vượt qua`check`. 

| Sự kiện | Hành động | R hiện tại | Lính nhập ngũ | Truy vấn chiều cao ứng viên | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Truy vấn ([1,3],H=2) | 3 | không có giá trị nào theo thời gian | không | -1 | 
| 2 | Vị trí chèn 1, chiều cao 1 | 1 | (1\mapsto1) | | | 
| 3 | Vị trí chèn 3, chiều cao 3 | 3 | (1\mapsto1,\ 3\mapsto3) | | | 
| 4 | Truy vấn ([1,2],H=2) | 2 | vị trí 1 hợp lệ | 1 | 1 | 
| 5 | Truy vấn ([2,3],H=2) | 3 | vị trí 3 hợp lệ | 3 | 1 | 

Đầu ra là```
-1
1
1
```Phần thú vị là truy vấn đầu tiên được xử lý trong quá trình quét (R=3) sau khi cả hai vị trí đã được chèn. Điều kiện id sự kiện vẫn từ chối cả hai người lính. Đây là bất biến trung tâm của phép biến đổi ngoại tuyến. 

Đối với ví dụ thứ hai, hãy xem xét:```
1
5 8
1 1 5 10
0 2 7
0 5 13
1 2 5 10
0 1 10
1 1 2 10
1 3 4 10
0 4 9
```Các kết quả đầu ra là:```
-1
3
0
-1
```Truy vấn đầu tiên xảy ra trước bất kỳ thao tác chèn nào. Ở sự kiện thứ tư, vị trí 2 và 5 đã được chèn vào, có độ cao 7 và 13. Đối với kẻ địch có chiều cao 10, cả hai đều là khoảng cách 3. Cấu trúc dữ liệu có thể chọn một trong hai vì chỉ yêu cầu sự khác biệt. 

Ở sự kiện 6, vị trí 1 có chiều cao 10 và được chèn trước đó nên khớp chính xác cho ra 0. Ở sự kiện 7, khoảng được yêu cầu là ([3,4]), nhưng vị trí 4 chưa được chèn, trong khi vị trí 3 trống nên đáp án là (-1). 

| Sự kiện | Hành động | R hiện tại | Những người lính hợp lệ trong phạm vi truy vấn | Chiều cao gần nhất | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Truy vấn ([1,5],H=10) | 5 | không có trước sự kiện 1 | không | -1 | 
| 2 | Vị trí chèn 2, chiều cao 7 | 2 | | | | 
| 3 | Vị trí chèn 5, chiều cao 13 | 5 | | | | 
| 4 | Truy vấn ([2,5],H=10) | 5 | (2\mapsto7,\ 5\mapsto13) | 7 hoặc 13 | 3 | 
| 5 | Vị trí chèn 1, chiều cao 10 | 1 | | | | 
| 6 | Truy vấn ([1,2],H=10) | 2 | (1\mapsto10,\ 2\mapsto7) | 10 | 0 | 
| 7 | Truy vấn ([3,4],H=10) | 4 | không có giá trị | không | -1 | 
| 8 | Vị trí chèn 4, chiều cao 9 | 4 | | | | 

Ví dụ này thực hiện cả ranh giới của tìm kiếm độ cao và sự phân biệt giữa thứ tự vị trí và thứ tự sự kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N+M\log^2N)) | Mỗi bản cập nhật chạm vào nút cây (O(\log N)). Mỗi truy vấn thực hiện hai tìm kiếm độ cao, mỗi lần truy cập các nút (O(\log N)), với tìm kiếm nhị phân bên trong mỗi hàng đợi nút. | 
| Không gian | (O(N\log N+M+N)) | Hàng đợi đơn điệu chứa tối đa một vị trí tiềm năng cho mỗi bản cập nhật trên mỗi cấp cây, trong khi siêu dữ liệu sự kiện và truy vấn sử dụng không gian (O(M+N)). | 

Ràng buộc tổng hợp (\sum N\le5\cdot10^5) giữ tổng dung lượng hàng đợi trong (O(5\cdot10^5\log5\cdot10^5)), khoảng mười triệu vị trí số nguyên. Việc triển khai lưu trữ các vị trí đó dưới dạng số nguyên bốn byte, về cơ bản phù hợp hơn với Python so với danh sách các đối tượng số nguyên Python. Bài xã luận chính thức đưa ra cùng một giới hạn (O(n\log n+m\log^2n)) cho cách tiếp cận hàng đợi đơn điệu. 

Giới hạn (M\le10^6) cũng giải thích lý do tại sao mã tránh các bộ sự kiện nặng về đối tượng và xử lý các truy vấn thông qua các mảng nhỏ gọn và danh sách liên kết. Thuật toán được thiết kế dựa trên thực tế là mỗi chiến hào nhận được tối đa một người lính, do đó có nhiều nhất (N) bản cập nhật mặc dù có thể có nhiều truy vấn hơn. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp trên được lưu trong cùng một tệp và điểm vào của nó là`solve()`chức năng. Nó thay thế đầu vào và đầu ra tiêu chuẩn để các xác nhận thực hiện việc triển khai thực tế thay vì một thuật toán tham chiếu riêng biệt.```python
# helper: run the solution on one input string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Official sample
assert run(
    """1
3 5
1 1 3 2
0 1 1
0 3 3
1 1 2 2
1 2 3 2
"""
) == "-1\n1\n1\n", "official sample"

# Minimum-size case
assert run(
    """1
1 3
1 1 1 7
0 1 7
1 1 1 7
"""
) == "-1\n0\n", "minimum size and future update"

# Boundary and singleton intervals
assert run(
    """1
3 5
0 1 100
0 3 1
1 1 1 50
1 2 2 50
1 3 3 50
"""
) == "50\n-1\n49\n", "singleton ranges"

# Equal heights and exact matches
assert run(
    """1
4 6
0 1 7
0 2 7
0 3 7
1 1 3 9
1 2 2 7
1 4 4 7
"""
) == "2\n0\n-1\n", "equal values"

# Queries before and after insertions, including a later right endpoint
assert run(
    """1
5 8
1 1 5 10
0 2 7
0 5 13
1 2 5 10
0 1 10
1 1 2 10
1 3 4 10
0 4 9
"""
) == "-1\n3\n0\n-1\n", "time and position boundaries"

# Maximum M stress shape: N = 1, M = 1,000,000.
# Only the first event inserts the soldier; every later query must answer 0.
M = 1_000_000
lines = ["1", f"1 {M}", "0 1 123456789"]
lines.extend("1 1 1 123456789" for _ in range(M - 1))
max_m_input = "\n".join(lines) + "\n"

expected = "0\n" * (M - 1)
assert run(max_m_input) == expected, "maximum M"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức |`-1, 1, 1`| Các phần chèn thêm trong tương lai và các tìm kiếm tiền thân/kế thừa thông thường | 
| (N=1) trường hợp tối thiểu |`-1, 0`| Truy vấn trống theo sau là kết quả khớp chính xác | 
| Khoảng đơn |`50, -1, 49`| Ranh giới bao gồm (L,R) và vị trí trống | 
| Chiều cao bằng nhau |`2, 0, -1`| Chiều cao trùng lặp và kết quả khớp chính xác | 
| Ranh giới thời gian và vị trí |`-1, 3, 0, -1`| Thứ tự sự kiện so với thứ tự vị trí | 
| (M=10^6), (N=1) | (999999) số không | Số sự kiện tối đa và truy vấn lặp lại | 

## Vỏ cạnh 

### Truy vấn trước khi chèn bất kỳ 

cho```
1
1 1
1 1 1 5
```truy vấn là sự kiện 1. Không có người lính nào nên câu trả lời là (-1). Cây chiều cao trống và việc triển khai trực tiếp xuất ra (-1) khi không có bản cập nhật. 

### Chèn trong tương lai 

cho```
1
2 3
1 1 2 5
0 2 5
1 1 2 5
```quá trình quét đạt tới (R=2) và chèn người lính từ sự kiện 2 trước khi xử lý truy vấn từ sự kiện 3. Đối với sự kiện 1, người lính tương tự cũng hiện diện thực tế trong cấu trúc ngoại tuyến, nhưng chỉ số sự kiện của nó là 2, không vượt qua bài kiểm tra nghiêm ngặt`candidate < qid`. Do đó sự kiện 1 trả về (-1). Sự kiện 3 nhìn thấy người lính và trả về 0. 

### Khoảng đơn 

cho```
1
3 5
0 1 100
0 3 1
1 1 1 50
1 2 2 50
1 3 3 50
```truy vấn đầu tiên chỉ nhìn thấy vị trí 1 và trả về (|100-50|=50). Cái thứ hai chỉ nhìn thấy vị trí 2, trống nên trả về (-1). Người thứ ba chỉ nhìn thấy vị trí 3 và trả về (|1-50|=49). Việc quét sử dụng điều kiện`update_pos[candidate] >= L`, trong khi (R) đã được cố định bởi vòng lặp bên ngoài, do đó cả hai điểm cuối vẫn được bao gồm. 

### Chiều cao trùng lặp 

cho```
1
4 6
0 1 7
0 2 7
0 3 7
1 1 3 9
1 2 2 7
1 4 4 7
```truy vấn đầu tiên tìm thấy chiều cao 7 và trả về 2. Truy vấn thứ hai tìm thấy chiều cao chính xác 7 ở vị trí 2 và trả về 0. Truy vấn thứ ba chỉ chứa vị trí trống 4 và trả về (-1). Sử dụng nén`sorted(set(update_heights))`, do đó, các chiều cao trùng lặp sẽ trở thành một lá giá trị, trong khi hàng đợi nút vẫn chứa tất cả các sự kiện chèn có chiều cao đó. 

### Mục nhập hàng đợi thống trị 

Hãy xem xét hai người lính trong cùng một phân đoạn giá trị, với vị trí trước đó được chèn sau:```
1
2 3
0 1 10
0 2 10
1 1 2 10
```Ở đây thứ tự sự kiện ngày càng tăng nên cả hai mục đều tồn tại. Để xem quy tắc thống trị, tình huống liên quan là khi vị trí nhỏ hơn có chỉ số sự kiện lớn hơn:```
1
2 3
1 1 2 10
0 2 10
0 1 10
```Truy vấn đầu tiên được xử lý với chỉ mục sự kiện 1, do đó, thao tác chèn sau này không hợp lệ. Ở truy vấn cuối cùng, cả hai người lính đều hợp lệ. Khi lính vị trí-2 được chèn đầu tiên trong quá trình quét (R=2) và lính vị trí-1 chỉ được chèn khi đợt quét đạt đến vị trí 1, việc xây dựng hàng đợi tuân theo thứ tự vị trí thay vì thứ tự sự kiện. Nếu một người lính ở vị trí sau có chỉ số sự kiện nhỏ hơn, nó sẽ loại bỏ các chỉ số sự kiện lớn hơn khỏi đuôi. Những người lính bị loại bỏ chiếm ưu thế vì người lính mới vừa có mặt sớm hơn vừa ở xa hơn bên phải. 

### Kết hợp chính xác được bao quanh bởi các độ cao khác 

Giả sử chiều cao hợp lệ là 3 và 9 và chiều cao của kẻ thù là 6. Không bên nào là chính xác, do đó cả người tiền nhiệm và người kế nhiệm đều cần:```
1
3 4
0 1 3
0 3 9
1 1 3 6
1 1 3 6
```Cả hai ứng cử viên đều có khoảng cách 3. Thuật toán tìm hạng 3 thông qua tìm kiếm tiền thân và hạng 9 thông qua tìm kiếm kế tiếp, sau đó lấy giá trị nhỏ nhất. Nó không cho rằng một bên luôn là đủ. 

Bất biến quan trọng trong tất cả các trường hợp này là hàng đợi của nút chứa chính xác các sự kiện chèn không thống trị cho khoảng chiều cao đó. Các chỉ số sự kiện và vị trí của nó tăng lên cùng nhau. Khi bất biến đó được giữ nguyên, một tìm kiếm nhị phân sẽ xác định sự kiện hợp lệ mới nhất và sự kiện đó cũng có vị trí hợp lệ xa nhất. Sau đó, cây phân đoạn sẽ giảm toàn bộ truy vấn có độ cao gần nhất thành hai tìm kiếm đơn điệu.
