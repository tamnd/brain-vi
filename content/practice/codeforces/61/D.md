---
title: "CF 61D - Chiến Thắng Vĩnh Cửu"
description: "Chúng ta được yêu cầu tìm khoảng cách tối thiểu mà Shapur phải đi để đến thăm tất cả các thành phố ít nhất một lần. Các thành phố được kết nối theo cấu trúc cây, nghĩa là có chính xác $n-1$ đường hai chiều và một con đường duy nhất giữa hai thành phố bất kỳ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "greedy", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 61
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 57 (Div. 2)"
rating: 1800
weight: 61
solve_time_s: 89
verified: true
draft: false
---

[CF 61D - Chiến thắng vĩnh cửu](https://codeforces.com/problemset/problem/61/D) 

**Đánh giá:** 1800 
**Thẻ:** dfs và tương tự, đồ thị, tham lam, đường đi ngắn nhất, cây cối 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm khoảng cách tối thiểu mà Shapur phải đi để đến thăm tất cả các thành phố ít nhất một lần. Các thành phố được kết nối theo cấu trúc cây, nghĩa là có chính xác$n-1$đường hai chiều và đường đi duy nhất giữa hai thành phố bất kỳ. Shapur bắt đầu ở thành phố 1 và có thể kết thúc ở bất kỳ thành phố nào. Mỗi con đường có một trọng lượng, đại diện cho chiều dài của nó. Đầu vào cho biết số lượng thành phố$n$và sau đó$n-1$bộ ba mô tả các con đường với trọng lượng của chúng. Đầu ra là một số nguyên duy nhất: tổng khoảng cách di chuyển tối thiểu. 

Bởi vì$n$có thể lớn như$10^5$, bất kỳ giải pháp nào tệ hơn tuyến tính hoặc gần tuyến tính về số lượng thành phố và đường sẽ quá chậm. Một giải pháp ngây thơ thử tất cả các lộ trình hoặc hoán vị có thể rõ ràng là không khả thi vì số lượng hoán vị tăng theo giai thừa. Chúng ta cần khai thác cấu trúc cây để giảm số lần tính toán. 

Trường hợp cạnh không rõ ràng là một cây có đường đi nặng nhất nằm trong một chuỗi dài từ thành phố 1 đến một chiếc lá. Ví dụ: với ba thành phố trên một đường thẳng: thành phố 1 kết nối với 2 với trọng số 10 và 2 kết nối với 3 với trọng số 20. Chiến lược tối ưu là đi bộ 1 → 2 → 3 và dừng lại ở 3. Một cách tiếp cận bất cẩn cho rằng Shapur phải quay lại thành phố 1 sẽ nhân đôi một số trọng số cạnh một cách không chính xác, cho kết quả là 60 thay vì 30. 

Một trường hợp tinh tế khác là khi cây có hình ngôi sao, tất cả các thành phố được kết nối trực tiếp với thành phố 1. Ở đây, đường đi tối ưu chỉ có thể đi qua mỗi cạnh một lần mà không quay trở lại, do đó, tổng tất cả các trọng số của cạnh sẽ cho câu trả lời. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các đường đi bắt đầu từ thành phố 1 ghé thăm mọi thành phố chính xác một lần, tính toán khoảng cách của chúng và lấy mức tối thiểu. Mặc dù điều này đúng, nhưng số lượng đường đi có thể có trong cây là giai thừa của số lượng thành phố có hình dạng không tầm thường, điều này trở nên không khả thi đối với$n$lớn như$10^5$. 

Quan sát quan trọng là các thành phố tạo thành một cái cây. Để tham quan tất cả các thành phố, mỗi cạnh phải được đi qua ít nhất một lần, vì việc bỏ qua một cạnh sẽ khiến các thành phố được kết nối với nó không được ghé thăm. Nếu chúng ta bỏ qua thành phố xuất phát, phương pháp đơn giản sẽ đi qua mỗi cạnh hai lần: một lần để đi sâu hơn, một lần để quay lại. Điều này tính mỗi cạnh là gấp đôi trọng lượng của nó. Cách duy nhất để tiết kiệm khoảng cách là tránh quay lại trên con đường đơn dài nhất từ ​​thành phố 1, đây sẽ là con đường từ thành phố 1 đến lá xa nhất. Đi qua con đường dài nhất chỉ một lần thay vì hai lần sẽ có tổng khoảng cách tối thiểu. 

Do đó, cách tiếp cận tối ưu là tính tổng tất cả các trọng số của cạnh nhân với 2 rồi trừ đi khoảng cách tối đa từ thành phố 1 đến bất kỳ lá nào. DFS được sử dụng để tính toán khoảng cách tối đa này một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích đầu vào để đọc$n$và$n-1$các cạnh. Lưu trữ các cạnh trong danh sách kề với cả trọng số cạnh và trọng số cạnh. Đại diện này cho phép di chuyển nhanh chóng từ bất kỳ thành phố nào. 
2. Tính tổng trọng số của tất cả các cạnh. Vì mỗi cạnh phải được đi qua ít nhất một lần theo cả hai hướng ngoại trừ đường đi mà chúng ta tránh quay lui, tổng này nhân với 2 sẽ cho tổng hành trình ngây thơ nếu chúng ta quay lại điểm xuất phát sau khi ghé thăm từng nhánh. 
3. Thực hiện tìm kiếm theo chiều sâu bắt đầu từ thành phố 1 để tìm khoảng cách tối đa từ thành phố 1 đến lá bất kỳ. Khởi tạo bộ tích lũy khoảng cách thành 0, sau đó đối với mỗi hàng xóm chưa ghé thăm, hãy tính toán đệ quy độ dài đường dẫn và theo dõi mức tối đa. 
4. Trừ khoảng cách tối đa thu được từ tổng gấp đôi để có khoảng cách di chuyển tối thiểu. Điều này hiệu quả vì dọc theo đường đi từ gốc đến lá xa nhất, chúng ta không cần phải quay lại các cạnh; tất cả các cạnh khác được tính hai lần. 

Tại sao nó hoạt động: Mỗi cạnh phải được duyệt qua ít nhất một lần để đảm bảo tất cả các thành phố đều được ghé thăm. Đếm mỗi cạnh hai lần ban đầu sẽ tính đến việc đi ra và quay lại. Bằng cách xác định đường đi đơn dài nhất từ ​​thành phố xuất phát, chúng ta có thể tránh được việc đi qua các cạnh đó. DFS đảm bảo rằng chúng tôi tìm thấy chính xác đường đi này theo thời gian tuyến tính và việc trừ độ dài đường đi tối đa này đảm bảo việc di chuyển tối thiểu mà không bỏ sót bất kỳ thành phố nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(200000)

n = int(input())
adj = [[] for _ in range(n + 1)]
total_weight = 0

for _ in range(n - 1):
    u, v, w = map(int, input().split())
    adj[u].append((v, w))
    adj[v].append((u, w))
    total_weight += w

visited = [False] * (n + 1)
max_path = 0

def dfs(node, dist):
    global max_path
    visited[node] = True
    if dist > max_path:
        max_path = dist
    for neighbor, weight in adj[node]:
        if not visited[neighbor]:
            dfs(neighbor, dist + weight)

dfs(1, 0)

result = 2 * total_weight - max_path
print(result)
```Danh sách kề được chọn để truyền tải DFS hiệu quả. Biến toàn cục`max_path`theo dõi khoảng cách tối đa. DFS tích lũy khoảng cách theo cách đệ quy và đảm bảo mỗi nút được truy cập một lần. Chúng tôi nhân tổng trọng lượng lên hai vì ban đầu chúng tôi cho rằng chúng tôi đi đến từng thành phố và quay lại. Trừ`max_path`tránh việc tính hai lần đường đi dài nhất từ ​​gốc tới lá. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2 3
2 3 4
```| Bước | Nút | Quận | max_path | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | Bắt đầu DFS | 
| 2 | 2 | 3 | 3 | Thăm hàng xóm 2 | 
| 3 | 3 | 7 | 7 | Thăm hàng xóm 3 | 

Tổng trọng lượng = 3 + 4 = 7. Nhân với 2 → 14. Trừ max_path 7 → 7. Đầu ra là 7. Xác nhận đạt được quãng đường tối thiểu bằng cách đi bộ 1→2→3 và dừng lại. 

### Tùy chỉnh đầu vào 2```
5
1 2 1
1 3 2
3 4 3
3 5 4
```| Bước | Nút | Quận | max_path | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | Bắt đầu DFS | 
| 2 | 2 | 1 | 1 | Thăm hàng xóm 2 | 
| 3 | 3 | 2 | 2 | Thăm hàng xóm 3 | 
| 4 | 4 | 5 | 5 | Thăm hàng xóm 4 | 
| 5 | 5 | 6 | 6 | Thăm hàng xóm 5 | 

Tổng trọng số = 1+2+3+4=10 → nhân đôi = 20. Trừ max_path 6 → 14. Đường dẫn tối ưu 1→3→5, sau đó quay lại 3→4→3→1→2. Xác nhận thuật toán xử lý các đường phân nhánh một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | DFS truy cập từng nút và cạnh chính xác một lần | 
| Không gian | O(n) | Danh sách kề và mảng đã thăm chiếm không gian tuyến tính | 

Với những hạn chế của$n \le 10^5$, thuật toán tuyến tính hiệu quả và phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    adj = [[] for _ in range(n + 1)]
    total_weight = 0
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))
        total_weight += w

    visited = [False] * (n + 1)
    max_path = 0

    def dfs(node, dist):
        nonlocal max_path
        visited[node] = True
        if dist > max_path:
            max_path = dist
        for neighbor, weight in adj[node]:
            if not visited[neighbor]:
                dfs(neighbor, dist + weight)

    dfs(1, 0)
    return str(2 * total_weight - max_path)

# provided sample
assert run("3\n1 2 3\n2 3 4\n") == "7", "sample 1"

# custom tests
assert run("1\n") == "0", "single node"
assert run("2\n1 2 10\n") == "10", "two nodes only"
assert run("4\n1 2 1\n2 3 2\n3 4 3\n") == "6", "linear chain"
assert run("5\n1 2 1\n1 3 2\n3 4 3\n3 5 4\n") == "14", "branching tree"
assert run("3\n1 2 0\n2 3 0\n") == "0", "zero-weight edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| | | |
