---
title: "CF 426A - Sereja và Cốc"
description: "Chúng ta có một chiếc cốc có dung tích s và một số cốc chứa nước. Người chơi lần lượt chọn một cốc không rỗng và đổ hết nước vào cốc. Chiếc cốc bắt đầu trống rỗng và nước không bao giờ bị loại bỏ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 426
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 243 (Div. 2)"
rating: 800
weight: 426
solve_time_s: 80
verified: true
draft: false
---

[CF 426A - Sereja và Cốc](https://codeforces.com/problemset/problem/426/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chiếc cốc có dung tích`s`và một số cốc chứa nước. Người chơi lần lượt chọn một cốc không rỗng và đổ hết nước vào cốc. Chiếc cốc bắt đầu trống rỗng và nước không bao giờ bị loại bỏ. Nếu tại bất kỳ thời điểm nào lượng nước bên trong cốc lớn hơn sức chứa của nó thì người chơi thực hiện nước đi đó sẽ thua. 

Câu hỏi đặt ra là liệu các cốc có thể được rót theo thứ tự nào đó để không ai thua hay không. 

Chi tiết quan trọng là mọi cốc cuối cùng đều phải được rót đầy. Vì chiếc cốc chỉ thu được nước nên lượng nước sau khi di chuyển bằng tổng thể tích của tất cả các cốc. Thứ tự đổ chỉ thay đổi các trạng thái trung gian. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 cốc và mỗi thể tích cốc nhiều nhất là 10. Ngay cả một cuộc tìm kiếm mạnh mẽ trên nhiều khả năng cũng sẽ diễn ra đủ nhanh ở đây, nhưng vấn đề có một quan sát đơn giản hơn nhiều khiến mọi thứ chỉ còn một so sánh duy nhất. 

Một sai lầm phổ biến là nghĩ rằng thứ tự rót là quan trọng. Hãy xem xét đầu vào này:```
3 5
4 1 1
```Câu trả lời đúng là:```
NO
```Một cách tiếp cận bất cẩn có thể thử đổ`1`, sau đó`1`, sau đó`4`, hy vọng có một trật tự tốt hơn. Nhưng lượng nước cuối cùng luôn luôn`6`, vượt quá dung tích cốc`5`. Vì cuối cùng tất cả các cốc đều phải được rót nên một số người chơi phải làm tràn cốc. 

Một trường hợp khác xuất hiện khi tổng lượng nước khớp chính xác với dung tích cốc:```
2 7
3 4
```Câu trả lời đúng là:```
YES
```Tràn chỉ xảy ra khi số lượng lớn hơn khả năng. Tiếp cận chính xác`7`được cho phép. 

Tình huống dễ bỏ sót thứ ba là khi có nhiều cốc nhỏ:```
5 4
1 1 1 1 1
```Câu trả lời đúng là:```
NO
```Mỗi nước đi trông có vẻ vô hại nhưng sau cốc thứ năm, chiếc cốc chứa`5`, vượt quá khả năng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ nhất là thử mọi thứ tự đổ có thể. Đối với mỗi hoán vị, chúng tôi mô phỏng quy trình và kiểm tra xem cốc có bị tràn hay không. 

Điều này có tác dụng vì trò chơi hoàn toàn được xác định khi chúng ta chọn thứ tự các cốc. Nếu có ít nhất một đơn hàng tránh được tình trạng tràn thì câu trả lời là`"YES"`. 

Vấn đề là số lượng hoán vị tăng theo giai thừa. Với`n = 100`, có`100!`những mệnh lệnh có thể có, vốn rất lớn về mặt thiên văn. Ngay cả đối với`n = 15`, việc tìm kiếm toàn diện đã không thể thực hiện được trong vòng một giây. 

Quan sát đột phá là thứ tự thực sự không quan trọng. 

Lượng nước trong cốc chỉ tăng lên. Sau khi đổ tất cả các cốc, cốc chứa chính xác tổng thể tích của tất cả các cốc. Nếu tổng số này vượt quá dung tích cốc thì ở một bước nào đó sẽ xảy ra hiện tượng tràn. Không có đơn đặt hàng có thể tránh được nó. 

Mặt khác, nếu tổng thể tích tối đa bằng dung tích cốc thì việc rót cốc theo thứ tự bất kỳ là an toàn. Mực nước không bao giờ vượt quá tổng lượng cuối cùng và tổng lượng cuối cùng đã phù hợp. 

Điều này làm giảm toàn bộ vấn đề để kiểm tra:```
sum(a) <= s
```Nếu đúng thì in`"YES"`. Ngược lại, in`"NO"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! × n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số cốc`n`và dung tích cốc`s`. 
2. Đọc danh sách các tập cốc. 
3. Tính tổng thể tích của tất cả các cốc. 
4. So sánh tổng lượng nước với dung tích cốc. 

Nếu tổng nhỏ hơn hoặc bằng`s`, in`"YES"`. 

Ngược lại, in`"NO"`. 

### Tại sao nó hoạt động 

Chiếc cốc bắt đầu trống rỗng và nước chỉ được thêm vào, không bao giờ loại bỏ. Sau tất cả các lần di chuyển, tổng lượng nước bên trong cốc bằng tổng thể tích của tất cả các cốc. 

Nếu tổng số này vượt quá dung tích cốc thì việc tràn là không thể tránh khỏi vì bản thân trạng thái cuối cùng không hợp lệ. 

Nếu tổng vừa với cốc thì mọi trạng thái trung gian cũng vừa, vì lượng trung gian luôn nhỏ hơn hoặc bằng lượng cuối cùng. 

Do hành vi đơn điệu này nên việc kiểm tra tổng là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, s = map(int, input().split())
a = list(map(int, input().split()))

if sum(a) <= s:
    print("YES")
else:
    print("NO")
```Chương trình trực tiếp tuân theo quan sát toán học từ thuật toán. 

Dòng đầu tiên ghi số lượng cốc và dung tích cốc. Dòng thứ hai đọc tất cả các tập cốc vào một danh sách. 

Thao tác quan trọng là`sum(a)`. Điều này tính toán tổng lượng nước cuối cùng sẽ chảy vào cốc. 

Việc so sánh sử dụng`<=`, không`<`. Cho phép đạt đến dung tích chính xác vì cốc chỉ tràn khi số lượng vượt quá giới hạn. 

Không cần xử lý đặc biệt đối với việc đặt hàng vì bằng chứng cho thấy việc đặt hàng không thể thay đổi kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
1 1 1
```| Bước | Cốc hiện tại | Nước Trong Cốc | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| Đổ 1 | 1 | 1 | 
| Đổ 2 | 1 | 2 | 
| Đổ 3 | 1 | 3 | 

Tổng lượng nước là`3`, nhỏ hơn khả năng`4`. Vì số lượng cuối cùng phù hợp nên mọi lượng trung gian cũng phù hợp. Câu trả lời là`"YES"`. 

### Ví dụ 2 

đầu vào:```
3 5
4 1 1
```| Bước | Cốc hiện tại | Nước Trong Cốc | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| Đổ 1 | 4 | 4 | 
| Đổ 2 | 1 | 5 | 
| Đổ 3 | 1 | 6 | 

Tổng lượng nước là`6`, vượt quá khả năng`5`. Cho dù chúng ta có sắp xếp lại các cốc như thế nào thì số tiền cuối cùng vẫn là`6`, nên việc tràn là khó tránh khỏi. Câu trả lời là`"NO"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét các thể tích cốc một lần để tính tổng của chúng | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng ngoài danh sách đầu vào | 

Với tối đa 100 cốc, giải pháp sẽ diễn ra ngay lập tức. Cả thời gian và mức sử dụng bộ nhớ đều thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, s = map(int, input().split())
    a = list(map(int, input().split()))

    if sum(a) <= s:
        print("YES")
    else:
        print("NO")

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
assert run("3 4\n1 1 1\n") == "YES\n", "sample 1"

# minimum valid input
assert run("2 1\n1 1\n") == "NO\n", "minimum size with overflow"

# exact boundary
assert run("2 7\n3 4\n") == "YES\n", "sum exactly equals capacity"

# all equal values
assert run("5 10\n2 2 2 2 2\n") == "YES\n", "all equal and fits exactly"

# overflow by one
assert run("4 9\n2 2 2 4\n") == "NO\n", "sum exceeds by one"

# large input within limits
assert run(
    "100 1000\n" + "10 " * 100 + "\n"
) == "YES\n", "maximum-sized input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 1`|`NO`| Kích thước đầu vào nhỏ nhất và không thể tránh khỏi tràn | 
|`2 7 / 3 4`|`YES`| Bình đẳng chính xác về năng lực được cho phép | 
|`5 10 / 2 2 2 2 2`|`YES`| Giá trị thống nhất và phù hợp chính xác | 
|`4 9 / 2 2 2 4`|`NO`| Tràn bởi một đơn vị | 
|`100 1000 / 10 repeated 100 times`|`YES`| Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét trường hợp tổng thể tích chính xác bằng dung tích cốc:```
2 7
3 4
```Thuật toán tính toán`3 + 4 = 7`. Từ`7 <= 7`, nó in`"YES"`. 

Theo dõi quá trình: 

| Sau khi di chuyển | Nước Trong Cốc | 
| --- | --- | 
| Bắt đầu | 0 | 
| Sau cốc đầu tiên | 3 | 
| Sau cốc thứ hai | 7 | 

Chiếc cốc không bao giờ vượt quá sức chứa nên sự bình đẳng phải được coi là an toàn. 

Bây giờ hãy xem xét trường hợp mỗi cốc đều nhỏ nhưng tổng cộng lại quá lớn:```
5 4
1 1 1 1 1
```Thuật toán tính toán`1 + 1 + 1 + 1 + 1 = 5`. Từ`5 > 4`, nó in`"NO"`. 

Theo dõi quá trình: 

| Sau khi di chuyển | Nước Trong Cốc | 
| --- | --- | 
| Bắt đầu | 0 | 
| Sau cốc đầu tiên | 1 | 
| Sau cốc thứ hai | 2 | 
| Sau cốc thứ ba | 3 | 
| Sau cốc thứ tư | 4 | 
| Sau cốc thứ năm | 5 | 

Tràn nước chỉ xảy ra ở nước đi cuối cùng nhưng vẫn không thể tránh khỏi. 

Cuối cùng, hãy xem xét trường hợp một cốc đã gần đầy:```
3 5
4 1 1
```Tổng cộng là`6`, do đó thuật toán in`"NO"`ngay lập tức. 

Việc thử các đơn đặt hàng khác nhau không giúp ích gì: 

| Đặt hàng | Nước Cuối Cùng | 
| --- | --- | 
|`4,1,1`| 6 | 
|`1,4,1`| 6 | 
|`1,1,4`| 6 | 

Số tiền cuối cùng luôn giống nhau, đó chính xác là lý do tại sao việc đặt hàng không liên quan.
