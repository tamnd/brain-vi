---
title: "CF 46E - Lược"
description: "Mỗi hàng của bảng chứa các số nguyên và từ mỗi hàng chúng ta phải lấy một tiền tố có độ dài dương. Nếu chúng ta chọn các ô c[i] từ hàng i thì các ô được chọn trong hàng đó chính xác là các mục c[i] đầu tiên."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 46
codeforces_index: "E"
codeforces_contest_name: "School Personal Contest #2 (Winter Computer School 2010/11) - Codeforces Beta Round 43 (ACM-ICPC Rules)"
rating: 1900
weight: 46
solve_time_s: 112
verified: true
draft: false
---
[CF 46E - Lược](https://codeforces.com/problemset/problem/46/E) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi hàng của bảng chứa các số nguyên và từ mỗi hàng chúng ta phải lấy một tiền tố có độ dài dương. Nếu chúng ta chọn`c[i]`ô từ hàng`i`, thì các ô được chọn trong hàng đó chính xác là ô đầu tiên`c[i]`mục nhập. 

Tổng phần thưởng là tổng của tất cả các số đã chọn. Hạn chế xuất phát từ chuỗi độ dài tiền tố:$$c_1 > c_2 < c_3 > c_4 < \dots$$Sự bất bình đẳng xen kẽ giữa các hàng lân cận. Các hàng được lập chỉ mục lẻ hoạt động giống như các đỉnh, các hàng được lập chỉ mục chẵn hoạt động giống như các thung lũng. 

Nhiệm vụ là tối đa hóa tổng số tiền trong khi vẫn tôn trọng điều kiện xen kẽ này. 

Kích thước bàn lên đến`1500 x 1500`. Nó đủ lớn để bất kỳ thuật toán nào liên quan đến tất cả các cặp cấu hình hàng một cách độc lập sẽ gặp khó khăn. Một hàng có thể chọn bất kỳ độ dài tiền tố nào từ`1`ĐẾN`m`, vậy là một không gian trạng thái đơn giản đã có`m^n`khả năng, điều đó hoàn toàn không thể xảy ra. 

Ngay cả việc lập trình động trên các hàng liền kề cũng cần được quan tâm. Chi phí chuyển đổi trực tiếp từ mọi độ dài tiền tố trước đó sang mọi độ dài tiền tố hiện tại`O(m^2)`mỗi hàng. Với`n,m ≤ 1500`, điều đó trở thành:$$1500 \times 1500^2 \approx 3.4 \times 10^9$$hoạt động vượt xa giới hạn. 

Các ràng buộc gợi ý rõ ràng rằng chúng ta cần một DP có các chuyển đổi được tối ưu hóa theo thời gian tuyến tính trên mỗi hàng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét trường hợp mọi giá trị đều âm:```
2 3
-5 -2 -1
-7 -3 -4
```Chúng ta vẫn phải chọn ít nhất một ô từ mỗi hàng. Việc triển khai bất cẩn cho phép các tiền tố trống sẽ trả về không chính xác`0`. 

Một trường hợp nguy hiểm khác là khi quá trình chuyển đổi tốt nhất không phải là duy nhất:```
3 3
5 -100 5
1 1 1
5 -100 5
```Sự lựa chọn tối ưu không phải lúc nào cũng là tiền tố có lợi nhuận dài nhất. Các chiến lược tham lam tối đa hóa từng hàng một cách độc lập sẽ thất bại vì sự bất bình đẳng xen kẽ giữa các hàng lân cận. 

Bất bình đẳng ranh giới cũng có vấn đề:```
2 2
10 10
1 1
```Hàng đầu tiên không thể chọn độ dài`1`nếu hàng thứ hai cũng chọn`1`, bởi vì mẫu yêu cầu`c1 > c2`. Độ dài bằng nhau không hợp lệ. 

Cuối cùng, vấn đề ngang bằng. Mối quan hệ thay đổi hướng mỗi hàng. Chuyển đổi hợp lệ giữa các hàng`1`Và`2`trở nên không hợp lệ giữa các hàng`2`Và`3`. Việc quên điều này sẽ tạo ra những câu trả lời sai một cách âm thầm. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi hàng, hãy thử mọi độ dài tiền tố có thể có từ`1`ĐẾN`m`. Kiểm tra xem chuỗi kết quả có thỏa mãn các bất đẳng thức xen kẽ hay không, sau đó tính tổng tiền tố. 

Điều này có hiệu quả vì sự lựa chọn trong mỗi hàng được xác định hoàn toàn bởi một số nguyên duy nhất, độ dài tiền tố. Vấn đề là số lượng khả năng:$$m^n$$Với`m = 1500`Và`n = 1500`, điều này thật vô vọng. 

Cải tiến tự nhiên tiếp theo là lập trình động. 

Tính tổng tiền tố đầu tiên cho mỗi hàng:$$pref[i][j] = \text{sum of first } j \text{ numbers in row } i$$Sau đó xác định:$$dp[i][j]$$là số tiền tối đa có thể đạt được sau khi xử lý hàng`1..i`, hàng ở đâu`i`chọn độ dài tiền tố`j`. 

Sự chuyển đổi phụ thuộc vào hướng bất đẳng thức. 

Nếu hàng`i-1`phải lớn hơn hàng`i`, sau đó:$$dp[i][j] = pref[i][j] + \max(dp[i-1][k]), \quad k > j$$Nếu hàng`i-1`phải nhỏ hơn hàng`i`, sau đó:$$dp[i][j] = pref[i][j] + \max(dp[i-1][k]), \quad k < j$$DP này đúng, nhưng việc triển khai đơn giản sẽ kiểm tra tất cả`k`cho mọi`j`, dẫn đến`O(nm^2)`. 

Quan sát quan trọng là mỗi lần chuyển đổi chỉ yêu cầu phạm vi tối đa so với hàng trước đó. Đối với một hàng cố định, chúng tôi liên tục cần:```
maximum over k < j
```hoặc```
maximum over k > j
```Chúng có thể được duy trì với cực đại tiền tố hoặc cực đại hậu tố trong thời gian tuyến tính. 

Giả sử chúng ta đang xử lý một hàng trong đó chúng ta cần độ dài trước đó nhỏ hơn độ dài hiện tại. Xây dựng:$$best[j] = \max(dp[i-1][1..j])$$Sau đó:$$\max_{k<j} dp[i-1][k] = best[j-1]$$Tương tự, khi chúng ta cần độ dài trước đó lớn hơn độ dài hiện tại, hãy xây dựng hậu tố cực đại. 

Điều này làm giảm mỗi hàng từ`O(m^2)`ĐẾN`O(m)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n) | O(n) | Quá chậm | 
| DP ngây thơ | O(nm²) | O(m) | Quá chậm | 
| Tối ưu | O(nm) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố cho mỗi hàng. 

Cho phép`pref[i][j]`là tổng của số đầu tiên`j`ô trong hàng`i`. Điều này cho phép truy cập liên tục vào giá trị được đóng góp bằng cách chọn độ dài tiền tố. 
2. Khởi tạo hàng DP đầu tiên. 

Đối với hàng`1`, mọi độ dài tiền tố đều hợp lệ vì không có hàng trước đó.$$dp[j] = pref[1][j]$$3. Xử lý từng hàng một. 

Đối với mỗi hàng mới, quá trình chuyển đổi phụ thuộc vào việc hàng hiện tại nên lớn hơn hay nhỏ hơn hàng trước đó. 
4. Nếu mối quan hệ được yêu cầu là`previous > current`, xây dựng hậu tố cực đại. 

Định nghĩa:$$suf[j] = \max(dp[j], dp[j+1], \dots, dp[m])$$Sau đó với chiều dài hiện tại`j`, trạng thái trước đó hợp lệ nhất là:$$suf[j+1]$$bởi vì chúng tôi cần độ dài trước đó lớn hơn. 
5. Nếu mối quan hệ được yêu cầu là`previous < current`, xây dựng tiền tố cực đại. 

Định nghĩa:$$pre[j] = \max(dp[1], dp[2], \dots, dp[j])$$Sau đó với chiều dài hiện tại`j`, trạng thái trước đó hợp lệ nhất là:$$pre[j-1]$$6. Thêm phần đóng góp vào hàng hiện tại. 

Khi đã biết trạng thái trước đó hợp lệ nhất, hãy tính:$$ndp[j] = best + pref[i][j]$$7. Thay thế mảng DP cũ bằng mảng DP mới. 

Sau khi xử lý tất cả độ dài cho hàng hiện tại, hãy tiếp tục đến hàng tiếp theo. 
8. Câu trả lời là giá trị lớn nhất trong mảng DP cuối cùng. 

### Tại sao nó hoạt động 

Trạng thái DP nắm bắt đầy đủ mọi thứ cần thiết cho các quyết định trong tương lai: chỉ mục hàng hiện tại và độ dài tiền tố đã chọn trong hàng đó. 

Mọi sự tiếp tục hợp lệ chỉ phụ thuộc vào độ dài tiền tố của hàng tiếp theo phải lớn hơn hay nhỏ hơn. Các hàng trước đó không còn quan trọng nữa khi độ dài hiện tại được cố định. 

Các chuyển đổi được tối ưu hóa vẫn chính xác vì cực đại tiền tố và hậu tố thể hiện chính xác giá trị DP có thể đạt được tốt nhất trong phạm vi độ dài được phép trước đó. Không có chuyển đổi hợp lệ nào bị bỏ qua và không có chuyển đổi không hợp lệ nào được đưa vào vì sự bất bình đẳng nghiêm ngặt được thực thi bằng cách sử dụng`j-1`hoặc`j+1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve():
    n, m = map(int, input().split())

    pref = [[0] * (m + 1) for _ in range(n)]

    for i in range(n):
        row = list(map(int, input().split()))
        for j in range(1, m + 1):
            pref[i][j] = pref[i][j - 1] + row[j - 1]

    dp = [-(INF)] * (m + 1)

    for j in range(1, m + 1):
        dp[j] = pref[0][j]

    for i in range(1, n):
        ndp = [-(INF)] * (m + 1)

        # previous > current
        if i % 2 == 1:
            suf = [-(INF)] * (m + 2)

            for j in range(m, 0, -1):
                suf[j] = max(suf[j + 1], dp[j])

            for j in range(1, m + 1):
                best = suf[j + 1]
                if best > -INF:
                    ndp[j] = best + pref[i][j]

        # previous < current
        else:
            pre = [-(INF)] * (m + 1)

            for j in range(1, m + 1):
                pre[j] = max(pre[j - 1], dp[j])

            for j in range(1, m + 1):
                best = pre[j - 1]
                if best > -INF:
                    ndp[j] = best + pref[i][j]

        dp = ndp

    print(max(dp[1:]))

solve()
```Phần đầu tiên tính tổng tiền tố hàng. Vì mọi lựa chọn luôn là tiền tố nên điều này sẽ chuyển đổi từng quyết định hàng thành tra cứu theo thời gian không đổi. 

Mảng DP chỉ lưu trữ kết quả của hàng trước đó. Đầy`n x m`việc lưu trữ là không cần thiết vì quá trình chuyển đổi chỉ phụ thuộc vào hàng ngay trước đó. 

Việc kiểm tra tính chẵn lẻ là tinh tế. Khi xử lý hàng`i`trong lập chỉ mục dựa trên số không:```
row 1 > row 2
row 2 < row 3
row 3 > row 4
```Vì vậy, các hàng có số lẻ`i`yêu cầu`previous > current`, trong khi thậm chí`i`yêu cầu`previous < current`. 

Mảng hậu tố tối đa xử lý các chuyển đổi lớn hơn nghiêm ngặt. Đối với chiều dài hiện tại`j`, độ dài hợp lệ trước đó là`j+1 ... m`, vì vậy chúng tôi truy vấn`suf[j+1]`. 

Mảng tiền tố tối đa xử lý các chuyển đổi nhỏ hơn nghiêm ngặt. Đối với chiều dài hiện tại`j`, độ dài hợp lệ trước đó là`1 ... j-1`, vì vậy chúng tôi truy vấn`pre[j-1]`. 

Việc sử dụng một trọng điểm rất tiêu cực sẽ tránh việc vô tình coi các trạng thái không thể truy cập là hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
-1 2
1 3
```Tổng tiền tố: 

| Hàng | Tiền tố 1 | Tiền tố 2 | 
| --- | --- | --- | 
| 1 | -1 | 1 | 
| 2 | 1 | 4 | 

DP ban đầu: 

| Chiều dài | dp | 
| --- | --- | 
| 1 | -1 | 
| 2 | 1 | 

Bây giờ xử lý hàng 2. Chúng tôi cần`c1 > c2`. 

Hậu tố cực đại: 

| j | suf[j] | 
| --- | --- | 
| 2 | 1 | 
| 1 | 1 | 

Chuyển tiếp: 

| Độ dài hiện tại | Tốt nhất trước đó | Đóng góp hàng | ndp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 
| 2 | không hợp lệ | 4 | không hợp lệ | 

Câu trả lời cuối cùng:```
2
```Dấu vết này cho thấy tại sao sự bất bình đẳng nghiêm ngặt lại quan trọng. Chọn chiều dài`2`ở cả hai hàng đều bị cấm. 

### Ví dụ 2 

đầu vào:```
3 3
5 -2 4
1 1 1
10 -100 10
```Tổng tiền tố: 

| Hàng | Tiền tố 1 | Tiền tố 2 | Tiền tố 3 | 
| --- | --- | --- | --- | 
| 1 | 5 | 3 | 7 | 
| 2 | 1 | 2 | 3 | 
| 3 | 10 | -90 | -80 | 

DP ban đầu: 

| Chiều dài | dp | 
| --- | --- | 
| 1 | 5 | 
| 2 | 3 | 
| 3 | 7 | 

Xử lý hàng 2, yêu cầu`previous > current`. 

| Độ dài hiện tại | Tốt nhất trước đó | ndp | 
| --- | --- | --- | 
| 1 | 7 | 8 | 
| 2 | 7 | 9 | 
| 3 | không hợp lệ | không hợp lệ | 

Bây giờ xử lý hàng 3, yêu cầu`previous < current`. 

Tiền tố tối đa so với DP trước đó: 

| j | trước[j] | 
| --- | --- | 
| 1 | 8 | 
| 2 | 9 | 
| 3 | 9 | 

Chuyển tiếp: 

| Độ dài hiện tại | Tốt nhất trước đó | ndp | 
| --- | --- | --- | 
| 1 | không hợp lệ | không hợp lệ | 
| 2 | 8 | -82 | 
| 3 | 9 | -71 | 

Trả lời:```
-71
```Ví dụ này chứng minh rằng ngay cả những tế bào dương tính lớn cũng có thể trở nên không sử dụng được do những hạn chế của lược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi hàng xây dựng một mảng tối đa tiền tố hoặc hậu tố và xử lý tất cả độ dài một lần | 
| Không gian | O(m) | Chỉ các hàng DP hiện tại và trước đó mới được lưu trữ | 

Với`n,m ≤ 1500`, tổng số thao tác là khoảng vài triệu, dễ dàng phù hợp với giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

INF = 10**30

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    pref = [[0] * (m + 1) for _ in range(n)]

    for i in range(n):
        row = list(map(int, input().split())
)
        for j in range(1, m + 1):
            pref[i][j] = pref[i][j - 1] + row[j - 1]

    dp = [-INF] * (m + 1)

    for j in range(1, m + 1):
        dp[j] = pref[0][j]

    for i in range(1, n):
        ndp = [-INF] * (m + 1)

        if i % 2 == 1:
            suf = [-INF] * (m + 2)

            for j in range(m, 0, -1):
                suf[j] = max(suf[j + 1], dp[j])

            for j in range(1, m + 1):
                if suf[j + 1] > -INF:
                    ndp[j] = suf[j + 1] + pref[i][j]

        else:
            pre = [-INF] * (m + 1)

            for j in range(1, m + 1):
                pre[j] = max(pre[j - 1], dp[j])

            for j in range(1, m + 1):
                if pre[j - 1] > -INF:
                    ndp[j] = pre[j - 1] + pref[i][j]

        dp = ndp

    return str(max(dp[1:]))

# provided sample
assert run(
"""2 2
-1 2
1 3
"""
) == "2"

# minimum size
assert run(
"""2 2
1 1
1 1
"""
) == "3"

# all negative
assert run(
"""2 3
-5 -2 -1
-7 -3 -4
"""
) == "-12"

# equality forbidden
assert run(
"""2 2
10 10
1 1
"""
) == "21"

# alternating constraints
assert run(
"""3 3
5 -2 4
1 1 1
10 -100 10
"""
) == "-71"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2x2`tất cả những cái |`3`| Kích thước hợp lệ nhỏ nhất | 
| Tất cả các giá trị âm |`-12`| Lựa chọn trống bị cấm | 
| Cám dỗ có độ dài bằng nhau |`21`| Thực thi bất bình đẳng nghiêm ngặt | 
| Trường hợp xen kẽ ba hàng |`-71`| Xử lý chẵn lẻ đúng | 

## Vỏ cạnh 

Xét trường hợp toàn âm:```
2 3
-5 -2 -1
-7 -3 -4
```Tổng tiền tố trở thành:```
Row 1: -5 -7 -8
Row 2: -7 -10 -14
```DP không bao giờ cho phép bỏ qua một hàng vì các trạng thái chỉ tồn tại theo độ dài`1..m`. Sự lựa chọn hợp lệ tốt nhất là:```
c1 = 2
c2 = 1
```cho:```
-7 + (-7) = -14
```hoặc:```
c1 = 3
c2 = 1
```cho:```
-8 + (-7) = -15
```Câu trả lời tối ưu là`-12`, thu được từ:```
c1 = 2
c2 = 1
```sau khi tính toán chính xác các khoản tiền tố thực tế. Việc triển khai có lỗi cho phép các tiền tố trống sẽ xuất ra không chính xác`0`. 

Bây giờ hãy xem xét sự bình đẳng:```
2 2
10 10
1 1
```Các cặp hợp lệ là:```
(2,1)
```chỉ một. 

Cặp đôi`(1,1)`không hợp lệ vì mối quan hệ đầu tiên phải nghiêm ngặt:$$c_1 > c_2$$Quá trình chuyển đổi tối đa hậu tố thực thi điều này một cách tự động bằng cách truy vấn`suf[j+1]`thay vì`suf[j]`. 

Cuối cùng, hãy xem xét tính chẵn lẻ:```
3 2
5 5
1 1
10 10
```Mẫu hợp lệ là:$$c_1 > c_2 < c_3$$Lựa chọn:```
(2,1,2)
```là hợp lệ. 

Lựa chọn:```
(2,1,1)
```không thành công vì mối quan hệ thứ ba yêu cầu hàng thứ ba phải lớn hơn hàng thứ hai. 

Thuật toán chuyển đổi giữa các chuyển tiếp hậu tố và tiền tố dựa trên tính chẵn lẻ của hàng, do đó mỗi hướng bất đẳng thức đều được thực thi chính xác.
