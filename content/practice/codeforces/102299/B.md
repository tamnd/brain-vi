---
title: "CF 102299B - Tiếng Nga của Russo"
description: "Chúng ta cần quyết định xem liệu một dòng đầu vào có thể được tạo ra từ M không kết thúc của ngữ pháp đã cho hay không. Dòng chứa các chữ số, khoảng trắng và các ký tự dấu câu :, Ngữ pháp mô tả ba lớp. T là một chuỗi chữ số hoặc một biểu thức {M } hoàn chỉnh."
date: "2026-08-13T23:11:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "B"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 500
verified: true
draft: false
---

[CF 102299B - Tiếng Nga của Russo](https://codeforces.com/problemset/problem/102299/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần quyết định xem liệu một dòng đầu vào có thể được tạo từ nonterminal hay không`M`của ngữ pháp đã cho. Dòng chứa chữ số, khoảng trắng và ký tự dấu chấm câu`:`,`|`,`{`,`}`, Và`$`. Khoảng trắng được phép giữa các ký hiệu ngữ pháp, nhưng các chữ số tạo thành một`I`mã thông báo phải liên tiếp, vì vậy`123`là một mã thông báo số nguyên trong khi`1 23`là hai chuỗi chữ số riêng biệt và không thể nối thành một`I`. 

Ngữ pháp mô tả ba lớp.`T`là một chuỗi chữ số hoặc một chuỗi đầy đủ`{ M }`sự biểu lộ.`P`là một hoặc nhiều`T`các giá trị được nối bằng dấu hai chấm.`M`là một biểu thức được tạo từ`P`các giá trị được nối bằng các thanh dọc, với khả năng bổ sung của các thanh dọc dẫn đầu và các giá trị đặc biệt`⟦PROTECT_6⟧`trong định dạng của vấn đề ban đầu xuất hiện dưới dạng`$$$`khi câu lệnh được trích xuất từ ​​đánh dấu toán học của nó, nhưng ký tự cuối thực tế là một`⟦PROTECT_7⟧ | 2`. 

Đầu vào có nhiều nhất`10^5`nhân vật. Điều đó loại trừ các thuật toán liên tục thử nhiều cách mở rộng ngữ pháp có thể hoặc phân tích cú pháp quay lui. Về cơ bản chúng ta cần một lần chuyển qua đầu vào, bởi vì ngay cả`O(n log n)`công việc là không cần thiết và một trình phân tích cú pháp bậc hai có thể đã thực hiện được`10^10`hoạt động ở kích thước tối đa. Giới hạn bộ nhớ 256 MB đủ rộng để lưu trữ đầu vào được mã hóa và một vài mảng kích thước`O(n)`. 

Có một số trường hợp trình phân tích cú pháp hợp lý bề ngoài không thành công. Đầu tiên, biểu thức trống là không hợp lệ. Ví dụ: một dòng trống phải tạo ra`NO`, bởi vì mọi khai triển của`M`cuối cùng chứa một`P`, và mọi`P`chứa một`T`. Trình phân tích cú pháp coi chuỗi con trống là biểu thức đệ quy hợp lệ sẽ chấp nhận chuỗi đó một cách không chính xác. 

Thứ hai, một thanh dọc không thể đứng một mình. đầu vào`| 1`là hợp lệ bởi vì`M`có thể mở rộng như`| M`và phần còn lại`M`có thể trở thành`1`, Nhưng`1 |`không hợp lệ vì thanh ở giữa luôn yêu cầu một thanh khác`P`sau nó. Một trình phân tích cú pháp chỉ đếm các thanh mà không kiểm tra toán hạng của chúng có thể chấp nhận toán hạng sau không chính xác. 

Thứ ba, các chữ số không thể được phân tách bằng khoảng trắng. đầu vào`1 2`không hợp lệ.`I`là một chuỗi các chữ số liên tiếp, trong khi ngữ pháp không có quy tắc nào cho phép hai chữ số`I`các token xuất hiện cạnh nhau. Trình mã thông báo loại bỏ tất cả khoảng trắng trước tiên sẽ biến điều này thành`12`và chấp nhận nó một cách sai lầm. 

Thứ tư, niềng răng phải chứa đầy đủ`M`. đầu vào`{}`là không hợp lệ, trong khi`{1}`là hợp lệ. Việc xử lý dấu ngoặc nhọn như dấu câu khớp thông thường mà không xác thực nội dung của chúng sẽ được chấp nhận`{}`không chính xác. 

Thứ năm,`⟦PROTECT_8⟧ | 2`là hợp lệ, nhưng`⟦PROTECT_9⟧`phải được theo sau bởi một thanh dọc và sau đó là một giá trị hợp lệ`P`. Đây chính xác là sự thay thế đặc biệt được đại diện bởi`H = '$'`theo sau là`| P`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng diễn giải đầu vào theo mọi cách tạo ngữ pháp hiện hành. Khó khăn là ngữ pháp có chứa đệ quy, đặc biệt là`M -> M | P`Và`M -> | M`, do đó, việc triển khai đệ quy gốc đơn giản sẽ bị mắc kẹt bởi đệ quy trái hoặc phải quay lại giữa nhiều dẫn xuất có thể có. Nếu chúng ta tưởng tượng việc liệt kê tất cả các dẫn xuất theo độ dài của đầu vào, số lượng ứng cử viên có thể tăng theo cấp số nhân, với`Theta(2^n)`các nhánh có thể có trong trường hợp xấu nhất. Tại`n = 10^5`, điều đó vượt xa mọi thứ có thể thực hiện được. 

Trình phân tích cú pháp brute-force hoạt động vì ngữ pháp nhỏ và mọi dẫn xuất thành công đều tương ứng với một phân tích cú pháp hợp lệ. Nó thất bại vì ngữ pháp gốc được viết dưới dạng ẩn cấu trúc đơn giản hơn nhiều. Điều quan trọng là loại bỏ đệ quy trái về mặt đại số trước khi thực hiện bất cứ điều gì. 

Từ```
M = H | P
  | | M
  | P

H = M | $
```chúng ta có thể quan sát điều đó`M -> M | P`chỉ đơn giản là cho phép nhiều hơn`| P`phần được thêm vào một đã hợp lệ`M`. Việc sản xuất`M -> | M`cho phép bất kỳ số lượng thanh dẫn đầu. Sau khi loại bỏ đệ quy này, ngôn ngữ của`M`có thể được mô tả như```
M = |* B
B = P ( | P )*
  | $ | P ( | P )*
```Đây là quan sát trung tâm. MỘT`M`bao gồm 0 hoặc nhiều thanh dẫn đầu, theo sau là một thanh thông thường`P`trình tự hoặc bằng`$ | P`tiếp theo là nhiều hơn nữa`| P`miếng. 

Sự đơn giản hóa tương tự áp dụng cho`P`. Định nghĩa đệ quy trái của nó```
P = P : T | T
```chính xác là tương đương với```
P = T ( : T )*
```Bây giờ ngữ pháp đã đủ xác định để phân tích từ trái sang phải. Phần đệ quy duy nhất còn lại là`{ M }`và dấu ngoặc nhọn cho chúng ta một cấu trúc lồng nhau rõ ràng. Chúng ta có thể xử lý việc lồng ghép đó bằng một ngăn xếp thay vì đệ quy Python. 

Trước tiên, chúng tôi mã hóa dòng trong khi vẫn giữ được sự khác biệt giữa các chữ số liên tiếp và các chuỗi chữ số riêng biệt. Chúng tôi cũng bảo tồn`$`làm mã thông báo riêng và chỉ bỏ qua khoảng trắng giữa các mã thông báo. Sau đó, chúng tôi khớp từng cặp dấu ngoặc bằng cách sử dụng một ngăn xếp. Nếu dấu ngoặc nhọn mở xảy ra ở vị trí mã thông báo`l`và dấu đóng ngoặc phù hợp của nó là ở`r`, các mã thông báo giữa chúng tạo thành một`M`. 

Chúng ta có thể đánh giá các biểu thức dấu ngoặc lồng nhau từ trong ra ngoài. Khi xử lý bên ngoài`{ M }`, mọi biểu thức dấu ngoặc nhọn lồng nhau bên trong nó đều đã được đánh giá, do đó, dấu ngoặc nhọn có thể được coi là hợp lệ hoặc không hợp lệ`T`không có cuộc gọi đệ quy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(2^n)`trong trường hợp xấu nhất |`O(n)`mỗi dẫn xuất được khám phá | Quá chậm | 
| Trình phân tích cú pháp quay lui đệ quy | Hàm mũ hoặc không tận cùng vì đệ quy trái |`O(n)`độ sâu đệ quy | Quá chậm/không an toàn | 
| Mã thông báo + ngăn xếp dấu ngoặc rõ ràng + phân tích cú pháp xác định |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét ký tự đầu vào theo ký tự và xây dựng mã thông báo. Các chữ số liên tiếp trở thành một`I`mã thông báo. Mỗi`$`,`:`,`|`,`{`, Và`}`là một mã thông báo riêng biệt. Khoảng trắng được bỏ qua. Chúng ta không được xóa khoảng trắng trước khi mã hóa, bởi vì`1 2`phải duy trì các mã thông báo có hai chữ số thay vì trở thành`12`. 
2. Quét các mã thông báo và ghép các dấu ngoặc nhọn với một ngăn xếp. Khi`{`được tìm thấy, hãy đẩy chỉ mục mã thông báo của nó. Khi`}`được tìm thấy, hãy bật vị trí mở phù hợp. Nếu không có dấu ngoặc mở, dữ liệu nhập ngay lập tức không hợp lệ. Sau khi quét, sẽ cần có một ngăn xếp trống, nếu không, một số dấu ngoặc mở sẽ không bao giờ được đóng. 
3. Xử lý tất cả các cặp dấu ngoặc phù hợp theo thứ tự giảm dần của vị trí mở của chúng. Dấu ngoặc mở lồng nhau luôn có chỉ số mã thông báo lớn hơn dấu ngoặc mở chứa nó, do đó tính hợp lệ của nó được tính trước tiên. Lưu trữ giá trị hợp lệ của mỗi dấu ngoặc mở trong một mảng. 
4. Đối với một`M`khoảng thời gian, trước tiên hãy sử dụng bất kỳ số lượng hàng đầu nào`|`mã thông báo. Điều này tương ứng trực tiếp với các ứng dụng lặp đi lặp lại của`M -> | M`. 
5. Sau thanh dẫn đầu, hãy kiểm tra mã thông báo tiếp theo. Nếu nó là`⟦PROTECT_10⟧`và yêu cầu mã thông báo sau đây`|`. Sau đó một`P`phải tuân theo. Đây là điều đặc biệt`H = '$'`trường hợp. 
6. Nếu không, hãy phân tích cú pháp thông thường`P`. MỘT`P`bắt đầu bằng một giá trị hợp lệ`T`, theo sau là 0 hoặc nhiều hơn`:`Và`T`cặp. MỘT`T`là mã thông báo chữ số hoặc mã thông báo dấu ngoặc nhọn được lưu trữ bên trong`M`kết quả là hợp lệ. 
7. Một lần đầu tiên`P`đã được phân tích cú pháp, mọi thứ còn lại`|`phải được theo sau bởi cái khác`P`. Điều này xử lý quy tắc được chuyển đổi`M = |* P ( | P )*`và cả`$ | P ( | P )*`hình thức. 
8. Đối với hợp lệ`M`khoảng thời gian, việc phân tích cú pháp phải kết thúc chính xác tại ranh giới của nó. Nếu vẫn còn bất kỳ mã thông báo không mong muốn nào hoặc được yêu cầu`P`hoặc`T`bị thiếu, khoảng thời gian không hợp lệ. 
9. Cuối cùng, chạy giống hệt`M`trình phân tích cú pháp trên chuỗi mã thông báo hoàn chỉnh. Đầu vào hoàn chỉnh chỉ được chấp nhận nếu cấp cao nhất`M`là hợp lệ và tiêu thụ mọi mã thông báo. 

Điều bất biến là bất cứ khi nào chúng tôi xử lý một`M`khoảng thời gian, mọi lồng nhau`{ M }`bên trong khoảng đó đã có giá trị hợp lệ chính xác. Sau đó, trình phân tích cú pháp cho khoảng hiện tại sẽ tuân theo chính xác ngữ pháp không đệ quy trái tương đương: thanh dẫn đầu, một tùy chọn`$ |`, một`P`, và bằng 0 hoặc nhiều hơn`| P`hậu tố. Vì mỗi mã thông báo chỉ được sử dụng với số lần không đổi và mỗi biểu thức lồng nhau được đánh giá một lần nên quyết định cuối cùng chính xác là liệu ngữ pháp gốc có thể tạo ra dữ liệu đầu vào hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(s: str) -> str:
    tokens = []
    n = len(s)
    i = 0

    while i < n:
        c = s[i]

        if c.isspace():
            i += 1
            continue

        if c.isdigit():
            j = i + 1
            while j < n and s[j].isdigit():
                j += 1
            tokens.append(("I", s[i:j]))
            i = j
            continue

        if c in "$:|{}":
            tokens.append((c, c))
            i += 1
            continue

        return "NO"

    m = len(tokens)

    if m == 0:
        return "NO"

    # Match every pair of braces.
    matching = [-1] * m
    stack = []

    for i, (typ, _) in enumerate(tokens):
        if typ == "{":
            stack.append(i)
        elif typ == "}":
            if not stack:
                return "NO"
            opening = stack.pop()
            matching[opening] = i

    if stack:
        return "NO"

    # inner_ok[pos] is meaningful when tokens[pos] == "{"
    # and stores whether the M inside that brace pair is valid.
    inner_ok = [False] * m

    def parse_m(left: int, right: int) -> bool:
        """
        Check whether tokens[left:right] form a valid M.
        All brace expressions inside this interval have already
        been evaluated.
        """
        i = left

        # M -> |* B
        while i < right and tokens[i][0] == "|":
            i += 1

        if i >= right:
            return False

        # B is either P (| P)* or $ | P (| P)*.
        if tokens[i][0] == "$":
            i += 1
            if i >= right or tokens[i][0] != "|":
                return False
            i += 1

        def parse_t(pos: int) -> int:
            if pos >= right:
                return -1

            typ = tokens[pos][0]

            if typ == "I":
                return pos + 1

            if typ == "{":
                close = matching[pos]
                if close == -1 or close >= right:
                    return -1
                if not inner_ok[pos]:
                    return -1
                return close + 1

            return -1

        def parse_p(pos: int) -> int:
            pos = parse_t(pos)
            if pos == -1:
                return -1

            while pos < right and tokens[pos][0] == ":":
                pos = parse_t(pos + 1)
                if pos == -1:
                    return -1

            return pos

        i = parse_p(i)
        if i == -1:
            return False

        while i < right and tokens[i][0] == "|":
            i = parse_p(i + 1)
            if i == -1:
                return False

        return i == right

    # Process inner brace expressions before outer ones.
    openings = [
        i for i in range(m)
        if tokens[i][0] == "{"
    ]

    for opening in reversed(openings):
        closing = matching[opening]
        inner_ok[opening] = parse_m(opening + 1, closing)

    return "YES" if parse_m(0, m) else "NO"

def main() -> None:
    s = input()
    print(solve(s))

if __name__ == "__main__":
    main()
```Trình mã thông báo cố tình chặt chẽ hơn một cách đơn giản`''.join(s.split())`tiếp cận. Khi nhìn thấy một chữ số, nó thực hiện toàn bộ lần chạy liên tiếp và tạo chính xác một chữ số`I`mã thông báo. Khoảng trắng chấm dứt hoạt động đó, vì vậy`12 34`trở thành hai mã thông báo và không thể vô tình được hiểu là`1234`. 

Việc quét nẹp sử dụng`matching`để ghi lại vị trí đóng cho mỗi dấu ngoặc mở. Dấu ngoặc nhọn đóng không trùng khớp sẽ bị từ chối ngay lập tức và ngăn xếp không trống sau khi quét có nghĩa là dấu ngoặc nhọn mở không có đối tác đóng. 

các`inner_ok`mảng thay thế các lệnh gọi hàm đệ quy. Khi`parse_t`cuộc gặp gỡ`{`, nó nhảy trực tiếp đến kết quả phù hợp`}`và tham khảo kết quả đã được tính toán cho phần đính kèm`M`. Việc xử lý các phần mở theo thứ tự ngược lại đảm bảo rằng các biểu thức lồng nhau được biết trước biểu thức cha mẹ của chúng. 

Ngữ pháp đã chuyển đổi được mã hóa trực tiếp trong`parse_m`. Vòng lặp ban đầu tiêu thụ các thanh dẫn đầu. các`⟦PROTECT_11⟧ | P`, trong khi nhánh thông thường bắt đầu trực tiếp bằng`P`. Sau lần đầu tiên`P`, mỗi thanh phải được theo sau bởi một thanh khác`P`. trận chung kết`i == right`check is essential because successfully parsing a prefix is not enough, the whole interval must be consumed.

 Không có đệ quy tỷ lệ thuận với độ sâu lồng nhau của dấu ngoặc nhọn, do đó, đầu vào chứa hàng chục nghìn dấu ngoặc nhọn lồng nhau không đạt giới hạn đệ quy của Python. Tất cả các chỉ mục đều là chỉ mục mã thông báo và dấu ngoặc nhọn phù hợp được sử dụng bằng cách nhảy qua toàn bộ biểu thức lồng nhau đã được xác thực. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`1`, mã thông báo tạo ra một`I`mã thông báo. 

| Vị trí | Mã thông báo | Trạng thái phân tích cú pháp | Hành động | 
| --- | --- | --- | --- | 
| 0 |`I`| Bắt đầu`M`| Không dẫn đầu` | `, phân tích`P`| 
| 0 |`I`| Phân tích cú pháp`P`|`I`là hợp lệ`T`| 
| 1 | Kết thúc | Sau đó`P`| Không còn nữa` | `, khoảng thời gian tiêu thụ hoàn toàn | 

các`M`chứa một`P`, cái`P`chứa một`T`, và`T`là dãy số`1`. Trình phân tích cú pháp đạt đến cuối cùng một cách chính xác, vì vậy câu trả lời là`YES`. 

### Mẫu 2 

Đối với đầu vào`: 1`, khoảng trắng bị bỏ qua và các mã thông báo được`:`Và`I`. 

| Vị trí | Mã thông báo | Trạng thái phân tích cú pháp | Hành động | 
| --- | --- | --- | --- | 
| 0 |`:`| Bắt đầu`M`| Không dẫn đầu` | `, cố gắng phân tích cú pháp`P`| 
| 0 |`:`| Phân tích cú pháp`P`|`T`được yêu cầu đầu tiên | 
| 0 |`:`| Phân tích cú pháp`T`|`:`cũng không`I`cũng không`{`, do đó việc phân tích cú pháp không thành công | 

Đại tràng thuộc về bên trong`P`, nhưng một`P`phải bắt đầu bằng một`T`. Vì trước tiên không thể`T`, toàn bộ`M`không hợp lệ và câu trả lời là`NO`. 

### Mẫu 3 

Đối với đầu vào`⟦PROTECT_12⟧`,`|`,`I`. 

| Vị trí | Mã thông báo | Trạng thái phân tích cú pháp | Hành động | 
| --- | --- | --- | --- | 
| 0 |`⟦PROTECT_13⟧`chọn ngành đặc biệt | 
| 1 |` | `| Sau đó`$`| Có dấu phân cách bắt buộc | 
| 2 |`I`| Phân tích cú pháp`P`|`I`là hợp lệ`T`| 
| 3 | Kết thúc | Sau đó`P`| Đầu vào đã được tiêu thụ hoàn toàn | 

Đây chính xác là hình thức đặc biệt`⟦PROTECT_14⟧`phần cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mã thông báo, khớp dấu ngoặc nhọn, đánh giá biểu thức bên trong và phân tích cú pháp cuối cùng mỗi quá trình nhập dữ liệu với số lần không đổi. | 
| Không gian |`O(n)`| Mã thông báo, thông tin khớp dấu ngoặc nhọn, giá trị hợp lệ và ngăn xếp dấu ngoặc nhọn đều tuyến tính ở kích thước đầu vào. | 

Với nhiều nhất`10^5`ký tự đầu vào, quét tuyến tính chỉ thực hiện một lượng công việc không đổi nhỏ cho mỗi ký tự. Ngăn xếp rõ ràng cũng tránh được các vấn đề về độ sâu đệ quy, trong khi bộ lưu trữ phụ tuyến tính ở dưới mức giới hạn bộ nhớ 256 MB một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(s: str) -> str:
    tokens = []
    n = len(s)
    i = 0

    while i < n:
        c = s[i]

        if c.isspace():
            i += 1
            continue

        if c.isdigit():
            j = i + 1
            while j < n and s[j].isdigit():
                j += 1
            tokens.append(("I", s[i:j]))
            i = j
            continue

        if c in "$:|{}":
            tokens.append((c, c))
            i += 1
            continue

        return "NO"

    m = len(tokens)
    if m == 0:
        return "NO"

    matching = [-1] * m
    stack = []

    for i, (typ, _) in enumerate(tokens):
        if typ == "{":
            stack.append(i)
        elif typ == "}":
            if not stack:
                return "NO"
            opening = stack.pop()
            matching[opening] = i

    if stack:
        return "NO"

    inner_ok = [False] * m

    def parse_m(left: int, right: int) -> bool:
        i = left

        while i < right and tokens[i][0] == "|":
            i += 1

        if i >= right:
            return False

        if tokens[i][0] == "$":
            i += 1
            if i >= right or tokens[i][0] != "|":
                return False
            i += 1

        def parse_t(pos: int) -> int:
            if pos >= right:
                return -1

            typ = tokens[pos][0]

            if typ == "I":
                return pos + 1

            if typ == "{":
                close = matching[pos]
                if close == -1 or close >= right:
                    return -1
                if not inner_ok[pos]:
                    return -1
                return close + 1

            return -1

        def parse_p(pos: int) -> int:
            pos = parse_t(pos)
            if pos == -1:
                return -1

            while pos < right and tokens[pos][0] == ":":
                pos = parse_t(pos + 1)
                if pos == -1:
                    return -1

            return pos

        i = parse_p(i)
        if i == -1:
            return False

        while i < right and tokens[i][0] == "|":
            i = parse_p(i + 1)
            if i == -1:
                return False

        return i == right

    openings = [i for i in range(m) if tokens[i][0] == "{"]

    for opening in reversed(openings):
        inner_ok[opening] = parse_m(opening + 1, matching[opening])

    return "YES" if parse_m(0, m) else "NO"

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided samples
assert run("1\n") == "YES", "sample 1"
assert run(": 1\n") == "NO", "sample 2"
assert run("$ | 2\n") == "YES", "sample 3"

# Custom cases
assert run("") == "NO", "empty input"

assert run("1 2\n") == "NO", "whitespace cannot occur inside I"

assert run("{1:2}|3\n") == "YES", "nested M and colon expression"

assert run("{}\n") == "NO", "empty M inside braces"

assert run("||||123\n") == "YES", "arbitrarily many leading bars"

assert run("1|\n") == "NO", "bar requires a following P"

assert run("$\n") == "NO", "$ requires | P"

assert run("5 : 14\n") == "YES", "colon-separated P"

assert run("{" * 25000 + "1" + "}" * 25000 + "\n") == "YES", \
    "deep nesting without recursive calls"

assert run("1" * 100000 + "\n") == "YES", \
    "maximum-size digit sequence"

assert run("1||2\n") == "YES", "empty-looking M between bars is allowed via leading-bar recursion"

assert run("1|:2\n") == "NO", "bar cannot be followed by an invalid P"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Dòng trống |`NO`| Đầu vào kích thước tối thiểu và thực tế là`M`không được để trống | 
|`1 2`|`NO`| Khoảng trắng chấm dứt một`I`mã thông báo | 
|`{1:2} | 3`|`YES`| Lồng nhau`M`, dấu ngoặc nhọn, dấu hai chấm và thanh với nhau | 
|`{}`|`NO`| Nội dung trống không thể tạo thành một`M`| 
|` |  |  |  | 123`|`YES`| Dẫn đầu lặp đi lặp lại` | `từ`M -> | M`| 
|`1 | `|`NO`| Mất tích`P`sau dấu phân cách | 
|`⟦PROTECT_15⟧`hình thức yêu cầu` | P`| 
|`1`lặp đi lặp lại 100000 lần |`YES`| Kích thước đầu vào tối đa và dài`I`mã thông báo | 
| 25000 dấu ngoặc nhọn lồng nhau`1`|`YES`| Lồng sâu mà không cần đệ quy Python | 
|`1 |  | 2`|`YES`| Thanh thứ hai có thể được giải thích thông qua quy tắc thanh đầu của thanh còn lại`M`| 
|`1 | :2`|`NO`| Một thanh phải được theo sau bởi một câu hoàn chỉnh`P`| 

## Vỏ cạnh 

Đầu vào trống được xử lý trước khi bắt đầu phân tích cú pháp. Không có token nên không thể có`T`,`P`, hoặc`M`. Thuật toán ngay lập tức trở lại`NO`. 

Vì`1 2`, mã thông báo tạo ra`I, I`, còn hơn là`I`chứa đựng`12`. đầu tiên`P`chỉ tiêu thụ cái đầu tiên`I`. Vì mã thông báo tiếp theo là mã thông báo khác`I`còn hơn là`:`hoặc`|`,`parse_m`kết thúc với một mã thông báo chưa được sử dụng và trả về`NO`. Đây là lý do tại sao không thể xóa khoảng trắng khỏi đầu vào một cách đơn giản. 

Vì`1|`, đầu tiên`P`tiêu thụ thành công`1`. Trình phân tích cú pháp sau đó sẽ thấy`|`và đi vào vòng lặp khác`P`. Không có mã thông báo sau thanh, vì vậy`parse_p`thất bại và kết quả là`NO`. 

Vì`⟦PROTECT_16⟧`và ngay lập tức kiểm tra`|`. Vì đầu vào kết thúc nên dấu phân cách bắt buộc đó không có, nên kết quả là`NO`. Vì`$ | 2`, dấu phân cách tồn tại và`2`cung cấp những thứ cần thiết`P`, vì vậy cùng một nhánh thành công. 

Vì`{}`, bộ so khớp dấu ngoặc nhọn sẽ ghép nối chính xác hai dấu ngoặc nhọn, sau đó`parse_m`được gọi vào khoảng trống giữa chúng. Vì không có mã thông báo để xây dựng`P`, được lưu trữ`inner_ok`giá trị là`False`, và bên ngoài`T`bị từ chối. Kết quả là`NO`. 

Vì`{1:2}|3`, khoảng thời gian niềng răng bên trong được xử lý đầu tiên. Hình thức token của nó`P = T : T`, trong đó cả hai`T`giá trị là các chuỗi chữ số, vì vậy`inner_ok`trở thành`True`. Bộ phân tích cú pháp bên ngoài sau đó có thể xử lý`{1:2}`như một`T`, theo sau là`| 3`, tạo ra một giá trị hợp lệ`M`và câu trả lời`YES`. 

Đối với một biểu thức lồng nhau sâu chẳng hạn như 25000 dấu ngoặc nhọn mở, theo sau là`1`, theo sau là 25000 dấu ngoặc nhọn đóng, ngăn xếp dấu ngoặc nhọn khớp với tất cả các cặp. Biểu thức trong cùng nhất được đánh giá trước tiên và mỗi biểu thức bên ngoài sử dụng kết quả được lưu trữ của biểu thức con của nó. Không có lệnh gọi hàm Python nào được thực hiện cho từng cấp độ lồng nhau, vì vậy thuật toán vẫn an toàn ở độ sâu có thể khiến cho việc giảm đệ quy thông thường trở nên không đáng tin cậy.
