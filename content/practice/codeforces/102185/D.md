---
title: "CF 102185D - \u0415\u0432\u0440\u043e\u0432\u0438\u0434\u0435\u043d\u0438\u0435"
description: "Chúng ta cần xây dựng một bài hát có độ dài đúng T giây. Một điệp khúc có độ dài A và có thể lặp lại nhiều lần tùy ý."
date: "2026-08-19T06:28:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "D"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 223
verified: true
draft: false
---

[CF 102185D - \u0415\u0432\u0440\u043e\u0432\u0438\u0434\u0435\u043d\u0438\u0435](https://codeforces.com/problemset/problem/102185/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một bài hát chính xác`T`giây. Một điệp khúc có độ dài`A`và có thể lặp lại nhiều lần tùy ý. Mỗi đoạn kết hiện có có thể được sử dụng tối đa một lần, trong khi mỗi câu đều có độ dài`B`và đại diện cho một bản sao mới được viết, vì vậy nếu chúng ta sử dụng`k`câu, nhóm phải có khả năng viết ít nhất`k`những câu thơ. 

Giả sử chúng ta chọn một số tập con của`N`xen kẽ. Đặt tổng thời lượng của nó là`x`và kích thước của nó là`m`. Sau đó chúng tôi cần một số số`c`của những điệp khúc như vậy`x + kB + cA = T`. 

Giới hạn thứ tự cũng có thể được thể hiện bằng số. có`k`những câu thơ, vì vậy giữa những câu thơ liên tiếp chúng ta cần ít nhất`k - 1`các bộ phận khác. Những đoạn dạo đầu và điệp khúc được chọn chính là những phần có thể tách biệt các câu thơ. Vì vậy chúng ta cần`m + c >= k - 1`. 

Đầu vào chứa`T`,`A`, Và`B`, theo sau là tối đa 500 đoạn xen kẽ có thời lượng tối đa là 500. Mục tiêu`T`có thể lớn như`10^18`, do đó, một chương trình động được lập chỉ mục theo tổng thời lượng bài hát là không thể. Các tham số nhỏ hữu ích là`A`,`B`và số lượng xen kẽ. Từ`A <= 500`, một không gian trạng thái bao gồm các phần dư modulo`A`là đủ nhỏ. 

Ngoài ra còn có một ràng buộc cấu trúc hữu ích cho câu trả lời. Nếu một bài hát hợp lệ sử dụng ít nhất`A`câu thơ, loại bỏ chính xác`A`câu thơ và thêm`B`hợp xướng. Tổng thời lượng của chúng bằng nhau, bởi vì`A * B`giây được loại bỏ và`B * A`giây được thêm vào. Số lượng câu thơ giảm đi, trong khi số lượng dấu phân cách có sẵn lại tăng lên. Do đó bài hát kết quả vẫn hợp lệ. Do đó, một nghiệm tối thiểu luôn có ít hơn`A`những câu thơ. 

Điều này thay đổi việc tìm kiếm dường như không giới hạn về số lượng câu thơ thành tối đa 500 khả năng. 

Trường hợp cạnh đầu tiên không có phần xen kẽ. Ví dụ,```
10 3 10
0
```có câu trả lời`1`. Một câu đã kéo dài 10 giây nên nó tạo thành toàn bộ bài hát. Giải pháp nhất quyết có điệp khúc giữa các câu sẽ bác bỏ nó một cách không chính xác, vì chỉ có một câu và không cần tách biệt. 

Trường hợp cạnh thứ hai là không có câu thơ nào. Ví dụ,```
10 5 1
3
2 5 3
```có câu trả lời`0`, vì hai đoạn điệp khúc đã có tổng thời lượng là 10. Việc thực hiện bất cẩn mà bắt đầu kiểm tra từ một câu sẽ bỏ lỡ câu trả lời hợp lệ. 

Trường hợp cạnh thứ ba chính là điều kiện tách biệt. Trong mẫu đầu tiên, ba câu thơ chỉ có thể thực hiện được vì đoạn dạo đầu đã chọn và các đoạn điệp khúc cùng nhau cung cấp đủ dấu phân cách. Chỉ kiểm tra phương trình trong toàn bộ thời gian là không đủ, vì vẫn không thể sắp xếp được nghiệm số. 

Trường hợp cạnh thứ tư là mục tiêu có thể lớn hơn nhiều so với mọi giá trị DP. Các đoạn xen kẽ có tổng thời lượng tối đa`500 * 500 = 250000`, trong khi`T`có thể`10^18`. Chúng ta không bao giờ được phân bổ một mảng tỷ lệ thuận với`T`, và tất cả các phép tính liên quan đến`T`phải sử dụng số học số nguyên thay vì giả định về khoảng thời gian có kích thước máy bị giới hạn. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ liệt kê mọi tập hợp con của các phần xen kẽ. Đối với mỗi tập hợp con, chúng tôi biết thời lượng của nó`x`và số phần tử của nó`m`. Sau đó chúng ta có thể thử mọi số câu thơ có thể có từ`0`bởi vì`A - 1`, tính toán số lượng điệp khúc cần thiết, đồng thời kiểm tra thời lượng và điều kiện phân tách. 

Lực lượng vũ phu là chính xác bởi vì mọi bài hát hợp pháp đều xác định chính xác đoạn chuyển tiếp nào được sử dụng, bao nhiêu câu được sử dụng và bao nhiêu đoạn điệp khúc được sử dụng. Vấn đề là việc liệt kê tập hợp con. Với`N = 500`, có`2^500`tập hợp con và thậm chí chỉ thực hiện một lượng công việc không đổi cho mỗi tập hợp con đã vượt quá giới hạn. Đang cố gắng lên tới`A`số lượng câu làm cho phiên bản đơn giản trở nên đại khái`O(A * 2^N)`. 

Điều quan trọng là số lượng điệp khúc không bị hạn chế. Khi chúng ta biết tổng thời lượng của các đoạn xen kẽ đã chọn theo modulo`A`, phương trình`x + kB + cA = T`xác định giá trị có thể có của`k`modulo`A`. Vì câu trả lời tối thiểu luôn nhỏ hơn`A`, với mỗi dư lượng có nhiều nhất một ứng cử viên tối thiểu`k`. 

Vấn đề còn lại là số lượng đoạn chuyển tiếp được chọn, vì mỗi đoạn chuyển tiếp được chọn cũng là một dấu phân cách. Chúng tôi giải quyết điều đó bằng DP tổng tập hợp con bị chặn. Đối với mọi số lượng có thể của các đoạn xen kẽ được chọn và mọi modulo dư`A`, chúng tôi lưu trữ tổng thời lượng xen kẽ tối thiểu để hiện thực hóa trạng thái. 

Chúng ta không cần phân biệt số đếm lớn hơn hoặc bằng`A - 1`. Mỗi ứng viên`k`ở dưới`A`, do đó, điều kiện phân tách chỉ hỏi xem số lượng đoạn xen kẽ đã đạt đến ngưỡng nào đó dưới đây hay chưa`A`. Chúng tôi sử dụng ít nhất một trạng thái DP giới hạn cho tất cả các lần đếm`A - 1`. Điều này giữ DP ở mức`O(N A^2)`các trạng thái và sự chuyển tiếp. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(A * 2^N)`|`O(1)`bên cạnh trạng thái tập hợp con | Quá chậm | 
| DP tối ưu |`O(N A^2)`|`O(A^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`m`là số lượng các đoạn xen kẽ được chọn và`x`tổng thời lượng của chúng. Đối với một số câu thơ cố định`k`, bài hát phải thỏa mãn`x + kB + cA = T`. 

Việc sắp xếp có thể thực hiện được chính xác khi`m + c >= k - 1`. 
2. Chứng minh chỉ cần xét là đủ`0 <= k < A`. Nếu một bài hát hợp lệ có`k >= A`, di dời`A`câu thơ và thêm`B`hợp xướng. Cả hai hoạt động đều thay đổi thời lượng bằng cách`AB`, do đó thời lượng không thay đổi. Số dấu phân cách không giảm, số câu thơ giảm. Việc lặp lại phép biến đổi này cuối cùng sẽ tạo ra một bài hát hợp lệ với ít hơn`A`những câu thơ. 
3. Đối với mọi dư lượng có thể`r`modulo`A`, tìm số nhỏ nhất`k`TRONG`[0, A - 1]`thỏa mãn`kB ≡ T - r (mod A)`. 

Điều này có thể được thực hiện đơn giản bằng cách liệt kê nhiều nhất`A`các giá trị có thể có của`k`và ghi lại dư lượng mà mỗi cái tương ứng. Một số dư lượng có thể không có dung dịch khi`gcd(A, B)`không chia`T - r`. 
4. Xây dựng tập con DP. Cho phép`dp[m][r]`là tổng thời lượng tối thiểu của một tập hợp con chứa chính xác`m`xen kẽ và có thời lượng phù hợp với`r`modulo`A`. 

Chúng tôi chỉ lưu trữ số lượng chính xác dưới đây`A - 1`. Trạng thái cuối cùng,`m = A - 1`, có nghĩa là ít nhất`A - 1`các đoạn xen kẽ đã chọn. 

Khi xử lý một khoảng thời gian xen kẽ`s`, chuyển từ`m`ĐẾN`m + 1`và từ dư lượng`r`ĐẾN`(r + s) % A`. Thứ nguyên đếm được xử lý ngược để mỗi đoạn xen kẽ được sử dụng nhiều nhất một lần. 
5. Đối với mỗi dư lượng`r`, có được ứng cử viên của nó`k`. Nếu không có ứng cử viên, phần dư này không thể tạo ra một bài hát có số câu thơ tối thiểu có thể. 
6. Đối với mỗi trạng thái DP đại diện`m`các đoạn xen kẽ đã chọn, tính số giây tối thiểu mà các đoạn xen kẽ cộng với các đoạn điệp khúc bị ép buộc bởi điều kiện phân tách. 

Nếu như`m >= k - 1`, không có điệp khúc bổ sung nào bị ép buộc, vì vậy thời lượng không phải câu thơ được yêu cầu chỉ đơn giản là`x`. 

Nếu như`m < k - 1`, ít nhất`k - m - 1`cần phải có điệp khúc nên thời lượng không phải câu thơ là`x + A * (k - m - 1)`. 
7. Hãy để thời lượng phi thơ tối thiểu được yêu cầu này là`need`. Thời gian còn lại sau các câu thơ là`T - kB`. Ứng viên khả thi chính xác khi`need <= T - kB`. 

Sự khác biệt còn lại khi đó là bội số không âm của`A`, vì vậy nó có thể chứa đầy những điệp khúc bổ sung. 
8. Lấy điều nhỏ nhất khả thi`k`trên tất cả các dư lượng. 

Tại sao nó hoạt động: DP chứa mọi tập hợp con có thể có của các đoạn xen kẽ, được nhóm theo modulo số lượng và thời lượng của nó`A`và lưu trữ khoảng thời gian nhỏ nhất cho mỗi trạng thái đó. Đối với một dư lượng cố định, sự đồng đẳng xác định số lượng câu thơ nhỏ nhất có thể. Bất kỳ số lượng câu lớn hơn là không cần thiết cho một giải pháp tối thiểu. Kiểm tra DP sau đó sẽ thêm chính xác số lượng điệp khúc tối thiểu cần thiết để tách các câu. Nếu thời lượng kết quả phù hợp với bên trong`T`, thời gian không sử dụng có thể chia cho`A`và có thể chứa đầy những điệp khúc bổ sung tùy ý. Ngược lại, mỗi bài hát hợp lệ tương ứng với một trạng thái DP và một dư lượng ứng cử viên, do đó không thể bỏ qua giải pháp tối thiểu hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve_case(T, A, B, S):
    n = len(S)

    # A minimum answer is always < A.
    # We only need exact counts below A - 1.
    # The last state represents all counts >= A - 1.
    K = min(n, A - 1)

    dp = [[INF] * A for _ in range(K + 1)]
    dp[0][0] = 0

    for s in S:
        # We only transition from exact states m < K.
        # The state K is already "at least K", and adding the
        # current item cannot improve its minimum sum.
        for m in range(K - 1, -1, -1):
            cur = dp[m]
            nxt = dp[m + 1]

            for r in range(A):
                x = cur[r]
                if x == INF:
                    continue

                nr = (r + s) % A
                nx = x + s
                if nx < nxt[nr]:
                    nxt[nr] = nx

    # candidate[r] = smallest k in [0, A - 1] satisfying
    # k * B == T - r (mod A).
    candidate = [-1] * A

    for k in range(A):
        r = (T - k * B) % A
        if candidate[r] == -1:
            candidate[r] = k

    answer = A

    for r in range(A):
        k = candidate[r]
        if k == -1:
            continue

        verse_time = k * B
        if verse_time > T:
            continue

        budget = T - verse_time

        for m in range(K + 1):
            x = dp[m][r]
            if x == INF:
                continue

            if m < k - 1:
                x += A * (k - m - 1)

            if x <= budget:
                answer = min(answer, k)
                break

    return -1 if answer == A else answer

def solve():
    T, A, B = map(int, input().split())
    N = int(input())

    if N:
        S = list(map(int, input().split()))
    else:
        S = []

    print(solve_case(T, A, B, S))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của bộ triển khai`K = min(N, A - 1)`. DP không cần số lượng lớn hơn`A - 1`, bởi vì mỗi số câu ứng cử viên đều nhỏ hơn`A`. Trạng thái cuối cùng đại diện cho tất cả các tập con có ít nhất`A - 1`các phần tử. 

Quá trình chuyển đổi lặp lại`m`ngược lại. Đây là cách tiêu chuẩn để thực hiện chuyển đổi tập hợp con bằng 0 hoặc một. Nhà nước`K`được cố tình không được sử dụng như một nguồn chuyển tiếp. Vì nó đã đại diện cho mọi số lượng đủ lớn nên việc thêm một đoạn xen kẽ khác chỉ có thể tăng thời lượng trong khi vẫn giữ trạng thái ở cùng loại đủ điều kiện. Nó không thể cải thiện thời lượng tối thiểu được lưu trữ ở đó. 

các`candidate`mảng tránh giải phương trình mô-đun riêng biệt cho từng phần dư. Chúng tôi liệt kê mọi thứ có thể`k`một lần và tính số dư`r = (T - kB) mod A`. 

Nếu hai giá trị của`k`tạo ra cùng một dư lượng, thì dư lượng nhỏ hơn sẽ là dư lượng duy nhất có liên quan, bởi vì nhiệm vụ yêu cầu số lượng câu thơ tối thiểu. 

Vòng lặp cuối cùng kiểm tra mọi dư lượng và mọi số lượng DP. Khi`m < k - 1`, những dấu phân cách còn thiếu phải được cung cấp bởi các dàn hợp xướng, điều này góp phần`A * (k - m - 1)`giây. Khi`m >= k - 1`, không có điệp khúc nào bị ép buộc bởi điều kiện đặt hàng. 

Tất cả các phép tính thời lượng đều sử dụng số nguyên Python, vì vậy`10^18`ràng buộc vào`T`không gây tràn. Bản thân DP chỉ chứa các giá trị tối đa bằng tổng thời lượng xen kẽ, nhiều nhất là`250000`, cộng với các hiệu chỉnh dấu tách nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
100 11 20
3
13 7 24
```Ứng cử viên hữu ích là tập hợp con chỉ chứa khoảng thời gian xen kẽ`7`. Modulo dư lượng của nó`11`là`7`. Đối với phần dư này,`3 * 20 ≡ 100 - 7 (mod 11)`, 

vì vậy ứng cử viên tối thiểu là`k = 3`. 

Trạng thái DP có liên quan và kiểm tra cuối cùng là: 

|`m`|`r`|`x`|`k`| điệp khúc bắt buộc | Thời gian không có câu bắt buộc | Ngân sách | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 7 | 3 |`3 - 1 - 1 = 1`|`7 + 11 = 18`|`100 - 60 = 40`| 

22 giây còn lại không được sử dụng để tính toán điệp khúc bắt buộc vì phương trình được xử lý modulo`A`. Sau đoạn dạo đầu đã chọn và ba câu thơ, 33 giây có thể được chiếm bởi ba đoạn điệp khúc, mang lại`7 + 3 * 20 + 3 * 11 = 100`. 

Như vậy câu trả lời là`3`. 

### Mẫu 2 

Đầu vào là```
10 5 1
3
2 5 3
```Đối với câu 0, tập con trống có phần dư`0`, Và`T = 10`chia hết cho`A = 5`. DP bắt đầu với`dp[0][0] = 0`. 

Ứng viên là`k = 0`và vì không có câu thơ nên không cần có dấu phân cách. 

|`m`|`r`|`x`|`k`| điệp khúc bắt buộc | Thời gian không có câu bắt buộc | Ngân sách | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 0 | 10 | 

10 giây còn lại tràn ngập hai đoạn điệp khúc. Vậy số câu tối thiểu là`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N A^2)`| có`N`xen kẽ,`O(A)`đếm trạng thái và`O(A)`dư lượng | 
| Không gian |`O(A^2)`| DP có nhiều nhất`A`đếm trạng thái và`A`dư lượng | 

Đây`N <= 500`Và`A <= 500`, do đó giới hạn lý thuyết nhiều nhất là khoảng 125 triệu kiểm tra trạng thái DP đơn giản. Mức tiêu thụ bộ nhớ chỉ khoảng 250000 trạng thái nguyên. Quan trọng nhất là thuật toán không bao giờ phụ thuộc vào`T`như một kích thước DP, điều này là cần thiết bởi vì`T`có thể đạt được`10^18`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_case(T, A, B, S):
    INF = 10**30
    n = len(S)

    K = min(n, A - 1)

    dp = [[INF] * A for _ in range(K + 1)]
    dp[0][0] = 0

    for s in S:
        for m in range(K - 1, -1, -1):
            cur = dp[m]
            nxt = dp[m + 1]

            for r in range(A):
                x = cur[r]
                if x == INF:
                    continue

                nr = (r + s) % A
                nx = x + s
                if nx < nxt[nr]:
                    nxt[nr] = nx

    candidate = [-1] * A

    for k in range(A):
        r = (T - k * B) % A
        if candidate[r] == -1:
            candidate[r] = k

    answer = A

    for r in range(A):
        k = candidate[r]
        if k == -1:
            continue

        budget = T - k * B
        if budget < 0:
            continue

        for m in range(K + 1):
            x = dp[m][r]
            if x == INF:
                continue

            if m < k - 1:
                x += A * (k - m - 1)

            if x <= budget:
                answer = min(answer, k)
                break

    return -1 if answer == A else answer

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    try:
        T, A, B = map(int, input().split())
        N = int(input())
        S = list(map(int, input().split())) if N else []
        return str(solve_case(T, A, B, S))
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("""100 11 20
3
13 7 24
""") == "3", "sample 1"

assert run("""10 5 1
3
2 5 3
""") == "0", "sample 2"

assert run("""8 9 2
2
1 2
""") == "-1", "sample 3"

assert run("""10 3 10
0
""") == "1", "sample 4"

# Minimum-size input.
assert run("""1 1 1
0
""") == "0", "minimum-size case"

# No interludes, with the verse being the only possible construction.
assert run("""10 3 10
0
""") == "1", "single verse boundary case"

# All interludes equal to the chorus length.
# The empty subset already works because T is divisible by A.
assert run("""6 3 2
4
3 3 3 3
""") == "0", "all-equal interludes"

# Maximum-size N and very large T.
large_interludes = " ".join(["500"] * 500)
assert run(f"""1000000000000000000 500 499
500
{large_interludes}
""") == "0", "maximum-size and large-T case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`,`N=0`|`0`| Giá trị tối thiểu và trường hợp không câu | 
|`10 3 10`,`N=0`|`1`| Một câu thơ có thể tạo thành toàn bộ bài hát | 
|`6 3 2`, bốn đoạn chuyển tiếp của`3`|`0`| Tất cả các đoạn xen kẽ bằng thời lượng điệp khúc | 
|`T=10^18`,`A=500`,`B=499`, 500 xen kẽ của`500`|`0`| Tối đa`N`, thời lượng tối đa và rất lớn`T`| 

## Vỏ cạnh 

Trường hợp zero-verse được xử lý trực tiếp bằng cách cho phép`k = 0`. Trong mẫu thứ hai, tập hợp con trống có phần dư bằng 0 và`T`là bội số của`A`, do đó DP tìm thấy`dp[0][0] = 0`và chấp nhận ngay lập tức. 

Khi có đúng một câu thơ thì yêu cầu phân cách là`k - 1 = 0`. Vì vậy, không cần điệp khúc hoặc đoạn dạo đầu giữa các câu thơ. đầu vào```
10 3 10
0
```có`k = 1`,`c = 0`và tổng thời lượng là 10, vì vậy câu trả lời là`1`. 

Khi`A = 1`, mọi khoảng thời gian đều có dư lượng bằng 0 modulo`A`. Ứng viên`k = 0`luôn tồn tại và bất kỳ khoảng thời gian mục tiêu nào cũng có thể được lấp đầy hoàn toàn bằng các đoạn điệp khúc. DP cũng vẫn có giá trị vì chiều dư của nó có kích thước bằng một. 

Khi`N = 0`, DP chỉ chứa tập con trống. Điều này là đủ vì phần điệp khúc là không giới hạn, vì vậy tất cả thời lượng còn lại phải do các điệp khúc cung cấp và câu hỏi duy nhất là liệu số câu có đúng modulo dư hay không.`A`. 

Điều kiện phân tách được xử lý riêng biệt với phương trình mô-đun. Một tập hợp con có thể có dư lượng thời lượng chính xác phù hợp trong khi chứa quá ít đoạn xen kẽ để phân tách tất cả các câu. biểu hiện`A * (k - m - 1)`thêm chính xác các dấu phân cách điệp khúc bị thiếu, ngăn không cho tập hợp con đó được chấp nhận không chính xác. 

Giá trị to lớn của`T`không ảnh hưởng đến kích thước DP. Ví dụ, với`T = 10^18`, tất cả các tổng xen kẽ vẫn bị giới hạn bởi`250000`. DP chỉ xử lý những khoản tiền nhỏ đó, trong khi`T`được sử dụng trong việc kiểm tra số học cuối cùng.
