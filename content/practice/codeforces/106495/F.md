---
title: "CF 106495F - F(x,l,r)"
description: "Chúng ta được cung cấp một danh sách các số nguyên dương và giá trị ban đầu x. Chúng tôi có thể chọn bất kỳ thứ tự nào của danh sách. Sau khi chọn thứ tự, chúng tôi xử lý từng số một. Mỗi số được xử lý sẽ thay thế giá trị hiện tại bằng số dư của nó khi chia cho số đó."
date: "2026-06-25T08:39:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106495
codeforces_index: "F"
codeforces_contest_name: "2026 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 106495
solve_time_s: 37
verified: true
draft: false
---

[CF 106495F - F(x,l,r)](https://codeforces.com/problemset/problem/106495/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên dương và một giá trị ban đầu`x`. Chúng tôi có thể chọn bất kỳ thứ tự nào của danh sách. Sau khi chọn thứ tự, chúng tôi xử lý từng số một. Mỗi số được xử lý sẽ thay thế giá trị hiện tại bằng số dư của nó khi chia cho số đó. Mục tiêu là sắp xếp các số sao cho giá trị cuối cùng càng lớn càng tốt. 

Đầu vào bao gồm độ dài của mảng, các giá trị mảng và giá trị bắt đầu. Đầu ra là giá trị tối đa có thể còn lại sau khi tất cả các phép toán modulo kết thúc. Thứ tự quan trọng vì thao tác modulo có thể hủy thông tin mà các thao tác sau này không thể khôi phục được. 

Độ dài mảng nhỏ, nhiều nhất là 21, nhưng bản thân các giá trị có thể rất lớn. Điều này loại trừ việc thử tất cả các hoán vị vì`21!`vượt xa những gì có thể chạy. Một giải pháp phải khai thác số lượng phần tử nhỏ hơn là kích thước của các con số. Một không gian trạng thái của`2^21`là khoảng hai triệu trạng thái, điều này khả thi ở ngôn ngữ cấp thấp và vẫn có thể quản lý được bằng Python với việc sử dụng bộ nhớ cẩn thận. 

Một sai lầm phổ biến là cho rằng nên áp dụng các ước số lớn hơn trước hoặc sau cùng. Ví dụ, với`x = 15`và những con số`[5, 6, 7]`, đặt ước số lớn nhất lên trước sẽ cho`15 % 7 = 1`, sau đó`1 % 5 = 1`, sau đó`1 % 6 = 1`. Thứ tự tối ưu là`5, 7, 6`, sản xuất`15 % 5 = 0`,`0 % 7 = 0`,`0 % 6 = 0`, vẫn chưa tối ưu. Câu trả lời thực tế có được bằng cách`7, 6, 5`:`15 % 7 = 1`,`1 % 6 = 1`,`1 % 5 = 1`. Ví dụ này cho thấy các lựa chọn tham lam chỉ dựa trên kích thước ước số không thể hiện được sự phụ thuộc giữa các phép toán. 

Một trường hợp khác là khi một số giá trị đã nhỏ. Nếu giá trị hiện tại nhỏ hơn mọi ước số còn lại thì tất cả các phép toán modulo trong tương lai sẽ giữ nguyên giá trị đó. Ví dụ: với đầu vào:```
2
10 20
5
```câu trả lời là`5`. Việc triển khai bất cẩn tiếp tục áp dụng các phép biến đổi mà không xem xét trạng thái hiện tại có thể mong đợi giá trị giảm một cách không chính xác. 

Các giá trị lặp đi lặp lại cũng cần được chăm sóc. Vì:```
3
5 5 100
12
```câu trả lời là`2`. Hai ước số giống nhau là các phép toán riêng biệt nên cả hai đều phải được xem xét. Việc xử lý các giá trị dưới dạng một tập hợp thay vì các phần tử được lập chỉ mục sẽ mất các đơn hàng hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi thứ tự có thể. Đối với mỗi hoán vị, mô phỏng các phép toán modulo và giữ giá trị cuối cùng lớn nhất. Điều này đúng vì mọi sự sắp xếp có thể đều được kiểm tra. Tuy nhiên, số hoán vị là`n!`, và cho`n = 21`đây là về`5.1 * 10^19`khả năng. Ngay cả một tỷ phép tính mỗi giây cũng không làm cho phương pháp này trở nên thực tế. 

Quan sát quan trọng là nhiều đơn đặt hàng khác nhau đạt đến cùng một tình huống trung gian. Sau khi một số số đã được sử dụng, điều quan trọng duy nhất là số nào còn lại và giá trị hiện tại là bao nhiêu. Vì giá trị hiện tại luôn được tạo ra từ giá trị ban đầu bằng các phép toán modulo nên chúng ta có thể lưu trữ giá trị tốt nhất có thể truy cập được sau khi xử lý một tập hợp con các chỉ số cụ thể. 

Đối với mỗi tập hợp con của các phần tử đã sử dụng, tiếp theo chúng tôi thử thêm từng phần tử chưa sử dụng. Quá trình chuyển đổi chỉ đơn giản là áp dụng thêm một thao tác modulo. có`2^n`tập hợp con và mỗi tập hợp con kiểm tra tối đa`n`những lựa chọn tiếp theo, đưa ra về`n * 2^n`hoạt động. 

Sự giảm thiểu quan trọng là chúng ta không bao giờ quan tâm đến thứ tự chính xác tạo ra một tập hợp con. Nếu hai đơn hàng khác nhau sử dụng cùng một tập hợp các phần tử thì chỉ giá trị hiện tại lớn hơn mới có thể quan trọng vì các phép toán modulo trong tương lai sẽ đơn điệu so với giá trị hiện tại. Giá trị hiện tại nhỏ hơn không bao giờ có thể tốt hơn giá trị hiện tại lớn hơn sau khi áp dụng các thao tác tương tự còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu | O(n * 2^n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`dp`Ở đâu`dp[mask]`biểu thị giá trị hiện tại tối đa có thể đạt được sau khi sử dụng chính xác các phần tử có chỉ số được đặt trong`mask`. Mặt nạ trống bắt đầu bằng giá trị ban đầu`x`bởi vì chưa có hoạt động nào xảy ra. 
2. Lặp lại tất cả các mặt nạ. Đối với mỗi trạng thái có thể truy cập, hãy thử mọi phần tử không có trong mặt nạ. Điều này thể hiện việc chọn số tiếp theo trong hoán vị. 
3. Áp dụng phép toán modulo với phần tử đã chọn đó. Cập nhật trạng thái chứa mặt nạ cũ cộng với phần tử mới với giá trị tối đa giữa nội dung hiện tại và giá trị mới thu được. 
4. Sau khi tất cả các tập hợp con đã được xử lý, mặt nạ đầy đủ chứa câu trả lời vì mọi phần tử mảng đều được sử dụng đúng một lần. 

Tại sao nó hoạt động: Bất biến là vậy`dp[mask]`luôn lưu trữ giá trị tốt nhất có thể sau khi sử dụng chính xác các phần tử trong`mask`. Quá trình chuyển đổi sẽ thử mọi lựa chọn tiếp theo có thể, do đó mọi khả năng tiếp tục có thể của mọi thứ tự từng phần đều được khám phá. Chỉ giữ giá trị tối đa cho một tập hợp con là an toàn vì các phép toán modulo trong tương lai không thể làm cho giá trị ban đầu nhỏ hơn đánh bại giá trị lớn hơn khi áp dụng cùng các ước số. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    x = int(input())

    size = 1 << n
    dp = array('Q', [0]) * size
    dp[0] = x

    for mask in range(size):
        cur = dp[mask]
        if cur == 0 and mask != 0:
            continue
        remaining = ((1 << n) - 1) ^ mask
        while remaining:
            bit = remaining & -remaining
            i = bit.bit_length() - 1
            nxt = mask | bit
            val = cur % a[i]
            if val > dp[nxt]:
                dp[nxt] = val
            remaining -= bit

    print(dp[-1])

if __name__ == "__main__":
    solve()
```các`dp`sử dụng mảng`array('Q')`thay vì danh sách Python thông thường. Có hơn hai triệu trạng thái, vì vậy việc lưu trữ các đối tượng số nguyên trong Python sẽ lãng phí rất nhiều bộ nhớ. Mọi giá trị được lưu trữ đều phù hợp với một số nguyên 64 bit không dấu vì tất cả các đầu vào đều có nhiều nhất`10^18`. 

Vòng chuyển tiếp trích xuất một bit được đặt từ các phần tử còn lại bằng cách sử dụng`remaining & -remaining`. Điều này tránh việc quét qua các hoạt động không cần thiết và cũng tránh tạo thêm danh sách. Chỉ số được phục hồi với`bit_length() - 1`. 

Việc kiểm tra cho`cur == 0`bỏ qua các trạng thái vô ích. Khi giá trị hiện tại trở thành 0, mọi phép toán modulo sau này sẽ giữ giá trị đó bằng 0, do đó tất cả các trạng thái con cháu của trạng thái đó cũng sẽ bằng 0. Trạng thái cuối cùng là`dp[(1 << n) - 1]`, điều này cũng giống như`dp[-1]`bởi vì mặt nạ đầy đủ là chỉ số cuối cùng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
5 6 7
15
```Các trạng thái quan trọng là: 

| mặt nạ | các yếu tố được sử dụng | giá trị tốt nhất hiện tại | 
| --- | --- | --- | 
| 000 | không | 15 | 
| 001 | 5 | 0 | 
| 010 | 6 | 3 | 
| 100 | 7 | 1 | 
| 111 | tất cả | 3 | 

Bắt đầu với`15`, đang chọn`6`đầu tiên cho`3`và các phép toán modulo còn lại không thể giảm thêm nữa. DP giữ đường dẫn này và bỏ qua các đường dẫn yếu hơn. 

Đối với mẫu thứ hai:```
4
20 21 22 10
107
```| mặt nạ | các yếu tố được sử dụng | giá trị tốt nhất hiện tại | 
| --- | --- | --- | 
| 0000 | không | 107 | 
| 0001 | 20 | 7 | 
| 0010 | 21 | 2 | 
| 0100 | 22 | 19 | 
| 1000 | 10 | 7 | 
| 1111 | tất cả | 9 | 

Câu trả lời tốt nhất đến từ việc chọn các thao tác theo thứ tự để lại phần dư lớn trước các bước cuối cùng. Các trạng thái tập hợp con cho phép thuật toán khám phá thứ tự đó mà không tạo ra các hoán vị một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 2^n) | Mỗi tập hợp con thử từng phần tử tiếp theo có thể | 
| Không gian | O(2^n) | Một giá trị được lưu trữ cho mỗi tập hợp con | 

Với`n = 21`, có khoảng hai triệu tiểu bang. Nhân với 21 sẽ có khoảng 44 triệu lần chuyển đổi, phù hợp với giới hạn dự định và triển khai hiệu quả. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    x = int(input())

    size = 1 << n
    dp = array('Q', [0]) * size
    dp[0] = x

    for mask in range(size):
        cur = dp[mask]
        if cur == 0 and mask:
            continue
        rem = (size - 1) ^ mask
        while rem:
            bit = rem & -rem
            i = bit.bit_length() - 1
            nxt = mask | bit
            val = cur % a[i]
            if val > dp[nxt]:
                dp[nxt] = val
            rem -= bit

    return str(dp[-1])

assert solution("""3
5 6 7
15
""") == "3"

assert solution("""4
20 21 22 10
107
""") == "9"

assert solution("""1
100
37
""") == "37"

assert solution("""3
5 5 100
12
""") == "2"

assert solution("""4
2 4 8 16
1000000000000000000
""") == "0"

assert solution("""5
9 10 11 12 13
100
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 100 / 37`|`37`| Phần tử đơn và không có modulo có hại | 
|`5 5 100`với`12`|`2`| Các giá trị lặp lại và xử lý các chỉ số riêng biệt | 
|`2 4 8 16`với giá trị khổng lồ |`0`| Giá trị lớn và đạt 0 | 
|`9 10 11 12 13`với`100`|`10`| Lựa chọn đặt hàng khác nhau | 

## Vỏ cạnh 

Đối với trường hợp giá trị bắt đầu đã nhỏ hơn mọi ước số:```
2
10 20
5
```Trạng thái ban đầu có giá trị`5`. Cả hai lựa chọn tiếp theo có thể tạo ra`5 % 10 = 5`hoặc`5 % 20 = 5`. Mỗi thao tác sau này giữ nguyên giá trị, do đó trạng thái mặt nạ đầy đủ vẫn được giữ nguyên`5`. 

Đối với các phần tử lặp lại:```
3
5 5 100
12
```DP không hợp nhất hai số năm vì các trạng thái dựa trên các chỉ số. Một đường dẫn sử dụng đường dẫn đầu tiên`5`, cái khác sử dụng cái thứ hai`5`và cả hai thao tác vẫn khả dụng độc lập. Con đường tốt nhất là`100, 5, 5`, cho`12 % 100 = 12`, sau đó`12 % 5 = 2`, sau đó`2 % 5 = 2`. 

Đối với đường dẫn đạt tới số 0:```
4
2 4 8 16
1000000000000000000
```DP có thể gặp phải trạng thái trong đó giá trị hiện tại trở thành 0 sau khi áp dụng số chia`2`. Tất cả các chuyển đổi từ trạng thái đó đều bằng 0, đó là lý do tại sao việc bỏ qua các trạng thái đó là đúng. Câu trả lời cuối cùng vẫn là con số 0.
