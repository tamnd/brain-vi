---
title: "CF 102700K - Loại thảm họa"
description: "Mỗi chuỗi đầu vào có một cấu trúc rất cụ thể. Nó bắt đầu bằng một chuỗi các chữ cái viết thường không trống, ngay sau đó là một chuỗi các chữ số không trống. Chúng ta có hai chuỗi như vậy và phải so sánh chúng bằng cách sử dụng thứ tự tùy chỉnh."
date: "2026-08-05T12:31:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "K"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 57
verified: true
draft: false
---

[CF 102700K - Loại thảm khốc](https://codeforces.com/problemset/problem/102700/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chuỗi đầu vào có một cấu trúc rất cụ thể. Nó bắt đầu bằng một chuỗi các chữ cái viết thường không trống, ngay sau đó là một chuỗi các chữ số không trống. Chúng ta có hai chuỗi như vậy và phải so sánh chúng bằng cách sử dụng thứ tự tùy chỉnh. 

Việc so sánh hoạt động chính xác như thứ tự từ điển thông thường cho đến khi đạt đến hậu tố số. Sự khác biệt là toàn bộ hậu tố số được coi là một số nguyên duy nhất thay vì một chuỗi ký tự. Ví dụ,`"z2"`nên đến trước`"z11"`vì các số nguyên thỏa mãn`2 < 11`, mặc dù nhân vật`'2'`về mặt từ điển lớn hơn`'1'`. 

Mỗi chuỗi có thể dài bằng$10^5$nhân vật. Vì chỉ có hai chuỗi nên việc đọc đầu vào cần có thời gian tuyến tính. Bất kỳ giải pháp nào liên tục quét các chuỗi hoặc thực hiện phép tính bậc hai đều không cần thiết. Một lần đi qua mỗi chuỗi là đủ nhanh. Hậu tố số có thể chứa tới$10^5$các chữ số, do đó, chuyển đổi nó thành số nguyên là một ý tưởng tồi trong các ngôn ngữ có kiểu số nguyên có kích thước cố định vì nó sẽ tràn. Ngay cả trong Python, việc xây dựng một số nguyên khổng lồ như vậy cũng chậm hơn mức cần thiết. So sánh các phần số dưới dạng chuỗi chữ số là đủ. 

Một số trường hợp đáng chú ý. 

Giả sử các tiền tố chữ cái khác nhau.```
abd14
abc14
```Câu trả lời đúng là`>`bởi vì`"abd"`về mặt từ điển lớn hơn`"abc"`. Việc triển khai bất cẩn luôn so sánh các phần số trước sẽ báo cáo sai sự bằng nhau. 

Giả sử các tiền tố bằng nhau nhưng các số có độ dài khác nhau.```
x9
x10
```Câu trả lời đúng là`<`. So sánh các chuỗi số theo từ điển sẽ kết luận sai`"9" > "10"`. Đối với các số nguyên không có số 0 đứng đầu, chuỗi chữ số dài hơn luôn biểu thị số lớn hơn. 

Giả sử cả hai chuỗi đều giống hệt nhau.```
asgfsd4213456
asgfsd4213456
```Câu trả lời đúng là`=`. Sau khi cả tiền tố và phần số khớp nhau, phép so sánh phải báo cáo sự bằng nhau thay vì dẫn đến kết quả tùy ý. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là chia mỗi chuỗi thành các phần chữ cái và số, chuyển hậu tố số thành số nguyên, sau đó so sánh tiền tố trước và số nguyên thứ hai. Điều này tạo ra thứ tự chính xác vì nó khớp chính xác với định nghĩa vấn đề. 

Điểm yếu là chuyển đổi số nguyên. Hậu tố số có thể chứa tới$10^5$chữ số, vượt xa khả năng của các loại số nguyên tiêu chuẩn trong hầu hết các ngôn ngữ. Ngay cả những ngôn ngữ có số nguyên chính xác tùy ý cũng phải phân bổ và xử lý một đối tượng số nguyên khổng lồ, đây là công việc không cần thiết. 

Quan sát quan trọng là các phần số không bao giờ chứa các số 0 đứng đầu. Điều đó mang lại một tài sản rất thuận tiện. Nếu hai số nguyên dương có số chữ số khác nhau thì số nào có nhiều chữ số hơn sẽ lớn hơn. Chỉ khi số lượng chữ số bằng nhau thì chúng ta mới cần so sánh từ điển của các chuỗi chữ số, bởi vì các biểu diễn thập phân có độ dài bằng nhau sẽ bảo toàn thứ tự số. 

Điều này cho phép chúng ta so sánh các số mà không cần phân tích chúng dưới dạng số nguyên. Chúng tôi chỉ chia mỗi chuỗi một lần, so sánh các tiền tố chữ cái, so sánh độ dài của hậu tố số nếu cần và cuối cùng so sánh các chuỗi chữ số khi độ dài khớp nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Được chấp nhận trong Python, không an toàn do chuyển đổi số nguyên lớn | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi đầu vào. 
2. Đối với mỗi chuỗi, quét từ đầu cho đến khi xuất hiện chữ số đầu tiên. Mọi thứ trước vị trí đó là tiền tố chữ cái và mọi thứ từ vị trí đó trở đi là hậu tố số. 
3. So sánh các tiền tố chữ cái theo từ điển. Nếu chúng khác nhau thì xuất ra`<`hoặc`>`theo thứ tự từ điển của chúng và dừng lại. Phần số không liên quan khi các tiền tố khác nhau. 
4. So sánh độ dài của các hậu tố số. Nếu một hậu tố có nhiều chữ số hơn, nó đại diện cho số nguyên lớn hơn vì các số 0 đứng đầu bị cấm. 
5. Nếu các hậu tố số có cùng độ dài, hãy so sánh chúng theo từ điển dưới dạng chuỗi. Các chuỗi thập phân có độ dài bằng nhau có cùng thứ tự với các số nguyên tương ứng của chúng. 
6. Đầu ra`<`,`>`, hoặc`=`theo kết quả. 

### Tại sao nó hoạt động 

Sự so sánh luôn tuân theo cùng một thứ tự như trật tự Katastrophic. Tiền tố chữ cái xác định kết quả bất cứ khi nào chúng khác nhau vì chúng xuất hiện trước hậu tố số. Khi các tiền tố bằng nhau, chỉ các số nguyên được biểu diễn mới quan trọng. Không có số 0 đứng đầu, so sánh số nguyên tương đương với việc so sánh số chữ số đầu tiên và thứ tự từ điển thứ hai. Mọi quyết định do thuật toán đưa ra đều khớp chính xác với các quy tắc này, do đó thứ tự được tạo ra luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def split_parts(s):
    i = 0
    while i < len(s) and s[i].isalpha():
        i += 1
    return s[:i], s[i:]

a = input().strip()
b = input().strip()

pa, na = split_parts(a)
pb, nb = split_parts(b)

if pa < pb:
    print("<")
elif pa > pb:
    print(">")
else:
    if len(na) < len(nb):
        print("<")
    elif len(na) > len(nb):
        print(">")
    else:
        if na < nb:
            print("<")
        elif na > nb:
            print(">")
        else:
            print("=")
```Hàm trợ giúp thực hiện chính xác một lần quét cho đến chữ số đầu tiên, tạo ra hai phần bắt buộc của mỗi chuỗi. Vì định dạng đầu vào đảm bảo rằng mọi chuỗi bao gồm các chữ cái theo sau là các chữ số nên không cần xác thực bổ sung. 

Sự so sánh đầu tiên là giữa các tiền tố chữ cái vì chúng có mức độ ưu tiên cao hơn trong thứ tự. Chỉ khi chúng giống hệt nhau thì thuật toán mới tiếp tục sử dụng các hậu tố số. 

Thay vì chuyển đổi các hậu tố số thành số nguyên, việc triển khai trước tiên sẽ so sánh độ dài của chúng. Điều này tránh tràn và xây dựng số nguyên lớn không cần thiết. Chỉ khi độ dài khớp nhau thì nó mới so sánh trực tiếp các chuỗi chữ số, tương đương về mặt toán học với so sánh số nguyên cho các biểu diễn thập phân có độ dài bằng nhau không có số 0 đứng đầu. 

Việc thực hiện cẩn thận bảo tồn thứ tự so sánh. Hoán đổi so sánh số trước so sánh tiền tố sẽ tạo ra kết quả không chính xác bất cứ khi nào các tiền tố chữ cái khác nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
z2
z11
```| Bước | Tiền tố 1 | Tiền tố 2 | Số 1 | Số 2 | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Chia | z | z | 2 | 11 | Tiền tố bằng | 
| So sánh độ dài | z | z | 1 | 2 | Số đầu tiên nhỏ hơn | 

Đầu ra:```
<
```Ví dụ này cho thấy tại sao độ dài số lại quan trọng. So sánh từng nhân vật`"2"`Và`"11"`sẽ tạo ra thứ tự sai. 

### Mẫu 2 

đầu vào:```
abd14
abc14
```| Bước | Tiền tố 1 | Tiền tố 2 | Số 1 | Số 2 | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Chia | abd | abc | 14 | 14 | Tiền tố khác nhau | 
| So sánh tiền tố | abd | abc | 14 | 14 |`abd > abc`| 

Đầu ra:```
>
```Ví dụ này chứng minh rằng hậu tố số bị bỏ qua khi tiền tố chữ cái xác định thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi được quét một lần, sau đó là so sánh chuỗi tuyến tính | 
| Không gian | O(n) | Các chuỗi tiền tố và hậu tố được cắt cùng nhau chứa tất cả các ký tự gốc | 

Thời gian chạy là tuyến tính trong tổng kích thước đầu vào, đây là mức tối ưu vì mỗi ký tự phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ cũng tuyến tính do các chuỗi con được tạo, vừa vặn thoải mái trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    def split_parts(s):
        i = 0
        while i < len(s) and s[i].isalpha():
            i += 1
        return s[:i], s[i:]

    a = input().strip()
    b = input().strip()

    pa, na = split_parts(a)
    pb, nb = split_parts(b)

    if pa < pb:
        print("<")
    elif pa > pb:
        print(">")
    else:
        if len(na) < len(nb):
            print("<")
        elif len(na) > len(nb):
            print(">")
        else:
            if na < nb:
                print("<")
            elif na > nb:
                print(">")
            else:
                print("=")

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

assert run("z2\nz11\n") == "<\n", "sample 1"
assert run("abd14\nabc14\n") == ">\n", "sample 2"
assert run("asgfsd4213456\nasgfsd4213456\n") == "=\n", "sample 3"

assert run("a1\na2\n") == "<\n", "minimum numeric comparison"
assert run("b999\nb1000\n") == "<\n", "different digit counts"
assert run("zzz5\nzza999\n") == ">\n", "prefix comparison dominates"
assert run("abc12345678901234567890\nabc12345678901234567891\n") == "<\n", "very large numbers without integer conversion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a1`,`a2`|`<`| So sánh hợp lệ nhỏ nhất | 
|`b999`,`b1000`|`<`| So sánh số theo số chữ số | 
|`zzz5`,`zza999`|`>`| So sánh tiền tố được ưu tiên | 
| Số có độ dài bằng nhau rất dài |`<`| Không cần chuyển đổi số nguyên | 

## Vỏ cạnh 

Xem xét đầu vào```
abd14
abc14
```Thuật toán chia chuỗi thành`"abd"`Và`"14"`, Và`"abc"`Và`"14"`. Các tiền tố khác nhau ngay lập tức nên nó so sánh`"abd"`với`"abc"`và đầu ra`>`. Các hậu tố số không bao giờ được kiểm tra vì chúng không thể ảnh hưởng đến kết quả khi các tiền tố khác nhau. 

Xem xét đầu vào```
x9
x10
```Cả hai tiền tố đều là`"x"`, do đó thuật toán tiếp tục so sánh số. Các hậu tố có độ dài`1`Và`2`, làm`"10"`số nguyên lớn hơn. Thuật toán xuất ra chính xác`<`mà không chuyển đổi một trong hai số thành số nguyên. 

Xem xét đầu vào```
asgfsd4213456
asgfsd4213456
```Các tiền tố khớp chính xác, các hậu tố số có cùng độ dài và các chuỗi chữ số giống hệt nhau. Mọi so sánh đều báo cáo sự bằng nhau, vì vậy kết quả cuối cùng là`=`. Điều này xác nhận rằng thuật toán không báo cáo sai một chuỗi lớn hơn khi mọi thành phần đều khớp.
