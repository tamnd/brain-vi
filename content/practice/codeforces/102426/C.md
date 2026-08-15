---
title: "CF 102426C - LytchenLovesJSON"
description: "Nhiệm vụ cơ bản là triển khai một trình thông dịch JSON nhỏ. Đầu vào bắt đầu bằng một tài liệu JSON hợp lệ có gốc luôn là một đối tượng. Tài liệu có thể chứa các đối tượng lồng nhau, mảng, chuỗi, số, boolean và null."
date: "2026-08-14T15:23:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "C"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 175
verified: true
draft: false
---

[CF 102426C - LytchenLovesJSON](https://codeforces.com/problemset/problem/102426/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ cơ bản là triển khai một trình thông dịch JSON nhỏ. 

Đầu vào bắt đầu bằng một tài liệu JSON hợp lệ có gốc luôn là một đối tượng. Tài liệu có thể chứa các đối tượng lồng nhau, mảng, chuỗi, số, boolean và`null`. Định dạng của nó là tùy ý, vì vậy không thể sử dụng khoảng trắng và ngắt dòng để xác định vị trí kết thúc của các giá trị riêng lẻ. 

Sau tài liệu JSON, mỗi dòng còn lại là một truy vấn. Một truy vấn mô tả một đường dẫn qua biểu đồ đối tượng. Một cái tên như`birthday.year`có nghĩa là nhìn lên`birthday`trong đối tượng hiện tại và sau đó`year`trong đối tượng kết quả. Một hậu tố như`[3]`có nghĩa là lấy một phần tử mảng hoặc một ký tự từ một chuỗi. Một số thao tác chỉ mục có thể được xâu chuỗi, như trong`a[0][2]`. 

Đối với mỗi truy vấn, chúng tôi đạt được một giá trị và in nó ở định dạng đặc biệt mà câu lệnh yêu cầu hoặc báo cáo loại lỗi đầu tiên gặp phải. Một thuộc tính đối tượng bị thiếu sẽ tạo ra`Error: no such attribute`. Một chỉ mục được áp dụng cho bất cứ thứ gì ngoài chuỗi hoặc mảng sẽ tạo ra`Error: invalid type`. Một chỉ mục chuỗi hoặc mảng hợp lệ nằm ngoài phạm vi của nó tạo ra`Error: index overflow`. 

Tài liệu chứa tối đa 100 dòng, tối đa 100 ký tự, do đó kích thước văn bản của nó tối đa là khoảng 10.000 ký tự. Có tối đa 100 truy vấn, mỗi truy vấn có tối đa 100 ký tự. Các giới hạn này đủ nhỏ để thậm chí việc phân tích lại toàn bộ tài liệu cho mỗi truy vấn vẫn chỉ quét được khoảng một triệu ký tự đầu vào trong trường hợp xấu nhất. Lựa chọn kỹ thuật hữu ích vẫn là phân tích cú pháp một lần, vì đối tượng được phân tích cú pháp sau đó có thể được sử dụng lại bởi mọi truy vấn và việc triển khai trở nên rõ ràng hơn về mặt khái niệm. 

Ngữ pháp JSON cũng có nghĩa là cần phải có một trình mã thông báo chuỗi chung. Giá trị JSON có thể vượt qua nhiều dòng vật lý và khoảng trắng có thể xuất hiện giữa bất kỳ mã thông báo cấu trúc nào. Trình phân tích cú pháp dựa trên`split`, các biểu thức chính quy trên các dòng riêng lẻ hoặc các giả định về thụt lề có thể âm thầm thất bại. 

Các chuỗi cần được xử lý đặc biệt vì các chuỗi thoát của chúng là một phần của hành vi có thể quan sát được của vấn đề. Ví dụ, hãy xem xét:```
{"s":"a\\nb"}
s
s[1]
```Chuỗi chứa hai ký tự dấu gạch chéo ngược và`n`ở giữa chứ không phải là một dòng mới thực sự. Đầu ra cần thiết cho`s`là`a\nb`, với trình tự thoát được bảo toàn. Một trình phân tích cú pháp sử dụng một cách mù quáng bộ giải mã JSON thông thường của Python sẽ biến thành`\n`vào một dòng mới và sẽ tạo ra sự trình bày sai. 

Trường hợp tinh tế thứ hai là một trích dẫn đã thoát:```
{"s":"a\"b"}
s
s[1]
```Giá trị là`a"b`, vì vậy đầu ra là`a"b`và ký tự được lập chỉ mục là`"`. Việc coi trích dẫn là phần cuối của chuỗi sẽ làm hỏng phân tích cú pháp. 

Một chỉ mục cũng có thể được áp dụng cho một chuỗi thay vì một mảng:```
{"s":"abc"}
s[0]
s[3]
```Đầu ra đúng là:```
a
Error: index overflow
```Việc thực hiện bất cẩn bằng cách sử dụng`if index > len(s)`thay vì`if index >= len(s)`sẽ chấp nhận sai`s[3]`. 

Cuối cùng, việc tra cứu thuộc tính có thể tuân theo một nguyên hàm được lập chỉ mục:```
{"a":[10]}
a[0].x
```Kết quả là:```
Error: no such attribute
```Sau đó`a[0]`giá trị hiện tại là số`10`, không có thuộc tính đối tượng. Điều này khác với việc áp dụng chỉ mục cho chính con số đó.`invalid type`lỗi. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là phân tích tài liệu JSON và trả lời ngay từng truy vấn. Việc triển khai đơn giản thậm chí có thể phân tích cú pháp toàn bộ tài liệu JSON một cách độc lập cho mọi truy vấn. Vì tài liệu có tối đa 10.000 ký tự và có tối đa 100 truy vấn nên phương pháp đó thực hiện tối đa khoảng 1.000.000 thao tác phân tích cấp ký tự, cộng với xử lý truy vấn. Dưới những ràng buộc này, nó thực sự đủ nhanh. 

Điểm yếu của cách tiếp cận đó là công việc lặp đi lặp lại. Mọi truy vấn đều bắt đầu từ cùng một đối tượng gốc, do đó việc xây dựng lại cùng một biểu đồ đối tượng lên đến 100 lần không mang lại lợi ích về mặt thuật toán. 

Quan sát hữu ích là tài liệu JSON không thể thay đổi trong suốt quá trình nhập. Khi nó đã được chuyển đổi thành một cây gồm các đối tượng, mảng và giá trị nguyên thủy, mọi truy vấn chỉ đơn giản là đi qua cùng một cây đó. Chúng ta có thể phân tích cú pháp tài liệu chính xác một lần, giữ từng đối tượng dưới dạng từ điển và mỗi mảng dưới dạng danh sách, sau đó diễn giải từng truy vấn dựa trên cấu trúc được lưu trữ. 

Bản thân trình phân tích cú pháp là một trình phân tích cú pháp gốc đệ quy. JSON có cấu trúc đệ quy đặc biệt thuận tiện: một đối tượng chứa các cặp khóa-giá trị, một mảng chứa các giá trị và một giá trị có thể đệ quy là một đối tượng hoặc mảng khác. Mỗi hàm phân tích cú pháp sử dụng các ký tự từ một vị trí được chia sẻ và trả về cả giá trị được phân tích cú pháp và vị trí mới. 

Phần không chuẩn duy nhất là xử lý chuỗi. Thay vì yêu cầu thư viện JSON của Python giải mã chuỗi, trình phân tích cú pháp sẽ tự xử lý. Những cuộc trốn thoát`\t`,`\\`,`\/`, Và`\"`được chuyển đổi thành các ký tự tương ứng của chúng, trong khi các chuỗi thoát khác vẫn giữ nguyên chữ vì đặc tả đầu ra yêu cầu chúng phải được giữ nguyên. Điều này cũng cung cấp cho việc lập chỉ mục chuỗi sự biểu diễn chính xác mà vấn đề mong đợi. 

Hai cách tiếp cận có thể được so sánh như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân tích lại JSON cho mọi truy vấn | O(QS + QL) | O(S) | Được chấp nhận theo những ràng buộc này | 
| Phân tích một lần, trả lời các truy vấn trên cây | O(S + QL) | O(S) | Được chấp nhận, ưa thích | 

Đây`S`là độ dài tài liệu JSON,`Q`là số lượng truy vấn và`L`là độ dài truy vấn tối đa. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng đầu vào và nối chúng thành một luồng ký tự. Chúng tôi không thể quyết định tài liệu JSON kết thúc ở đâu bằng cách xem các dòng vì tài liệu có thể chứa định dạng tùy ý và ngắt dòng. 
2. Phân tích giá trị gốc bằng trình phân tích cú pháp JSON gốc đệ quy. Trình phân tích cú pháp bỏ qua khoảng trắng JSON trước mỗi giá trị, sau đó gửi đi theo ký tự đầu tiên. MỘT`{`bắt đầu một đối tượng,`[`bắt đầu một mảng,`"`bắt đầu một chuỗi,`t`hoặc`f`bắt đầu một boolean,`n`bắt đầu`null`và một dấu hiệu hoặc chữ số bắt đầu một số. 
3. Biểu thị mọi giá trị được phân tích cú pháp dưới dạng một cặp chứa loại và dữ liệu của nó. Các đối tượng lưu trữ một từ điển từ khóa đến giá trị, mảng lưu trữ danh sách các giá trị, chuỗi lưu trữ biểu diễn ký tự đã xử lý của chúng, số lưu trữ các giá trị dấu phẩy động và các boolean nguyên thủy và`null`được lưu trữ trực tiếp. 
4. Sau khi phân tích đối tượng gốc, sử dụng vị trí ký tự hiện tại của trình phân tích cú pháp để tìm các dòng truy vấn còn lại. Điều này an toàn hơn việc cố gắng xác định dòng JSON cuối cùng bằng cách sử dụng dấu ngoặc nhọn hoặc thụt lề vì trình phân tích cú pháp đã biết chính xác giá trị gốc kết thúc ở đâu. 
5. Chia từng truy vấn tại`.`để có được các phân đoạn truy cập thuộc tính của nó. Trong mỗi phân đoạn, trước tiên hãy đọc tên thuộc tính theo thứ tự chữ cái và sau đó đọc từng phân đoạn.`[index]`hậu tố gắn liền với nó. 
6. Bắt đầu mọi truy vấn với đối tượng gốc. Đối với mỗi tên thuộc tính, hãy kiểm tra xem giá trị hiện tại có phải là một đối tượng hay không và liệu khóa được yêu cầu có tồn tại hay không. Nếu một trong hai điều kiện không thành công, hãy in`Error: no such attribute`và ngừng xử lý truy vấn đó. 
7. Sau khi lấy thành công giá trị thuộc tính, xử lý các thao tác chỉ mục của nó từ trái sang phải. Chỉ mục chỉ hợp lệ khi giá trị hiện tại là một chuỗi hoặc một mảng. Nếu không thì in`Error: invalid type`. 
8. Đối với một chuỗi hoặc mảng hợp lệ, hãy so sánh chỉ mục được yêu cầu với độ dài của nó. Một chỉ mục là hợp pháp chính xác khi`0 <= index < length`. Nếu nó nằm ngoài phạm vi đó, hãy in`Error: index overflow`; nếu không thì thay thế giá trị hiện tại bằng phần tử đã chọn. 
9. Khi truy vấn hoàn chỉnh đã được sử dụng, hãy tuần tự hóa giá trị kết quả theo loại của nó. Các số sử dụng ký hiệu dấu phẩy cố định với hai chữ số sau dấu thập phân. Mảng và đối tượng được tuần tự hóa theo cách đệ quy và các khóa đối tượng được sắp xếp theo từ điển trước khi cặp khóa-giá trị của chúng được in. 

Bất biến trong suốt quá trình đánh giá truy vấn là`current`chính xác là giá trị JSON mà tiền tố của truy vấn đã xử lý cho đến nay đạt được. Xử lý thuộc tính thay đổi nó thành phần tử con tương ứng của đối tượng hiện tại, trong khi xử lý chỉ mục thay đổi nó thành phần tử hoặc ký tự tương ứng. Vì mỗi thao tác chỉ được thực hiện sau khi kiểm tra loại và giới hạn hiện tại nên mọi chuyển đổi thành công đều là một cạnh hợp lệ trong biểu đồ đối tượng JSON. Nếu một thao tác không thể thực hiện được, lỗi được báo cáo sẽ tương ứng chính xác với thao tác không hợp lệ đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Parser:
    def __init__(self, text):
        self.s = text
        self.n = len(text)
        self.i = 0

    def skip_ws(self):
        while self.i < self.n and self.s[self.i] in " \t\r\n":
            self.i += 1

    def parse(self):
        self.skip_ws()
        c = self.s[self.i]

        if c == '{':
            return self.parse_object()
        if c == '[':
            return self.parse_array()
        if c == '"':
            return ("string", self.parse_string())
        if c == 't':
            self.i += 4
            return ("bool", True)
        if c == 'f':
            self.i += 5
            return ("bool", False)
        if c == 'n':
            self.i += 4
            return ("null", None)

        return self.parse_number()

    def parse_string(self):
        self.i += 1
        result = []

        while True:
            c = self.s[self.i]
            self.i += 1

            if c == '"':
                return ''.join(result)

            if c != '\\':
                result.append(c)
                continue

            esc = self.s[self.i]
            self.i += 1

            if esc == 't':
                result.append('\t')
            elif esc == '\\':
                result.append('\\')
            elif esc == '/':
                result.append('/')
            elif esc == '"':
                result.append('"')
            else:
                # The statement requires other escape sequences
                # to be kept as written.
                result.append('\\')
                result.append(esc)

    def parse_number(self):
        start = self.i

        if self.s[self.i] == '-':
            self.i += 1

        while self.i < self.n and self.s[self.i].isdigit():
            self.i += 1

        if self.i < self.n and self.s[self.i] == '.':
            self.i += 1
            while self.i < self.n and self.s[self.i].isdigit():
                self.i += 1

        if self.i < self.n and self.s[self.i] in 'eE':
            self.i += 1
            if self.s[self.i] in '+-':
                self.i += 1
            while self.i < self.n and self.s[self.i].isdigit():
                self.i += 1

        return ("number", float(self.s[start:self.i]))

    def parse_object(self):
        self.i += 1
        obj = {}
        self.skip_ws()

        if self.s[self.i] == '}':
            self.i += 1
            return ("object", obj)

        while True:
            self.skip_ws()
            key = self.parse_string()

            self.skip_ws()
            self.i += 1  # ':'

            value = self.parse()
            obj[key] = value

            self.skip_ws()
            if self.s[self.i] == '}':
                self.i += 1
                return ("object", obj)

            self.i += 1  # ','

    def parse_array(self):
        self.i += 1
        arr = []
        self.skip_ws()

        if self.s[self.i] == ']':
            self.i += 1
            return ("array", arr)

        while True:
            arr.append(self.parse())
            self.skip_ws()

            if self.s[self.i] == ']':
                self.i += 1
                return ("array", arr)

            self.i += 1  # ','

def format_value(value):
    typ, data = value

    if typ == "bool":
        return "true" if data else "false"

    if typ == "number":
        return f"{data:.2f}"

    if typ == "string":
        return data

    if typ == "null":
        return "null"

    if typ == "array":
        return "[ " + ", ".join(format_value(x) for x in data) + " ]"

    # object
    items = []
    for key in sorted(data):
        items.append(f'"{key}": {format_value(data[key])}')
    return "{ " + ", ".join(items) + " }"

def answer_query(root, query):
    current = root

    for part in query.split('.'):
        p = 0

        while p < len(part) and part[p].isalpha():
            p += 1

        key = part[:p]

        if current[0] != "object" or key not in current[1]:
            return "Error: no such attribute"

        current = current[1][key]

        while p < len(part):
            # part[p] must be '[' because the input is guaranteed valid.
            p += 1
            start = p

            while p < len(part) and part[p].isdigit():
                p += 1

            index = int(part[start:p])
            p += 1  # ']'

            if current[0] not in ("string", "array"):
                return "Error: invalid type"

            if index >= len(current[1]):
                return "Error: index overflow"

            if current[0] == "string":
                current = ("string", current[1][index])
            else:
                current = current[1][index]

    return format_value(current)

def main():
    lines = []
    while True:
        line = input()
        if not line:
            break
        lines.append(line)

    text = ''.join(lines)

    parser = Parser(text)
    root = parser.parse()

    # The parser stops exactly after the JSON document.
    rest = text[parser.i:]

    queries = rest.splitlines()
    out = []

    for query in queries:
        query = query.strip()
        if query:
            out.append(answer_query(root, query))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    main()
```các`Parser`lớp duy trì một con trỏ,`self.i`, vào luồng đầu vào hoàn chỉnh. Mọi hàm phân tích cú pháp đều tiêu thụ chính xác các ký tự thuộc giá trị của nó. Các cuộc gọi đệ quy là những gì xử lý việc lồng nhau một cách tự nhiên, do đó, một đối tượng chứa một mảng chứa một đối tượng khác không yêu cầu logic độ sâu trong trường hợp đặc biệt.`parse_string`đáng được quan tâm đặc biệt. Bốn dạng thoát mà các quy tắc đầu ra quan tâm rõ ràng sẽ được chuyển đổi thành các ký tự thực tế của chúng. Các lối thoát khác được giữ lại với dấu gạch chéo ngược hàng đầu của chúng. Đặc biệt,`\u25A0`vẫn là chuỗi sáu ký tự`\u25A0`, khớp với hành vi đầu ra được yêu cầu thay vì hành vi giải mã Unicode của Python.`parse_number`sử dụng dấu tùy chọn, phần nguyên, phần phân số và số mũ một cách độc lập. Đầu vào được đảm bảo chứa JSON hợp lệ, do đó trình phân tích cú pháp không cần xác thực mọi trường hợp số không đúng định dạng. 

Người đánh giá truy vấn có chủ ý kiểm tra thuộc tính trước khi xử lý dấu ngoặc của nó. Một truy vấn như`missing[0]`phải báo cáo thuộc tính bị thiếu thay vì loại chỉ mục không hợp lệ, vì không có giá trị nào mà chỉ mục có thể hoạt động. 

Việc kiểm tra ranh giới chỉ mục sử dụng`index >= len(current[1])`. Vì các chỉ số được đảm bảo không âm nên không có trường hợp giới hạn dưới riêng biệt nào. Bản thân Python có tính năng lập chỉ mục phủ định, do đó việc loại bỏ rõ ràng các chỉ mục phủ định sẽ là cần thiết ở định dạng đầu vào ít hạn chế hơn, nhưng ở đây mọi chỉ mục truy vấn đều không âm. 

Bộ tuần tự hóa định dạng đệ quy các mảng và đối tượng. Các khóa đối tượng được sắp xếp tại thời điểm đầu ra, vì thứ tự đầu vào không nhất thiết phải là thứ tự đầu ra được yêu cầu. Các giá trị số được định dạng bằng`:.2f`, cung cấp chính xác hai chữ số sau dấu thập phân. 

## Ví dụ đã hoạt động 

Tuyên bố được cung cấp có chứa một mẫu lớn. Đoạn trích trong lời nhắc dường như bị mất hoặc bị hỏng một phần mẫu xung quanh bản cuối cùng`teammates`các truy vấn, vì vậy dấu vết sau đây sử dụng phần rõ ràng của mẫu đó. 

Đối với truy vấn`grades[4][1]`, phần có liên quan của tài liệu là:```
"grades": [90, 80, 88, 100, [55, 80]]
```Việc đánh giá tiến hành như sau. 

| Bước | Hoạt động | Giá trị hiện tại | 
| --- | --- | --- | 
| 1 | Bắt đầu từ thư mục gốc | đối tượng | 
| 2 | Truy cập`grades`|`[90, 80, 88, 100, [55, 80]]`| 
| 3 | Áp dụng`[4]`|`[55, 80]`| 
| 4 | Áp dụng`[1]`|`80`| 
| 5 | Số định dạng |`80.00`| 

Bất biến chính được hiển thị ở đây: sau mỗi thao tác,`current`chính xác là giá trị được mô tả bởi tiền tố được xử lý của truy vấn. Chỉ mục thứ hai hoạt động trên mảng thu được từ chỉ mục đầu tiên, không phải trên mảng gốc`grades`mảng. 

Đối với ví dụ thứ hai, hãy xem xét:```
{
"a": {
"z": 1,
"x": [10, 20]
},
"s": "abc"
}
a.x[1]
s[2]
a.x[2]
a.x[0].missing
```Truy vấn đầu tiên đi theo một cạnh đối tượng, một cạnh đối tượng khác, một cạnh mảng và cuối cùng đạt đến số`20`. 

| Bước | Hoạt động | Giá trị hiện tại | 
| --- | --- | --- | 
| 1 | Bắt đầu từ thư mục gốc | đối tượng | 
| 2 | Truy cập`a`| đối tượng | 
| 3 | Truy cập`x`|`[10, 20]`| 
| 4 | Áp dụng`[1]`|`20`| 
| 5 | Định dạng |`20.00`| 

Vì`s[2]`, chuỗi được lập chỉ mục trực tiếp và tạo ra`c`. Vì`a.x[2]`, mảng có độ dài bằng hai, vì vậy chỉ mục`2`nằm ngoài phạm vi pháp luật`0`bởi vì`1`, sản xuất`Error: index overflow`. Vì`a.x[0].missing`, chỉ mục thành công và để lại giá trị hiện tại là số`10`; tra cứu thuộc tính sau đây không thể tìm thấy thuộc tính đối tượng và tạo ra`Error: no such attribute`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S + QL + S log S) | Chi phí phân tích cú pháp O(S), chi phí truy vấn O(QL) và sắp xếp các khóa đối tượng trong quá trình tuần tự hóa có chi phí tối đa là O(S log S) trên các đối tượng được lưu trữ | 
| Không gian | O(S) | Cây JSON được phân tích cú pháp lưu trữ các giá trị, khóa, mảng và đối tượng lồng nhau của tài liệu | 

Đây`S`nhiều nhất là khoảng 10.000 ký tự,`Q`nhiều nhất là 100 và`L`tối đa là 100. Ngay cả phương pháp phân tích cú pháp lặp lại cũng chỉ quét khoảng một triệu ký tự tài liệu, trong khi cách triển khai đã chọn sẽ phân tích cú pháp một lần và sử dụng lại cây kết quả. Việc sử dụng bộ nhớ thoải mái dưới 128 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`main`điểm vào. Trình trợ giúp gọi chương trình chính xác đó, giúp các xác nhận kiểm tra cùng một trình phân tích cú pháp, trình đánh giá truy vấn và trình tuần tự hóa được sử dụng để gửi.```python
import subprocess
import sys

def run(inp: str) -> str:
    result = subprocess.run(
        [sys.executable, "solution.py"],
        input=inp,
        text=True,
        capture_output=True,
        check=True
    )
    return result.stdout

# Provided sample core
sample1 = r'''{
"name": "Lchen",
"gender": false,
"height": 1.60e+2,
"birthday": {
"year": 2000,
"month": 1,
"day": 1,
"aggregate": [2000, 1, 1]
},
"grades": [90, 80, 88, 100, [55, 80]],
"laboratory": null
}
name
name[0]
name.gender
gender
gender[1]
height
birthday.year
grades[2]
grades[4]
grades[4][1]
laboratory
grades[5]
'''

assert run(sample1) == (
    "Lchen\n"
    "L\n"
    "Error: no such attribute\n"
    "false\n"
    "Error: invalid type\n"
    "160.00\n"
    "2000.00\n"
    "88.00\n"
    "[ 55.00, 80.00 ]\n"
    "80.00\n"
    "null\n"
    "Error: index overflow"
), "provided sample core"

# Custom 1: minimum-size object, missing attribute, invalid index type.
case1 = '''{"a":0}
a
b
a[0]
'''
assert run(case1) == (
    "0.00\n"
    "Error: no such attribute\n"
    "Error: invalid type"
), "minimum object and error types"

# Custom 2: nested arrays and string escape handling.
case2 = r'''{
"a": [[1, 2], []],
"s": "A\\B"
}
a[0][1]
a[1][0]
s[1]
'''
assert run(case2) == (
    "2.00\n"
    "Error: index overflow\n"
    "\\"
), "nested indexing and backslash"

# Custom 3: boundary index, object key sorting, exponent and negative number.
case3 = '''{
"z": 3,
"a": {
"y": 2,
"x": [-12.5e0, 3]
}
}
a
a.x[0]
a.x[2]
z
'''
assert run(case3) == (
    '{ "x": [ -12.50, 3.00 ], "y": 2.00 }\n'
    "-12.50\n"
    "Error: index overflow\n"
    "3.00"
), "sorting, exponent and upper-bound index"

# Custom 4: maximum number of JSON lines and maximum number of queries.
keys = [chr(ord('a') + i) for i in range(26)]
keys += ['a' + chr(ord('a') + i) for i in range(26)]
keys += ['b' + chr(ord('a') + i) for i in range(26)]
keys += ['c' + chr(ord('a') + i) for i in range(20)]

json_lines = ['{']
for i, key in enumerate(keys):
    json_lines.append(f'"{key}": 7' + (',' if i + 1 < len(keys) else ''))
json_lines.append('}')

# Add enough repeated queries to reach the 100-query limit.
queries = [keys[i % len(keys)] for i in range(100)]
max_case = '\n'.join(json_lines + queries) + '\n'

expected = ''.join("7.00\n" for _ in range(100)).rstrip('\n')
assert run(max_case) == expected, "maximum query count and large document"
```Các trường hợp tùy chỉnh thực hiện các dạng hư hỏng và đặc tính cấu trúc khác nhau. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`{"a":0}`với`a`,`b`,`a[0]`|`0.00`, thuộc tính bị thiếu, loại không hợp lệ | Đối tượng có kích thước tối thiểu và mức độ ưu tiên lỗi | 
| Mảng lồng nhau với`"A\\B"`|`2.00`, tràn,`\`| Nhiều chỉ số và xử lý thoát chuỗi | 
| Đối tượng lồng nhau với`[-12.5e0,3]`| Đối tượng được sắp xếp,`-12.50`, tràn,`3.00`| Phân tích số, tuần tự hóa, sắp xếp khóa, ranh giới trên | 
| Tài liệu 100 dòng với 100 truy vấn | 100 dòng`7.00`| Số lượng truy vấn tối đa và xử lý tài liệu lớn | 

## Vỏ cạnh 

Đối với một chuỗi thoát, hãy xem xét:```
{"s":"a\"b"}
s
s[1]
```Trình phân tích cú pháp nhập chuỗi sau dấu ngoặc kép đầu tiên. Khi nó nhìn thấy`\"`, nó sử dụng cả hai ký tự và thêm một trích dẫn theo nghĩa đen thay vì kết thúc chuỗi. Giá trị được lưu trữ là`a"b`, vì vậy truy vấn đầu tiên in`a"b`và bản in thứ hai`"`. Trình phân tích cú pháp tìm kiếm trích dẫn thô tiếp theo sẽ chấm dứt chuỗi quá sớm. 

Đối với chuỗi thoát vẫn phải ở dạng văn bản, hãy xem xét:```
{"s":"x\u25A0y"}
s
```Trình phân tích cú pháp nhìn thấy`\u`, nhận ra rằng đó không phải là một trong bốn dạng thoát cần được giải mã và lưu trữ cả dấu gạch chéo ngược và`u`, theo sau là các chữ số còn lại là ký tự thông thường. Kết quả đầu ra là`x\u25A0y`. sử dụng`json.loads`trực tiếp thay vào đó sẽ tạo ra một ký tự ô vuông đen Unicode và sẽ vi phạm hành vi đầu ra được chỉ định. 

Để lập chỉ mục chuỗi, hãy xem xét:```
{"s":"abc"}
s[0]
s[2]
s[3]
```Hai truy vấn đầu tiên chọn`a`Và`c`. Người thứ ba nhìn thấy`index == len(s)`, thế là nó báo`Error: index overflow`. Điều kiện tương tự áp dụng cho mảng. Khoảng pháp luật nửa mở,`[0, length)`. 

Đối với các loại chỉ mục không hợp lệ, hãy xem xét:```
{"x":false,"y":null,"z":{"a":1}}
x[0]
y[0]
z[0]
```Cả ba truy vấn đều tạo ra`Error: invalid type`. Boolean,`null`và các giá trị đối tượng không thể lập chỉ mục được. Trình phân tích cú pháp không cố gắng diễn giải các hoạt động dành riêng cho Python như lập chỉ mục từ điển hoặc boolean. 

Đối với một thuộc tính bị thiếu sau khi lập chỉ mục thành công, hãy xem xét:```
{"a":[10]}
a[0].x
```

`a`giải quyết thành một mảng,`[0]`giải quyết theo số`10`, Và`.x`sau đó yêu cầu một thuộc tính của số đó. Vì chỉ có đối tượng mới có thuộc tính nên kết quả đúng là`Error: no such attribute`. Việc triển khai sẽ kiểm tra loại giá trị hiện tại trước khi tìm trong từ điển của nó. 

Đối với mảng lồng nhau, hãy xem xét:```
{"a":[[[7]]]}
a[0][0][0]
```Chỉ mục đầu tiên thay đổi giá trị hiện tại từ mảng ngoài sang mảng giữa, chỉ mục thứ hai thay đổi nó thành mảng bên trong và chỉ mục thứ ba đạt tới`7`, được in dưới dạng`7.00`. Xử lý dấu ngoặc một cách tuần tự là điều làm cho số lượng chỉ mục được xâu chuỗi tùy ý hoạt động mà không có trường hợp đặc biệt. 

Để định dạng đối tượng, hãy xem xét:```
{"z":1,"a":2}
a
```Đầu ra là:```
{ "a": 2.00, "z": 1.00 }
```Thứ tự đầu vào không liên quan. Trình tuần tự hóa sắp xếp các khóa từ điển trước khi xây dựng đầu ra, điều này là cần thiết vì thứ tự đối tượng JSON trong đầu vào không được đảm bảo khớp với thứ tự từ điển cần thiết.
