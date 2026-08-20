---
title: "CF 102272B - \u0110\u1ebfm Th\u1ecf"
description: "Chúng ta có một mảng typ[1..N], trong đó mỗi vị trí đại diện cho một con thỏ và giá trị tại vị trí đó xác định loài của nó. Đối với bất kỳ khoảng liền kề nào [l, r], điểm số là số lượng loài khác nhau xuất hiện trong khoảng đó."
date: "2026-08-19T05:27:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102272
codeforces_index: "B"
codeforces_contest_name: "HCW 19 Individual Day 1"
rating: 0
weight: 102272
solve_time_s: 969
verified: false
draft: false
---

[CF 102272B - \u0110\u1ebfm Th\u1ecf](https://codeforces.com/problemset/problem/102272/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 16 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`typ[1..N]`, trong đó mỗi vị trí đại diện cho một con thỏ và giá trị tại vị trí đó xác định loài của nó. Đối với bất kỳ khoảng liền kề nào`[l, r]`, điểm số là số lượng loài khác nhau xuất hiện trong khoảng đó. Chúng ta cần tổng số điểm này trong mọi khoảng thời gian liền kề không trống có thể có. 

Đầu vào chứa tối đa 10 trường hợp thử nghiệm và tổng số phần tử mảng trên tất cả các trường hợp thử nghiệm là tối đa`2 * 10^6`. Một trường hợp thử nghiệm có thể chứa`10^6`các phần tử, do đó`O(N^2)`phương pháp vượt xa thời gian sẵn có. Ngay cả đối với một trường hợp thử nghiệm, có khoảng`N^2 / 2`khoảng thời gian, đó là khoảng`5 * 10^11`khi`N = 10^6`. Một thuật toán về cơ bản phải là tuyến tính, hoặc nhiều nhất là gần tuyến tính, trong`N`. Số nhận dạng loài có thể lớn bằng`10^9`, do đó, việc sử dụng một mảng được mã định danh lập chỉ mục trực tiếp nói chung là không phù hợp. Bản đồ băm là đủ vì chúng ta chỉ cần nhớ vị trí mới nhất của từng loài. 

Câu trả lời cũng có thể lớn hơn nhiều so với số nguyên 32 bit. Nếu mỗi con thỏ có một loài khác nhau thì câu trả lời là tổng độ dài của tất cả các mảng con, đó là`N(N+1)(N+2)/6`. Vì`N = 10^6`, đây là`166667166667000000`, do đó việc triển khai phải sử dụng loại số nguyên có khả năng giữ các giá trị có độ lớn này. Số nguyên Python tự động xử lý việc này. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Hãy xem xét đầu vào nhỏ nhất```
1
1
7
```Chỉ có một khoảng cách,`[1,1]`, và nó chứa một loài, vì vậy câu trả lời là`1`. Một giải pháp khởi tạo lần xuất hiện trước đó của nó thành`1`thay vì`0`hoặc quên bao gồm vị trí hiện tại, có thể tạo ra số 0 không chính xác. 

Các loài lặp đi lặp lại cũng cần được điều trị cẩn thận. Vì```
1
3
1 1 1
```mỗi một trong sáu khoảng chứa chính xác một loài, vì vậy câu trả lời là`6`. Khi thứ ba`1`được xử lý, lần xuất hiện trước đó của nó là vị trí`2`, không phải vị trí`1`. Sử dụng lần xuất hiện đầu tiên thay vì lần xuất hiện gần đây nhất sẽ tính quá nhiều khoảng thời gian. 

Trường hợp ranh giới thứ hai là khi mọi giá trị đều khác nhau:```
1
3
1 2 3
```Mười điểm được đóng góp bởi tất cả các khoảng là`1 + 2 + 2 + 3 = 10`. Mỗi lần xuất hiện đều giới thiệu loài của nó cho toàn bộ phạm vi điểm cuối bên trái có thể có, do đó, việc coi mỗi vị trí chỉ đóng góp một lần sẽ bỏ lỡ nhiều khoảng thời gian. 

Cuối cùng, một loài lặp đi lặp lại không ngừng đóng góp mãi mãi. Vì```
1
4
1 2 1 2
```câu trả lời đúng là`15`. Một phương pháp bất cẩn chỉ đếm các loài mới trên toàn cầu sẽ đếm các loài`1`Và`2`mỗi lần một lần và mất đi sự đóng góp của các loài đó vào các khoảng thời gian bắt đầu sau lần xuất hiện trước đó của chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi khoảng`[l, r]`, duy trì một tập hợp các loài trong khi mở rộng`r`và thêm kích thước đã đặt vào câu trả lời. Điều này đúng vì tập hợp chứa chính xác các loài riêng biệt trong khoảng hiện tại. Tuy nhiên, có`N(N+1)/2`khoảng thời gian và ngay cả khi mỗi phần mở rộng được thực hiện hiệu quả, vẫn có`Theta(N^2)`hoạt động. Tại`N = 10^6`, điều đó có nghĩa là khoảng`5 * 10^11`khoảng cách, điều này hoàn toàn không thể thực hiện được. 

Một cách hữu ích hơn để suy nghĩ về vấn đề là đảo ngược câu hỏi. Thay vì hỏi số lượng loài trong mỗi khoảng, hãy hỏi có bao nhiêu khoảng chứa một sự xuất hiện cụ thể làm sự xuất hiện đại diện cho loài của nó. 

Giả sử vị trí`i`chứa loài`x`. Cho phép`prev[i]`là vị trí trước đó có chứa`x`, hoặc`0`nếu đây là lần đầu tiên xảy ra. Cho phép`next[i]`là vị trí tiếp theo chứa`x`, hoặc`N+1`nếu đây là lần cuối cùng xảy ra. 

Chức vụ`i`đại diện cho loài`x`trong chính xác những khoảng thời gian đó`[l,r]`thỏa mãn`prev[i] < l <= i <= r < next[i]`. 

Điểm cuối bên trái có`i - prev[i]`các lựa chọn và điểm cuối phù hợp có`next[i] - i`sự lựa chọn. Vì vậy sự xuất hiện này góp phần`(i - prev[i]) * (next[i] - i)`đến câu trả lời cuối cùng. 

Có một cách triển khai thậm chí còn đơn giản hơn mà không cần mảng xuất hiện tiếp theo. Xử lý mảng từ trái sang phải. Khi vị trí`i`với loài`x`đã đạt được, hãy`p`là sự xuất hiện trước đó của`x`. 

Với mỗi khoảng kết thúc tại`i`, sự xuất hiện mới tại`i`tăng số lượng khác biệt chính xác khi điểm cuối bên trái lớn hơn`p`. có`i-p`điểm cuối bên trái như vậy. Do đó, tổng số lượng khác biệt trong số tất cả các khoảng kết thúc tại`i`tăng lên bởi`i-p`. 

Chúng ta có thể duy trì`cur`, tổng số loài khác biệt trong tất cả các khoảng kết thúc ở vị trí hiện tại. Sau đó`cur += i - p`và chúng tôi thêm`cur`cho câu trả lời toàn cầu. Điểm quan trọng là các khoảng có điểm cuối bên trái nhiều nhất là`p`loài đã có sẵn`x`trước vị trí`i`, trong khi các khoảng thời gian bắt đầu sau`p`đã không. 

Hai quan điểm đều tương đương nhau. Cái đầu tiên gán cho mỗi loài xuất hiện một hình chữ nhật có điểm cuối bên trái và bên phải hợp lệ. Cái thứ hai quét điểm cuối bên phải và duy trì tổng đóng góp của tất cả các khoảng kết thúc ở đó. Phiên bản quét chỉ yêu cầu sự xuất hiện mới nhất của mỗi loài và đặc biệt nhỏ gọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N^2)`|`O(N)`| Quá chậm | 
| Tối ưu |`O(N)`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bản đồ trống`last`lưu trữ vị trí mới nhất mà mỗi loài xuất hiện. Sử dụng`0`là vị trí trước đó cho một loài chưa từng xuất hiện trước đó. Điều này mang lại cho mỗi lần xuất hiện đầu tiên ranh giới chính xác mà không có trường hợp đặc biệt. 
2. Khởi tạo`cur = 0`Và`answer = 0`. Đây`cur`biểu thị tổng số loài riêng biệt trong tất cả các khoảng có điểm cuối bên phải là vị trí hiện tại.`answer`tích lũy giá trị này trên mọi điểm cuối bên phải. 
3. Quét mảng từ trái sang phải. Tại vị trí`i`, đọc lần xuất hiện trước đó`p = last.get(typ[i], 0)`. 
4. Tăng`cur`qua`i - p`. Xem xét tất cả các khoảng kết thúc tại`i`. Điểm cuối bên trái của họ nằm trong khoảng từ`1`bởi vì`i`. Nếu điểm cuối bên trái nhiều nhất`p`, những loài tương tự đã xuất hiện trong khoảng thời gian trước khi đạt tới`i`, vì vậy sự xuất hiện này không tạo thêm loài mới. Nếu điểm cuối bên trái nằm trong`p+1`bởi vì`i`, đây là lần xuất hiện đầu tiên của loài trong khoảng, vì vậy nó thêm chính xác một loài riêng biệt. có`i-p`điểm cuối bên trái như vậy. 
5. Thêm`cur`ĐẾN`answer`. Mỗi khoảng có chính xác một điểm cuối bên phải, vì vậy sau khi xử lý vị trí`i`, tất cả các khoảng kết thúc tại`i`bây giờ đã đóng góp đúng một lần. 
6. Đặt`last[typ[i]] = i`. Vị trí hiện tại phải trở thành vị trí xuất hiện trước đó đối với tất cả các vị trí trong tương lai có chứa cùng một loài. Việc sử dụng lần xuất hiện cũ hơn ở đây sẽ khiến phạm vi điểm cuối bên trái bị ảnh hưởng trở nên quá lớn. 
7. Sau khi quét xong, hãy in`answer`. Mỗi khoảng không trống đã được xem xét chính xác một lần, được nhóm theo điểm cuối bên phải của nó. 

### Tại sao nó hoạt động 

Vị trí bất biến sau khi xử lý`i`đó có phải là`cur`bằng tổng số loài khác biệt trong mỗi khoảng kết thúc tại`i`. 

Khi loài`x = typ[i]`xảy ra ở vị trí`i`, coi lần xuất hiện trước đó của nó là`p`. Trong một khoảng thời gian`[l,i]`, sự xuất hiện tại`i`giới thiệu một loài mới chính xác khi`l > p`. Có chính xác`i-p`các giá trị có thể có của`l`, do đó thêm`i-p`với tổng trước đó sẽ cho kết quả tổng chính xác cho tất cả các khoảng kết thúc tại`i`. Đang cập nhật`last[x]`ĐẾN`i`bảo toàn thuộc tính tương tự cho lần xuất hiện tiếp theo. Vì câu trả lời chung cộng tổng số chính xác cho mọi điểm cuối bên phải có thể có, nên mỗi khoảng đóng góp chính xác số lượng loài riêng biệt của nó một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        last = {}
        cur = 0
        answer = 0

        for i, x in enumerate(a, 1):
            p = last.get(x, 0)

            cur += i - p
            answer += cur

            last[x] = i

        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Bản đồ`last`tương ứng trực tiếp với giá trị lần xuất hiện trước đó được sử dụng trong phần hướng dẫn. Lần đầu tiên xuất hiện một loài`last.get(x, 0)`trả về 0, vì vậy vị trí`i`đóng góp`i`loài mới xuất hiện giữa các khoảng thời gian kết thúc ở đó. 

Biến`cur`không phải là số lượng loài khác biệt trong một khoảng thời gian cụ thể. Đó là tổng số loài riêng biệt trong tất cả các khoảng kết thúc ở vị trí hiện tại. Sự phân biệt này là cần thiết. Vì`1 2 2`, khi xử lý thứ hai`2`,`cur`trở thành`3`, bởi vì các khoảng kết thúc ở đó là`[2,2]`, chứa một loài, và`[1,2]`, gồm có hai loài. 

Thứ tự thực hiện cũng quan trọng. Chúng tôi đọc vị trí cũ từ`last`trước khi cập nhật nó. Nếu bản đồ được cập nhật trước, mỗi lần xuất hiện lặp lại sẽ có kết quả không chính xác như lần xuất hiện trước đó, khiến đóng góp của nó bằng 0. 

Các số nguyên có độ chính xác tùy ý của Python rất hữu ích ở đây vì câu trả lời có thể đạt tới khoảng`1.67 * 10^17`vì`N = 10^6`. Trong C++, số nguyên 64 bit sẽ được yêu cầu. 

Kích thước đầu vào có thể đạt tới hai triệu số nguyên trong tất cả các trường hợp thử nghiệm, do đó việc triển khai lưu trữ một mảng và một từ điển cho mỗi trường hợp thử nghiệm.`sys.stdin.readline`và một lần ghi đầu ra được lưu vào bộ đệm duy nhất giữ cho chi phí I/O của Python ở mức nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1, test case đầu tiên 

Đối với mảng`1 2 3`, mỗi loài đều mới khi gặp phải. giá trị`i-p`do đó`1`,`2`, Và`3`. 

| Chức vụ`i`| Loài | Trước`p`|`i-p`|`cur`|`answer`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 2 | 3 | 4 | 
| 3 | 3 | 0 | 3 | 6 | 10 | 

Sau vị trí`1`, khoảng duy nhất là`[1,1]`, với một loài Sau vị trí`2`, hai khoảng kết thúc ở đó có điểm`1`Và`2`, cho`cur = 3`. Sau vị trí`3`, ba khoảng kết thúc ở đó có điểm`1`,`2`, Và`3`, cho`cur = 6`. Câu trả lời cuối cùng là`1 + 3 + 6 = 10`. 

### Mẫu 1, test case thứ hai 

cho`1 2 2 3`, vị trí thứ ba lặp lại loài`2`. Sự xuất hiện trước đó của nó là vị trí`2`, vậy chỉ có khoảng`[3,3]`thu được một loài mới từ sự xuất hiện ở vị trí`3`. 

| Chức vụ`i`| Loài | Trước`p`|`i-p`|`cur`|`answer`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 2 | 3 | 4 | 
| 3 | 2 | 2 | 1 | 4 | 8 | 
| 4 | 3 | 0 | 4 | 8 | 16 | 

Tại vị trí`3`, các khoảng kết thúc ở đó là`[3,3]`,`[2,3]`, Và`[1,3]`, điểm của họ là`1`,`1`, Và`2`. Tổng số của họ là`4`, khớp`cur`. Tại vị trí`4`, giống loài`3`là mới trên toàn cầu, vì vậy cả bốn khoảng kết thúc ở đó đều có một loài riêng biệt, ngày càng tăng`cur`từ`4`ĐẾN`8`. Kết quả cuối cùng là`16`. 

### Ví dụ về loài lặp lại 

Hãy xem xét`1 2 1 2`. 

| Chức vụ`i`| Loài | Trước`p`|`i-p`|`cur`|`answer`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 2 | 3 | 4 | 
| 3 | 1 | 1 | 2 | 5 | 9 | 
| 4 | 2 | 2 | 2 | 7 | 16 | 

Tại vị trí`3`, giống loài`1`xuất hiện lần cuối ở vị trí`1`. Điểm cuối bên trái có thể là`2`hoặc`3`, vậy có hai khoảng kết thúc tại`3`nơi sự xuất hiện này giới thiệu các loài`1`. Điều này mang lại sự gia tăng`3-1=2`. Câu trả lời cuối cùng là`16`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N)`| Mỗi phần tử mảng được xử lý một lần và mỗi thao tác bản đồ băm được mong đợi`O(1)`. | 
| Không gian |`O(N)`| Mỗi mảng và bản đồ có thể chứa tối đa`N`các phần tử. | 

Trên tất cả các trường hợp thử nghiệm, tổng số`N`nhiều nhất là`2 * 10^6`, do đó tổng thời gian chạy dự kiến ​​là tuyến tính ở kích thước đầu vào hoàn chỉnh. Việc sử dụng bộ nhớ cũng tuyến tính trong trường hợp thử nghiệm riêng lẻ lớn nhất và duy trì trong giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
# The solution is copied into solve() so the tests can replace stdin.

import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        last = {}
        cur = 0
        answer = 0

        for i, x in enumerate(a, 1):
            p = last.get(x, 0)
            cur += i - p
            answer += cur
            last[x] = i

        out.append(str(answer))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """2
3
1 2 3
4
1 2 2 3
"""
) == "10\n16", "provided sample"

# Minimum size
assert run(
    """1
1
7
"""
) == "1", "single rabbit"

# All values equal
assert run(
    """1
4
5 5 5 5
"""
) == "10", "all intervals contain exactly one species"

# Alternating repeated values
assert run(
    """1
4
1 2 1 2
"""
) == "16", "repeated species with gaps"

# Every value is different
assert run(
    """1
4
1 2 3 4
"""
) == "20", "all species are different"

# Maximum-size test, all values equal.
# The answer is the number of nonempty subarrays.
n = 1_000_000
inp = "1\n{}\n{}\n".format(n, "1 " * (n - 1) + "1")
expected = n * (n + 1) // 2
assert run(inp) == str(expected), "maximum N with all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 7`|`1`| Kích thước tối thiểu và ranh giới lần xuất hiện đầu tiên | 
|`1 / 4 / 5 5 5 5`|`10`| Giá trị lặp lại và xử lý lần xuất hiện mới nhất | 
|`1 / 4 / 1 2 1 2`|`16`| Sự lặp lại được phân tách bởi các loài khác | 
|`1 / 4 / 1 2 3 4`|`20`| Mỗi lần xuất hiện đều mới trên toàn cầu | 
|`N = 10^6`, tất cả các giá trị`1`|`500000500000`| Kích thước đầu vào tối đa và câu trả lời lớn | 

## Vỏ cạnh 

Đối với một con thỏ, đầu vào là```
1
1
7
```Tại vị trí`1`, không có sự xuất hiện trước đó, vì vậy`p = 0`. Mức tăng là`1 - 0 = 1`, cho`cur = 1`Và`answer = 1`. Có chính xác một khoảng có thể, vì vậy kết quả là chính xác. 

Đối với tất cả các loài bình đẳng,```
1
3
1 1 1
```việc thực hiện là 

| Vị trí | Lần xuất hiện trước | Tăng |`cur`|`answer`| 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 3 | 
| 3 | 2 | 1 | 3 | 6 | 

Mỗi khoảng chứa chính xác một loài và có sáu khoảng. Ranh giới chính là mỗi lần xuất hiện lặp lại sử dụng vị trí ngay trước đó làm`p`, do đó, nó cộng chính xác một vào tổng số cho các khoảng thời gian bắt đầu từ lần xuất hiện đó. 

Đối với một mảng mà mỗi loài đều khác nhau,```
1
3
1 2 3
```tất cả các vị trí trước đó đều bằng không. Số gia tăng là`1`,`2`, Và`3`, sản xuất`cur`giá trị`1`,`3`, Và`6`. Câu trả lời toàn cầu là`10`. Điều này xác minh rằng thuật toán tính mức đóng góp trên tất cả các điểm cuối bên trái có thể có thay vì coi một lần xuất hiện chỉ đóng góp vào một khoảng thời gian. 

Đối với các loài lặp lại được phân tách bằng các giá trị khác,```
1
4
1 2 1 2
```vị trí thứ ba đã xuất hiện trước đó`1`, vậy mức tăng của nó là`3-1=2`. Vị trí thứ tư đã xuất hiện trước đó`2`, vậy mức tăng của nó là`4-2=2`. Kết quả`cur`giá trị là`1`,`3`,`5`, Và`7`, và câu trả lời là`16`. Điều này mắc phải lỗi phổ biến là lưu trữ lần xuất hiện đầu tiên thay vì lần xuất hiện mới nhất. 

Trường hợp có giá trị lớn cũng có vấn đề. Vì`N = 10^6`với tất cả các giá trị khác nhau, câu trả lời là`N(N+1)(N+2)/6 = 166667166667000000`. 

Số nguyên 32 bit sẽ bị tràn nặng, trong khi biểu diễn số nguyên của Python lưu kết quả chính xác. Bản thân thuật toán không cần bất kỳ xử lý đặc biệt nào cho trường hợp này vì giống nhau`i-p`công thức áp dụng với`p = 0`ở mọi vị trí.
