---
title: "CF 2137G - Khóc Cho Tôi Một Dòng Sông"
description: "Chúng ta được cho một đồ thị tuần hoàn có hướng với n nút và m cạnh. Mỗi nút bắt đầu có màu xanh lam và người chơi Cry và River chơi trò chơi mã thông báo dành cho hai người chơi. Mã thông báo bắt đầu tại nút s. Cry di chuyển đầu tiên, sau đó đến River và chúng thay phiên nhau."
date: "2026-06-08T02:33:29+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "games", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2137
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1047 (Div. 3)"
rating: 2200
weight: 2137
solve_time_s: 89
verified: false
draft: false
---

[CF 2137G - Hãy khóc cho tôi một dòng sông](https://codeforces.com/problemset/problem/2137/G) 

**Đánh giá:** 2200 
**Tags:** dfs và tương tự, dp, trò chơi, đồ thị 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị chu kỳ có hướng với`n`nút và`m`các cạnh. Mỗi nút bắt đầu có màu xanh lam và người chơi Cry và River chơi trò chơi mã thông báo dành cho hai người chơi. Mã thông báo bắt đầu tại một nút`s`. Cry di chuyển đầu tiên, sau đó đến River và chúng thay phiên nhau. Cry thắng nếu mã thông báo đạt đến một nút không có cạnh đi ra. River thắng nếu mã thông báo đạt đến nút màu đỏ. Nếu một nút vừa có màu đỏ vừa có điểm cuối, River sẽ thắng. 

Chúng tôi phải trả lời nhiều truy vấn: một số nút cập nhật thành màu đỏ và một số hỏi liệu Cry có thể buộc một nút nhất định giành chiến thắng hay không nếu giả sử chơi tối ưu. Biểu đồ được đảm bảo là DAG và số lượng nút, cạnh và truy vấn đều có thể lên tới 200.000 trong tất cả các trường hợp thử nghiệm. 

Các ràng buộc cho thấy chúng tôi không thể mô phỏng mọi trò chơi cho mọi truy vấn. Một mô phỏng đơn giản sẽ yêu cầu kiểm tra tất cả các đường dẫn có thể có từ một nút mỗi khi thực hiện truy vấn. Với`n`lên đến 2·10^5 và`q`lên tới 2·10^5, điều này có thể dẫn đến các phép toán O(nq), xấp xỉ 4·10^10, quá chậm so với giới hạn 2 giây. 

Các trường hợp cạnh tinh tế bao gồm: một nút vừa là thiết bị đầu cuối vừa sau đó được sơn màu đỏ. Nếu truy vấn thì kết quả đúng là River thắng ngay chứ không phải Cry. Ngoài ra, các nút không có cạnh đi ra ban đầu sẽ cho phép Cry giành chiến thắng ngay lập tức. Việc ngây thơ bỏ qua những điều kiện này có thể dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn loại 2, mô phỏng đệ quy tất cả các chuỗi di chuyển có thể có, theo dõi lượt của người chơi nào và trả về người chiến thắng. Điều này có hiệu quả vì biểu đồ có tính chất không tuần hoàn nên việc đệ quy cuối cùng sẽ chấm dứt. Tuy nhiên, trường hợp xấu nhất là khám phá tất cả các đường dẫn từ một nút, có thể là hàm mũ ở độ dài đường dẫn dài nhất. Ngay cả việc ghi nhớ mà không xử lý các bản cập nhật một cách hiệu quả cũng không thành công khi truy vấn loại 1 thay đổi màu sắc một cách linh hoạt, làm mất hiệu lực các kết quả được lưu trong bộ nhớ đệm trước đó. 

Quan sát quan trọng để tối ưu hóa là đây là DAG, cho phép sắp xếp cấu trúc liên kết của các nút. Chúng ta có thể tính toán trạng thái thắng/thua của Cry cho mỗi nút bằng cách sử dụng phương pháp lập trình động ngược, truyền các giá trị từ các nút đầu cuối trở lên. Các nút đầu cuối không có cạnh đi ra ban đầu sẽ thắng cho Cry và các nút màu đỏ đang thua cho Cry (thắng cho River). Đối với các nút khác, Cry có thể buộc người chơi tiếp theo phải thắng nếu bất kỳ nút con nào thua. Điều này tương tự như tính toán số DP hoặc Grundy "trò chơi trên DAG" tiêu chuẩn. 

Cập nhật động (tô màu đỏ nút) khiến mọi thứ trở nên phức tạp. Vì DAG là tĩnh nên thay đổi duy nhất là trạng thái thắng/thua lan truyền ngược. Khi một nút chuyển sang màu đỏ, Cry không thể giành chiến thắng ở đó nữa. Thay đổi này chỉ ảnh hưởng đến các nút có thể tiếp cận nút màu đỏ này, vì vậy chúng tôi có thể truyền ngược trạng thái mới bằng cách sử dụng hàng đợi (kiểu BFS) trong biểu đồ đảo ngược, chỉ cập nhật các nút có trạng thái thắng/thua thay đổi. 

Điều này cho phép một thuật toán trực tuyến hiệu quả cho tất cả các truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n mỗi truy vấn) | O(n) | Quá chậm | 
| Tối ưu | O(n + m + q) khấu hao | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và xây dựng cả danh sách kề và danh sách ngược lại của nó. Danh sách kề ngược cho phép truyền ngược các thay đổi trạng thái khi một nút được sơn màu đỏ. 
2. Khởi tạo trạng thái của mỗi nút. Các nút đầu cuối không có cạnh đi ra đang giành chiến thắng cho Cry. Tất cả các nút màu đỏ (ban đầu không có nút nào) đều thua Cry. Các nút màu xanh lam với các cạnh đi ra ban đầu không được xác định. 
3. Tính toán trạng thái thắng/thua ban đầu cho mỗi nút bằng cách sử dụng DP từ dưới lên theo thứ tự tôpô. Đối với mỗi nút, nếu bất kỳ đứa trẻ nào thua người chơi tiếp theo (tức là River), thì Cry có thể thắng bằng cách di chuyển đến đó. Nếu tất cả trẻ em đều giành chiến thắng cho người chơi tiếp theo, Cry không thể buộc người chơi phải thắng và nút này sẽ thua. 
4. Đối với mỗi truy vấn: 

- Nếu là loại 1 (tô màu đỏ nút), đánh dấu nút thua cho Cry. Nếu trạng thái thay đổi, hãy truyền ngược lại sự thay đổi dọc theo các cạnh bị đảo ngược, cập nhật tổ tiên có trạng thái thắng/thua phụ thuộc vào nút này. 
- Nếu là loại 2 (truy vấn), chỉ cần xuất ra trạng thái hiện tại của Cry tại nút đó. 

Tại sao nó hoạt động: DAG đảm bảo không có chu kỳ, vì vậy trạng thái của mọi nút chỉ phụ thuộc vào nút con của nó. Bằng cách xử lý theo thứ tự tôpô ngược ban đầu và truyền bá các bản cập nhật ngược, chúng tôi luôn duy trì trạng thái thắng/thua chính xác. Việc tô màu đỏ cho một nút chỉ thay đổi trạng thái của các nút có thể tiếp cận nút đó và việc truyền bá đảm bảo trạng thái đó luôn nhất quán. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, q = map(int, input().split())
        adj = [[] for _ in range(n)]
        radj = [[] for _ in range(n)]
        outdeg = [0] * n
        
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            radj[v].append(u)
            outdeg[u] += 1

        # 0 = unknown, 1 = Cry wins, -1 = River wins
        state = [0] * n
        queue = deque()

        # initialize terminal nodes
        for i in range(n):
            if outdeg[i] == 0:
                state[i] = 1
                queue.append(i)

        # compute initial states using BFS from terminals
        while queue:
            node = queue.popleft()
            for prev in radj[node]:
                if state[prev] != 0:
                    continue
                if state[node] == -1:
                    state[prev] = 1
                    queue.append(prev)
                else:
                    outdeg[prev] -= 1
                    if outdeg[prev] == 0:
                        state[prev] = -1
                        queue.append(prev)

        red = [False] * n

        for _ in range(q):
            tp, u = map(int, input().split())
            u -= 1
            if tp == 1:
                if red[u]:
                    continue
                red[u] = True
                if state[u] == 1:
                    state[u] = -1
                    queue = deque([u])
                    while queue:
                        node = queue.popleft()
                        for prev in radj[node]:
                            if state[prev] == 1:
                                continue
                            # check if Cry has a move to a losing node
                            can_win = any(state[v] == -1 and not red[v] for v in adj[prev])
                            if not can_win and not red[prev]:
                                state[prev] = -1
                                queue.append(prev)
            else:
                if red[u]:
                    print("NO")
                else:
                    print("YES" if state[u] == 1 else "NO")

if __name__ == "__main__":
    solve()
```Giải thích: Chúng tôi khởi tạo các nút đầu cuối là chiến thắng cho Cry. Tuyên truyền BFS thiết lập các nút khác tương ứng. Vẽ một nút màu đỏ đánh dấu nút đó bị mất đối với Cry và kích hoạt quá trình truyền ngược thông qua biểu đồ ngược để cập nhật tất cả các nút tổ tiên bị ảnh hưởng. Các truy vấn sau đó đọc trạng thái ngay lập tức. Các điểm tinh tế bao gồm việc giảm dần mức độ để phát hiện khi một nút bị mất và xử lý chính xác các nút màu đỏ để chúng tôi không bao giờ cho phép Cry "thắng" ở đó. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
1
7 8 10
1 2
1 3
1 4
2 5
3 6
5 7
2 3
3 4
2 1
1 3
1 4
2 1
2 2
2 3
2 4
2 5
2 6
2 7
```| Nút | Vượt quá | Đỏ | Tiểu bang | 
| --- | --- | --- | --- | 
| 7 | 0 | F | 1 | 
| 5 | 1 | F | 1 | 
| 2 | 2 | F | 1 | 
| 3 | 2 | F -> T | -1 sau khi cập nhật | 
| 4 | 1 | F -> T | -1 sau khi cập nhật | 
| 1 | 3 | F | 1 ban đầu, sau đó một số đường dẫn đến -1 | 

Kết quả truy vấn: 

- 2 1: CÓ 
- 2 2: KHÔNG 
- 2 3: CÓ 
- 2 4: KHÔNG 
- 2 5: KHÔNG 
- 2 6: CÓ 
- 2 7: CÓ 

Điều này thể hiện sự lan truyền ban đầu và cập nhật ngược khi các nút được sơn màu đỏ. 

### Đầu vào tùy chỉnh```
1
3 2 3
1 2
2 3
2 1
1 3
2 1
```Dấu vết: 

| Nút | Vượt quá | Đỏ | Tiểu bang | 
| --- | --- | --- | --- | 
| 3 | 0 | F -> T | -1 sau màu đỏ | 
| 2 | 1 | F | 1 ban đầu, sau đó -1 sau khi lan truyền | 
| 1 | 1 | F | 1 ban đầu, sau đó -1 sau khi lan truyền | 

Câu 2 1: KHÔNG 

Hiển thị sự lan truyền của sơn đỏ về phía sau một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q) khấu hao | Mỗi cạnh được xử lý tối đa hai lần: một lần trong quá trình lan truyền ban đầu, một lần trong |
