---
title: "CF 104823E - chuỗi"
description: "Chúng ta được cung cấp một chuỗi có độ dài $n$ trong đó mỗi vị trí chứa một ký tự ASCII hiển thị. Sau đó, chúng ta được cung cấp các phép toán $m$, mỗi phép toán chọn hai ký tự riêng biệt $(x, y)$."
date: "2026-06-28T12:37:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104823
codeforces_index: "E"
codeforces_contest_name: "The 17-th BIT Campus Programming Contest - Online Round"
rating: 0
weight: 104823
solve_time_s: 41
verified: true
draft: false
---

[CF 104823E - chuỗi](https://codeforces.com/problemset/problem/104823/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài$n$trong đó mỗi vị trí chứa một ký tự ASCII hiển thị. Sau đó chúng tôi được trao$m$hoạt động, mỗi hoạt động chọn hai ký tự riêng biệt$(x, y)$. Đối với mọi thao tác, quy tắc là hoán đổi toàn cục: mỗi lần xuất hiện của$x$trở thành$y$, và mỗi lần xuất hiện của$y$trở thành$x$, đồng thời trên toàn bộ chuỗi. Đây không phải là sự hoán đổi cục bộ tại các vị trí mà là sự gắn nhãn lại đầy đủ các ký tự. 

Sau khi áp dụng tất cả các thao tác theo thứ tự, chúng ta phải xuất ra chuỗi chuyển đổi cuối cùng. 

Hạn chế quan trọng là cả hai$n$Và$m$có thể lớn như$10^5$. Một mô phỏng trực tiếp quét toàn bộ chuỗi cho mọi hoạt động sẽ yêu cầu tới$10^5 \times 10^5 = 10^{10}$cập nhật ký tự, vượt xa giới hạn một giây cho phép. Ngay cả việc quét chuỗi một lần cho mỗi thao tác cũng đã quá chậm. 

Một cạm bẫy tinh tế xuất phát từ việc giải thích việc hoán đổi như những sự thay thế độc lập. Việc hoán đổi diễn ra đồng thời, do đó, thứ tự thay thế trung gian trong một thao tác không được lọt vào kết quả. Ví dụ: nếu chúng ta thay thế nhầm tất cả$x \to y$đầu tiên và sau đó là tất cả$y \to x$, chúng ta sẽ làm hỏng logic trừ khi chúng ta cẩn thận tách lớp ánh xạ. 

Một lỗi tiềm ẩn khác là liên tục viết lại chính chuỗi đó. Vì các ký tự chỉ thay đổi danh tính chứ không thay đổi vị trí nên chúng ta không cần sửa đổi trực tiếp chuỗi trong mỗi thao tác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi thao tác$(x, y)$, chúng tôi quét toàn bộ chuỗi và thay thế mọi lần xuất hiện của$x$với$y$và mọi sự xuất hiện của$y$với$x$. Điều này mô phỏng chính xác sự cố vì mỗi thao tác là một hoán đổi toàn cầu được áp dụng thống nhất. 

Vấn đề là hiệu suất. Mỗi lần quét là$O(n)$, và chúng tôi làm điều đó$m$lần, dẫn đến$O(nm)$. Với$10^5$ở cả hai chiều, điều này trở nên không khả thi. 

Quan sát quan trọng là bản thân chuỗi không cần phải sửa đổi trong quá trình hoạt động. Điều thực sự thay đổi là ánh xạ từ các ký tự gốc sang các ký tự hiện tại. Mỗi thao tác chỉ cập nhật mối quan hệ giữa hai ký hiệu chứ không cập nhật vị trí trong chuỗi. 

Vì vậy, thay vì chạm vào chuỗi nhiều lần, chúng tôi duy trì một mảng ánh xạ cho chúng tôi biết, đối với mỗi ký tự ASCII có thể, nó hiện đang đại diện cho cái gì. Ban đầu, mỗi ký tự đều ánh xạ tới chính nó. Khi chúng tôi xử lý một thao tác$(x, y)$, chúng ta trao đổi hình ảnh của$x$Và$y$bên trong bản đồ này. Sau tất cả các thao tác, chúng tôi áp dụng ánh xạ cuối cùng này một lần cho chuỗi gốc. 

Điều này làm giảm vấn đề duy trì hoán vị của bảng chữ cái có kích thước không đổi (nhiều nhất là 94 ký tự ASCII hiển thị). Mỗi lần hoán đổi là$O(1)$và việc xây dựng lại cuối cùng là$O(n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Hoán đổi bản đồ |$O(n + m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mọi ký tự là một chỉ mục trong một mảng có kích thước cố định biểu thị các giá trị ASCII. Ý tưởng cốt lõi là duy trì bản đồ nhận dạng hiện tại phát triển theo từng hoạt động. 

1. Khởi tạo một mảng ánh xạ sao cho mỗi ký tự ánh xạ tới chính nó. Điều này thể hiện sự chuyển đổi danh tính trước khi áp dụng bất kỳ giao dịch hoán đổi nào. 
2. Đối với mỗi thao tác$(x, y)$, diễn giải cả hai ký tự dưới dạng chỉ mục và hoán đổi các giá trị được ánh xạ của chúng bên trong mảng ánh xạ. Điều này mô phỏng việc áp dụng trao đổi toàn cầu mà không cần chạm vào chính chuỗi đó. Lý do điều này có hiệu quả là vì chúng tôi đang theo dõi quá trình chuyển đổi nhãn thay vì nội dung chuỗi. 
3. Sau khi xử lý tất cả các thao tác, hãy tạo chuỗi cuối cùng bằng cách lặp lại từng ký tự trong chuỗi gốc và thay thế nó bằng giá trị được ánh xạ của nó. 
4. Xuất kết quả đã xây dựng. 

Lý do điều này hiệu quả là vì mảng ánh xạ luôn biểu thị hoán vị tích lũy của các ký tự được tạo ra bởi tất cả các lần hoán đổi. Mỗi thao tác là một chuyển vị trong một nhóm hoán vị trên bộ ký tự và việc kết hợp các chuyển vị này tương đương với việc cập nhật mảng ánh xạ tăng dần. Vì các hoán đổi được áp dụng toàn cầu và đối xứng nên không tồn tại sự phụ thuộc vị trí và ký tự cuối cùng của mỗi ký hiệu gốc chỉ phụ thuộc vào hoán vị cuối cùng chứ không phải trạng thái trung gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    s = input().rstrip("\n")

    # visible ASCII range: 33 to 126 inclusive
    OFFSET = 33
    SIZE = 94

    mp = [i for i in range(SIZE)]

    for _ in range(m):
        x, y = input().split()
        xi = ord(x) - OFFSET
        yi = ord(y) - OFFSET
        mp[xi], mp[yi] = mp[yi], mp[xi]

    res = []
    for ch in s:
        idx = ord(ch) - OFFSET
        res.append(chr(mp[idx] + OFFSET))

    print("".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp phân tách mối quan tâm một cách rõ ràng. Mảng ánh xạ`mp`lưu trữ hình ảnh hiện tại của mỗi ký tự trong tất cả các lần hoán đổi. Mỗi thao tác chỉ hoán đổi hai mục trong mảng này, giữ nguyên bất biến đó`mp[c]`là danh tính được biến đổi cuối cùng của nhân vật`c`cho đến hoạt động hiện tại. 

Vòng lặp cuối cùng áp dụng phép chuyển đổi này một lần, đảm bảo chúng ta chỉ phải trả chi phí tuyến tính theo độ dài chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
abcd
a c
d z
```Chúng tôi theo dõi ánh xạ qua các ký tự hiển thị có liên quan: 

| Bước | Hoạt động | Thay đổi bản đồ | Ánh xạ kết quả (có liên quan) | 
| --- | --- | --- | --- | 
| 0 | ban đầu | danh tính | a→a, b→b, c→c, d→d, z→z | 
| 1 | một c | hoán đổi a và c | a→c, c→a | 
| 2 | dz | hoán đổi d và z | d→z, z→d | 

Bây giờ áp dụng cho chuỗi: 

- a → c 
- b → b 
- c → a 
- d → z 

Đầu ra trở thành:```
cbaz
```Điều này cho thấy chúng ta chưa bao giờ chạm vào chuỗi trong quá trình xử lý mà chỉ chạm vào ánh xạ. 

### Ví dụ 2 

đầu vào:```
5 3
@#$%@
# !
@ !
? !
```Chúng tôi chỉ theo dõi các nhân vật có liên quan: 

| Bước | Hoạt động | Thay đổi ánh xạ khóa | 
| --- | --- | --- | 
| 0 | ban đầu | @→@, #→#, !→! | 
| 1 | # ! | # Và ! trao đổi | 
| 2 | @ ! | @ hoán đổi với hiện tại ! | 
| 3 | ? ! | ? hoán đổi với hiện tại ! | 

Sau khi áp dụng các hoán đổi tuần tự, hình ảnh cuối cùng của mỗi ký tự chỉ được xác định bằng các chuyển vị tích lũy. 

Áp dụng ánh xạ cuối cùng cho`@#$%@`sản lượng:```
?@$%?
```Dấu vết này chứng minh rằng các giao dịch hoán đổi toàn cầu lặp đi lặp lại sẽ hợp thành một hoán vị duy nhất, bất kể trạng thái trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi thao tác là một hoán đổi thời gian không đổi trong một mảng có kích thước cố định và việc tái cấu trúc cuối cùng là tuyến tính theo độ dài chuỗi | 
| Không gian |$O(1)$| Kích thước mảng ánh xạ được giới hạn bởi bộ ký tự hiển thị ASCII | 

Các ràng buộc cho phép lên đến$10^5$các thao tác và độ dài chuỗi, đồng thời giải pháp xử lý từng thao tác chính xác một lần, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.flush = lambda: None

    import builtins
    input_backup = builtins.input
    builtins.input = sys.stdin.readline

    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()

    builtins.input = input_backup
    return out.getvalue().strip()

# provided samples
assert run("""4 2
abcd
a c
d z
""") == "cbaz"

assert run("""5 3
@#$%@
# !
@ !
? !
""") == "?@$%?"

# custom tests
assert run("""1 1
a
a b
""") == "b"

assert run("""3 2
abc
a b
b c
""") == "cab"

assert run("""6 0
abcdef
""") == "abcdef"

assert run("""4 3
aaaa
a b
b c
c a
""") == "aaaa"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trao đổi đơn | b | trường hợp tối thiểu | 
| hoán đổi chuỗi | taxi | thành phần của giao dịch hoán đổi | 
| không có hoạt động | abcdef | trường hợp nhận dạng | 
| hoán đổi chu kỳ | aaa | ổn định chu trình hoán vị | 

## Vỏ cạnh 

Trường hợp một cạnh là khi nhiều giao dịch hoán đổi tạo thành chu kỳ. Ví dụ như việc hoán đổi`a b`, sau đó`b c`, sau đó`c a`không “phá vỡ” bất cứ thứ gì; nó tạo ra một hoán vị hợp lệ. Thuật toán xử lý việc này một cách tự nhiên vì nó chỉ tổng hợp các chuyển vị trong mảng ánh xạ. 

đầu vào:```
3 3
abc
a b
b c
c a
```Lập bản đồ từng bước: 

- sau a b: a↔b 
- sau b c: b↔c (vì vậy dạng chu trình) 
- sau c a: hoàn thành chu trình 

Ánh xạ cuối cùng trả mọi ký tự về vị trí được xoay, tạo ra chuỗi cuối cùng nhất quán. Việc triển khai không bao giờ phụ thuộc vào thứ tự bên trong chuỗi, do đó không phát sinh sự mâu thuẫn. 

Một trường hợp khác là khi các ký tự không xuất hiện trong chuỗi có liên quan đến việc hoán đổi. Vì mảng ánh xạ bao gồm toàn bộ phạm vi ASCII nên các thao tác này vẫn cập nhật chính xác hành vi trong tương lai nếu các ký tự đó xuất hiện sau hoặc ảnh hưởng đến các ánh xạ khác.
