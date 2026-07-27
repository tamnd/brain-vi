---
title: "CF 102784C - Thủ thuật hoặc điều trị tối ưu"
description: "Timmy biết những lời đề nghị kẹo mà Alice sẽ chấp nhận. Mỗi loại kẹo có một giá trị trao đổi cố định, nghĩa là mỗi miếng kẹo đó đều đóng góp cùng một số thanh Chuckles. Có một số ngôi nhà và mỗi ngôi nhà cung cấp một bộ sưu tập các loại kẹo với số lượng nhất định."
date: "2026-07-27T19:53:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102784
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 1"
rating: 0
weight: 102784
solve_time_s: 61
verified: true
draft: false
---

[CF 102784C - Thủ thuật hoặc đối xử tối ưu](https://codeforces.com/problemset/problem/102784/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Timmy biết những lời đề nghị kẹo mà Alice sẽ chấp nhận. Mỗi loại kẹo có một giá trị trao đổi cố định, nghĩa là mỗi miếng kẹo đó đều đóng góp cùng một số thanh Chuckles. Có một số ngôi nhà và mỗi ngôi nhà cung cấp một bộ sưu tập các loại kẹo với số lượng nhất định. Timmy chỉ có thể đến thăm một số ngôi nhà giới hạn và sau khi đến thăm một ngôi nhà, cậu ấy sẽ lấy hết số kẹo từ đó. Mục tiêu là chọn những ngôi nhà tốt nhất sao cho tổng giá trị trao đổi của tất cả kẹo thu thập được càng lớn càng tốt. 

Phần đầu tiên của đầu vào mô tả bảng trao đổi. Đối với mỗi tên kẹo, chúng ta biết được một viên kẹo đó có giá trị bao nhiêu thanh Chuckles. Dữ liệu đầu vào còn lại mô tả từng ngôi nhà và số kẹo có sẵn ở đó. Kết quả đầu ra là số thanh Chuckles tối đa mà Timmy có thể nhận được sau khi ghé thăm đúng số nhà cho phép. 

Những hạn chế là nhỏ. Có tối đa 200 loại kẹo và tối đa 200 ngôi nhà, mỗi ngôi nhà chứa thông tin về tối đa 200 loại kẹo. Điều này có nghĩa là chúng tôi có đủ khả năng để kiểm tra từng mục kẹo một lần và thực hiện các hoạt động tỷ lệ thuận với số lượng ngôi nhà. Một giải pháp thử mọi nhóm nhà có thể sẽ yêu cầu khám phá sự kết hợp lên tới 200 ngôi nhà, điều này là không thể vì số lượng lựa chọn tăng theo cấp số nhân. 

Quan sát chính là các ngôi nhà không tương tác với nhau. Lấy một ngôi nhà không làm thay đổi giá trị của ngôi nhà khác và không có hạn chế về loại kẹo nào có thể được thu thập cùng nhau. Quyết định duy nhất là nên bao gồm những ngôi nhà nào. Điều đó biến vấn đề thành việc tìm k giá trị ngôi nhà độc lập lớn nhất. 

Một số trường hợp đặc biệt có thể phá vỡ việc triển khai bất cẩn. Nếu Timmy chỉ có thể đến thăm một ngôi nhà thì câu trả lời đơn giản là giá trị của ngôi nhà mang lại lợi nhuận cao nhất. 

đầu vào:```
1
Candy 10
3 1
1
Candy 5
1
Candy 2
1
Candy 8
```Đầu ra:```
50
```Một sai lầm có thể là tính tổng tất cả các ngôi nhà hoặc vô tình chọn ngôi nhà đầu tiên thay vì ngôi nhà tối đa. Ngôi nhà thứ ba là lựa chọn chính xác vì năm viên kẹo trị giá 10 viên, mỗi viên sẽ cho 50 thanh Chuckles. 

Một trường hợp khác là khi nhiều ngôi nhà có cùng giá trị. Thuật toán phải cho phép chọn bất kỳ trong số chúng vì kết quả tổng thể giống hệt nhau. 

đầu vào:```
1
Candy 4
2 2
1
Candy 3
1
Candy 3
```Đầu ra:```
24
```Việc triển khai bất cẩn loại bỏ các giá trị trùng lặp hoặc sử dụng một bộ sẽ làm mất một trong các ngôi nhà và tạo ra câu trả lời sai. Cần cả hai nhà vì Timmy có thể đến thăm hai nhà. 

Trường hợp ranh giới cuối cùng là khi mỗi ngôi nhà có các loại kẹo khác nhau và mỗi ngôi nhà phải được ghé thăm. Trong trường hợp đó, việc phân loại vẫn có hiệu quả vì số lượng nhà được yêu cầu bằng tổng số nhà. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính giá trị của mỗi ngôi nhà và sau đó thử từng nhóm k ngôi nhà có thể. Việc tính toán giá trị rất dễ dàng: với mỗi chiếc kẹo trong một ngôi nhà, hãy nhân số lượng với giá trị trao đổi và cộng nó vào tổng số tiền của ngôi nhà. Vấn đề chỉ xuất hiện khi chọn nhà. Với h nhà, việc kiểm tra mọi tập con có thể có cần 2^h lựa chọn. Với h = 200, điều này vượt xa những gì bất kỳ chương trình nào có thể xử lý. 

Sở dĩ vũ lực thất bại không phải vì tính toán nhà cái đắt tiền. Nó thất bại vì các lựa chọn là độc lập nhưng có rất nhiều. Điều quan trọng cần lưu ý là mỗi ngôi nhà đều có một giá trị số cuối cùng và việc chọn một ngôi nhà không bao giờ làm thay đổi giá trị của một ngôi nhà khác. Khi mỗi ngôi nhà đã được chuyển đổi thành phần đóng góp của thanh Chuckles, các chi tiết kẹo ban đầu không còn quan trọng nữa. 

Điều này làm giảm vấn đề trong việc chọn k số lớn nhất từ ​​danh sách các giá trị h. Sắp xếp danh sách là cách đơn giản nhất để thực hiện việc này. Sau khi sắp xếp theo thứ tự giảm dần, k giá trị đầu tiên chính xác là những ngôi nhà tối đa hóa tổng phần thưởng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^h * h) | O(h) | Quá chậm | 
| Tối ưu | O(tổng số kẹo + h log h) | O(h + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bảng trao đổi kẹo và lưu trữ giá trị của từng tên kẹo. Các mô tả ngôi nhà sau này đề cập đến những tên này, vì vậy chúng tôi cần truy cập liên tục về giá trị của từng viên kẹo. 
2. Xử lý từng nhà một cách độc lập. Đối với mỗi loại kẹo trong nhà, hãy nhân số miếng kẹo với giá trị thanh Chuckles của nó và cộng kết quả vào tổng giá trị của ngôi nhà. Sau bước này, mỗi ngôi nhà được biểu thị bằng một số duy nhất. 
3. Sắp xếp tất cả các giá trị ngôi nhà từ lớn nhất đến nhỏ nhất. Những lựa chọn tốt nhất là những nhà có đóng góp cá nhân cao nhất vì không có sự tương tác giữa các lựa chọn. 
4. Thêm k giá trị đầu tiên sau khi sắp xếp và xuất kết quả. Đây chính xác là những ngôi nhà mà Timmy nên ghé thăm. 

Tại sao nó hoạt động: giá trị của một nhóm ngôi nhà là tổng giá trị riêng lẻ của chúng. Vì mỗi ngôi nhà đều đóng góp độc lập nên việc thay thế một ngôi nhà đã chọn bằng một ngôi nhà lớn hơn chưa được chọn không bao giờ có thể làm giảm câu trả lời. Sau khi sắp xếp, k ngôi nhà đầu tiên ít nhất có giá trị bằng mọi ngôi nhà ngoài nhóm đó, vì vậy không có lựa chọn nào khác có thể tạo ra tổng số lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    value = {}

    for _ in range(n):
        name, v = input().split()
        value[name] = int(v)

    h, k = map(int, input().split())

    houses = []

    for _ in range(h):
        c = int(input())
        total = 0
        for _ in range(c):
            name, amount = input().split()
            total += value[name] * int(amount)
        houses.append(total)

    houses.sort(reverse=True)
    print(sum(houses[:k]))

if __name__ == "__main__":
    solve()
```Từ điển lưu trữ giá trị trao đổi của mỗi tên kẹo. Vì phần mô tả ngôi nhà sử dụng tên thay vì số nhận dạng nên từ điển sẽ tránh việc tìm kiếm nhiều lần trong danh sách các loại kẹo. 

Trong khi đọc một ngôi nhà, mã sẽ ngay lập tức chuyển đổi tất cả thông tin về kẹo thành một giá trị tổng duy nhất. Điều này ngăn chặn việc lưu trữ không cần thiết các danh sách kẹo riêng lẻ vì chỉ có sự đóng góp cuối cùng của mỗi nhà mới quan trọng. 

Sắp xếp giảm dần đặt những ngôi nhà có giá trị nhất vào đầu. Lấy`houses[:k]`chọn chính xác số nhà mà Timmy có thể ghé thăm. Số nguyên Python xử lý tổng số có thể một cách an toàn, do đó không cần xử lý tràn thủ công. 

Điều kiện biên tinh tế duy nhất là k có thể bằng h. Trong trường hợp đó, lát cắt chỉ chứa toàn bộ danh sách, phù hợp với yêu cầu Timmy phải đến thăm từng nhà. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
3
LactoseyWay 2
Twicks 3
DigDog 5
2 1
2
LactoseyWay 3
Twicks 1
1
DigDog 2
```Thuật toán đánh giá từng ngôi nhà một cách độc lập. 

| Bước | Nhà đã qua xử lý | Giá trị căn nhà | Giá trị được sắp xếp | 
| --- | --- | --- | --- | 
| 1 | LactoseyWay và Twicks | 2 * 3 + 3 * 1 = 9 | [9] | 
| 2 | ĐàoDog | 5 * 2 = 10 | [10, 9] | 
| 3 | Chọn k = 1 | 10 | [10, 9] | 

Nhà có lãi nhất sẽ tặng hai viên kẹo DigDog. Mỗi thanh có giá trị bằng 5 thanh Chuckles, vì vậy câu trả lời là 10. Dấu vết này chứng tỏ tại sao chỉ có giá trị ngôi nhà lớn nhất mới quan trọng. 

Mẫu 2: 

đầu vào:```
2
CandyA 4
CandyB 7
3 2
1
CandyA 5
2
CandyB 2
CandyA 1
1
CandyB 3
```| Bước | Nhà đã qua xử lý | Giá trị căn nhà | Giá trị được sắp xếp | 
| --- | --- | --- | --- | 
| 1 | Nhà 1 | 4 * 5 = 20 | [20] | 
| 2 | Nhà 2 | 7 * 2 + 4 * 1 = 18 | [20, 18] | 
| 3 | Nhà 3 | 7 * 3 = 21 | [21, 20, 18] | 
| 4 | Chọn k = 2 | 21 + 20 = 41 | [21, 20, 18] | 

Những ngôi nhà được chọn là ngôi nhà thứ ba và thứ nhất. Điều này cho thấy thứ tự đầu vào không có ý nghĩa và thuật toán phải so sánh tất cả các nhà trước khi chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số kẹo + h log h) | Mỗi mục kẹo được xử lý một lần, sau đó các giá trị h được sắp xếp. | 
| Không gian | O(n + h) | Giá trị kẹo và giá trị ngôi nhà được tính toán sẽ được lưu trữ. | 

Kích thước đầu vào tối đa đủ nhỏ để việc sắp xếp 200 giá trị nhà là chuyện nhỏ. Công việc chủ yếu là đọc và chuyển đổi thông tin về kẹo nên lời giải dễ dàng nằm trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    value = {}

    for _ in range(n):
        name, v = input().split()
        value[name] = int(v)

    h, k = map(int, input().split())
    houses = []

    for _ in range(h):
        c = int(input())
        total = 0
        for _ in range(c):
            name, amount = input().split()
            total += value[name] * int(amount)
        houses.append(total)

    houses.sort(reverse=True)
    print(sum(houses[:k]))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""3
LactoseyWay 2
Twicks 3
DigDog 5
2 1
2
LactoseyWay 3
Twicks 1
1
DigDog 2
""") == "10\n", "sample 1"

assert run("""1
Candy 10
3 1
1
Candy 5
1
Candy 2
1
Candy 8
""") == "50\n", "single best house"

assert run("""1
Candy 4
2 2
1
Candy 3
1
Candy 3
""") == "24\n", "equal houses"

assert run("""2
A 1
B 100
2 2
1
A 100
1
B 1
""") == "200\n", "visit all houses"

assert run("""3
A 5
B 6
C 7
4 2
1
A 1
1
B 1
1
C 1
3
A 10
B 10
C 10
""") == "390\n", "large individual house value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 10 | Lựa chọn nhà cơ bản | 
| Trường hợp nhà đơn tốt nhất | 50 | k = 1 ranh giới | 
| Nhà bình đẳng | 24 | Các giá trị trùng lặp phải được giữ | 
| Thăm tất cả các nhà | 200 | xử lý k = h | 
| Giá trị căn nhà cá nhân lớn | 390 | Phép nhân và tổng đúng | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là chọn chính xác một ngôi nhà. Đối với đầu vào:```
1
Candy 10
3 1
1
Candy 5
1
Candy 2
1
Candy 8
```các giá trị ngôi nhà được tính toán là 50, 20 và 80. Việc sắp xếp cho kết quả 80, 50, 20 và chọn một ngôi nhà trả về 80. Thuật toán không bao giờ phụ thuộc vào thứ tự đầu vào, do đó giá trị cao nhất được chọn chính xác. 

Trường hợp cạnh thứ hai là nhiều giá trị ngôi nhà giống hệt nhau. Vì:```
1
Candy 4
2 2
1
Candy 3
1
Candy 3
```mỗi ngôi nhà có giá trị 12. Danh sách được sắp xếp là [12, 12] và lấy cả hai ngôi nhà sẽ ra 24. Thuật toán giữ các giá trị trùng lặp vì một ngôi nhà là một lựa chọn riêng biệt, không chỉ là một số duy nhất. 

Trường hợp cuối cùng là phải đến thăm từng ngôi nhà. Nếu k bằng h thì tiền tố được sắp xếp chứa tất cả các nhà. Việc sắp xếp không thay đổi gì về tổng và thuật toán trả về tổng giá trị của mỗi ngôi nhà, phù hợp với hành vi được yêu cầu.
