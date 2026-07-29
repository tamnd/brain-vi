---
title: "CF 102760E ​​- Băm tối thiểu"
description: "Chúng ta có một đồ thị vô hướng. Mỗi đỉnh bắt đầu bằng một nhãn duy nhất và giá trị nhãn là hoán vị của các số từ 1 đến n. Trong mỗi vòng của quy trình, mỗi đỉnh sẽ nhìn vào các đỉnh lân cận của nó và giữ giá trị nhỏ nhất hiện có trong số đó."
date: "2026-07-28T23:50:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "E"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 65
verified: true
draft: false
---

[CF 102760E - Min-hashing](https://codeforces.com/problemset/problem/102760/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng. Mỗi đỉnh bắt đầu bằng một nhãn duy nhất và giá trị nhãn là hoán vị của các số từ`1`ĐẾN`n`. Trong mỗi vòng của quy trình, mỗi đỉnh sẽ nhìn vào các đỉnh lân cận của nó và giữ giá trị nhỏ nhất hiện có trong số đó. Vòng đầu tiên sử dụng nhãn ban đầu và mỗi vòng sau sử dụng các giá trị do vòng trước tạo ra. 

Sau vòng đấu`k`, ta đếm có bao nhiêu cặp đỉnh khác nhau có cùng giá trị. Nhiệm vụ là tìm giá trị lớn nhất có thể có của số đếm này qua tất cả các vòng dương. 

Các ràng buộc rất lớn: có thể có tới`100000`đỉnh và`250000`các cạnh. Một mô phỏng cập nhật liên tục ở mọi đỉnh sẽ đòi hỏi phải hiểu cần bao nhiêu vòng và thậm chí một vòng đã có giá`O(n + m)`. Vì đồ thị có thể lớn và câu trả lời phụ thuộc vào hành vi sau nhiều vòng, nên chúng ta cần quan sát cấu trúc hơn là mô phỏng trực tiếp. 

Các nhãn là duy nhất, đó là một thuộc tính hữu ích. Khi hai đỉnh có cùng giá trị sau một số vòng, cả hai đều chọn cùng một đỉnh được gắn nhãn tối thiểu từ một số tập hợp các đỉnh có thể tiếp cận. Hoạt động tối thiểu lặp đi lặp lại không tạo ra giá trị mới mà chỉ chọn các nhãn hiện có. 

Một số trường hợp rất dễ xử lý sai. Xét một đồ thị có một cạnh.```
2 1
1 2
1 2
```Sau vòng đầu tiên, cả hai đỉnh đều nhận được nhãn của đỉnh kia nên giá trị là`[2, 1]`và câu trả lời là`0`. Một giải pháp bao gồm không chính xác các nhãn gốc làm vòng có thể sẽ tính cặp, điều này không được phép vì`k`bắt đầu từ`1`. 

Một trường hợp khó khăn khác là một ngôi sao.```
4 3
1 2 3 4
1 2
1 3
1 4
```Sau một vòng, tất cả các lá đều nhận được nhãn`1`, cho`C(3, 2) = 3`cặp bằng nhau. Trung tâm nhận được nhãn lá nhỏ nhất. Chi tiết quan trọng là trung tâm và các lá không trở thành một nhóm mãi mãi, bởi vì biểu đồ có tính chất lưỡng cực và tính chẵn lẻ của các bước đi rất quan trọng. 

Trường hợp cuối cùng là một chu trình lẻ.```
3 3
1 2 3
1 2
2 3
3 1
```Sau đủ số vòng, mỗi đỉnh có thể đến mọi đỉnh khác với một bước đi có độ dài cần thiết. Cuối cùng tất cả các đỉnh đều nhận được nhãn`1`, sản xuất`C(3, 2) = 3`. Việc xử lý mọi thành phần giống như biểu đồ lưỡng cực sẽ bỏ lỡ điều này vì các chu kỳ lẻ sẽ loại bỏ hạn chế chẵn lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng quá trình. Đối với mỗi vòng, chúng tôi quét mọi cạnh và tính giá trị lân cận tối thiểu cho cả hai điểm cuối. Sau khi có được nhãn mới, chúng tôi đếm các giá trị bằng nhau bằng bảng tần số. Điều này đúng vì nó tuân theo định nghĩa chính xác. 

Vấn đề là quyết định chúng ta cần mô phỏng trong bao lâu. Một biểu đồ có thể chứa các đường dẫn dài và không có giới hạn nhỏ hữu ích nào về số vòng của câu lệnh. Lặp đi lặp lại`O(n + m)`làm việc trong nhiều vòng là quá tốn kém. Trong trường hợp xấu nhất, việc cố gắng mô phỏng cho đến khi không có gì thay đổi có thể vượt xa những gì có thể thực hiện được.`n = 100000`. 

Quan sát quan trọng là ngừng xem xét các giá trị và thay vào đó hãy xem các đỉnh ban đầu nào có thể ảnh hưởng đến một đỉnh sau đó.`k`vòng. Sau một vòng, một đỉnh sẽ lưu nhãn tối thiểu giữa các đỉnh ở khoảng cách một. Sau hai vòng, nó lưu nhãn tối thiểu trong số các đỉnh có thể tiếp cận được bằng một bước dài hai. Nói chung,`h^(k)`là nhãn tối thiểu trong số tất cả các đỉnh có thể tiếp cận được bằng một bước đi có độ dài chính xác`k`. 

Đối với một thành phần không lưỡng cực được kết nối, sau đủ số bước, mỗi đỉnh có thể đến mọi đỉnh bằng cách sử dụng các bước đi có độ dài cần thiết. Nhãn nhỏ nhất trong thành phần sẽ trải rộng khắp nơi, do đó toàn bộ thành phần sẽ trở thành một nhóm. 

Đối với một thành phần lưỡng cực được kết nối, mỗi bước đi sẽ luân phiên các bên. Sau đủ các bước, một đỉnh có thể đến mọi đỉnh ở phía được xác định bởi độ dài bước đi chẵn lẻ. Nhãn nhỏ nhất ở mỗi bên trải sang phía đối diện hoặc cùng một phía tùy theo độ chẵn lẻ của vòng. Các nhãn thực tế không ảnh hưởng đến số lượng cặp bằng nhau, chỉ có kích thước của hai lớp màu là quan trọng. 

Do đó, mức tối đa thu được từ trạng thái ổn định của mọi thành phần. Các đóng góp thành phần có thể được thêm vào một cách độc lập vì các đỉnh từ các thành phần được kết nối khác nhau không bao giờ có thể ảnh hưởng lẫn nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Quá phụ thuộc vào số vòng, mỗi vòng là`O(n + m)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n + m)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt qua mọi thành phần được kết nối bằng DFS hoặc BFS trong khi tô màu đồ thị bằng hai màu. Màu sắc cho chúng ta biết thành phần đó có phải là lưỡng cực hay không và cũng cho biết kích thước của mỗi bên. Thông tin phụ chính xác là thứ kiểm soát khả năng tiếp cận sau nhiều vòng. 
2. Trong khi duyệt một thành phần, hãy lưu trữ xem có xuất hiện xung đột hay không, nghĩa là một cạnh nối hai đỉnh có cùng màu. Sự xung đột như vậy chứng tỏ thành phần đó không phải là lưỡng đảng. Đối với thành phần không có hai bên, tất cả các đỉnh cuối cùng đều nhận được nhãn tối thiểu của thành phần đó. 
3. Đối với thành phần không lưỡng cực về quy mô`s`, thêm vào`s * (s - 1) / 2`để trả lời. Mỗi cặp bên trong thành phần này cuối cùng đều có giá trị bằng nhau. 
4. Đối với thành phần lưỡng cực có kích thước lớp màu`a`Và`b`, thêm vào`a * (a - 1) / 2 + b * (b - 1) / 2`. Sau đủ vòng, hai lớp màu sẽ trở thành hai nhóm độc lập có giá trị bằng nhau trong mỗi nhóm. 
5. In tổng của tất cả các thành phần đóng góp. Quá trình tìm kiếm các thành phần đã bao trùm toàn bộ biểu đồ nên không cần mô phỏng các vòng băm. 

Tại sao nó hoạt động: 

Giá trị của một đỉnh sau`k`các vòng được xác định bởi nhãn gốc tối thiểu trong số các đỉnh có thể tiếp cận được bằng một bước dài`k`. Trong biểu đồ không lưỡng cực được kết nối, các bước đi đủ dài có thể kết nối hai đỉnh bất kỳ, do đó nhãn tối thiểu của toàn bộ thành phần sẽ chạm tới mọi đỉnh. Trong biểu đồ hai bên được kết nối, các bước đi duy trì tính chẵn lẻ của các cạnh, do đó, chỉ các đỉnh từ cùng một lớp màu mới có thể chia sẻ cùng một tập hợp có thể truy cập cuối cùng. Vì nhãn được chọn bên trong mỗi bộ có thể truy cập là duy nhất nhưng giống hệt nhau đối với tất cả các đỉnh có cùng một tập hợp có thể truy cập, nên các nhóm cuối cùng chính xác là các lớp màu của các thành phần lưỡng cực và toàn bộ thành phần cho các thành phần không lưỡng cực. Không có vòng nào trước đó có thể tạo ra các nhóm lớn hơn vì mỗi vòng chỉ là sự hạn chế của các đỉnh có thể tiếp cận được trước khi đạt đến các tập hợp có thể tiếp cận ổn định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    labels = list(map(int, input().split()))

    graph = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    color = [-1] * n
    ans = 0

    for start in range(n):
        if color[start] != -1:
            continue

        stack = [start]
        color[start] = 0
        cnt = [1, 0]
        bipartite = True

        while stack:
            v = stack.pop()
            for u in graph[v]:
                if color[u] == -1:
                    color[u] = color[v] ^ 1
                    cnt[color[u]] += 1
                    stack.append(u)
                elif color[u] == color[v]:
                    bipartite = False

        if bipartite:
            ans += cnt[0] * (cnt[0] - 1) // 2
            ans += cnt[1] * (cnt[1] - 1) // 2
        else:
            s = cnt[0] + cnt[1]
            ans += s * (s - 1) // 2

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện chỉ cần truyền tải đồ thị. Các nhãn được đọc vì chúng thuộc về định nghĩa ban đầu, nhưng số lượng cặp bằng nhau cuối cùng chỉ phụ thuộc vào cấu trúc thành phần chứ không phụ thuộc vào giá trị thực của nhãn. 

các`color`mảng thực hiện kiểm tra lưỡng cực. Một thành phần mới bắt đầu bằng màu sắc`0`và mọi cạnh đều yêu cầu điểm cuối còn lại có màu đối lập. Nếu chúng ta tìm thấy một cạnh giữa các màu bằng nhau thì thành phần đó chứa một chu kỳ lẻ. 

số lượng`cnt[0]`Và`cnt[1]`là kích thước của hai mặt của một thành phần lưỡng cực. Số lượng các cặp bằng nhau trong một nhóm có kích thước`x`là`x * (x - 1) // 2`, đó là lý do tại sao công thức được sử dụng trực tiếp thay vì xây dựng các giá trị băm cuối cùng. 

Số nguyên Python không tràn, nhưng câu trả lời có thể lớn bằng`C(100000, 2)`, do đó, ngôn ngữ có số nguyên có chiều rộng cố định sẽ cần loại 64 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5
1 2 3 4 5
1 2
2 3
3 4
4 5
5 1
```Đồ thị là một chu trình có độ dài 5 nên nó không phải là đồ thị lưỡng cực. 

| Bước | Kích thước thành phần | lưỡng đảng | Kích thước màu | Đã thêm cặp | 
| --- | --- | --- | --- | --- | 
| DFS kết thúc | 5 | Không | Không áp dụng |`5 * 4 / 2 = 10`| 

Chu kỳ lẻ có nghĩa là mọi đỉnh cuối cùng có thể đến mọi đỉnh với độ dài bước đi phù hợp. Nhãn tối thiểu trải rộng đến cả năm đỉnh, tạo ra mười cặp bằng nhau. 

### Mẫu 2 

đầu vào:```
4 3
1 2 3 4
1 2
2 3
3 4
```Đồ thị là một đường đi có hai đỉnh ở mỗi cạnh. 

| Bước | Kích thước thành phần | lưỡng đảng | Kích thước màu | Đã thêm cặp | 
| --- | --- | --- | --- | --- | 
| DFS kết thúc | 4 | Có | 2, 2 |`1 + 1 = 2`| 

Sau nhiều vòng, một bên của đường dẫn nhận được một nhãn tối thiểu và bên kia nhận được nhãn tối thiểu còn lại. Mỗi bên đóng góp một cặp bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Mỗi đỉnh và cạnh được thăm một lần trong quá trình duyệt đồ thị. | 
| Không gian |`O(n + m)`| Danh sách kề lưu trữ biểu đồ và mảng lưu trữ trạng thái truyền tải. | 

Các giới hạn cho phép xử lý đồ thị tuyến tính. Một chuyến đi ngang qua`100000`đỉnh và`250000`các cạnh vừa khít một cách thoải mái, trong khi việc mô phỏng lặp đi lặp lại quá trình băm thì không. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

# Sample 1
assert run("""5 5
1 2 3 4 5
1 2
2 3
3 4
4 5
5 1
""") == "10\n"

# Sample 2
assert run("""4 3
1 2 3 4
1 2
2 3
3 4
""") == "2\n"

# Minimum graph
assert run("""2 1
1 2
1 2
""") == "0\n"

# Star graph
assert run("""5 4
1 2 3 4 5
1 2
1 3
1 4
1 5
""") == "6\n"

# Complete triangle
assert run("""3 3
1 2 3
1 2
2 3
3 1
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh có một cạnh |`0`| Vòng hợp lệ đầu tiên là`k = 1`, không phải nhãn ban đầu. | 
| Đồ thị sao |`6`| Một bên lưỡng cực lớn tạo ra nhiều cặp bằng nhau. | 
| Tam giác |`3`| Các chu kỳ lẻ phải được coi là không lưỡng cực. | 
| Mẫu đường dẫn |`2`| Các thành phần lưỡng cực sử dụng kích thước lớp màu. | 

## Vỏ cạnh 

Đối với đồ thị một cạnh:```
2 1
1 2
1 2
```DFS tìm thấy một thành phần lưỡng cực có kích thước màu`1`Và`1`. Sự đóng góp là`0 + 0 = 0`. Điều này khớp với quy trình vì mỗi đỉnh luôn nhận được nhãn của đỉnh kia, vì vậy không có hai đỉnh nào chia sẻ một giá trị. 

Đối với đồ thị ngôi sao:```
4 3
1 2 3 4
1 2
1 3
1 4
```Các màu có kích thước`1`Và`3`. Thuật toán bổ sung`C(1,2) + C(3,2) = 3`. Ba lá có cùng một mặt có thể tiếp cận được, trong khi phần trung tâm vẫn nằm trong nhóm còn lại. 

Đối với chu kỳ lẻ:```
3 3
1 2 3
1 2
2 3
3 1
```DFS phát hiện ra rằng đồ thị không thể được tô bằng hai màu vì các đỉnh liền kề sẽ cần cùng một màu ở đâu đó. Toàn bộ thành phần có kích thước ba, vì vậy nó góp phần`C(3,2) = 3`. 

Đối với các biểu đồ bị ngắt kết nối, mỗi thành phần được xử lý riêng biệt. Một đỉnh trong một thành phần không bao giờ có thể nhìn thấy nhãn từ thành phần khác, vì vậy việc cộng các đóng góp thành phần độc lập sẽ mang lại giá trị tối đa toàn cục chính xác.
