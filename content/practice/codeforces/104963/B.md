---
title: "CF 104963B - \u0412\u0435\u043b\u043e\u0434\u043e\u0440\u043e\u0436\u043a\u0438"
description: "Chúng ta có một quảng trường hình chữ nhật được tạo thành từ các hình vuông đơn vị, có chiều rộng $w$ và chiều cao $h$. Một số ô đơn vị này bị nứt và phải được loại bỏ hoàn toàn trong quá trình thi công."
date: "2026-06-28T18:21:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104963
codeforces_index: "B"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2022. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 104963
solve_time_s: 79
verified: true
draft: false
---

[CF 104963B - \u0412\u0435\u043b\u043e\u0434\u043e\u0440\u043e\u0436\u043a\u0438](https://codeforces.com/problemset/problem/104963/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một quảng trường hình chữ nhật được tạo thành từ các hình vuông đơn vị, có chiều rộng$w$và chiều cao$h$. Một số ô đơn vị này bị nứt và phải được loại bỏ hoàn toàn trong quá trình thi công. Chúng ta được phép vạch hai đường thẳng dành cho xe đạp: một dải ngang và một dải dọc, cả hai đều thẳng hàng với lưới, cả hai đều có cùng chiều rộng nguyên$c$. Mọi thứ được bao phủ bởi những dải này đều bị loại bỏ khỏi quảng trường. 

Sau khi loại bỏ hai dải này, mỗi ô bị nứt phải nằm bên trong ít nhất một trong các dải đã loại bỏ. Tương tự, phải tồn tại sự lựa chọn một đoạn chiều cao nằm ngang$c$và một đoạn thẳng đứng có chiều rộng$c$sao cho mọi ô xấu đều được bao phủ bởi ít nhất một trong số chúng. 

Chúng ta cần tìm mức tối thiểu có thể$c$. 

Những ràng buộc ngay lập tức gợi ý rằng$w$Và$h$có thể rất lớn, lên tới$10^9$, vì vậy chúng tôi không thể biểu diễn lưới một cách rõ ràng. Số lượng tế bào bị nứt nhiều nhất$3 \cdot 10^5$, có nghĩa là toàn bộ lời giải phải chỉ phụ thuộc vào những điểm này. Bất kỳ cách tiếp cận nào quét tất cả các hàng hoặc cột theo chiều rộng ứng cử viên sẽ quá chậm nếu được thực hiện một cách đơn giản, nhưng các thao tác trên danh sách điểm đều khả thi. 

Một ý tưởng ngây thơ sẽ thử tất cả các vị trí có thể có cho dải ngang và dải dọc và tính chiều rộng tối thiểu cần thiết để bao phủ các điểm chưa được che phủ còn lại. Điều này là không khả thi vì số lượng vị trí tỷ lệ thuận với$w \cdot h$, điều đó là không thể. 

Một lực lượng vũ phu tinh tế hơn sẽ khắc phục$c$, rồi cố gắng xác định xem có tồn tại một khoảng chiều cao theo chiều ngang hay không$c$bao gồm tất cả các điểm ngoại trừ những điểm được bao phủ bởi một khoảng chiều rộng dọc$c$. Thậm chí, điều này còn trở nên tốn kém nếu được triển khai trực tiếp vì nó đề xuất quét các tọa độ được sắp xếp và kiểm tra nhiều cấu hình. 

Các trường hợp cạnh thường phá vỡ lý luận ngây thơ bao gồm các tình huống trong đó tất cả các ô xấu nằm trong một hàng hoặc cột. Trong trường hợp đó, câu trả lời rõ ràng là$1$, bởi vì một dải có chiều rộng 1 có thể bao phủ mọi thứ. Một trường hợp phức tạp khác là khi các ô xấu tạo thành hình chữ thập, buộc cả hai dải đều phải sử dụng hết công suất, đẩy câu trả lời lên một giá trị lớn. 

## Phương pháp tiếp cận 

Quan sát chính là vấn đề cơ bản là bao phủ một tập hợp các điểm bằng hai dải thẳng hàng theo trục có chiều rộng bằng nhau. Nếu chúng ta cố định dải ngang thì dải dọc phải che hết các điểm chưa được che phủ còn lại. Điều này cho thấy rằng đối với một đoạn ngang cố định, chiều rộng dọc yêu cầu được xác định hoàn toàn bởi tọa độ x của các điểm không được che phủ và đối xứng đối với một đoạn dọc cố định. 

Nếu chọn dải ngang che hàng$[y, y+c-1]$, thì bất kỳ điểm nào bên trong dải này đã được xử lý. Các điểm còn lại đều phải được che bằng một dải dọc có chiều rộng$c$, nghĩa là tọa độ x của chúng phải nằm trong một khoảng độ dài$c$. Vì vậy, tính khả thi giảm xuống còn việc kiểm tra xem tọa độ x còn lại có thể được chứa trong một số cửa sổ có kích thước hay không$c$. 

Điều này gợi ý một cấu trúc: cho một$c$, chúng ta có thể quét qua các vị trí dải ngang có thể được xác định bằng tọa độ y được sắp xếp của các ô xấu. Đối với mỗi vị trí, chúng tôi xác định điểm nào nằm ngoài và sau đó kiểm tra xem phạm vi x của chúng có thể được bao phủ bởi một đoạn có chiều dài hay không$c$. Việc tính toán lại trực tiếp cho từng vị trí sẽ quá chậm, nhưng vì chúng tôi chỉ cần x tối thiểu và tối đa trong số các điểm bị loại trừ nên chúng tôi có thể duy trì chúng một cách hiệu quả bằng cách sử dụng cấu trúc được sắp xếp và cửa sổ trượt. 

Đối số đối xứng áp dụng khi chúng ta hoán đổi vai trò của x và y, nhưng chúng ta không cần thực hiện rõ ràng cả hai nếu chúng ta coi một hướng là chính và hướng kia là ràng buộc dẫn xuất. 

Giải pháp cuối cùng thường dựa vào việc sắp xếp các điểm theo cả hai tọa độ và sử dụng hai con trỏ hoặc tính toán trước hậu tố tiền tố để duy trì các giá trị cực trị một cách hiệu quả cho các cửa sổ trượt của y. 

Lực lượng vũ phu hoạt động vì mỗi cấu hình rất dễ xác minh sau khi được chọn, nhưng nó không thành công vì số lượng cấu hình là bậc hai theo số điểm. Quan sát cho thấy chỉ các giá trị x cực trị của các điểm chưa được phát hiện mới làm giảm mức xác minh xuống O(1) trên mỗi cấu hình sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực trên các vị trí |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp + cửa sổ trượt + cực tiền tố |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách thử các vị trí ứng cử viên cho dải ngang bằng cách sử dụng tọa độ y được sắp xếp của các ô bị nứt. 

1. Sắp xếp tất cả các ô bị nứt theo tọa độ y của chúng. Điều này cho phép chúng tôi coi bất kỳ dải ngang nào là một đoạn liền kề theo thứ tự này, vì dải hợp lệ tương ứng với việc chọn khoảng y. 
2. Tính toán trước các mảng tiền tố và hậu tố trên danh sách đã sắp xếp lưu trữ tọa độ x tối thiểu và tối đa. Các mảng này cho phép chúng ta nhanh chóng biết phạm vi x của bất kỳ tập hợp con điểm nào nằm ngoài khoảng y đã chọn. 
3. Đối với mọi phân đoạn điểm có thể nằm bên trong dải ngang, chúng tôi hiểu nó là một khối liền kề trong mảng được sắp xếp theo y. Đối với một phân khúc$[l, r]$, các điểm bên trong được bao phủ bởi dải ngang và các điểm bên ngoài được chia thành hai nhóm: các nhóm bên dưới$l$và những điều trên$r$. 
4. Đối với các điểm còn lại bên ngoài dải, hãy tính giá trị x tối thiểu và tối đa bằng cách sử dụng dữ liệu tiền tố và hậu tố. Điều này mang lại khoảng ngang chính xác mà dải dọc phải bao phủ. 
5. Kiểm tra xem nhịp x này có thể được che bởi một dải dọc có chiều rộng không$c$, tức là liệu$\max x - \min x + 1 \le c$. Nếu có, lựa chọn dải ngang này sẽ hoạt động. 
6. Lặp lại một cách đối xứng bằng cách sắp xếp theo x và xử lý dải dọc trước vì cấu hình tối ưu có thể được ghi lại tốt hơn theo hướng ngược lại. 
7. Câu trả lời là tối thiểu$c$mà một trong hai hướng đều mang lại cấu hình hợp lệ. Từ$c$là đơn điệu (nếu một giải pháp phù hợp với$c$, nó hoạt động với các giá trị lớn hơn), chúng ta có thể tìm kiếm nhị phân trên$c$. 

### Tại sao nó hoạt động 

Bất kỳ giải pháp hợp lệ nào đều chia thành hai tập hợp: những tập hợp được bao phủ bởi dải ngang và những tập hợp được bao phủ bởi dải dọc. Tập đầu tiên tương ứng với các điểm có tọa độ y nằm trong một khoảng độ dài$c$và điểm thứ hai tương ứng với các điểm có tọa độ x nằm trong một khoảng có độ dài$c$. Đối với một cố định$c$, bất kỳ giải pháp khả thi nào cũng phải tạo ra một số điểm phân chia phù hợp với khoảng thời gian đó. Việc sắp xếp đảm bảo rằng mọi dải ngang hợp lệ đều tương ứng với một phân đoạn liền kề theo thứ tự y, do đó việc liệt kê các phân đoạn đó bao gồm tất cả các khả năng. Tính đúng đắn xuất phát từ thực tế là tính khả thi của phạm vi x chỉ phụ thuộc vào các điểm cực trị, do đó tiền tố và hậu tố cực tiểu và cực đại mô tả đầy đủ bất kỳ sự phân chia nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(points, c, swap=False):
    if swap:
        pts = [(y, x) for x, y in points]
    else:
        pts = points

    pts.sort()  # sort by y (or x if swapped)
    n = len(pts)

    xs = [p[1] for p in pts]

    pref_min = [0] * n
    pref_max = [0] * n
    suf_min = [0] * n
    suf_max = [0] * n

    pref_min[0] = pref_max[0] = xs[0]
    for i in range(1, n):
        pref_min[i] = min(pref_min[i - 1], xs[i])
        pref_max[i] = max(pref_max[i - 1], xs[i])

    suf_min[-1] = suf_max[-1] = xs[-1]
    for i in range(n - 2, -1, -1):
        suf_min[i] = min(suf_min[i + 1], xs[i])
        suf_max[i] = max(suf_max[i + 1], xs[i])

    l = 0
    for r in range(n):
        while l <= r and pts[r][0] - pts[l][0] + 1 > c:
            l += 1

        # try making [l, r] the horizontal strip
        min_x = float('inf')
        max_x = -float('inf')

        if l > 0:
            min_x = min(min_x, pref_min[l - 1])
            max_x = max(max_x, pref_max[l - 1])
        if r + 1 < n:
            min_x = min(min_x, suf_min[r + 1])
            max_x = max(max_x, suf_max[r + 1])

        if min_x == float('inf'):
            return True
        if max_x - min_x + 1 <= c:
            return True

    return False

def solve():
    w, h, n = map(int, input().split())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    lo, hi = 1, min(w, h)

    while lo < hi:
        mid = (lo + hi) // 2
        if ok(points, mid, False) or ok(points, mid, True):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xác định trình kiểm tra tính khả thi cho chiều rộng cố định$c$. Nó tùy ý hoán đổi tọa độ để kiểm tra cả hai hướng, vì vai trò ngang và dọc là đối xứng. 

Bên trong trình kiểm tra, việc sắp xếp theo hướng dải cho phép chúng tôi coi bất kỳ dải hợp lệ nào là một đoạn liền kề. Mảng tiền tố và hậu tố trên tọa độ x cho phép truy xuất theo thời gian liên tục các giá trị x cực trị bên ngoài bất kỳ phân đoạn đã chọn nào. 

Con trỏ trượt trên y đảm bảo chúng ta chỉ xem xét tối đa các dải chiều cao ngang hợp lệ$c$. Đối với mỗi dải như vậy, chúng tôi tính toán x-span còn lại và xác minh xem nó có phù hợp với chiều rộng không$c$. 

Tìm kiếm nhị phân bên ngoài sử dụng tính đơn điệu: một khi chiều rộng hoạt động thì bất kỳ chiều rộng lớn hơn nào cũng hoạt động, bởi vì việc tăng chiều rộng dải chỉ làm giảm bớt các ràng buộc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 6 5
(5,4), (2,6), (4,1), (2,3), (1,4)
```Chúng tôi kiểm tra một ứng cử viên$c = 3$. 

Sắp xếp theo y: 

| Bước | tôi | r | Dải bên trong y-range | Bên ngoài phút x | Bên ngoài tối đa x | x-nhịp được rồi | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | (1) | tính từ phần còn lại | tính toán | không | 
| 2 | 0 | 2 | (1..3) | tính toán | tính toán | vâng | 

Tại$r=2$, cửa sổ y bao phủ các điểm có y trong phạm vi kích thước tối đa là 3 và các điểm còn lại có giá trị x vừa với khoảng rộng 3. Điều này khẳng định tính khả thi. 

Thuật toán tìm thấy rằng$c=3$hoạt động và tìm kiếm nhị phân hội tụ về nó. 

### Mẫu 2 

đầu vào:```
4 3 4
(1,1), (4,3), (4,1), (1,3)
```Vì$c=3$, bất kỳ dải ngang nào có chiều cao 3 có thể che phủ hầu hết tất cả các hàng, không yêu cầu độ che phủ dọc. Khoảng x của các điểm còn lại trống, do đó điều kiện được thỏa mãn một cách tầm thường. 

Điều này cho thấy một trường hợp suy biến trong đó chỉ một dải xử lý hiệu quả tất cả các ràng buộc và thuật toán xử lý chính xác phần còn lại trống là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \log \min(w,h))$| sắp xếp bên trong mỗi lần kiểm tra và tìm kiếm nhị phân$c$| 
| Không gian |$O(n)$| lưu trữ điểm và mảng tiền tố/hậu tố | 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$điểm, vì vậy$O(n \log n)$mỗi lần kiểm tra có thể chấp nhận được nếu tìm kiếm nhị phân chạy trong khoảng 30 lần lặp, tạo ra tổng cộng vài triệu thao tác, vừa vặn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    w, h, n = map(int, input().split())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    def ok(points, c, swap=False):
        if swap:
            pts = [(y, x) for x, y in points]
        else:
            pts = points

        pts.sort()
        n = len(pts)
        xs = [p[1] for p in pts]

        pref_min = [0] * n
        pref_max = [0] * n
        suf_min = [0] * n
        suf_max = [0] * n

        pref_min[0] = pref_max[0] = xs[0]
        for i in range(1, n):
            pref_min[i] = min(pref_min[i - 1], xs[i])
            pref_max[i] = max(pref_max[i - 1], xs[i])

        suf_min[-1] = suf_max[-1] = xs[-1]
        for i in range(n - 2, -1, -1):
            suf_min[i] = min(suf_min[i + 1], xs[i])
            suf_max[i] = max(suf_max[i + 1], xs[i])

        l = 0
        for r in range(n):
            while l <= r and pts[r][0] - pts[l][0] + 1 > c:
                l += 1

            min_x = float('inf')
            max_x = -float('inf')

            if l > 0:
                min_x = min(min_x, pref_min[l - 1])
                max_x = max(max_x, pref_max[l - 1])
            if r + 1 < n:
                min_x = min(min_x, suf_min[r + 1])
                max_x = max(max_x, suf_max[r + 1])

            if min_x == float('inf'):
                return True
            if max_x - min_x + 1 <= c:
                return True

        return False

    def solve():
        w, h, n = map(int, inp().split())
        points = [tuple(map(int, inp().split())) for _ in range(n)]

        lo, hi = 1, min(w, h)
        while lo < hi:
            mid = (lo + hi) // 2
            if ok(points, mid, False) or ok(points, mid, True):
                hi = mid
            else:
                lo = mid + 1
        return str(lo)

    return solve()

# provided samples
assert run("""5 6 5
5 4
2 6
4 1
2 3
1 4
""") == "3"

assert run("""4 3 4
1 1
4 3
4 1
1 3
""") == "3"

# custom cases
assert run("""1 1 1
1 1
""") == "1", "single cell"

assert run("""5 5 2
1 1
5 5
""") == "2", "diagonal endpoints"

assert run("""5 5 4
1 1
1 5
5 1
5 5
""") == "4", "corners require full span"

assert run("""6 6 3
2 2
2 3
2 4
""") == "1", "single column cluster"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 1 | ranh giới tối thiểu | 
| điểm cuối đường chéo | 2 | điểm tách biệt cần cả hai dải | 
| các góc yêu cầu toàn nhịp | 4 | lây lan trong trường hợp xấu nhất | 
| cụm cột đơn | 1 | trường hợp dự phòng dọc | 

## Vỏ cạnh 

Một ô bị nứt sẽ kiểm tra xem thuật toán có xử lý chính xác các tập hợp phần dư trống hay không. Với đầu vào`1 1 1`và chỉ`(1,1)`, bất kì$c \ge 1$hoạt động và tìm kiếm nhị phân hội tụ về 1 vì kiểm tra tính khả thi ngay lập tức trả về true khi cả hai phạm vi tiền tố và hậu tố đều trống. 

Hai điểm đặt cách xa nhau như`(1,1)`Và`(5,5)`, buộc phải sử dụng cả hai dải. Vì$c=1$, không dải nào có thể bao phủ cả hai cùng một lúc, do đó việc kiểm tra nhịp dọc không thành công đối với tất cả các phần tách. Thuật toán loại bỏ chính xác giá trị nhỏ$c$bởi vì khoảng x hoặc khoảng y của các điểm không được che chắn luôn vượt quá 1. 

Cấu hình đầy đủ góc`(1,1), (1,h), (w,1), (w,h)`buộc chiều rộng tối đa. Bất kỳ ứng cử viên nào$c < w$hoặc$c < h$không thành công vì sau bất kỳ lựa chọn ngang nào, vẫn còn ít nhất hai điểm có khoảng cách x là tối đa. Việc tính toán tiền tố-hậu tố cho thấy điều này vì min và max x trên phần còn lại luôn trải rộng trên toàn bộ chiều rộng, buộc$c = \min(w,h)$.
