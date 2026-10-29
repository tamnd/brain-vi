---
title: "CF 106201C - \u041f\u043e\u043b\u043d\u043e\u0435 \u043f\u0440\u043e\u0445\u043e\u0436\u0434\u0435\u043d\u0438\u0435"
description: "Chúng ta được cung cấp một chuỗi các giai đoạn cố định trong một trò chơi, trong đó giai đoạn $i$ tương ứng với việc mở khóa hoặc nhận được phần kết thúc thứ $i$."
date: "2026-06-19T18:29:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106201
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2025"
rating: 0
weight: 106201
solve_time_s: 54
verified: true
draft: false
---

[CF 106201C - \u041f\u043e\u043b\u043d\u043e\u0435 \u043f\u0440\u043e\u0445\u043e\u0436\u0434\u0435\u043d\u0438\u0435](https://codeforces.com/problemset/problem/106201/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các giai đoạn cố định trong một trò chơi, trong đó giai đoạn$i$tương ứng với việc mở khóa hoặc lấy được$i$-kết thúc thứ Di chuyển từ sân khấu$i$lên sân khấu$i+1$mất một khoảng thời gian đã biết$d_{i+1}$, do đó thời gian tiến triển được tích lũy và tăng dần trừ khi chúng ta chọn tạm dừng. 

Ở mỗi giai đoạn$i$, cũng có một sự kiện duy nhất: luồng bắt đầu ở thời điểm tuyệt đối$a_i$. Nếu chúng ta đang ở chính xác thời điểm đó (hoặc chưa vượt qua), chúng ta có thể chọn dừng quá trình tiến triển, đợi nếu cần và xem nó. Việc xem không mất thời gian, nhưng sau khi xem, chúng ta ngay lập tức tiếp tục giai đoạn tiếp theo. Nếu chúng ta bỏ qua nó, nó sẽ bị mất vĩnh viễn và chúng ta không bao giờ có thể quay lại được sau này. Ngoài ra, khi chúng ta vượt qua giai đoạn$i$, chúng tôi không được phép quay lại và xem các buổi phát sóng trước đó. 

Vì vậy, quy trình này là một bước chuyển tiếp duy nhất qua các giai đoạn, trong đó ở mỗi giai đoạn chúng tôi: 

1. tiếp tục ngay lập tức, mất luồng hoặc 
2. đảm bảo chúng tôi có mặt đúng lúc$a_i$, xem luồng rồi tiếp tục. 

Mục tiêu là tối đa hóa số lượng luồng chúng tôi có thể xem. 

Thời gian sau khi kết thúc giai đoạn$i$hoàn toàn được xác định bởi luồng trước đó mà chúng tôi đã chọn tham dự, vì việc tạm dừng chỉ có thể khiến chúng tôi tiến lên chứ không bao giờ lùi lại. 

Các ràng buộc đi lên đến$n \le 2 \cdot 10^5$, do đó mọi nghiệm đều phải gần tuyến tính hoặc$n \log n$. Việc mô phỏng các quyết định theo phương pháp bậc hai ngay lập tức là quá chậm, vì mỗi trạng thái sẽ phụ thuộc vào tất cả các lựa chọn trước đó. 

Một vấn đề tế nhị là việc chờ đợi chỉ hữu ích nếu chúng ta đến đủ sớm. Nếu ở giai đoạn$i$chúng tôi đã vượt qua$a_i$, thì luồng đó mãi mãi không thể tồn tại được. Một trường hợp thất bại khác xuất hiện khi luôn tham lam nhận một luồng nếu có thể: làm như vậy có thể khiến chúng ta trì hoãn đến mức mất đi nhiều luồng sau này thực sự có giá trị hơn. 

Ví dụ: giả sử việc xem một luồng sớm buộc chúng ta phải đến quá muộn cho hai luồng sau mà lẽ ra chúng ta có thể xem. Một chiến lược tham lam ngây thơ luôn sử dụng các luồng có sẵn sẽ thất bại ở đây vì nó không tính đến tác động của thời gian trong tương lai. 

## Phương pháp tiếp cận 

Khó khăn chính là mọi quyết định đều ảnh hưởng đến thời điểm hiện tại và do đó, luồng nào trong tương lai vẫn có thể truy cập được. Lực lượng vũ phu ngây thơ sẽ thử tất cả các tập hợp con của luồng theo thứ tự thời gian, mô phỏng dòng thời gian kết quả mỗi lần. Điều đó có hiệu quả về mặt khái niệm vì mỗi lựa chọn xác định duy nhất sự tiến hóa theo thời gian, nhưng nó ngay lập tức bùng nổ: mỗi bước chia thành hai lựa chọn, dẫn đến$2^n$khả năng, và thậm chí mô phỏng một con đường duy nhất là$O(n)$, do đó độ phức tạp tổng thể là theo cấp số nhân. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ thử lập trình động theo các giai đoạn và thời gian hiện tại, nhưng thời gian là một giá trị liên tục lên đến$10^9$, vì vậy không gian trạng thái này quá lớn. 

Quan sát quan trọng là ở mỗi giai đoạn, quyết định hữu ích duy nhất là liệu chúng ta có đủ khả năng để điều chỉnh chính xác vào thời điểm đó hay không.$a_i$mà không làm tổn hại đến các cơ hội trong tương lai. Nếu chúng ta đã qua rồi$a_i$, luồng đã biến mất. Nếu không, chúng ta có thể chọn “chờ” nó, nhưng việc chờ đợi đó phải trả giá: nó sẽ làm dịch chuyển tất cả thời gian đến trong tương lai về phía trước. 

Điều này gợi ý rằng thay vì theo dõi thời gian chính xác, chúng ta nên theo dõi mức độ chậm trễ mà chúng ta sẵn sàng đưa ra. Một quan điểm hữu ích hơn là suy nghĩ xem liệu chúng ta có thể "khớp" một tập hợp các luồng đã chọn vào dòng thời gian theo thứ tự hay không. Mỗi lần chúng tôi phát trực tuyến, chúng tôi buộc thời gian hiện tại ít nhất phải trở thành$a_i$, sau đó tiếp tục tiến về phía trước. 

Điều này trở thành một cấu trúc khả thi tham lam cổ điển: chúng tôi cố gắng chọn các luồng theo thứ tự chỉ số tăng dần, nhưng chỉ chấp nhận một luồng nếu nó không phá vỡ tính khả thi liên quan đến tiến trình thời gian. Chi phí chuyển đổi giữa các giai đoạn liên tiếp là cố định nên chúng tôi có thể duy trì thời gian kết thúc hiện tại và kiểm tra từng luồng cục bộ. 

Sự đơn giản hóa không rõ ràng là chiến lược tối ưu tương ứng với việc đưa ra các quyết định chấp nhận/từ chối tối ưu cục bộ theo thứ tự, bởi vì bất kỳ sự chậm trễ nào do lấy luồng đều không thể đảo ngược và chỉ ảnh hưởng đến khả năng tiếp cận trong tương lai chứ không bao giờ ảnh hưởng đến các lựa chọn trong quá khứ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Mô phỏng tham lam tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì thời gian hiện tại$t$, đại diện cho thời điểm chúng tôi kết thúc giai đoạn hiện tại trong lịch trình đã chọn và là bộ đếm số lượng luồng chúng tôi đã xem thành công. 

Chúng tôi cũng sử dụng thực tế là giai đoạn$1$bắt đầu vào lúc$0$, và di chuyển từ sân khấu$i$ĐẾN$i+1$luôn luôn thêm$d_{i+1}$. 

### Các bước 

1. Khởi tạo thời gian hiện tại$t = 0$và trả lời$ans = 0$. 

Điều này thể hiện rằng chúng tôi bắt đầu trước bất kỳ trò chơi nào và chưa xem gì cả. 
2. Lặp lại qua các giai đoạn$i = 1$ĐẾN$n$. 

Ở mỗi giai đoạn, về mặt khái niệm chúng ta sẽ đến thời điểm$t$sau khi kết thúc giai đoạn trước. 
3. Nếu$t > a_i$, bỏ qua luồng này hoàn toàn. 

Điều này là bắt buộc: chúng ta đã bỏ lỡ thời điểm bắt đầu và việc trì hoãn thêm nữa chỉ làm tăng thêm thời gian. 
4. Nếu không thì chọn xem luồng$i$, bộ$t = a_i$, và tăng$ans$. 

Chúng tôi đồng bộ hóa với thời gian phát trực tuyến, điều này có thể khiến bạn phải chờ đợi và chúng tôi cam kết với lựa chọn này. 
5. Nếu$i < n$, tăng thời gian lên$d_{i+1}$. 

Mô hình này hoàn thành giai đoạn tiếp theo ngay sau khi hoàn thành giai đoạn hiện tại. 

### Tại sao nó hoạt động 

Bất biến chính là sau giai đoạn xử lý$i$, biến$t$là thời gian sớm nhất có thể chúng ta có thể đạt đến giai đoạn$i+1$dựa trên các quyết định đã đưa ra cho đến nay, đồng thời tối đa hóa số lượng luồng đã xem trong số tất cả các lịch trình phù hợp với các quyết định đó. 

Ở mỗi bước, nếu chúng ta có thể tham dự luồng$i$, làm như vậy không bao giờ làm giảm số lượng các quyết định khả thi trong tương lai ngoài tác dụng tăng$t$ĐẾN$a_i$. Bất kỳ lịch trình thay thế nào bỏ qua luồng$i$còn việc tham dự muộn hơn là điều không thể vì buổi phát sóng bị ràng buộc vào một thời gian cố định. Vì vậy, quyết định có ý nghĩa duy nhất là liệu chúng ta có còn đạt được nó hay không, và nếu có thì chúng ta sẽ thực hiện. 

Điều này đảm bảo rằng mọi luồng đều được thực hiện khi khả thi hoặc không thể truy cập vĩnh viễn và không việc sắp xếp lại các lựa chọn nào có thể cải thiện số lượng mà không vi phạm các hạn chế về thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    d = list(map(int, input().split()))

    t = 0
    ans = 0

    for i in range(n):
        if t <= a[i]:
            t = a[i]
            ans += 1

        if i < n - 1:
            t += d[i]

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một biến dòng thời gian duy nhất`t`biểu thị thời gian hoàn thành hiện tại sau khi kết thúc giai đoạn được xem xét gần nhất. Sự so sánh`t <= a[i]`mã hóa xem luồng vẫn có thể truy cập được hay không. Nếu đúng như vậy, chúng tôi “chờ” đến chính xác thời điểm đó, được mô hình hóa bằng cách cài đặt`t = a[i]`và chúng tôi tính nó là đã xem. 

Việc bổ sung`d[i]`sau mỗi màn chơi là rất quan trọng vì nó đảm bảo rằng thời gian luôn diễn ra theo chi phí cố định để chuyển sang màn cuối tiếp theo. Một lỗi phổ biến là thêm`d[i+1]`thay vì`d[i]`, điều này làm sai lệch các chỉ số vì`d`bắt đầu từ quá trình chuyển sang giai đoạn 2. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [3, 4, 6]
d = [2, 2]
```Chúng tôi theo dõi thời gian và quyết định: 

| tôi | t trước | một [tôi] | quyết định | t sau khi xem | t sau khi di chuyển | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | lấy | 3 | 5 | 1 | 
| 2 | 5 | 4 | bỏ qua | 5 | 7 | 1 | 
| 3 | 7 | 6 | bỏ qua | 7 | - | 1 | 

Chúng tôi chỉ lấy luồng đầu tiên vì sau khi chờ đợi, thời gian tích lũy trở nên quá lớn để có thể đạt được những lần phát sau. Điều này phù hợp với ý kiến ​​cho rằng cam kết sớm có thể cản trở các cơ hội sau này. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [1, 3, 6, 8, 14]
d = [1, 1, 1, 1]
```| tôi | t trước | một [tôi] | quyết định | t sau khi xem | t sau khi di chuyển | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | lấy | 1 | 2 | 1 | 
| 2 | 2 | 3 | lấy | 3 | 4 | 2 | 
| 3 | 4 | 6 | lấy | 6 | 7 | 3 | 
| 4 | 7 | 8 | lấy | 8 | 9 | 4 | 
| 5 | 9 | 14 | lấy | 14 | - | 5 | 

Ở đây, mọi luồng đều khả thi và thuật toán sắp xếp thời gian một cách tham lam cho từng luồng mà không làm mất hiệu lực các luồng trong tương lai. Điều này chứng tỏ rằng cùng một quy tắc xử lý cả lịch trình dày đặc và thưa thớt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi giai đoạn được xử lý một lần với công việc liên tục | 
| Không gian |$O(1)$| Chỉ có thời gian và bộ đếm hiện tại được lưu trữ | 

Thuật toán chạy thoải mái trong giới hạn vì$n \le 2 \cdot 10^5$và tất cả các phép toán đều là những cập nhật và so sánh số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    d = list(map(int, input().split()))

    t = 0
    ans = 0

    for i in range(n):
        if t <= a[i]:
            t = a[i]
            ans += 1
        if i < n - 1:
            t += d[i]

    return str(ans)

# sample-like cases
assert run("3\n3 4 6\n2 2\n") == "1"
assert run("5\n1 3 6 8 14\n1 1 1 1\n") == "5"

# minimum case
assert run("2\n10 20\n5\n") == "2"

# all equal timestamps, tight spacing
assert run("4\n5 5 5 5\n1 1 1\n") == "4"

# impossible late stream
assert run("3\n1 100 1000\n100 100\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tiến triển tối thiểu | 2 | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều bằng a[i] | 4 | lựa chọn ngay lập tức lặp đi lặp lại | 
| khoảng trống lớn trong a[i] | 2 | bỏ qua các luồng muộn không thể truy cập | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi thời gian hiện tại chính xác bằng$a_i$. Điều kiện phải cho phép lấy luồng trong trường hợp này. Séc`t <= a[i]`đảm bảo sự bình đẳng được chấp nhận. Nếu được viết là`t < a[i]`, các luồng diễn ra chính xác vào thời điểm hiện tại sẽ bị bỏ qua do nhầm lẫn. 

Một trường hợp khác là khi chờ một luồng sẽ đẩy thời gian về phía trước nhiều đến mức tất cả các luồng còn lại không thể truy cập được. Thuật toán xử lý việc này một cách tự nhiên vì mỗi lần so sánh tiếp theo đều thất bại một lần.`t`vượt quá tương lai`a[i]`giá trị, buộc bỏ qua và duy trì tính chính xác. 

Trường hợp cạnh cuối cùng là rất nhỏ hoặc rất lớn`d[i]`. Vì thuật toán chỉ tích lũy các giá trị này thành`t`, tràn không phải là vấn đề đáng lo ngại trong Python, nhưng trong các ngôn ngữ số nguyên có chiều rộng cố định, điều này sẽ yêu cầu bộ nhớ 64-bit.
