---
title: "CF 102859A - Bánh Táo"
description: "Trình tự chúng tôi muốn khôi phục là chuyến tham quan Euler của một đồ thị hoàn chỉnh. Mỗi số từ 1 đến N là một đỉnh và viết hai số khác nhau cạnh nhau có nghĩa là đi qua cạnh nối hai đỉnh đó."
date: "2026-07-25T14:20:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102859
codeforces_index: "A"
codeforces_contest_name: "mBIT Standard November 2020"
rating: 0
weight: 102859
solve_time_s: 51
verified: true
draft: false
---

[CF 102859A - Apple Pie](https://codeforces.com/problemset/problem/102859/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trình tự chúng tôi muốn khôi phục là chuyến tham quan Euler của một đồ thị hoàn chỉnh. Mỗi số từ`1`ĐẾN`N`là một đỉnh, và viết hai số khác nhau cạnh nhau có nghĩa là đi qua cạnh nối hai đỉnh đó. Một dãy hợp lệ phải sử dụng mọi cạnh có thể có giữa hai đỉnh khác nhau đúng một lần. 

Đầu vào cung cấp phần hiển thị của chuỗi như vậy sau khi chiếc bánh táo phủ kín một phần giữa liên tục. Phía bên trái là tiền tố vẫn hiển thị và phía bên phải là hậu tố vẫn hiển thị. Chúng ta cần quyết định xem có tồn tại phần giữa ẩn nào đó có thể được chèn vào giữa chúng hay không để chuỗi hoàn chỉnh trở thành phép duyệt hợp lệ trên tất cả các cạnh. 

Các giới hạn được thiết kế xung quanh một biểu đồ có nhiều nhất`N = 1000`đỉnh. Số lượng giá trị hiển thị có thể đạt tới`5 * 10^5`, vì vậy bất kỳ giải pháp nào cố gắng xây dựng hoặc thử nghiệm nhiều chuỗi có thể đều không thể thực hiện được. Chúng ta cần một cách tiếp cận gần tuyến tính về lượng đầu vào cộng với số cạnh có thể có. Vì đồ thị đầy đủ có khoảng`N^2 / 2`các cạnh, một`O(N^2)`giải pháp có thể chấp nhận được vì`N`chỉ là 1000. 

Các trường hợp cạnh đầu tiên xuất phát từ sự tồn tại của chính chuỗi hoàn chỉnh. Ví dụ: với:```
2
0
0
```câu trả lời là`Y`, vì cạnh duy nhất nằm giữa các đỉnh`1`Và`2`, Vì thế`[1,2]`là có thể. Một giải pháp bất cẩn giả định chuỗi hiển thị phải chứa ít nhất một phần tử sẽ bác bỏ trường hợp này. 

Một trường hợp cạnh khác là cạnh lặp lại ở phần nhìn thấy được. Vì:```
3
2 2 1
2 2 1
```câu trả lời là`N`. Cặp đôi`{1,2}`xuất hiện hai lần trong các phần hiển thị, do đó không có phần giữa ẩn nào có thể sửa chữa được trình tự. Việc triển khai chỉ kiểm tra số lượng đỉnh và bỏ qua các cạnh đã được sử dụng sẽ chấp nhận nó không chính xác. 

Trường hợp khó khăn cuối cùng là đỉnh lặp lại không lặp lại cạnh nào. Vì:```
3
2 2 1
2 2 2
```câu trả lời là`N`. Các phần nhìn thấy được chứa cạnh`{1,2}`và cạnh`{2,3}`sẽ bị thiếu, nhưng hậu tố yêu cầu sử dụng`{2,2}`, thậm chí không phải là cạnh được phép vì các số bằng nhau liền kề không hợp lệ. Chỉ kiểm tra độ mà không xác nhận bước đi ban đầu sẽ bỏ lỡ điều này. 

Quan sát đồ thị quan trọng là toàn bộ chuỗi là một đường Euler. Chúng ta có thể loại bỏ các cạnh đã được hiển thị ở bên trái và bên phải. Phần ẩn chỉ cần là đường Euler trong biểu đồ còn lại, nối các phần hiển thị nếu cả hai đều tồn tại. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là cố gắng xây dựng phần giữa còn thiếu. Tổng số cạnh có thể lên tới`499500`, do đó, người ta có thể thử mô phỏng việc xây dựng đường mòn Euler cho mọi điểm bắt đầu có thể hoặc cố gắng quay lại phần bị ẩn. Điều này không thực tế vì số lượng các cấp có thể có của các cạnh là rất lớn. Ngay cả việc kiểm tra một phần nhỏ các con đường có thể cũng nhanh chóng vượt quá thời gian sẵn có. 

Sự thay đổi quan điểm hữu ích là ngừng suy nghĩ về chuỗi còn thiếu và thay vào đó hãy nghĩ về các cạnh vẫn chưa được sử dụng. Tiền tố và hậu tố hiển thị đã sửa một số cạnh. Nếu các cạnh đó hợp lệ, chúng có thể bị xóa khỏi biểu đồ hoàn chỉnh. Câu hỏi đặt ra là liệu biểu đồ còn lại có đường Euler với các điểm cuối cần thiết hay không. 

Đồ thị vô hướng có đường Euler chính xác khi tất cả các đỉnh không cô lập thuộc về một thành phần liên thông và số đỉnh bậc lẻ bằng 0 hoặc hai. Nếu có hai đỉnh lẻ thì chúng phải là điểm bắt đầu và kết thúc của đường đi. 

Vì biểu đồ ban đầu đã hoàn chỉnh nên chúng ta có thể lưu trữ các cạnh nào đã được sử dụng bởi các phần hiển thị trong ma trận boolean. Sau đó, chúng tôi tính toán độ trong biểu đồ còn lại, kiểm tra tính kết nối và xác minh điều kiện Euler. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số cạnh ẩn | O(N^2) | Quá chậm | 
| Tối ưu | O(P + Q + N^2) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai phần nhìn thấy được của trình tự. Duyệt qua từng phần và đánh dấu mỗi cặp liền kề là cạnh được sử dụng. Nếu một cặp liền kề chứa các giá trị bằng nhau hoặc cùng một cạnh vô hướng xuất hiện hai lần thì câu trả lời là ngay lập tức`N`. 
2. Coi tất cả các cạnh không được đánh dấu của đồ thị hoàn chỉnh là phần ẩn. Tính bậc của mỗi đỉnh trong đồ thị còn lại này. 
3. Xác định các điểm cuối của đường Euler cần thiết. Nếu tồn tại cả hai mặt hiển thị thì phần ẩn phải bắt đầu sau mặt trái và kết thúc trước mặt phải. Nếu thiếu một bên, điểm cuối bị thiếu có thể được chọn tự do. 
4. Đếm các đỉnh bậc lẻ của đồ thị còn lại. Nếu cả hai điểm cuối đều cố định thì chúng phải khớp chính xác với các đỉnh bậc lẻ. Nếu một hoặc cả hai điểm cuối đều trống, thì các cấu hình bậc lẻ được phép tuân theo quy tắc đường Euler thông thường. 
5. Kiểm tra xem mọi đỉnh có cạnh còn lại đều thuộc về một thành phần được kết nối. Các cạnh không được sử dụng bị ngắt kết nối không thể được bao phủ bởi một chuỗi ẩn. 
6. Nếu mọi điều kiện đều đạt, xuất ra`Y`; nếu không thì xuất ra`N`. 

Tại sao nó hoạt động: Chuỗi hiển thị đã sử dụng một tập hợp các cạnh từ biểu đồ hoàn chỉnh. Mọi sự hoàn thành hợp lệ đều phải sử dụng chính xác các cạnh còn lại, do đó phần ẩn chính xác là vệt Euler của đồ thị còn lại. Định lý đường mòn Euler đưa ra các điều kiện cần và đủ để tồn tại một đường mòn như vậy. Vì các yêu cầu điểm cuối cũng được kiểm tra nên mọi trường hợp được chấp nhận đều có sự hoàn thành hợp lệ và mọi trường hợp bị từ chối đều không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    left = list(map(int, input().split()))
    p = left[0]
    left = left[1:]

    right = list(map(int, input().split()))
    q = right[0]
    right = right[1:]

    used = [[False] * n for _ in range(n)]
    bad = False

    def add_edges(arr):
        nonlocal bad
        for i in range(len(arr) - 1):
            a = arr[i] - 1
            b = arr[i + 1] - 1
            if a == b or used[a][b]:
                bad = True
            else:
                used[a][b] = True
                used[b][a] = True

    add_edges(left)
    add_edges(right)

    if bad:
        print("N")
        return

    deg = [0] * n
    graph = [[] for _ in range(n)]

    for i in range(n):
        for j in range(i + 1, n):
            if not used[i][j]:
                deg[i] += 1
                deg[j] += 1
                graph[i].append(j)
                graph[j].append(i)

    fixed_start = left[-1] - 1 if p else -1
    fixed_end = right[0] - 1 if q else -1

    odd = [i for i in range(n) if deg[i] % 2]

    if fixed_start != -1 and fixed_end != -1:
        if len(odd) == 0:
            if fixed_start != fixed_end:
                print("N")
                return
        elif len(odd) == 2:
            if set(odd) != {fixed_start, fixed_end}:
                print("N")
                return
        else:
            print("N")
            return
    elif fixed_start != -1:
        if len(odd) == 2:
            if fixed_start not in odd:
                print("N")
                return
        elif len(odd) != 0:
            print("N")
            return
    elif fixed_end != -1:
        if len(odd) == 2:
            if fixed_end not in odd:
                print("N")
                return
        elif len(odd) != 0:
            print("N")
            return
    else:
        if len(odd) not in (0, 2):
            print("N")
            return

    start = -1
    for i in range(n):
        if deg[i]:
            start = i
            break

    if start != -1:
        stack = [start]
        seen = [False] * n
        seen[start] = True
        while stack:
            v = stack.pop()
            for u in graph[v]:
                if not seen[u]:
                    seen[u] = True
                    stack.append(u)

        for i in range(n):
            if deg[i] and not seen[i]:
                print("N")
                return

    print("Y")

if __name__ == "__main__":
    solve()
```Ma trận`used`lưu trữ chính xác các cạnh đã được nhìn thấy. Vì các đỉnh được đánh số từ`1`, quá trình triển khai sẽ chuyển đổi chúng thành các chỉ mục dựa trên 0 trước khi truy cập vào mảng. 

Lần truyền tải đầu tiên xác nhận hai phần đã biết của chuỗi. Các giá trị liền kề bằng nhau không hợp lệ vì một cạnh trong biểu đồ hoàn chỉnh chỉ kết nối các đỉnh khác nhau. Việc nhìn thấy cùng một cạnh vô hướng hai lần cũng là điều không thể vì mỗi cạnh phải xuất hiện đúng một lần. 

Việc tính toán mức độ còn lại bắt đầu từ biểu đồ hoàn chỉnh và chỉ giữ lại các cạnh không được sử dụng một cách hiệu quả. Các điều kiện Euler sau đó được kiểm tra dựa trên biểu đồ còn lại này. Việc xử lý điểm cuối bị tách biệt vì tiền tố hoặc hậu tố bị thiếu có nghĩa là một đầu của đường Euler ẩn không được xác định trước. 

Tìm kiếm theo chiều sâu cuối cùng sẽ bỏ qua các đỉnh bị cô lập vì chúng không chứa các cạnh cần phải duyệt qua. Chỉ các đỉnh tham gia vào biểu đồ còn lại phải thuộc cùng một thành phần liên thông. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
2 2 1
2 3 2
```trình tự nhìn thấy được là`[2,2,1]`ở bên trái và`[2,3,2]`ở bên phải. 

| Bước | Các cạnh đã qua sử dụng | Các đỉnh lẻ còn lại | Kết quả | 
| --- | --- | --- | --- | 
| Đọc trái |`{2,2}`không hợp lệ? không, đỉnh lặp lại thực sự được phát hiện | - | Không hợp lệ | 

Dấu vết này chứng tỏ tại sao các giá trị liền kề bằng nhau phải bị từ chối ngay lập tức. Mẫu thực tế sử dụng đầu vào bên trái`2 2 1`, có nghĩa là các giá trị là`2,1`, không`2,2,1`sau khi phân tích số lượng. Các cạnh nhìn thấy được là`{2,1}`Và`{3,2}`, rời khỏi đường đi Euler`[2,1,3,2]`. 

Đối với đầu vào:```
3
2 2 1
1 3
```những phần nhìn thấy được là`[2,1]`Và`[3]`. 

| Bước | Các cạnh đã qua sử dụng | Các đỉnh lẻ còn lại | Kết quả | 
| --- | --- | --- | --- | 
| Di dời`{1,2}`| bờ rìa`{1,2}`đi | đỉnh 2,3 | Tiếp tục | 
| Kiểm tra điểm cuối hậu tố | đường dẫn ẩn phải kết thúc ở số 3 | không khớp | Từ chối | 

Điều này thể hiện điều kiện điểm cuối. Biểu đồ không được sử dụng không thể kết thúc ở điểm bắt đầu có hậu tố được yêu cầu trong khi sử dụng tất cả các cạnh còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(P + Q + N^2) | Các chuỗi hiển thị được quét một lần và các cạnh đồ thị hoàn chỉnh được kiểm tra một lần. | 
| Không gian | O(N^2) | Ma trận cạnh lưu trữ xem mỗi cạnh có thể đã xuất hiện hay chưa. | 

Đồ thị lớn nhất có thể có khoảng nửa triệu cạnh, do đó việc lưu trữ bậc hai có thể chấp nhận được đối với`N <= 1000`. Thời gian chạy bị chi phối bởi việc quét các cạnh có thể có, đủ nhỏ cho các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out

# Sample 1
assert run("""2
0
0
""") == "Y\n"

# Sample 2
assert run("""3
1 2
0
""") == "Y\n"

# Sample 3
assert run("""3
2 2 1
2 3 2
""") == "Y\n"

# Invalid repeated edge
assert run("""3
2 2 1
2 2 1
""") == "N\n"

# Invalid equal adjacent values
assert run("""3
2 2 1
2 2 2
""") == "N\n"

# Larger odd complete graph with no visible part
assert run("""5
0
0
""") == "Y\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`N=2`không có giá trị hiển thị |`Y`| Đồ thị nhỏ nhất có thể | 
| Một tiền tố hiển thị duy nhất |`Y`| Xử lý hậu tố ẩn miễn phí | 
| Chuyến tham quan Euler một phần hợp lệ hoàn chỉnh |`Y`| Hoàn thành bình thường | 
| Cạnh nhìn thấy lặp đi lặp lại |`N`| Tính độc đáo của cạnh | 
| Giá trị liền kề bằng nhau |`N`| Chuyển tiếp không hợp lệ | 
| Quan sát trống trên số lẻ`N`|`Y`| Sự tồn tại của mạch Euler | 

## Vỏ cạnh 

Khi chiếc bánh táo bao phủ toàn bộ chuỗi, cả hai phần nhìn thấy đều trống. Thuật toán chỉ đơn giản kiểm tra xem bản thân đồ thị hoàn chỉnh có đường Euler hay không. Vì`N=2`, đồ thị còn lại có hai đỉnh và đường đi bậc lẻ. Đối với số lẻ`N`, mọi đỉnh đều có bậc chẵn nên tồn tại chu trình Euler. Thậm chí`N > 2`, có quá nhiều đỉnh bậc lẻ và câu trả lời trở thành`N`. 

Khi một mặt hiển thị chứa cùng một cạnh hai lần, thuật toán sẽ loại bỏ trong lần quét đầu tiên. Ví dụ:```
3
3 1 2 1
0
```chứa các cạnh`{1,2}`Và`{1,2}`lại. Đánh dấu ma trận cạnh sẽ nắm bắt được điều này trước khi thực hiện bất kỳ suy luận Euler nào. 

Khi chỉ nhìn thấy một bên, điểm cuối còn thiếu của vệt Euler ẩn được phép thay đổi. Ví dụ:```
3
1 2
0
```rời khỏi phần ẩn bắt đầu sau đỉnh`2`. Các cạnh còn lại có thể đi qua như sau`2 -> 1 -> 3 -> 2`, do đó thuật toán chấp nhận nó vì các điều kiện Euler cho phép một điểm cuối phù hợp.
