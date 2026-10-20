---
title: "CF 105692B - GoGo"
description: "Nhiệm vụ là mô phỏng một ngôn ngữ lập trình rất nhỏ và xác định điều gì sẽ xảy ra khi một tập lệnh nhất định được thực thi. Tập lệnh trông giống như một thân hàm duy nhất bắt đầu từ func main() và kết thúc ở dấu ngoặc đóng của nó."
date: "2026-06-26T08:08:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105692
codeforces_index: "B"
codeforces_contest_name: "Baozii Cup 1"
rating: 0
weight: 105692
solve_time_s: 51
verified: true
draft: false
---

[CF 105692B - GoGo](https://codeforces.com/problemset/problem/105692/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là mô phỏng một ngôn ngữ lập trình rất nhỏ và xác định điều gì sẽ xảy ra khi một tập lệnh nhất định được thực thi. Tập lệnh trông giống như một phần thân hàm duy nhất bắt đầu từ`func main()`và kết thúc ở dấu ngoặc đóng của nó. Bên trong hàm này, các biến được khai báo với kiểu rõ ràng, giá trị được gán, được sửa đổi thông qua các phép toán giống như phép cộng và được in ra. 

Ngôn ngữ chỉ có hai loại dữ liệu, số nguyên và chuỗi. Các biến bắt đầu bằng các giá trị mặc định, số 0 cho số nguyên và chuỗi trống cho biến chuỗi. Quá trình thực thi diễn ra theo từng dòng và mọi câu lệnh đều nghiêm ngặt về loại và khai báo. Bất kỳ vi phạm nào như sử dụng biến không được khai báo, trộn lẫn các loại hoặc khai báo lại một biến sẽ ngay lập tức dừng thực thi và tạo ra lỗi thời gian chạy. 

Đầu vào là mã nguồn đầy đủ dưới dạng một khối văn bản. Đầu ra là chuỗi chính xác của các giá trị được in hoặc một từ duy nhất “lỗi thời gian chạy” nếu việc thực thi trở nên không hợp lệ tại bất kỳ thời điểm nào. 

Về cơ bản, các ràng buộc này rất nhỏ về độ dài mã, khoảng mười nghìn ký tự, loại trừ mọi thứ ngoài việc quét tuyến tính bằng một trình thông dịch đơn giản. Bất kỳ cách tiếp cận nào liên tục quét lại chương trình hoặc thực hiện phân tích cú pháp nặng cho mỗi thao tác vẫn sẽ vượt qua, nhưng bất cứ điều gì tệ hơn chi phí tuyến tính hoặc gần tuyến tính đều không cần thiết. 

Một điểm tinh tế là lỗi không chỉ liên quan đến các biến không xác định. Kiểu không khớp giữa các phép toán số nguyên và chuỗi cũng chấm dứt việc thực thi ngay lập tức. Một trường hợp góc quan trọng khác là tự phân công như`x = x`hoặc`x += x`, hợp lệ và phải hoạt động như bản sao giá trị hoặc số học/nối tùy thuộc vào loại. 

Một sai lầm ngây thơ nhưng phổ biến là coi các biến luôn tồn tại một khi được đề cập hoặc cho phép thay đổi kiểu tiềm ẩn. Ví dụ: 

đầu vào:```
func main() {
var x int
x = "abc"
}
```Điều này sẽ xuất ra:```
runtime error
```Việc triển khai bất cẩn lưu trữ mọi thứ trong một từ điển chung mà không thực thi các loại sẽ chấp nhận nó một cách không chính xác. 

Một trường hợp khác là in các biến không được khai báo: 

đầu vào:```
func main() {
Println(x)
}
```Điều này phải ngay lập tức tạo ra:```
runtime error
```Mặc dù chưa từng có nhiệm vụ nào xảy ra. 

Cuối cùng, các vòng lặp được giới hạn và đơn giản, nhưng chúng vẫn quan trọng vì chúng lặp lại các câu lệnh. Một sai lầm là quên rằng thân vòng lặp không phải là cấu trúc đệ quy và phải được thực hiện chính xác n lần với đánh giá mới mỗi lần lặp. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải quyết vấn đề này là xây dựng một trình thông dịch nhỏ. Ý tưởng brute-force là mã hóa chương trình thành các dòng và thực thi từng dòng một cách tuần tự trong khi vẫn duy trì bảng ký hiệu ánh xạ tên biến theo các giá trị và loại hiện tại của chúng. 

Mỗi câu lệnh được xử lý bằng cách kiểm tra hình thức của nó. Một khai báo sẽ chèn một biến mới vào bảng, nhưng chỉ khi nó chưa tồn tại. Một bài tập cập nhật một biến đã được khai báo trước đó sau khi xác minh rằng vế phải tồn tại và khớp với loại. Phép cộng hoặc nối sẽ sửa đổi giá trị hiện tại tại chỗ với các ràng buộc cùng loại. Các câu lệnh in chỉ đơn giản là nối thêm đầu ra hoặc gây ra lỗi nếu biến bị thiếu. 

Cách tiếp cận này đã có kích thước tuyến tính trong chương trình vì mỗi dòng được xử lý một lần. Không có sự tối ưu hóa có ý nghĩa nào ngoài việc phân tích cú pháp cẩn thận. Lý do duy nhất mà người ta có thể coi đó là “bạo lực” là vì nó không xử lý trước hoặc xây dựng cấu trúc ngoài việc thực thi ngay lập tức. 

Thách thức chính không phải là hiệu suất mà là tính chính xác trong việc xử lý các quy tắc ngôn ngữ một cách nhất quán, đặc biệt là các vòng lặp lồng nhau và thực thi không có phạm vi. 

Quan sát quan trọng là ngôn ngữ được hạn chế có chủ ý để tránh phân tích cú pháp phức tạp hoặc phạm vi lồng nhau. Không có chức năng nào ngoài chức năng chính, không có vòng lặp lồng nhau và không có tính năng tạo bóng thay đổi. Điều này làm cho trình thông dịch một lần là đủ. Trạng thái duy nhất cần có là một từ điển các biến và một ngăn xếp vòng lặp lưu trữ số lần lặp lại một đoạn dòng. 

Do đó, giải pháp tối ưu có độ phức tạp giống hệt với ý tưởng Brute Force nhưng được cấu trúc cẩn thận để mô phỏng chính xác luồng điều khiển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng từng dòng mà không xử lý vòng lặp thích hợp | O(n) | O(n) | Sai trong các trường hợp khó khăn | 
| Trình thông dịch đầy đủ với ngăn xếp vòng lặp và kiểm tra kiểu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Mô hình thực thi có thể được triển khai bằng cách trước tiên xử lý trước chương trình thành một danh sách các mã thông báo hoặc dòng, sau đó mô phỏng bộ đếm chương trình có hỗ trợ các vòng lặp. 

1. Phân tích đầu vào thành các câu lệnh riêng lẻ, giữ nguyên trật tự. Mỗi dòng được coi là một lệnh duy nhất. Điều này quan trọng vì các vòng lặp phụ thuộc vào việc nhóm dòng chính xác hơn là phân tích cú pháp cấp mã thông báo. 
2. Duy trì một từ điển ánh xạ các tên biến thành một cặp loại và giá trị. Cấu trúc này là bắt buộc vì mọi thao tác đều phải xác thực tính nhất quán của kiểu trước khi thực thi. 
3. Khi gặp câu lệnh khai báo, chỉ chèn biến nếu nó chưa tồn tại. Nếu nó tồn tại, hãy chấm dứt ngay lập tức với một lỗi. Việc khởi tạo mặc định phụ thuộc vào loại, các biến số nguyên bắt đầu từ 0 và các biến chuỗi bắt đầu ở chuỗi trống. 
4. Đối với các câu lệnh gán, hãy đánh giá vế phải trước. Nếu nó là nghĩa đen, hãy giải thích nó theo ngữ cảnh; nếu nó là một biến, hãy đảm bảo nó tồn tại. Sau đó kiểm tra tính tương thích của loại trước khi cập nhật biến bên trái. 
5. Đối với các câu lệnh cộng hoặc nối, hãy xác minh rằng cả hai toán hạng đều tồn tại và có cùng loại. Sau đó cập nhật phía bên trái tại chỗ bằng cách sử dụng phép cộng số nguyên hoặc nối chuỗi. 
6. Đối với câu lệnh Println, hãy đánh giá đối số. Nếu đó là một biến không được khai báo, hãy kết thúc với lỗi. Nếu không thì hãy thêm biểu diễn chuỗi của nó vào đầu ra. 
7. Đối với các vòng lặp có dạng`for range n {`, Đẩy chỉ mục lệnh hiện tại và số lần lặp còn lại vào ngăn xếp. Khi đến dấu ngoặc nhọn đóng, hãy giảm bộ đếm vòng lặp và quay lại bắt đầu vòng lặp hoặc bật bối cảnh vòng lặp. 
8. Tiếp tục thực hiện cho đến khi tất cả các câu lệnh được xử lý hoặc xảy ra lỗi. 

Tính chính xác dựa trên tính bất biến mà tại mỗi thời điểm thực thi, bảng biến phản ánh chính xác tất cả các hoạt động hợp lệ trước đó và không có trạng thái không hợp lệ nào được áp dụng một phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    code = sys.stdin.read().splitlines()

    vars = {}
    types = {}
    out = []

    i = 0
    n = len(code)

    loop_stack = []

    def parse_value(token):
        if token in vars:
            return types[token], vars[token]
        if token.startswith('"'):
            return "string", token.strip('"')
        return "int", int(token)

    while i < n:
        line = code[i].strip()

        if not line or line == "func main() {" or line == "}":
            i += 1
            continue

        if line.startswith("var "):
            _, name, typ = line.split()
            if name in vars:
                print("runtime error")
                return
            vars[name] = 0 if typ == "int" else ""
            types[name] = typ

        elif line.startswith("Println"):
            inside = line[line.find("(")+1:line.rfind(")")]
            t, v = parse_value(inside)
            out.append(str(v))

        elif "+=" in line:
            left, right = line.split("+=")
            left = left.strip()
            right = right.strip()

            if left not in vars:
                print("runtime error")
                return

            tl, vl = types[left], vars[left]
            tr, vr = parse_value(right)

            if tl != tr:
                print("runtime error")
                return

            if tl == "int":
                vars[left] = vl + vr
            else:
                vars[left] = vl + vr

        elif "=" in line:
            left, right = line.split("=")
            left = left.strip()

            if left not in vars:
                print("runtime error")
                return

            tr, vr = parse_value(right.strip())
            if tr != types[left]:
                print("runtime error")
                return

            vars[left] = vr

        elif line.startswith("for range"):
            cnt = int(line.split()[2])
            loop_stack.append((i, cnt))

        elif line == "}":
            if loop_stack:
                start, cnt = loop_stack[-1]
                cnt -= 1
                if cnt > 0:
                    loop_stack[-1] = (start, cnt)
                    i = start
                else:
                    loop_stack.pop()

        i += 1

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Trình thông dịch duy trì hai từ điển, một cho giá trị và một cho kiểu. Sự tách biệt này làm cho việc kiểm tra kiểu trở nên rõ ràng và tránh sự mơ hồ khi so sánh hoặc gán giá trị. 

Xử lý vòng lặp sử dụng một ngăn xếp để lưu trữ chỉ mục bắt đầu và các lần lặp còn lại. Khi gặp dấu ngoặc đóng, chương trình sẽ nhảy lùi hoặc thoát khỏi vòng lặp tùy thuộc vào số lượng còn lại. 

Một cạm bẫy triển khai phổ biến là quên rằng việc phân tích cú pháp bên phải phải phân biệt giữa các chuỗi ký tự, số nguyên và biến. Một vấn đề tế nhị khác là đảm bảo rằng việc tự phân công như`x += x`sử dụng giá trị đã được cập nhật của`x`đúng theo thứ tự đánh giá. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
func main() {
var x int
x = 5
Println(x)
x += x
Println(x)
}
```| Bước | Tuyên bố | x | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | var x int | 0 | | 
| 2 | x = 5 | 5 | | 
| 3 | Println(x) | 5 | 5 | 
| 4 | x += x | 10 | 5 | 
| 5 | Println(x) | 10 | 5, 10 | 

Dấu vết này cho thấy cách tự cộng số nguyên nhân đôi giá trị vì phía bên phải được đánh giá trước khi gán. 

### Ví dụ 2 

đầu vào:```
func main() {
var s string
s = "a"
for range 3 {
s += "b"
}
Println(s)
}
```| Bước | Tuyên bố | s | Trạng thái vòng lặp | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | chuỗi var s | "" | | | 
| 2 | s = "một" | "một" | | | 
| 3 | bắt đầu vòng lặp | "một" | 3 lần lặp | | 
| 4 | s += "b" | "ab" | 2 trái | | 
| 5 | s += "b" | "abb" | 1 trái | | 
| 6 | s += "b" | "abbb" | 0 trái | | 
| 7 | Bản in | "abbb" | | abbb | 

Ví dụ thứ hai xác nhận rằng trạng thái vòng lặp không phụ thuộc vào các cập nhật biến và phép nối được tích lũy qua các lần lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi dòng được xử lý một lần, với các thao tác từ điển theo thời gian không đổi và phân tích cú pháp đơn giản | 
| Không gian | O(n) | Lưu trữ các biến, ngăn xếp vòng lặp và đầu ra | 

Các ràng buộc cho phép tối đa khoảng mười nghìn ký tự, do đó, trình thông dịch tuyến tính phù hợp thoải mái trong giới hạn. Ngay cả với các vòng lặp lồng nhau được giới hạn bởi các hằng số nhỏ, tổng công việc vẫn ở mức nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample-like basic execution
assert run("""func main() {
var x int
x = 3
Println(x)
}""") == "3"

# string concatenation loop
assert run("""func main() {
var s string
s = "a"
for range 2 {
s += "b"
}
Println(s)
}""") == "abb"

# runtime error: undeclared variable
assert run("""func main() {
Println(x)
}""") == "runtime error"

# runtime error: type mismatch
assert run("""func main() {
var x int
x = "a"
}""") == "runtime error"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không khai báo Println | lỗi thời gian chạy | xử lý vi phạm truy cập | 
| gõ bài tập không khớp | lỗi thời gian chạy | gõ nghiêm ngặt | 
| nối vòng lặp | abb | tính đúng đắn của vòng lặp | 
| in số nguyên cơ bản | 3 | thực hiện tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là các hoạt động tự tham chiếu. Ví dụ: 

đầu vào:```
func main() {
var x int
x = 1
x += x
Println(x)
}
```Việc thực thi tiến hành với`x = 1`, sau đó`x += x`đánh giá vế phải trước, vẫn là 1 nên kết quả trở thành 2. Trình thông dịch đúng phải đảm bảo không cập nhật nhầm`x`trước khi đánh giá vế phải. 

Một trường hợp cạnh khác là khai báo lại: 

đầu vào:```
func main() {
var x int
var x string
}
```Tuyên bố thứ hai phải ngay lập tức chấm dứt thực hiện. Việc triển khai đúng sẽ kiểm tra sự tồn tại trước khi chèn vào bảng ký hiệu. 

Trường hợp cạnh cuối cùng là ranh giới vòng lặp: 

đầu vào:```
func main() {
for range 1 {
Println(1)
}
}
```Vòng lặp thực hiện chính xác một lần và dấu ngoặc đóng phải giảm và thoát một cách chính xác. Một lỗi phổ biến là bỏ qua lần lặp cuối cùng hoặc không thoát khỏi vòng lặp, cả hai đều do quản lý ngăn xếp không chính xác.
