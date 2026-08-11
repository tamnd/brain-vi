---
title: "CF 102397B - Tính diện tích"
description: "Chúng ta cần xây dựng lại kích thước của một mảnh đất hình chữ nhật từ diện tích của nó. Dữ liệu vào chứa một số nguyên n, biểu thị diện tích hình chữ nhật. Chúng ta cần in ra hai số nguyên dương chiều cao và chiều rộng có tích chính xác là n."
date: "2026-08-11T15:47:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "B"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 95
verified: true
draft: false
---

[CF 102397B - Tính diện tích](https://codeforces.com/problemset/problem/102397/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng lại kích thước của một mảnh đất hình chữ nhật từ diện tích của nó. Đầu vào chứa một số nguyên duy nhất`n`, đại diện cho diện tích của hình chữ nhật. Chúng ta cần in hai số nguyên dương`height`Và`width`sản phẩm của ai chính xác là`n`. 

Ví dụ, nếu`n = 20`, cả hai`4 5`Và`2 10`mô tả các hình chữ nhật hợp lệ vì tích của chúng là 20. Bài toán cho phép rõ ràng bất kỳ cặp hợp lệ nào, do đó không cần phải tìm một hình chữ nhật duy nhất hoặc tối ưu hóa các kích thước. 

Ràng buộc`1 <= n <= 200`làm cho vấn đề trở nên cực kỳ nhỏ. Ngay cả một thuật toán kiểm tra mọi số nguyên từ 1 đến`n`thực hiện tối đa 200 lần lặp, đủ nhanh cho giới hạn 1,5 giây. Tuy nhiên, cấu trúc toán học mang lại cho chúng ta một cách rõ ràng hơn`O(sqrt(n))`tiếp cận. Điều này quan trọng về mặt khái niệm vì các bài toán cặp nhân tố thường có giới hạn lớn hơn nhiều, trong đó việc kiểm tra mọi giá trị lên đến`n`sẽ trở nên quá đắt. 

Các trường hợp cạnh chính đến từ các hình vuông hoàn hảo và diện tích nhỏ nhất có thể. Vì`n = 1`, kích thước duy nhất có thể là`1 1`. Việc triển khai bất cẩn bắt đầu kiểm tra từ 2 sẽ không bao giờ tìm thấy yếu tố nào và có thể không in được bất cứ thứ gì. 

Đối với một hình vuông hoàn hảo như`n = 16`, kích thước`4 4`là hợp lệ vì cùng một yếu tố có thể được sử dụng hai lần. Việc triển khai chỉ tìm kiếm không chính xác hai yếu tố khác nhau có thể từ chối trường hợp này mặc dù các thứ nguyên bằng nhau là hoàn toàn hợp lệ. 

Đối với một khu vực đắc địa như`n = 7`, không có hệ số giữa 2 và`sqrt(7)`. Hình chữ nhật hợp lệ chỉ đơn giản là`1 7`. Việc triển khai giả định mọi số đều có hệ số không tầm thường có thể thất bại ở đây. 

Định dạng mẫu trong câu lệnh được cung cấp bỏ qua các dòng đầu vào tương ứng, nhưng các cặp đầu ra được hiển thị rõ ràng tương ứng với các khu vực`20`,`16`, Và`6`:`4 * 5 = 20`,`4 * 4 = 16`, Và`2 * 3 = 6`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Hãy thử mọi chiều cao có thể từ 1 đến`n`, và bất cứ khi nào`n`có thể chia hết cho chiều cao đó, hãy sử dụng`n / height`như chiều rộng. Số chia thành công đầu tiên ngay lập tức cho một cặp hợp lệ vì`height * (n / height) = n`. Trong trường hợp xấu nhất, điều này thực hiện`n`kiểm tra tính chia hết. Với hạn chế thực tế`n <= 200`, điều đó có nghĩa là tối đa 200 lần kiểm tra, vì vậy ở đây hoàn toàn chấp nhận biện pháp bạo lực. Nó sẽ chỉ trở nên quá chậm nếu ràng buộc tăng lên đáng kể, ví dụ như với các giá trị xung quanh`10^9`hoặc lớn hơn, trong đó hiệu suất lên tới`n`lặp đi lặp lại không còn thực tế nữa. 

Quan sát hữu ích là các yếu tố đi theo cặp. Nếu như`a * b = n`Và`a <= b`, sau đó`a <= sqrt(n)`. Do đó, nếu chúng ta tìm kiếm hướng xuống từ`floor(sqrt(n))`, giá trị đầu tiên chia`n`đưa ra một cặp nhân tố ngay lập tức. Không có lý do để kiểm tra bất cứ điều gì lớn hơn`sqrt(n)`, bởi vì hệ số phù hợp của nó sẽ nhỏ hơn hoặc bằng`sqrt(n)`. 

Điều này làm giảm việc tìm kiếm tối đa`n`kiểm tra tối đa`sqrt(n)`séc. Đối với bài toán này, cả hai phương pháp đều đủ nhanh, nhưng cách tiếp cận căn bậc hai nắm bắt được lý do tiêu chuẩn đằng sau các bài toán cặp nhân tố và có quy mô tốt hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận cho`n <= 200`| 
| Tìm kiếm từ`sqrt(n)`| O(sqrt(n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc khu vực`n`. Chúng ta cần tìm ước số dương bất kỳ của`n`và sử dụng yếu tố bổ sung của nó làm chiều thứ hai. 
2. Tính toán`floor(sqrt(n))`. Một cặp nhân tố luôn có thể được sắp xếp sao cho nhân tố nhỏ hơn nhiều nhất là`sqrt(n)`, vì vậy việc tìm kiếm ngoài ranh giới này là không cần thiết. 
3. Bắt đầu lúc`floor(sqrt(n))`và di chuyển xuống 1. Việc tìm kiếm đi xuống thuận tiện vì nó tìm thấy cặp nhân tố có số chiều lớn nhất có thể nhỏ hơn, mặc dù bài toán không yêu cầu cặp nhân tố cụ thể này. 
4. Đối với mỗi ứng viên`h`, kiểm tra xem`n % h == 0`. Nếu số dư bằng 0 thì`h`chia diện tích một cách chính xác, vì vậy`n // h`là một số nguyên và cặp`h, n // h`tạo thành một hình chữ nhật hợp lệ. 
5. In`h`Và`n // h`và dừng lại. Một ước số được đảm bảo tìm thấy vì 1 chia hết cho mọi số nguyên dương. 

### Tại sao nó hoạt động 

Điều bất biến là mọi ứng cử viên được vòng lặp xem xét nhiều nhất là`sqrt(n)`. Đối với mọi nhân tử hợp lệ`n = a * b`, ít nhất một trong`a`Và`b`nhiều nhất là`sqrt(n)`. Vì vòng lặp kiểm tra mọi số nguyên từ`floor(sqrt(n))`xuống 1 thì cuối cùng nó cũng phải gặp phải hệ số như vậy. Khi đó, giá trị bổ sung`n // a`thỏa mãn`a * (n // a) = n`, nên kích thước in luôn có diện tích chính xác theo yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

h = int(n ** 0.5)

while h >= 1:
    if n % h == 0:
        print(h, n // h)
        break
    h -= 1
```Dòng đầu tiên đọc giá trị vùng đơn. Chỉ có một ca kiểm thử nên không cần vòng lặp ca kiểm thử bên ngoài. 

biểu hiện`int(n ** 0.5)`đưa ra phần nguyên của căn bậc hai cho những ràng buộc rất nhỏ này. Sau đó, chúng tôi kiểm tra các kích thước nhỏ hơn có thể có từ giá trị đó xuống 1. 

Kiểm tra tính chia hết là hoạt động trung tâm. Nếu như`n % h`bằng 0, phép chia số nguyên sẽ cho chiều bù chính xác. Chúng tôi in ngay lập tức vì bất kỳ cặp hợp lệ nào đều được chấp nhận. 

Điều kiện vòng lặp sử dụng`h >= 1`còn hơn là`h > 1`bởi vì 1 là hệ số dự phòng cho mọi giá trị dương`n`. Điều này xử lý cả số nguyên tố và`n = 1`không có trường hợp đặc biệt. 

Không có vấn đề tràn số nguyên trong Python và phép chia chỉ được thực hiện sau khi khả năng chia hết được xác nhận. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu được hiển thị đầu tiên, diện tích là`20`và kích thước hiển thị dự kiến ​​​​là`4 5`. 

|`n`|`h`|`n % h`| Hành động | 
| --- | --- | --- | --- | 
| 20 | 4 | 0 | In`4 5`| 

Căn bậc hai của 20 xấp xỉ 4,47, do đó vòng lặp bắt đầu ở 4. Vì 20 chia hết cho 4 nên thuật toán ngay lập tức thu được thừa số bù 5. Bất biến được thỏa mãn vì`4 * 5 = 20`. 

### Mẫu 2 

Đối với mẫu hiển thị thứ hai, diện tích là`16`và kích thước dự kiến ​​​​là`4 4`. 

|`n`|`h`|`n % h`| Hành động | 
| --- | --- | --- | --- | 
| 16 | 4 | 0 | In`4 4`| 

Đây`sqrt(16) = 4`, do đó vòng lặp bắt đầu chính xác ở căn bậc hai. Hệ số căn bậc hai chia số, tạo ra hai chiều bằng nhau. Điều này chứng tỏ tại sao hình vuông hoàn hảo không cần xử lý đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(n)) | Nhiều nhất`floor(sqrt(n))`việc kiểm tra tính chia hết được thực hiện. | 
| Không gian | O(1) | Chỉ có khu vực và một yếu tố ứng cử viên được lưu trữ. | 

Với`n <= 200`, vòng lặp thực hiện ít hơn 15 lần kiểm tra vì`sqrt(200)`là khoảng 14.14. Do đó, giải pháp có biên độ rất lớn dưới giới hạn thời gian 1,5 giây và sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm 

Vì bài toán chấp nhận bất kỳ cặp nhân tố hợp lệ nào nên trình trợ giúp kiểm tra bên dưới sẽ xác thực thuộc tính toán học của kết quả đầu ra thay vì yêu cầu một cặp nhân tố cụ thể. Các xác nhận cho các mẫu chấp nhận các kích thước chính xác được hiển thị trong câu lệnh đồng thời kiểm tra xem các kích thước được trả về có nhân với khu vực được yêu cầu hay không.```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    h = int(n ** 0.5)

    while h >= 1:
        if n % h == 0:
            print(h, n // h)
            break
        h -= 1

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

def check(inp: str, expected_area: int):
    out = run(inp)
    a, b = map(int, out.split())
    assert a * b == expected_area, (
        f"Invalid rectangle for area {expected_area}: {a} {b}"
    )
    assert a >= 1 and b >= 1

# Provided samples, whose input lines are omitted in the supplied statement.
assert run("20\n") == "4 5", "sample 1"
assert run("16\n") == "4 4", "sample 2"
assert run("6\n") == "2 3", "sample 3"

# Minimum-size input.
assert run("1\n") == "1 1", "minimum area"

# Maximum-size input.
check("200\n", 200)

# Another perfect square.
assert run("25\n") == "5 5", "perfect square"

# Prime number, where only 1 and n are factors.
check("197\n", 197)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1 1`| Diện tích tối thiểu và ranh giới vòng lặp tại 1 | 
|`200`|`10 20`| Ràng buộc tối đa và hỗn hợp không vuông | 
|`25`|`5 5`| Xử lý vuông hoàn hảo | 
|`197`|`1 197`| Đầu vào chính và dự phòng cho yếu tố 1 | 

## Vỏ cạnh 

cho`n = 1`,`floor(sqrt(1))`là 1. Kiểm tra tính chia hết đầu tiên là`1 % 1 == 0`, do đó thuật toán in`1 1`. Không cần trường hợp đặc biệt riêng biệt nào và ranh giới vòng lặp dưới được xử lý chính xác. 

Để có hình vuông hoàn hảo`n = 16`, vòng lặp bắt đầu lúc 4. Vì`16 % 4 == 0`, nó in`4 4`. Cho phép các kích thước bằng nhau vì hình chữ nhật không yêu cầu chiều cao và chiều rộng khác nhau. 

Đối với đầu vào chính`n = 7`,`floor(sqrt(7))`là 2. Thuật toán kiểm tra`7 % 2`, khác 0 thì đạt tới 1. Vì`7 % 1 == 0`, nó in`1 7`. Điều này có hiệu quả vì mọi số nguyên dương đều có ước số là 1. 

Để có đầu vào tối đa`n = 200`, vòng lặp bắt đầu từ 14 và kiểm tra hướng xuống. Ước số đầu tiên gặp phải là 10, vì`200 % 10 == 0`. Chiều bổ sung là`200 // 10 = 20`, vì vậy đầu ra là`10 20`. Sản phẩm của họ có đúng 200, thỏa mãn diện tích hình chữ nhật yêu cầu.
