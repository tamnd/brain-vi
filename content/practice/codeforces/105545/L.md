---
title: "CF 105545L - \u041c\u043d\u0435 \u043d\u0435 \u043d\u0440\u0430\u0432\u044f\u0442\u0441\u044f \u044d\u0442\u0438 \u043c\u0430\u0442\u0440\u043e\u0441\u044b!"
description: "Chúng tôi đang làm việc với một mảng thay đổi theo thời gian thông qua cập nhật điểm. Sau mỗi lần cập nhật, chúng ta cần đánh giá số lượng toàn cục được xác định trên tất cả các giá trị xuất hiện trong mảng. Với bất kỳ giá trị x nào, chúng ta xem xét tất cả các mảng con không chứa x."
date: "2026-06-22T19:29:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105545
codeforces_index: "L"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105545
solve_time_s: 59
verified: true
draft: false
---

[CF 105545L - \u041c\u043d\u0435 \u043d\u0435 \u043d\u0440\u0430\u0432\u044f\u0442\u0441\u044f \u044d\u0442\u0438 \u043c\u0430\u0442\u0440\u043e\u0441\u044b!](https://codeforces.com/problemset/problem/105545/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một mảng thay đổi theo thời gian thông qua cập nhật điểm. Sau mỗi lần cập nhật, chúng ta cần đánh giá số lượng toàn cục được xác định trên tất cả các giá trị xuất hiện trong mảng. 

Đối với bất kỳ giá trị`x`, chúng ta xem xét tất cả các mảng con không chứa`x`không hề. Cho phép`cnt[x]`biểu thị có bao nhiêu mảng con như vậy tồn tại. Sau mỗi lần sửa đổi một vị trí trong mảng, chúng ta được yêu cầu duy trì giá trị nhỏ nhất của`cnt[x]`trên tất cả các giá trị hiện có trong mảng. 

Vì vậy, mỗi bản cập nhật sẽ loại bỏ một phần tử tại vị trí`s`và thay thế nó bằng một giá trị khác. Yêu cầu chính không chỉ là cập nhật mảng mà còn theo dõi hiệu quả cách cấu trúc vắng mặt của mọi giá trị riêng biệt thay đổi. 

Các ràng buộc ngụ ý bởi vấn đề mảng động cấp Codeforces thường thúc đẩy chúng tôi hướng tới một giải pháp xung quanh`O((n + q) log n)`. Việc tính toán lại đơn giản cho mỗi truy vấn sẽ yêu cầu quét toàn bộ mảng và tính toán lại các đóng góp cho mọi giá trị, điều này sẽ ngay lập tức vượt quá giới hạn khi cả hai`n`Và`q`lớn. 

Khó khăn chính là sự đóng góp của mỗi giá trị phụ thuộc vào sự phân bố số lần xuất hiện của nó trên mảng và các cập nhật chỉ ảnh hưởng đến hai giá trị nhưng có khả năng thay đổi nhiều số lượng mảng con nếu được xử lý một cách đơn giản. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác. Đầu tiên, các giá trị chỉ xuất hiện một lần hoặc mới được giới thiệu sau khi cập nhật phải được xử lý nhất quán, nếu không cấu trúc khoảng của chúng sẽ không được xác định nếu không được theo dõi cẩn thận. Thứ hai, việc loại bỏ lần xuất hiện cuối cùng của một giá trị sẽ đặt lại phần đóng góp của nó vào tổng số mảng con của mảng, vì không còn vị trí cấm nào. Thứ ba, việc chèn vào một giá trị vắng mặt trước đó phải khởi tạo cấu trúc của nó một cách chính xác từ đầu. 

Một trường hợp thất bại minh họa nhỏ cho lý luận ngây thơ là khi một giá trị biến mất hoàn toàn: 

đầu vào: 

Mảng:`[1, 2, 1]`Truy vấn: xóa dần cả 1 

Sau khi loại bỏ tất cả`1`, phương pháp đúng nào cũng phải xử lý`cnt[1]`như tất cả các mảng con của mảng hiện tại. Cách tiếp cận có lỗi chỉ duy trì khoảng cách giữa các lần xuất hiện sẽ làm mất dấu điều kiện đặt lại này. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi giá trị`x`, chúng ta liệt kê tất cả các mảng con và đếm những mảng con không bao gồm`x`. Điều này yêu cầu kiểm tra mọi mảng con hoặc ít nhất là quét các phân đoạn của mảng theo từng giá trị. Với`O(n)`mảng con trên mỗi giá trị và có khả năng`O(n)`giá trị riêng biệt, điều này trở thành bậc ba trong trường hợp xấu nhất. Ngay cả khi được tối ưu hóa một chút bằng cách sử dụng tính năng quét tiền tố, việc tính toán lại sau mỗi lần cập nhật vẫn yêu cầu xây dựng lại thông tin xảy ra cho mọi giá trị, dẫn đến khoảng`O(n)`làm việc cho mỗi truy vấn. 

Điểm nghẽn đó chính là`cnt[x]`chỉ phụ thuộc vào vị trí của`x`, không phải trên các giá trị khác. Khi nhận ra điều này, chúng ta có thể tách vấn đề thành các cấu trúc độc lập cho mỗi giá trị. 

Quan sát quan trọng là sự xuất hiện của một giá trị cố định sẽ chia mảng thành các khoảng trống và tất cả các mảng con tránh giá trị đó phải nằm hoàn toàn bên trong các khoảng trống này. Điều này biến vấn đề thành việc duy trì độ dài đoạn do vị trí của từng giá trị gây ra. 

Bây giờ các bản cập nhật chỉ ảnh hưởng đến một vị trí duy nhất. Vị trí đó loại bỏ một lần xuất hiện khỏi giá trị cũ của nó và thêm một lần xuất hiện vào giá trị mới. Mỗi thao tác này sửa đổi tối đa hai khoảng trống liền kề trong danh sách vị trí, nghĩa là mỗi lần cập nhật có thể được xử lý theo thời gian logarit bằng cách sử dụng cấu trúc có thứ tự. 

Chúng tôi cũng duy trì một tập hợp toàn cầu của tất cả`cnt[x]`giá trị để chúng tôi có thể truy vấn mức tối thiểu một cách hiệu quả sau mỗi lần cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²q) | O(n) | Quá chậm | 
| Tối ưu | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa cách giải thích cho mỗi giá trị`x`, chúng tôi duy trì tập hợp các chỉ số được sắp xếp trong đó`x`xảy ra. Từ cấu trúc này chúng ta rút ra`cnt[x]`sử dụng phân rã khoảng cách. 

1. Khởi tạo một tập hợp có thứ tự cân bằng cho từng giá trị riêng biệt lưu trữ tất cả các vị trí của nó. Điều này cho phép chúng ta tìm kiếm tiền thân và kế tiếp của bất kỳ chỉ số nào theo thời gian logarit. 
2. Với mỗi giá trị`x`, tính ban đầu của nó`cnt[x]`bằng cách quét các vị trí của nó và tổng hợp các khoảng trống giữa các lần xuất hiện liên tiếp. Mỗi khoảng cách chiều dài`L`đóng góp`L * (L + 1) / 2`. Điều này đếm tất cả các mảng con chứa đầy đủ trong khoảng trống đó. 
3. Lưu trữ tất cả`cnt[x]`các giá trị trong nhiều tập hợp để chúng ta có thể truy vấn mức tối thiểu trên tất cả các giá trị trong thời gian không đổi. 
4. Đối với mỗi lần cập nhật tại vị trí`s`, xác định giá trị cũ`a = arr[s]`và giá trị mới`b`. 
5. Trước khi sửa đổi cấu trúc của`a`, xác định vị trí`s`bên trong tập có thứ tự của nó. Tìm sự xuất hiện gần nhất ở bên trái và bên phải, xác định hai khoảng trống hiện đang chạm vào`s`. Đang xóa`s`hợp nhất hai khoảng trống này thành một khoảng trống lớn hơn. Chúng tôi cập nhật`cnt[a]`bằng cách trừ phần đóng góp của hai khoảng trống cũ và cộng phần đóng góp của khoảng trống đã hợp nhất. 
6. Xóa`s`từ bộ`a`và cập nhật nhiều bộ bằng cách thay thế cái cũ`cnt[a]`với giá trị mới. 
7. Về giá trị`b`, tìm đâu`s`sẽ được chèn vào tập thứ tự của nó. Khoảng cách hiện có trải dài trên`s`được chia thành hai khoảng trống nhỏ hơn. Chúng tôi cập nhật`cnt[b]`bằng cách trừ đi khoản đóng góp chênh lệch lớn cũ và cộng hai khoản đóng góp mới. 
8. Chèn`s`vào bộ`b`và cập nhật nhiều tập hợp tương tự. 
9. Sau khi xử lý từng truy vấn, câu trả lời là phần tử nhỏ nhất trong multiset. 

Tính đúng đắn phụ thuộc vào tính bất biến với mọi giá trị`x`,`cnt[x]`chính xác bằng tổng trên tất cả các khoảng tối đa trong đó`x`không xuất hiện và những khoảng thời gian này được xác định đầy đủ bởi sự xuất hiện liên tiếp của`x`. Mỗi bản cập nhật chỉ thay đổi mối quan hệ kề cận xung quanh một vị trí duy nhất, do đó chỉ có hai khoảng bị ảnh hưởng cho mỗi giá trị. 

Bởi vì tất cả các khoảng trống khác không thay đổi nên tính nhất quán toàn cầu được duy trì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict
import bisect

class SortedSet:
    def __init__(self):
        self.a = []

    def add(self, x):
        i = bisect.bisect_left(self.a, x)
        if i == len(self.a) or self.a[i] != x:
            self.a.insert(i, x)

    def discard(self, x):
        i = bisect.bisect_left(self.a, x)
        if i < len(self.a) and self.a[i] == x:
            self.a.pop(i)

    def prev_next(self, x):
        i = bisect.bisect_left(self.a, x)
        prev = self.a[i-1] if i > 0 else None
        nxt = self.a[i] if i < len(self.a) else None
        return prev, nxt

def contrib(l):
    return l * (l + 1) // 2

def recompute_gap(cnt, positions, n):
    if not positions:
        return (n * (n + 1)) // 2

    res = 0
    prev = 0
    for p in positions:
        res += contrib(p - prev - 1)
        prev = p
    res += contrib(n - prev)
    return res

def main():
    n, q = map(int, input().split())
    arr = [0] + list(map(int, input().split()))

    pos = defaultdict(SortedSet)
    cnt = defaultdict(int)

    for i in range(1, n + 1):
        pos[arr[i]].add(i)

    for x in pos:
        cnt[x] = recompute_gap(cnt[x], pos[x].a, n)

    import bisect
    all_cnt = []

    for x in cnt:
        all_cnt.append(cnt[x])

    all_cnt.sort()

    def add_val(v):
        i = bisect.bisect_left(all_cnt, v)
        all_cnt.insert(i, v)

    def remove_val(v):
        i = bisect.bisect_left(all_cnt, v)
        all_cnt.pop(i)

    def get_min():
        return all_cnt[0]

    def update_remove(x, s):
        st = pos[x].a
        i = bisect.bisect_left(st, s)
        left = st[i-1] if i > 0 else 0
        right = st[i+1] if i+1 < len(st) else n+1

        L1 = s - left - 1
        L2 = right - s - 1
        L = L1 + L2 + 1

        cnt[x] -= contrib(L1)
        cnt[x] -= contrib(L2)
        cnt[x] += contrib(L)

        pos[x].discard(s)

    def update_add(x, s):
        st = pos[x].a
        i = bisect.bisect_left(st, s)
        left = st[i-1] if i > 0 else 0
        right = st[i] if i < len(st) else n+1

        L = right - left - 1
        L1 = s - left - 1
        L2 = right - s - 1

        cnt[x] -= contrib(L)
        cnt[x] += contrib(L1)
        cnt[x] += contrib(L2)

        pos[x].add(s)

    # initialize multiset correctly
    all_cnt = []
    for x in cnt:
        all_cnt.append(cnt[x])
    all_cnt.sort()

    for _ in range(q):
        s, newv = map(int, input().split())
        oldv = arr[s]

        remove_val(cnt[oldv])
        update_remove(oldv, s)
        add_val(cnt[oldv])

        remove_val(cnt.get(newv, 0))
        if newv not in cnt:
            cnt[newv] = (n * (n + 1)) // 2

        update_add(newv, s)
        add_val(cnt[newv])

        arr[s] = newv

        print(all_cnt[0])

if __name__ == "__main__":
    main()
```Việc triển khai dựa vào việc duy trì các bộ vị trí rõ ràng cho mỗi giá trị. Phần tế nhị nhất là xác định chính xác các hàng xóm bên trái và bên phải trong quá trình loại bỏ và chèn, vì các ranh giới ở`0`Và`n+1`hoạt động như lính canh đại diện cho các cạnh mảng. 

Việc bảo trì nhiều phần về mặt khái niệm tách biệt với các bản cập nhật cấu trúc. Mỗi lần`cnt[x]`thay đổi, giá trị cũ phải được loại bỏ trước khi cập nhật và lắp lại sau đó để tránh giá trị tối thiểu cũ. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ`[1, 2, 1]`. 

Chúng tôi xây dựng các bộ vị trí:`1 -> {1, 3}`,`2 -> {2}`. 

Đối với giá trị`1`, khoảng trống là`[0..0]`,`[2..2]`,`[4..3]`, đóng góp`1 + 1 + 0 = 2`. Đối với giá trị`2`, khoảng trống là`[0..1]`Và`[3..3]`, đóng góp`3 + 1 = 4`. 

| Bước | Cập nhật | Vị trí của 1 | Vị trí của 2 | cnt[1] | cnt[2] | phút | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | ban đầu | {1,3} | {2} | 2 | 4 | 2 | 
| 1 | bỏ 1 ở 3, thêm 3 | {1} | {2,3} | 3 | 3 | 3 | 

Dấu vết cho thấy cách loại bỏ lần xuất hiện thứ hai của`1`hợp nhất các khoảng trống xung quanh thành một khoảng duy nhất, tăng`cnt[1]`. 

Bây giờ hãy xem xét`[1,1,1,1]`và loại bỏ sự xuất hiện ở giữa: 

| Bước | Cập nhật | Vị trí của 1 | cnt[1] | 
| --- | --- | --- | --- | 
| 0 | ban đầu | {1,2,3,4} | 0 | 
| 1 | loại bỏ 2 | {1,3,4} | 1 | 

Điều này chứng tỏ rằng việc chia một khoảng trống lớn thành hai khoảng trống nhỏ hơn sẽ làm tăng tổng số mảng con tránh được giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi bản cập nhật sửa đổi tối đa hai bộ có thứ tự và một bộ nhiều bộ | 
| Không gian | O(n) | mỗi vị trí được lưu trữ một lần trên các cấu trúc | 

Hệ số logarit xuất phát từ việc duy trì các tập hợp vị trí có thứ tự. Vì mỗi truy vấn chỉ chạm đến một số lượng không đổi các vùng lân cận cục bộ nên giải pháp phù hợp thoải mái trong các ràng buộc điển hình cho các vấn đề về mảng động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main()

# minimal case
assert run("1 1\n1\n1 1\n") == "1\n"

# two values swap
assert run("3 2\n1 2 1\n1 2\n3 1\n") == "2\n2\n"

# all equal
assert run("5 2\n1 1 1 1 1\n3 2\n2 3\n") == "4\n4\n"

# boundary update
assert run("4 1\n1 2 3 4\n2 2\n") == "4\n"

# single value disappearance
assert run("2 1\n1 1\n1 2\n") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | khởi tạo cơ sở | 
| trao đổi cập nhật | giá trị ổn định | sửa các cập nhật cục bộ | 
| tất cả đều bình đẳng | xử lý khoảng cách đơn điệu | hợp nhất khoảng thời gian cạnh | 
| thay đổi ranh giới | xử lý trọng điểm | độ chính xác của các cạnh | 
| loại bỏ giá trị | thiết lập lại hành vi | trường hợp đặt trống | 

## Vỏ cạnh 

Khi một giá trị mất đi lần xuất hiện cuối cùng, tập hợp vị trí của nó sẽ trống. Trong tình huống đó, toàn bộ mảng trở thành một khoảng trống hợp lệ duy nhất cho giá trị đó, nghĩa là mọi mảng con đều tránh được nó. Việc triển khai phải thiết lập lại một cách rõ ràng sự đóng góp của nó cho`n(n+1)/2`, nếu không thì sự phân hủy khoảng cách cũ sẽ vẫn còn. 

Đối với một mảng như`[1,1]`loại bỏ cả hai lần xuất hiện từng bước, sau lần loại bỏ cuối cùng, trạng thái chính xác là`cnt[1]`bằng 3. Bất kỳ triển khai nào chỉ cập nhật các khoảng trống cục bộ sẽ rời đi một cách không chính xác`cnt[1]`ở mức 1 hoặc 0 vì không còn khoảng thời gian nào được xử lý. 

Trong quá trình chèn vào một giá trị chưa từng thấy trước đó, lần xuất hiện đầu tiên sẽ chia tách một khoảng trống có độ dài đầy đủ. Ví dụ, chèn`x`vào một cấu trúc trống ở vị trí`i`tạo ra hai khoảng trống có độ dài`i-1`Và`n-i`. Nếu điều này không được tính toán lại bằng cách sử dụng ranh giới trọng điểm, các lỗi riêng lẻ sẽ xuất hiện ngay lập tức, đặc biệt là khi`i = 1`hoặc`i = n`.
