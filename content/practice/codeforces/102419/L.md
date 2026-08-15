---
title: "CF 102419L – Phát hiện gian lận."
description: "Chúng tôi có hai chương trình được viết bằng một ngôn ngữ nhỏ với ba loại câu lệnh: xác định một biến, đọc một biến, in một biến và gán tổng của hai biến cho một biến khác."
date: "2026-08-12T20:38:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "L"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 769
verified: true
draft: false
---

[CF 102419L - Phát hiện gian lận.](https://codeforces.com/problemset/problem/102419/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chương trình được viết bằng một ngôn ngữ nhỏ với ba loại câu lệnh: xác định một biến, đọc một biến, in một biến và gán tổng của hai biến cho một biến khác. Một biến được xác định nhiều nhất một lần và mọi biến đều được xác định trước khi nó được sử dụng. 

Hai chương trình được coi là tương đương nếu chúng ta có thể đổi tên các biến trong chương trình đầu tiên sao cho mọi câu lệnh đều trở thành câu lệnh tương ứng trong chương trình thứ hai. Việc đổi tên là toàn cầu, vì vậy nếu`a`được đổi tên thành`x`, mỗi lần xuất hiện của`a`phải trở thành`x`. Thứ tự của các biến trong`define`các câu lệnh là một phần của chương trình và hai toán hạng của phép cộng cũng có tính vị trí. biểu hiện`a=b+c`không tương đương với`a=c+b`chỉ vì phép cộng có tính chất giao hoán về mặt toán học. 

Đầu vào chứa số dòng trong chương trình đầu tiên, theo sau là các dòng đó, sau đó là số dòng trong chương trình thứ hai, theo sau là các dòng đó. Mỗi chương trình có tối đa 1000 dòng. Nó đủ nhỏ để quét tuyến tính có thể dễ dàng đủ nhanh, nhưng nó cũng đủ lớn để việc thử mọi sự tương ứng có thể có giữa các biến là hoàn toàn không thực tế. Với tối đa 1000 biến riêng biệt, có thể có tới`1000!`có thể đổi tên. 

Trường hợp cạnh đầu tiên là độ dài chương trình khác nhau. Ví dụ,```
1
define a
2
define x
define y
```phải sản xuất`NO`. Việc đổi tên biến không thể chèn hoặc xóa các câu lệnh, vì vậy các chương trình có số dòng khác nhau không bao giờ có thể khớp. Việc triển khai bất cẩn chỉ so sánh các biến được sử dụng ở các vị trí tương ứng có thể bỏ qua điều này ngay lập tức. 

Trường hợp cạnh thứ hai là thứ tự của toán hạng. Coi như```
5
define a
define b
define c
a=b+c
print a
5
define x
define y
define z
x=z+y
print x
```Đầu ra đúng là`NO`. Ánh xạ duy nhất có thể có từ ba định nghĩa là`a -> x`,`b -> y`, Và`c -> z`. Dưới bản đồ đó,`a=b+c`trở thành`x=y+z`, không`x=z+y`. điều trị`+`vì giao hoán về mặt toán học sẽ chấp nhận cặp này một cách không chính xác. 

Trường hợp cạnh thứ ba là ánh xạ biến phải là một-một. Ví dụ,```
3
define a
define b
print a
3
define x
define x2
print x
```là`NO`. Chương trình đầu tiên yêu cầu`a -> x`, trong khi`b`phải ánh xạ tới một biến khác. Nếu quá trình triển khai chỉ lưu trữ ánh xạ từ tên chương trình đầu tiên sang tên chương trình thứ hai và không bao giờ kiểm tra hướng ngược lại, thì nó có thể vô tình cho phép hai biến khác nhau ánh xạ tới cùng một tên. 

Trường hợp hữu ích cuối cùng là khi thứ tự định nghĩa khác nhau. Ví dụ,```
5
define a
define b
define c
a=b+c
print a
5
define x
define z
define y
x=y+z
print x
```là`YES`, sử dụng`a -> x`,`b -> y`, Và`c -> z`. Bản thân các tên văn bản không có ý nghĩa. Điều quan trọng là liệu một lần đổi tên nhất quán có làm cho chuỗi câu lệnh hoàn chỉnh giống hệt nhau hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là thu thập tất cả các biến trong chương trình đầu tiên và tất cả các biến trong chương trình thứ hai, sau đó thử mọi phép đối chiếu giữa hai bộ. Đối với mỗi phép song song ứng cử viên, chúng tôi thay thế mọi biến trong chương trình đầu tiên và so sánh chương trình kết quả với chương trình thứ hai. Điều này đúng vì định nghĩa của gian lận chính xác là sự tồn tại của sự phản đối như vậy. 

Vấn đề là số lượng bijection. Nếu có`k`các biến riêng biệt, có`k!`những ánh xạ có thể có. Việc kiểm tra một bản đồ yêu cầu`O(n+m)`hoạt động, vì vậy tổng độ phức tạp là`O(k! (n+m))`. Với`k=1000`, thậm chí số lượng ứng cử viên còn lớn đến mức không thể tưởng tượng được, rất lâu trước khi việc so sánh dòng trở nên phù hợp. 

Điều quan trọng nhất là chính chương trình sẽ cho chúng ta biết những biến nào phải tương ứng. Chúng ta không cần phải đoán một bản đồ hoàn chỉnh trước tiên. Bất cứ khi nào chương trình đầu tiên đề cập đến một biến ở một vị trí câu lệnh nào đó, vị trí tương ứng trong chương trình thứ hai sẽ cho chúng ta biết biến đó phải ánh xạ tới. Khi sự tương ứng đó được thiết lập, mỗi lần xuất hiện sau đó của cùng một biến phải sử dụng chính xác cùng một mục tiêu. 

Chúng ta có thể thực thi điều này trực tiếp bằng hai từ điển. Một từ điển ánh xạ các biến từ chương trình đầu tiên sang các biến trong chương trình thứ hai. Từ điển ngược ánh xạ các biến từ chương trình thứ hai trở lại các biến trong chương trình đầu tiên. Khi so sánh một cặp lần xuất hiện biến, hoặc ánh xạ chưa được nhìn thấy trước đó và chúng tôi thiết lập nó hoặc nó đã được thiết lập và phải phù hợp với cặp hiện tại. Ánh xạ ngược ngăn không cho hai biến nguồn khác nhau được gán cho cùng một biến mục tiêu. 

Phương pháp brute-force hoạt động vì nó tìm kiếm rõ ràng tất cả các lần đổi tên có thể, nhưng không thành công vì có rất nhiều lần đổi tên theo giai thừa. Quan sát cho thấy mỗi lần xuất hiện tương ứng ngay lập tức hạn chế ánh xạ khả thi duy nhất cho phép chúng ta xây dựng phép đối chiếu cần thiết trong khi quét các chương trình một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(k! (n+m))`|`O(k+n+m)`| Quá chậm | 
| Ánh xạ hai chiều |`O(n+m)`|`O(k+n+m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cả hai chương trình hoàn chỉnh và từ chối chúng ngay lập tức nếu số dòng của chúng khác nhau. Việc đổi tên sẽ thay đổi tên nhưng không thể thay đổi cấu trúc chương trình. 
2. Phân tích từng dòng thành một lệnh và các vị trí biến đổi của nó. MỘT`define`,`read`, hoặc`print`dòng chứa một biến. Một phép gán chứa ba biến: đích, toán hạng bên trái và toán hạng bên phải. Bản thân lệnh không bao giờ được đổi tên. 
3. Tạo hai từ điển trống. Cái đầu tiên lưu trữ ánh xạ từ các biến trong chương trình đầu tiên đến các biến trong chương trình thứ hai. Cái thứ hai lưu trữ ánh xạ ngược. 
4. Quét các dòng tương ứng của hai chương trình từ trên xuống dưới. Nếu lệnh của họ khác nhau, hãy quay lại`NO`, bởi vì việc đổi tên biến không thể thay đổi một lệnh như`read`vào trong`print`. 
5. Với mỗi cặp vị trí biến tương ứng, hãy kiểm tra hai tên biến. Nếu biến đầu tiên đã có ánh xạ, hãy xác minh rằng nó ánh xạ tới biến thứ hai hiện tại. Nếu không được thì trả về`NO`. 
6. Nếu biến đầu tiên chưa được ánh xạ, hãy kiểm tra xem biến thứ hai đã được ánh xạ từ biến đầu tiên khác chưa. Nếu có thì quay lại`NO`. Nếu không thì thiết lập cả hai hướng của ánh xạ. 
7. Nếu mọi vị trí tương ứng đều vượt qua các bước kiểm tra này, hãy quay lại`YES`. Tại thời điểm đó, mỗi biến có một đối tác nhất quán duy nhất và bởi vì mọi lệnh và mọi vị trí biến đều khớp nhau nên việc đổi tên chương trình đầu tiên sẽ tạo ra chính xác chương trình thứ hai. 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau khi xử lý bất kỳ tiền tố nào của hai chương trình, hai từ điển sẽ mô tả việc đổi tên biến một-một hợp lệ cho toàn bộ tiền tố đó. Khi gặp một cặp biến mới, ánh xạ hiện tại phải phù hợp với nó, trong khi ánh xạ mới chỉ có thể được đưa ra nếu mục tiêu của nó chưa được gán cho biến nguồn khác. Do đó, bất biến được bảo toàn sau mỗi lần biến xuất hiện. 

Nếu thuật toán từ chối, thì cấu trúc chương trình khác hoặc một số biến sẽ cần hai tên khác nhau hoặc hai biến khác nhau sẽ cần cùng một tên. Không có tình huống nào trong số đó có thể được khắc phục bằng cách đổi tên toàn cục khác, vì vậy các chương trình không thể tương đương. 

Nếu thuật toán đến cuối, mọi sự tương ứng biến được sử dụng bởi một trong hai chương trình đều nhất quán và một-một. Việc thay thế mọi biến chương trình đầu tiên bằng biến chương trình thứ hai được ánh xạ của nó sẽ làm cho mọi câu lệnh tương ứng giống hệt nhau, đây chính xác là điều kiện bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_line(line):
    line = line.strip()

    parts = line.replace(" ", "").split("=")

    if len(parts) == 1:
        command, var = line.split()
        return command, [var]

    left = parts[0]
    right = parts[1]
    b, c = right.split("+")
    return "assign", [left, b, c]

def equivalent(program1, program2):
    if len(program1) != len(program2):
        return False

    forward = {}
    backward = {}

    for line1, line2 in zip(program1, program2):
        command1, vars1 = parse_line(line1)
        command2, vars2 = parse_line(line2)

        if command1 != command2 or len(vars1) != len(vars2):
            return False

        for a, b in zip(vars1, vars2):
            if a in forward:
                if forward[a] != b:
                    return False
            else:
                if b in backward:
                    return False

                forward[a] = b
                backward[b] = a

    return True

def solve():
    n = int(input())
    program1 = [input().strip() for _ in range(n)]

    m = int(input())
    program2 = [input().strip() for _ in range(m)]

    print("YES" if equivalent(program1, program2) else "NO")

if __name__ == "__main__":
    solve()
```các`parse_line`hàm bình thường hóa cú pháp gán bằng cách loại bỏ khoảng trắng trước khi chia nó thành ba vị trí biến. Điều này xử lý cả hai hình thức nhỏ gọn như`a=b+c`và các hình thức cách nhau như`a = b + c`. 

Đối với một lệnh đơn giản,`split()`tách từ lệnh khỏi biến của nó. Trình phân tích cú pháp trả về một biểu diễn nội bộ chung, với`assign`biểu thị một phép gán và danh sách liên quan chứa đích, toán hạng bên trái và toán hạng bên phải theo đúng thứ tự đó. 

các`equivalent`Đầu tiên, hàm kiểm tra số dòng vì hai chương trình phải có cấu trúc giống hệt nhau. Sau đó nó duy trì`forward`Và`backward`, thực hiện hai hướng của song ánh. 

Séc`forward[a] != b`bắt một biến buộc phải có hai tên khác nhau. các`backward`tra cứu bắt được hai biến khác nhau bị buộc phải chia sẻ một tên mục tiêu. Cả hai bước kiểm tra đều cần thiết vì việc đổi tên được yêu cầu là sự kết hợp giữa các biến xuất hiện trong hai chương trình. 

Không có đệ quy và không có tính toán số, do đó tràn số nguyên và độ sâu đệ quy là không liên quan. Quá trình quét xử lý mỗi dòng và mỗi lần xuất hiện biến, với các thao tác từ điển mất thời gian không đổi dự kiến. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hai chương trình có cấu trúc giống nhau nên thuật toán bắt đầu so sánh các câu lệnh của chúng. Ba định nghĩa đầu tiên thiết lập các ánh xạ duy nhất có thể có. 

| Dòng | Lệnh | Vị trí biến đầu tiên | Vị trí biến thứ hai | Trạng thái bản đồ | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | xác định |`a`|`a`|`a -> a`| tiếp tục | 
| 2 | xác định |`b`|`b`|`a -> a`,`b -> b`| tiếp tục | 
| 3 | xác định |`c`|`c`|`a -> a`,`b -> b`,`c -> c`| tiếp tục | 
| 4 | giao |`a,b,c`|`a,c,b`|`a -> a`đồng ý,`b -> c`xung đột | từ chối | 

Tại dòng 4, điểm đến`a`nhất quán, nhưng chương trình đầu tiên yêu cầu`b -> c`và bản đồ hiện có yêu cầu`b -> b`. Một lần đổi tên toàn cục không thể đáp ứng cả hai yêu cầu, vì vậy câu trả lời là`NO`. 

### Mẫu 2 

Ở đây thứ tự định nghĩa khác nhau, vì vậy những lần xuất hiện đầu tiên sẽ thiết lập một cách đổi tên không cần thiết. 

| Dòng | Lệnh | Vị trí biến đầu tiên | Vị trí biến thứ hai | Trạng thái bản đồ | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | xác định |`a`|`a`|`a -> a`| tiếp tục | 
| 2 | xác định |`b`|`c`|`a -> a`,`b -> c`| tiếp tục | 
| 3 | xác định |`c`|`b`|`a -> a`,`b -> c`,`c -> b`| tiếp tục | 
| 4 | giao |`a,b,c`|`a,c,b`| tất cả các ánh xạ đều đồng ý | tiếp tục | 
| 5 | in |`a`|`a`|`a -> a`đồng ý | chấp nhận | 

Hoán đổi bản đồ`b`Và`c`. Áp dụng nó cho chương trình đầu tiên biến đổi`a=b+c`vào trong`a=c+b`, khớp chính xác với chương trình thứ hai. Thuật toán chấp nhận vì mọi lần xuất hiện sau đều tôn trọng ánh xạ được thiết lập bởi các định nghĩa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n+m)`| Mỗi dòng đầu vào và mỗi lần xuất hiện biến đều được xử lý một lần, với dự kiến`O(1)`hoạt động từ điển. | 
| Không gian |`O(n+m)`| Hai chương trình được lưu trữ và hai ánh xạ yêu cầu không gian tỷ lệ thuận với số lượng biến riêng biệt. | 

Vì cả hai chương trình đều chứa tối đa 1000 dòng nên chỉ có vài nghìn lần xuất hiện biến cần xử lý. Giải pháp tuyến tính thấp hơn nhiều so với giới hạn 1 giây và 256 MB. Tìm kiếm vũ phu giai thừa là cách tiếp cận duy nhất về cơ bản là không phù hợp. 

## Trường hợp thử nghiệm```python
import sys
import io

def parse_line(line):
    line = line.strip()

    parts = line.replace(" ", "").split("=")

    if len(parts) == 1:
        command, var = line.split()
        return command, [var]

    left = parts[0]
    right = parts[1]
    b, c = right.split("+")
    return "assign", [left, b, c]

def equivalent(program1, program2):
    if len(program1) != len(program2):
        return False

    forward = {}
    backward = {}

    for line1, line2 in zip(program1, program2):
        command1, vars1 = parse_line(line1)
        command2, vars2 = parse_line(line2)

        if command1 != command2 or len(vars1) != len(vars2):
            return False

        for a, b in zip(vars1, vars2):
            if a in forward:
                if forward[a] != b:
                    return False
            else:
                if b in backward:
                    return False
                forward[a] = b
                backward[b] = a

    return True

def solve():
    n = int(input())
    program1 = [input().strip() for _ in range(n)]

    m = int(input())
    program2 = [input().strip() for _ in range(m)]

    return "YES" if equivalent(program1, program2) else "NO"

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        return solve()
    finally:
        sys.stdin = old_stdin
        input = old_input

sample1 = """\
5
define a
define b
define c
a=b+c
print a
5
define a
define b
define c
a=c+b
print a
"""
assert run(sample1) == "NO", "sample 1"

sample2 = """\
5
define a
define b
define c
a=b+c
print a
5
define a
define c
define b
a=c+b
print a
"""
assert run(sample2) == "YES", "sample 2"

sample3 = """\
5
define a
define b
define c
a=b+c
print a
5
define a
define b
define c
a=b+c
print a
"""
assert run(sample3) == "YES", "sample 3"

# Minimum-size programs. A single variable can always be renamed to another
# variable because there is no second constraint.
assert run("""\
1
define a
1
define x
""") == "YES", "minimum size"

# Different program lengths can never be made equal by renaming.
assert run("""\
1
define a
2
define x
define y
""") == "NO", "different lengths"

# The same source variable is forced to map to two different target variables.
assert run("""\
4
define a
define b
print a
print a
4
define x
define y
print x
print y
""") == "NO", "inconsistent mapping"

# The target variables are swapped, but the whole program is still equivalent.
assert run("""\
6
define first
define second
define third
first=second+third
print third
read first
6
define x
define z
define y
x=z+y
print y
read x
""") == "YES", "nontrivial bijection"

# Large input, exercising the linear scan.
lines1 = ["define v0"]
lines1.extend(f"define v{i}" for i in range(1, 1000))
lines1.append("print v999")

lines2 = ["define x0"]
lines2.extend(f"define x{i}" for i in range(1, 1000))
lines2.append("print x999")

large_input = (
    str(len(lines1)) + "\n" +
    "\n".join(lines1) + "\n" +
    str(len(lines2)) + "\n" +
    "\n".join(lines2) + "\n"
)
assert run(large_input) == "YES", "maximum-size linear scan"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một`define`dòng trong mỗi chương trình |`YES`| Kích thước đầu vào tối thiểu và đổi tên cơ bản | 
| Chương trình có 1 và 2 dòng |`NO`| Độ dài chương trình khác nhau | 
| Biến nguồn lặp lại được ánh xạ tới các mục tiêu khác nhau |`NO`| Tính nhất quán của ánh xạ chuyển tiếp | 
| Ba biến có hoán vị không cần thiết |`YES`| Đổi tên hai chiều và sử dụng nhiều lần | 
| 1000 định nghĩa cộng với cách sử dụng cuối cùng |`YES`| Đầu vào có kích thước tối đa và độ phức tạp tuyến tính | 

## Vỏ cạnh 

Trường hợp có độ dài khác nhau được xử lý trước bất kỳ so sánh biến nào. Vì```
1
define a
2
define x
define y
```

`equivalent`thấy rằng độ dài chương trình là`1`Và`2`và ngay lập tức quay trở lại`False`. Đầu ra là`NO`. Không có ánh xạ nào có thể thay đổi số lượng câu lệnh. 

Đối với trường hợp thứ tự toán hạng,```
4
define a
define b
define c
a=b+c
4
define x
define y
define z
x=z+y
```các định nghĩa thiết lập`a -> x`,`b -> y`, Và`c -> z`. Khi đạt được nhiệm vụ, cặp biến đầu tiên của nó là`a -> x`, hợp lệ. Cặp thứ hai yêu cầu`b -> z`, Nhưng`b`đã được ánh xạ tới`y`, do đó thuật toán bác bỏ. Đầu ra là`NO`. Thuật toán không bao giờ sắp xếp các toán hạng, do đó, nó xử lý chính xác biểu thức dưới dạng cú pháp thay vì phép cộng toán học. 

Đối với hoán vị không cần thiết,```
4
define a
define b
define c
a=b+c
4
define x
define z
define y
x=z+y
```ba dòng đầu tiên thiết lập`a -> x`,`b -> z`, Và`c -> y`. Tại bài tập, ba cặp là`a -> x`,`b -> z`, Và`c -> y`, tất cả đều đồng ý với ánh xạ hiện có. Đầu ra là`YES`. Điều này chứng tỏ tại sao việc so sánh tên biến thô hoặc vị trí định nghĩa là không đủ. 

Đối với việc tái sử dụng không nhất quán,```
4
define a
define b
print a
print b
4
define x
define y
print x
print x
```bản in đầu tiên thiết lập`a -> x`. Bản in thứ hai yêu cầu`b -> x`. Từ điển ngược đã có sẵn`x -> a`, do đó thuật toán từ chối ánh xạ. Đầu ra là`NO`. Nếu không có từ điển ngược, việc triển khai bất cẩn có thể chấp nhận hai biến khác nhau được đổi tên thành cùng một biến, đây không phải là cách đổi tên hợp lệ.
