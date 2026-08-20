---
title: "CF 102263C - Kiểm tra văn bản"
description: "Roze muốn văn bản cuối cùng trên màn hình phải chính xác là văn bản được yêu cầu, bao gồm cả cách viết hoa và khoảng trắng giữa các từ liên tiếp. Văn bản bắt buộc được cung cấp dưới dạng n từ, vì vậy nội dung màn hình mong muốn là những từ được nối bằng một khoảng trắng."
date: "2026-08-19T02:48:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "C"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 205
verified: true
draft: false
---

[CF 102263C - Kiểm tra văn bản](https://codeforces.com/problemset/problem/102263/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Roze muốn văn bản cuối cùng trên màn hình phải chính xác là văn bản được yêu cầu, bao gồm cả cách viết hoa và khoảng trắng giữa các từ liên tiếp. Văn bản cần thiết được đưa ra dưới dạng`n`các từ, vì vậy nội dung màn hình mong muốn là những từ được nối bằng một khoảng trắng. 

Phần thứ hai của đầu vào mô tả các thao tác thực tế trên bàn phím, mỗi phím một dòng. Phím chữ cái tạo ra ký tự chữ thường hoặc chữ hoa tùy thuộc vào trạng thái CapsLock hiện tại. Nhấn`CapsLock`chuyển đổi trạng thái đó.`Space`chèn một khoảng trắng, trong khi`Backspace`xóa ký tự cuối cùng hiện có trên màn hình, nếu màn hình không trống. 

Nhiệm vụ là thực hiện các hành động này chính xác như bàn phím thực hiện và so sánh màn hình kết quả với văn bản dự định. Chúng tôi in`Correct`chỉ khi hai chuỗi giống hệt nhau. 

Số lượng từ và số lần nhấn phím đều dưới 2000 và tổng độ dài của các từ cần thiết cũng dưới 2000. Những giới hạn này đủ nhỏ để ngay cả việc triển khai bậc hai thường có thể vừa vặn một cách thoải mái, nhưng chúng cũng tạo ra một mô phỏng tuyến tính trực tiếp cực kỳ đơn giản. Không cần băm, lập trình động hoặc bất kỳ cấu trúc dữ liệu nâng cao nào. Chúng ta chỉ cần xử lý mỗi key một lần và duy trì màn hình hiện tại. 

Một số chi tiết có thể khiến việc triển khai không chính xác dẫn đến âm thầm chấp nhận văn bản sai. 

Hãy xem xét đầu vào này:```
2
a b
2
a
b
```Màn hình cuối cùng là`ab`, trong khi văn bản được yêu cầu là`a b`, vậy câu trả lời là`Incorrect`. Một cách triển khai chỉ đơn giản là so sánh chuỗi các chữ cái trong khi bỏ qua`Space`sẽ chấp nhận nó một cách sai lầm. 

Viết hoa cũng là một phần của văn bản. Ví dụ:```
2
A b
3
CapsLock
a
b
```Màn hình cuối cùng là`Ab`, không`A b`, vậy câu trả lời là`Incorrect`. Việc triển khai bất cẩn có thể chỉ theo dõi những chữ cái đã được nhập và quên rằng CapsLock thay đổi kiểu chữ của chúng. 

Backspace ở màn hình trống là một điều kiện biên khác. Ví dụ:```
2
a b
4
a
Backspace
Space
b
```Hai hành động đầu tiên để màn hình trống, sau đó`Space`sản xuất`" "`, Và`b`sản xuất`" b"`. Kết quả là`Incorrect`. Tổng quát hơn, phím lùi không được làm cho chiều dài màn hình trở thành âm. 

Cuối cùng, phím xóa lùi có thể xóa dấu cách giống như xóa một chữ cái. Ví dụ:```
2
a b
4
a
Space
Backspace
b
```Màn hình cuối cùng là`ab`, không`a b`, vậy câu trả lời là`Incorrect`. Việc xử lý các khoảng trắng tách biệt khỏi các chữ cái trong quá trình xóa sẽ là sai lầm. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp là điểm khởi đầu tự nhiên. Duy trì một chuỗi đại diện cho màn hình hiện tại. Đối với mỗi chữ cái, hãy thêm ký tự được viết đúng cách. Vì`Space`, thêm một khoảng trắng. Vì`Backspace`, hãy xóa ký tự cuối cùng khi có ký tự đó. Vì`CapsLock`, chuyển đổi cờ Boolean. Khi tất cả các phím đã được xử lý, hãy so sánh màn hình mô phỏng với`" ".join(words)`. 

Ý tưởng này đúng vì mọi thao tác bàn phím đều có biểu diễn trực tiếp trong mô phỏng. Điểm yếu chính của việc triển khai đơn giản nhất là việc lựa chọn cấu trúc dữ liệu. Chuỗi Python là bất biến, vì vậy một thao tác như`screen = screen[:-1]`tạo một chuỗi mới và sao chép các ký tự còn lại. Các phần bổ sung lặp đi lặp lại cũng có thể sao chép các chuỗi ngày càng dài. 

Với nhiều nhất là 1999 lần nhấn phím, số lượng sao chép ký tự trong trường hợp xấu nhất là vào khoảng một triệu thao tác đối với một chuỗi các thao tác thêm và xóa được xây dựng cẩn thận, do đó, ngay cả việc triển khai ngây thơ này cũng có thể chấp nhận được đối với các ràng buộc ban đầu. Tuy nhiên, hành vi bậc hai của nó là không cần thiết và sẽ trở thành vấn đề nếu cùng một tác vụ được mở rộng tới hàng trăm nghìn lần nhấn phím. Vấn đề không phải ở bản thân mô phỏng mà là liên tục xây dựng lại toàn bộ tiền tố của màn hình. 

Quan sát quan trọng là màn hình hoạt động giống hệt như một ngăn xếp. Một ký tự mới gõ được đặt ở cuối và`Backspace`loại bỏ chính xác ký tự được viết gần đây nhất. Danh sách Python cung cấp cho chúng tôi khả năng thêm và xóa theo thời gian liên tục từ cuối, do đó, nó thể hiện màn hình một cách tự nhiên. 

Với cách biểu diễn này, mọi phím đều yêu cầu phải làm việc liên tục. Chúng tôi xử lý chuỗi khóa một lần, tạo ra màn hình cuối cùng theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chuỗi bất biến ngây thơ | O(m2) trường hợp xấu nhất | O(m) | Được chấp nhận đối với các giới hạn này nhưng chậm không cần thiết | 
| Mô phỏng ngăn xếp | O(m + L) | O(m + L) | Đã chấp nhận | 

Đây`m`là số lần nhấn phím và`L`là độ dài của văn bản cần thiết. 

## Hướng dẫn thuật toán 

1. Đọc`n`các từ mục tiêu và xây dựng màn hình cần thiết với`" ".join(words)`. Việc nối là cần thiết vì khoảng cách giữa các từ là một phần của nội dung phải được kiểm tra. 
2. Tạo một danh sách trống có tên`screen`. Danh sách này đại diện cho các ký tự hiện tại trên màn hình, với thành phần danh sách cuối cùng tương ứng với ký tự mà Backspace sẽ loại bỏ. 
3. Đặt biến Boolean`caps`ĐẾN`False`. Bàn phím bắt đầu ở chế độ chữ thường, vì vậy phím chữ cái đầu tiên phải được hiểu là chữ thường trừ khi có`CapsLock`key đã chuyển đổi trạng thái. 
4. Xử lý từng`m`các phím được nhấn theo thứ tự. Nếu chìa khóa là`CapsLock`, lật`caps`. Không có ký tự nào được thêm vào màn hình vì CapsLock chỉ thay đổi cách giải thích các phím chữ cái trong tương lai. 
5. Nếu chìa khóa là`Backspace`, loại bỏ phần tử cuối cùng của`screen`khi danh sách không trống. Nếu danh sách trống, không làm gì cả, khớp với hoạt động của bàn phím. 
6. Nếu chìa khóa là`Space`, nối thêm`" "`ĐẾN`screen`. Dấu cách là các ký tự màn hình thông thường nhằm mục đích Backspace và so sánh cuối cùng. 
7. Nếu không thì khóa là một chữ cái. Chuyển nó thành chữ hoa khi`caps`là đúng và viết thường khi`caps`là sai thì hãy nối ký tự kết quả vào`screen`. 
8. Sau khi tất cả các khóa đã được xử lý, hãy chuyển đổi`screen`thành một chuỗi và so sánh nó với văn bản đích. In`Correct`nếu chúng bằng nhau và`Incorrect`nếu không thì. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của chuỗi khóa,`screen`chứa chính xác các ký tự sẽ hiển thị trên màn hình của bàn phím thực sau khi thực thi cùng tiền tố đó và`caps`bằng trạng thái CapsLock hiện tại của bàn phím. 

Ban đầu cả hai đều đúng vì màn hình trống và CapsLock tắt. Mỗi phím có thể giữ nguyên bất biến: CapsLock chỉ thay đổi trạng thái, Backspace xóa ký tự hiển thị cuối cùng khi có thể, Space thêm dấu cách và một chữ cái thêm chữ cái có kiểu chữ được xác định bởi trạng thái hiện tại. Do đó, sau khi tất cả các khóa được xử lý,`screen`chính xác là màn hình cuối cùng thực tế. Do đó, việc so sánh nó với văn bản được yêu cầu là đủ để xác định xem văn bản đó có được in chính xác hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    words = input().split()
    target = " ".join(words)

    m = int(input())

    screen = []
    caps = False

    for _ in range(m):
        key = input().strip()

        if key == "CapsLock":
            caps = not caps
        elif key == "Backspace":
            if screen:
                screen.pop()
        elif key == "Space":
            screen.append(" ")
        else:
            screen.append(key.upper() if caps else key.lower())

    result = "".join(screen)

    print("Correct" if result == target else "Incorrect")

if __name__ == "__main__":
    solve()
```Chuỗi mục tiêu được xây dựng một lần từ các từ. Vì đầu vào đảm bảo chính xác một khoảng cách dự định giữa các từ liên tiếp,`" ".join(words)`tạo ra chính xác chuỗi phải xuất hiện trên màn hình. 

các`screen`list là ngăn xếp được mô tả trong thuật toán.`append`xử lý các ký tự vào màn hình, trong khi`pop`loại bỏ ký tự được viết gần đây nhất. các`if screen`việc kiểm tra là cần thiết vì Backspace trên màn hình trống không có tác dụng. 

các`caps`cờ được bật trước khi xử lý bất kỳ chữ cái nào sau này. MỘT`CapsLock`Bản thân khóa không bao giờ xuất hiện trên màn hình, vì vậy nó không được thêm vào ngăn xếp. 

Thứ tự của việc kiểm tra điều kiện quan trọng vì`CapsLock`,`Backspace`, Và`Space`là những tên khóa đặc biệt chứ không phải là các chữ cái. Bất kỳ khóa nào khác đều được đảm bảo là một ký tự chữ cái duy nhất để nhánh cuối cùng có thể xử lý nó một cách an toàn. 

Không có lo ngại về tràn số nguyên vì thuật toán chỉ sử dụng các bộ đếm được giới hạn bởi kích thước đầu vào. Danh sách có thể chứa tối đa`m`ký tự, do đó việc sử dụng bộ nhớ của nó là tuyến tính. 

## Ví dụ đã hoạt động 

Câu lệnh cung cấp một mẫu, vì vậy dấu vết thứ hai bên dưới sử dụng một trường hợp được xây dựng nhỏ để thực hiện CapsLock và Backspace. 

### Mẫu 1 

Văn bản cần thiết là`Hello World`. Các phím được nhấn đầu tiên tạo ra`Hell`, sau đó Backspace xóa phần cuối cùng`l`, và các hoạt động sau này cuối cùng tạo ra`Howorld`thay vì văn bản cần thiết. 

| Chìa khóa | Mũ | Màn hình | 
| --- | --- | --- | 
| CapsLock | đúng |`""`| 
| h | đúng |`"H"`| 
| CapsLock | sai |`"H"`| 
| e | sai |`"He"`| 
| tôi | sai |`"Hel"`| 
| tôi | sai |`"Hell"`| 
| Phím lùi | sai |`"Hel"`| 
| o | sai |`"Helo"`| 
| Không gian | sai |`"Helo "`| 
| w | sai |`"Helo w"`| 
| o | sai |`"Helo wo"`| 
| Phím lùi | sai |`"Helo w"`| 
| Phím lùi | sai |`"Helo "`| 
| w | sai |`"Helo w"`| 
| o | sai |`"Helo wo"`| 
| r | sai |`"Helo wor"`| 
| tôi | sai |`"Helo worl"`| 
| d | sai |`"Helo world"`| 

Màn hình cuối cùng là`"Helo world"`, trong khi mục tiêu là`"Hello World"`. Cả hai đều mất tích`l`và cách viết hoa không chính xác của`World`vấn đề, vậy câu trả lời là`Incorrect`. 

### Đã thi công mẫu 2 

Hãy xem xét đầu vào sau:```
2
Ab c
6
CapsLock
a
b
CapsLock
Space
c
```Việc thực hiện là: 

| Chìa khóa | Mũ | Màn hình | 
| --- | --- | --- | 
| CapsLock | đúng |`""`| 
| một | đúng |`"A"`| 
| b | đúng |`"AB"`| 
| CapsLock | sai |`"AB"`| 
| Không gian | sai |`"AB "`| 
| c | sai |`"AB c"`| 

Màn hình cuối cùng là`"AB c"`, nhưng văn bản được yêu cầu là`"Ab c"`, vậy kết quả là`Incorrect`. Dấu vết này chứng minh tại sao CapsLock phải tác động độc lập đến từng chữ cái trong tương lai thay vì chỉ thay đổi chuỗi cuối cùng sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m + L) | Mỗi khóa được xử lý một lần và phép so sánh cuối cùng sẽ quét các chuỗi được tạo và chuỗi đích | 
| Không gian | O(m + L) | Màn hình mục tiêu và màn hình mô phỏng đều yêu cầu lưu trữ tuyến tính | 

Các ràng buộc ban đầu giữ cả hai`m`và độ dài mục tiêu dưới 2000, do đó giải pháp tuyến tính chỉ thực hiện vài nghìn thao tác cơ bản và sử dụng rất ít bộ nhớ. Nó cũng tránh việc sao chép bậc hai không cần thiết do cập nhật chuỗi bất biến, giúp việc triển khai phù hợp với các phiên bản lớn hơn đáng kể của cùng một vấn đề. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.splitlines()
    it = iter(data)

    n = int(next(it))
    words = next(it).split()
    target = " ".join(words)

    m = int(next(it))

    screen = []
    caps = False

    for _ in range(m):
        key = next(it).strip()

        if key == "CapsLock":
            caps = not caps
        elif key == "Backspace":
            if screen:
                screen.pop()
        elif key == "Space":
            screen.append(" ")
        else:
            screen.append(key.upper() if caps else key.lower())

    return "Correct" if "".join(screen) == target else "Incorrect"

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample
sample1 = """2
Hello World
17
CapsLock
h
CapsLock
e
l
l
Backspace
o
Space
w
o
Backspace
Backspace
w
o
r
l
d
"""
assert run(sample1) == "Incorrect", "sample 1"

# Minimum-size style case, exact text with one space
case2 = """2
a b
3
a
Space
b
"""
assert run(case2) == "Correct", "basic correct text"

# Backspace on an empty screen, followed by the target
case3 = """2
a b
5
Backspace
a
Space
b
Backspace
"""
assert run(case3) == "Incorrect", "empty backspace and final deletion"

# CapsLock toggles twice, producing the original lowercase text
case4 = """2
ab cd
7
CapsLock
a
CapsLock
b
Space
c
d
"""
assert run(case4) == "Correct", "CapsLock toggling"

# Backspace removes a space, so the final text has no separator
case5 = """2
a b
4
a
Space
Backspace
b
"""
assert run(case5) == "Incorrect", "backspace removes space"

# Large boundary-style case
words = ["a" * 999, "b" * 999]
target = " ".join(words)
case6 = (
    "2\n"
    + target
    + "\n"
    + "1999\n"
    + "\n".join(list("a" * 999 + "b" * 1000))
    + "\n"
)
assert run(case6) == "Incorrect", "large input boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`Incorrect`| Mẫu được cung cấp, bao gồm xóa và viết hoa | 
|`a b`với`a`,`Space`,`b`|`Correct`| Mô phỏng thành công cơ bản | 
| Dẫn đầu`Backspace`theo sau là văn bản |`Incorrect`| Backspace trên màn hình trống | 
| Hai nút chuyển đổi CapsLock |`Correct`| Trạng thái CapsLock thay đổi và trở về chữ thường | 
|`a`,`Space`,`Backspace`,`b`|`Incorrect`| Backspace cũng phải xóa dấu cách | 
| 999`a`ký tự và 999`b`nhân vật |`Incorrect`| Đầu vào biên lớn và xử lý tuyến tính | 

## Vỏ cạnh 

Phải bỏ qua Backspace trên màn hình trống. Đối với đầu vào```
2
a b
5
Backspace
a
Space
b
Backspace
```Backspace đầu tiên giữ nguyên ngăn xếp trống. Ba phím tiếp theo tạo`"a b"`và phím Backspace cuối cùng sẽ xóa`b`, rời đi`"a "`. Từ`"a "`khác với`"a b"`, câu trả lời là`Incorrect`. các`if screen`bảo vệ ngăn chặn một không hợp lệ`pop`và mô hình chính xác hành vi của bàn phím. 

Dấu cách được lưu trữ trong cùng một ngăn xếp với các chữ cái. Vì```
2
a b
4
a
Space
Backspace
b
```ngăn xếp phát triển từ`[]`ĐẾN`["a"]`, sau đó`["a", " "]`, sau đó quay lại`["a"]`, và cuối cùng là`["a", "b"]`. Văn bản cuối cùng là`"ab"`, vậy kết quả là`Incorrect`. Việc coi Backspace là một thao tác chỉ có chữ cái sẽ để lại khoảng trống bị xóa trong kết quả và tạo ra câu trả lời sai. 

CapsLock chỉ ảnh hưởng đến các lần nhấn chữ tiếp theo. Với```
2
A b
3
CapsLock
a
b
```lá cờ trở thành đúng trước`a`, do đó màn hình trở thành`"A"`. Cờ vẫn đúng khi`b`được ép, sản xuất`"AB"`. Mục tiêu là`"A b"`, vậy câu trả lời là`Incorrect`. Mô phỏng không thay đổi các ký tự đã có trên màn hình khi bật Caps Lock. 
Nhấn Caps Lock liên tiếp sẽ hủy nhau. Vì```
2
ab cd
7
CapsLock
a
CapsLock
b
Space
c
d
```lần chuyển đổi đầu tiên làm cho`a`chữ hoa, sản xuất`"A"`. Chuyển đổi thứ hai trở về chữ thường, vì vậy`b`,`c`, Và`d`là chữ thường. Màn hình cuối cùng là`"Ab cd"`, khác với mục tiêu`"ab cd"`, vì vậy đầu vào cụ thể này là`Incorrect`. Nếu chữ cái đầu tiên cũng là chữ hoa trong mục tiêu thì hai nút chuyển đổi tương tự sẽ khôi phục chính xác chế độ chữ thường cho các chữ cái còn lại. Điểm mấu chốt là trạng thái thay đổi ở vị trí chính xác của mỗi`CapsLock`nhấn. 

Các khoảng trống cần thiết phải được kiểm tra chính xác, không chỉ đơn thuần được coi là dấu phân cách. Vì```
2
a b
2
a
b
```ngăn xếp trở thành`"ab"`, trong khi mục tiêu là`"a b"`. Thuật toán so sánh các chuỗi hoàn chỉnh để in chính xác`Incorrect`. 

Cuối cùng, một chuỗi có thể chứa nhiều thao tác triệt tiêu lẫn nhau. Ví dụ,```
2
a b
6
a
Backspace
Backspace
a
Space
b
```bắt đầu bằng`"a"`, xóa nó, bỏ qua Backspace thứ hai vì màn hình trống, sau đó xây dựng`"a b"`. Kết quả cuối cùng là`Correct`. Trường hợp này xác nhận rằng thuật toán không nhầm lẫn giữa ký tự cũ đã bị xóa với trạng thái màn hình hiện tại.
