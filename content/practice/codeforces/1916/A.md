---
title: "CF 1916A - 2023"
description: "Chúng ta bắt đầu với một mảng a chưa xác định có tích tất cả các phần tử chính xác là 2023. Một số phần tử k đã bị loại bỏ, để lại mảng b có độ dài n. Nhiệm vụ là xác định liệu một mảng ban đầu như vậy có thể tồn tại hay không. Nếu có thể, chúng ta phải xuất ra bất kỳ tập hợp k số đã loại bỏ hợp lệ nào."
date: "2026-06-08T19:51:12+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1916
codeforces_index: "A"
codeforces_contest_name: "Good Bye 2023"
rating: 800
weight: 1916
solve_time_s: 155
verified: true
draft: false
---

[CF 1916A - 2023](https://codeforces.com/problemset/problem/1916/A) 

**Đánh giá:** 800 
**Tas:** xây dựng thuật toán, thực hiện, toán, lý thuyết số 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng không xác định`a`có tích của tất cả các phần tử chính xác là`2023`. Một số`k`các phần tử đã bị loại bỏ, để lại mảng`b`chiều dài`n`. 

Nhiệm vụ là xác định liệu một mảng ban đầu như vậy có thể tồn tại hay không. Nếu có thể, chúng ta phải xuất ra bất kỳ tập hợp hợp lệ nào của`k`số bị loại bỏ. Sau khi nhân tất cả các số trong`b`và tất cả các số chúng tôi xuất ra, kết quả phải chính xác`2023`. 

Quan sát quan trọng là thứ tự của các phần tử không quan trọng. Chỉ có sản phẩm là quan trọng. Nếu tích của các số còn lại quá lớn hoặc không phải là ước số của`2023`, không có cách nào để hoàn thành mảng. Ngược lại, chúng ta có thể chọn các số bị loại bỏ có tích thỏa mãn thừa số còn thiếu. 

Những hạn chế là cực kỳ nhỏ. Cả hai`n`Và`k`nhiều nhất là`5`, và có nhiều nhất`100`trường hợp thử nghiệm. Ngay cả việc tìm kiếm toàn diện trên nhiều khả năng cũng sẽ phù hợp một cách thoải mái trong giới hạn. Tuy nhiên, bài toán có một giải pháp toán học đơn giản chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Có một số trường hợp dễ bỏ sót. 

Giả sử tích còn lại không chia`2023`. 

đầu vào:```
1
2 1
5 2
```Sản phẩm của`b`là`10`. Từ`2023`không chia hết cho`10`, không có tập hợp số nguyên bổ sung nào có thể tạo ra kết quả cuối cùng chính xác`2023`. Câu trả lời đúng là:```
NO
```Một giải pháp bất cẩn chỉ kiểm tra xem sản phẩm có nhỏ hơn không`2023`sẽ chấp nhận trường hợp này một cách sai lầm. 

Một trường hợp quan trọng khác xảy ra khi tích còn lại đã bằng`2023`. 

đầu vào:```
1
1 1
2023
```Sản phẩm còn thiếu là`1`. Vì chúng ta phải xuất ra chính xác một số bị loại bỏ nên chúng ta có thể xuất ra:```
YES
1
```giá trị`1`bảo toàn tổng sản phẩm. Một giải pháp đòi hỏi phải tìm một hệ số lớn hơn`1`sẽ thất bại. 

Trường hợp cạnh thứ ba là khi cần có nhiều số bị loại bỏ. 

đầu vào:```
1
1 3
1
```Sản phẩm còn lại là`1`, do đó các số bị loại bỏ phải nhân lên`2023`. Một câu trả lời hợp lệ là:```
YES
2023 1 1
```Chúng tôi chỉ cần bất kỳ bộ sưu tập hợp lệ nào chính xác`k`những con số. Quên yêu cầu xuất chính xác`k`giá trị sẽ tạo ra một câu trả lời không hợp lệ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các bộ sưu tập có thể có của`k`loại bỏ các số và kiểm tra xem tích của chúng nhân với tích của`b`, bằng`2023`. 

Điều này hiệu quả vì sản phẩm mục tiêu là cố định và rất nhỏ. Tuy nhiên, ngay cả với những ràng buộc nhỏ, không gian tìm kiếm sẽ tăng nhanh nếu chúng ta cho phép các giá trị ứng cử viên tùy ý. Cách tiếp cận phụ thuộc vào các yếu tố đoán và kết hợp khám phá, điều này là không cần thiết. 

Cấu trúc của vấn đề gợi ý một hướng đi đơn giản hơn nhiều. Sản phẩm ban đầu được biết trước:```
2023 = 7 × 17 × 17
```Cho phép```
P = product of all elements in b
```Nếu một mảng ban đầu tồn tại thì```
P × (product of removed numbers) = 2023
```Điều này ngay lập tức ngụ ý rằng`P`phải chia`2023`. 

Nếu như`2023 % P != 0`, không có giải pháp tồn tại. 

Nếu không thì sản phẩm còn thiếu là```
M = 2023 / P
```Bây giờ chúng ta cần chính xác`k`đã loại bỏ các số có tích là`M`. 

Việc xây dựng dễ dàng nhất là đặt`M`là số bị loại bỏ đầu tiên và điền vào phần còn lại`k - 1`vị trí với`1`. Sản phẩm của họ vẫn còn`M`, vậy điều kiện được thỏa mãn. 

Điều này làm giảm toàn bộ vấn đề thành một kiểm tra chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng các yếu tố ứng cử viên | O(k) | Không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tích`P`của tất cả các phần tử trong mảng đã cho`b`. 
2. Kiểm tra xem`2023`chia hết cho`P`. 

Nếu như`2023 % P != 0`, đầu ra`"NO"`bởi vì không có tập hợp các số bị loại bỏ nào có thể tạo ra tổng sản phẩm chính xác`2023`. 
3. Ngược lại tính hệ số còn thiếu:```
M = 2023 / P
```4. Đầu ra`"YES"`. 
5. Viết các số đã loại bỏ thành:```
[M, 1, 1, ..., 1]
```với chính xác`k`những con số. 
6. Xuất danh sách này. 

### Tại sao nó hoạt động 

hãy để`P`là tích của mảng còn lại. 

Nếu như`P`không chia`2023`, thì tích của các số bị loại bỏ sẽ phải bằng`2023 / P`, không phải là số nguyên. Vì tất cả các phần tử của mảng đều là số nguyên nên không thể tồn tại mảng ban đầu hợp lệ. 

Nếu như`P`chia rẽ`2023`, định nghĩa`M = 2023 / P`. Các số bị loại bỏ được xây dựng có tích```
M × 1 × 1 × ... × 1 = M
```Do đó tích của tất cả các phần tử của mảng được xây dựng lại là```
P × M = P × (2023 / P) = 2023.
```Việc xây dựng luôn tạo ra chính xác`k`số, vì vậy mọi trường hợp được chấp nhận đều mang lại câu trả lời hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, k = map(int, input().split())
    b = list(map(int, input().split()))

    prod = 1
    for x in b:
        prod *= x

    if 2023 % prod != 0:
        print("NO")
    else:
        print("YES")
        missing = 2023 // prod
        ans = [missing] + [1] * (k - 1)
        print(*ans)
```Phần đầu tiên tính tích của các phần tử còn lại. Từ`n ≤ 5`và mọi giá trị nhiều nhất là`2023`, sản phẩm dễ dàng phù hợp với số học số nguyên của Python. 

Kiểm tra khả năng chia nhỏ là kiểm tra toàn bộ tính khả thi. Nếu tích còn lại không phải là ước của`2023`, việc xây dựng lại là không thể. 

Khi có thể xây dựng lại, mã sẽ tính hệ số còn thiếu`2023 // prod`. Giá trị đó trở thành phần tử bị loại bỏ đầu tiên. Các vị trí còn lại được lấp đầy`1`, không làm thay đổi sản phẩm và đảm bảo rằng chính xác`k`số được in. 

Thứ tự nhân là không liên quan vì chỉ có sản phẩm cuối cùng mới quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4 2
1 289 1 1
```| Bước | Giá trị | 
| --- | --- | 
| Sản phẩm của`b`| 289 | 
| Kiểm tra`2023 % 289`| 0 | 
| Thiếu yếu tố | 7 | 
| Số đầu ra | 7, 1 | 

Xác minh:```
289 × 7 × 1 = 2023
```Dấu vết cho thấy trường hợp thành công điển hình. Sản phẩm còn lại đã chứa hầu hết các hệ số của`2023`và chỉ thiếu một yếu tố. 

### Ví dụ 2 

đầu vào:```
1
2 2
5 2
```| Bước | Giá trị | 
| --- | --- | 
| Sản phẩm của`b`| 10 | 
| Kiểm tra`2023 % 10`| 3 | 
| Chia được? | Không | 
| Kết quả | KHÔNG | 

Từ`10`không phải là ước của`2023`, không có tích số nguyên nào có thể lấp đầy khoảng trống. Thuật toán ngay lập tức từ chối trường hợp thử nghiệm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Nhân tất cả các phần tử của`b`một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với`n ≤ 5`và nhiều nhất`100`trường hợp thử nghiệm, thời gian chạy thực tế là không đổi. Giải pháp dễ dàng đáp ứng các giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = []
    t = int(input())

    for _ in range(t):
        n, k = map(int, input().split())
        b = list(map(int, input().split()))

        prod = 1
        for x in b:
            prod *= x

        if 2023 % prod != 0:
            out.append("NO")
        else:
            out.append("YES")
            missing = 2023 // prod
            ans = [missing] + [1] * (k - 1)
            out.append(" ".join(map(str, ans)))

    return "\n".join(out) + "\n"

# sample-style checks
assert run(
"""1
4 2
1 289 1 1
"""
) == "YES\n7 1\n"

assert run(
"""1
2 2
5 2
"""
) == "NO\n"

# minimum size
assert run(
"""1
1 1
2023
"""
) == "YES\n1\n"

# product already 1, multiple removed values
assert run(
"""1
1 3
1
"""
) == "YES\n2023 1 1\n"

# exact factorization present
assert run(
"""1
3 1
7 17 17
"""
) == "YES\n1\n"

# non-divisor larger than a factor
assert run(
"""1
1 1
289
"""
) == "YES\n7\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2023`với`k=1`|`YES 1`| Sản phẩm đã hoàn thiện | 
|`1`với`k=3`|`YES 2023 1 1`| Số lượng phần tử chính xác bị loại bỏ | 
|`7 17 17`|`YES 1`| Thiếu hệ số bằng 1 | 
|`5 2`|`NO`| Từ chối không chia | 
|`289`|`YES 7`| Thiếu hệ số lớn hơn 1 | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 1
5 2
```Sản phẩm của`b`là`10`. Thuật toán kiểm tra:```
2023 % 10 = 3
```Vì số dư khác 0 nên nó xuất ra:```
NO
```Không có thừa số nguyên nào có thể biến đổi`10`vào trong`2023`. 

Coi như:```
1
1 1
2023
```Sản phẩm của`b`đã rồi`2023`.```
M = 2023 / 2023 = 1
```Các đầu ra thuật toán:```
YES
1
```nhân với`1`giữ cho tổng sản phẩm không thay đổi. 

Coi như:```
1
1 3
1
```Sản phẩm của`b`là`1`, Vì thế```
M = 2023
```Các số bị loại bỏ được xây dựng là:```
2023 1 1
```Sản phẩm của họ vẫn`2023`, và có đúng ba số được tạo ra theo yêu cầu. 

Coi như:```
1
3 1
7 17 7
```Sản phẩm của`b`là```
833
```Từ```
2023 % 833 != 0
```đầu ra của thuật toán:```
NO
```Điều này chứng tỏ rằng ngay cả khi mỗi phần tử riêng lẻ được phân chia`2023`, tích kết hợp của chúng có thể không, và việc kiểm tra tính chia hết sẽ phát hiện lỗi một cách chính xác.
