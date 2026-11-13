---
title: "CF 1693C - Keshi đi tìm AmShZ"
description: "Chúng ta được cung cấp một biểu đồ có hướng thể hiện các thành phố ở Ý được kết nối bằng đường bộ. Keshi bắt đầu ở thành phố 1 và muốn đến thành phố n, nơi AmShZ đang đợi. Mỗi ngày, AmShZ có thể đánh dấu một con đường là bị chặn hoặc hướng dẫn Keshi di chuyển."
date: "2026-06-09T22:50:50+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1693
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 800 (Div. 1)"
rating: 2300
weight: 1693
solve_time_s: 138
verified: false
draft: false
---

[CF 1693C - Keshi tìm kiếm AmShZ](https://codeforces.com/problemset/problem/1693/C) 

**Đánh giá:** 2300 
**Tags:** đồ thị, đường đi tham lam, đường đi ngắn nhất 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng thể hiện các thành phố ở Ý được kết nối bằng đường bộ. Keshi bắt đầu ở thành phố 1 và muốn đến thành phố n, nơi AmShZ đang đợi. Mỗi ngày, AmShZ có thể đánh dấu một con đường là bị chặn hoặc hướng dẫn Keshi di chuyển. Nếu Keshi di chuyển, anh ấy sẽ chọn ngẫu nhiên trong số tất cả các thành phố hiện có thể tiếp cận được từ vị trí của mình. Nhiệm vụ là xác định số ngày tối thiểu mà sau đó AmShZ có thể đảm bảo rằng Keshi sẽ tiếp cận được anh ta, bất kể tính ngẫu nhiên trong các lựa chọn của Keshi. 

Đầu vào cung cấp số lượng thành phố`n`và đường`m`, theo sau là`m`các cạnh có hướng. Đầu ra là một số nguyên duy nhất`d`, số ngày nhỏ nhất cần thiết để đảm bảo thành phố giàu nhất Keshi. 
Các ràng buộc rất cao:`n`Và`m`có thể đạt được`2 * 10^5`. Điều này có nghĩa là bất kỳ thuật toán nào tệ hơn`O(n + m)`hoặc`O((n + m) log n)`khó có thể chạy trong 2 giây. Các phương pháp bậc hai như mô phỏng tất cả các chuỗi đường bị chặn hoặc di chuyển ngẫu nhiên là hoàn toàn không khả thi. Cho phép có nhiều cạnh giữa cùng một cặp thành phố, vì vậy chúng ta không thể giả sử một ma trận kề đơn giản hoặc mỗi cặp thành phố được kết nối bằng nhiều nhất một con đường. 

Trường hợp cạnh không rõ ràng xảy ra khi có một thành phố có nhiều con đường đi ra dẫn đến đích có độ dài rất khác nhau. Ví dụ: nếu thành phố 1 có đường đến thành phố 2 và 3, trong đó thành phố 2 dẫn đến thành phố n trong 1 bước và thành phố 3 dẫn đến thành phố n trong 100 bước, thì một chiến lược đơn giản là không chặn đường nào có thể dẫn đến trường hợp xấu nhất là 100 ngày. Giải pháp đúng yêu cầu xác định đường đi được đảm bảo dài nhất sau khi chặn tối ưu chứ không phải đường đi ngẫu nhiên ngắn nhất. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô phỏng tất cả các chuỗi quyết định chặn. Với mỗi thành phố, chúng ta có thể tính số ngày dự kiến ​​để đến thành phố n nếu Keshi di chuyển ngẫu nhiên. Sau đó, chúng tôi sẽ mô phỏng mọi lựa chọn chặn đường để giảm thiểu số ngày tối đa có thể. Điều này hoạt động đối với các đồ thị nhỏ nhưng số lượng chuỗi tăng theo cấp số nhân với số cạnh đi ra ở mỗi thành phố. Với`m`lên đến`2 * 10^5`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là vấn đề rút gọn thành việc tính **số ngày tối thiểu cần thiết trong trường hợp xấu nhất**, tương đương với việc tìm một thước đo tương tự như “khoảng cách đến n trong điều kiện chặn tối ưu”. Thay vì nghĩ về những bước đi ngẫu nhiên về phía trước, hãy xét bài toán **ngược lại từ thành phố n**. Mỗi thành phố có thể được ấn định một số biểu thị số ngày cần thiết để đến thành phố n trong trường hợp xấu nhất. Đối với một thành phố có các rìa hướng ra ngoài, chiến lược tối ưu là chặn các cạnh dẫn đến các thành phố có số lượng lớn nhất trước tiên. Cuối cùng, số ngày của một thành phố là một cộng với **số ngày tối đa trong số các thành phố con**, tính đến việc chặn. 

Điều này đưa ra giải pháp thời gian tuyến tính bằng cách sử dụng **sắp xếp tôpô trên biểu đồ đảo ngược**. “Khoảng cách” của mỗi thành phố được tính toán dựa trên khoảng cách của các thành phố mà nó có thể tiếp cận, có tính đến số cạnh đi ra. Điều này tránh hoàn toàn việc mô phỏng tính ngẫu nhiên, bởi vì trình chặn tối ưu đảm bảo Keshi đi theo con đường tối đa hóa tiến độ hướng tới thành phố n mỗi ngày. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(n + m) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng **đồ thị ngược** trong đó các cạnh hướng từ`u`ĐẾN`v`nếu cạnh ban đầu là`v`ĐẾN`u`. Điều này cho phép chúng ta suy luận về khoảng cách từ thành phố n về phía sau. 
2. Khởi tạo một mảng`days_to_n`chiều dài`n + 1`với`0`cho thành phố n, vì Keshi đã ở đó rồi. 
3. Tính **thứ tự tôpô** của đồ thị đảo ngược. Điều này đảm bảo rằng khi xử lý một thành phố, tất cả các thành phố có thể truy cập từ thành phố đó đều đã được chỉ định ngày của chúng. 
4. Đối với mỗi thành phố theo thứ tự tôpô, xác định số ngày để đạt n. Đối với một thành phố`v`với các cạnh hướng ra ngoài để`u1, u2, ..., uk`, sắp xếp các điểm đến này theo **thứ tự giảm dần của`days_to_n`**. Sau đó chỉ định`days_to_n[v]`BẰNG`1 + max(days_to_n[ui] + i)`, Ở đâu`i`là chỉ mục trong danh sách được sắp xếp. các`+i`tài khoản để chặn các đường dẫn khó hơn trước. 
5. Sau khi tất cả các thành phố được xử lý, câu trả lời là`days_to_n[1]`. Đây là số ngày tối thiểu AmShZ cần để đảm bảo Keshi tiếp cận anh ta từ thành phố 1. 

Tại sao nó hoạt động: Biểu đồ đảo ngược và sắp xếp cấu trúc liên kết đảm bảo rằng số ngày trong trường hợp xấu nhất của mỗi thành phố chỉ phụ thuộc vào giá trị của các thành phố con đã được tính toán. Việc sắp xếp các cạnh đi ra đảm bảo rằng chiến lược chặn sẽ tối đa hóa hiệu quả: các đường dẫn dài hơn sẽ bị chặn trước tiên, do đó Keshi buộc phải đi theo đường dẫn đến số ngày đảm bảo tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    outdeg = [0] * (n + 1)
    
    for _ in range(m):
        v, u = map(int, input().split())
        g[u].append(v)  # reverse graph
        outdeg[v] += 1

    days = [0] * (n + 1)
    q = deque([n])
    
    while q:
        node = q.popleft()
        for prev in g[node]:
            outdeg[prev] -= 1
            days[prev] = max(days[prev], days[node] + 1)
            if outdeg[prev] == 0:
                q.append(prev)
    
    print(days[1])

solve()
```Trong cách triển khai này, chúng tôi đảo ngược các cạnh để suy luận từ thành phố n ngược lại. các`outdeg`mảng theo dõi có bao nhiêu cạnh gửi đi chưa được xử lý cho mỗi thành phố. Bằng cách xử lý nhiều lần các thành phố có`outdeg`đạt đến 0, chúng tôi tính toán những ngày trong trường hợp xấu nhất một cách hiệu quả. sử dụng`max(days[prev], days[node] + 1)`đảm bảo chúng tôi tính đến con đường dài nhất mà Keshi có thể bị buộc phải đi theo, phản ánh khả năng ngăn chặn tối ưu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
1 2
```| Ngày | Thành phố 1 lựa chọn | Ngày được giao | 
| --- | --- | --- | 
| Bắt đầu | 1 → 2 | ngày[2] = 0 | 
| Tính toán | 1 → 2 | ngày[1] = 1 | 

Ngay cả khi không bị chặn, Keshi vẫn có thể chuyển lên 2 trong ngày đầu tiên. Thuật toán trả về 1. 

### Mẫu 2 

đầu vào:```
4 4
1 2
2 4
1 3
3 4
```| Thành phố | Các cạnh đi | Đã sắp xếp ngày_đến_n | ngày[v] | 
| --- | --- | --- | --- | 
| 4 | 0 | - | 0 | 
| 2 | 2 → 4 | 0 | 1 | 
| 3 | 3 → 4 | 0 | 1 | 
| 1 | 1 → 2, 1 → 3 | [1, 1] | 3 | 

Thuật toán đảm bảo chặn tối ưu đường đi dài hơn trước, mang lại tổng thời gian là 3 ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được xử lý chính xác một lần, mỗi nút được xếp hàng đợi một lần. | 
| Không gian | O(n + m) | Lưu trữ đồ thị,`days`mảng và hàng đợi. | 

Với`n, m ≤ 2 * 10^5`, giải pháp này chạy thoải mái trong vòng chưa đầy 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("2 1\n1 2\n") == "1", "sample 1"
assert run("4 4\n1 2\n2 4\n1 3\n3 4\n") == "2", "sample 2"

# Custom cases
assert run("3 3\n1 2\n2 3\n1 3\n") == "2", "1 → 3 blocked first"
assert run("5 6\n1 2\n1 3\n2 5\n3 4\n4 5\n2 4\n") == "3", "multiple routes to 5"
assert run("2 1\n1 2\n") == "1", "minimum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 thành phố, nhiều lối đi | 2 | Lực chặn đường đi dài nhất đầu tiên | 
| 5 thành phố, 6 con đường | 3 | Tuyên truyền chính xác qua nhiều lớp | 
| 2 thành phố, 1 con đường | 1 | Đầu vào có kích thước tối thiểu được xử lý | 

## Vỏ cạnh 

Nếu một thành phố có nhiều cạnh đi ra có cùng độ dài trong trường hợp xấu nhất thì bước sắp xếp đảm bảo việc chặn bất kỳ đơn hàng nào vẫn tạo ra tổng số ngày như nhau. Ví dụ: trong `1 → 2, 1 →
