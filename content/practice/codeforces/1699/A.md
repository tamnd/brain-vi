---
title: "CF 1699A - Bài toán ba số thứ ba"
description: "Chúng ta được yêu cầu xây dựng ba số nguyên $a$, $b$ và $c$, mỗi số nằm trong một phạm vi lớn lên đến $10^9$, sao cho một biểu thức cụ thể liên quan đến các giá trị XOR theo cặp khớp với một số mục tiêu nhất định $n$."
date: "2026-06-09T22:09:38+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1699
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 804 (Div. 2)"
rating: 800
weight: 1699
solve_time_s: 123
verified: false
draft: false
---

[CF 1699A - Bài toán ba số thứ ba](https://codeforces.com/problemset/problem/1699/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng ba số nguyên$a$,$b$, Và$c$, mỗi cái nằm trong một phạm vi lớn lên tới$10^9$, sao cho một biểu thức cụ thể liên quan đến các giá trị XOR theo cặp khớp với một số mục tiêu nhất định$n$. Biểu thức tính tổng XOR của mỗi cặp:$(a \oplus b) + (b \oplus c) + (a \oplus c)$. 

Khó khăn chính không phải là đánh giá XOR mà là việc chọn các giá trị làm cho ba XOR theo cặp này cộng lại chính xác.$n$, hoặc chứng minh rằng không có bộ ba nào như vậy tồn tại. 

Đầu vào chứa nhiều truy vấn độc lập. Đối với mỗi cái, chúng tôi xuất ra bất kỳ bộ ba hoặc bộ ba hợp lệ nào$-1$nếu không thể. Từ$n$có thể lớn như$10^9$và có tới$10^4$trường hợp thử nghiệm, chúng tôi cần xây dựng thời gian không đổi cho mỗi trường hợp. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Việc thử nhanh các giá trị nhỏ cho thấy biểu thức$(a \oplus b) + (b \oplus c) + (a \oplus c)$luôn tạo ra số chẵn. Ví dụ: nếu tất cả các bit được xem xét độc lập thì mỗi bit sẽ đóng góp 0 hoặc 2 vào tổng số. Điều này đã ngụ ý rằng kỳ quặc$n$không thể đại diện được. Một tìm kiếm ngây thơ sẽ bỏ lỡ hạn chế về cấu trúc này và cố gắng xây dựng không chính xác các giá trị lẻ. 

Một cạm bẫy khác là giả định rằng việc đặt hai biến bằng nhau hoặc sử dụng cấp số cộng đơn giản luôn có tác dụng. Mặc dù một số giá trị nhỏ có thể được vá theo cách thủ công, nhưng cần có một mẫu chung tôn trọng tính độc lập theo bit. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là thử tất cả các bộ ba$(a, b, c)$trong một phạm vi hợp lý và tính biểu thức. Ngay cả khi chúng ta giới hạn các giá trị ở một giới hạn nhỏ như vài nghìn, thì đây vẫn là khối trong trường hợp xấu nhất, dẫn đến hàng tỷ phép tính. Điều đó vượt xa những gì có thể thực hiện được. 

Quan sát quan trọng là biểu thức có thể được kiểm soát bằng cách chọn cẩn thận các mẫu bit thay vì tìm kiếm theo giá trị. XOR hoạt động độc lập trên từng bit, vì vậy chúng ta có thể giải quyết vấn đề từng chút một. 

Một đồng nhất thức quan trọng xuất hiện khi chúng ta xem xét các bộ ba có cấu trúc đơn giản. Nếu chúng ta sửa$a = 0$, biểu thức được đơn giản hóa thành:$$(a \oplus b) + (b \oplus c) + (a \oplus c) = b + (b \oplus c) + c$$Bây giờ chúng tôi muốn định hình các khoản đóng góp sao cho tổng của chúng bằng$n$. Thay vì giải quyết vấn đề này một cách trực tiếp, chúng ta cố gắng biểu diễn$n$là sự kết hợp của hai số có cấu trúc XOR đơn giản. 

Một cấu trúc tiêu chuẩn phù hợp với tất cả mọi người$n$là chia tay$n$thành hai nửa bằng nhau:$$n = 2x$$và chọn:$$a = x,\quad b = 0,\quad c = x$$Bây giờ hãy tính:$$a \oplus b = x,\quad b \oplus c = x,\quad a \oplus c = 0$$Vì vậy tổng trở thành$x + x + 0 = 2x = n$. 

Điều này hoạt động bất cứ khi nào$n$là chẵn. Đối với số lẻ$n$, đối số chẵn lẻ cho thấy không thể xảy ra, vì vậy chúng tôi xuất ra$-1$. 

Cách tiếp cận bạo lực không thành công vì nó bỏ qua cấu trúc trong XOR và xử lý vấn đề như tìm kiếm số. Giải pháp tối ưu hoạt động vì XOR tuyến tính hóa trên các bit, cho phép xây dựng trực tiếp với các đóng góp được kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^3)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Xây dựng tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$. Nếu như$n$là số lẻ, xuất ngay$-1$. Điều này xuất phát từ thực tế là biểu thức luôn có giá trị là số chẵn do tính đối xứng theo cặp của các đóng góp bit. 
2. Tính toán$x = n / 2$. Chúng tôi mong muốn phân phối tổng số tiền đóng góp một cách đồng đều cho hai kỳ hạn XOR bằng nhau. 
3. Xây dựng bộ ba$a = x$,$b = 0$,$c = x$. Lựa chọn này đảm bảo hai cặp XOR đóng góp chính xác$x$mỗi thứ trong khi thứ ba đóng góp bằng không. 
4. Xuất bộ ba. 

### Tại sao nó hoạt động 

Mỗi vị trí bit trong XOR đóng góp độc lập. Đối với bất kỳ bit nào ở đâu$a$Và$c$khớp, XOR của họ bằng 0. Từ$b = 0$, XOR với$b$bảo tồn các giá trị của$a$Và$c$. Điều này tạo ra chính xác hai đóng góp giống hệt nhau bằng$x$, tổng hợp thành$n$. Việc xây dựng đảm bảo tính không tiêu cực và nằm trong giới hạn vì$x \le 10^9 / 2$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    if n % 2 == 1:
        print(-1)
    else:
        x = n // 2
        print(x, 0, x)
```Giải pháp áp dụng trực tiếp cách xây dựng xuất phát ở trên. Kiểm tra tính chẵn lẻ xử lý các trường hợp không thể thực hiện được trước tiên. Một lần$n$được xác nhận là chẵn, chia cho hai sẽ cho giá trị chính xác được sử dụng trong cấu trúc đối xứng. 

Sự lựa chọn$b = 0$rất quan trọng vì nó bảo toàn các giá trị trong XOR, tránh mọi sự hủy bỏ ngoài ý muốn giữa các bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 4$Chúng tôi tính toán$x = 2$và xây dựng$a = 2$,$b = 0$,$c = 2$. 

| Bước | một | b | c | a⊕b | b⊕c | a⊕c | Tổng hợp | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 0 | 2 | - | - | - | - | 
| Tính toán XOR | 2 | 0 | 2 | 2 | 2 | 0 | 4 | 

Điều này xác nhận việc xây dựng tạo ra số tiền cần thiết. 

### Ví dụ 2:$n = 1$Từ$n$thật kỳ lạ, chúng tôi ngay lập tức xuất ra$-1$. Không có bộ ba nào có thể tồn tại vì tổng XOR phải luôn chẵn. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| Kiểm tra tính chẵn lẻ |$1 \bmod 2 = 1$| từ chối | 

Điều này chứng tỏ tình trạng bất khả thi được xử lý mà không cần nỗ lực thi công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi bài kiểm tra | Mỗi bài kiểm tra chỉ bao gồm việc kiểm tra tính chẵn lẻ và phép chia | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Giải pháp dễ dàng phù hợp trong giới hạn ngay cả đối với$10^4$các trường hợp thử nghiệm vì mỗi trường hợp được giải quyết bằng các thao tác liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if n % 2 == 1:
            out.append("-1")
        else:
            x = n // 2
            out.append(f"{x} 0 {x}")
    return "\n".join(out)

# provided samples
assert run("5\n4\n1\n12\n2046\n194723326\n") == "2 0 2\n-1\n6 0 6\n1023 0 1023\n97361663 0 97361663"

# custom cases
assert run("1\n2\n") == "1 0 1", "minimum even"
assert run("1\n1\n") == "-1", "minimum odd"
assert run("1\n0\n") == "0 0 0", "zero edge"
assert run("1\n1000000000\n") == "500000000 0 500000000", "large even"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 0 1 | trường hợp chẵn hợp lệ nhỏ nhất | 
| 1 | -1 | trường hợp lẻ không hợp lệ nhỏ nhất | 
| 0 | 0 0 0 | hành vi không có ranh giới | 
| 1000000000 | 500000000 0 500000000 | xây dựng giới hạn trên | 

## Vỏ cạnh 

cho$n = 1$, thuật toán sẽ bác bỏ ngay lập tức do tính chẵn lẻ lẻ. Đầu vào kích hoạt kiểm tra$n \bmod 2 = 1$, và không có tính toán nào xảy ra nữa, tạo ra$-1$. 

Vì$n = 0$, nếu được xem xét, kết quả xây dựng tương tự$x = 0$, dẫn đến$(0, 0, 0)$và tổng XOR là$0$, đó là nhất quán. 

Đối với các giá trị chẵn lớn như$n = 10^9$, bộ tính toán$x = 5 \cdot 10^8$. Tất cả các giá trị vẫn nằm trong giới hạn và cấu trúc XOR không bị ảnh hưởng bởi độ lớn vì chỉ có mẫu bit mới quan trọng.
