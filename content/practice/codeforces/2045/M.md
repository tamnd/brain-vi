---
title: "CF 2045M - Mê cung gương"
description: "Hãy tưởng tượng chùm tia laser đang di chuyển dọc theo các đường lưới giữa các ô. Bất cứ khi nào chùm tia đi vào một ô qua một phía, nội dung của ô đó sẽ xác định nó sẽ rời đi từ phía nào. Một ô trống không đổi hướng."
date: "2026-06-08T09:20:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2045
codeforces_index: "M"
codeforces_contest_name: "2024-2025 ICPC Asia Jakarta Regional Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1800
weight: 2045
solve_time_s: 94
verified: true
draft: false
---

[CF 2045M - Mê cung gương](https://codeforces.com/problemset/problem/2045/M) 

**Đánh giá:** 1800 
**Tags:** bạo lực, dfs và tương tự, đồ thị, triển khai 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy tưởng tượng chùm tia laser đang di chuyển dọc theo các đường lưới giữa các ô. Bất cứ khi nào chùm tia đi vào một ô qua một phía, nội dung của ô đó sẽ xác định nó sẽ rời đi từ phía nào. 

Một ô trống không đổi hướng. MỘT`/`gương quay chùm tia đi một góc 90 độ và`\`gương quay nó 90 độ theo hướng khác. 

Một tia laser được đặt bên ngoài lưới trên một trong các lỗ ranh giới. Sau khi vào lưới, chùm tia đi theo một đường xác định cho đến khi cuối cùng nó lại rời khỏi lưới. 

Chúng ta phải tìm mọi lỗ biên có chùm tia chiếu tới mọi ô gương trong lưới ít nhất một lần. 

Kích thước lưới tối đa là 200 × 200. Chỉ có`2(R + C)`các vị trí laser có thể có, tối đa là 800. Có thể mô phỏng đơn giản từ mọi ranh giới, nhưng chúng ta phải cẩn thận về lượng công việc được thực hiện trên mỗi mô phỏng. 

Phần tinh tế nhất là hiểu "đánh vào mọi gương" nghĩa là gì. Một tấm gương bị chiếu vào bất cứ khi nào chùm tia đi vào ô đó và phản chiếu lên tấm gương đó. Chúng tôi không quan tâm nó bị tấn công bao nhiêu lần. 

Một lỗi phổ biến là mô phỏng độc lập từng ranh giới và đếm các gương đã ghé thăm. Cách đó có tác dụng nhưng nó bỏ qua đặc tính cấu trúc mạnh hơn của mê cung. 

Một sai lầm dễ mắc phải khác là cho rằng các lối vào khác nhau tạo ra các đường dẫn không liên quan. Trên thực tế, mọi đường truyền tia là một phần của biểu đồ cố định được xác định hoàn toàn bởi lưới. Khi biểu đồ đó được xây dựng, tất cả các câu trả lời có thể được trích xuất gần như ngay lập tức. 

Hãy xem xét ví dụ nhỏ này:```
1 1
/
```Chiếc gương duy nhất được chạm vào tất cả bốn lối vào có thể có, vì vậy câu trả lời chứa tất cả bốn vị trí biên. 

Giải pháp chỉ theo dõi các ô thay vì trạng thái chùm tia có thể không thành công vì việc nhập vào cùng một ô từ các hướng khác nhau có thể dẫn đến các lần tiếp tục khác nhau. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi lối vào ranh giới, hãy mô phỏng chùm tia cho đến khi nó rời khỏi lưới. Trong quá trình mô phỏng, đánh dấu từng gương được chạm vào. Nếu chạm vào tất cả các gương, hãy ghi lại lối vào đó. 

Một chùm tia có thể ghé thăm nhiều nhất`O(RC)`trạng thái vì trạng thái được xác định bởi một ô và một hướng. có`O(R + C)`lối vào. Tổng độ phức tạp trở thành`O((R + C)RC)`. 

Với`R = C = 200`, đây là đại khái```
800 × 40000 = 32,000,000
```các chuyển đổi trạng thái. 

Con số đó không lớn đến mức vô lý, nhưng có một quan sát rõ ràng hơn. 

Chuyển động của chùm tia là hoàn toàn xác định. Mỗi mặt của mỗi ô được kết nối với chính xác một mặt khác. Điều này có nghĩa là toàn bộ mê cung thực sự là một tập hợp các thành phần đường dẫn được kết nối. 

Thay vì truy tìm từng tia laser riêng biệt, chúng ta có thể chế tạo các bộ phận đó một lần. 

Biểu diễn mỗi cạnh của ô dưới dạng một nút. 

Bên trong một ô, nội dung xác định các cạnh được kết nối: 

Đối với ô trống:```
N <-> S
W <-> E
```Vì`/`:```
N <-> W
S <-> E
```Vì`\`:```
N <-> E
S <-> W
```Các ô liền kề cũng kết nối thông qua đường viền chung của chúng. 

Sau khi tất cả các kết nối được thêm vào, mọi nút đều thuộc về một thành phần DSU nào đó. 

Bây giờ hãy xem xét một tế bào gương. 

Một chùm tia chạm vào tấm gương đó nếu đường đi của nó đi vào một trong hai nhánh thành phần đi qua gương. Chúng ta có thể đếm, đối với mỗi thành phần, có bao nhiêu gương có thể truy cập được từ thành phần đó. 

Nếu một lỗ mở ranh giới thuộc về một thành phần có thể chạm tới tất cả các gương thì lỗ mở đó là một câu trả lời hợp lệ. 

Điều này làm giảm toàn bộ vấn đề xuống còn một cấu trúc DSU cộng với một lượt đếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((R+C)RC) | O(RC) | Về mặt khái niệm quá chậm, không cần thiết | 
| Tối ưu | O(RC α(RC)) | O(RC) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo nút DSU cho mọi phía của mỗi ô. 
2. Nối các cạnh ô theo nội dung ô. 

Đối với một ô trống, kết nối phía bắc với phía nam và phía tây với phía đông. 

Đối với một`/`gương, nối bắc với tây, nam với đông. 

Đối với một`\`gương, nối bắc với đông, nam với tây. 
3. Sử dụng sơ đồ đánh số trong đó hai ô lân cận tự động chia sẻ cùng một nút viền. 

Mô hình này di chuyển qua các ranh giới ô mà không cần thêm các cạnh bổ sung sau này. 
4. Sau khi tất cả các phép hợp được thực hiện, mỗi thành phần DSU đại diện cho một thành phần quỹ đạo chùm tia tối đa. 
5. Hãy để`tot`là tổng số gương. 
6. Với mỗi ô gương, xác định hai thành phần DSU tương ứng với hai cạnh của gương đó. 

Vì`/`, gương tách ra`(N,W)`từ`(S,E)`. 

Vì`\`, gương tách ra`(N,E)`từ`(S,W)`. 

Một chùm tia của hai bên có thể chiếu vào tấm gương đó. 
7. Đối với mỗi gương, hãy cộng một số đếm cho cả hai thành phần DSU có liên quan. 

Nếu cả hai bên đều thuộc cùng một thành phần thì chỉ thêm một lần. 
8. Với mỗi lần mở ranh giới, hãy tìm thành phần DSU của nó. 
9. Nếu số thành phần đó bằng`tot`, thì mọi gương đều có thể truy cập được từ lối vào đó, vì vậy hãy ghi lại chuỗi câu trả lời tương ứng. 

### Tại sao nó hoạt động 

Thành phần DSU đại diện cho tất cả các trạng thái chùm tia được kết nối bởi các quy tắc chuyển động xác định. 

Đối với một gương cụ thể, một chùm tia chỉ có thể chạm vào nó nếu chùm tia đó thuộc về một trong hai thành phần quỹ đạo chạm vào gương đó. Trong bước đếm, chúng ta đăng ký tấm gương đó cho chính xác những thành phần đó. 

Sau khi xử lý tất cả các gương,`cnt[component]`bằng số lượng gương riêng biệt có thể bị ảnh hưởng bởi bất kỳ trạng thái chùm tia nào bên trong thành phần đó. 

Một lối vào ranh giới thuộc về chính xác một thành phần. Chùm tia của nó có thể chiếu tới mọi gương nếu thành phần đó được liên kết với mọi gương. Đó chính xác là điều kiện```
cnt[component] = total number of mirrors.
```Vì vậy, mọi lối vào được báo cáo đều hợp lệ và mọi lối vào hợp lệ đều được báo cáo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

R, C = map(int, input().split())
grid = [input().strip() for _ in range(R)]

MAXN = 100000

parent = list(range(MAXN))
cnt = [0] * MAXN

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b):
    ra = find(a)
    rb = find(b)
    if ra != rb:
        parent[ra] = rb

# directions:
# 1 = N, 2 = S, 3 = W, 4 = E
def node(i, j, d):
    if d == 1:
        return (i - 1) * C + j
    if d == 2:
        return i * C + j
    return (R + 1) * C + (i - 1) * (C + 1) + j + (1 if d == 4 else 0)

for i in range(1, R + 1):
    for j in range(1, C + 1):
        ch = grid[i - 1][j - 1]

        if ch == '.':
            union(node(i, j, 1), node(i, j, 2))
            union(node(i, j, 3), node(i, j, 4))
        elif ch == '/':
            union(node(i, j, 1), node(i, j, 3))
            union(node(i, j, 2), node(i, j, 4))
        else:  # '\'
            union(node(i, j, 1), node(i, j, 4))
            union(node(i, j, 2), node(i, j, 3))

total_mirrors = 0

for i in range(1, R + 1):
    for j in range(1, C + 1):
        ch = grid[i - 1][j - 1]

        if ch == '.':
            continue

        total_mirrors += 1

        if ch == '/':
            a = find(node(i, j, 1))
            b = find(node(i, j, 2))
        else:
            a = find(node(i, j, 1))
            b = find(node(i, j, 3))

        cnt[a] += 1
        if a != b:
            cnt[b] += 1

ans = []

for r in range(1, R + 1):
    if cnt[find(node(r, 1, 3))] == total_mirrors:
        ans.append(f"W{r}")

for r in range(1, R + 1):
    if cnt[find(node(r, C, 4))] == total_mirrors:
        ans.append(f"E{r}")

for c in range(1, C + 1):
    if cnt[find(node(1, c, 1))] == total_mirrors:
        ans.append(f"N{c}")

for c in range(1, C + 1):
    if cnt[find(node(R, c, 2))] == total_mirrors:
        ans.append(f"S{c}")

print(len(ans))
if ans:
    print(*ans)
```Chức năng đánh số là chi tiết triển khai chính. Thay vì tạo các nút riêng biệt cho mỗi cạnh ô và sau đó kết nối rõ ràng các ô lân cận, sơ đồ đánh số sẽ gán cùng một ID cho cả hai cạnh của đường viền chung. Điều đó tự động mô hình hóa chuyển động giữa các ô liền kề. 

Giai đoạn đếm cũng tinh tế. Một tấm gương đóng góp vào hai thành phần quỹ đạo. Nếu cả hai bên đều thuộc về cùng một thành phần DSU thì chúng ta chỉ phải đếm gương một lần cho thành phần đó. 

Các khoảng mở ranh giới tương ứng trực tiếp với các đoạn viền bên ngoài. Bốn vòng cuối cùng chỉ đơn giản là kiểm tra các nút viền đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
1 1
/
```Có một tấm gương. 

| Gương | Hợp phần A | Hợp phần B | 
| --- | --- | --- | 
| (1,1) '/' | Phía Tây Bắc | bên S/E | 

Cả hai thành phần đều nhận được số lượng`1`. 

| Lối vào | Đếm thành phần | hợp lệ | 
| --- | --- | --- | 
| N1 | 1 | Có | 
| S1 | 1 | Có | 
| W1 | 1 | Có | 
| E1 | 1 | Có | 

Đầu ra:```
4
N1 S1 W1 E1
```Điều này chứng tỏ rằng một gương duy nhất có thể được tiếp cận từ nhiều lối vào ranh giới độc lập. 

### Ví dụ 2```
2 2
..
./
```Tổng số gương = 1. 

Sau khi xây dựng DSU, một số lỗ biên thuộc về các thành phần không bao giờ chạm tới gương. Số lượng thành phần của họ vẫn bằng không. 

| Lối vào | Đếm thành phần | hợp lệ | 
| --- | --- | --- | 
| N1 | 0 | Không | 
| N2 | 1 | Có | 
| W2 | 1 | Có | 
| S1 | 0 | Không | 

Chỉ những lối vào có thành phần nhìn thấy gương mới được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(RC α(RC)) | Mỗi hoạt động liên kết/tìm gần như không đổi | 
| Không gian | O(RC) | Nút DSU và số lượng gương | 

Lưới chứa tối đa 40.000 ô. Số lượng nút DSU tỷ lệ thuận với số lượng đường viền lưới, cũng`O(RC)`. Điều này dễ dàng phù hợp với cả giới hạn thời gian 1 giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    # paste solution into a solve() function and call it
    pass

# sample 1
# output order is not unique, so real judging should compare sets

# single mirror
inp = """\
1 1
/
"""

# every entrance hits the only mirror
# expected count = 4

# minimum size with backslash mirror
inp2 = """\
1 1
\\
"""

# all entrances valid again

# mirror isolated from some boundaries
inp3 = """\
2 2
..
./
"""

# no mirrors except one corner mirror

# large empty grid except one mirror
inp4 = "200 200\n" + ("." * 199 + "/\n") + ("." * 200 + "\n") * 199
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`với`/`| 4 câu trả lời | Lưới tối thiểu | 
|`1×1`với`\`| 4 câu trả lời | Xử lý hướng gương | 
| Gương góc đơn | Bộ một phần lối vào | Lập bản đồ ranh giới | 
|`200×200`lưới | Kết thúc nhanh chóng | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét```
1 1
/
```Hai mặt của gương thuộc các thành phần DSU khác nhau. Thuật toán tăng cả hai bộ đếm thành phần. Mỗi lỗ mở ranh giới thuộc về một trong hai thành phần đó, vì vậy cả bốn lối vào đều được báo cáo. 

Coi như```
1 1
\
```Phân vùng nhân bản thay đổi nhưng logic đếm giống hệt nhau. Thuật toán không dựa vào mô phỏng hình học mà chỉ dựa vào cấu trúc thành phần nên cả hai hướng gương đều được xử lý thống nhất. 

Coi như```
2 2
..
./
```Một số lỗ biên thuộc về các thành phần không bao giờ chạm vào gương. Số lượng thành phần của chúng vẫn bằng 0, trong khi các thành phần chạm vào gương sẽ nhận được số lượng một. Thử nghiệm bình đẳng cuối cùng đối với tổng số gương sẽ tự động lọc chúng ra. 

Trường hợp khó phát hiện nhất là khi cả hai mặt của gương đều thuộc cùng một thành phần DSU. Việc kiểm tra việc thực hiện`if a != b`trước khi thêm số thứ hai. Nếu không có điều kiện đó, một chiếc gương sẽ được tính hai lần và một số lối vào hợp lệ sẽ bị từ chối.
