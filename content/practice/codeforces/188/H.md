---
title: "CF 188H - Ngăn xếp"
description: "Đầu vào là một chuỗi mô tả các thao tác trên ngăn xếp. Mỗi chữ số có nghĩa là \"đẩy số này vào ngăn xếp\". Mỗi + hoặc có nghĩa là \"lấy hai giá trị trên cùng từ ngăn xếp, áp dụng thao tác và đẩy kết quả trở lại\"."
date: "2026-06-04T23:20:43+07:00"
tags: ["codeforces", "competitive-programming", "*special", "expression-parsing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 188
codeforces_index: "H"
codeforces_contest_name: "Surprise Language Round 6"
rating: 1800
weight: 188
solve_time_s: 101
verified: true
draft: false
---

[CF 188H - Ngăn xếp](https://codeforces.com/problemset/problem/188/H) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt, phân tích biểu thức, triển khai 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là một chuỗi mô tả các thao tác trên ngăn xếp. Mỗi chữ số có nghĩa là "đẩy số này vào ngăn xếp". Mỗi`+`hoặc`*`có nghĩa là "lấy hai giá trị trên cùng từ ngăn xếp, áp dụng thao tác và đẩy kết quả trở lại". 

Về cơ bản, đây là việc đánh giá một biểu thức được viết bằng Ký hiệu tiếng Ba Lan ngược (ký hiệu hậu tố). Ngăn xếp lưu trữ các kết quả trung gian. Bất cứ khi nào chúng ta gặp một toán tử, hai giá trị có sẵn gần đây nhất sẽ trở thành toán hạng của nó. 

Ví dụ: chuỗi:```
12+3*
```có nghĩa:```
push 1
push 2
add -> 3
push 3
multiply -> 9
```Câu trả lời cuối cùng là giá trị còn lại ở đầu ngăn xếp sau khi tất cả các thao tác đã được xử lý. 

Những hạn chế là cực kỳ nhỏ. Chuỗi thao tác có độ dài tối đa là 20, do đó, ngay cả những cách tiếp cận rất kém hiệu quả cũng sẽ chạy ngay lập tức. Mô phỏng trực tiếp thực hiện một thao tác ngăn xếp cho mỗi ký tự, trong trường hợp xấu nhất chỉ có vài chục thao tác. 

Thách thức chính không phải là hiệu suất mà là việc triển khai chính xác hành vi ngăn xếp. 

Một sai lầm dễ mắc phải là sử dụng toán hạng sai thứ tự. Đối với phép cộng và phép nhân, điều này không ảnh hưởng đến kết quả vì cả hai phép toán đều có tính giao hoán, nhưng trong một bộ đánh giá hậu tố tổng quát hơn thì điều đó sẽ quan trọng. Việc triển khai đúng vẫn phải bật toán hạng thứ hai trước và toán hạng đầu tiên thứ hai. 

Coi như:```
12+
```Ngăn xếp trước`+`là:```
[1, 2]
```Toán tử sử dụng toán hạng`1`Và`2`, sản xuất`3`. 

Một trường hợp đặc biệt khác xảy ra khi đầu vào chỉ chứa các chữ số và không có toán tử. 

đầu vào:```
123
```Ngăn xếp phát triển như sau:```
[1]
[1, 2]
[1, 2, 3]
```Câu trả lời là yếu tố hàng đầu,`3`. Giải pháp giả sử ngăn xếp luôn kết thúc bằng chính xác một giá trị sẽ không thành công ở đây. 

Trường hợp cạnh thứ ba là đầu vào một ký tự. 

đầu vào:```
7
```Đầu ra:```
7
```Thuật toán phải xử lý chính xác một ngăn xếp chứa chính xác một phần tử từ đầu đến cuối. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng quá trình chính xác như được mô tả. Duy trì một ngăn xếp. Khi một chữ số xuất hiện, hãy đẩy giá trị số của nó. Khi một toán tử xuất hiện, hãy bật hai giá trị, áp dụng thao tác và đẩy kết quả. 

Vì mọi thao tác được mô tả bởi đầu vào tương ứng với một lượng công việc không đổi nên tổng thời gian chạy tỷ lệ thuận với độ dài của chuỗi. 

Người ta cũng có thể nghĩ đến việc xây dựng lại biểu thức toán học được biểu thị bằng ký hiệu hậu tố và sau đó đánh giá nó. Cách đó hiệu quả nhưng gây ra sự phức tạp không cần thiết. Chúng ta cần xây dựng cây biểu thức hoặc chuyển đổi ký hiệu sang dạng khác trước khi đánh giá. 

Quan sát quan trọng là bài toán đã cho chúng ta biết cách thực hiện phép tính. Ngăn xếp không chỉ đơn thuần là một chi tiết triển khai mà còn là cơ chế đánh giá thực tế. Thực hiện theo các hoạt động theo nghĩa đen sẽ đưa ra câu trả lời ngay lập tức. 

Vì độ dài đầu vào tối đa là 20 nên cả hai cách tiếp cận đều đủ nhanh. Mô phỏng trực tiếp đơn giản hơn và khớp chính xác với phát biểu vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng ngăn xếp Brute Force | O(n) | O(n) | Đã chấp nhận | 
| Mô phỏng ngăn xếp tối ưu | O(n) | O(n) | Đã chấp nhận | 

Đối với vấn đề này, mô phỏng tự nhiên đã là tối ưu. 

## Hướng dẫn thuật toán 

1. Tạo một ngăn xếp trống. 
2. Xử lý chuỗi đầu vào từ trái sang phải. 
3. Nếu ký tự hiện tại là một chữ số, hãy chuyển nó thành số nguyên và đẩy nó vào ngăn xếp. 
4. Nếu ký tự hiện tại là`+`, lấy hai giá trị trên cùng ra khỏi ngăn xếp, thêm chúng và đẩy kết quả trở lại. 

Ngăn xếp luôn chứa đủ giá trị vì đầu vào được đảm bảo hợp lệ. 
5. Nếu ký tự hiện tại là`*`, lấy hai giá trị trên cùng ra khỏi ngăn xếp, nhân chúng và đẩy kết quả trở lại. 
6. Sau khi tất cả các ký tự đã được xử lý, xuất phần tử trên cùng của ngăn xếp. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của đầu vào, ngăn xếp chứa chính xác các giá trị tồn tại trong ngăn xếp thực được mô tả bởi câu lệnh vấn đề. Các chữ số thêm các giá trị mới, trong khi các toán tử sử dụng hai giá trị trên cùng và thay thế chúng bằng kết quả của phép toán. Vì mỗi bước tái tạo chính xác hành vi ngăn xếp được yêu cầu nên trạng thái ngăn xếp cuối cùng giống hệt với quá trình thực thi dự kiến. Do đó, phần tử trên cùng ở cuối là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    stack = []

    for ch in s:
        if ch.isdigit():
            stack.append(int(ch))
        elif ch == '+':
            b = stack.pop()
            a = stack.pop()
            stack.append(a + b)
        else:  # '*'
            b = stack.pop()
            a = stack.pop()
            stack.append(a * b)

    print(stack[-1])

if __name__ == "__main__":
    solve()
```Việc thực hiện theo mô phỏng trực tiếp. 

Ngăn xếp được biểu thị bằng danh sách Python. Việc bổ sung tương ứng với thao tác đẩy và`pop()`loại bỏ phần tử trên cùng hiện tại. 

Khi gặp một toán tử, mã sẽ loại bỏ hai phần tử. Cửa sổ bật lên đầu tiên cung cấp toán hạng thứ hai và cửa sổ bật lên thứ hai cung cấp toán hạng đầu tiên. Mặc dù phép cộng và phép nhân có tính chất giao hoán, việc giữ đúng thứ tự toán hạng là một cách làm tốt và làm cho việc thực hiện nhất quán với đánh giá hậu tố nói chung. 

Cuối cùng, vấn đề yêu cầu phần tử ngăn xếp trên cùng. Giá trị đó được lưu trữ tại`stack[-1]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12+3*66*+
```| Nhân vật | Hành động | Ngăn xếp sau hành động | 
| --- | --- | --- | 
| 1 | đẩy 1 | [1] | 
| 2 | đẩy 2 | [1, 2] | 
| + | 1 + 2 = 3 | [3] | 
| 3 | đẩy 3 | [3, 3] | 
| * | 3 * 3 = 9 | [9] | 
| 6 | đẩy 6 | [9, 6] | 
| 6 | đẩy 6 | [9, 6, 6] | 
| * | 6 * 6 = 36 | [9, 36] | 
| + | 9 + 36 = 45 | [45] | 

Đầu ra:```
45
```Dấu vết này cho thấy các kết quả trung gian thay thế toán hạng của chúng trên ngăn xếp như thế nào. Mỗi toán tử giảm kích thước ngăn xếp đi một trong khi vẫn giữ nguyên giá trị của biểu thức con được đánh giá. 

### Ví dụ 2 

đầu vào:```
123
```| Nhân vật | Hành động | Ngăn xếp sau hành động | 
| --- | --- | --- | 
| 1 | đẩy 1 | [1] | 
| 2 | đẩy 2 | [1, 2] | 
| 3 | đẩy 3 | [1, 2, 3] | 

Đầu ra:```
3
```Ví dụ này chứng tỏ rằng ngăn xếp cuối cùng không bắt buộc phải chứa chính xác một giá trị. Bài toán chỉ yêu cầu phần tử trên cùng, đó là`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(n) | Trong trường hợp xấu nhất, tất cả các ký tự đều là chữ số và vẫn còn trên ngăn xếp | 

Đây`n`là độ dài của chuỗi thao tác. Từ`n ≤ 20`, thời gian chạy thực sự là tức thời và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    s = input().strip()
    stack = []

    for ch in s:
        if ch.isdigit():
            stack.append(int(ch))
        elif ch == '+':
            b = stack.pop()
            a = stack.pop()
            stack.append(a + b)
        else:
            b = stack.pop()
            a = stack.pop()
            stack.append(a * b)

    return str(stack[-1])

# provided sample
assert run("12+3*66*+\n") == "45", "sample 1"

# custom cases
assert run("7\n") == "7", "single digit"
assert run("12+\n") == "3", "single operation"
assert run("123\n") == "3", "no operators"
assert run("99*\n") == "81", "multiplication only"
assert run("11111111111111111111\n") == "1", "maximum length, all digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`|`7`| Đầu vào hợp lệ tối thiểu | 
|`12+`|`3`| Xử lý toán tử cơ bản | 
|`123`|`3`| Không có nhà khai thác nào có mặt | 
|`99*`|`81`| Đường nhân | 
|`11111111111111111111`|`1`| Độ dài tối đa chỉ với lần đẩy | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
123
```Thực thi:```
push 1 -> [1]
push 2 -> [1, 2]
push 3 -> [1, 2, 3]
```Thuật toán đưa ra phần tử trên cùng,`3`. Nó không sai khi cho rằng chính xác một phần tử phải còn lại trên ngăn xếp. 

Hãy xem xét đầu vào:```
7
```Thực thi:```
push 7 -> [7]
```Không có toán tử nào được xử lý. Phần tử trên cùng cuối cùng là`7`, được in trực tiếp. 

Hãy xem xét đầu vào:```
12+
```Thực thi:```
push 1 -> [1]
push 2 -> [1, 2]
+ -> [3]
```Toán tử sử dụng hai giá trị gần đây nhất và đẩy tổng của chúng. Ngăn xếp kết quả chứa một giá trị duy nhất,`3`, trở thành câu trả lời. 

Những trường hợp này bao gồm các tình huống có nhiều khả năng dẫn đến các giả định không chính xác nhất về kích thước ngăn xếp hoặc cách xử lý của toán tử và mô phỏng xử lý tất cả chúng một cách tự nhiên.
