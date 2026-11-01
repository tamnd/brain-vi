---
title: "CF 106433F - Vận hành đường dây bị hỏng"
description: "Mạng lưới là một cây đại lý. Chính xác k tác nhân phải được đánh dấu là đang hoạt động. Mỗi kênh liên lạc có hai điểm cuối nhận được các trạng thái khác nhau, một hoạt động và một không hoạt động, sẽ đóng góp một điểm."
date: "2026-06-25T09:38:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106433
codeforces_index: "F"
codeforces_contest_name: "XXX Spain Olympiad in Informatics, online qualifier"
rating: 0
weight: 106433
solve_time_s: 39
verified: true
draft: false
---

[CF 106433F - Vận hành đường dây bị hỏng](https://codeforces.com/problemset/problem/106433/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mạng lưới là một cây đại lý. Chính xác`k`đại lý phải được đánh dấu là đang hoạt động. Mỗi kênh liên lạc có hai điểm cuối nhận được các trạng thái khác nhau, một hoạt động và một không hoạt động, sẽ đóng góp một điểm. Mục tiêu là chọn các tác nhân hoạt động sao cho số lượng kênh giao nhau như vậy càng lớn càng tốt. 

Bởi vì mạng là một cái cây nên mỗi cặp tác nhân có một đường dẫn duy nhất giữa chúng. Thuộc tính này loại bỏ các chu kỳ và làm cho cách tiếp cận lập trình động cây con trở nên tự nhiên. 

Ràng buộc`n <= 2000`là manh mối chính. Một nghiệm bậc hai là thực tế vì tổng số đỉnh trong tất cả các trường hợp thử nghiệm cũng chỉ là 2000. Một nghiệm xung quanh`O(n^2)`mỗi trường hợp thử nghiệm là an toàn, trong khi các giải pháp liệt kê các tập hợp con hoặc thử tất cả các lựa chọn của`k`các đỉnh là không thể vì số lượng các lựa chọn có thể tăng theo cấp số nhân. 

Các trường hợp cạnh quan trọng đến từ giá trị chính xác của`k`và từ cấu trúc của cây. 

Vì`k = 0`, không có tác nhân nào hoạt động nên không có cạnh nào có thể kết nối các nhóm khác nhau. Ví dụ:```
Input
1
2 0
1 2
```Câu trả lời là:```
0
```Một giải pháp bất cẩn giả sử có ít nhất một đỉnh hoạt động có thể truy cập vào các trạng thái không hợp lệ hoặc đếm cạnh không chính xác. 

Vì`k = n`, mọi tác nhân đều hoạt động nên một lần nữa không có cạnh giao nhau. Ví dụ:```
Input
1
3 3
1 2
2 3
```Câu trả lời là:```
0
```Phương pháp tham lam cố gắng tối đa hóa mức độ của các đỉnh được chọn có thể thất bại vì việc chọn tất cả các đỉnh sẽ loại bỏ mọi ranh giới có thể có. 

Một chuỗi cũng hoạt động khác với một ngôi sao. Coi như:```
Input
1
5 2
1 2
1 3
1 4
1 5
```Câu trả lời là:```
3
```Việc chọn tâm và một lá chỉ cho ba cạnh giao nhau. Việc chọn hai lá chỉ cho ra hai cạnh giao nhau. Một chiến lược chỉ dựa trên việc chọn các đỉnh có bậc cao có thể bỏ qua nhu cầu xem xét các đỉnh được chọn tương tác với các đỉnh con của chúng như thế nào. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi nhóm có thể`k`các tác nhân hoạt động, đếm các cạnh giao nhau giữa các nhóm hoạt động và không hoạt động và giữ mức tối đa. Điều này đúng vì mọi lựa chọn hợp lệ đều được kiểm tra. Tuy nhiên, số lượng lựa chọn`C(n, k)`, trở nên rất lớn ngay cả đối với mức độ vừa phải`n`. Với`n = 2000`, việc liệt kê các tập con là hoàn toàn không thể. 

Cấu trúc cây cho chúng ta cách xây dựng câu trả lời thay vì phải đoán. Khi chúng ta lấy rễ cây, mỗi cạnh sẽ nối một đỉnh với một trong các đỉnh con của nó. Việc cạnh đó có đóng góp hay không chỉ phụ thuộc vào trạng thái của hai điểm cuối. Điều này có nghĩa là mỗi cây con có thể tóm tắt tất cả các lựa chọn có thể có bên trong nó. 

Thông tin còn thiếu trong cây con một chiều DP là liệu gốc của cây con đó có hoạt động hay không. Cây con chứa các đỉnh hoạt động không nhất thiết phải làm cho cạnh của cây mẹ trở thành cạnh cắt nhau, vì bản thân cây con đó có thể vẫn không hoạt động. Chúng tôi giải quyết vấn đề này bằng cách giữ hai trạng thái: một trạng thái mà đỉnh hiện tại đang hoạt động và một trạng thái là đỉnh không hoạt động. 

Lực lượng vũ phu hoạt động vì nó kiểm tra tất cả các bộ hoạt động có thể có, nhưng không thành công vì có quá nhiều. Quan sát cho thấy mọi đóng góp của cạnh chỉ phụ thuộc vào hai điểm cuối cho phép chúng tôi kết hợp các câu trả lời tối ưu từ các cây con nhỏ hơn bằng cách hợp nhất kiểu ba lô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n,k) * n) | O(n) | Quá chậm | 
| Cây DP | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây ở một đỉnh tùy ý, chẳng hạn như đỉnh`1`và chạy tìm kiếm theo chiều sâu đầu tiên. Lệnh DFS cho phép chúng tôi xử lý con trước cha mẹ của chúng. 
2. Với mọi đỉnh`v`, duy trì hai mảng.`active[v][i]`là số cạnh giao nhau tối đa bên trong cây con của`v`chính xác khi nào`i`các đỉnh đang hoạt động và`v`bản thân nó đang hoạt động.`inactive[v][i]`có ý nghĩa tương tự khi`v`bản thân nó không hoạt động. 
3. Khởi tạo mỗi đỉnh với hai khả năng. Nếu như`v`đang hoạt động, cây con hiện chứa một đỉnh đang hoạt động và không có cạnh giao nhau nào. Nếu như`v`không hoạt động, nó không chứa các đỉnh hoạt động và cũng không có cạnh giao nhau. 
4. Hợp nhất mọi cây con con vào đỉnh hiện tại bằng cách sử dụng chuyển tiếp ba lô. Giả sử một cây con đóng góp`j`các đỉnh hoạt động Nếu như`v`đang hoạt động, việc chọn một root con đang hoạt động sẽ sử dụng`active[child][j]`, trong khi chọn một root con không hoạt động sẽ sử dụng`inactive[child][j] + 1`bởi vì cạnh giữa`v`và đứa trẻ vượt qua vách ngăn. 
5. Thực hiện chuyển đổi đối xứng khi`v`không hoạt động. Việc chọn một gốc con đang hoạt động sẽ thêm một cạnh giao nhau, trong khi việc chọn một gốc con không hoạt động thì không. 
6. Sau khi tất cả các phần tử con đã được hợp nhất, câu trả lời là tốt hơn hết là kích hoạt root bằng`k`các đỉnh hoạt động hoặc làm cho nó không hoạt động với`k`các đỉnh hoạt động 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý một đỉnh, mỗi trạng thái DP sẽ lưu trữ số cạnh giao nhau tốt nhất có thể có bên trong cây con đó cho mọi số đỉnh hoạt động có thể có trong khi vẫn tôn trọng trạng thái của gốc cây con. Mỗi cạnh được xem xét chính xác một lần khi cạnh cha và con của nó được hợp nhất. Tại thời điểm đó, trạng thái DP biết trạng thái của cả hai điểm cuối, do đó cạnh được tính chính xác khi nó đi qua hoặc bị bỏ qua khi không đi qua. Vì mọi kết hợp cây con đều được khám phá thông qua việc hợp nhất ba lô, nên trạng thái gốc cuối cùng thể hiện sự lựa chọn chính xác nhất có thể của`k`các đỉnh hoạt động 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, k, edges):
    graph = [[] for _ in range(n)]
    for u, v in edges:
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    active = [None] * n
    inactive = [None] * n
    size = [0] * n

    def dfs(v, p):
        size[v] = 1
        active[v] = [-10**9] * 2
        inactive[v] = [-10**9] * 2

        active[v][1] = 0
        inactive[v][0] = 0

        for u in graph[v]:
            if u == p:
                continue
            dfs(u, v)

            new_active = [-10**9] * (size[v] + size[u] + 1)
            new_inactive = [-10**9] * (size[v] + size[u] + 1)

            for i in range(size[v] + 1):
                if active[v][i] > -10**9:
                    for j in range(size[u] + 1):
                        if active[u][j] > -10**9:
                            new_active[i + j] = max(
                                new_active[i + j],
                                active[v][i] + active[u][j]
                            )
                        if inactive[u][j] > -10**9:
                            new_active[i + j] = max(
                                new_active[i + j],
                                active[v][i] + inactive[u][j] + 1
                            )

                if inactive[v][i] > -10**9:
                    for j in range(size[u] + 1):
                        if active[u][j] > -10**9:
                            new_inactive[i + j] = max(
                                new_inactive[i + j],
                                inactive[v][i] + active[u][j] + 1
                            )
                        if inactive[u][j] > -10**9:
                            new_inactive[i + j] = max(
                                new_inactive[i + j],
                                inactive[v][i] + inactive[u][j]
                            )

            size[v] += size[u]
            active[v] = new_active[:size[v] + 1]
            inactive[v] = new_inactive[:size[v] + 1]

    dfs(0, -1)
    return max(active[0][k], inactive[0][k])

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        n, k = map(int, input().split())
        edges = [tuple(map(int, input().split())) for _ in range(n - 1)]
        ans.append(str(solve_case(n, k, edges)))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Danh sách kề lưu trữ cây vì mỗi đỉnh có thể có nhiều lân cận và đầu vào chỉ cung cấp các cạnh. DFS chuyển đổi cây vô hướng thành cây con có gốc. 

Hai mảng DP là chi tiết triển khai trung tâm.`active[v][i]`Và`inactive[v][i]`có ý nghĩa khác nhau, điều này tránh được lỗi thường gặp khi đếm cạnh khi cây con con chứa các đỉnh hoạt động nhưng bản thân cây con đó không hoạt động. 

Trong quá trình hợp nhất, các vòng lặp thử mọi khả năng phân chia các đỉnh hoạt động giữa phần đã được xử lý của`v`và cây con con mới. Quá trình chuyển đổi chỉ thêm một khi trạng thái điểm cuối khác nhau. Các trạng thái ban đầu xử lý các trường hợp biên`0`Và`1`đỉnh hoạt động bên trong cây con. 

Số nguyên Python không bị tràn, nhưng vẫn cần giá trị trọng điểm âm để biểu thị các trạng thái không thể. Câu trả lời luôn được tìm thấy ở chỉ mục`k`, vì bài toán yêu cầu chính xác số đỉnh hoạt động đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
3 2
1 2
2 3
5 2
1 2
1 3
1 4
1 5
2 0
1 2
```Đối với cây đầu tiên: 

| Đỉnh | Tiểu bang | Số lượng hoạt động | Các cạnh chéo tốt nhất | 
| --- | --- | --- | --- | 
| 1 | không hoạt động | 2 | 2 | 
| 1 | hoạt động | 2 | 1 | 
| Trả lời | | | 2 | 

Sự lựa chọn tối ưu sẽ kích hoạt hai điểm cuối của chuỗi. Cả hai cạnh đều vượt qua phân vùng. 

Đối với cây thứ hai: 

| Đỉnh | Tiểu bang | Số lượng hoạt động | Các cạnh chéo tốt nhất | 
| --- | --- | --- | --- | 
| 1 | hoạt động | 2 | 3 | 
| 1 | không hoạt động | 2 | 2 | 
| Trả lời | | | 3 | 

Kích hoạt tâm và một lá sẽ tạo ra ba cạnh giao nhau từ các lá còn lại. 

### Mẫu 2 

đầu vào:```
1
4 1
1 2
2 3
3 4
```| Đỉnh | Tiểu bang | Số lượng hoạt động | Các cạnh chéo tốt nhất | 
| --- | --- | --- | --- | 
| 1 | hoạt động | 1 | 1 | 
| 2 | hoạt động | 1 | 2 | 
| 3 | hoạt động | 1 | 2 | 
| 4 | hoạt động | 1 | 1 | 
| Trả lời | | | 2 | 

Đỉnh hoạt động đơn lẻ tốt nhất là một trong các đỉnh ở giữa vì nó có hai cạnh liên quan có thể cắt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi cạnh cây thực hiện việc hợp nhất ba lô trên số lượng hoạt động có thể có. | 
| Không gian | O(n^2) | Mảng DP được lưu trữ trên tất cả các đỉnh chứa thông tin tổng bậc hai trong trường hợp xấu nhất. | 

Tổng số tối đa`n`trong mọi trường hợp là 2000, do đó thuật toán bậc hai phù hợp thoải mái trong giới hạn. Việc triển khai chỉ lưu trữ các mảng cho cây hiện tại, giúp việc sử dụng bộ nhớ trở nên thiết thực. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    t = int(next(it))
    out = []

    def solve_case(n, k, edges):
        graph = [[] for _ in range(n)]
        for u, v in edges:
            u -= 1
            v -= 1
            graph[u].append(v)
            graph[v].append(u)

        A = [None] * n
        B = [None] * n
        sz = [0] * n

        def dfs(v, p):
            sz[v] = 1
            A[v] = [-10**9, 0]
            B[v] = [0, -10**9]

            for u in graph[v]:
                if u == p:
                    continue
                dfs(u, v)
                na = [-10**9] * (sz[v] + sz[u] + 1)
                nb = [-10**9] * (sz[v] + sz[u] + 1)
                for i in range(sz[v] + 1):
                    for j in range(sz[u] + 1):
                        if i < len(A[v]) and j < len(A[u]):
                            na[i+j] = max(na[i+j], A[v][i] + A[u][j])
                        if i < len(A[v]) and j < len(B[u]):
                            na[i+j] = max(na[i+j], A[v][i] + B[u][j] + 1)
                        if i < len(B[v]) and j < len(A[u]):
                            nb[i+j] = max(nb[i+j], B[v][i] + A[u][j] + 1)
                        if i < len(B[v]) and j < len(B[u]):
                            nb[i+j] = max(nb[i+j], B[v][i] + B[u][j])
                sz[v] += sz[u]
                A[v] = na[:sz[v]+1]
                B[v] = nb[:sz[v]+1]

        dfs(0, -1)
        return max(A[0][k], B[0][k])

    for _ in range(t):
        n = int(next(it))
        k = int(next(it))
        edges = []
        for _ in range(n - 1):
            edges.append((int(next(it)), int(next(it))))
        out.append(str(solve_case(n, k, edges)))

    return "\n".join(out)

assert run("""3
3 2
1 2
2 3
5 2
1 2
1 3
1 4
1 5
2 0
1 2
""") == "2\n3\n0", "samples"

assert run("""1
2 1
1 2
""") == "1", "minimum tree"

assert run("""1
5 5
1 2
2 3
3 4
4 5
""") == "0", "all active"

assert run("""1
5 0
1 2
1 3
1 4
1 5
""") == "0", "none active"

assert run("""1
4 1
1 2
2 3
3 4
""") == "2", "middle vertex choice"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba trường hợp mẫu |`2, 3, 0`| Tính đúng đắn cơ bản và ranh giới`k`giá trị | 
| Hai đỉnh với một hoạt động |`1`| Cây nhỏ nhất có thể | 
| Toàn bộ cây đang hoạt động |`0`| Không có cạnh giao nhau khi tất cả các đỉnh có chung một trạng thái | 
| Không có đỉnh hoạt động |`0`| Xử lý tập hợp đã chọn trống | 
| Chuỗi có một đỉnh hoạt động |`2`| Chọn một đỉnh bên trong thay vì một chiếc lá | 

## Vỏ cạnh 

Khi nào`k = 0`, mọi trạng thái DP ngoại trừ trạng thái không hoạt động hoàn toàn đều không thể thực hiện được ở gốc. Vì:```
1
2 0
1 2
```DFS bắt đầu với mỗi lá chỉ cho phép không có đỉnh hoạt động nào thông qua trạng thái không hoạt động của nó. Việc hợp nhất không bao giờ tạo ra ranh giới vì cả hai điểm cuối vẫn không hoạt động, đưa ra câu trả lời`0`. 

Khi`k = n`, mọi đỉnh đều phải hoạt động. DP chỉ có thể sử dụng các trạng thái hoạt động xuyên suốt cây. Vì mỗi cạnh đều có hai điểm cuối hoạt động nên không có chuyển đổi nào bổ sung thêm đóng góp giao nhau. Vì:```
1
3 3
1 2
2 3
```câu trả lời vẫn còn`0`. 

Đối với cây hình ngôi sao:```
1
5 2
1 2
1 3
1 4
1 5
```gốc có bốn con. DP so sánh việc chọn trung tâm bằng một lá với việc chọn hai lá. Việc lựa chọn ở giữa tạo ra ba cạnh con với các trạng thái điểm cuối khác nhau, do đó đạt được câu trả lời tối ưu`3`. 

Đối với một chuỗi:```
1
4 1
1 2
2 3
3 4
```DP giữ cả hai khả năng trạng thái gốc cho mọi cây con. Nó phát hiện ra rằng việc chọn đỉnh`2`hoặc`3`cho hai cạnh giao nhau, trong khi việc chọn điểm cuối chỉ cho một. Đây chính xác là trường hợp mà sự lựa chọn tham lam dựa trên mức độ địa phương là không đủ.
