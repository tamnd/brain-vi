---
title: "CF 1896C - Mảng so khớp"
description: "Chúng tôi có hai mảng có độ dài bằng nhau. Mảng a được cố định, còn mảng b có thể được sắp xếp lại tùy ý. Sau khi chọn hoán vị của b, chúng ta so sánh từng vị trí của mảng. Vẻ đẹp của cặp mảng kết quả là số lượng chỉ mục trong đó a[i] b[i]."
date: "2026-06-08T21:37:56+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1896
codeforces_index: "C"
codeforces_contest_name: "CodeTON Round 7 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1400
weight: 1896
solve_time_s: 158
verified: false
draft: false
---

[CF 1896C - Mảng so khớp](https://codeforces.com/problemset/problem/1896/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai mảng có độ dài bằng nhau. Mảng`a`đã được sửa, trong khi mảng`b`có thể được sắp xếp lại tùy ý. Sau khi chọn một hoán vị của`b`, chúng tôi so sánh vị trí mảng theo vị trí. 

Vẻ đẹp của cặp mảng kết quả là số lượng chỉ mục trong đó`a[i] > b[i]`. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một giá trị mục tiêu`x`. Nhiệm vụ là xác định xem có tồn tại hoán vị của`b`chính xác là vẻ đẹp của ai`x`. Nếu hoán vị như vậy tồn tại, chúng ta phải đưa ra một cách sắp xếp hợp lệ. 

Tổng kích thước của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Điều đó ngay lập tức loại trừ bất cứ điều gì kiểm tra nhiều hoán vị. Thậm chí một`O(n²)`thuật toán sẽ thực hiện xung quanh`4 · 10^10`trong trường hợp xấu nhất vượt xa giới hạn. Kể từ khi sắp xếp`2 · 10^5`số lượng có thể dễ dàng phải chăng, một giải pháp xung quanh`O(n log n)`mỗi tệp thử nghiệm là mục tiêu tự nhiên. 

Phần khó khăn là chúng ta không tối đa hóa hoặc giảm thiểu vẻ đẹp. Chúng ta cần một sự sắp xếp tạo ra chính xác một số vị trí chiến thắng nhất định. 

Một sai lầm dễ mắc phải là gán một cách tham lam các giá trị nhỏ của`b`bất cứ nơi nào có thể và dừng lại một lần`x`chiến thắng đã được tạo ra. 

Coi như:```
n = 3, x = 1
a = [2, 4, 3]
b = [4, 1, 2]
```Một kẻ tham lam bất cẩn có thể đặt`1`chống lại`4`, tạo 1 win rồi gán tùy ý các giá trị còn lại. Nó có thể vô tình tạo ra chiến thắng thứ hai. Câu trả lời đúng tồn tại:```
b = [2, 4, 1]
```tạo ra chính xác một vị trí chiến thắng. 

Một trường hợp tế nhị khác xảy ra khi vẻ đẹp được yêu cầu là không thể.```
n = 1, x = 1
a = [1]
b = [2]
```Không có sự sắp xếp lại tồn tại. Từ`1 > 2`là sai, vẻ đẹp luôn bằng không. 

Nguồn lỗi thứ ba đến từ các giá trị ngang nhau.```
n = 3, x = 0
a = [2, 2, 2]
b = [2, 2, 2]
```Vẻ đẹp chỉ tính đến sự bất bình đẳng nghiêm ngặt. Giá trị bằng nhau không đóng góp. Bất kỳ logic nào xử lý`>=`vì chiến thắng sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo ra mọi hoán vị của`b`, tính toán độ đẹp của từng cách sắp xếp và kiểm tra xem có vẻ đẹp nào bằng`x`. 

Việc tính toán vẻ đẹp mất`O(n)`thời gian. có`n!`hoán vị. Ngay cả đối với`n = 10`, chuyện này đã là về rồi`3.6 million`hoán vị. Vì`n = 20`, nó trở nên hoàn toàn không khả thi. Vì giới hạn thực tế là`2 · 10^5`, lực lượng vũ phu chỉ hữu ích cho việc hiểu cấu trúc của vấn đề. 

Quan sát chính là chỉ có thứ tự tương đối của các giá trị mới quan trọng. Giả sử chúng ta sắp xếp vị trí của`a`theo giá trị. Nếu chúng ta muốn chính xác`x`vị trí ở đâu`a[i] > b[i]`, thì nhỏ nhất`x`giá trị của`b`nên được sử dụng để tạo ra những chiến thắng đó, vì các giá trị nhỏ sẽ dễ bị đánh bại nhất. 

Hãy sắp xếp các chỉ số của`a`bằng cách tăng`a[i]`. Đặt các chỉ số đó là:```
p[0], p[1], ..., p[n-1]
```Đồng thời sắp xếp các giá trị của`b`. 

Để tạo chính xác`x`thắng, chỉ định nhỏ nhất`x`các phần tử được sắp xếp`b`đến lớn nhất`x`các yếu tố của`a`. 

Tại sao? Những phần tử lớn nhất của`a`là những nơi dễ dàng nhất để đảm bảo`a[i] > b[i]`. 

Sau nhiệm vụ đó, phần còn lại`n - x`giá trị của`b`được bổ nhiệm vào các vị trí còn lại của`a`. 

Một công trình rất trang nhã xuất hiện. Nếu được sắp xếp`b`là:```
b0 ≤ b1 ≤ ... ≤ b(n-1)
```sau đó sau khi sắp xếp vị trí của`a`, ta đặt:```
b0 ... b(x-1)
```lên cái lớn nhất`x`vị trí của`a`, Và```
bx ... b(n-1)
```vào cái nhỏ nhất`n-x`các vị trí. 

Điều này tương đương với việc xoay vòng đã sắp xếp`b`mảng theo`x`vị trí liên quan đến sắp xếp`a`. 

Sự sắp xếp kết quả hoặc mang lại chính xác`x`thắng hoặc chứng minh rằng không có giải pháp nào tồn tại. Chúng tôi chỉ đơn giản là đếm vẻ đẹp thực sự sau đó. Nếu số đếm bằng`x`, chúng tôi xuất ra sự sắp xếp. Mặt khác, không có sự sắp xếp hợp lệ nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp chỉ số của`a`bằng cách tăng giá trị. 
2. Sắp xếp tất cả các giá trị của`b`. 
3. Tạo một mảng câu trả lời có độ dài`n`. 
4. Đối với nhỏ nhất`n - x`vị trí theo thứ tự sắp xếp của`a`, gán giá trị lớn nhất`n - x`các giá trị được sắp xếp`b`. 

Chính xác hơn, đối với`i = 0 ... n-x-1`:```
ans[pos[i]] = sorted_b[x + i]
```Những vị trí này nên tránh tạo ra chiến thắng bất cứ khi nào có thể. 
5. Đối với lớn nhất`x`vị trí theo thứ tự sắp xếp của`a`, gán giá trị nhỏ nhất`x`các giá trị được sắp xếp`b`. 

Vì`i = 0 ... x-1`:```
ans[pos[n-x+i]] = sorted_b[i]
```Đây là những vị trí nhằm mục đích trở thành chiến thắng. 
6. Tính toán vẻ đẹp của sự sắp xếp được xây dựng. 
7. Nếu vẻ đẹp ngang bằng`x`, in`"YES"`và sự sắp xếp. 
8. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Sau khi sắp xếp cả hai mảng, việc xây dựng sẽ ghép cặp giá trị nhỏ nhất`x`các yếu tố của`b`với các phần tử lớn nhất của`a`. Nếu có sự sắp xếp nào có thể tạo ra`x`những vị trí chiến thắng, đó là những vị trí thuận lợi nhất để giành chiến thắng. 

Các phần tử lớn hơn còn lại của`b`được so khớp với các phần tử nhỏ hơn của`a`, giảm thiểu cơ hội tạo thêm chiến thắng. 

Sự sắp xếp này là cấu trúc dịch chuyển theo chu kỳ tiêu chuẩn được sử dụng trong giải pháp chính thức. Nếu đạt được vẻ đẹp mong muốn thì công trình sẽ hiện thực hóa được điều đó. Nếu vẻ đẹp do công trình tạo ra không chính xác`x`, thì số trận thắng yêu cầu không thể đạt được bằng bất kỳ hoán vị nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = sorted(range(n), key=lambda i: a[i])
        b.sort()

        ans = [0] * n

        for i in range(n - x):
            ans[pos[i]] = b[x + i]

        for i in range(x):
            ans[pos[n - x + i]] = b[i]

        beauty = sum(a[i] > ans[i] for i in range(n))

        if beauty == x:
            print("YES")
            print(*ans)
        else:
            print("NO")

solve()
```Bước đầu tiên sắp xếp các chỉ số của`a`thay vì sắp xếp`a`chính nó. Cuối cùng chúng ta cần xuất hoán vị theo thứ tự ban đầu nên việc theo dõi các vị trí là điều cần thiết. 

Hai vòng gán thực hiện dịch chuyển theo chu kỳ. Vòng lặp đầu tiên lấp đầy các phần tử nhỏ hơn của`a`với phần lớn hơn được sắp xếp`b`. Vòng lặp thứ hai đặt các giá trị nhỏ nhất của`b`vào các giá trị lớn nhất của`a`. 

Sau khi xây dựng sắp xếp ứng viên, mã sẽ thực hiện xác minh trực tiếp. Việc kiểm tra cuối cùng này rất quan trọng vì sự hiện diện của các giá trị bằng nhau có thể ngăn cản một số vị trí chiến thắng dự định thực sự thỏa mãn.`a[i] > b[i]`. 

Bước xác minh chỉ`O(n)`, vì vậy nó không ảnh hưởng đến độ phức tạp tổng thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, x = 1
a = [2, 4, 3]
b = [4, 1, 2]
```Thông tin được sắp xếp:```
pos = [0, 2, 1]
sorted_b = [1, 2, 4]
```| Bước | Vị trí đã sử dụng | Giá trị được chỉ định | trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | [0,0,0] | 
| i=0 trong vòng lặp đầu tiên | vị trí[0]=0 | 2 | [2,0,0] | 
| i=1 trong vòng lặp đầu tiên | vị trí[1]=2 | 4 | [2,0,4] | 
| i=0 trong vòng lặp thứ hai | vị trí[2]=1 | 1 | [2,1,4] | 

Tính toán vẻ đẹp: 

| Chỉ mục | một [tôi] | trả lời[i] | a[i] > ans[i] | 
| --- | --- | --- | --- | 
| 0 | 2 | 2 | Không | 
| 1 | 4 | 1 | Có | 
| 2 | 3 | 4 | Không | 

Vẻ đẹp bình đẳng`1`, vì vậy câu trả lời được chấp nhận. 

Ví dụ này cho thấy giá trị nhỏ nhất của`b`được cố ý đặt vào giá trị lớn nhất của`a`để tạo ra chính xác một vị trí chiến thắng. 

### Ví dụ 2 

đầu vào:```
n = 3, x = 3
a = [2, 4, 3]
b = [4, 1, 2]
```Thông tin được sắp xếp:```
pos = [0, 2, 1]
sorted_b = [1, 2, 4]
```| Bước | Vị trí đã sử dụng | Giá trị được chỉ định | trả lời | 
| --- | --- | --- | --- | 
| vòng lặp i=0 giây | vị trí[0]=0 | 1 | [1,0,0] | 
| vòng lặp i=1 giây | vị trí[1]=2 | 2 | [1,0,2] | 
| vòng lặp i=2 giây | vị trí[2]=1 | 4 | [1,4,2] | 

Tính toán vẻ đẹp: 

| Chỉ mục | một [tôi] | trả lời[i] | a[i] > ans[i] | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | Có | 
| 1 | 4 | 4 | Không | 
| 2 | 3 | 2 | Có | 

Vẻ đẹp chỉ có`2`. 

Vì sự sắp xếp được xây dựng không đạt được`3`, đầu ra đúng là`"NO"`. 

Ví dụ này minh họa tác dụng của các giá trị bằng nhau. Mặc dù chúng tôi cố gắng tạo ra ba chiến thắng, giá trị`4`không thể bị người khác đánh bại`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong công việc | 
| Không gian | O(n) | Mảng cho chỉ số và câu trả lời | 

Tổng kích thước của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Việc sắp xếp từng trường hợp thử nghiệm sẽ mang lại tổng độ phức tạp là`O(N log N)`trên toàn bộ đầu vào, vừa vặn thoải mái trong giới hạn. 

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
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = sorted(range(n), key=lambda i: a[i])
        b.sort()

        ans = [0] * n

        for i in range(n - x):
            ans[pos[i]] = b[x + i]

        for i in range(x):
            ans[pos[n - x + i]] = b[i]

        beauty = sum(a[i] > ans[i] for i in range(n))

        if beauty == x:
            out.append("YES")
            out.append(" ".join(map(str, ans)))
        else:
            out.append("NO")

    return "\n".join(out)

# minimum size, beauty 0
assert run(
"""1
1 0
1
2
"""
) == "YES\n2"

# minimum size, impossible beauty 1
assert run(
"""1
1 1
1
2
"""
) == "NO"

# all values equal
assert run(
"""1
3 0
2 2 2
2 2 2
"""
) == "YES\n2 2 2"

# boundary x = n, impossible because of equality
assert run(
"""1
3 3
2 4 3
4 1 2
"""
) == "NO"

# off-by-one style case
assert run(
"""1
3 2
2 4 3
4 1 2
"""
).startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, x=0`| CÓ | Trường hợp hợp lệ nhỏ nhất | 
|`n=1, x=1`| KHÔNG | Mục tiêu làm đẹp không thể | 
| Tất cả các giá trị bằng nhau | CÓ | Xử lý bất bình đẳng nghiêm ngặt | 
|`x=n`ví dụ | KHÔNG | Bình đẳng ngăn cản vẻ đẹp trọn vẹn | 
|`n=3, x=2`| CÓ | Ranh giới xoay là chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
1
2
```Mảng được sắp xếp không thay đổi. Việc xây dựng tạo ra:```
ans = [2]
```Vẻ đẹp là:```
1 > 2 = false
```Vẻ đẹp là`0`, không`1`, do đó thuật toán in`"NO"`. 

Bây giờ hãy xem xét các giá trị bằng nhau:```
1
3 0
2 2 2
2 2 2
```Sau khi sắp xếp, mọi bài tập vẫn còn vị trí`2`chống lại`2`. 

Vẻ đẹp trở thành:```
0
```bởi vì sự bình đẳng không được tính là chiến thắng. Bước xác minh chấp nhận chính xác sự sắp xếp. 

Cuối cùng hãy xem xét:```
1
3 3
2 4 3
4 1 2
```Việc xây dựng đặt các giá trị nhỏ nhất của`b`so với giá trị lớn nhất của`a`, cố gắng tạo ra ba chiến thắng. Sự sắp xếp kết quả chứa một vị trí với:```
4 vs 4
```đó không phải là một chiến thắng. Vẻ đẹp trở nên`2`. Việc xác minh cuối cùng phát hiện sự không phù hợp và kết quả đầu ra`"NO"`. 

Những ví dụ này cho thấy lý do tại sao bước xác minh là cần thiết. Việc xây dựng xác định sự sắp xếp ứng cử viên duy nhất có thể mang lại vẻ đẹp mục tiêu và số lượng cuối cùng xác nhận liệu nó có thực sự làm được hay không.
