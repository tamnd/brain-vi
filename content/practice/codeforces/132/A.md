---
title: "CF 132A - Băng Turing"
description: "Chúng ta được cung cấp văn bản cuối cùng được in bằng thủ tục đầu ra \"Turing Tape\" kỳ lạ của INTERCAL. Mỗi ký tự in được tạo từ một số nguyên của một mảng không xác định. Quá trình mã hóa phụ thuộc vào ký tự được in trước đó, vì vậy mỗi bước đều được liên kết với ký tự trước đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 132
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 96 (Div. 1)"
rating: 1300
weight: 132
solve_time_s: 99
verified: true
draft: false
---

[CF 132A - Băng Turing](https://codeforces.com/problemset/problem/132/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp văn bản cuối cùng được in bằng quy trình xuất "Turing Tape" kỳ lạ của INTERCAL. Mỗi ký tự in được tạo từ một số nguyên của một mảng không xác định. Quá trình mã hóa phụ thuộc vào ký tự được in trước đó, vì vậy mỗi bước đều được liên kết với ký tự trước đó. 

Đối với một ký tự đơn, quy trình hoạt động như sau: 

Đầu tiên, lấy ký tự được in trước đó và đảo ngược biểu diễn nhị phân 8 bit của nó. Đối với ký tự đầu tiên, giá trị này được coi là`0`. 

Thứ hai, trừ giá trị mảng hiện tại theo modulo`256`. 

Thứ ba, đảo ngược các bit một lần nữa và diễn giải kết quả dưới dạng mã ASCII của ký tự được in. 

Nhiệm vụ là xây dựng lại mảng ban đầu từ văn bản được in. 

Độ dài đầu vào tối đa là`100`, nó rất nhỏ. Ngay cả một giải pháp không hiệu quả cũng dễ dàng nằm trong giới hạn. Thách thức thực sự là hiểu được phép biến đổi một cách chính xác và tránh những sai lầm khi đảo ngược bit và số học modulo. 

Phần dễ mắc lỗi nhất là hướng dẫn của công thức. Mã hóa sẽ trừ đi giá trị mảng khỏi ký tự đảo ngược trước đó, chứ không phải ngược lại. Nếu chúng ta vô tình tính toán:```
x = current - previous
```thay vì```
x = previous - current  (mod 256)
```toàn bộ trình tự trở nên sai. 

Một lỗi phổ biến khác là chỉ đảo ngược các bit quan trọng thay vì chính xác 8 bit. 

Ví dụ như nhân vật`'A'`có giá trị ASCII`65`.```
65 = 01000001₂
```Đảo ngược tất cả 8 bit mang lại:```
10000010₂ = 130
```Việc thực hiện bất cẩn chỉ có thể đảo ngược`"1000001"`và nhận được`"1000001"`một lần nữa, sản xuất không chính xác`65`. 

Xử lý modulo là một chi tiết tinh tế khác. Giả sử giá trị đảo ngược trước đó là`0`và giá trị đảo ngược hiện tại là`18`. 

Phần tử mảng được yêu cầu là:```
(0 - 18) mod 256 = 238
```Nếu chúng ta quên phép toán modulo, chúng ta sẽ xuất ra`-18`, điều này không hợp lệ vì các phần tử mảng phải là số nguyên 8 bit không dấu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi giá trị mảng có thể có từ`0`ĐẾN`255`cho từng ký tự và mô phỏng quá trình mã hóa cho đến khi ký tự được tạo ra khớp với văn bản đích. 

Đối với một vị trí, chúng tôi có thể sẽ kiểm tra`256`các ứng cử viên và mỗi bài kiểm tra yêu cầu đảo ngược các bit và kiểm tra ký tự kết quả. Với nhiều nhất`100`ký tự, điều này trở nên đại khái`256 × 100 = 25600`những lần thử, điều này vẫn hoàn toàn ổn đối với các giới hạn. 

Brute-force hoạt động vì không gian tìm kiếm rất nhỏ. Mỗi phần tử mảng là một giá trị 8 bit, do đó chỉ có`256`khả năng. 

Vấn đề là lực lượng vũ phu đã bỏ qua đại số ẩn bên trong phép biến đổi. Quá trình mã hóa hoàn toàn có thể đảo ngược. Một khi chúng ta nhận ra điều đó thì việc đoán mò sẽ trở nên không cần thiết. 

Giả định:```
p = reverse_bits(previous_character)
c = reverse_bits(current_character)
```Quy tắc mã hóa cho biết:```
c = (p - a[i]) mod 256
```Sắp xếp lại mang lại:```
a[i] = (p - c) mod 256
```Mọi thứ ở phía bên phải đều đã được biết từ chính văn bản. Ký tự trước đó đã được biết và ký tự hiện tại đã được biết. Điều đó có nghĩa là mỗi phần tử mảng có thể được tính trực tiếp trong thời gian không đổi. 

Quan sát quan trọng là sự đảo ngược bit là nghịch đảo của chính nó. Đảo ngược hai lần sẽ trả về số ban đầu. Do đó, chúng ta có thể đảo ngược các giá trị ASCII của các ký tự được in một cách đơn giản và tái tạo lại các trạng thái trung gian chính xác được sử dụng trong quá trình mã hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(256n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Duy trì một biến`prev`, biểu thị giá trị nhị phân đảo ngược của ký tự được in trước đó. Khởi tạo nó thành`0`, vì câu lệnh xác định bước đầu tiên theo cách đó. 
3. Với mỗi ký tự trong chuỗi, hãy tính giá trị ASCII của nó bằng`ord()`. 
4. Đảo ngược các bit của giá trị ASCII này bằng chính xác 8 bit. 

Sự đảo ngược phải bảo toàn các số 0 đứng đầu. Ví dụ:```
00010010 -> 01001000
```5. Gọi giá trị đảo ngược này là`cur`. 
6. Quy tắc mã hóa là:```
cur = (prev - a[i]) mod 256
```Sắp xếp lại mang lại:```
a[i] = (prev - cur) mod 256
```Tính giá trị này và in nó. 
7. Cập nhật`prev = cur`trước khi xử lý ký tự tiếp theo. 

### Tại sao nó hoạt động 

Thuật toán tái tạo lại chính xác các giá trị trung gian được sử dụng trong quá trình mã hóa. 

Đối với mỗi nhân vật,`cur`bằng với kết quả được tạo ra sau bước 2 của quy trình ban đầu, vì việc đảo ngược ký tự được in sẽ khôi phục trạng thái trung gian đó. Vì phương trình mã hóa là:```
cur = (prev - a[i]) mod 256
```giải quyết cho`a[i]`xác định duy nhất giá trị mảng ban đầu. Tính bất biến đó là`prev`luôn lưu trữ cách biểu diễn đảo ngược của ký tự được in trước đó, khớp chính xác với trạng thái của bộ mã hóa trước khi xử lý phần tử tiếp theo. 

Bởi vì mỗi bước đều có thể đảo ngược và modulo`256`số học được bảo toàn, mảng được xây dựng lại được đảm bảo là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def reverse_bits(x):
    res = 0

    for i in range(8):
        if x & (1 << i):
            res |= (1 << (7 - i))

    return res

def solve():
    s = input().rstrip("\n")

    prev = 0

    for ch in s:
        cur = reverse_bits(ord(ch))
        value = (prev - cur) % 256
        print(value)
        prev = cur

solve()
```các`reverse_bits`thực hiện đảo ngược chính xác 8 bit. Chi tiết này quan trọng vì các ký tự ASCII có thể chứa các số 0 đứng đầu ở dạng nhị phân. Việc sử dụng tính năng cắt chuỗi mà không có phần đệm sẽ tạo ra câu trả lời sai cho nhiều ký tự. 

Biến`prev`lưu trữ biểu diễn nhị phân đảo ngược của ký tự được in trước đó. Ban đầu nó là`0`, khớp với quy tắc đặc biệt của câu lệnh cho ký tự đầu tiên. 

Đối với mỗi ký tự, mã sẽ tính:```
value = (prev - cur) % 256
```Hoạt động modulo là cần thiết vì quy trình ban đầu sử dụng số học 8 bit không dấu. của Python`% 256`chuyển đổi rõ ràng những khác biệt âm thành phạm vi chính xác`0..255`. 

Bản cập nhật:```
prev = cur
```phải xảy ra sau khi in câu trả lời hiện tại. Cập nhật quá sớm sẽ dịch chuyển toàn bộ chuỗi sang một vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Hi
```Theo dõi từng bước: 

| Nhân vật | ASCII | Các bit đảo ngược (`cur`) | Trước (`prev`) | Giá trị đầu ra | 
| --- | --- | --- | --- | --- | 
| H | 72 | 18 | 0 | 238 | 
| tôi | 105 | 150 | 18 | 124 | 

Đầu ra:```
238
124
```Dấu vết này cho thấy trạng thái chuyển từ ký tự này sang ký tự tiếp theo như thế nào. Phép tính thứ hai phụ thuộc vào dạng đảo ngược của`'H'`, không phải trên giá trị ASCII gốc. 

### Ví dụ 2 

đầu vào:```
A
```Theo dõi từng bước: 

| Nhân vật | ASCII | Các bit đảo ngược (`cur`) | Trước (`prev`) | Giá trị đầu ra | 
| --- | --- | --- | --- | --- | 
| A | 65 | 130 | 0 | 126 | 

Đầu ra:```
126
```Ví dụ này chứng minh tại sao số 0 đứng đầu lại quan trọng. Dạng 8 bit của`65`là:```
01000001
```Đảo ngược tất cả tám bit mang lại:```
10000010 = 130
```Đảo ngược ngắn hơn sẽ tạo ra câu trả lời sai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và việc đảo ngược bit luôn mất 8 lần lặp | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với nhiều nhất`100`ký tự, thời gian chạy rất nhỏ. Ngay cả cách tiếp cận bạo lực cũng có thể được thực hiện một cách thoải mái, nhưng giải pháp tái thiết trực tiếp sẽ rõ ràng hơn và chính xác về mặt toán học. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def reverse_bits(x):
    res = 0

    for i in range(8):
        if x & (1 << i):
            res |= (1 << (7 - i))

    return res

def solve():
    input = sys.stdin.readline

    s = input().rstrip("\n")

    prev = 0
    out = []

    for ch in s:
        cur = reverse_bits(ord(ch))
        out.append(str((prev - cur) % 256))
        prev = cur

    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample
assert run("Hello, World!\n") == (
    "238\n108\n112\n0\n64\n194\n48\n26\n244\n168\n24\n16\n162\n"
), "sample 1"

# minimum size input
assert run("A\n") == "126\n", "single character"

# repeated characters
assert run("AAA\n") == "126\n0\n0\n", "same reversed value repeatedly"

# boundary ASCII values
assert run(" ~\n") == "252\n252\n", "space and tilde"

# alternating pattern
assert run("Hi\n") == "238\n124\n", "state transition correctness"

# maximum size style test
assert len(run(("A" * 100) + "\n").strip().split()) == 100, "length 100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A`|`126`| Khởi tạo một ký tự | 
|`AAA`|`126 0 0`| Cập nhật trạng thái chính xác trên các ký tự bằng nhau | 
|` ~`|`252 252`| Giá trị ASCII có thể in ranh giới | 
|`Hi`|`238 124`| Sự phụ thuộc vào ký tự trước đó | 
|`A`lặp lại 100 lần | 100 kết quả đầu ra | Xử lý kích thước đầu vào tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
A
```Thuật toán bắt đầu bằng:```
prev = 0
```Giá trị ASCII của`'A'`là`65`. 

Đảo ngược 8 bit của nó là:```
01000001 -> 10000010 = 130
```Đầu ra trở thành:```
(0 - 130) mod 256 = 126
```Thuật toán xử lý chính xác quy tắc khởi tạo đặc biệt cho ký tự đầu tiên. 

Bây giờ hãy xem xét các ký tự lặp lại:```
AAA
```Giá trị đảo ngược của`'A'`luôn luôn là`130`. 

Các tính toán là:```
First:  (0 - 130) mod 256 = 126
Second: (130 - 130) mod 256 = 0
Third:  (130 - 130) mod 256 = 0
```Trường hợp này xác nhận rằng thuật toán cập nhật`prev`chính xác sau mỗi lần lặp. 

Cuối cùng, hãy xem xét các ký tự có dạng nhị phân bắt đầu bằng số 0:```
A
```Dạng nhị phân của nó là:```
01000001
```Nếu chúng ta chỉ đảo ngược các bit quan trọng, chúng ta sẽ nhận được sai:```
1000001
```vẫn còn đó`65`. 

Việc triển khai xử lý rõ ràng chính xác 8 vị trí, do đó giá trị đảo ngược chính xác`130`được thu được. Điều này ngăn chặn các lỗi im lặng trên nhiều ký tự ASCII có thể in được.
