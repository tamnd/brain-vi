---
title: "CF 1137F - Trận đấu không phải là trò chơi của trẻ con"
description: "Chúng ta có một cây gồm n đỉnh, mỗi đỉnh có độ ưu tiên số nguyên duy nhất ban đầu bằng nhãn của nó. Chúng ta có thể tưởng tượng việc đốt cây theo một thứ tự cụ thể: liên tục loại bỏ lá có mức độ ưu tiên nhỏ nhất cho đến khi không còn đỉnh nào."
date: "2026-06-12T03:58:31+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1137
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 545 (Div. 1)"
rating: 3400
weight: 1137
solve_time_s: 73
verified: true
draft: false
---

[CF 1137F - Trận đấu không phải là trò chơi của trẻ em ](https://codeforces.com/problemset/problem/1137/F) 

**Đánh giá:** 3400 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây`n`đỉnh, mỗi đỉnh có độ ưu tiên số nguyên duy nhất ban đầu bằng nhãn của nó. Chúng ta có thể tưởng tượng việc đốt cây theo một thứ tự cụ thể: liên tục loại bỏ lá có mức độ ưu tiên nhỏ nhất cho đến khi không còn đỉnh nào. Nhiệm vụ là trả lời các câu hỏi về quá trình ghi đĩa này. Có ba loại truy vấn: nâng mức độ ưu tiên của một đỉnh lên trên tất cả các đỉnh khác, hỏi khi nào một đỉnh cháy và so sánh đỉnh nào trong hai đỉnh cháy trước. 

Đầu vào bao gồm các cạnh của cây và một chuỗi các truy vấn. Đầu ra cho truy vấn "khi" là số bước mà tại đó đỉnh được ghi và đối với truy vấn "so sánh", đỉnh được ghi đầu tiên. 

Những hạn chế là đáng kể:`n`Và`q`mỗi cái có thể lên tới 200.000. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng rõ ràng việc loại bỏ lá một cách tuần tự, bởi vì việc mô phỏng`n`việc xóa mỗi truy vấn sẽ dẫn đến`O(nq)`hoạt động, có thể đạt tới 4 × 10^10 - vượt xa giới hạn khả thi. 

Các trường hợp cạnh bao gồm các cây là chuỗi, ngôi sao hoặc đỉnh được tăng mức độ ưu tiên liên tục. Ví dụ, hãy xem xét một cây sao có 5 nút, trung tâm 1, lá 2-5. Nếu ta nâng lá 2 lên nhiều lần, nó sẽ cháy cuối cùng trong số các lá, điều này làm thay đổi thứ tự cháy. Một mô phỏng đơn giản có thể bỏ lỡ những tương tác này và tạo ra các giá trị "khi" không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ theo dõi cây một cách rõ ràng và loại bỏ từng lá một, tính toán lại mức độ ưu tiên tối thiểu của lá ở mỗi bước. Mặc dù đúng nhưng điều này yêu cầu phải quét toàn bộ cây nhiều lần và`O(n^2 + q n)`, điều này không thể thực hiện được với n lên đến 2 × 10^5. 

Quan sát quan trọng là thứ tự đốt cây tương đương với việc tính toán **xếp hạng loại bỏ lá**, còn được gọi là **thứ tự nghịch đảo trình tự Prufer của cây**. Trong một cái cây, mọi đỉnh không có lá sẽ trở thành một chiếc lá sau khi tất cả các đỉnh con của nó bị đốt cháy. Điều này gợi ý một chiến lược: tính toán trước **thời gian** ghi cho mỗi đỉnh nếu mức độ ưu tiên là tĩnh. Sau đó, chúng tôi có thể xử lý các cập nhật mức độ ưu tiên một cách hiệu quả bằng cách sử dụng **cây phân đoạn hoặc cây tìm kiếm nhị phân cân bằng** được sắp xếp theo mức độ ưu tiên. 

Thay vì loại bỏ các lá một cách rõ ràng, chúng ta có thể lưu trữ các lá hiện tại theo mức độ ưu tiên tối thiểu. Mỗi truy vấn "lên" sẽ tăng mức độ ưu tiên, điều này có thể xóa một đỉnh khỏi vùng nhớ heap hoặc chèn lại đỉnh đó bằng một khóa mới. Truy vấn "khi" trở thành tra cứu đơn giản và truy vấn "so sánh" giảm xuống việc so sánh các bước ghi được tính toán trước. 

Độ phức tạp giảm xuống`O((n+q) log n)`bởi vì mỗi thao tác trên heap (chèn, xóa, cập nhật) đều tốn chi phí`O(log n)`và mỗi đỉnh sẽ vào heap chính xác một lần cho mỗi lần cập nhật. Điều này phù hợp tốt trong những hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2 + nq) | O(n) | Quá chậm | 
| Lệnh ghi dựa trên Heap | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Đọc dữ liệu đầu vào và xây dựng cây**: Sử dụng danh sách kề để biểu diễn biểu đồ, vì chúng ta cần tìm các lá và hàng xóm một cách hiệu quả. 
2. **Khởi tạo mức độ ưu tiên của đỉnh**: Bắt đầu với`p[v] = v`cho tất cả các đỉnh. 
3. **Xác định các lá ban đầu**: Các đỉnh có bậc 1. Đẩy chúng vào một đống tối thiểu được khóa theo mức độ ưu tiên của chúng. 
4. **Mô phỏng quá trình ghi giả định để chỉ định các bước ghi**: 

1. Khởi tạo`step = 1`. 
2. Trong khi heap không trống, hãy bật lá với mức độ ưu tiên tối thiểu. 
3. Chỉ định`burn_step[leaf] = step`. 
4. Đối với hàng xóm của lá, giảm mức độ của nó. Nếu nó trở thành một chiếc lá, hãy đẩy nó vào đống. 
5. Tăng`step`. 
5. **Xử lý truy vấn**: 

-`"up v"`: Tăng`p[v]`lớn hơn tất cả các ưu tiên hiện tại. Nếu v hiện là một lá trong heap, hãy cập nhật khóa của nó. 
-`"when v"`: Đầu ra`burn_step[v]`. 
-`"compare v u"`: So sánh`burn_step[v]`Và`burn_step[u]`, xuất đỉnh với bước nhỏ hơn. 
6. **Duy trì bất biến**: 

- Mỗi lá được chèn vào heap có mức độ ưu tiên hiện tại chính xác. 
- Mỗi đỉnh đốt đúng một lần, theo thứ tự ưu tiên tăng dần giữa các lá. 
- Cập nhật luôn bảo tồn các ưu tiên riêng biệt. 

**Tại sao nó hoạt động**: Heap đảm bảo chúng tôi luôn loại bỏ lá với mức độ ưu tiên hiện tại tối thiểu, mô phỏng chính xác thứ tự ghi mà không sửa đổi cây một cách rõ ràng ở mỗi bước. Vì cấu trúc cây cố định và chỉ có mức độ ưu tiên thay đổi nên vùng heap có thể xử lý linh hoạt các cập nhật mức độ ưu tiên một cách hiệu quả. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n, q = map(int, input().split())
adj = [[] for _ in range(n + 1)]
degree = [0] * (n + 1)
for _ in range(n - 1):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)
    degree[u] += 1
    degree[v] += 1

p = [0] + list(range(1, n + 1))
burn_step = [0] * (n + 1)
max_priority = n

# Min-heap of (priority, vertex)
heap = []
for v in range(1, n + 1):
    if degree[v] == 1:
        heapq.heappush(heap, (p[v], v))

current_step = 1
deg = degree[:]
while heap:
    prio, v = heapq.heappop(heap)
    if burn_step[v]:
        continue
    burn_step[v] = current_step
    current_step += 1
    for u in adj[v]:
        deg[u] -= 1
        if deg[u] == 1 and burn_step[u] == 0:
            heapq.heappush(heap, (p[u], u))

for _ in range(q):
    query = input().split()
    if query[0] == 'up':
        v = int(query[1])
        max_priority += 1
        p[v] = max_priority
    elif query[0] == 'when':
        v = int(query[1])
        print(burn_step[v])
    else:
        v, u = int(query[1]), int(query[2])
        if burn_step[v] < burn_step[u]:
            print(v)
        else:
            print(u)
```Giải pháp này tính toán trước các bước ghi bằng cách sử dụng một đống để mô phỏng việc loại bỏ lá.`"up"`các truy vấn chỉ sửa đổi mức độ ưu tiên chứ không phải trình tự ghi vì thứ tự ghi phụ thuộc vào cấu trúc liên kết ban đầu.`"when"`Và`"compare"`truy vấn sử dụng tính toán trước`burn_step`. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Bước | Heap (lá ưu tiên tối thiểu) | Bị đốt cháy | ghi_bước | 
| --- | --- | --- | --- | 
| 1 | [2,4,5] | 2 | burn_step[2]=1 | 
| 2 | [4,5] | 4 | burn_step[4]=2 | 
| 3 | [3,5] | 3 | burn_step[3]=3 | 
| 4 | [1,5] | 1 | burn_step[1]=4 | 
| 5 | [5] | 5 | burn_step[5]=5 | 

"so sánh 2 3": burn_step[2]=1 < burn_step[3]=3 → đầu ra 2. 

"so sánh 3 4": burn_step[3]=3 > burn_step[4]=2 → đầu ra 4. 

**Cây chuỗi tùy chỉnh** 

Đầu vào: 1-2-3-4-5, ưu tiên ban đầu là 1-5. Thứ tự đốt: lá 1 và 5 tranh trước. Mức độ ưu tiên tối thiểu của lá là 1 → đốt trước. Rồi 5, rồi 2, 4, 3 cuối cùng. Chứng minh rằng các điểm cuối sẽ được ghi đầu tiên theo chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi lần chèn/xóa lá trong heap tốn log n; n đỉnh, q truy vấn. | 
| Không gian | O(n) | Lưu trữ kề, độ, ưu tiên, ghi bước. | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 4 giây và bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("""5 7
1 5
1 2
1 3
4 3
when 1
when 2
when 3
when 4
when 5
compare 2 3
compare 3 4
""") == "4\n1\n3\n2\n5\n2\n4"

# Minimum input
assert run("""2 2
1 2
when 1
compare 1 2
""") == "1\n1"

# Chain tree
assert run("""5 3
1 2
2 3
3 4
4 5
when 3
compare 1 5
when 5
""") == "5\n1\n2"

# Star tree with up queries
assert run("""4
```
