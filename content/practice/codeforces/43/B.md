---
title: "CF 43B - Thư"
description: "Chúng tôi được cung cấp hai chuỗi. Chuỗi đầu tiên là tiêu đề của tờ báo và chuỗi thứ hai là bức thư nặc danh mà Vasya muốn xây dựng từ đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 43
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 42 (Div. 2)"
rating: 1100
weight: 43
solve_time_s: 91
verified: true
draft: false
---
[CF 43B - Thư](https://codeforces.com/problemset/problem/43/B) 

**Đánh giá:** 1100 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi. Chuỗi đầu tiên là tiêu đề của tờ báo và chuỗi thứ hai là bức thư nặc danh mà Vasya muốn xây dựng từ đó. 

Mỗi ký tự không phải dấu cách trong dòng tiêu đề chỉ được sử dụng tối đa một lần. Không gian rất đặc biệt, chúng không tiêu thụ bất cứ thứ gì. Nếu văn bản chữ cái có chứa khoảng trắng, Vasya có thể chỉ cần để lại khoảng trống giữa các ký tự mà không cần các ký tự khoảng trắng thực tế từ dòng tiêu đề. 

Nhiệm vụ là xác định xem mọi chữ cái cần thiết trong văn bản mục tiêu có thể được cung cấp bởi dòng tiêu đề với tần suất đủ hay không. Chữ hoa và chữ thường được coi là các ký tự khác nhau, vì vậy`'A'`Và`'a'`không thể thay thế cho nhau. 

Các chuỗi ngắn, dài tối đa 200 ký tự. Ngay cả một giải pháp không hiệu quả cũng sẽ vừa vặn thoải mái trong giới hạn, vì quét bậc hai trên 200 ký tự chỉ thực hiện được khoảng 40.000 thao tác. Điều này có nghĩa là vấn đề không nằm ở việc tối ưu hóa nâng cao mà là ở việc xử lý số lượng ký tự một cách chính xác. 

Phần khó khăn là các khoảng trống phải được bỏ qua hoàn toàn. Việc triển khai bất cẩn có thể tính khoảng trắng là ký tự bình thường và từ chối các câu trả lời hợp lệ một cách không chính xác. 

Hãy xem xét đầu vào này:```
abc
a c
```Câu trả lời đúng là:```
YES
```Tiêu đề không chứa dấu cách nhưng cũng không cần phải cắt dấu cách nên vẫn có thể viết chữ. 

Phân biệt chữ hoa chữ thường là một nguồn lỗi phổ biến khác.```
Abc
abc
```Câu trả lời đúng là:```
NO
```Tiêu đề có chứa chữ hoa`A`, nhưng chữ cái này yêu cầu viết thường`a`. 

Một trường hợp tinh tế khác xuất hiện khi một ký tự tồn tại trong dòng tiêu đề nhưng không đủ số lần.```
hello
helloo
```Câu trả lời đúng là:```
NO
```Giải pháp chỉ kiểm tra xem mỗi ký tự có xuất hiện ít nhất một lần hay không sẽ thất bại vì mục tiêu cần hai ký tự.`'o'`nhân vật. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp nhất là xử lý từng ký tự chuỗi đích. Đối với mọi ký tự không phải khoảng trắng trong thư, hãy quét qua dòng tiêu đề cho đến khi tìm thấy ký tự không được sử dụng phù hợp. Khi sử dụng xong hãy đánh dấu ký tự đó để sau này không sử dụng lại được. 

Điều này có hiệu quả vì mỗi chữ cái trong mục tiêu sử dụng rõ ràng một chữ cái phù hợp từ nguồn. Vấn đề là mỗi lần tra cứu có thể quét gần như toàn bộ dòng tiêu đề. Nếu cả hai chuỗi đều có độ dài`n`, điều này dẫn đến`O(n^2)`sự phức tạp về thời gian. 

Với`n ≤ 200`, thậm chí cách tiếp cận này cũng dễ dàng vượt qua. Trong trường hợp xấu nhất, chúng tôi làm khoảng`200 × 200 = 40,000`so sánh. Tuy nhiên, phương pháp này còn vụng về vì nó tốn nhiều thời gian tìm kiếm các ký tự mà chúng ta có thể đếm được một lần. 

Quan sát tốt hơn là chỉ có tần số mới quan trọng. Thứ tự các chữ cái trong tiêu đề không liên quan. Nếu bức thư cần ba`'a'`ký tự, dòng tiêu đề phải chứa ít nhất ba ký tự`'a'`nhân vật ở đâu đó. 

Điều đó biến vấn đề thành một nhiệm vụ đếm. Chúng tôi đếm số lần mỗi ký tự không phải dấu cách xuất hiện trong dòng tiêu đề, sau đó duyệt qua văn bản đích và sử dụng một lần xuất hiện cho mỗi ký tự bắt buộc. Nếu tại bất kỳ thời điểm nào một ký tự không có sẵn, câu trả lời sẽ ngay lập tức`"NO"`. 

Điều này làm giảm logic thành thời gian tuyến tính vì mỗi ký tự được xử lý một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi tiêu đề và chuỗi chữ cái đích. 
2. Tạo bản đồ tần suất cho các ký tự trong dòng tiêu đề. 

Chúng tôi chỉ tính các ký tự không phải khoảng trắng vì khoảng trắng là trống và không cần phải cắt khỏi tờ báo. 
3. Duyệt từng ký tự mục tiêu một. 
4. Nếu ký tự hiện tại là khoảng trắng, hãy bỏ qua nó. 

Dấu cách không bao giờ tiêu tốn bất cứ điều gì từ dòng tiêu đề. 
5. Đối với mỗi ký tự không phải khoảng trắng, hãy kiểm tra xem tần số của nó trên bản đồ có dương hay không. 

Nếu số đếm bằng 0 thì dòng tiêu đề không chứa đủ bản sao của ký tự đó, vì vậy hãy in`"NO"`và dừng lại ngay lập tức. 
6. Nếu không, hãy giảm tần số của ký tự đó đi một. 

Mô hình này sử dụng một chữ cái được cắt ra từ dòng tiêu đề. 
7. Nếu toàn bộ chuỗi mục tiêu được xử lý thành công, hãy in`"YES"`. 

### Tại sao nó hoạt động 

Thuật toán duy trì sự bất biến là bản đồ tần số luôn biểu thị các chữ cái chưa sử dụng còn lại có trong dòng tiêu đề. 

Bất cứ khi nào mục tiêu cần một ký tự, chúng tôi sẽ sử dụng một lần xuất hiện phù hợp. Nếu không có ký tự bắt buộc thì không có sự sắp xếp nào có thể xây dựng mục tiêu vì mọi ký tự có thể sử dụng đều đã được tính chính xác một lần. 

Vì các khoảng trắng bị bỏ qua và mọi ký tự khác được khớp với nguồn cung cấp sẵn có nên thuật toán chấp nhận chính xác các chữ cái có thể xây dựng được và loại bỏ tất cả các ký tự không thể xây dựng được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

headline = input().rstrip('\n')
letter = input().rstrip('\n')

freq = {}

for ch in headline:
    if ch != ' ':
        freq[ch] = freq.get(ch, 0) + 1

for ch in letter:
    if ch == ' ':
        continue

    if freq.get(ch, 0) == 0:
        print("NO")
        break

    freq[ch] -= 1
else:
    print("YES")
```Phần đầu tiên đọc cả hai dòng chính xác như đã nhập. sử dụng`rstrip('\n')`rất quan trọng vì chúng tôi chỉ muốn xóa dòng mới ở cuối được thêm vào bởi đầu vào, chứ không phải khoảng trắng bên trong hoặc hàng đầu thuộc về chuỗi. 

Từ điển`freq`lưu trữ bao nhiêu bản sao chưa sử dụng của mỗi ký tự còn lại trong dòng tiêu đề. Các khoảng trắng bị bỏ qua vì chúng không liên quan đến việc xây dựng. 

Vòng lặp thứ hai xử lý chữ cái đích. Bất cứ khi nào một khoảng trắng xuất hiện, thuật toán sẽ bỏ qua nó ngay lập tức. Đối với các ký tự thông thường, việc tra cứu từ điển sẽ kiểm tra xem có còn tồn tại ít nhất một bản sao chưa sử dụng hay không. 

các`for ... else`cấu trúc ở đây thuận tiện. các`else`khối chỉ thực thi nếu vòng lặp kết thúc mà không nhấn`break`. Điều đó có nghĩa là câu trả lời là`"YES"`chính xác khi không phát hiện thấy thiếu ký tự. 

Một chi tiết triển khai tinh tế là việc sử dụng`freq.get(ch, 0)`. Không có giá trị mặc định`0`, việc truy vấn một ký tự chưa bao giờ xuất hiện trong dòng tiêu đề sẽ gây ra lỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Instead of dogging Your footsteps it disappears but you dont notice anything
where is your dog
```Dấu vết: 

| Nhân vật hiện tại | Có sẵn trước | Có sẵn sau | Kết quả | 
| --- | --- | --- | --- | 
| w | 0 | 0 | Thất bại | 
| ký tự còn lại | chưa được xử lý | chưa được xử lý | KHÔNG | 

Thuật toán thất bại ngay lập tức vì chữ thường`'w'`không xuất hiện ở tiêu đề. Dòng tiêu đề chứa chữ in hoa và nhiều chữ cái viết thường nhưng không chứa chữ thường`'w'`. 

Ví dụ này chứng tỏ rằng thuật toán dừng ngay khi phát hiện thấy không thể thực hiện được, thay vì quét một cách không cần thiết. 

### Ví dụ 2 

đầu vào:```
hello world
hello
```Dấu vết: 

| Nhân vật hiện tại | Có sẵn trước | Có sẵn sau | Kết quả | 
| --- | --- | --- | --- | 
| h | 1 | 0 | tiếp tục | 
| e | 1 | 0 | tiếp tục | 
| tôi | 3 | 2 | tiếp tục | 
| tôi | 2 | 1 | tiếp tục | 
| o | 2 | 1 | tiếp tục | 

Đầu ra cuối cùng:```
YES
```Dấu vết này cho thấy các ký tự lặp lại được xử lý chính xác như thế nào. Tính cho`'l'`giảm hai lần vì mục tiêu yêu cầu hai bản sao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi được quét một lần | 
| Không gian | O(n) | Bản đồ tần số lưu trữ số lượng ký tự | 

Độ dài đầu vào tối đa là 200 ký tự, do đó giải pháp sẽ chạy ngay lập tức trong giới hạn. Ngay cả các hoạt động từ điển Python cũng có thời gian cố định một cách hiệu quả cho thang đo này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    headline = input().rstrip('\n')
    letter = input().rstrip('\n')

    freq = {}

    for ch in headline:
        if ch != ' ':
            freq[ch] = freq.get(ch, 0) + 1

    for ch in letter:
        if ch == ' ':
            continue

        if freq.get(ch, 0) == 0:
            print("NO")
            return

        freq[ch] -= 1

    print("YES")

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
assert run(
    "Instead of dogging Your footsteps it disappears but you dont notice anything\n"
    "where is your dog\n"
) == "NO\n", "sample 1"

# minimum-size input
assert run(
    "a\n"
    "a\n"
) == "YES\n", "single character"

# spaces should not matter
assert run(
    "abc\n"
    "a c\n"
) == "YES\n", "spaces ignored"

# case sensitivity
assert run(
    "Abc\n"
    "abc\n"
) == "NO\n", "uppercase and lowercase differ"

# insufficient frequency
assert run(
    "hello\n"
    "helloo\n"
) == "NO\n", "repeated character shortage"

# large repeated input
assert run(
    "a" * 200 + "\n" +
    "a" * 200 + "\n"
) == "YES\n", "maximum equal strings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / a`| CÓ | Trường hợp hợp lệ nhỏ nhất | 
|`abc / a c`| CÓ | Dấu cách không tiêu tốn ký tự | 
|`Abc / abc`| KHÔNG | Phân biệt chữ hoa chữ thường | 
|`hello / helloo`| KHÔNG | Đếm tần số | 
| 200`'a'`nhân vật | CÓ | Xử lý độ dài tối đa | 

## Vỏ cạnh 

Hãy xem xét trường hợp mục tiêu chứa khoảng trắng nhưng dòng tiêu đề thì không. 

đầu vào:```
abc
a b c
```Thuật toán xây dựng bản đồ tần số này:```
a:1, b:1, c:1
```Trong khi quét mục tiêu, các khoảng trắng sẽ bị bỏ qua hoàn toàn. Chỉ một`'a'`,`'b'`, Và`'c'`tiêu thụ số lượng. Tất cả số đếm vẫn hợp lệ, do đó đầu ra trở thành:```
YES
```Điều này xác nhận rằng khoảng trắng được bỏ qua một cách chính xác. 

Bây giờ hãy xem xét phân biệt chữ hoa chữ thường. 

đầu vào:```
A
a
```Bản đồ tần số chỉ chứa chữ hoa`'A'`. 

Khi thuật toán kiểm tra chữ thường`'a'`,`freq.get('a', 0)`trả lại`0`. Thuật toán in ngay lập tức:```
NO
```Điều này chứng tỏ rằng chữ hoa và chữ thường được coi là các ký hiệu khác nhau. 

Cuối cùng, hãy xem xét tình trạng thiếu ký tự lặp lại. 

đầu vào:```
banana
bananaa
```Tần số ban đầu: 

| Nhân vật | Đếm | 
| --- | --- | 
| b | 1 | 
| một | 3 | 
| n | 2 | 

Mục tiêu yêu cầu bốn`'a'`nhân vật. Sau khi tiêu thụ ba phần đầu tiên, số còn lại sẽ bằng 0. Khi thuật toán đạt đến thứ tư`'a'`, nó phát hiện sự thiếu hụt và in:```
NO
```Điều này xác minh rằng thuật toán theo dõi tần số chính xác chứ không phải sự tồn tại đơn giản.
