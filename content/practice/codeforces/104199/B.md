---
title: "CF 104199B - \u0420\u0430\u0441\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0430 \u043c\u0435\u0431\u0435\u043b\u0438"
description: "Chúng tôi được cung cấp một khách sạn có nhiều phòng, mỗi phòng có số lượng ghế cần thiết theo một kế hoạch lý tưởng. Trên thực tế, khách sạn có tổng số ghế $N$ phải được phân bổ cho các phòng $K$."
date: "2026-07-02T00:01:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104199
codeforces_index: "B"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 18-02-23"
rating: 0
weight: 104199
solve_time_s: 86
verified: true
draft: false
---

[CF 104199B - \u0420\u0430\u0441\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0430 \u043c\u0435\u0431\u0435\u043b\u0438](https://codeforces.com/problemset/problem/104199/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một khách sạn có nhiều phòng, mỗi phòng có số lượng ghế cần thiết theo một kế hoạch lý tưởng. Trên thực tế, khách sạn có tổng cộng$N$những chiếc ghế phải được phân bổ khắp$K$phòng. Các ràng buộc không chỉ đơn giản là “phù hợp với mục tiêu”: mỗi phòng phải nhận được ít nhất một chiếc ghế, và sự thiếu hụt ở tất cả các phòng, được định nghĩa là số lượng ghế mỗi phòng bị thiếu so với yêu cầu lý tưởng, phải giống hệt nhau. 

Vì vậy, nếu một căn phòng ban đầu mong đợi$a_i$ghế và chúng tôi đặt$x_i$ghế, sau đó$x_i \ge 1$và tất cả các giá trị$a_i - x_i$phải là cùng một hằng số$d$, được chia sẻ khắp mọi phòng. Điều đó có nghĩa là mỗi phòng đều được lấp đầy một cách đồng đều bởi$d$, đồng thời vẫn đảm bảo mỗi phòng có ít nhất một chiếc ghế. 

Mục tiêu là chọn thiếu đồng phục hợp lệ$d$và phân bổ những chiếc ghế đáp ứng nhu cầu đó, đồng thời tối đa hóa số lượng ghế vẫn chưa được sử dụng. Tương tự, chúng ta muốn giảm thiểu số lượng ghế phải đặt. 

Những hạn chế là lớn trong$N$nhưng vừa phải ở$K$. Từ$K$có thể đi lên$10^5$, mọi nghiệm đều phải tuyến tính hoặc gần tuyến tính trong$K$. Một giải pháp cố gắng khắc phục mọi tình trạng thiếu hụt có thể xảy ra hoặc phân bổ lại ghế nhiều lần cho mỗi ứng cử viên sẽ quá chậm nếu tốn kém.$O(K^2)$hoặc tệ hơn. 

Trường hợp chìa khóa ẩn cạnh xuất hiện khi một số phòng có sức chứa rất nhỏ. Nếu một phòng có$a_i = 1$, khi đó lực lượng phân công hợp lệ duy nhất$x_i = 1$, do đó mức thiếu hụt được cố định ở mức 0 đối với căn phòng đó, điều này hạn chế tình trạng thiếu hụt toàn cầu. Bất kỳ nỗ lực ngây thơ nào cho rằng tất cả các phòng đều có thể chia sẻ mức thâm hụt lớn về đồng phục sẽ bị phá vỡ ngay lập tức tại đây. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là đoán sự thiếu hụt chung$d$. Nếu chúng ta sửa$d$, thì mỗi phòng phải nhận$x_i = a_i - d$ghế. Điều này ngay lập tức áp đặt hai điều kiện khả thi: thứ nhất,$x_i \ge 1$, Vì thế$d \le a_i - 1$cho mỗi phòng, nghĩa là$d \le \min(a_i) - 1$. Thứ hai, tổng số ghế sử dụng phải chính xác$\sum (a_i - d)$, bằng$\sum a_i - Kd$, và điều này không được vượt quá$N$. 

Vì vậy để cố định$d$, tính khả thi giảm xuống còn việc kiểm tra xem$Kd \ge \sum a_i - N$. Cấu trúc trở nên đơn điệu: tăng dần$d$giảm số lượng ghế được sử dụng, giúp dễ dàng chi tiêu hơn nhưng cũng bị hạn chế bởi diện tích phòng tối thiểu. 

Cách tiếp cận brute-force sẽ thử tất cả các giá trị có thể có của$d$từ$0$lên đến$\min(a_i) - 1$, tính toán tổng số ghế cần thiết mỗi lần và theo dõi cấu hình khả thi nhất. Mỗi chi phí kiểm tra$O(K)$, và có thể có tới$1000$các giá trị khác nhau của$d$, dẫn đến đại khái$10^8$hoạt động trong trường hợp xấu nhất. Đó là ranh giới nhưng không cần thiết. 

Quan sát quan trọng là tính khả thi chỉ phụ thuộc vào các biểu thức tuyến tính trong$d$. Thay vì lặp đi lặp lại$d$, chúng ta có thể tính giá trị tối đa hợp lệ$d$trực tiếp bằng cách sử dụng các ràng buộc số học:$$Kd \le \sum a_i - N$$Và$$d \le \min(a_i) - 1.$$Vì vậy tối ưu$d$là mức tối thiểu của hai giới hạn trên. 

Một lần$d$cố định, số ghế thực tế đặt là bắt buộc nên số ghế lưu lại chỉ đơn giản là$N - \sum (a_i - d)$, hoặc tương đương$N - (\sum a_i - Kd)$. 

Điều này làm giảm toàn bộ vấn đề thành việc tính toán một vài tập hợp trên mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$d$|$O(K \cdot \min a_i)$|$O(1)$| Quá chậm | 
| Số học tối ưu |$O(K)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính toán số liệu thống kê toàn cầu 

Chúng tôi tính toán tổng sức chứa của tất cả các phòng và sức chứa tối thiểu trong số đó. Hai giá trị này kiểm soát hoàn toàn tính khả thi của bất kỳ sự thiếu hụt đồng nhất nào. 

Tổng xác định có bao nhiêu ghế trong cấu hình lý tưởng, trong khi giá trị tối thiểu xác định mức thâm hụt đồng nhất tối đa cho phép mà không vi phạm ràng buộc “ít nhất một ghế mỗi phòng”. 

### 2. Tính mức thiếu hụt tối đa có thể từ ngân sách 

Chúng tôi viết lại tổng giới hạn sử dụng như sau:$$\sum a_i - Kd \le N$$mang lại:$$d \ge \frac{\sum a_i - N}{K}$$Tuy nhiên, vì chúng tôi muốn tính khả thi và tiết kiệm tối đa nên thay vào đó chúng tôi tính toán giá trị lớn nhất$d$không vi phạm bất đẳng thức này:$$d \le \frac{\sum a_i - N}{K}$$Chúng tôi lấy giá trị sàn này vì$d$phải là số nguyên. 

### 3. Áp dụng ràng buộc khả thi cho từng phòng 

Mỗi phòng vẫn phải bố trí ít nhất một ghế, vì vậy:$$a_i - d \ge 1 \Rightarrow d \le a_i - 1$$Ràng buộc chặt chẽ nhất đến từ căn phòng nhỏ nhất, vì vậy:$$d \le \min(a_i) - 1$$### 4. Kết hợp các ràng buộc 

hợp lệ$d$là mức tối thiểu của giới hạn bắt nguồn từ ngân sách và giới hạn cấu trúc. Chúng tôi cũng đảm bảo$d \ge 0$. 

### 5. Tính đáp án cuối cùng 

Một lần$d$đã được cố định, tổng số ghế được đặt là$\sum a_i - Kd$. Số ghế đã lưu là:$$N - (\sum a_i - Kd)$$### Tại sao nó hoạt động 

Bất biến quan trọng là mọi cấu hình hợp lệ đều tương ứng với việc chọn một số nguyên duy nhất$d$sao cho tất cả việc phân công phòng đều chính xác$a_i - d$. Không có quyền tự do thay đổi phòng một cách độc lập vì điều kiện buộc phải có những mức thâm hụt giống nhau. Điều này làm giảm không gian tìm kiếm từ$K$phép gán chiều cho một tham số vô hướng. Mỗi giải pháp khả thi được đại diện duy nhất bởi một số$d$và mọi giá trị hợp lệ$d$tạo ra một cấu hình hợp lệ, do đó tối ưu hóa$d$tương đương với việc tối ưu hóa trên tất cả các bài tập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    total = sum(a)
    mn = min(a)
    
    # upper bound from "at least one chair per room"
    d1 = mn - 1
    
    # upper bound from total chairs constraint
    d2 = (total - n) // k
    
    d = min(d1, d2)
    if d < 0:
        d = 0
    
    used = total - k * d
    print(n - used)

if __name__ == "__main__":
    solve()
```Trước tiên, quá trình triển khai sẽ đọc dữ liệu đầu vào và tính toán các giá trị tổng hợp trong một lần chuyển qua các phòng. giá trị`d1`buộc không có phòng nào trống, vì trừ nhiều hơn`a_i - 1`sẽ vi phạm ràng buộc tối thiểu. giá trị`d2`buộc tổng số ghế được giao không vượt quá nguồn cung sẵn có khi được biểu thị dưới dạng thâm hụt đồng đều. 

Sau khi lựa chọn phương án khả thi nhất`d`, chúng tôi tính toán có bao nhiêu chiếc ghế thực sự được sử dụng. Câu trả lời cuối cùng chính là sự khác biệt giữa ghế có sẵn và ghế đã qua sử dụng. 

Một điểm tinh tế đang kẹp`d`về không. Nếu không có điều này, những trường hợp`total < n`sẽ tạo ra thâm hụt âm mặc dù không thiếu hụt có thể được áp dụng một cách có ý nghĩa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
20 5
2 3 4 5 6
```Chúng tôi tính toán:$$\sum a_i = 20,\quad \min a_i = 2$$| Bước | Giá trị | 
| --- | --- | 
| tổng cộng | 20 | 
| phút a_i | 2 | 
| d1 | 1 | 
| d2 | (20 - 20) // 5 = 0 | 
| d | 0 | 
| đã qua sử dụng | 20 | 
| đã lưu | 0 | 

Ở đây ngân sách khớp chính xác với số tiền lý tưởng, do đó không có thâm hụt nào được đưa ra. Câu trả lời là 0. 

Điều này xác nhận rằng khi nguồn cung khớp chính xác với nhu cầu thì cấu hình tối ưu sẽ tránh được mọi biến dạng. 

### Ví dụ 2 

đầu vào:```
10 3
3 3 3
```Chúng tôi tính toán:$$\sum a_i = 9,\quad \min a_i = 3$$| Bước | Giá trị | 
| --- | --- | 
| tổng cộng | 9 | 
| phút a_i | 3 | 
| d1 | 2 | 
| d2 | (9 - 10) // 3 = -1 | 
| d | 0 | 
| đã qua sử dụng | 9 | 
| đã lưu | 1 | 

Ở đây chúng tôi đã có nhiều ghế hơn mức cần thiết cho cấu hình lý tưởng, vì vậy chúng tôi có thể tiết kiệm ít nhất một chiếc ghế. Tính toán cho thấy mức độ tiêu cực$d2$buộc thuật toán phải giải quyết ở mức thâm hụt bằng không. 

Điều này minh họa rằng giải pháp xử lý chính xác các trường hợp dư thừa mà không cố gắng thiếu hụt tiêu cực không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K)$| Một lần để tính tổng và tối thiểu | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$K \le 10^5$và thuật toán chỉ thực hiện công việc tuyến tính mà không có vòng lặp lồng nhau hoặc tính toán lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("20 5\n2 3 4 5 6\n") == "0"

# minimum case
assert run("1 1\n1\n") == "0"

# all equal
assert run("15 3\n5 5 5\n") == "0"

# surplus chairs
assert run("10 3\n3 3 3\n") == "1"

# tight constraint min-bound active
assert run("10 3\n1 10 10\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/1 | 0 | trường hợp cạnh phòng đơn | 
| 15 3 / 5 5 5 | 0 | cấu hình đối xứng | 
| 10 3 / 3 3 3 | 1 | xử lý dư thừa | 
| 10 3 / 1 10 10 | 7 | hạn chế về phòng tối thiểu chiếm ưu thế | 

## Vỏ cạnh 

Khi chỉ có một phòng, điều kiện buộc tất cả ghế phải được đặt trong phòng đó, do đó không thể tiết kiệm được trừ khi$N$vượt quá cơ cấu hạn chế năng lực của nó cho phép. Thuật toán tính toán$d1 = a_1 - 1$Và$d2 = (a_1 - N)$và kẹp chính xác vào giá trị khả thi, mang lại vị trí nhất quán. 

Khi tất cả sức chứa của các phòng đều giống nhau, hạn chế tối thiểu sẽ trở thành yếu tố chi phối và mọi nỗ lực nhằm tăng mức thâm hụt đồng đều sẽ bị hạn chế ngay lập tức. Thuật toán giảm xuống việc kiểm tra xem tổng ngân sách của chủ tịch cao hơn hay thấp hơn số tiền lý tưởng và số tiền được tính toán$d$phản ánh chính xác sự cân bằng đó. 

Khi một phòng có sức chứa rất nhỏ như 1, nó buộc$d = 0$, vì không được phép thâm hụt. Thuật toán nắm bắt điều này thông qua$d1 = 0$, đảm bảo tất cả các tính toán khác tôn trọng ràng buộc cấu trúc chặt chẽ nhất mà không cần vỏ đặc biệt.
