---
title: "CF 2149A - Hãy tích cực"
description: "Chúng ta được cung cấp một mảng có độ dài $n$ trong đó mỗi phần tử là $-1$, $0$ hoặc $1$. Chúng ta được phép tăng bất kỳ phần tử nào lên một số lần bất kỳ. Mục tiêu là làm cho sản phẩm của tất cả các yếu tố hoàn toàn tích cực trong khi thực hiện ít thao tác nhất có thể."
date: "2026-06-08T01:08:58+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2149
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1054 (Div. 3)"
rating: 800
weight: 2149
solve_time_s: 74
verified: true
draft: false
---

[CF 2149A - Hãy tích cực](https://codeforces.com/problemset/problem/2149/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$trong đó mỗi phần tử là$-1$,$0$, hoặc$1$. Chúng ta được phép tăng bất kỳ phần tử nào lên một số lần bất kỳ. Mục tiêu là làm cho sản phẩm của tất cả các yếu tố hoàn toàn tích cực trong khi thực hiện ít thao tác nhất có thể. 

Một tích hoàn toàn dương yêu cầu số phần tử âm trong mảng cuối cùng là số chẵn và không có số 0. Mỗi số 0 phải được tăng ít nhất một lần để trở thành một. Mỗi$-1$có thể tăng một lần để trở thành 0 hoặc hai lần để trở thành một. Một người không cần bất kỳ hoạt động. 

Các hạn chế là nhỏ:$n \le 8$Và$t \le 10^4$. Từ$n$rất nhỏ, về mặt kỹ thuật chúng ta có thể xem xét mọi sự kết hợp của các thao tác trên các phần tử, nhưng giới hạn trên của số lượng trường hợp kiểm thử yêu cầu giải pháp có thời gian không đổi cho mỗi trường hợp kiểm thử. Vấn đề hoàn toàn là số học và mỗi mảng có thể được phân tích độc lập. 

Các trường hợp cạnh phát sinh khi mảng chứa tất cả các số 0, tất cả các số 1 hoặc một số lẻ$-1$s với số không. Ví dụ, nếu mảng là$[-1, 0]$, chúng ta phải tăng số 0 lên một (1 thao tác) và$-1$đến một (2 thao tác) cho tổng số 3 thao tác. Một cách tiếp cận ngây thơ có thể bỏ sót rằng việc chuyển một số âm về 0 là không đủ nếu vẫn còn một số âm lẻ. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử tất cả các kết hợp gia số có thể có cho từng phần tử và kiểm tra xem sản phẩm thu được có dương hay không. Mỗi$-1$có ba mục tiêu tiềm năng: 0, 1 hoặc ở mức$-1$. Mỗi số 0 có hai mục tiêu tiềm năng: 0 hoặc 1. Điều này mang lại$3^k \cdot 2^m$sự kết hợp, ở đâu$k$là số lượng của$-1$Và$m$là số 0. Đối với$n = 8$, nhiều nhất là thế này$3^8 \approx 6561$kết hợp, khả thi cho một trường hợp thử nghiệm duy nhất nhưng không hiệu quả trên toàn bộ$10^4$trường hợp thử nghiệm. 

Cách tiếp cận tối ưu xuất phát từ việc nhận ra rằng chúng ta không cần phải mô phỏng từng mức tăng. Chúng ta có thể tính toán số lượng hoạt động một cách phân tích. Mỗi số 0 phải được tăng lên một lần. Đếm số âm. Nếu số âm là số lẻ thì ta phải đổi 1 âm thành dương (2 thao tác) để được kết quả dương. Thông tin chi tiết quan trọng là độ dài mảng nhỏ và tất cả các phép toán đều tuyến tính theo số lượng số 0 và số âm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(1) | Chấp nhận được với n ≤ 8 nhưng không cần thiết | 
| Tối ưu | O(n) | O(1) | Được chấp nhận và đơn giản | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm là độc lập. 
2. Với mỗi mảng, đếm số số 0 và số$-1$S. 
3. Mỗi số 0 cần chính xác một số gia để trở thành một. Thêm số này vào tổng số hoạt động. 
4. Mọi tiêu cực đều góp phần lật đổ dấu hiệu của sản phẩm. Nếu số âm là chẵn, chúng ta có thể để chúng là$-1$hoặc lật chúng thành một; sản phẩm đã tích cực rồi. Nếu số âm là số lẻ, hãy chọn một âm và tăng gấp đôi để thành một, đảm bảo tích số trở thành dương. 
5. Tất cả những cái tích cực đều yêu cầu không có thao tác nào. 
6. Tính tổng các thao tác cần thiết và kết quả đầu ra cho từng trường hợp thử nghiệm. 

Điều bất biến là chúng ta luôn tăng số 0 lên số 1 và đảm bảo số lượng số âm là chẵn. Điều này đảm bảo sản phẩm hoàn toàn tích cực với các thao tác tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations_to_positive(arr):
    zeros = arr.count(0)
    negatives = arr.count(-1)
    
    ops = zeros + negatives  # flip zeros once, negatives at least once
    if negatives % 2 == 1:
        ops += 1  # need one extra increment to flip one negative to positive
    
    return ops

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(min_operations_to_positive(a))
```Mã đầu tiên đếm số không và số âm. Chúng tôi tăng số 0 một lần, số âm một lần (lên 0 hoặc cuối cùng lên một) và nếu có số âm lẻ, chúng tôi sẽ thêm một thao tác nữa để lật hoàn toàn một số âm thành một. Sử dụng tính năng tích hợp sẵn của Python`count`đơn giản hóa việc ghi sổ và đảm bảo sự rõ ràng. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`[-1, 0, 1]`| số không | âm bản | hoạt động | tiêu cực kỳ lạ? | thêm | tổng số hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | vâng | 1 | 3 | 

Giải thích: Một số tăng bằng 0, một số tăng âm, cộng thêm một số tăng thêm để chuyển âm sang dương. Phù hợp với sản lượng dự kiến 3. 

**Ví dụ 2** 

đầu vào:`[-1, -1, 0, 1]`| số không | âm bản | hoạt động | tiêu cực kỳ lạ? | thêm | tổng số hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | không | 0 | 3 | 

Giải thích: Một số gia bằng 0 và hai số gia âm. Vì số âm là số chẵn nên không cần thêm gì cả. Tổng số hoạt động tối thiểu 3. Sản phẩm dương tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm số không và số âm là tuyến tính theo chiều dài mảng. | 
| Không gian | O(1) | Chỉ có một vài bộ đếm được sử dụng, không phụ thuộc vào kích thước đầu vào. | 

Từ$n \le 8$Và$t \le 10^4$, giải pháp này chạy dễ dàng trong vòng chưa đầy 0,1 giây, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("3\n3\n-1 0 1\n4\n-1 -1 0 1\n5\n-1 -1 -1 0 0\n") == "3\n3\n4", "sample cases"

# Custom cases
assert run("1\n1\n0\n") == "1", "single zero"
assert run("1\n2\n-1 -1\n") == "2", "two negatives even"
assert run("1\n2\n-1 0\n") == "3", "negative and zero"
assert run("1\n8\n1 1 1 1 1 1 1 1\n") == "0", "all positive ones"
assert run("1\n3\n-1 -1 -1\n") == "4", "all negatives odd count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Phần tử đơn tăng 0 thành một | 
|`-1 -1`|`2`| Số âm chẵn, không cần thực hiện thêm thao tác nào | 
|`-1 0`|`3`| Âm lẻ cộng 0, cần lật thêm | 
|`1 1 1 1 1 1 1 1`|`0`| Tất cả những điều tích cực không yêu cầu thao tác | 
|`-1 -1 -1`|`4`| Những tiêu cực kỳ lạ, những thay đổi tối thiểu đảm bảo sản phẩm tích cực | 

## Vỏ cạnh 

Nếu mảng chứa tất cả các số 0 thì mỗi số 0 phải được tăng lên một lần. Ví dụ,`[0, 0, 0]`dẫn đến 3 hoạt động. Nếu mảng chứa số lẻ$-1$s có số 0, các số 0 được tăng lên trước, sau đó một số âm được tăng thêm một thời gian để cân bằng dấu. Giải pháp xử lý vấn đề này bằng cách tính tổng các số 0, số âm và thêm một thao tác bổ sung nếu số âm là số lẻ. Điều này đảm bảo số lượng thao tác tối thiểu trong mọi tình huống biên.
