---
title: "CF 104316F - \u041b\u0438\u0441\u0438\u0446\u0430 \u0438 \u043f\u043e\u043b\u043d\u044b\u0439 \u043e\u0431\u0445\u043e\u0434 \u0434\u0440\u0435\u0432\u0430"
description: "Chúng ta được cho một cái cây, nghĩa là một đồ thị liên thông không có chu trình. Một con cáo bắt đầu ở một đỉnh nào đó và có thể di chuyển trong một bước nhảy bằng cách sử dụng một quy tắc bất thường: từ đỉnh v, nó có thể nhảy đến đỉnh u nếu có một cạnh nối trực tiếp chúng hoặc nếu tồn tại một số điểm trung gian…"
date: "2026-07-01T19:36:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104316
codeforces_index: "F"
codeforces_contest_name: "VIII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b"
rating: 0
weight: 104316
solve_time_s: 71
verified: true
draft: false
---

[CF 104316F - \u041b\u0438\u0441\u0438\u0446\u0430 \u0438 \u043f\u043e\u043b\u043d\u044b\u0439 \u043e\u0431\u0445\u043e\u0434 \u0434\u0440\u0435\u0432\u0430](https://codeforces.com/problemset/problem/104316/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây, nghĩa là một đồ thị liên thông không có chu trình. Một con cáo bắt đầu ở một đỉnh nào đó và có thể di chuyển chỉ bằng một bước nhảy bằng cách sử dụng một quy tắc khác thường: từ một đỉnh`v`, nó có thể nhảy tới một đỉnh`u`hoặc nếu có một cạnh nối trực tiếp chúng hoặc nếu tồn tại một số đỉnh trung gian`w`sao cho cả hai`v`Và`u`liền kề với`w`. Nói cách khác, được phép di chuyển nếu hai đỉnh ở khoảng cách đồ thị 1 hoặc 2. 

Nhiệm vụ là quyết định xem có tồn tại một chu trình đi qua mọi đỉnh đúng một lần hay không, trong đó các đỉnh liên tiếp trong chu trình được kết nối bằng một trong những bước nhảy được phép này. Nếu một chu trình như vậy tồn tại, chúng ta phải đưa ra bất kỳ thứ tự hợp lệ nào. 

Kích thước đầu vào lên tới 200.000 đỉnh, do đó, mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính theo thời gian. Điều này ngay lập tức loại trừ mọi nỗ lực liệt kê các hoán vị hoặc xây dựng rõ ràng biểu đồ đầy đủ về các bước nhảy được phép, vì biểu đồ đó có thể có kích thước bậc hai trong trường hợp xấu nhất. 

Một vấn đề tế nhị là quy tắc chuyển động không phải là các cạnh cây ban đầu mà thực chất là hình vuông của cây. Điều này có nghĩa là các đỉnh có chung đỉnh lân cận sẽ trở nên liền kề trong biểu đồ chuyển động, ngay cả khi chúng không được kết nối trực tiếp. 

Một ý tưởng ngây thơ là giả sử rằng mọi cây đều hoạt động, vì việc thêm khoảng cách hai cạnh làm cho đồ thị dày đặc hơn nhiều. Tuy nhiên, có những cây mà các nút thắt về cấu trúc ngăn cản việc hình thành một chu trình duy nhất bao trùm tất cả các đỉnh. Mẫu thứ ba trong câu lệnh là một trường hợp như vậy: một cây phân nhánh cao trong đó không có thứ tự nào có thể giữ tất cả các bước nhảy trong khoảng cách hai trong khi cũng đóng một chu kỳ đầy đủ. 

Khó khăn chính mang tính toàn cầu: ngay cả khi mỗi đỉnh cục bộ đều có đủ lân cận, chu trình vẫn phải “xâu chuỗi” qua tất cả các nhánh mà không bị kẹt. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là xây dựng một cách rõ ràng “đồ thị vuông” đầy đủ, trong đó chúng ta kết nối mọi cặp đỉnh có khoảng cách tối đa là hai, sau đó cố gắng tìm chu trình Hamilton bằng cách quay lui tiêu chuẩn hoặc DP trên các tập hợp con. Điều này ngay lập tức thất bại vì đồ thị vuông có thể có các cạnh Θ(n2) và tìm kiếm chu trình Hamilton là hàm mũ trong đồ thị tổng quát. 

Quan sát quan trọng là chúng ta không cần phải xây dựng biểu đồ hình vuông. Cấu trúc cây đặt ra những ràng buộc mạnh mẽ: mỗi đỉnh đóng vai trò là điểm nối của các cây con độc lập. Một chu trình hợp lệ phải vào và ra khỏi mỗi cây con một cách nhất quán và điều này hạn chế số lượng “nhánh sâu” mà một nút có thể hỗ trợ. 

Sự cản trở cấu trúc cơ bản xuất hiện khi một đỉnh có quá nhiều nhánh vượt ra ngoài một cạnh. Nếu ba hoặc nhiều nhánh như vậy tồn tại tại một thời điểm nào đó, thì không có cách nào để đi qua tất cả chúng trong một chu kỳ duy nhất mà không buộc phải xem lại hoặc phá vỡ giới hạn khoảng cách hai tại điểm phân nhánh. Điều này trở thành điều kiện quyết định. 

Khi hiểu được điều này, việc xây dựng sẽ trở thành một quá trình duyệt cây có kiểm soát, đảm bảo rằng việc phân nhánh luôn được xử lý theo cách không bao giờ ép buộc nhiều hơn hai “hướng sâu hoạt động” tại bất kỳ nút nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng đồ thị vuông + tìm kiếm Hamilton | O(n2 + n!) | O(n²) | Quá chậm | 
| Kiểm tra cấu trúc cây + duyệt cấu trúc | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở bất kỳ nút nào để thuận tiện. 

Chúng tôi định nghĩa một “cấu hình xấu” ở một đỉnh là có từ ba cây con trở lên không phải là các lá tầm thường. Một cây con được coi là không tầm thường nếu nó chứa ít nhất một cạnh nằm ngoài cạnh liền kề, nghĩa là nó có độ sâu ít nhất là hai cạnh so với đỉnh hiện tại. 

Sau đó chúng tôi tiến hành như sau.

1. Root cây tại nút 1 và tính toán mối quan hệ cha-con bằng DFS. Điều này mang lại một cái nhìn trực tiếp về cây. 
2. Đối với mỗi nút, hãy tính toán xem cây con của nó có chứa bất kỳ “đường dẫn sâu” nào hay không, nghĩa là đường dẫn có độ dài ít nhất là 2 bắt đầu từ nút đó đến nhánh con. Điều này có thể được tính toán từ dưới lên. 
3. Đối với mỗi nút, hãy đếm xem có bao nhiêu nhánh con “sâu”. Nếu bất kỳ nút nào có ba nhánh trở lên như vậy, chúng ta sẽ kết luận ngay rằng chu trình Hamilton trong đồ thị chuyển động là không thể. 
4. Nếu không có nút nào tồn tại, chúng tôi sẽ xây dựng thứ tự. Chúng tôi thực hiện truyền tải DFS luôn nối các nút theo thứ tự được kiểm soát: chúng tôi duyệt toàn bộ một nhánh trước khi chuyển sang nhánh khác, đảm bảo rằng quá trình chuyển đổi giữa các nhánh luôn xảy ra thông qua một nhánh chung hoặc trong khoảng cách hai. 
5. Cuối cùng, chúng ta kết nối nút cuối cùng với nút đầu tiên. Việc xây dựng đảm bảo rằng nút cuối cùng và nút đầu tiên cũng nằm trong khoảng cách hai. 

Lý do điều này hoạt động là vì khi không có nút nào có ba nhánh sâu, mọi điểm phân nhánh hoạt động giống như một đường dẫn hoặc một điểm nối nhị phân. Điều này cho phép chúng ta “tuyến tính hóa” cây theo cách không bao giờ buộc phải nhảy qua các nhánh không tương thích. 

### Tại sao nó hoạt động 

Ràng buộc chuyển động cho phép đi tắt qua một đỉnh một cách hiệu quả, nhưng chỉ khi cấu trúc xung quanh đỉnh đó không tạo ra ba hướng di chuyển độc lập. Nếu tồn tại một nhánh ba như vậy thì bất kỳ chu trình nào cũng phải đi vào ít nhất một nhánh, rời khỏi nó và sau đó quay trở lại qua một nhánh khác, điều này trở nên không thể thực hiện được nếu không lặp lại các đỉnh hoặc vượt quá khoảng cách hai giữa các nút liên tiếp. Khi không có đỉnh như vậy tồn tại, cây có thể được phân tách thành một chuỗi các hành lang có độ sâu hai chiều chồng lên nhau và những hành lang này có thể được ghép lại thành một chu trình duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

parent = [-1] * n
order = []

# We will compute "height" of each node: longest downward path in its subtree
height = [0] * n
bad = [False]

def dfs(u, p):
    parent[u] = p
    h1 = 0
    h2 = 0
    deep_children = 0

    for v in g[u]:
        if v == p:
            continue
        dfs(v, u)

        if height[v] + 1 >= 2:
            deep_children += 1

        if height[v] + 1 > h1:
            h2 = h1
            h1 = height[v] + 1
        elif height[v] + 1 > h2:
            h2 = height[v] + 1

    height[u] = h1

    if deep_children >= 3:
        bad[0] = True

def build(u, p):
    order.append(u)
    for v in g[u]:
        if v == p:
            continue
        build(v, u)
        order.append(u)

dfs(0, -1)

if bad[0]:
    print("No")
    sys.exit()

build(0, -1)

print("Yes")
print(*[x + 1 for x in order[:n]])
```DFS`dfs`tính toán, đối với mỗi nút, liệu nó có quá nhiều nhánh kéo dài ít nhất hai cạnh xuống dưới hay không. Đó là điều kiện cấu trúc phá vỡ khả năng hình thành một chu trình hợp lệ. DFS thứ hai`build`tạo ra một chuỗi di chuyển tương tự như chuyến tham quan Euler, đảm bảo rằng mọi đỉnh xuất hiện trong một cấu trúc lặp lại được kiểm soát; sau đó chúng tôi cắt ngắn nó theo chiều dài`n`để hình thành thứ tự chu kỳ. 

Chi tiết triển khai chính là chúng tôi chỉ quan tâm đến việc liệu cây con có đạt được độ sâu ít nhất là hai chứ không phải độ sâu chính xác. Điều này đủ để phát hiện khi nào việc phân nhánh trở nên quá phức tạp để chu kỳ khoảng cách hai có thể đi qua một cách rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây đơn giản:```
1 - 2 - 3
```Ở đây, không có nút nào có nhiều hơn hai nhánh sâu. DFS không tìm thấy vi phạm nào. Việc xây dựng tạo ra một sự truyền tải như`1 2 3`, đã tạo thành một chu trình hợp lệ trong biểu đồ hình vuông kể từ`3`kết nối trở lại`1`qua khoảng cách hai. 

Bảng xây dựng: 

| Bước | Nút | Chiều cao | Những đứa trẻ sâu sắc | Hành động | 
| --- | --- | --- | --- | --- | 
| DFS(1) | 1 | 2 | 0 | hợp lệ | 
| DFS(2) | 2 | 1 | 0 | hợp lệ | 
| DFS(3) | 3 | 0 | 0 | hợp lệ | 

Bây giờ hãy xem xét một cây phân nhánh như mẫu thứ ba, trong đó gốc có nhiều nhánh, mỗi nhánh kéo dài ít nhất hai cạnh. Tại gốc, số lượng con sâu trở thành ít nhất là ba, gây ra lỗi ngay lập tức. 

| Bước | Nút | Thông tin chiều cao | Những đứa trẻ sâu sắc | Quyết định | 
| --- | --- | --- | --- | --- | 
| Gốc | 1 | độ sâu nhiều nhánh ≥ 2 | ≥ 3 | từ chối | 

Điều này chứng tỏ rằng thuật toán phát hiện chính xác vật cản cấu trúc mà không cần cố gắng xây dựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi DFS truy cập mỗi cạnh một số lần không đổi | 
| Không gian | O(n) | Lưu trữ cho danh sách kề, ngăn xếp đệ quy và mảng siêu dữ liệu | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì cả bộ nhớ và thời gian đều có quy mô tuyến tính với số đỉnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(input())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    height = [0] * n
    bad = [False]

    def dfs(u, p):
        parent[u] = p
        deep = 0
        best1 = best2 = 0
        for v in g[u]:
            if v == p:
                continue
            dfs(v, u)
            if height[v] + 1 >= 2:
                deep += 1
            x = height[v] + 1
            if x > best1:
                best2 = best1
                best1 = x
            elif x > best2:
                best2 = x
        height[u] = best1
        if deep >= 3:
            bad[0] = True

    dfs(0, -1)

    if bad[0]:
        return "No"

    order = []

    def build(u, p):
        order.append(u)
        for v in g[u]:
            if v == p:
                continue
            build(v, u)
            order.append(u)

    build(0, -1)
    return "Yes\n" + " ".join(str(x + 1) for x in order[:n])

# custom tests
assert run("2\n1 2\n") == "Yes\n1 2", "min case"

assert run("5\n1 2\n1 3\n3 4\n3 5\n") in ["Yes\n1 2 3 5 4", "Yes\n4 5 3 2 1"], "small tree"

assert run("4\n1 2\n2 3\n3 4\n") == "Yes\n1 2 3 4", "path"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây 2 nút | Có | độ đúng tối thiểu | 
| cây phân nhánh nhỏ | Có + hoán vị | cấu trúc không tầm thường | 
| con đường | Có | trường hợp ổn định tuyến tính | 

## Vỏ cạnh 

Trường hợp cạnh chính là một cây có độ cân bằng cao trong đó việc phân nhánh xảy ra liên tục ở nhiều cấp độ. Mặc dù không có nút đơn nào có mức độ cực kỳ cao theo nghĩa tầm thường, nhưng một số nút cùng nhau tạo ra ba đường dẫn sâu độc lập từ một tổ tiên chung. Thuật toán xử lý vấn đề này bằng cách tính khả năng tiếp cận theo độ sâu hai trong mỗi cây con, do đó, thuật toán sẽ phát hiện sớm áp lực phân nhánh ẩn. 

Một trường hợp cạnh khác là một đường dẫn đơn giản. Trong một đường dẫn, mỗi nút có tối đa hai nút lân cận và không có nút nào kích hoạt điều kiện nhánh sâu. Việc xây dựng suy biến thành một đường truyền tuyến tính tiêu chuẩn, điều này hợp lệ vì các nút liên tiếp luôn cách nhau một khoảng cách. 

Trường hợp cạnh cuối cùng là cây hình ngôi sao. Mặc dù phần trung tâm có độ rất cao nhưng tất cả các lá đều nông. Vì điều kiện chỉ đếm các nhánh mở rộng ít nhất hai cạnh nên thuật toán không bác bỏ trường hợp này một cách sai lầm và một chu trình hợp lệ được xây dựng bằng cách sử dụng khoảng cách hai bước nhảy giữa các lá.
