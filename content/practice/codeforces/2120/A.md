---
title: "CF 2120A - Hình vuông hình chữ nhật"
description: "Chúng ta có ba hình chữ nhật thẳng hàng với trục. Kích thước của chúng đã được sắp xếp sao cho $$l3 le l2 le l1$$ và $$b3 le b2 le b1.$$ Không thể xoay hình chữ nhật."
date: "2026-06-08T03:52:24+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 2120
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1033 (Div. 2) and CodeNite 2025"
rating: 800
weight: 2120
solve_time_s: 116
verified: true
draft: false
---

[CF 2120A - Hình vuông hình chữ nhật](https://codeforces.com/problemset/problem/2120/A) 

**Đánh giá:** 800 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba hình chữ nhật thẳng hàng với trục. Kích thước của chúng đã được sắp xếp sao cho$$l_3 \le l_2 \le l_1$$Và$$b_3 \le b_2 \le b_1.$$Các hình chữ nhật không thể xoay được. Chúng ta phải xác định xem liệu cả ba hình chữ nhật có thể được đặt mà không chồng lên nhau sao cho chúng cùng nhau tạo thành một hình vuông hay không. 

Kích thước rất nhỏ, nhiều nhất là 100. Số lượng trường hợp thử nghiệm nhiều nhất là 100. Điều này có nghĩa là chúng ta không cần tối ưu hóa phức tạp. Ngay cả việc kiểm tra một số cấu hình hình học cho mỗi trường hợp thử nghiệm cũng thực sự là một khoảng thời gian không đổi. 

Khó khăn chính không phải là hiệu suất mà là xác định mọi cách có thể để ba hình chữ nhật không thể xoay có thể kết hợp thành hình vuông. 

Một lỗi phổ biến là chỉ kiểm tra xem tổng diện tích có phải là một hình vuông hoàn hảo hay không. Diện tích bằng nhau là cần thiết, nhưng vẫn chưa đủ. 

Coi như:```
1
3 3 3 3 2 1
```Tổng diện tích là$$9+9+2=20,$$không phải là hình vuông nên câu trả lời rõ ràng là KHÔNG. 

Những ví dụ tinh tế hơn tồn tại khi diện tích là hình vuông nhưng không thể sắp xếp được. 

Ví dụ:```
1
8 5 3 5 3 3
```Tổng diện tích là$$40+15+9=64=8^2.$$Một giải pháp bất cẩn có thể trả lời CÓ ngay lập tức. Câu trả lời đúng là KHÔNG vì độ dài cạnh không cho phép hình chữ nhật xếp cạnh nhau$8 \times 8$quảng trường. 

Một trường hợp dễ bỏ sót khác xảy ra khi cả ba hình chữ nhật phải được xếp chồng lên nhau theo một hướng.```
1
5 3 5 1 5 1
```Tất cả các chiều rộng đều bằng nhau. Đặt chúng chồng lên nhau tạo ra một$5 \times 5$vuông, vì vậy câu trả lời là CÓ. Bất kỳ giải pháp nào chỉ tìm kiếm các bố cục phức tạp hơn sẽ từ chối trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Một tư duy vũ phu là một điểm khởi đầu tốt. Vì chỉ có ba hình chữ nhật, người ta có thể thử liệt kê tất cả các vị trí tương đối có thể có trên một lưới và kiểm tra xem liệu liên kết có tạo thành hình vuông hay không. 

Việc tìm kiếm như vậy sẽ đúng vì mọi sắp xếp hợp lệ cuối cùng sẽ được kiểm tra. Vấn đề là vị trí hình học tạo ra nhiều tọa độ và kiểm tra chồng chéo. Mặc dù những hạn chế thực tế là nhỏ nhưng cách tiếp cận này phức tạp một cách không cần thiết. 

Quan sát quan trọng là chỉ có ba hình chữ nhật và phép quay bị cấm. Bất kỳ việc lát hình vuông nào bằng ba hình chữ nhật đều phải có cấu trúc rất hạn chế. 

Gọi độ dài cạnh hình vuông là$S$. Đầu tiên, tổng diện tích phải bằng$S^2$. Nếu diện tích không phải là một hình vuông hoàn hảo, câu trả lời ngay lập tức là KHÔNG. 

Bây giờ hãy xem xét ranh giới bên ngoài của hình vuông. Vì chỉ có ba hình chữ nhật được sử dụng nên một hình chữ nhật phải chạm vào toàn bộ cạnh của hình vuông. Điều đó chỉ tạo ra hai bố cục cấp cao nhất có thể. 

Khả năng đầu tiên là cả ba hình chữ nhật đều trải rộng hết chiều rộng của hình vuông và được xếp chồng lên nhau theo chiều dọc hoặc đối xứng tất cả đều trải dài hết chiều cao và được xếp chồng lên nhau theo chiều ngang. 

Khả năng thứ hai là một hình chữ nhật chiếm trọn một dải dọc theo một cạnh của hình vuông. Sau đó, hai hình chữ nhật còn lại phải lấp đầy chính xác hình chữ nhật còn lại. Vì chỉ còn lại hai hình chữ nhật nên chúng chỉ có thể lấp đầy khoảng trống đó bằng cách xếp chồng lên nhau hoặc đặt cạnh nhau. 

Vì các kích thước đã được sắp xếp sẵn và chỉ có ba hình chữ nhật nên việc kiểm tra một số cấu hình này là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vị trí vũ phu | Bảng liệt kê hình học lớn | O(1) | Phức tạp không cần thiết | 
| Phân tích trường hợp hình học tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng diện tích:$$A=l_1b_1+l_2b_2+l_3b_3.$$1. Hãy để$$S=\sqrt{A}.$$Nếu như$S^2 \ne A$, tạo thành một hình vuông là không thể, vì vậy hãy trả lời KHÔNG. 

1. Kiểm tra xem cả ba hình chữ nhật có thể xếp chồng lên nhau theo chiều dọc hay không. 

Điều này đòi hỏi$$l_1=l_2=l_3=S$$Và$$b_1+b_2+b_3=S.$$Nếu đúng thì trả lời CÓ. 

1. Kiểm tra xem cả ba hình chữ nhật có thể xếp chồng lên nhau theo chiều ngang hay không. 

Điều này đòi hỏi$$b_1=b_2=b_3=S$$Và$$l_1+l_2+l_3=S.$$Nếu đúng thì trả lời CÓ. 

1. Hãy thử coi mỗi hình chữ nhật là hình chữ nhật chiếm toàn bộ chiều rộng của hình vuông. 

Giả sử hình chữ nhật$i$có kích thước$(l,b)$. 

Nó phải thỏa mãn$$l=S.$$Sau khi đặt xong chiều cao còn lại là$$H=S-b.$$Hai hình chữ nhật còn lại phải điền chính xác vào một$S \times H$hình chữ nhật. 

Vì chỉ còn lại hai hình chữ nhật nên chúng phải:$$h_1=h_2=H,\qquad w_1+w_2=S$$hoặc$$w_1=w_2=S,\qquad h_1+h_2=H.$$Nếu một trong hai điều kiện được giữ, hãy trả lời CÓ. 

1. Một cách đối xứng, hãy thử từng hình chữ nhật dưới dạng một dải có chiều cao đầy đủ. 

Điều này đòi hỏi$$b=S.$$Chiều rộng còn lại là$$W=S-l.$$Hai hình chữ nhật còn lại phải điền chính xác vào một$W \times S$hình chữ nhật bằng cách sử dụng các dấu kiểm hai hình chữ nhật giống nhau. 

1. Nếu không có cấu hình nào ở trên hoạt động, hãy trả lời KHÔNG. 

### Tại sao nó hoạt động 

Bất kỳ việc xếp hình vuông nào bằng ba hình chữ nhật thẳng hàng sẽ tạo ra một phân vùng của hình vuông. Một hình chữ nhật phải chạm vào toàn bộ một cạnh của hình vuông, nếu không mỗi cạnh sẽ được chia thành nhiều hình chữ nhật và cần có ít nhất bốn hình chữ nhật. 

Loại bỏ hình chữ nhật đó để lại một vùng hình chữ nhật. Vì chỉ còn lại hai hình chữ nhật nên hình chữ nhật còn lại chỉ có thể được phân chia bằng một đường cắt thẳng, theo chiều ngang hoặc chiều dọc. 

Thuật toán kiểm tra chính xác những khả năng này. Mỗi ô vuông hợp lệ thuộc về một trong số chúng và mọi cấu hình được chấp nhận rõ ràng tạo thành một hình vuông không chồng chéo. Do đó thuật toán vừa đầy đủ vừa chính xác. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

def can_form_square(rects):
    area = sum(l * b for l, b in rects)
    s = isqrt(area)

    if s * s != area:
        return False

    # all stacked vertically
    if all(l == s for l, b in rects) and sum(b for l, b in rects) == s:
        return True

    # all stacked horizontally
    if all(b == s for l, b in rects) and sum(l for l, b in rects) == s:
        return True

    for i in range(3):
        l, b = rects[i]
        others = [rects[j] for j in range(3) if j != i]

        # full-width strip
        if l == s:
            h = s - b
            (l1, b1), (l2, b2) = others

            if b1 == h and b2 == h and l1 + l2 == s:
                return True

            if l1 == s and l2 == s and b1 + b2 == h:
                return True

        # full-height strip
        if b == s:
            w = s - l
            (l1, b1), (l2, b2) = others

            if l1 == w and l2 == w and b1 + b2 == s:
                return True

            if b1 == s and b2 == s and l1 + l2 == w:
                return True

    return False

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        l1, b1, l2, b2, l3, b3 = map(int, input().split())

        rects = [(l1, b1), (l2, b2), (l3, b3)]

        ans.append("YES" if can_form_square(rects) else "NO")

    sys.stdout.write("\n".join(ans))

solve()
```Phần đầu tiên tính tổng diện tích và xác minh rằng đó là một hình vuông hoàn hảo. Nếu không có sự kiểm tra này, những trường hợp không thể thực hiện được có thể được chấp nhận sau này. 

Hai bước kiểm tra tiếp theo xử lý các bố cục đơn giản nhất trong đó mọi hình chữ nhật đều trải dài toàn bộ chiều rộng hoặc toàn bộ chiều cao của hình vuông cuối cùng. 

Vòng lặp sau đó coi mỗi hình chữ nhật là dải bên ngoài tiềm năng. Khi dải đó được cố định, chỉ còn lại hai hình chữ nhật. Hai hình chữ nhật có thể phân chia một hình chữ nhật chỉ theo hai cách, chia ngang hoặc chia dọc. Các đẳng thức thứ nguyên tương ứng được kiểm tra trực tiếp. 

Việc triển khai không bao giờ dựa vào căn bậc hai dấu phẩy động. sử dụng`isqrt`tránh hoàn toàn các vấn đề chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3 5 1 5 1
```Tổng diện tích:$$15+5+5=25.$$Cạnh vuông:$$S=5.$$| Bước | Tình trạng | Kết quả | 
| --- | --- | --- | 
| Diện tích hình vuông hoàn hảo |$25=5^2$| Có | 
| Ngăn xếp dọc | chiều rộng = 5, tổng chiều cao = 3+1+1=5 | Có | 
| Trả lời | | CÓ | 

Điều này thể hiện cấu hình xếp chồng thuần túy. Mỗi hình chữ nhật trải dài toàn bộ chiều rộng của hình vuông. 

### Ví dụ 2 

đầu vào:```
8 5 3 5 3 3
```| Bước | Giá trị | 
| --- | --- | 
| Khu vực | 64 | 
| Cạnh vuông | 8 | 

Kiểm tra bố cục: 

| Kiểm tra | Kết quả | 
| --- | --- | 
| Ngăn xếp dọc | Thất bại | 
| Ngăn xếp ngang | Thất bại | 
| Hình chữ nhật 1 dạng dải | Các kích thước còn lại không tương thích | 
| Hình chữ nhật 2 dạng dải | Không thể | 
| Hình chữ nhật 3 dạng dải | Không thể | 

Câu trả lời cuối cùng:```
NO
```Ví dụ này cho thấy chỉ diện tích thôi là chưa đủ. Mặc dù$64=8^2$, độ dài cạnh không cho phép ốp lát hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một số lần kiểm tra hình học cố định | 
| Không gian | O(1) | Chỉ sử dụng một vài biến | 

Mỗi trường hợp thử nghiệm yêu cầu đánh giá một số lượng công thức không đổi. Với tối đa 100 test case, thời gian chạy không đáng kể so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isqrt

def run(inp: str) -> str:
    def can_form_square(rects):
        area = sum(l * b for l, b in rects)
        s = isqrt(area)

        if s * s != area:
            return False

        if all(l == s for l, b in rects) and sum(b for l, b in rects) == s:
            return True

        if all(b == s for l, b in rects) and sum(l for l, b in rects) == s:
            return True

        for i in range(3):
            l, b = rects[i]
            others = [rects[j] for j in range(3) if j != i]

            if l == s:
                h = s - b
                (l1, b1), (l2, b2) = others

                if b1 == h and b2 == h and l1 + l2 == s:
                    return True

                if l1 == s and l2 == s and b1 + b2 == h:
                    return True

            if b == s:
                w = s - l
                (l1, b1), (l2, b2) = others

                if l1 == w and l2 == w and b1 + b2 == s:
                    return True

                if b1 == s and b2 == s and l1 + l2 == w:
                    return True

        return False

    data = inp.strip().splitlines()
    t = int(data[0])
    out = []

    for i in range(1, t + 1):
        vals = list(map(int, data[i].split()))
        rects = [(vals[0], vals[1]), (vals[2], vals[3]), (vals[4], vals[5])]
        out.append("YES" if can_form_square(rects) else "NO")

    return "\n".join(out)

# provided sample
assert run(
"""5
100 100 10 10 1 1
5 3 5 1 5 1
2 3 1 2 1 1
8 5 3 5 3 3
3 3 3 3 2 1
"""
) == """NO
YES
YES
NO
NO"""

# minimum dimensions
assert run(
"""1
1 1 1 1 1 1
"""
) == "NO"

# all equal rectangles forming a square
assert run(
"""1
2 2 2 2 2 2
"""
) == "NO"

# vertical stacking
assert run(
"""1
4 2 4 1 4 1
"""
) == "YES"

# perfect-square area but impossible layout
assert run(
"""1
8 5 3 5 3 3
"""
) == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1, 1×1, 1×1 | KHÔNG | Kích thước nhỏ nhất | 
| 2×2, 2×2, 2×2 | KHÔNG | Hình chữ nhật hoàn toàn bằng nhau không tự động hoạt động | 
| 4×2, 4×1, 4×1 | CÓ | Xếp chồng theo chiều dọc thuần túy | 
| 8×5, 3×5, 3×3 | KHÔNG | Diện tích vuông hoàn hảo nhưng hình học không thể | 

## Vỏ cạnh 

Hãy xem xét:```
1
5 3 5 1 5 1
```Diện tích là$25$, Vì thế$S=5$. Tất cả các hình chữ nhật có chiều rộng là 5 và tổng chiều cao của chúng là 5. Thuật toán chấp nhận trong quá trình kiểm tra ngăn xếp dọc và trả về CÓ. Giải pháp chỉ tìm kiếm bố cục hình chữ L sẽ từ chối nó một cách không chính xác. 

Coi như:```
1
8 5 3 5 3 3
```Diện tích là$64$, cho$S=8$. Mọi cấu hình hình học được kiểm tra bằng thuật toán đều thất bại. Câu trả lời là KHÔNG mặc dù diện tích đó là một hình vuông hoàn hảo. Điều này ngăn ngừa lỗi phổ biến khi sử dụng diện tích làm tiêu chí duy nhất. 

Coi như:```
1
2 3 1 2 1 1
```Diện tích là$$6+2+1=9,$$Vì thế$S=3$. các$2 \times 3$hình chữ nhật chiếm một dải chiều cao đầy đủ. Chiều rộng còn lại là 1. Các hình chữ nhật khác là$1 \times 2$Và$1 \times 1$, xếp chồng lên nhau theo chiều dọc để lấp đầy một$1 \times 3$vùng đất. Thuật toán phát hiện điều này trong trường hợp dải chiều cao đầy đủ và trả về CÓ. 

Coi như:```
1
100 100 10 10 1 1
```Diện tích là$$10101,$$đó không phải là một hình vuông hoàn hảo. Thuật toán loại bỏ ngay lập tức trước khi thực hiện bất kỳ kiểm tra hình học nào. Điều này xử lý các kích thước lớn nhất một cách an toàn và hiệu quả.
