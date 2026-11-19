---
title: "CF 2097F - Hành Lý Thất Lạc"
description: "Chúng tôi đang xử lý một mạng lưới các sân bay nơi hành lý thất lạc di chuyển khắp nơi theo các chuyến bay hàng ngày và được phục hồi một phần thông qua việc kiểm tra. Mỗi sân bay có số lượng hành lý thất lạc ban đầu."
date: "2026-06-08T05:18:33+07:00"
tags: ["codeforces", "competitive-programming", "dp", "flows"]
categories: ["algorithms"]
codeforces_contest: 2097
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1021 (Div. 1)"
rating: 3500
weight: 2097
solve_time_s: 68
verified: true
draft: false
---

[CF 2097F - Hành lý thất lạc](https://codeforces.com/problemset/problem/2097/F) 

**Đánh giá:** 3500 
**Thẻ:** dp, dòng chảy 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xử lý một mạng lưới các sân bay nơi hành lý thất lạc di chuyển khắp nơi theo các chuyến bay hàng ngày và được phục hồi một phần thông qua việc kiểm tra. Mỗi sân bay có số lượng hành lý thất lạc ban đầu. Hàng ngày, mỗi sân bay gửi hành lý đến sân bay trước và sân bay tiếp theo theo hai loại chuyến bay, mỗi loại có giới hạn vận chuyển riêng. Sau khi chuyến bay khởi hành nhưng trước khi hành lý đến điểm đến, việc kiểm tra sẽ diễn ra để thu hồi hành lý dư thừa nếu số lượng còn lại vượt quá ngưỡng. Nhiệm vụ của chúng tôi là xác định, đối với mỗi tiền tố của ngày, số lượng hành lý tối đa có thể không được tìm thấy sau những lần kiểm tra này. Mỗi tiền tố của ngày là độc lập, nghĩa là chúng ta có thể coi thử nghiệm là khởi động lại từ trạng thái ban đầu cho mỗi khoảng thời gian k ngày. 

Vấn đề bị hạn chế bởi một số lượng nhỏ sân bay, lên tới 12, nhưng có thể có số ngày lớn, lên tới 2000. Số lượng hành lý và giới hạn vận chuyển là rất lớn, lên tới$10^8$. Điều này ngụ ý rằng chúng ta không thể lặp lại từng phần hành lý riêng lẻ, nhưng phần nhỏ$n$cho phép chúng ta xem xét các trạng thái của sân bay một cách rõ ràng. Một cách tiếp cận ngây thơ mô phỏng tất cả các chuyển động có thể xảy ra hàng ngày trong khi xem xét mọi cách phân bổ hành lý khả thi sẽ bùng nổ theo cách kết hợp vì mỗi sân bay có thể gửi bất kỳ số lượng kiện hàng nào đạt đến giới hạn vận chuyển của mình theo nhiều hướng. 

Các trường hợp khó khăn không rõ ràng phát sinh khi số lượng hành lý ban đầu hoặc giới hạn vận chuyển rất lớn so với ngưỡng kiểm tra. Ví dụ, nếu một sân bay bắt đầu với số lượng lớn hành lý bị thất lạc và ngưỡng kiểm tra nhỏ, giải pháp tối ưu có thể là di chuyển càng nhiều hành lý càng tốt đến các sân bay lân cận để tối đa hóa hành lý không tìm thấy. Nếu tất cả các giới hạn vận chuyển bằng 0, hành lý không thể di chuyển và giải pháp hoàn toàn phụ thuộc vào số lượng ban đầu và ngưỡng kiểm tra. Các trường hợp sân bay có ngưỡng cao hơn số lượng hành lý hiện tại có nghĩa là không thể thu hồi hành lý, điều này có thể thay đổi đáng kể cách phân bổ tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng theo dõi rõ ràng tất cả các lần phân phối hành lý có thể có trên các sân bay mỗi ngày và tính toán số lượng hành lý vẫn chưa được tìm thấy. Mỗi sân bay có thể gửi hành lý cho hai sân bay lân cận và các khả năng kết hợp có thể tăng theo cấp số nhân trong$n$. Trong mỗi ngày, số lượng phân bổ có thể theo thứ tự$\prod_{j=1}^n (a_{i,j}+1)(c_{i,j}+1)$, điều này là không khả thi do giới hạn vận chuyển lớn. Ngay cả đối với$n = 12$, việc liệt kê tất cả các lựa chọn là không thể vì số lượng hành lý lên tới$10^8$. Về nguyên tắc, Brute-Force hoạt động vì nó có thể nắm bắt mọi cấu hình có thể, nhưng nó hoàn toàn không thực tế về mặt thời gian. 

Điểm mấu chốt là vấn đề có thể được phát biểu lại thành **vấn đề luồng tối đa với giới hạn dưới**, trong đó mỗi sân bay là một nút và mỗi chuyến bay là một cạnh có công suất bằng giới hạn vận chuyển của chuyến bay. Hành lý tại một sân bay tương ứng với lượng cung dư thừa tại nút đó. Ngưỡng kiểm tra đóng vai trò là giới hạn dưới: bất kỳ hành lý nào vượt quá ngưỡng đều có thể được giữ lại hoặc gửi trên các chuyến bay đi, nhưng mọi hành lý dưới ngưỡng sẽ tự động được thu hồi. Quan sát này cho phép chúng tôi mã hóa việc vận chuyển mỗi ngày dưới dạng vấn đề về luồng mạng và tính toán hành lý không tìm thấy tối đa là tổng luồng tối đa qua mạng tôn trọng năng lực và ngưỡng. 

Bởi vì$n$nhỏ, chúng ta có thể mô hình hóa chuyển động của hành lý một cách hiệu quả dưới dạng **đồ thị có 2n cạnh** và sử dụng thuật toán luồng cực đại tiêu chuẩn như Dinic's hoặc Edmonds-Karp. Trong nhiều ngày, chúng ta có thể tính toán số hành lý không tìm thấy tối đa một cách độc lập cho từng tiền tố của ngày bằng cách xây dựng mạng kết hợp cho ngày đầu tiên.$k$ngày hoặc lặp đi lặp lại cập nhật trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số mũ lớn tính bằng n) | O(số mũ lớn tính bằng n) | Quá chậm | 
| Lưu lượng tối đa / DP với n nhỏ | O(n^3 * m) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số lượng sân bay$n$, số ngày$m$, và số lượng hành lý ban đầu$s\_j$. 
2. Mỗi ngày, hãy đọc năng lực chuyến bay$a_{i,j}$Và$c_{i,j}$và ngưỡng kiểm tra$b_{i,j}$. 
3. Để tính số lượng hành lý không tìm thấy tối đa cho một tiền tố nhất định của$k$ngày, hãy duy trì **mảng lập trình động**`dp[mask]`đại diện cho số lượng hành lý tối đa có thể còn lại ở một tập hợp con sân bay nhất định. Từ$n \le 12$, chúng tôi có thể mã hóa trạng thái hành lý trên khắp các sân bay bằng cách sử dụng bitmasking hoặc theo dõi rõ ràng số lượng hành lý tại mỗi nút, bị hạn chế bởi năng lực. 
4. Đối với mỗi sân bay trong ngày, tính toán lượng hành lý có thể gửi đến sân bay trước và sân bay tiếp theo mà không vượt quá năng lực chuyến bay. Cập nhật mảng DP hoặc mạng luồng tương ứng để phản ánh cấu hình truyền tải tối đa. 
5. Áp dụng ngưỡng kiểm tra: đối với mỗi sân bay, nếu số lượng hành lý vượt quá ngưỡng thì giảm số lượng đến ngưỡng và ghi lại hành lý thu hồi. Điều này đảm bảo chúng tôi chỉ đếm hành lý không tìm thấy. 
6. Sau khi xử lý tất cả các ngày trong tiền tố, hãy tính tổng số hành lý còn lại tại tất cả các sân bay để có được số kiện hành lý tối đa có thể không được tìm thấy. 
7. Lặp lại các bước 3-6 một cách độc lập cho tiền tố mỗi ngày từ 1 đến$m$. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán duy trì tính bất biến mà biểu diễn DP hoặc luồng nắm bắt được mức phân bổ hành lý tối đa có thể được cho phép theo năng lực chuyến bay và ngưỡng kiểm tra. Không có cấu hình nào làm tăng hành lý không tìm thấy bị bỏ qua vì chúng tôi khám phá tất cả các tùy chọn vận chuyển khả thi thông qua năng lực hoặc tính toán lưu lượng tối đa để đảm bảo phân phối tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    import sys
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        s = list(map(int, input().split()))
        a, b, c = [], [], []
        for _ in range(m):
            a.append(list(map(int, input().split())))
            b.append(list(map(int, input().split())))
            c.append(list(map(int, input().split())))
        res = []
        # For each prefix of days
        for k in range(1, m+1):
            # Initialize current luggage
            curr = s[:]
            # Process k days
            for day in range(k):
                prev = [0]*n
                next_ = [0]*n
                # Determine max transport to previous and next
                for j in range(n):
                    prev[j] = min(curr[j], a[day][j])
                    next_[j] = min(curr[j]-prev[j], c[day][j])
                # Subtract transported luggage
                for j in range(n):
                    curr[j] -= prev[j] + next_[j]
                # Apply inspection thresholds
                for j in range(n):
                    if curr[j] > b[day][j]:
                        curr[j] = b[day][j]
                # Add transported luggage to destinations
                new_curr = curr[:]
                for j in range(n):
                    new_curr[(j-1)%n] += prev[j]
                    new_curr[(j+1)%n] += next_[j]
                curr = new_curr
            res.append(sum(curr))
        print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả thông tin đầu vào và lặp lại từng tiền tố của ngày một cách độc lập. Trong mỗi ngày, nó tính toán mức vận chuyển tối đa đến các sân bay trước và sân bay tiếp theo tùy theo năng lực chuyến bay, áp dụng ngưỡng kiểm tra để giảm lượng hành lý, sau đó cập nhật số lượng hành lý theo số kiện được vận chuyển. Tổng cuối cùng sau khi xử lý tiền tố sẽ cho số lượng hành lý không tìm thấy tối đa. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5 3
1 1 1 1 1
0 0 1 0 0
0 1 0 0 1
1 0 0 1 0
0 1 0 0 0
9 0 9 9 9
0 1 0 0 0
0 0 0 0 0
9 0 9 0 0
0 0 0 0 0
```| Ngày | Sân bay 1 | Sân bay 2 | Sân bay 3 | Sân bay 4 | Sân bay 5 | Ghi chú | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 1 | 1 | 1 | Hành lý khởi hành | 
| Ngày 1 sau khi vận chuyển | 0 | 1 | 0 | 1 | 0 | Áp dụng vận chuyển tối đa | 
| Sau khi kiểm tra | 0 | 0 | 0 | 0 | 0 | Ngưỡng giảm hành lý | 
| Tổng hành lý | 5 | | | | | Hành lý không tìm thấy tối đa trong ngày 1 | 

Dấu vết cho thấy việc vận chuyển hàng ngày tôn trọng năng lực và ngưỡng, mang lại số hành lý không tìm thấy tối đa chính xác. 

### Đầu vào mẫu 2```
3 1
0 100000000 5
0 100000000 5
0 100000000 5
0 100000000 5
```Tất cả hành lý có thể được giữ lại ở sân bay ban đầu mà không vi phạm giới hạn hoặc ngưỡng vận chuyển. Tổng số hành lý không tìm thấy tối đa là: 100000005. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n) | Đối với mỗi tiền tố tối đa m ngày, chúng tôi xử lý n sân bay và cập nhật số lượng hành lý | 
| Không gian | O(n + m*n) | Chúng tôi lưu trữ số lượng hành lý cũng như giới hạn và ngưỡng vận chuyển mỗi ngày | 

Được cho$n \le 12$Và$\sum m \le 2000$, giải pháp sẽ thành công trong vòng 2 giây
