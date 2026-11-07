---
title: "CF 133B - Đơn nguyên"
description: "Chúng ta được cung cấp một chương trình Brainfuck ngắn bao gồm các ký tự như +, -, , <, v.v. Mỗi lệnh tương ứng với một chuỗi nhị phân 4 bit cố định. Sau khi thay thế mọi ký tự bằng mã nhị phân của nó, chúng tôi ghép tất cả các đoạn 4 bit đó thành một số nhị phân dài."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 133
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 96 (Div. 2)"
rating: 1200
weight: 133
solve_time_s: 90
verified: true
draft: false
---

[CF 133B - Đơn nguyên](https://codeforces.com/problemset/problem/133/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chương trình Brainfuck ngắn bao gồm các nhân vật như`+`,`-`,`>`,`<`, vân vân. Mỗi lệnh tương ứng với một chuỗi nhị phân 4 bit cố định. Sau khi thay thế mọi ký tự bằng mã nhị phân của nó, chúng tôi ghép tất cả các đoạn 4 bit đó thành một số nhị phân dài. 

Nhiệm vụ là diễn giải chuỗi nhị phân đó dưới dạng số nguyên cơ số 2 và in giá trị của nó theo modulo`1000003`. 

Lúc đầu, cách diễn đạt về biểu diễn đơn nguyên có thể khiến bạn mất tập trung. Kích thước của cách biểu diễn một số của một số chỉ là chính số đó, vì một số viết một số`n`sử dụng`n`bản sao của`1`. Vì vậy, chúng ta thực sự không cần phải xây dựng bất kỳ chuỗi đơn nhất nào. Chúng ta chỉ cần giá trị thập phân của số nhị phân. 

Độ dài đầu vào tối đa là 100 ký tự. Mỗi ký tự đóng góp chính xác 4 chữ số nhị phân, do đó số nhị phân cuối cùng có tối đa 400 bit. Các ngôn ngữ như Python về mặt kỹ thuật có thể lưu trữ trực tiếp các số nguyên như vậy, nhưng vấn đề yêu cầu rõ ràng về kết quả modulo`1000003`, vì vậy số học mô-đun là cách tiếp cận rõ ràng nhất. 

Một lỗi triển khai ngây thơ là cố gắng xây dựng chuỗi nhị phân khổng lồ và chuyển đổi nó sau đó. Điều đó vẫn có tác dụng với những ràng buộc này, nhưng nó bỏ qua quan sát cốt lõi rằng con số có thể được xây dựng tăng dần giống hệt như chuyển đổi cơ sở. 

Một lỗi phổ biến khác là hiểu sai "kích thước của chương trình đơn nhất" nghĩa là gì. Hãy xem xét đầu vào:```
,.
```Ánh xạ trở thành:```
, -> 1101
. -> 1100
```Số nhị phân nối là:```
11011100
```Điều đó bằng`220`ở dạng thập phân nên đáp án là`220`, không phải độ dài của chuỗi nhị phân và không phải số chữ số đơn phân được viết rõ ràng. 

Trường hợp cạnh tinh vi hơn xuất hiện khi biểu diễn nhị phân bắt đầu bằng`1`và chứa nhiều số không sau đó. Ví dụ:```
>
```bản đồ tới:```
1000
```cái nào bằng`8`. Việc triển khai bất cẩn làm loại bỏ các số 0 ở đầu không chính xác hoặc xử lý các phần một cách độc lập có thể tạo ra giá trị sai. 

Một lỗi dễ xảy ra khác là do quên giảm bớt mô-đun trong quá trình xây dựng. Số nguyên 400 bit vẫn phù hợp với Python, nhưng trong các ngôn ngữ có số nguyên có chiều rộng cố định, số nguyên này sẽ tràn nhanh chóng. Sử dụng modulo sau mỗi bước sẽ tránh được vấn đề đó một cách rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo tuyên bố theo nghĩa đen. Chúng tôi tạo một từ điển từ các lệnh Brainfuck thành chuỗi 4 bit, nối tất cả các đoạn nhị phân thành một chuỗi nhị phân lớn, sau đó chuyển chuỗi nhị phân đó thành số nguyên thập phân. 

Đối với độ dài đầu vào tối đa là 100, độ dài chuỗi nhị phân chỉ là 400, do đó, ngay cả chuyển đổi trực tiếp cũng đủ nhanh. Độ phức tạp là tuyến tính theo số lượng bit được tạo ra. 

Cách tiếp cận thú vị hơn tránh việc lưu trữ toàn bộ số nhị phân. Giả sử chúng ta đã xử lý một số tiền tố và giá trị số của nó là`x`. Khi chúng ta nối thêm một khối 4 bit khác`b`, giá trị mới trở thành:```
x * 16 + value(b)
```bởi vì việc thêm 4 chữ số nhị phân cũng giống như dịch chuyển sang trái 4 bit, nhân với`2^4 = 16`. 

Điều này biến vấn đề thành việc xây dựng số vị trí tiêu chuẩn. Chúng tôi quét chương trình từ trái sang phải, nhân liên tục câu trả lời hiện tại với 16 và cộng giá trị số của lệnh tiếp theo. 

Vì câu trả lời cuối cùng là cần thiết theo modulo`1000003`, chúng ta có thể lấy modulo sau mỗi thao tác:```
ans = (ans * 16 + value) % MOD
```Điều này giữ cho số lượng nhỏ trong suốt quá trình tính toán. 

Brute-force hoạt động vì tổng đầu vào rất nhỏ nhưng phương pháp tăng dần sạch hơn, sử dụng bộ nhớ bổ sung không đổi và phản ánh trực tiếp cách hình thành các số nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ánh xạ từ mỗi lệnh Brainfuck tới giá trị thập phân của nó sau khi chuyển đổi nhị phân. 

Ví dụ,`">"`ánh xạ sang nhị phân`1000`, bằng số thập phân`8`. Tương tự,`"["`bản đồ tới`1110`, bằng`14`. 
2. Khởi tạo`ans = 0`. 

Biến này lưu trữ giá trị của số nhị phân được hình thành cho đến nay. 
3. Xử lý chuỗi đầu vào từ trái sang phải. 

Mỗi lệnh đóng góp chính xác 4 chữ số nhị phân. 
4. Đối với mỗi nhân vật`c`, cập nhật:```
ans = (ans * 16 + value[c]) % 1000003
```Nhân với 16 sẽ dịch chuyển số nhị phân hiện tại sang 4 bit. Thêm`value[c]`nối thêm khối 4 bit mới. 
5. Sau khi xử lý xong tất cả ký tự, in`ans`. 

### Tại sao nó hoạt động 

Sau khi xử lý lần đầu`k`lệnh,`ans`bằng giá trị thập phân của phép nối các mã nhị phân 4 bit tương ứng của chúng, lấy modulo`1000003`. 

Ban đầu, trước khi xử lý bất cứ thứ gì, chuỗi nhị phân trống và giá trị của nó là`0`, do đó bất biến đúng. 

Khi một khối 4 bit mới được thêm vào, số nhị phân hiện có sẽ dịch chuyển sang trái 4 vị trí, nhân với 16. Việc cộng giá trị của khối mới sẽ tạo ra chính xác giá trị số của chuỗi nhị phân mở rộng. Vì số học mô-đun bảo toàn phép cộng và phép nhân, việc lấy modulo sau mỗi lần cập nhật sẽ giữ cho bất biến luôn đúng. 

Cuối cùng, tất cả các lệnh đã được xử lý, vì vậy`ans`chính xác là kết quả được yêu cầu modulo`1000003`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000003

values = {
    '>': 8,   # 1000
    '<': 9,   # 1001
    '+': 10,  # 1010
    '-': 11,  # 1011
    '.': 12,  # 1100
    ',': 13,  # 1101
    '[': 14,  # 1110
    ']': 15   # 1111
}

def solve():
    s = input().strip()

    ans = 0

    for ch in s:
        ans = (ans * 16 + values[ch]) % MOD

    print(ans)

solve()
```Từ điển lưu trữ giá trị thập phân của mỗi mã 4 bit thay vì chính chuỗi nhị phân. Điều này tránh việc phân tích cú pháp nhị phân lặp đi lặp lại. 

Bản cập nhật cốt lõi:```
ans = (ans * 16 + values[ch]) % MOD
```khớp chính xác với cấu trúc toán học. Nhân với 16 sẽ dịch số nhị phân hiện có thành 4 bit và việc cộng giá trị mới sẽ nối thêm khối lệnh tiếp theo. 

Áp dụng modulo trong mỗi lần lặp sẽ giữ cho các giá trị trung gian ở mức nhỏ. Dù sao thì Python cũng có thể xử lý toàn bộ số nguyên, nhưng mẫu này an toàn hơn và phù hợp trực tiếp với yêu cầu của bài toán. 

sử dụng`strip()`là quan trọng bởi vì`input()`bao gồm ký tự dòng mới ở cuối. Nếu không loại bỏ nó, việc tra cứu từ điển sẽ không thành công`'\n'`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
,.
```Ánh xạ:```
, -> 1101 -> 13
. -> 1100 -> 12
```| Bước | Nhân vật | Giá trị | Trước và | Mới và | 
| --- | --- | --- | --- | --- | 
| 1 |`,`| 13 | 0 | 13 | 
| 2 |`.`| 12 | 13 | 220 | 

Câu trả lời cuối cùng là`220`. 

Dấu vết này cho thấy việc thêm khối 4 bit tương ứng với việc nhân với 16 như thế nào. Sau khi xử lý`,`, chúng ta có hệ nhị phân`1101`. Đang bổ sung`1100`sản xuất`11011100`, bằng`13 * 16 + 12 = 220`. 

### Ví dụ 2 

đầu vào:```
[]
```Ánh xạ:```
[ -> 1110 -> 14
] -> 1111 -> 15
```| Bước | Nhân vật | Giá trị | Trước và | Mới và | 
| --- | --- | --- | --- | --- | 
| 1 |`[`| 14 | 0 | 14 | 
| 2 |`]`| 15 | 14 | 239 | 

Số nhị phân cuối cùng là:```
11101111
```cái nào bằng`239`. 

Ví dụ này chứng minh rằng thuật toán hoạt động thống nhất cho mọi lệnh. Ý nghĩa của các hoạt động Brainfuck không bao giờ quan trọng, chỉ có mã nhị phân được gán cho chúng thôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(1) | Chỉ một số biến và một từ điển có kích thước cố định được lưu trữ | 

Với tối đa 100 ký tự, thời gian chạy rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1000003

values = {
    '>': 8,
    '<': 9,
    '+': 10,
    '-': 11,
    '.': 12,
    ',': 13,
    '[': 14,
    ']': 15
}

def solve():
    s = input().strip()

    ans = 0

    for ch in s:
        ans = (ans * 16 + values[ch]) % MOD

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run(",.\n") == "220", "sample 1"

# minimum size input
assert run(">\n") == "8", "single character"

# another single character
assert run("]\n") == "15", "maximum 4-bit value"

# two-character construction
assert run("[]\n") == "239", "binary concatenation"

# long repeated input
expected = 0
for _ in range(100):
    expected = (expected * 16 + 10) % MOD
assert run("+" * 100 + "\n") == str(expected), "maximum length"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`,.`|`220`| Mẫu được cung cấp | 
|`>`|`8`| Đầu vào kích thước tối thiểu | 
|`]`|`15`| Khối đơn lớn nhất | 
|`[]`|`239`| Nối nhị phân đúng | 
|`++++...`(100 lần) | Kết quả modulo tính toán | Độ dài đầu vào tối đa và cập nhật mô-đun | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
>
```Bản đồ là:```
1000
```cái nào bằng`8`. 

Thuật toán bắt đầu với`ans = 0`và thực hiện:```
ans = 0 * 16 + 8 = 8
```Đầu ra cuối cùng là chính xác. 

Bây giờ hãy xem xét trường hợp có giá trị 4 bit lớn nhất có thể:```
]
```Mã nhị phân của nó là:```
1111
```cái nào bằng`15`. 

Thuật toán tính toán:```
ans = 0 * 16 + 15 = 15
```Không cần xử lý đặc biệt đối với các giá trị ở gần đầu trên của ánh xạ. 

Cuối cùng, hãy xem xét một đầu vào dài chẳng hạn như 100 bản sao của`+`. Mỗi`+`đóng góp nhị phân`1010`, hoặc số thập phân`10`. Số trung gian tăng lên cực kỳ nhanh chóng. Giải pháp lưu trữ mọi thứ bằng số nguyên có chiều rộng cố định sẽ bị tràn. Việc triển khai của chúng tôi áp dụng modulo sau mỗi lần cập nhật:```
ans = (ans * 16 + 10) % 1000003
```vì vậy giá trị luôn ở mức dưới đây`1000003`, bất kể độ dài đầu vào.
