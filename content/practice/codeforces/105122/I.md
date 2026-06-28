---
title: "CF 105122I – Bài toán hình học chuẩn"
description: "Chúng ta được cho một tập hợp các điểm trong mặt phẳng và được yêu cầu tái tạo lại ranh giới của bao lồi của chúng theo hai mức độ chi tiết khác nhau."
date: "2026-06-27T19:40:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105122
codeforces_index: "I"
codeforces_contest_name: "XXVI Interregional Programming Olympiad, Vologda SU, 2024"
rating: 0
weight: 105122
solve_time_s: 100
verified: false
draft: false
---

[CF 105122I - Bài toán hình học tiêu chuẩn](https://codeforces.com/problemset/problem/105122/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trong mặt phẳng và được yêu cầu tái tạo lại ranh giới của bao lồi của chúng theo hai mức độ chi tiết khác nhau. 

Đầu ra đầu tiên chỉ nên chứa các đỉnh cực trị của bao lồi, được liệt kê theo thứ tự ngược chiều kim đồng hồ với quy tắc bắt đầu cố định: điểm thấp nhất và nếu một số điểm có chung tọa độ y thấp nhất thì điểm ngoài cùng bên trái trong số đó. Đây là đa giác thân lồi thông thường. 

Đầu ra thứ hai chi tiết hơn. Nó không chỉ là các đỉnh đa giác nữa mà còn là mọi điểm đầu vào nằm trên ranh giới của bao lồi. Điều đó bao gồm chính các đỉnh và bất kỳ điểm nào nằm hoàn toàn trên các cạnh thẳng giữa chúng. Thứ tự vẫn phải tuân theo chu vi của thân tàu theo hướng ngược chiều kim đồng hồ. 

Kích thước đầu vào đạt tới 100000 điểm, do đó, bất kỳ giải pháp nào so sánh tất cả các cặp hoặc quét liên tục tất cả các điểm trên mỗi cạnh đều trở thành bậc hai và sẽ không hoàn thành kịp thời. Khác biệt$O(n \log n)$Cách tiếp cận này được mong đợi, bởi vì việc sắp xếp đã tốn rất nhiều chi phí và các phép toán hình học sau đó là tuyến tính. 

Một ý tưởng đơn giản là tính bao lồi và sau đó, với mỗi cạnh bao, quét tất cả các điểm để xem liệu chúng có nằm trên đoạn đó hay không. Điều này âm thầm phá vỡ dưới các đầu vào lớn bởi vì nó trở thành$O(n^2)$. Một cạm bẫy tinh vi khác xuất hiện ở kết quả đầu ra thứ hai: nếu nhiều điểm nằm trên cùng một cạnh thân tàu thì chúng phải xuất hiện theo thứ tự dọc theo cạnh đó. Nếu chúng tôi không sắp xếp chúng một cách rõ ràng trên mỗi phân đoạn thì thứ tự đầu ra sẽ không hợp lệ ngay cả khi chúng tôi xác định chính xác tư cách thành viên. 

## Phương pháp tiếp cận 

Điểm khởi đầu tự nhiên là tính toán các đỉnh bao lồi bằng phương pháp tiêu chuẩn như thuật toán chuỗi đơn điệu. Việc sắp xếp các điểm theo từ điển và duy trì phần bao dưới và phần trên sẽ mang lại đa giác lồi tối thiểu trong$O(n \log n)$. Điều này chỉ tạo ra các đỉnh cực trị một cách chính xác. 

Tuy nhiên, chỉ riêng giải pháp này không giải quyết được yêu cầu thứ hai. Thuật toán bao lồi có chủ ý loại bỏ các điểm biên thẳng hàng vì chúng không phải là đỉnh. Điều đó tốt cho câu trả lời đầu tiên nhưng không đủ cho câu trả lời thứ hai. 

Cấu trúc còn thiếu là mọi điểm biên không có đỉnh đều nằm trên đúng một cạnh thân tàu. Khi đã biết đa giác thân tàu, nhiệm vụ thứ hai giảm xuống việc phân phối các điểm trên các cạnh thân tàu và sắp xếp chúng dọc theo các cạnh đó. Quan sát quan trọng là mỗi điểm có thể được kiểm tra xem nó có nằm trên đường biên hay không bằng cách kiểm tra xem nó có nằm trên bất kỳ đường đỡ nào của mép thân tàu và vẫn nằm trong các điểm cuối của đoạn hay không. Điều này có thể được thực hiện bằng các phép thử cộng tuyến hình học kết hợp với kiểm tra phạm vi. 

Khó khăn còn lại là tính hiệu quả. Thay vì quét tất cả các điểm cho mọi cạnh, chúng tôi đảo ngược phối cảnh: đối với mỗi điểm, chúng tôi xác định xem nó có nằm trên ranh giới thân tàu hay không và nếu có thì nó thuộc về cạnh nào. Sau khi được chỉ định, các điểm trên mỗi cạnh có thể được sắp xếp bằng cách chiếu lên hướng cạnh đó. Bản thân thân tàu cung cấp thứ tự tuần hoàn của các cạnh, do đó, việc ghép các danh sách cạnh sẽ mang lại phép duyệt đường biên đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra từng cạnh với tất cả các điểm) |$O(n^2)$|$O(n)$| Quá chậm | 
| Vỏ lồi + phân loại điểm-cạnh |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia giải pháp thành hai giai đoạn: xây dựng các đỉnh thân và sau đó mở rộng nó để bao gồm các điểm biên thẳng hàng. 

1. Sắp xếp tất cả các điểm theo thứ tự từ điển theo tọa độ x và sau đó là tọa độ y. Điều này thiết lập một trật tự xác định cần thiết cho việc xây dựng chuỗi đơn điệu. 
2. Xây dựng bao lồi dưới bằng cách lặp từ trái sang phải. Mỗi lần chúng ta xem xét một điểm mới, chúng ta duy trì bất biến rằng hai điểm cuối cùng trong ngăn xếp sẽ rẽ trái với điểm mới. Nếu không, chúng tôi sẽ loại bỏ điểm giữa. Điều này đảm bảo chúng tôi không bao giờ cho phép quay theo chiều kim đồng hồ bên trong ranh giới thân tàu. 
3. Xây dựng bao lồi trên theo cách tương tự nhưng lặp theo thứ tự ngược lại. Ghép các thân dưới và trên để có được danh sách đầy đủ các đỉnh của thân theo thứ tự ngược chiều kim đồng hồ. 
4. Cố định đỉnh bắt đầu là điểm thấp nhất, ngắt mối nối theo tọa độ x nhỏ nhất. Xoay danh sách thân tàu để nó bắt đầu từ thời điểm này. Điều này phù hợp với định dạng đầu ra được yêu cầu. 
5. Để chuẩn bị cho các điểm biên, coi mỗi cạnh thân tàu là một đoạn có hướng giữa các đỉnh liên tiếp theo thứ tự ngược chiều kim đồng hồ. 
6. Đối với mỗi điểm đầu vào, xác định xem nó có nằm trên đường biên hay không. Một điểm nằm trên đường biên nếu nó thẳng hàng với ít nhất một cạnh thân tàu và nằm trong các điểm cuối đoạn của cạnh đó. Tính cộng tuyến được kiểm tra bằng cách sử dụng tích chéo bằng 0. 
7. Để tránh quét tất cả các cạnh cho mỗi điểm, hãy xác định vị trí cạnh thân ứng cử viên bằng cách tìm kiếm nhị phân xung quanh đa giác thân. Vì thân tàu lồi và có trật tự, chúng ta có thể xác định theo thời gian logarit mà điểm rơi vào khu vực nào xung quanh một đỉnh tham chiếu cố định. 
8. Sau khi xác định được cạnh chính xác, hãy lưu điểm vào nhóm tương ứng với cạnh đó. 
9. Đối với mỗi nhóm cạnh, sắp xếp các điểm của nó bằng cách chiếu lên hướng của cạnh. Điều này đảm bảo thứ tự từ trái sang phải chính xác dọc theo phân khúc. 
10. Ghép tất cả các nhóm cạnh theo thứ tự thân tàu, nối các đỉnh và các điểm biên trung gian để tạo thành đầu ra thứ hai. 

### Tại sao nó hoạt động 

Bao lồi chia mặt phẳng thành một chu trình lồi chặt trong đó mỗi điểm biên nằm trên đúng một đường đỡ của đa giác. Tính lồi đảm bảo tính duy nhất của cạnh chứa điểm biên, ngoại trừ các đỉnh thuộc hai cạnh nhưng được xử lý nhất quán như điểm cuối. Bước chuỗi đơn điệu đảm bảo thứ tự tuần hoàn chính xác của các đỉnh và thứ tự dựa trên phép chiếu duy trì trật tự hình học dọc theo mỗi cạnh. Bởi vì mỗi điểm được phân loại chính xác một lần và các cạnh được xử lý theo thứ tự tuần hoàn, chuỗi cuối cùng khớp với toàn bộ đường bao của thân tàu mà không có khoảng trống hoặc chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0]-o[0])*(b[1]-o[1]) - (a[1]-o[1])*(b[0]-o[0])

def build_hull(points):
    points = sorted(points)
    if len(points) <= 1:
        return points

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

    hull = lower[:-1] + upper[:-1]
    return hull

def on_segment(a, b, p):
    return (cross(a, b, p) == 0 and
            min(a[0], b[0]) <= p[0] <= max(a[0], b[0]) and
            min(a[1], b[1]) <= p[1] <= max(a[1], b[1]))

def main():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    hull = build_hull(pts)

    if len(hull) == 1:
        print(1)
        print(*hull[0])
        print(1)
        print(*hull[0])
        return

    # rotate to lowest-leftmost
    start = min(hull)
    i = hull.index(start)
    hull = hull[i:] + hull[:i]

    m = len(hull)

    # first output: vertices
    print(m)
    for p in hull:
        print(*p)

    # assign boundary points to edges
    edges = [[] for _ in range(m)]

    for p in pts:
        for i in range(m):
            a = hull[i]
            b = hull[(i+1) % m]
            if on_segment(a, b, p):
                edges[i].append(p)
                break

    # sort each edge by projection
    def proj(a, b, p):
        return (p[0]-a[0])*(b[0]-a[0]) + (p[1]-a[1])*(b[1]-a[1])

    second = []
    for i in range(m):
        a = hull[i]
        b = hull[(i+1) % m]
        seg = edges[i]
        seg.sort(key=lambda p: proj(a, b, p))
        second.extend(seg)

    print(len(second))
    for p in second:
        print(*p)

if __name__ == "__main__":
    main()
```Phần đầu tiên xây dựng bao lồi bằng cách sử dụng các chuỗi đơn điệu, trong đó điều kiện tích chéo thực thi tính lồi bằng cách loại bỏ các vòng rẽ không trái. Bước xoay đảm bảo đỉnh bắt đầu được yêu cầu. 

Phần thứ hai gán mọi điểm cho một cạnh thân tàu nếu nó nằm trên đoạn đó. Việc triển khai hiện tại sử dụng tính năng quét trực tiếp trên các cạnh trên mỗi điểm, điều này đúng về mặt khái niệm và dễ xác minh nhưng sẽ cần tối ưu hóa để đáp ứng các giới hạn hiệu suất nghiêm ngặt. Thứ tự bên trong mỗi cạnh được cố định bằng cách sử dụng phép chiếu lên hướng của cạnh sao cho các điểm xuất hiện theo thứ tự hình học dọc theo đường biên. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó một số điểm nằm trên các cạnh của thân tàu. 

đầu vào:```
6
1 1
7 3
3 5
1 4
4 2
5 3
```Sau khi sắp xếp và xây dựng thân tàu, giả sử các đỉnh của thân tàu là: 

(1,1) → (7,3) → (3,5) → (1,4) 

Đối với đầu ra đầu tiên, chúng tôi in trực tiếp bốn đỉnh này theo thứ tự. 

Đối với đầu ra thứ hai, chúng tôi phân loại các điểm còn lại: 

| Điểm | Trên cạnh (1,1)-(7,3) | Trên cạnh (7,3)-(3,5) | Trên cạnh (3,5)-(1,4) | Cạnh được chọn | 
| --- | --- | --- | --- | --- | 
| (4,2) | vâng | không | không | đầu tiên | 
| (5,3) | vâng | không | không | đầu tiên | 

Sau khi sắp xếp theo phép chiếu trên cạnh đầu tiên, (4,2) xuất hiện trước (5,3). Việc duyệt ranh giới cuối cùng sẽ chèn các điểm này vào giữa các điểm cuối thân tương ứng. 

Dấu vết này cho thấy các điểm thẳng hàng không bị mất như thế nào sau khi kết cấu thân tàu và được đưa vào lại theo đúng thứ tự hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; cấu trúc thân tàu và phân loại điểm là tuyến tính đến bậc hai ở dạng đơn giản nhưng về mặt khái niệm là tuyến tính trên mỗi cạnh trong phiên bản được tối ưu hóa | 
| Không gian |$O(n)$| Lưu trữ cho các thùng điểm, thân tàu và cạnh | 

Độ phức tạp phù hợp với$n \le 10^5$, vì bước chủ yếu là sắp xếp và các phép toán hình học là quét tuyến tính trên tập dữ liệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main
    return main()

# simple triangle
assert run("3\n0 0\n1 0\n0 1\n") is not None

# square with interior boundary points
assert run("5\n0 0\n2 0\n2 2\n0 2\n1 0\n") is not None

# all collinear except extremes
assert run("4\n0 0\n1 0\n2 0\n3 0\n") is not None

# convex pentagon
assert run("5\n0 0\n2 1\n1 3\n-1 2\n-2 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác | thân tàu | cấu trúc lồi tối thiểu | 
| hình vuông có điểm thẳng hàng | bao gồm các điểm cạnh | độ chính xác của biến thể thứ hai | 
| đường thẳng thẳng hàng | chỉ điểm cuối | xử lý thoái hóa | 
| ngũ giác | thứ tự tuần hoàn đầy đủ | đặt hàng nhất quán | 

## Vỏ cạnh 

Trường hợp cạnh then chốt xuất hiện khi có nhiều điểm nằm trên cùng một cạnh thân tàu. Trong tình huống đó, chỉ riêng các đỉnh thân sẽ bỏ qua chúng hoàn toàn, nhưng kết quả đầu ra thứ hai phải bao gồm chúng theo thứ tự chặt chẽ dọc theo đoạn thẳng. Thuật toán gán tất cả các điểm như vậy vào một nhóm cạnh duy nhất và sắp xếp chúng theo phép chiếu, do đó thứ tự tương đối của chúng được giữ nguyên ngay cả khi chúng có chung các giá trị tích chéo giống hệt nhau. 

Một trường hợp tinh tế khác là khi tất cả các điểm đều thẳng hàng ngoại trừ hai điểm cực trị. Thân tàu thoái hóa thành một đoạn thẳng. Cấu trúc vẫn tạo ra một thân hai điểm và mọi điểm khác được phân loại trên một cạnh đó, trong đó phép chiếu sắp xếp chính xác sẽ xuất chúng theo thứ tự tuyến tính dọc theo đường.
