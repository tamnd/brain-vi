---
title: "CF 91A - Tiêu Đề Báo"
description: "Chúng ta có chuỗi nguồn s1, đại diện cho tiêu đề của một tờ báo. Chúng tôi có thể lấy bao nhiêu bản sao của dòng tiêu đề này tùy thích và ghép chúng lại với nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 91
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 75 (Div. 1 Only)"
rating: 1500
weight: 91
solve_time_s: 117
verified: true
draft: false
---

[CF 91A - Tiêu đề báo](https://codeforces.com/problemset/problem/91/A) 

**Đánh giá:** 1500 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi nguồn`s1`, đại diện cho tiêu đề của một tờ báo. Chúng tôi có thể lấy bao nhiêu bản sao của dòng tiêu đề này tùy thích và ghép chúng lại với nhau. Từ chuỗi dài thu được, chúng ta được phép xóa các ký tự tùy ý mà vẫn giữ nguyên thứ tự các ký tự còn lại. 

Chuỗi mục tiêu`s2`phải xuất hiện dưới dạng một chuỗi của các tiêu đề được nối. Nhiệm vụ là xác định số bản sao tối thiểu của`s1`cần thiết để xây dựng`s2`. Nếu một số nhân vật của`s2`không bao giờ xuất hiện ở`s1`, nhiệm vụ là không thể. 

Hoạt động này hoàn toàn là về các chuỗi tiếp theo. Chúng tôi không sắp xếp lại các ký tự mà chỉ bỏ qua chúng. Ví dụ, nếu`s1 = "abc"`Và`s2 = "cab"`, một bản sao là không đủ vì`"cab"`không phải là một dãy con của`"abc"`. Hai bản sao hoạt động vì`"cab"`là một dãy con của`"abcabc"`. 

Các ràng buộc định hình mạnh mẽ giải pháp. Chiều dài của`s1`nhiều nhất là`10^4`, đủ nhỏ để xử lý trước. Chiều dài của`s2`có thể đạt được`10^6`, có nghĩa là bất kỳ thuật toán nào thực hiện quét toàn bộ`s1`cho mỗi nhân vật của`s2`sẽ là quá chậm. Một giải pháp kiểu bậc hai như`O(|s1| * |s2|)`có thể yêu cầu xung quanh`10^10`hoạt động trong trường hợp xấu nhất, điều này là không thể trong vòng hai giây. Độ phức tạp của mục tiêu phải gần tuyến tính trong`|s2|`, với chi phí tối đa là logarit. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
s1 = "abc"
s2 = "xyz"
```Câu trả lời đúng là`-1`bởi vì không có phép nối nào có thể tạo ra các ký tự không bao giờ xuất hiện trong`s1`. Việc thực hiện tham lam bất cẩn có thể lặp đi lặp lại để cố gắng khớp`'x'`. 

Một trường hợp phức tạp khác là:```
s1 = "abc"
s2 = "cab"
```Câu trả lời đúng là`2`. Bản sao đầu tiên cung cấp`'c'`, nhưng sau khi đến cuối chuỗi chúng ta phải bắt đầu lại từ đầu để có được`'a'`Và`'b'`. Việc triển khai chỉ kiểm tra xem`s2`là một chuỗi các lần quét lặp lại mà không đếm số lần đặt lại chính xác có thể xuất ra`1`. 

Các ký tự lặp đi lặp lại cũng có vấn đề:```
s1 = "ab"
s2 = "bbbb"
```Câu trả lời là`4`. Mỗi bản sao chỉ đóng góp một`'b'`. Một cách tiếp cận ngây thơ đếm các ký tự hoặc tần số riêng biệt thay vì giữ nguyên trật tự sẽ thất bại ở đây. 

Một điều kiện biên tinh tế xuất hiện khi ký tự hiện tại tồn tại trong`s1`, nhưng không phải sau vị trí hiện tại:```
s1 = "abca"
s2 = "aa"
```Câu trả lời là`1`, bởi vì cả hai`'a'`các ký tự có thể được khớp trong cùng một bản sao. Nếu việc xử lý vị trí hiện tại bị tắt đi một đơn vị, thuật toán có thể bắt đầu một bản sao mới không chính xác sau lần đầu tiên.`'a'`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất mô phỏng quá trình theo đúng nghĩa đen. Chúng tôi tiếp tục quét qua`s1`cố gắng khớp các ký tự của`s2`theo thứ tự. Bất cứ khi nào chúng ta đi đến cuối`s1`, chúng ta bắt đầu một bản sao khác và tiếp tục khớp. 

Phương pháp brute-force này đúng vì nó tuân theo chính xác quá trình xây dựng chuỗi tiếp theo. Đối với mỗi nhân vật trong`s2`, nó tìm kiếm lần xuất hiện có thể sử dụng tiếp theo trong bản sao hiện tại của`s1`. 

Vấn đề là hiệu suất. Giả định`s1`có chiều dài`10^4`Và`s2`có chiều dài`10^6`. Trong trường hợp xấu nhất, với mỗi ký tự của`s2`chúng tôi có thể quét gần như tất cả`s1`. Điều đó tạo ra khoảng`10^10`so sánh nhân vật, vượt xa giới hạn thời gian. 

Quan sát quan trọng là việc quét tuyến tính lặp đi lặp lại rất lãng phí. Mỗi lần chúng tôi hỏi, “sự xuất hiện tiếp theo của nhân vật ở đâu?”`c`sau vị trí`p`?”, chúng tôi tính toán lại câu trả lời từ đầu. Vì`s1`không bao giờ thay đổi, chúng ta nên xử lý trước nó một lần và trả lời các truy vấn này một cách nhanh chóng. 

Một cấu trúc thuận tiện là lưu trữ, đối với mỗi ký tự, danh sách đã sắp xếp các vị trí mà nó xuất hiện trong`s1`. 

Ví dụ:```
s1 = "abac"
```Chúng tôi lưu trữ:```
a -> [0, 2]
b -> [1]
c -> [3]
```Bây giờ giả sử chúng ta hiện đang ở vị trí`1`và cần cái tiếp theo`'a'`. Thay vì quét tuyến tính, chúng tôi tìm kiếm nhị phân bên trong`[0, 2]`để tìm giá trị đầu tiên lớn hơn`1`, đó là`2`. 

Nếu không có vị trí như vậy tồn tại, bản sao hiện tại của`s1`đã cạn kiệt. Chúng tôi bắt đầu một bản sao mới và lấy lần xuất hiện đầu tiên của ký tự đó. 

Điều này biến các lần quét lặp lại tốn kém thành các truy vấn logarit hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | s1 | × | 
| Tối ưu | O( | s1 | + | 

## Hướng dẫn thuật toán 

1. Tiền xử lý`s1`bằng cách lưu trữ tất cả các vị trí xuất hiện cho mỗi ký tự. 

Đối với mỗi chỉ số`i`TRONG`s1`, nối thêm`i`vào danh sách tương ứng`s1[i]`. 
2. Trước khi xử lý`s2`, xác minh rằng mọi ký tự của`s2`xuất hiện ở đâu đó trong`s1`. 

Nếu thiếu một ký tự, hãy in`-1`ngay lập tức vì không có số lượng nối nào có thể tạo ra nó. 
3. Bắt đầu với một bản sao của`s1`. 

Duy trì một biến`pos`đại diện cho vị trí hiện tại bên trong bản sao hoạt động. thiết lập ban đầu`pos = -1`, nghĩa là chúng ta chưa sử dụng bất kỳ ký tự nào. 
4. Ký tự quy trình của`s2`từ trái sang phải. 

Đối với nhân vật hiện tại`c`, truy xuất danh sách lần xuất hiện đã được sắp xếp của nó. 
5. Sử dụng tìm kiếm nhị phân để tìm lần xuất hiện đầu tiên lớn hơn`pos`. 

Điều này mang lại vị trí thứ tự hợp lệ tiếp theo bên trong bản sao hiện tại. 
6. Nếu có vị trí như vậy, hãy di chuyển`pos`tới chỉ số đó. 

Chúng tôi đã khớp nhân vật thành công mà không cần tờ báo nào khác. 
7. Nếu không có trường hợp hợp lệ nào tồn tại, hãy bắt đầu một bản sao mới của`s1`. 

Tăng bộ đếm câu trả lời, sau đó lấy lần xuất hiện đầu tiên của`c`từ danh sách của nó và thiết lập`pos`đến giá trị đó. 
8. Sau khi xử lý tất cả các ký tự, xuất ra số lượng bản sao được sử dụng. 

### Tại sao nó hoạt động 

Thuật toán luôn tiêu thụ nhiều nhất có thể từ bản sao hiện tại trước khi bắt đầu bản sao mới. Sự lựa chọn tham lam này là tối ưu vì việc bắt đầu một bản sao mới sớm hơn không bao giờ tạo ra các cơ hội khớp bổ sung bên trong bản sao hiện tại. 

Ở mỗi bước,`pos`đại diện cho vị trí phù hợp mới nhất trên tờ báo hiện tại. Tìm kiếm nhị phân chọn lần xuất hiện tiếp theo hợp lệ sớm nhất có thể. Việc chọn lần xuất hiện sớm nhất sẽ để lại hậu tố tối đa còn lại cho các ký tự trong tương lai, điều này chỉ có thể hữu ích. 

Bất cứ khi nào không có sự xuất hiện sau đó`pos`, mọi sự tiếp tục tiếp theo bên trong bản sao hiện tại là không thể. Bắt đầu một bản sao mới không chỉ đơn thuần là một sự lựa chọn mà nó là cần thiết. 

Bởi vì thuật toán chỉ bắt đầu một bản sao mới khi bị ép buộc nên tổng số bản sao là tối thiểu. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    s1 = input().strip()
    s2 = input().strip()

    positions = {}

    for i, ch in enumerate(s1):
        if ch not in positions:
            positions[ch] = []
        positions[ch].append(i)

    for ch in s2:
        if ch not in positions:
            print(-1)
            return

    ans = 1
    pos = -1

    for ch in s2:
        arr = positions[ch]

        idx = bisect_right(arr, pos)

        if idx == len(arr):
            ans += 1
            pos = arr[0]
        else:
            pos = arr[idx]

    print(ans)

solve()
```Bước tiền xử lý xây dựng một từ điển từ các ký tự đến các vị trí xuất hiện được sắp xếp. Vì chúng tôi quét`s1`từ trái sang phải, danh sách sẽ được sắp xếp tự động. 

Việc kiểm tra không thể xảy ra trước vòng lặp chính. Điều này tránh các trường hợp góc trong đó quá trình so khớp sẽ liên tục thất bại đối với một ký tự bị thiếu. 

Biến`pos`theo dõi chỉ mục phù hợp mới nhất bên trong bản sao hiện tại của`s1`. sử dụng`-1`ban đầu thì thuận tiện vì lần xuất hiện hợp lệ đầu tiên của bất kỳ ký tự nào sẽ luôn lớn hơn`-1`. 

Chi tiết thực hiện quan trọng là việc sử dụng`bisect_right`. Chúng tôi cần sự xuất hiện đầu tiên ngay sau vị trí hiện tại. sử dụng`bisect_left`sẽ cho phép sử dụng lại cùng một vị trí ký tự nhiều lần một cách không chính xác. 

Khi tìm kiếm nhị phân không tìm được vị trí hợp lệ tiếp theo, chúng ta phải bắt đầu một bản sao mới và sử dụng lần xuất hiện đầu tiên của ký tự. Quên cài đặt lại`pos`chính xác đây là một lỗi phổ biến. 

Thuật toán không bao giờ xây dựng rõ ràng các bản sao lặp đi lặp lại của`s1`. Nó chỉ mô phỏng quá trình chuyển đổi giữa chúng, giúp duy trì mức sử dụng bộ nhớ ở mức nhỏ ngay cả khi câu trả lời lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s1 = "abc"
s2 = "abcbc"
```Danh sách xảy ra:```
a -> [0]
b -> [1]
c -> [2]
```| Ký tự hiện tại | Vị trí hiện tại | Vị trí có thể sử dụng tiếp theo | Cần bản sao mới | Trả lời | 
| --- | --- | --- | --- | --- | 
| một | -1 | 0 | Không | 1 | 
| b | 0 | 1 | Không | 1 | 
| c | 1 | 2 | Không | 1 | 
| b | 2 | không | Có | 2 | 
| c | 1 | 2 | Không | 2 | 

Câu trả lời cuối cùng:```
2
```Dấu vết cho thấy rõ hành vi tham lam. Ba ký tự đầu tiên nằm gọn trong một bản sao. Sau khi khớp`'c'`ở vị trí`2`, không muộn hơn`'b'`tồn tại, vì vậy một bản sao mới trở thành bắt buộc. 

### Ví dụ 2 

đầu vào:```
s1 = "ab"
s2 = "bbbb"
```Danh sách xảy ra:```
b -> [1]
```| Ký tự hiện tại | Vị trí hiện tại | Vị trí có thể sử dụng tiếp theo | Cần bản sao mới | Trả lời | 
| --- | --- | --- | --- | --- | 
| b | -1 | 1 | Không | 1 | 
| b | 1 | không | Có | 2 | 
| b | 1 | không | Có | 3 | 
| b | 1 | không | Có | 4 | 

Câu trả lời cuối cùng:```
4
```Ví dụ này chứng minh rằng thuật toán tôn trọng các ràng buộc về thứ tự hơn là tần số. Mỗi bản sao chỉ đóng góp một bản có thể sử dụng được`'b'`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s1 | 
| Không gian | O( | s1 | 

Với`|s2|`lên tới`10^6`, giải pháp vẫn hiệu quả vì mỗi bước chỉ thực hiện tìm kiếm logarit trên danh sách xuất hiện. Việc sử dụng bộ nhớ rất nhỏ so với giới hạn vì chúng tôi chỉ lưu trữ các chỉ mục từ`s1`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_right

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    s1 = input().strip()
    s2 = input().strip()

    positions = {}

    for i, ch in enumerate(s1):
        if ch not in positions:
            positions[ch] = []
        positions[ch].append(i)

    for ch in s2:
        if ch not in positions:
            return "-1"

    ans = 1
    pos = -1

    for ch in s2:
        arr = positions[ch]

        idx = bisect_right(arr, pos)

        if idx == len(arr):
            ans += 1
            pos = arr[0]
        else:
            pos = arr[idx]

    return str(ans)

# provided sample
assert run("abc\nxyz\n") == "-1", "sample 1"

# minimum size
assert run("a\na\n") == "1", "single character"

# repeated resets
assert run("ab\nbbbb\n") == "4", "multiple newspaper copies"

# subsequence fits in one copy
assert run("abca\naa\n") == "1", "same copy reuse"

# boundary transition
assert run("abc\ncab\n") == "2", "wrap to next copy"

# large repeated pattern
assert run("abc\nabcabcabc\n") == "3", "exact repeated copies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`abc / xyz`|`-1`| Phát hiện ký tự bị thiếu | 
|`a / a`|`1`| Đầu vào hợp lệ tối thiểu | 
|`ab / bbbb`|`4`| Nhiều lần buộc phải đặt lại | 
|`abca / aa`|`1`| Xử lý đúng các ký tự lặp lại trong một bản sao | 
|`abc / cab`|`2`| Chuyển đổi giữa các bản sao | 
|`abc / abcabcabc`|`3`| Hành vi nối chính xác | 

## Vỏ cạnh 

Xét trường hợp không thể xảy ra:```
s1 = "abc"
s2 = "xyz"
```Trong quá trình tiền xử lý, thuật toán chỉ lưu trữ các vị trí cho`'a'`,`'b'`, Và`'c'`. Trong khi kiểm tra`s2`, nhân vật`'x'`bị thiếu trong từ điển nên thuật toán sẽ in ngay lập tức`-1`. Không có mô phỏng xảy ra. 

Bây giờ hãy kiểm tra trường hợp bao quanh:```
s1 = "abc"
s2 = "cab"
```Thuật toán khớp đầu tiên`'c'`ở vị trí`2`. Nhân vật tiếp theo là`'a'`. Tìm kiếm nhị phân bên trong`[0]`cho một giá trị lớn hơn`2`không thành công nên thuật toán bắt đầu một bản sao mới và thiết lập`pos = 0`. Sau đó`'b'`trùng khớp ở vị trí`1`. Câu trả lời cuối cùng trở thành`2`. 

Đối với các ký tự lặp lại:```
s1 = "ab"
s2 = "bbbb"
```Sau khi khớp với cái đầu tiên`'b'`ở vị trí`1`, mỗi lần tìm kiếm sau này`'b'`bên trong cùng một bản sao không thành công vì không có vị trí nào lớn hơn`1`. Thuật toán bắt đầu một bản sao mới một cách chính xác mỗi lần, tạo ra`4`. 

Cuối cùng, hãy xem xét:```
s1 = "abca"
s2 = "aa"
```đầu tiên`'a'`vị trí phù hợp`0`. Đối với thứ hai`'a'`, tìm kiếm nhị phân tìm vị trí`3`, vẫn nằm trong cùng một bản sao. Câu trả lời vẫn còn`1`. Điều này xác nhận thuật toán không được đặt lại sớm khi có một lần xuất hiện hợp lệ khác tồn tại sau đó trong bản sao hiện tại.
