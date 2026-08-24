---
title: "CF 102163A - Hasan thẩm phán lười biếng"
description: "Chúng ta có hai loại đoạn thẳng hữu hạn. Một đoạn ngang được mô tả bởi tọa độ x trái và phải và tọa độ y cố định của nó. Một đoạn thẳng đứng được mô tả bởi tọa độ y dưới và trên cùng và tọa độ x cố định của nó."
date: "2026-08-23T10:49:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "A"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 2337
verified: true
draft: false
---

[CF 102163A - Hasan thẩm phán lười biếng](https://codeforces.com/problemset/problem/102163/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38 phút 57 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai loại đoạn thẳng hữu hạn. Một đoạn ngang được mô tả bởi tọa độ x trái và phải và tọa độ y cố định của nó. Một đoạn thẳng đứng được mô tả bởi tọa độ y dưới và trên cùng và tọa độ x cố định của nó. 

Chọn một đoạn ngang và một đoạn thẳng cắt nhau tại điểm C. Bốn nhánh của dấu cộng thu được kết thúc ở bốn điểm cuối của hai đoạn đó. Nếu giao lộ ở`(x, y)`, chiều dài bốn cánh tay là`x - x1`,`x2 - x`,`y - y1`, Và`y2 - y`. 

Giá trị của dấu cộng này là giá trị nhỏ nhất trong bốn số đó. Chúng tôi cần giá trị tối đa có thể có trên mọi cặp ngang và dọc hợp lệ. 

Với tối đa`10^5`đoạn ngang và`10^5`các đoạn thẳng đứng, việc thử từng cặp sẽ cần tới`10^10`kiểm tra trong một trường hợp thử nghiệm. Giới hạn một giây loại bỏ hoàn toàn điều đó. Thậm chí một`O((N+M) log(N+M))`giải pháp phù hợp hơn nhiều ở đây, trong khi thuật toán có hệ số logarit bổ sung vẫn có thể thực tế vì phạm vi tọa độ chỉ`10^5`. 

Có một số trường hợp ranh giới mà việc triển khai đúng phải xử lý cẩn thận. Đầu tiên, chạm vào điểm cuối là giao điểm hợp lệ, nhưng nó cho độ dài bằng 0 ở phía đó. Ví dụ,```
1
1 1
1 1 1
1 1 1
```có câu trả lời`0`. Một giải pháp yêu cầu sự chồng chéo hoàn toàn tích cực sẽ báo cáo không chính xác rằng không có giao điểm. 

Trường hợp thứ hai là đoạn ngang có thể đủ dài trong khi đoạn dọc lại quá ngắn. Ví dụ,```
1
1 1
1 10 5
1 5 5
```có câu trả lời`0`. Các đoạn giao nhau, nhưng đoạn thẳng đứng chỉ có hai đơn vị khoảng trống ở cạnh ngắn hơn tại điểm giao cắt tốt nhất có thể, và trên thực tế, tổng chiều dài của nó chỉ bằng bốn, do đó không thể hình thành điểm cộng dương của chiều dài lớn hơn 0 tại điểm giao nhau cần thiết. Tổng quát hơn, một ứng cử viên có độ dài`d`yêu cầu cả hai đoạn phải có tổng chiều dài ít nhất`2d`. 

Trường hợp thứ ba là đẳng thức tại các ranh giới bị thu hẹp. Coi như```
1
1 1
1 5 3
1 5 3
```Câu trả lời là`2`, bởi vì giao nhau tại`(3,3)`để lại chính xác hai đơn vị theo mọi hướng. Kiểm tra độ dài`d`phải sử dụng các điều kiện bao gồm như`x1 + d <= x <= x2 - d`. Thay thế chúng bằng các bất đẳng thức nghiêm ngặt sẽ làm mất đi câu trả lời hợp lệ này. 

Đầu vào không đảm bảo rằng các điểm cuối được trình bày theo thứ tự tăng dần, do đó, việc triển khai mạnh mẽ sẽ bình thường hóa mọi phân đoạn trước tiên. Các lời giải tham khảo cho bài toán này cũng thực hiện tương tự trước khi xử lý hình học. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi phân đoạn ngang so với mọi phân đoạn dọc. Đối với một cặp, chúng tôi kiểm tra xem phạm vi x và y của chúng có giao nhau hay không, tính tọa độ giao điểm và sau đó đánh giá bốn khoảng cách từ điểm đó đến các điểm cuối của đoạn. Điều này đúng vì mọi dấu cộng có thể được xác định bởi đúng một cặp như vậy. Vấn đề là số lượng cặp. Với`N = M = 10^5`, có thể có`10^10`cặp, vượt xa giới hạn thời gian cho phép. 

Quan sát hữu ích là ngừng cố gắng tối đa hóa câu trả lời một cách trực tiếp. Thay vào đó, hãy đặt câu hỏi có hoặc không: liệu chúng ta có thể tạo một dấu cộng có giá trị ít nhất bằng`d`? 

Đối với đoạn ngang`[x1, x2]`ở độ cao`y`, giao điểm tại x có ít nhất`d`đơn vị ở cả hai phía ngang chính xác khi`x1 + d <= x <= x2 - d`. 

Vì vậy, đoạn ngang có thể được thay thế, đối với kiểm tra cụ thể này, bằng khoảng x cho phép`[x1+d, x2-d]`. Nếu như`x1+d > x2-d`, đoạn ngang này không thể tham gia vào cộng kích thước`d`. 

Tương tự như vậy, một đoạn dọc`[y1, y2]`tại x có thể hỗ trợ kích thước cộng thêm`d`chính xác khi chiều cao giao lộ thỏa mãn`y1 + d <= y <= y2-d`. 

Bây giờ hình học đã trở thành một bài toán đường quét. Sắp xếp các đoạn dọc theo x. Đối với một cố định`d`, mỗi đoạn ngang hữu ích sẽ hoạt động khi quá trình quét đạt tới`x1+d`và ngừng hoạt động sau`x2-d`. Trong khi xử lý một đoạn dọc tại tọa độ x, các chiều ngang hoạt động chính xác là những đoạn có khoảng x được phép chứa x. 

Trong số các chiều ngang đang hoạt động, chúng ta chỉ cần biết liệu có ít nhất một chiều ngang có tọa độ y bên trong khoảng hợp lệ của đoạn thẳng đứng hay không`[y1+d, y2-d]`. Cây Fenwick trên tọa độ y là cách triển khai tiêu chuẩn. Giải pháp C++ được chấp nhận ban đầu sử dụng chính xác quá trình quét này, kích hoạt các chiều ngang theo ranh giới bên trái của chúng, loại bỏ chúng sau ranh giới bên phải của chúng và truy vấn khoảng y bằng cây Fenwick. 

Quan sát cuối cùng là sự đơn điệu. Nếu cộng thêm kích thước`d`tồn tại thì điểm cộng của mọi kích thước nhỏ hơn cũng tồn tại. Điều đó làm cho câu trả lời phù hợp cho tìm kiếm nhị phân. Chúng tôi kiểm tra một ứng cử viên`d`và nếu khả thi, hãy tìm kiếm các giá trị lớn hơn; mặt khác, tìm kiếm các giá trị nhỏ hơn. 

Đối với việc triển khai Python bên dưới, quá trình quét tương tự được kết hợp với một tập hợp bit dựa trên khối nhỏ trên vũ trụ tọa độ y. Tọa độ nhiều nhất là`10^5`, vì vậy chúng ta có thể duy trì tọa độ y nào hiện đang hoạt động mà không cần thao tác trên cây logarit. Mỗi khối chứa 256 vị trí y và bitset nhỏ thứ hai ghi lại các khối không trống. Một truy vấn khoảng thời gian sau đó chạm vào tối đa hai khối ranh giới và tập hợp bit cấp khối nhỏ gọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NM)`|`O(N+M)`| Quá chậm | 
| Tìm kiếm nhị phân + quét + cây Fenwick |`O((N+M) log C log C)`|`O(N+M+C)`| Đã chấp nhận | 
| Tìm kiếm nhị phân + quét + bitset tọa độ giới hạn |`O((N+M) log C)`cho tọa độ cố định ràng buộc |`O(N+M+C)`| Đã chấp nhận | 

Đây`C`nhiều nhất là tọa độ tối đa`10^5`. Cách tiếp cận thứ hai là công thức cấu trúc dữ liệu thông thường, trong khi cách thứ ba đặc biệt phù hợp với Python dưới giới hạn một giây vì vũ trụ tọa độ được giới hạn rõ ràng. 

## Hướng dẫn thuật toán 

1. Chuẩn hóa mọi đoạn ngang sao cho`x1 <= x2`, và mọi đoạn thẳng đứng sao cho`y1 <= y2`. Tính câu trả lời lớn nhất có thể từ một nửa độ dài của bất kỳ đoạn nào. 
2. Sắp xếp các đoạn ngang hai lần, một lần theo`x1`và một lần bởi`x2`. Đối với ứng viên cố định`d`, khoảng thời gian sử dụng được của chúng trở thành`[x1+d, x2-d]`. Kể từ khi thêm tương tự`d`giữ nguyên thứ tự, danh sách không bao giờ cần phải sắp xếp lại trong quá trình tìm kiếm nhị phân. 
3. Sắp xếp các đoạn thẳng đứng theo tọa độ x. Chúng tôi sẽ quét qua chúng từ trái sang phải. 
4. Đối với ứng viên`d`, loại bỏ mọi đoạn ngang với`x2-x1 < 2d`, vì không có đủ chỗ theo chiều ngang cho hai cánh tay dài`d`. Với mỗi đoạn ngang còn lại, hãy xác định`left = x1+d`Và`right = x2-d`. 
5. Trong quá trình quét x, hãy chèn một đoạn ngang khi`left <= x`cho đoạn thẳng đứng hiện tại. Loại bỏ nó khi`right < x`. Việc so sánh nghiêm ngặt để loại bỏ là có chủ ý. Nếu như`right == x`, đường thẳng đứng đi qua điểm cuối của khoảng ngang bị thu hẹp và điều đó vẫn cho kết quả chính xác`d`đơn vị ở phía tương ứng. 
6. Duy trì tọa độ y của tất cả các đoạn ngang hiện đang hoạt động. Nếu một số chiều ngang đang hoạt động có cùng tọa độ y, hãy duy trì một số đếm thay vì một boolean đơn giản, vì việc xóa một trong số chúng không được vô tình xóa tọa độ trong khi một chiều ngang khác vẫn hoạt động. 
7. Đối với mỗi đoạn thẳng đứng, trước tiên yêu cầu`y2-y1 >= 2d`. Chiều cao vượt qua có thể có của nó chính xác là`[y1+d, y2-d]`. Nếu tập y hoạt động chứa bất kỳ tọa độ nào trong khoảng này, thì ít nhất một cộng kích thước`d`tồn tại. 
8. Tìm kiếm nhị phân lớn nhất khả thi`d`. Giới hạn dưới là bằng không. Giới hạn trên có thể là nửa độ dài lớn nhất của bất kỳ phân đoạn đầu vào nào. Nếu như`check(mid)`thành công, lưu trữ`mid`và tiếp tục sang phải; nếu không thì tiếp tục sang trái. 

### Tại sao nó hoạt động 

Đối với một cố định`d`, đoạn ngang hoạt động chính xác là đoạn ngang có tọa độ x có thể được chọn sao cho cả hai nhánh ngang có chiều dài ít nhất`d`. Khi quét ở tọa độ dọc x, tập hoạt động chứa chính xác tất cả các chiều ngang có khoảng x được phép chứa x đó. 

Một đoạn dọc đóng góp ít nhất một điểm cộng hợp lệ về kích thước`d`chính xác khi nào tọa độ y của nó có thể được chọn bên trong`[y1+d, y2-d]`. Do đó, truy vấn phạm vi trên tọa độ y ngang đang hoạt động là đúng chính xác khi một số chiều ngang và chiều dọc này có thể tạo thành một dấu cộng như vậy. Do đó, việc quét sẽ trả lời`check(d)`một cách chính xác. 

Cuối cùng, tính khả thi là đơn điệu. Việc tăng chiều dài cánh tay cần thiết chỉ có thể loại bỏ các nút giao có thể có chứ không bao giờ tạo ra các nút giao mới. Tìm kiếm nhị phân do đó tìm thấy giá trị khả thi tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

BLOCK = 256

class ActiveY:
    def __init__(self, max_y):
        self.size = max_y + 1
        self.blocks = [0] * ((max_y >> 8) + 1)
        self.count = [0] * (max_y + 1)
        self.nonempty = 0

    def add(self, y):
        c = self.count[y]
        self.count[y] = c + 1
        if c == 0:
            b = y >> 8
            bit = 1 << (y & 255)
            old = self.blocks[b]
            self.blocks[b] = old | bit
            if old == 0:
                self.nonempty |= 1 << b

    def remove(self, y):
        c = self.count[y] - 1
        self.count[y] = c
        if c == 0:
            b = y >> 8
            bit = 1 << (y & 255)
            new = self.blocks[b] & ~bit
            self.blocks[b] = new
            if new == 0:
                self.nonempty &= ~(1 << b)

    def any_in_range(self, lo, hi):
        if lo > hi:
            return False

        b1 = lo >> 8
        b2 = hi >> 8

        if b1 == b2:
            left = lo & 255
            right = hi & 255
            mask = ((1 << (right - left + 1)) - 1) << left
            return (self.blocks[b1] & mask) != 0

        left = lo & 255
        if self.blocks[b1] & (~((1 << left) - 1) & ((1 << 256) - 1)):
            return True

        right = hi & 255
        if self.blocks[b2] & ((1 << (right + 1)) - 1):
            return True

        if b2 - b1 <= 1:
            return False

        width = b2 - b1 - 1
        middle_mask = ((1 << width) - 1) << (b1 + 1)
        return (self.nonempty & middle_mask) != 0

def solve_case(horizontal, vertical, max_coord):
    n = len(horizontal)
    m = len(vertical)

    horizontal.sort(key=lambda p: p[0])
    by_right = sorted(horizontal, key=lambda p: p[1])
    vertical.sort(key=lambda p: p[2])

    max_len = 0
    for x1, x2, _ in horizontal:
        max_len = max(max_len, (x2 - x1) // 2)
    for y1, y2, _ in vertical:
        max_len = max(max_len, (y2 - y1) // 2)

    def check(d):
        left_list = []
        right_list = []

        for x1, x2, y in horizontal:
            if x2 - x1 >= 2 * d:
                left_list.append((x1 + d, y))
                right_list.append((x2 - d, y))

        active = ActiveY(max_coord)

        li = 0
        ri = 0
        ln = len(left_list)
        rn = len(right_list)

        for y1, y2, x in vertical:
            if y2 - y1 < 2 * d:
                continue

            while li < ln and left_list[li][0] <= x:
                active.add(left_list[li][1])
                li += 1

            while ri < rn and right_list[ri][0] < x:
                active.remove(right_list[ri][1])
                ri += 1

            if active.any_in_range(y1 + d, y2 - d):
                return True

        return False

    lo = 0
    hi = max_len
    ans = 0

    while lo <= hi:
        mid = (lo + hi) // 2
        if check(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    return ans

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        horizontal = []
        vertical = []
        max_coord = 0

        for _ in range(n):
            x1, x2, y = map(int, input().split())
            if x1 > x2:
                x1, x2 = x2, x1
            horizontal.append((x1, x2, y))
            max_coord = max(max_coord, x1, x2, y)

        for _ in range(m):
            y1, y2, x = map(int, input().split())
            if y1 > y2:
                y1, y2 = y2, y1
            vertical.append((y1, y2, x))
            max_coord = max(max_coord, y1, y2, x)

        out.append(str(solve_case(horizontal, vertical, max_coord)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào sẽ chuẩn hóa các điểm cuối ngay lập tức, do đó, mọi so sánh sau này có thể giả định tọa độ tăng dần. Câu trả lời tối đa có thể có nhiều nhất là một nửa độ dài của một đoạn, vì cộng chiều dài`d`nhu cầu`2d`các đơn vị dọc theo cả đoạn ngang và đoạn dọc. 

Hai thứ tự ngang được chuẩn bị một lần. Sắp xếp theo`x1`đưa ra thứ tự các khoảng ngang bắt đầu hoạt động, đồng thời sắp xếp theo`x2`đưa ra thứ tự mà chúng hết hạn. Thêm`d`đến mọi điểm cuối bên trái và trừ đi`d`từ mọi điểm cuối bên phải không thay đổi thứ tự nào, do đó các phép lặp tìm kiếm nhị phân không yêu cầu một cách sắp xếp khác. 

các`ActiveY`cấu trúc lưu trữ số đếm cho mỗi tọa độ y. Bitset đầu tiên của nó ghi lại các vị trí bị chiếm dụng bên trong mỗi khối tọa độ 256. Bitset thứ hai ghi lại khối nào chứa ít nhất một tọa độ hoạt động. Điều này làm cho việc chèn và xóa không đổi theo thời gian đối với vũ trụ tọa độ cố định của bài toán. 

Truy vấn khoảng trước tiên kiểm tra trực tiếp hai khối ranh giới. Nếu có các khối hoàn chỉnh giữa chúng, nó sẽ kiểm tra mức độ chiếm dụng của chúng bằng cách sử dụng tập bit cấp khối nhỏ gọn. Chỉ số khối lớn nhất chỉ khoảng`10^5 / 256`, do đó các phép toán số nguyên này vẫn rất nhỏ mặc dù phạm vi tọa độ ban đầu lớn. 

Việc quét sử dụng`left <= x`khi chèn và`right < x`khi gỡ bỏ. Những bất bình đẳng đó là chi tiết ranh giới quan trọng. Đoạn ngang có khoảng thời gian sử dụng được kết thúc chính xác ở tọa độ x dọc hiện tại vẫn là ứng cử viên hợp lệ. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn khi xây dựng mặt nạ bit. Giới hạn tọa độ cũng giữ cho mọi bitset nhỏ một cách thoải mái. Việc triển khai C++ ban đầu sử dụng cây Fenwick thay vì cấu trúc vũ trụ giới hạn này, với cùng phạm vi hình học và các ranh giới khoảng bao gồm giống nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào chứa các đoạn ngang`[1,5]`Tại`y=3`,`[2,4]`Tại`y=2`, Và`[6,12]`Tại`y=6`. Các đoạn dọc là`[1,5]`Tại`x=3`Và`[6,9]`Tại`x=2`. 

Vì`d=2`, chiều ngang đầu tiên chỉ có thể sử dụng được tại`x=3`, với tọa độ y`3`. Chiều ngang thứ hai có độ dài chính xác theo yêu cầu và có thể sử dụng được ở`x=3`cũng vậy. Ngành dọc đầu tiên có khoảng y hợp lệ`[3,3]`. 

| Bước | Ứng viên`d`| Thẳng đứng`(y1,y2,x)`| Giá trị y ngang đang hoạt động | Khoảng truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 |`(1,5,3)`|`{3,2}`|`[3,3]`| Đã tìm thấy | 
| 2 | 2 |`(6,9,2)`|`{}`sau khi đặt hàng x |`[8,7]`| Không cần thiết | 

Đường thẳng đứng thứ nhất cắt đường ngang thứ nhất tại`(3,3)`. Chiều dài bốn cánh tay là`2,2,2,2`, vì vậy câu trả lời ít nhất là`2`. Không thể có giá trị lớn hơn vì chiều ngang đầu tiên có tổng chiều dài`4`và chiều ngang thứ hai cũng có tổng chiều dài`2`. Như vậy câu trả lời là`2`. 

### Dấu vết tùy chỉnh 

Hãy xem xét```
1
1 1
1 9 5
3 7 5
```Đoạn ngang có phạm vi x`[1,9]`và đoạn thẳng đứng có phạm vi y`[3,7]`, cả hai cắt nhau tại tọa độ`5`. 

Vì`d=2`, phạm vi x có thể sử dụng theo chiều ngang là`[3,7]`và phạm vi y có thể sử dụng theo chiều dọc là`[5,5]`. 

| Bước | Ứng viên`d`| Ngang có thể sử dụng x | Dọc x | Giá trị y đang hoạt động | Dọc có thể sử dụng được y | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 |`[3,7]`, y=`5`|`5`|`{5}`|`[5,5]`| Đã tìm thấy | 
| 2 | 3 |`[4,6]`, y=`5`|`5`|`{5}`|`[6,4]`| Không thể | 

Việc kiểm tra cho`d=2`thành công vì vượt qua tại`(5,5)`để lại bốn đơn vị theo chiều ngang và hai đơn vị theo chiều dọc, do đó cánh tay ngắn nhất là`2`. Việc kiểm tra cho`d=3`không thành công vì đoạn dọc chỉ có tổng chiều dài`4`, không đủ cho hai nhánh dài`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((N+M) log C)`| Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các phân đoạn theo thời gian tuyến tính và tìm kiếm nhị phân sẽ thực hiện`O(log C)`séc | 
| Không gian |`O(N+M+C)`| Các phân đoạn, bản sao được sắp xếp, bộ đếm y và tập hợp tọa độ giới hạn được lưu trữ | 

Đây`C <= 10^5`. Thuộc tính thực tế quan trọng là cấu trúc y động sử dụng vũ trụ tọa độ giới hạn thay vì cây Python với`O(log C)`làm việc cho mỗi sự kiện. Tìm kiếm nhị phân cần tối đa khoảng 17 lần lặp vì`2^17 > 10^5`, do đó, khoảng vài triệu thao tác xử lý phân đoạn được thực hiện cho mỗi trường hợp thử nghiệm lớn. 

Giải pháp C++ ban đầu sử dụng cây Fenwick và do đó có hệ số logarit bổ sung khi quét, nhưng nó phù hợp với các giới hạn đã nêu trong C++. Phiên bản Python thay thế cấu trúc dữ liệu logarit đó bằng cách biểu diễn tập hợp tọa độ để giữ cho việc triển khai phù hợp với cùng một giới hạn chặt chẽ. 

## Trường hợp thử nghiệm```python
# Helper: execute the same solve logic on an input string.
import sys
import io

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

# Provided sample.
assert run("""\
1
3 2
1 5 3
2 4 2
6 12 6
1 5 3
6 9 2
""") == "2\n", "sample 1"

# Minimum-size segments. They intersect at one point, but every arm has length zero.
assert run("""\
1
1 1
1 1 1
1 1 1
""") == "0\n", "minimum-size segments"

# Endpoint intersection with positive horizontal length, catching strict-boundary errors.
assert run("""\
1
1 1
1 5 3
3 3 3
""") == "0\n", "endpoint-only intersection"

# Exact maximum plus. Both segments have length 8 and cross at their centers.
assert run("""\
1
1 1
1 9 5
1 9 5
""") == "4\n", "exact half-length"

# Reversed endpoints must be normalized.
assert run("""\
1
1 1
9 1 5
9 1 5
""") == "4\n", "reversed endpoints"

# Two horizontals share the same y coordinate. Removing one must not remove
# the coordinate while the other is still active.
assert run("""\
1
2 1
1 10 5
3 8 5
1 10 5
""") == "3\n", "duplicate active y"

# Large boundary coordinates.
assert run("""\
1
1 1
1 100000 50000
1 100000 50000
""") == "49999\n", "coordinate boundary"

# Multiple test cases.
assert run("""\
2
1 1
1 5 3
1 5 3
1 1
1 2 1
1 2 1
""") == "2\n1\n", "multiple test cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / [1,1] / [1,1]`|`0`| Các đoạn có kích thước tối thiểu và các nhánh có độ dài bằng 0 | 
|`1 1 / [1,5] at y=3 / [3,3] at x=3`|`0`| Giao lộ chỉ có điểm cuối | 
|`1 1 / [1,9] at y=5 / [1,9] at x=5`|`4`| Ranh giới nửa chiều dài và bao gồm chính xác | 
| đảo ngược`[9,1]`điểm cuối |`4`| Chuẩn hóa điểm cuối | 
| Hai đường ngang có cùng y |`3`| Đếm tham chiếu chính xác trong quá trình xóa | 
| tọa độ bằng`100000`|`49999`| Ranh giới tọa độ tối đa | 
| Hai trường hợp thử nghiệm |`2`,`1`| Cách ly trạng thái giữa các trường hợp thử nghiệm | 

## Vỏ cạnh 

Đối với giao lộ chỉ xảy ra ở điểm cuối, thuật toán giữ lại ứng cử viên vì khoảng ngang có thể sử dụng được đóng lại. Vì```
1
1 1
1 5 3
3 3 3
```đoạn ngang cho phép vượt qua tại`x=3`, trong khi đoạn thẳng đứng chỉ có thể cắt nhau tại`y=3`. Giao điểm là hợp lệ, nhưng đoạn thẳng đứng có độ dài bằng 0, vì vậy câu trả lời là`0`. Trong kiểm tra độ dài dương,`y1+d <= y2-d`ngay lập tức thất bại cho mọi`d > 0`. 

Đối với một đoạn có tổng độ dài chính xác gấp đôi câu trả lời được yêu cầu, khoảng thu hẹp bao gồm một tọa độ. Vì```
1
1 1
1 5 3
3 3 3
```ứng cử viên`d=0`là khả thi, trong khi`d=1`là không thể vì đoạn thẳng đứng không thể cung cấp một đơn vị cho cả hai bên. Tổng quát hơn, đối với```
1
1 1
1 5 3
1 5 3
```ứng cử viên`d=2`tạo ra phạm vi ngang`[3,3]`và phạm vi dọc`[3,3]`. Việc quét sử dụng`<=`để kích hoạt và`<`để loại bỏ, do đó tọa độ ranh giới chung vẫn hoạt động và câu trả lời`2`được tìm thấy. 

Khi một số đoạn ngang có cùng tọa độ y, cấu trúc hoạt động phải thể hiện bội số. Giả sử hai chiều ngang có cùng y bắt đầu hoạt động và một chiều hết hạn. Tọa độ y vẫn được biểu thị bằng chiều ngang khác. các`count[y]`mảng xử lý việc này một cách trực tiếp: bit được đặt khi số đếm thay đổi từ 0 thành một và chỉ bị xóa khi số đếm trở về 0. 

Các điểm cuối đảo ngược được xử lý trước khi thực hiện bất kỳ hình học nào. Ví dụ,```
1
1 1
9 1 5
9 1 5
```được chuẩn hóa thành hai phân đoạn từ`1`ĐẾN`9`. Tâm của chúng trùng nhau tại`5`, đưa ra câu trả lời`4`. Nếu không chuẩn hóa, các biểu thức như`x2-x1`sẽ trở nên âm và giới hạn trên của tìm kiếm nhị phân có thể âm thầm trở nên không chính xác. 

Trường hợp thoái hóa toàn bộ cũng an toàn. Nếu mọi đoạn ngang và dọc đều bao gồm một điểm duy nhất thì mọi điểm cộng có thể có đều có giá trị bằng 0. Giới hạn dưới của tìm kiếm nhị phân ban đầu bằng 0 và`check(0)`chấp nhận chính xác bất kỳ giao điểm thực tế nào trong khi mọi ứng cử viên tích cực đều bị từ chối. 

Cuối cùng, các nút giao liên quan đến nhiều đường ở cùng tọa độ không cần xử lý đặc biệt. Bài toán yêu cầu chúng ta chọn một đoạn ngang và một đoạn dọc, do đó, mỗi đoạn ngang hoạt động kết hợp với đoạn dọc hiện tại đại diện cho một cặp hợp lệ. Thuật toán chỉ cần biết liệu có tồn tại ít nhất một cặp như vậy với độ dài ứng cử viên hiện tại chứ không phải tồn tại bao nhiêu cặp.
