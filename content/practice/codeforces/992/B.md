---
title: "CF 992B - Tin học nghiên cứu Nastya"
description: "Chúng ta có bốn số nguyên $l, r, x, y$. Nhiệm vụ là đếm các cặp có thứ tự $(a, b)$ sao cho cả hai số đều nằm trong khoảng $[l, r]$, và ước chung lớn nhất của chúng chính xác là $x$ trong khi bội số chung nhỏ nhất của chúng chính xác là $y$."
date: "2026-06-17T00:20:47+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 992
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 489 (Div. 2)"
rating: 1600
weight: 992
solve_time_s: 77
verified: true
draft: false
---

[CF 992B - Tin học nghiên cứu của Nastya](https://codeforces.com/problemset/problem/992/B) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho bốn số nguyên$l, r, x, y$. Nhiệm vụ là đếm các cặp có thứ tự$(a, b)$sao cho cả hai số đều nằm trong khoảng$[l, r]$, và ước chung lớn nhất của chúng chính xác là$x$trong khi bội số chung nhỏ nhất của chúng chính xác là$y$. 

Thay vì suy nghĩ theo các số nguyên tùy ý, sẽ hữu ích khi xem điều kiện như một ràng buộc cấu trúc rất cứng nhắc về cách thức$a$Và$b$được xây dựng. GCD buộc cả hai số phải chia sẻ một hệ số cơ sở chung, trong khi LCM buộc cấu trúc nguyên tố kết hợp của chúng phải khớp chính xác$y$. Vì vậy, chúng ta không tìm kiếm tự do bên trong khoảng mà là tìm kiếm các cặp phải thẳng hàng hoàn hảo với một mẫu số học cố định. 

Giới hạn$l, r \le 10^9$loại trừ mọi cách tiếp cận lặp lại trên tất cả các cặp ứng cử viên. Ngay cả việc lặp lại một biến duy nhất cũng là giới hạn nếu được thực hiện cho mỗi lần kiểm tra không có cấu trúc. Giải pháp phải giảm không gian tìm kiếm xuống mức phụ thuộc vào ước số hoặc phân tích nhân tử của$y$, không phải trên kích thước khoảng. 

Một tình huống quan trọng phát sinh khi$y$không chia hết cho$x$. Trong trường hợp đó, không có cặp nào có thể tồn tại vì GCD phải chia LCM và bất kỳ cặp hợp lệ nào cũng phải thỏa mãn$x \mid y$. Một trường hợp tinh vi khác là khi ngay cả sau khi thỏa mãn tính chia hết, một trong các biến được chuyển đổi vẫn nằm ngoài khoảng cho phép sau khi chuẩn hóa. 

Ví dụ, nếu$l = 1, r = 2, x = 2, y = 3$, không có cặp nào vì$x$không chia$y$, mặc dù cả hai giới hạn đều đủ nhỏ để lực lượng vũ phu vẫn có thể thử các ứng cử viên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp thử tất cả các cặp$(a, b)$trong phạm vi$[l, r]$và kiểm tra xem GCD của họ có$x$và LCM là$y$. Điều này đúng nhưng có trường hợp xấu nhất là$(r-l+1)^2$séc. Với$r$lên đến$10^9$, ngay cả một khoảng rút gọn vẫn có thể quá lớn để có thể liệt kê được. 

Quan sát chính là loại bỏ ràng buộc GCD trước tiên bằng cách chuẩn hóa cả hai số. Nếu chúng ta đặt$a = x \cdot A$Và$b = x \cdot B$, sau đó$\gcd(A, B) = 1$. Phép biến đổi này tách biệt thừa số chung và giảm vấn đề đếm các cặp nguyên tố cùng nhau. 

Thay vào điều kiện LCM, ta có:$$\text{lcm}(a, b) = x \cdot A \cdot B = y$$ngụ ý:$$A \cdot B = \frac{y}{x} = k$$Vì vậy, toàn bộ vấn đề giảm xuống việc đếm các cặp yếu tố có thứ tự$(A, B)$như vậy$A \cdot B = k$,$\gcd(A, B) = 1$, và cả hai$xA, xB$nằm ở$[l, r]$. 

Điều này mang lại một cấu trúc rõ ràng: chúng ta chỉ cần liệt kê các ước của$k$và với mỗi ước số$A$, định nghĩa$B = k / A$, sau đó kiểm tra các ràng buộc về tính đồng nguyên tố và khoảng. 

Lực lượng vũ phu không thành công vì nó tìm kiếm trong không gian có kích thước 2D$O(r^2)$, trong khi bài toán biến đổi chỉ phụ thuộc vào cấu trúc ước số, tức là$O(\sqrt{k})$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\sqrt{y/x})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên hãy kiểm tra xem$y$chia hết cho$x$. Nếu không, câu trả lời ngay lập tức là 0 vì không có tỷ lệ GCD nào có thể tạo ra LCM không bội số. Điều này tránh được những công việc không cần thiết. 
2. Tính toán$k = y / x$. Bây giờ chúng ta giải thích lại vấn đề theo các cặp nhân tố của$k$, trong đó mỗi cặp hợp lệ tương ứng với$(A, B)$. 
3. Lặp lại tất cả các số nguyên$A$như vậy$A \cdot A \le k$. Mỗi cái như vậy$A$đại diện cho một ước số tiềm năng của$k$, và chúng ta có thể tính toán$B = k / A$khi$A$chia rẽ$k$. 
4. Với mỗi ước số$A$, kiểm tra xem$k \bmod A = 0$. Nếu không, hãy bỏ qua vì nó không thể tạo thành một cặp hợp lệ. 
5. Hãy để$B = k / A$. Kiểm tra xem$\gcd(A, B) = 1$. Điều kiện này đảm bảo rằng các số ban đầu$xA$Và$xB$có GCD chính xác$x$, không lớn hơn 
6. Chuyển đổi về tên miền ban đầu bằng cách xác minh giới hạn:$l \le xA \le r$Và$l \le xB \le r$. 
7. Nếu tất cả các điều kiện đều đúng, hãy đếm các cặp đã đặt hàng. Nếu như$A \ne B$, cả hai$(A, B)$Và$(B, A)$là hợp lệ, vì vậy hãy thêm 2. Nếu$A = B$, thêm 1. 

### Tại sao nó hoạt động 

Bất kỳ cặp hợp lệ$(a, b)$phải có ước chung lớn nhất$x$, do đó chia cả hai số cho$x$tạo ra số nguyên$A$Và$B$đó là nguyên tố cùng nhau. Điều kiện LCM buộc sản phẩm của họ phải chính xác$y/x$, do đó mọi cặp hợp lệ đều tương ứng một cách khách quan với hệ số nguyên tố cùng nhau của$k$. Giới hạn chỉ lọc những hệ số nào thực sự nằm trong khoảng. Vì mỗi bước duy trì sự tương đương theo cả hai hướng nên không có cặp hợp lệ nào bị mất và không có cặp không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    l, r, x, y = map(int, input().split())
    
    if y % x != 0:
        print(0)
        return
    
    k = y // x
    ans = 0
    
    for a in range(1, int(math.isqrt(k)) + 1):
        if k % a != 0:
            continue
        b = k // a
        
        if math.gcd(a, b) != 1:
            continue
        
        A = x * a
        B = x * b
        
        if l <= A <= r and l <= B <= r:
            if a == b:
                ans += 1
            else:
                ans += 2
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xác nhận điều kiện chia hết cần thiết giữa$x$Và$y$. Không có nó, phương trình đã biến đổi$A \cdot B = k$thậm chí sẽ không tích phân, do đó không gian tìm kiếm sẽ sụp đổ ngay lập tức. 

Vòng lặp sử dụng các ước số$\sqrt{k}$bởi vì mỗi cặp số chia đều được gặp đúng một lần thông qua$a$Và$k/a$. Kiểm tra gcd thực thi rằng không còn yếu tố chung bổ sung nào sau khi loại bỏ$x$, điều này rất cần thiết cho tính đúng đắn của phép biến đổi ngược. 

Việc kiểm tra ranh giới được áp dụng sau khi xây dựng lại các giá trị ban đầu$a = xA$Và$b = xB$, vì các ràng buộc được xác định trong miền gốc chứ không phải miền chuẩn hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 1 2
```Đây$k = 2$. Các ước số là (1, 2). 

| A | B | gcd(A,B) | xA | xB | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 2 | vâng | 
| 2 | 1 | 1 | 2 | 1 | vâng | 

Cả hai cặp đều nằm trong giới hạn nên câu trả lời là 2. 

Điều này xác nhận rằng thứ tự được giữ nguyên và cả hai cấu hình đối xứng đều được tính. 

### Ví dụ 2 

đầu vào:```
1 12 3 12
```Đây$k = 4$. Các ước số là (1,4), (2,2), (4,1). 

| A | B | gcd(A,B) | xA | xB | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 3 | 12 | vâng | 
| 2 | 2 | 2 | 6 | 6 | không | 
| 4 | 1 | 1 | 12 | 3 | vâng | 

Câu trả lời là 2 cặp có thứ tự hợp lệ. 

Ví dụ này cho thấy điều kiện đồng nguyên tố loại bỏ việc phân tích nhân tử theo đường chéo như thế nào trong đó các thừa số chung vẫn còn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{y/x})$| Ta liệt kê các ước của$k = y/x$và mỗi lần kiểm tra là thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ bất kể kích thước đầu vào | 

Giới hạn số chia đảm bảo lời giải vừa vặn thoải mái trong vòng 1 giây ngay cả khi$y$tùy thuộc vào$10^9$, vì vòng lặp chạy nhiều nhất là khoảng$3 \times 10^4$lần lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    l, r, x, y = map(int, sys.stdin.readline().split())
    
    if y % x != 0:
        return "0\n"
    
    k = y // x
    ans = 0
    
    for a in range(1, int(math.isqrt(k)) + 1):
        if k % a != 0:
            continue
        b = k // a
        if math.gcd(a, b) != 1:
            continue
        A = x * a
        B = x * b
        if l <= A <= r and l <= B <= r:
            ans += 1 if a == b else 2
    
    return str(ans) + "\n"

# provided samples
assert run("1 2 1 2\n") == "2\n", "sample 1"

# custom cases
assert run("1 1 1 1\n") == "1\n", "single point"
assert run("1 10 2 20\n") == "2\n", "simple factor pairs"
assert run("1 100 3 12\n") == "2\n", "multiple divisors with gcd filtering"
assert run("1 100 5 7\n") == "0\n", "impossible case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | trường hợp ranh giới tối thiểu | 
| 1 10 2 20 | 2 | cặp nhân tố hợp lệ cơ bản | 
| 1 100 3 12 | 2 | gcd lọc chính xác | 
| 1 100 5 7 | 0 | trường hợp chia hết không hợp lệ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$y$không chia hết cho$x$. Ví dụ, với đầu vào$l=1, r=100, x=4, y=6$, sự chuyển đổi sẽ yêu cầu$A \cdot B = 1.5$, điều đó là không thể. Thuật toán ngay lập tức trả về 0 trước khi thử bất kỳ tìm kiếm ước số nào. 

Một trường hợp cạnh khác là khi$A = B$, nghĩa$k$là một hình vuông hoàn hảo Trong tình huống đó, chỉ có một thứ tự tồn tại thay vì hai. Ví dụ,$x=2, y=8$cho$k=4$, và cặp$A=B=2$tạo ra chính xác một cấu hình hợp lệ. 

Trường hợp thứ ba là cắt ranh giới: ngay cả khi một cặp nhân tố hợp lệ ở dạng chuẩn hóa, chia tỷ lệ theo$x$có thể đẩy nó ra ngoài$[l, r]$. Ví dụ,$l=10, r=12, x=3, y=12$tạo ra cặp chuẩn hóa$(1,4)$, Nhưng$3\cdot4=12$có hiệu lực trong khi$3\cdot1=3$thì không, vì vậy chỉ có một hướng tồn tại sau khi lọc giới hạn.
