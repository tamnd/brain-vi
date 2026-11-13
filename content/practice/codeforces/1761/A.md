---
title: "CF 1761A - Hai hoán vị"
description: "Chúng tôi không được yêu cầu xây dựng các hoán vị. Chúng ta chỉ cần quyết định xem hai hoán vị có độ dài n có thể tồn tại sao cho chúng có cùng chính xác a vị trí ở đầu và chính xác b vị trí ở cuối hay không. Hãy suy nghĩ về hai hoán vị p và q."
date: "2026-06-09T14:02:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1761
codeforces_index: "A"
codeforces_contest_name: "Pinely Round 1 (Div. 1 + Div. 2)"
rating: 800
weight: 1761
solve_time_s: 206
verified: true
draft: false
---

[CF 1761A - Hai hoán vị](https://codeforces.com/problemset/problem/1761/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán mang tính xây dựng 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi không được yêu cầu xây dựng các hoán vị. Chúng ta chỉ cần quyết định xem hai hoán vị độ dài có`n`có thể tồn tại sao cho chúng chia sẻ chính xác`a`vị trí ngay từ đầu và chính xác`b`các vị trí ở cuối. 

Hãy suy nghĩ về hai hoán vị`p`Và`q`. Nếu tiền tố chung dài nhất của chúng có độ dài`a`, sau đó vị trí`0...a-1`phải bằng nhau và có vị trí`a`phải khác nhau, trừ khi`a = n`. Tương tự, nếu hậu tố chung dài nhất của chúng có độ dài`b`, sau đó là cái cuối cùng`b`các vị trí phải bằng nhau và vị trí ngay trước hậu tố đó phải khác nhau, trừ khi`b = n`. 

Những hạn chế là rất nhỏ. Độ dài hoán vị tối đa là`100`, và có thể có tới`10^4`trường hợp thử nghiệm. Thậm chí một`O(n)`hoặc`O(n^2)`kiểm tra mỗi trường hợp thử nghiệm sẽ là chuyện nhỏ. Thử thách không phải là hiệu suất, mà là việc khám phá điều kiện chính xác để xác định liệu một cặp hợp lệ có tồn tại hay không. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như:```
n = 1, a = 1, b = 1
```Câu trả lời là`Yes`. Chỉ có một hoán vị duy nhất`[1]`, vì vậy cả hai hoán vị phải giống hệt nhau. Một giải pháp từ chối một cách mù quáng những trường hợp`a + b >= n`sẽ xuất ra không chính xác`No`. 

Coi như:```
n = 2, a = 1, b = 1
```Câu trả lời là`No`. Nếu vị trí thứ nhất trùng nhau và vị trí thứ hai cũng khớp thì các hoán vị hoàn toàn giống nhau. Độ dài tiền tố và hậu tố chung của chúng đều là`2`, không`1`. 

Coi như:```
n = 4, a = 1, b = 1
```Câu trả lời là`Yes`. Một cặp hợp lệ là:```
p = [1,2,3,4]
q = [1,3,2,4]
```Độ dài tiền tố phổ biến là`1`và độ dài hậu tố chung là`1`. 

Một lỗi phổ biến là chỉ kiểm tra xem`a + b <= n`. Ví dụ:```
n = 3, a = 1, b = 1
```Đây`a + b = 2 <= 3`, nhưng câu trả lời vẫn là`No`. Sau khi cố định một vị trí chung ở phía trước và một vị trí ở phía sau, chỉ còn lại một vị trí ở giữa. Hai hoán vị không thể khác nhau ở đúng một vị trí vì bất kỳ thay đổi nào cũng phải di chuyển ít nhất hai giá trị. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra tất cả các hoán vị có độ dài`n`, kiểm tra từng cặp và tính tiền tố và hậu tố chung dài nhất của chúng. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa. Thật không may, số lượng hoán vị là`n!`. Ngay cả đối với`n = 10`, đã có rồi`3,628,800`hoán vị, làm cho việc tìm kiếm hoàn chỉnh trở nên vô vọng. 

Quan sát quan trọng đến từ việc đếm các vị trí. 

đầu tiên`a`vị trí buộc phải bằng nhau. cuối cùng`b`vị trí cũng buộc phải bằng nhau. Điều đó có nghĩa`a + b`các vị trí đã được cố định là khớp, mặc dù một số vị trí có thể trùng nhau. 

Các vị trí còn lại tạo thành vùng duy nhất mà các hoán vị có thể khác nhau. 

Cho phép```
free = n - a - b
```Nếu như`free < 0`, thì tiền tố và hậu tố chung bắt buộc sẽ chồng chéo quá nhiều. Cách duy nhất điều này có thể xảy ra là nếu các hoán vị trở nên hoàn toàn giống nhau. Điều đó chỉ có thể xảy ra với trường hợp đặc biệt`n = a = b`. 

Nếu như`free = 0`, mọi vị trí buộc phải khớp nhau, do đó các hoán vị lại giống hệt nhau. Trừ khi`n = a = b`, không thể đạt được độ dài tiền tố và hậu tố dài nhất được yêu cầu. 

Nếu như`free = 1`, có đúng một vị trí không cố định. Hai hoán vị không thể khác nhau ở đúng một vị trí, vì việc di chuyển một giá trị nhất thiết cũng ảnh hưởng đến vị trí khác. 

Để tạo các hoán vị khác nhau, chúng ta cần ít nhất hai vị trí trống. Sau đó, chúng ta có thể hoán đổi hai giá trị bên trong vùng tự do đó và đạt được sự không khớp cần thiết. 

Vậy điều kiện trở thành:```
n - a - b >= 2
```hoặc trường hợp đặc biệt:```
a = n and b = n
```Các bài xã luận của Codeforce thường thể hiện ý tưởng tương tự như:```
a + b <= n - 2
```hoặc```
a = n and b = n
```Những điều kiện này là tương đương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n!) 2 · n) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`a`, Và`b`. 
2. Kiểm tra xem`a == n`Và`b == n`. 

Trong tình huống này cả hai hoán vị có thể đơn giản là giống hệt nhau. Câu trả lời là`Yes`. 
3. Ngược lại, hãy tính xem`a + b <= n - 2`. 

Điều này có nghĩa là ít nhất hai vị trí vẫn nằm ngoài tiền tố chung và hậu tố chung bắt buộc. 
4. Nếu điều kiện đúng, xuất ra`Yes`. 

Với ít nhất hai vị trí trống, chúng ta có thể sắp xếp hoán đổi bên trong vùng đó, tạo ra các hoán vị khác nhau trong khi vẫn giữ nguyên tiền tố và hậu tố cần thiết. 
5. Ngược lại, xuất ra`No`. 

Ít hơn hai vị trí tự do có nghĩa là các hoán vị sẽ giống hệt nhau hoặc cần khác nhau ở chính xác một vị trí, điều này là không thể đối với các hoán vị. 

### Tại sao nó hoạt động 

đầu tiên`a`vị trí và cuối cùng`b`vị trí được cố định bằng nhau giữa hai hoán vị. Mọi sự khác biệt phải xảy ra ở các vị trí còn lại. 

Một hoán vị không thể khác một hoán vị khác ở đúng một vị trí. Bất kỳ giá trị di chuyển nào cũng buộc ít nhất một vị trí bổ sung phải thay đổi. Do đó, một cặp không giống nhau yêu cầu ít nhất hai vị trí được phép thay đổi. 

Khi`a + b <= n - 2`, có ít nhất hai vị trí như vậy và một phép hoán đổi đơn giản bên trong vùng đó sẽ tạo ra một cặp hợp lệ. Khi`a + b > n - 2`, vẫn còn ít hơn hai vị trí trống. Trường hợp khả thi duy nhất là khi mọi vị trí được chia sẻ, cụ thể là`a = b = n`, trong đó các hoán vị giống hệt nhau thỏa mãn yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, a, b = map(int, input().split())

    if (a == n and b == n) or (a + b <= n - 2):
        print("Yes")
    else:
        print("No")
```Việc thực hiện tuân theo điều kiện toán học trực tiếp. 

Nhánh đầu tiên xử lý tình huống duy nhất trong đó cả hai độ dài chung dài nhất đều bằng toàn bộ độ dài hoán vị. Trong trường hợp đó hai hoán vị có thể giống hệt nhau. 

Nhánh thứ hai kiểm tra xem có ít nhất hai vị trí nằm ngoài tiền tố và hậu tố bắt buộc phải khớp hay không. sử dụng`a + b <= n - 2`tránh tính toán rõ ràng số lượng vị trí trống, mặc dù nó hoàn toàn giống nhau. 

Không có lo ngại về tràn vì tất cả các giá trị tối đa đều`100`. Phần tinh tế duy nhất là ghi nhớ trường hợp đặc biệt`a = b = n`. Không có nó, một đầu vào như`1 1 1`sẽ sản xuất không chính xác`No`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, a = 1, b = 1
```| Bước | n | một | b | Kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 1 | a=n và b=n? | Không | 
| 2 | 4 | 1 | 1 | a+b <= n-2 ? | | 
| 3 | 4 | 1 | 1 | 2 <= 2 | Có | 

Đầu ra:```
Yes
```Có chính xác hai vị trí miễn phí. Chúng ta có thể hoán đổi các vị trí đó giữa các hoán vị, tạo ra sự không khớp trong khi vẫn giữ nguyên tiền tố và hậu tố bắt buộc. 

### Ví dụ 2 

đầu vào:```
n = 3, a = 1, b = 1
```| Bước | n | một | b | Kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | a=n và b=n? | Không | 
| 2 | 3 | 1 | 1 | a+b <= n-2 ? | | 
| 3 | 3 | 1 | 1 | 2 <= 1 | Không | 

Đầu ra:```
No
```Chỉ còn một vị trí nằm ngoài tiền tố và hậu tố chung. Hai hoán vị không thể khác nhau ở đúng một vị trí, do đó không tồn tại cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Số phép tính số học không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Ngay cả với`10^4`trường hợp thử nghiệm, chương trình chỉ thực hiện một số thao tác cho mỗi trường hợp. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, a, b = map(int, input().split())

        if (a == n and b == n) or (a + b <= n - 2):
            ans.append("Yes")
        else:
            ans.append("No")

    return "\n".join(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""4
1 1 1
2 1 2
3 1 1
4 1 1
"""
) == "Yes\nNo\nNo\nYes", "sample 1"

# minimum size
assert run(
"""1
1 1 1
"""
) == "Yes", "single element permutation"

# exactly one free position
assert run(
"""1
5 2 2
"""
) == "No", "cannot differ in exactly one position"

# exactly two free positions
assert run(
"""1
6 2 2
"""
) == "Yes", "minimum accepted free region"

# maximum size style case
assert run(
"""1
100 49 49
"""
) == "Yes", "large valid case"

# all positions forced equal but not full length
assert run(
"""1
2 1 1
"""
) == "No", "identical permutations would violate longest lengths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`Yes`| Trường hợp đặc biệt khi hoán vị giống hệt nhau | 
|`5 2 2`|`No`| Chính xác là một vị trí miễn phí | 
|`6 2 2`|`Yes`| Ranh giới nơi tồn tại hai vị trí miễn phí | 
|`100 49 49`|`Yes`| Đầu vào hợp lệ lớn gần giới hạn | 
|`2 1 1`|`No`| Bắt buộc phải bình đẳng ở mọi vị trí nhưng độ dài dài nhất nhỏ hơn | 

## Vỏ cạnh 

Hãy xem xét:```
n = 1
a = 1
b = 1
```Đầu tiên thuật toán sẽ kiểm tra xem`a == n`Và`b == n`. Cả hai điều kiện đều đúng, vì vậy nó xuất ra`Yes`. 

Dấu vết:```
a == n -> true
b == n -> true
answer -> Yes
```Điều này xử lý chính xác hoán vị duy nhất`[1]`. 

Coi như:```
n = 2
a = 1
b = 1
```Trường hợp đặc biệt thất bại vì`a`Và`b`cả hai đều không bằng nhau`n`. 

Sau đó:```
a + b = 2
n - 2 = 0
```điều kiện`2 <= 0`là sai, do đó thuật toán đưa ra`No`. 

Mọi vị trí sẽ bị buộc phải bằng nhau, làm cho các hoán vị giống hệt nhau. Tiền tố và hậu tố chung dài nhất của chúng sẽ trở thành`2`, không`1`. 

Coi như:```
n = 3
a = 1
b = 1
```Thuật toán đánh giá:```
a + b = 2
n - 2 = 1
```Từ`2 <= 1`là sai, câu trả lời là`No`. 

Chỉ còn lại một vị trí ở giữa. Mọi nỗ lực thay đổi hoán vị sẽ yêu cầu thay đổi ít nhất hai vị trí, điều này là không thể nếu chỉ có một vị trí trống. 

Coi như:```
n = 4
a = 1
b = 1
```Thuật toán đánh giá:```
a + b = 2
n - 2 = 2
```Từ`2 <= 2`là đúng, câu trả lời là`Yes`. 

Có chính xác hai vị trí vẫn chưa được cố định. Việc hoán đổi các giá trị ở các vị trí đó sẽ tạo ra các hoán vị hợp lệ với độ dài tiền tố và hậu tố chung được yêu cầu.
