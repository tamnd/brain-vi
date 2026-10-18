---
title: "CF 105591B - \u041f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u0442\u0440\u0435\u0443\u0433\u043e\u043b\u044c\u043d\u0438\u043a"
description: "Chúng ta được hỏi liệu có thể tạo thành một tam giác có độ dài các cạnh là ba số tự nhiên liên tiếp và có chu vi bằng một giá trị cho trước $p$ hay không. Tam giác hợp lệ trong cài đặt này được xác định đầy đủ bởi số nguyên bắt đầu $a$."
date: "2026-06-22T17:58:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105591
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, 7-8 \u043a\u043b\u0430\u0441\u0441\u044b, \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2024"
rating: 0
weight: 105591
solve_time_s: 43
verified: true
draft: false
---

[CF 105591B - \u041f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u0442\u0440\u0435\u0443\u0433\u043e\u043b\u044c\u043d\u0438\u043a](https://codeforces.com/problemset/problem/105591/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được hỏi liệu có thể tạo thành một tam giác có độ dài các cạnh là ba số tự nhiên liên tiếp và có chu vi bằng một giá trị cho trước không$p$. 

Một tam giác hợp lệ trong cài đặt này được xác định đầy đủ bởi một số nguyên bắt đầu$a$. Khi đó các bên phải$a$,$a+1$, Và$a+2$. Ràng buộc chu vi cố định tổng của chúng, vì vậy chúng tôi thực sự đang kiểm tra xem liệu có tồn tại số nguyên hay không$a \ge 1$như vậy$$a + (a+1) + (a+2) = p.$$Đồng thời, ba giá trị này phải thỏa mãn bất đẳng thức tam giác, nghĩa là cạnh lớn nhất phải nhỏ hơn tổng của hai cạnh còn lại. 

Đầu vào là một số nguyên duy nhất$p$và đầu ra là nhị phân: in 1 nếu tồn tại ít nhất một bộ ba như vậy, nếu không thì in 0. 

Mặc dù bài toán trông có vẻ hình học nhưng nó lại hoàn toàn thuộc về cấu trúc số học. Ràng buộc chính không phải là hình học mà là liệu một hệ thống tuyến tính đơn giản có nghiệm số nguyên đảm bảo tính dương và bất đẳng thức tam giác hay không. 

Phạm vi$1 \le p \le 10^9$ngay lập tức loại trừ mọi nỗ lực thử tất cả các bộ ba số nguyên liên tiếp. Mặc dù$a$nhiều nhất là về$p/3$, lặp đi lặp lại đến$10^9$là không thể dưới giới hạn 1 giây. Lời giải phải giảm việc tìm kiếm về thời gian không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi chu vi rất nhỏ. Ví dụ,$p = 1$hoặc$p = 2$không thể tạo thành bất kỳ hình tam giác nào cả, nhưng thậm chí các giá trị lớn hơn một chút như$p = 4$hoặc$p = 5$cũng thất bại vì bộ ba liên tiếp nhỏ nhất là$1,2,3$, chu vi của nó đã là 6. Một dạng sai sót khác là quên bất đẳng thức tam giác, mặc dù đối với các số nguyên liên tiếp, nó tự động được thỏa mãn khi giá trị dương giữ nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi giá trị bắt đầu có thể$a$, tính bộ ba$(a, a+1, a+2)$, kiểm tra xem tổng có bằng không$p$, và chứng minh bất đẳng thức tam giác. Điều này đúng nhưng không hiệu quả. Chỉ riêng điều kiện tổng lực$3a + 3 = p$, vì vậy trong thực tế chỉ có một ứng cử viên$a$cho mỗi$p$, nhưng một cách tiếp cận ngây thơ vẫn sẽ lặp lại về mặt khái niệm$O(p)$các giá trị có thể có của$a$. Với$p$lên đến$10^9$, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là cấu trúc của các cạnh quyết định hoàn toàn chu vi. Một khi chúng ta giả sử các số nguyên liên tiếp thì không còn lựa chọn tổ hợp nào nữa. Sửa phương trình chu vi$a$duy nhất:$$a = \frac{p - 3}{3}.$$Vì vậy, toàn bộ vấn đề quy về việc kiểm tra xem giá trị này có phải là số nguyên hay không và nó có ít nhất là 1 hay không. Bất đẳng thức tam giác khi đó không thêm một ràng buộc mới vì với mọi giá trị dương$a$,$$a + (a+1) > a+2$$giữ tự động. 

Như vậy bài toán trở thành bài toán chia hết và kiểm tra giới hạn dưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$a$| O(p) | O(1) | Quá chậm | 
| Kiểm tra công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$p - 3$. Điều này tương ứng với việc loại bỏ sự đóng góp liên tục của các phần bù cố định trong cấu trúc liên tiếp. 
2. Kiểm tra xem$p - 3$chia hết cho 3. Nếu không, không có số nguyên$a$tồn tại nên không thể lập được tam giác hợp lệ. 
3. Nếu chia hết thì tính$a = (p - 3) / 3$. Đây là ứng cử viên duy nhất có thể cho bên nhỏ nhất. 
4. Xác minh rằng$a \ge 1$. Điều này đảm bảo các cạnh là số tự nhiên hợp lệ bắt đầu từ 1 hoặc lớn hơn. 
5. Nếu cả hai điều kiện đều giữ nguyên, xuất 1; nếu không thì xuất ra 0. 

Logic dựa trên thực tế là mọi tam giác hợp lệ phải tương ứng với chính xác một cấp số cộng có độ dài 3, do đó không cần phải tìm kiếm. 

### Tại sao nó hoạt động 

Mọi cấu hình hợp lệ phải có dạng$(a, a+1, a+2)$. Phương trình chu vi tạo ra một lực duy nhất$a$. Vì không có quyền tự do lựa chọn phe phái nên câu hỏi về sự tồn tại quy về việc liệu điều này có bị ép buộc hay không?$a$nằm trong miền hợp lệ của các số tự nhiên. Bất đẳng thức tam giác không thêm hạn chế nào vì các số nguyên liên tiếp luôn thỏa mãn nó một lần$a \ge 1$. Điều này làm cho điều kiện số học vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

p = int(input())

# Solve 3a + 3 = p
if (p - 3) % 3 != 0:
    print(0)
else:
    a = (p - 3) // 3
    if a >= 1:
        print(1)
    else:
        print(0)
```Mã trực tiếp mã hóa phương trình dẫn xuất. biểu hiện$3a + 3 = p$đến từ việc tính tổng ba số nguyên liên tiếp. Việc kiểm tra modulo đảm bảo tính toàn vẹn của$a$. Kiểm tra thứ hai yêu cầu chuỗi bắt đầu từ một số tự nhiên ít nhất là 1, vì bài toán yêu cầu độ dài các cạnh nguyên dương. 

Một cạm bẫy phổ biến là quên kiểm tra$a \ge 1$, sẽ chấp nhận không chính xác các trường hợp như$p = 3$, sản xuất$a = 0$, không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$p = 18$Chúng tôi tính toán$a = (18 - 3) / 3 = 5$. 

| Bước | p | p-3 | (p-3)%3 | một | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 18 | - | - | - | - | 
| Tính số dư | 18 | 15 | 0 | - | vâng | 
| Tính một | 18 | 15 | 0 | 5 | ứng cử viên | 
| Kiểm tra giới hạn | 18 | 15 | 0 | 5 | hợp lệ | 

Bộ ba là$5, 6, 7$, có tổng bằng 18 và thỏa mãn bất đẳng thức tam giác. 

Điều này xác nhận rằng khi cấp số cộng khớp chính xác thì nghiệm tồn tại. 

### Ví dụ 2:$p = 10$Chúng tôi tính toán$a = (10 - 3) / 3 = 7/3$, không phải là số nguyên. 

| Bước | p | p-3 | (p-3)%3 | một | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 10 | - | - | - | - | 
| Tính số dư | 10 | 7 | 1 | - | không | 

Vì tính chia hết không thành công nên không có bộ ba liên tiếp hợp lệ nào có thể tổng bằng 10. 

Điều này cho thấy hầu hết các giá trị của$p$ngay lập tức bị bác bỏ bởi điều kiện mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ kiểm tra số học và một modulo | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp thực hiện một số lượng phép toán số nguyên không đổi bất kể$p$, do đó, nó phù hợp một cách tầm thường trong các ràng buộc ngay cả đối với$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    p = int(input())
    if (p - 3) % 3 != 0:
        return "0"
    a = (p - 3) // 3
    if a >= 1:
        return "1"
    return "0"

# provided-style sample
assert run("18\n") == "1"

# too small
assert run("5\n") == "0"

# smallest valid triangle 1,2,3
assert run("6\n") == "1"

# invalid divisibility
assert run("10\n") == "0"

# larger valid case 10,11,12 sum 33
assert run("33\n") == "1"

# boundary just below valid
assert run("5\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 | 1 | tam giác hợp lệ tối thiểu (1,2,3) | 
| 5 | 0 | dưới chu vi tối thiểu | 
| 10 | 0 | cấu trúc không chia hết cho 3 | 
| 33 | 1 | tiến trình hợp lệ lớn hơn | 

## Vỏ cạnh 

Một chu vi nhỏ như$p = 6$sản xuất$a = 1$, cho tam giác tối thiểu$1,2,3$. Thuật toán tính toán$(6 - 3) = 3$, chia hết cho 3, thu được$a = 1$, vì vậy nó chấp nhận chính xác. 

Vì$p = 5$, phép tính cho$p - 3 = 2$, không chia hết cho 3. Thuật toán ngay lập tức loại bỏ nó, phù hợp với thực tế là không có ba số nguyên dương liên tiếp nào có thể tổng bằng 5. 

cho$p = 3$, chúng tôi nhận được$p - 3 = 0$, chia hết cho 3, thu được$a = 0$. Kiểm tra cuối cùng$a \ge 1$bác bỏ nó, tránh một tam giác không hợp lệ với các cạnh không dương.
