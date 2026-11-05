---
title: "CF 12B - Giải pháp đúng?"
description: "Alice đưa cho Bob một số thập phân và yêu cầu anh ta sắp xếp lại các chữ số của nó sao cho số kết quả càng nhỏ càng tốt, trong khi vẫn là số thập phân hợp lệ không có số 0 đứng đầu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 12
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 12 (Div 2 Only)"
rating: 1100
weight: 12
solve_time_s: 79
verified: true
draft: false
---
[CF 12B - Giải pháp đúng?](https://codeforces.com/problemset/problem/12/B) 

**Đánh giá:** 1100 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Alice đưa cho Bob một số thập phân và yêu cầu anh ta sắp xếp lại các chữ số của nó sao cho số kết quả càng nhỏ càng tốt, trong khi vẫn là số thập phân hợp lệ không có số 0 đứng đầu. Bob trả về một số khác và chúng ta phải quyết định xem liệu câu trả lời của anh ấy có thực sự là sự sắp xếp nhỏ nhất có thể hay không. 

Nhiệm vụ không phải là tạo ra tất cả các sự sắp xếp lại. Chúng ta chỉ cần kiểm tra xem số của Bob có khớp với cách sắp xếp tối thiểu đúng hay không. 

Số lượng nhiều nhất là$10^9$, vì vậy chúng chứa tối đa 10 chữ số. Mặc dù kích thước đầu vào rất nhỏ nhưng vấn đề vẫn đòi hỏi phải xử lý cẩn thận các số 0 ở đầu. Một loại số ngây thơ như`"0013"`sẽ tạo ra một biểu diễn không hợp lệ vì số thập phân không thể bắt đầu bằng số 0 trừ khi toàn bộ số đó bằng 0. 

Trường hợp cạnh nguy hiểm nhất xuất hiện khi số ban đầu chứa số 0. Ví dụ: 

đầu vào:```
1002
1002
```Sự sắp xếp lại nhỏ nhất đúng là`1002`, không`0012`. Việc triển khai bất cẩn chỉ sắp xếp tất cả các chữ số tăng dần sẽ tạo ra kết quả không chính xác`"0012"`. 

Một tình huống phức tạp khác là khi bản thân con số đó bằng 0. 

đầu vào:```
0
0
```Câu trả lời là hợp lệ vì sự sắp xếp lại duy nhất vẫn bằng không. 

Một loại sai lầm khác xảy ra khi câu trả lời của thí sinh có đúng chữ số nhưng không phải là nhỏ nhất. 

đầu vào:```
310
130
```Các chữ số khớp nhau, nhưng cách sắp xếp hợp lệ nhỏ nhất là`103`, vì vậy đầu ra đúng là`WRONG_ANSWER`. 

Khó khăn cốt lõi là không kiểm tra xem các chữ số có khớp nhau hay không. Thách thức thực sự là xây dựng sự sắp xếp hợp lệ nhỏ nhất về mặt từ điển theo hạn chế “không có số 0 đứng đầu”. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra mọi hoán vị của các chữ số của số ban đầu, loại bỏ những số có số 0 đứng đầu, chuyển đổi các hoán vị còn lại thành số và lấy mức tối thiểu. Sau đó, chúng tôi so sánh mức tối thiểu đó với câu trả lời của Bob. 

Điều này hiệu quả vì nó trực tiếp tuân theo định nghĩa của vấn đề. Mọi sự sắp xếp lại có thể đều được kiểm tra, vì vậy đảm bảo tìm thấy được sự sắp xếp hợp lệ nhỏ nhất. 

Vấn đề là hoán vị tăng theo giai thừa. Một số có 10 chữ số có tới$10! = 3,628,800$hoán vị. Con số đó đã lên tới vài triệu ứng viên và các chữ số trùng lặp sẽ tạo thêm sự phức tạp trong công việc ghi sổ nếu chúng ta muốn tránh phải làm việc lặp lại. Đối với kích thước đầu vào nhỏ như vậy, nó vẫn có thể vượt qua được ở một số ngôn ngữ, nhưng nó tốn kém một cách không cần thiết đối với một vấn đề có giải pháp mang tính xây dựng đơn giản. 

Quan sát quan trọng là số hợp lệ nhỏ nhất phải đặt chữ số nhỏ nhất khác 0 trước vì chữ số đầu tiên có tác động lớn nhất đến giá trị của số. Sau khi sửa chữ số đầu tiên đó, mọi vị trí còn lại phải chứa chữ số nhỏ nhất có sẵn, bao gồm cả số 0. 

Điều này ngay lập tức gợi ý một cách xây dựng tham lam: 

Đầu tiên, sắp xếp tất cả các chữ số tăng dần. 

Nếu chữ số đầu tiên không bằng 0 thì thứ tự sắp xếp đã tối ưu. 

Nếu thứ tự sắp xếp bắt đầu bằng số 0, hãy xác định chữ số đầu tiên khác 0, đặt nó ở phía trước, sau đó nối tất cả các chữ số còn lại theo thứ tự tăng dần. 

Ví dụ:`3100`Các chữ số được sắp xếp:`0013`Chữ số khác 0 đầu tiên là`1`. 

Di chuyển nó lên phía trước:`1003`Đó là sự sắp xếp hợp lệ nhỏ nhất. 

Sau khi xây dựng dạng tối thiểu chuẩn mực này, chúng ta chỉ cần so sánh nó với câu trả lời của Bob. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(d! \cdot d)$|$O(d)$| Quá chậm/không cần thiết | 
| Tối ưu |$O(d \log d)$|$O(d)$| Đã chấp nhận | 

Đây,$d$là số chữ số. 

## Hướng dẫn thuật toán 

1. Đọc số gốc`n`dưới dạng một chuỗi và câu trả lời của Bob`m`như một chuỗi. 

Việc xử lý chúng dưới dạng chuỗi sẽ tránh được các vấn đề với số 0 đứng đầu trong quá trình xử lý. 
2. Chuyển đổi các chữ số của`n`vào một danh sách và sắp xếp chúng theo thứ tự tăng dần. 

Điều này mang lại sự sắp xếp nhỏ nhất về mặt từ điển của các chữ số. 
3. Kiểm tra xem chữ số đầu tiên sau khi sắp xếp có bằng 0 hay không. 

Nếu nó không bằng 0 thì thứ tự sắp xếp đã là số hợp lệ nhỏ nhất. 
4. Nếu chữ số đầu tiên bằng 0, quét từ trái sang phải cho đến khi tìm được chữ số đầu tiên khác 0. 

Chữ số này phải trở thành chữ số đứng đầu vì mọi số hợp lệ đều phải bắt đầu bằng chữ số khác 0. 
5. Hoán đổi chữ số đầu tiên khác 0 đó vào vị trí phía trước. 

Tất cả các chữ số còn lại vẫn được sắp xếp nên số kết quả vẫn ở mức tối thiểu. 
6. Nối các chữ số thành một chuỗi biểu thị sự sắp xếp lại hợp lệ nhỏ nhất. 
7. So sánh kết quả đã xây dựng với câu trả lời của Bob. 

Nếu chúng khớp chính xác, hãy in`OK`. Nếu không thì in`WRONG_ANSWER`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên giá trị vị trí. Chữ số ngoài cùng bên trái đóng góp nhiều nhất vào độ lớn của số, vì vậy việc giảm thiểu nó luôn được ưu tiên cao nhất. 

Trong số tất cả các chữ số khác 0, việc chọn chữ số nhỏ nhất cho vị trí đầu tiên sẽ mang lại đóng góp dẫn đầu nhỏ nhất có thể. Sau khi sửa chữ số đó, các vị trí còn lại phải được giảm thiểu một cách độc lập, điều này đạt được bằng cách sắp xếp các chữ số còn lại tăng dần. 

Bởi vì mọi chữ số sau chữ số đầu tiên được đặt theo thứ tự nhỏ nhất có thể, nên không có sự sắp xếp lại hợp lệ nào khác có thể tạo ra một số nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = input().strip()
    m = input().strip()

    digits = sorted(n)

    if digits[0] == '0':
        for i in range(len(digits)):
            if digits[i] != '0':
                digits[0], digits[i] = digits[i], digits[0]
                break

    smallest = ''.join(digits)

    if smallest == m:
        print("OK")
    else:
        print("WRONG_ANSWER")

solve()
```Giải pháp bắt đầu bằng cách đọc cả hai số dưới dạng chuỗi. Điều này quan trọng vì câu trả lời của Bob có thể chứa các số 0 đứng đầu trong dữ liệu đầu vào và việc chuyển đổi ngay lập tức sang số nguyên sẽ làm mất thông tin đó. 

Các chữ số của`n`được sắp xếp theo thứ tự tăng dần. Nếu chữ số đầu tiên đã khác 0 thì dãy đã sắp xếp ngay lập tức là dãy hợp lệ nhỏ nhất. 

Khi chuỗi được sắp xếp bắt đầu bằng số 0, mã sẽ tìm kiếm chữ số khác 0 đầu tiên và hoán đổi nó vào vị trí đầu tiên. Thao tác này là đủ vì các chữ số còn lại đã được sắp xếp tăng dần. Không cần sắp xếp lại bổ sung. 

So sánh cuối cùng được thực hiện dưới dạng so sánh chuỗi. Điều này an toàn hơn so sánh số vì`"0012"`Và`"12"`đại diện cho các câu trả lời khác nhau trong bối cảnh của vấn đề này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3310
1033
```| Bước | Trạng thái chữ số | Giải thích | 
| --- | --- | --- | 
| Chữ số ban đầu |`['3', '3', '1', '0']`| Số gốc | 
| Sau khi sắp xếp |`['0', '1', '3', '3']`| Thứ tự từ điển nhỏ nhất | 
| Chữ số khác 0 đầu tiên |`1`| Phải trở thành chữ số hàng đầu | 
| Sau khi trao đổi |`['1', '0', '3', '3']`| Sự sắp xếp hợp lệ nhỏ nhất | 
| Chuỗi cuối cùng |`1033`| So sánh với câu trả lời của Bob | 

Câu trả lời được xây dựng khớp với câu trả lời của Bob, vì vậy kết quả đầu ra là:```
OK
```Dấu vết này chứng minh tại sao các số 0 đứng đầu không thể vẫn ở phía trước ngay cả sau khi sắp xếp. 

### Ví dụ 2 

đầu vào:```
310
130
```| Bước | Trạng thái chữ số | Giải thích | 
| --- | --- | --- | 
| Chữ số ban đầu |`['3', '1', '0']`| Số gốc | 
| Sau khi sắp xếp |`['0', '1', '3']`| Nhỏ nhất về mặt từ điển | 
| Chữ số khác 0 đầu tiên |`1`| Chữ số hàng đầu hợp lệ nhỏ nhất | 
| Sau khi trao đổi |`['1', '0', '3']`| Sắp xếp hợp lệ tối thiểu | 
| Chuỗi cuối cùng |`103`| So sánh với câu trả lời của Bob | 

Bob đã trả lời`130`, nhưng sự sắp xếp tối thiểu thực sự là`103`. 

Đầu ra đúng là:```
WRONG_ANSWER
```Ví dụ này cho thấy rằng chỉ các chữ số trùng khớp là không đủ. Sự sắp xếp cũng phải ở mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(d \log d)$| Sắp xếp các chữ số chiếm ưu thế | 
| Không gian |$O(d)$| Lưu trữ danh sách chữ số | 

Số chữ số tối đa chỉ là 10, do đó thời gian chạy thực tế là không đổi trong thực tế. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = input().strip()
    m = input().strip()

    digits = sorted(n)

    if digits[0] == '0':
        for i in range(len(digits)):
            if digits[i] != '0':
                digits[0], digits[i] = digits[i], digits[0]
                break

    smallest = ''.join(digits)

    if smallest == m:
        print("OK")
    else:
        print("WRONG_ANSWER")

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
assert run("3310\n1033\n") == "OK", "sample 1"

# minimum value
assert run("0\n0\n") == "OK", "single zero"

# leading zero trap
assert run("1002\n1002\n") == "OK", "smallest valid arrangement keeps zeroes after first digit"

# wrong arrangement with same digits
assert run("310\n130\n") == "WRONG_ANSWER", "digits match but arrangement is not minimal"

# already sorted
assert run("1234\n1234\n") == "OK", "already minimal"

# maximum size boundary
assert run("1000000000\n1000000000\n") == "OK", "10 digits with many zeroes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 / 0`|`OK`| Xử lý trường hợp 0 ​​có một chữ số | 
|`1002 / 1002`|`OK`| Xử lý đúng các số 0 đứng đầu | 
|`310 / 130`|`WRONG_ANSWER`| Các chữ số trùng khớp là không đủ | 
|`1234 / 1234`|`OK`| Đã đặt hàng tối thiểu | 
|`1000000000 / 1000000000`|`OK`| Kích thước ranh giới có nhiều số 0 | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1002
1002
```Các chữ số được sắp xếp trở thành:```
0012
```Sự sắp xếp này không hợp lệ vì nó bắt đầu bằng số 0. Thuật toán quét tìm chữ số khác 0 đầu tiên, đó là`1`, và hoán đổi nó vào phía trước:```
1002
```Các chữ số còn lại vẫn được sắp xếp nên đây là cách sắp xếp hợp lệ nhỏ nhất. 

Bây giờ hãy xem xét:```
0
0
```Danh sách chữ số chỉ chứa một ký tự:```
['0']
```Không cần trao đổi vì không có chữ số khác 0. Kết quả được xây dựng vẫn còn`"0"`, phù hợp với câu trả lời đúng. 

Cuối cùng, hãy xem xét:```
909
990
```Các chữ số được sắp xếp:```
099
```Sau khi hoán đổi chữ số khác 0 đầu tiên về phía trước:```
909
```Bob đã trả lời`990`, cái nào lớn hơn. Thuật toán in chính xác:```
WRONG_ANSWER
```
