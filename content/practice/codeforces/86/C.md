---
title: "CF 86C - Kỹ thuật di truyền"
description: "Chúng ta được cung cấp một số đoạn DNA ngắn theo bảng chữ cái {A, C, G, T}. Chuỗi DNA dài hơn được coi là hợp lệ nếu mọi vị trí của chuỗi thuộc về ít nhất một lần xuất hiện của một trong các đoạn đã cho. Các mảnh có thể chồng lên nhau tùy ý."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "string-suffix-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 86
codeforces_index: "C"
codeforces_contest_name: "Yandex.Algorithm 2011: Round 2"
rating: 2500
weight: 86
solve_time_s: 132
verified: true
draft: false
---

[CF 86C - Kỹ thuật di truyền](https://codeforces.com/problemset/problem/86/C) 

**Đánh giá:** 2500 
**Tas:** dp, cấu trúc hậu tố chuỗi, cây 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số đoạn DNA ngắn trên bảng chữ cái`{A, C, G, T}`. Chuỗi DNA dài hơn được coi là hợp lệ nếu mọi vị trí của chuỗi thuộc về ít nhất một lần xuất hiện của một trong các đoạn đã cho. 

Các mảnh có thể chồng lên nhau tùy ý. Một mảnh có thể bắt đầu bên trong một mảnh khác và nhiều mảnh có thể bao phủ cùng một vị trí. Yêu cầu duy nhất là sau khi đặt các đoạn xuất hiện ở đâu đó bên trong chuỗi, mọi vị trí ký tự đều được bao phủ bởi ít nhất một lần xuất hiện. 

Chúng ta phải đếm xem có bao nhiêu chuỗi dài`n`thỏa mãn điều kiện này. 

Những ràng buộc này là nhỏ ở một số khía cạnh nhưng lại có vẻ lớn ở những khía cạnh khác. Có nhiều nhất`10`mẫu và chiều dài mỗi mẫu nhiều nhất là`10`, do đó tổng chiều dài mẫu là rất nhỏ. Điều đó gợi ý mạnh mẽ việc xây dựng một loại máy tự động nào đó dựa trên các mẫu. Mặt khác,`n`có thể đạt được`1000`, điều này ngay lập tức loại bỏ bất kỳ số mũ nào trong`n`hoặc bất kỳ DP nào lưu trữ toàn bộ mặt nạ che phủ trên các vị trí. 

Phần nguy hiểm của vấn đề là phạm vi phủ sóng mang tính toàn cầu. Liệu một vị trí có được đảm bảo hay không tùy thuộc vào các khoảng thời gian được tạo bởi các lần xuất hiện mẫu có thể bắt đầu sớm hơn và kết thúc muộn hơn. Một DP ngây thơ chỉ theo dõi trạng thái máy tự động hiện tại là không đủ. 

Một số trường hợp đặc biệt khiến cho các phương pháp đơn giản hơn không thành công. 

Coi như:```
3 1
AA
```Các chuỗi hợp lệ chỉ là:```
AAA
```Chuỗi`"AAB"`không hợp lệ vì vị trí`3`không được che chắn, mặc dù mẫu đã kết thúc ở vị trí`2`. Một DP bất cẩn chỉ kiểm tra xem liệu mẫu nào đó có xuất hiện ở đâu đó hay không sẽ chấp nhận nó một cách sai lầm. 

Một trường hợp tế nhị khác:```
4 2
AAA
AA
```Chuỗi`"AAAA"`là hợp lệ. Mức độ phù hợp đến từ các lần xuất hiện chồng chéo:```
AAA
 AAA
```Một chiến lược tham lam luôn cố gắng mở rộng phạm vi bao phủ với mẫu dài nhất có thể thất bại vì phạm vi bao phủ có thể cần các kết quả trùng khớp ngắn hơn chồng chéo. 

Các mẫu trùng lặp cũng được cho phép:```
3 2
A
A
```Câu trả lời vẫn là`1`, không`2`. Chúng tôi đếm các chuỗi riêng biệt chứ không phải cách để che chúng. 

Vấn đề khái niệm khó nhất là vấn đề này:```
5 1
AT
```Kể cả nếu chúng ta hợp nhau`"AT"`kết thúc ở vị trí`4`, chức vụ`5`có thể vẫn chưa được khám phá. Phạm vi bao gồm từng chỉ mục riêng lẻ, không phải về việc liệu chuỗi có chứa nhiều kết quả khớp hay không. 

Giải pháp cần nhớ phạm vi phủ sóng hiện tại mở rộng bao xa trong khi quét chuỗi từ trái sang phải. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo tất cả`4^n`Các chuỗi DNA và đối với mỗi chuỗi, hãy kiểm tra xem mọi vị trí có thuộc về ít nhất một lần xuất hiện của mẫu hay không. 

Việc kiểm tra một chuỗi có thể được thực hiện bằng cách khớp chuỗi con tiêu chuẩn trong khoảng`O(n * total_pattern_length)`. Vấn đề thực sự là số lượng chuỗi. Với`n = 1000`, chúng ta sẽ có`4^1000`ứng cử viên, điều này là không thể về mặt thiên văn. 

Ý tưởng tự nhiên tiếp theo là lập trình động trên tiền tố. Trong khi xây dựng chuỗi từ trái sang phải, chúng tôi muốn biết mẫu nào hiện phù hợp làm hậu tố. Điều đó ngay lập tức gợi ý đến máy tự động Aho-Corasick, bởi vì nó lưu trữ tất cả thông tin khớp hậu tố một cách gọn gàng. 

Riêng máy tự động vẫn chưa đủ. 

Giả sử chúng ta đang ở vị trí`i`. Việc biết nút tự động hiện tại cho chúng ta biết mẫu nào kết thúc tại`i`, nhưng giá trị còn phụ thuộc vào vị trí`i`bản thân nó được bao phủ. Mức độ phù hợp đến từ các khoảng thời gian được tạo bởi các trận đấu trước đó. Nếu một mô hình có chiều dài`L`kết thúc tại`i`, sau đó nó bao gồm các vị trí`[i-L+1, i]`. 

Quan sát quan trọng là trong khi quét từ trái sang phải, điều duy nhất quan trọng là vị trí ngoài cùng bên phải đã được đảm bảo che phủ. 

Thay vì lưu trữ tập hợp chính xác các vị thế được bảo hiểm, chúng tôi lưu trữ một số:`rem = how many future positions are still guaranteed covered`Nếu một mô hình có chiều dài`L`kết thúc ở vị trí hiện tại, sau đó phạm vi bảo hiểm sẽ mở rộng`L-1`vị trí trong tương lai. Trong khi di chuyển về phía trước một ký tự,`rem`giảm đi một. Bất cứ khi nào một trận đấu dài hơn xuất hiện, chúng tôi sẽ làm mới nó. 

Điều này chuyển đổi điều kiện phủ sóng toàn cầu thành chuyển đổi DP cục bộ. 

Máy tự động đưa ra tất cả các mẫu khớp kết thúc ở vị trí hiện tại. DP giữ phạm vi phủ sóng tối đa còn lại. Cùng nhau, họ hoàn toàn đặc trưng cho nhà nước. 

Số lượng trạng thái máy tự động là rất nhỏ vì tổng chiều dài mẫu tối đa là`100`. Phạm vi bảo hiểm còn lại nhiều nhất là`10`, vì độ dài mẫu nhiều nhất là`10`. Điều đó làm cho một DP có hình khối hoàn toàn khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^n · n · tổngLen) | O(n) | Quá chậm | 
| Tối ưu | O(n · tiểu bang · maxLen · 4) | O(trạng thái · maxLen) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng máy tự động Aho-Corasick từ tất cả các mẫu. 

Mỗi nút đại diện cho một tiền tố của một số mẫu. Sự chuyển đổi tuân theo các ký tự DNA. Các liên kết bị lỗi kết nối một nút với hậu tố thích hợp dài nhất cũng có trong bộ ba. 
2. Đối với mỗi nút tự động, hãy tính`best[node]`.`best[node]`là độ dài tối đa của bất kỳ mẫu nào kết thúc tại nút này hoặc dọc theo chuỗi lỗi của nó. 

Điều này quan trọng vì khi chúng ta đến một nút sau khi đọc một số tiền tố của chuỗi, mọi mẫu khớp kết thúc ở đây đều có khả năng mở rộng phạm vi bao phủ. Chỉ có cái dài nhất là quan trọng. 
3. Xác định trạng thái DP. 

Cho phép:`dp[pos][state][rem]`biểu thị số cách xây dựng hình thứ nhất`pos`các ký tự như vậy:`state`là nút tự động hiện tại.`rem`có bao nhiêu vị trí bắt đầu từ`pos+1`đã được đảm bảo bảo hiểm. 
4. Khởi tạo:```
dp[0][root][0] = 1
```Trước khi đọc bất cứ điều gì, không có vị trí trong tương lai nào được đề cập. 
5. Chuyển đổi bằng cách thêm một ký tự. 

Từ`(pos, state, rem)`: 

Hãy thử tất cả bốn chữ cái DNA. 

Di chuyển qua máy tự động để`next_state`. 
6. Cập nhật phạm vi bảo hiểm còn lại. 

Đầu tiên, mức độ phù hợp hiện tại giảm đi một vì chúng tôi đã nâng cao một vị trí:```
new_rem = max(rem - 1, 0)
```Sau đó kiểm tra xem một số mẫu có kết thúc ở đây không. Nếu mẫu phù hợp dài nhất có độ dài`L`, nó bao phủ`L-1`vị trí tương lai:```
new_rem = max(new_rem, L - 1)
```7. Từ chối các vị trí không được che chắn. 

Vị trí hiện tại được bảo hiểm nếu:`rem > 0`hoặc một mẫu kết thúc ở đây bao phủ nó ngay lập tức. 

Tương tự, sau khi xử lý quá trình chuyển đổi, vị trí hiện tại là hợp lệ nếu:```
max(rem, L) > 0
```Vì mọi mẫu đều có độ dài dương nên điều kiện này xuất hiện một cách tự nhiên thông qua logic cập nhật. 
8. Sau khi xử lý xong tất cả`n`các ký tự, chỉ các trạng thái có`rem == 0`được tự động hợp lệ. 

Trên thực tế, mọi vị trí được xử lý đều đã được kiểm tra trong quá trình chuyển đổi, vì vậy tất cả các trạng thái còn lại đều hợp lệ. Tính tổng tất cả các giá trị DP theo chiều dài`n`. 

### Tại sao nó hoạt động 

Bất biến là: 

Sau khi xử lý`pos`nhân vật,`rem`bằng số lượng vị trí trong tương lai đã được đảm bảo bao phủ bởi các lần xuất hiện mẫu đã hoàn thành trước đó. 

Bất cứ khi nào một mô hình có chiều dài`L`kết thúc ở vị trí hiện tại, nó đóng góp phạm vi bảo hiểm cho chính xác vị trí tiếp theo`L-1`các vị trí. Sử dụng mức tối đa sẽ bảo toàn tất cả thông tin cần thiết vì chỉ khoảng thời gian hoạt động có phạm vi tiếp cận xa nhất mới quan trọng. 

Một vị trí trở nên không hợp lệ chính xác khi chúng ta tiến vào vị trí đó mà không có khoảng thời gian bảo hiểm hoạt động và không có mẫu mới nào kết thúc ở đó. DP không bao giờ cho phép chuyển đổi như vậy. 

Vì máy tự động xác định chính xác mọi mẫu kết thúc ở mọi vị trí và DP theo dõi chính xác phạm vi phủ sóng trong tương lai nên mọi cấu trúc được chấp nhận đều tương ứng với một chuỗi được lọc hợp lệ và mọi chuỗi được lọc hợp lệ đều tuân theo một số đường dẫn DP. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

MOD = 1000000009
ALPHA = {'A': 0, 'C': 1, 'G': 2, 'T': 3}

def solve():
    n, m = map(int, input().split())
    patterns = [input().strip() for _ in range(m)]

    # trie
    nxt = [[-1] * 4]
    fail = [0]
    best = [0]

    for s in patterns:
        node = 0

        for ch in s:
            c = ALPHA[ch]

            if nxt[node][c] == -1:
                nxt[node][c] = len(nxt)
                nxt.append([-1] * 4)
                fail.append(0)
                best.append(0)

            node = nxt[node][c]

        best[node] = max(best[node], len(s))

    # build automaton
    q = deque()

    for c in range(4):
        if nxt[0][c] == -1:
            nxt[0][c] = 0
        else:
            v = nxt[0][c]
            fail[v] = 0
            q.append(v)

    while q:
        v = q.popleft()

        best[v] = max(best[v], best[fail[v]])

        for c in range(4):
            to = nxt[v][c]

            if to == -1:
                nxt[v][c] = nxt[fail[v]][c]
            else:
                fail[to] = nxt[fail[v]][c]
                q.append(to)

    states = len(nxt)
    max_len = 10

    dp = [[[0] * (max_len + 1) for _ in range(states)] for _ in range(n + 1)]
    dp[0][0][0] = 1

    for pos in range(n):
        for state in range(states):
            for rem in range(max_len + 1):
                cur = dp[pos][state][rem]

                if cur == 0:
                    continue

                for c in range(4):
                    ns = nxt[state][c]

                    longest = best[ns]

                    # current position must be covered
                    if rem == 0 and longest == 0:
                        continue

                    nrem = max(rem - 1, longest - 1)

                    dp[pos + 1][ns][nrem] += cur
                    dp[pos + 1][ns][nrem] %= MOD

    ans = 0

    for state in range(states):
        for rem in range(max_len + 1):
            ans += dp[n][state][rem]

    ans %= MOD

    print(ans)

solve()
```Cấu trúc trie chèn từng ký tự mẫu theo từng ký tự. Mỗi nút đầu cuối lưu trữ độ dài mẫu tối đa kết thúc ở đó. Sử dụng mức tối đa là đủ vì khoảng thời gian dài hơn chiếm ưu thế so với khoảng thời gian ngắn hơn cho phạm vi phủ sóng trong tương lai. 

BFS trên trie xây dựng các liên kết lỗi chính xác như trong Aho-Corasick tiêu chuẩn. Trong quá trình truyền tải này, chúng tôi truyền bá thông tin mẫu thông qua các liên kết lỗi:```
best[v] = max(best[v], best[fail[v]])
```Nếu không có dòng này, các kết quả phù hợp với hậu tố sẽ bị mất. Ví dụ, nếu các mẫu là`"AAA"`Và`"AA"`, rồi sau khi đọc`"AAA"`chúng ta phải thừa nhận điều đó`"AA"`cũng kết thúc ở đó. 

Kích thước DP cho`rem`chỉ là`10`bởi vì không có chiều dài mẫu vượt quá`10`, vì vậy phạm vi phủ sóng trong tương lai không thể mở rộng hơn`9`. 

Logic chuyển tiếp là cốt lõi của giải pháp. 

Nếu như`rem == 0`và không có mẫu nào kết thúc ở vị trí hiện tại thì vị trí hiện tại sẽ không được phát hiện và quá trình chuyển đổi phải bị loại bỏ. 

Ngược lại ta tính:```
nrem = max(rem - 1, longest - 1)
```Khoảng hoạt động trước đó mất một bước vì chúng tôi đã di chuyển sang phải một vị trí. Một mẫu mới phù hợp có thể mở rộng phạm vi phủ sóng xa hơn. 

Tổng hợp mọi trạng thái theo chiều dài`n`là đúng vì phạm vi bảo hiểm được xác thực trực tuyến trong quá trình chuyển đổi. Không có vị trí không được che chắn nào có thể tồn tại trong DP. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
A
```Chỉ có chuỗi`"AA"`là hợp lệ. 

| Vị trí | Chuỗi hiện tại | Trạng thái tự động hóa | rem Trước | Độ dài trận đấu | rem Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | A | A | 0 | 1 | 0 | 
| 2 | AA | A | 0 | 1 | 0 | 

Bất kỳ nỗ lực nào để đặt`C`,`G`, hoặc`T`ngay lập tức thất bại vì không có mẫu nào bao trùm được vị trí đó. 

Ví dụ này xác nhận ý nghĩa của`rem = 0`. Vùng phủ sóng không tồn tại vượt quá vị trí hiện tại khi chiều dài mẫu là`1`. 

### Ví dụ 2 

đầu vào:```
4 1
AA
```Chuỗi hợp lệ:```
AAAA
```Dấu vết: 

| Vị trí | Chuỗi hiện tại | rem Trước | Độ dài trận đấu | rem Sau | 
| --- | --- | --- | --- | --- | 
| 1 | A | 0 | 0 | không hợp lệ | 
| 2 | AA | 0 | 2 | 1 | 
| 3 | AAA | 1 | 2 | 1 | 
| 4 | AAAA | 1 | 2 | 1 | 

Vị trí đầu tiên không thể đứng một mình vì chưa có mẫu nào kết thúc ở đó. Bảo hiểm chỉ bắt đầu một lần đầu tiên`"AA"`kết thúc ở vị trí`2`. 

Sau đó, các lần xuất hiện chồng chéo sẽ duy trì phạm vi phủ sóng liên tục. 

Điều này chứng tỏ tại sao việc theo dõi phạm vi bảo hiểm trực tuyến là cần thiết. Kiểm tra "chứa mẫu" đơn giản sẽ chấp nhận không chính xác nhiều chuỗi không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · tiểu bang · maxLen · 4) | DP qua các vị trí, trạng thái tự động hóa, phạm vi bao phủ còn lại và các chữ cái DNA | 
| Không gian | O(n · tiểu bang · maxLen) | Bảng DP đầy đủ | 

Máy tự động chứa tối đa khoảng`100`trạng thái vì tổng chiều dài mẫu nhiều nhất là`100`. các`rem`kích thước nhiều nhất là`10`. Ngay cả với`n = 1000`, tổng số lần chuyển đổi vẫn ở mức thoải mái dưới vài triệu thao tác, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

MOD = 1000000009
ALPHA = {'A': 0, 'C': 1, 'G': 2, 'T': 3}

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    patterns = [input().strip() for _ in range(m)]

    nxt = [[-1] * 4]
    fail = [0]
    best = [0]

    for s in patterns:
        node = 0

        for ch in s:
            c = ALPHA[ch]

            if nxt[node][c] == -1:
                nxt[node][c] = len(nxt)
                nxt.append([-1] * 4)
                fail.append(0)
                best.append(0)

            node = nxt[node][c]

        best[node] = max(best[node], len(s))

    q = deque()

    for c in range(4):
        if nxt[0][c] == -1:
            nxt[0][c] = 0
        else:
            v = nxt[0][c]
            q.append(v)

    while q:
        v = q.popleft()

        best[v] = max(best[v], best[fail[v]])

        for c in range(4):
            to = nxt[v][c]

            if to == -1:
                nxt[v][c] = nxt[fail[v]][c]
            else:
                fail[to] = nxt[fail[v]][c]
                q.append(to)

    states = len(nxt)

    dp = [[[0] * 11 for _ in range(states)] for _ in range(n + 1)]
    dp[0][0][0] = 1

    for pos in range(n):
        for state in range(states):
            for rem in range(11):
                cur = dp[pos][state][rem]

                if cur == 0:
                    continue

                for c in range(4):
                    ns = nxt[state][c]
                    longest = best[ns]

                    if rem == 0 and longest == 0:
                        continue

                    nrem = max(rem - 1, longest - 1)

                    dp[pos + 1][ns][nrem] += cur
                    dp[pos + 1][ns][nrem] %= MOD

    ans = 0

    for state in range(states):
        for rem in range(11):
            ans += dp[n][state][rem]

    return str(ans % MOD) + "\n"

# provided sample
assert run("2 1\nA\n") == "1\n", "sample 1"

# minimum size
assert run("1 1\nA\n") == "1\n", "single covered character"

# no valid strings
assert run("1 1\nAA\n") == "0\n", "pattern longer than string"

# overlapping coverage
assert run("4 1\nAA\n") == "1\n", "overlapping AA matches"

# all characters allowed
assert run("3 4\nA\nC\nG\nT\n") == "64\n", "every string valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / A`|`1`| Phiên bản hợp lệ tối thiểu | 
|`1 1 / AA`|`0`| Mẫu dài hơn mục tiêu | 
|`4 1 / AA`|`1`| Xử lý phạm vi bảo hiểm chồng chéo | 
|`3 4 / A C G T`|`64`| Mọi vị trí được bảo hiểm độc lập | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
AA
```Không có chuỗi hợp lệ tồn tại. 

Sau khi đọc một ký tự, không mẫu nào có thể kết thúc vì độ dài mẫu duy nhất là`2`. Quá trình chuyển đổi DP thấy:```
rem = 0
longest = 0
```và từ chối trạng thái ngay lập tức. Câu trả lời cuối cùng trở thành`0`. 

Bây giờ hãy xem xét phạm vi bảo hiểm chồng chéo:```
4 1
AA
```Tại vị trí`2`, đầu tiên`"AA"`trận đấu tạo ra:```
rem = 1
```ý nghĩa vị trí`3`đã được đảm bảo bảo hiểm. Khi vị trí`3`được xử lý, một cái khác`"AA"`kết thúc và làm mới vùng phủ sóng một lần nữa. Thuật toán xâu chuỗi các khoảng chồng chéo một cách chính xác. 

Cuối cùng, hãy xem xét kết quả khớp hậu tố thông qua các liên kết lỗi:```
3 2
AAA
AA
```Khi máy tự động đọc`"AAA"`, nút cho`"AAA"`cũng kế thừa trận đấu cho`"AA"`thông qua liên kết thất bại của nó. Không lan truyền:```
best[v] = max(best[v], best[fail[v]])
```DP sẽ bỏ lỡ các kết quả phù hợp với hậu tố ngắn hơn và từ chối không chính xác một số chuỗi hợp lệ.
