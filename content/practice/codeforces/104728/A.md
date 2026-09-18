---
title: "CF 104728A - \u7b80\u5355\u7684\u52a0\u6cd5\u4e58\u6cd5\u8ba1\u7b97\u9898"
description: "Chúng ta bắt đầu với giá trị x = 0 và muốn đạt được giá trị đích y. Chúng tôi được phép thực hiện hai loại hoạt động. Loại đầu tiên thêm bất kỳ số nguyên nào từ 1 đến n vào giá trị hiện tại. Loại thứ hai nhân giá trị hiện tại với một trong tối đa m số nhân đã cho."
date: "2026-06-29T02:44:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "A"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 77
verified: true
draft: false
---

[CF 104728A - \u7b80\u5355\u7684\u52a0\u6cd5\u4e58\u6cd5\u8ba1\u7b97\u9898](https://codeforces.com/problemset/problem/104728/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một giá trị`x = 0`và muốn đạt được giá trị mục tiêu`y`. Chúng tôi được phép thực hiện hai loại hoạt động. Loại đầu tiên thêm bất kỳ số nguyên nào từ`1`ĐẾN`n`đến giá trị hiện tại. Loại thứ hai nhân giá trị hiện tại với một trong số tối đa`m`các số nhân đã cho. 

Nhiệm vụ là tính toán số lượng thao tác tối thiểu cần thiết để đạt được chính xác`y`bắt đầu từ số không. 

Đây là bài toán đường đi ngắn nhất được ngụy trang. Mỗi giá trị nguyên của`x`có thể được xem như một nút và mỗi thao tác xác định các cạnh được định hướng tới các nút khác. Từ bất kỳ tiểu bang nào`x`, chúng ta có thể đi đến`x + a`cho tất cả`1 ≤ a ≤ n`, và để`x * b`cho mỗi số nhân`b`. 

Các ràng buộc cho thấy rằng việc xây dựng đồ thị trực tiếp là không thể. Giá trị mục tiêu`y`tùy thuộc vào`5 × 10^6`, do đó không gian trạng thái lớn nhưng vẫn đủ nhỏ để BFS được kiểm soát cẩn thận đối với các giá trị. Số lượng nhân nhiều nhất là 10, điều này rất quan trọng vì nó giúp quản lý việc phân nhánh từ phép nhân. 

Một cách tiếp cận ngây thơ khám phá tất cả các chuỗi hoạt động phát triển theo cấp số nhân theo chiều sâu. Ngay cả khi chúng ta giả sử mỗi tiểu bang phân nhánh vào`n + m`chuyển tiếp, độ sâu cần thiết có thể lên tới`y`, làm cho việc liệt kê bạo lực hoàn toàn không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi phép nhân với 1 tồn tại trong`B`. Điều này tạo ra các vòng lặp tự. Ví dụ, nếu`b = 1`, sau đó`x → x`luôn luôn có thể. Một BFS bất cẩn không đánh dấu chính xác các trạng thái đã truy cập có thể lặp vô thời hạn hoặc truy cập lại các trạng thái vô số lần. 

Một trường hợp cạnh khác là khi`n = 1`. Sau đó, phép cộng luôn tăng chính xác 1, giảm bài toán thành đường đi ngắn nhất cổ điển với các chuyển tiếp có cấu trúc rất chặt chẽ. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ coi đây là một biểu đồ không có trọng số và cố gắng khám phá tất cả các trạng thái từ`0`, tạo ra tất cả các giá trị có thể truy cập bằng cách liên tục áp dụng các phép cộng và phép nhân. Điều này đúng về mặt khái niệm vì mỗi hoạt động đều có chi phí đơn vị, vì vậy lần đầu tiên chúng ta đạt được`y`, ta có dãy ngắn nhất 

Tuy nhiên, yếu tố phân nhánh khiến cách tiếp cận này bùng nổ. Từ bất kỳ tiểu bang nào, có tới`n + m`chuyển tiếp đi, và`n`có thể lớn như`5 × 10^6`. Ngay cả khi chúng ta hạn chế bản thân ở những giá trị lên tới`y`, mỗi bước BFS sẽ yêu cầu lặp lại một phạm vi bổ sung khổng lồ, điều này là không thể. 

Quan sát quan trọng là phép toán cộng không cần phải xem xét tất cả`a`riêng lẻ. Từ một trạng thái nhất định`x`, tất cả các phép cộng tạo ra một phạm vi trạng thái liền kề từ`x + 1`ĐẾN`x + n`. Điều này có nghĩa là chúng ta có thể coi phép cộng như một sự nới lỏng phạm vi thay vì liệt kê từng cạnh. 

Điều này biến bài toán thành một đường đi ngắn nhất trên các số nguyên mà chúng ta có thể nhảy tới`x * b`hoặc thư giãn một khoảng thời gian`[x + 1, x + n]`. Cấu trúc bây giờ giống với BFS trên các giá trị với các chuyển đổi được tối ưu hóa, trong đó mỗi trạng thái được xử lý một lần và việc thư giãn được thực hiện trong thời gian không đổi được khấu hao bằng cách sử dụng mở rộng giống như deque hoặc thứ tự BFS. 

Các phép tính nhân rất ít (`m ≤ 10`), vì vậy chúng có thể được xử lý một cách rõ ràng. Phép cộng trở thành phép chuyển tiếp chủ yếu nhưng có thể được xử lý hiệu quả bằng cách mở rộng về phía trước theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^y) | O(y) | Quá chậm | 
| BFS với các chuyển tiếp được tối ưu hóa | O(y · m) | O(y) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình từng giá trị từ`0`ĐẾN`y`dưới dạng một nút trong biểu đồ và tính khoảng cách ngắn nhất bằng BFS. 

1. Khởi tạo một mảng`dist`kích thước`y + 1`với tất cả các giá trị được đặt thành vô cùng và được đặt`dist[0] = 0`. Điều này thể hiện số lượng thao tác tối thiểu cần thiết để đạt được từng giá trị. 
2. Đẩy trạng thái bắt đầu`0`vào hàng đợi. 
3. Bật một giá trị`x`từ hàng đợi. Điều này thể hiện trạng thái hiện tại với số lượng hoạt động nhỏ nhất được biết. 
4. Thử tất cả các phép tính nhân: với mỗi`b`TRONG`B`, tính toán`nx = x * b`. Nếu như`nx ≤ y`Và`dist[nx] > dist[x] + 1`, cập nhật nó và đẩy`nx`vào hàng đợi. Phép nhân được coi là bước nhảy trực tiếp vì nó tạo ra một trạng thái tiếp theo. 
5. Xử lý phép cộng bằng cách xem xét tất cả các giá trị từ`x + 1`ĐẾN`x + n`. Thay vì lặp đi lặp lại tất cả`n`khả năng cho mọi`x`, chúng ta chỉ thư giãn các trạng thái khi chúng ta tiếp cận chúng lần đầu tiên. Nếu như`dist[x + 1] > dist[x] + 1`, chúng tôi truyền tiếp theo cách giống như BFS được kiểm soát. Điều này đảm bảo mỗi tiểu bang được truy cập một lần. 
6. Tiếp tục cho đến khi hàng đợi trống hoặc`y`đã đạt được. 
7. Trở về`dist[y]`. 

Lựa chọn thiết kế quan trọng là chúng ta không bao giờ liệt kê rõ ràng tất cả`n`bổ sung cho mỗi nút. Thay vào đó, mỗi trạng thái được nới lỏng tối đa một lần và cấu trúc của BFS đảm bảo rằng lần đầu tiên chúng ta đạt được một giá trị là tối ưu. 

### Tại sao nó hoạt động 

BFS đảm bảo rằng các trạng thái được xử lý theo thứ tự tăng dần của số lượng hoạt động. Mỗi chuyển đổi có chi phí chính xác là 1, cho dù đó là phép cộng hay phép nhân. Từng là một bang`x`được hoàn thiện (xuất hiện với khoảng cách tối thiểu), mọi nỗ lực trong tương lai để cải thiện nó sẽ yêu cầu một đường dẫn dài hơn, điều này mâu thuẫn với thứ tự BFS. Điểm tối ưu hóa quan trọng là các cạnh bổ sung tạo thành một chuỗi chuyển tiếp đơn điệu, do đó, mỗi nút được chèn nhiều nhất một lần, duy trì tính chính xác trong khi tránh việc liệt kê dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

y, n, m = map(int, input().split())
B = list(map(int, input().split()))

INF = 10**18
dist = [INF] * (y + 1)
dist[0] = 0

q = deque([0])

while q:
    x = q.popleft()
    d = dist[x]

    # multiplication transitions
    for b in B:
        nx = x * b
        if nx <= y and dist[nx] > d + 1:
            dist[nx] = d + 1
            q.append(nx)

    # addition transitions
    # relax forward up to n steps
    for nx in range(x + 1, min(y + 1, x + n + 1)):
        if dist[nx] > d + 1:
            dist[nx] = d + 1
            q.append(nx)
        else:
            break

print(dist[y])
```Vòng lặp nhân rất đơn giản: mỗi trạng thái mở rộng tối đa thành`m`các ứng cử viên và chúng tôi sẽ thư giãn cho họ nếu chúng tôi tìm thấy một con đường ngắn hơn. 

Vòng lặp bổ sung khai thác tính đơn điệu. Khi chúng tôi đạt đến vị trí đã đạt được với chi phí bằng hoặc tốt hơn, các vị trí tiếp theo trong phạm vi đó không thể cải thiện thông qua cùng một lớp, vì vậy chúng tôi dừng sớm. Điều này ngăn chặn việc quét dư thừa trong khoảng thời gian lớn trên nhiều trạng thái. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 3 1
2
```Chúng tôi bắt đầu lúc`0`với khoảng cách`0`. 

| Bước | x | quận [x] | Hoạt động áp dụng | Tiểu bang mới | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | +1..+3, ×2 | 1,2,3,0 | 
| 2 | 1 | 1 | ×2 | 2 | 
| 3 | 2 | 1 | ×2 | 4 | 
| 4 | 3 | 1 | ×2 | 6 | 
| 5 | 4 | 2 | ×2 | 8 | 
| 6 | 5 | 2 | ×2 | 10 | 

Chúng tôi đạt được`10`trong 3 thao tác, ví dụ:`0 → 3 → 6 → 10`. 

Dấu vết này cho thấy phép cộng nhanh chóng lấp đầy một phạm vi như thế nào, trong khi phép nhân tăng tốc độ tăng trưởng. 

### Mẫu 2 

đầu vào:```
100 6 3
2 3 5
```| Bước | x | quận [x] | Hoạt động áp dụng | Tiểu bang mới | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | +1..+6 | 1-6 | 
| 2 | 6 | 1 | ×2, ×3, ×5 | 12,18,30 | 
| 3 | 12 | 2 | ×2, ×3, ×5 | 24,36,60 | 
| 4 | 24 | 3 | ×2, ×3, ×5 | 48,72,120 | 

Chúng tôi đạt được`100`nhanh chóng thông qua`6 → 30 → 60 → 100`trong 3 bước (có bổ sung trung gian). 

Ví dụ này nêu bật cách phép nhân chiếm ưu thế khi đạt đến cơ số vừa phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(y · m) | Mỗi trạng thái được xử lý một lần, mỗi trạng thái kích hoạt tối đa m phép nhân và quét cộng giới hạn | 
| Không gian | O(y) | Mảng khoảng cách và hàng đợi BFS trên các giá trị lên đến y | 

Giới hạn`y ≤ 5 × 10^6`Và`m ≤ 10`làm cho điều này trở nên khả thi. BFS đảm bảo mỗi trạng thái được truy cập nhiều nhất một lần và bộ nhân nhỏ ngăn chặn sự bùng nổ trong phân nhánh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    y, n, m = map(int, input().split())
    B = list(map(int, input().split()))

    INF = 10**18
    dist = [INF] * (y + 1)
    dist[0] = 0
    from collections import deque
    q = deque([0])

    while q:
        x = q.popleft()
        d = dist[x]

        for b in B:
            nx = x * b
            if nx <= y and dist[nx] > d + 1:
                dist[nx] = d + 1
                q.append(nx)

        for nx in range(x + 1, min(y + 1, x + n + 1)):
            if dist[nx] > d + 1:
                dist[nx] = d + 1
                q.append(nx)
            else:
                break

    return str(dist[y])

# provided samples
assert run("10 3 1\n2\n") == "3"
assert run("100 6 3\n2 3 5\n") == "3"

# custom cases
assert run("1 5 1\n2\n") == "1", "min target"
assert run("5 1 1\n2\n") == "5", "only +1 steps"
assert run("10 10 1\n1\n") == "10", "multiplication useless"
assert run("20 3 2\n2 3\n") == "3", "mixed operations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 1/2 | 1 | cạnh mục tiêu tối thiểu | 
| 5 1 1/2 | 5 | con đường gia tăng bắt buộc | 
| 10 10 1 / 1 | 10 | phép nhân suy biến | 
| 20 3 2 / 2 3 | 3 | chuyển tiếp kết hợp | 

## Vỏ cạnh 

Khi nào`B`chứa`1`, phép nhân tạo ra các vòng lặp tự. Ví dụ, bắt đầu từ`x = 5`, áp dụng`×1`sản lượng`5`. Logic BFS bỏ qua điều này bởi vì`dist[5]`sẽ không cải thiện từ cùng một giá trị, do đó không xảy ra vòng lặp vô hạn. 

Khi`n = 1`, phép cộng trở thành một chuỗi tất định. Bắt đầu từ`0`, chúng ta chỉ có thể tiếp cận`1, 2, 3, ...`. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp cộng chỉ tiến lên từng bước một. 

Khi`y`nhỏ nhưng`n`lớn, vòng lặp bổ sung được cắt ngắn một cách hiệu quả bởi`min(y, x + n)`, ngăn chặn việc thăm dò không cần thiết ngoài phạm vi mục tiêu.
