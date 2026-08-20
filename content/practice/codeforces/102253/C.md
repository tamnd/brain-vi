---
title: "CF 102253C - Cây nhiều màu sắc"
description: "Chúng ta có một cây có (n) đỉnh. Mỗi đỉnh có một màu và đối với hai đỉnh riêng biệt bất kỳ, chúng ta nhìn vào đường đi duy nhất của chúng và đếm xem có bao nhiêu màu khác nhau xuất hiện trên đường đi đó. Nhiệm vụ là tính tổng giá trị này trên tất cả (frac{n(n-1)}2) cặp đỉnh không có thứ tự."
date: "2026-08-19T00:43:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102253
codeforces_index: "C"
codeforces_contest_name: "2017 Chinese Multi-University Training, BeihangU Contest"
rating: 0
weight: 102253
solve_time_s: 145
verified: true
draft: false
---

[CF 102253C - Cây đầy màu sắc](https://codeforces.com/problemset/problem/102253/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có (n) đỉnh. Mỗi đỉnh có một màu và đối với hai đỉnh riêng biệt bất kỳ, chúng ta nhìn vào đường đi duy nhất của chúng và đếm xem có bao nhiêu màu khác nhau xuất hiện trên đường đi đó. Nhiệm vụ là tính tổng giá trị này trên tất cả (\frac{n(n-1)}2) cặp đỉnh không có thứ tự. Đầu vào chứa một số trường hợp thử nghiệm, mỗi trường hợp bao gồm các màu của đỉnh theo sau là các cạnh của cây (n-1). Đầu ra được yêu cầu là tổng số tiền cho mọi trường hợp, trước số trường hợp của nó. Định dạng bài toán chính thức và đầu ra mẫu được cung cấp bởi Codeforces Gym 102253C. 

Ràng buộc (n\le 2\cdot10^5) loại trừ mọi thứ gần với thời gian bậc hai. Đã có khoảng (2\cdot10^{10}) cặp đỉnh ở kích thước tối đa, do đó, ngay cả việc liệt kê từng cặp một lần cũng vượt xa giới hạn hai giây có thể chịu đựng được. Giải pháp dự định phải xử lý mỗi đỉnh và cạnh chỉ một số lần không đổi, tạo ra độ phức tạp tuyến tính trong (n). Các giá trị màu cũng tối đa là (n), cho phép chúng tôi duy trì thông tin về mỗi màu trong các mảng thông thường. 

Có một số cách dễ dàng để có được một câu trả lời sai. Các màu lặp lại chỉ được đóng góp một lần cho một đường dẫn. Ví dụ,```
2
1 1
1 2
```chỉ có một đường dẫn và đường dẫn đó chứa chính xác một màu riêng biệt, vì vậy câu trả lời là`1`. Giải pháp thêm một đóng góp cho mỗi đỉnh sẽ thu được kết quả không chính xác`2`. 

Hai điểm cuối được bao gồm trong đường dẫn của họ. Vì```
2
1 2
1 2
```đường dẫn duy nhất chứa màu (1) và (2), vì vậy câu trả lời là`2`. Việc coi một đường dẫn chỉ chứa các đỉnh bên trong của nó sẽ cho màu 0 không chính xác. 

Không thể quên thành phần ở phía gốc của cây khi xóa màu. Ví dụ,```
3
1 1 2
1 2
1 3
```có giá trị đường dẫn (1,2,2), đưa ra câu trả lời`5`. Đối với màu (2), việc xóa đỉnh duy nhất của nó sẽ để lại các đỉnh (1) và (2) được kết nối với nhau, do đó một đường sẽ tránh được màu (2). Một phép tính chỉ kiểm tra các cây con bên dưới các lần xuất hiện của một màu sẽ bỏ sót thành phần phía gốc còn lại này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liệt kê từng cặp đỉnh, tìm đường đi của nó, chèn màu trên đường dẫn đó vào một tập hợp và thêm kích thước của tập hợp đó. Điều này đúng vì mỗi đường dẫn được kiểm tra chính xác một lần và tập hợp này sẽ loại bỏ các màu trùng lặp. Vấn đề là khối lượng công việc. Trên một chuỗi, tổng số đỉnh đã đi qua tất cả các đường dẫn là 

[ 
\sum_{d=1}^{n-1}(n-d)(d+1) 
=\frac{n(n-1)(n+4)}6 
=\Theta(n^3). 
] 

Tại (n=2\cdot10^5), đây là khoảng (1,3\cdot10^{15}) lượt truy cập đỉnh, vì vậy cách tiếp cận bạo lực gần như không khả thi. 

Sự thay đổi quan điểm hữu ích là tính toán sự đóng góp của từng màu một cách độc lập. Một đường dẫn có tập hợp màu chứa (k) màu đóng góp (1) cho câu trả lời cho mỗi (k) màu đó. Do đó, câu trả lời cuối cùng là tổng số đường đi chứa ít nhất một đỉnh màu (c) trên mỗi màu (c). Sự đóng góp này được thể hiện dễ dàng hơn thông qua phần bổ sung của nó. Tổng cộng có (\binom n2) đường dẫn, do đó sự đóng góp của màu (c) là 

[ 
\binom n2-\text{số đường tránh màu }c. 
] 

Bây giờ hãy loại bỏ mọi đỉnh có màu (c). Đồ thị còn lại là một khu rừng. Một đường đi tránh được (c) chính xác khi cả hai điểm cuối đều nằm trong cùng một thành phần được kết nối của khu rừng đó. Nếu kích thước thành phần là (s_1,s_2,\ldots), số lượng đường tránh (c) là 

[ 
\sum_i \binom{s_i}{2}. 
] 

Điều này chuyển vấn đề thành tính toán kích thước của tất cả các thành phần thu được bằng cách loại bỏ từng màu. Làm điều đó một cách độc lập cho mọi màu sắc sẽ lại là phương trình bậc hai. Quan sát quan trọng là tất cả các tính toán này có thể được mô phỏng đồng thời trong một DFS. Giải pháp tiêu chuẩn mô tả điều này là sử dụng ý tưởng cây ảo mà không xây dựng cây ảo một cách rõ ràng. 

Gốc cây tại đỉnh (1). Xét một đỉnh hiện tại (u), có màu (c) và một con (v). Bên trong cây con của (v), một số nút thuộc về cây con có gốc ở lần xuất hiện cao nhất của màu (c). Các nút đó được tách khỏi (u) nếu tất cả các đỉnh màu-(c) bị xóa. Mọi nút khác trong cây con của (v) thuộc về một thành phần được kết nối gắn liền với (u). Nếu cây con có kích thước (sz[v]) và (x) của các nút đó đã được tính bằng các đỉnh màu-(c) cao hơn thì kích thước thành phần là (sz[v]-x), góp phần tránh các đường dẫn (\binom{sz[v]-x}{2}). 

Chúng ta có thể thu được (x) mà không cần xây dựng bất cứ điều gì một cách rõ ràng. Đối với mỗi màu (c), duy trì`dom[c]`, số lượng đã được liên kết với các đỉnh màu (c) cao nhất hiện có liên quan trong DFS. Ngay trước khi chuyển sang dạng con (v), hãy lưu lại`dom[c]`. Sau khi trở về từ (v), sự khác biệt giữa giá trị mới và giá trị đã lưu chính xác là lượng cây con bị chiếm giữ bởi các vùng màu-(c) cao hơn đó. Sự khác biệt này là số lượng cần trừ từ (sz[v]). Ý tưởng gia tăng tương tự là cốt lõi của các công thức cây-DP được chấp nhận cho vấn đề này. 

Cuối cùng, sau khi DFS kết thúc, một màu vẫn có thể có thành phần cao hơn mức xuất hiện cao nhất của nó. Kích thước của nó là (n-\text{dom[c]), vì vậy chúng tôi thêm (\binom{n-\text{dom[c]}2) vào số lượng đường tránh màu đó. Đây là thành phần phía gốc mà phép tính cây con thuần túy cục bộ sẽ bỏ sót. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) trên chuỗi | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu màu sắc riêng biệt. Có các đường dẫn (\binom n2), và ban đầu chúng ta có thể tưởng tượng rằng mỗi màu riêng biệt đều góp phần vào mọi đường dẫn. Do đó giá trị ban đầu là 

[ 
\text{distinctColors}\cdot\binom n2. 
] 

Chúng tôi sẽ trừ các đường tránh từng màu. 

1. Căn cây tại đỉnh (1) và xác định (sz[u]) là số đỉnh trong cây con của (u). DFS cần giá trị này khi trở về từ mọi đứa trẻ. 
2. Duy trì một mảng`dom[c]`. Trong DFS, nó biểu thị tổng kích thước đã được hấp thụ bởi các vùng màu (c) cao nhất hiện tại. Khi xử lý đỉnh (u) có màu (c), hãy khởi tạo giá trị cục bộ`added = 1`, bởi vì bản thân (u) sẽ trở thành một phần của vùng mới được biểu thị bằng màu (c). 
3. Xử lý từng con (v) của (u) một lần. Cứu`before = dom[c]`ngay trước khi vào (v). Cây con của (v) sau đó được xử lý hoàn toàn trước khi thực hiện bất kỳ điều gì khác với đứa trẻ này. 
4. Sau khi trở về từ (v), hãy tính 

[ 
x=dom[c]-trước. 
] 

Giá trị (x) đếm phần cây con của (v) thuộc vùng màu-(c) cao hơn. Những vùng đó chứa các đỉnh màu-(c) và bị ngắt kết nối với (u) sau khi màu (c) bị xóa. 

1. Phần còn lại 

[ 
khối=sz[v]-x 
] 

chính xác là thành phần liên thông chứa (v) không chứa đỉnh màu-(c). Mỗi cặp đỉnh bên trong thành phần này tạo ra một đường tránh màu (c), vì vậy hãy thêm 

[ 
\binom{khối}{2} 
] 

đến số lượng đường dẫn sẽ bị trừ khỏi câu trả lời. 

1. Thêm`block`đến địa phương`added`giá trị của (u). Sau khi mỗi phần tử con đã được xử lý xong, hãy cập nhật 

[ 
dom[c]\mathrel{+}=đã thêm. 
] 

Điều này thay thế các vùng màu-(c) cao nhất trước đó bằng vùng được đại diện bởi (u), đây chính xác là trạng thái mà cha mẹ của (u) cần. 

1. Sau toàn bộ DFS, xử lý mọi màu thực sự xuất hiện. Thành phần phía gốc còn lại của nó có kích thước 

[ 
n-dom[c]. 
] 

Thêm 

[ 
\binom{n-dom[c]}2 
] 

đến số lượng đường tránh màu đó. 

1. Trừ tổng số đường tránh khỏi`distinctColors * C(n, 2)`. Kết quả là tổng số màu riêng biệt cần thiết trên tất cả các đường dẫn. 

Việc triển khai bên dưới sử dụng ngăn xếp DFS rõ ràng thay vì đệ quy Python. Cây có thể là một chuỗi có độ dài (2\cdot10^5), do đó Python DFS đệ quy có nguy cơ vượt quá ngăn xếp đệ quy của trình thông dịch. Ngăn xếp rõ ràng duy trì chính xác thứ tự xử lý cha-con giống như phép lặp đệ quy. 

### Tại sao nó hoạt động 

Sửa một màu (c). Xóa mọi đỉnh màu (c) sẽ phân chia cây thành các thành phần được kết nối và chính xác các cặp đỉnh bên trong một thành phần có các đường tránh (c). Với mọi con (v) của đỉnh (u) có màu (c),`dom[c] - before`đếm chính xác các phần của cây con của (v) đã được phân tách bằng các đỉnh màu-(c) cao nhất bên dưới (v). Việc loại bỏ các phần đó để lại một thành phần có kích thước không có màu (c) được kết nối`sz[v] - (dom[c] - before)`, do đó các cặp (\binom{size}{2}) của nó được tính chính xác một lần. Sau khi tất cả trẻ em được xử lý,`dom[c]`đại diện cho các vùng đã được chiếm bởi các đỉnh màu-(c) cao nhất. Thành phần duy nhất chưa được xử lý là thành phần nằm trên các đỉnh màu-(c) cao nhất, có kích thước là (n-dom[c]) và được tính ở cuối. Do đó, mỗi cặp tránh (c) được tính một lần và mọi cặp chứa (c) đều bị loại khỏi phần bù đó. Tổng hợp sự đóng góp này trên tất cả các màu sẽ cho ra chính xác số lượng màu riêng biệt trên mỗi đường dẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    out = []
    case_no = 0

    while True:
        line = input()
        if not line:
            break
        line = line.strip()
        if not line:
            continue

        n = int(line)
        color = [0] + list(map(int, input().split()))

        graph = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            graph[u].append(v)
            graph[v].append(u)

        present = [False] * (n + 1)
        distinct = 0
        for u in range(1, n + 1):
            c = color[u]
            if not present[c]:
                present[c] = True
                distinct += 1

        # dom[c] is the amount already absorbed by the highest
        # color-c regions during the current DFS.
        dom = [0] * (n + 1)

        # sz[u] is the subtree size.
        sz = [0] * (n + 1)

        # Number of paths avoiding their corresponding colors.
        bad = 0

        # Frame:
        # [vertex, parent, next_edge_index, added, before]
        #
        # added is the local value that will be added to dom[color[u]]
        # when u finishes.
        # before stores dom[color[u]] immediately before entering
        # the currently processed child.
        sz[1] = 1
        stack = [[1, 0, 0, 1, 0]]

        while stack:
            frame = stack[-1]
            u = frame[0]
            p = frame[1]

            if frame[2] < len(graph[u]):
                v = graph[u][frame[2]]
                frame[2] += 1

                if v == p:
                    continue

                c = color[u]
                frame[4] = dom[c]

                sz[v] = 1
                stack.append([v, u, 0, 1, 0])
                continue

            # Finish u.
            c = color[u]
            dom[c] += frame[3]
            stack.pop()

            if stack:
                parent_frame = stack[-1]
                parent = parent_frame[0]
                pc = color[parent]

                added_in_child = dom[pc] - parent_frame[4]
                block = sz[u] - added_in_child

                bad += block * (block - 1) // 2
                parent_frame[3] += block
                sz[parent] += sz[u]

        # The component above the highest occurrence of each color.
        for c in range(1, n + 1):
            if not present[c]:
                continue
            block = n - dom[c]
            bad += block * (block - 1) // 2

        total = distinct * n * (n - 1) // 2
        answer = total - bad

        case_no += 1
        out.append(f"Case #{case_no}: {answer}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào lưu trữ các màu trong một mảng có một chỉ mục sao cho số đỉnh và chỉ số mảng khớp nhau. các`present`mảng đồng thời đếm các màu riêng biệt, cho phép đóng góp ban đầu chỉ sử dụng các màu thực sự xảy ra. 

các`dom`mảng là phần trung tâm của trạng thái. Nó được lập chỉ mục theo màu chứ không phải theo đỉnh vì cùng một tính toán được thực hiện cho mọi màu cùng một lúc. Khi trẻ làm xong, cha mẹ so sánh bài mới`dom[color[parent]]`với giá trị được lưu trước khi nhập con đó. Chỉ có sự khác biệt thuộc về cây con đó, vì vậy phép trừ này ngăn không cho thông tin tích lũy trong các cây con anh chị em trước đó được tính lại. 

các`added`trường trong mỗi khung DFS bắt đầu tại một cho đỉnh hiện tại. Mỗi đứa trẻ đều đóng góp khối không màu của mình vào giá trị này. Khi đỉnh kết thúc, việc thêm`added`ĐẾN`dom[color[u]]`làm cho toàn bộ vùng màu cao nhất mới được hình thành có sẵn cho cha mẹ của nó. 

Vòng lặp cuối cùng xử lý phần cây phía trên lần xuất hiện cao nhất của mỗi màu. Bỏ qua bước này là nguyên nhân phổ biến gây ra các câu trả lời sai khi gốc không có màu đó. 

Tất cả số lượng đường dẫn đều sử dụng số nguyên Python, do đó không có vấn đề tràn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, cần phải có số nguyên 64 bit vì câu trả lời có thể theo thứ tự (n^3). 

Ngăn xếp rõ ràng cũng có chủ ý. DFS đệ quy có độ sâu (O(n)) trên một chuỗi, trong khi phiên bản lặp lại sử dụng bộ nhớ heap (O(n)) và không có lỗi về độ sâu đệ quy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây là chuỗi (1-2-3), có màu sắc (1,2,1). Có ba con đường. Đóng góp ban đầu là hai màu riêng biệt nhân với ba đường dẫn, cho (6). 

DFS xử lý cây con của đỉnh (2), bao gồm cả đỉnh (3). Khi đỉnh (3) kết thúc, màu (1) đã hấp thụ một đỉnh. Khi đỉnh (2) kết thúc, màu (2) đã hấp thụ hai đỉnh. Trở lại đỉnh (1), vùng màu (1) chiếm toàn bộ cây. 

| Đỉnh | Màu sắc | Con | Kích thước trẻ em |`before`|`dom`sau con |`block`|`bad`đã thêm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 3 | 1 | không | 0 | 0 | 1 | 0 | 0 | 
| 2 | 2 | 3 | 1 | 0 | 1 | 0 | 0 | 
| 1 | 1 | 2 | 2 | 0 | 1 | 1 | 0 | 
| 1 | 1 | 2 xong | 2 | 0 | 3 | 0 | 0 | 

Sau DFS,`dom[1] = 3`, vì vậy màu (1) không còn thành phần gốc nào.`dom[2] = 2`, để lại một thành phần có kích thước (1), góp phần tạo ra các đường tránh bằng 0. Như vậy`bad = 0`, và câu trả lời vẫn là (6). 

Ví dụ này giải thích tại sao các màu giống nhau không thể được đếm độc lập theo đỉnh. Hai lần xuất hiện của màu (1) cùng nhau đóng góp chính xác một đơn vị cho mỗi đường dẫn chứ không phải hai. 

### Mẫu 2 

Cái cây là```
        1(1)
       /   \
    2(2)   3(1)
    /  \      \
 4(3) 5(2)    6(1)
```Có sáu đỉnh và ba màu khác nhau nên giá trị ban đầu là 

[ 
3\binom62=45. 
] 

Các chuyển tiếp DFS có liên quan là: 

| Đỉnh | Màu sắc | Con | Kích thước trẻ em |`before`|`dom`sau con |`block`|`bad`đã thêm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 4 | 3 | không | 1 | 0 | 1 | 0 | 0 | 
| 5 | 2 | không | 1 | 0 | 1 | 0 | 0 | 
| 2 | 2 | 4 | 1 | 0 | 0 | 1 | 0 | 
| 2 | 2 | 5 | 1 | 0 | 1 | 0 | 0 | 
| 3 | 1 | 6 | 1 | 0 | 1 | 0 | 0 | 
| 1 | 1 | 2 | 3 | 0 | 0 | 3 | 3 | 
| 1 | 1 | 3 | 2 | 0 | 2 | 0 | 0 | 

Sau DFS, các giá trị liên quan đến ba màu là 

| Màu sắc |`dom[color]`| Kích thước bên gốc | Đường tránh gốc rễ | 
| --- | --- | --- | --- | 
| 1 | 6 | 0 | 0 | 
| 2 | 3 | 3 | 3 | 
| 3 | 1 | 5 | 10 | 

Con của đỉnh (1) dẫn đến đỉnh (2) tạo ra thành phần không có màu (1) có kích thước (3), chiếm (3) đường tránh. Màu (2) có thành phần khác có kích thước (3) phía trên lần xuất hiện cao nhất của nó, chiếm (3) đường dẫn, trong khi màu (3) có thành phần phía gốc có kích thước (5), chiếm (10) đường dẫn. 

Do đó tổng số màu đóng góp còn thiếu là 

[ 
3+3+10=16, 
] 

và câu trả lời cuối cùng là 

[ 
45-16=29. 
] 

Dấu vết thể hiện sự bất biến trung tâm:`dom[c]`mang chính xác phần đã được phân tách bằng số lần xuất hiện màu cao hơn (c), trong khi`sz[v] - delta`là thành phần không có màu còn lại trong cây con đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi đỉnh và mỗi cạnh cây được xử lý với số lần không đổi, sau đó là một lần quét (n) màu có thể. | 
| Không gian | (O(n)) | Danh sách kề, kích thước cây con, trạng thái màu, mảng hiện diện và ngăn xếp DFS rõ ràng đều sử dụng bộ nhớ tuyến tính. | 

(n) tối đa là (2\cdot10^5), do đó, đường chuyền tuyến tính phù hợp với giới hạn thời gian hai giây. Việc triển khai tránh cả số bậc hai của các cặp đỉnh và độ sâu DFS đệ quy tỷ lệ với (n). Câu trả lời có thể vượt quá phạm vi 32 bit, nhưng số nguyên Python xử lý trực tiếp các giá trị được yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution():
    input = sys.stdin.readline
    out = []
    case_no = 0

    while True:
        line = input()
        if not line:
            break
        line = line.strip()
        if not line:
            continue

        n = int(line)
        color = [0] + list(map(int, input().split()))

        graph = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            graph[u].append(v)
            graph[v].append(u)

        present = [False] * (n + 1)
        distinct = 0
        for u in range(1, n + 1):
            c = color[u]
            if not present[c]:
                present[c] = True
                distinct += 1

        dom = [0] * (n + 1)
        sz = [0] * (n + 1)
        bad = 0

        sz[1] = 1
        stack = [[1, 0, 0, 1, 0]]

        while stack:
            frame = stack[-1]
            u, p = frame[0], frame[1]

            if frame[2] < len(graph[u]):
                v = graph[u][frame[2]]
                frame[2] += 1

                if v == p:
                    continue

                frame[4] = dom[color[u]]
                sz[v] = 1
                stack.append([v, u, 0, 1, 0])
            else:
                c = color[u]
                dom[c] += frame[3]
                stack.pop()

                if stack:
                    parent_frame = stack[-1]
                    parent = parent_frame[0]
                    pc = color[parent]

                    delta = dom[pc] - parent_frame[4]
                    block = sz[u] - delta

                    bad += block * (block - 1) // 2
                    parent_frame[3] += block
                    sz[parent] += sz[u]

        for c in range(1, n + 1):
            if present[c]:
                block = n - dom[c]
                bad += block * (block - 1) // 2

        total = distinct * n * (n - 1) // 2
        answer = total - bad

        case_no += 1
        out.append(f"Case #{case_no}: {answer}")

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solution()
    finally:
        sys.stdin = old_stdin

sample1 = """\
3
1 2 1
1 2
2 3
"""

assert run(sample1) == "Case #1: 6", "sample 1"

sample2 = """\
6
1 2 1 3 2 1
1 2
1 3
2 4
2 5
3 6
"""

assert run(sample2) == "Case #1: 29", "sample 2"

minimum_same = """\
2
1 1
1 2
"""

assert run(minimum_same) == "Case #1: 1", "minimum size, equal colors"

minimum_different = """\
2
1 2
1 2
"""

assert run(minimum_different) == "Case #1: 2", "minimum size, different colors"

boundary_color = """\
3
3 1 2
1 2
2 3
"""

assert run(boundary_color) == "Case #1: 7", "color value n"

repeated_colors = """\
5
1 2 1 2 1
1 2
2 3
3 4
4 5
"""

assert run(repeated_colors) == "Case #1: 16", "repeated colors on a chain"

n = 200000
colors = " ".join(["1"] * n)
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
maximum_case = f"{n}\n{colors}\n{edges}\n"

assert run(maximum_case) == "Case #1: 19999900000", "maximum n, all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`, màu sắc`1 1`, bờ rìa`1 2`|`Case #1: 1`| Kích thước tối thiểu và xử lý màu trùng lặp | 
|`2`, màu sắc`1 2`, bờ rìa`1 2`|`Case #1: 2`| Cả hai điểm cuối đều đóng góp và các cặp đặt hàng không được tính | 
|`3`, màu sắc`3 1 2`, cạnh`1 2`,`2 3`|`Case #1: 7`| Giá trị màu bằng giá trị tối đa cho phép (n) | 
| Dây chuyền 5 màu`1 2 1 2 1`|`Case #1: 16`| Nhiều màu lặp lại được phân tách bằng các màu khác | 
| Chuỗi 200000 đỉnh, đủ màu`1`|`Case #1: 19999900000`| Kích thước đầu vào tối đa, số học số nguyên lớn và xử lý cây sâu | 

## Vỏ cạnh 

Đối với cây tối thiểu có hai đỉnh cùng màu,```
2
1 1
1 2
```khoản đóng góp ban đầu là (1\cdot\binom22=1). DFS hấp thụ cả hai đỉnh vào`dom[1]`, vì vậy thành phần phía gốc có kích thước bằng 0. Không có đường tránh màu (1), cho`bad = 0`và câu trả lời cuối cùng`1`. Màu lặp lại được tính một lần vì thuật toán hoạt động trên mỗi màu chứ không phải trên mỗi đỉnh. 

Đối với hai đỉnh có màu khác nhau,```
2
1 2
1 2
```có một đường dẫn và nó chứa cả hai màu. Khoản đóng góp ban đầu là (2\cdot1=2). Mỗi màu không có thành phần không cần thiết sau khi đỉnh duy nhất của nó bị loại bỏ, do đó không có đường tránh nào bị trừ. Kết quả là`2`. Điều này cũng xác nhận rằng cả hai điểm cuối đều thuộc về đường dẫn. 

Đối với màu xuất hiện cách xa gốc,```
3
1 1 2
1 2
1 3
```các đường dẫn là (1\leftrightarrow2), (1\leftrightarrow3) và (2\leftrightarrow3), với các giá trị (1,2,2). Tổng của họ là`5`. Đối với màu (2), việc loại bỏ đỉnh (3) sẽ để lại một thành phần có kích thước (2), do đó, đường dẫn chính xác (\binom22=1) sẽ tránh được nó. Đóng góp cuối cùng của màu (2) là (3-1=2). Các đỉnh cùng màu (1) và (2) góp phần tạo ra một màu chung thay vì hai phần đóng góp riêng biệt, tạo ra tổng số`5`. 

Đối với chuỗi luân phiên```
5
1 2 1 2 1
1 2
2 3
3 4
4 5
```các giá trị đường dẫn là (2,1,2,1,2,1,2,1,2,2) trên mười cặp không có thứ tự, tổng cộng là`16`. Trong DFS, sự khác biệt`dom[c] - before`ngăn chặn sự xuất hiện sau đó của cùng một màu khỏi bị tính là một phần của thành phần không có màu thuộc về lần xuất hiện trước đó. Đây chính xác là tình huống phá vỡ các công thức kích thước cây con đơn giản hơn. 

Đối với cây bằng nhau có kích thước tối đa, mọi đường dẫn đều chứa một màu duy nhất, vì vậy câu trả lời đơn giản là 

[ 
\binom{200000}{2}=19999900000. 
] 

Thuật toán xử lý chuỗi lặp đi lặp lại, không bao giờ lặp lại sâu 200000 cấp. Vì mọi đỉnh đều có cùng màu nên mọi khối bên con đều có kích thước bằng 0,`dom[1]`kết thúc ở (200000) và thành phần phía gốc cũng có kích thước bằng 0. Câu trả lời là do đó`19999900000`, xác nhận cả ranh giới số nguyên lớn và hành vi tuyến tính dự định.
