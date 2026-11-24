---
title: "CF 64B - Biểu thức"
description: "Đầu vào là một biểu thức số học nhỏ được viết dưới dạng chuỗi ba ký tự. Ký tự đầu tiên và thứ ba là các chữ số từ 0 đến 9 và ký tự ở giữa là + hoặc -. Nhiệm vụ là đánh giá biểu thức và in số nguyên kết quả."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "expression-parsing"]
categories: ["algorithms"]
codeforces_contest: 64
codeforces_index: "B"
codeforces_contest_name: "Unknown Language Round 1"
rating: 1500
weight: 64
solve_time_s: 99
verified: true
draft: false
---

[CF 64B - Biểu thức](https://codeforces.com/problemset/problem/64/B) 

**Đánh giá:** 1500 
**Thẻ:** *đặc biệt, phân tích biểu thức 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là một biểu thức số học nhỏ được viết dưới dạng chuỗi ba ký tự. Ký tự thứ nhất và thứ ba là các chữ số từ`0`ĐẾN`9`, và ký tự ở giữa là`+`hoặc`-`. Nhiệm vụ là đánh giá biểu thức và in số nguyên kết quả. 

Ví dụ, đầu vào`1-5`đại diện cho phép trừ`1 - 5`, vì vậy đầu ra đúng là`-4`. đầu vào`7+2`đại diện cho`7 + 2`, vì vậy đầu ra là`9`. 

Các ràng buộc cực kỳ nhỏ vì biểu thức luôn có độ dài cố định. Không cần phân tích thư viện, mã thông báo, đệ quy hoặc cây biểu thức. Một lượng công việc không đổi là đủ, vì chúng ta chỉ cần đọc ba ký tự và áp dụng một phép tính số học. 

Phần tinh tế duy nhất là xử lý phép trừ một cách chính xác khi kết quả trở thành âm. Việc triển khai bất cẩn giả định tất cả các đầu ra đều không âm sẽ thất bại đối với các đầu vào như:```
1-5
```Đầu ra đúng là:```
-4
```Một lỗi dễ mắc phải khác là quên chuyển đổi ký tự thành số nguyên trước khi thao tác với chúng. Ví dụ: nếu ai đó trực tiếp trừ mã ký tự thì kết quả sẽ vô nghĩa. Với đầu vào:```
9-2
```chúng ta phải tính phép trừ số nguyên`9 - 2 = 7`, không phải sự khác biệt giữa các giá trị ASCII. 

Trường hợp cạnh thứ ba bằng không. Những biểu hiện như:```
0-0
```phải sản xuất chính xác:```
0
```Một số triển khai có lỗi vô tình trừ trường hợp đặc biệt và xử lý sai các toán hạng bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra đặc tính của toán tử và tính toán kết quả tương ứng. Vì độ dài biểu thức luôn chính xác là ba nên chúng ta có thể truy cập chữ số đầu tiên, toán tử và chữ số thứ hai theo chỉ mục. Sau khi chuyển đổi các ký tự chữ số thành số nguyên, chúng ta thực hiện cộng hoặc trừ. 

Ngay cả một giải pháp theo phong cách vũ phu cũng có hiệu quả về mặt thời gian ở đây. Người ta có thể kiểm tra thủ công tất cả các biểu thức có thể có từ`0+0`bởi vì`9-9`, lưu trữ chúng trong bảng tra cứu và in kết quả phù hợp. Điều đó vẫn chỉ bao gồm vài trăm phép tính vì chỉ có 200 biểu thức có thể có. Cách tiếp cận này đúng vì mọi đầu vào hợp lệ đều thuộc về tập hữu hạn này, nhưng nó cứng nhắc một cách không cần thiết và khó đọc hơn. 

Quan sát rõ ràng hơn là đầu vào đã mã hóa mọi thứ một cách trực tiếp. Ký tự ở giữa xác định hoàn toàn phép toán số học nào sẽ được áp dụng. Khi chúng ta chuyển đổi hai chữ số thành số nguyên, việc đánh giá sẽ trở thành một phép cộng hoặc phép trừ đơn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 

Biểu thức luôn có dạng`digit operator digit`, do đó độ dài của nó được cố định ở ba ký tự. 
2. Trích xuất chữ số đầu tiên, toán tử và chữ số thứ hai. 

Chữ số đầu tiên nằm ở chỉ số`0`, toán tử tại chỉ mục`1`, và chữ số thứ hai ở chỉ số`2`. 
3. Chuyển các ký tự chữ số thành số nguyên. 

Số học trên các ký tự không chính xác nên chúng ta phải chuyển đổi chúng bằng cách sử dụng`int()`. 
4. Kiểm tra người vận hành. 

Nếu người vận hành là`+`, tính tổng. Nếu không, hãy tính sự khác biệt. 
5. In kết quả. 

### Tại sao nó hoạt động 

Định dạng biểu thức đảm bảo chính xác một toán tử và hai toán hạng có một chữ số. Thuật toán đọc trực tiếp ba thành phần đó và áp dụng quy tắc số học tương ứng. Vì phép cộng và phép trừ được thực hiện chính xác như được xác định trong biểu thức nên giá trị được tạo ra luôn khớp với kết quả toán học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

a = int(s[0])
op = s[1]
b = int(s[2])

if op == '+':
    print(a + b)
else:
    print(a - b)
```Chương trình bắt đầu bằng cách đọc chuỗi biểu thức và xóa dòng mới ở cuối bằng`strip()`. 

Ký tự đầu tiên và thứ ba được chuyển đổi thành số nguyên ngay lập tức. Điều này tránh được những sai sót do thao tác trực tiếp lên nhân vật. Ký tự ở giữa được lưu trữ dưới dạng toán tử. 

Điều kiện kiểm tra xem toán tử có phải là`'+'`. Nếu đúng, chương trình sẽ in tổng. Mặt khác, toán tử hợp lệ duy nhất còn lại là`'-'`, do đó nó in ra kết quả phép trừ. 

Không có vấn đề về ranh giới vì định dạng đầu vào đảm bảo chính xác ba ký tự trong biểu thức. Số nguyên Python cũng xử lý các giá trị âm một cách an toàn, vì vậy các trường hợp như`1-5`làm việc một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1-5
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc chuỗi |`s`|`"1-5"`| 
| Chữ số đầu tiên |`a`|`1`| 
| Nhà điều hành |`op`|`'-'`| 
| Chữ số thứ hai |`b`|`5`| 
| Tính toán |`a - b`|`-4`| 

Toán tử là phép trừ nên thuật toán tính toán`1 - 5`. Ví dụ này xác nhận rằng các câu trả lời phủ định được xử lý chính xác. 

### Ví dụ 2 

đầu vào:```
7+2
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc chuỗi |`s`|`"7+2"`| 
| Chữ số đầu tiên |`a`|`7`| 
| Nhà điều hành |`op`|`'+'`| 
| Chữ số thứ hai |`b`|`2`| 
| Tính toán |`a + b`|`9`| 

Dấu vết này thể hiện nhánh bổ sung. Thuật toán xác định chính xác toán tử và thực hiện phép toán số học phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số truy cập ký tự và một phép tính số học được thực hiện | 
| Không gian | O(1) | Thuật toán lưu trữ một số lượng biến không đổi | 

Giải pháp dễ dàng nằm trong giới hạn vì kích thước đầu vào không bao giờ tăng. Thời gian chạy và mức sử dụng bộ nhớ không đổi bất kể chữ số cụ thể được sử dụng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s = input().strip()

    a = int(s[0])
    op = s[1]
    b = int(s[2])

    if op == '+':
        print(a + b)
    else:
        print(a - b)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout
    return out.getvalue()

# provided sample
assert run("1-5\n") == "-4\n", "sample 1"

# custom cases
assert run("0+0\n") == "0\n", "minimum values"
assert run("9+9\n") == "18\n", "maximum addition result"
assert run("9-0\n") == "9\n", "subtraction with zero"
assert run("0-9\n") == "-9\n", "negative result boundary"
assert run("5-5\n") == "0\n", "equal operands"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0+0`|`0`| Toán hạng nhỏ nhất có thể | 
|`9+9`|`18`| Kết quả phép cộng lớn nhất | 
|`9-0`|`9`| Phép trừ với số 0 | 
|`0-9`|`-9`| Xử lý kết quả tiêu cực | 
|`5-5`|`0`| Toán hạng bằng nhau | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1-5
```Thuật toán trích xuất`a = 1`,`op = '-'`, Và`b = 5`. Vì toán tử là phép trừ nên trong máy tính`1 - 5 = -4`. Đầu ra là:```
-4
```Điều này xác nhận rằng các giá trị âm được xử lý chính xác mà không cần bất kỳ logic đặc biệt nào. 

Bây giờ hãy xem xét:```
0-0
```Các giá trị được trích xuất là`a = 0`,`op = '-'`, Và`b = 0`. Việc tính toán trở thành`0 - 0 = 0`, do đó thuật toán in ra:```
0
```Trường hợp này xác minh rằng phép trừ không vô tình gây ra lỗi dấu khi cả hai toán hạng đều bằng nhau. 

Cuối cùng, hãy xem xét:```
9+9
```Thuật toán đọc`a = 9`,`op = '+'`, Và`b = 9`. Vì toán tử là phép cộng nên nó tính toán`9 + 9 = 18`. Đầu ra trở thành:```
18
```Điều này chứng tỏ rằng kết quả không bị giới hạn ở một chữ số, mặc dù cả hai toán hạng đều có một chữ số.
