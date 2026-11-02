---
title: "CF 1089J - Giảm thiểu JS"
description: "Chúng tôi được cung cấp một tệp nguồn ngôn ngữ lập trình nhỏ cùng với một bộ mã thông báo dành riêng. Nguồn ban đầu có thể chứa các nhận xét, khoảng trắng tùy ý và mã định danh do người dùng xác định."
date: "2026-06-13T03:46:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1089
codeforces_index: "J"
codeforces_contest_name: "2018-2019 ICPC, NEERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3200
weight: 1089
solve_time_s: 364
verified: false
draft: false
---

[CF 1089J - Giảm thiểu JS](https://codeforces.com/problemset/problem/1089/J) 

**Đánh giá:** 3200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 6 phút 4 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tệp nguồn ngôn ngữ lập trình nhỏ cùng với một bộ mã thông báo dành riêng. Nguồn ban đầu có thể chứa các nhận xét, khoảng trắng tùy ý và mã định danh do người dùng xác định. Trình phân tích cú pháp sẽ xóa nhận xét, bỏ qua dấu cách và liên tục trích xuất mã thông báo hợp lệ dài nhất theo quy tắc ngôn ngữ. 

Nhiệm vụ có hai phần riêng biệt. 

Đầu tiên, chúng ta phải xây dựng lại chuỗi mã thông báo chính xác được đại diện bởi nguồn. Trong quá trình này, các nhận xét sẽ biến mất và khoảng cách trở nên không liên quan. 

Thứ hai, mọi từ do người dùng định nghĩa phải được đổi tên theo sơ đồ xác định. Từ riêng biệt đầu tiên gặp phải sẽ trở thành mã định danh chữ thường khả dụng đầu tiên, từ riêng biệt thứ hai trở thành mã định danh chữ thường khả dụng thứ hai, v.v. Mã thông báo dành riêng được loại trừ khỏi chuỗi mã định danh được tạo này. 

Sau khi đổi tên, chúng ta phải in một dòng có mã thông báo tạo ra chính xác chuỗi mã thông báo đã đổi tên. Trong số tất cả các đầu ra như vậy, chúng ta cần một đầu ra có số lượng khoảng trống tối thiểu có thể. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Có tối đa 40 mã thông báo dành riêng, tối đa 40 dòng nguồn và mỗi dòng có độ dài tối đa 80. Toàn bộ dữ liệu đầu vào chỉ có vài nghìn ký tự. Điều này có nghĩa là chúng ta không cần máy móc phân tích cú pháp phức tạp. Ngay cả những thuật toán có độ phức tạp tỷ lệ thuận với bình phương độ dài nguồn cũng hoàn toàn an toàn. 

Phần khó khăn không phải là hiệu quả. Thử thách ở đây là tái tạo chính xác hành vi khớp dài nhất của trình phân tích cú pháp và xác định chính xác thời điểm cần có khoảng trắng giữa các mã thông báo đầu ra liền kề. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét các mã thông báo dành riêng là tiền tố của các mã thông báo dành riêng khác.```
Reserved: + ++
Source: ++
```Trình phân tích cú pháp phải tạo mã thông báo`++`, không phải hai`+`mã thông báo. Trình phân tích cú pháp chấp nhận mã thông báo dành riêng phù hợp đầu tiên thay vì mã thông báo khớp dài nhất sẽ tạo ra chuỗi mã thông báo sai. 

Hãy xem xét các mã thông báo dành riêng cũng đáp ứng định nghĩa của một từ.```
Reserved: while
Source: while abc
```Mã thông báo`while`được bảo lưu và không bao giờ được đổi tên. Việc coi mọi chuỗi chữ và số là một từ sẽ đổi tên nó một cách không chính xác. 

Hãy xem xét các mã thông báo liền kề hợp nhất thành một từ lớn hơn.```
Tokens: a b
```In ấn`ab`không có khoảng trắng sẽ thay đổi kết quả phân tích cú pháp từ hai từ thành một từ. Cần ít nhất một khoảng trống. 

Tình trạng ngược lại cũng xảy ra.```
Tokens: a +
```In ấn`a+`là hoàn toàn an toàn. Thêm một khoảng trắng sẽ chỉ làm cho kết quả dài hơn. 

Một trường hợp đặc biệt khó phát hiện là khi hai mã thông báo dành riêng ghép nối thành một mã thông báo dành riêng khác.```
Reserved: + ++
Tokens: + +
```In ấn`++`sẽ được phân tích cú pháp dưới dạng đơn`++`mã thông báo vì trình phân tích cú pháp luôn lấy mã thông báo dài nhất có thể. Dấu cách là bắt buộc mặc dù mã thông báo không phải là từ hoặc số. 

Hiểu chính xác khi nào các mã thông báo liền kề có thể được viết mà không có dấu phân cách là cốt lõi của vấn đề. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất hữu ích để hiểu vấn đề tối ưu hóa. 

Sau khi nhận được chuỗi mã thông báo đã đổi tên, giả sử có`k`mã thông báo. Giữa mỗi cặp liền kề, chúng ta có thể chèn khoảng trắng hoặc không. có`2^(k-1)`những kết quả đầu ra có thể. Đối với mỗi đầu ra ứng viên, chúng ta có thể chạy lại trình phân tích cú pháp và kiểm tra xem nó có tái tạo lại chuỗi mã thông báo mong muốn hay không. Câu trả lời hợp lệ ngắn nhất sẽ là câu trả lời. 

Cách tiếp cận này đúng vì nó kiểm tra rõ ràng mọi vị trí có thể có của khoảng trắng. Thật không may, thậm chí chỉ vài chục mã thông báo đã khiến không gian tìm kiếm trở nên phi thường. 

Quan sát quan trọng là quyết định giữa hai mã thông báo lân cận hoàn toàn mang tính cục bộ. 

Giả sử chúng ta đã đổi tên tất cả các số nhận dạng. Xem xét hai mã thông báo đầu ra liên tiếp`A`Và`B`. 

Nếu viết`A+B`làm cho trình phân tích cú pháp tạo ra chính xác hai mã thông báo giống nhau, thì việc bỏ khoảng trắng luôn tốt hơn vì khoảng trắng chỉ làm tăng độ dài. 

Nếu viết`A+B`khiến trình phân tích cú pháp tạo ra bất kỳ thứ gì khác, thì bắt buộc phải có khoảng trắng. 

Không có sự tương tác với các vị trí khác. Liệu chúng ta có cần dấu phân cách giữa các mã thông báo hay không`i`Và`i+1`chỉ phụ thuộc vào hai mã thông báo đó. 

Điều này làm giảm vấn đề thành hai nhiệm vụ độc lập. 

Đầu tiên, khôi phục và đổi tên chuỗi mã thông báo. 

Thứ hai, đối với mỗi cặp liền kề, hãy xác định xem phép nối có làm thay đổi kết quả phân tích cú pháp hay không. 

Vì kích thước đầu vào rất nhỏ nên chúng ta chỉ cần sử dụng lại chính trình phân tích cú pháp. Đối với một cặp`(A,B)`, phân tích chuỗi`A+B`. Nếu kết quả chính xác`[A,B]`, không cần không gian. Nếu không, hãy chèn một khoảng trắng. 

Trình phân tích cú pháp phù hợp dài nhất tương tự trích xuất mã thông báo từ nguồn ban đầu cũng trả lời câu hỏi về khoảng cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k · k) | O(k) | Quá chậm | 
| Tối ưu | O(T + k · L²) | O(T) | Đã chấp nhận | 

Đây`T`là tổng chiều dài nguồn và`L`là độ dài mã thông báo tối đa. Vì tất cả các giới hạn đều rất nhỏ nên điều này dễ dàng đủ nhanh. 

## Hướng dẫn thuật toán 

### Mô hình phân tích cú pháp 

Trình phân tích cú pháp phải khớp chính xác với câu lệnh. 

Ở bất kỳ vị trí nào: 

1. Bỏ qua dấu cách. 
2. Trong số tất cả các mã thông báo dành riêng bắt đầu từ vị trí hiện tại, hãy lấy mã dài nhất nếu có. 
3. Mặt khác, nếu ký tự hiện tại là một chữ số, hãy sử dụng chuỗi chữ số dài nhất và tạo mã thông báo số. 
4. Nếu không, hãy sử dụng từ hợp lệ dài nhất và tạo mã thông báo từ. 

Đầu vào được đảm bảo là hợp lệ. 

### Tạo từ 

Chuỗi định danh mục tiêu bao gồm các từ viết thường được sắp xếp trước theo độ dài và sau đó theo từ điển:```
a, b, ..., z, aa, ab, ...
```Bất kỳ mã định danh nào mà bản thân nó là mã thông báo dành riêng đều phải được bỏ qua. 

### Đổi tên 

Trong khi duyệt qua chuỗi mã thông báo được phân tích cú pháp: 

1. Nếu mã thông báo không phải là một từ, hãy giữ nguyên nó. 
2. Nếu mã thông báo là mã thông báo dành riêng, hãy giữ nguyên mã thông báo đó. 
3. Mặt khác, nếu từ này chưa xuất hiện trước đó, hãy gán mã định danh được tạo sẵn tiếp theo. 
4. Thay thế mỗi lần xuất hiện của từ gốc đó bằng mã định danh được chỉ định của nó. 

### Khoảng cách tối thiểu 

Đối với mỗi cặp mã thông báo được đổi tên liền kề`(A,B)`: 

1. Tạo thành chuỗi`A+B`. 
2. Phân tích chuỗi này bằng cách sử dụng cùng một mã thông báo. 
3. Nếu kết quả đúng là dãy hai mã thông báo`[A,B]`, nối thêm`B`trực tiếp sau`A`. 
4. Nếu không thì chèn một khoảng trắng trước`B`. 

Một khoảng trắng luôn là đủ vì các khoảng trắng được bỏ qua trước khi mã hóa. 

### Tại sao nó hoạt động 

Trình phân tích cú pháp có tính xác định và luôn chọn mã thông báo hợp lệ dài nhất ở mỗi vị trí. 

Sau khi đổi tên, chuỗi mã thông báo mục tiêu sẽ được cố định. Sự tự do duy nhất còn lại là nơi đặt không gian. 

Đối với bất kỳ cặp liền kề nào, việc bỏ qua dấu phân cách là hợp lệ chính xác khi phân tích cú pháp nối tái tạo hai mã thông báo đó. Nếu không, thì không có đầu ra nào không có dấu phân cách có thể giữ nguyên chuỗi mã thông báo vì trình phân tích cú pháp sẽ diễn giải ranh giới theo cách khác. 

Vì mọi quyết định về không gian chỉ phụ thuộc vào một cặp liền kề và các khoảng trắng không bao giờ giảm độ dài nên chiến lược tối ưu là bỏ qua một khoảng trắng bất cứ khi nào có thể và chỉ chèn một khoảng trắng khi cần thiết. Áp dụng quy tắc này một cách độc lập cho mọi ranh giới sẽ mang lại tổng số khoảng trắng tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

WORD_CHARS = set(
    "abcdefghijklmnopqrstuvwxyz"
    "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    "0123456789_$"
)

def is_word_start(c):
    return c.isalpha() or c == '_' or c == '$'

def tokenize(text, reserved):
    tokens = []
    i = 0
    n = len(text)

    while i < n:
        while i < n and text[i] == ' ':
            i += 1
        if i >= n:
            break

        best = None
        best_len = 0

        for tok in reserved:
            if text.startswith(tok, i) and len(tok) > best_len:
                best = tok
                best_len = len(tok)

        if best is not None:
            tokens.append(best)
            i += best_len
            continue

        c = text[i]

        if c.isdigit():
            j = i
            while j < n and text[j].isdigit():
                j += 1
            tokens.append(text[i:j])
            i = j
            continue

        j = i
        while j < n and text[j] in WORD_CHARS:
            j += 1
        tokens.append(text[i:j])
        i = j

    return tokens

def is_word_token(token, reserved):
    if token in reserved:
        return False
    if not token:
        return False
    if token[0].isdigit():
        return False
    return all(ch in WORD_CHARS for ch in token)

def generate_names(reserved):
    length = 1

    while True:
        total = 26 ** length

        for x in range(total):
            cur = []
            y = x

            for _ in range(length):
                cur.append(chr(ord('a') + (y % 26)))
                y //= 26

            name = ''.join(reversed(cur))

            if name not in reserved:
                yield name

        length += 1

def solve():
    n = int(input())

    reserved = set()
    if n:
        reserved = set(input().split())
    else:
        input()

    m = int(input())

    source_tokens = []

    for _ in range(m):
        line = input().rstrip('\n')

        pos = line.find('#')
        if pos != -1:
            line = line[:pos]

        source_tokens.extend(tokenize(line, reserved))

    gen = generate_names(reserved)

    mapping = {}
    renamed = []

    for tok in source_tokens:
        if is_word_token(tok, reserved):
            if tok not in mapping:
                mapping[tok] = next(gen)
            renamed.append(mapping[tok])
        else:
            renamed.append(tok)

    if not renamed:
        print()
        return

    answer = [renamed[0]]

    for i in range(1, len(renamed)):
        a = renamed[i - 1]
        b = renamed[i]

        merged = a + b
        parsed = tokenize(merged, reserved)

        if len(parsed) == 2 and parsed[0] == a and parsed[1] == b:
            answer.append(b)
        else:
            answer.append(' ')
            answer.append(b)

    print(''.join(answer))

if __name__ == "__main__":
    solve()
```Tokenizer là thành phần trung tâm. Nó được sử dụng hai lần: một lần để khôi phục chuỗi mã thông báo ban đầu và sau đó để kiểm tra xem hai mã thông báo đầu ra lân cận có thể được nối một cách an toàn hay không. 

Quy tắc mã thông báo được đặt trước lâu nhất phải được triển khai trước khi nhận dạng số và từ. Điều này phù hợp với quy tắc ưu tiên của câu lệnh. Ví dụ, nếu`while`được bảo lưu, chuỗi`while`phải trở thành mã thông báo dành riêng chứ không phải là một từ. 

Trình tạo mã định danh liệt kê các chuỗi chữ thường bằng cách tăng độ dài và thứ tự từ điển. Bất kỳ tên nào được tạo ra thuộc về tập hợp dành riêng đều bị bỏ qua. 

Giai đoạn giãn cách có chủ ý sử dụng lại trình phân tích cú pháp thay vì cố gắng rút ra hàng tá trường hợp đặc biệt. Điều này tránh những sai sót liên quan đến sự chồng chéo mã thông báo dành riêng, chẳng hạn như`+`so với`++`hoặc số nhận dạng liền kề với số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào từ tuyên bố. 

Các từ riêng biệt đầu tiên của người dùng xuất hiện theo thứ tự sau: 

| Từ gốc | Tên được gán | 
| --- | --- | 
| xơ | một | 
| số | b | 
| return_value | c | 
| trước | d | 
| tạm thời | e | 

Một phần của chuỗi mã thông báo được đổi tên trở thành: 

| Chỉ số mã thông báo | Mã thông báo | 
| --- | --- | 
| 1 | vui vẻ | 
| 2 | một | 
| 3 | ( | 
| 4 | b | 
| 5 | ) | 
| 6 | { | 
| 7 | var | 
| 8 | c | 

Bây giờ hãy xem xét một số cặp liền kề. 

| A | B | Nối | Kết quả được phân tích | Không gian cần thiết | 
| --- | --- | --- | --- | --- | 
| vui vẻ | một | niềm vui | [funa] | Có | 
| một | ( | a( | [a, (] | Không | 
| c | = | c= | [c, =] | Không | 
| + | d | +d | [+, d] | Không | 

Đầu ra cuối cùng là:```
fun a(b){var c=1,d=0,e;while(b>0){e=c;c=c+d;d=e;b--;}return c;}
```Ví dụ này chứng minh rằng hầu hết các dấu phân cách đều biến mất, trong khi các khoảng trắng chỉ tồn tại khi phép nối sẽ hợp nhất các mã thông báo. 

### Ví dụ 2 

Ví dụ được xây dựng:```
Reserved:
+
++

Source:
x + +
```Đổi tên mang lại:```
a + +
```Phân tích ranh giới: 

| A | B | Nối | Kết quả được phân tích | Không gian cần thiết | 
| --- | --- | --- | --- | --- | 
| một | + | một+ | [a, +] | Không | 
| + | + | ++ | [++] | Có | 

Đầu ra cuối cùng:```
a+ +
```Nếu không có khoảng trống được chèn vào, trình phân tích cú pháp sẽ thấy một`++`mã thông báo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T + k · L²) | Phân tích nguồn cộng với phân tích lại từng nối liền kề | 
| Không gian | O(T) | Lưu trữ mã thông báo, ánh xạ và đầu ra | 

Tổng kích thước nguồn chỉ có vài nghìn ký tự. Ngay cả việc lặp đi lặp lại việc phân tích lại các cặp mã thông báo được nối cũng không đáng kể. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    # insert solution implementation here
    pass

# sample 1
assert run("""16
fun while return var { } ( ) , ; > = + ++ - --
9
fun fib(num) { # compute fibs
  var return_value = 1, prev = 0, temp;
  while (num > 0) {
    temp = return_value; return_value = return_value + prev;
    prev = temp;
    num--;
  }
  return return_value;
}
""").strip() == "fun a(b){var c=1,d=0,e;while(b>0){e=c;c=c+d;d=e;b--;}return c;}"

# minimum case
assert run("""0

1
abc
""").strip() == "a"

# repeated identifier
assert run("""0

1
foo foo foo
""").strip() == "a a a"

# reserved token overlapping another reserved token
assert run("""2
+ ++
1
x + +
""").strip() == "a+ +"

# identifier next to number
assert run("""0

1
x 123
""").strip() == "a 123"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mã định danh duy nhất |`a`| Chương trình hợp lệ nhỏ nhất | 
|`foo foo foo`|`a a a`| Đổi tên nhất quán các từ lặp đi lặp lại | 
|`x + +`với`+`Và`++`dành riêng |`a+ +`| Sự mơ hồ về mã thông báo dành riêng phù hợp dài nhất | 
|`x 123`|`a 123`| Ranh giới số từ cần có dấu phân cách | 

## Vỏ cạnh 

Hãy xem xét các từ dành riêng trông giống như số nhận dạng.```
Reserved:
while

Source:
while x
```Trình phân loại mã thông báo`while`như được bảo lưu trước khi xem xét quy tắc từ. Nó vẫn không thay đổi. Chỉ một`x`được đổi tên thành`a`. Đầu ra trở thành:```
while a
```Giải pháp đổi tên mọi mã thông báo có hình dạng định danh sẽ tạo ra sai`a b`. 

Hãy xem xét hai từ trở thành một từ sau khi nối.```
Source:
foo bar
```Sau khi đổi tên:```
a b
```Sự nối`ab`phân tích cú pháp dưới dạng một mã thông báo từ duy nhất. Việc phân tích lại phát hiện điều này vì kết quả là`[ab]`thay vì`[a, b]`. Một khoảng trắng được chèn vào, cho kết quả đầu ra chính xác`a b`. 

Xem xét các toán tử chồng chéo.```
Reserved:
+
++

Source:
+ +
```Sự nối`++`phân tích cú pháp dưới dạng một mã thông báo dành riêng. Trả về`[++]`, do đó một khoảng trắng được chèn vào. Đầu ra vẫn còn`+ +`, giữ nguyên chuỗi mã thông báo gốc. 

Hãy xem xét một từ theo sau là một số.```
Source:
abc 123
```Sau khi đổi tên:```
a 123
```Sự nối`a123`là một mã thông báo từ đơn vì các chữ số có thể xuất hiện sau ký tự đầu tiên của từ. Việc phân tích lại sẽ nắm bắt được điều này, vì vậy thuật toán sẽ chèn dấu phân cách và xuất ra`a 123`. Điều này bảo tồn ranh giới mã thông báo dự định.
