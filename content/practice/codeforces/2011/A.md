---
title: "CF 2011A - Giải quyết vấn đề"
description: "Jane có một danh sách các vấn đề với những khó khăn đã biết. Vấn đề cuối cùng trong danh sách thực sự khó hơn mọi vấn đề trước đó. Cấp độ kỹ năng của cô ấy là một số nguyên $x$."
date: "2026-06-08T13:09:28+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 2011
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 11"
rating: 0
weight: 2011
solve_time_s: 108
verified: true
draft: false
---

[CF 2011A - Giải quyết vấn đề](https://codeforces.com/problemset/problem/2011/A) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Jane có một danh sách các vấn đề với những khó khăn đã biết. Vấn đề cuối cùng trong danh sách thực sự khó hơn mọi vấn đề trước đó. 

Trình độ kỹ năng của cô ấy là một số nguyên$x$. Cô ấy có thể giải quyết mọi vấn đề khó khăn nhất$x$và không thể giải được bất kỳ bài toán nào có độ khó lớn hơn$x$. 

Chúng ta được biết rằng cô ấy đã giải quyết được mọi vấn đề ngoại trừ vấn đề cuối cùng. Từ thông tin này, chúng ta phải xác định xem liệu trình độ kỹ năng của cô ấy có thể được xác định chính xác hay không. Nếu có chính xác một giá trị có thể có của$x$, chúng tôi in nó. Nếu một số giá trị của$x$thỏa mãn các quan sát, chúng tôi in`Ambiguous`. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chứa danh sách các khó khăn. Đầu ra của mỗi trường hợp thử nghiệm là giá trị kỹ năng duy nhất hoặc từ`Ambiguous`. 

Những hạn chế là rất nhỏ. Có tối đa 1000 trường hợp kiểm tra, mỗi trường hợp chứa tối đa 50 độ khó và mỗi độ khó tối đa là 50. Ngay cả một cuộc tìm kiếm vũ phu trên tất cả các giá trị kỹ năng có thể có cũng sẽ đủ nhanh. Thách thức không phải là hiệu quả mà là hiểu được thông tin mà các quan sát thực sự cung cấp. 

Sai lầm phổ biến nhất là cho rằng kỹ năng của Jane phải bằng độ khó tối đa trong số các bài toán được giải. Điều đó chỉ đúng trong một số trường hợp. 

Coi như:```
8
8 8 5 3 4 6 8 12
```Jane giải được tất cả các bài toán có độ khó 8 trở xuống, nhưng không đạt ở độ khó 12. Kỹ năng của cô ấy có thể là 8, 9, 10 hoặc 11. Câu trả lời đúng là:```
Ambiguous
```Một trường hợp tinh tế khác là khi tất cả các vấn đề được giải quyết đều có độ khó như nhau.```
4
3 3 3 4
```Cô đã giải được độ khó 3 và thất bại ở độ khó 4. Vì kỹ năng là một số nguyên nên khả năng duy nhất là$x=3$. Câu trả lời đúng là:```
3
```Một giải pháp bất cẩn luôn in ra độ khó được giải quyết tối đa sẽ có tác dụng ở đây, nhưng vì lý do sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi giá trị kỹ năng số nguyên có thể$x$, kiểm tra xem nó có phù hợp với các quan sát hay không. 

Giá trị kỹ năng là hợp lệ nếu mọi vấn đề được giải quyết đều gặp khó khăn tối đa$x$và vấn đề chưa được giải quyết cuối cùng có độ khó lớn hơn$x$. 

Vì tất cả các độ khó nhiều nhất là 50 nên chúng ta có thể kiểm tra mọi$x$từ 0 đến 50 và đếm xem có bao nhiêu đáp ứng các điều kiện này. Nếu chính xác một cái hoạt động, hãy in nó. Nếu không thì in`Ambiguous`. 

Phương pháp này đúng vì nó trực tiếp thực hiện định nghĩa về cấp độ kỹ năng hợp lệ. Độ phức tạp của nó là$O(50n)$cho mỗi trường hợp thử nghiệm, đủ nhanh. 

Quan sát quan trọng là chúng ta thực sự không cần phải liệt kê bất cứ điều gì. 

Cho phép$$m = \max(d_1,d_2,\ldots,d_{n-1})$$là khó khăn lớn nhất trong số các bài toán đã được giải. 

Vì Jane đã giải quyết được tất cả các vấn đề trước đó nên kỹ năng của cô ấy phải thỏa mãn$$x \ge m.$$Vì cô ấy đã thất bại ở bài toán khó cuối cùng$d_n$,$$x < d_n.$$Do đó mọi giá trị kỹ năng hợp lệ đều nằm trong khoảng$$m \le x \le d_n-1.$$Số giá trị kỹ năng có thể có là$$(d_n-1)-m+1 = d_n-m.$$Nếu như$d_n-m=1$, có chính xác một giá trị có thể, đó là$m$. 

Nếu như$d_n-m>1$, có thể có nhiều giá trị kỹ năng và câu trả lời là`Ambiguous`. 

Toàn bộ vấn đề được rút gọn thành việc so sánh độ khó được giải lớn nhất với độ khó cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(50n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc những khó khăn của test case hiện tại. 
2. Tính độ khó lớn nhất trong số các câu đầu tiên$n-1$vấn đề. Gọi giá trị này`mx`. 

Jane đã giải quyết được tất cả những vấn đề này, vì vậy bất kỳ kỹ năng hợp lệ nào ít nhất cũng phải`mx`. 
3. Hãy để`last`là độ khó của bài toán cuối cùng. 

Jane không thể giải quyết vấn đề này, vì vậy mọi kỹ năng hợp lệ đều phải nhỏ hơn`last`. 
4. Các giá trị kỹ năng hợp lệ là tất cả các số nguyên trong phạm vi`[mx, last-1]`. 
5. Nếu`last - mx == 1`, phạm vi chứa chính xác một số nguyên, cụ thể là`mx`. In`mx`. 
6. Nếu không thì phạm vi chứa nhiều số nguyên. In`Ambiguous`. 

### Tại sao nó hoạt động 

Mọi vấn đề được giải quyết đều đặt ra giới hạn dưới cho cấp độ kỹ năng và giới hạn mạnh nhất đó là độ khó được giải tối đa`mx`. 

Bài toán cuối cùng thất bại áp đặt một giới hạn trên, cụ thể là$x < \text{last}$. 

Không có thông tin khác quan trọng. Bất kỳ số nguyên nào giữa các giới hạn này đều thỏa mãn mọi quan sát. Như vậy tập hợp các kỹ năng khả thi chính xác là khoảng$[mx, \text{last}-1]$. 

Nếu khoảng đó chứa một số nguyên thì kỹ năng được xác định duy nhất. Nếu nó chứa nhiều hơn một số nguyên thì một số kỹ năng phù hợp với bằng chứng và câu trả lời phải là`Ambiguous`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    d = list(map(int, input().split()))

    mx = max(d[:-1])
    last = d[-1]

    if last - mx == 1:
        print(mx)
    else:
        print("Ambiguous")
```Giải pháp tuân theo đặc tính toán học trực tiếp. 

Biến`mx`lưu trữ khó khăn lớn nhất trong số tất cả các vấn đề được giải quyết. Bất kỳ kỹ năng hợp lệ nào ít nhất phải có giá trị này. 

Biến`last`lưu trữ độ khó của vấn đề duy nhất chưa được giải quyết. Mọi kỹ năng hợp lệ phải nhỏ hơn giá trị này. 

Do đó, khoảng thời gian của các kỹ năng khả thi là`[mx, last - 1]`. Khoảng chứa đúng một số nguyên khi`last - mx == 1`. Trong tình huống đó chúng tôi in`mx`. Nếu không thì có nhiều kỹ năng có thể có và chúng tôi in`Ambiguous`. 

Không có điều kiện biên phức tạp nào ngoài việc nhớ rằng bài toán cuối cùng bị loại khỏi phép tính cực đại. sử dụng`max(d[:-1])`xử lý việc này một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| Bước | Giá trị | 
| --- | --- | 
|`mx`| 4 | 
|`last`| 5 | 
| Kỹ năng hợp lệ | {4} | 
| Đầu ra | 4 | 

Khoảng khả thi là`[4, 4]`, chứa chính xác một giá trị. Kỹ năng của Jane được xác định một cách độc đáo. 

### Ví dụ 2 

đầu vào:```
8
8 8 5 3 4 6 8 12
```| Bước | Giá trị | 
| --- | --- | 
|`mx`| 8 | 
|`last`| 12 | 
| Kỹ năng hợp lệ | {8, 9, 10, 11} | 
| Đầu ra | mơ hồ | 

Một số giá trị kỹ năng đáp ứng tất cả các quan sát. Không có gì phân biệt giữa chúng, vì vậy câu trả lời là`Ambiguous`. 

Những ví dụ này minh họa tính bất biến trung tâm: mọi kỹ năng hợp lệ phải nằm giữa độ khó được giải quyết lớn nhất và độ khó bị thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét để tìm giá trị lớn nhất trong số n-1 phần tử đầu tiên | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với$n \le 50$, thời gian chạy không đáng kể. Thậm chí trên tất cả 1000 trường hợp thử nghiệm, giải pháp chỉ thực hiện vài chục nghìn thao tác và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        d = list(map(int, input().split()))

        mx = max(d[:-1])
        last = d[-1]

        if last - mx == 1:
            ans.append(str(mx))
        else:
            ans.append("Ambiguous")

    print("\n".join(ans))

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

# provided sample
assert run(
"""3
5
1 2 3 4 5
8
8 8 5 3 4 6 8 12
4
3 3 3 4
"""
) == "4\nAmbiguous\n3\n"

# minimum size
assert run(
"""1
2
1 2
"""
) == "1\n"

# ambiguous interval of length 3
assert run(
"""1
2
1 4
"""
) == "Ambiguous\n"

# all solved difficulties equal
assert run(
"""1
5
7 7 7 7 8
"""
) == "7\n"

# boundary value near maximum difficulty
assert run(
"""1
4
50 50 50 51
"""
) == "50\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`1`| Kích thước hợp lệ tối thiểu | 
|`1 4`|`Ambiguous`| Nhiều kỹ năng khả thi | 
|`7 7 7 7 8`|`7`| Mọi khó khăn đều được giải quyết bằng nhau | 
|`50 50 50 51`|`50`| Giá trị biên gần độ khó tối đa | 

## Vỏ cạnh 

### Nhiều kỹ năng có thể 

đầu vào:```
1
2
3 7
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
|`mx`| 3 | 
|`last`| 7 | 

Các kỹ năng hợp lệ là 3, 4, 5 và 6. Vì`last - mx = 4`, có thể có nhiều hơn một kỹ năng. Đầu ra là:```
Ambiguous
```Một giải pháp luôn in`mx`sẽ sai. 

### Chính xác là một kỹ năng có thể 

đầu vào:```
1
4
3 3 3 4
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
|`mx`| 3 | 
|`last`| 4 | 

Khoảng khả thi là`[3, 3]`, chứa đúng một giá trị. Đầu ra là:```
3
```Điều này khẳng định rằng những khó khăn được giải quyết lặp đi lặp lại không tự nó tạo ra sự mơ hồ. 

### Khoảng cách lớn sau bài toán khó nhất được giải quyết 

đầu vào:```
1
5
1 2 3 4 10
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
|`mx`| 4 | 
|`last`| 10 | 

Các kỹ năng có thể có là 4, 5, 6, 7, 8 và 9. Kết quả đầu ra là:```
Ambiguous
```Khoảng cách giữa vấn đề được giải quyết khó nhất và vấn đề không thành công sẽ trực tiếp xác định số lượng giá trị kỹ năng còn lại có thể. 

### Khoảng thời gian khả thi tối thiểu 

đầu vào:```
1
3
5 2 6
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
|`mx`| 5 | 
|`last`| 6 | 

Khoảng thời gian là`[5, 5]`, vậy đáp án là:```
5
```Điều này thể hiện điều kiện chính xác cho tính duy nhất: độ khó của bài toán không thành công phải lớn hơn chính xác một độ khó so với bài toán được giải khó nhất.
