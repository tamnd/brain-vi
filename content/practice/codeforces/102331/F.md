---
title: "CF 102331F - Cây kéo dài nhanh"
description: "Chúng ta có một tập các đỉnh có trọng số và một danh sách các cạnh được lập chỉ mục. Ban đầu không có cạnh đồ thị nào nên mỗi đỉnh là thành phần liên thông của chính nó."
date: "2026-08-14T05:00:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "F"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 276
verified: true
draft: false
---

[CF 102331F - Cây kéo dài nhanh](https://codeforces.com/problemset/problem/102331/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập các đỉnh có trọng số và một danh sách các cạnh được lập chỉ mục. Ban đầu không có cạnh đồ thị nào nên mỗi đỉnh là thành phần liên thông của chính nó. Đối với một cạnh (i=(a_i,b_i,s_i)), cạnh đó có thể được sử dụng khi các điểm cuối của nó hiện nằm trong các thành phần khác nhau và tổng trọng số đỉnh của hai thành phần đó ít nhất là (s_i). Trong số tất cả các cạnh có thể sử dụng được, quy trình luôn chọn chỉ số nhỏ nhất, nối hai thành phần và lặp lại. 

Đầu ra chính xác là chuỗi các chỉ số cạnh được chọn bởi quá trình này. Bởi vì mọi thao tác thành công đều kết hợp hai thành phần khác nhau nên có thể chọn tối đa (n-1) cạnh. Vấn đề ban đầu có (n, m\le 300000), trọng số và ngưỡng đỉnh nhiều nhất (10^6), giới hạn thời gian 5 giây và bộ nhớ 256 MiB. 

Mô phỏng trực tiếp không thể kiểm tra nhiều lần tất cả (m) cạnh sau mỗi lần hợp. Trong trường hợp xấu nhất có thể có (n-1) phép kết hợp thành công, do đó việc quét mọi cạnh sau mỗi phép kết hợp sẽ thực hiện gần đúng. 

[ 
(n-1)m < 9\cdot 10^{10} 
] 

kiểm tra cạnh. Điều đó vượt xa những gì việc triển khai trong 5 giây có thể thực hiện được. 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. Đầu tiên, một cạnh có ngưỡng bằng 0 có thể sử dụng được ngay lập tức, nhưng chỉ một lần nếu các bản sao sau đó kết nối các đỉnh đã được kết nối. Ví dụ,```
2 2
0 0
1 2 0
1 2 0
```có đầu ra```
1
1
```Cạnh thứ hai cũng thỏa mãn điều kiện về trọng lượng nhưng các điểm cuối của nó đã được kết nối. 

Sự bình đẳng chính xác cũng quan trọng vì điều kiện lớn hơn hoặc bằng ngưỡng. Vì```
2 1
2 3
1 2 5
```đầu ra là```
1
1
```Một so sánh chặt chẽ sẽ loại bỏ cạnh duy nhất một cách không chính xác. 

Một cạnh ban đầu không sử dụng được có thể trở nên sử dụng được sau khi một cạnh có chỉ số nhỏ hơn, hoàn toàn khác được chọn. Ví dụ,```
3 2
1 1 1
1 2 3
2 3 2
```ban đầu chỉ có cạnh 2 là có thể sử dụng được. Sau khi cạnh 2 nối đỉnh 2 và đỉnh 3, thành phần đó có trọng số 2, do đó cạnh 1 có thể sử dụng được vì (1+2=3). Đầu ra đúng là```
2
2 1
```Quá trình quét một lần qua mảng cạnh sẽ dừng không chính xác sau khi chọn cạnh 2. 

Cuối cùng, giới hạn dưới danh nghĩa (n\ge1) không đưa ra một trường hợp hợp lệ với (n=1,m\ge1), bởi vì mỗi bộ dữ liệu yêu cầu hai điểm cuối riêng biệt. Trường hợp hợp lệ nhỏ nhất có hai đỉnh. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu rất đơn giản về mặt khái niệm. Duy trì DSU, quét liên tục các cạnh từ chỉ số 1 đến (m) và chọn cạnh đầu tiên có điểm cuối có các đại diện khác nhau và có tổng hai thành phần đạt đến ngưỡng của nó. Sau khi kết hợp thành công, hãy bắt đầu một lần quét khác từ chỉ mục 1. Điều này tuân theo chính xác định nghĩa, do đó tính chính xác của nó là ngay lập tức. Vấn đề của nó là quá trình quét lặp lại: có thể có (n-1) kết hợp và mỗi lần quét có chi phí (O(m)), cho (O(nm)) hoặc gần như (9\cdot10^{10}) kiểm tra ở mức giới hạn tối đa. 

Quan sát hữu ích là trọng lượng thành phần chỉ tăng lên. Xét một cạnh có hai trọng số thành phần hiện tại là (x) và (y), với ngưỡng (s). Nếu chưa sử dụng được thì xác định số tiền còn lại là 

[ 
r=s-x-y>0. 
] 

Để cạnh có thể sử dụng được, trọng số của hai thành phần cùng nhau phải tăng ít nhất (r). Ít nhất một trong hai thành phần phải đóng góp ít nhất 

[ 
\left\lceil\frac r2\right\rceil. 
] 

Vì vậy, thay vì kiểm tra cạnh này bất cứ khi nào thành phần điểm cuối thay đổi, chúng tôi đặt cảnh báo cho cả hai thành phần. Báo động về thành phần trọng lượng (x) kích hoạt vào lúc 

[ 
x+\left\lceil\frac r2\right\rceil, 
] 

và những vụ cháy khác tại 

[ 
y+\left\lceil\frac r2\right\rceil. 
] 

Nếu không có báo động nào phát ra thì chắc chắn cạnh đó không thể sử dụng được. Khi một cảnh báo kích hoạt, chúng tôi sẽ kiểm tra cạnh một lần. Nếu (x+y\ge s), cạnh sẽ đủ điều kiện trên toàn cầu. Nếu không, chúng tôi sẽ tính lại số tiền còn lại và chia số tiền còn lại đó làm đôi. 

Giả sử chuông báo động ở phía đầu tiên phát ra. Cạnh đó đã tăng ít nhất (\lceil r/2\rceil) nên số tiền còn lại mới thỏa mãn 

[ 
r' \le r-\left\lceil\frac r2\right\rceil 
= \left\lfloor\frac r2\right\rfloor. 
] 

Do đó, mỗi khi cạnh tương tự được xem xét lại mà không thể sử dụng được thì ngưỡng còn lại của nó ít nhất sẽ giảm đi một nửa. Vì (s\le10^6), một cạnh chỉ cần tính toán lại cảnh báo (O(\log s)). 

Chúng ta vẫn cần tránh việc có cảnh báo cho mỗi cạnh được gắn riêng biệt với mỗi đỉnh. Tất cả các đỉnh trong cùng một thành phần được kết nối đều có trọng số thành phần giống hệt nhau, do đó cảnh báo của chúng có thể được lưu trữ cùng nhau. Mỗi thành phần DSU sở hữu một lượng cảnh báo tối thiểu. Khi hai thành phần hợp nhất, chúng tôi hợp nhất vùng báo động nhỏ hơn thành vùng lớn hơn. Đây là kỹ thuật từ nhỏ đến lớn, do đó mục cảnh báo chỉ được di chuyển (O(\log n)) lần. 

Cuối cùng, mọi cạnh thực sự có thể sử dụng được sẽ được đặt vào một đống tối thiểu toàn cục được khóa bởi chỉ mục ban đầu của nó. Chỉ số nhỏ nhất luôn được xử lý đầu tiên. Một mục nhập có thể trở nên cũ vì một cạnh được chọn khác có thể đã tham gia các điểm cuối của nó, vì vậy trước khi sử dụng một ứng cử viên toàn cầu, chúng tôi kiểm tra lại các đại diện DSU của nó. 

Đây là kết nối trung tâm giữa lực lượng vũ phu và giải pháp tối ưu. Lực lượng vũ phu hoạt động vì mọi cạnh đều được kiểm tra chính xác khi điều kiện của nó có thể thay đổi, nhưng nó kiểm tra quá nhiều cạnh. Việc quan sát giảm một nửa cho phép chúng tôi chỉ lên lịch những thời điểm khi một cạnh có thể trở nên phù hợp, trong khi DSU và từ nhỏ đến lớn làm cho các thay đổi thành phần trở nên hiệu quả. Ý tưởng đáng báo động tương tự là kỹ thuật tiêu chuẩn được sử dụng cho vấn đề này trong bài xã luận của cuộc thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm)) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(m\log C\log n\log m)) để triển khai heap nhị phân trực tiếp | (O(n+m\log C)) | Cách tiếp cận được chấp nhận | 

Ở đây (C) là ngưỡng tối đa, nhiều nhất là (10^6). Với một đống có thể kết hợp được, hệ số hợp nhất đống có thể được giảm xuống, đây là dạng thường được trích dẫn cho giải pháp dự định. trực tiếp`heapq`Việc triển khai bên dưới giữ cho cấu trúc dữ liệu đơn giản và tuân theo cùng một chiến lược từ nhỏ đến lớn. 

## Hướng dẫn thuật toán 

1. Xây dựng DSU chứa một đỉnh cho mỗi thành phần. Lưu trữ tổng trọng lượng hiện tại của từng thành phần tại đại diện của nó. Vì tất cả các ngưỡng đều lớn nhất là (10^6), nên trọng số thành phần có thể được giới hạn một cách an toàn ở mức (10^6): khi một thành phần đạt đến giá trị đó, chỉ riêng thành phần đó đã đủ để đáp ứng mọi ngưỡng có thể có. 
2. Với mỗi cạnh (i=(u,v,s)), hãy tìm các thành phần hiện tại của (u) và (v). Ban đầu chúng chỉ là các đỉnh riêng lẻ. Nếu tổng trọng số của chúng đã bằng ít nhất (s), hãy đặt (i) vào vùng heap ứng viên toàn cục. Nếu không hãy để 

[ 
d=s-w_u-w_v 
] 

và đặt cảnh báo trên từng thành phần điểm cuối tại 

[ 
w_u+\left\lceil\frac d2\right\rceil 
\quad\text{và}\quad 
w_v+\left\lceil\frac d2\right\rceil. 
] 

Hai cảnh báo đảm bảo rằng cạnh sẽ được xem xét lại trước khi nó có thể có hiệu lực. 

1. Liên tục loại bỏ chỉ số cạnh nhỏ nhất khỏi đống ứng cử viên toàn cầu. Tìm đại diện DSU hiện tại của các điểm cuối của nó. Nếu chúng bằng nhau, thì ứng cử viên đó đã cũ vì một số cạnh trước đó đã kết nối các thành phần đó, vì vậy hãy loại bỏ nó. Nếu không, cạnh chính xác là cạnh nhỏ nhất hiện đủ điều kiện, vì vậy hãy thêm chỉ mục của nó vào câu trả lời. 
2. Hợp nhất hai thành phần điểm cuối. Sử dụng từ nhỏ đến lớn trên đống cảnh báo, giữ vùng heap lớn hơn làm đích đến và di chuyển mọi cảnh báo từ vùng heap nhỏ hơn vào đó. Cộng các trọng số thành phần lại với nhau và giới hạn kết quả ở (10^6). 
3. Sau khi hợp nhất, hãy kiểm tra cảnh báo nhỏ nhất trong vùng nhớ thành phần mới. Nếu ngưỡng của nó lớn hơn trọng lượng thành phần mới thì không có cảnh báo nào ở dưới nó có thể kích hoạt, vì vậy hãy dừng lại. Nếu đạt đến ngưỡng của nó, hãy loại bỏ nó và xem xét lại cạnh đó. 
4. Khi xem xét lại cảnh báo, hãy tìm các thành phần hiện tại của điểm cuối của cạnh. Nếu chúng đã bằng nhau thì cảnh báo sẽ cũ và có thể bị loại bỏ. Nếu không hãy tính tổng trọng lượng hiện tại của chúng. Nếu nó đạt đến ngưỡng cạnh, hãy đặt cạnh đó vào đống ứng cử viên toàn cầu. Nếu không, hãy tính lượng còn lại mới và tạo hai cảnh báo nửa ngưỡng mới. 
5. Tiếp tục xử lý đống ứng viên toàn cầu cho đến khi nó trống. Khi đó không có cạnh nào có các thành phần điểm cuối khác nhau thỏa mãn ngưỡng của nó nên quá trình ban đầu cũng phải dừng lại. 

### Tại sao nó hoạt động 

Đối với mỗi cạnh có điểm cuối vẫn nằm trong các thành phần khác nhau, hãy duy trì bất biến rằng ngưỡng cảnh báo của nó trên mỗi điểm cuối là trọng số thành phần hiện tại cộng với một nửa mức thâm hụt hiện tại còn lại, được làm tròn lên. Nếu không có cảnh báo nào được kích hoạt thì cả hai thành phần đều tăng ít hơn một nửa mức thâm hụt đó, do đó mức tăng tổng hợp của chúng nhỏ hơn toàn bộ mức thâm hụt và cạnh chưa thể thỏa mãn điều kiện của nó. Nếu cảnh báo kích hoạt, việc kiểm tra tình trạng thực tế sẽ xác định liệu cạnh hiện có đủ điều kiện hay không. Nếu không, mức thâm hụt còn lại chỉ bằng một nửa so với trước đây, do đó, việc thay thế các cảnh báo sẽ giữ nguyên tính bất biến và chỉ đảm bảo tính toán lại nhiều lần theo logarit. 

Đống ứng cử viên toàn cầu chứa mọi cạnh đã đủ điều kiện, có thể cùng với các cạnh cũ mà điểm cuối của chúng sau đó đã được nối. Vì trọng số thành phần không bao giờ giảm nên một cạnh đủ điều kiện vẫn đủ điều kiện cho đến khi điểm cuối của nó bằng nhau. Do đó, sau khi loại bỏ các ứng viên cũ, phần tử nhỏ nhất của heap toàn cục chính xác là cạnh hợp lệ nhỏ nhất mà quy trình ban đầu yêu cầu. Mỗi phép kết hợp được thực hiện trên hai thành phần khác nhau, vì vậy chuỗi được tạo ra chính xác là chuỗi từ câu lệnh. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

MAX_S = 1_000_000

def solve():
    n, m = map(int, input().split())
    weight = list(map(int, input().split()))

    parent = list(range(n))
    size = [1] * n

    # Component weights. They are capped at MAX_S because
    # every threshold is at most MAX_S.
    comp = weight[:]

    U = [0] * m
    V = [0] * m
    S = [0] * m

    # alarms[root] contains (absolute_threshold, edge_id)
    alarms = [[] for _ in range(n)]

    # Edges that are currently eligible, ordered by original index.
    ready = []

    answer = []

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def schedule(e):
        u = find(U[e])
        v = find(V[e])

        if u == v:
            return

        remaining = S[e] - comp[u] - comp[v]

        if remaining <= 0:
            heapq.heappush(ready, e)
            return

        half = (remaining + 1) // 2
        heapq.heappush(alarms[u], (comp[u] + half, e))
        heapq.heappush(alarms[v], (comp[v] + half, e))

    def merge_components(a, b):
        a = find(a)
        b = find(b)

        if a == b:
            return a

        # Keep the larger alarm heap.
        if len(alarms[a]) > len(alarms[b]):
            a, b = b, a

        parent[a] = b
        size[b] += size[a]
        comp[b] = min(MAX_S, comp[a] + comp[b])

        small = alarms[a]
        large = alarms[b]

        # Move the smaller heap into the larger heap.
        while small:
            threshold, e = heapq.heappop(small)

            # The old alarm may already be obsolete because its endpoints
            # became connected.
            x = find(U[e])
            y = find(V[e])

            if x == y:
                continue

            if threshold <= comp[b]:
                schedule(e)
            else:
                heapq.heappush(large, (threshold, e))

        # Process every alarm that has become active because the component
        # weight increased.
        while large and large[0][0] <= comp[b]:
            threshold, e = heapq.heappop(large)

            x = find(U[e])
            y = find(V[e])

            if x == y:
                continue

            schedule(e)

        return b

    # Build the initial alarm structure.
    for e in range(m):
        u, v, s = map(int, input().split())
        u -= 1
        v -= 1

        U[e] = u
        V[e] = v
        S[e] = s

        schedule(e)

    while ready:
        e = heapq.heappop(ready)

        u = find(U[e])
        v = find(V[e])

        if u == v:
            continue

        # The edge is eligible and has the smallest index among all
        # candidates currently known to be eligible.
        answer.append(e + 1)

        merge_components(u, v)

    print(len(answer))
    print(*answer)

if __name__ == "__main__":
    solve()
```Mảng DSU có ý nghĩa thông thường của chúng.`parent`xác định các thành phần,`size`hỗ trợ heuristic liên minh, và`comp`lưu trữ tổng trọng lượng hiện tại cho mỗi đại diện. Việc nén đường dẫn giúp việc tra cứu đại diện lặp đi lặp lại có hiệu quả với thời gian khấu hao không đổi.`alarms[root]`là hàng đợi ưu tiên cục bộ cho một thành phần. Một mục`(threshold, e)`nói rằng cạnh đó`e`nên được xem xét lại ngay khi thành phần này đạt đến`threshold`. Ngưỡng này là tuyệt đối chứ không phải là mức tăng tương đối, điều này làm cho việc hợp nhất hai vùng heap trở nên đơn giản. 

các`schedule`chức năng là trung tâm của thủ thuật giảm một nửa. Đầu tiên nó kiểm tra xem cạnh đã hợp lệ hay chưa. Nếu đúng như vậy, cạnh sẽ đi vào`ready`. Nếu không, phần thâm hụt còn lại sẽ được chia đều cho hai thành phần điểm cuối. biểu thức`(remaining + 1) // 2`là việc thực hiện số nguyên của mức trần yêu cầu. 

Hoạt động hợp nhất có chủ ý chọn vùng cảnh báo nhỏ hơn làm nguồn. Mỗi mục nhập được di chuyển sẽ tăng gấp đôi kích thước của cấu trúc dữ liệu chứa nó, do đó cảnh báo chỉ có thể di chuyển theo logarit nhiều lần. Sau khi trọng số thành phần thay đổi, các cảnh báo nhỏ nhất sẽ được kiểm tra nhiều lần cho đến cảnh báo đầu tiên có ngưỡng vẫn còn quá lớn. 

Thứ tự thực hiện trong`merge_components`vấn đề. DSU cha được thay đổi và trọng số thành phần mới được tính toán trước khi các cảnh báo được xem xét lại, bởi vì mọi cảnh báo phải được đánh giá theo trạng thái thành phần mới. Khi`schedule`được gọi sau đó, nó sử dụng`find`một lần nữa, do đó, một cạnh có điểm cuối vô tình được hợp nhất trong quá trình hoạt động này sẽ bị loại bỏ một cách an toàn. 

Tất cả số học được thực hiện bằng số nguyên Python, do đó không có vấn đề tràn. Trong C++, số nguyên 64 bit cũng là một lựa chọn an toàn, mặc dù giới hạn ngưỡng tạo ra các giá trị 32 bit đủ cho tổng thành phần được thuật toán sử dụng. 

Các ràng buộc ban đầu của cuộc thi được thiết kế xoay quanh kỹ thuật heap từ nhỏ đến lớn này. Giải pháp được công bố sử dụng cùng cảnh báo nửa thâm hụt và hàng đợi ưu tiên ứng viên toàn cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trọng số thành phần ban đầu là (1,4,3,4,0). Các cạnh 2, 3, 4 và 5 đủ điều kiện ngay lập tức, trong khi cạnh 1 có tổng trọng số (4+0=4<5). 

| Hoạt động | Ứng viên toàn cầu | Cạnh đã chọn | Thay đổi thành phần | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2, 3, 4, 5 | 2 | (1) và (3) hợp nhất, trọng số (4) | đáp án = 2 | 
| Sau khi hợp nhất | 3, 4, 5 | 3 | (4) và (0) hợp nhất, trọng số (4) | đáp án = 2, 3 | 
| Sau khi hợp nhất | 4, 5 | 4 | trọng số thành phần (4) hợp nhất với đỉnh 4 của trọng số (4) | đáp án = 2, 3, 4 | 
| Sau khi hợp nhất | 1, 5 | 1 | cạnh 1 hiện nối thành phần trọng số 8 với đỉnh 5, vì vậy nó đủ điều kiện | đáp án = 2, 3, 1, 4 | 

Phần thú vị là sự chuyển đổi cuối cùng. Edge 1 ban đầu ở dưới ngưỡng của nó, nhưng thành phần điểm cuối của nó đã tăng từ trọng số 4 lên trọng số 8. Cơ chế cảnh báo sẽ phát hiện sự thay đổi đó mà không cần quét lại tất cả các cạnh. Hàng đợi toàn cầu vẫn quyết định cạnh đủ điều kiện nào có chỉ số nhỏ nhất, tạo ra thứ tự bắt buộc (2,3,1,4). Mẫu chính thức có chính xác đầu ra này. 

### Mẫu 2 

Trọng số ban đầu là (3,2,2). Các cạnh 1, 2 và 4 yêu cầu tổng trọng số là 6 giữa các đỉnh 1 và 2, vì vậy ban đầu không có cạnh nào hợp lệ. Edge 3 có ngưỡng 3 và có hiệu lực ngay lập tức. Cạnh 5 yêu cầu 6 điểm giữa đỉnh 2 và 3 và ban đầu cũng ngắn. 

| Hoạt động | Ứng viên toàn cầu | Cạnh đã chọn | Trọng lượng thành phần liên quan | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 3 | (3+2=5) | hợp nhất 1 và 2 | 
| Sau cạnh 3 | 5 | 5 | (5+2=7) | hợp nhất 1,2 với 3 | 
| Cuối cùng | trống | không | tất cả các đỉnh được kết nối | dừng lại | 

Khi cạnh 3 nối đỉnh 1 và đỉnh 2, trọng số thành phần của chúng trở thành 5. Điều này vượt qua ngưỡng cảnh báo cho cạnh 5 và điều kiện thực tế của nó hiện là (5+2\ge6). Do đó, Edge 5 được chèn vào vùng ứng cử viên toàn cầu và được chọn tiếp theo. Đầu ra là (3,5), khớp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m\log C\log n\log m)) | Mỗi cạnh được xem xét lại (O(\log C)) lần, các mục cảnh báo di chuyển (O(\log n)) lần khi hợp nhất từ ​​nhỏ đến lớn và chi phí hoạt động heap nhị phân (O(\log m)). | 
| Không gian | (O(n+m\log C)) | DSU và mảng biên sử dụng (O(n+m)), trong khi tổng số mục nhập cảnh báo được tạo là (O(m\log C)). | 

Ngưỡng giới hạn (C\le10^6) làm cho hệ số giảm một nửa trở nên nhỏ, với tối đa khoảng 20 mức giảm có ý nghĩa trên mỗi cạnh. Cải tiến quan trọng đối với lực lượng vũ phu là một cạnh không còn được kiểm tra sau mỗi lần hợp nhất thành phần. Giải pháp dự định thường được tóm tắt dưới dạng hợp nhất từ ​​nhỏ đến lớn kết hợp với cập nhật cảnh báo (O(\log C)) trên mỗi cạnh. 

Đối với giới hạn 256 MiB ban đầu, việc triển khai C++ là lựa chọn an toàn hơn trong cuộc thi vì các mục heap Python có chi phí đối tượng đáng kể. Việc triển khai Python ở trên là cách triển khai trung thực của thuật toán, nhưng phương pháp tiệm cận, thay vì biểu diễn đối tượng Python, mới là những gì mà các ràng buộc cuộc thi ban đầu được thiết kế xung quanh. 

## Trường hợp thử nghiệm```python
# The test harness assumes the solve() function from the solution above
# is already defined.

import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    output = io.StringIO()
    sys.stdout = output

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output.getvalue()

# Provided sample 1
assert run("""\
5 5
1 4 3 4 0
4 5 5
3 1 1
2 5 2
4 3 1
4 1 4
""") == """\
4
2 3 1 4
""", "sample 1"

# Provided sample 2
assert run("""\
3 5
3 2 2
1 2 6
1 2 6
1 2 3
1 2 6
2 3 6
""") == """\
2
3 5
""", "sample 2"

# Minimum valid instance, threshold zero.
assert run("""\
2 1
0 0
1 2 0
""") == """\
1
1
""", "minimum valid instance"

# Exact equality at the threshold.
assert run("""\
2 1
2 3
1 2 5
""") == """\
1
1
""", "equality boundary"

# An initially impossible edge becomes valid after another edge is chosen.
assert run("""\
3 2
1 1 1
1 2 3
2 3 2
""") == """\
2
2 1
""", "dynamic eligibility and index ordering"

# Zero weights with a positive threshold: no edge can ever become valid.
assert run("""\
2 1
0 0
1 2 1
""") == """\
0

""", "never eligible"

# Maximum-size structural stress test.
# The first 299999 edges form a chain and are all immediately valid.
n = 300000
edges = [f"{i} {i + 1} 0" for i in range(1, n)]
edges.append(f"1 {n} 0")

large_input = (
    f"{n} {n}\n"
    + " ".join(["0"] * n)
    + "\n"
    + "\n".join(edges)
    + "\n"
)

expected_large = (
    f"{n - 1}\n"
    + " ".join(map(str, range(1, n)))
    + "\n"
)

assert run(large_input) == expected_large, "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, trọng lượng`0 0`, ngưỡng`0`|`1 / 1`| Biểu đồ hợp lệ tối thiểu và ngưỡng bằng 0 | 
|`2 1`, trọng lượng`2 3`, ngưỡng`5`|`1 / 1`| Bình đẳng chính xác với ngưỡng | 
|`3 2`, trọng lượng`1 1 1`, cạnh`(1,2,3)`Và`(2,3,2)`|`2 / 2 1`| Một cạnh trở nên đủ điều kiện sau khi một cạnh khác được chọn | 
|`2 1`, trọng lượng`0 0`, ngưỡng`1`|`0 / blank`| Ngưỡng dương vĩnh viễn không thể | 
|`300000`đỉnh và`300000`cạnh chuỗi không ngưỡng |`299999 / 1..299999`| Kích thước đầu vào tối đa, hợp nhất DSU và cạnh cuối cùng cũ | 

## Vỏ cạnh 

Trường hợp ngưỡng 0 được xử lý trong`schedule`. Vì```
2 2
0 0
1 2 0
1 2 0
```cạnh 1 ngay lập tức được chèn vào vùng ứng cử viên toàn cầu. Nó được chọn và nối hai đỉnh. Khi cạnh 2 cuối cùng bị xóa khỏi vùng toàn cục,`find(1)`Và`find(2)`bằng nhau nên bị loại bỏ vì cũ. Đầu ra là`1`theo sau là chỉ số cạnh`1`. 

Bình đẳng chính xác được xử lý bởi`remaining <= 0`Bài kiểm tra. Với```
2 1
2 3
1 2 5
```tổng trọng số thành phần ban đầu bằng chính xác 5. Cạnh được chèn vào`ready`ngay lập tức và được chọn, đưa ra kết quả`1`và sau đó`1`. 

Trường hợp thứ tự động```
3 2
1 1 1
1 2 3
2 3 2
```bắt đầu chỉ với cạnh 2 đủ điều kiện. Sau khi chọn nó, đỉnh 2 và 3 tạo thành thành phần có trọng số 2. Cạnh 1 hiện có thành phần điểm cuối có trọng số 1 và 2, do đó đạt đến ngưỡng 3. Nó được chèn vào đống ứng viên toàn cầu và được chọn tiếp theo. Đầu ra là`2 1`. Điều này chứng tỏ tại sao thuật toán phải duy trì các cảnh báo trong tương lai thay vì chỉ quét các cạnh một lần. 

Đối với một ngưỡng dương không thể có,```
2 1
0 0
1 2 1
```mức thâm hụt còn lại là 1, do đó cả hai cảnh báo đều được đặt cao hơn trọng lượng thành phần hiện tại của chúng một đơn vị. Không có thành phần nào phát triển nên cả cảnh báo đều không kích hoạt và vùng ứng cử viên toàn cầu vẫn trống. Quá trình dừng ngay lập tức, tạo ra các cạnh được chọn bằng 0 và dòng thứ hai trống. 

Thử nghiệm chuỗi kích thước tối đa sử dụng ngưỡng bằng 0, do đó mọi cạnh ban đầu đều đủ điều kiện. Đống toàn cục liên tục chọn chỉ số nhỏ nhất còn lại. Cạnh đầu tiên (299999) kết nối chuỗi, trong khi cạnh cuối cùng nối các đỉnh đã có trong cùng thành phần và bị loại bỏ. Do đó, thuật toán tạo ra chính xác (299999) cạnh được chọn mà không cần phải kiểm tra tất cả (300000) cạnh sau mỗi lần kết hợp.
