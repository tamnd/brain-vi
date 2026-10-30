---
title: "CF 106222A - Sông"
description: "Mỗi người ở một vị trí ở bờ bắc và làm việc ở một vị trí ở bờ nam. Chỉ có thể đi bộ dọc theo bờ sông, trong khi việc qua sông thường cần một chiếc thuyền mất B đơn vị thời gian."
date: "2026-06-25T06:57:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106222
codeforces_index: "A"
codeforces_contest_name: "ZCO 2025"
rating: 0
weight: 106222
solve_time_s: 62
verified: true
draft: false
---

[CF 106222A - Sông](https://codeforces.com/problemset/problem/106222/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người ở một vị trí ở bờ bắc và làm việc ở một vị trí ở bờ nam. Chỉ có thể đi bộ dọc theo bờ sông, trong khi băng qua sông thường phải dùng thuyền.`B`đơn vị thời gian. Chúng ta có thể xây dựng một cây cầu duy nhất tại một địa điểm nào đó`L`, và việc băng qua cây cầu đó mất chính xác`1`đơn vị thời gian. Một người sẽ chọn cây cầu nếu tuyến cầu nhiều nhất`T`đơn vị thời gian chậm hơn so với tuyến đường thuyền tối ưu của họ. Chúng ta muốn chọn vị trí cầu và tìm giá trị dung sai nhỏ nhất`T`ít nhất như vậy`M`người ta sử dụng cầu. 

Những hạn chế là thách thức thực sự. Có thể có tới`10^5`người, trong khi vị trí và thời gian thuyền có thể lớn bằng`10^9`. Bất kỳ giải pháp nào thử ở mọi vị trí cầu đều không thể thực hiện được vì có tới`10^9`các địa điểm ứng cử viên. Chúng ta cần thứ gì đó xung quanh`O(N log N)`. 

Một điểm tinh tế là tuyến đường thuyền tối ưu không nhất thiết phải qua trực tiếp tại vị trí nhà hoặc vị trí nơi làm việc. Một người có thể đi bộ trước, sau đó đi thuyền. Thiếu quan sát này dẫn đến thời gian di chuyển không chính xác. 

Hãy xem xét một người có`H = 2`,`W = 8`,`B = 5`. 

Lộ trình đi thuyền tối ưu là:```
walk from 2 to any position between 2 and 8
cross by boat
walk to 8
```Chi phí của nó là`|2 - 8| + 5 = 11`, không`5`. 

Một lỗi dễ mắc phải khác là quên rằng vị trí cầu nối phải là số nguyên. Khi bất đẳng thức liên quan đến phép chia cho hai, việc làm tròn phải được xử lý cẩn thận. 

Ví dụ:```
H = 4, W = 4, B = 1
```Nếu như`T = 0`, chỉ có một cây cầu được xây dựng chính xác tại vị trí`4`được chấp nhận. Việc coi các vị trí là liên tục sẽ cho phép các vị trí lân cận một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi vị trí cầu`L`từ`0`ĐẾN`K - 1`. Đối với mỗi vị trí, chúng tôi có thể tính toán số lượng người sẽ sử dụng cây cầu và sau đó xác định dung sai cần thiết. Ngay cả khi việc đánh giá một vị trí chỉ mất`O(N)`, tổng độ phức tạp sẽ là`O(NK)`, điều đó thật vô vọng khi`K`có thể đạt được`10^9`. 

Nhận xét quan trọng đến từ việc thể hiện chi phí tăng thêm khi sử dụng cây cầu. 

Cho phép```
l = min(H, W)
r = max(H, W)
```Thời gian đi tàu tối ưu là```
|H - W| + B
```bởi vì băng qua bất cứ nơi nào giữa`H`Và`W`giảm thiểu khoảng cách đi bộ. 

Nếu cây cầu ở vị trí`L`, thời gian đi qua cầu là```
|H - L| + 1 + |W - L|
```Chi phí tăng thêm trở thành```
|H - L| + |W - L| - |H - W| + (1 - B)
```Ba thuật ngữ đầu tiên có ý nghĩa hình học hữu ích:```
|H - L| + |W - L| - |H - W|
= 2 * distance(L, [l, r])
```Ở đâu`distance(L, [l, r])`là khoảng cách từ`L`đến khoảng thời gian`[l, r]`. 

Vì vậy, một người sử dụng cầu iff```
2 * distance(L, [l, r]) + (1 - B) <= T
```Sắp xếp lại:```
distance(L, [l, r]) <= (T + B - 1) / 2
```Cho phép```
D = floor((T + B - 1) / 2)
```Vì khoảng cách là số nguyên nên điều kiện trở thành```
distance(L, [l, r]) <= D
```Điều đó có nghĩa là người này chấp nhận mọi vị trí cầu bên trong```
[l - D, r + D]
```Sau khi cắt thành`[0, K - 1]`. 

Đối với một cố định`D`, mỗi người đóng góp một khoảng vị trí cầu có thể chấp nhận được. Chúng tôi chỉ cần biết liệu một số vị trí có được bao phủ bởi ít nhất`M`khoảng thời gian. 

Đó là một vấn đề chồng chéo tối đa tiêu chuẩn. Sử dụng một đường quét với các sự kiện khác nhau sẽ mang lại một`O(N log N)`kiểm tra. 

Số lượng chồng chéo là đơn điệu trong`D`, vì vậy chúng ta có thể tìm kiếm nhị phân nhỏ nhất`D`điều đó cho phép ít nhất`M`mọi người. Sau khi có được điều đó`D`, chúng tôi chuyển đổi nó trở lại dung sai tối thiểu`T`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NK) | O(1) | Quá chậm | 
| Tối ưu | O(N log N log K) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi người, hãy tính khoảng```
[l, r] = [min(H, W), max(H, W)]
```2. Tìm kiếm nhị phân số nguyên nhỏ nhất`D`sao cho tồn tại một vị trí cầu được bao phủ bởi ít nhất`M`khoảng mở rộng. 
3. Trong quá trình kiểm tra lỗi cố định`D`, khai triển mọi khoảng thành```
[max(0, l - D), min(K - 1, r + D)]
```Đây chính xác là những vị trí cầu được người đó chấp nhận. 
4. Chuyển đổi các khoảng thời gian thành các sự kiện quét:```
+1 at left endpoint
-1 at right endpoint + 1
```5. Sắp xếp tất cả các vị trí sự kiện và quét từ trái sang phải, duy trì số lượng chồng chéo hiện tại. 
6. Nếu sự chồng chéo đạt ít nhất`M`, cái này`D`là khả thi. 
7. Sau khi tìm kiếm nhị phân tìm được phương án khả thi tối thiểu`D`, lấy lại giá trị dung sai nhỏ nhất. 

Chúng tôi cần```
floor((T + B - 1) / 2) >= D
```Số nguyên không âm nhỏ nhất thỏa mãn điều này là```
T = max(0, 2 * D - (B - 1))
```### Tại sao nó hoạt động 

Đối với một người cố định, số lượng```
|H - L| + |W - L| - |H - W|
```chính xác là gấp đôi khoảng cách từ`L`đến khoảng cách giữa nhà và nơi làm việc. Khai triển khoảng đó bằng`D`ở cả hai bên tạo ra tập hợp chính xác các vị trí cầu thỏa mãn điều kiện dung sai. 

Một vị trí cầu là khả thi khi và chỉ khi nó thuộc về ít nhất`M`của những khoảng mở rộng này. Đường quét tính toán số lượng khoảng thời gian tối đa bao trùm bất kỳ vị trí nào, do đó nó xác định chính xác tính khả thi cho một vị trí nhất định.`D`. 

Bởi vì ngày càng tăng`D`chỉ mở rộng khoảng thời gian, tính khả thi là đơn điệu. Do đó tìm kiếm nhị phân tìm thấy khả năng tối thiểu khả thi`D`. Công thức cuối cùng chuyển đổi mức tối thiểu đó`D`trở lại dung sai tối thiểu`T`, vậy đáp án là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k, b = map(int, input().split())

    segs = []
    for _ in range(n):
        h, w = map(int, input().split())
        l = min(h, w)
        r = max(h, w)
        segs.append((l, r))

    def feasible(d):
        events = []

        for l, r in segs:
            left = max(0, l - d)
            right = min(k - 1, r + d)

            events.append((left, 1))
            events.append((right + 1, -1))

        events.sort()

        cur = 0
        i = 0
        while i < len(events):
            pos = events[i][0]

            delta = 0
            while i < len(events) and events[i][0] == pos:
                delta += events[i][1]
                i += 1

            cur += delta
            if cur >= m:
                return True

        return False

    lo = 0
    hi = k

    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    d = lo
    t = max(0, 2 * d - (b - 1))
    print(t)

solve()
```Phần đầu tiên chỉ lưu trữ khoảng thời gian giữa nhà và nơi làm việc của mỗi người. Khoảng thời gian đó là tất cả những gì chúng ta cần sau này. 

Việc kiểm tra tính khả thi sẽ mở rộng từng khoảng thời gian bằng`D`, cắt nó vào phạm vi cầu hợp lệ và thực hiện một đường quét. sử dụng`right + 1`đối với sự kiện xóa là thủ thuật tiêu chuẩn cho các khoảng thời gian bao hàm. 

Tìm kiếm nhị phân chạy trên`D`, không trực tiếp trên`T`. Cấu trúc chồng chéo chỉ phụ thuộc vào`D`, điều này làm cho việc kiểm tra trở nên sạch sẽ hơn nhiều. 

Tất cả số học đều sử dụng số nguyên Python, vì vậy các giá trị lên tới`10^9`và hơn thế nữa đều được xử lý an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3 10 1
1 2
7 4
4 3
9 9
0 0
```Vì`D = 1`: 

| Người | Khoảng thời gian ban đầu | Khoảng thời gian mở rộng | 
| --- | --- | --- | 
| 1 | [1, 2] | [0, 3] | 
| 2 | [4, 7] | [3, 8] | 
| 3 | [3, 4] | [2, 5] | 
| 4 | [9, 9] | [8, 9] | 
| 5 | [0, 0] | [0, 1] | 

Tại vị trí`L = 3`, ba khoảng đầu tiên trùng nhau. 

| Vị trí | Bảo hiểm | 
| --- | --- | 
| 3 | 3 | 

Vì thế`D = 1`là khả thi. 

Câu trả lời là```
T = max(0, 2 * 1 - (1 - 1))
  = 2
```### Ví dụ 2 

đầu vào:```
2 2 8 1
1 1
6 6
```Vì`D = 2`: 

| Người | Khoảng thời gian mở rộng | 
| --- | --- | 
| 1 | [0, 3] | 
| 2 | [4, 7] | 

Không có sự chồng chéo tồn tại. 

Vì`D = 3`: 

| Người | Khoảng thời gian mở rộng | 
| --- | --- | 
| 1 | [0, 4] | 
| 2 | [3, 7] | 

Địa điểm`3`Và`4`được bao phủ bởi cả hai khoảng, vì vậy`D = 3`là khả thi. 

Sau đó```
T = 2 * 3 = 6
```phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N log K) | Tìm kiếm nhị phân qua`D`, mỗi lần kiểm tra sẽ thực hiện quét trên`2N`sự kiện | 
| Không gian | O(N) | Danh sách sự kiện và khoảng thời gian được lưu trữ | 

Với`N = 10^5`Và`log K ≈ 30`, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    sys.stdin = input_data
    sys.stdout = output_data

    import sys
    input = sys.stdin.readline

    n, m, k, b = map(int, input().split())

    segs = []
    for _ in range(n):
        h, w = map(int, input().split())
        segs.append((min(h, w), max(h, w)))

    def feasible(d):
        events = []
        for l, r in segs:
            L = max(0, l - d)
            R = min(k - 1, r + d)
            events.append((L, 1))
            events.append((R + 1, -1))

        events.sort()

        cur = 0
        i = 0
        while i < len(events):
            pos = events[i][0]
            delta = 0
            while i < len(events) and events[i][0] == pos:
                delta += events[i][1]
                i += 1
            cur += delta
            if cur >= m:
                return True
        return False

    lo, hi = 0, k
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    ans = max(0, 2 * lo - (b - 1))
    print(ans)

    sys.stdout = sys.__stdout__
    return output_data.getvalue()

# provided samples
assert run("5 3 10 1\n1 2\n7 4\n4 3\n9 9\n0 0\n") == "2\n"
assert run("5 4 10 2\n1 2\n7 4\n4 3\n9 9\n0 0\n") == "3\n"
assert run("5 5 10 4\n1 2\n7 4\n4 3\n9 9\n0 0\n") == "7\n"
assert run("2 2 8 1\n1 1\n6 6\n") == "6\n"

# custom cases
assert run("1 1 5 10\n2 2\n") == "0\n"
assert run("3 2 10 1\n5 5\n5 5\n5 5\n") == "0\n"
assert run("2 2 10 1\n0 0\n9 9\n") == "10\n"
assert run("2 1 10 1\n0 9\n9 0\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Độc thân, thuyền lớn giờ | 0 | Cầu đã nhanh hơn thuyền | 
| Tất cả các khoảng giống hệt nhau | 0 | Chồng chéo tối đa tại một điểm | 
| Đầu đối diện của thành phố | 10 | Yêu cầu mở rộng lớn | 
| Chỉ cần một người dùng | 0 | tối thiểu`M`hành vi | 

## Vỏ cạnh 

Hãy xem xét:```
1 1 5 10
2 2
```Cây cầu ở vị trí`2`mất thời gian`1`, trong khi con thuyền cần có thời gian`10`. Cây cầu đã tốt hơn rồi, nên khoan dung`0`là đủ. Thuật toán tìm`D = 0`, sau đó tính```
T = max(0, -(B - 1)) = 0
```cái nào đúng. 

Bây giờ hãy xem xét:```
2 2 8 1
1 1
6 6
```Các khoảng ban đầu là rời rạc. Thuật toán mở rộng cả hai khoảng trong quá trình tìm kiếm nhị phân. Giá trị khả thi đầu tiên là`D = 3`, nơi các khoảng mở rộng trở thành`[0,4]`Và`[3,7]`. Chúng trùng nhau nên một vị trí cầu nối có thể làm hài lòng cả hai người. Dung sai kết quả là```
T = 2 * 3 = 6
```phù hợp với câu trả lời dự kiến. 

Cuối cùng, hãy xem xét trường hợp làm tròn nhạy cảm:```
1 1 10 1
4 4
```Vì`T = 0`, chúng tôi nhận được```
D = floor((0 + 1 - 1)/2) = 0
```vì vậy khoảng cầu chấp nhận được vẫn còn`[4,4]`. Vị trí duy nhất`4`hoạt động đúng như yêu cầu. Công thức số nguyên tránh được mọi sai sót do khoảng cách nửa số nguyên gây ra.
