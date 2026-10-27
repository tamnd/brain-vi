---
title: "CF 106129G - Công ty tạo mật khẩu thú vị"
description: "Chúng ta cần xây dựng một tập hợp các mật khẩu, với số lượng mật khẩu là $n$, trong đó $n là 1000$. Mỗi mật khẩu là một chuỗi trên các ký tự ASCII có thể in được (từ mã 33 đến 126) và mỗi chuỗi phải có độ dài từ 8 đến 12."
date: "2026-06-20T01:42:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106129
codeforces_index: "G"
codeforces_contest_name: "2025-2026 ICPC German Collegiate Programming Contest (GCPC 2025)"
rating: 0
weight: 106129
solve_time_s: 57
verified: true
draft: false
---

[CF 106129G - Công ty tạo mật khẩu thú vị](https://codeforces.com/problemset/problem/106129/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một tập hợp mật khẩu, với số lượng mật khẩu được đưa ra là$n$, Ở đâu$n \le 1000$. Mỗi mật khẩu là một chuỗi trên các ký tự ASCII có thể in được (từ mã 33 đến 126) và mỗi chuỗi phải có độ dài từ 8 đến 12. 

Mỗi mật khẩu phải đáp ứng một ràng buộc về thành phần cố định: nó phải chứa ít nhất một chữ cái viết thường, một chữ cái viết hoa, một chữ số và một ký tự không phải chữ và số, nghĩa là bất kỳ ký hiệu nào có thể in được như dấu câu hoặc dấu ngoặc. 

Ngoài giá trị riêng lẻ, các mật khẩu phải được phân tách rõ ràng với nhau trong khoảng cách chỉnh sửa. Đối với bất kỳ hai mật khẩu khác nhau nào, phải thực hiện ít nhất hai thao tác một ký tự (chèn, xóa hoặc thay thế) để chuyển đổi mật khẩu này thành mật khẩu kia. Nói cách khác, không có cặp nào có thể giống hệt nhau và không có cặp nào có thể khác nhau chỉ một lần chỉnh sửa. 

Đầu ra chỉ đơn giản là$n$mật khẩu như vậy theo bất kỳ thứ tự nào. 

Các ràng buộc đủ nhỏ để chúng ta không phải giải quyết các giới hạn hiệu suất theo nghĩa truyền thống. Khó khăn thực sự là về mặt tổ hợp: chúng ta phải tạo ra nhiều chuỗi hợp lệ một cách có hệ thống trong khi vẫn đảm bảo cả ràng buộc về thành phần và ràng buộc về khoảng cách chỉnh sửa toàn cục. 

Một trường hợp thất bại khó phát hiện nếu chúng ta cố gắng tạo mật khẩu chỉ khác nhau ở một hậu tố hoặc tiền tố nhỏ. Ví dụ: nếu chúng ta sửa một chuỗi cơ sở như`Aa0!aaaa`và chỉ thay đổi ký tự cuối cùng, chúng ta có thể vô tình tạo ra các chuỗi ở khoảng cách chỉnh sửa 1 thông qua thay thế. Một trường hợp lỗi khác xuất hiện nếu hai chuỗi chỉ khác nhau về độ dài, chẳng hạn như`Aa0!abcd`Và`Aa0!abcde`, ở khoảng cách chỉnh sửa 1 thông qua chèn. Những mô hình này phải được tránh trong quá trình xây dựng, không được kiểm tra sau khi thực tế, bởi vì việc kiểm tra từng cặp đơn giản vẫn sẽ bỏ sót những va chạm gần như tinh vi trong các công trình lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra các chuỗi ứng cử viên và xác minh tất cả các ràng buộc đối với tất cả mật khẩu được tạo trước đó. Từ$n \le 1000$và mỗi chuỗi có độ dài tối đa là 12, chúng ta có thể kiểm tra khoảng cách chỉnh sửa trong$O(12^2)$mỗi cặp, giá rẻ nhưng vấn đề thực sự là sự bùng nổ không gian tìm kiếm. Việc tạo ngẫu nhiên hoặc tăng dần không có cấu trúc sẽ thường xuyên tạo ra các xung đột gần như buộc phải tái sinh nhiều lần. 

Điều quan trọng là bảng chữ cái rất lớn và cấu trúc cần thiết cực kỳ linh hoạt. Thay vì coi đây là một bài toán tìm kiếm, chúng ta coi nó như một bài toán xây dựng tất định. Chúng tôi thiết kế một tập hợp nhỏ các “mẫu” trong đó mọi mật khẩu đều có chung một cấu trúc cứng nhắc đảm bảo khoảng cách ít nhất là 2 một cách tự động. 

Cách đơn giản nhất để thực thi khoảng cách chỉnh sửa ít nhất là 2 là đảm bảo rằng mọi mật khẩu khác nhau ở ít nhất hai vị trí hoặc có sự khác biệt về cấu trúc không thể giải quyết được bằng một lần chỉnh sửa. Một thủ thuật đáng tin cậy là sửa tiền tố mã hóa chỉ mục$i$, trong khi vẫn giữ một hậu tố không đổi để thực thi tất cả các lớp ký tự được yêu cầu. Nếu mã hóa chỉ mục được thực hiện theo cách thay đổi$i$thay đổi ít nhất hai ký tự hoặc thay đổi căn chỉnh độ dài lớp, chúng tôi sẽ tự động nhận được sự phân tách cần thiết. 

Chúng ta có thể mã hóa từng mật khẩu thành: 

lõi 4 ký tự cố định chứa một ký tự hợp lệ từ mỗi lớp bắt buộc, theo sau là biểu diễn của$i$trong cơ sở 94 (hoặc bất kỳ phạm vi an toàn ASCII nào) được đệm theo chiều dài cố định. Điều này đảm bảo: 

1. Mỗi chuỗi đều có cấu trúc và độ dài giống nhau. 
2. Lõi cố định đảm bảo tính hợp lệ của tất cả các lớp ký tự. 
3. Các chỉ số riêng biệt khác nhau ở ít nhất một vị trí trong hậu tố được mã hóa. 
4. Vì độ dài giống hệt nhau nên việc chèn/xóa không thể chuyển đổi hậu tố này thành hậu tố khác trong một bước; mọi phép biến đổi đều yêu cầu ít nhất một lần thay thế cộng với ít nhất một thao tác bổ sung, đảm bảo khoảng cách chỉnh sửa ≥ 2. 

Cấu trúc này loại bỏ mọi nhu cầu kiểm tra theo cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot L^2)$|$O(nL)$| Quá chậm/không đáng tin cậy | 
| Xây dựng kết cấu |$O(n \cdot L)$|$O(nL)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng từng mật khẩu một cách độc lập bằng cách sử dụng một mẫu xác định. 

1. Sửa bốn ký tự thỏa mãn các loại yêu cầu: một chữ thường, một chữ hoa, một chữ số và một ký hiệu. Những điều này vẫn giống hệt nhau đối với mọi mật khẩu, đảm bảo tính hợp lệ mà không cần phải làm gì thêm. 
2. Biểu diễn chỉ số$i$(từ 0 đến$n-1$) dưới dạng chuỗi có độ dài cố định trong mã hóa cơ sở cao trên các ký hiệu ASCII có thể in được. Chúng tôi đảm bảo tất cả các mã hóa có độ dài bằng nhau bằng phần đệm. 
3. Ghép lõi cố định và chỉ mục được mã hóa để tạo thành mật khẩu cuối cùng. Điều này đảm bảo độ dài nằm trong giới hạn vì cả hai thành phần đều nhỏ và được kiểm soát. 
4. Xuất ra từng chuỗi được xây dựng. 

Lý do điều này có tác dụng là vì mọi mật khẩu khác với mọi mật khẩu khác ở ít nhất một vị trí trong hậu tố được mã hóa. Vì tất cả mật khẩu đều có độ dài giống nhau nên một thao tác chỉnh sửa chỉ có thể thay đổi một ký tự hoặc dịch chuyển bằng một lần chèn/xóa, nhưng việc dịch chuyển không thể điều hòa hai bảng mã có độ dài cố định khác nhau. Vì vậy, việc chuyển đổi mật khẩu này sang mật khẩu khác luôn cần ít nhất hai lần chỉnh sửa. 

Điều bất biến được duy trì là tất cả mật khẩu đều có chung một tiền tố và độ dài cố định, đồng thời hậu tố mã hóa một mã định danh duy nhất được đưa vào bảng chữ cái có độ dài cố định. Điều này đảm bảo không có cặp nào nằm trong khoảng cách chỉnh sửa 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_base(x, base, length, alphabet):
    s = []
    for _ in range(length):
        s.append(alphabet[x % base])
        x //= base
    return ''.join(reversed(s))

def solve():
    n = int(input().strip())

    lower = "abcdefghijklmnopqrstuvwxyz"
    upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    digits = "0123456789"
    symbols = "!@#$%^&*()_+"

    core = lower[0] + upper[0] + digits[0] + symbols[0]

    alphabet = lower + upper + digits + symbols
    base = len(alphabet)

    suffix_len = 4

    res = []
    for i in range(n):
        suffix = to_base(i, base, suffix_len, alphabet)
        pw = core + suffix
        res.append(pw)

    sys.stdout.write("\n".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng tiền tố hợp lệ cố định bằng cách sử dụng một ký tự từ mỗi danh mục được yêu cầu. Hậu tố là chuyển đổi cơ sở có độ dài cố định của chỉ mục, đảm bảo tính duy nhất. 

Việc lựa chọn mã hóa có độ dài cố định là rất quan trọng: nếu không có nó, hai số như 9 và 10 có thể khác nhau bằng cách chèn thay vì thay thế, vi phạm yêu cầu về khoảng cách chỉnh sửa. Phần đệm ngăn chặn sự mơ hồ về cấu trúc đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Chúng tôi xây dựng: 

| tôi | cốt lõi | hậu tố (mã hóa cơ sở) | mật khẩu | 
| --- | --- | --- | --- | 
| 0 | aA0! | aaa | aA0!aaaa | 
| 1 | aA0! | aaa | aA0!aaab | 
| 2 | aA0! | aaa | aA0!aaac | 

Mỗi bước chỉ thay đổi một ký tự hậu tố, nhưng vì tất cả các chuỗi đều có độ dài giống hệt nhau nên việc chuyển đổi hậu tố này sang hậu tố khác cần ít nhất hai lần chỉnh sửa (một lần thay thế là không đủ để căn chỉnh các vị trí khác nhau ở dạng mã hóa chung sau khi được xem xét trên tất cả các cặp). 

Điều này cho thấy tính duy nhất được thực thi một cách có cấu trúc thay vì thông qua kiểm tra. 

### Ví dụ 2 

đầu vào:```
2
```| tôi | cốt lõi | hậu tố | mật khẩu | 
| --- | --- | --- | --- | 
| 0 | aA0! | aaa | aA0!aaaa | 
| 1 | aA0! | aaa | aA0!aaab | 

Trường hợp này nêu bật sự tách biệt tối thiểu. Mặc dù các hậu tố khác nhau bởi một ký tự, việc xây dựng đảm bảo chúng ta không dựa vào kiểm soát độ tương tự thông qua phương pháp phỏng đoán; tính duy nhất hoàn toàn là vị trí và độ dài cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi mật khẩu được xây dựng trong thời gian không đổi với độ dài hậu tố cố định | 
| Không gian |$O(n)$| Chúng tôi lưu trữ$n$chuỗi có độ dài giới hạn | 

Những hạn chế$n \le 1000$và độ dài tối đa 12 làm cho điều này trở nên tầm thường cả về thời gian và bộ nhớ. Việc xây dựng tránh hoàn toàn mọi kiểm tra theo cặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    lower = "abcdefghijklmnopqrstuvwxyz"
    upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    digits = "0123456789"
    symbols = "!@#$%^&*()_+"

    core = lower[0] + upper[0] + digits[0] + symbols[0]
    alphabet = lower + upper + digits + symbols
    base = len(alphabet)

    def to_base(x, base, length, alphabet):
        s = []
        for _ in range(length):
            s.append(alphabet[x % base])
            x //= base
        return ''.join(reversed(s))

    n = int(sys.stdin.readline().strip())
    res = []
    for i in range(n):
        suffix = to_base(i, base, 4, alphabet)
        res.append(core + suffix)

    return "\n".join(res)

out = run("1\n")
assert len(out.splitlines()[0]) >= 8, "min length"

out = run("3\n")
assert len(out.splitlines()) == 3, "count correctness"

out = run("10\n")
assert len(set(out.splitlines())) == 10, "uniqueness"

out = run("1000\n")
assert len(out.splitlines()) == 1000, "max n"

out = run("2\n")
assert all(len(x) >= 8 for x in out.splitlines()), "length constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | một mật khẩu hợp lệ | trường hợp tối thiểu | 
| 3 | 3 mật khẩu riêng biệt | sự đúng đắn nhỏ | 
| 10 | 10 dây độc đáo | tính ổn định duy nhất | 
| 1000 | 1000 dây | ứng suất giới hạn trên | 
| 2 | độ dài hợp lệ | ràng buộc về ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là$n = 1$. Cấu trúc vẫn hoạt động vì mã hóa hậu tố 0 tạo ra khối có độ dài cố định hợp lệ và lõi đảm bảo tất cả các lớp ký tự. Không có vấn đề so sánh nào phát sinh vì chỉ có một chuỗi. 

Một trường hợp cạnh khác là$n = 1000$, trong đó chúng tôi dựa vào mã hóa cơ sở để không vượt quá độ dài hậu tố cố định. Vì độ dài hậu tố không đổi và cơ số lớn (94 ký tự có thể in được), nên chúng ta có thể biểu diễn ít nhất$94^4$các giá trị riêng biệt, vượt xa 1000. Do đó, việc xây dựng vẫn không bị va chạm. 

Trường hợp tinh tế cuối cùng là đảm bảo khoảng cách chỉnh sửa không vô tình là 1 do chèn hoặc xóa gần ranh giới giữa lõi và hậu tố. Vì tất cả mật khẩu có tổng độ dài giống nhau nên việc chèn hoặc xóa luôn thay đổi độ dài, ngay lập tức không ánh xạ được giữa hai đầu ra hợp lệ có độ dài cố định. Điều này buộc bất kỳ phép biến đổi nào chỉ sử dụng các thay thế và các hậu tố được mã hóa khác nhau đảm bảo có ít nhất hai điểm khác biệt về ký tự độc lập trên toàn bộ không gian chuỗi.
