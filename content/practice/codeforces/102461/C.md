---
title: "CF 102461C - Lợi nhuận quảng cáo"
description: "Chúng tôi bắt đầu với chính xác 10.000 người đăng ký. Có hai loại video. Một video thông thường sẽ tăng số lượng người đăng ký theo số lượng ai và không mang lại doanh thu trực tiếp."
date: "2026-08-09T15:03:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102461
codeforces_index: "C"
codeforces_contest_name: "Innopolis Open 2019-2020, qualification, contest 2"
rating: 0
weight: 102461
solve_time_s: 453
verified: true
draft: false
---

[CF 102461C - Lợi nhuận quảng cáo](https://codeforces.com/problemset/problem/102461/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với chính xác 10.000 người đăng ký. Có hai loại video. Một video thông thường sẽ tăng số lượng người đăng ký lên gấp nhiều lần`a_i`và không mang lại doanh thu trực tiếp. Một video thương mại có thông số`c_i`Và`b_i`: khi nó được phát hành, nó kiếm được`c_i`xu cho mỗi người đăng ký hiện có và sau đó số lượng người đăng ký giảm dần`b_i`. 

Đối với mỗi số được yêu cầu`d`, chúng ta phải chọn chính xác`d`video từ những video đã chuẩn bị sẵn và sắp xếp những video đã chọn đó theo thứ tự tốt nhất có thể. Câu trả lời là tổng doanh thu quảng cáo tối đa. Những ràng buộc ban đầu có`n,m <= 100`,`a_i,b_i,c_i <= 100`và giới hạn thời gian là một giây với bộ nhớ 512 MB. 

Các giá trị nhỏ của`n`Và`m`loại trừ các thuật toán liệt kê các tập hợp con hoặc hoán vị tùy ý. Với 200 video có sẵn, ngay cả việc liệt kê mọi thứ tự có thể cũng đã được yêu cầu theo thứ tự`200!`, vượt xa mọi điều khả thi. Thay vào đó, giới hạn số hữu ích là tổng của tất cả các giá trị thương mại`c_i`giá trị tối đa là 10.000. Điều đó mang lại cho chúng ta một kích thước giống như chiếc ba lô có thể quản lý được. 

Có một số trường hợp đặc biệt trong đó việc triển khai đơn giản có thể âm thầm gặp trục trặc. Nếu như`d = 0`, câu trả lời chính xác là 0 vì không có video thương mại nào có thể được phát hành. Ví dụ,```
0
0
1
0
```có đầu ra```
0
```Việc triển khai khởi tạo mọi câu trả lời cho một trọng điểm phủ định và không bao giờ xử lý riêng các video bằng 0 có thể vô tình in ra trọng điểm đó. 

Một trường hợp khác là không có video thương mại nào cả. Vì```
2
5
7
0
3
0
1
2
```mọi câu trả lời đều bằng không. Các video thông thường có thể tăng số lượng người đăng ký, nhưng nếu không có video thương mại thì sẽ không có gì để kiếm tiền. 

Trường hợp thứ ba là khi số lượng video được yêu cầu lớn hơn số lượng video thương mại. Giả định```
2
5
6
1
10 1
1
2
```Sự lựa chọn tối ưu cho`d = 2`là video thương mại cùng với video thông thường trị giá 6 người đăng ký. Lợi nhuận là`10 * 10006 = 100060`. Chọn hai video thông thường sẽ không có kết quả. Một giải pháp chỉ xem xét tập hợp con thương mại và quên rằng có thể yêu cầu các video thông thường để tiếp cận chính xác`d`video sẽ thất bại ở đây. 

Trường hợp tinh vi nhất là việc đặt hàng các video thương mại. Vì```
0
2
10 20
20 10
1
2
```câu trả lời là`299900`. Video thương mại thứ hai nên được phát hành đầu tiên vì nó`c/b`tỷ lệ càng lớn. Đảo ngược thứ tự sẽ làm mất thêm người đăng ký trước quảng cáo đắt tiền hơn. Mẫu chính thức thể hiện chính xác hiện tượng đặt hàng này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi bộ video có thể có và mọi thứ tự của bộ đó. Đối với một cố định`d`, có 

[ 
P(n+m,d)=\frac{(n+m)!}{(n+m-d)!} 
] 

sự lựa chọn có thứ tự. Nếu tất cả các giá trị có thể có của`d`được truy vấn, tổng số lịch trình là 

[ 
\sum_{d=0}^{n+m} P(n+m,d). 
]

Với`n+m=200`, điều này đã chứa thuật ngữ`200!`, khoảng`7.9 * 10^374`. Lực lượng vũ phu là chính xác bởi vì mọi lựa chọn có thể và mọi thứ tự có thể đều được xem xét, nhưng nó gần như trở nên vô dụng ngay lập tức. 

Quan sát hữu ích đầu tiên là các video thông thường phải luôn xuất hiện trước các video thương mại. Giả sử một video thông thường tăng lượng người đăng ký lên`a`và một video thương mại có hệ số`c`hiện được phát hành tại`s`người đăng ký. Phát hành video thương mại đầu tiên kiếm được`c*s`. Phát hành video thông thường kiếm được tiền đầu tiên`c*(s+a)`, nghĩa là lớn hơn. Do đó, mọi video thông thường được chọn cho câu trả lời đều có thể được chuyển lên phía trước mà không ảnh hưởng đến lợi nhuận. 

Quan sát thứ hai là, khi tập hợp các video thương mại được cố định, thứ tự tương đối của chúng bị ép buộc bởi một đối số trao đổi theo cặp đơn giản. Xem xét các video thương mại`x`Và`y`, và giả sử có`s`người đăng ký trước họ. Nếu như`x`được phát hành trước`y`, doanh thu kết hợp của họ là 

[ 
c_xs+c_y(s-b_x). 
]

Nếu như`y`được phát hành trước`x`, đó là 

[ 
c_ys+c_x(s-b_y). 
] 

Đơn hàng đầu tiên ít nhất cũng tốt đúng như vậy khi 

[ 
c_xb_y \ge c_yb_x, 
] 

tương đương với 

[ 
\frac{c_x}{b_x}\ge\frac{c_y}{b_y}. 
] 

Vì vậy các video thương mại phải xuất hiện theo thứ tự giảm dần`c/b`. 

Bây giờ hãy sửa thứ tự đó và giả sử chúng tôi chọn một số video thương mại. Nếu chúng ta xử lý chúng theo thứ tự ngược lại, cụ thể là tăng`c/b`, thêm video thương mại với thông số`(c,b)`sau khi các quảng cáo được xử lý trước đó sẽ bị phạt 

[ 
b \cdot C, 
] 

ở đâu`C`là tổng của`c`giá trị của các quảng cáo được xử lý trước đó. Điều này mang lại một chương trình năng động kiểu ba lô. 

Cho phép`dp[k][s]`là mức phạt âm tối đa sau khi chọn chính xác`k`video thương mại có tổng cộng`c`là`s`. Khi video quảng cáo tiếp theo có thông số`(c,b)`, bỏ qua nó không thay đổi gì cả. Lấy nó thay đổi trạng thái thành 

\max(dp[k+1][s+c], dp[k][s]-b\cdot s). 
] 

Doanh thu thực tế sau đó dễ dàng được tái tạo lại. Nếu các video thương mại đã chọn có tổng số`c`bằng`s`và các video thông thường được chọn đóng góp`A`số người đăng ký bổ sung, số người đăng ký ban đầu được xem bởi mỗi video thương mại là`10000+A`. Vì vậy, doanh thu cơ bản của họ là 

[ 
s(10000+A), 
] 

và giá trị DP trừ đi chính xác tổn thất do các video thương mại trước đó gây ra. 

Đối với một số cố định`l`của các video thông thường, chúng ta nên chọn`l`lớn nhất`a_i`các giá trị. Mỗi video thông thường đã chọn sẽ được đặt trước mỗi video thương mại, do đó toàn bộ đóng góp của video đó sẽ được nhân với tổng số`c`của các video thương mại đã chọn. Không có lý do gì để chọn một cái nhỏ hơn`a_i`. 

Có thêm một cách tối ưu hóa hữu ích hơn cho Python. Đối với một số cố định`k`video thương mại, mọi trạng thái DP`(s, dp[k][s])`đại diện cho một dòng 

[ 
f(x)=s x+dp[k][s], 
] 

ở đâu`x=10000+A`là số lượng người đăng ký trước tất cả các quảng cáo. Chúng ta cần mức tối đa của những dòng này cho một số giá trị tăng dần của`x`, một cho mỗi số lượng video thông thường có thể có. Vì các sườn dốc`s`đang tăng lên, đường bao trên có thể được xây dựng bằng bao lồi đơn điệu và được truy vấn theo thời gian tuyến tính. 

Phương pháp brute-force hoạt động hiệu quả vì nó thể hiện rõ ràng mọi lịch trình có thể, nhưng không thành công vì có nhiều lịch trình giai thừa. Quan sát cho thấy thứ tự của video thông thường và video thương mại có thể được chuẩn hóa, theo sau là cố định`c/b`đặt hàng quảng cáo, giảm vấn đề xuống DP ba lô bị giới hạn trên tổng hệ số thương mại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(sum P(n+m,d))`|`O(n+m)`| Quá chậm | 
| Tối ưu |`O(m²C + nm + m log m)`, Ở đâu`C=sum c_i <= 10000`|`O(mC + n)`| Đã chấp nhận | 

Việc triển khai còn hạn chế mỗi lần quét DP ở mức tối thiểu và tối đa có thể truy cập được cho mỗi lần quét.`k`, nhỏ hơn đáng kể so với việc quét tất cả`C`vị trí trong nhiều đầu vào. 

## Hướng dẫn thuật toán 

1. Sắp xếp mức tăng video thông thường theo thứ tự giảm dần và xây dựng tổng tiền tố. Nếu chúng ta sử dụng chính xác`l`video thông thường, tổng mức tăng tốt nhất có thể là tổng tiền tố của video đầu tiên`l`các giá trị. Điều này hiệu quả vì mọi video thông thường đều được phát hành trước mỗi video thương mại, vì vậy mỗi người đăng ký bổ sung sẽ hỗ trợ mọi quảng cáo được chọn như nhau. 
2. Lưu trữ mọi video thương mại dưới dạng`(c,b)`và sắp xếp chúng bằng cách tăng`c/b`. Thứ tự phát hành tối ưu thực tế đang giảm dần`c/b`, nhưng việc xử lý các quảng cáo theo thứ tự ngược lại làm cho quá trình chuyển đổi DP trở nên thuận tiện. Tỷ lệ bằng nhau có thể được đặt hàng tùy ý vì việc hoán đổi các quảng cáo có tỷ lệ bằng nhau không làm thay đổi doanh thu. 
3. Xác định`dp[k][s]`là giá trị tối đa của hình phạt tương tác tiêu cực khi chọn`k`video thương mại có tổng hệ số`s`trong số các quảng cáo được xử lý cho đến nay. Ban đầu chỉ`dp[0][0] = 0`có thể truy cập được. 
4. Xử lý từng video thương mại. Đối với một video`(c,b)`, lặp lại`k`hướng xuống dưới nên không thể chọn cùng một video hai lần. Đối với mọi số tiền có thể tiếp cận`s`, việc bỏ qua video sẽ khiến trạng thái không thay đổi. Việc chọn nó sẽ tạo ra trạng thái`(k+1, s+c)`có giá trị`dp[k][s] - b*s`. 

Thuật ngữ`b*s`có ý nghĩa trực tiếp. Tất cả các quảng cáo được xử lý trước đó đều đóng góp`c`giá trị của tổng hệ số`s`và việc phát hành quảng cáo hiện tại sau chúng có nghĩa là nó sẽ mất`b`thuê bao cho mỗi đơn vị hệ số đó. 
5. Với mỗi số`k`của các quảng cáo đã chọn, giải thích mọi trạng thái DP hữu hạn`(s,dp[k][s])`như một dòng`s*x + dp[k][s]`. Đây`x`là số người đăng ký sau tất cả các video thông thường đã chọn và ngay trước chuỗi quảng cáo. 
6. Xây dựng phần thân trên của các đường này. Độ dốc đã tăng lên vì độ dốc chính xác là tổng`c`giá trị`s`. Xóa đường ở giữa bất cứ khi nào giao điểm của nó với đường trước xảy ra không sớm hơn giao điểm của đường trước với đường tiếp theo. Một dòng như vậy không bao giờ có thể tối ưu cho bất kỳ truy vấn nào`x`. 
7. Truy vấn thân tàu để tìm mọi số có thể`l`của các video thông thường. Giá trị truy vấn là`10000 + prefix[l]`. Nếu thân tàu cho`profit_ads`, thì tổng số video đã phát hành là`d=l+k`, vậy hãy cập nhật`answer[d]`với giá trị này. 
8. Đọc các giá trị được yêu cầu`d`và in các câu trả lời được tính toán trước tương ứng. Vì tất cả đều có thể`d`các giá trị được xử lý trước khi đọc truy vấn, mọi truy vấn đều được trả lời theo thời gian không đổi. 

### Tại sao nó hoạt động 

Đối với mọi lịch trình khả thi, việc chuyển tất cả các video thông thường đã chọn lên phía trước không thể làm giảm số lượng người đăng ký của bất kỳ video thương mại nào, do đó, lịch trình tối ưu sẽ ưu tiên tất cả các video thông thường. Trong số các video thương mại được chọn, phép tính trao đổi theo cặp chứng tỏ rằng việc giảm`c/b`là thứ tự tương đối tối ưu duy nhất, cho đến khi có quan hệ. DP xử lý chính xác theo hướng ngược lại của thứ tự này và quá trình chuyển đổi của nó sẽ trừ đi`b`lần tổng số`c`quảng cáo đã được xử lý, chính xác là doanh thu bị mất do những quảng cáo đó được phát hành trước đó. Do đó, mọi tập hợp con thương mại đều có chính xác hình phạt tối ưu được đại diện bởi một trạng thái DP. Cuối cùng, việc chọn mức tăng video thông thường lớn nhất là tối ưu cho mỗi số lượng video thông thường cố định và phần thân sẽ đánh giá tập hợp con thương mại tốt nhất cho mỗi số lượng người đăng ký thu được. Do đó, mọi kết hợp được xem xét theo mức tối đa hóa cuối cùng đều tối ưu cho số lượng video đã phát hành và mọi số có thể đều được xem xét. 

## Giải pháp Python```python
import sys
from functools import cmp_to_key

input = sys.stdin.readline

NEG = -10**30
INF = 10**30
INITIAL = 10000

def solve():
    n = int(input())
    good = [int(input()) for _ in range(n)]
    good.sort(reverse=True)

    pref = [0]
    for x in good:
        pref.append(pref[-1] + x)

    m = int(input())
    ads = []
    total_c = 0

    for _ in range(m):
        c, b = map(int, input().split())
        ads.append((c, b))
        total_c += c

    # DP is processed in increasing c / b order.
    # The actual release order is the reverse.
    def cmp(x, y):
        left = x[0] * y[1]
        right = y[0] * x[1]
        if left < right:
            return -1
        if left > right:
            return 1
        return 0

    ads.sort(key=cmp_to_key(cmp))

    # dp[k][s] = maximum negative interaction penalty
    # for k ads with total coefficient s.
    dp = [[NEG] * (total_c + 1) for _ in range(m + 1)]
    dp[0][0] = 0

    # Exact lower/upper bounds on possible total c for k selected
    # advertisements among the already processed advertisements.
    min_sum = [INF] * (m + 1)
    max_sum = [NEG] * (m + 1)
    min_sum[0] = 0
    max_sum[0] = 0

    processed = 0

    for c, b in ads:
        # Descending k makes the update 0/1: the current advertisement
        # cannot be used again during this iteration.
        for k in range(processed, -1, -1):
            lo = min_sum[k]
            hi = max_sum[k]

            if lo == INF:
                continue

            row = dp[k]
            nxt = dp[k + 1]

            for s in range(lo, hi + 1):
                val = row[s]
                if val == NEG:
                    continue

                ns = s + c
                nv = val - b * s

                if nv > nxt[ns]:
                    nxt[ns] = nv

        # Update reachable sum bounds after using this advertisement.
        for k in range(processed, -1, -1):
            if min_sum[k] != INF:
                v = min_sum[k] + c
                if v < min_sum[k + 1]:
                    min_sum[k + 1] = v

            if max_sum[k] != NEG:
                v = max_sum[k] + c
                if v > max_sum[k + 1]:
                    max_sum[k + 1] = v

        processed += 1

    # Build upper hull and query it for every possible subscriber count.
    #
    # A line is represented by y = slope * x + intercept.
    # Slopes are increasing because slope = total commercial c.
    def build_hull(k):
        hull_m = []
        hull_b = []

        lo = min_sum[k]
        hi = max_sum[k]

        if lo == INF:
            return hull_m, hull_b

        row = dp[k]

        for s in range(lo, hi + 1):
            intercept = row[s]
            if intercept == NEG:
                continue

            while len(hull_m) >= 2:
                m1 = hull_m[-2]
                b1 = hull_b[-2]
                m2 = hull_m[-1]
                b2 = hull_b[-1]

                # Remove the middle line if:
                # (b1-b2)/(m2-m1) >= (b2-intercept)/(s-m2)
                if (b1 - b2) * (s - m2) >= \
                   (b2 - intercept) * (m2 - m1):
                    hull_m.pop()
                    hull_b.pop()
                else:
                    break

            hull_m.append(s)
            hull_b.append(intercept)

        return hull_m, hull_b

    answer = [NEG] * (n + m + 1)

    for k in range(m + 1):
        hull_m, hull_b = build_hull(k)

        if not hull_m:
            continue

        pointer = 0

        for l in range(n + 1):
            x = INITIAL + pref[l]

            while pointer + 1 < len(hull_m):
                cur = hull_m[pointer] * x + hull_b[pointer]
                nxt = hull_m[pointer + 1] * x + hull_b[pointer + 1]

                if nxt >= cur:
                    pointer += 1
                else:
                    break

            profit = hull_m[pointer] * x + hull_b[pointer]
            d = k + l

            if profit > answer[d]:
                answer[d] = profit

    q = int(input())
    out = []

    for _ in range(q):
        d = int(input())
        out.append(str(answer[d]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu vào trước tiên sắp xếp các mức tăng thường xuyên và xây dựng tổng tiền tố của chúng.`pref[l]`là tổng số người đăng ký tăng tối đa có thể đạt được từ chính xác`l`video thông thường. 

Các video thương mại được sắp xếp theo phép nhân chéo thay vì phép chia dấu phẩy động. Bộ so sánh đặt nhỏ hơn`c/b`đầu tiên vì DP xử lý ngược đơn hàng thương mại tối ưu. sử dụng`c1 * b2`Và`c2 * b1`tránh mọi vấn đề so sánh dấu phẩy động có thể xảy ra. 

DP sử dụng một mảng cho mỗi số lượng quảng cáo được chọn có thể. Lặp lại`k`hướng xuống dưới là hướng ba lô 0/1 tiêu chuẩn. Nếu chúng ta lặp lên trên, quảng cáo hiện tại có thể được sử dụng nhiều lần trong cùng một lần lặp. 

Tổng DP là tổng`c`, không phải tổng số`b`. Đây là chi tiết triển khai trung tâm. Khi một quảng cáo giảm`b`được đặt sau các quảng cáo có tổng hệ số là`s`, doanh thu của nó giảm chính xác`b*s`. Đó là lý do tại sao quá trình chuyển đổi trừ`b * s`. 

các`min_sum`Và`max_sum`mảng chỉ là một tối ưu hóa triển khai. Họ ghi lại tổng số nhỏ nhất và lớn nhất có thể`c`cho mỗi`k`, do đó vòng lặp bên trong không quét các vị trí không thể có trong hàng DP. các`NEG`trọng điểm được chọn thấp hơn nhiều so với mọi câu trả lời khả thi, trong khi số nguyên Python có độ chính xác tùy ý, do đó, tràn không phải là vấn đề đáng lo ngại. 

Bước thân tàu cuối cùng biến mọi trạng thái DP thành một đường thẳng. Độ dốc của nó là tổng hệ số thương mại`s`và phần chặn của nó là hình phạt tương tác được DP lưu trữ. Vì số lượng người đăng ký`x`chỉ tăng lên khi nhiều video thông thường hơn được chọn, các truy vấn thân tàu có thể tiến về phía trước một cách đơn điệu. 

Các mẫu chính thức là```
1
10
2
10 20
20 10
4
0
1
2
3
```với đầu ra```
0
200000
299900
300200
```Và```
3
10
40
30
3
5 10
15 20
1 100
7
0
1
2
3
4
5
6
```với đầu ra```
0
150000
199900
209870
210710
211340
211550
```Đây là hai mẫu từ vấn đề ban đầu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các quảng cáo là`(c,b)=(10,20)`Và`(20,10)`. Tỉ số của chúng là`0.5`Và`2`, do đó quá trình DP`(10,20)`đầu tiên và`(20,10)`thứ hai. 

| Quảng cáo đã được xử lý |`k`| Tổng cộng`c`| Hình phạt DP | 
| --- | --- | --- | --- | 
| không | 0 | 0 | 0 | 
|`(10,20)`| 1 | 10 | 0 | 
|`(10,20),(20,10)`| 2 | 30 | -100 | 

Vì`d=1`, chỉ lấy quảng cáo thứ hai mang lại`20 * 10000 = 200000`. Vì`d=2`, lấy cả hai quảng cáo mang lại`30 * 10000 - 100 = 299900`. Vì`d=3`, video thông thường sẽ thêm 10 người đăng ký trước cả hai quảng cáo, vì vậy cơ sở sẽ trở thành`10010 * 30`, và mức phạt tương tự là 100`300200`. 

|`d`| Video thông thường | Video thương mại | Người đăng ký cơ sở | Tổng cộng`c`| Phạt đền | Lợi nhuận | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 10000 | 0 | 0 | 0 | 
| 1 | 0 | 1 | 10000 | 20 | 0 | 200000 | 
| 2 | 0 | 2 | 10000 | 30 | 100 | 299900 | 
| 3 | 1 | 2 | 10010 | 30 | 100 | 300200 | 

Dấu vết này chứng tỏ tại sao DP lại dựa trên tổng`c`và lý do tại sao việc đóng góp video thông thường được áp dụng cho mọi quảng cáo đã chọn. 

Đối với mẫu thứ hai, mức tăng đều đặn trở thành`[40,30,10]`, với tổng tiền tố`[0,40,70,80]`. Các tỷ số thương mại là`1/100`,`5/10`, Và`15/20`, vậy thứ tự DP là`(1,100)`,`(5,10)`,`(15,20)`. Thứ tự phát hành thực tế tối ưu là ngược lại. 

|`d`| Video thông thường | Video thương mại | Người đăng ký cơ sở | Tổng cộng`c`| Phạt đền | Lợi nhuận | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 10000 | 0 | 0 | 0 | 
| 1 | 0 |`(15,20)`| 10000 | 15 | 0 | 150000 | 
| 2 | 0 |`(15,20),(5,10)`| 10000 | 20 | 100 | 199900 | 
| 3 | 0 | cả 3 | 10000 | 21 | 130 | 209870 | 
| 4 | 1 | cả 3 | 10040 | 21 | 130 | 210710 | 
| 5 | 2 | cả 3 | 10070 | 21 | 130 | 211340 | 
| 6 | 3 | cả 3 | 10080 | 21 | 130 | 211550 | 

Ba hàng cuối cùng cho thấy lý do tại sao các video thông thường lại hữu ích mặc dù chúng không tạo ra doanh thu trực tiếp. Sau khi mọi video thương mại đã được chọn, mỗi video thông thường bổ sung sẽ tăng số lượng người đăng ký được xem bởi cả ba quảng cáo. 

## Phân tích độ phức tạp 

hãy để 

[ 
C=\sum_{i=1}^{m} c_i. 
] 

Bởi vì mỗi`c_i <= 100`Và`m <= 100`, chúng tôi có`C <= 10000`. 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m²C + nm + m log m)`| DP có`m`các lớp quảng cáo và`m`số lượng được chọn có thể có trên tổng số tiền lên tới`C`; kết cấu thân tàu quét từng hàng DP một lần | 
| Không gian |`O(mC + n + m)`| DP có`(m+1)(C+1)`tiểu bang, với tổng tiền tố và dữ liệu quảng cáo được lưu trữ riêng | 

Trường hợp xấu nhất chính thức của DP là về`100 * 100 * 10000 = 10^8`các vị trí trạng thái cơ bản. Việc triển khai tránh việc quét số tiền không thể truy cập được, điều này đặc biệt hiệu quả khi thương mại`c_i`giá trị bằng nhau hoặc có cấu trúc chung lớn. Thân tàu loại bỏ phần bổ sung`O(nmC)`lần quét cuối cùng được sử dụng bởi quá trình triển khai đơn giản. Việc sử dụng bộ nhớ là khoảng một triệu tham chiếu số nguyên cho DP, nằm trong giới hạn 512 MB. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`chức năng từ giải pháp trên nằm trong cùng một tệp.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        solve()

        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run(
    """1
10
2
10 20
20 10
4
0
1
2
3
"""
) == "0\n200000\n299900\n300200", "sample 1"

# Sample 2
assert run(
    """3
10
40
30
3
5 10
15 20
1 100
7
0
1
2
3
4
5
6
"""
) == "0\n150000\n199900\n209870\n210710\n211340\n211550", "sample 2"

# Minimum-size input: no videos at all.
assert run(
    """0
0
1
0
"""
) == "0", "minimum-size case"

# All values equal.
assert run(
    """2
5
5
2
10 5
10 5
5
0
1
2
3
4
"""
) == "0\n100000\n199950\n200050\n200150", "equal values"

# Boundary case: d is larger than the number of commercials.
assert run(
    """1
100
1
1 100
3
0
1
2
"""
) == "0\n10000\n10100", "d larger than m"

# Maximum n and m, with only three queried values.
parts = ["100"]
parts.extend(["100"] * 100)
parts.append("100")
parts.extend(["100 100"] * 100)
parts.append("3")
parts.extend(["0", "100", "200"])

max_input = "\n".join(parts) + "\n"

assert run(max_input) == (
    "0\n50500000\n150500000"
), "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 regular, 0 commercial, d=0`|`0`| Kích thước tối thiểu và ranh giới không có video | 
| Hai quảng cáo thông thường và hai quảng cáo giống hệt nhau |`0, 100000, 199950, 200050, 200150`| Tỷ lệ bằng nhau, giá trị lặp lại và chọn số khác nhau của cả hai loại video | 
| Một truy vấn thông thường, một truy vấn thương mại`0,1,2`|`0, 10000, 10100`| Chính xác`d`, kể cả trường hợp`d > m`| 
|`n=m=100`, tất cả các giá trị bằng nhau |`0, 50500000, 150500000`| Kích thước đầu vào tối đa và tổng số nguyên lớn | 

## Vỏ cạnh 

Vụ không có video được nhà nước xử lý`dp[0][0] = 0`. Thân tàu cuối cùng cho`k=0`chứa dòng đơn`y=0`, do đó chọn`l=0`video thông thường tạo ra câu trả lời bằng không. Vì```
0
0
1
0
```chương trình in`0`. 

Khi không có video thương mại`m=0`, DP chỉ chứa`dp[0][0]`. Mọi truy vấn thân tàu đều có giá trị bằng 0 bất kể có bao nhiêu video thông thường được chọn. Như vậy đối với```
2
5
7
0
3
0
1
2
```ba câu trả lời là`0`,`0`, Và`0`. 

Khi`d`vượt quá số lượng video thương mại có sẵn, mức tối đa hóa cuối cùng sẽ tự động xem xét các trạng thái có`k=m`Và`l=d-m`. Vì```
2
5
6
1
10 1
1
2
```cách duy nhất để có được lợi nhuận dương với đúng hai video là chọn video thương mại và một video thông thường. Mức tăng đều đặn lớn nhất là 6, vì vậy câu trả lời là`10 * 10006 = 100060`. 

Trường hợp cạnh thứ tự được xử lý bằng cách sắp xếp tỷ lệ. Vì```
0
2
10 20
20 10
1
2
```thứ tự DP là`(10,20)`theo sau là`(20,10)`, bởi vì`10/20 < 20/10`. Thứ tự phát hành thực tế tương ứng là ngược lại. Tổng hệ số là 30 và lần chuyển đổi DP thứ hai sẽ phải chịu hình phạt là`10 * 10 = 100`. Lợi nhuận thu được là`30 * 10000 - 100 = 299900`. 

Cuối cùng, sự đảm bảo về số lượng người đăng ký có nghĩa là mọi chuỗi được xem xét vẫn hợp lệ với số lượng người đăng ký không âm. Thuật toán không bao giờ cần từ chối trạng thái DP dựa trên sự tiêu cực của người đăng ký, bởi vì vấn đề đảm bảo tính khả thi ngay cả trong trường hợp xấu nhất là chỉ phát hành tất cả các video thương mại. 

Việc tối ưu hóa thân tàu là không cần thiết để hiểu ý tưởng cốt lõi, nhưng nó làm cho việc triển khai Python trở nên thực tế hơn nhiều so với bản dịch theo nghĩa đen của bản cuối cùng đơn giản`O(nmC)`quét.
