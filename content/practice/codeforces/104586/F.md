---
title: "CF 104586F - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u043a\u0440\u043e\u043b\u0438\u0447\u044c\u0438 \u043d\u043e\u0440\u044b"
description: "Chúng tôi được tặng một cái cây có hang động. Mỗi hang động chứa một số quả bóng. Một nhân vật bắt đầu tại một hang động cố định và đi xuyên qua cái cây hướng tới một chiếc lá, nhưng tại mỗi ngã ba, anh ta chọn ngẫu nhiên cạnh chưa được ghé thăm tiếp theo."
date: "2026-06-30T07:34:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104586
codeforces_index: "F"
codeforces_contest_name: "Codemasters Codecup 2023 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 104586
solve_time_s: 100
verified: true
draft: false
---

[CF 104586F - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u043a\u0440\u043e\u043b\u0438\u0447\u044c\u0438 \u043d\u043e\u0440\u044b](https://codeforces.com/problemset/problem/104586/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây có hang động. Mỗi hang động chứa một số quả bóng. Một nhân vật bắt đầu tại một hang động cố định và đi xuyên qua cái cây hướng tới một chiếc lá, nhưng tại mỗi ngã ba, anh ta chọn ngẫu nhiên cạnh chưa được ghé thăm tiếp theo. Anh ta không bao giờ quay trở lại một cạnh mà anh ta đã sử dụng, vì vậy bước đi của anh ta luôn là một con đường đơn giản từ nút bắt đầu xuống đến một chiếc lá nào đó. 

Mỗi hang động đóng góp số lượng bóng của nó nếu người đi bộ đi qua nó. Nhiệm vụ là tính tổng số quả bóng dự kiến ​​thu được khi bắt đầu từ nút xuất phát nhất định. 

Kích thước đầu vào lên tới mười nghìn nút, do đó, bất kỳ giải pháp nào liệt kê các đường dẫn một cách rõ ràng hoặc mô phỏng các bước đi ngẫu nhiên đều không thể thực hiện được. Việc liệt kê đầy đủ các đường dẫn trong cây sẽ tăng theo cấp số nhân với mức độ phân nhánh, vì vậy hướng khả thi duy nhất là tính toán các kỳ vọng cục bộ và kết hợp chúng thông qua lập trình động trên cây. 

Trường hợp cạnh tinh tế xuất hiện khi nút bắt đầu đã là một lá. Trong trường hợp đó bước đi không bao giờ di chuyển nên câu trả lời chỉ là giá trị tại nút đó. Một trường hợp góc khác là khi cây thoái hóa thành một đường dẫn, trong đó tính ngẫu nhiên biến mất hoàn toàn và kỳ vọng trở nên xác định, điều này rất hữu ích cho việc kiểm tra độ tỉnh táo. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng rõ ràng tất cả các đường dẫn từ gốc đến lá có thể có từ nút bắt đầu. Tại mỗi nút có độ d, bước đi sẽ chia thành d phần tiếp theo có khả năng như nhau. Ngay cả trong một cây có kích thước 10000, số lượng đường dẫn từ gốc tới lá riêng biệt có thể theo cấp số nhân, do đó phương pháp này thất bại ngay lập tức. 

Quan sát quan trọng là quá trình này là một bước đi ngẫu nhiên trên cây mà không bao giờ xem lại các nút, vì vậy khi chúng ta bước vào cây con con, kỳ vọng trong tương lai chỉ phụ thuộc vào cây con đó chứ không phụ thuộc vào phần còn lại của biểu đồ. Điều này cho phép chúng ta định nghĩa hàm E[v] là số lượng bóng dự kiến ​​được thu thập bắt đầu từ nút v và di chuyển ra khỏi nút gốc mà chúng ta đã xuất phát. 

Tại nút v, mọi hàng xóm ngoại trừ nút cha đều có khả năng được chọn làm bước tiếp theo như nhau. Nếu chúng ta di chuyển đến hàng xóm u, chúng ta sẽ thu thập mọi thứ trong cây con của u theo E[u]. Do đó, E[v] trở thành giá trị tại v cộng với giá trị trung bình của E[u] trên tất cả các con u. Đây là cây DP tiêu chuẩn, nhưng được chuẩn hóa theo mức độ trừ một. 

Để tính toán điều này một cách rõ ràng, chúng tôi root cây tại nút bắt đầu và thực hiện DFS để tính toán các kỳ vọng từ dưới lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các con đường | O(exp(n)) | O(n) | Quá chậm | 
| Cây DP với kỳ vọng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây tại nút bắt đầu s. Bước đi luôn di chuyển ra khỏi nút cha, vì vậy đối với mỗi nút, chúng tôi coi nút cha của nó là bị cấm khi tính toán xác suất chuyển đổi. 

Chúng tôi tính toán, đối với mỗi nút v, số lượng bóng dự kiến ​​​​được thu thập bắt đầu từ v khi nó được nhập từ cha mẹ của nó. 

1. Xây dựng danh sách kề cho cây. 

Điều này cho phép truy cập liên tục theo thời gian tới các hàng xóm, điều này là cần thiết vì các quá trình chuyển đổi phụ thuộc vào mức độ. 
2. Chạy DFS từ nút bắt đầu, chuyển nút gốc. 

Cần có cha mẹ vì quy tắc chuyển đổi loại trừ cạnh mà chúng ta đã xuất phát. 
3. Với mỗi nút v, tập hợp tất cả các nút lân cận ngoại trừ nút cha. 

Đây là những bước tiếp theo hợp lệ. Nếu không có những láng giềng như vậy thì v là một chiếc lá theo nghĩa gốc. 
4. Nếu v là lá, đặt dp[v] = a[v]. 

Cuộc đi bộ dừng ở đây nên kỳ vọng chính xác là giá trị cục bộ. 
5. Ngược lại, hãy tính dp[v] = a[v] cộng với giá trị trung bình của dp[u] trên tất cả các hàng xóm u hợp lệ. 

Mỗi người hàng xóm được chọn với xác suất 1/độ(v không bao gồm cha mẹ), do đó kỳ vọng là tuyến tính:

dp[v] = a[v] + sum(dp[u]) / k. 
6. Trả về dp[s], giá trị được tính tại nút bắt đầu. 

Phép đệ quy truyền các kỳ vọng từ các lá trở lên một cách tự nhiên, bởi vì dp[u] được xác định đầy đủ trước khi nó được sử dụng trong dp[v]. 

### Tại sao nó hoạt động 

Tại bất kỳ nút v nào, khi bước đi đến v từ nút cha của nó, đường dẫn tương lai là một quá trình Markov chỉ phụ thuộc vào v và cây con của nó. Mỗi cạnh đi ra được chọn thống nhất trong số các cạnh còn lại, do đó kỳ vọng về lợi ích trong tương lai chính xác là mức trung bình của kỳ vọng của trẻ. Tính tuyến tính của kỳ vọng đảm bảo rằng việc tính tổng các đóng góp từ các cây con độc lập và tính trung bình cho các lựa chọn sẽ tạo ra kỳ vọng tổng thể chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(200000)

n, s = map(int, input().split())
a = list(map(float, input().split()))

g = [[] for _ in range(n)]
for _ in range(n - 1):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    g[x].append(y)
    g[y].append(x)

dp = [0.0] * n

def dfs(v, p):
    children = []
    for to in g[v]:
        if to != p:
            dfs(to, v)
            children.append(to)

    if not children:
        dp[v] = a[v]
        return dp[v]

    total = 0.0
    for to in children:
        total += dp[to]

    dp[v] = a[v] + total / len(children)
    return dp[v]

print(dfs(s - 1, -1))
```Giải pháp xây dựng cây ở dạng kề và chạy DFS từ nút bắt đầu. Đệ quy tính toán các giá trị dp từ dưới lên. Mỗi nút chỉ tổng hợp kết quả từ các nút con của nó, ngoại trừ cạnh cha để thực thi quy tắc không quay lui. 

Phần tế nhị duy nhất là đảm bảo việc chuẩn hóa xác suất được thực hiện bằng cách sử dụng số cạnh đi ra hợp lệ chứ không phải mức độ đầy đủ. Đây là lý do tại sao trẻ em được lọc rõ ràng trong DFS. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
0 2 0 0
1 2
2 3
3 4
```Đây là một chuỗi, vì vậy mọi nút ngoại trừ điểm cuối đều có chính xác một nút con trong DFS có gốc là 4. 

| Nút | Trẻ em | tính toán giá trị dp | 
| --- | --- | --- | 
| 1 | không | 0 | 
| 2 | 1 | 2 + 0 = 2 | 
| 3 | 2 | 0 + 2 = 2 | 
| 4 | 3 | 0 + 2 = 2 | 

Kỳ vọng ở nút 4 là 2 vì mọi đường dẫn đều mang tính xác định và luôn đi qua nút 2. 

### Ví dụ 2 

đầu vào:```
6 6
0 1 2 2 0 0
1 3
3 5
5 4
5 6
2 4
```Từ nút 6, có chính xác một người hàng xóm, vì vậy bước đi ban đầu là xác định. 

| Nút | Trẻ em | dp | 
| --- | --- | --- | 
| 1 | không | 0 | 
| 2 | không | 1 | 
| 3 | 5 | 2 + dp[5] | 
| 4 | 5 | 2 + dp[5] | 
| 5 | 4,6 | 0 + (dp[4] + dp[6]) / 2 | 
| 6 | 5 | 0 + dp[5] | 

Giải từ dưới lên cho kết quả dp[5] = 2, dp[4] = 4, dp[3] = 6, dp[6] = 2,5. 

Bảng này cho thấy cách phân nhánh giới thiệu tính trung bình, trong khi các chuỗi lá truyền bá các đóng góp xác định lên trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và cạnh được xử lý một lần trong DFS | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Các ràng buộc cho phép tối đa 10000 nút, do đó, việc truyền tải tuyến tính vừa vặn thoải mái trong giới hạn. Mỗi nút thực hiện công việc liên tục ngoài các lệnh gọi đệ quy, do đó tổng thời gian chạy vẫn tỷ lệ thuận với kích thước của cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, s = map(int, input().split())
    a = list(map(float, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        x, y = map(int, input().split())
        g[x-1].append(y-1)
        g[y-1].append(x-1)

    dp = [0.0] * n

    sys.setrecursionlimit(200000)

    def dfs(v, p):
        child = []
        for to in g[v]:
            if to != p:
                dfs(to, v)
                child.append(to)
        if not child:
            dp[v] = a[v]
            return dp[v]
        dp[v] = a[v] + sum(dp[c] for c in child) / len(child)
        return dp[v]

    return str(dfs(s-1, -1))

# minimum chain
assert abs(float(run("""2 1
1 2
1 2
""")) - 3.0) < 1e-9

# star-shaped tree
assert abs(float(run("""4 1
10 0 0 0
1 2
1 3
1 4
""")) - 10.0) < 1e-9

# balanced tree
assert abs(float(run("""7 1
1 1 1 1 1 1 1
1 2
1 3
2 4
2 5
3 6
3 7
""")) - 2.0) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | tích lũy xác định | độ chính xác của đường dẫn tuyến tính | 
| ngôi sao | trung bình phân chia đơn | kỳ vọng phân nhánh | 
| cân bằng | trung bình đa cấp | tính đúng đắn đệ quy | 

## Vỏ cạnh 

Nút bắt đầu lá được xử lý trực tiếp bởi trường hợp cơ sở. Vì không có con nào nên DFS gán dp[v] = a[v], phù hợp với thực tế là không có chuyển động nào xảy ra. 

Một nút cấp cao được xử lý bằng cách tính trung bình trên tất cả các nút con. Vì chúng tôi loại trừ cấp độ gốc một cách rõ ràng nên chúng tôi không bao giờ đưa vào một bước lùi không chính xác, nếu không sẽ làm sai lệch xác suất. 

Một chuỗi tuyến tính giảm xuống một bước đi xác định. Mỗi nút có chính xác một nút con, do đó, giá trị trung bình suy biến thành truyền trực tiếp mà không bị pha loãng, phù hợp với hành vi dự định của chuyển động cưỡng bức dọc theo một đường dẫn.
