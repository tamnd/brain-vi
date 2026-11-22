---
title: "CF 291B - Đối số dòng lệnh"
description: "Chúng ta được cung cấp một chuỗi dòng lệnh duy nhất được viết bằng hệ điều hành Pindows hư cấu. Một dòng lệnh bao gồm các từ vị, là các mã thông báo được truyền cho một chương trình. Có hai cách một từ vị có thể xuất hiện. Một từ vựng thông thường là một chuỗi tối đa các ký tự không phải khoảng trắng."
date: "2026-06-05T16:51:38+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 291
codeforces_index: "B"
codeforces_contest_name: "Croc Champ 2013 - Qualification Round"
rating: 1300
weight: 291
solve_time_s: 137
verified: false
draft: false
---

[CF 291B - Đối số dòng lệnh](https://codeforces.com/problemset/problem/291/B) 

**Đánh giá:** 1300 
**Tas:** *đặc biệt, triển khai, chuỗi 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dòng lệnh duy nhất được viết bằng hệ điều hành Pindows hư cấu. Một dòng lệnh bao gồm các từ vị, là các mã thông báo được truyền cho một chương trình. 

Có hai cách một từ vị có thể xuất hiện. 

Một từ vựng thông thường là một chuỗi tối đa các ký tự không phải khoảng trắng. Ví dụ: trong:```
run.exe one two
```các từ vựng là`run.exe`,`one`, Và`two`. 

Một từ vựng được trích dẫn bắt đầu bằng`"`và kết thúc bằng sự phù hợp`"`. Mọi thứ giữa những dấu ngoặc kép đó đều thuộc về một từ vị, bao gồm cả dấu cách. Nội dung thậm chí có thể trống rỗng. Ví dụ:```
"a b" ""
```chứa hai từ vựng:`a b`và chuỗi trống. 

Đầu vào được đảm bảo là một dòng lệnh hợp lệ. Các trích dẫn không bao giờ mơ hồ, các khối trích dẫn được đóng đúng cách và mỗi từ vị được phân tách khỏi các từ vị lân cận bằng dấu cách hoặc ở cuối chuỗi. 

Nhiệm vụ của chúng ta chỉ đơn giản là phân tích dòng lệnh và in mọi từ vựng trên dòng riêng của nó, được bao quanh bởi`<`Và`>`. 

Độ dài chuỗi tối đa là$10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liên tục sao chép các chuỗi con lớn hoặc quét lại các phần của đầu vào. Quét tuyến tính là mục tiêu tự nhiên vì chúng tôi chỉ cần xác định ranh giới mã thông báo một lần. 

Một số trường hợp đáng chú ý. 

Hãy xem xét một đối số được trích dẫn trống:```
""
```Đầu ra đúng là:```
<>
```Trình phân tích cú pháp giả sử mọi mã thông báo chứa ít nhất một ký tự sẽ vô tình loại bỏ đối số này. 

Hãy xem xét một đối số được trích dẫn có chứa dấu cách:```
"a b c"
```Đầu ra đúng là:```
<a b c>
```Việc chia đầu vào bằng dấu cách sẽ tạo ra ba phần riêng biệt không chính xác. 

Hãy xem xét một đối số được trích dẫn bao gồm một khoảng trắng:```
" "
```Đầu ra đúng là:```
< >
```Điều này khác với một đối số trống, vì vậy chúng ta phải giữ nguyên chính xác mọi ký tự bên trong dấu ngoặc kép. 

Cuối cùng, hãy xem xét dấu câu liền kề:```
run.exe one, two!
```Đầu ra đúng là:```
<run.exe>
<one,>
<two!>
```Dấu câu là một phần của từ vị. Chỉ có dấu cách và dấu ngoặc kép mới có ý nghĩa đặc biệt. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xử lý chuỗi ký tự theo ký tự và xây dựng từ vị hiện tại. Bất cứ khi nào chúng ta gặp một khoảng trống bên ngoài dấu ngoặc kép, chúng ta sẽ hoàn thành từ vựng hiện tại. Bất cứ khi nào chúng tôi gặp một câu trích dẫn, chúng tôi sẽ tìm câu trích dẫn kết thúc phù hợp và coi mọi thứ giữa chúng là một mã thông báo. 

Việc triển khai bạo lực có thể liên tục tìm kiếm các trích dẫn phù hợp và liên tục tạo các chuỗi con mới. Vì mỗi tìm kiếm có thể duyệt qua một phần lớn của chuỗi nên độ phức tạp trong trường hợp xấu nhất có thể giảm xuống theo hướng$O(n^2)$. Với$n = 10^5$, điều đó trở nên quá đắt. 

Cấu trúc của đầu vào cho chúng ta một lựa chọn tốt hơn nhiều. Mỗi nhân vật thuộc về đúng một trong ba loại: 

1. Dấu cách ở ngoài dấu ngoặc kép. 
2. Một ký tự bên trong từ vựng được trích dẫn. 
3. Một ký tự của một từ vựng không được trích dẫn. 

Khi một ký tự đã được xử lý, chúng ta không bao giờ cần phải xem lại ký tự đó nữa. Điều này gợi ý một lần quét từ trái sang phải. 

Quan sát quan trọng là các từ vựng được trích dẫn luôn bắt đầu bằng`"`và câu lệnh đảm bảo dòng lệnh là hợp lệ. Ngay khi nhìn thấy báo giá mở đầu, chúng tôi có thể tiếp tục tiến lên cho đến báo giá đóng tương ứng. Nội dung giữa chúng tạo thành một từ vị hoàn chỉnh. Đối với các từ vựng không được trích dẫn, chúng ta tiến tới khoảng trống tiếp theo. 

Mỗi ký tự được truy cập với số lần không đổi, đưa ra giải pháp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc toàn bộ dòng đầu vào. 
2. Duy trì chỉ mục`i`trỏ tới vị trí hiện tại. 
3. Nếu`s[i]`là một không gian, tiến lên`i`. 

Các khoảng trắng bên ngoài từ vị chỉ là dấu phân cách nên chúng không thuộc về bất kỳ mã thông báo nào. 
4. Nếu`s[i]`là một trích dẫn kép, hãy bắt đầu phân tích một từ vựng được trích dẫn. 

Đi qua dấu ngoặc kép mở đầu, ghi nhớ vị trí bắt đầu và tiếp tục cho đến dấu ngoặc kép đóng phù hợp. Chuỗi con giữa các dấu ngoặc kép đó là một từ vị. 
5. Lưu trữ từ vựng đó và chuyển qua dấu ngoặc kép kết thúc. 
6. Nếu không, hãy bắt đầu phân tích một từ vựng không được trích dẫn. 

Tiếp tục di chuyển về phía trước cho đến khi đến một khoảng trống hoặc đến cuối chuỗi. Các ký tự được duyệt qua tạo thành một từ vị. 
7. Lưu trữ từ vựng. 
8. Lặp lại cho đến khi toàn bộ chuỗi được xử lý. 
9. In từng từ vựng được lưu trữ bao quanh bởi`<`Và`>`. 

### Tại sao nó hoạt động 

Ở mọi vị trí, trình phân tích cú pháp nằm ngoài một từ vị hoặc bên trong chính xác một từ vị. Dữ liệu đầu vào đảm bảo rằng các trích dẫn được khớp chính xác và các từ vị được phân tách bằng dấu cách hoặc ranh giới chuỗi. 

Khi gặp một trích dẫn, theo định nghĩa, mọi ký tự cho đến trích dẫn phù hợp đều thuộc cùng một từ vị. Khi gặp một ký tự không phải dấu ngoặc kép, không phải khoảng trắng, từ vị sẽ mở rộng cho đến khoảng trắng phân cách tiếp theo. Đây chính xác là các quy tắc được sử dụng bởi đặc tả dòng lệnh. 

Vì mỗi từ vị được trích xuất theo các điều kiện biên xác định của nó và mỗi ký tự thuộc về chính xác một vùng được xử lý, thuật toán đưa ra chính xác các từ vị được biểu thị bằng dòng lệnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().rstrip('\n')

    n = len(s)
    i = 0
    tokens = []

    while i < n:
        if s[i] == ' ':
            i += 1
            continue

        if s[i] == '"':
            i += 1
            start = i

            while i < n and s[i] != '"':
                i += 1

            tokens.append(s[start:i])
            i += 1  # skip closing quote
        else:
            start = i

            while i < n and s[i] != ' ':
                i += 1

            tokens.append(s[start:i])

    sys.stdout.write('\n'.join(f'<{token}>' for token in tokens))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo logic phân tích cú pháp trực tiếp. 

Vòng lặp bên ngoài luôn trỏ đến ký tự chưa được xử lý đầu tiên. Các dấu cách sẽ bị bỏ qua ngay lập tức vì chúng không bao giờ thuộc về một từ vị. 

Khi gặp báo giá, mã sẽ ghi lại vị trí ngay sau báo giá mở đầu và tăng dần cho đến báo giá đóng cửa. lát cắt`s[start:i]`chứa chính xác nội dung được trích dẫn. Điều này xử lý các chuỗi trống một cách tự nhiên vì nếu các dấu ngoặc kép liền kề,`start == i`và lát cắt trống. 

Đối với các từ vựng thông thường, trình phân tích cú pháp sẽ tiến tới khoảng trắng tiếp theo. Vì dấu ngoặc kép không thể xuất hiện bên trong từ vựng không được trích dẫn ở đầu vào hợp lệ nên không cần kiểm tra bổ sung. 

Một sai lầm phổ biến là sử dụng`split()`trên đầu vào. Điều đó phá hủy thông tin về các phần được trích dẫn và không thể biểu thị các đối số trống. Một điểm tinh tế khác là bảo toàn khoảng trống bên trong các từ vị được trích dẫn. Việc trích xuất chuỗi con thực hiện việc này một cách tự động vì chỉ các ký tự trích dẫn bên ngoài mới bị xóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
"RUn.exe O" "" "   2ne, " two! . " "
```| Bước | Loại vị trí | Trích xuất từ ​​vựng | 
| --- | --- | --- | 
| 1 | Đã trích dẫn |`RUn.exe O`| 
| 2 | Đã trích dẫn | `` | 
| 3 | Đã trích dẫn |`  2ne,`| 
| 4 | Không trích dẫn |`two!`| 
| 5 | Không trích dẫn |`.`| 
| 6 | Đã trích dẫn |` `| 

Đầu ra:```
<RUn.exe O>
<>
<   2ne, >
<two!>
<.>
< >
```Ví dụ này thể hiện đồng thời tất cả các trường hợp đặc biệt: dấu cách bên trong dấu ngoặc kép, đối số trống, đối số thông thường và đối số được trích dẫn bao gồm một khoảng trắng. 

### Ví dụ 2 

đầu vào:```
run.exe one two
```| Bước | Loại vị trí | Trích xuất từ ​​vựng | 
| --- | --- | --- | 
| 1 | Không trích dẫn |`run.exe`| 
| 2 | Không trích dẫn |`one`| 
| 3 | Không trích dẫn |`two`| 

Đầu ra:```
<run.exe>
<one>
<two>
```Dấu vết này cho thấy trường hợp đơn giản hơn trong đó mọi từ vị đều không được trích dẫn và các khoảng trắng đóng vai trò hoàn toàn là dấu phân cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra tối đa một số lần không đổi | 
| Không gian | O(n) | Các từ vị được lưu trữ cùng nhau chứa tối đa n ký tự | 

Với độ dài đầu vào tối đa$10^5$, quét tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng tuyến tính theo kích thước đầu vào và nằm trong giới hạn khả dụng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    s = inp.rstrip('\n')

    n = len(s)
    i = 0
    tokens = []

    while i < n:
        if s[i] == ' ':
            i += 1
            continue

        if s[i] == '"':
            i += 1
            start = i

            while i < n and s[i] != '"':
                i += 1

            tokens.append(s[start:i])
            i += 1
        else:
            start = i

            while i < n and s[i] != ' ':
                i += 1

            tokens.append(s[start:i])

    return '\n'.join(f'<{x}>' for x in tokens)

# provided sample
assert run('"RUn.exe O" "" "   2ne, " two! . " "') == (
    "<RUn.exe O>\n"
    "<>\n"
    "<   2ne, >\n"
    "<two!>\n"
    "<.>\n"
    "< >"
), "sample 1"

# custom cases
assert run('""') == "<>", "empty quoted argument"

assert run('"a b c"') == "<a b c>", "spaces inside quoted token"

assert run('abc') == "<abc>", "single unquoted token"

assert run('one two three') == (
    "<one>\n<two>\n<three>"
), "multiple ordinary tokens"

assert run('" " "."') == (
    "< >\n<.>"
), "space token and punctuation token"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`""`|`<>`| Từ vựng trích dẫn trống | 
|`"a b c"`|`<a b c>`| Khoảng trống được bảo toàn bên trong dấu ngoặc kép | 
|`abc`|`<abc>`| Lệnh thông thường nhỏ nhất | 
|`one two three`| Ba từ vựng riêng biệt | Xử lý dải phân cách tiêu chuẩn | 
|`" " "."`|`< >`,`<.>`| Phân biệt khoảng trắng với chuỗi trống | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
""
```Trình phân tích cú pháp nhìn thấy trích dẫn mở đầu, chuyển sang trích dẫn tiếp theo ngay lập tức và trích xuất chuỗi con giữa chúng. Chuỗi con đó có độ dài bằng 0, vì vậy từ vựng được lưu trữ là chuỗi trống. Đầu ra trở thành:```
<>
```Không cần mã trường hợp đặc biệt. 

Coi như:```
" "
```Sau khi vào chế độ trích dẫn, trình phân tích cú pháp sẽ thu thập khoảng trắng giữa các dấu ngoặc kép. Chuỗi con được trích xuất là`" "`còn hơn là`""`, sản xuất:```
< >
```Điều này xác nhận rằng khoảng trắng bên trong dấu ngoặc kép là dữ liệu, không phải dấu phân cách. 

Coi như:```
"a b" c
```Từ vựng đầu tiên được phân tích cú pháp hoàn toàn bên trong dấu ngoặc kép, mang lại`a b`. Sau dấu ngoặc kép, trình phân tích cú pháp bỏ qua khoảng trắng và phân tích từ vựng không được trích dẫn`c`. Đầu ra là:```
<a b>
<c>
```Khoảng trắng bên trong vẫn là một phần của đối số đầu tiên vì chế độ trích dẫn sẽ bỏ qua ngữ nghĩa của dấu phân cách cho đến khi đạt được dấu ngoặc kép đóng phù hợp. 

Coi như:```
run.exe one, two!
```Trình phân tích cú pháp không bao giờ xử lý dấu câu một cách đặc biệt. Nó chỉ dừng phân tích cú pháp không trích dẫn khi đạt đến một khoảng trắng. Các từ vựng được trích xuất là:```
<run.exe>
<one,>
<two!>
```Điều này xác minh rằng dấu câu thuộc về mã thông báo chính xác như nó xuất hiện trong đầu vào.
