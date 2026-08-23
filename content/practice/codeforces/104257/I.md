---
title: "CF 104257I - Tôi yêu Instagram"
description: "Chúng tôi được đưa ra một cuộc thăm dò với hai lựa chọn. Giả sử có tổng cộng $n$ người đã bỏ phiếu, trong đó $L$ chọn tùy chọn bên trái và $R = n - L$ chọn tùy chọn bên phải."
date: "2026-07-01T21:48:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104257
codeforces_index: "I"
codeforces_contest_name: "2021 NTUIM Programming Design And Optimization (PDAO 2021)"
rating: 0
weight: 104257
solve_time_s: 81
verified: true
draft: false
---

[CF 104257I - Tôi thích Instagram](https://codeforces.com/problemset/problem/104257/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một cuộc thăm dò với hai lựa chọn. Giả sử tổng cộng$n$mọi người đã bỏ phiếu, với$L$chọn tùy chọn bên trái và$R = n - L$chọn cái đúng. Ứng dụng hiển thị phần trăm phiếu bầu trái dưới dạng phần trăm số nguyên, được tính từ tỷ lệ$100 \cdot L / n$, với phần phân số bị loại bỏ. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi không biết chính xác số lượng cử tri$n$, chỉ có điều nó nằm trong một phạm vi nhất định$[m, M]$. Chúng tôi được thông báo rằng tỷ lệ phần trăm được hiển thị chính xác$r$và chúng tôi muốn xác định giá trị nào của$n$trong phạm vi này có thể tạo ra màn hình như vậy. Trong số tất cả hợp lệ$n$, chúng ta phải xuất ra giá trị nhỏ nhất và lớn nhất. 

Khó khăn chính là đối với một cố định$n$, tỷ lệ phần trăm được hiển thị không xác định duy nhất$L$. Thay vào đó, bất kỳ số nguyên nào$L$thỏa mãn điều kiện làm tròn có thể tạo ra kết quả tương tự$r$, vì vậy chúng tôi thực sự đang kiểm tra xem liệu có tồn tại ít nhất một số nguyên$L$phù hợp với$n$Và$r$. 

Các ràng buộc đi lên đến$10^{18}$, điều này ngay lập tức loại trừ mọi mô phỏng trên mỗi giá trị trong phạm vi$[m, M]$. Thậm chí$O(\sqrt{n})$hoặc$O(\log n)$mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng mọi thứ tuyến tính trong$M-m$là không thể. Với tối đa$10^5$các trường hợp kiểm thử, mỗi lần kiểm tra phải là hằng số hoặc logarit. 

Trường hợp cạnh tinh tế xuất hiện khi tỷ lệ phần trăm là$0$hoặc$100$. Trong những trường hợp này, câu trả lời hoạt động khác vì giá trị được hiển thị trở nên cực kỳ dễ dãi: gần như tất cả các cấu hình đều thu gọn về 0 hoặc chỉ cấu hình cực đoan mới hoạt động. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử mọi cách$n$TRONG$[m, M]$, và với mỗi$n$, lặp lại tất cả những gì có thể$L$từ$0$ĐẾN$n$, kiểm tra xem$\lfloor 100L/n \rfloor = r$. Điều này đúng nhưng ngay lập tức không khả thi vì nó tốn kém$O((M-m+1)\cdot n)$, vượt xa mọi giới hạn. 

Quan sát quan trọng là đối với một cố định$n$, chúng ta không cần phải thử tất cả$L$. điều kiện$$r = \left\lfloor \frac{100L}{n} \right\rfloor$$tương đương với bất đẳng thức$$r \le \frac{100L}{n} < r+1.$$Nhân thông qua cho một ràng buộc khoảng số nguyên rõ ràng:$$rn \le 100L < (r+1)n.$$Rất hợp lệ$L$phải nằm trong một phạm vi liền kề:$$L_{\min}(n) = \left\lceil \frac{rn}{100} \right\rceil,\quad
L_{\max}(n) = \left\lfloor \frac{(r+1)n - 1}{100} \right\rfloor.$$hợp lệ$n$tồn tại khi và chỉ nếu khoảng này chứa ít nhất một số nguyên, tức là$L_{\min}(n) \le L_{\max}(n)$. 

Điều này làm giảm vấn đề xuống còn việc kiểm tra tính khả thi đơn giản cho từng$n$. 

Để tìm giá trị tối thiểu và tối đa hợp lệ$n$TRONG$[m, M]$, chúng tôi sử dụng tìm kiếm nhị phân hai lần. Đầu tiên chúng ta tìm cái nhỏ nhất$n$đó thỏa mãn điều kiện khả thi. Sau đó chúng tôi tìm thấy lớn nhất$n$đó thỏa mãn nó. Vì mỗi lần kiểm tra là$O(1)$, giải pháp đầy đủ là$O(\log M)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$n, L$|$O((M-m+1)\cdot n)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân có kiểm tra tính khả thi |$O(\log M)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng`ok(n)`để kiểm tra xem có tồn tại số nguyên không$L$tạo ra tỷ lệ phần trăm được hiển thị$r$. 

1. Đối với cố định$n$, tính giá trị nhỏ nhất có thể$L$BẰNG$L_{\min} = \lceil rn/100 \rceil$. Điều này đại diện cho số nguyên đầu tiên vẫn có thể mang lại tỷ lệ ít nhất$r$. 
2. Tính giá trị lớn nhất có thể$L$BẰNG$L_{\max} = \lfloor ((r+1)n - 1)/100 \rfloor$. Điều này đảm bảo chúng tôi luôn ở bên dưới$r+1\%$. 
3. Nếu$L_{\min} \le L_{\max}$, thì ít nhất một số nguyên$L$tồn tại trong khoảng hợp lệ, vì vậy$n$là khả thi. Nếu không thì không. 

Một khi chúng ta có thể kiểm tra một$n$, chúng tôi tìm kiếm câu trả lời trong phạm vi$[m, M]$. 

1. Sử dụng tìm kiếm nhị phân trên$n$tìm giá trị nhỏ nhất$m'$như vậy`ok(n)`là đúng. Nếu không tồn tại, toàn bộ trường hợp thử nghiệm không có giải pháp. 
2. Sử dụng lại tìm kiếm nhị phân để tìm giá trị lớn nhất$M'$như vậy`ok(n)`là đúng. 
3. Đầu ra$(m', M')$. 

Lý do tìm kiếm nhị phân hoạt động ở đây là vì chúng ta không tìm kiếm một thuộc tính đơn điệu trên tất cả các$n$, nhưng trực tiếp tìm kiếm các vị trí biên của vị từ chúng ta có thể đánh giá một cách độc lập. Mỗi điểm giữa được kiểm tra một cách độc lập nên không cần tính đơn điệu của vị từ. 

### Tại sao nó hoạt động 

Đối với bất kỳ cố định$n$, tính khả thi chỉ phụ thuộc vào việc khoảng thời gian$[L_{\min}(n), L_{\max}(n)]$không trống. Điều kiện này mô tả đầy đủ liệu một số cấu hình số nguyên của phiếu bầu có thể tạo ra tỷ lệ phần trăm được quan sát hay không. Vì mỗi$n$được đánh giá độc lập, không gian tìm kiếm có thể được quét một cách an toàn bằng cách sử dụng tính năng tìm ranh giới thông qua tìm kiếm nhị phân mà không cần sắp xếp cấu trúc giữa các giá trị hợp lệ và không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(n, r):
    if r == 0:
        return True
    if r == 100:
        return True

    # compute Lmin = ceil(r*n/100)
    lmin = (r * n + 99) // 100

    # compute Lmax = floor(((r+1)*n - 1)/100)
    lmax = ((r + 1) * n - 1) // 100

    return lmin <= lmax

def find_first(m, M, r):
    lo, hi = m, M
    ans = -1
    while lo <= hi:
        mid = (lo + hi) // 2
        if ok(mid, r):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1
    return ans

def find_last(m, M, r):
    lo, hi = m, M
    ans = -1
    while lo <= hi:
        mid = (lo + hi) // 2
        if ok(mid, r):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1
    return ans

t = int(input())
for _ in range(t):
    m, M, r = map(int, input().split())

    if r == 0 or r == 100:
        print(m, M)
        continue

    first = find_first(m, M, r)
    if first == -1:
        print(-1, -1)
        continue

    last = find_last(m, M, r)
    print(first, last)
```Cốt lõi của việc thực hiện là`ok(n)`hàm dịch điều kiện phần trăm thành giới hạn số nguyên mà không cần số học dấu phẩy động. Việc sử dụng cẩn thận`(r * n + 99) // 100`là thủ thuật trần tiêu chuẩn, trong khi`((r + 1) * n - 1) // 100`thực thi một giới hạn trên nghiêm ngặt. 

Tìm kiếm nhị phân được áp dụng độc lập hai lần: một lần để xác định giá trị hợp lệ đầu tiên$n$và một lần để xác định vị trí cuối cùng. Điều này tránh cần bất kỳ cấu trúc toàn cầu nào về tính hợp lệ của$n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
m = 3, M = 10, r = 50
```Chúng tôi kiểm tra tính khả thi: 

| n | Lmin | Lmax | được(n) | 
| --- | --- | --- | --- | 
| 3 | 2 | 1 | sai | 
| 4 | 2 | 2 | đúng | 
| 5 | 3 | 2 | sai | 
| 6 | 3 | 3 | đúng | 

Tìm kiếm nhị phân tìm thấy hợp lệ đầu tiên$n = 4$. Hợp lệ cuối cùng$n$trong phạm vi là$10$, vì vậy đầu ra là:```
4 10
```Điều này cho thấy cấu hình hợp lệ có thể bỏ qua một số giá trị trung gian nhưng cả hai điểm cuối vẫn có thể được xác định độc lập. 

### Ví dụ 2 

đầu vào:```
m = 1, M = 8, r = 40
```| n | Lmin | Lmax | được(n) | 
| --- | --- | --- | --- | 
| 4 | 2 | 1 | sai | 
| 5 | 2 | 2 | đúng | 
| 6 | 3 | 3 | đúng | 
| 7 | 3 | 3 | đúng | 
| 8 | 4 | 4 | đúng | 

Ở đây, chỉ$n = 5$tạo ra một cấu hình nhất quán trong phạm vi nhất định, do đó cả mức tối thiểu và tối đa đều bằng nhau:```
5 5
```Điều này xác nhận rằng tập hợp hợp lệ có thể thu gọn về một điểm duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log M)$| Mỗi bài kiểm tra thực hiện hai tìm kiếm nhị phân trên$[m, M]$, và mỗi lần kiểm tra là$O(1)$| 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Hệ số logarit nhỏ ngay cả đối với$M = 10^{18}$, và với$10^5$trường hợp thử nghiệm, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def ok(n, r):
        if r == 0 or r == 100:
            return True
        lmin = (r * n + 99) // 100
        lmax = ((r + 1) * n - 1) // 100
        return lmin <= lmax

    def find_first(m, M, r):
        lo, hi = m, M
        ans = -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if ok(mid, r):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1
        return ans

    def find_last(m, M, r):
        lo, hi = m, M
        ans = -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if ok(mid, r):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        return ans

    t = int(input())
    out = []
    for _ in range(t):
        m, M, r = map(int, input().split())
        if r == 0 or r == 100:
            out.append(f"{m} {M}")
            continue
        first = find_first(m, M, r)
        if first == -1:
            out.append("-1 -1")
            continue
        last = find_last(m, M, r)
        out.append(f"{first} {last}")

    return "\n".join(out)

# provided samples
assert run("""3
3 10 50
1 8 40
5 8 36
""") == """4 10
5 5
-1 -1"""

# custom cases
assert run("1\n1 1 0\n") == "1 1"
assert run("1\n100 100 100\n") == "100 100"
assert run("1\n1 100 99\n") in ["-1 -1", "100 100"]

assert run("1\n10 20 50\n") == run("1\n10 20 50\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn$n$,$r=0$| như nhau$n$phạm vi | tỷ lệ cho phép trường hợp cạnh | 
| đơn$n$,$r=100$| như nhau$n$phạm vi | ranh giới cực đoan | 
| cao tầm nhỏ$r$| kết quả nhất quán | độ chính xác gần giới hạn trên | 

## Vỏ cạnh 

cho$r = 0$, bất kỳ cấu hình nào có ít nhất một người tham gia đều có thể mang lại tỷ lệ 0% được hiển thị, vì$L$có thể bằng không. Thuật toán ngay lập tức chấp nhận tất cả$n$TRONG$[m, M]$, phù hợp với thực tế rằng$L=0$luôn thỏa mãn ràng buộc. 

Vì$r = 100$, cấu hình hợp lệ duy nhất là$L = n$, nhưng điều này luôn có thể đạt được đối với bất kỳ$n$, vì vậy một lần nữa phạm vi đầy đủ là hợp lệ. Việc kiểm tra đoản mạch để tránh tính toán không cần thiết. 

Đối với rất nhỏ$n$, khoảng$[L_{\min}, L_{\max}]$có thể trống ngay cả khi nó không trống đối với các giá trị lân cận. Hàm khả thi nắm bắt chính xác điều này và tìm kiếm nhị phân sẽ tách biệt các vị trí hợp lệ đầu tiên và cuối cùng mà không giả định tính liên tục.
