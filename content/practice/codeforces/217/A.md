---
title: "CF 217A - Trượt băng"
description: "Chúng ta được cung cấp một tập hợp các đường trượt tuyết trên lưới 2D có tọa độ nguyên. Bajtek chỉ có thể di chuyển theo đường thẳng dọc theo trục x hoặc y, trượt từ chỗ trôi này cho đến khi chạm vào chỗ khác."
date: "2026-06-05T00:53:21+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 217
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 134 (Div. 1)"
rating: 1200
weight: 217
solve_time_s: 87
verified: false
draft: false
---

[CF 217A - Trượt băng](https://codeforces.com/problemset/problem/217/A) 

**Đánh giá:** 1200 
**Tags:** bạo lực, dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đường trượt tuyết trên lưới 2D có tọa độ nguyên. Bajtek chỉ có thể di chuyển theo đường thẳng dọc theo trục x hoặc y, trượt từ chỗ trôi này cho đến khi chạm vào chỗ khác. Nói cách khác, hai đợt tuyết trôi được kết nối nếu chúng có cùng tọa độ x hoặc cùng tọa độ y. Bajtek muốn có thể di chuyển giữa hai đợt tuyết bất kỳ, trực tiếp hoặc gián tiếp, vì vậy chúng tôi cần thêm số lượng đợt tuyết mới tối thiểu để làm cho tập hợp được kết nối hoàn toàn theo các quy tắc di chuyển này. 

Đầu vào cung cấp số lần trôi hiện có$n$(lên tới 100) và tọa độ của chúng. Đầu ra là một số nguyên duy nhất: số lần trôi bổ sung cần thiết ít nhất để kết nối tất cả các lần trôi hiện có. Giá trị nhỏ của$n$chỉ ra rằng một$O(n^2)$giải pháp có thể chấp nhận được, điều này cho phép chúng ta xem xét tất cả các cặp độ lệch khi xác định kết nối. 

Một điểm tinh tế nảy sinh khi sự trôi dạt nằm trong các cụm biệt lập. Ví dụ: nếu có hai sự trôi dạt tại (1,1) và (2,2), chúng không thể tiếp cận trực tiếp với nhau. Thêm một drift tại (1,2) hoặc (2,1) sẽ kết nối chúng. Một cách tiếp cận bất cẩn có thể cố gắng chỉ xem xét các bước di chuyển theo cặp trực tiếp hoặc giả định tính liền kề của đường chéo hoạt động, điều này sẽ tạo ra câu trả lời sai bằng 0 cho trường hợp này. 

Một trường hợp cạnh khác là khi tất cả các drift nằm trên cùng một hàng hoặc cột. Ví dụ: ba độ trôi tại (1,1), (1,2), (1,3) đã tạo thành một thành phần được kết nối duy nhất, do đó không cần có độ trôi mới. Hiểu sai điều này có thể dẫn đến việc thêm những sai sót không cần thiết. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ là cố gắng đặt các điểm trôi mới ở mọi tọa độ nguyên từ 1 đến 1000 theo cả hai hướng x và y cho đến khi lưới được kết nối. Điều này là không thực tế vì ngay cả đối với$n = 100$, số lượng vị trí có thể có là theo thứ tự$10^6$và việc kiểm tra kết nối nhiều lần rất tốn kém. 

Thông tin chi tiết quan trọng đến từ việc xem các đợt tuyết trôi dưới dạng biểu đồ: mỗi đợt trôi là một nút và một cạnh tồn tại giữa các nút nếu chúng có cùng tọa độ x hoặc y. Vấn đề sau đó giảm xuống việc tìm số lượng thành phần được kết nối trong biểu đồ này. Mỗi thành phần được kết nối đều có thể truy cập nội bộ. Để kết nối$k$thành phần vào một mạng kết nối duy nhất, chúng ta cần chính xác$k-1$sự trôi dạt bổ sung, bởi vì mỗi sự trôi dạt mới có thể kết nối hai thành phần. Điều này tương tự như việc kết nối các đảo riêng biệt trong một mạng lưới: để giảm$k$đảo thành một,$k-1$cầu là đủ và cần thiết. 

Khi chúng ta hiểu điều này, giải pháp sẽ trở nên đơn giản: xây dựng biểu đồ, đếm các thành phần được kết nối và trừ đi một để có câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((1000*1000)^n) | O(1000*1000) | Quá chậm | 
| Đồ thị + DFS/BFS | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mỗi đợt tuyết trôi dưới dạng một nút trong biểu đồ. Hai nút được kết nối nếu tọa độ x hoặc y của chúng khớp nhau. Điều này mô hình hóa các ràng buộc chuyển động trượt một cách chính xác. 
2. Khởi tạo một mảng đã truy cập để theo dõi những phần trôi dạt nào đã được khám phá. 
3. Lặp lại từng lần trôi dạt. Nếu một điểm trôi chưa được truy cập, hãy bắt đầu tìm kiếm theo chiều sâu (DFS) từ điểm trôi đó để đánh dấu tất cả các điểm trôi có thể tiếp cận. Mỗi cuộc gọi DFS xác định một thành phần được kết nối duy nhất. 
4. Đếm tổng số thành phần được kết nối được phát hiện ở bước 3. 
5. Số lần trôi bổ sung tối thiểu cần thiết là số thành phần được kết nối trừ đi một. Mỗi drift mới có thể kết nối hai thành phần, vì vậy$k-1$trôi kết nối$k$thành phần thành một. 

Tại sao nó hoạt động: DFS phát hiện chính xác tất cả các lần trôi có thể truy cập được từ một lần trôi bắt đầu nhất định dọc theo các bước di chuyển hợp lệ. Theo định nghĩa, mỗi lần duyệt DFS đánh dấu chính xác một thành phần được kết nối. Vì mỗi drift mới có thể kết nối chính xác hai thành phần, kết nối$k$thành phần yêu cầu$k-1$độ trôi bổ sung, đảm bảo mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10000)

def main():
    n = int(input())
    drifts = [tuple(map(int, input().split())) for _ in range(n)]
    
    graph = [[] for _ in range(n)]
    for i in range(n):
        for j in range(i + 1, n):
            if drifts[i][0] == drifts[j][0] or drifts[i][1] == drifts[j][1]:
                graph[i].append(j)
                graph[j].append(i)
    
    visited = [False] * n
    
    def dfs(u):
        visited[u] = True
        for v in graph[u]:
            if not visited[v]:
                dfs(v)
    
    components = 0
    for i in range(n):
        if not visited[i]:
            dfs(i)
            components += 1
    
    print(components - 1)

if __name__ == "__main__":
    main()
```Việc xây dựng danh sách kề đảm bảo mỗi lần trôi dạt được kết nối với tất cả những lần di chuyển khác có chung x hoặc y, trực tiếp mô hình hóa các nước đi hợp pháp. DFS đánh dấu mọi lần trôi dạt có thể truy cập được từ lần trôi dạt bắt đầu, đảm bảo rằng mỗi thành phần được tính một lần.`components - 1`cung cấp số lần trôi bổ sung tối thiểu để kết nối tất cả các thành phần. Chúng tôi tăng giới hạn đệ quy để xử lý độ sâu trường hợp xấu nhất trong DFS khi tất cả các lần trôi tạo thành một chuỗi tuyến tính duy nhất. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2
2 1
1 2
```| Bước | đã ghé thăm | thành phần | 
| --- | --- | --- | 
| bắt đầu | [Sai, Sai] | 0 | 
| DFS từ 0 | [Đúng, Sai] | 1 | 
| DFS từ 1 | [Đúng, Đúng] | 2 | 

Đầu ra:`2 - 1 = 1`Giải thích: Hai drift riêng biệt cần một drift mới để kết nối. 

**Ví dụ tùy chỉnh** 

đầu vào:```
4
1 1
1 2
3 4
4 4
```| Bước | đã ghé thăm | thành phần | 
| --- | --- | --- | 
| bắt đầu | [F,F,F,F] | 0 | 
| DFS từ 0 | [T,T,F,F] | 1 | 
| DFS từ 2 | [T,T,T,T] | 2 | 

Đầu ra:`2 - 1 = 1`Giải thích: Hai thành phần [(1,1),(1,2)] và [(3,4),(4,4)] cần một drift để kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Xây dựng danh sách kề so sánh từng cặp trôi một lần; DFS là O(n+n^2) = O(n^2). | 
| Không gian | O(n^2) | Danh sách kề lưu trữ các cạnh cho mỗi cặp trôi chia sẻ x hoặc y. | 

Được cho$n \le 100$,$n^2 = 10000$hoạt động thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("2\n2 1\n1 2\n") == "1", "sample 1"

# Custom cases
assert run("1\n5 5\n") == "0", "single drift needs no connection"
assert run("3\n1 1\n1 2\n1 3\n") == "0", "all drifts in same column"
assert run("4\n1 1\n1 2\n3 4\n4 4\n") == "1", "two separate components"
assert run("5\n1 1\n2 1\n3 1\n1 2\n2 2\n") == "0", "all drifts connected via x or y"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 lần trôi tại (5,5) | 0 | Nút đơn, không cần kết nối | 
| 3 trôi cùng cột | 0 | Kết nối bằng chuyển động dọc | 
| Hai thành phần riêng biệt | 1 | Đếm chính xác các cụm bị ngắt kết nối | 
| Cụm kết nối phức tạp | 0 | DFS ghi lại tất cả các lỗi trôi dạt có thể tiếp cận | 

## Vỏ cạnh 

Đối với một lần trôi dạt`(5,5)`, DFS truy cập nút đó và không tìm thấy hàng xóm nào. Thành phần = 1, đầu ra = 0. Đối với các vùng trôi được căn chỉnh dọc theo một hàng hoặc cột, DFS đánh dấu chính xác tất cả các nút, do đó các thành phần = 1, đầu ra = 0. Khi tồn tại nhiều cụm bị cô lập, DFS sẽ phát hiện từng cụm một cách độc lập, đảm bảo`components - 1`đếm chính xác số lượng kết nối cần thiết. Đối với các tọa độ x hoặc y chồng chéo, việc xây dựng danh sách kề đảm bảo mọi di chuyển hợp pháp có thể được thể hiện.
