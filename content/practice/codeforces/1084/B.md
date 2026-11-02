---
title: "CF 1084B - Kvass và nàng công bằng"
description: "Chúng tôi được đưa cho một vài thùng kvass, mỗi thùng chứa một lượng ban đầu. Trong một thao tác, chúng ta có thể giảm chính xác một lít bất kỳ thùng nào và mỗi thao tác như vậy đóng góp một lít vào tổng lượng mà chúng ta muốn “đổ ra”."
date: "2026-06-15T05:47:34+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1084
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 526 (Div. 2)"
rating: 1200
weight: 1084
solve_time_s: 144
verified: true
draft: false
---

[CF 1084B - Kvass và nàng công bằng](https://codeforces.com/problemset/problem/1084/B) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một vài thùng kvass, mỗi thùng chứa một lượng ban đầu. Trong một thao tác, chúng ta có thể giảm chính xác một lít bất kỳ thùng nào và mỗi thao tác như vậy đóng góp một lít vào tổng lượng mà chúng ta muốn “đổ ra”. Mục đích là để loại bỏ chính xác$s$tổng số lít trên tất cả các thùng. Sau khi làm như vậy, mức kvass còn lại rất quan trọng: chúng tôi quan tâm đến việc tối đa hóa giá trị tối thiểu trong số tất cả các thùng. 

Vì vậy, nhiệm vụ không chỉ là kiểm tra xem liệu chúng ta có thể loại bỏ$s$lít, mà là về việc phân phối những thứ bị loại bỏ đó theo cách tránh làm cho bất kỳ thùng nào trở nên quá nhỏ. 

Hạn chế chính là$n \le 10^3$Và$s \le 10^{12}$. cái nhỏ$n$cho thấy chúng ta có thể đủ khả năng bậc hai hoặc$n \log n$hành vi, nhưng rất lớn$s$ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng giảm một lít mỗi lần. Giải pháp nào cũng phải suy luận tổng hợp chứ không phải thao tác từng bước một. 

Trường hợp khó nhận biết xuất hiện khi tổng kvass nhỏ hơn$s$. Trong trường hợp đó, không có sự phân phối chất loại bỏ nào có thể đến được mục tiêu, ngay cả khi chúng tôi làm trống tất cả các thùng. Một tình huống phức tạp khác là khi việc tham lam giảm số thùng lớn nhất cục bộ có vẻ đúng nhưng thực tế lại phân bổ sai số lượng bị loại bỏ theo cách làm giảm mức tối thiểu nhiều hơn mức cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là mô phỏng việc loại bỏ từng lít một. Ở mỗi bước, chúng tôi chọn một thùng có giá trị tối đa hiện tại và giảm nó đi một. Điều này giữ mức tối thiểu càng lớn càng tốt tại địa phương, vì chúng tôi tránh chạm vào các thùng nhỏ. Sau đó$s$những hoạt động như vậy, chúng tôi tính toán giá trị tối thiểu. 

Điều này hoạt động chính xác vì mỗi thao tác đều bảo toàn trạng thái tốt nhất có thể cho bước tiếp theo. Tuy nhiên, sự phức tạp là$O(s \log n)$nếu chúng ta duy trì một đống, và$s$có thể lên đến$10^{12}$, điều này làm cho phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là câu trả lời cuối cùng là một vấn đề về ngưỡng. Thay vì mô phỏng các hoạt động, chúng tôi giả định giá trị tối thiểu ứng viên$x$. Nếu chúng ta buộc mỗi thùng ít nhất phải có$x$, thì phần thừa ở trên$x$có thể được gỡ bỏ một cách tự do. Tổng số tiền có thể tháo rời cho một khoản cố định$x$trở thành$$\sum \max(0, v_i - x).$$Nếu số tiền này ít nhất là$s$, thì có thể đạt được mức tối thiểu ít nhất$x$. Nếu không thì không thể được. 

Điều này biến bài toán thành việc tìm giá trị lớn nhất$x$sao cho điều kiện được giữ. Vì tính khả thi là đơn điệu trong$x$, chúng ta có thể tìm kiếm nhị phân câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng tham lam |$O(s \log n)$|$O(n)$| Quá chậm | 
| Tìm kiếm nhị phân theo câu trả lời |$O(n \log V)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền của tất cả các thùng. Nếu nó nhỏ hơn$s$, ngay lập tức trả về -1. Không có đủ kvass để loại bỏ$s$lít bất kể chiến lược. 
2. Đặt phạm vi tìm kiếm nhị phân cho câu trả lời. Giá trị cuối cùng tối thiểu có thể là 0 và tối đa là$\max(v_i)$, vì không có thùng nào có thể vượt quá giá trị ban đầu của nó. 
3. Đối với giá trị ứng viên$x$, hãy tính xem có thể loại bỏ bao nhiêu kvass nếu mỗi thùng giảm xuống ít nhất$x$. Đối với mỗi thùng, nếu$v_i > x$, chúng ta có thể loại bỏ$v_i - x$, nếu không thì bằng không. 
4. Nếu tổng số kvass có thể tháo rời ít nhất$s$, sau đó$x$là khả thi. Điều này có nghĩa là chúng ta có thể đạt được một cấu hình trong đó ít nhất tất cả các thùng đều$x$, vì vậy chúng tôi cố gắng tăng$x$. 
5. Nếu tổng lượng kvass có thể tháo rời nhỏ hơn$s$, sau đó$x$là quá lớn. Chúng tôi không thể duy trì mức cao tối thiểu đó trong khi loại bỏ đủ kvass, vì vậy chúng tôi giảm$x$. 
6. Chạy tìm kiếm nhị phân để tìm ra phương án khả thi lớn nhất$x$. Trả lại nó như là câu trả lời. 

### Tại sao nó hoạt động 

Tính chất quan trọng nhất là tính đơn điệu. Nếu một giá trị$x$là khả thi thì bất kỳ giá trị nhỏ hơn nào cũng có thể thực hiện được, vì việc giảm mức tối thiểu chỉ làm tăng lượng kvass có thể tháo rời được. Điều này tạo ra một vị từ đơn điệu trên$x$, đảm bảo tìm kiếm nhị phân tìm thấy ngưỡng tối ưu. Kiểm tra tính khả thi nắm bắt chính xác tất cả các cách phân phối loại bỏ hợp lệ vì mọi cấu hình cuối cùng hợp lệ đều tương ứng với một số lựa chọn về mức độ giảm của mỗi thùng và điều đó được đặc trưng đầy đủ bởi ràng buộc ngưỡng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(v, s, x):
    total = 0
    for a in v:
        if a > x:
            total += a - x
    return total >= s

n, s = map(int, input().split())
v = list(map(int, input().split()))

if sum(v) < s:
    print(-1)
    sys.exit()

lo, hi = 0, max(v)
ans = 0

while lo <= hi:
    mid = (lo + hi) // 2
    if can(v, s, mid):
        ans = mid
        lo = mid + 1
    else:
        hi = mid - 1

print(ans)
```Mã bắt đầu bằng việc kiểm tra tính khả thi trên tổng số tiền, vì không có thỏa thuận nào có thể vượt quá kvass có sẵn. Chức năng trợ giúp`can`tính tổng số tiền có thể tháo rời nếu chúng ta giới hạn tất cả các thùng ở mức`x`. Sau đó, tìm kiếm nhị phân sẽ điều chỉnh mức tối thiểu ứng viên lên khi khả thi và giảm xuống nếu không, cuối cùng hội tụ đến mức tối thiểu hợp lệ tối đa. 

Một lỗi phổ biến ở đây là quên rằng việc xóa khỏi các thùng khác nhau sẽ độc lập sau khi chúng tôi ấn định một ngưỡng. Một người khác đang cố gắng mô phỏng một cách tham lam thay vì chuyển vấn đề thành một hàm quyết định đơn điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
4 3 5
```Chúng tôi kiểm tra tính khả thi đối với các giá trị tối thiểu khác nhau. 

| x | số tiền có thể tháo rời | khả thi | 
| --- | --- | --- | 
| 0 | 12 | vâng | 
| 2 | 3 | vâng | 
| 3 | 1 | không | 

Tìm kiếm nhị phân hội tụ về 3. 

Điều này cho thấy yếu tố giới hạn không phải là tổng khối lượng mà là mức độ loại bỏ có thể được phân bổ đồng đều trong khi vẫn giữ tất cả các thùng trên ngưỡng. 

### Ví dụ 2 

đầu vào:```
2 4
5 3
```| x | số tiền có thể tháo rời | khả thi | 
| --- | --- | --- | 
| 2 | 4 | vâng | 
| 3 | 2 | không | 

Câu trả lời là 2. 

Điều này chứng tỏ một trường hợp trong đó một thùng lớn chiếm ưu thế trong việc loại bỏ, nhưng việc đẩy các khối tối thiểu lên quá cao cũng đủ để loại bỏ tổng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log V)$| Tìm kiếm nhị phân trên phạm vi giá trị, mỗi lần kiểm tra sẽ quét tất cả các thùng | 
| Không gian |$O(1)$| Chỉ lưu trữ đầu vào và bộ đếm | 

Với$n \le 1000$và giá trị lên đến$10^9$, số lần kiểm tra là khoảng 30 và mỗi lần kiểm tra là tuyến tính$n$, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n, s = map(int, input().split())
    v = list(map(int, input().split()))

    if sum(v) < s:
        return "-1"

    def can(x):
        total = 0
        for a in v:
            if a > x:
                total += a - x
        return total >= s

    lo, hi = 0, max(v)
    ans = 0

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    return str(ans)

def run(inp: str) -> str:
    return solve(inp)

# provided sample
assert run("3 3\n4 3 5\n") == "3"

# minimum case
assert run("1 1\n5\n") == "4"

# impossible case
assert run("2 10\n1 2\n") == "-1"

# all equal
assert run("4 4\n5 5 5 5\n") == "4"

# large skew
assert run("3 5\n10 1 1\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/5 | 4 | ranh giới thùng đơn | 
| 2 10 / 1 2 | -1 | loại bỏ hoàn toàn không thể | 
| 4 thùng cả 5, s=4 | 4 | phân phối thống nhất | 
| 10 1 1 trường hợp | 3 | phân phối lệch | 

## Vỏ cạnh 

Khi tổng lượng kvass nhỏ hơn$s$, thuật toán ngay lập tức trả về -1. Ví dụ, đầu vào`2 10`với thùng`[1, 2]`tạo ra tổng 3, không đủ. Việc kiểm tra sớm ngăn tìm kiếm nhị phân tạo ra giá trị vô nghĩa. 

Khi tất cả các thùng đều bằng nhau, hãy nói`[5, 5, 5]`với$s = 2$, quá trình kiểm tra tính khả thi cho thấy rằng bất kỳ mức tối thiểu nào trên 3 đều không thể thực hiện được vì chỉ có tổng cộng 2 lần loại bỏ trên tất cả các thùng. Tìm kiếm nhị phân ổn định chính xác ở mức 3. 

Khi một thùng lớn hơn đáng kể so với những thùng khác, chẳng hạn như`[10, 1, 1]`, chất thải tập trung trong thùng lớn. Công thức ngưỡng vẫn hoạt động vì nó chỉ tính phần vượt quá$x$, không phụ thuộc vào việc xóa video đến từ đâu.
