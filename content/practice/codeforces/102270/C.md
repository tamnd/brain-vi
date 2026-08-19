---
title: "CF 102270C - Chia"
description: "Chúng ta cần đếm các số nguyên dương (N) trong khoảng ([A,B]) thỏa mãn ba ràng buộc có vẻ độc lập. Đầu tiên, mọi chữ số thập phân của (N) phải thuộc tập hợp được cung cấp (S). Thứ hai, (N) phải chia hết cho (X)."
date: "2026-08-19T05:01:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102270
codeforces_index: "C"
codeforces_contest_name: "HCW 19 Individual Day 2"
rating: 0
weight: 102270
solve_time_s: 619
verified: false
draft: false
---

[CF 102270C - Chia](https://codeforces.com/problemset/problem/102270/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các số nguyên dương (N) trong khoảng ([A,B]) thỏa mãn ba ràng buộc có vẻ độc lập. 

Đầu tiên, mọi chữ số thập phân của (N) phải thuộc tập hợp được cung cấp (S). Thứ hai, (N) phải chia hết cho (X). Thứ ba, nếu chúng ta đánh số các vị trí từ bên phải thì chữ số ở vị trí (i) phải nguyên tố cùng nhau với (i). Nói cách khác, đối với chữ số (d_i) ở vị trí (i)-th tính từ bên phải, chúng ta yêu cầu (\gcd(d_i,i)=1). Câu trả lời là số lượng các số nguyên như vậy, modulo giảm (10^9+7). 

Các ràng buộc chính thức cho phép (B) có tối đa 101 chữ số thập phân, trong khi (X) có thể lớn bằng (10^5). Điều đó ngay lập tức loại trừ việc liệt kê khoảng thời gian. Ngay cả việc lưu trữ hoặc lặp lại tất cả các số cũng sẽ cần tới khoảng (10^{100}) ứng cử viên. Chúng ta cần một thuật toán có chi phí phụ thuộc vào số chữ số và trên (X), không phụ thuộc vào kích thước số của (B). 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Đầu tiên là số 0 đứng đầu. Số 0 có thể là chữ số hợp lệ ở vị trí bên trong, nhưng nó không thể là chữ số có nghĩa nhất của số nguyên dương. Ví dụ, với đầu vào```
1 9 1
0
```câu trả lời là`0`. Việc thực hiện bất cẩn có thể liên quan đến`0`dưới dạng số có một chữ số hợp lệ hoặc coi các biểu diễn như`01`dưới dạng số nguyên dương thông thường. 

Vấn đề thứ hai là điều kiện nguyên tố cùng được tính từ bên phải chứ không phải từ bên trái. Ví dụ,`12`hợp lệ đối với điều kiện vị trí vì chữ số ngoài cùng bên phải của nó là (2) và (\gcd(2,1)=1), trong khi chữ số bên trái của nó là (1) và (\gcd(1,2)=1). Đây là lý do tại sao chỉ cần kiểm tra chữ số theo chỉ số của nó trong cách biểu diễn từ trái sang phải sẽ cho kết quả sai. 

Vấn đề thứ ba là giới hạn trên khi xử lý các chữ số từ bên phải. Coi như`B = 20`và số có một chữ số`8`. So sánh chữ số thấp`8`với chữ số thấp`0`thực hiện phép trừ`0 - 8`mượn, nhưng`8`rõ ràng là vẫn nhỏ hơn`20`. DP bị ràng buộc từ phải sang trái phải nhớ rằng số ngắn hơn sẽ tự động nằm dưới giới hạn trên dương dài hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi số nguyên từ (A) đến (B). Đối với mỗi ứng cử viên, chúng tôi sẽ kiểm tra các chữ số của nó, xác minh tư cách thành viên trong (S), kiểm tra điều kiện nguyên tố cùng nhau ở mọi vị trí và kiểm tra tính chia hết cho (X). Điều này đúng vì mỗi ứng viên đều được kiểm tra chính xác một lần và mọi điều kiện bắt buộc đều được kiểm tra rõ ràng. 

Vấn đề là kích thước của khoảng. Khi (B) có 101 chữ số thì có thể có thứ tự (10^{100}) ứng viên. Ngay cả khi việc kiểm tra một ứng cử viên chỉ thực hiện (O(100)) thao tác, thì trường hợp xấu nhất sẽ là đại khái (O(100\cdot10^{100})), điều này hoàn toàn không khả thi. 

Quan sát quan trọng là các hạn chế là chữ số cục bộ ngoại trừ khả năng chia hết. Tại vị trí (i), chúng ta có thể xác định ngay chữ số nào là hợp lệ bằng cách kiểm tra (\gcd(d,i)=1). Tính chia hết cũng có thể được biểu diễn bằng modulo dư (X). Điều đó mang lại một chữ số DP có trạng thái chỉ chứa phần dư và một lượng nhỏ thông tin về sự so sánh với giới hạn. 

Có một thủ thuật đặc biệt hữu ích ở đây. Điều kiện vị trí được xác định từ bên phải, do đó thay vì xử lý giới hạn từ trái sang phải, chúng ta xử lý cả số được xây dựng và giới hạn từ phải sang trái. Sau đó, sự so sánh có thể được thể hiện bằng khoản vay được tạo ra trong khi trừ đi số được xây dựng khỏi giới hạn. 

Giả sử các chữ số (i) thấp hiện đang được xử lý của giới hạn là (B_{\text{low}}) và các chữ số tương ứng của số được xây dựng là (N_{\text{low}}). Chúng tôi duy trì xem phép trừ (B_{\text{low}}-N_{\text{low}}) có cần mượn vào vị trí tiếp theo hay không. Điều này biến điều kiện chặt chẽ từ trái sang phải thông thường thành điều kiện vay hai trạng thái. 

Phần còn lại cũng thuận tiện từ bên phải. Nếu các chữ số thấp (i-1) đã được chọn tạo thành một giá trị có số dư (r) và chữ số mới ở vị trí (i) là (d), thì giá trị mới là 

[ 
r+d\cdot10^{i-1}. 
] 

Vì vậy chúng ta chỉ cần duy trì modulo dư (X). 

Cùng một DP có thể đếm mọi độ dài có thể cho đến độ dài giới hạn. Nếu số được xây dựng có ít chữ số hơn giới hạn thì số đó sẽ tự động nhỏ hơn, bất kể lần mượn cuối cùng là bao nhiêu. Chỉ một số có cùng số chữ số với giới hạn mới cần mượn số 0 cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(100\cdot10^{100})) | (O(1)) | Quá chậm | 
| Tối ưu | (O(LX | S | )) mỗi giới hạn | (O(X)) | Đã chấp nhận | 

Đây (L\le101). Chúng tôi chạy DP giới hạn cho (B) và cho (A-1), điều này chỉ thay đổi hệ số không đổi. 

## Hướng dẫn thuật toán 

1. Xác định`dp0[r]`Và`dp1[r]`sau khi xử lý một số vị trí từ bên phải.`dp0[r]`đếm các công trình có phép trừ hiện tại từ các chữ số thấp tương ứng của giới hạn không có khoản vay, trong khi`dp1[r]`đếm các công trình có vay mượn. Cả hai mảng đều được lập chỉ mục theo modulo còn lại (X). 

Ban đầu không có chữ số nào được chọn, số dư bằng 0 và không có chữ số mượn, vì vậy`dp0[0] = 1`và mọi trạng thái khác đều bằng không. 
2. Xử lý các vị trí (i=1,2,\ldots,L) từ phải sang trái. Với mỗi vị trí, hãy xây dựng danh sách các chữ số từ (S) thỏa mãn (\gcd(d,i)=1). Đây chính xác là những chữ số có thể xuất hiện ở vị trí này. 

Điều này trực tiếp kết hợp điều kiện vị trí bất thường vào quá trình chuyển đổi, do đó, một chữ số không hợp lệ sẽ không bao giờ được đưa vào DP. 
3. Duy trì (p=10^{i-1}\bmod X). Nếu chữ số hiện tại là (d), việc thêm nó vào các vị trí thấp hơn đã được tạo sẵn sẽ thay đổi phần còn lại từ (r) thành 

[ 
(r+d p)\bmod X. 
] 

Sức mạnh được cập nhật bằng cách nhân với 10 sau mỗi vị trí. 
4. Gọi (b) là chữ số thứ (i) của giới hạn từ bên phải. Giả sử khoản vay trước đó là (c). Phép trừ ở vị trí này là 

[ 
b-d-c. 
] 

Khoản vay mới được xác định bằng việc liệu giá trị này có âm hay không. Chỉ có ba trường hợp. 

Nếu (d<b), cả hai khoản vay cũ đều không tạo ra khoản vay mới. 

Nếu (d=b), khoản vay cũ vẫn là khoản vay, trong khi không có khoản vay cũ vẫn không có khoản vay. 

Nếu (d>b), cả hai khoản vay cũ đều có thể tạo ra một khoản vay mới. 

Đây là giá trị tương đương từ phải sang trái của quá trình chuyển đổi trạng thái chặt chẽ quen thuộc ở chữ số DP thông thường. 
5. Sau khi xử lý vị trí (i), đếm các số có độ dài đúng (i). Để đặt vị trí (i) là vị trí quan trọng nhất, chữ số được chọn phải khác 0. Đối với mọi chữ số hợp pháp khác 0 (d), chúng ta biết chính xác số dư trước đó có thể dẫn đến số dư bằng 0, cụ thể là 

[ 
r\equiv-d\cdot10^{i-1}\pmod X. 
] 

Chỉ có một số dư như vậy, vì vậy việc đếm các số chia hết có độ dài chính xác chỉ cần một số thao tác bổ sung không đổi cho mỗi chữ số. 
6. Nếu (i<L), mọi số dương có chữ số (i) đều nhỏ hơn giới hạn chữ số (L). Do đó, cả hai trạng thái vay đều được chấp nhận khi tính phần đóng góp của độ dài này. 

Nếu (i=L), số có cùng độ dài với giới hạn, do đó chỉ những trạng thái có số vay cuối cùng bằng 0 mới được chấp nhận. 
7. Tính toán`count_leq(B)`, sau đó giảm (A) đi một dưới dạng chuỗi thập phân và tính`count_leq(A-1)`. Câu trả lời khoảng cần thiết là 

[ 
\text{count_leq}(B)-\text{count_leq}(A-1). 
] 

Phép trừ được thực hiện theo modulo (10^9+7). 

Tại sao nó hoạt động: sau khi xử lý (i) vị trí đầu tiên từ bên phải, mỗi trạng thái DP thể hiện chính xác một tập hợp các lựa chọn hợp lệ cho các vị trí đó, được phân loại theo modulo còn lại (X) và theo khoản vay trong phép trừ từ giới hạn. Quá trình chuyển đổi xem xét mọi chữ số hợp pháp tiếp theo chính xác một lần và gửi nó đến trạng thái mượn và dư mới chính xác về mặt toán học. Khi một số dừng sau (i) chữ số, chữ số có nghĩa nhất của nó bắt buộc phải khác 0. Đối với các số ngắn hơn, cả hai trạng thái mượn đều hợp lệ vì phần cao hơn chưa được xử lý của giới hạn chứa ít nhất một chữ số dương. Đối với các số có độ dài bằng nhau, mượn số 0 chính xác là điều kiện mà số được xây dựng nhiều nhất là giới hạn. Do đó, mỗi số hợp lệ được tính một lần và không có số không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

MOD = 1_000_000_007

def dec_one(s):
    s = s.lstrip('0')
    if not s:
        return None

    a = list(s)
    i = len(a) - 1

    while i >= 0 and a[i] == '0':
        a[i] = '9'
        i -= 1

    if i < 0:
        return None

    a[i] = str(ord(a[i]) - ord('0') - 1)

    res = ''.join(a).lstrip('0')
    return res if res else '0'

def count_leq(t, x, source_digits):
    if t is None or t == '0':
        return 0

    t = t.lstrip('0')
    if not t:
        return 0

    length = len(t)

    allowed = [[] for _ in range(length + 1)]
    for pos in range(1, length + 1):
        cur = allowed[pos]
        for d in source_digits:
            if gcd(d, pos) == 1:
                cur.append(d)

    # dp0[r]: low processed digits have no borrow.
    # dp1[r]: low processed digits have a borrow.
    dp0 = [0] * x
    dp1 = [0] * x
    dp0[0] = 1

    power = 1
    answer = 0

    # Digits of t indexed from the right.
    bound_digits = [ord(c) - 48 for c in reversed(t)]

    for pos in range(1, length + 1):
        bd = bound_digits[pos - 1]
        digits = allowed[pos]

        ndp0 = [0] * x
        ndp1 = [0] * x

        # Count numbers that stop at this position.
        # For pos < length, both borrow states are valid.
        # For pos == length, only borrow 0 is valid.
        if pos < length:
            length_count = 0

            for d in digits:
                if d == 0:
                    continue

                add = (d * power) % x
                prev = (-add) % x

                v = dp0[prev] + dp1[prev]
                if v >= MOD:
                    v -= MOD

                length_count += v
                if length_count >= MOD:
                    length_count -= MOD

            answer += length_count
            if answer >= MOD:
                answer -= MOD
        else:
            length_count = 0

            for d in digits:
                if d == 0:
                    continue

                add = (d * power) % x
                prev = (-add) % x

                if d < bd:
                    v = dp0[prev] + dp1[prev]
                elif d == bd:
                    v = dp0[prev]
                else:
                    v = 0

                if v >= MOD:
                    v -= MOD

                length_count += v
                if length_count >= MOD:
                    length_count -= MOD

            answer += length_count
            if answer >= MOD:
                answer -= MOD

        # Build the DP for the next position.
        for d in digits:
            add = (d * power) % x

            if d < bd:
                # Both old borrow states become borrow 0.
                src0 = dp0
                src1 = dp1
                dest = ndp0

                for r in range(x):
                    j = r + add
                    if j >= x:
                        j -= x

                    v = src0[r] + src1[r]
                    if v >= MOD:
                        v -= MOD

                    v += dest[j]
                    if v >= MOD:
                        v -= MOD

                    dest[j] = v

            elif d == bd:
                # No old borrow stays at 0, old borrow stays at 1.
                src0 = dp0
                src1 = dp1

                for r in range(x):
                    j = r + add
                    if j >= x:
                        j -= x

                    v = ndp0[j] + src0[r]
                    if v >= MOD:
                        v -= MOD
                    ndp0[j] = v

                    v = ndp1[j] + src1[r]
                    if v >= MOD:
                        v -= MOD
                    ndp1[j] = v

            else:
                # Both old borrow states become borrow 1.
                src0 = dp0
                src1 = dp1
                dest = ndp1

                for r in range(x):
                    j = r + add
                    if j >= x:
                        j -= x

                    v = src0[r] + src1[r]
                    if v >= MOD:
                        v -= MOD

                    v += dest[j]
                    if v >= MOD:
                        v -= MOD

                    dest[j] = v

        dp0, dp1 = ndp0, ndp1

        power = (power * 10) % x

    return answer

def solve_case(a, b, x, s):
    digits = [ord(c) - 48 for c in s]

    right_of_a = dec_one(a)

    upper = count_leq(b, x, digits)
    lower = count_leq(right_of_a, x, digits)

    return (upper - lower) % MOD

def main():
    A, B, X = input().split()
    X = int(X)
    S = input().strip()

    print(solve_case(A, B, X, S))

if __name__ == "__main__":
    main()
```các`allowed`mảng là việc thực hiện trực tiếp quy tắc vị trí. Vị trí 1 rất đặc biệt vì mọi chữ số đều nguyên tố cùng nhau với 1, trong khi số 0 cũng được phép ở đó về mặt toán học. Số 0 vẫn hợp lệ đối với vị trí ít quan trọng nhất, nhưng bước đếm độ dài chính xác sẽ bỏ qua nó bất cứ khi nào vị trí đó trở thành chữ số có nghĩa nhất. 

Hai mảng còn lại là trạng thái DP trung tâm. Các mảng được tạo lại ở mọi vị trí vì mỗi lần chuyển đổi chỉ phụ thuộc vào vị trí ngay trước đó. Không có lịch sử nào vượt quá phần còn lại hiện tại và khoản vay là cần thiết. 

bản cập nhật`j = r + add`theo sau là một phép trừ của`x`tránh hoạt động modulo tương đối tốn kém bên trong vòng lặp trong cùng. Vì cả hai`r`Và`add`đang ở trong`[0,x-1]`, tổng của chúng nhỏ hơn`2x`, vì vậy một phép trừ là đủ. 

Số lượng độ dài chính xác tách biệt với quá trình chuyển đổi vì số 0 là hợp pháp trong nội bộ nhưng không hợp lệ khi là chữ số hàng đầu. Đối với chữ số ứng viên khác 0`d`, số dư duy nhất trước đó tạo ra số dư cuối cùng bằng 0 là`(-d * power) % x`. Điều này tránh việc quét toàn bộ mảng phần còn lại lần thứ hai chỉ để xác định có bao nhiêu chữ số đầu hợp lệ tạo ra phần còn lại bằng 0. 

Quá trình chuyển đổi khoản vay đáng được quan tâm đặc biệt. Vì`d < bd`, ngay cả khoản vay hiện tại cũng được hấp thụ vì`bd - d - 1`vẫn không âm. Vì`d == bd`, khoản vay hiện tại vẫn tồn tại. Vì`d > bd`, cả hai trường hợp đều cần phải vay. Việc trộn lẫn ba trường hợp này rất có thể là nguyên nhân dẫn đến câu trả lời sai khi triển khai từ phải sang trái. 

Số nguyên Python không bị tràn, nhưng tất cả số DP đều giảm theo modulo (10^9+7) sau mỗi lần cộng. Việc giảm thập phân được thực hiện trực tiếp trên chuỗi, điều này là cần thiết vì (A) có thể có 101 chữ số và không cần phải khớp với số nguyên có chiều rộng cố định. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
A = 1
B = 20
X = 2
S = 1234789
```Các trạng thái hữu ích từ phải sang trái cho giới hạn trên được tóm tắt dưới đây. 

| Vị trí | Chữ số bị ràng buộc | Chữ số được phép | Đóng góp chiều dài | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1,2,3,4,7,8,9 | 3 | Bội số có một chữ số của 2 là 2, 4, 8 | 
| 2 | 2 | 1,3,7,9 | 3 | Các bội số hợp lệ có hai chữ số là 12, 14, 18 | 

Ở vị trí 1, mọi chữ số đều nguyên tố cùng nhau với 1. Vì các số có một chữ số tự động ở dưới`20`, các số chia hết hợp lệ là`2`,`4`, Và`8`. 

Ở vị trí 2, chữ số có nghĩa lớn nhất phải nguyên tố cùng với 2 nên chỉ`1`,`3`,`7`, Và`9`được phép. Chữ số bị ràng buộc là 2. Các kết hợp tạo ra số dư bằng 0 và kết thúc không có khoản vay là`12`,`14`, Và`18`. Do đó, tổng số là 6, phù hợp với đầu ra mẫu. 

Đối với mẫu 2,```
A = 1
B = 20
X = 3
S = 0123678
```Dấu vết cũng tương tự. 

| Vị trí | Chữ số bị ràng buộc | Chữ số được phép | Đóng góp chiều dài | Số hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0,1,2,3,6,7,8 | 2 | 3, 6 | 
| 2 | 2 | 1,3,7 | 2 | 12, 18 | 

Ở vị trí 1, số 0 bị bỏ qua làm chữ số đứng đầu, để lại`3`Và`6`là bội số một chữ số của 3. 

Ở vị trí 2, chữ số thứ hai từ phải sang bị giới hạn bởi (\gcd(d,2)=1), do đó chỉ trong số các chữ số được cung cấp`1`,`3`, Và`7`là có thể. Các điều kiện ràng buộc và phần còn lại rời khỏi`12`Và`18`. Tổng số là 4, một lần nữa phù hợp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(LX | S | )) | Có (L) vị trí, (X) trạng thái còn lại và tối đa 10 chữ số ứng cử viên cho mỗi vị trí, với hai giới hạn | 
| Không gian | (O(X+L | S | )) | Hai mảng còn lại và các chữ số hợp lệ được tính toán trước | 

Độ dài giới hạn tối đa chỉ khoảng 101 chữ số, trong khi (X\le10^5). Thuật toán không bao giờ phụ thuộc vào độ lớn số của (A) hoặc (B), chỉ phụ thuộc vào độ dài thập phân của chúng. Việc sử dụng bộ nhớ vẫn tuyến tính theo (X), đây là yêu cầu chính cho các thử nghiệm lớn nhất. 

## Trường hợp thử nghiệm```python
# Save the editorial solution as solution.py before running these tests.

from solution import solve_case

# Provided samples
assert solve_case(
    "1", "20", 2, "1234789"
) == 6, "sample 1"

assert solve_case(
    "1", "20", 3, "0123678"
) == 4, "sample 2"

# Minimum-size input.
assert solve_case(
    "1", "1", 1, "1"
) == 1, "single valid number"

# Single boundary value that is not divisible by X.
assert solve_case(
    "11", "11", 2, "12"
) == 0, "exact boundary but wrong remainder"

# Exact boundary value that is valid.
assert solve_case(
    "12", "12", 2, "12"
) == 1, "exact boundary"

# All-equal digit set. Valid numbers are 7 and 77.
assert solve_case(
    "1", "100", 7, "7"
) == 2, "all-equal digits"

# Maximum-size decimal bound.
# With only digit 1 available, every repunit from 1 to 101 digits is valid,
# and X = 1 makes every one divisible.
big_b = "1" + "0" * 100
assert solve_case(
    "1", big_b, 1, "1"
) == 101, "maximum-size bound"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1`|`1`| Khoảng thời gian tối thiểu và số hợp lệ duy nhất | 
|`11 11 2 / 12`|`0`| Ranh giới trên và dưới chính xác không chia hết được | 
|`12 12 2 / 12`|`1`| Ranh giới chính xác với mọi điều kiện được thỏa mãn | 
|`1 100 7 / 7`|`2`| Tính đồng nguyên tố vị trí và chữ số lặp lại | 
|`1 10^100 1 / 1`|`101`| Độ dài chữ số tối đa và giới hạn chính xác tùy ý | 

## Vỏ cạnh 

Số 0 đứng đầu được xử lý bằng bước đếm độ dài chính xác thay vì cấm số 0 trên toàn cầu. Ví dụ,```
1 9 1
0
```có câu trả lời`0`. Số 0 là hợp pháp ở vị trí 1 theo quy tắc gcd, nhưng nó không thể là chữ số đứng đầu của số nguyên dương, vì vậy ứng cử viên không bao giờ được tính là số có một chữ số. 

Số 0 vẫn có thể là chữ số bên trong hợp lệ khi vị trí của nó cách bên phải 1. Đây là lý do tại sao số 0 phải được giữ nguyên trong tập chuyển tiếp. Ví dụ: nếu các chữ số được phép là`01`, số`10`có chữ số 0 ngoài cùng bên phải và (\gcd(0,1)=1), trong khi chữ số hàng đầu của nó thỏa mãn (\gcd(1,2)=1). Thuật toán cho phép số 0 trong quá trình chuyển đổi đầu tiên và chỉ từ chối nó nếu nó được sử dụng làm chữ số có nghĩa nhất. 

Các số ngắn hơn giới hạn yêu cầu xử lý đặc biệt khi so sánh từ phải sang trái. Vì`B = 20`, số có một chữ số`8`gây ra khoản vay khi so sánh chữ số duy nhất của nó với chữ số thấp`0`của`20`. DP giữ trạng thái mượn đó thay vì từ chối số đó. Vì số được xây dựng chỉ có một chữ số trong khi giới hạn có hai chữ số nên cả hai trạng thái vay cuối cùng đều được chấp nhận cho khoản đóng góp một chữ số. 

Điều kiện gcd vị trí phải sử dụng khoảng cách từ bên phải. Vì`12`, chữ số ngoài cùng bên phải ở vị trí 1 và chữ số bên trái ở vị trí 2. Cả hai đều thỏa mãn điều kiện vì (\gcd(2,1)=1) và (\gcd(1,2)=1). DP xử lý chính xác theo thứ tự đó, do đó vị trí được sử dụng để xây dựng bộ chữ số được phép luôn là vị trí chính xác. 

Cuối cùng, khoảng được lấy là`count_leq(B) - count_leq(A-1)`. Điều này xử lý cả hai điểm cuối mà không có trường hợp đặc biệt nào trong DP chính. Vì`A = B = 12`, số đếm đầu tiên bao gồm`12`và lần đếm thứ hai dừng lại ở`11`, để lại đúng một số. Vì`A = B = 11`với`X = 2`, cả hai phép tính đều đồng ý rằng không có số chia hết hợp lệ nên kết quả bằng 0.
