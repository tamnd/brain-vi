---
title: "CF 150B - Số lượng dây"
description: "Chúng ta muốn đếm các chuỗi có độ dài n được xây dựng từ một bảng chữ cái có kích thước m, với một hạn chế mạnh: mọi chuỗi con có độ dài k phải là một bảng màu."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 150
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 107 (Div. 1)"
rating: 1600
weight: 150
solve_time_s: 100
verified: true
draft: false
---

[CF 150B - Số lượng dây](https://codeforces.com/problemset/problem/150/B) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, dfs và tương tự, đồ thị, toán học 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi muốn đếm các chuỗi có độ dài`n`được xây dựng từ một bảng chữ cái có kích thước`m`, dưới một hạn chế mạnh: mọi chuỗi con có độ dài`k`phải là một palindrome. 

Một chuỗi con có độ dài`k`bắt đầu ở vị trí`i`chứa các ký tự$$s_i, s_{i+1}, \dots, s_{i+k-1}$$và là một palindrome có nghĩa là$$s_i = s_{i+k-1}, \quad s_{i+1} = s_{i+k-2}, \dots$$Nhiệm vụ là đếm xem có bao nhiêu chuỗi đầy đủ thỏa mãn tất cả các ràng buộc đó cùng một lúc. Vì con số có thể trở nên rất lớn nên chúng ta đưa ra câu trả lời theo modulo$10^9 + 7$. 

Các giới hạn đủ nhỏ để cho phép xây dựng biểu đồ hoặc DFS trên các vị trí. Với`n, k ≤ 2000`, MỘT$O(n^2)$thuật toán hoàn toàn an toàn. Giải pháp hình khối sẽ bắt đầu trở nên rủi ro, đặc biệt là trong Python. Việc liệt kê tất cả các chuỗi là không thể vì có$m^n$các ứng cử viên, và thậm chí đối với`n = 50`con số đó đã lớn về mặt thiên văn rồi. 

Phần khó khăn là các điều kiện palindrome từ các chuỗi con chồng chéo sẽ tương tác với nhau. Một điều kiện địa phương có thể buộc các sự bình đẳng cách nhau rất xa. 

Hãy xem xét ví dụ này:```
n = 5, k = 3
```Các chuỗi con là:```
s[0..2], s[1..3], s[2..4]
```Từ bảng màu đầu tiên ta có:```
s0 = s2
```Từ thứ hai:```
s1 = s3
```Từ thứ ba:```
s2 = s4
```Kết hợp chúng mang lại:```
s0 = s2 = s4
s1 = s3
```Vì vậy toàn bộ chuỗi chỉ được xác định bởi hai nhóm ký tự độc lập. 

Một lỗi phổ biến là xử lý từng chuỗi con một cách độc lập và nhân lên các khả năng. Điều đó vượt quá số lượng vì các vị trí giống nhau xuất hiện trong nhiều chuỗi con. 

Một trường hợp cạnh tinh tế khác là`k = 1`. Mỗi chuỗi con có độ dài`1`tự động là một bảng màu, do đó không có ràng buộc bình đẳng nào cả. 

Ví dụ:```
n = 4, m = 3, k = 1
```Mọi chuỗi đều hoạt động, vì vậy câu trả lời là:```
3^4 = 81
```Việc triển khai bất cẩn luôn hợp nhất các vị trí bên trong cửa sổ có thể vô tình thêm các ràng buộc ngay cả khi không tồn tại. 

Cực đoan ngược lại là`k = n`. Khi đó toàn bộ chuỗi phải là một palindrome. 

Ví dụ:```
n = 5, m = 2, k = 5
```Chúng tôi cần:```
s0 = s4
s1 = s3
```Chỉ có ba vị trí độc lập nên đáp án là:```
2^3 = 8
```Hiểu rằng vấn đề thực sự là về mối quan hệ bình đẳng giữa các chỉ số là quan sát chính. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo mọi chuỗi có độ dài`n`, sau đó kiểm tra xem mọi chuỗi con có độ dài`k`là một palindrome. 

có$m^n$dây. Đối với mỗi chuỗi, chúng tôi kiểm tra đại khái`n`chuỗi con và mỗi lần kiểm tra palindrome có giá lên tới`k`. 

Điều đó mang lại:$$O(m^n \cdot n \cdot k)$$điều này là vô vọng ngay cả đối với các giá trị vừa phải. 

Lý do vũ lực hoạt động về mặt khái niệm là vì điều kiện dễ dàng được xác minh cục bộ. Vấn đề là số lượng chuỗi ứng cử viên bùng nổ theo cấp số nhân. 

Quan sát quan trọng là các ràng buộc palindrom chỉ buộc một số vị trí phải bằng nhau. Họ không bao giờ hạn chế ký tự thực tế nào được sử dụng. 

Giả sử một chuỗi con có độ dài`k`bắt đầu lúc`i`. Sau đó với mọi phần bù`j`:$$s_{i+j} = s_{i+k-1-j}$$Vì vậy, các vị trí trở nên được kết nối bởi các mối quan hệ bình đẳng. 

Khi hai vị trí được biết là bằng nhau, chúng hoạt động như một biến duy nhất. Sau khi xử lý tất cả các chuỗi con, chuỗi được phân chia thành các thành phần được kết nối có vị trí bằng nhau. 

Nếu có`c`thành phần độc lập thì mỗi thành phần có thể chọn bất kỳ thành phần nào trong`m`các chữ cái một cách độc lập. 

Vì vậy, câu trả lời trở thành:$$m^c$$modulo$10^9+7$. 

Điều này chuyển đổi vấn đề từ việc xây dựng các chuỗi thành việc đếm các thành phần được kết nối trong biểu đồ. 

Chúng ta có thể mô hình hóa các vị trí`0 ... n-1`như các đỉnh đồ thị. Bất cứ khi nào một điều kiện palindrome cho biết hai vị trí phải khớp nhau, chúng ta sẽ kết nối chúng. Sau đó, chúng tôi tính toán xem còn lại bao nhiêu thành phần được kết nối. 

DFS, BFS hoặc DSU đều hoạt động. Vì các ràng buộc rất nhỏ nên một giải pháp DFS đơn giản là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m^n \cdot n \cdot k)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(nk)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo biểu đồ với`n`đỉnh, một đỉnh cho mỗi vị trí chuỗi. 
2. Với mọi chuỗi con có độ dài`k`, thêm các cạnh đẳng thức được ngụ ý bởi điều kiện palindrome. 

Nếu chuỗi con bắt đầu tại`i`, sau đó:$$s_{i+j} = s_{i+k-1-j}$$cho tất cả hợp lệ`j`. 

Vì vậy, chúng tôi kết nối hai chỉ số đó trong biểu đồ. 
3. Sau khi xử lý tất cả các chuỗi con, các vị trí được kết nối qua bất kỳ đường dẫn nào đều phải chứa cùng một ký tự. 

Điều này hiệu quả vì sự bình đẳng có tính chất bắc cầu. Nếu như`a = b`Và`b = c`, sau đó`a = c`. 
4. Chạy DFS trên biểu đồ để đếm các thành phần được kết nối. 

Mỗi thành phần được kết nối tương ứng với một lựa chọn ký tự độc lập. 
5. Tính toán:$$m^{\text{components}} \bmod (10^9+7)$$sử dụng lũy ​​thừa mô đun nhanh. 

### Tại sao nó hoạt động 

Mọi ràng buộc palindrome có thể được viết dưới dạng tập hợp các đẳng thức giữa các cặp vị trí. Biểu đồ lưu trữ chính xác những đẳng thức này. 

Nếu hai vị trí thuộc cùng một thành phần liên thông thì tồn tại một chuỗi quan hệ bình đẳng kết nối chúng nên chúng phải chứa cùng một ký tự. 

Nếu hai vị trí nằm trong các thành phần khác nhau thì không có ràng buộc nào liên kết chúng nên chúng có thể được chọn độc lập. 

Do đó, mỗi thành phần được kết nối hoạt động giống như một biến tự do với`m`sự lựa chọn. Nhân trên tất cả các thành phần sẽ cho:$$m^c$$Ở đâu`c`là số thành phần được kết nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    n, m, k = map(int, input().split())

    graph = [[] for _ in range(n)]

    # Add palindrome equality constraints
    for start in range(n - k + 1):
        l = start
        r = start + k - 1

        while l < r:
            graph[l].append(r)
            graph[r].append(l)
            l += 1
            r -= 1

    visited = [False] * n

    def dfs(node):
        stack = [node]
        visited[node] = True

        while stack:
            u = stack.pop()

            for v in graph[u]:
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)

    components = 0

    for i in range(n):
        if not visited[i]:
            components += 1
            dfs(i)

    print(pow(m, components, MOD))

if __name__ == "__main__":
    main()
```Biểu đồ lưu trữ các ràng buộc bình đẳng giữa các vị trí. Đối với mọi chuỗi con có độ dài`k`, chúng tôi kết nối các chỉ mục được phản chiếu vì các vị trí đó phải khớp với nhau trong một bảng màu. 

Vòng lặp:```
while l < r:
```là quan trọng. Chúng tôi chỉ kết nối các cặp được nhân đôi một lần. Khi`k`là số lẻ, ký tự ở giữa không tạo ra bất kỳ ràng buộc nào vì nó phản chiếu chính nó. 

DFS tính toán các thành phần được kết nối. Mỗi nút không được truy cập sẽ bắt đầu một thành phần mới, đại diện cho một nhóm ký tự độc lập. 

Việc sử dụng DFS lặp sẽ tránh được những lo ngại về độ sâu đệ quy, mặc dù đệ quy cũng sẽ phù hợp với các giới hạn này. 

Câu trả lời cuối cùng sử dụng lũy ​​thừa mô-đun tích hợp của Python:```
pow(m, components, MOD)
```chạy theo thời gian logarit. 

Một chi tiết triển khai tinh tế là lập chỉ mục. Báo cáo vấn đề đương nhiên dựa trên 1, nhưng mã sử dụng chỉ mục dựa trên 0 xuyên suốt. Chuỗi con`[start, start + k - 1]`phải xử lý cẩn thận để tránh nhầm lẫn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1
```Chỉ có một vị trí và một nhân vật có thể. 

| Bước | Hành động | Đồ thị | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | Tạo nút 0 | Không có cạnh | 0 | 
| 2 | Không cần cặp palindrom | Không có cạnh | 0 | 
| 3 | DFS từ nút 0 | Một thành phần | 1 | 
| 4 | Tính toán$1^1$| Câu trả lời cuối cùng | 1 | 

Dấu vết cho thấy`k = 1`trường hợp cạnh. Không có ràng buộc bình đẳng nào được thêm vào, vì vậy mọi vị trí vẫn độc lập. 

### Ví dụ 2 

đầu vào:```
5 2 3
```Chuỗi con có độ dài 3:```
[0,1,2]
[1,2,3]
[2,3,4]
```Các ràng buộc của Palindrom:```
0 = 2
1 = 3
2 = 4
```| Bước | Đã thêm cạnh | Nhóm được kết nối | 
| --- | --- | --- | 
| 1 | (0, 2) | {0,2} | 
| 2 | (1, 3) | {1,3} | 
| 3 | (2, 4) | {0,2,4}, {1,3} | 

Bây giờ có chính xác hai thành phần được kết nối. 

Vì vậy, câu trả lời là:$$2^2 = 4$$Các chuỗi hợp lệ là:```
ababa
babab
aaaaa
bbbbb
```Ví dụ này cho thấy các chuỗi con chồng chéo truyền các đẳng thức qua khoảng cách xa như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk)$| Mỗi chuỗi con đóng góp nhiều nhất`k/2`cạnh bình đẳng | 
| Không gian |$O(n^2)$| Đồ thị có thể chứa$O(nk)$cạnh trong trường hợp xấu nhất | 

Với`n ≤ 2000`, đồ thị vẫn nhỏ thoải mái. Ngay cả trong những trường hợp dày đặc nhất, tổng công việc chỉ là vài triệu thao tác, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    graph = [[] for _ in range(n)]

    for start in range(n - k + 1):
        l = start
        r = start + k - 1

        while l < r:
            graph[l].append(r)
            graph[r].append(l)
            l += 1
            r -= 1

    visited = [False] * n

    def dfs(node):
        stack = [node]
        visited[node] = True

        while stack:
            u = stack.pop()

            for v in graph[u]:
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)

    components = 0

    for i in range(n):
        if not visited[i]:
            components += 1
            dfs(i)

    print(pow(m, components, MOD))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup

    return out.getvalue().strip()

# provided sample
assert run("1 1 1\n") == "1", "sample 1"

# k = 1, every string valid
assert run("4 3 1\n") == "81", "all strings allowed"

# full string must be palindrome
assert run("5 2 5\n") == "8", "entire string palindrome"

# overlapping constraints create one component
assert run("4 10 2\n") == "10", "adjacent equality chain"

# odd palindrome propagation
assert run("5 2 3\n") == "4", "two independent groups"

# maximum-style stress shape
assert run("2000 1 2000\n") == "1", "single alphabet letter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 3 1`|`81`| Không có ràng buộc khi`k = 1`| 
|`5 2 5`|`8`| Hành vi palindrome toàn chuỗi | 
|`4 10 2`|`10`| Các đẳng thức liền kề hợp nhất mọi thứ | 
|`5 2 3`|`4`| Các ràng buộc truyền bá cửa sổ chồng chéo | 
|`2000 1 2000`|`1`| Giá trị biên lớn | 

## Vỏ cạnh 

Hãy xem xét:```
4 3 1
```Mỗi chuỗi con có độ dài`1`tự động là một palindrome. Đồ thị không nhận được cạnh nào cả. 

DFS thấy bốn thành phần riêng biệt:```
{0}, {1}, {2}, {3}
```Vì vậy, câu trả lời là:$$3^4 = 81$$Điều này xác nhận thuật toán không vô tình đưa ra các ràng buộc khi`k = 1`. 

Bây giờ hãy xem xét:```
5 2 5
```Toàn bộ chuỗi phải là một palindrome. 

Thuật toán bổ sung:```
0 = 4
1 = 3
```Chức vụ`2`ở lại một mình. 

Các thành phần được kết nối trở thành:```
{0,4}, {1,3}, {2}
```Vì vậy, có ba lựa chọn độc lập:$$2^3 = 8$$Điều này xác minh việc xử lý chính xác khi bảng màu kéo dài toàn bộ chuỗi. 

Cuối cùng, hãy xem xét trường hợp các đẳng thức lan truyền bắc cầu:```
4 10 2
```Mỗi chuỗi con có độ dài`2`phải là một bảng màu, nghĩa là các ký tự liền kề phải khớp nhau. 

Các cạnh của đồ thị là:```
0-1
1-2
2-3
```DFS hợp nhất mọi thứ thành một thành phần:```
{0,1,2,3}
```Chỉ còn lại một lựa chọn nhân vật, vì vậy câu trả lời là:$$10^1 = 10$$Điều này xác nhận rằng các thành phần được kết nối nắm bắt chính xác đẳng thức bắc cầu.
