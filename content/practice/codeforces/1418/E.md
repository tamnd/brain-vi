---
title: "CF 1418E - Thiệt hại dự kiến"
description: "Chúng tôi có nhiều điểm mạnh cố định của quái vật. Đối với mỗi truy vấn, một lá chắn bắt đầu bằng độ bền a và giá trị phòng thủ b. Quái vật được chiến đấu theo thứ tự ngẫu nhiên thống nhất. Quái vật có sức mạnh ít nhất là b sẽ tiêu tốn một điểm độ bền nhưng không gây sát thương."
date: "2026-06-11T06:51:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1418
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 95 (Rated for Div. 2)"
rating: 2400
weight: 1418
solve_time_s: 84
verified: true
draft: false
---

[CF 1418E - Thiệt hại dự kiến](https://codeforces.com/problemset/problem/1418/E) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, tổ hợp, xác suất 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhiều điểm mạnh cố định của quái vật. Đối với mỗi truy vấn, một tấm khiên bắt đầu bằng độ bền`a`và giá trị phòng thủ`b`. Quái vật được chiến đấu theo thứ tự ngẫu nhiên thống nhất. 

Một con quái vật có sức mạnh ít nhất là`b`tiêu tốn một điểm độ bền nhưng không gây sát thương. Quái vật yếu hơn không làm gì trong khi độ bền vẫn dương. Khi độ bền bằng 0, mọi quái vật còn lại sẽ gây sát thương bằng sức mạnh của nó. 

Câu hỏi yêu cầu tổng thiệt hại dự kiến ​​trên tất cả các đơn đặt hàng có thể xảy ra. 

Các giới hạn là lớn. Cả số lượng quái vật và số lượng truy vấn đều đạt`2·10^5`, vì vậy mọi thứ bậc hai đều là vô vọng. Ngay cả việc xử lý mọi truy vấn theo thời gian tuyến tính cũng sẽ cần khoảng`4·10^10`hoạt động. Chúng ta cần thứ gì đó xung quanh`O((n+m) log n)`. 

Trường hợp tinh tế đầu tiên xuất hiện khi không có đủ quái vật mạnh để làm cạn kiệt lá chắn.```
3 1
1 2 10
5 100
```Ít nhất không có quái vật nào có sức mạnh`100`, nên độ bền không bao giờ giảm. Thiệt hại luôn bằng 0 và câu trả lời là`0`. Một công thức bất cẩn chia cho số lượng quái vật mạnh sẽ sụp đổ. 

Một tình huống khó khăn khác xảy ra khi số lượng quái vật mạnh bằng độ bền.```
4 1
1 2 5 8
2 5
```Những con quái vật mạnh mẽ là`5`Và`8`. Chúng tiêu tốn chính xác hai điểm độ bền, sau đó không để lại độ bền, nhưng không còn quái vật mạnh nào để tấn công. Chỉ còn lại những quái vật yếu hơn nên thiệt hại dự kiến ​​là`1+2=3`. Đối xử bình đẳng với mọi quái vật sau khi khiên bị vỡ sẽ bị tính quá mức. 

Bản sao cũng có vấn đề.```
3 1
7 7 7
1 7
```Chính xác một quái vật tiêu tốn độ bền và hai quái vật còn lại gây sát thương. Thiệt hại dự kiến ​​luôn là`14`, không phải một số giá trị trung bình trên các giá trị riêng biệt. Sắp xếp và sử dụng bội số một cách chính xác sẽ tránh được sai lầm này. 

## Phương pháp tiếp cận 

Một quan điểm vũ phu là đơn giản. Đối với một truy vấn, liệt kê tất cả`n!`ra lệnh, mô phỏng cuộc chiến và tính trung bình thiệt hại gây ra. Điều này đúng về mặt toán học nhưng lại tốn kém một cách vô lý. 

Thậm chí thay thế phép liệt kê hoán vị bằng phép tính xác suất và xử lý từng truy vấn một cách độc lập trong`O(n)`quá chậm. Với`2·10^5`truy vấn, điều này trở nên đại khái`4·10^10`hoạt động. 

Quan sát quan trọng là chỉ những quái vật có sức mạnh ít nhất`b`ảnh hưởng đến độ bền. Giả sử có`k`những con quái vật như vậy. Nếu như`k<a`, tấm khiên không bao giờ bị vỡ và câu trả lời là số không. 

Cho rằng`k≥a`. Hãy xem xét một con quái vật mạnh mẽ. Trong số`k`quái vật mạnh, mọi mệnh lệnh tương đối đều có khả năng xảy ra như nhau. Chiếc khiên sống sót sau lần đầu tiên`a`quái vật mạnh và tan vỡ ngay sau đó. Một quái vật mạnh cụ thể gây sát thương chính xác khi nó xuất hiện sau vị trí`a`trong số đó`k`quái vật. 

Xác suất của sự kiện đó bằng$$\frac{k-a}{k}.$$Quái vật yếu không bao giờ tiêu tốn độ bền. Một khi lá chắn đã cạn kiệt bởi`a`quái vật mạnh, mọi quái vật yếu xuất hiện sau đó đều gây sát thương. Vì những quái vật yếu được xen kẽ ngẫu nhiên với những quái vật mạnh nên xác suất chúng xuất hiện sau điểm đột phá là$$\frac{k-a+1}{k+1}.$$Những xác suất này chỉ phụ thuộc vào`k`, không dựa trên sức mạnh cá nhân. Sau khi sắp xếp các điểm mạnh và xây dựng tổng tiền tố, chúng ta có thể thu được tổng điểm mạnh và điểm yếu theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!·n) | O(n) | Quá chậm | 
| Tối ưu | O((n+m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp sức mạnh của quái vật. 
2. Xây dựng tổng tiền tố sao cho tổng của bất kỳ hậu tố hoặc tiền tố nào có thể thu được trong thời gian không đổi. 
3. Đối với một truy vấn`(a,b)`, tìm kiếm nhị phân cho vị trí đầu tiên có giá trị ít nhất`b`. 
4. Hãy để`k`ít nhất là số lượng quái vật có sức mạnh`b`. 

Đây chính xác là những con quái vật tiêu tốn độ bền. 
5. Nếu`k<a`, đầu ra`0`. 

Chiếc khiên không bao giờ đạt đến độ bền bằng không. 
6. Tính tổng số quái mạnh, ký hiệu`Sstrong`, và tổng số quái vật yếu, ký hiệu là`Sweak`. 
7. Mỗi quái vật mạnh đều đóng góp xác suất`(k-a)/k`. 

Đóng góp dự kiến ​​của họ bằng$$S_{strong}\cdot\frac{k-a}{k}.$$1. Mỗi quái vật yếu đều đóng góp xác suất`(k-a+1)/(k+1)`. 

Đóng góp dự kiến ​​của họ bằng$$S_{weak}\cdot\frac{k-a+1}{k+1}.$$1. Thêm hai đóng góp và lấy mọi thứ theo modulo`998244353`. 

### Tại sao nó hoạt động 

Trong số những con quái vật mạnh mẽ, chỉ có thứ tự tương đối của chúng mới là quan trọng đối với mức tiêu hao độ bền. Đúng là lần đầu tiên`a`quái vật mạnh bị chặn, trong khi số còn lại`k-a`Quái vật mạnh tấn công sau khi lá chắn bị vỡ. Tính đối xứng mang lại xác suất cho mỗi quái vật mạnh`(k-a)/k`. 

Điểm đứt xảy ra ngay sau khi`a`-con quái vật mạnh mẽ thứ. Một con quái vật yếu có khả năng chiếm giữ bất kỳ con quái vật nào như nhau.`k+1`khoảng trống xung quanh những con quái vật mạnh mẽ. có`k-a+1`khoảng trống sau điểm phá vỡ, đưa ra xác suất`(k-a+1)/(k+1)`. Tổng hợp những kỳ vọng độc lập này đối với tất cả quái vật sẽ mang lại thiệt hại chính xác dự kiến. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

MOD = 998244353

n, m = map(int, input().split())
d = list(map(int, input().split()))

d.sort()

pref = [0]
for x in d:
    pref.append(pref[-1] + x)

inv = [1] * (n + 2)
for i in range(2, n + 2):
    inv[i] = pow(i, MOD - 2, MOD)

total_sum = pref[n]

ans = []

for _ in range(m):
    a, b = map(int, input().split())

    pos = bisect_left(d, b)
    k = n - pos

    if k < a:
        ans.append("0")
        continue

    strong_sum = total_sum - pref[pos]
    weak_sum = pref[pos]

    p_strong = (k - a) * inv[k] % MOD
    p_weak = (k - a + 1) * inv[k + 1] % MOD

    res = (
        strong_sum % MOD * p_strong
        + weak_sum % MOD * p_weak
    ) % MOD

    ans.append(str(res))

print("\n".join(ans))
```Mảng được sắp xếp cho phép tìm kiếm nhị phân xác định những con quái vật mạnh cho mỗi truy vấn. Tổng tiền tố cho tổng sức mạnh của cả hai nhóm mà không cần quét mảng. 

điều kiện`k<a`phải được xử lý trước khi tính xác suất. Nếu không, chúng ta sẽ gán sai tử số âm. 

Việc sử dụng xác suất yếu`k+1`ở mẫu số, không`k`. Đây là nguồn sai lầm phổ biến nhất. Một con quái vật yếu được chèn vào giữa các khoảng trống xung quanh những con quái vật mạnh và có nhiều khoảng trống hơn những con quái vật mạnh. 

Tất cả các phép chia đều được thực hiện bằng nghịch đảo mô đun vì mô đun là số nguyên tố. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu.```
3 2
1 3 1
2 1
1 2
```Điểm mạnh được sắp xếp là`[1,1,3]`. 

### Truy vấn`(2,1)`| Số lượng | Giá trị | 
| --- | --- | 
| k | 3 | 
| Tổng mạnh | 5 | 
| Tổng yếu | 0 | 
| Xác suất mạnh | 1/3 | 
| Xác suất yếu | 2/4 | 
| Thiệt hại dự kiến ​​| 3/5 | 

Modulo`998244353`,`5/3`trở thành`665496237`. 

Ví dụ này cho thấy ngay cả những con quái vật mạnh cũng có thể gây sát thương khi chúng có nhiều hơn độ bền. 

### Truy vấn`(1,2)`| Số lượng | Giá trị | 
| --- | --- | 
| k | 1 | 
| Tổng mạnh | 3 | 
| Tổng yếu | 2 | 
| Xác suất mạnh | 0 | 
| Xác suất yếu | 1/2 | 
| Thiệt hại dự kiến ​​| 1 | 

Con quái vật mạnh mẽ duy nhất sẽ tiêu tốn điểm độ bền duy nhất. Sau đó cả hai quái vật yếu đều có xác suất`1/2`xuất hiện sau này. 

Hãy xem xét một ví dụ khác.```
4 1
1 2 5 8
2 5
```| Số lượng | Giá trị | 
| --- | --- | 
| k | 2 | 
| Tổng mạnh | 13 | 
| Tổng yếu | 3 | 
| Xác suất mạnh | 0 | 
| Xác suất yếu | 1/3 | 
| Thiệt hại dự kiến ​​| 1 | 

Dấu vết này nhấn mạnh thực tế là những con quái vật mạnh không đóng góp gì khi số lượng của chúng ngang bằng với độ bền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log n) | Sắp xếp cộng với một tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian | O(n) | Đã sắp xếp mảng và tổng tiền tố | 

Chi phí phân loại được thanh toán một lần. Mỗi truy vấn chỉ thực hiện tìm kiếm nhị phân và một vài phép tính số học, dễ dàng phù hợp với giới hạn của`2·10^5`truy vấn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MOD = 998244353

    n, m = map(int, input().split())
    d = list(map(int, input().split()))
    d.sort()

    pref = [0]
    for x in d:
        pref.append(pref[-1] + x)

    total_sum = pref[n]
    inv = [1] * (n + 2)
    for i in range(2, n + 2):
        inv[i] = pow(i, MOD - 2, MOD)

    out = []

    for _ in range(m):
        a, b = map(int, input().split())
        pos = bisect_left(d, b)
        k = n - pos

        if k < a:
            out.append("0")
            continue

        strong_sum = total_sum - pref[pos]
        weak_sum = pref[pos]

        p1 = (k - a) * inv[k] % MOD
        p2 = (k - a + 1) * inv[k + 1] % MOD

        val = (
            strong_sum % MOD * p1
            + weak_sum % MOD * p2
        ) % MOD

        out.append(str(val))

    return "\n".join(out)

# provided sample
assert run(
"""3 2
1 3 1
2 1
1 2
"""
) == """665496237
1"""

# minimum size
assert run(
"""1 1
5
1 10
"""
) == "0"

# all equal
assert run(
"""3 1
7 7 7
1 7
"""
) == "14"

# no shield break
assert run(
"""4 1
1 2 3 4
4 5
"""
) == "0"

# boundary k = a
assert run(
"""4 1
1 2 5 8
2 5
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quái đơn, phòng thủ lớn | 0 | Khiên không bao giờ vỡ | 
| Mọi điểm mạnh đều ngang nhau | 14 | Xử lý đa dạng | 
| Không có quái vật nào đạt được phòng thủ | 0 |`k<a`chi nhánh | 
|`k=a`| 1 | Ranh giới giữa sống sót và phá vỡ | 

## Vỏ cạnh 

Khi không có con quái vật nào đủ mạnh,```
3 1
1 2 10
2 100
```tìm kiếm nhị phân mang lại`k=0`. Từ`0<2`, thuật toán ngay lập tức xuất ra`0`. Không có tính toán xác suất được thực hiện. 

Khi số lượng quái vật mạnh bằng độ bền,```
4 1
1 2 5 8
2 5
```chúng tôi có được`k=2`. Xác suất mạnh trở thành bằng 0 vì`k-a=0`. Quái vật yếu góp phần có xác suất`1/3`, tạo ra thiệt hại dự kiến$$(1+2)\cdot\frac13=1.$$Khi tất cả các giá trị đều giống nhau,```
3 1
7 7 7
1 7
```tất cả quái vật thuộc nhóm mạnh. Đây`k=3`, tổng mạnh là`21`và mỗi quái vật đóng góp với xác suất`2/3`. Thiệt hại dự kiến ​​bằng`14`. Việc sắp xếp sẽ bảo tồn tất cả các bản sao, do đó độ mạnh trùng lặp được xử lý một cách tự nhiên.
