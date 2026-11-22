---
title: "CF 3B - Xe tải"
description: "Chúng tôi có một chiếc xe tải có sức chở v. Có hai loại thuyền."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 3
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 3"
rating: 1900
weight: 3
solve_time_s: 245
verified: true
draft: false
---
[CF 3B - Xe tải](https://codeforces.com/problemset/problem/3/B) 

**Xếp hạng:** 1900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 4m 5s 
**Đã xác minh:** có 

## Hiểu vấn đề 

Chúng tôi có một chiếc xe tải có sức chứa`v`. Có hai loại thuyền. 

Một chiếc thuyền kayak chiếm 1 đơn vị không gian. 

Một chiếc catamaran chiếm 2 đơn vị không gian. 

Con thuyền nào cũng có giá trị, sức chở của nó. Mục tiêu là tải một tập hợp con các thuyền có tổng không gian chiếm dụng không vượt quá thể tích của xe tải, đồng thời tối đa hóa tổng công suất chuyên chở. 

Đầu vào cung cấp cho tất cả các thuyền một. Đối với mỗi chiếc thuyền, chúng tôi biết loại và giá trị của nó. Chúng ta phải in tổng giá trị tối đa và một bộ chỉ số tối ưu. 

Các ràng buộc hoàn toàn định hình giải pháp. Có thể có tới`10^5`thuyền, điều này ngay lập tức loại trừ bất kỳ chương trình động dựa trên tập hợp con nào trên tổng khối lượng. Bản thân âm lượng có thể lớn bằng`10^9`, vậy thậm chí`O(n * v)`là không thể cả về thời gian và trí nhớ. 

Một giải pháp bậc hai cũng quá chậm. Với`10^5`các mặt hàng, thậm chí`O(n^2)`có nghĩa là xung quanh`10^10`hoạt động, vượt xa những gì phù hợp trong 2 giây. Chúng ta cần thứ gì đó xung quanh`O(n log n)`. 

Phần khó khăn là các mục chỉ có hai trọng số có thể là 1 và 2. Cấu trúc đặc biệt đó là toàn bộ lý do khiến vấn đề có thể được giải quyết một cách tham lam. 

Có một số trường hợp đặc biệt phá vỡ lý luận ngây thơ. 

Giả sử chúng ta luôn chọn chiếc thuyền có giá trị cao nhất trước tiên. 

đầu vào:```
3 2
2 100
1 60
1 60
```Lựa chọn tham lam theo giá trị lấy catamaran có giá trị 100. 

Câu trả lời tối ưu là cả hai chiếc thuyền kayak, tổng giá trị là 120. 

Vấn đề là việc chọn một vật phẩm cỡ 2 có thể chặn hai vật phẩm cỡ 1 rất mạnh. 

Một trường hợp nguy hiểm khác xuất hiện khi tập còn lại trở thành số lẻ. 

đầu vào:```
4 3
2 100
2 99
1 60
1 50
```Không thể lấy hai chiếc catamaran tốt nhất vì tổng kích thước là 4. 

Đi một thuyền catamaran và một thuyền kayak là 160. 

Lấy ba chiếc thuyền kayak là không thể vì chỉ có hai chiếc tồn tại. 

Việc triển khai bất cẩn nhằm tối ưu hóa riêng từng loại có thể bỏ sót giải pháp hỗn hợp. 

Một trường hợp tế nhị hơn là khi nhiều chiếc thuyền có giá trị như nhau. 

đầu vào:```
5 4
1 10
1 10
2 20
2 20
1 10
```Một số câu trả lời tối ưu tồn tại. Thuật toán vẫn phải xuất ra một tập hợp các chỉ số gốc hợp lệ. Quên lưu giữ các chỉ số trong quá trình sắp xếp là một lỗi phổ biến ở đây. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Mỗi chiếc thuyền có thể được đón hoặc bỏ qua, vì vậy chúng tôi có thể lặp qua tất cả các tập hợp con, tính tổng khối lượng chiếm dụng và tổng sức chở, sau đó giữ lại tập hợp con khả thi nhất. 

Điều này hoạt động hợp lý vì nó kiểm tra mọi câu trả lời có thể. Vấn đề là số lượng tập hợp con. Với`n = 10^5`, chúng ta cần phải kiểm tra`2^100000`sự kết hợp, điều này là không thể về mặt thiên văn. 

Một hướng đi mạnh mẽ hơn thực tế hơn là quyết định xem chúng ta sẽ đi bao nhiêu thuyền đôi. Khi con số đó được cố định, khoảng trống còn lại sẽ được xác định và chúng ta có thể lấp đầy nó bằng thuyền kayak. 

Giả sử chúng ta sắp xếp tất cả thuyền hai thân theo giá trị giảm dần và tất cả thuyền kayak theo giá trị giảm dần. Sau đó với mọi số lượng du thuyền hai thân có thể`k`, chúng tôi làm điều tốt nhất`k`catamaran và số lượng thuyền kayak tốt nhất có thể phù hợp với không gian còn lại. 

Điều này có vẻ hứa hẹn vì sau khi danh sách được sắp xếp, lựa chọn tốt nhất cho số lượng cố định luôn là tiền tố có giá trị lớn nhất. 

Quan sát chính là trọng số của mục chỉ là 1 và 2. Do đó, mọi giải pháp khả thi đều có thể được biểu diễn dưới dạng:`some prefix of the sorted size-1 list + some prefix of the sorted size-2 list`Không bao giờ có lý do để bỏ qua một vật phẩm mạnh hơn trong khi lấy một vật phẩm yếu hơn có cùng kích thước. 

Sau khi sắp xếp, chúng ta có thể tính toán trước tổng tiền tố cho cả hai nhóm. Sau đó, việc thử mọi số lượng catamaran có thể sẽ trở nên hiệu quả. Đối với mỗi`k`, chúng tôi biết: 

Không gian sử dụng =`2 * k`Dung lượng còn lại =`v - 2 * k`Đóng góp thuyền kayak tốt nhất = tổng tiền tố của lần đầu tiên`remaining space`thuyền kayak 

Tổng giá trị được tính bằng`O(1)`sau khi tiền xử lý. 

Sau đó chúng tôi xây dựng lại các chỉ số tương ứng với sự phân chia tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các thuyền và tách chúng thành hai mảng:`ones`cho thuyền kayak và`twos`dành cho thuyền catamaran. 

Mỗi mục lưu trữ cả giá trị và chỉ mục gốc vì đầu ra yêu cầu đánh số gốc. 
2. Sắp xếp cả hai mảng theo thứ tự dung lượng giảm dần. 

Sau khi phân loại, cách tốt nhất để lấy`k`thuyền cùng loại chỉ đơn giản là lấy chiếc đầu tiên`k`. 
3. Xây dựng tổng tiền tố cho cả hai mảng. 

Cho phép:`pref1[i]`= tổng giá trị tốt nhất`i`thuyền kayak`pref2[i]`= tổng giá trị tốt nhất`i`thuyền hai thân 

Điều này cho phép chúng tôi đánh giá bất kỳ giải pháp ứng cử viên nào trong thời gian không đổi. 
4. Lặp lại số lượng thuyền hai thân đã sử dụng. 

Giả sử chúng ta lấy`k`catamaran. Không gian chiếm đóng của họ là`2 * k`. 

Nếu con số này đã vượt quá khối lượng xe tải, hãy ngừng xem xét lớn hơn`k`. 
5. Tính công suất còn lại. 

Không gian còn lại trở thành:`rem = v - 2 * k`Vì thuyền kayak chiếm cỡ 1 nên chúng tôi có thể chở nhiều nhất`rem`thuyền kayak. 

Chúng tôi cũng không thể vượt quá số lượng thuyền kayak hiện có, vì vậy:`take1 = min(rem, len(ones))`6. Tính tổng sức chứa của phần chia này. 

Tổng giá trị trở thành:`pref2[k] + pref1[take1]`So sánh nó với câu trả lời tốt nhất được thấy cho đến nay. 
7. Lưu trữ cấu hình tốt nhất. 

Cứu:`best_twos = k`

`best_ones = take1`8. Xây dựng lại câu trả lời. 

Xuất ra các chỉ số của: 

cái đầu tiên`best_ones`thuyền kayak và 

cái đầu tiên`best_twos`catamarans từ các mảng được sắp xếp. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ một đối số trao đổi. 

Hãy xem xét mọi giải pháp tối ưu. Trong số tất cả các thuyền kayak đã chọn, nếu tồn tại một thuyền kayak chưa được chọn có giá trị lớn hơn, việc hoán đổi chúng sẽ làm tăng tổng sức chở mà không làm thay đổi không gian chiếm dụng. Vì vậy, trong một giải pháp tối ưu, thuyền kayak được chọn phải là thuyền kayak có giá trị cao nhất trong số tất cả các thuyền kayak. 

Lập luận tương tự được áp dụng độc lập cho tàu hai thân. 

Điều đó có nghĩa là mọi lời giải tối ưu đều được xác định đầy đủ chỉ bằng hai con số: 

có bao nhiêu thuyền kayak được chọn và bao nhiêu thuyền hai thân được chọn. 

Sau khi chúng tôi ấn định số lượng thuyền hai thân, sức chứa còn lại sẽ xác định duy nhất số lượng thuyền kayak tối đa có thể. Tổng tiền tố sau đó sẽ đưa ra giá trị tốt nhất có thể đạt được cho phần phân chia đó. 

Vì chúng tôi thử mọi số lượng du thuyền hai thân khả thi nên một trong các lần lặp lại phù hợp với cấu hình tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, v = map(int, input().split())

ones = []
twos = []

for i in range(1, n + 1):
    t, p = map(int, input().split())

    if t == 1:
        ones.append((p, i))
    else:
        twos.append((p, i))

ones.sort(reverse=True)
twos.sort(reverse=True)

pref1 = [0]
for val, _ in ones:
    pref1.append(pref1[-1] + val)

pref2 = [0]
for val, _ in twos:
    pref2.append(pref2[-1] + val)

best = 0
best_ones = 0
best_twos = 0

for k in range(len(twos) + 1):
    used = 2 * k

    if used > v:
        break

    rem = v - used
    take1 = min(rem, len(ones))

    total = pref2[k] + pref1[take1]

    if total > best:
        best = total
        best_twos = k
        best_ones = take1

answer = []

for i in range(best_ones):
    answer.append(str(ones[i][1]))

for i in range(best_twos):
    answer.append(str(twos[i][1]))

print(best)
print(" ".join(answer))
```Phần đầu tiên phân loại thuyền theo loại nhưng vẫn giữ nguyên chỉ số gốc. Việc giữ các chỉ số trong quá trình sắp xếp là điều cần thiết vì đầu ra yêu cầu các vị trí so với thứ tự đầu vào ban đầu. 

Việc sắp xếp cả hai nhóm giảm dần đảm bảo rằng tiền tố luôn tối ưu cho số lượng cố định. 

Mảng tổng tiền tố được xây dựng với số 0 đứng đầu bổ sung. Điều này làm cho các biểu thức như`pref1[0]`hợp lệ một cách tự nhiên và loại bỏ việc kiểm tra điều kiện biên. 

Vòng lặp chính lặp lại số lượng catamaran đã chọn. Vì mỗi catamaran tiêu thụ 2 chiếc nên khối lượng sử dụng là`2 * k`. Ngay khi điều này vượt quá`v`, giá trị lớn hơn của`k`cũng không thể thực hiện được nên vòng lặp sẽ dừng ngay lập tức. 

Biểu thức:```
take1 = min(rem, len(ones))
```là tinh tế. Khối lượng còn lại có thể lớn hơn số lượng thuyền kayak hiện có. quên đi`min`gây ra truy cập ngoài giới hạn. 

Giai đoạn xây dựng lại chỉ cần lấy các tiền tố tương ứng từ các mảng đã được sắp xếp. Bởi vì bằng chứng đã chứng minh rằng các giải pháp tối ưu luôn sử dụng tiền tố nên việc tái cấu trúc này khớp chính xác với mức tối ưu được tính toán. 

Số nguyên Python xử lý an toàn tất cả các khoản tiền ở đây. Tổng giá trị lớn nhất có thể là nhiều nhất`10^5 * 10^4 = 10^9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
2 7
1 3
```Các nhóm được sắp xếp:`ones = [(3, 3), (2, 1)]`

`twos = [(7, 2)]`Tổng tiền tố:`pref1 = [0, 3, 5]`

`pref2 = [0, 7]`| k | Không gian đã qua sử dụng | Còn lại | lấy1 | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | 5 | 
| 1 | 2 | 0 | 0 | 7 | 

Giá trị tốt nhất là 7. 

Các chỉ số được chọn:```
2
```Dấu vết này cho thấy tại sao tối đa hóa số lượng thuyền một cách mù quáng là sai. Hai chiếc thuyền kayak vừa vặn, nhưng chiếc catamaran đơn có tổng giá trị lớn hơn. 

### Ví dụ 2 

đầu vào:```
5 4
1 8
1 7
2 15
2 14
1 6
```Các nhóm được sắp xếp:`ones = [(8,1), (7,2), (6,5)]`

`twos = [(15,3), (14,4)]`Tổng tiền tố:`pref1 = [0, 8, 15, 21]`

`pref2 = [0, 15, 29]`| k | Không gian đã qua sử dụng | Còn lại | lấy1 | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 4 | 3 | 21 | 
| 1 | 2 | 2 | 2 | 30 | 
| 2 | 4 | 0 | 0 | 29 | 

Câu trả lời hay nhất là 30. 

Thuyền được chọn: 

catamaran 3 và thuyền kayak 1, 2. 

Ví dụ này chứng minh rằng giải pháp tối ưu có thể kết hợp cả hai loại. Chỉ lấy loại mạnh nhất trên toàn cầu là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Mảng và tổng tiền tố lưu trữ tất cả các thuyền | 

Với`n = 10^5`,`O(n log n)`dễ dàng đủ nhanh trong Python. Việc sử dụng bộ nhớ là tuyến tính và vừa vặn thoải mái trong 64 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, v = map(int, input().split())

    ones = []
    twos = []

    for i in range(1, n + 1):
        t, p = map(int, input().split())

        if t == 1:
            ones.append((p, i))
        else:
            twos.append((p, i))

    ones.sort(reverse=True)
    twos.sort(reverse=True)

    pref1 = [0]
    for val, _ in ones:
        pref1.append(pref1[-1] + val)

    pref2 = [0]
    for val, _ in twos:
        pref2.append(pref2[-1] + val)

    best = 0
    best_ones = 0
    best_twos = 0

    for k in range(len(twos) + 1):
        used = 2 * k

        if used > v:
            break

        rem = v - used
        take1 = min(rem, len(ones))

        total = pref2[k] + pref1[take1]

        if total > best:
            best = total
            best_twos = k
            best_ones = take1

    ans = []

    for i in range(best_ones):
        ans.append(str(ones[i][1]))

    for i in range(best_twos):
        ans.append(str(twos[i][1]))

    print(best)
    print(" ".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run(
"""3 2
1 2
2 7
1 3
"""
).splitlines()[0] == "7", "sample 1"

# minimum case
assert run(
"""1 1
1 5
"""
).splitlines()[0] == "5", "single kayak"

# only catamarans fit
assert run(
"""3 2
2 10
2 20
1 1
"""
).splitlines()[0] == "20", "best catamaran"

# mixed optimal answer
assert run(
"""5 4
1 8
1 7
2 15
2 14
1 6
"""
).splitlines()[0] == "30", "mixed choice"

# equal values
assert run(
"""4 4
1 10
1 10
2 20
2 20
"""
).splitlines()[0] == "40", "multiple optimal answers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thuyền kayak đơn | 5 | Đầu vào kích thước tối thiểu | 
| Catamaran mạnh mẽ | 20 | Chọn một món đồ cỡ 2 thay vì kết hợp yếu hơn | 
| Hỗn hợp tối ưu | 30 | Tương tác đúng giữa cả hai loại | 
| Giá trị bằng nhau | 40 | Nhiều giải pháp tối ưu và tái thiết ổn định | 

## Vỏ cạnh 

Hãy xem xét phản ví dụ trước đó chống lại sự tham lam theo giá trị. 

đầu vào:```
3 2
2 100
1 60
1 60
```Các nhóm được sắp xếp:`ones = [60, 60]`

`twos = [100]`Thuật toán đánh giá: 

cho`k = 0`: 

lấy hai chiếc thuyền kayak, tổng cộng = 120 

cho`k = 1`: 

lấy một chiếc catamaran, tổng cộng = 100 

Thuật toán chọn đúng 120. Một chiến lược tham lam ngây thơ chọn giá trị đơn lớn nhất trước tiên sẽ thất bại ở đây. 

Bây giờ hãy xem xét trường hợp công suất hỗn hợp. 

đầu vào:```
4 3
2 100
2 99
1 60
1 50
```Các lần lặp là: 

cho`k = 0`: 

lấy hai chiếc thuyền kayak, tổng cộng = 110 

cho`k = 1`: 

không gian còn lại = 1 

lấy một chiếc thuyền kayak, tổng cộng = 160 

cho`k = 2`: 

không gian bị chiếm dụng = 4, không thể 

Thuật toán tạo ra catamaran có giá trị 100 và thuyền kayak có giá trị 60. Điều này xác nhận rằng việc thử mọi số lượng vật dụng cỡ 2 khả thi sẽ xử lý chính xác dung lượng lẻ còn lại. 

Cuối cùng, hãy xem xét các mặt hàng có giá trị bằng nhau. 

đầu vào:```
5 4
1 10
1 10
2 20
2 20
1 10
```Bất kỳ sự kết hợp nào của hai thuyền hai thân hoặc bốn đơn vị tổng giá trị thuyền kayak đều cho cùng một câu trả lời. Thuật toán sắp xếp trong khi bảo toàn các chỉ số và chỉ đưa ra một cấu hình tiền tố tối ưu hợp lệ. Vì bài toán chấp nhận bất kỳ tập hợp tối ưu nào nên điều này đúng.
