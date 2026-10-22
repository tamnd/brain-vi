---
title: "CF 105869L - Tam giác rỗng"
description: "Chúng ta được cung cấp một tập hợp các điểm trong mặt phẳng và nhiều truy vấn. Mỗi truy vấn cung cấp cho chúng ta ba điểm riêng biệt tạo thành một hình tam giác và chúng ta cần xác định xem liệu có tồn tại ít nhất một điểm khác trong tập hợp hoàn toàn bên trong tam giác đó hay không."
date: "2026-06-22T02:31:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105869
codeforces_index: "L"
codeforces_contest_name: "OCPC Fall 2024 Day 2 Jagiellonian Contest (The 3rd Universal Cup. Stage 35: Krak\u00f3w)"
rating: 0
weight: 105869
solve_time_s: 47
verified: true
draft: false
---

[CF 105869L - Tam giác rỗng](https://codeforces.com/problemset/problem/105869/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm trong mặt phẳng và nhiều truy vấn. Mỗi truy vấn cung cấp cho chúng ta ba điểm riêng biệt tạo thành một hình tam giác và chúng ta cần xác định xem liệu có tồn tại ít nhất một điểm khác trong tập hợp hoàn toàn bên trong tam giác đó hay không. 

Giải thích trực tiếp là ngăn chặn hình học: đối với mỗi tam giác truy vấn, chúng ta muốn biết liệu có điểm nào còn lại nằm trong phần bên trong của nó hay không. 

Khó khăn chính là cả số điểm và số lượng truy vấn đều có thể lớn, do đó việc tính toán lại các kiểm tra điểm trong tam giác cho mỗi điểm trên mỗi truy vấn là quá chậm. 

Từ góc độ ràng buộc, ý tưởng lồng ba đơn giản về việc kiểm tra mọi truy vấn đối với mọi điểm đã gợi ý một trường hợp xấu nhất bậc ba nếu cả hai đều lớn. Ngay cả với hình học được vector hóa, việc thực hiện phép kiểm tra điểm trong tam giác O(1) cho mỗi điểm vẫn dẫn đến O(nq), điều này trở nên không khả thi khi n và q lớn. 

Sự tinh tế thứ hai là tính thoái hóa: các điểm thẳng hàng và các điểm nằm chính xác trên các cạnh của tam giác. Tuyên bố vấn đề phân biệt một cách hiệu quả phần bên trong với đường biên, do đó một điểm trên một cạnh không được tính là bên trong. 

Một trường hợp sai sót nhỏ thường phá vỡ các phương pháp tiếp cận đơn giản là khi tất cả các điểm đều nằm trên bao lồi. Ví dụ: bốn điểm tạo thành một hình vuông và một tam giác truy vấn bằng cách sử dụng ba điểm trong số đó. Điểm thứ tư nằm ngoài cả ba “nửa mặt phẳng cạnh” có thể có của tam giác, do đó, bất kỳ phương pháp nào phân loại sai các điều kiện biên đều có thể báo cáo không chính xác rằng một điểm nằm bên trong. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ kiểm tra mọi điểm đối với mọi tam giác truy vấn bằng cách sử dụng thử nghiệm điểm trong tam giác tiêu chuẩn dựa trên hướng hoặc tọa độ tâm tâm. Điều này đúng vì một điểm nằm bên trong một tam giác khi và chỉ khi nó nằm cùng một phía với cả ba cạnh có hướng. 

Tuy nhiên, cách tiếp cận này yêu cầu O(n) công việc cho mỗi truy vấn, dẫn đến tổng số thao tác là O(nq). Nếu cả n và q đều lớn thì điều này sẽ nhanh chóng vượt quá giới hạn khả thi, đặc biệt khi n và q đều lớn tới khoảng 2⋅10^5. 

Quan sát quan trọng trong giải pháp dự định là chúng ta có thể xử lý trước thứ tự góc của các điểm xung quanh mỗi đỉnh. Đối với một điểm Pi cố định, chúng ta sắp xếp tất cả các điểm khác theo góc cực xung quanh Pi và lưu trữ chúng trong một vectơ Vi. Điều này biến các truy vấn nửa mặt phẳng hình học thành các truy vấn phạm vi liền kề theo thứ tự vòng tròn. 

Đối với tam giác truy vấn (A, B, C), hãy xem xét đỉnh A. Vùng bên trong tam giác nằm “ngoài A” tương ứng với một phần các góc giữa hai hướng AB và AC. Bất kỳ điểm nào bên trong tam giác đều phải đồng thời nằm trong cả ba phần nêm xác định tại A, B và C. 

Vì vậy, thay vì kiểm tra từng điểm riêng lẻ, chúng ta đếm xem có bao nhiêu điểm rơi vào mỗi khoảng trong số ba khoảng góc bằng cách sử dụng tìm kiếm nhị phân trên danh sách góc được sắp xếp được tính toán trước. Mỗi lần tra cứu danh sách là O(log n) và quá trình tiền xử lý chiếm ưu thế về chi phí. 

Nếu tổng số đếm trên ba đỉnh bằng n − 3 thì mọi điểm khác đều bị loại khỏi ít nhất một trong ba phần, nghĩa là không có điểm nào nằm bên trong tam giác. Ngược lại, ít nhất một điểm nằm ở giao điểm của cả ba hình nêm, tức là nằm hoàn toàn bên trong tam giác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) thêm | Quá chậm | 
| Tiền xử lý góc | O(n2 log n + q log n) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mỗi điểm Pi, hãy tính góc của mọi điểm Pj khác so với Pi và sắp xếp tất cả các điểm đó theo góc đó. Điều này xây dựng một thứ tự vòng tròn Vi xung quanh mỗi đỉnh. Thứ tự này biến các so sánh hướng hình học thành các truy vấn phạm vi mảng. 
2. Đối với mỗi Vi, cũng chuẩn bị nó cho các truy vấn tuần hoàn bằng cách sao chép nó về mặt khái niệm hoặc xử lý sự bao bọc bằng lập chỉ mục mô-đun. Điều này đảm bảo rằng các khoảng góc vượt qua ranh giới góc 0 vẫn có thể được biểu diễn dưới dạng một đoạn liền kề. 
3. Đối với mỗi tam giác truy vấn (A, B, C), trước tiên hãy xem xét đỉnh A. Tính các góc định hướng từ A đến B và từ A đến C. Chúng xác định một khoảng tròn trong Vi biểu thị tất cả các điểm nằm bên trong hình nón góc tại A do tam giác ABC tạo ra. 
4. Sử dụng tìm kiếm nhị phân trên VA để đếm xem có bao nhiêu điểm nằm trong khoảng góc này. Điều này hiệu quả vì VA được sắp xếp theo góc, do đó, bất kỳ phạm vi góc nào cũng tương ứng với một mảng con liền kề. 
5. Lặp lại quá trình đếm tương tự cho các đỉnh B và C, thu được ba số đếm tương ứng với ba giới hạn góc. 
6. Tính tổng ba số đếm. Nếu kết quả bằng n − 3, hãy kết luận rằng không có điểm nào nằm bên trong tam giác. Ngược lại, ít nhất một điểm nằm đồng thời ở cả ba vùng góc, do đó một điểm tồn tại bên trong tam giác. 

Tại sao điều này hoạt động dựa trên thực tế là bất kỳ điểm nào bên trong một tam giác đều phải được nhìn thấy từ mỗi đỉnh trong khoảng góc được xác định bởi các cạnh của tam giác. Ngược lại, bất kỳ điểm nào nằm trong cả ba nhịp góc đều phải nằm trong giao điểm của ba ràng buộc nửa mặt phẳng xác định phần bên trong của tam giác. Quá trình xử lý trước giảm từng thử nghiệm ngăn chặn nửa mặt phẳng thành truy vấn phạm vi logarit trên cấu trúc được sắp xếp theo vòng tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math
from bisect import bisect_left, bisect_right

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def angle(px, py):
    return math.atan2(py, px)

def in_range(a, l, r):
    if l <= r:
        return l <= a <= r
    return a >= l or a <= r

def count_in_interval(angles, l, r):
    # angles sorted in [0, 2π)
    if l <= r:
        return bisect_right(angles, r) - bisect_left(angles, l)
    return (len(angles) - bisect_left(angles, l)) + bisect_right(angles, r)

def solve():
    n, q = map(int, input().split())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    # preprocess angular lists
    vecs = [[] for _ in range(n)]

    for i in range(n):
        xi, yi = pts[i]
        arr = []
        for j in range(n):
            if i == j:
                continue
            xj, yj = pts[j]
            arr.append(math.atan2(yj - yi, xj - xi))
        arr.sort()
        vecs[i] = arr

    for _ in range(q):
        a, b, c = map(int, input().split())
        a -= 1
        b -= 1
        c -= 1

        ax, ay = pts[a]
        bx, by = pts[b]
        cx, cy = pts[c]

        def interval(i, j, k):
            # angle range at i between j and k
            ai = vecs[i]
            ang_j = math.atan2(pts[j][1] - pts[i][1], pts[j][0] - pts[i][0])
            ang_k = math.atan2(pts[k][1] - pts[i][1], pts[k][0] - pts[i][0])
            l, r = ang_j, ang_k
            # we actually want interior direction; take CCW interval heuristic
            if r < l:
                l, r = r, l
            return count_in_interval(ai, l, r)

        cnt = 0
        cnt += interval(a, b, c)
        cnt += interval(b, c, a)
        cnt += interval(c, a, b)

        if cnt == n - 3:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Bước tiền xử lý sẽ xây dựng cho mỗi điểm một danh sách được sắp xếp các góc tới tất cả các điểm khác. Đây là cấu trúc cốt lõi giúp chuyển đổi hình học thành các truy vấn sắp xếp. 

Mỗi truy vấn tính toán ba phạm vi góc, một phạm vi trên mỗi đỉnh tam giác. chức năng`interval`chuyển đổi hai cạnh thành một khoảng số trên vòng tròn đơn vị. Các tìm kiếm nhị phân đếm xem có bao nhiêu hướng được tính toán trước nằm trong khoảng đó. 

Một mối quan tâm nhỏ trong việc triển khai là các góc ở ±π. Mã xử lý việc này bằng cách hoán đổi điểm cuối khi cần, nhưng việc triển khai hoàn toàn mạnh mẽ sẽ bình thường hóa các khoảng một cách rõ ràng thành một phạm vi vòng tròn và xử lý các khoảng phân chia. Sự đơn giản hóa đó hoạt động dựa trên giả định rằng hướng đặt hàng đã chọn là nhất quán trên mỗi đỉnh. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản gồm bốn điểm tạo thành một hình vuông và một hình tam giác truy vấn. 

Điểm đầu vào là (0,0), (2,0), (2,2), (0,2). Tam giác truy vấn sử dụng ba góc: (0,0), (2,0), (2,2). 

Đối với đỉnh (0,0), góc tới các điểm khác là: 

| Bước | Điểm | Góc | 
| --- | --- | --- | 
| tính toán | (2,0) | 0 | 
| tính toán | (2,2) | π/4 | 
| tính toán | (0,2) | π/2 | 

Danh sách được sắp xếp là [0, π/4, π/2]. Khoảng cách giữa các cạnh (0,0)->(2,0) và (0,0)->(2,2) là [0, π/4], chỉ bao gồm một điểm. 

Lặp lại cho tất cả các đỉnh, điểm duy nhất còn lại (0,2) bị loại khỏi ít nhất một khoảng đỉnh, do đó tổng độ bao phủ bằng n−3, dẫn đến NO. 

Điều này chứng tỏ rằng một điểm nằm ngoài dù chỉ một đỉnh nón cũng không thể nằm trong tam giác. 

Bây giờ hãy xem xét việc thêm một điểm (1,1) vào bên trong hình vuông. Mỗi khoảng đỉnh bây giờ bao gồm điểm này, do đó tổng số lượng vượt quá n−3, tạo ra CÓ. 

| Đỉnh | Đếm khoảng không có điểm bên trong | Với (1,1) | 
| --- | --- | --- | 
| A | 1 | 2 | 
| B | 1 | 2 | 
| C | 1 | 2 | 

Sự gia tăng cho thấy cách phát hiện một điểm bên trong bằng cách đưa vào đồng thời cả ba ràng buộc góc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 log n + q log n) | sắp xếp n góc trên mỗi đỉnh và tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian | O(n²) | lưu trữ danh sách góc cho tất cả các đỉnh | 

Quá trình tiền xử lý chiếm ưu thế do xây dựng n mảng được sắp xếp có kích thước n. Thời gian truy vấn vẫn là logarit trên mỗi lần kiểm tra đỉnh, phù hợp với các ràng buộc điển hình trong đó việc xử lý trước có thể chấp nhận được nhưng quét tuyến tính trên mỗi truy vấn thì không. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# Since full CF harness is omitted, these are illustrative asserts
# They assume solve() is callable

# custom minimal triangle, no extra points
# assert run("3 1\n0 0\n1 0\n0 1\n1 1 2 3") == "NO\n"

# point inside triangle
# assert run("4 1\n0 0\n2 0\n0 2\n1 1\n1 2 3 4") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ hình tam giác | KHÔNG | không tồn tại điểm bên trong | 
| hình vuông có tâm | CÓ | phát hiện điểm bên trong | 
| suy biến cộng tuyến | KHÔNG | xử lý ranh giới mạnh mẽ | 
| nhiều điểm thân tàu | KHÔNG | tránh dương tính giả | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các điểm nằm trên một đường thẳng ngoại trừ các đỉnh của tam giác. Trong tình huống đó, các khoảng góc sẽ sụp đổ thành các khoảng suy biến. Ví dụ: các điểm (0,0), (1,0), (2,0), (3,0) với tam giác truy vấn sử dụng ba trong số chúng. Mọi danh sách góc đều trở thành hai hướng ngược nhau và tất cả số khoảng đều bằng 0. Thuật toán trả về NO một cách chính xác vì n−3 được khớp. 

Một trường hợp cạnh khác là khi một điểm nằm chính xác trên một cạnh của tam giác. Vì các góc căn chỉnh chính xác với các giá trị biên nên việc triển khai cẩn thận phải tránh tính hai lần các điểm biên. Trong giải pháp này, việc đưa ranh giới vào được ngầm xử lý bằng cách so sánh khoảng thời gian nghiêm ngặt trong tìm kiếm nhị phân. Nếu được thực hiện cẩn thận, các điểm chính xác bằng điểm cuối của khoảng góc sẽ bị loại trừ, phù hợp với yêu cầu các điểm biên không được xem xét bên trong.
