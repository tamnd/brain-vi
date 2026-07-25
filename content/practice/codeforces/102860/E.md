---
title: "CF 102860E - Cờ có các ngôi sao"
description: "Lá cờ được làm từ n ngôi sao xếp thành hàng ngang. Các hàng không nhất thiết phải chứa chính xác cùng số lượng ngôi sao, nhưng mỗi kích thước hàng phải khác với mọi kích thước hàng khác tối đa một."
date: "2026-07-25T14:12:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102860
codeforces_index: "E"
codeforces_contest_name: "2020-2021 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 20)"
rating: 0
weight: 102860
solve_time_s: 50
verified: true
draft: false
---

[CF 102860E - Cờ có ngôi sao](https://codeforces.com/problemset/problem/102860/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Lá cờ được làm từ`n`các ngôi sao xếp thành hàng ngang. Các hàng không nhất thiết phải chứa chính xác cùng số lượng ngôi sao, nhưng mỗi kích thước hàng phải khác với mọi kích thước hàng khác tối đa một. Nếu sử dụng hai kích thước hàng khác nhau thì các hàng có cùng kích thước không thể chạm vào nhau nên hai kích thước đó phải xen kẽ nhau. Trong số tất cả các cách sắp xếp có thể, chúng ta cần tìm sự khác biệt nhỏ nhất có thể có giữa số hàng và số sao tối đa trong bất kỳ hàng nào. 

Đầu vào chứa một số nguyên duy nhất`n`, số lượng sao có sẵn. Đầu ra là chênh lệch tuyệt đối tối thiểu có thể có giữa chiều cao và chiều rộng của lá cờ đó. 

Giá trị của`n`có thể lớn như`10^12`, vì vậy việc lặp lại mọi số hàng có thể là không thể. Một giải pháp tuyến tính sẽ yêu cầu lên tới`10^12`kiểm tra, vượt xa thời gian có sẵn. Chúng ta cần khai thác cấu trúc toán học của số lượng hàng có thể có. Số giá trị khác nhau của`floor(n / x)`chỉ là về`2 * sqrt(n)`, đủ nhỏ cho giới hạn này. 

Những trường hợp khó khăn xuất hiện khi số lượng sao không thể chia thành các hàng bằng nhau. Ví dụ, với`n = 5`, sử dụng hai hàng sẽ cho kích thước`3`Và`2`, nhưng những hàng đó được phép vì chỉ có hai hàng trong số đó. Việc triển khai bất cẩn có thể từ chối nó vì kích thước hai hàng khác nhau. Câu trả lời đúng là`1`.```
5
```Đầu ra:```
1
```Một trường hợp cạnh khác là khi tất cả các hàng có cùng độ dài. Vì`n = 16`, bốn hàng bốn sao là hợp lệ. Câu trả lời là`0`.```
16
```Đầu ra:```
0
```Giải pháp chỉ kiểm tra các hàng xen kẽ có thể bỏ sót trường hợp này vì không có hai kích thước hàng khác nhau để xen kẽ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi số hàng có thể`r`. Đối với mỗi lựa chọn, độ dài hàng tối đa là`ceil(n / r)`. Chúng ta có thể kiểm tra xem các ngôi sao còn lại có thể được phân bổ theo các hàng có kích thước hay không`ceil(n / r)`Và`floor(n / r)`. Điều này đúng vì mọi cờ hợp lệ phải có chính xác hai kích thước hàng có thể có đó. Tuy nhiên, cố gắng hết sức`r`từ`1`ĐẾN`n`thực hiện`O(n)`séc. Với`n`lên tới`10^12`, điều này là không thể. 

Quan sát quan trọng là số lượng hàng hợp lệ có thể được mô tả bằng thương số`q = floor(n / r)`. Khi`q`đã được sửa, tất cả đều có thể`r`các giá trị tạo thành một khoảng. Chỉ có khoảng`2 * sqrt(n)`thương số khác nhau. 

Giả sử một lá cờ có`r`hàng và`q = floor(n / r)`. Cho phép`rem = n mod r`. 

Nếu như`rem = 0`, mỗi hàng có kích thước`q`, Vì thế`r`phải là`n / q`. 

Nếu không thì có`rem`hàng có kích thước`q + 1`Và`r - rem`hàng có kích thước`q`. Hai kích thước hàng này phải xen kẽ nhau, do đó số lượng của chúng có thể khác nhau tối đa một:```
abs(rem - (r - rem)) <= 1
```Điều này đưa ra ba phương trình có thể:```
rem = r / 2
rem = (r - 1) / 2
rem = (r + 1) / 2
```Thay thế`n = q * r + rem`đưa ra ba ứng cử viên trực tiếp cho`r`:```
r = 2n / (2q + 1)
r = (2n + 1) / (2q + 1)
r = (2n - 1) / (2q + 1)
```Đối với mỗi khoảng thương, chúng ta chỉ cần kiểm tra một vài ứng viên này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các khoảng có giá trị bằng nhau của`floor(n / r)`. Để đếm hàng bắt đầu`l`, tính toán`q = n // l`và số hàng lớn nhất`r`với cùng một thương số. Điều này bỏ qua nhiều hàng không thể đếm được cùng một lúc. 
2. Đối với thương số hiện tại`q`, kiểm tra ứng viên`n / q`. Đây là số hàng duy nhất có thể có trong đó tất cả các hàng có kích thước bằng nhau. 
3. Kiểm tra ba công thức rút ra từ số hàng xen kẽ:```
2n / (2q + 1)
(2n + 1) / (2q + 1)
(2n - 1) / (2q + 1)
```Chỉ các ứng cử viên trong khoảng thương hiện tại mới thực sự có giá trị này`q`. 
4. Đối với mỗi số hàng ứng cử viên, hãy xác minh rằng phân phối hàng là hợp lệ và cập nhật giá trị tối thiểu của`abs(rows - maximum_row_size)`. 

Tại sao nó hoạt động: mọi cờ có thể có một số hàng`r`. Thuật toán truy cập vào khoảng thương số duy nhất chứa giá trị này`r`. Trong khoảng đó, các phương trình trên bao gồm mọi cách có thể mà kích thước hai hàng có thể thay thế. Các hàng có kích thước bằng nhau được xử lý riêng. Vì mọi sắp xếp hợp lệ đều được kiểm tra nên chênh lệch nhỏ nhất được ghi lại là chênh lệch tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n):
    ans = n
    def check(r):
        nonlocal ans
        if r <= 0:
            return
        q = n // r
        rem = n % r
        if rem != 0 and abs(rem - (r - rem)) > 1:
            return
        ans = min(ans, abs(r - ((n + r - 1) // r)))

    l = 1
    while l <= n:
        q = n // l
        r = n // q

        if n % q == 0:
            check(n // q)

        d = 2 * q + 1
        for x in (2 * n, 2 * n + 1, 2 * n - 1):
            if x > 0 and x % d == 0:
                cand = x // d
                if l <= cand <= r:
                    check(cand)

        l = r + 1

    return ans

def main():
    n = int(input())
    print(solve_case(n))

if __name__ == "__main__":
    main()
```Vòng lặp chính nhảy giữa các khoảng thương số thay vì tăng từng hàng một. Khoảng thời gian`[l, r]`chứa tất cả số hàng tạo ra giống nhau`floor(n / rows)`giá trị. 

Trường hợp chia hết được kiểm tra riêng vì tất cả các hàng có thể có cùng kích thước. Đối với các trường hợp còn lại, ba ứng cử viên được tạo ra chính xác là các tình huống mà số lượng hàng lớn hơn và nhỏ hơn có thể thay thế nhau. 

Hàm xác thực tính toán lại phần còn lại và kiểm tra điều kiện xen kẽ. Điều này tránh việc chỉ dựa vào đại số và tránh các ứng cử viên không hợp lệ do phép chia số nguyên gây ra. Số nguyên Python xử lý các giá trị lớn một cách an toàn, do đó không cần xử lý tràn. 

## Ví dụ đã hoạt động 

cho`n = 16`: 

| hàng | thương số | phần còn lại | kích thước hàng tối đa | sự khác biệt | 
| --- | --- | --- | --- | --- | 
| 4 | 4 | 0 | 4 | 0 | 

Thuật toán tìm cách sắp xếp hàng bằng nhau gồm 4 hàng 4 sao, đưa ra đáp án tối ưu`0`. 

Vì`n = 5`: 

| hàng | thương số | phần còn lại | kích thước hàng tối đa | sự khác biệt | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 1 | 3 | 1 | 

Hai hàng chứa ba và hai ngôi sao. Vì chỉ có hai hàng nên các kích cỡ khác nhau sẽ tự động thay thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(n)) | Chỉ có O(sqrt(n)) giá trị khác nhau của`floor(n / r)`và mỗi khoảng thời gian tạo ra một số lần kiểm tra không đổi. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài bộ đếm và câu trả lời hiện tại. | 

Giá trị đầu vào lớn nhất là`10^12`, như vậy khoảng hai triệu khoảng thương đã là giới hạn trên an toàn. Giải pháp tránh mọi vòng lặp phụ thuộc trực tiếp vào`n`. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_case(n):
    ans = n

    def check(r):
        nonlocal ans
        if r <= 0:
            return
        q = n // r
        rem = n % r
        if rem != 0 and abs(rem - (r - rem)) > 1:
            return
        ans = min(ans, abs(r - ((n + r - 1) // r)))

    l = 1
    while l <= n:
        q = n // l
        r = n // q

        if n % q == 0:
            check(n // q)

        d = 2 * q + 1
        for x in (2 * n, 2 * n + 1, 2 * n - 1):
            if x > 0 and x % d == 0:
                cand = x // d
                if l <= cand <= r:
                    check(cand)

        l = r + 1

    return ans

def run(inp: str) -> str:
    return str(solve_case(int(inp.strip()))) + "\n"

assert run("16") == "0\n", "equal rows"
assert run("5") == "1\n", "two alternating rows"
assert run("1") == "0\n", "single star"
assert run("1000000000000") == "0\n", "perfect square boundary"
assert run("50") == "3\n", "non-square large case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`16`|`0`| Kích thước hàng bằng nhau | 
|`5`|`1`| Hai kích thước hàng khác nhau | 
|`1`|`0`| Kích thước tối thiểu | 
|`1000000000000`|`0`| Xử lý số nguyên và đầu vào lớn | 
|`50`|`3`| Sắp xếp hình vuông không hoàn hảo | 

## Vỏ cạnh 

cho`n = 5`, thuật toán đạt đến khoảng thương với`q = 2`. Ứng viên`r = 2`được tạo ra từ công thức xen kẽ. Phần còn lại là`1`, vậy các hàng có kích thước`3`Và`2`. Số lượng của chúng khác nhau một, có nghĩa là cờ hợp lệ và câu trả lời trở thành`1`. 

Vì`n = 16`, ứng viên`r = 4`xuất phát từ trường hợp hàng bằng nhau bởi vì`16`chia hết cho`4`. Phần còn lại bằng 0 nên mỗi hàng chứa bốn ngôi sao. Chiều cao và chiều rộng đều bằng 4, tạo ra câu trả lời`0`. 

Với giá trị lớn như`n = 10^12`, thuật toán không bao giờ thử đếm hàng nghìn tỷ hàng. Nó chỉ truy cập các khoảng thương và số học tương tự hoạt động với các số nguyên lớn. Đây là sự khác biệt giữa giải pháp khả thi và tìm kiếm vũ phu.
