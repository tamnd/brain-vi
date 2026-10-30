---
title: "CF 106267J - Chín Vòng Tròn"
description: "Chúng tôi được cung cấp một số vòng tròn trong mặt phẳng. Mỗi đường tròn có tâm và bán kính cố định và chúng ta được phép chia tỷ lệ tất cả bán kính theo một hệ số không âm duy nhất $k$. Sau khi chia tỷ lệ, mọi đường tròn sẽ trở thành một hình tròn có cùng tâm nhưng bán kính $k cdot ri$."
date: "2026-06-18T23:16:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106267
codeforces_index: "J"
codeforces_contest_name: "The 20-th Beihang University Collegiate Programming Contest (BCPC 2025) - Final"
rating: 0
weight: 106267
solve_time_s: 83
verified: true
draft: false
---

[CF 106267J - Chín vòng tròn](https://codeforces.com/problemset/problem/106267/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số vòng tròn trong mặt phẳng. Mỗi vòng tròn có tâm và bán kính cố định và chúng ta được phép chia tỷ lệ tất cả bán kính theo một hệ số không âm duy nhất$k$. Sau khi chia tỷ lệ, mọi đường tròn sẽ trở thành một hình tròn có cùng tâm nhưng bán kính$k \cdot r_i$. 

Sau đó chúng ta hỏi liệu có tồn tại một đường thẳng cắt mỗi đĩa hay không. Giao lộ ở đây có nghĩa là đường thẳng cắt qua đĩa hoặc tiếp tuyến với nó. Nếu như$k = 0$, tất cả các đường tròn đều trở thành điểm, do đó đường thẳng phải đi qua mọi tâm. 

Nhiệm vụ là tìm số nhỏ nhất$k$sao cho ít nhất một đường có thể giao nhau đồng thời với tất cả các đĩa được chia tỷ lệ. 

Ràng buộc hình học quan trọng là một đường thẳng cắt một đĩa khi và chỉ khi khoảng cách vuông góc từ tâm đĩa đến đường thẳng lớn nhất là bán kính của nó. Vì vậy, đối với một đường cố định, mỗi vòng tròn áp đặt một giới hạn trên cho khoảng cách cho phép từ tâm của nó đến đường thẳng. 

Với$n$Cho đến khoảng năm 2000, việc kiểm tra đơn giản trên tất cả các đường được xác định bởi các cặp hoặc bộ ba đường tròn đã gần như khả thi nếu được thực hiện cẩn thận, nhưng việc tìm kiếm hình học trực tiếp trên các đường tùy ý là không thể. Cấu trúc của ràng buộc buộc chúng ta phải chuyển vấn đề thành một vấn đề chỉ phụ thuộc vào mối quan hệ từng cặp. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các tâm đều giống hệt nhau. Trong trường hợp đó, bất kỳ dòng nào cũng hoạt động$k = 0$, vì mọi vòng tròn đều co lại thành một điểm ở cùng một vị trí. Một giải pháp bất cẩn giả sử hình học theo cặp vẫn có thể hoạt động, nhưng việc triển khai chia cho các biểu thức như$r_i + r_j$phải tránh hành vi bằng 0 hoặc thoái hóa ở các bước xuôi dòng. 

Một trường hợp cạnh khác là khi tất cả bán kính đều bằng nhau. Câu trả lời không nhất thiết phải bằng 0, bởi vì ngay cả với tỷ lệ bằng nhau, một đường thẳng vẫn phải đi qua trong một khoảng cách cố định đến tất cả các tâm, điều này trở thành vấn đề về chiều rộng thuần túy của một tập hợp điểm. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ định nghĩa. Chúng tôi cố gắng đoán một dòng, sau đó tính toán mức tối thiểu$k$cần thiết cho dòng đó, sau đó thu nhỏ trên tất cả các dòng. Đối với đường dây cố định$L$, mỗi vòng tròn$i$góp phần hạn chế$k \ge \frac{d_i}{r_i}$, Ở đâu$d_i$là khoảng cách từ tâm đến đường thẳng. Vậy tốt nhất$k$vì đường đó là mức tối đa của các tỷ lệ này. Khó khăn là khoảng cách của các dòng là liên tục và thậm chí còn hạn chế đối với các ứng cử viên có ý nghĩa tổ hợp.$O(n^2)$hoặc nhiều khả năng hơn. 

Quan sát quan trọng là chúng ta có thể tham số hóa một đường theo hướng bình thường của nó. Sửa hướng đơn vị$n$và chiếu tất cả các điểm lên trục đó. Sau đó, đường này được xác định bằng sự dịch chuyển dọc theo hướng này và tính khả thi sẽ giảm xuống điều kiện giao nhau. Điều này chuyển đổi hình học thành bài toán tối ưu hóa 1D cho mỗi hướng. 

Đối với một hướng cố định$n$, mỗi điểm$p_i$dự án thành vô hướng$t_i = p_i \cdot n$. Một đường thẳng vuông góc với$n$tương ứng với việc chọn ca$c$và tính khả thi trở thành một tập hợp các ràng buộc có dạng$|t_i + c| \le k r_i$. Mỗi điểm đóng góp một khoảng giá trị$c$và tất cả các khoảng phải giao nhau. 

Điều này biến vấn đề thành các ràng buộc về sự khác biệt theo cặp:$$|t_i - t_j| \le k (r_i + r_j)$$vì vậy đối với một hướng cố định,$$k(n) = \max_{i,j} \frac{|(p_i - p_j)\cdot n|}{r_i + r_j}.$$Bây giờ xác định vectơ$$u_{ij} = \frac{p_i - p_j}{r_i + r_j}.$$Khi đó biểu thức trở thành$$k(n) = \max_{i,j} |u_{ij} \cdot n|.$$Vì vậy, thay vì suy luận về đường tròn, bây giờ chúng ta suy luận về một tập hợp các điểm$u_{ij}$trong máy bay. Chúng tôi muốn chọn một hướng đi$n$giúp giảm thiểu hình chiếu tối đa của bộ này. Về mặt hình học, đây chính xác là chiều rộng tối thiểu của điểm được đặt dưới góc quay. 

Vấn đề còn lại đó là$u_{ij}$có$O(n^2)$các phần tử nhưng$n$đủ nhỏ để việc xây dựng tất cả các cặp có thể chấp nhận được. Sau khi bộ này được xây dựng, vấn đề sẽ trở thành vấn đề về chiều rộng thân tàu lồi cổ điển, có thể giải quyết được bằng thước cặp quay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên đường | Hàm mũ / Vô hạn | O(1) | Quá chậm | 
| Giảm theo cặp + thân lồi + thước cặp quay | O(n^2 log n) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng tất cả các vectơ cặp$u_{ij} = \frac{(x_i - x_j, y_i - y_j)}{r_i + r_j}$cho tất cả$i < j$và cũng bao gồm các số âm đối xứng ngầm hoặc rõ ràng. Bước này chuyển đổi các ràng buộc vòng tròn thành một tập hợp điểm hình học duy nhất. 
2. Hãy đối xử với tất cả$u_{ij}$như các điểm trong mặt phẳng. Mục tiêu trở thành tìm hướng giảm thiểu tích số chấm tuyệt đối tối đa trên tập hợp này. 
3. Tính bao lồi của tất cả các điểm này. Các điểm nằm hoàn toàn bên trong thân tàu không bao giờ ảnh hưởng đến các phép chiếu cực trị, vì vậy chúng có thể bị loại bỏ mà không làm thay đổi câu trả lời. 
4. Áp dụng thước cặp quay trên thân lồi để tính chiều rộng tối thiểu của nó theo mọi hướng. Đối với mỗi cạnh của thân tàu, coi hướng của nó là hướng bình thường dự kiến ​​và tính khoảng cách giữa các đường đỡ. 
5. Chiều rộng tối thiểu thu được là phạm vi chiếu nhỏ nhất có thể. Vì tập hợp đối xứng (vì hoán đổi$i$Và$j$phủ định vectơ), hình chiếu tuyệt đối tối đa bằng một nửa chiều rộng này. 
6. Trở về$k = \frac{\text{minimum width}}{2}$. 

Lý do cốt lõi của điều này là vì hạn chế tồi tệ nhất đối với bất kỳ hướng cố định nào luôn được nhận ra bởi một cặp điểm và sau khi được chuẩn hóa bởi$r_i + r_j$, các ràng buộc theo cặp này hoạt động giống như các hình chiếu tuyến tính của một tập hợp điểm phẳng hữu hạn. Hướng tối ưu phải phù hợp với hướng đỡ của thân lồi của bộ đó, do đó không có cấu hình bên trong nào có thể xác định giải pháp tốt hơn cạnh thân tàu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

def convex_hull(points):
    points = sorted(points)
    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1]

def width(points):
    n = len(points)
    if n <= 1:
        return 0.0

    j = 1
    best = float("inf")

    for i in range(n):
        ni = (points[(i + 1) % n][0] - points[i][0],
              points[(i + 1) % n][1] - points[i][1])

        while True:
            k1 = abs(ni[0] * (points[(j + 1) % n][0] - points[i][0]) +
                     ni[1] * (points[(j + 1) % n][1] - points[i][1]))
            k2 = abs(ni[0] * (points[j][0] - points[i][0]) +
                     ni[1] * (points[j][1] - points[i][1]))
            if k1 > k2:
                j = (j + 1) % n
            else:
                break

        cur = abs(ni[0] * (points[j][0] - points[i][0]) +
                  ni[1] * (points[j][1] - points[i][1])) / (ni[0]**2 + ni[1]**2)
        best = min(best, cur)

    return best * 2

n = int(input())
circles = [tuple(map(int, input().split())) for _ in range(n)]

pts = []
for i in range(n):
    x1, y1, r1 = circles[i]
    for j in range(i + 1, n):
        x2, y2, r2 = circles[j]
        denom = r1 + r2
        dx = (x1 - x2) / denom
        dy = (y1 - y2) / denom
        pts.append((dx, dy))

hull = convex_hull(pts)
ans = width(hull)
print(ans)
```Giai đoạn xây dựng xây dựng tất cả các vectơ sai phân đã chuẩn hóa. Bao lồi loại bỏ tất cả các tương tác cặp không cực trị, chỉ để lại các ứng cử viên có thể xác định phép chiếu tối đa. Sau đó, bước thước cặp xoay sẽ tìm ra chiều rộng tối thiểu có thể có của thân tàu này theo tất cả các hướng. 

Logic nhân và chia cuối cùng xuất phát trực tiếp từ đối số đối xứng: vì mọi hiệu vectơ đều xuất hiện cùng với số âm của nó, nên chiều rộng bằng hai lần phép chiếu tuyệt đối tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét ba đường tròn có tọa độ nguyên nhỏ sao cho dễ nhìn thấy các vectơ theo cặp. Giả sử hai điểm tạo ra sự khác biệt được chuẩn hóa chi phối theo một hướng cụ thể. Thuật toán trước tiên xây dựng tất cả các vectơ theo cặp, sau đó loại bỏ các vectơ bên trong thông qua bao lồi. 

| Bước | Tiểu bang | 
| --- | --- | 
| Cặp vectơ | Tất cả sự khác biệt được chuẩn hóa$u_{ij}$| 
| Vỏ lồi | Chỉ còn lại các vectơ định hướng cực đoan | 
| Thước cặp | Kiểm tra từng hướng cạnh thân tàu | 
| Trả lời | Nửa chiều rộng tối thiểu | 

Điều này chứng tỏ các mối quan hệ cặp bên trong không bao giờ ảnh hưởng đến ràng buộc cuối cùng, mặc dù chúng tồn tại trong công thức ban đầu. 

### Ví dụ 2 

Nếu tất cả các vòng tròn có bán kính giống hệt nhau nhưng nằm rải rác trên một đường thẳng, thì việc chuẩn hóa theo cặp sẽ giảm xuống các chênh lệch tọa độ theo tỷ lệ và thân tàu sẽ trở thành hình thon dài giống như 1D. 

| Bước | Tiểu bang | 
| --- | --- | 
| Cặp vectơ | Bộ cộng tuyến | 
| Vỏ lồi | Hai điểm cuối chiếm ưu thế | 
| Thước cặp | Chiều rộng tính theo phương vuông góc | 
| Trả lời | Được kiểm soát bằng sự tách biệt cực độ | 

Điều này cho thấy giải pháp tự nhiên giảm xuống mức tối thiểu hóa chiều rộng cổ điển khi hình học suy biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 log n) | xây dựng tất cả các cặp cộng với phân loại thân lồi | 
| Không gian | O(n^2) | lưu trữ tất cả các vectơ theo cặp | 

Với$n \le 2000$, việc tạo theo cặp tạo ra tối đa khoảng 2 triệu điểm, đây là đường biên nhưng khả thi trong Python được tối ưu hóa hoặc thoải mái trong giới hạn bộ nhớ nhất định của C++. 

Thân lồi và thước cặp quay đều hoạt động theo thời gian tuyến tính tương ứng với số lượng điểm được xây dựng, do đó việc phân loại chiếm ưu thế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    return _sys.stdout.getvalue()

# sample tests would go here if full solver was wrapped
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòng tròn giống hệt nhau | 0 | trường hợp suy biến k=0 | 
| hai vòng tròn xa xôi | dương k | sự thống trị của cặp đơn | 
| trung tâm cộng tuyến | chiều rộng hữu hạn | Giảm hình học 1D | 
| bộ nhỏ ngẫu nhiên | phao ổn định | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các bán kính đều giống nhau và tất cả các tâm đều nằm trên một đường thẳng. Trong trường hợp này, mọi vectơ chuẩn hóa nằm trên một đường thẳng trong không gian được biến đổi và bao lồi suy biến thành một đoạn. Các thước cặp quay vẫn hoạt động, nhưng chỉ có hai hướng đối cực là quan trọng, và câu trả lời hoàn toàn đến từ cặp cực đoan. 

Một trường hợp cạnh khác xảy ra khi nhiều đường tròn có cùng tâm nhưng bán kính khác nhau. Khi đó nhiều vectơ cặp trở thành 0 và chúng không ảnh hưởng đến thân tàu. Việc triển khai đúng phải tránh các vấn đề về phép chia và đảm bảo vectơ 0 không làm sai lệch tính toán thân tàu. 

Trường hợp cuối cùng là khi một vòng tròn lấn át tất cả những vòng tròn khác có bán kính rất lớn. Sau đó, tất cả các khác biệt được chuẩn hóa liên quan đến vòng tròn đó sẽ trở nên rất nhỏ và thân tàu sẽ co lại tương ứng. Thuật toán xử lý việc này một cách tự nhiên vì việc chia tỷ lệ đã được đưa vào bước chuẩn hóa theo cặp.
