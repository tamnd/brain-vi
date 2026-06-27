---
title: "CF 105093K - Kẻ xâm chiếm không gian"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô chứa một giá trị cường độ. Một ô đặc biệt là vị trí bắt đầu của người chơi và một ô khác là ô đích chứa mỏ Unobtanium."
date: "2026-06-27T20:51:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "K"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 36
verified: true
draft: false
---

[CF 105093K - Kẻ xâm chiếm không gian](https://codeforces.com/problemset/problem/105093/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô chứa một giá trị cường độ. Một ô đặc biệt là vị trí bắt đầu của người chơi và một ô khác là ô đích chứa mỏ Unobtanium. Mỗi ô khác chứa một kẻ thù có sức mạnh bằng giá trị được lưu trữ trong ô đó. 

Người chơi bắt đầu với sức mạnh của ô xuất phát. Di chuyển được phép theo bốn hướng. Khi người chơi vào ô mới, họ phải có sức mạnh nghiêm ngặt hơn kẻ thù trong ô đó, nếu không sẽ chết ngay lập tức. Nếu họ thắng, họ sẽ hấp thụ sức mạnh của kẻ thù và sức mạnh của chính họ sẽ tăng lên theo lượng đó. 

Có một ràng buộc bổ sung làm cho vấn đề này trở nên không tầm thường. Sau mỗi trận chiến thành công, tất cả các ô lân cận đều được kiểm tra. Nếu bất kỳ người hàng xóm nào có kẻ thù có sức mạnh lớn hơn sức mạnh cập nhật của người chơi, người hàng xóm đó sẽ ngay lập tức tấn công và giết chết người chơi. Vì vậy, việc sống sót sau một cuộc chiến không chỉ phụ thuộc vào tế bào hiện tại mà còn phụ thuộc vào biên giới xung quanh. 

Nhiệm vụ là xác định xem có tồn tại bất kỳ con đường nào từ đầu đến đích sao cho mọi hành động đều an toàn dưới những hạn chế đang phát triển này hay không. 

Tổng cộng lưới có thể lớn tới 10^6 ô cho mỗi lần kiểm tra, do đó, bất kỳ giải pháp nào truy cập lại các trạng thái nhiều lần hoặc mô phỏng rõ ràng tất cả các đường dẫn sẽ không thành công. Một đường đi ngắn nhất hoặc DFS qua các trạng thái là không khả thi vì mỗi trạng thái phụ thuộc vào cường độ hiện tại, thay đổi dọc theo đường dẫn, làm cho không gian trạng thái có hiệu quả theo cấp số nhân. 

Một trường hợp phức tạp nảy sinh từ việc “kiểm tra trả thù”. Ngay cả khi bạn có thể đánh bại một ô, bạn có thể chết ngay lập tức do một người hàng xóm mạnh hơn sau trận chiến. Ví dụ: nếu sức mạnh cập nhật của bạn trở thành 10 và người hàng xóm có sức mạnh 12, thì nước đi đó không hợp lệ ngay cả khi người hàng xóm đó không đi trên đường của bạn. Một BFS ngây thơ chỉ kiểm tra ô đích sẽ chấp nhận những chuyển đổi như vậy một cách không chính xác. 

Một tình huống khó khăn khác xảy ra khi một con đường có vẻ an toàn tại địa phương nhưng yêu cầu tích lũy sức mạnh sớm ở một khu vực khác trước khi quay lại khu vực bị hạn chế. Điều này có nghĩa là lối suy nghĩ tham lam đi theo con đường ngắn nhất sẽ thất bại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi đây là một tìm kiếm biểu đồ trong đó mỗi trạng thái được xác định bởi vị trí và cường độ hiện tại. Từ một trạng thái, chúng tôi thử tất cả bốn bước di chuyển, cập nhật sức mạnh và xác thực cả ô mục tiêu cũng như ràng buộc trả thù. Điều này đúng nhưng sẽ bùng nổ ngay lập tức vì sức mạnh có thể tăng lên bằng tổng của tất cả các ô, và cùng một ô có thể truy cập được nhiều sức mạnh khác nhau. Trong trường hợp xấu nhất, điều này tạo ra nhiều trạng thái theo cấp số nhân. 

Quan sát quan trọng là hạn chế trả thù chỉ phụ thuộc vào _sức mạnh hiện tại_ chứ không phụ thuộc vào lịch sử đường đi. Khi bạn vào một ô, câu hỏi duy nhất của bạn là liệu sức mạnh hiện tại của bạn có đủ để tồn tại cho cả ô đó và các ô lân cận hay không. Điều này cho thấy chúng ta nên luôn cố gắng tiếp cận các ô theo thứ tự tăng dần về độ mạnh cần thiết, tương tự như thuật toán của Dijkstra. 

Chúng ta có thể diễn giải lại vấn đề như một quá trình tiếp cận trong đó mỗi tế bào sẽ sẵn sàng khi chúng ta có đủ sức mạnh để chiếm giữ nó một cách an toàn. Mỗi lần chúng ta vào một ô, sức mạnh của chúng ta sẽ tăng lên, có thể mở khóa được nhiều ô hơn. Đây chính xác là một quá trình mở rộng đơn điệu và một hàng đợi ưu tiên được sắp xếp theo cường độ hiện tại sẽ nắm bắt được nó một cách tự nhiên. 

Chúng tôi luôn mở rộng từ trạng thái có thể tiếp cận mạnh nhất hiện tại, vì điều đó tối đa hóa cơ hội mở khóa các ô mới. Bất kỳ trạng thái yếu hơn nào cũng không thể dẫn tới khả năng tiếp cận sớm hơn các khu vực mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (trạng thái BFS/DFS có sức mạnh) | Hàm mũ | O(r·c·S) | Quá chậm | 
| Tìm kiếm đầu tiên tốt nhất với hàng đợi ưu tiên | O(rc log(rc)) | O(rc) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Giải thích mỗi ô lưới là một nút có trọng số bằng với yêu cầu về sức mạnh của nó nhưng có phần thưởng động bằng giá trị của nút đó khi được thu thập. Nút bắt đầu bắt đầu với cường độ ban đầu bằng giá trị ô của nó. 
2. Duy trì hàng ưu tiên của các ô có thể truy cập, được sắp xếp theo cường độ hiện tại tại thời điểm chúng được tiếp cận. Chúng tôi luôn mở rộng bang có sức mạnh cao nhất trước tiên. Điều này đảm bảo chúng tôi không bao giờ bỏ qua con đường có thể mở khóa các khu vực khó hơn sớm hơn. 
3. Theo dõi các ô đã truy cập để đảm bảo chúng tôi không xử lý cùng một ô nhiều lần, vì khi một ô được lấy, sự đóng góp vào sức mạnh của ô đó sẽ được thêm vào vĩnh viễn. 
4. Từ ô hiện tại, cố gắng di chuyển theo bốn hướng. Đối với mỗi hàng xóm, hãy kiểm tra xem cường độ hiện tại có lớn hơn giá trị của hàng xóm hay không. Nếu không, hãy bỏ qua nó. 
5. Nếu việc di chuyển được phép, hãy mô phỏng việc tiến vào ô bằng cách tăng cường độ. Sau khi cập nhật sức mạnh, hãy xác minh điều kiện trả thù: cả bốn người hàng xóm của ô mới phải có sức mạnh nhỏ hơn hoặc bằng sức mạnh được cập nhật. Nếu bất kỳ người hàng xóm nào vi phạm điều này, hãy loại bỏ việc di chuyển. 
6. Nếu việc di chuyển hợp lệ và hàng xóm chưa được ghé thăm, hãy đẩy nó vào hàng ưu tiên với cường độ được cập nhật. 
7. Dừng ngay lập tức khi ô đích được đưa ra khỏi hàng đợi. Nếu hàng đợi trống mà không đến được thì mục tiêu sẽ không thể truy cập được. 

Ý tưởng chính là mỗi khi chúng tôi xử lý một ô, chúng tôi đang thực hiện với sức mạnh tối đa có thể trong số tất cả các cách hiện đã biết để tiếp cận ô đó, vì vậy mọi cải tiến trong tương lai đều phải đến từ các khu vực chưa được khám phá trước đây. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì biên giới của các ô có thể tiếp cận với các giá trị cường độ liên quan và luôn mở rộng biên giới cường độ tối đa trước tiên. Vì cường độ chỉ tăng dọc theo các đường dẫn và không bao giờ giảm, nên khi một ô được xử lý với cường độ nhất định, thì bất kỳ đường dẫn thay thế nào đến cùng một ô sau đó không thể tạo ra trạng thái mạnh hơn nếu không đi qua các nút đã được xử lý hoặc trạng thái có cường độ cao hơn lẽ ra đã được mở rộng trước đó. Điều này chứng tỏ rằng trong lần đầu tiên chúng tôi hoàn thiện một ô, chúng tôi đã xem xét mọi cách để tiếp cận nó với sức mạnh tối đa, do đó không thể bỏ lỡ con đường nào tốt hơn đến đích. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        r, c = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(r)]
        isr, jsc, itr, jtc = map(int, input().split())
        isr -= 1
        jsc -= 1
        itr -= 1
        jtc -= 1

        start_val = grid[isr][jsc]
        target = (itr, jtc)

        visited = [[False] * c for _ in range(r)]
        pq = []
        heapq.heappush(pq, (-start_val, isr, jsc))

        dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        while pq:
            neg_str, x, y = heapq.heappop(pq)
            strength = -neg_str

            if visited[x][y]:
                continue
            visited[x][y] = True

            if (x, y) == target:
                print("UNOBTANIUM")
                break

            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if nx < 0 or nx >= r or ny < 0 or ny >= c:
                    continue
                if visited[nx][ny]:
                    continue

                if strength <= grid[nx][ny]:
                    continue

                new_strength = strength + grid[nx][ny]

                ok = True
                for ddx, ddy in dirs:
                    ax, ay = nx + ddx, ny + ddy
                    if 0 <= ax < r and 0 <= ay < c:
                        if grid[ax][ay] > new_strength:
                            ok = False
                            break

                if ok:
                    heapq.heappush(pq, (-new_strength, nx, ny))
            else:
                continue
        else:
            print("UNOBTAINABLE")

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc triển khai sử dụng vùng heap tối đa bằng cách phủ định các điểm mạnh để trạng thái mạnh nhất có thể tiếp cận luôn được mở rộng trước tiên. Mảng đã truy cập đảm bảo mỗi ô được xử lý một lần ở cường độ tốt nhất được biết đến. 

Chi tiết quan trọng là điều kiện trả thù được kiểm tra ngay sau khi mô phỏng nước đi, trước khi đẩy trạng thái. Điều này tránh việc truyền bá các trạng thái không hợp lệ vào không gian tìm kiếm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ nơi điểm khởi đầu đã đủ mạnh để di chuyển qua một đường tăng dần đơn điệu. 

| Bước | Tế bào | Sức mạnh trước | Di chuyển hợp lệ | Sức mạnh sau | Kiểm tra trả thù | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,1) bắt đầu | 3 | - | 3 | được | 
| 2 | (1,2) | 3 | có (3 > 1) | 4 | được | 
| 3 | (2,2) | 4 | có (4 > 4? không, rất không hợp lệ) | - | - | 

Điều này cho thấy rằng ngay cả khi một đường dẫn dường như có thể tiếp cận được về mặt hình học, thì sự đẳng thức sẽ chặn chuyển động, ngăn chặn việc mở rộng không chính xác. 

Thuật toán dừng sớm một cách chính xác vì không có sự tiếp tục hợp lệ nào tồn tại. 

### Ví dụ 2 

Trường hợp cần đi đường vòng: 

| Bước | Tế bào | Sức mạnh trước | Di chuyển hợp lệ | Sức mạnh sau | Kiểm tra trả thù | 
| --- | --- | --- | --- | --- | --- | 
| 1 | bắt đầu | 5 | - | 5 | được | 
| 2 | nút an toàn có giá trị cao | 5 | vâng | 12 | được | 
| 3 | mục tiêu | 12 | vâng | 15 | được | 

Ở đây, thứ tự hàng đợi ưu tiên đảm bảo trạng thái trung gian mạnh hơn được khám phá trước, mở khóa mục tiêu, trong khi BFS ngây thơ có thể thử các tuyến yếu hơn trước và bị chặn. 

Điều này xác nhận rằng thứ tự mở rộng là cần thiết cho tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(rc log(rc)) | Mỗi ô được chèn và xóa khỏi heap tối đa một lần, mỗi thao tác tốn thời gian logarit | 
| Không gian | O(rc) | Lưới, mảng đã truy cập và vùng nhớ heap lưu trữ tối đa một mục nhập trên mỗi ô | 

Các ràng buộc đảm bảo rằng tổng số ô trong các trường hợp thử nghiệm tối đa là 10^5, do đó độ phức tạp này vừa vặn trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm
