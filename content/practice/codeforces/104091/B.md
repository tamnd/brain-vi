---
title: "CF 104091B - \u0412\u0441\u0442\u0443\u043f\u0438\u0442\u0435\u043b\u044c\u043d\u043e\u0435 \u0438\u0441\u043f\u044b\u0442\u0430\u043d\u0438\u0435 \u0432 \u041a\u043e\u043b\u043b\u0435\u0433\u0438\u044e \u0412\u0438\u043d\u0442\u0435\u0440\u0445\u043e\u043b\u0434\u0430"
description: "Chúng ta được cho một hệ thống số vị trí có cơ số $b$. Mỗi số nguyên có một biểu diễn trong cơ số đó và chúng ta quan tâm đến số lượng các số 0 ở cuối trong biểu diễn đó."
date: "2026-07-02T02:28:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104091
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041f\u0435\u0442\u0440\u043e\u0437\u0430\u0432\u043e\u0434\u0441\u043a\u0435 \u0438 \u041a\u0430\u0440\u0435\u043b\u0438\u0438 2022-2023"
rating: 0
weight: 104091
solve_time_s: 48
verified: true
draft: false
---

[CF 104091B - \u0412\u0441\u0442\u0443\u043f\u0438\u0442\u0435\u043b\u044c\u043d\u043e\u0435 \u0438\u0441\u043f\u044b\u0442\u0430\u043d\u0438\u0435 \u0432 \u041a\u043e\u043b\u043b\u0435\u0433\u0438\u044e \u0412\u0438\u043d\u0442\u0435\u0440\u0445\u043e\u043b\u0434\u0430](https://codeforces.com/problemset/problem/104091/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống chữ số vị trí với cơ số$b$. Mỗi số nguyên có một biểu diễn trong cơ số đó và chúng ta quan tâm đến số lượng các số 0 ở cuối trong biểu diễn đó. 

Một số được coi là hợp lệ nếu khi viết dưới dạng cơ số$b$, chữ số cuối cùng của nó chứa ít nhất$d$số không. Nói cách khác, số đó phải chia hết cho$b^d$. Nhiệm vụ là đếm có bao nhiêu số nguyên trong dãy$[l, r]$thỏa mãn điều kiện chia hết này. 

Những ràng buộc ngay lập tức đẩy chúng ta ra khỏi bất kỳ sự liệt kê trực tiếp nào. Phạm vi có thể lên tới$10^{18}$, vì vậy việc lặp lại tất cả các số là không thể. Căn cứ$b$nhiều nhất là 100, vậy$b^d$vẫn có thể quản lý được vì$d \le 9$, nhưng phạm vi số quá lớn để kiểm tra một cách đơn giản. 

Một trường hợp thất bại thường xuất hiện khi chỉ suy nghĩ bằng trực giác thập phân. Ví dụ, trong cơ số 10, việc đếm các số kết thúc bằng ít nhất 2 số 0 có nghĩa là đếm bội số của 100. Nhưng trong cơ số 2 hoặc cơ số 7, khoảng cách giữa các số hợp lệ thay đổi đáng kể. Một vòng lặp đơn giản kiểm tra tính chia hết cho mỗi số trong$[l, r]$sẽ hết thời gian cho các phạm vi như$10^{18}$. 

Một cạm bẫy tinh vi khác là việc xử lý$d = 0$. Trong trường hợp đó, mọi số đều hợp lệ, vì việc có ít nhất 0 số 0 ở cuối luôn luôn đúng. Bất kỳ việc triển khai nào quên trường hợp đặc biệt này sẽ cố gắng tính toán số chia hết cho$b^0 = 1$, điều này tốt về mặt toán học, nhưng có thể gây ra sự phức tạp không cần thiết hoặc các lỗi trường hợp phức tạp trong logic nếu được xử lý không nhất quán. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp qua mọi số nguyên$x$từ$l$ĐẾN$r$, chuyển đổi nó thành cơ sở$b$, đếm các số 0 ở cuối và kiểm tra xem số đếm đó có ít nhất là$d$. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, chuyển đổi một số thành cơ sở$b$chi phí$O(\log_b x)$, và chúng tôi làm điều này cho đến$10^{18}$những con số. Ngay cả với số học nhanh, điều này vẫn vượt xa khả năng thực hiện. 

Quan sát quan trọng là các số 0 ở cuối trong cơ số$b$tương đương với phép chia cho$b^d$. Nếu một số kết thúc bằng ít nhất$d$số không trong cơ sở$b$, thì nó phải có dạng$k \cdot b^d$. Vì vậy, vấn đề giảm xuống việc đếm xem có bao nhiêu bội số của$b^d$nằm ở$[l, r]$. 

Điều này biến vấn đề thành một nhiệm vụ đếm tiền tố cổ điển. Ta tính xem có bao nhiêu bội số của$b^d$nhỏ hơn hoặc bằng$r$, trừ đi bao nhiêu nhỏ hơn$l$, và nhận được câu trả lời trong thời gian không đổi. 

Sự tinh tế duy nhất là đảm bảo tính chính xác cho các trường hợp Edge như$l = 1$và giá trị lớn của$b^d$, có thể vượt quá$10^{18}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)\log_b r)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề để đếm bội số của một giá trị cố định$p = b^d$bên trong một phạm vi. 

1. Tính toán$p = b^d$. Đây là số nhỏ nhất có cơ số$b$đại diện có ít nhất$d$số 0 ở cuối. 
2. Nếu$d = 0$, quay lại ngay$r - l + 1$, vì mọi số đều thỏa mãn điều kiện 
3. Tính xem có bao nhiêu bội số của$p$nhỏ hơn hoặc bằng$r$, đó là$\left\lfloor \frac{r}{p} \right\rfloor$. 
4. Tính xem có bao nhiêu bội số của$p$đúng là ít hơn$l$, đó là$\left\lfloor \frac{l-1}{p} \right\rfloor$. 
5. Trừ hai giá trị này để có được số đếm hợp lệ trong$[l, r]$. 

Lý do đằng sau bước 4 rất quan trọng: chúng tôi đang đếm các tiền tố lên tới$r$và loại bỏ mọi thứ trước đó$l$, vì vậy chúng ta phải loại trừ những số nhỏ hơn một cách nghiêm ngặt$l$, không phải những giá trị nhỏ hơn hoặc bằng$l$. 

### Tại sao nó hoạt động 

Mọi số nguyên chia hết cho$b^d$có ít nhất$d$số 0 ở cuối trong cơ sở$b$, vì nhân với$b$dịch chuyển các chữ số còn lại trong cơ sở$b$và thêm một số 0. Ngược lại, bất kỳ số nào có ít nhất$d$số 0 ở cuối phải chia hết cho$b^d$. Điều này thiết lập sự tương ứng một-một giữa các số hợp lệ và bội số của$b^d$, do đó việc đếm các số hợp lệ sẽ giảm chính xác xuống việc đếm bội số trong một khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

b = int(input().strip())
d = int(input().strip())
l = int(input().strip())
r = int(input().strip())

if d == 0:
    print(r - l + 1)
else:
    p = pow(b, d)
    def count(x):
        if x <= 0:
            return 0
        return x // p

    ans = count(r) - count(l - 1)
    print(ans)
```Giải pháp được xây dựng xung quanh việc rút gọn bài toán thành số học theo bội số. Bước quan trọng là tính toán$b^d$sử dụng hiệu quả phép lũy thừa tích hợp, xử lý các giá trị lớn một cách an toàn trong phạm vi số nguyên lớn của Python. 

Phép trừ`count(r) - count(l - 1)`đảm bảo bao gồm chính xác ranh giới bên trái, vì bất kỳ bội số nào của$p$bằng$l$phải được tính. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp cơ số 10 trong đó chúng tôi đếm các số có ít nhất 1 số 0 ở cuối trong phạm vi$[1, 20]$. 

Cho phép$b = 10$,$d = 1$, Vì thế$p = 10$. 

| Bước | Giá trị | 
| --- | --- | 
| p | 10 | 
| đếm(r) = 20 // 10 | 2 | 
| đếm(l-1) = 0 // 10 | 0 | 
| trả lời | 2 | 

Các số hợp lệ là 10 và 20, phù hợp với phép tính. 

Bây giờ hãy xem xét một ví dụ nhị phân:$b = 2$,$d = 3$,$l = 1$,$r = 20$. Đây$p = 8$. 

| Bước | Giá trị | 
| --- | --- | 
| p | 8 | 
| đếm(20) | 2 | 
| đếm(0) | 0 | 
| trả lời | 2 | 

Các số hợp lệ là 8 và 16, cả hai đều chia hết cho 8, phù hợp với yêu cầu có ít nhất ba số 0 ở cuối trong hệ nhị phân. 

Những dấu vết này xác nhận rằng thuật toán hoạt động nhất quán trên các cơ sở khác nhau chứ không chỉ theo trực giác thập phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một vài phép tính số học và một phép lũy thừa | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu phụ trợ | 

Những ràng buộc cho phép$l, r \le 10^{18}$, vì vậy mọi xử lý theo số sẽ không thể thực hiện được. Việc giảm bài toán về số học theo thời gian không đổi đảm bảo nó chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    b = int(input().strip())
    d = int(input().strip())
    l = int(input().strip())
    r = int(input().strip())

    if d == 0:
        return str(r - l + 1)
    p = pow(b, d)

    def count(x):
        if x <= 0:
            return 0
        return x // p

    return str(count(r) - count(l - 1))

# provided sample-like checks
assert run("10\n1\n1\n20") == "2"

# custom cases
assert run("2\n3\n1\n20") == "2"      # 8, 16
assert run("10\n0\n5\n5") == "1"      # all numbers valid
assert run("5\n2\n1\n24") == str(24 // 25)  # no multiples of 25
assert run("3\n2\n9\n27") == "3"      # 9,18,27
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 3 1 20 | 2 | đếm bội nhị phân | 
| 10 0 5 5 | 1 | trường hợp d = 0 cạnh | 
| 5 2 1 24 | 0 | không có bội số hợp lệ | 
| 3 2 9 27 | 3 | bao gồm ranh giới chính xác | 

## Vỏ cạnh 

Khi nào$d = 0$, định nghĩa trở nên trống rỗng: mọi số đều có ít nhất 0 số 0 ở cuối. Thuật toán xử lý việc này bằng cách trả về ngay kích thước khoảng đầy đủ, tránh tính lũy thừa không cần thiết. 

Khi$b^d > r$, không có số hợp lệ nào ngoại trừ số 0, nằm ngoài phạm vi vì$l \ge 1$. Công thức dựa trên phép chia đương nhiên trả về 0 vì cả hai$\lfloor r / p \rfloor$Và$\lfloor (l-1)/p \rfloor$là số không. 

Khi$l$chính nó là bội số của$b^d$, phép trừ với$l - 1$đảm bảo sự hòa nhập. Ví dụ, với$l = 8$,$b^d = 8$, thuật ngữ$count(l - 1) = count(7) = 0$, vậy 8 được tính đúng. 

Những trường hợp này xác nhận rằng công thức số học khớp với định nghĩa số 0 ở cuối trên tất cả các điều kiện biên.
