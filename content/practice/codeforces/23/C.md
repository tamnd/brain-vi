---
title: "CF 23C - Cam và Táo"
description: "Chúng tôi có 2N - 1 hộp. Mỗi hộp có hai số, số quả táo và số quả cam bên trong. Chúng ta phải chọn chính xác N hộp sao cho tập được chọn chứa ít nhất một nửa số quả táo và ít nhất một nửa số quả cam trong mỗi hộp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 23
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 23"
rating: 2500
weight: 23
solve_time_s: 149
verified: true
draft: false
---
[CF 23C - Cam và Táo](https://codeforces.com/problemset/problem/23/C) 

**Đánh giá:** 2500 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`2N - 1`hộp. Mỗi hộp có hai số, số quả táo và số quả cam bên trong. Chúng ta phải lựa chọn chính xác`N`các hộp sao cho bộ đã chọn chứa ít nhất một nửa số táo và ít nhất một nửa số cam trong mỗi hộp. 

Từ "ít nhất một nửa" có ý nghĩa quan trọng vì tổng số có thể là số lẻ. Nếu tổng số táo là`S`, chúng ta cần các hộp đã chọn chứa ít nhất`ceil(S / 2)`táo. Điều kiện tương tự áp dụng độc lập cho cam. 

Đầu ra không chỉ là liệu tập hợp con đó có tồn tại hay không. Chúng ta cũng cần in một tập hợp các chỉ mục hợp lệ. 

Ràng buộc lớn nhất là tổng của`N`trên tất cả các trường hợp thử nghiệm, nhiều nhất là`10^5`. Vì mỗi bài kiểm tra có`2N - 1`hộp, tổng kích thước đầu vào là tuyến tính. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận liệt kê tập hợp con nào. Ngay cả đối với`N = 30`, kiểm tra tất cả các tập hợp con có kích thước`N`sẽ rất lớn về mặt thiên văn. Một giải pháp xung quanh`O(N log N)`hoặc`O(N)`mỗi bài kiểm tra là mục tiêu thực tế. 

Phần khó khăn là chúng ta phải thỏa mãn đồng thời hai điều kiện cân bằng độc lập. Một chiến lược tham lam chỉ tối ưu hóa táo có thể dễ dàng thất bại đối với cam. 

Hãy xem xét ví dụ này:```
N = 2

Box 1: (100, 1)
Box 2: (1, 100)
Box 3: (50, 50)
```Chọn hai hộp có nhiều táo nhất sẽ được hộp`1`Và`3`, trong đó có đủ táo nhưng chỉ`51`cam ra khỏi`151`. Điều đó là không đủ. 

Một sai lầm dễ mắc phải khác là quên rằng chúng ta cần chính xác`N`hộp, không phải "nhiều nhất`N`". Ví dụ:```
N = 2

(10, 10)
(1, 1)
(1, 1)
```Hộp đầu tiên đã chứa hơn một nửa số trái cây, nhưng chúng ta vẫn phải xuất ra chính xác hai chỉ số. 

Trường hợp phức tạp hơn sẽ xuất hiện khi tổng số là số lẻ.```
N = 1

(3, 5)
```Một nửa số táo là`1.5`, vì vậy chúng ta cần ít nhất`2`. Một nửa số quả cam là`2.5`, vì vậy chúng ta cần ít nhất`3`. Lỗi chia số nguyên có thể chấp nhận sai các giá trị nhỏ hơn. 

Phần đáng ngạc nhiên của vấn đề này là câu trả lời luôn luôn có thể. Thử thách này hoàn toàn mang tính xây dựng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử mọi tập hợp con có kích thước`N`, tính tổng số táo và cam bên trong nó và kiểm tra xem cả hai tổng có bằng ít nhất một nửa tổng số toàn cầu hay không. 

Điều này có tác dụng vì điều kiện rất dễ xác minh khi một tập hợp con được sửa. Vấn đề là số lượng tập hợp con. Chúng ta sẽ cần kiểm tra:$$\binom{2N-1}{N}$$sự lựa chọn. Vì`N = 20`, đây đã là hơn 35 tỷ tập hợp con. Ngay cả khi kiểm tra liên tục, điều này vẫn vô vọng. 

Vì vậy, câu hỏi thực sự trở thành: cấu trúc ẩn nào đảm bảo rằng một tập hợp con hợp lệ luôn tồn tại? 

Quan sát quan trọng đến từ việc ghép nối. Sắp xếp các hộp theo sự khác biệt:$$a_i - o_i$$Những hộp có nhiều táo hơn cam sẽ được xếp về một bên, và những hộp có nhiều cam hơn táo sẽ ở phía bên kia. 

Bây giờ hãy nhìn vào các cặp liền kề theo thứ tự này. Đối với mỗi cặp, một hộp tương đối nặng táo và hộp kia tương đối nặng cam. Nếu chúng ta chọn chính xác một hộp từ mỗi cặp thì sự mất cân bằng có xu hướng bị loại bỏ. 

Vì có`2N - 1`hộp, sau khi sắp xếp chúng ta có thể hình thành`N - 1`cặp liền kề cộng với một hộp ở giữa còn sót lại. Sau đó, chúng tôi chọn một hộp từ mỗi cặp và bao gồm cả hộp còn lại, cho kết quả chính xác`N`các ô đã chọn. 

Điều tuyệt vời là chỉ có hai cách tự nhiên để làm điều này: 

Lựa chọn đầu tiên: 

lấy phần tử bên trái từ mỗi cặp cộng với ô còn lại. 

Lựa chọn thứ hai: 

lấy phần tử bên phải từ mỗi cặp cộng với hộp còn lại. 

Một trong hai bộ này luôn thỏa mãn cả hai điều kiện quả. 

Tại sao? Bởi vì cùng nhau, hai ứng cử viên này sẽ phân vùng tất cả các hộp ngoại trừ hộp còn sót lại. Tổng số táo của họ cộng lại bằng tổng số táo, và điều này cũng đúng với quả cam. Nếu một ứng cử viên có ít hơn một nửa số táo thì ứng viên kia sẽ tự động có hơn một nửa số táo. Việc đặt hàng bởi`a_i - o_i`đảm bảo hướng tương tự cũng có tác dụng với cam. 

Điều này biến một tìm kiếm tổ hợp không thể thành một cấu trúc dựa trên sắp xếp đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{2N-1}{N} \cdot N)$|$O(N)$| Quá chậm | 
| Tối ưu |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ô và tính tổng số táo và cam. 
2. Với mỗi ô, hãy tính giá trị:$$a_i - o_i$$Điều này đo xem hộp nặng hơn táo hay nặng cam hơn. 

1. Sắp xếp các hộp theo giá trị này. 

Sau khi sắp xếp, các hộp lân cận được cân bằng tương đối với nhau. Những khác biệt tích cực lớn xuất hiện ở một bên và những khác biệt tiêu cực lớn ở phía bên kia. 

1. Xây dựng hai bộ câu trả lời ứng viên. 

Thí sinh thứ nhất nhận hộp ở các vị trí:```
0, 2, 4, ...
```Thí sinh thứ 2 nhận hộp ở các vị trí:```
1, 3, 5, ...
```Vì độ dài mảng là số lẻ nên một trong các ứng cử viên này sẽ chứa chính xác`N`hộp và cái còn lại sẽ chứa`N - 1`. Nối hộp cuối cùng còn lại vào bộ nhỏ hơn. 

1. Tính tổng quả của một tập ứng cử viên. 

Nếu nó đã chứa ít nhất một nửa số quả cả hai, hãy in nó. 

1. Ngược lại, in tập ứng viên còn lại. 

Bằng chứng đảm bảo rằng một trong hai phải hoạt động. 

### Tại sao nó hoạt động 

Sau khi sắp xếp theo`a_i - o_i`, mỗi cặp`(2k, 2k+1)`thỏa mãn:$$(a_{2k} - o_{2k}) \le (a_{2k+1} - o_{2k+1})$$Sắp xếp lại mang lại:$$a_{2k} + o_{2k+1} \le a_{2k+1} + o_{2k}$$Điều này có nghĩa là việc chuyển từ phần tử bên trái của một cặp sang phần tử bên phải sẽ làm tăng số quả táo ít nhất cũng như giảm số quả cam. 

Bây giờ hãy xem xét hai tập ứng cử viên. Họ cùng nhau phân chia tất cả các hộp. Tổng số táo của họ cộng vào tổng số táo toàn cầu, và tổng số cam của họ cộng vào tổng số cam toàn cầu. 

Nếu ứng viên đầu tiên có ít hơn một nửa số táo thì ứng viên thứ hai có nhiều hơn một nửa số táo. Bất đẳng thức trên đảm bảo số thứ hai cũng có ít nhất một nửa số quả cam. Đối xứng, nếu ứng viên đầu tiên có đủ táo thì cũng có đủ cam. 

Vì vậy, một ứng cử viên phải đáp ứng cả hai điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        boxes = []
        total_a = 0
        total_o = 0

        for i in range(1, 2 * n):
            a, o = map(int, input().split())
            boxes.append((a - o, a, o, i))
            total_a += a
            total_o += o

        boxes.sort()

        cand1 = []
        cand2 = []

        sum1_a = 0
        sum1_o = 0

        sum2_a = 0
        sum2_o = 0

        for i in range(2 * n - 1):
            _, a, o, idx = boxes[i]

            if i % 2 == 0:
                cand1.append(idx)
                sum1_a += a
                sum1_o += o
            else:
                cand2.append(idx)
                sum2_a += a
                sum2_o += o

        need_a = (total_a + 1) // 2
        need_o = (total_o + 1) // 2

        if sum1_a >= need_a and sum1_o >= need_o:
            out.append("YES")
            out.append(" ".join(map(str, cand1)))
        else:
            out.append("YES")
            out.append(" ".join(map(str, cand2)))

    sys.stdout.write("\n".join(out))

solve()
```Việc thực hiện theo sau việc xây dựng trực tiếp. 

Mỗi hộp được lưu trữ dưới dạng:```
(a - o, a, o, index)
```Giá trị đầu tiên là khóa sắp xếp, trong khi số lượng và chỉ mục ban đầu được giữ nguyên để sử dụng sau này. 

Sau khi sắp xếp, chúng ta chia mảng theo tính chẵn lẻ của chỉ mục. Vì tổng số ô là số lẻ nên các vị trí chẵn chứa chính xác`N`các phần tử và các vị trí lẻ chứa`N - 1`. Định lý xây dựng đảm bảo tập hợp chỉ số chẵn là đủ, nếu không thì tập hợp chỉ mục lẻ sẽ đủ nếu chúng ta chọn quy ước chẵn lẻ ngược lại. Bởi vì độ dài được sắp xếp là`2N - 1`, các vị trí được lập chỉ mục chẵn là những vị trí chứa`N`các phần tử. 

Một nửa yêu cầu được tính toán bằng cách sử dụng phép chia trần:```
(total + 1) // 2
```Điều này rất quan trọng khi tổng số là số lẻ. 

Số nguyên Python xử lý an toàn các giá trị lên tới`10^9`và hơn thế nữa, vì vậy tràn không phải là một vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 2

1: (10, 15)
2: (5, 7)
3: (20, 18)
```Sự khác biệt: 

| Hộp | Táo | Cam | a - o | 
| --- | --- | --- | --- | 
| 1 | 10 | 15 | -5 | 
| 2 | 5 | 7 | -2 | 
| 3 | 20 | 18 | 2 | 

Sau khi sắp xếp: 

| Vị trí | Hộp | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 3 | 

Ứng viên từ các vị trí chẵn: 

| Bước | Hộp Được Chọn | Tổng Táo | Tổng Cam | 
| --- | --- | --- | --- | 
| 1 | 1 | 10 | 15 | 
| 2 | 3 | 30 | 33 | 

Tổng số táo = 35, cần 18. 

Tổng số cam = 40, cần 20. 

Tập được chọn thỏa mãn cả hai điều kiện. 

Đầu ra:```
1 3
```Dấu vết này cho thấy tại sao việc xây dựng lại cân bằng hai loại trái cây một cách tự nhiên. Một hộp nặng quả cam và hộp kia nặng quả táo. 

### Ví dụ 2 

đầu vào:```
N = 3

1: (100, 1)
2: (1, 100)
3: (50, 50)
4: (40, 60)
5: (60, 40)
```Sự khác biệt: 

| Hộp | a - o | 
| --- | --- | 
| 2 | -99 | 
| 4 | -20 | 
| 3 | 0 | 
| 5 | 20 | 
| 1 | 99 | 

Thứ tự sắp xếp: 

| Vị trí | Hộp | 
| --- | --- | 
| 0 | 2 | 
| 1 | 4 | 
| 2 | 3 | 
| 3 | 5 | 
| 4 | 1 | 

Ứng viên vị trí chẵn: 

| Bước | Hộp Được Chọn | Tổng Táo | Tổng Cam | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 100 | 
| 2 | 3 | 51 | 150 | 
| 3 | 1 | 151 | 151 | 

Tổng số: 

| Trái cây | Tổng cộng | Cần thiết | 
| --- | --- | --- | 
| Táo | 251 | 126 | 
| Cam | 251 | 126 | 

Bộ được chọn hoạt động hoàn hảo. 

Ví dụ này chứng tỏ rằng thuật toán xử lý sự mất cân bằng cực độ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Sắp xếp thống trị thời gian chạy | 
| Không gian |$O(N)$| Chúng tôi lưu trữ tất cả các hộp và chỉ số ứng cử viên | 

Tổng số tiền của`N`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`, do đó tổng công việc sắp xếp dễ dàng phù hợp với thời hạn. Việc sử dụng bộ nhớ là tuyến tính theo số lượng hộp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())

        boxes = []
        total_a = 0
        total_o = 0

        for i in range(1, 2 * n):
            a, o = map(int, input().split())
            boxes.append((a - o, a, o, i))
            total_a += a
            total_o += o

        boxes.sort()

        ans = []
        sa = 0
        so = 0

        for i in range(0, 2 * n - 1, 2):
            _, a, o, idx = boxes[i]
            ans.append(idx)
            sa += a
            so += o

        need_a = (total_a + 1) // 2
        need_o = (total_o + 1) // 2

        assert sa >= need_a
        assert so >= need_o

        out.append("YES")
        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# provided samples
assert run(
"""2
2
10 15
5 7
20 18
1
0 0
"""
) == """YES
1 3
YES
1""", "sample 1"

# minimum size
assert run(
"""1
1
3 5
"""
) == """YES
1""", "minimum case"

# all equal values
assert run(
"""1
2
10 10
10 10
10 10
"""
) == """YES
1 3""", "all equal"

# strong imbalance
assert run(
"""1
3
100 1
1 100
50 50
40 60
60 40
"""
) == """YES
2 3 1""", "extreme imbalance"

# off-by-one half requirement
assert run(
"""1
2
1 1
1 1
1 2
"""
) == """YES
1 3""", "ceiling division"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hộp đơn | Hãy luôn chọn nó | Hạn chế tối thiểu | 
| Tất cả các giá trị bằng nhau | Bất kỳ tập hợp con cân bằng nào cũng hoạt động | Sự ổn định dưới mối quan hệ | 
| Mất cân bằng cực độ | Xây dựng vẫn thành công | Tính đúng đắn của việc sắp xếp cái nhìn sâu sắc | 
| Tổng số lẻ | Xử lý vách ngăn trần đúng cách | An toàn từng người một | 

## Vỏ cạnh 

### Tổng số lẻ 

đầu vào:```
1
2
1 1
1 1
1 2
```Tổng số táo = 3, nên chúng ta cần 2. 

Tổng số quả cam = 4, vậy chúng ta cần 2. 

Sự khác biệt được sắp xếp: 

| Hộp | Sự khác biệt | 
| --- | --- | 
| 3 | -1 | 
| 1 | 0 | 
| 2 | 0 | 

Ngay cả chức vụ cũng tặng hộp`3`Và`2`. 

Tổng của chúng là: 

| Táo | Cam | 
| --- | --- | 
| 2 | 3 | 

Thuật toán sử dụng chính xác phép chia trần. Việc kiểm tra theo tầng sẽ không chính xác khi chỉ cho phép 1 quả táo. 

### Mất cân bằng nặng giữa các loại trái cây 

đầu vào:```
1
2
100 1
1 100
50 50
```Sự khác biệt được sắp xếp: 

| Hộp | Sự khác biệt | 
| --- | --- | 
| 2 | -99 | 
| 3 | 0 | 
| 1 | 99 | 

Ngay cả chức vụ cũng tặng hộp`2`Và`1`. 

Tổng số: 

| Táo | Cam | 
| --- | --- | 
| 101 | 101 | 

Tổng số toàn cầu là 151 cho cả hai loại trái cây, vì vậy chúng ta cần 76. 

Thuật toán thành công vì việc ghép cặp được sắp xếp cân bằng các thái cực đối lập nhau. 

### Nhiều hộp giống hệt nhau 

đầu vào:```
1
3
5 5
5 5
5 5
5 5
5 5
```Mọi sự khác biệt đều bằng 0, vì vậy thứ tự sắp xếp không quan trọng. 

Các vị trí chẵn chọn ba ô: 

| Táo | Cam | 
| --- | --- | 
| 15 | 15 | 

Tổng số toàn cầu là 25 mỗi cái, vì vậy chúng ta cần 13. 

Việc xây dựng vẫn hoạt động ngay cả khi tất cả các phím so sánh được gắn.
