---
title: "CF 81B - Định dạng trình tự"
description: "Chúng ta được cung cấp một biểu diễn văn bản lộn xộn của một chuỗi. Chuỗi có thể chứa số nguyên dương, dấu phẩy, dấu cách và mã thông báo đặc biệt .... Dấu cách có thể xuất hiện sai vị trí hoặc xuất hiện nhiều lần. Nhiệm vụ hoàn toàn là định dạng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 81
codeforces_index: "B"
codeforces_contest_name: "Yandex.Algorithm Open 2011: Qualification 1"
rating: 1700
weight: 81
solve_time_s: 103
verified: true
draft: false
---

[CF 81B - Định dạng trình tự](https://codeforces.com/problemset/problem/81/B) 

**Đánh giá:** 1700 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu diễn văn bản lộn xộn của một chuỗi. Chuỗi có thể chứa số nguyên dương, dấu phẩy, dấu cách và mã thông báo đặc biệt`...`. Dấu cách có thể xuất hiện sai vị trí hoặc xuất hiện nhiều lần. 

Nhiệm vụ hoàn toàn là định dạng. Chúng ta phải viết lại chuỗi để mọi mã thông báo tuân theo chính sách giãn cách chính xác. 

Dấu phẩy phải được theo sau đúng một dấu cách trừ khi đó là ký tự cuối cùng của chuỗi. Mã thông báo`...`phải có chính xác một khoảng trắng trước nó trừ khi nó xuất hiện ở đầu. Khoảng cách giữa hai số thu gọn lại thành một khoảng trắng. Mọi không gian khác đều phải biến mất. 

Độ dài đầu vào tối đa là 255 ký tự, rất nhỏ. Ngay cả việc xử lý bậc hai cũng sẽ trôi qua một cách thoải mái. Khó khăn thực sự không phải là hiệu suất, mà là hiểu chính xác không gian nào là hợp pháp và không gian nào không. 

Phần nguy hiểm nhất của vấn đề là các không gian không được xử lý thống nhất. Cần có khoảng trắng sau dấu phẩy, trong khi khoảng trắng trước dấu phẩy bị cấm. Một không gian trước đó`...`là bắt buộc, trong khi một khoảng trắng sau`...`bị cấm trừ khi một quy tắc khác đưa ra nó. Cho phép có khoảng cách giữa hai số nhưng phải thu gọn thành một. 

Một số trường hợp phức tạp âm thầm phá vỡ việc triển khai ngây thơ. 

Coi như:```
1 ,2
```Đầu ra đúng là:```
1, 2
```Một giải pháp bất cẩn chỉ chèn các khoảng trắng bị thiếu sau dấu phẩy có thể khiến khoảng trắng không hợp lệ trước dấu phẩy không thay đổi. 

Một trường hợp phức tạp khác là:```
...1
```Đầu ra đúng là:```
...1
```Không được có khoảng trống sau`...`. Quy tắc chỉ nói về một khoảng trắng trước nó. 

Bây giờ hãy xem xét:```
1     2
```Đầu ra đúng là:```
1 2
```Khoảng cách giữa các số thu gọn lại thành chính xác một. Nếu chúng ta xóa tất cả dấu cách trên toàn bộ rồi xây dựng lại định dạng xung quanh dấu phẩy thì dấu phân cách hợp lệ này sẽ biến mất. 

Một trường hợp tế nhị hơn:```
1,...,2
```Đầu ra đúng là:```
1, ..., 2
```Mã thông báo`...`hành xử khác với những con số. Nó cần một khoảng trắng ở đầu và dấu phẩy sau nó vẫn cần một khoảng trắng ở cuối. 

Cách an toàn nhất để giải quyết vấn đề là ngừng suy nghĩ về từng nhân vật mà thay vào đó hãy nghĩ về các mã thông báo. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ liên tục quét chuỗi và sửa chữa các vi phạm cục bộ. Một lượt có thể xóa các khoảng trắng trùng lặp, một lượt khác có thể chèn các khoảng trắng bị thiếu sau dấu phẩy, một lượt khác có thể xóa các khoảng trắng trước dấu phẩy, v.v. 

Điều này hoạt động vì mọi quy tắc định dạng đều mang tính cục bộ. Mỗi lỗi có thể được sửa bằng cách chỉ kiểm tra các ký tự gần đó. 

Vấn đề với cách tiếp cận này không phải là sự phức tạp tiệm cận. Chỉ với 255 ký tự, thậm chí nhiều lần quét cũng đủ nhanh. Vấn đề thực sự là sự tương tác giữa các quy tắc. Một bước sửa chữa có thể vô tình tạo ra vi phạm mới cho quy tắc khác. Ví dụ: việc xóa mạnh các khoảng trắng có thể loại bỏ khoảng trống cần thiết trước`...`. Việc triển khai trở nên mong manh vì mọi phép biến đổi đều phụ thuộc vào thứ tự của các phép biến đổi trước đó. 

Một quan sát rõ ràng hơn nhiều là đầu vào đã bao gồm các mã thông báo được xác định rõ ràng. Mỗi phần có ý nghĩa là:```
number
comma
...
```Bản thân các không gian không bao giờ quan trọng về mặt ngữ nghĩa. Chúng chỉ mô tả mối quan hệ định dạng giữa các mã thông báo lân cận. 

Khi chúng tôi mã hóa chuỗi, định dạng sẽ trở nên xác định. Chúng ta có thể xây dựng lại câu trả lời từ đầu trong khi quyết định chính xác khoảng trống nào thuộc về các mã thông báo liền kề. 

Điều này hoàn toàn tránh được logic dọn dẹp phức tạp. 

Các quy tắc tái thiết trở nên đơn giản: 

Nếu mã thông báo hiện tại là dấu phẩy, hãy thêm trực tiếp mã thông báo đó mà không có khoảng trắng trước đó. 

Nếu mã thông báo hiện tại là`...`, hãy thêm một khoảng trắng vào đầu trừ khi nó bắt đầu chuỗi. 

Nếu hai mã thông báo liên tiếp là số, hãy đặt chính xác một khoảng trắng giữa chúng. 

Nếu mã thông báo trước đó là dấu phẩy, hãy đặt chính xác một khoảng trắng trước mã thông báo không phải dấu phẩy hiện tại. 

Mọi thứ khác đều không có không gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force sửa chuỗi lặp lại | O(n²) | O(n) | Được chấp nhận nhưng mong manh | 
| Token hóa và xây dựng lại | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi đầu vào từ trái sang phải và trích xuất mã thông báo. 

Bỏ qua hoàn toàn khoảng trắng trong quá trình mã thông báo. Bất cứ khi nào chúng ta gặp các chữ số, hãy sử dụng toàn bộ số đó. Bất cứ khi nào chúng ta gặp phải`...`, coi cả ba dấu chấm là một mã thông báo. Dấu phẩy trở thành mã thông báo độc lập. 
2. Lưu trữ tất cả các token được trích xuất theo thứ tự. 

Sau bước này, khoảng cách lộn xộn ban đầu không còn quan trọng nữa. Chúng tôi chỉ bảo tồn cấu trúc ngữ nghĩa của chuỗi. 
3. Xây dựng lại mã thông báo câu trả lời bằng mã thông báo. 

Đối với mỗi mã thông báo, hãy quyết định xem có cần khoảng trắng hay không trước khi thêm nó vào. 
4. Nếu mã thông báo là dấu phẩy, hãy thêm nó ngay lập tức. 

Dấu phẩy không bao giờ có dấu cách trước chúng. 
5. Nếu mã thông báo trước đó là dấu phẩy, hãy chèn chính xác một khoảng trắng trước mã thông báo hiện tại. 

Điều này thực thi quy tắc về dấu phẩy. 
6. Nếu mã thông báo hiện tại là`...`và nó không phải là mã thông báo đầu tiên, hãy chèn chính xác một khoảng trắng trước nó. 

Điều này thực thi quy tắc đặc biệt cho các điểm đình chỉ. 
7. Nếu cả mã thông báo trước và mã thông báo hiện tại đều là số, hãy chèn chính xác một khoảng trắng. 

Điều này xử lý các chuỗi trong đó các số chỉ được phân tách bằng dấu cách. 
8. Thêm mã thông báo hiện tại. 
9. In chuỗi được xây dựng lại. 

### Tại sao nó hoạt động 

Thuật toán tách phân tích cú pháp khỏi định dạng. 

Trong quá trình mã hóa, mọi mã thông báo có ý nghĩa sẽ được giữ nguyên chính xác một lần và mọi khoảng trống ban đầu sẽ bị loại bỏ. Điều này đảm bảo rằng không có khoảng cách không chính xác nào tồn tại trong giai đoạn tái thiết. 

Trong quá trình xây dựng lại, mọi không gian pháp lý được đưa ra một cách rõ ràng từ các quy tắc định dạng. Vì mỗi quy tắc chỉ phụ thuộc vào các mã thông báo lân cận nên mọi ranh giới giữa các mã thông báo được xử lý chính xác một lần. 

Các quy tắc tái thiết là loại trừ lẫn nhau và đầy đủ. Mọi khoảng trống bắt buộc sẽ được thêm vào và mọi khoảng trống bị cấm sẽ bị bỏ qua. Bởi vì chuỗi cuối cùng chỉ được xây dựng từ những quyết định được kiểm soát này nên đầu ra luôn đáp ứng mọi yêu cầu về định dạng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_number(token):
    return token[0].isdigit()

s = input().rstrip()

tokens = []
n = len(s)
i = 0

while i < n:
    if s[i] == ' ':
        i += 1

    elif s[i].isdigit():
        j = i
        while j < n and s[j].isdigit():
            j += 1
        tokens.append(s[i:j])
        i = j

    elif s[i] == ',':
        tokens.append(',')
        i += 1

    else:
        tokens.append('...')
        i += 3

ans = []

for idx, token in enumerate(tokens):
    if idx > 0:
        prev = tokens[idx - 1]

        need_space = False

        if prev == ',':
            need_space = True
        elif token == '...':
            need_space = True
        elif is_number(prev) and is_number(token):
            need_space = True

        if need_space:
            ans.append(' ')

    ans.append(token)

print(''.join(ans))
```Giai đoạn đầu tiên trích xuất mã thông báo trong khi hoàn toàn bỏ qua khoảng trắng. Đây là sự đơn giản hóa quan trọng. Một khi các khoảng trống biến mất, cấu trúc còn lại sẽ trở nên rõ ràng. 

Vòng phân tích chữ số rất quan trọng vì số có thể chứa nhiều chữ số. Mỗi lần chỉ đọc một ký tự sẽ bị phân chia không chính xác`123`thành ba token. 

các`...`mã thông báo phải được sử dụng dưới dạng một đơn vị. Tiến lên bằng ba ký tự đảm bảo rằng chúng ta không bao giờ diễn giải các dấu chấm một cách riêng lẻ. 

Giai đoạn xây dựng lại chỉ kiểm tra các token lân cận. Biến`need_space`tập trung tất cả logic khoảng cách vào một nơi, giúp xác minh tính chính xác dễ dàng hơn. 

Thứ tự kiểm tra có ý nghĩa quan trọng về mặt khái niệm, mặc dù bản thân các điều kiện tương thích lẫn nhau. Quy tắc dấu phẩy sẽ ghi đè mọi thứ vì dấu phẩy không bao giờ nhận được khoảng trắng ở đầu. Việc triển khai xử lý việc này một cách tự nhiên bằng cách chỉ quyết định có nên chèn khoảng trắng trước mã thông báo hiện tại hay không. 

Một điểm tinh tế là`...`luôn có khoảng trống ở đầu trừ khi đó là mã thông báo đầu tiên. Điều này khớp chính xác với tuyên bố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1,2 ,3,...,     10
```Mã thông báo tạo ra: 

| Vị trí | Mã thông báo được trích xuất | 
| --- | --- | 
| 0 | 1 | 
| 1 | , | 
| 2 | 2 | 
| 3 | , | 
| 4 | 3 | 
| 5 | , | 
| 6 | ... | 
| 7 | , | 
| 8 | 10 | 

Tái thiết: 

| Mã thông báo hiện tại | Mã thông báo trước | Chèn không gian? | Sản lượng hiện tại | 
| --- | --- | --- | --- | 
| 1 | không | Không | 1 | 
| , | 1 | Không | 1, | 
| 2 | , | Có | 1, 2 | 
| , | 2 | Không | 1, 2, | 
| 3 | , | Có | 1, 2, 3 | 
| , | 3 | Không | 1, 2, 3, | 
| ... | , | Có | 1, 2, 3, ... | 
| , | ... | Không | 1, 2, 3, ..., | 
| 10 | , | Có | 1, 2, 3, ..., 10 | 

Đầu ra cuối cùng:```
1, 2, 3, ..., 10
```Ví dụ này chứng minh rằng tất cả các khoảng trống ban đầu có thể được loại bỏ một cách an toàn. Định dạng chính xác xuất hiện hoàn toàn từ các mối quan hệ mã thông báo. 

### Ví dụ 2 

đầu vào:```
12     34...56
```Mã thông báo: 

| Vị trí | Mã thông báo được trích xuất | 
| --- | --- | 
| 0 | 12 | 
| 1 | 34 | 
| 2 | ... | 
| 3 | 56 | 

Tái thiết: 

| Mã thông báo hiện tại | Mã thông báo trước | Chèn không gian? | Sản lượng hiện tại | 
| --- | --- | --- | --- | 
| 12 | không | Không | 12 | 
| 34 | 12 | Có | 12 34 | 
| ... | 34 | Có | 12 34 ... | 
| 56 | ... | Không | 12 34 ...56 | 

Đầu ra cuối cùng:```
12 34 ...56
```Dấu vết này cho thấy rằng khoảng trắng chỉ được chèn vào khi các quy tắc yêu cầu chúng một cách rõ ràng. Không có khoảng trống tự động sau`...`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được quét một số lần không đổi | 
| Không gian | O(n) | Cả mã thông báo và chuỗi đầu ra đều lưu trữ tối đa O(n) ký tự | 

Kích thước đầu vào cực kỳ nhỏ nhưng giải pháp tuyến tính vẫn được ưa chuộng hơn vì nó giữ cho logic rõ ràng và có thể dự đoán được. Thuật toán thoải mái phù hợp trong mọi giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    def is_number(token):
        return token[0].isdigit()

    s = input().rstrip()

    tokens = []
    n = len(s)
    i = 0

    while i < n:
        if s[i] == ' ':
            i += 1

        elif s[i].isdigit():
            j = i
            while j < n and s[j].isdigit():
                j += 1
            tokens.append(s[i:j])
            i = j

        elif s[i] == ',':
            tokens.append(',')
            i += 1

        else:
            tokens.append('...')
            i += 3

    ans = []

    for idx, token in enumerate(tokens):
        if idx > 0:
            prev = tokens[idx - 1]

            need_space = False

            if prev == ',':
                need_space = True
            elif token == '...':
                need_space = True
            elif is_number(prev) and is_number(token):
                need_space = True

            if need_space:
                ans.append(' ')

        ans.append(token)

    print(''.join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("1,2 ,3,...,     10\n") == "1, 2, 3, ..., 10", "sample 1"

# minimum size
assert run("7\n") == "7", "single token"

# spaces between numbers
assert run("12     34\n") == "12 34", "collapse spaces"

# comma formatting
assert run("1 ,2,3\n") == "1, 2, 3", "remove spaces before commas"

# dots formatting
assert run("1,...,2\n") == "1, ..., 2", "space before dots"

# no space after dots unless required elsewhere
assert run("...123\n") == "...123", "dots at beginning"

# larger mixed case
assert run("10,20   30,...40\n") == "10, 20 30, ...40", "mixed formatting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`|`7`| Đầu vào hợp lệ tối thiểu | 
|`12     34`|`12 34`| Thu gọn khoảng trắng giữa các số | 
|`1 ,2,3`|`1, 2, 3`| Xóa khoảng trắng không hợp lệ trước dấu phẩy | 
|`1,...,2`|`1, ..., 2`| Xử lý đúng`...`| 
|`...123`|`...123`| Không có khoảng trống bắt buộc sau dấu chấm | 
|`10,20   30,...40`|`10, 20 30, ...40`| Nhiều quy tắc định dạng tương tác | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 ,2
```Mã thông báo bỏ qua hoàn toàn khoảng trắng và tạo ra:```
["1", ",", "2"]
```Trong quá trình xây dựng lại, dấu phẩy được nối trực tiếp sau`1`, Và`2`nhận được chính xác một khoảng trắng ở đầu vì nó theo sau dấu phẩy. 

Đầu ra trở thành:```
1, 2
```Điều này sẽ loại bỏ chính xác khoảng trống bất hợp pháp trước dấu phẩy. 

Bây giờ hãy xem xét:```
...1
```Các mã thông báo là:```
["...", "1"]
```Mã thông báo đầu tiên không bao giờ nhận được khoảng trắng ở đầu vì nó bắt đầu chuỗi. số`1`cũng không nhận được khoảng trắng ở đầu vì mã thông báo trước đó không phải là dấu phẩy hay số khác. 

Kết quả cuối cùng là:```
...1
```Điều này xác nhận rằng thuật toán không phát minh sai khoảng trắng sau`...`. 

Tiếp theo, kiểm tra:```
1     2
```Các mã thông báo trở thành:```
["1", "2"]
```Vì cả hai mã thông báo liền kề đều là số nên việc xây dựng lại sẽ chèn chính xác một khoảng trắng giữa chúng. 

Đầu ra là:```
1 2
```Tất cả các khoảng trắng thừa sẽ biến mất trong khi vẫn giữ được dấu phân cách cần thiết. 

Cuối cùng, hãy xem xét:```
1,...,2
```Trình tự mã thông báo là:```
["1", ",", "...", ",", "2"]
```các`...`mã thông báo nhận được khoảng trống ở đầu vì đây không phải là mã thông báo đầu tiên. số`2`nhận được một khoảng trống ở đầu vì nó theo sau dấu phẩy. 

Chuỗi được xây dựng lại trở thành:```
1, ..., 2
```Điều này chứng tỏ rằng dấu phẩy và điểm treo tương tác chính xác ngay cả khi không có khoảng trắng trong dữ liệu đầu vào ban đầu.
