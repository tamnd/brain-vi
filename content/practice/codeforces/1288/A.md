---
title: "CF 1288A - Thời hạn"
description: "Đối với mỗi trường hợp thử nghiệm, chúng tôi có thời hạn là n ngày và một chương trình thường cần d ngày để hoàn thành. Trước khi chạy chương trình, chúng tôi có thể dành x ngày để tối ưu hóa nó."
date: "2026-06-11T19:00:43+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1288
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 80 (Rated for Div. 2)"
rating: 1100
weight: 1288
solve_time_s: 117
verified: true
draft: false
---

[CF 1288A - Hạn chót](https://codeforces.com/problemset/problem/1288/A) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, toán học, tìm kiếm bậc ba 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi có thời hạn`n`ngày và một chương trình thường cần`d`ngày để hoàn thành. 

Trước khi chạy chương trình, chúng ta có thể dành`x`ngày tối ưu hóa nó. Sau khi chi tiêu những`x`ngày, thời gian chạy sẽ là:$$\left\lceil \frac{d}{x+1} \right\rceil$$Vì việc tối ưu hóa và thực thi không thể xảy ra cùng lúc nên tổng thời gian sử dụng là:$$x + \left\lceil \frac{d}{x+1} \right\rceil$$Chúng ta phải xác định xem có tồn tại một số nguyên không âm hay không`x`sao cho tổng thời gian là nhiều nhất`n`. 

Những hạn chế là thách thức chính. Cả hai`n`Và`d`có thể lớn như`10^9`, do đó lặp qua mọi giá trị có thể có của`x`là không thể. Ngay cả một trường hợp thử nghiệm cũng có thể yêu cầu kiểm tra hàng tỷ ứng viên. Với tối đa 50 trường hợp thử nghiệm, bất kỳ giải pháp nào gần giống`O(d)`là hoàn toàn không khả thi. 

biểu thức$$f(x)=x+\left\lceil \frac{d}{x+1} \right\rceil$$has a useful shape. BẰNG`x`tăng lên, số hạng thứ nhất tăng lên trong khi số hạng thứ hai co lại. Mức tối thiểu xảy ra gần điểm mà các hiệu ứng này cân bằng lẫn nhau. Khai thác cấu trúc đó là điều làm cho vấn đề trở nên dễ dàng. 

Một số trường hợp đặc biệt có thể gây ra sai sót. 

Coi như:```
1
1 1
```Câu trả lời đúng là`YES`bởi vì lựa chọn`x = 0`cung cấp tổng thời gian`1`. Một giải pháp chỉ kiểm tra các giá trị dương của`x`sẽ thất bại. 

Coi như:```
1
5 11
```Câu trả lời là`NO`. Mặc dù việc tối ưu hóa có ích nhưng không có giá trị nào`x`giảm tổng thời gian xuống`5`hoặc ít hơn. Chiến lược tham lam như "tiếp tục tối ưu hóa cho đến khi thời gian chạy trở nên nhỏ" không đảm bảo tổng số tối thiểu. 

Coi như:```
1
1000000000 1000000000
```Câu trả lời là`YES`. Các giá trị lớn yêu cầu tính toán số nguyên cẩn thận. Các ngôn ngữ có số nguyên có chiều rộng cố định có thể bị tràn nếu triển khai bất cẩn. Python xử lý việc này một cách tự động, nhưng thuật toán vẫn phải tránh các phép tính lớn không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi mức độ tối ưu hóa có thể`x`. Với mỗi ứng viên chúng tôi tính toán$$x+\left\lceil \frac{d}{x+1} \right\rceil$$và kiểm tra xem nó có nhiều nhất không`n`. 

Cách tiếp cận bạo lực này là đúng vì nó xem xét mọi lựa chọn hợp pháp. Vấn đề là thời gian chạy của nó. Trong trường hợp xấu nhất,`d`có thể`10^9`, vì vậy việc kiểm tra tất cả các giá trị từ`0`ĐẾN`d`sẽ yêu cầu khoảng một tỷ đánh giá cho một trường hợp thử nghiệm. Điều đó vượt xa giới hạn. 

Để tìm thứ gì đó nhanh hơn, hãy nhìn vào hàm$$f(x)=x+\left\lceil \frac{d}{x+1} \right\rceil.$$Bỏ qua trần nhà trong giây lát sẽ mang lại$$x+\frac{d}{x+1}.$$Đây là một biểu thức hình lồi cổ điển. Ban đầu tăng dần`x`giúp ích vì số hạng chia giảm nhanh chóng. Về sau ngày càng tăng`x`đau vì thuật ngữ tuyến tính chiếm ưu thế. 

Mức tối thiểu xảy ra gần$$x \approx \sqrt d - 1.$$Từ`d ≤ 10^9`, chúng tôi có$$\sqrt d \le 31623.$$Sự quan sát đó thay đổi mọi thứ. Giá trị tối ưu chỉ có thể ở gần căn bậc hai, do đó đủ để kiểm tra tất cả các giá trị lên đến khoảng`√d`. Việc này chỉ tốn khoảng ba mươi nghìn lượt kiểm tra cho mỗi trường hợp thử nghiệm. 

Một cách khác để xem nó là Codeforces dự định sử dụng một lực lượng toán học đơn giản. Nếu như`x`trở nên lớn hơn nhiều so với`√d`, số hạng tuyến tính đã quá đắt rồi, trong khi số hạng chia thay đổi rất chậm. Mức tối thiểu không thể ẩn xa khu vực căn bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d) | O(1) | Quá chậm | 
| Tối ưu | O(√d) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với trường hợp thử nghiệm, trước tiên hãy kiểm tra xem`d ≤ n`. 

Nếu chương trình đã kết thúc trước thời hạn thì không cần tối ưu hóa và câu trả lời là ngay lập tức`YES`. 
2. Tính toán`limit = ⌊√d⌋ + 2`. 

Cực tiểu của hàm mục tiêu xảy ra gần`√d`, vì vậy kiểm tra xa hơn điểm đó một chút là đủ. 
3. Với mọi số nguyên`x`từ`0`ĐẾN`limit`, tính toán$$x+\left\lceil \frac{d}{x+1} \right\rceil.$$Trần nhà có thể được tính bằng cách sử dụng số học số nguyên:$$\left\lceil \frac{d}{x+1} \right\rceil = \frac{d+x}{x+1}$$với phép chia số nguyên. 
4. Nếu bất kỳ ứng cử viên nào tạo ra giá trị không vượt quá`n`, đầu ra`YES`. 

Chúng tôi chỉ cần một lượng tối ưu hóa khả thi. 
5. Nếu tất cả thí sinh đều trượt, hãy xuất ra`NO`. 

### Tại sao nó hoạt động 

chức năng$$f(x)=x+\left\lceil \frac{d}{x+1} \right\rceil$$là tổng của số hạng tăng và số hạng giảm. Mức tối thiểu của nó đạt được gần điểm cân bằng của hai hiệu ứng đó, tức là vào khoảng`√d`. 

Đối với các giá trị lớn hơn đáng kể so với`√d`, phần tuyến tính`x`đã vượt quá vùng nơi mức tối thiểu có thể xảy ra, trong khi số hạng phân chia chỉ thay đổi một chút. Kiểm tra tất cả các giá trị từ`0`thông qua đại khái`√d`đảm bảo rằng chúng ta gặp phải giá trị tối thiểu hoặc không tệ hơn nó. Nếu có bất kỳ giải pháp khả thi nào tồn tại, một trong những giá trị được kiểm tra đó sẽ đáp ứng điều kiện thời hạn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, d = map(int, input().split())

    ok = False
    limit = math.isqrt(d) + 2

    for x in range(limit + 1):
        total = x + (d + x) // (x + 1)
        if total <= n:
            ok = True
            break

    print("YES" if ok else "NO")
```Vòng lặp kết thúc`x`là cốt lõi của giải pháp. Thay vì khám phá tất cả số lượng tối ưu hóa có thể, nó chỉ kiểm tra các giá trị xấp xỉ`√d`. 

biểu thức```
(d + x) // (x + 1)
```thực hiện việc chia trần$$\left\lceil \frac{d}{x+1} \right\rceil.$$Một lỗi phổ biến là sử dụng phép chia số nguyên thông thường:```
d // (x + 1)
```tính toán sàn và có thể chấp nhận không chính xác các lịch trình không thể thực hiện được. 

sử dụng`math.isqrt(d)`tránh các vấn đề về độ chính xác của dấu phẩy động. Từ`d`có thể đạt được`10^9`, cả hai cách tiếp cận đều có hiệu quả ở đây, nhưng căn bậc hai số nguyên sẽ rõ ràng và chính xác hơn. 

Thuật toán dừng ngay khi tìm được giá trị khả thi vì bài toán chỉ hỏi giá trị đó có tồn tại hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
d = 5
```| x | trần(5/(x+1)) | tổng cộng | 
| --- | --- | --- | 
| 0 | 5 | 5 | 
| 1 | 3 | 4 | 

Tại`x = 1`, tổng thời gian trở thành`4`, phù hợp với thời hạn. 

Thuật toán xuất ra ngay lập tức`YES`. Ví dụ này cho thấy rằng một lượng tối ưu hóa nhỏ có thể có ích ngay cả khi thời gian chạy ban đầu vượt quá thời hạn. 

### Ví dụ 2 

đầu vào:```
n = 5
d = 11
```| x | trần(11/(x+1)) | tổng cộng | 
| --- | --- | --- | 
| 0 | 11 | 11 | 
| 1 | 6 | 7 | 
| 2 | 4 | 6 | 
| 3 | 3 | 6 | 
| 4 | 3 | 7 | 
| 5 | 2 | 7 | 

Tổng số tối thiểu đạt được là`6`, vẫn lớn hơn`5`. 

Vì không có giá trị được kiểm tra nào thỏa mãn thời hạn nên câu trả lời là`NO`. 

Ví dụ này minh họa rằng việc tối ưu hóa không phải lúc nào cũng bù đắp được cho thời gian chạy lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√d) | Chúng tôi chỉ kiểm tra các giá trị từ`0`về khoảng`√d`| 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Từ`√10^9 ≈ 31623`, mỗi trường hợp kiểm thử chỉ thực hiện vài chục nghìn lần lặp. Với tối đa 50 trường hợp thử nghiệm, điều này dễ dàng phù hợp với giới hạn thời gian và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        n, d = map(int, input().split())

        ok = False
        limit = math.isqrt(d) + 2

        for x in range(limit + 1):
            total = x + (d + x) // (x + 1)
            if total <= n:
                ok = True
                break

        ans.append("YES" if ok else "NO")

    return "\n".join(ans) + "\n"

# provided samples
assert run("3\n1 1\n4 5\n5 11\n") == "YES\nYES\nNO\n", "sample 1"

# minimum values
assert run("1\n1 1\n") == "YES\n", "minimum case"

# exact boundary where no optimization needed
assert run("1\n10 10\n") == "YES\n", "d equals n"

# impossible case
assert run("1\n1 100\n") == "NO\n", "far beyond deadline"

# maximum values
assert run("1\n1000000000 1000000000\n") == "YES\n", "maximum constraints"

# near optimum around sqrt(d)
assert run("1\n6 11\n") == "YES\n", "minimum total equals 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`YES`| Đầu vào hợp lệ nhỏ nhất | 
|`10 10`|`YES`| Không cần tối ưu hóa | 
|`1 100`|`NO`| Lịch trình rõ ràng là không thể | 
|`1000000000 1000000000`|`YES`| Giá trị lớn nhất | 
|`6 11`|`YES`| Ranh giới nơi tối ưu đáp ứng chính xác thời hạn | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
```Thuật toán kiểm tra`x = 0`.$$0+\left\lceil \frac{1}{1}\right\rceil = 1$$Từ`1 ≤ 1`, nó xuất ra`YES`. Đây là lý do tại sao việc tìm kiếm phải bao gồm`x = 0`. 

Coi như:```
1
10 10
```Không có bất kỳ tối ưu hóa nào:$$0+\left\lceil \frac{10}{1}\right\rceil = 10$$Điều kiện đã được giữ. Thuật toán ngay lập tức tìm thấy giá trị khả thi và trả về`YES`. 

Coi như:```
1
5 11
```Tổng số được kiểm tra là:$$11,7,6,6,7,\ldots$$Tối thiểu là`6`, vẫn lớn hơn`5`. Sau khi kiểm tra tất cả các giá trị liên quan đến khoảng`√11`, thuật toán kết luận chính xác rằng không tồn tại lịch trình khả thi và in ra`NO`. 

Coi như:```
1
1000000000 1000000000
```Điều tối ưu xảy ra gần`√10^9 ≈ 31623`. 

Ví dụ:$$31622+\left\lceil \frac{10^9}{31623}\right\rceil \approx 63245$$cái nào thấp hơn nhiều`10^9`. Thuật toán nhanh chóng tìm thấy giá trị đó và đưa ra`YES`. Điều này xác nhận rằng việc triển khai xử lý các đầu vào lớn nhất một cách hiệu quả.
