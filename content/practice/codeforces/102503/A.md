---
title: "CF 102503A - Vincent Người Lớn"
description: "Chúng ta có bốn người có chiều cao v, a, r và p. Chúng ta phải chọn chính xác ba người trong số họ và xếp ba người đó lại với nhau. Chiều cao kết quả là tổng của ba chiều cao riêng lẻ của chúng. Tàu lượn siêu tốc chấp nhận người kết quả nếu tổng đó ít nhất là h."
date: "2026-08-09T19:06:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "A"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 520
verified: true
draft: false
---

[CF 102503A - Vincent Adultman](https://codeforces.com/problemset/problem/102503/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có bốn người có chiều cao`v`,`a`,`r`, Và`p`. Chúng ta phải chọn chính xác ba người trong số họ và xếp ba người đó lại với nhau. Chiều cao kết quả là tổng của ba chiều cao riêng lẻ của chúng. Tàu lượn siêu tốc chấp nhận người kết quả nếu số tiền đó ít nhất là`h`. 

Nhiệm vụ là xác định xem ít nhất một trong bốn nhóm ba có thể đạt được chiều cao yêu cầu hay không. Nếu một nhóm như vậy tồn tại, chúng tôi in`WAW`; nếu không, chúng tôi in`AWW`. 

Đầu vào chứa bốn chiều cao trên bốn dòng đầu tiên, theo sau là chiều cao tối thiểu bắt buộc`h`. Mỗi giá trị nằm trong khoảng từ 12 đến 150, vì vậy các giá trị rất nhỏ và số học số nguyên là quá đủ. Quan trọng hơn, có đúng bốn người, nghĩa là chỉ có`C(4,3) = 4`các nhóm có thể kiểm tra Ngay cả một tìm kiếm toàn diện trực tiếp cũng thực hiện một số lượng bổ sung và so sánh liên tục, do đó, giới hạn thời gian 1 giây không có gì hạn chế. 

Có một số trường hợp biên có thể gây ra việc triển khai không chính xác nếu điều kiện được viết một cách bất cẩn. Đầu tiên, đạt chính xác chiều cao cần thiết là đủ. Ví dụ, với độ cao`12, 12, 12, 20`Và`h = 36`, chọn ba người có chiều cao`12, 12, 12`đưa ra chính xác`36`, vì vậy đầu ra đúng là`WAW`. sử dụng`>`thay vì`>=`sẽ in sai`AWW`. 

Trường hợp thứ hai xảy ra khi nhóm thành công duy nhất có người cao nhất. Ví dụ, với độ cao`12, 12, 12, 20`Và`h = 44`, tổng của ba độ cao lớn nhất là`44`, vậy câu trả lời là`WAW`. Việc triển khai chỉ kiểm tra ba giá trị đầu vào đầu tiên sẽ bỏ lỡ nhóm hợp lệ. 

Trường hợp thứ ba là ngay cả ba người cao nhất cũng quá lùn. Với độ cao`20, 20, 20, 20`Và`h = 61`, mọi nhóm có thể đều có chiều cao`60`, vậy câu trả lời là`AWW`. Thực tế là bốn người cùng nhau sẽ đạt được`80`không giúp được gì, vì người được hình thành phải chứa đúng ba người. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu có thể chỉ cần liệt kê từng nhóm ba người, cộng chiều cao của họ và kiểm tra xem tổng đó có ít nhất là`h`. Điều này hoàn toàn chính xác vì mọi lựa chọn hợp pháp đều được xem xét. Với bốn người thì có chính xác`C(4,3) = 4`những nhóm như vậy, vì vậy trường hợp xấu nhất cần có bốn phép cộng và bốn phép so sánh, với tổng chi phí không đổi cho việc liệt kê. 

Thực tế không có lý do gì mà phương pháp brute-force này trở nên quá chậm đối với vấn đề đã cho. Kích thước đầu vào được cố định ở bốn người, vì vậy thời gian chạy của nó là`O(1)`. Trong một phiên bản tổng quát với`n`mọi người, việc kiểm tra từng bộ ba sẽ mất`O(n^3)`, có thể trở nên đắt đỏ. Tuy nhiên, đối với vấn đề Codeforce cụ thể này,`n = 4`, làm cho phương pháp đầy đủ vừa đơn giản vừa được chấp nhận hoàn toàn. 

Chúng ta có thể thực hiện quan sát tương tự thậm chí còn gọn gàng hơn. Trong số tất cả các nhóm ba người, nhóm có số tiền lớn nhất có thể bao gồm ba người cao nhất. Nếu ba người đó không thể với tới`h`, không nhóm nào khác có thể. Nếu họ có thể, nhóm đó chính là một lựa chọn hợp lệ. Do đó, việc sắp xếp bốn độ cao và kiểm tra ba giá trị lớn nhất cũng giải quyết được vấn đề trong thời gian không đổi, mặc dù việc sắp xếp là không cần thiết. 

Việc liệt kê trực tiếp được ưu tiên hơn vì nó phản ánh chính xác định nghĩa của nhiệm vụ và tránh đưa ra một thao tác mà vấn đề không cần. Cái nhìn sâu sắc quan trọng là không gian tìm kiếm chỉ chứa bốn lựa chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(1)`cho bốn người |`O(1)`| Đã chấp nhận | 
| Kiểm tra ba cao nhất |`O(1)`cho bốn người |`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn chiều cao thành một mảng và đọc chiều cao cần thiết`h`. Việc giữ các chiều cao cùng nhau giúp bạn dễ dàng suy luận về mọi nhóm ba người có thể có. 
2. Liệt kê mọi sự kết hợp của ba vị trí riêng biệt trong số bốn độ cao. Chỉ có bốn kết hợp, vì vậy mọi nhóm hợp pháp đều có thể được kiểm tra trực tiếp. 
3. Với mỗi sự kết hợp, hãy tính tổng ba chiều cao của nó và so sánh với`h`. Sử dụng`>=`bởi vì một người có chiều cao chính xác bằng chiều cao tối thiểu cho phép có thể đi xe. 
4. Nếu bất kỳ số tiền nào ít nhất`h`, in ngay`WAW`. Tìm một nhóm thành công là đủ vì câu hỏi hỏi liệu có tồn tại ít nhất một nhóm hợp lệ hay không. 
5. Nếu cả bốn kết hợp đều thất bại trong việc so sánh, hãy in`AWW`. Tại thời điểm đó, mọi nhóm ba khả năng đều đã được kiểm tra, do đó không có sự sắp xếp hợp lệ nào tồn tại. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi kiểm tra bất kỳ số lượng kết hợp nào, mọi kết hợp được kiểm tra đều được phân loại chính xác là đủ cao hoặc quá ngắn. Thuật toán kiểm tra tất cả bốn nhóm có thể có của ba. Nếu nó in`WAW`, nó đã tìm được một nhóm có tổng ít nhất là`h`, vậy câu trả lời là hợp lệ. Nếu nó in`AWW`, mọi nhóm có thể có tổng dưới đây`h`, vì vậy không tồn tại nhóm hợp lệ. Vì thuật toán xem xét mọi lựa chọn hợp pháp nên nó không thể bỏ sót một giải pháp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

v = int(input())
a = int(input())
r = int(input())
p = int(input())
h = int(input())

heights = [v, a, r, p]

for i in range(4):
    for j in range(i + 1, 4):
        for k in range(j + 1, 4):
            if heights[i] + heights[j] + heights[k] >= h:
                print("WAW")
                sys.exit()

print("AWW")
```Bốn độ cao được lưu trữ trong một danh sách để các vòng lặp kết hợp có thể hoạt động đồng đều. Ba vòng lặp lồng nhau sử dụng`i < j < k`, điều này đảm bảo rằng mỗi nhóm ba người được xem xét chính xác một lần và không có người nào được chọn hai lần. 

Việc so sánh sử dụng`>= h`, phù hợp với yêu cầu chiều cao của người được định hình phải đúng ngưỡng. As soon as a valid triple is found,`sys.exit()`kết thúc chương trình vì việc kiểm tra các bộ ba còn lại không thể thay đổi câu trả lời. 

Không có vấn đề tràn số nguyên trong Python và ngay cả trong các ngôn ngữ có kiểu số nguyên có chiều rộng cố định, tổng lớn nhất có thể ở đây chỉ là`150 + 150 + 150 = 450`. 

Đầu vào chỉ chứa một ca kiểm thử, do đó không có vòng lặp ca kiểm thử bên ngoài. Người được yêu cầu`input = sys.stdin.readline`form is still used for efficient and standard competitive-programming input handling.

 ## Ví dụ đã hoạt động 

### Mẫu 1 

Bốn chiều cao đều là`20`, và độ cao cần tìm là`61`. Mỗi nhóm ba người có tổng bằng nhau. 

|`i`|`j`|`k`| Độ cao đã chọn | Sum |`sum >= h`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 20, 20, 20 | 60 | No |
 | 0 | 1 | 3 | 20, 20, 20 | 60 | Không | 
| 0 | 2 | 3 | 20, 20, 20 | 60 | Không | 
| 1 | 2 | 3 | 20, 20, 20 | 60 | Không | 

Cả bốn nhóm pháp đều thất bại vì`60 < 61`. The algorithm reaches the final`print("AWW")`, tạo ra đầu ra chính xác. 

### Mẫu 2 

Độ cao là`24, 55, 42, 69`, với`h = 143`. 

|`i`|`j`|`k`| Độ cao đã chọn | Tổng hợp |`sum >= h`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 24, 55, 42 | 121 | Không | 
| 0 | 1 | 3 | 24, 55, 69 | 148 | Yes |

 Nhóm thứ hai đã đạt được`143`, do đó thuật toán in`WAW`và thoát ra mà không cần kiểm tra hai kết hợp còn lại. Điều này chứng tỏ tại sao thuật toán có thể dừng ngay khi tìm thấy một nhóm hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(1)`| Có chính xác bốn bộ ba để kiểm tra. | 
| Không gian |`O(1)`| Thuật toán chỉ lưu trữ bốn độ cao và số lượng biến vòng lặp không đổi. | 

Các ràng buộc cực kỳ nhỏ và thuật toán chỉ thực hiện một số phép cộng và so sánh số nguyên. Nó thoải mái trong cả giới hạn thời gian 1 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    heights = [int(input()), int(input()), int(input()), int(input())]
    h = int(input())

    for i in range(4):
        for j in range(i + 1, 4):
            for k in range(j + 1, 4):
                if heights[i] + heights[j] + heights[k] >= h:
                    return "WAW"

    return "AWW"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("20\n20\n20\n20\n61\n") == "AWW", "sample 1"
assert run("24\n55\n42\n69\n143\n") == "WAW", "sample 2"

# Minimum-size values, and exactly reaching the threshold.
assert run("12\n12\n12\n12\n36\n") == "WAW", "minimum values at boundary"

# Maximum-size values, with a threshold that is exactly reachable.
assert run("150\n150\n150\n12\n450\n") == "WAW", "maximum values at boundary"

# All groups are one unit short of the threshold.
assert run("20\n20\n20\n20\n61\n") == "AWW", "just below threshold"

# The only successful group contains the fourth input value.
assert run("12\n12\n12\n20\n44\n") == "WAW", "valid group uses fourth person"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`12 12 12 12, h = 36`|`WAW`| Giá trị tối thiểu và ngưỡng chính xác | 
|`150 150 150 12, h = 450`|`WAW`| Giá trị tối đa và ngưỡng chính xác | 
|`20 20 20 20, h = 61`|`AWW`| Ba người vừa dưới yêu cầu | 
|`12 12 12 20, h = 44`|`WAW`| Một nhóm thành công phải có người thứ 4 | 

## Vỏ cạnh 

Ranh giới bình đẳng được xử lý bởi`>=`so sánh. Đối với đầu vào```
12
12
12
12
36
```bộ ba đầu tiên có tổng`12 + 12 + 12 = 36`. Từ`36 >= 36`, thuật toán trả về ngay`WAW`. Một triển khai sử dụng`>`sẽ từ chối trường hợp này một cách không chính xác. 

Trường hợp ngôi thứ tư là cần thiết sẽ được xử lý vì các vòng lặp lồng nhau không đặc quyền cho bất kỳ vị trí đầu vào nào. Vì```
12
12
12
20
44
```ba bộ ba đầu tiên có tổng dưới đây`44`, nhưng sự kết hợp chứa độ cao`12, 12, 20`đạt tới`44`. Khi các vòng lặp đạt được chỉ số`1, 2, 3`, điều kiện thành công và đầu ra là`WAW`. 

Trường hợp bốn người là đủ nhưng ba người thì không cũng được xử lý đúng. Với```
20
20
20
20
61
```mỗi bộ ba hợp pháp có chiều cao`60`. Thuật toán không bao giờ xem xét cả bốn người cùng nhau, vì nhiệm vụ chỉ cho phép ba người trong người được hình thành. Mỗi bộ ba đều thất bại, vì vậy đầu ra là`AWW`. 

Cuối cùng, các giá trị tối đa không yêu cầu bất kỳ xử lý đặc biệt nào. Với```
150
150
150
12
450
```ba người đầu tiên đã đến nơi`450`, chính xác là chiều cao cần thiết. Thuật toán in`WAW`và số học số nguyên của Python xử lý tổng trực tiếp mà không có bất kỳ lo ngại nào về tràn. 

Một bài xã luận ngắn hơn cũng có thể sử dụng quan sát thậm chí còn đơn giản hơn rằng chỉ có ba người cao nhất mới quan trọng, nhưng phiên bản đầy đủ được cho là rõ ràng hơn cho vấn đề này vì chỉ có thể có bốn bộ ba.
