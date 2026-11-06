---
title: "CF 1303B - Dự án quốc gia"
description: "Chúng tôi đang xây dựng một con đường có chiều dài $n$, trong đó mỗi đơn vị đường cuối cùng phải được trải nhựa đúng một lần. Việc xây dựng tiến hành từng ngày và mỗi ngày chúng ta có thể làm việc trên đúng một đơn vị hoặc không làm gì cả. Thời tiết có tính chu kỳ."
date: "2026-06-16T05:41:22+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1303
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 82 (Rated for Div. 2)"
rating: 1400
weight: 1303
solve_time_s: 213
verified: false
draft: false
---

[CF 1303B - Dự án quốc gia](https://codeforces.com/problemset/problem/1303/B) 

**Đánh giá:** 1400 
**Thẻ:** toán 
**Thời gian giải:** 3m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xây dựng một con đường dài$n$, trong đó mỗi đơn vị đường cuối cùng phải được trải nhựa đúng một lần. Việc xây dựng tiến hành từng ngày và mỗi ngày chúng ta có thể làm việc trên đúng một đơn vị hoặc không làm gì cả. 

Thời tiết có tính chu kỳ. Nó bắt đầu bằng một khối$g$những ngày tốt lành, tiếp theo là$b$những ngày tồi tệ, và mô hình này lặp đi lặp lại mãi mãi. Quá trình này luôn bắt đầu vào một ngày tốt lành. Nếu chúng ta trải nhựa vào ngày đẹp trời thì đơn vị đó sẽ có chất lượng cao; nếu chúng ta đặt nó vào một ngày xấu, nó sẽ có chất lượng thấp. Bỏ qua một ngày không tiến triển việc xây dựng nhưng vẫn tiến lịch. 

Mục tiêu không phải là giảm thiểu chi phí hay thời gian theo nghĩa đơn giản mà là giảm thiểu tổng số ngày cần thiết để tất cả$n$các đơn vị đã được hoàn thiện, với một hạn chế nữa là ít nhất một nửa trong số chúng phải có chất lượng cao. Số lượng đơn vị chất lượng cao cần thiết là$\lceil n/2 \rceil$. 

Ràng buộc$n, g, b \le 10^9$Và$T \le 10^4$loại trừ bất kỳ mô phỏng nào trong nhiều ngày. Ngay cả việc mô phỏng một trường hợp thử nghiệm đơn lẻ theo thang đo câu trả lời cũng là không thể, vì bản thân câu trả lời có thể theo thứ tự$10^{18}$. Lời giải phải dựa vào đặc tính toán học trực tiếp về số lượng ngày tốt xuất hiện trong tiền tố thời gian. 

Một vấn đề tế nhị xuất hiện là ngày tốt hiếm hoi so với ngày xấu. Một chiến lược tham lam ngây thơ như “luôn làm việc vào những ngày tốt và lấp đầy những ngày còn lại bằng những ngày tồi tệ” có thể đánh lừa trực giác vì việc bỏ qua sẽ làm thay đổi sự liên kết với cấu trúc tuần hoàn. Một trường hợp thất bại khác là giả định rằng chúng ta có thể độc lập chọn ngày nào được sử dụng tốt mà không tôn trọng vị trí định kỳ cố định của chúng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng từng ngày, theo dõi xem mỗi ngày là tốt hay xấu và đếm xem chúng tôi đã xây dựng được bao nhiêu đơn vị chất lượng tốt và tổng số. Chúng tôi sẽ dừng lại khi chúng tôi đạt được$n$đơn vị và kiểm tra xem ít nhất một nửa có chất lượng cao hay không. Điều này hoạt động về mặt khái niệm vì nó tuân theo trực tiếp định nghĩa quy trình, nhưng nó đòi hỏi khả năng lặp lại cho đến khi có thời gian trả lời, thời gian này có thể cực kỳ lớn. Trong trường hợp xấu nhất, khi hiếm có ngày tốt và chúng ta thường xuyên bỏ qua hoặc lãng phí việc căn chỉnh, mô phỏng có thể phát triển đến mức$O(\text{answer})$, vượt xa giới hạn. 

Nhận xét quan trọng là chúng ta không bao giờ được hưởng lợi từ việc hoàn thành tổng số đơn vị ít hơn nếu chúng ta đã có đủ cơ hội tốt trước đó. Thay vào đó, cấu trúc chỉ phụ thuộc vào số lượng ngày tốt xảy ra trong ngày đầu tiên.$d$ngày trong lịch. Vì mô hình này hoàn toàn tuần hoàn nên chúng ta có thể tính số ngày tốt trong bất kỳ tiền tố nào mà không cần mô phỏng. 

Đối với bất kỳ tiền tố nào của$d$ngày, chúng ta có đầy đủ các chu kỳ dài$g+b$và mỗi chu kỳ đóng góp chính xác$g$những ngày tốt lành. Phần còn lại đóng góp$\min(g, d \bmod (g+b))$những ngày tốt lành. Như vậy:$$\text{good}(d) = \left\lfloor \frac{d}{g+b} \right\rfloor g + \min(g, d \bmod (g+b))$$Bây giờ bài toán xây dựng trở thành: tìm giá trị nhỏ nhất$d$sao cho chúng ta có thể đặt$n$tổng số đơn vị trong$d$ngày, đồng thời đảm bảo ít nhất$\lceil n/2 \rceil$trong số những vị trí đó rơi vào những ngày tốt lành. Vì chúng tôi luôn có thể làm việc vào bất kỳ ngày nào nên tổng số vị trí trong$d$ngày chỉ đơn giản là$d$, vậy chúng ta chỉ cần$d \ge n$, và hạn chế thực sự là liệu ngày đầu tiên có tốt không$d$ít nhất là ngày$\lceil n/2 \rceil$. Nếu không, chúng ta phải gia hạn$d$cho đến khi đủ ngày tốt lành xuất hiện. 

Do đó, vấn đề trở thành tìm kiếm nhị phân trên$d$, kiểm tra điều kiện đơn điệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\text{answer})$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + đếm số học |$O(T \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào một trường hợp thử nghiệm duy nhất. 

1. Tính số lượng đơn vị chất lượng cao cần thiết như sau:$need = \lceil n/2 \rceil$. Đây là số ngày tốt tối thiểu mà chúng ta phải “che phủ” bằng việc trải nhựa. 
2. Xác định hàm$good(d)$trả về số ngày tốt lành xuất hiện trong lần đầu tiên$d$ngày sử dụng số học chu kỳ. Điều này tránh mô phỏng và sử dụng trực tiếp cấu trúc tuần hoàn. 
3. Kiểm tra xem một$d$là đủ: nó phải thỏa mãn cả hai$d \ge n$(chúng tôi phải có ít nhất đủ ngày để đặt tất cả các đơn vị) và$good(d) \ge need$(tồn tại đủ vị trí chất lượng tốt). 
4. Sử dụng tìm kiếm nhị phân$d$trong một phạm vi rộng lớn, thường$[1, 10^{18}]$, bởi vì câu trả lời có thể rất lớn khi những ngày tồi tệ chiếm ưu thế. 
5. Đối với mỗi điểm giữa, hãy đánh giá điều kiện. Nếu khả thi thì di chuyển sang trái; nếu không thì di chuyển sang phải. Chúng tôi đang tìm kiếm số ngày khả thi nhỏ nhất. 
6. Xuất đầu tiên$d$thỏa mãn cả hai ràng buộc. 

Lý do tìm kiếm nhị phân có giá trị là do việc tăng$d$chỉ có thể tăng số ngày tốt và tổng số slot có sẵn nên tính khả thi là đơn điệu. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tính khả thi chỉ phụ thuộc vào độ dài tiền tố chứ không phụ thuộc vào các lựa chọn lập lịch. Đối với bất kỳ cố định$d$, ta luôn có chính xác$d$cơ hội trải nhựa đường, và chính xác$good(d)$trong số đó là những cơ hội có chất lượng tốt. Vì chúng ta có thể chọn đơn vị nào để gán cho ngày nào, hạn chế duy nhất là liệu có đủ số ngày tốt để đáp ứng số lượng yêu cầu hay không. Khi một tiền tố khả thi thì mọi tiền tố lớn hơn vẫn khả thi vì cả tổng số và số lượng tốt đều không giảm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def good_days(d, g, b):
    cycle = g + b
    full = d // cycle
    rem = d % cycle
    return full * g + min(g, rem)

def solve():
    T = int(input())
    for _ in range(T):
        n, g, b = map(int, input().split())
        need = (n + 1) // 2

        lo, hi = 1, 10**18
        ans = hi

        while lo <= hi:
            mid = (lo + hi) // 2
            if mid >= n and good_days(mid, g, b) >= need:
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        print(ans)

if __name__ == "__main__":
    solve()
```chức năng`good_days`thực hiện trực tiếp công thức tính tuần hoàn. Tìm kiếm nhị phân thực thi đồng thời hai ràng buộc: tổng số ngày đủ để hoàn thành con đường và đủ số ngày tốt để đáp ứng yêu cầu chất lượng. 

Một cạm bẫy phổ biến là quên`mid >= n`tình trạng. Nếu không có nó, tìm kiếm có thể trả về một tiền tố có đủ số ngày tốt nhưng không đủ tổng số ngày để hoàn thành việc xây dựng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 1
```Chúng tôi cần$need = 3$. Ngày tốt xen kẽ mỗi chu kỳ 2. 

| d | chu kỳ đầy đủ | rem | tốt(d) | d ≥ n | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | không | không | 
| 3 | 1 | 1 | 2 | vâng | không | 
| 5 | 2 | 1 | 3 | vâng | vâng | 

Điểm khả thi đầu tiên là$d = 5$. Điều này cho thấy rằng mặc dù số ngày tốt rất ít nhưng việc chờ đợi lâu hơn sẽ giúp điều chỉnh đủ số ngày tốt với các vị trí cần thiết. 

### Ví dụ 2 

đầu vào:```
8 10 10
```Đây$g \ge n$, nên mỗi ngày ở khối đầu tiên đều tốt. 

| d | chu kỳ đầy đủ | rem | tốt(d) | d ≥ n | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 8 | 0 | 8 | 8 | vâng | vâng | 

Câu trả lời là 8 ngay lập tức, vì tất cả các đơn vị cần thiết có thể được đặt trong những ngày tốt mà không cần phải kéo dài sang những thời kỳ xấu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log n)$| Mỗi trường hợp thử nghiệm thực hiện tìm kiếm nhị phân lên tới$10^{18}$với tính toán ngày tốt theo thời gian liên tục | 
| Không gian |$O(1)$| Chỉ các biến số học được lưu trữ | 

Hệ số logarit đủ nhỏ để$10^4$các trường hợp thử nghiệm và tất cả các phép toán đều là số học số nguyên, do đó lời giải dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import ceil

    def good_days(d, g, b):
        cycle = g + b
        full = d // cycle
        rem = d % cycle
        return full * g + min(g, rem)

    def solve():
        T = int(input())
        for _ in range(T):
            n, g, b = map(int, input().split())
            need = (n + 1) // 2

            lo, hi = 1, 10**18
            ans = hi

            while lo <= hi:
                mid = (lo + hi) // 2
                if mid >= n and good_days(mid, g, b) >= need:
                    ans = mid
                    hi = mid - 1
                else:
                    lo = mid + 1

            print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n5 1 1\n8 10 10\n1000000 1 1000000\n") == "5\n8\n499999500000"

# minimum input
assert run("1\n1 1 1\n") == "1"

# all good days dominate
assert run("1\n10 100 1\n") == "10"

# equal pattern boundary
assert run("1\n6 2 2\n") == "6"

# bad-heavy stress
assert run("1\n5 1 1000000000\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | trường hợp cạnh tối thiểu | 
| 10 100 1 | 10 | ngày tốt lành dồi dào | 
| 6 2 2 | 6 | ranh giới chu kỳ cân bằng | 
| 5 1 1e9 | 5 | thống trị cực xấu | 

## Vỏ cạnh 

Khi nào$g \ge n$, tất cả các đơn vị tốt cần thiết có thể được đặt bên trong khối ngày tốt đầu tiên. Thuật toán xử lý việc này vì$good(n) = n$, làm cho việc tìm kiếm nhị phân được giải quyết ngay lập tức tại$d = n$. 

Khi$g \ll b$, những ngày tốt lành cực kỳ thưa thớt và hầu hết các chu kỳ đều bị lãng phí. Tìm kiếm nhị phân mở rộng$d$cho đến khi tích lũy đủ chu kỳ và công thức số học đếm chính xác từng phần chu kỳ mà không cần mô phỏng. 

Khi$n = 1$, yêu cầu không quan trọng và ngày đầu tiên luôn hoạt động vì trình tự bắt đầu vào một ngày tốt, vì vậy$good(1) = 1$.
