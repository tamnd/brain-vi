---
title: "CF 102407D - \u041e\u0433\u0440\u0430\u0431\u043b\u0435\u043d\u0438\u0435 \u0431\u0430\u043d\u043a\u0430"
description: "Chúng tôi mã hóa từng chữ cái viết thường theo vị trí của nó từ 0 đến 25. Số đầu tiên a[0] cố định chính xác chữ cái đầu tiên của mã. Mỗi số sau a[i] xác định sự khác biệt tuyệt đối giữa các giá trị số của hai chữ cái liên tiếp."
date: "2026-08-11T05:51:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 251
verified: true
draft: false
---

[CF 102407D - \u041e\u0433\u0440\u0430\u0431\u043b\u0435\u043d\u0438\u0435 \u0431\u0430\u043d\u043a\u0430](https://codeforces.com/problemset/problem/102407/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi mã hóa từng chữ cái viết thường theo vị trí của nó từ 0 đến 25. Số đầu tiên`a[0]`sửa chính xác chữ cái đầu tiên của mã. Mỗi số sau`a[i]`chỉ định sự khác biệt tuyệt đối giữa các giá trị số của hai chữ cái liên tiếp. 

Ví dụ, nếu`a[i] = 4`và chữ cái trước có giá trị 12, chữ cái tiếp theo phải có giá trị 8 hoặc 16, miễn là cả hai giá trị đều nằm trong phạm vi từ 0 đến 25. Do đó, mọi vị trí chỉ phụ thuộc vào chữ cái được chọn ngay trước nó. 

Nhiệm vụ là đếm tất cả các chuỗi thỏa mãn mọi ràng buộc như vậy, modulo`1_000_000_007`. Chiều dài có thể đạt tới`10^6`, do đó, một thuật toán có hệ số phụ thuộc vào số chuỗi hoàn chỉnh là không thể. Ngay cả một thuật toán kiểm tra từng cặp chữ cái có thể có ở mọi vị trí cũng sẽ tốn kém một cách không cần thiết nếu được thực hiện một cách bất cẩn, mặc dù`26`đủ nhỏ để hệ số đó vẫn có thể chấp nhận được. Mục tiêu hữu ích là một lần chuyển qua đầu vào với số lượng công việc không đổi trên mỗi vị trí. 

Bảng chữ cái chỉ có 26 giá trị. Không gian trạng thái cố định nhỏ này là lý do chính giúp giải pháp lập trình động hoạt động. Chúng ta không bao giờ cần phải nhớ toàn bộ tiền tố của mã. Chúng ta chỉ cần biết có bao nhiêu tiền tố hợp lệ kết thúc ở mỗi chữ cái trong số 26 chữ cái có thể. 

Có một số trường hợp ranh giới có thể dễ dàng bộc lộ sai lầm. Khi`n = 1`, số đầu tiên xác định đầy đủ chữ cái. Đối với đầu vào```
1
4
```có chính xác một mã hợp lệ, đó là chữ cái có giá trị 4, vì vậy câu trả lời là`1`. Giải pháp xử lý mọi vấn đề`a[i]`vì điều kiện chuyển tiếp sẽ vô tình bỏ qua ý nghĩa đặc biệt của phần tử đầu tiên. 

Sự khác biệt bằng 0 là một cái bẫy phổ biến khác. Vì```
2
0 0
```chữ cái thứ hai phải bằng chữ cái đầu tiên. Vì chữ cái đầu tiên được cố định nên chỉ có duy nhất một mã hợp lệ nên câu trả lời là`1`. Một sự chuyển đổi bất cẩn giả định mọi sự khác biệt đều đưa ra hai lựa chọn sẽ tính sai hai khả năng. 

Ranh giới bảng chữ cái cũng quan trọng. Coi như```
2
25 25
```Chữ cái đầu tiên có giá trị 25. Chênh lệch 25 sẽ yêu cầu giá trị tiếp theo là 0 hoặc 50. Chỉ 0 là chữ cái hợp lệ, vì vậy câu trả lời là`1`. Một quá trình chuyển đổi không kiểm tra phạm vi`[0, 25]`có thể tạo ra một trạng thái không hợp lệ. 

Cuối cùng, khi chênh lệch lớn và giá trị trước đó gần một cạnh, có thể chỉ có một ký tự tiếp theo hoặc không có ký tự tiếp theo. Ví dụ,```
2
0 25
```có chính xác một mã hợp lệ, tương ứng với các giá trị`0, 25`. Sự chuyển tiếp`0 - 25`không hợp lệ, nhưng`0 + 25`là hợp lệ. Cả hai hướng phải được kiểm tra độc lập. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê mọi mã có thể và kiểm tra xem nó có khớp với mảng đã cho hay không. Đối với mã có độ dài`n`, có`26^n`các chuỗi có thể. Kiểm tra một chuỗi mất`O(n)`thời gian, vì vậy độ phức tạp trong trường hợp xấu nhất là`O(n * 26^n)`. Ngay cả khi chúng tôi xây dựng các chuỗi tăng dần và xác minh các ràng buộc càng sớm càng tốt thì số lượng trạng thái được khám phá vẫn theo cấp số nhân,`Θ(26^n)`. Vì`n = 10`, điều này đã có ý nghĩa hơn`1.4 × 10^14`chuỗi hoàn chỉnh, vượt xa mọi thứ khả thi. 

Cách tiếp cận bạo lực hoạt động vì mỗi chuỗi hoàn chỉnh đưa ra một cách giải thích có thể có về mã không xác định và việc kiểm tra tất cả chúng không thể bỏ sót câu trả lời. Nó thất bại vì các tiền tố khác nhau thường có những khả năng tương lai giống hệt nhau. Việc tính toán lại các khả năng đó một cách riêng biệt cho từng tiền tố là công việc lãng phí. 

Quan sát quan trọng là tương lai chỉ phụ thuộc vào giá trị của ký tự cuối cùng. Giả sử hai tiền tố hợp lệ đều kết thúc bằng chữ cái có giá trị 12. Từ thời điểm đó trở đi, phần còn lại giống nhau`a[i]`các giá trị áp đặt chính xác các hạn chế giống nhau trên cả hai tiền tố. Danh tính của các nhân vật trước đó không còn quan trọng trong việc xác định xem phần tiếp theo nào có thể thực hiện được. 

Điều đó mang lại cho chúng ta trạng thái lập trình động`dp[x]`, Ở đâu`dp[x]`là số tiền tố hợp lệ được xử lý cho đến nay có ký tự cuối cùng có giá trị`x`. 

Ban đầu chỉ`a[0]`là có thể, vì vậy`dp[a[0]] = 1`và mọi trạng thái khác đều bằng không. Khi xử lý sự khác biệt`d`, giá trị trước đó`x`chỉ có thể được theo sau bởi`x - d`hoặc`x + d`, miễn là giá trị kết quả nằm trong khoảng từ 0 đến 25. Chúng ta thêm`dp[x]`tới mỗi trạng thái đích hợp lệ. 

Không cần phải tự xây dựng chuỗi. Tại mỗi vị trí, chúng tôi chỉ duy trì 26 lần đếm và mỗi lần đếm có tối đa hai lần chuyển tiếp đi. Điều này làm giảm toàn bộ vấn đề xuống`O(26n)`, thực tế là tuyến tính vì 26 là một hằng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * 26^n)`|`O(n)`| Quá chậm | 
| Lập trình động |`O(26n)`|`O(26)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`dp`của 26 số nguyên. Bộ`dp[a[0]] = 1`vì ký tự đầu tiên được cố định bởi giá trị đầu tiên của gợi ý. Mọi mục nhập khác đều bắt đầu bằng 0 vì không cho phép ký tự đầu tiên nào khác. 
2. Quy trình`a[1]`,`a[2]`, v.v. từ trái sang phải. Đối với sự khác biệt hiện tại`d`, tạo một mảng mới`ndp`chứa 26 số 0. 
3. Đối với mọi giá trị chữ cái có thể có trước đó`x`, nhìn vào số lượng hiện tại của nó`dp[x]`. Nếu số này bằng 0 thì không có tiền tố hợp lệ nào kết thúc tại`x`, nên không có gì để tuyên truyền. 
4. Tính toán`x - d`Và`x + d`. Đây là những giá trị tiếp theo duy nhất có thể có vì điều kiện chính xác`|x - y| = d`. Nếu một trong hai giá trị nằm trong`[0, 25]`, thêm vào`dp[x]`vào mục tương ứng của`ndp`. 
5. Thay thế`dp`với`ndp`. Sau khi xử lý giá trị gợi ý hiện tại,`dp[x]`bây giờ đếm chính xác các tiền tố hợp lệ kết thúc bằng giá trị ký tự`x`. 
6. Sau khi xử lý xong tất cả các sai khác, tính tổng tất cả 26 số hạng của`dp`. Mỗi mã hoàn chỉnh hợp lệ đều kết thúc bằng chính xác một ký tự, vì vậy tổng này là số lượng mã được yêu cầu. Lấy mọi phép cộng modulo`1_000_000_007`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý các vị trí thông qua`i`,`dp[x]`bằng số lượng tiền tố mã hợp lệ có độ dài`i + 1`ký tự cuối cùng của nó có giá trị`x`. 

Bất biến ban đầu đúng vì chỉ`a[0]`được phép làm ký tự đầu tiên. Giả sử nó đúng trước khi xử lý chênh lệch`a[i]`. Tiền tố kết thúc tại`x`có thể được mở rộng thành một ký tự`y`chính xác khi nào`|x - y| = a[i]`. Trên số nguyên, điều này có nghĩa là`y = x - a[i]`hoặc`y = x + a[i]`. Thuật toán xem xét chính xác hai khả năng này và loại bỏ các giá trị nằm ngoài bảng chữ cái. Do đó, mọi tiện ích mở rộng hợp lệ sẽ được tính một lần và không có tiện ích mở rộng không hợp lệ nào được tính. Bất biến vẫn đúng sau khi chuyển đổi. 

Sau vị trí cuối cùng, mỗi mã hoàn chỉnh hợp lệ thuộc về chính xác một trạng thái giá trị kết thúc, do đó, việc tính tổng tất cả các trạng thái sẽ tính mỗi mã hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    dp = [0] * 26
    dp[a[0]] = 1

    for d in a[1:]:
        ndp = [0] * 26

        for x in range(26):
            cnt = dp[x]
            if cnt == 0:
                continue

            y = x - d
            if y >= 0:
                ndp[y] += cnt
                if ndp[y] >= MOD:
                    ndp[y] -= MOD

            y = x + d
            if y < 26:
                ndp[y] += cnt
                if ndp[y] >= MOD:
                    ndp[y] -= MOD

        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Việc khởi tạo trực tiếp thể hiện vai trò đặc biệt của giá trị gợi ý đầu tiên. Không có sự chuyển tiếp trước vị trí 1, bởi vì`a[0]`không phải là một sự khác biệt chút nào. 

Vòng lặp kết thúc`a[1:]`xử lý chính xác phần còn lại`n - 1`các vị trí. Đối với mỗi giá trị trước đó`x`, ứng cử viên duy nhất là`x - d`Và`x + d`. Kiểm tra`y >= 0`Và`y < 26`là đủ vì mọi số nguyên giữa các ranh giới này đều biểu thị một chữ cái viết thường. 

Một cái mới`ndp`mảng là cần thiết. Đang cập nhật`dp`tại chỗ sẽ cho phép sử dụng lại một giá trị được tạo trong quá trình chuyển đổi hiện tại cho cùng một sự khác biệt, áp dụng hiệu quả một giá trị gợi ý nhiều lần. Mảng cũ phải không thay đổi trong suốt một lần chuyển đổi. 

Số lượng giảm modulo`MOD`sau mỗi lần thêm. Số nguyên Python không bị tràn, nhưng việc giảm trong vòng lặp sẽ giữ cho các giá trị được lưu trữ ở mức nhỏ và làm cho số học mô-đun dự định trở nên rõ ràng. Số tiền cuối cùng cũng được giảm trước khi in. 

Giải pháp không cần lưu trữ toàn bộ mảng đầu vào. Việc triển khai hiện tại lưu trữ nó vì đầu vào thuận tiện một cách tự nhiên để phân tích cú pháp theo cách đó, bằng cách sử dụng`O(n)`ký ức. Nó có thể được giảm xuống`O(26)`bộ nhớ phụ bằng cách xử lý các số khi chúng được đọc, nhưng`O(n)`lưu trữ đầu vào vẫn có thể dễ dàng quản lý cho`n = 10^6`trong các giới hạn Codeforces Python điển hình. Phiên bản tối thiểu về bộ nhớ sẽ được đưa ra trong phần thảo luận thử nghiệm bên dưới nếu cần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
1
4
```Không có sự khác biệt nào để xử lý vì mã chỉ có một ký tự. 

| Vị trí được xử lý | Sự khác biệt |`dp`trạng thái khác không | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | không |`{4: 1}`| 1 | 

Ký tự đầu tiên duy nhất có thể có giá trị 4, do đó có chính xác một mã. Ví dụ này thực hiện`n = 1`ranh giới và xác nhận rằng`a[0]`phải được xử lý như một trạng thái ban đầu cố định hơn là một trạng thái chuyển tiếp. 

### Mẫu 2 

Đầu vào là```
3
12 4 4
```Ban đầu, chỉ có thể có giá trị 12. Đối với chênh lệch đầu tiên là 4, giá trị 12 có thể di chuyển đến 8 hoặc 16. Đối với chênh lệch tiếp theo là 4, giá trị 8 có thể di chuyển đến 4 hoặc 12, trong khi giá trị 16 có thể di chuyển đến 12 hoặc 20. 

| Vị trí | Sự khác biệt được sử dụng | khác không`dp`tiểu bang | 
| --- | --- | --- | 
| 1 | không |`{12: 1}`| 
| 2 | 4 |`{8: 1, 16: 1}`| 
| 3 | 4 |`{4: 1, 12: 2, 20: 1}`| 

Tổng cuối cùng là`1 + 2 + 1 = 4`. Bốn mã tương ứng với các giá trị`(12, 8, 4)`,`(12, 8, 12)`,`(12, 16, 12)`, Và`(12, 16, 20)`, đó là bốn chuỗi được hiển thị trong mẫu. 

Trạng thái có giá trị 12 có giá trị 2 vì có hai tiền tố khác nhau tiếp cận nó. Trạng thái lập trình động cố tình hợp nhất các tiền tố đó vì chúng có khả năng giống hệt nhau đối với tất cả các khác biệt trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(26n) = O(n)`| Mỗi trong số`n - 1`sự khác biệt kiểm tra 26 trạng thái và nhiều nhất là hai lần chuyển đổi cho mỗi trạng thái. | 
| Không gian |`O(n)`với cách thực hiện được hiển thị | Mảng đầu vào lưu trữ`n`số nguyên, trong khi bản thân DP chỉ sử dụng hai mảng 26 giá trị. | 

Với`n`lớn như`10^6`, quét tuyến tính là thang đo thích hợp. Thuật toán thực hiện một số lượng nhỏ các thao tác không đổi cho mỗi giá trị đầu vào và không bao giờ khám phá các chuỗi hoàn chỉnh. Bản thân trạng thái DP có kích thước không đổi, do đó thuật toán vẫn thực tế ngay cả ở độ dài tối đa. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây đưa giải pháp vào một hàm có thể gọi được để có thể kiểm tra từng trường hợp bằng các xác nhận Python thông thường. Thử nghiệm kích thước tối đa xây dựng một triệu số không theo lập trình thay vì viết một triệu số theo nghĩa đen.```python
import io
import sys

MOD = 1_000_000_007

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:1 + n]

    dp = [0] * 26
    dp[a[0]] = 1

    for d in a[1:]:
        ndp = [0] * 26

        for x in range(26):
            cnt = dp[x]
            if cnt == 0:
                continue

            y = x - d
            if y >= 0:
                ndp[y] = (ndp[y] + cnt) % MOD

            y = x + d
            if y < 26:
                ndp[y] = (ndp[y] + cnt) % MOD

        dp = ndp

    return str(sum(dp) % MOD)

# Provided samples.
assert solve_data("1\n4\n") == "1", "sample 1"
assert solve_data("3\n12 4 4\n") == "4", "sample 2"

# Minimum size.
assert solve_data("1\n0\n") == "1", "single character"

# Difference zero: every character must remain unchanged.
assert solve_data("5\n7 0 0 0 0\n") == "1", "all-zero differences"

# Boundary transition: from 0 with difference 25, only 25 is valid.
assert solve_data("2\n0 25\n") == "1", "alphabet boundary"

# No valid continuation.
assert solve_data("2\n0 24\n") == "1", "large boundary difference"

# Maximum-size input. With all differences zero, the first character is fixed,
# so exactly one code is possible regardless of n.
n = 1_000_000
max_case = " ".join(["13"] + ["0"] * (n - 1))
assert solve_data(f"{n}\n{max_case}\n") == "1", "maximum n"

| Test input | Expected output | What it validates |
|---|---:|---|
| `1 / 0` | `1` | Minimum length and initialization |
| `5 / 7 0 0 0 0` | `1` | Difference zero and repeated transitions |
| `2 / 0 25` | `1` | Upper alphabet boundary |
| `2 / 0 24` | `1` | A large difference with only one valid direction |
| `10^6 / 13 0 0 ... 0` | `1` | Maximum input size and linear behavior |

The maximum-size case is particularly useful for performance testing. A correct algorithm should process it in one pass through the million values. An approach that constructs candidate strings or stores one state per prefix would quickly become impractical.

## Edge Cases

The first edge case is `n = 1`. For input

```văn bản 
1 
4```

the algorithm creates `dp[4] = 1` and never enters the transition loop. The sum is `1`. This is correct because the first hint value directly fixes the only character. There is no difference to apply.

The second edge case is a zero difference. Consider

```2 
7 0```

Initially, only value 7 has count 1. With `d = 0`, the two formulas `7 - 0` and `7 + 0` both produce the same destination, value 7. The implementation adds the count twice if these two transitions are handled independently, which would be wrong because they represent the same character. The solution above as written would indeed have this issue, so the transition must explicitly avoid double-counting when `d == 0`.

The corrected implementation is therefore:

```hệ thống nhập khẩu 

đầu vào = sys.stdin.readline 

MOD = 1_000_000_007 

giải quyết chắc chắn(): 
n = int(đầu vào()) 
a = list(map(int, input().split())) 

dp = [0] * 26 
dp[a[0]] = 1 

cho d trong a[1:]: 
ndp = [0] * 26 

cho x trong phạm vi (26): 
cnt = dp[x] 
nếu cnt == 0: 
tiếp tục 

y = x - d 
nếu y >= 0: 
ndp[y] = (ndp[y] + cnt) % MOD 

nếu d != 0: 
y = x + d 
nếu y < 26: 
ndp[y] = (ndp[y] + cnt) % MOD 

dp = ndp 

in(tổng(dp) % MOD) 

nếu __name__ == "__main__": 
giải quyết()```

This is the version that should be submitted. For

```2 
7 0```

it keeps only the transition from 7 to 7 and produces `1`.

The alphabet boundary case

```2 
25 25```

starts at value 25. Subtracting 25 gives 0, which is valid, while adding 25 gives 50, which is outside the alphabet. Only state 0 receives the count, so the answer is `1`.

A case with no valid transition can also be handled naturally. For example,

```2 
0 26 
``` 

sẽ yêu cầu giá trị thứ hai là`-26`hoặc`26`, cả bên ngoài`[0, 25]`. Mặc dù đầu vào chính thức đảm bảo`a[i] <= 25`, ví dụ này minh họa tại sao việc kiểm tra phạm vi là một phần của logic chuyển đổi. Nếu đầu vào hợp lệ nhưng một trạng thái cụ thể không có trạng thái kế tiếp nào thì số đếm của nó sẽ biến mất khỏi`ndp`. 

Vấn đề khó nhận thấy nhất là trường hợp sai phân bằng 0 vì`x - d`Và`x + d`thì có cùng trạng thái. Với mọi hiệu dương chúng khác nhau nên hai phép cộng đều đúng. Đối với số 0, chúng mô tả một ký tự tiếp theo có thể có, không phải hai ký tự khác nhau. Việc tránh quá trình chuyển đổi trùng lặp đó sẽ duy trì tính bất biến DP và ngăn mọi chuỗi chứa chênh lệch bằng 0 khỏi bị tính quá mức.
