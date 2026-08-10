---
title: "CF 102392E - Chuyển giao sự sống"
description: "Chúng tôi có (n) người với độ tuổi đã biết. Mỗi người phải di chuyển với tư cách là người lái xe hoặc hành khách trên ô tô hoặc một mình trên xe máy. Một ô tô có sức chứa (k), chính xác một trong những người ngồi trên xe là người lái xe và người lái xe đó phải ít nhất (lc) tuổi."
date: "2026-08-10T19:32:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 256
verified: true
draft: false
---

[CF 102392E - Chuyển giao sự sống](https://codeforces.com/problemset/problem/102392/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (n) người với độ tuổi đã biết. Mỗi người phải di chuyển với tư cách là người lái xe hoặc hành khách trên ô tô hoặc một mình trên xe máy. Một ô tô có sức chứa (k), chính xác một trong những người ngồi trên xe là người lái xe và người lái xe đó phải ít nhất (l_c) tuổi. Những người khác trong xe không có yêu cầu về độ tuổi tối thiểu ngoài ít nhất 1 tuổi. Một xe máy chở một người, người này phải ít nhất (l_m) tuổi. 

Một chiếc ô tô có giá (p_c), trong khi một chiếc xe máy có giá (p_m). Vì (p_c>p_m), số lượng ô tô quan trọng vì một ô tô có thể thay thế nhiều xe máy, nhưng việc sử dụng nhiều ô tô hơn cũng tạo ra nhiều người lái xe có thể cần độ tuổi của họ tăng lên. 

Trước khi đi du lịch, độ tuổi có thể được chuyển đổi giữa mọi người. Nếu một người mất (x) năm thì một người khác được đúng (x) năm, nên tổng số tuổi của dân số không bao giờ thay đổi. Chuyển chi phí một năm (t). Đối với mỗi người, tuổi cuối cùng có thể khác với tuổi ban đầu của họ tối đa là (d) và không ai được trẻ hơn 1. 

Đối với một kế hoạch vận chuyển cố định, vấn đề là vấn đề phân bổ. Một số người cần thêm độ tuổi để đáp ứng yêu cầu về lái xe hoặc mô tô, trong khi những người khác đủ tuổi để quyên góp. Chi phí chuyển nhượng chính xác là tổng số năm phải cộng thêm cho những người ban đầu còn quá trẻ. 

Đầu vào chứa (n) và (k), sau đó là hai ngưỡng tuổi và hai giá xe, sau đó là giá chuyển nhượng (t) và thay đổi độ tuổi tối đa của từng cá nhân (d), tiếp theo là mảng độ tuổi. Đầu ra yêu cầu là tổng chi phí thuê và chuyển nhượng tối thiểu hoặc (-1) nếu không có thỏa thuận hợp lệ. 

Giới hạn đủ lớn để loại trừ bất kỳ số mũ hoặc bậc hai nào. Với (n\le 10^5), thuật toán (O(n^2)) đã thực hiện khoảng (10^{10}) thao tác cơ bản trong trường hợp xấu nhất, vượt xa giới hạn một giây cho phép. Việc sắp xếp là ổn vì (O(n\log n)) có khoảng vài triệu so sánh ở quy mô này. Sau khi sắp xếp, công việc còn lại cần phải tuyến tính. 

Có một số trường hợp khó xử lý. Đầu tiên, một hành khách đi ô tô không cần phải thỏa mãn (l_m). Ví dụ,```
2 2
18 1000 16 1
5 3
16 15
```có câu trả lời (1010). Người 16 tuổi có thể trở thành người lái xe bằng cách nhận 2 năm từ người 15 tuổi, người có tuổi cuối cùng là 13. Hành khách được phép dưới 16 tuổi vì ngưỡng mô tô chỉ áp dụng cho người đi xe máy. Một giải pháp yêu cầu không chính xác mỗi hành khách đi ô tô ít nhất phải có (l_m) sẽ tuyên bố rằng sự sắp xếp này là không thể. 

Thứ hai, giới hạn thay đổi độ tuổi áp dụng cho mọi người chứ không chỉ cho những người được nhận tuổi. Ví dụ,```
3 2
20 3 15 1
1 3
20 11 13
```có câu trả lời (6). Sử dụng một chiếc xe hơi. Em 20 tuổi lái xe, em 11 tuổi chở khách, em 13 tuổi đi xe máy. Người đi xe máy cần thêm 2 năm, người đi sau có thể hiến đúng 2 năm, đổi từ 11 thành 9. Kết quả là chi phí (3+1+2=6). Một giải pháp bất cẩn coi hành khách trên xe là cần thiết (l_m) sẽ từ chối sự sắp xếp. 

Thứ ba, trường hợp một người cần chính xác (d) năm bổ sung là hợp lệ. Nếu mức thâm hụt là (d+1) thì người đó không thể nhận đủ tuổi, ngay cả khi những người khác có nhiều tuổi rảnh rỗi. Giới hạn cá nhân phải được kiểm tra riêng biệt với tổng số tuổi sẵn có. 

Cuối cùng, số lượng ô tô có thể là (\lceil n/k\rceil), không chỉ (\lfloor n/k\rfloor). Toa cuối cùng được phép chở ít hơn (k) người vì (k) là sức chứa. Ví dụ, với (n=5,k=3), hai ô tô có thể chở tất cả mọi người, với ba người ở ô tô thứ nhất và hai người ở ô tô thứ hai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ liệt kê số lượng ô tô và sau đó thử mọi cách phân công người có thể có cho người lái xe, hành khách trên ô tô và người đi xe máy. Đối với mỗi nhiệm vụ, chúng tôi có thể tính toán độ tuổi thiếu hụt, độ tuổi có thể quyên góp và chi phí tương ứng. Điều này đúng vì mọi sự sắp xếp vận chuyển có thể đều được xem xét. 

Vấn đề là số lượng nhiệm vụ. Trong trường hợp đặc biệt (k=1), chỉ có người lái xe và người đi xe máy, và tất cả (2^n) tập hợp con đều có thể. Nếu mọi nhiệm vụ của ứng viên đều được đánh dấu vào (O(n)), thì công việc trong trường hợp xấu nhất là (O(n2^n)), điều này là không thể đối với (n=10^5). Cho phép ba vai trò chỉ có thể làm cho không gian tìm kiếm lớn hơn. 

Quan sát quan trọng là khi số lượng ô tô được cố định thì quy mô của cả ba nhóm cũng cố định. Giả sử có (c) ô tô. Có (c) người lái xe, có tới (c(k-1)) hành khách đi ô tô, số còn lại sử dụng xe máy. Chính xác hơn, số lượng người đi xe máy là 

[ 
m=\max(0,n-ck). 
] 

Nếu (ck<n), tất cả (c) ô tô có thể được lấp đầy hoàn toàn. Nếu (ck\ge n) thì không có xe máy và ô tô cuối cùng chỉ chở được một phần. 

Ba vai trò có ba ngưỡng độ tuổi khác nhau: 

[ 
l_c > l_m > 1. 
] 

Sự ra lệnh đó cho chúng ta sự phân công tham lam. Sau khi sắp xếp mọi người theo độ tuổi theo thứ tự giảm dần, những người lớn tuổi nhất (c) phải là người lái xe, những người tiếp theo (c(k-1)) phải là hành khách ô tô và những người còn lại nên sử dụng xe máy. 

Tại sao điều này làm việc? Hãy xem xét hai người có độ tuổi (x\ge y) và hai vai trò có ngưỡng (H>L). Đưa ra ngưỡng cao hơn cho (x) thay vì (y) không thể làm tăng yêu cầu chuyển tuổi. chức năng 

[ 
\max(0,H-a)-\max(0,L-a) 
] 

không tăng khi (a) phát triển, vì vậy người lớn tuổi luôn là ứng cử viên sáng giá hơn cho vai trò chặt chẽ hơn. 

Đối số trao đổi tương tự có tác dụng cho tính khả thi. Xác định mức đóng góp ròng hữu ích tối đa của một người được ấn định ngưỡng (T) là 

[ 
B_T(a)=\min(d,a-T). 
] 

Nếu (a<T), đây là số âm và thể hiện số năm mà người đó phải nhận. Nếu (a\ge T), nó dương và thể hiện số năm người đó có thể hiến tặng trong khi tôn trọng giới hạn (d)-năm. Đối với hai ngưỡng (H>L), chênh lệch (B_L(a)-B_H(a)) không tăng với (a). Do đó, việc giao vai trò chặt chẽ hơn cho người lớn tuổi không bao giờ làm giảm tổng số dư hiện có. 

Vì vậy, sự sắp xếp được sắp xếp đồng thời giảm thiểu yêu cầu chuyển khoản và tối đa hóa số dư chuyển khoản sẵn có. Chúng ta không cần xem xét việc phân công vai trò thay thế. 

Sau khi sắp xếp, tổng tiền tố cho phép chúng ta đánh giá mọi số lượng ô tô có thể có trong thời gian không đổi. Chúng ta chỉ cần quét (c=0,1,\ldots,\lceil n/k\rceil). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) trong trường hợp xấu nhất (k=1) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp mọi lứa tuổi theo thứ tự giảm dần. Đối với số lượng ô tô cố định, nhóm đầu tiên sẽ là người lái xe, nhóm tiếp theo là hành khách trên ô tô và nhóm còn lại là người đi xe máy. Thứ tự của các ngưỡng (l_c>l_m>1) làm cho phép gán này trở nên tối ưu bằng đối số trao đổi ở trên. 
2. Tính toán trước tổng tiền tố cho bốn đại lượng. Đối với trình điều khiển, hãy lưu trữ mức tăng cần thiết (\max(0,l_c-a_i)) và mức đóng góp ròng (\min(d,a_i-l_c)). Đối với người đi xe máy, lưu trữ các đại lượng tương tự với ngưỡng (l_m). Đối với hành khách đi ô tô, ngưỡng của họ là 1, do đó mức đóng góp của họ chỉ đơn giản là (\min(d,a_i-1)). Hành khách không bao giờ cần thêm tuổi vì mỗi độ tuổi ban đầu ít nhất là 1. 
3. Đếm số lượng ô tô (c) từ 0 đến (\lceil n/k\rceil). Đối với (c) ô tô, hãy để 

[ 
q=\min(n,ck). 
] 

Những người được sắp xếp (c) đầu tiên là người lái xe, vị trí (c) đến (q-1) là hành khách ô tô và vị trí (q) đến (n-1) là người đi xe máy. 

1. Tính số tiền chuyển cần thiết. Chỉ người lái xe và người đi xe máy mới có thể yêu cầu thêm độ tuổi, vì vậy 

[ 
R= 
\text{driverNeed[c] 
+ 
\left(\text{motorcycleNeed[n]-\text{motorcycleNeed[q]\right). 
] 

Hành khách không đóng góp gì vào số tiền này vì họ chỉ cần đủ ít nhất 1 tuổi. 

1. Kiểm tra giới hạn (d) năm của cá nhân. Nếu có tài xế thì tài xế nhỏ tuổi nhất phải đáp ứng 

[ 
a_{c-1}+d\ge l_c. 
] 

Nếu có xe máy thì người đi xe máy nhỏ tuổi nhất phải đáp ứng 

[ 
a_{n-1}+d\ge l_m. 
] 

Vì mảng đã được sắp xếp nên việc kiểm tra người nhỏ tuổi nhất trong mỗi nhóm là đủ. Một người có mức thâm hụt lớn hơn (d) không bao giờ có thể đạt đến độ tuổi yêu cầu, bất kể người khác có bao nhiêu tuổi. 

1. Tính tổng số dư tuổi thanh toán. Đối với mọi trình điều khiển, hãy sử dụng (\min(d,a_i-l_c)). Đối với mọi hành khách, hãy sử dụng (\min(d,a_i-1)). Đối với mọi người đi xe máy, hãy sử dụng (\min(d,a_i-l_m)). Tổng ít nhất phải bằng 0. 

Số dư không âm có nghĩa là tổng số tiền có thể quyên góp đủ để trang trải mọi khoản thâm hụt. Vì mọi mức thâm hụt của từng cá nhân đã được kiểm tra theo (d), nên người cho và người nhận có thể được ghép nối cho đến khi đáp ứng được mọi yêu cầu về độ tuổi tăng lên. 

1. Tính toán chi phí vận chuyển. Có (c) ô tô và 

[ 
\max(0,n-ck) 
] 

xe máy. Như vậy chi phí thuê là 

[ 
c\cdot p_c+\max(0,n-ck)\cdot p_m. 
] 

Chi phí chuyển giao là (R\cdot t). Thêm các giá trị này và giảm thiểu câu trả lời trên tất cả khả thi (c). 

1. Nếu không có giá trị nào của (c) khả thi, hãy in (-1). Ngược lại in ra tổng chi phí nhỏ nhất tìm được. 

### Tại sao nó hoạt động 

Đối với bất kỳ số lượng ô tô cố định nào, ba vai trò đều có ngưỡng (l_c>l_m>1). Việc trao đổi giữa hai người có độ tuổi (x\ge y) cho thấy rằng việc ấn định ngưỡng chặt chẽ hơn cho (x) không bao giờ làm tăng số tiền chuyển cần thiết và không bao giờ làm giảm số dư chuyển khoản khả dụng. Việc lặp lại các trao đổi này sẽ tạo ra sự sắp xếp chính xác được sắp xếp theo thuật toán. 

Đối với sự sắp xếp đó, tổng tiền tố tính toán chính xác độ tuổi mà mỗi người phải nhận và số tiền tối đa chính xác mà mỗi người có thể đóng góp theo giới hạn (d)-năm. Kiểm tra thâm hụt cá nhân đảm bảo rằng không có người nhận nào vi phạm ràng buộc cá nhân của họ, trong khi kiểm tra số dư tổng thể đảm bảo rằng có thể cung cấp đủ độ tuổi yêu cầu. Do đó, mọi ứng cử viên được chấp nhận đều tương ứng với một tập hợp chuyển giao hợp lệ và mọi kế hoạch vận chuyển hợp lệ được thể hiện bằng một trong số lượng xe được liệt kê. Lấy mức tối thiểu trên tất cả các ứng cử viên sẽ mang lại tổng chi phí tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    lc, pc, lm, pm = map(int, input().split())
    t, d = map(int, input().split())
    a = list(map(int, input().split()))

    a.sort(reverse=True)

    # Prefix sums.
    #
    # need_c[i]:
    #   total age required to make the first i people valid drivers.
    #
    # need_m[i]:
    #   total age required to make the first i people valid motorcycle riders.
    #
    # bal_c[i]:
    #   total net contribution of the first i people as drivers.
    #
    # bal_m[i]:
    #   total net contribution of the first i people as motorcycle riders.
    #
    # bal_p[i]:
    #   total net contribution of the first i people as car passengers.
    #
    # A passenger only has to remain at least 1 year old.

    need_c = [0] * (n + 1)
    need_m = [0] * (n + 1)
    bal_c = [0] * (n + 1)
    bal_m = [0] * (n + 1)
    bal_p = [0] * (n + 1)

    for i, age in enumerate(a, 1):
        need_c[i] = need_c[i - 1] + max(0, lc - age)
        need_m[i] = need_m[i - 1] + max(0, lm - age)

        bal_c[i] = bal_c[i - 1] + min(d, age - lc)
        bal_m[i] = bal_m[i - 1] + min(d, age - lm)
        bal_p[i] = bal_p[i - 1] + min(d, age - 1)

    max_cars = (n + k - 1) // k
    INF = 10**30
    ans = INF

    for c in range(max_cars + 1):
        q = min(n, c * k)

        # First c people are drivers.
        # Next q-c people are car passengers.
        # Remaining people are motorcycle riders.

        # Every driver must be able to reach lc.
        if c > 0 and a[c - 1] + d < lc:
            continue

        # Every motorcycle rider must be able to reach lm.
        if q < n and a[n - 1] + d < lm:
            continue

        # Required age transfer.
        need = need_c[c] + (need_m[n] - need_m[q])

        # Total net amount of age available after respecting
        # the d-year limit for every individual.
        balance = (
            bal_c[c]
            + (bal_p[q] - bal_p[c])
            + (bal_m[n] - bal_m[q])
        )

        if balance < 0:
            continue

        motorcycles = max(0, n - c * k)
        cost = c * pc + motorcycles * pm + need * t

        if cost < ans:
            ans = cost

    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp tạo ra ba nhóm vai trò liên tiếp được mô tả trong thuật toán. Vì sắp xếp của Python chạy trong (O(n\log n)), đây là phần siêu tuyến tính duy nhất của giải pháp. 

Năm mảng tiền tố cho phép đánh giá mọi số lượng ô tô ứng viên mà không cần quét lại người. Ví dụ,`need_c[c]`chính xác là việc chuyển giao theo yêu cầu đầu tiên`c`trình điều khiển, trong khi`need_m[n] - need_m[q]`là sự chuyển giao theo yêu cầu của nhóm xe máy. 

biểu thức`min(d, age - threshold)`là chi tiết triển khai chính. Khi`age`dưới ngưỡng, nó âm và thể hiện mức tăng cần thiết. Khi`age`trên ngưỡng, nó dương nhưng bị giới hạn ở mức`d`, bởi vì người đó không thể mất nhiều hơn`d`năm. Đối với hành khách, ngưỡng là 1, vì vậy`min(d, age - 1)`luôn không âm. 

ranh giới`q = min(n, c * k)`xử lý chiếc xe cuối cùng được lấp đầy một phần. Khi`c*k >= n`, không có xe máy và các vị trí`c`bởi vì`n-1`đều là hành khách. Đây là lý do tại sao vòng lặp bao gồm`ceil(n/k)`ô tô. 

Số nguyên Python không bị tràn nhưng sử dụng một giá trị lớn rõ ràng`INF`giữ logic chi phí tối thiểu rõ ràng. Câu trả lời tối đa có thể cũng nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 2
18 1000 16 1
5 3
16 15
```Sau khi sắp xếp, các độ tuổi đã có`[16, 15]`. 

| Ô tô (c) | (q) | Trình điều khiển | Hành khách | Xe máy | Cần | Số dư | Khả thi | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | không | không | 16, 15 | 1 | -1 | Không | 2 | 
| 1 | 2 | 16 | 15 | không | 2 | 1 | Có | 1010 | 

Với số không ô tô, người đi xe máy 15 tuổi cần thêm một năm nữa, nhưng không ai có thể hiến tuổi khi vẫn đáp ứng yêu cầu nên số dư là âm. 

Với một chiếc ô tô, thiếu niên 16 tuổi trở thành tài xế và cần 2 năm. Trẻ 15 tuổi chỉ là hành khách nên có thể đóng góp 2 năm và trở thành 13. Cả hai thay đổi cá nhân nhiều nhất là (d=3). Tổng số là (1000+2\cdot5=1010). 

### Mẫu 2 

Đầu vào là```
2 2
23 10 15 5
2 2
9 20
```Sau khi sắp xếp, độ tuổi là`[20, 9]`. 

| Ô tô (c) | (q) | Trình điều khiển | Hành khách | Xe máy | Cần | Số dư | Khả thi | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | không | không | 20, 9 | 6 | -4 | Không | 40 | 
| 1 | 2 | 20 | 9 | không | 3 | -1 | Không | 10 | 

Với số 0 ô tô, cậu bé 9 tuổi cần 6 năm để đạt được ngưỡng 15 xe máy, nhưng (d=2), vì vậy người này không thể trở thành người lái xe máy hợp lệ. 

Với một chiếc ô tô, thanh niên 20 tuổi sẽ phải trở thành tài xế 23 tuổi, cần 3 năm, một lần nữa vượt quá giới hạn cá nhân (d=2). Do đó không có phương án vận chuyển khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Chi phí sắp xếp (O(n\log n)) và tất cả việc kiểm tra việc xây dựng tiền tố và số lượng ô tô đều mất (O(n)). | 
| Không gian | (O(n)) | Mỗi độ tuổi được sắp xếp và năm mảng tiền tố đều sử dụng bộ nhớ tuyến tính. | 

Hoạt động chủ yếu là sắp xếp (10^5) độ tuổi, sau đó là quét tuyến tính một lần tối đa (\lceil n/k\rceil+1) số lượng ô tô có thể có. Điều này dễ dàng nằm trong giới hạn dự định, đồng thời tránh được việc tìm kiếm phân công vai trò theo cấp số nhân. 

## Trường hợp thử nghiệm```python
# Complete assert-based test harness.
# The solution itself is the solve() function below.

import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    lc, pc, lm, pm = map(int, input().split())
    t, d = map(int, input().split())
    a = list(map(int, input().split()))

    a.sort(reverse=True)

    need_c = [0] * (n + 1)
    need_m = [0] * (n + 1)
    bal_c = [0] * (n + 1)
    bal_m = [0] * (n + 1)
    bal_p = [0] * (n + 1)

    for i, age in enumerate(a, 1):
        need_c[i] = need_c[i - 1] + max(0, lc - age)
        need_m[i] = need_m[i - 1] + max(0, lm - age)

        bal_c[i] = bal_c[i - 1] + min(d, age - lc)
        bal_m[i] = bal_m[i - 1] + min(d, age - lm)
        bal_p[i] = bal_p[i - 1] + min(d, age - 1)

    max_cars = (n + k - 1) // k
    INF = 10**30
    ans = INF

    for c in range(max_cars + 1):
        q = min(n, c * k)

        if c > 0 and a[c - 1] + d < lc:
            continue

        if q < n and a[n - 1] + d < lm:
            continue

        need = need_c[c] + need_m[n] - need_m[q]

        balance = (
            bal_c[c]
            + bal_p[q] - bal_p[c]
            + bal_m[n] - bal_m[q]
        )

        if balance < 0:
            continue

        motorcycles = max(0, n - c * k)
        cost = c * pc + motorcycles * pm + need * t
        ans = min(ans, cost)

    print(-1 if ans == INF else ans)

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

# Provided samples.
assert run("""\
2 2
18 1000 16 1
5 3
16 15
""") == "1010", "sample 1"

assert run("""\
2 2
23 10 15 5
2 2
9 20
""") == "-1", "sample 2"

# Minimum-size input.
assert run("""\
1 1
18 5 16 1
3 2
16
""") == "1", "minimum-size case"

# All values equal.
assert run("""\
6 3
20 10 10 6
2 5
15 15 15 15 15 15
""") == "36", "all-equal case"

# Boundary case where exactly d years must be transferred.
assert run("""\
3 2
20 3 15 1
1 5
20 15 10
""") == "8", "exact d transfer"

# A person too young for a motorcycle can become a car passenger.
assert run("""\
3 2
20 3 15 1
1 3
20 11 13
""") == "6", "car passenger has no lm requirement"

# Maximum-size input, generated rather than written explicitly.
n = 100000
ages = " ".join(["1"] * n)
max_input = f"""\
{n} 1
2 2 1 1
0 0
{ages}
"""
assert run(max_input) == "100000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 18 5 16 1 / 3 2 / 16`|`1`| Đầu vào có kích thước tối thiểu và trường hợp không ô tô | 
| Sáu người đều ở độ tuổi 15 |`36`| Tất cả các độ tuổi bằng nhau và so sánh giữa số lượng xe khác nhau | 
|`20 15 10`với (d=5) |`8`| Việc chuyển nhượng đúng (d) năm là hợp pháp | 
|`20 11 13`với (d=3) |`6`| Người đi ô tô không cần ngưỡng xe máy | 
| (10^5) người, tất cả 1 tuổi |`100000`| Tối đa (n), mảng tiền tố lớn và quét tuyến tính | 

## Vỏ cạnh 

Trường hợp tế nhị đầu tiên là sự phân biệt giữa người đi ô tô và người đi xe máy. TRONG```
3 2
20 3 15 1
1 3
20 11 13
```một chiếc xe là đủ. Em 20 tuổi lái xe, em 11 tuổi chở khách, em 13 tuổi đi xe máy. Người đi xe máy cần 2 năm, còn người đi sau tặng 2 năm đó. Hành khách kết thúc ở tuổi 9, điều này là hợp pháp vì chỉ có giới hạn dưới của 1 và giới hạn thay đổi (d=3) mới quan trọng. Tổng số là (3+1+2=6). 

Trường hợp tinh vi thứ hai là giới hạn chuyển khoản cá nhân. Giả sử một người đi xe máy thấp hơn 4 tuổi (l_m) trong khi (d=3). Ngay cả khi người khác có mười năm rảnh rỗi, tay đua cũng không thể nhận được cả bốn vì tuổi của họ có thể tăng lên nhiều nhất là ba. Thuật toán nắm bắt được điều này trước khi dựa vào tổng số dư bằng cách kiểm tra người lái mô tô trẻ nhất với (l_m-d). 

Lý do tương tự cũng áp dụng cho người lái xe. Nếu người lái xe trẻ nhất được chọn có độ tuổi (l_c-d-1) thì số xe ứng cử viên đó là không thể. Vì người lái xe là người (c) đầu tiên sau khi sắp xếp nên chỉ kiểm tra người thứ (c) là đủ. 

Trường hợp cạnh thứ ba là chi phí chuyển giao bằng không. Khi (t=0), thuật toán vẫn thực hiện tất cả các kiểm tra tính khả thi. Một kế hoạch khả thi chỉ tốn tiền thuê phương tiện, trong khi một kế hoạch không khả thi vẫn không thể thực hiện được. Việc bỏ qua việc tính toán tính khả thi chỉ vì việc chuyển tiền là miễn phí sẽ là không chính xác. 

Trường hợp cạnh thứ tư là (d=0). Không ai có thể thay đổi độ tuổi nên mọi người lái xe được chọn đều phải đáp ứng (l_c), mọi người đi xe máy đều phải đáp ứng (l_m) và hành khách không được đóng góp gì. Các công thức cân bằng tự nhiên giảm xuống 0 đối với những người đã vượt quá ngưỡng vai trò của họ và thâm hụt âm đối với những người ở dưới ngưỡng đó. 

Trường hợp cạnh thứ năm là chiếc xe cuối cùng được lấp đầy một phần. Với (n=5) và (k=3), hai ô tô có thể chở tất cả mọi người. Ba người đầu tiên chiếm một ô tô và hai người còn lại chiếm ô tô thứ hai. Thuật toán tiếp cận (c=\lceil5/3\rceil=2), đặt (q=\min(5,6)=5) và tạo chính xác hai người lái xe và ba hành khách không có xe máy. 

Trường hợp cạnh cuối cùng là (k=1). Mỗi ô tô chở đúng một người nên không có hành khách trên ô tô. Thuật toán vẫn hoạt động vì (c(k-1)=0), làm cho chặng hành khách trống. Các ứng cử viên bao gồm từ số không ô tô, trong đó mọi người đều sử dụng mô tô, đến (n) ô tô, trong đó mọi người đều là người lái xe.
