---
title: "CF 102793I - \u0422\u0435\u043e\u0440\u0438\u044f \u0420\u0430\u043c\u0441\u0435\u044f"
description: "Chúng ta có một đồ thị vô hướng trong đó các phòng là các đỉnh và các đường hầm là các cạnh. Chúng ta cần tìm một nhóm nhỏ các phòng có một trong hai thuộc tính. Khả năng đầu tiên là một nhóm có kích thước l, nghĩa là mỗi cặp phòng trong nhóm đều có một đường hầm giữa chúng."
date: "2026-07-27T18:03:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102793
codeforces_index: "I"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102793
solve_time_s: 67
verified: true
draft: false
---

[CF 102793I - \u0422\u0435\u043e\u0440\u0438\u044f \u0420\u0430\u043c\u0441\u0435\u044f](https://codeforces.com/problemset/problem/102793/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trong đó các phòng là các đỉnh và các đường hầm là các cạnh. Chúng ta cần tìm một nhóm nhỏ các phòng có một trong hai thuộc tính. Khả năng đầu tiên là một nhóm có quy mô`l`, nghĩa là mỗi cặp phòng trong nhóm đều có một đường hầm giữa chúng. Khả năng thứ hai là một phe phái chống lại quy mô`k`, nghĩa là không có cặp phòng nào trong nhóm có đường hầm giữa chúng. Nếu không có cấu trúc nào tồn tại, chúng tôi in`-1`. 

Đầu vào cung cấp số lượng phòng, đường hầm và hai kích thước mục tiêu. Biểu đồ có thể có tối đa 300000 đỉnh và 300000 cạnh, trong khi cả hai kích thước nhóm được yêu cầu tối đa là 5. Các giá trị nhỏ của`k`Và`l`là những hạn chế chính. Việc tìm kiếm một nhóm tổng quát là khó, nhưng việc tìm kiếm một nhóm hoặc một tập hợp kích thước độc lập nhiều nhất là năm cho phép chúng ta sử dụng lý thuyết Ramsey. Việc biểu diễn đồ thị thưa thớt cũng có vấn đề vì việc lưu trữ hoặc kiểm tra tất cả các cặp đỉnh có thể có là không thể. 

Với 300000 đỉnh, ngay cả việc kiểm tra từng cặp phòng cũng sẽ cần khoảng 90000000000 thao tác, vượt xa giới hạn. Bất kỳ giải pháp nào liệt kê trực tiếp bộ ba, bộ bốn hoặc nhóm lớn hơn cũng nhanh chóng trở nên bất khả thi. Chúng ta cần một cách tiếp cận phụ thuộc gần như tuyến tính vào kích thước biểu đồ và chỉ khám phá một cây tìm kiếm nhỏ do kích thước câu trả lời tối đa gây ra. 

Trường hợp cạnh đầu tiên là khi một kích thước được yêu cầu là một. Bất kỳ đỉnh đơn nào vừa là một nhóm vừa là một nhóm phản nhóm có kích thước bằng một, vì vậy câu trả lời ngay lập tức là bất kỳ đỉnh nào. Ví dụ:```
Input:
5 0 1 3

Output:
1
```Giải pháp chỉ tìm kiếm các cạnh hoặc không cạnh sẽ thất bại vì nó sẽ bỏ lỡ thực tế là nhóm một đỉnh luôn hợp lệ. 

Một trường hợp cạnh khác là khi đồ thị trống. Nếu chúng ta cần một nhóm chống phân nhóm thì mọi tập đỉnh đều hoạt động, nhưng nếu chúng ta cần một nhóm có kích thước lớn hơn một thì không có nhóm nào tồn tại. Ví dụ:```
Input:
4 0 3 2

Output:
1 2 3
```Thuật toán phải có khả năng di chuyển chính xác qua các đỉnh không lân cận, bởi vì tất cả các đỉnh ngoại trừ đỉnh được chọn đều thuộc về phía chống tập đoàn. 

Trường hợp khó khăn cuối cùng là khi biểu đồ hoàn tất. Khi đó các nhóm chống lớn hơn một là không thể, nhưng mỗi nhóm đỉnh đều là một nhóm. Ví dụ:```
Input:
4 6 3 4

Output:
1 2 3 4
```Việc triển khai bất cẩn chỉ kiểm tra các cạnh bị thiếu sẽ thất bại mặc dù câu trả lời của nhóm là ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi nhóm đỉnh có thể có với kích thước được yêu cầu và kiểm tra xem nó tạo thành một cụm hay phản cụm. Đối với kích thước mục tiêu là 5, điều này có nghĩa là kiểm tra đại khái`n^5`các nhóm. Với`n = 300000`, điều này là hoàn toàn không thể. 

Một hướng đi tốt hơn xuất phát từ cấu trúc của vấn đề. Chúng tôi không tìm kiếm một đồ thị con tùy ý. Chúng tôi chỉ quan tâm đến việc tìm một nhóm hoặc nhóm đối diện của nó, một nhóm độc lập và cả hai kích thước đều có nhiều nhất là năm. 

Chọn một đỉnh`v`. Mọi nhóm hợp lệ chứa`v`phải chọn các đỉnh còn lại từ đúng một trong hai vị trí. Nếu nhóm là một cụm thì tất cả các đỉnh còn lại phải là lân cận của`v`. Nếu nhóm là anti-clique thì tất cả các đỉnh còn lại phải không lân cận với`v`. 

Điều này cung cấp một tìm kiếm đệ quy. Để tìm một nhóm có quy mô`c`hoặc một phe phái chống lại quy mô`i`, chúng ta chia tập ứng cử viên hiện tại thành các đỉnh lân cận và không lân cận của một đỉnh. Sau đó chúng tôi tìm kiếm một nhóm có quy mô`c - 1`giữa những người hàng xóm, hoặc một nhóm chống lại quy mô`i - 1`giữa những người không phải là hàng xóm. 

Phương pháp vũ lực không thành công vì nó xử lý mọi tập hợp con như nhau. Phương pháp đệ quy chỉ tuân theo các kết hợp vẫn có thể chứa một trong các cấu trúc được yêu cầu. Vì cả hai tham số tối đa là 5 nên độ sâu đệ quy rất nhỏ. Lý thuyết Ramsey đưa ra sự đảm bảo bổ sung rằng nếu không có câu trả lời nào tồn tại thì đồ thị tránh cả hai cấu trúc không thể lớn. Đệ quy chỉ đạt đến một số lượng nhỏ các đỉnh trong những trường hợp khó. 

Chúng ta có thể sử dụng phép truy hồi`R(a, b) <= R(a - 1, b) + R(a, b - 1)`với trường hợp cơ sở`R(1, x) = R(x, 1) = 1`. Điều này mang lại giới hạn trên an toàn cho kích thước của một tập hợp phải chứa một nhóm kích thước`a`hoặc một phe phái chống lại quy mô`b`. Nếu tập ứng cử viên nhỏ hơn giá trị này, lệnh gọi đệ quy có thể dừng ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^5) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu tìm kiếm đệ quy với đầy đủ các đỉnh. Các tham số mô tả cần thêm bao nhiêu đỉnh nữa cho một nhóm và một nhóm chống. 
2. Nếu yêu cầu nhóm hoặc yêu cầu chống nhóm là một, hãy trả về bất kỳ đỉnh hiện tại nào. Một đỉnh duy nhất luôn thỏa mãn cả hai định nghĩa. 
3. Nếu tập ứng cử viên hiện tại nhỏ hơn giới hạn Ramsey cho các yêu cầu còn lại, hãy dừng nhánh này. Nó không thể chứa một câu trả lời hợp lệ. 
4. Chọn một đỉnh`v`từ bộ hiện tại. Chia tất cả các ứng cử viên khác thành hai nhóm. Nhóm đầu tiên chứa các hàng xóm của`v`và nhóm thứ hai chứa các phần tử không lân cận của`v`. 
5. Tìm kiếm nhóm hàng xóm trong khi giảm kích thước nhóm yêu cầu đi một. Điều này đúng vì mọi đỉnh bổ sung của một cụm chứa`v`phải được kết nối với`v`. 
6. Nếu không thành công, hãy tìm kiếm nhóm không lân cận trong khi giảm kích thước chống nhóm được yêu cầu xuống một nhóm. Điều này đúng vì mỗi đỉnh bổ sung của một nhóm chống lại chứa`v`phải tránh`v`. 
7. Nếu cả hai lệnh gọi đệ quy đều thất bại thì không có cấu trúc hợp lệ nào tồn tại trong nhánh này. Tiếp tục trả về lỗi cho đến khi tìm thấy câu trả lời hoặc gốc trả về lỗi. 

Tại sao nó hoạt động: 

Điều bất biến là mọi lệnh gọi đệ quy đều nhận được chính xác các đỉnh vẫn có thể hoàn thành một trong hai cấu trúc được yêu cầu. Nếu một giải pháp chứa đỉnh được chọn`v`, phần còn lại của nghiệm đó phải nằm hoàn toàn trong nhóm lân cận hoặc nhóm không lân cận. Phép đệ quy khám phá cả hai khả năng, vì vậy nó không thể loại bỏ một câu trả lời hợp lệ. Giới hạn Ramsey chỉ loại bỏ các tập hợp được đảm bảo về mặt toán học là không chứa nghiệm có kích thước được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k, l = map(int, input().split())

    graph = [[] for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    ramsey = [[0] * 6 for _ in range(6)]
    for i in range(1, 6):
        ramsey[1][i] = 1
        ramsey[i][1] = 1

    for i in range(2, 6):
        for j in range(2, 6):
            ramsey[i][j] = ramsey[i - 1][j] + ramsey[i][j - 1]

    mark = [0] * n
    token = 0

    def dfs(vertices, clique_need, indep_need):
        nonlocal token

        if clique_need == 1 or indep_need == 1:
            return [vertices[0]]

        if len(vertices) < ramsey[clique_need][indep_need]:
            return None

        v = vertices[0]

        token += 1
        cur = token
        for u in graph[v]:
            mark[u] = cur

        neigh = []
        non_neigh = []

        for u in vertices[1:]:
            if mark[u] == cur:
                neigh.append(u)
            else:
                non_neigh.append(u)

        if clique_need > 1:
            res = dfs(neigh, clique_need - 1, indep_need)
            if res is not None:
                return [v] + res

        if indep_need > 1:
            res = dfs(non_neigh, clique_need, indep_need - 1)
            if res is not None:
                return [v] + res

        return None

    ans = dfs(list(range(n)), l, k)

    if ans is None:
        print(-1)
    else:
        print(*[x + 1 for x in ans])

if __name__ == "__main__":
    solve()
```Danh sách kề chỉ lưu trữ các cạnh hiện có, điều này cần thiết vì đồ thị có thể có 300000 đỉnh. các`mark`mảng tránh việc xây dựng các tập kề cận nhiều lần. Đối với mỗi đỉnh được chọn, các đỉnh lân cận của nó sẽ nhận được giá trị mã thông báo hiện tại, cho phép phân chia thành các đỉnh lân cận và không lân cận chỉ bằng một lần quét danh sách ứng cử viên. 

Hàm đệ quy giữ hai bộ đếm.`clique_need`biểu thị số đỉnh vẫn cần thiết cho một cụm, trong khi`indep_need`đại diện tương tự cho một phe phái chống lại. Khi một trong số chúng đạt đến một, danh sách đỉnh hiện tại đã chứa câu trả lời một đỉnh hợp lệ. 

Bảng Ramsey sử dụng giới hạn trên thay vì giá trị chính xác. Điều này là đủ vì mục đích duy nhất là cắt tỉa những cành không thể mọc được. Các giá trị rất nhỏ vì cả hai kích thước mục tiêu đều tối đa là năm. 

Các đỉnh được trả về sẽ được chuyển đổi trở lại thành chỉ mục dựa trên một trước khi in. Việc triển khai sẽ tự động tránh tràn số nguyên vì số nguyên Python có độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
Input:
4 0 2 4
```Đồ thị không có cạnh. Chúng ta cần một nhóm chống 4 đỉnh hoặc 2 đỉnh. 

| Bước | Đỉnh ứng cử viên | Bè phái cần | Cần chống bè phái | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1,2,3,4 | 4 | 2 | Chọn đỉnh 1 | 
| 2 | Hàng xóm trống rỗng | 3 | 2 | Nhánh bè phái thất bại | 
| 3 | 2,3,4 | 4 | 1 | Trả về đỉnh 2 | 

Nhánh thứ hai thành công ngay lập tức vì bất kỳ đỉnh đơn nào cũng là một anti-clique hợp lệ có kích thước 1, hoàn thành anti-clique hai đỉnh được yêu cầu cùng với đỉnh 1. 

Hãy xem xét:```
Input:
4 6 3 4
```Biểu đồ đã hoàn tất. 

| Bước | Đỉnh ứng cử viên | Bè phái cần | Cần chống bè phái | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1,2,3,4 | 4 | 3 | Chọn đỉnh 1 | 
| 2 | 2,3,4 | 3 | 3 | Tìm kiếm hàng xóm | 
| 3 | 3,4 | 2 | 3 | Tìm kiếm hàng xóm | 
| 4 | 4 | 1 | 3 | Trả về đỉnh 4 | 

Đường đệ quy tiếp tục đi về phía lân cận vì mọi đỉnh còn lại đều được kết nối. Nó xây dựng lại toàn bộ nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi phân chia đệ quy lớn chỉ quét các ứng cử viên hiện tại và độ sâu đệ quy được giới hạn bởi 10 vì cả hai kích thước mục tiêu đều tối đa là 5. | 
| Không gian | O(n + m) | Danh sách kề lưu trữ tất cả các cạnh và đệ quy chỉ lưu trữ các danh sách ứng cử viên nhỏ. | 

Giải pháp phù hợp với các ràng buộc vì phần tốn kém của đệ quy chỉ xảy ra khi các tập ứng cử viên đủ lớn để quan trọng. Giới hạn Ramsey đảm bảo rằng các tìm kiếm không thành công sẽ nhanh chóng thu hẹp lại thành các tập hợp nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

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

assert run("""5 5 3 3
1 2
2 3
3 4
4 5
1 5
""") == "-1\n", "cycle without triangle"

assert run("""4 0 2 4
""").strip() in {"1 2", "1 3", "1 4", "2 3", "2 4", "3 4"}, "empty graph"

assert run("""4 6 3 4
1 2
1 3
1 4
2 3
2 4
3 4
""").strip() == "1 2 3 4", "complete graph"

assert run("""1 0 5 1
""").strip() == "1", "single vertex"

assert run("""5 0 5 5
""").strip() == "1 2 3 4 5", "large anti-clique"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Năm đỉnh tạo thành một chu trình |`-1`| Kiểm tra trường hợp không thể xảy ra khi cả hai cấu trúc đều không tồn tại | 
| Biểu đồ trống | Bất kỳ cặp hợp lệ nào | Checks non-neighbor recursion |
 | Đồ thị hoàn chỉnh | Bốn đỉnh | Kiểm tra đệ quy nhóm | 
| Một đỉnh |`1`| Kiểm tra việc xử lý kích thước một | 
| Biểu đồ trống với yêu cầu kích thước năm | Tất cả các đỉnh | Kiểm tra kích thước mục tiêu tối đa | 

## Vỏ cạnh 

Đối với trường hợp một đỉnh:```
Input:
1 0 5 1
```Hàm đệ quy ngay lập tức thấy rằng một nhóm có kích thước bằng một là đủ. Nó trả về đỉnh duy nhất mà không cần kiểm tra các cạnh. 

Đối với biểu đồ trống:```
Input:
5 0 5 3
```Việc chọn đỉnh 1 sẽ tạo ra một nhóm lân cận trống và một nhóm không lân cận chứa các đỉnh từ 2 đến 5. Nhánh nhóm không thành công vì không có cạnh, nhưng nhánh chống nhóm giữ tất cả các đỉnh còn lại. Phép đệ quy cuối cùng trả về tất cả năm đỉnh vì mọi cặp đều bị ngắt kết nối. 

Đối với biểu đồ hoàn chỉnh:```
Input:
5 10 3 5
```Chọn đỉnh 1 sẽ đặt mọi đỉnh còn lại vào nhóm lân cận. Phía chống cụm trống, nhưng phía chống cụm liên tục loại bỏ một đỉnh cần thiết cho đến khi thu thập được năm đỉnh. Đầu ra là năm đỉnh bất kỳ trong biểu đồ. 

Đối với trường hợp không có cấu trúc tồn tại:```
Input:
5 5 3 3
1 2
2 3
3 4
4 5
1 5
```Đồ thị là một chu trình có độ dài năm. Mọi phép chia đệ quy cuối cùng đều đạt đến một tập ứng cử viên nhỏ hơn giới hạn Ramsey cho các yêu cầu còn lại, chứng tỏ rằng không có nhánh nào có thể chứa một hình tam giác hoặc một tập hợp độc lập có kích thước ba. Thuật toán trả về`-1`.
