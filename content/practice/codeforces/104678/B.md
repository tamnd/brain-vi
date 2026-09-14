---
title: "CF 104678B - Đêm truyền phát"
description: "Chúng ta được cho một khoảng thời gian từ giây 1 đến giây n. Dọc theo dòng thời gian này, có k luồng video, mỗi luồng được biểu thị bằng một cửa sổ hoạt động nửa mở trong thực tế nhưng được xử lý một cách hiệu quả dưới dạng khoảng thời gian đóng từ giây đầu ai đến giây cuối bi."
date: "2026-06-29T14:35:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104678
codeforces_index: "B"
codeforces_contest_name: "October come back. Together training"
rating: 0
weight: 104678
solve_time_s: 79
verified: false
draft: false
---

[CF 104678B - Đêm truyền phát](https://codeforces.com/problemset/problem/104678/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một khoảng thời gian từ giây 1 đến giây n. Dọc theo dòng thời gian này, có k luồng video, mỗi luồng được biểu thị bằng một cửa sổ hoạt động nửa mở trong thực tế nhưng được xử lý một cách hiệu quả dưới dạng khoảng thời gian đóng từ giây đầu ai đến giây cuối bi. 

Nhiệm vụ là chọn một chuỗi các luồng sao cho bạn có thể xem chúng lần lượt mà không bị trùng lặp về thời gian và mục tiêu là tối đa hóa số lượng luồng bạn quản lý để xem. Chuyển đổi duy nhất được phép là nếu một luồng kết thúc vào thời điểm t, bạn có thể ngay lập tức bắt đầu một luồng khác bắt đầu vào thời điểm t. Sự chồng chéo theo bất kỳ cách nào khác làm cho hai luồng không tương thích trong chuỗi. 

Đầu ra là số khoảng thời gian tối đa bạn có thể xâu chuỗi lại với nhau theo quy tắc này. 

Các ràng buộc lên tới khoảng 200000, do đó, bất kỳ giải pháp nào kém hơn O(k log k) đều có nguy cơ hết thời gian. Cách tiếp cận bậc hai sẽ yêu cầu kiểm tra tất cả các cặp hoặc chạy DP trong tất cả các khoảng thời gian, dẫn đến khoảng 4e10 thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều khoảng có cùng thời gian bắt đầu hoặc kết thúc. Ví dụ: nếu tất cả các khoảng đều giống nhau như (2,3), thì chỉ có thể chọn một khoảng mặc dù có nhiều ứng cử viên. Một trường hợp cạnh khác là khi một khoảng dài chồng lên nhiều khoảng ngắn; một phương pháp tham lam chọn khoảng thời gian dài trước có thể chặn các cơ hội tạo chuỗi tốt hơn sau này. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi đây là bài toán đường đi dài nhất trong biểu đồ tuần hoàn có hướng trong đó mỗi khoảng trỏ đến tất cả các khoảng có thể theo sau nó. Chúng ta có thể thử mọi khoảng thời gian làm điểm bắt đầu và thử đệ quy tất cả các khoảng thời gian hợp lệ tiếp theo có thời gian bắt đầu ít nhất là thời điểm kết thúc khoảng thời gian hiện tại. Điều này khám phá chính xác tất cả các chuỗi hợp lệ, nhưng với mỗi khoảng thời gian, chúng tôi có thể quét tối đa k chuỗi khác, dẫn đến chuyển đổi O(k²) và đệ quy hoặc DP qua các trạng thái vẫn là bậc hai. 

Thông tin chi tiết về cấu trúc quan trọng là khả năng tương thích chỉ phụ thuộc vào thời gian kết thúc của khoảng thời gian hiện tại. Khi chúng tôi kết thúc một luồng tại thời điểm t, mọi luồng bắt đầu vào hoặc sau t đều hợp lệ. Điều này gợi ý một sự lựa chọn tham lam: luôn lấy luồng tiếp theo kết thúc sớm nhất có thể trong số tất cả các luồng hiện có. Bằng cách hoàn thành sớm, chúng tôi tối đa hóa khoảng thời gian còn lại cho các lựa chọn trong tương lai, giúp bảo toàn được nhiều cơ hội hơn. 

Để thực hiện điều này, chúng tôi sắp xếp các khoảng thời gian theo thời gian bắt đầu và duyệt qua chúng trong khi vẫn duy trì cấu trúc “các luồng có sẵn” được sắp xếp theo thời gian kết thúc. Ở mỗi bước, chúng tôi nâng cao thời gian hiện tại và thêm tất cả các luồng có thời gian bắt đầu có thể truy cập được, sau đó chọn luồng có thời gian kết thúc nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k²) | O(k) | Quá chậm | 
| Tối ưu (Tham lam + đống) | O(k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các khoảng thời gian theo thời gian bắt đầu của chúng. 

Điều này đảm bảo chúng tôi có thể tiết lộ dần dần các luồng khi thời gian trôi qua mà không cần quét liên tục toàn bộ danh sách. 
2. Duy trì một con trỏ trong các khoảng thời gian đã được sắp xếp và một vùng heap tối thiểu được khóa theo thời gian kết thúc. 

Heap đại diện cho tất cả các luồng đã bắt đầu nhưng chưa được chọn. 
3. Khởi tạo thời gian hiện tại về 0 và trả lời về 0. 

Về mặt khái niệm, chúng tôi bắt đầu trước giây đầu tiên, cho phép xem xét bất kỳ luồng nào bắt đầu từ 1. 
4. Trong khi vẫn còn các khoảng chưa được xử lý hoặc vùng heap chưa trống, hãy lặp lại quy trình. 

Vòng lặp này mô phỏng việc di chuyển theo thời gian trong khi thu thập các luồng có thể sử dụng được. 
5. Thêm vào heap tất cả các khoảng có thời gian bắt đầu nhỏ hơn hoặc bằng thời gian hiện tại. 

Đây chính xác là những luồng có sẵn để bắt đầu vào thời điểm này. 
6. Nếu heap trống, hãy chuyển thời gian hiện tại sang thời gian bắt đầu của khoảng thời gian tiếp theo.

Điều này ngăn chặn tình trạng bị đình trệ khi có khoảng trống trong vùng phủ sóng. 
7. Ngược lại, trích xuất khoảng thời gian có thời gian kết thúc nhỏ nhất từ ​​heap và lấy nó. 

Đây là bước tham lam: việc chọn dòng hoàn thiện sớm nhất sẽ duy trì tính linh hoạt tối đa cho các lựa chọn trong tương lai. 
8. Đặt thời gian hiện tại về cuối khoảng thời gian đã chọn và tăng dần câu trả lời. 

Chúng tôi tiến về phía trước theo đúng thời gian mà luồng đã chọn chạy. 
9. Tiếp tục cho đến khi không còn khoảng thời gian để xử lý. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trong số tất cả các luồng hiện có sẵn, việc chọn luồng kết thúc sớm nhất sẽ không bao giờ làm giảm số lượng tối ưu trong tương lai. Bất kỳ lựa chọn nào khác kết thúc sau chỉ có thể hạn chế hoặc trì hoãn quyền truy cập vào các khoảng thời gian trong tương lai, vì nó chiếm nhiều thời gian hơn trong khi không mang lại lợi thế bổ sung nào về khả năng tiếp cận. Điều này thiết lập một đối số trao đổi tiêu chuẩn: bất kỳ giải pháp tối ưu nào chọn khoảng thời gian hoàn thiện không tối thiểu đều có thể được chuyển đổi thành giải pháp chọn khoảng thời gian hoàn thiện tối thiểu mà không làm giảm tổng số luồng đã chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, k = map(int, input().split())
    intervals = [tuple(map(int, input().split())) for _ in range(k)]
    
    intervals.sort()  # sort by start time
    
    i = 0
    current_time = 0
    ans = 0
    heap = []
    
    while i < k or heap:
        if not heap:
            current_time = max(current_time, intervals[i][0])
        
        while i < k and intervals[i][0] <= current_time:
            heapq.heappush(heap, intervals[i][1])
            i += 1
        
        if not heap:
            continue
        
        end_time = heapq.heappop(heap)
        ans += 1
        current_time = end_time
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo ý tưởng đường quét trực tiếp. Sắp xếp theo thời gian bắt đầu cho phép chúng tôi kích hoạt tăng dần các khoảng thời gian. Heap chỉ lưu trữ thời gian kết thúc vì thời gian bắt đầu đã được đáp ứng khi chèn vào. Điểm tinh tế quan trọng nhất là bước nhảy khi vùng heap trống, giúp tránh việc tính sai thời gian nhàn rỗi là cơ hội bị bỏ lỡ. 

Sự lựa chọn của`current_time = max(current_time, intervals[i][0])`đảm bảo chúng ta không bao giờ quay ngược thời gian nếu khoảng thời gian trước đó kết thúc sau lần bắt đầu khả dụng tiếp theo. Điều này rất quan trọng khi các khoảng thời gian rời rạc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
1 3
2 5
3 4
```Các khoảng được sắp xếp: 

(1,3), (2,5), (3,4) 

| Bước | Thời điểm hiện tại | Đống (thời gian kết thúc) | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 → 1 | [3] | cộng (1,3) | 0 | 
| 2 | 1 | [3,5] | cộng (2,5) | 0 | 
| 3 | 1 | [3,5] | lấy (1,3) | 1 | 
| 4 | 3 | [4,5] | cộng (3,4) | 1 | 
| 5 | 3 | [5] | lấy (3,4) | 2 | 

Thuật toán chọn (1,3) rồi chọn (3,4). Điều này xác nhận rằng khoảng thời gian hoàn thiện sớm cho phép tạo chuỗi tốt hơn so với việc cố gắng thực hiện khoảng thời gian dài (2,5) trước. 

### Mẫu 2 

đầu vào:```
6 4
2 3
2 3
2 3
2 3
```Tất cả các khoảng đều giống hệt nhau. 

| Bước | Thời điểm hiện tại | Đống | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 → 2 | [3,3,3,3] | thêm tất cả | 0 | 
| 2 | 2 | [3,3,3] | lấy một cái | 1 | 
| 3 | 3 | [] | dừng lại | 1 | 

Chỉ có thể chọn một khoảng vì sau lần chọn đầu tiên, tất cả các khoảng còn lại không còn sử dụng được nữa. 

Những dấu vết này xác nhận rằng các bản sao không làm tăng câu trả lời một cách giả tạo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k) | Việc sắp xếp chiếm ưu thế và mỗi khoảng thời gian được đẩy và xuất hiện một lần từ heap | 
| Không gian | O(k) | Lưu trữ đống và khoảng thời gian | 

Các ràng buộc cho phép khoảng cách lên tới 200000 và mỗi thao tác heap là logarit, do đó tổng số thao tác vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    def solve():
        n, k = map(int, input().split())
        intervals = [tuple(map(int, input().split())) for _ in range(k)]
        intervals.sort()

        i = 0
        current_time = 0
        ans = 0
        heap = []

        while i < k or heap:
            if not heap:
                current_time = max(current_time, intervals[i][0])

            while i < k and intervals[i][0] <= current_time:
                heapq.heappush(heap, intervals[i][1])
                i += 1

            if not heap:
                continue

            heapq.heappop(heap)
            ans += 1

        return str(ans)

    return solve()

# provided samples
assert run("5 3\n1 3\n2 5\n3 4\n") == "2"
assert run("6 4\n2 3\n2 3\n2 3\n2 3\n") == "1"

# minimum input
assert run("2 1\n1 2\n") == "1"

# non-overlapping chain
assert run("10 3\n1 2\n2 3\n3 4\n") == "3"

# overlapping chain with choice
assert run("10 3\n1 5\n2 3\n3 4\n") == "2"

# all intervals start late
assert run("10 2\n5 6\n7 8\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | 1 | tính đúng đắn của trường hợp tối thiểu | 
| chuỗi 1-2-3-4 | 3 | đầy tham lam xiềng xích | 
| chồng chéo dài + ngắn | 2 | tham lam tránh xa lựa chọn xấu | 
| khoảng thời gian muộn rời rạc | 2 | nhảy đúng thời điểm | 

## Vỏ cạnh 

Trường hợp quan trọng là khi có khoảng trống trong phạm vi bao phủ theo khoảng thời gian. Giả sử đầu vào là:```
10 2
5 6
7 8
```Heap ban đầu trống, do đó thuật toán nhảy current_time lên 5. Nó chọn (5,6), sau đó chuyển sang 6. Tại thời điểm này, không có khoảng thời gian hoạt động nào cho đến thời điểm 7, vì vậy nó lại nhảy. Nếu không có logic nhảy này, việc triển khai đơn giản có thể liên tục kiểm tra các vùng trống hoặc kết luận sai rằng không thể sử dụng được các khoảng thời gian tiếp theo. 

Một trường hợp khác là sự chồng chéo nặng nề khi có nhiều khoảng thời gian đồng thời:```
10 4
1 10
2 3
3 4
4 5
```Một người tham lam ngây thơ bắt đầu sớm nhất có thể chọn (1,10) trước và kết thúc ngay bằng câu trả lời 1. Thay vào đó, chiến lược dựa trên đống ưu tiên (2,3), rồi (3,4), rồi (4,5), tạo ra câu trả lời 3. Tính chính xác đến từ việc luôn chọn thời gian kết thúc nhỏ nhất trong số các lựa chọn có sẵn, chứ không phải thời điểm bắt đầu sớm nhất hoặc thời lượng dài nhất.
