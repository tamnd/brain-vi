---
title: "CF 106367C - Hướng dẫn của Shichieichou"
description: "Chúng ta có một cây có gốc có gốc là nút 1. Mỗi lá nằm ở cùng độ sâu, do đó cây hoàn toàn cân bằng về độ sâu của lá, mặc dù cấu trúc phân nhánh có thể tùy ý. Mỗi con bướm được gắn vào một nút và biến mất vào một thời điểm nhất định."
date: "2026-06-19T15:04:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106367
codeforces_index: "C"
codeforces_contest_name: "Whalica Cup (Round 2)"
rating: 0
weight: 106367
solve_time_s: 145
verified: true
draft: false
---

[CF 106367C - Hướng dẫn của Shichieichou](https://codeforces.com/problemset/problem/106367/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc là nút 1. Mỗi lá nằm ở cùng độ sâu, do đó cây hoàn toàn cân bằng về độ sâu của lá, mặc dù cấu trúc phân nhánh có thể tùy ý. 

Mỗi con bướm được gắn vào một nút và biến mất vào một thời điểm nhất định. Ao bắt đầu từ bất kỳ lá nào tại thời điểm 0 và cuối cùng phải đến gốc. Di chuyển từ một nút ở độ sâu`d`đến độ sâu`d - 1`luôn tốn một đơn vị thời gian. 

Thông thường, Ao chỉ có thể di chuyển về phía cha mẹ. Một lần trong cuộc hành trình, thay vào đó, cô ấy có thể sử dụng một sức mạnh đặc biệt và di chuyển từ nút hiện tại của mình ở độ sâu`d`đến bất kỳ nút nào ở độ sâu`d - 1`. 

Bất cứ khi nào Ao ghé thăm một nút vào thời điểm nào đó`t`, mọi con bướm tại nút đó có thời gian biến mất ít nhất là`t`có thể được thu thập. 

Nhiệm vụ là chọn lá xuất phát và nước đi đặc biệt tùy chọn sao cho số lượng bướm thu thập được tối đa. 

Kích thước đầu vào lớn. Trong tất cả các trường hợp thử nghiệm, cả tổng số nút và tổng số bướm đều cao nhất`2 · 10^5`. Bất kỳ giải pháp nào kiểm tra từng lá cùng với mọi vị trí nhảy có thể sẽ quá chậm. Chúng tôi cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một quan sát tinh tế là thời gian đến thực tế tại một nút chỉ phụ thuộc vào độ sâu của nó. Vì tất cả các lá đều ở độ sâu`H`, một nút ở độ sâu`d`luôn được ghé thăm đúng lúc`H - d`, bất kể tuyến đường đã chọn. Điều này đơn giản hóa đáng kể vấn đề. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một con bướm biến mất đúng lúc Ao đến.```
3 1
1 2
1 3
1 1
```Các lá ở độ sâu 1 nên rễ đạt được ở thời điểm 1. Một con bướm ở gốc với`t = 1`vẫn có thể được sưu tầm vì điều kiện là`current time ≤ ti`. 

Một cái bẫy khác là chiêu thức đặc biệt không tiết kiệm được thời gian. Từ độ sâu`d`đến độ sâu`d - 1`, cả nước đi gốc bình thường và nước đi đặc biệt đều có giá chính xác bằng một đơn vị. Động thái đặc biệt chỉ thay đổi địa điểm chứ không thay đổi thời gian đến. 

Ví dụ:```
    1
   / \
  2   3
 /     \
4       5
```Nhảy từ nút 4 đến nút 3 vẫn đạt độ sâu 1 tại thời điểm 1. Bất kỳ giải pháp nào coi bước nhảy là một phím tắt kịp thời sẽ tạo ra các kết quả kiểm tra tính đủ điều kiện không chính xác. 

Cạm bẫy thứ ba là quên rằng động thái đặc biệt là tùy chọn. Đôi khi con đường tốt nhất chỉ đơn giản là một con đường từ lá tới gốc bình thường. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi lá bắt đầu, mọi độ sâu có thể có khi sử dụng bước di chuyển đặc biệt và mọi nút đích ở cấp cao hơn tiếp theo. Đối với mỗi tuyến đường ứng cử viên, chúng tôi có thể mô phỏng hành trình và đếm số lượng bướm đã thu thập được. 

Điều này có tác dụng về mặt khái niệm vì mọi hành trình hợp lệ đều có thể được mô tả chính xác theo cách đó. Thật không may, số lượng lá có thể tuyến tính theo`n`và số lượng các lựa chọn nhảy có thể có cũng có thể là tuyến tính. Sự phức tạp kết quả dễ dàng đạt đến`O(n^2)`hoặc tệ hơn, điều đó là không thể đối với`2 · 10^5`nút. 

Quan sát quan trọng là thời gian đến chỉ phụ thuộc vào độ sâu. 

Cho phép`H`là độ sâu lá chung. 

Một con bướm ở nút`v`có thể thu thập được nếu```
ti ≥ H - depth(v)
```Điều kiện này chỉ phụ thuộc vào nút và con bướm. Nó không phụ thuộc vào con đường đã chọn. 

Do đó, chúng tôi có thể xử lý trước từng nút và tính toán:```
value(v) = number of butterflies at v that are collectible whenever v is visited
```Vấn đề ban đầu trở nên thuần túy mang tính cấu trúc. 

Giả sử chiêu thức đặc biệt được sử dụng khi đứng ở nút`a`độ sâu`d`. 

Trước khi nhảy, Ao đi theo con đường từ chiếc lá nào đó hướng lên trên.`a`. 

Sau khi nhảy, Ao đáp xuống một nút nào đó`b`ở độ sâu`d - 1`và sau đó theo chuỗi tổ tiên từ`b`tới tận gốc. 

Các nút được thu thập được chia thành hai phần độc lập: 

1. Một chuỗi đi xuống từ`a`đến một chiếc lá nào đó. 
2. Một chuỗi hướng lên từ gốc tới`b`. 

Vì hai phần này liên quan đến phạm vi độ sâu khác nhau nên điểm số của chúng có thể được tối ưu hóa một cách độc lập. 

Điều này biến vấn đề thành hai phép tính lập trình động cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây tại nút 1 và tính độ sâu của mỗi nút. 
2. Tìm độ sâu lá chung`H`. 
3. Đối với mỗi con bướm`(a, t)`, kiểm tra xem```
t ≥ H - depth(a)
```Nếu đúng thì tăng`value(a)`bởi một. 

Sau bước này, mỗi nút sẽ lưu trữ số lượng bướm được đảm bảo có thể thu thập được bất cứ khi nào nút đó được truy cập. 
4. Tính toán```
prefix(v) = sum of value(x)
            along the root-to-v path
```sử dụng truyền tải DFS hoặc BFS. 
5. Đối với mọi độ sâu`d`, cửa hàng```
bestPrefix[d] =
    maximum prefix(v)
    among all nodes at depth d
```Nếu cú ​​nhảy tiếp đất ở độ sâu`d`, đây là điểm tốt nhất có thể đạt được từ phần gốc của tuyến đường. 
6. Tính DP từ dưới lên:```
down(v) =
    maximum score of a path
    starting at v and ending at a leaf
```Đối với một chiếc lá:```
down(v) = value(v)
```Đối với một nút nội bộ:```
down(v) = value(v) + max(down(child))
```7. Hãy xem xét những hành trình không sử dụng chiêu thức đặc biệt. 

Bất kỳ cuộc hành trình nào như vậy chính xác là một con đường từ lá đến gốc. 

Điểm của nó bằng`prefix(leaf)`. 

Khởi tạo câu trả lời với giá trị lớn nhất trong số tất cả các lá. 
8. Hãy xem xét những hành trình sử dụng chiêu thức đặc biệt. 

Giả sử bước nhảy được thực hiện từ nút`v`ở độ sâu`d ≥ 1`. 

Sự đóng góp tốt nhất của lá là`down(v)`. 

Sự đóng góp tốt nhất của root-side là`bestPrefix[d - 1]`. 

Tổng số điểm là```
down(v) + bestPrefix[d - 1]
```Cập nhật câu trả lời ở mức tối đa trên tất cả các nút không phải gốc. 
9. Xuất ra số điểm tối đa. 

### Tại sao nó hoạt động 

Mỗi nút được truy cập ở độ sâu`d`đạt được vào thời điểm`H - d`, bất kể tuyến đường. Do đó, việc thu thập được một con bướm chỉ phụ thuộc vào nút và thời gian biến mất của nó. Việc thay thế bướm bằng trọng lượng nút sẽ bảo toàn chính xác mục tiêu. 

Nếu chiêu thức đặc biệt được sử dụng từ một nút ở độ sâu`d`, tuyến đường phân hủy thành hai khoảng độ sâu khác nhau. Phần dưới độ sâu`d`đóng góp một chuỗi từ lá tới nút, có mức tối ưu là`down(v)`. Phần trên độ sâu`d`đóng góp chuỗi gốc tới nút kết thúc ở độ sâu nào đó`d - 1`nút, có mức tối ưu là`bestPrefix[d - 1]`. 

Vì hai khoảng này sử dụng độ sâu khác nhau nên chúng không bao giờ trùng nhau và có thể được tối ưu hóa độc lập. Mỗi tuyến hợp lệ tương ứng với chính xác một phân tách như vậy và mọi phân tách tương ứng với một tuyến hợp lệ. Tận dụng tối đa tất cả các vị trí nhảy có thể và cũng xem xét trường hợp không nhảy sẽ đảm bảo câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())

        g = [[] for _ in range(n + 1)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        parent = [0] * (n + 1)
        depth = [0] * (n + 1)

        order = [1]
        parent[1] = -1

        for v in order:
            for to in g[v]:
                if to == parent[v]:
                    continue
                parent[to] = v
                depth[to] = depth[v] + 1
                order.append(to)

        H = 0
        for v in range(2, n + 1):
            if len(g[v]) == 1:
                H = depth[v]
                break

        value = [0] * (n + 1)

        for _ in range(m):
            a, ti = map(int, input().split())
            if ti >= H - depth[a]:
                value[a] += 1

        prefix = [0] * (n + 1)
        best_prefix = [-10**18] * (H + 1)

        for v in order:
            if v == 1:
                prefix[v] = value[v]
            else:
                prefix[v] = prefix[parent[v]] + value[v]

            d = depth[v]
            if prefix[v] > best_prefix[d]:
                best_prefix[d] = prefix[v]

        down = value[:]

        for v in reversed(order):
            best_child = 0

            for to in g[v]:
                if to == parent[v]:
                    continue
                if down[to] > best_child:
                    best_child = down[to]

            down[v] = value[v] + best_child

        answer = 0

        for v in range(2, n + 1):
            if len(g[v]) == 1:
                answer = max(answer, prefix[v])

        for v in range(2, n + 1):
            d = depth[v]
            answer = max(answer, down[v] + best_prefix[d - 1])

        ans.append(str(answer))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Lần duyệt đầu tiên sẽ lấy rễ cây và tính toán độ sâu. Vì tất cả các lá đều có cùng độ sâu nên việc tìm bất kỳ lá nào ngay lập tức sẽ cho giá trị chung`H`. 

Giai đoạn xử lý bướm chuyển đổi vấn đề phụ thuộc thời gian ban đầu thành trọng số nút. Sau thời điểm đó, phần còn lại của thuật toán không bao giờ cần xem xét lại thời gian biến mất.`prefix[v]`lưu trữ điểm số của toàn bộ root-to-`v`xích. Việc ghi lại giá trị tối đa ở mọi độ sâu cho phép chúng tôi trả lời ngay câu hỏi: "Nếu cú ​​nhảy tiếp đất ở độ sâu`d`, điểm gốc tốt nhất hiện có là bao nhiêu?" 

DP từ dưới lên tính toán`down[v]`, điểm cao nhất có thể đạt được bằng cách chọn lá có lợi nhất bên dưới`v`. Quá trình chuyển đổi sử dụng giá trị con tối đa vì mọi đường dẫn từ gốc tới lá chỉ có thể tiếp tục qua một con. 

Một lỗi phổ biến là tính hai lần ranh giới bước nhảy. Công thức```
down(v) + bestPrefix[d - 1]
```tránh điều đó vì hai mảnh chiếm độ sâu khác nhau. Nút ở độ sâu`d`chỉ thuộc về`down(v)`, trong khi nút đích chỉ thuộc về phần tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
1 3
3 0
1 1
```Độ sâu lá phổ biến là`H = 1`. 

Giá trị nút: 

| Nút | Độ sâu | Thời gian cần thiết | Bướm | giá trị | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | (1,1) | 1 | 
| 2 | 1 | 0 | không | 0 | 
| 3 | 1 | 0 | (3,0) | 1 | 

Giá trị tiền tố: 

| Nút | tiền tố | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 2 | 

Giá trị giảm: 

| Nút | xuống | 
| --- | --- | 
| 2 | 0 | 
| 3 | 1 | 
| 1 | 2 | 

Con đường không nhảy tốt nhất là lá 3 về gốc, cho điểm 2. 

Tùy chọn nhảy không thể cải thiện điều này. 

Trả lời:`2`. 

Ví dụ này xác nhận rằng việc đến đúng thời điểm biến mất vẫn được tính. 

### Ví dụ 2 

đầu vào:```
6 3
1 2
1 3
2 4
2 5
3 6
6 1
2 1
1 1
```Độ sâu lá phổ biến là`H = 2`. 

Giá trị nút: 

| Nút | Độ sâu | giá trị | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 6 | 2 | 1 | 
| người khác | - | 0 | 

Giá trị tiền tố: 

| Nút | tiền tố | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 0 | 
| 4 | 1 | 
| 5 | 1 | 
| 6 | 1 | 

Giá trị giảm: 

| Nút | xuống | 
| --- | --- | 
| 6 | 1 | 
| 3 | 1 | 
| 2 | 1 | 
| 1 | 2 | 

Sử dụng bước nhảy từ nút 6 đến nút 2 sẽ thu thập cả hai vùng có lợi nhuận:```
6 -> jump to 2 -> 1
```Điểm = 2. 

Trả lời:`2`. 

Ví dụ này chứng tỏ tại sao nước đi đặc biệt lại có giá trị mặc dù nó không tiết kiệm được thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và bướm được xử lý với số lần không đổi | 
| Không gian | O(n) | Đồ thị, độ sâu, mảng DP và thứ tự truyền tải | 

Tổng số tiền của`n`Và`m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, do đó nghiệm tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout
    return out.getvalue().strip()

# sample-style tests
assert run(
"""1
3 2
1 2
1 3
3 0
1 1
"""
) == "2"

assert run(
"""1
6 3
1 2
1 3
2 4
2 5
3 6
6 1
2 1
1 1
"""
) == "2"

# minimum tree
assert run(
"""1
2 1
1 2
2 0
"""
) == "1"

# no butterflies
assert run(
"""1
3 0
1 2
1 3
"""
) == "0"

# jump strictly improves answer
assert run(
"""1
6 2
1 2
1 3
2 4
2 5
3 6
4 0
3 1
"""
) == "2"

# boundary: disappearance exactly at arrival time
assert run(
"""1
2 1
1 2
1 1
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây tối thiểu có một con bướm | 1 | Trường hợp hợp lệ nhỏ nhất | 
| Không có bướm ở bất cứ đâu | 0 | Xử lý đóng góp trống | 
| Nhảy cải thiện điểm số một cách nghiêm túc | 2 | Sử dụng đúng chiêu thức đặc biệt | 
| Thời gian biến mất bằng thời gian đến | 1 | So sánh toàn diện`≤`| 
| Trường hợp kiểu mẫu | 2, 2 | Chức năng cốt lõi | 

## Vỏ cạnh 

Xét ranh giới đẳng thức:```
1
2 1
1 2
1 1
```Độ sâu của lá là`H = 1`. Root được truy cập vào thời điểm đó`1`. Con bướm cũng có lúc biến mất`1`, nên nó có giá trị sưu tầm. Kiểm tra tiền xử lý sử dụng```
ti >= H - depth(node)
```trở thành```
1 >= 1
```và đếm chính xác con bướm. 

Bây giờ hãy xem xét trường hợp bước nhảy thay đổi vị trí nhưng không thay đổi thời gian:```
1
6 2
1 2
1 3
2 4
2 5
3 6
4 0
3 1
```Nút 4 đóng góp ở độ sâu 2 và nút 3 đóng góp ở độ sâu 1. Một giải pháp sai lầm coi bước nhảy như một phím tắt thời gian sẽ tính toán thời gian đến không chính xác. Thuật toán không bao giờ làm điều này. Thời gian đến chỉ được xác định từ độ sâu nên cả hai con bướm đều được đếm chính xác, mang lại câu trả lời`2`. 

Cuối cùng, hãy xem xét tình huống bỏ qua bước nhảy là tối ưu:```
1
3 1
1 2
1 3
2 0
```Bắt đầu từ lá 2 đã thu thập được con bướm duy nhất. Bất kỳ bước nhảy nào sẽ đến một nhánh khác và không thể cải thiện điểm số. Thuật toán đánh giá rõ ràng tất cả các đường dẫn từ lá tới gốc thông qua`prefix(leaf)`giá trị, vì vậy giá trị tối ưu không nhảy không bao giờ bị bỏ qua.
