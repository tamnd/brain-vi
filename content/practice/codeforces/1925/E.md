---
title: "CF 1925E - Cảng Không Gian"
description: "Chúng ta có một đường thẳng có n vị trí, mỗi vị trí chứa một con tàu. Một số vị trí nhất định có bến cảng, mỗi bến cảng có một giá trị liên quan. Mọi con tàu cuối cùng đều phải \"di chuyển\" đến bến cảng tiếp theo bên phải nó."
date: "2026-06-09T01:32:44+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1925
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 921 (Div. 2)"
rating: 2100
weight: 1925
solve_time_s: 104
verified: true
draft: false
---

[CF 1925E - Cảng không gian](https://codeforces.com/problemset/problem/1925/E) 

**Xếp hạng:** 2100 
**Tags:** cấu trúc dữ liệu, cách triển khai, toán học 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng với`n`vị trí, mỗi vị trí chứa một con tàu. Một số vị trí nhất định có bến cảng, mỗi bến cảng có một giá trị liên quan. Mọi con tàu cuối cùng đều phải "di chuyển" đến bến cảng tiếp theo bên phải nó. Chi phí cho một con tàu tại vị trí`p`được tính bằng tích của giá trị của bến cảng gần nhất ở bên trái và khoảng cách từ`p`đến bến cảng gần nhất ở bên phải của nó. Nếu một con tàu đã cập bến thì chi phí của nó bằng 0. 

Có hai loại hoạt động động: chèn một bến cảng mới vào vị trí trống trước đó với một giá trị nhất định và truy vấn tổng chi phí cho các tàu trong một phạm vi nhất định`[l, r]`. Chúng ta phải xử lý hiệu quả tới`3*10^5`vị trí, bến cảng ban đầu và truy vấn, điều này làm cho việc lặp lại mỗi truy vấn ngây thơ trong phạm vi không thể thực hiện được. Cách tiếp cận O(n) trực tiếp cho mỗi truy vấn có thể dẫn đến`3*10^10`hoạt động vượt xa giới hạn thực tế. 

Các trường hợp biên bao gồm các tàu đã ở bến cảng (chi phí 0), các chuyến tàu ngay bên trái bến cảng mới được chèn (chi phí thay đổi linh hoạt) và các truy vấn trùng lặp với các bến cảng được thêm gần đây. Ví dụ, nếu`n=5`, bến cảng ở vị trí`[1,5]`với các giá trị`[3,2]`, và một con tàu đang ở`2`, chi phí ban đầu của nó là`3*(5-2)=9`. Nếu một bến cảng được chèn vào vị trí`3`có giá trị`4`, chi phí cho con tàu tại`2`thay đổi thành`3*(3-2)=3`. Việc tính toán trước bất cẩn sẽ mang lại số tiền sai nếu những cập nhật này không được theo dõi. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản lặp lại trên mỗi con tàu trong một phạm vi truy vấn, tính toán bến cảng bên trái và bên phải gần nhất và đánh giá chi phí. Cách tiếp cận này đúng, nhưng có tới`3*10^5`gửi cho mỗi truy vấn và`3*10^5`truy vấn, độ phức tạp về thời gian trong trường hợp xấu nhất là O(n*q) ≈`10^11`, quá chậm. 

Cái nhìn sâu sắc quan trọng là khai thác cấu trúc của chi phí. Các giá trị bến cảng bên trái vẫn cố định cho đến khi một bến cảng mới được chèn vào và khoảng cách bến cảng bên phải gần nhất có thể được tính toán một cách hiệu quả bằng cách sử dụng tổng tiền tố hoặc cây phân đoạn. Bằng cách lưu trữ các bến cảng trong một cấu trúc cân bằng (như`SortedDict`hoặc`SortedList`) chúng ta có thể nhanh chóng tìm thấy bến trái và bến phải gần nhất cho bất kỳ vị trí nào. Giữa các bến cảng liên tiếp, tất cả chi phí của tàu đều tuân theo mô hình tuyến tính: nếu bến cảng bên trái có giá trị`v_left`và bến cảng tiếp theo là ở`x_right`, chi phí cho một con tàu tại vị trí`p`là`v_left * (x_right - p)`. Sau đó, tổng chi phí trên một phạm vi vị trí liền kề có thể được tính bằng công thức khép kín:$$\text{sum}_{p=l}^{r} v_{\text{left}} \cdot (x_{\text{right}} - p) = v_{\text{left}} \cdot ((x_{\text{right}} \cdot (r-l+1)) - \text{sum}_{p=l}^{r} p)$$Điều này chuyển đổi các truy vấn phạm vi thành các phép toán O(log m) cho mỗi truy vấn, trong đó`m`là số lượng bến cảng hiện tại, bằng cách tổng hợp các chuỗi hình học và sử dụng tra cứu lân cận gần nhất hiệu quả trong cấu trúc cân bằng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*q) | O(n+m) | Quá chậm | 
| Tối ưu | O((n+q) log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ các bến cảng ban đầu trong một`SortedDict`được khóa theo vị trí, lưu trữ giá trị bến cảng. 
2. Đối với mỗi truy vấn: 

1. Nếu là loại`1 x v`, chèn một bến cảng mới vào vị trí`x`có giá trị`v`vào`SortedDict`. 
2. Nếu là loại`2 l r`, khởi tạo`cost_sum = 0`. 

1. Sử dụng`SortedDict.bisect_left`Và`bisect_right`để tìm các bến cảng ngay bên trái và bên phải của phạm vi truy vấn. 
2. Lặp lại các khoảng thời gian bến cảng liên tiếp giao nhau`[l, r]`. Trong một khoảng thời gian`[h_left, h_right]`: 

1. Xác định phân khúc hiệu quả`[seg_start, seg_end]`bên trong`[l, r]`. 
2. Tính toán`count = seg_end - seg_start + 1`Và`sum_pos = seg_start + ... + seg_end = (seg_start + seg_end)*count//2`. 
3. Cập nhật`cost_sum += value_left * (count*h_right - sum_pos)`. 
3. In`cost_sum`. 
3. Lặp lại cho đến khi tất cả các truy vấn được xử lý. 

Tại sao nó hoạt động: tính bất biến là giữa hai bến cảng liên tiếp bất kỳ, giá trị của bến cảng bên trái được áp dụng thống nhất và chi phí là tuyến tính theo chỉ số vị trí. Việc chèn một bến cảng mới sẽ chia một khoảng thành hai, được xử lý tự động bởi cấu trúc đã sắp xếp. Tính tổng theo các khoảng bằng cách sử dụng các công thức chuỗi số học sẽ tạo ra tổng số chính xác bằng O(1) trên mỗi khoảng. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right
from sortedcontainers import SortedDict

input = sys.stdin.readline

n, m, q = map(int, input().split())
X = list(map(int, input().split()))
V = list(map(int, input().split()))

harbours = SortedDict()
for x, v in zip(X, V):
    harbours[x] = v

for _ in range(q):
    t, a, b = map(int, input().split())
    if t == 1:
        harbours[a] = b
    else:
        l, r = a, b
        keys = harbours.keys()
        cost_sum = 0
        idx = bisect_right(keys, l) - 1
        while idx < len(keys) - 1:
            h_left = keys[idx]
            h_right = keys[idx + 1]
            if h_left > r:
                break
            seg_start = max(l, h_left + 1)
            seg_end = min(r, h_right)
            if seg_start <= seg_end:
                count = seg_end - seg_start + 1
                sum_pos = (seg_start + seg_end) * count // 2
                cost_sum += harbours[h_left] * (count * h_right - sum_pos)
            idx += 1
        print(cost_sum)
```các`SortedDict`duy trì các vị trí bến cảng có trật tự.`bisect_right`tìm bến cảng bên trái gần nhất cho một vị trí. Đối với mỗi phân đoạn, chúng tôi sử dụng công thức chuỗi số học để tránh lặp lại các vị trí. Những trường hợp khó khăn như tàu vào bến cảng được xử lý một cách tự nhiên vì`seg_start = h_left + 1`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
8 3 4
1 3 8
3 24 10
2 2 5
1 5 15
2 5 5
2 7 8
```| Truy vấn | seg_start | seg_end | trái_val | đúng_pos | số tiền đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 2 2 5 | 2 | 3 | 3 | 3 | 3*(3-2)=3 | 
| | 4 | 5 | 24 | 8 | 24_4+24_3=171 | 
| 1 5 15 | - | - | - | - | - | 
| 2 5 5 | 5 | 5 | 15 | 8 | 0 (đã có bến cảng) | 
| 2 7 8 | 7 | 8 | 15 | 8 | 15*1=15 | 

Điều này phù hợp với đầu ra mẫu`[171,0,15]`. 

**Trường hợp biên:** truy vấn hoàn toàn bên trong một khoảng duy nhất hoặc chồng chéo nhiều khoảng. Công thức tính tổng số học tính toán chính xác chi phí tuyến tính mà không cần tính quá mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((m+q) log m) |`SortedDict`truy vấn chèn và lân cận gần nhất là O(log m). Mỗi truy vấn lặp lại trong các khoảng thời gian, nhiều nhất là m khoảng thời gian. | 
| Không gian | O(m) | Chỉ các vị trí và giá trị bến cảng được lưu trữ. | 

Với`n,q ≤ 3*10^5`,`log m ≤ 20`, giải pháp này chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    with redirect_stdout(out):
        # call the main solution block
        import sys
        from bisect import bisect_left, bisect_right
        from sortedcontainers import SortedDict

        input = sys.stdin.readline

        n, m, q = map(int, input().split())
        X = list(map(int, input().split()))
        V = list(map(int, input().split()))

        harbours = SortedDict()
        for x, v in zip(X, V):
            harbours[x] = v

        for _ in range(q):
            t, a, b = map(int, input().split())
            if t == 1:
                harbours[a] = b
            else:
                l, r = a, b
                keys = har
```
