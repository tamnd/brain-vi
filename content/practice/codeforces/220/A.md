---
title: "CF 220A - Chú Voi Nhỏ và Vấn Đề"
description: "Chúng ta được cung cấp một mảng ban đầu được sắp xếp theo thứ tự không giảm. Tại một thời điểm nào đó, không có gì xảy ra hoặc chính xác một cặp phần tử có thể đã bị hoán đổi."
date: "2026-06-04T01:54:05+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 220
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 136 (Div. 1)"
rating: 1300
weight: 220
solve_time_s: 93
verified: true
draft: false
---

[CF 220A - Chú voi nhỏ và vấn đề](https://codeforces.com/problemset/problem/220/A) 

**Đánh giá:** 1300 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng ban đầu được sắp xếp theo thứ tự không giảm. Tại một thời điểm nào đó, không có gì xảy ra hoặc chính xác một cặp phần tử có thể đã bị hoán đổi. 

Câu hỏi đặt ra là liệu mảng hiện tại có thể được chuyển đổi thành một mảng được sắp xếp đầy đủ bằng cách sử dụng tối đa một lần hoán đổi hai vị trí bất kỳ hay không. Nếu câu trả lời là có, chúng tôi sẽ in`"YES"`. Nếu không chúng tôi in`"NO"`. 

Độ dài mảng có thể lớn bằng$10^5$. Bất kỳ thuật toán nào thử mọi hoán đổi có thể sẽ cần khoảng$n^2$những nỗ lực. Với$n = 10^5$, điều đó có nghĩa là xung quanh$10^{10}$khả năng, điều này hoàn toàn không thể thực hiện được trong thời gian giới hạn 2 giây. Chúng ta cần một cái gì đó gần gũi$O(n \log n)$hoặc$O(n)$. 

Một khía cạnh tinh tế của vấn đề là các giá trị không nhất thiết phải khác biệt. Sự trùng lặp làm cho một số cách tiếp cận trực quan thất bại. 

Coi như:```
4
1 3 2 2
```Mảng được sắp xếp là:```
1 2 2 3
```Mảng hiện tại khác với mảng được sắp xếp chỉ ở vị trí 2 và 4, do đó, một lần hoán đổi sẽ sửa nó. Câu trả lời đúng là:```
YES
```Một giải pháp bất cẩn chỉ tìm kiếm sự đảo ngược có thể kết luận sai rằng cần phải làm nhiều việc hơn. 

Một trường hợp quan trọng khác là khi mảng đã được sắp xếp:```
5
1 2 2 3 4
```Câu trả lời vẫn là:```
YES
```bởi vì không được phép hoán đổi. Yêu cầu là "nhiều nhất một lần hoán đổi", không phải "chính xác một lần hoán đổi". 

Trường hợp cạnh thứ ba xuất hiện khi có nhiều hơn hai vị trí không đồng ý với thứ tự sắp xếp:```
4
4 3 2 1
```Phiên bản được sắp xếp là:```
1 2 3 4
```Tất cả bốn vị trí đều khác nhau. Không một trao đổi đơn lẻ nào có thể khắc phục được tất cả chúng, vì vậy câu trả lời là:```
NO
```Một giải pháp chỉ kiểm tra xem số lần đảo ngược có nhỏ hay không sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một phương pháp bắt buộc đơn giản là thử mọi giao dịch hoán đổi có thể xảy ra, bao gồm cả khả năng thực hiện không hoán đổi. Đối với mỗi lựa chọn, chúng tôi kiểm tra xem mảng kết quả có được sắp xếp hay không. 

có$O(n^2)$sự hoán đổi có thể xảy ra. Kiểm tra xem một mảng có được sắp xếp hay không$O(n)$thời gian. Tổng độ phức tạp trở thành$O(n^3)$. 

Vì$n = 10^5$, điều này chậm một cách vô vọng. Thậm chí$O(n^2)$sẽ là quá lớn. 

Quan sát chính xuất phát từ việc so sánh mảng hiện tại với phiên bản được sắp xếp đầy đủ của nó. 

Giả sử chúng ta tạo một bản sao đã được sắp xếp của mảng. Bất kỳ vị trí nào mà mảng hiện tại và mảng đã sắp xếp đã khớp nhau đều không cần thay đổi. Một lần hoán đổi duy nhất chỉ có thể ảnh hưởng đến hai vị thế. Do đó, nếu có nhiều hơn hai vị trí khác nhau so với mảng đã sắp xếp thì một lần hoán đổi không bao giờ có thể làm cho các mảng giống hệt nhau. 

Điều này biến vấn đề thành một kiểm tra rất đơn giản. 

Xây dựng phiên bản đã sắp xếp của mảng. Đếm xem có bao nhiêu chỉ mục chứa các giá trị khác nhau trong mảng ban đầu và mảng được sắp xếp. 

Nếu số vị trí không khớp là: 

- 0, mảng đã được sắp xếp. 
- 2, hoán đổi 2 vị trí đó sẽ sửa được mảng. 
- Nhiều hơn 2, một lần hoán đổi là không đủ. 

Vì mỗi lần hoán đổi đều thay đổi chính xác hai vị trí nên câu trả lời là`"YES"`chính xác khi số lượng không khớp nhiều nhất là 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Tạo một bản sao đã được sắp xếp của mảng. 

Bản sao được sắp xếp đại diện cho cấu hình mục tiêu mà chúng tôi muốn tiếp cận. 
3. So sánh mảng ban đầu và mảng đã sắp xếp theo từng vị trí. 

Đếm xem có bao nhiêu chỉ số chứa các giá trị khác nhau. 
4. Nếu số lượng không khớp nhiều nhất là 2, hãy in`"YES"`. 

Một lần hoán đổi duy nhất ảnh hưởng đến chính xác hai vị thế. Không có điểm không khớp nào có nghĩa là mảng đã được sắp xếp, trong khi hai điểm không khớp tương ứng với hai vị trí liên quan đến việc hoán đổi. 
5. Nếu không, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Hãy để bản sao được sắp xếp$b$. 

Nếu mảng ban đầu có thể được sắp xếp bằng cách sử dụng tối đa một lần hoán đổi thì tất cả các vị trí ngoại trừ có thể là hai vị trí được hoán đổi phải khớp nhau$b$. Do đó số lượng không khớp giữa mảng ban đầu và$b$không thể vượt quá 2. 

Ngược lại, nếu có chính xác hai vị trí không khớp, việc trao đổi giá trị tại các vị trí đó sẽ khiến cả hai vị trí khớp đồng thời với mảng đã sắp xếp. Nếu không có điểm nào không khớp thì mảng đã được sắp xếp. 

Do đó, mảng có thể được sắp xếp trong nhiều nhất một lần hoán đổi khi và chỉ khi số lượng không khớp nhiều nhất là 2. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    b = sorted(a)

    mismatches = 0
    for x, y in zip(a, b):
        if x != y:
            mismatches += 1

    print("YES" if mismatches <= 2 else "NO")

if __name__ == "__main__":
    solve()
```The implementation follows the algorithm directly.

 Bản sao được sắp xếp`b`là sự sắp xếp mục tiêu. Sau đó, chúng tôi quét cả hai mảng cùng lúc và đếm các vị trí có giá trị khác nhau. 

Quan sát quan trọng là chúng tôi tính các vị trí không khớp thay vì đảo ngược. Các bản sao có thể làm cho lý luận dựa trên đảo ngược không đáng tin cậy, trong khi so sánh với mảng đã sắp xếp sẽ nắm bắt chính xác vị trí nào không chính xác. 

Không cần xử lý đặc biệt đối với các mảng đã được sắp xếp. Trong trường hợp đó, số lượng không khớp bằng 0 và điều kiện`mismatches <= 2`tự nhiên trở lại`"YES"`. 

Số nguyên Python dễ dàng xử lý các giá trị lên tới$10^9$, do đó không có mối lo ngại tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
```Bản sao được sắp xếp:```
1 2
```| Vị trí | Bản gốc | Đã sắp xếp | Số lượng không khớp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 2 | 0 | 

Số lượng không khớp cuối cùng là 0. 

Đầu ra:```
YES
```Điều này chứng tỏ trường hợp không cần trao đổi. 

### Ví dụ 2 

đầu vào:```
3
3 1 2
```Bản sao được sắp xếp:```
1 2 3
```| Vị trí | Bản gốc | Đã sắp xếp | Số lượng không khớp | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 
| 2 | 1 | 2 | 2 | 
| 3 | 2 | 3 | 3 | 

Số lượng không khớp cuối cùng là 3. 

Đầu ra:```
NO
```Ba vị trí không đồng ý với mảng được sắp xếp. Vì một lần hoán đổi chỉ có thể ảnh hưởng đến hai vị trí nên việc sắp xếp trong một lần hoán đổi là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |$O(n)$| Bản sao đã sắp xếp của mảng được lưu trữ | 

Với$n \le 10^5$, MỘT$O(n \log n)$giải pháp chạy thoải mái trong giới hạn. Mảng kích thước bổ sung$n$cũng dễ dàng phù hợp trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    b = sorted(a)
    mismatches = sum(x != y for x, y in zip(a, b))

    return ("YES" if mismatches <= 2 else "NO") + "\n"

# provided sample
assert run("2\n1 2\n") == "YES\n", "sample 1"

# one swap fixes array
assert run("3\n2 1 3\n") == "YES\n", "single swap"

# requires more than one swap
assert run("4\n4 3 2 1\n") == "NO\n", "multiple swaps needed"

# all equal values
assert run("5\n7 7 7 7 7\n") == "YES\n", "all equal"

# duplicates with one valid swap
assert run("4\n1 3 2 2\n") == "YES\n", "duplicates"

# minimum size, unsorted
assert run("2\n2 1\n") == "YES\n", "minimum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 2 1`| CÓ | Mảng không tầm thường nhỏ nhất | 
|`4 / 4 3 2 1`| KHÔNG | Nhiều hơn hai điểm không khớp | 
|`5 / 7 7 7 7 7`| CÓ | Tất cả các giá trị giống nhau | 
|`4 / 1 3 2 2`| CÓ | Các giá trị trùng lặp được xử lý chính xác | 
|`3 / 2 1 3`| CÓ | Chính xác là cần một lần hoán đổi | 

## Vỏ cạnh 

Hãy xem xét một mảng đã được sắp xếp:```
5
1 2 2 3 4
```Bản sao được sắp xếp giống hệt nhau.```
Original: 1 2 2 3 4
Sorted:   1 2 2 3 4
```Số lượng không khớp là 0, do đó thuật toán sẽ in:```
YES
```Điều này phù hợp với yêu cầu không cho phép hoán đổi. 

Xem xét các bản sao:```
4
1 3 2 2
```Bản sao được sắp xếp là:```
1 2 2 3
```So sánh các vị trí:```
1 = 1
3 ≠ 2
2 = 2
2 ≠ 3
```Có đúng hai điểm không khớp. Hoán đổi hai vị trí đó sẽ mang lại mảng được sắp xếp, do đó thuật toán sẽ đưa ra:```
YES
```Hãy xem xét một trường hợp cần nhiều hơn một lần trao đổi:```
4
4 3 2 1
```Bản sao được sắp xếp:```
1 2 3 4
```Mỗi vị trí đều khác nhau, tạo ra bốn điểm không khớp. Vì một lần hoán đổi có thể sửa chữa tối đa hai vị trí nên thuật toán sẽ đưa ra:```
NO
```Tiêu chí đếm không khớp nắm bắt chính xác tất cả các trường hợp như vậy.
