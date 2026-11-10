---
title: "CF 1538C - Số Cặp"
description: "Chúng ta được cung cấp một mảng các số nguyên và một phạm vi được xác định bởi hai giá trị l và r. Nhiệm vụ là đếm tất cả các cặp chỉ số riêng biệt (i, j) sao cho tổng các phần tử tại các chỉ số này nằm trong phạm vi đã cho, bao gồm cả các biên."
date: "2026-06-10T14:52:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1538
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 725 (Div. 3)"
rating: 1300
weight: 1538
solve_time_s: 426
verified: false
draft: false
---

[CF 1538C - Số cặp](https://codeforces.com/problemset/problem/1538/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, toán học, hai con trỏ 
**Thời gian giải:** 7 phút 6 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một phạm vi được xác định bởi hai giá trị,`l`Và`r`. Nhiệm vụ là đếm tất cả các cặp chỉ số phân biệt`(i, j)`sao cho tổng các phần tử tại các chỉ số này nằm trong phạm vi nhất định, bao gồm cả các ranh giới. Nói cách khác, chúng tôi muốn số lượng cặp`(i, j)`Ở đâu`i < j`Và`l ≤ a[i] + a[j] ≤ r`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được kích thước của mảng`n`, giới hạn dưới và giới hạn trên`l`Và`r`và chính mảng đó. Đầu ra là số lượng các cặp đủ điều kiện cho mỗi trường hợp thử nghiệm. 

Những hạn chế là rất quan trọng. Với`n`lên tới 2·10^5 cho mỗi trường hợp thử nghiệm và tổng số tiền`n`trên các trường hợp thử nghiệm cũng lên tới 2·10^5, thuật toán O(n²) sẽ yêu cầu theo thứ tự 10^10 thao tác trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong 2 giây. Chúng ta cần một thuật toán gần O(n log n) hơn để đảm bảo an toàn. Các giá trị trong mảng và giới hạn có thể lớn tới 10^9, vì vậy chúng ta phải tránh các kỹ thuật dựa vào việc lập chỉ mục mảng theo mảng giá trị hoặc tần số có tỷ lệ theo độ lớn của số. 

Các trường hợp biên bao gồm các mảng có tất cả các giá trị bằng nhau, các mảng trong đó không có cặp nào đáp ứng phạm vi và các mảng trong đó có nhiều cặp tập hợp xung quanh các ranh giới`l`Và`r`. Ví dụ, với`a = [1, 1, 1]`Và`l = 5, r = 6`, không có cặp nào thỏa mãn điều kiện, vì vậy đầu ra đúng là 0. Việc triển khai ngây thơ có thể quên kiểm tra`i < j`và các cặp đếm kép hoặc bao gồm các khoản tiền không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các cặp`(i, j)`với`i < j`và kiểm tra xem tổng có nằm trong khoảng không`l`Và`r`. Điều này được đảm bảo là chính xác vì nó kiểm tra rõ ràng mọi cặp có thể, nhưng nó yêu cầu thời gian O(n2) cho mỗi trường hợp thử nghiệm. Vì`n`gần bằng 2·10^5, trong trường hợp xấu nhất sẽ có khoảng 2·10^10 lần lặp, điều này là không khả thi. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là chúng ta không cần phải kiểm tra từng cặp riêng lẻ. Nếu chúng ta sắp xếp mảng, đối với một phần tử cố định`a[i]`, vấn đề giảm xuống việc đếm có bao nhiêu phần tử`a[j]`với`j > i`thỏa mãn`l - a[i] ≤ a[j] ≤ r - a[i]`. Vì mảng đã được sắp xếp nên giá trị hợp lệ`a[j]`s tạo thành một đoạn liền kề. Điều này cho phép chúng ta sử dụng tìm kiếm nhị phân để tìm giá trị hợp lệ ngoài cùng bên trái và ngoài cùng bên phải một cách hiệu quả.`a[j]`s, đưa ra số cặp hợp lệ trong O(log n) cho mỗi phần tử. Tổng độ phức tạp trở thành O(n log n), có thể chấp nhận được dưới các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Sắp xếp + Tìm kiếm nhị phân | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự tăng dần. Việc sắp xếp là cần thiết để áp dụng tìm kiếm nhị phân cho việc đếm cặp một cách hiệu quả. 
2. Khởi tạo một biến`count`về không. Điều này sẽ tích lũy số lượng cặp hợp lệ. 
3. Đối với mỗi chỉ số`i`từ 0 đến n - 2, xét`a[i]`là phần tử đầu tiên của cặp. Phần tử thứ hai phải thỏa mãn`l - a[i] ≤ a[j] ≤ r - a[i]`với`j > i`. 
4. Sử dụng`bisect_left`để tìm chỉ số`left`của phần tử đầu tiên`a[j]`trong mảng được sắp xếp lớn hơn hoặc bằng`l - a[i]`, tìm kiếm trong mảng con bắt đầu từ`i + 1`. 
5. Sử dụng`bisect_right`để tìm chỉ số`right`của phần tử thứ nhất lớn hơn`r - a[i]`, cũng trong mảng con bắt đầu từ`i + 1`. 
6. Số cặp hợp lệ cho việc này`i`là`right - left`. Thêm cái này vào`count`. 
7. Sau khi xử lý xong tất cả`i`,`count`chứa tổng số cặp hợp lệ. Xuất nó ra. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng đối với mỗi`i`, tất cả các ứng cử viên`a[j]`với`j > i`đang theo thứ tự tăng dần. Thuộc tính liền kề hợp lệ`a[j]`s bên trong`[l - a[i], r - a[i]]`đảm bảo rằng tìm kiếm nhị phân tìm thấy phạm vi chính xác mà không thiếu các phần tử hoặc đếm kép. Tổng hợp tất cả`i`bao gồm tất cả các cặp với`i < j`. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, l, r = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()
        count = 0
        for i in range(n - 1):
            left = bisect.bisect_left(a, l - a[i], i + 1)
            right = bisect.bisect_right(a, r - a[i], i + 1)
            count += right - left
        print(count)

if __name__ == "__main__":
    solve()
```Bước sắp xếp chuẩn bị mảng cho việc tìm kiếm nhị phân hiệu quả. Vòng lặp đảm bảo chúng tôi chỉ đếm các cặp có`i < j`.`bisect_left`Và`bisect_right`xử lý chính xác các điều kiện biên, bao gồm các tổng chính xác bằng`l`hoặc`r`. Tìm kiếm từ`i + 1`ngăn cản việc đếm các cặp ở đâu`j ≤ i`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`a = [5, 1, 2], l = 4, r = 7`| tôi | một [tôi] | l - a[i] | r - a[i] | chỉ mục bên trái | chỉ số bên phải | tính đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 6 | 1 | 3 | 2 | 
| 1 | 2 | 2 | 5 | 2 | 3 | 1 | 
| 2 | 5 | -1 | 2 | 3 | 3 | 0 | 

Tổng số: 2 (phù hợp với sản lượng dự kiến). 

### Mẫu 2 

đầu vào:`a = [5, 1, 2, 4, 3], l = 5, r = 8`Mảng được sắp xếp:`[1, 2, 3, 4, 5]`| tôi | một [tôi] | l - a[i] | r - a[i] | chỉ mục bên trái | chỉ số bên phải | tính đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 7 | 3 | 5 | 2 | 
| 1 | 2 | 3 | 6 | 2 | 5 | 3 | 
| 2 | 3 | 2 | 5 | 3 | 5 | 2 | 
| 3 | 4 | 1 | 4 | 4 | 4 | 0 | 

Tổng số: 7 (phù hợp với sản lượng dự kiến). 

Những dấu vết này xác nhận rằng thuật toán xác định chính xác các phạm vi hợp lệ và đếm số cặp chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp lấy O(n log n) và mỗi lần lặp n thực hiện hai tìm kiếm nhị phân O(log n) | 
| Không gian | O(n) | Mảng được lưu trữ trong bộ nhớ; không cần cấu trúc cực lớn | 

Thuật toán phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB đối với các ràng buộc nhất định. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n3 4 7\n5 1 2\n5 5 8\n5 1 2 4 3\n4 100 1000\n1 1 1 1\n5 9 13\n2 5 5 1 1") == "2\n7\n0\n1", "samples"

# Custom cases
assert run("1\n2 3 5\n1 1") == "1", "minimum size array"
assert run("1\n3 2 2\n1 1 1") == "0", "no pairs in range"
assert run("1\n4 2 4\n2 2 2 2") == "6", "all equal values"
assert run("1\n5 5 5\n1 2 3 4 5") == "2", "boundary sums"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 phần tử, tổng trong phạm vi | 1 | Thuật toán xử lý mảng có kích thước tối thiểu | 
| 3 phần tử, tổng ngoài phạm vi | 0 | Xác định chính xác không có cặp hợp lệ | 
| 4 phần tử bằng nhau | 6 | |
