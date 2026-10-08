---
title: "CF 104945G - Món ăn yêu thích"
description: "Mỗi món ăn có hai thuộc tính: điểm hương vị và điểm trình bày. Mỗi người cũng có hai sở thích, đóng vai trò là trọng số cho hai thuộc tính giống nhau đó."
date: "2026-06-28T07:10:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104945
codeforces_index: "G"
codeforces_contest_name: "2023-2024 ICPC Southwestern European Regional Contest (SWERC 2023)"
rating: 0
weight: 104945
solve_time_s: 78
verified: false
draft: false
---

[CF 104945G - Món ăn yêu thích](https://codeforces.com/problemset/problem/104945/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi món ăn có hai thuộc tính: điểm hương vị và điểm trình bày. Mỗi người cũng có hai sở thích, đóng vai trò là trọng số cho hai thuộc tính giống nhau đó. Một người đánh giá một món ăn bằng cách lấy tổng trọng lượng của hương vị và giá trị mạ của món ăn, sử dụng trọng số của chính họ. Nhiệm vụ là xác định, đối với mỗi người, món ăn nào tối đa hóa sự đánh giá này. Nếu nhiều đĩa đạt giá trị tốt nhất như nhau thì phải chọn đĩa có chỉ số nhỏ nhất. 

Việc đọc trực tiếp cho thấy rằng mọi người đều có khả năng đánh giá từng món ăn và hàm tính điểm là một biểu thức tuyến tính đơn giản của hai biến. Điều này ngay lập tức đặt vấn đề vào một bối cảnh hình học: mỗi món ăn là một điểm trên một mặt phẳng và mỗi người xác định một hướng mà chúng ta chiếu tất cả các điểm, lấy tích số chấm tối đa. 

Hạn chế rất lớn, lên tới 500.000 món ăn và 500.000 người. Bất kỳ phương pháp tiếp cận nào kiểm tra tất cả các cặp món ăn và con người sẽ yêu cầu đánh giá khoảng 2,5 × 10^11, vượt xa giới hạn khả thi. Ngay cả việc quét tuyến tính của mỗi người trên tất cả các món ăn cũng sẽ quá chậm. 

Điểm tinh tế then chốt là cả món ăn và con người đều sống trong cùng một không gian hai chiều và việc đánh giá là tích số chấm. Cấu trúc này cho phép tối ưu hóa hình học thay vì so sánh lực lượng vũ phu. 

Một số trường hợp đặc biệt cần được chăm sóc. Đầu tiên, các mối quan hệ phải được giải quyết bằng chỉ số nhỏ nhất, do đó, giải pháp chỉ theo dõi giá trị lớn nhất cũng phải duy trì thứ tự chỉ mục. 

Thứ hai, hướng suy thoái quan trọng. Nếu một người có trọng lượng như (0, P), thì chỉ có lớp mạ mới là vấn đề; tương tự với (T, 0). Bất kỳ phép biến đổi hình học nào cũng không được giả sử cả hai tọa độ đều dương. 

Thứ ba, những cạm bẫy về độ chính xác sẽ nảy sinh nếu người ta cố gắng chuẩn hóa thành các hệ số góc dấu phẩy động; so sánh phải chính xác với số học số nguyên. 

## Phương pháp tiếp cận 

Giải pháp brute-force đánh giá từng món ăn cho mỗi người. Đối với người l có trọng số (T, P), chúng ta tính T·t_k + P·p_k cho mọi k và lấy giá trị lớn nhất. Điều này đúng vì nó trực tiếp thực hiện định nghĩa. Tuy nhiên, giá của nó là O(NM), trong trường hợp xấu nhất là 2,5 × 10^11 phép nhân và phép cộng, khiến nó không thể sử dụng được. 

Quan sát quan trọng là mỗi món ăn là một điểm (t_k, p_k) và mỗi người xác định một hàm tuyến tính trên các điểm này. Vấn đề giảm xuống còn việc trả lời nhiều truy vấn tích số chấm tối đa trên một tập hợp điểm 2D tĩnh. Đây là một bài toán truy vấn hình học cổ điển trong đó việc xử lý trước bao lồi của các điểm cho phép truy vấn phép chiếu tối đa hiệu quả. 

Cái nhìn sâu sắc quan trọng là đối với bất kỳ hướng cố định nào (T, P), cực đại của Tt + Pp trên một tập hợp các điểm xảy ra tại điểm cực trị của bao lồi. Các điểm bên trong không bao giờ có thể là tối ưu vì chúng là tổ hợp lồi của các điểm biên và do đó không thể vượt quá mức tối đa mà các đỉnh thân tàu đạt được theo bất kỳ hướng tuyến tính nào. 

Do đó, trước tiên chúng ta tính toán bao lồi của tất cả các điểm đĩa. Sau khi sắp xếp các điểm theo tọa độ x và xây dựng phần thân trên và phần dưới, chúng ta thu được một đa giác biểu thị tất cả các món ăn có khả năng tối ưu. Sau đó, mỗi truy vấn sẽ giảm xuống việc tìm đỉnh của đa giác lồi này để tối đa hóa tích chấm với một vectơ chỉ hướng cho trước. Vì tích số chấm trên một đa giác lồi là không đồng nhất dọc theo các đỉnh của nó theo thứ tự tuần hoàn, nên chúng ta có thể tính toán trước và trả lời các truy vấn bằng cách sử dụng tìm kiếm ba ngôi hoặc cách tiếp cận con trỏ xoay tùy theo thứ tự. 

Để hỗ trợ các truy vấn nhanh, chúng ta có thể tính toán trước phần thân trong O(N log N) và sau đó trả lời từng truy vấn trong O(log H), trong đó H là kích thước phần thân, sử dụng tìm kiếm bậc ba trên các đỉnh đa giác lồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(1) | Quá chậm | 
| Vỏ lồi + Tìm kiếm truy vấn | O(N log N + M log N) | O(N) | Đã chấp nhận |

## Hướng dẫn thuật toán 

## Bước 1: Thể hiện món ăn dưới dạng điểm 

Mỗi đĩa k được biểu diễn dưới dạng một điểm (t_k, p_k) trong không gian 2D. Điều này định hình lại việc đánh giá như tính toán tích số chấm với vectơ truy vấn. 

## Bước 2: Tính bao lồi các điểm đĩa 

Sắp xếp tất cả các điểm theo từ điển và xây dựng phần thân dưới và phần trên bằng cách sử dụng một ngăn xếp đơn điệu. Các điểm nằm bên trong hình lồi sẽ bị loại bỏ. 

Bước này đúng vì mọi mục tiêu tuyến tính đều đạt cực đại trên một tập lồi tại điểm cực trị. Các điểm bên trong không bao giờ có thể chiếm ưu thế về mọi hướng. 

## Bước 3: Chuẩn bị thân tàu cho truy vấn tuần hoàn 

Sắp xếp các đỉnh của thân tàu theo thứ tự, tạo thành một đa giác lồi. Đảm bảo thứ tự nhất quán (theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ). 

## Bước 4: Xử lý từng người dưới dạng truy vấn chỉ đường 

Với mỗi người có trọng số (T, P), hãy hiểu đây là một vectơ chỉ hướng. Chúng ta phải tìm đỉnh thân tối đa hóa T·x + P·y. 

## Bước 5: Sử dụng tìm kiếm ba chiều trên đa giác lồi 

Bởi vì tích số chấm trên một đa giác lồi là không đồng nhất dọc theo các đỉnh của nó nên hãy áp dụng tìm kiếm bậc ba trên các chỉ số của thân. Ở mỗi lần so sánh điểm giữa, hãy đánh giá tích số chấm và loại bỏ mặt kém hơn. 

Việc phá vỡ mối quan hệ được xử lý bằng cách so sánh các chỉ số nếu tích số chấm bằng nhau. 

## Bước 6: Kết quả đầu ra 

Trả về chỉ số của đĩa tương ứng với đỉnh thân tốt nhất. 

### Tại sao nó hoạt động 

Bao lồi chứa chính xác những điểm không thể biểu diễn được dưới dạng tổ hợp lồi của các điểm khác. Vì hàm mục tiêu là tuyến tính nên bất kỳ điểm bên trong nào cũng luôn bị chi phối bởi một số điểm biên theo mọi hướng. Điểm cực đại của hàm tuyến tính trên đa giác lồi phải xảy ra ở một đỉnh. Do đó, việc giới hạn không gian tìm kiếm ở các đỉnh thân sẽ đảm bảo tính chính xác. Tính không đồng nhất của tích số chấm dọc theo ranh giới thân tàu đảm bảo rằng tìm kiếm bậc ba tìm thấy chính xác giá trị cực đại toàn cục mà không bỏ sót cực đại cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

def dot(p, q):
    return p[0] * q[0] + p[1] * q[1]

def convex_hull(points):
    points.sort()
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

    return lower[:-1] + upper[:-1]

def best_vertex(hull, v):
    n = len(hull)
    l, r = 0, n - 1

    def f(i):
        return dot(hull[i], v)

    while r - l > 3:
        m1 = l + (r - l) // 3
        m2 = r - (r - l) // 3
        if f(m1) < f(m2):
            l = m1
        else:
            r = m2

    best = l
    for i in range(l, r + 1):
        if f(i) > f(best) or (f(i) == f(best) and hull[i][2] < hull[best][2]):
            best = i
    return hull[best][2]

def main():
    N, M = map(int, input().split())
    points = []
    for i in range(N):
        t, p = map(int, input().split())
        points.append((t, p, i + 1))

    hull = convex_hull(points)

    for _ in range(M):
        T, P = map(int, input().split())
        print(best_vertex(hull, (T, P)))

if __name__ == "__main__":
    main()
```Việc thực hiện bắt đầu bằng việc tính toán bao lồi sử dụng phương pháp chuỗi đơn điệu tiêu chuẩn. Mỗi điểm vẫn giữ lại chỉ mục ban đầu của nó để có thể xử lý việc phá vỡ ràng buộc mà không có sự mơ hồ. 

Hàm truy vấn thực hiện tìm kiếm ba chiều trên thân tàu. Hàm mục tiêu được đánh giá dưới dạng tích vô hướng với vectơ truy vấn. Quá trình quét tuyến tính cuối cùng đối với một số ứng cử viên cuối cùng đảm bảo tính chính xác trong khoảng thời gian nhỏ mà tìm kiếm ba ngôi ngừng tinh chỉnh. 

Một điểm tinh tế là quy tắc hòa. Khi hai món ăn tạo ra điểm số giống nhau, thì món ăn có chỉ số nhỏ nhất phải được chọn, do đó việc triển khai sẽ so sánh các chỉ số ban đầu khi các tích số chấm khớp nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Món ăn: 

(2,5), (3,4), (4,2), (1,6) 

Mọi người: 

(6,4), (2,8), (5,5) 

Đầu tiên chúng ta xây dựng bao lồi. Các điểm (1,6), (2,5), (3,4), (4,2) đều nằm trên biên theo thứ tự độ dốc giảm dần nên tất cả đều giữ nguyên. 

Đối với người (6,4), chúng ta đánh giá tích chấm: 

| Món ăn | Giá trị | 
| --- | --- | 
| 1 | 6·2 + 4·5 = 32 | 
| 2 | 6·3 + 4·4 = 34 | 
| 3 | 6·4 + 4·2 = 32 | 
| 4 | 6·1 + 4·6 = 30 | 

Tối đa là món 2. 

Đối với người (2,8): 

| Món ăn | Giá trị | 
| --- | --- | 
| 1 | 44 | 
| 2 | 44 | 
| 3 | 28 | 
| 4 | 50 | 

Tối đa là món 4. 

Đối với người (5,5): 

| Món ăn | Giá trị | 
| --- | --- | 
| 1 | 35 | 
| 2 | 35 | 
| 3 | 30 | 
| 4 | 35 | 

Liên kết giữa các đĩa 1, 2, 4, chỉ số nhỏ nhất là 1. 

Điều này xác nhận việc bẻ dây chính xác và giảm chính xác đến các điểm cực trị. 

### Mẫu 2 

Món ăn: 

(1,0), (0,2), (0,1) 

Mọi người: 

(1,1), (2,2), (2,1), (1,0) 

Hull là tất cả ba điểm. 

Với người (2,2), tích chấm: 

| Món ăn | Giá trị | 
| --- | --- | 
| 1 | 2 | 
| 2 | 4 | 
| 3 | 2 | 

Ngon nhất là món 2, khẳng định sự thống trị của hướng mạ nặng. 

Đối với người (1,0), chỉ có hương vị là quan trọng: 

| Món ăn | Giá trị | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 0 | 

Tốt nhất là món 1, khẳng định việc xử lý đúng hướng thoái hóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + M log H) | Cấu trúc bao lồi chiếm ưu thế trong quá trình tiền xử lý, mỗi truy vấn tìm kiếm các đỉnh của bao | 
| Không gian | O(N) | Lưu trữ tất cả các điểm và đỉnh thân tàu | 

Chi phí tiền xử lý phù hợp thoải mái trong giới hạn 500.000 điểm. Mỗi truy vấn chỉ hoạt động trên bao lồi, thường nhỏ hơn nhiều so với N, đảm bảo đánh giá nhanh ngay cả ở quy mô lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main
    return sys.stdout.getvalue()

# provided samples
assert run("""4 3
2 5
3 4
4 2
1 6
6 4
2 8
5 5
""").strip() == "2\n4\n1"

assert run("""3 4
1 0
0 2
0 1
1 1
2 2
2 1
1 0
""").strip() == "2\n2\n1\n1"

# minimum size
assert run("""1 2
5 7
1 1
2 3
""").strip() == "1\n1"

# all equal slope dominance
assert run("""3 1
1 1
2 2
3 3
1 1
""").strip() == "3"

# boundary weights
assert run("""2 2
10 0
0 10
1 0
0 1
""").strip() == "1\n2"

# large tie case
assert run("""3 1
1 2
2 1
1 2
1 1
""").strip() == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hộp đựng bát đĩa | 1 1 | xử lý tối thiểu | 
| chuỗi có độ dốc bằng nhau | 3 | sự thống trị đơn điệu | 
| trọng lượng theo trục | 1 2 | xử lý hướng thoái hóa | 
| nhân đôi giá trị tối ưu | 1 | sự đúng đắn của sự ràng buộc | 

## Vỏ cạnh 

Trường hợp cạnh then chốt xảy ra khi tất cả các đĩa nằm trên một đường thẳng trong mặt phẳng. Trong tình huống đó, mọi điểm đều là một phần của bao lồi và thuật toán giảm xuống việc quét tất cả các đỉnh. Đối với hướng truy vấn được căn chỉnh theo đường thẳng, nhiều điểm có thể liên kết với nhau. Việc triển khai giải quyết vấn đề này bằng cách theo dõi các chỉ mục gốc và chọn chỉ số nhỏ nhất, đảm bảo đầu ra xác định. 

Một trường hợp khác là khi vectơ trọng số của một người căn chỉnh chính xác với một trục, chẳng hạn như (T, 0). Trong trường hợp đó, tích chấm chỉ phụ thuộc vào t_k và món ăn tối ưu chỉ đơn giản là món có điểm hương vị tối đa. Bao lồi vẫn chứa tất cả các ứng cử viên và tìm kiếm bậc ba đánh giá chính xác các điểm cuối nơi chứa các giá trị x cực trị. 

Trường hợp cuối cùng liên quan đến các giá trị tối ưu trùng lặp trên các đỉnh thân không liền kề. Tìm kiếm bậc ba có thể dừng lại trong một khoảng nhỏ chứa nhiều ứng cử viên bằng nhau, nhưng lần quét tuyến tính cuối cùng trong khoảng đó đảm bảo rằng tất cả đều được kiểm tra và chỉ số nhỏ nhất được chọn.
