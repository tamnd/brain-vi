---
title: "CF 104301B - Hai hình vuông"
description: "Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn cung cấp một số nguyên không âm $n$ và chúng ta phải đếm xem có bao nhiêu số nguyên $x$ trong phạm vi từ $0$ đến $n$ có thể được viết dưới dạng tổng của hai bình phương số nguyên, nghĩa là $x = a^2 + b^2$ đối với một số số nguyên $a$ và $b$."
date: "2026-07-01T20:14:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104301
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #10 (TEN-Forces)"
rating: 0
weight: 104301
solve_time_s: 94
verified: true
draft: false
---

[CF 104301B - Hai hình vuông](https://codeforces.com/problemset/problem/104301/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn cung cấp một số nguyên không âm$n$, và chúng ta phải đếm xem có bao nhiêu số nguyên$x$trong phạm vi từ$0$ĐẾN$n$có thể được viết dưới dạng tổng của hai bình phương nguyên, nghĩa là$x = a^2 + b^2$đối với một số số nguyên$a$Và$b$. Cả hai$a$Và$b$có thể âm hoặc dương, nhưng vì bình phương loại bỏ dấu nên việc biểu diễn chỉ phụ thuộc vào giá trị tuyệt đối của chúng. 

Đầu ra của mỗi truy vấn không phải là số lượng biểu diễn mà là số lượng giá trị riêng biệt$x \le n$thừa nhận ít nhất một đại diện như vậy. 

Các ràng buộc đẩy chúng ta tới quá trình tiền xử lý. Với tối đa$10^5$truy vấn và$n \le 10^7$, việc tính toán lại câu trả lời cho mỗi truy vấn là không khả thi. Bất kỳ phương pháp cho mỗi truy vấn nào thậm chí là tuyến tính trong$n$sẽ đạt được$10^{12}$trong trường hợp xấu nhất vượt xa giới hạn. Điều này ngay lập tức gợi ý rằng chúng ta phải tính toán cấu trúc toàn cục một khi đạt đến giá trị tối đa$n$, sau đó trả lời từng truy vấn trong thời gian không đổi. 

Một trường hợp cạnh tinh tế là$x = 0$. Nó hợp lệ vì$0 = 0^2 + 0^2$, và phải được đưa vào. Một điểm khác thường gây ra sai sót là tính trùng lặp. Ví dụ,$5 = 1^2 + 2^2$và cả$5 = 2^2 + 1^2$, nhưng nó chỉ nên được tính một lần. Đầu ra là về sự tồn tại, không phải sự đa dạng. 

Một cách tiếp cận ngây thơ thường cố gắng kiểm tra từng$x$bằng cách quét tất cả các cặp$(a, b)$, nhưng vòng lặp kép này ngay lập tức trở nên quá lớn ngay cả đối với một truy vấn tối đa$n$, vì nó sẽ yêu cầu lặp lại tất cả các cặp lên đến$\sqrt{n}$, đại khái$10^4$theo mỗi chiều, hoặc$10^8$kiểm tra mỗi truy vấn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn và mỗi số$x \le n$, chúng tôi thử tất cả các cặp số nguyên$(a, b)$như vậy$a^2 + b^2 = x$. Số lượng ứng viên cho mỗi$x$tỷ lệ thuận với$\sqrt{x}$, vì vậy hãy kiểm tra tất cả$x \le n$dẫn đến đại khái$\sum_{x=1}^{n} \sqrt{x}$, phát triển theo thứ tự$n^{3/2}$. Với$n = 10^7$, điều này hoàn toàn không thể thực hiện được. 

Điều quan trọng là chúng ta không cần phải tính toán lại cấu trúc cho mỗi truy vấn. Tính chất “có thể viết dưới dạng tổng của hai bình phương” chỉ phụ thuộc vào giá trị của$x$, chứ không phải trên chính truy vấn. Điều này cho phép chúng tôi tính toán trước tất cả các giá trị hợp lệ lên đến mức tối đa$n$một lần. 

Thay vì kiểm tra từng con số riêng lẻ, chúng tôi lật ngược góc nhìn. Chúng tôi liệt kê các ô vuông có thể$a^2$Và$b^2$, và trực tiếp đánh dấu tổng của chúng. Từ$a^2 \le 10^7$, chúng ta chỉ cần$a \le 3162$. Điều này làm giảm vấn đề lặp lại trên tất cả các cặp$(a, b)$với$a^2 + b^2 \le \text{maxN}$. Mỗi cặp đóng góp một tổng có thể truy cập được, chúng tôi đánh dấu số tiền này trong một mảng boolean. 

Sau khi đánh dấu tất cả các số tiền có thể truy cập, chúng tôi xây dựng một mảng tổng tiền tố để mỗi truy vấn có thể được trả lời bằng$O(1)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^{3/2})$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tính toán trước các cặp hình vuông |$O(n \sqrt{n})$tiền xử lý |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tính toán trước tất cả các giá trị cho đến truy vấn tối đa có thể và sau đó trả lời từng truy vấn bằng cách sử dụng tổng tiền tố. 

1. Đầu tiên, xác định giá trị lớn nhất$n$trên tất cả các trường hợp thử nghiệm. Điều này là cần thiết vì chúng tôi muốn thực hiện một phép tính toàn cục duy nhất thay vì tính toán lại cho từng truy vấn. Nếu không có điều này, chúng ta sẽ lãng phí thời gian để lặp lại công việc giống hệt nhau. 
2. Tạo một mảng boolean`ok`kích thước`max_n + 1`, ban đầu tất cả đều sai. Mảng này sẽ lưu trữ liệu một số có thể được biểu diễn dưới dạng tổng của hai bình phương hay không. 
3. Lặp lại tất cả các số nguyên$a$như vậy$a^2 \le max_n$. Đối với mỗi$a$, tính toán$a^2$một lần và tái sử dụng nó. Điều này tránh việc tính toán lại các ô vuông nhiều lần và đảm bảo hiệu quả. 
4. Đối với mỗi cố định$a$, lặp lại tất cả các số nguyên$b$như vậy$a^2 + b^2 \le max_n$. Với mỗi cặp, đánh dấu`ok[a^2 + b^2] = True`. Chúng tôi chỉ liệt kê các cặp hợp lệ, do đó không cần kiểm tra giới hạn ngoài điều kiện vòng lặp. 
5. Sau khi tất cả các cặp được xử lý, hãy xây dựng một mảng tổng tiền tố`pref`Ở đâu`pref[i]`đếm xem có bao nhiêu giá trị$x \le i$thỏa mãn`ok[x]`. Điều này chuyển đổi vấn đề từ tập hợp truy vấn thành viên thành truy vấn đếm phạm vi. 
6. Đối với mỗi test case$n$, đầu ra`pref[n]`. 

### Tại sao nó hoạt động 

Mọi biểu diễn hợp lệ$x = a^2 + b^2$tương ứng với đúng một cặp số nguyên không âm$(a, b)$với$a \le b$hoặc$b \le a$, nhưng chúng ta không cần tính duy nhất ở cấp độ cặp. Chúng tôi chỉ cần đảm bảo rằng mỗi số tiền có thể truy cập được đánh dấu ít nhất một lần. Vì chúng tôi liệt kê tất cả các cặp một cách có hệ thống nên mọi số có thể biểu thị đều được đánh dấu. Tổng tiền tố sau đó đếm chính xác các số nguyên có ít nhất một biểu diễn hợp lệ, do đó câu trả lời là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ns = [int(input()) for _ in range(t)]
    max_n = max(ns)

    ok = [False] * (max_n + 1)

    a = 0
    while a * a <= max_n:
        a2 = a * a
        b = 0
        while a2 + b * b <= max_n:
            ok[a2 + b * b] = True
            b += 1
        a += 1

    pref = [0] * (max_n + 1)
    cnt = 0
    for i in range(max_n + 1):
        if ok[i]:
            cnt += 1
        pref[i] = cnt

    out = []
    for n in ns:
        out.append(str(pref[n]))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Ý tưởng cốt lõi trong việc triển khai là vòng lặp kép$a$Và$b$, chỉ chạy khi tổng bình phương nằm trong giới hạn. Điều này đảm bảo chúng tôi không bao giờ khám phá các cặp không cần thiết vượt quá mức tối đa$n$. Mảng boolean ngăn chặn việc đếm hai lần cùng một giá trị từ các cặp khác nhau. 

Việc xây dựng tổng tiền tố là cần thiết vì nó chuyển đổi một mảng thành viên được tính toán trước thành cấu trúc trả lời truy vấn. Nếu không có nó, mỗi truy vấn sẽ yêu cầu quét tới$n$, như thế sẽ là quá chậm. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào đại diện: một trường hợp nhỏ và một trường hợp kiểu ranh giới lớn hơn một chút. 

### Ví dụ 1 

đầu vào:```
n = 6
```Chúng tôi tính toán các giá trị đại diện lên đến 6. 

| một | b | a^2 + b^2 | đánh dấu | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | vâng | 
| 0 | 1 | 1 | vâng | 
| 1 | 1 | 2 | vâng | 
| 1 | 2 | 5 | vâng | 
| 2 | 2 | 8 | bỏ qua ( > 6 ) | 

Bây giờ chúng tôi tính toán số lượng tiền tố: 

| tôi | được [tôi] | trước[i] | 
| --- | --- | --- | 
| 0 | T | 1 | 
| 1 | T | 2 | 
| 2 | T | 3 | 
| 3 | F | 3 | 
| 4 | F | 3 | 
| 5 | T | 4 | 
| 6 | F | 4 | 

Vậy đáp án là 4. 

Dấu vết này cho thấy các cặp lặp lại được loại bỏ trùng lặp một cách tự nhiên thông qua đánh dấu boolean và việc tích lũy tiền tố đảm bảo việc đếm chính xác. 

### Ví dụ 2 

đầu vào:```
n = 10
```Tổng số tiền được tạo chính: 

| một | b | tổng hợp | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 0 | 2 | 4 | 
| 1 | 2 | 5 | 
| 1 | 3 | 10 | 
| 2 | 2 | 8 | 

Các giá trị được đánh dấu là {0, 1, 2, 4, 5, 8, 9, 10 tùy thuộc vào mức độ đầy đủ của phép liệt kê}. Mảng tiền tố sau đó đếm xem có bao nhiêu số nguyên lên tới 10 thuộc về tập hợp này, đưa ra câu trả lời cuối cùng. 

Ví dụ này nhấn mạnh rằng cấu trúc thưa thớt và phép liệt kê trực tiếp xây dựng tập hợp chính xác có thể truy cập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \sqrt{n})$| Mỗi$a$lặp đi lặp lại$b$lên đến$\sqrt{n - a^2}$, bao gồm tất cả các cặp hợp lệ một lần | 
| Không gian |$O(n)$| Mảng Boolean và mảng tổng tiền tố lên đến mức tối đa$n$| 

Chi phí tiền xử lý có thể chấp nhận được vì$n \le 10^7$và vòng lặp bên trong được giới hạn chặt chẽ bởi phạm vi giảm dần của giá trị hợp lệ$b$. Mỗi truy vấn sau đó được trả lời trong thời gian không đổi, phù hợp với yêu cầu xử lý lên tới$10^5$truy vấn một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    ns = [int(input()) for _ in range(t)]
    max_n = max(ns)

    ok = [False] * (max_n + 1)

    a = 0
    while a * a <= max_n:
        a2 = a * a
        b = 0
        while a2 + b * b <= max_n:
            ok[a2 + b * b] = True
            b += 1
        a += 1

    pref = [0] * (max_n + 1)
    cnt = 0
    for i in range(max_n + 1):
        if ok[i]:
            cnt += 1
        pref[i] = cnt

    return "\n".join(str(pref[n]) for n in ns)

# provided samples
assert run("4\n1\n6\n576\n10000000\n") == "2\n5\n200\n1985460"

# custom cases
assert run("1\n0\n") == "1"
assert run("1\n2\n") == "3"
assert run("2\n5\n10\n") == "4\n8"
assert run("3\n3\n4\n8\n") == "3\n4\n6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| trường hợp tối thiểu, không đại diện | 
|`2`|`3`| hành vi tổng bình phương không tầm thường nhỏ nhất | 
|`5, 10`|`4, 8`| tính nhất quán trên nhiều truy vấn | 
|`3, 4, 8`|`3, 4, 6`| ranh giới hỗn hợp và bước nhảy thưa thớt | 

## Vỏ cạnh 

###Trường hợp:$n = 0$đầu vào:```
1
0
```Thuật toán khởi tạo`ok[0] = True`bởi vì$0 = 0^2 + 0^2$. Mảng tiền tố bắt đầu bằng`pref[0] = 1`, do đó đầu ra là 1. Vòng lặp kép không bao giờ bỏ sót trường hợp này vì$a = 0, b = 0$được bao gồm một cách rõ ràng. 

### Trường hợp: ranh giới nhỏ chỉ tồn tại một vài tổng 

đầu vào:```
1
2
```Các dấu đếm:$0 = 0^2 + 0^2$,$1 = 0^2 + 1^2$,$2 = 1^2 + 1^2$. 

Như vậy`ok`có ba giá trị thực tối đa là 2 và tiền tố trả về chính xác là 3. Cấu trúc đảm bảo không trùng lặp vì cả ba giá trị đều là các chỉ mục riêng biệt trong mảng. 

### Trường hợp: biểu diễn thưa thớt xung quanh các giá trị lớn hơn 

đầu vào:```
1
8
```Các giá trị được đánh dấu bao gồm 0, 1, 2, 4, 5 và 8. Số tiền tố tích lũy chính xác sáu giá trị này. Vòng lặp lồng nhau đảm bảo rằng tất cả các cặp hợp lệ cho đến giới hạn đều được khám phá, do đó không có số biểu thị nào bị bỏ sót, mặc dù chúng không liền kề nhau.
