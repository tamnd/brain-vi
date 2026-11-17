---
title: "CF 1955D - Tìm kiếm dãy số không chính xác"
description: "Chúng ta được yêu cầu đếm các đoạn con có độ dài m trong mảng a là “tốt” so với mảng b khác có độ dài m. Một phân đoạn được coi là tốt nếu sau khi sắp xếp lại các phần tử của nó, ít nhất k phần tử trong số chúng khớp với các phần tử từ b."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1955
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 938 (Div. 3)"
rating: 1400
weight: 1955
solve_time_s: 46
verified: true
draft: false
---

[CF 1955D - Tìm kiếm dãy số không chính xác](https://codeforces.com/problemset/problem/1955/D) 

**Đánh giá:** 1400 
**Thẻ:** cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các phân đoạn có độ dài`m`trong một mảng`a`đó là “tốt” so với mảng khác`b`chiều dài`m`. Một phân đoạn được coi là tốt nếu sau khi sắp xếp lại các phần tử của nó, ít nhất`k`trong số chúng phù hợp với các yếu tố từ`b`. Trong thực tế, điều này có nghĩa là chúng ta đang kiểm tra xem giao điểm nhiều tập hợp của phân đoạn con và`b`chứa ít nhất`k`các phần tử. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có mảng`a`Và`b`, và số nguyên`n`,`m`,`k`. Tổng kích thước của tất cả`a`mảng trên các trường hợp thử nghiệm không vượt quá`2 * 10^5`, và tương tự cho`b`. Điều này giới hạn tổng số hoạt động chúng ta có thể thực hiện một cách hiệu quả. Bất kỳ giải pháp nào lặp lại trên mỗi phân đoạn và so sánh nó với`b`từng phần tử trong`O(n * m)`thời gian sẽ quá chậm, vì trong trường hợp xấu nhất điều này có thể đạt tới`2 * 10^5 * 2 * 10^5 = 4 * 10^10`hoạt động. 

Các trường hợp cạnh bao gồm các phân đoạn trong đó các phần tử lặp lại,`k = m`(yêu cầu một trận đấu đầy đủ),`k = 1`(yêu cầu kết quả khớp tối thiểu) và`a`hoặc`b`chứa các số lặp lại. Một cách tiếp cận đơn giản có thể đếm các phần tử riêng biệt không chính xác hoặc thất bại khi có nhiều phần tử trùng lặp. Ví dụ, nếu`a = [1,1,1]`Và`b = [1,2,3]`với`k = 2`, thuật toán không được tính sai phân đoạn này là tốt vì chỉ có một`1`TRONG`b`có thể phù hợp với phân khúc. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại trên mọi phân đoạn có độ dài`m`TRONG`a`, và với mỗi cái, hãy tính giao điểm nhiều tập hợp với`b`. Việc đếm các kết quả trùng khớp yêu cầu lặp lại tất cả các phần tử của phân đoạn và so sánh với`b`. Điều này hoạt động chính xác vì nó trực tiếp thực hiện định nghĩa, nhưng nó có`O(n * m)`độ phức tạp cho mỗi trường hợp thử nghiệm và thất bại với số lượng lớn`n`Và`m`. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể duy trì một cửa sổ trượt có độ dài`m`qua`a`và theo dõi xem có bao nhiêu phần tử khớp với những phần tử đó trong`b`bằng cách sử dụng bản đồ tần số. Ban đầu, chúng ta đếm số lần xuất hiện của mỗi số trong`b`trong một cuốn từ điển. Sau đó, khi chúng ta trượt cửa sổ qua`a`, chúng tôi duy trì một từ điển khác để đếm số phần tử trong cửa sổ hiện tại xuất hiện trong`b`. Khi một phần tử đi vào cửa sổ, chúng tôi sẽ tăng số lượng khớp nếu nó có trong`b`và vẫn chưa được khớp hoàn toàn. Khi một phần tử rời khỏi cửa sổ, chúng tôi sẽ giảm số lượng so khớp nếu trước đó nó đã góp phần vào giao điểm. Điều này làm giảm độ phức tạp`O(n)`mỗi trường hợp thử nghiệm sử dụng`O(m)`thêm không gian để theo dõi tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(m) | Quá chậm | 
| Cửa sổ trượt + Bản đồ tần số | O(n) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng từ điển tần số cho mảng`b`. Mỗi khóa là một số và giá trị là số lần nó xuất hiện. Điều này cho phép chúng tôi kiểm tra một cách hiệu quả xem một phần tử trong phân đoạn có thể khớp hay không`b`. 
2. Khởi tạo cửa sổ trượt trên cửa sổ đầu tiên`m`các yếu tố của`a`. Duy trì số lượng phần tử trong cửa sổ khớp với các phần tử trong`b`mà không vượt quá tần số yêu cầu của họ. Đối với mỗi phần tử vào cửa sổ, hãy tăng bộ đếm so khớp nếu phần tử đó tồn tại trong`b`và chưa đạt đến số lượng tối đa từ`b`. 
3. Kiểm tra xem bộ đếm đối sánh cho cửa sổ hiện tại có ít nhất`k`. Nếu vậy, hãy tăng câu trả lời. 
4. Trượt cửa sổ về phía trước từng phần tử một. Đối với mỗi ca, hãy xóa phần tử rời khỏi cửa sổ và thêm phần tử vào. Cập nhật bộ đếm trận đấu theo từ điển tần số. Khi loại bỏ, hãy giảm bộ đếm nếu phần tử đang đóng góp cho kết quả khớp; khi thêm, hãy tăng nó nếu bây giờ nó đóng góp. 
5. Lặp lại cho đến hết mảng. Câu trả lời sẽ là tổng số cửa sổ có ít nhất bộ đếm trận đấu`k`. 

Tại sao nó hoạt động: Ở mỗi bước, bộ đếm khớp phản ánh chính xác số phần tử trong cửa sổ hiện tại có thể tương ứng với các phần tử trong`b`không vượt quá tần số của chúng. Trượt cửa sổ sẽ cập nhật số đếm này một cách hiệu quả, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        b_count = Counter(b)
        window_count = Counter()
        matches = 0
        result = 0

        # Initialize first window
        for i in range(m):
            x = a[i]
            if x in b_count:
                if window_count[x] < b_count[x]:
                    matches += 1
                window_count[x] += 1
        if matches >= k:
            result += 1

        # Slide the window
        for i in range(m, n):
            out_elem = a[i - m]
            in_elem = a[i]

            if out_elem in b_count:
                if window_count[out_elem] <= b_count[out_elem]:
                    matches -= 1
                window_count[out_elem] -= 1

            if in_elem in b_count:
                if window_count[in_elem] < b_count[in_elem]:
                    matches += 1
                window_count[in_elem] += 1

            if matches >= k:
                result += 1

        print(result)

if __name__ == "__main__":
    solve()
```Giải pháp này bắt đầu bằng cách đếm tần số trong`b`, sau đó trượt một cửa sổ lên`a`, cập nhật các kết quả phù hợp và kiểm tra tình trạng tốt của phân đoạn ở mỗi bước. Các phần tinh tế đang đảm bảo rằng việc tăng và giảm`matches`tôn trọng số lần xuất hiện tối đa được cho phép bởi`b_count`, xử lý các số lặp lại một cách chính xác. Ranh giới cạnh được quản lý bằng cách lập chỉ mục cẩn thận với`i - m`cho phần tử rời khỏi cửa sổ. 

## Ví dụ đã hoạt động 

### Mẫu 1 Trace 

đầu vào:```
7 4 2
4 1 2 3 4 5 6
1 2 3 4
```| Chỉ mục cửa sổ | Cửa sổ | Trận đấu | Kết quả | 
| --- | --- | --- | --- | 
| 0-3 | [4,1,2,3] | 4 | 1 | 
| 1-4 | [1,2,3,4] | 4 | 2 | 
| 2-5 | [2,3,4,5] | 3 | 3 | 
| 3-6 | [3,4,5,6] | 2 | 4 | 

Dấu vết này xác nhận rằng thuật toán đếm chính xác các kết quả khớp cho từng cửa sổ và cập nhật kết quả bất cứ khi nào khớp >= k. 

### Mẫu 2 Dấu vết 

đầu vào:```
7 4 3
4 1 2 3 4 5 6
1 2 3 4
```| Chỉ mục cửa sổ | Cửa sổ | Trận đấu | Kết quả | 
| --- | --- | --- | --- | 
| 0-3 | [4,1,2,3] | 4 | 1 | 
| 1-4 | [1,2,3,4] | 4 | 2 | 
| 2-5 | [2,3,4,5] | 3 | 3 | 
| 3-6 | [3,4,5,6] | 2 | 3 | 

Dấu vết này cho thấy các cửa sổ có ít hơn 3 kết quả khớp sẽ bị bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử vào và ra khỏi cửa sổ đúng một lần. | 
| Không gian | O(m) | Bộ đếm lưu trữ tần số của các phần tử từ`b`và cửa sổ hiện tại. | 

Cho rằng tổng của tất cả`n`trên các trường hợp thử nghiệm là <= 2 * 10^5, giải pháp này phù hợp với giới hạn 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("5\n7 4 2\n4 1 2 3 4 5 6\n1 2 3 4\n7 4 3\n4 1 2 3 4 5 6\n1 2 3 4\n7 4 4\n4 1 2 3 4 5 6\n1 2 3 4\n11 5 3\n9 9 2 2 10 9 7 6 3 6 3\n6 9 7 8 10\n4 1 1\n4 1 5 6\n6\n") == "4\n3\n2\n4\n1"

# Custom cases
assert run("1\n3 3 3\n1 1 1\n1 1 1\n") == "1", "all equal"
assert run("1\n5 2 1\n1 2 3 4 5\n5 6\n") == "2", "minimal k=1"
assert run("1\n6 3 2\n1 2 2
```
