---
title: "CF 102309A - APA của Orz Pandas"
description: "Chúng ta được cung cấp các biểu thức số học C++ thông thường có toán hạng là các mã định danh chỉ được tạo từ các chữ cái tiếng Anh. Các toán tử là nhị phân +, -, , / và % và dấu ngoặc đơn có thể thay đổi thứ tự đánh giá."
date: "2026-08-13T23:51:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "A"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 71
verified: true
draft: false
---

[CF 102309A - APA của Orz Pandas](https://codeforces.com/problemset/problem/102309/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp các biểu thức số học C++ thông thường có toán hạng là các mã định danh chỉ được tạo từ các chữ cái tiếng Anh. Các toán tử là nhị phân`+`,`-`,`*`,`/`, Và`%`và dấu ngoặc đơn có thể thay đổi thứ tự đánh giá. Đối với mỗi dòng đầu vào, chúng ta phải tạo ra Java tương đương`BigInteger`sự biểu lộ. 

Bản dịch trung tâm là toán tử trở thành lệnh gọi phương thức trên toán hạng bên trái của nó. Như vậy`a+b`trở thành`a.add(b)`,`a-b`trở thành`a.subtract(b)`,`a*b`trở thành`a.multiply(b)`,`a/b`trở thành`a.divide(b)`, Và`a%b`trở thành`a.remainder(b)`. Các dấu ngoặc đơn được thể hiện một cách tự nhiên bằng các lệnh gọi phương thức lồng nhau, vì vậy chúng không được sao chép vào đầu ra trừ khi cú pháp Java thực sự yêu cầu chúng. Các ví dụ chính thức xác nhận rằng`(a+b)+c`trở thành`a.add(b).add(c)`, trong khi`a+(b+c)`trở thành`a.add(b.add(c))`. 

Đầu vào bao gồm nhiều biểu thức độc lập, mỗi biểu thức trên một dòng và mỗi biểu thức có độ dài tối đa là 1000. Điều này đủ nhỏ để thậm chí có thể quản lý được thao tác chuỗi bậc hai, nhưng trình phân tích cú pháp tuyến tính cũng tự nhiên và tránh được công việc lặp lại không cần thiết. Không có số học nào để thực hiện nên việc tràn số nguyên là không liên quan. Sự phức tạp chính xuất phát từ việc tôn trọng quyền ưu tiên thông thường của phép nhân, phép chia và số dư so với phép cộng và phép trừ, cùng với các dấu ngoặc đơn rõ ràng. 

Trường hợp cạnh đầu tiên là toán hạng bên phải được đóng ngoặc đơn. Vì`a+(b+c)`, đầu ra đúng là`a.add(b.add(c))`. Việc triển khai bất cẩn chỉ dịch các toán tử từ trái sang phải có thể tạo ra`a.add(b).add(c)`, làm thay đổi cây biểu thức. 

Trường hợp cạnh thứ hai là toán hạng bên trái được nhóm rõ ràng. Vì`(a+b)+c`, đầu ra đúng là`a.add(b).add(c)`. Các dấu ngoặc đơn bên ngoài không cần phải xuất hiện trong Java vì`a.add(b)`đã là một biểu thức hoàn chỉnh có thể đóng vai trò là người nhận lệnh gọi phương thức khác. Giữ mọi dấu ngoặc đơn đầu vào sẽ tạo ra cú pháp không cần thiết không chính xác như`(a.add(b)).add(c)`. 

Trường hợp cạnh thứ ba là ưu tiên của toán tử. Vì`a+b*c`, đầu ra đúng là`a.add(b.multiply(c))`, không`a.add(b).multiply(c)`. Trình phân tích cú pháp phải xây dựng cùng một cây mà C++ sẽ xây dựng trước khi dịch cây đó thành các lệnh gọi phương thức. 

Trường hợp cạnh thứ tư là phép trừ hoặc phép chia không liên kết. Vì`a-(b-c)`, đầu ra đúng là`a.subtract(b.subtract(c))`. Liên kết lại nó như`a.subtract(b).subtract(c)`sẽ đại diện`(a-b)-c`, đó là một biểu thức khác. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về phân tích cú pháp là thử mọi dấu ngoặc đơn nhị phân có thể có của toán hạng và tìm một dấu ngoặc đơn tương thích với thứ tự ưu tiên và dấu ngoặc đơn của C++. Với`k`toán hạng, số dấu ngoặc đơn nhị phân đầy đủ là số Catalan`C(k-1)`. Đối với 500 toán hạng, điều này đã có thể thực hiện được trong biểu thức 1000 ký tự, chẳng hạn như`a+a+a+...`, nó có kích thước lớn về mặt thiên văn nên cách tiếp cận như vậy hoàn toàn không phù hợp. 

Lực lượng vũ phu hoạt động về mặt khái niệm vì một trong các cây biểu thức được tạo là cây được xác định bởi mức độ ưu tiên và dấu ngoặc đơn của đầu vào. Nó thất bại vì nó khám phá một số lượng lớn các cây mà ngữ pháp có thể xác định cục bộ mà không cần xem xét chúng. 

Điều quan trọng cần lưu ý là các biểu thức số học có ngữ pháp rất đơn giản. Biểu thức là một chuỗi các phép tính cộng hoặc trừ được áp dụng cho các biểu thức cấp nhân, trong khi biểu thức cấp nhân là một chuỗi các`*`,`/`, hoặc`%`các phép toán áp dụng cho biểu thức nguyên tử. Biểu thức nguyên tử là mã định danh hoặc biểu thức được đặt trong ngoặc đơn khác. 

Ngữ pháp đó cho phép chúng ta phân tích biểu thức một cách trực tiếp. Chúng tôi phân tích đệ quy đơn vị có ý nghĩa nhỏ nhất, sau đó kết hợp các đơn vị đó theo mức độ ưu tiên. Khi đã có sẵn cây biểu thức chính xác, việc dịch thuật sẽ trở thành máy móc: đối với mỗi nút nhị phân, dịch con bên trái của nó làm nút nhận và con bên phải của nó làm đối số. 

Điều này cũng giải thích tại sao dấu ngoặc đơn biến mất. Một biểu thức con trong ngoặc đơn được phân tích cú pháp thành một nút cây hoàn chỉnh. Khi nút đó trở thành toán hạng bên phải của một thao tác, cú pháp gọi phương thức của Java đã cung cấp nhóm được yêu cầu, như trong`a.add(b.multiply(c))`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(k-1)) | O(k) mỗi ứng viên | Quá chậm | 
| gốc đệ quy | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định ngữ pháp biểu đạt theo ba cấp độ. Bộ xử lý phân tích cú pháp cấp cao nhất`+`Và`-`, cấp độ xử lý tiếp theo`*`,`/`, Và`%`và mức thấp nhất xử lý các mã định danh và biểu thức được đặt trong ngoặc đơn. Thứ tự này khớp trực tiếp với quyền ưu tiên số học C++ thông thường. 
2. Phân tích một thừa số bằng cách đọc mã định danh khi ký tự hiện tại là một chữ cái. Sử dụng toàn bộ mã định danh vì mã định danh có thể chứa nhiều chữ cái, chẳng hạn như`abc`. 
3. Nếu một yếu tố bắt đầu bằng`(`, sử dụng dấu ngoặc đơn đó, phân tích cú pháp đệ quy một biểu thức hoàn chỉnh và sau đó sử dụng kết quả khớp`)`. Cây biểu thức được trả về đại diện cho nội dung của dấu ngoặc đơn, vì vậy bản thân dấu ngoặc đơn không cần tồn tại ở đầu ra. 
4. Phân tích biểu thức cấp nhân bằng cách phân tích một thừa số đầu tiên. Trong khi nhân vật tiếp theo là`*`,`/`, hoặc`%`, phân tích một yếu tố khác và tạo một nút nhị phân với toán tử tương ứng. Lặp lại điều này từ trái sang phải sẽ mang lại sự kết hợp bên trái chính xác. 
5. Phân tích biểu thức cấp bổ sung theo cách tương tự, nhưng sử dụng`+`Và`-`làm toán tử và sử dụng các biểu thức cấp nhân làm toán hạng của nó. Vì trình phân tích cú pháp cấp thấp hơn sử dụng tất cả`*`,`/`, Và`%`hoạt động đầu tiên, quyền ưu tiên của chúng sẽ tự động được giữ nguyên. 
6. Sau khi phân tích cú pháp biểu thức hoàn chỉnh, dịch đệ quy cây của nó. Một nút định danh trả về tên của nó một cách trực tiếp. Đối với nút nhị phân, dịch con trái và con phải, sau đó tạo`left.method(right)`. 
7. Ánh xạ năm toán tử tới Java của chúng`BigInteger`phương pháp. Việc lập bản đồ là`+`ĐẾN`add`,`-`ĐẾN`subtract`,`*`ĐẾN`multiply`,`/`ĐẾN`divide`, Và`%`ĐẾN`remainder`. 

### Tại sao nó hoạt động 

Trình phân tích cú pháp duy trì tính bất biến rằng mọi cây con được trả về biểu thị chính xác biểu thức C++ được bao phủ bởi phạm vi đầu vào đã sử dụng, với mức độ ưu tiên và nhóm chính xác. Các dấu ngoặc đơn buộc trình phân tích cú pháp phải hoàn thành một biểu thức hoàn chỉnh trước khi quay lại toán tử xung quanh, trong khi các mức ưu tiên riêng biệt ngăn toán tử có mức ưu tiên thấp hơn tiếp thu toán hạng thuộc về một phép toán có mức ưu tiên cao hơn. 

Đối với mỗi nút cây nhị phân, bản dịch đặt cây con bên trái đã dịch trước lệnh gọi phương thức và cây con bên phải đã dịch bên trong đối số của nó. Do đó, biểu thức Java thu được có cây biểu thức giống hệt như biểu thức C++ được phân tích cú pháp. Vì mọi toán tử đầu vào được dịch theo tương ứng của nó`BigInteger`phương thức, biểu thức kết quả sẽ bảo toàn cả toán hạng và nhóm của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

METHOD = {
    '+': 'add',
    '-': 'subtract',
    '*': 'multiply',
    '/': 'divide',
    '%': 'remainder'
}

class Parser:
    def __init__(self, s):
        self.s = s
        self.n = len(s)
        self.pos = 0

    def parse(self):
        return self.parse_expr()

    def parse_expr(self):
        node = self.parse_term()

        while self.pos < self.n and self.s[self.pos] in '+-':
            op = self.s[self.pos]
            self.pos += 1
            right = self.parse_term()
            node = (op, node, right)

        return node

    def parse_term(self):
        node = self.parse_factor()

        while self.pos < self.n and self.s[self.pos] in '*/%':
            op = self.s[self.pos]
            self.pos += 1
            right = self.parse_factor()
            node = (op, node, right)

        return node

    def parse_factor(self):
        if self.s[self.pos] == '(':
            self.pos += 1
            node = self.parse_expr()
            self.pos += 1
            return node

        start = self.pos
        while self.pos < self.n and self.s[self.pos].isalpha():
            self.pos += 1

        return self.s[start:self.pos]

def translate(node):
    if isinstance(node, str):
        return node

    op, left, right = node
    return translate(left) + '.' + METHOD[op] + '(' + translate(right) + ')'

def solve_line(s):
    parser = Parser(s)
    tree = parser.parse()
    return translate(tree)

def main():
    output = []

    for line in sys.stdin:
        s = line.strip()
        if s:
            output.append(solve_line(s))

    sys.stdout.write('\n'.join(output))

if __name__ == "__main__":
    main()
```các`Parser`lưu trữ đầu vào và một con trỏ`pos`. Mỗi hàm phân tích cú pháp sử dụng chính xác phần biểu thức thuộc cấp độ ngữ pháp của nó.`parse_expr`xử lý phép cộng và phép trừ, trong khi`parse_term`xử lý phép nhân, chia và số dư. 

các`parse_factor`chức năng là nơi xử lý việc nhóm. Khi nó nhìn thấy`(`, nó phân tích toàn bộ biểu thức trước khi sử dụng phần đóng`)`. Đây là điều phân biệt`a+(b+c)`từ`(a+b)+c`. 

Trình phân tích cú pháp biểu thị một mã định danh dưới dạng một chuỗi và một phép toán nhị phân dưới dạng một bộ ba phần tử chứa toán tử và hai phần tử con của nó. Điều này mang lại cho chúng ta một cây biểu thức rõ ràng mà không cần phải thao tác nhiều lần với chuỗi gốc. 

Giai đoạn dịch thuật sử dụng cây đó thay vì các ký tự gốc. Ví dụ, cây cho`a+(b*c)`có`+`tại gốc của nó,`a`là con trái của nó, và`*`với trẻ em`b`Và`c`là con bên phải của nó. Do đó, bản dịch tạo ra`a.add(b.multiply(c))`. 

Các cuộc gọi đệ quy không bao giờ thực hiện số học, do đó không có vấn đề tràn số nguyên. Độ dài biểu thức tối đa chỉ là 1000 ký tự. Mã này cũng chỉ làm tăng mối lo ngại về giới hạn đệ quy của Python ở mức độ nhẹ vì các dấu ngoặc đơn lồng nhau sâu yêu cầu ít nhất hai ký tự cho mỗi cấp độ lồng nhau, nhưng việc triển khai có thể thêm một cách an toàn.`sys.setrecursionlimit`nếu muốn. Giới hạn đã cho giữ trình phân tích cú pháp đệ quy tự nhiên trong giới hạn thực tế. 

## Ví dụ đã hoạt động 

Đối với biểu thức mẫu đầu tiên,`a+b+c`, trình phân tích cú pháp sẽ thấy hai thao tác bổ sung có cùng mức độ ưu tiên. Bởi vì vòng lặp xử lý chúng từ trái sang phải nên cây kết quả là`(a+b)+c`. 

| Vị trí đầu vào | Trạng thái phân tích cú pháp | Cây con được xây dựng | 
| --- | --- | --- | 
| Đọc`a`|`pos`sau đó`a`|`a`| 
| Đọc`+b`|`+`kết hợp`a`Và`b`|`a+b`| 
| Đọc`+c`|`+`kết hợp cây trước đó và`c`|`(a+b)+c`| 
| Dịch gốc`+`|`add`|`a.add(b).add(c)`| 

Thuộc tính quan trọng ở đây là tính kết hợp trái. Phần bổ sung đầu tiên trở thành người nhận phần thứ hai`add`cuộc gọi, vì vậy đầu ra được xâu chuỗi một cách tự nhiên. 

Đối với biểu thức mẫu thứ hai,`(a+b)%(c+d)`, dấu ngoặc đơn khiến mỗi bên được phân tích cú pháp dưới dạng biểu thức cộng hoàn chỉnh trước`%`được xử lý. 

| Vị trí đầu vào | Trạng thái phân tích cú pháp | Cây con được xây dựng | 
| --- | --- | --- | 
| Đọc`(a+b)`| Phân tích biểu thức được nhóm |`a+b`| 
| Đọc`%`|`%`chờ yếu tố phù hợp của nó |`a+b`| 
| Đọc`(c+d)`| Phân tích biểu thức nhóm thứ hai |`c+d`| 
| Kết hợp`%`| Hoạt động gốc |`(a+b)%(c+d)`| 
| Dịch gốc`%`|`remainder`|`a.add(b).remainder(c.add(d))`| 

Điều này chứng tỏ tại sao không cần in dấu ngoặc đơn gốc. Mỗi biểu thức được nhóm sẽ trở thành một biểu thức phương thức Java lồng nhau và chính đối số phương thức sẽ cung cấp việc nhóm được yêu cầu. Điều này phù hợp với đầu ra mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự đầu vào được sử dụng một lần trong quá trình phân tích cú pháp và mỗi nút cây biểu thức được dịch một lần. | 
| Không gian | O(n) | Cây biểu thức chứa các nút O(n) và trình phân tích cú pháp đệ quy sử dụng không gian ngăn xếp O(n) trong cách lồng sâu nhất có thể. | 

Dòng đầu vào tối đa chỉ có 1000 ký tự, do đó, việc phân tích cú pháp tuyến tính sẽ để lại khoảng trống đáng kể dưới giới hạn 1 giây và 256 MB do sự cố chỉ định. Thuật toán cũng xử lý mọi trường hợp thử nghiệm một cách độc lập và đọc cho đến khi EOF theo yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

METHOD = {
    '+': 'add',
    '-': 'subtract',
    '*': 'multiply',
    '/': 'divide',
    '%': 'remainder'
}

class Parser:
    def __init__(self, s):
        self.s = s
        self.n = len(s)
        self.pos = 0

    def parse(self):
        return self.parse_expr()

    def parse_expr(self):
        node = self.parse_term()

        while self.pos < self.n and self.s[self.pos] in '+-':
            op = self.s[self.pos]
            self.pos += 1
            right = self.parse_term()
            node = (op, node, right)

        return node

    def parse_term(self):
        node = self.parse_factor()

        while self.pos < self.n and self.s[self.pos] in '*/%':
            op = self.s[self.pos]
            self.pos += 1
            right = self.parse_factor()
            node = (op, node, right)

        return node

    def parse_factor(self):
        if self.s[self.pos] == '(':
            self.pos += 1
            node = self.parse_expr()
            self.pos += 1
            return node

        start = self.pos
        while self.pos < self.n and self.s[self.pos].isalpha():
            self.pos += 1

        return self.s[start:self.pos]

def translate(node):
    if isinstance(node, str):
        return node

    op, left, right = node
    return translate(left) + '.' + METHOD[op] + '(' + translate(right) + ')'

def solve_line(s):
    return translate(Parser(s).parse())

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return '\n'.join(
            solve_line(line.strip())
            for line in sys.stdin
            if line.strip()
        )
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("a+b+c\n") == "a.add(b).add(c)", "sample 1"
assert run("(a+b)+c\n") == "a.add(b).add(c)", "sample 2"
assert run("a+(b+c)\n") == "a.add(b.add(c))", "sample 3"
assert run("(a+b)%(c+d)\n") == "a.add(b).remainder(c.add(d))", "sample 4"

# Minimum-size expression
assert run("x\n") == "x", "single identifier"

# Repeated identical identifier
assert run("a+a+a+a\n") == "a.add(a).add(a).add(a)", "repeated identifier"

# Precedence and right-side grouping
assert run("a+b*c-d/e\n") == \
       "a.add(b.multiply(c)).subtract(d.divide(e))", \
       "operator precedence"

# Non-associative operations and nested grouping
assert run("a-(b-c/(d+e))\n") == \
       "a.subtract(b.subtract(c.divide(d.add(e))))", \
       "nested grouping"

# Maximum-size expression, 500 identifiers and 499 operators
expr = "+".join(["a"] * 500)
expected = "a" + ".add(a)" * 499
assert len(expr) == 999
assert run(expr + "\n") == expected, "maximum-size expression"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`x`|`x`| Biểu thức kích thước tối thiểu và phân tích hệ số | 
|`a+a+a+a`|`a.add(a).add(a).add(a)`| Kết hợp trái và định danh lặp đi lặp lại | 
|`a+b*c-d/e`|`a.add(b.multiply(c)).subtract(d.divide(e))`| Ưu tiên nhân và chia | 
|`a-(b-c/(d+e))`|`a.subtract(b.subtract(c.divide(d.add(e))))`| Dấu ngoặc đơn lồng nhau và toán tử không liên kết | 
| 500 bản`a`tham gia bởi`+`|`a.add(a)...`| Kích thước đầu vào tối đa và chuỗi bên trái lặp đi lặp lại | 

## Vỏ cạnh 

cho`a+(b+c)`, trình phân tích cú pháp đầu tiên sẽ đọc`a`như phía bên trái của phép cộng bên ngoài. Khi nó vào dấu ngoặc đơn, nó sẽ phân tích đệ quy`b+c`thành một cây con hoàn chỉnh. Bản dịch của cây con đó là`b.add(c)`, do đó nút bên ngoài trở thành`a.add(b.add(c))`. Việc thay thế phẳng từ trái sang phải sẽ tạo ra không chính xác`a.add(b).add(c)`. 

Vì`(a+b)+c`, trước tiên trình phân tích cú pháp sẽ nhập dấu ngoặc đơn và xây dựng cây con`a+b`. Sau khi quay lại biểu thức bên ngoài, nó kết hợp cây con đó với`c`. Dịch tạo ra`a.add(b).add(c)`. Dấu ngoặc đơn biến mất vì`a.add(b)`đã là một biểu thức Java hợp lệ và có thể trực tiếp trở thành người nhận`.add(c)`. 

Vì`a+b*c`,`parse_expr`hỏi`parse_term`cho các toán hạng của nó. Toán hạng bên phải được phân tích cú pháp bởi`parse_term`, tiêu thụ toàn bộ`b*c`hoạt động trước khi quay trở lại. Cây kết quả là`a+(b*c)`, và đầu ra là`a.add(b.multiply(c))`. Điều này nắm bắt các triển khai xử lý các toán tử chỉ theo sự xuất hiện của chúng trong đầu vào. 

Vì`a-(b-c)`, bên ngoài`-`nhận được cây con hoàn chỉnh`b-c`là toán hạng bên phải của nó. Kết quả là`a.subtract(b.subtract(c))`. Nếu việc triển khai làm phẳng biểu thức thành một chuỗi, nó sẽ tạo ra`a.subtract(b).subtract(c)`, đại diện cho một cây khác. 

Vì`a/b%c`, cả hai`/`Và`%`thuộc cùng mức độ ưu tiên và được kết hợp ở bên trái. Trình phân tích cú pháp đầu tiên xây dựng`(a/b)`, sau đó áp dụng`%`đến kết quả đó, đưa ra`a.divide(b).remainder(c)`. Đây là một trường hợp biên hữu ích khác vì việc xử lý`%`vì có mức độ ưu tiên khác sẽ âm thầm thay đổi cây. 

Đối với một mã định danh duy nhất như`x`,`parse_factor`tiêu thụ toàn bộ mã định danh và không tìm thấy toán tử nào sau đó. Cây chỉ có một lá nên việc dịch đơn giản là trả về`x`. Không có lệnh gọi phương thức hoặc dấu ngoặc đơn nhân tạo nào được giới thiệu. 

Cuối cùng, một biểu thức có độ dài tối đa chẳng hạn như 500 bản sao của`a`được nối bởi 499 dấu cộng chứa 999 ký tự. Mỗi thao tác cộng được thực hiện một lần, tạo ra cây sâu bên trái và cuối cùng là chuỗi`a.add(a).add(a)...`. Thuật toán không cần tìm kiếm các toán tử phù hợp hoặc xem xét lại các quyết định trước đó, do đó công việc của nó phát triển tuyến tính với kích thước đầu vào.
