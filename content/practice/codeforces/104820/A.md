---
title: "CF 104820A - \u0414\u043e\u0433\u043e\u043d\u044f\u043b\u043a\u0438"
description: "Hai người chơi xuất phát trên trục số: Alice ở vị trí $a$, Bob ở vị trí $b$, với $a < b$. Mỗi giây Alice di chuyển sang phải với tốc độ nguyên cố định $c$, và Bob di chuyển sang phải với tốc độ nguyên cố định $d$."
date: "2026-06-28T12:54:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104820
codeforces_index: "A"
codeforces_contest_name: "\u0420\u0421\u041e-\u0410\u043b\u0430\u043d\u0438\u044f 2018-2023. \u0418\u0437\u0431\u0440\u0430\u043d\u043d\u043e\u0435"
rating: 0
weight: 104820
solve_time_s: 68
verified: true
draft: false
---

[CF 104820A - \u0414\u043e\u0433\u043e\u043d\u044f\u043b\u043a\u0438](https://codeforces.com/problemset/problem/104820/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi xuất phát trên trục số: Alice ở vị trí$a$, Bob đang ở vị trí$b$, với$a < b$. Mỗi giây Alice di chuyển sang phải với một tốc độ nguyên cố định$c$và Bob di chuyển sang phải với tốc độ nguyên cố định$d$. Tốc độ của Bob$d$được biết, nhưng Alice đã quên mất tốc độ của chính mình$c$. Điều cô ấy nhớ là tốc độ của cô ấy đã được chọn sao cho tại một thời điểm nguyên nào đó$t \ge 0$, cả hai người chơi đều chiếm giữ vị trí giống nhau. 

Nhiệm vụ không phải là tái tạo lại một tốc độ hợp lệ mà là đếm xem có bao nhiêu giá trị nguyên của$c$làm cho một cuộc họp như vậy có thể thực hiện được. 

Điều kiện hội tụ là tồn tại một số nguyên$t \ge 0$như vậy$$a + ct = b + dt.$$Sắp xếp lại mang lại$$(c - d)t = b - a.$$Vậy sự khác biệt$b-a$phải chia hết cho$c-d$, và dấu của$c-d$xác định liệu Alice có thể bắt kịp hay cô ấy đã nhanh hơn rồi. 

Những ràng buộc cho phép$a, b, d$lên đến$10^{12}$, vì vậy bất kỳ cách tiếp cận nào lặp đi lặp lại tất cả những gì có thể$c$ngay lập tức là không thể. Thậm chí lặp đi lặp lại lên đến$b-a$sẽ không thể thực hiện được khi khoảng cách quá lớn. 

Một trường hợp khó nhận thấy là khi Alice đã chậm hơn Bob. Nếu như$c \le d$, sau đó$c-d \le 0$và Alice chỉ có thể gặp Bob nếu họ xuất phát ở cùng một điểm, điều này bị cấm rõ ràng vì$a < b$. Vì vậy lời giải hợp lệ phải thỏa mãn$c > d$, giảm không gian tìm kiếm xuống các giá trị dương ở trên$d$. 

Một chi tiết quan trọng khác là thời gian họp$t$phải là số nguyên. Điều này chuyển bài toán thành điều kiện chia hết trên$b-a$và vô hiệu hóa mọi lý do chỉ xem xét thời gian có giá trị thực. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là thử tất cả các tốc độ nguyên có thể$c$và kiểm tra xem có tồn tại số nguyên không$t$thỏa mãn phương trình$$a + ct = b + dt.$$Đối với một cố định$c$, chúng tôi tính toán sự khác biệt$b-a$và kiểm tra xem nó có thể được biểu diễn dưới dạng$t(c-d)$đối với một số nguyên$t \ge 0$. Điều này tương đương với việc kiểm tra xem$c-d$chia rẽ$b-a$. Tuy nhiên, lặp đi lặp lại tất cả$c$lên đến$10^{12}$quá chậm, vì mỗi lần kiểm tra có thời gian không đổi nhưng phạm vi rất lớn. 

Nhận xét quan trọng là vấn đề chỉ phụ thuộc vào sự khác biệt giữa các tốc độ chứ không phụ thuộc vào giá trị tuyệt đối của chúng. Cho phép$k = c - d$. Sau đó$k$phải là một số nguyên dương sao cho$k \mid (b-a)$. Một lần$k$đã được sửa, chúng tôi phục hồi$c = d + k$. Điều này biến bài toán thành việc đếm các ước số dương của$b-a$. 

Vì vậy, thay vì tìm kiếm theo tốc độ, chúng ta đếm các ước của một số, nhiều nhất là$10^{12}$. Việc đếm các ước số có thể được thực hiện trong$O(\sqrt{n})$bằng cách kiểm tra các cặp yếu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$c$|$O(b-a)$|$O(1)$| Quá chậm | 
| Phép liệt kê số chia |$O(\sqrt{b-a})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$n = b - a$. 

1. Tính khoảng cách$n = b - a$. Điều này cô lập số lượng duy nhất xác định liệu một cuộc họp có thể diễn ra hay không. 
2. Khởi tạo bộ đếm câu trả lời về 0. Điều này sẽ tính sự khác biệt tốc độ hợp lệ$k$. 
3. Lặp lại tất cả các số nguyên$i$từ$1$ĐẾN$\lfloor \sqrt{n} \rfloor$. Mỗi$i$được coi như một ứng cử viên ước số tiềm năng của$n$. 
4. Nếu$i$chia rẽ$n$, sau đó$i$đóng góp một giá trị hợp lệ cho$k$, và cả$n/i$đóng góp một giá trị khác trừ khi chúng bằng nhau. Lý do là mọi ước số đều tương ứng với chênh lệch tốc độ hợp lệ$k = c-d$, và mỗi cái như vậy$k$xác định chính xác một giá trị$c$. 
5. Đếm cẩn thận tất cả các ước số đó, đảm bảo rằng khi$i^2 = n$, số chia chỉ được tính một lần. 
6. Xuất ra tổng số. 

### Tại sao nó hoạt động 

phương trình$a + ct = b + dt$giảm xuống$(c-d)t = n$, Vì thế$c-d$phải là ước số dương của$n$. Ngược lại, mọi ước số dương$k$của$n$mang lại một giải pháp hợp lệ bằng cách chọn$t = n/k$Và$c = d + k$, luôn là số nguyên dương. Điều này thiết lập sự tương ứng một-một giữa tốc độ hợp lệ$c$và các ước dương của$b-a$, vì vậy tính hợp lệ$c$chính xác là đếm các ước số của$b-a$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, d = map(int, input().split())
    n = b - a

    ans = 0
    i = 1
    while i * i <= n:
        if n % i == 0:
            ans += 1
            if i * i != n:
                ans += 1
        i += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên nén vấn đề vào máy tính$n = b-a$. Sau đó nó đếm các ước của$n$bằng phương pháp căn bậc hai tiêu chuẩn. Mỗi ước số tương ứng với một lựa chọn hợp lệ của$c$, bởi vì$c = d + k$Và$k$là bất kỳ ước số nào của$n$. Vòng lặp xử lý cẩn thận trường hợp hình vuông hoàn hảo để tránh tính hai lần. 

Một điểm thực hiện tinh tế là$d$không bao giờ xuất hiện trong logic đếm số chia. Nó chỉ thay đổi giá trị cuối cùng của$c$, nhưng không ảnh hưởng đến số lượng hợp lệ$k$hiện hữu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 2 3
```Đây$n = 1$. 

| tôi | n % tôi == 0 | (các) số chia được thêm vào | trả lời | 
| --- | --- | --- | --- | 
| 1 | vâng | 1 | 1 | 

Ước số duy nhất của 1 là 1, nghĩa là$k=1$, Vì thế$c = d + 1 = 4$là tốc độ hợp lệ duy nhất. 

Điều này xác nhận rằng ngay cả khi khoảng cách là tối thiểu, phương pháp vẫn đếm chính xác một cấu hình hợp lệ. 

### Mẫu 2 

đầu vào:```
1 11 12
```Đây$n = 10$. 

| tôi | tìm được ước số | trả lời | 
| --- | --- | --- | 
| 1 | 1, 10 | 2 | 
| 2 | 2, 5 | 4 | 
| 3 | không | 4 | 

Câu trả lời cuối cùng là 4. 

Điều này cho thấy nhiều cặp ước số đóng góp độc lập và mỗi ước số tương ứng với một chênh lệch tốc độ hợp lệ riêng biệt$k$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{b-a})$| Chúng tôi chỉ kiểm tra các ước số tối đa căn bậc hai của khoảng cách | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Khoảng cách$b-a$có thể lớn như$10^{12}$, Vì thế$\sqrt{b-a} \le 10^6$. Điều này phù hợp thoải mái trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b, d = map(int, input().split())
    n = b - a

    ans = 0
    i = 1
    while i * i <= n:
        if n % i == 0:
            ans += 1
            if i * i != n:
                ans += 1
        i += 1

    return str(ans)

# provided samples
assert run("1 2 3\n") == "1"
assert run("1 11 12\n") == "4"
assert run("11 54 65\n") == "2"

# custom cases
assert run("1 2 1\n") == "1", "small gap"
assert run("1 1000000000000 1\n") == str(len([i for i in range(1, int((999999999999)**0.5)+1) if (999999999999)%i==0])*2 - (1 if int((999999999999)**0.5)**2 == 999999999999 else 0)), "large composite"
assert run("5 6 100\n") == "1", "minimal distance"
assert run("10 11 2\n") == "1", "single divisor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 | 1 | khoảng cách nhỏ nhất khác không | 
| 1 10^12 1 | tính toán | kiểm tra căng thẳng giá trị lớn | 
| 5 6 100 | 1 | trường hợp cạnh khoảng cách tối thiểu | 
| 10 11 2 | 1 | tính đúng đắn khi d không liên quan | 

## Vỏ cạnh 

Khi nào$b-a = 1$, thuật toán chỉ kiểm tra$i = 1$, tìm chính xác một ước số và trả về 1. Điều này tương ứng với sự khác biệt về tốc độ duy nhất có thể có$k = 1$, mang lại thời gian họp hợp lệ$t = 1$. 

Khi$b-a$là một hình vuông hoàn hảo, chẳng hạn như$n = 36$, vòng lặp đạt tới$i = 6$. Số chia$6$chỉ nên được tính một lần và điều kiện`if i * i != n`đảm bảo tính chính xác bằng cách tránh tính hai lần. 

Khi$n$là số nguyên tố, chỉ$1$Và$n$được tính. Vòng lặp xác định chính xác chính xác hai ước số, phản ánh chính xác hai chênh lệch tốc độ hợp lệ và do đó hai giá trị hợp lệ của$c$.
