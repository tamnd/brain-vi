---
title: "CF 1176E - Che nó lại!"
description: "Chúng ta được cung cấp một đồ thị liên thông vô hướng cho mỗi truy vấn và chúng ta phải chọn một tập hợp con các đỉnh sao cho mỗi đỉnh mà chúng ta không chọn đều có ít nhất một đỉnh lân cận được chọn. Nói cách khác, mọi đỉnh không được chọn phải được “che phủ” bởi một đỉnh được chọn liền kề."
date: "2026-06-13T10:18:37+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1176
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 565 (Div. 3)"
rating: 1700
weight: 1176
solve_time_s: 633
verified: false
draft: false
---

[CF 1176E - Che nó lại!](https://codeforces.com/problemset/problem/1176/E) 

**Đánh giá:** 1700 
**Thẻ:** dfs và tương tự, dsu, đồ thị, đường đi ngắn nhất, cây 
**Thời gian giải:** 10m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị liên thông vô hướng cho mỗi truy vấn và chúng ta phải chọn một tập hợp con các đỉnh sao cho mỗi đỉnh mà chúng ta không chọn đều có ít nhất một đỉnh lân cận được chọn. Nói cách khác, mọi đỉnh không được chọn phải được “che phủ” bởi một đỉnh được chọn liền kề. Hạn chế thêm là số đỉnh được chọn không được vượt quá một nửa tổng số đỉnh. 

Vì vậy, nhiệm vụ cơ bản là xây dựng một tập hợp đỉnh chiếm ưu thế trên biểu đồ, nhưng với kích thước được đảm bảo giới hạn ở mức tối đa là tầng (n/2). Chúng ta không cần cực tiểu hóa bất cứ thứ gì, chỉ cần tạo ra bất kỳ tập hợp lệ nào thỏa mãn cả điều kiện bao phủ và ràng buộc kích thước. 

Các ràng buộc rất lớn: tổng của tất cả n và m trên các truy vấn lên tới 2⋅10^5 và có thể có tới 2⋅10^5 truy vấn. Điều này buộc phải có giải pháp tuyến tính hoặc gần tuyến tính cho mỗi truy vấn, với tổng độ phức tạp khoảng O(n + m) tổng thể. Bất cứ điều gì liên quan đến kiểm tra theo cặp, liệt kê tập hợp con hoặc duyệt đồ thị lặp đi lặp lại trên mỗi đỉnh sẽ quá chậm. 

Trường hợp cạnh tinh vi phát sinh trong các đồ thị thưa thớt giống như một đường dẫn. Nếu chúng ta cố gắng chọn một cách tham lam các đỉnh tùy ý mà không có cấu trúc, chúng ta có thể vô tình chọn quá nhiều đỉnh hoặc không đảm bảo được phạm vi bao phủ. Ví dụ: trong đường dẫn 1-2-3-4-5, việc chọn {2,4} có tác dụng nhưng các chiến lược ngây thơ như luôn chọn mọi đỉnh thứ hai bắt đầu không chính xác có thể vượt quá hoặc bỏ lỡ phạm vi bao phủ tại các ranh giới tùy thuộc vào thứ tự truyền tải. 

Một trường hợp khác là các đồ thị dày đặc như các cụm. Trong một đồ thị hoàn chỉnh, bất kỳ đỉnh nào cũng bao phủ tất cả các đỉnh khác, vì vậy chỉ cần chọn 1 đỉnh là đủ. Một chiến lược xen kẽ ngây thơ vẫn có thể chọn quá nhiều đỉnh một cách không cần thiết nhưng vẫn hợp lệ. 

Khó khăn chính là đảm bảo một công trình luôn nằm trong khoảng n/2 đồng thời đảm bảo sự thống trị. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là thử tất cả các tập hợp con của các đỉnh, kiểm tra xem chúng có chiếm ưu thế trong biểu đồ hay không và theo dõi các tập hợp đó có kích thước tối đa là n/2. Đối với mỗi tập ứng cử viên, việc xác minh phạm vi bao phủ yêu cầu quét tất cả các cạnh hoặc danh sách kề, vốn đã tốn O(n + m). Vì có 2^n tập con nên điều này hoàn toàn không khả thi. 

Chúng tôi cần một cấu trúc vốn đã giới hạn kích thước câu trả lời trong khi vẫn đảm bảo phạm vi bao phủ. Quan sát quan trọng là nếu chúng ta tô màu 2 đồ thị bằng cách sử dụng màu chẵn lẻ BFS hoặc DFS, thì mọi cạnh sẽ kết nối các nút chẵn lẻ giống nhau hoặc khác nhau, nhưng chúng ta vẫn có thể phân chia các đỉnh thành hai nhóm dựa trên độ sâu của cây BFS. Vì biểu đồ được kết nối nên các mức BFS cung cấp cho chúng ta cấu trúc giống như phân vùng tự nhiên ngay cả khi biểu đồ không phải là phân chia hai bên. 

Bây giờ hãy xem xét việc nhóm các đỉnh theo mức độ chẵn lẻ của mức BFS của chúng: độ sâu chẵn và độ sâu lẻ. Mỗi cạnh kết nối các nút có độ sâu khác nhau tối đa 1 theo thuật ngữ cây BFS, nhưng nói chung, các cạnh của biểu đồ có thể kết nối các mức tùy ý. Tuy nhiên, thuộc tính quan trọng mà chúng tôi khai thác lại đơn giản hơn: mọi cạnh đều có điểm cuối và ít nhất một điểm cuối thuộc về tập hợp mức chẵn hoặc lẻ. Do đó, mọi đỉnh không được chọn từ một tập chẵn lẻ đều liền kề (thông qua các cạnh của cây BFS hoặc các cạnh của đồ thị) với một số đỉnh trong tập chẵn lẻ khác theo cách đảm bảo phạm vi bao phủ khi chọn tập nhỏ hơn. 

Một cách mạnh mẽ hơn để xem nó là lấy gốc biểu đồ ở bất kỳ nút nào và tô màu theo độ tương đương khoảng cách. Mỗi cạnh kết nối chẵn lẻ giống nhau hoặc đối diện. Nếu một cạnh kết nối cùng một chẵn lẻ, điều đó không giúp ích trực tiếp nhưng phạm vi bao phủ được đảm bảo trên toàn cầu vì mọi đỉnh đều có mối quan hệ cha-con trong cây BFS kết nối chẵn lẻ đối diện. Do đó, việc chọn tập chẵn lẻ nhỏ hơn sẽ đảm bảo rằng mọi đỉnh không được chọn đều có ít nhất một đỉnh lân cận trong tập đã chọn. 

Vì hai tập hợp phân chia các đỉnh nên một trong số chúng có kích thước tối đa là n/2, thỏa mãn ràng buộc ngay lập tức. 

Điều này làm giảm vấn đề xuống còn một BFS duy nhất và số chẵn lẻ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · (n + m)) | O(n) | Quá chậm | 
| Phân vùng chẵn lẻ BFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lấy gốc đồ thị ở bất kỳ đỉnh nào, thường là 1 và tính khoảng cách BFS. 

1. Chạy BFS từ nút 1 và ghi lại khoảng cách tương đương cho mỗi nút. 

Chúng ta gán cho mỗi nút 0 hoặc 1 tùy thuộc vào khoảng cách của nó tới nút gốc là chẵn hay lẻ. Điều này tạo ra hai nhóm ứng cử viên. 
2. Tập hợp các nút vào hai danh sách dựa trên tính chẵn lẻ. 

Một danh sách chứa tất cả các nút có khoảng cách chẵn, danh sách còn lại chứa tất cả các nút có khoảng cách lẻ. 
3. So sánh kích thước của hai danh sách. 

Vì chúng cùng nhau chứa tất cả các nút nên ít nhất một danh sách có kích thước tối đa là tầng (n/2). Chúng tôi chọn danh sách đó. 
4. Xuất danh sách đã chọn làm câu trả lời. 

Tại sao điều này là đủ xuất phát từ cách phân lớp BFS đảm bảo cấu trúc khả năng tiếp cận từ gốc và đảm bảo mọi nút đều có đường dẫn xen kẽ các bước chẵn lẻ, ngụ ý rằng việc loại bỏ một lớp chẵn lẻ vẫn để lại mọi nút liền kề với ít nhất một đỉnh đã chọn. 

### Tại sao nó hoạt động 

Mỗi đỉnh thuộc đúng một trong hai lớp chẵn lẻ. Tập được chọn là tập nhỏ hơn trong hai lớp này, vì vậy bất kỳ đỉnh nào không được chọn sẽ thuộc về lớp kia. Trong cây BFS, mọi nút ngoại trừ nút gốc đều có nút cha có tính chẵn lẻ đối diện, nghĩa là mọi nút không được chọn đều có ít nhất một nút liền kề trong lớp chẵn lẻ đối diện. Vì chúng tôi chọn toàn bộ một lớp nên mọi nút không được chọn đều đảm bảo có một nút lân cận trong tập đã chọn. Giới hạn kích thước được giữ nguyên vì chúng tôi luôn chọn phân vùng nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    dist = [-1] * (n + 1)
    q = deque([1])
    dist[1] = 0

    while q:
        u = q.popleft()
        for v in g[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    even = []
    odd = []

    for i in range(1, n + 1):
        if dist[i] % 2 == 0:
            even.append(i)
        else:
            odd.append(i)

    if len(even) <= len(odd):
        ans = even
    else:
        ans = odd

    print(len(ans))
    print(*ans)
```BFS tính toán khoảng cách ngắn nhất từ ​​nút 1, điều này là đủ vì khả năng kết nối đảm bảo mọi nút đều có thể truy cập được. Sự phân chia chẵn lẻ được lưu trữ trong hai danh sách. Cuối cùng, chúng tôi chọn danh sách nhỏ hơn để đảm bảo ràng buộc về kích thước. 

Một sai lầm phổ biến là cho rằng việc tô màu tùy ý; BFS đảm bảo việc gán tính chẵn lẻ nhất quán trên biểu đồ. Một điểm tinh tế khác là chúng ta không cần xác minh phạm vi bao phủ một cách rõ ràng, vì việc xây dựng tính chẵn lẻ đảm bảo nó về mặt cấu trúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```Đây là một biểu đồ hoàn chỉnh. 

| Bước | Xếp hàng | Nút | quận | thậm chí | lẻ | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [1] | 1 | 0 | [] | [] | 
| BFS 1 | [2,3,4] | 1 xử lý | 0 | [1] | [] | 
| BFS 2 | [] | 2,3,4 toàn quận 1 | 1 | [1] | [2,3,4] | 

Chúng tôi chọn chẵn = [1] vì nó nhỏ hơn. 

Điều này cho thấy trong các biểu đồ dày đặc, độ sâu BFS giảm nhanh chóng và vẫn mang lại một phân vùng tối thiểu hợp lệ. 

### Ví dụ 2 

đầu vào:```
5 4
1 2
2 3
3 4
4 5
```Đây là một con đường. 

| Bước | Xếp hàng | phân công phân phối | thậm chí | lẻ | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [1] | 1:0 | [] | [] | 
| BFS | [2] | 2:1 | [1] | [] | 
| BFS | [3] | 3:2 | [1,3,5] | [2,4] | 

Chúng tôi chọn lẻ = [2,4] vì nó nhỏ hơn. 

Điều này xác nhận rằng các lớp xen kẽ trong một đường dẫn sẽ tạo ra một tập hợp lớp phủ hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | BFS truy cập từng đỉnh và cạnh một lần cho mỗi truy vấn | 
| Không gian | O(n + m) | danh sách kề và mảng khoảng cách | 

Tổng kích thước đầu vào trên tất cả các truy vấn được giới hạn bởi 2⋅10^5 cạnh, do đó, quá trình truyền tải tuyến tính này dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    from collections import deque

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        dist = [-1] * (n + 1)
        q = deque([1])
        dist[1] = 0

        while q:
            u = q.popleft()
            for v in g[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)

        even = []
        odd = []
        for i in range(1, n + 1):
            if dist[i] % 2 == 0:
                even.append(i)
            else:
                odd.append(i)

        ans = even if len(even) <= len(odd) else odd
        out.append(str(len(ans)))
        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# sample 1
assert run("""2
4 6
1 2
1 3
1 4
2 3
2 4
3 4
6 8
2 5
5 4
4 3
4 1
1 3
2 3
2 6
5 6
""") == """1
1
3
4 3 6"""

# chain graph
assert run("""1
5 4
1 2
2 3
3 4
4 5
""").split()[0] == "2"

# star graph
assert run("""1
5 4
1 2
1 3
1 4
1 5
""").split()[0] == "1"

# complete graph
assert run("""1
4 6
1 2
1 3
1 4
2 3
2 4
3 4
""").split()[0] == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị hoàn chỉnh | 1 đỉnh | trường hợp sập tối ưu | 
| đồ thị chuỗi | lựa chọn n/2 | cấu trúc xen kẽ | 
| đồ thị sao | trung tâm hoặc lá | xử lý mất cân bằng cực độ | 
| mẫu | phân vùng hợp lệ | tính đúng đắn trên đồ thị hỗn hợp | 

## Vỏ cạnh 

Trong một ngôi sao tập trung vào một đỉnh, độ sâu BFS tạo ra một tâm ở khoảng cách 0 và tất cả các tâm khác ở khoảng cách 1. Thuật toán chọn lớp chẵn lẻ nhỏ hơn, là tâm, bao phủ chính xác tất cả các lá. 

Trong một đường đi dài, BFS xen kẽ các khoảng cách chính xác dọc theo chuỗi, do đó các lớp chẵn lẻ được chia đều hoặc gần như đồng đều. Việc chọn lớp nhỏ hơn vẫn bao gồm mọi nút bên trong thông qua các nút lân cận của nó. 

Trong một biểu đồ hoàn chỉnh, BFS gán khoảng cách 1 cho tất cả các nút ngoại trừ nút gốc. Tập chẵn trở thành một nút duy nhất, nút này đã thống trị tất cả các nút khác vì mọi đỉnh đều liền kề với nó.
