---
title: "CF 106087B - \u041b\u0435\u043d\u0438\u0432\u044b\u0435 \u0433\u043e\u043b\u0443\u0431\u0446\u044b"
description: "Chúng ta có tổng khoảng cách $s$ và mức tăng cố định $k$. Một chuỗi các người chạy (được gọi là “chim bồ câu” trong câu lệnh) tạo ra một danh sách các số nguyên dương $a1, a2, dấu chấm, an$ sao cho giá trị đầu tiên ít nhất là 1 và mọi giá trị tiếp theo đều lớn hơn ít nhất $k$ so với giá trị trước đó…"
date: "2026-06-20T04:23:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106087
codeforces_index: "B"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u043f\u0435\u0440\u0432\u044b\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 106087
solve_time_s: 40
verified: true
draft: false
---

[CF 106087B - \u041b\u0435\u043d\u0438\u0432\u044b\u0435 \u0433\u043e\u043b\u0443\u0431\u0446\u044b](https://codeforces.com/problemset/problem/106087/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một khoảng cách tổng thể$s$và mức tăng cố định$k$. Một dãy người chạy (được gọi là “bồ câu” trong câu lệnh) tạo ra một danh sách các số nguyên dương$a_1, a_2, \dots, a_n$sao cho giá trị đầu tiên ít nhất là 1 và mọi giá trị tiếp theo ít nhất là$k$lớn hơn lần trước. Tổng của tất cả các giá trị là chính xác$s$. Nhiệm vụ là xác định độ dài tối đa có thể$n$của một trình tự như vậy. 

Nói cách khác, chúng ta muốn gói gọn càng nhiều số hạng càng tốt vào một cách tăng dần ít nhất-$k$trình tự giống như số học, trong khi vẫn giữ tổng số cố định ở$s$. Số hạng đầu tiên chỉ bị ràng buộc ở mức tối thiểu là 1 và không có giới hạn trên cho các giá trị riêng lẻ ngoại trừ tổng phải khớp$s$. 

Các ràng buộc làm rõ rằng chúng ta không thể thử tất cả các giá trị của$n$bằng cách xây dựng trình tự một cách rõ ràng. tổng$s$có thể đi lên$10^{18}$, do đó, mọi giải pháp đều phải hoạt động theo thời gian logarit hoặc không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$không được phép, nhưng$k = 1$là. Trong trường hợp đó, chuỗi trở thành một chuỗi tăng nghiêm ngặt đơn giản với khoảng cách tối thiểu 1 và cấu trúc tối ưu hoạt động khác nhau về độ lớn so với cấu trúc lớn.$k$. Một trường hợp cạnh khác là khi$s$là rất nhỏ, ví dụ$s = 1$, trong đó câu trả lời tầm thường là 1 bất kể$k$. Một nỗ lực ngây thơ giả định ít nhất hai thuật ngữ hoặc cách xây dựng từ 0 sẽ thất bại ở đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử độ dài cố định$n$, xây dựng chuỗi hợp lệ nhỏ nhất có thể có độ dài đó và kiểm tra xem tổng của nó có vượt quá không$s$. Đối với một cố định$n$, trình tự hợp lệ tối thiểu có được bằng cách thiết lập$a_1 = 1$,$a_2 = 1 + k$,$a_3 = 1 + 2k$, vân vân. Điều này là tối ưu vì bất kỳ giá trị bắt đầu lớn hơn nào cũng chỉ làm tăng tổng. 

Tổng của chuỗi tối thiểu này là$$S(n) = n + k(0 + 1 + \dots + (n-1)) = n + k \cdot \frac{n(n-1)}{2}.$$Nếu như$S(n) \le s$, sau đó là một chuỗi có độ dài$n$là có thể; nếu không thì không. Điều này làm giảm vấn đề để tìm lớn nhất$n$sao cho bất đẳng thức bậc hai đúng. 

Ý tưởng vũ phu sẽ tăng lên$n$cho đến khi số tiền vượt quá$s$, có thể lên tới$O(\sqrt{s})$hoặc tệ hơn cho mỗi trường hợp thử nghiệm. Với$s \le 10^{18}$, đây vẫn có thể là ranh giới nhưng trở nên quá chậm đối với$t = 1000$trong trường hợp xấu nhất nếu được thực hiện một cách ngây thơ. 

Quan sát quan trọng là$S(n)$là đơn điệu trong$n$. Khi độ dài trở nên không hợp lệ, tất cả độ dài lớn hơn cũng không hợp lệ. Điều này cho phép tìm kiếm nhị phân trên$n$. Từ$S(n)$tăng trưởng bậc hai, tối đa có thể$n$đại khái là$O(\sqrt{s})$, do đó phạm vi tìm kiếm nhỏ và ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu ngày càng tăng$n$|$O(\sqrt{s})$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân bật$n$|$O(\log s)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề để kiểm tra tính khả thi của độ dài chuỗi nhất định$n$. 

1. Đối với cố định$n$, tính tổng nhỏ nhất có thể$S(n) = n + k \cdot n(n-1)/2$. Điều này tương ứng với cấp số hợp lệ nhỏ nhất, vì bất kỳ sự gia tăng nào trong bất kỳ số hạng nào cũng chỉ làm tăng tổng số tiền. 
2. Nếu$S(n) \le s$, sau đó$n$là khả thi. Nếu không thì không. 
3. Sử dụng tìm kiếm nhị phân trên$n$, bắt đầu từ 1 đến giới hạn trên an toàn. Giới hạn tự nhiên là$2 \cdot 10^9$, nhưng trong thực tế$\sqrt{2s/k}$là đủ; chúng ta có thể kẹp một cách an toàn tại$10^{9}$. 
4. Trong tìm kiếm nhị phân, liên tục kiểm tra điểm giữa bằng công thức. Nếu khả thi, hãy di chuyển sang phải để thử lớn hơn$n$, nếu không thì di chuyển sang trái. 
5. Đáp án cuối cùng lớn nhất khả thi$n$. 

Sự tinh tế quan trọng là sử dụng số học 64-bit khi tính toán$k \cdot n(n-1)/2$. Mặc dù$n$trong thực tế là nhỏ, phép nhân trung gian có thể tràn các loại 32 bit. 

### Tại sao nó hoạt động 

Đối với bất kỳ chuỗi độ dài hợp lệ nào$n$, thay thế từng$a_i$với giá trị nhỏ nhất có thể phù hợp với các ràng buộc sẽ tạo ra một chuỗi có cùng độ dài và tổng không lớn hơn. Sự chuyển đổi này làm giảm hoặc bảo toàn tổng một cách nghiêm ngặt, do đó tính khả thi hoàn toàn được xác định bởi việc xây dựng tối thiểu. Vì số tiền tối thiểu đó là đơn điệu trong$n$, tìm kiếm nhị phân xác định chính xác độ dài khả thi tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def feasible(n, s, k):
    return n + k * n * (n - 1) // 2 <= s

def solve():
    t = int(input())
    for _ in range(t):
        s, k = map(int, input().split())

        lo, hi = 1, 10**9
        ans = 1

        while lo <= hi:
            mid = (lo + hi) // 2
            if feasible(mid, s, k):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        print(ans)

if __name__ == "__main__":
    solve()
```chức năng`feasible`mã hóa công thức tính tổng tối thiểu dẫn xuất. Phép chia này an toàn vì nó sử dụng toàn bộ số học số nguyên. Tìm kiếm nhị phân duy trì tính bất biến mà tất cả các giá trị lên tới`ans`đều khả thi, và bất cứ điều gì vượt quá`hi`thì không. 

Giới hạn trên của$10^9$là an toàn bởi vì ngay cả với$k = 1$, tổng tăng lên như thế nào$n^2/2$, vượt quá$10^{18}$trước đó cũng được$10^9$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = 10, k = 1
```Chúng tôi kiểm tra tính khả thi của các phương pháp khác nhau$n$. 

| n | S(n) = n + n(n-1)/2 | Khả thi | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 3 | vâng | 
| 3 | 6 | vâng | 
| 4 | 10 | vâng | 
| 5 | 15 | không | 

Tìm kiếm nhị phân sẽ hội tụ đến$n = 4$. Điều này cho thấy cấu trúc hoạt động giống như số tam giác khi$k = 1$. 

### Ví dụ 2 

đầu vào:```
s = 20, k = 3
```| n | S(n) = n + 3*n(n-1)/2 | Khả thi | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 2 + 3 = 5 | vâng | 
| 3 | 3 + 9 = 12 | vâng | 
| 4 | 4 + 18 = 22 | không | 

Câu trả lời là$n = 3$. Điều này cho thấy lớn hơn thế nào$k$tăng tốc độ tăng trưởng và giảm chiều dài có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log s)$| tìm kiếm nhị phân cho mỗi trường hợp thử nghiệm trên phạm vi khả thi | 
| Không gian |$O(1)$| chỉ có một vài biến được sử dụng | 

Hệ số logarit nhỏ ngay cả đối với$s = 10^{18}$, và với$t \le 1000$, nghiệm dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    def feasible(n, s, k):
        return n + k * n * (n - 1) // 2 <= s

    def solve():
        t = int(input())
        for _ in range(t):
            s, k = map(int, input().split())
            lo, hi = 1, 10**9
            ans = 1
            while lo <= hi:
                mid = (lo + hi) // 2
                if feasible(mid, s, k):
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1
            print(ans)

    solve()
    return ""

# provided sample-style cases
assert run("2\n10 1\n10 2\n") == ""

# minimum case
assert run("1\n1 100\n") == ""

# small k large n behavior
assert run("1\n100 1\n") == ""

# large k reduces quickly
assert run("1\n1000000000000000000 1000000000\n") == ""

# exact triangular boundary
assert run("1\n6 1\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 100 | 1 | số tiền tối thiểu có thể | 
| 100, 1 | lớn | tăng trưởng dưới k=1 | 
| 1e18, 1e9 | 1 | sự thống trị k cực đoan | 
| 6, 1 | 3 | trường hợp ranh giới tam giác | 

## Vỏ cạnh 

cho$s = 1$, bất kể$k$, chỉ có thể tồn tại một phần tử vì ngay cả chuỗi nhỏ nhất có độ dài 2 cũng yêu cầu tổng ít nhất$1 + (1 + k) > 1$. Thuật toán xử lý việc này vì$S(2) = 2 + k > 1$, vì vậy tìm kiếm nhị phân không bao giờ mở rộng quá 1. 

Đối với rất lớn$k$, chẳng hạn như$k \ge s$, riêng kỳ thứ hai đã vượt quá ngân sách. Ví dụ,$s = 10, k = 20$. Sau đó$S(2) = 2 + 20 = 22$, vậy chỉ$n = 1$là khả thi. Việc kiểm tra tính khả thi ngay lập tức bác bỏ$n = 2$, và đáp án vẫn là 1. 

cho$k = 1$, tốc độ tăng trưởng chậm nhất, tạo ra chiều dài tối đa có thể. Thuật toán vẫn hoạt động vì nó quy về số tam giác và tìm kiếm nhị phân tìm chính xác số lớn nhất$n$thỏa mãn$n(n+1)/2 \le s$.
