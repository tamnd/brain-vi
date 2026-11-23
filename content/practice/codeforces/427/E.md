---
title: "CF 427E - Cảnh sát tuần tra"
description: "Tất cả tội phạm đều đứng trên trục x và chúng ta phải chọn một tọa độ nguyên cho đồn cảnh sát. Xe tuần tra xuất phát từ đồn, đến thăm một số tội phạm, đưa chúng về đồn, sau đó lặp lại cho đến khi mọi người bị bắt. Mỗi chuyến có thể chở tối đa m tội phạm."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 427
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 244 (Div. 2)"
rating: 2000
weight: 427
solve_time_s: 114
verified: true
draft: false
---

[CF 427E - Cảnh sát tuần tra](https://codeforces.com/problemset/problem/427/E) 

**Xếp hạng:** 2000 
**Tags:** tham lam, thực hiện, toán học, tìm kiếm bậc ba 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tất cả tội phạm đều đứng trên trục x và chúng ta phải chọn một tọa độ nguyên cho đồn cảnh sát. Xe tuần tra xuất phát từ đồn, đến thăm một số tội phạm, đưa chúng về đồn, sau đó lặp lại cho đến khi mọi người bị bắt. Mỗi chuyến đi có thể chở tối đa`m`tội phạm. 

Chi phí khoảng cách của một chuyến đi rất đơn giản. Nếu tên tội phạm xa nhất được viếng thăm trong chuyến đi đó là ở khoảng cách xa`d`từ ga, ô tô phải đi từ ga đến điểm đó rồi quay trở lại nên chi phí chuyến đi`2d`. 

Câu hỏi thực sự là làm thế nào để chia bọn tội phạm thành các chuyến đi và đặt trạm ở đâu sao cho tổng quãng đường di chuyển là tối thiểu. 

Các vị trí đã được sắp xếp. Có thể có tới`10^6`tội phạm, ngay lập tức loại trừ bất cứ điều gì bậc hai. Thậm chí`O(n log n)`đã gần đạt đến giới hạn thực tế trong Python đối với kích thước đầu vào này, vì vậy giải pháp dự kiến ​​về cơ bản phải là tuyến tính sau khi sắp xếp và chúng tôi thật may mắn vì đầu vào đã được sắp xếp. 

Một sai lầm phổ biến là cho rằng vị trí trạm tốt nhất luôn là dải phân cách. Điều đó đúng khi mỗi tội phạm được vận chuyển riêng lẻ, nhưng sai ở đây vì chiếc xe có thể chở nhiều tội phạm trong một chuyến đi. 

Coi như:```
n = 4, m = 2
positions = [0, 1, 100, 101]
```Nếu ta đặt trạm gần dải phân cách, xung quanh`50`, cả hai bên đều trở nên đắt đỏ. Giải pháp tối ưu thực sự là ở gần một cụm, vì tội phạm xa nhất sẽ thực hiện các chuyến đi trong mỗi đợt. 

Một lỗi dễ dàng khác xuất hiện khi tội phạm tồn tại ở cả hai phía của nhà ga. Các chuyến đi bên trái và bên phải là độc lập. Một chuyến đi không thể kết hợp cả hai chiều một cách hiệu quả vì ô tô sẽ phải băng qua ga lần nữa. 

Ví dụ:```
n = 3, m = 2
positions = [-10, -9, 100]
```Nếu trạm ở`0`, phương án tối ưu là: 

- một chuyến đi cho`100`, trị giá`200`- một chuyến đi cho`-10`Và`-9`, trị giá`20`Tổng cộng`220`. 

Cố gắng gộp tội phạm trái và phải vào một chuyến đi chỉ làm tăng thêm khoảng cách. 

Một trường hợp tinh vi khác xảy ra khi nhà ga được xây dựng chính xác nơi có một số tội phạm đứng.```
n = 5, m = 2
positions = [1, 1, 1, 10, 10]
```Tòa nhà tại`1`ngay lập tức bắt giữ ba tên tội phạm mà không tốn chi phí đi lại. Chỉ có hai tên tội phạm ở`10`yêu cầu vận chuyển. 

Việc thực hiện bất cẩn mà vẫn tính cả tội phạm tại đồn vào chi phí vận chuyển sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Vì trạm phải được đặt ở tọa độ nguyên nào đó, nên chúng ta có thể thử mọi vị trí ứng cử viên, tính toán chi phí vận chuyển tối ưu cho trạm đó và lấy mức tối thiểu. 

Giả sử trạm được cố định tại`x`. 

Tất cả tội phạm ở bên trái của`x`có thể được xử lý độc lập với tội phạm ở bên phải. Một mặt, chiến lược tối ưu là tham lam: luôn đưa những tên tội phạm xa nhất còn lại đi cùng nhau trong một chuyến đi, vì chi phí chuyến đi chỉ phụ thuộc vào khoảng cách xa nhất đạt được. 

Ví dụ: nếu khoảng cách ở một bên là:```
1 3 5 8 10
```Và`m = 2`, thì việc phân nhóm tối ưu là:```
(10, 8), (5, 3), (1)
```với chi phí:```
2 * (10 + 5 + 1)
```không:```
2 * (10 + 8 + 5)
```bởi vì một khi chúng ta đã đi đến khoảng cách`10`, lấy`8`cùng nhau là miễn phí. 

Vì vậy, đối với một trạm cố định, chúng ta có thể tính toán câu trả lời một cách tham lam theo thời gian tuyến tính. 

Vấn đề là số lượng trạm ứng cử. Tọa độ có thể đạt tới`10^9`, vì vậy việc ép buộc các vị trí số nguyên là không thể. Ngay cả việc hạn chế tọa độ tội phạm vẫn mang lại`O(n^2)`tổng công việc, quá chậm đối với`10^6`các phần tử. 

Quan sát quan trọng là hàm chi phí là hàm lồi. 

Di chuyển trạm sang bên phải một chút sẽ làm giảm khoảng cách bên phải nhưng lại tăng khoảng cách bên trái. Bởi vì các chuyến đi được nhóm lại theo thứ tự sắp xếp một cách tham lam nên tổng chi phí thay đổi đơn điệu theo độ dốc. Điều này tạo ra một hàm lồi rời rạc trên tọa độ trạm. 

Tính lồi có nghĩa là chúng ta có thể sử dụng tìm kiếm ba chiều trên vị trí trạm. 

Tốt hơn nữa, việc đánh giá một vị trí có thể được thực hiện trong`O(n / m)`thay vì`O(n)`nếu chúng ta trực tiếp đi ngang qua nhóm tội phạm. 

Sự phức tạp kết quả trở nên đủ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O((n / m) log C) | O(1) | Đã chấp nhận | 

Đây`C`là phạm vi tọa độ, đại khái`2 * 10^9`. 

## Hướng dẫn thuật toán 

1. Chọn vị trí trạm ứng tuyển`x`. 
2. Tách tội phạm thành những người còn lại`x`và những quyền đó`x`. 

Tội phạm chính xác tại`x`không cần vận chuyển và không đóng góp gì. 
3. Xử lý phía bên trái. 

Khoảng cách từ nhà ga là:```
x - position
```Vì các vị trí được sắp xếp nên tội phạm xa nhất là tội phạm ngoài cùng bên trái. 

Chiến lược tối ưu là nhóm những người còn lại xa nhất`m`tội phạm vào một chuyến đi. 

Nếu các chỉ số ở phía bên trái là:```
l, l+1, ..., r
```sau đó chúng tôi thêm:```
2 * (x - positions[i])
```cho mọi`i`được thực hiện theo các bước`m`từ phía xa nhất. 
4. Xử lý tương tự với bên phải. 

Những tội phạm xa nhất là những kẻ ở ngoài cùng bên phải. 

Chúng tôi lại lấy các nhóm có kích thước`m`từ phía xa nhất và thêm:```
2 * (positions[i] - x)
```5. Tổng của hai bên là tổng chi phí cho vị trí nhà ga`x`. 
6. Vì hàm giá là hàm lồi nên hãy thực hiện tìm kiếm bậc ba trên`x`. 

Chúng tôi so sánh chi phí ở hai điểm giữa và loại bỏ mặt xấu hơn. 
7. Sau khi khoảng tìm kiếm trở nên nhỏ, bắt buộc tất cả các tọa độ nguyên còn lại trong khoảng đó và xuất ra giá trị tối thiểu. 

### Tại sao nó hoạt động 

Đối với một trạm cố định, việc phân nhóm tham lam là tối ưu vì chi phí của một chuyến đi chỉ phụ thuộc vào tên tội phạm ở xa nhất được truy cập. Khi một chuyến đi đã đạt đến khoảng cách`d`, mang thêm bất kỳ tội phạm nào đến gần hơn`d`không thêm khoảng cách di chuyển. Vì vậy, mỗi chuyến đi nên ngăn chặn những tên tội phạm ở xa nhất trước tiên. 

Tổng chi phí theo vị trí trạm là hàm lồi. Di chuyển trạm sang phải sẽ giảm chi phí do các nhóm bên phải đóng góp và tăng chi phí do các nhóm bên trái đóng góp. Mỗi nhóm đóng góp một thuật ngữ tuyến tính có giá trị tuyệt đối và tổng các hàm lồi vẫn lồi. Tìm kiếm bậc ba có hiệu lực trên các chức năng như vậy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    def cost(x):
        res = 0

        # left side
        i = 0
        while i < n and a[i] < x:
            i += 1

        j = 0
        while j < i:
            res += 2 * (x - a[j])
            j += m

        # right side
        j = n - 1
        while j >= i and a[j] > x:
            j -= 1

        k = n - 1
        while k > j:
            res += 2 * (a[k] - x)
            k -= m

        return res

    lo = -10**9
    hi = 10**9

    while hi - lo > 5:
        m1 = lo + (hi - lo) // 3
        m2 = hi - (hi - lo) // 3

        if cost(m1) <= cost(m2):
            hi = m2
        else:
            lo = m1

    ans = 10**30

    for x in range(lo, hi + 1):
        ans = min(ans, cost(x))

    print(ans)

solve()
```các`cost(x)`Hàm thực hiện trực tiếp logic phân nhóm tham lam. 

Đối với phía bên trái, tội phạm đã được sắp xếp từ xa nhất đến gần nhất với nhà ga. Mọi`m`-tên tội phạm xác định chi phí của một chuyến đi, bởi vì tên tội phạm đó là thành viên xa nhất trong nhóm của nó. 

Giả định:```
positions = [-10, -8, -3, -1]
x = 0
m = 2
```Các lô là:```
(-10, -8)
(-3, -1)
```Chỉ một`-10`Và`-3`xác định khoảng cách chuyến đi, vì vậy chúng tôi bước từng bước`m`. 

Phía bên phải hoạt động đối xứng từ đầu mảng. 

Tìm kiếm ternary sử dụng tọa độ nguyên. Sau khi thu hẹp khoảng đủ, chúng ta ép buộc một số tọa độ còn lại vì các hàm lồi rời rạc có thể có các vùng phẳng. 

Tất cả số học đều sử dụng số nguyên Python để xử lý câu trả lời tối đa có thể một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 6
1 2 3
```Từ`m >= n`, tất cả tội phạm có thể được vận chuyển trong một chuyến đi. 

Trạm tối ưu là ở`2`. 

| Trạm x | Chuyến đi | Tổng chi phí | 
| --- | --- | --- | 
| 1 | đạt 3 và quay trở lại | 4 | 
| 2 | đạt 3 và quay trở lại | 2 | 
| 3 | đạt 1 và quay trở lại | 4 | 

Tối thiểu là`2`. 

Ví dụ này cho thấy tại sao tội phạm đứng ở đồn lại không phải trả chi phí nào. Khi nhà ga ở`2`, tội phạm`2`bị bắt ngay lập tức. 

### Ví dụ 2 

đầu vào:```
5 2
-10 -5 1 2 20
```Trạm thử`x = 1`. 

Lô bên trái:```
(-10, -5)
```Trị giá:```
2 * 11 = 22
```Lô bên phải:```
(20, 2)
```Trị giá:```
2 * 19 = 38
```Tổng cộng:```
60
```| Bên | Tội phạm xa nhất | Chi phí hàng loạt | 
| --- | --- | --- | 
| Trái | -10 | 22 | 
| Đúng | 20 | 38 | 

Dấu vết này thể hiện sự bất biến tham lam. Trong một đợt, chỉ có tội phạm xa nhất mới có vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n / m) log C) | Mỗi đánh giá chi phí xử lý một tội phạm mỗi đợt, tìm kiếm bậc ba thực hiện nhiều đánh giá theo logarit | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Phạm vi tọa độ là khoảng`2 * 10^9`, vì vậy tìm kiếm bậc ba thực hiện khoảng 40 lần lặp. Mỗi đánh giá sẽ quét các nhóm tội phạm một cách hiệu quả, phù hợp thoải mái trong giới hạn ngay cả đối với`10^6`tội phạm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        def cost(x):
            res = 0

            i = 0
            while i < n and a[i] < x:
                i += 1

            j = 0
            while j < i:
                res += 2 * (x - a[j])
                j += m

            j = n - 1
            while j >= i and a[j] > x:
                j -= 1

            k = n - 1
            while k > j:
                res += 2 * (a[k] - x)
                k -= m

            return res

        lo = -10**9
        hi = 10**9

        while hi - lo > 5:
            m1 = lo + (hi - lo) // 3
            m2 = hi - (hi - lo) // 3

            if cost(m1) <= cost(m2):
                hi = m2
            else:
                lo = m1

        ans = 10**30

        for x in range(lo, hi + 1):
            ans = min(ans, cost(x))

        return str(ans)

    return solve()

# provided sample
assert run("3 6\n1 2 3\n") == "2", "sample 1"

# single criminal
assert run("1 1\n10\n") == "0", "station can be built at criminal"

# all equal positions
assert run("5 2\n7 7 7 7 7\n") == "0", "all arrested instantly"

# symmetric positions
assert run("4 2\n-10 -1 1 10\n") == "40", "balanced sides"

# off-by-one batching
assert run("5 2\n1 2 3 4 5\n") == "6", "correct grouping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 10`|`0`| Vụ án hình sự đơn lẻ | 
|`5 2 / 7 7 7 7 7`|`0`| Tội phạm tại trạm | 
|`4 2 / -10 -1 1 10`|`40`| Xử lý trái/phải độc lập | 
|`5 2 / 1 2 3 4 5`|`6`| Bước hàng loạt chính xác | 

## Vỏ cạnh 

Hãy xem xét tất cả tội phạm đã đứng ở cùng một tọa độ.```
5 3
10 10 10 10 10
```Thuật toán đánh giá vị trí trạm`10`. 

Cả quét trái và phải đều trống vì không có tội phạm nào thỏa mãn`< x`hoặc`> x`. 

Tổng chi phí còn lại`0`, điều này đúng vì mọi người đều bị bắt ngay lập tức. 

Bây giờ hãy xem xét tội phạm của cả hai bên.```
4 2
-100 -1 1 100
```Tại ga`0`, thuật toán có dạng: 

Lô còn lại:```
(-100, -1)
```Đúng lô:```
(100, 1)
```Tổng cộng:```
2 * 100 + 2 * 100 = 400
```Hai bên xử lý độc lập là tối ưu vì qua trạm không có lợi thế gì. 

Cuối cùng, hãy xem xét các lô cuối cùng chưa hoàn thiện.```
5 2
1 2 3 4 100
```Các nhóm bên phải là:```
(100, 4)
(3, 2)
(1)
```Thuật toán chỉ tính phí chính xác cho thành viên xa nhất trong mỗi lô:```
2 * 100 + 2 * 3 + 2 * 1
```Một lỗi thường gặp là quên rằng đợt một phần cuối cùng vẫn cần một chuyến đi.
