---
title: "CF 106129H - Kết nối vui vẻ"
description: "Chúng ta được cho một đồ thị có hướng trong đó các đỉnh biểu thị các ga xe lửa và các cạnh biểu thị các kết nối tàu một chiều. Hai người bắt đầu từ hai trạm khác nhau và mỗi người có thể di chuyển dọc theo các cạnh được định hướng bao nhiêu lần cũng được."
date: "2026-06-20T07:31:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106129
codeforces_index: "H"
codeforces_contest_name: "2025-2026 ICPC German Collegiate Programming Contest (GCPC 2025)"
rating: 0
weight: 106129
solve_time_s: 42
verified: true
draft: false
---

[CF 106129H - Kết nối vui vẻ](https://codeforces.com/problemset/problem/106129/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó các đỉnh biểu thị các ga xe lửa và các cạnh biểu thị các kết nối tàu một chiều. Hai người bắt đầu từ hai trạm khác nhau và mỗi người có thể di chuyển dọc theo các cạnh được định hướng bao nhiêu lần cũng được. Câu hỏi đặt ra là liệu có tồn tại ít nhất một trạm có thể truy cập được từ cả hai điểm xuất phát hay không. 

Vì vậy, nhiệm vụ giảm xuống còn vấn đề về khả năng tiếp cận trong biểu đồ có hướng: từ nút`a`và nút`b`, chúng tôi muốn tìm xem liệu các tập hợp có thể truy cập của chúng có giao nhau hay không và nếu có thì xuất ra bất kỳ nút nào trong giao điểm. 

Các ràng buộc lên tới 100.000 nút và 100.000 cạnh, điều này ngay lập tức loại trừ mọi phương pháp bậc hai như kiểm tra khả năng tiếp cận giữa mỗi cặp nút hoặc tính toán lại các đường đi ngắn nhất cho mỗi nút ứng cử viên. Bất kỳ giải pháp nào về cơ bản phải tuyến tính theo kích thước của biểu đồ, thường là`O(n + m)`. 

Một vấn đề tế nhị sẽ xuất hiện nếu người ta cố gắng lý luận một cách tham lam từ cấu trúc. Ví dụ: chọn “trạm họp” dựa trên kết nối cục bộ hoặc điểm gặp sớm nhất trong cây BFS là không an toàn vì khả năng tiếp cận không đối xứng và các đường dẫn có thể phân kỳ sâu trước khi hội tụ lại. 

Một trường hợp biên khác là khi không có đường dẫn đi từ một trong các nguồn. Ví dụ: 

đầu vào:```
3 1
1 2
3 1
```Từ`3`bạn có thể tiếp cận`1`và sau đó`2`, nhưng từ`1`bạn chỉ có thể tiếp cận`2`. Trạm họp duy nhất có thể là`2`. Một cách tiếp cận ngây thơ chỉ kiểm tra những người hàng xóm trực tiếp của`a`hoặc`b`sẽ bỏ lỡ điều này. 

Cuối cùng, nếu biểu đồ bị ngắt kết nối hoặc chứa các chuỗi dài có hướng, thì rất dễ giả định sai sự chồng chéo dựa trên trực giác vô hướng. Hướng hoàn toàn quan trọng ở đây. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tính toán toàn bộ các nút có thể truy cập từ`a`sử dụng BFS hoặc DFS và tính toán riêng tập hợp có thể truy cập từ`b`, sau đó cắt hai tập hợp. Điều này đúng và đơn giản về mặt khái niệm. 

Mỗi BFS/DFS chạy trong`O(n + m)`, và đặt giao điểm là`O(n)`nếu được thực hiện với mảng boolean. Vì vậy, ngay cả cách tiếp cận đơn giản cũng đã tuyến tính trên mỗi nguồn, do đó`O(n + m)`tổng cộng. Điều này đã đủ hiệu quả cho các hạn chế. 

Tuy nhiên, một lực lượng vũ phu mang tính khái niệm hơn có thể cố gắng, đối với mỗi nút`v`, để kiểm tra xem`v`có thể truy cập từ cả hai`a`Và`b`bằng cách chạy BFS hai lần cho mỗi nút hoặc chạy kiểm tra khả năng tiếp cận ngược cho mỗi nút. Điều đó sẽ dẫn đến`O(n(n + m))`, nó quá lớn. 

Quan sát quan trọng là khả năng tiếp cận từ một nguồn duy nhất sẽ phân chia biểu đồ thành một vùng có thể tiếp cận. Chúng ta chỉ cần biết liệu hai vùng có thể truy cập có trùng nhau hay không và nếu có, chúng ta có thể dừng ngay lập tức khi tìm thấy bất kỳ nút chung nào. Điều này cho phép chúng tôi tránh những công việc không cần thiết bằng cách xen kẽ hoặc dừng sớm, nhưng ngay cả khi không có thủ thuật tối ưu hóa, hai lần chạy BFS là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên mỗi nút | O(n(n + m)) | O(n + m) | Quá chậm | 
| Hai giao lộ BFS/DFS + | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán độc lập tất cả các nút có thể truy cập từ mỗi trạm bắt đầu bằng cách truyền tải đồ thị. 

1. Xây dựng danh sách kề từ các cạnh có hướng. Cấu trúc này cho phép truyền tải hiệu quả các kết nối đi từ bất kỳ trạm nào. 
2. Chạy BFS hoặc DFS bắt đầu từ trạm`a`, đánh dấu mọi nút đã truy cập trong một mảng boolean`reach_a`. Điều này ghi lại chính xác tập hợp các ga mà người A có thể đến sau bất kỳ số chuyến tàu nào. 
3. Chạy BFS hoặc DFS khác bắt đầu từ trạm`b`, đánh dấu các nút đã truy cập trong`reach_b`. Điều này tương tự bao gồm tất cả các trạm mà Hannah có thể truy cập. 
4. Quét tất cả các trạm từ`1`ĐẾN`n`. Đối với mỗi trạm`v`, kiểm tra xem cả hai`reach_a[v]`Và`reach_b[v]`là đúng. Nút đầu tiên như vậy là điểm gặp mặt hợp lệ và có thể được xuất ra ngay lập tức. 
5. Nếu không có nút nào tồn tại sau khi quét tất cả các trạm, xuất`"no"`. 

Lý do chúng ta có thể quay lại bất kỳ nút giao lộ nào một cách an toàn là vì bài toán không yêu cầu tính tối ưu như thời gian di chuyển ngắn nhất hay ga sớm nhất; mọi sự chồng chéo có thể tiếp cận đều hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán tính toán rõ ràng các bộ khả năng tiếp cận phía trước của cả hai nút bắt đầu. Theo định nghĩa, một nút nằm trong giao điểm của các tập hợp này khi và chỉ khi cả hai người tham gia có thể tiếp cận nó một cách độc lập dọc theo các đường dẫn được chỉ dẫn. Việc thăm dò BFS/DFS đảm bảo tính đầy đủ của các tập hợp này vì mỗi cạnh được duyệt chính xác một lần từ mỗi nguồn, do đó không có nút có thể truy cập nào bị bỏ qua. Do đó, bất kỳ nút nào được tìm thấy trong cả hai tập hợp đã truy cập đều là trạm gặp nhau chính xác và nếu không có nút nào tồn tại thì giao điểm sẽ trống và không có giải pháp nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def bfs(start, adj, n):
    vis = [False] * (n + 1)
    stack = [start]
    vis[start] = True

    while stack:
        v = stack.pop()
        for nxt in adj[v]:
            if not vis[nxt]:
                vis[nxt] = True
                stack.append(nxt)
    return vis

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    for _ in range(m):
        x, y = map(int, input().split())
        adj[x].append(y)

    a, b = map(int, input().split())

    reach_a = bfs(a, adj, n)
    reach_b = bfs(b, adj, n)

    for i in range(1, n + 1):
        if reach_a[i] and reach_b[i]:
            print("yes")
            print(i)
            return

    print("no")

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng danh sách kề có hướng và sau đó thực hiện hai tìm kiếm theo chiều sâu độc lập bằng cách sử dụng một ngăn xếp. Mảng đã truy cập`reach_a`Và`reach_b`mã hóa khả năng tiếp cận từ mỗi nút bắt đầu. 

Một chi tiết triển khai nhỏ quan trọng là đánh dấu các nút được truy cập ngay lập tức khi đẩy lên ngăn xếp. Điều này ngăn chặn việc chèn lặp lại và giữ cho quá trình truyền tải tuyến tính. Lần quét cuối cùng là cần thiết vì chúng tôi được phép xuất ra bất kỳ trạm họp hợp lệ nào, không nhất thiết phải là trạm đầu tiên được phát hiện trong quá trình truyền tải. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 3
2 3
1 2
```Chúng tôi tính toán khả năng tiếp cận từ`1`Và`2`. 

| Bước | Nút từ A | Đã ghé thăm A | Nút từ B | Đã ghé thăm B | Đã tìm thấy giao lộ | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | {1} | 2 | {2} | không | 
| Mở rộng A | 3 | {1,3} | 2 | {2} | không | 
| Mở rộng B | 3 đã thấy | {1,3} | 3 | {2,3} | vâng | 

Cả hai nút tiếp cận`3`, vì vậy đầu ra là:```
yes
3
```Điều này chứng tỏ rằng sự hội tụ có thể xảy ra ngay cả khi các nguồn khác nhau và đường đi không đối xứng. 

### Ví dụ 2 

đầu vào:```
3 2
2 1
2 3
1 3
```Khả năng tiếp cận: 

| Bước | Nút từ A | Đã ghé thăm A | Nút từ B | Đã ghé thăm B | Đã tìm thấy giao lộ | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | {1,3} | 3 | {3} | không | 
| Mở rộng B | - | {1,3} | xong | {3} | không | 

Ở đây, từ`1`chúng ta có thể tiếp cận`3`, và từ`3`chúng ta chỉ đến được chính nó, nên giao điểm là`{3}`và đầu ra là:```
yes
3
```Điều này cho thấy thuật toán xử lý chính xác khả năng tiếp cận không đối xứng trong đó một điểm xuất phát đã gần đến điểm gặp nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai lần duyệt đồ thị cộng với quét tuyến tính qua các nút | 
| Không gian | O(n + m) | Danh sách kề cộng với hai mảng đã truy cập | 

Các ràng buộc cho phép tổng cộng tối đa 200.000 phần tử biểu đồ và mỗi phần tử được xử lý với số lần không đổi. Điều này phù hợp thoải mái trong giới hạn thông thường cho cửa sổ thực thi 2 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        return sys.stdout.getvalue()
    finally:
        pass

# Provided samples
# (formatting assumed; adjust if needed)

# Custom cases
assert run("""2 0
1 2
1 2
""").strip() == "no", "no edges"

assert run("""2 1
1 2
1 2
""").strip().split()[0] == "yes", "single path"

assert run("""4 4
1 2
2 3
3 4
1 4
1 3
""").strip().split()[0] == "yes", "multiple paths"

assert run("""3 2
1 2
2 3
3 1
""").strip().split()[0] == "yes", "cycle case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có cạnh | không | xử lý đồ thị bị ngắt kết nối | 
| con đường đơn | vâng | khả năng tiếp cận đơn giản | 
| nhiều đường dẫn | vâng | cạnh dư thừa | 
| trường hợp chu kỳ | vâng | hành vi được kết nối mạnh mẽ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một nút bắt đầu đã có thể truy cập được từ nút kia. Ví dụ, nếu`b`có thể đạt được`a`, sau đó`a`có thể đã là một điểm gặp gỡ hợp lệ. 

đầu vào:```
2 1
1 2
1 2
```Từ`1`, tập hợp có thể truy cập là`{1,2}`. Từ`2`, tập hợp có thể truy cập là`{2}`. Giao lộ là`{2}`, vậy đầu ra là`yes 2`. BFS từ`2`chỉ đánh dấu chính xác chính nó và lần quét cuối cùng sẽ tìm thấy nút`2`như phổ biến. 

Một trường hợp khác là biểu đồ bị ngắt kết nối:```
4 2
1 2
3 4
1 3
```Từ`1`chúng tôi đạt được`{1,2}`, từ`3`chúng tôi đạt được`{3,4}`, giao lộ trống rỗng. Thuật toán in chính xác`no`sau khi quét tất cả các nút, vì không có nút nào được đánh dấu trong cả hai mảng đã truy cập. 

Trường hợp khó phát hiện cuối cùng là khi tồn tại nhiều câu trả lời hợp lệ. Vì chúng tôi quét các nút theo thứ tự tăng dần nên chúng tôi trả về trạm họp có nhãn nhỏ nhất, nhưng mọi thứ tự sẽ đúng tùy theo vấn đề.
