---
title: "CF 1842B - Tenzing và Sách"
description: "Chúng tôi có ba chồng sách. Mỗi chồng có n cuốn sách được sắp xếp từ trên xuống dưới. Mỗi cuốn sách đều có một số nguyên không âm được viết trên đó. Tenzing bắt đầu với kiến ​​thức bằng 0."
date: "2026-06-09T06:12:58+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1842
codeforces_index: "B"
codeforces_contest_name: "CodeTON Round 5 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1100
weight: 1842
solve_time_s: 105
verified: true
draft: false
---

[CF 1842B - Tenzing và Sách](https://codeforces.com/problemset/problem/1842/B) 

**Đánh giá:** 1100 
**Tags:** bitmasks, tham lam, toán học 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba chồng sách. Mỗi ngăn xếp chứa`n`sách sắp xếp từ trên xuống dưới. Mỗi cuốn sách đều có một số nguyên không âm được viết trên đó. 

Tenzing bắt đầu với kiến ​​thức tương đương`0`. Bất cứ khi nào anh ấy đọc một cuốn sách có giá trị`v`, kiến ​​thức của anh ấy trở thành`knowledge | v`, Ở đâu`|`là phép toán OR theo bit. 

Hạn chế đến từ cấu trúc ngăn xếp. Tenzing chỉ có thể đọc cuốn sách hàng đầu hiện tại của chồng sách. Sau khi đọc xong, cuốn sách đó sẽ biến mất và cuốn sách tiếp theo sẽ có sẵn. Anh ta có thể chuyển đổi giữa các ngăn xếp bất cứ lúc nào và có thể dừng bất cứ khi nào anh ta muốn. 

Mục đích là để xác định liệu một số chuỗi đọc có thể làm cho kiến ​​thức cuối cùng của anh ta chính xác bằng`x`. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được giá trị đích`x`và ba ngăn xếp. Chúng ta phải xuất`"Yes"`nếu một số trình tự đọc hợp lệ đạt chính xác`x`, nếu không thì`"No"`. 

Tổng của tất cả`n`giá trị trên toàn bộ đầu vào nhiều nhất là`10^5`. Điều đó ngay lập tức gợi ý rằng cần phải có giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào thử nhiều thứ tự đọc khác nhau hoặc khám phá các trạng thái của giá trị OR sẽ nhanh chóng trở nên không khả thi. 

Phần nguy hiểm nhất của vấn đề là bản chất không thể đảo ngược của bitwise OR. Một khi một chút trở thành`1`, nó không bao giờ có thể quay trở lại`0`. Việc triển khai ngây thơ có thể vô tình cho phép những cuốn sách giới thiệu những thông tin không có trong`x`, mặc dù việc di chuyển như vậy sẽ vĩnh viễn khiến việc tiếp cận`x`không thể nào. 

Hãy xem xét ví dụ này:```
n = 1, x = 2

Stack A: 3
Stack B: 0
Stack C: 0
```giá trị`3`chứa bit`11₂`, trong khi`x = 2`là`10₂`. Đọc`3`giới thiệu thêm một bit không thuộc về`x`, vậy câu trả lời là`"No"`. 

Một trường hợp tế nhị khác là chúng ta có thể dừng lại bất cứ lúc nào.```
n = 1, x = 0

Stack A: 5
Stack B: 7
Stack C: 9
```Câu trả lời đúng là`"Yes"`bởi vì không được phép đọc sách. 

Sai lầm phổ biến thứ ba là nghĩ rằng mọi cuốn sách đều phải đọc.```
n = 2, x = 1

Stack A: 1 2
Stack B: 0 0
Stack C: 0 0
```Chỉ đọc cuốn sách đầu tiên của ngăn xếp A đã tạo ra`1`. Đọc thêm sẽ tạo ra giá trị`3`, điều đó là sai. Vì được phép dừng lại nên câu trả lời là`"Yes"`. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử tất cả các trình tự đọc có thể. Ở mỗi bước, chúng ta có thể chọn một trong các ngăn xếp hiện không trống hoặc dừng lại. Số lượng các chuỗi có thể tăng theo cấp số nhân vì mỗi ngăn xếp có thể được nâng cao một cách độc lập. Ngay cả đối với những giá trị khiêm tốn của`n`, số lượng khả năng trở nên rất lớn. 

Ý tưởng brute-force là đúng vì nó khám phá rõ ràng mọi thứ tự đọc hợp lệ và kiểm tra xem một số OR cuối cùng có bằng không`x`. Vấn đề là với`n`lên đến`10^5`nhìn chung, việc khám phá như vậy là hoàn toàn không thể. 

Quan sát quan trọng đến từ việc kiểm tra xem khi nào một cuốn sách có thể được đọc một cách an toàn. 

Giả sử kiến ​​thức hiện tại là`cur`. Nếu một cuốn sách có chứa một chút gì đó không có trong`x`, thì sau khi thực hiện thao tác OR bit đó sẽ tồn tại mãi mãi. Tiếp cận chính xác`x`trở nên không thể. 

Điều đó có nghĩa là mọi cuốn sách chúng ta từng đọc đều phải thỏa mãn:```
(book | x) = x
```Tương tự, tất cả các bit được đặt trong cuốn sách đã được chứa trong`x`. 

Bây giờ hãy nhìn vào một ngăn xếp từ trên xuống. Ngay khi gặp một cuốn sách có chứa điều cấm kỵ, chúng ta không bao giờ có thể đọc được cuốn sách đó. Vì nó chặn quyền truy cập vào tất cả các sách bên dưới nó nên toàn bộ phần còn lại của ngăn xếp đó sẽ không thể sử dụng được. 

Điều này mang lại một đặc tính rất đơn giản. 

Đối với mỗi ngăn xếp, hãy đọc sách từ trên cùng trong khi các bit của chúng là tập hợp con của`x`. HOẶC tất cả các giá trị như vậy thành kết quả chung. Ngay khi chúng tôi thấy sách không hợp lệ, hãy ngừng xử lý ngăn xếp đó. 

Tại sao HOẶC mọi cuốn sách hợp lệ có thể truy cập lại an toàn? Bởi vì OR chỉ thêm bit. Mỗi cuốn sách hợp lệ có thể truy cập đều đóng góp các bit thuộc về`x`và không bao giờ có lý do để bỏ qua những bit như vậy. Nếu có thể lấy được một chút từ một số cuốn sách hợp lệ có thể truy cập được thì việc đưa nó vào HOẶC cuối cùng chỉ giúp chúng ta tiếp cận`x`. 

Sau khi xử lý cả ba ngăn xếp, đặt OR tích lũy là`ans`. 

Nếu như`ans == x`, thì chúng ta có thể chọn thứ tự đọc đọc chính xác những cuốn sách hợp lệ có thể truy cập đó và tiếp cận`x`. Nếu không thì một chút`x`không bao giờ có được, vì vậy câu trả lời là`"No"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`ans = 0`. 
2. Xử lý từng ngăn xếp một cách độc lập từ trên xuống dưới. 
3. Đối với giá trị sổ sách hiện tại`v`, kiểm tra xem tất cả các bit được thiết lập của nó có thuộc về`x`. 

Điều này tương đương với việc kiểm tra:```
(v | x) == x
```4. Nếu kiểm tra thành công, HOẶC ghi vào câu trả lời:```
ans |= v
```Một cuốn sách như vậy có thể được đọc một cách an toàn vì nó không bao giờ đưa ra những điều cấm kỵ. 
5. Nếu kiểm tra không thành công, hãy ngừng xử lý ngăn xếp này ngay lập tức. 

Bất kỳ cuốn sách nào ở dưới nó đều không thể truy cập được vì sách phải được xóa khỏi đầu trước. 
6. Sau khi cả ba ngăn xếp đã được xử lý, hãy so sánh`ans`với`x`. 
7. Đầu ra`"Yes"`nếu như`ans == x`, nếu không thì xuất ra`"No"`. 

### Tại sao nó hoạt động 

Một cuốn sách chứa đựng một chút bên ngoài`x`không bao giờ có thể xuất hiện trong một chuỗi đọc hợp lệ. Đọc nó sẽ đặt vĩnh viễn một bit bị cấm, làm cho giá trị cuối cùng lớn hơn`x`ở vị trí bit đó. 

Do đó, đối với mỗi ngăn xếp, chỉ có thể truy cập được tiền tố tối đa của những cuốn sách hợp lệ. Bất kỳ cuốn sách nào sau cuốn sách không hợp lệ đầu tiên đều không thể truy cập được vì không thể xóa sách chặn. 

Thuật toán tính toán OR của mọi cuốn sách có thể tham gia vào một chuỗi hợp lệ. Nếu kết quả OR bằng`x`, thì tất cả các bit của`x`có thể đạt được và không có bit bị cấm xuất hiện. Đọc những cuốn sách đó mang lại kết quả chính xác`x`. 

Nếu kết quả OR bị thiếu một số bit`x`, thì không có cuốn sách nào có thể truy cập được chứa bit đó. Không có chiến lược đọc nào có thể tạo ra nó, vì vậy hãy tiếp cận`x`là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, x = map(int, input().split())

        stacks = [
            list(map(int, input().split())),
            list(map(int, input().split())),
            list(map(int, input().split()))
        ]

        ans = 0

        for stack in stacks:
            for v in stack:
                if (v | x) == x:
                    ans |= v
                else:
                    break

        print("Yes" if ans == x else "No")

if __name__ == "__main__":
    solve()
```Giải pháp theo sau bằng chứng trực tiếp. 

điều kiện`(v | x) == x`kiểm tra xem mọi bit có được đặt vào không`v`cũng được thiết lập trong`x`. Đây là quan sát quan trọng của vấn đề. Việc sử dụng bất kỳ điều kiện nào khác có xu hướng bỏ sót các trường hợp trong đó một cuốn sách giới thiệu một điều bị cấm. 

các`break`cũng quan trọng không kém. Khi một cuốn sách không hợp lệ xuất hiện, ngăn xếp đó sẽ bị chặn vĩnh viễn. Việc tiếp tục kiểm tra những cuốn sách bên dưới sẽ cho rằng chúng có thể truy cập được một cách không chính xác. 

Biến`ans`lưu trữ OR của tất cả các sách hợp lệ có thể truy cập được. Vì OR có tính kết hợp và giao hoán nên thứ tự đọc chính xác không quan trọng đối với giá trị cuối cùng. 

Số nguyên Python dễ dàng xử lý các giá trị liên quan vì tối đa tất cả các số đều`10^9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, x = 7

A: 1 2 3 4 5
B: 5 4 3 2 1
C: 1 3 5 7 9
```Xử lý: 

| Ngăn xếp | Sách | Có hiệu lực? | trả lời sau bước | 
| --- | --- | --- | --- | 
| A | 1 | Có | 1 | 
| A | 2 | Có | 3 | 
| A | 3 | Có | 3 | 
| A | 4 | Có | 7 | 
| A | 5 | Có | 7 | 
| B | 5 | Có | 7 | 
| B | 4 | Có | 7 | 
| B | 3 | Có | 7 | 
| B | 2 | Có | 7 | 
| B | 1 | Có | 7 | 
| C | 1 | Có | 7 | 
| C | 3 | Có | 7 | 
| C | 5 | Có | 7 | 
| C | 7 | Có | 7 | 
| C | 9 | Không | dừng lại | 

Giá trị cuối cùng: 

| trả lời | x | Kết quả | 
| --- | --- | --- | 
| 7 | 7 | Có | 

Ví dụ này cho thấy rằng việc gặp phải một cuốn sách không hợp lệ chỉ chặn phần còn lại của ngăn xếp đó. Mọi thứ trước khi nó vẫn có thể sử dụng được. 

### Ví dụ 2 

đầu vào:```
n = 2, x = 2

A: 3 2
B: 0 0
C: 0 0
```Xử lý: 

| Ngăn xếp | Sách | Có hiệu lực? | trả lời sau bước | 
| --- | --- | --- | --- | 
| A | 3 | Không | 0 | 
| B | 0 | Có | 0 | 
| B | 0 | Có | 0 | 
| C | 0 | Có | 0 | 
| C | 0 | Có | 0 | 

Giá trị cuối cùng: 

| trả lời | x | Kết quả | 
| --- | --- | --- | 
| 0 | 2 | Không | 

Cuốn sách đầu tiên trong ngăn xếp A chứa một bit bị cấm. Vì không thể loại bỏ nó nên giá trị hữu ích`2`bên dưới không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cuốn sách được kiểm tra tối đa một lần | 
| Không gian | O(1) | Chỉ có một vài biến ngoài mảng đầu vào | 

Tổng số sách trong tất cả các trường hợp thử nghiệm nhiều nhất là`3 × 10^5`, do đó quá trình quét tuyến tính dễ dàng nằm gọn trong giới hạn thời gian một giây. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ các ngăn xếp đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, x = map(int, input().split())

        stacks = [
            list(map(int, input().split())),
            list(map(int, input().split())),
            list(map(int, input().split()))
        ]

        ans = 0

        for stack in stacks:
            for v in stack:
                if (v | x) == x:
                    ans |= v
                else:
                    break

        out.append("Yes" if ans == x else "No")

    return "\n".join(out)

# provided sample
assert run(
"""3
5 7
1 2 3 4 5
5 4 3 2 1
1 3 5 7 9
5 2
3 2 3 4 5
5 4 3 2 1
3 3 5 7 9
3 0
1 2 3
3 2 1
2 2 2
"""
) == "Yes\nNo\nYes"

# minimum size, already equal to zero
assert run(
"""1
1 0
0
0
0
"""
) == "Yes"

# blocked useful value below invalid book
assert run(
"""1
2 2
3 2
0 0
0 0
"""
) == "No"

# all equal values
assert run(
"""1
3 7
7 7 7
7 7 7
7 7 7
"""
) == "Yes"

# need contributions from multiple stacks
assert run(
"""1
1 7
1
2
4
"""
) == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`x = 0`, tất cả sách không | Có | Đọc không hoặc nhiều sách đều có tác dụng | 
| Giá trị hữu ích của khối sách hàng đầu không hợp lệ bên dưới | Không | Xử lý đúng khả năng truy cập ngăn xếp | 
| Tất cả các giá trị bằng`x`| Có | Hoạt động OR lặp đi lặp lại | 
| Bit được chia thành ba ngăn xếp | Có | Kết hợp đóng góp từ các ngăn xếp khác nhau | 

## Vỏ cạnh 

### Giá trị mục tiêu bằng 0 

đầu vào:```
1
1 0
5
7
9
```Mọi cuốn sách hàng đầu đều không hợp lệ vì mỗi cuốn đều chứa những thông tin không có trong`0`. 

Thuật toán ngay lập tức dừng trên cả ba ngăn xếp.`ans`còn lại`0`, bằng`x`, vậy câu trả lời là`"Yes"`. 

Điều này tương ứng với việc không đọc sách. 

### Cuốn sách hữu ích bị ẩn bên dưới một cuốn sách không hợp lệ 

đầu vào:```
1
2 2
3 2
0 0
0 0
```Cuốn sách đầu tiên trong ngăn xếp A là`3`(`11₂`), có chứa bit bị cấm. Quá trình xử lý ngăn xếp A dừng ngay lập tức. 

giá trị`2`bên dưới nó không bao giờ được kiểm tra vì nó không thể truy cập được theo bất kỳ chuỗi hợp lệ nào. 

HOẶC cuối cùng là`0`, vậy câu trả lời là`"No"`. 

### Để đạt x cần có nhiều ngăn xếp 

đầu vào:```
1
1 7
1
2
4
```Sản lượng chế biến:```
ans = 0
ans |= 1 -> 1
ans |= 2 -> 3
ans |= 4 -> 7
```HOẶC cuối cùng bằng`7`, vậy câu trả lời là`"Yes"`. 

Điều này chứng tỏ rằng thuật toán tập hợp chính xác các bit từ tất cả các ngăn xếp có thể truy cập và kết hợp chúng thông qua OR.
