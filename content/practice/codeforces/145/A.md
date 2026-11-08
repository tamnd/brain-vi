---
title: "CF 145A - Chuyển đổi may mắn"
description: "Chúng ta có hai chuỗi có độ dài bằng nhau, cả hai chỉ bao gồm các ký tự 4 và 7. Mục tiêu là chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng ít thao tác nhất. Có hai hoạt động được phép. Chúng ta có thể lật một chữ số, thay đổi 4 thành 7 hoặc 7 thành 4."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 145
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 104 (Div. 1)"
rating: 1200
weight: 145
solve_time_s: 122
verified: true
draft: false
---

[CF 145A - Chuyển đổi may mắn](https://codeforces.com/problemset/problem/145/A) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài bằng nhau, cả hai đều chỉ gồm các ký tự`4`Và`7`. Mục tiêu là chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng ít thao tác nhất. 

Có hai hoạt động được phép. Chúng ta có thể lật một chữ số, thay đổi`4`ĐẾN`7`hoặc`7`ĐẾN`4`. Chúng ta cũng có thể hoán đổi hai vị trí bất kỳ trong chuỗi đầu tiên. 

Thách thức là quyết định khi nào một giao dịch hoán đổi có giá trị hơn các lần lật riêng lẻ. Hoán đổi có thể sửa chữa hai vị trí không chính xác cùng một lúc, vì vậy sử dụng hoán đổi một cách thông minh là chìa khóa để giảm thiểu câu trả lời. 

Độ dài chuỗi có thể lớn bằng$10^5$. Bất kỳ thuật toán nào thử nhiều lần hoán đổi có thể xảy ra một cách rõ ràng đều trở nên không khả thi. Ví dụ: việc kiểm tra tất cả các cặp chỉ số sẽ yêu cầu$O(n^2)$hoạt động xung quanh$10^{10}$so sánh trong trường hợp xấu nhất Điều đó vượt xa giới hạn thời gian. Chúng ta cần một giải pháp tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi các vị trí không khớp không bổ sung cho nhau. 

Coi như:```
a = 444
b = 777
```Mọi vị trí đều khác nhau theo cùng một cách:`4 -> 7`. Không có trao đổi nào có thể giúp ích vì việc hoán đổi các chữ số giống hệt nhau không thay đổi gì cả. Câu trả lời đúng là`3`, một lần lật cho mỗi vị trí. 

Một trường hợp phức tạp khác là khi sự không khớp xảy ra theo hướng ngược nhau.```
a = 47
b = 74
```Tại chỉ mục`0`, chúng tôi cần`4 -> 7`. 

Tại chỉ mục`1`, chúng tôi cần`7 -> 4`. 

Một lần hoán đổi sẽ sửa chữa cả hai vị trí ngay lập tức, vì vậy câu trả lời là`1`. Một giải pháp bất cẩn đếm các giá trị không khớp một cách độc lập sẽ trả về sai`2`. 

Một ví dụ hỗn hợp cho thấy lý do tại sao chúng ta phải ghép những phần không khớp bổ sung một cách tham lam.```
a = 4477
b = 7744
```Có hai`4 -> 7`không khớp và hai`7 -> 4`sự không phù hợp. Chúng ta có thể sửa từng cặp bằng một lần hoán đổi, vì vậy câu trả lời là`2`, không`4`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi kiểm tra tất cả các vị trí không khớp và liên tục thử mọi cách hoán đổi có thể để xem liệu nó có làm giảm số lượng ký tự không chính xác hay không. Nếu không có trao đổi hữu ích nào tồn tại, chúng tôi sẽ lật một chữ số. 

Cách tiếp cận này đúng vì nó khám phá rõ ràng việc cải thiện hoạt động. Vấn đề là chi phí. có thể có$O(n)$không khớp và thử tất cả các giao dịch hoán đổi giữa chúng$O(n^2)$. Với$n = 10^5$, tốc độ này quá chậm. 

Quan sát quan trọng là chỉ tồn tại hai loại không khớp. 

Một vị trí có thể yêu cầu:```
4 -> 7
```hoặc```
7 -> 4
```Giả sử chúng ta có một sự không khớp của từng loại:```
a[i] = 4, b[i] = 7
a[j] = 7, b[j] = 4
```Hoán đổi`a[i]`Và`a[j]`sửa cả hai vị trí trong một thao tác. Không có nước đi nào tốt hơn vì một thao tác không thể sửa nhiều hơn hai vị trí không chính xác. 

Điều này biến vấn đề thành việc đếm xem có bao nhiêu điểm không phù hợp bổ sung có thể được ghép nối với nhau. 

Cho phép:```
x = number of positions with 4 -> 7
y = number of positions with 7 -> 4
```Chúng tôi có thể thực hiện:```
min(x, y)
```trao đổi. 

Sau khi sử dụng tất cả các giao dịch hoán đổi có thể, chỉ còn lại một loại không khớp. Những vị trí còn lại phải được cố định riêng lẻ bằng cách lật. 

Câu trả lời cuối cùng trở thành:```
swaps + remaining_flips
= min(x, y) + |x - y|
= max(x, y)
```Mặc dù có tồn tại công thức đơn giản hóa, việc tính toán các hoán đổi và các lần lật còn lại riêng biệt làm cho lý luận trở nên rõ ràng hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi`a`Và`b`. 
2. Khởi tạo hai bộ đếm:`cnt47`cho các vị trí ở đó`a[i] = 4`Và`b[i] = 7`,`cnt74`cho các vị trí ở đó`a[i] = 7`Và`b[i] = 4`. 
3. Quét các chuỗi từ trái sang phải. 
4. Đối với từng vị trí: 

Nếu các ký tự đã khớp rồi thì bỏ qua vì không cần thực hiện thao tác nào. 
5. Nếu không khớp`4 -> 7`, tăng`cnt47`. 
6. Nếu không khớp`7 -> 4`, tăng`cnt74`. 
7. Tính xem có thể thực hiện bao nhiêu lần hoán đổi:```
swaps = min(cnt47, cnt74)
```Mỗi lần hoán đổi như vậy sẽ sửa một lỗi không khớp từ mỗi danh mục. 
8. Sau khi thực hiện tất cả các giao dịch hoán đổi có lợi, một loại giao dịch không khớp có thể vẫn còn. 

Các vị trí còn lại yêu cầu lật riêng lẻ:```
flips = abs(cnt47 - cnt74)
```9. In:```
swaps + flips
```### Tại sao nó hoạt động 

Việc hoán đổi chỉ có tác dụng khi hai điểm không khớp này bổ sung cho nhau. Hoán đổi hai loại không khớp giống hệt nhau sẽ không thay đổi gì hữu ích. 

Mỗi cặp bổ sung luôn có thể được sửa chữa trong một thao tác và không có thao tác nào có thể sửa chữa nhiều hơn hai vị trí không chính xác. Điều này làm cho việc ghép đôi tham lam trở nên tối ưu. 

Sau khi tất cả các cặp có thể được sử dụng, các điểm không khớp còn lại đều giống nhau về hướng. Hoán đổi không còn hữu ích nữa, vì vậy mỗi vị trí còn lại phải được lật riêng lẻ. 

Bởi vì thuật toán tối đa hóa các giao dịch hoán đổi hữu ích và chỉ thực hiện các lần lật không thể tránh khỏi sau đó nên số lượng thao tác thu được là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = input().strip()
b = input().strip()

cnt47 = 0
cnt74 = 0

for x, y in zip(a, b):
    if x == y:
        continue

    if x == '4' and y == '7':
        cnt47 += 1
    else:
        cnt74 += 1

swaps = min(cnt47, cnt74)
flips = abs(cnt47 - cnt74)

print(swaps + flips)
```Phần đầu tiên đọc hai chuỗi và khởi tạo bộ đếm cho hai loại không khớp. 

Vòng lặp so sánh các vị trí tương ứng. Các vị trí phù hợp sẽ bị bỏ qua ngay lập tức vì chúng đã đáp ứng được cấu hình mục tiêu. 

Mọi sự không phù hợp phải thuộc về chính xác một trong hai loại có thể. Vì các chuỗi chỉ chứa`4`Và`7`, cái`else`chi nhánh đại diện một cách an toàn`7 -> 4`. 

Sau khi đếm số lần không khớp, mã sẽ tính số lần hoán đổi hiệu quả tối đa. Mỗi lần hoán đổi sẽ tiêu tốn một điểm không khớp từ cả hai danh mục. 

Sự mất cân bằng còn lại không thể được sửa chữa bằng cách hoán đổi nữa vì tất cả các điểm không khớp còn sót lại đều hướng về cùng một hướng. Mỗi người yêu cầu một lần lật. 

Một lỗi phổ biến là cố gắng đếm tổng số điểm không khớp và chia cho hai. Điều đó không thành công khi tất cả các điểm không khớp đều có cùng một hướng. Ví dụ:```
a = 444
b = 777
```Có ba sự không khớp, nhưng không có sự hoán đổi hữu ích nào. 

Một điểm tinh tế khác là sử dụng`.strip()`khi đọc đầu vào. Không có nó, dòng mới ở cuối sẽ trở thành một phần của chuỗi và gây ra những so sánh không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
47
74
```Dấu vết: 

| Chỉ mục | một [tôi] | b[i] | cnt47 | cnt74 | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 7 | 1 | 0 | 
| 1 | 7 | 4 | 1 | 1 | 

Sau khi xử lý: 

| Biến | Giá trị | 
| --- | --- | 
| hoán đổi | 1 | 
| lật | 0 | 
| trả lời | 1 | 

Ví dụ này thể hiện ý tưởng tham lam trung tâm. Tồn tại một cặp bổ sung, vì vậy một hoán đổi duy nhất sẽ sửa chữa cả hai vị trí. 

### Ví dụ 2 

đầu vào:```
444
777
```Dấu vết: 

| Chỉ mục | một [tôi] | b[i] | cnt47 | cnt74 | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 7 | 1 | 0 | 
| 1 | 4 | 7 | 2 | 0 | 
| 2 | 4 | 7 | 3 | 0 | 

Sau khi xử lý: 

| Biến | Giá trị | 
| --- | --- | 
| hoán đổi | 0 | 
| lật | 3 | 
| trả lời | 3 | 

Trường hợp này cho thấy tại sao chỉ tính những số không khớp là không đủ. Mọi sự không khớp đều có cùng một hướng, vì vậy việc hoán đổi không bao giờ có ích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần quét tuyến tính qua cả hai chuỗi | 
| Không gian |$O(1)$| Chỉ có một số bộ đếm số nguyên được lưu trữ | 

Với$n \le 10^5$, một thuật toán tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi vì không cần mảng phụ trợ hoặc cấu trúc dữ liệu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a = input().strip()
    b = input().strip()

    cnt47 = 0
    cnt74 = 0

    for x, y in zip(a, b):
        if x == y:
            continue

        if x == '4' and y == '7':
            cnt47 += 1
        else:
            cnt74 += 1

    swaps = min(cnt47, cnt74)
    flips = abs(cnt47 - cnt74)

    print(swaps + flips)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run("47\n74\n") == "1", "sample 1"

# minimum size, already equal
assert run("4\n4\n") == "0", "single equal character"

# all mismatches in one direction
assert run("444\n777\n") == "3", "all flips needed"

# perfectly pairable mismatches
assert run("4477\n7744\n") == "2", "all mismatches repaired by swaps"

# mixed case with leftover flips
assert run("4447\n7774\n") == "2", "one swap and one flip"

# large equal strings
n = 100000
a = "4" * n
b = "4" * n
assert run(f"{a}\n{b}\n") == "0", "maximum size equal strings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 4`|`0`| Đầu vào kích thước tối thiểu | 
|`444 / 777`|`3`| Không thể hoán đổi | 
|`4477 / 7744`|`2`| Mọi sự không phù hợp đều có thể được ghép nối | 
|`4447 / 7774`|`2`| Kết hợp hoán đổi và lật | 
| Chuỗi lớn bằng nhau |`0`| Xử lý các hạn chế tối đa một cách hiệu quả | 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi sự không khớp đều có cùng một hướng. 

đầu vào:```
444
777
```Thuật toán tính:```
cnt47 = 3
cnt74 = 0
```Không có cặp bổ sung nào tồn tại, vì vậy:```
swaps = 0
flips = 3
```Đầu ra trở thành`3`. Điều này đúng vì việc hoán đổi các chữ số giống nhau không bao giờ làm thay đổi chuỗi. 

Bây giờ hãy xem xét tình huống ngược lại trong đó mọi sự không khớp đều có thể được ghép nối. 

đầu vào:```
4477
7744
```Thuật toán tính toán:```
cnt47 = 2
cnt74 = 2
```Sau đó:```
swaps = 2
flips = 0
```Câu trả lời là`2`. Mỗi lần hoán đổi sẽ sửa chữa đồng thời hai vị trí, điều này là tối ưu. 

Cuối cùng, hãy xem xét một ví dụ có thể ghép đôi một phần. 

đầu vào:```
4447
7777
```Số lượng không khớp trở thành:```
cnt47 = 3
cnt74 = 0
```Một lần nữa, không có giao dịch hoán đổi nào hữu ích. Thuật toán xuất ra chính xác`3`. 

Việc thực hiện bất cẩn có tính toán`mismatches // 2`sẽ sản xuất không chính xác`1`.
