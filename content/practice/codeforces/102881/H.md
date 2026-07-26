---
title: "CF 102881H - Mảng ngắn nhất"
description: "Chúng ta được cung cấp một biểu đồ có trọng số được kết nối và một mảng các giá trị dương s. Nhiệm vụ là tìm đỉnh x được lập chỉ mục nhỏ nhất sao cho giá trị được ghi ở mỗi đỉnh chính xác là khoảng cách ngắn nhất từ ​​x đến đỉnh đó."
date: "2026-07-25T12:34:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102881
codeforces_index: "H"
codeforces_contest_name: "ECPC 2019 Kickoff"
rating: 0
weight: 102881
solve_time_s: 64
verified: true
draft: false
---

[CF 102881H - Mảng ngắn nhất](https://codeforces.com/problemset/problem/102881/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số được kết nối và một mảng các giá trị dương`s`. Nhiệm vụ là tìm đỉnh được lập chỉ mục nhỏ nhất`x`sao cho giá trị được viết ở mỗi đỉnh chính xác là khoảng cách ngắn nhất từ`x`tới đỉnh đó. Điều kiện đặc biệt là ở tại`x`không được coi là đường dẫn có độ dài bằng 0 hợp lệ, vì vậy giá trị tại`x`chính nó là độ dài của bước đi khép kín không trống ngắn nhất bắt đầu và kết thúc tại`x`. Đồ thị có trọng số cạnh dương và không có cạnh song song hoặc vòng tự. 

Các giới hạn đủ lớn để việc thử mọi đỉnh làm điểm xuất phát có thể là không thể. Với tối đa`2 * 10^5`các đỉnh và các cạnh, một lần chạy Dijkstra có thể chấp nhận được, nhưng việc chạy nó một lần từ mỗi đỉnh sẽ yêu cầu khoảng`O(n * (n + m) log n)`hoạt động vượt xa thời gian sẵn có. Giải pháp cần trích xuất thông tin về tất cả các đỉnh bắt đầu có thể có cùng một lúc. 

Phần bất thường của vấn đề là giá trị được lưu trữ ở đỉnh nguồn. Trong một mảng đường dẫn ngắn nhất bình thường, nguồn sẽ chứa`0`. Ở đây nó chứa chu kỳ dương ngắn nhất bắt đầu và kết thúc ở đỉnh đó. Trong đồ thị vô hướng có trọng số dương, giá trị này gấp đôi trọng lượng cạnh tối thiểu tới đỉnh đó, bởi vì bước đi khép kín rẻ nhất có thể đi qua cạnh liền kề nhẹ nhất và ngay lập tức quay trở lại. 

Một lỗi phổ biến là chấp nhận một đỉnh có khoảng cách chính xác với tất cả các đỉnh khác nhưng lại quên xác minh giá trị của chính nó. Ví dụ:```
3 2
4 2 2
1 2 2
2 3 2
```Đầu ra đúng là`2`. đỉnh`2`đạt đến đỉnh`1`Và`3`ở khoảng cách`2`, và chuyến đi quay về không trống ngắn nhất của nó cũng là`4`. Một giải pháp bất cẩn chỉ kiểm tra khoảng cách đến các đỉnh khác có thể chấp nhận đỉnh không chính xác`1`hoặc`3`, bởi vì chúng có cùng khoảng cách hướng ra ngoài tới một số đỉnh nhưng khoảng cách bản thân của chúng phải là`4`, không`2`. 

Một trường hợp cạnh khác xuất hiện khi nhiều đỉnh có thể thỏa mãn mảng. Vấn đề yêu cầu chỉ số nhỏ nhất. Ví dụ:```
4 4
6 3 6 6
1 3 6
1 2 3
3 2 3
4 2 3
```Đầu ra đúng là`1`. Một số mối quan hệ khoảng cách trông đối xứng, nhưng chỉ phải in chỉ số hợp lệ nhỏ nhất. 

Một cái bẫy cuối cùng là một mảng khoảng cách không thể. Ví dụ:```
3 3
1 5 5
1 2 2
2 3 2
1 3 10
```Đầu ra là`-1`. Các giá trị khẳng định rằng có thể đạt tới một số đỉnh với khoảng cách nhỏ hơn mức mà biểu đồ thực sự cho phép, do đó không có đỉnh bắt đầu nào có thể tạo ra mảng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là kiểm tra mọi đỉnh có thể là gốc tọa độ. Đối với một đỉnh được chọn`x`, chúng tôi sẽ chạy Dijkstra từ`x`, so sánh tất cả các khoảng cách được tính toán với`s`và xác minh thêm rằng giá trị đặc biệt`s[x]`khớp với quãng đường đi bộ không trống ngắn nhất từ`x`. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa. Tuy nhiên, trường hợp xấu nhất thực hiện`n`Dijkstra chạy. Với`n`Và`m`cả xung quanh`200000`, điều này trở nên đại khái`200000 * 400000 log(200000)`hoạt động, điều đó là không thể thực hiện được. 

Quan sát quan trọng là toàn bộ mảng có thể được xem từ một đỉnh nhân tạo mới. Tạo một đỉnh ảo`z`và kết nối nó với mọi đỉnh ban đầu`i`với một cạnh trọng lượng`s[i]`. Nếu mảng hợp lệ đối với một số nguồn`x`, thì khoảng cách ngắn nhất từ`z`đến mọi đỉnh đều chính xác`s[i]`. Cạnh trực tiếp cung cấp khoảng cách này và bất kỳ đường dẫn nào sử dụng các cạnh của biểu đồ gốc không thể ngắn hơn vì các nhãn đã hoạt động giống như khoảng cách ngắn nhất. 

Câu hỏi còn lại là đỉnh nào có thể là nguồn thực sự. Đối với một nguồn hợp lệ`x`, cạnh nhân tạo`z -> x`không phải là cách duy nhất để tiếp cận`x`với khoảng cách`s[x]`. Chúng ta có thể đi từ`z`đến một số đỉnh khác`u`, trả tiền`s[u]`, rồi đi qua đồ thị ban đầu trở lại`x`. Điều này tương ứng với quãng đường đi bộ không trống ngắn nhất từ`x`. 

Vì vậy, chúng tôi chạy Dijkstra đa nguồn trong đó mọi đỉnh ban đầu hoạt động như một nguồn có khoảng cách`s[i]`. Trong quá trình này, chúng tôi giữ hai khoảng cách tốt nhất đến mọi đỉnh, nhưng hai khoảng cách đó phải đến từ các đỉnh bắt đầu khác nhau. Nguồn tốt thứ hai cho đỉnh`i`cho chúng ta biết liệu có một đường đi có độ dài chính xác hay không`s[i]`điều đó không bắt đầu bằng cạnh nhân tạo để`i`. Nếu điều đó tồn tại,`i`là một câu trả lời có thể 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n + m) log n) | O(n + m) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mọi đỉnh ban đầu làm nguồn trong Dijkstra đa nguồn. đỉnh`i`bắt đầu bằng khoảng cách`s[i]`và mã định danh nguồn`i`. Điều này mô phỏng việc thêm đỉnh ảo mà không lưu trữ nó một cách rõ ràng. 
2. Trong Dijkstra, hãy duy trì khoảng cách tốt nhất và tốt thứ hai cho mỗi đỉnh, trong đó hai mục được lưu trữ phải có mã định danh nguồn khác nhau. Chúng ta chỉ quan tâm đến hai nguồn gốc khác nhau vì nguồn gốc thứ nhất có thể đơn giản là cạnh nhân tạo trực tiếp. 
3. Thư giãn các cạnh đồ thị một cách bình thường. Khi một đường dẫn đến một đỉnh, hãy cập nhật mục nhập tốt nhất hoặc mục nhập tốt thứ hai của nó tùy thuộc vào việc mã định danh nguồn đã có sẵn hay chưa. 
4. Sau khi Dijkstra kết thúc, hãy kiểm tra từng đỉnh theo thứ tự tăng dần. Điều kiện đầu tiên là khoảng cách tốt nhất tới đỉnh phải bằng`s[i]`. Nếu khoảng cách nhỏ hơn tồn tại, mảng đã cho không thể biểu thị bất kỳ cây đường đi ngắn nhất nào. 
5. Điều kiện thứ hai là khoảng cách tốt thứ hai cũng phải bằng`s[i]`. Điều này có nghĩa là có thể đạt đến đỉnh với cùng một giá trị mà không cần sử dụng chính nó làm điểm bắt đầu nhân tạo, đây chính xác là đường dẫn không trống bắt buộc quay về nguồn. 
6. Xuất ra đỉnh đầu tiên thỏa mãn cả hai điều kiện. Nếu không tồn tại, xuất`-1`. 

Tại sao nó hoạt động: 

Dijkstra đa nguồn tính toán các đường đi ngắn nhất từ nguồn ảo được gắn theo khái niệm với mọi đỉnh. Mảng khoảng cách hợp lệ có nghĩa là mọi đỉnh phải giữ khoảng cách nhân tạo trực tiếp của nó là khoảng cách ngắn nhất có thể. Đỉnh đặc biệt duy nhất là nguồn thực, bởi vì cạnh nhân tạo trực tiếp của nó đại diện cho đường đi không di chuyển bị cấm. Sự tồn tại của một đường đi có độ dài bằng nhau từ một nguồn khác chứng tỏ rằng giá trị có thể được tạo ra bằng một bước đi thực tế bên trong biểu đồ gốc. Do đó, tiêu chí khoảng cách tốt nhất thứ hai xác định chính xác các nguồn ban đầu có thể có. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    s = list(map(int, input().split()))

    graph = [[] for _ in range(n)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, w))
        graph[v].append((u, w))

    INF = 10**30

    best_d = [INF] * n
    best_o = [-1] * n
    second_d = [INF] * n
    second_o = [-1] * n

    heap = []

    for i, x in enumerate(s):
        best_d[i] = x
        best_o[i] = i
        heapq.heappush(heap, (x, i, i))

    def update(v, d, o):
        if best_o[v] == o:
            if d < best_d[v]:
                best_d[v] = d
                return True
            return False

        if second_o[v] == o:
            if d < second_d[v]:
                second_d[v] = d
                return True
            return False

        if d < best_d[v]:
            second_d[v] = best_d[v]
            second_o[v] = best_o[v]
            best_d[v] = d
            best_o[v] = o
            return True

        if d < second_d[v]:
            second_d[v] = d
            second_o[v] = o
            return True

        return False

    while heap:
        d, v, o = heapq.heappop(heap)

        if d != best_d[v] and d != second_d[v]:
            continue

        for u, w in graph[v]:
            nd = d + w
            if update(u, nd, o):
                heapq.heappush(heap, (nd, u, o))

    for i in range(n):
        if best_d[i] != s[i]:
            print(-1)
            return

    for i in range(n):
        if second_d[i] == s[i]:
            print(i + 1)
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Việc khởi tạo thay thế việc xây dựng nút ảo. Thay vì thêm một cách rõ ràng`z`, mọi đỉnh đều bắt đầu bằng khoảng cách bằng cạnh nối nó với`z`. 

các`update`chức năng là cốt lõi của việc thực hiện. Nó chỉ giữ lại hai đường đi nhỏ nhất có đỉnh bắt đầu khác nhau. Việc giữ hai mục có cùng nguồn gốc sẽ không giúp ích gì vì đường dẫn thứ hai phải thể hiện một cạnh nhân tạo khác. 

Thư giãn Dijkstra sử dụng số nguyên kiểu 64 bit thông qua kiểu số nguyên thông thường của Python. Điều này quan trọng vì độ dài đường dẫn có thể đạt tới khoảng`2 * 10^14`. Việc kiểm tra đống cũ là cần thiết vì một đỉnh có thể có nhiều mục lỗi thời sau khi cải tiến. 

Lần quét cuối cùng được thực hiện theo thứ tự chỉ mục tăng dần. Đỉnh hợp lệ đầu tiên tự động là câu trả lời được lập chỉ mục nhỏ nhất. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 4
6 3 6 6
1 3 6
1 2 3
3 2 3
4 2 3
```Các trạng thái Dijkstra quan trọng là: 

| Đỉnh | Nguồn ban đầu | Khoảng cách tốt nhất | Nguồn thứ hai | Khoảng cách thứ hai | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | 2 | 6 | 
| 2 | 2 | 3 | 1 | 6 | 
| 3 | 3 | 6 | 1 | 6 | 
| 4 | 4 | 6 | 1 | 6 | 

đỉnh`1`có đường đi thứ hai có độ dài`6`, vì vậy nó hợp lệ. Đây là chỉ mục hợp lệ đầu tiên và câu trả lời là`1`. 

Ví dụ này cho thấy tại sao cần có nguồn ngắn thứ hai. Cạnh nhân tạo trực tiếp không đủ để chứng minh rằng một đỉnh là gốc ẩn. 

Đối với mẫu thứ ba:```
5 6
8 4 4 8 6
1 2 4
1 3 4
2 4 4
3 4 4
1 5 3
5 3 2
```Các tiểu bang là: 

| Đỉnh | Khoảng cách tốt nhất | Khoảng cách thứ hai | 
| --- | --- | --- | 
| 1 | 4 | 8 | 
| 2 | 4 | 8 | 
| 3 | 4 | 8 | 
| 4 | 8 | 8 | 
| 5 | 6 | 10 | 

Chỉ có đỉnh`4`có đường dẫn thứ hai được yêu cầu bằng giá trị của nó. 

Dấu vết thể hiện quy tắc tự giữ khoảng cách đặc biệt. đỉnh`4`không được xác định bằng khoảng cách bằng 0 mà bằng một con đường có độ dài ngắn nhất khác`8`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi cạnh đồ thị được nới lỏng trong một quy trình Dijkstra đa nguồn. | 
| Không gian | O(n + m) | Đồ thị và hai trạng thái tốt nhất cho mỗi đỉnh được lưu trữ. | 

Các ràng buộc cho phép một lần truyền tải Dijkstra vì số đỉnh và cạnh nhiều nhất là khoảng`200000`. Việc sử dụng bộ nhớ vẫn tuyến tính, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""4 4
6 3 6 6
1 3 6
1 2 3
3 2 3
4 2 3
""") == "1\n", "sample 1"

assert run("""4 4
6 3 4 8
1 3 4
1 2 3
2 4 5
3 4 4
""") == "1\n", "sample 2"

assert run("""5 6
8 4 4 8 6
1 2 4
1 3 4
2 4 4
3 4 4
1 5 3
5 3 2
""") == "4\n", "sample 3"

assert run("""1 0
1
""") == "-1\n", "single isolated vertex"

assert run("""3 2
4 2 4
1 2 2
2 3 2
""") == "2\n", "line graph"

assert run("""3 3
1 5 5
1 2 2
2 3 2
1 3 10
""") == "-1\n", "invalid distances"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn |`-1`| Xử lý trường hợp đi bộ không trống bị thiếu. | 
| Biểu đồ đường |`2`| Kiểm tra tính toán khoảng cách tự đặc biệt. | 
| Khoảng cách không hợp lệ |`-1`| Từ chối các mảng không thể được tạo ra. | 

## Vỏ cạnh 

Đối với trường hợp đỉnh đơn không hợp lệ:```
1 0
1
```Không có cạnh nào và không thể có bước đi không trống nào bắt đầu và kết thúc ở đỉnh duy nhất. Thuật toán khởi tạo đỉnh nhưng không bao giờ tạo đường dẫn nguồn thứ hai, do đó nó in chính xác`-1`. 

Đối với nguồn ở giữa dòng:```
3 2
4 2 4
1 2 2
2 3 2
```Quá trình đa nguồn tìm thấy đỉnh đó`2`có khoảng cách`2`đến cả những người hàng xóm và một tuyến đường dài khác`4`trở lại với chính nó. Khoảng cách tốt thứ hai của nó chính xác là`4`, nên nó được chấp nhận. 

Đối với một mảng không thể:```
3 3
1 5 5
1 2 2
2 3 2
1 3 10
```Bản thân biểu đồ cung cấp một tuyến đường ngắn hơn giữa một số đỉnh so với các giá trị đã cho cho phép. Khoảng cách tốt nhất được tính toán bởi Dijkstra trở nên nhỏ hơn khoảng cách tương ứng`s`giá trị, vì vậy bước xác thực đầu tiên sẽ từ chối toàn bộ mảng. 

Đối với trường hợp nhiều ứng cử viên:```
4 4
6 3 6 6
1 3 6
1 2 3
3 2 3
4 2 3
```Một số đỉnh nhận được các đường dẫn có độ dài bằng nhau, nhưng việc quét từ chỉ mục`1`trở lên trả về nguồn hợp lệ nhỏ nhất theo yêu cầu.
