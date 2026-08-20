---
title: "CF 102268C - Đôi Đẹp"
description: "Chúng ta cần xây dựng hai mảng số nguyên a và b có giá trị được sắp xếp theo hai hoán vị khác nhau. Hoán vị p biểu thị thứ tự không giảm của các giá trị trong a, trong khi q biểu thị thứ tự không giảm của các giá trị trong b."
date: "2026-08-19T04:01:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "C"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 463
verified: false
draft: false
---

[CF 102268C - Cặp đôi thú vị](https://codeforces.com/problemset/problem/102268/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 43 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng hai mảng số nguyên`a`Và`b`có giá trị được sắp xếp theo hai hoán vị khác nhau. Hoán vị`p`chỉ ra thứ tự không giảm của các giá trị trong`a`, trong khi`q`chỉ ra thứ tự không giảm của các giá trị trong`b`. 

Đối với chỉ số`i < j`, cặp đôi thật ngầu khi`a[i] + b[j] < 0`. Nhiệm vụ là thực hiện chính xác`k`những cặp như vậy. Các giá trị phải nằm giữa`-n`Và`n`. 

Những hạn chế là lớn, với`n`lên tới`300000`, vì vậy hãy kiểm tra tất cả`O(n^2)`cặp là không thể. Ở kích thước tối đa có khoảng`4.5 * 10^10`các cặp có thể, vượt xa những gì giới hạn hai giây có thể xử lý. Chúng ta cần một công trình có thời gian chạy tối đa là khoảng`O(n log n)`và lý tưởng là gần tuyến tính. 

Không có giá trị không thể có của`k`. Mọi số nguyên từ`0`bởi vì`n(n-1)/2`có thể được hiện thực hóa bằng cách xây dựng dưới đây, vì vậy câu trả lời luôn là`Yes`. Bài toán ban đầu sử dụng chính xác các giới hạn và điều kiện đặt hàng này. 

Trường hợp cạnh đầu tiên là`n = 1`. Giá trị duy nhất có thể là`k = 0`, vì không có cặp nào với`i < j`. Ví dụ,```
1 0
1
1
```phải sản xuất`Yes`, không`No`. Một giải pháp giả sử có ít nhất một cặp tồn tại có thể vô tình thất bại ở đây. 

Trường hợp cạnh thứ hai là`k = 0`. Ví dụ,```
3 0
1 2 3
1 2 3
```cần mỗi cặp không mát mẻ. Chỉ xây dựng một giải pháp tích cực thôi là chưa đủ`k`và sau đó để lại một giá trị biên chưa được khởi tạo. Việc xây dựng có chủ ý ngừng tạo các cặp thú vị sau khi mục tiêu đã đạt đến 0. 

Trường hợp cạnh thứ ba là giá trị tối đa có thể,```
3 3
1 2 3
1 2 3
```nơi mà cả ba cặp đều phải ngầu. Việc xây dựng giải quyết vấn đề này bằng cách thực hiện tất cả các`b`giá trị bằng 0 và tất cả`a`các giá trị âm. Việc thực hiện bất cẩn sử dụng`<= 0`thay vì bất đẳng thức nghiêm ngặt bắt buộc cũng có thể đưa ra quyết định biên không chính xác. 

Trường hợp cạnh thứ tư là khi mục tiêu nằm hoàn toàn bên trong sự đóng góp của một vị trí. Ví dụ,```
4 5
1 2 3 4
4 3 2 1
```phải dừng lại giữa chừng`q`chức vụ. Đơn giản chỉ cần phân công từng`b`hoặc`0`hoặc`n`chỉ có thể tạo ra những tổng số nhất định. Việc gán một phần là thứ mang lại cho chúng ta những giá trị còn thiếu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là cố gắng gán giá trị cho`a`Và`b`, sau đó đếm từng cặp`(i,j)`với`i < j`Và`a[i] + b[j] < 0`. Ngay cả khi các mảng đã được xây dựng rồi, việc kiểm tra câu trả lời theo cách này cũng mất rất nhiều thời gian.`O(n^2)`hoạt động. Vì`n = 300000`, đó là về`4.5 * 10^10`kiểm tra cặp, do đó nó không thể là một phần của thuật toán được chấp nhận. 

Quan sát hữu ích là chúng ta không cần phải tìm kiếm trên các mảng tùy ý. Chúng ta có thể cố ý làm cho các giá trị của`a`tất cả đều khác biệt và tiêu cực. Đặt`a[p_t] = t - 1 - n`. 

Như vậy, cùng`p`, các giá trị chính xác`-n, -n+1, ..., -1`, vì vậy thứ tự yêu cầu là tự động. 

Bây giờ hãy xem xét một chỉ số cố định`j`và giả sử`b[j] = 0`. Vì mọi`a[i]`là âm, mọi chỉ số`i < j`tạo thành một cặp tuyệt vời với`j`. Đóng góp của nó chính xác là`j - 1`. 

Ở thái cực khác, nếu`b[j] = n`, sau đó`a[i] + b[j] >= 0`cho mọi`i`, bởi vì`a[i] >= -n`. Đóng góp của nó là bằng không. 

Điều này cho chúng ta một cách thuận tiện để tiêu thụ mục tiêu`k`. Chúng tôi xử lý các vị trí theo thứ tự được đưa ra bởi`q`. Đối với vị trí hiện tại`x = q_t`, giao`b[x] = 0`đóng góp chính xác`x - 1`. Nếu mục tiêu còn lại ít nhất là`x - 1`, chúng tôi lấy tất cả các cặp đó. 

Cuối cùng sẽ có vị trí đầu tiên mà mục tiêu còn lại`r`nhỏ hơn`x - 1`. Vào thời điểm đó chúng ta cần chính xác`r`của`x - 1`các cặp có thể có liên quan`x`. Vì các giá trị`a[1], ..., a[x-1]`khác nhau, hãy sắp xếp chúng. Nếu như`c`đó có phải là danh sách đã được sắp xếp không, thiết lập`b[x] = -c[r]`với việc lập chỉ mục dựa trên số 0 sẽ tạo ra sự chính xác`r`các giá trị đó thỏa mãn`a[i] + b[x] < 0`. 

Sau vị trí đó, mọi vị trí còn lại`b`có thể`n`, đóng góp bằng không. 

Các bài tập cũng bảo đảm thứ tự cần thiết của`b`. Trước vị trí một phần, các giá trị là`0`. Ở vị trí một phần,`b[x]`nằm giữa`1`Và`n`. Sau đó các giá trị được`n`. Do đó dọc theo`q`dãy không giảm. 

Cấu trúc này giống như ý tưởng trung tâm được sử dụng trong các giải pháp đã công bố cho bài toán: tạo một mảng theo thứ tự nghiêm ngặt và sử dụng một vị trí được chỉ định một phần của mảng kia để nhận ra phần còn lại cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^2)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo`a`theo`p`bằng cách thiết lập`a[p[t]] = t - n - 1`dựa trên một`t`. Các giá trị kết quả là`-n, -n+1, ..., -1`, Vì thế`a`đang tăng lên nghiêm trọng cùng với`p`và mọi giá trị đều nằm trong phạm vi cho phép. 
2. Ban đầu thiết lập mọi`b[i] = n`. Giá trị như vậy không tạo ra cặp nào thú vị vì`a[i] + n >= 0`. 
3. Xử lý các yếu tố của`q`từ trái sang phải. Đặt phần tử hiện tại là`x = q[t]`. Nếu chúng ta đặt`b[x] = 0`, thì mọi chỉ số`i < x`thật tuyệt với`x`, đưa ra chính xác`x - 1`cặp mới. 
4. Nếu mục tiêu còn lại`k`ít nhất là`x - 1`, bộ`b[x] = 0`và trừ`x - 1`từ`k`. Điều này hoàn toàn sử dụng sự đóng góp có sẵn ở vị trí này. 
5. Ngược lại mục tiêu còn lại thỏa mãn`0 <= k < x - 1`. Hãy xem xét các giá trị`a[1], ..., a[x-1]`và sắp xếp chúng ngày càng nhiều hơn`c`. Bộ`b[x] = -c[k]`, sử dụng lập chỉ mục dựa trên số không. Đúng là lần đầu tiên`k`giá trị của`c`thực sự nhỏ hơn`c[k]`, chính xác như vậy`k`chỉ số`i < x`thỏa mãn`a[i] + b[x] < 0`. 
6. Về muộn hơn`b[q[t]]`bằng`n`. Chúng không đóng góp gì nên tổng số cặp thú vị không thay đổi. 
7. Đầu ra`Yes`, theo sau là`a`Và`b`. Mục tiêu phải đạt đến 0 vào thời điểm này vì tổng công suất của tất cả các vị trí là`1 + 2 + ... + (n-1) = n(n-1)/2`, đó là mức tối đa cho phép`k`. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý một số tiền tố của`q`, các vị trí đã được chỉ định đóng góp chính xác số lượng mục tiêu ban đầu đã được sử dụng, trong khi tất cả các vị trí chưa được xử lý đóng góp bằng không. Khi đóng góp toàn bộ`x-1`phù hợp, gán số 0 cho`b[x]`tiêu thụ chính xác số cặp đó. Khi nó không phù hợp, các giá trị riêng biệt của`a[1..x-1]`chúng ta hãy chọn chính xác bất kỳ số nào từ`0`bởi vì`x-2`bằng cách chọn một ngưỡng thích hợp. Vì tất cả các giá trị sau này của`b`là`n`, không cặp nào sau này có thể trở nên ngầu. Vì vậy, số cặp mát mẻ cuối cùng chính xác là số lượng được yêu cầu`k`. 

Các ràng buộc về thứ tự cũng xuất phát từ quá trình xây dựng. Các giá trị của`a`tăng nghiêm ngặt theo`p`. Các giá trị của`b`dọc theo`q`có dạng số 0, có thể là một giá trị trong`[1,n]`, sau đó`n`s, vì vậy chúng không giảm. Mọi giá trị được gán đều nằm trong`[-n,n]`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    q = list(map(int, input().split()))

    a = [0] * (n + 1)
    b = [n] * (n + 1)

    # Along p, a contains -n, -n+1, ..., -1.
    for pos, x in enumerate(p):
        a[x] = pos - n

    remaining = k

    for x in q:
        full = x - 1

        if full <= remaining:
            # b[x] = 0 makes every i < x cool.
            b[x] = 0
            remaining -= full
        else:
            # Need exactly 'remaining' cool pairs ending at x.
            # a[1..x-1] are all distinct.
            values = sorted(a[1:x])

            # values[remaining] is the (remaining + 1)-st
            # smallest value. Exactly 'remaining' values are smaller.
            b[x] = -values[remaining]

            remaining = 0
            break

    print("Yes")
    print(*a[1:])
    print(*b[1:])

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên thiết lập cấu trúc cố định của`a`. Với số không dựa trên`pos`, bài tập`pos - n`sản xuất`-n`cho phần tử đầu tiên của`p`Và`-1`cho cái cuối cùng. 

Vòng lặp thứ hai thực hiện quá trình tham lam. Biến`remaining`là số lượng cặp mát vẫn cần thiết. Khi`x - 1 <= remaining`, cài đặt`b[x]`về 0 là an toàn vì nó sử dụng tất cả các cặp kết thúc tại`x`. 

Trường hợp một phần là nơi duy nhất cần sắp xếp. lát cắt`a[1:x]`chứa chính xác các giá trị thuộc các chỉ số nhỏ hơn`x`. Vì mọi giá trị của`a`khác biệt,`values[remaining]`có chính xác`remaining`các phần tử nhỏ hơn. Việc phủ định nó đưa ra ngưỡng chính xác cho bất đẳng thức nghiêm ngặt. 

biểu hiện`values[remaining]`còn hơn là`values[remaining - 1]`là một điểm chung. Nếu như`remaining = 0`, chúng ta không cần giá trị của`a[1:x]`để thỏa mãn bất đẳng thức nên ta chọn giá trị nhỏ nhất và làm cho bất đẳng thức đủ chặt chẽ để chọn ra các phần tử bằng 0. 

Nhiệm vụ`b[x] = -values[remaining]`luôn nằm trong phạm vi cho phép vì mọi`a`giá trị nằm ở`[-n,-1]`, vậy phủ định của nó nằm ở`[1,n]`. 

Số nguyên Python có độ chính xác tùy ý, do đó giá trị tiềm năng lớn`n(n-1)/2`không tràn. Đầu vào được đọc với`sys.stdin.readline`theo yêu cầu đối với số lượng lớn`n`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu chính thức là```
5 3
3 5 1 2 4
1 2 3 4 5
```Việc xây dựng trước hết tạo ra`a`từ`p`. 

|`t`|`p[t]`|`a[p[t]]`| Còn lại`k`| 
| --- | --- | --- | --- | 
| 1 | 3 | -5 | 3 | 
| 2 | 5 | -4 | 3 | 
| 3 | 1 | -3 | 3 | 
| 4 | 2 | -2 | 3 | 
| 5 | 4 | -1 | 3 | 

Như vậy`a = [-3,-2,-5,-1,-4]`. 

Sau đó chúng tôi xử lý`q`. 

| Hiện hành`x`|`x-1`| Còn lại trước | Hành động | Còn lại sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 3 |`b[1]=0`| 3 | 
| 2 | 1 | 3 |`b[2]=0`| 2 | 
| 3 | 2 | 2 |`b[3]=0`| 0 | 
| 4 | 3 | 0 | chuyển nhượng một phần | 0 | 

Vì`x=4`, các giá trị trước chỉ mục`4`là`[-3,-2,-5]`. Đã sắp xếp, chúng là`[-5,-3,-2]`. Vì mục tiêu còn lại bằng 0 nên chúng tôi chọn`-5`và chỉ định`b[4]=5`. Vị trí còn lại cũng giữ nguyên`5`. 

Một đầu ra hợp lệ được tạo ra bởi thuật toán là```
Yes
-3 -2 -5 -1 -4
0 0 0 5 5
```Có chính xác ba cặp tuyệt vời:`(1,2)`,`(1,3)`, Và`(2,3)`. Mẫu chính thức sử dụng một cấu trúc hợp lệ khác, điều này được mong đợi vì tác vụ yêu cầu bất kỳ mảng hợp lệ nào. 

### Đã thi công mẫu 2 

Hãy xem xét```
4 5
1 2 3 4
4 3 2 1
```Đây`a = [-4,-3,-2,-1]`. 

| Hiện hành`x`|`x-1`| Còn lại trước | Hành động | Còn lại sau | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | 5 |`b[4]=0`| 2 | 
| 3 | 2 | 2 |`b[3]=0`| 0 | 
| 2 | 1 | 0 | chuyển nhượng một phần | 0 | 

Tại`x=2`, giá trị trước đó duy nhất là`a[1]=-4`. Giá trị nhỏ nhất là`-4`, Vì thế`b[2]=4`. Điều này tạo ra các cặp không thú vị ở chỉ mục`2`. 

Mảng cuối cùng là```
a = [-4,-3,-2,-1]
b = [4,4,0,0]
```Hai cặp kết thúc ở chỉ mục`3`thật tuyệt và ba cặp kết thúc ở chỉ số`4`thật tuyệt, đang cho đi`2 + 3 = 5`. Ví dụ này thực hiện trường hợp mục tiêu đạt được chính xác ở một trong các đóng góp đầy đủ và sau đó vị trí một phần không đóng góp được sử dụng để duy trì thứ tự của`b`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Việc xây dựng là tuyến tính ngoại trừ một loại nhiều nhất`n-1`các giá trị. | 
| Không gian |`O(n)`| Các hoán vị và hai mảng được xây dựng yêu cầu bộ nhớ tuyến tính và tiền tố được sắp xếp tạm thời cũng sử dụng bộ nhớ tuyến tính. | 

Thuật toán chỉ thực hiện một loại sắp xếp có khả năng tốn kém, vì vậy`O(n log n)`dễ dàng thích hợp cho`n = 300000`. Việc sử dụng bộ nhớ cũng tuyến tính và vừa vặn thoải mái trong giới hạn 256 MiB đã nêu. 

## Trường hợp thử nghiệm 

Kết quả đầu ra không phải là duy nhất, vì vậy các bài kiểm tra sẽ xác minh các thuộc tính toán học thay vì so sánh các mảng được tạo với một câu trả lời cụ thể.```python
import io
import sys

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    k = next(it)

    p = [next(it) for _ in range(n)]
    q = [next(it) for _ in range(n)]

    a = [0] * (n + 1)
    b = [n] * (n + 1)

    for pos, x in enumerate(p):
        a[x] = pos - n

    remaining = k

    for x in q:
        full = x - 1

        if full <= remaining:
            b[x] = 0
            remaining -= full
        else:
            values = sorted(a[1:x])
            b[x] = -values[remaining]
            remaining = 0
            break

    return "Yes\n" + " ".join(map(str, a[1:])) + "\n" + \
           " ".join(map(str, b[1:])) + "\n"

def run(inp: str) -> str:
    return solve_data(inp)

def verify(inp: str, out: str):
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    k = next(it)
    p = [next(it) for _ in range(n)]
    q = [next(it) for _ in range(n)]

    lines = out.strip().splitlines()
    assert lines[0] == "Yes"

    a = list(map(int, lines[1].split()))
    b = list(map(int, lines[2].split()))

    assert len(a) == n
    assert len(b) == n

    assert all(-n <= x <= n for x in a)
    assert all(-n <= x <= n for x in b)

    for i in range(n - 1):
        assert a[p[i] - 1] <= a[p[i + 1] - 1]
        assert b[q[i] - 1] <= b[q[i + 1] - 1]

    count = 0
    for i in range(n):
        for j in range(i + 1, n):
            if a[i] + b[j] < 0:
                count += 1

    assert count == k

# Provided sample
sample1 = """\
5 3
3 5 1 2 4
1 2 3 4 5
"""
out = run(sample1)
verify(sample1, out)

# Custom case: minimum n
case2 = """\
1 0
1
1
"""
out = run(case2)
verify(case2, out)

# Custom case: k = 0
case3 = """\
4 0
2 4 1 3
3 1 4 2
"""
out = run(case3)
verify(case3, out)

# Custom case: maximum k, producing all-zero b
case4 = """\
4 6
4 1 3 2
2 4 1 3
"""
out = run(case4)
verify(case4, out)

# Custom case: partial contribution, catches off-by-one errors
case5 = """\
4 5
1 2 3 4
4 3 2 1
"""
out = run(case5)
verify(case5, out)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 3 / 3 5 1 2 4 / 1 2 3 4 5`| Bất kỳ hợp lệ`Yes`xây dựng | Mẫu chính thức và hành vi ranh giới một phần thông thường | 
|`1 0 / 1 / 1`|`Yes`với`a=-1`,`b=0`| Kích thước tối thiểu và không có cặp | 
|`4 0 / 2 4 1 3 / 3 1 4 2`| Bất kỳ hợp lệ`Yes`xây dựng | Không có mục tiêu | 
|`4 6 / 4 1 3 2 / 2 4 1 3`| Bất kỳ hợp lệ`Yes`xây dựng | Mục tiêu tối đa và tất cả các cặp đều thú vị | 
|`4 5 / 1 2 3 4 / 4 3 2 1`| Bất kỳ hợp lệ`Yes`xây dựng | Đóng góp một phần và ranh giới bất bình đẳng nghiêm ngặt | 

Người xác minh chỉ cố tình kiểm tra từng cặp trong dây đai kiểm tra. Điều đó không sao cả vì số lượng trường hợp thử nghiệm rất nhỏ trong khi giải pháp được gửi không bao giờ thực hiện việc xác minh bậc hai này. 

## Vỏ cạnh 

cho`n=1`, đầu vào```
1 0
1
1
```tạo ra`a[1] = -1`. hiện tại`q`vị trí là`1`, có đóng góp đầy đủ là`0`, Vì thế`b[1]`trở thành 0 và mục tiêu còn lại vẫn bằng 0. Không có cặp nào cả nên kết quả đầu ra là hợp lệ. 

Vì`k=0`, coi như```
3 0
1 2 3
1 2 3
```Vị trí đầu tiên có`x=1`, vì vậy đóng góp đầy đủ của nó bằng không. Tại`x=2`, mục tiêu được yêu cầu đã bằng 0 và`x-1=1`, do đó thuật toán đi vào trường hợp một phần. Giá trị trước đó duy nhất là`a[1]=-3`, và nó gán`b[2]=3`. Từ`a[1]+3=0`, bất đẳng thức nghiêm ngặt thất bại, tạo ra 0 cặp tuyệt vời. Phần còn lại`b`giá trị là`3`, cũng tạo ra cặp số 0. Việc xây dựng xử lý chính xác ranh giới nghiêm ngặt. 

Để đạt được mục tiêu tối đa,```
3 3
1 2 3
1 2 3
```các giá trị của`a`là`[-3,-2,-1]`. Các vị trí trong`q`đóng góp`0`,`1`, Và`2`, tương ứng, và cả ba đều được lấy hoàn toàn bằng cách gán`b`bằng không. Mỗi cặp đều có một tiêu cực`a`giá trị cộng 0, nên cả ba cặp đều mát. 

Để đóng góp một phần, hãy xem xét```
4 5
1 2 3 4
4 3 2 1
```đầu tiên`q`giá trị là`4`, đóng góp ba cặp và rời đi`2`. Tiếp theo là`3`, đóng góp thêm hai và để lại số không. Vị trí tiếp theo không thể đóng góp bất cứ điều gì vì mục tiêu đã bằng 0, do đó việc xây dựng một phần sẽ chọn giá trị nhỏ nhất trước đó.`a`giá trị làm ngưỡng của nó. Điều này không mang lại cặp bổ sung nào và các vị trí còn lại nhận được`n`. Số cuối cùng là chính xác`5`. 

Lý do thi công không bao giờ cần in`No`là mọi mục tiêu trong khoảng thời gian cho phép có thể được phân tách một cách tham lam thành những đóng góp đầy đủ`q_t-1`cộng thêm một số dư cuối cùng. Tổng của tất cả các năng lực này là`(q_1-1) + (q_2-1) + ... + (q_n-1) = n(n-1)/2`. 

Vị trí một phần cuối cùng có thể nhận ra mọi phần còn lại dưới khả năng của nó vì vị trí trước đó`a`các giá trị là khác biệt. Sự kết hợp đó bao gồm toàn bộ phạm vi yêu cầu của`k`.
