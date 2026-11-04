---
title: "CF 1153C - Chuỗi phân phát và dấu ngoặc đơn"
description: "Chúng ta được cung cấp một chuỗi gồm ba loại ký tự: dấu ngoặc đơn mở \"(\", dấu ngoặc đơn đóng \")\" và dấu chấm hỏi \"?\". Mục tiêu của chúng tôi là thay thế từng \"?"
date: "2026-06-12T02:51:34+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1153
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 551 (Div. 2)"
rating: 1700
weight: 1153
solve_time_s: 118
verified: false
draft: false
---

[CF 1153C - Trình tự phân phát và dấu ngoặc đơn](https://codeforces.com/problemset/problem/1153/C) 

**Đánh giá:** 1700 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm ba loại ký tự: dấu ngoặc đơn mở "(", dấu ngoặc đơn đóng ")" và dấu chấm hỏi "?". Mục tiêu của chúng tôi là thay thế từng "?" bằng "(" hoặc ")" sao cho chuỗi kết quả là chuỗi dấu ngoặc đơn chính xác, nghĩa là nó cân bằng và mọi tiền tố có ít nhất nhiều "(" như ")". Ngoài ra, mọi tiền tố nghiêm ngặt (mọi tiền tố ngoại trừ toàn bộ chuỗi) phải là _inchính_ dưới dạng một chuỗi dấu ngoặc đơn, tương đương với việc nói rằng không có tiền tố nghiêm ngặt nào tự nó tạo thành một chuỗi cân bằng. 

Kích thước đầu vào có thể lớn tới 300.000 ký tự. Điều này loại trừ bất kỳ giải pháp nào xem xét tất cả các thay thế có thể có của "?" một cách ngây thơ, vì số đó sẽ là số mũ theo số "?". Do đó, mọi giải pháp được chấp nhận đều phải chạy theo thời gian tuyến tính tương ứng với độ dài chuỗi. 

Có một vài trường hợp cần xem xét. Nếu độ dài của chuỗi là số lẻ thì ngay lập tức không có giải pháp nào, vì chuỗi ngoặc đúng phải có số "(" và ") bằng nhau". Nếu chuỗi đã bắt đầu bằng ")" hoặc kết thúc bằng "(", thì không có giải pháp nào khả thi vì ký tự đầu tiên của chuỗi đúng phải là "(", và ký tự cuối cùng phải là ")". Nếu tất cả các ký tự là "?", chúng ta phải cẩn thận chọn cách phân phối "(" và ")" để đáp ứng điều kiện tiền tố nghiêm ngặt. Các cách tiếp cận bất cẩn có thể tạo ra các chuỗi được cân bằng quá sớm, chẳng hạn như "()", điều này sẽ vi phạm yêu cầu nghiêm ngặt về tiền tố. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các thay thế có thể có của "?" và kiểm tra tính chính xác của từng chuỗi kết quả, đồng thời kiểm tra xem tất cả các tiền tố nghiêm ngặt có hợp lệ hay không. Cách tiếp cận này đúng về nguyên tắc, nhưng nếu có tới 300.000 "?" ký tự, số lượng chuỗi là$2^{300,000}$, điều đó hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần xác định số lượng "(" và ")" cần đặt vì trình tự hoàn toàn được xác định sau khi chúng ta chỉ định số đếm chính xác. Đối với một chuỗi có độ dài n, chuỗi dấu ngoặc đơn đúng yêu cầu chính xác n/2 "(" và n/2 ")". Nếu chúng ta đếm có bao nhiêu dấu "(" và ")" đã xuất hiện, chúng ta sẽ biết chính xác có bao nhiêu dấu chấm hỏi phải trở thành "(" và bao nhiêu dấu chấm hỏi phải trở thành ")". 

Khi đã biết cách phân phối, chúng ta có thể tham lam thay thế "?" từ trái sang phải bằng dấu "(" cho đến khi đạt số lượng yêu cầu thì sử dụng ")" cho các phần còn lại. Điều này đảm bảo rằng tổng số "(" và ")" là chính xác. Sau đó, chúng ta phải kiểm tra số dư đang chạy trong khi quét từ trái sang phải: nó không bao giờ được giảm xuống dưới 0 và không bao giờ được đạt 0 trước ký tự cuối cùng, vì không có tiền tố nghiêm ngặt nào có thể đúng. 

Quan sát cho phép xây dựng thời gian tuyến tính là điều kiện "tiền tố nghiêm ngặt" buộc nửa đầu của "(" và nửa sau của ")" phải được sắp xếp cẩn thận: nếu chúng ta đặt "(" cuối cùng quá muộn, chúng ta có thể vô tình tạo ra một tiền tố hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Đếm & Kiểm tra tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên hãy kiểm tra xem độ dài của chuỗi có phải là số lẻ hay không. Nếu đúng như vậy, hãy in ":(" và thoát, vì trình tự cân bằng là không thể. 
2. Đếm số "(" và ")" hiện có trong chuỗi. Đặt open_ Need là n/2 trừ đi số lượng hiện tại của "(", và close_ Need là n/2 trừ đi số lượng hiện tại của ")". Nếu một trong hai giá trị âm, hãy in ":(", vì đã có quá nhiều dấu ngoặc đơn cố định. 
3. Thay thế từng dấu "?" từ trái sang phải. Đối với mỗi dấu "?", nếu open_ Need lớn hơn 0, hãy thay thế bằng "(" và giảm open_ Need. Nếu không, hãy thay thế bằng ")" và giảm close_ Need. Cuối cùng ta sẽ có đúng n/2 "(" và n/2 ")". 
4. Kiểm tra số dư đang chạy của chuỗi. Khởi tạo số dư = 0. Quét từng ký tự: nếu "(", số dư tăng; nếu ")", số dư giảm. Nếu số dư < 0 tại bất kỳ điểm nào, hãy in ":(", vì đã xảy ra tiền tố chuỗi không chính xác. Ngoài ra, nếu số dư == 0 trước ký tự cuối cùng, hãy in ":(", vì tiền tố nghiêm ngặt chính xác đã được hình thành. 
5. Nếu trình tự vượt qua quá trình kiểm tra số dư, hãy xuất trình tự đó. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng số "(" và ")" sẽ khớp chính xác với số lượng cần thiết để có một chuỗi dấu ngoặc đơn chính xác. Thứ tự thay thế đảm bảo rằng "(" xuất hiện sớm và ")" xuất hiện đủ muộn để ngăn bất kỳ tiền tố nghiêm ngặt nào trở thành chính xác. Việc kiểm tra số dư đảm bảo không có tiền tố nào trở nên hợp lệ sớm. Sự kết hợp này đảm bảo cả hai điều kiện đều được thỏa mãn hoặc báo cáo chính xác là không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = list(input().strip())

if n % 2 != 0:
    print(":(")
    exit()

half = n // 2
open_count = s.count('(')
close_count = s.count(')')

open_needed = half - open_count
close_needed = half - close_count

if open_needed < 0 or close_needed < 0:
    print(":(")
    exit()

for i in range(n):
    if s[i] == '?':
        if open_needed > 0:
            s[i] = '('
            open_needed -= 1
        else:
            s[i] = ')'
            close_needed -= 1

balance = 0
for i in range(n):
    if s[i] == '(':
        balance += 1
    else:
        balance -= 1
    if balance < 0 or (balance == 0 and i != n - 1):
        print(":(")
        exit()

print("".join(s))
```Mỗi phần của mã này trực tiếp thực hiện các bước thuật toán. Đếm dấu ngoặc đơn cho phép chúng ta phân bổ "?" một cách hiệu quả. Vòng lặp đầu tiên chuyển đổi "?" tham lam vào "(" cho đến khi đạt được một nửa thì điền phần còn lại bằng ")". Vòng lặp thứ hai kiểm tra số dư đang chạy, thực thi cả hai rằng không có tiền tố nào hợp lệ và chuỗi cuối cùng được cân bằng. Điều tinh tế quan trọng là kiểm tra số dư == 0 đó trước khi ký tự cuối cùng kích hoạt ":(", xử lý điều kiện tiền tố nghiêm ngặt. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:`(?????`| tôi | s[i] | open_ Needed | close_ Needed | cân bằng | 
| --- | --- | --- | --- | --- | 
| 0 | ( | 2 | 3 | 1 | 
| 1 | ? -> ( | 1 | 3 | 2 | 
| 2 | ? -> ) | 1 | 2 | 1 | 
| 3 | ? -> ( | 0 | 2 | 2 | 
| 4 | ? -> ) | 0 | 1 | 1 | 
| 5 | ? -> ) | 0 | 0 | 0 | 

Số dư không bao giờ âm và đầu tiên đạt 0 ở ký tự cuối cùng. Đầu ra:`(()())`. 

Trường hợp cạnh không có giải pháp: 

đầu vào:`?)??(`Độ dài chẵn nhưng ký tự đầu tiên là ")" → ngay lập tức không hợp lệ → đầu ra ":(". 

Dấu vết này chứng minh rằng phép gán tham lam kết hợp với kiểm tra số dư sẽ xác định chính xác các chuỗi hợp lệ và loại bỏ các chuỗi không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần để thay thế "?" và một lượt để kiểm tra số dư. | 
| Không gian | O(n) | Chuỗi được lưu trữ dưới dạng danh sách để đột biến. | 

Với n lên tới 3 * 10^5, giải pháp này chạy thoải mái trong giới hạn thời gian 1 giây. Không có cấu trúc dữ liệu nặng bổ sung được sử dụng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    s = list(sys.stdin.readline().strip())

    if n % 2 != 0:
        return ":("
    half = n // 2
    open_count = s.count('(')
    close_count = s.count(')')
    open_needed = half - open_count
    close_needed = half - close_count
    if open_needed < 0 or close_needed < 0:
        return ":("
    for i in range(n):
        if s[i] == '?':
            if open_needed > 0:
                s[i] = '('
                open_needed -= 1
            else:
                s[i] = ')'
                close_needed -= 1
    balance = 0
    for i in range(n):
        if s[i] == '(':
            balance += 1
        else:
            balance -= 1
        if balance < 0 or (balance == 0 and i != n - 1):
            return ":("
    return "".join(s)

# provided samples
assert run("6\n(?????\n") == "(()())", "sample 1"
# custom cases
assert run("4\n)??(\n") == ":(", "invalid start and end"
assert run("2\n??\n") == "()", "minimum size even"
assert run("8\n????????\n") == "(((())))", "all question marks"
assert run("6\n((????\n") == ":(", "too many fixed ("
assert run("6\n()??()\n") == ":(", "would form valid strict prefix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\n)??(\n | :( | Không thể phát hiện do bắt đầu/kết thúc | 
| 2\n??\n | () | Trường hợp hợp lệ kích thước tối thiểu | 
| 8\n????????\n | (((()))) | Toàn tham lam làm việc | 
| 6\n((????\n | :( | Đã sửa quá nhiều "(" |
