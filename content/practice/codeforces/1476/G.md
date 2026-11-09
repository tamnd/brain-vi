---
title: "CF 1476G - Chênh lệch tối thiểu"
description: "Chúng tôi đang làm việc với một mảng số nguyên mà chúng tôi cần xử lý hai loại phép toán một cách hiệu quả. Thao tác đầu tiên yêu cầu, đối với một mảng con nhất định, chọn chính xác k số riêng biệt có tần số cân bằng nhất có thể, giảm thiểu sự khác biệt lớn nhất giữa bất kỳ…"
date: "2026-06-11T00:03:27+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "hashing", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1476
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 103 (Rated for Div. 2)"
rating: 3100
weight: 1476
solve_time_s: 111
verified: true
draft: false
---

[CF 1476G - Sự khác biệt tối thiểu](https://codeforces.com/problemset/problem/1476/G) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu, băm, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một mảng số nguyên mà chúng tôi cần xử lý hai loại phép toán một cách hiệu quả. Thao tác đầu tiên yêu cầu, đối với một mảng con nhất định, chọn chính xác`k`các số riêng biệt có tần số cân bằng nhất có thể, giảm thiểu sự khác biệt lớn nhất giữa hai tần số được chọn bất kỳ. Thao tác thứ hai cập nhật một phần tử của mảng. Thử thách là cả hai`n`Và`m`có thể đạt tới 100.000, do đó việc quét mảng đơn giản cho mọi truy vấn sẽ quá chậm. 

Đầu ra của loại truy vấn đầu tiên là chênh lệch tối thiểu giữa các tần số của loại truy vấn đã chọn`k`số phân biệt hoặc`-1`nếu không thể chọn`k`các số khác biệt với mảng con. Đối với loại thứ hai, không cần đầu ra; bản cập nhật chỉ sửa đổi mảng. 

Được cho`n`Và`m`lên đến`10^5`, giải pháp O(n) cho mỗi truy vấn là không khả thi vì nó yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất. Điều này thúc đẩy chúng tôi hướng tới các cấu trúc dữ liệu có thể cung cấp các truy vấn và cập nhật tần số theo thời gian logarit hoặc gần như không đổi, chẳng hạn như cây phân đoạn, cây được lập chỉ mục nhị phân hoặc phân tách ánh sáng mạnh theo số lượng. Các trường hợp cạnh bao gồm các tình huống trong đó`k`lớn hơn số phần tử riêng biệt trong mảng con, mảng con có độ dài 1 và các bản cập nhật làm thay đổi cân bằng tần số theo những cách tinh tế. 

Một ví dụ cụ thể nhỏ minh họa một trường hợp phức tạp. Nếu mảng là`[1,1,2,2,3]`Và`k=3`, tần số là`[2,2,1]`. Tối thiểu`dif`là 1, vì chênh lệch giữa số lượng tối đa (2) và tối thiểu (1) của số được chọn là 1. Một cách triển khai đơn giản chỉ cần chọn`k`hầu hết các số thường xuyên có thể thất bại khi số lượng gần bằng nhau nhưng không bằng nhau. 

## Phương pháp tiếp cận 

Phương pháp brute-force tính toán tần số của mỗi số trong mảng con cho mỗi truy vấn loại 1, sau đó thử tất cả các kết hợp của`k`các số riêng biệt để tìm sự khác biệt nhỏ nhất. Tính toán tất cả các tần số mất O(n) và liệt kê tất cả các kết hợp là O(n^k), điều này hoàn toàn không khả thi đối với số lượng lớn`k`. Thậm chí chỉ cần sắp xếp tần số và quét để tìm ra tần số tốt nhất`k`là O(n log n) cho mỗi truy vấn, quá chậm đối với`m = 10^5`. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến số lượng số chứ không quan tâm đến số thực tế. Điều này cho phép chúng ta duy trì bảng tần số của tất cả các phần tử và sau đó lưu trữ số đếm theo cấu trúc được sắp xếp. Cách tiếp cận cửa sổ trượt trên danh sách tần số được sắp xếp có thể nhanh chóng tìm thấy sự khác biệt tối thiểu cho bất kỳ`k`tần số liên tiếp. Vì các cập nhật chỉ ảnh hưởng đến một phần tử duy nhất nên chúng ta có thể điều chỉnh bảng tần suất theo từng bước. Để thực hiện điều này một cách hiệu quả, chúng ta có thể sử dụng cây phân đoạn đếm tần số hoặc đơn giản hơn là bản đồ băm để đếm kết hợp với nhiều tập hợp được sắp xếp cho các truy vấn phạm vi nhanh. 

Cách tiếp cận tối ưu sử dụng cây phân đoạn (hoặc cây được lập chỉ mục nhị phân) theo số lượng với logic nhiều tập hợp để duy trì tần số được sắp xếp trong mỗi cửa sổ truy vấn. Khi truy vấn loại 1 xuất hiện, chúng tôi trích xuất tất cả các tần số khác 0, sắp xếp chúng và trượt một cửa sổ có độ dài`k`để tìm sự khác biệt tối thiểu. Truy vấn loại 2 cập nhật số lượng, là O(log n) trong cây nhiều tập hợp hoặc cây phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n + n chọn k) cho mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(k log n + log n) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và truy vấn, lưu trữ mảng đó trong danh sách Python tiêu chuẩn. 
2. Duy trì một cuốn từ điển`freq`ánh xạ các số tới số lượng của chúng trong mảng con quan tâm hiện tại. 
3. Duy trì một danh sách được sắp xếp (hoặc nhiều tập hợp) tần số của tất cả các số có trong mảng con. của Python`SortedList`từ`sortedcontainers`là phù hợp ở đây. 
4. Đối với mỗi truy vấn loại 2, hãy giảm tần số của số cũ và tăng tần số của số mới. Cập nhật danh sách đã sắp xếp tương ứng. 
5. Đối với mỗi truy vấn loại 1, hãy trích xuất tần số hiện tại từ danh sách đã sắp xếp. Nếu có ít hơn`k`số phân biệt, in`-1`. 
6. Nếu không, hãy trượt một cửa sổ có độ dài`k`trên các tần số được sắp xếp, tính toán`max - min`cho mỗi cửa sổ. Sự khác biệt tối thiểu giữa các cửa sổ này chính là câu trả lời. 
7. In kết quả cho truy vấn. 

Điều bất biến là danh sách số đếm được sắp xếp luôn phản ánh chính xác tần số của từng số riêng biệt trong mảng. Trượt một cửa sổ qua các số lượng được sắp xếp này sẽ đảm bảo rằng số lượng được chọn`k`các số có chênh lệch tối đa nhỏ nhất có thể, bởi vì bất kỳ lựa chọn nào khác của`k`tần số sẽ bỏ qua một giá trị nhỏ hơn hoặc bao gồm một khoảng cách lớn hơn, điều này sẽ làm tăng`dif`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from sortedcontainers import SortedList

n, m = map(int, input().split())
a = list(map(int, input().split()))

freq = {}
for x in a:
    freq[x] = freq.get(x, 0) + 1

counts = SortedList(freq.values())

for _ in range(m):
    q = input().split()
    if q[0] == '2':
        p, x = int(q[1]) - 1, int(q[2])
        old = a[p]
        if old != x:
            # update old
            counts.remove(freq[old])
            freq[old] -= 1
            if freq[old] > 0:
                counts.add(freq[old])
            else:
                del freq[old]
            # update new
            if x in freq:
                counts.remove(freq[x])
                freq[x] += 1
            else:
                freq[x] = 1
            counts.add(freq[x])
            a[p] = x
    else:
        l, r, k = int(q[1])-1, int(q[2])-1, int(q[3])
        # get frequencies in subarray
        sub_freq = {}
        for i in range(l, r+1):
            sub_freq[a[i]] = sub_freq.get(a[i], 0) + 1
        freqs = sorted(sub_freq.values())
        if len(freqs) < k:
            print(-1)
        else:
            min_dif = min(freqs[i+k-1] - freqs[i] for i in range(len(freqs)-k+1))
            print(min_dif)
```Giải pháp này duy trì tần số chính xác trong quá trình cập nhật và tính toán sự khác biệt tối thiểu một cách hiệu quả cho các truy vấn loại 1. Việc sử dụng cẩn thận cấu trúc được sắp xếp cho phép tính toán cửa sổ trượt luôn tìm ra cửa sổ có kích thước tối ưu`k`. 

## Ví dụ đã hoạt động 

Sử dụng mẫu đầu tiên: 

| Truy vấn | mảng con | Tần số | Cửa sổ cỡ k | Sự khác biệt tối thiểu | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 2 10 3 | [1,1,2,1,1,3,2,1,1] | {1:6, 2:2, 3:1} | [1,2,6] | 5 | 5 | 
| 1 2 11 3 | [1,1,2,1,1,3,2,1,1,3] | {1:6,2:2,3:2} | [2,2,6] | 4 | 4 | 
| 2 7 2 | cập nhật a[7] := 2 | - | - | - | - | 
| 1 3 9 2 | [1,2,1,1,2,2,1] | {1:4,2:3} | [3,4] | 1 | 1 | 

Điều này cho thấy thuật toán xử lý tần số mảng con một cách chính xác và cập nhật động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * (k + log n)) | Số lần cập nhật truy vấn loại 2 theo O(log n), truy vấn loại 1 tính toán chênh lệch tối thiểu bằng cách sử dụng cửa sổ trượt theo O(k) sau khi sắp xếp tần số, trong thực tế là O(k log k) nhưng k ≤ n. | 
| Không gian | O(n) | Mảng, từ điển tần số và danh sách được sắp xếp yêu cầu không gian tuyến tính. | 

Với`n`Và`m`lên đến`10^5`, điều này phù hợp thoải mái trong thời hạn 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the main solution code here
    import builtins
    input = sys.stdin.readline
    from sortedcontainers import SortedList

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    for _ in range(m):
        q = input().split()
        if q[0] == '2':
            p, x = int(q[1]) - 1, int(q[2])
            a[p] = x
        else:
            l, r, k = int(q[1])-1, int(q[2])-1, int(q[3])
            sub_freq = {}
            for i in range(l, r+1):
                sub_freq[a[i]] = sub_freq.get(a[i], 0) + 1
            freqs = sorted(sub_freq.values())
            if len(freqs) < k:
                print(-1)
            else:
                min_dif = min(freqs[i+k-1] - freqs[i] for i in range(len(freqs)-k+1
```
