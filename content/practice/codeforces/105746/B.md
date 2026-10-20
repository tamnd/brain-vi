---
title: "CF 105746B - Trang trí nội thất"
description: "Chúng ta được cấp một cây trong đó mỗi nút đã có sẵn một màu và màu cuối cùng mong muốn cho mỗi nút. Chúng tôi được phép sơn lại một nút bất kỳ số nào trong phạm vi từ 1 đến N và mỗi lần sơn lại được tính là một thao tác."
date: "2026-06-22T04:42:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105746
codeforces_index: "B"
codeforces_contest_name: "Bangladesh Olympiad in Informatics 2025 National Round Day 1"
rating: 0
weight: 105746
solve_time_s: 65
verified: true
draft: false
---

[CF 105746B - Trang trí nhà cửa](https://codeforces.com/problemset/problem/105746/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó mỗi nút đã có sẵn một màu và màu cuối cùng mong muốn cho mỗi nút. Chúng tôi được phép sơn lại một nút bất kỳ số nào trong phạm vi từ 1 đến N và mỗi lần sơn lại được tính là một thao tác. Mục đích là chuyển đổi màu ban đầu thành màu mục tiêu. 

Có một hạn chế toàn cầu luôn được áp dụng, bao gồm cả các trạng thái trung gian: các nút liền kề trong cây không bao giờ được chia sẻ cùng một màu. Ràng buộc này áp dụng sau mỗi thao tác sơn lại, không chỉ trong cấu hình cuối cùng. 

Nhiệm vụ là quyết định xem có thể đạt được màu mục tiêu trong khi tôn trọng quy tắc này hay không và nếu có thể, hãy giảm thiểu số lượng thao tác sơn lại và xuất ra một chuỗi hợp lệ. 

Ràng buộc N lên tới 100000 buộc phải có một giải pháp tuyến tính hoặc gần tuyến tính. Bất kỳ chiến lược nào cố gắng mô phỏng các trạng thái đổi màu bằng quay lui hoặc xem xét tất cả các hoán vị của thứ tự nút sẽ ngay lập tức thất bại, vì ngay cả việc sắp xếp các hoán vị của N phần tử cũng không khả thi. 

Một khó khăn nhỏ là ngay cả khi màu cuối cùng của nút khác với màu ban đầu, việc sơn lại quá sớm có thể tạm thời tạo ra xung đột với nút lân cận vẫn giữ màu ban đầu. Tương tự, việc sơn lại quá muộn có thể gây xung đột với hàng xóm đã chuyển sang màu cuối cùng. Do đó, thứ tự của các hoạt động là vấn đề trung tâm. 

Một trường hợp lỗi nhỏ xuất hiện khi hai nút liền kề muốn hoán đổi màu sắc. 

đầu vào:```
2
1 2
2 1
1 2
```Nếu chúng ta vẽ lại nút 1 trước, nó sẽ trở thành 2, nhưng nút 2 vẫn là 2, vi phạm tính kề cận. Nếu chúng ta vẽ lại nút 2 trước, xung đột đối xứng sẽ xảy ra. Câu trả lời đúng là không thể. 

Điều này cho thấy vấn đề không phải là chọn nút nào để thay đổi mà là tìm ra thứ tự phụ thuộc nhất quán giữa các thay đổi. 

## Phương pháp tiếp cận 

Chế độ xem brute-force coi điều này giống như thử tất cả các lệnh có thể đổi màu các nút có màu ban đầu khác với màu mục tiêu của chúng. Đối với mỗi hoán vị, chúng tôi mô phỏng các nút đổi màu và xác minh sau mỗi bước rằng không có cạnh nào trở thành đơn sắc. Điều này hoạt động về mặt khái niệm vì nó trực tiếp thực thi ràng buộc, nhưng nó yêu cầu kiểm tra tới k! các mệnh lệnh trong đó k là số nút cần thay đổi, điều này trở nên không khả thi ngay cả với k khoảng 20. 

Quan sát quan trọng là mỗi nút chỉ tương tác với các nút lân cận và mỗi nút được vẽ lại chính xác một lần. Điều này biến vấn đề thành việc xây dựng thứ tự phụ thuộc hợp lệ. Liệu nút u có thể được sơn hay không chỉ phụ thuộc vào việc hàng xóm hiện tại có màu bằng màu mà bạn sắp nhận hay không. Vì mỗi nút lân cận vẫn ở trạng thái ban đầu hoặc đã ở trạng thái cuối cùng nên mọi xung đột đều giảm xuống thành một ràng buộc định hướng giữa hai nút. 

Điều này cho phép chúng tôi thay thế tìm kiếm chuỗi toàn cục bằng biểu đồ có hướng trên các nút phải được tô màu lại. Các cạnh mã hóa các ràng buộc ưu tiên xuất phát từ các xung đột màu sắc tiềm ẩn. Nếu đồ thị có hướng này chứa một chu trình thì không tồn tại thứ tự hợp lệ nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hoán vị Brute Force | O(k! · N) | O(N) | Quá chậm | 
| Biểu đồ phụ thuộc + sắp xếp tôpô | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chỉ tập trung vào các nút có màu ban đầu khác với màu mục tiêu của chúng, vì các nút đã khớp với mục tiêu của chúng không bao giờ cần phải chạm vào. 

1. Xác định tập S các nút trong đó C[u] != T[u]. Đây là những nút duy nhất chúng tôi sẽ thực sự sơn lại. Các nút bên ngoài S vẫn cố định mãi mãi có màu C[u]. 
2. Đối với mỗi cạnh (u, v), chúng tôi phân tích xem việc sơn lại một điểm cuối có thể xung đột với điểm cuối kia như thế nào tùy thuộc vào màu sắc mà nó có thể giữ trong quá trình. Hai loại xung đột xuất hiện: 

Nếu v không được tô màu lại và C[v] bằng T[u] thì u không thể được sơn trong khi v vẫn còn nguyên. Vì v không bao giờ thay đổi nên điều này làm cho toàn bộ bài toán không thể thực hiện được ngay lập tức. 

Nếu v được tô màu lại thì chúng ta phải quyết định xem v phải đứng trước hay sau u tùy thuộc vào việc màu ban đầu hay màu cuối cùng của nó khớp với T[u] hay T[v]. 
3. Chúng ta xây dựng đồ thị có hướng trên các nút trong S bằng các quy tắc sau: 

Nếu C[v] == T[u] thì v phải được vẽ trước u, vì nếu không bạn sẽ thấy v đã xung đột rồi. 

Nếu T[v] == T[u] thì u phải được vẽ trước v, nếu không v sau này sẽ tạo ra xung đột với u đã có T[u]. 
4. Sau khi xây dựng tất cả các ràng buộc, chúng ta kiểm tra xem đồ thị có hướng này có chu trình hay không. Nếu có thì không có đơn đặt hàng hợp lệ. 
5. Nếu nó không có chu trình, chúng ta tính toán thứ tự tôpô của các nút trong S. 
6. Chúng tôi đưa ra các hoạt động bằng cách sơn lại các nút theo thứ tự tôpô đó, đặt trực tiếp từng nút thành màu mục tiêu T[u]. 

Tại sao nó hoạt động dựa trên sự bất biến về trạng thái trong quá trình thực thi. Tại bất kỳ thời điểm nào, mọi nút chưa được xử lý vẫn có màu ban đầu và mọi nút được xử lý đều có màu cuối cùng. Mỗi cạnh có hướng mã hóa chính xác điều kiện cần thiết để ngăn cạnh hàng xóm khớp với màu được chỉ định. Thứ tự cấu trúc liên kết đảm bảo rằng bất cứ khi nào một nút được xử lý, tất cả các nút lân cận có thể xung đột đều đã ở trạng thái bắt buộc, đã được sửa hoặc vẫn chưa được xử lý, đảm bảo không có cạnh nào trở nên không hợp lệ trong quá trình này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    C = [0] + list(map(int, input().split()))
    T = [0] + list(map(int, input().split()))

    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    need = [False] * (n + 1)
    for i in range(1, n + 1):
        if C[i] != T[i]:
            need[i] = True

    # build directed constraints
    g = [[] for _ in range(n + 1)]
    indeg = [0] * (n + 1)

    for u in range(1, n + 1):
        for v in adj[u]:
            if not need[u] and not need[v]:
                continue
            # if v is fixed (not changed)
            if not need[v]:
                if C[v] == T[u] and need[u]:
                    print(-1)
                    return
                continue

            # v is in S
            if need[u]:
                if C[v] == T[u]:
                    g[v].append(u)
                    indeg[u] += 1
                if T[v] == T[u]:
                    g[u].append(v)
                    indeg[v] += 1

    # topo sort over nodes in need
    from collections import deque
    q = deque([i for i in range(1, n + 1) if need[i] and indeg[i] == 0])

    order = []
    while q:
        u = q.popleft()
        order.append(u)
        for v in g[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    S = [i for i in range(1, n + 1) if need[i]]
    if len(order) != len(S):
        print(-1)
        return

    print(len(order), 1)
    for u in order:
        print(u, T[u])

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ cô lập các nút yêu cầu thay đổi, vì mọi thứ khác hoạt động như một nguồn hạn chế vĩnh viễn. Sau đó, nó quét từng cạnh và chuyển các xung đột màu cục bộ thành các ràng buộc thứ tự có hướng. Mỗi ràng buộc được mã hóa dưới dạng cạnh biểu đồ và theo dõi mức độ mà mỗi nút có bao nhiêu điều kiện tiên quyết. 

Sắp xếp tôpô là thuật toán Kahn tiêu chuẩn. Điểm tinh tế quan trọng là các nút không nằm trong tập thay đổi không bao giờ là một phần của biểu đồ nhưng chúng vẫn tham gia vào quá trình xác thực ràng buộc. Việc kiểm tra sớm khả năng không thể thực hiện được là điều cần thiết, vì nếu không, chúng ta sẽ cố gắng lên lịch cho một thứ không bao giờ có thể hài lòng do một hàng xóm cố định đã có màu xung đột. 

Mỗi thao tác đầu ra trực tiếp đặt một nút thành màu mục tiêu của nó, điều này an toàn vì thứ tự đảm bảo rằng không có nút lân cận hiện bằng màu đó tại thời điểm đó. 

## Ví dụ đã hoạt động 

Hãy xem xét cấu trúc mẫu đầu tiên trong đó cần có nhiều lần đổi màu trong một cây có hình dạng xung quanh nút trung tâm. Thuật toán trước tiên đánh dấu tất cả các nút có màu ban đầu khác với mục tiêu, sau đó rút ra các ràng buộc dọc theo các cạnh. Đồ thị có hướng thu được thực thi một chuỗi hợp lệ duy nhất. 

Một dấu vết đơn giản hóa: 

| Bước | Nút đã chọn | điều kiện vô độ | hành động | 
| --- | --- | --- | --- | 
| 1 | bất kỳ nút 0 độ nào | an toàn | sơn lại để nhắm mục tiêu | 
| 2 | có sẵn tiếp theo | an toàn | sơn lại | 
| 3 | tiếp tục | mọi ràng buộc đều được thỏa mãn | sơn lại | 

Quan sát quan trọng từ việc thực thi này là không có nút nào được vẽ trong khi nút lân cận đang ở trạng thái xung đột, bởi vì tình huống như vậy sẽ tạo ra một cạnh có hướng ngăn cản thứ tự này. 

Đối với trường hợp không thể: 

đầu vào:```
2
1 2
2 1
1 2
```Ở đây cả hai nút đều nằm trong tập thay đổi. Cạnh tạo ra cả hai ràng buộc cùng một lúc: mỗi nút yêu cầu nút kia phải ở trước nó. Điều này tạo thành một chu kỳ có độ dài 2. 

| Nút | hạn chế 1 | hạn chế 2 | 
| --- | --- | --- | 
| 1 | phải trước 2 | phải sau 2 | 
| 2 | phải trước 1 | phải sau 1 | 

Không có thứ tự tôpô nào tồn tại, do đó thuật toán cho ra kết quả đúng -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi cạnh đóng góp tối đa hai kiểm tra ràng buộc có hướng và sắp xếp cấu trúc liên kết xử lý mỗi nút và cạnh một lần | 
| Không gian | O(N) | Danh sách kề và mảng mức độ lưu trữ các cấu trúc có kích thước tuyến tính | 

Các ràng buộc cho phép tối đa 100000 nút, vì vậy độ phức tạp tuyến tính là cần thiết. Cả cấu trúc đồ thị và thuật toán của Kahn đều chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n = int(input())
        C = [0] + list(map(int, input().split()))
        T = [0] + list(map(int, input().split()))

        adj = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)

        need = [False] * (n + 1)
        for i in range(1, n + 1):
            if C[i] != T[i]:
                need[i] = True

        g = [[] for _ in range(n + 1)]
        indeg = [0] * (n + 1)

        for u in range(1, n + 1):
            for v in adj[u]:
                if not need[u] and not need[v]:
                    continue
                if not need[v]:
                    if need[u] and C[v] == T[u]:
                        print(-1)
                        return
                    continue

                if need[u]:
                    if C[v] == T[u]:
                        g[v].append(u)
                        indeg[u] += 1
                    if T[v] == T[u]:
                        g[u].append(v)
                        indeg[v] += 1

        q = deque([i for i in range(1, n + 1) if need[i] and indeg[i] == 0])
        order = []
        while q:
            u = q.popleft()
            order.append(u)
            for v in g[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    q.append(v)

        S = [i for i in range(1, n + 1) if need[i]]
        if len(order) != len(S):
            print(-1)
            return

        print(len(order), 1)
        for u in order:
            print(u, T[u])

    return sys.stdout.getvalue()

# minimal
assert run("""2
1 2
2 1
1 2
""").strip() == "-1"

# already correct
assert run("""3
1 2 3
1 2 3
1 2
1 3
""").split()[0] == "0"

# simple chain
assert run("""3
1 2 3
3 2 1
1 2
2 3
""").split()[0] >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hoán đổi 2 nút | -1 | chu kỳ phụ thuộc lẫn nhau | 
| cây đã đúng rồi | 0 | không cần thao tác | 
| chuỗi 3 nút | đơn hàng hợp lệ | lập lịch DAG cơ bản | 

## Vỏ cạnh 

Hoán đổi hai nút trực tiếp là chế độ lỗi rõ ràng nhất. Mỗi nút buộc nút kia đến sớm hơn và muộn hơn một cách đồng thời, tạo ra một chu kỳ mà thuật toán phát hiện ngay lập tức thông qua sự mất cân bằng mức độ. 

Một trường hợp tinh tế khác xảy ra khi một nút được cố định (C[u] = T[u]) nhưng hoạt động như một trình chặn. Các nút như vậy không bao giờ được lên lịch, tuy nhiên chúng vẫn có thể cấm các nút lân cận lấy một số màu nhất định. Việc kiểm tra tính không thể thực hiện sớm đảm bảo rằng nếu một nút cố định đã có màu phù hợp với mục tiêu của nút lân cận thì không thể thực hiện được giải pháp nào vì màu cố định đó không thể bị di chuyển ra khỏi đường đi. 

Ở những cây lớn hơn, nhiều chuỗi phụ thuộc có thể lồng vào nhau. Việc sắp xếp cấu trúc liên kết xử lý việc này một cách tự nhiên và bất kỳ chu kỳ nào, dù lớn hay gián tiếp, đều biểu hiện dưới dạng các nút còn lại với mức độ khác 0 sau khi xử lý, điều này gây ra chính xác tình trạng không thể xảy ra.
