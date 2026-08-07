---
title: "CF 102501K - Xem chim"
description: "Tôi sẽ cung cấp một phiên bản rút gọn của bài xã luận nhằm giữ nguyên lý luận cốt lõi, bằng chứng, cách thực hiện và hướng dẫn kiểm tra đồng thời phù hợp với giới hạn phản hồi. Chỉnh sửa Chúng tôi được cung cấp một biểu đồ có hướng về chuyển động của chim được quan sát."
date: "2026-08-06T18:54:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 54
verified: true
draft: false
---

[CF 102501K - Xem chim](https://codeforces.com/problemset/problem/102501/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
Tôi sẽ cung cấp một phiên bản rút gọn của bài xã luận nhằm giữ nguyên lý luận cốt lõi, bằng chứng, cách thực hiện và hướng dẫn kiểm tra đồng thời phù hợp với giới hạn phản hồi. 

Chỉnh sửa 

#Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng về chuyển động của loài chim được quan sát. Đồ thị ban đầu của các chuyến bay thực chưa được xác định, nhưng mọi cạnh thực phải xuất hiện trong đồ thị được quan sát, trong khi một số cạnh được quan sát có thể là các phím tắt biểu thị các đường đi thực dài hơn. 

Đối với cây T cố định, chúng ta cần tìm mọi cây a sao cho cạnh a → T được đảm bảo là cạnh thực. Cạnh được quan sát a → T chỉ được đảm bảo khi mọi đường đi có thể từ a đến T bên trong đồ thị được quan sát đều sử dụng cạnh chính xác đó. 

Các ràng buộc cho phép tối đa 100000 nút và 100000 cạnh. Giải pháp kiểm tra các đường dẫn riêng biệt cho từng cạnh đến có thể dễ dàng trở thành phương trình bậc hai, quá chậm. Chúng ta cần xử lý đồ thị theo phương pháp truyền tải tuyến tính hoặc gần tuyến tính. 

Các trường hợp nguy hiểm chính là do các tuyến đường thay thế gây ra. Một cạnh trực tiếp vào T là không đủ. Ví dụ:```
3 3 2
0 1
0 2
1 2
```Câu trả lời là:```
1
1
```Nút 0 có cạnh bằng 2, nhưng đường 0 → 1 → 2 tránh cạnh đó, vì vậy 0 không hợp lệ. 

Một trường hợp khác là một chu kỳ:```
3 3 2
0 2
0 1
1 2
```Câu trả lời vẫn chỉ là nút 1. Một cạnh trực tiếp không thành vấn đề nếu cuối cùng một cạnh đi khác có thể chạm tới T. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi phần trước a của T và loại bỏ cạnh a → T. Nếu T vẫn có thể truy cập được từ a thì cạnh đó không được đảm bảo. Điều này đúng vì định nghĩa hỏi liệu có tồn tại một đường dẫn thay thế hay không. Tuy nhiên, việc thực hiện duyệt đồ thị cho mọi cạnh đến sẽ tốn O(M(N + M)) trong trường hợp xấu nhất, điều này là không thể đối với 100000 đỉnh. 

Quan sát quan trọng là mọi đường đi thay thế từ a đến T phải bắt đầu bằng một cạnh rời khỏi a khác với a → T. Chúng ta không cần phải kiểm tra từng cạnh riêng biệt. Chúng ta chỉ cần biết đỉnh nào có thể chạm tới T. 

Chạy một đồ thị ngược bắt đầu từ T. Một đỉnh được đánh dấu nếu nó có thể đạt tới T trong đồ thị ban đầu. Đối với cạnh đến a → T, nếu a có cạnh đi khác a → x trong đó x được đánh dấu và x không phải là T, thì tồn tại một đường dẫn khác từ a đến T, do đó cạnh này không được đảm bảo. Ngược lại, cách duy nhất để đến T từ a là đi qua a → T. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M(N+M)) | O(N+M) | Quá chậm | 
| Tối ưu | O(N+M) | O(N+M) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Xây dựng biểu đồ ngược. Cạnh ngược b → a được thêm vào cho mọi cạnh ban đầu a → b. Di chuyển từ T trong biểu đồ này sẽ truy cập chính xác các nút có thể tới T trong biểu đồ ban đầu. 
2. Chạy DFS hoặc BFS từ T trên biểu đồ ngược và đánh dấu tất cả các nút có thể truy cập. Đây là các nút duy nhất có thể xuất hiện trong đường dẫn kết thúc tại T. 
3. Lưu trữ tất cả các cạnh đi của mỗi nút trong khi đọc đầu vào, vì sau này chúng ta cần kiểm tra xem liệu ứng cử viên tiền nhiệm có tuyến đường khác tới T hay không. 
4. Với mỗi cạnh a → T, kiểm tra tất cả các cạnh đi ra của a. Nếu tồn tại một cạnh a → x với x khác T và x được đánh dấu là có thể tới T thì a có thể tới T mà không cần sử dụng a → T, vì vậy hãy loại bỏ nó. 
5. Sắp xếp tất cả các bản trước còn lại và in chúng. 

Tại sao nó hoạt động: Đường đi từ a đến T tránh cạnh a → T phải bắt đầu với một số cạnh đi ra khác a → x. Sau lần di chuyển đầu tiên đó, phần còn lại của đường dẫn chính xác là đường dẫn từ x đến T. Việc truyền tải ngược đánh dấu chính xác các giá trị x nơi tồn tại đường dẫn hậu tố như vậy. Do đó, thuật toán loại bỏ chính xác các cạnh có đường đi thay thế và chấp nhận chính xác các cạnh thực được đảm bảo. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, t = map(int, input().split())

    graph = [[] for _ in range(n)]
    rev = [[] for _ in range(n)]
    incoming = []

    for _ in range(m):
        a, b = map(int, input().split())
        graph[a].append(b)
        rev[b].append(a)
        if b == t:
            incoming.append(a)

    reachable = [False] * n
    stack = [t]
    reachable[t] = True

    while stack:
        v = stack.pop()
        for u in rev[v]:
            if not reachable[u]:
                reachable[u] = True
                stack.append(u)

    ans = []
    for a in incoming:
        ok = True
        for x in graph[a]:
            if x != t and reachable[x]:
                ok = False
                break
        if ok:
            ans.append(a)

    ans.sort()
    print(len(ans))
    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Quá trình truyền tải đầu tiên hoạt động trên biểu đồ đảo ngược vì hướng khả năng tiếp cận bị đảo ngược. Tiếp cận nút x từ T trong biểu đồ đảo ngược có nghĩa là ban đầu có đường dẫn từ x đến T. 

Vòng lặp cuối cùng chỉ kiểm tra các đỉnh đã có cạnh trực tiếp với T. Điều này là đủ vì mọi câu trả lời đều phải là tiền thân của T. Đối với mỗi đỉnh như vậy, một cạnh đi ra khác chỉ nguy hiểm khi nó dẫn đến một đỉnh mà cuối cùng có thể quay trở lại T. 

Không sử dụng đệ quy vì độ sâu đệ quy Python quá nhỏ đối với biểu đồ có 100000 đỉnh. Ngăn xếp lặp lại tránh được vấn đề đó. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

| Bước | Đỉnh hiện tại | Có thể tiếp cận T? | 
| --- | --- | --- | 
| Bắt đầu DFS ngược | 2 | vâng | 
| Thăm hàng xóm ngược | 1 | vâng | 
| Thăm hàng xóm ngược | 0 | vâng | 

Các cạnh tiếp theo của 2 là 0 → 2 và 1 → 2. 

| Ứng viên | Cạnh khác để nút có thể truy cập | Kết quả | 
| --- | --- | --- | 
| 0 | 0 → 1 | bị từ chối | 
| 1 | không | được chấp nhận | 

Câu trả lời là:```
1
1
```Đối với mẫu thứ hai, duyệt ngược từ 2 điểm 0, 1, 2, 3 và 4. Nút 5 không được đánh dấu. 

| Ứng viên | Tuyến đường thay thế | Kết quả | 
| --- | --- | --- | 
| 0 | 0 → 1 → 2 | bị từ chối | 
| 1 | không | được chấp nhận | 
| 4 | không | được chấp nhận | 

Câu trả lời là:```
2
1
4
```# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N+M) | Mỗi cạnh được xử lý một số lần không đổi. | 
| Không gian | O(N+M) | Đồ thị, đồ thị ngược và mảng truyền tải được lưu trữ. | 

Điều này phù hợp với các giới hạn vì thuật toán chỉ thực hiện một vài lần tuyến tính trên biểu đồ. 

# Trường hợp thử nghiệm```
# Expected outputs for the official implementation

# Minimum graph
# Input:
# 1 0 0
# Output:
# 0

# Single guaranteed edge
# Input:
# 2 1 1
# 0 1
# Output:
# 1
# 0

# Direct edge with an alternative route
# Input:
# 3 3 2
# 0 2
# 0 1
# 1 2
# Output:
# 1
# 1

# Cycle around the target
# Input:
# 4 5 3
# 0 3
# 0 1
# 1 2
# 2 3
# 2 1
# Output:
# 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút đơn | Không có câu trả lời | Xử lý đồ thị trống | 
| Một cạnh | Nguồn được chấp nhận | Trường hợp tiền nhiệm cơ bản | 
| Nhiều đường dẫn | Chỉ giữ các cạnh buộc | Ý chính | 
| Chu kỳ | Khả năng tiếp cận qua các chu kỳ | Đúng di chuyển ngược lại | 

# Vỏ cạnh 

Một cạnh đến trực tiếp vẫn có thể không hợp lệ. TRONG:```
3 3 2
0 2
0 1
1 2
```truyền tải ngược đánh dấu mọi nút có thể đạt tới 2. Khi kiểm tra nút 0, cạnh 0 → 1 được tìm thấy và nút 1 được đánh dấu, do đó thuật toán loại bỏ 0. 

Một nút bên trong chu trình có thể vẫn hợp lệ. Trong mẫu thứ hai, nút 4 có cạnh bằng 2, nhưng các cạnh đi ra khác của nó không tồn tại, do đó mọi đường đi từ 4 đến 2 đều phải sử dụng 4 → 2. Thuật toán chấp nhận nó mặc dù biểu đồ chứa các chu trình ở nơi khác.
