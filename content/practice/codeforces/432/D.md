---
title: "CF 432D - Tiền tố và hậu tố"
description: "Chúng ta được cấp một chuỗi các chữ cái viết hoa. Mục tiêu của chúng tôi là xác định tất cả các tiền tố của chuỗi giống hệt với một số hậu tố và đối với mỗi tiền tố như vậy, hãy đếm số lần nó xuất hiện ở bất kỳ đâu trong chuỗi dưới dạng một chuỗi con liền kề."
date: "2026-06-07T02:36:37+07:00"
tags: ["codeforces", "competitive-programming", "dp", "string-suffix-structures", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 432
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 246 (Div. 2)"
rating: 2000
weight: 432
solve_time_s: 251
verified: true
draft: false
---

[CF 432D - Tiền tố và Hậu tố](https://codeforces.com/problemset/problem/432/D) 

**Xếp hạng:** 2000 
**Tags:** dp, cấu trúc hậu tố chuỗi, chuỗi, hai con trỏ 
**Thời gian giải:** 4 phút 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ cái viết hoa. Mục tiêu của chúng tôi là xác định tất cả các tiền tố của chuỗi giống hệt với một số hậu tố và đối với mỗi tiền tố như vậy, hãy đếm số lần nó xuất hiện ở bất kỳ đâu trong chuỗi dưới dạng một chuỗi con liền kề. Ví dụ, nếu chuỗi là`"ABACABA"`, tiền tố`"A"`cũng là hậu tố có độ dài bằng một và xuất hiện tổng cộng bốn lần trong chuỗi. 

Chuỗi đầu vào có thể dài tới 100.000 ký tự. Một cách tiếp cận đơn giản là kiểm tra mọi tiền tố có thể có đối với mọi chuỗi con có thể sẽ thực hiện theo thứ tự$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thời gian một giây. Điều này cho chúng ta biết rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Các trường hợp cạnh có thể gây khó khăn cho việc triển khai đơn giản bao gồm các chuỗi trong đó mọi ký tự đều giống nhau, chẳng hạn như`"AAAAA"`. Trong trường hợp này, nhiều tiền tố có độ dài khác nhau khớp với hậu tố và số lượng của chúng tăng lên khi xuất hiện chồng chéo. Một giải pháp bất cẩn có thể đếm sai các chuỗi con chồng chéo hoặc bỏ qua các tiền tố có độ dài bằng toàn bộ chuỗi. Tương tự, các chuỗi không có mẫu lặp lại, như`"ABCDEFG"`, yêu cầu thuật toán chỉ trả về chính xác các kết quả phù hợp có độ dài 1 nếu có mà không xác định sai các kết quả phù hợp không tồn tại dài hơn. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên tất cả các độ dài tiền tố có thể có. Đối với mỗi tiền tố, nó kiểm tra xem nó có khớp với hậu tố tương ứng có cùng độ dài hay không. Sau đó nó quét toàn bộ chuỗi để đếm số lần tiền tố này xuất hiện. Mặc dù đúng nhưng cách tiếp cận này có độ phức tạp khoảng$O(n^2)$, bởi vì mỗi lần kiểm tra tiền tố đều yêu cầu quét chuỗi và tổng số tiền tố là$O(n)$. Điều này là không thực tế đối với$n = 10^5$. 

Cách tiếp cận tối ưu tận dụng thực tế là chúng ta có thể tính toán các lần xuất hiện chuỗi con lặp lại một cách hiệu quả bằng cách sử dụng hàm tiền tố từ thuật toán Knuth-Morris-Pratt (KMP). Hàm tiền tố tại vị trí$i$cho chúng ta biết độ dài của tiền tố thích hợp dài nhất của chuỗi con$s[0..i]$đó cũng là hậu tố của chuỗi con đó. Sử dụng hàm này, chúng ta có thể xác định tất cả các độ dài trong đó tiền tố khớp với hậu tố theo thời gian tuyến tính. Hơn nữa, bằng cách đếm số lần mỗi độ dài tiền tố xuất hiện trong mảng hàm tiền tố, chúng ta có thể xác định tần suất mỗi chuỗi con ứng cử viên xuất hiện dưới dạng chuỗi con. Điều này làm giảm vấn đề xuống$O(n)$thời gian. 

Brute-force hoạt động vì nó trực tiếp thực hiện định nghĩa vấn đề, nhưng nó không thành công đối với các chuỗi dài. Quan sát cho thấy hàm tiền tố mã hóa tất cả các mối quan hệ tiền tố-hậu tố lặp lại cho phép chúng ta giảm cả nhận dạng tiền tố-hậu tố và đếm chuỗi con thành một lần truyền tuyến tính trên chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Hàm KMP / Tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính hàm tiền tố cho chuỗi. Đối với mỗi vị trí`i`,`pi[i]`lưu trữ độ dài của tiền tố thích hợp dài nhất của`s[0..i]`khớp với một hậu tố kết thúc tại`i`. Hàm này nắm bắt tất cả các mối quan hệ tiền tố-hậu tố lặp lại. 
2. Đếm tần số của từng độ dài tiền tố trên chuỗi bằng cách sử dụng mảng hàm tiền tố. Khởi tạo một mảng đếm có kích thước`n+1`với số không. Đối với mỗi`pi[i]`, tăng`count[pi[i]]`. Điều này sẽ giúp chúng ta sau này tính toán số lần mỗi tiền tố xuất hiện dưới dạng một chuỗi con. 
3. Truyền số đếm từ tiền tố dài hơn đến tiền tố ngắn hơn. Vì tiền tố có độ dài`x`cũng đóng góp vào tất cả các tiền tố là tiền tố thích hợp của nó, lặp từ độ dài lớn nhất đến nhỏ nhất:`count[pi[i]] += count[i+1]`. Điều này đảm bảo rằng mọi tiền tố đều bao gồm tất cả các lần xuất hiện của các tiền tố chồng chéo dài hơn. 
4. Xác định tất cả độ dài tiền tố cũng là hậu tố. Bắt đầu từ độ dài chuỗi đầy đủ và tuân theo chuỗi hàm tiền tố:`length = n`, sau đó`length = pi[length-1]`, lặp lại cho đến khi`length == 0`. Thu thập các độ dài này. 
5. Với mỗi độ dài tiền tố được thu thập, số lần xuất hiện là`count[length] + 1`, vì theo định nghĩa, mỗi tiền tố xuất hiện ít nhất một lần. Xuất chúng theo thứ tự độ dài tăng dần. 

Tại sao nó hoạt động: hàm tiền tố mã hóa tất cả các cấu trúc tiền tố-hậu tố lặp lại theo thời gian tuyến tính. Bằng cách truyền số đếm từ tiền tố dài hơn đến tiền tố ngắn hơn, chúng tôi tính đến các lần xuất hiện chồng chéo. Chuỗi theo sau`pi[length-1]`đảm bảo rằng chúng tôi tìm thấy tất cả các tiền tố cũng là hậu tố, được sắp xếp bằng cách đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

# Step 1: Compute prefix function
pi = [0] * n
for i in range(1, n):
    j = pi[i-1]
    while j > 0 and s[i] != s[j]:
        j = pi[j-1]
    if s[i] == s[j]:
        j += 1
    pi[i] = j

# Step 2: Count occurrences of each prefix length
count = [0] * (n + 1)
for length in pi:
    count[length] += 1

# Step 3: Propagate counts
for i in range(n, 0, -1):
    count[pi[i-1]] += count[i]

# Step 4: Collect prefix-suffix matches
lengths = []
k = n
while k > 0:
    lengths.append(k)
    k = pi[k-1]
lengths.sort()

# Step 5: Output results
print(len(lengths))
for l in lengths:
    print(l, count[l] + 1)
```Tính toán hàm tiền tố trong vòng lặp đầu tiên là KMP tiêu chuẩn. Bước 2 khởi tạo một mảng tần số. Bước 3 truyền các lần xuất hiện từ tiền tố dài hơn đến tiền tố phụ thích hợp của chúng, tính đến các kết quả trùng khớp. Bước 4 lặp lại chuỗi hàm tiền tố để tìm tất cả các tiền tố là hậu tố. Sắp xếp đảm bảo đầu ra theo thứ tự tăng dần. các`+1`ở Bước 5 tính đến tiền tố xuất hiện ít nhất một lần. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`"ABACABA"`| tôi | s[i] | pi[i] | đếm sau Bước 2 | 
| --- | --- | --- | --- | 
| 0 | A | 0 | [0,0,0,0,0,0,0,0] | 
| 1 | B | 0 | giống nhau | 
| 2 | A | 1 | [0,1,0,0,0,0,0,0] | 
| 3 | C | 0 | [0,1,0,0,0,0,0,0] | 
| 4 | A | 1 | [0,2,0,0,0,0,0,0] | 
| 5 | B | 2 | [0,2,1,0,0,0,0,0] | 
| 6 | A | 3 | [0,2,1,1,0,0,0,0] | 

Sau khi truyền bá số đếm và truy tìm chuỗi tiền tố-hậu tố, độ dài`[1, 3, 7]`được xác định. Số lần xuất hiện của chúng là`[4, 2, 1]`, phù hợp với đầu ra mẫu. 

Đầu vào tùy chỉnh:`"AAAA"`| tôi | pi[i] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 

Việc truyền bá số đếm sẽ cho số lượng`[4,3,2,1]`và chuỗi tiền tố-hậu tố mang lại độ dài`[1,2,3,4]`với những lần xuất hiện`[4,3,2,1]`. 

Những dấu vết này xác nhận tính chính xác của các lần lặp lại chồng chéo và khớp chuỗi đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán hàm tiền tố là tuyến tính; sự lan truyền đếm và truy tìm hậu tố tiền tố cũng là tuyến tính | 
| Không gian | O(n) | Mảng`pi`Và`count`sử dụng`O(n)`không gian | 

Được cho$n \le 10^5$và hoạt động$O(n)$, giải pháp này chạy thoải mái dưới 1 giây và sử dụng ít hơn giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("ABACABA\n") == "3\n1 4\n3 2\n7 1", "sample 1"

# Minimum-size input
assert run("A\n") == "1\n1 1", "single character"

# All equal characters
assert run("AAAA\n") == "4\n1 4\n2 3\n3 2\n4 1", "all same"

# No repeating prefix-suffix
assert run("ABCDEFG\n") == "1\n1 1", "no repeats"

# Overlapping occurrences
assert run("ABABAB\n") == "3\n2 3\n4 2\n6 1", "overlapping repeats"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"A"`|`"1\n1 1"`| Xử lý chuỗi tối thiểu một cách chính xác | 
|`"AAAA"`| `"4\n1 4 | |
