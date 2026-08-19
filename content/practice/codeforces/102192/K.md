---
title: "CF 102192K - Làm nổ bong bóng"
description: "Hãy coi mỗi hàng là một đỉnh bên trái và mỗi cột là một đỉnh bên phải. Bóng ở ô (r, c) là cạnh giữa hàng r và cột c. Khi chúng ta thả một quả bóng bay vào (r, c), mọi quả bong bóng còn lại ở hàng r hoặc cột c sẽ biến mất."
date: "2026-08-18T02:15:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "K"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 236
verified: true
draft: false
---

[CF 102192K - Làm nổ bong bóng](https://codeforces.com/problemset/problem/102192/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi hàng là một đỉnh bên trái và mỗi cột là một đỉnh bên phải. Một quả bóng ở phòng giam`(r, c)`là một cạnh giữa hàng`r`và cột`c`. 

Khi chúng tôi thả một quả bóng bay vào`(r, c)`, mọi sự cố bong bóng còn lại phải chèo`r`hoặc cột`c`biến mất. Do đó, hai bong bóng cùng bật trực tiếp không bao giờ có thể chia sẻ một hàng hoặc một cột. Những quả bóng bay trực tiếp sẽ tạo thành một sự khớp trong biểu đồ lưỡng cực này. 

Còn một điều kiện nữa. Sau khi ném hết phi tiêu, mọi quả bóng bay hẳn đã biến mất. Một quả bóng không được bật trực tiếp phải chia sẻ hàng hoặc cột của nó với một quả bóng nào đó được bật trực tiếp. Trong thuật ngữ biểu đồ, kết quả khớp trực tiếp phải đạt mức tối đa. Do đó, vấn đề là đếm các kết quả phù hợp tối đa theo kích thước, sau đó tính đến thực tế là các phi tiêu được ném theo thứ tự. 

Đối với mọi`x`từ`1`bởi vì`k`, đầu ra yêu cầu số thứ tự chính xác`x`vị trí bong bóng tạo thành một quá trình thanh toán bù trừ như vậy. Thứ tự rất quan trọng, vì vậy nếu một bộ`x`bóng bay nổ hợp lệ được tìm thấy, nó góp phần`x!`trình tự phi tiêu khác nhau. 

Lưới có nhiều nhất`12`hàng và`20`cột. Kích thước nhỏ là hạn chế chính. Một trạng thái mô tả tất cả các hàng có thể có ba khả năng trên mỗi hàng, đưa ra`3^12 = 531441`nhiều nhất là các trạng thái. Con số này lớn nhưng có thể quản lý được nhờ một chương trình năng động được triển khai cẩn thận. Một trạng thái bao gồm tất cả`20`cột sẽ có`3^20`, vốn đã quá lớn, do đó, lưới phải được định hướng sao cho kích thước được biểu thị bằng trạng thái bậc ba là kích thước nhỏ hơn. 

Số lượng phi tiêu nhiều nhất là`20`, nhưng có thể có nhiều nhất`12`bong bóng được thả trực tiếp vì bong bóng được thả trực tiếp không thể xếp chung một hàng. tham số`k`vẫn hữu ích cho việc cắt tỉa các trạng thái có nhiều hơn`k`phi tiêu, nhưng nó không làm thay đổi số lượng trạng thái tiệm cận. 

Tuyên bố chính thức sử dụng giới hạn bảy giây và bộ nhớ 256 MB. Mẫu của nó bao gồm bốn trường hợp thử nghiệm và có kết quả đầu ra`1, 2`,`2, 0`,`1, 8, 0`, Và`2`, tương ứng. 

Trường hợp cạnh đầu tiên là một lưới hoàn toàn trống. Ví dụ,```
1
1 1 1
.
```có đầu ra```
0
```bởi vì không có vị trí pháp lý cho ngay cả phi tiêu đầu tiên. Việc triển khai bất cẩn coi việc khớp trống như một chiến lược thanh toán bù trừ thành công sẽ báo cáo không chính xác một chiều cho số phi tiêu bằng 0, nhưng vấn đề chỉ yêu cầu số lượng phi tiêu dương. 

Một trường hợp khác là một lưới có nhiều quả bóng bay xếp thành một hàng. Vì```
1
1 3 3
QQQ
```đầu ra đúng là```
3
0
0
```Một phi tiêu là đủ và có thể có ba ô cho nó. Một lực lượng vũ phu dựa trên việc chọn các tập hợp con bóng bay tùy ý có thể đếm chính xác ba lựa chọn ô đơn, nhưng nó có thể dễ dàng xử lý sai thực tế là thứ tự phi tiêu không có hiệu lực khi chỉ có một phi tiêu. 

Một trường hợp tế nhị hơn là```
1
2 2 2
Q.
.Q
```Đầu ra đúng là```
0
2
```Mỗi quả bóng bay phải được bật trực tiếp và hai phi tiêu có thể được ném theo một trong hai thứ tự. Một cách tiếp cận bất cẩn chỉ đếm các tập hợp ô được bật ra sẽ trả về`1`thay vì`2`. 

Cuối cùng, hãy xem xét một lưới dày đặc trong đó`k`lớn hơn số hàng. Ví dụ, một`2 x 2`lưới toàn bóng bay không bao giờ có thể yêu cầu hoặc cho phép ba quả bóng bay được thả trực tiếp vì hai quả bóng bay được thả trực tiếp đã sử dụng cả hai hàng. Câu trả lời cho số lượng phi tiêu lớn hơn phải bằng không. DP xử lý việc này một cách tự nhiên vì số lượng hàng giới hạn số chữ số trạng thái bằng`1`, đại diện cho các hàng đã được chọn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là chọn quả bóng đầu tiên bật lên, mô phỏng hàng và cột của nó bị xóa, chọn quả bóng còn lại tiếp theo và tiếp tục cho đến khi tất cả các quả bóng bay biến mất hoặc`k`phi tiêu đã được sử dụng. Điều này đúng vì mọi chuỗi phi tiêu hợp pháp đều được khám phá trực tiếp và mô phỏng tuân thủ chính xác các quy tắc trò chơi. 

Vấn đề là yếu tố phân nhánh. MỘT`12 x 20`lưới có thể chứa`240`bong bóng. Nếu tất cả chúng đều có mặt và chúng ta thử mọi trình tự có thứ tự một cách chính xác`20`phi tiêu, số lá là`240 * 239 * 238 * ... * 221 = 240! / 220!`. 

Đó là về rồi`10^47`trình tự. Ngay cả việc chỉ kiểm tra các tập hợp con thay vì các chuỗi được sắp xếp cũng mang lại khả năng tìm kiếm theo cấp số nhân trên hàng trăm quả bóng bay. 

Quan sát hữu ích là các quả bóng bay trực tiếp thực sự tạo thành một sự phù hợp. Sau khi chúng tôi quyết định hàng nào đã được chọn, thông tin duy nhất cần có về các cột trước đó là liệu một hàng đã được chọn hay chưa, liệu nó đã xuất hiện trong một cột không được chọn và vẫn cần được chọn hay liệu nó chưa bao giờ xuất hiện trong một cột có liên quan. 

Điều đó mang lại ba trạng thái mỗi hàng. Chúng tôi xử lý cột lưới theo cột, sử dụng mặt nạ bậc ba cho các trạng thái hàng. Chúng tôi cố tình giữ các cột bên ngoài trạng thái vì chỉ có`20`của họ. 

Đối với một cột cố định, chỉ có hai loại hành động. Chúng tôi hoặc không ném phi tiêu vào cột đó, trong trường hợp đó, mỗi quả bóng bay trong cột sẽ có nghĩa vụ cuối cùng phải chọn hàng của nó hoặc chúng tôi ném phi tiêu vào một quả bóng bay trong cột. Trong trường hợp thứ hai, hàng đó sẽ được chọn, trong khi tất cả các bong bóng khác trong cột sẽ biến mất ngay lập tức và không tạo ra nghĩa vụ nào trong tương lai. 

Do đó, DP liệt kê mọi kết quả khớp có thể xảy ra một lần, sử dụng thứ tự cột tăng dần làm biểu diễn chuẩn của nó. Sau cột cuối cùng, một trạng thái thành công chính xác khi không có hàng nào có bong bóng nổi bật chưa được chọn. Nếu trạng thái chứa`x`các hàng đã chọn, nó đại diện cho một kết hợp kích thước tối đa chuẩn`x`. 

Các vị trí phi tiêu sau đó có thể được sắp xếp tùy ý. Vì các bong bóng được chọn tạo thành một kết quả khớp, không có hai bong bóng nào trong số chúng có chung một hàng hoặc cột, nên mọi hoán vị của các bong bóng được chọn đều là thứ tự phi tiêu hợp lệ. Chúng tôi nhân số lượng kết quả phù hợp về kích thước chuẩn`x`qua`x!`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(P(B, k))`, Ở đâu`B <= 240`|`O(B)`| Quá chậm | 
| Tối ưu |`O(n * m * 3^m)`|`O(3^m)`| Đã chấp nhận | 

Đây`m <= 12`là thứ nguyên được đại diện bởi trạng thái ternary và`n <= 20`là số lượng cột được xử lý. 

## Hướng dẫn thuật toán 

1. Coi mỗi quả bóng như một cạnh giữa hàng và cột của nó. Một bộ bong bóng được bật trực tiếp phải phù hợp vì hai quả bóng bay được bật trực tiếp không thể chia sẻ một hàng hoặc cột. 
2. Xử lý lưới theo cột. Vì có nhiều nhất`20`cột và nhiều nhất`12`hàng, việc lưu trữ trạng thái ba ngôi trên các hàng là biểu diễn nhỏ hơn. 
3. Cho mỗi hàng một trong ba trạng thái. Tình trạng`0`có nghĩa là không có bong bóng nào ở hàng này xuất hiện trong một cột không được bật lên. Tình trạng`1`có nghĩa là hàng đã được chọn bởi phi tiêu. Tình trạng`2`có nghĩa là bong bóng ở hàng này đã xuất hiện ở cột trước đó và chưa xuất hiện, vì vậy hàng này vẫn cần được chọn sau. 
4. Bắt đầu với trạng thái`0`cho mỗi hàng và một cách DP. 
5. Đối với cột hiện tại, trước tiên hãy cân nhắc việc không ném phi tiêu vào đó. Mọi bong bóng trong cột đó chỉ biến mất nếu hàng của nó được chọn sau đó, vì vậy mọi hàng hiện tại đều ở trạng thái`0`có chứa bong bóng trong cột này sẽ chuyển sang trạng thái`2`. Các hàng đã ở trạng thái`2`ở đó, trong khi hàng ở trạng thái`1`vẫn được chọn. 
6. Tiếp theo, hãy xem xét việc ném phi tiêu vào mỗi quả bóng của cột hiện tại có hàng chưa được chọn. Hàng đã chọn thay đổi thành trạng thái`1`. Tất cả các bong bóng khác trong cột hiện tại đều bị phi tiêu này thổi bay nên không tạo trạng thái mới`2`nghĩa vụ. 
7. Từ chối các trạng thái có số lượng hàng được chọn vượt quá`k`. Vì mỗi hàng được chọn tương ứng với một phi tiêu, những trạng thái như vậy không bao giờ có thể đóng góp vào câu trả lời. 
8. Sau khi tất cả các cột đã được xử lý, chỉ giữ lại các trạng thái không chứa chữ số`2`. Trạng thái như vậy không có hàng nào vẫn cần phi tiêu, vì vậy mọi quả bóng bay đã được xóa trực tiếp hoặc bằng cách chia sẻ một hàng hoặc cột với một quả bóng bay trực tiếp. 
9. Thêm giá trị DP của từng trạng thái thành công vào câu trả lời được lập chỉ mục theo số trạng thái của nó`1`chữ số. Điều này tính mỗi kết quả khớp tối đa một lần vì các cột đã chọn của nó được xử lý theo thứ tự tăng dần. 
10. Với mỗi`x`, nhân số lượng kết quả khớp chuẩn với`x!`. Phép nhân chuyển đổi thứ tự cột chuẩn thành tất cả các thứ tự có thể có của`x`ném phi tiêu. 

Sau các bước được đánh số, bất biến chính là sau khi xử lý bước đầu tiên`j`cột, trạng thái ghi lại chính xác những hàng nào đã được chọn và những hàng nào vẫn cần được chọn vì đã gặp phải bong bóng không rõ ràng trong các cột đó. Quá trình chuyển đổi sẽ bỏ chọn cột hiện tại, tạo chính xác các nghĩa vụ mới đó hoặc chọn một bong bóng hợp lệ và xóa phần còn lại của cột đó ngay lập tức. Do đó, mỗi đường dẫn DP tương ứng với một kết quả khớp và mỗi kết quả khớp có chính xác một đường dẫn khi các cột được chọn của nó được xem xét từ trái sang phải. Trạng thái cuối cùng không có chữ số`2`chính xác là một sự kết hợp tối đa, chính xác là một bộ phi tiêu phá hủy mọi quả bóng bay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(m, n, k, grid):
    # We keep m as the smaller dimension.
    # The input already guarantees m <= 12 and n <= 20.
    #
    # For every column, mask bit r is set when grid[r][col] == 'Q'.
    columns = []
    for col in range(n):
        mask = 0
        for r in range(m):
            if grid[r][col] == 'Q':
                mask |= 1 << r
        columns.append(mask)

    states = 3 ** m

    pow3 = [1] * m
    for r in range(1, m):
        pow3[r] = pow3[r - 1] * 3

    # sum3[mask] = sum(pow3[r]) over all set bits r.
    # It lets us change all selected zero digits to digit 2 at once.
    sum3 = [0] * (1 << m)
    for mask in range(1, 1 << m):
        bit = mask & -mask
        r = bit.bit_length() - 1
        sum3[mask] = sum3[mask ^ bit] + pow3[r]

    # one[s]: rows whose ternary digit is 1.
    # two[s]: rows whose ternary digit is 2.
    # cnt[s]: number of digits equal to 1.
    #
    # Row 0 is the least significant ternary digit.
    one = [0] * states
    two = [0] * states
    cnt = [0] * states

    for s in range(1, states):
        q, d = divmod(s, 3)
        one[s] = (one[q] << 1) | (1 if d == 1 else 0)
        two[s] = (two[q] << 1) | (1 if d == 2 else 0)
        cnt[s] = cnt[q] + (1 if d == 1 else 0)

    dp = [0] * states
    dp[0] = 1

    full_rows = (1 << m) - 1

    for col_mask in columns:
        ndp = [0] * states

        for s in range(states):
            ways = dp[s]
            if ways == 0:
                continue

            used = cnt[s]
            if used > k:
                continue

            one_mask = one[s]
            two_mask = two[s]

            # Case 1: do not shoot this column.
            #
            # Every row containing a balloon in this column and currently
            # in state 0 becomes state 2.
            zero_rows = col_mask & ~(one_mask | two_mask) & full_rows
            base = s + 2 * sum3[zero_rows]

            ndp[base] += ways

            # Case 2: shoot one balloon in this column.
            #
            # A row already in state 1 cannot be shot again.
            # Starting from 'base', the chosen row would have digit 2,
            # but becomes digit 1, so subtract one power of 3.
            if used < k:
                available = col_mask & ~one_mask & full_rows

                while available:
                    bit = available & -available
                    ns = base - pow3[bit.bit_length() - 1]
                    ndp[ns] += ways
                    available ^= bit

        dp = ndp

    # Successful states have no digit 2.
    # Digit 1 counts directly popped rows, hence darts.
    ans = [0] * (min(k, m) + 1)

    for s in range(states):
        ways = dp[s]
        if ways and two[s] == 0:
            x = cnt[s]
            if 1 <= x <= k:
                ans[x] += ways

    # The DP stores each matching in increasing column order.
    # The actual darts may be thrown in any order.
    fact = 1
    result = []
    for x in range(1, k + 1):
        if x <= m:
            fact *= x
            result.append(str(ans[x] * fact))
        else:
            result.append("0")

    return "\n".join(result)

def solve(data):
    tokens = data.split()
    it = iter(tokens)

    t = int(next(it))
    out = []

    for _ in range(t):
        m = int(next(it))
        n = int(next(it))
        k = int(next(it))

        grid = [next(it).decode() if isinstance(next_val := next(it), bytes) else next_val
                for _ in range(m)]

        out.append(solve_case(m, n, k, grid))

    return "\n".join(out) + "\n"

def main():
    t = int(input())
    out = []

    for _ in range(t):
        m, n, k = map(int, input().split())
        grid = [input().strip() for _ in range(m)]
        out.append(solve_case(m, n, k, grid))

    sys.stdout.write("\n".join(out) + "\n")

if __name__ == "__main__":
    main()
```Biểu diễn cốt lõi là một số nguyên ternary. Chữ số bậc ba có ý nghĩa nhỏ nhất thuộc về hàng 0, vì vậy việc thay đổi trạng thái hàng chỉ là cộng hoặc trừ lũy thừa của ba. Các mảng`one`Và`two`hãy để quá trình chuyển đổi xác định hàng nào được chọn và hàng nào vẫn có bóng bay chưa được giải quyết mà không giải mã nhiều lần số thứ ba.`sum3`là một tối ưu hóa nhỏ quan trọng trong không gian trạng thái này. Khi một cột không được bật lên, mọi hàng bong bóng hiện chưa nhìn thấy sẽ thay đổi từ chữ số`0`để chữ số`2`. Thay vì sửa đổi từng hàng riêng biệt, mã sẽ thu thập tất cả các hàng như vậy thành một mặt nạ bit và cộng gấp đôi lũy thừa tương ứng của ba. 

Quá trình chuyển đổi chụp đáng được chú ý đặc biệt. Biến`base`thể hiện điều gì sẽ xảy ra nếu cột hiện tại không được bật lên. Đối với một hàng được chọn làm vị trí phi tiêu, hàng đó sẽ có chữ số`2`TRONG`base`, nhưng phi tiêu thay đổi nó thành chữ số`1`. Do đó, việc trừ chính xác một lũy thừa của ba sẽ cho kết quả đúng cho dù chữ số ban đầu có phải là`0`hoặc`2`. 

Một hàng có chữ số`1`không thể chọn lại được. Phi tiêu trước đó của nó đã loại bỏ mọi quả bóng bay trong hàng đó, vì vậy không thể còn quả bóng bay hợp pháp ở đó. Đây là lý do tại sao`available`loại bỏ`one_mask`. 

Số nguyên Python có độ chính xác tùy ý, điều này cần thiết ở đây. Số lượng chuỗi phi tiêu hợp lệ có thể lớn hơn nhiều so với số nguyên 64 bit, do đó, không giống như bài toán đếm mô-đun thông thường, không áp dụng mô-đun nào. 

các`solve`trình trợ giúp được bao gồm cho các bài kiểm tra dựa trên khẳng định bên dưới. Điểm tham gia cuộc thi thực tế sử dụng`input = sys.stdin.readline`và xử lý các trường hợp thử nghiệm một cách trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét mẫu chính thức đầu tiên:```
2 2 2
QQ
.Q
```Các cột là`{row 1}`Và`{row 1, row 2}`. Chúng tôi sử dụng`0`vì không nhìn thấy được,`1`đã được chọn bởi phi tiêu và`2`cho một hàng hiện có bong bóng chưa được giải quyết. 

| Cột | Trạng thái trước đó | Hành động | Tiểu bang mới | Cách | 
| --- | --- | --- | --- | --- | 
| 1 |`00`| không có phi tiêu |`20`| 1 | 
| 1 |`00`| bắn hàng 1 |`10`| 1 | 
| 2 |`20`| không có phi tiêu |`22`| 1 | 
| 2 |`20`| bắn hàng 1 |`10`| 1 | 
| 2 |`10`| không có phi tiêu |`12`| 1 | 
| 2 |`10`| bắn hàng 2 |`11`| 1 | 

Sau cột thứ hai, nêu`10`Và`11`hợp lệ vì chúng không chứa chữ số`2`. Cái đầu tiên đại diện cho một quả bóng được chọn, vì vậy nó góp phần`1`kết hợp kinh điển. Hình thứ hai đại diện cho hai quả bóng được chọn, vì vậy nó góp phần`1`kết hợp kinh điển. 

Đối với một phi tiêu,`1! = 1`. Đối với hai phi tiêu,`2! = 2`. Do đó, đầu ra cuối cùng là`1, 2`, phù hợp với mẫu chính thức. 

Phần thú vị là trạng thái`20`khi cột thứ 2 bắn vào hàng 1. Bong bóng của hàng 2 ở cột đó bị thổi bay ngay lập tức nên hàng 2 không trở thành trạng thái chưa giải quyết được. Chính vì vậy kết quả là`10`, không phải trạng thái chứa hàng 2 là chưa được giải quyết. 

### Mẫu 2 

Mẫu thứ hai là```
2 2 2
QQ
..
```Cả hai quả bóng đều ở cùng một hàng. 

| Cột | Trạng thái trước đó | Hành động | Tiểu bang mới | Cách | 
| --- | --- | --- | --- | --- | 
| 1 |`00`| không có phi tiêu |`20`| 1 | 
| 1 |`00`| bắn hàng 1 |`10`| 1 | 
| 2 |`20`| không có phi tiêu |`20`| 1 | 
| 2 |`20`| bắn hàng 1 |`10`| 1 | 
| 2 |`10`| không có phi tiêu |`10`| 1 | 

Trạng thái cuối cùng`10`nhận được theo hai cách. Chúng tương ứng với việc bắn quả bóng đầu tiên hoặc bắn quả bóng thứ hai. Cả hai lựa chọn đều xóa toàn bộ hàng, vì vậy câu trả lời cho một phi tiêu là`2`. 

Không có giải pháp hai phi tiêu hợp lệ. Khi một quả bóng bay trong hàng bật ra, quả bóng còn lại sẽ biến mất cùng với nó, vì vậy không bao giờ có phi tiêu thứ hai hợp pháp. Đầu ra là`2, 0`, một lần nữa phù hợp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n * m * 3^m)`| có`3^m`trạng thái hàng cho mỗi`n`cột và mỗi tiểu bang có thể thử mọi hàng bong bóng trong cột đó | 
| Không gian |`O(3^m + 2^m)`| Hai lớp DP và siêu dữ liệu trạng thái thứ ba chi phối việc sử dụng bộ nhớ | 

Với`m = 12`, chỉ có`531441`trạng thái bậc ba. Công việc chuyển tiếp tối đa là khoảng`20 * 12 * 531441`, Về`127.5`triệu thao tác hàng trạng thái đơn giản. Việc triển khai tránh giải mã các trạng thái ternary bên trong vòng lặp trong cùng và xử lý các hàng bằng mặt nạ bit, điều này cần thiết cho giới hạn trên. Yêu cầu bộ nhớ bị chi phối bởi hai lớp DP và vẫn nằm trong giới hạn không gian trạng thái dự định. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import factorial

def run(inp: str) -> str:
    return solve(inp)

# Provided samples
sample = """\
4
2 2 2
QQ
.Q
2 2 2
QQ
..
3 3 3
.Q.
QQQ
.Q.
1 3 1
Q.Q
"""

expected_sample = """\
1
2
2
0
1
8
0
2
"""

assert run(sample) == expected_sample, "official samples"

# Minimum-size non-empty grid
assert run("""\
1
1 1 1
Q
""") == "1\n", "single balloon"

# Minimum-size empty grid
assert run("""\
1
1 1 1
.
""") == "0\n", "empty grid"

# One row, every balloon can be cleared by any one dart
assert run("""\
1
1 3 3
QQQ
""") == "3\n0\n0\n", "one-row boundary"

# Two diagonal balloons must both be popped, in either order
assert run("""\
1
2 2 2
Q.
.Q
""") == "0\n2\n", "diagonal balloons"

# Maximum-size all-balloon grid.
# A maximal matching must have exactly 12 darts.
max_grid = "12 20 20\n" + "\n".join(["Q" * 20] * 12) + "\n"
max_input = "1\n" + max_grid

max_matching_ordered = (
    factorial(20) // factorial(8) * factorial(12)
)

max_expected = "\n".join(
    ["0"] * 11 + [str(max_matching_ordered)]
) + "\n"

assert run(max_input) == max_expected, "maximum dense grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 x 1`với`Q`|`1`| Lưới không trống nhỏ nhất có thể | 
|`1 x 1`với`.`|`0`| Không có phi tiêu hợp pháp và xử lý bảng trống | 
|`1 x 3`với`QQQ`|`3, 0, 0`| Nhiều quả bóng bay trong một hàng và số lượng phi tiêu hữu ích tối đa | 
|`2 x 2`với bóng bay chéo |`0, 2`| Trình tự phi tiêu được sắp xếp và yêu cầu các cú đánh trực tiếp tạo thành một | 
|`12 x 20`tất cả`Q`|`0`thông qua số phi tiêu`11`, khi đó có giá trị lớn tại`12`| Kích thước trạng thái tối đa, số lượng chính xác tùy ý và`x!`yếu tố đặt hàng | 

## Vỏ cạnh 

Đối với một lưới trống, chẳng hạn như```
1
1 1 1
.
```trạng thái ban đầu`0`tồn tại ở mọi cột vì không có bong bóng nào trong bất kỳ mặt nạ cột nào. Đây là trạng thái không phi tiêu hợp lệ, nhưng vòng lặp đầu ra bắt đầu tại`x = 1`, nên không có gì được báo cáo. Kết quả là`0`. 

Đối với một hàng có chứa`QQQ`,```
1
1 3 3
QQQ
```DP chỉ có ba trạng thái ternary. Việc chụp cột đầu tiên, thứ hai hoặc thứ ba sẽ tạo trạng thái có một hàng được chọn và không có trạng thái chưa được giải quyết. Đây là ba vị trí một phi tiêu khác nhau. Các câu trả lời cuối cùng là`3, 0, 0`. 

Đối với bóng bay chéo,```
1
2 2 2
Q.
.Q
```quả phi tiêu đầu tiên có thể làm nổ quả bóng chéo, nhưng sau khi làm như vậy, quả bóng còn lại vẫn giữ nguyên vì cả hàng và cột của nó đều không bị xóa. Do đó, DP chỉ giữ trạng thái hai phi tiêu là thành công. Có hai lệnh phi tiêu có thể xảy ra, đưa ra`2`. 

Quá trình chuyển đổi tinh tế nhất xảy ra khi một cột chứa nhiều bong bóng và một trong số chúng được chọn. Giả sử trạng thái hiện tại có bong bóng chưa được giải quyết ở hàng 1 và không có thông tin trước đó về hàng 2, trong khi cột hiện tại chứa bong bóng ở cả hai hàng. Bắn hàng 1 sẽ loại bỏ bóng bay hàng 1 và cột ngay lập tức. Hàng 2 không được đánh dấu là chưa được giải quyết chỉ vì một quả bóng bay tồn tại ở đó, bởi vì quả bóng đó vừa bị thổi bay. Quá trình chuyển đổi chụp`base - pow3[r]`xử lý chính xác tình huống này. 

Khi`k > m`, câu trả lời cho mọi`x > m`nhất thiết là bằng không. Không có hai quả bóng bay trực tiếp nào có thể nằm chung một hàng, vì vậy nhiều nhất`m`phi tiêu có thể được sử dụng theo một trình tự hợp lệ. Việc thực hiện vẫn in chính xác`k`dòng, nhưng kết quả DP giai thừa chỉ được sử dụng cho`x <= m`; tất cả các giá trị lớn hơn được phát ra bằng 0.
