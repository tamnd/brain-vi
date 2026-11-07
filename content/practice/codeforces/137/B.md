---
title: "CF 137B - Hoán vị"
description: "Chúng ta được cung cấp một mảng gồm các số nguyên $n$, mỗi số từ 1 đến 5000, và chúng ta được yêu cầu chuyển đổi mảng này thành một hoán vị của các số từ 1 đến $n$. Hoán vị là một dãy trong đó mỗi số nguyên từ 1 đến $n$ xuất hiện đúng một lần."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 137
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 98 (Div. 2)"
rating: 1000
weight: 137
solve_time_s: 73
verified: true
draft: false
---

[CF 137B - Hoán vị](https://codeforces.com/problemset/problem/137/B) 

**Đánh giá:** 1000 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt$n$các số nguyên, mỗi số từ 1 đến 5000 và chúng ta được yêu cầu chuyển mảng này thành một hoán vị của các số từ 1 đến$n$. Hoán vị là một dãy trong đó mỗi số nguyên từ 1 đến$n$xuất hiện đúng một lần. Hoạt động được phép là thay đổi một phần tử thành bất kỳ số nguyên nào khác. Mục tiêu là giảm thiểu số lượng những thay đổi như vậy. 

Đầu vào cho biết độ dài chuỗi$n$và chính mảng đó. Đầu ra là một số nguyên biểu thị số lượng thay thế tối thiểu cần thiết. 

Những hạn chế còn khiêm tốn:$n$có thể lên tới 5000. Từ một người ngây thơ$O(n^2)$có thể liên quan đến việc quét mảng nhiều lần, chúng ta nên hướng tới một giải pháp$O(n \log n)$hoặc$O(n)$. Việc sử dụng bộ nhớ cũng bị hạn chế nhưng việc lưu trữ dải tần có kích thước 5000 là có thể chấp nhận được. 

Các trường hợp đặc biệt bao gồm các chuỗi đã tạo thành một hoán vị hợp lệ, các chuỗi trong đó tất cả các phần tử đều giống nhau hoặc các chuỗi có nhiều số nằm ngoài phạm vi từ 1 đến-$n$phạm vi. Ví dụ, nếu$n = 3$và mảng là$[5, 5, 5]$, kết quả đúng là 3 vì cả ba số phải được đổi thành 1, 2 và 3. Cách tiếp cận bất cẩn chỉ có thể đếm các số trùng lặp mà không kiểm tra giới hạn, dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là lặp qua tất cả các số từ 1 đến$n$và kiểm tra xem mỗi cái có trong mảng hay không. Nếu thiếu, chúng ta có thể quét mảng để tìm phần tử cần thay thế. Về nguyên tắc, điều này hoạt động, nhưng việc quét nhiều lần sẽ làm phức tạp$O(n^2)$trong trường hợp xấu nhất là quá chậm để$n = 5000$. 

Cách tiếp cận tối ưu thúc đẩy việc đếm. Chúng ta quan sát thấy rằng để có một hoán vị hợp lệ, mỗi số từ 1 đến$n$phải xuất hiện đúng một lần. Bất kỳ số nào nằm ngoài phạm vi này hoặc bất kỳ số nào trùng lặp trong phạm vi đều phải được thay đổi. Do đó, chúng ta có thể tính tần số của mỗi số và lặp lại các số từ 1 đến$n$. Mỗi số còn thiếu tương ứng với một thay đổi bắt buộc. Chúng tôi cũng tính các số "thêm" trùng lặp hoặc nằm ngoài giới hạn. Số lần thay đổi tối thiểu là tổng số số bị thiếu, số này sẽ luôn được khớp với đủ số thừa để thay thế. 

Cái nhìn sâu sắc là vấn đề giảm xuống việc đếm số thiếu và số thừa. Việc sắp xếp mảng hoặc cố gắng thực hiện các hoán đổi phức tạp là không cần thiết vì bất kỳ phần tử dư thừa nào cũng có thể được sử dụng lại để lấp đầy chỗ trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng tần số`freq`kích thước$n+1$đếm số lần xuất hiện của các số từ 1 đến$n$. Bây giờ hãy bỏ qua các số nằm ngoài phạm vi này. 
2. Lặp lại mảng đầu vào. Đối với mỗi phần tử$a_i$, nếu nó nằm trong khoảng từ 1 đến$n$, tăng`freq[a_i]`. Nếu không, hãy tính nó là phần tử "thừa", phần tử này cuối cùng sẽ cần được thay thế. 
3. Khởi tạo bộ đếm`changes`về không. Lặp qua các số từ 1 đến$n$. Nếu như`freq[i]`bằng 0, nó có nghĩa là số$i$bị thiếu. Tăng`changes`bởi một. Đối với các số có tần số lớn hơn một, các bản sao bổ sung sẽ góp phần tạo nên nhóm các phần tử dư thừa. 
4. Trở về`changes`. Giá trị này chính xác là số lượng thay thế cần thiết vì mỗi số bị thiếu có thể được điền bằng cách sử dụng phần tử bổ sung từ các số trùng lặp hoặc số nằm ngoài phạm vi. 

Điều bất biến là sau khi đếm tần số, tổng các số bị thiếu bằng số thay thế tối thiểu cần thiết. Không có sự mơ hồ về việc thay đổi phần tử nào vì bất kỳ số thừa nào cũng có thể được thay đổi thành số bị thiếu mà không ảnh hưởng đến các số bị thiếu khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

freq = [0] * (n + 1)
excess = 0

for x in a:
    if 1 <= x <= n:
        freq[x] += 1
    else:
        excess += 1

changes = 0
for i in range(1, n + 1):
    if freq[i] == 0:
        changes += 1

print(changes)
```Đầu tiên, mã sẽ đếm số lần mỗi số trong phạm vi hoán vị hợp lệ xuất hiện. Bất kỳ số nào ngoài 1 đến$n$tự động được coi là vượt quá. Khi đó, với mỗi số từ 1 đến$n$, chúng tôi kiểm tra xem nó có bị thiếu hay không. Mỗi số còn thiếu tương ứng trực tiếp với một thay đổi bắt buộc. Chúng ta không cần theo dõi chính xác số vượt quá nào sẽ được thay đổi vì câu hỏi chỉ yêu cầu số lượng. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
3 1 2
```| Bước | tần số | dư thừa | thay đổi | 
| --- | --- | --- | --- | 
| Ban đầu | [0,0,0,0] | 0 | 0 | 
| Quy trình 3 | [0,0,0,1] | 0 | 0 | 
| Quy trình 1 | [0,1,0,1] | 0 | 0 | 
| Quy trình 2 | [0,1,1,1] | 0 | 0 | 
| Kiểm tra thiếu | không có số 0 trong freq[1..3] | 0 | 0 | 

Đầu ra: 0. Mảng đã là một hoán vị. 

### Đầu vào mẫu 2```
4
4 4 1 5
```| Bước | tần số | dư thừa | thay đổi | 
| --- | --- | --- | --- | 
| Ban đầu | [0,0,0,0,0] | 0 | 0 | 
| Quy trình 4 | [0,0,0,0,1] | 0 | 0 | 
| Quy trình 4 | [0,0,0,0,2] | 0 | 0 | 
| Quy trình 1 | [0,1,0,0,2] | 0 | 0 | 
| Quy trình 5 | ngoài phạm vi | 1 | 0 | 
| Kiểm tra thiếu | tần số[2]=0, tần số[3]=0 | thay đổi = 2 | 2 | 

Kết quả: 2. Chúng ta cần thay thế hai phần tử thừa (một phần tử 4 và 5) bằng 2 và 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để đếm tần số, một lần để đếm số bị thiếu. | 
| Không gian | O(n) | Mảng tần số có kích thước n+1. | 

Điều này phù hợp thoải mái trong giới hạn 2 giây cho$n \le 5000$và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    freq = [0] * (n + 1)
    excess = 0
    for x in a:
        if 1 <= x <= n:
            freq[x] += 1
        else:
            excess += 1
    changes = 0
    for i in range(1, n + 1):
        if freq[i] == 0:
            changes += 1
    return str(changes)

# Provided samples
assert run("3\n3 1 2\n") == "0", "sample 1"
assert run("4\n4 4 1 5\n") == "2", "sample 2"

# Custom cases
assert run("1\n1\n") == "0", "single element correct"
assert run("3\n5 5 5\n") == "3", "all out of range"
assert run("5\n1 2 2 4 5\n") == "1", "one missing in range"
assert run("5\n1 1 1 1 1\n") == "4", "all duplicates"
assert run("5\n2 3 4 6 7\n") == "2", "mix of duplicates and out-of-range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 0 | Đầu vào tối thiểu, đã chính xác | 
| 3\n5 5 5 | 3 | Tất cả các số nằm ngoài phạm vi, tất cả đều phải thay đổi | 
| 5\n1 2 2 4 5 | 1 | Thiếu một số, trùng lặp | 
| 5\n1 1 1 1 1 | 4 | Tất cả các bản sao, cần nhiều thay đổi | 
| 5\n2 3 4 6 7 | 2 | Thiếu 1 và 5, thêm số ngoài phạm vi | 

## Vỏ cạnh 

Nếu dãy có tất cả các số bằng một giá trị nằm ngoài phạm vi hoán vị, chẳng hạn như`[6, 6, 6]`vì`n=3`, thuật toán tính cả ba số thừa và xác định các số còn thiếu 1, 2, 3.`changes`bộ đếm trở thành 3, khớp với số lượng số bị thiếu. 

Nếu chuỗi có các bản sao trong phạm vi, như`[1, 1, 2, 4]`vì`n=4`, thuật toán đếm tần số`[0,2,1,0,1]`và xác định số 3 còn thiếu
