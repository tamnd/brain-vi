---
title: "CF 445B - DZY Yêu Hóa Học"
description: "Các hóa chất và phản ứng tạo thành một đồ thị vô hướng. Mỗi đỉnh đại diện cho một chất hóa học và cạnh giữa hai đỉnh có nghĩa là hai chất đó phản ứng với nhau. Chúng ta bắt đầu với mức độ nguy hiểm bằng 1. Hóa chất được đổ lần lượt theo thứ tự bất kỳ."
date: "2026-06-07T16:02:53+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "greedy"]
categories: ["algorithms"]
codeforces_contest: 445
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 254 (Div. 2)"
rating: 1400
weight: 445
solve_time_s: 109
verified: true
draft: false
---

[CF 445B - DZY yêu thích hóa học](https://codeforces.com/problemset/problem/445/B) 

**Đánh giá:** 1400 
**Tags:** dfs và tương tự, dsu, tham lam 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các hóa chất và phản ứng tạo thành một đồ thị vô hướng. Mỗi đỉnh đại diện cho một chất hóa học và cạnh giữa hai đỉnh có nghĩa là hai chất đó phản ứng với nhau. 

Chúng ta bắt đầu với mức độ nguy hiểm bằng 1. Hóa chất được đổ lần lượt theo thứ tự bất kỳ. Bất cứ khi nào hóa chất mới được thêm vào có ít nhất một cạnh phản ứng với hóa chất đã có sẵn bên trong ống, thì mối nguy hiểm hiện tại sẽ được nhân với 2. Nếu không thì mối nguy hiểm sẽ không thay đổi. 

Nhiệm vụ là chọn thứ tự đổ sao cho mối nguy hiểm cuối cùng được tối đa hóa. 

Việc đọc trực tiếp gợi ý rằng câu trả lời phụ thuộc vào thứ tự, nhưng cấu trúc biểu đồ ẩn chứa một mẫu đơn giản hơn nhiều. Câu hỏi quan trọng là chúng ta có thể ép nhân với 2 bao nhiêu lần. 

Các ràng buộc đủ nhỏ để bất kỳ thuật toán đồ thị tuyến tính hoặc gần tuyến tính nào cũng có thể dễ dàng được chấp nhận. Kể cả nếu$n$Và$m$chỉ có khoảng vài chục trong vấn đề ban đầu, giải pháp dự định dựa trên các thành phần được kết nối và chạy trong$O(n+m)$. Hoán vị cưỡng bức vũ phu sẽ yêu cầu$n!$những khả năng gần như ngay lập tức trở thành không thể. 

Một số trường hợp đặc biệt rất dễ bị hiểu sai. 

Xét một đồ thị không có cạnh:```
3 0
```Không có hóa chất nào phản ứng với hóa chất được đưa vào trước đó. Sự nguy hiểm không bao giờ thay đổi, vì vậy câu trả lời là:```
1
```Một cách tiếp cận bất cẩn bằng cách đếm các đỉnh thay vì phản ứng có thể tạo ra giá trị lớn hơn một cách không chính xác. 

Hãy xem xét một chuỗi được kết nối:```
3 2
1 2
2 3
```Một thứ tự tối ưu là 1, 2, 3. Hóa chất đầu tiên không cho phép nhân. Thứ hai và thứ ba đều phản ứng với thứ gì đó đã có sẵn, do đó nguy hiểm tăng gấp đôi. Câu trả lời là:```
4
```Một sai lầm phổ biến là nghĩ rằng mọi cạnh đều đóng góp một phép nhân. Có hai cạnh ở đây, nhưng chỉ có hai lần chèn thành công sau đỉnh đầu tiên. 

Hãy xem xét hai thành phần bị ngắt kết nối:```
4 2
1 2
3 4
```Mỗi thành phần đóng góp một phép nhân. Câu trả lời là:```
4
```Xử lý toàn bộ biểu đồ như một cấu trúc duy nhất sẽ đưa ra kết quả không chính xác$2^3=8$. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi thứ tự rót có thể. Đối với mỗi hoán vị, chúng tôi mô phỏng quy trình và đếm số lượng lần thêm vào gây ra phản ứng với một hóa chất đã được đưa vào. 

Điều này đúng vì nó đánh giá rõ ràng mọi thứ tự hợp lệ. Thật không may, có$n!$hoán vị. Ngay cả đối với$n=15$, điều này đã có hơn một nghìn tỷ khả năng, khiến nó hoàn toàn không thể thực hiện được. 

Để tìm ra cách tiếp cận tốt hơn, chúng ta cần hiểu điều gì xảy ra bên trong một thành phần được kết nối. 

Lấy bất kỳ thành phần được kết nối nào có chứa$k$đỉnh. Hóa chất đầu tiên được đổ từ thành phần đó không thể phản ứng với bất cứ thứ gì từ cùng một thành phần vì chưa có thành phần nào của nó có mặt. Mọi đỉnh khác có thể được sắp xếp để phản ứng. 

Tại sao? Bắt đầu từ bất kỳ đỉnh nào của thành phần. Vì thành phần được kết nối nên chúng ta có thể hiển thị các đỉnh còn lại theo thứ tự DFS hoặc BFS. Mỗi đỉnh mới được tiết lộ đều có một cạnh so với một số đỉnh đã được tiết lộ trước đó, vì vậy mỗi lần chèn đó sẽ nhân mức độ nguy hiểm lên gấp 2. 

Điều đó có nghĩa là một thành phần được kết nối có kích thước$k$đóng góp chính xác$k-1$phép nhân. 

Các thành phần được kết nối khác nhau là độc lập. Mỗi thành phần đóng góp riêng$k-1$phép nhân và tính tổng tất cả các thành phần sẽ cho$$\sum (k_i - 1)$$Ở đâu$k_i$là kích thước của$i$-thành phần được kết nối. 

Vì kích thước các thành phần cộng lại bằng$n$,$$\sum (k_i - 1) = n - c$$Ở đâu$c$là số thành phần được kết nối. 

Do đó, mối nguy hiểm cuối cùng là$$2^{\,n-c}.$$Tất cả những gì còn lại là đếm các thành phần được kết nối với DFS, BFS hoặc DSU. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot (n+m))$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n+m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị vô hướng từ các cặp phản ứng. 
2. Tìm tất cả các thành phần được kết nối bằng DFS. 
3. Đối với mỗi đỉnh chưa được thăm, hãy khởi động một DFS và đếm xem có bao nhiêu đỉnh thuộc về thành phần của nó. 
4. Nếu một thành phần chứa$k$đỉnh, thêm$k-1$với tổng số phép nhân nguy hiểm. 

Đỉnh đầu tiên của thành phần không thể tạo ra phản ứng. Mỗi đỉnh còn lại có thể được chèn vào sau một đỉnh lân cận đã có sẵn. 
5. Sau khi xử lý tất cả các thành phần, gọi tổng số phép nhân là$cnt$. 
6. Câu trả lời là$2^{cnt}$. 
7. In câu trả lời. 

### Tại sao nó hoạt động 

Bên trong một thành phần được kết nối có kích thước$k$, trước tiên phải chèn ít nhất một đỉnh. Việc chèn đó không thể nhân lên mối nguy hiểm vì không có đỉnh nào khác của thành phần hiện diện. 

Mọi đỉnh khác đều có thể được chèn sau khi tiếp cận nó thông qua quá trình truyền tải DFS hoặc BFS. Tại thời điểm đó, một trong những người hàng xóm của nó đã có mặt, do đó việc chèn vào sẽ nhân mức độ nguy hiểm lên gấp 2. 

Do đó một thành phần đóng góp chính xác$k-1$phép nhân, không hơn không kém. Tính tổng tất cả các thành phần sẽ cho ra tổng số phép nhân. Vì mỗi phép nhân nhân đôi mức độ nguy hiểm và mức độ nguy hiểm ban đầu là 1 nên câu trả lời cuối cùng là$2^{\sum(k-1)}$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    visited = [False] * n
    multiplications = 0

    for start in range(n):
        if visited[start]:
            continue

        stack = [start]
        visited[start] = True
        size = 0

        while stack:
            v = stack.pop()
            size += 1

            for to in graph[v]:
                if not visited[to]:
                    visited[to] = True
                    stack.append(to)

        multiplications += size - 1

    print(1 << multiplications)

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề vì đầu vào mô tả một cách tự nhiên các cạnh giữa các hóa chất. 

DFS đếm kích thước của từng thành phần được kết nối. Khi đã biết kích thước thành phần, chúng tôi thêm`size - 1`đến số lần nhân đôi nguy hiểm thành công. 

Câu trả lời cuối cùng là lũy thừa của hai. Số nguyên Python có độ chính xác tùy ý, do đó, ngay cả câu trả lời hợp lệ lớn nhất cũng phù hợp một cách thoải mái. sử dụng`1 << multiplications`tính toán$2^{\text{multiplications}}$một cách trực tiếp và hiệu quả. 

Các đỉnh trong đầu vào được đánh số từ 1, trong khi danh sách Python sử dụng chỉ mục dựa trên 0, do đó mỗi điểm cuối được giảm chính xác một lần khi đọc các cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 0
```| Bước | Kích thước thành phần | Phép nhân được thêm vào | Tổng số nhân | 
| --- | --- | --- | --- | 
| DFS từ 1 | 1 | 0 | 0 | 

Câu trả lời cuối cùng:$$2^0 = 1$$Đầu ra:```
1
```Điều này thể hiện đồ thị nhỏ nhất có thể. Một hóa chất cô lập duy nhất không bao giờ tạo ra phản ứng. 

### Ví dụ 2 

đầu vào:```
4 2
1 2
3 4
```| Bước | Kích thước thành phần | Phép nhân được thêm vào | Tổng số nhân | 
| --- | --- | --- | --- | 
| DFS từ 1 | 2 | 1 | 1 | 
| DFS từ 3 | 2 | 1 | 2 | 

Câu trả lời cuối cùng:$$2^2 = 4$$Đầu ra:```
4
```Ví dụ này cho thấy các thành phần bị ngắt kết nối đóng góp độc lập. Mỗi thành phần có kích thước hai đóng góp một lần nhân đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+m)$| Mỗi đỉnh và cạnh được truy cập nhiều nhất một lần trong DFS | 
| Không gian |$O(n+m)$| Danh sách kề cộng với mảng đã truy cập | 

Thuật toán thực hiện duyệt đồ thị đơn. Mỗi cạnh được kiểm tra một số lần không đổi, khiến nó dễ dàng đủ nhanh để vượt qua các giới hạn của bài toán. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    visited = [False] * n
    mult = 0

    for i in range(n):
        if visited[i]:
            continue

        stack = [i]
        visited[i] = True
        size = 0

        while stack:
            v = stack.pop()
            size += 1

            for to in graph[v]:
                if not visited[to]:
                    visited[to] = True
                    stack.append(to)

        mult += size - 1

    return str(1 << mult)

# provided sample
assert run("1 0\n") == "1", "sample 1"

# single edge
assert run("2 1\n1 2\n") == "2", "one reaction"

# chain of three vertices
assert run("3 2\n1 2\n2 3\n") == "4", "connected component size 3"

# two disconnected pairs
assert run("4 2\n1 2\n3 4\n") == "4", "two components"

# complete graph on four vertices
assert run(
    "4 6\n"
    "1 2\n"
    "1 3\n"
    "1 4\n"
    "2 3\n"
    "2 4\n"
    "3 4\n"
) == "8", "single component size 4"

# all isolated vertices
assert run("5 0\n") == "1", "no reactions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| Đầu vào tối thiểu | 
|`2 1`|`2`| Thành phần kết nối đơn có kích thước 2 | 
| Chuỗi dài 3 |`4`| Logic truyền tải được kết nối | 
| Hai cặp ngắt kết nối |`4`| Nhiều thành phần | 
| Đồ thị hoàn chỉnh có 4 đỉnh |`8`| Đồ thị dày đặc, câu trả lời chỉ phụ thuộc vào kích thước thành phần | 
|`5 0`|`1`| Tất cả các đỉnh bị cô lập | 

## Vỏ cạnh 

###Tất cả hóa chất đều được cách ly 

đầu vào:```
4 0
```Biểu đồ chứa bốn thành phần được kết nối, mỗi thành phần có kích thước 1. 

| Kích thước thành phần | Đóng góp | 
| --- | --- | 
| 1 | 0 | 
| 1 | 0 | 
| 1 | 0 | 
| 1 | 0 | 

Tổng số nhân bằng 0 nên đáp án là:$$2^0 = 1.$$Thuật toán tính toán chính xác`size - 1 = 0`cho mọi thành phần. 

### Một thành phần được kết nối lớn 

đầu vào:```
4 3
1 2
2 3
3 4
```Biểu đồ được kết nối. 

| Kích thước thành phần | Đóng góp | 
| --- | --- | 
| 4 | 3 | 

Câu trả lời là:$$2^3 = 8.$$DFS tìm thấy một thành phần có kích thước 4 và cộng chính xác 3 phép nhân. 

### Nhiều thành phần bị ngắt kết nối có kích thước khác nhau 

đầu vào:```
6 3
1 2
2 3
5 6
```Kích thước thành phần là 3, 1 và 2. 

| Kích thước thành phần | Đóng góp | 
| --- | --- | 
| 3 | 2 | 
| 1 | 0 | 
| 2 | 1 | 

Tổng số phép nhân:$$2 + 0 + 1 = 3.$$Câu trả lời cuối cùng:$$2^3 = 8.$$Điều này xác nhận rằng thuật toán xử lý các đỉnh biệt lập và các thành phần không cần thiết cùng nhau mà không có bất kỳ trường hợp đặc biệt nào.
