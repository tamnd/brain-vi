---
title: "CF 1023A - So khớp mẫu ký tự đại diện đơn"
description: "Chúng ta được cung cấp một chuỗi mẫu s và một chuỗi mục tiêu t. Mẫu trông gần giống như một chuỗi chữ thường bình thường, ngoại trừ việc nó có thể chứa một ký tự đặc biệt."
date: "2026-06-16T21:52:32+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1023
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 504 (rated, Div. 1 + Div. 2, based on VK Cup 2018 Final)"
rating: 1200
weight: 1023
solve_time_s: 116
verified: true
draft: false
---

[CF 1023A - So khớp mẫu ký tự đại diện đơn](https://codeforces.com/problemset/problem/1023/A) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi mẫu`s`và một chuỗi mục tiêu`t`. Mẫu trông gần giống như một chuỗi chữ thường bình thường, ngoại trừ việc nó có thể chứa một ký tự đặc biệt`*`. Ký tự đại diện này rất linh hoạt: khi chúng ta “khởi tạo” mẫu, chúng ta được phép thay thế`*`với bất kỳ chuỗi chữ thường nào, kể cả chuỗi trống. Mọi thứ khác trong`s`phải không thay đổi. 

Nhiệm vụ là quyết định liệu chúng ta có thể chọn một số thay thế cho`*`để chuỗi kết quả trở nên chính xác bằng`t`. 

Từ góc độ ràng buộc, cả hai chuỗi có thể rất lớn, lên tới 200.000 ký tự. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng kiểm tra rõ ràng nhiều sự thay thế cho`*`hoặc xây dựng tất cả các mở rộng có thể. Mọi giải pháp đều phải kiểm tra chuỗi theo thời gian tuyến tính và chỉ thực hiện công việc không đổi trên mỗi ký tự. 

Một trường hợp cạnh tinh tế xuất phát từ thực tế là`*`có thể biến mất hoàn toàn. Nếu ai đó cho rằng nó phải đóng góp ít nhất một ký tự, họ sẽ thất bại trong trường hợp`s = "ab*cd"`Và`t = "abcd"`. 

Một cạm bẫy phổ biến khác là quên rằng`*`có thể đại diện cho một chuỗi rất dài, không chỉ một ký tự. Ví dụ, nếu`s = "a*b"`Và`t = "axxxxb"`, đoạn giữa có thể lớn tùy ý. 

Cuối cùng, trường hợp nguy hiểm nhất là khi`s`không có`*`không hề. Sau đó, câu trả lời hoàn toàn là kiểm tra sự bình đẳng. Một cách tiếp cận ngây thơ luôn cố gắng chia rẽ`*`không kiểm tra sự tồn tại của nó sẽ coi toàn bộ chuỗi là logic tiền tố hoặc hậu tố không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử tất cả các lựa chọn thay thế có thể có cho ký tự đại diện. Nếu như`*`xuất hiện giữa vị trí`L`Và`R`TRONG`s`, chúng tôi sẽ cố gắng căn chỉnh`s`tiền tố và hậu tố của`t`và điền vào giữa tùy ý. Tuy nhiên, số lượng chuỗi có thể thay thế`*`là hàm mũ theo chiều dài của khoảng cách. Ngay cả khi chúng ta chỉ xem xét độ dài, chúng ta cũng cần phải thử tất cả các giá trị từ`0`lên đến`m`và với mỗi độ dài, hãy xây dựng lại một chuỗi hoặc xác thực căn chỉnh. Điều này dẫn đến ít nhất hành vi bậc hai trong trường hợp xấu nhất. 

Quan sát quan trọng là ký tự đại diện là phần linh hoạt duy nhất. Mọi thứ ở bên trái của`*`phải khớp với tiền tố của`t`và mọi thứ ở bên phải phải khớp với hậu tố của`t`. Khi những phần cố định đó đã được căn chỉnh, câu hỏi duy nhất còn lại là liệu`t`có đủ “khoảng trống ở giữa” để phù hợp với việc mở rộng ký tự đại diện. 

Điều này làm giảm vấn đề phân tách cả hai chuỗi xung quanh ký tự đại diện và kiểm tra ba điều kiện: khớp tiền tố, khớp hậu tố và tính khả thi về độ dài. Thay vì khám phá tất cả các lựa chọn thay thế, chúng tôi chỉ xác nhận một ràng buộc về cấu trúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) hoặc tệ hơn | O(m) | Quá chậm | 
| Tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét`s`để tìm vị trí của`*`nếu nó tồn tại. Nếu không có ký tự đại diện, câu trả lời sẽ giảm xuống việc kiểm tra xem`s == t`. Điều này là cần thiết vì nếu không có tính linh hoạt thì khuôn mẫu sẽ cố định. 
2. Nếu`*`tồn tại, chia tách`s`thành hai phần cố định: tiền tố`left = s[:i]`và hậu tố`right = s[i+1:]`. Đây là những phần phải khớp chính xác với`t`. 
3. Kiểm tra xem`t`ít nhất phải bằng chiều dài của các bộ phận cố định kết hợp lại. Cụ thể là chúng ta cần`len(t) >= len(left) + len(right)`. Nếu không, ngay cả một sự thay thế trống cũng không thể thu hẹp khoảng cách. 
4. So sánh`left`với tiền tố của`t`. Tức là xác minh`t[:len(left)] == left`. Điều này đảm bảo ký tự đại diện không được sử dụng để sửa đổi cấu trúc cố định. 
5. So sánh`right`với hậu tố của`t`. Tức là xác minh`t[-len(right):] == right`. Điều này đảm bảo phần đuôi của mẫu căn chỉnh chính xác sau khi chèn phần mở rộng ký tự đại diện. 
6. Nếu cả tiền tố và hậu tố đều khớp và điều kiện độ dài được giữ nguyên, ký tự đại diện có thể hấp thụ chính xác đoạn giữa của`t`. Nếu không thì không thể. 

### Tại sao nó hoạt động 

Ký tự đại diện là nguồn tự do duy nhất và nó hoạt động giống như một đoạn liền kề được chèn vào giữa hai chuỗi cứng. Mọi kết quả khớp hợp lệ đều phải bảo toàn thứ tự chính xác của các ký tự bên ngoài vùng ký tự đại diện. Vì vậy, giải pháp nào cũng phải căn chỉnh tiền tố của`s`với tiền tố của`t`và hậu tố của`s`với hậu tố của`t`. Khi những ràng buộc đó được thỏa mãn, phần còn lại của`t`tương ứng duy nhất với việc thay thế ký tự đại diện. Không có bậc tự do nào khác tồn tại, vì vậy những kiểm tra này vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    s = input().strip()
    t = input().strip()

    if '*' not in s:
        print("YES" if s == t else "NO")
        return

    i = s.index('*')
    left = s[:i]
    right = s[i+1:]

    if len(left) + len(right) > m:
        print("NO")
        return

    if t[:len(left)] != left:
        print("NO")
        return

    if t[m - len(right):] != right:
        print("NO")
        return

    print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xử lý trường hợp suy biến trong đó không có ký tự đại diện nào tồn tại, vì điều đó làm giảm vấn đề thành kiểm tra đẳng thức trực tiếp. Sau khi tìm thấy ký tự đại diện, chúng tôi chia mẫu thành các phần tiền tố và hậu tố một cách rõ ràng, đồng thời tránh chạm vào chính ký tự đại diện đó. 

Việc kiểm tra độ dài là rất quan trọng vì nó ngăn chặn việc cắt lát không hợp lệ trên các chuỗi ngắn và thực thi điều kiện khả thi là ký tự đại diện phải bao gồm phần còn lại của chuỗi.`t`. Việc so sánh tiền tố và hậu tố được thực hiện bằng cách cắt trực tiếp, đảm bảo xác minh O(1)-mỗi ký tự. 

Một chi tiết triển khai tinh tế đang sử dụng`m - len(right)`thay vì cố gắng suy luận về vị trí mà ký tự đại diện sẽ xuất hiện một cách linh hoạt. Điều này tránh các lỗi riêng lẻ và giữ cho việc căn chỉnh hậu tố không phụ thuộc vào độ lớn của ký tự đại diện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 10
code*s
codeforces
```| Bước | trái | đúng | khớp tiền tố | khớp hậu tố | độ dài hợp lệ | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | "mã" | "s" | - | - | - | - | 
| kiểm tra | - | - | mã == mã | s == s | 10 >= 6 | CÓ | 

Tiền tố “code” khớp với phần đầu của`t`và hậu tố “s” khớp với phần cuối. Phần “lực” còn lại chính là thứ thay thế`*`. 

### Ví dụ 2 

đầu vào:```
4 4
a*b*
abca
```| Bước | trái | đúng | khớp tiền tố | khớp hậu tố | độ dài hợp lệ | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | "một" | "b*" | - | - | - | - | 
| kiểm tra | - | - | một == một | b* ≠ ca | 4 >= 2 | KHÔNG | 

Ở đây việc so sánh hậu tố không thành công vì`right = "b*"`được coi là văn bản cố định và phải khớp chính xác, điều này không có. Điều này cho thấy việc xử lý ký tự đại diện hoàn toàn chỉ ở một vị trí và không cho phép mở rộng nhiều lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Chúng tôi quét`s`một lần và so sánh tiền tố và hậu tố với`t`sử dụng cắt | 
| Không gian | O(1) | Chỉ sử dụng các chỉ số và lát cắt; không có cấu trúc phụ trợ tỷ lệ thuận với kích thước đầu vào | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì mỗi ký tự được kiểm tra tối đa một lần và không xảy ra quá trình xử lý lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        s = input().strip()
        t = input().strip()

        if '*' not in s:
            print("YES" if s == t else "NO")
            return

        i = s.index('*')
        left = s[:i]
        right = s[i+1:]

        if len(left) + len(right) > m:
            print("NO")
            return

        if t[:len(left)] != left:
            print("NO")
            return

        if t[m - len(right):] != right:
            print("NO")
            return

        print("YES")

    solve()
    return sys.stdout.getvalue().strip()

# samples
assert run("6 10\ncode*s\ncodeforces\n") == "YES"

# no wildcard exact match
assert run("3 3\nabc\nabc\n") == "YES"

# no wildcard mismatch
assert run("3 3\nabc\nabd\n") == "NO"

# wildcard empty match
assert run("3 2\nab*\nab\n") == "YES"

# wildcard too short target
assert run("4 2\na*b*\nab\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có ký tự đại diện phù hợp | CÓ | trường hợp bình đẳng | 
| không có ký tự đại diện không khớp | KHÔNG | kết hợp chặt chẽ | 
| mở rộng trống ký tự đại diện | CÓ | xử lý thay thế trống | 
| không đủ độ dài | KHÔNG | thực thi hạn chế độ dài | 

## Vỏ cạnh 

Trường hợp một cạnh là khi ký tự đại diện ở đầu, chẳng hạn như`s = "*abc"`Và`t = "zzzabc"`. Bộ thuật toán`left = ""`và chỉ kiểm tra hậu tố. Kết hợp hậu tố`t[-3:] == "abc"`thành công và việc kiểm tra tiền tố hoàn toàn đúng. 

Một trường hợp khác là khi ký tự đại diện ở cuối, chẳng hạn như`s = "abc*"`Và`t = "abcxyz"`. Đây`right = ""`, do đó, việc kiểm tra hậu tố sẽ so sánh một chuỗi trống, chuỗi này luôn thành công và chỉ có ràng buộc tiền tố mới quan trọng. 

Một trường hợp tế nhị cuối cùng là khi`s`không có ký tự đại diện và dài hơn`t`. Thuật toán ngay lập tức rơi vào nhánh kiểm tra đẳng thức, loại bỏ chính xác mà không cần thử bất kỳ logic cắt nào, ngăn ngừa các lỗi chỉ mục ngẫu nhiên.
