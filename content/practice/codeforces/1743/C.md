---
title: "CF 1743C - Lưu tạp chí"
description: "Chúng tôi có một hàng hộp. Hộp thứ i chứa một[i] tạp chí. Một chuỗi nhị phân cho chúng ta biết hộp nào ban đầu có nắp. Mỗi nắp được gắn vào một hộp ban đầu chứa số '1'. Cái nắp đó chỉ có hai lựa chọn: giữ nguyên vị trí hoặc di chuyển một vị trí sang trái."
date: "2026-06-09T16:01:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1743
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 137 (Rated for Div. 2)"
rating: 1100
weight: 1743
solve_time_s: 138
verified: true
draft: false
---

[CF 1743C - Lưu tạp chí](https://codeforces.com/problemset/problem/1743/C) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, dp, tham lam 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng hộp. Hộp`i`chứa`a[i]`tạp chí. Một chuỗi nhị phân cho chúng ta biết hộp nào ban đầu có nắp. 

Mỗi nắp được gắn vào một hộp ban đầu chứa`'1'`. Cái nắp đó chỉ có hai lựa chọn: giữ nguyên vị trí hoặc di chuyển một vị trí sang trái. Một nắp không thể di chuyển nhiều lần. 

Sau khi tất cả các quyết định được đưa ra, mỗi hộp có nắp sẽ đóng góp số tạp chí của nó vào câu trả lời. Mục tiêu là tối đa hóa tổng số tạp chí đã lưu. 

Điều thú vị là nắp là những vật thể độc lập nhưng các lựa chọn của chúng lại tương tác với nhau. Một nắp từ vị trí`i`có thể bảo vệ một trong hai hộp`i`hoặc hộp`i-1`. Việc chọn một tùy chọn có thể ngăn nắp khác bảo vệ chiếc hộp có giá trị hơn. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, điều này ngay lập tức loại trừ mọi thứ bậc hai. MỘT`O(n²)`thuật toán sẽ thực hiện khoảng`4 · 10^10`trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một số trường hợp đặc biệt khiến việc triển khai tham lam không thành công. 

Coi như:```
n = 2
s = 01
a = [10, 1]
```Nắp duy nhất bắt đầu ở vị trí 2. Giữ nắp ở đó sẽ tiết kiệm`1`, trong khi di chuyển nó sang trái sẽ lưu`10`. Câu trả lời đúng là`10`. Một chiến lược luôn bảo vệ các hộp có mái che hiện có sẽ trở lại`1`. 

Coi như:```
n = 3
s = 011
a = [5, 100, 1]
```Kết quả tối ưu là`105`. 

Di chuyển nắp từ vị trí 2 sang vị trí 1, giữ nguyên nắp ở vị trí 3. Các hộp đã lưu là`{1, 3}`có giá trị`5 + 1 = 6`, điều đó không tối ưu. 

Thay vào đó, hãy giữ nắp ở vị trí 2 và di chuyển nắp từ vị trí 3 sang vị trí 2. Vì hai nắp không thể chiếm cùng một hộp cuối cùng nên chúng ta cần diễn giải quy trình tốt hơn. Giải pháp tham lam đúng đắn sẽ chọn ra những tạp chí tốt nhất trong một dãy tạp chí liên tiếp một cách hiệu quả. Một quyết định cục bộ đơn giản có thể dễ dàng mất đi giá trị`100`. 

Một trường hợp tế nhị khác là:```
n = 4
s = 0111
a = [5, 4, 5, 1]
```Câu trả lời là`14`, không`10`. 

Một chiến lược ngây thơ luôn bảo vệ chiếc hộp hiện đang được che chắn sẽ tiết kiệm được`4 + 5 + 1 = 10`. Di chuyển nắp từ vị trí 2 sang vị trí 1 cho phép chúng ta tiết kiệm`5 + 5 + 4 = 14`. 

Nhận xét quan trọng là đôi khi chúng ta cố tình hy sinh một chiếc hộp có nắp nhỏ hơn để dành nắp cho một chiếc hộp lớn hơn không có nắp. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ kiểm tra mọi nắp và thử cả hai khả năng, ở lại hoặc di chuyển sang trái. Nếu có`k`nắp, điều này tạo ra`2^k`các cấu hình có thể. Trong trường hợp xấu nhất, mọi vị trí đều có nắp, do đó`k = n`. 

Vì`n = 2 · 10^5`, thậm chí`2^50`đã là không thể, và`2^200000`có kích thước lớn về mặt thiên văn. Cách tiếp cận bạo lực là đúng vì nó kiểm tra rõ ràng mọi thỏa thuận pháp lý, nhưng nó gần như không thể sử dụng được ngay lập tức. 

Để tìm ra giải pháp nhanh hơn, chúng ta cần hiểu cái nắp thực sự đại diện cho điều gì. 

Giả sử chúng ta gặp một`'1'`. Cái nắp đó đảm bảo rằng một hộp trong một khu vực địa phương nhỏ cuối cùng sẽ được bảo vệ. Nếu một`'1'`theo sau một`'0'`, chúng ta có quyền lựa chọn: bảo vệ hộp hiện tại hoặc chuyển lớp bảo vệ sang hộp chưa được che chắn trước đó. 

Điều này tạo ra một hiệu ứng dây chuyền bên trong mỗi phân đoạn liên tiếp của những cái đó. Bất cứ khi nào chúng ta nhìn thấy một`'1'`nối tiếp cái khác`'1'`, việc bảo vệ có thể được sắp xếp lại một cách hiệu quả giữa các hộp trong phân đoạn đó. Tạp chí duy nhất chúng tôi có thể mất là những giá trị nhỏ nhất trong vùng được sắp xếp lại. 

Một quá trình tham lam đặc biệt tao nhã xuất hiện. 

Chúng tôi quét từ trái sang phải. 

Khi chúng ta đạt đến một vị trí chứa`'1'`, chúng tôi tạm thời lưu các tạp chí của nó bằng cách thêm`a[i]`để trả lời. 

Nếu vị trí trước đó là`'0'`, chúng tôi đã phát hiện ra sự khởi đầu của một khối mới. Thay vào đó, chúng tôi có thể muốn chuyển phần bảo vệ sang trái và bao gồm hộp trước đó. 

Quy tắc tham lam là: 

Duy trì số lượng tạp chí tối thiểu trong số các hộp được bảo vệ hiện được chọn bên trong khối hoạt động. 

Khi mới`'1'`mở rộng khối, nếu hộp chưa được phát hiện trước đó có nhiều tạp chí hơn hộp được bảo vệ nhỏ nhất cho đến nay, hãy thay thế giá trị nhỏ nhất đó bằng giá trị của hộp trước đó. 

Sự thay thế này chính xác là những gì sự sắp xếp lại tối ưu sẽ làm. Thay vào đó, chúng tôi đánh đổi chiếc hộp được bảo vệ yếu nhất và bảo vệ một chiếc hộp chưa được che chắn có giá trị hơn. 

Kết quả là quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k) | O(k) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo câu trả lời về 0. 
2. Quét mảng từ trái sang phải. 
3. Khi nào`s[i] = '1'`, thêm vào`a[i]`cho câu trả lời vì hộp này ban đầu được bảo vệ và là một ứng cử viên để tiếp tục được bảo vệ. 
4. Nếu`s[i] = '1'`Và`s[i-1] = '0'`, một khối mới bắt đầu. Đặt một biến`cur`ĐẾN`a[i]`. 

Đây`cur`lưu trữ số tạp chí nhỏ nhất hiện được chọn bên trong khối hoạt động này. 
5. Nếu`s[i] = '1'`Và`s[i-1] = '1'`, cập nhật`cur = min(cur, a[i])`. 

Giá trị được bảo vệ nhỏ nhất trong khối có thể đã thay đổi. 
6. Bất cứ khi nào một khối bắt đầu sau số 0, hãy so sánh giá trị của hộp chưa được phát hiện`a[i-1]`với`cur`. 

Nếu như`a[i-1] > cur`, thay thế hộp được bảo vệ yếu nhất bằng hộp không được che chắn này sẽ tăng câu trả lời lên`a[i-1] - cur`. 

Thêm sự khác biệt này vào câu trả lời và đặt`cur = a[i-1]`. 
7. Tiếp tục cho đến hết mảng. 

Phiên bản thân thiện với việc triển khai hơn được sử dụng bởi hầu hết các giải pháp được chấp nhận sẽ hơi khác một chút nhưng tương đương. 

Khi chúng ta gặp phải một`'1'`sau một`'0'`, chúng ta bắt đầu một khối và theo dõi giá trị của ô vị trí 0 trước đó. Khi khối phát triển, bất cứ khi nào giá trị được bảo vệ mới lớn hơn giá trị được lưu trữ đó, chúng tôi sẽ hoán đổi chúng về mặt khái niệm và nhận thêm tạp chí. 

### Tại sao nó hoạt động 

Hãy xem xét một phân đoạn tối đa trông giống như:```
0 1 1 1 1 ...
```Mỗi nắp bên trong đoạn này chỉ được phép di chuyển sang trái một vị trí. Tổng số hộp có thể được bảo vệ bằng số nắp trong phân khúc. 

Trong số tất cả các hộp bao gồm vị trí số 0 đứng đầu và các hộp trong đoạn, phải loại trừ chính xác một hộp khỏi sự bảo vệ. Để tối đa hóa số tạp chí đã lưu, chúng ta nên loại trừ hộp có số tạp chí nhỏ nhất. 

Quá trình quét tham lam duy trì chính xác tính bất biến này. Khi phân khúc phát triển, nó liên tục theo dõi giá trị nhỏ nhất hiện được bảo vệ. Bất cứ khi nào một ứng cử viên tốt hơn xuất hiện, nó sẽ thay thế giá trị nhỏ nhất đó. Đến cuối đoạn, mọi giá trị ngoại trừ giá trị tối thiểu đều góp phần đưa ra câu trả lời, đó là lựa chọn tối ưu. 

Vì các phân đoạn là độc lập nên việc tối ưu hóa từng phân đoạn một cách độc lập sẽ mang lại câu trả lời tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()
        a = list(map(int, input().split()))

        ans = 0
        prev = 0

        for i in range(n):
            if s[i] == '1':
                if i > 0 and s[i - 1] == '0':
                    prev = a[i - 1]

                if prev and prev > a[i]:
                    ans += prev
                    prev = a[i]
                else:
                    ans += a[i]

        print(ans)

solve()
```Biến`ans`lưu trữ tổng số tạp chí đã lưu cho đến nay. 

Biến`prev`đại diện cho ô chưa được phát hiện của ứng viên ngay trước khối ô hiện tại. Khi một khối bắt đầu ở vị trí`i`, hộp ở`i-1`là chiếc hộp duy nhất không được che chắn có khả năng đi vào bộ được bảo vệ thông qua việc dịch chuyển nắp. 

Khi chúng tôi xử lý một`'1'`, chúng tôi so sánh giá trị của nó với`prev`. 

Nếu như`prev > a[i]`, bảo vệ hộp trước đó tốt hơn bảo vệ hộp hiện tại. Chúng tôi thêm`prev`và giữ giá trị nhỏ hơn làm ứng cử viên mới mà sau này có thể bị loại bỏ. 

Nếu như`prev <= a[i]`, hộp hiện tại ít nhất cũng có giá trị, vì vậy chúng tôi bảo vệ nó và thêm`a[i]`. 

bản cập nhật`prev = a[i]`sau khi thay thế là phần tinh tế. Điều đó có nghĩa là giá trị nhỏ hơn sẽ trở thành ô được chọn yếu nhất trong phân đoạn hiện tại, sau này có thể được thay thế lại bằng một ứng cử viên thậm chí còn lớn hơn. 

Tất cả số học dễ dàng nằm gọn trong các số nguyên Python vì câu trả lời tối đa nhiều nhất là`2 · 10^5 × 10^4 = 2 · 10^9`. 

## Ví dụ đã hoạt động 

### Mẫu 1```
s = 01110
a = [10, 5, 8, 9, 6]
```| tôi | s[i] | một [tôi] | trước trước | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 10 | 0 | bỏ qua | 0 | 
| 1 | 1 | 5 | 10 | lấy 10, đặt trước=5 | 10 | 
| 2 | 1 | 8 | 5 | lấy 8 | 18 | 
| 3 | 1 | 9 | 5 | mất 9 | 27 | 
| 4 | 0 | 6 | 5 | bỏ qua | 27 | 

Câu trả lời cuối cùng:`27`. 

Nắp đầu tiên được sử dụng ở hộp 1 tốt hơn hộp 2 vì`10 > 5`. Sau quyết định đó, các hộp còn lại trong khối vẫn được bảo vệ. 

### Mẫu 2```
s = 011011
a = [20, 10, 9, 30, 20, 19]
```| tôi | s[i] | một [tôi] | trước trước | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 20 | 0 | bỏ qua | 0 | 
| 1 | 1 | 10 | 20 | lấy 20, trước=10 | 20 | 
| 2 | 1 | 9 | 10 | lấy 10, trước=9 | 30 | 
| 3 | 0 | 30 | 9 | bỏ qua | 30 | 
| 4 | 1 | 20 | 30 | lấy 30, trước=20 | 60 | 
| 5 | 1 | 19 | 20 | lấy 20, trước=19 | 80 | 

Câu trả lời cuối cùng:`80`. 

Ví dụ này cho thấy logic tham lam tương tự hoạt động độc lập như thế nào trên nhiều phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một lần | 
| Không gian | O(1) | Chỉ có một số biến được duy trì | 

Tổng của tất cả`n`giá trị trên các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`. Quét tuyến tính trên tất cả các vị trí chỉ thực hiện vài trăm nghìn thao tác, vừa vặn trong giới hạn thời gian. Thuật toán sử dụng bộ nhớ bổ sung không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()
        a = list(map(int, input().split()))

        ans = 0
        prev = 0

        for i in range(n):
            if s[i] == '1':
                if i > 0 and s[i - 1] == '0':
                    prev = a[i - 1]

                if prev and prev > a[i]:
                    ans += prev
                    prev = a[i]
                else:
                    ans += a[i]

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""4
5
01110
10 5 8 9 6
6
011011
20 10 9 30 20 19
4
0000
100 100 100 100
4
0111
5 4 5 1
"""
) == "27\n80\n0\n14"

# minimum size
assert run(
"""1
1
0
7
"""
) == "0"

# single lid
assert run(
"""1
2
01
10 1
"""
) == "10"

# all ones
assert run(
"""1
4
1111
5 5 5 5
"""
) == "20"

# replacement chain
assert run(
"""1
4
0111
100 1 2 3
"""
) == "105"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, s=0`|`0`| Đầu vào kích thước tối thiểu | 
|`01, [10,1]`|`10`| Di chuyển nắp sang trái có lợi | 
|`1111, [5,5,5,5]`|`20`| Không có số 0 đứng đầu trước khối | 
|`0111, [100,1,2,3]`|`105`| Nhiều thay thế bên trong một khối | 

## Vỏ cạnh 

Hãy xem xét:```
n = 2
s = 01
a = [10, 1]
```Ở đầu khối,`prev = 10`. Từ`10 > 1`, thuật toán thêm`10`và các cửa hàng`1`là giá trị yếu nhất. Câu trả lời cuối cùng là`10`, điều này là tối ưu vì nắp nên di chuyển sang trái. 

Coi như:```
n = 4
s = 0111
a = [5, 4, 5, 1]
```Thuật toán bắt đầu với`prev = 5`. Tại giá trị`4`, nó thay thế và đạt được`5`. Tại giá trị`5`, nó đạt được`5`. Tại giá trị`1`, nó đạt được`4`. Tổng số tạp chí đã lưu bằng nhau`14`. Đây chính xác là sự sắp xếp tối ưu trong đó giá trị nhỏ nhất trong phân khúc, cụ thể là`1`, bị loại trừ. 

Coi như:```
n = 4
s = 0000
a = [100, 100, 100, 100]
```Không có vị trí nào có nắp nên quá trình quét không bao giờ thêm bất cứ thứ gì. Câu trả lời vẫn còn`0`, điều đó đúng. 

Coi như:```
n = 4
s = 1111
a = [3, 7, 2, 8]
```Không có số 0 đứng trước có thể được đưa vào tập được bảo vệ. Mỗi nắp chỉ bảo vệ một số hộp bên trong phân khúc và tất cả bốn hộp đều có thể được bảo vệ. Thuật toán cộng tất cả các giá trị và trả về`20`.
