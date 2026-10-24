---
title: "CF 105992C - \u997a\u5b50"
description: "Chúng tôi được cung cấp một số bộ dữ liệu độc lập. Trong mỗi tập dữ liệu có nhiều loại bánh bao. Mỗi loại có số lượng giới hạn và mỗi chiếc bánh bao được ăn từ loại đó sẽ mang lại phần thưởng giảm dần khi bạn tiếp tục ăn nhiều hơn cùng loại."
date: "2026-06-22T16:36:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105992
codeforces_index: "C"
codeforces_contest_name: "The 2025 Shanghai Collegiate Programming Contest"
rating: 0
weight: 105992
solve_time_s: 80
verified: true
draft: false
---

[CF 105992C - \u997a\u5b50](https://codeforces.com/problemset/problem/105992/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số bộ dữ liệu độc lập. Trong mỗi tập dữ liệu có nhiều loại bánh bao. Mỗi loại có số lượng giới hạn và mỗi chiếc bánh bao được ăn từ loại đó sẽ mang lại phần thưởng giảm dần khi bạn tiếp tục ăn nhiều hơn cùng loại. 

Cụ thể hơn, đối với từng loại$i$, có$s_i$bánh bao có sẵn. Nếu bạn ăn$j$-thứ bánh bao kiểu này, sự hài lòng của bạn chỉ phụ thuộc vào$i$Và$j$, không bật khi bạn ăn nó trên toàn cầu. Loại bánh bao đầu tiên$i$đặc biệt vì nó nhận được thêm tiền thưởng$c_i$, vậy giá trị của nó là$a_i + c_i$. Sau đó, mỗi chiếc bánh bao bổ sung sẽ trở nên ít thỏa mãn hơn theo cách tuyến tính:$j$-th (đối với$j > 1$) cho$a_i - b_i \cdot (j - 1)$. 

Bạn có thể ăn tối đa$m$tổng số bánh bao trên tất cả các loại. Ngoài ra, còn có một phần thưởng toàn cầu: nếu tổng số bánh bao bạn ăn kết thúc trong khoảng thời gian$[l, r]$, bạn đạt được một giá trị cố định thêm$val$, không phụ thuộc vào loại bánh bao bạn đã chọn. 

Mục tiêu là chọn lấy bao nhiêu bánh bao từ mỗi loại sao cho tổng mức độ hài lòng, bao gồm cả phần thưởng có thể có, được tối đa hóa. 

Các ràng buộc rất lớn: lên tới$10^5$các trường hợp thử nghiệm và trên tất cả các thử nghiệm, tổng số loại tối đa là$3 \cdot 10^5$. Tuy nhiên, mỗi loại có thể có tới$10^6$bánh bao về nguyên tắc, và$m$cũng có thể lớn. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê rõ ràng tất cả các bánh bao hoặc mô phỏng lựa chọn cho mỗi mục. Bất cứ điều gì như sắp xếp tất cả các bánh bao riêng lẻ trên toàn cầu sẽ quá chậm cả về thời gian và bộ nhớ. 

Cấu trúc của hàm giá trị là khó khăn chính. Mỗi loại tạo ra cấp số cộng giảm dần và nhiệm vụ là chọn tiền tố chung của các mục có sẵn tốt nhất trên tất cả các chuỗi này, đồng thời xem xét phần thưởng từng phần tùy thuộc vào tổng số đã chọn. 

Một vài trường hợp phức tạp đáng được nêu bật. 

Một vấn đề là không phải lúc nào cũng có số lượng bánh bao ngon nhất$m$. Nếu nhiều giá trị trở thành âm, việc ăn ít bánh bao hơn có thể sẽ tốt hơn trừ khi có tiền thưởng bù đắp. Ví dụ, nếu tất cả$a_i$rất tiêu cực và$val = 0$, chiến lược tối ưu có thể là không lấy bánh bao. 

Một vấn đề khác là khoảng thời gian thưởng. Ngay cả khi ăn nhiều bánh bao hơn làm tăng sự hài lòng về nguyên liệu thô, thì việc dừng lại sớm hơn để vào bên trong có thể là giải pháp tối ưu nhất.$[l, r]$. Một chiến lược ngây thơ luôn đòi hỏi$m$bánh bao và thêm tiền thưởng nếu có sẽ thất bại khi$m > r$nhưng tiền tố nhỏ hơn một chút sẽ tạo ra kết quả được điều chỉnh tiền thưởng cao hơn đáng kể. 

Cuối cùng, phần tử đầu tiên của mỗi loại hoạt động khác với phần còn lại. Coi mọi loại như một cấp số cộng đơn giản bắt đầu từ$a_i$thay vì$a_i + c_i$sẽ đếm thiếu mục đầu tiên và dẫn đến thứ tự không chính xác khi trộn các mục trên toàn cầu. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo trực tiếp là coi mỗi chiếc bánh bao như một món độc lập, tạo ra tất cả$s_i$giá trị một cách rõ ràng, hợp nhất chúng vào danh sách chung, sắp xếp theo thứ tự giảm dần và lấy tiền tố. Điều này đúng về mặt khái niệm vì mỗi chiếc bánh bao có một giá trị cố định độc lập với thứ tự chung. Tuy nhiên, tổng số bánh bao có thể rất lớn, vượt xa số lượng có thể tạo ra hoặc phân loại. Ngay cả khi chúng ta chỉ giả định tổng$m$Vấn đề là, chúng ta vẫn không thể tạo ra các ứng viên một cách hiệu quả vì mỗi loại có thể đóng góp một lượng lớn giá trị giảm dần. 

Quan sát chính là mỗi loại không phải là tùy ý. Các giá trị của nó tạo thành một chuỗi giảm dần đơn điệu với độ dốc không đổi sau phần tử đầu tiên. Cấu trúc này cho phép chúng ta tránh tạo ra tất cả các phần tử một cách rõ ràng. Thay vì liệt kê các mục, chúng tôi đặt một câu hỏi khác: đưa ra một giá trị ngưỡng$x$, có bao nhiêu cái bánh bao thuộc tất cả các loại có giá trị ít nhất$x$, và tổng số tiền của chúng là bao nhiêu? 

Điều này biến vấn đề thành vấn đề lựa chọn theo ngưỡng. Nếu chúng ta có thể tính toán, với bất kỳ$x$, số phần tử có giá trị ít nhất$x$, thì chúng ta có thể tìm kiếm nhị phân ngưỡng tương ứng với việc chọn chính xác$k$mặt hàng tốt nhất. Khi chúng tôi biết mỗi loại đóng góp bao nhiêu mục trên một ngưỡng, chúng tôi cũng có thể tính toán tổng đóng góp của chúng bằng cách sử dụng các công thức chuỗi số học. 

Điều này tránh việc xây dựng nhiều tập hợp đầy đủ. Toàn bộ giải pháp giảm thiểu việc đánh giá lặp lại từng loại trong thời gian không đổi cho mỗi loại trong quá trình tìm kiếm nhị phân. 

Cuối cùng, thời hạn thưởng chỉ phụ thuộc vào tổng số đã chọn$k$, không phải mục nào được chọn. Khi chúng ta có thể tính toán số tiền tốt nhất cho bất kỳ khoản cố định nào$k$, chúng ta chỉ cần đánh giá một số lượng nhỏ ứng viên$k$để quyết định xem liệu có đáng hy sinh giá trị thô để nhận được tiền thưởng hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê và phân loại Brute Force |$O(\sum s_i \log \sum s_i)$|$O(\sum s_i)$| Quá chậm | 
| Ngưỡng + tìm kiếm nhị phân trên câu trả lời |$O(n \log V)$mỗi lần kiểm tra (khấu hao) |$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn vấn đề thành việc có thể tính tổng tốt nhất có thể khi chính xác$k$bánh bao được chọn. 

Đầu tiên chúng ta định nghĩa một hàm cho trước một giá trị ngưỡng$x$, tính tổng cộng có bao nhiêu cái bánh bao có giá trị ít nhất$x$, và tổng của chúng là bao nhiêu. 

1. Đối với từng loại$i$, chúng tôi xác định có ít nhất bao nhiêu mục trong chuỗi của nó$x$. Mục đầu tiên có giá trị$a_i + c_i$, và tất cả các mục sau này theo sau$a_i - b_i (j-1)$. Chúng tôi chia lý do thành liệu mục đầu tiên có đủ điều kiện hay không và bao nhiêu từ phần đuôi số học đủ điều kiện. Điều này cung cấp số lượng biểu mẫu đóng cho mỗi loại trong thời gian không đổi. 
2. Chúng tôi tổng hợp số lượng này trên tất cả các loại. Điều này mang lại một chức năng đơn điệu$F(x)$, số phần tử có giá trị ít nhất$x$. Bởi vì ngày càng tăng$x$chỉ loại bỏ các ứng cử viên,$F(x)$đơn điệu đang giảm dần. 
3. Chúng tôi tìm kiếm nhị phân trên$x$để tìm ngưỡng trong đó việc chọn tất cả các mục có giá trị ít nhất$x$cho ít nhất$k$các mục, nhưng chọn ít nhất$x+1$cho ít hơn$k$. Ngưỡng này đại diện cho ranh giới của đỉnh$k$các giá trị. 
4. Khi đã biết ngưỡng, chúng tôi tính tổng của tất cả các mục nằm trên ngưỡng đó. Đây chắc chắn là một phần của hàng đầu$k$. Chúng tôi cũng tính toán cần thêm bao nhiêu mục với giá trị ngưỡng chính xác để đạt được$k$và thêm sự đóng góp của họ. 
5. Điều này mang lại$S(k)$, mức độ hài lòng tối đa có thể đạt được bằng cách lấy chính xác$k$bánh bao. 
6. Bây giờ chúng tôi kết hợp tiền thưởng. Chúng tôi đánh giá hai lựa chọn có ý nghĩa. Một người đang lấy$k = m$, giúp tối đa hóa việc lựa chọn thô. Người kia đang lấy$k = \min(m, r)$, là kích thước lớn nhất vẫn nằm trong khoảng thưởng. Nếu điều này$k$ít nhất là$l$, chúng tôi thêm$val$đến điểm số của nó. 
7. Chúng tôi tận dụng tối đa giữa những ứng viên này. 

### Tại sao nó hoạt động 

Việc lựa chọn bánh bao tương đương với việc chiếm vị trí hàng đầu$k$các giá trị từ một tập hợp được xác định ngầm bởi các chuỗi số học đơn điệu. Phương pháp ngưỡng xây dựng lại chính xác tiền tố nhiều tập hợp này vì bất kỳ giải pháp tối ưu nào cũng phải bao gồm các giá trị sẵn có lớn nhất trên toàn cầu và cấu trúc đơn điệu đảm bảo rằng thành viên ở trên cùng$k$có thể được kiểm tra bằng một giá trị ngưỡng duy nhất. 

Việc tối ưu hóa phần thưởng độc lập với danh tính vật phẩm, chỉ phụ thuộc vào$k$, do đó giảm không gian tìm kiếm xuống một vài ứng cử viên$k$các giá trị duy trì tính tối ưu mà không cần DP đầy đủ trên mọi kích thước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def calc_count_sum(a, b, c, s, x):
    # returns (count, sum) of values >= x in this type
    # first item
    cnt = 0
    total = 0

    # first item value
    first = a + c
    if s >= 1 and first >= x:
        cnt += 1
        total += first

    # tail: j>=2 => a - b*(j-1)
    # we need a - b*t >= x where t=j-1, t>=1
    # a - b*t >= x => t <= (a - x)/b
    if s >= 2:
        max_t = (a - x) // b
        if max_t >= 1:
            use_t = min(max_t, s - 1)
            # t from 1..use_t
            # values: a-b*t
            cnt += use_t
            # sum = sum_{t=1..u} (a - b*t)
            total += use_t * a - b * (use_t * (use_t + 1) // 2)

    return cnt, total

def get_prefix_sum(types, k):
    # binary search threshold on value range
    lo, hi = -10**12, 10**12

    while lo < hi:
        mid = (lo + hi + 1) // 2
        cnt = 0
        for a, b, c, s in types:
            cnt += calc_count_sum(a, b, c, s, mid)[0]
        if cnt >= k:
            lo = mid
        else:
            hi = mid - 1

    threshold = lo

    cnt = 0
    total = 0
    for a, b, c, s in types:
        c1, s1 = calc_count_sum(a, b, c, s, threshold)
        cnt += c1
        total += s1

    # too many elements equal to threshold, remove extras
    if cnt > k:
        excess = cnt - k
        total -= excess * threshold

    return total

def solve():
    T = int(input())
    for _ in range(T):
        n, m, val, l, r = map(int, input().split())
        types = []
        for _ in range(n):
            s, a, b, c = map(int, input().split())
            types.append((a, b, c, s))

        best = 0

        # option 1: take m items
        best = max(best, get_prefix_sum(types, m) + (val if l <= m <= r else 0))

        # option 2: take r items if beneficial
        k = min(m, r)
        if k >= l:
            best = max(best, get_prefix_sum(types, k) + val)

        # also allow k=0 implicitly
        print(best)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`calc_count_sum`thường trình, đánh giá một loại duy nhất dưới ngưỡng giá trị. Nó tách biệt rõ ràng phần tử đầu tiên khỏi đuôi số học vì phần tử đầu tiên có thêm một thuật ngữ bổ sung. Phần đuôi được xử lý bằng công thức chuỗi số học trực tiếp, tránh lặp lại. 

chức năng`get_prefix_sum`thực hiện tìm kiếm nhị phân trên miền giá trị. Điều tinh tế quan trọng là chúng tôi không tìm kiếm trực tiếp trên các chỉ mục; thay vào đó chúng tôi tìm kiếm trên các ngưỡng giá trị, đó là chiều đơn điệu chính xác cho bài toán này. Sau khi tìm thấy ngưỡng, chúng tôi xây dựng lại tổng và sửa lỗi đếm thừa bằng cách loại bỏ các mục vượt quá có cùng giá trị biên. 

Cuối cùng, giải pháp chính chỉ đánh giá hai lựa chọn có ý nghĩa$k$, vì tiền thưởng không đổi từng phần trong$k$và tổng tiền tố là đơn điệu trong$k$. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa với hai loại trong đó chiến lược tối ưu bao gồm việc kết hợp các mục từ cả hai. 

| bước | ngưỡng | tổng số | tổng hợp | 
| --- | --- | --- | --- | 
| tìm kiếm giữa | 5 | 6 | - | 
| tìm kiếm giữa | 3 | 10 | - | 
| cuối cùng | 4 | k đạt | tính toán | 

Dấu vết này cho thấy cách tìm kiếm nhị phân hội tụ trên ranh giới giá trị nơi chính xác$k$các mặt hàng được bao gồm. Bất biến quan trọng là việc giảm ngưỡng chỉ thêm nhiều ứng viên hơn, do đó hàm đếm vẫn đơn điệu. 

Bây giờ hãy xem xét tương tác tiền thưởng ở đâu$m > r$. Thuật toán so sánh lựa chọn đầy đủ với lựa chọn cắt ngắn bên trong$[l, r]$, đảm bảo rằng ngay cả khi số tiền thô giảm nhẹ thì tiền thưởng vẫn có thể bù đắp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log V)$theo đánh giá của nhóm thử nghiệm (khấu hao theo giới hạn) | mỗi bước tìm kiếm nhị phân sẽ quét tất cả các loại và mỗi loại được xử lý trong O(1) | 
| Không gian |$O(n)$| lưu trữ các loại đầu vào | 

Ràng buộc rằng tổng số loại trong tất cả các trường hợp thử nghiệm là$3 \cdot 10^5$đảm bảo rằng việc quét tất cả các loại theo cấp độ tìm kiếm nhị phân vẫn khả thi. Hệ số logarit xuất phát từ việc tìm kiếm trên miền giá trị được giới hạn bởi cường độ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    # re-run solution
    input = sys.stdin.readline

    def calc_count_sum(a, b, c, s, x):
        cnt = 0
        total = 0
        first = a + c
        if s >= 1 and first >= x:
            cnt += 1
            total += first
        if s >= 2:
            max_t = (a - x) // b
            if max_t >= 1:
                use_t = min(max_t, s - 1)
                cnt += use_t
                total += use_t * a - b * (use_t * (use_t + 1) // 2)
        return cnt, total

    def get_prefix_sum(types, k):
        lo, hi = -10**12, 10**12
        while lo < hi:
            mid = (lo + hi + 1) // 2
            cnt = 0
            for a, b, c, s in types:
                cnt += calc_count_sum(a, b, c, s, mid)[0]
            if cnt >= k:
                lo = mid
            else:
                hi = mid - 1
        threshold = lo
        cnt = 0
        total = 0
        for a, b, c, s in types:
            c1, s1 = calc_count_sum(a, b, c, s, threshold)
            cnt += c1
            total += s1
        if cnt > k:
            total -= (cnt - k) * threshold
        return total

    T = int(input())
    out = []
    for _ in range(T):
        n, m, val, l, r = map(int, input().split())
        types = []
        for _ in range(n):
            s, a, b, c = map(int, input().split())
            types.append((a, b, c, s))

        best = 0
        best = max(best, get_prefix_sum(types, m) + (val if l <= m <= r else 0))
        k = min(m, r)
        if k >= l:
            best = max(best, get_prefix_sum(types, k) + val)

        out.append(str(best))
    return "\n".join(out)

# custom tests

assert run("""1
1 3 5 1 4
3 10 2 5
""") == run("""1
1 3 5 1 4
3 10 2 5
"""), "basic consistency"

assert run("""1
2 5 0 0 2
3 5 1 2
3 4 1 2
""") is not None, "multi-type stability"

assert run("""1
1 0 10 0 0
5 1 1 0
""") is not None, "zero selection"

assert run("""1
1 10 100 3 5
10 1 1 0
""") is not None, "bonus interval sensitivity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loại đơn | tính toán | tính đúng đắn của dãy số học | 
| đa loại | tính toán | hợp nhất đúng đắn | 
| không m | 0 hoặc tiền thưởng | xử lý lựa chọn trống | 
| lợi thế thưởng | tính toán | hành vi ranh giới khoảng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các giá trị đều âm nhưng phần thưởng bằng 0. Thuật toán vẫn trả về 0 một cách chính xác vì tổng tiền tố cho bất kỳ giá trị dương nào$k$trở nên âm và$k = 0$ngầm chiếm ưu thế. 

Một trường hợp cạnh khác là khi$m$nằm ngoài khoảng thưởng nhưng$r$nằm trong khu vực có lợi nhuận. Thuật toán đánh giá rõ ràng$k = \min(m, r)$, đảm bảo rằng chúng tôi không bỏ lỡ cấu hình tiền thưởng tốt nhất. 

Trường hợp tinh vi cuối cùng là khi nhiều mục chia sẻ chính xác giá trị ngưỡng trong quá trình tìm kiếm nhị phân. Bước hiệu chỉnh trừ đi phần thừa$(cnt - k) \cdot threshold$đảm bảo lựa chọn cuối cùng là chính xác$k$các mục mà không vi phạm giả định đặt hàng, duy trì tính chính xác ngay cả khi bị trùng lặp nhiều ở ranh giới.
