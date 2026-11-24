---
title: "CF 86A - Phản xạ"
description: "Chúng ta được cho hai số nguyên, l và r. Đối với mỗi số n trong khoảng này, chúng ta xây dựng một số khác gọi là số phản ánh của nó. Sự phản ánh được tạo ra từng chữ số. Mỗi chữ số thập phân d trở thành 9 - d."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 86
codeforces_index: "A"
codeforces_contest_name: "Yandex.Algorithm 2011: Round 2"
rating: 1600
weight: 86
solve_time_s: 129
verified: true
draft: false
---

[CF 86A - Phản ánh](https://codeforces.com/problemset/problem/86/A) 

**Đánh giá:** 1600 
**Thẻ:** toán 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`l`Và`r`. Đối với mỗi số`n`bên trong khoảng này, chúng ta xây dựng một số khác gọi là số phản ánh của nó. 

Sự phản ánh được tạo ra từng chữ số. Mỗi chữ số thập phân`d`trở thành`9 - d`. Ví dụ:

-`192 → 807`-`10 → 89`-`91 → 08 → 8`Trọng số của một số là:$$n \cdot \psi(n)$$Ở đâu$\psi(n)$là sự phản ánh của`n`. 

Nhiệm vụ là tìm trọng số tối đa có thể có trong số tất cả các số nguyên trong phạm vi`[l, r]`. 

Phạm vi có thể lên tới$10^9$. Việc quét mạnh mẽ trên mọi số ngay lập tức bị nghi ngờ vì bản thân khoảng thời gian đó có thể chứa gần một tỷ giá trị. Ngay cả khi việc tính toán sự phản chiếu chỉ mất một vài thao tác, việc lặp qua$10^9$ứng viên sẽ không phù hợp với giới hạn 2 giây. 

Khó khăn chính là sự phản ánh phụ thuộc vào từng chữ số một cách độc lập nên tích không bị đơn điệu. Số lớn hơn không nhất thiết cho trọng số lớn hơn. 

Có một số trường hợp khó bỏ sót. 

Hãy xem xét phạm vi:```
8 10
```Các trọng số là: 

-`8 * 1 = 8`-`9 * 0 = 0`-`10 * 89 = 890`Mức tối đa tăng vọt khi chuyển từ độ dài chữ số này sang độ dài chữ số khác. Một giả định ngây thơ như “câu trả lời gần r” không thành công ở đây. 

Một trường hợp nguy hiểm khác là số có tận cùng bằng nhiều`9`S:```
99 → 0
999 → 0
```Sự phản chiếu của chúng giảm xuống 0 vì tất cả các chữ số trở thành`0`. Việc triển khai bất cẩn để giữ các số 0 đứng đầu dưới dạng chữ số thập phân có thể xử lý không chính xác`"00"`dưới dạng một số có hai chữ số có ý nghĩa. 

Ngoài ra còn có trường hợp cạnh cấu trúc nằm ở giữa phạm vi hợp lệ cho số chữ số cố định. Đối với số có hai chữ số: 

| n | ψ(n) | Sản phẩm | 
| --- | --- | --- | 
| 40 | 59 | 2360 | 
| 44 | 55 | 2420 | 
| 49 | 50 | 2450 | 
| 50 | 49 | 2450 | 
| 55 | 44 | 2420 | 

Mức tối đa xuất hiện xung quanh tâm, không phải ở điểm cuối. Hiểu hình dạng này là cốt lõi của giải pháp. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là lặp lại mọi số nguyên từ`l`ĐẾN`r`, tính toán độ phản xạ của nó, nhân hai giá trị và giữ mức tối đa. 

Tính toán sự phản ánh là dễ dàng. Nếu một chữ số là`d`, chữ số phản ánh của nó là`9 - d`. Chúng ta có thể xử lý số dưới dạng chuỗi hoặc từng chữ số một cách toán học. 

Phương pháp brute-force này đúng vì nó kiểm tra mọi ứng cử viên một cách rõ ràng. Vấn đề là số lượng ứng viên. Trong trường hợp xấu nhất, độ dài khoảng gần bằng$10^9$. Ngay cả chỉ với một vài phép tính số học cho mỗi số, điều này vẫn vượt xa khả năng thực hiện. 

Bước ngoặt đến từ việc viết lại phép phản xạ bằng đại số. 

Giả định`n`có chính xác`k`chữ số. Cho phép:$$M = 10^k - 1$$Đây là con số bao gồm toàn bộ`9`s có chiều dài`k`. 

Ví dụ:

-`k = 2 → M = 99`-`k = 3 → M = 999`Phản ánh từng chữ số một cách độc lập hoàn toàn giống như:$$\psi(n) = M - n$$Ví dụ:$$192 \rightarrow 999 - 192 = 807$$Vậy trọng lượng trở thành:$$f(n)=n(M-n)$$Việc mở rộng mang lại:$$f(n) = -n^2 + Mn$$Đây là một parabol bậc hai mở xuống. Cực đại của nó xảy ra lúc:$$n = \frac{M}{2}$$Điều đó thay đổi toàn bộ vấn đề. Thay vì tìm kiếm hàng tỷ giá trị, chúng ta chỉ cần kiểm tra các con số gần đỉnh parabol. 

Có một điều phức tạp. Công thức chỉ áp dụng cho các số có cùng số chữ số. Khi số chữ số thay đổi,`M`cũng thay đổi. Từ`r ≤ 10^9`, có nhiều nhất 10 độ dài chữ số khác nhau cần xét. 

Đối với mỗi độ dài chữ số: 

- xác định giao điểm hợp lệ với`[l, r]`- Tính trung điểm$M/2$- kiểm tra các số nguyên gần nhất xung quanh điểm giữa đó trong khoảng 

Bởi vì một phương trình bậc hai đạt cực đại ở tâm nên chỉ cần một số lượng ứng viên không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r - l + 1) | O(1) | Quá chậm | 
| Tối ưu | O(số chữ số) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại mọi độ dài chữ số có thể`k`từ 1 đến 10. 

Kể từ khi`r ≤ 10^9`, chỉ tồn tại một số ít độ dài chữ số. 
2. Với độ dài chữ số hiện tại, hãy tính:$$M = 10^k - 1$$Đây là số toàn 9 cho độ dài này. 

1. Tính khoảng hợp lệ của`k`-các chữ số cũng nằm bên trong`[l, r]`. 

Nhỏ nhất`k`-số có chữ số là:$$10^{k-1}$$ngoại trừ`k = 1`, trong đó mức tối thiểu là`1`. 

lớn nhất`k`-số có chữ số là:$$10^k - 1$$1. Giao khoảng độ dài chữ số này với`[l, r]`. 

Nếu giao điểm trống, bỏ qua độ dài chữ số này. 
2. Hàm của độ dài chữ số này là:$$f(n)=n(M-n)$$Vì đây là một parabol hướng xuống nên cực đại xảy ra gần:$$\frac{M}{2}$$1. Hãy:$$x = \left\lfloor \frac{M}{2} \right\rfloor$$Ứng cử viên số nguyên tối ưu phải là`x`hoặc`x + 1`. 

1. Kẹp các ứng cử viên này vào khoảng hợp lệ cho độ dài chữ số này. 

Đồng thời kiểm tra ranh giới khoảng vì điểm giữa có thể nằm ngoài khoảng. 
2. Đánh giá sản phẩm cho tất cả các ứng viên được thu thập và giữ mức tối đa toàn cầu. 

### Tại sao nó hoạt động 

Đối với độ dài chữ số cố định`k`, mọi số đều thỏa mãn:$$\psi(n)=M-n$$Ở đâu$M=10^k-1$. Hàm trọng số trở thành:$$f(n)=n(M-n)$$đó là một phương trình bậc hai lõm. Một phương trình bậc hai lõm đạt cực đại tại đỉnh của nó, nằm ở$M/2$. Vì chúng ta chỉ quan tâm đến các số nguyên bên trong một khoảng bị chặn nên giá trị tối ưu phải là một trong những số nguyên gần đỉnh nhất hoặc một trong các ranh giới khoảng nếu đỉnh nằm bên ngoài. 

Chúng tôi tối đa hóa độc lập theo từng độ dài chữ số và mọi số trong`[l, r]`thuộc về đúng một nhóm có độ dài chữ số. Việc tận dụng những gì tốt nhất trong số tất cả các nhóm sẽ mang lại sự tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def reflection(x):
    s = str(x)
    t = ''.join(str(9 - int(c)) for c in s)
    return int(t)

def weight(x):
    return x * reflection(x)

l, r = map(int, input().split())

ans = 0

for k in range(1, 11):
    low = 1 if k == 1 else 10 ** (k - 1)
    high = 10 ** k - 1

    L = max(l, low)
    R = min(r, high)

    if L > R:
        continue

    M = high
    mid = M // 2

    candidates = {L, R}

    for x in [mid, mid + 1]:
        if L <= x <= R:
            candidates.add(x)

    for x in candidates:
        ans = max(ans, weight(x))

print(ans)
```Giải pháp nhóm các số theo độ dài chữ số vì công thức phản ánh phụ thuộc vào số đó có bao nhiêu chữ số. 

Đối với độ dài chữ số cố định, phản ánh tương đương với việc trừ đi số 9 có độ dài đó. Mã vẫn tính toán phản ánh trực tiếp để đảm bảo rõ ràng và an toàn. Vì chỉ có một số lượng ứng viên không đổi được đánh giá nên hiệu quả không bị ảnh hưởng. 

Việc tính toán điểm giữa là tinh tế. Bậc hai cực đại xảy ra ở`M / 2`, Nhưng`M`là số lẻ với mọi số đều là 9. Điều đó có nghĩa là mức tối đa thực sự nằm bằng nhau giữa hai số nguyên. Chúng ta phải kiểm tra cả hai`mid`Và`mid + 1`. 

Tập ứng cử viên cũng bao gồm các ranh giới khoảng. Nếu đỉnh parabol nằm ngoài phạm vi hợp lệ cho độ dài chữ số này thì mức tối đa trong khoảng đó xảy ra ở ranh giới gần nhất. 

Số nguyên Python tự động xử lý các sản phẩm lớn một cách an toàn. Trong C++, điều này yêu cầu số nguyên 64 bit vì câu trả lời có thể vượt quá`2^31`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 7
```Đối với số có một chữ số:$$M = 9$$Chức năng là:$$f(n)=n(9-n)$$| k | Khoảng thời gian | M | giữa | Ứng viên | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [3, 7] | 9 | 4 | 3, 4, 5, 7 | 20 | 

Kiểm tra: 

-`3 * 6 = 18`-`4 * 5 = 20`-`5 * 4 = 20`-`7 * 2 = 14`Trả lời:```
20
```Ví dụ này cho thấy tính đối xứng của phương trình bậc hai xung quanh điểm giữa. 

### Ví dụ 2 

đầu vào:```
8 10
```| k | Khoảng thời gian | M | giữa | Ứng viên | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [8, 9] | 9 | 4 | 8, 9 | 8 | 
| 2 | [10, 10] | 99 | 49 | 10 | 890 | 

Vì`10`:$$\psi(10)=89$$

$$10 \cdot 89 = 890$$Trả lời:```
890
```Dấu vết này chứng tỏ tại sao việc chuyển đổi độ dài chữ số lại quan trọng. Di chuyển từ`9`ĐẾN`10`thay đổi cơ sở phản ánh từ`9`ĐẾN`99`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Độ dài tối đa 10 chữ số và chỉ kiểm tra ứng viên liên tục | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Thời gian chạy thực tế là không đổi vì số lượng độ dài chữ số không bao giờ vượt quá 10. Điều này dễ dàng phù hợp với giới hạn thời gian ngay cả trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    def reflection(x):
        s = str(x)
        t = ''.join(str(9 - int(c)) for c in s)
        return int(t)

    def weight(x):
        return x * reflection(x)

    l, r = map(int, input().split())

    ans = 0

    for k in range(1, 11):
        low = 1 if k == 1 else 10 ** (k - 1)
        high = 10 ** k - 1

        L = max(l, low)
        R = min(r, high)

        if L > R:
            continue

        M = high
        mid = M // 2

        candidates = {L, R}

        for x in [mid, mid + 1]:
            if L <= x <= R:
                candidates.add(x)

        for x in candidates:
            ans = max(ans, weight(x))

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("3 7\n") == "20", "sample 1"

# minimum range
assert run("1 1\n") == "8", "single value"

# digit-length transition
assert run("8 10\n") == "890", "crossing 9 to 10"

# midpoint optimum
assert run("40 60\n") == "2450", "peak near center"

# all equal large value
assert run("999999999 999999999\n") == "0", "all nines reflect to zero"

# large boundary case
assert run("1000000000 1000000000\n") == "899999999000000000", "largest input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`8`| Đầu vào hợp lệ tối thiểu | 
|`8 10`|`890`| Chuyển đổi độ dài chữ số | 
|`40 60`|`2450`| Tối đa gần tâm parabol | 
|`999999999 999999999`|`0`| Phản ánh của tất cả số 9 | 
|`1000000000 1000000000`|`899999999000000000`| Giá trị biên lớn nhất | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
9 9
```Sự phản xạ:$$\psi(9)=0$$Cân nặng:$$9 \cdot 0 = 0$$Thuật toán xử lý việc này một cách tự nhiên. Đối với số có một chữ số:$$M=9$$Và:$$9(9-9)=0$$Không cần xử lý đặc biệt đối với các số 0 đứng đầu vì việc chuyển đổi số nguyên sẽ tự động loại bỏ chúng. 

Bây giờ hãy xem xét:```
8 10
```Điểm giữa của số có một chữ số là khoảng`4`, nằm bên ngoài`[8,9]`. Thuật toán quay lại kiểm tra ranh giới khoảng một cách chính xác. 

Đối với các số có hai chữ số, ứng cử viên hợp lệ duy nhất là`10`, phản xạ của nó trở thành`89`. Câu trả lời nhảy từ`8`ĐẾN`890`. 

Một trường hợp cạnh quan trọng khác là:```
50 50
```Đối với số có hai chữ số:$$M=99$$Sự phản xạ:$$99-50=49$$Cân nặng:$$50 \cdot 49=2450$$Trung điểm của parabol nằm giữa`49`Và`50`, do đó cả hai đều tạo ra giá trị tối ưu như nhau. Kiểm tra cả hai`mid`Và`mid+1`đảm bảo tính đúng đắn cho số lẻ`M`. 

Cuối cùng, hãy xem xét:```
999999999 1000000000
```Vì`999999999`:$$\psi=0$$Trọng lượng bằng không. 

Vì`1000000000`:$$\psi=8999999999$$Trọng lượng trở nên vô cùng lớn. 

Thuật toán xử lý độ dài hai chữ số một cách độc lập và xác định chính xác kết quả lớn hơn từ khoảng 10 chữ số.
