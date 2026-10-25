---
title: "CF 106078G - Sao Thổ"
description: "Chúng ta có chuỗi mục tiêu t và danh sách các chuỗi dữ liệu đã ghi. Chúng ta có thể chọn bất kỳ tập hợp con nào của chuỗi đã ghi nhưng thứ tự ban đầu của chúng không thể thay đổi. Các chuỗi đã chọn được nối với nhau và mục tiêu là làm cho chuỗi đích xuất hiện dưới dạng chuỗi con nhiều lần nhất có thể."
date: "2026-06-25T12:09:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106078
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 9-17-25 Div. 1 (Advanced)"
rating: 0
weight: 106078
solve_time_s: 42
verified: true
draft: false
---

[CF 106078G - Sao Thổ](https://codeforces.com/problemset/problem/106078/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi mục tiêu`t`và danh sách các chuỗi dữ liệu đã ghi. Chúng ta có thể chọn bất kỳ tập hợp con nào của chuỗi đã ghi nhưng thứ tự ban đầu của chúng không thể thay đổi. Các chuỗi đã chọn được nối với nhau và mục tiêu là làm cho chuỗi đích xuất hiện dưới dạng chuỗi con nhiều lần nhất có thể. Số lần xuất hiện chồng chéo được tính, do đó, một trận đấu kết thúc ở một vị trí có thể chia sẻ các ký tự với một trận đấu khác. 

Đầu vào bao gồm một mẫu mục tiêu ngắn và lên tới 1000 điểm dữ liệu. Mỗi điểm dữ liệu là một chuỗi có độ dài tối đa là 100. Đầu ra là số lần mục tiêu tối đa có thể xảy ra sau khi chọn tập hợp con tốt nhất. 

Hạn chế chính định hình giải pháp là độ dài mục tiêu nhỏ. Mục tiêu có độ dài tối đa là 20, trong khi tổng lượng dữ liệu có thể đạt khoảng 100000 ký tự. Một giải pháp thử mọi tập hợp con của chuỗi là không thể vì có rất nhiều sự lựa chọn theo cấp số nhân. Ngay cả việc kiểm tra mọi ranh giới nối có thể cũng sẽ quá tốn kém. Chúng ta cần nén thông tin mang theo giữa các chuỗi đã chọn. 

Phần duy nhất của phép nối đã được xây dựng có ý nghĩa quan trọng đối với các kết quả khớp trong tương lai là hậu tố có thể trở thành khởi đầu cho sự xuất hiện trong tương lai của`t`. Ví dụ, nếu mục tiêu là`abab`, sau khi đọc một số văn bản, chúng ta chỉ cần biết hậu tố hiện tại có phải là`a`,`ab`, hoặc`aba`. Toàn bộ sự nối trước đó là không liên quan. 

Việc triển khai bất cẩn có thể thất bại khi có các kết quả trùng lặp. Ví dụ, nếu mục tiêu là`aaa`và văn bản được chọn là`aaaa`, đầu ra đúng là`2`, vì các lần xuất hiện ở vị trí từ 1 đến 3 và 2 đến 4. Giải pháp nhảy qua các ký tự trùng khớp sau khi tìm thấy lần xuất hiện sẽ chỉ được tính`1`. 

Một trường hợp phức tạp khác là khi một sự xuất hiện vượt qua ranh giới giữa hai chuỗi được chọn. Xem xét mục tiêu`abc`và chuỗi dữ liệu`ab`Và`c`. Chọn cả hai chuỗi sẽ nối`abc`, vậy câu trả lời là`1`. Một phương pháp đếm số lần xuất hiện bên trong mỗi chuỗi riêng biệt sẽ cho ra kết quả không chính xác.`0`. 

Trường hợp cạnh cuối cùng là không chọn chuỗi. Nếu mọi chuỗi dữ liệu đều vô dụng thì câu trả lời phải được giữ nguyên`0`. Ví dụ, với mục tiêu`xyz`và một chuỗi dữ liệu`aaaa`, đầu ra đúng là`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi tập hợp con có thể có của các điểm dữ liệu. Đối với mỗi tập hợp con, chúng ta nối các chuỗi đã chọn và đếm số lần xuất hiện của`t`. Điều này đúng vì mọi tài liệu cuối cùng hợp lệ đều tương ứng với chính xác một tập hợp con. Vấn đề là số lượng tập hợp con, có thể`2^1000`, vì vậy cách tiếp cận này vượt xa những gì chúng tôi có thể xử lý. 

Quan sát quan trọng là tương lai không phụ thuộc vào sự nối kết đầy đủ. Khi xử lý chuỗi từ trái sang phải, chúng ta chỉ cần trạng thái automaton mô tả hậu tố dài nhất của văn bản hiện tại cũng là tiền tố của`t`. Đây là thông tin trạng thái tương tự được sử dụng bởi kết hợp chuỗi dựa trên hàm tiền tố. 

Vì độ dài mục tiêu nhiều nhất là 20 nên chỉ có một số ít trạng thái có thể xảy ra. Đối với mỗi chuỗi dữ liệu, chúng ta có thể tính toán trước điều gì sẽ xảy ra nếu chúng ta nối thêm chuỗi đó trong khi bắt đầu từ mỗi trạng thái có thể. Quá trình chuyển đổi cho chúng ta biết trạng thái mới và số lần xuất hiện mới của`t`được tạo ra. 

Vấn đề sau đó trở thành một quá trình lập trình động đơn giản. Trong khi quét các điểm dữ liệu,`dp[state]`lưu trữ số lượng kết quả khớp tối đa có thể đạt được sau khi xem xét các chuỗi được xử lý và kết thúc ở trạng thái tự động hóa đó. Đối với mỗi chuỗi mới, chúng tôi bỏ qua hoặc lấy chuỗi đó và áp dụng quá trình chuyển đổi được tính toán trước của chuỗi đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * tổng chiều dài) | O(tổng chiều dài) | Quá chậm | 
| Tối ưu | O(n * | t | * ( | 

## Hướng dẫn thuật toán 

1. Xây dựng một máy tự động dựa trên chức năng tiền tố cho chuỗi mục tiêu. Đối với mọi trạng thái có thể, hãy lưu trữ trạng thái tiếp theo sau khi đọc một ký tự và liệu ký tự đó có hoàn thành một lần xuất hiện hay không. Trạng thái biểu thị số lượng ký tự của mục tiêu hiện khớp với hậu tố của những gì chúng ta đã đọc. 
2. Đối với mỗi chuỗi dữ liệu, hãy mô phỏng việc thêm nó vào văn bản hiện tại bắt đầu từ mọi trạng thái có thể. Lưu trữ trạng thái kết quả và số lần xuất hiện được tạo. Điều này hiệu quả vì mục tiêu ngắn nên việc tính toán trước tất cả các chuyển đổi sẽ rẻ. 
3. Khởi tạo mảng lập trình động với trạng thái`0`có giá trị`0`. Điều này thể hiện một phép nối được chọn trống. 
4. Xử lý chuỗi dữ liệu theo thứ tự. Đối với mỗi chuỗi, trước tiên hãy sao chép các giá trị DP hiện tại làm tùy chọn mà chúng tôi bỏ qua chuỗi này. 
5. Đối với mọi trạng thái tự động hiện tại, hãy thử lấy chuỗi hiện tại. Chuyển sang trạng thái tiếp theo được tính toán trước và cộng số lượng kết quả khớp được tạo ra bởi chuỗi này. Giữ giá trị tốt nhất cho trạng thái đó. 
6. Sau khi tất cả các chuỗi được xử lý, câu trả lời là giá trị tối đa trong số tất cả các trạng thái tự động vì trạng thái hậu tố cuối cùng không quan trọng. 

Tại sao nó hoạt động: bất biến DP là sau khi xử lý lần đầu tiên`i`chuỗi dữ liệu,`dp[s]`là số lần xuất hiện tốt nhất có thể có trong số tất cả các lựa chọn của các chuỗi khiến trình so khớp ở trạng thái`s`. Việc bỏ qua một chuỗi sẽ giữ nguyên trạng thái và điểm số, trong khi việc lấy một chuỗi sẽ có tác dụng chính xác như việc nối thêm chuỗi đó. Vì mọi lựa chọn có thể là lấy hoặc bỏ qua chuỗi tiếp theo nên tất cả các phép nối hợp lệ đều được xem xét. Trạng thái tự động chứa chính xác thông tin cần thiết cho các tiện ích mở rộng trong tương lai, do đó không có thông tin cần thiết nào bị mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    target = input().strip()
    n = int(input())
    strings = [input().strip() for _ in range(n)]

    m = len(target)

    pi = [0] * m
    for i in range(1, m):
        j = pi[i - 1]
        while j > 0 and target[i] != target[j]:
            j = pi[j - 1]
        if target[i] == target[j]:
            j += 1
        pi[i] = j

    nxt = [[0] * 26 for _ in range(m)]
    add = [[0] * 26 for _ in range(m)]

    for state in range(m):
        for c in range(26):
            ch = chr(ord('a') + c)
            j = state
            while j > 0 and target[j] != ch:
                j = pi[j - 1]
            if target[j] == ch:
                j += 1
            if j == m:
                add[state][c] = 1
                j = pi[m - 1]
            nxt[state][c] = j

    transitions = []
    for s in strings:
        cur = []
        for start in range(m):
            state = start
            score = 0
            for ch in s:
                idx = ord(ch) - ord('a')
                score += add[state][idx]
                state = nxt[state][idx]
            cur.append((state, score))
        transitions.append(cur)

    dp = [-10**9] * m
    dp[0] = 0

    for trans in transitions:
        ndp = dp[:]
        for state in range(m):
            if dp[state] < 0:
                continue
            to, gain = trans[state]
            if dp[state] + gain > ndp[to]:
                ndp[to] = dp[state] + gain
        dp = ndp

    print(max(dp))

if __name__ == "__main__":
    solve()
```Hàm tiền tố xây dựng các liên kết lỗi cần thiết để tiếp tục khớp sau khi không khớp. Điều này tránh việc quét lại toàn bộ mục tiêu bất cứ khi nào một ký tự không phù hợp. 

các`nxt`Và`add`các bảng mô tả máy tự động.`nxt[state][character]`là trạng thái hậu tố sau khi đọc ký tự, trong khi`add[state][character]`cho biết liệu nhân vật đó có hoàn thành một lần xuất hiện hay không. Khi một sự kiện được tìm thấy, trạng thái sẽ quay trở lại hậu tố thích hợp dài nhất cũng là tiền tố, điều này cho phép trùng lặp. 

Bảng chuyển đổi cho mỗi chuỗi dữ liệu được tính toán một lần. Quá trình mô phỏng sử dụng máy tự động thay vì liên tục tìm kiếm mục tiêu nên ngay cả những chuỗi dữ liệu dài cũng được xử lý nhanh chóng. 

Bản cập nhật DP sao chép mảng hiện tại trước tiên vì vẫn có thể bỏ qua chuỗi. Sau đó, mọi trạng thái đều thử lấy chuỗi và cập nhật trạng thái kết quả. Không có vấn đề gì với việc tràn số nguyên trong Python, nhưng giá trị ban đầu âm thể hiện các trạng thái không thể. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
target = lol
strings:
olo
lol
olo
```Sự phát triển DP là: 

| Dữ liệu được xử lý | Trạng thái tốt nhất sau khi xử lý | Điểm tối đa | 
| --- | --- | --- | 
| không | trạng thái 0: 0 | 0 | 
| ôi | chưa có trận đấu đầy đủ | 0 | 
| cười lớn | lấy nó tạo ra một trận đấu | 1 | 
| ôi | vượt qua ranh giới trước đó tạo ra nhiều trận đấu hơn | 3 | 

Dấu vết cho thấy tại sao việc giữ trạng thái hậu tố lại quan trọng. Chuỗi cuối cùng không chứa`lol`một mình, nhưng nó hoàn thành các kết quả khớp vì văn bản đã chọn trước đó. 

Đối với mẫu thứ hai:```
target = ababac
strings:
abab
aba
abac
```| Dữ liệu được xử lý | Lựa chọn tốt nhất | Điểm | 
| --- | --- | --- | 
| không | trống | 0 | 
| abab | lấy chuỗi đầu tiên | 0 | 
| aba | bỏ qua hoặc thực hiện, không hoàn thành mục tiêu | 0 | 
| bàn tính | kết hợp đầu tiên và cuối cùng | 1 | 

Điều này cho thấy DP xử lý các lựa chọn giữa việc lấy và bỏ qua chuỗi trong khi vẫn giữ nguyên thứ tự ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m * (m + L)) | Mỗi trong số`n`chuỗi được mô phỏng từ mọi trạng thái mục tiêu, trong đó`m`là độ dài mục tiêu và`L`là độ dài chuỗi. | 
| Không gian | O(m) | DP chỉ lưu trữ một giá trị cho mỗi trạng thái tự động. | 

Đây`m`nhiều nhất là 20, nên không gian trạng thái rất nhỏ. Tổng độ dài văn bản được xử lý tối đa là khoảng 100000 ký tự, giúp phương pháp này dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    target = sys.stdin.readline().strip()
    n = int(sys.stdin.readline())
    strings = [sys.stdin.readline().strip() for _ in range(n)]

    m = len(target)

    pi = [0] * m
    for i in range(1, m):
        j = pi[i - 1]
        while j > 0 and target[i] != target[j]:
            j = pi[j - 1]
        if target[i] == target[j]:
            j += 1
        pi[i] = j

    nxt = [[0] * 26 for _ in range(m)]
    add = [[0] * 26 for _ in range(m)]

    for s in range(m):
        for c in range(26):
            ch = chr(97 + c)
            j = s
            while j > 0 and target[j] != ch:
                j = pi[j - 1]
            if target[j] == ch:
                j += 1
            if j == m:
                add[s][c] = 1
                j = pi[m - 1]
            nxt[s][c] = j

    dp = [0] + [-10**9] * (m - 1)

    for text in strings:
        trans = []
        for start in range(m):
            state = start
            score = 0
            for ch in text:
                x = ord(ch) - 97
                score += add[state][x]
                state = nxt[state][x]
            trans.append((state, score))

        ndp = dp[:]
        for i in range(m):
            if dp[i] >= 0:
                j, gain = trans[i]
                ndp[j] = max(ndp[j], dp[i] + gain)
        dp = ndp

    sys.stdin = old
    return str(max(dp)) + "\n"

assert solve("""lol
3
olo
lol
olo
""") == "3\n"

assert solve("""ababac
3
abab
aba
abac
""") == "1\n"

assert solve("""aaa
1
aaaa
""") == "2\n"

assert solve("""abc
2
ab
c
""") == "1\n"

assert solve("""xyz
1
aaaa
""") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`lol`với ba dây |`3`| Chồng chéo và trùng khớp ranh giới | 
|`ababac`mẫu |`1`| Chọn một tập hợp con theo thứ tự | 
|`aaa`Và`aaaa`|`2`| Sự xuất hiện chồng chéo | 
|`abc`với`ab`,`c`|`1`| Trận đấu trên ranh giới chuỗi | 
|`xyz`với`aaaa`|`0`| Không có điểm dữ liệu hữu ích | 

## Vỏ cạnh 

Đối với mục tiêu`aaa`và chuỗi`aaaa`, máy tự động bắt đầu ở trạng thái`0`. Sau khi đọc ba ký tự đầu tiên, nó tìm thấy một lần xuất hiện và quay lại trạng thái`2`, đại diện cho hậu tố`aa`. Đọc ký tự thứ tư sẽ tạo ra một sự kiện khác. Đầu ra của thuật toán`2`bởi vì nó không bao giờ loại bỏ hậu tố sau một trận đấu. 

Đối với mục tiêu`abc`và dây`ab`Và`c`, chuỗi được chọn đầu tiên sẽ để máy tự động ở trạng thái`2`, nghĩa là hậu tố hiện tại là`ab`. Khi chuỗi thứ hai bắt đầu bằng`c`, máy tự động ngay lập tức hoàn thành mẫu và thêm một điểm vào điểm. Thẻ điều khiển này khớp với nhiều chuỗi đã chọn. 

Đối với mục tiêu`xyz`và chuỗi`aaaa`, mọi chuyển đổi đều tránh xa một trận đấu đã hoàn thành. DP giữ điểm số cao nhất có thể tiếp cận được vì`0`, đại diện cho việc chọn không có gì hữu ích. Thuật toán không bao giờ buộc phải chọn một chuỗi, vì vậy những đóng góp không thể thực hiện được sẽ không ảnh hưởng đến kết quả.
