---
title: "CF 107A - Cấp Nước Ký Túc Xá"
description: "Những ngôi nhà và đường ống tạo thành một đồ thị có hướng với cấu trúc rất đặc biệt. Mỗi ngôi nhà có thể có nhiều nhất một đường ống vào và nhiều nhất một đường ống ra. Hạn chế đó thay đổi biểu đồ từ biểu đồ có hướng chung thành một tập hợp các chuỗi độc lập."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 107
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 83 (Div. 1 Only)"
rating: 1400
weight: 107
solve_time_s: 139
verified: true
draft: false
---

[CF 107A - Cấp nước cho ký túc xá](https://codeforces.com/problemset/problem/107/A) 

**Đánh giá:** 1400 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Những ngôi nhà và đường ống tạo thành một đồ thị có hướng với cấu trúc rất đặc biệt. Mỗi ngôi nhà có thể có nhiều nhất một đường ống vào và nhiều nhất một đường ống ra. Hạn chế đó thay đổi biểu đồ từ biểu đồ có hướng chung thành một tập hợp các chuỗi độc lập. 

Một ngôi nhà có dòng chảy đi nhưng không có dòng chảy vào là sự khởi đầu của một chuỗi. Ngôi nhà đó cần một chiếc xe tăng. Một ngôi nhà có dòng chảy vào nhưng không có dòng chảy ra là sự kết thúc của một chuỗi. Ngôi nhà đó cần một cái vòi. 

Đối với mỗi chuỗi như vậy, chúng tôi phải báo cáo ba giá trị: 

1. nhà bắt đầu chứa bể, 
2. nhà cuối chứa vòi, 
3. đường kính ống tối thiểu dọc theo đường dẫn. 

Đường kính tối thiểu rất quan trọng vì nước chỉ có thể chảy ở mức đường ống hẹp nhất cho phép. 

Ví dụ: nếu chúng ta có:```
1 -> 2 (10)
2 -> 3 (20)
3 -> 4 (5)
```thì câu trả lời là:```
1 4 5
```vì ống cổ chai có đường kính 5. 

Hạn chế rất nhỏ, chỉ tối đa 1000 ngôi nhà và 1000 đường ống. Thậm chí một$O(n^2)$giải pháp sẽ trôi qua một cách thoải mái. Tuy nhiên, cấu trúc đồ thị cho phép một giải pháp tuyến tính rõ ràng. 

Những sai lầm nguy hiểm nhất đến từ việc hiểu sai cấu trúc chuỗi. 

Một lỗi phổ biến là khởi động DFS từ mọi nút thay vì chỉ các nguồn thực sự. 

Coi như:```
4 3
1 2 5
2 3 4
3 4 3
```Đầu ra đúng là:```
1
1 4 3
```Việc triển khai bất cẩn cũng có thể tạo ra:```
2 4 3
3 4 3
```bởi vì nó bắt đầu truyền tải từ các nút nội bộ. 

Một trường hợp tế nhị khác là những ngôi nhà biệt lập.```
3 0
```Không nhà nào có đường ống nên không có cặp vòi-bể nào cả:```
0
```Một giải pháp đơn giản có thể vô tình coi các nút bị cô lập vừa là bể chứa vừa là vòi. 

Trường hợp cạnh thứ ba là nhiều chuỗi bị ngắt kết nối.```
6 4
1 2 8
2 3 6
4 5 10
5 6 7
```Câu trả lời phải chứa cả hai hệ thống độc lập:```
2
1 3 6
4 6 7
```Thứ tự đầu ra quan trọng. Kết quả phải được sắp xếp theo chỉ số bể chứa, điều này xảy ra một cách tự nhiên nếu chúng ta quét các nhà từ 1 đến$n$. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi ngôi nhà, hãy thử đi theo các đường ống đi ra ngoài cho đến khi kết thúc chuỗi. Trong khi đi bộ, hãy theo dõi đường kính tối thiểu nhìn thấy. 

Điều này hiệu quả vì mỗi nút có nhiều nhất một cạnh đi ra, do đó việc truyền tải rất đơn giản và không bao giờ phân nhánh. 

Vấn đề xuất hiện khi chúng ta lặp lại cùng một công việc nhiều lần. Trong một chuỗi dài$n$:```
1 -> 2 -> 3 -> ... -> n
```bắt đầu từ mọi nút gây ra sự truyền tải lặp đi lặp lại. 

Từ nút 1 chúng ta đi bộ$n$các bước. 

Từ nút 2 chúng ta đi bộ$n-1$các bước. 

Từ nút 3 chúng tôi đi bộ$n-2$các bước. 

Tổng số trở thành:$$n + (n-1) + (n-2) + \dots + 1 = O(n^2)$$Với các ràng buộc đã cho, điều này vẫn vượt qua nhưng nó bỏ qua cấu trúc chính của biểu đồ. 

Quan sát quan trọng là mọi hệ thống nước hợp lệ đều bắt đầu tại một nút có:```
indegree = 0
outdegree = 1
```Một nút như vậy là điểm khởi đầu duy nhất của một chuỗi. Nếu chúng ta chỉ bắt đầu từ các nút này thì mỗi đường ống sẽ được truy cập chính xác một lần. 

Từ mỗi nút nguồn, chúng tôi liên tục di chuyển về phía trước cho đến khi đến được nút không có đường ống đi. Trong quá trình truyền tải, chúng tôi duy trì đường kính tối thiểu. 

Bởi vì mỗi nút có nhiều nhất một cạnh đi ra nên việc truyền tải là xác định. Không phân nhánh, không xem lại, không mơ hồ. 

Điều này làm giảm độ phức tạp về thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng để lưu trữ các kết nối đi, kết nối vào và đường kính ống. 

Vì mỗi nút có nhiều nhất một đường dẫn đi ra nên chúng tôi chỉ cần một đích đến cho mỗi nút thay vì danh sách lân cận. 
2. Đọc từng ống`(a, b, d)`. 

Cửa hàng:

-`to[a] = b`-`diameter[a] = d`-`indegree[b] += 1`Điều này cho phép truy cập trực tiếp vào nút tiếp theo trong chuỗi. 
3. Quét tất cả các ngôi nhà từ`1`ĐẾN`n`. 

Một ngôi nhà là sự khởi đầu của một hệ thống hợp lệ nếu: 

- nó có một đường ống đi ra, 
- nó không có đường ống vào. 

Đây chính xác là những ngôi nhà xe tăng. 
4. Đối với mỗi nhà chứa, hãy đi theo chuỗi cho đến khi không còn đường ống thoát ra. 

Trong khi đi ngang: 

- cập nhật đường kính tối thiểu, 
- di chuyển đến nút tiếp theo. 
5. Nút đến cuối cùng là nhà máy. 

Cửa hàng: 

- bể chứa, 
- gõ, 
- đường kính tối thiểu. 
6. In tất cả các kết quả được lưu trữ. 

Vì các ngôi nhà được quét theo thứ tự tăng dần nên thứ tự đầu ra đã chính xác. 

### Tại sao nó hoạt động 

Mọi thành phần được kết nối trong biểu đồ này là một chuỗi có hướng đơn giản vì mỗi nút có nhiều nhất một cạnh vào và một cạnh ra. 

Hệ thống nước hợp lệ phải bắt đầu tại nút duy nhất không có cạnh đầu vào và kết thúc tại nút duy nhất không có cạnh đầu ra. 

Chỉ bắt đầu truyền tải từ các nút nguồn đảm bảo rằng: 

- mỗi chuỗi được xử lý chính xác một lần, 
- không có nút bên trong nào trở thành một bể riêng biệt một cách không chính xác, 
- đường kính tối thiểu được thu thập trong quá trình truyền tải chính xác là khả năng thắt cổ chai của chuỗi đó. 

Vì việc truyền tải đi theo con đường duy nhất có thể có trong thành phần nên thuật toán không thể bỏ sót hoặc trùng lặp bất kỳ câu trả lời hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, p = map(int, input().split())

to = [0] * (n + 1)
diameter = [0] * (n + 1)
indegree = [0] * (n + 1)

for _ in range(p):
    a, b, d = map(int, input().split())
    to[a] = b
    diameter[a] = d
    indegree[b] += 1

answer = []

for i in range(1, n + 1):
    if indegree[i] == 0 and to[i] != 0:
        current = i
        minimum_diameter = float('inf')

        while to[current] != 0:
            minimum_diameter = min(minimum_diameter, diameter[current])
            current = to[current]

        answer.append((i, current, minimum_diameter))

print(len(answer))

for tank, tap, d in answer:
    print(tank, tap, d)
```Các mảng là sự đơn giản hóa cốt lõi. Bởi vì mỗi nút có nhiều nhất một cạnh đi ra,`to[u]`xác định duy nhất ngôi nhà tiếp theo trong chuỗi. Chúng tôi không cần danh sách kề hoặc đệ quy DFS. 

các`indegree`mảng xác định nút nào là nguồn thực sự. Chỉ những nút đó mới có thể chứa xe tăng. 

Vòng lặp truyền tải đi qua một chuỗi tại một thời điểm. Biến`minimum_diameter`theo dõi đường ống cổ chai gặp phải cho đến nay. Vì đường dẫn là tuyến tính nên mức chạy tối thiểu duy nhất này là đủ. 

sử dụng`float('inf')`vì giá trị ban đầu tránh được việc xử lý trường hợp đặc biệt cho cạnh đầu tiên. 

Một lỗi dễ mắc phải là cập nhật mức tối thiểu sau khi chuyển sang nút tiếp theo. Đường kính ống thuộc về cạnh đi ra của nút hiện tại, do đó mức tối thiểu phải được cập nhật trước khi tiến lên. 

Một điểm tinh tế khác là dừng lại một cách chính xác. Nút cuối cùng trong chuỗi có`to[current] == 0`, nghĩa là nó không có đường ống đi ra ngoài và là nhà chứa vòi nước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 10
2 3 20
```Trạng thái truyền tải: 

| Nút hiện tại | Nút tiếp theo | Tối thiểu hiện tại | 
| --- | --- | --- | 
| 1 | 2 | 10 | 
| 2 | 3 | 10 | 

Kết quả cuối cùng:```
1 3 10
```Dây chuyền bắt đầu từ nhà 1 vì nó không có đường ống dẫn vào. Đường kính nhỏ nhất dọc theo đường đi là`min(10, 20) = 10`. 

### Ví dụ 2 

đầu vào:```
6 4
1 2 8
2 3 6
4 5 10
5 6 7
```Truyền tải chuỗi đầu tiên: 

| Nút hiện tại | Nút tiếp theo | Tối thiểu hiện tại | 
| --- | --- | --- | 
| 1 | 2 | 8 | 
| 2 | 3 | 6 | 

Truyền tải chuỗi thứ hai: 

| Nút hiện tại | Nút tiếp theo | Tối thiểu hiện tại | 
| --- | --- | --- | 
| 4 | 5 | 10 | 
| 5 | 6 | 7 | 

Đầu ra cuối cùng:```
2
1 3 6
4 6 7
```Ví dụ này cho thấy các chuỗi bị ngắt kết nối được xử lý độc lập. Mỗi dây chuyền có đường kính bể, vòi và cổ chai riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và đường ống được xử lý nhiều nhất một lần | 
| Không gian | O(n) | Mảng cho các cạnh, đường kính và độ trong | 

Với tối đa 1000 ngôi nhà và đường ống, giải pháp dễ dàng nằm trong giới hạn. Ngay cả các giải pháp bậc hai cũng có thể vượt qua, nhưng việc truyền tải tuyến tính sẽ khai thác triệt để cấu trúc đồ thị. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, p = map(int, input().split())

    to = [0] * (n + 1)
    diameter = [0] * (n + 1)
    indegree = [0] * (n + 1)

    for _ in range(p):
        a, b, d = map(int, input().split())
        to[a] = b
        diameter[a] = d
        indegree[b] += 1

    answer = []

    for i in range(1, n + 1):
        if indegree[i] == 0 and to[i] != 0:
            current = i
            minimum_diameter = float('inf')

            while to[current] != 0:
                minimum_diameter = min(minimum_diameter, diameter[current])
                current = to[current]

            answer.append((i, current, minimum_diameter))

    output = [str(len(answer))]

    for a, b, d in answer:
        output.append(f"{a} {b} {d}")

    print("\n".join(output))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""3 2
1 2 10
2 3 20
"""
) == """1
1 3 10
"""

# no pipes
assert run(
"""3 0
"""
) == """0
"""

# single pipe
assert run(
"""2 1
1 2 5
"""
) == """1
1 2 5
"""

# multiple disconnected chains
assert run(
"""6 4
1 2 8
2 3 6
4 5 10
5 6 7
"""
) == """2
1 3 6
4 6 7
"""

# equal diameters
assert run(
"""4 3
1 2 5
2 3 5
3 4 5
"""
) == """1
1 4 5
"""

# bottleneck in middle
assert run(
"""5 4
1 2 100
2 3 2
3 4 50
4 5 60
"""
) == """1
1 5 2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có đường ống |`0`| Các nút bị cô lập bị bỏ qua | 
| Ống đơn |`1 2 5`| Chuỗi hợp lệ đơn giản nhất | 
| Nhiều chuỗi bị ngắt kết nối | Hai đầu ra | Thành phần độc lập | 
| Đường kính bằng nhau | Tối thiểu không thay đổi | Theo dõi tắc nghẽn ổn định | 
| Nút cổ chai ở giữa | Tối thiểu trở thành cạnh bên trong | Tuyên truyền tối thiểu đúng | 

## Vỏ cạnh 

### Các nút bên trong bị xử lý không chính xác như xe tăng 

đầu vào:```
4 3
1 2 5
2 3 4
3 4 3
```Thuật toán tính toán theo độ: 

| Nút | Bằng cấp | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Chỉ nút 1 đủ tiêu chuẩn là xe tăng vì chỉ riêng nút này không có cạnh đi vào. 

Truyền tải:```
1 -> 2 -> 3 -> 4
```Đường kính tối thiểu trở thành:```
min(5, 4, 3) = 3
```Đầu ra:```
1
1 4 3
```Các nút bên trong sẽ tự động bị bỏ qua vì mức độ của chúng không bằng 0. 

### Nhà biệt lập 

đầu vào:```
3 0
```Mỗi nút có:```
indegree = 0
outdegree = 0
```Điều kiện để bắt đầu truyền tải là:```
indegree[i] == 0 and to[i] != 0
```Vì không có nút nào có đường dẫn đi nên không có quá trình truyền tải nào bắt đầu. 

Đầu ra:```
0
```Điều này ngăn chặn việc các ngôi nhà biệt lập bị báo cáo không chính xác dưới dạng cặp vòi-bể. 

### Nhiều hệ thống độc lập 

đầu vào:```
6 4
1 2 8
2 3 6
4 5 10
5 6 7
```Thuật toán tìm thấy hai nút nguồn:```
1 and 4
```Lần duyệt đầu tiên:```
1 -> 2 -> 3
minimum = 6
```Lần duyệt thứ hai:```
4 -> 5 -> 6
minimum = 7
```Đầu ra:```
2
1 3 6
4 6 7
```Mỗi thành phần được kết nối được xử lý độc lập và không có cạnh nào được xem lại.
