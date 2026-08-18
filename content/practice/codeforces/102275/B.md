---
title: "CF 102275B - Chuỗi bit dưới dạng dịch vụ"
description: "Chúng ta cần xây dựng một chuỗi nhị phân có độ dài (N). Mỗi yêu cầu đưa ra một khoảng ([X,Y]) và các ký tự bên trong khoảng đó phải đọc giống hệt nhau từ cả hai đầu."
date: "2026-08-17T17:52:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102275
codeforces_index: "B"
codeforces_contest_name: "2019 Facebook Hacker Cup, Round 2"
rating: 0
weight: 102275
solve_time_s: 737
verified: true
draft: false
---

[CF 102275B - Chuỗi bit dưới dạng dịch vụ](https://codeforces.com/problemset/problem/102275/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một chuỗi nhị phân có độ dài (N). Mỗi yêu cầu đưa ra một khoảng ([X,Y]) và các ký tự bên trong khoảng đó phải đọc giống hệt nhau từ cả hai đầu. Chuỗi cuối cùng phải đáp ứng mọi yêu cầu về bảng màu như vậy, đồng thời làm cho tổng số số 0 và số 1 càng gần nhau càng tốt. 

Đầu vào chứa một số hoa hồng độc lập. Đối với mỗi cái, (N) là độ dài chuỗi, (M) là số lượng yêu cầu về bảng màu và các cặp (M) tiếp theo mô tả điểm cuối của chúng. Đầu ra phải chứa một chuỗi tối ưu hợp lệ cho mỗi khoản hoa hồng, có tiền tố là số trường hợp của nó. Có thể có nhiều chuỗi tối ưu nên chúng ta chỉ cần xây dựng một chuỗi. 

Các ràng buộc này đủ lớn để loại trừ việc suy luận trực tiếp trên tất cả các chuỗi có thể. Với (N\le 4000), có (2^{4000}) chuỗi có thể, do đó việc tìm kiếm toàn diện là hoàn toàn không khả thi. Cũng có thể có (10.000) yêu cầu, do đó việc xử lý mọi yêu cầu bằng cách sao chép nhiều lần hoặc xây dựng lại các cấu trúc lớn sẽ rất lãng phí. Cuộc thi chính thức sử dụng giới hạn thời gian 15 giây và 512 MB bộ nhớ, điều này làm cho giải pháp (O(N^2)) trở nên thiết thực nhưng không có nhiều lý do để chấp nhận cách tiếp cận theo cấp số nhân hoặc (O(MN^2)). 

Trường hợp cạnh đầu tiên là (M=0). Ví dụ, với đầu vào`1 0`, không có hạn chế nào cả, vì vậy`0`hoặc`1`là tối ưu. Việc triển khai bất cẩn cho rằng mọi vị trí đều thuộc về một thành phần bị ràng buộc có thể thất bại do tạo ra một phép gán trống hoặc do quên các thành phần đơn lẻ. 

Trường hợp cạnh thứ hai là một palindrome có độ dài một, chẳng hạn như`1 1`. Nó không áp đặt sự bình đẳng nào cả vì khoảng chỉ có một ký tự. Vì`N=3, M=1`với yêu cầu`2 2`, câu trả lời tối ưu có thể là`010`, với hai số 0 và một số một. Việc coi tâm của một palindrome là sự bình đẳng giữa hai vị trí khác nhau sẽ tạo ra một ràng buộc không tồn tại. 

Trường hợp cạnh thứ ba là một palindrome có độ dài lẻ. Vì`N=5`với yêu cầu`1 5`, vị trí`1`Và`5`phải phù hợp, vị trí`2`Và`4`phải phù hợp và vị trí`3`là miễn phí. Chuỗi`00100`là tối ưu. Một vòng lặp bất cẩn luôn xử lý các cặp cho đến khi hai con trỏ giao nhau có thể vô tình truy cập vào trung tâm hai lần hoặc tạo một chỉ mục không hợp lệ. 

Trường hợp cạnh thứ tư là các yêu cầu chồng chéo. Vì`N=6`, yêu cầu`[1,5]`Và`[2,6]`ngụ ý`B1=B5`,`B2=B4`,`B2=B6`, Và`B3=B5`. Các đẳng thức tương tác bắc cầu, do đó một số vị trí không được ghép trực tiếp bởi cùng một bảng màu sẽ trở thành bằng nhau. Việc xử lý mọi yêu cầu một cách độc lập và gán bit ngay lập tức có thể vi phạm mối quan hệ bắc cầu này. 

Trường hợp tinh tế cuối cùng xảy ra khi một số yêu cầu có cùng giá trị (X+Y). Ví dụ,`[1,7]`Và`[2,6]`cả hai đều có tổng điểm cuối (8). Khoảng thứ hai hoàn toàn nằm trong khoảng thứ nhất, vì vậy khoảng thứ nhất đã áp đặt mọi đẳng thức mà khoảng thứ hai yêu cầu. Chỉ giữ lại khoảng rộng nhất cho mỗi tổng điểm cuối sẽ loại bỏ công việc dư thừa mà không thay đổi các ràng buộc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là thử mọi chuỗi nhị phân, từ chối nó nếu bất kỳ khoảng thời gian bắt buộc nào không phải là một bảng màu và trong số các chuỗi còn sót lại, hãy giữ chuỗi có chênh lệch 0-1 nhỏ nhất. Điều này đúng vì mọi chuỗi có thể đều được xem xét. Vấn đề của nó là không gian tìm kiếm: có (2^N) chuỗi. Tại (N=4000), tức là có (2^{4000}) ứng viên. Nếu mọi ứng cử viên đều được kiểm tra theo các khoảng (M) và mỗi lần kiểm tra bảng màu sẽ quét tối đa (N) ký tự, thì trường hợp xấu nhất là theo thứ tự so sánh ký tự (2^{4000}\cdot 10.000\cdot4.000). Ngay cả việc tạo ra các ứng cử viên cũng đã là không thể. 

Quan sát hữu ích đầu tiên là yêu cầu về bảng màu không thực sự ràng buộc các giá trị một cách độc lập. Nó tạo ra những hạn chế về sự bình đẳng. Vì`[l,r]`, chúng tôi yêu cầu 

[ 
B_l=B_r,\quad B_{l+1}=B_{r-1},\quad\ldots 
] 

Do đó, toàn bộ vấn đề trước tiên có thể được xem dưới dạng biểu đồ về các vị trí. Mọi đẳng thức bắt buộc đều là một cạnh và mọi thành phần được kết nối phải nhận được một bit. 

Có một quan sát thứ hai giữ nguyên cấu trúc đẳng thức ở (O(N^2)) thay vì (O(MN)). Một cặp đối xứng ((i,j)) có tổng điểm cuối duy nhất (i+j). Đối với một bảng màu`[l,r]`, mọi đẳng thức mà nó tạo ra đều có cùng tổng (l+r). Nếu hai yêu cầu có cùng một tổng, hãy nói`[l,r]`Và`[l',r']`, các khoảng của chúng được lồng vào nhau vì (r=s-l). Cái có điểm cuối bên trái nhỏ hơn hoàn toàn chứa cái kia, vì vậy chỉ khoảng rộng nhất cho tổng đó mới quan trọng. 

Chỉ có (2N-1) tổng điểm cuối có thể. Sau khi giữ lại khoảng rộng nhất cho mỗi tổng, chúng tôi tạo các cặp đối xứng của nó và hợp nhất các điểm cuối của chúng với cấu trúc liên kết tập hợp rời rạc. Trên tất cả các tổng, mỗi cặp vị trí không có thứ tự có thể xuất hiện nhiều nhất một lần, vì tổng của nó là duy nhất. Do đó, nhiều nhất (\binom N2) các cặp đẳng thức được xử lý, tạo ra thời gian (O(N^2\alpha(N))) cho giai đoạn hợp nhất. 

Sau khi các đẳng thức được giải quyết, giả sử các thành phần được kết nối có kích thước (s_1,s_2,\ldots,s_k). Chọn một thành phần để chứa`1`đóng góp toàn bộ kích thước của nó vào số lượng đơn vị. Do đó, chúng ta cần chọn kích thước thành phần có tổng càng gần (N/2 càng tốt). Đây là vấn đề về tổng tập hợp con, nhưng (N\le4000) cho phép DP tập hợp bit đặc biệt nhỏ gọn. Một số nguyên Python có thể biểu thị tất cả các tổng có thể truy cập dưới dạng bit và quá trình chuyển đổi cho một thành phần có (các) kích thước chỉ đơn giản là`reachable |= reachable << s`. 

Giải pháp brute-force hoạt động hiệu quả vì nó kiểm tra rõ ràng mọi nhiệm vụ. Nó thất bại vì có nhiều bài tập theo cấp số nhân. Quan sát đẳng thức thay đổi hoàn toàn vấn đề: đầu tiên thu gọn tất cả các vị trí buộc phải đồng ý, sau đó giải quyết vấn đề tổng tập hợp con chỉ trên các kích thước thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^N MN)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N^2\alpha(N)+M+N^2)) | (O(N^2)) trường hợp xấu nhất đối với các bit DP được lưu trữ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng palindrome (M) và nhóm chúng theo giá trị (s=X+Y). Với mỗi tổng, chỉ giữ lại số nhỏ nhất (X). Vì (Y=s-X), khoảng này là khoảng rộng nhất có tổng đó và mọi khoảng khác có cùng tổng đều nằm bên trong nó. 
2. Tạo DSU chứa một phần tử cho mỗi vị trí chuỗi. Ban đầu, mỗi vị trí đều là thành phần riêng của nó vì không có sự bình đẳng nào được thiết lập. 
3. Đối với mỗi khoảng thời gian giữ lại`[l,r]`, hợp nhất`l`với`r`, sau đó`l+1`với`r-1`, tiếp tục cho đến khi hai vị trí gặp nhau. Đây chính xác là những đẳng thức được yêu cầu bởi palindrome đó. 
4. Đếm kích thước của mỗi thành phần DSU cuối cùng. Mọi vị trí bên trong một thành phần phải nhận được cùng một bit, do đó, một thành phần có (các) kích thước hoạt động giống như một phần tử có trọng lượng không thể phân chia được. 
5. Chạy DP tổng tập hợp con trên các kích thước thành phần này. Biểu thị tập hợp các tổng có thể truy cập bằng các bit của một số nguyên Python. Bit (x) được đặt chính xác khi một số tập hợp thành phần có tổng kích thước (x). Bắt đầu từ tổng bằng 0, một thành phần có kích thước (s) sẽ thay đổi tập hợp có thể truy cập từ`dp`ĐẾN`dp | (dp << s)`. 
6. Tìm tổng có thể đạt được gần nhất với (N/2). Nếu tổng này là (z), việc gán`1`đối với các thành phần được chọn đó sẽ cho ra (z) số 1 và (N-z) số 0, do đó, chênh lệch thu được là (|N-2z|), là giá trị nhỏ nhất khi chọn (z). 
7. Xây dựng lại các thành phần tạo thành tập hợp con đã chọn. Lưu trữ bit DP sau mỗi thành phần. Bắt đầu từ mục tiêu đã chọn và xử lý các thành phần ngược lại, một thành phần sẽ được chọn nếu mục tiêu không thể được hình thành nếu không có nó. Khi được chọn, hãy trừ kích thước của nó khỏi mục tiêu. 
8. Chỉ định`1`đến mọi vị trí có thành phần đã được chọn và`0`đến mọi vị trí khác. Tiền tố chuỗi kết quả với`Case #k:`theo yêu cầu của định dạng đầu ra. 

### Tại sao nó hoạt động 

Bất biến DSU là hai vị trí nằm trong cùng một thành phần một cách chính xác khi các ràng buộc đẳng thức được xử lý cho đến nay buộc chúng phải có cùng một bit. Mỗi khoảng palindrome đóng góp chính xác các đẳng thức điểm cuối đối xứng của nó, vì vậy sau khi tất cả các khoảng được xử lý, mọi chuỗi hợp lệ phải không đổi trên mỗi thành phần DSU. Ngược lại, việc gán một bit cho mỗi thành phần sẽ tự động đáp ứng mọi đẳng thức và do đó mọi yêu cầu về bảng màu. 

Giai đoạn tổng hợp con xem xét chính xác số lượng có thể có của một. Một thành phần chỉ có thể hoàn toàn bằng 0 hoặc hoàn toàn bằng một, do đó, việc chọn các thành phần có tổng kích thước (z) sẽ tạo ra chính xác (z). DP tìm mọi (z) như vậy và chọn giá trị có thể truy cập gần nhất với (N/2) để giảm thiểu (|z-(N-z)|). Do đó, chuỗi được xây dựng vừa hợp lệ vừa tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = [-1] * n

    def find(self, x):
        parent = self.parent
        while parent[x] >= 0:
            if parent[parent[x]] >= 0:
                parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(self, a, b):
        parent = self.parent
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return

        if parent[a] > parent[b]:
            a, b = b, a

        parent[a] += parent[b]
        parent[b] = a

def solve_case(n, intervals):
    # For each sum l + r, only the widest interval is necessary.
    widest = [n + 1] * (2 * n + 1)

    for l, r in intervals:
        s = l + r
        if l < widest[s]:
            widest[s] = l

    dsu = DSU(n)

    # Positions are zero-based here.
    # For a fixed sum s, r = s - l, so the interval is determined by l.
    for s in range(2, 2 * n + 1):
        l = widest[s]
        if l == n + 1:
            continue

        l -= 1
        r = s - l - 2

        while l < r:
            dsu.union(l, r)
            l += 1
            r -= 1

    # Compress all components and obtain their sizes.
    components = []
    root_to_id = {}
    comp_id = [-1] * n

    for i in range(n):
        root = dsu.find(i)
        if root not in root_to_id:
            root_to_id[root] = len(components)
            components.append(-dsu.parent[root])
        comp_id[i] = root_to_id[root]

    k = len(components)

    # Bitset subset sum.
    # dp bit x == 1 iff sum x is reachable.
    dp_history = [1]
    dp = 1

    for size in components:
        dp |= dp << size
        dp_history.append(dp)

    target = n // 2

    # Find the reachable sum closest to n / 2.
    best = target
    while best >= 0 and ((dp >> best) & 1) == 0:
        best -= 1

    # For odd n, the upper side can be equally good.
    upper = target + 1
    if upper <= n and ((dp >> upper) & 1):
        if abs(n - 2 * upper) < abs(n - 2 * best):
            best = upper

    # Reconstruct the selected components.
    selected = [False] * k
    cur = best

    for i in range(k, 0, -1):
        size = components[i - 1]

        if ((dp_history[i - 1] >> cur) & 1) == 0:
            selected[i - 1] = True
            cur -= size

    answer = ['0'] * n

    for i in range(n):
        if selected[comp_id[i]]:
            answer[i] = '1'

    return ''.join(answer)

def main():
    t = int(input())
    out = []

    for case_no in range(1, t + 1):
        n, m = map(int, input().split())
        intervals = [tuple(map(int, input().split())) for _ in range(m)]

        ans = solve_case(n, intervals)
        out.append(f"Case #{case_no}: {ans}")

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    main()
```các`widest`mảng được lập chỉ mục bởi`l+r`. Giá trị ban đầu của nó là trọng điểm lớn hơn mọi điểm cuối bên trái có thể có. Khi một khoảng được đọc, chúng tôi giữ lại điểm cuối bên trái nhỏ nhất cho tổng của nó. Vì điểm cuối bên phải được xác định bởi tổng nên đây chính xác là khoảng lớn nhất cho tổng đó. 

DSU sử dụng các giá trị âm trong`parent`để lưu trữ kích thước thành phần ở gốc. Điều này tránh một mảng kích thước riêng biệt. Liên kết theo kích thước giữ cho cây nông, và`find`thực hiện nén đường dẫn. 

Việc chuyển đổi từ đầu vào dựa trên một sang vị trí dựa trên 0 xảy ra khi xử lý một khoảng. Đối với một khoảng ban đầu`[l,r]`, sau khi trừ đi một từ`l`, điểm cuối bên phải dựa trên số 0 là`s-l-2`. Vòng lặp hợp nhất các vị trí đối xứng và dừng ở giữa, do đó, một palindrome có độ dài một hoặc độ dài lẻ không tạo ra một cặp không hợp lệ. 

Danh sách thành phần chứa số vị trí trong mỗi thành phần DSU. Quá trình chuyển đổi tổng tập hợp con sử dụng các số nguyên có kích thước tùy ý của Python làm tập hợp bit. Nếu bit (x) xuất hiện trước khi xử lý thành phần có kích thước (s), bit (x+s) sẽ xuất hiện trong`dp << s`. 

các`dp_history`list lưu trữ số tiền có thể truy cập sau mỗi thành phần. Điều này tốn (O(N^2)) bit trong trường hợp xấu nhất, chỉ vài megabyte cho (N=4000). Nó làm cho việc xây dựng lại trở nên đơn giản bởi vì khi xem xét một thành phần có kích thước`size`, chúng tôi có thể kiểm tra xem mục tiêu hiện tại có thể truy cập được trước thành phần đó hay không. Nếu không, thành phần đó phải thuộc tập hợp con đã chọn. 

Không cần xử lý tràn số nguyên trong Python. Các giá trị có khả năng lớn duy nhất là các số nguyên bitset, có kích thước được giới hạn bởi các bit (N+1). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hoa hồng đầu tiên là`N=4, M=0`, do đó không có ràng buộc đẳng thức. Mỗi vị trí là thành phần riêng của nó. 

| Bước | Kích thước thành phần | Số tiền có thể tiếp cận | Mục tiêu được chọn | 
| --- | --- | --- | --- | 
| Bắt đầu |`[1,1,1,1]`|`{0}`|`2`| 
| Thêm 1 |`[1]`|`{0,1}`|`2`| 
| Thêm 1 |`[1,1]`|`{0,1,2}`|`2`| 
| Thêm 1 |`[1,1,1]`|`{0,1,2,3}`|`2`| 
| Thêm 1 |`[1,1,1,1]`|`{0,1,2,3,4}`|`2`| 
| Tái thiết | bốn thành phần đơn lẻ | hai lựa chọn |`2`những cái | 

Thuật toán có thể chọn bất kỳ hai thành phần đơn lẻ nào, tạo ra`0011`,`0101`, hoặc một cách sắp xếp khác với hai số 0 và hai số một. Điều bất biến ở đây là không có yêu cầu về bảng màu, mọi vị trí vẫn có thể được chỉ định độc lập. 

### Mẫu 2 

Hoa hồng thứ hai là`N=6, M=1`với yêu cầu`[1,6]`. Tổng điểm cuối của nó là (7), vì vậy khoảng rộng nhất của tổng (7) là`[1,6]`. 

| Bước | Cặp hợp nhất | Thành phần sau khi hợp nhất | Kích thước thành phần | 
| --- | --- | --- | --- | 
| Bắt đầu | không |`{1},{2},{3},{4},{5},{6}`|`[1,1,1,1,1,1]`| 
| 1 |`1 = 6`|`{1,6},{2},{3},{4},{5}`|`[2,1,1,1,1]`| 
| 2 |`2 = 5`|`{1,6},{2,5},{3},{4}`|`[2,2,1,1]`| 
| 3 |`3 = 4`|`{1,6},{2,5},{3,4}`|`[2,2,2]`| 
| DP | kích thước`2,2,2`|`{0,2,4,6}`| mục tiêu`3`, tốt nhất`2`| 
| Tái thiết | chọn một thành phần cỡ 2 | hai cái | sự khác biệt`2`| 

Một kết quả tối ưu hợp lệ là`110011`. Mọi cặp đối xứng trong bảng màu đầy đủ đều phù hợp và chuỗi chứa bốn số 0 và hai số 1, tạo ra chênh lệch tối thiểu có thể có là (2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2\alpha(N)+M+KN)) | Tối đa các cặp đối xứng (\binom N2) được xử lý và các phép toán tổng tập hợp con sử dụng số nguyên bit (O(N)) cho tối đa (K\le N) thành phần | 
| Không gian | (O(N^2)) bit cộng với (O(N+M)) bộ nhớ phụ | Lịch sử DP lưu trữ tối đa (N) bitset, mỗi bit chứa tối đa (N+1) bit | 

Pha đẳng thức được giới hạn bởi số cặp vị trí không có thứ tự, khoảng 8 triệu khi (N=4000). Giai đoạn tổng tập hợp con đặc biệt hiệu quả trong Python vì các phép dịch và phép toán OR chạy trên các từ máy được đóng gói thay vì một đối tượng Python trên mỗi tổng. Các ràng buộc được thiết kế sao cho cách tiếp cận bậc hai này là thực tế, không giống như việc liệt kê (2^N) chuỗi có thể. 

## Trường hợp thử nghiệm 

Các thử nghiệm bên dưới sử dụng trình xác nhận thay vì so sánh với một chuỗi đầu ra cố định, vì bài toán chấp nhận nhiều chuỗi tối ưu khác nhau. Trình xác thực sẽ kiểm tra xem đầu ra có phải là nhị phân hay không, mỗi khoảng được yêu cầu là một bảng màu và hiệu số 0 bằng với mức tối ưu thực sự thu được bằng phép tính tổng tập hợp con độc lập nhỏ trên các thành phần đẳng thức của đầu ra.```python
# helper: run solution on input string, return output string
import sys
import io

class DSU:
    def __init__(self, n):
        self.p = [-1] * n

    def find(self, x):
        while self.p[x] >= 0:
            if self.p[self.p[x]] >= 0:
                self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.p[a] > self.p[b]:
            a, b = b, a
        self.p[a] += self.p[b]
        self.p[b] = a

def solve_case(n, intervals):
    widest = [n + 1] * (2 * n + 1)

    for l, r in intervals:
        s = l + r
        widest[s] = min(widest[s], l)

    dsu = DSU(n)

    for s in range(2, 2 * n + 1):
        l = widest[s]
        if l == n + 1:
            continue

        l -= 1
        r = s - l - 2

        while l < r:
            dsu.union(l, r)
            l += 1
            r -= 1

    root_id = {}
    comp = [-1] * n
    sizes = []

    for i in range(n):
        root = dsu.find(i)
        if root not in root_id:
            root_id[root] = len(sizes)
            sizes.append(-dsu.p[root])
        comp[i] = root_id[root]

    dp = 1
    hist = [dp]

    for s in sizes:
        dp |= dp << s
        hist.append(dp)

    target = n // 2
    best = None

    for x in range(n + 1):
        if (dp >> x) & 1:
            if best is None or abs(n - 2 * x) < abs(n - 2 * best):
                best = x

    chosen = [False] * len(sizes)
    cur = best

    for i in range(len(sizes), 0, -1):
        s = sizes[i - 1]
        if ((hist[i - 1] >> cur) & 1) == 0:
            chosen[i - 1] = True
            cur -= s

    ans = ['0'] * n
    for i in range(n):
        if chosen[comp[i]]:
            ans[i] = '1'

    return ''.join(ans)

def solution(inp):
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    t = int(sys.stdin.readline())
    out = []

    for case_no in range(1, t + 1):
        n, m = map(int, sys.stdin.readline().split())
        intervals = [
            tuple(map(int, sys.stdin.readline().split()))
            for _ in range(m)
        ]
        out.append(f"Case #{case_no}: {solve_case(n, intervals)}")

    sys.stdin = old_stdin
    return '\n'.join(out)

def validate(inp):
    lines = inp.strip().splitlines()
    it = iter(lines)
    t = int(next(it))

    expected_cases = []

    for _ in range(t):
        n, m = map(int, next(it).split())
        intervals = [tuple(map(int, next(it).split())) for _ in range(m)]
        expected_cases.append((n, intervals))

    output = solution(inp).splitlines()
    assert len(output) == t

    for case_no, ((n, intervals), line) in enumerate(
        zip(expected_cases, output), 1
    ):
        prefix = f"Case #{case_no}: "
        assert line.startswith(prefix)
        s = line[len(prefix):]

        assert len(s) == n
        assert set(s) <= {'0', '1'}

        for l, r in intervals:
            part = s[l - 1:r]
            assert part == part[::-1]

        # Build equality components independently.
        dsu = DSU(n)
        for l, r in intervals:
            l -= 1
            r -= 1
            while l < r:
                dsu.union(l, r)
                l += 1
                r -= 1

        sizes = {}
        for i in range(n):
            root = dsu.find(i)
            sizes[root] = sizes.get(root, 0) + 1

        dp = 1
        for size in sizes.values():
            dp |= dp << size

        ones = s.count('1')
        best = min(
            abs(n - 2 * x)
            for x in range(n + 1)
            if (dp >> x) & 1
        )

        assert abs(n - 2 * ones) == best

# Provided samples.
sample = """6
4 0
6 1
1 6
4 2
1 2
2 4
5 3
3 5
2 2
2 4
10 5
3 6
1 4
6 8
5 9
9 10
25 10
17 20
"""

validate(sample)

# Minimum-size input.
validate("""1
1 0
""")

# A length-one palindrome must impose no equality.
validate("""1
3 1
2 2
""")

# Full palindrome with odd length, exercising the center position.
validate("""1
5 1
1 5
""")

# Many overlapping and nested intervals.
validate("""1
8 5
1 8
2 7
3 6
1 5
4 4
""")

# Boundary-heavy case, with intervals touching both ends.
validate("""1
10 6
1 2
9 10
1 10
2 9
3 8
4 7
""")

# Large case with no constraints.
large = "1\n4000 0\n"
validate(large)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 0`| Bất kỳ chuỗi một bit nào | Tối thiểu (N), không có ràng buộc | 
|`1 / 3 1 / 2 2`| Bất kỳ chuỗi ba bit tối ưu nào | Palindrome dài một | 
|`1 / 5 1 / 1 5`| Bất kỳ bảng màu tối ưu nào | Xử lý trung tâm có độ dài lẻ | 
|`1 / 8 5 / overlapping intervals`| Bất kỳ tối ưu hợp lệ nào | Hợp nhất DSU bắc cầu và các ràng buộc lồng nhau | 
|`1 / 10 6 / boundary intervals`| Bất kỳ tối ưu hợp lệ nào | Ranh giới dựa trên một và các ràng buộc điểm cuối chồng chéo | 
|`1 / 4000 0`| Bất kỳ chuỗi 4000-bit cân bằng nào | Hành vi DP tối đa (N) và độc lập bậc hai | 

## Vỏ cạnh 

cho`1 0`, DSU bắt đầu bằng một thành phần có kích thước một. DP tổng tập hợp con đạt cả 0 và 1, vì vậy nó chọn mục tiêu có chênh lệch bằng 0. Do đó, đầu ra được xây dựng lại là một bit hợp lệ. 

Vì`3 1`với yêu cầu`2 2`, khoảng được lưu trữ có tổng (4), nhưng sau khi chuyển đổi nó thành tọa độ dựa trên 0, vị trí bên trái và bên phải giống hệt nhau. Điều kiện vòng lặp`l < r`là sai ngay lập tức nên không có phép kết nào được thực hiện. Tất cả ba vị trí vẫn độc lập và pha tổng hợp con chọn hai vị trí cho một bit và một vị trí cho bit kia, tạo ra sự khác biệt. 

Vì`5 1`với yêu cầu`1 5`, DSU hợp nhất các vị trí`1`Và`5`, sau đó`2`Và`4`, trong khi rời khỏi vị trí`3`một mình. Do đó, kích thước thành phần là`2,2,1`. Số lượng một lần có thể truy cập bao gồm`0,1,2,3,4,5`, vì vậy mục tiêu hai hoặc ba mang lại sự khác biệt một. Một chuỗi kết quả như`00100`là một palindrome và có ba số 0 và hai số một. 

Đối với các yêu cầu chồng chéo như`1 8`,`2 7`, Và`3 6`, mỗi khoảng đóng góp các đẳng thức đối xứng của nó. DSU kết hợp các đẳng thức một cách bắc cầu, vì vậy nếu một yêu cầu kết nối các vị trí`1`Và`8`, và một cái khác cuối cùng kết nối`8`với một vị trí khác, cả ba đều thuộc cùng một thành phần. Việc gán cuối cùng được thực hiện cho mỗi thành phần, do đó, không có lựa chọn nào sau này có thể vô tình đưa ra các bit khác nhau cho các vị trí buộc phải bằng nhau. 

Đối với nhiều khoảng có cùng tổng điểm cuối, chỉ khoảng rộng nhất được giữ lại. Giả sử các yêu cầu là`[1,7]`Và`[2,6]`. Cả hai đều có tổng là tám. Khoảng đầu tiên tạo ra các cặp`(1,7)`,`(2,6)`, Và`(3,5)`, do đó khoảng thứ hai không đóng góp gì mới. Việc loại bỏ nó là an toàn vì đồ thị đẳng thức không thay đổi. 

Đối với (N=4000) và (M=0), mọi vị trí đều là thành phần đơn lẻ. Giai đoạn DSU không thực hiện liên kết nào, trong khi bitset DP đạt mọi tổng từ 0 đến 4000. Mục tiêu được chọn chính xác là 2000, do đó đầu ra chứa 2000 số 0 và 2000 số 1. Điều này thực hiện số lượng thành phần lớn nhất có thể và giải thích lý do tại sao biểu diễn tổng tập hợp con số nguyên đóng gói lại thích hợp hơn bảng boolean bậc hai của Python.
