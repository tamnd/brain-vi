---
title: "CF 1844D - Hàng Chính"
description: "Chúng ta cần xây dựng một chuỗi có độ dài n bằng cách sử dụng càng ít chữ cái viết thường riêng biệt càng tốt. Điều hấp dẫn là chuỗi phải duy trì hợp lệ đối với mọi hình dạng lưới có thể có mà việc truyền tải chính hàng tạo ra chuỗi đó."
date: "2026-06-09T06:02:06+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "number-theory", "strings"]
categories: ["algorithms"]
codeforces_contest: 1844
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 884 (Div. 1 + Div. 2)"
rating: 1400
weight: 1844
solve_time_s: 104
verified: true
draft: false
---

[CF 1844D - Hàng Chính](https://codeforces.com/problemset/problem/1844/D) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, tham lam, toán học, lý thuyết số, dây 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một chuỗi có độ dài`n`sử dụng càng ít chữ cái viết thường riêng biệt càng tốt. 

Điều đáng chú ý là chuỗi phải vẫn hợp lệ đối với **mọi hình dạng lưới có thể** mà việc truyền tải chính hàng tạo ra chuỗi đó. Nếu như`n = r × c`, thì chuỗi có thể được hiểu là một`r × c`lưới. Với mọi nhân tử của`n`, lưới kết quả không được có cặp ô liền kề chứa cùng một ký tự. 

Theo thứ tự hàng lớn, các hàng xóm ngang tương ứng với các vị trí khác nhau bởi`1`bên trong một hàng, trong khi các hàng xóm theo chiều dọc tương ứng với các vị trí khác nhau bởi`c`. 

Giả sử chúng ta xây dựng một chuỗi`s`. Với mỗi ước số`c`của`n`, chúng tôi phải đảm bảo: 

-`s[i] != s[i+1]`bất cứ khi nào vị trí`i`Và`i+1`nằm trong cùng một hàng. 
-`s[i] != s[i+c]`bất cứ khi nào cả hai vị trí tồn tại. 

Đầu vào chứa tối đa`10^4`trường hợp thử nghiệm, nhưng tổng của tất cả`n`giá trị nhiều nhất là`10^6`. Điều này có nghĩa là một`O(n)`việc xây dựng cho mỗi trường hợp thử nghiệm là hoàn toàn ổn, trong khi bất cứ điều gì liên quan đến việc kiểm tra tất cả các chuỗi có thể có hoặc tất cả các màu lưới có thể có đều cực kỳ tốn kém. 

Phần khó khăn là không xây dựng được một chuỗi hợp lệ. Phần khó khăn là chứng minh rằng nó sử dụng số lượng ký tự riêng biệt tối thiểu có thể. 

Coi như`n = 6`. Một chuỗi xen kẽ ngây thơ`"ababab"`chỉ sử dụng hai chữ cái. Nó hoạt động cho một`1 × 6`lưới và một`6 × 1`lưới. Tuy nhiên, đối với một`2 × 3`lưới các ký tự đầu tiên và thứ tư trở thành hàng xóm dọc:```
a b a
b a b
```Các ô chứa ký tự đầu tiên và thứ tư đều là`'a'`, tạo ra một sự kề cận không hợp lệ. 

Một trường hợp tế nhị khác là khi`n`là nguyên tố. Vì`n = 5`, độ rộng lưới duy nhất có thể là`1`Và`5`. Không có ràng buộc liền kề theo chiều dọc ngoại trừ khoảng cách`5`, không tồn tại bên trong chuỗi. Hai chữ là đủ:```
ababa
```Một giải pháp luôn sử dụng nhiều chữ cái sẽ hợp lệ nhưng không tối ưu. 

Quan sát quan trọng là số lượng chữ cái tối thiểu phụ thuộc vào ước số nhỏ nhất của`n`. 

## Phương pháp tiếp cận 

Quan điểm bạo lực trước hết là hữu ích. 

Giả sử chúng ta quyết định sử dụng`k`các chữ cái và thử tất cả các chuỗi có độ dài có thể`n`. Đối với mỗi chuỗi chúng ta có thể kiểm tra mọi ước số`c`của`n`và xác minh rằng các vị trí ở khoảng cách`1`và khoảng cách`c`không bao giờ chứa các ký tự bằng nhau. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa. 

Vấn đề là không gian tìm kiếm. Ngay cả khi chỉ có hai chữ cái cũng có`2^n`dây. Vì`n`lên đến`10^6`, việc tìm kiếm toàn diện là hoàn toàn không thể. 

Cấu trúc của các ràng buộc kề gợi ý một góc nhìn khác. 

Giả sử chúng ta tạo ra một chuỗi định kỳ```
abcabcabcabc...
```với thời gian`k`. 

Hai vị trí chứa cùng một chữ cái khi chỉ số của chúng khác nhau một bội số`k`. 

Bây giờ hãy nhìn vào bất kỳ số chia nào`c`của`n`. 

Một xung đột theo chiều dọc xuất hiện nếu một khoảng cách nào đó`c`là bội số của`k`, bởi vì khi đó```
s[i] = s[i+c].
```Vì vậy chúng ta cần một khoảng thời gian`k`sao cho **không có ước số của`n`lớn hơn 1 thì chia hết cho`k`**. 

Cách dễ nhất để đạt được điều này là chọn`k`là số nguyên dương nhỏ nhất lớn hơn`1`điều đó **không** chia`n`. 

Sau đó: 

- Mọi số nhỏ hơn đều chia hết`n`. 
- Từ`k`bản thân nó không phân chia`n`, không có ước số của`n`có thể là bội số của`k`. Nếu không thì`k`cũng sẽ chia`n`. 

Điều này ngay lập tức loại bỏ tất cả các xung đột theo chiều dọc. 

Xung đột theo chiều ngang cũng không thể xảy ra vì các vị trí liền kề khác nhau bởi`1`và các ký tự liên tiếp trong một khoảng thời gian dài`k ≥ 2`luôn luôn khác nhau. 

Tại sao điều này là tối ưu? 

Nếu chúng ta sử dụng ít hơn`k`các chữ cái riêng biệt, nói`m < k`, thì theo định nghĩa của`k`, mọi số đều nhỏ hơn`k`chia rẽ`n`. Đặc biệt,`m`chia rẽ`n`. 

Bất kỳ chuỗi nào chỉ sử dụng`m`các chữ cái phải lặp lại một số chữ cái mỗi`m`các vị trí trong một cách xây dựng định kỳ và ước số`m`tạo ra một khoảng cách thẳng đứng bị cấm. Chính thức hơn, người ta có thể chỉ ra rằng ít hơn`k`các chữ cái không thể thỏa mãn tất cả các ước cùng một lúc. 

Do đó số lượng tối thiểu các chữ cái riêng biệt chính xác là số nguyên nhỏ nhất lớn hơn`1`điều đó không phân chia`n`. 

Sau khi tìm thấy giá trị này, chúng ta chỉ cần viết các chữ cái theo chu kỳ:```
a b c ... (k letters) a b c ...
```và cắt ngắn theo chiều dài`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(kⁿ) hoặc tệ hơn | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm số nguyên nhỏ nhất`k ≥ 2`như vậy`n % k != 0`. 
2. Sử dụng cái đầu tiên`k`chữ thường:```
a, b, c, ...
```3. Xây dựng câu trả lời về độ dài`n`bằng cách đặt ký tự```
chr(ord('a') + (i % k))
```ở vị trí`i`. 
4. Xuất chuỗi kết quả. 

Tại sao bước 1 đưa ra số lượng chữ cái tối thiểu? 

Mọi số nguyên nhỏ hơn`k`chia rẽ`n`. Nếu chúng ta cố gắng sử dụng ít hơn`k`các ký tự riêng biệt, một trong những ước số đó sẽ tạo ra các vị trí ký tự bằng nhau không thể tránh khỏi ở khoảng cách bị cấm. Lựa chọn chính xác`k`tránh điều đó vì không có ước số của`n`có thể là bội số của`k`. 

### Tại sao nó hoạt động 

Chuỗi được xây dựng có dấu chấm`k`. Hai vị trí chỉ chứa cùng một ký tự khi khoảng cách của chúng là bội số của`k`. 

Đối với bất kỳ chiều rộng lưới nào`c`chia`n`, một xung đột theo chiều dọc sẽ đòi hỏi`c`là bội số của`k`. Điều đó không thể xảy ra vì`k`không chia`n`, trong khi mọi ước số`c`của`n`làm. 

Xung đột theo chiều ngang tương ứng với khoảng cách`1`. Từ`k ≥ 2`, các vị trí liền kề luôn nhận được các chữ cái khác nhau. 

Vì vậy, mọi cách giải thích lưới có thể đều tốt. 

Để tối ưu, mọi số nguyên nhỏ hơn`k`chia rẽ`n`. Bất kỳ giải pháp nào sử dụng ít hơn`k`các chữ cái riêng biệt sẽ cần phải đáp ứng các ràng buộc liên quan đến một trong các ước số đó, điều này là không thể. Kể từ đây`k`là số lượng ký tự riêng biệt tối thiểu có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        k = 2
        while n % k == 0:
            k += 1

        s = []
        for i in range(n):
            s.append(chr(ord('a') + (i % k)))

        ans.append(''.join(s))

    sys.stdout.write('\n'.join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tìm kiếm số nguyên nhỏ nhất lớn hơn số nguyên không chia hết`n`. Giá trị này luôn lớn nhất là khoảng`log₂(n) + 1`, bởi vì nếu`2,3,4,...,k-1`tất cả đều chia rẽ`n`, sau đó`n`sẽ rất lớn. Trong thực tế vòng lặp chỉ thực hiện một vài lần. 

Sau khi có được`k`, chuỗi được tạo theo chu kỳ. Chức vụ`i`nhận được lá thư tương ứng với`i mod k`, tạo ra một khoảng thời gian chính xác`k`. 

Một chi tiết thực hiện đáng chú ý. Chúng ta không bao giờ cần phải kiểm tra hình dạng lưới một cách rõ ràng. Đối số lý thuyết số đảm bảo tính đúng đắn cho mọi ước số của`n`, do đó việc xây dựng chuỗi tuần hoàn là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
```Tìm số không chia nhỏ nhất. 

| k đã test | 4% k | Kết quả | 
| --- | --- | --- | 
| 2 | 0 | chia | 
| 3 | 1 | dừng lại | 

Vì thế`k = 3`. 

Xây dựng: 

| Vị trí | tôi % 3 | Nhân vật | 
| --- | --- | --- | 
| 0 | 0 | một | 
| 1 | 1 | b | 
| 2 | 2 | c | 
| 3 | 0 | một | 

Đầu ra:```
abca
```Ví dụ này cho thấy rằng bảng chữ cái có kích thước nguyên tố là không cần thiết. Chúng ta chỉ cần số không chia nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
n = 6
```Tìm số không chia nhỏ nhất. 

| k đã test | 6% k | Kết quả | 
| --- | --- | --- | 
| 2 | 0 | chia | 
| 3 | 0 | chia | 
| 4 | 2 | dừng lại | 

Vì thế`k = 4`. 

Xây dựng: 

| Vị trí | tôi % 4 | Nhân vật | 
| --- | --- | --- | 
| 0 | 0 | một | 
| 1 | 1 | b | 
| 2 | 2 | c | 
| 3 | 3 | d | 
| 4 | 0 | một | 
| 5 | 1 | b | 

Đầu ra:```
abcdab
```Ở đây các ước số là`1, 2, 3, 6`. Không ai trong số họ là bội số của`4`, do đó các chữ cái bằng nhau không bao giờ xuất hiện ở khoảng cách dọc bị cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng câu trả lời chi phối công việc | 
| Không gian | O(n) | Bản thân chuỗi đầu ra yêu cầu bộ nhớ O(n) | 

Tổng của tất cả độ dài trong các trường hợp thử nghiệm nhiều nhất là`10^6`, nên tổng số ký tự được tạo ra chỉ là một triệu. Cả thời gian chạy và mức sử dụng bộ nhớ đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        k = 2
        while n % k == 0:
            k += 1

        out.append(''.join(chr(ord('a') + (i % k)) for i in range(n)))

    return '\n'.join(out)

# provided sample-sized checks
assert run("4\n4\n2\n1\n6\n") == "abca\nab\na\nabcdab"

# minimum size
assert run("1\n1\n") == "a"

# prime length
assert run("1\n5\n") == "ababa"

# power of two
assert run("1\n8\n") == "abcabcab"

# another composite
assert run("1\n12\n") == "abcdeabcdeab"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`a`| Chiều dài nhỏ nhất có thể | 
|`n=5`|`ababa`| Số nguyên tố chỉ cần hai chữ cái | 
|`n=8`|`abcabcab`| Lặp lại phép chia cho 2 trước khi tìm số không chia | 
|`n=12`|`abcdeabcdeab`| Một số ước số liên tiếp trước khi dừng | 

## Vỏ cạnh 

cho`n = 1`, lưới duy nhất có thể chứa một ô duy nhất. Thuật toán kiểm tra`k = 2`, tìm thấy rằng`2`không chia`1`, và cấu trúc`"a"`. Một nhân vật riêng biệt rõ ràng là tối ưu. 

Vì`n = 2`, tìm kiếm tìm thấy`k = 3`bởi vì`2`chia rẽ`2`Nhưng`3`không. Đầu ra là`"ab"`. Chỉ sử dụng một ký tự sẽ tạo ra`"aa"`, điều này không hợp lệ đối với`1 × 2`lưới vì các ô liền kề sẽ khớp. 

Đối với một số nguyên tố như`n = 13`, thử nghiệm đầu tiên đã thành công vì`13 % 2 = 1`. Các đầu ra thuật toán:```
ababababababa
```Chỉ có hai chữ cái được sử dụng. Vì ước số duy nhất là`1`Và`13`, không tồn tại ràng buộc theo chiều dọc và hai chữ cái là tối ưu. 

Đối với một số tổng hợp cao như`n = 60`, các bài kiểm tra thuật toán:```
2, 3, 4, 5, 6
```tất cả đều chia`60`, sau đó dừng lại ở`7`. Đầu ra sử dụng chính xác bảy chữ cái trong một chu kỳ. Bất kỳ kích thước bảng chữ cái nhỏ hơn sẽ tương ứng với một số chia`60`, điều này sẽ tạo ra một vùng lân cận lưới bị cấm.
