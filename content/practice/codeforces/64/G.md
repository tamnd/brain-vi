---
title: "CF 64G - Đường dẫn Canon hóa"
description: "Chúng tôi được cung cấp một đường dẫn kiểu Unix tuyệt đối. Đường dẫn được chia thành các thành phần bởi / và mỗi thành phần đại diện cho một thư mục hoặc tên tệp thông thường, \".\", hoặc \"..\". Tên bình thường có nghĩa là “đi vào thư mục hoặc tập tin này”. Thành phần \"."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 64
codeforces_index: "G"
codeforces_contest_name: "Unknown Language Round 1"
rating: 2200
weight: 64
solve_time_s: 102
verified: true
draft: false
---

[CF 64G - Chuẩn hóa đường dẫn](https://codeforces.com/problemset/problem/64/G) 

**Đánh giá:** 2200 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một đường dẫn kiểu Unix tuyệt đối. Đường dẫn được chia thành các thành phần bởi`/`và mọi thành phần đại diện cho một thư mục hoặc tên tệp thông thường,`"."`, hoặc`".."`. 

Tên bình thường có nghĩa là “đi vào thư mục hoặc tập tin này”. 

thành phần`"."`có nghĩa là “ở trong thư mục hiện tại”, vì vậy nó không thay đổi gì cả. 

thành phần`".."`có nghĩa là "di chuyển đến thư mục mẹ". 

Nhiệm vụ là đơn giản hóa đường dẫn sao cho kết quả cuối cùng không chứa`"."`hoặc`".."`thành phần. Nếu tại một thời điểm nào đó đường dẫn cố gắng di chuyển lên trên thư mục gốc thì đường dẫn đó không hợp lệ và chúng ta phải in`-1`. 

Độ dài đường dẫn tối đa là 1000 ký tự, rất nhỏ. Ngay cả lời giải bậc hai cũng có thể dễ dàng vượt qua, nhưng cấu trúc của bài toán đương nhiên dẫn đến lời giải theo thời gian tuyến tính. Vì mọi thành phần đường dẫn được xử lý độc lập và chỉ ảnh hưởng đến ngăn xếp thư mục hiện tại nên chúng ta có thể giải quyết vấn đề theo một lượt từ trái sang phải. 

Các trường hợp nguy hiểm không liên quan đến hiệu suất mà liên quan đến việc diễn giải chính xác các thành phần đặc biệt. 

Hãy xem xét đầu vào:```
/../../a
```đầu tiên`".."`đã cố gắng rời khỏi thư mục gốc. Đầu ra đúng là:```
-1
```Việc triển khai bất cẩn có thể âm thầm bỏ qua các phần bổ sung`".."`hoạt động và trả lại không chính xác`/a`. 

Một trường hợp tế nhị khác là phân biệt`"."`Và`".."`từ những tên thông thường có chứa dấu chấm. 

Ví dụ:```
/.../..hidden/./x
```Các thành phần`"..."`Và`"..hidden"`là tên thông thường, không phải lệnh đặc biệt. Đường dẫn đơn giản hóa chính xác là:```
/.../..hidden/x
```Việc triển khai có lỗi chỉ kiểm tra xem một thành phần có chứa các dấu chấm hay không sẽ xử lý chúng một cách không chính xác như các thao tác thư mục mẹ. 

Bản thân thư mục gốc cũng cần được xử lý cẩn thận.```
/
```Kết quả phải chính xác`/`. Việc xây dựng lại câu trả lời bằng cách nối các thành phần một cách mù quáng có thể vô tình tạo ra một chuỗi trống. 

## Phương pháp tiếp cận 

Một cách tiếp cận brute-force sẽ liên tục tìm kiếm đường dẫn cho sự xuất hiện của`"."`Và`".."`, viết lại chuỗi và tiếp tục cho đến khi không còn thành phần đặc biệt nào. 

Ví dụ, nếu chúng ta thấy`/a/b/../c`, chúng ta có thể viết lại nó thành`/a/c`. Nếu chúng ta thấy`/./`, chúng tôi loại bỏ nó. Phương pháp này đơn giản về mặt khái niệm vì nó mô phỏng trực tiếp việc đơn giản hóa đường dẫn trên chính chuỗi đó. 

Vấn đề là mỗi lần viết lại đều dịch chuyển các ký tự trong chuỗi. Nếu đường dẫn chứa nhiều thư mục lồng nhau và nhiều`".."`hoạt động, mỗi lần viết lại có thể tốn thời gian tuyến tính và chúng tôi có thể thực hiện nhiều lần viết lại tuyến tính. Với độ dài đường đi là`n`, điều này có thể phân hủy thành`O(n²)`hoạt động. 

Quan sát chính là việc chuẩn hóa đường dẫn chỉ phụ thuộc vào trình tự các thành phần thư mục hiện đang hoạt động. Khi chúng tôi nhập một thư mục, chúng tôi sẽ thêm nó vào đường dẫn hiện tại. Khi chúng ta gặp phải`".."`, chúng tôi xóa thư mục gần đây nhất. Hành vi này chính xác là một ngăn xếp. 

Điều đó thay đổi vấn đề từ “viết lại chuỗi liên tục” thành “xử lý các thành phần một lần từ trái sang phải”. 

Đối với mỗi thành phần: 

Nếu nó là`"."`, chúng tôi bỏ qua nó. 

Nếu nó là`".."`, chúng tôi lấy ra một thư mục từ ngăn xếp. Nếu ngăn xếp đã trống thì đường dẫn không hợp lệ. 

Ngược lại, thành phần này là một tên thông thường nên chúng ta đẩy nó vào ngăn xếp. 

Cuối cùng, ngăn xếp đã chứa các thành phần đường dẫn chuẩn theo thứ tự. Chúng tôi chỉ đơn giản là tham gia cùng họ với`/`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Được chấp nhận nhưng không hiệu quả | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đường dẫn đầu vào dưới dạng chuỗi. 
2. Chia đường dẫn cho`/`. 

Vì đường dẫn luôn bắt đầu bằng`/`, thành phần phân tách đầu tiên sẽ là một chuỗi trống. Chúng tôi chỉ đơn giản bỏ qua các thành phần trống trong quá trình xử lý. 
3. Tạo một ngăn xếp trống. 

Ngăn xếp biểu thị đường dẫn chuẩn hiện tại từ thư mục gốc đến thư mục hiện tại. 
4. Xử lý từng thành phần từ trái qua phải. 

Điều này phù hợp với hành vi thực sự của việc điều hướng qua hệ thống tập tin. 
5. Nếu thành phần này trống hoặc`"."`, bỏ qua nó. 

Các thành phần trống xuất hiện vì đường dẫn bắt đầu bằng`/`. thành phần`"."`có nghĩa là “ở lại đây”, nên nó không thay đổi gì cả. 
6. Nếu thành phần đó là`".."`, hãy thử xóa thư mục cuối cùng khỏi ngăn xếp. 

Nếu ngăn xếp trống thì chúng ta đã ở thư mục gốc nên việc di chuyển lên trên là không thể. In`-1`và dừng lại. 
7. Mặt khác, thành phần này là tên thư mục hoặc tên tệp thông thường. 

Đẩy nó vào ngăn xếp. 
8. Sau khi xử lý tất cả các thành phần, hãy xây dựng lại đường dẫn chuẩn. 

Nếu ngăn xếp trống, đường dẫn chuẩn chỉ đơn giản là`/`. 

Nếu không, hãy nối tất cả các phần tử ngăn xếp với`/`và thêm một dấu gạch chéo ở đầu. 

### Tại sao nó hoạt động 

Sau khi xử lý lần đầu`k`các thành phần, ngăn xếp luôn biểu diễn đường dẫn chuẩn chính xác tương ứng với các thành phần đó`k`thành phần. 

Đẩy một tên bình thường sẽ khớp với việc nhập một thư mục. Bật lên`".."`khớp với việc quay trở lại thư mục mẹ. Bỏ qua`"."`các trận đấu nằm trong cùng một thư mục. 

Bởi vì mọi thao tác đều cập nhật ngăn xếp chính xác như ngữ nghĩa của hệ thống tập tin yêu cầu, trạng thái ngăn xếp cuối cùng là phiên bản chuẩn duy nhất của đường dẫn. Tình huống không hợp lệ duy nhất là cố gắng bật từ một ngăn xếp trống, tương ứng chính xác với việc di chuyển lên trên thư mục gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    path = input().strip()

    stack = []

    for part in path.split('/'):
        if part == '' or part == '.':
            continue

        if part == '..':
            if not stack:
                print(-1)
                return
            stack.pop()
        else:
            stack.append(part)

    if not stack:
        print('/')
    else:
        print('/' + '/'.join(stack))

solve()
```Giải pháp tuân theo mô phỏng dựa trên ngăn xếp trực tiếp. 

Cuộc gọi đến`split('/')`chuyển đổi đường dẫn thành các thành phần riêng lẻ. Bởi vì đường dẫn đầu vào luôn bắt đầu bằng`/`, thành phần đầu tiên sẽ trở thành một chuỗi rỗng. Chúng tôi bỏ qua các thành phần như vậy. 

Ngăn xếp lưu trữ các thư mục hiện đang hoạt động. Mọi tên thông thường đều được thêm vào. Mọi`".."`xóa thư mục gần đây nhất. 

Chi tiết triển khai quan trọng nhất là kiểm tra xem ngăn xếp có trống hay không trước khi gọi`pop()`. Việc quên kiểm tra này sẽ gây ra sự cố hoặc cho phép di chuyển không chính xác phía trên thư mục gốc. 

Một điểm tinh tế khác là xây dựng lại câu trả lời cuối cùng. Nếu ngăn xếp trống, đường dẫn chuẩn chính xác là`/`, không phải là một chuỗi rỗng. Trường hợp đặc biệt đó phải được xử lý riêng. 

Điều kiện:```
if part == '' or part == '.':
```cũng quan trọng. Chỉ khớp chính xác với`"."`Và`".."`là đặc biệt. Các chuỗi như`"..."`hoặc`"..a"`phải vẫn là tên đường dẫn thông thường. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
/usr/share/mysql/../tomcat6/conf/server.xml
```| Thành phần | Hành động | Ngăn xếp | 
| --- | --- | --- | 
|`usr`| đẩy |`["usr"]`| 
|`share`| đẩy |`["usr", "share"]`| 
|`mysql`| đẩy |`["usr", "share", "mysql"]`| 
|`..`| bật |`["usr", "share"]`| 
|`tomcat6`| đẩy |`["usr", "share", "tomcat6"]`| 
|`conf`| đẩy |`["usr", "share", "tomcat6", "conf"]`| 
|`server.xml`| đẩy |`["usr", "share", "tomcat6", "conf", "server.xml"]`| 

Đầu ra cuối cùng:```
/usr/share/tomcat6/conf/server.xml
```Dấu vết này thể hiện tính bất biến cốt lõi: ngăn xếp luôn lưu trữ đường dẫn chuẩn sau khi xử lý tiền tố hiện tại của các thành phần. 

### Ví dụ 2 

đầu vào:```
/../../a
```| Thành phần | Hành động | Ngăn xếp | 
| --- | --- | --- | 
|`..`| cửa sổ bật lên không hợp lệ |`[]`| 

Đầu ra cuối cùng:```
-1
```Ví dụ này thực hiện điều kiện lỗi duy nhất trong bài toán. Thuật toán ngay lập tức phát hiện nỗ lực di chuyển lên trên thư mục gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thành phần đường dẫn được xử lý một lần | 
| Không gian | O(n) | Ngăn xếp có thể lưu trữ tất cả các thành phần | 

Đây,`n`là độ dài của chuỗi đầu vào. Vì độ dài đường dẫn tối đa chỉ là 1000 nên giải pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    path = input().strip()

    stack = []

    for part in path.split('/'):
        if part == '' or part == '.':
            continue

        if part == '..':
            if not stack:
                print(-1)
                return
            stack.pop()
        else:
            stack.append(part)

    if not stack:
        print('/')
    else:
        print('/' + '/'.join(stack))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("/usr/share/mysql/../tomcat6/conf/server.xml\n") == \
       "/usr/share/tomcat6/conf/server.xml", "sample 1"

# minimum-size input
assert run("/\n") == "/", "root directory"

# invalid upward movement
assert run("/../../a\n") == "-1", "cannot move above root"

# only current-directory operations
assert run("/./././x\n") == "/x", "ignore current directory"

# names containing dots are ordinary names
assert run("/.../..hidden/./x\n") == "/.../..hidden/x", \
       "dot-containing names"

# returns to root exactly
assert run("/a/b/../..\n") == "/", "back to root"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`/`|`/`| Xử lý đúng root | 
|`/../../a`|`-1`| Phát hiện chuyển động đi lên không hợp lệ | 
|`/./././x`|`/x`| Bỏ qua`"."`thành phần | 
|`/.../..hidden/./x`|`/.../..hidden/x`| Phân biệt tên đặc biệt với tên thông thường | 
|`/a/b/../..`|`/`| Xây dựng lại đúng ngăn xếp trống | 

## Vỏ cạnh 

Hãy xem xét trường hợp di chuyển lên trên không hợp lệ:```
/../../a
```Thuật toán bắt đầu với một ngăn xếp trống. Thành phần đầu tiên là`".."`. Vì ngăn xếp trống nên không có thư mục mẹ nào để quay về. Thuật toán in ngay lập tức`-1`. 

Điều này khớp chính xác với ngữ nghĩa của hệ thống tập tin. Bỏ qua thao tác này sẽ cho phép các đường dẫn bên ngoài thư mục gốc một cách không chính xác. 

Bây giờ hãy xem xét tên có chứa dấu chấm:```
/.../..hidden/./x
```Các thành phần là:```
"...", "..hidden", ".", "x"
```Thuật toán chỉ xử lý các kết quả khớp chính xác với`"."`Và`".."`càng đặc biệt. Vì thế`"..."`Và`"..hidden"`được đẩy bình thường vào ngăn xếp. các`"."`thành phần bị bỏ qua và`"x"`được thêm vào. 

Ngăn xếp cuối cùng trở thành:```
["...", "..hidden", "x"]
```được tái cấu trúc thành:```
/../..hidden/x
```Cuối cùng, hãy xem xét một đường dẫn thu gọn hoàn toàn về gốc:```
/a/b/../..
```Ngăn xếp phát triển như sau:```
["a"]
["a", "b"]
["a"]
[]
```Cuối cùng, ngăn xếp trống. Thuật toán in`/`thay vì một chuỗi trống, đây là cách thể hiện chuẩn xác của thư mục gốc.
