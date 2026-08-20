---
title: "CF 102185H - ĐỊA PHƯƠNG++"
description: "Nhật ký là một chuỗi các nhóm chữ số được phân tách bằng dấu cách. Mỗi số nguyên không âm ban đầu được in bằng cách lấy biểu diễn thập phân thông thường của nó và chèn dấu phân cách cứ ba chữ số từ bên phải."
date: "2026-08-19T15:41:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "H"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 160
verified: true
draft: false
---

[CF 102185H - LOCALC++](https://codeforces.com/problemset/problem/102185/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 40 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhật ký là một chuỗi các nhóm chữ số được phân tách bằng dấu cách. Mỗi số nguyên không âm ban đầu được in bằng cách lấy biểu diễn thập phân thông thường của nó và chèn dấu phân cách cứ ba chữ số từ bên phải. Chúng ta phải khôi phục xem có bao nhiêu dãy số nguyên ban đầu khác nhau có thể tạo ra chính xác dãy nhóm này, với mỗi số nguyên nhỏ hơn (10^K). 

Đầu vào cung cấp (N) nhóm theo thứ tự chính xác và giới hạn chữ số (K). Chúng tôi không tự mình xây dựng lại các giá trị số. Chúng tôi đang đếm các cách có thể để đặt ranh giới giữa các nhóm liền kề sao cho mỗi khối kết quả là số nguyên thập phân được định dạng hợp lệ và chứa tối đa (K) chữ số. 

Khối đại diện cho một số nguyên có hình dạng rất cụ thể. Nhóm đầu tiên của nó có một, hai hoặc ba chữ số, ngoại trừ số 0 đứng đầu bị cấm và mọi nhóm tiếp theo phải có chính xác ba chữ số. Ngoại lệ duy nhất liên quan đến số 0 là số nguyên`0`, đại diện của nó bao gồm một nhóm duy nhất`0`. Một khối có nhóm đầu tiên như`003`là không thể, mặc dù`003`là hoàn toàn hợp lệ như một nhóm sau một nhóm khác. Tổng số chữ số trong khối cũng phải lớn nhất là (K), vì số nguyên hoàn toàn nhỏ hơn (10^K). 

Các ràng buộc loại trừ lập trình động bậc hai. Với (N=2\cdot10^5), vòng chuyển tiếp (O(N^2)) thực hiện khoảng (N^2/2=2\cdot10^{10}) kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn một giây. Giải pháp dự định phải xử lý mỗi nhóm đầu vào chỉ với số lần không đổi, đưa ra thuật toán (O(N)). 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Coi như```
1 3
0
```Có chính xác một dãy số nguyên có thể có, đó là số nguyên 0 duy nhất, vì vậy câu trả lời là`1`. điều trị`0`giống như nhóm đầu tiên thông thường có thể được theo sau bởi các nhóm ba chữ số sẽ cho phép biểu diễn không chính tắc một cách không chính xác. 

Cho phép các số 0 đứng đầu bên trong các nhóm tiếp tục. Ví dụ,```
2 4
1 000
```có câu trả lời`1`, vì hai nhóm tạo thành số nguyên`1000`. Một giải pháp bác bỏ mọi nhóm chứa các số 0 đứng đầu sẽ bác bỏ trường hợp này một cách không chính xác. 

Giới hạn chữ số là chính xác. Vì```
2 5
999 999
```hai nhóm không thể tạo thành một số vì số đó cần có sáu chữ số. Chúng phải là hai số nguyên riêng biệt, vì vậy câu trả lời là`1`. Với (K=6), cả số chia và số kết hợp đều có thể thực hiện được, đưa ra câu trả lời`2`. 

Một nhóm ngắn hơn ba chữ số không thể là một nhóm tiếp theo. Ví dụ,```
3 7
10 500 4
```chỉ có thể được chia thành`10 500`Và`4`, vậy câu trả lời là`1`. Quá trình chuyển đổi chỉ kiểm tra tổng số chữ số mà quên quy tắc tiếp tục ba chữ số sẽ đếm không chính xác một số chứa cả ba nhóm. 

## Phương pháp tiếp cận 

Giải pháp quy hoạch động trực tiếp bắt đầu bằng (dp[i]), số cách biểu diễn (i) nhóm đầu tiên. Đối với mọi vị trí (i), chúng ta có thể thử mọi vị trí bắt đầu có thể (j) của số nguyên cuối cùng. Sau đó chúng tôi kiểm tra xem các nhóm (j,\ldots,i) có tạo thành một đại diện hợp pháp hay không và liệu số chữ số của nó có nhiều nhất là (K) hay không. Nếu đúng như vậy, chúng tôi thêm (dp[j-1]) vào (dp[i]). 

Điều này đúng vì mọi biểu diễn của tiền tố đều có chính xác một số nguyên cuối cùng, do đó vị trí bắt đầu có thể có của số nguyên đó sẽ chia tất cả các nghiệm thành các trường hợp riêng biệt. Vấn đề là số lần chuyển tiếp. Có thể có (\Theta(N^2)) cặp ((j,i)) và đối với (N=2\cdot10^5) thì đó là khoảng (2\cdot10^{10}) kiểm tra. 

Cấu trúc của một số nguyên được định dạng mang lại cho chúng ta sự tối ưu hóa còn thiếu. Khi nhóm số nguyên đầu tiên đã được chọn, mọi nhóm sau phải có chính xác ba chữ số. Do đó, trong khi quét liên tiếp các nhóm ba chữ số, các vị trí bắt đầu có thể tạo thành một phạm vi liền kề. Giới hạn chữ số cũng giới hạn phạm vi này ở số lượng nhóm tối đa cố định. 

Có nhiều nhất một vị trí bắt đầu có thể có ít hơn ba chữ số trong một lần chạy như vậy. Đó là nhóm ngay trước khi chạy nhóm ba chữ số. Sự đóng góp của nó có thể được xử lý riêng biệt. Tất cả các khởi đầu có thể khác là các nhóm có ba chữ số và sự đóng góp của chúng tạo thành một khoảng trượt của các giá trị (dp[j-1]). Tổng tiền tố cho phép chúng ta thu được toàn bộ khoảng thời gian đó trong thời gian không đổi. 

DP vũ phu hoạt động vì mọi ranh giới có thể có trước đó đều được xem xét. Nó thất bại vì có quá nhiều ranh giới để kiểm tra. Quan sát cho thấy các phần tiếp theo hợp pháp buộc phải có ba chữ số sẽ biến nhiều chuyển đổi đó thành một tổng phạm vi, giảm toàn bộ tính toán về thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP vượt qua mọi ranh giới trước đó | (O(N^2)) | (O(N)) | Quá chậm | 
| Tổng tiền tố khi bắt đầu có ba chữ số hợp lệ | (O(N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định (dp[i]) là số dãy số nguyên ban đầu hợp lệ có đầu ra được định dạng chính xác là nhóm (i) đầu tiên. Đặt (dp[0]=1), đại diện cho tiền tố trống. 
2. Xử lý các nhóm từ trái sang phải. Nếu nhóm hiện tại có ít hơn ba chữ số, nó không thể tiếp tục số nguyên bắt đầu trước đó. Nó phải là biểu diễn hoàn chỉnh của một số nguyên mới, vì vậy (dp[i]) là (dp[i-1]) khi nhóm là biểu diễn thập phân độc lập hợp lệ hoặc bằng 0 nếu ngược lại. 
3. Hãy nhớ nhóm gần đây nhất có độ dài không bằng ba. Giữa vị trí đó và vị trí hiện tại, mỗi nhóm có ba chữ số. Nếu nhóm hiện tại có ba chữ số, bất kỳ số nguyên nào kết thúc ở đây đều bắt đầu ở một trong các nhóm có ba chữ số trong lần chạy này hoặc bắt đầu ở nhóm ngắn hơn được ghi nhớ đó. 
4. Đối với nhóm bắt đầu có ba chữ số ở vị trí (j), số nhóm từ (j) đến (i) là (i-j+1). Mỗi nhóm đóng góp ba chữ số, do đó số nguyên được phép chính xác khi 
[ 
3(i-j+1)\le K. 
] 
Do đó, số lần bắt đầu có ba chữ số hợp lệ thỏa mãn 
[ 
j\ge i-\left\lfloor\frac K3\right\rfloor+1. 
] 
Chỉ bắt đầu trong chuỗi ba chữ số liên tiếp hiện tại mới có liên quan. 
5. Nhóm ba chữ số chỉ có thể là nhóm đầu tiên khi ký tự đầu tiên của nó khác 0. Lưu trữ (dp[j-1]) cho mọi vị trí (j) như vậy trong tổng tiền tố. Sau đó, tổng của tất cả các lần bắt đầu có ba chữ số hợp lệ hiện tại sẽ được tính theo thời gian không đổi. 
6. Xử lý nhóm ngắn hơn ngay trước khi chạy riêng. Nếu độ dài của nó là (L<3), bắt đầu một số nguyên ở đó và kết thúc tại (i) yêu cầu 
[ 
L+3(i-j)\le K, 
] 
trong đó (j) là vị trí của nhóm ngắn hơn đó. Nhóm như vậy chỉ là nhóm đầu tiên hợp lệ khi nó là tiền tố thập phân độc lập hợp pháp và nó không phải là nhóm đầu tiên hợp lệ.`0`, bởi vì`0`không thể được theo sau bởi nhiều nhóm hơn. 
7. Cộng các khoản đóng góp từ số lần bắt đầu hợp lệ có ba chữ số và số lần bắt đầu ngắn hơn có thể có. Tổng kết quả là (dp[i]). Giảm nó theo modulo (10^9+7). 
8. Đầu ra (dp[N]), đếm mọi vị trí hợp lệ của các ranh giới số nguyên trên nhật ký hoàn chỉnh. 

Điều bất biến là sau khi xử lý vị trí (i), (dp[i]) sẽ tính chính xác các cách hợp lệ để phân vùng các nhóm (i) đầu tiên. Mọi số nguyên cuối cùng có thể được phân loại duy nhất theo nhóm đầu tiên của nó. Nếu nhóm đầu tiên đó có ba chữ số thì nó thuộc phạm vi ba chữ số được duy trì. Nếu không thì đó phải là nhóm ngắn hơn gần đây nhất. Tổng tiền tố bao gồm chính xác ba chữ số bắt đầu thỏa mãn cả quy tắc định dạng và giới hạn chữ số (K), trong khi kiểm tra nhóm ngắn hơn riêng biệt xử lý khả năng duy nhất còn lại. Do đó, không có phân vùng hợp lệ nào bị bỏ qua và không có phân vùng không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n, k = map(int, input().split())
    a = input().split()

    dp = [0] * (n + 1)

    # pref[i] = sum of dp[j - 1] for valid three-digit starts j <= i.
    pref = [0] * (n + 1)

    dp[0] = 1

    # Last position whose group has length different from 3.
    last_short = 0

    max_three_groups = k // 3

    for i in range(1, n + 1):
        s = a[i - 1]
        length = len(s)

        if length == 3:
            # This position may be the first group of an integer only
            # if it does not start with zero.
            if s[0] != '0':
                pref[i] = (pref[i - 1] + dp[i - 1]) % MOD
            else:
                pref[i] = pref[i - 1]

            # All three-digit starts must lie in the current run.
            left = max(last_short + 1, i - max_three_groups + 1)

            ways = (pref[i] - pref[left - 1]) % MOD

            # The group immediately before this run may itself be
            # the first group of the integer.
            if last_short:
                t = a[last_short - 1]
                tlen = len(t)

                # "0" can only represent the integer zero by itself.
                valid_as_first = t != "0" and t[0] != '0'

                if valid_as_first:
                    digits = tlen + 3 * (i - last_short)
                    if digits <= k:
                        ways += dp[last_short - 1]
                        ways %= MOD

            dp[i] = ways

        else:
            # A group shorter than three digits cannot continue a
            # previously started integer.
            if s == "0" or s[0] != '0':
                dp[i] = dp[i - 1]
            else:
                dp[i] = 0

            pref[i] = pref[i - 1]
            last_short = i

    print(dp[n] % MOD)

if __name__ == "__main__":
    solve()
```Mảng`dp`triển khai trạng thái lập trình động từ hướng dẫn. giá trị`dp[i-1]`đã được biết khi vị trí (i) được xử lý, do đó nó có thể được sử dụng ngay lập tức dưới dạng phần đóng góp của một số nguyên mới bắt đầu tại (i). 

các`pref`mảng chỉ lưu trữ các đóng góp từ các nhóm ba chữ số là nhóm đầu tiên hợp pháp. Tại vị trí (j), phần đóng góp là`dp[j - 1]`, bởi vì mọi thứ trước (j) đã được phân vùng và số nguyên mới bắt đầu tại (j). 

Giới hạn dưới`left`kết hợp hai hạn chế độc lập.`last_short + 1`ngăn cản thí sinh vượt qua nhóm ngắn hơn ba chữ số, trong khi`i - max_three_groups + 1`thực thi giới hạn chữ số (K) cho một số có nhóm đầu tiên cũng có ba chữ số. 

Nhóm ngắn hơn trước khi chạy ba chữ số hiện tại được kiểm tra riêng. Độ dài nhóm đầu tiên của nó có thể là một hoặc hai, do đó số lượng tối đa của các nhóm ba chữ số tiếp theo phụ thuộc vào độ dài chính xác đó thay vì chỉ đơn giản là`K // 3`. 

Sự điều trị đặc biệt của`"0"`là cần thiết vì biểu diễn thập phân của số 0 không chứa nhóm bổ sung. Một mã thông báo như`"000"`được phép ở trong một số, nhưng không thể là nhóm đầu tiên của số đó. 

Số nguyên Python không bị tràn, nhưng tất cả các phép cộng DP và tổng tiền tố đều được giảm modulo (10^9+7) để các giá trị được lưu trữ vẫn ở mức nhỏ. Mỗi mảng sử dụng chỉ mục dựa trên một cho các vị trí DP, điều này tạo nên`dp[j - 1]`tương ứng trực tiếp với điểm bắt đầu có thể có tại nhóm (j). 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
8 7
10 500 303 4 507 89 654 003
```ta có (K//3=2), vậy một số có nhóm đầu tiên có ba chữ số có thể chứa nhiều nhất hai nhóm. 

| tôi | Nhóm | Nhóm ngắn cuối cùng | Ba chữ số bắt đầu đóng góp | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | trống | 0 | không | 1 | 
| 1 |`10`| 1 | không | 1 | 
| 2 |`500`| 1 |`500`| 2 | 
| 3 |`303`| 1 |`500`,`303`| 3 | 
| 4 |`4`| 4 | không | 3 | 
| 5 |`507`| 4 |`507`| 6 | 
| 6 |`89`| 6 | không | 6 | 
| 7 |`654`| 6 |`654`| 12 | 
| 8 |`003`| 6 | không có, bởi vì`003`không thể bắt đầu | 6 | 

Ở vị trí 8,`003`chỉ có thể là phần tiếp theo, nhưng phần trước`89`không thể tiếp tục vào đó vì một số bắt đầu bằng`89`sẽ có ba chữ số cộng với ba chữ số khác, tổng cộng sáu chữ số, được phép. Sự đóng góp từ sự khởi đầu đó đã được đại diện bởi`dp[6]`. Câu trả lời cuối cùng là`6`. 

Đối với mẫu thứ hai,```
3 6
328 032 0
```hai nhóm đầu tiên có thể tạo thành số nguyên có sáu chữ số`328032`. Nhóm thứ hai không thể bắt đầu một số nguyên mới vì chữ số đầu tiên của nó bằng 0. trận chung kết`0`phải là một số nguyên riêng biệt. 

| tôi | Nhóm | Nhóm ngắn cuối cùng | Ba chữ số bắt đầu đóng góp | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | trống | 0 | không | 1 | 
| 1 |`328`| 0 |`328`| 1 | 
| 2 |`032`| 0 | không | 1 | 
| 3 |`0`| 3 | không | 1 | 

Hai nhóm đầu tiên cung cấp một số hợp lệ và nhóm cuối cùng cung cấp thêm một số nguyên. Không có sự phân chia thay thế bởi vì`032`không phải là nhóm đầu tiên hợp lệ. Câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi nhóm được xử lý một lần và mỗi lần chuyển đổi được đánh giá thông qua tổng tiền tố theo thời gian không đổi. | 
| Không gian | (O(N)) | Mảng DP và tổng tiền tố chứa các giá trị (N+1). | 

Với (N\le2\cdot10^5), thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi nhóm. Việc sử dụng bộ nhớ là tuyến tính theo (N), thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run the solution logic on an input string
import io
import sys

MOD = 1_000_000_007

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    k = int(next(it))
    a = [next(it) for _ in range(n)]

    dp = [0] * (n + 1)
    pref = [0] * (n + 1)

    dp[0] = 1
    last_short = 0
    max_three_groups = k // 3

    for i in range(1, n + 1):
        s = a[i - 1]
        length = len(s)

        if length == 3:
            if s[0] != '0':
                pref[i] = (pref[i - 1] + dp[i - 1]) % MOD
            else:
                pref[i] = pref[i - 1]

            left = max(last_short + 1, i - max_three_groups + 1)

            ways = (pref[i] - pref[left - 1]) % MOD

            if last_short:
                t = a[last_short - 1]
                tlen = len(t)

                valid_as_first = t != "0" and t[0] != '0'

                if valid_as_first:
                    digits = tlen + 3 * (i - last_short)
                    if digits <= k:
                        ways += dp[last_short - 1]
                        ways %= MOD

            dp[i] = ways

        else:
            if s == "0" or s[0] != '0':
                dp[i] = dp[i - 1]
            else:
                dp[i] = 0

            pref[i] = pref[i - 1]
            last_short = i

    return str(dp[n] % MOD)

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample 1
assert run(
    """8 7
10 500 303 4 507 89 654 003
"""
) == "6", "sample 1"

# Provided sample 2
assert run(
    """3 6
328 032 0
"""
) == "1", "sample 2"

# Minimum-size input, the only number is zero.
assert run(
    """1 3
0
"""
) == "1", "minimum case"

# Exact digit boundary: 6 digits fit when K=6, but not when K=5.
assert run(
    """2 6
999 999
"""
) == "2", "exact six-digit boundary"

assert run(
    """2 5
999 999
"""
) == "1", "five-digit boundary"

# Leading zero is valid only as a continuation group.
assert run(
    """2 4
1 000
"""
) == "1", "leading-zero continuation"

# All groups are valid starts, but K=3 permits only one group per number.
assert run(
    """3 3
999 999 999
"""
) == "1", "all equal groups"

# Maximum N with a simple answer. K=3 forces every group to be separate.
n = 200_000
inp = f"{n} 3\n" + " ".join(["999"] * n) + "\n"
assert run(inp) == "1", "maximum N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 0`|`1`| Đầu vào nhỏ nhất và biểu diễn đặc biệt của số 0 | 
|`2 6 / 999 999`|`2`| Ranh giới chữ số (K) chính xác và hai phân vùng có thể có | 
|`2 5 / 999 999`|`1`| Lỗi từng chữ số một trong giới hạn chữ số | 
|`2 4 / 1 000`|`1`| Số 0 đứng đầu chỉ được phép đối với các nhóm tiếp tục | 
|`3 3 / 999 999 999`|`1`| Nhóm lặp lại và tối đa ba chữ số | 
|`200000 3 / 999 ... 999`|`1`| Hành vi tối đa (N) và thời gian tuyến tính | 

## Vỏ cạnh 

Đối với trường hợp không,```
1 3
0
```nhóm có độ dài nhỏ hơn ba và được công nhận là đại diện độc lập hợp lệ. Bộ thuật toán`dp[1]=dp[0]=1`. Nó không chữa trị`0`làm tiền tố ngắn hơn ứng cử viên cho nhóm ba chữ số sau, do đó, một biểu diễn không chính tắc như`0 123`không bao giờ được tính. 

Để tiếp tục có số 0 đứng đầu,```
2 4
1 000
```nhóm đầu tiên`1`là một khởi đầu ngắn hợp lệ. Ở vị trí thứ hai,`000`không thể là số nguyên mới, nhưng nó có thể tiếp tục số nguyên bắt đầu từ`1`. Tổng chiều dài là (1+3=4), nằm trong giới hạn, vì vậy`dp[2]=1`. 

Đối với ranh giới chính xác,```
2 6
999 999
```cửa sổ bắt đầu gồm ba chữ số cho phép hai nhóm vì (3\cdot2=6). Hai sự khởi đầu có thể xảy ra là nhóm thứ nhất và nhóm thứ hai, tạo ra hai phân vùng`[999,999]`Và`[999] [999]`. Như vậy`dp[2]=2`. 

Thay vào đó, khi (K=5),```
2 5
999 999
```hai nhóm ba chữ số sẽ cần sáu chữ số, vì vậy nhóm thứ nhất không thể tiếp thu được nhóm thứ hai. Chỉ còn lại phân vùng đã chia, đưa ra`dp[2]=1`. Giới hạn dưới`i - K//3 + 1`loại bỏ chính xác quá trình chuyển đổi hai nhóm. 

Đối với một nhóm ngắn buộc phải có ranh giới,```
3 7
10 500 4
```hai nhóm đầu tiên có thể hình thành`10500`, trong khi`4`chỉ có một chữ số và không thể tiếp tục số đó. Khi vị trí 3 được xử lý, thuật toán sẽ đặt lại lần chạy ba chữ số ở`4`và có được`dp[3]=dp[2]`. Câu trả lời là`1`. 

Cuối cùng, bài kiểm tra có kích thước tối đa chứa 200.000 nhóm. Với (K=3), không có số nguyên nào có thể chứa hai nhóm ba chữ số, do đó phân vùng hợp lệ duy nhất có mỗi nhóm là một số nguyên riêng biệt. Thuật toán vẫn thực hiện một lần cập nhật liên tục cho mỗi nhóm và trả về`1`, chứng minh tại sao độ phức tạp tuyến tính là cần thiết cho các ràng buộc ban đầu.
