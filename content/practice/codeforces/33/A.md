---
title: "CF 33A - Bữa tối ăn gì?"
description: "Mỗi chiếc răng thuộc đúng một hàng. Khi Valerie ăn một chiếc diếc bằng một hàng, mỗi chiếc răng trong hàng đó sẽ mất đi một đơn vị khả năng sống sót. Một hàng sẽ không thể sử dụng được ngay khi có ít nhất một chiếc răng bên trong nó giảm xuống dưới 0."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 33
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 33 (Codeforces format)"
rating: 1200
weight: 33
solve_time_s: 233
verified: true
draft: false
---
[CF 33A - Bữa tối ăn gì?](https://codeforces.com/problemset/problem/33/A) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 3m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chiếc răng thuộc đúng một hàng. Khi Valerie ăn một chiếc diếc bằng một hàng, mỗi chiếc răng trong hàng đó sẽ mất đi một đơn vị khả năng sống sót. Một hàng sẽ không thể sử dụng được ngay khi có ít nhất một chiếc răng bên trong nó giảm xuống dưới 0. 

Đối với một hàng, hệ số giới hạn là răng yếu nhất trong hàng đó. Nếu một hàng chứa khả năng tồn tại của răng`[5, 8, 3]`, thì hàng đó chỉ được dùng 3 lần, vì sau lần thứ 4 dùng răng còn sống`3`sẽ trở nên tiêu cực. 

Nhiệm vụ là tính tổng số lượng cá diếc tối đa mà Valerie có thể ăn tổng cộng, đồng thời không bao giờ làm tiêu cực bất kỳ chiếc răng nào. Cô ấy có thể chọn các hàng khác nhau cho những người đóng đinh khác nhau và mỗi người đóng đinh sử dụng đúng một hàng. 

Đầu vào cung cấp cho tất cả các răng riêng lẻ. Đối với mỗi chiếc răng, chúng ta biết chỉ số hàng và khả năng tồn tại còn lại của nó. Đầu ra là một số nguyên, số lượng cá diếc tối đa cô ấy có thể ăn, nhưng không nhiều hơn khẩu phần có sẵn`k`. 

Các ràng buộc đủ nhỏ để hầu hết mọi giải pháp tuyến tính hoặc bậc hai đều hoạt động thoải mái. Có nhiều nhất là 1000 chiếc răng, vì vậy ngay cả một`O(n^2)`giải pháp thực hiện khoảng một triệu thao tác, điều này không đáng kể trong giới hạn 2 giây. Điều này có nghĩa là thách thức không phải là tối ưu hóa mà là xác định chính xác số lượng mà mỗi hàng đóng góp. 

Sai lầm dễ nhất là nghĩ rằng một hàng đóng góp tổng số khả năng tồn tại của răng của nó. Điều đó sai vì tất cả các răng trong hàng đã chọn đều giảm cùng nhau. 

Hãy xem xét đầu vào này:```
3 1 100
1 5
1 2
1 8
```Chỉ có một hàng. Sau khi sử dụng hai lần, chiếc răng thứ hai đạt đến số không. Sử dụng thêm một lần nữa sẽ khiến nó trở nên tiêu cực. Câu trả lời đúng là:```
2
```Một giải pháp bất cẩn tính tổng hàng sẽ xuất ra không chính xác`15`. 

Một trường hợp khó phát hiện khác là khi một số răng đã không còn khả năng tồn tại.```
2 2 10
1 0
2 7
```Hàng 1 không thể được sử dụng dù chỉ một lần. Hàng 2 có thể được sử dụng 7 lần. Câu trả lời là:```
7
```Nếu việc triển khai vô tình coi số 0 là có thể sử dụng được một lần, thì nó sẽ xuất ra`8`. 

Thêm một trường hợp nguy hiểm nữa xuất hiện khi tổng số bữa ăn có thể vượt quá khẩu phần ăn sẵn có`k`.```
2 2 3
1 10
2 10
```Cả hai hàng cùng nhau cho phép ăn 20 bữa, nhưng chỉ có 3 chiếc diếc. Câu trả lời đúng là:```
3
```Câu trả lời cuối cùng phải luôn được giới hạn bởi`k`. 

## Phương pháp tiếp cận 

Thật dễ dàng để tưởng tượng một mô phỏng lực lượng vũ phu. Ở mỗi bước, chọn bất kỳ hàng nào vẫn còn khả năng tồn tại của tất cả các răng không âm sau một lần sử dụng nữa, giảm từng răng trong hàng đó và đếm xem đã ăn bao nhiêu bữa trước khi tất cả các hàng không thể sử dụng được hoặc phần ăn kết thúc. 

Điều này hoạt động vì quá trình này tuân theo chính xác câu lệnh. Vấn đề là chúng ta liên tục sửa đổi nhiều chiếc răng cho mỗi con diếc ăn. Vì khả năng tồn tại của răng có thể lớn bằng`10^6`, số lượng bữa ăn mô phỏng cũng có thể đạt tới`10^6`. Trong trường hợp xấu nhất, việc quét liên tục các hàng và cập nhật răng sẽ trở nên tốn kém không cần thiết. 

Quan sát quan trọng là thứ tự ăn uống không quan trọng. Hàng hoàn toàn độc lập. Nếu một hàng dùng được 5 lần và một hàng khác dùng được 7 lần thì chúng cộng lại sẽ đóng góp đúng 12 bữa ăn có thể. 

Đối với một hàng cố định, số lượng có thể sử dụng được bằng khả năng tồn tại tối thiểu giữa các răng của nó. Mỗi lần sử dụng sẽ làm giảm tất cả các răng cùng nhau, do đó, chiếc răng yếu nhất sẽ quyết định thời điểm gãy hàng. 

Điều đó làm giảm toàn bộ vấn đề xuống: 

1. Tìm khả năng tồn tại tối thiểu trong mỗi hàng. 
2. Tính tổng các cực tiểu này. 
3. Giới hạn kết quả bằng`k`. 

Một khi quan sát này xuất hiện, việc thực hiện sẽ trở nên rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k · n) | O(n) | Về mặt khái niệm quá chậm | 
| Tối ưu | O(n + m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`mn`kích thước`m`, được khởi tạo với giá trị rất lớn.`mn[i]`sẽ lưu trữ khả năng tồn tại tối thiểu của răng bên trong hàng`i`. 
2. Đọc từng mô tả về răng. 

Mỗi chiếc răng thuộc một hàng và có giá trị tồn tại. 
3. Cập nhật số tiền tối thiểu cho hàng đó. 

Nếu một chiếc răng có khả năng tồn tại nhỏ hơn mức tối thiểu được lưu trữ hiện tại, hãy thay thế nó. 

Điều này hiệu quả vì răng yếu nhất sẽ quyết định số lần toàn bộ hàng có thể được sử dụng. 
4. Sau khi xử lý tất cả các răng, tính tổng cực tiểu của tất cả các hàng. 

Mỗi hàng đóng góp chính xác khả năng tồn tại tối thiểu của nó vào tổng số bữa ăn. 
5. Đầu ra`min(total, k)`. 

Valerie không thể ăn nhiều cá thập tự hơn khẩu phần thực sự chứa. 

### Tại sao nó hoạt động 

Đối với bất kỳ hàng nào, mỗi lần sử dụng sẽ giảm tất cả các răng trong hàng đó đi đúng một. Sau đó`t`sử dụng, một chiếc răng có khả năng tồn tại ban đầu`c`trở thành`c - t`. Để giữ cho tất cả các răng không âm, chúng ta cần`c - t >= 0`cho mỗi răng trong hàng. 

Điều đó có nghĩa là:```
t <= minimum viability in the row
```Vì vậy, hàng có thể được sử dụng chính xác bao nhiêu lần tùy theo mức độ răng yếu nhất của nó cho phép. Các hàng không can thiệp lẫn nhau vì ăn một hàng không bao giờ thay đổi răng ở hàng khác. Tổng hợp các công suất độc lập này sẽ cho tổng số bữa ăn tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())

INF = 10**18
mn = [INF] * m

for _ in range(n):
    r, c = map(int, input().split())
    r -= 1
    mn[r] = min(mn[r], c)

answer = min(sum(mn), k)

print(answer)
```Mảng`mn`lưu trữ khả năng tồn tại tối thiểu cho mỗi hàng. Vì các hàng được đánh số từ 1 trong đầu vào nên mã sẽ chuyển đổi chúng thành chỉ mục dựa trên 0 trước khi truy cập vào mảng. 

Việc sử dụng giá trị ban đầu lớn sẽ đảm bảo rằng chiếc răng đầu tiên nhìn thấy trong một hàng sẽ trở thành giá trị tối thiểu hiện tại. Câu lệnh đảm bảo mỗi hàng chứa ít nhất một răng, do đó không có hàng nào chưa được khởi tạo. 

Dòng:```
mn[r] = min(mn[r], c)
```là cốt lõi của giải pháp. Nó liên tục theo dõi răng yếu nhất trong mỗi hàng. 

Sau khi tất cả các răng đã được xử lý xong,`sum(mn)`bằng tổng số bữa ăn về mặt lý thuyết có thể. trận chung kết`min(..., k)`xử lý trường hợp tồn tại nhiều năng lực hơn những cây thánh giá có sẵn. 

Số nguyên Python xử lý an toàn tất cả các giá trị ở đây, vì tổng tối đa có thể nhiều nhất là`1000 * 10^6 = 10^9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 18
2 3
1 2
3 6
2 3
```Các bước xử lý: 

| Răng | Hàng | Khả năng tồn tại | Hàng tối thiểu hiện tại | 
| --- | --- | --- | --- | 
| 1 | 2 | 3 | [INF, 3, INF] | 
| 2 | 1 | 2 | [2, 3, INF] | 
| 3 | 3 | 6 | [2, 3, 6] | 
| 4 | 2 | 3 | [2, 3, 6] | 

Hiện nay:```
sum = 2 + 3 + 6 = 11
```Từ`k = 18`, câu trả lời là:```
11
```Dấu vết này thể hiện tính bất biến trung tâm: mỗi hàng chỉ quan tâm đến khả năng tồn tại tối thiểu của răng. 

### Ví dụ 2 

đầu vào:```
5 2 4
1 10
1 1
2 7
2 9
2 3
```Các bước xử lý: 

| Răng | Hàng | Khả năng tồn tại | Hàng tối thiểu hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | 10 | [10, INF] | 
| 2 | 1 | 1 | [1, INF] | 
| 3 | 2 | 7 | [1, 7] | 
| 4 | 2 | 9 | [1, 7] | 
| 5 | 2 | 3 | [1, 3] | 

Hiện nay:```
sum = 1 + 3 = 4
```Từ`k = 4`, câu trả lời là:```
4
```Ví dụ này cho thấy tại sao việc lấy mức tối thiểu là cần thiết. Các giá trị lớn trong cùng một hàng không giúp ích gì khi hết răng yếu nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần duyệt qua tất cả các răng và một lần tính tổng qua các hàng | 
| Không gian | O(m) | Lưu trữ một giá trị tối thiểu trên mỗi hàng | 

Với tối đa 1000 răng và hàng, giải pháp này cực kỳ nhanh. Việc sử dụng bộ nhớ rất nhỏ và thời gian chạy thực sự tức thời trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    INF = 10**18
    mn = [INF] * m

    for _ in range(n):
        r, c = map(int, input().split())
        r -= 1
        mn[r] = min(mn[r], c)

    print(min(sum(mn), k))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""4 3 18
2 3
1 2
3 6
2 3
"""
) == "11\n", "sample 1"

# minimum-size input
assert run(
"""1 1 0
1 5
"""
) == "0\n", "k is zero"

# single row, weakest tooth limits everything
assert run(
"""3 1 100
1 5
1 2
1 8
"""
) == "2\n", "minimum tooth determines row capacity"

# all rows equal
assert run(
"""4 2 10
1 4
1 4
2 4
2 4
"""
) == "8\n", "equal minima across rows"

# cap by k
assert run(
"""2 2 3
1 10
2 10
"""
) == "3\n", "answer cannot exceed available crucians"

# row with zero viability
assert run(
"""3 2 10
1 0
1 5
2 7
"""
) == "7\n", "row with zero minimum unusable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Răng đơn,`k = 0`|`0`| Giới hạn phần chiếm ưu thế | 
| Một hàng có khả năng tồn tại`5,2,8`|`2`| Hàng kiểm soát khả năng tồn tại tối thiểu | 
| Khả năng tồn tại bình đẳng ở mọi nơi |`8`| Tích lũy bình thường qua các hàng | 
| Công suất lớn nhưng nhỏ`k`|`3`| Câu trả lời cuối cùng phải được giới hạn | 
| Hàng chứa số 0 |`7`| Hàng tối thiểu không đóng góp gì | 

## Vỏ cạnh 

Hãy xem xét trường hợp một chiếc răng yếu chặn một hàng răng chắc khỏe:```
3 1 100
1 5
1 2
1 8
```Thuật toán lưu trữ giá trị tối thiểu cho hàng 1. 

Sau khi xử lý toàn bộ răng:```
mn = [2]
```Tổng số trở thành`2`, do đó đầu ra là:```
2
```Sau hai lần sử dụng, răng có thể tồn tại được`2`đạt đến số không. Sử dụng thêm một lần nữa sẽ khiến nó trở nên tiêu cực. 

Bây giờ hãy xem xét một hàng chứa một chiếc răng có khả năng tồn tại bằng 0:```
2 2 10
1 0
2 7
```cực tiểu trở thành:```
mn = [0, 7]
```Tổng số tiền là`7`, vậy đáp án là:```
7
```Hàng đầu tiên không thể được sử dụng dù chỉ một lần vì răng yếu nhất của nó sẽ ngay lập tức trở nên tiêu cực sau một lần sử dụng. 

Cuối cùng, hãy cân nhắc khi tổng công suất vượt quá phần ăn tối có sẵn:```
2 2 3
1 10
2 10
```Tổng cực tiểu của hàng là:```
10 + 10 = 20
```Nhưng chỉ có 3 cây thánh giá tồn tại. Thuật toán được áp dụng:```
min(20, 3) = 3
```và kết quả đầu ra:```
3
```Nắp cuối cùng này là cần thiết ngay cả khi răng có thể hỗ trợ nhiều bữa ăn hơn.
