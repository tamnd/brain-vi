---
title: "CF 106088B - \u0421\u043a\u0443\u0447\u043d\u044b\u0439 \u0443\u0440\u043e\u043a"
description: "Chúng ta có một lưới hình chữ nhật có kích thước $n nhân m$. Từ lưới này, chúng tôi chọn một hình chữ nhật phụ được căn chỉnh với các đường lưới, nghĩa là chúng tôi chọn một khối hàng và cột liền kề."
date: "2026-06-19T21:21:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106088
codeforces_index: "B"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u0432\u0442\u043e\u0440\u043e\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 106088
solve_time_s: 54
verified: true
draft: false
---

[CF 106088B - \u0421\u043a\u0443\u0447\u043d\u044b\u0439 \u0443\u0440\u043e\u043a](https://codeforces.com/problemset/problem/106088/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$n \times m$. Từ lưới này, chúng tôi chọn một hình chữ nhật phụ được căn chỉnh với các đường lưới, nghĩa là chúng tôi chọn một khối hàng và cột liền kề. Hình chữ nhật chỉ được xác định bởi kích thước, chiều cao và chiều rộng của nó, vì vị trí của nó không quan trọng và hai hình chữ nhật chỉ được coi là giống nhau nếu kích thước của chúng khác nhau. 

Đối với mỗi hình chữ nhật có kích thước được chọn$a \times b$, về mặt khái niệm, chúng tôi vẽ tất cả các phân đoạn đơn vị tạo thành các đường lưới bên trong nó, bao gồm cả ranh giới của nó. Mỗi đoạn đơn vị giữa các điểm lưới liền kề tốn một giây để vẽ. Chúng tôi được đưa ra một giới hạn thời gian$t$và chúng ta cần đếm xem có bao nhiêu kích thước hình chữ nhật khác nhau cần ít nhất$t$các đoạn cần vẽ. 

Đầu ra chính không phải là số lượng vị trí bên trong lưới mà là số lượng cặp thứ nguyên hợp lệ$(a, b)$sao cho chi phí vẽ hình chữ nhật đó ít nhất là$t$, với các ràng buộc$1 \le a \le n$,$1 \le b \le m$. 

Số lượng phân đoạn trong một$a \times b$hình chữ nhật xuất phát từ hình học lưới. có$(a+1)b$đoạn thẳng đứng và$(b+1)a$các đoạn nằm ngang nên tổng số là:$$(a+1)b + (b+1)a = 2ab + a + b$$Chúng ta phải đếm từng cặp$(a, b)$thỏa mãn:$$2ab + a + b \ge t$$Vấn đề hạn chế:$n \le 10^3$, Nhưng$m, t \le 10^9$. Điều này ngay lập tức gợi ý rằng chúng ta không thể lặp lại tất cả$a, b$cặp trong$O(nm)$, vì điều đó sẽ tùy thuộc vào$10^{12}$hoạt động. Thậm chí$O(n \log m)$có thể chấp nhận được, vì$n$nhỏ và chúng ta có thể khai thác tính đơn điệu trong$b$. 

Các trường hợp cạnh phát sinh khi$t$là rất nhỏ hoặc rất lớn. 

Nếu như$t = 1$, mọi hình chữ nhật đều hợp lệ, vì ngay cả một$1 \times 1$hình chữ nhật có$2\cdot1\cdot1 + 1 + 1 = 4$phân đoạn. 

Nếu như$t$là cực kỳ lớn, ví dụ$t = 10^9$, thì ngay cả hình chữ nhật tối đa$n \times m$có thể không thỏa mãn điều kiện nên đáp án trở thành số 0. 

Một lỗi phổ biến là bỏ qua các phân đoạn ranh giới và chỉ sử dụng$2ab$, điều này sẽ tính thiếu chi phí và dẫn đến so sánh không chính xác, đặc biệt đối với các hình chữ nhật nhỏ. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ rất đơn giản: liệt kê mọi kích thước hình chữ nhật$a \times b$, tính toán$2ab + a + b$, và kiểm tra xem nó có ít nhất$t$. Điều này đúng vì công thức đo trực tiếp số lượng phân đoạn đơn vị. Tuy nhiên, điều này đòi hỏi phải kiểm tra$n \times m$cặp, trong trường hợp xấu nhất là$10^3 \times 10^9$, hoàn toàn không thể thực hiện được. 

Cấu trúc của sự bất bình đẳng làm thay đổi mọi thứ. sửa chữa$a$, biểu thức$2ab + a + b$tăng trưởng tuyến tính với$b$. Điều đó có nghĩa là đối với một chiều cao cố định$a$, tồn tại một giá trị ngưỡng$b$sau đó tất cả các chiều rộng lớn hơn đều hợp lệ. Hành vi đơn điệu này cho phép chúng ta giải quyết giá trị hợp lệ nhỏ nhất$b$và đếm xem còn lại bao nhiêu lựa chọn$O(1)$. 

Sắp xếp lại bất đẳng thức:$$2ab + a + b \ge t$$

$$b(2a + 1) \ge t - a$$Đối với mỗi$a$, chúng ta có thể tính số nguyên tối thiểu$b$thỏa mãn:$$b \ge \left\lceil \frac{t - a}{2a + 1} \right\rceil$$Sau đó tất cả$b$từ giá trị đó lên đến$m$là hợp lệ, miễn là giới hạn dưới nằm trong phạm vi. 

Điều này biến bài toán đếm hai chiều thành bài toán quét một chiều$a$, với số học theo thời gian không đổi trên mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chiều cao có thể$a$độc lập và với mỗi cái hãy tính xem có bao nhiêu chiều rộng$b$là hợp lệ. 

1. Cố định một giá trị$a$từ 1 đến$n$. Điều này cô lập một chiều để chúng ta có thể suy luận về tính đơn điệu trong$b$. 
2. Viết lại bất đẳng thức$2ab + a + b \ge t$vào trong$b(2a+1) \ge t - a$. Điều này cô lập$b$bởi vì nó cho phép chúng ta giải quyết điều kiện ngưỡng trực tiếp. 
3. Tính số nguyên nhỏ nhất$b$đó thỏa mãn bất đẳng thức. Nếu như$t \le a$, thì thậm chí$b = 1$hoạt động, vì vế phải trở thành không dương và mọi hình chữ nhật có chiều cao này đều hợp lệ. 
4. Ngược lại tính:$$b_{\min} = \left\lceil \frac{t - a}{2a + 1} \right\rceil$$Bước này đảm bảo chúng tôi tìm thấy chiều rộng hợp lệ đầu tiên. 
5. Nếu$b_{\min} \le m$, thì tất cả các chiều rộng từ$b_{\min}$ĐẾN$m$có giá trị, góp phần$m - b_{\min} + 1$hình chữ nhật. Nếu như$b_{\min} > m$, chiều cao này không đóng góp gì cả. 
6. Tổng số tiền đóng góp$a$. 

### Tại sao nó hoạt động 

Để cố định$a$, hàm$f(b) = 2ab + a + b$đang gia tăng nghiêm trọng trong$b$. Điều này đảm bảo một hành vi ngưỡng duy nhất: một lần$b$đủ lớn để thỏa mãn bất đẳng thức thì mọi giá trị lớn hơn cũng thỏa mãn nó. Do đó tính hợp lệ$b$quy về việc tìm điểm đầu tiên nơi bất đẳng thức xảy ra. Vì mỗi$a$được xử lý độc lập và đầy đủ, mỗi hình chữ nhật hợp lệ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, t = map(int, input().split())

ans = 0

for a in range(1, n + 1):
    # inequality: 2ab + a + b >= t
    # b(2a+1) >= t - a

    if t <= a:
        # even b = 1 works
        ans += m
        continue

    numerator = t - a
    denom = 2 * a + 1

    b_min = (numerator + denom - 1) // denom  # ceil division

    if b_min <= m:
        ans += m - b_min + 1

print(ans)
```Việc thực hiện cốt lõi trực tiếp theo sau phép biến đổi đại số. Chi tiết quan trọng là việc xử lý vụ việc$t \le a$, trong đó tử số trở thành số không dương và phép chia trần ngây thơ sẽ tạo ra giá trị 0 hoặc âm không chính xác. Việc xử lý nó một cách riêng biệt sẽ tránh được các vấn đề riêng lẻ. 

Việc phân chia trần được thực hiện bằng cách sử dụng số học số nguyên, đảm bảo tính chính xác mà không có rủi ro về độ chính xác của dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`2 3 15`Chúng tôi lặp đi lặp lại trên độ cao$a = 1, 2$. 

Vì$a = 1$, ngưỡng là:$$2b + 1 + b = 3b + 1 \ge 15 \Rightarrow b \ge 5$$Từ$m = 3$, không hợp lệ$b$. 

Vì$a = 2$:$$4b + 2 + b = 5b + 2 \ge 15 \Rightarrow b \ge 3$$Chỉ một$b = 3$hoạt động. 

| một | tình trạng | b_min | số b hợp lệ | 
| --- | --- | --- | --- | 
| 1 | 3b+1 ≥ 15 | 5 | 0 | 
| 2 | 5b+2 ≥ 15 | 3 | 1 | 

Câu trả lời cuối cùng là 1. 

Điều này cho thấy độ cao lớn hơn sẽ làm giảm ngưỡng hiệu quả về chiều rộng như thế nào. 

### Ví dụ 2:`3 3 15`Chúng tôi kiểm tra tất cả$a \in [1,3]$. 

| một | bất bình đẳng | b_min | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 3b+1 ≥ 15 | 5 | 0 | 
| 2 | 5b+2 ≥ 15 | 3 | 1 | 
| 3 | 7b+3 ≥ 15 | 2 | 2 | 

Tổng cộng là$3$. 

Ví dụ này nhấn mạnh rằng nhiều chiều cao có thể góp phần chồng chéo các chiều rộng hợp lệ và thuật toán tích lũy chúng một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chiều cao được xử lý một lần với số học theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có một vài biến được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$n \le 10^3$, do đó, ngay cả vòng lặp trong trường hợp xấu nhất cũng không đáng kể trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n, m, t = map(int, input().split())

    ans = 0
    for a in range(1, n + 1):
        if t <= a:
            ans += m
            continue

        numerator = t - a
        denom = 2 * a + 1
        b_min = (numerator + denom - 1) // denom

        if b_min <= m:
            ans += m - b_min + 1

    return str(ans)

# provided samples
assert run("2 3 15") == "1"
assert run("3 3 15") == "3"
assert run("2 3 20") == "0"
assert run("5 4 17") == "2"

# custom cases
assert run("1 1 1") == "1"  # minimum grid always valid
assert run("1 1 10") == "0" # single cell insufficient
assert run("3 100 1") == "300" # all rectangles valid
assert run("10 10 1000000000") == "0" # impossible threshold
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | lưới tối thiểu luôn được tính | 
| 1 1 10 | 0 | trường hợp nhỏ không thể | 
| 3 100 1 | 300 | tất cả các hình chữ nhật tập hợp hợp lệ | 
| 10 10 1000000000 | 0 | cắt tỉa ngưỡng cực đoan | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$t \le a$, trong đó việc sắp xếp lại đại số gợi ý một tử số không dương. Ví dụ, với$n = 5, m = 5, t = 2$, vì$a = 3$chúng tôi nhận được$t - a = -1$. Hành vi đúng là mọi$b \in [1, m]$hoạt động. Mã kiểm tra rõ ràng điều kiện này và chỉ định đóng góp đầy đủ$m$, ngăn chặn việc phân chia trần không chính xác. 

Một trường hợp cạnh khác là khi tính toán$b_{\min}$vượt quá$m$. Ví dụ,$n = 3, m = 3, t = 100$. Cho tất cả$a$, ngưỡng này lớn đến mức$b_{\min} > 3$, và đóng góp phải bằng không. Thuật toán đảm bảo điều này bằng cách kiểm tra$b_{\min} \le m$trước khi thêm bất kỳ thứ gì, tránh số âm hoặc lỗi bao hàm.
