---
title: "CF 102439J - Boedi"
description: "Josya là người đầu tiên tham gia đầu vào. Mỗi vận động viên chạy năm vòng giống hệt nhau và bắn tổng cộng 20 mục tiêu. Mười mục tiêu được bắn từ tư thế nằm sấp và mười mục tiêu từ tư thế đứng. Một cú đánh không tốn thêm thời gian, trong khi mỗi cú đánh trượt sẽ cộng thêm chính xác 60 giây."
date: "2026-08-12T08:14:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "J"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 150
verified: true
draft: false
---

[CF 102439J - Boedium](https://codeforces.com/problemset/problem/102439/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Josya là người đầu tiên tham gia đầu vào. Mỗi vận động viên chạy năm vòng giống hệt nhau và bắn tổng cộng 20 mục tiêu. Mười mục tiêu được bắn từ tư thế nằm sấp và mười mục tiêu từ tư thế đứng. Một cú đánh không tốn thêm thời gian, trong khi mỗi cú đánh trượt sẽ cộng thêm chính xác 60 giây. 

Đối với vận động viên (i), gọi (K_i) là tổng số mục tiêu trượt. Khi đó, thời gian đua, ngoài thời gian bắn thông thường bị hủy bỏ trong mọi so sánh, là 

[ 
5\cdot time_i + 60K_i. 
] 

Mười lần bắn nằm sấp đưa ra một biến ngẫu nhiên nhị thức với 10 lần thử và xác suất trượt (1-down_i). Mười lượt bắn đứng đưa ra một biến ngẫu nhiên nhị thức khác với 10 lần thử và xác suất trượt (1-up_i). Do đó (K_i) chỉ có thể nhận 21 giá trị từ 0 đến 20. 

Josya đứng trên bục vinh quang khi có nhiều nhất hai vận động viên khác về đích nhanh hơn cô ấy. Bình đẳng không được tính là nhanh hơn, đây là nguồn gốc chính của các trường hợp biên trong so sánh. 

Dữ liệu đầu vào chứa tối đa 50.000 vận động viên và thời gian vòng đua tối đa là 600 giây. Vì số lần trượt có thể xảy ra chỉ là 21 nên chiều hướng lớn là số lượng vận động viên chứ không phải phạm vi thời gian đua có thể xảy ra. Phương thức (O(n^2)) sẽ yêu cầu khoảng (2,5\cdot10^9) hoạt động theo cặp ở kích thước tối đa, vượt xa giới hạn hai giây. Chúng ta cần một phương pháp mà công việc của mỗi vận động viên là không đổi. 

Có một số trường hợp đặc biệt trong đó việc triển khai hợp lý có thể sai. Đầu tiên, hòa không được tính là vận động viên nhanh hơn. Ví dụ,```
4
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
```có câu trả lời`1.000000000000`. Mọi vận động viên đều về đích vào cùng một thời điểm, vì vậy Josya không có đối thủ nào nhanh hơn. Một so sánh sử dụng`<=`thay vì`<`sẽ đếm không chính xác ba vận động viên bị hòa. 

Hình phạt một phút cũng tạo ra ranh giới chính xác. Coi như```
4
10 0.500 0.500
22 1.000 1.000
22 1.000 1.000
22 1.000 1.000
```Thời gian cơ bản của Josya là (50) giây. Với đúng một lần trượt, cô ấy sẽ về đích sau (110) giây, cùng thời gian với mọi đối thủ. Cô ấy vẫn còn trên bục giảng trong trường hợp đó. Với hai lần trượt trở lên, cả ba đối thủ đều nhanh hơn. Vì Josya có 20 cú đánh độc lập với xác suất trượt (1/2), nên câu trả lời là 

\frac{21}{1048576}. 
] 

Một công thức vô tình coi đẳng thức nhanh hơn sẽ mất toàn bộ phần đóng góp (K=1). 

Số lượng người tham gia nhỏ nhất cũng đặc biệt. Với một, hai hoặc ba vận động viên, Josya không bao giờ có thể có ba vận động viên dẫn trước mình một cách vượt trội, vì vậy câu trả lời luôn là 1. Ví dụ:```
1
100 0.000 0.000
```có câu trả lời`1.000000000000`, bất kể cách phân phối bắn súng của Josya. 

Cuối cùng, xác suất chính xác bằng 0 hoặc 1 phải hoạt động mà không chia cho xác suất chẳng hạn như (1-p). Việc triển khai dựa trên phép lặp nhị thức có chứa`p / (1-p)`có thể chia cho 0 khi một vận động viên có xác suất trúng đích là 1. Việc tính toán các thuật ngữ nhị thức trực tiếp tránh được vấn đề này. 

## Phương pháp tiếp cận 

Một giải pháp cưỡng bức trực tiếp trước tiên sẽ tính toán xác suất của mọi số lần bỏ lỡ có thể có đối với mọi vận động viên, sau đó liệt kê mọi tổ hợp số lần bỏ lỡ của họ. Mỗi vận động viên có 21 tổng điểm có thể xảy ra, vì vậy đối với (n) vận động viên có (21^n) kết quả cuộc đua có thể xảy ra. Ngay cả khi bỏ qua thực tế là mỗi kết quả cũng cần được tính trọng số theo xác suất của nó, vì (n=50000) con số này lớn đến mức vô vọng. 

Lực lượng vũ phu hoạt động vì việc ấn định đầy đủ số lần bỏ lỡ sẽ quyết định hoàn toàn tất cả thời gian của cuộc đua. Nó thất bại vì chúng ta thực sự không cần biết kết quả chính xác của từng vận động viên. Đối với kết quả Josya cố định, mỗi thí sinh có thể giảm xuống còn một sự kiện Bernoulli: thí sinh này có về đích nhanh hơn Josya không? 

Sự quan sát đó thay đổi hoàn toàn vấn đề. Sửa số lần trượt của Josya thành (k). Đối với mọi vận động viên khác (i), chúng ta có thể tính toán 

[ 
q_i(k)=P(\text{vận động viên }i\text{ về đích nhanh hơn Josya}\mid K_J=k). 
] 

Khi (k) được cố định, các sự kiện của các đối thủ khác nhau sẽ độc lập. Chúng tôi chỉ quan tâm số sự kiện Bernoulli thành công là 0, 1 hay 2, bởi vì bất kỳ giá trị nào từ 3 trở lên có nghĩa là Josya sẽ trượt bục vinh quang. 

Chúng ta có thể duy trì một mảng lập trình động ba trạng thái. Ban đầu, xác suất để không có vận động viên nào nhanh hơn là 1. Sau khi xử lý một đối thủ có xác suất nhanh hơn Josya (q), các trạng thái được cập nhật bằng cách giữ nguyên số lượng hiện tại hoặc tăng thêm một. Chúng tôi loại bỏ hai trạng thái trên vì chúng không bao giờ có thể đóng góp cho câu trả lời. 

Câu hỏi còn lại là làm thế nào để tính (q_i(k)) một cách hiệu quả. Giả sử Josya có thời gian vòng đua (T) và (k) trượt, trong khi đối thủ (i) có thời gian vòng đua (t_i) và (j) trượt. Đối thủ cạnh tranh hoàn toàn nhanh hơn khi 

[ 
5t_i+60j < 5T+60k. 
] 

Chia cho 5 được 

[ 
t_i+12j < T+12k. 
] 

Đối với (k) cố định, số nguyên lớn nhất được phép (j) là 

[ 
m=\left\lfloor\frac{T+12k-t_i-1}{12}\right\rfloor. 
] 

Vì vậy (q_i(k)) chỉ đơn giản là xác suất mà vận động viên (i) có nhiều nhất (m) trượt. Vì (m) luôn nằm trong khoảng từ 0 đến 20 khi phù hợp nên chúng ta có thể tính toán trước hàm phân phối tích lũy số lần trượt của mỗi vận động viên. 

Chỉ có 21 giá trị có thể có của (k). Đối với mỗi đối thủ, chúng tôi xử lý tất cả (n-1) đối thủ cạnh tranh và duy trì ba trạng thái DP. Điều này đưa ra phép tính cuối cùng (O(21n)). Việc tính toán phân phối trượt 21 giá trị cho mỗi vận động viên cũng tốn một khối lượng công việc không đổi, bởi vì đó là tích chập của hai phân phối nhị thức có kích thước 11. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(21^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(21n)) | (O(21n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời gian vòng đua và xác suất bắn của Josya, sau đó đọc các vận động viên còn lại. Dòng đầu tiên sau (n) mô tả Josya, vì vậy cô ấy phải được tách biệt khỏi các đối thủ cạnh tranh. 
2. Đối với mỗi vận động viên, hãy xây dựng phân bố xác suất của tổng số lần trượt của họ. Phần nằm nghiêng là phân phối nhị thức với 10 lần thử và xác suất trượt (1-down_i), trong khi phần đứng có 10 lần thử và xác suất trượt (1-up_i). Việc kết hợp hai phân phối này mang lại xác suất từ ​​0 đến 20 lần trượt. 
3. Chuyển phân phối sai của mọi thí sinh thành phân phối tích lũy. Lưu trữ (F_i[x]=P(K_i\le x)) cho mọi (x) từ 0 đến 20. Dạng tích lũy cho phép chúng ta trả lời một truy vấn nhanh hơn Josya trong thời gian không đổi. 
4. Tính phân phối riêng của Josya (P_J[k]=P(K_J=k)) cho tất cả (k) từ 0 đến 20. Chúng ta sẽ điều kiện riêng biệt từng giá trị có thể có của (k). 
5. Sửa số lần bỏ lỡ Josya cụ thể (k). Thời gian chạy hiệu quả của cô ấy, sau khi loại bỏ thời gian bắn thông thường, là (5T+60k). Đối với đối thủ cạnh tranh (i), hãy tính 

[ 
m_i=\left\lfloor\frac{T+12k-t_i-1}{12}\right\rfloor. 
]

Phép trừ một là phép biến đổi bất đẳng thức nghiêm ngặt thành giới hạn trên số nguyên. Nếu không có nó, một tỷ số hòa chính xác sẽ được tính là về đích nhanh hơn một cách không chính xác. 

1. Nếu (m_i<0), đối thủ cạnh tranh (i) không thể nhanh hơn nên đặt (q_i=0). Nếu (m_i\ge20), mỗi lần bỏ lỡ có thể sẽ khiến đối thủ đó nhanh hơn, vì vậy hãy đặt (q_i=1). Nếu không thì sử dụng (q_i=F_i[m_i]). 
2. Xử lý từng đối thủ cạnh tranh bằng mảng DP`dp`. Ba mục nhập của nó thể hiện xác suất có chính xác 0, 1 hoặc 2 đối thủ cạnh tranh được xử lý nhanh hơn Josya. Đối với một đối thủ cạnh tranh có xác suất (q) nhanh hơn, quá trình chuyển đổi là 

[ 
new_0=dp_0(1-q), 
] 

[ 
new_1=dp_1(1-q)+dp_0q, 
] 

[ 
new_2=dp_2(1-q)+dp_1q. 
] 

Các bang có ba đối thủ nhanh hơn trở lên sẽ bị loại vì họ không bao giờ có thể đưa Josya lên bục vinh quang. 

1. Sau khi tất cả đối thủ cạnh tranh đã được xử lý,`dp[0] + dp[1] + dp[2]`là xác suất có điều kiện để Josya đứng trên bục vinh quang đã cho (K_J=k). Nhân số này với (P_J[k]) và thêm nó vào câu trả lời chung. 
2. Lặp lại DP cho tất cả 21 lần đếm trượt có thể có của Josya và in xác suất kết quả. 

### Tại sao nó hoạt động 

Việc sửa số lần trượt (k) của Josya sẽ khắc phục hoàn toàn thời gian đua của cô ấy. Đối với mọi đối thủ, sự kiện nhanh hơn khi đó là sự kiện Bernoulli có xác suất chính xác là CDF đếm sai của đối thủ ở ngưỡng dẫn xuất. Các vận động viên khác nhau bắn độc lập nên các sự kiện Bernoulli này diễn ra độc lập. 

Bất biến DP là sau khi xử lý một số đối thủ cạnh tranh,`dp[x]`là xác suất để chính xác (x) trong số các đối thủ cạnh tranh được xử lý đó nhanh hơn hoàn toàn so với Josya, với (x\in{0,1,2}). Mỗi đối thủ mới hoặc ở bên ngoài nhóm nhanh hơn hoặc tham gia vào nhóm đó, đưa ra các chuyển đổi chính xác đã nêu. Vì chỉ đếm tối đa hai mới có thể dẫn đến bục vinh quang nên việc loại bỏ số lượng lớn hơn sẽ không có kết quả hợp lệ. 

Cuối cùng, 21 giá trị có thể có về số lần bỏ lỡ của Josya là rời rạc và đầy đủ. Việc tính trọng số của từng xác suất lên bục có điều kiện bằng xác suất đếm trượt tương ứng của Josya sẽ cho ra chính xác tổng xác suất cần thiết. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

C10 = [1, 10, 45, 120, 210, 252, 210, 120, 45, 10, 1]

def distribution(down, up):
    # Number of misses in the 10 prone shots.
    miss_down = 1.0 - down
    prone = [
        C10[k] * (miss_down ** k) * (down ** (10 - k))
        for k in range(11)
    ]

    # Number of misses in the 10 standing shots.
    miss_up = 1.0 - up
    standing = [
        C10[k] * (miss_up ** k) * (up ** (10 - k))
        for k in range(11)
    ]

    # Convolution: total misses range from 0 to 20.
    dist = [0.0] * 21
    for i in range(11):
        pi = prone[i]
        for j in range(11):
            dist[i + j] += pi * standing[j]

    return dist

def solve():
    n = int(input())

    t0, down0, up0 = input().split()
    t0 = int(t0)
    down0 = float(down0)
    up0 = float(up0)

    # Store competitors' lap times separately. Their CDFs are
    # stored transposed: cdfs[m][i] is competitor i's P(K <= m).
    times = []
    cdfs = [array('d') for _ in range(21)]

    for _ in range(n - 1):
        t, down, up = input().split()
        t = int(t)
        down = float(down)
        up = float(up)

        times.append(t)

        dist = distribution(down, up)
        cur = 0.0
        for k in range(21):
            cur += dist[k]
            cdfs[k].append(cur)

    josya = distribution(down0, up0)

    ans = 0.0
    competitors = n - 1

    for k in range(21):
        pj = josya[k]
        if pj == 0.0:
            continue

        dp0, dp1, dp2 = 1.0, 0.0, 0.0

        for i in range(competitors):
            # Competitor is faster iff
            # 5 * times[i] + 60 * misses
            # < 5 * t0 + 60 * k.
            #
            # Equivalent to
            # 12 * misses < t0 + 12*k - times[i].
            #
            # Maximum integer misses is floor((D - 1) / 12).
            D = t0 + 12 * k - times[i]
            m = (D - 1) // 12

            if m < 0:
                q = 0.0
            elif m >= 20:
                q = 1.0
            else:
                q = cdfs[m][i]

            nq = 1.0 - q

            ndp2 = dp2 * nq + dp1 * q
            ndp1 = dp1 * nq + dp0 * q
            ndp0 = dp0 * nq

            dp0, dp1, dp2 = ndp0, ndp1, ndp2

        ans += pj * (dp0 + dp1 + dp2)

    print(f"{ans:.12f}")

if __name__ == "__main__":
    solve()
```các`distribution`đầu tiên hàm tạo ra hai phân phối nhị thức. Số mũ là số lần trượt, còn số mũ còn lại là số lần trúng đích. Viết công thức trực tiếp sẽ tránh được việc chia cho 0 khi xác suất trúng chính xác là 0 hoặc 1. 

Convolution kết hợp mười cú đánh nằm sấp và mười cú đánh đứng. Chỉ có 11 số hạng trong mỗi phép phân phối, vì vậy 121 phép nhân cho mỗi vận động viên là công việc không đổi. 

Các phân phối tích lũy được lưu trữ trong`array('d')`các đối tượng thay vì danh sách float Python lồng nhau. Có nhiều nhất (21\cdot49999) giá trị được lưu trữ và giá trị double chiếm 8 byte, vì vậy việc này chiếm khoảng 8,4 MB cho dữ liệu số. Điều này giúp việc sử dụng bộ nhớ thoải mái dưới giới hạn 256 MB. 

Việc so sánh sử dụng`D = t0 + 12 * k - times[i]`. biểu hiện`(D - 1) // 12`là cố ý. Ví dụ, nếu`D`chính xác là 12, điều kiện là`12 * misses < 12`, vì vậy chỉ được phép bỏ lỡ 0. Công thức cho`(12 - 1) // 12 = 0`. sử dụng`D // 12`sẽ để xảy ra sai sót một cách không chính xác và sẽ biến đối thủ có kết quả hòa thành thắng. 

DP chỉ giữ ba trạng thái vì sự kiện mong muốn có nhiều nhất hai vận động viên nhanh hơn. Trạng thái thứ tư sẽ không bao giờ đóng góp vào câu trả lời, vì vậy việc lưu trữ nó sẽ chỉ tạo thêm công việc không cần thiết. 

Không thể tràn số nguyên trong Python và tất cả các phép tính xác suất đều sử dụng dấu phẩy động có độ chính xác kép. Sai số bắt buộc là (10^{-9}), trong khi việc tính toán chỉ liên quan đến phân bố xác suất nhỏ và khoảng một triệu lần chuyển đổi DP, do đó, sai số dấu phẩy động thu được nằm trong phạm vi dung sai yêu cầu một cách an toàn. 

## Ví dụ đã hoạt động 

Mẫu chính thức là```
4
45 0.700 0.700
60 0.800 0.800
90 0.900 0.900
120 1.000 1.000
```và đầu ra của nó là```
0.675394632273
```Đối với một đấu thủ có thời gian vòng đua là 60 và Josya có thời gian vòng đua là 45, điều kiện càng nhanh hơn. 

[ 
60+12j < 45+12k. 
] 

Số lần trượt của thí sinh lớn nhất được phép là (k-2). Đối với các vận động viên có thời gian vòng đua là 90 và 120, ngưỡng tương ứng là (k-4) và (k-6). Ngưỡng âm cho xác suất bằng không. Thuật toán đánh giá các CDF này cho mọi số lần bỏ lỡ Josya có thể xảy ra và sau đó chạy DP ba trạng thái. 

Một dấu vết minh bạch hơn thu được từ một chủng tộc hoàn toàn xác định:```
4
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
```Mọi vận động viên đều không có lần trượt nào với xác suất là 1, vì vậy Josya cũng không có lần trượt nào. 

| Đối thủ cạnh tranh đã được xử lý | Xác suất nhanh hơn (q) |`dp[0]`|`dp[1]`|`dp[2]`| 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 1 | 0 | 0 | 
| 1 | 0 | 1 | 0 | 0 | 
| 2 | 0 | 1 | 0 | 0 | 
| 3 | 0 | 1 | 0 | 0 | 

Đối với số lần trượt duy nhất có thể có của Josya (k=0), vận động viên có cùng thời gian vòng đua phải có ít lần trượt hơn 0 để nhanh hơn. Ngưỡng của nó là (-1), nên xác suất nhanh hơn của nó bằng 0. Bất biến được giữ nguyên sau mỗi thí sinh và xác suất lên bục có điều kiện là 1. 

Đối với ví dụ ranh giới```
4
10 0.500 0.500
22 1.000 1.000
22 1.000 1.000
22 1.000 1.000
```Tổng số lần trượt của Josya tuân theo phân phối nhị thức với 20 lần thử và xác suất (1/2). 

| Josya nhớ (k) | (P(K_J=k)) | Ngưỡng đối thủ (m) | Xác suất nhanh hơn (q) | Xác suất lên bục có điều kiện | 
| --- | --- | --- | --- | --- | 
| 0 | (1/2^{20}) | (-6) | 0 | 1 | 
| 1 | (20/2^{20}) | (-1) | 0 | 1 | 
| 2 | (190/2^{20}) | 4 | 1 | 0 | 
| 3 đến 20 | Xác suất còn lại | Ít nhất 5 | 1 | 0 | 

Đối với (k=1), thời gian của Josya là (50+60=110) giây, bằng chính xác thời gian sạch bóng của mỗi đấu thủ (5\cdot22=110). Ngưỡng là (-1) nên không có đối thủ nào được tính là nhanh hơn. Đối với (k=2), Josya cần 120 giây, trong khi cả ba đối thủ đều về đích trong 110 giây, do đó DP kết thúc với ba vận động viên nhanh hơn và không đóng góp gì. 

Câu trả lời cuối cùng là 

\frac{21}{1048576} 
\khoảng 0,0000200271606445. 
] 

Dấu vết này chứng tỏ tại sao sự bất bình đẳng nghiêm ngặt trong tính toán ngưỡng là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(21\cdot121\cdot n + 21n)) = (O(n)) | Hai phân phối nhị thức 11 phần tử của mỗi vận động viên được tích hợp, sau đó mỗi kết quả trong số 21 kết quả của Josya xử lý mọi đối thủ cạnh tranh với ba trạng thái DP. | 
| Không gian | (O(21n)) | Chúng tôi lưu trữ 21 xác suất tích lũy cho mỗi đối thủ, cộng với thời gian vòng đua. | 

Các hệ số không đổi là nhỏ vì số lần trượt tối đa được cố định là 20. Với 50.000 vận động viên, DP chính chỉ thực hiện khoảng một triệu chuyển đổi của đối thủ, trong khi cấu trúc phân phối thực hiện khoảng sáu triệu phép tính số học đơn giản. nhỏ gọn`array('d')`bộ nhớ cũng giữ mức sử dụng bộ nhớ ở mức dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

C10 = [1, 10, 45, 120, 210, 252, 210, 120, 45, 10, 1]

def distribution(down, up):
    miss_down = 1.0 - down
    prone = [
        C10[k] * (miss_down ** k) * (down ** (10 - k))
        for k in range(11)
    ]

    miss_up = 1.0 - up
    standing = [
        C10[k] * (miss_up ** k) * (up ** (10 - k))
        for k in range(11)
    ]

    dist = [0.0] * 21
    for i in range(11):
        for j in range(11):
            dist[i + j] += prone[i] * standing[j]

    return dist

def solve():
    input = sys.stdin.readline
    n = int(input())

    t0, down0, up0 = input().split()
    t0 = int(t0)
    down0 = float(down0)
    up0 = float(up0)

    times = []
    cdfs = [array('d') for _ in range(21)]

    for _ in range(n - 1):
        t, down, up = input().split()
        t = int(t)
        down = float(down)
        up = float(up)

        times.append(t)

        dist = distribution(down, up)
        cur = 0.0
        for k in range(21):
            cur += dist[k]
            cdfs[k].append(cur)

    josya = distribution(down0, up0)

    ans = 0.0

    for k in range(21):
        pj = josya[k]
        if pj == 0.0:
            continue

        dp0, dp1, dp2 = 1.0, 0.0, 0.0

        for i in range(n - 1):
            D = t0 + 12 * k - times[i]
            m = (D - 1) // 12

            if m < 0:
                q = 0.0
            elif m >= 20:
                q = 1.0
            else:
                q = cdfs[m][i]

            nq = 1.0 - q

            ndp2 = dp2 * nq + dp1 * q
            ndp1 = dp1 * nq + dp0 * q
            ndp0 = dp0 * nq

            dp0, dp1, dp2 = ndp0, ndp1, ndp2

        ans += pj * (dp0 + dp1 + dp2)

    print(f"{ans:.12f}")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
4
45 0.700 0.700
60 0.800 0.800
90 0.900 0.900
120 1.000 1.000
"""

assert abs(float(run(sample)) - 0.675394632273) < 1e-10, "provided sample"

assert abs(float(run("""\
1
100 0.000 0.000
""")) - 1.0) < 1e-12, "minimum size"

assert abs(float(run("""\
4
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
""")) - 1.0) < 1e-12, "all equal values"

assert abs(float(run("""\
4
9 1.000 1.000
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
""")) - 0.0) < 1e-12, "three strictly faster"

assert abs(float(run("""\
4
10 0.500 0.500
22 1.000 1.000
22 1.000 1.000
22 1.000 1.000
""")) - 21.0 / 1048576.0) < 1e-12, "strict tie boundary"

assert abs(float(run("""\
4
10 0.900 0.900
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
""")) - 0.9 ** 20) < 1e-12, "stochastic Josya"

maximum_input = "50000\n" + "\n".join(
    "1 1.000 1.000" for _ in range(50000)
) + "\n"

assert abs(float(run(maximum_input)) - 1.0) < 1e-12, "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 100 0.000 0.000`|`1`| Số lượng người tham gia tối thiểu và thực tế là Josya không thể có ba đối thủ nhanh hơn. | 
| Bốn vận động viên xác định giống hệt nhau |`1`| Mối quan hệ không được tính là nhanh hơn. | 
| Josya ở 10 giây, ba đối thủ ở 9 giây |`0`| Chính xác có ba đối thủ cạnh tranh nhanh hơn phải loại bỏ Josya. | 
| Josya ở 10 giây với xác suất 0,5, đối thủ ở 22 giây | (21/1048576) | Ranh giới chính xác một phút và sự bất bình đẳng nghiêm ngặt. | 
| Josya với xác suất trúng đích 0,9, ba đối thủ sạch sẽ bằng thời gian | (0,9^{20}) | Xây dựng đúng cách phân phối trượt 20 phát bắn. | 
| 50.000 vận động viên xác định giống hệt nhau |`1`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính. | 

## Vỏ cạnh 

Trường hợp thời gian bằng nhau được xử lý bởi`D - 1`trong công thức ngưỡng. Vì```
4
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
10 1.000 1.000
```Josya có (k=0) và đối với mọi đối thủ cạnh tranh (D=0). Công thức cho ra (m=(-1)//12=-1), do đó mọi xác suất nhanh hơn đều bằng 0. DP vẫn còn`(1, 0, 0)`và câu trả lời chính xác là 1. 

Ranh giới chính xác một phút được xử lý tương tự. Vì```
4
10 0.500 0.500
22 1.000 1.000
22 1.000 1.000
22 1.000 1.000
```khi Josya trượt một lần, (D=0), cho (m=-1). Các đối thủ đang hòa nhau ở 110 giây, vì vậy xác suất nhanh hơn của họ chính xác là bằng 0. Khi Josya có hai lần trượt, (D=60), bỏ cuộc (m=4), và mọi đối thủ sạch sẽ đều nhanh hơn. Do đó, thuật toán giữ chính xác các trường hợp (k=0) và (k=1), tạo ra (21/1048576). 

Đối với một người tham gia,```
1
100 0.000 0.000
```không có sự chuyển đổi của đối thủ cạnh tranh nào cả. DP bắt đầu lúc`(1, 0, 0)`với mọi số lần bỏ lỡ Josya có thể xảy ra và tổng phân phối Josya là 1. Do đó, câu trả lời là 1 mà không yêu cầu bất kỳ trường hợp đặc biệt nào trong thuật toán chính. 

Đối với xác suất bắn xác định bằng 0 hoặc 1, công thức nhị thức trực tiếp vẫn hợp lệ. Nếu xác suất trúng là 1 thì mọi số lần trượt dương đều nhận được xác suất bằng 0. Nếu xác suất trúng là 0 thì chỉ có số lần bắn trượt tối đa mới nhận được xác suất là một. Vì việc triển khai không bao giờ chia cho xác suất trúng hoặc trượt nên cả hai điểm cuối đều được xử lý mà không có ngoại lệ bằng số.
