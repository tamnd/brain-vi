---
title: "CF 27D - Đường Vành Đai 2"
description: "Chúng ta có một chu trình gồm n thành phố được sắp xếp trên một vòng. Mỗi cặp thành phố liên tiếp đã được kết nối bằng đường vành đai ngoài và thành phố n cũng được kết nối trở lại thành phố 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 27
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 27 (Codeforces format, Div. 2)"
rating: 2200
weight: 27
solve_time_s: 90
verified: true
draft: false
---
[CF 27D - Đường vành đai 2](https://codeforces.com/problemset/problem/27/D) 

**Đánh giá:** 2200 
**Thẻ:** 2-sat, dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chu kỳ`n`các thành phố được sắp xếp trên một vòng Mỗi cặp thành phố liên tiếp đã được kết nối bằng đường vành đai ngoài và thành phố`n`cũng được kết nối trở lại thành phố`1`. 

Chúng tôi được tặng thêm`m`những con đường mới. Mỗi con đường mới nối hai thành phố bằng một đường cong phải nằm hoàn toàn bên trong hoặc bên ngoài vành đai. Hai con đường mới chỉ được phép chạm vào nhau tại các điểm cuối chung. Bất kỳ giao lộ nào khác đều bị cấm. 

Nhiệm vụ là quyết định, đối với mỗi con đường mới, nên đi vào trong hay ra ngoài để không có cặp đường nào cắt nhau. Nếu phép gán như vậy tồn tại, chúng tôi sẽ in một chuỗi có độ dài`m`mỗi nhân vật ở đâu`i`hoặc`o`. Nếu không chúng tôi in`Impossible`. 

Hình học trở nên đơn giản hơn nhiều khi chúng ta nhận thấy rằng chiếc vòng cố định trật tự hình tròn của các thành phố. Con đường chỉ đơn giản là một dây của một vòng tròn. Hai dây giao nhau bên trong đường tròn chính xác khi điểm cuối của chúng luân phiên nhau quanh vòng tròn. 

Các giới hạn đủ nhỏ để chúng ta có thể so sánh rõ ràng từng cặp đường. Từ`m ≤ 100`, có nhiều nhất`4950`cặp. Điều đó ngay lập tức gợi ý một`O(m²)`xây dựng đồ thị là an toàn. Cấu trúc dữ liệu hình học phức tạp hơn là không cần thiết. 

Phần nguy hiểm không phải là hiệu suất, mà là mô hình hóa chính xác tình trạng băng qua. Việc triển khai bất cẩn thường khiến thứ tự tuần hoàn bị sai, đặc biệt khi các điểm cuối quấn quanh phần cuối của việc đánh số. 

Một trường hợp tinh tế là khi hai con đường có chung một điểm cuối. Những con đường như vậy không bao giờ tạo ra một giao lộ cấm. 

Ví dụ:```
5 2
1 3
1 4
```Những con đường này chỉ chạm vào thành phố`1`, do đó cả hai có thể được kéo vào bên trong. Việc kiểm tra điểm cuối xen kẽ ngây thơ bỏ qua các đỉnh được chia sẻ có thể đánh dấu chúng là giao nhau một cách không chính xác. 

Một lỗi dễ dàng khác xuất hiện khi điểm cuối không được chuẩn hóa. 

Ví dụ:```
6 2
5 2
1 4
```Nếu chúng ta rời khỏi con đường đầu tiên như`(5,2)`thay vì chuyển đổi nó thành`(2,5)`, việc kiểm tra định kỳ trở nên không nhất quán và có thể bỏ sót việc giao nhau. 

Một vấn đề hình học tinh tế hơn đến từ cấu trúc hình tròn. 

Ví dụ:```
6 2
1 4
2 5
```Đi quanh vòng tròn ra lệnh`1 2 4 5`. Các điểm cuối xen kẽ nhau nên các dây sẽ giao nhau nếu được đặt ở cùng một phía. Giải pháp đúng đắn nào cũng phải ép một đường vào trong và một đường kia ở ngoài. 

Cuối cùng, có những cấu hình trong đó các ràng buộc mâu thuẫn với nhau. 

Ví dụ:```
6 3
1 4
2 5
3 6
```Mọi cặp đều bắt chéo nhau nên mỗi cặp phải nằm đối diện nhau. Điều này tạo thành một chu kỳ ràng buộc kỳ lạ, điều này là không thể nếu chỉ có hai bên. Đầu ra đúng là`Impossible`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Mỗi con đường đều chọn bên trong hoặc bên ngoài một cách độc lập nên có`2^m`các nhiệm vụ có thể. Đối với mỗi bài tập, chúng ta có thể kiểm tra từng cặp đường và xác minh rằng các cặp đường giao nhau được đặt ở hai phía đối diện nhau. 

Điều này hoạt động vì điều kiện hoàn toàn là theo cặp. Hai con đường chỉ quan trọng nếu chúng giao nhau về mặt hình học. Thật không may, ngay cả`m = 100`làm cho`2^100`lớn về mặt thiên văn. Không có chiến lược cắt tỉa nào cứu được phương pháp này trong trường hợp xấu nhất. 

Quan sát quan trọng là mọi cặp giao nhau đều áp đặt một ràng buộc nhị phân: 

Nếu hai con đường giao nhau thì đúng một trong hai con đường đó phải nằm trong đó. 

Đó là điều kiện 2 màu cổ điển. Chúng ta có thể coi mỗi con đường là một biến boolean:`True`có nghĩa là bên trong,`False`nghĩa là ở bên ngoài. 

Bất cứ khi nào hai con đường giao nhau, giá trị của chúng phải khác nhau. Điều này chuyển bài toán thành việc kiểm tra xem đồ thị có phải là đồ thị lưỡng cực hay không. Mỗi con đường trở thành một đỉnh. Chúng ta nối hai đỉnh nếu các đường tương ứng giao nhau. Sau đó: 

Nếu đồ thị là lưỡng cực, một phân vùng có thể ở bên trong và phân vùng kia có thể ở bên ngoài. 

Nếu đồ thị không phải là đồ thị lưỡng cực thì một số chu kỳ lẻ sẽ tạo ra những yêu cầu trái ngược nhau. 

Vấn đề này cũng thường được mô tả là một trường hợp 2-SAT vì mỗi cặp chéo tạo ra:`(A or B) and (!A or !B)`thực thi`A != B`. 

Đối với vấn đề cụ thể này, việc kiểm tra hai bên đơn giản hơn và hoàn toàn đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m · m2) | O(m) | Quá chậm | 
| Tối ưu | O(m2) | O(m2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các con đường và chuẩn hóa từng con đường sao cho`a < b`. 

Điều này cho phép chúng ta coi mỗi con đường là một khoảng tiêu chuẩn theo thứ tự vòng tròn. 
2. Với mỗi cặp đường, hãy xác định xem chúng có giao nhau hay không. 

Hai con đường`(a,b)`Và`(c,d)`giao nhau nếu điểm cuối của chúng thay thế:`a < c < b < d`hoặc`c < a < d < b`Điểm cuối được chia sẻ bị bỏ qua vì được phép chạm vào điểm cuối. 
3. Xây dựng đồ thị trong đó mỗi đường là một đỉnh. 

Nếu hai đường giao nhau, hãy thêm một cạnh vô hướng giữa các đỉnh của chúng. 
4. Cố gắng tô màu 2 đồ thị bằng DFS hoặc BFS. 

Gán màu`0`sang một bên và tô màu`1`đến cái khác. 
5. Bất cứ khi nào chúng ta đi qua một cạnh, buộc đỉnh lân cận nhận được màu đối lập. 

Điều này phù hợp với yêu cầu các đường giao nhau phải nằm ở hai phía đối diện của vành đai. 
6. Nếu chúng ta gặp một cạnh có điểm cuối cùng màu thì đồ thị đó không phải là đồ thị lưỡng cực. 

Trong trường hợp đó in`Impossible`. 
7. Nếu không, hãy chuyển màu thành ký tự. 

Ví dụ:`0 -> i`

`1 -> o`In chuỗi kết quả. 

### Tại sao nó hoạt động 

Biểu đồ nắm bắt chính xác các tình huống bị cấm. Mỗi cạnh đại diện cho một cặp đường không thể vẽ trên cùng một phía vì chúng giao nhau về mặt hình học. 

Một bản vẽ hợp lệ tương đương với việc gán một trong hai cạnh cho mỗi con đường sao cho các đỉnh liền kề nhận được các phép gán khác nhau. Đó chính xác là định nghĩa của cách tô màu đồ thị lưỡng cực. 

Nếu đồ thị là lưỡng cực, việc tô màu trực tiếp sẽ cho một kết cấu hợp lệ. Nếu nó không phải là lưỡng cực, một số chu kỳ lẻ đòi hỏi các màu xen kẽ xung quanh chu kỳ và cuối cùng tạo ra sự mâu thuẫn. Không có vị trí hợp lệ tồn tại trong trường hợp đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def intersect(a, b, c, d):
    if len({a, b, c, d}) < 4:
        return False

    return (a < c < b < d) or (c < a < d < b)

def solve():
    n, m = map(int, input().split())

    roads = []

    for _ in range(m):
        a, b = map(int, input().split())

        if a > b:
            a, b = b, a

        roads.append((a, b))

    graph = [[] for _ in range(m)]

    for i in range(m):
        a, b = roads[i]

        for j in range(i + 1, m):
            c, d = roads[j]

            if intersect(a, b, c, d):
                graph[i].append(j)
                graph[j].append(i)

    color = [-1] * m

    def dfs(v, c):
        color[v] = c

        for to in graph[v]:
            if color[to] == -1:
                if not dfs(to, c ^ 1):
                    return False
            elif color[to] == c:
                return False

        return True

    for i in range(m):
        if color[i] == -1:
            if not dfs(i, 0):
                print("Impossible")
                return

    ans = ''.join('i' if c == 0 else 'o' for c in color)
    print(ans)

solve()
```Chi tiết quan trọng đầu tiên là chuẩn hóa điểm cuối. Mỗi con đường được lưu trữ với`a < b`. Nếu không có bước này, việc kiểm tra điểm cuối xen kẽ sẽ trở nên không nhất quán. 

các`intersect`Hàm thực hiện trực tiếp điều kiện hình học. Hai dây cắt nhau khi một đầu của dây thứ hai nằm hoàn toàn trong quãng của dây thứ nhất và đầu kia nằm ở ngoài. Các điểm cuối dùng chung bị loại trừ trước khi kiểm tra vì chúng là các điểm giao nhau hợp pháp. 

Việc xây dựng biểu đồ so sánh mỗi cặp đường một lần. Từ`m ≤ 100`, chi phí này nhiều nhất là khoảng năm nghìn séc. 

DFS thực hiện tô màu lưỡng cực thông thường. biểu hiện`c ^ 1`lật giữa`0`Và`1`. Nếu chúng ta xem lại một đỉnh có cùng màu với đỉnh lân cận của nó, chúng ta sẽ phát hiện ra một chu trình lẻ và ngay lập tức kết thúc. 

Cuối cùng, màu sắc được dịch sang`i`Và`o`. Mọi bản đồ nhất quán đều hoạt động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 3
2 4
```Các con đường giao nhau vì thứ tự là`1 2 3 4`. 

| Cặp | Giao nhau? | Cạnh đồ thị | 
| --- | --- | --- | 
| (1,3) và (2,4) | Có | 0 ↔ 1 | 

Màu DFS: 

| Đỉnh | Màu sắc | Ký tự đầu ra | 
| --- | --- | --- | 
| 0 | 0 | tôi | 
| 1 | 1 | o | 

Đầu ra có thể:```
io
```Ví dụ này thể hiện ràng buộc cốt lõi. Các đường giao nhau phải sơn màu đối lập nhau. 

### Ví dụ 2 

đầu vào:```
6 3
1 4
2 5
3 6
```Giao lộ theo cặp: 

| Cặp | Giao nhau? | 
| --- | --- | 
| (1,4) và (2,5) | Có | 
| (1,4) và (3,6) | Có | 
| (2,5) và (3,6) | Có | 

Đồ thị trở thành một hình tam giác. 

Nỗ lực tô màu DFS: 

| Đỉnh | Màu được chỉ định | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 1 | 

Bây giờ đỉnh`1`Và`2`liền kề nhau nhưng có cùng màu sắc nên đồ thị không có hai phần. 

Đầu ra:```
Impossible
```Dấu vết này cho thấy các chu kỳ lẻ tương ứng với các yêu cầu hình học trái ngược nhau như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m2) | Mỗi cặp đường đều được kiểm tra một lần | 
| Không gian | O(m2) | Đồ thị giao nhau có thể chứa các cạnh O(m²) | 

Với`m ≤ 100`, ngay cả trường hợp đồ thị dày đặc cũng rất nhỏ. Khoảng năm nghìn cặp kiểm tra và nhiều nhất là mười nghìn mục lân cận dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def intersect(a, b, c, d):
        if len({a, b, c, d}) < 4:
            return False

        return (a < c < b < d) or (c < a < d < b)

    n, m = map(int, input().split())

    roads = []

    for _ in range(m):
        a, b = map(int, input().split())

        if a > b:
            a, b = b, a

        roads.append((a, b))

    graph = [[] for _ in range(m)]

    for i in range(m):
        a, b = roads[i]

        for j in range(i + 1, m):
            c, d = roads[j]

            if intersect(a, b, c, d):
                graph[i].append(j)
                graph[j].append(i)

    color = [-1] * m

    def dfs(v, c):
        color[v] = c

        for to in graph[v]:
            if color[to] == -1:
                if not dfs(to, c ^ 1):
                    return False
            elif color[to] == c:
                return False

        return True

    ok = True

    for i in range(m):
        if color[i] == -1:
            if not dfs(i, 0):
                ok = False
                break

    if not ok:
        return "Impossible"

    return ''.join('i' if c == 0 else 'o' for c in color)

# provided sample
assert run(
"""4 2
1 3
2 4
"""
) in ["io", "oi"], "sample 1"

# minimum valid input
assert run(
"""4 1
1 3
"""
) == "i", "single road"

# impossible odd cycle
assert run(
"""6 3
1 4
2 5
3 6
"""
) == "Impossible", "odd cycle"

# shared endpoint should not intersect
assert run(
"""5 2
1 3
1 4
"""
) == "ii", "shared endpoint"

# non-crossing nested chords
assert run(
"""6 2
1 6
2 5
"""
) == "ii", "nested non-crossing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đường đơn |`i`| Đồ thị tầm thường có một thành phần | 
| Ba cặp giao nhau |`Impossible`| Mâu thuẫn chu kỳ lẻ | 
| Trường hợp điểm cuối được chia sẻ |`ii`| Cho phép chạm vào điểm cuối | 
| Hợp âm lồng nhau |`ii`| Các khoảng lồng nhau không giao nhau | 

## Vỏ cạnh 

Hãy xem xét những con đường có chung một điểm cuối:```
5 2
1 3
1 4
```Sau khi chuẩn hóa chúng ta có`(1,3)`Và`(1,4)`. Kiểm tra giao lộ trước tiên thông báo rằng bốn điểm cuối không khác biệt nên ngay lập tức trả về`False`. Không có cạnh nào được thêm vào biểu đồ. Cả hai con đường có thể nhận được cùng một màu, tạo ra`ii`. 

Bây giờ hãy xem xét các điểm cuối đảo ngược:```
6 2
5 2
1 4
```Chuyển đổi chuẩn hóa`(5,2)`vào trong`(2,5)`. Thứ tự xung quanh vòng tròn trở thành`1 2 4 5`, luân phiên nên các con đường giao nhau. Một cạnh được thêm vào và đồ thị có các màu đối lập nhau. Nếu không chuẩn hóa, logic khoảng sẽ thất bại. 

Cuối cùng, hãy xem xét tam giác bất khả thi:```
6 3
1 4
2 5
3 6
```Mỗi cặp xen kẽ nhau nên đồ thị là 3 chu trình. DFS chỉ định màu sắc:```
0 -> 0
1 -> 1
2 -> 0
```Nhưng đỉnh`0`Và`2`liền kề nhau, tạo ra xung đột. Thuật toán in chính xác`Impossible`.
