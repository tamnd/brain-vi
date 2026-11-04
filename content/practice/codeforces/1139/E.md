---
title: "CF 1139E - Tối đa hóa Mex"
description: "Mỗi học sinh thuộc về đúng một câu lạc bộ và có một giá trị tiềm năng. Vào một ngày nhất định, một số học sinh đã rời khỏi câu lạc bộ của mình vĩnh viễn. Từ những học sinh còn lại, chúng tôi có thể chọn tối đa một học sinh từ mỗi câu lạc bộ. Các học sinh được chọn thành lập đội dự thi."
date: "2026-06-12T03:52:25+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graph-matchings", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1139
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 548 (Div. 2)"
rating: 2400
weight: 1139
solve_time_s: 108
verified: true
draft: false
---

[CF 1139E - Tối đa hóa Mex](https://codeforces.com/problemset/problem/1139/E) 

**Đánh giá:** 2400 
**Tags:** luồng, đối sánh biểu đồ, đồ thị 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh thuộc về đúng một câu lạc bộ và có một giá trị tiềm năng. Vào một ngày nhất định, một số học sinh đã rời khỏi câu lạc bộ của mình vĩnh viễn. Từ những học sinh còn lại, chúng tôi có thể chọn tối đa một học sinh từ mỗi câu lạc bộ. Các học sinh được chọn thành lập đội dự thi. 

Sức mạnh của đội là tổng hợp những tiềm năng đã được lựa chọn. Chúng tôi muốn số lượng mex lớn nhất có thể đạt được. 

Ít nhất một mex`k`có nghĩa là mọi giá trị`0, 1, ..., k-1`xuất hiện trong số các sinh viên được chọn. Vì chúng tôi chỉ có thể nhận một học sinh từ mỗi câu lạc bộ nên câu hỏi thực sự không còn là về từng học sinh nữa. Đó là về việc liệu các giá trị`0, 1, ..., k-1`có thể được phân công vào các câu lạc bộ riêng biệt. 

Đầu vào mô tả các sinh viên ban đầu, sau đó là trình tự xóa. Sau lần xóa đầu tiên, chúng ta cần câu trả lời cho ngày 1, sau lần xóa thứ hai, câu trả lời cho ngày thứ 2, v.v. 

Những hạn chế là quan sát quan trọng. Có tối đa 5000 sinh viên và số lượng câu lạc bộ cũng nhiều nhất là 5000. Tiềm năng hoàn toàn dưới 5000. Một giải pháp tính toán lại mọi thứ từ đầu sau mỗi lần xóa sẽ cần khoảng 5000 phép tính độc lập trên biểu đồ có kích thước 5000, vượt xa giới hạn. Chúng ta cần sử dụng lại công việc giữa các ngày. 

Một trường hợp tế nhị là khi một số câu lạc bộ có thể cung cấp cùng một giá trị.```
Club 1: potential 0
Club 2: potential 0
Club 3: potential 1
```Mex là 2 vì giá trị 0 có thể đến từ câu lạc bộ 1 hoặc câu lạc bộ 2, trong khi giá trị 1 đến từ câu lạc bộ 3. Sự lựa chọn tham lam của câu lạc bộ có sẵn đầu tiên có thể tự đưa mình vào một góc nếu nó không cho phép chỉ định lại. 

Một sai lầm dễ mắc phải khác là coi học sinh thay vì các câu lạc bộ là mục tiêu phù hợp.```
Club 1: potentials {0,1}
Club 2: potentials {1}
```Giá trị 0 và 1 không thể cùng được lấy từ câu lạc bộ 1. Bất kỳ mô hình đúng nào cũng phải đảm bảo rằng mỗi câu lạc bộ đóng góp tối đa một học sinh được chọn. 

Tình huống không rõ ràng thứ ba xảy ra khi một học sinh bị xóa sau đó xuất hiện trở lại theo quy trình ngược lại. Mex tối đa có thể tăng lên do một cạnh mới xuất hiện trong biểu đồ khớp cơ bản. Việc tính toán lại toàn bộ kết quả khớp mỗi lần là không cần thiết, nhưng thuật toán vẫn phải có khả năng tìm ra các đường dẫn tăng cường sắp xếp lại các bài tập trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý mỗi ngày một cách độc lập. Sau khi áp dụng các thao tác xóa trong ngày, hãy xây dựng biểu đồ trong đó giá trị`x`được kết nối với mọi câu lạc bộ hiện có học sinh có tiềm năng`x`. Sau đó kiểm tra xem mex có ít nhất là 1, ít nhất là 2, ít nhất là 3, v.v. bằng cách sử dụng đối sánh lưỡng cực. 

Điều này đúng vì ít nhất mex`k`có nghĩa là giá trị`0..k-1`đều có thể được phân công vào các câu lạc bộ riêng biệt. Công thức so khớp nắm bắt chính xác điều kiện đó. 

Vấn đề là chi phí. Có tới 5000 ngày. Việc xây dựng lại và kết hợp lại mỗi ngày dẫn đến hàng chục hoặc hàng trăm triệu hoạt động khớp. 

Quan sát quan trọng là việc xóa là vĩnh viễn. Xử lý ngoại tuyến cho phép chúng tôi đảo ngược thời gian. 

Thay vì xóa từng học sinh một, hãy bắt đầu từ trạng thái cuối cùng khi tất cả các thao tác xóa đã diễn ra. Sau đó thêm học sinh trở lại theo thứ tự ngược lại. 

Việc thêm một học sinh chỉ thêm một cạnh vào biểu đồ:```
value p_i  --->  club c_i
```Không có cạnh nào được loại bỏ. 

Bây giờ hãy xem xét sự phù hợp chứng nhận mex hiện tại. Giả sử chúng ta đã biết rằng các giá trị`0..cur-1`có thể được kết hợp với các câu lạc bộ khác nhau. Khi các cạnh mới được thêm vào, mex chỉ có thể tăng lên. Chúng tôi không bao giờ cần phải kiểm tra lại các giá trị cũ. 

Chúng tôi duy trì sự khớp trên biểu đồ lưỡng cực:```
values  <->  clubs
```Bất biến là các giá trị`0..cur-1`được khớp. Để kiểm tra xem mex có thể trở thành`cur+1`, chúng ta chỉ cần thử khớp giá trị`cur`. Nếu một đường dẫn tăng cường tồn tại, chúng tôi sẽ mở rộng đường dẫn phù hợp và tăng dần`cur`. Chúng tôi tiếp tục làm điều này cho đến khi không thể khớp giá trị tiếp theo. 

Điều này biến toàn bộ quá trình thành một vấn đề so sánh gia tăng. Mỗi giá trị được khớp thành công nhiều nhất một lần, do đó tổng khối lượng công việc vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d · V · E) | O(E) | Quá chậm | 
| Tối ưu | O(E · V) được khấu hao ở đây O(n · 5000) | O(E) | Đã chấp nhận |`V`biểu thị số đỉnh giá trị, nhiều nhất là 5001, và`E ≤ n`. 

## Hướng dẫn thuật toán 

1. Đọc tất cả học sinh và tất cả các truy vấn xóa. 
2. Đánh dấu từng học sinh sẽ bị xóa. 
3. Xây dựng biểu đồ tương ứng với trạng thái sau khi thực hiện tất cả các thao tác xóa. Đối với mỗi học sinh sống sót sau tất cả`d`ngày, hãy tăng thêm lợi thế từ giá trị tiềm năng của nó cho câu lạc bộ của nó. 
4. Duy trì sự kết hợp Kuhn tiêu chuẩn từ câu lạc bộ đến giá trị.`match[club]`lưu trữ giá trị hiện đang sử dụng câu lạc bộ đó. 
5. Hãy để`cur = 0`. Nhiều lần cố gắng tìm đường dẫn tăng dần bắt đầu từ giá trị`cur`. 
6. Nếu DFS thành công, giá trị`cur`trở nên phù hợp. Tăng`cur`và thử giá trị tiếp theo. 
7. Khi DFS bị lỗi,`cur`chính xác là mex tối đa hiện tại. Lưu trữ nó làm câu trả lời cho trạng thái sau khi xóa tất cả. 
8. Quá trình xóa theo thứ tự ngược lại. Khi sinh viên`k_i`được khôi phục, thêm cạnh`(p[k_i], c[k_i])`tới đồ thị. 
9. Sau khi thêm cạnh mới, mex chỉ có thể tăng lên. Tiếp tục vòng lặp tương tự từ hiện tại`cur`, liên tục cố gắng khớp giá trị`cur`. 
10. Lưu trữ mex thu được cho trạng thái mới này. 
11. Sau khi tất cả các phép cộng ngược được xử lý, chúng ta biết mex cho mọi trạng thái trung gian. Xuất ra đáp án tương ứng với ngày`1..d`. 

### Tại sao nó hoạt động 

Đối với bất kỳ giá trị`x`, một cạnh`(x, club)`có nghĩa là câu lạc bộ hiện có ít nhất một học sinh tích cực có tiềm năng`x`. 

Ít nhất một mex`k`yêu cầu mọi giá trị`0..k-1`xuất hiện trong đội đã chọn. Vì mỗi câu lạc bộ chỉ đóng góp tối đa một học sinh nên những giá trị đó phải được gán cho các câu lạc bộ riêng biệt. Đây chính xác là một giá trị bao phủ phù hợp`0..k-1`. 

Việc so khớp được duy trì luôn bao gồm tất cả các giá trị bên dưới`cur`. Khi DFS thành công về giá trị`cur`, một đường dẫn tăng cường sẽ tạo ra một lớp phủ phù hợp`0..cur`. Khi DFS thất bại, điều kiện của Hall bị vi phạm về giá trị`cur`, vì vậy không có kết quả phù hợp nào có thể bao gồm`0..cur`. Do đó, mex tối đa có thể đạt được chính xác là`cur`. 

Vì xử lý ngược chỉ thêm các cạnh nên các giá trị được so khớp trước đó không bao giờ trở thành không hợp lệ. Mex chỉ có thể tăng lên, vì vậy việc tiếp tục khớp từ kết quả hiện tại là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    p = list(map(int, input().split()))
    c = list(map(int, input().split()))

    d = int(input())
    rem = [int(input()) - 1 for _ in range(d)]

    removed = [False] * n
    for x in rem:
        removed[x] = True

    MAXV = 5001
    g = [[] for _ in range(MAXV)]

    for i in range(n):
        if not removed[i]:
            g[p[i]].append(c[i])

    match = [-1] * (m + 1)

    sys.setrecursionlimit(20000)

    def dfs(v):
        if vis[v]:
            return False
        vis[v] = True

        for club in g[v]:
            if match[club] == -1 or dfs(match[club]):
                match[club] = v
                return True
        return False

    cur = 0

    while cur < MAXV:
        vis = [False] * MAXV
        if dfs(cur):
            cur += 1
        else:
            break

    ans = [0] * (d + 1)
    ans[d] = cur

    for idx in range(d - 1, -1, -1):
        student = rem[idx]
        g[p[student]].append(c[student])

        while cur < MAXV:
            vis = [False] * MAXV
            if dfs(cur):
                cur += 1
            else:
                break

        ans[idx] = cur

    print("\n".join(str(ans[i]) for i in range(1, d + 1)))

if __name__ == "__main__":
    solve()
```Biểu đồ được lập chỉ mục theo giá trị tiềm năng.`g[v]`chứa mọi câu lạc bộ hiện có thể cung cấp giá trị`v`. 

Kết quả khớp được lưu trữ từ câu lạc bộ đến giá trị. Đây là cách biểu diễn Kuhn tiêu chuẩn. Trong DFS, nếu một câu lạc bộ đã bị chiếm giữ bởi một giá trị khác, chúng tôi sẽ cố gắng di chuyển giá trị đó đến nơi khác một cách đệ quy. 

Biến`cur`luôn đại diện cho giá trị đầu tiên chưa được biết là có thể so khớp. Việc tăng cường thành công sẽ tăng nó lên đúng một. 

Một lỗi triển khai phổ biến là xây dựng lại kết quả khớp sau mỗi học sinh được khôi phục. Toàn bộ quan điểm của cách tiếp cận ngoại tuyến là việc so khớp hiện tại vẫn hợp lệ vì các cạnh chỉ được thêm vào chứ không bao giờ bị xóa. 

Một chi tiết tinh tế khác là việc lập chỉ mục câu trả lời. Xử lý ngược tính toán câu trả lời cho các trạng thái:```
after all deletions
after restoring last deletion
after restoring last two deletions
...
```Đầu ra yêu cầu tương ứng với các trạng thái sau ngày 1, ngày 2, ..., ngày d, được lưu trữ trong`ans[1] ... ans[d]`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
0 1 2 2 0
1 2 2 3 2
5
3
2
4
5
1
```Sau khi xóa tất cả, không còn học sinh nào nữa. 

| Đảo ngược trạng thái | Đã thêm sinh viên | Mexico hiện tại | 
| --- | --- | --- | 
| Sau khi xóa tất cả | không | 0 | 
| Khôi phục 1 | giá trị 0, câu lạc bộ 1 | 1 | 
| Khôi phục 5 | giá trị 0, câu lạc bộ 2 | 1 | 
| Khôi phục 4 | giá trị 2, câu lạc bộ 3 | 1 | 
| Khôi phục 2 | giá trị 1, câu lạc bộ 2 | 3 | 
| Khôi phục 3 | giá trị 2, câu lạc bộ 2 | 3 | 

Các câu trả lời ngược được lưu trữ trở thành:```
state0 = 3
state1 = 3
state2 = 1
state3 = 1
state4 = 1
state5 = 0
```Đầu ra cần thiết là`state1..state5`:```
3
1
1
1
0
```Ví dụ này cho thấy tại sao xử lý ngược lại hữu ích. Mex chỉ di chuyển lên trên khi các cạnh được thêm vào. 

### Ví dụ tùy chỉnh```
3 2
0 1 1
1 1 2
2
2
3
```Sau cả hai lần xóa, chỉ có giá trị`0`trong câu lạc bộ`1`vẫn còn. 

| Đảo ngược trạng thái | Các cạnh có sẵn | Mex | 
| --- | --- | --- | 
| Sau khi xóa tất cả | 0→1 | 1 | 
| Khôi phục học sinh 3 | 1→2 | 2 | 
| Khôi phục học sinh 2 | 1→1 và 1→2 | 2 | 

Trình tự trả lời là:```
2
1
```Dấu vết này thể hiện hành vi tăng cường. Một lần giá trị`1`được quyền truy cập vào câu lạc bộ`2`, giá trị`0`Và`1`có thể chiếm các câu lạc bộ khác nhau và mex trở thành`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 5000) khấu hao | Mỗi cạnh được chèn một lần, mỗi lần tăng mex thành công xảy ra một lần | 
| Không gian | O(n + m + 5000) | Biểu đồ, mảng khớp và ghi sổ kế toán DFS | 

Các giá trị tiềm năng được giới hạn bởi 5000, đây là điều làm cho việc so khớp tăng dần trở nên khả thi. Với tối đa 5000 sinh viên và 5000 câu lạc bộ, việc triển khai nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    c = list(map(int, input().split()))

    d = int(input())
    rem = [int(input()) - 1 for _ in range(d)]

    removed = [False] * n
    for x in rem:
        removed[x] = True

    MAXV = 5001
    g = [[] for _ in range(MAXV)]

    for i in range(n):
        if not removed[i]:
            g[p[i]].append(c[i])

    match = [-1] * (m + 1)

    sys.setrecursionlimit(20000)

    def dfs(v):
        if vis[v]:
            return False
        vis[v] = True
        for club in g[v]:
            if match[club] == -1 or dfs(match[club]):
                match[club] = v
                return True
        return False

    cur = 0
    while cur < MAXV:
        vis = [False] * MAXV
        if dfs(cur):
            cur += 1
        else:
            break

    ans = [0] * (d + 1)
    ans[d] = cur

    for idx in range(d - 1, -1, -1):
        s = rem[idx]
        g[p[s]].append(c[s])

        while cur < MAXV:
            vis = [False] * MAXV
            if dfs(cur):
                cur += 1
            else:
                break

        ans[idx] = cur

    return "\n".join(str(ans[i]) for i in range(1, d + 1))

# provided sample
assert run(
"""5 3
0 1 2 2 0
1 2 2 3 2
5
3
2
4
5
1
"""
) == "3\n1\n1\n1\n0"

# minimum size
assert run(
"""1 1
0
1
1
1
"""
) == "0"

# all values equal
assert run(
"""3 3
0 0 0
1 2 3
3
1
2
3
"""
) == "1\n1\n0"

# matching requires distinct clubs
assert run(
"""2 1
0 1
1 1
1
2
"""
) == "1"

# mex jumps after restoration
assert run(
"""3 2
0 1 1
1 1 2
2
2
3
"""
) == "2\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Sinh viên độc thân có giá trị 0 | 0 | Ví dụ kích thước tối thiểu | 
| Mọi thế năng đều bằng 0 | 1,1,0 | Giá trị lặp lại không làm tăng mex | 
| Giá trị 0 và 1 trong cùng một câu lạc bộ | 1 | Một câu lạc bộ không thể đại diện cho hai giá trị | 
| Khôi phục tạo ra một kết hợp mới | 2,1 | Logic đường dẫn tăng cường | 

## Vỏ cạnh 

Hãy xem xét:```
2 1
0 1
1 1
1
2
```Chỉ có một câu lạc bộ tồn tại. Mặc dù cả hai giá trị 0 và 1 đều có sẵn nhưng chúng không thể cùng xuất hiện trong nhóm đã chọn. Đồ thị chứa các cạnh:```
0 -> club 1
1 -> club 1
```Giá trị 0 có thể khớp nhưng giá trị 1 không thể khớp đồng thời. Thuật toán không thành công khi cố gắng tăng giá trị 1, do đó mex được báo cáo chính xác là 1. 

Bây giờ hãy xem xét:```
3 2
0 0 1
1 2 2
1
3
```Sau khi xóa giá trị duy nhất 1, đồ thị còn lại có:```
0 -> club 1
0 -> club 2
```Giá trị 0 phù hợp, giá trị 1 thì không. Thuật toán trả về mex 1. Nhiều bản sao của cùng một giá trị không có tác dụng trừ khi cũng có các giá trị thấp hơn bị thiếu. 

Cuối cùng:```
3 3
1 2 3
1 2 3
1
1
```Không có học sinh tích cực nào có giá trị 0. DFS cho giá trị 0 ngay lập tức bị lỗi. Thuật toán trả về mex 0 mà không xem xét bất kỳ giá trị nào lớn hơn, đó chính xác là định nghĩa của mex.
