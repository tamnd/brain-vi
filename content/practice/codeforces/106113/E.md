---
title: "CF 106113E - Una extra\u00f1a thành công\u00f3n"
description: "Giáo viên xây dựng một \"bảng chữ cái vô hạn\" bằng cách lặp lại các chữ cái từ a đến z mãi mãi. Mỗi khối gồm 26 chữ cái được bắt đầu bằng số lượng chu kỳ bảng chữ cái hoàn chỉnh xuất hiện trước nó. Một vài giá trị đầu tiên là: 0a, 0b, ..., 0z, 1a, 1b, ..., 1z, 2a, ..."
date: "2026-06-25T11:38:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106113
codeforces_index: "E"
codeforces_contest_name: "Coding Cup TecNM 2025"
rating: 0
weight: 106113
solve_time_s: 43
verified: true
draft: false
---

[CF 106113E - Una extra\u00f1a sucesi\u00f3n](https://codeforces.com/problemset/problem/106113/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thầy giáo xây dựng “bảng chữ cái vô hạn” bằng cách lặp lại các chữ cái`a`bởi vì`z`mãi mãi. Mỗi khối gồm 26 chữ cái được bắt đầu bằng số lượng chu kỳ bảng chữ cái hoàn chỉnh xuất hiện trước nó. 

Một vài giá trị đầu tiên là:`0a, 0b, ..., 0z, 1a, 1b, ..., 1z, 2a, ...`Nếu chúng ta đánh số các vị trí này bắt đầu từ 1 thì:`0a`đang ở vị trí 1,`0b`tại vị trí 2, ...,`0z`ở vị trí 26,`1a`ở vị trí 27, v.v. 

Trình tự`s`thu được bằng cách lấy dãy số$$5,\ 8,\ 13,\ 20,\ 29,\dots$$và thay thế mỗi số bằng giá trị được lưu tại vị trí đó trong bảng chữ cái vô hạn. Tuyên bố đưa ra sự khởi đầu của`s`:`0e, 0h, 0m, 0t, 1c, ...`Nhiệm vụ là hoạt động ngược lại. Cho một giá trị của bảng chữ cái vô hạn, xác định vị trí của`s`nó xuất hiện. Nếu nó không bao giờ xuất hiện, hãy in`-1`. 

Quan sát chính xuất phát từ việc chuyển đổi các giá trị được hiển thị của`s`trở lại vị trí bảng chữ cái của họ: 

| Giá trị | Vị trí bảng chữ cái | 
| --- | --- | 
| 0e | 5 | 
| 0h | 8 | 
| 0m | 13 | 
| 0t | 20 | 
| 1c | 29 | 

Sự khác biệt là:$$3,\ 5,\ 7,\ 9,\dots$$là các số lẻ liên tiếp. Điều đó có nghĩa là dãy số cơ bản là$$n^2 + 4$$vì$n = 1, 2, 3, \dots$. 

Kích thước đầu vào rất nhỏ. Tiền tố số có thể lớn bằng$10^8$, do đó vị trí bảng chữ cái thu được nhiều nhất là khoảng$2.6 \times 10^9$. Một giải pháp số học theo thời gian không đổi là quá đủ. 

Một lỗi phổ biến là quên rằng vị trí bảng chữ cái được lập chỉ mục 1. 

Ví dụ:```
Input:
0a
```Vị trí bảng chữ cái là 1, không phải 0. Vì$$1 - 4 = -3$$không phải là một hình vuông hoàn hảo, câu trả lời đúng là:```
-1
```Một sai lầm dễ mắc phải khác là chấp nhận bất kỳ căn bậc hai nào sau khi làm tròn dấu phẩy động. 

Ví dụ:```
Input:
0d
```Vị trí bảng chữ cái là 4. Khi đó:$$4 - 4 = 0$$cho căn bậc hai bằng 0. Chuỗi bắt đầu tại$n=1$, do đó vị trí 0 không tồn tại. Câu trả lời đúng là:```
-1
```Đây là một trong những trường hợp mẫu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra các giá trị của chuỗi$$n^2 + 4$$chuyển đổi từng giá trị thành biểu diễn bảng chữ cái vô hạn và so sánh nó với đầu vào. Điều này hoạt động vì trình tự rõ ràng. 

Vấn đề là quyết định tạo ra bao xa. Vị trí bảng chữ cái được biểu thị bởi đầu vào có thể lớn hơn hai tỷ, do đó chỉ số chuỗi tương ứng có thể vào khoảng năm mươi nghìn. Con số đó vẫn chưa lớn nhưng việc tạo và chuyển đổi các giá trị là công việc không cần thiết. 

Cấu trúc thực sẽ hiển thị khi chúng ta dịch dữ liệu đầu vào trở lại vị trí bảng chữ cái số của nó. 

Nếu đầu vào là`pX`, Ở đâu`X`là một chữ cái thì vị trí bảng chữ cái của nó là$$N = 26p + \text{letterIndex}(X)$$với`a = 1`,`b = 2`, ...,`z = 26`. 

Dãy số chứa chính xác các số$$n^2 + 4.$$Vì vậy, câu hỏi trở thành:$$n^2 + 4 = N$$hoặc$$n^2 = N - 4.$$Bây giờ chúng ta chỉ cần kiểm tra xem$N-4$là một hình vuông hoàn hảo dương. Nếu đúng như vậy thì căn bậc hai đó chính xác là vị trí bên trong dãy. Nếu không thì giá trị không bao giờ xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O($\sqrt N$) | O(1) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Tách tiền tố số và chữ cái cuối cùng. 
3. Chuyển chữ cái sang vị trí của nó trong bảng chữ cái, trong đó`a = 1`Và`z = 26`. 
4. Tính vị trí bảng chữ cái vô hạn:$$N = 26 \cdot prefix + letterPosition$$Điều này diễn ra trực tiếp từ việc xây dựng bảng chữ cái vô hạn. 

1. Tính toán:$$x = N - 4$$vì mọi phần tử của dãy số ẩn đều có dạng$n^2 + 4$. 

1. Nếu$x \le 0$, in`-1`. 

Trình tự bắt đầu lúc$n=1$, Vì thế$n=0$không hợp lệ. 

1. Tính căn bậc hai số nguyên$r = \lfloor \sqrt{x} \rfloor$. 
2. Nếu$r^2 = x$, in$r$. Nếu không thì in`-1`. 

### Tại sao nó hoạt động 

Mỗi giá trị của`s`xuất phát từ dãy số$n^2 + 4$. Một chuỗi từ bảng chữ cái vô hạn tương ứng với chính xác một vị trí bảng chữ cái$N$. Chuỗi xuất hiện trong`s`khi và chỉ khi vị trí của nó thỏa mãn$$N = n^2 + 4$$với một số nguyên dương nào đó$n$. 

Sắp xếp lại mang lại$$n^2 = N - 4.$$Một số nguyên dương$n$tồn tại chính xác khi$N-4$là một hình vuông hoàn hảo dương. Trong trường hợp đó căn bậc hai duy nhất là vị trí của dãy. Không có câu trả lời nào khác là có thể. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

s = input().strip()

letter = s[-1]
prefix = int(s[:-1])

pos = 26 * prefix + (ord(letter) - ord('a') + 1)

x = pos - 4

if x <= 0:
    print(-1)
else:
    r = isqrt(x)
    print(r if r * r == x else -1)
```Phần đầu tiên trích xuất tiền tố số và chữ cái cuối cùng. 

Việc chuyển đổi sang vị trí bảng chữ cái sử dụng thực tế là mỗi khối hoàn thành đóng góp 26 vị trí. Bức thư đóng góp một giá trị từ 1 đến 26. 

Sau khi có được`pos`, vấn đề giảm xuống còn việc kiểm tra xem`pos - 4`là một hình vuông hoàn hảo dương. 

sử dụng`math.isqrt`tránh các vấn đề về độ chính xác của dấu phẩy động. Căn bậc hai có dấu phẩy động có thể phân loại không chính xác các giá trị rất lớn gần một hình vuông hoàn hảo. Căn bậc hai số nguyên cho kết quả chính xác và kiểm tra cuối cùng`r * r == x`xác nhận trạng thái vuông góc hoàn hảo. 

điều kiện`x <= 0`là quan trọng. Ví dụ,`0d`sản xuất`x = 0`, nhưng trình tự bắt đầu từ$n=1$, vậy đáp án phải là`-1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3z
```| Biến | Giá trị | 
| --- | --- | 
| tiền tố | 3 | 
| lá thư | z | 
| thưVị trí | 26 | 
| N | 104 | 
| x = N-4 | 100 | 
| r | 10 | 

Từ$10^2 = 100$, câu trả lời là:```
10
```Điều này phù hợp với mẫu. 

### Ví dụ 2 

đầu vào:```
104d
```| Biến | Giá trị | 
| --- | --- | 
| tiền tố | 104 | 
| lá thư | d | 
| thưVị trí | 4 | 
| N | 2708 | 
| x = N-4 | 2704 | 
| r | 52 | 

Từ$52^2 = 2704$, câu trả lời là:```
52
```Điều này chứng tỏ rằng ngay cả các tiền tố lớn cũng giảm xuống thành một phép thử bình phương hoàn hảo đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một vài phép tính số học và một căn bậc hai số nguyên | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ | 

Vị trí bảng chữ cái lớn nhất có thể chỉ là vài tỷ, vì vậy mọi phép tính đều phù hợp thoải mái với các kiểu số nguyên tiêu chuẩn. Giải pháp hằng số thời gian dễ dàng thỏa mãn các giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys
from math import isqrt

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = sys.stdin.readline().strip()

    letter = s[-1]
    prefix = int(s[:-1])

    pos = 26 * prefix + (ord(letter) - ord('a') + 1)

    x = pos - 4

    if x <= 0:
        return "-1\n"

    r = isqrt(x)
    return f"{r if r * r == x else -1}\n"

# provided samples
assert run("0e\n") == "1\n", "sample 1"
assert run("3z\n") == "10\n", "sample 2"
assert run("104d\n") == "52\n", "sample 3"
assert run("0d\n") == "-1\n", "sample 4"

# custom cases
assert run("0a\n") == "-1\n", "smallest alphabet value"
assert run("0h\n") == "2\n", "second sequence element"
assert run("1c\n") == "5\n", "crossing alphabet block boundary"
assert run("99999999z\n") == "-1\n", "large non-member"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0a`|`-1`| Vị trí bảng chữ cái nhỏ nhất có thể | 
|`0h`|`2`| Phát hiện bình phương hoàn hảo cơ bản | 
|`1c`|`5`| Xử lý chính xác trên các khối 26 chữ cái | 
|`99999999z`|`-1`| Kiểm tra số học và bình phương có giá trị lớn | 

## Vỏ cạnh 

Hãy xem xét:```
0a
```Vị trí bảng chữ cái là:$$1$$Sau đó:$$1 - 4 = -3$$Vì giá trị là âm nên không có số nguyên dương$n$thỏa mãn$n^2=-3$. Thuật toán in ngay lập tức`-1`. 

Bây giờ hãy xem xét:```
0d
```Vị trí bảng chữ cái là:$$4$$Sau đó:$$4 - 4 = 0$$Căn bậc hai là 0, nhưng dãy bắt đầu bằng$n=1$. Thuật toán bác bỏ`x <= 0`và in chính xác:```
-1
```Cuối cùng, hãy xem xét:```
1c
```Vị trí bảng chữ cái là:$$26 + 3 = 29$$Sau đó:$$29 - 4 = 25 = 5^2$$Thuật toán tìm căn bậc hai chính xác 5 và đưa ra:```
5
```Điều này xác nhận rằng số khối và phần đóng góp của chữ cái được kết hợp chính xác khi chuyển đổi về vị trí bảng chữ cái số.
