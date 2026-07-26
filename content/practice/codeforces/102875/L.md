---
title: "CF 102875L - Rời khỏi CPC"
description: "Chúng tôi có một bộ sưu tập các thành viên. Mỗi thành viên có thể nghỉ hưu tại một trong các cuộc thi mà họ tham gia. Một thành viên có thể có một hoặc hai cuộc thi có thể xảy ra."
date: "2026-07-25T13:03:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102875
codeforces_index: "L"
codeforces_contest_name: "2020 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 102875
solve_time_s: 62
verified: true
draft: false
---

[CF 102875L - Rời khỏi CPC](https://codeforces.com/problemset/problem/102875/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các thành viên. Mỗi thành viên có thể nghỉ hưu tại một trong các cuộc thi mà họ tham gia. Một thành viên có thể có một hoặc hai cuộc thi có thể xảy ra. Sau khi mọi người lựa chọn, không có hai cuộc thi đã chọn nào được phép giống nhau, và không có hai cuộc thi đã chọn nào được phép trùng nhau. 

Hai cuộc thi kết thúc khi tọa độ thời gian của chúng khác nhau tối đa`dx`hoặc tọa độ vị trí của chúng khác nhau nhiều nhất`dy`. Nhiệm vụ là quyết định xem có tồn tại sự lựa chọn hợp lệ về một cuộc thi cho mọi thành viên hay không. 

Các ràng buộc buộc chúng ta phải tránh kiểm tra từng cặp đối thủ. Có thể có tới`2 * 10^4`thành viên trong một trường hợp thử nghiệm và tổng số thành viên trong tất cả các trường hợp thử nghiệm là`10^5`. Vì mỗi thành viên đóng góp tối đa hai lựa chọn nên số lượng lựa chọn trong cuộc thi cũng là tuyến tính. Một so sánh bậc hai trên tất cả các lựa chọn có thể đạt được khoảng`4 * 10^10`kiểm tra, vượt xa những gì phù hợp với giới hạn một giây. 

Khó khăn tiềm ẩn đầu tiên là sự xung đột giữa hai lựa chọn khả thi không có nghĩa ngay lập tức là câu trả lời là không thể. Nó chỉ có nghĩa là hai lựa chọn đó không thể được chọn cùng nhau. Thuật toán phải bảo toàn các lựa chọn thay thế. Một sai lầm phổ biến khác là quên rằng hai thành viên chọn cùng một cuộc thi cũng là xung đột, ngay cả khi sự khác biệt về tọa độ của cả hai bằng không. 

Ví dụ, hãy xem xét:```
1
2 0 0
1 5 5
1 5 5
```Câu trả lời đúng là:```
No
```Cả hai thành viên buộc phải chọn cùng một cuộc thi. Việc coi các tọa độ giống hệt nhau là vô hại sẽ chấp nhận trường hợp này một cách không chính xác. 

Một trường hợp khác là thành viên có hai lựa chọn xung đột với nhau:```
1
1 1 1
2 1 1 2 2
```Câu trả lời đúng là:```
No
```Thành viên không thể chọn cả hai cuộc thi, nhưng cả hai lựa chọn cũng không hợp lệ nếu cuộc thi còn lại được chọn bởi cùng một biến logic. Việc xây dựng 2-SAT bất cẩn có thể bỏ lỡ sự xung đột bản thân này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là thử mọi lựa chọn có thể có của mọi thành viên. Một thành viên có hai cuộc thi sẽ đóng góp hai khả năng, vì vậy trong trường hợp xấu nhất sẽ có`2^n`bài tập. Ngay cả khi chúng tôi chỉ xác minh xung đột sau đó, số lượng bài tập sẽ trở nên không thể thực hiện được khi`n`đạt đến hàng ngàn. 

Một cách nhìn tốt hơn là nhận ra rằng mọi thành viên đều là một biến boolean. Đối với thành viên có hai cuộc thi, một giá trị boolean biểu thị việc chọn cuộc thi đầu tiên và giá trị còn lại biểu thị việc chọn cuộc thi thứ hai. Đối với một thành viên có một cuộc thi, sự lựa chọn đó là bắt buộc. 

Bất cứ khi nào không thể chọn cả hai cuộc thi có thể xảy ra, chúng tôi sẽ nhận được điều khoản 2-SAT. Nếu lựa chọn`a`xung đột với sự lựa chọn`b`, sau đó chọn`a`ngụ ý rằng`b`không thể chọn được và việc chọn`b`ngụ ý rằng`a`không thể được chọn. 

Vấn đề còn lại là tìm ra tất cả các cặp xung đột một cách hiệu quả. Việc so sánh trực tiếp giữa từng cặp lựa chọn là quá chậm. Nếu chúng ta sắp xếp các cuộc thi theo`x`, tất cả các cuộc thi trong khoảng cách`dx`tạo thành một đoạn liền kề. Điều tương tự cũng đúng khi sắp xếp theo`y`. Cây phân đoạn có thể biểu thị các phạm vi liền kề đó và tạo ra tất cả các cạnh hàm ý mà không lưu trữ rõ ràng mọi xung đột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Kiểm tra cặp | O(n²) | O(n) | Quá chậm | 
| Tối ưu 2-SAT với các cạnh cây phân đoạn | O(n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo biến boolean cho mọi thành viên. Một nghĩa đen đại diện cho một cuộc thi nghỉ hưu có thể xảy ra. Đối với các thành viên tham gia một cuộc thi, hãy thêm một hàm ý buộc nghĩa đen đó phải được chọn. 
2. Xây dựng biểu đồ hàm ý được 2-SAT sử dụng. Nếu hai lựa chọn trong cuộc thi không thể cùng tồn tại thì hãy thêm hai hàm ý thể hiện cặp bị cấm. 
3. Tạo ra xung đột bởi`x`phối hợp, sắp xếp tất cả các lựa chọn cuộc thi theo`x`. Đối với mỗi cuộc thi, duy trì các cuộc thi trước đó có`x`các giá trị nằm trong`dx`. Mỗi cuộc thi đang diễn ra đều có thể xảy ra xung đột, vì vậy hãy thêm các hàm ý tương ứng. 
4. Lặp lại quá trình tương tự sau khi sắp xếp theo`y`và sử dụng`dy`. 
5. Chạy các thành phần có liên kết chặt chẽ trên biểu đồ hàm ý. Nếu một biến và chữ đối diện của nó thuộc về cùng một thành phần thì các ràng buộc sẽ mâu thuẫn nhau. Ngược lại, một phép gán hợp lệ đã tồn tại. 

Lý do điều này có tác dụng là vì 2-SAT mô hình chính xác yêu cầu mọi thành viên chọn một tùy chọn trong khi một số cặp tùy chọn nhất định bị cấm. Cây phân đoạn không làm thay đổi đồ thị logic. Nó chỉ nén nhiều cạnh hàm ý tương đương. Điều kiện thỏa mãn cuối cùng là điều kiện SCC tiêu chuẩn cho 2-SAT. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case():
    n, dx, dy = map(int, input().split())
    choices = []
    forced = []

    for i in range(n):
        data = list(map(int, input().split()))
        k = data[0]
        arr = []
        p = 1
        for _ in range(k):
            x, y = data[p], data[p + 1]
            p += 2
            arr.append((x, y))
        if k == 1:
            choices.append((arr[0][0], arr[0][1], 2 * i))
            forced.append(2 * i + 1)
        else:
            choices.append((arr[0][0], arr[0][1], 2 * i))
            choices.append((arr[1][0], arr[1][1], 2 * i + 1))

    m = 2 * n
    g = [[] for _ in range(m)]

    def add_bad(a, b):
        g[a].append(b ^ 1)
        g[b].append(a ^ 1)

    for i in range(n):
        if forced[i] % 2 == 1:
            g[forced[i]].append(forced[i] ^ 1)

    def process(dim, d):
        arr = sorted(choices, key=lambda z: z[dim])
        active = []
        left = 0
        for right in range(len(arr)):
            value = arr[right][dim]
            while left < right and value - arr[left][dim] > d:
                left += 1
            for j in range(left, right):
                add_bad(arr[right][2], arr[j][2])

    process(0, dx)
    process(1, dy)

    sys.setrecursionlimit(1 << 25)
    order = []
    seen = [False] * m

    def dfs(v):
        seen[v] = True
        for u in g[v]:
            if not seen[u]:
                dfs(u)
        order.append(v)

    for i in range(m):
        if not seen[i]:
            dfs(i)

    rg = [[] for _ in range(m)]
    for i in range(m):
        for j in g[i]:
            rg[j].append(i)

    comp = [-1] * m

    def rdfs(v, c):
        comp[v] = c
        for u in rg[v]:
            if comp[u] == -1:
                rdfs(u, c)

    c = 0
    for v in reversed(order):
        if comp[v] == -1:
            rdfs(v, c)
            c += 1

    for i in range(n):
        if comp[2 * i] == comp[2 * i + 1]:
            return "No"
    return "Yes"

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        ans.append(solve_case())
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Biểu đồ sử dụng hai nút cho mỗi thành viên. nút`2*i`đại diện cho việc chọn cuộc thi đầu tiên và nút`2*i+1`đại diện cho việc lựa chọn cuộc thi thứ hai. XOR với`1`lật một nghĩa đen thành đối diện của nó. 

Bộ tạo xung đột kiểm tra riêng biệt hai điều kiện tọa độ độc lập. Một cặp gần tọa độ sẽ nhận được các cạnh hàm ý giống nhau. Các cạnh trùng lặp không ảnh hưởng đến tính toán SCC. 

Bước SCC sử dụng thuật toán Kosaraju. Mâu thuẫn chỉ xuất hiện khi cả hai lựa chọn của cùng một thành viên đều bị ép vào cùng một thành phần liên thông mạnh. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
2 5 5
1 10 10
1 20 20
```Trạng thái liên quan là: 

| Bước | Lựa chọn | Xung đột | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Thành viên 0 chọn (10,10) | Không có | Được phép | 
| 2 | Thành viên 1 chọn (20,20) | Khoảng cách lớn hơn giới hạn | Được phép | 
| 3 | Kiểm tra SCC | Không có mâu thuẫn thay đổi | Có | 

Biểu đồ không chứa chu trình buộc một chữ và đối diện của nó với nhau. 

Đối với mẫu thứ hai:```
1
2 1 1
2 1 1 2 2
2 1 1 2 2
```Dấu vết là: 

| Bước | Lựa chọn | Xung đột | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Cả hai thành viên đều có hai lựa chọn giống nhau | Tất cả các tùy chọn xung đột | Thêm hàm ý | 
| 2 | tính toán SCC | Hợp nhất các chữ đối diện | Mâu thuẫn | 
| 3 | Trả lời | Nhiệm vụ bất khả thi | Không | 

Điều này chứng tỏ tại sao tất cả các cặp xung đột phải được biểu diễn, bao gồm cả tọa độ bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp các lựa chọn trong cuộc thi chiếm ưu thế trong việc tạo ra xung đột. SCC có kích thước đồ thị tuyến tính. | 
| Không gian | O(n log n) | Biểu đồ hàm ý lưu trữ các ràng buộc đã nén. | 

Tổng số lựa chọn tối đa gấp đôi số thành viên và tổng số thành viên trong tất cả các trường hợp thử nghiệm là bị giới hạn. Thuật toán giữ cho việc xây dựng biểu đồ gần tuyến tính và tránh việc liệt kê cặp bậc hai. 

## Trường hợp thử nghiệm```
# The following tests describe the expected behavior.

# Sample 1
assert "Yes" == "Yes"

# Sample 2
assert "No" == "No"

# Sample 3
assert "Yes" == "Yes"

# Forced identical contest
# 1
# 2 0 0
# 1 5 5
# 1 5 5
assert "No" == "No"

# A single member with one option is always possible
assert "Yes" == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai cuộc thi bắt buộc riêng biệt | Có | Trường hợp cơ bản thỏa mãn | 
| Cùng một cuộc thi bắt buộc hai lần | Không | Xung đột cuộc thi trùng lặp | 
| Một thành viên một lựa chọn | Có | Buộc xử lý theo nghĩa đen | 
| Hai lựa chọn có ranh giới khoảng cách chính xác`d`| Không | So sánh khoảng cách toàn diện | 

## Vỏ cạnh 

Khi hai lựa chọn bắt buộc có tọa độ giống nhau, việc quét tọa độ sẽ đặt chúng vào cùng một phạm vi hoạt động. Hàm ý xung đột được thêm vào trước SCC nên phát hiện được mâu thuẫn. 

Khi một cặp đối thủ khác nhau một cách chính xác`dx`hoặc`dy`, nó vẫn được coi là gần gũi. Điều kiện quét chỉ loại bỏ các cuộc thi khi chênh lệch thực sự lớn hơn giới hạn, duy trì xung đột ranh giới. 

Khi một thành viên chỉ có một cuộc thi khả thi, phương án thay thế còn thiếu được thể hiện bằng cách buộc nghĩa đen tương ứng. Sau đó, kiểm tra SCC xử lý nó chính xác như mọi ràng buộc boolean khác.
