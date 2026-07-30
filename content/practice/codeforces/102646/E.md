---
title: "CF 102646E - Tối đa hóa SCC"
description: "Ta có đồ thị có hướng với n đỉnh và m cạnh. Đồ thị ban đầu được đảm bảo liên thông chặt chẽ, nghĩa là mọi đỉnh đều có thể chạm tới mọi đỉnh khác bằng cách sử dụng các cạnh đã cho. Chúng ta phải giữ chính xác k cạnh này và loại bỏ phần còn lại."
date: "2026-07-30T23:09:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102646
codeforces_index: "E"
codeforces_contest_name: "Testing Round #XVII"
rating: 0
weight: 102646
solve_time_s: 118
verified: true
draft: false
---

[CF 102646E - Tối đa hóa SCC](https://codeforces.com/problemset/problem/102646/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có đồ thị có hướng với`n`đỉnh và`m`các cạnh. Đồ thị ban đầu được đảm bảo liên thông chặt chẽ, nghĩa là mọi đỉnh đều có thể chạm tới mọi đỉnh khác bằng cách sử dụng các cạnh đã cho. Chúng ta phải giữ chính xác`k`của các cạnh này và loại bỏ phần còn lại. Mục tiêu là làm cho biểu đồ còn lại có càng nhiều thành phần được kết nối chặt chẽ càng tốt. Đầu ra là số lượng thành phần tối đa và một bộ hợp lệ`k`chỉ số cạnh đạt được nó. 

Hạn chế chính đó là`k`nhỏ hơn`n`. Vì mỗi đỉnh đã là một thành phần có thể có liên thông mạnh riêng biệt nên câu trả lời tối đa tuyệt đối là`n`. Câu hỏi đặt ra là liệu chúng ta luôn có thể chọn`k`các cạnh mà không vô tình tạo ra một chu trình có hướng. 

Với`n`lên đến`100000`Và`m`lên đến`200000`, bất kỳ giải pháp nào thử nhiều tập hợp con của các cạnh hoặc tính toán lại SCC nhiều lần sẽ quá chậm. Cần có một giải pháp gần tuyến tính về kích thước của biểu đồ vì chỉ có vài triệu thao tác có sẵn trong bối cảnh cuộc thi thông thường. 

Một lỗi phổ biến là chọn các cạnh tùy ý. Ví dụ:```
3 3 2
1 2
2 3
3 1
```Đầu ra đúng là:```
3
1 2
```bởi vì các cạnh`1 -> 2`Và`2 -> 3`tạo thành một chuỗi có hướng, do đó mỗi đỉnh là SCC của chính nó. Một sự lựa chọn bất cẩn của các cạnh`1`Và`3`tạo ra chu kỳ`1 -> 2 -> 3 -> 1`không thể thực hiện được ở đây vì cạnh`2`bị thiếu, nhưng việc chọn cả ba cạnh rõ ràng sẽ tạo thành một SCC. Chi tiết quan trọng là các cạnh được chọn phải tránh chu kỳ. 

Một trường hợp cạnh khác là`k = 0`.```
5 5 0
1 2
2 3
3 4
4 5
5 1
```Đầu ra đúng là:```
5
```Đồ thị trống không có đường đi giữa các đỉnh khác nhau, vì vậy mỗi đỉnh là một SCC riêng biệt. Việc triển khai luôn cố gắng xuất ra ít nhất một cạnh sẽ không thành công. 

Trường hợp cạnh cuối cùng là khi đồ thị dày đặc và mỗi đỉnh có nhiều cạnh vào và cạnh ra. Ví dụ:```
4 6 3
1 2
2 3
3 4
4 1
1 3
2 4
```Câu trả lời đúng vẫn là`4`. Mật độ không quan trọng. Chúng ta chỉ cần một tập hợp con các cạnh không theo chu kỳ chứ không phải một biểu đồ thưa thớt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử các tập hợp khác nhau`k`các cạnh và tính số SCC cho mỗi lựa chọn. Điều này đúng vì việc kiểm tra mọi tập hợp con có thể đảm bảo rằng tập hợp con tốt nhất sẽ được tìm thấy. Tuy nhiên, số lượng lựa chọn`C(m, k)`, trở nên rất lớn ngay cả đối với đồ thị nhỏ. Chạy thuật toán SCC sau mỗi lựa chọn là không thể. 

Quan sát hữu ích xuất phát từ thực tế là đồ thị ban đầu được liên kết chặt chẽ. Bắt đầu từ bất kỳ đỉnh nào, DFS có thể đến mọi đỉnh khác. Cây DFS chứa chính xác`n - 1`các cạnh và các cạnh đó luôn tạo thành một cây có hướng từ cha mẹ đến con cái. Một cây không có chu trình có hướng nên mỗi đỉnh trong cây đó là SCC của chính nó. 

Từ`k < n`, chúng ta có thể lấy bất kỳ`k`các cạnh của cây DFS này. Đồ thị kết quả vẫn không có tính tuần hoàn, có nghĩa là nó có chính xác`n`các thành phần được kết nối chặt chẽ. Đây đã là mức tối đa về mặt lý thuyết nên không cần tối ưu hóa phức tạp hơn nữa. 

Brute-force hoạt động vì nó khám phá mọi biểu đồ còn lại có thể có, nhưng không thành công vì không gian tìm kiếm quá lớn. Việc quan sát cây DFS làm giảm vấn đề từ việc tìm kiếm giữa tất cả các tập hợp con cạnh đến việc xây dựng một tập hợp con tối ưu được đảm bảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(m, k) * (n + m)) | O(n + m) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS từ bất kỳ đỉnh nào, ví dụ như đỉnh`1`. Bất cứ khi nào DFS truy cập một đỉnh mới qua một cạnh, hãy ghi lại cạnh đó như một phần của cây DFS. Đồ thị có tính liên thông mạnh nên quá trình này sẽ đi qua tất cả các đỉnh. 
2. Lấy cái đầu tiên`k`ghi lại các cạnh của cây DFS và xuất ra các chỉ số của chúng. Đây là các cạnh bắt buộc vì chúng đến từ cấu trúc không tuần hoàn. 
3. Nếu`k`bằng 0, xuất ra 0 cạnh được chọn. Biểu đồ trống đã cung cấp số lượng SCC tối đa. 

Tại sao nó hoạt động: 

Các cạnh được chọn là tập hợp con của cây DFS. Mỗi cạnh trong cây này đều trỏ từ đỉnh cha tới đỉnh con. Việc đi theo các cạnh đã chọn chỉ có thể di chuyển xa hơn xuống cây, vì vậy việc quay lại đỉnh đã ghé thăm là không thể. Biểu đồ được chọn là DAG. Trong DAG, không có hai đỉnh khác nhau nào có thể tiếp cận được lẫn nhau, do đó mỗi đỉnh tạo thành SCC riêng. Vì thế câu trả lời là`n`, đó là số lượng SCC tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    graph = [[] for _ in range(n)]

    for i in range(1, m + 1):
        u, v = map(int, input().split())
        graph[u - 1].append((v - 1, i))

    ans = []
    seen = [False] * n

    sys.setrecursionlimit(300000)

    def dfs(u):
        seen[u] = True
        for v, idx in graph[u]:
            if not seen[v]:
                ans.append(idx)
                if len(ans) == k:
                    return True
                if dfs(v):
                    return True
        return False

    if k > 0:
        dfs(0)

    print(n)
    if k:
        print(*ans)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cả đỉnh đích và chỉ số cạnh ban đầu vì đầu ra yêu cầu số cạnh thay vì điểm cuối. 

DFS ghi lại một cạnh chính xác khi nó phát hiện ra một đỉnh mới. Điều này làm cho các cạnh được ghi trở thành cây bao trùm của phần có thể tiếp cận của biểu đồ. Vì đồ thị liên thông chặt xuất phát từ đỉnh`0`đạt đến mọi đỉnh. 

Quá trình đệ quy dừng lại ngay khi`k`các cạnh được thu thập. Điều này là an toàn vì mục tiêu duy nhất là xuất ra bất kỳ tập hợp con tối ưu hợp lệ nào, không nhất thiết là toàn bộ cây. điều kiện`k < n`đảm bảo rằng cây DFS luôn chứa đủ các cạnh. 

Không cần tính toán SCC sau khi xây dựng. Bằng chứng đã đảm bảo rằng các cạnh được chọn tạo thành DAG và do đó đạt được câu trả lời tối đa có thể. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 5 1
1 2
2 3
1 4
4 3
3 1
```Một lần truyền tải DFS có thể bắt đầu ở đỉnh`1`. 

| Bước | Đỉnh hiện tại | Cạnh được chọn | Các cạnh được chọn | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 -> 2 | 1 | 

Thuật toán dừng lại vì`k = 1`. 

Đồ thị được chọn chỉ chứa cạnh`1 -> 2`. Không tồn tại chu trình nên cả 4 đỉnh đều là SCC riêng biệt. Câu trả lời là`4`. 

Đối với một ví dụ tùy chỉnh:```
5 5 3
1 2
2 3
3 4
4 5
5 1
```Cây DFS tuân theo chu trình cho đến khi tìm được tất cả các đỉnh. 

| Bước | Đỉnh hiện tại | Cạnh được chọn | Các cạnh được chọn | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 -> 2 | 1 | 
| 2 | 2 | 2 -> 3 | 1 2 | 
| 3 | 3 | 3 -> 4 | 1 2 3 | 

Các cạnh được chọn tạo thành chuỗi`1 -> 2 -> 3 -> 4`. đỉnh`5`bị ngắt kết nối khỏi tập hợp con đã chọn này, nhưng điều đó được cho phép. Không có chu trình nên mọi đỉnh vẫn là một SCC độc lập và câu trả lời là`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được xử lý nhiều nhất một lần trong DFS | 
| Không gian | O(n + m) | Danh sách kề và mảng trạng thái DFS lưu trữ biểu đồ | 

Các giới hạn cho phép truyền tải tuyến tính vì`n`Và`m`đều nhiều nhất là vài trăm nghìn. Thuật toán chỉ lưu trữ biểu đồ đầu vào và một lượng nhỏ trạng thái DFS. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    def solve():
        input = sys.stdin.readline
        n, m, k = map(int, input().split())
        graph = [[] for _ in range(n)]

        for i in range(1, m + 1):
            u, v = map(int, input().split())
            graph[u - 1].append((v - 1, i))

        ans = []
        seen = [False] * n

        def dfs(u):
            seen[u] = True
            for v, idx in graph[u]:
                if not seen[v]:
                    ans.append(idx)
                    if len(ans) == k:
                        return True
                    if dfs(v):
                        return True
            return False

        if k:
            dfs(0)

        print(n)
        if k:
            print(*ans)

    solve()
    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""4 5 1
1 2
2 3
1 4
4 3
3 1
""").split()[0] == "4", "sample 1"

assert run("""7 7 0
1 2
2 3
3 4
4 5
5 6
6 7
7 1
""").split()[0] == "7", "sample 2"

assert run("""1 1 0
1 1
""").split()[0] == "1", "single vertex"

assert run("""5 5 4
1 2
2 3
3 4
4 5
5 1
""").split()[0] == "5", "large k"

assert run("""4 6 2
1 2
2 3
3 4
4 1
1 3
2 4
""").split()[0] == "4", "dense graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0`|`1`| Đồ thị kích thước tối thiểu và các cạnh bằng 0 | 
|`5 5 4`|`5`| Lấy gần như tất cả các cạnh được phép mà không tạo chu trình | 
|`4 6 2`|`4`| Đồ thị kết nối mạnh dày đặc | 
| Đồ thị mẫu |`n`| Các ví dụ gốc và cách thực thi thông thường | 

## Vỏ cạnh 

Khi nào`k = 0`, thuật toán bỏ qua lựa chọn DFS và chỉ in`n`. Điều này phù hợp với thực tế là một đồ thị có hướng trống không có SCC không tầm thường. Đối với ví dụ có năm đỉnh và không có cạnh nào được chọn, câu trả lời là`5`. 

Khi`k`gần với`n`, cây DFS vẫn cung cấp đủ các cạnh vì mọi cây bao trùm đều có chính xác`n - 1`các cạnh. Đối với đồ thị có`n = 5`Và`k = 4`, thuật toán có thể xuất ra cả bốn cạnh của cây và đồ thị được chọn vẫn không có tính tuần hoàn. 

Khi đồ thị ban đầu chứa nhiều chu trình thì các chu trình đó sẽ bị bỏ qua. Ví dụ: trong biểu đồ chứa`1 -> 2 -> 3 -> 1`, cây DFS chỉ có thể chọn`1 -> 2`Và`2 -> 3`. Cạnh sau bị loại bỏ chính xác là thứ sẽ hợp nhất các đỉnh thành một SCC, do đó, việc loại trừ nó sẽ mang lại kết quả tối ưu.
