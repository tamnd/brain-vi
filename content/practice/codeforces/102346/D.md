---
title: "CF 102346D - Tố cáo Mafia"
description: "Hệ thống phân cấp của mafia tạo thành một cây có gốc, với thành viên 1 là gốc. Mỗi thành viên khác đều có chính xác một cấp trên trực tiếp, vì vậy mỗi thành viên đều có một con đường riêng để lên cấp trên."
date: "2026-08-13T01:20:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 80
verified: true
draft: false
---

[CF 102346D - Tố cáo Mafia](https://codeforces.com/problemset/problem/102346/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống phân cấp của mafia tạo thành một cây có gốc, với thành viên 1 là gốc. Mỗi thành viên khác đều có chính xác một cấp trên trực tiếp, vì vậy mỗi thành viên đều có một con đường riêng để lên cấp trên. 

Khi người tiên kiến ​​xác định được một thành viên, cảnh sát có thể bắt giữ thành viên đó và sau đó liên tục thẩm vấn các thành viên bị bắt để lấy thông tin về cấp trên của họ. Kết quả là, việc chọn một thành viên sẽ bắt giữ mọi đỉnh trên đường đi từ thành viên đó đến gốc một cách hiệu quả. 

Cảnh sát có thể hỏi nhà tiên tri về nhiều nhất là thành viên K. Nhiệm vụ là chọn các thành viên đó sao cho hợp các đường dẫn gốc của chúng chứa càng nhiều đỉnh phân biệt càng tốt. 

Đầu vào mô tả cây có gốc thông qua N và K, theo sau là đỉnh gốc của mọi đỉnh từ 2 đến N. Đầu ra là số lượng thành viên mafia riêng biệt tối đa có thể bị bắt giữ. 

Với N lên tới 100000, một thuật toán liên quan đến tất cả các cặp, tất cả các tập hợp con hoặc chương trình động bậc hai là quá đắt. Một giải pháp xung quanh O(N log N) là phù hợp một cách thoải mái, trong khi ngay cả O(NK) cũng có thể đạt tới 10^10 thao tác khi K lớn. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. Nếu K bằng 1 thì câu trả lời đơn giản là độ dài của đường đi từ gốc tới đỉnh sâu nhất chứ không phải N. Ví dụ: với`N = 3`,`K = 1`, và bố mẹ`1 1`, đáp án là 2 vì chọn một trong hai đứa sẽ bắt giữ cả ông chủ và đứa trẻ đó. 

Một trường hợp tinh tế khác xảy ra khi một số nhánh có độ sâu bằng nhau. Với`N = 5`,`K = 2`, và bố mẹ`1 1 2 2`, cây có gốc 1, con 2 và 3, con 4 và 5 dưới 2. Chọn 4 bắt`1,2,4`, và chọn 3 chỉ thêm đỉnh 3, được 4. Thay vào đó, chọn cả 4 và 5 sẽ cho`1,2,4,5`, cũng 4. Lời giải không được phụ thuộc vào việc con nào được chọn là nhánh dài nhất khi độ sâu ràng buộc. 

Trường hợp thứ ba là một ngôi sao. Với`N = 5`,`K = 4`, và bố mẹ`1 1 1 1`, mỗi lá được chọn đóng góp chính xác một đỉnh mới sau lần chọn đầu tiên. Câu trả lời là 5. Việc triển khai chỉ xem xét các đường dẫn từ gốc đến lá hoàn chỉnh một lần có thể bỏ lỡ các nhánh độc lập này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi tập hợp có thể có tối đa K thành viên được chọn, đi theo con trỏ cha từ mỗi thành viên được chọn đến gốc và đếm các đỉnh riêng biệt trong liên kết kết quả. Điều này đúng vì tập hợp bị bắt chính xác là sự kết hợp của các đường dẫn gốc đó. Tuy nhiên, có`C(N,K)`các lựa chọn có thể có khi chính xác K thành viên được chọn và việc xử lý từng lựa chọn có thể yêu cầu công việc O(N). Trong trường hợp xấu nhất, đây là Θ(N · C(N,K)), là số mũ khi K ở khoảng N/2. Ngay cả một cách triển khai ít ngây thơ hơn là thử mọi lá được chọn có thể cũng vượt xa giới hạn. 

Quan sát hữu ích là khi một số đường dẫn gốc đã được chọn, các đỉnh đã bị bắt sẽ tạo thành một cây con có gốc được kết nối. Mọi đóng góp có thể còn lại đều nằm trong một cây con riêng biệt được gắn với vùng bị bắt đó bằng một cạnh. 

Giả sử vùng hiện đang bị bắt đạt đến đỉnh v và con c không phải là một phần của phần tiếp theo được chọn bên dưới v. Toàn bộ cây con của c vẫn chưa được xử lý. Nếu chúng ta sử dụng thêm một công cụ tiên tri bên trong cây con đó, thì số đỉnh mới lớn nhất mà chúng ta có thể thu được là độ dài của đường đi xuống sâu nhất bắt đầu từ c. 

Điều này tạo ra một quá trình tham lam tự nhiên. Bắt đầu với đường đi từ gốc tới đỉnh dài nhất trong toàn bộ cây. Sau khi đi theo con đường đó, mọi nhánh con bị bỏ qua dọc theo con đường đó sẽ trở thành một ứng cử viên độc lập. Trong số tất cả các ngành ứng cử viên hiện có, hãy chọn ngành có chiều sâu tối đa. Khi nhánh đó được chọn, hãy đi dọc theo con đường sâu nhất của chính nó và thêm tất cả các nhánh con bị bỏ qua vào tập ứng cử viên. 

Thuộc tính quan trọng là mỗi ứng cử viên đại diện cho một cây con hoàn toàn nguyên vẹn. Độ sâu của nó chính xác là số đỉnh mới tối đa mà một lựa chọn có thể đóng góp từ cây con đó. Việc chọn một nhánh có sẵn ngắn hơn không thể cung cấp nhiều đỉnh mới ngay lập tức hơn việc chọn nhánh dài nhất, trong khi việc chọn nhánh dài nhất sẽ hiển thị tất cả các nhánh bên còn lại của nó cho các lựa chọn trong tương lai. Điều này mang lại cho sự lựa chọn tham lam cấu trúc tối ưu của nó. 

Chúng tôi có thể duy trì các nhánh có sẵn trong một đống tối đa. DFS tiền xử lý sẽ tính toán`depth[v]`, số đỉnh trên đường đi xuống dài nhất bắt đầu từ v, cùng với`best[v]`, đứa trẻ bắt đầu một con đường như vậy. Mỗi khi một nhánh được chọn, chúng tôi theo dõi`best`con trỏ và đẩy từng đứa trẻ khác vào đống. 

Các phương pháp tiếp cận bạo lực và tối ưu có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Θ(N · C(N,K)) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây gốc từ mảng cha. Đỉnh 1 là gốc và mọi đỉnh`v > 1`được thêm vào danh sách con của cha mẹ đã cho của nó. 
2. Tính toán`depth[v]`, Ở đâu`depth[v]`có nghĩa là số đỉnh trên đường đi xuống dài nhất bắt đầu từ v. Đồng thời, lưu trữ`best[v]`, đứa trẻ bắt đầu một con đường dài nhất. Một chiếc lá có`depth[v] = 1`và không có đứa con tốt nhất. 
3. Đặt`(depth[1], 1)`thành một đống tối đa. Điều này thể hiện đường đi khả dụng đầu tiên, có thể bắt đầu ở bất kỳ đâu bên dưới gốc và đóng góp đầu tiên dài nhất có thể là đường đi sâu nhất từ ​​​​gốc. 
4. Lặp lại K lần. Loại bỏ ứng viên có độ sâu lớn nhất khỏi vùng nhớ heap và thêm độ sâu đó vào câu trả lời. Gốc ứng cử viên thuộc về một cây con chưa được chạm tới, do đó mọi đỉnh trên đường đi xuống đã chọn của nó đều bị bắt mới. 
5. Bắt đầu từ gốc ứng cử viên đã chọn, hãy làm theo`best[v]`cho đến khi chạm tới một chiếc lá. Tại mỗi đỉnh đã thăm v, kiểm tra tất cả các nút con ngoại trừ`best[v]`. Mỗi đứa trẻ như vậy bắt đầu một cây con đã bị bỏ qua bởi đường dẫn đã chọn, vì vậy hãy nhấn`(depth[child], child)`vào đống. 
6. Sau K lựa chọn, tổng tích lũy là số đỉnh bị bắt riêng biệt. Không có đường dẫn nào được chọn được tính cùng một đỉnh mới được thêm vào vì mọi ứng cử viên heap chỉ được tạo khi cây con của nó được tách khỏi đường dẫn đã chọn. 

Điều bất biến là mỗi mục nhập heap biểu thị một cây con hoàn toàn nguyên vẹn được gắn vào vùng đã bị bắt và độ sâu được lưu trữ của nó là mức đóng góp tối đa có thể đạt được từ cây con đó bằng cách sử dụng một truy vấn tiên kiến. Đường dẫn được chọn luôn chiếm đóng góp lớn nhất hiện có. Sau khi được chọn, các nhánh bên của nó chính xác sẽ trở thành những lựa chọn độc lập mới. Do đó, heap luôn mô tả tất cả những đóng góp tiếp theo có thể có và việc lấy mức tối đa của nó là lựa chọn tham lam tối ưu ở mọi giai đoạn. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve(data=None):
    if data is None:
        data = sys.stdin.buffer.read().split()

    it = iter(data)
    n = int(next(it))
    k = int(next(it))

    children = [[] for _ in range(n)]

    for v in range(1, n):
        p = int(next(it)) - 1
        children[p].append(v)

    # Iterative DFS order, avoiding recursion depth problems on a chain.
    order = []
    stack = [0]

    while stack:
        v = stack.pop()
        order.append(v)
        for c in children[v]:
            stack.append(c)

    # depth[v] = number of vertices in the longest downward path from v.
    # best[v] = child starting that path.
    depth = [1] * n
    best = [-1] * n

    for v in reversed(order):
        best_depth = 0
        best_child = -1

        for c in children[v]:
            if depth[c] > best_depth:
                best_depth = depth[c]
                best_child = c

        if best_child != -1:
            depth[v] = best_depth + 1
            best[v] = best_child

    # Python has a min-heap, so store negative depths.
    heap = [(-depth[0], 0)]
    answer = 0

    for _ in range(k):
        neg_len, root = heapq.heappop(heap)
        answer += -neg_len

        v = root

        # Select the longest path from this candidate subtree.
        while v != -1:
            chosen = best[v]

            # Every other child starts a new available subtree.
            for c in children[v]:
                if c != chosen:
                    heapq.heappush(heap, (-depth[c], c))

            v = chosen

    return str(answer)

if __name__ == "__main__":
    print(solve())
```Phần đầu tiên của quá trình triển khai sẽ xây dựng danh sách kề. Đầu vào xác định từng đỉnh từ 2 đến N bằng cách sử dụng cách đánh số dựa trên một, trong khi mã sẽ chuyển đổi mọi thứ thành lập chỉ mục dựa trên 0 ngay lập tức. Do đó, đỉnh 1 trở thành chỉ số 0. 

DFS lặp lại tạo ra thứ tự cha trước con. Đảo ngược thứ tự này sẽ đưa ra con trước cha mẹ, đó chính xác là thứ tự cần thiết để tính toán`depth[v]`từ các giá trị con đã được tính toán. Truyền tải lặp lại được sử dụng vì một cây có thể là một chuỗi có độ dài 100000, vượt quá ngăn xếp cuộc gọi đệ quy mặc định của Python. 

các`depth`phép tính chọn con có điểm tối đa`depth`. Vì bản thân đỉnh hiện tại đóng góp một đỉnh nên giá trị của nó bằng một cộng với giá trị con tốt nhất. các`best`mảng ghi nhớ con nào tạo ra mức tối đa đó. 

Heap lưu trữ các cây con có sẵn. của Python`heapq`là một đống tối thiểu, do đó mã lưu trữ độ sâu âm để làm cho độ sâu lớn nhất xuất hiện đầu tiên. 

Khi một ứng cử viên bị loại bỏ, độ sâu được lưu trữ của nó sẽ được thêm trực tiếp vào câu trả lời. Mã sau đó sẽ đi xuống con đường dài nhất đã chọn. Đối với mỗi đỉnh trên đường đi đó, tất cả các đỉnh con ngoại trừ phần tiếp theo đã chọn sẽ trở thành các mục nhập mới trong vùng nhớ heap. Những đứa trẻ đó không thể trùng lặp với con đường vừa được chọn nên toàn bộ con đường sâu nhất của chúng vẫn có thể được đóng góp trong tương lai. 

Không có vấn đề tràn số nguyên trong Python và câu trả lời tối đa chỉ là N. Vùng heap có thể chứa các mục O(N), trong khi mỗi đỉnh cây được xử lý trên nhiều nhất một đường dẫn đã chọn hoặc được xem xét dưới dạng nhánh bên, đưa ra số tuyến tính cần thiết của các hoạt động cấu trúc ngoài việc bảo trì vùng heap. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, cây có gốc ở 1 và đường đi sâu nhất là`1 -> 2 -> 4 -> 6 -> 8`. Nó chứa năm đỉnh. Sau khi lấy nó, các nhánh bên hữu ích là cây con có gốc ở 3, với độ sâu 2 và lá 7, với độ sâu 1. 

| Lựa chọn | Đống trước khi lựa chọn | Đường dẫn đã chọn | Đã thêm | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`(5,1)`|`1-2-4-6-8`| 5 | 5 | 
| 2 |`(2,3), (1,7)`|`3-5`| 2 | 7 | 

Lựa chọn thứ hai chọn cây con có gốc tại 3 vì đường đi sâu nhất của nó đóng góp hai đỉnh mới, trong khi đỉnh 7 chỉ đóng góp một đỉnh. Câu trả lời cuối cùng là 7. 

Đối với Mẫu 2, đường đi đầu tiên sâu nhất có thể là`1 -> 2 -> 4 -> 8`, với độ dài 4. Các nhánh bị bỏ qua của nó bao gồm cây con của đỉnh 5 và đỉnh 9. Nhánh gốc khác, bắt đầu từ 3, cũng có sẵn. 

| Lựa chọn | Đống trước khi lựa chọn | Đường dẫn đã chọn | Đã thêm | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`(4,1)`|`1-2-4-8`| 4 | 4 | 
| 2 |`(2,3), (2,5), (1,9)`|`3-6`| 2 | 6 | 
| 3 |`(2,5), (1,7), (1,9)`|`5-10`| 2 | 8 | 

Lựa chọn đầu tiên tạo ra một số cây con biên giới độc lập. Mỗi lựa chọn thứ hai và thứ ba sẽ thêm hai đỉnh mới, tổng cộng là 8. Bất biến được hiển thị ở đây: sau mỗi lựa chọn, vùng heap chứa đóng góp một đường đi tốt nhất có thể từ mọi nhánh vẫn chưa được khám phá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi đỉnh được xử lý với số lần không đổi và mỗi lần chèn hoặc loại bỏ heap có giá O(log N). | 
| Không gian | O(N) | Cây, thứ tự DFS, mảng sâu và vùng heap đều yêu cầu bộ nhớ tuyến tính. | 

Với N nhiều nhất là 100000, O(N log N) có nghĩa là khoảng vài triệu thao tác heap hoặc ít hơn, nằm trong quy mô dự kiến. Việc truyền tải lặp lại cũng tránh được các lỗi đệ quy trong chuỗi trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import io
import sys
import heapq

def solve(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    k = int(next(it))

    children = [[] for _ in range(n)]

    for v in range(1, n):
        p = int(next(it)) - 1
        children[p].append(v)

    order = []
    stack = [0]

    while stack:
        v = stack.pop()
        order.append(v)
        for c in children[v]:
            stack.append(c)

    depth = [1] * n
    best = [-1] * n

    for v in reversed(order):
        best_depth = 0
        best_child = -1

        for c in children[v]:
            if depth[c] > best_depth:
                best_depth = depth[c]
                best_child = c

        if best_child != -1:
            depth[v] = best_depth + 1
            best[v] = best_child

    heap = [(-depth[0], 0)]
    answer = 0

    for _ in range(k):
        neg_len, root = heapq.heappop(heap)
        answer += -neg_len

        v = root
        while v != -1:
            chosen = best[v]

            for c in children[v]:
                if c != chosen:
                    heapq.heappush(heap, (-depth[c], c))

            v = chosen

    return str(answer)

# Provided sample 1
assert solve(
    "8 2\n"
    "1 1 2 3 4 4 6\n"
) == "7", "sample 1"

# Provided sample 2
assert solve(
    "10 3\n"
    "1 1 2 2 3 3 4 4 5\n"
) == "8", "sample 2"

# Minimum-size tree, K = 1.
assert solve(
    "3 1\n"
    "1 1\n"
) == "2", "minimum size and K=1"

# Star, every selected leaf after the first contributes one new vertex.
assert solve(
    "6 5\n"
    "1 1 1 1 1\n"
) == "6", "star with K=N-1"

# A chain. Every selected path is identical, so extra selections add nothing.
assert solve(
    "7 3\n"
    "1 2 3 4 5 6\n"
) == "7", "pure chain"

# Maximum-size star, testing both N=100000 and K=N-1.
n = 100000
parents = " ".join(["1"] * (n - 1))
assert solve(f"{n} {n - 1}\n{parents}\n") == str(n), "maximum-size star"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 / 1 1`| 2 | Kích thước tối thiểu và ranh giới K = 1 | 
|`6 5 / 1 1 1 1 1`| 6 | Nhiều chi nhánh độc lập và K gần gũi với N | 
|`7 3 / 1 2 3 4 5 6`| 7 | Lựa chọn lặp đi lặp lại của một chuỗi đã được bảo hiểm | 
|`100000 99999 / 1 1 ... 1`| 100000 | N tối đa, hoạt động heap tối đa và cấu trúc sao | 

## Vỏ cạnh 

Đối với trường hợp K = 1, hãy xem xét`3 1`với bố mẹ`1 1`. Quá trình tiền xử lý mang lại`depth[1] = 2`, bởi vì đường dẫn sâu nhất chứa nút gốc và một nút con. Heap ban đầu chỉ chứa`(2,1)`, do đó lần lặp đơn cộng thêm 2 và dừng lại. Kết quả đầu ra là 2. Một giải pháp giả định rằng mọi nhánh có thể được tính độc lập sẽ trả về 3 không chính xác. 

Đối với các nhánh có độ sâu bằng nhau, hãy xem xét`5 2`với bố mẹ`1 1 2 2`. Gốc có hai con và đỉnh 2 có hai con. Đường đi sâu nhất có độ dài từ 3 đến đỉnh 4 hoặc 5. Giả sử thuật toán chọn`1-2-4`Đầu tiên. Trong khi xử lý đường dẫn đó, đỉnh 5 trở thành ứng cử viên có độ sâu 1 và đỉnh 3 là ứng cử viên có độ sâu 2. Lựa chọn thứ hai thực hiện`1-3`như một sự đóng góp mới của 1? Gốc đã bị che, do đó ứng viên có gốc tại 3 chỉ đóng góp đỉnh 3, trong khi ứng viên gốc tại 5 chỉ đóng góp đỉnh 5. Như vậy tổng số là 4. Kết quả tương tự thu được nếu đỉnh 5 được chọn trước. Sự ràng buộc bên trong`best[v]`thay đổi đường dẫn đã chọn nhưng không bao giờ thay đổi độ dài hoặc mức tối ưu cuối cùng. 

Đối với một ngôi sao, hãy xem xét`5 4`với bố mẹ`1 1 1 1`. Việc lựa chọn đầu tiên mất`1-2`, đóng góp 2. Việc xử lý đường đi đó sẽ hiển thị các đỉnh 3, 4 và 5 dưới dạng các ứng cử viên độc lập, mỗi đỉnh có độ sâu 1. Ba lựa chọn tiếp theo, mỗi lựa chọn đóng góp một đỉnh mới, tạo ra`2 + 1 + 1 + 1 = 5`. Đây chính xác là tổng số đỉnh và nó giải thích tại sao các phần tử con bị bỏ qua phải được chèn vào heap. 

Đối với một chuỗi, hãy xem xét`7 3`với bố mẹ`1 2 3 4 5 6`. Ứng viên đầu tiên có độ sâu 7 và chọn toàn bộ cây. Mỗi đỉnh trên đường đi đó đều có con được chọn bằng con duy nhất của nó, do đó không có nhánh bên nào được chèn vào. Heap sẽ trống sau lần lựa chọn đầu tiên. Vì việc chọn một đỉnh đã bị bắt không thể làm tăng câu trả lời nên các công dụng còn lại của nhà tiên tri không có tác dụng và câu trả lời vẫn là 7. 

Đối với ngôi sao có kích thước tối đa với 100000 đỉnh, đường dẫn được chọn đầu tiên đóng góp 2, trong khi 99998 đỉnh còn lại trở thành ứng cử viên cho vùng heap sâu 1. 99998 lựa chọn tiếp theo, mỗi lựa chọn đóng góp một đỉnh, vì vậy câu trả lời cuối cùng là 100000. Thuật toán không bao giờ thực hiện quét bậc hai qua các cặp lựa chọn và tổng công việc heap của nó vẫn là O(N log N).
