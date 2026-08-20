---
title: "CF 102190I - đầu vào/đầu ra tiêu chuẩn"
description: "Chúng ta có (n) điểm và ma trận khoảng cách (n lần n). Một số mục đã chứa giá trị cuối cùng của chúng, trong khi mọi (-1) biểu thị khoảng cách mà chúng ta có thể tự do lựa chọn."
date: "2026-08-20T00:44:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "I"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 133
verified: true
draft: false
---

[CF 102190I - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) điểm và ma trận khoảng cách (n \times n). Một số mục đã chứa giá trị cuối cùng của chúng, trong khi mọi (-1) biểu thị khoảng cách mà chúng ta có thể tự do lựa chọn. Ma trận cuối cùng phải thỏa mãn bốn thuộc tính bắt buộc: mọi mục nhập đường chéo đều bằng 0, mọi khoảng cách đều không âm, ma trận đối xứng và mọi khoảng cách trực tiếp đều tuân theo bất đẳng thức tam giác. 

Hạn chế đối với các mục hiện có là nghiêm ngặt. Nếu đầu vào nói (d(i,j)=7), câu trả lời cũng phải chứa (7) tại vị trí đó. Mặt khác, một mục không xác định có thể nhận bất kỳ giá trị nào từ (0) đến (10^9). Câu lệnh cho phép số 0 giữa các điểm khác nhau, vì vậy đây là một phép đo giả chứ không phải là quy ước chặt chẽ hơn trong đó các điểm khác nhau phải có khoảng cách dương. 

Ma trận lớn nhất có (500^2=250000) mục nhập và tổng (n) trên tất cả các trường hợp thử nghiệm nhiều nhất là (500). Điều này làm cho thuật toán (O(n^3)) trở thành mục tiêu tự nhiên. Với (n=500), khối công việc là khoảng (125) triệu lần lặp cơ bản, trong khi (O(n^4)) hoặc tệ hơn sẽ trở nên tốn kém một cách không cần thiết. Tổng nhỏ của (n) cũng ngăn cản nhiều trường hợp thử nghiệm lớn nhân chi phí khối. 

Có một số trường hợp chiến lược lấp đầy đơn giản có thể âm thầm thất bại. Một giá trị đường chéo đã biết như```
2
1 -1
-1 0
```phải sản xuất`NO`, vì điểm đầu tiên bắt buộc phải có khoảng cách bằng 0 với chính nó. Một thuật toán bất cẩn chỉ xử lý các mục không theo đường chéo có thể bỏ qua mâu thuẫn này. 

Tính đối xứng cũng có thể bị vi phạm trực tiếp. Ví dụ,```
2
0 3
4 0
```là không thể vì hai cách biểu diễn có cùng khoảng cách không giống nhau. Chỉ điền vào các mục (-1) sẽ không sửa được lỗi này vì cả giá trị hiện tại đều không thể thay đổi. 

Một mâu thuẫn tinh tế hơn đến từ một con đường ngắn hơn được biết đến. Coi như```
3
0 5 2
5 0 2
2 2 0
```Khoảng cách trực tiếp giữa các điểm (1) và (2) được cố định tại (5), nhưng tuyến đường (1 \rightarrow 3 \rightarrow 2) có chiều dài (4). Bất kỳ số liệu nào cũng phải thỏa mãn (d(1,2)\le4), vì vậy giá trị cố định (5) khiến cho trường hợp này không thể thực hiện được. Chỉ kiểm tra các hình tam giác hiện diện rõ ràng trong đầu vào là không đủ đối với các đường dẫn lớn hơn thuộc loại này. 

Cuối cùng, đồ thị khoảng cách đã biết có thể bị ngắt kết nối. Ví dụ,```
3
0 2 -1
2 0 -1
-1 -1 0
```là hoàn toàn có thể hoàn thành. Hai điểm đầu tiên tạo thành một thành phần và điểm thứ ba tạo thành một thành phần khác. Chúng ta có thể chọn khoảng cách đủ lớn giữa các thành phần. Một giải pháp giả sử mỗi cặp đều có đường đi ngắn nhất hữu hạn sẽ bác bỏ trường hợp này một cách không chính xác. 

Mẫu cuộc thi chính thức chứa bốn trường hợp thử nghiệm, bao gồm các tình huống bị ngắt kết nối và được chỉ định một phần được mô tả ở trên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mọi khoảng cách không xác định là một biến và thử các giá trị có thể cho đến khi tìm thấy số liệu hoàn chỉnh. Ngay cả sau khi khai thác tính đối xứng, một cá thể (n=500) vẫn có thể có 

[ 
\frac{500\cdot499}{2}=124750 
] 

các cặp không có thứ tự chưa biết. Vì giá trị đầu ra có thể là số nguyên bất kỳ từ (0) đến (10^9), nên việc liệt kê đầy đủ có 

[ 
(10^9+1)^{124750} 
] 

các nhiệm vụ có thể thực hiện được trong trường hợp xấu nhất. Việc kiểm tra từng ứng cử viên sẽ yêu cầu ít nhất (O(n^3)) công việc để xác minh tất cả các bất đẳng thức tam giác. Điều này không chỉ đơn thuần là quá chậm mà còn không khả thi về mặt tính toán. 

Ý tưởng về vũ lực có chứa điểm khởi đầu đúng đắn về mặt khái niệm: mọi khoảng cách đã biết đều có thể được coi là một ràng buộc không thể thay đổi. Điều quan trọng là ngừng coi các mục còn thiếu là các biến độc lập. 

Hãy coi mọi khoảng cách đã biết như một cạnh có trọng số vô hướng. Khi biểu đồ này được tạo, mọi số liệu hợp lệ đều phải đáp ứng 

[ 
d(u,v)\le d(u,x_1)+d(x_1,x_2)+\cdots+d(x_k,v) 
] 

với mọi đường đi giữa (u) và (v). Do đó, một cạnh cố định của trọng số (w) chỉ có thể tồn tại nếu không có đường đi nào được biết giữa các điểm cuối của nó có tổng trọng số nhỏ hơn (w). 

Số liệu đường đi ngắn nhất của biểu đồ đã biết cung cấp chính xác khoảng cách mạnh nhất bị ràng buộc bởi các ràng buộc hiện có. Nếu một cạnh cố định dài hơn đường đi ngắn nhất đã biết của nó thì trường hợp đó là không thể. Nếu không có cạnh cố định nào trở nên ngắn hơn thì bản thân khoảng cách đường đi ngắn nhất sẽ là sự hoàn thành hợp lệ bên trong mọi thành phần được kết nối. 

Đây chính xác là những gì Floyd-Warshall tính toán. Khởi tạo ma trận với khoảng cách đã biết và vô cực cho các mục bị thiếu, sau đó tính toán các đường đi ngắn nhất cho tất cả các cặp. Sau đó, mọi khoảng cách đã biết phải bằng khoảng cách đường đi ngắn nhất tương ứng. 

Các thành phần bị ngắt kết nối yêu cầu một chi tiết cuối cùng. Không có đường đi giữa hai thành phần khác nhau, vì vậy khoảng cách của chúng vẫn là vô cùng sau Floyd-Warshall. Chúng ta có thể thay mọi số vô hạn như vậy bằng (10^9). Việc chọn một giá trị chung đủ lớn giữa các thành phần sẽ duy trì sự bất đẳng thức tam giác và nằm trong phạm vi đầu ra được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta((10^9+1)^{\Theta(n^2)})) ứng viên | (O(n^2)) | Quá chậm | 
| Tối ưu | (O(n^3)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận và ghi nhớ những mục ban đầu đã được sửa. Chỉ đặt đường chéo làm việc về 0 nếu đường chéo đầu vào đã bằng 0. Nếu một mục nhập đường chéo đã biết là bất kỳ mục nào khác, hãy từ chối ngay lập tức vì không số liệu nào có thể thay đổi nó. 
2. Kiểm tra từng cặp (i,j) xem có đối xứng không. Nếu cả hai (d(i,j)) và (d(j,i)) đều đã biết thì chúng phải bằng nhau. Nếu chỉ biết một hướng, hãy sao chép giá trị đó sang hướng khác trong ma trận làm việc. Điều này hợp lệ vì mục bị thiếu chưa có hạn chế nào, trong khi tính đối xứng buộc nó phải bằng mục đã biết. 
3. Giải thích mọi khoảng cách ngoài đường chéo đã biết là cạnh có trọng số vô hướng. Khởi tạo khoảng cách còn thiếu tới`INF`, Ở đâu`INF`lớn hơn nhiều so với mọi khoảng cách đường đi ngắn nhất hữu hạn có thể có. 
4. Chạy Floyd-Warshall. Đối với mỗi đỉnh trung gian (k), hãy cố gắng cải thiện mọi cặp (i,j) đến (k) bằng cách sử dụng 

[ 
d(i,j)=\min(d(i,j),d(i,k)+d(k,j)). 
] 

Ma trận kết quả chứa đường đi ngắn nhất chỉ sử dụng các khoảng cách đã biết. 

1. Kiểm tra mọi khoảng cách cố định ban đầu dựa trên ma trận đường đi ngắn nhất. Nếu giá trị ban đầu (w) trở nên nhỏ hơn (w), cạnh cố định sẽ dài hơn đường đi bắt buộc và không thể tồn tại sự hoàn thành. Từ chối trường hợp thử nghiệm. 
2. Thay thế mọi thứ còn lại`INF`nhập bằng (10^9). Đây chính xác là các cặp nằm trong các thành phần được kết nối khác nhau của biểu đồ khoảng cách đã biết. Một giá trị chung lớn có thể kết nối các thành phần khác nhau một cách an toàn vì mọi khoảng cách bên trong hữu hạn đều nhỏ hơn nhiều so với (10^9). 
3. Xuất ma trận kết quả. Mọi mục nhập hữu hạn do Floyd-Warshall tạo ra đều là khoảng cách đường đi ngắn nhất, trong khi mọi mục nhập chéo thành phần là giá trị lớn chung. Đường chéo vẫn bằng không. 

**Tại sao nó hoạt động.** Bất biến trung tâm là sau Floyd-Warshall, mọi giá trị hữu hạn (d(i,j)) là độ dài tối thiểu của một đường đi được hình thành hoàn toàn từ các khoảng cách đã biết ban đầu. Bất kỳ sự hoàn thành hợp lệ nào cũng phải thỏa mãn bất đẳng thức tam giác dọc theo mọi đường dẫn như vậy, do đó nó phải có (d(i,j)) không lớn hơn giá trị đường đi ngắn nhất đó. Đồng thời, nếu một cạnh cố định ban đầu có chính xác giá trị đường đi ngắn nhất của nó thì việc giữ giá trị đó sẽ tương thích với mọi đường đi đã biết. Do đó, một giá trị cố định nhỏ hơn mọi đường dẫn đã biết là an toàn, trong khi giá trị cố định lớn hơn một số đường dẫn đã biết là không thể. Khi tất cả các mục cố định vượt qua thử nghiệm này, khoảng cách đường đi ngắn nhất sẽ thỏa mãn bất đẳng thức tam giác bằng cách xây dựng. Các thành phần được kết nối khác nhau không có ràng buộc nào giữa chúng và việc gán (10^9) cho mọi cặp thành phần chéo sẽ tạo ra các bất đẳng thức tam giác hợp lệ vì tất cả các khoảng cách hữu hạn bên trong đều nhỏ hơn (10^9). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**15
BIG = 10**9

def solve_case(a):
    n = len(a)

    # Build a symmetric working matrix.
    dist = [[INF] * n for _ in range(n)]
    fixed = []

    for i in range(n):
        if a[i][i] != -1 and a[i][i] != 0:
            return None

        dist[i][i] = 0

    for i in range(n):
        for j in range(i + 1, n):
            x = a[i][j]
            y = a[j][i]

            if x != -1 and y != -1:
                if x != y:
                    return None
                w = x
                fixed.append((i, j, w))
                dist[i][j] = w
                dist[j][i] = w
            elif x != -1:
                fixed.append((i, j, x))
                dist[i][j] = x
                dist[j][i] = x
            elif y != -1:
                fixed.append((i, j, y))
                dist[i][j] = y
                dist[j][i] = y

    # Floyd-Warshall.
    rng = range(n)
    for k in rng:
        dk = dist[k]
        for i in rng:
            di = dist[i]
            dik = di[k]
            if dik == INF:
                continue

            for j in rng:
                nd = dik + dk[j]
                if nd < di[j]:
                    di[j] = nd

    # Every fixed edge must still have exactly its original value.
    for u, v, w in fixed:
        if dist[u][v] != w:
            return None

    # Connect different components with one sufficiently large value.
    for i in rng:
        di = dist[i]
        for j in rng:
            if di[j] == INF:
                di[j] = BIG

    return dist

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = [list(map(int, input().split())) for _ in range(n)]

        ans = solve_case(a)

        if ans is None:
            out.append("NO")
            continue

        out.append("YES")
        for row in ans:
            out.append(" ".join(map(str, row)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn xây dựng đầu tiên có chủ ý tách ma trận gốc khỏi ma trận làm việc. các`fixed`list ghi lại mọi cặp không có thứ tự đã biết, để sau này chúng ta có thể phân biệt một cạnh được phép thay đổi với một cạnh được yêu cầu không thay đổi. 

Việc kiểm tra tính đối xứng được thực hiện trước Floyd-Warshall. Nếu chỉ biết một mặt của một cặp, việc sao chép nó sang mặt kia sẽ không sửa đổi một giá trị cố định. Nó chỉ đơn giản là gán giá trị còn thiếu mà lực đối xứng tạo ra. Nếu cả hai bên đều có mặt và khác nhau thì không thể có câu trả lời. 

Đường chéo nhận được sự xử lý đặc biệt vì không có lý do gì để đưa một đường tự lặp vào biểu đồ. Một số 0 đã biết nhất quán với mọi số liệu, trong khi bất kỳ giá trị đường chéo đã biết nào khác ngay lập tức khiến trường hợp này không thể thực hiện được.`INF`cố tình lớn hơn nhiều so với giới hạn đầu ra. Một thành phần được kết nối chứa tối đa 500 đỉnh có một đường đi đơn giản với tối đa 499 cạnh và mỗi cạnh ban đầu có nhiều nhất là 1000, do đó, bất kỳ đường đi ngắn nhất hữu hạn nào cũng có nhiều nhất là (499000). Người được chọn`INF`do đó không thể truy cập một cách an toàn bằng khoảng cách hợp pháp. Số nguyên Python cũng có độ chính xác tùy ý nên không có vấn đề tràn. 

Vòng lặp Floyd-Warshall bỏ qua một hàng khi`dist[i][k]`là vô hạn. Điều này quan trọng đối với các biểu đồ bị ngắt kết nối, trong đó nhiều cặp có thể không truy cập được trong suốt quá trình tính toán. Các biến cục bộ`dk`Và`di`cũng tránh lập chỉ mục lặp lại của danh sách hai chiều. 

Sau Floyd-Warshall, chỉ kiểm tra các mục đã biết của đầu vào là đủ. Các mục không xác định được phép lấy các giá trị đường dẫn ngắn nhất, do đó chúng không áp đặt hạn chế bổ sung. Nếu một cạnh cố định đã bị giảm đi thì việc giảm đó biểu thị một đường đi đã biết vi phạm các bất đẳng thức tam giác cần thiết. 

Cuối cùng, tất cả các cặp không thể truy cập được sẽ được chỉ định`BIG`. Giá trị chính xác là (10^9), được giới hạn đầu ra cho phép. Nó lớn hơn mọi khoảng cách hữu hạn có thể được tạo ra từ các ràng buộc ban đầu, do đó, khoảng cách giữa các thành phần không thể đưa ra một tuyến đường mới ngắn hơn có thể làm thay đổi khoảng cách nội bộ cố định. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên đã là một số liệu hoàn chỉnh:```
3
0 3 3
3 0 3
3 3 0
```Trạng thái chính trong Floyd-Warshall không thay đổi. 

| Trung cấp (k) | (d(1,2)) | (d(1,3)) | (d(2,3)) | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 3 | 3 | Tất cả đã được sửa | 
| 1 | 3 | 3 | 3 | Không cải thiện | 
| 2 | 3 | 3 | 3 | Không cải thiện | 
| 3 | 3 | 3 | 3 | Không cải thiện | 

Mọi giá trị cố định đều bằng khoảng cách đường đi ngắn nhất của nó, vì vậy câu trả lời là`YES`theo sau là cùng một ma trận. 

Ví dụ này chứng minh rằng Floyd-Warshall không cố gắng thay đổi các mục cố định một cách tùy tiện. Nó chỉ tiết lộ liệu một tập hợp các cạnh cố định khác có buộc chúng trở nên nhỏ hơn hay không. 

### Mẫu 2 

Mẫu thứ hai là```
3
0 0 0
0 0 -1
0 -1 0
```Cặp bị thiếu bị ép về 0 bởi tính đối xứng và khoảng cách 0 đã biết. 

| Sân khấu | (d(1,2)) | (d(1,3)) | (d(2,3)) | Tiểu bang | 
| --- | --- | --- | --- | --- | 
| Đầu vào | 0 | 0 | -1 | Thiếu một hướng | 
| Hoàn thành đối xứng | 0 | 0 | 0 | Giá trị thiếu trở thành 0 | 
| (k=1) | 0 | 0 | 0 | Không cải thiện | 
| (k=2) | 0 | 0 | 0 | Không cải thiện | 
| (k=3) | 0 | 0 | 0 | Không cải thiện | 

Ma trận cuối cùng đều là số không. 

Trường hợp này thực hiện phần không chuẩn của phát biểu: các điểm phân biệt được phép có khoảng cách bằng 0. Việc triển khai giả định tính tích cực nghiêm ngặt đối với các điểm khác biệt sẽ từ chối đầu vào hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3)) | Floyd-Warshall kiểm tra tất cả các bộ ba đỉnh | 
| Không gian | (O(n^2)) | Ma trận khoảng cách và thông tin cạnh cố định ban đầu chiếm không gian bậc hai | 

Với (n\le500), giới hạn bậc ba là thang đo dự kiến ​​cho bài toán này và tổng tổng của (n) trong các trường hợp thử nghiệm cũng nhiều nhất là 500. Ma trận chỉ yêu cầu tối đa (250000) giá trị khoảng cách, do đó việc sử dụng bộ nhớ bậc hai có thể dễ dàng quản lý được. 

## Trường hợp thử nghiệm 

Đầu ra của một vấn đề mang tính xây dựng không nhất thiết phải là duy nhất, do đó, một khai thác kiểm thử mạnh mẽ sẽ xác thực ma trận được trả về thay vì so sánh mọi kết quả.`YES`trường hợp thành một ma trận chính xác. Các mẫu chính thức nhỏ tình cờ phù hợp với cấu trúc xác định bên dưới.```python
import sys
import io

INF = 10**15
BIG = 10**9

def solve_case(a):
    n = len(a)
    dist = [[INF] * n for _ in range(n)]
    fixed = []

    for i in range(n):
        if a[i][i] != -1 and a[i][i] != 0:
            return None
        dist[i][i] = 0

    for i in range(n):
        for j in range(i + 1, n):
            x = a[i][j]
            y = a[j][i]

            if x != -1 and y != -1:
                if x != y:
                    return None
                fixed.append((i, j, x))
                dist[i][j] = x
                dist[j][i] = x
            elif x != -1:
                fixed.append((i, j, x))
                dist[i][j] = x
                dist[j][i] = x
            elif y != -1:
                fixed.append((i, j, y))
                dist[i][j] = y
                dist[j][i] = y

    for k in range(n):
        dk = dist[k]
        for i in range(n):
            di = dist[i]
            dik = di[k]
            if dik == INF:
                continue
            for j in range(n):
                nd = dik + dk[j]
                if nd < di[j]:
                    di[j] = nd

    for u, v, w in fixed:
        if dist[u][v] != w:
            return None

    for i in range(n):
        for j in range(n):
            if dist[i][j] == INF:
                dist[i][j] = BIG

    return dist

def solve(inp):
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = [list(map(int, input().split())) for _ in range(n)]
        ans = solve_case(a)

        if ans is None:
            out.append("NO")
        else:
            out.append("YES")
            for row in ans:
                out.append(" ".join(map(str, row)))

    sys.stdin = old_stdin
    return "\n".join(out)

def parse_output(s):
    return s.strip().splitlines()

def assert_valid_case(a, output_lines, pos):
    n = len(a)

    assert output_lines[pos] == "YES"
    pos += 1

    b = []
    for _ in range(n):
        row = list(map(int, output_lines[pos].split()))
        assert len(row) == n
        b.append(row)
        pos += 1

    for i in range(n):
        assert b[i][i] == 0
        for j in range(n):
            assert 0 <= b[i][j] <= BIG
            assert b[i][j] == b[j][i]
            if a[i][j] != -1:
                assert b[i][j] == a[i][j]

    for i in range(n):
        for j in range(n):
            for k in range(n):
                assert b[i][j] <= b[i][k] + b[k][j]

    return pos

# Official sample
sample = """\
4
3
0 3 3
3 0 3
3 3 0
3
0 0 0
0 0 -1
0 -1 0
3
5 6 7
-1 -1 -1
-1 -1 -1
3
-1 3 5
-1 -1 3
-1 -1 -1
"""

expected_sample = """\
YES
0 3 3
3 0 3
3 3 0
YES
0 0 0
0 0 0
0 0 0
NO
YES
0 3 5
3 0 3
5 3 0
"""

assert solve(sample) == expected_sample.strip(), "official sample"

# Minimum-size valid input.
case_min = """\
1
2
0 1000
1000 0
"""
assert solve(case_min) == """\
YES
0 1000
1000 0
""".strip(), "minimum size and maximum fixed distance"

# All distances equal to zero, including distinct points.
case_zero = """\
1
4
0 0 -1 0
0 0 0 -1
-1 0 0 0
0 -1 0 0
"""
assert solve(case_zero) == """\
YES
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
""".strip(), "all-zero pseudometric"

# A longer known path contradicts a fixed direct edge.
case_shorter_path = """\
1
3
0 5 2
5 0 2
2 2 0
"""
assert solve(case_shorter_path).strip() == "NO", "fixed edge is longer than a known path"

# Asymmetric fixed values.
case_asymmetric = """\
1
2
0 3
4 0
"""
assert solve(case_asymmetric).strip() == "NO", "symmetry contradiction"

# Maximum-size case, all distances initially unknown.
n = 500
rows = []
for i in range(n):
    row = [-1] * n
    row[i] = 0
    rows.append(row)

max_input = "1\n500\n" + "\n".join(" ".join(map(str, row)) for row in rows) + "\n"
max_output = solve(max_input)
max_lines = parse_output(max_output)

assert max_lines[0] == "YES"
assert len(max_lines) == 501

for i in range(500):
    row = list(map(int, max_lines[i + 1].split()))
    assert len(row) == 500
    assert row[i] == 0
    for j in range(500):
        if i != j:
            assert row[j] == BIG
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`điểm có khoảng cách`1000`| Ma trận cố định | Kích thước tối thiểu và khoảng cách đầu vào lớn nhất được phép | 
| Bốn điểm với mọi khoảng cách bằng 0 | Ma trận hoàn toàn bằng không | Khoảng cách bằng không giữa các điểm khác nhau | 
| Ba điểm có cạnh`2, 2, 5`|`NO`| Phát hiện đường dẫn đa cạnh ngắn hơn | 
| Hai điểm với`3`theo một hướng và`4`ở bên kia |`NO`| Xác thực tính đối xứng | 
| (500\times500) ma trận chỉ có đường chéo bằng 0 |`YES`, đường chéo bằng 0 và (10^9) ở nơi khác | Kích thước tối đa và các thành phần bị ngắt kết nối | 

## Vỏ cạnh 

Đường chéo cố định khác 0 bị loại bỏ trước khi xử lý đồ thị. Đối với đầu vào```
1
2
1 -1
-1 0
```mục nhập chéo đầu tiên là`1`, Vì thế`solve_case`ngay lập tức trở lại`None`. Đầu ra là`NO`. Không phép tính đường đi ngắn nhất nào có thể làm cho điều này hợp lệ vì đường chéo là tiên đề số liệu trực tiếp. 

Một cặp bất đối xứng được xử lý trước Floyd-Warshall. Với```
1
2
0 3
4 0
```cặp ((1,2)) có hai giá trị cố định,`3`Và`4`. Việc xây dựng phát hiện`x != y`và bác bỏ vụ kiện. Một giải pháp thay thế hấp dẫn là ghi đè lên mặt này bằng mặt kia, nhưng điều đó sẽ vi phạm yêu cầu không thể sửa đổi các giá trị đầu vào cố định. 

Mâu thuẫn đường đi ngắn hơn được phát hiện bởi pha đường đi ngắn nhất tất cả các cặp. Vì```
1
3
0 5 2
5 0 2
2 2 0
```cạnh ban đầu (1\leftrightarrow2) có trọng số`5`. Khi đỉnh (3) được coi là đỉnh trung gian, Floyd-Warshall thu được 

[ 
d(1,2)=\min(5,2+2)=4. 
] 

Giá trị cố định ban đầu là`5`, vì vậy lần xác thực cuối cùng không thành công và câu trả lời là`NO`. Đối số tương tự áp dụng cho đường dẫn chứa nhiều cạnh, đó là lý do tại sao chỉ kiểm tra các hình tam giác nhìn thấy trực tiếp là không đủ. 

Một cặp đối xứng được chỉ định một phần được điền từ phía đã biết. TRONG```
1
3
0 3 -1
-1 0 4
-1 -1 0
```các mục (d(1,2)) và (d(2,1)) trở thành`3`, trong khi (d(2,3)) và (d(3,2)) trở thành`4`. Cặp còn lại có thể được chọn làm`7`, đây là giá trị đường đi ngắn nhất qua đỉnh (2). Thuật toán phát hiện ra điều này một cách tự nhiên thông qua Floyd-Warshall. 

Các thành phần bị ngắt kết nối được hoàn thành sau khi tính toán đường dẫn ngắn nhất. Vì```
1
3
0 2 -1
2 0 -1
-1 -1 0
```đỉnh (1) và (2) có khoảng cách`2`, trong khi đỉnh (3) không có mối liên hệ nào với chúng. Floyd-Warshall để lại các mục nhập thành phần chéo tại`INF`và giai đoạn cuối cùng thay đổi các mục đó thành (10^9). Ma trận kết quả là```
0 2 1000000000
2 0 1000000000
1000000000 1000000000 0
```Mọi tam giác liên quan đến hai khoảng cách chéo thành phần đều hợp lệ và khoảng cách bên trong`2`nhỏ hơn nhiều so với (10^9+10^9). 

Cuối cùng, một đầu vào có mọi mục nhập ngoài đường chéo bằng 0 là hợp lệ theo định nghĩa của bài toán này. Ví dụ,```
1
3
0 0 0
0 0 0
0 0 0
```trôi qua không thay đổi. Việc triển khai không bao giờ chèn yêu cầu tích cực cho các đỉnh riêng biệt, phù hợp với điều kiện thực tế mà vấn đề sử dụng.
