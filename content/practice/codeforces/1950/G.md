---
title: "CF 1950G - Bài Hát Xáo Trộn"
description: "Mỗi bài hát có hai thuộc tính, một thể loại và một tác giả. Sau khi lược bỏ một số bài hát, chúng ta được phép sắp xếp lại các bài hát còn lại tùy ý."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dfs-and-similar", "dp", "graphs", "hashing", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1950
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 937 (Div. 4)"
rating: 1900
weight: 1950
solve_time_s: 82
verified: true
draft: false
---

[CF 1950G - Bài hát xáo trộn](https://codeforces.com/problemset/problem/1950/G) 

**Xếp hạng:** 1900 
**Thẻ:** bitmasks, dfs và tương tự, dp, đồ thị, băm, triển khai, chuỗi 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi bài hát có hai thuộc tính, một thể loại và một tác giả. Sau khi lược bỏ một số bài hát, chúng ta được phép sắp xếp lại các bài hát còn lại tùy ý. Mục tiêu là sắp xếp chúng sao cho mỗi cặp bài hát lân cận có chung ít nhất một thuộc tính: cùng thể loại, cùng tác giả hoặc cả hai. 

Chúng tôi muốn xóa càng ít bài hát càng tốt. Tương tự, chúng tôi muốn giữ lại càng nhiều bài hát càng tốt và tìm hiểu xem liệu những bài hát đó có thể được sắp xếp thành một chuỗi hợp lệ hay không. 

Một cách hữu ích để xem vấn đề là dưới dạng biểu đồ. Tạo một đỉnh cho mỗi bài hát. Kết nối hai bài hát nếu chúng có cùng thể loại hoặc cùng một tác giả. Sau đó, một danh sách nhạc thú vị chính xác là một thứ tự các bài hát trong đó mỗi cặp liên tiếp được kết nối bằng một cạnh. 

Điều đó có nghĩa là chúng ta đang tìm tập con lớn nhất của các đỉnh có thể được sắp xếp thành một đường đi đi qua mọi đỉnh đã chọn đúng một lần. Theo thuật ngữ đồ thị, chúng tôi muốn kích thước của đường đi Hamilton lớn nhất có trong đồ thị. Câu trả lời là:$$n - (\text{maximum number of songs that can appear in such a path})$$Các ràng buộc tiết lộ cách tiếp cận dự định. Số lượng bài hát nhiều nhất là 16, cực kỳ ít. Một giải pháp theo cấp số nhân trong$n$là có thể. Tuyên bố thậm chí còn đưa ra một gợi ý mạnh mẽ: tổng của$2^n$trên tất cả các trường hợp thử nghiệm không vượt quá$2^{16}$. Đây là một thiết lập cổ điển cho lập trình động tập hợp con. 

Độ dài chuỗi dài không quan trọng về mặt thuật toán. Chúng tôi chỉ so sánh thể loại và nhà văn để bình đẳng. Sau khi đọc các chuỗi, mỗi cặp bài hát có thể được kiểm tra độc lập. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một bài hát duy nhất:```
1
pop taylor
```Câu trả lời đúng là 0. Đường đi gồm một đỉnh đã hợp lệ. Việc triển khai yêu cầu ít nhất một cạnh trong đường dẫn sẽ trả về 1 không chính xác. 

Hãy xem xét ba bài hát hoàn toàn không liên quan:```
3
a x
b y
c z
```Không có hai bài hát có thể liền kề. Danh sách phát hợp lệ lớn nhất chứa chính xác một bài hát, vì vậy câu trả lời là 2. Việc triển khai bất cẩn có thể cho rằng mọi thành phần được kết nối đều đóng góp tất cả các đỉnh của nó. 

Hãy xem xét một biểu đồ trong đó giải pháp tốt nhất không sử dụng tất cả các đỉnh từ một thành phần được kết nối:```
4
a x
a y
b y
c z
```Các bài hát 1, 2 và 3 tạo thành một chuỗi, còn bài hát 4 thì tách biệt. Danh sách phát tối ưu giữ các bài hát 1, 2 và 3. Câu trả lời là 1. Chỉ kết nối thôi là chưa đủ; chúng ta cần một đường đi Hamilton thực tế trên tập hợp con đã chọn. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là thử mọi tập hợp con của các bài hát và mọi thứ tự bên trong tập hợp con đó. Đối với một tập hợp con có kích thước$k$, có$k!$các đơn đặt hàng có thể. Trong trường hợp xấu nhất, điều này trở thành$$\sum_{k=0}^{n} \binom{n}{k} k!$$vốn đã rất lớn đối với$n=16$. Ngay cả việc kiểm tra tất cả các hoán vị của 16 bài hát cũng yêu cầu khoảng$2 \times 10^{13}$mệnh lệnh, hoàn toàn không thể thực hiện được. 

Lý do vũ phu hoạt động về mặt khái niệm là thuộc tính mà chúng ta quan tâm chỉ phụ thuộc vào các bài hát liên tiếp theo thứ tự. Thách thức là tránh liệt kê tất cả các thứ tự có thể. 

Quan sát quan trọng là đồ thị chỉ có 16 đỉnh. Bất cứ khi nào một vấn đề hỏi liệu một tập hợp con các đỉnh có thể được sắp xếp thành một đường dẫn hay không, thì mặt nạ bit DP kiểu đường dẫn Hamilton sẽ trở nên tự nhiên. 

Cho phép`dp[mask][last]`có nghĩa là: 

"Sử dụng chính xác các đỉnh trong`mask`, có đường đi hợp lệ có đỉnh cuối cùng là`last`?" 

Nếu một đường đi như vậy tồn tại, chúng ta có thể nối thêm một đỉnh khác`nxt`bất cứ khi nào có một cạnh giữa`last`Và`nxt`. 

Điều này biến phép liệt kê hoán vị thành lập trình động trên các tập hợp con. Mỗi trạng thái đại diện cho nhiều thứ tự có thể có cùng một lúc. 

Kích thước tập hợp con lớn nhất mà một số trạng thái có thể truy cập được sẽ cung cấp số lượng bài hát tối đa mà chúng tôi có thể giữ lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| DP mặt nạ bit tối ưu |$O(2^n \cdot n^2)$|$O(2^n \cdot n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các bài hát. 
2. Xây dựng đồ thị vô hướng trên các bài hát. 

Kết nối bài hát`i`Và`j`nếu họ chia sẻ một thể loại hoặc chia sẻ một nhà văn. 
3. Tạo bảng DP`dp[mask][last]`.`dp[mask][last] = True`có nghĩa là tồn tại một đường dẫn hợp lệ sử dụng chính xác các đỉnh trong`mask`và kết thúc tại`last`. 
4. Khởi tạo tất cả các tập hợp con đơn lẻ. 

Cho mỗi bài hát`i`:`dp[1<<i][i] = True`Một bài hát luôn là một đường dẫn hợp lệ. 
5. Lặp lại tất cả các mặt nạ. 

Đối với mọi trạng thái có thể truy cập`(mask, last)`, hãy thử mở rộng đường dẫn. 
6. Với mọi đỉnh`nxt`chưa có trong`mask`, kiểm tra xem có cạnh nào giữa`last`Và`nxt`. 

Nếu có thì đánh dấu:`dp[mask | (1<<nxt)][nxt] = True`Điều này tương ứng với việc gắn thêm`nxt`đến cuối con đường hiện có. 
7. Bất cứ khi nào có thể truy cập được một trạng thái, hãy cập nhật độ dài đường dẫn tối đa bằng cách sử dụng số bit đã đặt trong`mask`. 
8. Sau khi xử lý tất cả các trạng thái, hãy`best`là kích thước tập hợp con lớn nhất có thể tiếp cận. 
9. Đầu ra`n - best`. 

### Tại sao nó hoạt động 

Trạng thái DP lưu trữ chính xác thông tin cần thiết để mở rộng đường dẫn: đỉnh nào đã được sử dụng và đỉnh nào hiện đang ở cuối. 

Việc khởi tạo bao gồm tất cả các đường dẫn có độ dài bằng một. 

Mỗi quá trình chuyển đổi sẽ thêm một đỉnh mới được kết nối với điểm cuối hiện tại, vì vậy mọi đường dẫn được tạo đều hợp lệ. 

Ngược lại, bất kỳ đường đi hợp lệ nào cũng có thể được xây dựng lại bằng cách liên tục loại bỏ đỉnh cuối cùng của nó. Bắt đầu từ đỉnh đầu tiên, DP sẽ tái tạo mọi đường dẫn hợp lệ thông qua các phần mở rộng này. 

Bởi vì mọi đường dẫn hợp lệ đều tương ứng với một số trạng thái DP có thể truy cập và mọi trạng thái DP có thể truy cập đều tương ứng với một đường dẫn hợp lệ, DP liệt kê chính xác tất cả các danh sách phát hợp lệ có thể có. Kích thước tập hợp con lớn nhất có thể tiếp cận chính xác là số lượng bài hát tối đa có thể được lưu giữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())

        genre = []
        writer = []

        for _ in range(n):
            g, w = input().split()
            genre.append(g)
            writer.append(w)

        adj = [[False] * n for _ in range(n)]

        for i in range(n):
            for j in range(i + 1, n):
                if genre[i] == genre[j] or writer[i] == writer[j]:
                    adj[i][j] = adj[j][i] = True

        m = 1 << n
        dp = [[False] * n for _ in range(m)]

        best = 1

        for i in range(n):
            dp[1 << i][i] = True

        for mask in range(m):
            length = mask.bit_count()

            for last in range(n):
                if not dp[mask][last]:
                    continue

                if length > best:
                    best = length

                for nxt in range(n):
                    if mask & (1 << nxt):
                        continue

                    if adj[last][nxt]:
                        dp[mask | (1 << nxt)][nxt] = True

        print(n - best)

solve()
```Việc xây dựng đồ thị tuân theo quy tắc kề trong câu lệnh. Hai bài hát được kết nối chính xác khi chúng có thể xuất hiện cạnh nhau. 

Bảng DP lưu trữ khả năng tiếp cận hơn là độ dài đường dẫn. Khả năng tiếp cận là đủ vì mỗi trạng thái đã xác định duy nhất số lượng đỉnh được sử dụng thông qua mặt nạ bit. 

Việc khởi tạo singleton là cần thiết. Không có nó, các đường dẫn gồm một bài hát sẽ không bao giờ tồn tại, gây ra câu trả lời sai cho các đỉnh bị cô lập. 

Quá trình chuyển đổi luôn nối thêm một đỉnh mới chưa được sử dụng. Vì mặt nạ bit ghi lại tất cả các đỉnh được sử dụng nên không có đỉnh nào có thể xuất hiện hai lần trên cùng một đường dẫn. 

Biến`best`theo dõi tập hợp con có thể tiếp cận lớn nhất gặp ở bất kỳ đâu trong DP. Chúng tôi không yêu cầu đường đi phải kết thúc ở một đỉnh cụ thể, vì vậy mọi điểm cuối có thể tiếp cận đều được xem xét. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
electronic themotans
electronic carlasdreams
pop themotans
pop irinarimes
```Các cạnh đồ thị: 

| Cặp | Đã kết nối? | 
| --- | --- | 
| 1-2 | Cùng thể loại | 
| 1-3 | Cùng một nhà văn | 
| 2-4 | Không | 
| 3-4 | Cùng thể loại | 

Một đường dẫn hợp lệ là:```
4 -> 3 -> 1 -> 2
```Sự phát triển của DP: 

| Mặt nạ | Cuối cùng | Có thể truy cập | 
| --- | --- | --- | 
| 0001 | 1 | Có | 
| 0101 | 3 | Có | 
| 0111 | 2 | Có | 
| 1111 | 4 | Có | 

Tập hợp con có thể truy cập lớn nhất có kích thước 4, vì vậy: 

| Số lượng | Giá trị | 
| --- | --- | 
| n | 4 | 
| tốt nhất | 4 | 
| trả lời | 0 | 

Ví dụ này cho thấy thứ tự có thể được sắp xếp lại hoàn toàn. Chúng tôi không bị hạn chế bởi thứ tự danh sách phát ban đầu. 

### Ví dụ 2 

đầu vào:```
4
a b
c d
e f
g h
```Không có bài hát chia sẻ một thể loại hoặc tác giả. 

Đồ thị: 

| Đỉnh | Hàng xóm | 
| --- | --- | 
| 1 | Không có | 
| 2 | Không có | 
| 3 | Không có | 
| 4 | Không có | 

Các trạng thái có thể truy cập: 

| Mặt nạ | Kích thước | 
| --- | --- | 
| 0001 | 1 | 
| 0010 | 1 | 
| 0100 | 1 | 
| 1000 | 1 | 

Không có mặt nạ lớn hơn nào có thể truy cập được. 

| Số lượng | Giá trị | 
| --- | --- | 
| n | 4 | 
| tốt nhất | 1 | 
| trả lời | 3 | 

Điều này chứng tỏ rằng một bài hát riêng biệt vẫn tạo thành một danh sách phát có độ dài hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n \cdot n^2)$| Mọi trạng thái đều thử tất cả các đỉnh tiếp theo có thể | 
| Không gian |$O(2^n \cdot n)$| Bảng DP trên mặt nạ và điểm cuối | 

Với$n \le 16$, chúng ta có nhiều nhất$2^{16} = 65536$mặt nạ. Số lượng hoạt động kết quả là thoải mái trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ, khoảng một triệu trạng thái boolean. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())

        genre = []
        writer = []

        for _ in range(n):
            g, w = input().split()
            genre.append(g)
            writer.append(w)

        adj = [[False] * n for _ in range(n)]

        for i in range(n):
            for j in range(i + 1, n):
                if genre[i] == genre[j] or writer[i] == writer[j]:
                    adj[i][j] = adj[j][i] = True

        m = 1 << n
        dp = [[False] * n for _ in range(m)]

        best = 1

        for i in range(n):
            dp[1 << i][i] = True

        for mask in range(m):
            for last in range(n):
                if not dp[mask][last]:
                    continue

                best = max(best, mask.bit_count())

                for nxt in range(n):
                    if (mask & (1 << nxt)) == 0 and adj[last][nxt]:
                        dp[mask | (1 << nxt)][nxt] = True

        out.append(str(n - best))

    return "\n".join(out)

# provided samples
assert run(
"""4
1
pop taylorswift
4
electronic themotans
electronic carlasdreams
pop themotans
pop irinarimes
7
rap eminem
rap drdre
rap kanyewest
pop taylorswift
indierock arcticmonkeys
indierock arcticmonkeys
punkrock theoffspring
4
a b
c d
e f
g h
"""
) == """0
0
4
3"""

# minimum size
assert run(
"""1
1
a b
"""
) == "0"

# all songs connected by same genre
assert run(
"""1
4
rock a
rock b
rock c
rock d
"""
) == "0"

# completely isolated songs
assert run(
"""1
3
a x
b y
c z
"""
) == "2"

# chain of length 3 plus isolated vertex
assert run(
"""1
4
a x
a y
b y
c z
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bài hát đơn | 0 | Trường hợp cơ sở, đường dẫn đơn | 
| Tất cả cùng thể loại | 0 | Đường dẫn Hamilton đầy đủ tồn tại | 
| Tất cả bị cô lập | 2 | Đường dẫn lớn nhất có thể có kích thước 1 | 
| Chuỗi cộng đỉnh cô lập | 1 | Tập hợp con tốt nhất không cần bao gồm tất cả các đỉnh | 

## Vỏ cạnh 

Hãy xem xét một bài hát duy nhất:```
1
pop taylor
```DP khởi tạo`dp[1][0] = True`. Không có chuyển đổi nào tồn tại nhưng kích thước tập hợp con có thể truy cập đã là 1. Do đó`best = 1`và câu trả lời là`1 - 1 = 0`. 

Hãy xem xét các bài hát hoàn toàn bị ngắt kết nối:```
3
a x
b y
c z
```Mỗi mặt nạ đơn đều có thể truy cập được. Không thể đạt tới mặt nạ lớn hơn vì không có cạnh đồ thị. Kích thước tập hợp con tối đa có thể truy cập vẫn là 1, đưa ra câu trả lời`3 - 1 = 2`. 

Hãy xem xét một thành phần được kết nối không thể sử dụng được đầy đủ:```
4
a x
a y
b y
c z
```Đồ thị chứa các cạnh`(1,2)`Và`(2,3)`. Đỉnh 4 bị cô lập. DP đạt mặt nạ tương ứng với đường dẫn`1 -> 2 -> 3`, đạt được kích thước 3. Không có trạng thái kích thước 4 nào có thể truy cập được. Thuật toán báo cáo chính xác`4 - 3 = 1`. 

Đây chính xác là loại tình huống mà việc chỉ kiểm tra kết nối sẽ không thành công, trong khi DP theo đường Hamilton vẫn đúng.
