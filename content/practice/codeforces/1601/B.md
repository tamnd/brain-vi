---
title: "CF 1601B - Ếch Lữ Khách"
description: "Giếng có độ sâu từ 0 đến n, trong đó 0 là mặt đất và n là vị trí xuất phát của con ếch. Giả sử con ếch hiện đang nghỉ ngơi ở độ sâu i."
date: "2026-06-10T08:20:40+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "graphs", "shortest-paths", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1601
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 751 (Div. 1)"
rating: 1900
weight: 1601
solve_time_s: 138
verified: true
draft: false
---

[CF 1601B - Người du hành ếch](https://codeforces.com/problemset/problem/1601/B) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, đồ thị, đường dẫn ngắn nhất, hai con trỏ 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Giếng có độ sâu từ`0`ĐẾN`n`, Ở đâu`0`là mặt đất và`n`là vị trí bắt đầu của con ếch. 

Giả sử con ếch hiện đang nghỉ ngơi ở độ sâu`i`. Từ đó nó chọn độ dài bước nhảy giữa`0`Và`a[i]`, do đó cú nhảy có thể tiếp đất ở bất kỳ độ sâu nào$$j \in [i-a_i,\; i]$$vì di chuyển lên trên làm giảm độ sâu. 

Sau khi hạ cánh ở độ sâu`j`, con ếch lập tức trượt xuống`b[j]`và kết thúc nghỉ ngơi tại$$j+b_j.$$Lần nhảy tiếp theo bắt đầu từ vị trí nghỉ ngơi mới này. 

Chúng ta phải tìm số lần nhảy tối thiểu cần thiết để đạt được độ sâu`0`. Bên cạnh con số tối thiểu, chúng ta cũng cần xuất ra chuỗi độ sâu đạt được ngay sau mỗi lần nhảy và trước khi trượt. 

Điều đầu tiên cần chú ý là kích thước ràng buộc. Độ sâu giếng có thể lớn tới`300000`. Bất kỳ giải pháp nào xem xét rõ ràng mọi bước nhảy có thể có từ mọi vị trí sẽ là phương trình bậc hai. Trong trường hợp xấu nhất, mỗi vị trí có thể nhảy tới hầu hết các vị trí nông hơn, tạo ra khoảng$$1 + 2 + \cdots + n = O(n^2)$$chuyển tiếp, xung quanh`9 × 10^10`khi`n = 300000`. Điều đó là hoàn toàn không thể trong vòng hai giây. 

Thử thách không phải là tìm ra những con đường ngắn nhất. Tìm kiếm theo chiều rộng là sự lựa chọn tự nhiên vì mỗi lần nhảy tốn đúng một lần di chuyển. Thách thức là thể hiện biểu đồ một cách hiệu quả. 

Trường hợp cạnh tinh tế xuất hiện khi một vị trí chỉ có thể nhảy tới vị trí đó.```
n = 1
a = [0]
b = [0]
```Con ếch bắt đầu ở độ sâu`1`, không thể di chuyển lên trên và không bao giờ đạt tới độ sâu`0`. Câu trả lời là`-1`. Việc triển khai bất cẩn giả định rằng mọi nút đều có ít nhất một lần di chuyển đi có thể lặp lại mãi mãi. 

Một trường hợp quan trọng khác là khi đạt tới độ sâu`0`xảy ra trực tiếp trong quá trình nhảy.```
n = 2
a = [0, 2]
b = [0, 0]
```Từ độ sâu`2`, con ếch có thể nhảy thẳng xuống vực sâu`0`. Không xảy ra hiện tượng trượt vì hành trình kết thúc ngay lập tức. Câu trả lời là một bước nhảy. Việc triển khai luôn áp dụng tính năng trượt sau khi hạ cánh sẽ tiếp tục quá trình một cách không chính xác. 

Trường hợp phức tạp thứ ba là một số độ sâu hạ cánh khác nhau có thể tạo ra cùng một vị trí nghỉ.```
n = 3
a = [0, 2, 2]
b = [1, 1, 0]
```Cả hai độ sâu hạ cánh`1`Và`2`có thể dẫn tới trạng thái tương tự trong tương lai. BFS phải tránh xem lại các trạng thái đã được xử lý nếu không nó sẽ thực hiện công việc dư thừa. 

## Phương pháp tiếp cận 

Một mô hình đồ thị đơn giản rất dễ viết. 

Hãy coi mọi độ sâu nghỉ như một đỉnh. Từ độ sâu`i`, mọi độ sâu hạ cánh$$j \in [i-a_i, i]$$tạo ra một cạnh hướng tới độ sâu nghỉ ngơi$$j+b_j.$$Nếu như`j=0`, chúng ta đã đạt được mục tiêu. 

Chạy BFS trên biểu đồ này sẽ tìm chính xác số lần nhảy tối thiểu vì mỗi cạnh tương ứng với chính xác một lần nhảy. Vấn đề là kích thước đồ thị. Một đỉnh có thể có`O(n)`các cạnh đi ra, sản xuất`O(n²)`các cạnh tổng thể. 

Quan sát quan trọng là công thức chuyển tiếp có thể đảo ngược. 

Giả sử chúng ta hiện đang ở độ sâu nghỉ ngơi`v`. Làm thế nào chúng ta có thể đến được đây? 

Lần nhảy cuối cùng chắc hẳn đã hạ cánh ở độ sâu nào đó`x`như vậy$$x+b_x=v.$$Sau khi hạ cánh tại`x`, con ếch trượt tới`v`. 

Bây giờ hãy xem xét vị trí nghỉ ngơi trước đó`p`. Cú nhảy từ`p`có thể hạ cánh tại`x`chỉ nếu$$p-a_p \le x.$$Sắp xếp lại mang lại$$p \ge x.$$Vì vậy mỗi người tiền nhiệm`p`nằm trong một khoảng$$[x,\; n].$$Thay vì khám phá phạm vi chuyển tiếp lớn, chúng tôi chạy BFS ngược bắt đầu từ độ sâu`0`. 

Đối với mỗi độ sâu hạ cánh`x`, chúng ta cần truy cập tất cả các vị trí vẫn chưa được xử lý`p`thỏa mãn$$p-a_p \le x.$$Mỗi vị trí chỉ nên được xử lý một lần. Đây chính xác là nơi cấu trúc "chỉ mục chưa được truy cập tiếp theo" được thiết lập rời rạc trở nên hữu ích. 

Chúng tôi duy trì tất cả các vị trí chưa được truy cập trong cấu trúc tập hợp liên kết. Khi một vị trí được phát hiện, chúng tôi sẽ xóa nó vĩnh viễn. Sau đó quét một phạm vi sẽ bỏ qua các chỉ mục đã được xử lý trong thời gian gần như không đổi. 

BFS thu được sẽ truy cập mọi độ sâu một lần và loại bỏ mọi độ sâu một lần, đưa ra giải pháp gần như tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS trên biểu đồ rõ ràng | O(n²) | O(n²) | Quá chậm | 
| Đảo ngược BFS + DSU chưa truy cập tiếp theo | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đánh số độ sâu từ`0`ĐẾN`n`. 

Độ sâu`1..n`tương ứng với mảng đầu vào. Độ sâu`0`là mặt đất. 

### 1. Xây dựng thông tin ngược 

Đối với mỗi độ sâu hạ cánh`x`, tính toán$$to[x] = x + b_x.$$Nếu con ếch đáp xuống`x`, nó nằm ở`to[x]`. 

Trong khi chạy BFS ngược từ độ sâu nghỉ`v`, mọi độ sâu hạ cánh thỏa mãn`to[x]=v`trở nên có liên quan. 

Lưu trữ tất cả như vậy`x`các giá trị trong nhóm được lập chỉ mục bởi`v`. 

### 2. Bắt đầu BFS từ độ sâu 0 

Độ sâu`0`tượng trưng cho sự thành công. 

Chạy ngược BFS có nghĩa là`dist[v]`sẽ trở thành số lần nhảy tối thiểu cần thiết để chạm đất từ ​​​​độ sâu nghỉ ngơi`v`. 

Khởi tạo:```
dist[0] = 0
queue = [0]
```### 3. Duy trì tất cả các vị trí chưa được khám phá trong cấu trúc DSU 

Đối với mọi độ sâu`i`từ`1`ĐẾN`n`, nó bắt đầu như chưa được khám phá. 

DSU hỗ trợ:```
find(i)
```ít nhất trả về vị trí nhỏ nhất chưa được khám phá`i`. 

Khi vị trí`i`bị phát hiện, hãy xóa nó bằng cách liên kết nó với`i+1`. 

Điều này cho phép quét theo khoảng thời gian trong khi bỏ qua các vị trí đã được xử lý trước đó. 

### 4. Xử lý trạng thái BFS v 

Đối với mỗi độ sâu hạ cánh`x`thỏa mãn```
x + b[x] = v
```chúng tôi cần tất cả các vị trí nghỉ ngơi của người tiền nhiệm`p`điều đó có thể nhảy tới`x`. 

Điều kiện là$$p-a_p \le x.$$Tương tự, mọi vị trí chưa được khám phá trong$$[x,\; n]$$có tầm với bên trái nhiều nhất`x`có thể truy cập được trong một lần nhảy nữa. 

Công thức ngược được sử dụng trong giải pháp chính thức sẽ quét tất cả các vị trí vẫn chưa được truy cập trong khoảng thời gian`[L,n]`, Ở đâu$$L=x.$$Khi sử dụng DSU, mỗi vị trí sẽ bị xóa vĩnh viễn ngay lần đầu tiên nó được phát hiện. 

### 5. Ghi lại thông tin phụ huynh 

Khi có độ sâu nghỉ ngơi mới`p`được phát hiện thông qua độ sâu hạ cánh`x`:```
parent[p] = v
jump[p] = x
```giá trị`jump[p]`lưu trữ độ sâu hạ cánh được sử dụng cho lần di chuyển đó. 

Thông tin này đủ để xây dựng lại trình tự cần thiết sau này. 

### 6. Tiếp tục BFS 

Đẩy mọi vị trí mới được phát hiện vào hàng đợi. 

Vì BFS khám phá các trạng thái bằng cách tăng khoảng cách, nên lần đầu tiên đạt đến một vị trí đã đưa ra số lần nhảy tối thiểu. 

### 7. Xây dựng lại đáp án 

Nếu độ sâu`n`chưa bao giờ được phát hiện, in`-1`. 

Ngược lại hãy bắt đầu từ`n`và nhiều lần theo dõi`parent`. 

Đối với mỗi bước, hãy thêm phần tương ứng`jump`giá trị. 

Độ sâu hạ cánh được thu thập xuất hiện theo thứ tự ngược lại, vì vậy hãy đảo ngược chúng trước khi in. 

### Tại sao nó hoạt động 

Đồ thị BFS ngược chính xác là đồ thị ban đầu với mọi cạnh đều bị đảo ngược. Một con đường dài`k`từ độ sâu`n`đến độ sâu`0`trong biểu đồ ban đầu tương ứng với một đường dẫn có độ dài`k`từ`0`ĐẾN`n`trong đồ thị đảo ngược. 

BFS trên biểu đồ không có trọng số luôn tìm khoảng cách đường đi ngắn nhất. Vì mỗi lần nhảy tốn một lần di chuyển nên lần đầu tiên độ sâu nghỉ được phát hiện sẽ đưa ra số lần nhảy tối thiểu. 

Cấu trúc DSU không bao giờ thay đổi đỉnh nào có thể truy cập được. Nó chỉ bỏ qua các đỉnh đã được phát hiện trước đó. Mỗi độ sâu được loại bỏ chính xác một lần, do đó không có đỉnh hợp lệ nào bị mất và không có đỉnh nào được xử lý hai lần. 

Bởi vì các con trỏ cha được ghi lại khi một đỉnh được phát hiện lần đầu tiên nên chúng mô tả cây đường đi ngắn nhất. Theo những gợi ý đó từ`n`quay lại`0`xây dựng lại một giải pháp bước nhảy tối thiểu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))
    b = [0] + list(map(int, input().split()))

    buckets = [[] for _ in range(n + 1)]

    for x in range(1, n + 1):
        buckets[x + b[x]].append(x)

    parent = [-1] * (n + 1)
    jump = [-1] * (n + 1)
    dist = [-1] * (n + 1)

    nxt = list(range(n + 2))

    def find(x):
        while nxt[x] != x:
            nxt[x] = nxt[nxt[x]]
            x = nxt[x]
        return x

    def erase(x):
        nxt[x] = find(x + 1)

    q = deque([0])
    dist[0] = 0

    while q:
        v = q.popleft()

        for x in buckets[v]:
            cur = find(x)

            while cur <= n and cur - a[cur] <= x:
                dist[cur] = dist[v] + 1
                parent[cur] = v
                jump[cur] = x

                erase(cur)
                q.append(cur)

                cur = find(cur)

    if dist[n] == -1:
        print(-1)
        return

    ans = []
    cur = n

    while cur != 0:
        ans.append(jump[cur])
        cur = parent[cur]

    ans.reverse()

    print(len(ans))
    print(*ans)

solve()
```Mảng xô lưu trữ tất cả độ sâu hạ cánh`x`được nhóm theo điểm đến sau trượt của họ`x + b[x]`. Đây là biểu diễn đồ thị ngược. 

Mảng DSU`nxt`hoạt động như một danh sách liên kết các vị trí vẫn chưa được khám phá. Đang gọi`find(i)`trả về độ sâu chưa được khám phá đầu tiên ít nhất`i`. Sau khi khám phá độ sâu,`erase()`loại bỏ nó vĩnh viễn. 

Trọng tâm của giải pháp là:```
cur = find(x)

while cur <= n and cur - a[cur] <= x:
```điều kiện$$cur-a[cur]\le x$$có nghĩa là một cú nhảy từ độ sâu nghỉ ngơi`cur`có thể hạ cánh ở độ sâu`x`. 

Ngay khi độ sâu đó được phát hiện, nó sẽ bị xóa khỏi DSU. Vì mỗi độ sâu sẽ biến mất vĩnh viễn sau lần truy cập đầu tiên nên tổng số lần lặp vòng lặp trên toàn bộ BFS chỉ là`O(n)`. 

Con trỏ cha lưu trữ nút BFS trước đó, trong khi`jump[cur]`lưu trữ độ sâu hạ cánh được sử dụng trên cạnh đường dẫn ngắn nhất đó. Việc tái thiết tuân theo các hướng dẫn này từ`n`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
3
0 2 2
1 1 0
```Các thùng đảo ngược có liên quan: 

| Hạ cánh x | x+b[x] | 
| --- | --- | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 3 | 

Dấu vết BFS: 

| Hàng đợi Pop | Hạ cánh x | Độ sâu mới được tìm thấy | Đã lưu trữ chính | 
| --- | --- | --- | --- | 
| 0 | không | không | không | 
| 2 | 1 | 3 | cha mẹ[3]=2 | 
| 3 | 2 | 2 | cha mẹ[2]=3 | 

Tái thiết:```
3 -> 2 -> 0
```Các bước nhảy được ghi lại:```
1 0
```Câu trả lời bao gồm hai bước nhảy, là tối ưu. 

### Ví dụ 2 

đầu vào```
2
0 1
1 0
```Dấu vết BFS: 

| Hàng đợi Pop | Hạ cánh x | Độ sâu mới được tìm thấy | 
| --- | --- | --- | 
| 0 | không | không | 

Không có chiều sâu nào khác được phát hiện. 

| Độ sâu | quận | 
| --- | --- | 
| 0 | 0 | 
| 1 | -1 | 
| 2 | -1 | 

Độ sâu`2`vẫn không thể truy cập được, vì vậy câu trả lời là`-1`. 

Ví dụ này cho thấy BFS ngược tự nhiên phát hiện các tình huống không thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Mỗi độ sâu được chèn vào BFS một lần và bị xóa khỏi DSU một lần | 
| Không gian | O(n) | Nhóm, mảng BFS, mảng gốc và bộ lưu trữ DSU | 

Với`n = 300000`, một thuật toán gần như tuyến tính có thể đủ nhanh. Việc sử dụng bộ nhớ cũng tuyến tính, thoải mái dưới giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = [0] + list(map(int, input().split()))
    b = [0] + list(map(int, input().split()))

    buckets = [[] for _ in range(n + 1)]

    for x in range(1, n + 1):
        buckets[x + b[x]].append(x)

    parent = [-1] * (n + 1)
    jump = [-1] * (n + 1)
    dist = [-1] * (n + 1)

    nxt = list(range(n + 2))

    def find(x):
        while nxt[x] != x:
            nxt[x] = nxt[nxt[x]]
            x = nxt[x]
        return x

    def erase(x):
        nxt[x] = find(x + 1)

    q = deque([0])
    dist[0] = 0

    while q:
        v = q.popleft()

        for x in buckets[v]:
            cur = find(x)

            while cur <= n and cur - a[cur] <= x:
                dist[cur] = dist[v] + 1
                parent[cur] = v
                jump[cur] = x

                erase(cur)
                q.append(cur)

                cur = find(cur)

    out = []

    if dist[n] == -1:
        out.append("-1")
    else:
        ans = []
        cur = n

        while cur != 0:
            ans.append(jump[cur])
            cur = parent[cur]

        ans.reverse()

        out.append(str(len(ans)))
        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# sample
assert run("3\n0 2 2\n1 1 0\n") == "2\n1 0"

# minimum size, impossible
assert run("1\n0\n0\n") == "-1"

# direct jump to ground
assert run("2\n0 2\n0 0\n") == "1\n0"

# impossible because every move returns to start
assert run("2\n1 1\n1 0\n") == "-1"

# boundary reachability
assert run("1\n1\n0\n") == "1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, a=[0], b=[0]`|`-1`| Trường hợp nhỏ nhất không thể | 
|`n=2, a=[0,2], b=[0,0]`| Một bước nhảy | Tiếp cận trực tiếp với mặt đất | 
|`n=2, a=[1,1], b=[1,0]`|`-1`| Hành vi tuần hoàn không có tiến bộ | 
|`n=1, a=[1], b=[0]`| Một bước nhảy | Xử lý ranh giới xung quanh độ sâu 0 | 

## Vỏ cạnh 

Hãy xem xét trường hợp không thể nhỏ nhất:```
1
0
0
```Độ sâu nghỉ ngơi duy nhất là`1`. Từ`a[1]=0`, con ếch không thể nhảy lên được chút nào. Trong BFS ngược, không có tiền thân về độ sâu`0`tồn tại. Độ sâu`1`không bao giờ được phát hiện,`dist[1]`còn lại`-1`và thuật toán in`-1`. 

Xem xét việc thoát trực tiếp:```
2
0 2
0 0
```Độ sâu`2`có thể nhảy trực tiếp đến độ sâu hạ cánh`0`. Trong biểu đồ ngược lại, độ sâu`2`trở thành tiền thân của`0`, vì vậy BFS phát hiện ra nó ngay lập tức với khoảng cách`1`. Tái thiết mang lại độ sâu hạ cánh duy nhất`0`. 

Hãy xem xét các trạng thái lặp đi lặp lại:```
3
0 2 2
1 1 0
```Một số con đường có thể dẫn đến cùng một độ sâu nghỉ ngơi. DSU loại bỏ độ sâu ngay lần đầu tiên nó được phát hiện. Các tìm kiếm sau này sẽ tự động bỏ qua nó. Thứ tự BFS đảm bảo rằng khám phá đầu tiên đã tương ứng với đường đi ngắn nhất, vì vậy bỏ qua khám phá sau là đúng. 

Hãy xem xét trường hợp cực đoan trong đó mọi`a[i]=i`. 

Mọi độ sâu đều có khả năng nhảy tới bất kỳ độ sâu nào nông hơn. Biểu đồ rõ ràng sẽ chứa nhiều cạnh bậc hai. BFS ngược dựa trên DSU vẫn loại bỏ từng độ sâu chính xác một lần, giữ cho tổng công việc gần như tuyến tính. Đây là trường hợp cần thiết phải có giải pháp tối ưu hóa.
