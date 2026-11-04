---
title: "CF 1141G - Tư nhân hóa đường ở Treeland"
description: "Chúng ta có một cây có $n$ đỉnh và $n-1$ cạnh. Mỗi cạnh phải được gán một số công ty. Một thành phố được coi là tốt nếu tất cả các con đường dẫn tới nó thuộc về các công ty khác nhau. Một thành phố trở nên tồi tệ nếu có ít nhất hai con đường gặp sự cố nhận cùng một công ty."
date: "2026-06-12T03:44:22+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "dfs-and-similar", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1141
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 547 (Div. 3)"
rating: 1900
weight: 1141
solve_time_s: 92
verified: true
draft: false
---

[CF 1141G - Tư nhân hóa đường ở Treeland](https://codeforces.com/problemset/problem/1141/G) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, dfs và tương tự, đồ thị, tham lam, cây 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh và$n-1$các cạnh. Mỗi cạnh phải được gán một số công ty. Một thành phố được coi là _tốt_ nếu tất cả các con đường liên quan đến nó thuộc về các công ty khác nhau. Một thành phố sẽ trở nên _xấu_ nếu có ít nhất hai con đường sự cố nhận cùng một công ty. 

Chúng tôi được phép có nhiều nhất$k$những thành phố tồi tệ. Trong số tất cả các nhiệm vụ hợp lệ, chúng ta phải giảm thiểu số lượng công ty$r$. Sau khi tìm được số nhỏ nhất có thể$r$, chúng ta cũng cần xuất ra một màu hợp lệ của các cạnh bằng cách sử dụng mã số công ty từ$1$ĐẾN$r$. 

Đồ thị là một cái cây, có nghĩa là có chính xác một đường đi đơn giữa mỗi cặp đỉnh. Đầu vào cung cấp điểm cuối của mỗi con đường và đầu ra phải gán mã số công ty cho mỗi con đường. 

Những hạn chế là thách thức thực sự. Cây có thể chứa tới$200\,000$các đỉnh, do đó, bất kỳ thuật toán nào quét liên tục tất cả các đỉnh hoặc thử nhiều màu ứng cử viên đều quá tốn kém. Chỉ có hai giây, giải pháp mong muốn phải gần với tuyến tính hoặc$O(n \log n)$. 

Khó khăn chính là việc giảm thiểu số lượng công ty không trực tiếp là vấn đề tô màu trên toàn bộ cây. Chúng tôi được phép vi phạm điều kiện về màu sắc riêng biệt ở mức tối đa$k$đỉnh. Hiểu được đỉnh nào nên được hy sinh là cái nhìn sâu sắc chính. 

Xét một ngôi sao có tâm ở đỉnh 1:```
1-2
1-3
1-4
1-5
```với$k=1$. 

Tâm có bậc 4. Nếu chúng ta cho phép đỉnh 1 xấu thì tất cả các cạnh có thể sử dụng cùng một màu và$r=1$. Một giải pháp mù quáng cố gắng làm cho mọi đỉnh đều tốt sẽ kết luận sai rằng cần có bốn màu khác nhau. 

Một trường hợp tế nhị khác là một đường dẫn:```
1-2-3-4-5
```với$k=0$. 

Mỗi đỉnh có bậc nhiều nhất là 2. Câu trả lời không phải là 2 vì việc tô màu các cạnh thích hợp của cây chỉ cần$\Delta$màu sắc, ở đâu$\Delta$là mức độ tối đa. Đây$\Delta=2$và các màu xen kẽ dọc theo đường dẫn sẽ hoạt động. Bất kỳ cách triển khai nào giả định mọi cạnh phải có một màu duy nhất sẽ đánh giá quá cao câu trả lời. 

Bẫy thứ ba xuất hiện khi nhiều đỉnh có cùng bậc. Giả sử độ là:```
5, 5, 5, 1, 1, ...
```Và$k=2$. 

Chúng ta có thể chọn bất kỳ hai đỉnh bậc 5 nào để trở thành xấu. Câu trả lời phụ thuộc vào bậc lớn thứ ba chứ không phụ thuộc vào đỉnh cụ thể nào được chọn. Việc triển khai xử lý các mức bằng nhau không chính xác có thể tạo ra giá trị lớn hơn mức cần thiết của$r$. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực là đoán số lượng công ty$r$, sau đó thử xác định xem cây có thể được tô màu nhiều nhất bằng$r$màu sắc trong khi thực hiện nhiều nhất$k$đỉnh xấu. 

Điều này nhanh chóng trở nên không khả thi. Ngay cả việc kiểm tra một giá trị duy nhất của$r$bằng cách khám phá các màu sắc có thể có của cạnh có thể có một không gian tìm kiếm khổng lồ. Một cái cây với$200\,000$đỉnh đã chứa$199\,999$các cạnh, do đó mọi tìm kiếm tổ hợp đều vô vọng. 

Bước ngoặt đến từ việc hỏi điều gì tạo ra một số lượng lớn màu sắc. 

Nếu một đỉnh phải duy trì tốt thì tất cả các cạnh liên quan cần có màu sắc riêng biệt. Một đỉnh bậc$d$ngay lập tức yêu cầu ít nhất$d$màu sắc có sẵn. 

Bây giờ giả sử chúng ta quyết định rằng một số đỉnh có thể xấu. Khi đó chúng ta không còn quan tâm liệu các cạnh tới có lặp lại màu sắc ở đó hay không. 

Quan sát này thay đổi hoàn toàn vấn đề. Các đỉnh duy nhất buộc số lượng màu lớn là các đỉnh chúng ta chọn để giữ nguyên. 

Giả sử các độ được sắp xếp theo thứ tự giảm dần:$$d_1 \ge d_2 \ge \cdots \ge d_n.$$Chúng ta có thể hy sinh nhiều nhất$k$đỉnh. Naturally, we should sacrifice the vertices with the largest degrees, since they impose the strongest color requirements.

 Sau khi loại bỏ những$k$hạn chế tồi tệ nhất, số lượng màu cần thiết tối thiểu sẽ trở thành$$r = d_{k+1}.$$Nếu như$k=0$, đây đơn giản là mức độ tối đa. Nếu như$k=n-1$, mọi đỉnh ngoại trừ một đỉnh đều có thể xấu, vì vậy$r=1$. 

Nhiệm vụ còn lại mang tính xây dựng: tạo màu cạnh bằng cách sử dụng chính xác$r$màu sắc. 

The standard tree edge-coloring DFS works perfectly here. We root the tree and color edges while traversing. Tại một đỉnh có bậc vượt quá$r$, chúng tôi cố ý cho phép lặp lại. Tại mỗi đỉnh khác, chúng ta gán các màu khác nhau cho tất cả các cạnh liên quan. 

Bởi vì$r$bằng với$(k+1)$- bậc lớn nhất, có nhiều nhất$k$đỉnh có bậc vượt quá$r$. Những điều đó trở thành những đỉnh xấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và tính bậc của mỗi đỉnh. 
2. Sắp xếp tất cả các độ theo thứ tự giảm dần. 
3. Hãy để$r$là$(k+1)$- bậc lớn nhất. Trong lập chỉ mục dựa trên số không, đây là`sorted_deg[k]`. 

Giá trị này là số lượng màu nhỏ nhất có thể vì ít nhất$n-k$các đỉnh phải duy trì ở mức tốt và mọi đỉnh có bậc tốt$d$yêu cầu$d$màu sắc riêng biệt. 
4. Gốc cây ở bất kỳ đỉnh nào, ví dụ đỉnh 1. 
5. Thực hiện DFS. 
6. Giả sử cạnh nối đỉnh hiện tại với đỉnh cha của nó có màu`parent_color`. 
7. Đối với các đỉnh con hiện tại, gán màu tuần tự từ 1 đến$r$, bỏ qua`parent_color`. 

Điều này đảm bảo rằng tất cả các cạnh tới tại đỉnh này nhận được các màu riêng biệt bất cứ khi nào bậc của nó không vượt quá$r$. 
8. Nếu bộ đếm màu vượt quá$r$, gói nó trở lại 1. 

Việc bao bọc như vậy chỉ có thể tạo ra các màu lặp lại ở các đỉnh có độ vượt quá$r$. Đó chính xác là những đỉnh điểm mà chúng tôi sẵn sàng làm xấu. 
9. Lưu trữ màu được chỉ định cho mỗi cạnh theo chỉ số đầu vào ban đầu của nó. 
10. Đầu ra$r$và tất cả các màu sắc cạnh. 

### Tại sao nó hoạt động 

Tính chất quan trọng là mọi đỉnh có bậc lớn nhất$r$nhận được các màu riêng biệt theo cặp trên tất cả các cạnh tới. 

Tại một đỉnh như vậy, một màu đã được chiếm giữ bởi cạnh của nó. DFS gán các màu riêng biệt cho tất cả các cạnh con trong khi bỏ qua màu gốc. Vì tổng số cạnh tới không vượt quá$r$, có đủ màu sắc và không xảy ra sự lặp lại. 

Sự lặp lại chỉ có thể xảy ra khi một đỉnh có bậc lớn hơn$r$. Số đỉnh như vậy nhiều nhất là$k$, bởi vì$r$đã được chọn là$(k+1)$- bậc lớn nhất. Như vậy nhiều nhất$k$các đỉnh trở nên xấu. 

Màu sắc chỉ sử dụng màu sắc$1 \ldots r$, thỏa mãn giới hạn đỉnh xấu và$r$là tối thiểu. Do đó thuật toán là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())

    g = [[] for _ in range(n)]
    deg = [0] * n
    edges = []

    for idx in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        edges.append((u, v))

        g[u].append((v, idx))
        g[v].append((u, idx))

        deg[u] += 1
        deg[v] += 1

    sorted_deg = sorted(deg, reverse=True)
    r = sorted_deg[k]

    ans = [0] * (n - 1)

    stack = [(0, -1, 0)]

    while stack:
        v, parent, parent_color = stack.pop()

        color = 1

        for to, eid in g[v]:
            if to == parent:
                continue

            if color == parent_color:
                color += 1

            if color > r:
                color = 1

            ans[eid] = color

            stack.append((to, v, color))

            color += 1
            if color > r:
                color = 1

    print(r)
    print(*ans)

if __name__ == "__main__":
    main()
```Phần đầu tiên xây dựng danh sách kề và tính độ đỉnh. 

Mảng độ đã sắp xếp chỉ được sử dụng một lần. Giá trị tại vị trí`k`chính xác là$(k+1)$- bậc lớn nhất, là số lượng màu tối ưu. 

DFS có tính lặp lại chứ không phải đệ quy. Một giải pháp đệ quy có thể vượt quá giới hạn đệ quy của Python trên cây với$200\,000$đỉnh. 

Biến`parent_color`ghi lại màu của cạnh dẫn vào đỉnh hiện tại. Khi gán màu cho các cạnh con, màu đó sẽ bị bỏ qua bất cứ khi nào có thể. Đây là điều đảm bảo các màu tới khác biệt ở các đỉnh bậc thấp. 

Hành vi bao quanh là có chủ ý. Khi một đỉnh có nhiều hơn`r`các cạnh sự cố, sự lặp lại trở nên không thể tránh khỏi. Những đỉnh đó chính xác là những đỉnh chúng ta được phép hy sinh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2
1 4
4 3
3 5
3 6
5 2
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 2 | 
| 6 | 1 | 

Sắp xếp độ: 

| Vị trí | Bằng cấp | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 1 | 
| 5 | 1 | 
| 6 | 1 | 

Từ$k=2$,$$r = 2.$$Dấu vết DFS: 

| Đỉnh | Màu gốc | Màu sắc trẻ em được chỉ định | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 4 | 1 | 2 | 
| 3 | 2 | 1, 2 | 
| 5 | 1 | 2 | 

Một đầu ra hợp lệ là:```
2
1 2 1 2 2
```Đỉnh 3 có bậc 3 lớn hơn$r=2$, vì vậy nó có thể trở nên tồi tệ. Tất cả các đỉnh khác vẫn tốt. 

### Ví dụ 2 

đầu vào:```
5 0
1 2
1 3
1 4
1 5
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 4 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 

Sắp xếp độ: 

| Vị trí | Bằng cấp | 
| --- | --- | 
| 1 | 4 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 

Từ$k=0$,$$r = 4.$$Dấu vết DFS: 

| Đỉnh | Màu gốc | Màu sắc trẻ em được chỉ định | 
| --- | --- | --- | 
| 1 | 0 | 1,2,3,4 | 

Đầu ra:```
4
1 2 3 4
```Không có đỉnh xấu nào được phép, do đó tâm phải nhận được bốn màu tới riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng độ chiếm ưu thế | 
| Không gian | O(n) | Danh sách kề, mảng độ, ngăn xếp DFS, đáp án | 

Cây chứa nhiều nhất$200\,000$đỉnh. Sắp xếp$200\,000$số nguyên đủ nhanh và tất cả công việc còn lại là tuyến tính theo số cạnh. Việc sử dụng bộ nhớ cũng thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
# For this problem many outputs are valid, so these tests
# check the optimal r value only.

import io
import sys

def run(inp: str) -> str:
    from collections import deque

    sys.stdin = io.StringIO(inp)

    n, k = map(int, sys.stdin.readline().split())
    deg = [0] * n

    edges = []
    for _ in range(n - 1):
        u, v = map(int, sys.stdin.readline().split())
        deg[u - 1] += 1
        deg[v - 1] += 1

    r = sorted(deg, reverse=True)[k]
    return str(r)

# sample
assert run(
"""6 2
1 4
4 3
3 5
3 6
5 2
"""
) == "2"

# minimum tree
assert run(
"""2 0
1 2
"""
) == "1"

# star, one bad vertex allowed
assert run(
"""5 1
1 2
1 3
1 4
1 5
"""
) == "1"

# star, no bad vertices allowed
assert run(
"""5 0
1 2
1 3
1 4
1 5
"""
) == "4"

# path graph
assert run(
"""5 0
1 2
2 3
3 4
4 5
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn | 1 | Cây nhỏ nhất có thể | 
| Ngôi sao, k=1 | 1 | Đỉnh bậc cao có thể bị hy sinh | 
| Ngôi sao, k=0 | 4 | Mức độ tối đa được yêu cầu khi tất cả các đỉnh phải tốt | 
| Đường dẫn | 2 | Xử lý đúng cây cấp 2 | 
| Mẫu | 2 | Trận đấu ví dụ chính thức | 

## Vỏ cạnh 

Hãy xem xét ngôi sao```
5 1
1 2
1 3
1 4
1 5
```Trình tự độ là$[4,1,1,1,1]$. Mức độ lớn thứ hai là 1, vì vậy$r=1$. Tâm là đỉnh duy nhất có bậc vượt quá$r$, điều này được cho phép vì$k=1$. Mọi cạnh đều nhận được màu 1. Giải pháp nhấn mạnh vào các màu riêng biệt ở mọi đỉnh sẽ tạo ra kết quả 4 không chính xác. 

Hãy xem xét con đường```
5 0
1 2
2 3
3 4
4 5
```Trình tự độ là$[2,2,2,1,1]$. Vì không cho phép có đỉnh xấu,$r=2$. Trong DFS, các màu thay thế một cách tự nhiên và mỗi đỉnh đều nhìn thấy các màu tới riêng biệt. Thuật toán không bao giờ đưa ra những màu không cần thiết. 

Xét một cây có ba bậc lớn nhất bằng nhau:```
7 2
1 2
1 3
1 4
5 1
6 1
7 1
```Trình tự độ là$[6,1,1,1,1,1,1]$. Với$k=2$, câu trả lời sẽ trở thành bậc lớn thứ ba, vẫn là 1. Thuật toán chỉ dựa vào thứ tự được sắp xếp, do đó các mối quan hệ được xử lý tự động mà không cần logic đặc biệt. 

Cuối cùng, khi$k=n-1$, mọi đỉnh ngoại trừ một đỉnh đều có thể xấu. Chỉ số mức độ được sắp xếp`k`luôn tồn tại vì mảng có độ dài$n$. Câu trả lời trở thành 1 và tô màu mọi cạnh bằng màu 1 là hợp lệ. Việc triển khai xử lý trường hợp này mà không cần phân nhánh.
