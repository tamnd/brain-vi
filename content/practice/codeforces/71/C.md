---
title: "CF 71C - Hiệp Sĩ Bàn Tròn"
description: "Các hiệp sĩ ngồi trên một vòng tròn, cách đều nhau. Mỗi vị trí được đánh dấu 1 cho hiệp sĩ đang có tâm trạng tốt hoặc 0 cho hiệp sĩ đang có tâm trạng tồi tệ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 71
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 65 (Div. 2)"
rating: 1600
weight: 71
solve_time_s: 100
verified: true
draft: false
---

[CF 71C - Hiệp sĩ bàn tròn](https://codeforces.com/problemset/problem/71/C) 

**Đánh giá:** 1600 
**Tags:** dp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các hiệp sĩ ngồi trên một vòng tròn, cách đều nhau. Mỗi vị trí đều được đánh dấu`1`cho một hiệp sĩ đang có tâm trạng tốt hoặc`0`cho một hiệp sĩ đang có tâm trạng tồi tệ. Chúng ta cần xác định xem có thể chọn một số hiệp sĩ tâm trạng tốt để chúng tạo thành các đỉnh của một đa giác đều có ít nhất ba đỉnh hay không. 

Bởi vì tất cả các hiệp sĩ đều cách đều nhau xung quanh vòng tròn, nên mọi đa giác đều tương ứng với việc chiếm các vị trí có kích thước bước cố định. Ví dụ, nếu`n = 12`và chúng ta chọn mỗi hiệp sĩ thứ ba, chúng ta sẽ có được các đỉnh của hình vuông. Nếu chúng ta chọn mỗi hiệp sĩ thứ hai, chúng ta sẽ có được một hình lục giác. 

Nhiệm vụ trở thành: có tồn tại một số kích thước bước sao cho việc nhảy liên tục theo bước đó chỉ truy cập các vị trí có chứa`1`và chu trình chứa ít nhất ba vị trí riêng biệt? 

Ràng buộc`n ≤ 10^5`ngay lập tức loại trừ bất cứ điều gì gần với thời gian khối. Thậm chí đầy đủ`O(n^2)`giải pháp trong Python có nhiều rủi ro đối với Codeforces, đặc biệt với giới hạn thời gian rất nhỏ. Chúng ta cần một cái gì đó gần tuyến tính hoặc`n log n`. 

Một điểm tinh tế là không phải mọi tập hợp con của các hiệp sĩ giỏi đều tạo thành một đa giác hợp lệ. Các đỉnh phải cách đều nhau xung quanh đường tròn. Một giải pháp bất cẩn chỉ có thể đếm được bao nhiêu`1`s tồn tại và kết luận rằng ít nhất ba hiệp sĩ giỏi là đủ. 

Coi như:```
6
1 1 1 0 0 0
```Câu trả lời đúng là`NO`. 

Tuy có ba hiệp sĩ giỏi nhưng lại xếp liền nhau, không cách đều nhau. Không có đa giác đều có thể được hình thành. 

Một trường hợp phức tạp khác xuất hiện khi đa giác sử dụng tất cả các quân mã.```
5
1 1 1 1 1
```Câu trả lời là`YES`. 

Toàn bộ hình ngũ giác tự nó là một đa giác đều. Bất kỳ việc triển khai nào chỉ kiểm tra các ước số thích hợp của`n`sẽ bỏ lỡ điều này một cách không chính xác. 

Một trường hợp khác liên quan đến các chu kỳ quá nhỏ.```
4
1 0 1 0
```Câu trả lời là`NO`. 

Việc chọn mỗi hiệp sĩ thứ hai chỉ tạo ra hai đỉnh, đây không phải là đa giác hợp lệ vì đa giác phải chứa ít nhất ba điểm. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu tuân theo hình học trực tiếp. Đối với mọi kích thước đa giác có thể`k`, chúng ta thử mọi vị trí bắt đầu có thể và kiểm tra xem tất cả các đỉnh của đa giác đó có chứa`1`. 

Giả sử đa giác có`k`đỉnh. Khi đó bước giữa các đỉnh liên tiếp là`n / k`, Vì thế`k`phải chia`n`. Với mỗi số chia`k ≥ 3`, chúng ta có thể kiểm tra tất cả`n`vị trí bắt đầu và đi qua toàn bộ chu trình. 

Điều này có tác dụng vì mọi đa giác đều trên các điểm cách đều nhau đều tương ứng chính xác với một trong các chu trình này. 

Vấn đề là thời gian chạy. Trong trường hợp xấu nhất, chúng ta có thể kiểm tra nhiều ước số và với mỗi ước số chúng ta có thể quét hầu hết các vị trí. Việc triển khai ngây thơ có thể hướng tới`O(n^2)`, quá chậm đối với`n = 10^5`. 

Quan sát chính là chúng ta không thực sự quan tâm trực tiếp đến kích thước đa giác. Điều quan trọng là khoảng cách nhảy. 

Nếu chúng ta nhảy một bước nào đó`d`, các vị trí đã ghé thăm tạo thành một chu trình có độ dài là:$$\frac{n}{\gcd(n,d)}$$Ít nhất chúng ta chỉ quan tâm đến chu kỳ có độ dài`3`. Mỗi đa giác đều hợp lệ tương ứng với một chu trình như vậy. 

Bây giờ hãy xem xét một số chia`k`của`n`, Ở đâu`k ≥ 3`. Các đỉnh đa giác chính xác là:$$i,\ i + \frac{n}{k},\ i + 2\frac{n}{k},\dots$$Vì vậy thay vì kiểm tra các tập con tùy ý, chúng ta chỉ cần kiểm tra các ước của`n`. 

Với mỗi số chia`k ≥ 3`, cho phép:$$step = \frac{n}{k}$$Sau đó, chúng tôi kiểm tra mọi modulo lớp dư lượng`step`. Nếu mọi phần tử trong một lớp dư lượng là`1`, chúng tôi đã tìm thấy một đa giác hợp lệ. 

Mỗi vị trí chỉ tham gia vào một số lần kiểm tra số chia có giới hạn, do đó độ phức tạp tổng thể vẫn hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số quân mã và mảng tâm trạng. 
2. Liệt kê tất cả các ước số`k`của`n`như vậy`k ≥ 3`. 

Một đa giác đều với`k`đỉnh chỉ có thể nếu`k`chia rẽ`n`, vì các đỉnh phải cách đều nhau xung quanh đường tròn. 
3. Đối với mỗi hợp lệ`k`, tính:```
step = n // k
```Nhảy qua`step`di chuyển từ đỉnh đa giác này sang đỉnh đa giác tiếp theo. 
4. Hãy thử mọi khoảng cách bắt đầu có thể từ`0`ĐẾN`step - 1`. 

Mỗi offset đại diện cho một chu kỳ của các vị trí cách đều nhau. 
5. For a fixed offset, walk through the cycle:```
offset, offset + step, offset + 2*step, ...
```modulo`n`. 
6. Kiểm tra xem mọi vị trí trong chu trình đó có chứa`1`. 

Nếu thậm chí một chu kỳ bao gồm toàn bộ`1`s, in`YES`ngay lập tức. 
7. Nếu tất cả các cấu hình số chia đều thất bại, hãy in`NO`. 

### Tại sao nó hoạt động 

Một đa giác đều trên các điểm hình tròn cách đều nhau phải sử dụng các đỉnh cách nhau một khoảng góc không đổi. Vì các quân mã cách đều nhau nên điều này chuyển trực tiếp thành một bước nhảy chỉ số cố định. 

Nếu một đa giác có`k`đỉnh thì các đỉnh liên tiếp cách nhau một cách chính xác`n / k`các vị trí. Thuật toán kiểm tra mọi ước số có thể`k`và mọi vị trí bắt đầu có thể có cho khoảng cách đó. 

Mọi đa giác hợp lệ xuất hiện chính xác trong một trong các lần kiểm tra này và mọi chu kỳ được kiểm tra đều tương ứng với một đa giác đều hợp lệ. Bởi vì thuật toán chỉ chấp nhận khi tất cả các đỉnh trong một chu trình chứa`1`, nó không thể tạo ra kết quả dương tính giả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    divisors = []

    d = 1
    while d * d <= n:
        if n % d == 0:
            if d >= 3:
                divisors.append(d)

            other = n // d
            if other != d and other >= 3:
                divisors.append(other)

        d += 1

    for k in divisors:
        step = n // k

        for start in range(step):
            ok = True

            for pos in range(start, n, step):
                if a[pos] == 0:
                    ok = False
                    break

            if ok:
                print("YES")
                return

    print("NO")

solve()
```Phần đầu tiên tính toán tất cả các ước của`n`có thể đại diện cho kích thước đa giác. Ta bỏ qua các ước số nhỏ hơn`3`vì đa giác có ít hơn ba đỉnh là không hợp lệ. 

Với mỗi số chia`k`, đoạn mã rút ra khoảng cách giữa các đỉnh đa giác bằng cách sử dụng`step = n // k`. Đây là sự dịch chuyển hình học quan trọng từ cấu trúc đa giác sang các chỉ số mảng. 

Vòng lặp lồng nhau`start`xử lý các góc quay khác nhau của cùng một mẫu đa giác. Ví dụ, với`step = 3`, trình tự bắt đầu từ`0`,`1`, Và`2`là những chu kỳ riêng biệt 

Vòng lặp trong cùng kiểm tra một chu trình hoàn chỉnh. sử dụng`range(start, n, step)`là đủ vì các vị trí bao quanh cấu trúc vòng tròn một cách tự nhiên thông qua khoảng cách mô-đun. Mọi vị trí trong lớp dư lượng đó thuộc về cùng một đa giác. 

Việc trở lại sớm rất quan trọng đối với hiệu suất. Khi chúng tôi tìm thấy một đa giác hợp lệ thì không cần phải làm gì thêm nữa. 

Một lỗi triển khai phổ biến là kiểm tra các ước của`step`thay vì ước của`n`. Một lỗi dễ mắc phải khác là quên rằng đa giác sử dụng tất cả`n`đỉnh là hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 1
```| k | bước | bắt đầu | Đã kiểm tra vị trí | Tất cả những cái | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 0 | 0,1,2 | Có | 

Thuật toán ngay lập tức phát hiện ra rằng cả ba vị trí đều tạo thành một tam giác đều hợp lệ. Vì mọi hiệp sĩ đều đang có tâm trạng vui vẻ nên câu trả lời là`YES`. 

### Ví dụ 2 

đầu vào:```
6
1 0 1 0 1 0
```| k | bước | bắt đầu | Đã kiểm tra vị trí | Tất cả những cái | 
| --- | --- | --- | --- | --- | 
| 3 | 2 | 0 | 0,2,4 | Có | 

các vị trí`0`,`2`, Và`4`cách đều nhau và tất cả đều chứa`1`. Các đỉnh này tạo thành một tam giác đều trên đường tròn. 

Dấu vết này thể hiện tính bất biến cốt lõi: các vị trí cách nhau bằng một bước không đổi tương ứng chính xác với một đa giác đều. 

### Ví dụ 3 

đầu vào:```
6
1 1 1 0 0 0
```| k | bước | bắt đầu | Đã kiểm tra vị trí | Tất cả những cái | 
| --- | --- | --- | --- | --- | 
| 3 | 2 | 0 | 0,2,4 | Không | 
| 3 | 2 | 1 | 1,3,5 | Không | 
| 6 | 1 | 0 | 0,1,2,3,4,5 | Không | 

Không có chu trình cách đều nhau chỉ chứa`1`s, vậy câu trả lời là`NO`. 

Trường hợp này cho thấy tại sao chỉ đếm những hiệp sĩ giỏi là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi lần kiểm tra số chia sẽ quét các cấp số học có tổng kích thước vẫn có thể quản lý được | 
| Không gian | O(1) | Chỉ có một vài biến phụ bên cạnh mảng đầu vào | 

Giải pháp thoải mái phù hợp với các hạn chế. Ngay cả đối với`n = 10^5`, số lượng ước số nhỏ và mỗi vị trí mảng chỉ được xem lại qua các lần quét dựa trên ước số. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        divisors = []

        d = 1
        while d * d <= n:
            if n % d == 0:
                if d >= 3:
                    divisors.append(d)

                other = n // d
                if other != d and other >= 3:
                    divisors.append(other)

            d += 1

        for k in divisors:
            step = n // k

            for start in range(step):
                ok = True

                for pos in range(start, n, step):
                    if a[pos] == 0:
                        ok = False
                        break

                if ok:
                    return "YES"

        return "NO"

    return solve()

# provided sample
assert run("3\n1 1 1\n") == "YES", "sample 1"

# minimum valid polygon
assert run("3\n1 0 1\n") == "NO", "minimum size failure"

# equally spaced triangle
assert run("6\n1 0 1 0 1 0\n") == "YES", "alternating pattern"

# consecutive ones are not enough
assert run("6\n1 1 1 0 0 0\n") == "NO", "non-regular subset"

# all knights good
assert run("5\n1 1 1 1 1\n") == "YES", "whole polygon"

# only two opposite points
assert run("4\n1 0 1 0\n") == "NO", "degenerate polygon"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 0 1`|`NO`| Xử lý kích thước tối thiểu | 
|`6 / 1 0 1 0 1 0`|`YES`| Phát hiện chu kỳ chính xác | 
|`6 / 1 1 1 0 0 0`|`NO`| Từ chối các tập con liên tiếp nhưng không đều đặn | 
|`5 / 1 1 1 1 1`|`YES`| Đa giác toàn vòng tròn | 
|`4 / 1 0 1 0`|`NO`| Bác bỏ trường hợp suy biến 2 điểm | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
6
1 1 1 0 0 0
```Thuật toán kiểm tra số chia`k = 3`, cho`step = 2`. 

Các chu kỳ là:```
0,2,4
1,3,5
```Chu kỳ đầu tiên chứa một`0`ở vị trí`4`. Chu kỳ thứ hai chứa`0`s tại các vị trí`3`Và`5`. Không có đa giác hợp lệ tồn tại. 

Trường hợp này xác minh rằng thuật toán yêu cầu khoảng cách bằng nhau chứ không chỉ đơn thuần là đủ hiệp sĩ giỏi. 

Bây giờ hãy xem xét:```
5
1 1 1 1 1
```Số chia`k = 5`sản xuất`step = 1`. 

Chu kỳ duy nhất là:```
0,1,2,3,4
```Mỗi vị trí đều chứa`1`, do đó thuật toán chấp nhận chính xác hình ngũ giác đầy đủ. 

Cuối cùng, kiểm tra:```
4
1 0 1 0
```Việc thực hiện bất cẩn có thể chấp nhận vì vị trí`0`Và`2`có khoảng cách bằng nhau. Thuật toán bác bỏ điều này vì các ước số duy nhất được kiểm tra là`k ≥ 3`. 

Vì`n = 4`, số chia hợp lệ chỉ là`4`, sản xuất`step = 1`. Vì không phải tất cả các vị trí đều`1`, câu trả lời trở thành`NO`.
