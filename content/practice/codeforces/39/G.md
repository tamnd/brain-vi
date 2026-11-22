---
title: "CF 39G - Hàm nghịch đảo"
description: "Chúng ta được cung cấp mã nguồn của hàm đệ quy nhỏ f(n) được viết bằng một tập con bị hạn chế rất nhiều của C++. Hàm chỉ chứa tuần tự if (...) return ...; lời khai và sự hoàn trả vô điều kiện…; các tuyên bố."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "G"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 2400
weight: 39
solve_time_s: 122
verified: true
draft: false
---
[CF 39G - Hàm nghịch đảo](https://codeforces.com/problemset/problem/39/G) 

**Đánh giá:** 2400 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp mã nguồn của một hàm đệ quy nhỏ`f(n)`được viết bằng một tập con bị hạn chế rất nhiều của C++. Hàm chỉ chứa tuần tự`if (...) return ...;`tuyên bố và vô điều kiện`return ...;`các tuyên bố. Các biểu thức có thể chứa các lệnh gọi số học, so sánh và đệ quy tới`f`. 

Số học của máy là không bình thường. Phép cộng, phép trừ và phép nhân được thực hiện theo modulo`32768`, trong khi phép chia là phép chia số nguyên thông thường. Mỗi đầu vào hợp lệ`n`nằm ở`[0, 32767]`và mọi kết quả của hàm cũng nằm trong phạm vi đó. 

Nhiệm vụ bị đảo ngược so với hướng thông thường. Chúng ta được cho một giá trị`y = f(n)`và chính định nghĩa hàm. Chúng ta phải thu hồi phần lớn nhất`n`như vậy`f(n) = y`. Nếu không như vậy`n`tồn tại, chúng tôi in`-1`. 

Phần đáng sợ nhất của câu lệnh là cú pháp đệ quy, nhưng kích thước đầu vào thực tế sẽ thay đổi hoàn toàn bản chất của vấn đề. Toàn bộ văn bản chương trình tối đa là 100 byte. Điều đó có nghĩa là cây cú pháp được phân tích cú pháp rất nhỏ. Kích thước tên miền cũng được cố định chính xác`32768`đầu vào có thể. Thậm chí đánh giá chức năng một cách riêng biệt cho mọi khả năng`n`là thực tế nếu mỗi đánh giá đều hiệu quả. 

Hạn chế đệ quy là sự đảm bảo về cấu trúc quan trọng. Bất cứ khi nào chúng tôi đánh giá`f(N)`, các cuộc gọi đệ quy chỉ có thể sử dụng các đối số nhỏ hơn. Điều này có nghĩa là biểu đồ phụ thuộc không theo chu kỳ. Chúng ta có thể tính toán một cách an toàn tất cả các giá trị từ`0`trở lên bằng cách sử dụng ghi nhớ hoặc lập trình động. 

Việc thực hiện bất cẩn có thể âm thầm thất bại ở một số nơi. 

Một lỗi phổ biến là quên rằng phép nhân cũng là modulo`32768`. 

Ví dụ:```
return 200 * 200;
```Kết quả toán học là`40000`, nhưng giá trị hàm là:```
40000 mod 32768 = 7232
```Sử dụng phép nhân số nguyên thông thường sẽ tạo ra câu trả lời hoàn toàn sai. 

Một vấn đề tinh tế khác là thứ tự đánh giá để phân chia. 

Coi như:```
return 7 / 2 * 3;
```Các hoạt động được đánh giá từ trái sang phải trong cùng mức độ ưu tiên. Kết quả là:```
(7 / 2) * 3 = 3 * 3 = 9
```không:```
7 / (2 * 3) = 1
```Trình phân tích cú pháp bỏ qua tính kết hợp sẽ đánh giá sai nhiều biểu thức. 

Các cuộc gọi đệ quy cũng tạo ra bẫy nếu bỏ qua việc ghi nhớ. 

Ví dụ:```
if (n == 0) return 1;
return f(n - 1) + 1;
```Không có bộ nhớ đệm, đánh giá tất cả`32768`các trạng thái đệ quy trở thành bậc hai. Với tính năng ghi nhớ, mỗi trạng thái được tính một lần. 

Sự tinh tế cuối cùng là yêu cầu trả lời. Chúng tôi cần giá trị tối đa hợp lệ`n`, không phải là người đầu tiên gặp phải. 

Ví dụ:```
if (n < 100) return 17;
return 17;
```Mỗi đầu vào ánh xạ tới`17`, vậy câu trả lời là`32767`. Dừng ở trận đầu tiên sẽ quay lại không chính xác`0`. 

## Phương pháp tiếp cận 

Ý tưởng dùng vũ lực trực tiếp rất đơn giản. Phân tích hàm, sau đó với mọi`n`từ`0`ĐẾN`32767`, đánh giá`f(n)`và so sánh nó với giá trị mục tiêu. Vì các lệnh gọi đệ quy chỉ đi đến các đối số nhỏ hơn nên đệ quy sẽ kết thúc một cách tự nhiên. 

Điều này nghe có vẻ khả thi vì kích thước tên miền chỉ`32768`. Vấn đề là tính toán lại nhiều lần. Giả sử hàm này là:```
return f(n - 1) + 1;
```Đánh giá`f(30000)`recursively recomputes almost the entire prefix every time. Tổng công việc trở nên gần như:```
1 + 2 + 3 + ... + 32768
```đó là về`5 * 10^8`đánh giá, quá chậm trong Python. 

Quan sát quan trọng là đồ thị phụ thuộc đệ quy hoàn toàn có tính một chiều. Mỗi trạng thái chỉ phụ thuộc vào các trạng thái nhỏ hơn. Điều đó có nghĩa là mỗi`f(n)`có thể được tính toán chính xác một lần và tái sử dụng mãi mãi. 

Sau khi phân tích chương trình thành cây cú pháp trừu tượng, chúng ta đánh giá các giá trị theo thứ tự tăng dần`n`. Bất cứ khi nào một biểu thức cần`f(x)`, giá trị cho`x`đã được tính toán rồi vì`x < n`. 

Bản thân cú pháp đủ nhỏ để một trình phân tích cú pháp gốc đệ quy viết tay là lý tưởng. Ngữ pháp đã mã hóa các quy tắc ưu tiên:```
product before sum
left associativity inside each level
```Vì vậy, chúng tôi phân tích các biểu thức một cách chính xác theo ngữ pháp và đánh giá chúng theo ngữ pháp hiện tại.`n`. 

Lực lượng vũ phu hoạt động vì tên miền rất nhỏ nhưng việc mở rộng đệ quy lặp đi lặp lại khiến nó trở nên quá đắt. Việc ghi nhớ biến đệ quy thành một chương trình động theo thời gian tuyến tính trên tất cả`32768`tiểu bang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force không cần ghi nhớ | Hàm mũ trong chuỗi đệ quy tồi tệ nhất | O(độ sâu đệ quy) | Quá chậm | 
| Đánh giá ghi nhớ tối ưu | O(32768 × kích thước chương trình) | O(32768) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đích và nối phần đầu vào còn lại thành một chuỗi. 

Dấu cách và ngắt dòng không liên quan đến ngữ pháp, do đó, làm việc trên một chuỗi nhỏ gọn sẽ đơn giản hóa việc phân tích cú pháp. 
2. Xây dựng trình mã thông báo và trình phân tích cú pháp gốc đệ quy cho ngữ pháp. 

Chúng tôi phân tích các biểu thức bằng cách sử dụng hệ thống phân cấp ưu tiên tiêu chuẩn:`comparison -> sum -> product -> atom`Điều này đảm bảo thứ tự đánh giá chính xác cho`+`,`-`,`*`, Và`/`. 
3. Phân tích nội dung hàm thành một danh sách các câu lệnh. 

Mỗi tuyên bố là:```
return expr;
```hoặc:```
if (cond) return expr;
```4. Biểu diễn các biểu thức dưới dạng nút cây cú pháp. 

Mỗi nút lưu trữ loại của nó:```
number
variable n
binary operator
recursive call
```Điều này cho phép đánh giá lặp đi lặp lại nhanh chóng cho các giá trị khác nhau của`n`. 
5. Tạo một mảng`dp[0..32767]`.`dp[x]`sẽ lưu trữ giá trị tính toán của`f(x)`. 
6. Đánh giá các giá trị theo thứ tự tăng dần từ`0`ĐẾN`32767`. 

Khi đánh giá`f(n)`, quét các câu lệnh một cách tuần tự. 

Đối với lợi nhuận có điều kiện: 

- Đánh giá điều kiện 
- nếu đúng, đánh giá và trả về biểu thức ngay lập tức 

Đối với sự trở lại vô điều kiện: 

- đánh giá và trả lại ngay lập tức 

Vì các lệnh gọi đệ quy chỉ sử dụng các đối số nhỏ hơn nên mọi lệnh gọi đệ quy đều cần thiết.`dp[k]`đã tồn tại rồi. 
7. Trong quá trình đánh giá biểu thức: 

- phép cộng, phép trừ và phép nhân được giảm modulo`32768`- phép trừ được chuẩn hóa trở lại thành`[0, 32767]`- phép chia sử dụng phép chia số nguyên thông thường 
- các cuộc gọi đệ quy sử dụng tính toán trước đó`dp[value]`8. Theo dõi lớn nhất`n`có giá trị tính toán bằng mục tiêu. 
9. In kết quả khớp lớn nhất`n`, hoặc`-1`nếu không có trận đấu nào tồn tại. 

### Tại sao nó hoạt động 

Vấn đề đảm bảo rằng các cuộc gọi đệ quy từ`f(N)`chỉ các giá trị mục tiêu nhỏ hơn`N`. Điều này tạo ra một trật tự tôpô trên các trạng thái:```
0, 1, 2, ...
```Khi tính toán`dp[n]`, mọi phụ thuộc đệ quy đã được hoàn thiện. Đánh giá biểu thức tuân theo chính xác ngữ pháp ngôn ngữ, do đó mức độ ưu tiên của toán tử và tính kết hợp khớp với đặc tả. Vì các câu lệnh được xử lý tuần tự và việc đánh giá dừng lại ở câu lệnh trả về đầu tiên nên việc thực thi mô phỏng giống hệt với chương trình gốc. Mọi`dp[n]`do đó chính xác bằng giá trị hàm thực`f(n)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 32768

class Parser:
    def __init__(self, s):
        self.s = s
        self.i = 0

    def skip(self):
        while self.i < len(self.s) and self.s[self.i].isspace():
            self.i += 1

    def match(self, t):
        self.skip()
        if self.s.startswith(t, self.i):
            self.i += len(t)
            return True
        return False

    def expect(self, t):
        assert self.match(t)

    def parse_number(self):
        self.skip()
        j = self.i
        while self.i < len(self.s) and self.s[self.i].isdigit():
            self.i += 1
        return ("num", int(self.s[j:self.i]))

    def parse_atom(self):
        self.skip()

        if self.match("n"):
            return ("n",)

        if self.match("f"):
            self.expect("(")
            expr = self.parse_sum()
            self.expect(")")
            return ("call", expr)

        if self.s[self.i].isdigit():
            return self.parse_number()

        if self.match("("):
            expr = self.parse_sum()
            self.expect(")")
            return expr

    def parse_product(self):
        node = self.parse_atom()

        while True:
            self.skip()

            if self.match("*"):
                node = ("bin", "*", node, self.parse_atom())
            elif self.match("/"):
                node = ("bin", "/", node, self.parse_atom())
            else:
                break

        return node

    def parse_sum(self):
        node = self.parse_product()

        while True:
            self.skip()

            if self.match("+"):
                node = ("bin", "+", node, self.parse_product())
            elif self.match("-"):
                node = ("bin", "-", node, self.parse_product())
            else:
                break

        return node

    def parse_condition(self):
        left = self.parse_sum()

        self.skip()

        if self.match("=="):
            op = "=="
        elif self.match(">"):
            op = ">"
        else:
            self.expect("<")
            op = "<"

        right = self.parse_sum()

        return (op, left, right)

    def parse_statement(self):
        self.skip()

        if self.match("if"):
            self.expect("(")
            cond = self.parse_condition()
            self.expect(")")
            self.expect("return")
            expr = self.parse_sum()
            self.expect(";")
            return ("if", cond, expr)

        self.expect("return")
        expr = self.parse_sum()
        self.expect(";")
        return ("ret", expr)

    def parse_program(self):
        self.expect("int")
        self.expect("f")
        self.expect("(")
        self.expect("int")
        self.expect("n")
        self.expect(")")
        self.expect("{")

        stmts = []

        while True:
            self.skip()
            if self.match("}"):
                break
            stmts.append(self.parse_statement())

        return stmts

def eval_expr(node, n, dp):
    t = node[0]

    if t == "num":
        return node[1]

    if t == "n":
        return n

    if t == "call":
        x = eval_expr(node[1], n, dp)
        return dp[x]

    op = node[1]

    a = eval_expr(node[2], n, dp)
    b = eval_expr(node[3], n, dp)

    if op == "+":
        return (a + b) % MOD

    if op == "-":
        return (a - b) % MOD

    if op == "*":
        return (a * b) % MOD

    return a // b

def eval_cond(cond, n, dp):
    op, left, right = cond

    a = eval_expr(left, n, dp)
    b = eval_expr(right, n, dp)

    if op == "<":
        return a < b

    if op == ">":
        return a > b

    return a == b

def solve():
    target = int(input())

    code = sys.stdin.read()

    parser = Parser(code)
    stmts = parser.parse_program()

    dp = [0] * MOD

    ans = -1

    for n in range(MOD):
        for stmt in stmts:
            if stmt[0] == "if":
                _, cond, expr = stmt

                if eval_cond(cond, n, dp):
                    dp[n] = eval_expr(expr, n, dp)
                    break
            else:
                _, expr = stmt
                dp[n] = eval_expr(expr, n, dp)
                break

        if dp[n] == target:
            ans = n

    print(ans)

solve()
```Trình phân tích cú pháp phản ánh ngữ pháp một cách trực tiếp.`parse_product`tay cầm`*`Và`/`, trong khi`parse_sum`tay cầm`+`Và`-`. Vì mỗi cấp độ liên tục sử dụng các toán tử từ trái sang phải nên tính kết hợp sẽ tự động trở thành chính xác. 

Cây biểu thức được biểu diễn bằng các bộ dữ liệu thay vì các lớp. Điều này giúp cho việc đánh giá nhẹ nhàng và đủ nhanh cho hàng chục nghìn lần thực thi. 

Thứ tự lập trình động là chi tiết triển khai quan trọng nhất. Chúng tôi tính toán`dp[0]`, sau đó`dp[1]`, vân vân. Bảo đảm sự cố cho biết các cuộc gọi đệ quy chỉ sử dụng các đối số nhỏ hơn, do đó mọi phụ thuộc đệ quy đều có sẵn. 

Xử lý modulo là một nơi khác dễ xảy ra lỗi. của Python`%`đã giữ kết quả phép trừ không âm:```
(a - b) % MOD
```khớp chính xác với định nghĩa vấn đề. 

Vòng lặp câu lệnh dừng ngay sau nhánh trả về đầu tiên, giống như việc thực thi thực sự. quên đi`break`sẽ tiếp tục quét các câu lệnh sau một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
17
int f(int n)
{
if (n < 100) return 17;
if (n > 99) return 27;
}
```Theo dõi các giá trị đã chọn: 
| và | Điều kiện đầu tiên`n < 100`| Giá trị trả về | Phù hợp với mục tiêu | 
| --- | --- | --- | --- | 
| 0 | đúng | 17 | vâng | 
| 50 | đúng | 17 | vâng | 
| 99 | đúng | 17 | vâng | 
| 100 | sai | 27 | không | 
| 32767 | sai | 27 | không | 

Đầu vào phù hợp lớn nhất là`99`. 

Ví dụ này chứng tỏ rằng việc thực thi dừng lại ở câu lệnh trả về được thỏa mãn đầu tiên. Điều kiện thứ hai không bao giờ được kiểm tra`n < 100`. 

### Ví dụ 2 

đầu vào:```
1
int f(int n)
{
if(n==0)return 1;
return f(n-1)+1;
}
```Theo dõi một số trạng thái đầu tiên: 

| n | Phụ thuộc đệ quy | Giá trị tính toán | 
| --- | --- | --- | 
| 0 | không | 1 | 
| 1 | f(0) | 2 | 
| 2 | f(1) | 3 | 
| 3 | f(2) | 4 | 

Giá trị mục tiêu`1`chỉ xuất hiện tại`n = 0`. 

Dấu vết này xác nhận lý do tại sao DP bậc tăng dần hoạt động. Mọi phụ thuộc đệ quy đều đã tồn tại trước khi trạng thái hiện tại được đánh giá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(32768 × S) |`S`là kích thước chương trình được phân tích cú pháp nhỏ | 
| Không gian | O(32768) | Mảng DP cho tất cả các giá trị hàm | 

Độ dài mã nguồn tối đa là 100 byte nên cây cú pháp cực kỳ nhỏ. Đánh giá mọi đầu vào từ`0`ĐẾN`32767`hoàn toàn an toàn trong giới hạn 5 giây. Việc sử dụng bộ nhớ bị chi phối bởi mảng kích thước DP`32768`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 32768

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    input = sys.stdin.readline

    class Parser:
        def __init__(self, s):
            self.s = s
            self.i = 0

        def skip(self):
            while self.i < len(self.s) and self.s[self.i].isspace():
                self.i += 1

        def match(self, t):
            self.skip()
            if self.s.startswith(t, self.i):
                self.i += len(t)
                return True
            return False

        def expect(self, t):
            assert self.match(t)

        def parse_number(self):
            self.skip()
            j = self.i
            while self.i < len(self.s) and self.s[self.i].isdigit():
                self.i += 1
            return ("num", int(self.s[j:self.i]))

        def parse_atom(self):
            self.skip()

            if self.match("n"):
                return ("n",)

            if self.match("f"):
                self.expect("(")
                expr = self.parse_sum()
                self.expect(")")
                return ("call", expr)

            if self.s[self.i].isdigit():
                return self.parse_number()

            self.expect("(")
            expr = self.parse_sum()
            self.expect(")")
            return expr

        def parse_product(self):
            node = self.parse_atom()

            while True:
                self.skip()

                if self.match("*"):
                    node = ("bin", "*", node, self.parse_atom())
                elif self.match("/"):
                    node = ("bin", "/", node, self.parse_atom())
                else:
                    break

            return node

        def parse_sum(self):
            node = self.parse_product()

            while True:
                self.skip()

                if self.match("+"):
                    node = ("bin", "+", node, self.parse_product())
                elif self.match("-"):
                    node = ("bin", "-", node, self.parse_product())
                else:
                    break

            return node

        def parse_condition(self):
            left = self.parse_sum()

            if self.match("=="):
                op = "=="
            elif self.match(">"):
                op = ">"
            else:
                self.expect("<")
                op = "<"

            right = self.parse_sum()

            return (op, left, right)

        def parse_statement(self):
            if self.match("if"):
                self.expect("(")
                cond = self.parse_condition()
                self.expect(")")
                self.expect("return")
                expr = self.parse_sum()
                self.expect(";")
                return ("if", cond, expr)

            self.expect("return")
            expr = self.parse_sum()
            self.expect(";")
            return ("ret", expr)

        def parse_program(self):
            self.expect("int")
            self.expect("f")
            self.expect("(")
            self.expect("int")
            self.expect("n")
            self.expect(")")
            self.expect("{")

            stmts = []

            while True:
                self.skip()
                if self.match("}"):
                    break
                stmts.append(self.parse_statement())

            return stmts

    def eval_expr(node, n, dp):
        t = node[0]

        if t == "num":
            return node[1]

        if t == "n":
            return n

        if t == "call":
            return dp[eval_expr(node[1], n, dp)]

        op = node[1]
        a = eval_expr(node[2], n, dp)
        b = eval_expr(node[3], n, dp)

        if op == "+":
            return (a + b) % MOD
        if op == "-":
            return (a - b) % MOD
        if op == "*":
            return (a * b) % MOD

        return a // b

    def eval_cond(cond, n, dp):
        op, l, r = cond
        a = eval_expr(l, n, dp)
        b = eval_expr(r, n, dp)

        if op == "<":
            return a < b
        if op == ">":
            return a > b
        return a == b

    target = int(input())
    code = sys.stdin.read()

    parser = Parser(code)
    stmts = parser.parse_program()

    dp = [0] * MOD
    ans = -1

    for n in range(MOD):
        for stmt in stmts:
            if stmt[0] == "if":
                _, cond, expr = stmt
                if eval_cond(cond, n, dp):
                    dp[n] = eval_expr(expr, n, dp)
                    break
            else:
                _, expr = stmt
                dp[n] = eval_expr(expr, n, dp)
                break

        if dp[n] == target:
            ans = n

    return str(ans)

# provided sample
assert run("""17
int f(int n)
{
if (n < 100) return 17;
if (n > 99) return 27;
}
""") == "99"

# constant function
assert run("""5
int f(int n){return 5;}
""") == "32767"

# modulo multiplication
assert run("""7232
int f(int n){return 200*200;}
""") == "32767"

# recursive chain
assert run("""4
int f(int n){
if(n==0)return 1;
return f(n-1)+1;
}
""") == "3"

# impossible target
assert run("""123
int f(int n){return 0;}
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hàm hằng trả về 5 | 32767 | Phải trả về đầu vào hợp lệ lớn nhất | 
|`200*200`| 32767 | Phép nhân modulo đúng | 
| Chuỗi tăng đệ quy | 3 | Đánh giá DP đệ quy | 
| Hàm số 0 không đổi với mục tiêu 123 | -1 | Xử lý đúng các câu trả lời còn thiếu | 

## Vỏ cạnh 

Hãy xem xét trường hợp số học modulo:```
7232
int f(int n){return 200*200;}
```Thuật toán đánh giá:```
200 * 200 = 40000
40000 mod 32768 = 7232
```Mỗi đầu vào ánh xạ tới`7232`, vì vậy câu trả lời lớn nhất là`32767`. Vì phép nhân bị rút gọn theo modulo`32768`, việc triển khai khớp chính xác với mô hình số học của bài toán. 

Bây giờ hãy xem xét các phụ thuộc đệ quy:```
3
int f(int n){
if(n==0)return 1;
return f(n-1)+1;
}
```Mảng DP điền theo thứ tự sau:```
dp[0] = 1
dp[1] = 2
dp[2] = 3
```Khi tính toán`dp[2]`, lệnh gọi đệ quy`f(1)`chỉ đơn giản là đọc`dp[1]`, đã được hoàn thiện. Không có sự đệ quy lặp lại xảy ra. 

Cuối cùng, hãy xem xét nhiều đầu vào phù hợp:```
17
int f(int n){return 17;}
```Mọi`n`là hợp lệ. Thuật toán quét tất cả`32768`giá trị và liên tục cập nhật câu trả lời mỗi khi có kết quả phù hợp khác xuất hiện. Câu trả lời được lưu trữ cuối cùng trở thành`32767`, đó chính xác là những gì tuyên bố yêu cầu.
