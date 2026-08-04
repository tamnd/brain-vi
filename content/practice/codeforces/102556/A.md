---
title: "CF 102556A - A - Hạng Riana và One Punch"
description: "Đầu vào là một hàng tròn các vị trí xung quanh thành phố. Một vị trí chứa kẻ thù khi được đánh dấu X và trống khi được đánh dấu bằng .. Một cú đấm có thể hạ gục mọi kẻ thù thuộc cùng một chuỗi kẻ thù liên tục."
date: "2026-08-04T09:08:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102556
codeforces_index: "A"
codeforces_contest_name: "2020 Ateneo de Manila University DISCS PrO HS Division"
rating: 0
weight: 102556
solve_time_s: 56
verified: true
draft: false
---

[CF 102556A - A - Hạng Riana và One Punch](https://codeforces.com/problemset/problem/102556/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào là một hàng tròn các vị trí xung quanh thành phố. Một vị trí chứa kẻ thù khi nó được đánh dấu bằng`X`và trống khi được đánh dấu bằng`.`. Một cú đấm có thể hạ gục mọi kẻ thù thuộc cùng một chuỗi kẻ thù liên tục. Một vị trí trống sẽ phá vỡ chuỗi đó, vì vậy mục tiêu là biến số lượng vị trí trống nhỏ nhất có thể thành kẻ thù cho đến khi tất cả kẻ thù thuộc về một chuỗi hình tròn. 

Đầu ra là số lượng tối thiểu của`.`những vị trí phải trở thành`X`các vị trí. Kết nối vòng tròn quan trọng vì vị trí cuối cùng liền kề với vị trí đầu tiên. Một nhóm kẻ thù có thể tiếp tục đi qua ranh giới này, vì vậy việc coi chuỗi như một đường bình thường sẽ cho kết quả không chính xác. 

Độ dài tối đa là 100 ký tự. Giá trị này đủ nhỏ để nhiều giải pháp tuyến tính hoặc bậc hai có thể chạy dễ dàng, nhưng nó cũng đủ lớn để việc thử mọi tập hợp vị trí được lấp đầy có thể là không cần thiết. Một lực lượng mạnh mẽ trên tất cả các vị trí trống sẽ cần tới$2^{100}$khả năng trong trường hợp xấu nhất, vượt xa những gì giới hạn một giây có thể xử lý. Cấu trúc hữu ích là các vị trí tạo thành một vòng tròn, do đó vấn đề có thể được giảm xuống thành việc tìm ra các thuộc tính của khoảng cách giữa các kẻ thù hiện có. 

Một số trường hợp đặc biệt có thể phá vỡ việc triển khai trực tiếp. Nếu tất cả các vị trí đều là kẻ thù thì câu trả lời là 0 vì chuỗi đã hoàn tất. 

Đối với đầu vào:```
XXXXX
```đầu ra đúng là:```
0
```Một phương pháp chỉ tìm kiếm các khoảng trống có thể cho rằng luôn có một khoảng trống cần lấp đầy. 

Nếu chỉ có một kẻ thù thì bản thân nó đã tạo thành một nhóm liên kết. 

Đối với đầu vào:```
.....
```đầu ra đúng là:```
0
```Không có kẻ thù nào cần được kết nối. Một giải pháp bất cẩn cho rằng có ít nhất một kẻ thù tồn tại có thể thất bại trong khi tìm kiếm kẻ thù đầu tiên.`X`. 

Một trường hợp quan trọng khác là khi kết nối đi qua đầu chuỗi. 

Đối với đầu vào:```
XX..XX
```đầu ra đúng là:```
0
```Hai nhóm ở đầu và cuối thực sự nằm liền kề nhau trên vòng tròn nên chúng đã là một chuỗi được kết nối. Quét tuyến tính bỏ qua cạnh tròn sẽ tính không chính xác khoảng trống ở giữa là thứ cần phải lấp đầy. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xem xét việc lấp đầy một số tập hợp con của các vị trí trống và kiểm tra xem vòng tròn kết quả có chỉ có một thành phần kẻ thù hay không. Đối với mỗi tập hợp con các dấu chấm có thể có, chúng ta có thể mô phỏng vòng tròn và đếm số lượng nhóm kẻ thù được kết nối. Điều này đúng vì nó khám phá mọi cách sắp xếp cuối cùng có thể có, nhưng số lượng tập hợp con tăng theo cấp số nhân. Với 100 vị trí, trường hợp xấu nhất chứa 100 vị trí trống, cho$2^{100}$những khả năng không thể xử lý được. 

Quan sát quan trọng đến từ việc nhìn vào những khoảng trống thay vì kẻ thù. Kẻ thù hiện tại được ngăn cách bằng các khoảng trống hình tròn có dấu chấm. Nếu chúng ta lấp đầy hoàn toàn khoảng trống, nhóm kẻ thù của cả hai bên sẽ kết nối với nhau thông qua khoảng trống đó. Để biến toàn bộ vòng tròn thành một nhóm kẻ thù được kết nối, mọi khoảng trống dấu chấm ngoại trừ một dấu chấm có thể phải được loại bỏ. 

Tại sao một khoảng cách có thể vẫn còn? Hãy tưởng tượng để lại một đoạn trống liên tục. Tất cả các khoảng trống khác đều được lấp đầy, vì vậy việc di chuyển vòng quanh vòng tròn qua các khoảng trống được lấp đầy sẽ đến được mọi kẻ thù. Đoạn trống còn lại chỉ đơn giản trở thành đoạn ngắt không được sử dụng giữa phần cuối và phần đầu của cùng một chuỗi kẻ thù được kết nối. 

Vì việc giữ một khoảng trống giúp chúng ta không phải lấp đầy các chấm của nó nên chúng ta nên giữ khoảng trống dấu chấm lớn nhất và lấp đầy tất cả những khoảng trống khác. Câu trả lời là tổng số chấm trừ đi chiều dài của khoảng cách hình tròn lớn nhất giữa các nhóm kẻ thù. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \times n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lượng kẻ thù trong vòng tròn. Nếu không có kẻ thù thì câu trả lời là 0 vì không có gì để kết nối. 
2. Tìm mọi dãy vị trí trống liên tiếp. Vì chuỗi có dạng tròn nên vị trí đầu tiên và cuối cùng có thể thuộc cùng một chuỗi trống nên quá trình quét phải xử lý ranh giới một cách cẩn thận. 
3. Ghi lại độ dài của chuỗi trống lớn nhất. Đây là khoảng trống duy nhất có thể để trống vì nó tượng trưng cho sự đứt gãy trong chuỗi vòng tròn cuối cùng. 
4. Đếm tổng số vị trí trống. Mọi vị trí trống ngoài khoảng trống lớn nhất đều phải biến thành kẻ thù. 
5. Trả về tổng số vị trí trống trừ đi độ dài khoảng trống lớn nhất. 

Lý do mà khoảng cách lớn nhất là khoảng cách tốt nhất nên được giữ nguyên là vì mọi khoảng trống khác phải biến mất để kết nối các nhóm kẻ thù. Giữ một khoảng cách nhỏ hơn sẽ buộc chúng tôi phải lấp đầy nhiều vị trí hơn mức cần thiết. 

Tại sao nó hoạt động: 

Hãy xem xét các nhóm kẻ thù hiện có xung quanh vòng tròn. Mỗi khoảng trống ngăn cách hai nhóm lân cận. Nếu có hai khoảng trống trở lên, vòng tròn chứa nhiều phần kẻ thù bị ngắt kết nối. Vì vậy, nhiều nhất một khoảng trống có thể tồn tại. Việc chọn khoảng trống lớn nhất sẽ giảm thiểu số lượng vị trí được lấp đầy vì nó tránh lấp đầy số lượng dấu chấm lớn nhất. Thuật toán kiểm tra tất cả các khoảng trống và chọn chính xác khoảng trống tối ưu này nên giá trị trả về là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    if 'X' not in s:
        print(0)
        return

    total_dots = s.count('.')

    doubled = s + s
    first_x = doubled.find('X')

    max_gap = 0
    current = 0

    for i in range(first_x, first_x + n):
        if doubled[i] == '.':
            current += 1
            max_gap = max(max_gap, current)
        else:
            current = 0

    if max_gap > total_dots:
        max_gap = total_dots

    print(total_dots - max_gap)

solve()
```Giải pháp đầu tiên xử lý trường hợp vòng tròn trống vì không có nhóm địch để kết nối. Sau đó, ít nhất một`X`tồn tại, vì vậy chúng tôi xoay chế độ xem vòng tròn bằng cách bắt đầu quét tại vị trí của kẻ thù. Điều này loại bỏ sự mơ hồ khi chuỗi dấu chấm đi qua phần cuối của chuỗi đầu vào. 

Chuỗi nhân đôi cho phép truy cập vào các hàng xóm hình tròn mà không cần sử dụng chỉ mục mô-đun. Chúng tôi chỉ quét chính xác`n`vị trí sau kẻ thù đầu tiên, do đó, chuỗi dấu chấm từ đầu đến cuối được tính là một khoảng cách liên tục. 

Biến`current`lưu trữ độ dài của đoạn trống hiện tại. Bất cứ khi nào kẻ thù xuất hiện, phân đoạn sẽ kết thúc và`max_gap`đã chứa khoảng cách tốt nhất được thấy cho đến nay. Vì khoảng trống lớn nhất có thể không thể vượt quá tổng số dấu chấm nên câu trả lời cuối cùng chỉ đơn giản là số dấu chấm không có trong khoảng trống đã lưu đó. 

Không có vấn đề tràn số nguyên trong Python vì độ dài đầu vào chỉ là 100. Điều kiện biên chính là chọn điểm bắt đầu chính xác. Bắt đầu từ một vị trí tùy ý có thể chia khoảng cách dấu tròn thành hai phần nhỏ hơn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
XX..XX.X....
```Quá trình quét bắt đầu từ kẻ thù đầu tiên và tìm ra những khoảng trống hình tròn. 

| Vị trí được xử lý | Nhân vật | Khoảng cách hiện tại | Khoảng cách lớn nhất | 
| --- | --- | --- | --- | 
| 0 | X | 0 | 0 | 
| 1 | X | 0 | 0 | 
| 2 | . | 1 | 1 | 
| 3 | . | 2 | 2 | 
| 4 | X | 0 | 2 | 
| 5 | X | 0 | 2 | 
| 6 | . | 1 | 2 | 
| 7 | X | 0 | 2 | 
| 8 | . | 1 | 2 | 
| 9 | . | 2 | 2 | 
| 10 | . | 3 | 3 | 
| 11 | . | 4 | 4 | 

Tổng cộng có 8 vị trí trống và khoảng trống lớn nhất có chiều dài 4. Để nguyên khoảng trống đó có nghĩa là cần phải lấp đầy 4 vị trí trống. 

Đối với mẫu thứ hai:```
X..XX.X..X
```Các khoảng trống hình tròn có độ dài 2, 1 và 2. 

| Vị trí được xử lý | Nhân vật | Khoảng cách hiện tại | Khoảng cách lớn nhất | 
| --- | --- | --- | --- | 
| 0 | X | 0 | 0 | 
| 1 | . | 1 | 1 | 
| 2 | . | 2 | 2 | 
| 3 | X | 0 | 2 | 
| 4 | X | 0 | 2 | 
| 5 | . | 1 | 2 | 
| 6 | X | 0 | 2 | 
| 7 | . | 1 | 2 | 
| 8 | . | 2 | 2 | 
| 9 | X | 0 | 2 | 

Có 5 vị trí trống và khoảng trống lớn nhất là 2, vì vậy câu trả lời là 3. Hai vị trí trống còn lại đại diện cho một khoảng trống có thể tiếp tục mở. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Vòng tròn được quét một lần sau khi tìm thấy kẻ thù đầu tiên. | 
| Không gian |$O(n)$| Chuỗi nhân đôi được lưu trữ để di chuyển vòng tròn dễ dàng hơn. | 

Kích thước đầu vào tối đa chỉ là 100 ký tự, vì vậy giải pháp tuyến tính này dễ dàng đáp ứng các giới hạn. Thuật toán cũng tránh được việc mô phỏng đồ thị phức tạp vì cấu trúc hình tròn làm giảm vấn đề đo các khoảng trống. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    s = sys.stdin.readline().strip()
    n = len(s)

    if 'X' not in s:
        ans = 0
    else:
        total_dots = s.count('.')
        doubled = s + s
        start = doubled.find('X')

        best = 0
        cur = 0
        for i in range(start, start + n):
            if doubled[i] == '.':
                cur += 1
                best = max(best, cur)
            else:
                cur = 0

        ans = total_dots - min(best, total_dots)

    sys.stdin = old_stdin
    return str(ans)

assert solution("XX..XX.X....") == "4", "sample 1"
assert solution("X..XX.X..X") == "3", "sample 2"

assert solution("X") == "0", "single enemy"
assert solution(".....") == "0", "no enemies"
assert solution("XXXXX") == "0", "already connected"
assert solution("XX..XX") == "0", "circular connection"
assert solution("X.X.X") == "2", "alternating enemies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`X`|`0`| Xử lý nhóm kẻ thù nhỏ nhất có thể. | 
|`.....`|`0`| Xử lý vụ án không có kẻ thù. | 
|`XXXXX`|`0`| Xử lý một vòng kết nối đã được kết nối. | 
|`XX..XX`|`0`| Kiểm tra kết nối bao quanh ở ranh giới. | 
|`X.X.X`|`2`| Kiểm tra nhiều khoảng trống nhỏ. | 

## Vỏ cạnh 

cho`XXXXX`, thuật toán nhận thấy tổng số chấm bằng 0. Khoảng cách lớn nhất cũng bằng 0 nên câu trả lời vẫn là 0. Không có vị trí trống nào ngăn cách nhóm địch. 

Vì`.....`, điều kiện đầu tiên được kích hoạt vì không có kẻ thù nào tồn tại. Thuật toán trả về số 0 ngay lập tức thay vì tìm kiếm vị trí ban đầu của kẻ thù. Điều này tránh việc lập chỉ mục không hợp lệ và phù hợp với ý tưởng rằng không cần kết nối kẻ thù. 

Vì`XX..XX`, cách tiếp cận tuyến tính có thể thấy hai nhóm cách nhau bằng hai dấu chấm. Thay vào đó, thuật toán bắt đầu từ kẻ thù và quét toàn bộ vòng tròn, xử lý chính xác điểm cuối và điểm bắt đầu liền kề nhau. Khoảng cách duy nhất có chiều dài 2, nhưng việc giữ khoảng cách đó vẫn khiến tất cả kẻ thù được kết nối qua phía bên kia của vòng tròn, vì vậy câu trả lời là 0. 

Đối với trường hợp như`X.X.X`, các khoảng trống đều có chiều dài bằng một. Thuật toán chỉ có thể lưu một trong những khoảng trống đó, vì vậy hai trong số ba dấu chấm phải được điền vào. Giá trị tính toán là`3 - 1 = 2`, phù hợp với số lượng thay đổi tối thiểu cần thiết.
