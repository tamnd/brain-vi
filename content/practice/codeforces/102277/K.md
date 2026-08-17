---
title: "CF 102277K - Sôcôla"
description: "Timothy có một hình hộp chữ nhật có chiều rộng và chiều cao cố định. Mỗi thanh sô cô la phải có kích thước nguyên, phải nằm gọn trong hộp đó mà không cần xoay và mỗi cặp kích thước có thể có chỉ được sử dụng nhiều nhất một lần. Một thanh có kích thước (a nhân b) có giá (a b)."
date: "2026-08-16T19:45:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "K"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 294
verified: true
draft: false
---

[CF 102277K - Sôcôla](https://codeforces.com/problemset/problem/102277/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 54 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Timothy có một hình hộp chữ nhật có chiều rộng và chiều cao cố định. Mỗi thanh sô cô la phải có kích thước nguyên, phải nằm gọn trong hộp đó mà không cần xoay và mỗi cặp kích thước có thể có chỉ được sử dụng nhiều nhất một lần. Một thanh có kích thước (a \times b) có giá (a b). 

Định hướng là một phần của bản sắc của một quán bar. Một thanh (5\times3) và một thanh (3\times5) là những quà tặng khác nhau, trong khi hai thanh (5\times5) sẽ giống hệt nhau và chỉ được sử dụng một thanh. Vì vậy, nếu hộp có kích thước (W\times H), những món quà có thể có chính xác là các cặp đặt hàng (WH). 

[ 
1\le a\le W,\qquad 1\le b\le H. 
] 

Nhiệm vụ là chọn càng nhiều cặp khác nhau càng tốt trong khi vẫn giữ tổng diện tích của chúng, cũng là tổng chi phí, trong số dư tiết kiệm của Timothy (B). Đầu ra là số lượng thanh tối đa có thể được tạo ra. 

Trang cuộc thi chính thức đưa ra giới hạn thời gian 3 giây và giới hạn bộ nhớ 256 MB. Khó khăn xuất phát từ thực tế là hình chữ nhật có thể biểu thị một số lượng rất lớn các cặp kích thước có thể có, do đó việc liệt kê tất cả các thanh (WH) không phải là một chiến lược khả thi. Chúng ta cần khai thác thực tế là giá thành của một thanh chỉ phụ thuộc vào sản phẩm (ab). 

Trường hợp cạnh không rõ ràng đầu tiên là một hộp vuông. Đối với hộp (2\times2), bốn chiều có thể có giá trị (1,2,2,4). Với ngân sách (5), câu trả lời đúng là (3), vì chúng ta có thể chọn (1\times1), (1\times2) và (2\times1), có tổng chi phí là (5). Việc triển khai bất cẩn coi (1\times2) và (2\times1) là cùng một thanh sẽ trả về (2). 

Trường hợp thứ hai xảy ra khi ngân sách quá nhỏ để mua bất cứ thứ gì. Đối với ô (3\times3) và ngân sách (0), câu trả lời đúng là (0). Mọi thanh hợp pháp đều có diện tích dương nên việc chọn thanh rẻ nhất sẽ vượt quá ngân sách. Việc triển khai giả định rằng luôn có thể chọn ít nhất một thanh sẽ trả về không chính xác (1). 

Trường hợp cạnh thứ ba là khi ngân sách đủ lớn để mua mọi thanh có thể. Đối với hộp (2\times2), tổng giá trị của cả bốn thanh là (1+2+2+4=9). Với ngân sách (9), đáp án là (4). Không có lý do gì để dừng lại sau khi tìm thấy một số tiền tố hợp lý, bởi vì mọi cặp thứ nguyên có thể đều có thể được lấy. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi cặp có thể ((a,b)), tính chi phí (ab), sắp xếp tất cả chi phí (WH) và lấy tiền tố rẻ nhất có tổng phù hợp với ngân sách. Điều này đúng vì mỗi thanh đều có giá trị như nhau đối với chúng ta, tức là có thêm một học sinh nhận được một món quà, vì vậy trong số số thanh cố định bất kỳ, chúng ta phải luôn chọn những thanh có sẵn rẻ nhất. 

Vấn đề là số lượng cặp. Việc tạo toàn bộ bảng nhân đã tốn các phép tính (O(WH)) và việc lưu trữ nó cần có bộ nhớ (O(WH)). Nếu cả hai chiều đều lớn thì điều này vượt xa những gì giải pháp 3 giây có thể thực hiện được. Ngay cả một phiên bản tránh lưu trữ bảng nhưng quét từng cặp vẫn thực hiện phép nhân (WH). 

Quan sát quan trọng là chúng ta không thực sự cần biết các thanh riêng lẻ theo thứ tự được sắp xếp. Đối với giá trị (x), hãy xem xét tất cả các thanh có diện tích tối đa là (x). Đối với chiều rộng cố định (a), chiều cao cho phép thỏa mãn 

[ 
b\le \left\lfloor\frac{x}{a}\right\rfloor. 
] 

Vì hộp giới hạn chiều cao ở (H) nên số thanh như vậy ở hàng (a) là 

[ 
t_a=\min\left(H,\left\lfloor\frac{x}{a}\right\rfloor\right). 
] 

Điều này cho phép chúng tôi tính toán cả số thanh có chi phí tối đa (x) và tổng chi phí của chúng mà không cần liệt kê từng cặp. 

Khó khăn còn lại là tổng hợp tất cả những gì có thể (a). Các giá trị (\lfloor x/a\rfloor) không đổi trong khoảng thời gian dài khi (a) trở nên lớn. Chúng ta có thể xử lý các giá trị (a) nhỏ riêng lẻ và các giá trị lớn trong các nhóm có cùng thương số. Đây là kỹ thuật phân nhóm theo tầng tiêu chuẩn và cung cấp công việc (O(\sqrt{x})) cho một truy vấn.

Có một cách thậm chí còn rõ ràng hơn để sử dụng chức năng này. Gọi (F(x)) là tổng chi phí của mỗi thanh pháp luật có giá tối đa là (x). Chúng ta có thể tìm (x) nhỏ nhất mà (F(x)>B). Khi đó mọi thanh rẻ hơn (x) đều có thể được mua, trong khi (x) là giá của nhóm thanh riêng biệt tiếp theo. Số tiền còn lại có thể mua một số thanh giá trị (x) đó. Điều này tránh tìm kiếm nhị phân thứ hai theo số lượng thanh. 

Phương pháp brute-force hoạt động vì việc sắp xếp đưa ra chính xác tiền tố rẻ nhất có thể. Không thành công vì bảng cửu chương quá lớn. Quan sát cho thấy tất cả các thanh có cùng diện tích tạo thành một nhóm ngưỡng cho phép chúng ta đếm và tính tổng toàn bộ các nhóm cùng một lúc, giảm vấn đề thành tổng chia theo sàn và tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(WH\log(WH))) | (O(WH)) | Quá chậm | 
| Nhóm ngưỡng | (O(\sqrt B\log B)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gọi (W) và (H) là kích thước của hộp và (B) là số tiền tiết kiệm được. Mỗi thanh sôcôla hợp pháp tương ứng với một cặp được đặt hàng ((a,b)) với (1\le a\le W) và (1\le b\le H), và giá của nó là (ab). 
2. Tính tổng chi phí của từng thanh có thể có: 

[ 
T=\frac{W(W+1)}2\cdot\frac{H(H+1)}2. 
] 

Nếu (B\ge T), mọi thanh có thể đều có thể mua được, nên câu trả lời đơn giản là (WH). 

1. Xác định`stats(x)`để trả về hai giá trị. Đầu tiên là số thanh pháp có diện tích lớn nhất (x). Thứ hai là tổng diện tích của chúng. 

Đối với chiều rộng cố định (a), số chiều cao có thể sử dụng là 

[ 
t=\min(H,\lfloor x/a\rfloor). 
] 

Số đóng góp của chiều rộng này là (t), trong khi đóng góp chi phí của nó là 

a\frac{t(t+1)}2. 
] 

1. Xử lý tất cả các chiều rộng có (t=H) cùng nhau. Điều này xảy ra khi 

[ 
a\le \left\lfloor\frac{x}{H}\right\rfloor. 
] 

Nếu phạm vi này chứa (m) chiều rộng, nó sẽ đóng góp thanh (mH) và 

[ 
\frac{m(m+1)}2\frac{H(H+1)}2 
] 

tổng chi phí. 

Việc nhóm phạm vi đầu tiên này rất quan trọng vì nó có thể chứa một số lượng lớn chiều rộng, trong khi mỗi chiều rộng trong số chúng đều có cùng số chiều cao hợp lệ. 

1. Đối với các chiều rộng nhỏ còn lại, lặp trực tiếp lên tới (\lfloor\sqrt{x}\rfloor). Chỉ có (O(\sqrt{x})) giá trị như vậy. 
2. Một lần (a>\sqrt{x}), thương số (\lfloor x/a\rfloor) nhỏ. Đối với dòng điện (a), đặt (q=\lfloor x/a\rfloor). Thương số tương tự vẫn có giá trị thông qua 

[ 
r=\left\lfloor\frac{x}{q}\right\rfloor. 
] 

Vì vậy, tất cả các chiều rộng từ (a) đến (r) có thể được xử lý cùng nhau. Đóng góp của họ vào số lượng là độ dài khoảng thời gian nhân với (q) và đóng góp chi phí của họ là 

[ 
\left(\sum_{i=a}^{r}i\right)\frac{q(q+1)}2. 
] 

1. Bây giờ chúng ta đã biết cách tính (F(x)), tổng chi phí của tất cả các thanh có diện tích lớn nhất (x). Vì (F(x)) không giảm nên tìm kiếm nhị phân cho (x) nhỏ nhất sao cho 

[ 
F(x)>B. 
] 

Giới hạn trên của tìm kiếm có thể được lấy là (\min(WH,B+1)). Nếu không phải tất cả các quán bar đều có giá cả phải chăng thì diện tích đầu tiên vượt quá ngân sách không thể lớn hơn (B+1). 

1. Đặt giá trị đầu tiên này là (x). Tính số lượng và tổng giá trị của tất cả các thanh có diện tích lớn nhất (x-1). Mỗi một trong những thanh này có thể được mua. 
2. Hãy để`remaining = B - cost`. Mỗi ứng cử viên còn lại trong nhóm chi phí tiếp theo có giá chính xác (x), vì vậy chúng ta có thể mua 

[ 
\left\lfloor\frac{\text{remaining}}x\right\rfloor 
] 

nhiều thanh hơn. 

1. Thêm số này vào số thanh có giá dưới (x). Kết quả là số lượng quà tặng tối đa có thể. 

### Tại sao nó hoạt động 

Bất biến là, với mọi ngưỡng (x),`stats(x)`mô tả chính xác tập hợp đầy đủ các thanh hợp pháp có giá tối đa là (x). Vì mọi thanh đều có lợi ích như nhau nên giải pháp tối ưu luôn bao gồm những thanh rẻ nhất hiện có. Gọi (x) là ngưỡng chi phí nhỏ nhất mà nhóm hoàn chỉnh của nó sẽ khiến ngân sách không đủ. Tất cả các thanh có giá dưới (x) phải được đưa vào một giải pháp tối ưu, vì việc thay thế bất kỳ thanh nào bằng thanh đắt tiền hơn không thể cải thiện số lượng quà tặng. Sau khi lấy chúng, mỗi ứng cử viên còn lại có chi phí chính xác (x), vì vậy lấy càng nhiều ứng viên trong phạm vi ngân sách còn lại cho phép là tối ưu. Việc nhóm ngưỡng chỉ thay đổi cách tính các thanh này chứ không thay đổi thanh nào được thể hiện. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

def solve_case(W, H, B):
    total_bars = W * H
    total_cost = (W * (W + 1) // 2) * (H * (H + 1) // 2)

    if B >= total_cost:
        return total_bars

    if B <= 0:
        return 0

    def stats(x):
        if x <= 0:
            return 0, 0

        # For a <= m, every height 1..H is affordable.
        m = min(W, x // H)

        count = m * H
        cost = (
            m * (m + 1) // 2
            * (H * (H + 1) // 2)
        )

        left = m + 1
        if left > W or left > x:
            return count, cost

        root = isqrt(x)

        # Small widths are processed individually.
        right = min(W, root)

        for a in range(left, right + 1):
            t = min(H, x // a)
            if t <= 0:
                break

            count += t
            cost += a * t * (t + 1) // 2

        left = max(left, root + 1)

        # For a > sqrt(x), floor(x / a) is constant on intervals.
        while left <= W and left <= x:
            q = x // left
            if q <= 0:
                break

            right = min(W, x // q)
            t = min(H, q)

            length = right - left + 1
            count += length * t

            sum_a = (left + right) * length // 2
            cost += sum_a * t * (t + 1) // 2

            left = right + 1

        return count, cost

    # If not all bars fit, the first area whose complete group
    # makes the total exceed B is at most B + 1.
    lo = 1
    hi = min(total_bars if total_bars < B + 1 else B + 1,
             W * H)

    while lo < hi:
        mid = (lo + hi) // 2
        _, cost = stats(mid)

        if cost > B:
            hi = mid
        else:
            lo = mid + 1

    x = lo

    count_before, cost_before = stats(x - 1)
    remaining = B - cost_before

    return count_before + remaining // x

def solve(data):
    values = list(map(int, data.split()))
    if not values:
        return ""

    W, H, B = values[:3]
    return str(solve_case(W, H, B))

def main():
    data = sys.stdin.buffer.read()
    sys.stdout.write(solve(data) + "\n")

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve_case`xử lý trường hợp toàn bộ bảng nhân có giá cả phải chăng. Tích của các tổng tam giác cho ra tổng diện tích chính xác của tất cả các cặp kích thước có thể có theo thứ tự.`stats(x)`là thói quen trung tâm. biểu hiện`m = min(W, x // H)`xác định chiều rộng mà mọi chiều cao lên tới`H`phù hợp dưới ngưỡng. Việc tính toán sự đóng góp của chúng bằng các công thức chuỗi số học sẽ ngăn chặn một vòng lặp tiềm ẩn rất lớn. 

Vòng lặp trực tiếp có chiều rộng lên tới`sqrt(x)`. Sau thời điểm đó,`x // a`nhỏ và chỉ thay đổi ở những vị trí tương đối thưa thớt. biểu hiện`right = min(W, x // q)`tìm khoảng lớn nhất mà thương còn lại`q`. Tổng của tất cả các chiều rộng trong khoảng đó được tính bằng công thức chuỗi số học tiêu chuẩn. 

Tìm kiếm nhị phân sử dụng tổng chi phí thay vì số lượng thanh. Đây là phần tinh tế của việc thực hiện. Nếu như`x`là giá trị đầu tiên mà giá của tất cả các thanh có diện tích lớn nhất`x`vượt quá ngân sách thì`x`phải tương ứng với chi phí thanh thực tế. Nếu không thì`F(x)`sẽ bằng`F(x-1)`, mâu thuẫn với tính tối thiểu. Do đó, mọi thanh không được chọn ở biên có giá chính xác`x`, do đó chia số nguyên cho`x`cung cấp chính xác bao nhiêu vẫn có thể đủ khả năng. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các sản phẩm như`W * (W + 1) * H * (H + 1)`không tràn. Các công thức chỉ sử dụng phép chia số nguyên sau phép nhân, tránh làm tròn dấu phẩy động. Việc tìm kiếm bao gồm cả hai bên và đánh giá`stats(x - 1)`là điểm phân biệt các thanh rẻ hơn với nhóm ranh giới. 

## Ví dụ đã hoạt động 

Vì trang Codeforces được cung cấp hiển thị tiêu đề câu lệnh và siêu dữ liệu cuộc thi nhưng không hiển thị I/O mẫu của câu lệnh trong phần trình bày văn bản của nó, nên các dấu vết sau đây sử dụng hai trường hợp cụ thể của cùng một vấn đề. Vấn đề UCF ban đầu là nguồn gốc của công thức. 

### Ví dụ 1 

Hãy xem xét một hộp (2\times2) có ngân sách (5). 

Bốn thanh có thể có diện tích (1,2,2,4). Ba giá rẻ nhất (1+2+2=5), vì vậy câu trả lời là (3). 

| Ngưỡng (x) | Thanh có giá (\le x) | Đếm | Tổng chi phí | 
| --- | --- | --- | --- | 
| 1 | (1\times1) | 1 | 1 | 
| 2 | (1\times1,1\times2,2\times1) | 3 | 5 | 
| 3 | ba thanh giống nhau | 3 | 5 | 
| 4 | cả bốn thanh | 4 | 9 | 

Tìm kiếm nhị phân tìm thấy (x=4), vì chi phí qua (3) là (5), trong khi chi phí qua (4) là (9). Thuật toán lấy cả ba thanh rẻ hơn (4), để lại ngân sách bằng 0 cho thanh cuối cùng. 

### Ví dụ 2 

Hãy xem xét một hộp (3\times3) có ngân sách (10). 

Các khu vực được 

[ 
1,2,3,2,4,6,3,6,9. 
] 

Sau khi sắp xếp, chúng 

[ 
1,2,2,3,3,4,6,6,9. 
] 

| Ngưỡng (x) | Đếm | Tổng chi phí | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 5 | 
| 3 | 5 | 11 | 
| 4 | 6 | 15 | 

Ngưỡng đầu tiên có nhóm hoàn chỉnh vượt quá ngân sách là (x=3). Tất cả năm thanh có diện tích tối đa (2) có giá (5), để lại (5). Mỗi thanh tiếp theo có giá (3), vì vậy chỉ có thể mua thêm một thanh nữa. Câu trả lời là (4). 

Dấu vết cho thấy lý do tại sao chúng tôi tìm kiếm ngưỡng đầu tiên có chi phí tích lũy vượt quá ngân sách thay vì chỉ tìm kiếm ngưỡng lớn nhất có chi phí tích lũy phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\sqrt B\log B)) | Mỗi truy vấn ngưỡng sử dụng nhóm phân chia tầng trong (O(\sqrt B)) và tìm kiếm nhị phân tạo ra các truy vấn (O(\log B)). | 
| Không gian | (O(1)) | Chỉ các biến vô hướng được duy trì. | 

Thuật toán không bao giờ xây dựng bảng nhân (W\times H). Các truy vấn ngưỡng hoạt động theo các khoảng số học, do đó mức sử dụng bộ nhớ không đổi ngay cả khi số lượng thanh sô cô la có thể có là rất lớn. Giới hạn 3 giây và 256 MB của cuộc thi ban đầu khiến việc giảm tiệm cận này trở nên cần thiết. 

## Trường hợp thử nghiệm 

Tuyên bố ban đầu có sẵn dưới dạng bộ vấn đề Cuộc thi Địa phương UCF 2018, trong đó vấn đề xuất hiện dưới tên Quà tặng Sôcôla. Các thử nghiệm sau đây thực hiện các kích thước tối thiểu, phạm vi kích thước lớn, định hướng chi phí bằng nhau, khả năng chi trả hoàn toàn và ranh giới nơi một thanh bổ sung trở nên quá đắt.```
# helper: run solution on input string, return output string
import io

def run(inp: str) -> str:
    return solve(inp).strip()

# Minimum-size input
assert run("1 1 1\n") == "1", "minimum box and exact budget"

# Nothing is affordable
assert run("3 3 0\n") == "0", "zero budget"

# 2x2 costs are 1, 2, 2, 4
assert run("2 2 5\n") == "3", "equal-cost orientations"

# All bars are affordable
assert run("2 2 9\n") == "4", "entire multiplication table fits"

# Boundary case: 3x3 sorted costs are 1,2,2,3,3,4,6,6,9
assert run("3 3 10\n") == "4", "cannot afford the fifth cheapest bar"

# One-dimensional box, useful for checking arithmetic progression
assert run("1 5 15\n") == "5", "all bars in a 1x5 box"

# Large dimensions with tiny budget
assert run("1000000 1000000 1\n") == "1", "large dimensions, smallest possible budget"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Phiên bản có kích thước tối thiểu và khả năng chi trả chính xác | 
|`3 3 0`|`0`| Trường hợp ranh giới không có thanh chi phí dương phù hợp | 
|`2 2 5`|`3`| Định hướng khác biệt với chi phí ngang nhau | 
|`2 2 9`|`4`| Khả năng chi trả hoàn chỉnh | 
|`3 3 10`|`4`| Ranh giới giữa hai nhóm chi phí tích lũy | 
|`1 5 15`|`5`| Hành vi chuỗi số học cho hộp một chiều | 
|`1000000 1000000 1`|`1`| Kích thước rất lớn với ngân sách nhỏ | 

## Vỏ cạnh 

Đối với một hộp vuông, hướng vẫn quan trọng. Trên đầu vào`2 2 5`, thuật toán tính chi phí (1,2,2,4). Ở ngưỡng (2),`stats(2)`trả về số lượng (3) và chi phí (5). Ngưỡng tiếp theo là (4), có chi phí tích lũy là (9), vì vậy câu trả lời vẫn là (3). Cả (1\times2) và (2\times1) đều được tính vì chúng tương ứng với các độ rộng khác nhau ở tổng bên ngoài. 

Đối với ngân sách trống, hãy nhập`3 3 0`ngay lập tức trở lại`0`. Việc kiểm tra việc thực hiện`B <= 0`trước khi bắt đầu tìm kiếm ngưỡng, do đó nó không bao giờ cố gắng chia cho chi phí biên bằng 0 hoặc giả sử rằng thanh chi phí dương có thể được chọn. 

Khi mọi thanh có thể phù hợp, đầu vào`2 2 9`được xử lý trước khi tìm kiếm nhị phân. Tổng chi phí là 

[ 
\frac{2\cdot3}{2}\frac{2\cdot3}{2}=9, 
] 

bằng với ngân sách, vì vậy tất cả các thanh (2\cdot2=4) đều có giá cả phải chăng. Việc thoát sớm này cũng ngăn chặn việc tính toán ngưỡng không cần thiết khi câu trả lời chỉ đơn giản là tổng số cặp thứ nguyên có thể có. 

Đối với trường hợp một chiều`1 5 15`, chi phí có thể là (1,2,3,4,5), có tổng là (15). Việc kiểm tra khả năng chi trả hoàn chỉnh trả về (5). Trường hợp này rất hữu ích vì mã nhóm thương vẫn phải hoạt động khi một chiều chính xác là một và không có cấu trúc hai chiều để khai thác. 

Đối với trường hợp lớn`1000000 1000000 1`, thanh rẻ nhất có thể là (1\times1), có giá chính xác là (1). Do đó, câu trả lời là (1). Việc tìm kiếm không bao giờ cần liệt kê lưới hàng triệu triệu. Ngưỡng ngay lập tức nằm gần chi phí nhỏ nhất và các công thức được nhóm ngầm biểu thị phần còn lại của hình chữ nhật.
