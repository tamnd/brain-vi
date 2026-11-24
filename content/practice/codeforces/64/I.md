---
title: "CF 64I - Sắp xếp bảng"
description: "Chúng ta được cung cấp một bảng trong đó mỗi hàng chứa một số trường chuỗi và dòng đầu vào đầu tiên cho chúng ta biết tên của mỗi cột. Một dòng khác mô tả cách sắp xếp các hàng. Mỗi quy tắc có dạng COLUMNNAME ASC hoặc COLUMNNAME DESC."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "sortings"]
categories: ["algorithms"]
codeforces_contest: 64
codeforces_index: "I"
codeforces_contest_name: "Unknown Language Round 1"
rating: 2400
weight: 64
solve_time_s: 110
verified: true
draft: false
---

[CF 64I - Sắp xếp bảng](https://codeforces.com/problemset/problem/64/I) 

**Đánh giá:** 2400 
**Thẻ:** *đặc biệt, phân loại 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bảng trong đó mỗi hàng chứa một số trường chuỗi và dòng đầu vào đầu tiên cho chúng ta biết tên của mỗi cột. Một dòng khác mô tả cách sắp xếp các hàng. Mỗi quy tắc có dạng`COLUMN_NAME ASC`hoặc`COLUMN_NAME DESC`. 

Việc sắp xếp hoạt động chính xác như một cơ sở dữ liệu`ORDER BY`khoản. Quy tắc đầu tiên có mức độ ưu tiên cao nhất. Nếu hai hàng bằng nhau theo quy tắc đó, chúng ta so sánh chúng bằng quy tắc thứ hai, v.v. Nếu tất cả các quy tắc coi hai hàng bằng nhau thì thứ tự ban đầu của chúng phải không thay đổi. Yêu cầu cuối cùng đó có nghĩa là việc sắp xếp phải ổn định. 

Kích thước bảng rất nhỏ so với các tiêu chuẩn lập trình cạnh tranh. Cả số hàng và số cột nhiều nhất là 100. Ngay cả một`O(n^2)`hoặc`O(n^2 log n)`giải pháp hoàn toàn an toàn ở đây. Phần đắt tiền không phải là số lượng hàng mà là việc xử lý nhiều khóa sắp xếp theo các hướng khác nhau trong khi vẫn duy trì được sự ổn định. 

Phần khó khăn không phải là hiệu suất mà là tính chính xác. Việc thực hiện bất cẩn có thể dễ dàng phá vỡ các quy tắc đặt hàng. 

Hãy xem xét ví dụ này:```
A B
A ASC, B DESC
x 1
x 3
x 2
```Đầu ra đúng:```
x 3
x 2
x 1
```Nếu chúng ta chỉ sắp xếp theo khóa đầu tiên`A`, các hàng không thay đổi vì tất cả các giá trị đều bằng nhau. Chìa khóa thứ hai vẫn còn quan trọng. 

Một trường hợp tế nhị khác là sự ổn định:```
A
A ASC
x
x
x
```Đầu ra đúng:```
x
x
x
```Sắp xếp ổn định duy trì thứ tự ban đầu của các phần tử bằng nhau. Một số triển khai bộ so sánh tùy chỉnh vô tình sắp xếp lại các hàng bằng nhau vì chúng trả về kết quả không nhất quán. 

Hướng hỗn hợp là một nguồn lỗi phổ biến khác:```
A B
A DESC, B ASC
1 z
2 y
2 x
```Đầu ra đúng:```
2 x
2 y
1 z
```Phím đầu tiên giảm dần, nhưng phím thứ hai tăng dần. Nếu chúng ta đảo ngược toàn bộ kết quả so sánh thay vì xử lý từng khóa một cách độc lập thì thứ tự sẽ sai. 

Cuối cùng, việc phân tích dòng quy tắc đòi hỏi sự cẩn thận. Các quy tắc được phân tách bằng`", "`, không chỉ dấu phẩy. Việc phân chia không chính xác có thể để lại khoảng trắng thừa kèm theo tên trường hoặc chỉ đường. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là so sánh các hàng theo cách thủ công. Đối với mỗi cặp hàng, chúng tôi kiểm tra từng quy tắc một cho đến khi tìm thấy trường có các hàng khác nhau. Nếu quy tắc hiện tại tăng dần, chúng tôi sử dụng thứ tự từ điển thông thường. Nếu nó giảm dần, chúng ta đảo ngược sự so sánh. Nếu tất cả các quy tắc đều khớp, các hàng được coi là bằng nhau và thứ tự ban đầu của chúng sẽ không thay đổi. 

Cách tiếp cận này đơn giản về mặt khái niệm vì nó phản ánh chính xác tuyên bố. Chúng ta có thể cắm một bộ so sánh như vậy vào một thuật toán sắp xếp. Với tối đa 100 hàng, ngay cả một loại ổn định bậc hai như sắp xếp bong bóng cũng có thể vượt qua một cách thoải mái. Trường hợp xấu nhất là xung quanh`100^2 * 100`, chỉ bằng một triệu so sánh trường. 

Tuy nhiên, Python đã cung cấp cho chúng ta một thuật toán sắp xếp ổn định, TimSort. Sử dụng nó đúng cách sẽ giúp quá trình triển khai trở nên sạch sẽ hơn và ít xảy ra lỗi hơn. 

Quan sát chính là việc sắp xếp ổn định cho phép chúng tôi xử lý các quy tắc từ mức ưu tiên thấp nhất đến mức ưu tiên cao nhất. Giả sử các quy tắc là:```
A ASC, B DESC, C ASC
```Đầu tiên chúng ta sắp xếp theo`C`, sau đó bởi`B`, sau đó bởi`A`. 

Tại sao điều này làm việc? Sau khi sắp xếp theo`C`, các hàng được sắp xếp chính xác theo khóa có mức ưu tiên thấp nhất. Sau này khi chúng tôi sắp xếp theo`B`, các hàng có khác nhau`B`các giá trị được sắp xếp lại chính xác, trong khi các hàng có bằng nhau`B`giữ trước đó của họ`C`đặt hàng vì sắp xếp của Python ổn định. Việc lặp lại quá trình này cuối cùng sẽ tạo ra thứ tự đa khóa từ điển chính xác mà chúng ta mong muốn. 

Điều này loại bỏ hoàn toàn sự cần thiết của một bộ so sánh tùy chỉnh. Mỗi lượt phân loại sử dụng một trường duy nhất và một`reverse=True/False`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sắp xếp bộ so sánh Brute Force | O(r² × k) | O(1) | Đã chấp nhận | 
| Sắp xếp nhiều lượt ổn định | O(k × r log r) | O(r) | Đã chấp nhận | 

Đây,`r`là số hàng và`k`là số quy tắc sắp xếp. 

## Hướng dẫn thuật toán 

1. Đọc tên cột và lưu trữ vị trí của từng cột trong từ điển. 

Điều này cho phép chúng tôi chuyển đổi một tên trường như`"AGE"`vào chỉ mục cột của nó trong thời gian không đổi. 
2. Đọc dòng quy tắc và chia nó cho`", "`. 

Mỗi phần trông giống như`"FIELD ASC"`hoặc`"FIELD DESC"`. 
3. Phân tích mọi quy tắc thành hai giá trị: chỉ mục cột và thứ tự có giảm dần hay không. 

Ví dụ,`"AGE DESC"`trở thành`(2, True)`nếu như`AGE`là cột thứ ba. 
4. Đọc tất cả các dòng còn lại dưới dạng hàng trong bảng. 

Mỗi hàng được lưu trữ dưới dạng danh sách các chuỗi. 
5. Xử lý các quy tắc theo thứ tự ngược lại. 

Quy tắc ít quan trọng nhất phải được áp dụng trước tiên vì các loại ổn định sau này sẽ bảo toàn các quyết định hòa giải trước đó. 
6. Đối với mỗi quy tắc, hãy sắp xếp các hàng bằng cách sử dụng cột tương ứng làm khóa. 

Sử dụng`reverse=True`theo thứ tự giảm dần và`reverse=False`cho thứ tự tăng dần. 
7. In các hàng theo thứ tự cuối cùng. 

### Tại sao nó hoạt động 

Sau khi xử lý quy tắc cuối cùng, các hàng được sắp xếp chính xác theo điều kiện có mức độ ưu tiên thấp nhất. 

Giả sử rằng sau khi xử lý các quy tắc từ vị trí`i + 1`trở đi, các hàng đã được sắp xếp chính xác theo tất cả các quy tắc có mức độ ưu tiên thấp hơn. Khi chúng tôi sắp xếp ổn định theo quy tắc`i`, các hàng có giá trị khác nhau cho quy tắc`i`di chuyển theo đúng thứ tự tương đối. Các hàng có giá trị bằng nhau cho quy tắc`i`giữ nguyên thứ tự trước đó vì việc sắp xếp ổn định. Thứ tự được bảo toàn đó đã đáp ứng mọi quy tắc có mức độ ưu tiên thấp hơn. 

Bằng quy nạp, sau khi xử lý tất cả các quy tắc, các hàng thỏa mãn thứ tự từ điển hoàn chỉnh mà bài toán yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    columns = input().split()

    pos = {}
    for i, name in enumerate(columns):
        pos[name] = i

    rules_line = input().strip()
    rules_raw = rules_line.split(", ")

    rules = []

    for rule in rules_raw:
        field, order = rule.split()
        rules.append((pos[field], order == "DESC"))

    rows = []

    for line in sys.stdin:
        line = line.strip()

        if not line:
            continue

        rows.append(line.split())

    for col, rev in reversed(rules):
        rows.sort(key=lambda row: row[col], reverse=rev)

    for row in rows:
        print(*row)

solve()
```Phần đầu tiên xây dựng ánh xạ từ tên cột đến chỉ mục. Điều này tránh việc tìm kiếm tuyến tính lặp lại bất cứ khi nào quy tắc tham chiếu đến một trường. 

Các quy tắc được lưu trữ theo cặp`(column_index, is_descending)`. Việc chuyển đổi sớm mọi thứ thành chỉ mục giúp vòng lặp sắp xếp trở nên đơn giản. 

Đọc các hàng cho đến khi EOF khớp với định dạng đầu vào một cách tự nhiên vì số lượng hàng không được cung cấp rõ ràng. 

Chi tiết triển khai quan trọng nhất là lặp lại`reversed(rules)`. Việc xử lý chúng theo thứ tự ban đầu sẽ hủy bỏ thứ tự có mức độ ưu tiên cao hơn đã được thiết lập trước đó. 

Cuộc gọi:```
rows.sort(key=lambda row: row[col], reverse=rev)
```hoạt động vì tính năng sắp xếp của Python ổn định. Các phần tử bằng nhau sẽ tự động giữ thứ tự trước đó của chúng. 

Tất cả các so sánh đều là so sánh chuỗi từ điển, chính xác theo yêu cầu của câu lệnh. Chúng tôi không bao giờ chuyển đổi giá trị thành số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
NAME GROUP AGE
GROUP ASC, AGE DESC
Alex 412 19
Peter 422 19
Sergey 412 18
Andrey 311 18
```Quy tắc sau khi phân tích cú pháp: 

| Ưu tiên quy tắc | Cột | Đặt hàng | 
| --- | --- | --- | 
| 1 | NHÓM | ASC | 
| 2 | TUỔI | MÔ TẢ | 

Chúng tôi xử lý chúng theo thứ tự ngược lại. 

#### Sắp xếp theo TUỔI MÔ TẢ 

| Hàng | TUỔI | 
| --- | --- | 
| Alex 412 19 | 19 | 
| Peter 422 19 | 19 | 
| Serge 412 18 | 18 | 
| Andrey 311 18 | 18 | 

Trạng thái sau khi sắp xếp: 

| Vị trí | Hàng | 
| --- | --- | 
| 1 | Alex 412 19 | 
| 2 | Peter 422 19 | 
| 3 | Serge 412 18 | 
| 4 | Andrey 311 18 | 

#### Sắp xếp theo NHÓM ASC 

| Hàng | NHÓM | 
| --- | --- | 
| Alex 412 19 | 412 | 
| Peter 422 19 | 422 | 
| Serge 412 18 | 412 | 
| Andrey 311 18 | 311 | 

Thứ tự cuối cùng: 

| Vị trí | Hàng | 
| --- | --- | 
| 1 | Andrey 311 18 | 
| 2 | Alex 412 19 | 
| 3 | Serge 412 18 | 
| 4 | Peter 422 19 | 

Dấu vết này cho thấy tại sao sự ổn định lại quan trọng. Cả hai hàng có nhóm`412`vẫn được sắp xếp theo độ tuổi giảm dần vì thứ tự đó đã được thiết lập trước đó. 

### Ví dụ 2 

đầu vào:```
A B
A DESC, B ASC
1 z
2 y
2 x
```Quy tắc sau khi phân tích cú pháp: 

| Ưu tiên quy tắc | Cột | Đặt hàng | 
| --- | --- | --- | 
| 1 | A | MÔ TẢ | 
| 2 | B | ASC | 

#### Sắp xếp theo B ASC 

| Vị trí | Hàng | 
| --- | --- | 
| 1 | 2x | 
| 2 | 2 năm | 
| 3 | 1z | 

#### Sắp xếp theo A DESC 

| Vị trí | Hàng | 
| --- | --- | 
| 1 | 2x | 
| 2 | 2 năm | 
| 3 | 1z | 

Các hàng với`A = 2`bảo tồn trước đó của họ`B ASC`đặt hàng vì loại thứ hai ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k × r log r) | Chúng tôi thực hiện một loại cho mỗi quy tắc | 
| Không gian | O(r) | Sắp xếp Python có thể sử dụng bộ nhớ phụ | 

Đây,`r`là số hàng và`k`là số quy tắc 

Với tối đa 100 hàng và 100 cột, thao tác này sẽ chạy ngay lập tức. Ngay cả một số lượt phân loại đầy đủ cũng không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    columns = input().split()

    pos = {}
    for i, name in enumerate(columns):
        pos[name] = i

    rules_line = input().strip()
    rules_raw = rules_line.split(", ")

    rules = []

    for rule in rules_raw:
        field, order = rule.split()
        rules.append((pos[field], order == "DESC"))

    rows = []

    for line in sys.stdin:
        line = line.strip()

        if not line:
            continue

        rows.append(line.split())

    for col, rev in reversed(rules):
        rows.sort(key=lambda row: row[col], reverse=rev)

    out = []

    for row in rows:
        out.append(" ".join(row))

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""NAME GROUP AGE
GROUP ASC, AGE DESC
Alex 412 19
Peter 422 19
Sergey 412 18
Andrey 311 18
"""
) == (
"""Andrey 311 18
Alex 412 19
Sergey 412 18
Peter 422 19
"""
), "sample 1"

# minimum size
assert run(
"""A
A ASC
x
"""
) == (
"""x
"""
), "single row"

# stability check
assert run(
"""A B
A ASC
x 1
x 2
x 3
"""
) == (
"""x 1
x 2
x 3
"""
), "stable ordering"

# mixed ascending and descending
assert run(
"""A B
A DESC, B ASC
1 z
2 y
2 x
"""
) == (
"""2 x
2 y
1 z
"""
), "mixed directions"

# lexicographic comparison
assert run(
"""A
A ASC
10
2
9
"""
) == (
"""10
2
9
"""
), "strings not integers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bàn một hàng | Cùng một hàng | Kích thước đầu vào tối thiểu | 
| Tất cả các khóa chính bằng nhau | Đơn hàng gốc được bảo quản | Sắp xếp ổn định | 
| Hỗn hợp ASC và DESC | Thứ tự lớp đúng | Xử lý hướng | 
| Giá trị`10`,`2`,`9`| Thứ tự từ điển | Chuỗi không phải là số nguyên | 

## Vỏ cạnh 

Xem xét các hàng bằng nhau theo mọi quy tắc:```
A B
A ASC, B DESC
x 1
x 1
x 1
```Thuật toán sắp xếp đầu tiên theo`B DESC`, không thay đổi gì vì tất cả các giá trị đều bằng nhau. Sau đó nó sắp xếp theo`A ASC`, điều này cũng không thay đổi gì cả. Vì việc sắp xếp Python ổn định nên thứ tự ban đầu được giữ nguyên chính xác. 

Bây giờ hãy xem xét các hướng xung đột:```
A B
A DESC, B ASC
2 z
2 x
1 y
```Thuật toán sắp xếp đầu tiên theo`B ASC`:```
2 x
1 y
2 z
```Sau đó nó sắp xếp theo`A DESC`:```
2 x
2 z
1 y
```Trong số các hàng nơi`A = 2`, trước đó`B ASC`Đơn hàng tồn tại vì loại thứ hai ổn định. 

Cuối cùng, hãy xem xét hành vi từ điển:```
A
A ASC
2
10
9
```So sánh chuỗi cho:```
10
2
9
```bởi vì`"10"`đến trước`"2"`về mặt từ điển. Thuật toán xử lý việc này một cách tự động vì nó không bao giờ chuyển đổi chuỗi thành số.
