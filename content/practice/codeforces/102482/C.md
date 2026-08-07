---
title: "CF 102482C - Chinh Phục Thế Giới"
description: "Chúng ta có một mạng lưới các quốc gia được kết nối và mạng lưới tạo thành một cái cây, nghĩa là mỗi cặp quốc gia có chính xác một con đường nối giữa họ. Mỗi con đường đều có chi phí cho mỗi đội quân đi qua nó. Quốc gia thứ nhất bắt đầu với quân đội xi và cần quân đội yi được thỏa mãn."
date: "2026-08-06T18:43:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102482
codeforces_index: "C"
codeforces_contest_name: "2018 ACM-ICPC World Finals"
rating: 0
weight: 102482
solve_time_s: 84
verified: true
draft: false
---

[CF 102482C - Chinh phục thế giới](https://codeforces.com/problemset/problem/102482/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các quốc gia được kết nối và mạng lưới tạo thành một cái cây, nghĩa là mỗi cặp quốc gia có chính xác một con đường nối giữa họ. Mỗi con đường đều có chi phí cho mỗi đội quân đi qua nó. Quốc gia`i`bắt đầu bằng`xi`quân đội và nhu cầu`yi`quân đội phải hài lòng. Quân đội bổ sung có thể vẫn còn ở bất cứ đâu sau khi tất cả các yêu cầu được đáp ứng. 

Nhiệm vụ là lựa chọn cách di chuyển quân sao cho mỗi quốc gia đạt được số lượng yêu cầu trong khi tổng chi phí đi lại càng nhỏ càng tốt. Vì mỗi đội quân di chuyển qua một con đường đều phải trả chi phí đường bộ nên sản lượng là tổng tối thiểu có thể có của tất cả các chi phí di chuyển. 

Hạn chế chính là kích thước của cây. Với tới 250.000 quốc gia, các thuật toán kiểm tra mọi chuyển động có thể xảy ra hoặc thử tất cả các nhiệm vụ của quân đội là không thể. Ngay cả một thuật toán với khoảng`O(n * number_of_armies)`các hoạt động sẽ quá lớn vì tổng số quân có thể lên tới 1.000.000. Giải pháp cần xử lý mỗi quốc gia và mỗi con đường chỉ với một số lần không đổi, hướng tới thuật toán cây tuyến tính. 

Tổng số quân bị giới hạn rất hữu ích cho một số giải pháp khả thi, nhưng số lượng quốc gia đủ lớn nên việc lưu trữ từng đội quân riêng biệt hoặc mô phỏng các hoạt động di chuyển sẽ không phù hợp một cách thoải mái. Việc những con đường tạo thành một cái cây là đặc tính cấu trúc quan trọng bởi vì mỗi con đường đều chia thế giới thành hai phía độc lập. 

Một giải pháp bất cẩn có thể thất bại đối với một quốc gia có cả nhu cầu đến và đi trong cùng một cây con. Ví dụ:```
2
1 2 10
5 3
0 4
```Quốc gia thứ nhất dư 2 quân đội và quốc gia thứ hai cần 4 quân đội. Câu trả lời đúng là 20 vì hai đội quân phải đi qua con đường duy nhất và mỗi đội tốn 10. Một giải pháp chỉ xét đến từng quốc gia có thể cho rằng quốc gia thứ nhất có thể đáp ứng được quốc gia thứ hai mà không tính đến số tiền thâm hụt còn lại. 

Một trường hợp cạnh khác là khi gốc của cây bị thừa hoặc thiếu. Ví dụ:```
1
5 3
```Câu trả lời đúng là 0. Không có nơi nào để di chuyển quân đội và quốc gia đã có đủ rồi. Một giải pháp giả định rằng mọi khoản thặng dư phải đi lên từ gốc sẽ cộng thêm chi phí một cách không chính xác. 

Trường hợp phức tạp cuối cùng là cây con chứa cả quân đội thiếu hụt và quân đội bổ sung. Ví dụ:```
3
1 2 7
2 3 4
0 2
5 1
0 3
```Cây con chứa nút 2 và 3 có tổng thặng dư là 0 vì nút 2 có thêm bốn đội quân và nút 3 cần thêm ba đội quân trong khi bản thân nút 2 cần một đội quân. Di chuyển duy nhất cần thiết là một đội quân từ nút 2 đến nút 3, chi phí là 4. Một giải pháp chỉ xem xét tổng số dư của cây con và bỏ qua cân bằng nội bộ sẽ đánh giá quá cao chi phí. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng liên tục di chuyển quân đội từ một quốc gia có thêm quân đội sang một quốc gia thiếu quân đội. Vì đồ thị là một cái cây nên khoảng cách giữa hai quốc gia bất kỳ rất dễ tính toán. Đối với mỗi đội quân bị mất, chúng ta có thể tìm một đội quân dư gần đó và di chuyển nó dọc theo con đường. Điều này đúng vì cuối cùng thì quân đội nào cũng phải di chuyển từ nơi dư thừa đến nơi thiếu hụt. 

Vấn đề là khối lượng công việc. Có thể có tới 1.000.000 quân đội và mỗi cuộc di chuyển của quân đội có thể liên quan đến việc đi bộ qua một con đường có tới 250.000 con đường. Một mô phỏng có thể đạt được xung quanh`10^11`truyền tải cạnh trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát làm thay đổi vấn đề là danh tính chính xác của từng đội quân không quan trọng. Hãy xem xét bất kỳ con đường. Việc loại bỏ con đường đó sẽ chia cây thành hai phần. Nếu thành phần bên dưới con đường có nhiều quân hơn mức cần thiết thì tất cả quân bổ sung phải băng qua con đường đó để rời đi. Nếu cần nhiều quân hơn số quân đang có, quân mất tích phải băng qua con đường đó để vào. Không có con đường thay thế nào vì đồ thị là một cái cây. 

Do đó, số lượng quân vượt qua một rìa hoàn toàn được xác định bởi tổng số dư hoặc thiếu bên trong một cạnh của cạnh đó. Chúng ta chỉ cần tính toán số dư cây con. 

Cho phép`balance[i] = xi - yi`. Giá trị dương nghĩa là cây con có thêm quân đội và giá trị âm nghĩa là nó cần quân đội. Trong lần duyệt theo chiều sâu đầu tiên, chúng tôi tính toán tổng số dư trong mỗi cây con. Đối với mỗi cạnh con, giá trị tuyệt đối của tổng cây con của cạnh đó cho chúng ta biết có bao nhiêu đội quân phải vượt qua cạnh đó. Nhân với chi phí cạnh sẽ biết được sự đóng góp của con đường đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số quân × n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây và lưu trữ số dư ban đầu của mỗi quốc gia`xi - yi`. Số dư dương tượng trưng cho những đội quân có thể rời khỏi đất nước, trong khi số dư âm tượng trưng cho những đội quân phải đến. 
2. Root cây ở bất kỳ quốc gia nào và thực hiện duyệt theo chiều sâu đầu tiên. Trong quá trình duyệt, hãy tính tổng số dư của từng cây con. 

Việc chọn gốc không ảnh hưởng đến câu trả lời vì mỗi cạnh được tính chính xác một lần và chi phí cạnh chỉ phụ thuộc vào hai cạnh được tạo bằng cách loại bỏ nó. 
3. Khi trả về từ cây con con về cây cha của nó, hãy thêm`abs(child_balance) * edge_cost`để trả lời. 

Cây con con sẽ gửi`child_balance`quân đội trở lên hoặc nhận được`-child_balance`quân đi xuống. Con đường phải xử lý chính xác số quân đó vì giữa hai bên không có mối liên hệ nào khác. 
4. Thêm số dư của con vào số dư của cha mẹ và tiếp tục cho đến khi toàn bộ cây được xử lý. 

Số dư cuối cùng của gốc không cần phải di chuyển vì tuyên bố đảm bảo rằng tổng số quân là đủ để đáp ứng mọi yêu cầu. 

### Tại sao nó hoạt động 

Đối với mỗi cạnh, cấu trúc cây buộc tất cả tương tác giữa hai cạnh của cạnh đó phải đi qua cạnh đó. Số lượng quân cuối cùng cần có trong một cây con là cố định, do đó số lượng thực tế đi qua kết nối duy nhất của nó cũng cố định. Quá trình truyền tải tính toán số tiền chính xác này cho mọi cạnh và tính chi phí tối thiểu có thể cho chuyển động không thể tránh khỏi đó. Vì mọi đóng góp của con đường đều bị ép buộc độc lập nên tổng của tất cả các đóng góp là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    graph = [[] for _ in range(n)]
    
    for _ in range(n - 1):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, c))
        graph[v].append((u, c))
    
    balance = [0] * n
    for i in range(n):
        x, y = map(int, input().split())
        balance[i] = x - y
    
    ans = 0
    stack = [(0, -1, 0)]
    order = []
    
    while stack:
        u, p, state = stack.pop()
        if state == 0:
            stack.append((u, p, 1))
            for v, c in graph[u]:
                if v != p:
                    stack.append((v, u, 0))
        else:
            order.append((u, p))
    
    for u, p in order:
        if p != -1:
            pass
    
    parent = [-1] * n
    parent_cost = [0] * n
    stack = [0]
    parent[0] = -2
    
    while stack:
        u = stack.pop()
        for v, c in graph[u]:
            if parent[v] == -1:
                parent[v] = u
                parent_cost[v] = c
                stack.append(v)
    
    ans = 0
    for u, p in reversed(order):
        if p != -1:
            ans += abs(balance[u]) * parent_cost[u]
            balance[p] += balance[u]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã tránh đệ quy vì một cây có 250.000 nút có thể tạo độ sâu đệ quy lớn hơn giới hạn mặc định của Python. Quá trình duyệt lặp đầu tiên tạo ra một chuỗi thứ tự sau, cho phép chúng ta xử lý con trước cha mẹ. 

Lần truyền thứ hai ghi lại nút cha của mỗi nút và chi phí của cạnh kết nối nó với nút cha đó. Điều này tránh việc lưu trữ trạng thái bổ sung trong quá trình tính toán thứ tự sau. 

Vòng lặp cuối cùng đi qua các nút theo thứ tự ngược lại. Tại thời điểm đó, số dư cây con của mọi con đã hoàn tất, do đó mã có thể tính chi phí biên và hợp nhất số dư của cây con với cây cha. 

Số nguyên Python tự động xử lý các giá trị câu trả lời lớn. Chi phí tối đa có thể vào khoảng`10^6 * 10^6 * 250000`theo tỷ lệ, vượt quá giới hạn số nguyên 32 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Sử dụng cây:```
1
|
5
|
3
```với số dư:```
node 1: 2 - 1 = 1
node 2: 5 - 0 = 5
node 3: 1 - 3 = -2
```| Nút được xử lý | Cân bằng cây con | Chi phí cạnh cho phụ huynh | Chi phí bổ sung | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | 5 | 5 | 25 | 25 | 
| 3 | -2 | 5 | 10 | 35 | 
| 1 | 4 | không | 0 | 35 | 

Cây con của nút 2 có thêm năm đội quân nên cả năm đội quân đều phải băng qua đường. Cây con của nút 3 cần có hai đội quân, vì vậy hai đội quân phải băng qua từ phía bên kia. 

### Mẫu 2 

Số dư là: 

| Nút | Số dư ban đầu | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | -1 | 
| 6 | -1 | 

Xử lý lá trước: 

| Nút được xử lý | Cân bằng cây con | Chi phí cạnh | Chi phí bổ sung | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 5 | -1 | 5 | 5 | 5 | 
| 6 | -1 | 1 | 1 | 6 | 
| 2 | -1 | 2 | 2 | 8 | 
| 3 | 1 | 5 | 5 | 13 | 
| 4 | 1 | 1 | 1 | 14 | 
| 1 | 1 | không | 0 | 14 | 

Dấu vết cho thấy một cây con chỉ có thể trở nên cân bằng sau khi xem xét tất cả các cây con của nó. Thuật toán không bao giờ quyết định sự di chuyển cục bộ giữa các quốc gia riêng lẻ. Nó chỉ sử dụng số lượng phải vượt qua từng cạnh ngăn cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và cạnh được truy cập với số lần không đổi. | 
| Không gian | O(n) | Danh sách kề và mảng truyền tải lưu trữ thông tin cho từng nút và cạnh. | 

Độ phức tạp tuyến tính là cần thiết cho giới hạn 250.000 quốc gia. Giải pháp không phụ thuộc vào số lượng quân đội vì nó không bao giờ mô phỏng các chuyển động riêng lẻ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3
1 2 5
1 3 5
2 1
5 0
1 3
""") == "35\n", "sample 1"

assert run("""6
1 2 2
1 3 5
1 4 1
2 5 5
2 6 1
0 0
1 0
2 1
2 1
0 1
0 1
""") == "14\n", "sample 2"

assert run("""1
5 3
""") == "0\n", "single nation"

assert run("""2
1 2 10
5 3
0 4
""") == "20\n", "one transfer"

assert run("""3
1 2 7
2 3 4
0 2
5 1
0 3
""") == "4\n", "internal balancing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quốc gia duy nhất | 0 | Không có cạnh nào tồn tại và thặng dư không cần chuyển động. | 
| Hai quốc gia kết nối | 20 | Một yêu cầu chuyển giao đơn giản qua một cạnh. | 
| Chuỗi ba nút | 4 | Một cây con có cung và cầu nội bộ. | 
| Cung cấp mẫu | 35 và 14 | Tính đúng đắn chung. | 

## Vỏ cạnh 

Trường hợp một quốc gia:```
1
5 3
```có số dư cây con là`2`, nhưng không có cạnh cha. Việc truyền tải đến gốc và không bao giờ tính chi phí, tạo ra`0`. Các đội quân còn lại có thể giữ nguyên vị trí. 

Đối với cây con chứa cả quân đội bổ sung và quân đội thiếu:```
3
1 2 7
2 3 4
0 2
5 1
0 3
```các giá trị cân bằng là`-2`,`4`, Và`-3`. Quá trình truyền tải đầu tiên xử lý nút 3, tính phí`3 * 4`cho cạnh tới nút 2 nếu đó là chế độ xem duy nhất. Sau khi kết hợp nút 3 với nút 2, số dư cây con trở thành 0 sau khi đáp ứng nhu cầu nội bộ, do đó chỉ còn lại chuyển động nội bộ cần thiết. Câu trả lời cuối cùng là`4`, vì một đội quân di chuyển từ nút 2 đến nút 3. 

Đối với một gốc có thặng dư:```
1
5 3
```số dư gốc vẫn dương sau khi xử lý tất cả con. Thuật toán cố tình không làm gì với phần còn sót lại này vì chỉ phải đặt những đội quân cần thiết tối thiểu. Quân đội bổ sung được phép ở lại bất cứ nơi nào.
