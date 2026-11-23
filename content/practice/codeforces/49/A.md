---
title: "CF 49A - Thám Tử"
description: "Chúng ta được cho một câu đại diện cho một câu hỏi. Câu luôn kết thúc bằng dấu chấm hỏi nhưng câu trả lời được xác định bằng chữ cái cuối cùng trước dấu chấm hỏi đó. Nếu chữ cái cuối cùng đó là nguyên âm thì câu trả lời đúng là \"CÓ\"."YES\"."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 49
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 46 (Div. 2)"
rating: 800
weight: 49
solve_time_s: 81
verified: true
draft: false
---
[CF 49A - Thám tử](https://codeforces.com/problemset/problem/49/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một câu đại diện cho một câu hỏi. Câu luôn kết thúc bằng dấu chấm hỏi nhưng câu trả lời được xác định bằng chữ cái cuối cùng trước dấu chấm hỏi đó. Nếu chữ cái cuối cùng là nguyên âm thì câu trả lời đúng là`"YES"`. Ngược lại, phản hồi là`"NO"`. 

Các nguyên âm trong bài toán này hơi khác thường vì`Y`cũng được coi là một nguyên âm. Cả chữ hoa và chữ thường đều có thể xuất hiện, vì vậy việc so sánh phải không phân biệt chữ hoa chữ thường. 

Độ dài đầu vào tối đa là 100 ký tự, rất nhỏ. Ngay cả việc quét toàn bộ chuỗi nhiều lần cũng sẽ dễ dàng nằm trong giới hạn. Vấn đề hoàn toàn là xác định chính xác chữ cái cuối cùng và kiểm tra xem nó có thuộc bộ nguyên âm hay không. 

Phần khó khăn là ký tự cuối cùng luôn là`?`, nhưng câu trả lời phụ thuộc vào chữ cái cuối cùng, không phải ký tự cuối cùng. Khoảng trống cũng không được tính. Việc thực hiện bất cẩn để kiểm tra`s[-1]`sẽ luôn luôn kiểm tra`?`và thất bại ngay lập tức. 

Hãy xem xét đầu vào này:```
Is it a melon?
```Chữ cái cuối cùng là`n`, là phụ âm nên đáp án đúng là:```
NO
```Một sai lầm dễ mắc phải khác là quên chuyển đổi chữ hoa chữ thường. Ví dụ:```
A?
```Chữ cái cuối cùng là chữ hoa`A`, vẫn là nguyên âm. Nếu không chuyển đổi sang chữ thường hoặc xử lý rõ ràng cả hai trường hợp, chương trình có thể in không chính xác`"NO"`. 

Theo tuyên bố, dấu cách không xuất hiện, nhưng dấu cách bên trong câu rất quan trọng vì chữ cái cuối cùng có thể cách xa phần cuối. Ví dụ:```
Is this a UFO ?
```Bức thư có ý nghĩa cuối cùng là`O`, không phải khoảng trắng trước`?`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là quét ngược từ cuối chuỗi cho đến khi gặp một chữ cái. Khi chúng tôi tìm thấy nó, chúng tôi kiểm tra xem nó có thuộc bộ nguyên âm hay không`{a, e, i, o, u, y}`. Nếu có, chúng tôi in`"YES"`, nếu không thì`"NO"`. 

Một cách giải thích mạnh mẽ sẽ là kiểm tra mọi ký tự trong chuỗi và ghi nhớ ký tự chữ cái cuối cùng được nhìn thấy cho đến nay. Vì độ dài chuỗi tối đa là 100 nên chuỗi này chạy rất nhanh. Trường hợp xấu nhất chỉ thực hiện kiểm tra 100 ký tự. 

Chúng ta có thể làm cho quá trình triển khai trở nên rõ ràng hơn một chút bằng cách khai thác cấu trúc của đầu vào. Chuỗi luôn kết thúc bằng`?`, và đảm bảo có ít nhất một chữ cái. Do đó, việc quét ngược sẽ ngay lập tức tìm thấy ký tự có liên quan mà không cần lưu trữ thêm bất cứ thứ gì. 

Điều quan trọng là chỉ có một ký tự quan trọng, đó là chữ cái cuối cùng trong câu. Mọi thứ khác đều không liên quan. Khi chúng tôi tách biệt chữ cái đó, vấn đề sẽ giảm xuống thành một bài kiểm tra tư cách thành viên đơn giản trong một bộ nguyên âm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc toàn bộ dòng đầu vào dưới dạng một chuỗi. 
2. Tạo một bộ chứa tất cả các nguyên âm viết thường:```
{a, e, i, o, u, y}
```3. Di chuyển chuỗi từ phải sang trái. 
4. Bỏ qua bất kỳ ký tự nào không phải là chữ cái. 

Điều này bỏ qua trận chung kết`?`và bất kỳ khoảng trống nào ở gần cuối. 
5. Khi tìm thấy chữ cái đầu tiên, hãy chuyển nó thành chữ thường. 
6. Kiểm tra xem chữ thường này có thuộc bộ nguyên âm hay không. 

Nếu có thì in`"YES"`. Ngược lại, in`"NO"`. 
7. Dừng ngay sau khi xử lý chữ cái hợp lệ đầu tiên vì chỉ có chữ cái cuối cùng mới quan trọng. 

### Tại sao nó hoạt động 

Quá trình quét ngược đảm bảo rằng ký tự chữ cái đầu tiên chúng ta gặp chính xác là chữ cái cuối cùng của câu hỏi. Luật chơi chỉ phụ thuộc vào chữ cái đó. Việc chuyển đổi sang chữ thường sẽ loại bỏ mọi sự phân biệt giữa chữ đầu vào viết hoa và chữ thường, do đó, tư cách thành viên trong bộ nguyên âm sẽ xác định chính xác câu trả lời trong mọi trường hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

vowels = set("aeiouy")

for ch in reversed(s):
    if ch.isalpha():
        if ch.lower() in vowels:
            print("YES")
        else:
            print("NO")
        break
```Chương trình bắt đầu bằng cách đọc toàn bộ câu hỏi dưới dạng một chuỗi. sử dụng`strip()`loại bỏ dòng mới ở cuối được giới thiệu bằng cách đọc đầu vào. 

Bộ nguyên âm được lưu trữ ở dạng chữ thường. Điều này cho phép chúng ta chuẩn hóa từng chữ cái ứng cử viên với`lower()`trước khi kiểm tra tư cách thành viên. 

Vòng lặp lặp qua chuỗi theo thứ tự ngược lại. Đây là cách rõ ràng nhất để xác định chữ cái có ý nghĩa cuối cùng vì đầu vào luôn kết thúc bằng`?`. 

các`isalpha()`kiểm tra là cần thiết. Nếu không có nó, chương trình sẽ kiểm tra dấu câu hoặc dấu cách thay vì các chữ cái. Ngay khi tìm thấy một chữ cái hợp lệ, chương trình sẽ xác định xem đó có phải là nguyên âm hay không và in ngay câu trả lời. 

các`break`tuyên bố quan trọng vì chỉ có chữ cái cuối cùng mới ảnh hưởng đến kết quả. Việc tiếp tục quét có thể vô tình ghi đè lên quyết định đúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Is it a melon?
```| Nhân vật hiện tại | Là Thư | Chữ thường | Nguyên âm? | Hành động | 
| --- | --- | --- | --- | --- | 
|`?`| Không | - | - | Bỏ qua | 
|`n`| Có |`n`| Không | In`NO`| 

Quá trình quét bỏ qua dấu chấm hỏi và ngay lập tức đạt đến`n`. Từ`n`không có trong bộ nguyên âm, câu trả lời là`"NO"`. 

### Ví dụ 2 

đầu vào:```
Are YOU happy?
```| Nhân vật hiện tại | Là Thư | Chữ thường | Nguyên âm? | Hành động | 
| --- | --- | --- | --- | --- | 
|`?`| Không | - | - | Bỏ qua | 
|`y`| Có |`y`| Có | In`YES`| 

Ví dụ này thể hiện hai chi tiết. Đầu tiên, thuật toán bỏ qua một cách chính xác`?`. Thứ hai,`y`được coi là một nguyên âm trong bài toán này, vì vậy câu trả lời trở thành`"YES"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Trong trường hợp xấu nhất, chúng tôi quét toàn bộ chuỗi một lần | 
| Không gian | O(1) | Chỉ một bộ nguyên âm nhỏ và một vài biến được sử dụng | 

Vì độ dài đầu vào không bao giờ vượt quá 100 ký tự nên ngay cả việc quét tuyến tính cũng có hiệu quả ngay lập tức. Việc sử dụng bộ nhớ là không đổi và thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s = input().strip()

    vowels = set("aeiouy")

    for ch in reversed(s):
        if ch.isalpha():
            if ch.lower() in vowels:
                print("YES")
            else:
                print("NO")
            break

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("Is it a melon?\n") == "NO\n", "sample 1"

# custom cases
assert run("A?\n") == "YES\n", "single uppercase vowel"
assert run("b?\n") == "NO\n", "single lowercase consonant"
assert run("Why?\n") == "YES\n", "y treated as vowel"
assert run("Is this a UFO ?\n") == "YES\n", "space before question mark"
assert run(("a" * 99) + "?\n") == "YES\n", "maximum length style case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A?`|`YES`| Xử lý nguyên âm viết hoa | 
|`b?`|`NO`| Trường hợp phụ âm đơn | 
|`Why?`|`YES`|`y`tính là nguyên âm | 
|`Is this a UFO ?`|`YES`| Bỏ qua khoảng trắng trước`?`| 
|`aaaa...a?`|`YES`| Đầu vào dài gần kích thước tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
A?
```Thuật toán quét từ phía sau. Nó bỏ qua`?`, đạt`A`, chuyển đổi nó thành chữ thường`a`, và tìm thấy nó trong tập nguyên âm. Đầu ra trở thành:```
YES
```Điều này xác nhận rằng chữ in hoa được xử lý chính xác. 

Bây giờ hãy xem xét:```
Is this a UFO ?
```Quá trình di chuyển ngược lại diễn ra như sau:```
? -> skip
(space) -> skip
O -> valid letter
```Sau khi chuyển đổi`O`sang chữ thường`o`, thuật toán xác định nó là nguyên âm và in:```
YES
```Trường hợp này xác nhận rằng khoảng trống gần cuối không ảnh hưởng đến việc xác định vị trí chữ cái cuối cùng. 

Cuối cùng, kiểm tra:```
Why?
```Chữ cái cuối cùng là`y`. Một số triển khai quên điều đó`y`được bao gồm trong số các nguyên âm trong vấn đề này. Thuật toán lưu trữ rõ ràng`y`bên trong bộ nguyên âm, để nó in chính xác:```
YES
```
