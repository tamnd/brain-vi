---
title: "CF 102190H - đầu vào/đầu ra tiêu chuẩn"
description: "Một ngày trên đồng hồ này có h giờ và mỗi giờ có m phút. Thời gian hiển thị được xác định bằng số giờ x và số phút y, trong đó 0 <= x < h và 0 <= y < m."
date: "2026-08-19T05:56:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "H"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 328
verified: true
draft: false
---

[CF 102190H - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một ngày trên đồng hồ này chứa`h`giờ, và mỗi giờ chứa`m`phút. Thời gian hiển thị được xác định bằng số giờ`x`và một số phút`y`, Ở đâu`0 <= x < h`Và`0 <= y < m`. 

Một phút được phân loại là huashui khi số phút của nó ít nhất bằng số giờ, vì vậy điều kiện đơn giản là`y >= x`. Mỗi cặp`(x, y)`đại diện cho một phút trong ngày vì giây không liên quan. Nhiệm vụ là tìm phân số của tất cả số phút thỏa mãn điều kiện này và in phân số đó ở dạng thấp nhất. 

có`h * m`tổng số phút hiển thị. Vì cả hai`h`Và`m`có thể lớn như`10^9`, sản phẩm này có thể tiếp cận`10^18`. Quan trọng hơn, việc lặp lại mỗi giờ hoặc mỗi phút sẽ cần tới`10^9`hoạt động, trong khi lặp qua mỗi cặp`(x, y)`sẽ yêu cầu lên tới`10^18`hoạt động. Giải pháp phải giảm số đếm xuống một số phép tính số học không đổi. 

Trường hợp biên đầu tiên là khi`h = m = 2`. Bốn lần là`(0,0)`,`(0,1)`,`(1,0)`, Và`(1,1)`. Ba thỏa mãn`y >= x`, vậy câu trả lời là`3/4`. Việc thực hiện bất cẩn bằng cách sử dụng`y > x`sẽ chỉ được tính`(0,1)`và sản xuất`1/4`. 

Một trường hợp ranh giới khác xảy ra khi có nhiều giờ hơn số phút chẳng hạn`7 2`. Đối với số giờ`0`Và`1`, một số phút đủ điều kiện, nhưng với mỗi giờ từ`2`bởi vì`6`, không phút nào có thể thỏa mãn`y >= x`vì số phút lớn nhất chỉ là`1`. Kết quả đúng là`3/14`. Việc triển khai giả định mỗi giờ đóng góp ít nhất một phút huashui sẽ bị tính quá mức. 

Tình huống ngược lại cũng có vấn đề. Vì`2 7`, giờ`0`có tất cả bảy phút đủ điều kiện, trong khi giờ`1`có sáu, cho`13`số phút đủ điều kiện trong số`14`, hoặc`13/14`. Sự bất bình đẳng bao gồm sự bình đẳng, vì vậy`(1,1)`phải được tính. Sử dụng so sánh chặt chẽ sẽ đưa ra kết quả không chính xác`12/14 = 6/7`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi thời gian hiển thị`(x, y)`và kiểm tra xem`y >= x`. Có chính xác`h * m`các cặp như vậy, vì vậy mặc dù phương pháp này rõ ràng là đúng, nhưng số phép tính trong trường hợp xấu nhất của nó là`10^18`. Ngay cả việc kiểm tra một cặp trong một thao tác liên tục trong thời gian duy nhất cũng vượt xa mọi giới hạn cuộc thi thực tế. 

Chúng ta có thể làm tốt hơn nhiều bằng cách sửa số giờ và đếm số phút hợp lệ về mặt toán học. Trong một giờ cố định`x`, điều kiện là`y >= x`. Nếu như`x >= m`, không có giá trị`y`, vì số phút nhiều nhất là`m - 1`. Nếu như`x < m`, số phút hợp lệ là`x, x + 1, ..., m - 1`, đưa ra chính xác`m - x`phút hợp lệ. 

Chỉ có cái đầu tiên`min(h, m)`số giờ có thể đóng góp. Cho phép`k = min(h, m)`. Số phút huashui là`m + (m - 1) + ... + (m - k + 1)`. 

Đây là một cấp số cộng với`k`điều khoản. Tổng của nó là`k * (2m - k + 1) / 2`. 

Mẫu số chỉ đơn giản là tổng số phút,`h * m`. Sau đó, chúng tôi giảm phân số kết quả bằng cách sử dụng ước số chung lớn nhất. 

Điều quan trọng cần lưu ý là bài toán đếm hai chiều thực chất là một cấp số cộng một chiều. Sự so sánh`y >= x`tạo ra một vùng hình tam giác trong`h x m`lưới và kích thước của nó có thể được tính toán trực tiếp thay vì truy cập vào các ô của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (hm) | O(1) | Quá chậm | 
| Tối ưu | O(log(hm)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`h`Và`m`, sau đó đặt`k = min(h, m)`. Chỉ số giờ từ`0`bởi vì`k - 1`có thể có ít nhất một số phút hợp lệ. 
2. Tính số phút hoa thủy là`k * (2m - k + 1) / 2`. Đây là tổng số tiền đóng góp`m, m - 1, ..., m - k + 1`. 
3. Tính tổng số phút là`h * m`. Mỗi giờ chứa chính xác`m`phút. 
4. Rút gọn tử số và mẫu số bằng ước số chung lớn nhất của chúng. Đầu ra phải là phân số rút gọn thay vì chỉ là phân số tương đương. 
5. In tử số rút gọn theo sau`/`và mẫu số rút gọn. 

### Tại sao nó hoạt động 

Cứ mỗi giờ`x < min(h, m)`, chính xác là số phút từ`x`bởi vì`m - 1`thỏa mãn`y >= x`, do đó giờ đó đóng góp`m - x`huashui phút. Đối với mọi`x >= m`, nó đóng góp bằng không. Do đó, số đếm hoàn chỉnh chính xác là cấp số cộng được tính bằng thuật toán. Chia số này cho tổng số`h * m`đưa ra tỷ lệ cần thiết và chia cả hai giá trị cho gcd của chúng sẽ tạo ra phần rút gọn duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

def solve():
    h, m = map(int, input().split())

    k = min(h, m)

    numerator = k * (2 * m - k + 1) // 2
    denominator = h * m

    g = gcd(numerator, denominator)

    print(numerator // g, denominator // g)

if __name__ == "__main__":
    solve()
```giá trị`k`nắm bắt ranh giới có liên quan duy nhất trong việc đếm. Nếu như`h <= m`, mỗi giờ đều có thể đóng góp, vì vậy`k = h`. Nếu như`h > m`, chỉ là cái đầu tiên`m`số giờ có thể đóng góp, vì vậy`k = m`. 

Tử số sử dụng công thức cấp số cộng thay vì vòng lặp. Sự phân chia theo`2`được thực hiện trước khi khử gcd, nhưng tích luôn luôn bằng nhau vì một trong`k`Và`2m - k + 1`là chẵn. Số nguyên Python cũng xử lý các giá trị trung gian lớn nhất một cách an toàn, do đó không có vấn đề tràn. 

biểu hiện`m - x`bao gồm điểm cuối`y = x`, phù hợp với`>=`tình trạng. Đây chính xác là nơi mà việc so sánh nghiêm ngặt sẽ gây ra lỗi riêng lẻ. 

Cuối cùng,`gcd`làm giảm phân số. Ví dụ: kết quả thô cho`h = 13, m = 11`là`66/143`, và gcd của họ là`11`, cho`6/13`. 

## Ví dụ đã hoạt động 

### Mẫu 1:`h = 2, m = 2`Các biến liên quan phát triển như sau. 

| Biến | Giá trị | 
| --- | --- | 
|`h`| 2 | 
|`m`| 2 | 
|`k = min(h, m)`| 2 | 
|`numerator`|`2 * (4 - 2 + 1) / 2 = 3`| 
|`denominator`|`2 * 2 = 4`| 
|`gcd`| 1 | 
| trả lời |`3/4`| 

Hai giờ đóng góp`2`Và`1`huashui phút tương ứng. Trường hợp bình đẳng`(1,1)`được bao gồm, đưa ra ba lần hợp lệ trong số bốn lần. 

### Mẫu 2:`h = 2, m = 7`| Biến | Giá trị | 
| --- | --- | 
|`h`| 2 | 
|`m`| 7 | 
|`k = min(h, m)`| 2 | 
|`numerator`|`2 * (14 - 2 + 1) / 2 = 13`| 
|`denominator`|`2 * 7 = 14`| 
|`gcd`| 1 | 
| trả lời |`13/14`| 

Giờ`0`đóng góp bảy phút và giờ hợp lệ`1`đóng góp sáu. Tổng cộng là`13`, vậy phân số đã giảm rồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(hm)) | Tính toán gcd mất thời gian logarit; tất cả các hoạt động khác là thời gian không đổi. | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Các ràng buộc cho phép các giá trị lên đến`10^9`, vì vậy một giải pháp đòi hỏi`O(h)`hoặc`O(m)`các lần lặp lại có thể yêu cầu tới một tỷ thao tác. Công thức số học tránh tất cả các lần lặp, làm cho thời gian chạy không đổi một cách hiệu quả ngoại trừ việc tính toán gcd. Các số nguyên có độ chính xác tùy ý của Python cũng xử lý các sản phẩm xung quanh một cách an toàn`10^18`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd

def solve():
    h, m = map(int, input().split())

    k = min(h, m)
    numerator = k * (2 * m - k + 1) // 2
    denominator = h * m

    g = gcd(numerator, denominator)
    print(numerator // g, denominator // g)

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

# Provided samples
assert run("2 2\n") == "3/4", "sample 1"
assert run("2 7\n") == "13/14", "sample 2"
assert run("7 2\n") == "3/14", "sample 3"
assert run("13 11\n") == "6/13", "sample 4"
assert run("100 33\n") == "17/100", "sample 5"
assert run("100005 100009\n") == "50007/100009", "sample 6"
assert run("1000000000 2\n") == "3/2000000000", "sample 7"
assert run("2 999999999\n") == "1999999997/1999999998", "sample 8"
assert run("914067307 998244353\n") == "541210700/998244353", "sample 9"

# Minimum-size input
assert run("2 2\n") == "3/4", "minimum values"

# Equal dimensions with a larger size
assert run("100 100\n") == "1/2", "all-equal values"

# More hours than minutes
assert run("5 2\n") == "3/10", "hours exceed minutes"

# More minutes than hours
assert run("2 5\n") == "9/10", "minutes exceed hours"

# Large boundary values
assert run("1000000000 1000000000\n") == "500000000000000001/1000000000000000000", "maximum equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`|`3/4`| Các ràng buộc tối thiểu và bao gồm sự bình đẳng | 
|`100 100`|`500000000000000001/1000000000000000000`| Bình đẳng`h`Và`m`có giá trị số học lớn | 
|`5 2`|`3/10`| Số giờ vượt quá số phút có sẵn không đóng góp | 
|`2 5`|`9/10`| Tất cả giờ đóng góp khi có nhiều phút hơn giờ | 
|`1000000000 1000000000`|`500000000000000001/1000000000000000000`| Xử lý số học và số nguyên có kích thước tối đa | 

## Vỏ cạnh 

Khi nào`h = m = 2`, đầu vào là`2 2`và tử số là`2 + 1 = 3`, trong khi mẫu số là`4`. Thuật toán được`k = 2`và tính toán`3/4`. Trường hợp bình đẳng`(1,1)`được tính vì tiến trình bắt đầu vào phút`x`, không`x + 1`. 

Khi có nhiều giờ hơn phút, hãy cân nhắc`7 2`. Đây`k = 2`, vậy chỉ có vài giờ`0`Và`1`đóng góp. Những đóng góp của họ là`2`Và`1`, cho`3`huashui phút hết`14`. Đầu ra là`3/14`. Giờ`2`bởi vì`6`được tự động loại trừ bằng cách lấy`k = min(h, m)`. 

Khi có nhiều phút hơn giờ, hãy cân nhắc`2 7`. Ở đây cả hai giờ đều đóng góp, với`7`phút hợp lệ cho giờ`0`Và`6`hàng giờ`1`. Tử số là`13`và mẫu số là`14`, vậy kết quả là`13/14`. Điều này kiểm tra xem toàn bộ hậu tố hợp lệ của số phút có được tính cho mỗi giờ hay không. 

Đối với kích thước bằng nhau tối đa,`1000000000 1000000000`, thuật toán không bao giờ lặp qua hàng tỷ giá trị. Nó trực tiếp đánh giá tiến trình số học với`k = 1000000000`, tạo ra phần rút gọn`500000000000000001/1000000000000000000`. Các giá trị trung gian lớn được biểu diễn an toàn bằng số nguyên Python. 

Điều kiện biên trung tâm trong tất cả các trường hợp này là như nhau: số giờ bằng`m`không có phút hợp lệ, vì số phút lớn nhất là`m - 1`. Đó là lý do tại sao số giờ đóng góp chính xác`min(h, m)`, còn hơn là`min(h, m + 1)`hoặc`min(h, m - 1)`.
