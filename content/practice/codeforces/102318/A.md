---
title: "CF 102318A - Hóa đơn tiền điện"
description: "Bài toán mô hình hóa hai hộ gia đình có mức tiêu thụ điện là số nguyên dương. Công ty điện lực sử dụng biểu giá lũy tiến: 100 CWh đầu tiên có giá 2 Americus mỗi chiếc, 9.900 tiếp theo có giá 3 chiếc, 990.000 tiếp theo có giá 5 chiếc và mỗi đơn vị vượt quá 1.000.000 có giá 7."
date: "2026-08-13T05:08:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "A"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 193
verified: true
draft: false
---

[CF 102318A - Hóa đơn tiền điện](https://codeforces.com/problemset/problem/102318/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô hình hóa hai hộ gia đình có mức tiêu thụ điện là số nguyên dương. Công ty điện lực sử dụng biểu giá lũy tiến: 100 CWh đầu tiên có giá 2 Americus mỗi chiếc, 9.900 tiếp theo có giá 3 chiếc, 990.000 tiếp theo có giá 5 chiếc và mỗi đơn vị vượt quá 1.000.000 có giá 7. Bài toán ban đầu cho chúng ta hai con số,`A`Và`B`.`A`là hóa đơn sẽ xảy ra nếu tổng mức tiêu dùng của cả hai hộ gia đình trước khi áp dụng biểu giá.`B`là sự khác biệt tuyệt đối giữa các hóa đơn riêng lẻ của họ. Chúng ta được đảm bảo rằng chính xác một cặp tiêu dùng sẽ tạo ra hai giá trị này và chúng ta phải xuất ra hóa đơn cá nhân của hộ gia đình nhỏ hơn. Đầu vào chứa nhiều trường hợp thử nghiệm và kết thúc bằng`0 0`. 

Nhiệm vụ đầu tiên là phục hồi tổng lượng tiêu thụ từ`A`. Điều này có thể thực hiện được vì hàm thuế quan ngày càng tăng. Khi đã biết tổng mức tiêu thụ, hãy gọi nó`C`, hai mức tiêu dùng cá nhân phải là`x`Và`C - x`. Vì chúng ta được hứa rằng mức tiêu dùng của chúng ta không lớn hơn của người hàng xóm nên chúng ta chỉ cần xem xét`1 <= x <= C / 2`. 

Giới hạn số là`A <= 10^9`. Điều đó có nghĩa là tổng mức tiêu thụ có thể vào khoảng 10^8, vì mức thuế cao nhất là 7 trên mỗi đơn vị. Quét tuyến tính trên mọi khả năng có thể`x`do đó sẽ yêu cầu hàng chục triệu lần lặp cho một trường hợp thử nghiệm. Với giới hạn 1 giây, như vậy là quá nhiều công việc, đặc biệt là trong Python. Thay vào đó, cấu trúc của biểu giá mang lại cho chúng ta một hàm đơn điệu, cho phép chúng ta sử dụng tìm kiếm nhị phân và giảm việc tìm kiếm xuống còn khoảng 30 lần lặp. 

Có một số trường hợp ranh giới trong đó việc triển khai trực tiếp có thể thất bại. Đầu tiên là trường hợp không kết thúc hợp lệ nhỏ nhất. Vì```
6 2
```mức tiêu dùng kết hợp là 3, bởi vì`bill(3) = 6`. Cách chia duy nhất có thể xảy ra là 1 và 2, có hóa đơn là 2 và 4, vì vậy câu trả lời là```
2
```Việc triển khai bất cẩn giả định rằng mọi trường hợp thử nghiệm đều có mức tiêu thụ đủ lớn để đưa vào phạm vi biểu giá thứ hai có thể tính toán điều này không chính xác. 

Ranh giới thuế quan ở đúng 100 đơn vị là một nguồn sai sót phổ biến khác. Coi như```
200 4
```Mức tiêu thụ kết hợp là 100. Phần chia duy nhất là 49 và 51. Hóa đơn của họ là 98 và 102, cho chênh lệch là 4, vì vậy câu trả lời là```
98
```sử dụng`100`vì kích thước của phạm vi đầu tiên thay vì tính phí chính xác 100 đơn vị đầu tiên ở mức 2 là cần thiết ở đây. 

Ranh giới ngay sau khoảng thuế đầu tiên cũng có ý nghĩa tương tự. Tổng mức tiêu thụ 201 đơn vị có hoá đơn`503`, vì 100 đơn vị đầu tiên có giá 200 và 101 đơn vị tiếp theo có giá 303. Một giải pháp vô tình sử dụng tỷ lệ thứ hai cho đơn vị thứ 100 sẽ thay đổi mọi phép tính sau đó. 

Phạm vi thuế cao nhất cũng có vấn đề. Ví dụ,```
4979977 4979968
```tương ứng với mức tiêu dùng 1.000.010 và 1, do đó hộ gia đình nhỏ hơn trả 2. Mức tiêu dùng kết hợp vượt qua ranh giới 1.000.000 đơn vị, do đó nhánh thuế cuối cùng phải được thực hiện chính xác. Trường hợp ranh giới này rất hữu ích vì việc triển khai có thể xuất hiện đúng trên các giá trị thông thường nhưng lại thất bại ngay khi mức tiêu thụ vượt quá 1.000.000. 

Cuối cùng,`0 0`không phải là trường hợp bình thường. Nó là thiết bị đầu cuối đầu vào. Đặc biệt, mặc dù các hóa đơn bằng nhau sẽ tạo ra`B = 0`, các ca kiểm thử thông thường đều có kết quả dương tính`B`, do đó, đầu vào có sai phân bằng 0 duy nhất mà chúng ta mong đợi là cặp kết thúc. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản trước tiên sẽ chuyển đổi`A`vào tổng mức tiêu thụ`C`, sau đó thử mọi mức tiêu thụ nhỏ hơn có thể`x`từ 1 đến`C / 2`. Với mỗi ứng viên, nó sẽ tính toán`bill(x)`Và`bill(C - x)`, so sánh sự khác biệt của chúng với`B`và dừng lại khi tìm thấy sự khác biệt cần thiết. Điều này hiệu quả vì mọi phần tách có thể đều được kiểm tra rõ ràng và câu lệnh đảm bảo rằng chính xác một phần tách là hợp lệ. 

Vấn đề là kích thước của không gian tìm kiếm. Khi`A`gần với`10^9`, tổng lượng tiêu thụ tương ứng là khoảng 143 triệu chiếc. Một nửa trong số đó là khoảng 71 triệu lượt chia ứng cử viên. Mỗi ứng viên cần ít nhất một vài phép tính số học và đánh giá biểu giá, vì vậy trường hợp xấu nhất là khoảng 71 triệu lần lặp cho mỗi trường hợp thử nghiệm. Điều đó không tương thích với giới hạn 1 giây. 

Nhận xét quan trọng là sự khác biệt giữa hai tờ tiền thay đổi một cách đơn điệu khi chúng ta di chuyển phần chia tách. Cho phép`D(x) = bill(C - x) - bill(x)`vì`x <= C / 2`. BẰNG`x`tăng lên, hộ gia đình nhỏ tiêu dùng nhiều hơn và hộ gia đình lớn hơn tiêu dùng ít hơn. Bởi vì`bill`đang gia tăng nghiêm trọng,`bill(x)`tăng trong khi`bill(C - x)`giảm đi. Do đó,`D(x)`giảm nghiêm ngặt. 

Điều đó biến tìm kiếm brute-force thành tìm kiếm nhị phân. Nếu như`D(x)`lớn hơn`B`, hộ gia đình chúng tôi chọn vẫn còn quá nhỏ nên chúng tôi tăng`x`. Nếu như`D(x)`nhỏ hơn`B`, chúng tôi đã làm cho hộ gia đình nhỏ trở nên quá lớn nên chúng tôi giảm`x`. Việc đảm bảo giải pháp duy nhất có nghĩa là mục tiêu chính xác cuối cùng sẽ được tìm thấy. 

Có một cách đơn giản hóa hữu ích hơn. Chúng ta không cần phải tìm kiếm tổng mức tiêu thụ có thể để phục hồi`C`. Hàm thuế quan có thể được đảo ngược trực tiếp vì mỗi phạm vi có một mức giá cố định. Ví dụ: hóa đơn trên 29.900 nhưng nhiều nhất là 4.979.900 tương ứng với`10,000 + (A - 29,900) / 5`đơn vị. Từ`A`được đảm bảo đến từ mức tiêu dùng kết hợp thực tế, việc phân chia là chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C) | O(1) | Quá chậm | 
| Tối ưu | O(log C) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`A`Và`B`. Nếu cả hai đều bằng 0, hãy dừng lại vì đây là trọng điểm đánh dấu sự kết thúc của đầu vào. 
2. Chuyển đổi hóa đơn tổng hợp`A`vào mức tiêu dùng kết hợp`C`. Thuế suất nghịch đảo được tính theo từng phần, sử dụng chi phí tích lũy 200, 29.900 và 4.979.900 tại ba ranh giới thuế quan. 
3. Đặt phạm vi tìm kiếm nhị phân thành`lo = 1`Và`hi = C // 2`. Chúng tôi chỉ tìm kiếm tối đa một nửa tổng mức tiêu dùng vì bài toán xác định hộ gia đình của chúng tôi là hộ tiêu dùng không nhiều hơn người hàng xóm. 
4. Chọn`mid = (lo + hi) // 2`và để hai mức tiêu thụ là`mid`Và`C - mid`. 
5. Tính toán hai hóa đơn riêng lẻ và sự khác biệt của chúng,`diff = bill(C - mid) - bill(mid)`. Hóa đơn thứ hai ít nhất là hóa đơn đầu tiên vì`mid <= C - mid`, vì vậy giá trị tuyệt đối là không cần thiết trong quá trình tìm kiếm. 
6. Nếu`diff == B`, chúng tôi đã tìm thấy sự phân chia duy nhất và`bill(mid)`là câu trả lời cần thiết. 
7. Nếu`diff > B`, tăng`mid`bằng cách di chuyển`lo`ĐẾN`mid + 1`. Việc tăng mức tiêu dùng của hộ gia đình nhỏ hơn làm cho hai hóa đơn trở nên gần nhau hơn, điều này làm giảm`diff`. 
8. Nếu`diff < B`, giảm bớt`mid`bằng cách di chuyển`hi`ĐẾN`mid - 1`. Chúng ta đã tiến quá xa tới mức tiêu dùng bình đẳng, do đó chênh lệch hóa đơn đã trở nên nhỏ hơn mức yêu cầu. 

Điều bất biến là mức tiêu thụ nhỏ hơn chính xác vẫn ở bên trong`[lo, hi]`. chức năng`D(x)`đang giảm dần nên mọi so sánh đều cho chúng ta biết nửa nào không thể chứa câu trả lời. Khi`D(mid) > B`, mọi giá trị bằng hoặc thấp hơn`mid`tạo ra sự khác biệt ít nhất cũng lớn như vậy, vì vậy câu trả lời phải ở bên phải. Khi`D(mid) < B`, mọi giá trị bằng hoặc cao hơn`mid`tạo ra sự khác biệt không lớn hơn, vì vậy câu trả lời phải ở bên trái. Vì phép chia hợp lệ được đảm bảo là duy nhất nên tìm kiếm nhị phân không thể loại bỏ nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def bill(energy):
    if energy <= 100:
        return energy * 2

    result = 200
    energy -= 100

    if energy <= 9900:
        return result + energy * 3

    result += 9900 * 3
    energy -= 9900

    if energy <= 990000:
        return result + energy * 5

    result += 990000 * 5
    energy -= 990000

    return result + energy * 7

def consumption_from_bill(amount):
    if amount <= 200:
        return amount // 2

    if amount <= 29900:
        return 100 + (amount - 200) // 3

    if amount <= 4979900:
        return 10000 + (amount - 29900) // 5

    return 1000000 + (amount - 4979900) // 7

def solve():
    while True:
        A, B = map(int, input().split())

        if A == 0 and B == 0:
            break

        total = consumption_from_bill(A)

        lo = 1
        hi = total // 2

        while lo <= hi:
            mid = (lo + hi) // 2

            smaller_bill = bill(mid)
            larger_bill = bill(total - mid)
            diff = larger_bill - smaller_bill

            if diff == B:
                print(smaller_bill)
                break

            if diff > B:
                lo = mid + 1
            else:
                hi = mid - 1

if __name__ == "__main__":
    solve()
```các`bill`chức năng thực hiện biểu giá lũy tiến sử dụng chi phí tích lũy. Sau 100 chiếc đầu tiên, 200 chiếc Americus đầu tiên đã được tính phí. Sau 9.900 đơn vị tiếp theo, chi phí tích lũy là`200 + 9900 * 3 = 29,900`. Sau 990.000 đơn vị nữa, nó trở thành`29,900 + 990000 * 5 = 4,979,900`. Các hằng số này cho phép mỗi phép tính hóa đơn chạy trong thời gian không đổi. 

các`consumption_from_bill`chức năng đảo ngược những ranh giới tương tự. Ví dụ, nếu`amount`nằm trong khoảng từ 29.901 đến 4.979.900, 10.000 đơn vị đầu tiên đã chiếm 29.900 Americus nên số tiền còn lại chia cho 5 và cộng thành 10.000. 

Tìm kiếm nhị phân có chủ ý sử dụng mức tiêu dùng hơn là tiền. Biểu giá là phi tuyến tính, vì vậy việc cố gắng tìm kiếm nhị phân trực tiếp trên chênh lệch hóa đơn mà không khôi phục tổng mức tiêu thụ trước tiên sẽ khiến mối quan hệ khó thể hiện hơn. Một lần`total`đã biết, hai mức tiêu dùng luôn cộng vào giá trị cố định đó. 

Việc tìm kiếm sử dụng`bill(total - mid) - bill(mid)`còn hơn là`abs(...)`. Bởi vì`mid <= total // 2`, hộ gia đình thứ hai tiêu dùng ít nhất bằng hộ gia đình thứ nhất và hàm hóa đơn ngày càng tăng. Do đó, hóa đơn lớn hơn được biết trước. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Dù sao thì giá trị trung gian lớn nhất cũng nằm trong biểu diễn số nguyên của Python. Việc sử dụng`sys.stdin.readline`cũng tránh được chi phí đầu vào không cần thiết khi có nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
1100 300
```Hóa đơn tổng hợp 1100 tương ứng với 400 đơn vị vì`bill(400) = 1100`. Mức tiêu thụ nhỏ hơn phải nằm trong khoảng từ 1 đến 200. 

|`lo`|`hi`|`mid`|`bill(mid)`|`bill(400-mid)`|`diff`| Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 200 | 100 | 200 | 700 | 500 |`diff > 300`, tăng`lo`| 
| 101 | 200 | 150 | 350 | 650 | 300 | Tìm thấy | 

Điểm giữa đầu tiên cho chênh lệch 500, quá lớn. Do đó, hộ gia đình nhỏ hơn cần tiêu dùng nhiều hơn. Điểm giữa tiếp theo chính xác là 150, cho ra các hóa đơn 350 và 650, chênh lệch của chúng là 300. Đầu ra yêu cầu là`350`. Mẫu và phần chia 150/250 cơ bản của nó được ghi lại trong nguồn vấn đề ban đầu. 

### Mẫu 2 

Đầu vào là```
35515 27615
```Hóa đơn kết hợp tương ứng với 11.123 đơn vị. Sự phân chia hợp lệ là 1.000 và 10.123, tạo ra các hóa đơn 2.900 và 30.515. 

|`lo`|`hi`|`mid`|`bill(mid)`|`bill(total-mid)`|`diff`| Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5561 | 2781 | 8243 | 24926 | 16683 |`diff < B`, giảm bớt`hi`| 
| 1 | 2780 | 1390 | 4070 | 29099 | 25029 |`diff < B`, giảm bớt`hi`| 
| 1 | 1389 | 695 | 1985 | 32040 | 30055 |`diff > B`, tăng`lo`| 
| 696 | 1389 | 1042 | 3026 | 30305 | 27279 |`diff < B`, giảm bớt`hi`| 
| 696 | 1041 | 868 | 2504 | 31175 | 28671 |`diff > B`, tăng`lo`| 
| 869 | 1041 | 975 | 2825 | 30640 | 27815 |`diff > B`, tăng`lo`| 
| 976 | 1041 | 1008 | 2924 | 30495 | 27571 |`diff < B`, giảm bớt`hi`| 
| 976 | 1007 | 991 | 2873 | 30565 | 27692 |`diff > B`, tăng`lo`| 
| 992 | 1007 | 999 | 2897 | 30520 | 27623 |`diff > B`, tăng`lo`| 
| 1000 | 1007 | 1003 | 2909 | 30500 | 27591 |`diff < B`, giảm bớt`hi`| 
| 1000 | 1002 | 1001 | 2903 | 30510 | 27607 |`diff < B`, giảm bớt`hi`| 
| 1000 | 1000 | 1000 | 2900 | 30515 | 27615 | Tìm thấy | 

Việc tìm kiếm liên tục thu hẹp khoảng thời gian trong khi vẫn duy trì mức tiêu thụ hợp lệ duy nhất. Một lần`mid`đạt 1.000 thì chênh lệch chính xác là 27.615 nên đáp án là`2,900`. Đầu ra mẫu chính thức là`350`Và`2900`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log C) | Mỗi trường hợp thử nghiệm thực hiện tính toán hóa đơn theo thời gian không đổi và giảm một nửa khoảng thời gian tìm kiếm tiêu thụ mỗi lần lặp. | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên cố định được sử dụng. | 

Vì`A <= 10^9`, mức tiêu thụ kết hợp nhiều nhất là khoảng 143 triệu đơn vị, do đó tìm kiếm nhị phân cần ít hơn 28 lần lặp. Mỗi lần lặp chỉ thực hiện một số phép tính số học số nguyên. Điều này thoải mái trong giới hạn 1 giây và 256 MB được nêu cho sự cố. 

## Trường hợp thử nghiệm```python
import sys
import io

def bill(energy):
    if energy <= 100:
        return energy * 2
    if energy <= 10000:
        return 200 + (energy - 100) * 3
    if energy <= 1000000:
        return 29900 + (energy - 10000) * 5
    return 4979900 + (energy - 1000000) * 7

def consumption_from_bill(amount):
    if amount <= 200:
        return amount // 2
    if amount <= 29900:
        return 100 + (amount - 200) // 3
    if amount <= 4979900:
        return 10000 + (amount - 29900) // 5
    return 1000000 + (amount - 4979900) // 7

def solve_data(data: str) -> str:
    inp = io.StringIO(data)
    out = []

    while True:
        line = inp.readline()
        if not line:
            break

        A, B = map(int, line.split())

        if A == 0 and B == 0:
            break

        total = consumption_from_bill(A)

        lo = 1
        hi = total // 2

        while lo <= hi:
            mid = (lo + hi) // 2

            small = bill(mid)
            large = bill(total - mid)
            diff = large - small

            if diff == B:
                out.append(str(small))
                break

            if diff > B:
                lo = mid + 1
            else:
                hi = mid - 1

    return "\n".join(out)

# Provided samples
assert solve_data(
    "1100 300\n35515 27615\n0 0\n"
) == "350\n2900", "provided samples"

# Minimum valid non-terminating case:
# consumptions 1 and 2 -> bills 2 and 4.
assert solve_data(
    "6 2\n0 0\n"
) == "2", "minimum valid case"

# Both households are inside the first tariff range:
# consumptions 49 and 51 -> bills 98 and 102.
assert solve_data(
    "200 4\n0 0\n"
) == "98", "first tariff boundary"

# Crosses the 1,000,000-unit tariff boundary:
# consumptions 1 and 1000010.
assert solve_data(
    "4979977 4979968\n0 0\n"
) == "2", "highest tariff boundary"

# Maximum-size valid A close to 1e9:
# consumptions 1 and 143145727.
assert solve_data(
    "999999996 999999987\n0 0\n"
) == "2", "large input"

# The all-equal case is represented by the required sentinel 0 0.
assert solve_data(
    "0 0\n"
) == "", "termination sentinel"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 2`|`2`| Đầu vào hợp lệ tối thiểu và mức tiêu thụ dương nhỏ nhất có thể | 
|`200 4`|`98`| Ranh giới thuế quan 100 đơn vị chính xác và thuế suất trên mỗi đơn vị bằng nhau ở cả hai bên | 
|`4979977 4979968`|`2`| Chuyển sang mức thuế cao nhất | 
|`999999996 999999987`|`2`| Giá trị lớn gần`10^9`giới hạn đầu vào | 
|`0 0`| Không có đầu ra | Xử lý trọng điểm | 

## Vỏ cạnh 

Đối với đầu vào hợp lệ tối thiểu`6 2`, biểu giá nghịch đảo chuyển đổi 6 chiếc Americus thành tổng cộng 3 đơn vị. Tìm kiếm nhị phân bắt đầu bằng`lo = 1`Và`hi = 1`, Vì thế`mid = 1`. Hai mức tiêu thụ là 1 và 2, hóa đơn của họ là 2 và 4, và chênh lệch chính xác là 2. Thuật toán in ngay lập tức 2. Không có giả định rằng một trong hai hộ gia đình tiêu thụ đủ điện để đạt đến phạm vi biểu giá thứ hai. 

Đối với ranh giới thuế quan đầu tiên, hãy xem xét`200 4`. Hàm nghịch đảo trả về 100 đơn vị vì 100 đơn vị đầu tiên có giá chính xác là 200. Tìm kiếm nhị phân kiểm tra mức tiêu thụ nhỏ hơn duy nhất có thể có, 50, nếu khoảng được biểu thị dưới dạng`1..50`, và cuối cùng tìm thấy phần tách duy nhất 49 và 51. Hóa đơn của chúng là 98 và 102, cho chênh lệch bắt buộc là 4. Phép tính coi đơn vị 100 là một phần của phạm vi đầu tiên và đơn vị 101 là đơn vị đầu tiên được tính phí ở mức 3. 

Đối với trường hợp vượt qua ranh giới cao nhất, hãy xem xét`4979977 4979968`. Hóa đơn kết hợp quy đổi thành 1.000.011 đơn vị. Sự phân chia hợp lệ là 1 và 1.000.010. Hộ thứ nhất trả 2. Hộ thứ hai trả`4,979,900 + 10 * 7 = 4,979,970`, vậy chênh lệch là 4.979.968. Do đó, việc tìm kiếm sẽ tìm ra mức tiêu dùng nhỏ hơn là 1 và đầu ra là 2. Việc tìm kiếm này thực hiện cả nhánh cuối cùng của biểu giá nghịch đảo và nhánh cuối cùng của biểu giá kỳ hạn. 

Đối với trường hợp có giới hạn lớn`999999996 999999987`, tổng mức tiêu thụ là 143.145.728. Sự phân chia hợp lệ là 1 và 143.145.727. Tờ tiền nhỏ hơn là 2, trong khi tờ tiền lớn hơn là 999.999.989, chênh lệch là 999.999.987. Mặc dù tổng mức tiêu thụ là hơn 143 triệu đơn vị, tìm kiếm nhị phân chỉ cần nhiều lần lặp theo logarit thay vì quét từng phần tách có thể có đó. 

các`0 0`đầu vào được xử lý trước khi chuyển đổi thuế quan hoặc tìm kiếm nhị phân. Nó chấm dứt vòng lặp ngay lập tức, do đó không thể vô tình hiểu nó là một cặp hộ gia đình không tiêu dùng. Sự khác biệt này quan trọng vì các trường hợp thử nghiệm thông thường có kết quả tích cực.`A`Và`B`, trong khi`0 0`chỉ được dành riêng làm điểm đánh dấu kết thúc.
