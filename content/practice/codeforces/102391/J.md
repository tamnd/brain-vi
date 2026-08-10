---
title: "CF 102391J - Parklife"
description: "Mỗi cây cầu có thể được biểu diễn bằng khoảng nửa mở ([Si,Ei)). Một cây cầu có thể nhìn thấy được từ cung nhỏ giữa các điểm liên tiếp (i) và (i+1) chính xác khi cung đó nằm trong khoảng này. Đầu vào chứa các cầu có trọng số (N)."
date: "2026-08-10T20:10:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "J"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 193
verified: true
draft: false
---

[CF 102391J - Parklife](https://codeforces.com/problemset/problem/102391/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi cây cầu có thể được biểu diễn bằng khoảng nửa mở ([S_i,E_i)). Một cây cầu có thể nhìn thấy được từ cung nhỏ giữa các điểm liên tiếp (i) và (i+1) chính xác khi cung đó nằm trong khoảng này. 

Đầu vào chứa các cầu có trọng số (N). Chúng ta có thể chọn bất kỳ tập con nào trong số chúng, nhưng với mỗi cung cơ sở, nhiều nhất (k) cầu được chọn có thể bao phủ nó. Vì mọi giá trị thẩm mỹ đều mang tính tích cực nên nhiệm vụ là tối đa hóa tổng giá trị của những cây cầu đã chọn. Chúng ta cần mức tối đa này cho mọi (k=1,2,\ldots,N). 

Điều kiện hình học mạnh hơn nhiều so với lần đầu tiên nó xuất hiện. Hai đoạn cầu không thể cắt nhau nên khoảng cách tầm nhìn của chúng không bao giờ có thể trùng nhau một phần. Đối với hai khoảng bất kỳ, chúng hoặc rời rạc hoặc một khoảng hoàn toàn chứa khoảng kia. Cấu trúc tầng này là thuộc tính trung tâm của vấn đề. Bài xã luận chính thức thực hiện việc giảm tương tự bằng cách xem các khoảng như các cặp dấu ngoặc phù hợp và sau đó tạo thành một cây phân tích. 

Giá trị của (N) có thể đạt tới (250.000), do đó, chương trình động (O(N^2)) sẽ kiểm tra ít nhất (62,5) tỷ trạng thái trong trường hợp xấu nhất. Điều đó vượt xa những gì giới hạn cuộc thi 2 giây có thể chịu đựng được. Phạm vi tọa độ đạt đến (10^6), nhưng điều đó không có nghĩa là chúng ta nên xây dựng một mảng trên tất cả các tọa độ và chạy DP dựa trên tọa độ. Cấu trúc hữu ích được xác định bởi chính các cầu nối (N), do đó thuật toán phải gần với (O(N\log N)). 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. 

Hãy xem xét một cây cầu duy nhất.```
1
1 2 7
```Chỉ có một cây cầu nên mọi (k\ge1) đều chọn nó. Câu trả lời là```
7
```Việc triển khai bất cẩn coi khoảng thời gian là đóng thay vì nửa mở có thể khiến các cầu chạm nhau ở điểm cuối chồng lên nhau một cách không chính xác. Ví dụ,```
2
1 2 5
2 3 7
```Hai khoảng nhìn thấy được là ([1,2)) và ([2,3)), vì vậy chúng rời rạc. Cả hai cây cầu có thể được chọn ngay cả với (k=1), cho```
12 12
```Một trường hợp quan trọng khác là một chuỗi các khoảng lồng nhau.```
3
1 6 10
2 5 20
3 4 30
```Đối với (k=1), chỉ có thể chọn cầu giá trị (30). Với (k=2), hai cầu bên trong có thể được chọn và với (k=3) cả ba cầu có thể được chọn. Đầu ra đúng là```
30 50 60
```Một giải pháp chỉ lấy các giá trị (k) tốt nhất trên toàn cầu sẽ thất bại vì ràng buộc lồng nhau phụ thuộc vào vị trí các cầu nối chồng lên nhau. 

Cuối cùng, các khoảng có thể có cùng điểm cuối bên trái. Ví dụ,```
3
1 5 10
1 4 20
2 3 30
```Thứ tự ngăn chặn đúng là ([1,5)) chứa ([1,4)) chứa ([2,3)). Chỉ sắp xếp theo điểm cuối bên trái là không đủ để khôi phục thứ tự đó. Khi các điểm cuối bên trái bằng nhau, khoảng thời gian dài hơn phải đến trước. 

## Phương pháp tiếp cận 

Cây trực tiếp DP là lần thử đầu tiên tự nhiên. Khi các khoảng được chuyển đổi thành cây ngăn chặn, hãy xác định (F_u(k)) là giá trị tối đa có thể đạt được từ cây con của (u) khi mọi đường dẫn từ gốc đến con cháu chứa tối đa (k) đỉnh được chọn. Đối với mọi nút và mọi (k), chúng ta có thể tính giá trị này bằng cách xem xét liệu chính (u) có được chọn hay không. 

DP này đúng vì mọi lựa chọn khả thi đều có chính xác một trong hai khả năng đó. Nếu (u) không được chọn thì mọi cây con con vẫn có dung lượng (k). Nếu (u) được chọn, nó sẽ tiêu thụ một đơn vị dung lượng cho mỗi khoảng bên trong nó, do đó mọi đứa trẻ đều nhận được dung lượng (k-1). Vấn đề là số lượng trạng thái. Có (O(N^2)) cặp có thể ((u,k)), cho ít nhất (62,5) tỷ trạng thái khi (N=250.000), trước khi tính đến công việc cần thiết để kết hợp các phần tử con. 

DP bạo lực hoạt động vì cây ngăn chặn nắm bắt mọi tương tác giữa các cây cầu. Nó thất bại vì nó lưu trữ cùng một thông tin riêng biệt cho mỗi (k), mặc dù các giá trị cho (k) liên tiếp có cấu trúc mạnh. 

Quan sát quan trọng là DP của mỗi cây con dưới dạng hàm của (k) có mức tăng cận biên giảm dần. Xác định 

[ 
D_u(k)=F_u(k)-F_u(k-1). 
] 

Những khác biệt này không tăng. Do đó, chúng ta có thể biểu diễn toàn bộ hàm (F_u) bằng bội số mức tăng cận biên của nó thay vì lưu trữ mọi giá trị DP. 

Giả sử các phần tử con của (u) có các dãy cận biên (A,B,\ldots). Khoảng cách của chúng không khớp nhau, vì vậy tất cả trẻ em có thể sử dụng cùng một lớp (k) một cách độc lập. Nếu lợi ích cận biên của hai đứa trẻ là 

[ 
a_1\ge a_2\ge\cdots 
] 

và 

[ 
b_1\ge b_2\ge\cdots, 
] 

thì cây con kết hợp có lợi ích cận biên 

[ 
a_1+b_1,\ a_2+b_2,\ldots. 
] 

Sau khi kết hợp tất cả các phần tử con, hãy xem xét bản thân cây cầu (u) có giá trị (w_u). Việc chọn nó sẽ tiêu tốn một lớp, do đó DP thu được tương đương với việc chèn thêm một mức tăng biên (w_u) vào chuỗi biên đã được sắp xếp. 

Đây chính xác là cấu trúc được mô tả bởi giải pháp chính thức: các hàm con được kết hợp bằng cách cộng từng cặp các đạo hàm đã sắp xếp của chúng và việc thêm một nút tương ứng với việc chèn trọng số của nó vào tập đạo hàm đó. 

Vùng heap tối đa lưu trữ lợi nhuận cận biên. Để kết hợp hai đống con, hãy liên tục loại bỏ các phần tử lớn nhất của chúng, cộng hai giá trị đó và đặt giá trị thu được vào đống cha. Chúng tôi luôn hợp nhất đống nhỏ hơn thành đống lớn hơn. Đây là kỹ thuật từ nhỏ đến lớn tiêu chuẩn và phân tích chính thức đưa ra giới hạn (O(N\log N)) cho toàn bộ quá trình heap. 

Gốc ảo cuối cùng có giá trị bằng 0 và chứa mọi cây cầu. Đống của nó thể hiện sự cải thiện biên thu được bằng cách tăng (k). Chúng tôi liên tục lấy phần lãi cận biên lớn nhất còn lại và tích lũy nó. Khi vùng heap trở nên trống, câu trả lời đã đạt đến mức tối đa và không thay đổi đối với tất cả (k) lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cây ngây thơ DP | (O(N^2)) | (O(N^2)) hoặc (O(N)) có tính toán lại | Quá chậm | 
| DP đống tối ưu | (O(N\log N)) | (O(N)) các mục heap đang hoạt động cộng với cây | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Biểu thị mỗi cây cầu theo khoảng hiển thị của nó ([S_i,E_i)). Bởi vì những cây cầu không bao giờ cắt nhau nên hai khoảng thời gian bất kỳ như vậy sẽ rời rạc hoặc lồng vào nhau. Điều này biến bài toán hình học thành bài toán khoảng tầng. 
2. Thêm một khoảng ảo chứa toàn bộ phạm vi tọa độ và đặt giá trị bằng 0. Nút ảo này trở thành nút gốc, cho phép một số cầu nối rời rạc cấp cao nhất cùng tồn tại trong một cây. 
3. Sắp xếp các khoảng thực bằng cách tăng điểm cuối bên trái, phá vỡ mối liên kết bằng cách giảm điểm cuối bên phải. Đây là thứ tự đặt trước của cây ngăn chặn. Khi điểm cuối bên trái bằng nhau, khoảng lớn hơn phải xuất hiện đầu tiên vì nó là điểm tổ tiên. 
4. Quét các khoảng đã sắp xếp bằng một ngăn xếp. Ngăn xếp chứa đường dẫn hiện tại từ gốc ảo đến khoảng thời gian được xử lý gần đây nhất. Đối với một khoảng mới ([S,E)), các khoảng pop có điểm cuối bên phải nhỏ hơn (E). Khoảng còn lại ở trên chính xác là khoảng nhỏ nhất chứa khoảng mới, do đó nó trở thành khoảng chính của khoảng mới. 
5. Xử lý cây từ dưới lên. Đối với mỗi nút (u), hãy duy trì một vùng heap tối đa chứa mức tăng cận biên của hàm DP của nó. Một lá bắt đầu bằng một giá trị duy nhất (w_u). 
6. Đối với một nút nội bộ, trước tiên hãy kết hợp các đống của tất cả các nút con của nó. Nếu hai đống chứa các chuỗi cận biên (A) và (B), hãy loại bỏ các phần tử lớn nhất của chúng theo cặp và chèn tổng của chúng. Điều này tính toán chuỗi cận biên của (F_A(k)+F_B(k)), vì cả hai cây con độc lập đều nhận được cùng một dung lượng (k). 
7. Luôn sử dụng vùng nhớ lớn hơn làm vùng nhớ đích. Nếu heap hiện tại có ít phần tử hơn heap của con khác, hãy hoán đổi chúng trước khi hợp nhất. Chỉ các phần tử từ vùng heap nhỏ hơn cần được loại bỏ, điều này mang lại giới hạn từ nhỏ đến lớn. 
8. Sau khi tất cả các phần tử con đã được kết hợp, hãy chèn (w_u) vào heap. Điều này thể hiện sự lựa chọn chọn (u), tiêu tốn thêm một mức lồng nhau và đóng góp chính xác (w_u) vào mức tăng biên tương ứng. 
9. Tại gốc ảo, liên tục loại bỏ mức tăng cận biên lớn nhất. Thêm nó vào câu trả lời đang chạy và in câu trả lời đang chạy đó cho giá trị tiếp theo của (k). Nếu vùng heap trống, hãy tiếp tục in câu trả lời tương tự vì không có cây cầu nào khác có thể cải thiện giải pháp. 

Tại sao nó hoạt động: đối với mỗi nút (u), tính bất biến của vùng heap là các phần tử của nó chính xác là mức tăng biên (F_u(k)-F_u(k-1)), được sắp xếp theo thứ tự không tăng. Các cây con độc lập thêm các hàm DP của chúng, do đó lợi ích cận biên của chúng sẽ tăng thêm từng vị trí. Việc thêm (u) sẽ thay đổi DP từ (G(k)) thành (\max(G(k),w_u+G(k-1))), đây chính xác là thao tác chèn (w_u) vào mức tăng cận biên đã sắp xếp của (G). Do đó, bất biến giữ quy nạp từ lá đến gốc. Ở gốc, việc lấy (k) mức tăng biên lớn nhất sẽ mang lại (F_{\text{root}}(k)), chính xác là tổng giá trị thẩm mỹ tối đa theo công suất (k). 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve(stream=None):
    if stream is None:
        stream = sys.stdin

    read = stream.readline
    n = int(read())

    intervals = []
    for _ in range(n):
        s, e, w = map(int, read().split())
        intervals.append((s, e, w))

    # Increasing left endpoint, decreasing right endpoint.
    intervals.sort(key=lambda x: (x[0], -x[1]))

    # Node 0 is the virtual root.
    # Its interval contains every real interval and its value is zero.
    end = [1_000_001] + [0] * n
    weight = [0] * (n + 1)

    parent = [0] * (n + 1)

    # Children are stored as linked lists to avoid creating
    # N separate Python list objects.
    head = [-1] * (n + 1)
    nxt = [-1] * (n + 1)

    stack = [0]

    for u, (s, e, w) in enumerate(intervals, 1):
        end[u] = e
        weight[u] = w

        # Since left endpoints are processed increasingly,
        # containment is determined by the right endpoint here.
        while e > end[stack[-1]]:
            stack.pop()

        p = stack[-1]
        parent[u] = p

        nxt[u] = head[p]
        head[p] = u

        stack.append(u)

    # Each heap stores negative marginal gains, so heapq acts
    # as a max-heap on the original positive values.
    heaps = [None] * (n + 1)

    # Parent IDs are always smaller than child IDs because the
    # intervals were processed in preorder.
    for u in range(n, -1, -1):
        h = None
        child = head[u]

        while child != -1:
            other = heaps[child]

            if h is None:
                h = other
            else:
                # Always merge the smaller heap into the larger heap.
                if len(other) > len(h):
                    h, other = other, h

                m = len(other)

                # Pair the largest marginal gains.
                merged = [
                    heapq.heappop(h) + heapq.heappop(other)
                    for _ in range(m)
                ]

                # Rebuild once instead of performing m separate pushes.
                h.extend(merged)
                heapq.heapify(h)

            # The child heap has now been completely consumed.
            heaps[child] = None
            child = nxt[child]

        if h is None:
            h = []

        # Selecting u contributes one new marginal gain w[u].
        heapq.heappush(h, -weight[u])
        heaps[u] = h

    # Extract marginal gains from the virtual root.
    h = heaps[0]
    answer = 0
    result = []

    for _ in range(n):
        if h:
            answer -= heapq.heappop(h)
        result.append(str(answer))

    return " ".join(result)

if __name__ == "__main__":
    sys.stdout.write(solve() + "\n")
```Các khoảng đầu vào được sắp xếp trước khi cây được xây dựng. Người phá vỡ mối ràng buộc là`-x[1]`, vì vậy nếu hai cầu bắt đầu tại cùng một điểm, khoảng lớn hơn sẽ được xử lý trước và có thể trở thành tổ tiên của cầu nhỏ hơn. 

Điều kiện ngăn xếp sử dụng`e > end[stack[-1]]`, không`>=`. Cho phép các điểm cuối bên phải bằng nhau vì một khoảng có thể chứa một khoảng khác trong khi chia sẻ điểm cuối bên phải của nó. Khoảng thời gian hiển thị là nửa mở, do đó, các cầu nối kết thúc tại một điểm và các cầu nối bắt đầu tại cùng một điểm đó sẽ rời rạc, điều này sẽ được ngăn xếp xử lý một cách tự nhiên sau khi khoảng thời gian trước đó được bật lên. 

Cây được lưu trữ bằng cách sử dụng`head`Và`nxt`thay vì danh sách cho mỗi nút. Điều này làm giảm đáng kể chi phí hoạt động của đối tượng Python khi (N) lớn. Bởi vì các khoảng được sắp xếp tạo thành một thứ tự trước, mỗi nút cha có chỉ số nút nhỏ hơn mọi nút con, do đó, việc duyệt số ngược là đủ cho DP từ dưới lên và tránh các vấn đề về độ sâu đệ quy. 

Heap lưu trữ các giá trị âm vì Python`heapq`là một đống tối thiểu. Do đó, mức tăng biên ban đầu lớn nhất là giá trị được lưu trữ nhỏ nhất. Trong quá trình hợp nhất con, các phần tử lớn nhất sẽ bị xóa khỏi cả hai đống và các biểu diễn phủ định của chúng sẽ được thêm vào. Nếu mức tăng ban đầu là (a) và (b), thì các giá trị được lưu trữ là (-a) và (-b) và tổng của chúng là (-(a+b)), chính xác là những gì mà đống được hợp nhất cần. 

các`merged`danh sách được xây dựng đầu tiên và sau đó được thêm vào vùng đích, theo sau là một danh sách`heapify`. Liên tục gọi`heappush`sẽ thực hiện công việc logarit không cần thiết cho mọi giá trị được hợp nhất mới. Số nguyên Python có độ chính xác tùy ý, do đó, tổng giá trị thẩm mỹ có thể đạt tới (250.000\cdot10^9=2,5\cdot10^{14}), không yêu cầu xử lý tràn đặc biệt. 

Việc trích xuất cuối cùng có chủ ý tiếp tục sau khi vùng heap trở nên trống rỗng. Thiếu mức tăng cận biên có nghĩa là việc tăng (k) hơn nữa không thể cải thiện câu trả lời, do đó tổng số hoạt động đơn giản là không thay đổi. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, cây ngăn chặn có hai nhánh chính. Phần đầu tiên chứa các khoảng ([1,2)), ([2,3)) và phần tử gốc của chúng ([1,3)). Cái thứ hai chứa ([3,4)), ([4,5)) và cha mẹ của chúng ([3,5)). 

| Nút đã xử lý | Trình tự biên con | Sau khi kết hợp trẻ em | Sau khi chèn giá trị nút | 
| --- | --- | --- | --- | 
| ([1,2),10) | không |`[]`|`[10]`| 
| ([2,3),10) | không |`[]`|`[10]`| 
| ([1,3),21) |`[10]`,`[10]`|`[20]`|`[21,20]`| 
| ([3,4),10) | không |`[]`|`[10]`| 
| ([4,5),10) | không |`[]`|`[10]`| 
| ([3,5),19) |`[10]`,`[10]`|`[20]`|`[20,19]`| 
| gốc ảo, 0 |`[21,20]`,`[20,19]`|`[41,39]`|`[41,39,0]`| 

Mức tăng cận biên đầu tiên là (41), vì vậy (k=1) cho (41). Mức tăng cận biên thứ hai là (39), vì vậy (k=2) cho (80). Mức tăng cận biên còn lại bằng 0, vì vậy mỗi (k) lớn hơn cũng cho (80). Điều này tạo ra đầu ra mẫu`41 80 80 80 80 80`. 

Đối với Mẫu 2, mỗi khoảng chứa khoảng tiếp theo: 

[ 
[1,5)\supset[2,5)\supset[3,5)\supset[4,5). 
] 

Mọi cây cầu đều có giá trị (1). 

| Nút đã xử lý | Trình tự biên con | Sau khi chèn giá trị 1 | 
| --- | --- | --- | 
| ([4,5),1) |`[]`|`[1]`| 
| ([3,5),1) |`[1]`|`[1,1]`| 
| ([2,5),1) |`[1,1]`|`[1,1,1]`| 
| ([1,5),1) |`[1,1,1]`|`[1,1,1,1]`| 
| gốc ảo, 0 |`[1,1,1,1]`|`[1,1,1,1,0]`| 

Bốn lợi ích cận biên dương đầu tiên đều là (1). Do đó câu trả lời là (1,2,3,4). Số 0 do gốc ảo đóng góp không làm thay đổi câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Chi phí sắp xếp (O(N\log N)) và quá trình xử lý heap từ nhỏ đến lớn là (O(N\log N)) tổng thể | 
| Không gian | (O(N)) | Các mảng khoảng, cây ngăn chặn và các mục nhập vùng heap đang hoạt động chứa các phần tử (O(N)) | 

Giới hạn cầu (250.000) khiến DP bậc hai không thể thực hiện được, trong khi (O(N\log N)) phù hợp với kích thước đầu vào. Giới hạn tọa độ của (10^6) không bao giờ xuất hiện dưới dạng hệ số nhân vì thuật toán chỉ hoạt động với các cầu được cung cấp và các mối quan hệ ngăn chặn của chúng. Biểu diễn số nguyên có độ chính xác tùy ý trong Python cũng đủ cho tổng giá trị tối đa có thể. 

## Trường hợp thử nghiệm```
# The solution is written so solve(stream) can be tested directly.
import io

def run(inp: str) -> str:
    return solve(io.StringIO(inp))

# Provided sample 1
assert run(
    """6
1 2 10
2 3 10
1 3 21
3 4 10
4 5 10
3 5 19
"""
) == "41 80 80 80 80 80", "sample 1"

# Provided sample 2
assert run(
    """4
1 5 1
2 5 1
3 5 1
4 5 1
"""
) == "1 2 3 4", "sample 2"

# Minimum-size input.
assert run(
    """1
1 2 7
"""
) == "7", "single bridge"

# Three disjoint bridges. They can all be selected already for k = 1.
assert run(
    """3
1 2 5
2 3 7
4 5 3
"""
) == "15 15 15", "disjoint intervals"

# A pure nesting chain.
assert run(
    """3
1 6 10
2 5 20
3 4 30
"""
) == "30 50 60", "nested chain"

# Endpoint touching plus an inner interval.
# [1,2) and [2,5) are disjoint, while [3,4) is nested in [2,5).
assert run(
    """3
1 2 5
2 5 100
3 4 7
"""
) == "105 112 112", "endpoint boundary"

# Same left endpoint, forcing the decreasing-right-endpoint tie breaker.
assert run(
    """3
1 5 10
1 4 20
2 3 30
"""
) == "30 50 60", "equal left endpoints"

# Maximum-size stress case.
# All 250000 intervals are pairwise disjoint and have value 1,
# so every answer is exactly 250000.
n = 250000
stress_input = str(n) + "\n" + "".join(
    f"{2 * i - 1} {2 * i} 1\n" for i in range(1, n + 1)
)
expected = " ".join(["250000"] * n)

assert run(stress_input) == expected, "maximum-size disjoint input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 2 7`|`7`| Tối thiểu (N), cây nút đơn và xử lý gốc | 
| Ba khoảng chạm hoặc rời nhau |`15 15 15`| Khoảng thời gian hiển thị nửa mở và cây con anh em độc lập | 
|`1 6`,`2 5`,`3 4`|`30 50 60`| Lồng ghép thuần túy và lợi ích cận biên liên tiếp | 
|`1 2`,`2 5`,`3 4`|`105 112 112`| Chạm điểm cuối kết hợp với lồng nhau | 
|`1 5`,`1 4`,`2 3`|`30 50 60`| Điểm cuối bên trái bằng nhau và sắp xếp điểm cuối bên phải giảm dần | 
| 250000 khoảng đơn vị rời rạc |`250000`lặp đi lặp lại 250000 lần | Tối đa (N), đầu ra lớn, khả năng mở rộng heap và cây | 

## Vỏ cạnh 

Trường hợp cầu đơn được xử lý bằng cách tạo một đống lá chứa giá trị của nó. Vì```
1
1 2 7
```đống cầu là`[7]`, gốc ảo kết hợp đống đó với không có phần tử con nào khác và chèn số 0 của chính nó. Lần trích xuất đầu tiên thêm (7), cho kết quả đầu ra cần thiết`7`. 

Đối với khoảng thời gian chạm,```
2
1 2 5
2 3 7
```cây cầu đầu tiên tương ứng với ([1,2)), trong khi cây cầu thứ hai tương ứng với ([2,3)). Khi khoảng thứ hai được xử lý, khoảng thứ nhất không còn chứa nó nữa, do đó ngăn xếp trở về gốc ảo. Họ trở thành anh em ruột. Đống biên một phần tử của chúng được kết hợp thành (5+7=12), mang lại mức tăng biên dương duy nhất là (12). Đầu ra là`12 12`. Việc coi các khoảng là đóng sẽ không chính xác làm cho các cầu nối này xung đột với (k=1). 

Đối với một chuỗi lồng nhau,```
3
1 6 10
2 5 20
3 4 30
```cái cây là một con đường. Cây cầu trong cùng bắt đầu bằng chuỗi cận biên`[30]`. Cha mẹ của nó chèn (20), tạo ra`[30,20]`, và cầu nối bên ngoài chèn (10), tạo ra`[30,20,10]`. Gốc thêm số không. Việc trích xuất những lợi ích này mang lại`30`, sau đó`50`, sau đó`60`. Đây chính xác là mức tối ưu vì mỗi đơn vị chồng chéo được phép bổ sung cho phép chúng ta chọn thêm một cây cầu trong chuỗi. 

Đối với các điểm cuối bên trái bằng nhau,```
3
1 5 10
1 4 20
2 3 30
```sắp xếp theo`(left, -right)`vị trí ([1,5)) trước ([1,4)). Do đó, ngăn xếp xác định chính xác chuỗi ngăn chặn. Nếu bộ ngắt kết nối bị đảo ngược, ([1,4)) có thể bị coi là không liên quan đến tổ tiên thực sự của nó một cách không chính xác. Chuỗi cận biên kết quả là`[30,20,10]`, cho`30 50 60`. 

Đối với trường hợp rời rạc có kích thước tối đa, mỗi cây cầu sẽ trở thành một con riêng biệt của gốc ảo. Mỗi đứa trẻ đóng góp một lợi ích cận biên là (1). Vì các phần tử con rời rạc nên các giá trị của chúng được kết hợp thành một mức tăng biên duy nhất là (250000). Không có lợi ích gì từ việc tăng (k), vì vậy mỗi một trong số (250000) câu trả lời đều là`250000`. Trường hợp này cũng thực hiện việc hợp nhất heap từ nhỏ đến lớn và xác nhận rằng việc triển khai không phụ thuộc vào phạm vi tọa độ gần với (N). 

Ý tưởng trung tâm để giải quyết các vấn đề tương tự là biểu diễn mức lợi cận biên: một khi ràng buộc tầng trở thành một cây, toàn bộ DP k chiều có thể thu gọn thành một tập hợp các mức lợi nhuận theo thứ tự trên mỗi cây con.
