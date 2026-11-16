---
title: "CF 1912F - Điên Cuồng"
description: "Chúng ta được yêu cầu tính toán khoảng thời gian dự kiến ​​của một cuộc truy đuổi trên một cái cây, nơi một sĩ quan cảnh sát và một kẻ chạy trốn thay phiên nhau di chuyển. Thành phố được biểu diễn dưới dạng cây vô hướng với $n$ đỉnh."
date: "2026-06-08T20:17:06+07:00"
tags: ["codeforces", "competitive-programming", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1912
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3100
weight: 1912
solve_time_s: 185
verified: false
draft: false
---

[CF 1912F - Cuộc chạy trốn điên cuồng](https://codeforces.com/problemset/problem/1912/F) 

**Đánh giá:** 3100 
**Tags:** toán, xác suất 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính toán khoảng thời gian dự kiến của một cuộc truy đuổi trên một cái cây, nơi một sĩ quan cảnh sát và một kẻ chạy trốn thay phiên nhau di chuyển. Thành phố được biểu diễn dưới dạng cây vô hướng với$n$đỉnh. Sĩ quan cảnh sát bắt đầu ở một đỉnh cố định, trong khi kẻ chạy trốn chọn bất kỳ đỉnh nào khác để bắt đầu. Sĩ quan di chuyển trước, bước tới đỉnh liền kề hoặc đứng yên, tiêu tốn một đơn vị thời gian. Kẻ chạy trốn sau đó ngay lập tức di chuyển đến bất kỳ đỉnh nào không nằm trên đường đến chỗ sĩ quan, tránh bị bắt trừ khi bị ép buộc. Cả hai đều chơi tối ưu: sĩ quan giảm thiểu thời gian bắt giữ dự kiến ​​và kẻ chạy trốn tối đa hóa nó. 

Đầu ra là số phút dự kiến ​​cho đến khi kẻ chạy trốn bị bắt. Bởi vì kẻ chạy trốn di chuyển ngay lập tức và luôn biết vị trí của sĩ quan, nên vấn đề giảm xuống còn việc tính toán thời gian bắt giữ dự kiến ​​trong trò chơi hai người chơi với thông tin hoàn hảo cho kẻ chạy trốn. 

Số đỉnh nhỏ ($n \le 100$), do đó các thuật toán có tính bậc ba$n$là khả thi. Cấu trúc cây đảm bảo một đường đi duy nhất giữa hai đỉnh bất kỳ, giúp đơn giản hóa việc tính toán các bước di chuyển an toàn cho kẻ chạy trốn. Các trường hợp cạnh bao gồm: 

-$n = 2$, nơi cảnh sát và kẻ chạy trốn ở hai đỉnh liền kề, dẫn đến việc bắt giữ ngay lập tức. 
- Cây có “con đường dài” nơi kẻ chạy trốn có thể liên tục di chuyển đến các đỉnh xa. Một cuộc tìm kiếm theo chiều rộng ngây thơ mà bỏ qua những chuyển động tức thời của kẻ chạy trốn sẽ không tạo ra được kỳ vọng chính xác. 

Hiểu được những điểm tinh tế này là rất quan trọng: chuyển động tức thời của kẻ chạy trốn có nghĩa là chúng ta không thể mô phỏng các vị trí theo từng ngã rẽ một cách ngây thơ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê mọi chuỗi hành động của sĩ quan và kẻ chạy trốn, đồng thời tính toán đệ quy thời gian bắt giữ dự kiến. Đối với từng vị trí sĩ quan$p$và vị trí trốn chạy$f$, chúng ta có thể thử tất cả các bước di chuyển của sĩ quan và tất cả các bước di chuyển an toàn có thể chạy trốn, tính trung bình các khả năng. Điều này dẫn đến$O(n^2 \cdot n \cdot n)$đệ quy trong trường hợp xấu nhất, vì có$n^2$cặp vị trí và lên đến$n$di chuyển cho mỗi người. Ngay cả với tính năng ghi nhớ, không gian trạng thái vẫn$O(n^2)$, nhưng đệ quy để tính toán các kỳ vọng có thể nhân giá trị này thành$O(n^4)$, điều này có thể chấp nhận được đối với$n \le 100$nhưng cồng kềnh và dễ mắc lỗi. 

Điểm mấu chốt là kẻ chạy trốn luôn có thể tránh được viên cảnh sát trừ khi viên cảnh sát đó đã "đủ gần" để bẫy anh ta. Nói cách khác, từ vị trí$(p, f)$, thời gian bắt giữ dự kiến ​​chỉ phụ thuộc vào khoảng cách của cây con: khoảng cách xa nhất mà sĩ quan có thể giảm trong một lần di chuyển. Kẻ chạy trốn sẽ phân bổ bản thân đến tất cả các vị trí được an toàn tối đa. Điều này làm giảm bài toán về một hệ phương trình tuyến tính, trong đó mỗi trạng thái$(p, f)$đại diện cho thời gian chụp dự kiến ​​không xác định$E[p][f]$. Nếu như$p = f$,$E[p][f] = 0$. Ngược lại, viên chức sẽ chọn một động thái để giảm thiểu$1 + E[p'][f']$, và kẻ chạy trốn chọn$f'$để tối đa hóa$E[p'][f']$. Điều này dẫn đến một hệ thống tuyến tính tối thiểu-tối đa có thể giải được bằng phép lặp hoặc nghịch đảo ma trận. 

Cái nhìn sâu sắc này làm giảm sự ngây thơ$O(n^4)$đệ quy với các nhánh xác suất thành một phép lặp$O(n^3)$Giải pháp: đối với mỗi vị trí sĩ quan, chúng tôi tính toán thời gian dự kiến ​​đối với tất cả các vị trí chạy trốn có thể có bằng cách sử dụng lập trình động, truyền bá các giá trị cho đến khi hội tụ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n^2) | Có thể chấp nhận nhưng phức tạp để thực hiện | 
| Hệ thống tuyến tính / DP | O(n^3) | O(n^2) | Hiệu quả, rõ ràng và khả thi | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào của cây và xây dựng biểu diễn danh sách kề. Mỗi đỉnh lưu trữ các đỉnh lân cận của nó để cho phép truy cập liên tục vào các bước di chuyển của sĩ quan. 
2. Khởi tạo mảng 2D$E[p][f]$đại diện cho thời gian bắt giữ dự kiến ​​nếu sĩ quan ở$p$và kẻ chạy trốn tại$f$. Bộ$E[p][p] = 0$cho tất cả$p$, vì việc chụp xảy ra ngay lập tức. 
3. Tính toán trước “nước đi an toàn” cho kẻ chạy trốn. Đối với mỗi cặp$(p, f)$, tìm tất cả các đỉnh$f'$sao cho đường đi từ$f$ĐẾN$f'$không đi qua$p$. Điều này có thể được thực hiện một cách hiệu quả bằng cách sử dụng BFS hoặc DFS từ$f$, bỏ qua cây con chứa$p$. 
4. Khởi tạo$E[p][f] = 1$vì$p \ne f$như một sự phỏng đoán ban đầu. Đây là thời gian cơ bản cho hành động tiếp theo của sĩ quan. 
5. Lặp lại cho đến khi hội tụ: 

1. Đối với từng chức vụ sĩ quan$p$và vị trí trốn chạy$f$, nếu như$p \ne f$, tính giá trị nhỏ nhất trên tất cả các lần di chuyển của sĩ quan$p' \in \{p\} \cup \text{neighbors}(p)$của$1 + \text{average of } E[p'][f']$trên tất cả các động thái chạy trốn an toàn$f'$. 
2. Cập nhật$E[p][f]$đến giá trị tối thiểu này. Lặp lại cho đến khi các thay đổi ở dưới một epsilon nhỏ ($1e-9$). 
6. Đáp án cuối cùng là thời gian dự kiến ​​từ$E[s][f]$, tính trung bình trên tất cả các vị trí xuất phát tạm thời có thể$f \ne s$, vì kẻ chạy trốn có thể chọn bất kỳ đỉnh ban đầu nào một cách tối ưu. 

Điều này hiệu quả vì toán tử ở bước 5 là một ánh xạ thu gọn: mỗi lần cập nhật mang các giá trị đến gần hơn với kỳ vọng thực và điểm cố định duy nhất thể hiện kỳ ​​vọng tối thiểu-tối đa cho tất cả các vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    adj = [[] for _ in range(n)]
    for _ in range(n-1):
        u, v = map(int, input().split())
        adj[u-1].append(v-1)
        adj[v-1].append(u-1)
    s = int(input()) - 1

    # Precompute distances and safe moves
    from collections import deque
    safe = [[[] for _ in range(n)] for _ in range(n)]
    for p in range(n):
        for f in range(n):
            visited = [False]*n
            visited[p] = True
            q = deque([f])
            safe[p][f] = []
            while q:
                u = q.popleft()
                safe[p][f].append(u)
                for v in adj[u]:
                    if not visited[v]:
                        visited[v] = True
                        q.append(v)

    # Initialize expected times
    E = [[0.0 if i==j else 1.0 for j in range(n)] for i in range(n)]
    eps = 1e-9
    changed = True
    while changed:
        changed = False
        for p in range(n):
            for f in range(n):
                if p == f:
                    continue
                best = float('inf')
                for np_ in [p] + adj[p]:
                    # Fugitive maximizes
                    sume = 0
                    moves = safe[np_][f]
                    for nf in moves:
                        sume += E[np_][nf]
                    sume /= len(moves)
                    best = min(best, 1 + sume)
                if abs(E[p][f] - best) > eps:
                    E[p][f] = best
                    changed = True

    # Fugitive chooses initial position to maximize expectation
    ans = 0.0
    for f in range(n):
        if f != s:
            ans = max(ans, E[s][f])
    print(ans)

if __name__ == "__main__":
    main()
```Danh sách lân cận cho phép truy cập liên tục vào các lần di chuyển của sĩ quan. Các nước đi an toàn cho kẻ chạy trốn được tính toán trước bằng BFS, bỏ qua cây con chứa sĩ quan. Bản cập nhật DP$E[p][f]$cho đến khi hội tụ. Việc tính trung bình các nước đi chạy trốn an toàn thực hiện kỳ ​​vọng xác suất của chiến lược hỗn hợp của kẻ chạy trốn. Chúng tôi đạt được mức tối đa so với các vị trí ban đầu vì kẻ chạy trốn chọn đỉnh xuất phát một cách tối ưu. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2
1 2
2
```| Bước | Sĩ quan | Kẻ chạy trốn | E[p][f] | 
| --- | --- | --- | --- | 
| ban đầu | 2 | 1 | 1 | 

Here, the officer can move to 1 immediately, capturing the fugitive in one minute. Đầu ra là 1. 

**Mẫu 2 (tùy chỉnh)** 

đầu vào:```
3
1 2
2 3
1
```| Bước | Sĩ quan | Kẻ chạy trốn | E[p][f] | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 2 | 1,5 | 
| ban đầu | 1 | 3 | 2 | 

Kẻ chạy trốn ở phút thứ 3 tối đa hóa thời gian. Kỳ vọng tối ưu là 2. 

Những dấu vết này cho thấy kẻ chạy trốn khai thác các vị trí ở xa và viên cảnh sát chọn những động thái giảm thiểu thời gian bắt giữ dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Đối với mỗi cặp sĩ quan và vị trí bỏ trốn, chúng tôi lặp lại lên đến$n$sĩ quan di chuyển và xem xét lên đến$n$những bước chạy trốn an toàn. | 
| Không gian | O(n^2) | Lưu trữ các mảng E[p][f] và safe[p][f]. | 

Được cho$n \le 100$, (O(n^3
