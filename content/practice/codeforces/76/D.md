---
title: "CF 76D - Plus và xor"
description: "Chúng ta có hai số nguyên không âm, A và B. Chúng ta cần xây dựng hai số nguyên không âm khác, X và Y, sao cho: - tổng của chúng bằng A - xor theo bit của chúng bằng B Trong số tất cả các cặp hợp lệ, chúng ta phải xuất ra cặp có X nhỏ nhất có thể."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 76
codeforces_index: "D"
codeforces_contest_name: "All-Ukrainian School Olympiad in Informatics"
rating: 1700
weight: 76
solve_time_s: 106
verified: true
draft: false
---

[CF 76D - Plus và xor](https://codeforces.com/problemset/problem/76/D) 

**Đánh giá:** 1700 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên không âm,`A`Và`B`. Chúng ta cần xây dựng hai số nguyên không âm khác,`X`Và`Y`, sao cho: 

- tổng của chúng bằng nhau`A`- xor bitwise của họ bằng`B`Trong số tất cả các cặp hợp lệ, chúng ta phải xuất ra cặp có giá trị nhỏ nhất có thể.`X`. 

Phần thú vị là phép cộng và xor tương tác thông qua các lần mang. Nếu chúng ta chỉ nhìn vào xor thì mọi bit đều hoạt động độc lập. Phép cộng thì khác vì việc mang từ bit thấp hơn sẽ thay đổi bit tiếp theo. Toàn bộ vấn đề thực sự nằm ở việc hiểu hai hoạt động này khớp với nhau như thế nào. 

Các con số có thể lớn như`2^64 - 1`, vì vậy việc sử dụng vũ lực trên tất cả các giá trị có thể là không thể. Thậm chí lặp lại tất cả các ứng cử viên cho`X`sẽ yêu cầu lên tới`2^64`hoạt động hoàn toàn không thể thực hiện được. Chúng ta cần thứ gì đó hoạt động trong thời gian gần như không đổi hoặc tệ nhất là tỷ lệ thuận với số lượng bit. Vì số nguyên 64 bit chỉ có 64 vị trí nên`O(64)`giải pháp có hiệu quả ngay lập tức. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
A = 5
B = 6
```Chúng tôi biết rằng:```
X + Y >= X xor Y
```bởi vì mang làm tăng tổng. Đây`B > A`, nên không có giải pháp nào tồn tại. Việc triển khai bất cẩn chỉ thao tác các bit mà không kiểm tra tính khả thi có thể tạo ra các giá trị trung gian âm không chính xác. 

Một trường hợp tinh tế khác là sự không khớp chẵn lẻ:```
A = 10
B = 3
```Sử dụng danh tính:```
A = (X xor Y) + 2 * (X & Y)
```chúng tôi nhận được:```
A - B = 7
```điều đó thật kỳ quặc. Từ`2 * (X & Y)`phải luôn chẵn, điều này là không thể. Thiếu điều kiện chẵn lẻ này sẽ gây ra câu trả lời sai. 

Ngoài ra còn có điều kiện chồng chéo:```
A = 2
B = 2
```Sau đó:```
(A - B) / 2 = 0
```đó là hợp lệ, đưa ra:```
X = 0
Y = 2
```Nếu không có:```
A = 10
B = 8
```chúng tôi nhận được:```
C = (A - B) / 2 = 1
```Hiện nay`C & B = 0`, do đó công trình xây dựng hoạt động. 

So sánh điều đó với:```
A = 14
B = 10
```Sau đó:```
C = 2
B = 1010
C = 0010
```Chúng chồng lên nhau trên bit 1. Điều đó có nghĩa là một bit sẽ cần đồng thời tạo ra giá trị mang và khác nhau về xor, điều này là không thể. Việc xây dựng lại ngây thơ có thể âm thầm tạo ra các số không hợp lệ ở đây. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là thử mọi giá trị có thể có của`X`, tính:```
Y = A - X
```và kiểm tra xem:```
X xor Y == B
```Điều này đúng vì mọi cặp hợp lệ đều phải thỏa mãn phương trình. Vấn đề là không gian tìm kiếm. Từ`A`có thể ở gần`2^64`, cách tiếp cận bạo lực có thể yêu cầu khoảng`10^19`lần lặp lại. Ngay cả một tỷ phép tính mỗi giây cũng sẽ phải mất hàng thế kỷ. 

Quan sát chính xuất phát từ một danh tính tiêu chuẩn liên quan đến tổng và xor:$X+Y=(X\oplus Y)+2(X\&Y)$Xor đếm các vị trí có bit khác nhau. các`AND`đếm các vị trí có cả hai bit`1`, tạo ra số mang trong quá trình cộng. Mỗi lần mang đóng góp gấp đôi giá trị bit của nó vào tổng cuối cùng. 

Từ`B = X xor Y`, chúng ta có thể viết lại:```
A = B + 2 * (X & Y)
```Cho phép:```
C = X & Y = (A - B) / 2
```Bây giờ vấn đề trở thành: 

- dựng hai số có xor bằng`B`- của ai và là`C`Tại mỗi bit: 

- nếu có một chút trong`B`là`1`, thì các bit của`X`Và`Y`phải khác nhau 
- nếu có một chút trong`C`là`1`, thì cả hai bit phải là`1`Hai điều kiện này không thể xảy ra đồng thời trên cùng một bit. Vì thế:```
B & C == 0
```phải nắm giữ. 

Khi điều kiện này được thỏa mãn, việc tái thiết trở nên dễ dàng: 

- mỗi`1`một chút`C`đi vào cả hai số 
- mỗi`1`một chút`B`phải nhập chính xác một trong các số 

Để giảm thiểu`X`, chúng tôi gán tất cả các bit chỉ xor cho`Y`thay vì`X`. 

Điều đó mang lại:```
X = C
Y = C + B
```miễn là điều kiện chồng chéo là hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^64) | O(1) | Quá chậm | 
| Tối ưu | O(64) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`A`Và`B`. 
2. Kiểm tra xem`A < B`. 

Từ:```
X + Y >= X xor Y
```tổng không bao giờ có thể nhỏ hơn xor. 
3. Tính toán:```
D = A - B
```Nếu như`D`là kỳ lạ, không có giải pháp tồn tại. 

Số lượng:```
D = 2 * (X & Y)
```phải luôn chẵn. 
4. Tính toán:```
C = D // 2
```Điều này thể hiện sự chung`1`bit được chia sẻ bởi cả hai số. 
5. Kiểm tra xem:```
C & B != 0
```Nếu điều này xảy ra, hãy xuất`-1`. 

Một bit không thể đồng thời thuộc về cấu trúc xor và mang. Một bit xor yêu cầu hai số khác nhau, trong khi một bit và yêu cầu cả hai số đó phải khác nhau.`1`. 
6. Xây dựng câu trả lời:```
X = C
Y = C + B
```Tất cả các bit được chia sẻ đều ở bên trong`C`. Tất cả các vết cắn khác nhau từ`B`được giao cho`Y`để giữ`X`càng nhỏ càng tốt. 
7. Đầu ra`X`Và`Y`. 

### Tại sao nó hoạt động 

Việc xây dựng dựa trên sự phân rã chính xác của phép cộng nhị phân thành xor và mang:```
X + Y = (X xor Y) + 2 * (X & Y)
```giá trị`C = (A - B) / 2`xác định duy nhất tất cả các bit tạo ra mang. Nếu bất kỳ bit nào thuộc về cả hai`B`Và`C`, chúng ta sẽ cần cùng một vị trí bit để vừa khác biệt vừa khớp nhau, điều này là không thể. 

Khi`B & C == 0`, vai trò của các bit được tách biệt hoàn toàn: 

- bit của`C`là`1`ở cả hai số 
- bit của`B`là`1`trong đúng một số 

Gán mọi bit chỉ xor cho`Y`giảm thiểu`X`, bởi vì việc thêm bất kỳ bit nào vào`X`sẽ chỉ làm tăng nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

A = int(input())
B = int(input())

if A < B:
    print(-1)
    sys.exit()

D = A - B

if D % 2:
    print(-1)
    sys.exit()

C = D // 2

if C & B:
    print(-1)
    sys.exit()

X = C
Y = C + B

print(X, Y)
```Điều kiện đầu tiên kiểm tra sự bất đẳng thức cơ bản giữa tổng và xor. Bất kỳ cặp hợp lệ nào cũng phải thỏa mãn:```
X + Y >= X xor Y
```bởi vì mang chỉ làm tăng tổng. 

Tiếp theo, mã tính toán`D = A - B`. Giá trị này phải bằng hai lần cấu trúc bit chia sẻ:```
D = 2 * (X & Y)
```Nếu như`D`thật kỳ lạ, việc xây dựng lại là không thể. 

Kiểm tra chồng chéo:```
if C & B:
```là điều kiện đúng cốt lõi. Một chút đặt trong`C`có nghĩa là cả hai số đều chứa`1`ở đó. Một chút đặt trong`B`có nghĩa là các con số khác nhau ở đó. Một vị trí bit không thể đáp ứng đồng thời cả hai yêu cầu. 

Việc xây dựng cuối cùng là bất đối xứng có chủ ý:```
X = C
Y = C + B
```Mỗi bit chỉ xor đi vào`Y`, giữ`X`tối thiểu. Sử dụng phép cộng thay vì bitwise hoặc an toàn vì`C`Và`B`không bao giờ chồng chéo lên nhau. 

Số nguyên Python tự động xử lý các giá trị 64 bit một cách an toàn, do đó không có rủi ro tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
A = 142
B = 76
```Tính toán đầu tiên:```
D = A - B = 66
C = 33
```Các dạng nhị phân:```
B = 1001100
C = 0100001
```Không có bit chồng chéo tồn tại. 

| Biến | Giá trị | 
| --- | --- | 
| A | 142 | 
| B | 76 | 
| D = A - B | 66 | 
| C = D/2 | 33 | 
| C & B | 0 | 
| X | 33 | 
| Y | 109 | 

Xác minh: 

| Biểu hiện | Kết quả | 
| --- | --- | 
| X + Y | 142 | 
| X xor Y | 76 | 

Dấu vết này cho thấy sự tách biệt rõ ràng giữa các bit mang và bit xor. Dạng bit chia sẻ`33`, trong khi các bit chỉ xor được thêm vào`Y`. 

### Ví dụ 2 

đầu vào:```
A = 10
B = 3
```| Biến | Giá trị | 
| --- | --- | 
| A | 10 | 
| B | 3 | 
| D = A - B | 7 | 

Từ`D`thật kỳ quặc, quá trình tái thiết sẽ dừng lại ngay lập tức. 

Điều này thể hiện điều kiện chẵn lẻ. Các bit mang được chia sẻ luôn đóng góp một lượng chẵn vào tổng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(64) | Chỉ một số lượng thao tác bitwise không đổi trên số nguyên 64 bit | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Giải pháp chỉ thực hiện một số phép tính số học và bitwise, không phụ thuộc vào độ lớn của các số. Điều này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    A = int(input())
    B = int(input())

    if A < B:
        print(-1)
        return

    D = A - B

    if D % 2:
        print(-1)
        return

    C = D // 2

    if C & B:
        print(-1)
        return

    print(C, C + B)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("142\n76\n") == "33 109", "sample 1"

# minimum values
assert run("0\n0\n") == "0 0", "both zero"

# impossible because A < B
assert run("5\n6\n") == "-1", "sum smaller than xor"

# impossible because parity fails
assert run("10\n3\n") == "-1", "odd difference"

# valid boundary-style case
assert run("2\n2\n") == "0 2", "xor without shared bits"

# overlap conflict
assert run("14\n10\n") == "-1", "and/xor overlap"

# large values
assert run("18446744073709551614\n0\n") == \
       "9223372036854775807 9223372036854775807", "64-bit range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0 0`| Đầu vào nhỏ nhất có thể | 
|`5 6`|`-1`| Tổng không được nhỏ hơn xor | 
|`10 3`|`-1`| Phát hiện sự khác biệt kỳ lạ | 
|`2 2`|`0 2`| Vỏ xor thuần túy với X tối thiểu | 
|`14 10`|`-1`| Sự chồng chéo giữa các bit xor và bit mang | 
| Vỏ 64-bit lớn | cặp lớn hợp lệ | Xử lý đúng gần giới hạn số nguyên | 

## Vỏ cạnh 

Hãy xem xét:```
A = 5
B = 6
```Thuật toán đầu tiên kiểm tra:```
A < B
```đó là sự thật. Vì xor không bao giờ có thể vượt quá tổng nên thuật toán sẽ in chính xác:```
-1
```mà không cố gắng xây dựng lại. 

Bây giờ hãy xem xét:```
A = 10
B = 3
```Thuật toán tính toán:```
D = 10 - 3 = 7
```Từ`7`thật kỳ lạ, không thể có:```
D = 2 * (X & Y)
```Thuật toán kết thúc ngay lập tức với`-1`. 

Cuối cùng, hãy xem xét xung đột chồng chéo:```
A = 14
B = 10
```Chúng tôi tính toán:```
D = 4
C = 2
```Nhị phân:```
B = 1010
C = 0010
```Hiện nay:```
C & B = 0010
```đó là khác không. 

Bit 1 sẽ cần phải là: 

- khác nhau giữa`X`Và`Y`vì nó thuộc về xor 
- bằng`1`trong cả hai số vì nó thuộc về và 

Sự mâu thuẫn đó làm cho trường hợp này không thể thực hiện được và thuật toán đưa ra kết quả chính xác`-1`.
