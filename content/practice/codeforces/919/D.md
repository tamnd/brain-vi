---
title: "CF 919D - Chuỗi con"
description: "Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh mang một chữ cái viết thường. Bước đi hợp lệ đi theo các cạnh được định hướng từ nút này sang nút khác và chúng ta được phép truy cập lại các nút và cạnh miễn là chúng ta tôn trọng hướng."
date: "2026-06-13T02:42:04+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 919
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 460 (Div. 2)"
rating: 1700
weight: 919
solve_time_s: 767
verified: true
draft: false
---

[CF 919D - Chuỗi con](https://codeforces.com/problemset/problem/919/D) 

**Đánh giá:** 1700 
**Thẻ:** dfs và các đồ thị, dp, tương tự 
**Thời gian giải:** 12 phút 47 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh mang một chữ cái viết thường. Bước đi hợp lệ đi theo các cạnh được định hướng từ nút này sang nút khác và chúng ta được phép truy cập lại các nút và cạnh miễn là chúng ta tôn trọng hướng. 

Đối với bất kỳ bước đi nào như vậy, chúng tôi xem xét chuỗi các chữ cái mà chúng tôi gặp dọc theo các nút đã truy cập. Giá trị của bước đi được xác định là tần số tối đa của bất kỳ chữ cái nào dọc theo chuỗi đó. Ví dụ: nếu bước đi tạo ra chuỗi “abaca”, thì chữ cái tốt nhất là ‘a’, xuất hiện 3 lần, do đó giá trị là 3. 

Nhiệm vụ là tìm một bước đi có giá trị lớn nhất có thể. Nếu tồn tại một chu trình cho phép chúng ta tiếp tục tích lũy số lần xuất hiện của một chữ cái nào đó vô thời hạn thì câu trả lời là không giới hạn và chúng ta phải trả về -1. 

Kích thước đầu vào đạt tới 300.000 nút và cạnh. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê các đường dẫn hoặc thực hiện khám phá theo cấp số nhân. Ngay cả O(n²) cũng quá lớn trong thực tế, vì vậy chúng ta buộc phải sử dụng chiến lược duyệt đồ thị tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi đồ thị chứa một chu trình có hướng. Nếu chu trình đó có thể truy cập được và chứa ít nhất một nút đóng góp một chữ cái cụ thể thì bằng cách lặp vô thời hạn, chúng ta có thể tăng số lượng chữ cái đó mà không bị ràng buộc. Ví dụ: nếu các nút 1 → 2 → 3 → 1 tạo thành một chu trình và tất cả đều có chữ cái 'a' thì câu trả lời là -1. Một DFS ngây thơ theo dõi tổng đường dẫn tốt nhất mà không phát hiện các lần truy cập lại sẽ coi các chu kỳ là hữu hạn và tạo ra một câu trả lời bị chặn một cách không chính xác. 

Một vấn đề khác là ngay cả khi các chu kỳ tồn tại, không phải tất cả chúng đều bao hàm sự vô hạn. Nếu một chu trình tồn tại nhưng chúng ta đang đếm một chữ cái không có trong chu trình đó thì chu trình đó không giúp tăng tần số của chữ cái đó. Lý do chính xác phụ thuộc vào việc theo dõi tích lũy theo từng chữ cái cụ thể chứ không chỉ khả năng tiếp cận. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng khám phá tất cả các đường dẫn trong biểu đồ và tính toán tần số chữ cái dọc theo mỗi đường dẫn. Người ta có thể tưởng tượng việc thực hiện DFS từ mọi nút, duy trì dải tần số có độ dài 26 và cập nhật mức tối đa toàn cầu. Điều này đúng về mặt khái niệm đối với các biểu đồ nhỏ nhưng không thành công vì mỗi nút có thể được xem lại qua nhiều đường dẫn và các chu trình tạo ra vô số bước đi. Ngay cả khi chúng ta giới hạn bản thân trong những đường đi đơn giản, số lượng chúng trong đồ thị có hướng có thể tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì suy nghĩ về các đường dẫn, chúng tôi coi đây là bài toán đường dẫn dài nhất trên DAG, nhưng có một điểm thay đổi: chúng tôi cần 26 chương trình động độc lập, mỗi chương trình cho mỗi chữ cái. Với mỗi chữ cái c, chúng ta muốn tính số lần xuất hiện tối đa của c dọc theo bất kỳ đường dẫn nào kết thúc tại mỗi nút. 

Nếu đồ thị không có chu trình thì điều này sẽ đơn giản. Chúng ta có thể sắp xếp đồ thị theo cấu trúc liên kết và nới lỏng các chuyển đổi: đối với mỗi cạnh u → v, chúng ta truyền các giá trị tốt nhất về phía trước. Sự hiện diện của các chu trình phá vỡ điều này, nhưng chúng ta có thể phát hiện các chu trình bằng thuật toán Kahn. Nếu chúng ta không xử lý được tất cả các nút theo thứ tự tôpô thì sẽ tồn tại một chu trình và vì bất kỳ chu trình nào cũng có thể được lặp đi lặp lại dọc theo một đường dẫn, điều đó hàm ý khả năng tích lũy lớn tùy ý đối với ít nhất một chữ cái dọc theo một chu trình có thể tiếp cận nào đó. Trong trường hợp đó, chúng tôi trả về -1. 

Do đó, giải pháp trở thành sự kết hợp giữa phát hiện chu trình và lập trình động đa nguồn trên cấu trúc DAG. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS trên đường dẫn | Hàm mũ | O(n) | Quá chậm | 
| Toposort + DP mỗi chữ cái | O(26 · (n + m)) | O(26 · n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chữ cái một cách độc lập và tính toán số lượng tốt nhất có thể bằng cách sử dụng DP theo thứ tự tôpô.

1. Xây dựng danh sách kề và tính toán độ của tất cả các nút. Điều này giúp chúng ta chuẩn bị cho thuật toán Kahn, thuật toán xử lý các nút theo thứ tự tôpô hợp lệ khi đồ thị có tính chu kỳ. 
2. Khởi tạo bảng DP`dp[v][c]`nghĩa là số lần xuất hiện tối đa của chữ cái`c`dọc theo bất kỳ đường dẫn nào kết thúc tại nút`v`. Ban đầu tất cả đều là số không. Nếu nút`v`bản thân nó có chữ cái`c`, chúng tôi thiết lập`dp[v][c] = 1`. 
3. Đẩy tất cả các nút có bậc 0 vào hàng đợi. Đây là những điểm khởi đầu hợp lệ cho bất kỳ con đường nào. 
4. Liên tục bật một nút`u`từ hàng đợi và truyền các giá trị DP của nó tới các hàng xóm`v`. Đối với mỗi chữ cái`c`, chúng tôi cố gắng cập nhật:`dp[v][c] = max(dp[v][c], dp[u][c] + (1 if s[v] == c else 0))`. 

Bước này đảm bảo rằng mọi đường dẫn tốt nhất kết thúc tại`u`có thể được mở rộng để`v`. 
5. Bất cứ khi nào chúng ta nới lỏng một cạnh, hãy giảm mức độ của nút đích. Nếu nó bằng 0, hãy đẩy nó vào hàng đợi. Điều này duy trì thứ tự xử lý tôpô chính xác. 
6. Giữ một bộ đếm các nút đã xử lý. Nếu sau khi hàng đợi kết thúc mà chúng ta chưa xử lý được tất cả các nút thì sẽ tồn tại một chu trình. Trong trường hợp đó, trả về -1 vì một số chu trình có thể được sử dụng để lặp lại quá trình truyền tải vô thời hạn, làm cho câu trả lời không bị giới hạn. 
7. Sau khi xử lý, câu trả lời là giá trị lớn nhất trên tất cả dp[v][c]. 

Lý do đằng sau DP là bất kỳ đường dẫn hợp lệ nào trong DAG đều có thể được phân tách theo thứ tự tôpô và cấu trúc con tối ưu được giữ nguyên vì việc mở rộng đường dẫn tốt nhất vào hàng xóm sẽ duy trì tính tối ưu. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là khi một nút`u`được đưa ra khỏi hàng đợi, tất cả các đường dẫn kết thúc tại`u`đã được xem xét đầy đủ về mặt giá trị DP. Điều này được đảm bảo bằng thứ tự tôpô: mọi cạnh tới`u`bắt nguồn từ một nút đã được xử lý. 

Vì thế,`dp[u][c]`là cuối cùng khi`u`được xử lý và tất cả các bản cập nhật trong tương lai cho các hàng xóm đều được xây dựng chính xác trên một giải pháp bài toán con hoàn chỉnh. Vì mọi biểu đồ tuần hoàn đều thừa nhận thứ tự tôpô, điều này đảm bảo chúng tôi khám phá tất cả các đường dẫn hợp lệ chính xác một lần theo thứ tự phụ thuộc. 

Nếu một chu trình tồn tại, thuật toán của Kahn không thể xử lý tất cả các nút, nghĩa là không có thứ tự tôpô hợp lệ. Điều đó tương ứng chính xác với sự tồn tại của một chu trình, mà trong bài toán này ngụ ý khả năng lặp lại vô hạn dọc theo một đường nào đó, do đó trả về -1 là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    s = input().strip()

    g = [[] for _ in range(n)]
    indeg = [0] * n

    for _ in range(m):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        g[x].append(y)
        indeg[y] += 1

    dp = [[0] * 26 for _ in range(n)]

    for i in range(n):
        dp[i][ord(s[i]) - 97] = 1

    q = deque([i for i in range(n) if indeg[i] == 0])
    visited = 0

    while q:
        u = q.popleft()
        visited += 1

        for v in g[u]:
            for c in range(26):
                val = dp[u][c] + (1 if ord(s[v]) - 97 == c else 0)
                if val > dp[v][c]:
                    dp[v][c] = val

            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    if visited < n:
        print(-1)
        return

    ans = 0
    for i in range(n):
        ans = max(ans, max(dp[i]))
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên thuật toán của Kahn để thực thi thứ tự xử lý theo chu kỳ. Bảng DP được cập nhật tại chỗ trong quá trình thư giãn cạnh và mỗi trạng thái biểu thị tần số tốt nhất có thể đạt được cho mỗi chữ cái cho đến nút đó. Một cạm bẫy triển khai phổ biến là quên khởi tạo`dp[i][s[i]] = 1`, điều này sẽ coi các nút bắt đầu không chính xác là không đóng góp lần nào cho chữ cái của chính chúng. 

Một điểm tinh tế khác là phát hiện chu kỳ: chỉ kiểm tra xem một nút có được truy cập lại hay không là chưa đủ. Tiêu chí đúng là liệu quá trình xử lý tôpô có hoàn thành tất cả các nút hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
abaca
1 2
1 3
3 4
4 5
```Chúng tôi chỉ theo dõi các trạng thái chính cho chữ cái 'a'. 

| Bước | Nút | dp tại nút (a) | Hành động | 
| --- | --- | --- | --- | 
| ban đầu | - | [1,0,1,1,1] trên mỗi nút | khởi tạo từ nhãn | 
| quá trình 1 | 1 | truyền tới 2,3 | dp[2]=1, dp[3]=2 | 
| quá trình 3 | 3 | dp[3]=2 | tuyên truyền tới 4 | 
| quá trình 4 | 4 | dp[4]=3 | tuyên truyền tới 5 | 
| quá trình 5 | 5 | dp[5]=3 | cuối cùng | 

Đường dẫn tốt nhất tích lũy ba ký tự 'a' dọc theo 1 → 3 → 4 → 5, xác nhận đầu ra 3. 

### Ví dụ 2 

đầu vào:```
3 3
aaa
1 2
2 3
3 1
```| Bước | Trạng thái xếp hàng | đã xử lý | phát hiện chu kỳ | 
| --- | --- | --- | --- | 
| ban đầu | [1,2,3] có thể hình thành chu kỳ | xử lý từng phần | indegree không bao giờ hoàn toàn rõ ràng | 

Sau khi xử lý, không phải tất cả các nút đều được truy cập do chu kỳ 1 → 2 → 3 → 1. Thuật toán trả về -1 ngay lập tức. 

Điều này xác nhận rằng bất kỳ chu kỳ nào có thể đạt được trong biểu đồ đều dẫn đến giá trị không giới hạn khi các chữ cái có thể được tích lũy vô thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · (n + m)) | Mỗi cạnh thư giãn 26 trạng thái chữ cái một lần | 
| Không gian | O(26 · n) | Bảng DP cho từng nút và chữ cái | 

Với n, m lên đến 300.000, điều này diễn ra thoải mái trong giới hạn vì hệ số không đổi 26 có thể quản lý được và tất cả các hoạt động đều là quét tuyến tính trên các cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, m = map(int, input().split())
    s = input().strip()

    g = [[] for _ in range(n)]
    indeg = [0] * n

    for _ in range(m):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        g[x].append(y)
        indeg[y] += 1

    dp = [[0] * 26 for _ in range(n)]
    for i in range(n):
        dp[i][ord(s[i]) - 97] = 1

    q = deque([i for i in range(n) if indeg[i] == 0])
    visited = 0

    while q:
        u = q.popleft()
        visited += 1
        for v in g[u]:
            for c in range(26):
                val = dp[u][c] + (1 if ord(s[v]) - 97 == c else 0)
                if val > dp[v][c]:
                    dp[v][c] = val
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    if visited < n:
        return "-1"

    return str(max(max(row) for row in dp))

# provided sample
assert run("""5 4
abaca
1 2
1 3
3 4
4 5
""") == "3"

# single node
assert run("""1 0
a
""") == "1"

# simple chain
assert run("""3 2
abc
1 2
2 3
""") == "1"

# all same letters no cycle
assert run("""4 3
aaaa
1 2
2 3
3 4
""") == "4"

# cycle case
assert run("""3 3
abc
1 2
2 3
3 1
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | khởi tạo trường hợp cơ sở | 
| đồ thị chuỗi | 1 | Độ chính xác của việc truyền DP | 
| tất cả các chữ cái giống nhau DAG | 4 | tích lũy trên đường đi | 
| đồ thị chu trình | -1 | phát hiện chu kỳ | 

## Vỏ cạnh 

Biểu đồ nút tối thiểu kiểm tra hành vi khởi tạo. Với một nút duy nhất được gắn nhãn 'a', không có cạnh nào, do đó DP sẽ trực tiếp trả về 1. Thuật toán khởi tạo dp[0][a] = 1 và xử lý nó ngay lập tức, tạo ra kết quả chính xác. 

Biểu đồ tuần hoàn đầy đủ kiểm tra khả năng phát hiện tăng trưởng không giới hạn. Trong chu trình tam giác 1 → 2 → 3 → 1, độ không bao giờ giảm xuống 0 đối với tất cả các nút, do đó thuật toán của Kahn xử lý ít hơn n nút. Bộ đếm đã truy cập phát hiện điều này và trả về -1, báo hiệu chính xác tiềm năng tích lũy vô hạn.
