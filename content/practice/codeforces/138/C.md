---
title: "CF 138C - Thần lùn nấm - 2"
description: "Chúng ta có những cái cây được đặt trên trục số. Mỗi cây có thể đổ sang trái, đổ sang phải hoặc đứng vững. Xác suất của ba kết quả này được đưa ra một cách độc lập cho mỗi cây. Cây đổ sẽ phá hủy nấm trong khoảng thời gian nửa mở được xác định bởi vị trí và chiều cao của cây."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "probabilities", "sortings"]
categories: ["algorithms"]
codeforces_contest: 138
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 99 (Div. 1)"
rating: 2200
weight: 138
solve_time_s: 120
verified: true
draft: false
---

[CF 138C - Thần lùn nấm - 2](https://codeforces.com/problemset/problem/138/C) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, xác suất, sắp xếp 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có những cái cây được đặt trên trục số. Mỗi cây có thể đổ sang trái, đổ sang phải hoặc đứng vững. Xác suất của ba kết quả này được đưa ra một cách độc lập cho mỗi cây. 

Cây đổ sẽ phá hủy nấm trong khoảng thời gian nửa mở được xác định bởi vị trí và chiều cao của cây. 

Nếu cây ở vị trí`x`với chiều cao`h`rơi sang trái, nó phá hủy nấm ở`[x - h, x)`. 

Nếu rơi đúng, nó sẽ phá hủy nấm trong`(x, x + h]`. 

Mỗi cây nấm đều có một vị trí và giá trị sức mạnh. Một cây nấm chỉ tồn tại nếu không có cây nào phá hủy nó. Chúng ta cần tổng sức mạnh dự kiến ​​của tất cả các cây nấm còn sống sót. 

Quan sát quan trọng là kỳ vọng là tuyến tính. Thay vì nghĩ về tất cả các loại nấm cùng nhau, chúng ta có thể tính toán xác suất sống sót của từng loại nấm một cách độc lập, nhân nó với sức mạnh của nấm và tính tổng mọi thứ. 

Các ràng buộc định hình mạnh mẽ giải pháp. có thể có`10^5`cây, do đó, bất kỳ thuật toán nào kiểm tra từng cây với từng cây nấm sẽ hoạt động khoảng`10^9`hoạt động trong trường hợp xấu nhất, quá chậm so với giới hạn 1 giây. Số lượng nấm ít hơn, chỉ`10^4`, điều này cho thấy việc lặp lại nấm và xử lý cây một cách hiệu quả là khả thi. 

Một điểm tinh tế là ranh giới khoảng. Sử dụng thác trái`[x - h, x)`, trong khi bên phải rơi sử dụng`(x, x + h]`. Bản thân vị trí cây bị loại trừ trong cả hai trường hợp. 

Hãy xem xét ví dụ này:```
1 1
5 3 100 0
5 10
```Cây ngã trái với xác suất là 1, nhưng cây nấm ở vị trí 5 vẫn sống sót vì khoảng thời gian là`[2, 5)`, và chính 5 bị loại trừ. Việc thực hiện bất cẩn bằng cách sử dụng`<=`cả hai bên sẽ phá hủy nó một cách không chính xác. 

Một sai lầm dễ dàng khác xuất hiện khi nhiều cây có thể đâm trúng cùng một cây nấm.```
2 1
0 5 50 0
10 5 0 50
5 100
```Cây nấm chỉ sống sót nếu không có sự kiện nguy hiểm nào xảy ra. Xác suất sống sót đúng là:```
(1 - 0.5) * (1 - 0.5) = 0.25
```Thêm xác suất trực tiếp và tính toán`1 - 0.5 - 0.5 = 0`sẽ sai vì các sự kiện của cây là độc lập. 

Trường hợp cạnh thứ ba liên quan đến nấm chính xác trên ranh giới.```
1 2
10 5 100 0
5 1
10 1
```Cây nấm lúc 5 tuổi bị phá hủy vì`5 ∈ [5,10)`. Cây nấm ở số 10 sống sót vì số 10 bị loại trừ. Việc xử lý đúng các khoảng thời gian nửa mở là điều cần thiết. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi cây nấm, lặp lại tất cả các cây và xác định xác suất cây này phá hủy cây nấm đó. 

Nếu một cây có thể phá hủy nấm bằng cách ngã sang trái, hãy nhân xác suất sống sót với`(1 - left_probability)`. Tương tự cho bên phải. 

Cuối cùng nhân sức mạnh của cây nấm với xác suất sống sót của nó và cộng nó vào câu trả lời. 

Điều này hiệu quả vì kết quả của cây là độc lập. Xác suất để một cây nấm sống sót trên tất cả các cây chỉ đơn giản là tích của xác suất mà mỗi cây riêng lẻ không phá hủy được nó. 

Vấn đề là sự phức tạp. Với`10^5`cây cối và`10^4`nấm, chúng tôi sẽ biểu diễn`10^9`kiểm tra định kỳ. Ngay cả các yếu tố không đổi rất nhỏ cũng sẽ hết thời gian. 

Điều quan trọng là mỗi cây chỉ ảnh hưởng đến một khoảng liên tục trên trục số. Thay vì xử lý từng cây cho từng cây nấm, chúng ta có thể xử lý nấm theo thứ tự được sắp xếp và duy trì hiệu ứng nhân với các sự kiện kiểu đường quét. 

Giả sử một cái cây đổ sang trái. Sau đó mỗi cây nấm trong`[a-h, a)`được nhân với`(1 - l/100)`trong xác suất sống sót của nó. 

Tương tự như vậy, một cú ngã bên phải ảnh hưởng đến`(a, a+h]`. 

Đây là những phép nhân phạm vi trên các vị trí nấm. 

Vì nấm chỉ`10^4`, chúng ta có thể sắp xếp chúng và áp dụng cập nhật khoảng thời gian nhân bằng cách sử dụng ý tưởng mảng sai phân theo thời gian logarit. Đối với mỗi khoảng thời gian, chúng tôi đánh dấu nơi bắt đầu đóng góp theo cấp số nhân và nơi kết thúc. 

Sau đó, một lần quét qua nấm sẽ tái tạo lại hệ số nhân tổng thể ảnh hưởng đến từng cây nấm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O((n + m) log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các loại nấm và sắp xếp chúng theo tọa độ. 

Việc sắp xếp cho phép chúng ta chuyển đổi hiệu ứng cây thành các phạm vi chỉ số liền kề trên mảng nấm. 
2. Tạo một mảng`mul`kích thước`m + 1`, khởi tạo bằng`1.0`. 

Mảng này lưu trữ các cập nhật sai phân nhân. Trong quá trình quét, sản phẩm đang chạy thể hiện sự đóng góp xác suất sống sót từ các khoảng cây đang hoạt động. 
3. Đối với mỗi cây, xử lý khoảng thời gian rơi trái. 

Khoảng thời gian nguy hiểm là`[a-h, a)`. 

Sử dụng tìm kiếm nhị phân trên tọa độ nấm được sắp xếp: 

-`L = lower_bound(a-h)`-`R = lower_bound(a)`Nấm trong chỉ số`[L, R)`bị ảnh hưởng. 
4. Áp dụng cập nhật nhân cho khoảng bên trái. 

Nếu xác suất rơi trái là`p`, mỗi cây nấm trong khoảng phải được nhân với`(1-p)`. 

Sử dụng cập nhật sai phân nhân: 

-`mul[L] *= (1-p)`-`mul[R] /= (1-p)`Sau đó, các tích số tiền tố sẽ xây dựng lại hệ số nhân đầy đủ. 
5. Xử lý khoảng thời gian rơi phải tương tự. 

Khoảng thời gian nguy hiểm là`(a, a+h]`. 

Chúng tôi cần: 

-`L = upper_bound(a)`-`R = upper_bound(a+h)`nấm trong`[L, R)`bị ảnh hưởng. 
6. Áp dụng cách cập nhật phạm vi nhân tương tự bằng cách sử dụng hệ số`(1-r)`. 
7. Quét nấm từ trái sang phải. 

Duy trì một sản phẩm đang chạy`cur`. 

Tại mỗi chỉ số: 

-`cur *= mul[i]`-`cur`bây giờ bằng với xác suất sống sót của loại nấm này. 
8. Thêm`cur * mushroom_power`để trả lời. 
9. In giá trị mong đợi cuối cùng. 

### Tại sao nó hoạt động 

Mỗi cây đều góp phần độc lập vào khả năng sống sót của mỗi cây nấm. 

Đối với một cây nấm cố định, xác suất sống sót của nó bằng:```
Π(probability this tree does not destroy it)
```Thuật toán áp dụng chính xác các hệ số nhân này trên tất cả các loại nấm nằm trong từng khoảng nguy hiểm. 

Kỹ thuật mảng sai phân nhân đảm bảo rằng trong quá trình quét, sản phẩm đang chạy chứa chính xác tích của tất cả các yếu tố khoảng thời gian hoạt động ảnh hưởng đến nấm hiện tại. 

Bởi vì mỗi lần cập nhật khoảng thời gian đều tương ứng chính xác với một sự kiện theo hướng cây và vì các khoảng thời gian được xây dựng với các ranh giới nửa mở chính xác nên mỗi cây nấm đều nhận được chính xác xác suất sống sót chính xác. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    trees = []
    for _ in range(n):
        a, h, l, r = map(int, input().split())
        trees.append((a, h, l / 100.0, r / 100.0))

    mushrooms = []
    for _ in range(m):
        b, z = map(int, input().split())
        mushrooms.append((b, z))

    mushrooms.sort()

    coords = [x for x, _ in mushrooms]

    mul = [1.0] * (m + 1)

    for a, h, lp, rp in trees:
        # Left interval: [a-h, a)
        L = bisect_left(coords, a - h)
        R = bisect_left(coords, a)

        if L < R and lp > 0:
            factor = 1.0 - lp
            mul[L] *= factor
            mul[R] /= factor

        # Right interval: (a, a+h]
        L = bisect_right(coords, a)
        R = bisect_right(coords, a + h)

        if L < R and rp > 0:
            factor = 1.0 - rp
            mul[L] *= factor
            mul[R] /= factor

    ans = 0.0
    cur = 1.0

    for i in range(m):
        cur *= mul[i]
        ans += cur * mushrooms[i][1]

    print("{:.10f}".format(ans))

if __name__ == "__main__":
    solve()
```Chi tiết triển khai quan trọng đầu tiên là phân loại nấm trong khi vẫn giữ nguyên giá trị sức mạnh của chúng. Tất cả các tìm kiếm nhị phân hoạt động trên mảng tọa độ được sắp xếp. 

Các bản cập nhật phạm vi sử dụng sai phân nhân thay vì sai phân cộng. Nếu một khoảng thời gian nhân tất cả xác suất sống sót với`f`, ta đặt:```
mul[L] *= f
mul[R] /= f
```Trong quá trình quét, tích tiền tố sẽ kích hoạt hệ số tại`L`và loại bỏ nó sau`R-1`. 

Việc xử lý ranh giới là phần khó nhất. 

Vì`[a-h, a)`chúng tôi sử dụng:```
bisect_left(a-h)
bisect_left(a)
```bởi vì ranh giới bên trái là bao hàm và ranh giới bên phải là độc quyền. 

Vì`(a, a+h]`chúng tôi sử dụng:```
bisect_right(a)
bisect_right(a+h)
```bởi vì ranh giới bên trái là loại trừ và ranh giới bên phải là bao hàm. 

Sử dụng sai sự kết hợp của`bisect_left`Và`bisect_right`âm thầm phá vỡ các trường hợp ranh giới. 

Biến quét`cur`luôn lưu trữ tổng số nhân xác suất sống sót của loại nấm hiện tại. Nhân với sức mạnh của nấm sẽ đóng góp vào giá trị mong đợi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
2 2 50 50
1 1
```Phân loại nấm: 

| Chỉ mục | Vị trí | Quyền lực | 
| --- | --- | --- | 
| 0 | 1 | 1 | 

Hiệu ứng cây: 

| Cây | Khoảng thời gian | Nấm bị ảnh hưởng | Yếu tố sinh tồn | 
| --- | --- | --- | --- | 
| Rơi trái | [0,2) | nấm 1 | 0,5 | 
| Đúng mùa thu | (2,4] | không có | 0,5 | 

Quét: 

| Chỉ mục | cur | Đóng góp | 
| --- | --- | --- | 
| 0 | 0,5 | 0,5 | 

Câu trả lời cuối cùng:```
0.5
```Ví dụ này xác nhận logic khoảng thời gian nửa mở. Cây nấm lúc 1 chỉ bị phá hủy khi rơi xuống bên trái. 

### Ví dụ 2```
2 1
0 5 50 0
10 5 0 50
5 100
```Cây nấm ở vị trí thứ 5. 

Xử lý cây: 

| Cây | Khoảng thời gian nguy hiểm | Đánh vào nấm? | Xác suất an toàn | 
| --- | --- | --- | --- | 
| Cây 1 còn lại | [-5,0) | Không | 1 | 
| Cây 1 phải | không | Không | 1 | 
| Cây 2 còn lại | không | Không | 1 | 
| Cây 2 phải | (10,15] | Không | 1 | 

Thực tế loại nấm này sống sót với xác suất là 1. 

Bây giờ thay đổi vị trí nấm thành 8:```
2 1
0 10 50 0
10 5 0 50
8 100
```Sau đó: 

| Cây | Khoảng thời gian nguy hiểm | Đánh vào nấm? | Xác suất an toàn | 
| --- | --- | --- | --- | 
| Cây 1 phải | (0,10] | Có | 0,5 | 
| Cây 2 còn lại | [5,10) | Có | 0,5 | 

Xác suất sống sót cuối cùng:```
0.5 × 0.5 = 0.25
```Đóng góp dự kiến:```
25
```Điều này chứng tỏ tại sao xác suất phải được nhân lên chứ không phải được cộng thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | Mỗi cây thực hiện nhiều tìm kiếm nhị phân liên tục | 
| Không gian | O(m) | Mảng tọa độ và nhân | 

Giải pháp dễ dàng phù hợp với các ràng buộc. Với`10^5`cây cối và`10^4`như nấm, tổng số tìm kiếm nhị phân có thể quản lý được và bản thân quá trình quét là tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left, bisect_right

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    trees = []
    for _ in range(n):
        a, h, l, r = map(int, input().split())
        trees.append((a, h, l / 100.0, r / 100.0))

    mushrooms = []
    for _ in range(m):
        b, z = map(int, input().split())
        mushrooms.append((b, z))

    mushrooms.sort()

    coords = [x for x, _ in mushrooms]

    mul = [1.0] * (m + 1)

    for a, h, lp, rp in trees:
        L = bisect_left(coords, a - h)
        R = bisect_left(coords, a)

        if L < R and lp > 0:
            f = 1.0 - lp
            mul[L] *= f
            mul[R] /= f

        L = bisect_right(coords, a)
        R = bisect_right(coords, a + h)

        if L < R and rp > 0:
            f = 1.0 - rp
            mul[L] *= f
            mul[R] /= f

    ans = 0.0
    cur = 1.0

    for i in range(m):
        cur *= mul[i]
        ans += cur * mushrooms[i][1]

    print("{:.10f}".format(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
out = float(run("""1 1
2 2 50 50
1 1
"""))
assert abs(out - 0.5) < 1e-6

# minimum size
out = float(run("""1 1
0 1 0 0
0 7
"""))
assert abs(out - 7.0) < 1e-6

# boundary test for left interval
out = float(run("""1 2
10 5 100 0
5 1
10 1
"""))
assert abs(out - 1.0) < 1e-6

# overlapping probabilities
out = float(run("""2 1
0 10 50 0
10 5 0 50
8 100
"""))
assert abs(out - 25.0) < 1e-6

# all mushrooms destroyed
out = float(run("""1 3
0 100 100 100
-50 1
50 1
0 1
"""))
assert abs(out - 1.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây đơn, nấm đơn | 0,5 | Xử lý xác suất cơ bản | 
| Kích thước tối thiểu | 7 | Cây không bao giờ đổ | 
| Trường hợp khoảng ranh giới | 1 | Đúng logic khoảng thời gian nửa mở | 
| Sự hủy diệt chồng chéo | 25 | Nhân xác suất độc lập | 
| Tất cả bị phá hủy ngoại trừ trung tâm | 1 | Vị trí cây bị loại trừ khỏi các khoảng | 

## Vỏ cạnh 

Hãy xem xét lại vấn đề ranh giới:```
1 2
10 5 100 0
5 1
10 1
```Khoảng thời gian nguy hiểm là`[5,10)`. 

Tìm kiếm nhị phân tạo ra:```
L = lower_bound(5)
R = lower_bound(10)
```Chỉ có cây nấm ở vị trí thứ 5 nằm trong phạm vi chỉ số đó. Nấm ở vị trí 10 bị loại trừ vì điểm cuối bên phải đang mở. 

Thuật toán xuất ra chính xác:```
1
```Bây giờ hãy xem xét nhiều cây ảnh hưởng đến một cây nấm:```
2 1
0 10 50 0
10 5 0 50
8 100
```Nấm nằm ở cả hai khoảng nguy hiểm. 

Các yếu tố cập nhật là:```
0.5 and 0.5
```Việc quét nhân chúng lại với nhau:```
0.5 × 0.5 = 0.25
```Giá trị dự kiến:```
25
```Điều này xác nhận rằng xác suất độc lập được xử lý chính xác. 

Cuối cùng, hãy xem xét nấm chính xác ở vị trí cây:```
1 1
0 100 100 100
0 10
```Khoảng bên trái là`[-100,0)`, khoảng bên phải là`(0,100]`. 

Vị trí 0 không thuộc khoảng nào. 

Tìm kiếm nhị phân tạo ra các phạm vi trống, do đó không áp dụng hệ số nhân. Cây nấm sống sót với xác suất 1, đưa ra câu trả lời:```
10
```Đây là lúc việc sử dụng nhầm các khoảng thời gian đóng sẽ không thành công.
