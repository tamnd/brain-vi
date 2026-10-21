---
title: "CF 105811K - Bảo tàng nghệ thuật Philadelphia"
description: "Chúng ta được tặng một cây phòng bảo tàng có gốc ở phòng 1, trong đó phòng 1 là lối vào. Hai người, Kuroni và Tfg, bắt đầu từ lối vào và luôn di chuyển cùng nhau trên một con đường duy nhất. Tại phòng không có lá, phải chọn đúng một phòng trẻ làm điểm đến tiếp theo."
date: "2026-06-25T15:21:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105811
codeforces_index: "K"
codeforces_contest_name: "UT Open 2025"
rating: 0
weight: 105811
solve_time_s: 48
verified: true
draft: false
---

[CF 105811K - Bảo tàng nghệ thuật Philadelphia](https://codeforces.com/problemset/problem/105811/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một cây phòng bảo tàng có gốc ở phòng 1, trong đó phòng 1 là lối vào. Hai người, Kuroni và Tfg, bắt đầu từ lối vào và luôn di chuyển cùng nhau trên một con đường duy nhất. 

Tại phòng không có lá, phải chọn đúng một phòng trẻ làm điểm đến tiếp theo. Kuroni chọn nước đi đầu tiên, sau đó Tfg chọn nước đi tiếp theo, sau đó lại chọn Kuroni, v.v. Vì biểu đồ là một cái cây và việc thăm lại các phòng bị cấm nên hành trình của họ chỉ đơn giản là một con đường từ gốc tới lá. 

Mỗi phòng có thể được Kuroni, Tfg, cả hai hoặc không ai thích. Điểm của người chơi là số phòng thích xuất hiện trên đường dẫn cuối cùng. 

Khi người chơi đưa ra quyết định, họ sẽ hành động một cách tối ưu. Đầu tiên, họ tối đa hóa điểm số cuối cùng của chính mình. Nếu có nhiều lựa chọn cho cùng số điểm, họ sẽ chọn lựa chọn mang lại số điểm lớn nhất cho người chơi khác. 

Nhiệm vụ là xác định điểm cuối cùng của cả hai người chơi. 

Cây chứa tới$10^5$nút. Bất kỳ giải pháp nào khám phá mọi đường dẫn từ gốc đến lá một cách độc lập đều bị loại trừ ngay lập tức, bởi vì một cây có kích thước này có thể chứa một số lá tuyến tính và nhiều chuỗi quyết định theo cấp số nhân về độ sâu. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế là người chơi không cố gắng giảm thiểu điểm số của người chơi khác. Quy tắc hòa của họ thực sự là hợp tác sau khi tỷ số của họ được ấn định. 

Hãy xem xét ví dụ nhỏ này:```
1
|
2
```Nếu cả hai người chơi đều thích phòng 1 và phòng 2 thì câu trả lời đơn giản là`(2, 2)`. Không có sự lựa chọn nào cả, vì vậy lý luận của lý thuyết trò chơi vẫn phải bao gồm chính xác mọi phòng được ghé thăm. 

Một sai lầm dễ mắc phải khác là quên quy tắc tie-break.```
    1
   / \
  2   3
```Giả sử điểm cuối cùng của Kuroni là như nhau bất kể cô ấy chọn phòng 2 hay phòng 3. Khi đó cô ấy phải chọn đứa trẻ cho Tfg điểm cuối cùng cao hơn. Một giải pháp chỉ tối đa hóa điểm số của người chơi hiện tại và bỏ qua các mối quan hệ có thể tạo ra đường dẫn sai. 

Cạm bẫy thứ ba là quên rằng điểm số đến từ tất cả các phòng trên con đường đã chọn, không chỉ phòng được chọn ở nước đi hiện tại. Một đứa trẻ hấp dẫn cục bộ có thể dẫn đến một cây con tồi tệ hơn sau này. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xử lý vấn đề như một cây trò chơi hoàn chỉnh. Từ mỗi phòng, chúng tôi thử từng đứa trẻ, đánh giá đệ quy trạng thái trò chơi kết quả và chọn theo thứ tự ưu tiên của người chơi. Điều này đúng vì nó tuân thủ chính xác các quy tắc chơi tối ưu. 

Vấn đề là nhiều trò chơi phụ lặp lại. Giả sử chúng ta đến một căn phòng nào đó$u$và đến lượt Kuroni. Kết quả từ thời điểm đó trở đi chỉ phụ thuộc vào cây con có gốc tại$u$và đến lượt ai. Nó không phụ thuộc vào con đường được sử dụng để đạt được$u$. 

Quan sát đó biến trò chơi thành một bài toán lập trình động cây. 

Xác định trạng thái bằng hai thông tin:```
(current node, whose turn chooses next)
```Đối với mỗi nút, chúng tôi tính toán hai câu trả lời.`dp0[u]`là cặp điểm cuối cùng có thể đạt được từ cây con có gốc tại`u`khi đến lượt Kuroni chọn phòng tiếp theo.`dp1[u]`là cặp tương tự khi đến lượt Tfg. 

Nếu như`u`là một chiếc lá, không còn quyết định nào nữa. Đường đi kết thúc ngay lập tức nên kết quả chỉ đơn giản là sự đóng góp của không gian`u`. 

Đối với một nút nội bộ, người chơi hiện tại chọn một nút con. Phần còn lại của trò chơi chính xác là trạng thái DP tương ứng ở đứa trẻ đó khi chuyển lượt. 

Chi tiết duy nhất còn lại là quy tắc so sánh. 

Khi Kuroni chọn, cô ấy thích điểm Kuroni lớn hơn trước, sau đó là điểm Tfg lớn hơn. 

Khi Tfg chọn, cô ấy thích điểm Tfg lớn hơn trước, sau đó là điểm Kuroni lớn hơn. 

Sau khi chọn được đứa con tốt nhất, chúng tôi thêm phần đóng góp của phòng hiện tại. 

Mỗi nút được xử lý một lần và mỗi cạnh được kiểm tra một lần, đưa ra giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo độ sâu của cây | O(độ sâu) | Quá chậm | 
| DP tối ưu trên cây | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút 1. 
2. Thực hiện duyệt DFS và nhận thứ tự sau để mọi nút con được xử lý trước nút cha của nó. 
3. Đối với nút lá`u`, bộ:```
dp0[u] = dp1[u] = (a[u], b[u])
```Vì không còn nước đi nào nên đường đi chỉ bao gồm`u`. 
4. Tính toán`dp0[u]`. 

Kiểm tra từng đứa trẻ`v`. 

Trò chơi tiếp tục từ`v`đến lượt Tfg nên kết quả ứng cử viên là`dp1[v]`. 

Chọn đứa trẻ có kết quả lớn nhất về mặt từ điển trong:```
(Kuroni score, Tfg score)
```Sau khi chọn con tốt nhất, hãy thêm sự đóng góp của nút`u`. 
5. Tính toán`dp1[u]`. 

Kiểm tra từng đứa trẻ`v`. 

Trò chơi tiếp tục từ`v`đến lượt của Kuroni nên kết quả của ứng cử viên là`dp0[v]`. 

Chọn đứa trẻ có kết quả lớn nhất về mặt từ điển trong:```
(Tfg score, Kuroni score)
```Sau khi chọn con tốt nhất, hãy thêm sự đóng góp của nút`u`. 
6. Câu trả lời là`dp0[1]`, vì trò chơi bắt đầu ở phòng 1 và Kuroni là người đưa ra quyết định đầu tiên. 

### Tại sao nó hoạt động 

Đối với bất kỳ nút nào, một khi chúng ta biết lượt của mình, trò chơi trong tương lai chỉ phụ thuộc vào cây con đó. Điều này mang lại cấu trúc nền tối ưu. 

Giả sử tất cả trẻ em đều có giá trị DP chính xác. 

Nếu đến lượt Kuroni, mỗi nước đi đúng luật đều tương ứng với việc chọn một đứa trẻ. Điểm kết quả chính xác là điểm được lưu trữ trong trạng thái của đứa trẻ đó. Quy tắc của Kuroni nói rằng cô ấy chọn kết quả tối đa hóa điểm số của chính mình, với điểm của Tfg chỉ được sử dụng làm điểm hòa. Việc chọn đứa trẻ giỏi nhất theo thứ tự đó khớp chính xác với định nghĩa của trò chơi. 

Lập luận tương tự cũng áp dụng cho lượt của Tfg. 

Vì các lá là đúng và mọi nút cha đều được tính từ các nút con đã đúng, quy nạp về kích thước cây con chứng minh rằng mọi trạng thái DP đều đúng. Giá trị gốc là kết quả cuối cùng của trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

a = list(map(int, input().split()))
b = list(map(int, input().split()))

g = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

parent = [-1] * n
order = [0]
parent[0] = 0

for u in order:
    for v in g[u]:
        if parent[v] == -1:
            parent[v] = u
            order.append(v)

dp0 = [(0, 0)] * n
dp1 = [(0, 0)] * n

for u in reversed(order):
    children = [v for v in g[u] if v != parent[u]]

    if not children:
        dp0[u] = (a[u], b[u])
        dp1[u] = (a[u], b[u])
        continue

    best_k = None
    for v in children:
        cand = dp1[v]
        if best_k is None or (cand[0], cand[1]) > (best_k[0], best_k[1]):
            best_k = cand

    dp0[u] = (best_k[0] + a[u], best_k[1] + b[u])

    best_t = None
    for v in children:
        cand = dp0[v]
        if best_t is None or (cand[1], cand[0]) > (best_t[1], best_t[0]):
            best_t = cand

    dp1[u] = (best_t[0] + a[u], best_t[1] + b[u])

ans = dp0[0]
print(ans[0], ans[1])
```DFS đầu tiên xây dựng các mối quan hệ cha mẹ và thứ tự truyền tải. Việc đảo ngược thứ tự đó sẽ tạo ra một chuỗi xử lý giống như thứ tự sau, trong đó mọi phần tử con đều được xử lý trước phần tử cha của nó. 

Hai mảng DP lưu trữ các cặp`(kuroni_score, tfg_score)`. 

Vì`dp0`, chúng tôi so sánh kết quả của trẻ một cách trực tiếp như`(kuroni, tfg)`bởi vì thứ tự ưu tiên của Kuroni tuân theo chính xác thứ tự từ điển đó. 

Vì`dp1`, chúng ta so sánh như`(tfg, kuroni)`vì điểm của Tfg là mục tiêu chính. 

Việc triển khai sử dụng truyền tải lặp thay vì DFS đệ quy. Với$10^5$các nút, độ sâu đệ quy có thể vượt quá giới hạn đệ quy mặc định của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
0 1 1 0 1
0 0 1 1 1
1 2
1 3
2 4
2 5
```Chế biến từ lá trở lên: 

| Nút | dp0 | dp1 | 
| --- | --- | --- | 
| 3 | (1,1) | (1,1) | 
| 4 | (0,1) | (0,1) | 
| 5 | (1,1) | (1,1) | 
| 2 | (2,1) | (2,1) | 
| 1 | (2,1) | (2,2) | 

Tại nút 2, cả hai lượt chọn phòng 5 vì`(1,1)`thống trị`(0,1)`. 

Tại nút 1, Kuroni so sánh kết quả của trẻ`(2,1)`Và`(1,1)`, chọn cái đầu tiên. Câu trả lời cuối cùng là:```
2 1
```Điều này chứng tỏ rằng các quyết định dựa trên kết quả hoàn toàn trong tương lai chứ không phải giá trị phòng ngay lập tức. 

### Ví dụ 2 

đầu vào:```
3
0 1 1
0 0 1
1 2
1 3
```Giá trị DP: 

| Nút | dp0 | dp1 | 
| --- | --- | --- | 
| 2 | (1,0) | (1,0) | 
| 3 | (1,1) | (1,1) | 
| 1 | (1,1) | (1,1) | 

Kuroni so sánh`(1,0)`Và`(1,1)`. 

Tỉ số của cô ấy là hòa nên sẽ chọn tie-break`(1,1)`. 

Câu trả lời cuối cùng:```
1 1
```Ví dụ này tách biệt quy tắc so sánh thứ cấp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và cạnh được xử lý với số lần không đổi | 
| Không gian | O(n) | Danh sách kề, mảng cha, thứ tự truyền tải và mảng DP | 

Với$n \le 10^5$, thời gian tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng tuyến tính và phù hợp thoải mái trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    g = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    order = [0]
    parent[0] = 0

    for u in order:
        for v in g[u]:
            if parent[v] == -1:
                parent[v] = u
                order.append(v)

    dp0 = [(0, 0)] * n
    dp1 = [(0, 0)] * n

    for u in reversed(order):
        children = [v for v in g[u] if v != parent[u]]

        if not children:
            dp0[u] = (a[u], b[u])
            dp1[u] = (a[u], b[u])
            continue

        best_k = None
        for v in children:
            cand = dp1[v]
            if best_k is None or (cand[0], cand[1]) > (best_k[0], best_k[1]):
                best_k = cand

        dp0[u] = (best_k[0] + a[u], best_k[1] + b[u])

        best_t = None
        for v in children:
            cand = dp0[v]
            if best_t is None or (cand[1], cand[0]) > (best_t[1], best_t[0]):
                best_t = cand

        dp1[u] = (best_t[0] + a[u], best_t[1] + b[u])

    return f"{dp0[0][0]} {dp0[0][1]}\n"

# sample
assert run(
"""5
0 1 1 0 1
0 0 1 1 1
1 2
1 3
2 4
2 5
"""
) == "2 1\n"

# minimum size
assert run(
"""1
1
0
"""
) == "1 0\n"

# tie-break by other player's score
assert run(
"""3
0 1 1
0 0 1
1 2
1 3
"""
) == "1 1\n"

# chain
assert run(
"""4
1 0 1 0
0 1 0 1
1 2
2 3
3 4
"""
) == "2 2\n"

# all zero preferences
assert run(
"""3
0 0 0
0 0 0
1 2
1 3
"""
) == "0 0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây nút đơn |`1 0`| Trường hợp cơ bản không có động thái | 
| Ví dụ về tie-break |`1 1`| Quy tắc so sánh thứ cấp | 
| Cây xích |`2 2`| Không phân nhánh, đường dẫn bắt buộc | 
| Tất cả số không |`0 0`| Điểm tích lũy chính xác ngay cả khi trống | 
| Trường hợp mẫu |`2 1`| Tính đúng đắn chung | 

## Vỏ cạnh 

Xét một cây chỉ có gốc:```
1
1
0
```Gốc cũng là một chiếc lá. Thuật toán gán ngay:```
dp0[1] = dp1[1] = (1, 0)
```Không có quyết định nào được đưa ra, và câu trả lời là`(1, 0)`. 

Bây giờ hãy xem xét một chiếc cà vạt thuần túy:```
3
0 1 1
0 0 1
1 2
1 3
```Kết quả của trẻ là`(1,0)`Và`(1,1)`. 

Điểm chính của Kuroni là 1 trong cả hai trường hợp. Thuật toán so sánh theo từ điển như`(Kuroni, Tfg)`và chọn`(1,1)`. Điều này hoàn toàn phù hợp với quy tắc tie-break của bài toán. 

Cuối cùng, hãy xem xét một cây có hình đường dẫn:```
4
1 0 1 0
0 1 0 1
1 2
2 3
3 4
```Mỗi nút có nhiều nhất một nút con. Không có sự lựa chọn tồn tại ở bất cứ đâu. DP chỉ đơn giản là tích lũy các khoản đóng góp phòng từ lá trở lên, tạo ra`(2,2)`. Điều này xác nhận rằng thuật toán hoạt động chính xác ngay cả khi khía cạnh trò chơi biến mất và đường dẫn bị ép buộc.
