---
title: "CF 102419A - Hai Dây"
description: "Chúng ta có hai chuỗi a và b có cùng độ dài. Chúng ta phải chọn hai vị trí khác nhau và hoán đổi các vị trí đó trong cả hai chuỗi cùng một lúc. Mục đích là làm cho kết quả lớn hơn về mặt từ điển so với kết quả b."
date: "2026-08-12T20:05:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "A"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 629
verified: true
draft: false
---

[CF 102419A - Hai chuỗi](https://codeforces.com/problemset/problem/102419/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi`a`Và`b`có cùng độ dài. Chúng ta phải chọn hai vị trí khác nhau và hoán đổi các vị trí đó trong cả hai chuỗi cùng một lúc. Mục đích là tạo ra kết quả`a`về mặt từ điển lớn hơn kết quả`b`. 

Chi tiết quan trọng là một hoán đổi sẽ di chuyển toàn bộ một cặp`(a[i], b[i])`. Chúng ta không chọn các ký tự từ hai chuỗi một cách độc lập. Nếu vị trí`i`được chuyển đến vị trí`j`, cả hai nhân vật của nó đều di chuyển cùng nhau. 

So sánh từ điển được quyết định bởi vị trí đầu tiên nơi hai chuỗi khác nhau. Ở vị trí như vậy,`a`thắng chính xác khi nào`a[i] > b[i]`. Vì vậy, đối với mọi vị trí, chỉ có mối quan hệ giữa hai ký tự của nó là quan trọng:`a[i] > b[i]`có nghĩa là vị trí này thuận lợi cho`a`.`a[i] = b[i]`có nghĩa là vị trí này là trung lập.`a[i] < b[i]`có nghĩa là vị trí này không thuận lợi cho`a`. 

Chiều dài có thể đạt tới`10^5`, do đó, một thuật toán kiểm tra từng cặp vị trí rồi so sánh các chuỗi kết quả là quá đắt. có khoảng`n^2 / 2`sự hoán đổi có thể xảy ra và việc so sánh hai chuỗi có thể mất`O(n)`, cho`O(n^3)`nhân vật hoạt động trong trường hợp xấu nhất. Với`n = 10^5`, chúng ta cần một nghiệm tuyến tính hoặc gần tuyến tính. 

Có một số trường hợp nhỏ có thể đánh lừa một chiến lược ngây thơ. Ví dụ,```
2
ba
ab
```Ở đây vị trí đầu tiên ban đầu có`b > a`, nhưng khả năng hoán đổi duy nhất có thể sẽ chuyển cặp thứ hai không thuận lợi lên phía trước. Kết quả là`ab`so với`ba`, vậy câu trả lời là`NO`. Chỉ cần tìm một vị trí ở đó`a[i] > b[i]`là không đủ khi vị trí đó đã là đầu tiên và`n = 2`. 

Một trường hợp khác là```
3
abc
abc
```Mọi vị thế đều trung lập nên không có giao dịch hoán đổi nào có thể tạo ra một vị thế mà`a`có tính chất lớn hơn. Câu trả lời là`NO`. Một giải pháp giả định bất kỳ sự hoán đổi nào cũng có thể thay đổi sự so sánh sẽ chấp nhận trường hợp này một cách không chính xác. 

Ở thái cực khác,```
3
zzz
aaa
```đã có rồi`a[0] > b[0]`. Chúng ta buộc phải hoán đổi, nhưng chúng ta có thể hoán đổi vị trí`2`Và`3`, giữ nguyên vị trí đầu tiên. Câu trả lời là`YES`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là thử mọi cặp`(x, y)`, thực hiện hoán đổi bản sao của hai chuỗi và so sánh các chuỗi kết quả theo từ điển. có`n(n-1)/2`cặp, đó là`O(n^2)`và mỗi so sánh có thể kiểm tra`O(n)`nhân vật. Trường hợp xấu nhất là như vậy`O(n^3)`. Vì`n = 100000`, đây là đại khái`10^15`hoạt động ở cấp độ nhân vật, vượt xa giới hạn thời gian. 

Quan sát hữu ích là chúng ta không thực sự quan tâm đến các ký tự chính xác ở mọi vị trí. Đối với so sánh từ điển, một vị trí chỉ cần được phân loại là thuận lợi, trung lập hoặc không thuận lợi tùy theo sự so sánh của`a[i]`Và`b[i]`. 

Giả sử chúng ta tìm được một vị trí`p`Ở đâu`a[p] > b[p]`. Nếu như`p`không phải là vị trí đầu tiên, hoán đổi vị trí`0`Và`p`ngay lập tức giải quyết vấn đề. Sau khi hoán đổi, vị trí`0`chứa cặp cũ từ`p`, vậy chuỗi mới thỏa mãn`a[0] > b[0]`. Do đó, các chuỗi được sắp xếp chính xác bất kể điều gì xảy ra sau đó. 

Nếu vị trí thuận lợi đã có vị trí`0`, chúng ta không thể sử dụng nó làm đích đến của một trao đổi khác mà không di chuyển nó đi. Khi`n >= 3`, chúng ta có thể trao đổi vị trí một cách đơn giản`1`Và`2`. Chức vụ`0`không thay đổi nên nó vẫn cho`a[0] > b[0]`. Điều này cũng đáp ứng yêu cầu thực hiện chính xác một lần hoán đổi. 

Chỉ một`n = 2`cần xử lý đặc biệt khi vị trí thuận lợi đã có trước. Chỉ có một khả năng hoán đổi nên sau khi hoán đổi, vị trí thứ hai cũ sẽ trở thành vị trí thứ nhất. Việc hoán đổi thành công chính xác khi`a[1] >= b[1]`. Nếu vị trí thứ hai là trung tính thì vị trí đầu tiên mới bằng nhau và cặp thuận lợi sẽ chuyển sang vị trí thứ hai. Nếu vị trí thứ hai cũng thuận lợi thì vị trí thứ nhất vẫn thuận lợi. Nếu điều đó không thuận lợi thì vị trí đầu tiên sẽ tạo ra`a < b`. 

Việc quan sát làm giảm toàn bộ vấn đề thành một lần quét tìm vị trí có`a[i] > b[i]`, cộng với khối lượng công việc không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét các chuỗi từ trái sang phải và tìm chỉ mục bất kỳ`p`như vậy`a[p] > b[p]`. Nếu không có chỉ mục đó tồn tại, xuất ra`NO`. Hoán đổi chỉ di chuyển các cặp hiện có giữa các vị trí, vì vậy nó không bao giờ có thể tạo ra một cặp thuận lợi nếu ban đầu không có cặp nào tồn tại. 
2. Nếu`p > 0`, vị trí đầu ra`1`Và`p + 1`. Sau khi hoán đổi chúng, cặp đôi ban đầu ở`p`chuyển đến vị trí đầu tiên, đưa ra`a[0] > b[0]`. Các chuỗi kết quả được sắp xếp ngay lập tức với`a`lớn hơn`b`. 
3. Nếu`p = 0`Và`n >= 3`, vị trí đầu ra`2`Và`3`. Vị trí đầu tiên không được chạm tới nên bất đẳng thức chặt chẽ`a[0] > b[0]`vẫn là sự so sánh mang tính quyết định đầu tiên. Việc hoán đổi vẫn được thực hiện vì sự cố yêu cầu chính xác một thao tác. 
4. Nếu`p = 0`Và`n = 2`, sự hoán đổi duy nhất có thể là vị trí`1`Và`2`. Sau lần hoán đổi này, vị trí thứ hai cũ sẽ trở thành vị trí đầu tiên. Chấp nhận trao đổi chính xác khi`a[1] >= b[1]`; nếu không thì xuất ra`NO`. 

Bất biến đằng sau lời giải là mối quan hệ so sánh của một vị trí di chuyển cùng với cặp của nó`(a[i], b[i])`. Bất cứ khi nào chúng ta di chuyển một cặp thuận lợi đã biết đến vị trí đầu tiên, nó sẽ trở thành sự so sánh mang tính quyết định đầu tiên và chứng tỏ`a > b`. Khi cặp thuận lợi đã ở vị trí đầu tiên, chúng ta có thể bảo toàn nó bằng cách hoán đổi hai vị trí sau, miễn là tồn tại hai vị trí như vậy. Tình huống duy nhất không thể thực hiện được là`n = 2`, trong đó phương án thay thế duy nhất phải được kiểm tra trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = input().strip()
    b = input().strip()

    pos = -1

    for i in range(n):
        if a[i] > b[i]:
            pos = i
            break

    if pos == -1:
        print("NO")
        return

    if pos > 0:
        print("YES")
        print(1, pos + 1)
        return

    if n >= 3:
        print("YES")
        print(2, 3)
        return

    # n == 2 and the favorable position is already position 1.
    if a[1] >= b[1]:
        print("YES")
        print(1, 2)
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Quá trình quét lưu trữ vị trí đầu tiên nơi`a[i] > b[i]`. Bất kỳ vị trí nào như vậy cũng đủ, bởi vì nếu nó chưa phải là vị trí đầu tiên, việc di chuyển nó đến vị trí đầu tiên sẽ giải quyết việc so sánh từ điển ngay lập tức. 

các`pos > 0`nhánh sử dụng các chỉ mục Python dựa trên 0 nhưng in các vị trí dựa trên một. Như vậy vị trí`0`trở thành`1`, trong khi`pos`trở thành`pos + 1`. 

Khi`pos == 0`Và`n >= 3`, vị trí`1`Và`2`trong lập chỉ mục dựa trên số 0 tương ứng với các vị trí đầu ra`2`Và`3`. Chúng được đảm bảo tồn tại bởi vì`n >= 3`, và không ảnh hưởng đến vị trí thuận lợi đầu tiên. 

Nhánh cuối cùng chỉ đạt được cho`n == 2`. Có chính xác một sự hoán đổi hợp pháp, vì vậy hãy kiểm tra`a[1] >= b[1]`là đủ. Sự bình đẳng được chấp nhận vì sau khi hoán đổi, các ký tự đầu tiên có thể bằng nhau, cho phép cặp thuận lợi từ vị trí đầu tiên ban đầu quyết định so sánh ở ký tự thứ hai. 

Không cần sao chép hoặc đột biến chuỗi. Thuật toán chỉ xác định một cặp chỉ số hợp lệ, do đó mức sử dụng bộ nhớ bổ sung là không đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3
abc
abc
```Quá trình quét tiến hành như sau. 

| chỉ mục |`a[index]`|`b[index]`| quan hệ |`pos`| 
| --- | --- | --- | --- | --- | 
| 0 | một | một | bằng | -1 | 
| 1 | b | b | bằng | -1 | 
| 2 | c | c | bằng | -1 | 

Không có vị trí nào thỏa mãn`a[i] > b[i]`, Vì thế`pos`còn lại`-1`. 

Thuật toán in:```
NO
```Điều này chứng tỏ rằng việc hoán đổi vị trí không thể tạo ra một cặp thuận lợi. Tất cả các cặp đều trung tính, do đó mọi hoán vị có thể làm cho hai chuỗi giống hệt nhau. 

### Mẫu 2 

Đầu vào là```
3
zzz
aaa
```Quá trình quét dừng ngay lập tức. 

| chỉ mục |`a[index]`|`b[index]`| quan hệ |`pos`| 
| --- | --- | --- | --- | --- | 
| 0 | z | một |`a > b`| 0 | 

Vị trí thuận lợi đã là đầu tiên, và`n = 3`, do đó thuật toán hoán đổi vị trí`2`Và`3`. 

| Hoạt động | Vị trí đầu tiên sau khi hoán đổi | Kết quả so sánh | 
| --- | --- | --- | 
| hoán đổi vị trí 2 và 3 |`z`vs`a`|`z > a`| 

Các chuỗi kết quả vẫn còn`zzz`Và`aaa`, nhưng việc hoán đổi cần thiết đã được thực hiện. Đầu ra là```
YES
2 3
```Ví dụ này chứng minh tại sao thao tác bắt buộc không gây ra vấn đề gì khi có ít nhất hai vị trí sau vị trí đầu tiên vốn đã thuận lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Các chuỗi được quét một lần để tìm vị trí với`a[i] > b[i]`. | 
| Không gian | O(1) | Chỉ các chuỗi đầu vào và số lượng chỉ mục không đổi được lưu trữ; không có cấu trúc bổ sung phụ thuộc vào`n`. | 

Với`n <= 10^5`, quá trình quét tuyến tính chỉ thực hiện khoảng một trăm nghìn so sánh ký tự, dễ dàng nằm trong giới hạn 1 giây. Thuật toán cũng sử dụng không gian phụ trợ không đổi, thấp hơn nhiều so với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from contextlib import redirect_stdout

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = input().strip()
    b = input().strip()

    pos = -1

    for i in range(n):
        if a[i] > b[i]:
            pos = i
            break

    if pos == -1:
        print("NO")
        return

    if pos > 0:
        print("YES")
        print(1, pos + 1)
        return

    if n >= 3:
        print("YES")
        print(2, 3)
        return

    if a[1] >= b[1]:
        print("YES")
        print(1, 2)
    else:
        print("NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    output = io.StringIO()

    try:
        sys.stdin = io.StringIO(inp)
        with redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin

    return output.getvalue()

# Provided samples
assert run("""3
abc
abc
""") == "NO\n", "sample 1"

assert run("""3
zzz
aaa
""") == "YES\n2 3\n", "sample 2"

# Minimum size, favorable position is second.
assert run("""2
ab
aa
""") == "YES\n1 2\n", "minimum size with favorable second position"

# Minimum size, favorable position is first but the second position is unfavorable.
assert run("""2
ba
ab
""") == "NO\n", "minimum size impossible case"

# All positions equal.
assert run("""4
abcd
abcd
""") == "NO\n", "all-equal strings"

# Maximum size, favorable position is the final character.
n = 100000
a = "a" * (n - 1) + "b"
b = "a" * n
assert run(f"{n}\n{a}\n{b}\n") == f"YES\n1 {n}\n", "maximum size and last position"

# Favorable position is first, with exactly three positions.
assert run("""3
baa
abb
""") == "YES\n2 3\n", "favorable first position"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / ab / aa`|`YES / 1 2`| Độ dài tối thiểu với cặp thuận lợi ở vị trí thứ hai. | 
|`2 / ba / ab`|`NO`| Điều đặc biệt`n = 2`trường hợp trao đổi duy nhất thực hiện`a < b`. | 
|`4 / abcd / abcd`|`NO`| Không có vị trí thuận lợi tồn tại ở bất cứ đâu. | 
|`100000 / a...ab / a...a`|`YES / 1 100000`| Độ dài tối đa và vị trí thuận lợi ở chỉ mục cuối cùng, nắm bắt các lỗi lập chỉ mục. | 
|`3 / baa / abb`|`YES / 2 3`| Vị trí đầu tiên thuận lợi trong khi vẫn phải thực hiện đúng một lần hoán đổi. | 

## Vỏ cạnh 

###Không có vị thế thuận lợi 

Hãy xem xét```
3
abc
abd
```Mọi vị trí đều thỏa mãn`a[i] <= b[i]`. Quá trình quét đến cuối mà không tìm thấy cặp thuận lợi, vì vậy`pos = -1`và thuật toán in`NO`. Vì việc hoán đổi chỉ di chuyển các cặp hiện có nên không có thao tác nào có thể làm cho ký tự lớn hơn xuất hiện trong`a`hơn trong`b`ở một vị trí mà trước đây điều đó là không thể. 

### Vị trí thuận lợi đã là vị trí đầu tiên với ba vị trí trở lên 

Hãy xem xét```
3
baa
abb
```Tại vị trí`1`,`b > a`, do đó quá trình quét tìm thấy`pos = 0`. Vì có ít nhất ba vị trí nên thuật toán chọn vị trí`2`Và`3`. Cặp đầu tiên còn lại`(b, a)`, do đó chuỗi kết quả vẫn thỏa mãn`a > b`ngay ở ký tự đầu tiên của họ. Hoán đổi bắt buộc chỉ thay đổi các vị trí sau này. 

### Vị trí thuận lợi là vị trí đầu tiên có hai vị trí 

Hãy xem xét```
2
ba
ab
```Vị trí đầu tiên có`b > a`, nhưng cái thứ hai có`a < b`. Từ`n = 2`, hoạt động hợp pháp duy nhất hoán đổi hai vị trí. Kết quả trở thành`ab`Và`ba`, có ký tự đầu tiên thỏa mãn`a < b`. Thuật toán kiểm tra`a[1] >= b[1]`, tìm thấy`a < b`, và in chính xác`NO`. 

Trường hợp tương phản```
2
ba
aa
```có`b > a`ở vị trí đầu tiên và`a = a`vào thứ hai. Sau khi hoán đổi, các chuỗi trở thành`ab`Và`aa`. Các ký tự đầu tiên bằng nhau và ký tự so sánh thứ hai là`b > a`, do đó kết quả là hợp lệ. Thuật toán chấp nhận vì`a[1] >= b[1]`. 

### Vị trí thuận lợi ở chỉ số cuối cùng 

Hãy xem xét```
4
aaab
aaaa
```Vị trí thuận lợi duy nhất là chỉ số`4`, Ở đâu`b > a`trong ký hiệu của dây, cụ thể là`b > a`. Quét dựa trên số không tìm thấy`pos = 3`. Hoán đổi vị trí`1`Và`4`di chuyển cặp thuận lợi đó lên phía trước, tạo ra sự so sánh ký tự đầu tiên của`b > a`. Đầu ra của thuật toán`YES 1 4`. 

Trường hợp này thực hiện`pos + 1`chuyển đổi và cho thấy tại sao bất kỳ vị trí thuận lợi nào ở phía trước đều có thể được chuyển thẳng lên phía trước.
