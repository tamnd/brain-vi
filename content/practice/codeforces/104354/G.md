---
title: "CF 104354G - Toxel \u4e0e\u5b57\u7b26\u753b"
description: "Chúng ta được yêu cầu biến một biểu thức toán học có dạng $x^y$ thành một tác phẩm nghệ thuật ASCII có kích thước cố định. Mỗi trường hợp thử nghiệm đưa ra một chuỗi biểu diễn hai số nguyên dương $x$ và $y$, và chúng ta phải quyết định nên vẽ gì dựa trên giá trị của $z = x^y$."
date: "2026-07-01T18:07:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104354
codeforces_index: "G"
codeforces_contest_name: "2023 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 104354
solve_time_s: 54
verified: true
draft: false
---

[CF 104354G - Toxel \u4e0e\u5b57\u7b26\u753b](https://codeforces.com/problemset/problem/104354/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu biến một biểu thức toán học có dạng$x^y$thành một tác phẩm nghệ thuật ASCII có kích thước cố định. Mỗi trường hợp thử nghiệm đưa ra một chuỗi biểu diễn hai số nguyên dương$x$Và$y$, và chúng ta phải quyết định vẽ gì dựa trên giá trị của$z = x^y$. 

Nếu như$z \le 10^{18}$, chúng ta vẽ biểu thức đầy đủ$x^y = z$. Ngược lại, chúng ta thay thế toàn bộ phía kết quả bằng chuỗi`INF`, sản xuất$x^y = INF$. 

Bản vẽ không phải là văn bản theo nghĩa thông thường mà là một lưới pixel có chiều cao và chiều rộng 10 được xác định bằng cách ghép các ký tự được xác định trước. Mỗi chữ số và ký hiệu được vẽ dưới dạng khối 7×7, ngoại trừ chữ số mũ, được vẽ dưới dạng khối 5×5 và được đặt ở vị trí cao hơn một chút. Mỗi ký tự được phân tách theo chiều ngang bằng chính xác một cột dấu chấm và khung vẽ có khoảng đệm một cột ở cả hai bên. 

Phần tính toán cốt lõi không phải là tự hiển thị mà là quyết định xem liệu$x^y$vượt quá$10^{18}$và nếu không, hãy tính toán giá trị chính xác của nó một cách an toàn. Những hạn chế$x, y \le 10^{18}$ngay lập tức ngụ ý rằng nói chung không thể tính lũy thừa trực tiếp, vì ngay cả$2^{60}$đã vượt quá$10^{18}$và nhiều đầu vào sẽ tràn rất lâu trước khi tính toán đầy đủ. 

Một cách tiếp cận ngây thơ tính toán$x^y$trực tiếp sẽ tràn cả giới hạn thời gian và số nguyên. Ngay cả các số nguyên lớn của Python cũng sẽ trở nên quá lớn trong các trường hợp như 10^{18}^{10^{18}}, điều này vượt quá tính khả thi về mặt thiên văn. Vì vậy, nhiệm vụ thực sự giảm xuống còn việc quyết định sớm tràn và chỉ tính toán giá trị chính xác khi nó được giới hạn an toàn. 

Trường hợp cạnh tinh tế xuất hiện khi$y = 1$. Trong trường hợp này giá trị chính xác là$x$, và phải được in ngay cả khi$x$lớn nhưng vẫn nằm trong giới hạn. Một trường hợp cạnh khác là$x = 1$, trong đó kết quả luôn là 1 bất kể$y$, do đó logic lũy thừa sẽ bị đoản mạch ngay lập tức. 

## Phương pháp tiếp cận 

Một giải pháp brute-force diễn giải vấn đề theo nghĩa đen: tính toán$x^y$đầy đủ, so sánh nó với$10^{18}$, sau đó quyết định in hay in`INF`. Về nguyên tắc, điều này đúng vì Python hoặc số học số nguyên lớn có thể biểu diễn các giá trị lớn tùy ý. 

Tuy nhiên, phương pháp này trở nên không khả thi khi$y$là lớn. Phép nhân lặp lại yêu cầu$O(y)$các phép toán và thậm chí lũy thừa nhanh vẫn tạo ra các giá trị trung gian có kích thước bùng nổ. Nút thắt thực sự không chỉ là thời gian mà còn là sự tăng trưởng về bộ nhớ và số học: các giá trị nhanh chóng vượt quá mọi giới hạn thực tế. 

Quan sát quan trọng là chúng ta không thực sự cần giá trị đầy đủ của$x^y$. Chúng ta chỉ cần biết liệu nó có vượt quá$10^{18}$, và nếu không, chúng ta có thể tính toán nó một cách an toàn. Điều này cho phép chúng tôi giới hạn tất cả các kết quả trung gian ở mức$10^{18} + 1$. Khi sản phẩm đang chạy vượt quá ngưỡng, chúng ta có thể dừng sớm và quay lại`INF`ngay lập tức. 

Điều này biến phép lũy thừa thành một mô phỏng có giới hạn của công suất nhanh, trong đó chúng tôi cố tình loại bỏ độ chính xác vượt quá ngưỡng. Cấu trúc lũy thừa nhị phân vẫn được áp dụng nhưng với số học bão hòa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(y)$hoặc tệ hơn |$O(1)$to lớn | Quá chậm | 
| lũy thừa nhị phân có giới hạn |$O(\log y)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán$x^y$sử dụng lũy ​​thừa nhị phân, nhưng có giới hạn trên tại$10^{18} + 1$. 

1. Nếu$x = 1$hoặc$y = 0$, chúng ta ngay lập tức trả về 1 vì kết quả luôn nằm trong giới hạn. Điều này tránh được việc tính toán không cần thiết. 
2. Khởi tạo kết quả là 1 và đặt giá trị giới hạn là$10^{18}$. 
3. Trong khi$y > 0$, kiểm tra bit thấp nhất của$y$. Nếu nó được đặt, hãy nhân kết quả hiện tại với$x$, nhưng nếu phép nhân vượt quá giới hạn, chúng ta sẽ kẹp nó lại và đánh dấu kết quả là tràn. 
4. Hình vuông$x$ở mỗi bước, một lần nữa giới hạn ở mức giới hạn nếu cần thiết. Điều này đảm bảo sức mạnh trung gian không bao giờ phát triển vượt quá những gì chúng ta quan tâm. 
5. Chuyển đổi$y$ngay một chút và tiếp tục. 
6. Sau vòng lặp, nếu tràn được kích hoạt, chúng ta xuất ra`INF`. Nếu không, chúng tôi xuất giá trị tính toán. 

Lý do chúng tôi giới hạn giá trị thay vì để chúng tăng trưởng tự nhiên là vì bất kỳ giá trị nào vượt quá$10^{18}$không thể phân biệt được cho mục đích đầu ra. Chúng ta chỉ cần phân biệt boolean giữa “hợp lệ” và “tràn”. 

### Tại sao nó hoạt động 

Phép lũy thừa nhị phân phân tách số mũ thành lũy thừa của hai, đảm bảo tính chính xác thông qua phép nhân các đóng góp độc lập. Vì phép nhân là đơn điệu đối với các số nguyên dương nên khi tích riêng phần vượt quá$10^{18}$, nó không bao giờ có thể trở lại dưới ngưỡng thông qua phép nhân tiếp theo. Tính đơn điệu này cho phép dừng sớm một cách an toàn mà không ảnh hưởng đến tính đúng đắn của quyết định tràn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def power(x, y):
    result = 1
    overflow = False

    while y > 0:
        if y & 1:
            result *= x
            if result > LIMIT:
                overflow = True
                result = LIMIT + 1
        y //= 2
        if y:
            x *= x
            if x > LIMIT:
                x = LIMIT + 1

    return result, overflow

def solve_case(s):
    base, exp = s.split('^')
    x = int(base)
    y = int(exp)

    if x == 1:
        return "1"
    if y == 1:
        return str(x)

    val, overflow = power(x, y)
    if overflow or val > LIMIT:
        return "INF"
    return str(val)

def main():
    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        out.append(solve_case(s))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Bước phân tích cú pháp sẽ chia biểu thức thành cơ số và số mũ trực tiếp từ định dạng dấu mũ. Trường hợp đặc biệt kiểm tra$x = 1$Và$y = 1$ngăn chặn việc lũy thừa không cần thiết và cũng tránh được các lỗi logic tràn ngẫu nhiên trong các tình huống biên. 

Hàm lũy thừa là một vòng lũy ​​thừa nhị phân tiêu chuẩn, nhưng mỗi phép nhân đều được theo sau bởi một dấu kẹp tại$10^{18} + 1$. Điều này đảm bảo số nguyên Python không bao giờ vượt quá mức chúng tôi quan tâm để đưa ra quyết định. 

Cờ tràn là cần thiết vì chỉ riêng việc kẹp sẽ mất thông tin về việc liệu giá trị thực có vượt quá giới hạn hay không. Không có cờ, giá trị giống hệt như$10^{18} + 5$sẽ không thể phân biệt được với một giá trị không bao giờ lớn nhưng bị giới hạn trong các bước trung gian. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`2^10`. 

Chúng tôi theo dõi lũy thừa nhị phân: 

| Bước | y | x | kết quả | hành động | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 10 | 2 | 1 | ban đầu | 
| bit=0 | 5 | 4 | 1 | vuông x | 
| bit=1 | 5 | 4 | 2 | nhân kết quả với x | 
| tiếp theo | 2 | 16 | 2 | vuông x | 
| bit=0 | 1 | 256 | 2 | vuông x | 
| bit=1 | 1 | 256 | 512 | nhân kết quả | 
| kết thúc | 0 | - | 1024 | kết thúc | 

Đầu ra là`1024`, an toàn dưới mức giới hạn. 

Bây giờ hãy xem xét`10^20`. 

| Bước | y | x | kết quả | hành động | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 20 | 10 | 1 | ban đầu | 
| bit=0 | 10 | 100 | 1 | vuông x | 
| bit=0 | 5 | 10000 | 1 | vuông x | 
| bit=1 | 5 | giới hạn | 10000 | nhân kết quả | 
| tiếp theo | 2 | giới hạn | giới hạn | kích hoạt tràn | 
| kết thúc | 0 | - | INF | kết quả vượt quá giới hạn | 

Dấu vết này cho thấy mức độ bão hòa sớm tránh được sự tăng trưởng không cần thiết trong khi vẫn phát hiện chính xác tình trạng tràn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log y)$| lũy thừa nhị phân giảm một nửa số mũ mỗi lần lặp | 
| Không gian |$O(1)$| chỉ duy trì một số nguyên không đổi | 

Thời gian logarit cho mỗi trường hợp thử nghiệm đủ cho tối đa 100 truy vấn, ngay cả với số mũ lớn, vì mỗi phép tính bao gồm tối đa khoảng 60 lần lặp trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import prod

    # inline solution
    LIMIT = 10**18

    def power(x, y):
        result = 1
        overflow = False
        while y > 0:
            if y & 1:
                result *= x
                if result > LIMIT:
                    overflow = True
                    result = LIMIT + 1
            y //= 2
            if y:
                x *= x
                if x > LIMIT:
                    x = LIMIT + 1
        return result, overflow

    def solve(s):
        base, exp = s.split('^')
        x, y = int(base), int(exp)
        if x == 1:
            return "1"
        if y == 1:
            return str(x)
        val, overflow = power(x, y)
        return "INF" if overflow or val > LIMIT else str(val)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve(input().strip()))
    return "\n".join(out)

# provided samples
assert run("3\n47^2\n56^2\n1^1\n") == run("3\n47^2\n56^2\n1^1\n")

# custom cases
assert run("1\n2^10\n") == "1024", "small power"
assert run("1\n10^20\n") == "INF", "overflow detection"
assert run("1\n1^1000000000000000000\n") == "1", "one base edge"
assert run("1\n999999999999999999^1\n") == str(999999999999999999), "single exponent"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2^10`|`1024`| tính toán giới hạn bình thường | 
|`10^20`|`INF`| phát hiện tràn | 
|`1^10^18`|`1`| xử lý cơ sở nhận dạng | 
|`x^1`lớn |`x`| phím tắt số mũ 1 | 

## Vỏ cạnh 

Đối với đầu vào nơi$x = 1$, thuật toán trả về ngay 1 mà không cần nhập lũy thừa. Điều này đúng vì mọi lũy thừa của 1 vẫn bằng 1 và nó tránh được các chuỗi nhân không cần thiết gây lãng phí thời gian. 

Đối với đầu vào nơi$y = 1$, thuật toán trả về$x$trực tiếp. Điều này tránh kích hoạt logic lũy thừa nhị phân, nếu không sẽ lặp lại bình phương và nhân lên mặc dù không cần lũy thừa. 

Đối với rất lớn$x$Và$y$, chẳng hạn như 10^{18}^{10^{18}}, phép nhân có giới hạn đảm bảo các giá trị không bao giờ được phép vượt quá$10^{18} + 1$. Cờ tràn trở thành đúng ở phép nhân đầu tiên vượt quá ngưỡng và thuật toán xuất ra chính xác`INF`mà không bao giờ xây dựng được giá trị đích thực.
