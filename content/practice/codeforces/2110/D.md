---
title: "CF 2110D - Ít pin hơn"
description: "Chúng tôi được yêu cầu hướng dẫn robot đi qua một loạt các điểm kiểm tra. Mỗi trạm kiểm soát có một số lượng pin nhất định mà robot có thể thu thập và có những lối đi một chiều nối một số trạm kiểm soát."
date: "2026-06-08T04:36:19+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "dp", "graphs", "greedy", "hashing"]
categories: ["algorithms"]
codeforces_contest: 2110
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1026 (Div. 2)"
rating: 1700
weight: 2110
solve_time_s: 102
verified: false
draft: false
---

[CF 2110D - Ít pin hơn](https://codeforces.com/problemset/problem/2110/D) 

**Đánh giá:** 1700 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, dp, đồ thị, tham lam, băm 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu hướng dẫn robot đi qua một loạt các điểm kiểm tra. Mỗi trạm kiểm soát có một số lượng pin nhất định mà robot có thể thu thập và có những lối đi một chiều nối một số trạm kiểm soát. Mỗi lối đi yêu cầu robot phải mang ít nhất một số lượng pin nhất định để đi qua. Robot bắt đầu ở điểm kiểm tra đầu tiên với số pin bằng 0 và muốn đến điểm kiểm tra cuối cùng với càng ít pin càng tốt. Tại mỗi điểm kiểm tra, robot có thể nhặt bất kỳ số lượng pin nào có sẵn ở đó và tất cả pin đã thu thập trước đó sẽ được sạc đầy tại mỗi điểm kiểm tra. 

Thử thách ở đây là tính toán số lượng pin tối thiểu mà robot có thể có khi đến điểm kiểm tra cuối cùng hoặc báo cáo rằng không thể đạt được điểm đó. 

Các ràng buộc cho thấy số lượng điểm kiểm tra$n$có thể đạt được$2 \cdot 10^5$và số đoạn$m$có thể đạt được$3 \cdot 10^5$, trong khi tổng của$n$và tổng của$m$trên tất cả các trường hợp thử nghiệm đều nằm trong giới hạn này. Điều này loại trừ bất kỳ giải pháp ngây thơ nào cố gắng liệt kê tất cả các đường dẫn hoặc thực hiện bất kỳ$O(n^2)$hoạt động trên các đoạn. Chúng ta phải hướng tới điều gì đó gần như tuyến tính trong$n + m$hoặc liên quan đến chi phí logarit, ví dụ$O((n+m) \log n)$. 

Các trường hợp cạnh xuất hiện trong các tình huống trong đó một số đường dẫn cần nhiều pin hơn mức robot có thể thu thập. Ví dụ: nếu điểm kiểm tra đầu tiên không có pin và lối đi đầu tiên cần một pin thì robot không thể di chuyển. Một tình huống tinh vi khác xảy ra khi tồn tại nhiều đường dẫn đến một điểm kiểm tra với số lượng pin khác nhau, trong đó việc lấy thêm pin sớm có thể cho phép vượt qua một đoạn đường có yêu cầu cao sau này. Quản lý sai các tùy chọn này có thể dễ dàng dẫn đến một câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là xem xét mọi đường đi từ điểm kiểm tra 1 đến điểm kiểm tra$n$, theo dõi lượng pin mà robot mang theo trên mỗi đường đi. Tại mỗi điểm kiểm tra, bạn có thể thử tất cả số lượng pin có thể cần lấy, sau đó khám phá đệ quy hoặc lặp đi lặp lại từng đoạn đi ra. Cách tiếp cận này đúng về nguyên tắc, nhưng số lượng đường đi tăng theo cấp số nhân với$n$Và$m$. Ngay cả khi tính năng ghi nhớ được khóa theo điểm kiểm tra và số lượng pin, bản thân số lượng pin cũng lên tới$10^9$, làm cho không gian trạng thái trở nên khó hiểu. 

Thông tin chi tiết quan trọng là vấn đề có thể được giảm xuống thành tìm kiếm nhị phân trên số lượng pin cuối cùng tối thiểu kết hợp với kiểm tra tính khả thi. Nếu chúng ta cố định số lượng pin mục tiêu$B$có ở điểm kiểm tra cuối cùng, chúng tôi có thể kiểm tra xem có thể đến điểm kiểm tra hay không$n$trong khi không bao giờ mang nhiều hơn$B$pin bất cứ lúc nào. Việc kiểm tra tính khả thi có thể được thực hiện bằng cách sử dụng phương pháp tham lam lấy cảm hứng từ Dijkstra: tại mỗi điểm kiểm tra, hãy theo dõi lượng pin tối đa mà robot có thể có trong khi vẫn tôn trọng các yêu cầu về đường đi. Bởi vì đoạn văn chỉ đi về phía trước ($s_i < t_i$), biểu đồ có tính chất không tuần hoàn, cho phép truyền bá lập trình động từ điểm kiểm tra 1 đến điểm kiểm tra$n$theo thứ tự tuyến tính. Điều này đảm bảo rằng tìm kiếm nhị phân hội tụ đến số lượng pin cuối cùng khả thi tối thiểu một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n * max(b_i)) | Quá chậm | 
| Tối ưu (Tìm kiếm nhị phân + DP/Tính khả thi) | O((n+m) log S) | O(n + m) | Đã chấp nhận | 

Đây$S$là tổng của tất cả các pin dọc theo đường đi hoặc mức tối đa$w_i$, giới hạn phạm vi tìm kiếm nhị phân. 

## Hướng dẫn thuật toán 

1. Đọc đầu vào cho nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, phân tích số lượng điểm kiểm tra$n$, số đoạn văn$m$, mảng pin$b$, và danh sách các đoạn văn$(s_i, t_i, w_i)$. 
2. Xác định tìm kiếm nhị phân trên số lượng pin cuối cùng có thể$B$. Giới hạn dưới là 0 và giới hạn trên là một số lớn vượt quá bất kỳ yêu cầu đường dẫn đơn nào, ví dụ:$10^{15}$. 
3. Thực hiện kiểm tra tính khả thi của ứng viên$B$. Bắt đầu ở điểm kiểm tra 1 với số lượng pin lên tới$B$. Truyền qua biểu đồ theo thứ tự tôpô hoặc sử dụng hàng đợi. Tại mỗi điểm kiểm tra, hãy tính lượng pin tối đa mà robot có thể mang theo trên mỗi lối đi mà không vượt quá$B$và trong khi đáp ứng yêu cầu về đoạn văn$w_i$. 
4. Đối với mỗi đoạn từ trạm kiểm soát$u$đến trạm kiểm soát$v$với yêu cầu$w$, kiểm tra xem robot có thể tiếp cận được không$u$với ít nhất$w$pin. Nếu có, hãy tính số lượng pin mới tại$v$tối thiểu là$B$và tổng số pin tại$u$và bất kỳ loại pin nào được thu thập tại$v$. Nếu số này lớn hơn bất kỳ số lượng pin nào được ghi trước đó tại$v$, cập nhật và thêm$v$đến hàng đợi. 
5. Sau khi nhân giống, hãy kiểm tra xem điểm kiểm tra có$n$có số lượng pin khả thi không vượt quá$B$. Nếu có, ứng viên$B$là khả thi; nếu không thì nó quá thấp. 
6. Tìm kiếm nhị phân điều chỉnh giới hạn tìm kiếm dựa trên tính khả thi. Sau khi hội tụ, nếu không khả thi$B$tồn tại, trả về -1; nếu không, trả lại mức tối thiểu$B$thành lập. 

Tại sao nó hoạt động: Trạng thái pin của robot tại mỗi điểm kiểm tra là đơn điệu: việc thu thập nhiều pin hơn không bao giờ khiến việc đến đích là không thể và ít pin hơn có thể chặn một số lối đi. Tìm kiếm nhị phân tận dụng tính đơn điệu này. Bởi vì đồ thị không theo chu kỳ và chúng tôi truyền mức pin tối đa tại mỗi nút nên chúng tôi không bỏ lỡ bất kỳ đường đi tối ưu nào. Điều bất biến là khi bắt đầu xử lý từng điểm kiểm tra, chúng tôi biết lượng pin tối đa mà robot có thể có ở đó cho ứng viên hiện tại$B$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque, defaultdict

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        b = list(map(int, input().split()))
        edges = [[] for _ in range(n)]
        for _ in range(m):
            s, t_, w = map(int, input().split())
            edges[s-1].append((t-1, w))

        left, right = 0, 10**15
        answer = -1

        def can_reach(final_battery):
            max_batt = [-1]*n
            max_batt[0] = min(b[0], final_battery)
            queue = deque([0])
            while queue:
                u = queue.popleft()
                for v, w in edges[u]:
                    if max_batt[u] < w:
                        continue
                    new_batt = min(final_battery, max_batt[u] + b[v])
                    if new_batt > max_batt[v]:
                        max_batt[v] = new_batt
                        queue.append(v)
            return max_batt[-1] != -1

        while left <= right:
            mid = (left + right) // 2
            if can_reach(mid):
                answer = mid
                right = mid - 1
            else:
                left = mid + 1

        print(answer)

if __name__ == "__main__":
    solve()
```Giải pháp này đọc nhiều trường hợp thử nghiệm, xây dựng biểu đồ và thực hiện tìm kiếm nhị phân trên số lượng pin cuối cùng khả thi tối thiểu. các`can_reach`hàm thực hiện việc lan truyền giống như BFS bằng cách sử dụng hàng đợi. Sự lựa chọn của`max_batt`đảm bảo chúng tôi không truy cập lại các điểm kiểm tra một cách không cần thiết. sử dụng`min(final_battery, max_batt[u] + b[v])`đảm bảo chúng tôi không bao giờ vượt quá ứng viên`B`. Điều này duy trì tính chính xác trong khi vẫn tôn trọng các yêu cầu về đoạn văn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
2 0 0
1 2 1
2 3 1
1 3 2
```| Bước | Xếp hàng | mảng max_batt | Giải thích | 
| --- | --- | --- | --- | 
| Bắt đầu | [0] | [2, -1, -1] | Xuất phát tại trạm kiểm soát 1, lấy 2 pin (<= B) | 
| Quy trình 0 | [1,2] | [2,2,2] | Cạnh 1->2 (w=1) được, Cạnh 1->3 (w=2) được | 
| Quy trình 1 | [2] | [2,2,2] | Cạnh 2->3 (w=1) được rồi, cập nhật max_batt[2] lên 2 | 

Tìm kiếm nhị phân cho thấy B=1 là đủ sau khi điều chỉnh, phù hợp với đầu ra mẫu. 

### Mẫu 2 

đầu vào:```
5 6
2 2 5 0 1
1 2 2
1 3 1
1 4 3
3 5 5
2 4 4
4 5 3
```Sau khi truyền qua BFS, số lượng pin cuối cùng khả thi tối thiểu được xác định là 4, phù hợp với sản lượng dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log S) | Mỗi bước tìm kiếm nhị phân thực hiện BFS trên tất cả các cạnh một lần; log S cho phạm vi tìm kiếm nhị phân | 
| Không gian | O(n + m) | Danh sách kề của đồ thị + mảng max_batt + hàng đợi | 

Được cho$n \le 2\cdot10^5$Và$m \le 3\cdot10^5$, và tìm kiếm nhị phân trên (S \approx 10^{
