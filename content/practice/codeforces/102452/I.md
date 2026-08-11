---
title: "CF 102452I - Tiểu hành tinh tới"
description: "Chúng tôi duy trì một bộ đếm không giảm cho mỗi đài quan sát. Một thành viên tham gia vào một thời điểm nào đó, chọn tối đa ba đài quan sát riêng biệt và yêu cầu tổng cộng ít nhất y phút từ các đài quan sát đó."
date: "2026-08-10T06:31:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 519
verified: true
draft: false
---

[CF 102452I - Các tiểu hành tinh đang đến](https://codeforces.com/problemset/problem/102452/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một bộ đếm không giảm cho mỗi đài quan sát. Một thành viên tham gia vào một thời điểm nào đó, chọn tối đa ba đài quan sát riêng biệt và yêu cầu tổng cộng ít nhất`y`vài phút từ những đài quan sát đó. Chỉ video được thu thập sau khi thành viên tham gia mới được tính vào mục tiêu của thành viên đó. 

Khi có bản cập nhật bổ sung`w`phút tới đài quan sát`x`, mọi thành viên vẫn còn hoạt động đang sử dụng`x`được`w`thêm phút từ đài quan sát đó. Chúng tôi phải báo cáo chính xác số thành viên có tổng điểm đạt mục tiêu lần đầu tiên trong lần cập nhật này. Các ID được báo cáo phải được sắp xếp ngày càng nhiều. 

Đầu vào được cố ý trực tuyến. giá trị`last`, bằng với số thành viên được báo cáo bởi sự kiện loại 2 trước đó, được sử dụng để giải mã XOR sự kiện hiện tại. Truy vấn đầu tiên sử dụng`last = 0`. Sự kiện loại 1 có mục tiêu của nó và mọi chỉ số quan sát được XOR với`last`, trong khi sự kiện loại 2 có cả đài quan sát và XOR tăng dần với`last`. Chúng ta phải giải mã một sự kiện trước khi sử dụng bất kỳ giá trị nào của nó. 

Vấn đề chính thức có`n,m <= 2 * 10^5`, mục tiêu và giá trị cập nhật nhiều nhất`10^6`và giới hạn thời gian 2 giây với bộ nhớ 512 MB. Điều này ngay lập tức loại trừ các thuật toán quét tất cả các thành viên sau mỗi lần cập nhật. Với`2 * 10^5`các sự kiện, công thức bậc hai sẽ xuất hiện`4 * 10^10`trong trường hợp xấu nhất, vượt xa những gì thực tế có thể thực hiện được. 

Ngoài ra còn có một hạn chế hữu ích về cấu trúc: mỗi thành viên sử dụng tối đa ba đài quan sát. Giới hạn liên tục đó là chìa khóa cho giải pháp nhanh hơn. Chúng tôi có đủ khả năng để kiểm tra tất cả các đài quan sát thuộc về một thành viên bất cứ khi nào báo động của thành viên đó thức dậy, vì có nhiều nhất là ba đài quan sát trong số đó. 

Một số trường hợp cạnh rất dễ xử lý sai. Đầu tiên, video được thu thập trước khi thành viên tham gia không được tính. Coi như:```
1 3
2 1 5
1 3 1 1
2 1 3
```Bản cập nhật đầu tiên làm cho đài quan sát 1 có 5 phút, nhưng chưa có thành viên nên câu trả lời là`0`. Sau đó, thành viên tham gia với mục tiêu 3. 5 phút trước đó không được tính. Bản cập nhật cuối cùng thêm 3 phút mới, vì vậy kết quả đúng là:```
0
0
1 1
```Một giải pháp chỉ so sánh mục tiêu của mọi thành viên với tổng số của đài quan sát toàn cầu sẽ hoàn thành sai mục tiêu của thành viên ngay sau khi tham gia. 

Thứ hai, một thành viên đã hoàn thành phải biến mất vĩnh viễn. Ví dụ:```
1 3
1 2 1 1
2 1 2
2 0 0
```Bản cập nhật đầu tiên sau khi thành viên tham gia có chính xác 2 phút, vì vậy truy vấn đầu tiên sẽ in`1 1`. Từ`last = 1`, bản cập nhật được mã hóa cuối cùng`2 0 0`thực ra có nghĩa là đài quan sát`1`, tăng`1`. Thành viên đã hoàn thành nên kết quả đúng là:```
1 1
0
```Việc thực hiện bất cẩn khiến cảnh báo của thành viên hoạt động sẽ báo cáo lại. 

Thứ ba, việc giải mã XOR phụ thuộc vào số câu trả lời trước đó. Ví dụ:```
1 4
1 1 1 1
2 1 1
1 0 1 0
2 0 0
```Thành viên đầu tiên hoàn thành sau sự kiện thứ hai, vì vậy`last = 1`. Do đó, sự kiện thứ ba giải mã từ`1 0 1 0`cho thành viên mới với mục tiêu 1 và đài quan sát 1. Sự kiện thứ tư giải mã thành một bản cập nhật khác của đài quan sát 1 x 1. Kết quả đầu ra là:```
1 1
1 2
```Nếu XOR được áp dụng sai cách`last`, mọi sự kiện sau đó sẽ bị hỏng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lưu giữ các đài quan sát đã chọn của mỗi thành viên, mục tiêu và lượng video được thu thập từ các đài quan sát đó kể từ khi thành viên tham gia. Về bản cập nhật cho đài thiên văn`x`, chúng tôi có thể kiểm tra mọi thành viên sử dụng`x`, tính lại tổng số hiện tại và kiểm tra xem nó có đạt được mục tiêu hay không. 

Điều này đúng vì bản cập nhật chỉ thay đổi sự đóng góp của`x`. Vấn đề là số lượng các cuộc kiểm tra như vậy. Trong trường hợp xấu nhất, khoảng một nửa sự kiện tạo ra thành viên và nửa còn lại cập nhật cùng một đài quan sát. Nếu mọi thành viên đều sử dụng đài quan sát đó thì chúng tôi sẽ thực hiện khoảng 

[ 
\frac m2 \cdot \frac m2 = \Theta(m^2) 
] 

kiểm tra thành viên. Vì`m = 2 * 10^5`, đây là theo thứ tự của`10^10`séc. 

Quan sát hữu ích là một thành viên với`k`đài quan sát không thể đạt được mục tiêu còn lại`r`trừ khi ít nhất một trong các đài quan sát của nó nhận được ít nhất 

[ 
\left\lceil \frac r k \right\rceil 
] 

phút bổ sung. Đây chỉ là nguyên tắc chuồng bồ câu. Nếu tất cả`k`các đài quan sát nhận được ít hơn số tiền đó, tổng mức tăng của họ sẽ nhỏ hơn rất nhiều so với`r`. 

Điều này cho phép chúng tôi thay thế một điều kiện toàn cầu khó khăn bằng một số cảnh báo cục bộ đơn giản. Giả sử một thành viên hiện đang cần`r`nhiều phút hơn và sử dụng đài quan sát`q1, ..., qk`. Chúng tôi đặt báo động vào mỗi`qi`, nói rằng chúng tôi muốn kiểm tra thành viên này khi đài quan sát đó thu được thành viên khác`ceil(r/k)`phút. 

Mỗi đài quan sát có thể duy trì các báo động của mình trong một đống tối thiểu được sắp xếp theo giá trị bộ đếm tuyệt đối mà tại đó báo động phát ra. Cập nhật cho đài thiên văn`x`chỉ cần kiểm tra đống thuộc về`x`. 

Phần thú vị là điều gì sẽ xảy ra khi có báo động vang lên nhưng thành viên vẫn chưa thực sự hoàn thành. Chúng tôi tính toán lại mục tiêu thực sự còn lại của thành viên. Giả sử bây giờ`r' > 0`. Chúng tôi cài đặt báo thức mới bằng cách sử dụng 

[ 
\left\lceil \frac{r'}k \right\rceil. 
] 

Vì ít nhất chuông báo thức cũ đã kêu sau`ceil(r/k)`số phút mới được thu thập tại một đài quan sát, mục tiêu còn lại giảm ít nhất`ceil(r/k)`. Đặc biệt, 

[ 
r' \le r-\left\lceil\frac rk\right\rceil 
\le \frac{k-1}{k}r. 
]

Từ`k <= 3`, mỗi cảnh báo thất bại sẽ làm giảm mục tiêu còn lại một phần không đổi. Vì`k = 3`, nhiều nhất là`2r/3`. Vì mục tiêu ban đầu nhiều nhất là`10^6`, một thành viên chỉ có thể được xây dựng lại`O(log 10^6)`lần. 

Đây là lý do tại sao cách tiếp cận đống hoạt động. Lực lượng vũ phu kiểm tra mọi thành viên trên mỗi bản cập nhật, trong khi cấu trúc cảnh báo chỉ kiểm tra một thành viên sau khi đã đạt đủ tiến độ để công việc còn lại của thành viên đó phải giảm đáng kể. Bài xã luận chính thức của cuộc thi mô tả chính xác chiến lược này, sử dụng rất nhiều cảnh báo cho mỗi đỉnh. 

Có một vấn đề triển khai trong Python. Một đống lười biếng đơn giản sẽ chèn các cảnh báo mới trong mỗi lần tái thiết và để lại các cảnh báo cũ bên trong các đống. Điều đó rất dễ viết, nhưng nó có thể tạo ra nhiều mục nhập cũ. Thay vào đó, cách triển khai bên dưới sử dụng vùng nhớ nhị phân được lập chỉ mục. Mỗi thành viên có tối đa ba nút cảnh báo và việc xây dựng lại một thành viên sẽ thay đổi khóa của các nút hiện có đó thay vì phân bổ các mục nhập vùng lưu trữ mới. Khi một thành viên hoàn thành, tất cả các nút báo động của nó sẽ bị loại bỏ trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(m^2)`|`O(m)`| Quá chậm | 
| Báo động đống |`O(m log m log V)`|`O(n + m)`| Đã chấp nhận | 

Đây`V <= 10^6`là mục tiêu tối đa. yếu tố`k <= 3`được hấp thụ vào các hằng số. 

## Hướng dẫn thuật toán 

1. Duy trì`value[x]`, tổng số tiền thu được tại đài thiên văn`x`cho đến nay. Giá trị này bao gồm video được thu thập trước khi thành viên tham gia, vì vậy mỗi thành viên cũng lưu trữ giá trị của mọi đài quan sát đã chọn tại thời điểm tham gia. Trừ đi các giá trị đã lưu này sẽ mang lại chính xác mức đóng góp được tính cho thành viên đó. 
2. Khi có thành viên mới có mục tiêu`y`Và`k`các đài quan sát đến, lưu các đài quan sát đã chọn và hiện tại của họ`value`quầy. Đối với mỗi đài quan sát đã chọn, hãy tạo một cảnh báo có mục tiêu là bộ đếm hiện tại cộng với 

[ 
\left\lceil\frac yk\right\rceil. 
] 

Mục tiêu là một giá trị đếm tuyệt đối, do đó, cảnh báo không cần phải nhớ mức độ thay đổi của đài quan sát giữa các sự kiện. 
3. Lưu trữ tất cả các báo động thuộc cùng một đài quan sát trong một đống tối thiểu. Mục tiêu nhỏ nhất là báo động tiếp theo có thể kích hoạt ở đó. Vùng chỉ mục cũng ghi nhớ vị trí của mọi nút cảnh báo, cho phép chúng ta thay đổi hoặc xóa cảnh báo tùy ý trong`O(log m)`thời gian. 
4. Đối với sự kiện loại 2, trước tiên hãy giải mã XOR quan sát và tăng dần bằng cách sử dụng sự kiện trước đó`last`. Tăng`value[x]`theo số gia được giải mã. 
5. Trong khi báo động tối thiểu trong`x`có mục tiêu không lớn hơn mục tiêu mới`value[x]`, xử lý cảnh báo đó. Thành viên gắn liền với cảnh báo được đảm bảo là một trong những thành viên có thể đã trở nên hoàn thiện nhờ bản cập nhật này. 
6. Tính số tiền thực tế thu được của thành viên bằng cách cộng`value[q] - base[q]`trên nhiều nhất ba đài quan sát của nó. Nếu số tiền này ít nhất là mục tiêu của thành viên, hãy đánh dấu thành viên đã hoàn thành, xóa tất cả các nút cảnh báo và gắn ID của thành viên đó vào câu trả lời cho bản cập nhật này. 
7. Nếu thành viên chưa hoàn thành thì hãy`r`là mục tiêu còn lại của nó. Đặt 

[ 
d=\left\lceil\frac rk\right\rceil. 
] 

Thay đổi từng nút báo động của thành viên để mục tiêu mới của nó được`value[q] + d`. Đây là số gia tăng trong tương lai, do đó bộ đếm hiện tại được sử dụng làm điểm bắt đầu mới. 
8. Sau khi xử lý tất cả các cảnh báo phát sinh trong bản cập nhật này, hãy sắp xếp các ID được thu thập cho bản cập nhật này và in chúng. Bộ`last`với số lượng ID được báo cáo. Giá trị này sau đó được sử dụng để giải mã sự kiện tiếp theo. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi thành viên đang hoạt động đều có một báo động trên mỗi đài quan sát đã chọn của mình và mỗi báo động được định vị chính xác`ceil(r/k)`đơn vị tương lai đi xa, ở đâu`r`là mục tiêu còn lại hiện tại của thành viên. Nếu thành viên đạt được mục tiêu thì ít nhất một đài quan sát được chọn phải đạt được ít nhất`ceil(r/k)`kể từ lần tái thiết cuối cùng, vì vậy một trong những cảnh báo này phải kích hoạt không muộn hơn thời điểm thành viên hoàn tất. Vì vậy, một thành viên không thể âm thầm vượt qua điểm hoàn thành của mình mà không bị kiểm tra. 

Khi cảnh báo kích hoạt, chúng tôi sẽ tính toán khoản đóng góp tích lũy chính xác kể từ khi thành viên tham gia. Nếu đã đạt được mục tiêu, chúng tôi sẽ báo cáo thành viên đó và xóa tất cả cảnh báo của thành viên đó để không bao giờ có thể báo cáo lại thành viên đó nữa. Nếu không, mục tiêu còn lại của nó nhiều nhất sẽ trở thành`(k-1) / k`mục tiêu còn lại trước đó của nó và các cảnh báo được xây dựng lại từ trạng thái mới. Do đó mọi thành viên chỉ có thể được xây dựng lại theo logarit nhiều lần. 

Các giá trị cơ sở được lưu trữ xử lý điều kiện chính xác khác: các quan sát được thực hiện trước khi thành viên tham gia không bao giờ được đưa vào tính toán đóng góp của nó. Cuối cùng, việc sắp xếp các ID sau mỗi lần cập nhật sẽ đưa ra chính xác thứ tự tăng dần theo yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    # Current total collected at every observatory.
    value = [0] * n

    # Member data, indexed by member id.
    goals = [0]
    positions = [()]
    bases = [()]
    nodes_of_member = [()]
    active = [False]

    # Every alarm is represented by one node.
    # node_target[node] is its absolute firing target.
    # node_member[node] is the member owning it.
    # node_pos[node] is the observatory heap containing it.
    # node_index[node] is its current index inside that heap.
    node_target = []
    node_member = []
    node_pos = []
    node_index = []

    # One indexed binary min-heap per observatory.
    heaps = [[] for _ in range(n)]

    def less(a, b):
        ta = node_target[a]
        tb = node_target[b]
        if ta != tb:
            return ta < tb
        return a < b

    def sift_up(heap, i):
        node = heap[i]
        while i:
            p = (i - 1) >> 1
            parent = heap[p]
            if not less(node, parent):
                break
            heap[i] = parent
            node_index[parent] = i
            i = p
        heap[i] = node
        node_index[node] = i

    def sift_down(heap, i):
        node = heap[i]
        size = len(heap)
        while True:
            left = i * 2 + 1
            if left >= size:
                break
            right = left + 1
            child = left
            if right < size and less(heap[right], heap[left]):
                child = right
            if not less(heap[child], node):
                break
            heap[i] = heap[child]
            node_index[heap[i]] = i
            i = child
        heap[i] = node
        node_index[node] = i

    def insert_node(node, pos):
        heap = heaps[pos]
        node_pos[node] = pos
        heap.append(node)
        node_index[node] = len(heap) - 1
        sift_up(heap, len(heap) - 1)

    def remove_node(node):
        pos = node_pos[node]
        heap = heaps[pos]
        i = node_index[node]
        last_node = heap.pop()

        if i < len(heap):
            heap[i] = last_node
            node_index[last_node] = i

            if i and less(last_node, heap[(i - 1) >> 1]):
                sift_up(heap, i)
            else:
                sift_down(heap, i)

        node_index[node] = -1

    def change_key(node, new_target):
        node_target[node] = new_target
        pos = node_pos[node]
        heap = heaps[pos]
        i = node_index[node]

        if i and less(node, heap[(i - 1) >> 1]):
            sift_up(heap, i)
        else:
            sift_down(heap, i)

    last = 0
    member_count = 0
    output = []

    for _ in range(m):
        parts = list(map(int, input().split()))
        op = parts[0]

        if op == 1:
            enc_y = parts[1]
            k = parts[2]

            y = enc_y ^ last

            qs = []
            bs = []

            for j in range(k):
                q = parts[3 + j] ^ last
                q -= 1
                qs.append(q)
                bs.append(value[q])

            member_count += 1
            mid = member_count

            goals.append(y)
            positions.append(tuple(qs))
            bases.append(tuple(bs))
            nodes_of_member.append(())
            active.append(True)

            delta = (y + k - 1) // k
            member_nodes = []

            for q in qs:
                node = len(node_target)

                node_target.append(value[q] + delta)
                node_member.append(mid)
                node_pos.append(q)
                node_index.append(-1)

                member_nodes.append(node)
                insert_node(node, q)

            nodes_of_member[mid] = tuple(member_nodes)

        else:
            x = (parts[1] ^ last) - 1
            add = parts[2] ^ last

            value[x] += add
            answer = []

            heap = heaps[x]

            while heap:
                node = heap[0]
                if node_target[node] > value[x]:
                    break

                mid = node_member[node]

                if not active[mid]:
                    remove_node(node)
                    continue

                qs = positions[mid]
                bs = bases[mid]
                k = len(qs)

                collected = 0
                for j in range(k):
                    collected += value[qs[j]] - bs[j]

                if collected >= goals[mid]:
                    active[mid] = False
                    answer.append(mid)

                    for nd in nodes_of_member[mid]:
                        remove_node(nd)
                else:
                    remaining = goals[mid] - collected
                    delta = (remaining + k - 1) // k

                    for j, nd in enumerate(nodes_of_member[mid]):
                        q = qs[j]
                        change_key(nd, value[q] + delta)

            answer.sort()

            output.append(str(len(answer)) + (
                " " + " ".join(map(str, answer)) if answer else ""
            ))

            last = len(answer)

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```các`value`mảng biểu thị lượng tích lũy hiện tại tại mỗi đài quan sát. Nó cố tình không thiết lập lại khi một thành viên tham gia. Thay vì,`bases`ghi lại giá trị cũ cho mỗi camera, do đó phần đóng góp liên quan đến thành viên là`value[q] - bases[q]`. 

các`goals`,`positions`,`bases`, Và`nodes_of_member`tất cả các mảng đều được lập chỉ mục theo ID thành viên. Vì một thành viên sử dụng tối đa ba đài quan sát nên mỗi bản ghi này có kích thước không đổi. 

Việc triển khai heap được lập chỉ mục thay vì lười biếng. Mọi cảnh báo đều có ID nút ổn định và`node_index`cho chúng ta biết chính xác nút đó nằm ở đâu trong vùng quan sát của nó.`change_key`do đó có thể sửa đổi một báo động hiện có trong`O(log m)`, trong khi`remove_node`có thể xóa một trong cùng một độ phức tạp. Điều này tránh việc giữ lại các cảnh báo lỗi thời sau khi xây dựng lại. 

Khi một thành viên không kiểm tra được, mã sẽ tính toán chính xác mục tiêu còn lại của nó và đặt tất cả các mục tiêu cảnh báo liên quan đến các quầy quan sát hiện tại. Biểu hiện trần`(remaining + k - 1) // k`là dạng số nguyên của`ceil(remaining / k)`. 

Khi một thành viên hoàn thành, tất cả các nút báo động của nó sẽ bị xóa ngay lập tức. Đây là lý do tại sao một thành viên đã hoàn thành không thể được báo cáo lại ngay cả khi một đài quan sát đã chọn khác được cập nhật sau đó. 

Đầu vào được giải mã trước khi bất kỳ trạng thái nào được thay đổi. Đối với sự kiện loại 1,`last`được áp dụng cho mục tiêu và mọi chỉ số quan sát. Đối với sự kiện loại 2, nó được áp dụng cho cả đài quan sát và sự tăng dần. Sau khi xử lý sự kiện loại 2,`last`trở thành số lượng thành viên được báo cáo bởi sự kiện đó. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Bộ đếm thực tế lớn nhất có thể vượt quá`10^6`bởi vì nhiều cập nhật có thể tích lũy tại cùng một đài quan sát, đó là một lý do khác để không sử dụng giả định về độ rộng cố định trong phần giải thích hoặc triển khai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu là:```
3 5
1 5 3 1 2 3
2 2 1
1 2 2 1 2
2 3 1
2 1 3
```Thành viên đầu tiên có mục tiêu 5 và camera tại đài quan sát 1, 2 và 3. Mức tăng báo động ban đầu của nó là`ceil(5 / 3) = 2`. 

| Sự kiện |`last`trước | Hoạt động | Tổng số đài quan sát | Nước thành viên | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | Thêm thành viên 1, mục tiêu 5,`{1,2,3}`|`(0,0,0)`| 5 còn lại, báo động lúc`2`| | 
| 2 | 0 | Thêm 1 vào đài quan sát 2 |`(0,1,0)`| 4 còn lại |`0`| 
| 3 | 0 | Thêm thành viên 2, mục tiêu 2,`{1,2}`|`(0,1,0)`| thành viên 2 bắt đầu từ những giá trị này | | 
| 4 | 0 | Thêm 1 vào đài quan sát 3 |`(0,1,1)`| không có báo động nào đạt được mục tiêu |`0`| 
| 5 | 0 | Thêm 3 vào đài quan sát 1 |`(3,1,1)`| thành viên 1 có`3+1+1=5`; thành viên 2 có`3+0=3`|`2 1 2`| 

Sự kiện thứ tư không hoàn thành thành viên 1 vì sự đóng góp của nó chỉ`0 + 1 + 1 = 2`. Bản cập nhật cuối cùng nâng đài quan sát lên 1 đủ để kích hoạt báo động cho cả hai thành viên và cả hai cuộc kiểm tra chính xác đều thành công. ID được sắp xếp trước khi in. 

### Mẫu 2 

Hãy xem xét ví dụ tập trung vào mã hóa:```
1 4
1 1 1 1
2 1 1
1 0 1 0
2 0 0
```| Sự kiện |`last`trước | Hoạt động được giải mã | Tổng đài quan sát | Thành viên tích cực | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | Thành viên 1: mục tiêu 1, camera 1 | 0 |`{1}`| | 
| 2 | 0 | Đài quan sát 1 được 1 | 1 |`{}`|`1 1`| 
| 3 | 1 | Thành viên 2: mục tiêu`0 XOR 1 = 1`, máy ảnh`0 XOR 1 = 1`| 1 |`{2}`| | 
| 4 | 1 | đài quan sát`0 XOR 1 = 1`được`0 XOR 1 = 1`| 2 |`{}`|`1 2`| 

Sự kiện thứ ba là phần hữu ích của ví dụ. Các giá trị được mã hóa của nó trông không hợp lệ nếu được diễn giải trực tiếp, nhưng sau XOR với`last = 1`chúng trở thành mục tiêu hợp lệ và chỉ số quan sát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m log m log V)`| Mỗi thành viên được xây dựng lại`O(log V)`lần, mỗi lần xây dựng lại chạm vào tối đa ba nút vùng nhớ heap và chi phí vận hành mỗi vùng nhớ heap`O(log m)`| 
| Không gian |`O(n + m)`| có`n`đống và tối đa ba nút báo động cho mỗi thành viên | 

Sự giảm hình học đến từ 

[ 
r' \le \frac{k-1}{k}r. 
] 

Trường hợp chậm nhất là`k = 3`, đưa ra một cách đại khái`r' <= 2r/3`. Bắt đầu từ`10^6`, điều này chỉ cần khoảng 35 lần tái thiết thất bại cho một thành viên. Vì mỗi thành viên có tối đa ba cảnh báo nên tổng số nút heap đang hoạt động là`O(m)`. Vùng nhớ được lập chỉ mục giữ cho bộ nhớ này bị ràng buộc ngay cả sau nhiều lần tái tạo cảnh báo. 

Vì`n,m <= 2 * 10^5`, các hệ số logarit là cần thiết cho tổ chức vùng heap, nhưng cải tiến quan trọng là chúng tôi không bao giờ quét các thành viên không liên quan sau khi cập nhật. Hằng số`k <= 3`giữ cho mọi tái thiết đủ nhỏ cho độ phức tạp dự kiến. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`.```python
import sys
import io

from solution import solve

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

# Provided sample.
assert run(
    """3 5
1 5 3 1 2 3
2 2 1
1 2 2 1 2
2 3 1
2 1 3
"""
) == "0\n0\n2 1 2", "sample"

# Minimum-size case: an update with no members.
assert run(
    """1 1
2 1 7
"""
) == "0", "minimum-size case"

# All three cameras receive equal amounts.
assert run(
    """3 4
1 9 3 1 2 3
2 1 3
2 2 3
2 3 3
"""
) == "0\n0\n1 1", "equal contributions"

# Boundary case: the total reaches the goal only on the third camera.
assert run(
    """3 4
1 6 3 1 2 3
2 1 2
2 2 2
2 3 2
"""
) == "0\n0\n1 1", "boundary and rebuild"

# XOR encoding after a nonzero last value.
assert run(
    """1 4
1 1 1 1
2 1 1
1 0 1 0
2 0 0
"""
) == "1 1\n1 2", "online xor decoding"

# Maximum event count: all events add members, so there are no output lines.
# Every event is valid because last remains zero.
max_case = "200000 200000\n" + ("1 1 1 1\n" * 200000)
assert run(max_case) == "", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`với một bản cập nhật |`0`| Kích thước tối thiểu và đống cảnh báo trống | 
| Ba camera có mức tăng bằng nhau |`0`,`0`,`1 1`| Nhiều lần xây dựng lại và`k = 3`trường hợp | 
| Mục tiêu 6 có đóng góp`2,2,2`|`0`,`0`,`1 1`| Ranh giới ngưỡng chính xác và phân chia trần | 
| Vụ án đài quan sát đơn được mã hóa |`1 1`,`1 2`| Sử dụng đúng cách trước đó`last`| 
|`200000`sự kiện loại 1 | Đầu ra trống | Số sự kiện tối đa và xử lý bộ nhớ | 

## Vỏ cạnh 

Trường hợp đặc biệt đầu tiên là video được thu thập trước khi thành viên tham gia. TRONG```
1 3
2 1 5
1 3 1 1
2 1 3
```bộ đếm toàn cầu trở thành 5 trước khi thành viên tồn tại. Khi thành viên được tạo, việc triển khai sẽ lưu trữ`base = 5`. Sau bản cập nhật cuối cùng, đóng góp có liên quan là`value[1] - base = 8 - 5 = 3`, vậy là thành viên đó đã đạt được mục tiêu của mình một cách chính xác. Đầu ra là`0`,`0`,`1 1`. 

Trường hợp thứ hai là thành viên hoàn thành và sau đó nhận thêm thông tin cập nhật. TRONG```
1 3
1 2 1 1
2 1 2
2 0 0
```bản cập nhật đầu tiên làm cho thành viên hoàn thiện. Của nó`active`cờ trở thành sai và tất cả các nút báo động của nó sẽ bị xóa. Từ`last = 1`, bản cập nhật được mã hóa cuối cùng là một bản cập nhật khác cho đài quan sát 1. Vùng heap của nó trống nên không có gì được báo cáo. Đầu ra là`1 1`,`0`. 

Trường hợp cạnh thứ ba là bản cập nhật gây ra cảnh báo nhưng không hoàn thành. Coi như:```
3 4
1 6 3 1 2 3
2 1 2
2 2 2
2 3 2
```Ban đầu, mỗi báo thức cách đó hai phút trong tương lai vì`ceil(6/3)=2`. Bản cập nhật đầu tiên kích hoạt báo động ở đài quan sát 1, nhưng thành viên chỉ thu thập được 2 phút. Mục tiêu còn lại của nó là 4, vì vậy các cảnh báo sẽ được xây dựng lại theo mức tăng dần.`ceil(4/3)=2`. Bản cập nhật thứ hai kích hoạt một báo động khác, còn lại 2 phút. Sau đó, các cảnh báo sẽ được xây dựng lại theo mức tăng dần`ceil(2/3)=1`. Bản cập nhật thứ ba kích hoạt cảnh báo cuối cùng và tổng số chính xác là 6. Đầu ra là`0`,`0`,`1 1`. 

Trường hợp cạnh thứ tư là một bản cập nhật lớn. Giả sử một thành viên có mục tiêu 10 và ba camera thì một camera đột nhiên nhận được 100 phút. Báo thức của nó phát ra một lần và việc kiểm tra chính xác ngay lập tức phát hiện ra rằng mục tiêu đã đạt được. Không cần phải mô phỏng từng phút một. Heap chỉ quan tâm liệu mục tiêu cảnh báo tuyệt đối có bị vượt qua hay không. 

Trường hợp cuối cùng là một số thành viên chia sẻ cùng một đài quan sát. Tất cả đều có các nút báo động riêng biệt trong đống của đài quan sát đó. Bản cập nhật xử lý mục tiêu nhỏ nhất trước tiên và sau khi thành viên đó được hoàn thành hoặc được xây dựng lại, vùng heap sẽ được kiểm tra lại. Điều này lặp lại cho đến khi mục tiêu tối thiểu lớn hơn giá trị quan sát hiện tại. Các thành viên không liên quan đến đài quan sát đó sẽ không bao giờ được cập nhật. 

Nếu bạn muốn, tôi cũng có thể cung cấp phiên bản thứ hai của bài xã luận bằng cách sử dụng kỹ thuật cảnh báo nhị phân mới hơn, giúp cải thiện đường tiệm cận bị ràng buộc gần đúng`O(m log V)`nhưng về cơ bản là phức tạp hơn để thực hiện.
