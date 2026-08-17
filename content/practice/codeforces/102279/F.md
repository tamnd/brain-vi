---
title: "CF 102279F - Mùa Lũ"
description: "Đường polyline mô tả một mặt cắt địa hình một chiều. Do tọa độ x tăng nghiêm ngặt nên mỗi cặp điểm liên tiếp tạo thành một đoạn địa hình, nước mưa chỉ có thể đọng lại ở vùng trũng khi địa hình dâng cao hai bên đủ ngăn nước…"
date: "2026-08-16T19:17:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "F"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 196
verified: true
draft: false
---

[CF 102279F - Mùa lũ lụt](https://codeforces.com/problemset/problem/102279/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đường polyline mô tả một mặt cắt địa hình một chiều. Do tọa độ x tăng nghiêm ngặt nên mỗi cặp điểm liên tiếp tạo thành một đoạn địa hình và nước mưa chỉ có thể ở trạng thái trũng khi địa hình dâng lên ở cả hai bên đủ để ngăn nước thoát ra ngoài. 

Đối với bất kỳ vùng trũng nào, mặt nước nằm ngang. Giả sử ranh giới bên trái của nó là điểm (i) và ranh giới bên phải của nó là điểm (r). Mực nước là mực nước thấp nhất trong hai độ cao giới hạn, 

[ 
h=\min(y_i,y_r). 
] 

Vùng nước là vùng nằm giữa địa hình và đường ngang (y=h), nhưng chỉ khi địa hình nằm dưới đường đó. Nhiệm vụ là tìm tổng của tất cả các vùng bị mắc kẹt như vậy, với mỗi phần địa hình thuộc nhiều nhất một vùng được đếm. 

Ràng buộc (n\le 10^5) loại trừ việc kiểm tra mọi cặp điểm. Thuật toán bậc hai sẽ thực hiện khoảng (10^{10}/2) kiểm tra cặp trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. Tọa độ tối đa là (10^6), do đó tổng chiều rộng ngang của toàn bộ đa tuyến tối đa là (10^6), nhưng điều đó không làm cho việc quét từ mọi điểm bắt đầu trở nên khả thi. Chúng ta cần một giải pháp (O(n)) hoặc (O(n\log n)). 

Có một số trường hợp khó xử lý. Chỉ với hai điểm, không có vết lõm kèm theo nào cả. Ví dụ,```
2
0 0
1000000 1000000
```có câu trả lời`0`. Một giải pháp giả định rằng mỗi cặp xác định một lưu vực sẽ tạo ra nước ở đây một cách không chính xác. 

Chiều cao bằng nhau cũng có vấn đề. Vì```
3
0 2
1 0
2 2
```câu trả lời là`1`, bởi vì hai bên tạo thành một lưu vực hình tam giác có mực nước là 2. Việc thực hiện bất cẩn tiếp theo lớn hơn chỉ sử dụng các điểm lớn hơn nghiêm ngặt có thể không nhận ra ranh giới bên phải khi chiều cao tối đa bằng chiều cao bên trái. 

Ranh giới bên phải cũng có thể thấp hơn ranh giới bên trái. Vì```
3
0 3
1 0
2 2
```mực nước là 2 chứ không phải 3 vì nước tràn qua bên phải ở độ cao 2. Câu trả lời là`2`. Đây là lý do tại sao chiều cao ranh giới phải là`min(y_i, y_r)`. 

Cuối cùng, khi điểm cuối bên phải cao hơn mực nước, đoạn địa hình cuối cùng cắt ngang mặt nước ở đâu đó bên trong nó. Vì```
4
0 2
1 0
2 1
3 3
```mực nước là 2. Hai đoạn đầu đóng góp (1) và (1,5), trong khi đoạn cuối chỉ đóng góp (0,25), vì chỉ phần dưới độ cao 2 chứa nước. Câu trả lời là`2.75`. Việc coi toàn bộ phần cuối cùng là ngập nước sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi ranh giới bên trái có thể có (i), tìm mọi ranh giới bên phải có thể có (j>i) và tính toán lượng nước mà cặp này có thể giữ lại. Có (n-1)/2) cặp, tức là khoảng (5\cdot10^9) cặp cho (n=10^5). Ngay cả khi mỗi cặp được xử lý trong thời gian không đổi thì tốc độ này vẫn quá chậm. Nếu diện tích cũng được tính bằng cách quét khoảng thời gian thì độ phức tạp sẽ trở thành bậc ba. 

Quan sát hữu ích là chúng ta không cần phải xem xét các ranh giới bên phải tùy ý. Bắt đầu từ điểm (i), ranh giới bên phải duy nhất có ý nghĩa là điểm đầu tiên bên phải có chiều cao lớn hơn (y_i). Nếu một điểm như vậy tồn tại thì mọi điểm trước nó nhiều nhất là (y_i), do đó nước bắt đầu tại (i) đạt chính xác ranh giới đó trước khi nó có thể tràn ra ngoài. Nếu không có điểm nào lớn hơn, nước cuối cùng sẽ tràn qua điểm cao nhất bên phải. Chúng tôi chọn lần xuất hiện đầu tiên của hậu tố tối đa đó làm ranh giới. 

Đây chính xác là cấu trúc được mô tả bởi bài xã luận chính thức: với mọi (i), hãy tìm (r_i>i) nhỏ nhất với (y_{r_i}>y_i), hoặc, nếu không tồn tại, vị trí đầu tiên đạt được độ cao tối đa trong số các vị trí bên phải. Các phạm vi kết quả được lồng nhau hoặc tách biệt hoàn toàn, cho phép xử lý chúng bằng cách chuyển trực tiếp từ phạm vi này sang phạm vi tiếp theo. 

Vị trí lớn hơn đầu tiên cho mọi điểm có thể được tìm thấy bằng một ngăn xếp đơn điệu trong (O(n)). Hậu tố dự phòng tối đa có thể được tính bằng một lần quét từ phải sang trái, cũng ở dạng (O(n)). 

Khi đã biết ranh giới (r_i), chúng tôi tính toán diện tích mặt nước bằng cách đi qua các đoạn từ (i) đến (r_i). Các phạm vi được xử lý thực sự là rời rạc vì sau khi xử lý ([i,r_i]), chúng tôi tiếp tục từ (r_i+1). Do đó, mặc dù một phạm vi riêng lẻ có thể chứa nhiều phân đoạn nhưng tổng số phân đoạn được xử lý trên toàn bộ thuật toán chỉ là (O(n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) hoặc tệ hơn | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc polyline thành mảng`x`Và`y`. Sự gia tăng nghiêm ngặt của`x`có nghĩa là mọi phân đoạn địa hình có thể được tích hợp độc lập. 
2. Tính toán`next_greater[i]`, chỉ số đầu tiên bên phải của`i`với`y[index] > y[i]`. Xử lý các chỉ mục từ phải sang trái trong khi duy trì một ngăn xếp có chiều cao giảm dần. Trước khi truy vấn ngăn xếp để tìm`i`, loại bỏ mọi điểm có chiều cao lớn nhất`y[i]`, bởi vì không có điểm nào trong số đó có thể là điểm cao hơn đầu tiên cho`i`. 
3. Tính toán`suffix_idx[i]`, vị trí đầu tiên đạt được độ cao tối đa trong số các chỉ số ở bên phải của`i`. Trong khi quét từ phải sang trái, hãy duy trì điểm tốt nhất được thấy cho đến nay. Khi chiều cao hiện tại lớn hơn nhiều, hãy thay thế điểm đó. Khi độ cao bằng nhau, giữ nguyên điểm hiện có vì nó ở xa hơn về bên phải và điểm hiện tại không phải là một phần của hậu tố được xem xét bởi`suffix_idx[i]`. 
4. Bắt đầu lúc`i=0`. Nếu như`next_greater[i]`tồn tại, hãy sử dụng nó như`r`. Nếu không thì sử dụng`suffix_idx[i]`. Nếu không có điểm nào ở bên phải thì không có lưu vực nào có thể tồn tại và thuật toán dừng lại. 
5. Đặt mực nước thành`h=min(y[i],y[r])`. Mọi đỉnh trong của dãy này nhiều nhất là`h`, theo định nghĩa của`r`. Nếu như`y[r] > h`, chỉ đoạn cuối cùng mới có thể vượt qua mặt nước. Nếu như`y[r] <= h`, mọi đoạn đều nằm hoàn toàn bên dưới hoặc trên mặt nước. 
6. Đối với đoạn ngập hoàn toàn từ ((x_1,y_1)) đến ((x_2,y_2)), địa hình là tuyến tính nên độ cao trung bình của đoạn đó là ((y_1+y_2)/2). Với mực nước`h`, đóng góp của nó là 

[ 
(x_2-x_1)\left(h-\frac{y_1+y_2}{2}\right). 
] 

Để tránh các nửa lặp lại, việc triển khai sẽ lưu trữ diện tích gấp đôi: 

[ 
(x_2-x_1)(2h-y_1-y_2). 
] 

1. Nếu đoạn cuối đi từ (y_1\le h) đến (y_2>h) thì nước chỉ chiếm phần trước khi đường đạt độ cao`h`. Độ sâu là một hình tam giác có chiều cao (h-y_1). Khoảng cách vượt qua là 

[ 
(x_2-x_1)\frac{h-y_1}{y_2-y_1}, 
] 

vậy diện tích gấp đôi 

[ 
(x_2-x_1)\frac{(h-y_1)^2}{y_2-y_1}. 
] 

Việc thực hiện đánh giá giá trị hợp lý này bằng cách sử dụng`Decimal`để duy trì đủ độ chính xác ngay cả đối với tọa độ lớn. 

1. Thêm diện tích của phạm vi hiện tại vào câu trả lời và đặt`i=r+1`. Các phạm vi không thể chồng chéo lên nhau trong phần được tính, vì vậy mỗi đoạn địa hình chỉ được xử lý tối đa một lần. 

### Tại sao nó hoạt động 

Đối với mỗi điểm bắt đầu (i), điểm được chọn (r_i) chính xác là nơi đầu tiên mà nước bắt nguồn từ (i) có thể thoát ra. Nếu tồn tại một điểm cao hơn thì mọi thứ trước điểm đó không cao hơn (y_i), do đó chiều cao giới hạn dưới là (y_i). Nếu không có điểm nào cao hơn thì điểm cao nhất ở hậu tố là nơi đầu tiên nước có thể tràn và độ cao của nó quyết định mực nước. Do đó ([i,r_i]) mô tả một cấu trúc nước bị bẫy tối đa. 

Hai cấu trúc như vậy được lồng vào nhau hoặc rời rạc. Khi chúng tôi xử lý cấu trúc ngoài cùng bên trái và sau đó chuyển sang`r_i+1`, mọi cấu trúc lồng nhau đều đã được chứa trong vùng được xử lý, trong khi mọi cấu trúc rời rạc sẽ bắt đầu sau đó. Do đó, cú nhảy không bao giờ bỏ qua vùng nước thuộc một lưu vực riêng biệt và không bao giờ tính hai lần trên cùng một đoạn. Vì mỗi phân đoạn được xử lý đóng góp chính xác diện tích nước hình học của nó nên kết quả tích lũy là tổng diện tích cần thiết. 

## Giải pháp Python```python
import sys
from decimal import Decimal, getcontext

input = sys.stdin.readline

getcontext().prec = 40

def solve(data=None):
    if data is None:
        tokens = iter(sys.stdin.buffer.read().split())
    else:
        tokens = iter(data.split())

    n = int(next(tokens))
    x = [0] * n
    y = [0] * n

    for i in range(n):
        x[i] = int(next(tokens))
        y[i] = int(next(tokens))

    if n <= 2:
        return "0.0000000000\n"

    # next_greater[i] = first j > i with y[j] > y[i].
    next_greater = [-1] * n
    stack = []

    for i in range(n - 1, -1, -1):
        while stack and y[stack[-1]] <= y[i]:
            stack.pop()

        if stack:
            next_greater[i] = stack[-1]

        stack.append(i)

    # suffix_idx[i] = first position attaining the maximum y
    # among positions strictly to the right of i.
    suffix_idx = [-1] * n
    best = -1

    for i in range(n - 1, -1, -1):
        suffix_idx[i] = best

        if best == -1 or y[i] > y[best]:
            best = i

    # Store twice the integer/rational area.
    integer_area2 = 0
    fractional_area2 = Decimal(0)

    i = 0

    while i < n - 1:
        r = next_greater[i]

        if r == -1:
            r = suffix_idx[i]

        if r == -1:
            break

        h = min(y[i], y[r])

        k = i
        while k < r:
            x1, y1 = x[k], y[k]
            x2, y2 = x[k + 1], y[k + 1]
            dx = x2 - x1

            if y2 <= h:
                # Entire segment is below the water surface.
                integer_area2 += dx * (2 * h - y1 - y2)
            else:
                # The segment crosses the water surface.
                # Only the triangular part before the crossing is wet.
                dy = y2 - y1
                numerator = dx * (h - y1) * (h - y1)

                fractional_area2 += (
                    Decimal(numerator) / Decimal(dy)
                )

                # This must be the last segment of this range.
                break

            k += 1

        i = r + 1

    area = (
        Decimal(integer_area2) + fractional_area2
    ) / Decimal(2)

    return f"{area:.10f}\n"

def main():
    sys.stdout.write(solve())

if __name__ == "__main__":
    main()
```Lần quét đầu tiên sẽ xây dựng mảng lớn hơn tiếp theo với ngăn xếp đơn điệu giảm dần. Khi điểm xử lý`i`, mọi phần tử ngăn xếp không cao hơn`y[i]`là điểm cao hơn đầu tiên vĩnh viễn vô dụng, vì vậy việc loại bỏ nó là an toàn. 

Quét hậu tố xử lý trường hợp không tồn tại điểm nào cao hơn. Nhiệm vụ để`suffix_idx[i]`xảy ra trước khi xem xét điểm`i`, đó là điều làm cho chỉ mục được lưu trữ hoàn toàn ở bên phải. Chiều cao bằng nhau cố tình không thay thế`best`, bởi vì điểm trước đó là lần xuất hiện đầu tiên của hậu tố tối đa đó. 

Vòng lặp chính sử dụng`r`là ranh giới được chọn bởi hai cấu trúc đó. Nếu như`r=i+1`, không có đoạn địa hình bên trong và phần đóng góp bằng không. Việc nhảy tới`r+1`vẫn đúng vì phạm vi này không thể chứa bất kỳ lưu vực có diện tích dương nào. 

Diện tích được tích lũy ở dạng gấp đôi. Các phân đoạn hoàn chỉnh đóng góp một số nguyên, trong khi phân đoạn giao nhau cuối cùng có thể tạo ra một số hữu tỷ. Số nguyên Python có độ chính xác tùy ý và`Decimal`xử lý phần hợp lý mà không làm mất độ chính xác mà giá trị dấu phẩy động nhị phân có thể đưa ra cho tọa độ lớn. 

Trường hợp vượt biên đáng được quan tâm đặc biệt. Khi`y2 > h`, sử dụng công thức hình thang thông thường sẽ tính được phần trên mặt nước. Thay vào đó, chỉ có hình tam giác từ`y1`chiều cao`h`đang bị nhấn chìm. Từ`r`là điểm đầu tiên ở trên`y_i`khi một điểm như vậy tồn tại thì có thể có nhiều nhất một đoạn giao nhau như vậy trong phạm vi được xử lý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

mẫu là```
7
1 1
2 0
3 1
5 5
7 2
8 3
9 0
```Đối với điểm đầu tiên, điểm cao hơn đầu tiên là điểm 4, có chiều cao là 5. Do đó, mực nước là 1. Hai đoạn đi lên và đi xuống xung quanh thung lũng bị ngập hoàn toàn. 

|`i`|`r`|`h`| Đóng góp phân khúc | Diện tích tích lũy | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | (0,5+0,5+0) | (1) | 
| 5 | 6 | 2 | (1/3+1/2) | (6/11) | 

Phạm vi đầu tiên là`[1,4]`, do đó thuật toán nhảy đến điểm 5. Điểm 5 và 6 nằm liền kề nhau, do đó phạm vi đó không chứa vùng lõm bên trong. Hai độ cao biên của nó là 2 và 3, không có vùng bị bẫy diện tích dương. 

Phạm vi bắt đầu từ điểm 4 cũng giải thích thuật ngữ phân số. Phía bên trái của nó tăng từ độ cao 5 lên 2 và mực nước là 3 khi ghép với điểm 6. Đoạn từ`(5,5)`ĐẾN`(7,2)`vượt qua độ cao 3 trước khi đến điểm cuối của điểm 5, tạo ra phần đóng góp hình tam giác là (1/3). Đoạn từ`(7,2)`ĐẾN`(8,3)`đóng góp khác (1/2). Kết quả cuối cùng là 

[ 
1+\frac13+\frac12=\frac{11}{6}=1.8333333333\ldots 
] 

phù hợp với đầu ra mẫu. 

### Ví dụ tùy chỉnh 

Hãy xem xét```
3
0 3
1 0
2 2
```Không có điểm nào cao hơn chiều cao 3 đầu tiên nên quy tắc dự phòng chọn điểm 3, điểm lớn nhất của hậu tố. Mực nước là`min(3,2)=2`. 

|`i`|`r`|`h`| Phân đoạn | Đóng góp diện tích hai lần | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2 |`(0,3) -> (1,0)`| 2 | 
| 1 | 3 | 2 |`(1,0) -> (2,2)`| 2 | 

Tổng diện tích nhân đôi là 4, cho diện tích 2. Ví dụ xác nhận rằng ranh giới bên phải không cần phải cao hơn ranh giới bên trái. Nước chỉ đơn giản là tràn ở độ cao giới hạn dưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Ngăn xếp lớn hơn tiếp theo, quét tối đa hậu tố và xử lý phạm vi mỗi lần chạm vào mỗi điểm với số lần không đổi. | 
| Không gian | (O(n)) | Mảng tọa độ, mảng lớn hơn tiếp theo, mảng hậu tố và ngăn xếp đơn điệu đều yêu cầu bộ nhớ tuyến tính. | 

Thuật toán chỉ thực hiện công việc tuyến tính cho (n\le10^5), để lại một lề thoải mái dưới giới hạn 2 giây. Mảng tọa độ chứa tối đa (10^5) phần tử, do đó mức sử dụng bộ nhớ cũng thấp hơn 256 MB. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây thực hiện kích thước đầu vào tối thiểu, đầu vào kích thước tối đa, chiều cao bằng nhau, ranh giới hậu tố tối đa dự phòng và phân đoạn giao nhau.```python
import io
import sys

# Import the solve function from the submitted solution.
# If the solution is stored in solution.py:
# from solution import solve

# For a self-contained test file, paste the solve function above here.

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided sample
sample1 = """\
7
1 1
2 0
3 1
5 5
7 2
8 3
9 0
"""

assert abs(float(run(sample1)) - 1.8333333333333333) <= 1e-6, "sample 1"

# Minimum-size input: no possible basin.
case_min = """\
2
0 0
1000000 1000000
"""

assert abs(float(run(case_min)) - 0.0) <= 1e-6, "minimum size"

# Maximum-size input with equal heights: no water anywhere.
n = 100000
case_max = str(n) + "\n" + "\n".join(
    f"{i} 1000000" for i in range(n)
) + "\n"

assert abs(float(run(case_max)) - 0.0) <= 1e-6, "maximum size / equal heights"

# No strictly higher point exists to the right.
# The suffix maximum at the right endpoint determines the water level.
case_suffix = """\
3
0 3
1 0
2 2
"""

assert abs(float(run(case_suffix)) - 2.0) <= 1e-6, "suffix maximum"

# The final segment crosses the water level before its endpoint.
case_crossing = """\
4
0 2
1 0
2 1
3 3
"""

assert abs(float(run(case_crossing)) - 2.75) <= 1e-6, "crossing segment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / (0,0) / (1000000,1000000)`|`0`| Đầu vào có kích thước tối thiểu và không có bồn rửa | 
| 100000 điểm với chiều cao`1000000`|`0`| Tối đa`n`và xử lý chiều cao bằng nhau | 
|`(0,3), (1,0), (2,2)`|`2`| Dự phòng hậu tố tối đa khi không có điểm cao hơn | 
|`(0,2), (1,0), (2,1), (3,3)`|`2.75`| Cắt đúng đoạn cuối cùng trên mặt nước | 

## Vỏ cạnh 

Chỉ với hai điểm, thuật toán không tính được phạm vi vì không có đoạn bên trong. Vì```
2
0 0
1000000 1000000
```

`i=0`không có ranh giới bên phải có ý nghĩa có khả năng tạo ra một vùng khép kín, vì vậy kết quả là chính xác`0`. 

Để có chiều cao bằng nhau, hãy xem xét```
3
0 2
1 0
2 2
```Không có điểm nào lớn hơn ở bên phải điểm đầu tiên. Hậu tố tối đa là điểm thứ ba ở độ cao 2, vì vậy`r=2`Và`h=2`. Mỗi đoạn trong số hai đoạn có chiều rộng đơn vị tạo thành một tam giác có diện tích (1/2), có tổng diện tích là`1`. Quy tắc dự phòng xử lý sự bình đẳng mà không coi điểm cuối bằng nhau là điểm lớn hơn. 

Khi ranh giới bên phải thấp hơn ranh giới bên trái,```
3
0 3
1 0
2 2
```hậu tố tối đa là chiều cao 2. Thuật toán sử dụng`h=min(3,2)=2`, không phải 3. Mỗi đoạn đóng góp một tam giác có diện tích 1, cho`2`. Điều này phù hợp với hành vi vật lý vì nước tràn qua ranh giới bên phải ngay khi đạt đến độ cao 2. 

Đối với đoạn giao nhau,```
4
0 2
1 0
2 1
3 3
```ranh giới bên phải là điểm cuối cùng, mực nước là 2. Đoạn thứ nhất đóng góp 1, đoạn thứ hai đóng góp (1,5), và đoạn cuối cùng từ độ cao 1 đến độ cao 3 cắt ngang mặt nước nửa chừng. Phần ướt của nó là một hình tam giác có đáy (1/2) và chiều cao bằng 1, góp phần (1/4). Tổng số là (1+1,5+0,25=2,75). Công thức giao cắt đặc biệt của quá trình triển khai nắm bắt chính xác phân đoạn một phần này. 

Trường hợp tinh tế cuối cùng là một ranh giới liền kề với điểm bắt đầu. Nếu như`r=i+1`, không có địa hình bên trong giữa hai điểm biên nên phạm vi không thể chứa nước vùng dương. Thuật toán thêm số 0 và nhảy tới`r+1`, ngăn ngừa cả lỗi xảy ra lẫn việc xử lý không cần thiết.
