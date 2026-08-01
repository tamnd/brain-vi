---
title: "CF 102576E - Ô nhiễm"
description: "Thế giới được thể hiện dưới dạng một mặt phẳng chứa nhiều vùng cấm hình tròn. Mỗi vụ nổ tạo ra một khu vực như vậy. Hai con vật trong truy vấn phải di chuyển bên trong một dải ngang, từ ymin đến ymax, không đi vào bất kỳ vòng tròn cấm nào."
date: "2026-07-31T07:33:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102576
codeforces_index: "E"
codeforces_contest_name: "2020 Petrozavodsk Winter Camp, Jagiellonian U Contest"
rating: 0
weight: 102576
solve_time_s: 75
verified: true
draft: false
---

[CF 102576E - Ô nhiễm](https://codeforces.com/problemset/problem/102576/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thế giới được thể hiện dưới dạng một mặt phẳng chứa nhiều vùng cấm hình tròn. Mỗi vụ nổ tạo ra một khu vực như vậy. Hai con vật trong truy vấn phải di chuyển trong một dải ngang, từ`ymin`ĐẾN`ymax`, mà không đi vào bất kỳ vòng cấm nào. Nhiệm vụ là quyết định xem cả hai điểm có thuộc cùng một phần được kết nối của dải đó hay không. 

Quan sát hình học quan trọng là một vòng tròn chỉ có thể chặn chuyển động bên trong một dải ngang khi nó chạm vào cả hai đường viền ngang của dải đó. Nếu một vòng tròn có nhịp dọc`[cy-r, cy+r]`, nó là một bức tường hoàn chỉnh cho một truy vấn chính xác khi:`cy-r <= ymin`Và`cy+r >= ymax`. 

Bởi vì tất cả các khu vực bị ô nhiễm đều rời rạc nên những bức tường này không thể hợp nhất thành những hình dạng phức tạp. Một bức tường ngăn cách dải đất thành bên trái và bên phải. Hai con vật không thể gặp nhau chính xác khi tồn tại một bức tường có tọa độ x ở tâm nằm hoàn toàn giữa tọa độ x của chúng. 

Các ràng buộc buộc phải có giải pháp ngoại tuyến. Với tối đa một triệu vụ nổ và một triệu truy vấn, việc kiểm tra từng vòng kết nối cho mỗi truy vấn sẽ yêu cầu khoảng`10^12`hoạt động, điều đó là không thể. Chúng ta cần một chiến lược tiền xử lý gần với`O((n+q) log n)`. 

Một lỗi phổ biến là chỉ kiểm tra xem vòng tròn chặn có tồn tại trong dải hay không. Điều đó là không đủ vì vòng tròn chặn có thể nằm hoàn toàn ở bên trái hoặc bên phải của cả hai con vật. Một sai lầm khác là đưa vào các vòng tròn có tọa độ trung tâm x bằng một trong các tọa độ của động vật. Trường hợp như vậy không thể xảy ra đối với một điểm động vật hợp lệ bên trong dải nếu vòng tròn kéo dài toàn bộ dải, bởi vì toàn bộ đường thẳng đứng xuyên qua tâm bị ô nhiễm. 

Ví dụ, hãy xem xét:```
1 2
0 0 1
-5 0 5 0 -1 1
```Vòng tròn chặn dải và ngăn cách hai điểm, vì vậy câu trả lời là`NO`. 

Tuy nhiên:```
1 2
0 0 1
-5 0 -3 0 -1 1
```Có cùng một đường tròn nhưng cả hai điểm đều nằm về phía bên trái của nó nên đáp án là`YES`. Giải pháp chỉ kiểm tra xem vòng tròn chặn có tồn tại hay không sẽ không thành công. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi vụ nổ cho mọi truy vấn. Đối với mỗi truy vấn, chúng tôi sẽ kiểm tra xem vụ nổ có bao phủ phạm vi thẳng đứng hay không và liệu tâm của nó có nằm giữa hai tọa độ x hay không. Điều này đúng vì mọi dấu phân cách phải là một trong những vòng tròn có chiều cao đầy đủ này. Tuy nhiên, với`10^6`truy vấn và`10^6`vòng tròn, trường hợp xấu nhất là`10^12`séc. 

Việc giảm thiểu quan trọng là ngừng suy nghĩ về các truy vấn riêng lẻ về mặt hình học và thay vào đó xử lý tất cả các truy vấn có cùng một ranh giới chuyển động. Sắp xếp các truy vấn theo vĩ độ thấp hơn của chúng. Khi chúng ta tăng`ymin`, nhiều vòng tròn có thể trở thành các bức tường phía dưới vì cạnh dưới của chúng hiện nằm dưới đường quét. Đối với mỗi vòng tròn được chèn, chúng tôi lưu trữ cạnh trên của nó ở tọa độ x. 

Tại bất kỳ thời điểm nào trong quá trình quét, cấu trúc chứa chính xác các vòng tròn thỏa mãn:`cy-r <= current_ymin`. 

Đối với một truy vấn với`[ymin, ymax]`, trong số các vòng tròn được chèn đó, chúng ta chỉ cần biết liệu tọa độ x nào đó giữa hai con vật có cạnh trên tối đa hay không`ymax`. Nếu giá trị đó tồn tại, vòng tròn đó sẽ chạm đến đường viền trên và chặn đường. 

Điều này trở thành vấn đề truy vấn phạm vi tối đa trên tọa độ x được nén. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Quét ngoại tuyến bằng cây phân đoạn | O((n+q) log n) | O(n+q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các vòng tròn theo tọa độ dưới cùng của chúng`cy-r`. Sắp xếp tất cả các truy vấn theo`ymin`. Trong quá trình quét, hãy chèn các vòng tròn có đáy đã nằm dưới ranh giới phía dưới của truy vấn hiện tại. 
2. Nén tất cả tọa độ x tâm vòng tròn. Cây phân đoạn lưu trữ, đối với mỗi vị trí x được nén, giá trị lớn nhất`cy+r`giữa các vòng tròn được chèn với tâm đó. 
3. Đối với mỗi truy vấn, hãy tìm phạm vi nén của tọa độ x tâm giữa hai vị trí con vật. Nếu phạm vi này trống thì không có vòng tròn nào có thể tách chúng ra. 
4. Truy vấn cây phân đoạn trên phạm vi x đó. Nếu tọa độ trên cùng được lưu tối đa ít nhất là`ymax`, tồn tại một bức tường ô nhiễm có chiều cao tối đa giữa các con vật, vì vậy câu trả lời là`NO`. Nếu không thì câu trả lời là`YES`. 

Tại sao nó hoạt động: 

Tính bất biến của quá trình quét là sau khi xử lý tất cả các vòng tròn với`cy-r <= ymin`, cây phân đoạn chứa chính xác các vòng tròn có thể chạm vào đáy của dải hiện tại. Truy vấn chỉ thất bại khi một trong các vòng kết nối đó cũng đạt tới`ymax`và tọa độ x trung tâm của nó nằm giữa hai con vật. Truy vấn phạm vi tối đa kiểm tra chính xác điều kiện này, vì vậy mọi câu trả lời trả về đều khớp với kết nối thực tế của dải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, n):
        self.n = 1
        while self.n < n:
            self.n *= 2
        self.t = [-(10**30)] * (2 * self.n)

    def update(self, i, v):
        i += self.n
        if self.t[i] >= v:
            return
        self.t[i] = v
        i //= 2
        while i:
            nv = self.t[2 * i]
            if self.t[2 * i + 1] > nv:
                nv = self.t[2 * i + 1]
            if self.t[i] == nv:
                break
            self.t[i] = nv
            i //= 2

    def query(self, l, r):
        if l >= r:
            return -(10**30)
        l += self.n
        r += self.n
        ans = -(10**30)
        while l < r:
            if l & 1:
                if self.t[l] > ans:
                    ans = self.t[l]
                l += 1
            if r & 1:
                r -= 1
                if self.t[r] > ans:
                    ans = self.t[r]
            l //= 2
            r //= 2
        return ans

n, q = map(int, input().split())

circles = []
xs = []

for _ in range(n):
    cx, cy, r = map(int, input().split())
    circles.append((cy - r, cy + r, cx))
    xs.append(cx)

xs.sort()
unique_x = []
for x in xs:
    if not unique_x or unique_x[-1] != x:
        unique_x.append(x)

circles.sort()

queries = []
for i in range(q):
    px, py, qx, qy, ymin, ymax = map(int, input().split())
    queries.append((ymin, ymax, px, qx, i))

queries.sort()

seg = SegmentTree(len(unique_x))
ans = ["YES"] * q
ptr = 0

import bisect

for ymin, ymax, px, qx, idx in queries:
    while ptr < n and circles[ptr][0] <= ymin:
        bottom, top, cx = circles[ptr]
        seg.update(bisect.bisect_left(unique_x, cx), top)
        ptr += 1

    if px > qx:
        px, qx = qx, px

    left = bisect.bisect_right(unique_x, px)
    right = bisect.bisect_left(unique_x, qx)

    if seg.query(left, right) >= ymax:
        ans[idx] = "NO"

print("\n".join(ans))
```Con trỏ quét chỉ di chuyển về phía trước nên mỗi vòng tròn được chèn đúng một lần. Bản cập nhật cây phân đoạn giữ cạnh trên tối đa cho mỗi tọa độ x vì trong số một số vòng tròn có cùng tâm, chỉ vòng tròn cao nhất mới có thể chặn truy vấn. 

Việc sử dụng`bisect_right`Và`bisect_left`là cố ý. Truy vấn yêu cầu các trung tâm nằm giữa hai con vật. Bao gồm một điểm cuối sẽ tạo ra các rào cản sai lầm. 

Tất cả các tọa độ vừa vặn thoải mái bên trong số nguyên Python. Thuật toán không bao giờ tính toán khoảng cách bình phương hoặc thực hiện các phép tính dấu phẩy động, do đó tránh được các vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu, các vòng tròn là: 

| Trung tâm x | Dưới cùng | Đầu trang | 
| --- | --- | --- | 
| 3 | 1 | 5 | 
| 7 | 4 | 10 | 
| 12 | 3 | 7 | 

Đối với truy vấn đầu tiên, dải này là`[2,6]`. 

| Bước | Ymin hiện tại | Đã chèn vòng tròn | Đỉnh tối đa giữa x=1 và x=14 | Kết quả | 
| --- | --- | --- | --- | --- | 
| Quét đạt 2 | 2 | x=3 | 5 | Tiếp tục | 
| Truy vấn ymax | 6 | x=3,x=12 đã được kiểm tra | 5 | CÓ | 

Đường tròn tại x=3 chạm tới cạnh dưới chứ không chạm đến cạnh trên nên không thể chia đôi dải. 

Đối với truy vấn thứ hai, dải này là`[4,7]`. 

| Bước | Ymin hiện tại | Đã chèn vòng tròn | Đỉnh tối đa giữa x=1 và x=14 | Kết quả | 
| --- | --- | --- | --- | --- | 
| Quét đạt 4 | 4 | x=3,x=7,x=12 | 10 | KHÔNG | 

Vòng tròn có tâm x=7 bao phủ toàn bộ dải đất và nằm giữa các con vật, tạo thành một bức tường không thể vượt qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+q) log n) | Mỗi cập nhật và truy vấn vòng tròn đều là logarit | 
| Không gian | O(n+q) | Lưu trữ vòng kết nối, truy vấn, tọa độ nén và câu trả lời | 

Giải pháp xử lý hai triệu đối tượng bằng các phép toán logarit, phù hợp với các giới hạn đã cho. Việc sử dụng bộ nhớ là tuyến tính. 

## Trường hợp thử nghiệm```
# The official solution is designed for direct stdin/stdout execution.
# These examples describe expected behavior.

# Minimum case:
# 1 1
# 0 0 1
# -2 0 2 0 -1 1
# Expected:
# NO

# No separating circle:
# 1 1
# 0 0 1
# -5 0 -3 0 -1 1
# Expected:
# YES

# Circle touches lower boundary only:
# 1 1
# 0 0 2
# -5 2 5 2 2 3
# Expected:
# YES

# Multiple circles, only one is relevant:
# 2 1
# -10 0 1
# 5 5 5
# -20 0 20 0 0 9
# Expected:
# NO
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vòng tròn có chiều cao đầy đủ đơn | KHÔNG | Phát hiện dấu phân cách cơ bản | 
| Vòng tròn bên ngoài phạm vi x | CÓ | Chỉ có bức tường giữa các loài động vật mới quan trọng | 
| Vòng tròn chạm một đường viền | CÓ | Đúng điều kiện khoảng dọc | 
| Một số vòng kết nối | KHÔNG | Cấu trúc quét xử lý nhiều chướng ngại vật | 

## Vỏ cạnh 

Vòng tròn chạm chính xác vào cả hai đường viền phải được tính là một bức tường. Điều kiện sử dụng`<=`Và`>=`, không phải là bất đẳng thức chặt chẽ. Đối với một vòng tròn có nhịp dọc`[0,10]`và một dải truy vấn`[0,10]`, vòng tròn chặn chuyển động. 

Vòng tròn chỉ chạm vào một đường viền sẽ không được tính. Một vòng tròn có nhịp thẳng đứng`[0,5]`không thể ngăn chặn chuyển động trong một dải`[0,10]`bởi vì một con vật có thể đi vòng quanh đỉnh của nó. 

Một số vòng tròn có cùng tọa độ x được xử lý bằng cách giữ giá trị lớn nhất trên cùng. Vòng tròn cao nhất như vậy sẽ thống trị tất cả các vòng tròn thấp hơn cho mọi truy vấn trong tương lai. 

Các truy vấn được trả lời độc lập với thứ tự chúng xuất hiện. Quét ngoại tuyến khôi phục thứ tự ban đầu bằng cách sử dụng chỉ mục truy vấn được lưu trữ trước khi in.
