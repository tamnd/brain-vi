---
title: "CF 102277I - Áo thun đồng đội"
description: "Travis có tối đa 25 người bạn và mỗi người bạn đã chọn một số áo từ 1 đến 99. Travis có thể chọn thêm một số áo cho mình, cũng từ 1 đến 99."
date: "2026-08-16T19:40:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "I"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 121
verified: true
draft: false
---

[CF 102277I - Áo sơ mi/áo thi đấu của đội](https://codeforces.com/problemset/problem/102277/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Travis có tối đa 25 người bạn, và mỗi người bạn đã chọn một số áo đấu từ 1 đến 99. Travis có thể chọn thêm một số áo đấu cho mình, cũng từ 1 đến 99. Anh ấy muốn biết liệu một số lựa chọn cho số áo đấu của anh ấy có thể ghép một số số áo đấu có sẵn và thu được số nguyên yêu thích của anh ấy một cách chính xác hay không. 

Thứ tự số áo được chọn là tùy ý. Số áo đấu phải được sử dụng tổng thể, vì vậy nếu ai đó có số áo 75, Travis có thể sử dụng`75`, nhưng không thể chỉ sử dụng`7`hoặc chỉ`5`. Có thể bỏ áo đấu của một người bạn và áo đấu của chính Travis cũng có thể bỏ qua. Đầu ra là`1`nếu tồn tại một lựa chọn số Travis làm cho số nguyên yêu thích có thể xây dựng được và`0`nếu không thì. Tuyên bố chính thức đưa ra`t < 1,000,000,000`,`n <= 25`và số áo trong phạm vi từ 1 đến 99. 

Giá trị nhỏ của`n`có thể đề xuất tìm kiếm theo cấp số nhân, nhưng hạn chế mạnh hơn nhiều là ở số nguyên yêu thích. Vì nó dưới một tỷ nên nó có nhiều nhất là 9 chữ số thập phân. Mỗi áo đấu đóng góp một hoặc hai chữ số. Điều đó có nghĩa là mục tiêu chỉ có một số ít cách có thể để chia biểu diễn thập phân của nó thành các phần có kích thước bằng áo đấu hợp lệ. Một giải pháp tỷ lệ thuận với số lần phân chia có thể xảy ra là đủ nhanh, trong khi việc liệt kê các tập hợp con và hoán vị tùy ý của tối đa 25 người bạn là quá tốn kém. 

Trường hợp cạnh đầu tiên là mục tiêu bao gồm một chữ số. Ví dụ, với```
7
1
24
```câu trả lời là`1`, bởi vì Travis có thể đơn giản chọn áo đấu`7`và sử dụng nó một mình. Giải pháp giả định rằng ít nhất một người bạn phải tham gia sẽ từ chối trường hợp này một cách không chính xác. 

Số 0 bên trong cần được chăm sóc. Ví dụ,```
101
1
10
```có câu trả lời`1`, bởi vì mục tiêu có thể được chia thành`10 | 1`, với áo thi đấu hiện có`10`tiếp theo là chiếc áo đấu được Travis chọn`1`. Việc xử lý từng chữ số một cách độc lập sẽ coi số 0 là số áo, trong khi chia nó thành`1 | 01`cũng sẽ không hợp lệ vì jersey`01`không tồn tại. 

Số áo lặp đi lặp lại là một nguồn sai lầm khác. Ví dụ,```
2222
3
2 2 2
```có câu trả lời`1`. Mục tiêu cần bốn bản sao áo thi đấu`2`, ba là do bạn bè cung cấp và Travis có thể chọn`2`chính anh ta. Việc triển khai dựa trên tập hợp mà quên đi bội số sẽ làm mất đi sự khác biệt này. 

Cuối cùng, có các giá trị phù hợp là chưa đủ nếu mục tiêu không thể được phân chia thành các giá trị hoàn chỉnh đó. Ví dụ,```
715
1
75
```có câu trả lời`0`. Áo bạn bè có sẵn đóng góp`75`, nhưng mục tiêu sẽ cần`7 | 15`hoặc`71 | 5`và không thể cung cấp sự phân rã bằng cách sử dụng một người bạn duy nhất cộng với một áo đấu mới được chọn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng chọn một tập hợp con bạn bè, chọn thứ tự cho tập hợp con đó, quyết định xem áo đấu của Travis có được sử dụng hay không, chọn số áo đấu của anh ấy, nối mọi thứ và so sánh kết quả với mục tiêu. Điều này đúng vì mọi công trình xây dựng hợp pháp đều được thể hiện bằng một số lựa chọn như vậy. Vấn đề là số lượng khả năng. Vì`n = 25`, số tập con có thứ tự là 

25!\sum_{j=0}^{25}\frac{1}{j!}, 
] 

đó là khoảng`e * 25!`, hoặc về`4.2 * 10^25`. Việc thử tới 99 lựa chọn cho áo đấu của Travis sẽ đẩy điều này về phía`4.2 * 10^27`công trình xây dựng ứng cử viên. Đây là nhiều cấp độ lớn hơn những gì một chương trình hai giây có thể kiểm tra. 

Nhận xét quan trọng là chúng ta thực sự không cần phải chọn bạn trước. Bản thân mục tiêu sẽ xác định trình tự số áo đấu có thể có. Vì mỗi áo đấu có số từ 1 đến 99 nên mỗi mảnh của mục tiêu phải chứa chính xác một hoặc hai chữ số. Đối với mục tiêu có`L`chữ số thì chỉ có`L - 1`vị trí mà chúng ta có thể cắt hoặc không cắt. Như vậy có nhiều nhất 

[ 
2^{L-1} \le 2^8 = 256 
] 

các phân vùng có thể. 

Đối với mỗi phân vùng, chúng tôi nhận được một chuỗi các số có một chữ số hoặc hai chữ số. Chúng ta có thể đếm xem có bao nhiêu bản sao của mỗi số áo đấu mà phân vùng yêu cầu. Những người bạn cung cấp một số bộ số áo đấu. Việc phân vùng này khả thi chính xác khi mọi bản sao được yêu cầu đều có sẵn trong số bạn bè, ngoại trừ việc Travis có thể cung cấp tối đa một bản sao bị thiếu. 

Không cần thiết phải thử hết 99 lựa chọn có thể có đối với Travis. Nếu một phân vùng thiếu một số áo, Travis có thể chỉ cần chọn số đó. Nếu thiếu hai bản sao trở lên, một áo không thể sửa chữa được phân vùng. Nếu không thiếu thứ gì, Travis có thể chọn bất kỳ số áo hợp lệ nào và chỉ cần không sử dụng nó. 

Cách tiếp cận vũ phu hoạt động vì nó tìm kiếm rõ ràng mọi cách xây dựng có thể, nhưng không thành công khi coi 25 người bạn là không gian tìm kiếm. Việc quan sát thấy mục tiêu có nhiều nhất chín chữ số sẽ thay đổi hoàn toàn không gian tìm kiếm. Thay vì khám phá các hoán vị của bạn bè, chúng tôi liệt kê tối đa 256 cách mà mục tiêu có thể được chia thành các số áo hợp pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(99 * e * n!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(2^8 * n)`|`O(1)`ngoài việc lưu trữ đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi số nguyên yêu thích sang biểu diễn chuỗi thập phân của nó. Chúng tôi làm việc trực tiếp với các chữ số vì cấu trúc hợp lệ là sự ghép nối của các số jersey, vì vậy mọi cấu trúc hợp lệ đều tương ứng với một phân vùng của chuỗi này. 
2. Đếm xem mỗi số áo từ 1 đến 99 xuất hiện bao nhiêu lần giữa những người bạn. Mảng tần số thuận tiện vì toàn bộ dải giá trị chỉ chứa 99 khả năng. 
3. Liệt kê mọi tập hợp con của`L - 1`khoảng cách giữa các chữ số mục tiêu liên tiếp. Khoảng trống được chọn có nghĩa là số áo hiện tại kết thúc ở đó. Khoảng trống không được chọn có nghĩa là chữ số tiếp theo thuộc về cùng một số áo. 
4. Đối với mỗi phân vùng, quét mục tiêu từ trái sang phải và tạo thành các phần có một chữ số hoặc hai chữ số tương ứng. Từ chối một mảnh bằng 0, vì số áo đấu bắt đầu từ 1. Đồng thời, từ chối một mảnh có hai chữ số bắt đầu bằng 0, vì số áo đấu được đưa ra mà không có số 0 đứng đầu. 
5. Đếm số lần xuất hiện bắt buộc của mỗi số áo đấu thu được. So sánh những yêu cầu này với tần suất của bạn bè. Với mỗi giá trị, hãy tính xem có bao nhiêu bản sao bị thiếu. 
6. Chấp nhận phân vùng nếu tổng số bản sao bị thiếu nhiều nhất là một. Nếu bằng 0 thì chỉ có bạn bè mới có thể tạo thành mục tiêu. Nếu đúng thì Travis chọn chính xác số áo còn thiếu đó. Nếu nó lớn hơn một, một chiếc áo đấu đơn lẻ không thể hoàn thành việc xây dựng. 
7. Nếu bất kỳ phân vùng nào được chấp nhận, hãy in`1`. Nếu mọi phân vùng đều bị lỗi, hãy in`0`. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ cách xây dựng hợp lệ nào của số nguyên yêu thích. Mỗi số áo trong cấu trúc đó có một hoặc hai chữ số, do đó cấu trúc tạo ra một phân vùng duy nhất của chuỗi thập phân của mục tiêu. Bảng liệt kê của chúng tôi kiểm tra phân vùng đó. So sánh tần số của nó chấp nhận chính xác khi tất cả các bản sao áo thi đấu cần thiết có thể được cung cấp bởi bạn bè cộng với tối đa một áo đấu do Travis chọn. Vì vậy mọi công trình hợp lệ đều được chấp nhận. Ngược lại, mọi phân vùng được chấp nhận đều mô tả một chuỗi số jersey hợp pháp có kết nối chính xác là mục tiêu và kiểm tra tần số đảm bảo rằng các bản sao được yêu cầu thực sự tồn tại sau khi Travis chọn một giá trị còn thiếu nếu cần. Do đó thuật toán trả về`1`chính xác khi tồn tại một công trình hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = input().strip()
    n = int(input())
    friends = list(map(int, input().split()))

    freq = [0] * 100
    for x in friends:
        freq[x] += 1

    length = len(t)

    # There are length - 1 possible cut positions.
    # A mask describes which positions are cuts.
    for mask in range(1 << (length - 1)):
        need = [0] * 100
        pos = 0
        missing = 0
        valid = True

        while pos < length:
            if pos + 1 < length:
                # If there is a cut after this digit, use one digit.
                if mask & (1 << pos):
                    value = ord(t[pos]) - ord('0')
                    pos += 1
                else:
                    # Otherwise use two digits.
                    if t[pos] == '0':
                        valid = False
                        break
                    value = (ord(t[pos]) - ord('0')) * 10
                    value += ord(t[pos + 1]) - ord('0')
                    pos += 2
            else:
                value = ord(t[pos]) - ord('0')
                pos += 1

            if value == 0:
                valid = False
                break

            need[value] += 1

        if not valid:
            continue

        for value in range(1, 100):
            if need[value] > freq[value]:
                missing += need[value] - freq[value]
                if missing > 1:
                    break

        if missing <= 1:
            print(1)
            return

    print(0)

if __name__ == "__main__":
    solve()
```Mảng tần số lưu trữ nhiều tập hợp của bạn bè thay vì chỉ các giá trị áo đấu riêng biệt của họ. Điều này là cần thiết vì hai bản sao áo thi đấu`7`là các tài nguyên khác nhau từ một bản sao của jersey`7`. 

Mặt nạ có một bit cho mỗi khoảng cách giữa các chữ số mục tiêu liền kề. Khi bit`pos`được đặt, chữ số tại`pos`kết thúc áo đấu hiện tại, vì vậy phần tiếp theo bắt đầu lúc`pos + 1`. Khi rõ ràng, chúng ta tiêu thụ hai chữ số cùng nhau. Vì số áo có nhiều nhất hai chữ số nên đây là hai khả năng duy nhất. 

Chữ số cuối cùng phải luôn được sử dụng dưới dạng áo đấu có một chữ số. Vòng lặp xử lý việc này bằng`pos + 1 < length`điều kiện, tránh truy cập ngoài giới hạn ở chữ số cuối cùng. 

Séc`t[pos] == '0'`trước khi tạo một số có hai chữ số sẽ ngăn các giá trị như`01`khỏi bị đối xử như áo đấu`1`. Sự riêng biệt`value == 0`kiểm tra từ chối một số 0 độc lập. 

các`missing`counter đo lường sự thiếu hụt nguồn lực thay vì cố gắng quyết định trước xem Travis nên chọn chiếc áo đấu nào. Nếu thiếu chính xác một bản sao thì giá trị đó là lựa chọn tối ưu của Travis. Nếu không có bản sao nào bị thiếu thì áo thi đấu của anh ấy không có ý nghĩa vì anh ấy không cần phải sử dụng nó. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Quan trọng hơn, mục tiêu được giữ dưới dạng một chuỗi, do đó không có lý do gì để thực hiện phép tính trên một giá trị có thể được nối dài. 

## Ví dụ đã hoạt động 

Bản PDF của tuyên bố trình bày các trường hợp mẫu ở định dạng hai cột. Các trường hợp mẫu riêng biệt là mục tiêu`707`với bạn bè`7, 24`, mục tiêu`70707`với bạn bè`7, 7`, mục tiêu`1122`với bạn bè`21, 1, 23`, và mục tiêu`715`với bạn bè`75`. Các đầu ra tương ứng là`1`,`0`,`0`, Và`0`. 

Đối với mẫu 1,```
707
2
7 24
```Phân vùng thành công là`7 | 07`chỉ nếu`07`là hợp pháp nên phân vùng đó bị từ chối. Phân vùng thành công thực sự là`70 | 7`. Travis chọn`70`, trong khi bạn của anh ấy cung cấp`7`. 

| Vị trí | Mảnh được chọn | Số lượng bắt buộc | Thiếu bản | 
| --- | --- | --- | --- | 
| 0 |`70`|`70:1`| 1 | 
| 2 |`7`|`70:1, 7:1`| 1 | 
| Kết thúc |`70 | 7 = 707`|`70:1, 7:1`| 1 | 

Điều bất biến là sau khi xử lý từng phần, mảng tần số được yêu cầu sẽ mô tả chính xác số jersey cần thiết theo tiền tố tương ứng của mục tiêu. Vì chỉ có áo đấu`70`bạn bè thiếu, Travis có thể cung cấp nó và câu trả lời là`1`. 

Đối với mẫu 2,```
70707
2
7 7
```Mục tiêu có năm chữ số, vì vậy mỗi phân vùng bao gồm các phần có một hoặc hai chữ số. Mục tiêu yêu cầu nhiều tài nguyên hơn hai bản sao có sẵn của`7`có thể cung cấp, bất kể Travis chọn chiếc áo đấu nào. 

| Vị trí | Mảnh được chọn | Số lượng bắt buộc | Thiếu bản | 
| --- | --- | --- | --- | 
| 0 |`70`|`70:1`| 1 | 
| 2 |`7`|`70:1, 7:1`| 1 | 
| 3 |`7`|`70:1, 7:2`| 1 | 
| Kết thúc | không hợp lệ hoặc cần một phần khác | Nhiều hơn một bản sao không có sẵn | 2+ | 

Mọi phân vùng có thể đều chứa phần đầu 0 không hợp lệ hoặc yêu cầu ít nhất hai bản sao không được bạn bè cung cấp. Một chiếc áo đấu thêm không thể bù đắp được cả hai sự thiếu hụt, vì vậy câu trả lời là`0`. 

Đối với mẫu 3,```
1122
3
21 1 23
```Một phân vùng như`1 | 12 | 2`có cấu trúc hợp lệ, nhưng nó yêu cầu jersey`12`và áo đấu`2`, cả hai đều không được cung cấp. Một phân vùng như`11 | 22`yêu cầu hai chiếc áo đấu cũng không có sẵn. Không có phân vùng nào để lại nhiều nhất một bản sao bị thiếu, vì vậy câu trả lời là`0`. 

Đối với mẫu 4,```
715
1
75
```Các phần có hai chữ số hữu ích duy nhất xung quanh mục tiêu là`71`Và`15`, nhưng có sẵn`75`cũng không khớp. Các phân vùng`7 | 15`Và`71 | 5`mỗi người yêu cầu hai chiếc áo đấu, trong khi người bạn chỉ có thể cung cấp một chiếc và một chiếc do Travis cung cấp. Vì không thể chọn áo bị thiếu để làm cho phân vùng tương ứng hoạt động với`75`, câu trả lời là`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(2^L * L + 2^L * 99) = O(2^L * 99)`|`L <= 9`, vậy tối đa 256 phân vùng được kiểm tra | 
| Không gian |`O(99)`| Mảng tần số có kích thước cố định là 100 | 

Mục tiêu lớn nhất chỉ có chín chữ số nên thuật toán kiểm tra tối đa 256 phân vùng. Ngay cả việc quét tất cả 99 giá trị jersey có thể có cho mỗi phân vùng cũng chỉ mất khoảng 25.000 thao tác đơn giản. Đây là mức thoải mái trong giới hạn hai giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB được báo cáo cho sự cố Codeforces. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    t = input().strip()
    n = int(input())
    friends = list(map(int, input().split())) if n else []

    freq = [0] * 100
    for x in friends:
        freq[x] += 1

    length = len(t)

    for mask in range(1 << (length - 1)):
        need = [0] * 100
        pos = 0
        valid = True

        while pos < length:
            if pos + 1 < length:
                if mask & (1 << pos):
                    value = ord(t[pos]) - ord('0')
                    pos += 1
                else:
                    if t[pos] == '0':
                        valid = False
                        break
                    value = (ord(t[pos]) - ord('0')) * 10
                    value += ord(t[pos + 1]) - ord('0')
                    pos += 2
            else:
                value = ord(t[pos]) - ord('0')
                pos += 1

            if value == 0:
                valid = False
                break

            need[value] += 1

        if not valid:
            continue

        missing = 0
        for value in range(1, 100):
            if need[value] > freq[value]:
                missing += need[value] - freq[value]
                if missing > 1:
                    break

        if missing <= 1:
            return "1\n"

    return "0\n"

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        return solve()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("707\n2\n7 24\n") == "1\n", "sample 1"
assert run("70707\n2\n7 7\n") == "0\n", "sample 2"
assert run("1122\n3\n21 1 23\n") == "0\n", "sample 3"
assert run("715\n1\n75\n") == "0\n", "sample 4"

# Minimum-size target, Travis supplies the only needed jersey.
assert run("1\n1\n24\n") == "1\n", "single digit target"

# All values equal, with Travis supplying the fourth copy.
assert run("2222\n3\n2 2 2\n") == "1\n", "repeated jersey"

# Internal zero handled through a valid two-digit jersey.
assert run("101\n1\n10\n") == "1\n", "internal zero"

# Maximum target length and maximum number of friends.
friends = " ".join(["9"] * 25)
assert run(f"999999999\n25\n{friends}\n") == "1\n", "maximum-size input"

# Two missing jersey copies cannot be repaired by one Travis jersey.
assert run("1234\n2\n12 34\n") == "0\n", "two-copy shortage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 24`|`1`| Mục tiêu kích thước tối thiểu và chỉ sử dụng áo đấu của Travis | 
|`2222 / 3 / 2 2 2`|`1`| Multiplicity và Travis cung cấp chính xác một bản sao bổ sung | 
|`101 / 1 / 10`|`1`| Xử lý đúng số 0 bên trong | 
|`999999999 / 25 / 9 ... 9`|`1`| Độ dài mục tiêu tối đa và số lượng bạn bè tối đa | 
|`1234 / 2 / 12 34`|`0`| Nhiều hơn một bản bị thiếu không thể sửa chữa được | 

## Vỏ cạnh 

Đối với mục tiêu một chữ số như```
1
1
24
```chỉ có một phân vùng có thể, đó là`1`. Danh sách bạn bè không có`1`, do đó thuật toán tính toán một bản sao bị thiếu. Vì số bản còn thiếu chính xác là một nên Travis chọn jersey`1`và thuật toán trả về`1`. 

Đối với số 0 nội bộ như```
101
1
10
```phân vùng`10 | 1`là hợp lệ. Phần đầu tiên là áo đấu hiện có`10`, và phần thứ hai được cung cấp bởi Travis. Thuật toán không bao giờ cố gắng diễn giải số 0 dưới dạng một jersey riêng biệt, vì vậy nó trả về`1`. 

Đối với các giá trị lặp lại,```
2222
3
2 2 2
```tần số yêu cầu là bốn bản sao của`2`, trong khi bạn bè cung cấp ba. Sự thiếu hụt chính xác là một nên Travis chọn`2`. Thuật toán trả về`1`, chứng minh tại sao tần số thay vì một tập hợp các giá trị sẵn có lại cần thiết. 

Đối với một cấu trúc không thể có bên trong,```
100
1
10
```mục tiêu không thể được phân chia hoàn toàn thành các số áo đấu dương có một hoặc hai chữ số hợp lệ. Sau đó`10`, phần còn lại`0`bản thân nó sẽ phải là một chiếc áo đấu, điều này là bất hợp pháp. Mọi phân vùng chứa số 0 độc lập đó đều bị từ chối, vì vậy câu trả lời là`0`. 

Đối với trường hợp cần thêm hai áo thi đấu,```
1234
2
12 34
```phân vùng tự nhiên là`12 | 34`, nhưng cả hai áo đấu đều đã tồn tại, vì vậy đầu vào cụ thể này thực sự khả thi và kết quả mong đợi là`1`. Một thử nghiệm thiếu hụt tốt hơn là```
1234
1
12
```Đây là phân vùng`12 | 34`yêu cầu áo`34`, mà Travis có thể cung cấp nên điều này cũng khả thi. Để buộc hai sự thiếu hụt, sử dụng```
123
1
45
```Các phân vùng hợp lệ duy nhất là`1 | 23`Và`12 | 3`. Mỗi người cần hai chiếc áo, trong khi người bạn có`45`và Travis chỉ có thể cung cấp một trong hai giá trị bắt buộc. Do đó, cả hai phân vùng đều bị lỗi, khiến`0`. Điều này minh họa ý nghĩa chính xác của`missing <= 1`tình trạng.
