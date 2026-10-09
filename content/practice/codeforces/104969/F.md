---
title: "CF 104969F - Pizza Stack"
description: "Chúng ta được cấp một bộ pizza được dán nhãn từ 1 đến n, trong đó mỗi nhãn cũng là bán kính của nó. Chúng ta phải sắp xếp tất cả các loại pizza thành một chồng dọc, tương đương với việc chọn một hoán vị các số từ 1 đến n."
date: "2026-06-28T18:26:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104969
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 1 (Advanced)"
rating: 0
weight: 104969
solve_time_s: 61
verified: true
draft: false
---

[CF 104969F - Ngăn xếp Pizza](https://codeforces.com/problemset/problem/104969/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ pizza được dán nhãn từ 1 đến n, trong đó mỗi nhãn cũng là bán kính của nó. Chúng ta phải sắp xếp tất cả các loại pizza thành một chồng dọc, tương đương với việc chọn một hoán vị các số từ 1 đến n. 

Đối với hai chiếc pizza bất kỳ trong ngăn xếp, hãy xem xét chiếc có hình dáng thấp hơn và chiếc ở trên nó. Nếu chiếc bánh pizza phía dưới có bán kính lớn hơn chiếc bánh phía trên, thì cặp đó sẽ góp phần tạo nên cái mà bài toán gọi là “cặp thích hợp”. Trong ngôn ngữ hoán vị, đây chính xác là một sự đảo ngược: một cặp chỉ số i < j sao cho giá trị tại i lớn hơn giá trị tại j. 

Vì vậy, nhiệm vụ trở thành đếm xem có bao nhiêu hoán vị có kích thước n chứa chính xác k nghịch đảo. 

Ràng buộc n ₫ 1000 và k ₫ 1000 ngay lập tức cho thấy rằng chúng ta không làm việc trực tiếp trong không gian giai thừa hoặc hàm mũ. Một phép liệt kê ngây thơ trên tất cả các hoán vị sẽ liên quan đến n! những khả năng đã trở thành không thể xảy ra ở khoảng n = 12 hoặc 13. Ngay cả n = 100 cũng vượt quá tầm với. Điều này thúc đẩy chúng tôi hướng tới một phương pháp lập trình động trong đó chúng tôi đếm các hoán vị bằng cách chèn dần các phần tử và theo dõi số lượng đảo ngược được hình thành. 

Các trường hợp Edge chủ yếu là lỗi về cấu trúc hơn là lỗi triển khai. Khi k = 0, chỉ có hoán vị tăng dần mới có tác dụng. Khi k đạt cực đại, k = n(n − 1)/2, chỉ có hoán vị giảm mới có tác dụng. Bất kỳ giải pháp nào cũng phải xử lý chính xác các trường hợp cực đoan này mà không dựa vào các giả định như k < n hoặc k nhỏ so với n, mặc dù k ≤ 1000 giới hạn không gian trạng thái DP. 

Một trường hợp tinh vi là khi n lớn nhưng k nhỏ. Ví dụ, n = 1000 và k = 1 vẫn yêu cầu suy luận về cách có thể tạo ra một nghịch đảo duy nhất bằng cách đặt chính xác một phần tử không theo thứ tự trong số nhiều vị trí tương đối đã cố định. Đây là nơi mà các tổ hợp đơn giản có xu hướng thất bại trừ khi cấu trúc DP được xác định cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các hoán vị từ 1 đến n và đếm các nghịch đảo cho mỗi hoán vị. Việc tính toán các nghịch đảo trên mỗi hoán vị mất O(n^2) và có n! hoán vị, do đó tổng công việc là O(n! · n^2), điều này gần như không thể thực hiện được ngay lập tức. 

Chúng ta cần một cấu trúc có thể xây dựng các hoán vị tăng dần. Quan sát chính là hãy nghĩ đến việc chèn từng số một theo thứ tự tăng dần của nhãn. Giả sử chúng ta đã xây dựng một cách sắp xếp hợp lệ các số từ 1 đến i − 1. Khi chèn số i, chúng ta có thể đặt nó ở bất kỳ vị trí nào trong số các phần tử i − 1 hiện có. Nếu chúng ta chèn nó vào vị trí t từ bên trái, nó sẽ tạo ra chính xác i − 1 − t phép đảo ngược mới, bởi vì nó sẽ được đặt trước nhiều phần tử nhỏ hơn đó. 

Điều này mang lại sự lặp lại rõ ràng: mỗi lần chèn đóng góp một số lần đảo ngược mới có thể kiểm soát được chỉ tùy thuộc vào vị trí của nó. Sự độc lập đó là điều cho phép lập trình động. 

Chúng ta định nghĩa dp[i][j] là số hoán vị của số i đầu tiên có chính xác j nghịch đảo. Với mỗi i, chúng ta thử tất cả các vị trí chèn có thể có của i vào một hoán vị có kích thước i − 1 và tích lũy các đóng góp nghịch đảo. 

Quá trình chuyển đổi trực tiếp sẽ thử các vị trí O(i) cho mỗi trạng thái, dẫn đến tổng thời gian là O(n^3). Tuy nhiên, chúng tôi có thể tối ưu hóa bằng cách sử dụng tổng tiền tố trên hàng dp trước đó. Điều này biến quá trình chuyển đổi bên trong thành O(1), giảm nghiệm đầy đủ thành O(nk). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n^2) | O(n) | Quá chậm | 
| DP với tổng tiền tố | O(nk) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một bảng dp trong đó dp[i][j] đếm các hoán vị của {1..i} với chính xác j nghịch đảo.

1. Khởi tạo dp[1][0] = 1. Một phần tử có đúng một hoán vị và không có phép nghịch đảo. Điều này neo giữ việc xây dựng. 
2. Với mỗi i từ 2 đến n, chúng ta tính dp[i] từ dp[i − 1]. Ở giai đoạn này, chúng tôi giả định rằng tất cả số lượng cho các tập hợp nhỏ hơn đều đã chính xác. 
3. Đối với i cố định, hãy xem xét việc chèn phần tử i vào mọi vị trí có thể có của hoán vị có kích thước i − 1. Nếu chúng ta chèn nó vào vị trí p (được lập chỉ mục 0), nó sẽ góp phần đảo ngược (i − 1 − p). 
4. Chuyển biểu thức này thành phép truy hồi: dp[i][j] bằng tổng của dp[i − 1][j − t] trên tất cả t từ 0 đến i − 1, trong đó t là số lần đảo ngược được tạo ra bằng cách chèn i. Đây là tổng cửa sổ trượt trên dp[i − 1]. 
5. Tính toán dp[i][j] một cách hiệu quả bằng cách sử dụng tổng tiền tố của dp[i − 1]. Đối với mỗi j, chúng tôi duy trì tổng cửa sổ đang chạy trên các giá trị i cuối cùng của dp[i − 1]. 
6. Đảm bảo rằng chúng ta chỉ xét j đến k, vì các giá trị lớn hơn không liên quan đến câu trả lời. 

Lý do chính khiến điều này có hiệu quả là việc chèn phần tử lớn nhất i không làm xáo trộn thứ tự tương đối giữa 1..i − 1, do đó tất cả cấu trúc đảo ngược chỉ xuất phát từ vị trí của nó. Mọi hoán vị có kích thước i được hình thành duy nhất bằng cách chèn i vào đúng một vị trí trong hoán vị có kích thước i − 1, do đó DP bao phủ tất cả các trạng thái mà không trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, k = map(int, input().split())

dp = [[0] * (k + 1) for _ in range(n + 1)]
dp[1][0] = 1

for i in range(2, n + 1):
    window_sum = 0
    for j in range(0, k + 1):
        window_sum += dp[i - 1][j]
        if j - i >= 0:
            window_sum -= dp[i - 1][j - i]
        dp[i][j] = window_sum % MOD

print(dp[n][k] % MOD)
```Mã xây dựng từng hàng DP. các`window_sum`duy trì một cửa sổ trượt có kích thước i trên hàng trước, tương ứng chính xác với vị trí chèn có thể có của i của số hiện tại i. Mỗi dp[i][j] tích lũy các đóng góp từ dp[i − 1][j], dp[i − 1][j − 1], ..., dp[i − 1][j − (i − 1)]. 

Một cạm bẫy phổ biến là quên rằng kích thước cửa sổ tăng theo i. Một vấn đề tế nhị khác là xử lý ranh giới khi j − i trở thành số âm, không được lập chỉ mục vào mảng. Modulo được áp dụng sau mỗi lần cập nhật trạng thái để tránh tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 3, k = 0 

Chúng tôi tính toán dp theo hàng. 

| tôi | j | nguồn window_sum | dp[i][j] | 
| --- | --- | --- | --- | 
| 1 | 0 | căn cứ | 1 | 
| 2 | 0 | dp[1][0] | 1 | 
| 2 | 1 | dp[1][0] (ca) | 1 | 
| 3 | 0 | dp[2][0] | 1 | 

Với n = 3, chỉ có hoán vị tăng dần 1 2 3 có nghịch đảo bằng 0. Bất kỳ sự hoán đổi nào cũng đều có ít nhất một sự đảo ngược, vì vậy câu trả lời là 1. 

### Ví dụ 2: n = 3, k = 1 

| tôi | j | đóng góp | dp[i][j] | 
| --- | --- | --- | --- | 
| 1 | 0 | căn cứ | 1 | 
| 2 | 1 | chèn 2 trước 1 | 1 | 
| 3 | 1 | từ dp[2][1] + dp[2][0] | 2 | 

Với n = 3 và k = 1, các hoán vị hợp lệ là 1 3 2 và 2 1 3. Mỗi hoán vị tương ứng với chính xác một nghịch đảo được hình thành bởi một hoán vị cục bộ duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi hàng dp được tính toán bằng một cửa sổ trượt trên k trạng thái | 
| Không gian | O(nk) | Bảng DP đầy đủ có kích thước n × k | 

Với n, k ≤ 1000, giải pháp thực hiện khoảng 10^6 lần chuyển đổi, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import comb

    MOD = 10**9 + 7

    n, k = map(int, inp.split())
    dp = [[0] * (k + 1) for _ in range(n + 1)]
    dp[1][0] = 1

    for i in range(2, n + 1):
        window_sum = 0
        for j in range(k + 1):
            window_sum += dp[i - 1][j]
            if j - i >= 0:
                window_sum -= dp[i - 1][j - i]
            dp[i][j] = window_sum % MOD

    return str(dp[n][k] % MOD)

# provided samples
assert run("3 0") == "1"
assert run("3 1") == "2"

# custom cases
assert run("1 0") == "1"
assert run("4 0") == "1"
assert run("4 6") == "1"
assert run("5 1") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | trường hợp cơ sở phần tử đơn | 
| 4 0 | 1 | chỉ hoán vị được sắp xếp | 
| 4 6 | 1 | hoán vị đảo ngược hoàn toàn | 
| 5 1 | 4 | vị trí đảo ngược đơn | 

## Vỏ cạnh 

Với n = 1 và k = 0, DP khởi tạo dp[1][0] = 1 và ngay lập tức trả về nó. Không có bước chuyển tiếp nên không có nguy cơ truy cập dp[0] hoặc các chỉ mục không hợp lệ. 

Với n = 4 và k = 0, cửa sổ trượt không bao giờ tích lũy bất kỳ đóng góp tích cực nào ngoài dp[i][0], vì tất cả số lần đảo ngược cao hơn đều không liên quan. Thuật toán bảo toàn dp[i][0] = 1 ở mọi cấp độ vì việc chèn phần tử lớn nhất vào cuối sẽ tạo ra nghịch đảo bằng 0 và tất cả các phần chèn thêm khác sẽ được lọc ra theo giới hạn k. 

Với n = 4 và k = 6, là số lần đảo ngược tối đa cho 4 phần tử, DP chỉ đếm chính xác hoán vị đảo ngược hoàn toàn. Cửa sổ trượt tự nhiên tích lũy chính xác một đường xây dựng hợp lệ thông qua các vị trí bắt buộc liên tiếp ở đầu mỗi hoán vị.
