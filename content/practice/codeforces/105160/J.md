---
title: "CF 105160J - \u4e0a\u5b66"
description: "Chúng ta có một cây có các nút được gắn nhãn từ 1 đến n, cộng thêm một nút 0. Nút 0 được kết nối với nút 1, do đó, nút 0 hoạt động giống như một nút gốc được gắn phía trên cây ban đầu. Mỗi cạnh còn lại kết nối n vị trí của học sinh thành một cây. Mỗi học sinh sống tại một nút duy nhất i."
date: "2026-06-27T11:02:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "J"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 69
verified: true
draft: false
---

[CF 105160J - \u4e0a\u5b66](https://codeforces.com/problemset/problem/105160/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có các nút được gắn nhãn từ 1 đến n, cộng thêm một nút 0. Nút 0 được kết nối với nút 1, do đó, nút 0 hoạt động giống như một nút gốc được gắn phía trên cây ban đầu. Mỗi cạnh còn lại kết nối n vị trí của học sinh thành một cây. 

Mỗi học sinh sống tại một nút duy nhất i. Tuyến xe buýt là một con đường không bao giờ lặp lại một cạnh và luôn kết thúc ở nút 0. Học sinh chỉ có thể lên xe buýt tại nút của chính mình và sau khi lên xe, họ sẽ đi dọc theo xe buýt đó cho đến khi đến nút 0. 

Bởi vì biểu đồ là một cây, nên bất kỳ tuyến đường đơn biên nào kết thúc tại nút 0 đều hoạt động giống như một đường đi từ trên xuống về phía gốc: nó không thể phân nhánh và không thể xem lại các cạnh, do đó, nó tương ứng một cách hiệu quả với việc chọn nút bắt đầu và đi dọc theo con đường đơn giản duy nhất hướng tới nút 0. 

Một hệ quả quan trọng là một xe buýt có thể phục vụ nhiều học sinh khi và chỉ khi tất cả những học sinh đó nằm trên một đường dẫn gốc duy nhất, nghĩa là các nút của chúng được sắp xếp trên một chuỗi tổ tiên trong cây có gốc (bắt nguồn từ nút 0). 

Chúng ta được yêu cầu đếm có bao nhiêu cách chọn 3 học sinh sao cho không phải tất cả các em đều được phục vụ bởi một chiếc xe buýt. Nói cách khác, chúng tôi đếm tất cả các bộ ba nút ngoại trừ những nút có ba nút nằm trên một đường dẫn từ gốc đến lá. 

Ràng buộc n lên tới 2 × 10^5 ngụ ý rằng mọi nghiệm đều phải gần tuyến tính hoặc O(n log n). Bất cứ điều gì bậc hai trên các cặp hoặc bộ ba nút sẽ quá chậm. Ngay cả các giải pháp kiểu O(n^{4/3}) ở đây cũng không cần thiết vì cấu trúc là một cây và đề xuất số lượng tổ hợp đơn giản sau khi xử lý trước. 

Một trường hợp phức tạp phát sinh từ sự hiểu lầm “cùng một chiếc xe buýt” nghĩa là gì. Đó không phải là về khả năng kết nối trong cây ban đầu mà là về việc nằm trên một chuỗi tổ tiên. Ví dụ: trong một ngôi sao có tâm ở nút 1, các nút 2, 3, 4 không nằm trên một đường dẫn duy nhất, do đó, bất kỳ bộ ba nào liên quan đến chúng đều hợp lệ. Ngược lại, trong chuỗi 4-3-2-1-0, ba nút bất kỳ dọc theo chuỗi đó đều không hợp lệ vì một bus từ 4 đến 0 đi qua tất cả chúng. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê tất cả các bộ ba nút và kiểm tra xem chúng có nằm trên một đường dẫn từ gốc đến 0 hay không. Việc kiểm tra tình trạng này yêu cầu xác minh xem một nút có phải là nút sâu nhất trong số chúng hay không và liệu hai nút còn lại có phải là tổ tiên của nút đó hay không. Điều này có thể được thực hiện bằng cách sử dụng con trỏ cha hoặc truy vấn LCA trong O(1) hoặc O(log n), nhưng việc liệt kê tất cả các bộ ba tốn O(n^3), điều này là không thể với n = 2 × 10^5. 

Quan sát quan trọng là lật ngược quan điểm. Thay vì kiểm tra bộ ba, chúng tôi đếm những cái xấu. Bộ ba là xấu khi cả ba nút đều nằm trên một chuỗi tổ tiên duy nhất trong cây có gốc. 

Bây giờ hãy sửa nút v là nút sâu nhất trong bộ ba như vậy. Hai nút còn lại phải được chọn trong số các nút tổ tiên của v (loại trừ chính v hoặc bao gồm nó một cách cẩn thận tùy theo công thức). Khi chúng tôi xác định độ sâu một cách chính xác, mỗi bộ ba xấu sẽ có một nút sâu nhất duy nhất, điều này tránh được việc tính hai lần. 

Điều này làm giảm vấn đề thành tổng tổ hợp thuần túy trên các nút sau khi tính toán độ sâu với một DFS hoặc BFS duy nhất từ nút 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu tăng gấp ba | O(n^3) | O(1) | Quá chậm | 
| Đếm độ sâu trên cây | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây tại nút 0 và tính độ sâu của mỗi nút, trong đó độ sâu[v] là số nút sinh viên trên đường đi từ v đến nút 0, bao gồm cả chính v.

1. Xây dựng danh sách kề của cây, bao gồm nút 0 nối với nút 1. 
2. Chạy BFS hoặc DFS từ nút 0 để tính toán độ sâu và mối quan hệ gốc. Chúng ta đặt độ sâu [0] = 0 và đối với bất kỳ u con nào của v, độ sâu [u] = độ sâu [v] + 1. Điều này đảm bảo độ sâu [v] đếm có bao nhiêu nút sinh viên nằm trên đường từ v đến phía gốc ngoại trừ nút 0. 
3. Tính trước tổng số bộ ba học sinh là C(n, 3). Điều này đại diện cho tất cả các lựa chọn có thể. 
4. Tính số bộ ba xấu. Với mỗi nút v, coi v là nút sâu nhất trong bộ ba. Sau đó, hai nút còn lại phải được chọn trong số các nút tổ tiên của v cũng là các nút sinh viên, trong đó có độ sâu [v] - 1. 

Vì vậy, sự đóng góp của v là C(độ sâu[v] - 1, 2), với điều kiện là độ sâu [v] ≥ 3. 
5. Tính tổng giá trị này trên tất cả các nút v. 
6. Câu trả lời là tổng số bộ ba trừ đi bộ ba xấu. 

Lý do sự phân tách này hoạt động là vì mọi bộ ba “cùng một bus” hợp lệ tương ứng với chính xác một nút sâu nhất theo thứ tự cây. Khi nút đó được cố định, hai nút còn lại bị ràng buộc duy nhất nằm phía trên nút đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
adj = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

# include node 0 connected to 1
adj.append([])  # index n+1 unused (just safety, not needed)
adj[0] = [1]
adj[1].append(0)

parent = [-1] * (n + 1)
depth = [0] * (n + 1)

stack = [0]
parent[0] = 0

while stack:
    v = stack.pop()
    for u in adj[v]:
        if u == parent[v]:
            continue
        parent[u] = v
        depth[u] = depth[v] + 1
        stack.append(u)

def C2(x):
    return x * (x - 1) // 2

total = n * (n - 1) * (n - 2) // 6
bad = 0

for v in range(1, n + 1):
    if depth[v] >= 3:
        bad += C2(depth[v] - 1)

print(total - bad)
```Đầu tiên, mã sẽ xây dựng cây và thêm rõ ràng kết nối giữa nút 0 và nút 1. DFS tính toán các mảng cha và mảng sâu bắt nguồn từ nút 0. 

Hàm C2(x) tính toán các hệ số nhị thức cho các cặp một cách hiệu quả. Đối với mỗi nút v, chúng tôi hiểu độ sâu [v] - 1 là số lượng tổ tiên hợp lệ trong số các nút sinh viên ở trên v. Việc chọn bất kỳ hai nút nào trong số chúng cùng với v sẽ tạo thành một bộ ba xấu. 

Cuối cùng, chúng tôi trừ những thứ này khỏi tổng số bộ ba. 

Một sai lầm phổ biến là xác định sai độ sâu: nếu độ sâu bắt đầu từ nút 1 thay vì nút 0 thì công thức tổ hợp sẽ bị hỏng. Một vấn đề khác là quên rằng nút 0 không phải là sinh viên và không được đưa vào các kết hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi đơn giản: 3-2-1-0. 

Độ sâu là: 

nút 1: 1, nút 2: 2, nút 3: 3. 

| v | độ sâu[v] | đóng góp xấu C(độ sâu[v]-1,2) | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| 3 | 3 | 1 | 

Tổng số bộ ba là C(3,3) = 1. 

Bộ ba xấu = 1 (bộ ba {1,2,3}). 

Trả lời = 0. 

Điều này xác nhận rằng trong một chuỗi tất cả các bộ ba đều không hợp lệ vì chúng nằm trên một đường đi. 

### Ví dụ 2 

Hãy xem xét một ngôi sao: nút 1 được kết nối với 0 và các nút 2, 3, 4 được kết nối với 1. 

Độ sâu: 

nút 1 = 1, nút 2,3,4 = 2. 

| v | độ sâu[v] | đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| 3 | 2 | 0 | 
| 4 | 2 | 0 | 

Tổng số bộ ba = C(4,3) = 4. 

Bộ ba xấu = 0. 

Đáp án = 4. 

Điều này cho thấy rằng trong cấu trúc phân nhánh, không có ba nút nào nằm trên một chuỗi tổ tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một DFS cho độ sâu và một nút truyền tuyến tính | 
| Không gian | O(n) | Danh sách kề và mảng phụ | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì cả bộ nhớ và thời gian chạy đều có quy mô tuyến tính với số lượng nút và n có thể lớn tới 2 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import comb

    n = int(_sys.stdin.readline())
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, _sys.stdin.readline().split())
        adj[u].append(v)
        adj[v].append(u)

    adj.append([])
    adj[0] = [1]
    adj[1].append(0)

    parent = [-1] * (n + 1)
    depth = [0] * (n + 1)

    stack = [0]
    parent[0] = 0

    while stack:
        v = stack.pop()
        for u in adj[v]:
            if u == parent[v]:
```
