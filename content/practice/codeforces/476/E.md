---
title: "CF 476E - Dreamoon và Dây"
description: "Chúng ta bắt đầu với một chuỗi s. Chúng ta có thể xóa chính xác x ký tự, giữ nguyên các ký tự còn lại theo thứ tự ban đầu. Chuỗi kết quả là một chuỗi con của s. Bên trong chuỗi kết quả đó, chúng tôi tìm kiếm càng nhiều lần xuất hiện không chồng chéo của mẫu p càng tốt."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 476
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 272 (Div. 2)"
rating: 2200
weight: 476
solve_time_s: 345
verified: true
draft: false
---

[CF 476E - Dreamoon và Dây](https://codeforces.com/problemset/problem/476/E) 

**Đánh giá:** 2200 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 5 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi`s`. Chúng tôi có thể xóa chính xác`x`ký tự, giữ nguyên các ký tự còn lại theo thứ tự ban đầu. Chuỗi kết quả là một chuỗi con của`s`. 

Bên trong chuỗi kết quả đó, chúng tôi tìm kiếm càng nhiều lần xuất hiện **không chồng chéo** của một mẫu`p`càng tốt. Đối với mỗi lần xóa`x`từ`0`ĐẾN`|s|`, chúng ta phải xuất ra số lần xuất hiện tối đa có thể đạt được. 

Khó khăn chính là số lần xuất hiện được tính sau khi xóa. Một bản sao của`p`không cần phải xuất hiện dưới dạng chuỗi con của chuỗi gốc. Chúng ta có thể xóa các ký tự ở giữa và làm cho các ký tự ở xa trở nên liền kề. 

Chiều dài của`s`nhiều nhất là 2000, trong khi`p`có độ dài tối đa là 500. Giải pháp bậc ba đã tiềm ẩn nhiều rủi ro và bất cứ điều gì gần với việc liệt kê tất cả các dãy con đều hoàn toàn không thể thực hiện được. Một giải pháp xung quanh`O(n²)`hoặc`O(n² + nm)`là mục tiêu phù hợp. 

Một số tình huống rất dễ xảy ra sai sót. 

Coi như:```
s = "aa"
p = "aa"
```Với việc xóa bằng 0, chúng tôi có một lần xuất hiện. Với một lần xóa, chúng tôi chỉ có`"a"`, do đó câu trả lời trở thành số không. Một lỗi phổ biến là chỉ tính toán số lần xóa tối thiểu cần thiết cho một lần xuất hiện và kết luận rằng mọi số lần xóa lớn hơn cũng có thể xảy ra. Điều đó là sai ở đây. 

Một trường hợp tế nhị khác là:```
s = "abca"
p = "aa"
```Mẫu này không phải là một chuỗi con của`s`, nhưng nó có thể được hình thành bằng cách xóa`"bc"`. Bất kỳ giải pháp nào chỉ tìm kiếm chuỗi con của chuỗi gốc đều bỏ lỡ các câu trả lời hợp lệ. 

Cạm bẫy thứ ba xuất hiện khi các lần xuất hiện có chung các ký tự trong chuỗi gốc:```
s = "aaaaa"
p = "aa"
```Các lần xuất hiện trong chuỗi cuối cùng phải không chồng chéo. Lựa chọn vị trí`(1,2)`Và`(2,3)`là bất hợp pháp vì cùng một ký tự tham gia vào cả hai bản sao. Lập trình động phải thực thi các khoảng rời rạc. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi chuỗi con của`s`, đếm xem có bao nhiêu bản sao không chồng lên nhau`p`xuất hiện và cập nhật câu trả lời cho số lần xóa tương ứng. Vì một chuỗi có độ dài 2000 có`2^2000`tiếp theo, điều này là vô vọng. 

Quan sát tiếp theo là một lần xuất hiện của`p`ở chuỗi cuối cùng tương ứng với việc chọn vị trí trong`s`câu thần chú đó`p`như một sự tiếp nối. Giả sử vị trí được chọn đầu tiên là`l`và vị trí được chọn cuối cùng là`r`. Bên trong khoảng`[l, r]`, mọi ký tự không được sử dụng trong lần xuất hiện đó đều phải bị xóa. Số lần xóa bắt buộc là$$(r-l+1)-|p|.$$Nếu chúng ta biết khoảng thời gian rẻ nhất đại diện cho một lần xuất hiện thì vấn đề bắt đầu giống như việc lập lịch trình khoảng thời gian. 

Đối với mọi vị trí xuất phát`i`, chúng tôi tính toán vị trí kết thúc sớm nhất có thể`end[i]`như vậy`p`có thể được so khớp như một dãy con bắt đầu tại`i`. Nếu một khoảng như vậy tồn tại, việc chọn nó sẽ đóng góp một lần xuất hiện và buộc$$(end[i]-i+1)-|p|$$việc xóa. 

Bây giờ vấn đề trở thành: 

Chọn một số khoảng thời gian xuất hiện không chồng chéo. Giảm thiểu số lần xóa bắt buộc cần thiết để có được chính xác`k`lần xuất hiện. 

Một khi đã biết mức tối thiểu đó, câu trả lời cuối cùng sẽ trở nên dễ dàng. Các ký tự nằm ngoài tất cả các khoảng đã chọn đều không liên quan. Chúng có thể được giữ lại hoặc xóa một cách tự do. Điều này chuyển đổi DP xóa tối thiểu thành câu trả lời cho mỗi lần xóa chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n)`|`O(n)`| Quá chậm | 
| DP tối ưu với chuyển tiếp theo khoảng thời gian |`O(n² + nm)`|`O(n²)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước bảng lần xuất hiện tiếp theo cho`s`. 

Đối với mỗi vị trí và mỗi chữ cái, hãy lưu lần xuất hiện đầu tiên của chữ cái đó tại hoặc sau vị trí đó. Điều này cho phép khớp chuỗi con trong thời gian không đổi cho mỗi ký tự của`p`. 
2. Đối với mọi vị trí`i`TRONG`s`, tính khoảng thời gian sớm nhất biểu thị một lần xuất hiện của`p`. 

Nếu như`s[i]`không bằng`p[0]`, không có sự kiện nào có thể bắt đầu từ đó. 

Ngược lại, hãy khớp một cách tham lam các ký tự còn lại của`p`sử dụng bảng lần xuất hiện tiếp theo. Nếu khớp thành công và kết thúc ở vị trí`r`, cửa hàng:```
end[i] = r
cost[i] = (r - i + 1) - m
```Ở đâu`m = |p|`. 
3. Hãy để`dp[pos][k]`là số lần xóa bắt buộc tối thiểu cần thiết sau khi xử lý tiền tố kết thúc trước`pos`, trong khi tạo chính xác`k`lần xuất hiện. 

Khởi tạo:```
dp[0][0] = 0
```4. Xử lý các vị trí từ trái qua phải. 

Tại vị trí`i`, có hai lựa chọn. 

Bỏ qua vị trí:```
dp[i+1][k] = min(dp[i+1][k], dp[i][k])
```Lấy khoảng thời gian xuất hiện bắt đầu từ`i`:```
dp[end[i]+1][k+1]
    = min(dp[end[i]+1][k+1],
          dp[i][k] + cost[i])
```Nhảy trực tiếp tới`end[i]+1`đảm bảo rằng các lần xuất hiện được chọn là rời rạc. 
5. Sau tất cả các chuyển đổi,`dp[n][k]`bằng với số lần xóa bắt buộc tối thiểu cần thiết cho chính xác`k`lần xuất hiện. 
6. Chuyển những giá trị tối thiểu này thành câu trả lời cho mỗi lần xóa. 

Nếu như`dp[n][k] = d`, sau đó:```
mandatory deletions = d
maximum deletions preserving k copies = n - k*m
```Mỗi lần xóa được tính trong khoảng thời gian```
[d, n - k*m]
```là có thể đạt được. 

Đối với mỗi lần xóa như vậy, hãy cập nhật câu trả lời bằng`k`. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện được chọn tương ứng với một khoảng trong đó phần bên trong của nó phải được làm sạch tất cả các ký tự không được sử dụng trong trận đấu. DP tích lũy rõ ràng những thao tác xóa bắt buộc đó và cấm chồng chéo bằng cách nhảy qua phần cuối của khoảng thời gian. 

Bất kỳ ký tự nào nằm ngoài khoảng thời gian xuất hiện đã chọn đều không ảnh hưởng đến việc các lần xuất hiện đó có tồn tại hay không. Những ký tự như vậy có thể được giữ hoặc xóa một cách độc lập. Do đó, DP chỉ cần giảm thiểu việc xóa bắt buộc. Khi những điều đó đã được sửa, mỗi lần xóa lớn hơn sẽ có giá trị lên tới`n - km`có thể đạt được bằng cách xóa các ký tự bên ngoài bổ sung. 

DP khám phá mọi tập hợp khoảng thời gian xuất hiện không chồng chéo hợp lệ và ghi lại chi phí xóa bắt buộc nhỏ nhất cho mỗi số lần xuất hiện. Đó chính xác là số lượng cần thiết để rút ra tất cả các câu trả lời. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    s = input().strip()
    p = input().strip()

    n = len(s)
    m = len(p)

    # next occurrence table
    nxt = [[n] * 26 for _ in range(n + 2)]

    for c in range(26):
        nxt[n][c] = n

    for i in range(n - 1, -1, -1):
        nxt[i] = nxt[i + 1][:]
        nxt[i][ord(s[i]) - 97] = i

    end = [-1] * n
    cost = [0] * n

    for i in range(n):
        if s[i] != p[0]:
            continue

        pos = i
        ok = True

        for j in range(1, m):
            pos = nxt[pos + 1][ord(p[j]) - 97]
            if pos == n:
                ok = False
                break

        if ok:
            end[i] = pos
            cost[i] = (pos - i + 1) - m

    INF = 30000

    dp = [array('H', [INF]) * (n + 1) for _ in range(n + 1)]
    dp[0][0] = 0

    for i in range(n):
        cur = dp[i]
        nxt_row = dp[i + 1]

        for k in range(n + 1):
            val = cur[k]
            if val == INF:
                continue

            if val < nxt_row[k]:
                nxt_row[k] = val

            if end[i] != -1:
                r = end[i] + 1
                nv = val + cost[i]
                if nv < dp[r][k + 1]:
                    dp[r][k + 1] = nv

    min_del = dp[n]

    ans = [0] * (n + 1)

    for k in range(n + 1):
        d = min_del[k]
        if d == INF:
            continue

        max_del = n - k * m
        if max_del < d:
            continue

        for x in range(d, max_del + 1):
            if k > ans[x]:
                ans[x] = k

    print(*ans)

solve()
```Bảng lần xuất hiện tiếp theo cho phép chúng ta tìm vị trí kết thúc sớm nhất của một chuỗi trùng khớp trong`O(m)`thời gian cho mỗi vị trí xuất phát. Vì có nhiều nhất là 2000 lần bắt đầu và`m ≤ 500`, chi phí tiền xử lý này`O(nm)`. 

DP lưu trữ các thao tác xóa bắt buộc tối thiểu. Sự chuyển đổi nhảy từ`i`trực tiếp đến`end[i] + 1`là cơ chế ngăn chặn sự chồng chéo. Hai lần xuất hiện không bao giờ có thể chia sẻ các ký tự vì lần xuất hiện thứ hai buộc phải bắt đầu sau khi khoảng thời gian đầu tiên kết thúc. 

Bước chuyển đổi cuối cùng là lúc nhiều giải pháp sai không thành công. Biết số lần xóa tối thiểu cho`k`số lần xuất hiện là không đủ. Chúng ta cũng phải tôn trọng giới hạn trên`n - km`, bởi vì mỗi lần xuất hiện cần`m`những nhân vật sống sót. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
s = "aaaaa"
p = "aa"
```Khoảng thời gian xuất hiện là: 

| Bắt đầu | Kết thúc | Chi phí | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 2 | 0 | 
| 2 | 3 | 0 | 
| 3 | 4 | 0 | 

Các trạng thái DP chính: 

| Vị trí | Lần xuất hiện | Xóa tối thiểu bắt buộc | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 2 | 1 | 0 | 
| 4 | 2 | 0 | 
| 5 | 2 | 0 | 

DP tìm thấy: 

| k | minDel[k] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 0 | 
| 2 | 0 | 

Vì`k = 2`, phạm vi xóa có thể đạt được là:```
[0, 5 - 2*2] = [0, 1]
```Vì vậy việc xóa được tính`0`Và`1`vẫn có thể giữ hai lần xuất hiện. 

Đầu ra cuối cùng:```
2 2 1 1 0 0
```Ví dụ này cho thấy việc xóa thêm ngoài khoảng thời gian xuất hiện được cho phép. 

### Ví dụ 2 

đầu vào:```
s = "abca"
p = "aa"
```Khoảng thời gian xuất hiện: 

| Bắt đầu | Kết thúc | Chi phí | 
| --- | --- | --- | 
| 0 | 3 | 2 | 

Độ dài khoảng là 4, trong khi độ dài mẫu là 2, do đó bắt buộc phải xóa hai lần. 

Kết quả DP: 

| k | minDel[k] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 2 | 

Đối với một lần xuất hiện:```
deletion range = [2, 4 - 2] = [2, 2]
```Chỉ có đúng hai lần xóa mới có thể giữ lại một bản sao của`"aa"`. 

Câu trả lời trở thành:```
0 0 1 0 0
```Điều này chứng tỏ tại sao chỉ kiểm tra xem`minDel[k] ≤ x`là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n² + nm)`| DP là`O(n²)`, xây dựng khoảng là`O(nm)`| 
| Không gian |`O(n²)`| Bảng kích thước DP`(n+1) × (n+1)`| 

Với`n ≤ 2000`, DP thực hiện khoảng bốn triệu cập nhật trạng thái, vừa vặn trong giới hạn. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from array import array

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = input().strip()
    p = input().strip()

    n = len(s)
    m = len(p)

    nxt = [[n] * 26 for _ in range(n + 2)]
    for i in range(n - 1, -1, -1):
        nxt[i] = nxt[i + 1][:]
        nxt[i][ord(s[i]) - 97] = i

    end = [-1] * n
    cost = [0] * n

    for i in range(n):
        if s[i] != p[0]:
            continue

        pos = i
        ok = True

        for j in range(1, m):
            pos = nxt[pos + 1][ord(p[j]) - 97]
            if pos == n:
                ok = False
                break

        if ok:
            end[i] = pos
            cost[i] = (pos - i + 1) - m

    INF = 30000
    dp = [array('H', [INF]) * (n + 1) for _ in range(n + 1)]
    dp[0][0] = 0

    for i in range(n):
        for k in range(n + 1):
            v = dp[i][k]
            if v == INF:
                continue

            if v < dp[i + 1][k]:
                dp[i + 1][k] = v

            if end[i] != -1:
                r = end[i] + 1
                nv = v + cost[i]
                if nv < dp[r][k + 1]:
                    dp[r][k + 1] = nv

    ans = [0] * (n + 1)

    for k in range(n + 1):
        d = dp[n][k]
        if d == INF:
            continue

        mx = n - k * m
        if mx < d:
            continue

        for x in range(d, mx + 1):
            ans[x] = max(ans[x], k)

    return " ".join(map(str, ans))

# provided sample
assert run("aaaaa\naa\n") == "2 2 1 1 0 0"

# minimum size
assert run("a\na\n") == "1 0"

# pattern impossible
assert run("abc\nd\n") == "0 0 0 0"

# exact deletion bound
assert run("aa\naa\n") == "1 0 0"

# subsequence but not substring
assert run("abca\naa\n") == "0 0 1 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / a`|`1 0`| Đầu vào hợp lệ nhỏ nhất | 
|`abc / d`|`0 0 0 0`| Không thể hình thành mẫu | 
|`aa / aa`|`1 0 0`| Giới hạn xóa trên`n - km`| 
|`abca / aa`|`0 0 1 0 0`| Khớp trình tự với việc xóa bắt buộc | 

## Vỏ cạnh 

Hãy xem xét:```
aa
aa
```DP tìm thấy một lần xuất hiện với chi phí xóa bắt buộc`0`. Số lần xóa tối đa duy trì lần xuất hiện đó cũng là`0`, vì cả hai ký tự đều được yêu cầu. Phạm vi có thể đạt được là`[0, 0]`. Đối với số lần xóa`1`, câu trả lời đúng sẽ trở thành`0`. 

Bây giờ hãy xem xét:```
abca
aa
```Khoảng thời gian xuất hiện sớm nhất là`[0, 3]`. chiều dài của nó là`4`, trong khi chiều dài mẫu là`2`, vì vậy việc xóa hai lần là không thể tránh khỏi. Bản ghi DP`minDel[1] = 2`. Từ`n - km = 2`, số lần xóa duy nhất có thể đạt được cho một lần xuất hiện chính xác là`2`. 

Cuối cùng:```
aaaaa
aa
```Có nhiều cách chồng chéo để chọn lần xuất hiện. DP không bao giờ cho phép chồng chéo vì việc chọn một khoảng sẽ nhảy trực tiếp đến vị trí sau khi kết thúc. Hai lần xuất hiện được chọn luôn tách rời nhau, phù hợp với yêu cầu của bài toán về các bản sao không chồng chéo trong chuỗi cuối cùng.
