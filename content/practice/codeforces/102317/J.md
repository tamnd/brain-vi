---
title: "CF 102317J - Thủy triều dâng"
description: "Chúng ta có một hang động hình chữ nhật được biểu thị bằng một lưới. Mỗi ô chứa chiều cao trần so với mặt biển tại thời điểm 0. Ca nô xuất phát từ ô phía Tây Bắc và phải đến ô phía Đông Nam, chỉ di chuyển giữa các ô liền kề."
date: "2026-08-16T19:08:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "J"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 113
verified: true
draft: false
---

[CF 102317J - Thủy triều dâng cao](https://codeforces.com/problemset/problem/102317/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hang động hình chữ nhật được biểu thị bằng một lưới. Mỗi ô chứa chiều cao trần so với mặt biển tại thời điểm 0. Ca nô xuất phát từ ô phía Tây Bắc và phải đến ô phía Đông Nam, chỉ di chuyển giữa các ô liền kề. 

Biển dâng lên một milimet sau mỗi giây di chuyển. Nếu ca nô đi vào một ô vào thời điểm`t`, có chiều cao trần ban đầu là`a`, thì chiều cao trần còn lại là`a - t`. Ô chỉ hợp lệ khi giá trị này hoàn toàn dương. Mục đích không phải là giảm thiểu thời gian đi lại. Thay vào đó, trong số tất cả các đường đi có thể, chúng ta muốn giá trị lớn nhất có thể có của chiều cao trần nhỏ nhất còn lại dọc theo đường đi đó. Đầu ra được yêu cầu là chiều cao tối thiểu tối đa đó, hoặc`impossible`nếu không có đường dẫn hợp lệ tồn tại. Kho lưu trữ cuộc thi ban đầu cung cấp`1 <= r,c <= 500`và độ cao lên tới`10^9`. 

Với nhiều nhất`500 * 500 = 250000`các ô, một thuật toán chỉ kiểm tra lưới với số lần không đổi là lý tưởng. Thậm chí`O(rc log 10^9)`là hợp lý vì hệ số logarit chỉ khoảng 30. An`O((rc)^2)`phương pháp sẽ yêu cầu khoảng`6.25 * 10^10`hoạt động di động trên hang động lớn nhất, vượt xa những gì một vài giây cho phép. 

Có một số trường hợp khó khăn trong đó việc triển khai có vẻ hợp lý lại đưa ra câu trả lời sai. Đầu tiên, ô bắt đầu được nhập vào thời điểm 0, do đó hang một ô không yêu cầu bất kỳ chuyển động nào. Vì```
1
1 1
7
```câu trả lời là`7`. Việc triển khai giả định ít nhất một nước đi hoặc bắt đầu câu trả lời từ số 0 có thể mắc sai lầm này. 

Vấn đề thứ hai là điều kiện dương tính nghiêm ngặt. Không được phép có trần chính xác bằng 0 khi vào ô. Vì```
1
1 3
3 1 3
```tuyến đường duy nhất đi vào ô giữa tại thời điểm một, mang lại cho nó chiều cao còn lại`1 - 1 = 0`. Đầu ra đúng là`impossible`. Một tấm séc sử dụng`>= 0`thay vì`> 0`sẽ chấp nhận con đường không chính xác. 

Vấn đề thứ ba là đường đi tốt nhất không nhất thiết phải là đường đi ngắn nhất về mặt hình học. Coi như```
1
2 3
10 2 10
10 10 10
```Tuyến đường hàng trên trực tiếp đi vào ô có chiều cao`2`tại một thời điểm và không hợp lệ. Tuyến đường hợp lệ sẽ đi xuống trước, sau đó qua hàng dưới cùng. Độ cao còn lại của nó là`10, 9, 8, 7`, vậy câu trả lời là`7`. Phương pháp chỉ kiểm tra tuyến đường ngắn nhất ở Manhattan sẽ bỏ lỡ đường vòng hợp lệ. 

Bản thân mẫu thể hiện hiện tượng tương tự một cách tinh tế hơn. Ở hang đầu tiên, đến đích trong khi vẫn giữ khoảng cách tối thiểu`3`đòi hỏi phải đi vòng quanh các ô thấp và chấp nhận một lộ trình dài hơn. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ trực tiếp nhất là liệt kê mọi con đường đơn giản có thể từ góc tây bắc đến góc đông nam. Đối với mỗi đường dẫn, chúng tôi biết chính xác thời gian mỗi ô được nhập, vì vậy chúng tôi có thể tính toán`a[cell] - time`cho mọi ô và giữ mức tối thiểu. Lấy mức tối đa trên tất cả các đường dẫn sẽ đưa ra câu trả lời chính xác cần thiết. 

Vấn đề là số lượng đường dẫn. Trong quá trình tìm kiếm đường dẫn đơn giản, sau lần di chuyển đầu tiên, có thể có tối đa ba lựa chọn ở mỗi bước tiếp theo, vì việc quay lại ô trước đó ngay lập tức là không cần thiết. Với`N = rc`các ô, cây tìm kiếm có thể có theo thứ tự`3^N`ứng cử viên bước đi. Ngay cả giới hạn trên thô`3^(N-1)`lớn về mặt thiên văn đối với`N = 250000`. Lực lượng vũ phu là chính xác vì nó xem xét mọi khả năng, nhưng không gian tìm kiếm theo cấp số nhân khiến nó không thể sử dụng được. 

Một quan sát hữu ích hơn là ngừng cố gắng tối ưu hóa đường đi và thay vào đó hãy đặt câu hỏi có hoặc không: liệu chúng ta có thể đạt được khoảng trống tối thiểu ít nhất là`K`? 

Giả sử khoảng trống mục tiêu tối thiểu là`K`. Nếu chúng ta nhập một ô vào thời điểm`t`, ô đó được chấp nhận chính xác khi`a[cell] - t >= K`. 

Đối với một cố định`K`, điều này biến vấn đề thành vấn đề về khả năng tiếp cận. Chúng tôi có thể chạy BFS ngay từ đầu. BFS đến mọi tế bào càng sớm càng tốt và đến sớm hơn luôn tốt hơn vì mực nước biển thấp hơn. Nếu một tế bào có thể đạt được tại thời điểm`t`, thì việc đến cùng một ô sau đó không bao giờ có thể giúp việc di chuyển trong tương lai của nó trở nên dễ dàng hơn. 

Điều này đưa ra một thử nghiệm khả thi đơn giản trong`O(rc)`. Vị từ là đơn điệu: nếu một đường dẫn có thể duy trì khoảng cách tối thiểu`K`, thì cùng một đường dẫn chắc chắn có thể duy trì bất kỳ khoảng trống nhỏ hơn nào. Do đó, chúng ta có thể tìm kiếm nhị phân khả thi lớn nhất`K`. 

Brute-force hoạt động vì nó đánh giá trực tiếp mọi đường dẫn, nhưng không thành công vì có nhiều đường dẫn theo cấp số nhân. Quan sát cho thấy khoảng trống tối thiểu cố định chuyển đổi vấn đề thành khả năng tiếp cận đến sớm nhất cho phép chúng tôi thay thế việc liệt kê đường dẫn bằng BFS và tính đơn điệu của thử nghiệm khả thi đó làm giảm tối ưu hóa tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ, lên đến khoảng`O(3^(rc))`đường dẫn ứng viên |`O(rc)`cho trạng thái DFS | Quá chậm | 
| Tối ưu |`O(rc log 10^9)`|`O(rc)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý câu trả lời của ứng viên`K`như một chiều cao trần tối thiểu cần thiết còn lại. Một ô được nhập vào thời điểm`t`có thể sử dụng được cho ứng viên này chính xác khi`a[cell] - t >= K`. 
2. Chạy BFS từ ô phía tây bắc. Mức BFS biểu thị thời gian đã trôi qua, do đó mọi ô bị xóa khỏi hàng đợi ở cấp độ`t`đã đạt được chính xác sau đó`t`giây. 
3. Khi xét hàng xóm từ xưa đến nay`t`, giả vờ như chúng ta nhập nó vào lúc nào đó`t + 1`. Chúng tôi chỉ xếp hàng nó khi`a[neighbor] - (t + 1) >= K`. Điều này đồng thời kiểm tra tình trạng thủy triều và khoảng trống tối thiểu mong muốn. 
4. Đánh dấu từng ô khi đến lần đầu tiên. BFS đảm bảo rằng lần đến đầu tiên này là lần đến sớm nhất có thể. Việc đến cùng một ô muộn hơn không thể hữu ích vì mọi mức trần trong tương lai sẽ nhỏ hơn vào thời điểm sau đó. 
5. Nếu BFS đến ô phía đông nam thì`K`là khả thi. Nếu BFS làm cạn kiệt các ô có thể truy cập mà không tiếp cận được nó,`K`là không thể. 
6. Vì tính khả thi là đơn điệu nên tìm kiếm nhị phân`K`. Bắt đầu từ`1`, bởi vì một đường dẫn hợp lệ phải có giải phóng mặt bằng tích cực hoàn toàn và sử dụng`min(a[start], a[target])`như một giới hạn trên an toàn. Nếu như`K = 1`không khả thi, hãy in`impossible`. 
7. Trong quá trình tìm kiếm nhị phân, giữ giá trị khả thi lớn nhất. Khi điểm giữa khả thi, hãy tìm kiếm cao hơn. Nếu không, tìm kiếm thấp hơn. 

Tại sao nó hoạt động: cố định`K`, BFS duy trì bất biến rằng mọi ô được truy cập đều có đường dẫn hợp lệ ngay từ đầu mà mọi ô được nhập đều có chiều cao còn lại ít nhất`K`. Bởi vì BFS phát hiện ra mỗi ô vào thời điểm sớm nhất có thể nên bất kỳ đường dẫn thay thế nào đến ô đó sau này đều không thể làm cho điều kiện trở nên dễ dàng hơn. Do đó, BFS đến đích chính xác khi tồn tại một số đường dẫn thỏa mãn yêu cầu. Các giá trị khả thi của`K`tạo thành tiền tố của các số nguyên dương, do đó tìm kiếm nhị phân trả về khoảng trống khả thi lớn nhất. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve_case(r, c, grid):
    width = c + 2
    height = r + 2
    total = width * height

    # Pad the grid with zeroes. Since every tested K is at least 1,
    # the padding can never be entered.
    a = [0] * total
    max_height = 0

    for i in range(r):
        base = (i + 1) * width + 1
        row = grid[i]
        for j, x in enumerate(row):
            a[base + j] = x
            if x > max_height:
                max_height = x

    start = width + 1
    target = r * width + c

    def feasible(k):
        if a[start] < k:
            return False

        seen = bytearray(total)
        seen[start] = 1

        q = deque([start])
        time = 0

        while q:
            time += 1
            next_time = time

            for _ in range(len(q)):
                v = q.popleft()

                # Four neighboring cells in the padded grid.
                for nv in (v - 1, v + 1, v - width, v + width):
                    if seen[nv]:
                        continue

                    if a[nv] - next_time < k:
                        continue

                    if nv == target:
                        return True

                    seen[nv] = 1
                    q.append(nv)

        return start == target

    # A valid path must have strictly positive minimum clearance.
    if not feasible(1):
        return "impossible"

    lo = 1
    hi = min(a[start], a[target])
    answer = 1

    while lo <= hi:
        mid = (lo + hi) // 2

        if feasible(mid):
            answer = mid
            lo = mid + 1
        else:
            hi = mid - 1

    return str(answer)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        r, c = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(r)]
        out.append(solve_case(r, c, grid))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Lưới được đệm bằng một đường viền bằng số 0. Mỗi ngưỡng tìm kiếm nhị phân ít nhất là`1`, nên những tế bào nhân tạo đó không bao giờ thỏa mãn được điều kiện. Điều này tránh việc kiểm tra ranh giới hàng và cột riêng biệt bên trong BFS và làm cho mỗi lần mở rộng chỉ đơn giản là thêm bốn chỉ mục. 

BFS không lưu trữ khoảng cách rõ ràng cho mỗi ô. Thay vào đó, việc xử lý hàng đợi từng cấp một sẽ cung cấp trực tiếp thời gian hiện tại. Khi hàng đợi chứa các ô đạt đến sau`time - 1`giây, tất cả hàng xóm của họ sẽ được nhập sau`time`giây. 

các`seen`mảng là một`bytearray`, hiệu quả về bộ nhớ cao hơn nhiều so với danh sách các giá trị boolean hoặc số nguyên trong Python. Mỗi ô được chèn vào hàng đợi BFS nhiều nhất một lần để kiểm tra tính khả thi cụ thể. 

Điều kiện sử dụng`a[nv] - next_time < k`để từ chối. Tương đương, nó chấp nhận`a[nv] - next_time >= k`. Kể từ khi tìm kiếm nhị phân bắt đầu tại`1`, điều này sẽ tự động thực thi yêu cầu về chiều cao dương nghiêm ngặt ban đầu. 

Việc quay lại sớm khi mục tiêu được phát hiện sẽ lưu phần còn lại của BFS. Ô bắt đầu được xử lý riêng vì nó được nhập vào thời điểm 0, không phải thời điểm một. 

Không có số học dấu phẩy động được sử dụng. Độ cao có thể đạt tới`10^9`, nhưng số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với hang đầu tiên trong mẫu, khoảng trống tối thiểu tối ưu là`3`. Một con đường đạt được nó là`(1,1) -> (2,1) -> (3,1) -> (3,2) -> (3,3) -> (2,3) -> (2,4) -> (2,5) -> (3,5) -> (4,5)`. 

Dấu vết sau đây sử dụng một con đường thành công như vậy và cho thấy tại sao câu trả lời có thể`3`. 

| Bước | Tế bào | Thời gian | Chiều cao ban đầu | Chiều cao còn lại | Tối thiểu cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 0 |`(1,1)`| 0 | 9 | 9 | 9 | 
| 1 |`(2,1)`| 1 | 9 | 8 | 8 | 
| 2 |`(3,1)`| 2 | 9 | 7 | 7 | 
| 3 |`(3,2)`| 3 | 6 | 3 | 3 | 
| 4 |`(3,3)`| 4 | 8 | 4 | 3 | 
| 5 |`(2,3)`| 5 | 8 | 3 | 3 | 
| 6 |`(2,4)`| 6 | 9 | 3 | 3 | 
| 7 |`(2,5)`| 7 | 12 | 5 | 3 | 
| 8 |`(3,5)`| 8 | 12 | 4 | 3 | 
| 9 |`(4,5)`| 9 | 12 | 3 | 3 | 

Chính xác là mức tối thiểu`3`, vì vậy bất kỳ ứng cử viên nào ở trên`3`phải thất bại trong khi`3`thành công. Đây là tình huống trọng tâm mà bài kiểm tra tính khả thi của BFS xử lý: tuyến đường dài hơn rất hữu ích vì nó đến các ô cao hơn thông qua một trình tự không bao giờ để khoảng trống giảm xuống dưới ứng viên. 

Đối với hang động thứ hai, lưới là một cột duy nhất:```
10
1
10
```Con đường khả thi duy nhất là đi thẳng xuống dưới. 

| Bước | Tế bào | Thời gian | Chiều cao ban đầu | Chiều cao còn lại | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 |`(1,1)`| 0 | 10 | 10 | Bắt đầu | 
| 1 |`(2,1)`| 1 | 1 | 0 | Từ chối | 
| 2 |`(3,1)`| 2 | 10 | 8 | Không thể truy cập | 

Ô ở giữa không thể vào được vì trần còn lại của nó bằng 0. Do đó, ngay cả ngưỡng dương nhỏ nhất`K = 1`là không khả thi nên thuật toán in`impossible`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(rc log 10^9)`| Mỗi lần kiểm tra tính khả thi sẽ truy cập từng ô nhiều nhất một lần và tìm kiếm nhị phân thực hiện tối đa khoảng 30 lần kiểm tra. | 
| Không gian |`O(rc)`| Lưới đệm, hàng đợi BFS và mảng đã truy cập đều tuyến tính về số lượng ô. | 

Để tối đa`500 x 500`hang động, tối đa một BFS xử lý`250000`tế bào thực. Do đó, khoảng 30 lần lặp tìm kiếm nhị phân mang lại khoảng 7,5 triệu lượt truy cập ô, với số lượng xử lý lân cận không đổi trong mỗi lượt truy cập. Điều này phù hợp với quy mô dự định của vấn đề tốt hơn nhiều so với việc liệt kê đường dẫn theo cấp số nhân. Kho lưu trữ cuộc thi chỉ định giới hạn thời gian 3 giây và giới hạn bộ nhớ 256 MB cho vấn đề này. 

## Trường hợp thử nghiệm 

Dây nịt sau đây sử dụng tương tự`solve_case`thực hiện như giải pháp được đệ trình. Trường hợp kích thước tối đa được tạo ra thay vì viết ra dưới dạng 250000 số nguyên riêng lẻ.```python
import io
import sys
from collections import deque

def solve_case(r, c, grid):
    width = c + 2
    total = (r + 2) * width

    a = [0] * total
    for i in range(r):
        base = (i + 1) * width + 1
        for j, x in enumerate(grid[i]):
            a[base + j] = x

    start = width + 1
    target = r * width + c

    def feasible(k):
        if a[start] < k:
            return False

        seen = bytearray(total)
        seen[start] = 1
        q = deque([start])
        time = 0

        while q:
            time += 1

            for _ in range(len(q)):
                v = q.popleft()

                for nv in (v - 1, v + 1, v - width, v + width):
                    if seen[nv]:
                        continue
                    if a[nv] - time < k:
                        continue
                    if nv == target:
                        return True

                    seen[nv] = 1
                    q.append(nv)

        return start == target

    if not feasible(1):
        return "impossible"

    lo, hi = 1, min(a[start], a[target])
    ans = 1

    while lo <= hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    return str(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        t = int(sys.stdin.readline())
        out = []

        for _ in range(t):
            r, c = map(int, sys.stdin.readline().split())
            grid = [
                list(map(int, sys.stdin.readline().split()))
                for _ in range(r)
            ]
            out.append(solve_case(r, c, grid))

        return "\n".join(out)
    finally:
        sys.stdin = old_stdin

# Provided sample.
sample = """\
2
4 5
9 5 4 0 0
9 4 8 9 12
9 6 8 7 12
0 0 9 8 12
3 1
10
1
10
"""
assert run(sample) == "3\nimpossible", "provided sample"

# Minimum-size cave.
assert run("""\
1
1 1
7
""") == "7", "single cell"

# All cells equal. A shortest path from (1,1) to (2,2)
# takes two moves, so the minimum remaining height is 5 - 2 = 3.
assert run("""\
1
2 2
5 5
5 5
""") == "3", "all equal values"

# Zero remaining height is not allowed.
assert run("""\
1
1 3
3 1 3
""") == "impossible", "strictly positive entry condition"

# The direct route is blocked, but a detour succeeds.
assert run("""\
1
2 3
10 2 10
10 10 10
""") == "7", "detour around a low cell"

# Maximum-size grid, all values equal.
# The shortest path needs 998 moves in a 500 x 500 grid.
# The last cell therefore has clearance 1_000_000_000 - 998.
r = c = 500
rows = "\n".join([" ".join(["1000000000"] * c) for _ in range(r)])
maximum_case = f"1\n{r} {c}\n{rows}\n"
assert run(maximum_case) == str(1_000_000_000 - 998), "maximum-size grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 7`|`7`| Đầu vào có kích thước tối thiểu và thời gian bằng 0 khi bắt đầu | 
|`2 x 2`, tất cả`5`|`3`| Giá trị hoàn toàn bằng nhau và ảnh hưởng của thời gian đã trôi qua | 
|`1 x 3 / 3 1 3`|`impossible`| Ranh giới giữa khoảng cách bằng 0 và dương | 
|`2 x 3 / 10 2 10 / 10 10 10`|`7`| Đường vòng dài hơn có thể đánh bại tuyến đường trực tiếp | 
|`500 x 500`, tất cả`10^9`|`999999002`| Kích thước tối đa và chiều cao lớn | 

## Vỏ cạnh 

Đối với hang động một ô```
1
1 1
7
```điểm bắt đầu và điểm đến là cùng một ô. Ca nô đi vào lúc 0 nên khoảng cách giữa nó là`7`. Việc kiểm tra tính khả thi ngay lập tức thành công đối với mọi`K <= 7`và tìm kiếm nhị phân trả về`7`. Không cần phải phát minh ra một chuyển động hoặc trừ đi một giây. 

Đối với trường hợp không giải phóng mặt bằng```
1
1 3
3 1 3
```BFS bắt đầu với ô đầu tiên tại thời điểm 0. Ở cấp độ BFS đầu tiên, nó thử ô ở giữa tại thời điểm một và đánh giá`1 - 1 = 0`. Vì mức tối thiểu của ứng viên ít nhất là`1`, ô bị từ chối. Không có con đường nào khác nên`K = 1`thất bại và câu trả lời là`impossible`. Sự so sánh nghiêm ngặt trong mã là điều ngăn cản việc chấp nhận mục nhập có chiều cao bằng 0. 

Để đi đường vòng, hãy xem xét```
1
2 3
10 2 10
10 10 10
```Người hàng xóm hàng đầu có chiều cao`2`và sẽ được nhập vào thời điểm một, chỉ để lại`1`milimét. Thay vào đó, BFS có thể nhập hàng xóm thấp hơn tại thời điểm có chiều cao`10`, sau đó di chuyển sang phải tại thời điểm hai và ba. Các giải phóng mặt bằng là`10, 9, 8, 7`, đưa ra câu trả lời của`7`. Tìm kiếm đến sớm nhất dựa trên hàng đợi sẽ tìm thấy tuyến đường này một cách tự nhiên mà không cần phải liệt kê các tuyến đường. 

Đối với trường hợp hoàn toàn bằng kích thước tối đa, mọi ô đều có chiều cao`10^9`. Vì không có trở ngại nào nên chiến lược tốt nhất chỉ đơn giản là đến đích càng nhanh càng tốt. MỘT`500 x 500`lưới yêu cầu`499 + 499 = 998`di chuyển, do đó điểm đến được nhập với thông tin rõ ràng`10^9 - 998 = 999999002`. BFS của thuật toán nhận ra rằng thời gian đến ngắn nhất là tối ưu khi mọi ô có cùng chiều cao, trong khi tìm kiếm nhị phân tìm thấy chính xác khoảng trống còn lại.
