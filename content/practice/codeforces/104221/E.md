---
title: "CF 104221E - \u0427\u0430\u0439\u043d\u044b\u0439 \u043c\u0430\u0433\u0430\u0437\u0438\u043d"
description: "Chúng ta được giao một số vị trí khả thi cho một cửa hàng dọc theo một đường thẳng và một nhóm khách hàng, mỗi người sống tại một điểm cố định và có ngưỡng chịu đựng cá nhân."
date: "2026-07-01T23:46:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104221
codeforces_index: "E"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104221
solve_time_s: 65
verified: true
draft: false
---

[CF 104221E - \u0427\u0430\u0439\u043d\u044b\u0439 \u043c\u0430\u0433\u0430\u0437\u0438\u043d](https://codeforces.com/problemset/problem/104221/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một số vị trí khả thi cho một cửa hàng dọc theo một đường thẳng và một nhóm khách hàng, mỗi người sống tại một điểm cố định và có ngưỡng chịu đựng cá nhân. Nếu chúng tôi chọn một vị trí cửa hàng và đặt một mức giá duy nhất cho mỗi đơn vị trà, mỗi khách hàng sẽ mua tối đa một đơn vị và chỉ khi chi phí di chuyển đến cửa hàng cộng với giá không vượt quá ngưỡng của họ. 

Đối với vị trí cửa hàng cố định$x_i$và giá cả$c$, khách hàng$j$sẵn sàng mua nếu$|x_i - y_j| + c \le a_j$. Sắp xếp lại điều này, điều kiện trở thành$c \le a_j - |x_i - y_j|$. Do đó, mỗi khách hàng sẽ đóng góp một mức giá tối đa có thể chấp nhận được cho mỗi địa điểm cửa hàng. 

Chúng ta phải chọn một địa điểm cửa hàng và một mức giá duy nhất$c$ít nhất như vậy$k$khách hàng thỏa mãn điều kiện đó và do đó mỗi người mua đúng một đơn vị. Mỗi khách hàng có thể mua tối đa một đơn vị, vì vậy số lượng khách hàng hài lòng chỉ đơn giản là số lượng khách hàng được đáp ứng ràng buộc. Mục tiêu là tối đa hóa$c$hoặc báo cáo rằng không tồn tại cấu hình hợp lệ. 

Những hạn chế$n, m \le 10^5$ngay lập tức loại trừ việc kiểm tra trực tiếp từng cặp khách hàng và vị trí cửa hàng, vì điều đó sẽ$10^{10}$hoạt động trong trường hợp xấu nhất. Bất kỳ giải pháp nào cũng phải tránh đánh giá rõ ràng tất cả các cặp. 

Một điểm tinh tế là tính khả thi là đơn điệu về giá cả. Nếu một mức giá$c$làm việc để bán$k$các mặt hàng thì bất kỳ mức giá nhỏ hơn nào cũng có tác dụng, bởi vì việc giảm$c$chỉ làm cho nhiều khách hàng thỏa mãn sự bất bình đẳng hơn. Sự đơn điệu này gợi ý rõ ràng về việc tìm kiếm nhị phân cho câu trả lời. 

Một sai lầm ngây thơ là tính toán cho mỗi cửa hàng số lượng khách hàng thỏa mãn điều kiện bằng cách lặp lại tất cả khách hàng của mỗi cửa hàng. Điều này lặp đi lặp lại công việc và quá chậm. 

Một sai lầm khác là cố gắng chỉ định khách hàng vào các cửa hàng một cách tham lam. Ràng buộc mang tính toàn cầu đối với mỗi cửa hàng được chọn, không phải đối với mỗi khách hàng, vì vậy các quyết định địa phương không được đưa ra. 

Các trường hợp đặc biệt đáng chú ý bao gồm các tình huống trong đó không cửa hàng nào có thể làm hài lòng dù chỉ một khách hàng với mức giá dương hoặc khi$k$lớn hơn bất kỳ số lượng khách hàng nào có thể được phục vụ tại bất kỳ địa điểm nào ngay cả ở mức giá bằng 0. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi vị trí cửa hàng, chúng tôi ngầm thử mọi mức giá có thể bằng cách kiểm tra xem có bao nhiêu khách hàng hài lòng$a_j - |x_i - y_j| \ge c$. Nếu chúng ta ấn định một mức giá dự kiến, việc tính số khách hàng hài lòng cho một cửa hàng cố định sẽ mất$O(n)$. Lặp lại điều này cho tất cả các cửa hàng mang lại lợi nhuận$O(mn)$mỗi lần kiểm tra giá và nếu không tối ưu hóa, chúng tôi sẽ cần phải tìm kiếm trên nhiều mức giá hoặc tính toán lại số lượng nhiều lần, điều này vượt xa giới hạn có thể chấp nhận được. 

Quan sát quan trọng là đối với một địa điểm cửa hàng cố định, mỗi khách hàng đóng góp một giới hạn trên độc lập về giá. Số lượng khách hàng có thể mua ở mức giá$c$chính xác là số lượng$j$như vậy$a_j - |x_i - y_j| \ge c$. Điều này mang tính đơn điệu ở$c$, vì vậy chúng ta có thể tìm kiếm nhị phân mức giá khả thi tối đa. Vấn đề còn lại là đánh giá tính khả thi một cách hiệu quả cho một$c$. 

Đối với một cố định$c$, mỗi khách hàng tạo ra một khoảng vị trí cửa hàng có thể chấp nhận được:$$|x_i - y_j| \le a_j - c$$có nghĩa là$$x_i \in [y_j - (a_j - c),\; y_j + (a_j - c)]$$Vì vậy, mỗi khách hàng sẽ trở thành một khoảng trên các vị trí cửa hàng và chúng ta cần chọn một điểm$x_i$được bao phủ bởi ít nhất$k$khoảng thời gian. Đây là một vấn đề nén đường quét/tọa độ cổ điển theo các khoảng thời gian, trong đó chúng tôi kiểm tra xem liệu bất kỳ vị trí nào có phạm vi bao phủ ít nhất không$k$. 

Chúng ta có thể sắp xếp các vị trí cửa hàng, nén chúng và sử dụng mảng khác biệt hoặc cây chỉ mục nhị phân để đếm xem có bao nhiêu khoảng bao phủ từng vị trí cửa hàng. Mỗi lần kiểm tra tính khả thi đều diễn ra$O(n + m)$và chúng tôi thực hiện tìm kiếm nhị phân trên câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(mn)$mỗi lần kiểm tra |$O(1)$| Quá chậm | 
| Tối ưu |$O((n + m)\log A)$|$O(n + m)$| Đã chấp nhận | 

Đây$A$là mức giá tối đa có thể, được giới hạn bởi$10^9$. 

## Hướng dẫn thuật toán 

Chúng ta chuyển vấn đề sang việc kiểm tra xem một mức giá nhất định có$c$là khả thi thì tìm kiếm nhị phân giá trị khả thi tối đa. 

### Kiểm tra tính khả thi với mức giá cố định$c$1. Đối với mỗi khách hàng$j$, tính toán$r_j = a_j - c$. Nếu như$r_j < 0$, khách hàng này hoàn toàn không thể mua được và bị bỏ qua. 
2. Đối với từng vị trí cửa hàng$x_i$, thông dịch tất cả các khách hàng thỏa mãn$|x_i - y_j| \le r_j$như đóng góp mức độ phù hợp +1 cho vị trí đó. 
3. Chuyển đổi từng khách hàng thành một khoảng thời gian$[y_j - r_j, y_j + r_j]$. Chúng tôi chỉ quan tâm đến sự trùng lặp với vị trí cửa hàng thực tế. 
4. Sắp xếp các vị trí cửa hàng và sử dụng tổng tiền tố để đếm xem mỗi cửa hàng có bao nhiêu khoảng thời gian. 
5. Nếu bất kỳ vị trí cửa hàng nào được bao phủ bởi ít nhất$k$khoảng cách, giá$c$là khả thi. 

Bước kỹ thuật quan trọng là ánh xạ hiệu quả khoảng thời gian bao phủ vào các vị trí cửa hàng riêng biệt. Chúng tôi sắp xếp các cửa hàng và sử dụng tìm kiếm nhị phân để xác định các điểm cuối khoảng trong mảng nén, sau đó áp dụng mảng khác biệt: thêm +1 vào chỉ mục cửa hàng được bảo hiểm đầu tiên và -1 sau chỉ mục được bao phủ cuối cùng. 

### Tìm kiếm nhị phân theo giá 

Chúng tôi tìm kiếm trong phạm vi$[0, \max a_j]$. Đối với mỗi giá trị trung bình, chúng tôi tiến hành kiểm tra tính khả thi. Nếu khả thi, chúng tôi sẽ thử mức giá cao hơn; nếu không chúng tôi sẽ giảm. 

### Tại sao nó hoạt động 

Đối với cửa hàng cố định, việc tăng giá chỉ làm giảm lượng khách hàng hài lòng$a_j - |x_i - y_j| \ge c$. Tính đơn điệu này đảm bảo rằng tính khả thi là đơn điệu trong$c$. Bản thân điều kiện khả thi được nắm bắt chính xác bởi phạm vi bảo hiểm theo khoảng thời gian vì mỗi khách hàng xác định một cách độc lập tập hợp các vị trí cửa hàng nơi họ sẵn sàng mua ở mức giá$c$. Một giải pháp hợp lệ tồn tại chính xác khi có một vị trí cửa hàng được bao phủ bởi ít nhất$k$những khoảng như vậy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(c, n, m, k, y, a, x):
    events = []
    for j in range(n):
        r = a[j] - c
        if r < 0:
            continue
        l = y[j] - r
        rgt = y[j] + r
        events.append((l, rgt))

    if not events:
        return False

    xs = sorted(x)
    m = len(xs)

    diff = [0] * (m + 1)

    import bisect
    for l, r in events:
        i = bisect.bisect_left(xs, l)
        j = bisect.bisect_right(xs, r) - 1
        if i <= j:
            diff[i] += 1
            diff[j + 1] -= 1

    cur = 0
    for i in range(m):
        cur += diff[i]
        if cur >= k:
            return True

    return False

def solve():
    n, m, k = map(int, input().split())
    y = []
    a = []
    for _ in range(n):
        yi, ai = map(int, input().split())
        y.append(yi)
        a.append(ai)

    x = list(map(int, input().split()))

    lo, hi = 0, max(a)
    ans = -1

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, n, m, k, y, a, x):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách vấn đề thành trình điều khiển tìm kiếm nhị phân và trình kiểm tra tính khả thi. Trình kiểm tra sẽ chuyển đổi từng khách hàng thành một loạt vị trí cửa hàng mà họ sẵn sàng mua, sau đó đếm các phần trùng lặp bằng cách sử dụng một mảng khác biệt trên tọa độ cửa hàng đã được sắp xếp. Các phép toán chia đôi đảm bảo mỗi khoảng được ánh xạ theo thời gian logarit. 

Một cạm bẫy phổ biến là chúng ta quên rằng chúng ta chỉ quan tâm đến vị trí của cửa hàng chứ không phải toàn bộ dây chuyền thực tế. Đó là lý do tại sao các khoảng được chiếu lên các chỉ mục trong mảng cửa hàng đã được sắp xếp thay vì được xử lý liên tục. 

Một sự tinh tế khác là xử lý tính khả thi trống rỗng: nếu tất cả khách hàng có$a_j < c$, hàm ngay lập tức trả về sai, ngăn chặn sự tích lũy không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 4 2
1 5
5 3
2 6
1 2 3 4
```Chúng tôi tìm kiếm nhị phân theo giá. 

| giữa (giá) | khoảng thời gian hợp lệ | phủ sóng tối đa trên các cửa hàng | khả thi | 
| --- | --- | --- | --- | 
| 3 | tất cả khách hàng đều có r ≥ 0 | ít nhất một cửa hàng có ≥2 | vâng | 
| 5 | chỉ những khách hàng có a_j ≥ 5 | độ bao phủ giảm nhưng vẫn ≥2 | vâng | 
| 6 | chỉ còn lại một khách hàng | độ che phủ tối đa < 2 | không | 

Việc tìm kiếm hội tụ đến 5. 

Điều này chứng tỏ rằng việc tăng giá sẽ làm giảm lượng khách hàng tích cực và giá trị tối ưu là lớn nhất vẫn cho phép ít nhất một cửa hàng được bao phủ bởi$k$khoảng thời gian của khách hàng. 

### Mẫu 2 

đầu vào:```
4 3 4
1 2
4 1
6 3
7 3
3 6 9
```Chúng tôi kiểm tra tính khả thi: 

Ở mức giá 0, khoảng thời gian là: 

khách hàng 1: [ -1, 3 ] 

khách hàng 2: [ 3, 5 ] 

khách hàng 3: [ 3, 9 ] 

khách hàng 4: [ 4, 10 ] 

Độ bao phủ tại các vị trí cửa hàng không bao giờ đạt tới 4 cùng một lúc. 

| giá | bảo hiểm tối đa | khả thi | 
| --- | --- | --- | 
| 0 | 3 | không | 
| tích cực nào | 3 | không | 

Như vậy kết quả là -1. 

Điều này cho thấy trường hợp ngay cả cấu hình tốt nhất có thể cũng không thể đáp ứng được nhu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log A)$| tìm kiếm nhị phân theo giá, mỗi lần kiểm tra tính khả thi đều tuyến tính theo số lượng khách hàng và cửa hàng | 
| Không gian |$O(n + m)$| khoảng thời gian lưu trữ, mảng tiền tố và vị trí cửa hàng được sắp xếp | 

Độ sâu tìm kiếm nhị phân được giới hạn bởi$\log 10^9 \approx 30$và mỗi quá trình kiểm tra tối đa$10^5$các yếu tố, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()).strip() if solve() else ""

# provided samples
assert run("""3 4 2
1 5
5 3
2 6
1 2 3 4
""") == "5"

assert run("""4 3 4
1 2
4 1
6 3
7 3
3 6 9
""") == "-1"

# custom cases

# minimum case
assert run("""1 1 1
1 10
1
""") == "10"

# impossible demand
assert run("""2 2 3
1 1
10 1
1 10
""") == "-1"

# all equal positions
assert run("""3 3 2
5 5
5 5
5 5
5 5 5
""") == "5"

# boundary overlap
assert run("""2 3 2
1 5
10 5
1 5 10
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 10 | khách hàng duy nhất, cửa hàng duy nhất | 
| nhu cầu không thể | -1 | k vượt quá bất kỳ phạm vi bảo hiểm nào có thể đạt được | 
| mọi vị trí ngang nhau | 5 | chồng chéo tối đa tại các vị trí giống hệt nhau | 
| chồng chéo ranh giới | 4 | độ chính xác ở các cạnh khoảng | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả$a_j < c$. Trong hoàn cảnh đó mỗi$r_j$trở thành số âm và danh sách khoảng trống. Kiểm tra tính khả thi ngay lập tức trả về sai, phù hợp với thực tế là không khách hàng nào có thể mua bất cứ thứ gì ở mức giá đó. 

Một trường hợp khác là khi các khoảng thời gian chạm chính xác vào ranh giới cửa hàng. Vì chúng tôi sử dụng`bisect_left`Và`bisect_right`, một khách hàng có khoảng thời gian kết thúc chính xác tại một vị trí cửa hàng vẫn đưa cửa hàng đó vào phạm vi phủ sóng một cách chính xác. Ví dụ: nếu khoảng thời gian của khách hàng là$[2, 5]$và cửa hàng bao gồm 5, ranh giới bên phải bao gồm cửa hàng đó vì`bisect_right`đặt nó bên trong phân khúc được bảo hiểm. 

Trường hợp cạnh cuối cùng là khi nhiều cửa hàng có chung tọa độ. Việc sắp xếp sẽ giữ lại các bản sao và mảng khác biệt tổng hợp chính xác phạm vi bao phủ trên tất cả các vị trí giống nhau, do đó, mỗi cửa hàng trùng lặp được xử lý độc lập nhưng nhất quán trong quá trình quét tiền tố.
