---
title: "CF 1760B - Bài toán yêu thích của Atilla"
description: "Chúng tôi được cung cấp một số chuỗi chữ thường. Đối với mỗi chuỗi, chúng tôi muốn bảng chữ cái nhỏ nhất chứa mọi ký tự xuất hiện trong chuỗi đó. Bảng chữ cái luôn là tiền tố của bảng chữ cái tiếng Anh."
date: "2026-06-09T14:21:06+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1760
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 835 (Div. 4)"
rating: 800
weight: 1760
solve_time_s: 207
verified: true
draft: false
---

[CF 1760B - Bài toán yêu thích của Atilla](https://codeforces.com/problemset/problem/1760/B) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, chuỗi 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số chuỗi chữ thường. Đối với mỗi chuỗi, chúng tôi muốn bảng chữ cái nhỏ nhất chứa mọi ký tự xuất hiện trong chuỗi đó. 

Bảng chữ cái luôn là tiền tố của bảng chữ cái tiếng Anh. Một bảng chữ cái cỡ 1 chỉ chứa`a`, một bảng chữ cái cỡ 2 chứa`a`Và`b`, vân vân. Một bảng chữ cái có kích thước 26 chứa mọi chữ cái viết thường. 

Câu hỏi đơn giản là: tiền tố nhỏ nhất của bảng chữ cái mà vẫn bao gồm tất cả các ký tự được sử dụng trong chuỗi là gì? 

Một cách hữu ích để suy nghĩ về nó là tập trung vào chữ cái lớn nhất hiện tại. Nếu một chuỗi chứa chữ cái`w`, thì bất kỳ bảng chữ cái hợp lệ nào cũng phải chứa tất cả các chữ cái từ`a`bởi vì`w`. Từ`w`là chữ cái thứ 23 thì câu trả lời phải có ít nhất là 23. Nếu tất cả các chữ cái khác sớm hơn`w`, thì bảng chữ cái cỡ 23 là đủ. 

Những hạn chế là rất nhỏ. Mỗi chuỗi có độ dài tối đa là 100 và có tối đa 1000 trường hợp kiểm thử. Thậm chí một$O(n^2)$giải pháp sẽ hoàn toàn ổn ở đây. Chúng ta chỉ cần quét từng chuỗi một lần. 

Một lỗi dễ mắc phải là đếm các ký tự khác nhau thay vì tìm ký tự lớn nhất. 

Coi như:```
1
3
bcf
```Các chữ cái riêng biệt là`{b,c,f}`, có kích thước 3. Một giải pháp bất cẩn sẽ tạo ra 3. Câu trả lời đúng là 6 vì`f`là chữ cái thứ 6 và bất kỳ bảng chữ cái nào có chứa`f`cũng phải chứa`a`bởi vì`e`. 

Một sai lầm khác là sử dụng độ dài chuỗi. 

Coi như:```
1
5
zzzzz
```Độ dài chuỗi là 5, nhưng câu trả lời là 26 vì`z`là lá thư thứ 26. 

Sai lầm thứ ba là sắp xếp các ký tự và sử dụng số lượng chữ cái duy nhất. 

Coi như:```
1
4
down
```Có bốn chữ cái riêng biệt, nhưng chữ cái lớn nhất là`w`, vậy đáp án là 23. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi kích thước bảng chữ cái từ 1 đến 26. Đối với mỗi kích thước, chúng tôi sẽ kiểm tra xem mọi ký tự của chuỗi có thuộc về bảng chữ cái đó hay không. Kích thước hợp lệ đầu tiên sẽ là câu trả lời. 

Điều này hoạt động vì chỉ có 26 kích cỡ bảng chữ cái có thể. Đối với một chuỗi có độ dài$n$, độ phức tạp là$O(26n)$. 

Quan sát quan trọng là câu trả lời hoàn toàn được xác định bởi ký tự lớn nhất trong chuỗi. Nếu ký tự lớn nhất là`k`, thì mọi ký tự nhỏ hơn sẽ tự động được đưa vào bảng chữ cái có kích thước tương ứng với`k`. Bất kỳ bảng chữ cái nhỏ hơn sẽ bỏ lỡ`k`chính nó. 

Điều đó làm giảm vấn đề tìm ký tự tối đa và chuyển đổi nó sang vị trí của nó trong bảng chữ cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`và chuỗi`s`. 
3. Tìm ký tự lớn nhất trong`s`. 
4. Chuyển ký tự đó sang vị trí bảng chữ cái bằng cách sử dụng:`ord(max_char) - ord('a') + 1`5. Xuất ra giá trị kết quả. 

Lý do điều này có tác dụng là vì bảng chữ cái có kích thước`x`chứa chính xác các chữ cái từ`a`thông qua`x`- lá thư thứ Ký tự lớn nhất xuất hiện trong chuỗi là ký tự duy nhất xác định tiền tố này phải kéo dài bao xa. 

### Tại sao nó hoạt động 

hãy để`c`là ký tự lớn nhất xuất hiện trong chuỗi. 

Bất kỳ bảng chữ cái hợp lệ nào cũng phải chứa`c`, vì vậy kích thước của nó ít nhất phải bằng vị trí bảng chữ cái của`c`. 

Một bảng chữ cái có kích thước bằng vị trí của`c`chứa mọi chữ cái từ`a`bởi vì`c`. Vì không có ký tự nào trong chuỗi lớn hơn`c`, tất cả các ký tự đều được bao gồm. 

Kích thước bảng chữ cái hợp lệ tối thiểu chính xác là vị trí của ký tự lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    s = input().strip()

    ans = ord(max(s)) - ord('a') + 1
    print(ans)
```Việc thực hiện trực tiếp theo quan sát từ thuật toán. 

Cuộc gọi đến`max(s)`tìm chữ cái viết thường lớn nhất theo từ điển trong chuỗi. Vì các chữ cái viết thường xuất hiện theo thứ tự bảng chữ cái trong bảng ASCII nên ký tự lớn nhất cũng là ký tự mới nhất theo thứ tự bảng chữ cái. 

biểu thức```
ord(max(s)) - ord('a') + 1
```chuyển đổi ký tự đó thành chỉ mục bảng chữ cái dựa trên 1. Ví dụ:```
ord('a') - ord('a') + 1 = 1
ord('f') - ord('a') + 1 = 6
ord('z') - ord('a') + 1 = 26
```Không có lo ngại về tràn vì tất cả các giá trị đều rất nhỏ. Điểm tinh tế duy nhất là loại bỏ dòng mới ở cuối bằng`.strip()`trước khi xử lý chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
down
```| Bước | Tối đa hiện tại | 
| --- | --- | 
| d | d | 
| o | o | 
| w | w | 
| n | w | 

Ký tự lớn nhất là`w`. 

| Nhân vật | Vị trí | 
| --- | --- | 
| w | 23 | 

Trả lời:`23`. 

Ví dụ này cho thấy câu trả lời phụ thuộc vào ký tự lớn nhất chứ không phụ thuộc vào số lượng ký tự riêng biệt. 

### Ví dụ 2 

đầu vào:```
1
3
bcf
```| Bước | Tối đa hiện tại | 
| --- | --- | 
| b | b | 
| c | c | 
| f | f | 

Ký tự lớn nhất là`f`. 

| Nhân vật | Vị trí | 
| --- | --- | 
| f | 6 | 

Trả lời:`6`. 

Ví dụ này chứng minh tại sao việc đếm các chữ cái riêng biệt sẽ không chính xác. Chỉ có ba chữ cái riêng biệt nhưng kích thước bảng chữ cái được yêu cầu là sáu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét chuỗi | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Vì mỗi chuỗi có độ dài tối đa là 100 nên thời gian chạy cực kỳ nhỏ. Ngay cả với 1000 trường hợp thử nghiệm, tổng công việc chỉ có khoảng 100.000 thao tác ký tự. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()
        out.append(str(ord(max(s)) - ord('a') + 1))

    return "\n".join(out)

# provided sample
assert run(
    "5\n"
    "1\n"
    "a\n"
    "4\n"
    "down\n"
    "10\n"
    "codeforces\n"
    "3\n"
    "bcf\n"
    "5\n"
    "zzzzz\n"
) == "1\n23\n19\n6\n26"

# minimum size
assert run("1\n1\na\n") == "1"

# largest possible letter
assert run("1\n1\nz\n") == "26"

# all characters identical
assert run("1\n5\nmmmmm\n") == "13"

# mixed letters
assert run("1\n6\nabczxy\n") == "26"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1`| Câu trả lời nhỏ nhất có thể | 
|`z`|`26`| Câu trả lời lớn nhất có thể | 
|`mmmmm`|`13`| Ký tự lặp lại | 
|`abczxy`|`26`| Ký tự lớn nhất xác định câu trả lời | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
1
a
```Ký tự lớn nhất là`a`, có vị trí là 1. Thuật toán đưa ra 1. Điều này xác minh bảng chữ cái nhỏ nhất có thể. 

Coi như:```
1
5
zzzzz
```Mỗi nhân vật đều`z`. Ký tự tối đa vẫn là`z`, có vị trí là 26. Thuật toán đưa ra 26. Việc lặp lại các lần xuất hiện không ảnh hưởng đến kết quả. 

Coi như:```
1
3
bcf
```Ký tự tối đa là`f`. Thuật toán đưa ra 6. Điều này xác nhận rằng câu trả lời không phải là số lượng các chữ cái riêng biệt. 

Coi như:```
1
4
down
```Ký tự tối đa là`w`. Thuật toán cho kết quả 23. Mặc dù chỉ xuất hiện bốn chữ cái khác nhau nhưng bảng chữ cái phải kéo dài đến chữ cái thứ 23 để bao gồm`w`.
