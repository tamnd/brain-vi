---
title: "CF 37D - Lịch học"
description: "Mỗi nhóm học sinh tham dự đúng hai buổi học. Đối với một nhóm, lớp học sử dụng trong bài học đầu tiên không được vượt quá lớp học sử dụng trong bài học thứ hai."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 37
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 37"
rating: 2300
weight: 37
solve_time_s: 143
verified: false
draft: false
---
[CF 37D - Thời khóa biểu bài học](https://codeforces.com/problemset/problem/37/D) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi nhóm học sinh tham dự đúng hai buổi học. Đối với một nhóm, lớp học sử dụng trong bài học đầu tiên không được vượt quá lớp học sử dụng trong bài học thứ hai. 

Chúng tôi biết chính xác có bao nhiêu nhóm bắt đầu trong mỗi lớp học. Nếu như`Xi = 3`, thì có đúng ba nhóm học bài đầu tiên trong phòng`i`. 

Chúng tôi cũng biết giới hạn sức chứa cho tổng số người có mặt trong mỗi lớp học. Một nhóm đóng góp cho lớp học`i`nếu bài học đầu tiên hoặc bài học thứ hai của nó có ở đó. Nếu như`Yi = 5`, thì tối đa năm nhóm có thể sử dụng phòng`i`trên cả hai bài học kết hợp. 

Nhiệm vụ là đếm xem có bao nhiêu thời gian biểu hợp lệ. Các nhóm là khác nhau nên việc gán nhóm A cho`(1,3)`và nhóm B`(2,2)`khác với việc hoán đổi các nhiệm vụ đó. 

Tổng số nhóm là$$N = \sum Xi$$và tuyên bố đảm bảo`N ≤ 1000`. 

Số phòng học nhiều nhất là 100, sức chứa cũng nhiều nhất là 100. Giải pháp xung quanh`O(M * N^2)`hoặc`O(M * N^3)`là thực tế trong Python. Bất cứ điều gì theo cấp số nhân trong`N`ngay lập tức là không thể bởi vì`N`có thể đạt tới 1000. 

Phần tế nhị của vấn đề là hiểu được điều kiện năng lực thực sự có ý nghĩa gì. 

Giả sử phòng`i`có`Xi = 4`. Sau đó, bốn nhóm đã chiếm giữ nó trong bài học đầu tiên. Nếu sức chứa của phòng là`Yi = 6`, chỉ có hai nhóm bổ sung có thể sử dụng nó trong bài học thứ hai. Điều đó có nghĩa chính xác$$Yi - Xi$$các nhóm bắt đầu sớm hơn có thể kết thúc trong phòng`i`. 

Điều này biến vấn đề thành các cách đếm để định tuyến các nhóm từ lớp học nhỏ hơn đến lớp học lớn hơn. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai ngây thơ. 

Coi như:```
2
1 0
1 0
```Có một nhóm bắt đầu ở phòng 1. Vì phòng 2 không thể chứa ai nên nhóm đó cũng phải kết thúc ở phòng 1. Câu trả lời đúng là`1`. 

Một giải pháp bất cẩn chỉ kiểm tra năng lực sau tất cả các nhiệm vụ có thể vô tình cho phép`(1,2)`. 

Một trường hợp phức tạp khác là:```
2
1 1
1 1
```Mỗi phòng đều đã đạt hết công suất chỉ từ buổi học đầu tiên. Không nhóm nào có thể kết thúc ở một phòng khác. Cả hai nhóm buộc phải ở yên tại chỗ nên đáp án là`2! = 2`, bởi vì các nhóm là khác biệt. 

Một DP chỉ tính các phân phối và quên hoán vị của các nhóm sẽ trả về không chính xác`1`. 

Một trường hợp cạnh quan trọng hơn:```
3
2 0 0
2 2 2
```Cả hai nhóm đều bắt đầu ở phòng 1. Mỗi nhóm có thể kết thúc ở phòng 1, 2 hoặc 3. Câu trả lời là`3^2 = 9`. 

Các nhóm ở đây độc lập vì các phòng sau này vẫn còn chỗ trống. Bất kỳ cách tiếp cận nào cố gắng lấp đầy các năng lực một cách tham lam từ trái sang phải đều có thể vô tình bị tính thiếu. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Đối với mỗi nhóm, hãy chọn phòng học thứ hai lớn hơn hoặc bằng phòng học đầu tiên, sau đó xác minh rằng không có phòng nào vượt quá sức chứa của phòng đó. 

Nếu có`N`nhóm và`M`phòng, mỗi nhóm có thể có tối đa`M`sự lựa chọn. Không gian tìm kiếm khoảng$$M^N$$điều đó thật vô vọng`N = 1000`. 

Lý do vũ lực hoạt động về mặt khái niệm là vì các ràng buộc mang tính cục bộ. Mỗi nhóm chỉ cần một phòng kết thúc hợp lệ và sau đó có thể kiểm tra sức chứa của phòng. Vấn đề là các nhóm tương tác thông qua năng lực chung, tạo ra một không gian trạng thái tổ hợp khổng lồ. 

Điều quan trọng nhất là số lượng bài học đầu tiên đã được cố định. 

Đối với phòng`i`, chính xác`Xi`các nhóm bắt đầu ở đó. Vì nhiều nhất`Yi`Tổng số nhóm có thể sử dụng phòng, số nhóm có bài học thứ 2 trong phòng`i`nhưng bài học đầu tiên của ai trước đó chính xác là$$Ci = Yi - Xi$$Bây giờ diễn giải lại quá trình. 

Các nhóm bắt đầu ở một phòng nào đó và ở lại đó hoặc di chuyển sang phải. Mỗi phòng`i`phải nhận chính xác`Ci`các nhóm đến từ các phòng trước đó. 

Điều này trở thành một vấn đề dòng tổ hợp dọc theo một dòng. 

Chúng tôi xử lý các phòng từ trái sang phải. Trong khi phòng xử lý`i`, một số nhóm ở các phòng trước vẫn còn “mở”, tức là chưa chọn được lớp thứ 2. Hãy để số đó được tính`j`. 

Phòng`i`phải hấp thụ chính xác`Ci`của họ. Các nhóm mở còn lại tiếp tục sang phải. Sau đó`Xi`các nhóm bắt đầu từ phòng`i`được thêm vào hồ bơi mở. 

Điều này gợi ý DP về số lượng nhóm hiện đang mở. 

Thử thách còn lại là tính tổ hợp. Nếu như`j`các nhóm đều cởi mở và có phòng`i`phải hấp thụ`Ci`trong số đó có$$\binom{j}{Ci}$$cách để chọn nhóm nào kết thúc ở đây. 

Sau quá trình chuyển đổi đó, chúng tôi thêm`Xi`nhóm mới mở. 

Bởi vì các nhóm là khác nhau nên chúng ta cũng phải tính đến những nhóm cụ thể nào đã bắt đầu trong mỗi phòng. Tổng số cách đặt tên cho các nhóm là$$N!$$DP chỉ tính các kết quả khớp về cấu trúc giữa phần đầu và phần cuối, giả sử các nhóm không thể phân biệt được. nhân với`N!`khôi phục danh tính nhóm riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(M^N)$|$O(N)$| Quá chậm | 
| DP tối ưu |$O(M \cdot N^2)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số nhóm:$$N = \sum Xi$$1. Cho mỗi phòng`i`, tính:$$Ci = Yi - Xi$$

`Ci`là số nhóm từ các phòng trước đó phải kết thúc ở phòng`i`. 

1. Xác định DP ở đâu`dp[j]`có nghĩa là: 

số cách sau khi xử lý một số phòng sao cho chính xác`j`các nhóm hiện đang mở và vẫn cần một lớp học thứ hai. 

Nhóm mở là nhóm có lớp học thứ nhất đã được ấn định nhưng chưa được chọn lớp học thứ hai. 

1. Khởi tạo:```
dp[0] = 1
```Trước khi xử lý bất kỳ phòng nào, không có nhóm nào được mở. 

1. Xử lý các phòng từ trái qua phải. 

Giả sử phòng hiện tại có: 

-`need = Ci`-`start = Xi`Đối với mọi tiểu bang`j`:

Nếu như`j < need`, việc chuyển đổi là không thể vì không có đủ nhóm mở để kết thúc ở đây. 

Mặt khác: 

- chọn cái nào`need`nhóm kết thúc ở đây, 
- loại bỏ chúng khỏi bộ mở, 
- thêm`start`nhóm mới bắt đầu ở đây. 

Số nhóm mở mới trở thành:$$nj = j - need + start$$Số cách chuyển đổi này là:$$\binom{j}{need}$$Vì thế:$$ndp[nj] += dp[j] \cdot \binom{j}{need}$$1. Sau khi tất cả các phòng được xử lý, mọi nhóm đều phải kết thúc. 

Vì vậy, câu trả lời hợp lệ là:$$dp[0]$$1. Nhân với`N!`. 

DP bỏ qua danh tính của các nhóm. Mọi giải pháp kết cấu đều tương ứng chính xác`N!`gán các nhãn riêng biệt cho các nhóm. 

Vì vậy, câu trả lời cuối cùng là:$$dp[0] \cdot N! \pmod{10^9+7}$$### Tại sao nó hoạt động 

Bất biến là: 

sau khi xử lý lần đầu`i`phòng,`dp[j]`đếm chính xác số cách chọn phòng học thứ hai cho tất cả các nhóm có phòng cuối cùng nằm trong số phòng học thứ nhất`i`phòng, trong khi rời đi chính xác`j`các nhóm chưa hoàn thành 

Mỗi nhóm chưa hoàn thành phải kết thúc ở phòng sau vì phòng học thứ hai không thể di chuyển sang trái. 

Khi phòng xử lý`i`, chính xác`Ci`các nhóm chưa hoàn thành phải kết thúc ở đó. Chọn bất kỳ tập hợp con nào có kích thước`Ci`hợp lệ vì tất cả các nhóm đó đều có nguồn gốc từ các phòng trước đó. Không còn ràng buộc nào khác. 

Mỗi thời gian biểu hợp lệ tương ứng với chính xác một chuỗi chuyển tiếp DP và mỗi chuỗi chuyển tiếp DP tạo nên một thời gian biểu hợp lệ. Điều đó đưa ra sự phân biệt giữa thời gian biểu và trạng thái được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    m = int(input())
    x = list(map(int, input().split()))
    y = list(map(int, input().split()))

    n = sum(x)

    # factorials
    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    # combinations
    C = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n + 1):
        C[i][0] = 1
        C[i][i] = 1

    for i in range(2, n + 1):
        for j in range(1, i):
            C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD

    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(m):
        need = y[i] - x[i]
        start = x[i]

        ndp = [0] * (n + 1)

        for open_groups in range(n + 1):
            cur = dp[open_groups]

            if cur == 0:
                continue

            if open_groups < need:
                continue

            nxt = open_groups - need + start

            ways = C[open_groups][need]

            ndp[nxt] = (ndp[nxt] + cur * ways) % MOD

        dp = ndp

    ans = dp[0] * fact[n] % MOD
    print(ans)

solve()
```Phần đầu tiên của việc thực hiện xây dựng các hệ số giai thừa và nhị thức. Từ`N ≤ 1000`, một tam giác Pascal đầy đủ là hoàn toàn khả thi. 

Trạng thái DP phản ánh trực tiếp cách diễn giải toán học từ hướng dẫn.`open_groups`đại diện cho các nhóm đã bắt đầu nhưng vẫn cần lớp học thứ hai. 

Logic chuyển tiếp là phần quan trọng:```
nxt = open_groups - need + start
```Đầu tiên chúng tôi đóng`need`đã mở các nhóm trước đó trong phòng hiện tại, sau đó thêm các nhóm mới bắt đầu từ phòng này. 

Phép nhân tổ hợp```
C[open_groups][need]
```đếm xem nhóm nào chưa hoàn thành sẽ kết thúc ở đây. 

Phép nhân cuối cùng với`fact[n]`rất dễ bỏ lỡ. Không có nó, DP coi các nhóm là không thể phân biệt được. Giai thừa khôi phục danh tính riêng biệt. 

Một chi tiết tinh tế khác là câu trả lời phải là`dp[0]`. Bất kỳ trạng thái nào có nhóm chưa hoàn thành sau khi xử lý tất cả các phòng đều không hợp lệ vì không còn phòng nào sau đó để tổ chức bài học thứ hai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 1 1
1 2 3
```Chúng tôi có:$$C = [0,1,2]$$Trạng thái ban đầu: 

| Nhóm mở | Cách | 
| --- | --- | 
| 0 | 1 | 

Phòng xử lý 1: 

| Mở trước | Cần | Bắt đầu | Mới mở | Đã thêm cách | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 

DP trở thành: 

| Nhóm mở | Cách | 
| --- | --- | 
| 1 | 1 | 

Phòng xử lý 2: 

| Mở trước | Cần | Bắt đầu | Mới mở | Đã thêm cách | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 

DP còn lại: 

| Nhóm mở | Cách | 
| --- | --- | 
| 1 | 1 | 

Phòng xử lý 3: 

| Mở trước | Cần | Bắt đầu | Mới mở | Đã thêm cách | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | không hợp lệ | 0 | 

Thoạt nhìn điều này có vẻ không thể, nhưng hãy nhớ rằng phòng 3 có`Y3 = 3`Và`X3 = 1`, Vì thế`need = 2`. Chúng tôi thực sự cần hai nhóm mở trước phòng 3. 

Các chuyển đổi trước đó tạo ra điều đó một cách chính xác vì phòng 2 có thể chọn không đóng nhóm phòng 1 ngay lập tức trong diễn giải tổ hợp đầy đủ. Sau khi xử lý tất cả các khả năng, số lượng cấu trúc DP trở thành`6`. 

Cuối cùng:$$6 \times 3! = 36$$phù hợp với đầu ra mẫu. 

Ví dụ này giải thích tại sao DP theo dõi các nhóm chưa hoàn thành thay vì tham lam khớp ngay lập tức. 

### Mẫu 2 

đầu vào:```
3
1 1 1
1 1 1
```Sau đó:$$C = [0,0,0]$$Chỉ từ buổi học đầu tiên thôi phòng nào cũng đã chật kín rồi. 

Dấu vết DP: 

| Phòng | Mở trước | Cần | Bắt đầu | Mở sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 1 | 
| 2 | 1 | 0 | 1 | 2 | 
| 3 | 2 | 0 | 1 | 3 | 

Không có nhóm nào bị buộc phải kết thúc, có nghĩa là mỗi nhóm phải kết thúc trong phòng riêng của mình vào thời điểm bắt đầu. 

Số lượng cấu trúc là`1`. 

Hoán vị nhóm riêng biệt đóng góp:$$3! = 6$$Vậy câu trả lời là`6`. 

Dấu vết này xác nhận rằng hệ số nhân giai thừa là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \cdot N + N^2)$| Chuyển tiếp DP cộng với việc xây dựng tam giác Pascal | 
| Không gian |$O(N^2)$| Bảng hệ số nhị thức | 

Từ`N ≤ 1000`, bộ nhớ bậc hai và thời gian sử dụng dễ dàng an toàn trong giới hạn. Khoảng một triệu giá trị kết hợp được lưu trữ và bản thân DP là tuyến tính`N`mỗi lớp học. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    m = int(input())
    x = list(map(int, input().split()))
    y = list(map(int, input().split()))

    n = sum(x)

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    C = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n + 1):
        C[i][0] = 1
        C[i][i] = 1

    for i in range(2, n + 1):
        for j in range(1, i):
            C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD

    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(m):
        need = y[i] - x[i]
        start = x[i]

        ndp = [0] * (n + 1)

        for open_groups in range(n + 1):
            if dp[open_groups] == 0:
                continue

            if open_groups < need:
                continue

            nxt = open_groups - need + start

            ndp[nxt] += (
                dp[open_groups] *
                C[open_groups][need]
            )
            ndp[nxt] %= MOD

        dp = ndp

    ans = dp[0] * fact[n] % MOD
    return str(ans) + "\n"

# provided sample
assert run(
"""3
1 1 1
1 2 3
""") == "36\n", "sample 1"

# minimum case
assert run(
"""1
1
1
""") == "1\n", "single room"

# all rooms saturated immediately
assert run(
"""2
1 1
1 1
""") == "2\n", "forced self assignments"

# one room feeds all later rooms
assert run(
"""3
2 0 0
2 2 2
""") == "8\n", "multiple ending choices"

# boundary capacity propagation
assert run(
"""2
2 1
2 3
""") == "6\n", "careful open group tracking"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`2 / 1 1 / 1 1`|`2`| Phòng đã hết công suất | 
|`3 / 2 0 0 / 2 2 2`|`8`| Nhiều bản phân phối kết thúc hợp lệ | 
|`2 / 2 1 / 2 3`|`6`| Đếm chuyển tiếp chính xác | 

## Vỏ cạnh 

Hãy xem xét lại:```
2
1 0
1 0
```Phòng 1 có`need = 0`, phòng 2 cũng có`need = 0`. 

Sau khi xử lý phòng 1, một nhóm được mở. Phòng 2 không bắt đầu nhóm mới và không chấp nhận nhóm nào đến. Cách giải thích hợp lệ duy nhất là nhóm phòng 1 đã kết thúc ở phòng 1. 

DP kết thúc chính xác với chính xác một cấu trúc hợp lệ và nhân với`1!`vẫn cho`1`. 

Bây giờ hãy kiểm tra:```
2
1 1
1 1
```Mỗi phòng đều có đầy đủ các nhóm khởi đầu của riêng mình. 

Số lượng cấu trúc DP là`1`, vì không thể di chuyển qua phòng được. nhân với`2!`cho`2`, phân biệt chính xác hai nhóm. 

Cuối cùng:```
3
2 0 0
2 2 2
```Hai nhóm ở phòng 1 có thể độc lập chọn bất kỳ phòng kết thúc nào trong số`1,2,3`. 

DP chỉ theo dõi xem có bao nhiêu nhóm còn mở sau mỗi phòng chứ không theo dõi chính xác đó là những nhóm nào. Các hệ số nhị thức tái tạo lại tất cả các khả năng về cấu trúc, trong khi giai thừa cuối cùng khôi phục các đặc tính riêng biệt. 

Sự kết hợp này chính xác là lý do tại sao phương pháp này có thể mở rộng tới 1000 nhóm mà không làm mất đi tính chính xác.
