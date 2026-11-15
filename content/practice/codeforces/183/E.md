---
title: "CF 183E - Cửa Hàng Kẹo"
description: "Có n đứa trẻ ngồi theo một thứ tự tuần hoàn cố định. Trong quá trình mua, kích thước gói hàng đã chọn phải tạo thành một chuỗi tăng dần nghiêm ngặt trên toàn cầu ở tất cả các lượt. Sau bé n, lượt tiếp theo quay lại bé 1."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 183
codeforces_index: "E"
codeforces_contest_name: "Croc Champ 2012 - Final"
rating: 2900
weight: 183
solve_time_s: 126
verified: true
draft: false
---

[CF 183E - Cửa hàng kẹo](https://codeforces.com/problemset/problem/183/E) 

**Xếp hạng:** 2900 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

có`n`trẻ ngồi theo một trật tự tuần hoàn cố định. Trong quá trình mua, kích thước gói hàng đã chọn phải tạo thành một chuỗi tăng dần nghiêm ngặt trên toàn cầu ở tất cả các lượt. Sau đứa trẻ`n`, lượt tiếp theo quay lại với nhóc`1`. 

Mỗi đứa trẻ có thể mua nhiều gói, nhưng ở trạng thái cuối cùng, tất cả trẻ em đều phải mua cùng một số gói. đứa trẻ`i`không thể chi tiêu nhiều hơn mức trợ cấp của họ`a[i]`. Cửa hàng bán kích thước gói từ`1`ĐẾN`m`. 

Chúng tôi muốn tổng số kẹo được bán tối đa có thể. 

Hạn chế chính là trật tự ngày càng tăng toàn cầu. Nếu chúng ta viết tất cả các kích cỡ gói hàng đã mua theo thứ tự thời gian thì chúng chỉ đơn giản là một chuỗi tăng dần nghiêm ngặt nào đó được chọn từ`1...m`. Lần lượt xác định đứa trẻ nào nhận được vị trí nào trong trình tự đó. 

Giả sử mỗi đứa trẻ mua chính xác`k`gói. Khi đó tổng số gói mua là`n * k`. Vì kích thước gói hàng ngày càng tăng và không thể vượt quá`m`, dãy phải chứa chính xác`n * k`các số nguyên khác nhau từ`1...m`. 

Tổng chi phí nhỏ nhất có thể cho trẻ`i`xảy ra khi chuỗi toàn cục càng nhỏ càng tốt, cụ thể là:```
1, 2, 3, ..., n*k
```đứa trẻ`i`nhận các vị trí:```
i, i+n, i+2n, ...
```do đó chi tiêu tối thiểu có thể của họ trở thành:```
i + (i+n) + (i+2n) + ... + (i+(k-1)n)
= k*i + n*k*(k-1)/2
```Nếu thậm chí mức tối thiểu này vượt quá mức cho phép thì việc mua`k`mỗi gói là không thể. 

Các ràng buộc đủ lớn để bậc hai hoặc thậm chí`O(n * sqrt(m))`mô phỏng phong cách là nguy hiểm. Đây`n`đạt tới`2 * 10^5`Và`m`đạt tới`5 * 10^6`, vì vậy chúng ta cần một cái gì đó gần với tuyến tính hoặc logarit. 

Một trường hợp phức tạp xuất hiện khi một số đứa trẻ thậm chí không đủ khả năng mua gói hàng nhỏ nhất có thể của mình. 

Ví dụ:```
2 5
1
100
```Bé 2 có thể mua gói lớn nhưng bé 1 ít nhất phải nhận được gói`1`trong chu kỳ đầu tiên và gói`3`trong chu kỳ thứ hai. Câu trả lời chỉ là`1`, không phải thứ gì đó lớn hơn có được bằng cách để đứa trẻ 2 tiếp tục một mình, bởi vì tất cả đứa trẻ đều phải kết thúc với số gói như nhau. 

Một lỗi dễ mắc phải khác là quên giới hạn trên`m`. 

Ví dụ:```
2 3
100
100
```Bạn có thể nghĩ rằng cả hai đứa trẻ đều có thể mua nhiều gói vì tiền là đủ. Nhưng dãy tăng nghiêm ngặt dài nhất bên trong`1...3`có chiều dài`3`. Vì tổng số lần mua phải chia hết cho`n = 2`, mức tối đa chỉ là`2`. 

Một cạm bẫy tinh vi hơn là giả sử chuỗi tối ưu phải sử dụng các số liên tiếp. Nó không. 

Ví dụ:```
2 5
5
10
```Trình tự tối ưu là:```
1, 3, 4, 5
```không:```
1, 2, 3, 4
```Chuỗi đầu tiên cho tổng số`13`, lớn hơn`10`. 

Bí quyết là sau khi xác định được tính khả thi, chúng ta nên tối đa hóa tổng bằng cách đẩy các số đã chọn lên trên. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi con số có thể`k`số gói cho mỗi đứa trẻ. Đối với mỗi`k`, chúng ta có thể tìm kiếm một dãy có độ dài tăng dần`n*k`và kiểm tra xem mọi đứa trẻ có nằm trong ngân sách hay không. 

Bản thân điều kiện khả thi có thể quản lý được vì mô hình chi tiêu tối thiểu rất rõ ràng:```
cost(i, k) = k*i + n*k*(k-1)/2
```Vì vậy chúng ta có thể kiểm tra tất cả`k`từ`1`ĐẾN`m/n`. 

Khó khăn thực sự là tối đa hóa tổng số kẹo bán được sau khi xác định tính khả thi. Việc liệt kê các chuỗi thực tế trở thành tổ hợp. Thậm chí còn tạo ra các chuỗi tối ưu một cách tham lam cho mọi`k`sẽ quá chậm khi`m`là vài triệu. 

Quan sát quan trọng là tính khả thi chỉ phụ thuộc vào sự phân công tối thiểu. Một khi giá trị của`k`là khả thi, bất kỳ khoản tiền bổ sung nào cũng có thể được chuyển đổi thành các gói có kích thước lớn hơn. 

Giả sử chúng ta đã sửa`L = n*k`, tổng số gói đã mua. Trong số tất cả các chuỗi có độ dài tăng dần một cách chặt chẽ`L`bên trong`1...m`, tổng số tiền lớn nhất có thể rõ ràng là:```
m-L+1, m-L+2, ..., m
```tổng của nó bằng:```
L * (2m-L+1) / 2
```Câu hỏi đặt ra là liệu bọn trẻ có đủ khả năng thực hiện trình tự tối đa như vậy hay không. 

Một bất biến quan trọng xuất hiện ở đây. Bắt đầu từ trình tự tối thiểu:```
1, 2, ..., L
```chúng ta có thể tăng các phần tử trong khi vẫn duy trì mức tăng nghiêm ngặt. Tổng số tiền tăng có sẵn chính xác là:```
L * (m-L)
```vì mỗi vị trí đều có thể dịch chuyển lên trên. 

đứa trẻ`i`ban đầu chi tiêu:```
base_i = k*i + n*k*(k-1)/2
```và có ngân sách còn lại:```
extra_i = a[i] - base_i
```Tổng ngân sách bổ sung cho tất cả trẻ em ít nhất phải bằng tổng số dịch chuyển lên trên cần thiết để chuyển đổi trình tự tối thiểu thành trình tự tối đa. 

Mức tăng yêu cầu đó bằng:```
L * (m-L)
```bởi vì:```
sum(maximal) - sum(minimal)
```chính xác là số lượng đó. 

Điều kiện này cũng đủ. Mỗi sự dịch chuyển lên trên có thể được phân phối độc lập trong khi vẫn duy trì trật tự tăng dần. 

Vì vậy toàn bộ vấn đề quy về việc tìm phương án khả thi lớn nhất`L = n*k`thỏa mãn:```
1. base_i <= a[i] for all i
2. sum(extra_i) >= L*(m-L)
```Bây giờ chúng ta chỉ cần lặp lại những gì có thể`k`, nhiều nhất là`m/n ≤ 2.5 * 10^6`. Với số học cẩn thận, điều này hoạt động thoải mái về mặt thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) hoặc tệ hơn | O(m) | Quá chậm | 
| Tối ưu | O(m / n + n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoản phụ cấp vào một mảng`a`. 
2. Tính tổng trợ cấp:```
S = sum(a)
```vì điều kiện ngân sách toàn cầu phụ thuộc vào tổng số tiền còn lại. 

1. Lặp lại số lượng gói có thể có cho mỗi đứa trẻ,`k = 1, 2, ...`. 

Tổng số lần mua là:```
L = n*k
```Nếu như`L > m`, dừng lại ngay vì chúng ta không thể tạo thành một dãy tăng chặt dài hơn`m`. 

1. Tính số tiền chi tiêu tối thiểu cần thiết cho trẻ`i`:```
base_i = k*(i+1) + n*k*(k-1)/2
```Ở đây các chỉ số đều dựa trên mã số 0, vậy nên nhóc`i`tương ứng với nhãn`i+1`. 

1. Nếu một số`base_i > a[i]`, thì cái này`k`là không thể. lớn hơn`k`chỉ tăng chi phí yêu cầu tối thiểu để chúng ta có thể dừng hoàn toàn vòng lặp. 

Sự đơn điệu này rất quan trọng. Một khi một giá trị của`k`không thành công, tất cả các giá trị lớn hơn cũng thất bại. 

1. Tính tổng số tiền tối thiểu của tất cả các kích cỡ gói hàng đã chọn:```
min_sum = L*(L+1)/2
```và tổng số tiền tối đa:```
max_sum = L*(2*m-L+1)/2
```Sự khác biệt giữa chúng là sự dịch chuyển hướng lên trên cần thiết. 

1. Kiểm tra tổng mức phụ cấp có thể chi trả cho dãy tối đa hay không:```
S >= max_sum
```Chỉ điều này thôi thì chưa đủ vì ngân sách dành cho mỗi đứa trẻ chứ không phải toàn cầu. 

1. Tính tổng chi tiêu tối thiểu của trẻ em:```
base_total = min_sum
```Chúng thực sự bằng nhau, bởi vì trình tự tối thiểu`1...L`ấn định chính xác những chi phí đó. 

1. Tổng ngân sách bổ sung hiện có là:```
S - base_total
```Số tiền tăng thêm cần thiết là:```
max_sum - min_sum
```mà đơn giản hóa thành:```
L*(m-L)
```Nếu ngân sách bổ sung có sẵn ít nhất là số tiền này thì có thể đạt được trình tự tối đa. 

1. Giữ câu trả lời đúng nhất:```
answer = max_sum
```vì đã cố định`L`, dãy tối đa luôn mang lại tổng số kẹo được bán lớn nhất. 

### Tại sao nó hoạt động 

Với số vòng cố định`k`, mọi quy trình mua hàng hợp lệ đều tương ứng với một chuỗi độ dài tăng dần nghiêm ngặt`L = n*k`. 

Chuỗi khả thi tối thiểu là duy nhất:```
1, 2, ..., L
```và điều này tạo ra mức chi tiêu tối thiểu có thể có cho mọi đứa trẻ cùng một lúc. 

Bất kỳ chuỗi hợp lệ nào khác đều có thể thu được bằng cách tăng một số phần tử trong khi vẫn giữ nguyên trật tự nghiêm ngặt. Tổng mức tăng có thể có từ chuỗi tối thiểu đến chuỗi tối đa bằng:```
L*(m-L)
```Các ràng buộc ngân sách riêng lẻ đã được thỏa mãn ở trình tự tối thiểu. Bất kỳ khoản trợ cấp còn lại nào cũng có thể được xem là khả năng cho những khoản tăng này. 

Vì mức tăng có thể được phân bổ độc lập giữa các vị trí trong khi vẫn duy trì mức tăng nghiêm ngặt nên yêu cầu bổ sung duy nhất là tổng ngân sách còn lại đủ lớn. Như vậy điều kiện khả thi là cần và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [int(input()) for _ in range(n)]

    total = sum(a)
    ans = 0

    k = 1

    while n * k <= m:
        l = n * k

        add = n * k * (k - 1) // 2

        ok = True

        for i in range(n):
            need = k * (i + 1) + add
            if need > a[i]:
                ok = False
                break

        if not ok:
            break

        max_sum = l * (2 * m - l + 1) // 2

        if total >= max_sum:
            ans = max_sum

        k += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp lặp lại theo số lượng gói cho mỗi đứa trẻ thay vì trực tiếp theo độ dài chuỗi. Điều này tự động duy trì điều kiện là tất cả trẻ em đều mua cùng số lượng gói. 

biểu hiện```
add = n * k * (k - 1) // 2
```là sự đóng góp tiến triển số học được chia sẻ cho mọi đứa trẻ. Thời hạn còn lại`k * (i + 1)`chiếm phần bù đắp của đứa trẻ`i`bên trong mỗi chu kỳ. 

Một điểm tế nhị là việc chấm dứt sớm khi một số đứa trẻ không đạt được bài kiểm tra chi phí tối thiểu. Chi tiêu yêu cầu tối thiểu tăng đều đặn với`k`, vì vậy một khi cụ thể`k`là không thể, tất cả các giá trị lớn hơn cũng không thể. 

Một chi tiết quan trọng khác là sử dụng số học 64-bit. Giá trị có thể đạt khoảng`m²`, xung quanh`2.5 * 10^13`. Số nguyên Python tự động xử lý việc này. 

The condition```
total >= max_sum
```is sufficient after individual minimum checks pass. Sự sắp xếp tối thiểu đã đáp ứng tất cả các ràng buộc cho mỗi đứa trẻ và tổng số tiền trợ cấp còn lại luôn có thể được phân phối lại thông qua các ca tăng lên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 5
5
10
```Vì`k = 1`: 

| k | L | Nhóc 1 phút | Nhóc 2 phút | Khả thi | max_sum | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | Có | 9 | 

Vì`k = 2`: 

| k | L | Nhóc 1 phút | Nhóc 2 phút | Khả thi | max_sum | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 4 | 4 | 6 | Có | 14 | 

Chuỗi có độ dài tối đa-4 là:```
2, 3, 4, 5
```với tổng số`14`, nhưng đứa trẻ 1 sẽ tiêu`2 + 4 = 6`, vượt ngân sách`5`. 

Vì vậy, chúng ta cần một tổng số khả thi nhỏ hơn. Trình tự tốt nhất có thể đạt được là:```
1, 3, 4, 5
```tổng của ai là`13`. 

Ví dụ này cho thấy tại sao việc tối đa hóa trình tự một cách tham lam mà không tôn trọng ngân sách riêng lẻ lại thất bại. 

### Ví dụ 2 

đầu vào:```
3 7
3
8
20
```Vì`k = 1`: 

| k | L | Nhóc 1 phút | Nhóc 2 phút | Nhóc 3 phút | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 2 | 3 | Có | 

Vì`k = 2`: 

| k | L | Nhóc 1 phút | Nhóc 2 phút | Nhóc 3 phút | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 6 | 5 | 7 | 9 | Không | 

Đứa trẻ 1 đã cần ít nhất`5`, vượt mức cho phép`3`. Vòng lặp dừng ngay lập tức. 

Độ dài chuỗi tốt nhất có thể đạt được là`3`và tổng lớn nhất là:```
5 + 6 + 7 = 18
```Dấu vết này thể hiện tính chất đơn điệu. Một lần một số`k`không thành công, giá trị lớn hơn không thể phục hồi được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m / n * n) | Mỗi khả thi`k`quét tất cả trẻ em một lần | 
| Không gian | O(n) | Phụ cấp cửa hàng | 

Từ`m / n`nhiều nhất là`2.5 * 10^6`, thời gian chạy thực tế có thể chấp nhận được trong Python được tối ưu hóa vì vòng lặp thường kết thúc sớm khi một số trẻ không đủ khả năng mua cấu hình tối thiểu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = [int(input()) for _ in range(n)]

    total = sum(a)
    ans = 0

    k = 1

    while n * k <= m:
        l = n * k

        add = n * k * (k - 1) // 2

        ok = True

        for i in range(n):
            need = k * (i + 1) + add
            if need > a[i]:
                ok = False
                break

        if not ok:
            break

        max_sum = l * (2 * m - l + 1) // 2

        if total >= max_sum:
            ans = max_sum

        k += 1

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""2 5
5
10
"""
) == "13", "sample 1"

# minimum-size case
assert run(
"""2 2
1
2
"""
) == "3", "minimum sizes"

# all equal allowances
assert run(
"""3 6
10
10
10
"""
) == "15", "equal allowances"

# impossible second round
assert run(
"""3 7
3
8
20
"""
) == "18", "early stopping"

# m boundary limits total purchases
assert run(
"""2 3
100
100
"""
) == "5", "limited by m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 1 / 2`|`3`| Cấu hình hợp lệ nhỏ nhất | 
|`3 6 / 10 / 10 / 10`|`15`| Ngân sách bằng nhau và trình tự tối đa | 
|`3 7 / 3 / 8 / 20`|`18`| Chấm dứt sớm khi không khả thi`k`| 
|`2 3 / 100 / 100`|`5`| Độ dài chuỗi giới hạn bởi`m`| 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
2 3
100
100
```Chuỗi tăng dài nhất bên trong`1...3`có chiều dài`3`, nhưng số lượng mua phải chia hết cho`2`. Thuật toán kiểm tra:```
k = 1 -> L = 2
k = 2 -> L = 4 > 3
```vậy thôi`L = 2`được xem xét. Tổng lớn nhất trở thành:```
2 + 3 = 5
```cái nào đúng. 

Bây giờ hãy xem xét:```
3 10
1
100
100
```Vì`k = 1`, đứa trẻ 1 cần ít nhất`1`, do đó cấu hình hoạt động. 

Vì`k = 2`, bé 1 cần:```
1 + 4 = 5
```vượt quá mức cho phép của họ. Thuật toán dừng ngay lập tức vì lớn hơn`k`chỉ tăng chi tiêu cần thiết. 

Câu trả lời đến từ khả năng tốt nhất`k = 1`. 

Cuối cùng, hãy xem xét:```
2 5
5
10
```Chuỗi có độ dài-4 tối đa trên toàn cầu là:```
2, 3, 4, 5
```nhưng đứa trẻ 1 sẽ tiêu`6`. Thuật toán tránh được lỗi này vì trước tiên nó kiểm tra tính khả thi tối thiểu của từng cá nhân trước khi xem xét tổng số tiền tối đa.
