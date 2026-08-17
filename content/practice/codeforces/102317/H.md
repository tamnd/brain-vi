---
title: "CF 102317H - Đếm các cặp chia"
description: "Chúng ta có một danh sách lên tới (p) số nguyên và mọi cặp vị trí có thứ tự ((i,j)) đều là một ứng cử viên. Cặp được tính khi (Ai) là ước số thực sự của (Aj), nghĩa là (Aj) là bội số nguyên của (Ai), nhưng hai giá trị không bằng nhau."
date: "2026-08-16T19:02:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "H"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 449
verified: true
draft: false
---

[CF 102317H - Đếm các cặp chia](https://codeforces.com/problemset/problem/102317/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một danh sách lên tới (p) số nguyên và mọi cặp vị trí có thứ tự ((i,j)) đều là một ứng cử viên. Cặp được tính khi (A_i) là ước số thực sự của (A_j), nghĩa là (A_j) là bội số nguyên của (A_i), nhưng hai giá trị không bằng nhau. Các giá trị bằng nhau không bao giờ tạo thành một cặp chia thích hợp, ngay cả khi giá trị đó xuất hiện nhiều lần. Các giá trị đầu vào nằm trong khoảng từ (0) đến (10^7), trong khi (p) có thể đạt tới (10^6). Tuyên bố chính thức cũng đưa ra một quy tắc đặc biệt cho số 0: số 0 không chia hết gì, trong khi mọi số nguyên khác 0 đều là ước số thực sự của số 0. 

Đầu ra là số cặp chỉ số có thứ tự thỏa mãn điều kiện đó. Số bội quan trọng, vì vậy nếu giá trị (2) xuất hiện ba lần và (6) xuất hiện hai lần, thì những lần xuất hiện đó đóng góp (3\cdot2=6) vào cặp với số chia (2) và số bị chia (6). Định dạng chính thức là`Test case #t: m`, theo sau là một dòng trống. 

Ràng buộc chính làm thay đổi hình dạng của giải pháp. Với (p=10^6), việc kiểm tra từng cặp có thứ tự sẽ yêu cầu kiểm tra tính chia hết (10^{12}) trong trường hợp xấu nhất, vượt xa giới hạn năm giây. Tuy nhiên, các giá trị chỉ được giới hạn bởi (10^7) và phạm vi giá trị giới hạn đó chính xác là thứ cho phép chúng ta thay thế phép liệt kê cặp bằng sàng bội số. Cuộc thi có 5 giây và 256 MB, vì vậy cách tiếp cận dự định cần khai thác giá trị giới hạn thay vì số lượng cặp. 

Có một số trường hợp đặc biệt có thể âm thầm phá vỡ việc triển khai hợp lý. 

Coi như```
1
2
1 1
```Câu trả lời là`0`. Mặc dù (1) chia hết cho (1), nhưng ước số thực sự phải khác với số mà nó chia. Một giải pháp chỉ kiểm tra`N % D == 0`sẽ đếm không chính xác cả hai cặp có thứ tự. 

Coi như```
1
2
0 5
```Câu trả lời là`1`, bởi vì ((5,0)) là một cặp chia thích hợp. Cặp ((0,5)) không hợp lệ vì số 0 không phải là ước của bất kỳ số nào. Một giải pháp đơn giản là kiểm tra`N % D == 0`thậm chí không thể đánh giá trường hợp số chia bằng 0 một cách an toàn. 

Coi như```
1
4
1 2 2 4
```Câu trả lời là`5`. Hai bản sao của (2) là các phần tử riêng biệt, vì vậy mỗi phần tử có thể đóng vai trò là ước số của (4), cho ra hai cặp. Giá trị (1) chia cả hai bản sao của (2) và (4), cho ra thêm ba bản sao. Việc coi đầu vào là một tập hợp thay vì nhiều tập hợp sẽ làm mất đi những bội số đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra từng cặp vị trí theo thứ tự. Với mỗi cặp ((i,j)), chúng ta kiểm tra xem (A_i) có khác 0 hay không, (A_i\neq A_j) và (A_j) có chia hết cho (A_i) hay không. Điều này đúng vì nó áp dụng định nghĩa trực tiếp cho mọi cặp có thể. Vấn đề là số lần kiểm tra (O(p^2)). Tại (p=10^6), trường hợp xấu nhất chứa (10^{12}) cặp có thứ tự, do đó, ngay cả một thử nghiệm chia hết rất rẻ cũng không thể làm cho phương pháp này khả thi. 

Quan sát hữu ích là điều kiện chỉ phụ thuộc vào các giá trị chứ không phụ thuộc vào vị trí. Giả sử một giá trị dương (d) xảy ra (freq[d]) lần. Mọi bội số dương (2d,3d,\ldots) xuất hiện trong đầu vào đều có thể là số bị chia của nó. Nếu (m) là bội số như vậy thì mọi lần xuất hiện của (d) sẽ ghép đôi với mọi lần xuất hiện của (m), góp phần 

[ 
tần số [d]\cdot tần số [m]. 
] 

Chúng ta có thể đếm những đóng góp này bằng cách cố định giá trị ước số (d) và duyệt qua các bội số của nó. Bản thân giá trị (d) bị cố tình bỏ qua vì sự bình đẳng bị cấm. 

Zero có thể được xử lý riêng biệt. Mỗi giá trị đầu vào dương là một ước số thực sự bằng 0, vì vậy nếu`zero_count`là số các số 0 và`positive_count`là số phần tử dương, số 0 đóng góp chính xác 

[ 
số lượng dương\cdot zero_count. 
] 

Bản thân số 0 không đóng góp gì với vai trò là số chia. 

Phương pháp brute-force hoạt động vì nó xem xét rõ ràng từng cặp, nhưng không thành công vì có quá nhiều cặp. Quan sát rằng tất cả cổ tức có thể có của một ước số dương cố định là bội số của nó cho phép chúng ta tổng hợp các giá trị bằng nhau trước tiên và liệt kê các mối quan hệ chia hết trên miền giá trị giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(p^2)) | (O(1)) bên cạnh đầu vào | Quá chậm | 
| Sàng bội số | (O(M\log M)) trong phạm vi giá trị đầy đủ, với (M\le10^7) | (O(M)) | Đã chấp nhận | 

Ở đây (M) là giá trị lớn nhất thực sự xuất hiện trong trường hợp thử nghiệm. Vì (p\le10^6), nhiều nhất (10^6) giá trị dương riêng biệt thực sự có thể xảy ra, do đó số bội số được truy cập thường nhỏ hơn đáng kể so với giới hạn lý thuyết (M\log M). 

## Hướng dẫn thuật toán 

1. Đọc giá trị (p) và tìm giá trị lớn nhất (M). Chúng tôi chỉ cần thông tin tần số lên tới (M), do đó việc phân bổ không gian lên tới giới hạn trên cố định (10^7) là không cần thiết khi mức tối đa thực tế nhỏ hơn. 
2. Xây dựng mảng tần số`freq`, Ở đâu`freq[x]`là số lần xuất hiện của giá trị (x). Một mảng được ưa chuộng hơn từ điển vì thuật toán truy cập liên tục các tần số ở bội số chính xác và phạm vi giá trị bị giới hạn. 
3. Đếm riêng các số 0. Số 0 không bao giờ có thể là ước số trong một cặp hợp lệ, nhưng mọi giá trị dương đều có thể là ước số của mọi số 0. 
4. Khởi tạo câu trả lời bằng`positive_count * zero_count`. Điều này giải thích cho mọi cặp có cổ tức bằng 0 và ước số của nó là dương. 
5. Với mỗi giá trị dương (d) xuất hiện trong đầu vào, hãy xem qua`2*d, 3*d, ...`trong khi bội số tối đa là (M). Đây chính xác là những giá trị dương mà (d) chia và khác với (d). 
6. Với mọi bội số (m), hãy cộng`freq[d] * freq[m]`để trả lời. Yếu tố đầu tiên chọn sự xuất hiện của số chia và yếu tố thứ hai chọn sự xuất hiện của số bị chia. Bắt đầu từ`2*d`còn hơn là`d`tự động loại trừ các giá trị bằng nhau. 
7. In câu trả lời tích lũy theo định dạng trường hợp kiểm tra bắt buộc và thêm dòng trống bắt buộc. 

Bất biến chính là sau khi xử lý giá trị ước số (d), mọi cặp hợp lệ có giá trị ước số là (d) và có cổ tức dương đều được tính chính xác một lần. Nó được tính khi vòng lặp bên ngoài đạt đến (d), bởi vì mọi giá trị chia hết dương đều xuất hiện trong vòng lặp bội số và tích của hai tần số sẽ tính mọi sự kết hợp lần xuất hiện của chúng. Không có cặp nào có giá trị bằng nhau được tính vì vòng lặp bội số bắt đầu tại (2d) và không có cặp nào có số 0 làm ước số được tính vì vòng lặp bên ngoài bắt đầu tại (1). Phần đóng góp bằng 0 riêng biệt chiếm cho mọi cặp hợp lệ còn lại liên quan đến số 0. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve_case(a):
    n = len(a)
    mx = max(a)

    zero_count = 0
    for x in a:
        if x == 0:
            zero_count += 1

    positive_count = n - zero_count

    # An unsigned 32-bit integer is enough because n <= 10^6.
    freq = array('I', [0]) * (mx + 1)

    for x in a:
        freq[x] += 1

    # Every positive number is a proper divisor of zero.
    ans = positive_count * zero_count

    # For each divisor d, inspect only its proper positive multiples.
    for d in range(1, mx + 1):
        cd = freq[d]
        if cd == 0:
            continue

        for m in range(d + d, mx + 1, d):
            cm = freq[m]
            if cm:
                ans += cd * cm

    return ans

def main():
    t = int(input())
    out = []

    for tc in range(1, t + 1):
        p = int(input())

        a = []
        while len(a) < p:
            a.extend(map(int, input().split()))

        ans = solve_case(a)
        out.append(f"Test case #{tc}: {ans}")
        out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`solve_case`trước tiên, hàm xác định giá trị tối đa thực tế, giữ cho dải tần số càng nhỏ càng tốt. Với tối đa là (10^7), mục nhập 32 bit không dấu là đủ vì mọi tần số tối đa là (10^6). các`array`module tránh được chi phí bộ nhớ lớn hơn nhiều khi lưu trữ hàng triệu đối tượng số nguyên Python. 

Phần đóng góp bằng 0 được tính trước vòng lặp bội số. Nếu có`positive_count`yếu tố tích cực và`zero_count`các số 0, mọi phần tử dương có thể chiếm vị trí số chia trong khi mọi số 0 có thể chiếm vị trí số bị chia, tạo ra tích Descartes của chúng. 

Vòng lặp chính bắt đầu tại`d + d`. Lựa chọn ranh giới duy nhất này xử lý hạn chế số chia thích hợp mà không cần kiểm tra đẳng thức riêng biệt. Bắt đầu lúc`d`và thêm`freq[d] * freq[d]`sẽ sai vì các giá trị bằng nhau không phải là cặp phân chia thích hợp. 

Phép nhân sử dụng số nguyên Python nên không có vấn đề tràn. Trong ngôn ngữ có số nguyên có chiều rộng cố định, loại 64 bit là cần thiết vì số lượng cặp thứ tự hợp lệ có thể theo thứ tự (p^2), có thể đạt tới (10^{12}). 

Trình đọc đầu vào cho phép các giá trị (p) trải rộng trên nhiều dòng vật lý mặc dù định dạng chính thức trình bày chúng dưới dạng một dòng. Điều này làm cho trình phân tích cú pháp trở nên mạnh mẽ mà không cần thay đổi thuật toán. 

## Ví dụ đã hoạt động 

Kho lưu trữ bài toán chính thức mô tả phần mẫu, nhưng câu lệnh được trích xuất có sẵn trong kho lưu trữ của cuộc thi không hiển thị các giá trị đầu vào và đầu ra mẫu. Do đó, hai ví dụ sau đây được xây dựng để hiển thị trực tiếp các trường hợp quan trọng. 

Coi như```
1
4
1 2 2 4
```Trạng thái tần số là`freq[1] = 1`,`freq[2] = 2`, Và`freq[4] = 1`. Không có số không. 

| Số chia`d`|`freq[d]`| Đã truy cập nhiều lần | Đã thêm đóng góp | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2, 4 | (1\cdot2 + 1\cdot1 = 3) | 3 | 
| 2 | 2 | 4 | (2\cdot1 = 2) | 5 | 
| 4 | 1 | không | 0 | 5 | 

Ba cặp được đóng góp bởi số chia (1) là hai lần xuất hiện của (2) và một lần xuất hiện của (4). Hai lần xuất hiện của (2), mỗi lần chia cho lần xuất hiện của (4), cho ra hai lần nữa. Đầu ra cuối cùng là`Test case #1: 5`. 

Bây giờ hãy xem xét```
1
3
0 5 10
```Có một giá trị 0 và hai giá trị dương. Phần đóng góp bằng 0 đã là (2), vì cả (5) và (10) đều là ước số thực sự của 0. 

| Số chia`d`|`freq[d]`| Đã truy cập nhiều lần | Đã thêm đóng góp | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | không | 0 | 2 | 
| 5 | 1 | 10 | (1\cdot1 = 1) | 3 | 
| 10 | 1 | không | 0 | 3 | 

Câu trả lời cuối cùng là`3`. Ba cặp là ((5,0)), ((10,0)) và ((5,10)). Số 0 không bao giờ xuất hiện dưới dạng số chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(M\log M)) trường hợp xấu nhất | Đối với ước số (d), nhiều nhất là bội số (M/d-1) được kiểm tra và tổng hài là (O(M\log M)). | 
| Không gian | (O(M)) | Mảng tần số lưu trữ một số đếm 32 bit cho mọi giá trị từ (0) đến (M). | 

Giá trị tối đa là (10^7), trong khi số phần tử đầu vào nhiều nhất là (10^6). Phạm vi giá trị giới hạn làm cho việc sàng bội số trở nên khả thi, trong khi việc liệt kê cặp (O(p^2)) sẽ yêu cầu kiểm tra tới (10^{12}). Giới hạn bộ nhớ của cuộc thi là 256 MB và việc triển khai Python sử dụng mảng tần số 32 bit nhỏ gọn thay vì danh sách Python gồm các đối tượng số nguyên. 

## Trường hợp thử nghiệm```python
# helper: run the solution on an input string
import io
import sys
from array import array

def solve_case(a):
    n = len(a)
    mx = max(a)

    zero_count = 0
    for x in a:
        if x == 0:
            zero_count += 1

    positive_count = n - zero_count

    freq = array('I', [0]) * (mx + 1)

    for x in a:
        freq[x] += 1

    ans = positive_count * zero_count

    for d in range(1, mx + 1):
        cd = freq[d]
        if cd == 0:
            continue

        for m in range(d + d, mx + 1, d):
            cm = freq[m]
            if cm:
                ans += cd * cm

    return ans

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)

        t = int(sys.stdin.readline())
        out = []

        for tc in range(1, t + 1):
            p = int(sys.stdin.readline())

            a = []
            while len(a) < p:
                a.extend(map(int, sys.stdin.readline().split()))

            out.append(f"Test case #{tc}: {solve_case(a)}")
            out.append("")

        return "\n".join(out)
    finally:
        sys.stdin = old_stdin

# Minimum-size input. 1 divides 0, so there is one valid pair.
assert run("1\n2\n0 1\n") == "Test case #1: 1\n", "minimum size"

# All values equal. Divisibility alone is not enough because equal values
# cannot form proper dividing pairs.
assert run("1\n4\n7 7 7 7\n") == "Test case #1: 0\n", "all equal"

# Duplicates must be counted by occurrence.
# 1 divides both 2s and 4: 3 pairs.
# Each of the two 2s divides 4: 2 pairs.
assert run("1\n4\n1 2 2 4\n") == "Test case #1: 5\n", "duplicates"

# Zero boundary case.
# 5 and 10 divide 0, and 5 divides 10.
assert run("1\n3\n0 5 10\n") == "Test case #1: 3\n", "zero handling"

# Maximum value boundary.
# Two copies of 10^7 are both proper divisors of zero.
assert run("1\n3\n10000000 10000000 0\n") == "Test case #1: 2\n", "maximum value"

# Maximum-size input, with all values equal.
# There are 10^6 equal values, but none can divide another properly.
max_n = 10**6
max_input = "1\n" + str(max_n) + "\n" + ("1 " * max_n)
assert run(max_input) == "Test case #1: 0\n", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 0 1`|`Test case #1: 1`| Kích thước tối thiểu và quy tắc 0 đặc biệt | 
|`4 / 7 7 7 7`|`Test case #1: 0`| Không được tính các giá trị bằng nhau | 
|`4 / 1 2 2 4`|`Test case #1: 5`| Các lần xuất hiện trùng lặp đóng góp độc lập | 
|`3 / 0 5 10`|`Test case #1: 3`| Số 0 dưới dạng cổ tức và mức chia thông thường | 
|`3 / 10000000 10000000 0`|`Test case #1: 2`| Ranh giới giá trị tối đa và giá trị tối đa trùng lặp | 
| (10^6) bản sao của`1`|`Test case #1: 0`| Kích thước đầu vào tối đa và thực tế là (p), không phải số lượng giá trị riêng biệt, kiểm soát bội số | 

## Vỏ cạnh 

Để có giá trị bằng nhau, hãy xem xét```
1
2
1 1
```Mảng tần số chứa`freq[1] = 2`. Vòng lặp bên ngoài đạt tới (d=1), nhưng vòng lặp bội số của nó bắt đầu ở (2), do đó không có bội số dương nào để kiểm tra. Số 0 cũng bằng 0, để lại câu trả lời bằng 0. Điều này trực tiếp thực thi điều kiện nghiêm ngặt (D\neq N). 

Với số 0 là ước số, hãy xem xét```
1
2
0 5
```Số dương là một và số 0 là một, vì vậy câu trả lời ban đầu là (1\cdot1=1). Vòng lặp bội số không bao giờ xử lý (d=0), tránh được ý tưởng không hợp lệ rằng số 0 sẽ chia cho một giá trị khác. Kết quả là`Test case #1: 1`. 

Với số 0 là cổ tức, hãy xem xét```
1
3
0 5 10
```Phần đóng góp bằng 0 ban đầu là (2), chiếm ((5,0)) và ((10,0)). Sau đó, số chia (5) tìm (10) là bội số dương thích hợp và thêm một cặp nữa. Kết quả là ba. Trường hợp này chứng tỏ tại sao số 0 không thể bị bỏ qua một cách đơn giản. 

Đối với các bản sao, hãy xem xét```
1
4
1 2 2 4
```Khi (d=1),`freq[1]`là một,`freq[2]`là hai, và`freq[4]`là một, nên phần đóng góp là ba. Khi (d=2), tần số của nó là hai và tần số của bội số thực (4) của nó là một, do đó, hai cặp nữa được thêm vào. Câu trả lời là năm. Thuật toán hoạt động với tần số chính xác vì mỗi lần xuất hiện của số chia có thể ghép với mỗi lần xuất hiện của số bị chia. 

Để có giá trị lớn nhất có thể, hãy xem xét```
1
3
10000000 10000000 0
```Hai bản sao của (10^7) đều là ước thực sự của 0, đóng góp hai cặp. Vì không có bội số dương nào lớn hơn trong phạm vi giá trị được phép nên vòng lặp bội số cho (10^7) không thực hiện phép lặp nào. Các bản sao bằng nhau của (10^7) không được tính. Kết quả là`Test case #1: 2`. 

Đối với kích thước đầu vào tối đa, một trường hợp căng thẳng hữu ích là một triệu bản sao có cùng giá trị:```
1
1000000
1 1 1 1 ...
```Không có số 0 và giá trị ước số phân biệt duy nhất là (1). Các bội số của nó bắt đầu ở (2), tần số của nó bằng 0, nên đáp án vẫn là 0. Tổng quát hơn, trường hợp này chứng tỏ tại sao thuật toán phụ thuộc vào số lượng giá trị riêng biệt cho phép tính bội số thực tế của nó, trong khi vẫn bảo toàn chính xác bội số của tất cả một triệu phần tử đầu vào.
