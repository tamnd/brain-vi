---
title: "CF 106421G - Lời chế nhạo của Tyson"
description: "Vấn đề yêu cầu chúng ta xem xét từng phân đoạn liền kề của thời gian bấm nút và quyết định xem thời gian trung bình của nó có nằm trong khoảng thời gian chấp nhận được hay không."
date: "2026-06-25T09:42:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106421
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 3-11-2026 Div. 2 (Advanced)"
rating: 0
weight: 106421
solve_time_s: 36
verified: true
draft: false
---

[CF 106421G - Lời chế nhạo của Tyson](https://codeforces.com/problemset/problem/106421/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng ta xem xét từng phân đoạn liền kề của thời gian bấm nút và quyết định xem thời gian trung bình của nó có nằm trong khoảng thời gian chấp nhận được hay không. Mảng chứa thời gian được ghi lại của mỗi lần thử và chúng ta cần tổng số nhóm liên tiếp không trống có giá trị trung bình ít nhất là`a`và nhiều nhất`b`. 

Những hạn chế làm cho thách thức chính trở nên rõ ràng. Có thể có tới`2 * 10^5`cố gắng, vì vậy việc kiểm tra trực tiếp từng phân đoạn là không thể. có khoảng`n^2 / 2`các phân đoạn liền kề, đạt khoảng`2 * 10^10`khi`n`là lớn. Một thuật toán bậc hai không thể phù hợp với giới hạn thời gian. Chúng ta cần một giải pháp gần`O(n log n)`bởi vì điều đó chỉ mang lại vài triệu thao tác cho kích thước đầu vào này. 

Những phần khó khăn đến từ mức trung bình. Một phân đoạn có giá trị trung bình là phân số vẫn có thể thỏa mãn điều kiện, do đó việc so sánh trực tiếp các tổng số nguyên là không đủ. Ví dụ, đoạn`[3, 4]`có mức trung bình`3.5`. Nếu phạm vi cho phép là`[3, 4]`, câu trả lời phải bao gồm nó. Một giải pháp chia bằng cách sử dụng số học số nguyên có thể coi giá trị trung bình là không chính xác.`3`. 

Một trường hợp khác là khi đường trung bình nằm chính xác trên một ranh giới. Đối với đầu vào:```
3 5 5
5 5 4
```đầu ra đúng là`3`, bởi vì các phân đoạn hợp lệ là`[5]`,`[5]`, Và`[5,5]`. Một giải pháp sử dụng so sánh chặt chẽ thay vì cho phép sự bình đẳng sẽ bỏ lỡ những trường hợp này. 

Vấn đề ranh giới thứ hai xuất hiện ở phía trên. Đối với đầu vào:```
2 1 3
1 3
```đoạn`[1,3]`có mức trung bình`2`, vì vậy nó hợp lệ và câu trả lời là`3`. Giới hạn trên phải được xử lý là "lớn hơn`b`không hợp lệ", không phải "lớn hơn hoặc bằng`b`là không hợp lệ". 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi vị trí bắt đầu và vị trí kết thúc, tính tổng của đoạn đó, chia cho độ dài của nó và kiểm tra xem giá trị trung bình có thuộc khoảng đó hay không. Tổng tiền tố làm giảm chi phí tìm tổng phân khúc để`O(1)`, nhưng vẫn có`O(n^2)`phân đoạn. Trong trường hợp xấu nhất với`n = 200000`, điều này có nghĩa là khoảng hai mươi tỷ tờ séc, vượt xa thời gian sẵn có. 

Quan sát hữu ích là một điều kiện trung bình có thể được viết lại mà không cần chia. Đối với một phân đoạn có giá trị`x1, x2, ..., xk`, điều kiện```
(x1 + x2 + ... + xk) / k >= c
```tương đương với```
(x1 - c) + (x2 - c) + ... + (xk - c) >= 0
```Bây giờ vấn đề trở thành đếm các mảng con với tổng được biến đổi không âm. 

Chúng ta có thể chia khoảng thời gian cần thiết thành hai số đếm đơn giản hơn:```
average in [a, b]
=
average >= a
-
average > b
```Số đếm đầu tiên sử dụng phép biến đổi`x[i] - a`và yêu cầu các mảng con có tổng ít nhất bằng 0. Công dụng thứ hai`x[i] - b`và yêu cầu các mảng con có tổng lớn hơn 0. 

Đối với một trong hai phiên bản, hãy`pref[i]`là tổng tiền tố của mảng được chuyển đổi. Một mảng con`(l, r]`có tổng hợp lệ khi:```
pref[r] - pref[l] >= 0
```có nghĩa là:```
pref[r] >= pref[l]
```Vì vậy, khi quét mảng từ trái sang phải, chúng ta chỉ cần biết có bao nhiêu tổng tiền tố trước đó nhỏ hơn hoặc bằng tổng tiền tố hiện tại. Đây là một vấn đề đếm thống kê đơn hàng. Việc nén tọa độ và cây Fenwick cho phép chúng ta duy trì các số đếm này trong`O(log n)`thời gian cho mỗi tiền tố. 

Phương pháp brute-force hoạt động vì nó kiểm tra chính xác điều kiện chúng ta cần, nhưng không thành công vì nó lặp lại công việc gần như giống hệt nhau cho quá nhiều phân đoạn. Phép biến đổi tổng tiền tố loại bỏ nhu cầu kiểm tra các phân đoạn riêng lẻ và cây Fenwick đếm tất cả các cặp tiền tố hợp lệ một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm trợ giúp đếm xem có bao nhiêu mảng con có trung bình ít nhất một giá trị đã chọn`c`. Trừ`c`từ mọi phần tử và đếm các mảng con có tổng được chuyển đổi không âm. Điều này chuyển đổi so sánh trung bình thành một vấn đề so sánh tiền tố. 
2. Xây dựng tổng tiền tố đã chuyển đổi. Bắt đầu với tổng tiền tố`0`trước khi mảng bắt đầu. Mỗi tiền tố mới biểu thị tổng giá trị được chuyển đổi của các phần tử được xử lý cho đến nay. 
3. Phối hợp nén tất cả các tổng tiền tố. Tổng tiền tố có thể âm và có thể lớn, do đó việc nén cho phép cây Fenwick lưu trữ thứ tự tương đối của chúng thay vì giá trị thực của chúng. 
4. Quét tổng tiền tố từ trái sang phải. Đối với tiền tố hiện tại`p`, đếm xem có bao nhiêu tiền tố trước đó thỏa mãn`previous <= p`. Mỗi cặp như vậy tạo ra một mảng con có tổng được biến đổi ít nhất bằng 0. Thêm số đếm vào câu trả lời và chèn`p`vào cây Fenwick. 
5. Tính số mảng con có giá trị trung bình lớn hơn`b`. Đây gần như là quá trình tương tự, ngoại trừ việc chúng ta cần`previous < p`vì tổng chuyển đổi phải hoàn toàn dương. 
6. Trừ số đếm thứ hai cho số đếm đầu tiên. Các phân khúc còn lại chính xác là những phân khúc có mức trung bình nằm trong khoảng`a`Và`b`. 

Bất biến đằng sau thuật toán là sau khi xử lý tiền tố, cây Fenwick chứa chính xác tất cả các tổng tiền tố trước đó và tần số của chúng. Mọi mảng con có thể kết thúc ở vị trí hiện tại đều tương ứng với một tiền tố trước đó. Truy vấn cây đếm chính xác các tiền tố làm cho phân đoạn hiện tại thỏa mãn bất đẳng thức được chuyển đổi, do đó mọi phân đoạn hợp lệ sẽ được tính một lần và mọi phân đoạn không hợp lệ đều bị loại trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        res = 0
        while i:
            res += self.bit[i]
            i -= i & -i
        return res

def count_average_condition(arr, c, strict):
    pref = [0]
    cur = 0
    for x in arr:
        cur += x - c
        pref.append(cur)

    vals = sorted(set(pref))
    index = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))
    ans = 0

    fw.add(index[0], 1)

    for i in range(1, len(pref)):
        pos = index[pref[i]]
        if strict:
            ans += fw.sum(pos - 1)
        else:
            ans += fw.sum(pos)
        fw.add(pos, 1)

    return ans

def solve():
    n, a, b = map(int, input().split())
    arr = list(map(int, input().split()))

    at_least_a = count_average_condition(arr, a, False)
    greater_than_b = count_average_condition(arr, b, True)

    print(at_least_a - greater_than_b)

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ tần số của tổng tiền tố thay vì giá trị của chúng. các`add`thao tác chèn tiền tố hiện tại sau khi nó được sử dụng để đếm, ngăn việc vô tình đếm đoạn trống. 

Cuộc gọi đầu tiên đến`count_average_condition`cho phép sự bình đẳng, do đó nó truy vấn số lượng tiền tố trước đó có giá trị nhiều nhất là tiền tố hiện tại. Lệnh gọi thứ hai yêu cầu tổng được chuyển đổi dương hoàn toàn, do đó, nó chỉ tính các tiền tố trước đó nhỏ hơn hoàn toàn. 

Số nguyên Python tự động xử lý câu trả lời có khả năng lớn. Câu trả lời có thể lớn bằng số mảng con, khoảng`n * (n + 1) / 2`, lớn hơn nhiều so với số nguyên 32 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
6 7 9
3 4 7 8 9 9
```các mảng con hợp lệ được tính bằng cách trừ đi những mảng con có giá trị trung bình trên`9`từ những người có mức trung bình ít nhất`7`. 

| Bước | Tiền tố hiện tại cho a=7 | Tiền tố trước được tính | Số lần chạy | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | Chèn 0 | 0 | 
| 1 | -4 | 0 | 0 | 
| 2 | -7 | 0 | 0 | 
| 3 | -7 | 2 | 2 | 
| 4 | -6 | 3 | 5 | 
| 5 | -4 | 4 | 9 | 
| 6 | -2 | 6 | 15 | 

Lần đầu tiên đếm tất cả các phân đoạn có mức trung bình ít nhất`7`. Lần thứ hai loại bỏ các phân đoạn có giá trị trung bình lớn hơn`9`, để lại câu trả lời cần thiết`12`. 

Một ví dụ thứ hai:```
3 5 5
5 5 4
```Đối với giới hạn dưới và giới hạn trên, chỉ có mức trung bình chính xác mới quan trọng. 

| Bước | Tiền tố sau khi trừ 5 | Tiền tố trước có quan hệ hợp lệ | Đếm thêm | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | Chèn 0 | 0 | 
| 1 | 0 | 1 | 1 | 
| 2 | 0 | 2 | 2 | 
| 3 | -1 | 0 | 0 | 

Số đếm đầu tiên cung cấp cho tất cả các phân đoạn có mức trung bình ít nhất`5`và thứ hai loại bỏ tất cả các phân đoạn có mức trung bình lớn hơn`5`. Câu trả lời còn lại là`3`, khớp ba phân đoạn với mức trung bình chính xác`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi quá trình trong số hai quá trình đếm`n`tổng tiền tố và mọi phép toán Fenwick đều mất thời gian logarit. | 
| Không gian | O(n) | Tổng tiền tố, tọa độ nén và cây Fenwick đều lưu trữ thông tin tuyến tính. | 

Với`n`lên đến`200000`, giải pháp thực hiện khoảng hai triệu phép tính logarit Fenwick, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            r = 0
            while i:
                r += self.bit[i]
                i -= i & -i
            return r

    def count(arr, c, strict):
        p = [0]
        s = 0
        for x in arr:
            s += x - c
            p.append(s)

        vals = sorted(set(p))
        mp = {x: i + 1 for i, x in enumerate(vals)}
        fw = Fenwick(len(vals))
        fw.add(mp[0], 1)
        ans = 0

        for x in p[1:]:
            pos = mp[x]
            ans += fw.sum(pos - 1 if strict else pos)
            fw.add(pos, 1)

        return ans

    n, a, b = map(int, input().split())
    arr = list(map(int, input().split()))
    return str(count(arr, a, False) - count(arr, b, True)) + "\n"

assert solve_case("""6 7 9
3 4 7 8 9 9
""") == "12\n"

assert solve_case("""1 5 5
5
""") == "1\n"

assert solve_case("""3 5 5
5 5 4
""") == "3\n"

assert solve_case("""2 1 3
1 3
""") == "3\n"

assert solve_case("""5 10 10
10 10 10 10 10
""") == "15\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 7 9 / 3 4 7 8 9 9`|`12`| Hành vi mẫu ban đầu | 
|`1 5 5 / 5`|`1`| Kích thước tối thiểu và ranh giới chính xác | 
|`3 5 5 / 5 5 4`|`3`| Xử lý bình đẳng ở giới hạn dưới và trên | 
|`2 1 3 / 1 3`|`3`| Trung bình phân số và giới hạn trên bao gồm | 
|`5 10 10 / 10 10 10 10 10`|`15`| Tất cả các giá trị bằng nhau và số lượng lớn các phân đoạn hợp lệ | 

## Vỏ cạnh 

Đối với trường hợp ranh giới chính xác:```
3 5 5
5 5 4
```các giá trị được chuyển đổi cho`c = 5`là`0, 0, -1`. Tổng tiền tố trở thành`0, 0, 0, -1`. Tổng tiền tố bằng nhau được tính trong lần chuyển đầu tiên, vì vậy`[5]`,`[5]`, Và`[5,5]`được bao gồm. Vượt qua thứ hai nghiêm ngặt chỉ loại bỏ các phân đoạn có mức trung bình trên`5`, trong số đó không có cái nào cả. 

Đối với trường hợp trung bình phân số:```
2 1 3
1 3
```toàn bộ phân đoạn có tổng`4`và chiều dài`2`, cho kết quả trung bình`2`. Mảng được chuyển đổi cho`c = 1`là`[0, 2]`, do đó, so sánh tiền tố sẽ nhận ra toàn bộ phân đoạn là hợp lệ. Đường giới hạn trên với`c = 3`không loại bỏ nó vì tổng chuyển đổi của nó là âm. 

Đối với tất cả các giá trị bằng nhau:```
5 10 10
10 10 10 10 10
```mọi mảng con có thể có giá trị trung bình`10`. Lượt đi đầu tiên tính tất cả`15`mảng con và lần vượt qua thứ hai được tính bằng 0 vì không có phân đoạn nào có giá trị trung bình lớn hơn`10`. Câu trả lời cuối cùng vẫn còn`15`, xác nhận rằng sự bình đẳng được xử lý chính xác.
