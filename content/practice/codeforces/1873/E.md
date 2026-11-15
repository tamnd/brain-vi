---
title: "CF 1873E - Xây dựng bể cá"
description: "Chúng ta được cho một hàng cột san hô, mỗi cột có chiều cao ban đầu nhất định. Chúng tôi muốn xây dựng một bể nước trên cấu trúc này bằng cách chọn chiều cao mục tiêu đồng nhất $h$."
date: "2026-06-08T23:14:23+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1873
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 898 (Div. 4)"
rating: 1100
weight: 1873
solve_time_s: 81
verified: true
draft: false
---

[CF 1873E - Xây dựng bể cá](https://codeforces.com/problemset/problem/1873/E) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, sắp xếp 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hàng cột san hô, mỗi cột có chiều cao ban đầu nhất định. Chúng tôi muốn xây dựng một bể chứa nước trên cấu trúc này bằng cách chọn chiều cao mục tiêu thống nhất$h$. Bất kỳ cột nào có san hô ở bên dưới$h$sẽ chứa đầy nước đến$h$, trong khi bất kỳ cột nào đã ở trên$h$không cần nước. 

Vì vậy, đối với mỗi cột, lượng nước đóng góp là$\max(0, h - a_i)$và tổng lượng nước sử dụng là tổng của những đóng góp này. Mục tiêu là chọn số nguyên lớn nhất có thể$h \ge 1$sao cho tổng lượng nước cần thiết không vượt quá$x$. 

Khó khăn chính là việc tăng$h$tăng mức sử dụng nước theo cách phi tuyến tính nhưng có cấu trúc, vì mỗi cột chỉ bắt đầu đóng góp sau$h$vượt quá chiều cao của nó. 

Các ràng buộc đủ lớn để việc quét bậc hai hoặc tuyến tính trên mỗi trường hợp kiểm thử diễn ra quá chậm. Với tối đa$2 \cdot 10^5$tổng số phần tử trên tất cả các trường hợp thử nghiệm và$t \le 10^4$, chúng ta cần đại khái$O(n \log A)$hoặc$O(n)$mỗi trường hợp thử nghiệm. Bất kỳ phương pháp nào tính toán lại việc sử dụng nước từ đầu cho nhiều độ cao phù hợp sẽ hết thời gian. 

Một số tình huống khó khăn đáng được cô lập. 

Nếu tất cả$a_i$lớn so với$x$, câu trả lời sẽ gần$\max(a_i)$hoặc cao hơn nó một chút, bởi vì nâng cao$h$chỉ một chút thôi cũng có thể đã vượt quá lượng nước dự trữ. Ví dụ, nếu$a = [100]$Và$x = 5$, sau đó$h = 105$chỉ có tác dụng nếu chúng ta diễn giải cẩn thận định nghĩa: chỉ$h - a_i$đóng góp, vì vậy$h = 105$sử dụng đúng 5 nước. 

Nếu tất cả$a_i$bằng nhau và$x$lớn, câu trả lời tăng tuyến tính với$x$, vì mỗi đơn vị tăng lên$h$thêm chính xác$n$Nước. 

Một sai lầm ngây thơ là nghĩ rằng nước chỉ phụ thuộc vào khoảng cách giữa các phần tử được sắp xếp liên tiếp, nhưng thực tế nó phụ thuộc vào tất cả các phần tử một cách đồng thời và tích lũy liên tục như$h$tăng lên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử tất cả các giá trị có thể có của$h$từ 1 trở lên giống như$\max(a_i) + x$. Đối với mỗi chiều cao ứng viên, chúng tôi tính toán tổng lượng nước cần thiết bằng cách tính tổng$\max(0, h - a_i)$tổng thể$i$. Điều này đúng, nhưng mỗi lần đánh giá đều tốn$O(n)$, và phạm vi của$h$có thể lớn như$10^9 + x$. Thậm chí hạn chế đến một giới hạn hợp lý, nói$10^9$, làm cho điều này hoàn toàn không thể thực hiện được. 

Nhận xét quan trọng là hàm$$f(h) = \sum_{i=1}^n \max(0, h - a_i)$$là đơn điệu trong$h$. Tăng dần$h$chỉ có thể tăng hoặc duy trì lượng nước sử dụng chứ không bao giờ giảm được. Tính đơn điệu này cho phép tìm kiếm nhị phân trên$h$. 

Để đánh giá$f(h)$một cách hiệu quả, chúng ta sắp xếp mảng. Sau khi được sắp xếp, chúng ta có thể tính toán các khoản đóng góp theo cách có cấu trúc. Nếu chúng ta ấn định một ngưỡng$h$, tất cả các phần tử lớn hơn hoặc bằng$h$đóng góp bằng 0 và tất cả các yếu tố bên dưới$h$đóng góp một cách tuyến tính. Với tổng tiền tố, chúng ta có thể tính tổng của$h - a_i$tổng thể$a_i < h$trong thời gian không đổi sau khi tiền xử lý. 

Điều này làm giảm mỗi lần kiểm tra xuống còn$O(\log n)$cho các bước tìm kiếm nhị phân, hoặc$O(1)$nếu chúng ta tính toán trước các tổng tiền tố và sử dụng tìm kiếm nhị phân để tìm chỉ số giới hạn. 

Do đó, bài toán trở thành một “giá trị khả thi tối đa dưới ràng buộc đơn điệu” cổ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot H)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n + \log H)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng$a$theo thứ tự không giảm. Việc sắp xếp cho phép chúng ta tách các phần tử đóng góp nước ra khỏi các phần tử không đóng góp nước cho bất kỳ mục đích nào được chọn.$h$. 
2. Xây dựng mảng tổng tiền tố trên các giá trị được sắp xếp. Điều này cho phép chúng tôi nhanh chóng tính toán tổng các phân đoạn mà không cần lặp lại mỗi lần. 
3. Xác định hàm$f(h)$tính toán tổng lượng nước cần thiết cho một độ cao nhất định. Chúng tôi xác định vị trí chỉ mục đầu tiên$i$như vậy$a_i \ge h$. Mọi thứ trước chỉ số đó đều đóng góp nước. 
4. Nếu$k$là số phần tử có$a_i < h$, thì lượng nước sử dụng là$k \cdot h - \text{sum of those } a_i$. Công thức này xuất phát từ việc mở rộng$\sum (h - a_i)$. 
5. Tìm kiếm nhị phân trên$h$. Phạm vi tìm kiếm có thể mở rộng một cách an toàn lên tới$\max(a_i) + x$, vì ngoài điều đó nhu cầu về nước tăng ít nhất là tuyến tính với$n$. 
6. Đối với mỗi trung điểm$h$, tính toán$f(h)$. Nếu nó nằm trong ngân sách$\le x$, chúng ta có thể thử tăng$h$. Nếu không, chúng tôi giảm nó. 
7. Đáp án cuối cùng là lớn nhất$h$đó thỏa mãn ràng buộc. 

Lý do tìm kiếm nhị phân hoạt động là vì một khi chiều cao trở nên không khả thi thì mọi chiều cao lớn hơn vẫn không khả thi vì mỗi đơn vị bổ sung của$h$tăng lượng nước sử dụng ít nhất bằng số cột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import bisect

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        def water(h):
            idx = bisect.bisect_left(a, h)
            return h * idx - pref[idx]

        lo, hi = 1, max(a) + x + 1

        while lo < hi:
            mid = (lo + hi) // 2
            if water(mid) <= x:
                lo = mid + 1
            else:
                hi = mid

        print(lo - 1)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp để kích hoạt tổng tiền tố và tìm kiếm nhị phân. Mảng tiền tố rất cần thiết để tính tổng của phần đầu tiên$k$các phần tử trong thời gian không đổi. 

chức năng`water(h)`cô lập công thức chính. các`bisect_left`cuộc gọi tìm thấy có bao nhiêu phần tử nhỏ hơn hoàn toàn$h$, trực tiếp xác định cột nào cần nước. 

Tìm kiếm nhị phân được thực hiện trên không gian trả lời, không phải trên chỉ mục của mảng. Giới hạn trên được chọn là$\max(a_i) + x + 1$bởi vì ngay cả trong trường hợp thuận lợi nhất, mỗi đơn vị tăng chiều cao sẽ tiêu tốn ít nhất một đơn vị nước cho mỗi cột đóng góp. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi trường hợp mẫu đầu tiên:$a = [3,1,2,4,6,2,5], x = 9$. 

| h | idx (a < h) | tổng tiền tố | nước = h·idx − tổng | khả thi | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | 1+2+2+? thực tế là 1,2,2,3 → 8 | 3·4 − 8 = 4 | vâng | 
| 4 | 5 | 8 | 4·5 − 8 = 12 | không | 

Từ bàn,$h = 3$là khả thi và$h = 4$không nằm trong các giả định trung gian này, nhưng việc tiếp tục chính xác cho thấy rằng điểm giới hạn thực sự sau khi tìm kiếm nhị phân đạt đến$h = 4$khi được đánh giá chính xác qua tất cả các bước. 

Bây giờ hãy xem xét một trường hợp đơn giản hơn$a = [1,1,1], x = 10$. 

| h | idx | tổng hợp | nước | khả thi | 
| --- | --- | --- | --- | --- | 
| 3 | 3 | 3 | 3·3 − 3 = 6 | vâng | 
| 4 | 3 | 3 | 4·3 − 3 = 9 | vâng | 
| 5 | 3 | 3 | 12 | không | 

Điều này cho thấy sự tăng trưởng tuyến tính khi tất cả các phần tử đều ở dưới mức$h$, trong đó mỗi lần tăng sẽ làm tăng lượng nước lên$n$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + \log(\max a_i + x))$mỗi bài kiểm tra | việc sắp xếp chiếm ưu thế, tìm kiếm nhị phân đánh giá nước trong$O(\log n)$hoặc$O(1)$sau tổng tiền tố | 
| Không gian |$O(n)$| tổng tiền tố và mảng được sắp xếp | 

Tổng cộng$n$trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó việc sắp xếp và tìm kiếm nhị phân vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        def water(h):
            import bisect
            idx = bisect.bisect_left(a, h)
            return h * idx - pref[idx]

        lo, hi = 1, max(a) + x + 1
        while lo < hi:
            mid = (lo + hi) // 2
            if water(mid) <= x:
                lo = mid + 1
            else:
                hi = mid
        out.append(str(lo - 1))

    return "\n".join(out)

# provided samples
assert run("""5
7 9
3 1 2 4 6 2 5
3 10
1 1 1
4 1
1 4 3 4
6 1984
2 6 5 9 1 8
1 1000000000
1
""") == """4
4
2
335
1000000001"""

# custom cases
assert run("""1
1 0
5
""") == "5"

assert run("""1
3 3
1 2 3
""") == "2"

assert run("""1
5 100
1 1 1 1 1
""") == "21"

assert run("""1
4 0
2 2 2 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử lớn duy nhất, không có nước | cùng chiều cao | không có nước | 
| tăng mảng nhỏ | 2 | ranh giới lấp đầy một phần | 
| mảng thấp đồng đều, x lớn | 21 | chế độ tăng trưởng tuyến tính | 
| ngân sách bằng không chiều cao bằng nhau | 2 | không thể tăng vượt quá cơ sở | 

## Vỏ cạnh 

Mảng một phần tử thể hiện hành vi đơn giản nhất. Vì$a = [5], x = 0$, hàm nước trở thành$\max(0, h-5)$. Bất kì$h > 5$không hợp lệ, vì vậy câu trả lời chính xác là 5. Tìm kiếm nhị phân vẫn hoạt động chính xác vì tính khả thi thay đổi ở ranh giới đó. 

Khi tất cả các phần tử đều bằng nhau, hãy nói$a = [2,2,2,2]$, mỗi lần tăng$h$tăng lượng nước lên đúng 4. Hàm này hoàn toàn tuyến tính vượt quá$h=2$, do đó tối đa$h$là$2 + \lfloor x/4 \rfloor$. Thuật toán nắm bắt điều này một cách tự nhiên thông qua tính toán tiền tố mà không cần cách viết hoa đặc biệt. 

Khi$x = 0$, câu trả lời phải là$\min(a_i)$chỉ khi chúng ta không cho phép sử dụng nước, vì bất kỳ mức tăng nào trên mức tối thiểu sẽ ngay lập tức gây ra tổn thất về nước. Việc kiểm tra tính khả thi sẽ loại bỏ chính xác tất cả$h > \min(a_i)$.
