---
title: "CF 102346I - Liên hành tinh"
description: "Chúng ta có một đồ thị có trọng số vô hướng có các đỉnh là các hành tinh và các cạnh là các đường di chuyển trực tiếp. Mọi hành tinh đều có nhiệt độ."
date: "2026-08-13T01:35:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 487
verified: true
draft: false
---

[CF 102346I - Liên hành tinh](https://codeforces.com/problemset/problem/102346/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có trọng số vô hướng có các đỉnh là các hành tinh và các cạnh là các đường di chuyển trực tiếp. Mọi hành tinh đều có nhiệt độ. Một khách hàng yêu cầu con đường ngắn nhất từ ​​hành tinh (A) đến hành tinh (B), nhưng đặt ra hạn chế đối với mọi hành tinh trung gian: tùy theo yêu cầu, hành tinh trung gian phải thuộc mức nhiệt độ lạnh nhất (K) hoặc nóng nhất (K). 

Nguồn và đích được miễn giới hạn nhiệt độ. Chỉ các đỉnh nằm giữa chúng trên tuyến mới phải thỏa mãn điều đó. Câu trả lời là tổng chiều dài cạnh tối thiểu của một tuyến đường hợp lệ hoặc (-1) khi không tồn tại tuyến đường hợp lệ. 

Cụm từ “nhiệt độ lạnh nhất (K)” được hiểu rõ nhất qua ngưỡng nhiệt độ do nhiệt độ nhỏ nhất thứ (K) gây ra. Nhiệt độ bằng nhau phải được coi là một nhóm ranh giới. Ví dụ: nếu nhiệt độ là (5,10,10,20), hai nhiệt độ lạnh nhất là (5) và (10), do đó cả hai hành tinh có nhiệt độ (10) đều được phép. Điều tương tự cũng áp dụng đối xứng với mặt nóng nhất. Mẫu thứ hai phụ thuộc chính xác vào hành vi này: với (K=2) ở phía nóng, nhiệt độ (20) và (10) được cho phép, do đó cả ba hành tinh ở nhiệt độ (10) đều có thể được sử dụng. 

Biểu đồ có tối đa 400 đỉnh, trong khi có thể có tới (N(N-1)/2), khoảng 80.000 cạnh và tối đa 100.000 truy vấn. Số lượng đỉnh nhỏ gợi ý rõ ràng về kỹ thuật đường đi ngắn nhất tất cả các cặp, trong khi số lượng truy vấn khổng lồ loại trừ việc chạy thuật toán đường đi ngắn nhất một cách độc lập cho mọi yêu cầu. Tuyên bố lưu trữ chính thức đưa ra giới hạn thời gian 1 giây và giới hạn bộ nhớ 1024 MB, do đó, việc triển khai dự định là một giải pháp kiểu Floyd-Warshall nhỏ gọn (O(N^3)). 

Một số chi tiết có thể khiến việc triển khai có vẻ hợp lý trả về câu trả lời sai. 

Đầu tiên, điểm cuối không bị hạn chế. Coi như:```
2 1
0 10
1 2 7
1
1 2 1 0
```Câu trả lời là`7`. Hành tinh 1 lạnh nhất nhưng hành tinh 2 không cần phải thỏa mãn hạn chế vì nó là đích đến. Giải pháp đơn giản là xóa mọi hành tinh nằm ngoài nhiệt độ cho phép sẽ xóa hành tinh 2 và quay lại không chính xác.`-1`. 

Thứ hai, nhiệt độ bằng nhau phải được xử lý theo nhóm. Coi như:```
4 3
1 2 2 3
1 2 1
2 3 1
3 4 1
2
1 4 2 0
1 4 2 1
```Đối với yêu cầu lạnh, nhiệt độ lạnh thứ hai là`2`, vì vậy cả hai hành tinh 2 và 3 đều được phép. Tuyến đường có chiều dài`3`. Đối với yêu cầu nóng, nhiệt độ nóng thứ hai cũng là`2`, vì vậy lộ trình tương tự là hợp lệ và câu trả lời lại là`3`. Việc triển khai chọn chính xác hai chỉ số hành tinh thay vì sử dụng ngưỡng nhiệt độ có thể vô tình chỉ cho phép một trong hai hành tinh có nhiệt độ`2`. 

Thứ ba, cạnh trực tiếp phải vẫn có sẵn ngay cả khi cả hai điểm cuối đều không thuộc nhiệt độ cho phép đã đặt. Ví dụ:```
3 1
0 50 100
1 3 9
1
1 3 1 0
```Câu trả lời là`9`, bởi vì tuyến đường này không có hành tinh trung gian nào cả. Một giải pháp yêu cầu mọi đỉnh của tuyến đường nằm trong tập hợp được phép sẽ từ chối nó một cách không chính xác. 

Cuối cùng, các biểu đồ bị ngắt kết nối phải được giữ nguyên trong suốt quá trình tính toán. Ví dụ:```
2 0
0 1
1
1 2 1 0
```Câu trả lời là`-1`, không phải là một giá trị hữu hạn lớn. Do đó, việc triển khai cần một giá trị vô cùng và phải chuyển một khoảng cách không thể tiếp cận trở lại`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng khách hàng một cách độc lập. Đối với một truy vấn, chúng ta có thể xác định ngưỡng nhiệt độ cho phép, bỏ qua các hành tinh trung gian bị cấm và chạy thuật toán Dijkstra từ (A) đến (B). Vì tất cả độ dài tuyến đường đều dương nên Dijkstra đúng cho từng yêu cầu riêng lẻ. 

Vấn đề là số lượng yêu cầu. Trong trường hợp xấu nhất có khoảng 80.000 cạnh và 100.000 truy vấn. Ngay cả khi sử dụng triển khai heap tốt, việc xử lý mọi yêu cầu một cách độc lập vẫn có chi phí (O(Q(R+N)\log N)). Với giới hạn tối đa, điều đó có nghĩa là theo thứ tự (10^5 \cdot 8\cdot10^4), khoảng hàng tỷ phép toán liên quan đến cạnh trước khi tính đến hệ số logarit. Việc tính toán lại giải pháp tất cả các cặp cho mỗi truy vấn thậm chí còn tệ hơn, ở mức (O(QN^3)), khoảng (6.4\cdot10^{11}) mức giãn Floyd-Warshall. 

Cấu trúc giúp giải pháp nhanh hơn có thể là tập hợp được phép được lồng vào nhau. Khi chúng ta chuyển từ nhiệt độ lạnh nhất sang nhiệt độ nóng hơn, chúng ta chỉ thêm các hành tinh. Đối với yêu cầu nguội, các đỉnh trung gian được phép là tất cả các hành tinh có nhiệt độ cao nhất là một ngưỡng nào đó. Do đó, một yêu cầu có (K) lớn hơn sẽ có tập hợp các đỉnh sẵn có cho yêu cầu có (K) nhỏ hơn. Sự lồng ghép tương tự tồn tại khi xử lý nhiệt độ từ nóng nhất đến lạnh nhất. 

Đây chính xác là cài đặt mà Floyd-Warshall có thể được xem như một chương trình động trên tập hợp các đỉnh trung gian được phép. Giả sử nhóm nhiệt độ (k) đầu tiên đã được kích hoạt. Cho phép`dist[i][j]`là khoảng cách ngắn nhất từ ​​(i) đến (j) có các đỉnh bên trong đều thuộc về các nhóm được kích hoạt đó. Khi một hành tinh mới (v) được cho phép, mọi đường đi ngắn nhất mới sẽ không sử dụng (v) hoặc có thể được chia thành một đường đi từ (i) đến (v), theo sau là đường đi từ (v) đến (j). Sự tái diễn là sự thư giãn Floyd-Warshall thông thường: 

[ 
dist[i][j] = \min(dist[i][j], dist[i][v] + dist[v][j]). 
] 

Chúng tôi chạy quá trình này một lần từ lạnh đến nóng và một lần từ nóng sang lạnh. Các truy vấn được lưu trữ theo số lượng nhóm nhiệt độ mà ngưỡng của chúng kích hoạt, do đó, mỗi truy vấn được trả lời chính xác khi đạt đến trạng thái yêu cầu của ma trận khoảng cách. 

Vấn đề nhiệt độ bằng nhau đương nhiên phù hợp với công thức này. Chúng tôi không trả lời các truy vấn giữa chừng trong nhóm nhiệt độ. Mọi hành tinh có cùng nhiệt độ sẽ được kích hoạt trước tiên và chỉ sau đó các truy vấn có ngưỡng đạt đến nhiệt độ đó mới được trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với Dijkstra cho mỗi truy vấn | (O(QR\log N)) trong trường hợp xấu nhất dày đặc | (O(N+R)) | Quá chậm | 
| Nhóm tối ưu Floyd-Warshall | (O(N^3+Q)) | (O(N^2+Q)) | Đã chấp nhận | 

Hai đường chuyền Floyd đóng góp (2N^3), vẫn là (O(N^3)). Với (N\le400), đây là nghiệm tiệm cận dự kiến. Triển khai được biên dịch là lựa chọn an toàn nhất trong giới hạn 1 giây được lưu trữ. Việc triển khai Python bên dưới sử dụng các biến cục bộ và ma trận tại chỗ để giữ hệ số không đổi ở mức thấp nhất có thể trong thực tế. 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và xây dựng ma trận khoảng cách (N\times N). Đặt đường chéo về 0, đặt mọi tuyến đường trực tiếp vào ma trận và để lại các tuyến đường không tồn tại ở vô cùng. Đồ thị là vô hướng, do đó cạnh giữa (u) và (v) khởi tạo cả hai`dist[u][v]`Và`dist[v][u]`. 
2. Sắp xếp tất cả nhiệt độ hành tinh và xây dựng các mức nhiệt độ riêng biệt theo thứ tự tăng dần. Các hành tinh có cùng nhiệt độ được xếp vào cùng một nhóm vì ngưỡng nhiệt độ đó phải cho phép tất cả chúng. 
3. Với mọi giá trị có thể có của (K), hãy xác định nhóm nhiệt độ nào đạt đến nhiệt độ nhỏ nhất (K). Nếu nhiệt độ được sắp xếp tại vị trí (K-1) là (x), mọi hành tinh có nhiệt độ tối đa (x) đều được phép. Lưu trữ số lượng nhóm nhiệt độ lạnh tương ứng. 
4. Thực hiện tiền xử lý đối xứng cho các yêu cầu nóng. Nhiệt độ lớn nhất thứ (K) xác định ngưỡng trên, vì vậy mọi hành tinh có nhiệt độ ít nhất giá trị đó đều được phép. Lưu trữ số nhóm nhiệt độ nóng tương ứng. 
5. Đặt mọi truy vấn vào một nhóm theo số nhóm nhiệt độ nóng hoặc lạnh hiệu quả của nó. Một truy vấn không cần phải được xử lý ngay lập tức vì tất cả các truy vấn yêu cầu cùng số nhóm đều sử dụng cùng một ma trận khoảng cách. 
6. Đối với truy vấn lạnh, xử lý các nhóm nhiệt độ từ lạnh nhất đến nóng nhất. Khi đã đến được một nhóm, hãy chạy chương trình thư giãn Floyd-Warshall một lần cho mọi hành tinh trong nhóm đó. Việc thêm tất cả các hành tinh vào nhóm trước khi trả lời các truy vấn là cần thiết vì các hành tinh có nhiệt độ bằng nhau đều được cho phép theo cùng một ngưỡng. 
7. Sau khi toàn bộ nhóm đã được kích hoạt, hãy trả lời mọi câu hỏi chưa có sẵn trong nhóm của nhóm đó bằng cách đọc`dist[A][B]`. Nếu nó vẫn là vô cùng, hãy lưu trữ`-1`. 
8. Khởi tạo lại ma trận khoảng cách về biểu đồ ban đầu và xử lý các nhóm nhiệt độ từ nóng nhất đến lạnh nhất. Áp dụng chính xác cách thư giãn Floyd-Warshall nhưng thay vào đó hãy trả lời các nhóm truy vấn nóng. 
9. Cuối cùng, in các câu trả lời đã lưu theo thứ tự truy vấn ban đầu. Hai đường chuyền không bao giờ cần cùng tồn tại dưới dạng ma trận khoảng cách, do đó, mỗi lần chỉ có một ma trận (N\times N) được giữ lại. 

### Tại sao nó hoạt động 

Sau khi một số nhóm nhiệt độ cụ thể được kích hoạt, bất biến Floyd-Warshall là`dist[i][j]`bằng đường đi ngắn nhất từ ​​(i) đến (j) có các hành tinh bên trong thuộc nhóm được kích hoạt. Khi một hành tinh mới (v) được thêm vào, mọi tuyến đường sử dụng (v) bên trong có thể được phân tách tại (v), do đó, việc nới lỏng thông qua (v) sẽ xem xét mọi tuyến đường mới có thể. Các tuyến không sử dụng (v) không thay đổi. Áp dụng điều này cho mọi hành tinh trong một nhóm nhiệt độ sẽ đưa ra những đường đi ngắn nhất chính xác có nhiệt độ trung gian thỏa mãn ngưỡng đó. Vì một truy vấn chỉ được trả lời sau khi toàn bộ nhóm nhiệt độ biên của nó được kích hoạt nên tất cả các hành tinh bị ràng buộc ở ngưỡng đều khả dụng. Các điểm cuối không bao giờ bị xóa khỏi ma trận nên chúng vẫn có thể sử dụng được bất kể nhiệt độ của chúng như thế nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**15

def process_orientation(n, groups, buckets, dist, answers):
    rng = range(n)

    for level, vertices in enumerate(groups, 1):
        for k in vertices:
            dk = dist[k]

            for i in rng:
                di = dist[i]
                dik = di[k]

                if dik >= INF:
                    continue

                for j in rng:
                    cand = dik + dk[j]
                    if cand < di[j]:
                        di[j] = cand

        for qi, a, b in buckets[level]:
            d = dist[a][b]
            answers[qi] = -1 if d >= INF else d

def solve():
    n, r = map(int, input().split())
    temp = list(map(int, input().split()))

    edges = []
    for _ in range(r):
        x, y, d = map(int, input().split())
        x -= 1
        y -= 1
        edges.append((x, y, d))

    q = int(input())

    queries = []
    for qi in range(q):
        a, b, k, typ = map(int, input().split())
        queries.append((a - 1, b - 1, k, typ))

    sorted_temp = sorted(temp)

    unique_temp = []
    for x in sorted_temp:
        if not unique_temp or unique_temp[-1] != x:
            unique_temp.append(x)

    groups_asc = []
    current = []
    last_temp = None

    for v in sorted(range(n), key=lambda x: temp[x]):
        if last_temp is None or temp[v] == last_temp:
            current.append(v)
        else:
            groups_asc.append(current)
            current = [v]
        last_temp = temp[v]

    if current:
        groups_asc.append(current)

    groups_desc = list(reversed(groups_asc))
    group_count = len(groups_asc)

    # cold_level[k] = number of cold temperature groups allowed
    # by the K-th smallest temperature.
    cold_level = [0] * (n + 1)

    # hot_level[k] = number of hot temperature groups allowed
    # by the K-th largest temperature.
    hot_level = [0] * (n + 1)

    # Map each planet temperature to its group from the cold side.
    temp_to_cold_group = {}
    for idx, x in enumerate(unique_temp):
        temp_to_cold_group[x] = idx + 1

    # Map each planet temperature to its group from the hot side.
    temp_to_hot_group = {}
    for idx, x in enumerate(unique_temp):
        temp_to_hot_group[x] = group_count - idx

    for k in range(1, n + 1):
        cold_level[k] = temp_to_cold_group[sorted_temp[k - 1]]
        hot_level[k] = temp_to_hot_group[sorted_temp[n - k]]

    cold_buckets = [[] for _ in range(group_count + 1)]
    hot_buckets = [[] for _ in range(group_count + 1)]

    for qi, (a, b, k, typ) in enumerate(queries):
        if typ == 0:
            level = cold_level[k]
            cold_buckets[level].append((qi, a, b))
        else:
            level = hot_level[k]
            hot_buckets[level].append((qi, a, b))

    answers = [-1] * q

    def initial_dist():
        dist = [[INF] * n for _ in range(n)]

        for i in range(n):
            dist[i][i] = 0

        for x, y, d in edges:
            if d < dist[x][y]:
                dist[x][y] = d
                dist[y][x] = d

        return dist

    if any(cold_buckets):
        dist = initial_dist()
        process_orientation(
            n,
            groups_asc,
            cold_buckets,
            dist,
            answers
        )

    if any(hot_buckets):
        dist = initial_dist()
        process_orientation(
            n,
            groups_desc,
            hot_buckets,
            dist,
            answers
        )

    sys.stdout.write("\n".join(map(str, answers)))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ xây dựng ma trận biểu đồ gốc. Bởi vì mọi độ dài tuyến đường đều dương và (N\le400), giá trị vô cực của (10^{15}) lớn hơn một cách thoải mái so với mọi tuyến đường đơn giản có thể có, có độ dài tối đa là khoảng (399\cdot1000). 

Quá trình tiền xử lý nhiệt độ là phần tinh tế nhất.`unique_temp`chứa mỗi nhiệt độ riêng biệt một lần, trong khi`groups_asc`chứa các chỉ số hành tinh thực tế thuộc về từng nhiệt độ. Hai cấu trúc phục vụ các mục đích khác nhau. Các giá trị duy nhất cho chúng tôi biết truy vấn đạt đến ngưỡng nào, trong khi các nhóm cho Floyd-Warshall biết đỉnh nào phải được kích hoạt ở ngưỡng đó. 

Các mảng`cold_level`Và`hot_level`chuyển đổi tham số truy vấn ban đầu (K) thành số nhóm. Đối với một truy vấn lạnh lùng,`sorted_temp[k - 1]`là nhiệt độ nhỏ nhất thứ (K). Mọi hành tinh có nhiệt độ đó phải được cho phép, vì vậy`temp_to_cold_group`đưa ra ranh giới nhóm hoàn chỉnh. Tính toán nóng sử dụng`sorted_temp[n - k]`đối với nhiệt độ lớn nhất (K). 

Chuyển đổi này cũng xử lý các bản sao một cách chính xác. Nếu nhiệt độ`5, 10, 10, 20`Và`K=2`, ngưỡng lạnh là`10`, và cả hai hành tinh có nhiệt độ`10`thuộc nhóm hoạt động. Nếu mọi nhiệt độ đều bằng nhau thì thậm chí`K=1`kích hoạt nhóm nhiệt độ duy nhất chứa mọi hành tinh. 

Nhóm truy vấn là thứ loại bỏ yếu tố (Q) khỏi phần đắt tiền. Mọi truy vấn có cùng ngưỡng hiệu dụng đều có thể được trả lời từ cùng trạng thái ma trận khoảng cách, vì vậy không có lý do gì để chạy một phép tính đường đi ngắn nhất khác cho nó. 

Bên trong`process_orientation`, vòng ngoài đi qua các nhóm nhiệt độ. Mỗi hành tinh trong nhóm hiện tại trở thành một đỉnh trung gian hợp pháp và áp dụng sự nới lỏng Floyd-Warshall tiêu chuẩn. Nhóm truy vấn chỉ được xử lý sau khi mọi đỉnh của nhóm được thêm vào. 

Nguồn và đích không bao giờ được lọc ra. Chúng vẫn hiện diện trong ma trận ngay từ đầu, xử lý trực tiếp quy tắc nhiệt độ điểm cuối không quan trọng. 

Việc triển khai sẽ xây dựng lại ma trận khoảng cách ban đầu trước hướng thứ hai. Việc thay đổi ma trận trong quá trình quét nguội không thể được sử dụng lại cho quá trình quét nóng vì hai lần quét có các tập đỉnh trung gian được phép khác nhau. 

Số nguyên Python không tràn và vô cực được chọn vượt xa bất kỳ độ dài tuyến đường hợp lệ nào. Cuộc thi được lưu trữ có giới hạn 1 giây rất chặt chẽ, do đó, phiên bản Python phải được coi là triển khai thuật toán dự định theo định hướng PyPy thay vì đảm bảo khớp với thời gian chạy của bài gửi C++. Bản thân thuật toán là (O(N^3)), là giới hạn dự định cho (N\le400). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Thứ tự nhiệt độ tăng dần là:```
planet 5: -210
planet 2: -180
planet 1:  -53
planet 6:   15
planet 7:  150
planet 4:  420
planet 3:  456
```Ở đây mọi nhiệt độ đều khác biệt nên mỗi nhóm nhiệt độ đều chứa chính xác một hành tinh. 

Đối với quét nóng, các nhóm được xử lý như`3, 4, 7, 6, 1, 2, 5`. 

| Nhóm nóng được kích hoạt | Hành tinh mới có | Truy vấn bị ảnh hưởng | Khoảng cách | 
| --- | --- | --- | --- | 
| 1 | 3 |`1 -> 2`, K=1 | 2 | 
| 2 | 4 |`1 -> 5`, K=2 | 11 | 
| 2 | 4 |`1 -> 7`, K=2 | 3 | 

Truy vấn đầu tiên đã có cạnh trực tiếp từ 1 đến 2, vì vậy câu trả lời của nó là 2. Sau khi có hành tinh 3 và 4, lộ trình`1 -> 3 -> 4 -> 5`có chiều dài`1 + 6 + 4 = 11`. Tuyến đường`1 -> 3 -> 7`có chiều dài`1 + 2 = 3`. 

Quét nguội bắt đầu với hành tinh 5, vì vậy truy vấn từ 5 đến 6 với (K=1) không thể sử dụng hành tinh 4, kết nối hữu ích duy nhất tới hành tinh 6. Nó vẫn không thể truy cập được. 

Đầu ra mẫu chính thức là`11, 2, -1, 3`. 

### Mẫu 2 

Nhiệt độ là:```
planet 1: 5
planet 2: 10
planet 3: 20
planet 4: 10
planet 5: 10
planet 6: 8
```Có bốn nhóm nhiệt độ riêng biệt:```
{1}: 5
{6}: 8
{2,4,5}: 10
{3}: 20
```Đối với các truy vấn nóng, thứ tự xử lý là:```
{3}, {2,4,5}, {6}, {1}
```| Nhóm nóng được kích hoạt | Giá trị nhiệt độ cho phép | Truy vấn | Khoảng cách | 
| --- | --- | --- | --- | 
| 1 |`{20}`|`1 -> 6`, K=1 | -1 | 
| 2 |`{20, 10}`|`1 -> 6`, K=2 | 25 | 
| 1 |`{20}`|`2 -> 4`, K=1 | 10 | 

Sau nhóm đầu tiên, chỉ có hành tinh 3 mới có thể được sử dụng làm trung gian nên chuỗi từ 1 đến 6 không thể hoàn thành. Sau nhóm thứ hai, các hành tinh 2, 4 và 5 đều có sẵn vì chúng có chung nhiệt độ 10. Đường dẫn đầy đủ```
1 -> 2 -> 3 -> 4 -> 5 -> 6
```có chiều dài`5 + 5 + 5 + 5 + 5 = 25`. 

Đối với truy vấn lạnh từ 4 đến 5 với (K=1), nhiệt độ lạnh nhất là 5, tương ứng với hành tinh 1. Cạnh trực tiếp từ 4 đến 5 là đủ nên đáp án là 5. 

Đầu ra mẫu cuối cùng là`25, -1, 5, 10`. 

Mẫu thứ hai đặc biệt hữu ích vì nó phát hiện được lỗi diễn giải nguy hiểm nhất. Việc chọn chính xác các chỉ số hành tinh (K) sẽ không cho phép cả ba hành tinh ở nhiệt độ 10, trong khi chọn nhiệt độ thứ (K) làm ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^3 + Q)) | Mỗi lần quét nhiệt độ thực hiện tối đa (N) lần chèn đỉnh Floyd-Warshall, mỗi lần tính giá (O(N^2)), trong khi mọi truy vấn đều được nhóm và trả lời một lần. | 
| Không gian | (O(N^2 + Q)) | Ma trận khoảng cách sử dụng (O(N^2)) và nhóm truy vấn cộng với việc sử dụng mảng câu trả lời (O(Q)). | 

Có hai lần quét Floyd-Warshall, một lần cho mỗi hướng nhiệt độ, nhưng hệ số không đổi của hai lần biến mất trong giới hạn tiệm cận. Với (N=400), số hạng bậc ba được giới hạn bởi (2\cdot400^3=128.000.000) độ giãn ma trận cơ bản trong trường hợp xấu nhất. Bản thân việc xử lý truy vấn là tuyến tính trong (Q), do đó, ngay cả 100.000 yêu cầu cũng thêm rất ít so với tính toán ma trận. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        solve()

        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
sample1 = """\
7 9
-53 -180 456 420 -210 15 150
1 2 2
1 3 1
2 3 4
2 4 2
2 5 5
3 4 6
6 4 10
4 5 4
3 7 2
4
1 5 2 1
1 2 1 1
5 6 1 0
1 7 2 1
"""

assert run(sample1) == "11\n2\n-1\n3", "sample 1"

# Provided sample 2
sample2 = """\
6 5
5 10 20 10 10 8
1 2 5
2 3 5
3 4 5
4 5 5
5 6 5
4
1 6 2 1
1 6 1 1
4 5 1 0
2 4 1 1
"""

assert run(sample2) == "25\n-1\n5\n10", "sample 2"

# Minimum-size graph, direct edge, endpoints must remain unrestricted.
case_min = """\
2 1
-5 100
1 2 7
2
1 2 1 0
1 2 1 1
"""

assert run(case_min) == "7\n7", "minimum-size direct route"

# No edges, so even though the endpoints themselves may have any
# temperature, no route exists.
case_disconnected = """\
2 0
0 1
2
1 2 1 0
1 2 1 1
"""

assert run(case_disconnected) == "-1\n-1", "disconnected graph"

# Equal temperatures at the boundary must all be admitted.
case_equal_boundary = """\
4 3
1 2 2 3
1 2 1
2 3 1
3 4 1
2
1 4 2 0
1 4 2 1
"""

assert run(case_equal_boundary) == "3\n3", "equal-temperature boundary"

# All temperatures equal. K=1 already includes every planet.
case_all_equal = """\
4 3
10 10 10 10
1 2 2
2 3 3
3 4 4
2
1 4 1 0
1 4 1 1
"""

assert run(case_all_equal) == "9\n9", "all equal temperatures"

# Maximum-size N and Q, with no edges. This exercises the query limit
# without requiring a huge expected-output literal.
n = 400
q = 100000

parts = [
    f"{n} 0",
    " ".join(["0"] * n),
    str(q),
]

for i in range(q):
    a = (i % n) + 1
    b = ((i + 1) % n) + 1
    parts.append(f"{a} {b} 1 {i & 1}")

case_max = "\n".join(parts) + "\n"

expected_max = "-1\n" * q
assert run(case_max) == expected_max[:-1], "maximum N and Q"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị tối thiểu có một cạnh |`7`,`7`| Tối thiểu (N), tuyến đường trực tiếp, điểm cuối không hạn chế | 
| Hai hành tinh bị cô lập |`-1`,`-1`| Các cặp không thể truy cập và xử lý vô cực | 
| Bốn hành tinh có hai nhiệt độ biên bằng nhau |`3`,`3`| Tất cả các hành tinh bị ràng buộc ở ngưỡng phải được kích hoạt | 
| Bốn hành tinh có nhiệt độ giống hệt nhau |`9`,`9`| Một nhóm nhiệt độ duy nhất có thể chứa mọi hành tinh | 
| (N=400,\ Q=100000), không có cạnh | 100.000 dòng`-1`| Số lượng truy vấn tối đa, số đỉnh tối đa, xử lý đầu ra lớn | 

## Vỏ cạnh 

### Điểm cuối nằm ngoài phạm vi nhiệt độ cho phép 

Hãy xem xét:```
2 1
0 100
1 2 7
1
1 2 1 0
```Nhiệt độ lạnh nhất là 0 nên chỉ có hành tinh 1 thuộc tập trung gian được phép. Tuy nhiên, lộ trình từ 1 đến 2 vẫn hợp lệ vì hành tinh 2 là điểm đến chứ không phải hành tinh trung gian. Ma trận khoảng cách chứa cạnh trực tiếp từ đầu nên thuật toán trả lời`7`trước khi cần đến bất kỳ đỉnh trung gian nào. 

### Nhiệt độ bằng nhau ở ngưỡng 

Hãy xem xét:```
4 3
1 2 2 3
1 2 1
2 3 1
3 4 1
1
1 4 2 0
```Nhiệt độ nhỏ thứ hai là 2. Cả hai hành tinh 2 và 3 đều có nhiệt độ đó nên cả hai đều được kích hoạt trước khi truy vấn được trả lời. Floyd-Warshall tìm thấy`1 -> 2 -> 3 -> 4`với khoảng cách 3. Nếu việc triển khai chỉ kích hoạt một hành tinh cho vị trí nhiệt độ thứ hai, nó sẽ báo cáo không chính xác rằng không thể truy cập được đích đến. 

### Tất cả các hành tinh đều có nhiệt độ như nhau 

Hãy xem xét:```
4 3
10 10 10 10
1 2 2
2 3 3
3 4 4
1
1 4 1 0
```Nhiệt độ lạnh nhất đầu tiên là 10 và mọi hành tinh đều có nhiệt độ đó. Toàn bộ nhóm nhiệt độ được kích hoạt ngay lập tức, do đó lộ trình`1 -> 2 -> 3 -> 4`được phép và có chi phí 9. Điều này cũng đúng đối với yêu cầu nóng nhất. Đây là lý do tại sao thuật toán hoạt động với các nhóm nhiệt độ thay vì các vị trí được sắp xếp riêng lẻ. 

### Không có tuyến đường nào tồn tại 

Hãy xem xét:```
2 0
0 1
1
1 2 1 0
```Ma trận khoảng cách bắt đầu bằng`dist[1][2] = INF`. Không có cạnh và không có hành tinh trung gian khả thi, vì vậy không có sự nới lỏng Floyd-Warshall nào có thể khiến cặp đôi này có thể tiếp cận được. Truy vấn nhìn thấy vô cùng và chuyển đổi nó thành`-1`. 

### Tuyến đường trực tiếp với các hành tinh trung gian bị cấm 

Hãy xem xét:```
3 1
0 50 100
1 3 9
1
1 3 1 0
```Cạnh duy nhất đi trực tiếp từ nguồn 1 đến đích 3. Hành tinh 2 không liên quan vì nó không được sử dụng. Hạn chế lạnh không làm mất hiệu lực của tuyến trực tiếp và câu trả lời là 9. Biểu diễn ma trận xử lý vấn đề này một cách tự nhiên vì các cạnh trực tiếp xuất hiện trước khi bất kỳ nhóm nhiệt độ nào được kích hoạt. 

### (K=N) 

Khi (K=N), nhiệt độ nhỏ nhất thứ (K) là nhiệt độ tối đa, do đó mọi hành tinh đều thuộc tập hợp lạnh cho phép. Một cách đối xứng, nhiệt độ lớn nhất (K) là nhiệt độ tối thiểu, do đó mọi hành tinh đều thuộc tập nóng cho phép. Quá trình tiền xử lý ánh xạ cả hai trường hợp tới tất cả các nhóm nhiệt độ, tạo ra các đường đi ngắn nhất cho tất cả các cặp thông thường. 

### Truy vấn có nhiệt độ trùng lặp và (K) bên trong một điểm hòa 

Giả sử nhiệt độ là`5, 10, 10, 10, 20`. Với (K=2), nhiệt độ nhỏ thứ hai là 10, không phải là một trong ba hành tinh có nhiệt độ 10. Do đó, nhiệt độ lạnh cho phép là 5 và 10, kích hoạt cả bốn hành tinh ở nhiệt độ đó. các`cold_level`phép tính sử dụng chính giá trị nhiệt độ nên nó tự động mở rộng ngưỡng cho toàn bộ nhóm liên kết. 

### Bộ cạnh trống 

Khi (R=0), mọi khoảng cách ngoài đường chéo đều bắt đầu ở vô cực. Các vòng lặp Floyd-Warshall vẫn chạy chính xác, nhưng không bao giờ có một cặp hữu hạn để thư giãn. Các truy vấn do đó trả về`-1`trừ khi một truy vấn có thể có (A=B), điều này bị cấm một cách rõ ràng.
