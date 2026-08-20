---
title: "CF 102203A - \u0414\u043e\u0431\u0440\u043e \u043f\u043e\u0436\u0430\u043b\u043e\u0432\u0430\u0442\u044c \u043d\u0430 \u0424\u043b\u043e\u0440\u0438\u043d\u0443!"
description: "Chúng tôi có hai báo cáo nhị phân về việc giao hàng tới n hành tinh. Đối với mỗi hành tinh, báo cáo đầu tiên cho chúng ta biết liệu cả hai loại kryt có được chuyển đến đó hay không. Báo cáo thứ hai cho chúng tôi biết liệu có ít nhất một loại được giao ở đó hay không."
date: "2026-08-18T11:17:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "A"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 49
verified: true
draft: false
---

[CF 102203A - \u0414\u043e\u0431\u0440\u043e \u043f\u043e\u0436\u0430\u043b\u043e\u0432\u0430\u0442\u044c \u043d\u0430 \u0424\u043b\u043e\u0440\u0438\u043d\u0443!](https://codeforces.com/problemset/problem/102203/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai báo cáo nhị phân về việc giao hàng tới`n`các hành tinh. Đối với mỗi hành tinh, báo cáo đầu tiên cho chúng ta biết liệu cả hai loại kryt có được chuyển đến đó hay không. Báo cáo thứ hai cho chúng tôi biết liệu có ít nhất một loại được giao ở đó hay không. 

Chúng tôi cần một báo cáo nhị phân thứ ba trong đó vị trí`i`là`1`chính xác là khi hành tinh này nhận được một loại kryt chứ không phải cả hai. Nói cách khác, chúng ta cần phân biệt các hành tinh nhận được chính xác một loại với các hành tinh nhận được cả hai loại hoặc không nhận được cả hai loại. 

Đối với một hành tinh, hãy đặt hai chỉ số phân phối thực tế là`a`Và`b`. Báo cáo đầu tiên chứa`a AND b`, trong khi cái thứ hai chứa`a OR b`. Giá trị mong muốn là`1`khi nào chính xác là một trong`a`Và`b`là`1`, đó chính xác là hoạt động XOR. Từ`a AND b`đã được đưa ra bởi báo cáo đầu tiên và`a OR b`đến giây, giá trị mong muốn chỉ đơn giản là`S1[i] XOR S2[i]`. 

Ràng buộc`n <= 10^5`có nghĩa là bản thân đầu vào chỉ chứa khoảng`10^5`ký tự, do đó, thuật toán xử lý mọi vị trí một lần là đủ nhanh. Một thuật toán với công thức bậc hai sẽ thực hiện xung quanh`10^10`hoạt động ở kích thước tối đa và sẽ không phù hợp với giới hạn một giây. Các thuật toán hàm mũ vượt xa phạm vi khả thi. 

Trường hợp cạnh đầu tiên là một hành tinh không có loại nào được phân phối. Ví dụ,```
1
0
0
```Câu trả lời là`0`. Một cách tiếp cận bất cẩn giải thích số 0 của báo cáo thứ hai bằng cách nào đó cho thấy sự khác biệt sẽ là sai, bởi vì không có sự chuyển giao nào cả. 

Trường hợp cạnh thứ hai là một hành tinh trong đó cả hai loại đều được phân phối:```
1
1
1
```Câu trả lời là một lần nữa`0`. Báo cáo đầu tiên đã cho chúng ta biết rằng cả hai loại đều có mặt, vì vậy hành tinh này không được xuất hiện trong báo cáo được yêu cầu. Chỉ sử dụng báo cáo thứ hai sẽ tạo ra sai sót`1`. 

Trường hợp hữu ích thứ ba có các vị trí liền kề với các tình huống khác nhau:```
4
0101
1111
```Câu trả lời là`1010`. Tại các vị trí có báo cáo khác nhau, chính xác một loại sẽ được phân phối. Tại các vị trí mà họ đồng ý, cả hai loại hoặc không loại nào được giao. 

## Phương pháp tiếp cận 

Một giải pháp hoàn toàn mạnh mẽ có thể hình dung ra mọi cấu hình phân phối có thể có cho hai loại trên tất cả`n`các hành tinh. Có hai quyết định nhị phân cho mỗi hành tinh, vì vậy có`2^(2n) = 4^n`các cấu hình có thể. Đối với mỗi cấu hình, chúng ta có thể xây dựng hai báo cáo, so sánh chúng với dữ liệu đầu vào và đưa ra câu trả lời tương ứng nếu chúng khớp. Điều này đúng về mặt logic vì nó xem xét mọi nhiệm vụ phân phối cơ bản có thể xảy ra, nhưng tại`n = 10^5`số lượng cấu hình là`4^100000`, lớn đến mức không thể hiểu được. 

Brute-force hoạt động vì nó tìm kiếm toàn bộ không gian của các trạng thái phân phối có thể có, nhưng việc tìm kiếm đó là không cần thiết vì mọi hành tinh đều độc lập với mọi hành tinh khác. Chúng ta không cần phải xây dựng lại hai bộ phân phối thực tế. Đối với một vị trí, báo cáo đầu tiên cho biết liệu hai bit phân phối có đồng thời hay không`1`, và phần thứ hai cho biết liệu có ít nhất một`1`. Hai thông tin đó hoàn toàn xác định xem có chính xác một thông tin hay không. 

Quan sát quan trọng là`S1[i]`là`a AND b`Và`S2[i]`là`a OR b`. Chính xác là một trong`a`Và`b`đúng khi hai giá trị này khác nhau. Vì vậy, câu trả lời ở mọi vị trí là`S1[i] XOR S2[i]`. Chúng ta có thể quét hai chuỗi một lần và tạo ra câu trả lời theo từng ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và hai chuỗi nhị phân`S1`Và`S2`. Mỗi ký tự ở chỉ mục`i`mô tả cùng một hành tinh trong cả hai báo cáo. 
2. Tạo một chuỗi kết quả trống. Chúng ta sẽ thêm một ký tự cho mỗi hành tinh nên kết quả sẽ có chính xác`n`nhân vật. 
3. Đối với mọi vị trí`i`từ`0`bởi vì`n - 1`, so sánh`S1[i]`Và`S2[i]`. Nếu chúng bằng nhau thì nối thêm`0`. Nếu chúng khác nhau, hãy nối thêm`1`. 

Lý do là sự bình đẳng có nghĩa là cả hai loại đều được phân phối hoặc không có loại nào được phân phối. Sự khác biệt có nghĩa là một báo cáo cho biết "cả hai" trong khi báo cáo kia cho biết "ít nhất một", điều này chỉ có thể xảy ra khi một loại được phân phối chính xác. 
4. In chuỗi nhị phân thu được. 

### Tại sao nó hoạt động 

Đối với mỗi hành tinh, hãy`a`Và`b`biểu thị liệu hai loại kryt có được giao hay không. Báo cáo đầu tiên chứa`a AND b`, và cái thứ hai chứa`a OR b`. Nếu cả hai`a`Và`b`bằng 0, cả hai giá trị báo cáo đều bằng 0. Nếu cả hai đều là một thì cả hai giá trị báo cáo đều là một. Nếu chính xác một là một thì giá trị AND bằng 0 và giá trị OR là một. Vì vậy câu trả lời mong muốn là`1`chính xác khi hai ký tự báo cáo khác nhau, đó chính xác là`S1[i] XOR S2[i]`. Vì lý do này áp dụng độc lập cho mọi vị trí nên chuỗi được xây dựng hoàn chỉnh là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s1 = input().strip()
    s2 = input().strip()

    ans = ''.join('1' if a != b else '0' for a, b in zip(s1, s2))
    print(ans)

if __name__ == "__main__":
    solve()
```Ba thao tác nhập đầu tiên đọc số lượng hành tinh và hai báo cáo.`strip()`xóa dòng mới mà không thay đổi bất kỳ ký tự nhị phân có ý nghĩa nào. 

các`zip(s1, s2)`biểu thức ghép nối vị trí báo cáo theo vị trí. Đối với mỗi cặp, biểu thức điều kiện tạo ra`1`khi các ký tự khác nhau và`0`khi chúng bằng nhau, triển khai chính xác XOR cho các ký tự nhị phân. 

sử dụng`join`xây dựng toàn bộ chuỗi câu trả lời một cách hiệu quả. Không cần chuyển đổi số nguyên, mặt nạ bit hoặc lập chỉ mục rõ ràng và không có phép tính ranh giới nào ngoài thực tế là cả hai chuỗi đều có độ dài`n`. 

Biến`n`được đọc vì nó là một phần của định dạng đầu vào, mặc dù bản thân cấu trúc có thể dựa vào hai chuỗi có độ dài cần thiết một cách an toàn. Số nguyên Python không có vấn đề tràn ở đây và thuật toán chỉ thực hiện công việc tuyến tính. 

## Ví dụ đã hoạt động 

Đoạn trích câu lệnh được cung cấp ở đây không chứa các giá trị đầu vào và đầu ra mẫu thực tế, vì vậy các dấu vết sau đây sử dụng hai đầu vào nhỏ để thực hiện hai tình huống có thể xảy ra. 

### Ví dụ 1```
4
0101
1111
```Hai báo cáo được xử lý như sau. 

| Vị trí |`S1[i]`|`S2[i]`| Khác biệt? | Nhân vật trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | Có | 1 | 
| 1 | 1 | 1 | Không | 0 | 
| 2 | 0 | 1 | Có | 1 | 
| 3 | 1 | 1 | Không | 0 | 

Câu trả lời kết quả là`1010`. Vị trí`0`Và`2`có ít nhất một lần giao hàng nhưng không phải cả hai, trong khi các vị trí`1`Và`3`có cả hai loại. 

### Ví dụ 2```
5
00000
10101
```| Vị trí |`S1[i]`|`S2[i]`| Khác biệt? | Nhân vật trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | Có | 1 | 
| 1 | 0 | 0 | Không | 0 | 
| 2 | 0 | 1 | Có | 1 | 
| 3 | 0 | 0 | Không | 0 | 
| 4 | 0 | 1 | Có | 1 | 

Câu trả lời kết quả là`10101`. Vì báo cáo đầu tiên không chứa vị trí mà cả hai loại được phân phối nên mọi`1`trong báo cáo thứ hai trực tiếp đại diện cho một hành tinh nhận được chính xác một loại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí của hai báo cáo được kiểm tra một lần. | 
| Không gian | O(n) | Chuỗi nhị phân kết quả chứa`n`nhân vật. | 

Với`n <= 10^5`, thuật toán chỉ thực hiện một số thao tác ký tự tuyến tính. Bản thân đầu vào và đầu ra đã có kích thước`O(n)`, do đó đây là phương án tối ưu tiệm cận vì ít nhất chúng ta phải đọc báo cáo và đưa ra câu trả lời. 

## Trường hợp thử nghiệm 

Đoạn trích câu lệnh được cung cấp không có giá trị mẫu thực tế, vì vậy bộ thử nghiệm bên dưới sử dụng hai ví dụ đã làm ở trên làm mẫu đại diện và thêm các trường hợp cho đầu vào nhỏ nhất, báo cáo bằng nhau, kích thước đầu vào tối đa và các ranh giới xen kẽ.```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s1 = input().strip()
    s2 = input().strip()

    ans = ''.join('1' if a != b else '0' for a, b in zip(s1, s2))
    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_input = sys.stdin.readline

    try:
        sys.stdin = io.StringIO(inp)
        return_value = io.StringIO()

        old_stdout = sys.stdout
        sys.stdout = return_value
        try:
            solve()
        finally:
            sys.stdout = old_stdout

        return return_value.getvalue().strip()
    finally:
        sys.stdin = old_stdin

# Worked example 1
assert run("4\n0101\n1111\n") == "1010", "worked example 1"

# Worked example 2
assert run("5\n00000\n10101\n") == "10101", "worked example 2"

# Minimum-size input
assert run("1\n0\n0\n") == "0", "minimum size"

# Both reports are identical, so every position is 0
assert run("6\n101010\n101010\n") == "000000", "identical reports"

# Maximum-size input
n = 100000
s1 = "0" * n
s2 = "1" * n
assert run(f"{n}\n{s1}\n{s2}\n") == "1" * n, "maximum size"

# Boundary and alternating positions
assert run("8\n00110011\n01100110\n") == "01010101", "alternating differences"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 0101 / 1111`|`1010`| Các quan điểm hỗn hợp trong đó các báo cáo đồng ý và không đồng ý | 
|`5 / 00000 / 10101`|`10101`| Trường hợp không có hành tinh nào nhận được cả hai loại | 
|`1 / 0 / 0`|`0`| Kích thước đầu vào tối thiểu và trường hợp không phân phối | 
|`6 / 101010 / 101010`|`000000`| Cả hai báo cáo đều giống nhau nên không hành tinh nào nhận được chính xác một loại | 
|`100000 / 000...0 / 111...1`|`111...1`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 
|`8 / 00110011 / 01100110`|`01010101`| Sự khác biệt xen kẽ và vị trí ranh giới | 

## Vỏ cạnh 

Trường hợp không giao hàng được biểu thị bằng các số 0 bằng nhau. Ví dụ,```
1
0
0
```Ở vị trí duy nhất,`S1[0]`Và`S2[0]`bằng nhau nên thuật toán sẽ nối thêm`0`. Đầu ra là`0`, chỉ ra chính xác rằng cả hai loại đều không được phân phối. 

Trường hợp cả hai loại được thể hiện bằng những trường hợp bằng nhau:```
1
1
1
```Ở đây hai báo cáo lại đồng ý, do đó thuật toán tạo ra`0`. Báo cáo đầu tiên nói rằng cả hai loại đều có mặt, trong khi báo cáo thứ hai xác nhận rằng có ít nhất một loại hiện diện. Vì báo cáo được yêu cầu chỉ chứa các hành tinh thuộc đúng một loại nên hành tinh này phải được loại trừ. 

Một trường hợp trong đó các báo cáo khác nhau chứng tỏ tình trạng tích cực:```
1
0
1
```Báo cáo đầu tiên cho biết cả hai loại không được phân phối đồng thời, trong khi báo cáo thứ hai cho biết ít nhất một loại được phân phối. Tình huống duy nhất có thể xảy ra là có đúng một loại được phân phối, do đó thuật toán sẽ so sánh`0`Và`1`, nhận thấy sự khác biệt và đưa ra kết quả`1`. 

Trường hợp kích thước tối đa có cùng logic ở mọi vị trí. Nếu như`n = 100000`,`S1`bao gồm`100000`số không và`S2`bao gồm`100000`những cái, mỗi cặp đều khác nhau, vì vậy câu trả lời là`100000`những cái đó. Thuật toán thực hiện một so sánh cho mỗi vị trí và không bao giờ cần các vòng lặp lồng nhau, do đó, đầu vào lớn vẫn nằm trong độ phức tạp tuyến tính dự định.
