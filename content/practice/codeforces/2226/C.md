---
title: "CF 2226C - Tượng đài tinh thần (Phiên bản dễ)"
description: "Mỗi phần tử mảng có thể được sửa đổi độc lập. Với giá trị $x$, chúng ta có thể chọn bất kỳ số nguyên dương $b$ nào, sau đó thay thế $x$ bằng $x bmod b$. Mục tiêu là tối đa hóa MEX của mảng kết quả."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2226
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1095 (Div. 2)"
rating: 0
weight: 2226
solve_time_s: 377
verified: false
draft: false
---

[CF 2226C - Tinh thần hoành tráng (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/2226/C) 

**Đánh giá:** - 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, toán học, hai con trỏ 
**Thời gian giải:** 6 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi phần tử mảng có thể được sửa đổi độc lập. Đối với một giá trị$x$, chúng ta có thể chọn bất kỳ số nguyên dương nào$b$, sau đó thay thế$x$qua$x \bmod b$. 

Mục tiêu là tối đa hóa MEX của mảng kết quả. Vì thao tác được áp dụng độc lập cho mọi vị trí nên bài toán trở thành: 

Với tập hợp các giá trị mà mỗi phần tử có thể được chuyển đổi thành, MEX lớn nhất có thể được xây dựng bằng cách gán một giá trị đích cho mỗi phần tử là bao nhiêu? 

Các ràng buộc là manh mối đầu tiên cho thấy việc xây dựng trực tiếp không được dự định. Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$2\cdot 10^5$, trong khi tổng giá trị lớn nhất là nhiều nhất$10^6$. Điều này gợi ý rõ ràng về một thuật toán có độ phức tạp phụ thuộc vào phạm vi giá trị hơn là vào$n^2$. 

Khó khăn chính là hiểu chính xác giá trị nào của một số$x$có thể trở thành sau một thao tác modulo. 

Hãy xem xét một vài ví dụ. 

Nếu như$x=6$, sau đó:$$6\bmod 7=6,$$Vì thế$6$có thể truy cập được. 

Cũng,$$6\bmod 6=0,\quad 6\bmod 5=1,\quad 6\bmod 4=2.$$Các giá trị có thể đạt được là$$\{0,1,2,6\}.$$Một giả định ngây thơ sẽ là mọi giá trị giữa$0$Và$x$có thể truy cập được. Điều đó là sai. Ví dụ,$3$không thể được sản xuất từ$6$. 

Một sai lầm dễ mắc phải khác là nghĩ rằng các giá trị lớn trùng lặp luôn có thể tạo ra các số bị thiếu tùy ý. Ví dụ:$$a=[2,2].$$Mỗi$2$chỉ có thể trở thành$0$hoặc$2$. Chúng ta có thể tạo ra$0$, nhưng chúng ta không bao giờ có thể tạo ra$1$. Câu trả lời là$1$, không$2$. 

Trường hợp tinh tế thứ ba là sự hiện diện của số không. 

Vì$$a=[0,0,0],$$mọi phần tử luôn luôn còn lại$0$, bởi vì$$0\bmod b = 0.$$Mảng kết quả không bao giờ có thể chứa$1$, vậy câu trả lời là$1$. 

Những ví dụ này cho thấy rằng chúng ta phải mô tả chính xác các giá trị có thể tiếp cận. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là tính toán tất cả các giá trị có thể tiếp cận cho mọi phần tử, sau đó tìm kiếm MEX lớn nhất có thể đạt được bằng cách so khớp giữa các phần tử và giá trị đích. 

Ngay cả sau khi quan sát thấy rằng mỗi giá trị chỉ có$O(x)$ở các trạng thái có thể tiếp cận được thì công thức đối sánh lưỡng cực thu được là quá đắt. Với giá trị lên đến$10^6$, cách tiếp cận này là hoàn toàn không thực tế. 

Quan sát quan trọng đến từ việc phân tích một con số duy nhất$x$. 

Giả sử chúng ta thay đổi mô đun$b$. 

Nếu như$b>x$, sau đó$$x\bmod b=x.$$Nếu như$b\le x$thì số dư luôn nhỏ hơn$x/2$. Bài xã luận chính thức chứng minh điều này bằng cách chia thành các trường hợp$b\le x/2$Và$x/2<b\le x$. 

Hơn nữa, mọi giá trị$$0\le y<\frac{x}{2}$$có thể đạt được bằng cách chọn$$b=x-y.$$Do đó, bộ có thể truy cập hoàn chỉnh là$$R(x)=\{x\}\cup \left\{ 0,1,\dots,\left\lfloor\frac{x-1}{2}\right\rfloor \right\}.$$Đặc tính này thay đổi hoàn toàn vấn đề. 

Bây giờ hãy xem xét việc kiểm tra xem$$\operatorname{mex}\ge k$$là có thể. 

Chúng ta cần nhận ra mọi con số$$0,1,\dots,k-1.$$Một giá trị$x<k$là đặc biệt. Từ$x$bản thân nó có thể truy cập được, sử dụng$x$nhận ra$x$ít nhất luôn tốt bằng việc sử dụng nó cho số lượng nhỏ hơn. Bất kỳ mục tiêu nhỏ hơn nào cũng có thể được tạo ra bởi một số giá trị đủ lớn sau này. 

Vì vậy, trong khi kiểm tra$k$, chúng ta tham lam dự trữ một lần xuất hiện của mọi giá trị$x<k$đại diện cho$x$. 

Sau khi làm điều này, một số con số trong$[0,k-1]$vẫn chưa được khám phá. Hãy để những số còn thiếu đó là$$m_1<m_2<\cdots.$$Mỗi phần tử còn lại$x$có thể bao phủ chính xác khoảng$$0 \le y \le \left\lfloor\frac{x-1}{2}\right\rfloor.$$Tương tự, nó có thể bao gồm một giá trị còn thiếu$m$nếu$$x\ge 2m+1.$$Bây giờ bài toán trở thành bài toán so khớp tham lam cổ điển. 

Sắp xếp các số còn thiếu ngày càng nhiều. Sắp xếp các phần tử lớn có sẵn ngày càng nhiều. Xử lý các số bị thiếu từ nhỏ nhất đến lớn nhất và luôn gán phần tử nhỏ nhất có sẵn có thể bao gồm số còn thiếu hiện tại. 

Nếu tại một thời điểm nào đó không có phần tử nào như vậy tồn tại thì$\operatorname{mex}\ge k$là không thể. 

Vì tính khả thi là đơn điệu trong$k$, việc tìm kiếm nhị phân trên câu trả lời sẽ trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu | Theo cấp số nhân hoặc tệ hơn | Lớn | Quá chậm | 
| Tìm kiếm nhị phân + Kiểm tra tham lam |$O(M\log^2 M)$|$O(M)$| Đã chấp nhận | 

Đây$M=\max(a)$, và bài toán đảm bảo rằng tổng của tất cả$M$giá trị nhiều nhất là$10^6$. 

## Hướng dẫn thuật toán 

### Giá trị có thể tiếp cận 

Với mọi giá trị$x$,$$R(x)= \{x\} \cup \{0,1,\dots,\lfloor (x-1)/2\rfloor\}.$$Đây là thực tế cấu trúc cơ bản. 

### Kiểm tra tính khả thi của bản sửa lỗi$k$1. Đếm tần số của mọi giá trị. 
2. Đối với mọi$x<k$, nếu một lần xuất hiện không được sử dụng của$x$tồn tại, dành chính xác một lần xuất hiện để thể hiện$x$. 
3. Thu thập tất cả các số trong$[0,k-1]$đó không được bảo lưu. Đây là những giá trị còn thiếu. 
4. Mọi phần tử mảng còn lại sẽ trở thành tài nguyên có sẵn. 
5. Đối với mỗi phần tử có sẵn$x$, tính toán$$t=\left\lfloor\frac{x-1}{2}\right\rfloor.$$Phần tử này có thể bao gồm bất kỳ giá trị còn thiếu nào không vượt quá$t$. 

1. Xử lý các giá trị còn thiếu từ nhỏ nhất đến lớn nhất. Duy trì nhiều bộ công suất sẵn có$t$. 
2. Đối với giá trị còn thiếu$m$, chọn công suất nhỏ nhất thỏa mãn$$t\ge m.$$Loại bỏ nó khỏi multiset. 

1. Nếu dung lượng đó không tồn tại thì việc kiểm tra không thành công. 
2. Nếu mọi giá trị còn thiếu đều được chỉ định thì việc kiểm tra sẽ thành công. 

### Tìm kiếm nhị phân 

Câu trả lời nằm giữa$0$Và$n$, bởi vì một mảng có độ dài$n$không thể có MEX lớn hơn$n$. 

Tìm kiếm nhị phân lớn nhất$k$mà việc kiểm tra tính khả thi thành công. 

### Tại sao nó hoạt động 

Bất cứ khi nào một giá trị$x<k$tồn tại, gán một bản sao trực tiếp cho$x$không bao giờ có thể làm giảm tính khả thi. Bất kỳ việc sử dụng thay thế nào của bản sao đó sẽ tạo ra một giá trị nhỏ hơn$x$, trong khi$x$bản thân nó chỉ có thể được tạo ra bởi một bản sao khác của$x$. Đặt trước những bản sao như vậy ngay lập tức luôn là tối ưu. 

Sau những sự đặt trước này, mọi phần tử còn lại chỉ đóng góp thông qua ngưỡng của nó$$\left\lfloor\frac{x-1}{2}\right\rfloor.$$Vấn đề dẫn đến là khớp các số bị thiếu với ngưỡng. Xử lý các số còn thiếu theo thứ tự tăng dần và luôn sử dụng ngưỡng khả thi nhỏ nhất là chiến lược tham lam tối ưu tiêu chuẩn. Bất kỳ ngưỡng lớn hơn nào được sử dụng trước đó chỉ có thể làm giảm tính linh hoạt trong tương lai. 

Do đó việc kiểm tra là chính xác và tìm kiếm nhị phân trên vị từ khả thi đơn điệu mang lại MEX tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
from bisect import bisect_left
from collections import Counter
import heapq

input = sys.stdin.readline

def can_make(a, k):
    cnt = Counter(a)

    missing = []

    for x in range(k):
        if cnt[x] > 0:
            cnt[x] -= 1
        else:
            missing.append(x)

    caps = []

    for x, c in cnt.items():
        if c <= 0:
            continue

        if x == 0:
            cap = -1
        else:
            cap = (x - 1) // 2

        for _ in range(c):
            caps.append(cap)

    caps.sort()

    ptr = 0
    m = len(caps)

    for need in missing:
        pos = bisect_left(caps, need, ptr)
        if pos == m:
            return False
        ptr = pos + 1

    return True

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        lo, hi = 0, n

        while lo < hi:
            mid = (lo + hi + 1) // 2

            if can_make(a, mid):
                lo = mid
            else:
                hi = mid - 1

        ans.append(str(lo))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Bảng tần số cho phép chúng ta dự trữ các bản sao của các giá trị nhỏ hơn MEX ứng cử viên. Mọi phần tử còn lại được chuyển đổi thành một số duy nhất,$$\left\lfloor\frac{x-1}{2}\right\rfloor,$$mô tả đầy đủ tính hữu ích của nó trong việc điền các giá trị còn thiếu. 

Việc so khớp tham lam theo công suất được sắp xếp được triển khai bằng tìm kiếm nhị phân. Con trỏ đảm bảo rằng mỗi dung lượng được sử dụng tối đa một lần. 

Phạm vi tìm kiếm nhị phân là$[0,n]$, bởi vì không có mảng có độ dài$n$có thể chứa tất cả các giá trị$0,\dots,n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$$[6,7]$$Kiểm tra$k=2$: 

Các giá trị cần thiết là$0$Và$1$. 

| Giá trị | Kín đáo? | 
| --- | --- | 
| 0 | Không | 
| 1 | Không | 

Thiếu giá trị:$$[0,1]$$Công suất sẵn có: 

| Yếu tố | Công suất | 
| --- | --- | 
| 6 | 2 | 
| 7 | 3 | 

Phù hợp: 

| Thiếu | Công suất được lựa chọn | 
| --- | --- | 
| 0 | 2 | 
| 1 | 3 | 

Tất cả các giá trị còn thiếu đều được che đậy, vì vậy$k=2$là khả thi. 

Từ$n=2$, câu trả lời là$$\boxed{2}.$$### Ví dụ 2 

đầu vào:$$[8,1,7,6,4,3]$$Kiểm tra$k=5$: 

Bản sao được bảo lưu: 

| Giá trị | Dành riêng | 
| --- | --- | 
| 0 | Không | 
| 1 | Có | 
| 2 | Không | 
| 3 | Có | 
| 4 | Có | 

Thiếu giá trị:$$[0,2]$$Công suất còn lại: 

| Yếu tố | Công suất | 
| --- | --- | 
| 8 | 3 | 
| 7 | 3 | 
| 6 | 2 | 

Phù hợp: 

| Thiếu | Công suất được lựa chọn | 
| --- | --- | 
| 0 | 2 | 
| 2 | 3 | 

Khả thi. 

Kiểm tra$k=6$thất bại vì không có cách nào để tạo ra tất cả các giá trị$0,1,2,3,4,5$đồng thời. 

Kể từ đây$$\boxed{5}.$$## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M\log^2 n)$mỗi bài kiểm tra thực tế,$O(n\log n\log n)$trường hợp xấu nhất | Tìm kiếm nhị phân và kiểm tra tham lam | 
| Không gian |$O(n)$| Tần số và dung lượng lưu trữ | 

Bởi vì tổng của tất cả các độ dài mảng nhiều nhất là$2\cdot 10^5$và tổng giá trị tối đa trong tất cả các thử nghiệm nhiều nhất là$10^6$, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    from bisect import bisect_left
    from collections import Counter

    data = iter(inp.strip().split())

    t = int(next(data))
    out = []

    def can_make(a, k):
        cnt = Counter(a)

        missing = []

        for x in range(k):
            if cnt[x]:
                cnt[x] -= 1
            else:
                missing.append(x)

        caps = []

        for x, c in cnt.items():
            cap = -1 if x == 0 else (x - 1) // 2
            caps.extend([cap] * c)

        caps.sort()

        ptr = 0

        for need in missing:
            pos = bisect_left(caps, need, ptr)
            if pos == len(caps):
                return False
            ptr = pos + 1

        return True

    for _ in range(t):
        n = int(next(data))
        a = [int(next(data)) for _ in range(n)]

        lo, hi = 0, n

        while lo < hi:
            mid = (lo + hi + 1) // 2
            if can_make(a, mid):
                lo = mid
            else:
                hi = mid - 1

        out.append(str(lo))

    return "\n".join(out)

assert run("1\n4\n0 1 2 3\n") == "4"
assert run("1\n2\n6 7\n") == "2"

assert run("1\n1\n0\n") == "1"
assert run("1\n1\n5\n") == "1"
assert run("1\n2\n2 2\n") == "1"
assert run("1\n3\n0 0 0\n") == "1"
assert run("1\n5\n1000000 1000000 1000000 1000000 1000000\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[0]`|`1`| Mảng nhỏ nhất có thể | 
|`[5]`|`1`| Giá trị lớn chỉ có thể tạo một số bắt buộc | 
|`[2,2]`|`1`| Các giá trị trùng lặp không phải lúc nào cũng tạo ra các số nguyên liên tiếp | 
|`[0,0,0]`|`1`| Zero không có công suất hữu ích | 
| Năm bản sao của`10^6`|`5`| Hành vi khớp giá trị lớn | 

## Vỏ cạnh 

Hãy xem xét$$[2,2].$$Tập hợp có thể truy cập của mỗi phần tử là$$\{0,2\}.$$Thuật toán cố gắng$k=2$. Giá trị$1$bị thiếu. Công suất còn lại là$$\left\lfloor\frac{2-1}{2}\right\rfloor=0,$$không thể che đậy$1$. Việc kiểm tra thất bại, tạo ra câu trả lời$1$. 

Coi như$$[0,0,0].$$Đối với mỗi phần tử,$$0\bmod b=0.$$Mỗi năng lực trở thành$-1$, nghĩa là nó không thể tạo ra bất kỳ giá trị dương nào còn thiếu. Thuật toán tìm thấy rằng$1$không thể che được nên câu trả lời là$1$. 

Coi như$$[6,7].$$Công suất là$2$Và$3$. Thiếu giá trị$0$Và$1$cả hai đều được che phủ. Thuật toán trả về chính xác$2$, chứng tỏ rằng các giá trị lớn có thể được chuyển đổi thành các số nhỏ ngay cả khi những số đó ban đầu không xuất hiện trong mảng.
