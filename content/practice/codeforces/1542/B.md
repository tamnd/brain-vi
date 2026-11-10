---
title: "CF 1542B - Cộng và nhân"
description: "Chúng ta bắt đầu từ số $1$. Từ bất kỳ số nào đã có trong tập hợp, chúng ta có thể thực hiện một trong hai thao tác: nhân với $a$ hoặc cộng $b$. Sau khi áp dụng các thao tác này nhiều lần theo bất kỳ thứ tự nào, chúng ta thu được một tập vô hạn các giá trị có thể truy cập được."
date: "2026-06-10T14:17:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1542
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 729 (Div. 2)"
rating: 1500
weight: 1542
solve_time_s: 980
verified: true
draft: false
---

[CF 1542B - Cộng và Nhân](https://codeforces.com/problemset/problem/1542/B) 

**Đánh giá:** 1500 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 16 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu từ số$1$. Từ bất kỳ số nào đã có trong tập hợp, chúng ta có thể thực hiện một trong hai thao tác: nhân với$a$, hoặc thêm$b$. Sau khi áp dụng các thao tác này nhiều lần theo bất kỳ thứ tự nào, chúng ta thu được một tập vô hạn các giá trị có thể truy cập được. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp$n$,$a$, Và$b$. Nhiệm vụ là xác định xem liệu$n$có thể được tạo ra bắt đầu từ$1$. 

Những ràng buộc là manh mối đầu tiên cho thấy vũ lực không thể phát huy tác dụng. Có tới$10^5$trường hợp thử nghiệm và mỗi tham số có thể lớn bằng$10^9$. Bất kỳ cách tiếp cận nào khám phá trực tiếp các trạng thái có thể tiếp cận sẽ ngay lập tức bùng nổ, bởi vì tập hợp là vô hạn và thậm chí giới hạn chúng ta ở các giá trị lên tới$n$có thể yêu cầu kiểm tra hàng tỷ con số. 

Giải pháp phải xử lý từng trường hợp thử nghiệm trong thời gian xấp xỉ logarit. Từ$a \le 10^9$, nhân lặp lại với$a$vượt quá$10^9$sau nhiều nhất là khoảng 30 bước khi$a \ge 2$. Điều đó cho thấy chúng ta nên tập trung vào sức mạnh của$a$, không phải trên toàn bộ tập hợp được tạo. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như$a=1$. Phép nhân không làm gì cả nên phép toán hữu ích duy nhất là cộng liên tục$b$. Ví dụ:```
n=13, a=1, b=4
```Những con số có thể tiếp cận được là$1,5,9,13,\dots$, vì vậy câu trả lời là Có. Một giải pháp tạo ra sức mạnh của một cách mù quáng$a$sẽ lặp lại mãi mãi vì$1,1,1,\dots$không bao giờ thay đổi. 

Một trường hợp phức tạp khác là khi$n$bản thân nó là sức mạnh của$a$.```
n=81, a=3, b=5
```Chúng ta chỉ cần nhân bốn lần. Không cần bổ sung. Câu trả lời là Có vì$$81 = 3^4 + 0\cdot 5.$$Một giải pháp nhất quyết sử dụng ít nhất một phép cộng sẽ thất bại. 

Trường hợp thứ ba là khi phép cộng phải xảy ra trước một số phép nhân.```
n=24, a=3, b=5
```Một cách xây dựng hợp lệ là$$1 \to 6 \to 8 \to 24.$$Trình tự không chỉ là "tất cả các phép nhân, rồi tất cả các phép cộng". Mô phỏng trực tiếp thứ tự hoạt động có vẻ phức tạp, nhưng cấu trúc toán học hóa ra lại khiến thứ tự không liên quan đến thử nghiệm cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thực hiện tìm kiếm biểu đồ bắt đầu từ$1$. Mỗi tiểu bang$x$có các cạnh để$x\cdot a$Và$x+b$. Điều này khám phá chính xác tất cả các số có thể truy cập. 

Vấn đề là kích thước của không gian trạng thái. Ngay cả khi chúng ta dừng lại ở những giá trị lớn hơn$n$, có thể vẫn còn tới$n$tiểu bang. Từ$n$có thể$10^9$, điều này hoàn toàn không thể thực hiện được. 

Để tìm thứ gì đó tốt hơn, chúng ta cần hiểu những con số có thể truy cập trông như thế nào. 

Giả sử chúng ta quyết định có bao nhiêu phép nhân với$a$chúng tôi sẽ biểu diễn. Hãy để số đó được tính$k$. Các phép nhân đóng góp một yếu tố$a^k$. Mỗi phép cộng đóng góp một số bội số của$b$. 

Một quan sát quan trọng là modulo$b$, thêm$b$không có gì thay đổi Điều duy nhất ảnh hưởng đến modulo dư$b$là phép nhân với$a$. 

Nếu một số có thể truy cập được thì sau khi sửa số nhân cuối cùng$k$, nó phải có dạng$$a^k + m b$$đối với một số số nguyên không âm$m$. 

Sắp xếp lại mang lại$$n-a^k = mb.$$Như vậy$n$có thể truy cập được khi và chỉ nếu tồn tại một sức mạnh nào đó$a^k$như vậy$$n \ge a^k$$Và$$(n-a^k)\bmod b = 0.$$Bây giờ không gian tìm kiếm trở nên nhỏ bé. Chúng ta chỉ cần kiểm tra sức mạnh$$1,a,a^2,a^3,\dots$$lên đến$n$. Có nhiều nhất khoảng 30 trong số đó khi$a>1$. 

Trường hợp đặc biệt$a=1$phải được xử lý riêng. Thế thì mọi sức mạnh vẫn còn$1$, do đó điều kiện trở thành$$(n-1)\bmod b = 0.$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(\log_a n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$,$a$, Và$b$. 
2. Nếu$a=1$, phép nhân không bao giờ thay đổi giá trị hiện tại. 

Những con số duy nhất có thể tiếp cận được là$$1,\ 1+b,\ 1+2b,\dots$$Kiểm tra xem$$(n-1)\bmod b = 0.$$Nếu có thì in`"Yes"`, nếu không thì in`"No"`. 
3. Nếu$a>1$, bắt đầu với công suất hiện tại bằng$1$. 
4. Trong khi công suất hiện tại tối đa$n$, kiểm tra xem$$(n-\text{power})\bmod b = 0.$$Nếu điều này giữ vững thì$$n=\text{power}+mb$$đối với một số số nguyên không âm$m$, Vì thế$n$có thể truy cập được. In`"Yes"`và ngừng xử lý trường hợp thử nghiệm này. 
5. Nếu không thì nhân công suất hiện tại với$a$và tiếp tục. 
6. Nếu mọi sức mạnh của$a$lên đến$n$đã được kiểm tra và không có gì hoạt động, in`"No"`. 

### Tại sao nó hoạt động 

Mỗi số có thể truy cập có thể được xem như là lũy thừa của$a$cộng với một số bổ sung về kích thước$b$. Sửa số nhân cuối cùng thành$k$. Phần nhân góp phần$a^k$, trong khi các phép cộng đóng góp một lượng chia hết cho$b$. Do đó mọi số có thể truy cập đều thỏa mãn$$n=a^k+mb.$$Ngược lại, nếu vì một số$k$chúng tôi có$$n=a^k+mb$$với$m\ge 0$, sau đó bắt đầu từ$1$, biểu diễn$k$phép nhân cho$a^k$, và biểu diễn$m$bổ sung mang lại$n$. Do đó, thuật toán kiểm tra chính xác điều kiện cần và đủ cho khả năng tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, a, b = map(int, input().split())

    if a == 1:
        print("Yes" if (n - 1) % b == 0 else "No")
        continue

    cur = 1
    ok = False

    while cur <= n:
        if (n - cur) % b == 0:
            ok = True
            break
        cur *= a

    print("Yes" if ok else "No")
```Nhánh đầu tiên xử lý$a=1$. Không có nó, vòng lặp quyền hạn sẽ không bao giờ chấm dứt vì nhân với$1$giữ nguyên giá trị hiện tại. 

Biến`cur`lưu trữ sức mạnh liên tiếp của$a$:$$1,\ a,\ a^2,\dots$$Đối với mỗi lũy thừa, chúng tôi kiểm tra xem sự khác biệt còn lại có$n$chia hết cho$b$. Việc kiểm tra chia hết thành công có nghĩa là số tiền còn lại có thể được cung cấp hoàn toàn thông qua các phép tính cộng. 

Số nguyên Python tự động mở rộng đến kích thước tùy ý, do đó không có vấn đề tràn. Vòng lặp dừng lại một lần`cur > n`, bởi vì quyền lực lớn hơn không thể tham gia vào một đại diện$$n=a^k+mb$$với$m\ge 0$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
24 3 5
```| Nguồn điện hiện tại |$n-\text{power}$| Chia hết cho 5? | 
| --- | --- | --- | 
| 1 | 23 | Không | 
| 3 | 21 | Không | 
| 9 | 15 | Có | 

sức mạnh$9$hoạt động vì$$24-9=15=3\cdot 5.$$Như vậy$$24=3^2+3\cdot 5,$$vì vậy câu trả lời là Có. 

Ví dụ này cho thấy chúng ta không cần phải xây dựng lại chuỗi hoạt động thực tế. Điều kiện chia hết đã nắm bắt được khả năng tiếp cận. 

### Ví dụ 2 

đầu vào:```
10 3 6
```| Nguồn điện hiện tại |$n-\text{power}$| Chia hết cho 6? | 
| --- | --- | --- | 
| 1 | 9 | Không | 
| 3 | 7 | Không | 
| 9 | 1 | Không | 

Sức mạnh tiếp theo là$27$, vượt quá$10$, thế là việc tìm kiếm kết thúc. 

Không có sức mạnh của$3$để lại một sự khác biệt chia hết cho$6$. Câu trả lời là Không. 

Ví dụ này chứng minh rằng chỉ kiểm tra quyền hạn của$a$là đủ. Nếu không cái nào trong số chúng hoạt động thì không có chuỗi thao tác nào có thể đạt tới$n$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log_a n)$| Chúng tôi kiểm tra từng sức mạnh của$a$lên đến$n$| 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Vì$a \ge 2$, số lũy thừa không vượt quá khoảng 30 khi$n \le 10^9$. Ngay cả với$10^5$các trường hợp thử nghiệm, tổng công việc chỉ là vài triệu phép tính số học đơn giản, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    data = io.StringIO(inp)
    out = []

    t = int(data.readline())

    for _ in range(t):
        n, a, b = map(int, data.readline().split())

        if a == 1:
            out.append("Yes" if (n - 1) % b == 0 else "No")
            continue

        cur = 1
        ok = False

        while cur <= n:
            if (n - cur) % b == 0:
                ok = True
                break
            cur *= a

        out.append("Yes" if ok else "No")

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""5
24 3 5
10 3 6
2345 1 4
19260817 394 485
19260817 233 264
"""
) == """Yes
No
Yes
No
Yes
"""

# minimum values
assert run(
"""1
1 1 1
"""
) == """Yes
"""

# a = 1 reachable
assert run(
"""1
13 1 4
"""
) == """Yes
"""

# a = 1 not reachable
assert run(
"""1
14 1 4
"""
) == """No
"""

# exact power of a
assert run(
"""1
81 3 5
"""
) == """Yes
"""

# large boundary-style case
assert run(
"""1
1000000000 1000000000 1
"""
) == """Yes
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`| Có | Giá trị nhỏ nhất có thể | 
|`13 1 4`| Có | Trường hợp đặc biệt$a=1$, có thể truy cập | 
|`14 1 4`| Không | Trường hợp đặc biệt$a=1$, không thể truy cập | 
|`81 3 5`| Có | Con số là sức mạnh thuần túy của$a$| 
|`1000000000 1000000000 1`| Có | Giá trị lớn và$b=1$| 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
13 1 4
```Từ$a=1$, thuật toán ngay lập tức sử dụng nhánh đặc biệt. Nó tính toán$$(13-1)\bmod 4 = 12\bmod 4 = 0.$$Câu trả lời là Có. Nếu không có trường hợp đặc biệt, vòng phát điện sẽ không bao giờ tiến triển. 

Hãy xem xét đầu vào:```
1
81 3 5
```Các quyền lực được kiểm tra là$1,3,9,27,81$. Khi dòng điện đạt tới$81$,$$81-81=0,$$cái đó có thể chia được cho$5$. Thuật toán trả về Có. Điều này xác nhận rằng không được phép bổ sung. 

Hãy xem xét đầu vào:```
1
10 3 6
```Các quyền lực được kiểm tra là$1,3,9$. Không để lại sự khác biệt chia hết cho$6$. Sức mạnh tiếp theo vượt quá$n$, vì vậy câu trả lời là Không. Điều này xác minh điều kiện dừng khi không tồn tại biểu diễn hợp lệ.
