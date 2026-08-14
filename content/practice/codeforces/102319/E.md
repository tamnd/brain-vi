---
title: "CF 102319E - Những chiếc đèn lồng bí ẩn của Enegue"
description: "Chúng ta có một hàng n chiếc đèn lồng, có chính xác k chiếc đang bật và 4 <= k <= n <= 100. Chúng ta có thể hỏi giám khảo về bất kỳ tập hợp con đèn lồng nào. Nếu tập hợp con đó chứa x đèn lồng thì giám khảo không tiết lộ x. Thay vào đó, nó trả về số ước của x là hợp số."
date: "2026-08-14T00:35:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102319
codeforces_index: "E"
codeforces_contest_name: "UBC Summer Contest 2018"
rating: 0
weight: 102319
solve_time_s: 767
verified: true
draft: false
---

[CF 102319E - Những chiếc đèn lồng bí ẩn của Enegue](https://codeforces.com/problemset/problem/102319/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12 phút 47 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng`n`chính xác là đèn lồng`k`trong số đó đang bật và`4 <= k <= n <= 100`. Chúng ta có thể hỏi thẩm phán về bất kỳ tập hợp con đèn lồng nào. Nếu tập hợp con đó chứa`x`thắp đèn lồng, thẩm phán không tiết lộ`x`. Thay vào đó, nó trả về số ước của`x`đó là sự tổng hợp. 

Cho phép`F(x)`biểu thị phản ứng đó. Ví dụ,`F(4) = 1`, bởi vì ước số tổng hợp duy nhất của`4`là`4`chính nó. Cũng,`F(3) = 0`, bởi vì`3`là nguyên tố. 

Nhiệm vụ là xác định chính xác`k`đèn lồng đang bật, sử dụng nhiều nhất`2 * 10^5`truy vấn. 

Giá trị nhỏ của`n`là lừa dối. có thể có`C(100, 4) = 3,921,225`các tập hợp con bốn đèn lồng khác nhau, vì vậy việc kiểm tra từng tập hợp con bốn đèn lồng đã quá tốn kém. Đồng thời,`n <= 100`có nghĩa là số lượng truy vấn bậc hai hoặc bậc ba có thể nằm trong giới hạn, vì`C(100, 3) = 161,700`. 

Khó khăn chính đó là`F(x)`không phải là một-một. Ví dụ,`F(25) = F(26) = 1`, bởi vì`25`có ước số tổng hợp`{25}`Và`26`có ước số tổng hợp`{26}`. Một chiến lược giả định câu trả lời xác định duy nhất số lượng đèn lồng được thắp sáng sẽ âm thầm thất bại. 

Ngoài ra còn có một vấn đề ranh giới xung quanh giá trị`4`. chúng tôi có`F(3) = 0`Nhưng`F(4) = 1`, điều này làm cho bốn đặc biệt hữu ích. Ví dụ, nếu`n = 4, k = 4`, truy vấn cả bốn chiếc đèn lồng đều cho`1`, trong khi bất kỳ tập hợp con nào chứa tối đa ba chiếc đèn lồng được thắp sáng đều cho`0`. Một phương pháp dựa trên tìm kiếm nhị phân thông thường không thể đơn giản coi phản hồi của thẩm phán là số ẩn. 

Mẫu được cung cấp có tính tương tác, do đó các câu trả lời được hiển thị của nó không tạo thành trường hợp kiểm tra đầu vào/đầu ra thông thường. Mẫu của`0`phản hồi là phản hồi từ bản ghi tương tác ban đầu chứ không phải dữ liệu mà chương trình hàng loạt có thể sao chép độc lập. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là truy vấn mọi tập hợp con bốn phần tử. Một tập hợp con bốn phần tử chứa từ 0 đến 4 đèn lồng đang sáng. Trong số những khả năng này, chỉ`x = 4`có một câu trả lời khác không, vì`0`,`1`,`2`, Và`3`không có ước số tổng hợp liên quan đến sự tương tác này. Do đó, truy vấn về bốn chiếc đèn lồng trả về`1`chính xác khi cả bốn chiếc đèn lồng đó đều bật sáng. 

Khi mỗi tập hợp con bốn đã được kiểm tra, đèn lồng sẽ bật chính xác khi nó thuộc về ít nhất một tập hợp con bốn dương. Điều này đúng vì`k >= 4`, vì vậy mỗi chiếc đèn lồng đã thắp sáng có thể được ghép nối với ba chiếc đèn lồng đang thắp sáng khác. 

Vấn đề là số lượng truy vấn. Trong trường hợp xấu nhất điều này thực hiện`C(100, 4) = 3,921,225`các truy vấn vượt xa giới hạn của`200,000`. 

Quan sát quan trọng là chúng ta thực sự không cần truy vấn các tập hợp con chứa chính xác bốn đèn lồng. Thay vào đó, chúng ta có thể loại bỏ một bộ đèn lồng nhỏ khỏi toàn bộ hàng. 

Giả sử chúng ta chọn một bộ`T`của`t`đèn lồng và truy vấn mọi đèn lồng ngoại trừ những chiếc đèn lồng trong`T`. Nếu như`r`đèn lồng bên trong`T`được bật, tập truy vấn chứa chính xác`k - r`thắp đèn lồng. Câu trả lời là do đó`F(k-r)`. 

Bây giờ chọn số dương nhỏ nhất`t`như vậy`F(k-t) != F(k)`. 

Đối với những hạn chế của vấn đề này, kiểm tra`t = 1, 2, 3`là đủ cho mọi`k`từ`4`bởi vì`100`. Đây là thuộc tính hữu hạn trong phạm vi cho phép nên có thể kiểm tra trong khi tính toán trước`F`. Lần chạy bằng nhau dài nhất có liên quan ở đây có độ dài bằng ba. 

Sự lựa chọn _nhỏ nhất_ như vậy`t`là điều làm cho truy vấn trở nên hữu ích. Đối với mọi`r < t`, sự tối thiểu mang lại`F(k-r) = F(k)`. 

Vì`r = t`, theo định nghĩa,`F(k-t) != F(k)`. 

Phần bổ sung được truy vấn của một`t`- tập hợp phần tử có thể chứa tối đa`t`loại trừ đèn lồng thắp sáng. Vì thế phản ứng của nó khác với`F(k)`chính xác khi nào`r = t`, có nghĩa là chính xác khi mỗi chiếc đèn lồng trong`T`đang bật. 

Chúng tôi đã chuyển đổi phản hồi số chia khó hiểu thành một phản hồi rõ ràng`t`-cách VÀ kiểm tra. Từ`t <= 3`, chúng tôi có thể kiểm tra mọi thứ có thể`t`-bộ phần tử. Trường hợp xấu nhất là tất cả các tập con gồm 3 phần tử, chỉ`161,700`truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^4)`truy vấn |`O(n)`| Quá chậm | 
| Tối ưu |`O(n^3)`truy vấn |`O(n)`| Đã chấp nhận | 

Công việc tính toán thực tế để xây dựng mỗi truy vấn là`O(n)`, vậy tổng khối lượng sản phẩm xây dựng là`O(n^4)`các thao tác ký tự trong trường hợp xấu nhất, nhưng số lượng truy vấn đánh giá, vốn là ràng buộc ràng buộc ở đây, nhiều nhất là`C(100,3) = 161,700`. 

## Hướng dẫn thuật toán 

1. Tính toán trước`F(x)`, số các ước số tổng hợp của mọi`x`từ`1`bởi vì`100`. Nếu như`x = p_1^{a_1} ... p_m^{a_m}`, tổng số ước của nó là`(a_1 + 1) ... (a_m + 1)`. Trong số đó, một người là`1`và chính xác`m`là số nguyên tố nên`F(x) = tau(x) - m - 1`. 
2. Đọc`n`Và`k`, sau đó tìm số nhỏ nhất`t`TRONG`{1, 2, 3}`vì cái gì`F(k-t)`khác với`F(k)`. Ràng buộc`k >= 4`đảm bảo rằng tất cả các chỉ số này đều tích cực. 
3. Xét một tập ứng cử viên`T`chính xác`t`đèn lồng. Hỏi về phần bù của`T`, nghĩa là mọi chiếc đèn lồng đều được bao gồm ngoại trừ những chiếc trong`T`. 
4. Hãy để`r`là số lượng đèn lồng thắp sáng trong`T`. Phần bổ sung được truy vấn chứa`k-r`thắp đèn lồng, nên thẩm phán quay lại`F(k-r)`. 
5. Nếu`r < t`, sự lựa chọn tối thiểu của`t`cho`F(k-r) = F(k)`. Nếu như`r = t`, tất cả các đèn lồng của`T`được thắp sáng và phản hồi là`F(k-t)`, khác với`F(k)`. Từ`r`không thể vượt quá`t`, phản ứng khác với`F(k)`chính xác khi mỗi chiếc đèn lồng trong`T`đang bật. 
6. Liệt kê mọi`t`-tập hợp phần tử`T`. Bất cứ khi nào phần bổ sung của nó tạo ra một câu trả lời khác với`F(k)`, đánh dấu từng chiếc đèn lồng trong`T`như được thắp sáng. 
7. Dừng lại ngay khi`k`đèn lồng đã được đánh dấu. Mỗi chiếc đèn lồng được đánh dấu đều được thắp sáng thực sự và bởi vì có chính xác`k`đèn lồng thắp sáng, bộ đánh dấu hoàn chỉnh là câu trả lời. 

### Tại sao nó hoạt động 

Bất biến đó là một`t`-bộ phần tử nhận được phản hồi khác với truy vấn phần bù của nó một cách chính xác khi tất cả`t`đèn lồng đang bật. Đối với một bộ chứa ít hơn`t`đèn lồng thắp sáng, phần bổ sung chứa một trong`k, k-1, ..., k-(t-1)`những chiếc đèn lồng được thắp sáng và tất cả những giá trị đó đều có phản hồi giống như`k`theo định nghĩa nhỏ nhất`t`. Nếu tất cả`t`đèn lồng được thắp sáng, phần bổ sung chứa`k-t`thắp đèn lồng và tạo ra một phản ứng khác. 

Bởi vì`t <= 3`trong khi`k >= 4`, mỗi chiếc đèn lồng được thắp sáng thuộc về ít nhất một`t`- tập hợp phần tử bao gồm toàn bộ những chiếc đèn lồng được thắp sáng. Một tập hợp con như vậy sẽ được phát hiện và sẽ đánh dấu chiếc đèn lồng đó. Ngược lại, không có chiếc đèn lồng không sáng nào có thể thuộc về một tập hợp con được phát hiện, bởi vì một tập hợp con được phát hiện phải bao gồm toàn bộ những chiếc đèn lồng được thắp sáng. Do đó, bộ được đánh dấu chính xác là bộ đèn lồng đang thắp sáng. 

## Giải pháp Python 

Chương trình sau đây là giải pháp tương tác thực tế. Nó đọc đầu tiên`n`Và`k`, in ngay từng truy vấn, đọc câu trả lời của giám khảo và cuối cùng in cấu hình đã khôi phục.```python
import sys
from itertools import combinations

input = sys.stdin.readline

def composite_divisor_count(x):
    if x <= 1:
        return 0

    divisors = 0
    for d in range(2, x + 1):
        if x % d != 0:
            continue

        # d is composite iff it has a divisor other than 1 and itself.
        composite = False
        for q in range(2, int(d ** 0.5) + 1):
            if d % q == 0:
                composite = True
                break

        if composite:
            divisors += 1

    return divisors

def main():
    n, k = map(int, input().split())

    f = [0] * (k + 1)
    for x in range(1, k + 1):
        f[x] = composite_divisor_count(x)

    base = f[k]

    t = -1
    for candidate in range(1, 4):
        if f[k - candidate] != base:
            t = candidate
            break

    # This is guaranteed by the constraints of the problem.
    if t == -1:
        return

    answer = [False] * n
    found = 0

    for excluded in combinations(range(n), t):
        query = ['1'] * n
        for i in excluded:
            query[i] = '0'

        print("? " + ''.join(query), flush=True)

        response = int(input())
        if response == -1:
            return

        if response != base:
            for i in excluded:
                if not answer[i]:
                    answer[i] = True
                    found += 1

            if found == k:
                break

    result = ''.join('1' if x else '0' for x in answer)
    print("! " + result, flush=True)

if __name__ == "__main__":
    main()
```các`composite_divisor_count`hàm chỉ được gọi tối đa cho số`100`, do đó phép chia thử nghiệm đơn giản của nó là quá đủ nhanh. Việc triển khai theo định hướng công thức hơn có thể sử dụng số chia và số thừa số nguyên tố riêng biệt, nhưng phiên bản trực tiếp có nghĩa là`F(x)`rõ ràng. 

Chương trình lưu trữ`F(k)`TRONG`base`. Việc tìm kiếm`t`cố tình bắt đầu lúc`1`, bởi vì sử dụng nhỏ nhất có thể`t`là điều đảm bảo rằng mỗi số lượng nhỏ hơn những chiếc đèn lồng được thắp sáng bị loại trừ sẽ tạo ra câu trả lời giống như bộ đầy đủ. 

Đối với mỗi sự kết hợp, truy vấn được khởi tạo cho tất cả`'1'`ký tự và các ký tự được chọn`t`các vị trí được thay đổi thành`'0'`. Đây là phần bổ sung của tập ứng cử viên, điều này rất cần thiết. Việc truy vấn ứng viên sẽ tự tạo ra`r`còn hơn là`k-r`và đối số tối thiểu sẽ không còn được áp dụng. 

Phản ứng chỉ được so sánh với`base`. Chúng ta không cần biết chính xác số lượng đèn lồng được thắp sáng trong truy vấn. Một phản ứng khác là đủ để kết luận rằng tất cả`t`đèn lồng bị loại trừ đang bật. 

Bộ luật này cũng xử lý các quyết định của thẩm phán`-1`phản hồi ngay lập tức, theo yêu cầu của giao thức tương tác. 

Không có vấn đề tràn số nguyên trong Python. Số lượng kết hợp lớn nhất chỉ là`161,700`, và các chuỗi có độ dài tối đa`100`. 

## Ví dụ đã hoạt động 

Vì vấn đề ban đầu có tính tương tác nên mẫu được cung cấp không thể được truy tìm dưới dạng đầu vào hàng loạt thông thường. Các ví dụ sau sử dụng cấu hình ẩn và mô phỏng câu trả lời của trọng tài. 

### Ví dụ 1 

Hãy xem xét`n = 9`,`k = 5`, có thắp đèn lồng ở các vị trí`1, 3, 5, 7, 9`. 

Vì`k = 5`, chúng tôi có`F(5) = 0`Và`F(4) = 1`, Vì thế`t = 1`. Một bộ loại trừ một phần tử được truy vấn bằng cách hỏi về tất cả các đèn lồng khác. 

| Bước | Đèn lồng bị loại trừ | Bổ sung đèn lồng thắp sáng | Phản hồi | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 1 | Đánh dấu 1 | 
| 2 | 2 | 5 | 0 | Bỏ qua | 
| 3 | 3 | 4 | 1 | Đánh dấu 3 | 
| 4 | 4 | 5 | 0 | Bỏ qua | 
| 5 | 5 | 4 | 1 | Đánh dấu 5 | 
| 6 | 6 | 5 | 0 | Bỏ qua | 
| 7 | 7 | 4 | 1 | Đánh dấu 7 | 
| 8 | 8 | 5 | 0 | Bỏ qua | 
| 9 | 9 | 4 | 1 | Đánh dấu 9 | 

Cấu hình cuối cùng là`101010101`. Ví dụ này minh họa trường hợp đơn giản nhất, trong đó việc loại bỏ một đèn lồng đã làm thay đổi phản hồi của số chia. 

### Ví dụ 2 

Hãy xem xét`n = 8`,`k = 6`, có thắp đèn lồng ở các vị trí`1, 2, 3, 5, 6, 8`. 

Đây`F(6) = 1`Và`F(5) = 0`, vậy một lần nữa`t = 1`. 

| Bước | Đèn lồng bị loại trừ | Bổ sung đèn lồng thắp sáng | Phản hồi | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 0 | Đánh dấu 1 | 
| 2 | 2 | 5 | 0 | Đánh dấu 2 | 
| 3 | 3 | 5 | 0 | Đánh dấu 3 | 
| 4 | 4 | 6 | 1 | Bỏ qua | 
| 5 | 5 | 5 | 0 | Đánh dấu 5 | 
| 6 | 6 | 5 | 0 | Đánh dấu 6 | 
| 7 | 7 | 6 | 1 | Bỏ qua | 
| 8 | 8 | 5 | 0 | Đánh dấu 8 | 

Cấu hình đã phục hồi là`11101101`. Sự khác biệt được đảo ngược so với ví dụ trước vì ở đây`F(k) = 1`Và`F(k-1) = 0`: một đèn lồng bị loại trừ sẽ sáng chính xác khi phản hồi thay đổi khỏi`1`. 

Một trường hợp thú vị hơn xảy ra tại`k = 34`. Đây`F(34) = F(33) = 1`, Nhưng`F(32) = 4`. Như vậy`t = 2`. Một truy vấn loại trừ hai đèn lồng sẽ trả về một giá trị khác chính xác khi cả hai đèn lồng bị loại trừ đều sáng. Kiểm tra tất cả các cặp là đủ để phục hồi tất cả các đèn lồng đã thắp sáng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Truy vấn |`O(n^3)`| Nhiều nhất`C(n,3)`tập hợp con được truy vấn | 
| Xây dựng truy vấn |`O(n^4)`hoạt động nhân vật | Nhiều nhất`C(n,3)`truy vấn, mỗi truy vấn có độ dài`n`| 
| Tính toán trước |`O(k^2 sqrt(k))`| Chỉ có giá trị lên tới`k <= 100`được kiểm tra | 
| Thêm không gian |`O(n)`| Câu trả lời và truy vấn tạm thời mỗi lần sử dụng`O(n)`không gian | 

Giới hạn truy vấn là ràng buộc chính. Với`n <= 100`, trường hợp xấu nhất là`C(100,3) = 161,700`, an toàn bên dưới`200,000`giới hạn truy vấn. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm 

Các chương trình tương tác không thể được kiểm tra một cách có ý nghĩa chỉ bằng cách cho ăn`n`Và`k`, vì chương trình mong đợi phản hồi của thẩm phán sau mỗi truy vấn. Đối với thử nghiệm cục bộ, cách tiếp cận hữu ích là mô phỏng cấu hình đèn lồng ẩn và triển khai cùng một giao thức truy vấn bên trong khai thác thử nghiệm. 

Các thử nghiệm sau đây thực hiện logic tái thiết thay vì vận chuyển tương tác.```python
import itertools

def composite_divisor_count(x):
    if x <= 1:
        return 0

    ans = 0
    for d in range(2, x + 1):
        if x % d != 0:
            continue

        composite = False
        for q in range(2, int(d ** 0.5) + 1):
            if d % q == 0:
                composite = True
                break

        if composite:
            ans += 1

    return ans

def solve_offline(n, hidden):
    k = sum(hidden)

    f = [0] * (k + 1)
    for x in range(1, k + 1):
        f[x] = composite_divisor_count(x)

    base = f[k]

    t = None
    for candidate in range(1, 4):
        if f[k - candidate] != base:
            t = candidate
            break

    assert t is not None

    answer = [False] * n

    for excluded in itertools.combinations(range(n), t):
        r = sum(hidden[i] for i in excluded)
        response = f[k - r]

        if response != base:
            for i in excluded:
                answer[i] = True

        if sum(answer) == k:
            break

    return ''.join('1' if x else '0' for x in answer)

# Provided sample parameters, using a concrete hidden configuration.
assert solve_offline(
    9,
    [1, 0, 1, 1, 0, 0, 1, 0, 1]
) == "101100101"

# Minimum-size instance: every lantern is on.
assert solve_offline(
    4,
    [1, 1, 1, 1]
) == "1111"

# k = 5, where t = 1.
assert solve_offline(
    8,
    [1, 0, 1, 0, 1, 0, 0, 1]
) == "10101001"

# k = 26, where F(26) == F(25), forcing t = 2.
hidden = [0] * 30
for i in [1, 4, 7, 10, 12, 14, 16, 18, 19, 20,
          21, 22, 23, 24, 25, 26, 27, 28, 29]:
    hidden[i] = 1
# Add seven more lit lanterns to make k = 26.
for i in [0, 2, 3, 5, 6, 8, 9]:
    hidden[i] = 1

assert sum(hidden) == 26
assert solve_offline(30, hidden) == ''.join(
    '1' if x else '0' for x in hidden
)

# Maximum-size instance: all 100 lanterns are on.
assert solve_offline(
    100,
    [1] * 100
) == "1" * 100
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=9, k=5`với cấu hình năm chiếc đèn lồng cụ thể | Năm vị trí giống nhau | Tái thiết cơ bản và tương ứng với các tham số mẫu được cung cấp | 
|`n=4, k=4`|`1111`| tối thiểu`n`Và`k`, trong đó cấu hình duy nhất có thể được bật | 
|`n=8, k=5`|`10101001`| các`t=1`trường hợp | 
|`n=30, k=26`| Cấu hình 26-bit ẩn | Một trường hợp ranh giới trong đó`F(k)=F(k-1)`, buộc truy vấn cặp | 
|`n=100, k=100`| 100 cái | Đầu vào có kích thước tối đa và phát hiện sớm tất cả các đèn lồng đang thắp sáng | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là`n = k = 4`. Cấu hình duy nhất có thể là`1111`. Đây`F(4) = 1`trong khi`F(3) = 0`, do đó thuật toán chọn`t = 1`. Loại trừ bất kỳ chiếc đèn lồng nào, để lại chính xác ba chiếc đèn lồng được thắp sáng và tạo ra`0`, khác với phản ứng cơ bản`1`. Do đó, mỗi chiếc đèn lồng đều được đánh dấu, mang lại`1111`. 

Trường hợp cạnh thứ hai là phản hồi đếm số chia lặp lại. Lấy`k = 26`. chúng tôi có`F(26) = 1`Và`F(25) = 1`, vì vậy việc thử nghiệm các phần bổ sung của đèn lồng đơn lẻ sẽ không tiết lộ gì. Thuật toán tiếp tục`t = 2`, Ở đâu`F(24) = 5`, khác với`F(26)`. Đối với một cặp đèn lồng bị loại trừ, không có hoặc có một chiếc đèn thắp sáng trong số các lá đèn lồng`25`hoặc`26`thắp sáng những chiếc đèn lồng trong truy vấn, cả hai đều tạo ra`1`. Chỉ khi cả hai đèn lồng bị loại trừ đều sáng thì truy vấn mới chứa`24`thắp đèn lồng và trở về`5`. Do đó, cặp này hoạt động như một bài kiểm tra AND hai đèn lồng chính xác. 

Trường hợp cạnh thứ ba là`k = 34`. Đây`F(34) = F(33) = 1`, vì vậy phần bổ sung đèn lồng đơn lại thất bại. Nhưng`F(32) = 4`, cho`t = 2`. Thuật toán kiểm tra từng cặp và phát hiện chính xác các cặp gồm hai chiếc đèn lồng đang thắp sáng. Từ`k = 34`lớn hơn hai rất nhiều, mỗi chiếc đèn thắp sáng đều thuộc về nhiều cặp như vậy nên đều tìm lại được. 

Trường hợp cạnh thứ tư là`k = 35`. Các giá trị thỏa mãn`F(35) = F(34) = F(33) = 1`, do đó sự khác biệt đầu tiên xảy ra tại`t = 3`, với`F(32) = 4`. Đây là trường hợp đếm truy vấn tồi tệ nhất vì tất cả các tập con gồm ba phần tử có thể phải được kiểm tra. chỉ có`C(100,3) = 161,700`các tập hợp con như vậy ngay cả khi`n = 100`, vẫn ở dưới mức`200,000`giới hạn. Đối với bất kỳ tập hợp ba con nào chứa ít hơn ba chiếc đèn lồng được thắp sáng, phần bổ sung được truy vấn chứa`35`,`34`, hoặc`33`thắp đèn lồng và trở về`1`. Một tập hợp ba con chứa ba chiếc đèn lồng thắp sáng`32`thắp đèn lồng và trở về`4`, do đó chính xác các tập hợp con mong muốn sẽ được phát hiện. 

Cuối cùng, khi tất cả`n`đèn lồng đang bật, mọi bộ loại trừ được truy vấn đều sáng hoàn toàn. Sự khác biệt đầu tiên`t`vẫn được xác định duy nhất bởi`k`và các tập con được phát hiện đầu tiên sẽ ngay lập tức đánh dấu các thành viên của chúng. Thuật toán dừng ngay khi tất cả`k`các vị trí đã được đánh dấu nên không cần phải khai thác hết tất cả các truy vấn có thể có trong trường hợp này.
