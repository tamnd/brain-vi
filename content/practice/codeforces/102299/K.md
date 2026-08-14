---
title: "CF 102299K - Người Nghèo"
description: "Chúng tôi có một loạt các giá chương tích cực. Bất cứ khi nào một khoản nợ đến hạn, Dostoyevskiy có thể bán bất kỳ tập hợp con nào của các chương đã viết sẵn và khoản nợ có thể được thanh toán chính xác khi giá trị của nó bằng tổng của tập hợp con đó."
date: "2026-08-13T08:15:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "K"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 132
verified: true
draft: false
---

[CF 102299K - Dân nghèo](https://codeforces.com/problemset/problem/102299/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các giá chương tích cực. Bất cứ khi nào một khoản nợ đến hạn, Dostoyevskiy có thể bán bất kỳ tập hợp con nào của các chương đã viết sẵn và khoản nợ có thể được thanh toán chính xác khi giá trị của nó bằng tổng của tập hợp con đó. Cùng một chương không thể được bán hai lần cho một khoản nợ, vì vậy câu hỏi liên quan là câu hỏi tổng hợp con. 

Chúng ta cần số nguyên dương nhỏ nhất không thể được hình thành dưới dạng tổng của một số tập hợp con của các mức giá đã cho. Câu trả lời không nhất thiết phải lớn hơn giá mỗi chương. Ví dụ, với giá`2, 5`, khoản nợ`1`là điều không thể, nên câu trả lời là`1`. 

Mảng có thể chứa tới`5 * 10^5`chương, trong khi mỗi mức giá có thể lớn bằng`10^12`. Điều này ngay lập tức loại trừ việc liệt kê các tập hợp con, vì có thể có`2^n`của họ. Ngay cả một cách tiếp cận xem xét nhiều cặp hoặc nhiều khoảng chương cũng sẽ quá chậm ở quy mô này. Chúng ta cần một thuật toán gần`O(n log n)`, điều này thực tế đối với nửa triệu phần tử dưới giới hạn 2 giây. 

Giá trị lớn của mỗi mức giá cũng có nghĩa là mảng lập trình động có kích thước cố định được lập chỉ mục theo tổng số tiền là không khả thi. Tổng giá có thể đạt`5 * 10^17`, vượt xa những gì DP tính theo tổng có thể phân bổ. May mắn thay, giải pháp chỉ cần một giá trị chạy duy nhất, vì vậy các số nguyên có độ chính xác tùy ý của Python xử lý trực tiếp câu trả lời có khả năng lớn. 

Có một số trường hợp đặc biệt trong đó việc triển khai có thể âm thầm gặp trục trặc. Nếu giá nhỏ nhất lớn hơn`1`, thì câu trả lời sẽ có ngay`1`. Ví dụ, với đầu vào`1`và giá cả`2`, đầu ra là`1`, bởi vì không có tập hợp con nào có thể tạo ra một đồng rúp. Việc triển khai bất cẩn bắt đầu từ mức giá đầu tiên và cho rằng nó có thể tạo ra mọi giá trị bên dưới sẽ bỏ lỡ trường hợp này một cách không chính xác. 

Trường hợp cạnh thứ hai xảy ra khi tất cả các giá trị cho đến một điểm nào đó đều có thể xây dựng được nhưng giá tiếp theo lại tạo ra khoảng trống. Ví dụ, với giá`1 2`, chúng ta có thể làm`1`,`2`, Và`3`, nhưng không`4`, vậy câu trả lời là`4`. Việc triển khai chỉ kiểm tra xem bản thân giá trị hiện tại có phải là tổng tập hợp con hay không, thay vì theo dõi toàn bộ khoảng thời gian có thể truy cập liên tục, có thể bỏ sót khoảng cách này. 

Trường hợp thứ ba là khi giá tiếp theo lớn hơn chính xác một mức so với mọi thứ hiện có thể đạt được. Với giá cả`1 1 3`, hai giá trị đầu tiên chúng ta hãy xây dựng mọi tổng từ`0`bởi vì`2`. giá trị`3`mở rộng điều này đến`5`, vậy câu trả lời là`6`. Điều kiện biên phải là`price <= reachable + 1`, không`price < reachable + 1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi tập hợp con của các chương, tính tổng của nó và ghi lại những tổng nào có thể. có`2^n`tập hợp con. Ngay cả khi mỗi tập hợp con được duy trì một cách hiệu quả, chúng ta vẫn cần`O(2^n)`công việc. Nếu mọi tập hợp con được xây dựng bằng cách kiểm tra tất cả`n`các phần tử, số lượng hoạt động sẽ là`O(n * 2^n)`, điều này hoàn toàn không thể thực hiện được đối với`n = 5 * 10^5`. Thậm chí`2^60`đã vượt xa mọi thứ có thể thực hiện được trong thời hạn. 

Một giải pháp thay thế quen thuộc hơn là quy hoạch động tổng con. Chúng tôi có thể đánh dấu số tiền nào có thể truy cập được và cập nhật chúng theo từng mức giá. Vấn đề là tổng số tiền có thể lớn bằng`5 * 10^17`, do đó, thậm chí hiệu quả về mặt lý thuyết`O(n * sum)`DP không thể phù hợp với bộ nhớ hoặc thời gian. 

Cấu trúc hữu ích xuất phát từ thực tế là mọi mức giá đều dương. Giả sử chúng ta đã xử lý một số chương và có thể xây dựng mọi giá trị từ`1`bởi vì`R`. Bây giờ hãy xem xét mức giá tiếp theo`x`, sau khi sắp xếp tất cả các giá ngày càng tăng. 

Nếu như`x <= R + 1`, thì các chương cũ sẽ tạo ra mọi giá trị từ`0`bởi vì`R`, trong khi thêm`x`cho phép chúng tôi tạo ra mọi giá trị từ`x`bởi vì`x + R`. Bởi vì`x <= R + 1`, hai khoảng này chạm nhau hoặc chồng lên nhau. Do đó, sự kết hợp của họ là mọi giá trị từ`0`bởi vì`R + x`. 

Nếu thay vào đó`x > R + 1`, sau đó`R + 1`không thể hình thành được. Mỗi chương đã được xử lý nhiều nhất là`R`và mọi tập hợp con sử dụng`x`hoặc một chương sau, thậm chí lớn hơn có tổng ít nhất`x`, lớn hơn`R + 1`. Vì thế`R + 1`chắc chắn là giá trị nhỏ nhất không thể có được. 

Điều này đưa ra một thuật toán tham lam. Sắp xếp giá, duy trì tiền tố lớn nhất của số tiền dương được biết là hoàn toàn có thể xây dựng được và mở rộng phạm vi đó bất cứ khi nào giá tiếp theo không để lại khoảng trống. Phương pháp brute-force khám phá rõ ràng tất cả các tập hợp con vì nó không có thông tin về số tiền nào được đảm bảo tồn tại. Quan sát trên nén tất cả thông tin đó thành một số nguyên,`R`. 

Sự phức tạp dẫn đến là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * 2^n)`|`O(2^n)`| Quá chậm | 
| Tập hợp con DP |`O(n * S)`|`O(S)`| Quá chậm và quá nhiều bộ nhớ | 
| Tham lam tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

Đây`S`biểu thị tổng số tiền của tất cả giá chương. Phương pháp tối ưu dành gần như toàn bộ thời gian để sắp xếp mảng. 

## Hướng dẫn thuật toán 

1. Đọc tất cả giá của các chương và sắp xếp chúng theo thứ tự không giảm. Việc sắp xếp là điều cho phép chúng tôi suy luận về việc mọi mức giá chưa được xử lý ít nhất cũng lớn bằng mức giá hiện tại. 
2. Khởi tạo`reachable = 0`. Điều này có nghĩa là không sử dụng chương nào, chúng ta có thể xây dựng tổng`0`và tại thời điểm này mọi giá trị trong khoảng từ`1`bởi vì`reachable`được xây dựng một cách trống rỗng. 
3. Xử lý giá được sắp xếp từ nhỏ nhất đến lớn nhất. Đối với mức giá hiện tại`x`, so sánh nó với`reachable + 1`, số tiền dương nhỏ nhất chưa đảm bảo có thể xây dựng được. 
4. Nếu`x > reachable + 1`, trở lại`reachable + 1`. Các chương trước chỉ có thể tính tổng tối đa`reachable`, trong khi bất kỳ tập hợp con nào chứa`x`có giá trị ít nhất`x`. Như vậy`reachable + 1`nằm trong một khoảng trống thực sự và là câu trả lời. 
5. Nếu không,`x <= reachable + 1`, vì vậy hãy thêm`x`ĐẾN`reachable`. Trước khi thêm`x`, mọi tổng từ`0`bởi vì`reachable`đã có thể. sử dụng`x`cùng với những tập hợp con giống nhau sẽ cho mọi tổng từ`x`bởi vì`x + reachable`. Từ`x <= reachable + 1`, không có khoảng cách giữa hai khoảng, vì vậy mọi giá trị từ`0`bởi vì`reachable + x`trở nên có thể. 
6. Nếu tất cả giá được xử lý mà không tìm thấy khoảng trống, hãy quay lại`reachable + 1`. Tại thời điểm đó mọi giá trị từ`0`thông qua tổng số tiền là có thể xây dựng được, trong khi số tiền lớn hơn tổng số là không thể. Giá trị đầu tiên như vậy chính xác là`reachable + 1`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý mọi giá trước giá hiện tại, mọi số nguyên từ`0`bởi vì`reachable`có thể được hình thành. Nếu giá tiếp theo vượt quá`reachable + 1`, sau đó`reachable + 1`không thể được hình thành: các tập hợp con không có giá đó nhiều nhất là`reachable`và các tập con chứa nó ít nhất có giá tiếp theo. Nếu giá cao nhất`reachable + 1`, số tiền không bao gồm nó`[0, reachable]`, và số tiền sử dụng nó bao gồm`[x, x + reachable]`. Các khoảng này chồng lên nhau hoặc chạm vào nhau, vì vậy chúng cùng nhau bao phủ`[0, reachable + x]`. Do đó, bất biến được bảo toàn và khoảng cách được phát hiện đầu tiên chính xác là tổng dương nhỏ nhất không thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    prices = list(map(int, input().split()))

    prices.sort()

    reachable = 0

    for x in prices:
        if x > reachable + 1:
            print(reachable + 1)
            return
        reachable += x

    print(reachable + 1)

if __name__ == "__main__":
    solve()
```Hai dòng đầu tiên ghi số chương và giá của chúng. Vì đầu vào chứa chính xác`n`giá trên dòng thứ hai, một cuộc gọi đến`input()`là đủ cho định dạng được chỉ định. 

Bước sắp xếp sẽ xếp giá theo thứ tự tăng dần. Bằng chứng tham lam phụ thuộc vào thứ tự này bởi vì, sau khi quyết định rằng giá hiện tại quá lớn để bù đắp cho giá trị còn thiếu tiếp theo, mọi mức giá còn lại ít nhất cũng lớn bằng và do đó cũng không thể lấp đầy khoảng trống đó.`reachable`đại diện cho số nguyên lớn nhất sao cho mọi giá trị từ`0`bởi vì`reachable`có thể xây dựng được. Séc`x > reachable + 1`là ranh giới quan trọng. Sự bình đẳng được cho phép bởi vì nếu`x == reachable + 1`, chương mới sẽ tự điền vào giá trị còn thiếu tiếp theo và mở rộng phạm vi liên tục. 

Khi giá hiện tại có thể sử dụng được,`reachable += x`cập nhật điểm cuối của khoảng thời gian mới được bảo hiểm. Không cần lưu trữ tập hợp con nào tạo ra tổng, bởi vì bất biến chỉ yêu cầu biết rằng tất cả các giá trị trong khoảng đều có sẵn. 

Số nguyên Python thích hợp ở đây vì tổng có thể đạt tới`5 * 10^17`. Các ngôn ngữ có loại số nguyên có chiều rộng cố định cần số nguyên 64 bit cho giá trị này, trong khi Python tự động phát triển biểu diễn số nguyên nếu cần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Giá cả là`2, 1, 10000000000`. Sau khi sắp xếp, chúng trở thành`1, 2, 10000000000`. 

| Giá hiện tại`x`|`reachable`trước |`reachable + 1`| Hành động |`reachable`sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 |`1 <= 1`, mở rộng | 1 | 
| 2 | 1 | 2 |`2 <= 2`, mở rộng | 3 | 
| 10000000000 | 3 | 4 |`10000000000 > 4`, dừng lại | 3 | 

Sau khi xử lý`1`Và`2`, mọi giá trị từ`0`bởi vì`3`có thể:`0`,`1`,`2`, Và`1 + 2 = 3`. Chương tiếp theo có giá mười tỷ, nên không có tập hợp con nào liên quan đến nó có thể tạo ra`4`. Câu trả lời là do đó`4`. 

### Mẫu 2 

Có một chương có giá`2`. 

| Giá hiện tại`x`|`reachable`trước |`reachable + 1`| Hành động |`reachable`sau | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 1 |`2 > 1`, dừng lại | 0 | 

Không có tổng dương nào nhỏ hơn`2`có thể được hình thành, vì vậy`1`ngay lập tức là món nợ không thể trả được nhỏ nhất. Dấu vết này thực hiện trường hợp câu trả lời nhỏ hơn chương rẻ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Việc sắp xếp chiếm ưu thế trong quá trình quét tham lam tuyến tính. | 
| Không gian |`O(n)`| Giá được lưu trữ trong một mảng; bản thân quá trình quét sử dụng`O(1)`trạng thái bổ sung. | 

Với nhiều nhất`5 * 10^5`giá cả, việc sắp xếp là thực tế trong giới hạn nhất định và quá trình quét tuyến tính sau đây nhỏ so với chi phí sắp xếp. Thuật toán không bao giờ phân bổ bộ nhớ tỷ lệ thuận với tổng giá, điều này rất cần thiết vì tổng giá đó có thể đạt tới`5 * 10^17`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    prices = list(map(int, input().split()))
    prices.sort()

    reachable = 0

    for x in prices:
        if x > reachable + 1:
            print(reachable + 1)
            return
        reachable += x

    print(reachable + 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("3\n2 1 10000000000\n") == "4", "sample 1"
assert run("1\n2\n") == "1", "sample 2"

# Minimum-size input, cheapest possible chapter
assert run("1\n1\n") == "2", "single chapter worth 1"

# All equal values
assert run("5\n1 1 1 1 1\n") == "6", "all equal values"

# Boundary case: x == reachable + 1 must extend the range
assert run("3\n1 1 3\n") == "6", "exact boundary"

# Gap after several constructible values
assert run("4\n1 2 2 10\n") == "6", "gap after continuous range"

# Large values must not cause fixed-range DP assumptions
assert run("3\n1000000000000 1000000000000 1000000000000\n") == "1", "large prices"

# Maximum-size case, all values equal to 1
n = 500000
assert run(f"{n}\n" + " ".join(["1"] * n) + "\n") == "500001", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`2`| Đầu vào tối thiểu và trường hợp mọi giá trị dương hiện có đều có thể xây dựng được. | 
|`5 / 1 1 1 1 1`|`6`| Các giá trị bằng nhau lặp đi lặp lại và khoảng thời gian dài liên tục có thể truy cập được. | 
|`3 / 1 1 3`|`6`| Chính xác`x = reachable + 1`ranh giới. | 
|`4 / 1 2 2 10`|`6`| Phát hiện khoảng cách thực sự đầu tiên sau một số lần mở rộng thành công. | 
|`3 / 1000000000000 ...`|`1`| Giá rất lớn và trường hợp thiếu giá trị ngay lập tức. | 
|`500000 / 1 1 ... 1`|`500001`| Tối đa`n`và quét tuyến tính sau khi sắp xếp. | 

## Vỏ cạnh 

Nếu chương rẻ nhất có giá hơn một rúp thì câu trả lời phải là`1`. Đối với đầu vào```
1
2
```ban đầu`reachable`là`0`, Vì thế`reachable + 1`là`1`. Vì giá được sắp xếp`2`lớn hơn`1`, thuật toán trả về`1`trước khi thêm chương. Điều này ngăn ngừa sai lầm phổ biến khi cho rằng câu trả lời ít nhất phải là giá chương nhỏ nhất. 

Đối với một khoảng trống sau một khoảng thời gian đã hoàn thành, hãy xem xét```
4
1 2 2 10
```Sau khi phân loại, giá đầu tiên`1`thay đổi`reachable`từ`0`ĐẾN`1`. giá cả`2`thỏa mãn`2 <= 2`, Vì thế`reachable`trở thành`3`. Tiếp theo`2`thỏa mãn`2 <= 4`, mở rộng phạm vi đến`5`. Giá cuối cùng`10`lớn hơn`6`, Vì thế`6`không thể được hình thành và được trả lại. Hai bản sao nhỏ hơn của`2`chứng minh tại sao thuật toán phải xem xét toàn bộ khoảng thời gian đã có thể tiếp cận thay vì chỉ các tổng tập hợp con riêng lẻ. 

Ranh giới bình đẳng được xử lý bởi```
3
1 1 3
```đầu tiên`1`cho`reachable = 1`, và cái thứ hai cho`reachable = 2`. Giá tiếp theo chính xác`3`, bằng`reachable + 1`, nên phải chấp nhận. Khoảng thời gian có thể truy cập mở rộng đến`5`, và câu trả lời trở thành`6`. Sử dụng một so sánh nghiêm ngặt như`x >= reachable + 1`sẽ quay lại không chính xác`3`. 

Giá trị chương rất lớn không yêu cầu xử lý đặc biệt. Vì```
3
1000000000000 1000000000000 1000000000000
```giá được sắp xếp đầu tiên đã lớn hơn`reachable + 1 = 1`, do đó thuật toán trả về`1`. Điều này cũng minh họa tại sao một DP được lập chỉ mục bằng số tiền có thể có là một sự trừu tượng sai lầm: các giá trị số có thể rất lớn ngay cả khi câu trả lời thực tế là rất nhỏ.
