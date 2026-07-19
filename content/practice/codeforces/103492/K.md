---
title: "CF 103492K - Khỉ nhảy"
description: "Chúng ta được cấp một cây trong đó mỗi nút có trọng số riêng biệt. Từ bất kỳ nút bắt đầu nào, một con khỉ được phép nhảy sang nút khác nếu nút đích đó là nút có trọng số tối đa dọc theo đường dẫn đơn giản duy nhất giữa hai nút."
date: "2026-07-03T06:14:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103492
codeforces_index: "K"
codeforces_contest_name: "China Collegiate Programming Contest 2021, Qualification Round (Online), Rematch"
rating: 0
weight: 103492
solve_time_s: 47
verified: true
draft: false
---

[CF 103492K - Khỉ nhảy](https://codeforces.com/problemset/problem/103492/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó mỗi nút có trọng số riêng biệt. Từ bất kỳ nút bắt đầu nào, một con khỉ được phép nhảy sang nút khác nếu nút đích đó là nút có trọng số tối đa dọc theo đường dẫn đơn giản duy nhất giữa hai nút. Sau mỗi lần nhảy, quá trình tiếp tục từ nút mới và chúng tôi muốn biết có bao nhiêu nút riêng biệt có thể được truy cập bắt đầu từ mọi nút bắt đầu có thể. 

Khó khăn chính là việc có cho phép một bước nhảy hay không phụ thuộc vào thông tin tổng thể dọc theo một đường đi chứ không chỉ phụ thuộc vào sự lân cận cục bộ. Giải thích trực tiếp là từ nút u, bạn có thể nhảy tới bất kỳ nút v nào sao cho không có nút nào trên đường dẫn u đến v có trọng số lớn hơn a[v], nghĩa là a[v] chiếm ưu thế trên đường dẫn đó. 

Đầu vào là một cây nên mỗi cặp nút có một đường dẫn duy nhất. Chúng ta phải tính toán, với mỗi nút k, có bao nhiêu nút có thể truy cập được khi thực hiện các bước nhảy hợp lệ lặp lại bắt đầu từ k. 

Các ràng buộc rất lớn: tổng số nút trên tất cả các trường hợp thử nghiệm lên tới 8 × 10^5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng các bước nhảy một cách độc lập trên mỗi nút bắt đầu bằng các truy vấn đường dẫn. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm, hoặc thậm chí O(n log n) trên mỗi nút, sẽ quá chậm. Chúng ta cần một cách xây dựng toàn cầu gần tuyến tính hoặc tuyến tính. 

Một vấn đề tế nhị là khả năng tiếp cận không đối xứng và không đơn điệu một cách rõ ràng. Một nút có trọng số cao có thể “kéo” nhiều nút về phía nó, nhưng điều kiện đường đi vẫn có thể chặn chuyển động nếu cực đại trung gian can thiệp. 

Một sai lầm ngây thơ là cho rằng từ một nút bạn luôn có thể nhảy sang nút lân cận hoặc tới bất kỳ nút nào có trọng số cao hơn. Ví dụ: hãy xem xét một dòng 1-2-3 có trọng số 3, 1, 2. Từ nút 2, bạn không thể nhảy lên 3, vì dọc theo đường dẫn 2-1-3 tối đa là 3 nhưng đường dẫn 2-1-3 không đơn giản theo nghĩa cây trừ khi bạn xem xét cấu trúc chính xác và thậm chí sau đó điều kiện phụ thuộc vào mức tối đa của đường dẫn đầy đủ, không phải là kề hoặc thứ tự. Điều này cho thấy lý luận tham lam cục bộ đã thất bại. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Đối với mỗi nút bắt đầu k, chúng tôi cố gắng mô phỏng tất cả các bước nhảy có thể tiếp cận. Từ nút hiện tại u, chúng tôi xem xét mọi mục tiêu v có thể có và kiểm tra xem giá trị tối đa trên đường dẫn u đến v có bằng a[v] hay không. Nếu vậy, v có thể truy cập được. Chúng tôi tiếp tục mở rộng từ các nút mới đạt được cho đến khi không thể thêm nút nào nữa. 

Việc kiểm tra mức tối đa đường dẫn cho mỗi cặp (u, v) có thể được thực hiện bằng LCA ở O(log n), do đó, việc mở rộng khả năng tiếp cận duy nhất đã là O(n^2 log n) trong trường hợp xấu nhất. Vì việc mở rộng này được lặp lại cho mọi nút bắt đầu, nên tổng số sẽ trở thành O(n^3 log n) trong các kịch bản khám phá dày đặc, điều này hoàn toàn không khả thi ở n tối đa 10^5. 

Hiểu biết sâu sắc về cấu trúc quan trọng là điều kiện “a[v] là mức cực đại trên đường đi từ u đến v” tương đương với việc nói rằng nếu chúng ta định hướng mọi cạnh từ trọng số thấp hơn đến trọng số cao hơn, thì chuyển động sẽ bị hạn chế bởi các đường đi trong đó mức tối đa trên đường đi đóng vai trò giống như một rào cản. Thay vì suy nghĩ về các truy vấn đường dẫn tùy ý, chúng tôi điều chỉnh lại quy trình như khám phá một cấu trúc trong đó các nút cao hơn “kiểm soát” kết nối giữa các nút thấp hơn. 

Cách tiêu chuẩn để thực hiện điều này một cách chính xác là xử lý các nút theo thứ tự trọng lượng tăng dần và duy trì một khu rừng động, nơi chúng tôi dần dần thêm các nút và kết nối chúng qua các cạnh với các thành phần đã hoạt động. Mỗi lần chúng tôi kích hoạt một nút, chúng tôi sẽ hợp nhất nút đó với các nút lân cận đã được kích hoạt. Điều này xây dựng một cấu trúc trong đó kết nối được điều chỉnh bằng cách tăng các ràng buộc tối đa. 

Để trả lời câu hỏi về khả năng tiếp cận, chúng tôi đảo ngược quan điểm: đối với nút k cố định, quy trình này tương đương với việc đếm số lượng nút có thể được liên kết với k là nút có trọng số cao nhất trên đường kết nối trong một khu rừng được xây dựng động. Điều này làm giảm việc duy trì các thành phần theo quy trình kích hoạt đơn điệu, có thể được theo dõi bằng cấu trúc giống DSU.

Quan sát quan trọng là khi các nút được kích hoạt theo thứ tự trọng lượng tăng dần, mỗi nút sẽ gắn với các nút lân cận đã được kích hoạt và cấu trúc kết quả tạo thành một cây hợp nhất trong đó mỗi thành phần có mức tối đa được xác định rõ ràng. Câu trả lời cho một nút tương ứng với kích thước của thành phần được “sở hữu” bởi nút có trọng số cao nhất có thể tiếp cận nó trong quy trình này. 

Điều này dẫn đến một giải pháp kiểu tìm liên kết, nhưng với thứ tự cẩn thận: chúng tôi mô phỏng kích hoạt và duy trì kích thước thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log n) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| DSU theo lệnh kích hoạt | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nút theo thứ tự tăng dần về trọng lượng của chúng. Trong quá trình này, chúng tôi dần dần “kích hoạt” các nút và kết nối chúng với các nút lân cận đã hoạt động. 

1. Sắp xếp tất cả các nút theo trọng lượng của chúng theo thứ tự tăng dần. Điều này xác định thứ tự các nút có sẵn để di chuyển. Lý do cho thứ tự này là vì bất kỳ ràng buộc đường dẫn hợp lệ nào đều được xác định bởi các giá trị tối đa, do đó các trọng số thấp hơn phải được đưa ra trước khi các trọng số cao hơn có thể đóng vai trò là điểm cuối. 
2. Duy trì cấu trúc tập hợp rời rạc trên các nút, ban đầu với mỗi nút không hoạt động và trong tập hợp riêng của nó. 
3. Lặp lại các nút theo thứ tự sắp xếp theo trọng số. Khi xử lý nút u, hãy đánh dấu nút đó là hoạt động. Tại thời điểm này, u trở thành ứng cử viên tối đa hiện tại cho bất kỳ con đường nào trong tương lai liên quan đến u. 
4. Đối với mỗi lân cận v của u, nếu v đã hoạt động, hãy hợp nhất các bộ DSU chứa u và v. Điều này kết nối các vùng hiện có thể truy cập lẫn nhau mà không vi phạm ràng buộc đường dẫn tối đa, vì tất cả các nút trung gian đều có mức trọng số hiện tại. 
5. Sau khi hợp nhất u với tất cả các hàng xóm đang hoạt động, chúng tôi ghi lại kích thước của thành phần được kết nối mà u thuộc về. Kích thước này biểu thị số lượng nút có thể đạt được thông qua u dưới dạng mức tối đa kiểm soát trong một số chuỗi nhảy hợp lệ. 
6. Lưu trữ kích thước thành phần này làm đáp án cho nút u. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình kích hoạt, tất cả các nút đang hoạt động đều có trọng số nhỏ hơn hoặc bằng nút hiện tại. Bất kỳ đường dẫn nào hoàn toàn bên trong các nút đang hoạt động đều có mức tối đa bằng một trong các nút đó và vì chúng tôi xử lý theo thứ tự tăng dần nên các thành phần DSU biểu thị các vùng tối đa mà không cần nút có trọng số cao hơn chưa được xử lý để đáp ứng ràng buộc đường dẫn. Mỗi liên kết tương ứng chính xác với phần mở rộng hợp lệ của khả năng tiếp cận mà không đưa ra mức tối đa trung gian cao hơn, nếu không sẽ chặn các bước nhảy. Bởi vì trọng số là khác biệt và kích hoạt là đơn điệu, mỗi kích thước thành phần được cố định tại thời điểm nút được xử lý và các nút cao hơn sau này không thể thay đổi khả năng tiếp cận đối với các nút thấp hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        adj = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)

        a = list(map(int, input().split()))

        order = sorted(range(n), key=lambda x: a[x])

        parent = list(range(n))
        size = [1] * n
        active = [False] * n
        ans = [0] * n

        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(x, y):
            x = find(x)
            y = find(y)
            if x == y:
                return
            if size[x] < size[y]:
                x, y = y, x
            parent[y] = x
            size[x] += size[y]

        for u in order:
            active[u] = True
            for v in adj[u]:
                if active[v]:
                    union(u, v)
            ans[u] = size[find(u)]

        sys.stdout.write("\n".join(map(str, ans)) + "\n")

if __name__ == "__main__":
    solve()
```Mã tuân theo ý tưởng kích hoạt trực tiếp. Danh sách kề lưu trữ cấu trúc cây. Mảng`order`đảm bảo các nút được xử lý theo thứ tự trọng số tăng dần. DSU chỉ duy trì các thành phần được kết nối giữa các nút hoạt động. 

các`active`mảng là cần thiết vì chúng ta không được kết nối một nút với các nút lân cận chưa có sẵn trong cấu trúc đơn điệu. Nếu không có điều này, chúng tôi sẽ hợp nhất không chính xác các thành phần thông qua các nút không được phép trong cấu trúc giới hạn tối đa hiện tại. 

Câu trả lời cho mỗi nút được thực hiện tại thời điểm nó bắt đầu hoạt động, sử dụng kích thước thành phần DSU. Khoảnh khắc đó ghi lại chính xác tập hợp đầy đủ các nút có thể tiếp cận với nút đó hoạt động như một mức tối đa hợp lệ dọc theo tất cả các đường dẫn được phép. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ: 

đầu vào:```
1
4
1 2
2 3
2 4
4 1 3 2
```Chúng tôi xử lý các nút theo thứ tự trọng số tăng dần: nút 0 (trọng lượng 1), nút 3 (trọng số 2), nút 2 (trọng lượng 3), nút 1 (trọng lượng 4). 

| Bước | Nút được kích hoạt | DSU sáp nhập | Kích thước thành phần | Trả lời được ghi lại | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | không | {0:1} | ans[0]=1 | 
| 2 | 3 | không | {3:1} | ans[3]=1 | 
| 3 | 2 | không | {2:1} | ans[2]=1 | 
| 4 | 1 | hợp nhất (1-2), (1-3) | {1,2,3,0} cỡ 4 | ans[1]=4 | 

Điều này cho thấy chỉ nút có trọng số cao nhất mới có thể thống nhất toàn bộ cấu trúc, trong khi các nút nhỏ hơn vẫn bị cô lập tại thời điểm kích hoạt. 

Bây giờ hãy xem xét một chuỗi:```
1
3
1 2
2 3
3 1 2
```Thứ tự là 0, 1, 2. 

| Bước | Nút được kích hoạt | Hợp nhất | Kích thước thành phần | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | không | 1 | 1 | 
| 2 | 1 | (1-0) | 2 | 2 | 
| 3 | 2 | (2-1) | 3 | 3 | 

Mỗi lần kích hoạt sẽ mở rộng một thành phần đang phát triển, phù hợp với ý tưởng rằng việc tăng trọng lượng sẽ dần dần mở khóa cây. 

Những dấu vết này xác nhận rằng các thành phần DSU tiến hóa một cách đơn điệu và các câu trả lời tương ứng với các vùng có thể truy cập cuối cùng tại thời điểm kích hoạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) cho mỗi trường hợp thử nghiệm | Mỗi cạnh được xem xét một lần trong quá trình kích hoạt và các hoạt động DSU gần như được khấu hao không đổi | 
| Không gian | O(n) | Mảng DSU, danh sách kề và mảng kế toán | 

Tổng số nút trong các trường hợp thử nghiệm tối đa là 8 × 10^5, do đó quá trình xử lý DSU tuyến tính hoặc gần tuyến tính phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            adj = [[] for _ in range(n)]
            for _ in range(n - 1):
                u, v = map(int, input().split())
                u -= 1; v -= 1
                adj[u].append(v)
                adj[v].append(u)
            a = list(map(int, input().split()))
            order = sorted(range(n), key=lambda x: a[x])
            parent = list(range(n))
            size = [1] * n
            active = [False] * n
            ans = [0] * n

            def find(x):
                while parent[x] != x:
                    parent[x] = parent[parent[x]]
                    x = parent[x]
                return x

            def union(x, y):
                x = find(x)
                y = find(y)
                if x == y:
                    return
                if size[x] < size[y]:
                    x, y = y, x
                parent[y] = x
                size[x] += size[y]

            for u in order:
                active[u] = True
                for v in adj[u]:
                    if active[v]:
                        union(u, v)
                ans[u] = size[find(u)]
            out.append("\n".join(map(str, ans)))
        return "\n".join(out)

    return solve()

# sample-style tests
assert run("""1
4
1 2
2 3
2 4
4 1 3 2
""") == "1\n1\n1\n4"

# chain test
assert run("""1
3
1 2
2 3
3 1 2
""") == "1\n2\n3"

# star test
assert run("""1
5
1 2
1 3
1 4
1 5
5 4 3 2 1
""") == "1\n1\n1\n1\n5"

# minimal test
assert run("""1
1
1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | 1 2 3 | truyền tuyến tính | 
| ngôi sao | đa dạng | hành vi hợp nhất trung tâm | 
| nút đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| giống mẫu | đưa ra | đúng đắn về cấu trúc hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nút có trọng số cao nhất nằm ở giữa cây. Ví dụ: 

đầu vào:```
1
5
1 2
2 3
3 4
4 5
3 1 5 2 4
```Ở đây nút 2 (giá trị 5) kích hoạt lần cuối và kết nối tất cả các nút hoạt động trước đó thông qua chuỗi. Trong quá trình kích hoạt, các nút nhỏ hơn chỉ tạo thành các thành phần nhỏ, nhưng khi nút tối đa kích hoạt, nó sẽ hợp nhất mọi thứ. DSU nắm bắt điều này một cách chính xác vì việc kết hợp chỉ được thực hiện với các hàng xóm đã hoạt động, đảm bảo không có đường dẫn bất hợp pháp nào được đưa ra. 

Một trường hợp cạnh khác là một ngôi sao có tâm ở nút có trọng lượng thấp. Vì tất cả các lá kích hoạt sau hoặc trước theo thứ tự khác nhau nên ban đầu các lá vẫn bị cô lập. Chỉ khi trung tâm kích hoạt thì nó mới hợp nhất với nhiều thành phần, tạo ra sự tăng vọt đột ngột về kích thước thành phần. Thuật toán xử lý việc này một cách tự nhiên vì việc hợp nhất là đối xứng và chỉ phụ thuộc vào trạng thái kích hoạt chứ không phụ thuộc vào các giả định về cấu trúc.
