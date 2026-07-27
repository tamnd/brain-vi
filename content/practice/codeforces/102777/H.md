---
title: "CF 102777H - \u041f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u043d\u043e\u0441\u0442\u044c \u0410\u0441\u043b\u0430\u043d\u0430"
description: "Chúng ta cần tìm giá trị của dãy ở vị trí rất lớn. Hai phần tử đầu tiên là cố định và mọi phần tử tiếp theo được tạo từ hai phần tử trước đó cộng với một giá trị phụ thuộc vào chỉ mục hiện tại."
date: "2026-07-27T20:34:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102777
codeforces_index: "H"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102777
solve_time_s: 63
verified: true
draft: false
---

[CF 102777H - \u041f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u043d\u043e\u0441\u0442\u044c \u0410\u0441\u043b\u0430\u043d\u0430](https://codeforces.com/problemset/problem/102777/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tìm giá trị của dãy ở vị trí rất lớn. Hai phần tử đầu tiên là cố định và mọi phần tử tiếp theo được tạo từ hai phần tử trước đó cộng với một giá trị phụ thuộc vào chỉ mục hiện tại. Câu trả lời bắt buộc là phần tử ở vị trí`n`, in modulo`10^9 + 7`. 

Khó khăn đến từ quy mô của`n`. Từ`n`có thể đạt được`10^9`, một thuật toán tính toán từng phần tử trước đó sẽ yêu cầu khoảng một tỷ lần chuyển đổi, quá chậm trong giới hạn một giây. Chúng ta cần giảm số lượng thao tác tùy thuộc vào`n`tùy thuộc vào số lượng bit trong`n`, gợi ý một cách tiếp cận logarit. 

Các trường hợp ranh giới là hai vị trí đầu tiên. Vì`n = 1`, câu trả lời là`1`, và cho`n = 2`, câu trả lời cũng là`1`. Một giải pháp bắt đầu tính lũy thừa ma trận ngay lập tức từ`n - 2`không xử lý các giá trị này sẽ tạo ra trạng thái bắt đầu không hợp lệ. Ví dụ, đầu vào`1`phải sản xuất`1`, trong khi việc triển khai bất cẩn có thể cố gắng nâng ma trận chuyển tiếp lên lũy thừa âm. 

Một sai lầm dễ mắc phải khác là quên rằng phép truy toán chứa chính chỉ mục đó. Đối với đầu vào`3`, phép tính là`2 * 1 + 3 * 1 + 3`, cho`8`. Xử lý phép truy toán giống như phép truy toán tuyến tính kiểu Fibonacci bình thường và bỏ qua giá trị chỉ mục được thêm vào sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp tuân theo định nghĩa theo nghĩa đen. Chúng tôi lưu trữ hai giá trị cuối cùng và liên tục tính toán giá trị tiếp theo cho đến khi đạt được`n`. Điều này đúng vì mỗi lần chuyển đổi sử dụng chính xác hai giá trị trước đó và chỉ mục hiện tại, vì vậy sau`n - 2`chuyển đổi giá trị được lưu trữ là câu trả lời. Vấn đề là số lần chuyển tiếp. Vì`n = 10^9`, thuật toán thực hiện khoảng một tỷ lần lặp, vượt xa những gì thực tế. 

Quan sát quan trọng là sự tái phát gần như là một sự tái phát tuyến tính. Phần bổ sung duy nhất được thêm vào`n`và một giá trị thay đổi tuyến tính có thể được lưu trữ như một phần của trạng thái. Chúng ta có thể biểu thị tình huống hiện tại bằng bốn số: giá trị chuỗi hiện tại, giá trị chuỗi trước đó, chỉ số hiện tại và hằng số`1`. 

Đối với một tiểu bang`[F_k, F_{k-1}, k, 1]`, trạng thái tiếp theo là`[F_{k+1}, F_k, k+1, 1]`. Phép biến đổi này là phép nhân ma trận cố định. Khi phép truy toán trở thành phép nhân lặp lại với một ma trận không đổi, phép lũy thừa nhị phân cho phép chúng ta nhảy từ chỉ số`2`lập chỉ mục`n`theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trực tiếp hai vị trí ban đầu. Nếu như`n`là`1`hoặc`2`, trở lại`1`vì sự lặp lại chỉ được xác định sau các giá trị này. 
2. Xây dựng ma trận chuyển tiếp. Nhà nước là`[F_k, F_{k-1}, k, 1]`và trạng thái tiếp theo được tạo ra bởi ma trận```
[2 3 1 1]
[1 0 0 0]
[0 0 1 1]
[0 0 0 1]
```Hàng đầu tiên tạo giá trị chuỗi tiếp theo, hàng thứ hai chuyển giá trị trước đó về phía trước và hai hàng cuối cùng duy trì chỉ mục hiện tại và số hạng không đổi. 

1. Bắt đầu từ trạng thái lúc`k = 2`, đó là`[1, 1, 2, 1]`. 
2. Nâng ma trận chuyển tiếp lên lũy thừa`n - 2`sử dụng lũy ​​thừa nhị phân. Mỗi phép nhân áp dụng nhiều phép chuyển đổi cùng một lúc và việc bình phương ma trận sẽ giảm số phép toán cần thiết về thời gian logarit. 
3. Nhân ma trận được cấp với trạng thái bắt đầu. Thành phần đầu tiên của vectơ kết quả là`F_n`, đó là câu trả lời bắt buộc. 

Tại sao nó hoạt động: trạng thái luôn lưu trữ chính xác thông tin cần thiết để thực hiện thêm một bước lặp lại. Áp dụng ma trận chuyển tiếp khi thay đổi trạng thái từ chỉ mục`k`lập chỉ mục`k + 1`. Áp dụng nó`n - 2`do đó, thời gian di chuyển trạng thái đã biết ở chỉ số`2`đến trạng thái mong muốn tại chỉ mục`n`. Phép lũy thừa ma trận chỉ thay đổi cách nhóm các ứng dụng lặp lại đó chứ không phải phép biến đổi được áp dụng, do đó thành phần đầu tiên thu được luôn là giá trị chuỗi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def multiply(a, b):
    n = len(a)
    m = len(b[0])
    k = len(b)
    res = [[0] * m for _ in range(n)]
    for i in range(n):
        for j in range(m):
            s = 0
            for x in range(k):
                s += a[i][x] * b[x][j]
            res[i][j] = s % MOD
    return res

def power_matrix(a, e):
    n = len(a)
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1
    while e:
        if e & 1:
            res = multiply(res, a)
        a = multiply(a, a)
        e >>= 1
    return res

def solve():
    n = int(input())
    if n <= 2:
        print(1)
        return

    transition = [
        [2, 3, 1, 1],
        [1, 0, 0, 0],
        [0, 0, 1, 1],
        [0, 0, 0, 1]
    ]

    state = [
        [1],
        [1],
        [2],
        [1]
    ]

    result = multiply(power_matrix(transition, n - 2), state)
    print(result[0][0])

if __name__ == "__main__":
    solve()
```các`multiply`hàm xử lý phép nhân các ma trận nhỏ trong khi giảm mọi giá trị theo modulo`10^9 + 7`. Vì kích thước ma trận không bao giờ thay đổi từ`4 x 4`, hoạt động này có kích thước không đổi. 

các`power_matrix`hàm là lũy thừa nhị phân tiêu chuẩn. Thay vì áp dụng quá trình chuyển đổi`n - 2`nhiều lần, nó liên tục bình phương ma trận chuyển tiếp và sử dụng các bit đã đặt của`n - 2`để lựa chọn những quyền lực cần thiết. 

Vectơ bắt đầu đại diện cho phần tử thứ hai của chuỗi. Thành phần thứ ba của nó là`2`, vì quá trình chuyển đổi cần biết chỉ mục hiện tại để thêm giá trị chỉ mục tiếp theo. Việc xử lý đặc biệt của`n <= 2`ngăn chặn các giá trị số mũ không hợp lệ và tránh các công việc ma trận không cần thiết. 

## Ví dụ đã hoạt động 

cho`n = 3`, ma trận được áp dụng một lần. 

| Bước | Chỉ số hiện tại | Tiểu bang | 
| --- | --- | --- | 
| Bắt đầu | 2 |`[1, 1, 2, 1]`| 
| Áp dụng chuyển tiếp | 3 |`[8, 1, 3, 1]`| 

Thành phần đầu tiên trở thành`8`, khớp với phép tính truy hồi`2 * 1 + 3 * 1 + 3`. 

Vì`n = 5`, ma trận được áp dụng ba lần. 

| Bước | Chỉ số hiện tại | Tiểu bang | 
| --- | --- | --- | 
| Bắt đầu | 2 |`[1, 1, 2, 1]`| 
| Áp dụng chuyển tiếp | 3 |`[8, 1, 3, 1]`| 
| Áp dụng chuyển tiếp | 4 |`[23, 8, 4, 1]`| 
| Áp dụng chuyển tiếp | 5 |`[75, 23, 5, 1]`| 

Dấu vết cho thấy hai thành phần đầu tiên luôn chứa các giá trị chuỗi liên tiếp, trong khi thành phần chỉ số tăng chính xác theo mỗi lần chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Phép lũy thừa nhị phân thực hiện logarit nhiều phép bình phương và phép nhân ma trận. | 
| Không gian | O(1) | Chỉ một số lượng cố định 4 x 4 ma trận và vectơ được lưu trữ. | 

Đầu vào lớn nhất chỉ cần khoảng 30 bước lũy thừa vì`log2(10^9)`là nhỏ. Kích thước ma trận không đổi giúp giải pháp hoạt động tốt trong giới hạn bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# The expected values are produced by the same recurrence definition.
assert run("1\n") == "1\n", "minimum position"
assert run("2\n") == "1\n", "second base position"
assert run("3\n") == "8\n", "first recurrence step"
assert run("5\n") == "75\n", "several recurrence steps"

# Maximum-size smoke test: checks that the logarithmic solution handles the limit.
assert len(run("1000000000\n").strip()) <= 10, "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Xử lý chỉ mục nhỏ nhất có thể. | 
|`2`|`1`| Xử lý giá trị cơ sở thứ hai. | 
|`3`|`8`| Kiểm tra lần sử dụng đầu tiên của sự tái phát. | 
|`5`|`75`| Kiểm tra một số chuyển tiếp theo trình tự. | 
|`1000000000`| Mười chữ số trở xuống modulo`10^9 + 7`| Xác nhận rằng thuật toán chia tỷ lệ đến chỉ số tối đa. | 

## Vỏ cạnh 

Đối với đầu vào`1`, thuật toán trả về ngay trước khi xây dựng ma trận. Câu trả lời là giá trị chuỗi đầu tiên được xác định trước, do đó không cần chuyển đổi. 

Đối với đầu vào`2`, lợi nhuận trực tiếp tương tự được sử dụng. Việc bắt đầu lũy thừa ma trận ở đây sẽ yêu cầu nâng ma trận lên lũy thừa 0, điều này có thể thực hiện được về mặt toán học, nhưng việc xử lý rõ ràng sẽ giữ cho hành vi ranh giới rõ ràng và tránh những công việc không cần thiết. 

Đối với đầu vào`3`, thuật toán thực hiện đúng một lần chuyển đổi từ`[1, 1, 2, 1]`. Thành phần đầu tiên thu được là`8`, chứng tỏ rằng thuật ngữ chỉ mục bổ sung được đưa vào một cách chính xác. 

Đối với đầu vào`1000000000`, thuật toán không bao giờ tạo ra tất cả các giá trị chuỗi trước đó. Nó chỉ thực hiện khoảng ba mươi bước bình phương ma trận, do đó chỉ số lớn ảnh hưởng đến số lượng bit được xử lý thay vì số phần tử chuỗi được tạo ra. 

Tôi cũng có thể điều chỉnh bài xã luận này theo định dạng kiểu Codeforces ngắn hơn nếu bạn muốn nội dung nào đó gần giống với những gì sẽ được xuất bản trong một bài xã luận chính thức của cuộc thi.
