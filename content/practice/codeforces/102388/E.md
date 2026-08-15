---
title: "CF 102388E - Chuồng ngựa"
description: "Chúng tôi có một biểu đồ vô hướng với tối đa 50 thành phố và tối đa 2500 con đường. Một con đường có thể kết nối hai thành phố khác nhau hoặc kết nối một thành phố với chính nó, vì vậy cho phép đi vòng. Bắt đầu từ một thành phố, chúng ta phải đi theo đúng (k) con đường và về đích tại cùng một thành phố."
date: "2026-08-14T13:55:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102388
codeforces_index: "E"
codeforces_contest_name: "SUFE ICPC Team Formation Test"
rating: 0
weight: 102388
solve_time_s: 313
verified: false
draft: false
---

[CF 102388E - Chuồng ngựa](https://codeforces.com/problemset/problem/102388/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một biểu đồ vô hướng với tối đa 50 thành phố và tối đa 2500 con đường. Một con đường có thể kết nối hai thành phố khác nhau hoặc kết nối một thành phố với chính nó, vì vậy cho phép đi vòng. Bắt đầu từ một thành phố, chúng ta phải đi theo đúng (k) con đường và về đích tại cùng một thành phố. Một thành phố là hợp lệ nếu tồn tại một lối đi khép kín có chiều dài chính xác (k). 

Nhiệm vụ là đếm tất cả các thành phố xuất phát hợp lệ một cách độc lập. Các thành phố khác nhau có thể sử dụng các lối đi hoàn toàn khác nhau và việc đi bộ được phép lặp lại cả thành phố và đường. 

Giá trị nhỏ của (n) là đầu mối chính. Chỉ với 50 đỉnh, một phương thức (O(n^3)) hoặc thậm chí là (O(n^3 \log k)) sẽ hợp lý trong một ngôn ngữ được biên dịch, trong khi giá trị khổng lồ (k \le 10^9) loại trừ mọi thứ xử lý trực tiếp hàng ngày. Chúng ta cần tránh thực hiện công tỷ lệ với (k). Thực tế là có nhiều nhất 2500 con đường cũng có nghĩa là việc duyệt đồ thị và các chương trình động nhỏ sẽ rẻ. 

Có một số trường hợp đặc biệt có thể đánh lừa một giải pháp chỉ dựa trên tính chẵn lẻ. 

Đầu tiên, (k=0) có nghĩa là con ngựa không di chuyển, vì vậy mọi thành phố đều đã quay trở lại chính nó. Ví dụ,```
1
1 0 0
```có đầu ra```
1
```Một giải pháp yêu cầu ít nhất một lần đi qua đường sẽ trả về 0 không chính xác. 

Thứ hai, một đỉnh cô lập không thể thực hiện bất kỳ bước đi có độ dài dương nào. Ví dụ,```
1
2 0 2
```có đầu ra```
0
```Không có đường ở cả hai thành phố, vì vậy mặc dù 2 là số chẵn, đối số thông thường "mọi chiều dài dương đều có tác dụng" không được áp dụng. 

Thứ ba, một vòng lặp tạo ra một bước đi khép kín có độ dài bằng một. Ví dụ,```
1
1 1 1
0 0
```có đầu ra```
1
```Kiểm tra lưỡng cực coi biểu đồ là biểu đồ đơn giản và bỏ qua các vòng lặp sẽ phân loại không chính xác thành phần này là lưỡng cực. 

Thứ tư, việc nằm trong một thành phần không lưỡng cực tự nó không đủ cho (k) số lẻ nhỏ. Coi như```
1
3 3 1
0 1
1 2
2 0
```Tam giác không có hai bên, nhưng không có bước đi khép kín một bước vì không có vòng lặp. Đầu ra đúng là```
0
```Đây là lý do tại sao thuật toán xử lý chính xác (k) nhỏ trước khi sử dụng thuộc tính chẵn lẻ cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các bước đi bằng lập trình động. Đối với mỗi (các) thành phố bắt đầu, hãy giữ cho tập hợp các thành phố có thể truy cập được sau chính xác (t) bước. Ban đầu chỉ có thể truy cập được. Đối với mỗi bước, hãy đi theo từng con đường sự cố từ mọi thành phố hiện có thể tiếp cận. Sau (k) bước, hãy kiểm tra xem (các) bước đó có thể truy cập được hay không. 

Điều này đúng vì trạng thái sau (t) bước chứa chính xác các điểm cuối có thể có của tất cả các bước có độ dài (t). Vấn đề là giá trị của (k). Trong trường hợp xấu nhất, chương trình động sẽ thực hiện xử lý lân cận (O(k n m)) khi nó được chạy cho mọi thành phố bắt đầu. Với (k=10^9), (n=50) và (m=2500), đây là thứ tự chuyển đổi đồ thị (1,25 \cdot 10^{14}). (k) lớn làm cho điều này không thể thực hiện được. 

Quan sát quan trọng là đồ thị vô hướng có mô hình dài hạn rất đơn giản cho các bước đi khép kín. Mọi độ dài chẵn dương đều có thể xảy ra ở mọi đỉnh không cô lập, bởi vì chúng ta có thể đi qua bất kỳ cạnh nào tới và ngay lập tức đi qua nó trở lại. Việc lặp lại bước đi hai bước đó sẽ mang lại độ dài chẵn dương. 

Độ dài lẻ hành xử khác nhau. Một thành phần liên thông là lưỡng cực chính xác khi nó không chứa chu trình lẻ. Trong thành phần lưỡng cực, mọi bước đi khép kín đều có độ dài chẵn, do đó không có giá trị lẻ nào của (k) có thể hoạt động. Trong thành phần không lưỡng cực, mỗi đỉnh cuối cùng đều có các bước đi khép kín của cả hai điểm chẵn lẻ. Cụ thể hơn, mỗi đỉnh có độ dài bước đi khép kín lẻ nhiều nhất là (2n-1). Khi tồn tại một bước đi khép kín lẻ, chúng ta có thể thêm bất kỳ số bước lùi hai bước nào, do đó mọi độ dài lẻ đủ lớn cũng tồn tại. 

Điều này mang lại cho chúng tôi sự phân chia rõ ràng. Nếu (k) lớn nhất là (2n), chúng ta chỉ cần tính toán chính xác câu trả lời bằng một chương trình động bitset nhỏ. Nếu (k) lớn hơn (2n), chúng ta không cần cấu trúc bước đi chính xác nữa. Đối với (k), mọi đỉnh không cô lập đều hoạt động. Đối với số lẻ (k), chính xác các đỉnh thuộc các thành phần không lưỡng cực đều hoạt động. 

Việc biểu diễn bitset làm cho phần chính xác trở nên đặc biệt rẻ. Một tập hợp các thành phố có thể truy cập được biểu thị bằng một số nguyên Python, trong đó bit (j) được đặt khi có thể truy cập thành phố (j). Để tiến lên một bước, đối với mỗi thành phố có thể tiếp cận (v), chúng tôi HOẶC tập hợp bit lân cận của nó vào tập hợp có thể tiếp cận mới. Vì (n\le50), tất cả những thứ này nằm gọn trong một vài số nguyên Python có kích thước bằng máy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP cho tất cả (k) bước | (O(k n m)) | (O(n^2)) | Quá chậm | 
| Tập bit chính xác DP lên tới (2n), sau đó phân tích chẵn lẻ/thành phần | (O(n^2 \min(k,n) + n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cả danh sách kề và tập bit kề cho biểu đồ. Danh sách kề được sử dụng để xác định các thành phần được kết nối và tính lưỡng cực. Bitset cho phép chương trình động chính xác cập nhật tất cả các điểm cuối có thể bằng cách sử dụng các phép toán OR số nguyên nhanh. 
2. Nếu (k=0), trả về (n). Cuộc đi bộ trống bắt đầu và kết thúc tại cùng một thành phố bất kể biểu đồ. 
3. Nếu (k\le2n), hãy tính kết quả chính xác. Đối với mỗi thành phố (i), tập hợp có thể truy cập ban đầu chỉ là ({i}), được biểu thị bằng số nguyên (1\ll i). Sau một bước, hãy thay thế tập hợp có thể tiếp cận bằng tập hợp các tập hợp kề của tất cả các thành phố hiện có thể tiếp cận. Lặp lại điều này chính xác (k) lần.

Sau lần lặp thứ (t), bit ở vị trí (j) được đặt chính xác khi tồn tại một quãng đường có độ dài (t) từ thành phố ban đầu (i) đến thành phố (j). Do đó, các bit đường chéo sau (k) lần lặp cho chúng ta biết chính xác thành phố nào có chiều dài đi bộ khép kín (k). 
4. Nếu (k>2n), hãy chạy BFS hoặc DFS trên mọi thành phần được kết nối trong khi gán cho mỗi đỉnh một màu nhị phân. Dọc theo mọi cạnh thông thường, các điểm cuối của nó phải có màu đối lập nhau trong biểu đồ hai bên. Nếu một cạnh nối hai đỉnh có cùng màu thì thành phần đó chứa chu trình lẻ và không có lưỡng cực. Một vòng lặp ngay lập tức tạo ra xung đột như vậy vì hai điểm cuối của nó có cùng một đỉnh. 
5. Đối với (k) chẵn lớn, hãy đếm mọi đỉnh có bậc dương. Từ một đỉnh như vậy, chọn một cạnh tới và đi qua nó qua lại. Việc lặp lại bước đi hai bước này sẽ tạo ra độ dài chẵn dương. 
6. Đối với (k) số lẻ lớn, hãy đếm mọi đỉnh có thành phần được tìm thấy là không lưỡng cực. Một thành phần không lưỡng cực chứa một chu trình lẻ. Từ bất kỳ đỉnh nào, đi đến chu trình đó, đi qua chu trình đó một lần và quay lại theo cùng một đường đi. Điều này mang lại một bước đi khép kín kỳ lạ. Cấu trúc có độ dài tối đa (2n-1) và vì (k>2n), hiệu giữa (k) và độ dài lẻ đó là số chẵn dương. Chúng ta có thể lấp đầy sự khác biệt đó bằng việc quay lại hai bước lặp đi lặp lại. 

### Tại sao nó hoạt động 

Đối với phần chính xác, bất biến là sau (t) lần lặp,`reach[i]`chứa chính xác các đỉnh có thể tiếp cận từ (i) bằng một bước đi chính xác (t) cạnh. Bản cập nhật lấy mọi đỉnh hiện có thể tiếp cận và đi theo một cạnh nữa, do đó, nó không bỏ lỡ một bước đi khả thi cũng như không đưa ra một điểm cuối không thể thực hiện được. Do đó, các lần lặp bit (i) sau (k) được đặt chính xác khi có bước đi khép kín có độ dài-(k) tại (i). 

Đối với (k lớn), trước tiên hãy xem xét độ dài chẵn. Bất kỳ đỉnh không cô lập nào đều có bước đi khép kín hai cạnh, có được bằng cách đi qua cạnh tới theo cả hai hướng. Lặp đi lặp lại nó sẽ cho mỗi độ dài chẵn dương. Một đỉnh cô lập không có bước đi dương nào cả. 

Đối với độ dài lẻ, một thành phần lưỡng cực không thể chứa một bước đi khép kín lẻ vì mỗi cạnh đều thay đổi phía lưỡng cực, vì vậy sau một số cạnh lẻ thì bước đi phải ở phía đối diện. Một thành phần không lưỡng cực chứa một chu trình lẻ. Đối với một đỉnh (v), hãy lấy đường đi ngắn nhất có độ dài (d) từ (v) đến chu trình đó và đặt chu trình lẻ có độ dài (l). Đường đi và chu trình có thể được chọn để chỉ gặp nhau ở điểm cuối, do đó (d+l\le n). Bước đi khép kín thu được có độ dài (2d+l), tối đa là (d+n\le2n-1). Việc thêm bất kỳ số lượng dấu lùi hai cạnh nào sẽ cho mỗi độ dài lẻ lớn hơn. Vì thuật toán chỉ sử dụng đối số này khi (k>2n), độ dài lẻ lớn cần thiết luôn có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, m, k, edges):
    graph = [[] for _ in range(n)]
    adj_bits = [0] * n
    degree = [0] * n

    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

        adj_bits[u] |= 1 << v
        adj_bits[v] |= 1 << u

        degree[u] += 1
        degree[v] += 1

    if k == 0:
        return n

    # Small k: compute the exact set of endpoints after k steps.
    if k <= 2 * n:
        reach = [1 << i for i in range(n)]

        for _ in range(k):
            new_reach = [0] * n

            for start in range(n):
                bits = reach[start]
                result = 0

                while bits:
                    low = bits & -bits
                    v = low.bit_length() - 1
                    result |= adj_bits[v]
                    bits -= low

                new_reach[start] = result

            reach = new_reach

        answer = 0
        for i in range(n):
            if (reach[i] >> i) & 1:
                answer += 1

        return answer

    # Large k: only the parity structure of each component matters.
    color = [-1] * n
    component = [-1] * n
    component_bad = []

    for start in range(n):
        if color[start] != -1:
            continue

        cid = len(component_bad)
        component_bad.append(False)

        color[start] = 0
        component[start] = cid
        stack = [start]

        while stack:
            u = stack.pop()

            for v in graph[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    component[v] = cid
                    stack.append(v)
                elif color[v] == color[u]:
                    component_bad[cid] = True

    if k % 2 == 0:
        return sum(degree[i] > 0 for i in range(n))

    return sum(component_bad[component[i]] for i in range(n))

def solve():
    t = int(input())

    for _ in range(t):
        n, m, k = map(int, input().split())
        edges = [tuple(map(int, input().split())) for _ in range(m)]

        print(solve_case(n, m, k, edges))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve_case`xây dựng hai biểu diễn đồ thị.`graph`lưu trữ các cạnh cho quá trình truyền tải thành phần và lưỡng cực sau này.`adj_bits[u]`lưu trữ mọi lân cận của (u) trong một số nguyên, do đó thực hiện thêm một bước biểu đồ sẽ trở thành một chuỗi các phép toán OR số nguyên. 

Chương trình động chính xác bắt đầu bằng`1 << i`đối với thành phố (i), bởi vì trước khi đi đến bất kỳ cạnh nào, thành phố duy nhất có thể tiếp cận là chính (i). Đối với mọi đỉnh có thể tiếp cận`v`,`adj_bits[v]`chứa tất cả các đích đến có thể sau một bước bổ sung. HOẶC-ing tất cả các mặt nạ đó sẽ mang lại chính xác bộ có thể truy cập tiếp theo. 

Vòng lặp được giới hạn ở`k <= 2 * n`. Ranh giới này là có chủ ý. Chúng ta không cần biết chính xác độ dài bước đi vượt quá (2n), vì cấu trúc thành phần hoàn toàn xác định câu trả lời ở đó. 

Quá trình truyền tải lưỡng cực gán màu sắc`0`Và`1`. Một vòng lặp xuất hiện trong`graph[u]`như một cạnh từ`u`với chính nó, vì vậy`color[v] == color[u]`ngay lập tức đánh dấu thành phần là không lưỡng cực. Các cạnh song song không gây ra vấn đề gì vì việc lặp lại cùng một phép kiểm tra kề không làm thay đổi kết quả. 

Đối với (k) thậm chí lớn,`degree[i] > 0`là điều kiện hoàn chỉnh. Đối với (k lớn lẻ), mã định danh thành phần ánh xạ từng đỉnh tới trạng thái lưỡng cực của nó, do đó`component_bad[component[i]]`trực tiếp cho biết thành phố (i) có thuộc thành phần không lưỡng đảng hay không. 

Không có vấn đề tràn số nguyên trong Python. Bitset lớn nhất chỉ có 50 bit liên quan và (k) được lưu trữ dưới dạng số nguyên Python thông thường. 

## Ví dụ đã hoạt động 

### Mẫu 1, testcase đầu tiên 

Đồ thị là một đường đi có độ dài bằng 2, với thành phố 0 ở giữa. 

Đối với (k=3), chúng ta ở trong phạm vi DP chính xác vì (3\le2n=6). 

| Bước | Thành phố 0 có thể truy cập | Thành phố 1 có thể tiếp cận | Thành phố 2 có thể tiếp cận | 
| --- | --- | --- | --- | 
| 0 | {0} | {1} | {2} | 
| 1 | {1,2} | {0} | {0} | 
| 2 | {0} | {1,2} | {1,2} | 
| 3 | {1,2} | {0} | {0} | 

Không có hàng nào chứa thành phố bắt đầu sau ba bước, vì vậy câu trả lời là 0. 

Biểu đồ có tính chất lưỡng cực, điều này cũng giải thích tại sao các bước đi khép kín lẻ không thể tồn tại. DP chính xác vẫn được sử dụng vì thuật toán phải xử lý tất cả các giá trị nhỏ của (k), bao gồm cả trường hợp chỉ tính chẵn lẻ cuối cùng là không đủ. 

### Mẫu 1, testcase thứ ba 

Biểu đồ chứa một hình tam giác (0,1,2), cộng với đường dẫn (3-4-0). Ở đây (n=5) và (k=5), do đó (k\le2n) và DP chính xác được sử dụng. 

| Bước | Thành Phố 0 | Thành phố 1 | Thành phố 2 | Thành phố 3 | Thành phố 4 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | {0} | {1} | {2} | {3} | {4} | 
| 1 | {1,2,4} | {0,2} | {0,1} | {4} | {0,3} | 
| 2 | {0,2,3,4} | {0,1,4} | {0,1,2,4} | {0,3} | {1,2,4} | 
| 3 | {0,1,2,3,4} | {0,1,2,3} | {0,1,2,3,4} | {4} | {0,1,2,3} | 
| 4 | {0,1,2,3,4} | {0,1,2,3,4} | {0,1,2,3,4} | {0,3} | {0,1,2,4} | 
| 5 | {0,1,2,3,4} | {0,1,2,3,4} | {0,1,2,3,4} | {4} | {0,1,2,3,4} | 

Các thành phố 0, 1, 2 và 4 chứa chính chúng sau năm bước. Thành phố 3 thì không nên đáp án là 4. 

Ví dụ này cũng cho thấy tại sao chỉ kết nối thôi là chưa đủ. Thành phố 3 được kết nối với tam giác không lưỡng cực, tuy nhiên nó không có lối đi khép kín dài 5. Tính toán chính xác xử lý hạn chế khoảng cách ngắn đó một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2\min(k,2n)+n+m)) | Mỗi bước DP chính xác xử lý tối đa (n) bộ có thể truy cập, mỗi bộ chứa tối đa (n) đỉnh. Lớn (k) chỉ yêu cầu duyệt một đồ thị. | 
| Không gian | (O(n+m)) | Danh sách kề, bitset, màu sắc, ID thành phần và mảng DP đều sử dụng tối đa không gian (O(n+m)). | 

Với (n\le50), pha chính xác thực hiện tối đa (2n=100) lần lặp. Ngay cả trong một biểu đồ dày đặc, mỗi lần lặp chỉ xử lý 50 bit nhỏ, do đó khối lượng công việc rất nhỏ. Pha lớn (k) chỉ là một phép truyền đồ thị tuyến tính. Giải pháp thoải mái phù hợp với giới hạn 3 giây và 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây tái tạo thuật toán thông qua`solve_case`và kiểm tra các mẫu cùng với một số trường hợp biên.```python
import io

def solve_case(n, m, k, edges):
    graph = [[] for _ in range(n)]
    adj_bits = [0] * n
    degree = [0] * n

    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
        adj_bits[u] |= 1 << v
        adj_bits[v] |= 1 << u
        degree[u] += 1
        degree[v] += 1

    if k == 0:
        return n

    if k <= 2 * n:
        reach = [1 << i for i in range(n)]

        for _ in range(k):
            new_reach = [0] * n

            for start in range(n):
                bits = reach[start]
                result = 0

                while bits:
                    low = bits & -bits
                    v = low.bit_length() - 1
                    result |= adj_bits[v]
                    bits -= low

                new_reach[start] = result

            reach = new_reach

        return sum((reach[i] >> i) & 1 for i in range(n))

    color = [-1] * n
    component = [-1] * n
    component_bad = []

    for start in range(n):
        if color[start] != -1:
            continue

        cid = len(component_bad)
        component_bad.append(False)

        color[start] = 0
        component[start] = cid
        stack = [start]

        while stack:
            u = stack.pop()

            for v in graph[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    component[v] = cid
                    stack.append(v)
                elif color[v] == color[u]:
                    component_bad[cid] = True

    if k % 2 == 0:
        return sum(degree[i] > 0 for i in range(n))

    return sum(component_bad[component[i]] for i in range(n))

def run(inp):
    data = list(map(int, inp.split()))
    p = 0

    t = data[p]
    p += 1
    out = []

    for _ in range(t):
        n, m, k = data[p], data[p + 1], data[p + 2]
        p += 3

        edges = []
        for _ in range(m):
            u, v = data[p], data[p + 1]
            p += 2
            edges.append((u, v))

        out.append(str(solve_case(n, m, k, edges)))

    return "\n".join(out) + "\n"

# Provided sample.
sample = """\
3
3 2 3
0 1
0 2
3 2 4
0 1
0 2
5 5 5
0 1
1 2
2 0
3 4
4 0
"""
assert run(sample) == "0\n3\n4\n", "sample"

# Minimum-size graph, no edges, k = 0.
assert run("""\
1
1 0 0
""") == "1\n", "k = 0"

# One vertex with several loops, all endpoints equal.
# Every positive k is possible.
assert run("""\
1
1 5 1000000000
0 0
0 0
0 0
0 0
0 0
""") == "1\n", "all-equal loop edges"

# Boundary between the exact and large-k phases.
# A single edge is bipartite, so even lengths work and odd lengths do not.
assert run("""\
4
2 1 4
0 1
2 1 5
0 1
3 2 6
0 1
1 2
3 2 7
0 1
1 2
""") == "2\n0\n3\n0\n", "parity boundary"

# Large odd k in a non-bipartite component.
# Triangle plus a leaf. Every vertex belongs to the same non-bipartite component.
assert run("""\
1
4 4 1000000001
0 1
1 2
2 0
2 3
""") == "4\n", "large odd non-bipartite"

# Maximum-size graph: complete graph on 50 vertices.
# There are 50^2 = 2500 roads when loops are included.
# Every vertex has a loop, so every positive k works.
n = 50
edges = [(i, j) for i in range(n) for j in range(n)]
max_input = "1\n50 2500 1000000000\n"
max_input += "\n".join(f"{u} {v}" for u, v in edges) + "\n"

assert run(max_input) == "50\n", "maximum-size dense graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 0 0`|`1`| Bước đi trống và kích thước biểu đồ tối thiểu | 
|`1 / 1 5 1000000000 / 0 0 ...`|`1`| Vòng lặp và độ dài lẻ lớn tùy ý | 
| Các trường hợp một cạnh và đường dẫn với (k=4,5,6,7) |`2,0,3,0`| Các bước đi khép kín chẵn và lẻ và ranh giới nhỏ/lớn | 
| Tam giác có một chiếc lá và số lẻ rất lớn (k) |`4`| Xử lý thành phần không lưỡng cực cho số lẻ lớn (k) | 
| Đồ thị hoàn chỉnh trên 50 đỉnh với 2500 con đường |`50`| Tối đa (n), tối đa (m) và lân cận dày đặc | 

## Vỏ cạnh 

Với (k=0), hãy xem xét```
1
1 0 0
```Thuật toán trả về ngay lập tức với`n`, bằng 1. Không cần thông tin kề cận vì quãng đường đi bộ có độ dài bằng 0 không yêu cầu đường. Điều này tránh được sai lầm phổ biến là yêu cầu thành phố xuất phát phải có mức độ tích cực. 

Đối với một đỉnh cô lập có số chẵn dương (k), hãy xem xét```
1
2 0 2
```Phím tắt lớn-(k) không được sử dụng vì (2\le2n), do đó DP chính xác bắt đầu bằng`{0}`Và`{1}`. Sau một bước, cả hai tập hợp trở nên trống vì không có cạnh liên quan và chúng vẫn trống. Không có bit chéo nào xuất hiện, cho kết quả 0. Nếu trường hợp tương tự có (k) chẵn lớn hơn nhiều, thì nhánh lớn-(k) sẽ kiểm tra rõ ràng`degree[i] > 0`, ngăn cản việc một thành phố bị cô lập được chấp nhận. 

Đối với một vòng lặp, hãy xem xét```
1
1 1 1
0 0
```DP chính xác bắt đầu bằng bit 0 được đặt. Sau một bước, nó OR mặt nạ kề của đỉnh 0, chứa bit 0 do vòng lặp. Bit đường chéo vẫn được đặt, vì vậy câu trả lời là 1. Trong nhánh lớn-(k), cùng một vòng lặp làm cho quá trình truyền tải lưỡng cực gặp một cạnh có điểm cuối cùng màu, đánh dấu thành phần không phải lưỡng cực. 

Đối với đồ thị không lưỡng cực có số lẻ (k) nhỏ, hãy xét tam giác```
1
3 3 1
0 1
1 2
2 0
```Đồ thị chứa một chu trình lẻ nhưng không có vòng lặp. Với (k=1), không có đỉnh nào có thể quay về chính nó trong một cạnh. Vì (1\le2n), DP chính xác được sử dụng và trả về đúng 0. Đây là lý do phân loại chẵn lẻ lớn (k) không thể áp dụng đơn giản cho mọi (k lẻ). 

Cuối cùng, hãy xem xét một biểu đồ lưỡng cực có số lẻ (k) rất lớn:```
1
3 2 1000000001
0 1
1 2
```Ở đây (k>2n), do đó thuật toán chạy kiểm tra lưỡng cực thay vì lặp lại một tỷ lần. Thành phần này là lưỡng cực, vì vậy`component_bad`là sai. Vì (k) là số lẻ nên không có thành phố nào được tính và câu trả lời là 0. Kết quả rút ra từ thực tế là mọi bước đi khép kín trong biểu đồ hai bên đều có độ dài chẵn.
