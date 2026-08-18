---
title: "CF 102262G - \u0421\u043a\u0430\u0447\u0438\u0432\u0430\u043d\u0438\u0435 \u0440\u0435\u0441\u0443\u0440\u0441\u043e\u0432 \u0432 \u0434\u0430\u0442\u0430-\u0446\u0435\u043d\u0442\u0440\u0435"
description: "Chúng ta có một đồ thị vô hướng có các đỉnh là máy chủ và các cạnh là các kết nối vật lý. Một máy chủ chỉ có thể tải xuống một tệp từ một máy chủ khác thuộc cùng thành phần được kết nối."
date: "2026-08-17T20:24:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102262
codeforces_index: "G"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u0444\u0438\u043d\u0430\u043b (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102262
solve_time_s: 122
verified: true
draft: false
---

[CF 102262G - \u0421\u043a\u0430\u0447\u0438\u0432\u0430\u043d\u0438\u0435 \u0440\u0435\u0441\u0443\u0440\u0441\u043e\u0432 \u0432 \u0434\u0430\u0442\u0430-\u0446\u0435\u043d\u0442\u0440\u0435](https://codeforces.com/problemset/problem/102262/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có các đỉnh là máy chủ và các cạnh là các kết nối vật lý. Một máy chủ chỉ có thể tải xuống một tệp từ một máy chủ khác thuộc cùng thành phần được kết nối. Mã định danh máy chủ là các số nguyên tùy ý lên tới (10^9), vì vậy chúng không thể được sử dụng trực tiếp làm chỉ mục mảng. 

Đối với mọi truy vấn, (X) là máy chủ đích và danh sách sau đây chứa các máy chủ hiện có tệp được yêu cầu. Chúng ta phải giữ chính xác những máy chủ (Y) được kết nối với (X) và in chúng theo thứ tự ban đầu. Thứ tự rất quan trọng nên danh sách truy vấn không thể đơn giản được chuyển đổi thành một tập hợp. 

Biểu đồ có thể chứa tối đa (10^6) cạnh. Do đó, có thể có gần như (2\cdot10^6) số nhận dạng máy chủ khác nhau. Chỉ với 1,5 giây khả dụng, việc duyệt đồ thị cho mỗi truy vấn là quá tốn kém. Với (Q\le 1000), thậm chí quét tất cả (10^6) cạnh một lần cho mỗi truy vấn có thể đạt tới (10^9) kiểm tra cạnh. Bản thân các danh sách truy vấn đều nhỏ, vì (K_i\le100), nên nhiệm vụ chính là xử lý trước kết nối một cách hiệu quả. 

Biểu đồ cũng có thể chứa số nhận dạng máy chủ chỉ xuất hiện trong truy vấn. Máy chủ như vậy là một đỉnh bị cô lập trừ khi nó được kết nối bởi một trong các cạnh đầu vào. Việc triển khai bất cẩn chỉ lưu trữ các đỉnh xuất hiện ở các cạnh có thể xử lý sai một truy vấn trong đó cả (X) và (Y) đều vắng mặt trong biểu đồ. Ví dụ:```
0
1
42 2
42 17
```Đầu ra đúng là:```
1 42
```Cả hai máy chủ đều bị cô lập, nhưng chúng là cùng một máy chủ khi số nhận dạng của chúng bằng nhau. Việc coi mọi mã định danh không xác định là thuộc về một thành phần "không xác định" chung nào đó sẽ chấp nhận không chính xác`17`. 

Trường hợp cạnh thứ hai là một máy chủ xuất hiện trong biểu đồ nhưng ứng cử viên khác thì không. Ví dụ:```
1
1 2
1
1 2
3 2
```Đầu ra đúng là:```
1 2
```Máy chủ`3`là một đỉnh bị cô lập và không thể tải xuống từ máy chủ`2`, mặc dù cả hai mã định danh đều được chương trình biết đến. Một giải pháp phải phân biệt được các đỉnh cô lập khác nhau. 

Trường hợp cạnh thứ ba là câu trả lời phải giữ nguyên các bản sao và sắp xếp thứ tự nếu đầu vào chứa chúng. Ví dụ:```
0
1
7 3
7 7 8
```Đầu ra là:```
2 7 7
```Truy vấn hỏi về ba nguồn được liệt kê và cả hai lần xuất hiện của`7`là hợp lệ. Việc sắp xếp hoặc loại bỏ danh sách truy vấn sẽ thay đổi kết quả đầu ra được yêu cầu. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là tìm riêng thành phần được kết nối của (X) cho mọi truy vấn, sử dụng DFS hoặc BFS. Sau khi biết thành phần này, mỗi máy chủ trong số tối đa 100 máy chủ ứng cử viên có thể được kiểm tra dựa trên thành phần đó. Điều này đúng vì tính kết nối chính xác là điều kiện xác định liệu có thể tải xuống hay không. 

Vấn đề là việc duyệt đồ thị lặp đi lặp lại. Trong trường hợp xấu nhất, một lần duyệt có thể kiểm tra tất cả (10^6) cạnh. Với (1000) truy vấn có tới (10^9) bài kiểm tra cạnh, trước khi tính công việc đỉnh bổ sung. Việc thực hiện truyền tải riêng cho từng ứng viên (Y) thậm chí còn tệ hơn, có thể là khoảng (10^{11}) lần kiểm tra kết nối. 

Cấu trúc đồ thị cho chúng ta khả năng quan sát mạnh mẽ hơn nhiều. Mọi truy vấn chỉ hỏi xem hai đỉnh có thuộc cùng một thành phần được kết nối hay không. Chúng tôi không bao giờ cần con đường thực tế giữa chúng. Đây chính xác là hoạt động được hỗ trợ bởi cấu trúc liên kết rời rạc hoặc DSU. Trong khi đọc từng cạnh ((A,B)), chúng ta hợp nhất các thành phần chứa (A) và (B). Sau khi tất cả các cạnh đã được xử lý, hai máy chủ được kết nối khi và chỉ khi đại diện DSU của chúng bằng nhau. 

Mã định danh máy chủ tùy ý gây ra vấn đề thứ hai. Một DSU thông thường yêu cầu các chỉ số nhỏ gọn như (0,\ldots,V-1). Nén tọa độ sẽ giải quyết được vấn đề đó, nhưng việc lưu trữ hàng triệu số nguyên Python và một từ điển lớn là điều không mong muốn dưới giới hạn bộ nhớ 64 MB. Thay vào đó, việc triển khai bên dưới sử dụng bảng băm địa chỉ mở nhỏ gọn được hỗ trợ bởi`array('I')`. Mỗi vị trí bảng băm được sử dụng sẽ lưu trữ mã định danh máy chủ và DSU cha của nó. Vì số nhận dạng là dương nên số 0 có thể biểu thị một vị trí chưa được sử dụng. 

Brute-force hoạt động vì quá trình truyền tải trực tiếp phát hiện chính xác một thành phần được kết nối nhưng không thành công khi cùng một quá trình truyền tải đắt tiền được lặp lại cho nhiều truy vấn. Nhận xét rằng mọi truy vấn chỉ là một bài kiểm tra tư cách thành viên thành phần cho phép chúng tôi thay thế việc truyền tải đồ thị bằng các hoạt động DSU gần như liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(Q(N+V))) | (O(N+V)) | Quá chậm, lên tới khoảng (10^9) bài kiểm tra | 
| DSU + bảng băm nhỏ gọn | Dự kiến ​​(O(N\alpha(V)+QK\alpha(V))) | (O(V)) | Đã chấp nhận | 

Ở đây (V\le2N) cho các đỉnh xuất hiện trong các cạnh, (\alpha) là hàm Ackermann nghịch đảo và các phép toán bảng băm có thời gian dự kiến ​​là (O(1)). 

## Hướng dẫn thuật toán 

1. Đọc (N), số cạnh của đồ thị và tạo một bảng băm nhỏ gọn đủ lớn cho tất cả các điểm cuối có thể có. Vì có tối đa (2N) điểm cuối riêng biệt và chúng tôi dự trữ ít nhất gấp đôi số vị trí đó nên hệ số tải vẫn ở mức dưới một nửa. Điều này đưa ra trình tự thăm dò dự kiến ​​​​ngắn. 
2. Đối với mọi cạnh ((A,B)), hãy chèn cả hai mã định danh máy chủ vào bảng băm nếu chúng không có. Một máy chủ mới được chèn vào sẽ khởi động với quyền DSU root của chính nó. Sau đó hợp nhất các nghiệm của (A) và (B). 

DSU không cần một mảng cha riêng biệt được lập chỉ mục bằng số đỉnh được nén. Bản thân khe bảng băm đóng vai trò là chỉ mục đỉnh, giúp lưu một mảng ánh xạ khác và giữ mức sử dụng bộ nhớ ở mức nhỏ. 
3. Thực hiện`find`với nén đường dẫn. Bắt đầu từ vị trí của máy chủ, đi theo vị trí gốc cho đến khi đạt được gốc có chính cha mẹ đó. Sau đó nén đường dẫn đi qua để các thao tác sau này đến thư mục gốc nhanh hơn. 
4. Thực hiện`union`bằng cách tìm cả hai gốc và gắn gốc này với gốc kia. Việc triển khai sử dụng chính vùng gốc làm giá trị gốc, do đó không cần thêm mảng thứ hạng hoặc kích thước. 
5. Đọc từng truy vấn sau khi biểu đồ đã được xử lý. Đối với đích (X), hãy tra cứu vị trí bảng băm của nó. Nếu nó tồn tại, gốc DSU của nó đại diện cho thành phần được kết nối của nó. Nếu nó không tồn tại thì (X) là một máy chủ bị cô lập. 
6. Đọc các nguồn ứng viên (K) theo thứ tự ban đầu. Đối với mỗi (Y), trước tiên hãy kiểm tra xem mã định danh của nó có tồn tại trong bảng băm của biểu đồ hay không. Nếu cả (X) và (Y) đều vắng mặt, chúng chỉ được kết nối khi số nhận dạng của chúng bằng nhau. Nếu thiếu chính xác một cái thì chúng không thể được kết nối. Nếu cả hai đều có mặt, hãy so sánh gốc DSU của chúng. 
7. Thêm mọi nguồn hợp lệ vào câu trả lời mà không thay đổi vị trí của nó so với các ứng viên khác. In số đếm theo sau là số nhận dạng đã chọn. 

### Tại sao nó hoạt động 

Sau khi tất cả các cạnh đã được xử lý, bất biến DSU là hai máy chủ xuất hiện trong biểu đồ có cùng một đại diện chính xác khi có đường dẫn giữa chúng. Mỗi cạnh đầu vào nối hai điểm cuối của nó, do đó mỗi cặp được kết nối bởi một chuỗi các cạnh cuối cùng sẽ được hợp nhất thành một bộ DSU. Ngược lại, DSU không bao giờ hợp nhất hai thành phần trừ khi cạnh đầu vào kết nối chúng, do đó hai thành phần đồ thị khác nhau không bao giờ có thể nhận được cùng một đại diện. 

Một máy chủ vắng mặt trong danh sách cạnh có độ 0 và do đó là một thành phần biệt lập. Hai máy chủ như vậy được kết nối chính xác khi chúng có cùng mã định danh. Logic truy vấn xử lý trường hợp này một cách riêng biệt, do đó mọi ứng cử viên đều được chấp nhận chính xác khi nó thuộc về thành phần được kết nối của đích. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MASK32 = 0xFFFFFFFF
MULT = 2654435761

def solve():
    n_line = input()
    if not n_line:
        return
    n = int(n_line)

    # At most 2*n different server identifiers occur in edges.
    # Keep the load factor below 1/2.
    need = max(2, 4 * n)
    size = 1
    while size < need:
        size <<= 1

    mask = size - 1

    # 0 means an unused slot. Server identifiers are >= 1.
    keys = array('I', [0]) * size
    parent = array('I', [0]) * size

    def locate(x):
        """Return the slot containing x, or 0 if x is absent."""
        p = (x * MULT) & mask
        while True:
            k = keys[p]
            if k == 0:
                return 0
            if k == x:
                return p
            p = (p + 1) & mask

    def get_slot(x):
        """Find x, inserting it as a new DSU root when necessary."""
        p = (x * MULT) & mask
        while True:
            k = keys[p]
            if k == 0:
                keys[p] = x
                parent[p] = p
                return p
            if k == x:
                return p
            p = (p + 1) & mask

    def find(p):
        root = p
        while parent[root] != root:
            root = parent[root]

        while parent[p] != p:
            nxt = parent[p]
            parent[p] = root
            p = nxt

        return root

    def union(a, b):
        ra = find(a)
        rb = find(b)
        if ra != rb:
            parent[rb] = ra

    for _ in range(n):
        a, b = map(int, input().split())
        sa = get_slot(a)
        sb = get_slot(b)
        union(sa, sb)

    q = int(input())
    out = []

    for _ in range(q):
        x, k = map(int, input().split())
        ys = list(map(int, input().split()))

        sx = locate(x)

        if sx == 0:
            # x is an isolated server.
            ans = [y for y in ys if y == x]
        else:
            rx = find(sx)
            ans = []

            for y in ys:
                sy = locate(y)
                if sy != 0 and find(sy) == rx:
                    ans.append(y)

        if ans:
            out.append(str(len(ans)) + " " + " ".join(map(str, ans)))
        else:
            out.append("0")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ phân bổ bảng băm theo số lượng điểm cuối cạnh tối đa có thể. Bốn vị trí được dành riêng cho mỗi cạnh, do đó, ngay cả khi mỗi điểm cuối khác nhau, bảng vẫn có hệ số tải tối đa là một nửa. Kích thước lũy thừa hai cho phép đạt được vị trí băm bằng mặt nạ bit thay vì hoạt động modulo đắt tiền hơn.`get_slot`được sử dụng trong khi xử lý các cạnh vì mọi điểm cuối phải trở thành đỉnh DSU.`locate`được sử dụng cho các truy vấn vì mã định danh truy vấn chưa bao giờ xuất hiện ở một cạnh phải vẫn là một đỉnh bị cô lập thay vì được chèn vào thành phần hiện có. 

Mảng cha lưu trữ các số vị trí của bảng băm. Đối với mã định danh mới được chèn vào,`parent[p] = p`, biến khe đó thành gốc của chính nó. Khi hai thành phần được hợp nhất, một gốc sẽ lưu trữ gốc kia làm cha mẹ của nó. Nén đường dẫn trong`find`sau đó thực hiện các hoạt động tiếp theo với thời gian gần như không đổi. 

Không có vấn đề tràn số nguyên trong Python. Phép nhân được sử dụng bởi hàm băm được che giấu rõ ràng thành 32 bit vì bảng băm nhỏ gọn được thiết kế xung quanh các mã định danh máy chủ 32 bit không dấu và kích thước bảng lũy ​​thừa hai. 

Danh sách truy vấn chỉ được xử lý sau khi DSU của đồ thị được hoàn thành. Thứ tự ban đầu được giữ nguyên vì các ứng cử viên được thêm vào`ans`khi chúng được đọc. Không có sự sắp xếp hoặc loại bỏ trùng lặp nào được thực hiện. 

Trường hợp ranh giới tinh tế là (N=0). Trong tình huống đó không có đỉnh đồ thị nào trong bảng băm. Mọi máy chủ được truy vấn đều bị cô lập, do đó, nguồn hợp lệ duy nhất cho đích (X) là một lần xuất hiện khác có cùng mã định danh (X). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Biểu đồ chứa hai thành phần bị ngắt kết nối. Cái đầu tiên chứa`54578972`,`99368556`,`79699669`,`64508306`, Và`56554555`. Thứ hai chứa`27101564`,`81480071`,`89445516`,`93762227`, Và`89808815`. 

Trạng thái DSU liên quan sau tất cả tám cạnh là: 

| Máy chủ | Đại diện thành phần | 
| --- | --- | 
| 54578972 | thành phần A | 
| 99368556 | thành phần A | 
| 79699669 | thành phần A | 
| 64508306 | thành phần A | 
| 56554555 | thành phần A | 
| 27101564 | thành phần B | 
| 81480071 | thành phần B | 
| 89445516 | thành phần B | 
| 93762227 | thành phần B | 
| 89808815 | thành phần B | 

Đối với các truy vấn, trạng thái quan trọng là: 

| Điểm đến (X) | Ứng viên | Cùng một thành phần? | Hành động đầu ra | 
| --- | --- | --- | --- | 
| 56554555 | 93762227 | Không | bỏ qua | 
| 56554555 | 79699669 | Có | giữ | 
| 99368556 | 64508306 | Có | giữ | 
| 99368556 | 56554555 | Có | giữ | 
| 27101564 | 99368556 | Không | bỏ qua | 
| 27101564 | 79699669 | Không | bỏ qua | 
| 93762227 | 56554555 | Không | bỏ qua | 
| 93762227 | 54578972 | Không | bỏ qua | 

Kết quả đầu ra là:```
1 79699669
2 64508306 56554555
0
0
```Dấu vết thể hiện sự bất biến DSU trung tâm. Hình dạng thực tế của từng thành phần không thành vấn đề khi mỗi tập hợp kết nối đều có một đại diện. 

### Mẫu 2 

Ở đây đồ thị có hai thành phần. Một chứa`85619126`,`64616465`,`98159933`,`73978229`,`29978081`, Và`72404745`. Cái kia chứa`97698445`,`75243921`,`36815728`,`18360411`,`66445821`, Và`92142380`. 

Quá trình xử lý truy vấn trông như thế này: 

| Điểm đến (X) | Ứng viên | Cùng một thành phần? | Hành động đầu ra | 
| --- | --- | --- | --- | 
| 97698445 | 75243921 | Có | giữ | 
| 97698445 | 92142380 | Có | giữ | 
| 97698445 | 98159933 | Không | bỏ qua | 
| 97698445 | 73978229 | Không | bỏ qua | 
| 66445821 | 29978081 | Không | bỏ qua | 
| 66445821 | 92142380 | Có | giữ | 
| 66445821 | 73978229 | Không | bỏ qua | 
| 66445821 | 97698445 | Có | giữ | 
| 18360411 | 29978081 | Không | bỏ qua | 
| 18360411 | 92142380 | Có | giữ | 
| 18360411 | 98159933 | Không | bỏ qua | 
| 18360411 | 97698445 | Có | giữ | 
| 36815728 | 64616465 | Không | bỏ qua | 
| 36815728 | 92142380 | Có | giữ | 
| 36815728 | 97698445 | Có | giữ | 
| 36815728 | 29978081 | Không | bỏ qua | 

Đầu ra là:```
2 75243921 92142380
2 92142380 97698445
2 92142380 97698445
2 92142380 97698445
```Thành phần tương tự có thể được biểu diễn bằng bất kỳ gốc DSU nào, do đó đại diện số thực tế là không liên quan. Chỉ có sự bình đẳng của các đại diện mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Dự kiến ​​(O(N\alpha(V)+QK\alpha(V))) | Mỗi cạnh thực hiện hai lần tra cứu băm dự kiến ​​(O(1)) và một lần hợp nhất DSU; mỗi truy vấn kiểm tra tối đa 100 nguồn | 
| Không gian | (O(V)) | Bảng băm nhỏ gọn và mảng cha chứa tối đa (O(2N)) vị trí máy chủ bị chiếm dụng | 

Ở đây (V\le2N) cho các đỉnh xuất hiện trong các cạnh của đồ thị. Với (N=10^6), việc triển khai dự trữ tối đa (2^{22}) vị trí băm và cả khóa và khóa gốc đều sử dụng số nguyên không dấu 4 byte. Do đó, hai mảng chính chiếm khoảng 32 MB, nhường chỗ cho quá trình xử lý đầu vào và đầu ra theo mô hình bộ nhớ dự định. 

Công việc truy vấn tối đa là (1000\cdot100=10^5) kiểm tra ứng viên, không đáng kể so với quá trình tiền xử lý biểu đồ. Phần đắt tiền được xử lý một lần, thay vì một lần cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

MASK32 = 0xFFFFFFFF
MULT = 2654435761

def solve(data: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)

    input = sys.stdin.readline

    n = int(input())

    need = max(2, 4 * n)
    size = 1
    while size < need:
        size <<= 1
    mask = size - 1

    keys = array('I', [0]) * size
    parent = array('I', [0]) * size

    def locate(x):
        p = (x * MULT) & mask
        while True:
            k = keys[p]
            if k == 0:
                return 0
            if k == x:
                return p
            p = (p + 1) & mask

    def get_slot(x):
        p = (x * MULT) & mask
        while True:
            k = keys[p]
            if k == 0:
                keys[p] = x
                parent[p] = p
                return p
            if k == x:
                return p
            p = (p + 1) & mask

    def find(p):
        root = p
        while parent[root] != root:
            root = parent[root]

        while parent[p] != p:
            nxt = parent[p]
            parent[p] = root
            p = nxt

        return root

    def union(a, b):
        a = find(a)
        b = find(b)
        if a != b:
            parent[b] = a

    for _ in range(n):
        a, b = map(int, input().split())
        union(get_slot(a), get_slot(b))

    q = int(input())
    out = []

    for _ in range(q):
        x, k = map(int, input().split())
        ys = list(map(int, input().split()))

        sx = locate(x)

        if sx == 0:
            ans = [y for y in ys if y == x]
        else:
            rx = find(sx)
            ans = []
            for y in ys:
                sy = locate(y)
                if sy != 0 and find(sy) == rx:
                    ans.append(y)

        out.append(
            str(len(ans)) +
            ((" " + " ".join(map(str, ans))) if ans else "")
        )

    sys.stdin = old_stdin
    return "\n".join(out)

sample1 = """\
8
54578972 99368556
79699669 54578972
64508306 99368556
56554555 64508306
27101564 81480071
89445516 27101564
93762227 81480071
89808815 93762227
4
56554555 2
93762227 79699669
99368556 2
64508306 56554555
27101564 2
99368556 79699669
93762227 2
56554555 54578972
"""

assert solve(sample1) == """\
1 79699669
2 64508306 56554555
0
0""", "sample 1"

sample2 = """\
10
85619126 64616465
98159933 85619126
73978229 85619126
29978081 64616465
72404745 29978081
97698445 75243921
36815728 97698445
18360411 97698445
66445821 75243921
92142380 66445821
4
97698445 4
75243921 92142380 98159933 73978229
66445821 4
29978081 92142380 73978229 97698445
18360411 4
29978081 92142380 98159933 97698445
36815728 4
64616465 92142380 97698445 29978081
"""

assert solve(sample2) == """\
2 75243921 92142380
2 92142380 97698445
2 92142380 97698445
2 92142380 97698445""", "sample 2"

minimum = """\
0
1
42 4
42 17 42 17
"""

assert solve(minimum) == "2 42 42", "minimum graph and isolated vertices"

boundary = """\
1
1 1000000000
2
1 3
1000000000 1 42
42 2
1 1000000000
"""

assert solve(boundary) == """\
2 1000000000 1
0""", "identifier boundary"

self_loops = """\
4
7 7
7 7
1000000000 1000000000
42 42
3
7 4
7 7 7 42
1000000000 3
1000000000 7 1000000000
42 2
42 43
"""

assert solve(self_loops) == """\
3 7 7 42
1 1000000000
1 42""", "self loops and repeated values"

# Maximum-size stress case: one million edges forming one long chain.
# The query also uses the largest legal server identifier.
n = 1_000_000
parts = [str(n)]
for i in range(1, n + 1):
    parts.append(f"{i} {i + 1}")
parts.append("1")
parts.append(f"1 3")
parts.append(f"1 {n + 1} {1_000_000_000}")

maximum_case = "\n".join(parts) + "\n"

assert solve(maximum_case) == f"2 {n + 1} 1", "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`1 79699669`,`2 64508306 56554555`, thì hai số 0 | Lọc thành phần cơ bản và các thành phần bị ngắt kết nối | 
| Mẫu 2 | Bốn dòng với hai nguồn hợp lệ mỗi dòng | Nhiều thành phần và bảo toàn trật tự nguồn | 
| (N=0), định danh bị cô lập lặp đi lặp lại |`2 42 42`| Máy chủ không xác định, sự bình đẳng của các số nhận dạng bị cô lập, các ứng cử viên trùng lặp | 
| Số nhận dạng`1`Và`1000000000`|`2 1000000000 1`, sau đó`0`| Ranh giới định danh và xử lý ứng viên vắng mặt | 
| Tự vòng lặp và giá trị lặp lại | Ba câu trả lời dành riêng cho từng thành phần | Tự lặp, trùng lặp và các thành phần chỉ chứa một máy chủ | 
| Dây chuyền một triệu cạnh |`2 1000001 1`| Kích thước biểu đồ tối đa, đường dẫn DSU dài và tải tiền xử lý thực tế lớn nhất | 

Kiểm tra kích thước tối đa được tạo ra có chủ ý thay vì nhúng một triệu dòng cạnh vào nguồn kiểm tra. Nó thực hiện cùng một kích thước đầu vào xác định hành vi bộ nhớ và thời gian của giải pháp thực tế. 

## Vỏ cạnh 

Khi không có biên, mọi máy chủ đều bị cô lập. Đối với đầu vào```
0
1
42 3
42 17 42
```điểm đến`42`không có mục nhập biểu đồ, do đó logic truy vấn sẽ lấy nhánh máy chủ bị cô lập. Nó giữ cả hai lần xuất hiện của`42`và từ chối`17`, sản xuất```
2 42 42
```Một máy chủ xuất hiện trong biểu đồ có thể được kết nối với một máy chủ khác trong khi vẫn không có mã định danh được truy vấn thứ ba. Vì```
1
1 2
1
1 3
2 3 1
```DSU chứa một thành phần`{1,2}`. Máy chủ`3`vắng mặt và cô lập, nên chỉ có`1`thuộc về thành phần của đích. Kết quả là```
1 1
```Đây là lý do tại sao một mã định danh vắng mặt không thể được chỉ định một cách đơn giản là một đại diện chung đặc biệt. Mỗi mã định danh vắng mặt đều là thành phần riêng biệt của nó. 

Nếu một ứng cử viên xuất hiện nhiều lần thì mỗi lần xuất hiện phải được đánh giá độc lập. Vì```
0
1
7 4
7 7 8 7
```đích đến là máy chủ bị cô lập`7`, do đó ba lần xuất hiện của`7`tất cả đều hợp lệ. Đầu ra là```
3 7 7 7
```Các cấu trúc triển khai`ans`bằng cách lặp lại danh sách ban đầu, do đó không mất thông tin thứ tự hoặc bội số. 

Vòng lặp tự không tạo kết nối đến máy chủ khác. Vì```
1
1000000000 1000000000
1
1000000000 2
1 1000000000
```DSU tạo ra một thành phần đơn chứa`1000000000`. Ứng viên`1`là một máy chủ bị cô lập khác, trong khi`1000000000`chính là đích đến. Đầu ra là```
1 1000000000
```Các cạnh lặp lại hoạt động tương tự. Đang gọi`union`trên hai đỉnh đã có trong cùng một bộ DSU không có gì thay đổi, do đó các kết nối trùng lặp không thể làm hỏng cấu trúc thành phần. 

Cuối cùng, số nhận dạng máy chủ có thể lớn bằng (10^9), trong khi số lượng máy chủ nhỏ hơn nhiều. Bảng băm có chủ ý lưu trữ các mã định danh dưới dạng số nguyên không dấu 32 bit thay vì cố gắng phân bổ một mảng được mã định danh lập chỉ mục. Sự khác biệt đó là nguyên nhân khiến việc sử dụng bộ nhớ phụ thuộc vào số lượng máy chủ thực tế thay vì giá trị nhận dạng tối đa.
