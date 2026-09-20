---
title: "CF 104763A - Sứa Nghệ Thuật"
description: "Đầu vào là một số nguyên N, xác định kích thước của bản vẽ văn bản về một con sứa. Bức tranh có hai phần riêng biệt. Phần thân chiếm N hàng đầu tiên. Mỗi hàng nội dung chứa chính xác 2N - 1 ký tự 'J' liên tiếp."
date: "2026-06-28T21:49:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104763
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 11-03-23 Div. 2 (Beginner)"
rating: 0
weight: 104763
solve_time_s: 135
verified: true
draft: false
---

[CF 104763A - Nghệ thuật sứa](https://codeforces.com/problemset/problem/104763/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là một số nguyên duy nhất`N`, xác định kích thước của bản vẽ văn bản về một con sứa. 

Bức tranh có hai phần riêng biệt. Cơ thể chiếm vị trí đầu tiên`N`hàng. Mỗi hàng nội dung chứa chính xác`2N - 1`liên tiếp`'J'`nhân vật. Bên dưới cơ thể có các xúc tu, cũng chiếm giữ`N`hàng. Mỗi hàng xúc tu chứa chính xác`N` `'S'`các ký tự được phân tách bằng dấu cách đơn, do đó hàng có dạng`"S S S ... S"`. 

Những hạn chế là rất nhỏ. Ngay cả ở giá trị tối đa của`N = 100`, đầu ra chỉ chứa`2N = 200`dòng và mỗi dòng nhiều nhất là`199`ký tự dài. Tổng số ký tự được in chỉ vào khoảng hai mươi nghìn, do đó, bất kỳ cấu trúc đơn giản nào của các chuỗi cần thiết đều dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

Khó khăn chính là định dạng đầu ra chính xác như được chỉ định. Chiều rộng cơ thể phải là`2N - 1`, không`2N`và các xúc tu chỉ được chứa khoảng trống giữa các`'S'`ký tự, không bao giờ sau ký tự cuối cùng. 

Một sai lầm dễ dàng xuất hiện khi`N = 1`. 

đầu vào:```
1
```Đầu ra đúng là```
J
S
```Việc thực hiện bất cẩn luôn chèn khoảng trống giữa các xúc tu mà không tính đến việc chỉ có một xúc tu có thể vô tình in ra`"S "`với một khoảng trống ở cuối. 

Một lỗi định dạng phổ biến khác là tính toán chiều rộng nội dung không chính xác. 

đầu vào:```
2
```Đầu ra đúng là```
JJJ
JJJ
S S
S S
```In ấn`2N = 4` `'J'`ký tự thay vì`2N - 1 = 3`tạo ra một cơ thể không chính xác. 

Cạm bẫy định dạng cuối cùng là để lại một khoảng trống ở cuối sau xúc tu cuối cùng. 

đầu vào:```
3
```Hàng xúc tu đúng là```
S S S
```In ấn```
S S S
```trông giống như một đầu đọc của con người nhưng không khớp chính xác với đầu ra được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xây dựng mọi dòng đầu ra chính xác như nó sẽ xuất hiện và in nó. Đối với phần thân, chúng ta tạo một chuỗi chứa`2N - 1` `'J'`ký tự và in nó`N`lần. Đối với các xúc tu, chúng ta tạo một chuỗi bằng cách nối`N`bản sao của`"S"`với các khoảng trắng đơn và in nó`N`lần. 

Vì bản thân vấn đề chỉ yêu cầu chúng ta tạo ra hình ảnh nên việc xây dựng trực tiếp này đã là tối ưu. Ngay cả trong trường hợp xấu nhất, chúng tôi chỉ tạo ra khoảng hai mươi nghìn ký tự, điều này không đáng kể đối với phần cứng hiện đại. 

Không có tối ưu hóa ẩn hoặc thuật toán nâng cao. Điều quan trọng là mỗi hàng trên cơ thể đều giống hệt nhau và mỗi hàng xúc tu đều giống hệt nhau. Thay vì phải xây dựng lại cùng một chuỗi nhiều lần từng ký tự, chúng ta có thể tạo từng hàng riêng biệt một lần và sử dụng lại nó. Điều này giúp việc thực hiện đơn giản đồng thời tránh được những công việc không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Đã chấp nhận | 
| Tối ưu | O(N2) | O(N) | Đã chấp nhận | 

Độ phức tạp bị chi phối bởi số lượng ký tự phải được in, do đó, không có thuật toán tiệm cận nào có thể hoạt động tốt hơn kích thước đầu ra. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`N`. 
2. Xây dựng hàng nội dung như`'J' * (2 * N - 1)`. Điều này tạo ra chính xác chiều rộng cần thiết cho mỗi hàng nội dung. 
3. In chính xác hàng nội dung`N`lần. Vì mọi hàng nội dung đều giống hệt nhau nên việc sử dụng lại cùng một chuỗi là đủ.
 4. Xây dựng hàng xúc tu bằng cách sử dụng`' '.join(['S'] * N)`. Việc kết hợp với các khoảng trắng sẽ tự động đặt chính xác một khoảng trống giữa các xúc tu liền kề và tránh một khoảng trống ở cuối. 
5. In chính xác hàng xúc tu`N`lần. Mỗi hàng xúc tu đều có hình dáng giống nhau nên có thể tái sử dụng cùng một chuỗi. 

### Tại sao nó hoạt động 

Hình ảnh được yêu cầu bao gồm hai phần hình chữ nhật. Mỗi hàng nội dung phải giống hệt nhau, chứa chính xác`2N - 1`liên tiếp`'J'`nhân vật. Mỗi hàng xúc tu cũng phải giống hệt nhau, chứa`N` `'S'`các ký tự cách nhau bởi dấu cách đơn. Thuật toán xây dựng một đại diện chính xác cho mỗi phần và in mỗi phần theo số lần yêu cầu. Vì mọi hàng được tạo đều khớp chính xác với thông số kỹ thuật nên bản vẽ hoàn chỉnh là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

body = "J" * (2 * n - 1)
tentacles = " ".join(["S"] * n)

for _ in range(n):
    print(body)

for _ in range(n):
    print(tentacles)
```Chương trình bắt đầu bằng cách đọc giá trị đầu vào duy nhất. 

Chuỗi được xây dựng đầu tiên là hàng nội dung. Nhân một chuỗi với`2 * n - 1`tạo ra chiều rộng chính xác cần thiết. Tính toán điều này một lần sẽ tránh việc xây dựng lại cùng một hàng nhiều lần. 

Hàng xúc tu được tạo bằng`" ".join(["S"] * n)`. Điều này an toàn hơn việc thêm khoảng trắng theo cách thủ công vì`join`đảm bảo có chính xác một khoảng cách giữa liền kề`'S'`ký tự và không có dấu cách ở cuối dòng. 

Hai vòng tương ứng trực tiếp với hai phần của bản vẽ. Vòng lặp đầu tiên in các hàng cơ thể, trong khi vòng lặp thứ hai in các hàng xúc tu. Vì vấn đề chỉ chứa một trường hợp thử nghiệm nên không cần vòng lặp bên ngoài bổ sung. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
```| Bước |`N`| Hàng đã xây dựng | Sản lượng sản xuất | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 3 | - | - | 
| Xây dựng cơ thể | 3 |`JJJJJ`| - | 
| Thân in | 3 |`JJJJJ`| 3 hàng thân | 
| Xây dựng xúc tu | 3 |`S S S`| - | 
| In xúc tu | 3 |`S S S`| 3 hàng xúc tu | 

Chiều rộng cơ thể là`2 × 3 - 1 = 5`, sản xuất`JJJJJ`. Hàng xúc tu chứa ba`'S'`các ký tự cách nhau bởi dấu cách đơn. Mỗi hàng được in phù hợp với định dạng được yêu cầu. 

### Mẫu 2 

đầu vào:```
6
```| Bước |`N`| Hàng đã xây dựng | Sản lượng sản xuất | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 6 | - | - | 
| Xây dựng cơ thể | 6 |`JJJJJJJJJJJ`| - | 
| Thân in | 6 |`JJJJJJJJJJJ`| 6 hàng thân | 
| Xây dựng xúc tu | 6 |`S S S S S S`| - | 
| In xúc tu | 6 |`S S S S S S`| 6 hàng xúc tu | 

Ví dụ này xác nhận rằng các chuỗi giống nhau được tính toán trước có thể được sử dụng lại bất kể giá trị của`N`. Chiều rộng cơ thể trở thành`11`và mỗi hàng xúc tu có sáu xúc tu với khoảng cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) | Việc in từng ký tự chiếm ưu thế trong thời gian chạy. | 
| Không gian | O(N) | Hai chuỗi đầu ra có độ dài tỷ lệ thuận với`N`được lưu trữ. | 

Từ`N`nhiều nhất là`100`, tổng kích thước đầu ra là rất nhỏ. Thuật toán dễ dàng đáp ứng cả giới hạn thời gian một giây và bộ nhớ khả dụng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    body = "J" * (2 * n - 1)
    tentacles = " ".join(["S"] * n)

    for _ in range(n):
        print(body)
    for _ in range(n):
        print(tentacles)

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

# provided samples
assert run("3\n") == (
    "JJJJJ\n"
    "JJJJJ\n"
    "JJJJJ\n"
    "S S S\n"
    "S S S\n"
    "S S S\n"
), "sample 1"

assert run("6\n") == (
    "JJJJJJJJJJJ\n"
    "JJJJJJJJJJJ\n"
    "JJJJJJJJJJJ\n"
    "JJJJJJJJJJJ\n"
    "JJJJJJJJJJJ\n"
    "JJJJJJJJJJJ\n"
    "S S S S S S\n"
    "S S S S S S\n"
    "S S S S S S\n"
    "S S S S S S\n"
    "S S S S S S\n"
    "S S S S S S\n"
), "sample 2"

# custom cases
assert run("1\n") == (
    "J\n"
    "S\n"
), "minimum size"

assert run("2\n") == (
    "JJJ\n"
    "JJJ\n"
    "S S\n"
    "S S\n"
), "small even size"

out = run("100\n")
lines = out.strip().split("\n")
assert len(lines) == 200, "correct number of rows"
assert all(line == "J" * 199 for line in lines[:100]), "body width"
assert all(line == " ".join(["S"] * 100) for line in lines[100:]), "tentacles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`| Một`J`hàng và một`S`hàng | Kích thước đầu vào tối thiểu | 
|`2`| Chiều rộng cơ thể của`3`và hai hàng xúc tu | Tính toán đúng`2N - 1`| 
|`100`| 200 hàng được định dạng chính xác | Hạn chế tối đa và kích thước đầu ra | 
|`3`|`S S S`không có dấu cách | Khoảng cách chính xác giữa các xúc tu | 

## Vỏ cạnh 

Khi nào`N = 1`, thuật toán tính hàng nội dung là`"J" * 1`, sản xuất một đĩa đơn`'J'`. Hàng xúc tu được tạo bằng cách nối một danh sách một phần tử, điều này tạo ra một cách tự nhiên`"S"`không có không gian thừa. 

đầu vào:```
1
```Bản dựng thực thi`body = "J"`Và`tentacles = "S"`, sau đó in từng cái một lần. 

Đầu ra:```
J
S
```Khi`N = 2`, việc tính chiều rộng cơ thể trở thành`2 × 2 - 1 = 3`. Cấu trúc thuật toán`"JJJ"`thay vì`"JJJJ"`, tránh được lỗi phổ biến nhất do một lỗi. 

đầu vào:```
2
```Bản dựng thực thi`body = "JJJ"`Và`tentacles = "S S"`. 

Đầu ra:```
JJJ
JJJ
S S
S S
```Vì`N = 3`, hàng xúc tu được tạo bằng cách sử dụng`join`, do đó khoảng trống chỉ xuất hiện giữa các xúc tu liền kề. 

đầu vào:```
3
```Thuật toán tính toán`tentacles = "S S S"`và in nó ba lần. Không có dấu cách nào được thêm vào, khớp chính xác với đầu ra được yêu cầu.
