---
title: "CF 102396C - Tàu phản lực"
description: "Hãy coi các thành phố như các đỉnh của một đồ thị vô hướng có các cạnh là các tuyến đường xe lửa hiện có. Vì các tuyến đường là hai chiều nên hai thành phố có thể đến được nhau một cách chính xác khi chúng thuộc cùng một thành phần được kết nối của biểu đồ này."
date: "2026-08-14T14:22:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "C"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 291
verified: false
draft: false
---

[CF 102396C - Tàu phản lực](https://codeforces.com/problemset/problem/102396/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi các thành phố như các đỉnh của một đồ thị vô hướng có các cạnh là các tuyến đường xe lửa hiện có. Vì các tuyến đường là hai chiều nên hai thành phố có thể đến được nhau một cách chính xác khi chúng thuộc cùng một thành phần được kết nối của biểu đồ này. 

Có một đồ thị vô hướng thứ hai trên cùng một đỉnh, biểu thị tình bạn. Đối với một truy vấn`? v`, chúng ta cần số đỉnh`u`như vậy`u`là bạn của`v`Và`u`nằm trong cùng thành phần được kết nối của`v`trong biểu đồ xe lửa. Câu trả lời không phải là tổng số bạn bè, vì hiện tại một số người bạn có thể không liên lạc được. 

Có thể có hai loại cập nhật. MỘT`T a b`thao tác chèn một cạnh đoàn tàu, do đó hai thành phần đoàn tàu riêng biệt trước đó có thể hợp nhất. MỘT`F a b`hoạt động chèn một khía cạnh tình bạn. Không có loại cạnh nào bị loại bỏ, đây là thuộc tính cấu trúc tạo nên giải pháp kết nối gia tăng khả thi. 

Dữ liệu đầu vào ban đầu chứa tối đa (10^5) thành phố, (10^5) tình bạn, (10^5) tuyến đường xe lửa và (10^5) hoạt động tiếp theo. Một giải pháp quét tất cả các thành phố hoặc tất cả tình bạn cho mỗi truy vấn sẽ mất tới (10^{10}) thao tác, vượt xa giới hạn hai giây cho phép. Chúng ta cần tổng công gần như tuyến tính hoặc gần tuyến tính, với hệ số logarit có thể chấp nhận được. 

Có một số trường hợp đặc biệt có thể đánh lừa việc triển khai trực tiếp. Đầu tiên, tình bạn có thể tồn tại trước khi điểm cuối của nó được kết nối với nhau. Ví dụ:```
2 1 0
1 2
0
```Không có đường tàu nên không có đầu ra. Thay vào đó, nếu chúng ta thêm một truy vấn:```
2 1 0
1 2
1
? 1
```đầu ra là`0`, mặc dù thành phố 1 và 2 là bạn bè. Giải pháp tính tình bạn mà không kiểm tra kết nối tàu sẽ in sai`1`. 

Thứ hai, một tình bạn mới được thêm vào có thể kết nối hai thành phố đã có thể tiếp cận được:```
2 0 1
1 2
1
F 1 2
```Không có câu hỏi nào, nhưng sau khi tình bạn được đưa vào, sự đóng góp của cả hai điểm cuối sẽ ngay lập tức trở thành một. Nếu việc triển khai chỉ xử lý tình bạn khi một cạnh tàu được thêm vào thì nó sẽ mất bản cập nhật này. 

Thứ ba, khi một cạnh tàu nối hai thành phần, một số tình bạn hiện có có thể trở nên hợp lệ cùng một lúc. Ví dụ:```
4 2 1
1 3
2 4
1 2
1
? 1
```Biểu đồ tàu kết nối 1 với 2, trong khi 3 và 4 nằm ngoài thành phần đó. Thành phố 1 không có người bạn nào có thể liên lạc được nên câu trả lời là`0`. Thay vào đó, nếu cạnh tàu nối các thành phần chứa 1 và 3 thì tình bạn`(1,3)`trở nên hợp lệ ngay lập tức. Việc triển khai bất cẩn chỉ kiểm tra hai điểm cuối của cạnh tàu mới có thể bỏ lỡ tình bạn giữa các đỉnh tùy ý của hai thành phần được hợp nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Duy trì biểu đồ tàu và cho mọi truy vấn`? v`, chạy DFS hoặc BFS từ`v`để xác định thành phần được kết nối hiện tại của nó. Sau đó kiểm tra mọi tình bạn của`v`và đếm những người có điểm cuối đã được truy cập. Điều này đúng vì DFS xác định chính xác các thành phố có thể truy cập từ`v`. 

Vấn đề là chi phí. Một tìm kiếm kết nối duy nhất có thể chạm vào (O(n+k+q)) đỉnh và cạnh trong trường hợp xấu nhất và có thể có (10^5) truy vấn. Trong một chuỗi các phép toán đủ dày đặc, điều này đưa ra thứ tự các phép toán đồ thị (10^{10}), quá chậm. 

Điểm khởi đầu tốt hơn là khai thác thực tế là các cạnh của đoàn tàu chỉ được chèn vào. Sau đó, các thành phần được kết nối có thể được duy trì bằng cấu trúc liên kết tập hợp rời rạc hoặc DSU. Một truy vấn`? v`có thể xác định ngay thành phần tàu hiện tại của`v`. 

Vấn đề còn lại là duy trì có bao nhiêu bạn bè của mỗi đỉnh nằm trong thành phần của nó. Cho phép`ans[v]`chính xác là con số đó. Khi một tình bạn`(a,b)`được chèn vào, chỉ có hai khả năng. Nếu như`a`Và`b`đã thuộc về cùng một thành phần đoàn tàu thì cả hai câu trả lời đều tăng thêm một. Nếu không thì tình bạn vẫn chưa hữu ích, nhưng nó phải được ghi nhớ vì việc hợp nhất tàu trong tương lai có thể khiến nó trở nên hữu ích. 

Giả sử một cạnh tàu hợp nhất hai thành phần khác nhau. Mọi tình bạn vượt qua hai thành phần đó đều trở nên hợp lệ sau khi hợp nhất. Chúng ta có thể kiểm tra mọi tình bạn giữa hai thành phần, nhưng làm điều đó một cách ngây thơ có thể quét liên tục các thành phần lớn. 

Quan sát chính là luôn xử lý thành phần nhỏ hơn. Lưu trữ danh sách các đỉnh thuộc mọi thành phần DSU. Khi hai thành phần hợp nhất, lặp qua mọi đỉnh của thành phần nhỏ hơn và kiểm tra tất cả các cạnh hữu nghị của nó. Một tình bạn có điểm cuối khác nằm trong thành phần lớn hơn sẽ vượt qua giới hạn và trở nên hợp lệ, vì vậy hãy tăng câu trả lời của cả hai điểm cuối. 

Tại sao điều này đủ nhanh? Bất cứ khi nào một đỉnh được xử lý trong quá trình hợp nhất thành phần, nó sẽ thuộc về thành phần nhỏ hơn. Sau khi hợp nhất, thành phần của nó có kích thước ít nhất là gấp đôi. Do đó, bất kỳ đỉnh cụ thể nào cũng có thể thuộc về cạnh nhỏ hơn nhiều nhất là (O(\log n)) lần. Mỗi khi đỉnh đó được xử lý, chúng tôi sẽ quét danh sách lân cận tình bạn của nó, do đó mỗi đỉnh kề tình bạn chỉ được quét tổng cộng (O(\log n)) lần. 

Điều này đưa ra ý tưởng tương tự từ nhỏ đến lớn được sử dụng trong phân tích chính thức của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(q(n+m+k))) | (O(n+m+k)) | Quá chậm | 
| Tối ưu | (O((m+q+k)\log n\cdot\alpha(n))) | (O(n+m+k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo DSU chứa tất cả (n) thành phố. Đối với mọi thành phần, hãy duy trì danh sách các đỉnh hiện tại của nó. Ban đầu mỗi thành phần chứa một thành phố. 
2. Lưu trữ mọi tình bạn vào danh sách kề`friends[v]`. Mỗi tình bạn`(a,b)`được lưu trữ hai lần, một lần trong`friends[a]`và một lần trong`friends[b]`, vì sau này chúng ta cần kiểm tra tất cả bạn bè của một đỉnh. 
3. Thêm các tuyến tàu ban đầu vào DSU. Vì các cạnh tàu chỉ bổ sung thêm kết nối nên DSU có thể biểu thị chính xác các thành phần được kết nối sau khi tất cả các tuyến đường ban đầu đã được chèn vào. 
4. Khởi tạo`ans[v]`bằng cách xử lý mọi tình bạn ban đầu`(a,b)`. Nếu như`a`Và`b`có cùng gốc DSU, tăng cả hai`ans[a]`Và`ans[b]`. Tại thời điểm này`ans[v]`chính xác là số lượng bạn bè hiện có thể liên lạc của`v`. 
5. Đối với một`F a b`hoạt động, nối thêm`b`ĐẾN`friends[a]`Và`a`ĐẾN`friends[b]`. Nếu hai điểm cuối hiện có cùng gốc DSU, hãy tăng`ans[a]`Và`ans[b]`. Nếu chúng thuộc các thành phần khác nhau, đừng thay đổi câu trả lời, vì tình bạn chỉ trở nên hữu ích sau khi các thành phần của chúng hợp nhất. 
6. Đối với một`T a b`hoạt động, tìm ra gốc rễ của`a`Và`b`. Nếu chúng đã bằng nhau, tuyến đường mới sẽ không thay đổi kết nối và không có câu trả lời, vậy là thao tác đã kết thúc. 
7. Nếu các gốc khác nhau, hãy so sánh kích thước các thành phần của chúng và chỉ định phần nhỏ hơn là`small`và cái lớn hơn như`large`. Danh sách thành viên của`small`là thành phần duy nhất chúng ta cần liệt kê. 
8. Trước khi thay đổi cha mẹ DSU, hãy lặp qua từng đỉnh`v`TRONG`small`và mọi người bạn`u`của`v`. Nếu như`u`hiện thuộc về`large`, sau đó`(v,u)`là một tình bạn vượt qua hai thành phần. Sau khi tuyến đường tàu mới được đưa vào, hai thành phố sẽ có thể tiếp cận được lẫn nhau, vì vậy hãy tăng cả hai`ans[v]`Và`ans[u]`. 
9. Hợp nhất`small`vào trong`large`trong DSU và nối các đỉnh của`small`vào danh sách thành viên của`large`. Kích thước thành phần cũng trở thành tổng của hai kích thước cũ. 
10. Đối với một`? v`vận hành, in`ans[v]`. Không cần phải duyệt qua biểu đồ vì tất cả các thay đổi có thể ảnh hưởng đến giá trị này đã được xử lý khi xảy ra tình bạn hoặc hợp nhất đoàn tàu tương ứng. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi thao tác được xử lý,`ans[v]`bằng số cạnh tình bạn liên quan tới`v`có điểm cuối khác nằm trong cùng thành phần tàu hiện tại với`v`. 

Một tình bạn mới được chèn vào sẽ được tính ngay lập tức một cách chính xác khi điểm cuối của nó đã được kết nối. Nếu chúng không được kết nối, tình bạn sẽ được lưu trữ trong cả hai danh sách lân cận và vẫn có sẵn để hợp nhất thành phần sau này. 

Khi hai thành phần đoàn tàu hợp nhất, tình bạn duy nhất có trạng thái có thể thay đổi là tình bạn có một điểm cuối trong mỗi thành phần. Chúng tôi kiểm tra mọi đỉnh của thành phần nhỏ hơn và tất cả các tình bạn của nó, vì vậy mọi tình bạn giao nhau đều được tìm thấy. Tình bạn nội bộ đã được tính, trong khi tình bạn ở bên ngoài cả hai thành phần vẫn không hợp lệ. Do đó, mọi tình bạn mới hợp lệ đều đóng góp chính xác một lần cho cả hai câu trả lời cuối cùng. 

Sau khi hợp nhất, DSU thể hiện khả năng kết nối tàu mới, do đó, bất biến vẫn được giữ nguyên. Vì tất cả các thao tác chỉ thêm các cạnh nên không có tình bạn hợp lệ nào trước đây có thể trở thành không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    friends = [[] for _ in range(n)]
    friendship_edges = []

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        friends[a].append(b)
        friends[b].append(a)
        friendship_edges.append((a, b))

    parent = list(range(n))
    size = [1] * n
    members = [[i] for i in range(n)]

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def merge(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return a

        if size[a] < size[b]:
            a, b = b, a

        parent[b] = a
        size[a] += size[b]
        members[a].extend(members[b])
        members[b] = []
        return a

    for _ in range(k):
        a, b = map(int, input().split())
        merge(a - 1, b - 1)

    ans = [0] * n

    for a, b in friendship_edges:
        if find(a) == find(b):
            ans[a] += 1
            ans[b] += 1

    q = int(input())
    out = []

    for _ in range(q):
        parts = input().split()
        typ = parts[0]

        if typ == '?':
            v = int(parts[1]) - 1
            out.append(str(ans[v]))

        elif typ == 'F':
            a = int(parts[1]) - 1
            b = int(parts[2]) - 1

            friends[a].append(b)
            friends[b].append(a)

            if find(a) == find(b):
                ans[a] += 1
                ans[b] += 1

        else:
            a = int(parts[1]) - 1
            b = int(parts[2]) - 1

            ra = find(a)
            rb = find(b)

            if ra == rb:
                continue

            if size[ra] > size[rb]:
                large, small = ra, rb
            else:
                large, small = rb, ra

            for v in members[small]:
                for u in friends[v]:
                    if find(u) == large:
                        ans[v] += 1
                        ans[u] += 1

            parent[small] = large
            size[large] += size[small]
            members[large].extend(members[small])
            members[small] = []

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```DSU được khởi tạo trước khi đánh giá tình bạn ban đầu vì các tuyến đường tàu ban đầu xác định các thành phần được kết nối ban đầu. Các cạnh tình bạn ban đầu được lưu riêng để chúng có thể được đánh giá sau khi tất cả các tuyến đường tàu ban đầu đã được chèn vào. 

các`members`mảng là cấu trúc bổ sung giúp có thể hợp nhất từ ​​nhỏ đến lớn. Chỉ riêng DSU có thể cho chúng ta biết liệu hai đỉnh có được kết nối với nhau hay không, nhưng nó không thể liệt kê tất cả các đỉnh thuộc về một thành phần một cách hiệu quả. Danh sách thành viên cung cấp chính xác thao tác còn thiếu đó. 

Thứ tự bên trong việc hợp nhất đoàn tàu rất tinh tế. Thành phần nhỏ hơn được quét trước khi thành phần mẹ của nó được thay đổi. Trong quá trình quét này,`find(u) == large`có nghĩa là`u`thực sự thuộc về thành phần khác trước khi hợp nhất. Nếu chúng ta thay đổi đỉnh cha trước tiên, thì tình bạn giữa hai đỉnh đều nằm trong`small`có thể bị nhầm lẫn với việc vượt qua tình bạn và được tính hai lần. 

các`F`hoạt động cập nhật danh sách lân cận tình bạn bất kể kết nối. Tình bạn phải được lưu giữ khi điểm cuối của nó bị ngắt kết nối, bởi vì sau này`T`hoạt động có thể làm cho nó hợp lệ. 

Tất cả số đếm đều vừa vặn thoải mái với số nguyên Python. Trong C++, ở đây một số nguyên có dấu 32 bit thông thường cũng đủ vì câu trả lời tối đa là (n-1), nhưng Python không có vấn đề về tràn số nguyên. 

Sự lặp đi lặp lại`find`tránh chi phí đệ quy và thực hiện nén đường dẫn. Liên kết theo kích thước thành phần đảm bảo rằng danh sách thành viên luôn được hợp nhất từ ​​danh sách nhỏ hơn sang danh sách lớn hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các cạnh tàu ban đầu là`(1,2)`Và`(1,4)`, vậy các thành phần đoàn tàu là`{1,2,4}`Và`{3}`. Tình bạn ban đầu là`(1,2)`Và`(1,3)`. Chỉ một`(1,2)`nằm trong thành phần của thành phố 1, vì vậy`ans[1]`bắt đầu từ một. 

| Hoạt động | Linh kiện | Hiệu ứng tình bạn mới |`ans[1]`| 
| --- | --- | --- | --- | 
| Trạng thái ban đầu |`{1,2,4}`,`{3}`|`(1,2)`tính,`(1,3)`không được tính | 1 | 
|`? 1`|`{1,2,4}`,`{3}`| không | 1 | 
|`F 4 1`|`{1,2,4}`,`{3}`| 1 và 4 đã được kết nối | 2 | 
|`? 1`|`{1,2,4}`,`{3}`| không | 2 | 
|`T 4 3`|`{1,2,3,4}`| tình bạn`(1,3)`vượt qua các thành phần hợp nhất | 3 | 
|`? 1`|`{1,2,3,4}`| không | 3 | 

Lần chèn đoàn tàu cuối cùng sẽ quét thành phần nhỏ hơn`{3}`. Tình bạn duy nhất của nó là với thành phố 1, thành phố thuộc thành phần kia, vì vậy`(3,1)`trở nên có thể truy cập được và`ans[1]`tăng từ hai lên ba. 

### Xây dựng ví dụ 2 

Hãy xem xét đầu vào sau:```
5 2 1
1 4
2 5
1 2
5
? 1
F 1 3
? 1
T 3 4
? 1
```Ban đầu các bộ phận của đoàn tàu được`{1,2}`Và`{3}`,`{4}`,`{5}`. Tình bạn`(1,4)`không thể truy cập được và`(2,5)`cũng không thể truy cập được từ thành phố 1. 

| Hoạt động | Linh kiện | Thay đổi có liên quan |`ans[1]`| 
| --- | --- | --- | --- | 
| Trạng thái ban đầu |`{1,2}`,`{3}`,`{4}`,`{5}`| Không có tình bạn trong thành phần thành phố 1 | 0 | 
|`? 1`|`{1,2}`,`{3}`,`{4}`,`{5}`| không | 0 | 
|`F 1 3`|`{1,2}`,`{3}`,`{4}`,`{5}`| 1 và 3 bị ngắt kết nối | 0 | 
|`? 1`|`{1,2}`,`{3}`,`{4}`,`{5}`| không | 0 | 
|`T 3 4`|`{1,2}`,`{3,4}`,`{5}`| tình bạn`(1,4)`vẫn vượt qua các thành phần | 0 | 
|`? 1`|`{1,2}`,`{3,4}`,`{5}`| không | 0 | 

Ví dụ này chứng minh rằng tình bạn có thể được lưu giữ trong thời gian dài mà không ảnh hưởng đến câu trả lời của cả hai điểm cuối. Nếu sau đó chúng ta thêm`T 2 3`, các thành phần`{1,2}`Và`{3,4}`hợp nhất, và cả hai tình bạn`(1,4)`Và`(1,3)`trở nên hợp lệ ngay lập tức. Quét thành phần nhỏ tìm thấy cả hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((m+q+k)\log n\cdot\alpha(n))) | Các hoạt động của DSU gần như không đổi, trong khi danh sách tình bạn của mỗi đỉnh chỉ được quét (O(\log n)) lần | 
| Không gian | (O(n+m+q)) | Mảng DSU, danh sách thành viên thành phần, danh sách lân cận tình bạn và tình bạn ban đầu được lưu trữ | 

Biểu đồ ban đầu chứa tối đa (10^5) cạnh tình bạn và (10^5) cạnh huấn luyện, trong khi tối đa (10^5) hoạt động bổ sung được xử lý. Yếu tố logarit xuất phát từ sự hợp nhất từ ​​nhỏ đến lớn, do đó tổng số lần quét tình bạn vẫn có thể quản lý được. Việc sử dụng bộ nhớ là tuyến tính theo số lượng thành phố và tình bạn được chèn vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    friends = [[] for _ in range(n)]
    friendship_edges = []

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        friends[a].append(b)
        friends[b].append(a)
        friendship_edges.append((a, b))

    parent = list(range(n))
    size = [1] * n
    members = [[i] for i in range(n)]

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def merge(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return

        if size[a] < size[b]:
            a, b = b, a

        parent[b] = a
        size[a] += size[b]
        members[a].extend(members[b])
        members[b] = []

    for _ in range(k):
        a, b = map(int, input().split())
        merge(a - 1, b - 1)

    ans = [0] * n

    for a, b in friendship_edges:
        if find(a) == find(b):
            ans[a] += 1
            ans[b] += 1

    q = int(input())
    out = []

    for _ in range(q):
        parts = input().split()
        typ = parts[0]

        if typ == '?':
            v = int(parts[1]) - 1
            out.append(str(ans[v]))

        elif typ == 'F':
            a = int(parts[1]) - 1
            b = int(parts[2]) - 1

            friends[a].append(b)
            friends[b].append(a)

            if find(a) == find(b):
                ans[a] += 1
                ans[b] += 1

        else:
            a = int(parts[1]) - 1
            b = int(parts[2]) - 1

            ra = find(a)
            rb = find(b)

            if ra == rb:
                continue

            if size[ra] >= size[rb]:
                large, small = ra, rb
            else:
                large, small = rb, ra

            for v in members[small]:
                for u in friends[v]:
                    if find(u) == large:
                        ans[v] += 1
                        ans[u] += 1

            parent[small] = large
            size[large] += size[small]
            members[large].extend(members[small])
            members[small] = []

    return '\n'.join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample1 = """\
4 2 2
1 2
1 3
1 2
1 4
5
? 1
F 4 1
? 1
T 4 3
? 1
"""

assert run(sample1) == "1\n2\n3", "sample 1"

case2 = """\
1 0 0
3
? 1
F 1 1
? 1
"""
# This input violates the original constraint a != b for F, so it is
# deliberately not used as a valid test.

case3 = """\
2 1 0
1 2
2
? 1
T 1 2
"""
assert run(case3) == "0", "friendship before train connection"

case4 = """\
4 2 0
1 3
1 4
5
? 1
T 1 2
? 1
T 2 3
? 1
"""
assert run(case4) == "0\n0\n2", "friendships become reachable after a merge"

case5 = """\
5 0 2
1 2
2 3
5
? 1
F 1 3
? 1
F 4 5
T 3 4
"""
assert run(case5) == "0\n1", "friendship inserted after connectivity"

# Maximum-size structural test. Every query asks about the same isolated city.
# There are 100000 cities and 100000 queries, so this also checks input/output
# handling near the limit.
n = 100000
q = 100000
max_case = f"{n} 0 0\n{q}\n" + "? 100000\n" * q
expected = "0\n" * q
assert run(max_case) == expected[:-1], "maximum-size repeated queries"

# Boundary case with all cities in one train component and every possible
# friendship among three cities.
case7 = """\
3 3 3
1 2
1 3
2 3
1 2
2 3
1 3
4
? 1
F 1 2
? 1
? 3
"""
assert run(case7) == "2\n3\n2", "complete component and repeated queries"
```Xác nhận đầu tiên là mẫu được cung cấp và kiểm tra sự tương tác hoàn chỉnh giữa việc chèn tình bạn và việc hợp nhất thành phần đào tạo. 

Trường hợp hợp lệ thứ hai sử dụng đồ thị nhỏ nhất có thể có hai thành phố. Tình bạn tồn tại ngay từ đầu nhưng lại không có đường tàu nên câu trả lời là con số không. 

Trường hợp hợp lệ thứ ba kiểm tra tình bạn chỉ có thể truy cập được sau khi hai thành phần đoàn tàu riêng biệt được hợp nhất. Nó nắm bắt các triển khai chỉ cập nhật câu trả lời khi có tình bạn. 

Trường hợp hợp lệ thứ tư sẽ chèn tình bạn sau khi điểm cuối của nó đã được kết nối. Nó xác minh rằng`F`ngay lập tức tăng các câu trả lời khi gốc DSU khớp. 

Thử nghiệm kích thước tối đa thực hiện (10^5) truy vấn giống hệt nhau trên (10^5) thành phố bị cô lập. Nó nhấn mạnh việc xử lý đầu vào và lặp lại các truy vấn có thời gian liên tục mà không cần xây dựng một tập cạnh lớn không cần thiết. 

Trường hợp cuối cùng đặt mọi thành phố vào một thành phần được kết nối ngay từ đầu và có tất cả các mối quan hệ bạn bè có thể có giữa ba thành phố. Nó kiểm tra xem câu trả lời có tính mỗi điểm cuối tình bạn chính xác một lần không và các truy vấn lặp lại không thay đổi trạng thái. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`1`,`2`,`3`| Tương tác hoàn chỉnh của cả hai loại cập nhật | 
|`2 1 0`với`? 1`|`0`| Tình bạn không hàm ý kết nối tàu | 
| Trường hợp hợp nhất bốn thành phố |`0`,`0`,`2`| Bạn có thể tiếp cận một số tình bạn sau khi hợp nhất | 
| Vụ án tình bạn năng động năm thành phố |`0`,`1`| Tình bạn gắn kết sau kết nối | 
| (n=q=100000), các truy vấn lặp lại riêng biệt | 100000 số không | Kích thước đầu vào tối đa và xử lý ranh giới | 
| Ba thành phố được kết nối đầy đủ |`2`,`3`,`2`| Thành phần hoàn chỉnh, truy vấn trùng lặp, đếm chính xác | 

## Vỏ cạnh 

### Tình bạn tồn tại nhưng các thành phố bị ngắt kết nối 

Hãy xem xét:```
2 1 0
1 2
1
? 1
```Danh sách lân cận tình bạn chứa thành phố 2 cho thành phố 1, nhưng`find(1)`Và`find(2)`là khác nhau. Tình bạn ban đầu không đóng góp gì cho`ans[1]`, vì vậy truy vấn sẽ in`0`. Nếu sau này`T 1 2`hoạt động xảy ra, thành phần nhỏ hơn chứa một thành phố, tình bạn của nó với thành phần khác được tìm thấy trong quá trình hợp nhất và`ans[1]`trở thành`1`. 

### Tình bạn được thêm vào bên trong thành phần tàu hiện có 

Hãy xem xét:```
2 0 1
1 2
2
F 1 2
? 1
```Tuyến đường tàu ban đầu đặt cả hai thành phố dưới cùng một gốc DSU. Khi`F 1 2`được xử lý, các gốc khớp nhau, do đó cả hai câu trả lời đều được tăng lên ngay lập tức. Truy vấn in`1`. 

### Một chuyến tàu hợp nhất kích hoạt nhiều tình bạn cũ 

Hãy xem xét:```
4 2 1
1 3
1 4
1 2
2
? 1
T 2 3
```Ban đầu thành phố 1 thuộc về`{1,2}`, trong khi thành phố 3 và 4 thì tách biệt. Truy vấn đầu tiên in`0`. Khi`T 2 3`được chèn vào,`{1,2}`hợp nhất với`{3}`. Thành phần nhỏ hơn là`{3}`và danh sách bạn bè của nó chứa thành phố 1. Số gia hợp nhất`ans[3]`Và`ans[1]`, vậy tình bạn`(1,3)`bây giờ đã được tính. 

Cơ chế tương tự xử lý tùy ý nhiều tình bạn vượt qua. Nếu thành phần nhỏ hơn chứa (x) đỉnh và danh sách tình bạn của chúng chứa (r) cạnh liên quan, thì tất cả (r) cạnh đều được xử lý trong quá trình hợp nhất đó. 

### Cạnh tàu kết nối các thành phố đã có trong cùng một thành phần 

Hãy xem xét:```
3 1 2
1 3
1 2
2 3
1
? 1
```Cả ba thành phố đều đã được kết nối trước khi tuyến tàu thứ hai được xem xét. Căn DSU của 2 và 3 bằng nhau nên cạnh thứ hai không quét và không thay đổi câu trả lời. Thành phố 1 có một người bạn, thành phố 3, vì vậy truy vấn sẽ in ra`1`. 

Bỏ qua`ra == rb`trường hợp này sẽ gây ra công việc không cần thiết và có thể dẫn đến việc đếm lặp lại không chính xác nếu thành phần được quét mặc dù không có kết nối mới. 

### Thành phần nhỏ hơn thay đổi từ hợp nhất sang hợp nhất 

Giả sử đồ thị đoàn tàu ban đầu là năm thành phố biệt lập và các tuyến đường được chèn vào dưới dạng`(1,2)`,`(3,4)`,`(1,3)`,`(1,5)`. Hai sự hợp nhất đầu tiên xử lý các thành phần một thành phố. Lần hợp nhất thứ ba xử lý một thành phần hai thành phố với một thành phần hai thành phố khác. Lần hợp nhất thứ tư xử lý thành phần một thành phố chứa 5 so với thành phần đã lớn hơn. 

Một thành phố chỉ có thể ở phía nhỏ hơn khi kích thước thành phần của nó tối đa bằng một nửa kích thước thành phần thu được. Mỗi sự kiện như vậy ít nhất sẽ tăng gấp đôi kích thước thành phần của nó. Bắt đầu từ kích thước một, điều này có thể xảy ra nhiều nhất (\lfloor\log_2 n\rfloor) lần, đó là lý do khiến việc quét tình bạn lặp đi lặp lại vẫn bị giới hạn.
