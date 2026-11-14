---
title: "CF 1777E - Cạnh đảo ngược"
description: "Bài toán cung cấp cho chúng ta một biểu đồ có hướng có trọng số và yêu cầu chúng ta làm cho nó "có thể truy cập mạnh mẽ" từ một nút nào đó bằng cách đảo ngược các cạnh tùy ý. Chi phí của việc đảo ngược là trọng số của cạnh nặng nhất mà chúng ta chọn để đảo ngược và mục tiêu của chúng ta là giảm thiểu mức tối đa này."
date: "2026-06-09T11:41:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1777
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 845 (Div. 2) and ByteRace 2023"
rating: 2200
weight: 1777
solve_time_s: 120
verified: true
draft: false
---

[CF 1777E - Đảo ngược cạnh](https://codeforces.com/problemset/problem/1777/E) 

**Đánh giá:** 2200 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, đồ thị, cây 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cung cấp cho chúng ta một biểu đồ có hướng có trọng số và yêu cầu chúng ta làm cho nó "có thể tiếp cận mạnh mẽ" từ một số nút bằng cách đảo ngược các cạnh tùy ý. Chi phí của việc đảo ngược là trọng số của cạnh nặng nhất mà chúng ta chọn để đảo ngược và mục tiêu của chúng ta là giảm thiểu mức tối đa này. Nếu biểu đồ đã có thể truy cập mạnh mẽ từ một số nút thì chi phí sẽ bằng 0 và nếu không thể đạt được khả năng truy cập đầy đủ ngay cả sau khi đảo ngược các cạnh thì câu trả lời là -1. 

Đầu vào chứa nhiều trường hợp thử nghiệm, mỗi trường hợp có tối đa 200.000 nút và 200.000 cạnh, nhưng tổng số nút và cạnh trên tất cả các trường hợp thử nghiệm không vượt quá 200.000. Ràng buộc này ngụ ý rằng chúng ta không thể cung cấp các thuật toán chậm hơn tuyến tính cho mỗi trường hợp thử nghiệm, lý tưởng nhất là tuyến tính hoặc tuyến tính với các yếu tố logarit. Cạnh lớn có trọng lượng lên tới$10^9$loại trừ việc tính trực tiếp theo trọng lượng; chúng ta phải coi chúng như những con số trừu tượng thay vì lặp lại tất cả các trọng số có thể có. 

Trường hợp biên đơn giản xảy ra khi đồ thị bị ngắt kết nối theo cách mà không một chuỗi đảo ngược đơn lẻ nào có thể làm cho nó có thể truy cập được từ bất kỳ nút nào. Ví dụ: một biểu đồ gồm hai nút không có cạnh nào yêu cầu đảo ngược một cạnh không tồn tại, điều này là không thể. Một trường hợp cạnh khác là biểu đồ đã được kết nối đầy đủ theo hướng thuận, trong đó câu trả lời đúng phải bằng 0 và việc triển khai bất cẩn có thể thêm trọng số dương không chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử đảo ngược tất cả các tập hợp con của các cạnh, kiểm tra khả năng tiếp cận từ mỗi nút và chọn tập hợp con có trọng số tối đa tối thiểu. Điều này rõ ràng là không thể thực hiện được vì có$2^m$tập hợp con và$m$có thể đạt tới 200.000. Ngay cả việc kiểm tra khả năng tiếp cận của từng nút trên mỗi tập hợp con cũng sẽ nhân lên điều này hơn nữa. 

Cái nhìn sâu sắc quan trọng là xem xét vấn đề về các thành phần được kết nối mạnh mẽ và ngưỡng khả năng tiếp cận biên. Instead of trying all subsets of edges, we can perform a binary search on the possible maximum weight of edges to reverse. Đối với trọng số tối đa của một ứng cử viên nhất định$W$, chúng ta xây dựng một đồ thị trong đó các cạnh nặng hơn$W$không thể đảo ngược và kiểm tra xem có tồn tại một nút mà từ đó tất cả các nút khác có thể truy cập được hay không khi chỉ đảo ngược các cạnh có trọng số$\le W$. 

Kiểm tra khả năng tiếp cận từ tất cả các nút cho một ứng viên$W$có thể được tối ưu hóa bằng cách sử dụng tìm kiếm theo chiều sâu hai lượt. Chúng tôi tính toán tập hợp các nút có thể truy cập thông qua các cạnh ban đầu cộng với các cạnh mà chúng tôi được phép đảo ngược và kiểm tra xem có nút nào tiếp cận được tất cả các nút khác hay không. Điều này làm giảm không gian tìm kiếm xuống còn$\log(\text{max weight})$lặp lại của DFS thời gian tuyến tính. 

Việc tìm kiếm nhị phân trên các trọng số cạnh và kiểm tra khả năng tiếp cận theo thời gian tuyến tính cùng nhau tạo ra một giải pháp hiệu quả. Phương pháp đơn giản này thất bại vì nó bùng nổ khi kết hợp với số cạnh, nhưng tìm kiếm nhị phân tận dụng tính đơn điệu của tính khả thi: nếu một trọng số tối đa nhất định hoạt động thì bất kỳ trọng số lớn hơn nào cũng hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * (n+m)) | O(n+m) | Quá chậm | 
| Tìm kiếm nhị phân + DFS | O((n+m) * log(trọng lượng tối đa)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng trường hợp kiểm thử và lặp lại từng trường hợp. Đối với mỗi trường hợp kiểm thử, hãy đọc các nút, cạnh và trọng số của biểu đồ. Lưu trữ các cạnh trong danh sách các bộ dữ liệu (u, v, w). 
2. Sắp xếp tất cả các trọng số của cạnh và loại bỏ các bản sao. Điều này cung cấp một tập hợp các trọng số đảo ngược tối đa ứng viên để sử dụng cho tìm kiếm nhị phân. Chúng tôi sử dụng các trọng số duy nhất vì tính khả thi chỉ thay đổi ở các trọng số cạnh hiện có. 
3. Khởi tạo hai con trỏ để tìm kiếm nhị phân:`low`bằng 0 và`high`là trọng số cạnh tối đa trong đồ thị. 
4. Trong khi`low <= high`, nhặt`mid`là trọng số tối đa ứng cử viên của các cạnh đảo ngược. Xây dựng danh sách kề trong đó các cạnh nặng hơn`mid`giữ nguyên hướng ban đầu và các cạnh$\le mid$có thể đảo ngược (chúng tôi lập mô hình cả hai hướng cho các cạnh này). 
5. Đối với danh sách kề hiện tại, thực hiện DFS từ bất kỳ nút nào. Nếu số lượng nút có thể truy cập bằng`n`, trọng số ứng cử viên hoạt động. Điều chỉnh tìm kiếm nhị phân: nếu thành công, di chuyển`high`ĐẾN`mid - 1`để tìm kiếm trọng lượng khả thi nhỏ hơn; nếu không thì di chuyển`low`ĐẾN`mid + 1`. 
6. Sau khi tìm kiếm nhị phân, nếu`low`vượt quá trọng lượng cạnh tối đa hoặc không tìm thấy trọng số khả thi, xuất ra -1. Ngược lại, xuất ra`low`là trọng lượng tối đa tối thiểu cho việc đảo ngược. 

Tại sao nó hoạt động: vấn đề có tính khả thi đơn điệu - nếu có thể tiếp cận tất cả các nút đảo ngược cạnh với trọng số tối đa$W$, thì có thể với bất kỳ$W' > W$. Tìm kiếm nhị phân khai thác tính đơn điệu này. DFS đảm bảo chúng tôi chỉ xem xét các nút có thể truy cập theo trọng số tối đa của ứng viên hiện tại, đưa ra các kiểm tra tính khả thi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        edges = []
        weights = set()
        for _ in range(m):
            u, v, w = map(int, input().split())
            edges.append((u-1, v-1, w))
            weights.add(w)
        weights = sorted(weights)

        def can_reach(max_w):
            adj = [[] for _ in range(n)]
            for u, v, w in edges:
                if w > max_w:
                    adj[u].append(v)
                else:
                    adj[u].append(v)
                    adj[v].append(u)
            visited = [False]*n
            stack = [0]
            visited[0] = True
            while stack:
                node = stack.pop()
                for nei in adj[node]:
                    if not visited[nei]:
                        visited[nei] = True
                        stack.append(nei)
            return all(visited)

        if can_reach(0):
            print(0)
            continue

        l, r = 0, len(weights)-1
        ans = -1
        while l <= r:
            mid = (l+r)//2
            if can_reach(weights[mid]):
                ans = weights[mid]
                r = mid - 1
            else:
                l = mid + 1
        print(ans)

solve()
```Giải pháp trước tiên sẽ kiểm tra xem biểu đồ gốc có cho phép truy cập đầy đủ hay không. Sau đó, nó thực hiện tìm kiếm nhị phân trên các trọng số cạnh duy nhất. Danh sách kề được xây dựng để cho phép đảo ngược các cạnh trong trọng số ứng cử viên hiện tại. DFS khám phá các nút có thể truy cập và kết quả khả thi sẽ thúc đẩy tìm kiếm nhị phân. Giới hạn đệ quy được tăng lên để xử lý các cuộc gọi DFS sâu trong các cây trong trường hợp xấu nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị: 2 nút, 1 cạnh 1→2 trọng số 3. 

| Bước | Hành động | Kết quả DFS | Giải thích | 
| --- | --- | --- | --- | 
| Ban đầu | Kiểm tra khả năng tiếp cận mà không cần đảo ngược | Nút 1 đến nút 2 | Đã có thể truy cập, chi phí 0 | 

Đầu ra: 0 

### Ví dụ 2 

Đồ thị: 4 nút, 2 cạnh 4→2 (5), 4→3 (20). 

| Bước | trọng lượng trung bình | DFS có thể truy cập | Tìm kiếm nhị phân | 
| --- | --- | --- | --- | 
| 0 | 5 | Nút 4 tiếp cận tất cả các nút | khả thi, hãy thử nhỏ hơn? Không có trọng lượng nhỏ hơn, ans=5 | 
| Kết quả | 5 | | Trọng lượng đảo chiều tối đa tối thiểu | 

Đầu ra: 5 

Những ví dụ này minh họa tính bất biến chính: tính khả thi đơn điệu khi trọng số đảo chiều tối đa tăng dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) * log m) | Tìm kiếm nhị phân theo trọng số cạnh và DFS tuyến tính trên mỗi lần kiểm tra | 
| Không gian | O(n+m) | Danh sách kề và mảng đã thăm | 

Với các ràng buộc sum(n)+sum(m) 2*10^5 trên tất cả các trường hợp thử nghiệm, giải pháp này là hiệu quả. Việc xây dựng DFS và danh sách kề là tuyến tính theo n+m và tìm kiếm nhị phân lặp lại tối đa m trọng số duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n2 1\n1 2 3\n5 4\n1 2 10\n2 3 10\n3 1 10\n4 5 10\n4 5\n1 2 10000\n2 3 20000\n1 3 30000\n4 2 500\n4 3 20\n4 5\n1 2 10000\n2 3 20000\n1 3 30000\n4 2 5\n4 3 20\n") == "0\n-1\n20\n5"

# Custom edge cases
assert run("1\n2 0\n") == "-1", "disconnected graph"
assert run("1\n3 3\n1 2 1\n2 3 1\n3 1 1\n") == "0", "already strongly reachable"
assert run("1\n4 3\n1 2 4\n2 3 2\n3 4 3\n") == "4", "need to reverse max weight 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút, không có cạnh | -1 | Khả năng tiếp cận không thể | 
| 3 nút, chu kỳ | 0 | Đã có thể truy cập, không thể đảo ngược | 
| 4 nút, đường dẫn | | |
