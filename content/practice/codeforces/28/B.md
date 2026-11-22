---
title: "CF 28B - pSort"
description: "Chúng ta bắt đầu với một mảng trong đó vị trí ban đầu của i chứa giá trị i. Mỗi vị trí cũng có khoảng cách nhảy cố định d[i]. Việc hoán đổi chỉ được phép giữa các vị trí i và j nếu |i - j| = d[i]."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 28
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 28 (Codeforces format)"
rating: 1600
weight: 28
solve_time_s: 93
verified: true
draft: false
---
[CF 28B - pSort](https://codeforces.com/problemset/problem/28/B) 

**Đánh giá:** 1600 
**Tags:** dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng trong đó vị trí`i`ban đầu chứa giá trị`i`. Mỗi vị trí cũng có khoảng cách nhảy cố định`d[i]`. Cho phép trao đổi giữa các vị trí`i`Và`j`chỉ nếu`|i - j| = d[i]`. 

Mảng mục tiêu là một hoán vị. Chúng ta phải quyết định xem các hoán đổi được phép có đủ để chuyển đổi mảng có thứ tự ban đầu thành hoán vị đó hay không. 

Chi tiết đầu tiên quan trọng là quy tắc hoán đổi không đối xứng trong câu lệnh. Chức vụ`i`có thể trao đổi với bất kỳ`j`thỏa mãn điều kiện về khoảng cách. Nhưng một khi một cặp như vậy tồn tại, việc hoán đổi bản thân nó là hoàn toàn bình thường, cả hai giá trị đều đổi chỗ cho nhau. Nếu vị trí`i`có thể đạt được`j`, sau đó vị trí`i`Và`j`thuộc cùng một cơ cấu chuyển động. 

Những hạn chế là rất nhỏ,`n ≤ 100`. Ngay cả các giải pháp hình khối cũng có thể vừa vặn thoải mái trong thời hạn. Điều đó có nghĩa là chúng tôi không cần tối ưu hóa nâng cao nhưng vẫn cần diễn giải biểu đồ chính xác. Việc tìm kiếm trạng thái mạnh mẽ trên tất cả các hoán vị là không thể bởi vì có`n!`tiểu bang. Ngay cả đối với`n = 10`, cái này đã trở nên quá lớn rồi. 

Quan sát quan trọng là các giao dịch hoán đổi chỉ di chuyển các giá trị bên trong các vùng vị thế được kết nối. Nếu vị trí`1`cuối cùng có thể đạt được vị trí`3`Và`5`thông qua một chuỗi hoán đổi, sau đó các giá trị có thể được sắp xếp lại tùy ý giữa các vị trí đó. Nếu hai vị trí thuộc về các thành phần được kết nối khác nhau thì không có chuỗi hoán đổi nào có thể di chuyển một giá trị qua ranh giới đó. 

Một sai lầm phổ biến là chỉ kiểm tra các giao dịch hoán đổi trực tiếp thay vì chuỗi các giao dịch hoán đổi. 

Coi như:```
3
2 3 1
1 1 1
```Câu trả lời đúng là`YES`. 

Chức vụ`1`có thể trao đổi với`2`, Và`2`có thể trao đổi với`3`. Mặc dù`1`không thể trao đổi trực tiếp với`3`, giá trị`1`vẫn có thể di chuyển đến đó thông qua các giao dịch hoán đổi trung gian. 

Một sai lầm dễ mắc phải khác là quên rằng đồ thị là vô hướng trong thực tế. 

Ví dụ:```
2
2 1
1 2
```Câu trả lời là`NO`. 

Chức vụ`1`có thể trao đổi với`2`bởi vì`|1 - 2| = 1`, nhưng vị trí`2`không thể trao đổi với`1`bởi vì`|2 - 1| ≠ 2`. Vì tuyên bố nói rằng`i`- tế bào thứ có thể trao đổi với`j`, chỉ các cạnh được tạo từ hợp lệ`i`vấn đề. Ở đây chỉ tồn tại một hướng, nhưng bản thân hoán đổi vẫn kết nối các vị trí. Chúng tôi coi đây là một lợi thế vô hướng vì sau một lần hoán đổi hợp pháp, các giá trị ở cả hai vị trí sẽ trao đổi. 

Một trường hợp tinh tế khác là các nút bị cô lập.```
4
1 3 2 4
4 1 1 4
```Câu trả lời là`YES`. 

Vị trí`2`Và`3`có thể trao đổi, trong khi vị trí`1`Và`4`đang bị cô lập. Hoán vị đích chỉ hoán đổi các giá trị bên trong cặp được kết nối`{2,3}`, vì vậy nó có thể truy cập được. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các hoán vị có thể tiếp cận bằng cách sử dụng BFS hoặc DFS trên các trạng thái. Mỗi trạng thái là một hoán vị và mỗi lần hoán đổi hợp pháp sẽ tạo ra một trạng thái khác. Điều này đúng vì cuối cùng mọi cấu hình có thể truy cập sẽ được khám phá. 

Vấn đề là số lượng trạng thái. có`n!`hoán vị. Thậm chí tại`n = 10`, tức là đã có hơn 3 triệu tiểu bang và mỗi tiểu bang có thể tạo ra nhiều giao dịch hoán đổi. Giới hạn thực tế là`100`, vì vậy việc tìm kiếm toàn diện là vô vọng. 

Cấu trúc của các giao dịch hoán đổi mang lại cách giải thích đơn giản hơn nhiều. 

Hãy nghĩ về các vị trí như các đỉnh đồ thị. Đối với mọi vị trí`i`, chúng tôi kết nối nó với mọi vị trí hợp lệ`j`thỏa mãn:```
|i - j| = d[i]
```Nếu hai vị trí nằm trong cùng một thành phần được kết nối thì các giá trị có thể di chuyển giữa chúng thông qua các chuỗi hoán đổi. Bên trong một thành phần được kết nối, việc sắp xếp lại tùy ý có thể thực hiện được vì việc hoán đổi dọc theo các đường dẫn cho phép chúng ta hoán đổi các giá trị được chứa một cách tự do. 

Điều đó thay đổi hoàn toàn vấn đề. Thay vì theo dõi các hoán vị, chúng tôi chỉ cần xác minh rằng mọi giá trị đều kết thúc bên trong cùng một thành phần được kết nối với vị trí ban đầu của nó. 

Giá trị`x`bắt đầu ở vị trí`x`. Trong hoán vị đích, nó xuất hiện ở một vị trí nào đó`pos`. Sự sắp xếp có thể đạt được khi và chỉ khi`x`Và`pos`thuộc về cùng một thành phần được kết nối. 

Chúng ta có thể xây dựng biểu đồ và tính toán các thành phần được kết nối bằng DFS hoặc DSU. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị`p`và những khoảng cách yêu thích`d`. 
2. Xây dựng biểu đồ về các vị trí`1 ... n`. 

Đối với mọi vị trí`i`, kiểm tra vị trí:```
i - d[i]
i + d[i]
```Nếu chúng nằm trong giới hạn, hãy kết nối chúng với`i`. 

Các cạnh này đại diện cho sự hoán đổi hợp pháp. 
3. Tính toán các thành phần được kết nối bằng DFS hoặc DSU. 

Các vị trí bên trong cùng một thành phần có thể trao đổi giá trị thông qua các chuỗi hoán đổi. 
4. Đối với mọi vị trí`i`, xét giá trị`p[i]`. 

Giá trị`p[i]`ban đầu bắt đầu ở vị trí`p[i]`. Để đặt nó ở vị trí`i`, cả hai vị trí phải thuộc cùng một thành phần được kết nối. 
5. Nếu bất kỳ vị trí nào không vượt qua được bước kiểm tra này, hãy in`NO`. 
6. Nếu không thì in`YES`. 

### Tại sao nó hoạt động 

Hoán đổi pháp lý không bao giờ di chuyển một giá trị ra ngoài thành phần được kết nối của nó vì mọi cạnh hoán đổi đều nằm bên trong thành phần đó. Vì vậy nếu giá trị`x`phải kết thúc ở vị trí`i`, sau đó`x`Và`i`phải thuộc cùng một thành phần. 

Hướng ngược lại cũng được giữ. Bên trong một thành phần được kết nối, các thao tác hoán đổi lặp đi lặp lại dọc theo các cạnh của biểu đồ cho phép các giá trị di chuyển đến bất kỳ đâu trong thành phần đó. Vì các thành phần được kết nối là độc lập nên chúng ta có thể sắp xếp lại từng thành phần riêng biệt theo thứ tự mong muốn. 

Điều đó làm cho điều kiện thành phần được kết nối vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)

        if a != b:
            self.parent[b] = a

def solve():
    n = int(input())

    p = list(map(int, input().split()))
    d = list(map(int, input().split()))

    dsu = DSU(n)

    for i in range(n):
        left = i - d[i]
        right = i + d[i]

        if left >= 0:
            dsu.union(i, left)

        if right < n:
            dsu.union(i, right)

    for i in range(n):
        target_value = p[i] - 1

        if dsu.find(i) != dsu.find(target_value):
            print("NO")
            return

    print("YES")

solve()
```DSU lưu trữ các thành phần được kết nối của các vị trí. Mọi hoán đổi hợp lệ sẽ tạo ra một cạnh giữa hai chỉ số, do đó các phép toán hợp sẽ xây dựng biểu đồ chuyển động tăng dần. 

Việc triển khai sử dụng lập chỉ mục dựa trên số 0 trong nội bộ. Đó là lý do tại sao giá trị ở vị trí`i`trở thành`p[i] - 1`trước khi so sánh các thành phần. Quên chuyển đổi này là một trong những lỗi thường gặp nhất trong vấn đề này. 

Đối với mỗi chỉ mục, chúng tôi thử cả hai đích đến có thể,`i - d[i]`Và`i + d[i]`. Việc kiểm tra giới hạn rất quan trọng vì một số bước nhảy rời khỏi mảng. 

Bước xác minh cuối cùng mã hóa trực tiếp bất biến cốt lõi. Giá trị`p[i]`bắt đầu ở vị trí`p[i] - 1`. Nếu vị trí ban đầu và đích đến hiện tại của nó thuộc về các thành phần khác nhau thì không có chuỗi hoán đổi pháp lý nào có thể đặt nó ở đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 4 3 2 1
1 1 1 1 1
```Xây dựng đồ thị: 

| Vị trí | d[i] | Vị trí có thể tiếp cận | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 1 | 1, 3 | 
| 3 | 1 | 2, 4 | 
| 4 | 1 | 3, 5 | 
| 5 | 1 | 4 | 

Tất cả các vị trí trở nên kết nối. 

Bảng thành phần: 

| Vị trí | Thành phần | 
| --- | --- | 
| 1 | A | 
| 2 | A | 
| 3 | A | 
| 4 | A | 
| 5 | A | 

Xác minh: 

| Vị trí mục tiêu | Giá trị cần thiết | Vị trí ban đầu của giá trị | Cùng một thành phần? | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 | Có | 
| 2 | 4 | 4 | Có | 
| 3 | 3 | 3 | Có | 
| 4 | 2 | 2 | Có | 
| 5 | 1 | 1 | Có | 

Đầu ra:```
YES
```Dấu vết này cho thấy rằng khi biểu đồ được kết nối hoàn toàn, mọi hoán vị đều có thể truy cập được. 

### Ví dụ 2 

đầu vào:```
4
2 1 4 3
2 2 1 1
```Xây dựng đồ thị: 

| Vị trí | d[i] | Vị trí có thể tiếp cận | 
| --- | --- | --- | 
| 1 | 2 | 3 | 
| 2 | 2 | 4 | 
| 3 | 1 | 2, 4 | 
| 4 | 1 | 3 | 

Các thành phần được kết nối: 

| Vị trí | Thành phần | 
| --- | --- | 
| 1 | A | 
| 2 | B | 
| 3 | B | 
| 4 | B | 

Xác minh: 

| Vị trí mục tiêu | Giá trị cần thiết | Vị trí ban đầu của giá trị | Cùng một thành phần? | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | Không | 

Đầu ra:```
NO
```Giá trị`2`bắt đầu trong thành phần`B`, nhưng vị trí`1`thuộc về thành phần`A`. Không có trình tự pháp lý nào có thể vượt qua ranh giới đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Hoạt động DSU gần như không đổi và chúng tôi xử lý tối đa hai cạnh trên mỗi nút | 
| Không gian | O(n) | Mảng cha DSU lưu trữ một mục nhập cho mỗi vị trí | 

Với`n ≤ 100`, giải pháp này thấp hơn nhiều so với giới hạn. Ngay cả việc truyền tải đồ thị dày đặc cũng có thể vượt qua một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))

        def find(self, x):
            if self.parent[x] != x:
                self.parent[x] = self.find(self.parent[x])
            return self.parent[x]

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)

            if a != b:
                self.parent[b] = a

    n = int(input())

    p = list(map(int, input().split()))
    d = list(map(int, input().split()))

    dsu = DSU(n)

    for i in range(n):
        left = i - d[i]
        right = i + d[i]

        if left >= 0:
            dsu.union(i, left)

        if right < n:
            dsu.union(i, right)

    for i in range(n):
        if dsu.find(i) != dsu.find(p[i] - 1):
            return "NO"

    return "YES"

# provided sample
assert run(
"""5
5 4 3 2 1
1 1 1 1 1
"""
) == "YES", "sample 1"

# minimum size
assert run(
"""1
1
1
"""
) == "YES", "single element"

# isolated positions
assert run(
"""4
2 1 3 4
4 4 4 4
"""
) == "NO", "no swaps possible"

# connected through chains
assert run(
"""3
2 3 1
1 1 1
"""
) == "YES", "indirect reachability"

# partial components
assert run(
"""4
1 3 2 4
4 1 1 4
"""
) == "YES", "swap only inside one component"

# off-by-one boundary test
assert run(
"""2
2 1
2 2
"""
) == "NO", "out-of-range jumps create no edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | CÓ | Xử lý thành phần tầm thường | 
| Không có giao dịch hoán đổi hợp lệ ở bất cứ đâu | KHÔNG | Các nút biệt lập vẫn cố định | 
| Đồ thị nối chuỗi | CÓ | Chuyển động gián tiếp qua các con đường | 
| Một thành phần nhỏ được kết nối | CÓ | Sắp xếp lại thành phần độc lập | 
| Nhảy ngoài phạm vi | KHÔNG | Kiểm tra ranh giới và lập chỉ mục chính xác | 

## Vỏ cạnh 

Xét trường hợp kết nối gián tiếp:```
3
2 3 1
1 1 1
```Các cạnh của đồ thị là:```
1 <-> 2
2 <-> 3
```Tất cả các vị trí thuộc về một thành phần được kết nối. Mặc dù vị trí`1`không thể trao đổi trực tiếp với`3`, giá trị`1`có thể di chuyển bằng cách sử dụng:```
[1 2 3]
-> swap 1 and 2
[2 1 3]
-> swap 2 and 3
[2 3 1]
```DSU hợp nhất chính xác cả ba vị trí, do đó thuật toán đưa ra`YES`. 

Bây giờ hãy xem xét các nút bị cô lập:```
4
2 1 3 4
4 4 4 4
```Mọi bước nhảy đều đi ra ngoài mảng nên không có cạnh nào được tạo ra. Mỗi vị trí tạo thành thành phần riêng của nó. 

Hoán vị mục tiêu cố gắng đặt giá trị`2`ở vị trí`1`. Thành phần của chúng khác nhau nên thuật toán trả về ngay`NO`. 

Cuối cùng, hãy xem xét một biểu đồ được kết nối một phần:```
4
1 3 2 4
4 1 1 4
```Chỉ có vị trí`2`Và`3`được kết nối. Hoán vị mục tiêu hoán đổi chính xác hai giá trị đó. 

Cấu trúc thành phần:```
{1}, {2,3}, {4}
```Mỗi giá trị vẫn nằm trong thành phần ban đầu của nó, do đó thuật toán sẽ xuất ra một cách chính xác`YES`.
