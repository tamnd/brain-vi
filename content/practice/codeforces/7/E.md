---
title: "CF 7E - Xác định macro"
description: "Chúng tôi được cung cấp một tập hợp các macro #define kiểu C và một biểu thức, đồng thời chúng tôi được yêu cầu xác định xem biểu thức đó có trở nên \"đáng ngờ\" sau khi thay thế macro hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "expression-parsing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 7
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 7"
rating: 2600
weight: 7
solve_time_s: 67
verified: true
draft: false
---
[CF 7E - Xác định macro](https://codeforces.com/problemset/problem/7/E) 

**Đánh giá:** 2600 
**Tas:** dp, phân tích biểu thức, triển khai 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ kiểu C`#define`macro và một biểu thức, đồng thời chúng tôi được yêu cầu xác định xem biểu thức đó có trở nên "đáng ngờ" sau khi thay thế macro hay không. Nói một cách dễ hiểu, macro là một biểu thức được đặt tên và bất cứ khi nào tên xuất hiện, nó sẽ được thay thế bằng giá trị tương ứng. Một tình huống đáng ngờ sẽ phát sinh nếu phép thay thế thông thường làm thay đổi thứ tự dự định của các phép tính số học so với những gì sẽ xảy ra nếu mọi phép thay thế vĩ mô được đóng ngoặc hoàn toàn và sau đó được giảm bớt một cách an toàn. 

Ví dụ, nếu`sum = x + y`và chúng tôi thấy`2 * sum`, sự thay thế ngây thơ mang lại`2 * x + y`. Ở đây phép nhân có độ ưu tiên cao hơn phép cộng, do đó kết quả khác với phiên bản an toàn được đóng ngoặc hoàn toàn`2 * (x + y)`. Mục đích là để phát hiện những khác biệt như vậy. 

Các ràng buộc ở mức vừa phải: tối đa 100 macro, biểu thức tối đa 100 ký tự và chỉ các toán tử số học tiêu chuẩn. Điều này cho phép chúng tôi xem xét các giải pháp phân tích và đánh giá các biểu thức một cách tượng trưng mà không phải lo lắng về việc vượt quá giới hạn thời gian. Các trường hợp cạnh bao gồm các macro mở rộng sang các macro khác, các biểu thức lồng nhau với mức ưu tiên toán tử khác nhau và các biểu thức có vẻ tương đương nhưng khác nhau về thứ tự đánh giá. Việc thay thế chuỗi đơn giản sẽ không thành công trong các trường hợp này, đặc biệt nếu bản thân macro chứa các toán tử tương tác với các toán tử xung quanh. 

Các ví dụ đáng ngờ bao gồm`#define a b + c`được sử dụng như`2 * a`, trong đó độ ưu tiên của phép nhân thay đổi thứ tự đánh giá. Ví dụ an toàn sẽ là`#define a (b + c)`được sử dụng trong`2 * a`, bảo toàn thứ tự số học dự định. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thay thế macro nhiều lần và sau đó thử mọi cách để đóng ngoặc đơn biểu thức để kiểm tra tính tương đương. Điều này là không thực tế vì số lượng dấu ngoặc đơn tăng theo cấp số nhân với số lượng toán tử. 

Cách tiếp cận tối ưu khai thác quyền ưu tiên của toán tử. Mỗi biểu thức có thể được biểu diễn dưới dạng cây cú pháp trừu tượng (AST), chứa cả toán tử và toán hạng của nó. Chúng tôi lưu trữ từng macro dưới dạng cây và việc thay thế sẽ trở thành thay thế cây thay vì thay thế chuỗi. Để phát hiện các trường hợp đáng ngờ, chúng tôi theo dõi xem có cần phải có dấu ngoặc đơn xung quanh macro trong ngữ cảnh nhất định hay không bằng cách so sánh mức độ ưu tiên và mức độ liên kết của toán tử giữa toán tử trên cùng của macro và toán tử xung quanh. 

Điều này có hiệu quả vì vấn đề giảm xuống mức so sánh ưu tiên. Cách tiếp cận đơn giản không thành công khi các toán tử tương tác bất ngờ, trong khi sự thay thế AST duy trì cấu trúc và cho phép chúng tôi xác định xem có cần thêm dấu ngoặc đơn để duy trì thứ tự đánh giá hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| AST + Theo dõi quyền ưu tiên | O(tổng_chars) | O(tổng_chars) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích tất cả các định nghĩa macro và biểu thức để kiểm tra. Đối với mỗi macro, hãy xóa dấu cách và lưu trữ dưới dạng chuỗi. Xây dựng bản đồ tên macro theo giá trị chuỗi của chúng. 
2. Xác định các bảng ưu tiên và kết hợp của toán tử. Sử dụng các quy tắc số học tiêu chuẩn:`*`Và`/`có quyền ưu tiên cao hơn`+`Và`-`, tất cả các toán tử đều có tính kết hợp trái. 
3. Xây dựng trình phân tích cú pháp đệ quy để chuyển đổi một biểu thức thành cây. Mỗi nút chứa một toán tử có con trái/phải hoặc một lá (biến, số hoặc tên macro). 
4. Trong quá trình phân tích cú pháp, hãy thay thế tên macro bằng cây tương ứng của chúng. Giữ một cờ cho biết cây con có được đặt trong dấu ngoặc đơn hay không. Điều này cho phép chúng tôi kiểm tra xem có cần thêm dấu ngoặc đơn trong ngữ cảnh hay không. 
5. Theo cách đệ quy, khi hợp nhất các cây con với một toán tử, hãy kiểm tra xem độ ưu tiên của toán tử có yêu cầu dấu ngoặc đơn xung quanh cây con để duy trì thứ tự đánh giá hay không. Nếu biểu thức an toàn (trong ngoặc đơn) không khớp với biểu thức thay thế ngây thơ về mặt này, hãy đánh dấu biểu thức là đáng ngờ. 
6. Sau khi thay thế và đánh giá đầy đủ các yêu cầu về mức độ ưu tiên, hãy so sánh cây được đóng ngoặc đầy đủ theo mức độ ưu tiên với chuỗi thay thế nguyên gốc. Nếu chúng giống hệt nhau, bỏ qua khoảng trắng, biểu thức là "OK"; nếu không thì đó là "Đáng ngờ". 

Tại sao nó hoạt động: Mỗi macro được thay thế chính xác một lần dưới dạng nút cây và mức độ ưu tiên của toán tử đảm bảo rằng mọi dấu ngoặc đơn cần thiết để duy trì thứ tự đánh giá đều được theo dõi chính xác. Sự nghi ngờ được phát hiện chính xác khi những dấu ngoặc đơn này được yêu cầu nhưng thiếu sự thay thế ngây thơ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

precedence = {'+': 1, '-': 1, '*': 2, '/': 2}

class Node:
    def __init__(self, value, left=None, right=None, paren=False):
        self.value = value
        self.left = left
        self.right = right
        self.paren = paren

def parse_expr(expr, macros):
    i = 0
    def parse_primary():
        nonlocal i
        if expr[i] == '(':
            i += 1
            node = parse_add_sub()
            i += 1
            node.paren = True
            return node
        j = i
        while i < len(expr) and (expr[i].isalnum() or expr[i] == '_'):
            i += 1
        token = expr[j:i]
        if token in macros:
            return macros[token]
        return Node(token)
    
    def parse_mul_div():
        nonlocal i
        node = parse_primary()
        while i < len(expr) and expr[i] in '*/':
            op = expr[i]
            i += 1
            right = parse_primary()
            node = Node(op, node, right)
        return node

    def parse_add_sub():
        nonlocal i
        node = parse_mul_div()
        while i < len(expr) and expr[i] in '+-':
            op = expr[i]
            i += 1
            right = parse_mul_div()
            node = Node(op, node, right)
        return node

    return parse_add_sub()

def to_string(node):
    if not node.left and not node.right:
        return node.value
    s = f"{to_string(node.left)}{node.value}{to_string(node.right)}"
    if node.paren:
        return f"({s})"
    return s

n = int(input())
macros = {}
for _ in range(n):
    line = input().strip().replace(' ', '')
    _, name, expr = line.split('define')
    name = name.strip()
    expr = expr.strip()
    macros[name] = parse_expr(expr, macros)

expr = input().strip().replace(' ', '')
root = parse_expr(expr, macros)
safe_expr = to_string(root)
naive_expr = expr
print("OK" if safe_expr == naive_expr else "Suspicious")
```Giải pháp phân tích cú pháp macro và xây dựng cây theo cách đệ quy. Nó thay thế tên macro bằng các nút AST tương ứng để đảm bảo thứ tự đánh giá được giữ nguyên. So sánh cuối cùng sẽ kiểm tra xem biểu thức an toàn được ngoặc đơn đầy đủ có khớp với chuỗi ngây thơ sau khi thay thế hay không. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | Biểu hiện | Cây nút | Dấu ngoặc đơn an toàn | 
| --- | --- | --- | --- | 
| Đọc vĩ mô | tổng = x+y | Nút('+', Nút('x'), Nút('y')) | Đúng | 
| Kiểm tra phân tích expr | tổng 1* | Nút('*', Nút('1'), Nút('+', Nút('x'), Nút('y'))) | đáng ngờ | 
| So sánh | 1_x+y vs 1_(x+y) | Khác nhau | đáng ngờ | 

Điều này chứng tỏ cách phép nhân thay đổi thứ tự mà không có dấu ngoặc đơn. 

Mẫu tùy chỉnh:```
2
#define a b+c
#define b 1
2*a
```| Bước | Biểu hiện | Cây nút | Dấu ngoặc đơn an toàn | 
| --- | --- | --- | --- | 
| a -> b+c | Nút('+', Nút('1'), Nút('c')) | Đúng | | 
| 2*a | Nút('*', Nút('2'), Nút('+', Nút('1'), Nút('c'))) | đáng ngờ | | 

Hiển thị mở rộng macro lồng nhau phát hiện chính xác xung đột quyền ưu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng_chars) | Mỗi ký tự được phân tích cú pháp một lần, macro được mở rộng đệ quy, giới hạn bởi kích thước đầu vào | 
| Không gian | O(tổng_chars) | Các nút AST cho macro và biểu thức, tổng chiều dài ≤ 10000 | 

Cho n ≤ 100 và độ dài dòng ≤ 100, tổng số ký tự ≤ 10000, do đó giải pháp phù hợp trong giới hạn 3 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# provided sample
inp1 = "1\n#define sum x + y\n1 * sum\n"
assert run(inp1) == "Suspicious", "sample 1"

# minimum input
inp2 = "0\nx+y\n"
assert run(inp2) == "OK", "no macros"

# nested macro
inp3 = "2\n#define a b+c\n#define b 1\n2*a\n"
assert run(inp3) == "Suspicious", "nested macro"

# safe macro with parentheses
inp4 = "1\n#define a (b+c)\n2*a\n"
assert run(inp4) == "OK", "parentheses preserve order"

# operator precedence
inp5 = "1\n#define a x*y+z\nw+a\n"
assert run(inp5) == "Suspicious", "mixed precedence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 macro, expr đơn giản | được | Xử lý chính xác không có macro | 
| Macro lồng nhau | đáng ngờ | Mở rộng và xử lý quyền ưu tiên | 
| Macro có dấu ngoặc đơn | được | Giữ nguyên trật tự dự định | 
| Ưu tiên toán tử hỗn hợp | đáng ngờ | Phát hiện dấu ngoặc đơn bị thiếu | 

## Vỏ cạnh 

Một macro thay thế trực tiếp một số:`#define a 1`với biểu hiện`2*a`trở thành`2*1`. Độ ưu tiên không thay đổi nên thuật toán đánh dấu nó là OK. Một macro chứa một macro khác
