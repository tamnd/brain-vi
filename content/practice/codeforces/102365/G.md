---
title: "CF 102365G - Infinity Plus One"
description: "Một chương trình đếm mô tả khoảng cách chúng ta di chuyển trong một vũ trụ có trật tự cố định. Chương trình trống không thay đổi gì, + lấy phần tử không được sử dụng tiếp theo, phép nối chạy hai chương trình lần lượt và [P] lặp lại P vô số lần."
date: "2026-08-12T23:58:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102365
codeforces_index: "G"
codeforces_contest_name: "UBC Programming Contest 2019 (UBCPC 2019)"
rating: 0
weight: 102365
solve_time_s: 125
verified: true
draft: false
---

[CF 102365G - Infinity Plus One](https://codeforces.com/problemset/problem/102365/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một chương trình đếm mô tả khoảng cách chúng ta di chuyển trong một vũ trụ có trật tự cố định. Chương trình trống không thay đổi gì cả,`+`lấy phần tử không được sử dụng tiếp theo, phép nối chạy hai chương trình lần lượt và`[P]`lặp lại`P`vô số lần. Bắt đầu từ tập trống, mọi chương trình đều tạo ra một phân đoạn ban đầu của vũ trụ, do đó việc so sánh hai chương trình tương đương với việc so sánh vị trí của các phần tử đầu tiên mà chúng không sử dụng. 

Cách hữu ích để suy nghĩ về những phân đoạn ban đầu này là phân số thứ tự. Một chuỗi hữu hạn của`+`các phép toán tạo ra một số tự nhiên thông thường. Lặp đi lặp lại`+`vô hạn đưa ra thứ tự vô hạn đầu tiên, thường được viết là (\omega). Việc lặp lại một chương trình đại diện cho (\alpha) sẽ tạo ra (\alpha\cdot\omega) vô hạn, trong khi chạy hết chương trình này đến chương trình khác tương ứng với phép cộng thứ tự. 

Ví dụ,`[+]`đại diện cho (\omega),`[+]+`đại diện cho (\omega+1) và`[+][+]`đại diện cho (\omega\cdot2). biểu hiện`+++[+++]`đại diện cho (3+\omega=\omega), vì việc thêm một số hữu hạn trước thứ tự giới hạn không làm thay đổi kết quả. Đây chính xác là lý do tại sao trực giác số nguyên thông thường lại nguy hiểm ở đây. 

Đầu vào chứa tối đa 100 chương trình và mỗi chương trình có độ dài tối đa là 100. Nó đủ nhỏ để phân tích từng biểu thức một cách đệ quy và thực hiện các phép toán biểu tượng trên một biểu diễn có kích thước tỷ lệ với biểu thức. Khó khăn không phải là kích thước đầu vào mà là thực tế là một số chương trình mô tả các đối tượng thực sự vô hạn, do đó việc thực thi chúng một cách rõ ràng không bao giờ có thể là một giải pháp chung chính xác. 

Đầu ra là các chuỗi chương trình ban đầu được sắp xếp theo thứ tự mà chúng đại diện. Các chương trình biểu diễn cùng một thứ tự có thể xuất hiện theo bất kỳ thứ tự tương đối nào. 

Trường hợp cạnh đầu tiên là vòng lặp trống. Ví dụ:```
2
[]
+
```Đầu ra đúng là:```
[]
+
```

`[]`lặp lại chương trình trống mãi mãi, nhưng chương trình trống không bao giờ thêm bất cứ thứ gì, vì vậy nó đại diện cho số 0. Việc coi mọi cặp dấu ngoặc là thứ gì đó nâng cao số lượng sẽ đặt nó không chính xác sau`+`. 

Một trường hợp cạnh khác là công hữu hạn trước một vòng lặp vô hạn:```
2
+++
+++[+++]
```Đầu ra đúng là:```
+++
+++[+++]
```Chương trình đầu tiên đại diện cho (3), trong khi chương trình thứ hai đại diện cho (3+\omega=\omega). Một triển khai ngây thơ chỉ được tính là hiển thị`+`các ký tự có thể coi biểu thức thứ hai là lớn hơn ba bước hữu hạn một cách không chính xác, thiếu thực tế là vòng lặp đạt đến thứ tự giới hạn. 

Một trường hợp đặc biệt tế nhị là:```
3
[+]
+[+]
+[+]+
```Các giá trị lần lượt là (\omega,\omega,\omega+1), vì vậy hai giá trị đầu tiên bằng nhau và có thể xuất hiện theo một trong hai thứ tự, theo sau là`+[+]+`. Lý do`+[+]`không phải (\omega+1) phải không`[+]`chạy mãi mãi sau lần đầu tiên`+`và chuỗi vô hạn sẽ lấp đầy tất cả các phần tử sau phần tử ban đầu đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng các tập hợp do chương trình tạo ra. Đối với một dãy hữu hạn`+`hoạt động này thật dễ dàng, nhưng`[P]`đòi hỏi vô số lần lặp lại. Người ta có thể cắt ngắn mọi vòng lặp sau một số (K) lần lặp lại và so sánh các tiền tố hữu hạn thu được, nhưng không có (K) hữu hạn nào làm cho điều này trở nên chính xác. Các chương trình`[+]`Và`[+]+`chỉ khác nhau sau khi tất cả các số tự nhiên đã được tạo ra. Bất kỳ mô phỏng nào chỉ thực hiện nhiều thao tác hữu hạn đều coi cả hai là tiền tố hữu hạn giống nhau. 

Các vòng lặp lồng nhau thậm chí còn làm cho việc mở rộng rõ ràng trở nên tồi tệ hơn. Nếu biểu thức độ sâu-(d) được mở rộng cho các lần lặp lại (K) ở mọi cấp độ vòng lặp, thì việc mở rộng có thể yêu cầu thực thi (\Theta(K^d)). Với các chuỗi có độ dài 100, độ sâu lồng nhau có thể đạt tới 50, do đó, ngay cả (K=100) cũng sẽ đưa ra trường hợp xấu nhất về mặt hình thức xung quanh (100^{50}=10^{100}) hoạt động mở rộng. Về cơ bản hơn, không có hữu hạn cố định (K) nào đưa ra thuật toán chính xác vì bản thân hành vi giới hạn phải được biểu diễn một cách tượng trưng. 

Quan sát quan trọng là mọi tập hợp được tạo đều là phân đoạn ban đầu của thứ tự giếng, do đó, nó có thứ tự là loại thứ tự. Các phép toán khả dụng chính xác là phép tính kế thừa thứ tự, phép cộng thứ tự và phép nhân với (\omega). Tất cả các thứ tự được tạo ra bởi các hoạt động này có thể được lưu trữ ở dạng chuẩn Cantor. 

Mỗi thứ tự liên quan đều có một cách biểu diễn duy nhất 

[ 
\omega^{\beta_1}c_1+\omega^{\beta_2}c_2+\cdots+\omega^{\beta_k}c_k, 
] 

trong đó số mũ giảm nghiêm ngặt và mọi hệ số là số nguyên hữu hạn dương. Bản thân các số mũ là số thứ tự và có thể được biểu diễn đệ quy theo cách tương tự. 

Điều này mang lại cho các đối tượng tượng trưng rất nhỏ. MỘT`+`hoạt động làm tăng số hạng hữu hạn lên một. Ghép nối hai chương trình thực hiện phép cộng thứ tự. Nếu một số thứ tự khác 0 có số hạng đứng đầu (\omega^\beta c), thì việc nhân nó với (\omega) sẽ loại bỏ mọi thứ ngoại trừ số mũ đứng đầu và tạo ra (\omega^{\beta+1}). Như vậy`[P]`có thể được đánh giá mà không cần thực hiện dù chỉ một lần lặp vô hạn. 

Ý tưởng dùng vũ lực có hiệu quả vì việc thực thi một chương trình thực sự đã xây dựng được tiền tố thứ tự của nó. Nó thất bại vì không thể đạt được số thứ tự giới hạn bằng mô phỏng hữu hạn. Quan sát thấy rằng các chương trình này có chính xác đại số của số học thứ tự cho phép chúng ta thay thế việc thực thi vô hạn bằng thao tác hữu hạn của các dạng chuẩn Cantor. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn và không có giới hạn chính xác | Không giới hạn | Quá chậm và nói chung là không chính xác | 
| Tối ưu | (O(M\log M\cdot L^2)) | (O(ML)) | Đã chấp nhận | 

Ở đây (L\le100) là độ dài tối đa của chương trình và (M\le100). 

## Hướng dẫn thuật toán 

1. Biểu diễn một thứ tự bằng dạng chuẩn Cantor của nó dưới dạng một bộ số hạng. Mỗi thuật ngữ là`(exponent, coefficient)`, Ở đâu`exponent`là một biểu diễn thứ tự khác. Tuple trống đại diện cho số không. Ví dụ: (7) được biểu diễn dưới dạng một số hạng có số mũ bằng 0 và hệ số bảy, trong khi (\omega+3) có hai số hạng, một số có số mũ một và một số có số mũ bằng 0. 
2. Phân tích từng chương trình dưới dạng một chuỗi biểu thức cho đến khi tìm được kết quả phù hợp`]`hoặc cuối chuỗi. MỘT`+`đóng góp số thứ tự, trong khi biểu thức trong ngoặc được đánh giá đệ quy rồi nhân với (\omega). 
3. Thực hiện phép cộng thứ tự trong khi phân tích chuỗi. Nếu toán hạng bên phải bằng 0 thì không có gì thay đổi. Ngược lại, đặt số mũ đứng đầu của nó là (\beta). Trong toán hạng bên trái, mọi số hạng có số mũ nhỏ hơn (\beta) sẽ bị loại bỏ, vì số hạng ở cấp độ thấp hơn sẽ bị nuốt khi thêm vào thứ tự cấp độ lớn hơn. Nếu toán hạng bên trái có một số hạng có số mũ chính xác (\beta), thì hệ số của nó sẽ tăng lên bằng hệ số của toán hạng bên phải. Sau đó, các số hạng còn lại của toán hạng bên phải sẽ được thêm vào. 
4. Triển khai kế thừa riêng biệt cho`+`. Nếu thứ tự là hữu hạn, hãy tăng hệ số của nó. Nếu nó không có số hạng cuối cùng hữu hạn, hãy thêm một số hạng (\omega^0) mới với hệ số một. Sự khác biệt này quan trọng bởi vì`+`là một hoạt động kế tiếp, trong khi`[+]`là phép toán giới hạn tạo ra (\omega). 
5. Đối với`[P]`, đầu tiên đánh giá`P`. Nếu nó đại diện cho số 0 thì kết quả vẫn bằng 0. Nếu không, hãy đặt số hạng hàng đầu của nó là (\omega^\beta c). Lặp lại toàn bộ thứ tự (\alpha) vô số lần sẽ cho ra (\alpha\cdot\omega=\omega^{\beta+1}). Hệ số và mọi số hạng thấp hơn biến mất, do đó CNF thu được bao gồm chính xác một số hạng có số mũ (\beta+1) và hệ số một. 
6. Sử dụng so sánh từ điển tự nhiên của các bộ dữ liệu CNF. So sánh các số mũ dẫn đầu trước. Nếu chúng khác nhau thì thứ tự có số mũ đứng đầu lớn hơn sẽ lớn hơn. Nếu chúng bằng nhau thì so sánh hệ số của chúng. Nếu những cái đó cũng bằng nhau, hãy tiếp tục với số hạng tiếp theo. Tiền tố thích hợp sẽ nhỏ hơn bộ dữ liệu dài hơn. Vì số mũ được biểu diễn bằng cùng một cấu trúc đệ quy nên phép so sánh bộ dữ liệu của Python thực hiện chính xác phép so sánh này theo cách đệ quy. 
7. Lưu trữ từng chương trình gốc cùng với biểu diễn thứ tự chuẩn của nó, sau đó sắp xếp theo cách biểu diễn và in các chuỗi gốc. Sự biểu diễn bằng nhau tương ứng với các chương trình đếm bằng nhau nên thứ tự của chúng không bị hạn chế. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi phân tích cú pháp bất kỳ đoạn chương trình nào, bộ dữ liệu được lưu trữ của nó chính xác là dạng chuẩn Cantor của thứ tự được biểu thị bởi đoạn đó. 

MỘT`+`phép toán bảo toàn bất biến vì nó chính xác là thứ tự kế tiếp. Việc nối chuỗi sẽ bảo toàn nó vì phép toán được triển khai là định nghĩa của phép cộng thứ tự ở dạng chuẩn Cantor. Đối với một biểu thức trong ngoặc, phần thân biểu thị một số thứ tự (\alpha). Sự lặp lại vô hạn biểu thị (\alpha\cdot\omega) và với mọi khác không (\alpha), tích đó chính xác là (\omega^{\beta+1}), trong đó (\beta) là số mũ đứng đầu của (\alpha). Do đó, mỗi bước phân tích đệ quy đều tạo ra thứ tự chính xác. 

Cuối cùng, dạng chuẩn Cantor là duy nhất và thứ tự từ điển của nó là thứ tự thứ tự. Việc sắp xếp các biểu diễn chuẩn này sẽ sắp xếp các chương trình theo quan hệ được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

ZERO = ()
ONE = ((ZERO, 1),)

def succ(a):
    """Return a + 1."""
    if not a:
        return ONE

    terms = list(a)

    # If the last term is omega^0 * c, increase c.
    if terms[-1][0] == ZERO:
        exp, coeff = terms[-1]
        terms[-1] = (exp, coeff + 1)
    else:
        # Append omega^0.
        terms.append((ZERO, 1))

    return tuple(terms)

def add(a, b):
    """Return the ordinal sum a + b in CNF."""
    if not a:
        return b
    if not b:
        return a

    beta = b[0][0]

    # Keep terms of a whose exponent is strictly greater
    # than beta. If beta occurs, keep that term too.
    i = 0
    while i < len(a) and a[i][0] > beta:
        i += 1

    result = list(a[:i])

    if i < len(a) and a[i][0] == beta:
        result.append((beta, a[i][1] + b[0][1]))
        i += 1
    else:
        result.append(b[0])

    result.extend(b[1:])
    return tuple(result)

def loop(a):
    """Return a * omega."""
    if not a:
        return ZERO

    # If a starts with omega^beta * c, then
    # a * omega = omega^(beta + 1).
    beta = a[0][0]
    return ((succ(beta), 1),)

def parse_program(s):
    n = len(s)

    def parse(pos, closing):
        cur = ZERO

        while pos < n and (not closing or s[pos] != ']'):
            if s[pos] == '+':
                cur = succ(cur)
                pos += 1

            elif s[pos] == '[':
                inside, pos = parse(pos + 1, True)
                cur = add(cur, loop(inside))

            else:
                # The caller consumes matching ']'.
                break

        if closing and pos < n and s[pos] == ']':
            pos += 1

        return cur, pos

    value, _ = parse(0, False)
    return value

def solve():
    m = int(input())
    programs = [input().strip() for _ in range(m)]

    values = [(parse_program(p), p) for p in programs]
    values.sort(key=lambda x: x[0])

    sys.stdout.write("\n".join(p for _, p in values))

if __name__ == "__main__":
    solve()
```Việc biểu diễn sử dụng các bộ dữ liệu bất biến để một số thứ tự có thể chứa các số thứ tự khác một cách an toàn làm số mũ của nó. Điều này cũng cung cấp cho Python một thuộc tính hữu ích miễn phí: đẳng thức bộ dữ liệu và so sánh từ điển so sánh đệ quy cấu trúc CNF hoàn chỉnh.`succ`xử lý đuôi hữu hạn một cách cẩn thận. Nếu số mũ cuối cùng bằng 0 thì thứ tự đã có thành phần hữu hạn và hệ số của nó tăng lên. Nếu không thì số thứ tự không có đuôi hữu hạn, vì vậy số hạng kế thừa sẽ tạo ra một số hạng (\omega^0) mới.`add`tuân theo quy tắc CNF để cộng thứ tự. Giả sử số hạng đầu tiên của toán hạng bên phải là (\omega^\beta c). Mọi số hạng ở bên trái có số mũ bên dưới (\beta) đều biến mất. Một số hạng có số mũ bằng (\beta) vẫn tồn tại và hệ số của nó kết hợp với hệ số của toán hạng bên phải. Phần còn lại của toán hạng bên phải được sao chép không thay đổi. 

Sự so sánh`a[i][0] > beta`là hợp lệ vì bản thân số mũ là các bộ thứ tự chuẩn. Python so sánh đệ quy các bộ dữ liệu đó theo cùng một thứ tự. Không cần chuyển đổi sang số nguyên khổng lồ và không có khả năng tràn số nguyên.`loop`là phép toán vô hạn quan trọng. Nếu như`a`khác 0 và bắt đầu bằng (\omega^\beta c), sau đó 

[ 
a\cdot\omega=\omega^{\beta+1}. 
] 

Toàn bộ phần bậc thấp biến mất trong giới hạn. Ví dụ: ((\omega+1)\omega=\omega^2), trong khi (7\omega=\omega). 

Trình phân tích cú pháp sử dụng một biểu thức trong ngoặc hoàn chỉnh theo cách đệ quy. Dấu ngoặc trống tự nhiên tạo ra số 0 vì chuỗi đệ quy không chứa phép toán nào. Sau khi phân tích cơ thể,`loop`áp dụng ngữ nghĩa của các dấu ngoặc xung quanh và thứ tự kết quả được thêm vào mọi thứ được phân tích cú pháp trước nó. 

## Ví dụ đã hoạt động 

Có một mẫu chính thức, vì vậy dấu vết thứ hai bên dưới sử dụng đầu vào tùy chỉnh nhỏ hơn được thiết kế để hiển thị hành vi giới hạn. 

### Mẫu 1 

Các giá trị chuẩn có liên quan được hiển thị dưới đây. 

| Chương trình | Giá trị được phân tích | Giải thích CNF | 
| --- | --- | --- | 
|`[][[][]][]`| (0) | trống | 
|`+`| (1) | (1) | 
|`+++++++`| (7) | (7) | 
|`+++[+++]`| (\omega) | (3+\omega) | 
|`[+]+`| (\omega+1) | (\omega+1) | 
|`[+][+]`| (\omega\cdot2) | (\omega+\omega) | 
|`+[+[+]+]+`| (\omega^2+1) | (1+(\omega+1)\omega+1) | 
|`[+][[+]][+]`| (\omega^2+\omega) | (\omega+\omega^2+\omega) | 

Trình phân tích cú pháp đạt tới`+++[+++]`bằng cách xây dựng đầu tiên (3), sau đó đánh giá`+++`bên trong ngoặc là (3), chuyển phần trong ngoặc thành (3\omega=\omega). Cộng ba số đầu tiên sẽ được (3+\omega=\omega). 

Vì`+[+[+]+]+`, bên trong`+[+]+`đại diện cho (\omega+1). Vòng lặp của nó là ((\omega+1)\omega=\omega^2) và những vòng kế thừa xung quanh cho ra (\omega^2+1). biểu hiện`[+][[+]][+]`thay vào đó kết thúc bằng`[+]`, nối thêm toàn bộ bản sao của (\omega), tạo ra (\omega^2+\omega). Điều này giúp phân biệt hai biểu thức có thể trông giống nhau đến mức gây nhầm lẫn. 

### Ví dụ về giới hạn tùy chỉnh 

Hãy xem xét:```
5
+
[+]
+[+]
+[+]+
[+][+]
```Dấu vết phân tích cú pháp là: 

| Chương trình | Hoạt động đang được áp dụng | Thứ tự hiện tại | 
| --- | --- | --- | 
|`+`| người kế nhiệm (0) | (1) | 
|`[+]`| vòng lặp của (1) | (\omega) | 
|`+[+]`| kế nhiệm rồi vòng lặp | (1\cdot\omega=\omega) | 
|`+[+]+`| người kế nhiệm, vòng lặp, người kế nhiệm | (\omega+1) | 
|`[+][+]`| vòng lặp của (1), sau đó thêm (\omega) | (\omega\cdot2) | 

Do đó, đầu ra được sắp xếp là:```
+
[+]
+[+]
+[+]+
[+][+]
```Các giá trị bằng nhau`[+]`Và`+[+]`thực hiện sự phân biệt giữa sự kế tiếp và sự lặp lại vô hạn. Phần tử sau trước tiên tạo một phần tử, nhưng vòng lặp vô hạn sau sẽ lấp đầy tất cả các vị trí hữu hạn còn lại, do đó tiền tố hữu hạn ban đầu sẽ biến mất khi được xem dưới dạng tổng thứ tự với (\omega). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(M\log M\cdot L^2)) | Phân tích cú pháp một chương trình thực hiện tối đa (O(L)) phép cộng thứ tự, mỗi lần sao chép tối đa (O(L)) thuật ngữ CNF và sắp xếp thực hiện so sánh (O(M\log M)) | 
| Không gian | (O(ML)) | Cách biểu diễn chuẩn của mỗi chương trình có (O(L)) thuật ngữ được lưu trữ và dữ liệu số mũ lồng nhau | 

Với (M\le100) và (L\le100), dữ liệu đầu vào chứa tối đa 10.000 ký tự. Biểu diễn mang tính biểu tượng vẫn rất nhỏ so với bất kỳ nỗ lực nào nhằm hiện thực hóa các tập hợp vô hạn. Thuật toán chỉ thực hiện phân tích cú pháp đệ quy, các thao tác bộ dữ liệu ngắn và sắp xếp, do đó nó phù hợp thoải mái với giới hạn một giây và giới hạn bộ nhớ 1024 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

ZERO = ()
ONE = ((ZERO, 1),)

def succ(a):
    if not a:
        return ONE

    terms = list(a)
    if terms[-1][0] == ZERO:
        exp, coeff = terms[-1]
        terms[-1] = (exp, coeff + 1)
    else:
        terms.append((ZERO, 1))
    return tuple(terms)

def add(a, b):
    if not a:
        return b
    if not b:
        return a

    beta = b[0][0]
    i = 0

    while i < len(a) and a[i][0] > beta:
        i += 1

    result = list(a[:i])

    if i < len(a) and a[i][0] == beta:
        result.append((beta, a[i][1] + b[0][1]))
        i += 1
    else:
        result.append(b[0])

    result.extend(b[1:])
    return tuple(result)

def loop(a):
    if not a:
        return ZERO

    beta = a[0][0]
    return ((succ(beta), 1),)

def parse_program(s):
    n = len(s)

    def parse(pos, closing):
        cur = ZERO

        while pos < n and (not closing or s[pos] != ']'):
            if s[pos] == '+':
                cur = succ(cur)
                pos += 1
            elif s[pos] == '[':
                inside, pos = parse(pos + 1, True)
                cur = add(cur, loop(inside))
            else:
                break

        if closing and pos < n and s[pos] == ']':
            pos += 1

        return cur, pos

    return parse(0, False)[0]

def solve_io(inp):
    data = inp.splitlines()
    m = int(data[0])
    programs = data[1:1 + m]

    values = [(parse_program(p), p) for p in programs]
    values.sort(key=lambda x: x[0])

    return "\n".join(p for _, p in values) + "\n"

# Provided sample
sample1_in = """8
+
[+]+
[+][+]
+++++++
+++[+++]
+[+[+]+]+
[+][[+]][+]
[][[][]][]
"""

sample1_out = """[][[][]][]
+
+++++++
+++[+++]
[+]+
[+][+]
+[+[+]+]+
[+][[+]][+]
"""

assert solve_io(sample1_in) == sample1_out, "sample 1"

# Minimum-size program and zero-producing loops
assert solve_io("""3
[]
+
++
""") == """[]
+
++
""", "minimum-size programs"

# All values equal
assert solve_io("""5
[]
[][]
[][[]]
[][[][]][]
[][][][]
""") == """[]
[][]
[][[]]
[][[][]][]
[][][][]
""", "all equal"

# Boundary between finite values and omega
assert solve_io("""5
+++
+++[+++]
[+]
+[+]
+[+]+
""") == """+++
+++[+++]
[+]
+[+]
+[+]+
""", "finite versus limit ordinal"

# Maximum-size programs, all equal
long_program = "+" * 100
maximum_input = "100\n" + "\n".join([long_program] * 100) + "\n"
maximum_output = "\n".join([long_program] * 100) + "\n"

assert solve_io(maximum_input) == maximum_output, "maximum-size input"

# A useful equality: 1 + omega = omega
assert solve_io("""4
+[+]
[+]
+[+]+
++[+]
""") == """[+]
+[+]
++[+]
+[+]+
""", "ordinal addition and successor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[]`,`+`,`++`|`[]`,`+`,`++`| Các chương trình có kích thước tối thiểu và thực tế là vòng lặp trống bằng 0 | 
| Một số biểu thức tạo ra số 0 | Thứ tự đầu vào | Cho phép biểu diễn thứ tự bằng nhau theo bất kỳ thứ tự nào | 
|`+++`,`+++[+++]`,`[+]`,`+[+]`,`+[+]+`|`+++`,`+++[+++]`,`[+]`,`+[+]`,`+[+]+`| Ranh giới giữa các số thứ tự hữu hạn, (\omega) và (\omega+1) | 
| 100 chương trình dài 100 | Cùng 100 chương trình | Kích thước đầu vào tối đa và không có vấn đề về đệ quy hoặc kích thước số nguyên | 
|`[+]`,`+[+]`,`++[+]`,`+[+]+`|`[+]`,`+[+]`,`++[+]`,`+[+]+`| Phép cộng thứ tự có thể xóa các tiền tố hữu hạn, trong khi phép cộng cuối cùng vẫn thay đổi giá trị | 

## Vỏ cạnh 

Chương trình trống được biểu diễn bằng bộ CNF trống. Như vậy`[]`đánh giá phần thân trống rỗng của nó bằng 0,`loop(0)`trả về 0 và việc ghép nó với một biểu thức khác không có tác dụng. Vì`[][[][]][]`, mọi phần thân trong ngoặc đều trống hoặc chỉ bao gồm các chương trình trống, vì vậy giá trị cuối cùng vẫn bằng 0. Điều này xử lý các dấu ngoặc trống được lồng tùy ý mà không có bất kỳ trường hợp phân tích cú pháp đặc biệt nào ngoài kết quả trả về đệ quy thông thường. 

Đối với tiền tố hữu hạn được theo sau bởi một vòng lặp vô hạn, tiền tố hữu hạn không được coi là phần bù cộng thông thường khi vòng lặp tạo ra thứ tự giới hạn. TRONG`+++[+++]`, phần đầu tiên cho (3), trong khi phần trong ngoặc cho (3\cdot\omega=\omega). Thủ tục cộng nhận (3+\omega), thấy rằng toán hạng bên phải có số mũ đứng đầu (1) và loại bỏ số hạng số mũ bằng 0 của toán hạng bên trái. Kết quả chính xác là (\omega). 

biểu hiện`+[+]`thực hiện quy tắc tương tự ở dạng nhỏ hơn. đầu tiên`+`cho (1) và`[+]`đại diện cho (\omega). Phép cộng (1+\omega) trở thành (\omega), vì số mũ 0 ở bên trái bị loại bỏ. Một trình phân tích cú pháp dựa trên số học số nguyên thông thường sẽ giữ sai số đầu tiên. 

Sự khác biệt giữa`[+]`Và`[+]+`kiểm tra ranh giới đối diện.`[+]`tạo ra (\omega), trong khi cuối cùng`+`TRONG`[+]+`là người kế nhiệm thực sự, nên kết quả là (\omega+1). Trong sự đại diện,`[+]`là`((1, 1),)`, trong khi người kế nhiệm của nó thêm số hạng có số mũ bằng 0, tạo ra`((1, 1), (0, 1))`. 

Các vòng lặp lồng nhau kiểm tra xem số mũ đứng đầu có được cập nhật chính xác hay không. Vì`[[+]]`, bên trong`[+]`đại diện cho (\omega). Do đó, vòng lặp bên ngoài sẽ tính toán (\omega\cdot\omega=\omega^2). Trong CNF, số mũ đứng đầu của (\omega) là (1), vì vậy`loop`áp dụng`succ`theo số mũ đó và tạo ra số hạng duy nhất (\omega^2). Việc lặp lại cấu trúc này sẽ hỗ trợ một cách tự nhiên các biểu thức sâu sắc hơn như`[[[+]]]`, đại diện cho (\omega^3). 

Cuối cùng, các chương trình bình đẳng không được bị ép buộc theo một trật tự văn bản cụ thể. Những biểu hiện như`[+]`Và`+[+]`cả hai đều đại diện cho (\omega), mặc dù cú pháp của chúng khác nhau. Khóa sắp xếp là biểu diễn thứ tự chuẩn, do đó các giá trị bằng nhau sẽ được so sánh bằng nhau. Kiểu sắp xếp ổn định của Python duy trì thứ tự đầu vào của chúng, điều này được sự cố cho phép.
