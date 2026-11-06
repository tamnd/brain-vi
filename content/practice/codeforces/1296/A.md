---
title: "CF 1296A - Mảng có tổng lẻ"
description: "Chúng ta có một dãy số nguyên. Một thao tác cho phép chúng ta chọn hai vị trí khác nhau và sao chép giá trị từ vị trí này sang vị trí khác. Vì chúng ta có thể lặp lại thao tác này nhiều lần nên các giá trị có thể được nhân đôi trong toàn bộ mảng."
date: "2026-06-11T18:32:25+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1296
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 617 (Div. 3)"
rating: 800
weight: 1296
solve_time_s: 108
verified: true
draft: false
---

[CF 1296A - Mảng có tổng lẻ](https://codeforces.com/problemset/problem/1296/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy số nguyên. Một thao tác cho phép chúng ta chọn hai vị trí khác nhau và sao chép giá trị từ vị trí này sang vị trí khác. Vì chúng ta có thể lặp lại thao tác này nhiều lần nên các giá trị có thể được nhân đôi trong toàn bộ mảng. 

Đối với mỗi trường hợp thử nghiệm, chúng ta cần xác định xem có thể chuyển đổi mảng sao cho tổng cuối cùng của tất cả các phần tử là số lẻ hay không. 

Những hạn chế là rất nhỏ. Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là 2000, do đó, ngay cả các giải pháp tương đối kém hiệu quả cũng sẽ phù hợp một cách thoải mái. Thách thức thực sự là hiểu được những gì hoạt động có thể và không thể thay đổi. 

Quan sát quan trọng là việc sao chép các giá trị không tạo ra số mới. Điều duy nhất quan trọng đối với tính chẵn lẻ của tổng là các phần tử chẵn hay lẻ. Mọi thao tác thay thế một phần tử bằng một phần tử hiện có khác, do đó mảng luôn chỉ chứa các giá trị xuất hiện ban đầu. 

Để có được tổng lẻ, mảng cuối cùng phải chứa số lẻ các phần tử lẻ. Vì tính chẵn lẻ của tổng chính xác là tính chẵn lẻ của số lượng số lẻ nên bài toán trở thành câu hỏi về các giá trị chẵn và lẻ thay vì độ lớn thực tế. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như:```
1
4
2 2 8 8
```Câu trả lời là`NO`. Mọi giá trị đều là số chẵn và việc sao chép các giá trị chẵn không bao giờ có thể tạo ra giá trị lẻ. Một giải pháp bất cẩn chỉ kiểm tra xem các hoạt động có sẵn hay không sẽ trả lời sai`YES`. 

Coi như:```
1
4
5 5 5 5
```Câu trả lời là`NO`. Tất cả các giá trị đều là số lẻ và có bốn giá trị trong số đó nên tổng là số chẵn. Vì không có giá trị chẵn trong mảng nên mọi vị trí sẽ luôn là số lẻ sau bất kỳ chuỗi thao tác nào. Số phần tử lẻ vẫn bằng`n`, do đó tổng không bao giờ có thể trở thành số lẻ. 

Coi như:```
1
2
2 3
```Câu trả lời là`YES`. Tổng ban đầu đã là số lẻ nên không cần thực hiện thao tác nào. Bất kỳ giải pháp nào giả định phải thực hiện ít nhất một thao tác sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Tư duy vũ phu bắt đầu bằng cách suy nghĩ trực tiếp về các hoạt động. Chúng ta có thể thử tạo các mảng có thể truy cập và kiểm tra xem có bất kỳ mảng nào trong số chúng có tổng lẻ hay không. Vì mỗi thao tác chọn hai chỉ số và có thể lặp lại vô thời hạn nên không gian trạng thái sẽ bùng nổ rất nhanh. Ngay cả đối với những giá trị khiêm tốn của`n`, số lượng mảng có thể trở nên rất lớn. Cách tiếp cận này đúng về nguyên tắc vì nó khám phá mọi khả năng, nhưng nó hoàn toàn không thực tế. 

Lý do khiến ý tưởng dùng vũ lực cảm thấy khó khăn là vì nó tập trung vào các giá trị chính xác. Hoạt động này thực sự lưu giữ ít thông tin hơn nhiều so với vẻ ngoài của nó. Điều duy nhất liên quan đến câu trả lời cuối cùng là tính chẵn lẻ. 

Tổng lẻ xảy ra chính xác khi số phần tử lẻ là số lẻ. 

Giả sử tổng hiện tại đã là số lẻ. Sau đó, chúng ta có thể chỉ cần thực hiện các thao tác bằng 0 và trả lời`YES`. 

Bây giờ giả sử tổng hiện tại là số chẵn. Chúng ta có thể thay đổi tính chẵn lẻ của nó không? 

Nếu mảng chứa cả giá trị lẻ và giá trị chẵn thì chúng ta có thể sao chép một giá trị chẵn lẻ vào vị trí của giá trị chẵn lẻ kia. Điều này cho phép chúng ta thay đổi số lượng phần tử lẻ. Đặc biệt, chúng ta luôn có thể làm cho số lẻ trở thành số lẻ, làm cho tổng số là số lẻ. 

Nếu tất cả các số đều là số chẵn thì mọi giá trị có thể xuất hiện đều là số chẵn, do đó tổng luôn là số chẵn. 

Nếu tất cả các số đều là số lẻ thì mọi vị trí luôn chứa một số lẻ. Số phần tử lẻ vĩnh viễn bằng`n`. Tổng chỉ là số lẻ khi`n`thật kỳ quặc. Vì hiện tại chúng ta đang xem xét trường hợp tổng là số chẵn, điều này có nghĩa là`n`là số chẵn và câu trả lời là`NO`. 

Điều này dẫn đến một tiêu chí rất đơn giản: 

Nếu tổng hiện tại là số lẻ, hãy trả lời`YES`. 

Còn không thì trả lời`YES`chỉ khi mảng chứa ít nhất một số lẻ và ít nhất một số chẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của tất cả các phần tử. 
2. Nếu tổng là số lẻ, hãy in`"YES"`. 

Không cần thực hiện thao tác nào vì điều kiện mục tiêu đã được thỏa mãn. 
3. Đếm xem mảng có chứa ít nhất một phần tử lẻ hay không. 
4. Đếm xem mảng có chứa ít nhất một phần tử chẵn hay không. 
5. Nếu có cả hai số chẵn lẻ, hãy in`"YES"`. 

Có cả hai số chẵn lẻ có nghĩa là chúng ta có thể sửa đổi số phần tử lẻ thông qua các thao tác sao chép, tạo ra tổng số lẻ có thể đạt được. 
6. Nếu không, hãy in`"NO"`. 

Khi tất cả các giá trị có cùng tính chẵn lẻ thì mọi giá trị trong tương lai cũng có tính chẵn lẻ đó, do đó tổng chẵn không thể chuyển thành tổng lẻ. 

### Tại sao nó hoạt động 

Tính chẵn lẻ của tổng chỉ phụ thuộc vào tính chẵn lẻ của số phần tử lẻ. Nếu số tiền ban đầu đã là số lẻ thì mục tiêu sẽ đạt được ngay lập tức. 

Khi tồn tại cả hai giá trị lẻ và chẵn, việc sao chép cho phép chúng ta chuyển đổi các phần tử từ lớp chẵn lẻ này sang lớp chẵn lẻ khác, thay đổi số lượng phần tử lẻ. Điều này giúp có thể thu được số lẻ các số lẻ và do đó có được tổng lẻ. 

Khi tất cả các giá trị đều là số chẵn, mọi mảng có thể truy cập đều bao gồm các số chẵn. Khi tất cả các giá trị đều là số lẻ thì mọi mảng có thể truy cập đều bao gồm các số lẻ. Trong cả hai trường hợp, cấu trúc chẵn lẻ không thể thay đổi, do đó, tổng chẵn sẽ tồn tại mãi mãi. Đây chính xác là những tình huống mà câu trả lời là`NO`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    if sum(a) % 2 == 1:
        print("YES")
        continue

    has_odd = any(x % 2 for x in a)
    has_even = any(x % 2 == 0 for x in a)

    if has_odd and has_even:
        print("YES")
    else:
        print("NO")
```Kiểm tra đầu tiên xử lý trường hợp dễ nhất. Nếu tổng đã là số lẻ thì chúng ta không cần phải suy luận về các phép toán nữa. 

Các biến`has_odd`Và`has_even`ghi lại xem mỗi chẵn lẻ có xuất hiện trong mảng hay không. Số lượng thực tế của họ là không cần thiết. Chỉ có sự tồn tại của cả hai điểm chẵn lẻ mới quan trọng. 

Điều kiện cuối cùng được rút ra trực tiếp từ chứng minh. Một tổng chẵn chỉ có thể được đổi thành số lẻ khi cả giá trị lẻ và giá trị chẵn đều có sẵn ở đâu đó trong mảng. 

Không tồn tại mối lo ngại tràn vì số tiền lớn nhất có thể chỉ là`2000 × 2000 = 4,000,000`, dễ dàng phù hợp với số nguyên Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
a = [2, 3]
```| Bước | Tổng hợp | Có Lẻ | Có Chẵn | Kết quả | 
| --- | --- | --- | --- | --- | 
| Trạng thái ban đầu | 5 | Có | Có | - | 
| Kiểm tra tổng chẵn lẻ | 5 là số lẻ | Có | Có | CÓ | 

Tổng đã là số lẻ rồi. Thuật toán trả lời ngay`YES`mà không xem xét bất kỳ hoạt động nào. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [2, 2, 8, 8]
```| Bước | Tổng hợp | Có Lẻ | Có Chẵn | Kết quả | 
| --- | --- | --- | --- | --- | 
| Trạng thái ban đầu | 20 | Không | Có | - | 
| Kiểm tra tổng chẵn lẻ | Thậm chí | Không | Có | Tiếp tục | 
| Sự hiện diện ngang bằng | Không có giá trị lẻ | Không | Có | KHÔNG | 

Ví dụ này cho thấy tại sao chỉ có sẵn các hoạt động là không đủ. Mọi giá trị đều là số chẵn nên mọi giá trị tương lai cũng là số chẵn. Một số tiền lẻ là không thể. 

### Ví dụ 3 

đầu vào:```
n = 3
a = [3, 3, 3]
```| Bước | Tổng hợp | Có Lẻ | Có Chẵn | Kết quả | 
| --- | --- | --- | --- | --- | 
| Trạng thái ban đầu | 9 | Có | Không | - | 
| Kiểm tra tổng chẵn lẻ | lẻ | Có | Không | CÓ | 

Mặc dù tất cả các giá trị đều có cùng tính chẵn lẻ, tổng hiện tại đã là số lẻ nên câu trả lời là`YES`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính tổng và thông tin chẵn lẻ | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng ngoài mảng đầu vào | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm tối đa là 2000, do đó giải pháp O(n) chạy thoải mái trong giới hạn. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ mảng đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if sum(a) % 2 == 1:
            out.append("YES")
            continue

        has_odd = any(x % 2 for x in a)
        has_even = any(x % 2 == 0 for x in a)

        out.append("YES" if has_odd and has_even else "NO")

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""5
2
2 3
4
2 2 8 8
3
3 3 3
4
5 5 5 5
4
1 1 1 1
"""
) == """YES
NO
YES
NO
NO
"""

# minimum size, odd element
assert run(
"""1
1
1
"""
) == """YES
"""

# minimum size, even element
assert run(
"""1
1
2
"""
) == """NO
"""

# mixed parity with even initial sum
assert run(
"""1
2
1 2
"""
) == """YES
"""

# all odd, even count
assert run(
"""1
6
7 7 7 7 7 7
"""
) == """NO
"""

# large all-even case
assert run(
"""1
8
2 4 6 8 10 12 14 16
"""
) == """NO
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, [1]`| CÓ | Mảng nhỏ nhất có tổng lẻ | 
|`n=1, [2]`| KHÔNG | Mảng nhỏ nhất có tổng chẵn | 
|`n=2, [1,2]`| CÓ | Tính chẵn lẻ hỗn hợp có thể tạo ra tổng lẻ | 
|`n=6, [7,7,7,7,7,7]`| KHÔNG | Tất cả đều là số lẻ với số chẵn | 
|`n=8, [2,4,6,8,10,12,14,16]`| KHÔNG | Mảng chẵn | 

## Vỏ cạnh 

### Phần tử chẵn đơn 

đầu vào:```
1
1
2
```Tổng số tiền là`2`, tức là chẵn. Mảng không chứa giá trị lẻ và một giá trị chẵn. Thuật toán đạt đến điều kiện cuối cùng và in`NO`. 

Vì chỉ có một giá trị và mọi thao tác đều sao chép một giá trị hiện có nên mảng không bao giờ có thể chứa số lẻ. 

### Tất cả các giá trị lẻ có độ dài chẵn 

đầu vào:```
1
4
5 5 5 5
```Tổng số tiền là`20`, tức là chẵn. Mảng chứa các giá trị lẻ nhưng không có giá trị chẵn. 

Thuật toán in`NO`. 

Mỗi vị trí sẽ luôn chứa một giá trị lẻ sau bất kỳ chuỗi nhiệm vụ nào. Số phần tử lẻ vẫn còn`4`, do đó tổng vẫn chẵn mãi mãi. 

### Số chẵn lẻ hỗn hợp với tổng chẵn ban đầu 

đầu vào:```
1
4
1 1 2 2
```Tổng số tiền là`6`, tức là chẵn. Mảng chứa cả giá trị chẵn và lẻ. 

Thuật toán in`YES`. 

Ví dụ: sao chép một giá trị lẻ vào một trong các vị trí chẵn. Mảng có thể trở thành`[1, 1, 1, 2]`, tổng của nó là`5`, một số lẻ. 

### Đã có tổng lẻ 

đầu vào:```
1
3
1 2 4
```Tổng số tiền là`7`, thật kỳ quặc. 

Thuật toán in ngay lập tức`YES`. 

Trường hợp này xác nhận rằng việc thực hiện các thao tác là tùy chọn. Câu trả lời đúng có thể đã đạt được trong mảng ban đầu.
