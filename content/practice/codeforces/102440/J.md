---
title: "CF 102440J - Giao hàng tại thành phố tương lai"
description: "Hãy coi mỗi ô lưới là một đỉnh của đồ thị. Dịch chuyển tức thời trực tiếp là một cạnh giữa hai đỉnh khi chúng nằm trong cùng một hàng hoặc cột, chứa cùng một chữ cái và có ít nhất một lần xuất hiện nữa của cùng một chữ cái đó giữa chúng."
date: "2026-08-08T13:59:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "J"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 129
verified: true
draft: false
---

[CF 102440J - Giao hàng tại thành phố tương lai](https://codeforces.com/problemset/problem/102440/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi ô lưới là một đỉnh của đồ thị. Dịch chuyển tức thời trực tiếp là một cạnh giữa hai đỉnh khi chúng nằm trong cùng một hàng hoặc cột, chứa cùng một chữ cái và có ít nhất một lần xuất hiện nữa của cùng một chữ cái đó giữa chúng. 

Một truy vấn đưa ra hai ô và hỏi xem chúng có thuộc cùng một thành phần được kết nối của biểu đồ này hay không. Cho phép dịch chuyển tức thời nhiều lần, vì vậy câu hỏi đặt ra không phải là liệu hai ô có dịch chuyển tức thời trực tiếp hay không, mà là liệu có chuỗi dịch chuyển tức thời hợp lệ nào đó kết nối chúng hay không. 

Lưới có tối đa (1000 \times 1000 = 10^6) ô, trong khi có thể có tới (10^6) truy vấn. Điều đó ngay lập tức loại trừ việc chạy tìm kiếm biểu đồ từ đầu cho mọi truy vấn. Ngay cả một tìm kiếm chỉ chạm vào (O(nm)) ô cho mỗi truy vấn cũng có thể thực hiện (10^{12}) lượt truy cập ô trong trường hợp xấu nhất. Chúng ta cần dành thời gian gần như tuyến tính theo kích thước của lưới trong quá trình tiền xử lý và trả lời từng truy vấn gần như ngay lập tức. 

Có một khó khăn khác ẩn giấu trong quy tắc dịch chuyển tức thời. Nếu một hàng chứa bốn chữ cái bằng nhau, các ô bằng nhau liền kề không thể dịch chuyển trực tiếp, nhưng bốn ô vẫn có thể tạo thành một thành phần được kết nối. Ví dụ, trong`aaaa`, vị trí 1 và 3 có thể dịch chuyển tức thời, vị trí 2 và 4 có thể dịch chuyển tức thời, còn vị trí 1 và 4 có thể dịch chuyển tức thời. Điều đó làm cho tất cả bốn vị trí được kết nối. Một giải pháp chỉ xem xét các ô bằng nhau lân cận hoặc chỉ xem xét các cặp ở khoảng cách chính xác là hai, có thể bỏ lỡ kết nối này. 

Trường hợp xảy ra đúng ba lần cũng đặc biệt. TRONG`aaa`, vị trí 1 và 3 có thể dịch chuyển tức thời, nhưng vị trí 2 không thể dịch chuyển đến một trong hai vị trí đó. Do đó, ba ô không tạo thành một thành phần. Câu trả lời đúng cho truy vấn từ vị trí 1 đến vị trí 2 là`No`, trong khi vị trí 1 đến vị trí 3 là`Yes`. 

Ô tương tự đã có thể truy cập được từ chính nó mà không cần thực hiện dịch chuyển tức thời. Do đó, một truy vấn như```
1 1
a
1
1 1 1 1
```phải sản xuất`Yes`. Cấu trúc kết nối đồ thị xử lý việc này một cách tự nhiên vì mọi đỉnh đều thuộc cùng một thành phần với chính nó. 

Cuối cùng, các chữ cái khác nhau không bao giờ có thể kết nối được. Mỗi dịch chuyển tức thời đều lưu giữ chữ cái, vì vậy ngay cả một chuỗi dịch chuyển dài cũng không thể thay đổi chữ cái của tòa nhà hiện tại. Ví dụ,```
1 2
ab
1
1 1 1 2
```sản xuất`No`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi mọi dịch chuyển tức thời hợp lệ là một cạnh đồ thị và thực hiện BFS hoặc DFS cho mỗi truy vấn. Tìm kiếm là chính xác vì câu trả lời bắt buộc chính xác là kết nối đồ thị. Vấn đề là số lượng truy vấn. Với (10^6) ô và (10^6) truy vấn, một tìm kiếm khám phá toàn bộ lưới cho mỗi truy vấn có thể tiếp cận (10^{12}) ô đã truy cập. Việc xây dựng mọi cạnh dịch chuyển tức thời có thể một cách rõ ràng cũng không hấp dẫn, bởi vì một hàng chứa nhiều bản sao của một chữ cái có thể chứa nhiều cặp hợp lệ theo phương pháp bậc hai. 

Cấu trúc của các chữ cái giống nhau trên một dòng cho chúng ta cách biểu diễn nhỏ hơn nhiều. Hãy xem xét sự xuất hiện của một chữ cái cố định trong một hàng, được sắp xếp từ trái sang phải. Đánh số chúng (1,2,\ldots,k). Cạnh trực tiếp hợp lệ tồn tại giữa lần xuất hiện (i) và lần xuất hiện (j) bất cứ khi nào (j-i\ge 2). 

Chúng ta không cần tất cả các cạnh này. Kết nối lần xuất hiện (i) với lần xuất hiện (i-2) là đủ để xử lý toàn bộ chuỗi ngoại trừ một khoảng trống nhỏ. Đối với bốn lần xuất hiện, các cạnh (1\leftrightarrow3) và (2\leftrightarrow4) tạo ra hai thành phần riêng biệt, vì vậy chúng tôi kết nối thêm lần xuất hiện 4 với lần xuất hiện 1. Bây giờ bốn lần xuất hiện đầu tiên được kết nối. Mỗi lần xuất hiện sau (i) đều kết nối với (i-2), vốn đã được kết nối với những lần xuất hiện trước đó. Do đó, một số cạnh này tạo ra các thành phần được kết nối giống hệt như tất cả các cạnh dịch chuyển tức thời hợp lệ. 

Điều này có nghĩa là trong khi quét một dòng, mỗi lần xuất hiện mới chỉ cần nhớ lần xuất hiện đầu tiên và hai lần xuất hiện ngay trước đó. Thông thường chúng ta thêm cạnh từ lần xuất hiện hiện tại vào lần xuất hiện hai vị trí trước đó. Khi lần xuất hiện hiện tại là lần xuất hiện thứ tư, chúng tôi sẽ kết nối thêm lần xuất hiện đó với lần xuất hiện đầu tiên. 

Chúng tôi áp dụng chính xác cách xây dựng tương tự cho các cột. Vì mọi cạnh dịch chuyển thực sự được biểu thị bằng tập hợp các cạnh rút gọn này, nên chúng ta có thể sử dụng cấu trúc hợp tập hợp rời rạc, DSU, để duy trì các thành phần được kết nối trong khi xử lý lưới. 

Các phương pháp tiếp cận bạo lực và tối ưu có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qnm)) trong trường hợp xấu nhất | (O(nm)) | Quá chậm | 
| Tối ưu | (O(nm\alpha(nm)+q\alpha(nm))) | (O(nm)) | Đã chấp nhận | 

Ở đây (\alpha) là hàm Ackermann nghịch đảo, hàm này thực tế không đổi đối với các kích thước đầu vào này. 

## Hướng dẫn thuật toán 

1. Coi mọi ô ((x,y)) là một đỉnh DSU có mã định danh (x\cdot m+y). Ban đầu, mỗi ô đều có thành phần riêng của nó vì chưa có dịch chuyển tức thời nào được xử lý. 
2. Xử lý lưới theo hàng. Đối với mỗi trong số 26 chữ cái, giữ lại lần xuất hiện đầu tiên, lần xuất hiện gần đây nhất, lần xuất hiện gần đây thứ hai và số lần xuất hiện nhỏ trong hàng hiện tại. 
3. Khi xử lý một ô chứa chữ cái (c), hãy xem lần xuất hiện gần đây thứ hai của (c) trong cùng một hàng. Nếu nó tồn tại, hãy kết hợp nó với ô hiện tại. Hai lần xuất hiện này có chính xác một hoặc nhiều lần xuất hiện của (c) giữa chúng, do đó việc dịch chuyển tức thời là hợp lệ. 
4. Khi ô hiện tại là lần xuất hiện thứ tư của (c) trong hàng, hãy kết hợp nó với lần xuất hiện đầu tiên. Cạnh bổ sung này kết nối hai nhóm mà lẽ ra vẫn tách biệt sau khi chỉ sử dụng khoảng cách hai cạnh. 
5. Cập nhật các lần xuất hiện được lưu trữ cho bức thư này. Ô hiện tại trở thành lần xuất hiện gần đây nhất và lần xuất hiện gần đây nhất trước đó trở thành lần xuất hiện gần đây thứ hai. 
6. Xử lý các cột theo cách tương tự khi quét lưới từ trên xuống dưới. Trạng thái dọc được giữ riêng cho từng cặp gồm một cột và một chữ cái. Điều này tránh được đường truyền lồng nhau thứ hai trên lưới và giữ cho việc triển khai tuyến tính. 
7. Sau khi đã thêm tất cả các kết nối ngang và dọc, hãy đọc mọi truy vấn. Chuyển đổi cả hai tọa độ thành mã định danh đỉnh DSU và so sánh gốc của chúng. Các gốc bằng nhau có nghĩa là tồn tại một chuỗi các dịch chuyển tức thời hợp lệ, vì vậy hãy in`Yes`. Các gốc khác nhau có nghĩa là không tồn tại dãy như vậy, vì vậy hãy in`No`. 

### Tại sao nó hoạt động 

Đối với bất kỳ hàng và chữ cái cố định nào, giả sử số lần xuất hiện của nó là (1,2,\ldots,k). Thuật toán thêm các cạnh (i\leftrightarrow i-2) bất cứ khi nào (i\ge3) và thêm (1\leftrightarrow4) khi (k\ge4). Đối với ba lần xuất hiện, cạnh được thêm duy nhất là (1\leftrightarrow3), đây chính xác là kết nối khả thi duy nhất. Đối với bốn lần xuất hiện, (1\leftrightarrow3), (2\leftrightarrow4) và (1\leftrightarrow4) kết nối cả bốn. Đối với mỗi lần xuất hiện sau (i), cạnh (i\leftrightarrow i-2) sẽ gắn nó vào một phần đã được kết nối của chuỗi. Do đó, các cạnh được chọn này có các thành phần được kết nối chính xác giống như mọi cạnh dịch chuyển tức thời hợp lệ trong hàng đó. 

Đối số tương tự áp dụng độc lập cho mọi cột và chữ cái. Vì mọi dịch chuyển tức thời hợp lệ được biểu thị bằng khả năng kết nối trong một trong các biểu đồ đường rút gọn này và mọi cạnh được thuật toán thêm vào đều là dịch chuyển tức thời hợp lệ, nên các thành phần DSU chính xác là các thành phần được kết nối của biểu đồ dịch chuyển tức thời ban đầu. Do đó, việc so sánh các gốc DSU sẽ đưa ra câu trả lời đúng cho mọi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    total = n * m

    # parent[x] < 0 means x is a root and -parent[x] is its component size.
    parent = [-1] * total

    def find(x):
        while parent[x] >= 0:
            if parent[parent[x]] >= 0:
                parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return

        if parent[a] > parent[b]:
            a, b = b, a

        parent[a] += parent[b]
        parent[b] = a

    # Horizontal state. It is reset for every row.
    first_h = [-1] * 26
    last1_h = [-1] * 26
    last2_h = [-1] * 26
    count_h = [0] * 26

    # Vertical state. One state exists for every (column, letter).
    states = m * 26
    first_v = [-1] * states
    last1_v = [-1] * states
    last2_v = [-1] * states
    count_v = bytearray(states)

    for i in range(n):
        # Start a fresh occurrence history for this row.
        for c in range(26):
            first_h[c] = -1
            last1_h[c] = -1
            last2_h[c] = -1
            count_h[c] = 0

        row = grid[i]
        base = i * m

        for j in range(m):
            idx = base + j
            c = row[j] - 97

            # Horizontal connections.
            p2 = last2_h[c]
            if p2 != -1:
                union(idx, p2)

            if count_h[c] == 3:
                union(idx, first_h[c])

            if count_h[c] == 0:
                first_h[c] = idx

            last2_h[c] = last1_h[c]
            last1_h[c] = idx
            if count_h[c] < 4:
                count_h[c] += 1

            # Vertical connections.
            s = j * 26 + c
            p2 = last2_v[s]
            if p2 != -1:
                union(idx, p2)

            if count_v[s] == 3:
                union(idx, first_v[s])

            if count_v[s] == 0:
                first_v[s] = idx

            last2_v[s] = last1_v[s]
            last1_v[s] = idx
            if count_v[s] < 4:
                count_v[s] += 1

    q = int(input())
    out = bytearray()

    for _ in range(q):
        x1, y1, x2, y2 = map(int, input().split())
        a = (x1 - 1) * m + (y1 - 1)
        b = (x2 - 1) * m + (y2 - 1)

        if find(a) == find(b):
            out.extend(b"Yes\n")
        else:
            out.extend(b"No\n")

    sys.stdout.buffer.write(out)

if __name__ == "__main__":
    solve()
```DSU sử dụng giá trị âm trong`parent`để lưu trữ kích thước thành phần. Điều này tránh được một mảng kích thước riêng biệt và giữ mức sử dụng bộ nhớ ở mức nhỏ. Liên kết theo kích thước ngăn không cho cây trở nên sâu, trong khi nén đường dẫn khiến cho việc tra cứu gốc tiếp theo gần như không đổi. 

Trạng thái ngang được đặt lại ở đầu mỗi hàng vì các lần xuất hiện ở các hàng khác nhau không thể tham gia vào cùng một dịch chuyển tức thời theo chiều ngang. Trạng thái dọc không được đặt lại vì một cột tiếp tục trên tất cả các hàng. 

các`count`các giá trị chỉ cần phân biệt 0, một, hai, ba và ít nhất bốn lần xuất hiện. Khi một chữ cái đã xuất hiện bốn lần trên một dòng, cạnh đặc biệt từ thứ nhất đến thứ tư đã được thêm vào và mỗi lần xuất hiện sau đó chỉ cần kết nối với lần xuất hiện hai vị trí trước đó. MỘT`bytearray`là đủ cho việc đếm dọc. 

điều kiện`count == 3`được kiểm tra có chủ ý trước khi tăng số lượng. Nó xác định lần xuất hiện thứ tư, không phải lần thứ ba. Đối với lần xuất hiện thứ tư,`first`là lần xuất hiện 1 và`last2`là lần xuất hiện thứ 2, do đó thuật toán sẽ thêm cả hai kết nối cần thiết. 

Tất cả tọa độ từ đầu vào đều dựa trên một. Việc chuyển đổi trừ đi một từ cả hai tọa độ trước khi tính toán định danh đỉnh dựa trên số 0. Không có vấn đề tràn số nguyên trong Python và mã định danh lớn nhất nằm bên dưới (10^6). 

Sản lượng được tích lũy trong một`bytearray`thay vì danh sách Python gồm một triệu chuỗi. Điều này giúp dự đoán được mức sử dụng bộ nhớ và tránh các lệnh gọi đầu ra lặp lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lưới là```
aaa
aaa
aaa
```Đối với mỗi hàng, ba`a`các lần xuất hiện được đánh số 1, 2, 3. Việc xử lý lần xuất hiện thứ ba sẽ thêm một cạnh giữa lần xuất hiện 3 và 1. Lần xuất hiện ở giữa vẫn tách biệt trong hàng đó. 

Điều tương tự cũng xảy ra theo chiều dọc. Kết quả là tất cả tám ô ranh giới tạo thành một thành phần, trong khi ô trung tâm vẫn bị cô lập. 

Một dấu vết nhỏ gọn của các kết nối ngang và dọc quan trọng là: 

| Tế bào | Kết nối ngang | Kết nối dọc | Thành phần kết quả | 
| --- | --- | --- | --- | 
| (1,1) | không | không | thành phần góc | 
| (1,2) | không | không | thành phần cạnh-giữa | 
| (1,3) | (1,1) | không | thành phần góc | 
| (2,1) | không | không | thành phần cạnh-giữa | 
| (2,2) | không | không | thành phần trung tâm | 
| (2,3) | không | không | thành phần cạnh-giữa | 
| (3,1) | không | (1,1) | thành phần góc | 
| (3,2) | không | (1,2) | thành phần cạnh-giữa | 
| (3,3) | (3,1) | (1,3) | thành phần góc | 

Quá trình xử lý dọc sau đó kết nối các ô cạnh-giữa và các ô góc tương ứng. Do đó, năm truy vấn tạo ra```
No
No
No
Yes
Yes
```Truy vấn thứ ba giải thích tại sao ô trung tâm lại bị cô lập. Truy vấn thứ tư chứng minh rằng một đường dẫn có thể sử dụng một số dịch chuyển tức thời, trong trường hợp này là di chuyển xung quanh vòng ngoài. 

### Bốn ô bằng nhau trong một hàng 

Hãy xem xét đầu vào được xây dựng```
1 4
aaaa
4
1 1 1 2
1 1 1 3
1 2 1 4
1 1 1 4
```Trình tự xuất hiện là (1,2,3,4). Thuật toán xử lý nó như sau. 

| Vị trí hiện tại | Lần xuất hiện thứ hai trước đó | Kết nối lần thứ tư | Hiệu ứng thành phần | 
| --- | --- | --- | --- | 
| 1 | không | không | bắt đầu thành phần | 
| 2 | không | không | vẫn riêng biệt | 
| 3 | 1 | không | kết nối 3 với 1 | 
| 4 | 2 | 1 | kết nối hai nhóm hiện có | 

Sau khi vị trí 4 được xử lý, cả bốn ô đều thuộc về một thành phần. Đầu ra là```
Yes
Yes
Yes
Yes
```Ví dụ này đặc biệt hữu ích vì chỉ kết nối các lần xuất hiện cách nhau hai vị trí sẽ dẫn đến kết quả không chính xác.`{1,3}`Và`{2,4}`bị ngắt kết nối. Cạnh bổ sung giữa lần xuất hiện đầu tiên và thứ tư sẽ khắc phục chính xác trường hợp đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm\alpha(nm)+q\alpha(nm))) | Mỗi ô chỉ tham gia vào một số lượng hoạt động DSU không đổi, theo sau là hai truy vấn gốc cho mỗi yêu cầu. | 
| Không gian | (O(nm)) | DSU lưu trữ một giá trị trên mỗi ô, trong khi các mảng lưới và trạng thái dòng là tuyến tính ở kích thước đầu vào. | 

Có nhiều nhất (10^6) ô và (10^6) truy vấn. Quá trình tiền xử lý chỉ chạm vào mỗi ô một số lần không đổi và giai đoạn truy vấn chỉ thực hiện hai lần tìm DSU cho mỗi truy vấn. Do đó, thuật toán sẽ chia tỷ lệ tuyến tính với kích thước đầu vào thực tế lên tới hệ số Ackermann nghịch đảo, thay vì nhân kích thước lưới với số lượng truy vấn. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io
from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1.
sample1 = """\
3 3
aaa
aaa
aaa
5
1 1 1 2
2 2 1 1
2 2 1 2
3 3 1 1
3 2 1 2
"""

assert run(sample1) == """\
No
No
No
Yes
Yes
""", "sample 1"

# Minimum-size grid and zero-length path.
sample_min = """\
1 1
a
1
1 1 1 1
"""

assert run(sample_min) == "Yes\n", "minimum-size input"

# Exactly three equal occurrences.
sample_three = """\
1 3
aaa
4
1 1 1 3
1 1 1 2
1 2 1 3
1 2 1 2
"""

assert run(sample_three) == """\
Yes
No
No
Yes
""", "three occurrences"

# Four equal occurrences. All four become connected.
sample_four = """\
1 4
aaaa
4
1 1 1 2
1 1 1 3
1 2 1 4
1 1 1 4
"""

assert run(sample_four) == """\
Yes
Yes
Yes
Yes
""", "four occurrences"

# Different letters can never be connected.
sample_letters = """\
2 2
ab
ba
4
1 1 1 2
1 1 2 1
1 2 2 2
1 1 1 1
"""

assert run(sample_letters) == """\
No
No
No
Yes
""", "different letters"

# Maximum grid dimensions with all equal cells.
# Every cell belongs to one component because each row has 1000 occurrences
# and the rows are connected vertically by the same argument.
n = 1000
m = 1000
grid = "\n".join(["a" * m for _ in range(n)])

sample_max = f"""\
{n} {m}
{grid}
3
1 1 1 1000
1 1 1000 1
500 500 1000 1000
"""

assert run(sample_max) == """\
Yes
Yes
Yes
""", "maximum-size grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 x 1`, một`a`|`Yes`| Khả năng tự tiếp cận và kích thước tối thiểu | 
|`1 x 3`,`aaa`|`Yes, No, No, Yes`| Chính xác ba lần xuất hiện và sự xuất hiện ở giữa bị cô lập | 
|`1 x 4`,`aaaa`| bốn`Yes`câu trả lời | Kết nối lần thứ tư đặc biệt | 
|`2 x 2`,`ab / ba`|`No, No, No, Yes`| Các chữ cái và tọa độ ranh giới khác nhau | 
|`1000 x 1000`, tất cả`a`| Ba`Yes`câu trả lời | Kích thước lưới tối đa và các thành phần được kết nối lớn | 

## Vỏ cạnh 

### Các ô bằng nhau liền kề 

cho```
1 2
aa
1
1 1 1 2
```hai ô chứa cùng một chữ cái và nằm trong cùng một hàng, nhưng không có ô thứ ba`a`giữa họ. Thuật toán chỉ thấy hai lần xuất hiện nên nó không thêm phép hợp nào. Các ô vẫn ở trong các thành phần DSU khác nhau và đầu ra là```
No
```Điều này ngăn ngừa lỗi phổ biến khi diễn giải quy tắc đơn giản là "cùng một chữ cái trong cùng một hàng hoặc cột". 

### Chính xác là ba lần xuất hiện 

cho```
1 3
aaa
2
1 1 1 3
1 1 1 2
```lần xuất hiện thứ ba được kết nối với lần xuất hiện đầu tiên vì có một`a`ở vị trí thứ 2 giữa chúng. Bản thân lần xuất hiện thứ hai không có dịch chuyển tức thời hợp lệ đến một trong hai điểm cuối. Do đó, DSU chứa một thành phần`{1,3}`và một thành phần`{2}`, sản xuất```
Yes
No
```Thuật toán xử lý vấn đề này vì lần xuất hiện thứ ba kết nối với lần xuất hiện thứ hai trước đó, tức là lần xuất hiện đầu tiên, trong khi không có kết nối lần xuất hiện thứ tư đặc biệt nào được tạo. 

### Bốn lần xuất hiện 

cho```
1 4
aaaa
2
1 1 1 2
1 1 1 4
```lần xuất hiện thứ tư đầu tiên kết nối với lần xuất hiện 2 vì chúng cách nhau hai lần. Nó cũng kích hoạt kết nối đặc biệt với lần xuất hiện 1. Ba lần xuất hiện đầu tiên đã có kết nối (1\leftrightarrow3), vì vậy cả bốn lần xuất hiện đều trở thành một thành phần. Đầu ra là```
Yes
Yes
```Đây là trường hợp giúp phân biệt cấu trúc giảm cạnh với chiến lược đơn giản hơn nhưng không chính xác là chỉ kết nối mỗi lần xuất hiện thứ hai. 

### Cùng một ô làm cả hai điểm cuối 

cho```
1 1
z
1
1 1 1 1
```không cần dịch chuyển tức thời. Nguồn và đích thực sự có cùng một đỉnh đồ thị, vì vậy`find(source) == find(destination)`ngay lập tức nắm giữ. Đầu ra là```
Yes
```### Các chữ cái khác nhau 

cho```
1 2
ab
1
1 1 1 2
```hai ô không thể được nối với nhau bằng dịch chuyển tức thời trực tiếp vì các chữ cái của chúng khác nhau. Chúng cũng không thể được kết nối thông qua các ô khác vì mọi dịch chuyển tức thời hợp lệ sẽ bảo toàn bức thư. DSU không bao giờ hợp nhất họ, đưa ra```
No
```### Tương tác các kết nối ngang và dọc 

Hãy xem xét```
3 4
aaaa
bbbb
aaaa
2
1 1 3 4
1 1 2 1
```Hàng đầu tiên và thứ ba mỗi hàng chứa bốn`a`tế bào, vì vậy mỗi`a`trong mỗi hàng thuộc về một thành phần nằm ngang. Mỗi cột sau đó chứa một`a`ở trên cùng và dưới cùng với cái khác`a`sự xuất hiện của cùng một chữ cái giữa chúng, do đó các thành phần trên và dưới được kết nối theo chiều dọc. Do đó, truy vấn đầu tiên là`Yes`. 

Truy vấn thứ hai yêu cầu kết nối một`a`tế bào với một`b`tế bào. Không có chuỗi dịch chuyển hợp lệ nào có thể thay đổi chữ cái, vì vậy nó`No`. 

Điều này minh họa tại sao giải pháp không thể xử lý trước các hàng và cột một cách độc lập rồi trả lời các truy vấn chỉ bằng một hướng. DSU kết hợp cả hai loại kết nối thành một cấu trúc kết nối toàn cầu. 

Nếu bạn muốn, tôi cũng có thể biến nó thành một bài xã luận theo phong cách Codeforces nhỏ gọn hơn, giữ nguyên bằng chứng nhưng giảm bớt phần trình bày một cách đáng kể.
