---
title: "CF 1582B - Luntik và các chuỗi tiếp theo"
description: "Chúng tôi được cung cấp một số mảng. Với mỗi mảng, gọi tổng của tất cả các phần tử là $s$. Chúng ta cần đếm xem có bao nhiêu dãy con có tổng chính xác là $s-1$. Một dãy con được hình thành bằng cách xóa bất kỳ số phần tử nào, có thể không có phần tử nào hoặc tất cả chúng."
date: "2026-06-10T09:58:23+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1582
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 750 (Div. 2)"
rating: 900
weight: 1582
solve_time_s: 94
verified: true
draft: false
---

[CF 1582B - Luntik và các chuỗi tiếp theo](https://codeforces.com/problemset/problem/1582/B) 

**Xếp hạng:** 900 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số mảng. Với mỗi mảng, gọi tổng của tất cả các phần tử là$s$. Chúng ta cần đếm chính xác có bao nhiêu dãy con có tổng$s-1$. 

Một dãy con được hình thành bằng cách xóa bất kỳ số phần tử nào, có thể không có phần tử nào hoặc tất cả chúng. Thứ tự các phần tử còn lại không thay đổi. 

Độ dài mảng tối đa là 60, đủ nhỏ để quét trực tiếp mảng, nhưng số lượng chuỗi con là$2^{60}$, đó là rất lớn. Việc liệt kê tất cả các dãy con là hoàn toàn không thể. Thậm chí$2^{40}$các hoạt động sẽ là quá nhiều cho giới hạn một giây. 

Bản thân các giá trị có thể lớn bằng$10^9$, nhưng điều đó không quan trọng lắm vì chúng ta không bao giờ cần quy hoạch động theo tổng. Cấu trúc của bài toán hóa ra chỉ phụ thuộc vào số lượng số 0 và số 1 xuất hiện. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như```
1
2
1 0
```Tổng tổng là 1. Chúng ta cần các dãy con có tổng bằng 0. Cả dãy con trống và dãy con chỉ chứa 0 đều thỏa mãn điều này, nên đáp án là 2. Một nghiệm quên dãy con trống sẽ trả về 1. 

Hãy xem xét```
1
2
1000 1000
```Tổng số là 2000. Để có được 1999, chúng ta phải loại bỏ các phần tử có tổng bằng 1, nhưng không có phần tử hoặc tổ hợp nào như vậy. Câu trả lời là 0. Một cách tiếp cận bất cẩn có thể cho rằng mọi mảng đều có ít nhất một dãy con hợp lệ. 

Một trường hợp thú vị khác là```
1
4
1 1 0 0
```Tổng tổng là 2. Một dãy con gần đầy đủ phải có tổng bằng 1. Chúng ta có thể loại bỏ chính xác một trong các số 1 và mỗi tập hợp số 0 còn lại có thể được chọn độc lập. Vì có hai số không, điều đó góp phần$2^2=4$sự lựa chọn. Có 2 cách chọn cách nào bỏ đi nên đáp án là$2 \times 4 = 8$. 

## Phương pháp tiếp cận 

Một giải pháp brute-force kiểm tra mọi dãy con, tính tổng của nó và đếm xem có bao nhiêu bằng nhau$s-1$. Điều này có tác dụng vì định nghĩa hỏi trực tiếp về các dãy con. Vấn đề là số lượng các chuỗi con. Với$n=60$, có$$2^{60}\approx 1.15\times 10^{18}$$khả năng, vượt xa bất cứ điều gì thực tế. 

Quan sát quan trọng đến từ việc xem xét phần bổ sung. Một dãy con có tổng$s-1$có nghĩa là các phần tử bị loại bỏ có tổng bằng 1. 

Vì tất cả các số đều không âm nên cách duy nhất để các phần tử bị loại bỏ có tổng bằng 1 là loại bỏ chính xác một phần tử bằng 1 và không loại bỏ số dương nào lớn hơn 1. 

Các số 0 không ảnh hưởng đến tổng nên mỗi số 0 có thể giữ nguyên hoặc bị xóa một cách độc lập. Nếu có$z$số không và$o$những cái đó, chúng tôi có$o$lựa chọn cái nào được loại bỏ và$2^z$cách để quyết định số 0 nào còn lại. 

Câu trả lời đơn giản là$$o \times 2^z$$Quét mảng một lần sẽ cho cả hai số đếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc một test case. 
2. Đếm xem có bao nhiêu phần tử bằng 0. Gọi số này là$z$. 

Các số 0 rất đặc biệt vì việc bao gồm hoặc loại trừ chúng không bao giờ làm thay đổi tổng. 
3. Đếm xem có bao nhiêu phần tử bằng 1. Gọi số này là$o$. 

Một dãy con hợp lệ phải loại trừ chính xác một phần tử có giá trị là 1. 
4. Tính đáp án dưới dạng$$o \times 2^z$$có$o$các lựa chọn cho cái bị loại bỏ và mọi số 0 độc lập đều có hai khả năng. 
5. Xuất kết quả. 

### Tại sao nó hoạt động 

Giả sử dãy con có tổng$s-1$. Tương tự, các phần tử bị loại bỏ có tổng bằng 1. 

Bởi vì mọi phần tử mảng đều không âm nên không thể xóa phần tử nào lớn hơn 1. Loại bỏ hai hoặc nhiều phần tử sẽ đóng góp ít nhất 2. Do đó, phải loại bỏ chính xác một phần tử bằng 1. 

Các số 0 không đóng góp gì vào số tiền bị loại bỏ, vì vậy chúng có thể bị loại bỏ hoặc giữ lại mà không ảnh hưởng đến tính hợp lệ. 

Mỗi dãy con hợp lệ tương ứng với một lựa chọn của một lựa chọn bị loại bỏ và một lựa chọn độc lập cho mỗi số 0. Ngược lại, mọi sự kết hợp như vậy đều tạo ra một dãy con hợp lệ. Vì sự tương ứng này là một-một nên số đếm chính xác là$o \times 2^z$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    zeros = 0
    ones = 0

    for x in a:
        if x == 0:
            zeros += 1
        elif x == 1:
            ones += 1

    print(ones * (1 << zeros))
```Chương trình xử lý từng trường hợp thử nghiệm một cách độc lập. 

Vòng lặp trên mảng đếm số không và số một. Các giá trị lớn hơn 1 bị bỏ qua vì chúng không bao giờ có thể thuộc về tập hợp bị loại bỏ có tổng phải bằng 1. 

biểu hiện`1 << zeros`tính toán$2^{\text{zeros}}$. Với tối đa 60 phần tử, giá trị lớn nhất là$2^{60}$, dễ dàng khớp với các số nguyên có độ chính xác tùy ý của Python. 

Thứ tự nhân không quan trọng. Nếu không có ai, câu trả lời sẽ tự động trở thành số 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| Bước | số không | những cái | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | - | 
| Đọc 1 | 0 | 1 | - | 
| Đọc 2 | 0 | 1 | - | 
| Đọc 3 | 0 | 1 | - | 
| Đọc 4 | 0 | 1 | - | 
| Đọc 5 | 0 | 1 | - | 
| Cuối cùng | 0 | 1 | 1 | 

Chỉ tồn tại một phần tử bằng 1 và không có số 0. Chúng ta phải loại bỏ phần tử đó để tạo ra chính xác một dãy con hợp lệ. 

### Ví dụ 2 

đầu vào:```
2
1 0
```| Bước | số không | những cái | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | - | 
| Đọc 1 | 0 | 1 | - | 
| Đọc 0 | 1 | 1 | - | 
| Cuối cùng | 1 | 1 | 2 | 

Cái duy nhất phải được loại bỏ. Số 0 có thể ở lại hoặc bị loại bỏ, đưa ra hai khả năng. Một tương ứng với dãy sau`(0)`và cái còn lại tương ứng với dãy con trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần vượt qua mảng | 
| Không gian |$O(1)$| Chỉ có hai quầy được lưu trữ | 

Từ$n\le 60$, công việc cho mỗi trường hợp thử nghiệm là rất nhỏ. Ngay cả với 1000 trường hợp thử nghiệm, chương trình chỉ thực hiện khoảng 60 nghìn lượt kiểm tra phần tử, con số này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        zeros = 0
        ones = 0

        for x in a:
            if x == 0:
                zeros += 1
            elif x == 1:
                ones += 1

        ans.append(str(ones * (1 << zeros)))

    return "\n".join(ans)

# provided samples
assert run(
"""5
5
1 2 3 4 5
2
1000 1000
2
1 0
5
3 0 2 1 1
5
2 1 0 3 0
"""
) == """1
0
2
4
4"""

# minimum size
assert run(
"""1
1
0
"""
) == "0"

# single one
assert run(
"""1
1
1
"""
) == "1"

# all zeros
assert run(
"""1
4
0 0 0 0
"""
) == "0"

# multiple ones and zeros
assert run(
"""1
4
1 1 0 0
"""
) == "8"

# maximum-style case with many zeros
assert run(
"""1
5
1 0 0 0 0
"""
) == "16"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| 0 | Không có ai tồn tại, câu trả lời phải bằng 0 | 
|`1`| 1 | Mảng phần tử đơn | 
|`0 0 0 0`| 0 | Tất cả số không | 
|`1 1 0 0`| 8 | Nhiều cái có thể tháo rời và các lựa chọn số 0 độc lập | 
|`1 0 0 0 0`| 16 | Tính toán đúng$2^z$| 

## Vỏ cạnh 

Hãy xem xét```
1
2
1 0
```Chúng tôi có`ones = 1`Và`zeros = 1`. Thuật toán trả về$$1 \times 2^1 = 2$$Hai dãy con hợp lệ là`(0)`và dãy con trống. Vì các số 0 có thể được loại bỏ một cách tự do nên thuật toán sẽ tính cả hai một cách tự nhiên. 

Coi như```
1
2
1000 1000
```Không có ai và không có số không. Thuật toán tính toán$$0 \times 2^0 = 0$$Không có tập hợp các phần tử bị loại bỏ nào có thể có tổng bằng 1, vì vậy câu trả lời chính xác là bằng 0. 

Coi như```
1
4
1 1 0 0
```Bộ đếm trở thành`ones = 2`Và`zeros = 2`. Câu trả lời là$$2 \times 2^2 = 8$$Chính xác một trong hai cái phải được loại bỏ. Mỗi số 0 có thể tồn tại hoặc biến mất một cách độc lập. Thuật toán đếm tất cả tám khả năng mà không liệt kê các chuỗi con.
