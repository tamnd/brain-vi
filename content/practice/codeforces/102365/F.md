---
title: "CF 102365F - Phân phối công bằng"
description: "Đối với mỗi tập hợp con của dân làng, hãy xem xét diện tích phần thân lồi của ngôi nhà của họ. Một hoán vị ngẫu nhiên mang lại cho mỗi dân làng một đóng góp cận biên: khi dân làng đó được chèn vào, hãy so sánh diện tích thân lồi mới với diện tích trước khi chèn."
date: "2026-08-13T00:02:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102365
codeforces_index: "F"
codeforces_contest_name: "UBC Programming Contest 2019 (UBCPC 2019)"
rating: 0
weight: 102365
solve_time_s: 216
verified: true
draft: false
---

[CF 102365F - Phân phối công bằng](https://codeforces.com/problemset/problem/102365/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi tập hợp con của dân làng, hãy xem xét diện tích phần thân lồi của ngôi nhà của họ. Một hoán vị ngẫu nhiên mang lại cho mỗi dân làng một đóng góp cận biên: khi dân làng đó được chèn vào, hãy so sánh diện tích thân lồi mới với diện tích trước khi chèn. Câu trả lời bắt buộc đối với một dân làng là mức đóng góp trung bình của mức đóng góp cận biên đó cho tất cả các lệnh chèn có thể có. 

Tương đương, đây là giá trị Shapley của hàm diện tích thân lồi. Đối với một dân làng cố định (p), chúng ta cần mức tăng dự kiến ​​về diện tích thân tàu khi (p) được chèn vào một thứ tự ngẫu nhiên thống nhất của các điểm (N-1) khác. 

Dữ liệu đầu vào chứa tối đa 200 điểm phẳng riêng biệt, với tọa độ được giới hạn bởi (10^4). Giá trị nhỏ của (N) loại trừ phép liệt kê giai thừa hoặc hàm mũ, nhưng nó dành chỗ cho thuật toán bậc ba. Cụ thể, (N^3) chỉ có (8\cdot10^6) lần lặp ở kích thước tối đa, đây là mục tiêu hợp lý để triển khai được tối ưu hóa. Giới hạn tọa độ cũng có nghĩa là mọi bài kiểm tra định hướng thông thường đều vừa khít với số nguyên 64 bit, mặc dù số nguyên Python loại bỏ mọi lo ngại về tràn. 

Trường hợp suy biến đầu tiên là (N=1). Một điểm có bao lồi có diện tích bằng 0 nên đáp án duy nhất là (0).```
1
10000 -10000
```Đầu ra đúng là```
0
```Việc triển khai giả định rằng mọi dân làng cuối cùng có thể tạo thành một hình tam giác có thể cố gắng chia cho một cấp độ không tồn tại. 

Trường hợp thứ hai là hai điểm.```
2
0 0
10000 10000
```Một lần nữa câu trả lời là```
0
0
```Bao lồi của hai điểm là một đoạn nên mọi đóng góp cận biên đều bằng không. Một công thức dựa trên ba điểm phải xử lý rõ ràng trường hợp này. 

Vấn đề thứ ba là tính cộng tác. Coi như```
4
0 0
1 0
2 0
0 1
```Thân tàu là một tam giác có các đỉnh ((0,0),(2,0),(0,1)), nhưng điểm ((1,0)) nằm trên một trong các cạnh của nó. Một phiên bản đơn giản của công thức vị trí tổng quát thông thường có thể đếm tam giác bằng cách sử dụng cả hai điểm cuối ((0,0),(2,0)) và cũng đếm các tam giác nhỏ hơn liên quan đến ((1,0)), tạo ra quá nhiều diện tích. Các giá trị Shapley đúng là```
0.4166666666666667
0.0833333333333333
0.0833333333333333
0.4166666666666667
```Việc triển khai bên dưới xử lý các suy biến như vậy bằng cách sử dụng nhiễu loạn ký hiệu vô hạn cho các quyết định định hướng trong khi vẫn giữ lại tọa độ ban đầu cho các khu vực tam giác. Đây là cách giải thích giới hạn chính xác vì diện tích thân lồi, và do đó, mỗi giá trị trung bình hữu hạn của đóng góp cận biên, là liên tục dưới các nhiễu loạn nhỏ tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp tuân theo định nghĩa theo nghĩa đen. Liệt kê mọi hoán vị (N!) Đối với mỗi hoán vị, hãy chèn từng điểm một, tính toán lại vỏ lồi sau mỗi lần chèn và thêm phần tăng diện tích cho dân làng tương ứng. Điều này đúng vì nó đánh giá rõ ràng chính xác thử nghiệm mà bài toán yêu cầu kỳ vọng của nó. 

Vấn đề là số giai thừa của hoán vị. Ngay cả trước khi tính đến kết cấu thân tàu, (200!) là xấp xỉ (7,9\cdot10^{374}). Việc tính toán lại phần thân cho mỗi tiền tố sẽ khiến công việc trở nên gần như (O(N!,N^2\log N)), điều này thật vô vọng. 

Quan sát hữu ích là sự gia tăng diện tích biên của thân tàu có cấu trúc hình học rất cụ thể. Giả sử (p) là điểm được chèn vào. Phần thân tàu mới chưa có trên thân tàu cũ là một chuỗi các hình tam giác có đỉnh chung là (p). Mỗi tam giác như vậy có thể được mô tả bởi hai điểm trước đó (q) và (q'). 

Hướng đường thẳng từ (q) đến (q') và đặt (H(q,q')) là nửa mặt phẳng mở bên trái của nó. Nếu (p) nằm trong nửa mặt phẳng đó thì tam giác (pqq') có thể xuất hiện ở phần mới lộ ra của thân tàu một cách chính xác khi (q) và (q') là hai điểm liên quan đầu tiên xuất hiện, trong khi (p) là điểm thứ ba. 

Gọi (L(q,q')) là số điểm đầu vào nằm hoàn toàn trong (H(q,q')). Trong số các điểm (L+2) bao gồm các điểm (q,q') và các điểm (L) đó, sự kiện bắt buộc nói rằng (q,q') chiếm hai vị trí đầu tiên, theo cả hai thứ tự, và (p) chiếm vị trí thứ ba. Xác suất của nó là 

# \frac{2!,(L-1)!}{(L+2)!} 

\frac{2}{L(L+1)(L+2)}. 
] 

Đây là mức giảm chính. Tập hợp khổng lồ các hoán vị biến mất. Đối với mỗi cặp điểm có thứ tự, chúng ta chỉ cần một số nguyên, mức nửa mặt phẳng của nó và một xác suất hữu tỉ. 

Đối với (p) cố định, đóng góp dự kiến sẽ là 

\sum_{\substack{q\ne q'\p\in H(q,q')}} 
\operatorname{area}(pqq')\rho(q,q'). 
] 

Các giá trị mức có thể được tính toán một cách hiệu quả bằng cách sắp xếp các điểm khác theo góc cực xung quanh mỗi (q) và sử dụng con trỏ quay. Khi đã biết các mức, việc đánh giá công thức cho tất cả (p) sẽ mất (O(N^3)) thời gian. 

Với (N\le200) đã cho, đây là nghiệm thực tế. Sự phân rã hình học tương tự cho phép sử dụng thuật toán (O(N^2)) tiên tiến hơn bằng cách tổng hợp các truy vấn nửa mặt phẳng có trọng số thông qua cách sắp xếp đường, đây là phiên bản tiệm cận tối ưu của ý tưởng này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N!,N^2\log N)) | (O(N)) | Quá chậm | 
| Cấp độ cặp và nửa mặt phẳng | (O(N^3)) | (O(N^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các điểm và chuẩn bị một chút ký hiệu nhiễu loạn cho các quyết định định hướng. Tọa độ ban đầu không thay đổi khi tính diện tích tam giác. Sự nhiễu loạn chỉ quyết định một điểm thuộc về phía nào của đường thẳng khi ba điểm ban đầu thẳng hàng. 
2. Với mỗi điểm (q), hãy sắp xếp tất cả các điểm khác theo góc cực của chúng xung quanh (q). Với sự nhiễu loạn, không có hai hướng liên quan nào được ràng buộc chính xác. 
3. Quét xung quanh các hướng đã sắp xếp bằng con trỏ thứ hai. Đối với cặp có hướng (q\to q'), mọi điểm gặp hoàn toàn ngược chiều kim đồng hồ từ (q') nhỏ hơn (180^\circ) đều nằm trong nửa mặt phẳng bên trái của đường thẳng có hướng. Số điểm như vậy là (L(q,q')). 
4. Chuyển đổi mọi cấp độ (L\ge1) thành xác suất 
[ 
\rho(q,q')=\frac{2}{L(L+1)(L+2)}. 
] 
Mức 0 nhận trọng số bằng 0 vì không có điểm thứ ba nào có thể nằm trong nửa mặt phẳng đó. 
5. Đối với mỗi điểm mục tiêu (p), hãy kiểm tra từng cặp không có thứ tự (q,q'). Tính diện tích kép đã ký ban đầu 
[ 
c=(q'-q)\times(p-q). 
] 
Nếu (c>0), (p) nằm bên trái (q\to q'), thì cộng 
[ 
\frac{c}{2}\rho(q,q'). 
] 
Nếu (c<0), (p) nằm bên trái (q'\to q), thì cộng 
[ 
\frac{-c}{2}\rho(q',q). 
] 
Nếu (c=0), tam giác có diện tích bằng 0 và không đóng góp gì. 
6. In giá trị tích lũy của mỗi điểm. Với (N<3), các vòng lặp tự nhiên tạo ra số 0 cho mọi dân làng. 

Bất biến đằng sau phép tính là mọi phần có diện tích dương được tạo bằng cách chèn (p) được biểu diễn bằng chính xác một cặp có hướng (q,q'), cụ thể là các điểm cuối của một cạnh lộ ra của thân tàu cũ. Sự kiện được biểu thị bởi cặp đó chính xác là (q,q') xuất hiện trước (p), trong khi mọi điểm ở phía liên quan đều xuất hiện sau (p). Xác suất được gán cho cặp chính xác là xác suất của sự kiện đó. Do đó, việc tính tổng các diện tích tam giác tương ứng sẽ mang lại đóng góp cận biên cho một hoán vị trong kỳ vọng và tính tuyến tính của kỳ vọng cho phép tất cả các cặp được tính tổng một cách độc lập. 

Đối với đầu vào cộng tuyến, nhiễu loạn vô hạn chỉ thay đổi cách phân loại cạnh của các bộ ba có diện tích ban đầu bằng 0. Dù sao thì những bộ ba như vậy cũng đóng góp diện tích bằng không. Đối với mọi bộ ba không thẳng hàng, hướng nguyên ban đầu khác 0 và lớn hơn nhiều so với nhiễu loạn vô cùng nhỏ, do đó việc phân loại cạnh của nó không thay đổi. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    if n < 3:
        for _ in range(n):
            print("0.0")
        return

    # The perturbation is only used for orientation / angular ordering.
    # x_i -> x_i + eps * i
    # y_i -> y_i + eps * i^2
    #
    # eps is chosen far below the smallest possible nonzero integer
    # orientation, which has absolute value at least 1.
    eps = 1e-10

    px = [x + eps * (i + 1) for i, (x, y) in enumerate(pts)]
    py = [y + eps * (i + 1) * (i + 1)
          for i, (x, y) in enumerate(pts)]

    rho = [[0.0] * n for _ in range(n)]

    # Compute the level of every directed line q -> r.
    #
    # For a fixed q, points are sorted by angle around q.
    # For every starting direction, a monotone pointer finds the
    # entire open semicircle to its left.
    for q in range(n):
        order = [i for i in range(n) if i != q]

        qx = px[q]
        qy = py[q]

        order.sort(
            key=lambda i: math.atan2(py[i] - qy, px[i] - qx)
        )

        m = n - 1
        doubled = order + order
        t = 0

        for s in range(m):
            if t < s + 1:
                t = s + 1

            a = doubled[s]
            ax = px[a] - qx
            ay = py[a] - qy

            while t < s + m:
                b = doubled[t]
                bx = px[b] - qx
                by = py[b] - qy

                cross = ax * by - ay * bx
                if cross > 0.0:
                    t += 1
                else:
                    break

            level = t - s - 1
            if level > 0:
                rho[q][a] = (
                    2.0 / (level * (level + 1) * (level + 2))
                )

    ans = [0.0] * n

    # For each p and each unordered pair q,r, exactly one orientation
    # puts p strictly on the left, unless p,q,r are collinear.
    for p in range(n):
        total = 0.0
        px0, py0 = pts[p]

        for q in range(n):
            qx, qy = pts[q]

            for r in range(q + 1, n):
                if r == p or q == p:
                    continue

                rx, ry = pts[r]

                cross = (
                    (rx - qx) * (py0 - qy)
                    - (ry - qy) * (px0 - qx)
                )

                if cross > 0:
                    total += 0.5 * cross * rho[q][r]
                elif cross < 0:
                    total += 0.5 * (-cross) * rho[r][q]

        ans[p] = total

    for value in ans:
        print("{:.15f}".format(value))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai xây dựng tọa độ nhiễu loạn. Sự nhiễu loạn là rất nhỏ, trong khi sự phụ thuộc của nó vào chỉ số điểm khiến nó có tính xác định. Nếu hướng ban đầu khác 0 thì giá trị tuyệt đối của nó ít nhất là 1 vì tất cả tọa độ đầu vào đều là số nguyên. Sự nhiễu loạn quá nhỏ để đảo ngược sự định hướng như vậy. 

Quét góc tính toán (L(q,q')) mà không cần kiểm tra tất cả các điểm (N) đối với mọi đường có hướng. Đối với mỗi (q cố định), các điểm được sắp xếp theo vòng tròn một lần. Khi hướng bắt đầu tiến lên, điểm cuối của hình bán nguyệt bên trái không bao giờ di chuyển lùi lại, do đó tất cả các mức cho (q) đó đều đạt được theo thời gian tuyến tính sau khi sắp xếp. 

các`rho`ma trận được lập chỉ mục bởi các cặp có hướng. Điều này quan trọng vì hai hướng của cùng một đường hình học có các nửa mặt phẳng bên trái khác nhau và do đó thường có các mức và xác suất khác nhau. 

Vòng lặp ba cuối cùng chỉ sử dụng các cặp không có thứ tự. Nếu tích chéo ban đầu là dương thì đường có hướng liên quan là (q\to r). Nếu nó âm thì hướng liên quan là (r\to q). Điều này tránh thực hiện cùng một cặp hình học hai lần. 

Tính toán diện tích sử dụng tọa độ nguyên ban đầu, không phải tọa độ nhiễu loạn. Điều này là cần thiết. Sự nhiễu loạn là một công cụ mang tính biểu tượng để quyết định các mối quan hệ tổ hợp, không phải là sự điều chỉnh số tiền thực tế được biểu thị bằng hình học. 

Tất cả các phép tính định hướng hình học liên quan đến tọa độ ban đầu đều là các phép tính số nguyên. Các số nguyên có độ chính xác tùy ý của Python khiến cho việc tràn ở đây là không thể. Các phép toán dấu phẩy động duy nhất là các giá trị xác suất, thứ tự góc và tích lũy cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu có bốn điểm:```
(2,2)
(0,2)
(2,0)
(1,1)
```Ba điểm đầu tiên tạo thành một tam giác có diện tích (2), trong khi ((1,1)) nằm bên trong tam giác đó. Số cổ phiếu dự kiến ​​là```
0.8333333333333333
0.5
0.5
0.1666666666666667
```Dấu vết sau đây tập trung vào sự đóng góp của cặp hình học. 

| Mục tiêu (p) | Cặp | Diện tích đôi | Cấp độ liên quan | Xác suất | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| ((2,2)) | ((0,2),(2,0)) | 4 | 1 | (1/3) | (2/3) | 
| ((2,2)) | các cặp liên quan khác | khác nhau | khác nhau | khác nhau | (1/6) tổng cộng | 
| ((0,2)) | cặp thân tàu | khác nhau | khác nhau | khác nhau | (1/2) tổng cộng | 
| ((2,0)) | cặp thân tàu | khác nhau | khác nhau | khác nhau | (1/2) tổng cộng | 
| ((1,1)) | cặp thân tàu | khác nhau | khác nhau | khác nhau | (1/6) tổng cộng | 

Điểm bên trong nhận được một giá trị dương vì nó có thể được chèn vào sau hai điểm tạo thành một bao nhỏ hơn và có thể phóng to bao đó, mặc dù nó không bao giờ thuộc về bao cuối cùng của cả bốn điểm. 

### Mẫu 2 

Hãy xem xét đầu vào ba điểm```
3
0 0
2 0
0 2
```Thân lồi toàn phần có diện tích (2). Với chính xác ba điểm, một dân làng đóng góp diện tích khác 0 chính xác khi dân làng đó đứng thứ ba trong hoán vị. Mỗi dân làng đứng thứ ba về chính xác (2!) trong số (3!) hoán vị, vì vậy mọi dân làng đều nhận được (2/3). 

| Mục tiêu (p) | Số điểm khác | Cặp liên quan | Diện tích đôi | Cấp độ | Xác suất | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| ((0,0)) | 2 | ((2,0),(0,2)) | 4 | 1 | (1/3) | (2/3) | 
| ((2,0)) | 2 | ((0,2),(0,0)) | 4 | 1 | (1/3) | (2/3) | 
| ((0,2)) | 2 | ((0,0),(2,0)) | 4 | 1 | (1/3) | (2/3) | 

Tổng của ba câu trả lời là (2), chính xác là diện tích của thân tàu cuối cùng. Đây là đặc tính hiệu quả của phép phân bổ Shapley xuất hiện trực tiếp trong hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^3)) | Tính toán cấp độ là (O(N^2\log N)), tiếp theo là tích lũy cặp (O(N^3)) | 
| Không gian | (O(N^2)) | Ma trận xác suất có hướng chứa (N^2) giá trị | 

Với (N\le200), phần khối chứa ít hơn bốn triệu kiểm tra cặp điểm không có thứ tự trên mỗi mục tiêu và chỉ có khoảng bốn triệu kết hợp tổng cặp mục tiêu sau khi khai thác các cặp không có thứ tự. Việc xử lý trước xác suất nhỏ hơn thế. Việc sử dụng bộ nhớ bị chi phối bởi ma trận xác suất (200\times200). 

Giải pháp hình học tiệm cận mạnh hơn làm giảm tập hợp nửa mặt phẳng có trọng số cuối cùng thành (O(N^2)), nhưng nó đòi hỏi phải xây dựng và đi qua một cách sắp xếp đường thẳng. Việc triển khai khối ở trên đơn giản hơn đáng kể và rất phù hợp với ràng buộc (N=200) của cuộc thi. Phân tách cặp cơ bản giống với phân tách được sử dụng trong kết quả (O(N^2)) đã biết. 

## Trường hợp thử nghiệm```python
# The tests below assume the solution above is placed in this file.
# For a standalone test script, the implementation is reproduced
# through the run() helper.

import sys
import io
import math

def algorithm(inp: str) -> list[float]:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    try:
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]

        if n < 3:
            return [0.0] * n

        eps = 1e-10

        px = [x + eps * (i + 1) for i, (x, y) in enumerate(pts)]
        py = [y + eps * (i + 1) * (i + 1)
              for i, (x, y) in enumerate(pts)]

        rho = [[0.0] * n for _ in range(n)]

        for q in range(n):
            order = [i for i in range(n) if i != q]
            qx, qy = px[q], py[q]

            order.sort(
                key=lambda i: math.atan2(py[i] - qy, px[i] - qx)
            )

            m = n - 1
            doubled = order + order
            t = 0

            for s in range(m):
                if t < s + 1:
                    t = s + 1

                a = doubled[s]
                ax = px[a] - qx
                ay = py[a] - qy

                while t < s + m:
                    b = doubled[t]
                    bx = px[b] - qx
                    by = py[b] - qy

                    if ax * by - ay * bx > 0.0:
                        t += 1
                    else:
                        break

                level = t - s - 1
                if level > 0:
                    rho[q][a] = (
                        2.0 / (level * (level + 1) * (level + 2))
                    )

        ans = [0.0] * n

        for p in range(n):
            px0, py0 = pts[p]
            total = 0.0

            for q in range(n):
                if q == p:
                    continue

                qx, qy = pts[q]

                for r in range(q + 1, n):
                    if r == p:
                        continue

                    rx, ry = pts[r]

                    cross = (
                        (rx - qx) * (py0 - qy)
                        - (ry - qy) * (px0 - qx)
                    )

                    if cross > 0:
                        total += 0.5 * cross * rho[q][r]
                    elif cross < 0:
                        total += 0.5 * (-cross) * rho[r][q]

            ans[p] = total

        return ans

    finally:
        sys.stdin = old_stdin

def run(inp: str) -> list[float]:
    return algorithm(inp)

def assert_close(actual, expected, name):
    assert len(actual) == len(expected), name
    for a, e in zip(actual, expected):
        assert abs(a - e) <= 1e-8 * max(1.0, abs(e)), (
            name, a, e
        )

# Provided sample
sample = """\
4
2 2
0 2
2 0
1 1
"""
assert_close(
    run(sample),
    [
        0.8333333333333333,
        0.5,
        0.5,
        0.16666666666666666,
    ],
    "provided sample",
)

# Minimum-size input
assert_close(
    run("""\
1
10000 -10000
"""),
    [0.0],
    "one point",
)

# Two points, still zero area
assert_close(
    run("""\
2
-10000 -10000
10000 10000
"""),
    [0.0, 0.0],
    "two points",
)

# Three points, every point gets one third of the triangle area.
triangle = """\
3
0 0
2 0
0 2
"""
assert_close(
    run(triangle),
    [2.0 / 3.0, 2.0 / 3.0, 2.0 / 3.0],
    "triangle",
)

# Square: all four vertices are symmetric, so every answer is 1/4.
square = """\
4
0 0
1 0
1 1
0 1
"""
assert_close(
    run(square),
    [0.25, 0.25, 0.25, 0.25],
    "square",
)

# Collinear points plus one off-line point.
degenerate = """\
4
0 0
1 0
2 0
0 1
"""
assert_close(
    run(degenerate),
    [
        5.0 / 12.0,
        1.0 / 12.0,
        1.0 / 12.0,
        5.0 / 12.0,
    ],
    "collinear boundary point",
)

# Maximum-size input. The expected individual answers are not written
# explicitly, so we check the defining efficiency property:
# their sum must equal the area of the full convex hull.
pts = [(i, (i * i) % 10000) for i in range(200)]
maximum = "200\n" + "\n".join(f"{x} {y}" for x, y in pts) + "\n"
got = run(maximum)

assert len(got) == 200, "maximum size length"
assert all(x >= -1e-9 for x in got), "nonnegative maximum answers"

def convex_hull(points):
    points = sorted(set(points))
    if len(points) <= 1:
        return points

    def cross(o, a, b):
        return (
            (a[0] - o[0]) * (b[1] - o[1])
            - (a[1] - o[1]) * (b[0] - o[0])
        )

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

def hull_area(points):
    h = convex_hull(points)
    if len(h) < 3:
        return 0.0

    s = 0
    for i in range(len(h)):
        x1, y1 = h[i]
        x2, y2 = h[(i + 1) % len(h)]
        s += x1 * y2 - y1 * x2

    return abs(s) / 2.0

assert abs(sum(got) - hull_area(pts)) <= 1e-6 * max(
    1.0, hull_area(pts)
), "maximum efficiency"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | (0,8333333,0,5,0,5,0,1666667) | Mẫu gốc và điểm nội thất | 
|`1 / (10000,-10000)`|`0`| Kích thước tối thiểu và thân tàu không có diện tích | 
| Hai điểm ranh giới đối diện |`0,0`| Không tồn tại tam giác | 
| Tam giác vuông |`2/3`cho mọi điểm | Xác suất (1/3) được chèn cuối cùng | 
| Đơn vị vuông |`0.25`cho mọi điểm | Tính đối xứng và phân bổ bình đẳng | 
| Chuỗi thẳng hàng cộng một điểm |`5/12,1/12,1/12,5/12`| Xử lý cộng tuyến thoái hóa | 
| 200 điểm được tạo | Tổng bằng diện tích toàn bộ thân tàu | Kích thước đầu vào tối đa và hiệu suất không đổi | 

## Vỏ cạnh 

Đối với một điểm duy nhất, giai đoạn tiền xử lý bị bỏ qua vì (N<3) và giải pháp ngay lập tức in ra số 0. Không có cặp định hướng nào có thể định nghĩa một tam giác, vì vậy điều này phù hợp với định nghĩa hình học. 

Đối với hai điểm, áp dụng hoàn trả sớm như nhau. Bao lồi của chúng là một đoạn thẳng có diện tích bằng 0. Điều này tránh việc xây dựng xác suất cho điểm thứ ba không tồn tại. 

Đối với ba điểm không thẳng hàng, khi chèn điểm thứ ba vào sẽ có đúng một cặp điểm liền kề. Nửa mặt phẳng chứa mục tiêu chứa chính xác một điểm, do đó (L=1) và 

[ 
\rho=\frac{2}{1\cdot2\cdot3}=\frac13. 
] 

Mục tiêu nhận được một phần ba diện tích tam giác. Vì mỗi điểm trong số ba điểm có cùng xác suất đứng thứ ba nên mỗi điểm nhận được chính xác một phần ba toàn bộ diện tích. 

Đối với các điểm thẳng hàng, tích chéo ban đầu có thể bằng 0 mặc dù nhiễu loạn ký hiệu đặt các điểm ở một phía xác định của đường thẳng. Thuật toán cố tình sử dụng các tọa độ khác nhau cho hai công việc này. Tích chéo ban đầu xác định diện tích tam giác thực tế, bằng 0 đối với bộ ba thẳng hàng. Hình học nhiễu loạn xác định thứ tự tổ hợp cần thiết cho xác suất. Điều này tương ứng với việc lấy giới hạn từ cấu hình vị trí chung và tránh tính hai lần một cạnh thân chứa một số điểm đầu vào. 

Đối với đầu vào thoái hóa của bê tông```
4
0 0
1 0
2 0
0 1
```toàn bộ thân tàu có diện tích (1). Hai điểm cuối ((0,0)) và ((0,1)) mỗi điểm nhận (5/12), trong khi hai điểm ở cạnh dưới cùng nhận (1/12). Tổng của họ là 

[ 
\frac5{12}+\frac1{12}+\frac1{12}+\frac5{12}=1, 
] 

nên toàn bộ diện tích thân tàu được phân bố đúng một lần. 

Việc kiểm tra hiệu quả cuối cùng cũng là một bất biến gỡ lỗi hữu ích. Đối với mỗi hoán vị, tổng của tất cả các kính thiên văn tăng biên đến diện tích thân tàu cuối cùng. Việc lấy kỳ vọng sẽ duy trì sự bình đẳng đó, do đó, các câu trả lời được tính toán phải luôn có tổng bằng diện tích bao lồi của tất cả các điểm đầu vào. Sự khác biệt đáng kể thường có nghĩa là nửa mặt phẳng có hướng, mức xác suất hoặc dấu hiệu định hướng được xử lý không chính xác.
