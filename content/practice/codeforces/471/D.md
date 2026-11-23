---
title: "CF 471D - MUH và Tường khối"
description: "Chúng ta có hai bức tường được thể hiện bằng dãy chiều cao của tháp. Bức tường của những con gấu có n tòa tháp, trong khi bức tường của Horace chứa w tòa tháp. Horace được phép dịch chuyển toàn bộ bức tường của mình theo chiều dọc với bất kỳ số lượng nào."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 471
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 269 (Div. 2)"
rating: 1800
weight: 471
solve_time_s: 118
verified: true
draft: false
---

[CF 471D - MUH và Tường khối](https://codeforces.com/problemset/problem/471/D) 

**Đánh giá:** 1800 
**Thẻ:** cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai bức tường được thể hiện bằng dãy chiều cao của tháp. 

Bức tường của gấu chứa`n`tháp, trong khi bức tường của Horace chứa`w`tháp. Horace được phép dịch chuyển toàn bộ bức tường của mình theo chiều dọc với bất kỳ số lượng nào. Mỗi tháp trên tường của anh ta phải được dịch chuyển một giá trị như nhau, nhưng giá trị đó có thể dương, âm hoặc bằng 0. 

Đối với một đoạn có độ dài`w`trong bức tường của những con gấu, chúng tôi muốn biết liệu Horace có thể chọn một số dịch chuyển theo chiều dọc để bức tường của anh ấy khớp chính xác với đoạn đó hay không. Nhiệm vụ là đếm xem có bao nhiêu phân đoạn như vậy tồn tại. 

Chi tiết quan trọng là Horace có thể thêm hằng số giống nhau vào mọi tháp trên tường của mình. Điều này có nghĩa là độ cao tuyệt đối không thành vấn đề. Chỉ có sự khác biệt giữa các tháp liên tiếp mới quan trọng. 

Những hạn chế đạt tới`2 · 10^5`các phần tử. Bất kỳ thuật toán nào so sánh mọi độ dài-`w`phân khúc theo mẫu trực tiếp sẽ yêu cầu khoảng`n · w`hoạt động, có thể đạt được khoảng`4 · 10^10`, vượt xa giới hạn 2 giây. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
n = 5, w = 1
a = [10, 20, 30, 40, 50]
b = [7]
```Mỗi tòa tháp có thể phù hợp với một bức tường của một tòa tháp vì Horace có thể dịch chuyển bức tường của mình với số lượng bất kỳ. Câu trả lời đúng là`5`. 

Một giải pháp chỉ dựa trên các mảng sai phân sẽ tạo ra các chuỗi sai phân trống và có thể trả về không chính xác`0`trừ khi trường hợp này được xử lý riêng. 

Một ví dụ khác:```
a = [5, 8, 11]
b = [1, 4]
```Phân khúc`[5,8]`khớp sau khi thêm`4`tới mọi phần tử của`b`, Và`[8,11]`cũng khớp sau khi thêm`7`. 

Câu trả lời là`2`, mặc dù không có độ cao nào trực tiếp bằng nhau. 

Ví dụ thứ ba cho thấy tại sao những khác biệt liên tiếp là đủ:```
a = [100, 103, 106]
b = [1, 4, 7]
```Sự khác biệt là:```
a: [3, 3]
b: [3, 3]
```Các bức tường khớp sau khi thêm`99`tới mọi phần tử của`b`. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ kiểm tra mọi chiều dài-`w`đoạn tường của con gấu. Đối với mỗi phân đoạn, hãy tính toán độ dịch chuyển cần thiết để căn chỉnh tháp đầu tiên, sau đó xác minh rằng mọi tháp còn lại đều khớp sau khi áp dụng dịch chuyển đó. 

Điều này đúng vì kết quả khớp hợp lệ phải sử dụng một phép dịch chuyển không đổi. Vấn đề xuất hiện khi chúng tôi phân tích chi phí. Có khoảng`n`phân khúc ứng cử viên và mỗi so sánh có thể kiểm tra tới`w`tháp. Độ phức tạp trong trường hợp xấu nhất là`O(nw)`, trở thành về`4 · 10^10`hoạt động khi cả hai mảng có độ dài`2 · 10^5`. 

Quan sát quan trọng là việc thêm một hằng số vào mỗi tháp sẽ bảo tồn những khác biệt liên tiếp. 

Giả sử bức tường của Horace là:```
b1, b2, b3, ...
```và chúng tôi thêm một số giá trị`c`:```
b1+c, b2+c, b3+c, ...
```Sau đó:```
(b2+c) - (b1+c) = b2-b1
```và tương tự cho mọi cặp liền kề. 

Một đoạn khớp với bức tường của Horace sau một số lần dịch chuyển khi và chỉ khi các mảng khác biệt của chúng giống hệt nhau. 

Cho phép:```
DA[i] = a[i+1] - a[i]
DB[i] = b[i+1] - b[i]
```Sự trùng khớp của các bức tường ban đầu tương ứng chính xác với sự xuất hiện của`DB`bên trong`DA`. 

Bây giờ vấn đề trở thành vấn đề khớp chuỗi cổ điển trên các chuỗi số nguyên. Chúng ta cần đếm bao nhiêu lần mẫu`DB`xảy ra trong văn bản`DA`. 

Thuật toán Knuth-Morris-Pratt tìm tất cả các lần xuất hiện của một mẫu trong thời gian tuyến tính. Vì các mảng khác nhau có độ dài`n-1`Và`w-1`, tổng độ phức tạp trở thành`O(n+w)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nw) | O(1) | Quá chậm | 
| Tối ưu (Chênh lệch + KMP) | O(n+w) | O(n+w) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai mảng chiều cao. 
2. Xử lý trường hợp đặc biệt`w = 1`. 

Bất kỳ tòa tháp đơn lẻ nào cũng có thể sánh ngang với bức tường voi một tháp sau khi chọn chuyển dịch dọc thích hợp. Câu trả lời đơn giản là`n`. 
3. Xây dựng mảng khác biệt của bức tường gấu. 

Với mỗi cặp liền kề, lưu trữ:```
a[i+1] - a[i]
```Hai bức tường giống nhau phải có các điểm khác biệt liên tiếp giống hệt nhau. 
4. Tương tự, xây dựng mảng khác biệt của bức tường Horace. 
5. Tính hàm tiền tố KMP cho mảng sai phân mẫu. 

Hàm tiền tố lưu trữ, đối với mọi vị trí, độ dài của tiền tố thích hợp dài nhất cũng là hậu tố. KMP sử dụng thông tin này để tránh kiểm tra lại các giá trị sau khi không khớp. 
6. Chạy kết hợp KMP trên mảng chênh lệch của những con gấu. 

Bất cứ khi nào chiều dài mẫu phù hợp đạt đến`len(DB)`, một sự kiện đã được tìm thấy. 
7. Đếm tất cả các lần xuất hiện và in kết quả. 

### Tại sao nó hoạt động 

Hai bức tường chỉ khác nhau một độ dịch chuyển theo chiều dọc nếu tồn tại một hằng số`c`sao cho mỗi tháp tương ứng khác nhau một cách chính xác`c`. 

Nếu một hằng số như vậy tồn tại thì mọi sai phân liền kề đều giống hệt nhau, do đó các mảng sai phân sẽ khớp nhau. 

Ngược lại, nếu các mảng chênh lệch khớp nhau thì sau khi căn chỉnh tháp đầu tiên của bức tường Horace với tháp đầu tiên của đoạn, tất cả các tháp còn lại buộc phải khớp vì mọi thay đổi liên tiếp đều giống nhau. Do đó, việc kết hợp các mảng khác nhau là cần thiết và đủ. 

KMP tìm thấy mọi lần xuất hiện của mảng khác biệt mẫu bên trong mảng khác biệt văn bản, vì vậy mỗi phân đoạn hợp lệ sẽ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def prefix_function(pattern):
    pi = [0] * len(pattern)

    for i in range(1, len(pattern)):
        j = pi[i - 1]

        while j > 0 and pattern[i] != pattern[j]:
            j = pi[j - 1]

        if pattern[i] == pattern[j]:
            j += 1

        pi[i] = j

    return pi

def solve():
    n, w = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    if w == 1:
        print(n)
        return

    text = [a[i + 1] - a[i] for i in range(n - 1)]
    pattern = [b[i + 1] - b[i] for i in range(w - 1)]

    pi = prefix_function(pattern)

    j = 0
    ans = 0

    for x in text:
        while j > 0 and x != pattern[j]:
            j = pi[j - 1]

        if x == pattern[j]:
            j += 1

        if j == len(pattern):
            ans += 1
            j = pi[j - 1]

    print(ans)

solve()
```Lựa chọn quan trọng đầu tiên là xử lý`w = 1`ngay lập tức. Mảng khác biệt của bức tường một thành phần trống và việc cố gắng chạy KMP trên một mẫu trống sẽ làm phức tạp việc triển khai một cách không cần thiết. Mọi vị trí đều hợp lệ, vì vậy câu trả lời đơn giản là`n`. 

Bước tiếp theo chuyển đổi cả hai bức tường thành các mảng khác nhau. Phép chuyển đổi này loại bỏ các độ cao tuyệt đối không liên quan và giữ chính xác thông tin được bảo toàn dưới sự dịch chuyển theo chiều dọc. 

Chức năng tiền tố là giai đoạn tiền xử lý KMP tiêu chuẩn. Nó ghi lại lượng mẫu vẫn khớp sau khi không khớp, cho phép quá trình quét luôn tuyến tính. 

Trong giai đoạn phù hợp,`j`luôn đại diện cho độ dài tiền tố phù hợp hiện tại. Khi`j`đạt đến độ dài mẫu đầy đủ, một lần xuất hiện đã được tìm thấy. Đặt lại`j`ĐẾN`pi[j-1]`cho phép các kết quả trùng lặp được tính chính xác. 

Tất cả số học đều phù hợp thoải mái với số nguyên Python. Sự khác biệt có thể là âm, nhưng KMP hoạt động trên các chuỗi số nguyên tùy ý giống hệt như hoạt động trên chuỗi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
13 5
2 4 5 5 4 3 2 2 2 3 3 2 1
3 4 4 3 2
```Mảng khác biệt:```
DA = [2,1,0,-1,-1,-1,0,0,1,0,-1,-1]
DB = [1,0,-1,-1]
```| Vị trí văn bản | Giá trị | Độ dài phù hợp sau khi xử lý | Đã tìm thấy sự cố | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | Không | 
| 1 | 1 | 1 | Không | 
| 2 | 0 | 2 | Không | 
| 3 | -1 | 3 | Không | 
| 4 | -1 | 4 | Có | 
| 5 | -1 | 0 | Không | 
| 6 | 0 | 0 | Không | 
| 7 | 0 | 0 | Không | 
| 8 | 1 | 1 | Không | 
| 9 | 0 | 2 | Không | 
| 10 | -1 | 3 | Không | 
| 11 | -1 | 4 | Có | 

Tổng số lần xuất hiện:`2`. 

Dấu vết này cho thấy chính xác những gì chúng tôi muốn. Chuỗi khác biệt về mẫu xuất hiện hai lần bên trong chuỗi khác biệt của con gấu, do đó có hai đoạn tường hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 2
5 8 11
1 4
```Mảng khác biệt:```
DA = [3,3]
DB = [3]
```| Vị trí văn bản | Giá trị | Độ dài phù hợp sau khi xử lý | Đã tìm thấy sự cố | 
| --- | --- | --- | --- | 
| 0 | 3 | 1 | Có | 
| 1 | 3 | 1 | Có | 

Trả lời:`2`. 

Cả hai đoạn có độ dài 2 đều khớp nhau sau khi chọn các dịch chuyển dọc khác nhau. Ví dụ này chứng minh rằng độ cao tuyệt đối không còn phù hợp khi mẫu sai khác khớp với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + w) | Xây dựng các mảng sai phân, hàm tiền tố điện toán và quét KMP đều là tuyến tính | 
| Không gian | O(n + w) | Lưu trữ các mảng khác biệt và hàm tiền tố | 

Với`n, w ≤ 2 · 10^5`, thuật toán thời gian tuyến tính chỉ thực hiện vài trăm nghìn phép tính và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def prefix_function(pattern):
        pi = [0] * len(pattern)

        for i in range(1, len(pattern)):
            j = pi[i - 1]

            while j > 0 and pattern[i] != pattern[j]:
                j = pi[j - 1]

            if pattern[i] == pattern[j]:
                j += 1

            pi[i] = j

        return pi

    n, w = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    if w == 1:
        return str(n)

    text = [a[i + 1] - a[i] for i in range(n - 1)]
    pattern = [b[i + 1] - b[i] for i in range(w - 1)]

    pi = prefix_function(pattern)

    ans = 0
    j = 0

    for x in text:
        while j > 0 and x != pattern[j]:
            j = pi[j - 1]

        if x == pattern[j]:
            j += 1

        if j == len(pattern):
            ans += 1
            j = pi[j - 1]

    return str(ans)

# provided sample
assert run(
"""13 5
2 4 5 5 4 3 2 2 2 3 3 2 1
3 4 4 3 2
"""
) == "2", "sample 1"

# minimum size
assert run(
"""1 1
5
10
"""
) == "1", "single tower"

# w = 1
assert run(
"""5 1
10 20 30 40 50
7
"""
) == "5", "every position matches"

# all equal values
assert run(
"""5 3
7 7 7 7 7
2 2 2
"""
) == "3", "all zero differences"

# off-by-one boundary
assert run(
"""4 4
10 11 13 16
1 2 4 7
"""
) == "1", "whole array matches"

# no match
assert run(
"""5 3
1 2 3 4 5
1 3 6
"""
) == "0", "pattern absent"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, w=1`|`1`| Đầu vào pháp lý nhỏ nhất | 
|`w=1`|`n`| Xử lý trường hợp đặc biệt | 
| Tất cả đều có chiều cao bằng nhau |`3`| Mẫu không khác biệt | 
|`n=w`mảng phù hợp |`1`| Ranh giới khớp toàn bộ mảng | 
| Không xảy ra |`0`| Từ chối đúng | 

## Vỏ cạnh 

Hãy xem xét mô hình một tháp:```
5 1
10 20 30 40 50
7
```Thuật toán ngay lập tức kích hoạt`w == 1`chi nhánh và lợi nhuận`5`. Mọi tòa tháp đều có thể trở thành chiều cao`7`sau khi chọn ca thích hợp nên mọi vị trí đều hợp lệ. 

Bây giờ hãy xem xét trường hợp trong đó việc so khớp yêu cầu một sự thay đổi lớn:```
3 3
100 103 106
1 4 7
```Mảng khác biệt là:```
DA = [3,3]
DB = [3,3]
```KMP tìm thấy một lần xuất hiện. Thuật toán không bao giờ sử dụng giá trị dịch chuyển thực tế`99`, bởi vì sự khác biệt phù hợp đã chứng tỏ rằng sự thay đổi như vậy tồn tại. 

Cuối cùng, hãy xem xét các giá trị lặp lại:```
5 3
7 7 7 7 7
2 2 2
```Mảng khác biệt trở thành:```
DA = [0,0,0,0]
DB = [0,0]
```KMP tìm thấy sự xuất hiện bắt đầu tại các vị trí`0`,`1`, Và`2`, đưa ra câu trả lời`3`. Các trận đấu trùng lặp được tính chính xác vì sau mỗi trận đấu đầy đủ, KMP sẽ quay trở lại bằng cách sử dụng chức năng tiền tố thay vì khởi động lại từ số 0.
