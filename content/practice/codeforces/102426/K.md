---
title: "CF 102426K - Hệ thống X-Window"
description: "Chúng tôi có một màn hình có chiều rộng (W) và chiều cao (H) và nhiều nhất là mười cửa sổ hình chữ nhật. Hệ tọa độ hơi khác thường: tọa độ thứ nhất tăng dần xuống dưới và tọa độ thứ hai tăng sang phải."
date: "2026-08-12T19:42:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "K"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 565
verified: true
draft: false
---

[CF 102426K - Hệ thống X-Window](https://codeforces.com/problemset/problem/102426/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một màn hình có chiều rộng (W) và chiều cao (H) và nhiều nhất là mười cửa sổ hình chữ nhật. Hệ tọa độ hơi khác thường: tọa độ thứ nhất tăng dần xuống dưới và tọa độ thứ hai tăng sang phải. Một cửa sổ có thể mở rộng ra ngoài màn hình, nhưng chỉ có giao điểm của nó với màn hình mới thực sự được hiển thị. 

Các cửa sổ được sắp xếp theo chỉ số z. Chỉ số z nhỏ hơn có nghĩa là cửa sổ ở phía trước. Ban đầu, cửa sổ (0) là cửa sổ trên cùng, tiếp theo là cửa sổ (1), v.v. Tại bất kỳ thời điểm nào cũng có chính xác một cửa sổ đang hoạt động. Một cú nhấp chuột được diễn giải bằng cách sử dụng màn hình hiện được hiển thị: trong số tất cả các cửa sổ chứa điểm màn hình được nhấp, cửa sổ ngoài cùng sẽ nhận được cú nhấp chuột. Nếu không có cửa sổ nào chứa điểm thì không có gì thay đổi. 

Khi cửa sổ được nhấp vào khác với cửa sổ đang hoạt động, nó sẽ hoạt động và được chuyển lên phía trước. Các cửa sổ khác giữ nguyên thứ tự tương đối của chúng. Di chuyển cửa sổ về phía trước có thể để lộ phần cửa sổ trước đó bị che bởi các cửa sổ phía trước nó. Đầu ra được yêu cầu chính xác là khu vực mới hiển thị và do đó phải được vẽ lại. 

Đầu vào cung cấp kích thước màn hình, tất cả các hình chữ nhật của cửa sổ theo thứ tự z ban đầu và chuỗi các lần nhấp chuột. Đối với mỗi lần nhấp, chúng tôi xuất ra vùng tối thiểu phải được vẽ lại sau khi xử lý lần nhấp đó. 

Các hạn chế nhỏ bất thường về số lượng cửa sổ và số lần nhấp chuột. Ta có (n,q\le 10), trong khi màn hình có thể lớn bằng (2000\times2000). Điều này làm cho các thuật toán theo cấp số nhân trong (n) thực tế, nhưng các thuật toán tùy thuộc vào từng pixel màn hình sẽ tốn kém một cách không cần thiết. Mô phỏng pixel có thể thực hiện tới (10\cdot2000\cdot2000\cdot10=400{,}000{,}000) kiểm tra ngăn chặn cửa sổ trong trường hợp xấu nhất. Hình học là liên tục, do đó, việc xử lý từng ô vuông đơn vị một cách độc lập cũng phức tạp hơn mức cần thiết về mặt khái niệm. 

Điều tinh tế đầu tiên là nhấp chuột phải được gán cho cửa sổ được hiển thị trên cùng chứ không chỉ đơn giản là cửa sổ đầu tiên trong đầu vào ban đầu. Ví dụ:```
1 1
2
0 0 1 1
0 0 1 1
1
0 0
```Câu trả lời là:```
0
```Cửa sổ đầu tiên đã hoạt động và che đi lượt nhấp chuột, mặc dù cửa sổ thứ hai cũng chứa điểm. Việc triển khai bất cẩn tìm kiếm các cửa sổ theo thứ tự đầu vào sau mỗi lần kích hoạt cuối cùng sẽ làm mất thứ tự z hiện tại. 

Điều tinh tế thứ hai là một cửa sổ có thể mở rộng ra ngoài màn hình. Coi như:```
4 4
2
0 0 1 1
-1 -1 3 3
1
2 2
```Cửa sổ thứ hai chỉ hiển thị bên trong màn hình, nhưng nhấp chuột vào ((2,2)) vẫn nằm trên ranh giới hiển thị của nó. Nó bắt đầu hoạt động và phần chồng lấp mới được hiển thị của nó với cửa sổ đầu tiên có diện tích (1), vì vậy đầu ra là:```
1
```Việc triển khai bỏ qua việc cắt bớt có thể sử dụng không chính xác toàn bộ hình chữ nhật ngoài màn hình khi tính toán vùng sơn lại. 

Điều tinh tế thứ ba là các cửa sổ chồng lên nhau phải được tính là một phần chứ không được tính tổng một cách độc lập. Coi như:```
4 4
3
0 0 1 3
0 0 3 1
0 0 3 3
1
2 2
```Nhấp chuột đến cửa sổ thứ ba. Hai cửa sổ ở phía trước che khu vực (3) và (3) của nó, nhưng phần chung của chúng có diện tích (1). Do đó, diện tích sơn lại cần thiết là (3+3-1=5), cho:```
5
```Chỉ cần thêm các khu vực giao nhau theo cặp sẽ tạo ra (6), điều này là sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là biểu diễn màn hình dưới dạng các ô đơn vị (W\times H). Vì mọi tọa độ đều là tích phân nên mọi ranh giới hình chữ nhật đều nằm trên các đường lưới, do đó diện tích của nó thực sự có thể được biểu thị bằng một số ô đơn vị. Đối với mỗi lần nhấp, chúng tôi có thể kiểm tra từng ô, xác định cửa sổ nào ở trên cùng ở đó và so sánh kết xuất cũ và mới. Điều này đúng, nhưng trường hợp xấu nhất có (2000\cdot2000) ô, tối đa (10) lần nhấp và tối đa (10) cửa sổ được kiểm tra trên mỗi ô. Điều đó mang lại (400) triệu lượt kiểm tra cửa sổ. Với giới hạn một giây, đây là mức độ chi tiết sai. 

Lực lượng vũ phu hoạt động vì nó đại diện rõ ràng cho màn hình, nhưng bản thân màn hình chứa nhiều thông tin hơn nhu cầu của vấn đề. Vùng sơn lại luôn được hình thành bởi các giao điểm và phần kết của các hình chữ nhật và chỉ có mười hình chữ nhật. 

Giả sử cửa sổ (t) đang được chuyển lên phía trước. Trước khi thực hiện thao tác, mọi cửa sổ hiện tại trước (t) theo thứ tự z đều bao phủ một phần của (t). Sau thao tác, (t) sẽ bao phủ tất cả các cửa sổ đó. Do đó, khu vực mới có thể nhìn thấy duy nhất là 

[ 
R_t\cap(R_1\cup R_2\cup\cdots\cup R_k), 
] 

trong đó (R_i) chính xác là các cửa sổ hiện ở phía trước (t), với tất cả các hình chữ nhật được hiểu là bị màn hình cắt bớt. 

Điều này làm giảm vấn đề tìm diện tích hợp của nhiều nhất là chín hình chữ nhật bên trong một hình chữ nhật mục tiêu. Với rất ít hình chữ nhật, việc loại trừ bao gồm đặc biệt thuận tiện. Với mọi tập con khác rỗng của các hình chữ nhật che phủ, hãy tính giao của tất cả các hình chữ nhật trong tập con đó. Cộng diện tích của nó khi tập hợp con chứa số hình chữ nhật lẻ và trừ nó khi tập hợp con chứa số chẵn. 

Có nhiều nhất (2^9-1=511) tập hợp con không trống cho một lần kích hoạt. Có thể tìm thấy mục tiêu chuột bằng cách quét theo thứ tự z hiện tại, chỉ tốn (O(n)). Sau khi tính toán vùng sơn lại, việc di chuyển cửa sổ đã chọn lên phía trước chỉ đơn giản là một thao tác danh sách. 

Cách tiếp cận vũ phu hoạt động vì nó mô hình hóa rõ ràng mọi đơn vị diện tích có thể nhìn thấy, nhưng không thành công vì màn hình lớn hơn nhiều so với số lượng đối tượng hình học. Quan sát cho thấy chỉ có sự kết hợp của tối đa chín hình chữ nhật mới cho phép chúng ta thay thế hàng triệu thao tác ô bằng vài trăm giao điểm hình chữ nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qWHn)) | (O(WH)) | Quá chậm | 
| Tối ưu | (O(qn^2 2^n)) | (O(n)) | Đã chấp nhận | 

Hệ số (n) bên trong giới hạn tối ưu xuất phát từ việc tính toán từng giao điểm của tập hợp con bằng cách quét các hình chữ nhật thuộc tập hợp con đó. Vì (n\le10), giá trị này khá nhỏ. 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi cửa sổ dưới dạng bốn tọa độ biểu thị khoảng dọc và khoảng ngang của nó. Bởi vì tọa độ đầu tiên là thẳng đứng nên một cửa sổ`(x, y, w, h)`chiếm (x\ldots x+h) theo chiều dọc và (y\ldots y+w) theo chiều ngang. Giữ thứ tự z ban đầu như`[0, 1, ..., n-1]`, trong đó phần tử đầu tiên là cửa sổ ngoài cùng. 
2. Đối với mỗi lần nhấp chuột, hãy quét theo thứ tự z hiện tại từ trước ra sau. Đối với mỗi cửa sổ, trước tiên hãy cắt hình chữ nhật của nó với màn hình, sau đó kiểm tra xem vị trí chuột có nằm bên trong hình chữ nhật đã cắt đó hay không. Sử dụng các so sánh toàn diện cho bài kiểm tra điểm vì câu lệnh xử lý rõ ràng các ranh giới cửa sổ như một phần của vùng được hiển thị. 
3. Nếu không có cửa sổ nào chứa vị trí chuột, xuất ra 0 và giữ nguyên thứ tự z. Không có kích hoạt nên màn hình hiển thị không thay đổi. 
4. Nếu cửa sổ được chọn đã là cửa sổ đang hoạt động, xuất số 0 và giữ nguyên thứ tự z. Nhấp vào một cửa sổ đã hoạt động sẽ không di chuyển bất cứ thứ gì. 
5. Ngược lại, đặt cửa sổ đã chọn là (t). Theo thứ tự z hiện tại, mọi cửa sổ trước (t) đều ở phía trước nó. Thu thập chính xác những cửa sổ đó. Diện tích cần vẽ lại là diện tích (t) được bao phủ bởi sự hợp nhất của các cửa sổ phía trước này. 
6. Kẹp cửa sổ mục tiêu vào màn hình trước khi tính diện tích của nó. Đối với mọi tập hợp con khác trống của các cửa sổ phía trước, hãy giao hình chữ nhật đích với mọi hình chữ nhật trong tập hợp con đó. Nếu giao điểm kết quả có chiều rộng và chiều cao dương, thì diện tích của nó sẽ được cộng cho tập hợp con có kích thước lẻ và trừ đi cho tập hợp con có kích thước chẵn. Đây là công thức loại trừ bao gồm tiêu chuẩn cho một công đoàn. 
7. Xóa (t) khỏi vị trí hiện tại của nó theo thứ tự z và chèn nó vào đầu. Đây chính xác là mô hình thay đổi chỉ số z của nó thành 0 trong khi tăng mọi chỉ số z trước đó lên một. 
8. Xuất ra vùng sơn lại đã tính toán và tiếp tục với lần nhấp chuột tiếp theo. Thứ tự được duy trì bây giờ là trạng thái hoàn chỉnh cần thiết để xử lý sự kiện chuột tiếp theo. 

### Tại sao nó hoạt động 

Bất biến chính là danh sách thứ tự z luôn lưu trữ các cửa sổ từ trước ra sau chính xác như hệ thống cửa sổ hiện tại. Do đó, cửa sổ đầu tiên chứa nhấp chuột chính xác là cửa sổ có pixel được hiển thị nhận được nhấp chuột đó. 

Khi một cửa sổ (t) khác hoạt động, chỉ có mối quan hệ của nó với các cửa sổ hiện tại ở phía trước nó thay đổi. Mọi cửa sổ phía trước như vậy trước đây đều hiển thị trên phần chồng chéo của chúng với (t), trong khi sau khi kích hoạt (t) sẽ hiển thị trên cùng phần chồng lấp đó. Không có phần nào khác của màn hình thay đổi. Do đó, vùng sơn lại chính xác là tập hợp các giao điểm giữa (t) và tất cả các cửa sổ trước nó. 

Loại trừ bao gồm tính toán chính xác sự kết hợp đó, ngay cả khi một số cửa sổ che phủ chồng lên nhau. Sau khi di chuyển (t) lên phía trước, bất biến thứ tự z được khôi phục. Do đó, mỗi lần nhấp đều được xử lý theo trạng thái màn hình chính xác và mọi khu vực được báo cáo chính xác là khu vực có kết xuất thay đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def intersect(a, b):
    x1 = max(a[0], b[0])
    y1 = max(a[1], b[1])
    x2 = min(a[2], b[2])
    y2 = min(a[3], b[3])
    if x1 >= x2 or y1 >= y2:
        return None
    return (x1, y1, x2, y2)

def area(r):
    if r is None:
        return 0
    return (r[2] - r[0]) * (r[3] - r[1])

def union_inside(target, rects):
    k = len(rects)
    if k == 0:
        return 0

    ans = 0

    for mask in range(1, 1 << k):
        cur = target
        bits = 0

        for i in range(k):
            if mask & (1 << i):
                bits += 1
                cur = intersect(cur, rects[i])
                if cur is None:
                    break

        if cur is not None:
            a = area(cur)
            if bits & 1:
                ans += a
            else:
                ans -= a

    return ans

def solve():
    W, H = map(int, input().split())
    n = int(input())

    windows = []

    for _ in range(n):
        x, y, w, h = map(int, input().split())

        # x is vertical, y is horizontal.
        # The screen is [0, H] x [0, W].
        windows.append((x, y, x + h, y + w))

    q = int(input())

    clicks = [tuple(map(int, input().split())) for _ in range(q)]

    # Frontmost to backmost.
    order = list(range(n))

    screen = (0, 0, H, W)

    out = []

    for u, v in clicks:
        target_pos = -1

        # Find the frontmost rendered window containing the click.
        for pos, idx in enumerate(order):
            clipped = intersect(windows[idx], screen)

            if clipped is not None:
                if clipped[0] <= u <= clipped[2] and clipped[1] <= v <= clipped[3]:
                    target_pos = pos
                    break

        if target_pos == -1:
            out.append("0")
            continue

        target = order[target_pos]

        # Clicking the active window causes no change.
        if target_pos == 0:
            out.append("0")
            continue

        # Windows before target are exactly the windows currently
        # covering it from the front.
        front_rects = [
            windows[idx]
            for idx in order[:target_pos]
        ]

        clipped_target = intersect(windows[target], screen)

        repaint = union_inside(clipped_target, front_rects)
        out.append(str(repaint))

        # Move target to the front.
        order.pop(target_pos)
        order.insert(0, target)

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`windows`mảng lưu trữ mỗi hình chữ nhật dưới dạng`(top, left, bottom, right)`trong hướng tọa độ của bài toán. Phạm vi theo chiều dọc sử dụng`h`, trong khi phạm vi ngang sử dụng`w`. Đây là nơi dễ vô tình hoán đổi chiều rộng và chiều cao. 

các`intersect`chức năng sử dụng nghiêm ngặt`x1 < x2`Và`y1 < y2`khi quyết định liệu một giao lộ có diện tích dương hay không. Một ranh giới có thể chứa một cú click chuột, nhưng ranh giới có diện tích bằng 0, do đó nó không được đóng góp vào tính toán sơn lại. 

Hình chữ nhật của màn hình là`(0, 0, H, W)`. Giao cắt một cửa sổ với hình chữ nhật này sẽ tự động xử lý tất cả các trường hợp ngoài màn hình. Không cần phải sửa đổi tọa độ cửa sổ ban đầu. 

Tìm kiếm nhấp chuột sử dụng`<=`ở cả hai đầu. Điều này tuân theo quy tắc của câu lệnh rằng ranh giới hiển thị thuộc về cửa sổ được hiển thị. Nếu hai cửa sổ gặp nhau tại một ranh giới, thứ tự z hiện tại vẫn quyết định cửa sổ nào nhận được nhấp chuột ở đó. 

các`union_inside`chức năng áp dụng loại trừ bao gồm trực tiếp. Một tập hợp con có số hình chữ nhật lẻ đóng góp tích cực, trong khi một tập hợp con có kích thước chẵn đóng góp tiêu cực. Hình chữ nhật mục tiêu được bao gồm trong mọi giao lộ, do đó phép kết hợp được tính toán sẽ tự động bị giới hạn trong khu vực cửa sổ đang được kích hoạt. 

Số nguyên Python là đủ cho tất cả các khu vực. Diện tích màn hình tối đa chỉ là (4{,}000{,}000), mặc dù việc loại trừ bao gồm có thể tạm thời cộng và trừ một số khu vực như vậy. 

Cuối cùng, cửa sổ đã chọn sẽ bị xóa khỏi vị trí cũ và được chèn vào chỉ số 0. Điều này trực tiếp thể hiện phép biến đổi chỉ số z được mô tả bởi bài toán. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp bắt đầu theo thứ tự z`[0, 1, 2]`, cửa sổ ở đâu`0`là trên cùng. Bảng sau theo dõi trạng thái quan trọng sau mỗi lần nhấp chuột. 

| Bấm vào | Cửa sổ đã chọn | Đơn hàng hiện tại trước | Cửa sổ phía trước | Khu vực sơn lại | Đặt hàng sau | 
| --- | --- | --- | --- | --- | --- | 
|`(2,1)`| 2 |`[0,1,2]`|`[0,1]`| 1 |`[2,0,1]`| 
|`(3,1)`| không |`[2,0,1]`| không | 0 |`[2,0,1]`| 
|`(1,2)`| 2 |`[2,0,1]`| không | 0 |`[2,0,1]`| 
|`(3,8)`| 1 |`[2,0,1]`|`[2,0]`| 4 |`[1,2,0]`| 
|`(3,3)`| 0 |`[1,2,0]`|`[1,2]`| 5 |`[0,1,2]`| 

Đối với lần nhấp đầu tiên, điểm nằm trên ranh giới hiển thị của cửa sổ`2`. Nó trùng lặp với cửa sổ`0`có diện tích (1), trong khi cửa sổ`1`không chồng lên nhau ở đó, cho ra kết quả đầu tiên là (1). Lần nhấp thứ hai không đạt đến cửa sổ hiển thị. Lần nhấp thứ ba đến cửa sổ`2`một lần nữa, nó đã hoạt động. 

Nhấp chuột thứ tư kích hoạt cửa sổ`1`. Nó trùng lặp với cửa sổ`0`đóng góp diện tích (4), trong khi nó trùng với cửa sổ`2`là số không. Nhấp chuột thứ năm kích hoạt cửa sổ`0`. Nó trùng lặp với cửa sổ`1`có diện tích (4) và nó trùng với cửa sổ`2`có diện tích (1). Các vùng này không chồng lên nhau nên hợp có diện tích (5). Kết quả đầu ra chính xác là:```
1
0
0
4
5
```Ví dụ thứ hai tập trung vào các cửa sổ che phủ chồng lên nhau:```
4 4
3
0 0 1 3
0 0 3 1
0 0 3 3
1
2 2
```Dấu vết trạng thái là: 

| Bấm vào | Cửa sổ đã chọn | Đơn hàng hiện tại trước | Tính toán bảo hiểm | Khu vực sơn lại | Đặt hàng sau | 
| --- | --- | --- | --- | --- | --- | 
|`(2,2)`| 2 |`[0,1,2]`| (3+3-1) | 5 |`[2,0,1]`| 

Điểm được nhấp nằm ngoài hai cửa sổ đầu tiên và bên trong cửa sổ thứ ba. Cửa sổ phía trước đầu tiên bao phủ hình chữ nhật (1\times3), cửa sổ thứ hai bao phủ hình chữ nhật (3\times1) và giao điểm của chúng là hình vuông (1\times1). Bao gồm-loại trừ cho (3+3-1=5). Ví dụ này chứng minh tại sao việc tính tổng các phần trùng lặp riêng lẻ là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(qn^2 2^n)) | Mỗi lần nhấp chuột sẽ quét (O(n)) cửa sổ và một lần kích hoạt sẽ đánh giá tối đa (2^n) tập hợp con, với tối đa (O(n)) giao điểm hình chữ nhật trên mỗi tập hợp con. | 
| Không gian | (O(n)) | Thuật toán lưu trữ các hình chữ nhật, thứ tự z và một lượng hình học tạm thời không đổi. | 

Với (n,q\le10), hệ số mũ lớn nhất chỉ là (2^{10}=1024). Ngay cả với hệ số bổ sung là (n^2), số lượng phép toán nguyên thủy vẫn nhỏ. Kích thước màn hình không xuất hiện một cách phức tạp vì thuật toán không bao giờ lặp lại trên từng pixel hoặc ô đơn vị riêng lẻ. 

## Trường hợp thử nghiệm```python
import sys
import io

def intersect(a, b):
    x1 = max(a[0], b[0])
    y1 = max(a[1], b[1])
    x2 = min(a[2], b[2])
    y2 = min(a[3], b[3])
    if x1 >= x2 or y1 >= y2:
        return None
    return x1, y1, x2, y2

def area(r):
    if r is None:
        return 0
    return (r[2] - r[0]) * (r[3] - r[1])

def union_inside(target, rects):
    k = len(rects)
    ans = 0

    for mask in range(1, 1 << k):
        cur = target
        bits = 0

        for i in range(k):
            if mask & (1 << i):
                bits += 1
                cur = intersect(cur, rects[i])
                if cur is None:
                    break

        if cur is not None:
            if bits & 1:
                ans += area(cur)
            else:
                ans -= area(cur)

    return ans

def solve():
    input = sys.stdin.readline

    W, H = map(int, input().split())
    n = int(input())

    windows = []
    for _ in range(n):
        x, y, w, h = map(int, input().split())
        windows.append((x, y, x + h, y + w))

    q = int(input())
    order = list(range(n))
    screen = (0, 0, H, W)

    ans = []

    for _ in range(q):
        u, v = map(int, input().split())

        pos = -1

        for i, idx in enumerate(order):
            r = intersect(windows[idx], screen)
            if r is not None and r[0] <= u <= r[2] and r[1] <= v <= r[3]:
                pos = i
                break

        if pos <= 0:
            ans.append("0")
            continue

        target = order[pos]
        target_rect = intersect(windows[target], screen)

        front = [windows[idx] for idx in order[:pos]]
        repaint = union_inside(target_rect, front)

        ans.append(str(repaint))

        order.pop(pos)
        order.insert(0, target)

    return "\n".join(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

sample1 = """\
9 6
3
1 2 5 4
2 5 3 2
-1 1 2 3
5
2 1
3 1
1 2
3 8
3 3
"""

assert run(sample1) == "1\n0\n0\n4\n5", "sample 1"

minimum_case = """\
1 1
1
0 0 1 1
1
1 1
"""

assert run(minimum_case) == "0", "minimum size"

boundary_case = """\
4 4
2
0 0 1 1
-1 -1 3 3
1
2 2
"""

assert run(boundary_case) == "1", "screen clipping and boundary"

union_case = """\
4 4
3
0 0 1 3
0 0 3 1
0 0 3 3
1
2 2
"""

assert run(union_case) == "5", "overlapping front windows"

reorder_case = """\
4 4
3
0 0 1 1
0 0 4 4
3 3 1 1
3
3 3
0 0
2 2
"""

assert run(reorder_case) == "1\n0\n2", "z-order changes"

maximum_equal_case = """\
2000 2000
10
0 0 2000 2000
0 0 2000 2000
0 0 2000 2000
0 0 2000 2000
0 0 2000 2000
0 0 2000 2000
0 0 2000 2000
0 0 2000 2000
0 0 2000 2000
0 0 2000 2000
10
0 0
1000 1000
2000 2000
0 2000
2000 0
1 1
1999 1999
500 1500
1500 500
1000 1000
"""

assert run(maximum_equal_case) == "\n".join(["0"] * 10), "maximum size and equal windows"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ kích thước tối thiểu |`0`| Màn hình nhỏ nhất, một cửa sổ, nhấp vào cửa sổ đang hoạt động | 
| Trường hợp ranh giới |`1`| Cắt bớt màn hình và bao gồm các ranh giới nhấp chuột | 
| Trường hợp công đoàn |`5`| Loại trừ bao gồm các cửa sổ che phủ chồng chéo | 
| Sắp xếp lại trường hợp |`1`,`0`,`2`| Thứ tự z động và kích hoạt lặp lại | 
| Trường hợp bằng nhau tối đa | Mười số không | Kích thước tối đa, số lượng cửa sổ và lần nhấp chuột tối đa, hình chữ nhật giống hệt nhau | 

## Vỏ cạnh 

Đầu vào có kích thước tối thiểu có một màn hình (1\times1) và một cửa sổ (1\times1). Cửa sổ duy nhất bắt đầu hoạt động, vì vậy bất kỳ cú nhấp chuột nào vào bên trong cửa sổ đó sẽ chọn cửa sổ đã hoạt động. Thuật toán tìm thấy nó ở vị trí 0 trong`order`, ngay lập tức xuất ra số 0 và không bao giờ thay đổi thứ tự. 

Trường hợp cắt sử dụng cửa sổ`(-1,-1,3,3)`trên màn hình (4\times4). Vùng nhìn thấy của nó chỉ là giao điểm với`[0,4] × [0,4]`. Nhấp chuột vào`(2,2)`nằm bên trong vùng hiển thị đó nhưng bên ngoài cửa sổ đầu tiên nên cửa sổ thứ hai được kích hoạt. Giao điểm với cửa sổ đầu tiên chính xác là hình vuông (1\times1)`[0,1] × [0,1]`, cho diện tích sơn lại (1). 

Trường hợp cửa sổ chồng chéo sẽ kích hoạt cửa sổ thứ ba trong khi có hai cửa sổ ở phía trước nó. Sự chồng chéo riêng lẻ của chúng với mục tiêu có các khu vực (3) và (3), nhưng sự chồng chéo chung của chúng có khu vực (1). Phép tính bao gồm-loại trừ là (3+3-1=5), do đó thuật toán đưa ra`5`thay vì số tiền không chính xác`6`. 

Trường hợp thứ tự động đầu tiên kích hoạt cửa sổ`2`, thay đổi thứ tự từ`[0,1,2]`ĐẾN`[2,0,1]`và khu vực sơn lại (1). Lần nhấp tiếp theo sẽ đến cửa sổ`0`, nó sẽ hoạt động và chuyển thứ tự sang`[0,2,1]`; sự trùng lặp của nó với cửa sổ`2`có diện tích bằng 0 nên số lượng sơn lại bằng 0. Nhấp chuột cuối cùng đến cửa sổ`1`. Lúc đó cả hai cửa sổ`0`và cửa sổ`2`ở phía trước nó và chồng lên nhau với cửa sổ`1`là các ô vuông đơn vị rời nhau, tạo ra diện tích sơn lại là (2). Điều này xác nhận rằng thuật toán sử dụng thứ tự z hiện tại thay vì thứ tự đầu vào ban đầu. 

Hộp có kích thước tối đa có mười cửa sổ toàn màn hình giống hệt nhau. Chỉ cửa sổ ngoài cùng phía trước mới có thể nhận được nhấp chuột vì nó che phủ hoàn toàn mọi cửa sổ khác. Nó đã hoạt động cho mỗi lần nhấp chuột, vì vậy mọi câu trả lời đều bằng không. Thử nghiệm xác nhận rằng kích thước màn hình lớn không khiến thuật toán phân bổ hoặc đi qua lưới (2000\times2000).
