---
title: "CF 102538J - Chỉ cần đếm"
description: "Chúng ta có một đồ thị vô hướng. Mỗi cạnh phải nhận một giá trị từ tập dư lượng modulo 5, nghĩa là các giá trị có thể là 0, 1, 2, 3, 4. Việc gán nhãn là hợp lệ khi mỗi đỉnh có các giá trị cạnh phụ có tổng chia hết cho 5."
date: "2026-08-03T21:05:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102538
codeforces_index: "J"
codeforces_contest_name: "300iq Contest 3"
rating: 0
weight: 102538
solve_time_s: 75
verified: true
draft: false
---

[CF 102538J - Chỉ đếm](https://codeforces.com/problemset/problem/102538/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng. Mỗi cạnh phải nhận một giá trị từ tập dư lượng modulo 5, nghĩa là các giá trị có thể có là`0, 1, 2, 3, 4`. Việc gán nhãn hợp lệ khi mọi đỉnh đều có các giá trị cạnh tới có tổng chia hết cho 5. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu nhãn cạnh hợp lệ. Câu trả lời được in theo modulo`998244353`. 

Đầu vào bao gồm một số trường hợp biểu đồ. Mỗi trường hợp cho biết số đỉnh và cạnh, theo sau là các cặp đỉnh được nối bởi mỗi cạnh. Biểu đồ có thể bị ngắt kết nối nên mỗi thành phần được kết nối phải được xử lý độc lập. 

Các ràng buộc làm cho một giải pháp tuyến tính hoặc gần như tuyến tính trở nên cần thiết. Tổng số đỉnh và cạnh trong tất cả các trường hợp thử nghiệm là bị giới hạn, do đó việc duyệt đồ thị là hợp lý. Bất kỳ cách tiếp cận nào cố gắng liệt kê các bài tập đều không có cơ hội vì mỗi cạnh có năm lựa chọn, đưa ra`5^m`khả năng. Ngay cả những đồ thị có kích thước vừa phải cũng làm cho nó trở nên lớn về mặt thiên văn. 

Khó khăn tiềm ẩn chính là hiểu được số lượng các ràng buộc độc lập. Một sai lầm phổ biến là cho rằng mọi điều kiện của đỉnh đều loại bỏ một bậc tự do. Điều đó sai vì các phương trình đỉnh không phải lúc nào cũng độc lập. Một cái cây và một chu trình lẻ hoạt động khác nhau và các thành phần rời rạc sẽ nhân lên những đóng góp của chúng. 

Ví dụ, một đỉnh cô lập không có cạnh.```
1 0
```Chỉ có một bài tập trống nên đáp án là`1`. Một giải pháp giả định rằng mọi thành phần được kết nối đều đóng góp một mức độ tự do của chu trình sẽ thêm một cái gì đó vào đây một cách không chính xác. 

Một hình tam giác là một trường hợp quan trọng khác.```
3 3
1 2
2 3
3 1
```Câu trả lời là`1`. Ba phương trình đỉnh độc lập với modulo 5 vì chu trình là số lẻ. Coi mọi chu trình như thêm một biến tự do sẽ tạo ra sai số`5`. 

Một hình vuông cho thấy hành vi ngược lại.```
4 4
1 2
2 3
3 4
4 1
```Câu trả lời là`5`. Phép gán xen kẽ xung quanh một chu trình chẵn sẽ tạo ra một biến tự do. Một phương pháp chỉ kiểm tra xem đồ thị có chứa một chu trình mà không kiểm tra xem nó là số lẻ hay số chẵn sẽ bỏ sót điều này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi cách gán giá trị có thể có cho các cạnh và kiểm tra tổng các đỉnh. Điều này đúng vì mọi nhãn có thể đều được kiểm tra và chỉ những nhãn hợp lệ mới được tính. Tuy nhiên, với`m`cạnh này đòi hỏi`5^m`bài tập. Đối với một đồ thị có hàng trăm nghìn cạnh thì điều này là không thể. 

Quan sát hữu ích đến từ việc xem bài toán như một hệ thống tuyến tính trên trường modulo 5. Mỗi cạnh là một biến. Mỗi đỉnh đưa ra một phương trình yêu cầu tổng các biến của cạnh liền kề bằng 0. Nếu có`m`các biến và hệ thống có thứ hạng`r`, số cách giải quyết là:`5^(m-r)`Toàn bộ vấn đề trở thành việc tìm thứ hạng của ma trận giống tỷ lệ mắc của đồ thị. 

Đối với một thành phần được kết nối với`n`đỉnh, thứ hạng chỉ phụ thuộc vào việc thành phần đó có phải là lưỡng cực hay không. Nếu thành phần là lưỡng cực thì các phương trình đỉnh có một phụ thuộc, do đó thứ hạng là`n-1`. Nếu thành phần không phải là lưỡng cực, chu kỳ lẻ sẽ loại bỏ sự phụ thuộc đó và thứ hạng sẽ trở thành`n`. 

Phương pháp brute-force hoạt động vì các phương trình mô tả hoàn toàn các phép gán hợp lệ, nhưng không thành công vì nó bỏ qua cấu trúc của hệ thống tuyến tính. Nhận xét rằng chỉ có thứ hạng mới quan trọng cho phép chúng ta thay thế phép liệt kê hàm mũ bằng phép duyệt đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(5^m · (n+m)) | O(n+m) | Quá chậm | 
| Tối ưu | O(n+m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của đồ thị. Mỗi cạnh được lưu trữ theo cả hai hướng vì sau này chúng ta cần duyệt qua các thành phần được kết nối và kiểm tra tính lưỡng cực. 
2. Chạy tìm kiếm theo chiều rộng hoặc tìm kiếm theo chiều sâu từ mọi đỉnh chưa được thăm. Trong quá trình truyền tải, gán cho mỗi đỉnh một trong hai màu. Nếu một cạnh nối hai đỉnh có cùng màu thì thành phần đó không phải là thành phần lưỡng cực. 
3. Đếm số đỉnh và cạnh bên trong thành phần được kết nối hiện tại. Nếu thành phần là lưỡng cực, hãy thêm`vertices - 1`đến tổng thứ hạng. Nếu không thì thêm`vertices`đến tổng thứ hạng. 

Lý do số cạnh không cần phải sử dụng trực tiếp là vì mọi thành phần đều đóng góp theo số đỉnh và cấu trúc chẵn lẻ của nó. Số cạnh chỉ xuất hiện sau trong số mũ. 

1. Sau khi xử lý xong tất cả các thành phần, tính kết quả như sau:`5^(total_edges - total_rank)`sử dụng lũy ​​thừa mô-đun. 

Tại sao nó hoạt động: 

Các phương trình đồ thị tạo thành một hệ tuyến tính trên modulo 5. Số nghiệm của hệ tuyến tính đồng nhất với`m`biến và xếp hạng`r`chính xác là`5^(m-r)`, bởi vì mọi biến tự do có thể độc lập nhận một trong năm giá trị. 

Đối với thành phần lưỡng cực liên thông, tổng của tất cả các phương trình đỉnh có dấu xen kẽ sẽ triệt tiêu mọi cạnh, tạo ra một phụ thuộc. Do đó, thứ hạng ít hơn số đỉnh. Đối với thành phần không có hai bên, một chu trình lẻ sẽ ngăn chặn sự phụ thuộc đó, do đó thứ hạng bằng số đỉnh. Tổng các thứ hạng này trên các thành phần được kết nối sẽ cho ra thứ hạng của toàn bộ hệ thống, điều này làm cho số mũ cuối cùng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    data = sys.stdin.buffer.read().split()
    if not data:
        return

    it = iter(data)
    t = int(next(it))
    ans = []

    for _ in range(t):
        n = int(next(it))
        m = int(next(it))

        g = [[] for _ in range(n)]
        for _ in range(m):
            a = int(next(it)) - 1
            b = int(next(it)) - 1
            g[a].append(b)
            g[b].append(a)

        color = [-1] * n
        rank = 0

        for start in range(n):
            if color[start] != -1:
                continue

            color[start] = 0
            stack = [start]
            vertices = 0
            bipartite = True

            while stack:
                v = stack.pop()
                vertices += 1

                for u in g[v]:
                    if color[u] == -1:
                        color[u] = color[v] ^ 1
                        stack.append(u)
                    elif color[u] == color[v]:
                        bipartite = False

            if bipartite:
                rank += vertices - 1
            else:
                rank += vertices

        ans.append(str(pow(5, m - rank, MOD)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc xây dựng danh sách kề tương ứng với bước thuật toán đầu tiên. Mỗi cạnh xuất hiện hai lần, nhưng điều này chỉ ảnh hưởng đến thời gian truyền chứ không ảnh hưởng đến số lượng toán học. 

Ngăn xếp DFS duy trì thành phần được kết nối hiện tại. Mảng`color`lưu trữ nỗ lực hai màu. Khi một hàng xóm được truy cập trước đó có cùng màu, thành phần này chứa một chu kỳ lẻ, vì vậy nó không phải là lưỡng cực. 

Biến`rank`lưu trữ tổng đóng góp xếp hạng của tất cả các thành phần. Cuộc gọi cuối cùng tới`pow`sử dụng phép lũy thừa mô-đun của Python, giúp tránh việc xây dựng giá trị khổng lồ`5^(m-rank)`trực tiếp. 

Không có lo ngại về tràn vì số nguyên Python có độ chính xác tùy ý và phép lũy thừa mô-đun giữ cho các giá trị trung gian được kiểm soát. Số mũ không bao giờ âm vì thứ hạng của hệ thống không thể vượt quá số biến cạnh. 

## Ví dụ đã hoạt động 

Đối với tam giác:```
3 3
1 2
2 3
3 1
```Việc truyền tải hoạt động như sau. 

| Thành phần hiện tại | Các đỉnh được tìm thấy | lưỡng đảng | Xếp hạng đóng góp | Số mũ | 
| --- | --- | --- | --- | --- | 
| tam giác | 3 | không | 3 | 3 - 3 = 0 | 

Tam giác có chu trình lẻ nên không có biến tự do. Câu trả lời là`5^0 = 1`. 

Đối với hình vuông:```
4 4
1 2
2 3
3 4
4 1
```Việc truyền tải mang lại: 

| Thành phần hiện tại | Các đỉnh được tìm thấy | lưỡng đảng | Xếp hạng đóng góp | Số mũ | 
| --- | --- | --- | --- | --- | 
| vuông | 4 | vâng | 3 | 4 - 3 = 1 | 

Chu kỳ chẵn tạo ra một bậc tự do. Câu trả lời là`5^1 = 5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+m) | Mỗi đỉnh và cạnh được thăm một số lần không đổi | 
| Không gian | O(n+m) | Danh sách kề và mảng truyền tải lưu trữ biểu đồ | 

Tổng kích thước đầu vào bị giới hạn, do đó, việc truyền tải tuyến tính dễ dàng phù hợp với giới hạn đã định. Giải pháp tránh mọi sự phụ thuộc vào số lượng phép gán cạnh có thể có. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 998244353

def solution(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    res = []

    for _ in range(t):
        n, m = map(int, input().split())
        g = [[] for _ in range(n)]

        for _ in range(m):
            a, b = map(int, input().split())
            a -= 1
            b -= 1
            g[a].append(b)
            g[b].append(a)

        color = [-1] * n
        rank = 0

        for i in range(n):
            if color[i] != -1:
                continue

            stack = [i]
            color[i] = 0
            cnt = 0
            ok = True

            while stack:
                v = stack.pop()
                cnt += 1
                for u in g[v]:
                    if color[u] == -1:
                        color[u] = color[v] ^ 1
                        stack.append(u)
                    elif color[u] == color[v]:
                        ok = False

            rank += cnt - 1 if ok else cnt

        res.append(str(pow(5, m - rank, MOD)))

    return "\n".join(res)

assert solution("""3
1 0
3 3
1 2
2 3
3 1
4 4
1 2
2 3
3 4
4 1
""") == """1
1
5""", "samples"

assert solution("""1
2 0
""") == "1", "isolated vertices"

assert solution("""1
5 4
1 2
2 3
3 4
4 5
""") == "1", "tree"

assert solution("""1
6 6
1 2
2 3
3 4
4 5
5 6
6 1
""") == "5", "even cycle"

assert solution("""1
5 5
1 2
2 3
3 4
4 5
5 1
""") == "1", "odd cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn không có cạnh |`1`| Xử lý bài tập trống | 
| Một cái cây |`1`| Thành phần lưỡng cực không có chu kỳ | 
| Một chu kỳ chẵn |`5`| Một biến tự do từ một chu kỳ chẵn | 
| Một chu kỳ kỳ lạ |`1`| Xếp hạng đầy đủ trong một thành phần không lưỡng đảng | 

## Vỏ cạnh 

Đối với đỉnh cô lập:```
1 0
```Thành phần này chứa một đỉnh và không có cạnh. Nó là lưỡng đảng nên đóng góp cấp bậc của nó là`1-1=0`. Số mũ là`0-0=0`, cho`1`. Thuật toán không loại bỏ một mức độ tự do không tồn tại một cách không chính xác. 

Đối với một cái cây:```
5 4
1 2
2 3
3 4
4 5
```Đồ thị là lưỡng cực. Thứ hạng thành phần là`5-1=4`, phù hợp với số cạnh. Số mũ bằng 0, do đó mọi giá trị cạnh đều bị ép buộc và chỉ còn lại phép gán toàn số 0. 

Đối với chu kỳ lẻ:```
3 3
1 2
2 3
3 1
```Nỗ lực tô màu phát hiện xung đột khi cạnh cuối cùng nối hai đỉnh có màu bằng nhau. Thứ hạng thành phần trở thành`3`thay vì`2`. Số mũ trở thành số 0, chỉ cho một phép gán hợp lệ một cách chính xác. 

Đối với chu trình chẵn:```
4 4
1 2
2 3
3 4
4 1
```Việc tô màu thành công. Sự đóng góp cấp bậc là`3`, để lại một biến cạnh tự do. Số mũ là một nên thành phần đóng góp năm giải pháp. Thuật toán nắm bắt được sự tự do bổ sung mà không cần xây dựng chu trình một cách rõ ràng.
