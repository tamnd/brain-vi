---
title: "CF 468B - Hai Bộ"
description: "Cho ta một tập n số nguyên phân biệt. Mỗi số phải được gán cho một trong hai nhóm. Nếu một số x được xếp vào nhóm A thì phần bù của nó đối với a, cụ thể là a - x, cũng phải có mặt và phải thuộc về A."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "dfs-and-similar", "dsu", "graph-matchings", "greedy"]
categories: ["algorithms"]
codeforces_contest: 468
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 268 (Div. 1)"
rating: 2000
weight: 468
solve_time_s: 152
verified: true
draft: false
---

[CF 468B - Hai bộ](https://codeforces.com/problemset/problem/468/B) 

**Xếp hạng:** 2000 
**Thẻ:** 2-sat, dfs và tương tự, dsu, khớp đồ thị, tham lam 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ`n`số nguyên phân biệt. Mỗi số phải được gán cho một trong hai nhóm. 

Nếu một số`x`được xếp vào nhóm`A`, thì phần bù của nó đối với`a`, cụ thể là`a - x`, cũng phải có mặt và phải thuộc về`A`. 

Tương tự, nếu`x`được xếp vào nhóm`B`, thì phần bù của nó đối với`b`, cụ thể là`b - x`, cũng phải có mặt và phải thuộc về`B`. 

Bản thân các con số đã được cố định. Chúng tôi không được phép thêm các giá trị còn thiếu. Chúng tôi chỉ chọn xem mỗi số hiện có có thuộc nhóm hay không`A`hoặc nhóm`B`. 

Đầu ra là một phép gán cho mỗi số đầu vào. Thông thường,`0`có nghĩa là số đó thuộc về`A`, Và`1`nghĩa là nó thuộc về`B`. 

Ràng buộc`n ≤ 100000`ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân. Thậm chí một`O(n²)`thuật toán sẽ yêu cầu khoảng`10¹⁰`trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(n log n)`. 

Một số trường hợp đặc biệt làm cho vấn đề trở nên phức tạp. 

Coi như:```
n = 1
a = 10
b = 20
p = [3]
```số`3`không có sự bổ sung`7`cũng không bổ sung`17`trong bộ này. Không thể chuyển nhượng được nên câu trả lời là`NO`. 

Bây giờ hãy xem xét:```
n = 2
a = 5
b = 100
p = [2, 3]
```Đây`2`Và`3`tạo thành một cặp hợp lệ cho tổng`5`. Không có số nào có đối tác hợp lệ cho tổng`100`. Cả hai số đều bị buộc phải thiết lập`A`. 

Một giải pháp bất cẩn cố gắng quyết định các con số một cách độc lập có thể thất bại trong các tình huống như:```
a = 10
b = 12
p = [1, 9, 3]
```Con số`1`chỉ có thể tham gia tổng cộng`10`, từ`9`tồn tại. Con số`3`chỉ có thể tham gia tổng cộng`12`, Nhưng`9`sau đó sẽ cần phải ở cả hai bộ cùng một lúc. Câu trả lời là`NO`. 

Khó khăn chính là các quyết định được truyền bá thông qua biểu đồ quan hệ bổ sung. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Với mỗi số, hãy chọn một trong hai bộ`A`hoặc đặt`B`, sau đó xác minh xem tất cả các yêu cầu bổ sung có đúng hay không. 

có`2ⁿ`bài tập. Với`n = 100000`, điều này là hoàn toàn không thể. 

Lý do vũ lực hoạt động về mặt khái niệm là vì các ràng buộc mang tính cục bộ. Một số chỉ quan tâm đến phần bù của nó đối với`a`Và`b`. Vấn đề là các lựa chọn cục bộ tương tác với nhau, tạo ra chuỗi phụ thuộc dài. 

Quan sát quan trọng là mỗi số có thể có nhiều nhất một đối tác tính tổng`a`và tối đa một đối tác`b`. 

Nếu chúng ta xây dựng một đồ thị có các đỉnh là các số cho trước thì: 

Cạnh loại A:`x ↔ a - x`bất cứ khi nào cả hai số tồn tại. 

Cạnh loại B:`x ↔ b - x`bất cứ khi nào cả hai số tồn tại. 

Giả sử một số thiếu đối tác A của nó. Khi đó nó không bao giờ có thể thuộc về tập hợp`A`, vì vậy nó buộc phải thiết lập`B`. 

Tương tự như vậy, nếu một số thiếu đối tác B, nó buộc phải đặt`A`. 

Những lựa chọn bắt buộc này lan truyền. Nếu một đỉnh bị ép vào`A`, thì mọi đỉnh nối qua cạnh A cũng phải nằm trong`A`. Nếu một trong những đỉnh đó đã bị buộc vào`B`, chúng ta có mâu thuẫn. 

Cấu trúc này hoàn toàn phù hợp cho việc xây dựng Disjoint Set Union. 

Giới thiệu hai nút đặc biệt:`SA`có nghĩa là "thuộc về A"`SB`có nghĩa là "thuộc về B" 

Với mỗi số: 

Nếu đối tác A của nó không tồn tại, hãy liên kết nó với`SB`. 

Nếu đối tác B của nó không tồn tại, hãy liên kết nó với`SA`. 

Đối với mọi A-edge hiện có`(x, a-x)`: 

Nối các đỉnh tương ứng. 

Đối với mọi B-edge hiện có`(x, b-x)`: 

Nối các đỉnh tương ứng. 

Giải pháp cổ điển được chấp nhận thực sự mô hình hóa vấn đề dưới dạng biểu đồ có các thành phần được kết nối là đường dẫn hoặc chu trình. Bên trong mỗi thành phần, tất cả các đỉnh phải nằm trong cùng một nhóm. DSU nắm bắt chính xác sự lan truyền này. 

Sau tất cả các công đoàn, nếu`SA`Và`SB`trở nên kết nối, các ràng buộc không nhất quán. 

Mặt khác, mọi thành phần được kết nối với`SA`được chỉ định để thiết lập`A`, mọi thành phần được kết nối với`SB`được chỉ định để thiết lập`B`và các thành phần trống còn lại có thể được gán tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n) | O(n) | Quá chậm | 
| DSU tối ưu | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số và xây dựng bản đồ băm từ giá trị đến chỉ mục. 
2. Tạo DSU với`n + 2`đỉnh. 

Hai đỉnh cuối cùng biểu diễn các trạng thái đặc biệt`SA`Và`SB`. 
3. Với mỗi số`x`, kiểm tra xem`a - x`tồn tại. 

Nếu nó không tồn tại thì`x`không thể thuộc về tập hợp`A`. Liên đỉnh của nó với`SB`. 
4. Với mỗi số`x`, kiểm tra xem`b - x`tồn tại. 

Nếu nó không tồn tại thì`x`không thể thuộc về tập hợp`B`. Liên đỉnh của nó với`SA`. 
5. Đối với mọi cặp đối tác A hiện có, hãy hợp nhất hai đỉnh tương ứng. 

Nếu một đỉnh thuộc`A`, cái còn lại cũng phải thuộc về`A`. 
6. Đối với mọi cặp đối tác B hiện có, hãy hợp nhất hai đỉnh tương ứng. 

Nếu một đỉnh thuộc`B`, cái còn lại cũng phải thuộc về`B`. 
7. Sau tất cả các công đoàn, hãy kiểm tra xem`SA`Và`SB`thuộc cùng một thành phần DSU. 

Nếu đúng như vậy, một số số nào đó sẽ bị ép đồng thời vào cả hai bộ và không có phép gán hợp lệ nào tồn tại. 
8. Ngược lại, hãy xác định cách gán của từng số. 

Nếu thành phần của nó được kết nối với`SA`, đầu ra`0`. 

Nếu không thì xuất ra`1`. 

### Tại sao nó hoạt động 

Đối tác A bị thiếu có nghĩa là số đó không bao giờ có thể được đặt vào bộ`A`, vậy nó phải thuộc về`B`. Tuyên bố đối xứng đúng cho các đối tác B bị thiếu. 

Mọi quan hệ bổ sung đều yêu cầu cả hai điểm cuối nhận được cùng một nhiệm vụ. DSU hợp nhất chính xác các đỉnh phải luôn chuyển động cùng nhau. 

Sau khi tất cả các hàm ý như vậy được xử lý, mỗi thành phần DSU đại diện cho một nhóm số có nhiệm vụ không thể tách rời. Kết nối một thành phần với`SA`sửa nó để thiết lập`A`, trong khi kết nối nó với`SB`sửa nó để thiết lập`B`. 

Nếu như`SA`Và`SB`kết thúc trong cùng một thành phần, một chuỗi hàm ý nào đó buộc một số phải thuộc về cả hai bộ cùng một lúc. Mặt khác, mọi thành phần đều có sự phân công nhất quán, đưa ra một phân vùng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)

        if a == b:
            return

        if self.size[a] < self.size[b]:
            a, b = b, a

        self.parent[b] = a
        self.size[a] += self.size[b]

def solve():
    n, a, b = map(int, input().split())
    p = list(map(int, input().split()))

    pos = {x: i for i, x in enumerate(p)}

    SA = n
    SB = n + 1

    dsu = DSU(n + 2)

    for i, x in enumerate(p):
        ax = a - x
        bx = b - x

        if ax not in pos:
            dsu.union(i, SB)

        if bx not in pos:
            dsu.union(i, SA)

    for i, x in enumerate(p):
        ax = a - x
        bx = b - x

        if ax in pos:
            dsu.union(i, pos[ax])

        if bx in pos:
            dsu.union(i, pos[bx])

    if dsu.find(SA) == dsu.find(SB):
        print("NO")
        return

    rootA = dsu.find(SA)

    ans = []
    for i in range(n):
        if dsu.find(i) == rootA:
            ans.append("0")
        else:
            ans.append("1")

    print("YES")
    print(" ".join(ans))

solve()
```Bản đồ băm`pos`cho phép tra cứu phần bổ sung theo thời gian liên tục. Nếu không có nó, mọi tìm kiếm bổ sung sẽ yêu cầu quét tuyến tính và giải pháp sẽ trở thành bậc hai. 

Hai đỉnh DSU đặc biệt là ý tưởng trung tâm. Họ đại diện cho tư cách thành viên bắt buộc ở một trong hai bộ. Những phần bổ sung bị thiếu sẽ tạo ra những nhiệm vụ bắt buộc đó ngay lập tức. 

Bước thứ hai hợp nhất tất cả các cặp bổ sung. Vì mọi quy tắc bù đều yêu cầu phân công như nhau nên DSU chính xác là cấu trúc phù hợp. 

Lần kiểm tra cuối cùng so sánh các đại diện của`SA`Và`SB`. Sự trùng khớp có nghĩa là các yêu cầu trái ngược nhau đã đạt đến cùng một thành phần. 

Khi xây dựng câu trả lời, mọi thành phần được kết nối với`SA`được chỉ định để thiết lập`A`. Tất cả các thành phần còn lại thuộc về tập hợp`B`. Bởi vì`SA`Và`SB`bị ngắt kết nối, nhiệm vụ này luôn nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5 9
2 3 4 5
```| Số | Đối tác A | Tồn tại | Đối tác B | Tồn tại | 
| --- | --- | --- | --- | --- | 
| 2 | 3 | Có | 7 | Không | 
| 3 | 2 | Có | 6 | Không | 
| 4 | 1 | Không | 5 | Có | 
| 5 | 0 | Không | 4 | Có | 

Nhiệm vụ bắt buộc: 

| Số | Buộc phải | 
| --- | --- | 
| 2 | A | 
| 3 | A | 
| 4 | B | 
| 5 | B | 

Bổ sung đoàn thể kết nối`(2,3)`Và`(4,5)`. 

Nhiệm vụ cuối cùng: 

| Số | Đặt | 
| --- | --- | 
| 2 | A | 
| 3 | A | 
| 4 | B | 
| 5 | B | 

Đầu ra:```
YES
0 0 1 1
```Ví dụ này cho thấy phần bổ sung bị thiếu sẽ ngay lập tức đẩy các thành phần sang một bên. 

### Ví dụ 2 

đầu vào:```
3 10 12
1 9 3
```| Số | Đối tác A | Tồn tại | Đối tác B | Tồn tại | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | Có | 11 | Không | 
| 9 | 1 | Có | 3 | Có | 
| 3 | 7 | Không | 9 | Có | 

Lực lượng: 

| Số | Buộc phải | 
| --- | --- | 
| 1 | A | 
| 3 | B | 

Kết nối cạnh A`1`Và`9`. 

Kết nối cạnh B`9`Và`3`. 

Cả ba số đều trở thành một phần của một thành phần DSU. 

Thành phần đó được kết nối với cả hai`SA`Và`SB`, tạo ra sự mâu thuẫn. 

Đầu ra:```
NO
```Điều này chứng tỏ các ràng buộc cục bộ độc lập có thể xung đột như thế nào thông qua một chuỗi các mối quan hệ bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Tra cứu băm cộng với hoạt động DSU | 
| Không gian | O(n) | Mảng DSU và bản đồ băm |`α(n)`là hàm Ackermann nghịch đảo và có hiệu lực không đổi đối với tất cả các kích thước đầu vào thực tế. Với`n = 100000`, giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def run(inp: str) -> str:
    out = io.StringIO()

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return
            if self.size[a] < self.size[b]:
                a, b = b, a
            self.parent[b] = a
            self.size[a] += self.size[b]

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, a, b = map(int, input().split())
    p = list(map(int, input().split()))

    pos = {x: i for i, x in enumerate(p)}

    SA = n
    SB = n + 1

    dsu = DSU(n + 2)

    for i, x in enumerate(p):
        if a - x not in pos:
            dsu.union(i, SB)
        if b - x not in pos:
            dsu.union(i, SA)

    for i, x in enumerate(p):
        if a - x in pos:
            dsu.union(i, pos[a - x])
        if b - x in pos:
            dsu.union(i, pos[b - x])

    if dsu.find(SA) == dsu.find(SB):
        return "NO"

    rootA = dsu.find(SA)
    ans = []

    for i in range(n):
        ans.append("0" if dsu.find(i) == rootA else "1")

    return "YES\n" + " ".join(ans)

# provided sample
assert run("4 5 9\n2 3 4 5\n") == "YES\n0 0 1 1"

# single value, impossible
assert run("1 10 20\n3\n") == "NO"

# pair forced into A
assert run("2 5 100\n2 3\n") == "YES\n0 0"

# contradiction through chain
assert run("3 10 12\n1 9 3\n") == "NO"

# all numbers belong to B
assert run("2 100 5\n2 3\n") == "YES\n1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 10 20 / 3`|`NO`| Số không có phần bổ sung hợp lệ | 
|`2 5 100 / 2 3`|`YES 0 0`| Toàn bộ thành phần buộc vào A | 
|`3 10 12 / 1 9 3`|`NO`| Tuyên truyền mâu thuẫn | 
|`2 100 5 / 2 3`|`YES 1 1`| Toàn bộ thành phần buộc vào B | 

## Vỏ cạnh 

Hãy xem xét:```
1 10 20
3
```số`3`thiếu cả hai phần bổ sung`7`Và`17`. Thuật toán kết hợp thành phần của nó với cả hai`SA`Và`SB`. Vì những đại diện đó trở nên giống hệt nhau nên câu trả lời là ngay lập tức`NO`. 

Coi như:```
2 5 100
2 3
```Cả hai số đều có đối tác A hợp lệ nhưng không có đối tác B hợp lệ. Thuật toán kết nối cả hai đỉnh với`SA`, hợp nhất chúng qua cạnh A và không bao giờ chạm vào`SB`. Nhiệm vụ cuối cùng được xác định duy nhất như đã đặt`A`. 

Coi như:```
3 10 12
1 9 3
```đỉnh`1`buộc phải`A`, đỉnh`3`buộc phải`B`. Hợp nhất các cạnh bổ sung`1 ↔ 9`Và`9 ↔ 3`, do đó cả ba đỉnh trở thành một thành phần DSU. Thành phần đó chạm vào cả hai nút đặc biệt, tạo ra sự mâu thuẫn. Thuật toán xuất ra chính xác`NO`. 

Coi như:```
4 8 8
1 7 3 5
```Ở đây cả hai tổng đều giống nhau. Mọi quan hệ bổ sung đều là quan hệ giống nhau. DSU chỉ đơn giản là hợp nhất các cặp phù hợp. Không có mâu thuẫn nào xuất hiện và mọi sự phân công nhất quán đều có hiệu quả. Thuật toán xử lý việc này một cách tự nhiên vì các phép hợp là bình thường.
