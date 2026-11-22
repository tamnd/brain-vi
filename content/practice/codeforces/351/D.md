---
title: "CF 351D - Jeff và giai đoạn loại bỏ"
description: "Đối với mỗi truy vấn, chúng ta xem xét một mảng con của mảng đã cho và tìm hiểu vẻ đẹp của nó. Một thao tác đơn lẻ chọn một số giá trị bằng nhau có vị trí tạo thành một cấp số cộng, loại bỏ chúng và sau đó cho phép chúng ta sắp xếp lại mọi thứ còn lại. Câu cuối cùng đó là chìa khóa."
date: "2026-06-06T22:08:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 351
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 204 (Div. 1)"
rating: 2700
weight: 351
solve_time_s: 159
verified: true
draft: false
---

[CF 351D - Jeff và Khoảng thời gian loại bỏ](https://codeforces.com/problemset/problem/351/D) 

**Đánh giá:** 2700 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi truy vấn, chúng ta xem xét một mảng con của mảng đã cho và yêu cầu **vẻ đẹp** của nó. 

Một thao tác đơn lẻ chọn một số giá trị bằng nhau có vị trí tạo thành một cấp số cộng, loại bỏ chúng và sau đó cho phép chúng ta sắp xếp lại mọi thứ còn lại. 

Câu cuối cùng đó là chìa khóa. Sau thao tác đầu tiên, thứ tự của các phần tử còn lại sẽ hoàn toàn có thể kiểm soát được. Nếu một giá trị xuất hiện$c$lần, chúng ta có thể đặt tất cả$c$sao chép vào các vị trí tạo thành cấp số cộng và xóa chúng trong một thao tác. Từ thời điểm đó trở đi, mỗi giá trị riêng biệt luôn có thể được xóa chỉ bằng một thao tác. 

Phần khó duy nhất là thao tác **đầu tiên**, vì nó phải sử dụng thứ tự ban đầu của mảng con được truy vấn. 

Độ dài mảng và số lượng truy vấn đều lên tới$10^5$. Bất kỳ giải pháp nào xử lý truy vấn một cách độc lập sẽ bị loại trừ ngay lập tức. Thậm chí$O(\sqrt n)$công việc cho mỗi truy vấn sẽ gần đạt đến giới hạn. Chúng tôi cần một giải pháp cấu trúc dữ liệu ngoại tuyến với khoảng$O((n+q)\sqrt n)$tổng số phức tạp. 

Trường hợp cạnh tinh tế xuất hiện khi một giá trị xuất hiện chính xác một lần trong khoảng truy vấn.```
[5, 7, 5]
```Đối với khoảng thời gian$[2,2]$, giá trị$7$xuất hiện một lần. Một vị trí đơn lẻ về cơ bản là một cấp số cộng, do đó giá trị đó có thể bị xóa ngay lập tức. Bất kỳ giải pháp nào chỉ kiểm tra các giá trị xảy ra ít nhất hai lần sẽ bỏ lỡ điều này. 

Một sai lầm dễ mắc phải khác là nghĩ rằng sau khi sắp xếp lại chúng ta có thể bằng cách nào đó kết hợp các giá trị khác nhau vào một thao tác.```
[1, 1, 2, 2]
```Các thao tác luôn chỉ xóa các giá trị bằng nhau. Ngay cả sau khi sắp xếp lại, điều tốt nhất chúng ta có thể làm là xóa tất cả$1$trong một hoạt động và tất cả$2$đang ở chỗ khác. Câu trả lời là$2$, không$1$. 

Trường hợp góc quan trọng nhất là khi mọi giá trị xuất hiện nhiều lần, nhưng không có giá trị nào trong số chúng tạo thành cấp số cộng.```
positions of 1: 1, 3, 6
positions of 2: 2, 5, 9
```Cả hai tập hợp vị trí đều không phải là một cấp số cộng. Thao tác đầu tiên không thể loại bỏ hoàn toàn bất kỳ giá trị nào. Thao tác đầu tiên thất bại thêm đó sẽ làm tăng câu trả lời lên một. 

## Phương pháp tiếp cận 

Quan điểm bạo lực hữu ích một cách đáng ngạc nhiên. 

Giả sử chúng ta biết tất cả các lần xuất hiện của mọi giá trị trong một khoảng truy vấn. Một giá trị có thể được loại bỏ ngay lập tức nếu tất cả các lần xuất hiện của nó trong khoảng đều nằm trên một cấp số cộng. Nếu chúng ta loại bỏ một giá trị như vậy trước tiên thì các giá trị riêng biệt còn lại có thể bị xóa trong một thao tác sau khi sắp xếp lại. 

Cho phép$D$là số lượng các giá trị phân biệt trong khoảng. 

Nếu một giá trị nào đó có thể được xóa ngay lập tức, chúng ta sẽ thực hiện một thao tác để xóa nó, sau đó$D-1$nhiều thao tác hơn cho các giá trị khác biệt còn lại. Câu trả lời là$D$. 

Nếu không có giá trị nào có thể xóa ngay lập tức thì thao tác đầu tiên chỉ có thể xóa một phần giá trị nào đó. Sau hoạt động đó, tất cả$D$vẫn tồn tại những giá trị riêng biệt. Sắp xếp lại thì cần$D$các hoạt động bổ sung. Câu trả lời là$D+1$. 

Vì vậy, mọi truy vấn đều giảm xuống còn hai đại lượng: 

1. Số lượng các giá trị phân biệt. 
2. Liệu có tồn tại ít nhất một giá trị mà số lần xuất hiện bên trong khoảng của nó tạo thành cấp số cộng hay không. 

Quan sát này là toàn bộ vấn đề. Nó xuất hiện trong nhiều giải pháp được chấp nhận và là nền tảng của cách tiếp cận thuật toán Mo tiêu chuẩn. 

Việc triển khai ngây thơ vẫn sẽ quá chậm. Đối với mỗi truy vấn, chúng tôi sẽ phải thu thập các lần xuất hiện của mọi giá trị và kiểm tra điều kiện cấp số cộng từ đầu. Trong trường hợp xấu nhất trở thành$O(n)$hoặc tệ hơn cho mỗi truy vấn, vượt xa những gì$10^5$truy vấn cho phép. 

Các điểm cuối của khoảng di chuyển dần dần theo thuật toán Mo. Số lượng các giá trị riêng biệt rất dễ duy trì. Thách thức thực sự là duy trì số lượng các giá trị mà số lần xuất hiện hiện tại của chúng tạo thành một cấp số cộng. 

Giải pháp được chấp nhận tính toán trước các mảng phụ trợ cho chúng ta biết chính xác khi nào việc thêm hoặc xóa một lần xuất hiện khiến một giá trị không còn là cấp số số học hoặc bắt đầu lại là một cấp số cộng. Sau đó, mỗi bản cập nhật Mo sẽ trở thành$O(1)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu (bảo trì Mo + AP) |$O((n+q)\sqrt n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Đặc tính chính 

hãy để$D$là số lượng các giá trị riêng biệt trong khoảng hiện tại. 

Cho phép$G$là số lượng các giá trị mà số lần xuất hiện của nó trong khoảng tạo thành một cấp số cộng. 

Nếu như$G>0$, câu trả lời là$D$. 

Nếu như$G=0$, câu trả lời là$D+1$. 

Vì vậy toàn bộ nhiệm vụ sẽ trở thành việc duy trì$D$Và$G$. 

### Cấu trúc xuất hiện tiền xử lý 

Đối với mọi vị trí$i$: 

1. Tính toán`pre[i]`, sự xuất hiện trước đó của cùng một giá trị. 
2. Tính toán`bak[i]`, lần xuất hiện tiếp theo của cùng một giá trị. 

Sau đó xây dựng hai mảng trợ giúp.`fl[i]`lưu trữ vị trí ngoài cùng bên trái mà chuỗi giá trị bằng nhau kết thúc tại$i$không còn là một cấp số cộng nữa.`fr[i]`lưu trữ thông tin đối xứng khi nhìn sang bên phải. 

Các mảng này được tính bằng phép lặp động trên các lần xuất hiện liên tiếp của cùng một giá trị. 

###Mo đặt hàng 

1. Sắp xếp tất cả các truy vấn bằng cách sử dụng thứ tự Mo tiêu chuẩn. 
2. Duy trì khoảng thời gian hiện tại$[L,R]$. 
3. Di chuyển$L$Và$R$để phù hợp với từng truy vấn. 

### Duy trì các giá trị riêng biệt 

1. Giữ`cnt[value]`. 
2. Khi tần số thay đổi từ$0$ĐẾN$1$, tăng$D$. 
3. Khi tần số thay đổi từ$1$ĐẾN$0$, giảm bớt$D$. 

### Duy trì các giá trị cấp số cộng 

1. Giữ$G$, số lượng giá trị mà lần xuất hiện hiện tại của chúng tạo thành một cấp số cộng. 
2. Khi một ranh giới di chuyển, chỉ có một lần xuất hiện được thêm vào hoặc xóa đi. 
3. Sử dụng`fl`Và`fr`, xác định trong$O(1)$liệu bản cập nhật đó có thay đổi trạng thái AP của giá trị tương ứng hay không. 
4. Tăng hoặc giảm$G$tương ứng. 

Các mảng được tính toán trước cho chúng tôi biết chính xác khi nào một lần xuất hiện mới xảy ra sẽ tạo ra vi phạm đầu tiên về khoảng cách bằng nhau hoặc khi loại bỏ một lần xuất hiện sẽ loại bỏ vi phạm cuối cùng. 

### Trả lời truy vấn 

1. Sau khi điều chỉnh khoảng thời gian, chúng ta biết$D$Và$G$. 
2. Nếu$G>0$, đầu ra$D$. 
3. Nếu không thì xuất ra$D+1$. 

### Tại sao nó hoạt động 

Sau thao tác đầu tiên, được phép sắp xếp lại tùy ý. Sau đó, mọi giá trị riêng biệt còn lại có thể được sắp xếp sao cho tất cả các bản sao của nó nằm trên một cấp số cộng và có thể bị xóa trong một thao tác. 

Khoảng thời gian truy vấn cần ít thao tác hơn nếu tồn tại một giá trị có thể bị xóa hoàn toàn trước khi xảy ra bất kỳ sự sắp xếp lại nào. Điều kiện đó tương đương với việc nói rằng sự xuất hiện của giá trị đó trong khoảng tạo thành một cấp số cộng. 

Do đó, câu trả lời chỉ phụ thuộc vào số lượng giá trị riêng biệt và liệu có ít nhất một giá trị hiện tại là "tốt" hay không. Cấu trúc Mo duy trì chính xác hai thông tin này nên mọi câu trả lời được báo cáo đều chính xác. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))

    maxv = 100000

    pre = [0] * (n + 1)
    bak = [0] * (n + 1)
    fl = [0] * (n + 1)
    fr = [0] * (n + 2)

    last = [0] * (maxv + 1)

    for i in range(1, n + 1):
        pre[i] = last[a[i]]
        last[a[i]] = i

        if pre[i] == 0 or pre[pre[i]] == 0:
            fl[i] = 0
        elif pre[pre[i]] - pre[i] == pre[i] - i:
            fl[i] = fl[pre[i]]
        else:
            fl[i] = pre[pre[i]]

    last = [0] * (maxv + 1)

    for i in range(n, 0, -1):
        bak[i] = last[a[i]]
        last[a[i]] = i

        if bak[i] == 0 or bak[bak[i]] == 0:
            fr[i] = n + 1
        elif bak[bak[i]] - bak[i] == bak[i] - i:
            fr[i] = fr[bak[i]]
        else:
            fr[i] = bak[bak[i]]

    q = int(input())
    block = isqrt(n) + 1

    queries = []
    for idx in range(q):
        l, r = map(int, input().split())
        queries.append((l, r, idx))

    queries.sort(key=lambda x: (x[0] // block, x[1]))

    cnt = [0] * (maxv + 1)
    ans = [0] * q

    distinct = 0
    good = 0

    def add_left(pos, cur_r):
        nonlocal distinct, good
        x = a[pos]

        cnt[x] += 1
        if cnt[x] == 1:
            distinct += 1
            good += 1
        elif fr[pos] <= cur_r and fr[bak[pos]] > cur_r:
            good -= 1

    def remove_left(pos, cur_r):
        nonlocal distinct, good
        x = a[pos]

        cnt[x] -= 1
        if cnt[x] == 0:
            distinct -= 1
            good -= 1
        elif fr[pos] <= cur_r and fr[bak[pos]] > cur_r:
            good += 1

    def add_right(pos, cur_l):
        nonlocal distinct, good
        x = a[pos]

        cnt[x] += 1
        if cnt[x] == 1:
            distinct += 1
            good += 1
        elif fl[pos] >= cur_l and fl[pre[pos]] < cur_l:
            good -= 1

    def remove_right(pos, cur_l):
        nonlocal distinct, good
        x = a[pos]

        cnt[x] -= 1
        if cnt[x] == 0:
            distinct -= 1
            good -= 1
        elif fl[pos] >= cur_l and fl[pre[pos]] < cur_l:
            good += 1

    L = 1
    R = 0

    for l, r, idx in queries:
        while L > l:
            L -= 1
            add_left(L, R)

        while R < r:
            R += 1
            add_right(R, L)

        while L < l:
            remove_left(L, R)
            L += 1

        while R > r:
            remove_right(R, L)
            R -= 1

        ans[idx] = distinct if good > 0 else distinct + 1

    sys.stdout.write("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý xây dựng các liên kết xảy ra trước đó và tiếp theo cho mọi giá trị. Những liên kết đó sau đó được sử dụng để tính toán`fl`Và`fr`, mã hóa nơi thuộc tính cấp số cộng bị phá vỡ lần đầu tiên. 

Cấu trúc Mo duy trì một khoảng trượt. Mỗi bản cập nhật chỉ thay đổi một vị trí, do đó trạng thái của một giá trị duy nhất có thể thay đổi. Mảng trợ giúp cho phép chúng tôi phát hiện sự thay đổi đó theo thời gian không đổi. 

Một nguồn lỗi phổ biến là quên rằng các hàm cập nhật phải sử dụng ranh giới khoảng **hiện tại**, chứ không phải ranh giới truy vấn đang được xử lý. Một lỗi thường gặp khác là cập nhật điểm cuối của khoảng trước khi đánh giá các điều kiện chuyển tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Khoảng đầu vào:```
[2, 2, 1, 1, 2]
```| Giá trị | Vị trí | 
| --- | --- | 
| 1 | 3, 4 | 
| 2 | 1, 2, 5 | 

Các giá trị riêng biệt: 

| D | Giá trị tốt | 
| --- | --- | 
| 2 | Giá trị 1 | 

Các vị trí giá trị$1$là$(3,4)$, đó là một cấp số cộng. 

Vì tồn tại một giá trị tốt nên câu trả lời là$D=2$. 

Ví dụ này thể hiện quan sát trung tâm: một giá trị có thể bị xóa ngay lập tức, sau đó giá trị còn lại sẽ bị xóa sau khi sắp xếp lại. 

### Ví dụ tùy chỉnh 

Mảng:```
[1, 2, 1, 2, 2, 1]
```Truy vấn:```
[1, 6]
```| Giá trị | Vị trí | 
| --- | --- | 
| 1 | 1, 3, 6 | 
| 2 | 2, 4, 5 | 

Kiểm tra khoảng cách bằng nhau: 

| Giá trị | Sự khác biệt | AP? | 
| --- | --- | --- | 
| 1 | 2, 3 | Không | 
| 2 | 2, 1 | Không | 

Đây$D=2$Và$G=0$. 

Trả lời:$$D + 1 = 3$$Thao tác đầu tiên không thể loại bỏ hoàn toàn bất kỳ giá trị nào, vì vậy thao tác bổ sung là không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\sqrt n)$| Thuật toán Mo chuẩn với$O(1)$cập nhật | 
| Không gian |$O(n)$| Liên kết xuất hiện, mảng trợ giúp, tần suất, truy vấn | 

Với$n,q \le 10^5$, đại khái$O((n+q)\sqrt n)$các hoạt động thoải mái phù hợp trong giới hạn thời gian và mức sử dụng bộ nhớ vẫn ở mức dưới 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from math import isqrt

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    n = int(input())
    a = [0] + list(map(int, input().split()))

    maxv = 100000

    pre = [0] * (n + 1)
    bak = [0] * (n + 1)
    fl = [0] * (n + 1)
    fr = [0] * (n + 2)

    last = [0] * (maxv + 1)

    for i in range(1, n + 1):
        pre[i] = last[a[i]]
        last[a[i]] = i

        if pre[i] == 0 or pre[pre[i]] == 0:
            fl[i] = 0
        elif pre[pre[i]] - pre[i] == pre[i] - i:
            fl[i] = fl[pre[i]]
        else:
            fl[i] = pre[pre[i]]

    last = [0] * (maxv + 1)

    for i in range(n, 0, -1):
        bak[i] = last[a[i]]
        last[a[i]] = i

        if bak[i] == 0 or bak[bak[i]] == 0:
            fr[i] = n + 1
        elif bak[bak[i]] - bak[i] == bak[i] - i:
            fr[i] = fr[bak[i]]
        else:
            fr[i] = bak[bak[i]]

    q = int(input())
    block = isqrt(n) + 1

    qs = []
    for idx in range(q):
        l, r = map(int, input().split())
        qs.append((l, r, idx))

    qs.sort(key=lambda x: (x[0] // block, x[1]))

    cnt = [0] * (maxv + 1)
    res = [0] * q

    distinct = 0
    good = 0

    def add_left(pos, cur_r):
        nonlocal distinct, good
        x = a[pos]
        cnt[x] += 1
        if cnt[x] == 1:
            distinct += 1
            good += 1
        elif fr[pos] <= cur_r and fr[bak[pos]] > cur_r:
            good -= 1

    def remove_left(pos, cur_r):
        nonlocal distinct, good
        x = a[pos]
        cnt[x] -= 1
        if cnt[x] == 0:
            distinct -= 1
            good -= 1
        elif fr[pos] <= cur_r and fr[bak[pos]] > cur_r:
            good += 1

    def add_right(pos, cur_l):
        nonlocal distinct, good
        x = a[pos]
        cnt[x] += 1
        if cnt[x] == 1:
            distinct += 1
            good += 1
        elif fl[pos] >= cur_l and fl[pre[pos]] < cur_l:
            good -= 1

    def remove_right(pos, cur_l):
        nonlocal distinct, good
        x = a[pos]
        cnt[x] -= 1
        if cnt[x] == 0:
            distinct -= 1
            good -= 1
        elif fl[pos] >= cur_l and fl[pre[pos]] < cur_l:
            good += 1

    L, R = 1, 0

    for l, r, idx in qs:
        while L > l:
            L -= 1
            add_left(L, R)
        while R < r:
            R += 1
            add_right(R, L)
        while L < l:
            remove_left(L, R)
            L += 1
        while R > r:
            remove_right(R, L)
            R -= 1

        res[idx] = distinct if good > 0 else distinct + 1

    return "\n".join(map(str, res))

# sample 1
assert run(
"""5
2 2 1 1 2
5
1 5
1 1
2 2
1 3
2 3
"""
) == "2\n1\n1\n2\n2"

# minimum size
assert run(
"""1
7
1
1 1
"""
) == "1"

# all equal
assert run(
"""4
5 5 5 5
1
1 4
"""
) == "1"

# two distinct singletons
assert run(
"""2
1 2
1
1 2
"""
) == "2"

# AP positions present
assert run(
"""5
1 2 1 3 1
1
1 5
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | 1 | Khoảng thời gian nhỏ nhất có thể | 
| Tất cả các giá trị bằng nhau | 1 | Một giá trị bị xóa trong một thao tác | 
| Hai singletons riêng biệt | 2 | Giá trị Singleton luôn tốt | 
| Vị trí 1,3,5 có cùng giá trị | 3 | Phát hiện cấp số cộng | 

## Vỏ cạnh 

Hãy xem xét:```
1
7
1
1 1
```Khoảng chứa một giá trị xuất hiện một lần. Một vị trí duy nhất là một cấp số cộng, vì vậy$G=1$,$D=1$, và câu trả lời là$1$. 

Coi như:```
4
5 5 5 5
1
1 4
```Tất cả các lần xuất hiện thuộc về một giá trị. Vị trí của chúng tạo thành một cấp số cộng, do đó toàn bộ khoảng biến mất trong một thao tác. Thuật toán giữ$D=1$Và$G=1$, đưa ra câu trả lời$1$. 

Coi như:```
6
1 2 1 2 2 1
1
1 6
```Giá trị$1$chiếm vị trí$1,3,6$, sự khác biệt$2,3$. Giá trị$2$chiếm vị trí$2,4,5$, sự khác biệt$2,1$. Không có giá trị là tốt. Thuật toán kết thúc bằng$D=2$Và$G=0$, trở về$3$, phù hợp với chiến lược tối ưu.
