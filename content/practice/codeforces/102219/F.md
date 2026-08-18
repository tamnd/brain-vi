---
title: "CF 102219F - Hạng quân sự"
description: "Chúng ta có hai hàng lính, mỗi hàng chứa các vị trí từ (1) đến (n). Một người lính ở vị trí (i) ở hàng đầu tiên phải được ghép đôi với đúng một người lính ở hàng thứ hai."
date: "2026-08-17T22:54:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "F"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 180
verified: false
draft: false
---

[CF 102219F - Cấp quân sự](https://codeforces.com/problemset/problem/102219/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hàng lính, mỗi hàng chứa các vị trí từ (1) đến (n). Một người lính ở vị trí (i) ở hàng đầu tiên phải được ghép đôi với đúng một người lính ở hàng thứ hai. Cặp ((i,j)) thường được phép khi (|i-j|\le e) và một số cặp bổ sung bị cấm rõ ràng. 

Do đó, một cặp hoàn chỉnh là một hoán vị (p) của (1,\ldots,n), trong đó người lính (i) ở hàng đầu tiên được ghép với người lính (p_i) ở hàng thứ hai. Chúng ta cần đếm các hoán vị thỏa mãn cả giới hạn khoảng cách và mọi giới hạn cặp cấm rõ ràng, modulo (10^9+7). 

Ràng buộc quan trọng là (e\le4). Giá trị của (n) có thể đạt tới (2000), do đó, một giải pháp phụ thuộc bậc hai hoặc tệ hơn vào (n) đã không còn phù hợp với giới hạn một giây, trong khi mọi giai thừa đều hoàn toàn không thể thực hiện được. Tuy nhiên, giá trị nhỏ của (e) có nghĩa là mỗi người lính chỉ có thể tương tác với tối đa (2e+1\le9) vị trí ở hàng khác. Phạm vi tương tác bị giới hạn đó là điều khiến trạng thái bitmask nhỏ trở nên khả thi. 

Có một số trường hợp ranh giới trong đó việc triển khai bất cẩn có thể âm thầm đếm các cặp không hợp lệ. Với (e=0), mỗi người lính có chính xác một đối tác khả thi, vì vậy```
1 0 0
```phải sản xuất```
1
```trong khi```
1 0 1
1 1
```phải sản xuất```
0
```Một giải pháp xử lý danh sách bị cấm tách biệt với giới hạn khoảng cách có thể vô tình tính đến việc ghép cặp danh tính trong trường hợp thứ hai. 

Điểm bắt đầu và kết thúc của các hàng cũng đặc biệt vì cửa sổ bình thường xung quanh một người lính mở rộng ra ngoài phạm vi (1,\ldots,n). Ví dụ,```
2 1 0
```có hai cặp hợp lệ là ((1,1),(2,2)) và ((1,2),(2,1)), nên đáp án là (2). Việc triển khai mặt nạ chỉ đơn giản giả định mọi vị trí trong cửa sổ là một người lính thực sự có thể giới thiệu các vị trí không tồn tại làm đối tác khả thi. 

Cuối cùng, cặp bị cấm phải được kiểm tra đối với người lính thực sự đang được xử lý, không chỉ đối với vị trí trên mặt nạ. Vì```
2 1 1
1 2
```câu trả lời không hạn chế là (2), nhưng việc ghép đôi (1\to2) bị cấm, chỉ để lại duy nhất một cặp hợp lệ. Một quá trình chuyển đổi chỉ kiểm tra xem một cột có được sử dụng hay không mà bỏ qua mối quan hệ bị cấm sẽ tạo ra (2) thay vì (1). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi hoán vị của (n) binh sĩ ở hàng thứ hai. Đối với mỗi hoán vị, hãy kiểm tra tất cả (n) cặp và xác minh điều kiện khoảng cách và điều kiện cặp cấm. Điều này đúng vì mọi kết quả khớp hoàn toàn giữa hai hàng đều tương ứng với chính xác một hoán vị. 

Vấn đề là số lượng hoán vị. Có (n!) Trong số chúng và việc kiểm tra một hoán vị mất (O(n)) thời gian, vì vậy công việc trong trường hợp xấu nhất là (O(n\cdot n!)). Tại (n=2000), điều này có nghĩa là theo thứ tự kiểm tra cặp (2000\cdot2000!), vượt xa những gì có thể thử. 

Phương pháp brute-force hoạt động vì nó giữ toàn bộ lịch sử khớp. Quan sát giúp có thể tạo ra trạng thái nhỏ hơn là khi xử lý lính từ trái sang phải, lính (i) chỉ có thể sử dụng các cột từ (i-e) đến (i+e). Khi chúng ta di chuyển đủ xa về bên phải, cột cũ sẽ không bao giờ được sử dụng lại. Chúng ta chỉ cần nhớ những vị trí nào bên trong cửa sổ di chuyển hẹp này đã được chiếm giữ. 

Có nhiều nhất (2e+1) vị trí trong cửa sổ đó (9). Chúng ta có thể biểu thị trạng thái bị chiếm dụng hoặc rảnh rỗi của chúng bằng mặt nạ bit chứa tối đa (9) bit, cho ra nhiều nhất (2^9=512) trạng thái. Đối với mỗi người lính ở hàng đầu tiên, chúng tôi thử từng vị trí trống trong cửa sổ, từ chối các cặp bị cấm và sau đó di chuyển cửa sổ sang bên phải một vị trí. 

Quá trình chuyển đổi cũng cho chúng tôi một cách để thực thi rằng mọi người lính ở hàng thứ hai cuối cùng đều được sử dụng. Khi cửa sổ di chuyển, vị trí ngoài cùng bên trái của nó sẽ biến mất vĩnh viễn. Nếu vị trí đó là một người lính thực sự và bit của nó vẫn bằng 0 thì việc khớp một phần không bao giờ có thể trở nên hoàn chỉnh, vì vậy chúng tôi loại bỏ trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot n!)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\cdot 2^{2e+1}\cdot(2e+1)+k)) | (O(2^{2e+1}+k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu thị cửa sổ hiện tại cho người lính ở hàng đầu tiên (i) là vị trí ở hàng thứ hai 
[ 
i-e,\ i-e+1,\ldots,i+e. 
] 
Bit (b) của mặt nạ tương ứng với vị trí (i-e+b). Bit được đặt có nghĩa là vị trí đó đã được chiếm giữ, trong khi bit không được đặt có nghĩa là vị trí đó vẫn khả dụng.

Các vị trí bên ngoài (1,\ldots,n) được coi là đã được sử dụng. Họ không phải là những người lính thực sự nên không có người chuyển giới nào được phép chọn họ. 
2. Xây dựng bitmask bị cấm cho mọi binh sĩ ở hàng đầu. Nếu ((u,v)) bị cấm và (v) nằm trong cửa sổ của (u), hãy đặt bit tương ứng với (v). Sau đó, quá trình chuyển đổi có thể từ chối tất cả các lựa chọn bị cấm rõ ràng bằng cách sử dụng thao tác một bit. 
3. Khởi tạo mặt nạ trước khi xử lý lính (1). Cửa sổ của nó là 
[ 
1-e,\ldots,1+e. 
] 
Mọi vị trí bên dưới (1) và mọi vị trí trên (n) đều bắt đầu như bị chiếm đóng vì những vị trí đó không tương ứng với những người lính thực sự. DP ban đầu có giá trị (1) cho mặt nạ này. 
4. Đối với mỗi người lính ở hàng đầu tiên (i), lặp lại mọi mặt nạ có thể tiếp cận. Với mỗi bit 0 (b), hãy xem xét việc ghép đôi người lính (i) với 
[ 
j=i-e+b. 
] 
Đây chính xác là tập hợp các đối tác thỏa mãn (|i-j|\le e), do đó không có đối tác hợp lệ nào bị bỏ sót. 
5. Từ chối quá trình chuyển đổi nếu bit (b) đã bị chiếm dụng hoặc nếu cặp ((i,j)) tương ứng bị cấm. Nếu không thì đặt bit (b), vì người lính (j) hiện được sử dụng. 
6. Trước khi chuyển sang hàng tiếp theo, yêu cầu đặt bit ngoài cùng bên trái. Bit ngoài cùng bên trái đại diện cho vị trí (i-e). Sau khi xử lý người lính (i), vị trí này sẽ không bao giờ xuất hiện trong bất kỳ cửa sổ nào trong tương lai. Nếu là một người lính thực sự không được sử dụng, sau này sẽ không có cơ hội sánh đôi, vì vậy việc ghép một phần phải bị loại bỏ. 
7. Dịch chuyển mặt nạ sang phải một chút để di chuyển từ cửa sổ xung quanh (i) sang cửa sổ xung quanh (i+1). Vị trí ngoài cùng bên phải mới là (i+e+1). Nếu nó lớn hơn (n), hãy đặt bit của nó ngay lập tức vì đó là vị trí không tồn tại. Nếu không thì hãy bỏ cài đặt này vì đây là một người lính thực sự mới, chưa được sử dụng. 
8. Sau khi người lính (n) đã được xử lý, mọi vị trí thực sự ở hàng thứ hai chỉ rời khỏi cửa sổ di chuyển sau khi được xác nhận đã chiếm giữ. Tất cả các vị trí còn lại đều nằm ngoài hàng và được đánh dấu là đã có người sử dụng. Do đó, mặt nạ đầy đủ, với mỗi bit được đặt, thể hiện chính xác các kết quả khớp đã hoàn thành. Giá trị DP của nó là câu trả lời. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý (i) lính đầu tiên và dịch chuyển cửa sổ, mọi vị trí thực của hàng thứ hai nhỏ hơn điểm cuối bên trái của cửa sổ hiện tại đã được sử dụng chính xác một lần, trong khi mặt nạ ghi lại chính xác những vị trí nào vẫn hiển thị trong cửa sổ đã được sử dụng. Quá trình chuyển đổi chọn một đối tác được phép, chưa được sử dụng cho người lính (i), do đó, nó sẽ mở rộng mọi kết quả khớp từng phần hợp lệ đúng một lần. Việc kiểm tra bit gửi đi sẽ ngăn không cho một người lính không được sử dụng biến mất vĩnh viễn. Vì những người lính trong tương lai chỉ có thể kết nối trong khoảng cách (e), nên không có thông tin nào bên ngoài cửa sổ hiện tại có thể ảnh hưởng đến bất kỳ quyết định nào trong tương lai. Do đó, mọi trạng thái DP còn sót lại biểu thị các kết quả khớp một phần hợp lệ và mọi kết quả khớp hoàn chỉnh hợp lệ tuân theo chính xác một chuỗi chuyển đổi sang mặt nạ đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n, e, k = map(int, input().split())

    width = 2 * e + 1
    states = 1 << width
    full = states - 1
    top_bit = 1 << (width - 1)

    # banned[i] has a bit set for every forbidden second-row
    # position inside the window of first-row soldier i.
    banned = [0] * (n + 1)

    for _ in range(k):
        u, v = map(int, input().split())

        # Pairs outside the distance window can never be used anyway.
        if abs(u - v) <= e:
            bit = v - (u - e)
            if 0 <= bit < width:
                banned[u] |= 1 << bit

    # Before processing row 1, the window is [1-e, 1+e].
    # Positions outside [1,n] are considered already occupied.
    initial = 0
    for bit in range(width):
        col = 1 - e + bit
        if col < 1 or col > n:
            initial |= 1 << bit

    # For every mask, precompute which free bits can be selected,
    # together with the mask before inserting the new rightmost bit.
    transitions = [[] for _ in range(states)]

    for mask in range(states):
        free = full ^ mask
        while free:
            bit = free & -free
            free -= bit

            new_mask = mask | bit

            # The outgoing position must already be occupied.
            if new_mask & 1:
                transitions[mask].append(
                    (bit, new_mask >> 1)
                )

    dp = [0] * states
    dp[initial] = 1

    for i in range(1, n + 1):
        ndp = [0] * states
        forbidden = banned[i]

        # The new rightmost position after this transition.
        new_col = i + e + 1
        new_col_is_virtual = new_col > n

        for mask, value in enumerate(dp):
            if value == 0:
                continue

            for bit, shifted in transitions[mask]:
                if forbidden & bit:
                    continue

                nxt = shifted
                if new_col_is_virtual:
                    nxt |= top_bit

                x = ndp[nxt] + value
                if x >= MOD:
                    x -= MOD
                ndp[nxt] = x

        dp = ndp

    print(dp[full] % MOD)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào chuyển đổi mọi cặp bị cấm thành một bit bên trong cửa sổ cục bộ của hàng tương ứng. Các cặp bị cấm đã nằm ngoài giới hạn khoảng cách có thể bị bỏ qua vì dù sao thì chúng cũng không bao giờ có thể tham gia vào một trận đấu hợp lệ. 

các`initial`mặt nạ xử lý ranh giới bên trái. Ví dụ: với (e=2), cửa sổ đầu tiên là ([-1,0,1,2,3]), do đó, hai vị trí đầu tiên là ảo và bắt đầu bằng tập bit của chúng. 

Việc tính toán trước`transitions`mảng chứa phần cấu trúc của mọi chuyển đổi mặt nạ. Điều kiện duy nhất tùy thuộc vào hàng đầu vào hiện tại là liệu bit đã chọn có bị cấm hay không. Việc tách hai phần này sẽ tránh việc xây dựng lại nhiều lần các chuyển đổi mặt nạ giống nhau cho tất cả (n) hàng. 

Séc`new_mask & 1`là điều kiện chính xác chính. Sau khi người lính hiện tại được ghép, cột ngoài cùng bên trái sắp biến mất. Nó nhất định đã bị chiếm đóng, nếu không thì không một người lính hàng đầu nào trong tương lai có thể tiếp cận được. 

Bit ngoài cùng bên phải mới chỉ được đặt khi vị trí mới lớn hơn (n). Vị trí như vậy nằm ngoài hàng thứ hai thực tế và không bao giờ được chọn, vì vậy việc đánh dấu vị trí đó bị chiếm giữ tương đương với việc loại bỏ nó khỏi vị trí xem xét. 

Số nguyên Python không bị tràn, nhưng tất cả các phép cộng DP vẫn bị giảm modulo (10^9+7). Việc triển khai sử dụng hai mảng một chiều, do đó bộ nhớ chỉ phụ thuộc vào số lượng mặt nạ chứ không phụ thuộc vào (n) lần số lượng mặt nạ. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
2 1 0
```có hai kết quả khớp hoàn chỉnh hợp lệ. Ở đây (e=1), vậy mỗi mặt nạ có ba bit. Cửa sổ đầu tiên là ([0,1,2]), trong đó vị trí (0) là ảo. 

| Người lính hàng đầu | Mặt nạ hiện tại | Cột được chọn | Mặt nạ sau ca | Ý nghĩa | 
| --- | --- | --- | --- | --- | 
| 1 |`001`| 1 |`101`| cột 1 được sử dụng | 
| 1 |`001`| 2 |`110`| cột 2 được sử dụng | 
| 2 |`101`| 2 |`111`| cột 1 và 2 được sử dụng | 
| 2 |`110`| 1 |`111`| cột 1 và 2 được sử dụng | 

Hai nhánh tương ứng chính xác với hai hoán vị. Cả hai kết thúc ở mặt nạ`111`, vậy đáp án là (2). Dấu vết cũng giải thích tại sao các vị trí ảo phải bắt đầu như bị chiếm đóng và tại sao trạng thái cuối cùng là mặt nạ đầy đủ. 

Đối với mẫu 2,```
2 1 1
1 2
```cặp giữa vị trí hàng thứ nhất (1) và vị trí hàng thứ hai (2) bị cấm. 

| Người lính hàng đầu | Mặt nạ hiện tại | Ứng viên | Kết quả | 
| --- | --- | --- | --- | 
| 1 |`001`| cột 1 | được chấp nhận, mặt nạ tiếp theo`101`| 
| 1 |`001`| cột 2 | bị từ chối bởi mặt nạ cấm | 
| 2 |`101`| cột 2 | chấp nhận, mặt nạ cuối cùng`111`| 

Chỉ có sự phù hợp về danh tính mới tồn tại. Câu trả lời là (1). Dấu vết này xác nhận rằng mặt nạ cặp cấm được áp dụng cho đối tác đã chọn trước khi thêm quá trình chuyển đổi DP. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\cdot2^{2e+1}\cdot(2e+1)+k)) | Có (n) hàng, nhiều nhất (2^{2e+1}) mặt nạ và nhiều nhất (2e+1) lựa chọn đối tác cho mỗi mặt nạ | 
| Không gian | (O(2^{2e+1}+n+k)) | Hai mảng DP, các chuyển tiếp được tính toán trước và mặt nạ bị cấm | 

Vì (e\le4) nên số lượng mặt nạ nhiều nhất là (2^9=512) và mỗi trạng thái có nhiều nhất (9) chuyển đổi ứng cử viên. Với (n\le2000), DP chỉ thực hiện vài triệu thao tác trạng thái nhỏ, trong khi đầu vào bị cấm chỉ đóng góp (O(k)) công việc tiền xử lý. Việc sử dụng bộ nhớ cũng rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# This test harness assumes solve_data is the same algorithm as the
# solve() function above, but accepts a string and returns the answer.

import io
import sys

MOD = 1_000_000_007

def solve_data(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))
    e = int(next(it))
    k = int(next(it))

    width = 2 * e + 1
    states = 1 << width
    full = states - 1
    top_bit = 1 << (width - 1)

    banned = [0] * (n + 1)

    for _ in range(k):
        u = int(next(it))
        v = int(next(it))
        if abs(u - v) <= e:
            bit = v - (u - e)
            if 0 <= bit < width:
                banned[u] |= 1 << bit

    initial = 0
    for bit in range(width):
        col = 1 - e + bit
        if col < 1 or col > n:
            initial |= 1 << bit

    transitions = [[] for _ in range(states)]

    for mask in range(states):
        free = full ^ mask
        while free:
            bit = free & -free
            free -= bit
            new_mask = mask | bit
            if new_mask & 1:
                transitions[mask].append((bit, new_mask >> 1))

    dp = [0] * states
    dp[initial] = 1

    for i in range(1, n + 1):
        ndp = [0] * states
        forbidden = banned[i]
        virtual_right = i + e + 1 > n

        for mask, value in enumerate(dp):
            if value == 0:
                continue

            for bit, shifted in transitions[mask]:
                if forbidden & bit:
                    continue

                nxt = shifted
                if virtual_right:
                    nxt |= top_bit

                ndp[nxt] = (ndp[nxt] + value) % MOD

        dp = ndp

    return str(dp[full])

# Provided sample 1
assert solve_data("2 1 0\n") == "2", "sample 1"

# Provided sample 2
assert solve_data("2 1 1\n1 2\n") == "1", "sample 2"

# Minimum size, only possible matching.
assert solve_data("1 0 0\n") == "1", "minimum size"

# Minimum size with its only pair forbidden.
assert solve_data("1 0 1\n1 1\n") == "0", "forbidden only pair"

# e = 0 means only the identity matching exists.
assert solve_data("5 0 0\n") == "1", "zero distance"

# e = 1, n = 3 gives identity, swap (1,2), or swap (2,3).
assert solve_data("3 1 0\n") == "3", "boundary window"

# Removing the (1,2) matching leaves two possibilities.
assert solve_data("3 1 1\n1 2\n") == "2", "forbidden boundary edge"

# For n = e + 1, every pair is allowed, so all 3! permutations work.
assert solve_data("3 2 0\n") == "6", "all positions allowed"

# Maximum n with the smallest state space.
assert solve_data("2000 0 0\n") == "1", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0`|`1`| Phiên bản có kích thước tối thiểu và trường hợp nhận dạng (e=0) | 
|`1 0 1\n1 1`|`0`| Một cặp bị cấm có thể loại bỏ cặp trùng khớp duy nhất | 
|`5 0 0`|`1`| Điều kiện biên khoảng cách bằng không | 
|`3 1 0`|`3`| Ranh giới cửa sổ di chuyển và một số hoán vị hợp lệ | 
|`3 1 1\n1 2`|`2`| Cặp cấm ở rìa cửa sổ được phép | 
|`3 2 0`|`6`| Mọi vị trí ở hàng thứ hai đều có thể truy cập được | 
|`2000 0 0`|`1`| Tối đa (n) với không gian trạng thái nhỏ nhất có thể | 

## Vỏ cạnh 

Khi (e=0), mọi binh sĩ hàng đầu chỉ có thể sử dụng binh sĩ hàng thứ hai có cùng chỉ số. Vì```
1 0 1
1 1
```mặt nạ ban đầu là`0`. Bit ứng cử viên duy nhất tương ứng với cột (1), nhưng mặt nạ bị cấm chứa bit đó, do đó không có chuyển đổi và giá trị DP mặt nạ đầy đủ cuối cùng là (0). Nếu không kiểm tra mặt nạ bị cấm trong quá trình chuyển đổi, thuật toán sẽ trả về sai (1). 

Ở ranh giới bên trái, các vị trí không tồn tại phải được coi là bị chiếm dụng. Coi như```
2 1 0
```Cửa sổ ban đầu là ([0,1,2]), vì vậy mặt nạ ban đầu của nó là`001`. Người lính đầu tiên có thể chọn cột (1) hoặc cột (2). Sau một trong hai lựa chọn, cột ảo đi (0) đã được sử dụng nên trạng thái có thể di chuyển sang phải một cách an toàn. Điều này ngăn không cho cột không tồn tại (0) bị vô tình chọn. 

Tại ranh giới bên phải, vị trí mới đi vào cửa sổ cuối cùng sẽ lớn hơn (n). Trong ví dụ tương tự, sau khi xử lý người lính đầu tiên, vị trí mới là (3), vị trí này không tồn tại. Bit của nó được thiết lập ngay lập tức. Khi người lính thứ hai được xử lý, vị trí ảo đó không thể được chọn, trong khi cột thực còn lại vẫn có thể được chọn. Do đó, cả hai kết quả khớp hợp lệ đều đạt được mặt nạ đầy đủ. 

Một cặp bị cấm chỉ có thể loại bỏ một nhánh của trạng thái hợp lệ. Vì```
2 1 1
1 2
```người lính đầu tiên có thể có hai cột trước khi xem xét hạn chế rõ ràng. Việc chuyển sang cột (2) bị xóa, chỉ còn lại cột (1). Người lính thứ hai sau đó buộc phải sử dụng cột (2), đưa ra chính xác một kết quả khớp hoàn chỉnh. 

Trạng thái cuối cùng phải là mặt nạ đầy đủ chứ không phải là mặt nạ sống sót tùy ý. Mỗi cột thực phải được xác nhận đã được sử dụng trước khi nó rời khỏi cửa sổ, trong khi các vị trí ngoài (n) được chèn rõ ràng là đã được sử dụng. Do đó, sau ca cuối cùng, tất cả các vị trí (2e+1) trong cửa sổ cuối cùng đều bị chiếm giữ. Đối với Mẫu 1, trạng thái cuối cùng là`111`và giá trị DP của nó chính xác là số lượng khớp hoàn chỉnh.
