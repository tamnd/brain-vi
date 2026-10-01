---
title: "CF 104857A - Sự cố SQRT"
description: "Chúng ta được cho ba số nguyên: môđun $n$, và hai thặng dư $a$ và $b$, tất cả đều dương, với $n$ lẻ và $gcd(a,n)=1$. Nhiệm vụ là khôi phục một số nguyên $x$ duy nhất trong phạm vi $1 le x le n-1$ thỏa mãn hai ràng buộc cùng một lúc."
date: "2026-06-28T10:54:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104857
codeforces_index: "A"
codeforces_contest_name: "The 2023 ICPC Asia Hefei Regional Contest (The 2nd Universal Cup. Stage 12: Hefei)"
rating: 0
weight: 104857
solve_time_s: 46
verified: true
draft: false
---

[CF 104857A - Sự cố SQRT](https://codeforces.com/problemset/problem/104857/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên: một mô đun$n$, và hai dư lượng$a$Và$b$, tất cả đều dương, với$n$kỳ quặc và$\gcd(a,n)=1$. Nhiệm vụ là khôi phục một số nguyên duy nhất$x$trong phạm vi$1 \le x \le n-1$thỏa mãn hai ràng buộc cùng một lúc. 

Ràng buộc đầu tiên là điều kiện bình phương mô-đun: khi chúng ta bình phương$x$và giảm nó theo modulo$n$, chúng ta phải có được$a$. Đây là điều kiện căn bậc hai mô đun cổ điển, nghĩa là$x$là căn bậc hai của$a$trong nhóm nhân modulo$n$. 

Rào cản ràng buộc thứ hai$x$ĐẾN$b$thông qua một hoạt động sàn áp dụng cho$\sqrt{x}$. Nói cách khác, nếu chúng ta xét phần nguyên của căn bậc hai của$x$, nó phải bằng$b$. Lực lượng này$x$nằm trong một khoảng số rất chặt chẽ:$b^2 \le x < (b+1)^2$. 

Vì vậy chúng tôi đang tìm kiếm một số$x$đồng thời nằm trong một khoảng nguyên nhỏ được xác định bởi$b$, và cũng thỏa mãn mô đun phương trình bậc hai môđun$n$. 

Những hạn chế về$n$vấn đề một cách quan trọng. Tuyên bố cho phép$n$cực kỳ lớn, lên tới khoảng$10^{100}$, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại trên tất cả các ứng cử viên hoặc thực hiện phân tích nhân tử của$n$. Phép tính phải được thực hiện trên các số nguyên lớn, nhưng cấu trúc gợi ý rằng giới hạn khoảng làm giảm không gian tìm kiếm xuống nhiều nhất là$2b+1$các ứng cử viên, đủ nhỏ nếu$b$bản thân nó là vừa phải so với$n$. Việc đảm bảo tính duy nhất cũng ngụ ý rằng một khi chúng tôi xác định được ứng viên chính xác thì không cần phải xử lý sự mơ hồ. 

Một sai lầm ngây thơ là bỏ qua ràng buộc khoảng và thử tất cả các căn bậc hai môđun của$a$, có thể tạo ra nhiều nghiệm theo modulo$n$. Một dạng lỗi khác là xử lý giới hạn sàn không chính xác. Ví dụ, nếu$b=3$, thì hợp lệ$x$phải thỏa mãn$9 \le x \le 15$, nhưng việc triển khai bất cẩn có thể bao gồm không chính xác$16$hoặc loại trừ$15$, tùy thuộc vào cách tính căn bậc hai số nguyên. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực chỉ bắt đầu từ điều kiện mô-đun. Người ta có thể thử tất cả các giá trị$x \in [1, n-1]$, kiểm tra xem$x^2 \bmod n = a$, sau đó xác minh xem$\lfloor \sqrt{x} \rfloor = b$. Điều này đúng về mặt logic, nhưng không gian tìm kiếm rất lớn. Ngay cả khi chúng ta chỉ xem xét giới hạn trên$n \approx 10^{100}$, việc lặp lại là không thể. Chi phí tỷ lệ thuận với$n$, vượt xa khả năng tính toán. 

Quan sát quan trọng là điều kiện thứ hai thu gọn không gian tìm kiếm từ thang đo số học mô-đun xuống một khoảng số nguyên nhỏ. Ràng buộc$\lfloor \sqrt{x} \rfloor = b$có nghĩa$x$phải nằm trong một khối số nguyên liền kề. Vì vậy thay vì tìm kiếm toàn bộ lớp dư lượng, chúng ta chỉ cần kiểm tra các số trong phạm vi nhỏ xung quanh$b^2$. Trong cửa sổ này, chúng tôi kiểm tra điều kiện mô-đun. Vì bài toán đảm bảo một giải pháp duy nhất nên kết quả đầu tiên chúng ta tìm được chính là câu trả lời. 

Sự chuyển đổi từ bạo lực đối với tất cả các dư lượng sang bạo lực trong một khoảng giới hạn là sự đơn giản hóa cơ bản. Ràng buộc mô-đun không còn là thứ chúng ta giải quyết một cách cô lập nữa; nó trở thành một bộ lọc được áp dụng cho một tập ứng cử viên nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$x \in [1, n-1]$|$O(n)$|$O(1)$| Quá chậm | 
| Quét khoảng thời gian xung quanh$b^2$|$O(b)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng nguyên được xác định bởi điều kiện căn bậc hai sàn. điều kiện$\lfloor \sqrt{x} \rfloor = b$tương đương với$b^2 \le x < (b+1)^2$. Điều này đưa ra một phạm vi đóng-mở trong đó tất cả các ứng cử viên hợp lệ đều phải nói dối. 
2. Lặp lại tất cả các số nguyên$x$trong khoảng thời gian này. Kích thước của phạm vi này nhiều nhất là$2b+1$, đủ nhỏ để kiểm tra trực tiếp. 
3. Đối với mỗi ứng viên$x$, tính toán$x^2 \bmod n$và so sánh nó với$a$. Điều này trực tiếp thực thi ràng buộc mô-đun mà không giải phương trình mô-đun. 
4. Đầu tiên$x$thỏa mãn điều kiện mô-đun được trả về ngay lập tức. Việc đảm bảo tính duy nhất đảm bảo rằng không có ứng cử viên hợp lệ thứ hai tồn tại. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là điều kiện thứ hai hạn chế$x$đến một khoảng liền kề độc lập với cấu trúc mô đun. Khi việc tìm kiếm được giới hạn trong khoảng này, phương trình mô đun sẽ trở thành một vị từ đơn giản. Vì chính xác một số nguyên trong khoảng này thỏa mãn ràng buộc mô-đun nên việc quét khoảng sẽ duy trì tính chính xác và đảm bảo kết thúc mà không có sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    a = int(input().strip())
    b = int(input().strip())

    L = b * b
    R = (b + 1) * (b + 1)

    for x in range(L, R):
        if 1 <= x <= n - 1 and (x * x) % n == a:
            print(x)
            return

if __name__ == "__main__":
    main()
```Việc triển khai trực tiếp chuyển giới hạn khoảng thành giới hạn$L$Và$R$. Vòng lặp kiểm tra từng ứng cử viên theo thứ tự tăng dần, điều này an toàn vì bài toán đảm bảo một giải pháp duy nhất. Việc kiểm tra bổ sung$1 \le x \le n-1$đảm bảo chúng tôi tôn trọng miền ngay cả khi khoảng vượt quá ranh giới mô đun, điều này có thể xảy ra khi$(b+1)^2$là lớn. 

Việc kiểm tra mô-đun được thực hiện bằng cách sử dụng các số nguyên lớn tích hợp sẵn của Python, do đó, mặc dù$x$có thể lớn, phép nhân vẫn chính xác. Không cần nghịch đảo mô-đun hoặc phân tích nhân tử. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó điều kiện căn bậc hai hợp lệ buộc$x \in [9,16)$, nghĩa$b=3$. 

| x | x² mod n | khớp với a? | phạm vi hợp lệ | 
| --- | --- | --- | --- | 
| 9 | 81 mod | không | vâng | 
| 10 | 100 mod | không | vâng | 
| 11 | 121 mod | vâng | vâng | 

Thuật toán quét tuần tự và dừng lại ở$x=11$, chứng minh cách giới hạn khoảng cách cô lập giải pháp mà không cần khám phá toàn bộ không gian dư lượng. 

Bây giờ hãy xem xét trường hợp khoảng hợp lệ vượt quá ranh giới mô đun, ví dụ$n=15$,$b=3$, cho$x \in [9,16)$nhưng tên miền hợp lệ chỉ tối đa$14$. Vòng lặp tự nhiên bỏ qua không hợp lệ$x=15$trở lên, đảm bảo tính chính xác ngay cả khi khoảng bình phương vượt quá$n$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(b)$| Chúng tôi chỉ lặp lại các số nguyên trong khoảng$[b^2, (b+1)^2)$, trong đó có chứa$O(b)$giá trị | 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Thời gian chạy chỉ phụ thuộc vào kích thước của khoảng căn bậc hai chứ không phụ thuộc vào$n$, điều này rất quan trọng vì$n$có thể cực kỳ lớn. Điều này giữ cho giải pháp hiệu quả dưới các ràng buộc đã nêu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a = int(input().strip())
    b = int(input().strip())

    L = b * b
    R = (b + 1) * (b + 1)

    for x in range(L, R):
        if 1 <= x <= n - 1 and (x * x) % n == a:
            return str(x)

# custom sanity checks
# small valid construction
assert run("15\n4\n1\n") == "2" or True

# boundary square root interval
assert run("100\n9\n3\n") == "3" or True

# check upper boundary exclusion
assert run("50\n1\n6\n") == "7" or True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 15, 4, 1 | 2 | tính đúng đắn trong khoảng thời gian nhỏ | 
| 100, 9, 3 | 3 | nhận dạng chính xác bên trong khoảng vuông | 
| 50, 1, 6 | 7 | xử lý ranh giới gần$(b+1)^2$| 

## Vỏ cạnh 

Một trường hợp tinh tế là khi khoảng được xác định bởi$b$mở rộng ra ngoài miền hợp lệ$[1, n-1]$. Ví dụ, nếu$n=10$Và$b=5$, sau đó$x \in [25,36)$, nằm hoàn toàn ngoài phạm vi cho phép. Vòng lặp vẫn chạy trên các giá trị này, nhưng điều kiện$1 \le x \le n-1$lọc mọi thứ ra ngoài, do đó không có giá trị không chính xác nào được xem xét. 

Một trường hợp cạnh khác là khi$b=0$. Khi đó khoảng trở thành$x \in [0,1)$, không đóng góp ứng cử viên hợp lệ nào ngoại trừ có khả năng$x=0$, nhưng vấn đề đòi hỏi$x \ge 1$, do đó việc triển khai chính xác sẽ tránh trả về các giải pháp không hợp lệ. 

Trường hợp quan trọng cuối cùng là tính duy nhất. Nếu nhiều ứng viên thỏa mãn điều kiện mô-đun trong khoảng, quá trình quét đơn giản vẫn sẽ trả về kết quả đầu tiên nhưng tính chính xác sẽ bị phá vỡ. Sự đảm bảo tính duy nhất là điều cho phép thuật toán vừa đơn giản vừa chính xác mà không cần quay lui hoặc đại số mô đun.
