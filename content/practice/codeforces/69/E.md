---
title: "CF 69E - Phân đoạn"
description: "Chúng ta được cung cấp một mảng các số nguyên và kích thước cửa sổ cố định k. Đối với mỗi mảng con (đoạn) liền kề có độ dài k, chúng ta cần tìm phần tử lớn nhất xuất hiện đúng một lần trong đoạn đó. Nếu không có phần tử nào xuất hiện đúng một lần, chúng ta sẽ xuất ra \"Không có gì\"."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 69
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 63 (Div. 2)"
rating: 1800
weight: 69
solve_time_s: 82
verified: true
draft: false
---

[CF 69E - Phân đoạn](https://codeforces.com/problemset/problem/69/E) 

**Đánh giá:** 1800 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên và kích thước cửa sổ cố định`k`. Đối với mỗi mảng con (đoạn) liền kề có độ dài`k`, chúng ta cần tìm phần tử lớn nhất xuất hiện đúng một lần trong đoạn đó. Nếu không có phần tử nào xuất hiện đúng một lần, chúng ta sẽ xuất ra "Không có gì". 

Đầu vào bắt đầu với kích thước mảng`n`và độ dài đoạn`k`, theo sau là`n`số nguyên. Đầu ra là`n-k+1`dòng, mỗi dòng tương ứng với một đoạn. 

Những hạn chế là đáng kể:`n`có thể lên tới 100.000, điều này loại trừ bất kỳ giải pháp nào thực hiện được nhiều hơn mức đại khái`O(n log n)`hoặc`O(n)`làm việc trên mỗi trường hợp thử nghiệm nếu chúng tôi muốn duy trì dưới 1 giây. Một cách tiếp cận đơn giản để kiểm tra tính duy nhất của tất cả các phần tử trong mỗi phân đoạn sẽ là`O(n*k)`, có thể đạt tới 10 tỷ thao tác trong trường hợp xấu nhất và sẽ hết thời gian chờ. 

Trường hợp cạnh không rõ ràng xảy ra khi một đoạn có tất cả các phần tử bị trùng lặp. Ví dụ: nếu phân đoạn là`[2, 2, 2]`, không có phần tử nào xuất hiện đúng một lần. Việc triển khai bất cẩn có thể cố gắng trả về mức tối đa và tạo ra`2`thay vì "Không có gì". Một trường hợp khác là khi mảng có số âm hoặc giá trị lớn; thuật toán phải so sánh chúng một cách chính xác mà không giả định tính tích cực. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp đi lặp lại trên mọi đoạn dài`k`. Đối với mỗi phân đoạn, chúng tôi đếm tần số của từng số và sau đó chọn mức tối đa trong số các số có tần số chính xác là một. Cách này hiệu quả vì nó đảm bảo kết quả chính xác bằng cách kiểm tra trực tiếp, nhưng độ phức tạp lại rất cao.`O(n*k)`- vì`n=10^5`Và`k=10^5`, thế là quá chậm. 

Quan sát quan trọng để có cách tiếp cận nhanh hơn là chúng ta có thể duy trì một cửa sổ trượt có kích thước`k`và cập nhật tần số khi chúng ta di chuyển từ phân đoạn này sang phân đoạn tiếp theo. Thay vì tính toán lại tần số từ đầu cho mỗi phân đoạn, chúng tôi giảm số lượng phần tử rời khỏi cửa sổ và tăng số lượng phần tử đi vào cửa sổ. Điều này làm giảm công việc mỗi bước về cơ bản`O(log k)`nếu chúng tôi duy trì tập hợp các ứng cử viên trong cấu trúc dữ liệu cho phép trích xuất tối đa một cách nhanh chóng, như BST cân bằng hoặc`SortedSet`. 

Trong bài toán này, chúng tôi sử dụng một`SortedSet`gồm các phần tử xuất hiện đúng một lần trong cửa sổ hiện tại. Bất cứ khi nào tần số của một số thay đổi từ một thành hai, chúng tôi sẽ xóa số đó khỏi tập hợp; khi nó giảm từ hai xuống một, chúng ta thêm nó vào. Mức tối đa của bộ này chính xác là con số chúng ta cần xuất ra. Nếu bộ trống, chúng tôi in "Không có gì". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*k) | O(k) | Quá chậm | 
| Cửa sổ trượt với Bản đồ tần số + Bộ sắp xếp | O(n log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo từ điển tần số để đếm số lần xuất hiện của các phần tử trong cửa sổ hiện tại. Khởi tạo một`SortedSet`(hoặc tương đương) để giữ tất cả các phần tử có tần số chính xác bằng một. 
2. Điền bản đồ tần số và đặt cho cửa sổ độ dài đầu tiên`k`. Đối với mỗi phần tử, hãy tăng số lượng của nó; nếu số đếm trở thành một, hãy thêm nó vào tập hợp; nếu nó trở thành hai, hãy loại bỏ nó. 
3. Xuất giá trị tối đa của tập hợp cho cửa sổ đầu tiên. Nếu tập hợp trống, hãy in "Không có gì". 
4. Trượt cửa sổ từng phần tử một. Đối với mỗi bước, hãy loại bỏ tần số của phần tử rời khỏi cửa sổ: nếu tần số của nó giảm từ hai xuống một, hãy thêm nó trở lại tập hợp; nếu nó giảm từ 1 xuống 0, hãy loại bỏ nó. Sau đó tăng tần suất của phần tử mới vào cửa sổ: nếu nó trở thành một, hãy thêm nó; nếu nó trở thành hai, hãy loại bỏ nó. 
5. Sau khi cập nhật tập hợp cho cửa sổ mới, xuất phần tử tối đa hoặc "Không có gì" nếu tập hợp trống. Lặp lại cho đến khi tất cả các cửa sổ được xử lý. 

Tại sao nó hoạt động: Ở mỗi bước, tập hợp chứa tất cả các số xuất hiện chính xác một lần trong cửa sổ hiện tại. Bằng cách cập nhật tăng dần, chúng tôi không bao giờ bỏ sót các phần tử hoặc bao gồm các phần tử trùng lặp, do đó, mức tối đa của tập hợp được đảm bảo đáp ứng yêu cầu của bài toán. Cửa sổ trượt đảm bảo chúng ta chỉ dành`O(log k)`thời gian mỗi bước nếu sử dụng cấu trúc cân bằng, giữ cho thuật toán hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from sortedcontainers import SortedSet
from collections import defaultdict

n, k = map(int, input().split())
a = [int(input()) for _ in range(n)]

freq = defaultdict(int)
unique = SortedSet()

# initialize first window
for i in range(k):
    freq[a[i]] += 1
for i in range(k):
    if freq[a[i]] == 1:
        unique.add(a[i])
    elif freq[a[i]] == 2:
        unique.discard(a[i])

# output for the first window
print(unique[-1] if unique else "Nothing")

# slide the window
for i in range(k, n):
    left = a[i - k]
    freq[left] -= 1
    if freq[left] == 1:
        unique.add(left)
    elif freq[left] == 0:
        unique.discard(left)

    right = a[i]
    freq[right] += 1
    if freq[right] == 1:
        unique.add(right)
    elif freq[right] == 2:
        unique.discard(right)

    print(unique[-1] if unique else "Nothing")
```Giải pháp bắt đầu bằng cách xây dựng bản đồ tần số cho cửa sổ đầu tiên và`SortedSet`của các phần tử xuất hiện đúng một lần. Khi trượt cửa sổ, chúng tôi cẩn thận cập nhật số lượng và điều chỉnh bộ cho phù hợp. Thứ tự thực hiện các thao tác rất quan trọng: giảm trước, tăng sau vì cửa sổ thay đổi từ trái sang phải. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
5 3
1
2
2
3
3
```| Cửa sổ | tần số | độc đáo | Đầu ra | 
| --- | --- | --- | --- | 
| [1,2,2] | {1:1,2:2} | {1} | 1 | 
| [2,2,3] | {2:2,3:1} | {3} | 3 | 
| [2,3,3] | {2:1,3:2} | {2} | 2 | 

Điều này thể hiện việc xử lý chính xác các bản sao và cập nhật phù hợp khi các phần tử vào và ra khỏi cửa sổ. 

**Đầu vào tùy chỉnh:** 

đầu vào:```
6 2
4
4
5
6
5
7
```| Cửa sổ | tần số | độc đáo | Đầu ra | 
| --- | --- | --- | --- | 
| [4,4] | {4:2} | {} | Không có gì | 
| [4,5] | {4:1,5:1} | {4,5} | 5 | 
| [5,6] | {5:1,6:1} | {5,6} | 6 | 
| [6,5] | {6:1,5:1} | {5,6} | 6 | 
| [5,7] | {5:1,7:1} | {5,7} | 7 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log k) | Cập nhật cửa sổ trượt liên quan đến`SortedSet`các thao tác trên mỗi phần tử, đó là log(size of window) | 
| Không gian | O(k) | Bản đồ tần số và`SortedSet`lưu trữ nhiều nhất`k`yếu tố | 

Giải pháp dễ dàng phù hợp trong giới hạn thời gian 1 giây cho`n=10^5`và sử dụng bộ nhớ dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    f = io.StringIO()
    with redirect_stdout(f):
        # insert solution code here
        import sys
        input = sys.stdin.readline
        from sortedcontainers import SortedSet
        from collections import defaultdict

        n, k = map(int, input().split())
        a = [int(input()) for _ in range(n)]

        freq = defaultdict(int)
        unique = SortedSet()

        for i in range(k):
            freq[a[i]] += 1
        for i in range(k):
            if freq[a[i]] == 1:
                unique.add(a[i])
            elif freq[a[i]] == 2:
                unique.discard(a[i])
        print(unique[-1] if unique else "Nothing")

        for i in range(k, n):
            left = a[i - k]
            freq[left] -= 1
            if freq[left] == 1:
                unique.add(left)
            elif freq[left] == 0:
                unique.discard(left)

            right = a[i]
            freq[right] += 1
            if freq[right] == 1:
                unique.add(right)
            elif freq[right] == 2:
                unique.discard(right)

            print(unique[-1] if unique else "Nothing")
    return f.getvalue().strip()

# provided sample
assert run("5 3\n1\n2\n2\n3\n3\n") == "1\n3\n2"

# custom cases
assert run("2 1\n1\n1\n") == "1\n1", "single element windows"
assert run("3 3\n2\n2\n2\n") == "Nothing", "all duplicates"
assert run("4
```
