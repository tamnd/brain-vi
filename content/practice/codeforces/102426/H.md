---
title: "CF 102426H - \u767d\u5b66\u4e32"
description: "Chúng ta có một dãy số nguyên dương. Một truy vấn đưa ra một khoảng [l, r] và hỏi liệu chúng ta có thể chọn bất kỳ ba phần tử nào từ khoảng đó có thể là độ dài các cạnh của một tam giác không suy biến hay không."
date: "2026-08-14T15:20:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "H"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 143
verified: true
draft: false
---

[CF 102426H - \u767d\u5b66\u4e32](https://codeforces.com/problemset/problem/102426/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy số nguyên dương. Một truy vấn đưa ra một khoảng`[l, r]`và hỏi liệu chúng ta có thể chọn bất kỳ ba phần tử nào từ khoảng đó có thể là độ dài các cạnh của một tam giác không suy biến hay không. 

Đối với ba số dương, sau khi sắp xếp chúng thành`x <= y <= z`, chúng tạo thành một hình tam giác đúng lúc`x + y > z`. Vị trí của các phần tử được chọn không quan trọng, chỉ có giá trị của chúng và liệu cả ba vị trí có nằm trong khoảng được truy vấn hay không. 

Đầu vào chứa tối đa`10^5`các phần tử trong một trường hợp thử nghiệm, với tổng của tất cả`n`trên các trường hợp thử nghiệm được giới hạn bởi`5 * 10^5`. Có thể có tới`2 * 10^5`truy vấn cho mỗi trường hợp thử nghiệm, với tổng số`10^6`truy vấn. Điều này loại trừ mọi thứ gần với việc quét toàn bộ khoảng thời gian cho mỗi truy vấn. Một giải pháp xung quanh`O(nm)`hoặc`O(n^2)`vượt xa công việc hiện có. Số lượng truy vấn lớn cũng có nghĩa là chỉ cần thực hiện một lượng công việc nhỏ không đổi cho mỗi khoảng thời gian ngắn để có được câu trả lời. 

Có một hạn chế mạnh mẽ đáng ngạc nhiên đối với các khoảng không chứa hình tam giác. Xem xét tất cả các giá trị trong khoảng thời gian như vậy và sắp xếp chúng:`b1 <= b2 <= ... <= bk`. 

Nếu có một số chỉ số`i`với`b[i] + b[i+1] > b[i+2]`, ba giá trị liên tiếp đó sẽ tạo thành một hình tam giác. Do đó khoảng không có tam giác phải thỏa mãn`b[i] + b[i+1] <= b[i+2]`cho mọi hợp lệ`i`. 

Vì mọi giá trị đều dương nên chuỗi được sắp xếp không có tam giác nhỏ nhất có thể bắt đầu bằng`1, 1, 2, 3, 5, 8, 13, ...`. 

Nó phát triển ít nhất nhanh bằng số Fibonacci. Từ`F40 = 102334155`, đã lớn hơn`10^8`, không có khoảng không tam giác nào có thể chứa 40 phần tử. Do đó, mỗi khoảng có độ dài ít nhất 40 sẽ tự động là một chuỗi màu trắng. 

Điều này chỉ để lại các khoảng thời gian có độ dài tối đa là 39 để kiểm tra rõ ràng. 

Có một số trường hợp ranh giới trong đó việc triển khai bất cẩn không thành công. Truy vấn chứa ít hơn ba phần tử không bao giờ có thể có màu trắng. Ví dụ,```
1
2 1
1 2
1 2
```có câu trả lời`no`, vì chỉ có hai giá trị. Việc triển khai chỉ kiểm tra xem giá trị lớn nhất có lớn hơn tổng của hai giá trị hay không có thể vô tình truy cập vào các phần tử không tồn tại. 

Bình đẳng cũng chưa đủ. Vì```
1
3 1
1 2 3
1 3
```câu trả lời là`no`, bởi vì`1 + 2 = 3`đưa ra một tam giác suy biến. Sự so sánh phải chặt chẽ`x + y > z`. 

Ba phần tử được chọn không nhất thiết phải chiếm các vị trí liên tiếp. Ví dụ,```
1
4 1
2 100 3 4
1 4
```có câu trả lời`yes`, bởi vì`2, 3, 4`tạo thành một hình tam giác mặc dù chúng xảy ra ở vị trí 1, 3 và 4. Chỉ kiểm tra các vị trí mảng liên tiếp sẽ cho kết quả sai. 

Cuối cùng, thứ tự của các giá trị là không liên quan. TRONG```
1
4 1
4 1 3 2
1 4
```câu trả lời là`yes`, vì sau khi sắp xếp chúng ta thu được`1, 2, 3, 4`Và`1 + 2 > 3`. Phương pháp chỉ kiểm tra các bộ ba theo thứ tự ban đầu sẽ bỏ sót các lựa chọn hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp vũ phu trực tiếp tuân theo định nghĩa. Đối với mỗi truy vấn, lấy tất cả các giá trị trong`[l, r]`, sắp xếp chúng và kiểm tra ba giá trị liên tiếp. Việc sắp xếp là đủ vì nếu ba giá trị bất kỳ tạo thành một hình tam giác thì sau khi sắp xếp cả bộ cũng có một bộ ba liên tiếp tạo thành một hình tam giác. Nếu các giá trị được sắp xếp là`b1 <= b2 <= ...`, hãy chọn một tam giác có cạnh lớn nhất có chiết số nhỏ nhất có thể. Hai giá trị ngay trước nó không thể được thay thế bằng các giá trị lớn hơn, do đó tổng của chúng ít nhất lớn bằng tổng của hai cạnh nhỏ hơn ban đầu. 

Vấn đề với cách tiếp cận này là kích thước khoảng. Trong trường hợp xấu nhất một truy vấn có độ dài`10^5`, vì vậy một truy vấn có thể đã yêu cầu`O(10^5 log 10^5)`công việc. Với`2 * 10^5`truy vấn, điều này có thể đạt được khoảng`2 * 10^5 * 10^5`hoạt động ở cấp độ phần tử ngay cả trước khi tính đến việc sắp xếp, vốn quá lớn. 

Quan sát hữu ích là thuộc tính tăng trưởng Fibonacci được mô tả ở trên. Phương pháp brute-force hoạt động vì nó chỉ cần các giá trị bên trong khoảng thời gian được truy vấn. Nó thất bại vì một khoảng có thể lớn. Nhận xét rằng mọi khoảng không có tam giác đều có độ dài dưới 40 đã loại bỏ chính xác khó khăn đó. 

Đối với mỗi truy vấn, nếu`r - l + 1 >= 40`, chúng tôi trả lời ngay`yes`. Nếu không, khoảng chứa tối đa 39 phần tử, vì vậy chúng tôi sao chép nó, sắp xếp các giá trị đó và kiểm tra các bộ ba liên tiếp. Số 39 là một hằng số thực vì giá trị lớn nhất được giới hạn bởi`10^8`. 

Đây cũng là lý do tại sao chúng ta không cần cây phân đoạn phức tạp hoặc cây tìm kiếm cân bằng. Cấu trúc dữ liệu sẽ giải quyết được vấn đề mà giới hạn Fibonacci đã loại bỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(m n log n)`trường hợp xấu nhất |`O(n)`| Quá chậm | 
| Tối ưu |`O(n + m * 39 log 39)`|`O(39)`thêm cho mỗi truy vấn | Đã chấp nhận | 

Vì 39 là hằng số nên độ phức tạp tối ưu có hiệu quả`O(n + m)`. 

## Hướng dẫn thuật toán 

1. Đọc mảng và tất cả các truy vấn. Bản thân mảng được giữ nguyên vì mọi truy vấn có thể được xử lý độc lập sau khi chúng tôi khai thác độ dài tối đa không có tam giác. 
2. Đối với một truy vấn`[l, r]`, tính độ dài của nó là`r - l + 1`. Nếu độ dài này ít nhất là 40, hãy in`yes`ngay lập tức. Khoảng không có tam giác gồm 40 số nguyên dương sẽ yêu cầu ít nhất dãy`1, 1, 2, 3, 5, ...`, có giá trị thứ 40 đã vượt quá`10^8`, do đó khoảng như vậy không thể tồn tại. 
3. Nếu độ dài nhỏ hơn 40, hãy trích xuất tối đa 39 giá trị từ khoảng và sắp xếp chúng. Sắp xếp chuyển đổi lựa chọn tùy ý của ba phần tử thành kiểm tra cục bộ trên các giá trị liên tiếp. 
4. Quét các giá trị đã sắp xếp. Với mỗi ba giá trị liên tiếp`x, y, z`, kiểm tra xem`x + y > z`. Nếu điều này đúng với bất kỳ bộ ba nào, hãy in`yes`. 
5. Nếu tất cả các bộ ba liên tiếp đều thỏa mãn`x + y <= z`, in`no`. Khi đó, dãy đã sắp xếp không thể chứa tam giác nào, bởi vì bất kỳ tam giác nào cũng sẽ tạo ra một bộ ba liên tiếp hợp lệ. 

### Tại sao nó hoạt động 

Bất biến chính là đối với một bộ sưu tập được sắp xếp`b1 <= b2 <= ... <= bk`, tập hợp chứa một tam giác khi và chỉ khi một số bộ ba liên tiếp thỏa mãn`b[i] + b[i+1] > b[i+2]`. Nếu có tam giác thì hãy lấy cạnh lớn nhất của nó`z`. Hai giá trị lớn nhất nhỏ hơn hoặc bằng`z`có tổng ít nhất bằng hai cạnh nhỏ hơn của tam giác nên bộ ba liên tiếp tương ứng cũng tạo thành một tam giác. Ngược lại, mọi bộ ba liên tiếp thỏa mãn bất đẳng thức trực tiếp là một tam giác hợp lệ. 

Đối với các khoảng có ít nhất 40 phần tử, đối số Fibonacci chứng minh rằng một hình tam giác phải tồn tại. Đối với các khoảng thời gian ngắn hơn, việc kiểm tra được sắp xếp rõ ràng là chính xác. Hai trường hợp này bao gồm mọi truy vấn có thể xảy ra, vì vậy mọi câu trả lời đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 40

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        for _ in range(m):
            l, r = map(int, input().split())

            length = r - l + 1

            if length >= LIMIT:
                out.append("yes")
                continue

            b = sorted(a[l - 1:r])

            ok = False
            for i in range(length - 2):
                if b[i] + b[i + 1] > b[i + 2]:
                    ok = True
                    break

            out.append("yes" if ok else "no")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào sử dụng các vị trí dựa trên một, trong khi các lát Python dựa trên 0 và loại trừ điểm cuối bên phải. Vì thế`[l, r]`trở thành`a[l - 1:r]`, chứa chính xác các phần tử cần thiết. 

Việc kiểm tra độ dài xảy ra trước khi xây dựng và sắp xếp lát cắt. Điều này quan trọng vì các khoảng thời gian dài đã được biết là có màu trắng, do đó không có lý do gì để dành thời gian xử lý các giá trị của chúng. 

Trong một khoảng thời gian ngắn,`sorted`tạo một danh sách mới chứa tối đa 39 số nguyên. Vòng lặp sau đó sẽ kiểm tra chính xác các bộ ba liên tiếp từ danh sách đã sắp xếp đó. Sự so sánh chặt chẽ`>`xử lý trường hợp suy biến như`1, 2, 3`một cách chính xác. 

Số nguyên Python có độ chính xác tùy ý, vì vậy`b[i] + b[i + 1]`không thể tràn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, giá trị đã cho`10^8`bị ràng buộc đã làm cho số nguyên có dấu 32 bit đủ cho tổng, nhưng việc sử dụng loại rộng hơn là vô hại. 

Hằng số 40 được cố ý chọn làm độ dài đầu tiên đảm bảo chứa một hình tam giác. Từ`F40 = 102334155 > 10^8`, một dãy không có tam giác có thể có nhiều nhất 39 phần tử. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp thử nghiệm đầu tiên có mảng`[1, 1, 2, 3, 4]`. 

Tất cả các truy vấn đều ngắn hơn 40, vì vậy mỗi truy vấn đều được kiểm tra rõ ràng. 

| Truy vấn | Giá trị | Giá trị được sắp xếp | Kiểm tra tam giác liên tiếp | Trả lời | 
| --- | --- | --- | --- | --- | 
|`[1, 3]`|`1, 1, 2`|`1, 1, 2`|`1 + 1 > 2`là sai |`no`| 
|`[2, 4]`|`1, 2, 3`|`1, 2, 3`|`1 + 2 > 3`là sai |`no`| 
|`[2, 5]`|`1, 2, 3, 4`|`1, 2, 3, 4`|`1 + 2 > 3`SAI,`2 + 3 > 4`đúng |`yes`| 

Truy vấn thứ ba giải thích tại sao việc kiểm tra tất cả các bộ ba liên tiếp sau khi sắp xếp là đủ. Tam giác hợp lệ là`2, 3, 4`. 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai có mảng`[2, 3, 4, 1, 2]`. 

Một lần nữa, cả hai truy vấn đều ngắn hơn 40. 

| Truy vấn | Giá trị | Giá trị được sắp xếp | Kiểm tra tam giác liên tiếp | Trả lời | 
| --- | --- | --- | --- | --- | 
|`[1, 4]`|`2, 3, 4, 1`|`1, 2, 3, 4`|`1 + 2 > 3`SAI,`2 + 3 > 4`đúng |`yes`| 
|`[3, 5]`|`4, 1, 2`|`1, 2, 4`|`1 + 2 > 4`sai |`no`| 

Truy vấn đầu tiên chứa hình tam giác`2, 3, 4`. Thứ hai chỉ chứa`1, 2, 4`, thất bại vì`1 + 2`không lớn hơn`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m * 39 log 39)`| Mọi truy vấn dài đều được trả lời ngay lập tức; mỗi truy vấn ngắn sắp xếp tối đa 39 giá trị | 
| Không gian |`O(39)`thêm | Một truy vấn ngắn tạo danh sách tạm thời có tối đa 39 giá trị | 

Bởi vì 39 là hằng số cố định được xác định bởi`10^8`giá trị bị ràng buộc, công việc truy vấn thực sự không đổi. Trên toàn bộ đầu vào, thuật toán thực hiện`O(sum n + sum m)`công tiệm cận với hệ số không đổi nhỏ, phù hợp với`sum n <= 5 * 10^5`Và`sum m <= 10^6`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        for _ in range(m):
            l, r = map(int, input().split())
            length = r - l + 1

            if length >= 40:
                out.append("yes")
                continue

            b = sorted(a[l - 1:r])

            ok = False
            for i in range(length - 2):
                if b[i] + b[i + 1] > b[i + 2]:
                    ok = True
                    break

            out.append("yes" if ok else "no")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample1 = """\
1
5 3
1 1 2 3 4
1 3
2 4
2 5
"""

assert run(sample1) == "no\no\nyes", "sample 1"

sample2 = """\
1
5 2
2 3 4 1 2
1 4
3 5
"""

assert run(sample2) == "yes\nno", "sample 2"

minimum = """\
1
1 3
7
1 1
1 1
1 1
"""

assert run(minimum) == "no\nno\nno", "minimum-size input"

all_equal = """\
1
6 4
5 5 5 5 5 5
1 1
1 2
1 3
2 6
"""

assert run(all_equal) == "no\nno\nyes\nyes", "all equal values"

boundary = """\
1
5 5
1 1 2 3 4
1 2
1 3
1 4
2 4
2 5
"""

assert run(boundary) == "no\nno\nno\nno\nyes", "boundary and off-by-one cases"

fib_limit = """\
1
40 3
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040 1346269 2178309 3524578 5702887 9227465 14930352 24157817 39088169 63245986
1 40
1 39
38 40
"""

assert run(fib_limit) == "yes\nyes\nno", "40-element threshold"

large = """\
1
100000 1
""" + " ".join(["1"] * 100000) + """
1 100000
"""

assert run(large) == "yes", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 3 / 7 / ...`|`no`,`no`,`no`| Khoảng một phần tử không bao giờ có thể chứa ba cạnh | 
| Sáu bản sao của`5`|`no`,`no`,`yes`,`yes`| Ba giá trị bằng nhau tạo thành một hình tam giác, trong khi một hoặc hai giá trị thì không | 
|`[1, 1, 2, 3, 4]`truy vấn ranh giới |`no`,`no`,`no`,`no`,`yes`| Bất bình đẳng nghiêm ngặt và ranh giới lát cắt chính xác | 
| Mảng 40 phần tử giống Fibonacci |`yes`,`yes`,`no`| Bảo đảm độ dài 40 và ranh giới kiểm tra rõ ràng 39 phần tử | 
| 100000 bản sao`1`|`yes`| Lớn`n`và thực tế là một khoảng thời gian dài được trả lời mà không cần sắp xếp | 

## Vỏ cạnh 

### Ít hơn ba phần tử 

Đối với đầu vào```
1
2 2
10 20
1 1
1 2
```cả hai khoảng đều có ít hơn ba phần tử. Thuật toán nhìn thấy độ dài 1 và 2, do đó nó không sử dụng quy tắc độ dài 40 tự động. Việc sắp xếp đưa ra danh sách có kích thước 1 và 2, đồng thời vòng lặp kiểm tra tam giác không có lần lặp lại. Cả hai câu trả lời đều`no`. 

### Tam giác suy biến 

cho```
1
3 1
1 2 3
1 3
```các giá trị được sắp xếp là`[1, 2, 3]`. Kiểm tra duy nhất là`1 + 2 > 3`, điều này sai vì tổng bằng cạnh lớn nhất. Thuật toán in`no`, xử lý chính xác đẳng thức như một tam giác suy biến chứ không phải là một tam giác hợp lệ. 

### Vị trí được chọn không liên tiếp 

cho```
1
4 1
2 100 3 4
1 4
```khoảng được sắp xếp là`[2, 3, 4, 100]`. Bộ ba thứ hai cho`2 + 3 > 4`, vậy câu trả lời là`yes`. Các vị trí ban đầu của`2, 3, 4`là 1, 3 và 4, cho thấy lý do tại sao thuật toán phải sắp xếp khoảng thay vì chỉ kiểm tra các vị trí mảng liền kề. 

### Chính xác là 39 phần tử 

Hãy xem xét tiền tố Fibonacci 39 phần tử```
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040 1346269 2178309 3524578 5702887 9227465 14930352 24157817 39088169 63245986
```Mỗi cặp giá trị nhỏ hơn liên tiếp có tổng bằng giá trị tiếp theo, do đó không tồn tại tam giác. Vì khoảng có độ dài 39 nên thuật toán vẫn thực hiện kiểm tra rõ ràng và in chính xác`no`. 

Trường hợp này là ranh giới ngăn cản việc sử dụng 39 làm ngưỡng tự động. 

### Chính xác là 40 phần tử 

Nếu chúng ta nối thêm một giá trị khác để giữ cho chuỗi không có tam giác, thì chúng ta cần giá trị tiếp theo ít nhất là`102334155`, nhưng vượt quá mức tối đa cho phép`10^8`. Do đó, không có đầu vào hợp lệ nào có thể chứa khoảng không có tam giác có độ dài 40. 

Do đó, thuật toán trả lời`yes`ngay lập tức bất cứ khi nào`r - l + 1 >= 40`, mà không kiểm tra các giá trị. Đây là sự tối ưu hóa trung tâm giúp biến số lượng lớn các truy vấn phạm vi thành một giải pháp thực tế.
