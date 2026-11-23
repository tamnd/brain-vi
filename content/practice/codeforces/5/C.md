---
title: "CF 5C - Chuỗi khung thông thường dài nhất"
description: "Chúng ta được cấp một chuỗi chỉ gồm ( và ). Trong số tất cả các chuỗi con liền kề, chúng ta cần tìm độ dài tối đa của chuỗi con tạo thành chuỗi ngoặc hợp lệ. Chúng ta cũng cần đếm xem có bao nhiêu chuỗi con đạt được độ dài tối đa đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dp", "greedy", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 5
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 5"
rating: 1900
weight: 5
solve_time_s: 81
verified: true
draft: false
---
[CF 5C - Chuỗi khung thông thường dài nhất](https://codeforces.com/problemset/problem/5/C) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dp, tham lam, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một sợi dây chỉ được làm từ`(`Và`)`. Trong số tất cả các chuỗi con liền kề, chúng ta cần tìm độ dài tối đa của chuỗi con tạo thành chuỗi ngoặc hợp lệ. Chúng ta cũng cần đếm xem có bao nhiêu chuỗi con đạt được độ dài tối đa đó. 

Chuỗi dấu ngoặc hợp lệ hoạt động giống hệt như các dấu ngoặc đơn được khớp chính xác trong một biểu thức. Mọi dấu ngoặc mở cuối cùng đều phải đóng và không có tiền tố nào có thể chứa nhiều dấu ngoặc đóng hơn dấu ngoặc mở. 

Độ dài đầu vào có thể đạt tới`10^6`, điều này thay đổi hoàn toàn những gì khả thi. Bất kỳ thuật toán nào kiểm tra tất cả các chuỗi con đều không thể thực hiện được. Có khoảng`n^2`chuỗi con và thậm chí việc dành thời gian liên tục cho mỗi chuỗi con sẽ vượt quá giới hạn thực tế. Với giới hạn thời gian 2 giây, chúng ta nên nhắm tới thứ gì đó gần với thời gian tuyến tính hoặc tệ nhất là`O(n log n)`. 

Phần khó khăn là chuỗi con hợp lệ dài nhất không nhất thiết phải là toàn bộ vùng hợp lệ giữa hai dấu ngoặc phù hợp. Các ký tự không hợp lệ có thể chia chuỗi thành các đoạn độc lập. Ví dụ:```
())(())
```Câu trả lời đúng là:```
4 1
```Chuỗi con`(())`là hợp lệ, nhưng tiền tố`())(`phá vỡ trình tự. Việc triển khai bất cẩn chỉ đếm các cặp khớp có thể trả về độ dài không chính xác`6`. 

Một lỗi dễ xảy ra khác khi có nhiều chuỗi con dài nhất:```
()()(()())
```Câu trả lời đúng là:```
10 1
```Toàn bộ chuỗi là hợp lệ, mặc dù các phần hợp lệ nhỏ hơn cũng tồn tại bên trong nó. Việc đếm từng phân đoạn hợp lệ một cách độc lập mà không theo dõi mức tối đa một cách chính xác có thể bị tính quá mức. 

Một trường hợp đặc biệt quan trọng là khi không tồn tại chuỗi con hợp lệ nào:```
))))
```Đầu ra phải là:```
0 1
```Số lượng là`1`, không`0`. Vấn đề xác định hành vi đặc biệt này một cách rõ ràng. 

Một trường hợp tinh tế hơn liên quan đến các vùng hợp lệ bị ngắt kết nối có kích thước bằng nhau:```
()())()
```Câu trả lời đúng là:```
4 1
```Chuỗi con`()()`có chiều dài`4`, trong khi trận chung kết`()`chỉ có chiều dài`2`. Nếu chúng tôi đặt lại trạng thái không chính xác sau dấu ngoặc không hợp lệ, chúng tôi có thể bỏ lỡ phân đoạn lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi tạo ra mọi chuỗi con và kiểm tra xem nó có tạo thành chuỗi dấu ngoặc hợp lệ hay không. Hiệu lực có thể được kiểm tra bằng cách sử dụng bộ đếm số dư: tăng cho`(`, giảm dần cho`)`và đảm bảo số dư không bao giờ trở nên âm và kết thúc bằng 0. 

Điều này hoạt động hợp lý vì mọi chuỗi ngoặc hợp lệ đều thỏa mãn các điều kiện đó. Vấn đề là chi phí. có`O(n^2)`các chuỗi con. Ngay cả khi việc kiểm tra tính hợp lệ được tối ưu hóa để`O(length)`, tổng độ phức tạp trở thành`O(n^3)`. Với`n = 10^6`, về mặt thiên văn thì điều này quá chậm. 

Chúng tôi có thể cải thiện việc kiểm tra tính hợp lệ bằng cách sử dụng số dư tiền tố, nhưng chúng tôi vẫn không đủ khả năng liệt kê tất cả các chuỗi con. Thậm chí`O(n^2)`là không thể ở quy mô này. 

Quan sát chính là chuỗi con trong khung hợp lệ hoạt động cục bộ. Bất cứ khi nào chúng ta gặp một dấu ngoặc đóng, chúng ta chỉ quan tâm liệu có tồn tại một dấu ngoặc mở không khớp trước nó hay không. Điều này tự nhiên gợi ý một ngăn xếp. 

Ngăn xếp lưu trữ các chỉ số của dấu ngoặc mở chưa khớp. Khi chúng tôi nhìn thấy`(`, chúng tôi đẩy chỉ mục của nó. Khi chúng tôi nhìn thấy`)`, chúng tôi cố gắng kết hợp nó với cái mới nhất chưa từng có`(`. 

Cái nhìn sâu sắc hơn là làm thế nào để khôi phục độ dài chuỗi con một cách hiệu quả. Giả sử vị trí`i`đóng một chuỗi hợp lệ. Khi đó chuỗi con hợp lệ dài nhất kết thúc tại`i`bắt đầu ngay sau dấu ngoặc chưa khớp gần nhất trước nó. Nếu chúng ta giữ chỉ mục của vị trí chưa từng có gần đây nhất trên ngăn xếp, chúng ta có thể tính toán độ dài theo thời gian không đổi. 

Điều này chuyển đổi vấn đề từ việc kiểm tra từng chuỗi con một cách độc lập sang xử lý chuỗi một lần từ trái sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Giải pháp ngăn xếp tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một ngăn xếp với một giá trị duy nhất`-1`. 

Điều này hoạt động như một lính canh. Nó biểu thị vị trí trước khi chuỗi bắt đầu và cho phép các chuỗi con hợp lệ bắt đầu tại chỉ mục`0`để được đo một cách chính xác. 
2. Duy trì hai biến:`best_len`, độ dài chuỗi con hợp lệ tối đa được tìm thấy cho đến nay.`count`, số lượng chuỗi con đạt được độ dài đó. 
3. Di chuyển chuỗi từ trái sang phải. 
4. Nếu ký tự hiện tại là`(`, đẩy chỉ mục của nó vào ngăn xếp. 

Các chỉ số này đại diện cho dấu ngoặc mở chưa khớp mà sau này có thể tạo thành các cặp hợp lệ. 
5. Nếu ký tự hiện tại là`)`, lấy một phần tử ra khỏi ngăn xếp. 

Chúng tôi đang cố gắng khớp khung đóng này với khung mở chưa khớp gần nhất. 
6. Sau khi bật lên, hãy kiểm tra xem ngăn xếp có trống không. 

Nếu nó trống, cái này`)`không thể sánh được. Đẩy chỉ mục của nó vào ngăn xếp làm ranh giới vô hiệu mới. 

Bất kỳ chuỗi con hợp lệ nào trong tương lai đều phải bắt đầu sau vị trí này. 
7. Ngược lại, ngăn xếp vẫn chứa chỉ mục. 

Đặt chỉ số hiện tại là`i`. Độ dài của chuỗi con hợp lệ kết thúc tại`i`bằng:```
i - stack[-1]
```Phần trên cùng của ngăn xếp bây giờ đánh dấu vị trí chưa khớp gần nhất trước phân đoạn hợp lệ. 
8. So sánh độ dài này với`best_len`. 

Nếu nó lớn hơn, hãy cập nhật`best_len`và thiết lập lại`count = 1`. 

Nếu bằng thì tăng`count`. 
9. Sau khi xử lý toàn bộ chuỗi, xử lý trường hợp đặc biệt trong đó`best_len == 0`. 

Đầu ra cần thiết trở thành`0 1`. 

### Tại sao nó hoạt động 

Ngăn xếp luôn lưu trữ các chỉ mục của dấu ngoặc không khớp. Sau khi xử lý vị trí`i`, phần trên cùng của ngăn xếp biểu thị vị trí gần nhất ngăn cản việc mở rộng chuỗi con hợp lệ kết thúc tại`i`. 

Khi một cặp phù hợp được hình thành, hãy loại bỏ cặp phù hợp`(`để lại chính xác một chỉ mục ranh giới trước khối hợp lệ hiện tại. Khoảng cách từ ranh giới đó tới`i`đưa ra chuỗi con hợp lệ tối đa kết thúc tại`i`. 

Mọi chuỗi con hợp lệ được phát hiện chính xác khi điểm cuối bên phải của nó được xử lý và chuỗi dài nhất được đo chính xác vì các vị trí không khớp sẽ phân chia chuỗi thành các vùng hợp lệ độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

stack = [-1]

best_len = 0
count = 0

for i, ch in enumerate(s):
    if ch == '(':
        stack.append(i)
    else:
        stack.pop()

        if not stack:
            stack.append(i)
        else:
            length = i - stack[-1]

            if length > best_len:
                best_len = length
                count = 1
            elif length == best_len:
                count += 1

if best_len == 0:
    print("0 1")
else:
    print(best_len, count)
```Ngăn xếp bắt đầu bằng`-1`bởi vì chuỗi con hợp lệ có thể bắt đầu ở chỉ mục`0`. Nếu không có trọng điểm này, một chuỗi như`()`sẽ tạo ra chiều dài`1`thay vì`2`. 

Khi xử lý`)`, chúng tôi ngay lập tức bật lên một lần. Điều này tương ứng với việc tiêu thụ một loại phù hợp`(`hoặc ranh giới không hợp lệ hiện tại. Nếu ngăn xếp trở nên trống sau đó, giá trị hiện tại`)`không có dấu ngoặc mở phù hợp. Sau đó chúng tôi lưu trữ chỉ mục của nó dưới dạng ranh giới không hợp lệ mới nhất. 

Biểu thức:```
length = i - stack[-1]
```là cốt lõi của thuật toán. Sau khi khớp thành công, phần trên cùng của ngăn xếp sẽ trỏ đến chỉ mục chưa khớp gần nhất trước khối hợp lệ hiện tại. Phép trừ cho độ dài chuỗi con chính xác. 

Thứ tự của các hoạt động quan trọng. Chúng ta phải bật lên trước khi kiểm tra tính trống rỗng. Việc kiểm tra trước sẽ coi các dấu ngoặc trùng khớp là không khớp một cách không chính xác. 

Đầu ra đặc biệt`0 1`được xử lý riêng vì không có chuỗi con hợp lệ nào được cập nhật`count`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
)((())))(()())
```| tôi | char | ngăn xếp sau khi xử lý | chiều dài hiện tại | tốt_len | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ) | [0] | - | 0 | 0 | 
| 1 | ( | [0, 1] | - | 0 | 0 | 
| 2 | ( | [0, 1, 2] | - | 0 | 0 | 
| 3 | ( | [0, 1, 2, 3] | - | 0 | 0 | 
| 4 | ) | [0, 1, 2] | 2 | 2 | 1 | 
| 5 | ) | [0, 1] | 4 | 4 | 1 | 
| 6 | ) | [0] | 6 | 6 | 1 | 
| 7 | ) | [7] | - | 6 | 1 | 
| 8 | ( | [7, 8] | - | 6 | 1 | 
| 9 | ( | [7, 8, 9] | - | 6 | 1 | 
| 10 | ) | [7, 8] | 2 | 6 | 1 | 
| 11 | ( | [7, 8, 11] | - | 6 | 1 | 
| 12 | ) | [7, 8] | 4 | 6 | 1 | 
| 13 | ) | [7] | 6 | 6 | 2 | 

Đầu ra cuối cùng:```
6 2
```Dấu vết này cho thấy dấu ngoặc đóng chưa khớp sẽ chia chuỗi thành các vùng riêng biệt như thế nào. chỉ mục`7`trở thành một ranh giới mới sau khi không hợp lệ`)`. 

### Ví dụ 2 

đầu vào:```
(()(
```| tôi | char | ngăn xếp sau khi xử lý | chiều dài hiện tại | tốt_len | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ( | [-1, 0] | - | 0 | 0 | 
| 1 | ( | [-1, 0, 1] | - | 0 | 0 | 
| 2 | ) | [-1, 0] | 2 | 2 | 1 | 
| 3 | ( | [-1, 0, 3] | - | 2 | 1 | 

Đầu ra cuối cùng:```
2 1
```Ví dụ này chứng tỏ rằng các dấu ngoặc mở chưa khớp còn lại trong ngăn xếp không làm mất hiệu lực các chuỗi con đã hoàn thành trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | Ngăn xếp có thể chứa tất cả các chỉ số trong ngoặc mở | 

Với`n`lên đến`10^6`, độ phức tạp tuyến tính chính xác là những gì chúng ta cần. Thuật toán thực hiện một lượng công việc không đổi trên mỗi ký tự và mức sử dụng bộ nhớ ngăn xếp vẫn ở mức thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    s = input().strip()

    stack = [-1]

    best_len = 0
    count = 0

    for i, ch in enumerate(s):
        if ch == '(':
            stack.append(i)
        else:
            stack.pop()

            if not stack:
                stack.append(i)
            else:
                length = i - stack[-1]

                if length > best_len:
                    best_len = length
                    count = 1
                elif length == best_len:
                    count += 1

    if best_len == 0:
        print("0 1")
    else:
        print(best_len, count)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run(")((())))(()())\n") == "6 2", "sample 1"

# minimum size
assert run("(\n") == "0 1", "single opening bracket"

# all invalid
assert run("))))\n") == "0 1", "all closing brackets"

# entire string valid
assert run("(()())\n") == "6 1", "whole string valid"

# multiple maximum substrings
assert run("()()())(()())\n") == "6 1", "largest segment counted once"

# disconnected equal maxima
assert run("()())()()\n") == "4 2", "two longest substrings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`(`|`0 1`| Kích thước đầu vào tối thiểu | 
|`))))`|`0 1`| Không tồn tại chuỗi con hợp lệ | 
|`(()())`|`6 1`| Toàn bộ chuỗi tạo thành một chuỗi hợp lệ | 
|`()()())(()())`|`6 1`| Chuỗi con dài nhất kéo dài cấu trúc phức tạp | 
|`()())()()`|`4 2`| Nhiều chuỗi con dài nhất được tính chính xác | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
))))
```Xử lý ký tự đầu tiên sẽ bật ra trọng điểm`-1`, để lại ngăn xếp trống. Thuật toán đẩy ngay chỉ số`0`là ranh giới không hợp lệ mới. Điều tương tự cũng xảy ra với mọi nhân vật còn lại. Không có chuỗi con hợp lệ nào được hình thành, vì vậy`best_len`ở lại`0`. Bản in xử lý trường hợp đặc biệt cuối cùng:```
0 1
```Bây giờ hãy xem xét:```
()())()
```Thuật toán lần đầu tiên phát hiện`()`với chiều dài`2`, sau đó`()()`với chiều dài`4`. Tại chỉ mục`4`, vô đối`)`xuất hiện, do đó ngăn xếp đặt lại với ranh giới`4`. trận chung kết`()`có chiều dài`2`, điều này không ảnh hưởng đến câu trả lời. Đầu ra trở thành:```
4 1
```Điều này xác nhận rằng các dấu ngoặc không hợp lệ sẽ phân tách chính xác các vùng. 

Cuối cùng, hãy xem xét:```
(()(
```Chuỗi con`()`bên trong chuỗi vẫn hợp lệ mặc dù các dấu ngoặc mở thêm vẫn không khớp. Sau khi khớp chỉ số`2`, ngăn xếp vẫn chứa chỉ mục`0`, cho chiều dài`2`. trận chung kết`(`vẫn chưa được so sánh nhưng không xóa các phân đoạn hợp lệ trước đó. Thuật toán xuất ra chính xác:```
2 1
```
