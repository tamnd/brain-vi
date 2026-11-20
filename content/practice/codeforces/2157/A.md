---
title: "CF 2157A - Cân bằng trong ngục tối"
description: "Chúng ta được cho một mảng các số nguyên và mục tiêu là làm cho nó \"cân bằng\". Một mảng được cân bằng nếu mỗi số x xuất hiện trong đó thực hiện chính xác x lần. Vì vậy nếu một số xuất hiện quá ít hoặc quá nhiều lần thì chúng ta cần loại bỏ một số phần tử để khắc phục."
date: "2026-06-08T00:18:15+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2157
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1066 (Div. 1 + Div. 2)"
rating: 800
weight: 2157
solve_time_s: 188
verified: true
draft: false
---

[CF 2157A - Cân bằng trong ngục tối](https://codeforces.com/problemset/problem/2157/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và mục tiêu là làm cho nó được "cân bằng". Một mảng được cân bằng nếu mỗi số`x`xuất hiện trong đó chính xác như vậy`x`lần. Vì vậy nếu một số xuất hiện quá ít hoặc quá nhiều lần thì chúng ta cần loại bỏ một số phần tử để khắc phục. Ví dụ,`[1, 4, 2, 4, 4, 4, 2]`được cân bằng vì`1`xuất hiện một lần,`2`xuất hiện hai lần và`4`xuất hiện bốn lần. Ngược lại,`[2, 2, 2]`không cân bằng vì`2`xuất hiện ba lần nhưng sẽ xuất hiện chính xác hai lần. 

Đầu vào chứa nhiều trường hợp kiểm thử, mỗi trường hợp có một mảng kích thước`n`và giá trị giữa`0`Và`n`. Nhiệm vụ của chúng ta là tìm số lần xóa tối thiểu cần thiết để đạt được một mảng cân bằng. 

Các ràng buộc khá nhỏ:`n`nhiều nhất là 100 và có tới 500 ca kiểm thử. Điều đó có nghĩa là ngay cả một cách tiếp cận bậc hai trong`n`mỗi trường hợp thử nghiệm là có thể chấp nhận được, bởi vì`100^2 * 500 = 5*10^6`hoạt động, phù hợp thoải mái trong vòng một giây. 

Một số trường hợp có biên không rõ ràng phát sinh khi các số xuất hiện nhiều lần nhưng không thể đạt đến "số cân bằng" hoặc khi`0`xảy ra. Ví dụ, một mảng`[0, 0, 0]`nên được giảm bớt để loại bỏ tất cả các số không vì`0`có thể xuất hiện 0 lần, cho phép xóa tối thiểu 3 lần. Tương tự, một số xuất hiện nhiều lần hơn giá trị của nó, như`[3, 3, 3, 3]`, yêu cầu xóa để giảm số lượng của nó xuống chính xác 3. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con có thể có của mảng và kiểm tra xem chúng có cân bằng hay không. Về mặt lý thuyết, điều này đúng vì chúng ta có thể loại bỏ các phần tử một cách tự do, nhưng số lượng tập hợp con là theo cấp số nhân, khiến nó không khả thi ngay cả đối với`n=20`. 

Cái nhìn sâu sắc quan trọng cho một giải pháp nhanh hơn là mảng chỉ có`n`nhiều nhất là các giá trị khác nhau và chúng ta có thể tập trung vào tần số của mỗi số. Đối với mỗi số`x`, nếu nó xuất hiện`f`nhiều khi, chỉ có hai lựa chọn hợp lý: loại bỏ tất cả các lần xuất hiện của`x`hoặc rời đi chính xác`x`của họ. Việc chọn cái tốt hơn trong hai cái này sẽ giảm thiểu việc xóa cục bộ. Vì các lựa chọn cho mỗi số riêng biệt là độc lập nên chúng ta có thể tính toán số lần xóa tối thiểu bằng cách tính tổng số lần xóa tối ưu cho mỗi số riêng biệt. 

Chúng ta có thể thực hiện điều này một cách hiệu quả bằng cách đếm tần số bằng từ điển hoặc máy tính.`Counter`và lặp lại tất cả các giá trị duy nhất trong mảng. Nếu giá trị bằng 0, chúng tôi luôn xóa tất cả các số 0 vì mảng cân bằng không thể bao gồm số 0. Nếu tần số vượt quá giá trị, chúng tôi sẽ xóa phần thừa; nếu tần số nhỏ hơn giá trị, chúng tôi sẽ xóa tất cả các lần xuất hiện vì chúng tôi không thể "tạo" thêm số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước mảng`n`và chính mảng đó. 
2. Đếm số lần xuất hiện của mỗi số bằng từ điển hoặc`Counter`. Điều này cho chúng ta tần số`f`cho mỗi số duy nhất`x`. 
3. Khởi tạo bộ đếm để xóa. Lặp lại qua từng số duy nhất: 

- Nếu`x`bằng 0, hãy cộng toàn bộ tần số của nó vào các lần xóa, vì các số 0 không thể xuất hiện trong một mảng cân bằng. 
- Nếu như`f < x`, thêm vào`f`để xóa, bởi vì chúng tôi không thể tăng số lần xuất hiện; lựa chọn duy nhất là loại bỏ tất cả. 
- Nếu như`f >= x`, thêm vào`f - x`để xóa, loại bỏ số lần xuất hiện dư thừa để khớp chính xác với số lượng mong muốn. 
4. Sau khi xử lý tất cả các số duy nhất, xuất ra tổng số lần xóa cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: Đối với mỗi số, chúng tôi khớp chính xác giá trị của nó hoặc xóa tất cả các phiên bản. Lựa chọn tối ưu cục bộ này đảm bảo số lần xóa tối thiểu trên toàn cầu vì số lần xuất hiện của các số là độc lập và không thể ảnh hưởng đến tính hợp lệ của nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    freq = Counter(a)
    deletions = 0
    for x, f in freq.items():
        if x == 0:
            deletions += f
        elif f < x:
            deletions += f
        else:
            deletions += f - x
    print(deletions)
```Giải pháp này trước tiên đếm số lần xuất hiện của mỗi số bằng`Counter`. Sau đó, nó lặp qua các số duy nhất và áp dụng logic xóa được mô tả trong hướng dẫn thuật toán. sử dụng`Counter`đảm bảo tính O(n) và lặp lại nhiều nhất`n`phím có hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 2, 2]`| x | f | Hành động | Đã xóa cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 1 | f >= x → xóa f-x = 0 | 0 | 
| 2 | 2 | f >= x → xóa f-x = 0 | 0 | 

Đầu ra: 0. Mảng đã được cân bằng. 

### Ví dụ 2 

đầu vào:`[1, 1, 2, 2, 3]`| x | f | Hành động | Đã xóa cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 2 | f >= x → xóa f-x = 1 | 1 | 
| 2 | 2 | f >= x → xóa f-x = 0 | 1 | 
| 3 | 1 | f < x → xóa tất cả f = 1 | 2 | 

Đầu ra: 2. Xóa một`1`và đĩa đơn`3`để cân bằng. 

Những dấu vết này xác nhận rằng số lần xóa được tính toán độc lập trên mỗi số và khớp với số dư dự định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm bằng`Counter`và việc lặp lại các khóa là tuyến tính theo kích thước mảng. | 
| Không gian | O(n) | các`Counter`lưu trữ tần số cho từng phần tử duy nhất, nhiều nhất là`n`. | 

Với n ∼ 100 và t ∼ 500 thì tổng số phép toán là ∼ 50.000, dễ dàng phù hợp với thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = Counter(a)
        deletions = 0
        for x, f in freq.items():
            if x == 0:
                deletions += f
            elif f < x:
                deletions += f
            else:
                deletions += f - x
        output.append(str(deletions))
    return "\n".join(output)

# provided samples
assert run("4\n3\n1 2 2\n5\n1 1 2 2 3\n10\n1 2 3 2 4 4 4 4 5 2\n1\n0\n") == "0\n2\n3\n1", "sample 1"

# custom cases
assert run("2\n3\n0 0 0\n4\n3 3 3 3\n") == "3\n1", "zeros and overcount"
assert run("1\n5\n5 5 5 5 5\n") == "0", "perfect balance"
assert run("1\n6\n1 1 1 2 2 3\n") == "3", "mixed under and over counts"
assert run("1\n1\n0\n") == "1", "single zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`| 3 | Xử lý đúng số 0 | 
|`3 3 3 3`| 1 | Giảm xuống đúng 3 lần xuất hiện | 
|`5 5 5 5 5`| 0 | Đã cân bằng, không xóa | 
|`1 1 1 2 2 3`| 3 | Số đếm thiếu và đếm thừa hỗn hợp | 
|`0`| 1 | Số 0 đơn được xử lý chính xác | 

## Vỏ cạnh 

Đối với mảng`[0, 0, 0]`,`Counter`cho`{0: 3}`. Thuật toán thêm cả ba vào phần xóa vì số 0 không thể giữ nguyên, mang lại kết quả là 3. Đối với`[3, 3, 3, 3]`,`Counter`cho`{3: 4}`. Thuật toán loại bỏ một để giảm số lượng xuống còn 3, điều này đúng. Đối với một mảng cân bằng hoàn hảo như`[5, 5, 5, 5, 5]`, không cần xóa. Giải pháp xử lý nhất quán các số 0, số đếm thừa và số đếm thiếu.
