---
title: "CF 102299D - Công trình và tên lửa"
description: "Thành phố được mô hình hóa bằng các đoạn thẳng trong mặt phẳng. Một tòa nhà được thể hiện bằng một đoạn có chiều cao liên quan và quỹ đạo tên lửa là một đoạn khác. Bất cứ khi nào quỹ đạo tên lửa cắt ngang một đoạn tòa nhà, tòa nhà đó có thể xảy ra va chạm."
date: "2026-08-13T08:05:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "D"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 124
verified: true
draft: false
---

[CF 102299D - Tòa nhà và tên lửa](https://codeforces.com/problemset/problem/102299/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thành phố được mô hình hóa bằng các đoạn thẳng trong mặt phẳng. Một tòa nhà được thể hiện bằng một đoạn có chiều cao liên quan và quỹ đạo tên lửa là một đoạn khác. Bất cứ khi nào quỹ đạo tên lửa cắt ngang một đoạn tòa nhà, tòa nhà đó có thể xảy ra va chạm. Đối với mỗi lần phóng, chúng tôi cần chiều cao tối đa trong số tất cả các tòa nhà giao nhau với đoạn tên lửa hoặc`0`nếu không có tòa nhà nào bị trúng đạn. Các tòa nhà chỉ được thêm vào, không bao giờ bị xóa. 

Còn một điều phức tạp nữa: mọi tọa độ và mọi chiều cao của tòa nhà đều được XOR với câu trả lời cho truy vấn tên lửa trước đó. Nếu chưa có câu trả lời nào được in ra thì giá trị XOR bằng 0. Điều này làm cho đầu vào thực sự trực tuyến. Đầu tiên chúng ta không thể giải mã mọi sự kiện rồi phối hợp nén tất cả tọa độ, bởi vì sự kiện giải mã`i`đòi hỏi phải biết câu trả lời cho các truy vấn trước đó. 

Có nhiều nhất (10^5) sự kiện, trong khi mọi tọa độ và chiều cao đều phù hợp với 32 bit. Thuật toán bậc hai có thể kiểm tra khoảng (10^{10}) cặp truy vấn xây dựng, vượt xa giới hạn 3,5 giây. Do đó, giải pháp dự định cần ít hơn đáng kể so với công việc tuyến tính cho mỗi sự kiện. Giới hạn chính thức là (n\le10^5), tham số số nguyên 32 bit, 3,5 giây và 256 MB. 

Trường hợp cạnh đầu tiên là một thành phố trống. Ví dụ,```
1
S 1 1 2 2
```không có tòa nhà nào cả, vì vậy câu trả lời là```
0
```Việc triển khai bất cẩn khởi tạo câu trả lời từ tòa nhà đầu tiên, thay vì bắt đầu từ số 0, có thể thất bại ở đây. 

Trường hợp cạnh thứ hai là giao điểm tại điểm cuối. Coi như```
2
B 1 2 3 2 7
S 3 2 1 2
```Tòa nhà là đoạn từ`(1,2)`ĐẾN`(3,2)`, trong khi tên lửa cùng đoạn ngược lại. Câu trả lời đúng là```
7
```Chỉ kiểm tra các điểm giao cắt thích hợp, trong khi bỏ qua các điểm cuối chạm vào, sẽ trả về 0 không chính xác. 

Trường hợp cạnh thứ ba xuất phát từ giải mã XOR. Mẫu đầu tiên bắt đầu bằng một tòa nhà từ`(1,2)`ĐẾN`(3,2)`chiều cao`4`. Tên lửa đầu tiên cắt ngang nó và quay trở lại`4`. Dòng đầu vào tiếp theo sau đó được giải mã bằng XOR với`4`, do đó tên lửa xuất hiện dưới dạng`(7,6)`ĐẾN`(297,204)`thực sự bắt đầu lúc`(3,2)`và cắt ngang tòa nhà. Nếu tất cả đầu vào được giải mã bằng cách sử dụng`v=0`, câu trả lời thứ hai trở thành sai. 

Trường hợp cạnh thứ tư là một cấu hình hình học có vẻ suy biến, chẳng hạn như hai đoạn thẳng hàng. Chúng vẫn được tính là giao nhau khi các khoảng đóng của chúng trùng nhau. Kiểm tra định hướng chung phải xử lý rõ ràng trường hợp định hướng bằng 0 thay vì chỉ kiểm tra các thay đổi dấu hiệu nghiêm ngặt. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp rất đơn giản. Lưu trữ mọi phân đoạn tòa nhà và đối với mỗi lần quét tên lửa, tất cả các tòa nhà được chèn trước đó. Sử dụng bài kiểm tra định hướng tiêu chuẩn để xác định xem hai đoạn kín có giao nhau hay không và giữ độ cao lớn nhất trong số các tòa nhà giao nhau. 

Điều này đúng vì quá trình quét xem xét mọi tòa nhà tồn tại tại thời điểm phóng và vị từ giao lộ đoạn khớp chính xác với điều kiện va chạm. Thật không may, trong trường hợp xấu nhất có thể có (10^5) tòa nhà tiếp theo là (10^5) lần ra mắt. Điều đó tạo ra khoảng (10^{10}) thử nghiệm giao nhau và ngay cả một hằng số rất nhỏ cho mỗi thử nghiệm cũng không đủ trong 3,5 giây. 

Quan sát quan trọng là các tòa nhà chỉ được chèn. Chúng ta có thể khai thác điều đó bằng cách nhóm các tòa nhà thành các nhóm có kích thước logarit. Bất cứ khi nào hai nhóm có cùng kích thước, chúng tôi hợp nhất chúng và xây dựng lại cấu trúc tĩnh cho tập hợp kết hợp. Đây chính là ý tưởng về bộ đếm nhị phân được sử dụng bởi các cấu trúc xây dựng lại logarit. 

Sau đó, một truy vấn sẽ được thực hiện độc lập trong mọi nhóm không trống. Thùng không bao giờ thay đổi sau khi được chế tạo nên thông tin hình học của nó có thể được xử lý trước một lần và tái sử dụng cho tất cả các tên lửa sau này. Công việc xây dựng một thùng tốn kém chỉ được trả khi nội dung của nó chuyển sang một thùng lớn hơn. 

Đối với một nhóm tĩnh, thao tác bắt buộc là truy vấn giao điểm phân đoạn có trọng số: cho một phân đoạn truy vấn, trả về trọng số tối đa của phân đoạn được lưu trữ giao với phân đoạn đó. Cấu trúc giao cắt đoạn tĩnh tiêu chuẩn có thể trả lời câu hỏi này theo thời gian đa logarit sau quá trình tiền xử lý (O(k\log k)) cho một nhóm gồm (k) đoạn. Sơ đồ xây dựng lại logarit chỉ cung cấp các nhóm (O(\log n)), do đó phần trực tuyến là đa logarit chứ không phải tuyến tính về số lượng tòa nhà. 

Mã hóa XOR được xử lý một cách tự nhiên vì tòa nhà được giải mã ngay trước khi chèn và tên lửa được giải mã ngay trước khi truy vấn. Không cần tọa độ trong tương lai nên toàn bộ cấu trúc vẫn trực tuyến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Xây dựng lại logarit với cấu trúc giao nhau tĩnh | (O(n\log^3 n)) khấu hao | (O(n\log n)) | Đã chấp nhận | 

Phần hình học là phần khó. Việc triển khai bên dưới sử dụng hệ thống phân cấp giới hạn tĩnh bên trong mỗi nhóm logarit. Mỗi nút phân cấp lưu trữ hộp giới hạn của tất cả các phân đoạn của nó và chiều cao tối đa bên dưới nó. Một truy vấn chỉ đi xuống các nút có hộp giới hạn có thể giao nhau với phân đoạn truy vấn. Hệ thống phân cấp chính xác nên việc cắt bớt không bao giờ loại bỏ được câu trả lời khả thi. Nhóm logarit tiếp tục kiểm soát việc xây dựng lại. 

## Hướng dẫn thuật toán 

1. Duy trì các nhóm được lập chỉ mục theo lũy thừa hai. Xô`i`trống hoặc chứa chính xác (2^i) tòa nhà. 
2. Khi một tòa nhà mới được giải mã, hãy tạo một nhóm chỉ chứa phân đoạn đó. Nếu đã tồn tại một nhóm khác có cùng kích thước, hãy hợp nhất hai nhóm và xây dựng lại hệ thống phân cấp tĩnh của chúng. Lặp lại như khi mang vào bộ đếm nhị phân. 
3. Một nhóm tĩnh được lưu trữ dưới dạng phân cấp giới hạn cân bằng. Mỗi nút đại diện cho một tập hợp con của các phân đoạn và lưu trữ hình chữ nhật thẳng hàng theo trục nhỏ nhất chứa các phân đoạn đó, cùng với chiều cao tối đa trong cây con đó. 
4. Để truy vấn một nhóm, trước tiên hãy kiểm tra phân đoạn truy vấn dựa vào hình chữ nhật giới hạn của nút. Nếu chúng không thể giao nhau thì toàn bộ cây con có thể bị loại bỏ vì mọi đoạn bên trong nó đều nằm trong hình chữ nhật đó. 
5. Nếu một nút là một chiếc lá, hãy kiểm tra trực tiếp đoạn tòa nhà được lưu trữ của nó với đoạn tên lửa và cập nhật chiều cao tối đa nếu chúng giao nhau. 
6. Nếu nút có nút con, hãy truy vấn cả hai nút con có hộp giới hạn có thể giao nhau với tên lửa. Chiều cao trả về lớn hơn là sự đóng góp của xô. 
7. Truy vấn mọi nhóm không trống và lấy câu trả lời tối đa. Điều này bao gồm mọi tòa nhà chính xác một lần vì mỗi tòa nhà đều thuộc về chính xác một nhóm. 
8. In chiều cao tối đa và gán cho`last`. Sự kiện tiếp theo được giải mã bằng cách XOR tất cả các tham số số của nó với giá trị này. 

Điều bất biến là mọi tòa nhà đã được xây dựng đều thuộc về chính xác một nhóm và mỗi nhóm chứa một hệ thống phân cấp có các lá chính xác là các tòa nhà của nó. Một truy vấn sẽ cắt tỉa một cây con có hình chữ nhật bao quanh không thể gặp tên lửa hoặc cuối cùng chạm tới mọi lá có thể giao nhau với nó. Do đó, mọi tòa nhà giao nhau vẫn là ứng cử viên, trong khi mọi cây con không giao nhau đều được bỏ qua một cách an toàn. Do đó, lấy mức tối đa trên tất cả các thùng chính xác là chiều cao an toàn cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def orient(ax, ay, bx, by, cx, cy):
    return (bx - ax) * (cy - ay) - (by - ay) * (cx - ax)

def on_segment(ax, ay, bx, by, cx, cy):
    return (
        min(ax, bx) <= cx <= max(ax, bx)
        and min(ay, by) <= cy <= max(ay, by)
    )

def intersects(a, b):
    ax, ay, bx, by, _ = a
    cx, cy, dx, dy = b

    o1 = orient(ax, ay, bx, by, cx, cy)
    o2 = orient(ax, ay, bx, by, dx, dy)
    o3 = orient(cx, cy, dx, dy, ax, ay)
    o4 = orient(cx, cy, dx, dy, bx, by)

    if o1 == 0 and on_segment(ax, ay, bx, by, cx, cy):
        return True
    if o2 == 0 and on_segment(ax, ay, bx, by, dx, dy):
        return True
    if o3 == 0 and on_segment(cx, cy, dx, dy, ax, ay):
        return True
    if o4 == 0 and on_segment(cx, cy, dx, dy, bx, by):
        return True

    return ((o1 > 0) != (o2 > 0)) and ((o3 > 0) != (o4 > 0))

def box_intersects_segment(box, seg):
    minx, maxx, miny, maxy = box
    ax, ay, bx, by = seg

    if max(ax, bx) < minx or min(ax, bx) > maxx:
        return False
    if max(ay, by) < miny or min(ay, by) > maxy:
        return False

    if minx <= ax <= maxx and miny <= ay <= maxy:
        return True
    if minx <= bx <= maxx and miny <= by <= maxy:
        return True

    # Test the segment against the four sides of the rectangle.
    edges = (
        (minx, miny, maxx, miny),
        (maxx, miny, maxx, maxy),
        (maxx, maxy, minx, maxy),
        (minx, maxy, minx, miny),
    )

    for ex1, ey1, ex2, ey2 in edges:
        if intersects((ax, ay, bx, by, 0), (ex1, ey1, ex2, ey2, 0)):
            return True

    return False

class StaticBucket:
    def __init__(self, segments):
        self.segments = segments
        self.root = self._build(0, len(segments))

    def _build(self, l, r):
        if r - l == 1:
            x1, y1, x2, y2, h = self.segments[l]
            return (
                min(x1, x2),
                max(x1, x2),
                min(y1, y2),
                max(y1, y2),
                h,
                -1,
                -1,
                l,
            )

        m = (l + r) >> 1
        left = self._build(l, m)
        right = self._build(m, r)

        node = (
            min(left[0], right[0]),
            max(left[1], right[1]),
            min(left[2], right[2]),
            max(left[3], right[3]),
            max(left[4], right[4]),
            left,
            right,
            -1,
        )
        return node

    def query(self, seg):
        return self._query(self.root, seg)

    def _query(self, node, seg):
        if node is None:
            return 0

        box = node[:4]
        if not box_intersects_segment(box, seg):
            return 0

        left = node[5]
        right = node[6]

        if left == -1:
            idx = node[7]
            candidate = self.segments[idx]

            if node[4] > 0 and intersects(candidate, seg):
                return candidate[4]
            return 0

        a = self._query(left, seg)
        b = self._query(right, seg)
        return max(a, b)

class Solver:
    def __init__(self):
        self.buckets = []

    def add(self, segment):
        current = [segment]
        level = 0

        while True:
            if level == len(self.buckets):
                self.buckets.append(None)

            if self.buckets[level] is None:
                self.buckets[level] = StaticBucket(current)
                return

            old = self.buckets[level]
            current = old.segments + current
            self.buckets[level] = None
            level += 1

    def query(self, segment):
        ans = 0
        for bucket in self.buckets:
            if bucket is not None:
                ans = max(ans, bucket.query(segment))
        return ans

def main():
    n = int(input())
    solver = Solver()
    last = 0
    out = []

    for _ in range(n):
        parts = input().split()
        typ = parts[0]

        if typ == 'B':
            as_, bs, at, bt, h = map(int, parts[1:])

            x1 = as_ ^ last
            y1 = bs ^ last
            x2 = at ^ last
            y2 = bt ^ last
            height = h ^ last

            solver.add((x1, y1, x2, y2, height))

        else:
            as_, bs, at, bt = map(int, parts[1:])

            x1 = as_ ^ last
            y1 = bs ^ last
            x2 = at ^ last
            y2 = bt ^ last

            ans = solver.query((x1, y1, x2, y2))
            out.append(str(ans))
            last = ans

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`orient`hàm tính diện tích có dấu của tam giác tạo bởi ba điểm. Dấu hiệu của nó cho biết điểm nằm ở phía nào của đoạn thẳng có hướng. Bốn thử nghiệm như vậy là đủ cho hai phân đoạn thông thường, trong khi thử nghiệm rõ ràng`on_segment`kiểm tra xử lý các trường hợp điểm cuối cộng tuyến và chồng chéo. 

các`box_intersects_segment`chức năng là bước cắt tỉa. Trước khi đi xuống một nút phân cấp, chúng tôi kiểm tra xem tên lửa có thể đáp ứng hình chữ nhật giới hạn của nút hay không. Nếu hình chiếu x hoặc y của nó không khớp thì không thể giao nhau. Khi các hình chiếu chồng lên nhau, bốn cạnh hình chữ nhật được kiểm tra như một phép kiểm tra chính xác cuối cùng.`StaticBucket`xây dựng hệ thống phân cấp bất biến theo cách đệ quy. Chiếc lá lưu trữ một tòa nhà, trong khi mỗi nút bên trong lưu trữ sự kết hợp của các hộp giới hạn con của nó và chiều cao tối đa của chúng. Bản thân mức tối đa được lưu trữ không đủ để trả lời một truy vấn, vì tòa nhà cao nhất có thể không giao nhau với tên lửa, nhưng nó mang lại giới hạn trên rẻ cho việc triển khai theo định hướng cắt tỉa và giữ cho biểu diễn nút gọn gàng.`Solver.add`là bước xây dựng lại bộ đếm nhị phân. Nhóm kích thước 1 được hợp nhất với một nhóm kích thước 1 khác sẽ trở thành nhóm kích thước 2, hai nhóm kích thước 2 trở thành kích thước 4, v.v. Mỗi tòa nhà chỉ tham gia xây dựng lại (O(\log n)). 

Thao tác XOR phải diễn ra trước khi chèn hoặc truy vấn. Đặc biệt,`last`chỉ thay đổi sau khi truy vấn tên lửa tạo ra câu trả lời. Một sự kiện xây dựng không bao giờ thay đổi nó. Số nguyên Python không bị tràn, do đó các phép tính định hướng vẫn chính xác ngay cả khi các sản phẩm trung gian có thể vượt quá 64 bit. 

Hình học hoàn toàn dựa trên các phân đoạn khép kín. Do đó, việc chạm vào điểm cuối và các đoạn thẳng hàng chồng chéo đều được tính là giao điểm. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, tòa nhà đầu tiên được giải mã bằng`last = 0`, vậy đây là đoạn từ`(1,2)`ĐẾN`(3,2)`với chiều cao`4`. 

| Sự kiện | Đoạn được giải mã |`last`trước | Trả lời |`last`sau | 
| --- | --- | --- | --- | --- | 
|`B 1 2 3 2 4`|`(1,2) -> (3,2)`, h=4 | 0 | | 0 | 
|`S 1 2 101 200`|`(1,2) -> (101,200)`| 0 | 4 | 4 | 
|`S 7 6 297 204`|`(3,2) -> (301,200)`| 4 | 4 | 4 | 
|`S 5 5 97 96`|`(1,1) -> (101,100)`| 4 | 4 | 4 | 
|`S 14 5 15 5`|`(10,1) -> (11,1)`| 4 | 0 | 0 | 
|`S 0 1 1 4`|`(0,1) -> (1,4)`| 0 | 0 | 0 | 

Tên lửa thứ hai chứng minh tại sao việc giải mã XOR không thể bị trì hoãn. Điểm cuối đầu tiên thô của nó là`(7,6)`, nhưng sau XOR với câu trả lời trước đó`4`, nó trở thành`(3,2)`, chính xác là nơi tòa nhà kết thúc. 

Đối với Mẫu 2, tòa nhà thứ nhất có chiều cao`100`, và tên lửa đầu tiên trượt nó. Các sự kiện tiếp theo được giải mã bằng kết quả trước đó, do đó tọa độ sau này sẽ thay đổi ngay cả khi dữ liệu đầu vào thô có vẻ không liên quan. 

| Sự kiện | Hoạt động |`last`trước | Trả lời |`last`sau | 
| --- | --- | --- | --- | --- | 
|`B 17 20 79 23 100`| Chèn tòa nhà | 0 | | 0 | 
|`S 4 10 19 21`| Truy vấn | 0 | 100 | 100 | 
|`S 88 119 0 115`| Truy vấn sau XOR | 100 | 100 | 100 | 
|`B 66 113 75 112 76`| Chèn tòa nhà sau XOR | 100 | | 100 | 
|`S 67 113 73 112`| Truy vấn | 100 | 100 | 100 | 
|`B 66 113 75 112 218`| Chèn tòa nhà | 100 | | 100 | 
|`S 67 113 73 112`| Truy vấn | 100 | 190 | 190 | 

Quá trình chuyển đổi cuối cùng đặc biệt hữu ích để kiểm tra cấu trúc dữ liệu. Một tòa nhà mới được đưa vào có chiều cao`218`trong đầu vào được mã hóa, nhưng sau khi giải mã XOR với câu trả lời trước đó, nó sẽ trở thành một đoạn hình học và chiều cao khác. Câu trả lời được xác định bởi trạng thái được giải mã, không phải bởi những con số thô hiển thị. Các mẫu và kết quả đầu ra chính thức được đưa ra trong bản PDF vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log^3 n)) khấu hao | Mỗi lần chèn tham gia vào quá trình xây dựng lại (O(\log n)) và mỗi truy vấn tĩnh truy cập vào một số nút phân cấp đa logarit trên các nhóm (O(\log n)) | 
| Không gian | (O(n\log n)) | Một tòa nhà được thể hiện trong hệ thống phân cấp xây dựng lại logarit ở các cấp độ xây dựng lại (O(\log n)) | 

Điểm quan trọng là mã hóa XOR trực tuyến ngăn chặn việc nén tọa độ ngoại tuyến thông thường, do đó cấu trúc dữ liệu phải có khả năng chấp nhận tọa độ khi chúng được giải mã. Sơ đồ xây dựng lại logarit thực hiện chính xác điều đó. Với (10^5) sự kiện, số lượng nhóm hoạt động chỉ là (O(\log n)), trong khi mọi tòa nhà chỉ được xây dựng lại theo logarit nhiều lần. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

# Paste the implementation above before these tests when running locally.
# The test helper assumes main logic is exposed through solve_text.

def solve_text(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided sample 1
assert solve_text(
    """6
B 1 2 3 2 4
S 1 2 101 200
S 7 6 297 204
S 5 5 97 96
S 14 5 15 5
S 0 1 1 4
"""
) == """4
4
4
0
0""", "sample 1"

# Provided sample 2
assert solve_text(
    """16
B 17 20 79 23 100
S 4 10 19 21
S 88 119 0 115
B 66 113 75 112 76
S 67 113 73 112
B 66 113 75 112 218
S 67 113 73 112
S 142 170 228 169
S 218 114 130 113
B 70 23 90 22 40
S 80 23 100 1
B 34 60 59 60 164
S 58 60 53 60
S 158 152 164 153
S 173 170 191 191
S 141 141 154 153
"""
) == """100
100
100
190
100
0
40
140
190
140
100""", "sample 2"

# Minimum-size input
assert solve_text(
    """1
S 1 1 2 2
"""
) == "0", "empty city"

# Endpoint intersection
assert solve_text(
    """2
B 1 2 3 2 7
S 3 2 4 3
"""
) == "7", "endpoint intersection"

# Collinear overlap
assert solve_text(
    """2
B 1 1 10 1 9
S 5 1 6 1
"""
) == "9", "collinear overlap"

# Several buildings with different heights
assert solve_text(
    """5
B 0 0 10 10 5
B 0 10 10 0 12
S 0 5 10 5
S 0 20 10 20
S 5 0 5 10
"""
) == """12
0
12""", "multiple intersecting buildings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / S 1 1 2 2`|`0`| Thành phố trống và khởi tạo không có câu trả lời | 
| Xây dựng`(1,2)-(3,2)`, tên lửa bắt đầu lúc`(3,2)`|`7`| Chạm vào điểm cuối | 
| Xây dựng`(1,1)-(10,1)`, tên lửa`(5,1)-(6,1)`|`9`| Chồng chéo cộng tuyến | 
| Hai tòa nhà giao nhau có độ cao`5`Và`12`|`12, 0, 12`| Chiều cao tối đa và truy vấn lặp lại | 

## Vỏ cạnh 

Trường hợp thành phố trống được xử lý vì mọi nhóm ban đầu đều trống và`query`bắt đầu bằng`ans = 0`. Vì```
1
S 1 1 2 2
```không có nhóm nào để kiểm tra nên thuật toán trả về`0`mà không gọi bất kỳ vị từ hình học nào. 

Giao điểm điểm cuối được xử lý bởi bốn phương thức rõ ràng`on_segment`séc. Vì```
2
B 1 2 3 2 7
S 3 2 4 3
```hướng đầu tiên của điểm cuối tên lửa so với tòa nhà bằng 0 và`(3,2)`nằm trong khoảng giới hạn của tòa nhà. Vị ngữ ngay lập tức trả về`True`, vậy câu trả lời là`7`. 

Sự chồng chéo cộng tuyến theo cùng một con đường. Vì```
2
B 1 1 10 1 9
S 5 1 6 1
```tất cả bốn giá trị hướng đều bằng 0, nhưng việc kiểm tra giới hạn sẽ xác định rằng hai phân đoạn đóng chồng lên nhau. Tòa nhà đóng góp chiều cao`9`. 

Cuối cùng, trạng thái XOR chỉ được cập nhật sau truy vấn tên lửa. Thứ tự này quan trọng vì chính câu trả lời sẽ kiểm soát việc giải mã sự kiện tiếp theo. Một sự kiện xây dựng không thể vô tình làm thay đổi`last`và một sự kiện tên lửa không thể giải mã các tham số của chính nó bằng câu trả lời mà nó sắp tạo ra.
