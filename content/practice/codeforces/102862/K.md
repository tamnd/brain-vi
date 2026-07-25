---
title: "CF 102862K - Chuỗi nhị phân"
description: "Bài toán mô tả một mảng nhị phân có vị trí có thể được xem như các đỉnh của đồ thị. Mỗi thao tác được phép là một cạnh: sử dụng cạnh đó sẽ lật hai bit tại điểm cuối của nó."
date: "2026-07-25T13:56:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102862
codeforces_index: "K"
codeforces_contest_name: "LU ICPC Selection Contest 2020 and KFU Open Contest 2020"
rating: 0
weight: 102862
solve_time_s: 42
verified: true
draft: false
---

[CF 102862K - Chuỗi nhị phân](https://codeforces.com/problemset/problem/102862/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bài toán mô tả một mảng nhị phân có vị trí có thể được xem như các đỉnh của đồ thị. Mỗi thao tác được phép là một cạnh: sử dụng cạnh đó sẽ lật hai bit tại điểm cuối của nó. Bắt đầu từ một mảng gồm tất cả các số 0, chúng ta cần quyết định xem có thể tạo ra cấu hình bit mục tiêu nhất định hay không. 

Đầu vào cung cấp số lượng vị trí, bit cuối cùng mong muốn và danh sách các cặp vị trí có thể được đảo ngược cùng nhau. Đầu ra là liệu một số thứ tự và sự lặp lại của các lần lật cặp này có thể tạo ra chính xác mảng mục tiêu hay không. 

Các giới hạn đủ lớn để các hoạt động mô phỏng không thực tế. Với tối đa$10^5$vị trí và$10^5$hoạt động, ngay cả một cách tiếp cận liên tục thử các hoạt động sẵn có cũng có thể đạt được$10^{10}$làm việc trong trường hợp xấu nhất. Giải pháp cần phải gần với tuyến tính, điều này gợi ý việc tìm kiếm thuộc tính cấu trúc của biểu đồ thay vì xây dựng chuỗi các thao tác. 

Các trường hợp cạnh chính đến từ các thành phần đồ thị. Xét một đỉnh cô lập:```
n = 2
target = [1, 0]
m = 0
```Câu trả lời là`No`. Vị trí đầu tiên không có cạnh nào nối với nó, vì vậy không có thao tác nào có thể thay đổi nó từ 0. 

Trường hợp thứ hai là thành phần trong đó tất cả các đỉnh được kết nối nhưng mục tiêu có tính chẵn lẻ lẻ:```
n = 3
target = [1, 1, 1]
m = 2
edges:
1 2
2 3
```Câu trả lời là`No`. Mỗi thao tác lật chính xác hai bit, do đó số lượng bit bên trong thành phần này luôn thay đổi một lượng chẵn modulo hai. Thành phần này bắt đầu bằng số 0, vì vậy nó chỉ có thể kết thúc với số lượng số chẵn. 

Việc triển khai bất cẩn chỉ có thể kiểm tra xem toàn bộ mảng có số chẵn hay không. Điều đó không thành công khi biểu đồ có nhiều thành phần:```
n = 4
target = [1, 1, 1, 0]
m = 1
edge:
1 2
```Câu trả lời là`No`. Thành phần đầu tiên có hai thành phần và có thể, nhưng đỉnh 3 là thành phần riêng biệt với một thành phần, không thể thay đổi. 

# Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử áp dụng các thao tác và theo dõi mọi mảng nhị phân có thể truy cập. Vì mỗi bit có hai trạng thái nên có thể có$2^n$mảng có thể. Ngay cả đối với$n=40$, giá trị này đã quá lớn và giới hạn đã cho của$10^5$làm cho việc thăm dò trạng thái là không thể. 

Một cách tiếp cận trực tiếp khác là coi mọi phép toán như một phương trình trên các giá trị nhị phân. Mỗi cạnh cho biết rằng việc áp dụng nó sẽ chuyển đổi cả hai điểm cuối, vì vậy trạng thái cuối cùng là sự kết hợp XOR của các cạnh được chọn. Việc giải hệ thống tuyến tính đầy đủ bằng phép loại bỏ Gaussian tổng quát sẽ có hiệu quả về mặt khái niệm, nhưng nó không cần thiết và sẽ phức tạp hơn cấu trúc đồ thị yêu cầu. 

Quan sát quan trọng là một thao tác không bao giờ thay đổi tính chẵn lẻ của số lượng đơn vị bên trong một thành phần được kết nối. Cả hai điểm cuối của một cạnh đều thuộc cùng một thành phần và việc lật cả hai bit sẽ thay đổi số lượng một thành cộng hai, trừ hai hoặc bằng 0. Trong số học modulo hai, tính chẵn lẻ của thành phần không thay đổi. 

Điều kiện này cũng đủ. Bên trong một thành phần được kết nối, bất kỳ cặp đỉnh nào cũng có thể được chuyển đổi với nhau một cách hiệu quả bằng cách đi dọc theo các đường dẫn trong thành phần đó. Bằng cách kết hợp các hoạt động đường dẫn như vậy, chúng ta có thể tạo bất kỳ cấu hình nào có tính chẵn lẻ. Một thành phần có ít nhất một cạnh có thể tạo ra mọi phép gán chẵn lẻ, trong khi một đỉnh bị cô lập không bao giờ có thể thay đổi được. 

Vấn đề giảm xuống còn việc tìm các thành phần được kết nối, đếm số lượng mục tiêu trong mỗi thành phần và kiểm tra các điều kiện chẵn lẻ này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị vô hướng trong đó mọi phép toán được phép đều trở thành cạnh giữa hai vị trí của nó. Các phép toán chỉ phụ thuộc vào đỉnh nào được kết nối, do đó thứ tự chính xác của các cạnh không quan trọng. 
2. Tìm mọi thành phần được kết nối bằng DFS hoặc BFS. Trong khi truy cập một thành phần, hãy đếm xem có bao nhiêu bit mục tiêu bằng một và đếm xem có bao nhiêu đỉnh thuộc về thành phần đó. 
3. Nếu một thành phần chỉ chứa một đỉnh và bit mục tiêu của nó là một, hãy loại bỏ nó ngay lập tức. Không có thao tác nào chạm vào đỉnh đó nên nó phải bằng 0. 
4. Đối với mỗi thành phần có các cạnh, hãy kiểm tra số lượng các thành phần bên trong nó. Nếu số đó là số lẻ, hãy từ chối mục tiêu vì tính chẵn lẻ của thành phần không thể thay đổi. 
5. Nếu mọi thành phần đều vượt qua các bước kiểm tra này, hãy chấp nhận mục tiêu. 

Tại sao nó hoạt động: mỗi thao tác lật hai đỉnh từ cùng một thành phần được kết nối, do đó tính chẵn lẻ của các đỉnh trong mọi thành phần là bất biến. Bất biến đưa ra một điều kiện cần thiết. Để đầy đủ, một thành phần được kết nối cho phép chúng ta di chuyển chuyển đổi qua các đường dẫn và kết hợp các thao tác cạnh, có nghĩa là mọi tập hợp đỉnh có kích thước chẵn đều có thể được lật. Vì mọi mục tiêu thành phần hợp lệ đều có tính chẵn lẻ nên tất cả các mục tiêu như vậy đều có thể truy cập được. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    b = list(map(int, input().split()))
    
    m = int(input())
    graph = [[] for _ in range(n)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    visited = [False] * n

    for start in range(n):
        if not visited[start]:
            stack = [start]
            visited[start] = True
            ones = 0
            size = 0

            while stack:
                u = stack.pop()
                size += 1
                ones += b[u]

                for v in graph[u]:
                    if not visited[v]:
                        visited[v] = True
                        stack.append(v)

            if size == 1:
                if ones == 1:
                    print("No")
                    return
            else:
                if ones % 2 == 1:
                    print("No")
                    return

    print("Yes")

if __name__ == "__main__":
    solve()
```Việc xây dựng biểu đồ thể hiện trực tiếp các hoạt động có sẵn. Một cạnh không được định hướng vì việc đảo vị trí$i$Và$j$có tác dụng tương tự bất kể chúng ta mô tả cặp này như thế nào. 

Vòng lặp DFS xử lý một thành phần được kết nối tại một thời điểm. Biến`ones`lưu trữ thông tin chẵn lẻ quan trọng, trong khi`size`phân biệt các thành phần bình thường với các đỉnh bị cô lập. Trường hợp đỉnh bị cô lập cần xử lý riêng vì kiểm tra chẵn lẻ sẽ chấp nhận không chính xác một thành phần chứa một bit 0 và chỉ từ chối sau khi đếm, nhưng không bao giờ có thể tạo ra một bit duy nhất. 

Quá trình truyền tải sử dụng ngăn xếp rõ ràng thay vì đệ quy. Độ sâu đệ quy của Python quá nhỏ đối với biểu đồ chứa đường dẫn có độ dài$10^5$, do đó DFS lặp lại sẽ tránh được tình trạng tràn ngăn xếp. 

# Ví dụ đã hoạt động 

## Mẫu 1 

đầu vào:```
5
1 1 0 1 1
3
1 3
3 4
2 5
```Các thành phần là`{1,3,4}`Và`{2,5}`. 

| Bước | Thành phần | Kích thước | Những người trong mục tiêu | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | {1,3,4} | 3 | 2 | Hợp lệ, chẵn lẻ | 
| 2 | {2,5} | 2 | 2 | Hợp lệ, chẵn lẻ | 

Thuật toán chấp nhận vì mọi thành phần đều thỏa mãn bất biến. 

## Mẫu 2 

đầu vào:```
2
0 1
1
1 2
```| Bước | Thành phần | Kích thước | Những người trong mục tiêu | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | {1,2} | 2 | 1 | Không hợp lệ, chẵn lẻ | 

Thao tác duy nhất là lật cả hai bit lại với nhau, do đó hai vị trí phải luôn có tính chẵn lẻ bằng nhau. sản xuất`[0,1]`là không thể. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được thăm một lần trong quá trình truyền tải đồ thị | 
| Không gian | O(n + m) | Biểu đồ và mảng đã truy cập lưu trữ cấu trúc đầu vào | 

Các giới hạn cho phép xử lý tuyến tính vì thuật toán chỉ quét đồ thị một lần. Nó tránh mọi sự phụ thuộc vào số lượng cấu hình bit có thể có. 

# Trường hợp thử nghiệm```python
import sys, io

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

# sample 1
assert run("""5
1 1 0 1 1
3
1 3
3 4
2 5
""") == "Yes\n"

# sample 2
assert run("""2
0 1
1
1 2
""") == "No\n"

# isolated vertex cannot be changed
assert run("""3
0 1 0
1
1 3
""") == "No\n"

# disconnected components with correct parity
assert run("""4
1 1 1 1
2
1 2
3 4
""") == "Yes\n"

# all zero target is always possible
assert run("""5
0 0 0 0 0
1
1 2
""") == "Yes\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thành phần được kết nối đơn lẻ với các thành phần chẵn | Có | Cấu hình cơ bản có thể truy cập | 
| Thành phần liên thông với thành phần lẻ | Không | Bất biến chẵn lẻ | 
| Đỉnh cô lập được đặt thành một | Không | Đỉnh không thể thay đổi | 
| Nhiều thành phần có tính chẵn lẻ hợp lệ | Có | Kiểm tra thành phần khôn ngoan | 
| Mục tiêu hoàn toàn bằng không | Có | Không thực hiện thao tác | 

# Vỏ cạnh 

Đối với một đỉnh bị cô lập, thuật toán tạo ra một thành phần có kích thước bằng một. Đối với đầu vào:```
3
0 1 0
1
1 3
```các thành phần là`{1,3}`Và`{2}`. Thành phần thứ hai có một đỉnh và một đỉnh bắt buộc, do đó thuật toán trả về`No`. Nó không bao giờ cố gắng phát minh ra một phép toán cho đỉnh đó. 

Đối với thành phần được kết nối chẵn lẻ lẻ:```
3
1 1 1
2
1 2
2 3
```quá trình duyệt tìm thấy một thành phần chứa cả ba đỉnh. Số lượng mục tiêu là ba, số này là số lẻ nên thuật toán sẽ loại bỏ nó. Mọi thao tác sẵn có đều đảo ngược hai vị trí, giữ nguyên tính chẵn lẻ của thành phần. 

Đối với các biểu đồ bị ngắt kết nối, mỗi thành phần phải được kiểm tra riêng:```
4
1 1 1 0
1
1 2
```Thành phần đầu tiên`{1,2}`có hai cái và có giá trị. Các đỉnh còn lại bị cô lập và đỉnh thứ ba yêu cầu một đỉnh không có cạnh liên quan. Thuật toán loại bỏ tại thành phần đó, tránh lỗi thường gặp là chỉ kiểm tra tổng số thành phần.
