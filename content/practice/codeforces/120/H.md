---
title: "CF 120H - Ngắn gọn là linh hồn của trí tuệ"
description: "Chúng tôi được cung cấp tới 200 từ viết thường riêng biệt. Đối với mỗi từ, chúng ta phải chọn một từ viết tắt ngắn có độ dài từ 1 đến 4 ký tự và các ký tự xuất hiện trong từ gốc theo cùng một thứ tự."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graph-matchings"]
categories: ["algorithms"]
codeforces_contest: 120
codeforces_index: "H"
codeforces_contest_name: "School Regional Team Contest, Saratov, 2011"
rating: 1800
weight: 120
solve_time_s: 153
verified: false
draft: false
---

[CF 120H - Ngắn gọn là linh hồn của trí tuệ](https://codeforces.com/problemset/problem/120/H) 

**Đánh giá:** 1800 
**Thẻ:** so khớp biểu đồ 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tới 200 từ viết thường riêng biệt. Đối với mỗi từ, chúng ta phải chọn một từ viết tắt ngắn có độ dài từ 1 đến 4 ký tự và các ký tự xuất hiện trong từ gốc theo cùng một thứ tự. Các ký tự không cần phải liền kề nhau nên mỗi chữ viết tắt chỉ đơn giản là một dãy con của từ. 

Hạn chế chính là tính duy nhất. Hai từ khác nhau không thể nhận được cùng một từ viết tắt. Nếu có sự phân công như vậy thì chúng ta phải in một chữ viết tắt hợp lệ cho mỗi từ. Nếu không chúng tôi in`-1`. 

Độ dài từ nhỏ thay đổi hoàn toàn bản chất của vấn đề. Mỗi từ có độ dài tối đa là 10 và các từ viết tắt được giới hạn độ dài tối đa là 4. Điều đó có nghĩa là tổng số các dãy con có thể có trên mỗi từ đủ nhỏ để liệt kê trực tiếp. 

Đối với một từ có độ dài 10, số dãy con có độ dài từ 1 đến 4 là:$$\binom{10}{1} + \binom{10}{2} + \binom{10}{3} + \binom{10}{4}$$

$\binom{10}{1}+\binom{10}{2}+\binom{10}{3}+\binom{10}{4}$Con số này bằng 385. Ngay cả sau khi loại bỏ các từ trùng lặp, mỗi từ chỉ đóng góp vài trăm từ viết tắt dự kiến. Với tối đa 200 từ, chúng tôi cảm thấy thoải mái trong phạm vi đối sánh dựa trên biểu đồ. 

Phần nguy hiểm là không tạo ra các chuỗi tiếp theo. Khó khăn thực sự là xử lý va chạm một cách chính xác. 

Hãy xem xét ví dụ này:```
2
abcd
abdc
```Cả hai từ đều có thể tạo ra`ab`,`ac`,`ad`, và nhiều người khác. Thuật toán tham lam gán chữ viết tắt đầu tiên chưa được sử dụng cho mỗi từ có thể không thành công tùy theo thứ tự. Nếu chúng ta cho`abcd`chữ viết tắt`abd`, từ thứ hai có thể mất tùy chọn duy nhất còn lại mặc dù tồn tại một phép gán toàn cục khác. 

Một trường hợp tinh tế khác xuất hiện khi một từ tạo ra các chuỗi con trùng lặp thông qua các lựa chọn chỉ mục khác nhau. 

Ví dụ:```
1
aaaa
```Trình tự tiếp theo`aa`có thể được hình thành bằng nhiều cách nhưng chỉ được xuất hiện một lần trong danh sách ứng cử viên. Việc quên tính năng loại bỏ trùng lặp sẽ làm phồng biểu đồ và có thể tạo ra các cạnh lặp lại không cần thiết. 

Ngoài ra còn có trường hợp không thể xảy ra:```
3
a
aa
aaa
```Mỗi từ chỉ có thể tạo ra từ viết tắt`a`. Vì các chữ viết tắt phải khác biệt nên kết quả đúng là:```
-1
```Việc triển khai bất cẩn chỉ kiểm tra tính khả thi cục bộ sẽ in sai các chữ viết tắt lặp đi lặp lại. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là quay lui bằng vũ lực. Đối với mỗi từ, hãy tạo tất cả các chữ viết tắt hợp lệ, sau đó thử gán các chữ viết tắt không sử dụng một cách đệ quy. Nếu chúng tôi hoàn thành thành công, chúng tôi sẽ in bài tập. 

Điều này hoạt động vì không gian tìm kiếm là hữu hạn. Vấn đề là yếu tố phân nhánh. Một từ có thể có khoảng 300 từ viết tắt riêng biệt. Trong trường hợp xấu nhất, việc thử tất cả các kết hợp sẽ trở nên lớn về mặt thiên văn:$$300^{200}$$

$300^{200}$Ngay cả khi cắt tỉa, việc quay lại không hạn chế là vô vọng. 

Quan sát quan trọng là đây thực sự không phải là vấn đề về trình tự. Đó là một vấn đề phân công. 

Mỗi từ có một tập hợp các chữ viết tắt được chấp nhận. Chúng ta cần chọn một từ viết tắt riêng biệt cho mỗi từ. Đây chính xác là sự kết hợp lưỡng cực. 

Ta vẽ đồ thị có hai cạnh: 

- Bên trái: chữ. 
- Bên phải: toàn bộ các chữ viết tắt riêng biệt xuất hiện dưới dạng dãy con của một từ nào đó. 

Chúng tôi kết nối một từ với mọi chữ viết tắt mà nó có thể sử dụng. 

Bây giờ nhiệm vụ trở thành tìm một từ phù hợp bao gồm mọi từ. Nếu sự trùng khớp như vậy tồn tại, mỗi cạnh khớp sẽ đưa ra tên viết tắt đã chọn. 

Sự chuyển đổi này rất mạnh mẽ vì sự kết hợp lưỡng cực tối đa sẽ tự động giải quyết các xung đột toàn cầu. Nếu một từ lấy từ viết tắt chặn từ khác, quá trình đường dẫn tăng cường có thể sắp xếp lại các lựa chọn trước đó. 

Biểu đồ đủ nhỏ cho thuật toán Kuhn dựa trên DFS tiêu chuẩn. Có tối đa 200 từ và mỗi từ đóng góp tối đa vài trăm cạnh. Tổng kích thước biểu đồ vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) đệ quy | Quá chậm | 
| Tối ưu | O(VE) | O(E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ. 
2. Với mỗi từ, hãy tạo ra mọi dãy con riêng biệt có độ dài từ 1 đến 4. 

Chúng tôi liệt kê các tập hợp con của các vị trí bằng cách sử dụng đệ quy hoặc kết hợp. Vì thứ tự ban đầu phải được giữ nguyên nên việc chọn chỉ mục sẽ tự động tạo ra các chuỗi con hợp lệ. 
3. Nén tất cả các chữ viết tắt riêng biệt thành ID số nguyên. 

Các thuật toán so khớp hoạt động hiệu quả hơn với các chỉ mục nhỏ gọn hơn là các chuỗi thô. 
4. Xây dựng biểu đồ lưỡng cực. 

Phía bên trái chứa một nút cho mỗi từ. Phía bên phải chứa một nút cho mỗi chữ viết tắt duy nhất. Thêm một cạnh từ từ`i`viết tắt`j`nếu chữ viết tắt đó có thể được hình thành từ từ. 
5. Chạy thuật toán Kuhn để tìm kết quả khớp lưỡng cực tối đa. 

Đối với mỗi từ, hãy cố gắng tìm một đường dẫn tăng thêm để gán cho nó một từ viết tắt duy nhất. Nếu một từ viết tắt đã được sử dụng, hãy cố gắng định tuyến lại từ hiện tại phù hợp ở nơi khác. 
6. Đếm xem có bao nhiêu từ được ghép thành công. 

Nếu kích thước phù hợp nhỏ hơn`n`, không có phép gán hợp lệ nào tồn tại và chúng tôi in`-1`. 
7. Ngược lại, hãy xây dựng lại câu trả lời. 

Mỗi chữ viết tắt phù hợp tương ứng với chính xác một từ. Xuất các chữ viết tắt đó theo thứ tự đầu vào. 

### Tại sao nó hoạt động 

Biểu đồ chứa chính xác các lựa chọn hợp lệ cho mỗi từ. Việc so khớp sẽ chọn các cạnh sao cho không có hai từ nào có cùng chữ viết tắt. 

Nếu tồn tại một sự kết hợp hoàn hảo thì mỗi từ sẽ nhận được một chữ viết tắt hợp lệ duy nhất. Ngược lại, bất kỳ giải pháp hợp lệ nào đều tương ứng trực tiếp với một kết quả phù hợp bao gồm tất cả các từ. 

Thuật toán Kuhn liên tục tìm các đường tăng cường. Mỗi đường dẫn tăng cường sẽ tăng số lượng từ phù hợp lên một trong khi vẫn giữ được tính duy nhất. Vì các đường dẫn tăng cường là đặc điểm cơ bản của việc so sánh hai bên tối đa nên kết quả so khớp cuối cùng là tối đa có thể. Nếu kích thước của nó bằng`n`, một bài tập hoàn chỉnh tồn tại. 

## Giải pháp Python```python
import sys
from itertools import combinations

input = sys.stdin.readline

def generate(word):
    s = set()
    m = len(word)

    for length in range(1, min(4, m) + 1):
        for idxs in combinations(range(m), length):
            cur = ''.join(word[i] for i in idxs)
            s.add(cur)

    return list(s)

def solve():
    n = int(input())
    words = [input().strip() for _ in range(n)]

    abbr_id = {}
    abbr_list = []

    graph = []

    for word in words:
        subs = generate(word)

        edges = []

        for sub in subs:
            if sub not in abbr_id:
                abbr_id[sub] = len(abbr_list)
                abbr_list.append(sub)

            edges.append(abbr_id[sub])

        graph.append(edges)

    m = len(abbr_list)

    match_right = [-1] * m

    def dfs(v, vis):
        for to in graph[v]:
            if vis[to]:
                continue

            vis[to] = True

            if match_right[to] == -1 or dfs(match_right[to], vis):
                match_right[to] = v
                return True

        return False

    matched = 0

    for v in range(n):
        vis = [False] * m

        if dfs(v, vis):
            matched += 1

    if matched != n:
        print(-1)
        return

    ans = [""] * n

    for abbr_idx, word_idx in enumerate(match_right):
        if word_idx != -1:
            ans[word_idx] = abbr_list[abbr_idx]

    print('\n'.join(ans))

solve()
```các`generate`hàm liệt kê tất cả các dãy con có độ dài từ 1 đến 4. Sử dụng`combinations`thuận tiện vì việc chọn chỉ số tăng sẽ tự động giữ nguyên thứ tự ký tự. các`set`loại bỏ các bản sao như nhiều cách để hình thành`"aa"`từ`"aaaa"`. 

Giai đoạn xây dựng đồ thị gán cho mỗi chữ viết tắt duy nhất một ID số nguyên. Việc nén này rất quan trọng vì nhiều từ có thể có cùng một từ viết tắt. 

Bản thân việc so khớp sử dụng cách triển khai đường dẫn tăng cường DFS tiêu chuẩn của thuật toán Kuhn. Cuộc gọi đệ quy là bước quan trọng. Khi một từ viết tắt đã được sử dụng, chúng tôi cố gắng di chuyển từ hiện được gán sang một nơi khác. Khả năng sắp xếp lại các nhiệm vụ về cơ bản là điều làm cho việc kết hợp trở nên mạnh mẽ hơn so với việc lựa chọn tham lam. 

các`vis`mảng phải được tạo lại cho mỗi lần thử DFS. Việc sử dụng lại nó qua các lần lặp sẽ chặn các đường dẫn tăng cường hợp lệ một cách không chính xác. 

Cuối cùng, kết quả khớp được lưu trữ từ chữ viết tắt sang từ. Sau khi thuật toán kết thúc, chúng tôi đảo ngược ánh xạ đó thành thứ tự đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
privet
spasibo
codeforces
java
marmelad
normalno
```Số chữ viết tắt được tạo ra rất lớn nên chúng tôi chỉ theo dõi các quyết định đối sánh thành công. 

| Lời | Một số từ viết tắt ứng cử viên | Được chọn | 
| --- | --- | --- | 
| cây thủy lạp |`pr`,`pret`,`pvt`,`riv`|`pret`| 
| spasibo |`sps`,`spo`,`sbo`|`sps`| 
| lực lượng mật mã |`cdfs`,`code`,`cofs`|`cdfs`| 
| java |`j`,`ja`,`java`|`java`| 
| mứt cam |`mama`,`mard`,`mela`|`mama`| 
| bình thườngkhông |`norm`,`nrml`,`nalo`|`norm`| 

Đầu ra:```
pret
sps
cdfs
java
mama
norm
```Dấu vết này cho thấy thuộc tính trung tâm của sự phù hợp. Nhiều từ có thể chia sẻ các ứng cử viên, nhưng nhiệm vụ cuối cùng đảm bảo tính duy nhất trên toàn cầu. 

### Ví dụ 2 

đầu vào:```
3
a
aa
aaa
```| Lời | Viết tắt của ứng viên | 
| --- | --- | 
| một |`a`| 
| aa |`a`,`aa`| 
| aaa |`a`,`aa`,`aaa`| 

Quá trình phù hợp có thể: 

| Bước | Được chỉ định viết tắt | 
| --- | --- | 
| Nối từ 1 |`a`| 
| Nối từ 2 |`aa`| 
| Nối từ 3 |`aaa`| 

Đầu ra:```
a
aa
aaa
```Điều này chứng tỏ tại sao các chuỗi dài hơn 1 lại quan trọng. Dù mọi lời nói đều chia sẻ`a`, những từ lớn hơn có thể chuyển sang các từ viết tắt thay thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(VE) | Thuật toán Kuhn trên đồ thị hai bên | 
| Không gian | O(E) | Danh sách kề cộng với mảng phù hợp | 

Đây,`V`là số từ cộng với chữ viết tắt, và`E`là số cặp từ viết tắt hợp lệ. 

Mỗi từ tạo ra tối đa 385 chuỗi con, vì vậy:$$E \le 200 \times 385$$

$200\times385$Đây là mức nhỏ thoải mái để triển khai đối sánh dựa trên DFS trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import combinations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def generate(word):
        s = set()
        m = len(word)

        for length in range(1, min(4, m) + 1):
            for idxs in combinations(range(m), length):
                s.add(''.join(word[i] for i in idxs))

        return list(s)

    n = int(input())
    words = [input().strip() for _ in range(n)]

    abbr_id = {}
    abbr_list = []
    graph = []

    for word in words:
        edges = []

        for sub in generate(word):
            if sub not in abbr_id:
                abbr_id[sub] = len(abbr_list)
                abbr_list.append(sub)

            edges.append(abbr_id[sub])

        graph.append(edges)

    m = len(abbr_list)

    match_right = [-1] * m

    def dfs(v, vis):
        for to in graph[v]:
            if vis[to]:
                continue

            vis[to] = True

            if match_right[to] == -1 or dfs(match_right[to], vis):
                match_right[to] = v
                return True

        return False

    matched = 0

    for v in range(n):
        vis = [False] * m

        if dfs(v, vis):
            matched += 1

    if matched != n:
        return "-1"

    ans = [""] * n

    for i, v in enumerate(match_right):
        if v != -1:
            ans[v] = abbr_list[i]

    return "\n".join(ans)

# provided-style sample
out = run(
"""6
privet
spasibo
codeforces
java
marmelad
normalno
"""
)

assert len(out.splitlines()) == 6

# minimum case
assert run(
"""1
a
"""
) == "a"

# impossible case
assert run(
"""2
a
a
"""
) == "-1"

# repeated-character subsequences
out = run(
"""3
a
aa
aaa
"""
)

lines = out.splitlines()
assert len(set(lines)) == 3

# boundary length 10
out = run(
"""2
abcdefghij
jihgfedcba
"""
)

assert len(out.splitlines()) == 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Từ đơn`a`|`a`| Đầu vào kích thước tối thiểu | 
| Hai giống hệt nhau`a`từ |`-1`| Phát hiện nhiệm vụ không thể | 
|`a`,`aa`,`aaa`| Ba chữ viết tắt riêng biệt | Phù hợp với logic phân công lại | 
| Hai từ dài 10 từ | Bất kỳ cặp hợp lệ nào | Tạo chuỗi con tối đa | 

## Vỏ cạnh 

Hãy xem xét trường hợp ký tự lặp lại:```
1
aaaa
```Trình tự tiếp theo`aa`có thể được tạo bằng nhiều cặp chỉ mục: 

- vị trí`(0,1)`- vị trí`(0,2)`- vị trí`(1,3)`Thuật toán lưu trữ các chuỗi con trong một tập hợp trước khi thêm các cạnh của biểu đồ. Kết quả là đồ thị chỉ chứa một cạnh của`aa`. Điều này ngăn chặn các cạnh trùng lặp không cần thiết và giữ cho hoạt động khớp luôn rõ ràng. 

Bây giờ hãy xem xét trường hợp va chạm không thể xảy ra:```
2
a
a
```Cả hai từ chỉ tạo ra một từ viết tắt: 

| Lời | Ứng viên | 
| --- | --- | 
| 1 |`a`| 
| 2 |`a`| 

Việc kết hợp có thể bao gồm nhiều nhất một từ. Sau khi chạy thuật toán Kuhn, kích thước phù hợp là`1 < 2`, do đó thuật toán in`-1`. 

Cuối cùng, hãy xem xét trường hợp tham lam thất bại nhưng kết hợp thành công:```
2
abcd
abdc
```Giả sử từ đầu tiên ban đầu được lấy`abd`. Từ thứ hai sau này có thể cần chữ viết tắt đó. Trong DFS, thuật toán khám phá các đường dẫn tăng cường và có thể gán lại từ đầu tiên cho một từ viết tắt hợp lệ khác như`abc`. Tính linh hoạt này chính là lý do tại sao kết hợp hai bên giải quyết chính xác ràng buộc về tính duy nhất toàn cầu.
