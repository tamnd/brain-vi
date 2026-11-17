---
title: "CF 1977C - Nikita và LCM"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Một dãy con được gọi là đặc biệt nếu bội số chung nhỏ nhất của tất cả các phần tử của nó không xuất hiện ở bất kỳ đâu trong mảng ban đầu. Nhiệm vụ là tìm độ dài tối đa có thể có của dãy con như vậy."
date: "2026-06-08T17:16:07+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1977
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 948 (Div. 2)"
rating: 1900
weight: 1977
solve_time_s: 152
verified: true
draft: false
---

[CF 1977C - Nikita và LCM](https://codeforces.com/problemset/problem/1977/C) 

**Xếp hạng:** 1900 
**Tags:** sức mạnh vũ phu, cấu trúc dữ liệu, dp, tham lam, toán học, lý thuyết số, sắp xếp 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Một dãy con được gọi là đặc biệt nếu bội số chung nhỏ nhất của tất cả các phần tử của nó không xuất hiện ở bất kỳ đâu trong mảng ban đầu. 

Nhiệm vụ là tìm độ dài tối đa có thể có của dãy con như vậy. 

Thứ tự của các phần tử bên trong một dãy con không quan trọng đối với LCM. Chỉ những giá trị được chọn mới quan trọng. Vì mọi phần tử của mảng có thể được lấy đi hoặc bị bỏ qua nên có rất nhiều dãy con theo cấp số nhân, vì vậy chúng ta cần khai thác cấu trúc lý thuyết số thay vì liệt kê chúng. 

Độ dài mảng có thể lớn tới 2000 và tổng của tất cả các độ dài trong tất cả các trường hợp thử nghiệm cũng nhiều nhất là 2000. Đây là một ràng buộc rất bất thường. Điều đó có nghĩa là chúng ta có thể thực hiện được phép tính bậc hai đại khái cho mỗi trường hợp thử nghiệm, nhưng bất cứ điều gì theo cấp số nhân đều là không thể. Thậm chí$O(n^3)$sẽ không thoải mái trong trường hợp xấu nhất. 

Bản thân các giá trị có thể lớn bằng$10^9$, quy định việc lập trình động trên các giá trị. Mọi giải pháp đều phải làm việc trực tiếp với ước số và LCM thay vì xây dựng các bảng lớn được lập chỉ mục theo giá trị. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
1
5
1 2 4 8 16
```Mọi dãy con không trống đều có LCM là một trong các phần tử mảng. Dãy con trống có LCM$0$, nhưng độ dài của nó bằng không. Câu trả lời đúng là 0. 

Một lỗi phổ biến là cho rằng nếu LCM của toàn bộ mảng thuộc về mảng thì không có câu trả lời nào tồn tại. Điều đó là sai. 

Ví dụ:```
1
4
2 3 6 12
```LCM của toàn bộ mảng là 12, hiện diện. Tuy nhiên trình tự tiếp theo$[2,3]$có LCM 6, cũng có mặt, trong khi$[2,12]$có LCM 12. Chúng ta phải tìm kiếm cẩn thận hơn nhiều so với việc chỉ kiểm tra LCM toàn mảng. 

Một trường hợp tế nhị khác là:```
1
3
2 3 5
```LCM của toàn bộ mảng là 30, không có. Sau đó lấy tất cả các phần tử ngay lập tức sẽ cho một dãy con đặc biệt có độ dài bằng 3. Thiếu quan sát này sẽ dẫn đến công việc không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Liệt kê tất cả các chuỗi con, tính LCM của chúng và giữ độ dài lớn nhất mà LCM không xuất hiện trong mảng. 

Điều này đúng vì nó trực tiếp kiểm tra định nghĩa. Thật không may, nó đòi hỏi phải kiểm tra$2^n$các chuỗi tiếp theo. Ngay cả đối với$n=50$điều này đã là không thể, và đây$n$có thể đạt tới 2000. 

Quan sát quan trọng xuất phát từ việc xem xét những gì LCM thực sự có thể xảy ra. 

Cho phép$M$là LCM của toàn bộ mảng. 

Mọi dãy con LCM đều phải chia$M$. Nó không bao giờ có thể chứa số nguyên tố lớn hơn số đã xuất hiện ở đâu đó trong mảng. 

Bây giờ hãy xem xét hai trường hợp. 

Nếu như$M$bản thân nó không xuất hiện trong mảng thì toàn bộ mảng đó đã đặc biệt rồi. LCM của nó bằng$M$, không có trong mảng. Câu trả lời là ngay lập tức$n$. 

Trường hợp thú vị là khi$M$không xuất hiện trong mảng. 

Khi đó mọi dãy con đặc biệt phải có LCM là ước số thực sự của$M$. Hơn nữa, nếu một giá trị không chia$M$, nó không thể là LCM của bất kỳ dãy con nào. 

Thực tế quan trọng là mọi ứng cử viên LCM đặc biệt phải là ước số của$M$đó không xuất hiện trong mảng. 

Số ước của một số lên đến$10^9$là nhỏ. Trên thực tế, ngay cả số ước tối đa có thể cũng chỉ khoảng một nghìn. Điều này làm cho việc liệt kê tất cả các ước của$M$. 

Đối với ước số ứng cử viên cố định$d$, dãy con lớn nhất có LCM bằng$d$? 

Mỗi phần tử được chọn phải chia$d$. Bất kỳ phần tử nào không chia$d$sẽ buộc LCM vượt quá$d$. 

Trong số tất cả các yếu tố phân chia$d$, chúng tôi chỉ cần lấy tất cả chúng. LCM kết hợp của họ có thể được tính toán. Nếu LCM đó bằng$d$, thì mọi phần tử như vậy có thể được đưa vào, mang lại độ dài tối đa có thể cho ứng cử viên này. 

Chúng tôi lặp lại điều này cho mọi ước số$d$của$M$không có trong mảng và lấy số lượng hợp lệ lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \cdot \tau(M))$|$O(\tau(M))$| Đã chấp nhận | 

Đây$\tau(M)$biểu thị số ước của$M$. 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính LCM của tất cả các phần tử. 
2. Trong khi tính toán LCM, hãy ngừng phát triển nó khi nó vượt quá giá trị mảng tối đa. 

Điều này quan trọng vì LCM toàn mảng thực tế có thể trở nên khổng lồ. Chúng ta chỉ cần biết liệu nó có lớn hơn mọi phần tử mảng hay không. 
3. Hãy để`mx`là giá trị mảng tối đa. 
4. Nếu LCM toàn mảng lớn hơn`mx`, thì nó không thể thuộc về mảng. 

Trong tình huống đó toàn bộ mảng là đặc biệt, vì vậy đầu ra`n`. 
5. Mặt khác, LCM toàn mảng bằng một giá trị mảng nào đó. Gọi giá trị này`M`. 
6. Liệt kê mọi ước của`M`. 
7. Với mỗi ước số`d`không xuất hiện trong mảng: 

1. Thu thập tất cả các phần tử mảng đang chia`d`. 
2. Đếm chúng. 
3. Tính LCM của tất cả các phần tử được thu thập. 
4. Nếu LCM thu được bằng`d`, thì ước số này có thể được coi là dãy LCM con. 
5. Cập nhật câu trả lời bằng số đếm. 
8. Xuất ra số lớn nhất tìm được. 

### Tại sao nó hoạt động 

Mọi dãy con LCM đều chia LCM toàn mảng$M$. Nếu như$M$bản thân nó không có trong mảng, việc lấy mọi phần tử đã cho một dãy con đặc biệt có độ dài tối đa có thể. 

Nếu không thì$M$thuộc về mảng. Bất kỳ dãy con đặc biệt nào cũng phải có LCM bằng ước số nào đó$d$của$M$đó không xuất hiện trong mảng. 

Đối với số chia cố định$d$, mọi phần tử được chọn phải chia$d$. Việc bao gồm tất cả các phần tử như vậy sẽ mang lại dãy con lớn nhất có thể có được$d$. Nếu LCM của tất cả các yếu tố đủ điều kiện đó bằng$d$, sau đó$d$là có thể đạt được. Nếu không, không có tập hợp con nào trong số chúng có thể tạo ra$d$, bởi vì việc loại bỏ các phần tử chỉ có thể làm giảm lũy thừa nguyên tố sẵn có. 

Do đó, mỗi LCM đặc biệt khả thi đều được kiểm tra chính xác một lần và với mỗi LCM chúng tôi tính toán độ dài tối đa có thể đạt được. Độ dài lớn nhất như vậy là câu trả lời. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def lcm(a, b):
    return a // gcd(a, b) * b

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        mx = max(a)
        s = set(a)

        cur_lcm = 1
        overflow = False

        for x in a:
            cur_lcm = lcm(cur_lcm, x)
            if cur_lcm > mx:
                overflow = True
                break

        if overflow:
            ans.append(str(n))
            continue

        M = cur_lcm

        divisors = []
        d = 1
        while d * d <= M:
            if M % d == 0:
                divisors.append(d)
                if d * d != M:
                    divisors.append(M // d)
            d += 1

        best = 0

        for target in divisors:
            if target in s:
                continue

            cnt = 0
            cur = 1

            for x in a:
                if target % x == 0:
                    cnt += 1
                    cur = lcm(cur, x)
                    if cur > target:
                        break

            if cur == target:
                best = max(best, cnt)

        ans.append(str(best))

    sys.stdout.write("\n".join(ans))

solve()
```Phần đầu tiên tính toán LCM của toàn bộ mảng. Chúng tôi không bao giờ cho phép nó phát triển một cách không cần thiết. Ngay khi nó vượt quá phần tử mảng tối đa, chúng ta đã biết nó không thể xuất hiện trong mảng nên câu trả lời trở thành`n`. 

Giai đoạn tiếp theo chỉ chạy khi LCM toàn mảng bằng giá trị mảng. Trong trường hợp đó, chúng tôi liệt kê tất cả các ước của LCM đó. 

Đối với mỗi ước số ứng cử viên, chúng tôi kiểm tra chính xác các phần tử mảng chia nó. Bất kỳ phần tử nào không chia ứng viên sẽ không bao giờ thuộc về một dãy con có LCM bằng ứng cử viên đó. 

Việc tính toán LCM sử dụng```
a // gcd(a, b) * b
```còn hơn là`a * b // gcd(a, b)`để tránh tràn trung gian trong các ngôn ngữ có số nguyên kích thước cố định. Python không tràn nhưng đây vẫn là cách triển khai lập trình cạnh tranh tiêu chuẩn. 

Một điểm tinh tế là chúng ta bỏ qua mọi ước số đã có trong mảng. Sự cố yêu cầu LCM thu được không có trong mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 4 8 16
```LCM toàn mảng là 16. 

| Bước | Giá trị | 
| --- | --- | 
| Phần tử tối đa | 16 | 
| LCM toàn mảng | 16 | 
| LCM hiện diện trong mảng? | Có | 

Bây giờ chúng ta kiểm tra các ước số không có trong mảng. 

| Ứng viên | Có mặt trong mảng? | Có thể đạt được? | 
| --- | --- | --- | 
| 1 | Có | Bỏ qua | 
| 2 | Có | Bỏ qua | 
| 4 | Có | Bỏ qua | 
| 8 | Có | Bỏ qua | 
| 16 | Có | Bỏ qua | 

Không tồn tại LCM đặc biệt hợp lệ. 

Trả lời:```
0
```Ví dụ này cho thấy rằng việc có nhiều ước số không giúp ích gì khi mọi LCM có thể đều đã xuất hiện trong mảng. 

### Ví dụ 2 

đầu vào:```
6
3 2 10 20 60 1
```| Bước | Giá trị | 
| --- | --- | 
| Phần tử tối đa | 60 | 
| LCM toàn mảng | 60 | 
| LCM hiện diện trong mảng? | Có | 

Xét ước số 30. 

| Yếu tố | Chia 30? | 
| --- | --- | 
| 3 | Có | 
| 2 | Có | 
| 10 | Có | 
| 20 | Không | 
| 60 | Không | 
| 1 | Có | 

Các phần tử được chọn là:```
3 2 10 1
```LCM của họ là 30. 

Vì 30 không xuất hiện trong mảng nên chúng ta thu được một dãy con đặc biệt có độ dài 4. 

Không có ứng cử viên hợp lệ lớn hơn tồn tại. 

Trả lời:```
4
```This example demonstrates why we must inspect divisors of the global LCM rather than only the full-array LCM itself.

 ## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot \tau(M))$| Mỗi ước số được kiểm tra dựa trên tất cả các phần tử mảng | 
| Không gian |$O(\tau(M))$| Lưu trữ cho ước số | 

Tổng của tất cả các độ dài mảng tối đa là 2000. Số ước của một số nguyên lên tới$10^9$là nhỏ, dưới vài nghìn. Khối lượng công việc kết quả là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    from math import gcd

    def lcm(a, b):
        return a // gcd(a, b) * b

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        mx = max(a)
        s = set(a)

        cur = 1
        overflow = False

        for x in a:
            cur = lcm(cur, x)
            if cur > mx:
                overflow = True
                break

        if overflow:
            out.append(str(n))
            continue

        M = cur

        divs = []
        d = 1
        while d * d <= M:
            if M % d == 0:
                divs.append(d)
                if d * d != M:
                    divs.append(M // d)
            d += 1

        best = 0

        for target in divs:
            if target in s:
                continue

            cnt = 0
            cur_l = 1

            for x in a:
                if target % x == 0:
                    cnt += 1
                    cur_l = lcm(cur_l, x)

            if cur_l == target:
                best = max(best, cnt)

        out.append(str(best))

    return "\n".join(out)

# provided samples
assert run("""6
5
1 2 4 8 16
6
3 2 10 20 60 1
7
2 3 4 6 12 100003 1200036
9
2 42 7 3 6 7 7 1 6
8
4 99 57 179 10203 2 11 40812
1
1
""") == "0\n4\n4\n5\n8\n0"

# custom cases
assert run("""1
3
2 3 5
""") == "3"

assert run("""1
1
7
""") == "0"

assert run("""1
4
2 2 2 2
""") == "0"

assert run("""1
3
2 4 8
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[2,3,5]`|`3`| Thiếu LCM mảng đầy đủ, đáp án là tất cả các phần tử | 
|`[7]`|`0`| Trường hợp ranh giới đơn phần tử | 
|`[2,2,2,2]`|`0`| Giá trị lặp lại | 
|`[2,4,8]`|`0`| Mọi LCM có thể đều đã có mặt | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
2 3 5
```LCM toàn mảng là 30, trong khi giá trị mảng tối đa là 5. Vì 30 vượt quá mọi phần tử mảng nên nó không thể xuất hiện. Thuật toán ngay lập tức trả về 3. Không cần liệt kê số chia. 

Bây giờ hãy xem xét:```
1
1
1
```LCM toàn mảng là 1, thuộc về mảng. Ước số duy nhất cũng là 1 và nó có trong mảng. Không có dãy con đặc biệt nào tồn tại nên đáp án là 0. 

Cuối cùng hãy xem xét:```
1
5
1 2 4 8 16
```LCM toàn mảng bằng 16 và thuộc về mảng. Mọi ước số của 16 cũng có mặt. Mọi dãy con LCM có thể có đều là một trong các ước số này, nên không có dãy nào có thể đặc biệt. Thuật toán kiểm tra tất cả các ước số, không tìm thấy mục tiêu hợp lệ và xuất ra chính xác 0.
