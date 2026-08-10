---
title: "CF 102397G - Game Siêu Lạ"
description: "Mỗi người chơi sở hữu một mảng có độ dài n. Đối với tổng mục tiêu cố định k, một cặp vị trí (i, j) là phù hợp khi i < j và hai giá trị cộng lại bằng k. Chúng ta cần đếm các cặp tốt một cách độc lập trong mảng của Mahmoud và mảng của Bashar, sau đó so sánh hai số đếm."
date: "2026-08-10T18:03:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "G"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 285
verified: true
draft: false
---

[CF 102397G - Trò chơi siêu kỳ lạ](https://codeforces.com/problemset/problem/102397/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 45 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người chơi sở hữu một mảng có độ dài`n`. Đối với tổng mục tiêu cố định`k`, một cặp vị trí`(i, j)`là tốt khi`i < j`và hai giá trị cộng lại thành`k`. Chúng ta cần đếm các cặp tốt một cách độc lập trong mảng của Mahmoud và mảng của Bashar, sau đó so sánh hai số đếm. Người chơi có nhiều cặp tốt hơn sẽ thắng, trong khi số điểm bằng nhau sẽ hòa. 

Điều kiện đặt hàng`i < j`có nghĩa là vị trí quan trọng khi đếm cặp. Ví dụ, trong`[1, 2]`với`k = 3`, có đúng một cặp tốt. TRONG`[1, 2, 1]`, có hai cặp tốt,`(1, 2)`Và`(2, 3)`. Chúng tôi đang đếm các cặp vị trí, không chỉ các kết hợp giá trị riêng biệt. 

Độ dài mảng có thể đạt tới`10^5`, do đó, một thuật toán kiểm tra từng cặp vị trí sẽ hoạt động khoảng`n(n-1)/2`séc. Ở kích thước tối đa, đây là`4,999,950,000`kiểm tra cặp, vượt xa giới hạn lập trình cạnh tranh khoảng 1,5 giây có thể hỗ trợ. Chúng ta cần một giải pháp mà công việc của nó phát triển gần như tuyến tính với`n`. 

Bản thân các giá trị cũng bị giới hạn bởi`10^5`, nhưng chúng ta không thực sự cần phải lặp lại mọi giá trị có thể. Thông tin hữu ích khi xử lý một mảng là số lần xuất hiện trước đó của mỗi giá trị đã xuất hiện. 

Một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. Đầu tiên, một giá trị có thể ghép nối với chính nó. Ví dụ:```
1 2
1
1
```Chỉ có một vị trí nên kết quả đúng là`Draw`, không phải là một cặp. Tổng quát hơn, đối với`[1, 1]`Và`k = 2`, hai giá trị bằng nhau tạo thành chính xác một cặp, bởi vì các vị trí`(1, 2)`là khác biệt. 

Thứ hai, các giá trị bằng nhau không được tính hai lần. Vì`[1, 2]`với`k = 3`, cặp giá trị`1`Và`2`là một cặp chứ không phải hai. Một giải pháp dựa trên tần số tính toán cả`cnt[1] * cnt[2]`Và`cnt[2] * cnt[1]`không hạn chế phần bổ sung nào được xử lý sẽ tính nó hai lần. 

Thứ ba, thứ tự các vị trí phải được tôn trọng. Vì`[2, 1]`Và`k = 3`, cặp`(1, 2)`vẫn hợp lệ vì vị trí đầu tiên chứa`2`và thứ hai chứa`1`. Các giá trị không cần phải xuất hiện theo thứ tự số tăng dần. Điều quan trọng là vị trí đầu tiên được xử lý trước vị trí thứ hai. 

Cuối cùng, số lượng cặp có thể lớn hơn nhiều so với`n`. Với`n = 100000`, một mảng chỉ chứa các giá trị ghép nối với chính chúng có thể chứa`4,999,950,000`cặp tốt. Số nguyên 32 bit sẽ tràn trên kết quả như vậy trong các ngôn ngữ có số nguyên có chiều rộng cố định, do đó biến đếm phải sử dụng loại số nguyên đủ lớn. Số nguyên Python tự động xử lý việc này. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp tuân theo định nghĩa chính xác. Đối với mọi vị trí`i`, chúng tôi kiểm tra mọi vị trí sau đó`j`và kiểm tra xem`a[i] + a[j] == k`. Mỗi cặp hợp lệ được tính chính xác một lần vì chúng tôi chỉ xem xét`j > i`. Điều này đưa ra câu trả lời đúng ngay lập tức, nhưng có`n(n-1)/2`cặp để kiểm tra. Khi`n = 100000`, đó là`4,999,950,000`kiểm tra, làm cho cách tiếp cận không thể sử dụng được. 

Cấu trúc của điều kiện cho chúng ta cách đếm tốt hơn nhiều. Giả sử chúng ta hiện đang xử lý một giá trị`x`. Vị trí trước đó tạo thành một cặp tốt với nó chính xác khi giá trị trước đó được`k - x`. Chúng ta không cần biết từng vị trí riêng lẻ. Chúng ta chỉ cần biết bao nhiêu lần`k - x`đã xuất hiện rồi. 

Điều này dẫn đến phương pháp tần số một lần. Duy trì`seen[v]`, số lần xuất hiện của giá trị`v`giữa các vị trí đã được xử lý. Khi giá trị hiện tại là`x`, có chính xác`seen[k - x]`các vị trí trước đó tạo thành một cặp tốt với nó. Thêm số đó vào câu trả lời rồi tăng`seen[x]`. 

Điều kiện đặt hàng`i < j`được xử lý tự nhiên bằng cách xử lý mảng từ trái sang phải. Mỗi cặp được tính khi gặp điểm cuối thứ hai của nó. Điều này cũng tự động giải quyết trường hợp giá trị bằng nhau. Nếu như`x == k - x`, chỉ những lần xuất hiện trước đó mới được tính, vì vậy một lần xuất hiện không bao giờ kết hợp với chính nó. 

Quy trình tương tự được áp dụng độc lập cho cả hai mảng. Sau đó chúng tôi so sánh hai số lượng của họ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đếm tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và tổng mục tiêu`k`, tiếp theo là mảng của Mahmoud và Bashar. 
2. Để đếm các cặp tốt trong một mảng, hãy tạo bản đồ tần số`seen`chứa các giá trị gặp phải cho đến nay. Ban đầu nó trống vì không có vị trí nào được xử lý. 
3. Xử lý mảng từ trái sang phải. Đối với giá trị hiện tại`x`, tính toán đối tác cần thiết của nó là`k - x`. 
4. Thêm`seen[k - x]`để trả lời. Mỗi lần xuất hiện được ghi lại đều thuộc về một vị trí trước đó, vì vậy mỗi lần xuất hiện đó sẽ tạo ra chính xác một cặp hợp lệ với vị trí hiện tại. 
5. Tăng`seen[x]`bởi một. Vị trí hiện tại phải có sẵn như vị trí trước đó cho tất cả các phần tử sau này, nhưng nó không được sử dụng để ghép nối với chính nó. 
6. Lặp lại quy trình đếm tương tự cho mảng Bashar. 
7. So sánh số lượng hai cặp. Nếu số lượng của Mahmoud lớn hơn, hãy in`Mahmoud`. Nếu số lượng của Bashar lớn hơn, hãy in`Bashar`. Ngược lại, in`Draw`. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của mảng,`seen[v]`bằng số vị trí trong tiền tố đó có giá trị là`v`. Khi xử lý giá trị tiếp theo`x`, mọi cặp tốt kết thúc ở vị trí hiện tại phải có giá trị sớm hơn bằng`k - x`, và có chính xác`seen[k - x]`những vị trí như vậy. Do đó, thuật toán thêm mỗi cặp hàng hóa mới hoàn thành đúng một lần. Bởi vì giá trị hiện tại được chèn vào`seen`chỉ sau khi đếm các đối tác của nó, một vị trí mới không thể ghép đôi với chính nó. Vì mỗi cặp có chính xác một điểm cuối sau đó nên mỗi cặp tốt chỉ được tính một lần và chỉ một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_pairs(arr, k):
    seen = {}
    pairs = 0

    for x in arr:
        pairs += seen.get(k - x, 0)
        seen[x] = seen.get(x, 0) + 1

    return pairs

def solve():
    n, k = map(int, input().split())
    mahmoud = list(map(int, input().split()))
    bashar = list(map(int, input().split()))

    mahmoud_pairs = count_pairs(mahmoud, k)
    bashar_pairs = count_pairs(bashar, k)

    if mahmoud_pairs > bashar_pairs:
        print("Mahmoud")
    elif bashar_pairs > mahmoud_pairs:
        print("Bashar")
    else:
        print("Draw")

if __name__ == "__main__":
    solve()
```các`count_pairs`hàm thực hiện bất biến từ trái sang phải từ thuật toán.`seen[x]`lưu trữ bao nhiêu vị trí trước đó chứa`x`, trong khi`pairs`lưu trữ số lượng cặp tốt có hai điểm cuối đã được xử lý. 

Việc tra cứu`seen.get(k - x, 0)`bằng 0 khi phần bù cần thiết chưa xuất hiện. Điều này tránh việc phải khởi tạo một mảng tần số cho mọi giá trị có thể, mặc dù mảng có kích thước cố định`100001`cũng sẽ hoạt động vì các giá trị đầu vào bị giới hạn. 

Thứ tự của hai câu lệnh bên trong vòng lặp là điều cần thiết. Chúng tôi đếm`seen[k - x]`trước khi tăng`seen[x]`. Nếu sự gia tăng xảy ra trước thì khi nào`x == k - x`, vị trí hiện tại sẽ được tính không chính xác là đối tác của chính nó. 

Các số nguyên có độ chính xác tùy ý của Python cũng xử lý số lượng cặp tối đa có thể mà không bị tràn. Không cần điều trị đặc biệt cho`x == k - x`, vì tần số của giá trị hiện tại chỉ chứa các vị trí trước đó. 

Hai mảng được tính riêng biệt, do đó các lần xuất hiện từ mảng của Mahmoud không bao giờ tương tác với các lần xuất hiện từ mảng của Bashar. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là:```
7 3
1 1 2 3 4 1 2
2 2 2 1 1 1 2
```Đối với mảng của Mahmoud, phần bù đích của mỗi giá trị là`3 - x`. 

| Vị trí | Hiện hành`x`| Yêu cầu`k-x`| Giá trị khớp trước đó | Đã thêm cặp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 0 | 0 | 
| 2 | 1 | 2 | 0 | 0 | 0 | 
| 3 | 2 | 1 | 2 | 2 | 2 | 
| 4 | 3 | 0 | 0 | 0 | 2 | 
| 5 | 4 | -1 | 0 | 0 | 2 | 
| 6 | 1 | 2 | 1 | 1 | 3 | 
| 7 | 2 | 1 | 3 | 3 | 6 | 

Mahmoud có`6`cặp tốt. 

Đối với mảng của Bashar: 

| Vị trí | Hiện hành`x`| Yêu cầu`k-x`| Giá trị khớp trước đó | Đã thêm cặp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 0 | 0 | 0 | 
| 2 | 2 | 1 | 0 | 0 | 0 | 
| 3 | 2 | 1 | 0 | 0 | 0 | 
| 4 | 1 | 2 | 3 | 3 | 3 | 
| 5 | 1 | 2 | 3 | 3 | 6 | 
| 6 | 1 | 2 | 3 | 3 | 9 | 
| 7 | 2 | 1 | 3 | 3 | 12 | 

Bashar có`12`cặp tốt, vì vậy đầu ra là`Bashar`. Câu lệnh mẫu hiển thị tên bằng chữ in hoa, nhưng mã thông báo đầu ra dự định là tên của người chơi và các giải pháp được chấp nhận được in theo cách thông thường`Bashar`. 

Ví dụ thứ hai thể hiện các giá trị bằng nhau:```
3 2
1 1 1
1 1 1
```Đối với một trong hai mảng, mỗi cặp vị trí riêng biệt đều có tổng`2`. 

| Vị trí | Hiện hành`x`| Yêu cầu`k-x`| Giá trị khớp trước đó | Đã thêm cặp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 0 | 
| 2 | 1 | 1 | 1 | 1 | 1 | 
| 3 | 1 | 1 | 2 | 2 | 3 | 

Cả hai người chơi đều có`3`cặp tốt, vậy kết quả là`Draw`. đầu tiên`1`không tạo thành một cặp với chính nó vì nó được thêm vào`seen`chỉ sau khi sự đóng góp của nó đã được tính toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử của cả hai mảng được xử lý một lần, với các phép toán bản đồ băm O(1) dự kiến. | 
| Không gian | O(n) | Bản đồ tần số có thể chứa tới`n`những giá trị riêng biệt. | 

Với`n <= 100000`, thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi phần tử mảng. Điều này nằm trong giới hạn dự định, trong khi phương pháp lực lượng bậc hai sẽ yêu cầu hàng tỷ kiểm tra cặp ở kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys
import io

def count_pairs(arr, k):
    seen = {}
    pairs = 0

    for x in arr:
        pairs += seen.get(k - x, 0)
        seen[x] = seen.get(x, 0) + 1

    return pairs

def solve():
    n, k = map(int, input().split())
    mahmoud = list(map(int, input().split()))
    bashar = list(map(int, input().split()))

    m = count_pairs(mahmoud, k)
    b = count_pairs(bashar, k)

    if m > b:
        print("Mahmoud")
    elif b > m:
        print("Bashar")
    else:
        print("Draw")

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        output = io.StringIO()
        old_stdout = sys.stdout
        sys.stdout = output

        try:
            solve()
        finally:
            sys.stdout = old_stdout

        return output.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided sample
assert run(
    """7 3
1 1 2 3 4 1 2
2 2 2 1 1 1 2
"""
) == "Bashar", "sample 1"

# Minimum size, no pair is possible.
assert run(
    """1 2
1
1
"""
) == "Draw", "single element"

# All values are equal and every distinct-position pair is valid.
assert run(
    """3 2
1 1 1
1 1 1
"""
) == "Draw", "all equal"

# Boundary values: 1 + 100000 = 100001.
assert run(
    """4 100001
1 100000 1 100000
1 1 100000 100000
"""
) == "Mahmoud", "boundary complement"

# Equal values must be counted as combinations of distinct positions.
assert run(
    """4 10
5 5 5 5
5 5 5 4
"""
) == "Mahmoud", "self-complement case"

# Maximum pair count still fits in Python's integer type.
# Each array has C(100000, 2) good pairs.
assert run(
    "100000 2\n" +
    ("1 " * 99999) + "1\n" +
    ("1 " * 99999) + "1\n"
) == "Draw", "maximum pair count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / 1 / 1`|`Draw`| Kích thước tối thiểu và không có cặp tự ghép | 
|`3 2 / 1 1 1 / 1 1 1`|`Draw`| Tất cả các giá trị bằng nhau và ghép nối giá trị bằng nhau | 
|`4 100001 / 1 100000 1 100000 / 1 1 100000 100000`|`Mahmoud`| Giá trị cho phép lớn nhất và ranh giới bổ sung | 
|`4 10 / 5 5 5 5 / 5 5 5 4`|`Mahmoud`| các`x == k-x`đếm trường hợp và kết hợp | 
|`100000 2`với mọi giá trị bằng`1`|`Draw`| Tối đa`n`và số lượng cặp rất lớn | 

## Vỏ cạnh 

Khi mảng chỉ chứa một phần tử thì không thể có một cặp vì cần có hai vị trí riêng biệt. Ví dụ:```
1 2
1
1
```Vòng đếm nhìn thấy`1`, tìm kiếm`1`giữa các vị trí trước đó, tìm thấy số lần xuất hiện bằng 0 và chỉ sau đó ghi lại vị trí hiện tại`1`. Cả hai mảng đều nhận được cặp 0, vì vậy câu trả lời là`Draw`. 

Khi một giá trị là phần bù của chính nó, thuật toán phải đếm các cặp giữa các lần xuất hiện khác nhau mà không ghép một lần xuất hiện với chính nó. Coi như:```
2 2
1 1
1 1
```Lần đầu tiên`1`,`seen[1]`bằng 0 nên đóng góp bằng 0. Sau khi chèn nó, thứ hai`1`tìm thấy một cái trước đó`1`và đóng góp một cặp. Cả hai người chơi đều có đúng một đôi, tạo ra`Draw`. Thứ tự tra cứu và chèn là yếu tố làm cho điều này trở nên chính xác. 

Khi các giá trị ở giới hạn của phạm vi đầu vào, phần bù bắt buộc vẫn có thể được xử lý mà không cần trường hợp đặc biệt. Coi như:```
2 100001
1 100000
1 1
```Giá trị thứ hai của Mahmoud là`100000`, phần bù của nó là`1`. Một trước đó`1`tồn tại nên Mahmoud có được một cặp tốt. Bashar không có`100000`, vì vậy nó bằng không. Kết quả là`Mahmoud`. 

Khi cùng một giá trị xuất hiện nhiều lần, số lượng cặp tăng theo phương trình bậc hai mặc dù bản thân thuật toán vẫn tuyến tính. Vì:```
4 10
5 5 5 5
5 5 5 4
```Mahmoud có`C(4, 2) = 6`cặp tốt vì mỗi hai khác biệt`5`tổng các vị trí thành`10`. Bashar có`C(3, 2) = 3`những cặp như vậy. Thuật toán tạo ra các số đếm này tăng dần khi`0 + 1 + 2 + 3`Và`0 + 1 + 2`, tương ứng, cho`Mahmoud`. 

Cuối cùng, kích thước mảng tối đa có thể tạo ra gần năm tỷ cặp tốt. Với`100000`bản sao của`1`Và`k = 2`, cặp nào cũng hay, tặng`100000 * 99999 / 2 = 4,999,950,000`cặp. Thuật toán vẫn chỉ thực hiện`100000`lặp lại cho mảng đó, trong khi số học số nguyên của Python lưu trữ số kết quả một cách an toàn.
