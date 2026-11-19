---
title: "CF 21A - ID Jabber"
description: "Chúng ta cần xác thực xem một chuỗi có tuân theo cú pháp chính xác của ID Jabber hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 21
codeforces_index: "A"
codeforces_contest_name: "Codeforces Alpha Round 21 (Codeforces format)"
rating: 1900
weight: 21
solve_time_s: 97
verified: false
draft: false
---
[CF 21A - ID Jabber](https://codeforces.com/problemset/problem/21/A) 

**Xếp hạng:** 1900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xác thực xem một chuỗi có tuân theo cú pháp chính xác của ID Jabber hay không. 

Một ID hợp lệ có ba phần có thể:`username@hostname`hoặc`username@hostname/resource`Tên người dùng và tài nguyên tuân theo các quy tắc tương tự. Chúng chỉ có thể chứa các chữ cái, chữ số và dấu gạch dưới tiếng Anh. Độ dài của chúng phải nằm trong khoảng từ 1 đến 16. 

Tên máy chủ hơi khác một chút. Nó được chia thành các phần cách nhau bằng dấu chấm. Mọi phần phải đáp ứng các quy tắc giống như tên người dùng, nghĩa là mỗi phần chỉ được chứa các chữ cái, chữ số và dấu gạch dưới và có độ dài từ 1 đến 16. Toàn bộ độ dài tên máy chủ phải nằm trong khoảng từ 1 đến 32. 

Đầu vào là một chuỗi đơn và chúng ta phải in`YES`nếu mọi quy tắc đều được thỏa mãn, ngược lại`NO`. 

Độ dài đầu vào tối đa là 100, vì vậy hiệu suất không phải là vấn đề đáng lo ngại. Ngay cả phân tích cú pháp bậc hai cũng sẽ dễ dàng phù hợp. Thách thức là tính đúng đắn. Hầu hết các lần gửi sai đều không thành công vì chúng phân tích các dấu phân cách không chính xác hoặc quên một trong các điều kiện về độ dài. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Tên máy chủ có thể chứa nhiều từ được phân tách bằng dấu chấm:```
a@b.c.d
```Điều này hợp lệ vì mỗi phân đoạn không trống và hợp pháp. 

Tên máy chủ có phân đoạn trống là không hợp lệ:```
a@b..c
```Câu trả lời đúng là`NO`vì chuỗi con giữa hai dấu chấm trống. Việc triển khai dựa trên sự phân chia bất cẩn đôi khi quên từ chối các phần trống. 

Phần tài nguyên là tùy chọn, nhưng nếu`/`tồn tại thì tài nguyên không thể trống:```
a@b/
```Câu trả lời là`NO`. 

Điều tương tự cũng áp dụng cho tên người dùng:```
@abc
```Điều này không hợp lệ vì độ dài tên người dùng phải ít nhất là 1. 

Một lỗi phân tích cú pháp phổ biến là cho phép nhiều`@`biểu tượng:```
a@b@c
```Điều này phải bị từ chối vì cấu trúc cho phép chính xác một tên người dùng và một tên máy chủ. 

Một trường hợp tế nhị khác là độ dài tên máy chủ. Ngay cả khi mỗi phân đoạn đều hợp lệ riêng lẻ thì toàn bộ tên máy chủ vẫn phải có tối đa 32 ký tự.```
a@abcdefghij.abcdefghij.abcdefghijx
```Mỗi đoạn có độ dài phù hợp, nhưng tổng tên máy chủ vượt quá 32, vì vậy câu trả lời là`NO`. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp nhất là thử mọi vị trí phân chia có thể cho`@`Và`/`, sau đó kiểm tra xem chuỗi con thu được có đáp ứng tất cả các quy tắc hay không. 

Ví dụ: chúng ta có thể lặp lại mọi thứ có thể`@`vị trí, sau đó mọi thứ có thể`/`đặt sau nó và xác minh tất cả các phần theo cách thủ công. Vì độ dài chuỗi tối đa là 100 nên chuỗi này vẫn chạy ngay lập tức. Trường hợp xấu nhất là khoảng vài nghìn lần kiểm tra chuỗi con. 

Điểm yếu của cách tiếp cận này không phải là tốc độ mà là sự phức tạp trong việc thực hiện. Khi có nhiều dấu phân cách, việc phân tích cú pháp brute-force sẽ dễ bị lỗi. Rất dễ vô tình chấp nhận các cấu trúc không đúng định dạng như nhiều`@`ký tự hoặc phân đoạn tên máy chủ trống. 

Cách tiếp cận rõ ràng hơn là phân tích chuỗi theo ngữ pháp của nó. 

Cấu trúc cứng nhắc: 

1. Phải có chính xác một`@`. 
2. Mọi chuyện trước đây`@`là tên người dùng. 
3. Mọi chuyện sau đó`@`là: 

- chỉ là tên máy chủ 
- hoặc tên máy chủ/tài nguyên 

Khi chuỗi được tách thành các thành phần logic này, việc xác thực sẽ trở thành cục bộ. Mỗi phần có thể được kiểm tra độc lập. 

Quan sát quan trọng là tất cả các từ nguyên tử đều có chung quy tắc xác thực: chỉ các chữ cái, chữ số và dấu gạch dưới, có độ dài từ 1 đến 16. Điều đó có nghĩa là chúng ta có thể viết một hàm xác thực có thể tái sử dụng và áp dụng nó ở mọi nơi. 

Đối với tên máy chủ, chúng tôi cũng phân chia theo dấu chấm và xác thực từng phân đoạn riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Kiểm tra xem chuỗi có chứa chính xác một`@`. 

Nếu không có hạn chế này, các đầu vào không đúng định dạng như`a@b@c`có thể được phân tích một cách mơ hồ. 
3. Chia chuỗi thành`username`Và`rest`sử dụng`@`. 
4. Xác thực tên người dùng. 

Tên người dùng phải: 

- có độ dài từ 1 đến 16 
- chỉ chứa các chữ cái, chữ số hoặc dấu gạch dưới 
5. Kiểm tra xem`rest`chứa một`/`. 

Nếu được thì chia thành`hostname`Và`resource`. 

Nếu không thì toàn bộ`rest`là tên máy chủ và không có tài nguyên. 
6. Nếu tài nguyên tồn tại, hãy xác thực tài nguyên đó bằng cách sử dụng các quy tắc tương tự như tên người dùng. 

Tài nguyên trống không hợp lệ. 
7. Xác thực độ dài tên máy chủ. 

Toàn bộ tên máy chủ phải có độ dài từ 1 đến 32. 
8. Chia tên máy chủ theo dấu chấm. 
9. Xác thực mọi phân đoạn tên máy chủ. 

Mỗi phân khúc phải: 

- không trống 
- có độ dài từ 1 đến 16 
- chỉ chứa các chữ cái, chữ số hoặc dấu gạch dưới 
10. Nếu mọi lần kiểm tra đều thành công, hãy in`YES`. Nếu không thì in`NO`. 

### Tại sao nó hoạt động 

Thuật toán phản ánh cấu trúc chính xác của ID Jabber hợp lệ. 

các`@`tách đảm bảo rằng có chính xác một phần tên người dùng và một phần tên máy chủ. tùy chọn`/`tách cô lập tài nguyên nếu nó tồn tại. Mọi thành phần nguyên tử còn lại đều được kiểm tra theo quy tắc ký tự và độ dài. 

Việc xác thực tên máy chủ là chính xác vì việc chia theo dấu chấm sẽ tạo ra chính xác các từ tên máy chủ được mô tả trong thông số kỹ thuật. Việc từ chối các phân đoạn trống sẽ ngăn các biểu mẫu không hợp lệ như dấu chấm liên tiếp hoặc dấu chấm đầu/cuối. 

Vì mọi quy tắc trong đặc tả đều được kiểm tra trực tiếp và độc lập nên thuật toán chấp nhận tất cả các ID hợp lệ và từ chối tất cả các ID không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def valid_word(s):
    if not (1 <= len(s) <= 16):
        return False

    for ch in s:
        if not (ch.isalnum() or ch == '_'):
            return False

    return True

def solve():
    s = input().strip()

    if s.count('@') != 1:
        print("NO")
        return

    username, rest = s.split('@')

    if not valid_word(username):
        print("NO")
        return

    if rest.count('/') > 1:
        print("NO")
        return

    if '/' in rest:
        hostname, resource = rest.split('/')

        if not valid_word(resource):
            print("NO")
            return
    else:
        hostname = rest

    if not (1 <= len(hostname) <= 32):
        print("NO")
        return

    parts = hostname.split('.')

    for part in parts:
        if not valid_word(part):
            print("NO")
            return

    print("YES")

solve()
```Giải pháp xoay quanh một người trợ giúp có thể tái sử dụng,`valid_word`. Hàm này triển khai logic xác thực được chia sẻ được sử dụng bởi các phân đoạn tên người dùng, tài nguyên và tên máy chủ. 

Việc kiểm tra kết cấu đầu tiên đang được tính`@`. sử dụng`count('@') != 1`ngay lập tức từ chối các đầu vào không đúng định dạng trước khi thực hiện bất kỳ phân tích cú pháp sâu hơn nào. 

Việc phân tích tài nguyên xứng đáng được xử lý cẩn thận. Đầu tiên chúng tôi kiểm tra xem có nhiều hơn một`/`tồn tại bên trong phần tài nguyên tên máy chủ. Đầu vào như:```
a@b/c/d
```phải bị từ chối. 

Xác thực tên máy chủ diễn ra trong hai lớp. Trước tiên, chúng tôi xác thực tổng chiều dài tên máy chủ vì câu lệnh áp đặt giới hạn riêng là 32 ký tự. Sau đó, chúng tôi chia theo dấu chấm và xác thực từng phân đoạn riêng lẻ. 

Hành vi phân tách sẽ bắt các thành phần trống một cách tự nhiên. Ví dụ:```
"ab..cd".split('.')
```sản xuất:```
["ab", "", "cd"]
```Chuỗi trống không thành công`valid_word`, từ chối chính xác tên máy chủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
[email protected]
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | s |`codeforces@tests`| 
| Chia cho @ | tên người dùng |`codeforces`| 
| Chia cho @ | nghỉ ngơi |`tests`| 
| Xác thực tên người dùng | kết quả | hợp lệ | 
| Kiểm tra / | tìm thấy | không | 
| Tên máy chủ | tên máy chủ |`tests`| 
| Chia tên máy chủ | bộ phận |`["tests"]`| 
| Xác thực các bộ phận | kết quả | hợp lệ | 
| Câu trả lời cuối cùng | đầu ra |`YES`| 

Ví dụ này cho thấy cấu trúc hợp lệ đơn giản nhất mà không có phần tài nguyên. Mọi thành phần đều đáp ứng các quy tắc về ký tự và độ dài cho phép. 

### Ví dụ 2 

đầu vào:```
a@b..c
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | s |`a@b..c`| 
| Chia cho @ | tên người dùng |`a`| 
| Chia cho @ | nghỉ ngơi |`b..c`| 
| Xác thực tên người dùng | kết quả | hợp lệ | 
| Tên máy chủ | tên máy chủ |`b..c`| 
| Chia tên máy chủ | bộ phận |`["b", "", "c"]`| 
| Xác thực phần trống | kết quả | không hợp lệ | 
| Câu trả lời cuối cùng | đầu ra |`NO`| 

Dấu vết này chứng tỏ tại sao các phân đoạn tên máy chủ trống phải bị từ chối. Các dấu chấm liên tiếp tạo ra một chuỗi con trống giữa chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một số lần không đổi | 
| Không gian | O(n) | Tách chuỗi tạo mảng chuỗi con | 

Độ dài đầu vào tối đa là 100, vì vậy giải pháp dễ dàng nằm gọn trong giới hạn. Ngay cả việc triển khai không hiệu quả cũng có thể diễn ra một cách thoải mái, nhưng trình phân tích cú pháp tuyến tính sạch hơn và dễ lý giải hơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    s = input().strip()

    def valid_word(x):
        if not (1 <= len(x) <= 16):
            return False

        for ch in x:
            if not (ch.isalnum() or ch == '_'):
                return False

        return True

    if s.count('@') != 1:
        print("NO")
        return

    username, rest = s.split('@')

    if not valid_word(username):
        print("NO")
        return

    if rest.count('/') > 1:
        print("NO")
        return

    if '/' in rest:
        hostname, resource = rest.split('/')

        if not valid_word(resource):
            print("NO")
            return
    else:
        hostname = rest

    if not (1 <= len(hostname) <= 32):
        print("NO")
        return

    for part in hostname.split('.'):
        if not valid_word(part):
            print("NO")
            return

    print("YES")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("[email protected]\n") == "YES", "sample 1"

# custom cases
assert run("a@b\n") == "YES", "minimum valid case"

assert run("@b\n") == "NO", "empty username"

assert run("a@b..c\n") == "NO", "empty hostname segment"

assert run("abcdefghijklmnopq@abc\n") == "NO", "username exceeds 16 chars"

assert run("a@abc/\n") == "NO", "empty resource"

assert run("a@b/c/d\n") == "NO", "multiple slashes"

assert run("a_b@c_d.e_f/resource_1\n") == "YES", "underscores allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a@b`|`YES`| Cấu trúc hợp lệ nhỏ nhất | 
|`@b`|`NO`| Tên người dùng không được để trống | 
|`a@b..c`|`NO`| Phân đoạn tên máy chủ trống không hợp lệ | 
|`abcdefghijklmnopq@abc`|`NO`| Độ dài tên người dùng giới hạn trên | 
|`a@abc/`|`NO`| Tài nguyên không được để trống | 
|`a@b/c/d`|`NO`| Chỉ cho phép một dấu gạch chéo tùy chọn | 
|`a_b@c_d.e_f/resource_1`|`YES`| Dấu gạch dưới là hợp pháp ở mọi nơi | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
a@b..c
```Thuật toán chia tên máy chủ thành:```
["b", "", "c"]
```Đoạn giữa trống không thành công`valid_word`vì độ dài của nó bằng 0. Thuật toán in chính xác`NO`. 

Bây giờ hãy xem xét:```
a@b/
```Trình phân tích cú pháp phát hiện`/`và chia thành:```
hostname = "b"
resource = ""
```Tài nguyên trống không xác thực được ngay lập tức, vì vậy câu trả lời là`NO`. 

Đối với nhiều`@`biểu tượng:```
a@b@c
```Lần kiểm tra đầu tiên đếm số lượng`@`nhân vật. Vì số đếm không chính xác là 1 nên chuỗi bị từ chối trước khi phân tích cú pháp tiếp theo. 

Đối với tên máy chủ vượt quá 32 ký tự:```
a@abcdefghij.abcdefghij.abcdefghijx
```Tổng độ dài tên máy chủ trở thành 33. Mặc dù mỗi phân đoạn riêng lẻ đều hợp lệ, điều kiện độ dài tên máy chủ riêng biệt không thành công, do đó thuật toán xuất ra`NO`. 

Cuối cùng, hãy xem xét một ví dụ phức tạp hợp lệ:```
abc_DEF@host_1.server_2/resource_3
```Tên người dùng, mọi phân đoạn tên máy chủ và tài nguyên đều đáp ứng quy tắc xác thực được chia sẻ. Độ dài tên máy chủ nằm trong khoảng 32. Thuật toán đạt đến điểm cuối mà không kích hoạt bất kỳ sự từ chối nào và in ra`YES`.
