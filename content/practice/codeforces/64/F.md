---
title: "CF 64F - Tên miền"
description: "Chúng ta cần quyết định xem một chuỗi nhất định có thể được hiểu là tên miền hợp lệ theo một bộ quy tắc đơn giản hay không. Chuỗi chỉ có thể chứa các chữ cái, chữ số và dấu chấm tiếng Anh viết thường. Dấu chấm tách chuỗi thành các đoạn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "expression-parsing"]
categories: ["algorithms"]
codeforces_contest: 64
codeforces_index: "F"
codeforces_contest_name: "Unknown Language Round 1"
rating: 2000
weight: 64
solve_time_s: 97
verified: true
draft: false
---

[CF 64F - Miền](https://codeforces.com/problemset/problem/64/F) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt, phân tích biểu thức 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần quyết định xem một chuỗi nhất định có thể được hiểu là tên miền hợp lệ theo một bộ quy tắc đơn giản hay không. 

Chuỗi chỉ có thể chứa các chữ cái, chữ số và dấu chấm tiếng Anh viết thường. Dấu chấm tách chuỗi thành các đoạn. Chuỗi không thể bắt đầu bằng dấu chấm hoặc kết thúc bằng dấu chấm và hai dấu chấm không thể xuất hiện liên tiếp. Sau khi chia bằng dấu chấm, đoạn cuối cùng phải có độ dài chính xác là 2 hoặc 3. 

Độ dài đầu vào tối đa là 100, rất nhỏ. Ngay cả một giải pháp không hiệu quả cũng có thể hoạt động thoải mái trong thời gian giới hạn. Thách thức thực sự không phải là tối ưu hóa mà là thực hiện cẩn thận mọi quy tắc mà không bỏ sót các trường hợp góc cạnh. 

Một lỗi phổ biến là chỉ kiểm tra các ký tự được phép trong khi quên các ràng buộc về cấu trúc liên quan đến dấu chấm. Ví dụ: 

đầu vào:```
abc..com
```Đầu ra đúng:```
NO
```Chuỗi chỉ sử dụng các ký tự hợp lệ nhưng các dấu chấm liên tiếp sẽ tạo ra một đoạn trống, điều này bị cấm. 

Một lỗi dễ mắc khác là quên rằng tên miền không thể bắt đầu hoặc kết thúc bằng dấu chấm. 

đầu vào:```
.google
```Đầu ra đúng:```
NO
```Một giải pháp dựa trên sự phân chia bất cẩn có thể tạo ra`["", "google"]`và vô tình chấp nhận nó. 

Việc hạn chế độ dài đoạn cuối cùng cũng gây ra những lỗi khó phát hiện. 

đầu vào:```
abc.c
```Đầu ra đúng:```
NO
```Đoạn cuối cùng có độ dài 1, trong khi chỉ cho phép độ dài 2 và 3. 

Tương tự: 

đầu vào:```
abc.comm
```Đầu ra đúng:```
NO
```Độ dài đoạn cuối cùng là 4. 

Một cái bẫy khác là chữ in hoa. Câu lệnh chỉ cho phép các chữ cái viết thường. 

đầu vào:```
Codeforces.com
```Đầu ra đúng:```
NO
```Một giải pháp sử dụng`isalnum()`trực tiếp sẽ chấp nhận không chính xác các ký tự viết hoa. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là mô phỏng chính xác định nghĩa. Chúng tôi quét từng ký tự chuỗi và xác minh mọi quy tắc một cách độc lập. 

Trước tiên chúng ta có thể kiểm tra xem mọi ký tự có thuộc tập hợp được phép hay không. Sau đó, chúng tôi xác minh rằng chuỗi không bắt đầu cũng không kết thúc bằng dấu chấm. Tiếp theo chúng ta kiểm tra xem không có cặp ký tự liền kề nào chứa hai dấu chấm. Cuối cùng, chúng tôi chia chuỗi theo dấu chấm và kiểm tra độ dài đoạn cuối cùng. 

Điều này đã chạy theo thời gian tuyến tính vì mỗi lần chạm vào chuỗi một lần. Với độ dài tối đa là 100, thậm chí việc quét chuỗi nhiều lần cũng không đáng kể. 

Một cách giải thích bạo lực khác có thể xảy ra là tạo ra tất cả các cách có thể để chia chuỗi thành các phân đoạn tên miền và kiểm tra tính hợp lệ theo cách thủ công. Cách tiếp cận đó trở nên phức tạp không cần thiết và tăng theo cấp số nhân với số lượng vị trí. Mặc dù các ràng buộc đủ nhỏ để nó vẫn có thể vượt qua, nhưng cấu trúc bài toán đưa ra một lộ trình rõ ràng hơn nhiều. 

Quan sát quan trọng là mọi quy tắc đều mang tính cục bộ. Các ký tự được phép phụ thuộc vào vị trí riêng lẻ, tính hợp lệ của dấu chấm liên tiếp chỉ phụ thuộc vào các ký tự lân cận và điều kiện hậu tố chỉ phụ thuộc vào phân đoạn cuối cùng. Không cần tìm kiếm hoặc phân tích tổng thể. Một lần quét từ trái sang phải kết hợp với một lần kiểm tra hậu tố cuối cùng sẽ xác định đầy đủ tính hợp lệ. 

Do đó, giải pháp tối ưu chỉ đơn giản là một trình xác nhận trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tách/tìm kiếm đầy đủ | O(2^n) | O(n) | Chậm một cách không cần thiết | 
| Quét xác thực trực tiếp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Kiểm tra xem chuỗi bắt đầu hay kết thúc bằng dấu chấm. 

Tên miền hợp lệ không thể có phân đoạn trống ở đầu hoặc cuối. 
3. Quét từng ký tự trong chuỗi. 

Nếu ký tự không phải là chữ thường, chữ số hoặc dấu chấm, hãy từ chối chuỗi ngay lập tức. 
4. Trong cùng một lần quét, hãy kiểm tra các ký tự liền kề. 

Nếu hai ký tự liên tiếp đều là dấu chấm, hãy loại bỏ chuỗi vì điều này tạo ra một đoạn trống. 
5. Tách chuỗi bằng dấu chấm làm dấu phân cách. 
6. Lấy đoạn cuối cùng và kiểm tra độ dài của nó. 

Độ dài phải là 2 hoặc 3. 
7. Nếu mọi kiểm tra đều đạt, hãy in`"YES"`. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán mã hóa trực tiếp mọi điều kiện từ định nghĩa của miền hợp lệ. Việc quét ký tự đảm bảo rằng không có biểu tượng bị cấm nào xuất hiện. Việc kiểm tra ranh giới ngăn chặn các phân đoạn trống ở đầu hoặc cuối. Việc kiểm tra dấu chấm liên tiếp sẽ ngăn chặn các đoạn trống ở giữa. Việc chia theo dấu chấm sẽ hiển thị đoạn cuối cùng để có thể xác minh chính xác giới hạn độ dài của nó. Vì mọi quy tắc đều được kiểm tra rõ ràng và việc từ chối xảy ra ngay lập tức khi một quy tắc không thành công, nên thuật toán chấp nhận chính xác các tên miền hợp lệ và từ chối tất cả các tên miền không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    if s[0] == '.' or s[-1] == '.':
        print("NO")
        return

    for i, ch in enumerate(s):
        valid = (
            ('a' <= ch <= 'z') or
            ('0' <= ch <= '9') or
            ch == '.'
        )

        if not valid:
            print("NO")
            return

        if i > 0 and s[i] == '.' and s[i - 1] == '.':
            print("NO")
            return

    parts = s.split('.')
    last_len = len(parts[-1])

    if last_len == 2 or last_len == 3:
        print("YES")
    else:
        print("NO")

solve()
```Điều kiện đầu tiên xử lý các chấm biên ngay lập tức. Điều này tránh việc vô tình chấp nhận các đoạn đầu hoặc cuối trống sau khi tách. 

Vòng lặp chính kết hợp hai lần kiểm tra thành một lần. Đầu tiên, nó xác thực các ký tự bằng cách sử dụng so sánh phạm vi rõ ràng thay vì các trợ giúp như`isalnum()`. Điều đó quan trọng bởi vì`isalnum()`sẽ cho phép chữ hoa không chính xác. 

Việc kiểm tra kề chỉ bắt đầu khi`i > 0`, ngăn chặn truy cập ngoài giới hạn đối với ký tự đầu tiên. 

After validation, the string is split by dots. Vì chúng tôi đã từ chối các dấu chấm liên tiếp và dấu chấm ranh giới nên mọi phân đoạn đều được đảm bảo không trống. Điều kiện duy nhất còn lại là kích thước của đoạn cuối cùng. 

Việc triển khai không bao giờ sử dụng các cấu trúc dữ liệu bổ sung ngoài kết quả phân tách và mọi thao tác đều tuyến tính theo độ dài chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
codeforces.com
```| Bước | Trạng thái hiện tại | Kết quả | 
| --- | --- | --- | 
| Boundary check | first=`c`, cuối cùng=`m`| hợp lệ | 
| Quét ký tự | tất cả các ký tự đều là chữ thường/dấu chấm | hợp lệ | 
| Dấu chấm liên tiếp | không tìm thấy | hợp lệ | 
| Chia |`["codeforces", "com"]`| hợp lệ | 
| Độ dài đoạn cuối |`len("com") = 3`| hợp lệ | 

Đầu ra:```
YES
```Ví dụ này thể hiện đường dẫn thành công tiêu chuẩn. Mọi điều kiện cấu trúc đều được giữ nguyên và độ dài miền cấp cao nhất có thể chấp nhận được. 

### Ví dụ 2 

đầu vào:```
abc..com
```| Bước | Trạng thái hiện tại | Kết quả | 
| --- | --- | --- | 
| Kiểm tra ranh giới | đầu tiên=`a`, cuối cùng=`m`| hợp lệ | 
| Quét ký tự | tất cả các ký tự được phép | hợp lệ | 
| Dấu chấm liên tiếp | thành lập`".."`tại chỉ số 3 và 4 | không hợp lệ | 

Đầu ra:```
NO
```Dấu vết này cho thấy lý do tại sao chỉ kiểm tra các ký tự được phép là không đủ. Cấu trúc được tạo bởi dấu chấm cũng có vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuỗi được quét một số lần không đổi | 
| Không gian | O(n) | Hoạt động phân chia lưu trữ các phân đoạn | 

Với`n ≤ 100`, thời gian chạy rất nhỏ. Ngay cả một giải pháp chậm hơn nhiều cũng có thể phù hợp thoải mái trong giới hạn, nhưng trình xác thực tuyến tính vừa đơn giản vừa gọn gàng hơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        s = input().strip()

        if s[0] == '.' or s[-1] == '.':
            return "NO"

        for i, ch in enumerate(s):
            valid = (
                ('a' <= ch <= 'z') or
                ('0' <= ch <= '9') or
                ch == '.'
            )

            if not valid:
                return "NO"

            if i > 0 and s[i] == '.' and s[i - 1] == '.':
                return "NO"

        parts = s.split('.')
        last_len = len(parts[-1])

        if last_len == 2 or last_len == 3:
            return "YES"

        return "NO"

    return solve()

# provided sample
assert run("codeforces.com\n") == "YES", "sample 1"

# custom cases
assert run("a.a\n") == "NO", "last segment too short"

assert run(".abc.com\n") == "NO", "leading dot"

assert run("abc..com\n") == "NO", "consecutive dots"

assert run("abc.comm\n") == "NO", "last segment too long"

assert run("a" * 97 + ".ru\n") == "YES", "maximum length style case"

assert run("Codeforces.com\n") == "NO", "uppercase letters forbidden"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a.a`|`NO`| Độ dài đoạn cuối 1 | 
|`.abc.com`|`NO`| Từ chối dấu chấm hàng đầu | 
|`abc..com`|`NO`| Từ chối dấu chấm liên tiếp | 
|`abc.comm`|`NO`| Độ dài đoạn cuối lớn hơn 3 | 
|`aaaa...aaa.ru`|`YES`| Đầu vào hợp lệ lớn gần độ dài tối đa | 
|`Codeforces.com`|`NO`| Chữ in hoa không hợp lệ | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
.google
```Thuật toán kiểm tra ngay ký tự đầu tiên và thấy đó là dấu chấm. Nó từ chối chuỗi trước khi xảy ra bất kỳ sự phân tách nào. Điều này ngăn chặn việc vô tình chấp nhận phân đoạn đầu tiên trống. 

Bây giờ hãy xem xét:```
abc..com
```Trong quá trình quét, khi thuật toán đạt đến dấu chấm thứ hai, nó sẽ so sánh nó với ký tự trước đó và phát hiện`".."`. Chuỗi bị từ chối ngay lập tức. Không xử lý sau này có thể ghi đè lỗi này. 

Đối với đầu vào:```
abc.c
```Tất cả các kiểm tra ký tự và cấu trúc dấu chấm đều thành công. Sau khi tách, đoạn cuối cùng là`"c"`, có độ dài là 1. Vì chỉ chấp nhận độ dài 2 và 3 nên thuật toán sẽ in`"NO"`. 

Cuối cùng, hãy xem xét:```
Codeforces.com
```Khi quét ký tự đầu tiên`'C'`, việc kiểm tra phạm vi chữ thường không thành công vì`'C'`ở bên ngoài`'a'`ĐẾN`'z'`. Thuật toán từ chối chuỗi ngay lập tức, thực thi chính xác quy tắc chỉ viết thường.
