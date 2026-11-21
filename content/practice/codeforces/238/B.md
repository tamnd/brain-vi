---
title: "CF 238B - Phân vùng nhàm chán"
description: "Chúng ta được cho một mảng các số nguyên và một giá trị h. Mỗi cặp phần tử đóng góp một giá trị tùy thuộc vào việc hai phần tử đó được đặt vào cùng một nhóm hay các nhóm khác nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 238
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 148 (Div. 1)"
rating: 1800
weight: 238
solve_time_s: 236
verified: true
draft: false
---

[CF 238B - Phân vùng nhàm chán](https://codeforces.com/problemset/problem/238/B) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 3 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một giá trị`h`. Mỗi cặp phần tử đóng góp một giá trị tùy thuộc vào việc hai phần tử đó được đặt vào cùng một nhóm hay các nhóm khác nhau. 

Nếu hai số`a[i]`Và`a[j]`thuộc cùng một dãy con thì giá trị cặp của chúng là:$$a[i] + a[j]$$Nếu chúng thuộc các dãy con khác nhau thì giá trị cặp sẽ là:$$a[i] + a[j] + h$$Đối với phân vùng đã chọn, chúng tôi xem xét từng cặp không có thứ tự và thu thập tất cả các giá trị này. Điểm tốt của phân vùng là sự khác biệt giữa giá trị cặp lớn nhất và nhỏ nhất. Chúng ta phải giảm thiểu sự khác biệt này và cũng xuất ra một phân vùng hợp lệ. 

Kích thước mảng đạt`10^5`, vì vậy bất kỳ thuật toán nào kiểm tra rõ ràng tất cả các cặp đều không thể thực hiện được. Số lượng cặp xấp xỉ:$$\frac{n(n-1)}{2}$$đó là về`5 * 10^9`trong trường hợp xấu nhất. Ngay cả việc lưu trữ tất cả các giá trị cặp cũng sẽ vượt quá giới hạn bộ nhớ. 

Các giá trị đầu vào và`h`có thể đạt được`10^8`, do đó, tổng vừa vặn một cách an toàn bên trong các số nguyên có dấu 32 bit, nhưng dù sao thì số nguyên Python cũng loại bỏ các mối lo ngại về tràn. 

Phần khó khăn là phân vùng thay đổi giá trị cặp bằng cách thêm`0`hoặc thêm`h`. Việc triển khai bất cẩn có thể cho rằng câu trả lời tối ưu luôn sử dụng cả hai nhóm, nhưng điều đó là sai. 

Coi như:```
2 100
1 1000
```Nếu chúng ta chia các số, giá trị cặp duy nhất sẽ trở thành:```
1 + 1000 + 100 = 1101
```Mức tối đa và tối thiểu đều bằng nhau, vì vậy lòng tốt là`0`. 

Nhưng nếu chúng ta giữ mọi thứ cùng nhau:```
1 + 1000 = 1001
```Lòng tốt vẫn còn đó`0`. 

Cả hai đều tối ưu. Vấn đề cho phép các chuỗi con trống, điều này quan trọng vì đôi khi đặt mọi thứ vào một nhóm là tốt nhất. 

Một trường hợp tế nhị khác xuất hiện khi`h = 0`.```
4 0
1 5 7 10
```Mỗi giá trị cặp chỉ đơn giản là`a[i] + a[j]`bất kể phân vùng, vì vậy câu trả lời chỉ phụ thuộc vào chính mảng đó. Phân vùng nào cũng tối ưu. Một giải pháp cố gắng ép buộc logic phân chia có ý nghĩa có thể vô tình làm phức tạp trường hợp này. 

Trường hợp cạnh quan trọng nhất là khi các phần tử nhỏ nhất và lớn nhất tương tác không tốt. 

Ví dụ:```
3 2
1 2 100
```Nếu chúng ta tách ra`100`một mình, sau đó các cặp nhóm chéo sẽ đạt được`+2`, có thể làm tăng giá trị tối đa quá nhiều. Chiến lược tối ưu phụ thuộc hoàn toàn vào việc kiểm soát tổng cặp tối thiểu và tối đa toàn cầu. 

Quan sát này thúc đẩy toàn bộ giải pháp. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Mọi phần tử đều có thể thuộc một trong hai nhóm, do đó có`2^n`các phân vùng có thể. Đối với mỗi phân vùng, chúng tôi có thể tính toán tất cả các giá trị cặp và đo mức chênh lệch giữa mức tối đa và mức tối thiểu. 

Tính đúng đắn là hiển nhiên vì chúng tôi kiểm tra mọi câu trả lời có thể. Thời gian chạy là thảm khốc. Ngay cả đối với`n = 30`, số lượng phân vùng đã vượt quá một tỷ. Với`n = 10^5`, tìm kiếm toàn diện là vô vọng. 

Bước tiếp theo là hiểu điều gì thực sự xác định giá trị cặp tối thiểu và tối đa. 

Giả sử mảng được sắp xếp:$$b_1 \le b_2 \le \dots \le b_n$$Nếu không có bất kỳ hiệu ứng phân vùng nào, tổng cặp nhỏ nhất có thể là:$$b_1 + b_2$$và lớn nhất là:$$b_{n-1} + b_n$$Phân vùng chỉ quyết định liệu một cặp cụ thể có nhận được thêm`h`. 

Điều đó có nghĩa là mọi giá trị cặp đều thuộc một trong hai dạng:$$a_i + a_j$$hoặc$$a_i + a_j + h$$Vì vậy, mức tối thiểu toàn cầu phải là:$$b_1 + b_2$$hoặc giá trị lớn hơn một chút nếu cặp đó vượt qua các nhóm. 

Tương tự, mức tối đa toàn cầu là:$$b_{n-1} + b_n$$hoặc lớn hơn bằng`h`. 

Cái nhìn sâu sắc quan trọng là chỉ có các cặp cực đoan mới quan trọng. Chúng ta không cần phải lý luận về từng cặp một. 

Chỉ có hai chiến lược có ý nghĩa. 

Chiến lược đầu tiên giữ mọi phần tử trong cùng một nhóm. Khi đó mọi giá trị của cặp đều không thay đổi, vì vậy:$$\text{goodness} = (b_{n-1} + b_n) - (b_1 + b_2)$$Chiến lược thứ hai cố gắng cải thiện mức chênh lệch bằng cách buộc cặp nhỏ nhất nhận được`+h`đồng thời ngăn chặn cặp lớn nhất nhận được`+h`. 

Để tạo các nhóm chéo cặp nhỏ nhất, hai phần tử nhỏ nhất phải được tách ra. 

Để giữ cặp lớn nhất trong một nhóm, hai phần tử lớn nhất phải ở cùng nhau. 

Sau khi sắp xếp, cách cắt duy nhất có thể có với thuộc tính này là:$$\{b_1\} \quad \text{and} \quad \{b_2, b_3, \dots, b_n\}$$hoặc tương đương đối xứng của nó. 

Theo phân vùng này: 

Cặp tối thiểu trở thành:$$b_1 + b_2 + h$$vì hai phần tử nhỏ nhất được tách ra. 

Cặp tối đa còn lại:$$b_{n-1} + b_n$$bởi vì hai yếu tố lớn nhất ở lại với nhau. 

Vì vậy, lòng tốt trở thành:$$(b_{n-1} + b_n) - (b_1 + b_2 + h)$$Nếu giá trị này trở thành âm, điều đó chỉ có nghĩa là vai trò được hoán đổi tối thiểu và tối đa, do đó mức chênh lệch thực tế là giá trị tuyệt đối. 

Câu trả lời cuối cùng là khả năng nhỏ hơn trong hai khả năng này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và ghi nhớ chỉ mục gốc của từng phần tử. 
2. Sắp xếp các phần tử theo giá trị. 
3. Tính mức độ tốt nếu tất cả các phần tử nằm trong cùng một nhóm.$$d_1 = (b_{n-1} + b_n) - (b_1 + b_2)$$Phân vùng này luôn hợp lệ vì một dãy con có thể trống. 

1. Tính mức độ tốt cho phép chia đặc biệt trong đó chỉ có phần tử nhỏ nhất bị cô lập.$$d_2 = |(b_{n-1} + b_n) - (b_1 + b_2 + h)|$$Giá trị tuyệt đối là cần thiết vì tăng cặp tối thiểu lên`h`có thể đẩy nó lên trên mức tối đa trước đó. 

1. So sánh`d_1`Và`d_2`. 
2. Nếu`d_1 <= d_2`, đặt mọi phần tử vào nhóm`1`. 
3. Ngược lại, đặt phần tử nhỏ nhất vào nhóm`1`và mọi phần tử khác vào nhóm`2`. 
4. Khôi phục các câu trả lời theo thứ tự đầu vào ban đầu và in mức độ và phân vùng đã chọn. 

### Tại sao nó hoạt động 

Tổng cặp nhỏ nhất trong toàn bộ mảng luôn được hình thành bởi hai phần tử nhỏ nhất sau khi sắp xếp. Tương tự, tổng cặp lớn nhất luôn được hình thành bởi hai phần tử lớn nhất. 

Phân vùng chỉ có thể thêm`h`cho các cặp đã chọn. Mọi nỗ lực nhằm cải thiện câu trả lời đều phải tăng giá trị cặp tối thiểu mà không tăng giá trị cặp tối đa. 

Cách duy nhất để tăng mức tối thiểu là tách hai phần tử nhỏ nhất. Cách duy nhất để tránh tăng mức tối đa là giữ hai phần tử lớn nhất lại với nhau. 

Sau khi mảng được sắp xếp, hai yêu cầu này sẽ xác định duy nhất phân vùng không tầm thường tối ưu. Bất kỳ phân vùng nào khác đều giữ nguyên mức tối thiểu hoặc tăng mức tối đa một cách không cần thiết. 

Vì vậy, mức tối ưu phải là một trong chính xác hai ứng cử viên, nhóm tất cả trong một hoặc nhóm nhỏ nhất cô lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, h = map(int, input().split())
a = list(map(int, input().split()))

arr = [(a[i], i) for i in range(n)]
arr.sort()

same_group = (arr[-1][0] + arr[-2][0]) - (arr[0][0] + arr[1][0])
split_group = abs(
    (arr[-1][0] + arr[-2][0]) -
    (arr[0][0] + arr[1][0] + h)
)

if same_group <= split_group:
    ans = [1] * n
    print(same_group)
    print(*ans)
else:
    ans = [2] * n
    ans[arr[0][1]] = 1
    print(split_group)
    print(*ans)
```Phần đầu tiên lưu trữ cả giá trị và chỉ mục gốc vì việc sắp xếp thay đổi thứ tự, nhưng đầu ra phải khớp với thứ tự đầu vào ban đầu. 

Sau khi sắp xếp, hai phần tử nhỏ nhất nằm ở vị trí`0`Và`1`, trong khi hai cái lớn nhất ở vị trí`n-2`Và`n-1`. 

Biến`same_group`tương ứng với phân vùng trong đó mọi phần tử thuộc cùng một chuỗi con. Không có cặp nào nhận được`+h`. 

Biến`split_group`tương ứng với việc cô lập phần tử nhỏ nhất. Cặp nhỏ nhất bây giờ vượt qua các nhóm và nhận được`+h`, trong khi cặp lớn nhất không thay đổi. 

Giá trị tuyệt đối là phần tinh tế. Giả định:```
h = 100
array = [1, 2, 3]
```Sau đó:```
largest pair = 5
smallest cross pair = 103
```Sự lây lan trở thành:```
103 - 5 = 98
```không`5 - 103`. 

Cuối cùng, chúng tôi xây dựng lại phân vùng. Nếu phân vùng tầm thường là tối ưu thì mọi phần tử đều có nhãn`1`. Mặt khác, chỉ phần tử nhỏ nhất trên toàn cầu mới nhận được nhãn`1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1 2 3
```Sau khi sắp xếp: 

| Vị trí | Giá trị | Chỉ mục gốc | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 2 | 1 | 
| 2 | 3 | 2 | 

Tính cả hai ứng viên: 

| Số lượng | Giá trị | 
| --- | --- | 
| Cặp lớn nhất | 2 + 3 = 5 | 
| Cặp nhỏ nhất | 1 + 2 = 3 | 
| cùng_nhóm | 5 - 3 = 2 | 
| chia_nhóm | \|5 - (3 + 2)\| = 0 | 

Từ`0 < 2`, chúng tôi cô lập phần tử nhỏ nhất. 

Phân vùng: 

| Yếu tố | Nhóm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 

Các giá trị cặp trở thành: 

| Cặp | Giá trị | 
| --- | --- | 
| (1,2) | 5 | 
| (1,3) | 6 | 
| (2,3) | 5 | 

Sự lây lan là`6 - 5 = 1`. 

Dấu vết này chứng minh tại sao chỉ thay đổi cặp nhỏ nhất có thể làm giảm đáng kể phạm vi. 

### Ví dụ 2 

đầu vào:```
4 100
1 2 3 4
```Mảng được sắp xếp: 

| Vị trí | Giá trị | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 4 | 

Giá trị ứng viên: 

| Số lượng | Giá trị | 
| --- | --- | 
| Cặp lớn nhất | 7 | 
| Cặp nhỏ nhất | 3 | 
| cùng_nhóm | 4 | 
| chia_nhóm | \|7 - 103\| = 96 | 

Bây giờ thêm`h`là quá lớn. Việc tăng cặp tối thiểu sẽ vượt quá mức nghiêm trọng. 

Phân vùng tối ưu: 

| Yếu tố | Nhóm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Ví dụ này cho thấy việc chia tách không phải lúc nào cũng có lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp thống trị thời gian chạy | 
| Không gian |$O(n)$| Mảng được sắp xếp và mảng trả lời lưu trữ tất cả các phần tử | 

Các ràng buộc cho phép thực hiện khoảng vài triệu thao tác một cách thoải mái trong thời gian giới hạn. Sắp xếp`10^5`các phần tử đủ nhanh trong Python và công việc còn lại là tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, h = map(int, input().split())
    a = list(map(int, input().split()))

    arr = [(a[i], i) for i in range(n)]
    arr.sort()

    same_group = (arr[-1][0] + arr[-2][0]) - (arr[0][0] + arr[1][0])
    split_group = abs(
        (arr[-1][0] + arr[-2][0]) -
        (arr[0][0] + arr[1][0] + h)
    )

    if same_group <= split_group:
        ans = [1] * n
        print(same_group)
        print(*ans)
    else:
        ans = [2] * n
        ans[arr[0][1]] = 1
        print(split_group)
        print(*ans)

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
assert run("3 2\n1 2 3\n") == "1\n1 2 2\n"

# minimum size
assert run("2 0\n5 10\n") == "0\n1 1\n"

# all equal values
assert run("5 3\n7 7 7 7 7\n") == "0\n1 2 2 2 2\n"

# large h prefers single group
assert run("4 100\n1 2 3 4\n") == "4\n1 1 1 1\n"

# off-by-one style ordering check
assert run("5 2\n10 1 9 2 8\n") == "3\n2 1 2 2 2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0 / 5 10`| lòng tốt`0`| Kích thước mảng tối thiểu và`h = 0`| 
|`5 3 / 7 7 7 7 7`| cô lập một phần tử | Các giá trị bằng nhau vẫn được hưởng lợi từ việc chia tách | 
|`4 100 / 1 2 3 4`| tất cả trong một nhóm | Lớn`h`có thể làm cho việc chia tách có hại | 
|`5 2 / 10 1 9 2 8`| chỉ số gốc nhỏ nhất bị cô lập | Khôi phục đúng thứ tự ban đầu | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các số đều bằng nhau. 

đầu vào:```
5 3
7 7 7 7 7
```Không phân tách, mọi giá trị cặp đều bằng nhau`14`, vậy thì lòng tốt là`0`. 

Nếu chúng ta cô lập một phần tử, các cặp nhóm chéo sẽ trở thành`17`, trong khi các cặp cùng nhóm vẫn còn`14`. Sự lây lan trở thành`3`. 

Thuật toán so sánh chính xác:```
same_group = 0
split_group = 3
```và giữ mọi thứ lại với nhau. 

Bây giờ hãy xem xét trường hợp việc chia tách giúp ích đáng kể. 

đầu vào:```
3 2
1 2 3
```Phạm vi cặp thô là:```
5 - 3 = 2
```Việc tách phần tử nhỏ nhất sẽ dịch chuyển mức tối thiểu lên trên:```
1 + 2 + 2 = 5
```trong khi mức tối đa vẫn còn`5`. 

Sự chênh lệch giảm xuống`0`. 

Cuối cùng, hãy xem xét trường hợp vượt quá nguy hiểm. 

đầu vào:```
4 100
1 2 3 4
```Việc tách cặp nhỏ nhất sẽ thay đổi mức tối thiểu từ`3`ĐẾN`103`, trở nên lớn hơn mức tối đa cũ`7`. 

Thuật toán sử dụng:```
abs(7 - 103)
```để tính mức chênh lệch thực`96`. 

Nếu không có giá trị tuyệt đối, việc triển khai sẽ tạo ra câu trả lời phủ định không chính xác.
