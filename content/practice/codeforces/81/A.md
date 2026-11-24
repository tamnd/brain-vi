---
title: "CF 81A - Trình cắm"
description: "Chúng ta được cung cấp một chuỗi chữ thường có thể chứa các lần nhấn phím lặp lại do vô tình. Bất cứ khi nào hai ký tự bằng nhau trở nên liền kề thì cả hai ký tự đó phải bị xóa. Sau khi loại bỏ một cặp, các cặp bằng nhau liền kề mới có thể xuất hiện và những cặp đó cũng phải bị loại bỏ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 81
codeforces_index: "A"
codeforces_contest_name: "Yandex.Algorithm Open 2011: Qualification 1"
rating: 1400
weight: 81
solve_time_s: 96
verified: true
draft: false
---

[CF 81A - Plug-in](https://codeforces.com/problemset/problem/81/A) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ thường có thể chứa các lần nhấn phím lặp lại do vô tình. Bất cứ khi nào hai ký tự bằng nhau trở nên liền kề thì cả hai ký tự đó phải bị xóa. Sau khi loại bỏ một cặp, các cặp bằng nhau liền kề mới có thể xuất hiện và những cặp đó cũng phải bị loại bỏ. 

Quá trình tiếp tục cho đến khi không còn ký tự bằng nhau liền kề. Chuỗi còn lại cuối cùng phải được in. 

Ví dụ, hãy xem xét chuỗi`abccba`. 

Đầu tiên, cặp`cc`biến mất:```
abccba -> abba
```Bây giờ là một cặp mới`bb`trở nên liền kề:```
abba -> aa
```Cuối cùng:```
aa -> ""
```Chi tiết quan trọng là việc xóa có thể dẫn đến nhiều lần xóa hơn sau này. Giải pháp chỉ xóa các cặp hiển thị trong chuỗi gốc sẽ sai. 

Độ dài đầu vào có thể đạt tới`2 * 10^5`, điều này ngay lập tức loại trừ việc liên tục xây dựng lại chuỗi từ đầu. Bất kỳ thuật toán nào quét toàn bộ chuỗi nhiều lần đều có thể trở thành bậc hai. Với 200.000 ký tự,`O(n^2)`Cách tiếp cận này có thể yêu cầu hàng chục tỷ thao tác, vượt xa giới hạn thời gian. 

Cần có một giải pháp tuyến tính hoặc gần tuyến tính. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
aaaa
```Việc triển khai bất cẩn có thể chỉ xóa cặp đầu tiên và dừng lại:```
aaaa -> aa
```Nhưng phần còn lại`aa`cũng phải biến mất nên kết quả đúng là chuỗi rỗng. 

Một trường hợp phức tạp khác là:```
abccba
```Các ký tự bằng nhau cuối cùng bị hủy không nằm liền kề trong chuỗi gốc. Chúng chỉ trở nên liền kề sau khi xóa trước đó. Bất kỳ cách tiếp cận nào xử lý các cặp độc lập mà không tính đến các phản ứng dây chuyền sẽ thất bại ở đây. 

Kịch bản quan trọng thứ ba là các ký tự xen kẽ:```
abababab
```Không có cặp bằng nhau liền kề nào tồn tại, vì vậy đầu ra phải không thay đổi. Thuật toán phải tránh xóa các chữ cái bằng nhau không liền kề. 

Cuối cùng, hành vi ranh giới có vấn đề. Đối với đầu vào:```
aaab
```Hai cái đầu tiên`a`ký tự biến mất:```
aaab -> ab
```Đĩa đơn còn lại`a`phải ở lại. Di chuyển chỉ mục không chính xác sau khi xóa thường gây ra lỗi ở đây. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là quét liên tục chuỗi để tìm các ký tự bằng nhau liền kề. Bất cứ khi nào tìm thấy một cái, hãy xóa cặp đó và khởi động lại quá trình quét. 

Ví dụ:```
abbaca
```trở thành:```
aaca
```sau đó:```
ca
```Cách tiếp cận này đúng vì nó mô phỏng chính xác lời phát biểu vấn đề. 

Vấn đề là hiệu quả. Việc xóa các ký tự ở giữa chuỗi rất tốn kém và việc khởi động lại quá trình quét liên tục sẽ khiến mọi việc trở nên tồi tệ hơn. Trong trường hợp xấu nhất, mỗi lần xóa có thể yêu cầu dịch chuyển gần như toàn bộ chuỗi. 

Giả sử độ dài đầu vào là`n`. Chúng tôi có thể thực hiện`O(n)`việc xóa và mỗi lần xóa có thể phải trả giá`O(n)`thời gian do sao chép hoặc xây dựng lại chuỗi. Tổng độ phức tạp trở thành`O(n^2)`. 

Với`n = 200000`, hành vi bậc hai quá chậm. 

Quan sát quan trọng là chỉ có ký tự còn sót lại gần đây nhất mới quan trọng khi xử lý một ký tự mới. 

Hãy tưởng tượng xây dựng chuỗi cuối cùng từ trái sang phải. 

Nếu ký tự hiện tại khác với ký tự cuối cùng còn sót lại thì hiện tại nó phải được giữ nguyên. 

Nếu trùng với ký tự cuối cùng còn sống sót thì hai ký tự đó sẽ ngay lập tức triệt tiêu lẫn nhau. 

Hành vi này chính xác là những gì ngăn xếp cung cấp. 

Chúng tôi duy trì một ngăn xếp chứa các ký tự hiện còn tồn tại sau khi xử lý tiền tố của chuỗi. 

Khi đọc một ký tự mới: 

Nếu nó khớp với đỉnh ngăn xếp, chúng ta sẽ bật lên trên cùng vì cặp biến mất. 

Nếu không, chúng tôi sẽ đẩy nhân vật. 

Mỗi ký tự được đẩy nhiều nhất một lần và xuất hiện nhiều nhất một lần, tạo ra độ phức tạp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Giải pháp ngăn xếp tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một ngăn xếp trống. 
2. Xử lý chuỗi từ trái sang phải, mỗi lần một ký tự. 
3. Đối với ký tự hiện tại, kiểm tra xem ngăn xếp có trống không và phần tử trên cùng của nó có bằng ký tự hiện tại hay không. 
4. Nếu chúng bằng nhau, hãy loại bỏ phần tử trên cùng khỏi ngăn xếp. 

Mô hình này xóa một cặp bằng nhau liên tiếp. 
5. Nếu không, hãy đẩy ký tự hiện tại vào ngăn xếp. 

Nhân vật này hiện đang sống sót và có thể tương tác với các nhân vật trong tương lai. 
6. Sau khi xử lý tất cả các ký tự, ngăn xếp chứa chính xác chuỗi còn lại. 
7. Nối các ký tự trong ngăn xếp lại với nhau và in kết quả. 

### Tại sao nó hoạt động 

Ngăn xếp luôn lưu trữ kết quả được xử lý đầy đủ cho tiền tố được thấy cho đến nay. 

Giả sử chúng ta đã xử lý xong dữ liệu đầu tiên`i`ký tự một cách chính xác. Khi nhân vật`i + 1`đến, chỉ có thể hình thành một cặp liền kề mới, giữa ký tự này và ký tự cuối cùng còn sống hiện tại. 

Nếu chúng bằng nhau, cả hai đều biến mất, vì vậy việc bật lên là chính xác. 

Nếu chúng khác nhau thì việc xóa liên quan đến ký tự mới vẫn chưa thể thực hiện được, vì vậy việc đẩy là đúng. 

Vì mỗi bước đều bảo toàn dạng tiền tố được xử lý chính xác nên ngăn xếp cuối cùng chính xác là chuỗi được rút gọn hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    stack = []

    for ch in s:
        if stack and stack[-1] == ch:
            stack.pop()
        else:
            stack.append(ch)

    print("".join(stack))

solve()
```Giải pháp tuân theo ý tưởng ngăn xếp trực tiếp. 

các`stack`list lưu trữ các ký tự còn sót lại sau khi xử lý tiền tố hiện tại. 

Đối với mỗi nhân vật`ch`, chúng tôi so sánh nó với đỉnh ngăn xếp. 

Nếu cả hai đều bằng nhau, chúng tạo thành một cặp bằng nhau liền kề trong chuỗi rút gọn hiện tại, vì vậy chúng ta loại bỏ phần trên cùng bằng`pop()`. 

Nếu không thì hiện tại nhân vật vẫn tồn tại nên chúng tôi sẽ thêm nó vào. 

Cuối cùng, ngăn xếp đã chứa câu trả lời cuối cùng theo thứ tự, vì vậy`"".join(stack)`xây dựng chuỗi kết quả một cách hiệu quả. 

Sử dụng danh sách Python làm ngăn xếp là điều quan trọng. Nối thêm và bật lên từ cuối đều là`O(1)`hoạt động. 

Một lỗi phổ biến là liên tục nối các chuỗi trong quá trình xử lý. Vì các chuỗi Python là bất biến nên việc ghép nối lặp đi lặp lại sẽ tạo ra các chuỗi mới nhiều lần và có thể giảm xuống độ phức tạp bậc hai. 

Một chi tiết tinh tế khác là sử dụng`strip()`khi đọc đầu vào. Dòng đầu vào kết thúc bằng ký tự dòng mới và chúng tôi không muốn dòng mới đó được xử lý như một phần của chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
hhoowaaaareyyoouu
```| Nhân vật hiện tại | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | 
| h | "" | đẩy | h | 
| h | h | bật | "" | 
| o | "" | đẩy | o | 
| o | o | bật | "" | 
| w | "" | đẩy | w | 
| một | w | đẩy | wa | 
| một | wa | bật | w | 
| một | w | đẩy | wa | 
| một | wa | bật | w | 
| r | w | đẩy | wr | 
| e | wr | đẩy | wre | 
| y | wre | đẩy | con khốn | 
| y | con khốn | bật | wre | 
| o | wre | đẩy | wreo | 
| o | wreo | bật | wre | 
| bạn | wre | đẩy | bạn | 
| bạn | bạn | bật | wre | 

Đầu ra cuối cùng:```
wre
```Ví dụ này cho thấy phản ứng dây chuyền lặp đi lặp lại. Một số cặp biến mất ngay lập tức và các ký tự sau này tiếp tục tương tác với trạng thái ngăn xếp được cập nhật. 

### Ví dụ 2 

đầu vào:```
abccba
```| Nhân vật hiện tại | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | 
| một | "" | đẩy | một | 
| b | một | đẩy | ab | 
| c | ab | đẩy | abc | 
| c | abc | bật | ab | 
| b | ab | bật | một | 
| một | một | bật | "" | 

Đầu ra cuối cùng:```
""
```Dấu vết này cho thấy tại sao các phản ứng dây chuyền lại quan trọng. Việc xóa đầu tiên sẽ tạo ra một cặp liền kề mới, tạo ra một cặp khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được đẩy một lần và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | Ngăn xếp có thể lưu trữ toàn bộ chuỗi trong trường hợp xấu nhất | 

Với tối đa 200.000 ký tự, độ phức tạp tuyến tính dễ dàng nằm trong giới hạn. Các hoạt động ngăn xếp có thời gian không đổi, do đó quá trình triển khai diễn ra hiệu quả ngay cả trên các đầu vào lớn nhất. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    s = input().strip()

    stack = []

    for ch in s:
        if stack and stack[-1] == ch:
            stack.pop()
        else:
            stack.append(ch)

    print("".join(stack))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("hhoowaaaareyyoouu\n") == "wre", "sample 1"

# minimum size
assert run("a\n") == "a", "single character"

# all equal
assert run("aaaaaa\n") == "", "all characters removed"

# chain reactions
assert run("abccba\n") == "", "multiple cascading removals"

# no removals
assert run("abababab\n") == "abababab", "alternating characters"

# boundary behavior
assert run("aaab\n") == "ab", "remaining character after deletion"

# large-style pattern
assert run("aabbccddeeff\n") == "", "repeated adjacent pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Kích thước đầu vào tối thiểu | 
|`aaaaaa`|`""`| Xóa tầng lặp đi lặp lại | 
|`abccba`|`""`| Phản ứng dây chuyền sau khi loại bỏ trung gian | 
|`abababab`|`abababab`| Không xảy ra việc xóa | 
|`aaab`|`ab`| Xử lý đúng sau khi xóa một phần | 
|`aabbccddeeff`|`""`| Nhiều cặp liền kề độc lập | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
aaaa
```Thực thi:```
stack = []
read 'a' -> push -> [a]
read 'a' -> pop  -> []
read 'a' -> push -> [a]
read 'a' -> pop  -> []
```Đầu ra cuối cùng:```
""
```Điều này xác nhận rằng thuật toán xử lý một cách tự nhiên việc xóa xếp tầng lặp đi lặp lại mà không cần quét lại chuỗi. 

Bây giờ hãy xem xét:```
abccba
```Thực thi:```
[a]
[a, b]
[a, b, c]
[a, b]
[a]
[]
```Chi tiết quan trọng là thứ hai`b`chỉ hủy sau khi`cc`cặp biến mất. Ngăn xếp tự động nắm bắt vùng lân cận mới này. 

Cuối cùng, hãy xem xét:```
aaab
```Thực thi:```
[a]
[]
[a]
[a, b]
```Đầu ra cuối cùng:```
ab
```Điều này xác minh việc xử lý chính xác khi một số lẻ các ký tự giống hệt nhau xuất hiện liên tiếp. Chỉ những cặp hoàn chỉnh mới biến mất và nhân vật chưa từng có còn lại sẽ sống sót.
