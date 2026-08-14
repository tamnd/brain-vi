---
title: "CF 102307G - Tốt nghiệp"
description: "Mỗi khóa học là một đỉnh trong đồ thị có hướng. Nếu a[i] = j thì khóa học i phải hoàn thành trước khóa học j. Một đường đi với a[i] = 0 không có đường đi nào sau nó trong quan hệ tiên quyết này."
date: "2026-08-13T07:19:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "G"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 111
verified: true
draft: false
---

[CF 102307G - Tốt nghiệp](https://codeforces.com/problemset/problem/102307/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi khóa học là một đỉnh trong đồ thị có hướng. Nếu như`a[i] = j`, thì tất nhiên`i`phải được hoàn thành trước khóa học`j`. Một khóa học với`a[i] = 0`không có tiến trình nào sau nó trong mối quan hệ tiên quyết này. 

Hạn chế cơ bản về cấu trúc là mỗi khóa học có thể là điều kiện tiên quyết cho nhiều nhất một khóa học khác. Trong thuật ngữ đồ thị, mỗi đỉnh có nhiều nhất là một bậc. Vì việc tốt nghiệp được đảm bảo là có thể thực hiện được nên biểu đồ liên quan không chứa chu trình có hướng. Do đó, đồ thị là một tập hợp các cây có các cạnh hướng về đường đi cuối cùng của chúng. 

Một học kỳ có thể chứa nhiều nhất`k`các khóa học và mỗi khóa học được bố trí trong một học kỳ phải hoàn thành tất cả các điều kiện tiên quyết trong các học kỳ trước đó. Nhiệm vụ là giảm thiểu số lượng học kỳ cần thiết để hoàn thành tất cả`n`các khóa học. 

Giá trị của`n`có thể đạt được`10^4`, trong khi`k`nhiều nhất là`10`. Điều này ngay lập tức loại trừ việc lập trình động tập hợp con trên tất cả các khóa học, bởi vì`2^10000`trạng thái vượt xa bất cứ thứ gì có thể chạy trong một giây. Giá trị nhỏ của`k`rất hữu ích cho việc mô tả các lựa chọn học kỳ có thể có, nhưng nó không mang lại tính thực tiễn cao vì vẫn có thể có một số lượng lớn các khóa học có sẵn. 

Có một số trường hợp đặc biệt có thể khiến việc sắp xếp tôpô đơn giản đưa ra câu trả lời sai. Đầu tiên, việc chỉ tham gia các khóa học có sẵn tùy ý là không tối ưu. Coi như```
5 2
2 3 0 0 0
```Khóa học`1 -> 2 -> 3`tạo thành một chuỗi, trong khi các khóa học`4`Và`5`là độc lập. Câu trả lời là`3`: tham gia các khóa học`1,4`, sau đó`2,5`, sau đó`3`. Một thuật toán bất cẩn có thể mất`4,5`đầu tiên và sau đó sẽ cần bốn học kỳ. 

Thứ hai, có đủ năng lực để tham gia mọi khóa học hiện có không có nghĩa là các khóa học mới mở khóa có thể được tham gia ngay lập tức. Vì```
4 2
3 3 4 0
```các khóa học`1`Và`2`khóa học mở khóa`3`, sau đó mở khóa khóa học`4`. Câu trả lời là`3`, không`2`. Các khóa học hoàn thành trong một học kỳ không thể đáp ứng điều kiện tiên quyết cho đến học kỳ tiếp theo. 

Cuối cùng, một khóa học không có người kế nhiệm không nhất thiết phải có sẵn ngay từ đầu. Vì```
3 3
0 1 2
```các cạnh là`3 -> 2 -> 1`. Mặc dù tất nhiên`1`là khóa học cuối cùng, nó không thể được thực hiện cho đến khi các khóa học`3`Và`2`đã được hoàn thành. Câu trả lời là`3`, mặc dù`k = 3`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force tự nhiên sẽ đại diện cho tập hợp các khóa học đã hoàn thành và thử đệ quy mọi lựa chọn hợp pháp lên đến`k`các khóa học hiện có sẵn cho học kỳ tiếp theo. Điều này đúng vì mọi lịch trình hợp lệ có thể xuất hiện ở đâu đó trong cây tìm kiếm, do đó việc lấy mức tối thiểu trên tất cả các lịch trình sẽ mang lại kết quả tối ưu. 

Vấn đề là số lượng trạng thái. Một tập hợp con DP đã có`2^n`tập hợp có thể của các khóa học đã hoàn thành. Vì`n = 10000`, điều đó có nghĩa là`2^10000`tiểu bang. Nếu chúng ta cũng liệt kê các nhóm khóa học có thể tham gia trong một học kỳ thì chỉ riêng bang đầu tiên có thể có 

[ 
\sum_{j=1}^{10} {10000 \choose j} 
] 

các lựa chọn có thể có, với số hạng lớn nhất`C(10000,10)`khoảng`2.76 * 10^33`. Cách tiếp cận bạo lực không chỉ đơn thuần là hơi chậm một chút, mà về cơ bản, không gian trạng thái của nó là theo cấp số nhân. 

Việc hạn chế biểu đồ cho chúng ta một cách mạnh mẽ hơn để suy luận về lịch trình. Mỗi khóa học có nhiều nhất một khóa học kế thừa, vì vậy từ bất kỳ khóa học nào cũng có một con đường duy nhất hướng tới khóa học cuối cùng. Xác định cấp độ của một khóa học là độ dài của con đường dài nhất bắt đầu từ khóa học đó và tuân theo các điều kiện tiên quyết để đến khóa học cuối cùng. Một khóa học cuối cùng có trình độ`0`, điều kiện tiên quyết trước mắt của nó có mức độ`1`, vân vân. 

Giả sử hiện có hai khóa học. Nếu một người có cấp độ cao hơn, việc hoàn thành nó sẽ cấp bách hơn vì còn phải tuân theo một chuỗi khóa học dài hơn. Dành một học kỳ cho một cấp độ`0`khóa học độc lập trong khi rời khỏi một cấp độ`5`lộ trình không bị ảnh hưởng có thể trì hoãn toàn bộ chuỗi quan trọng. 

Điều này dẫn đến thuật toán tham lam cấp cao nhất. Vào đầu mỗi học kỳ, trong số tất cả các khóa học hiện có, hãy tham gia tối đa`k`các khóa học với cấp độ lớn nhất. Sau khi toàn bộ học kỳ kết thúc, hãy xóa các khóa học đó khỏi biểu đồ và thêm mọi khóa học hiện đã mất tất cả các điều kiện tiên quyết. 

Đây là thuật toán cổ điển cấp cao nhất đầu tiên để lập lịch đơn vị thời gian trên cây, chính xác là cấu trúc thu được ở đây vì mỗi khóa học có nhiều nhất một khóa học kế tiếp. Thuật toán này được biết là tạo ra một lịch trình có khoảng cách tối thiểu cho cấu trúc ưu tiên đặc biệt này. 

Brute-force hoạt động vì nó xem xét rõ ràng mọi lịch trình có thể, nhưng không thành công khi số lượng lịch trình tăng theo cấp số nhân. Quan sát rằng biểu đồ tiên quyết là một khu rừng trong cây cho phép chúng tôi chỉ định mỗi khóa học một mức đường dẫn quan trọng và thay thế việc liệt kê lịch trình bằng hàng đợi ưu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ít nhất là theo cấp số nhân`O(2^n)`tiểu bang |`O(2^n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc người kế nhiệm`a[i]`của mỗi khóa học và tính toán số lượng điều kiện tiên quyết của mỗi khóa học. cho khóa học`v`, đây là số lượng khóa học`u`thỏa mãn`a[u] = v`. Một khóa học với số lượng điều kiện tiên quyết bằng 0 có sẵn khi bắt đầu. 
2. Tính trình độ của từng khóa học. Bắt đầu với tất cả các khóa học không có điều kiện tiên quyết vì chúng là lá của cây điều kiện tiên quyết. Cung cấp cho mỗi cấp độ khóa học như vậy`0`, sau đó xử lý chúng theo thứ tự tôpô. Khi một khóa học`u`chỉ vào`v`, cập nhật`v`với`level[u] + 1`. Từ`v`chỉ khả dụng sau khi tất cả các điều kiện tiên quyết của nó đã được xử lý, giá trị tối đa đó chính xác là độ dài của đường đi dài nhất từ`v`đến khóa học cuối cùng. 
3. Khôi phục số lượng điều kiện tiên quyết ban đầu. Lần vượt qua cấu trúc liên kết đầu tiên chỉ dành cho việc tính toán các cấp độ, trong khi lần vượt qua thứ hai sẽ thể hiện lịch trình học kỳ thực tế. 
4. Đặt mọi khóa học có sẵn ban đầu vào hàng đợi có mức độ ưu tiên tối đa, sử dụng cấp độ của nó làm mức độ ưu tiên. của Python`heapq`là một đống tối thiểu, vì vậy hãy lưu trữ`-level`thay vì. 
5. Vào đầu mỗi học kỳ, loại bỏ tối đa`k`các khóa học khỏi hàng ưu tiên và xếp chúng vào một đợt riêng biệt. Sự tách biệt quan trọng vì các khóa học được mở khóa theo đợt này không thể được thực hiện trong cùng một học kỳ. 
6. Sau khi chọn đợt, đánh dấu tất cả các khóa học của đợt đó là đã hoàn thành. Đối với mỗi khóa học hoàn thành`u`, hãy nhìn vào người kế nhiệm duy nhất của nó`v`. Giảm bớt`v`số lượng điều kiện tiên quyết còn lại của. Nếu nó trở thành 0, hãy chèn`v`vào hàng đợi ưu tiên với cấp độ của nó. 
7. Tăng số học kỳ và lặp lại cho đến khi hoàn thành tất cả các khóa học. Số lần lặp là số học kỳ tối thiểu. 

### Tại sao nó hoạt động 

Bất biến chính là hàng ưu tiên luôn chứa chính xác các khóa học có sẵn hợp pháp vào đầu học kỳ hiện tại, được sắp xếp theo độ dài của đường quan trọng còn lại. 

Đối với cấu trúc biểu đồ này, một khóa học có cấp độ lớn hơn nằm ở đầu chuỗi dài hơn vẫn phải hoàn thành. Việc lập kế hoạch cấp độ đầu tiên cao nhất sẽ lấp đầy mỗi học kỳ với các khóa học mà việc hoãn lại có khả năng kéo dài thời gian hoàn thành cuối cùng lớn nhất. Bởi vì mỗi khóa học có nhiều nhất một khóa học kế tiếp và mỗi khóa học mất đúng một học kỳ làm việc, nên các ràng buộc về quyền ưu tiên hình thành một khu rừng trong cây, cài đặt chính xác trong đó việc lập lịch trình cấp cao nhất là tối ưu. Do đó, mỗi học kỳ tham lam có thể được chọn mà không cần tăng thời gian hoàn thành tối thiểu có thể và sau khi tất cả các khóa học được xử lý, số lượng học kỳ thu được là tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve(stream):
    data = list(map(int, stream.read().split()))
    if not data:
        return ""

    it = iter(data)
    n = next(it)
    k = next(it)

    nxt = [next(it) - 1 for _ in range(n)]

    # indeg[v] = number of prerequisites of v.
    indeg = [0] * n
    for v in nxt:
        if v != -1:
            indeg[v] += 1

    # First topological pass: calculate the level of every course.
    # level[u] is the length of the longest path from u to a terminal course.
    rem = indeg[:]
    level = [0] * n
    q = []

    for u in range(n):
        if rem[u] == 0:
            q.append(u)

    head = 0
    while head < len(q):
        u = q[head]
        head += 1

        v = nxt[u]
        if v != -1:
            level[v] = max(level[v], level[u] + 1)
            rem[v] -= 1
            if rem[v] == 0:
                q.append(v)

    # Second pass: actually construct the optimal schedule.
    rem = indeg[:]

    pq = []
    for u in range(n):
        if rem[u] == 0:
            heapq.heappush(pq, (-level[u], u))

    completed = 0
    semesters = 0

    while completed < n:
        batch = []

        # Choose the courses for this semester before releasing
        # anything unlocked by them.
        take = min(k, len(pq))
        for _ in range(take):
            _, u = heapq.heappop(pq)
            batch.append(u)

        # Complete the whole batch simultaneously.
        for u in batch:
            completed += 1

        # Only now can successors become available.
        for u in batch:
            v = nxt[u]
            if v != -1:
                rem[v] -= 1
                if rem[v] == 0:
                    heapq.heappush(pq, (-level[v], v))

        semesters += 1

    return str(semesters)

def main():
    print(solve(sys.stdin))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve`đọc mảng kế tiếp và xây dựng`indeg`. Vì mỗi khóa học có nhiều nhất một khóa học kế tiếp nên không có danh sách kề nào cần duy trì. Giá trị đơn`nxt[u]`là đủ để tìm ra khóa học duy nhất có thể được mở khóa khi`u`được hoàn thành. 

Quá trình tôpô đầu tiên sử dụng một bản sao có tên`rem`. Mỗi lá ban đầu có`rem[u] == 0`, vì vậy nó có thể bắt đầu quá trình truyền tải. Khi`u`được xử lý, người kế nhiệm của nó`v`đạt được mức độ ứng cử viên`level[u] + 1`. Lấy mức tối đa là cần thiết vì một khóa học có thể có nhiều điều kiện tiên quyết và cấp độ của nó phải chiếm trình độ dài nhất trong chuỗi của chúng. 

Bản sao pass thứ hai`indeg`một lần nữa vì lần vượt qua đầu tiên đã tiêu tốn số lượng điều kiện tiên quyết. Việc sử dụng lại mảng đã sửa đổi sẽ khiến mọi khóa học xuất hiện quá sớm. 

Các cửa hàng xếp hàng ưu tiên`(-level[u], u)`. Việc phủ định cấp độ sẽ biến vùng heap tối thiểu của Python thành vùng heap tối đa cần thiết. Mã số khóa học chỉ được sử dụng như một yếu tố quyết định và không ảnh hưởng đến tính chính xác. 

các`batch`mảng là một chi tiết tinh tế nhưng cần thiết. Trước tiên, chúng tôi xóa tất cả các khóa học trong học kỳ hiện tại, sau đó xử lý các khóa học kế tiếp. Nếu những khóa kế tiếp được chèn vào vùng nhớ ngay sau mỗi lần loại bỏ và vùng nhớ được phép cung cấp một khóa học khác trong cùng một vòng lặp, thì một khóa học có thể được thực hiện trong cùng một học kỳ với điều kiện tiên quyết của nó. Lô riêng biệt sẽ ngăn ngừa lỗi từng cái một. 

Không thể tràn số nguyên trong Python và mức tối đa nhiều nhất là`n - 1`. Đống chứa nhiều nhất`n`các mục nhập, do đó việc sử dụng bộ nhớ của nó vẫn tuyến tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4 2
3 3 4 0
```Các cạnh là`1 -> 3`,`2 -> 3`, Và`3 -> 4`. Các cấp độ là`level[1] = 2`,`level[2] = 2`,`level[3] = 1`, Và`level[4] = 0`. 

| Học kỳ | Có sẵn trước học kỳ | Cấp độ | Các khóa học đã thực hiện | Mới có sẵn | 
| --- | --- | --- | --- | --- | 
| 1 |`1, 2`|`2, 2`|`1, 2`|`3`| 
| 2 |`3`|`1`|`3`|`4`| 
| 3 |`4`|`0`|`4`| không | 

Học kỳ đầu tiên sử dụng cả hai khóa học ở cấp độ quan trọng`2`. Sau khi hoàn thành, khóa học`3`trở nên có sẵn. Khóa học`4`không thể tham gia học kỳ thứ hai vì khóa học`3`được hoàn thành trong học kỳ đó. Câu trả lời là`3`. 

### Mẫu 2 

Đầu vào là```
3 3
0 1 2
```Đồ thị là chuỗi đơn`3 -> 2 -> 1`. Mức độ của nó là`2`,`1`, Và`0`. 

| Học kỳ | Có sẵn trước học kỳ | Cấp độ | Các khóa học đã thực hiện | Mới có sẵn | 
| --- | --- | --- | --- | --- | 
| 1 |`3`|`2`|`3`|`2`| 
| 2 |`2`|`1`|`2`|`1`| 
| 3 |`1`|`0`|`1`| không | 

Mặc dù năng lực là ba khóa học mỗi học kỳ, nhưng chuỗi ưu tiên chỉ cho phép một khóa học ở mỗi giai đoạn. Câu trả lời là`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Mỗi khóa học vào và rời khỏi hàng đợi ưu tiên tối đa một lần và mỗi cạnh được xử lý với số lần không đổi | 
| Không gian |`O(n)`| Mảng kế thừa, số lượng điều kiện tiên quyết, cấp độ, hàng đợi tôpô và hàng đợi ưu tiên đều là tuyến tính | 

chỉ có`n`các mối quan hệ tiên quyết vì mỗi khóa học chỉ có tối đa một khóa học kế tiếp. Với`n <= 10000`,`O(n log n)`chỉ thực hiện một số lượng nhỏ thao tác heap và vừa vặn thoải mái trong giới hạn một giây. Bộ lưu trữ phụ tuyến tính cũng thấp hơn nhiều so với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

input = sys.stdin.readline

def solve(stream):
    data = list(map(int, stream.read().split()))
    if not data:
        return ""

    it = iter(data)
    n = next(it)
    k = next(it)
    nxt = [next(it) - 1 for _ in range(n)]

    indeg = [0] * n
    for v in nxt:
        if v != -1:
            indeg[v] += 1

    rem = indeg[:]
    level = [0] * n
    q = []

    for u in range(n):
        if rem[u] == 0:
            q.append(u)

    head = 0
    while head < len(q):
        u = q[head]
        head += 1

        v = nxt[u]
        if v != -1:
            level[v] = max(level[v], level[u] + 1)
            rem[v] -= 1
            if rem[v] == 0:
                q.append(v)

    rem = indeg[:]
    pq = []

    for u in range(n):
        if rem[u] == 0:
            heapq.heappush(pq, (-level[u], u))

    completed = 0
    semesters = 0

    while completed < n:
        batch = []

        for _ in range(min(k, len(pq))):
            _, u = heapq.heappop(pq)
            batch.append(u)

        completed += len(batch)

        for u in batch:
            v = nxt[u]
            if v != -1:
                rem[v] -= 1

        for u in batch:
            v = nxt[u]
            if v != -1 and rem[v] == 0:
                heapq.heappush(pq, (-level[v], v))

        semesters += 1

    return str(semesters)

def run(inp: str) -> str:
    return solve(io.StringIO(inp)).strip()

# Provided samples
assert run("4 2\n3 3 4 0\n") == "3", "sample 1"
assert run("3 3\n0 1 2\n") == "3", "sample 2"

# Minimum-size input
assert run("1 1\n0\n") == "1", "single course"

# Maximum-size input, all values equal to zero
assert run("10000 10\n" + " ".join(["0"] * 10000) + "\n") == "1000", \
    "10000 independent courses with capacity 10"

# Capacity is large enough for all courses, but precedence still forces a chain
assert run("4 4\n2 3 4 0\n") == "4", \
    "large semester capacity cannot bypass prerequisites"

# Taking arbitrary available courses first would be suboptimal
assert run("5 2\n2 3 0 0 0\n") == "3", \
    "highest-level priority is necessary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`1`| Phiên bản kích thước tối thiểu | 
|`10000 10 / all zeros`|`1000`| Tối đa`n`, khối tối đa và các giá trị kế tiếp hoàn toàn bằng nhau | 
|`4 4 / 2 3 4 0`|`4`| Ưu tiên chiếm ưu thế về dung lượng và bắt lỗi mở khóa trong cùng học kỳ | 
|`5 2 / 2 3 0 0 0`|`3`| Cho thấy tại sao việc lập kế hoạch tôpô tùy ý là không tối ưu | 

## Vỏ cạnh 

Đối với trường hợp một khóa học```
1 1
0
```khóa học duy nhất có số điều kiện tiên quyết là 0 và cấp độ 0. Nó vào hàng ưu tiên ngay lập tức, được chọn trong đợt đầu tiên và số lượng hoàn thành sẽ trở thành một. Vòng lặp dừng sau một học kỳ, tạo ra`1`. 

Đối với trường hợp mỗi khóa học là độc lập,```
6 2
0 0 0 0 0 0
```tất cả sáu khóa học đều có cấp độ 0 và ban đầu có sẵn. Hàng đợi ưu tiên chứa tất cả sáu khóa học và thuật toán thực hiện hai khóa học cùng một lúc. Các đợt có hai, hai và hai khóa học, vì vậy câu trả lời là`3`. Đây chính xác là`ceil(6 / 2)`. 

Đối với công suất lớn với dây chuyền dài,```
4 4
2 3 4 0
```về mặt lý thuyết, tất cả bốn khóa học có thể phù hợp với một học kỳ, nhưng chỉ có khóa học`1`ban đầu có sẵn. Sau học kỳ một, khóa học`2`trở nên có sẵn, theo sau là khóa học`3`, và cuối cùng tất nhiên`4`. Thuật toán tạo ra bốn học kỳ vì các ràng buộc về quyền ưu tiên được kiểm tra trước mỗi đợt. 

Trường hợp lộ rõ ​​nhất là```
5 2
2 3 0 0 0
```Ban đầu, các khóa học`1`,`4`, Và`5`có sẵn. Cấp độ của họ là`2`,`0`, Và`0`. Hàng đợi ưu tiên chọn`1`cùng với một khóa học độc lập, sau đó chọn`2`cùng với khóa học độc lập còn lại và cuối cùng chọn`3`. Kết quả là ba học kỳ. Nếu việc triển khai chỉ sử dụng các khóa học có sẵn theo thứ tự hàng đợi, nó có thể chọn`4`Và`5`đầu tiên và có được bốn học kỳ, đó là lý do tại sao mức độ ưu tiên là điều cần thiết. 

Cuối cùng, các khóa học mới được mở khóa phải chờ học kỳ tiếp theo. TRONG```
4 2
3 3 4 0
```các khóa học`1`Và`2`được thực hiện trong học kỳ một. Chỉ sau khi toàn bộ học kỳ đó kết thúc khóa học`3`nhập bộ có sẵn. Khóa học`4`được ra trường sau học kỳ hai nên phải thi vào học kỳ ba. Việc triển khai theo đợt xử lý vấn đề này một cách chính xác vì các bản cập nhật kế nhiệm chỉ diễn ra sau khi tất cả các khóa học cho học kỳ hiện tại đã được chọn.
