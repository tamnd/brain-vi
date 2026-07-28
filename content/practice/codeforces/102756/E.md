---
title: "CF 102756E - Chuỗi chữ tượng hình"
description: "Vấn đề mô tả một chuỗi các giá trị chữ tượng hình. Mỗi giá trị được biểu thị bằng một số nguyên và vua chỉ chấp nhận các chuỗi trong đó XOR của tất cả các giá trị bằng 0."
date: "2026-07-29T00:36:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102756
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 1"
rating: 0
weight: 102756
solve_time_s: 59
verified: true
draft: false
---

[CF 102756E - Chuỗi chữ tượng hình](https://codeforces.com/problemset/problem/102756/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một chuỗi các giá trị chữ tượng hình. Mỗi giá trị được biểu thị bằng một số nguyên và vua chỉ chấp nhận các chuỗi trong đó XOR của tất cả các giá trị bằng 0. Sylvia có thể thay thế bất kỳ phần tử nào của chuỗi bằng các giá trị mới và cô ấy muốn thay đổi càng ít vị trí càng tốt. Nhiệm vụ là tìm số lượng vị trí tối thiểu phải được thay thế để XOR cuối cùng trở thành số 0. 

Đầu vào chứa độ dài của chuỗi và giá trị nguyên được viết cho mỗi chữ tượng hình. Đầu ra là số phần tử nhỏ nhất có giá trị cần thay đổi. Việc thay thế có thể sử dụng bất kỳ số nguyên nào, vì vậy điều quan trọng duy nhất là có bao nhiêu vị trí ban đầu phải bị loại bỏ khỏi phép tính XOR. 

Độ dài chuỗi có thể lớn tới 10000, do đó, giải pháp thử tất cả các tập hợp con vị trí có thể có là không thể. Số lượng tập hợp con tăng theo cấp số nhân, đạt khoảng$2^{10000}$, vượt xa những gì có thể được xử lý. Giải pháp dự định phải sử dụng thuộc tính trực tiếp của XOR và chạy gần với thời gian tuyến tính. 

Các trường hợp phức tạp là các tình huống trong đó trình tự đã hợp lệ hoặc chỉ cần một thay đổi. Ví dụ:```
Input
4
1 1 1 1
```Đầu ra đúng là:```
0
```bởi vì$1 \oplus 1 \oplus 1 \oplus 1 = 0$. Việc triển khai bất cẩn luôn cho rằng cần có ít nhất một sự thay thế sẽ trả về câu trả lời sai. 

Một trường hợp khác là:```
Input
3
2 4 8
```Đầu ra đúng là:```
1
```Tổng XOR là$2 \oplus 4 \oplus 8 = 14$. Việc thay thế bất kỳ một giá trị nào bằng XOR của hai giá trị còn lại sẽ làm cho toàn bộ chuỗi có XOR bằng 0. Một cách tiếp cận sai lầm chỉ cho phép thay đổi giá trị thành các số hiện có có thể kết luận không chính xác rằng không có nghiệm nào tồn tại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi vị trí có thể để thay thế. Đối với mỗi bộ đã chọn, chúng ta có thể kiểm tra xem liệu các giá trị không thay đổi còn lại có thể được hoàn thành thành một chuỗi có XOR bằng 0 hay không. Cách tiếp cận này đúng vì nó xem xét mọi câu trả lời có thể, nhưng số lượng tập hợp con là$2^n$. Với$n = 10000$, thậm chí xem xét một phần rất nhỏ của những khả năng này là không thể. 

Quan sát hữu ích đến từ hành vi của XOR. Nếu XOR của chuỗi ban đầu là$x$, thay thế một phần tử luôn là đủ trừ khi$x$đã bằng không rồi. Giả sử chúng ta chọn một vị trí và loại bỏ giá trị ban đầu của nó$a$. XOR của tất cả các phần tử còn lại là$x \oplus a$. Chúng ta có thể thay thế phần tử bị loại bỏ bằng chính xác$x \oplus a$, thực hiện XOR cuối cùng:$$(x \oplus a) \oplus (x \oplus a) = 0$$bởi vì mọi số XOR với chính nó đều trở thành số 0. 

Điều này có nghĩa là câu hỏi duy nhất là liệu chuỗi đó có thỏa mãn điều kiện hay không. Nếu có thì không cần thay thế. Nếu không, một sự thay thế luôn hoạt động. Toàn bộ vấn đề giảm xuống việc tính toán XOR của tất cả các phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính XOR của mọi giá trị chữ tượng hình trong chuỗi. Giá trị cuối cùng biểu thị trạng thái hiện tại của toàn bộ thông báo, vì XOR kết hợp tất cả các phần tử thành một giá trị. 
2. Nếu XOR kết quả bằng 0, xuất ra 0. Trình tự đã đáp ứng yêu cầu của nhà vua nên không cần thay đổi vị trí. 
3. Nếu không, xuất một. Bất kỳ phần tử đơn lẻ nào cũng có thể được thay thế bằng một giá trị làm cho tổng XOR bằng 0. 

Tại sao nó hoạt động: 

Bất biến chính là XOR có thể đảo ngược. Nếu XOR hiện tại là$x$và một phần tử$a$bị loại bỏ, XOR của các phần tử còn lại là$x \oplus a$. Chọn giá trị thay thế một cách chính xác$x \oplus a$làm cho hai giá trị giống hệt nhau triệt tiêu lẫn nhau. Vì cần ít nhất một thay thế khi XOR khác 0 và một thay thế luôn là đủ, nên câu trả lời tối thiểu là chính xác một trong trường hợp đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    x = 0
    for value in arr:
        x ^= value

    if x == 0:
        print(0)
    else:
        print(1)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ đọc trình tự và giữ giá trị XOR đang chạy. Biến`x`luôn lưu trữ XOR của các phần tử được xử lý cho đến nay, điều này tránh cần bất kỳ cấu trúc dữ liệu bổ sung nào. 

Sau khi xử lý tất cả các giá trị, mã sẽ kiểm tra hai trạng thái duy nhất có thể. XOR bằng 0 có nghĩa là chuỗi đã được chấp nhận. XOR khác 0 có nghĩa là một sự thay thế là cần thiết và đủ. 

Việc triển khai không cần lưu trữ bất cứ thứ gì ngoài mảng đầu vào. Hoạt động XOR hoạt động trực tiếp trên các số nguyên Python, do đó không có mối lo ngại về tràn mặc dù các giá trị có thể lớn bằng$10^9$. Thuật toán cũng tránh mọi sự phụ thuộc vào giá trị thay thế thực tế vì bài toán chỉ yêu cầu số lần thay thế. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
Input
3
2 4 8
```| Bước | Giá trị hiện tại | Chạy XOR | 
| --- | --- | --- | 
| Bắt đầu | không | 0 | 
| Đọc 2 | 2 | 2 | 
| Đọc 4 | 4 | 6 | 
| Đọc 8 | 8 | 14 | 

XOR cuối cùng là 14, không phải bằng 0. Cần có một sự thay thế. Dấu vết này chứng tỏ rằng mọi XOR khác 0 đều có thể được sửa bằng cách sửa đổi một phần tử. 

Đối với mẫu thứ hai:```
Input
4
1 1 1 1
```| Bước | Giá trị hiện tại | Chạy XOR | 
| --- | --- | --- | 
| Bắt đầu | không | 0 | 
| Đọc 1 | 1 | 1 | 
| Đọc 1 | 1 | 0 | 
| Đọc 1 | 1 | 1 | 
| Đọc 1 | 1 | 0 | 

XOR cuối cùng đã bằng 0 nên câu trả lời là 0. Dấu vết này xác nhận rằng thuật toán xử lý chính xác các chuỗi đã hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị chữ tượng hình được xử lý một lần bằng một thao tác XOR. | 
| Không gian | O(1) | Chỉ cần giá trị XOR đang chạy sau khi đọc đầu vào. | 

Độ dài chuỗi tối đa là 10000, do đó quét tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoại trừ dung lượng lưu trữ đầu vào mà Python yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    n = int(sys.stdin.readline())
    arr = list(map(int, sys.stdin.readline().split()))

    x = 0
    for v in arr:
        x ^= v

    print(0 if x == 0 else 1)

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided samples
assert run("3\n2 4 8\n") == "1\n", "sample 1"
assert run("4\n1 1 1 1\n") == "0\n", "sample 2"

# custom cases
assert run("2\n5 5\n") == "0\n", "two equal values cancel"
assert run("2\n0 7\n") == "1\n", "single nonzero value"
assert run("5\n10 20 30 40 50\n") == "1\n", "general nonzero xor"
assert run("10000\n" + "1 " * 9999 + "1\n") == "0\n", "large all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 5 5`|`0`| Xác nhận việc hủy XOR và các chuỗi đã hợp lệ. | 
|`2 / 0 7`|`1`| Kiểm tra xem XOR khác 0 có giá trị bằng 0 vẫn cần một lần thay thế. | 
|`5 / 10 20 30 40 50`|`1`| Kiểm tra một chuỗi bình thường có tổng XOR khác 0. | 
| 10000 giá trị của`1`|`0`| Xác nhận quá trình quét tuyến tính xử lý kích thước tối đa. | 

## Vỏ cạnh 

Đối với trường hợp đã hợp lệ:```
Input
4
1 1 1 1
```Thuật toán bắt đầu bằng XOR bằng 0 và áp dụng XOR với mỗi giá trị. Sau bốn thao tác, giá trị trở về 0. Vì XOR cuối cùng bằng 0 nên thuật toán trả về 0 mà không cần thực hiện bất kỳ sự thay thế nào. 

Đối với trường hợp thay thế duy nhất:```
Input
3
2 4 8
```XOR được tính toán là 14. Nếu phần tử đầu tiên được thay thế, hai phần tử còn lại có XOR$4 \oplus 8 = 12$. Thay thế phần tử đầu tiên bằng 12 sẽ cho:$$12 \oplus 4 \oplus 8 = 0$$Thuật toán không cần tìm giá trị thay thế này vì bằng chứng đảm bảo rằng giá trị đó tồn tại bất cứ khi nào tổng XOR khác 0. 

Đối với một chuỗi chứa số 0:```
Input
2
0 7
```Tổng XOR là 7. Thay giá trị đầu tiên bằng 7 sẽ tạo ra$7 \oplus 7 = 0$. Thuật toán vẫn trả về một vì chuỗi ban đầu không hợp lệ. 

Đối với đầu vào có kích thước tối đa, thuật toán thực hiện chính xác một thao tác XOR cho mỗi phần tử. Không có vòng lặp lồng nhau hoặc lệnh gọi đệ quy, do đó thời gian chạy tăng tuyến tính và vẫn an toàn đối với chuỗi lớn nhất được phép.
