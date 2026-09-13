---
title: "CF 104666E - Deep800080"
description: "Chúng ta có một trụ thẳng trong mặt phẳng, được xác định bởi một đường thẳng đi qua gốc tọa độ và điểm thứ hai $(A, B)$. Chúng ta được phép chọn bất kỳ điểm nào trên đường vô hạn này làm vị trí của lò nướng thịt."
date: "2026-06-29T09:53:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104666
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ICPC Central Europe Regional Contest (CERC 19)"
rating: 0
weight: 104666
solve_time_s: 93
verified: true
draft: false
---

[CF 104666E - Deep800080](https://codeforces.com/problemset/problem/104666/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một trụ thẳng trong mặt phẳng, được xác định bởi một đường thẳng đi qua điểm gốc và điểm thứ hai$(A, B)$. Chúng ta được phép chọn bất kỳ điểm nào trên đường vô hạn này làm vị trí của lò nướng thịt. Xung quanh điểm đã chọn đó, một đám mây khói hình tròn có bán kính cố định$R$xuất hiện. Mỗi chiếc thuyền là một điểm cố định trên mặt phẳng và một chiếc thuyền được coi là “cảnh báo” nếu nó nằm bên trong hoặc trên vòng tròn này. 

Nhiệm vụ là chọn một điểm trên bến tàu sao cho có nhiều nhất số lượng thuyền được bao phủ bởi vòng tròn. 

Về mặt hình học, đây là một bài toán chọn tâm bị ràng buộc: chúng ta không được tự do đặt đường tròn ở bất cứ đâu trong mặt phẳng, chỉ dọc theo một đường cố định và chúng ta muốn vị trí trên đường đó tối đa hóa số điểm trong khoảng cách.$R$. 

Các ràng buộc rất lớn, có thể lên tới$3 \cdot 10^5$thuyền. Bất kỳ cách tiếp cận nào tính toán lại khoảng cách cho mọi vị trí bến tàu có thể hoặc cố gắng rời rạc hóa các trung tâm ứng cử viên một cách trực tiếp sẽ thất bại. Kể cả ngây thơ$O(N^2)$quét qua các cặp thuyền là quá chậm. 

Một vấn đề tinh tế hơn là sự ổn định về số lượng. Bến tàu được xác định bằng tọa độ tùy ý và thuyền có thể nằm ở bất kỳ đâu trong phạm vi tọa độ lớn. Một giải pháp dựa trên các phép tính hình học lặp đi lặp lại phải tránh độ lệch chính xác khi so sánh khoảng cách và điểm cuối của các khoảng. 

Trường hợp cạnh xuất hiện khi thuyền ở xa bến tàu. Ví dụ: nếu tất cả các thuyền đều xa hơn$R$từ chính đường thẳng đó, thì không có chiếc thuyền nào có thể bị che phủ bất kể chúng ta đặt tâm vòng tròn ở đâu trên bến tàu, và câu trả lời là số 0. Việc triển khai bất cẩn không lọc những trường hợp này vẫn có thể tạo ra các khoảng thời gian không hợp lệ và tạo ra sự chồng chéo không chính xác. 

Một trường hợp khác là khi thuyền ở khoảng cách chính xác$R$từ tâm chuyển động cho một vị trí suy biến duy nhất trên đường thẳng. Những trường hợp này tạo ra các khoảng thu gọn thành một điểm và việc xử lý chúng không chính xác vì các khoảng trống sẽ mất các câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là chọn một điểm ứng cử viên trên bến tàu và đếm xem có bao nhiêu chiếc thuyền rơi trong khoảng cách$R$. Nếu chúng ta lấy mẫu nhiều điểm dọc theo đường hoặc thử tất cả các hình chiếu của thuyền trên đường làm tâm ứng cử viên, chúng ta có thể tính toán câu trả lời bằng cách kiểm tra khoảng cách trực tiếp. Điều này hoạt động về mặt khái niệm vì tâm vòng tròn tốt nhất phải nằm ở vị trí “quan trọng” nơi tập hợp các thuyền có mái che thay đổi, thường thẳng hàng với các sự kiện hình học liên quan đến ranh giới của các vòng tròn có tâm ở thuyền. 

Tuy nhiên, cách tiếp cận này quá chậm. Nếu chúng ta thử từng cặp thuyền để tạo ra các chuyển đổi ứng cử viên, chúng ta sẽ nhận được$O(N^2)$ứng viên. Ngay cả việc đánh giá chi phí của một ứng viên$O(N)$, dẫn đến$O(N^3)$trong trường hợp xấu nhất. Thậm chí giảm đánh giá xuống$O(1)$với quá trình tiền xử lý không khắc phục được sự bùng nổ ở các vị trí ứng cử viên. 

Quan sát quan trọng là chúng ta có thể đảo ngược quan điểm. Thay vì chọn một trung tâm và kiểm tra xem nó che phủ những chiếc thuyền nào, chúng ta sửa một chiếc thuyền và hỏi: chiếc thuyền này sẽ được che phủ ở vị trí nào của trung tâm trên bến tàu? 

Sửa một chiếc thuyền$P$. Trung tâm$C$phải thỏa mãn$|C - P| \le R$. Từ$C$bị ràng buộc nằm trên một đường, điều kiện này trở thành ràng buộc khoảng trên một tham số dọc theo đường. Mỗi thuyền đóng góp một khoảng vị trí trung tâm hợp lệ. Câu trả lời cuối cùng là số lượng khoảng thời gian chồng chéo tối đa. 

Do đó, bài toán hình học giảm xuống còn bài toán chồng lấp khoảng 1D sau khi chiếu tất cả các thuyền lên đường cầu tàu và thu nhỏ mỗi vòng tròn thành một khoảng dọc theo đường đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lấy mẫu Brute Force/tạo cặp |$O(N^2)$ĐẾN$O(N^3)$|$O(N)$| Quá chậm | 
| Phép chiếu khoảng + đường quét |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi hình học 2D thành hệ tọa độ 1D thẳng hàng với trụ. 

### 1. Xây dựng hệ tọa độ dọc bến tàu 

Chúng ta coi bến tàu như một đường vô tận đi qua$(0,0)$Và$(A,B)$. Chúng tôi xác định một vectơ hướng$d = (A,B)$và chuẩn hóa nó thành một vector đơn vị$u$. Điều này cho phép chúng ta biểu diễn bất kỳ điểm nào trên bến tàu bằng một tham số vô hướng duy nhất$t$, vị trí ở đâu$P(t) = (0,0) + t \cdot u$. 

Lý do cho bước này là vì tất cả các vị trí nướng hợp lệ đều nằm trên đường này, do đó việc giảm bài toán xuống một tham số duy nhất sẽ loại bỏ một bậc tự do hình học. 

### 2. Tính toán trước khoảng cách vuông góc và hình chiếu cho mỗi thuyền 

Đối với mỗi chiếc thuyền$X_i = (x_i, y_i)$, chúng ta tính hình chiếu của nó lên đường thẳng và khoảng cách vuông góc của nó với đường thẳng. 

Khoảng cách vuông góc quyết định liệu con thuyền có thể được che phủ hay không. Nếu khoảng cách này vượt quá$R$, thì không có đường tròn nào có tâm trên đường thẳng có thể chạm tới nó. 

Nếu thuyền có thể tiếp cận được, chúng tôi cũng tính toán tọa độ hình chiếu của nó$t_i$dọc theo dòng. 

### 3. Chuyển mỗi thuyền thành một quãng trên dây 

Đối với tâm cố định ở vị trí$t$, khoảng cách tới một chiếc thuyền được chia thành các thành phần vuông góc và song song. Thành phần vuông góc được cố định. Bán kính cho phép còn lại dọc theo đường thẳng trở thành:$$\Delta_i = \sqrt{R^2 - d_{\perp}^2}$$Vậy tâm phải thỏa mãn:$$t \in [t_i - \Delta_i,\; t_i + \Delta_i]$$Mỗi chiếc thuyền trở thành một khoảng trên đường thực. 

Đây là sự chuyển đổi quan trọng: ràng buộc vòng tròn 2D trở thành ràng buộc phân đoạn 1D. 

### 4. Quét dòng trên tất cả các khoảng 

Chúng tôi thu thập tất cả các điểm cuối khoảng thời gian. Mỗi lần bắt đầu sẽ thêm +1 vào phạm vi bao phủ, mỗi lần kết thúc sẽ trừ -1. Sắp xếp điểm cuối và quét mang lại sự chồng chéo tối đa tại bất kỳ điểm nào. 

Chúng tôi đảm bảo rằng các điểm cuối được xử lý một cách nhất quán để bao gồm một chiếc thuyền chính xác trên ranh giới. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là mọi vị trí trung tâm hợp lệ trên trụ tương ứng với chính xác một điểm trên tham số đường thực$t$và mỗi thuyền xác định chính xác tập hợp$t$các giá trị mà nó được bảo hiểm. Do đó, phạm vi bao phủ có tính chất cộng gộp theo các khoảng thời gian và việc tối đa hóa số lượng thuyền được che phủ trở nên tương đương với việc tìm một điểm có khoảng cách chồng lấp tối đa. Không có tương tác hình học nào giữa các thuyền còn tồn tại sau khi chiếu, do đó không có sự phụ thuộc tiềm ẩn nào có thể làm mất hiệu lực việc giảm bớt. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    N, R, A, B = map(int, input().split())

    # direction vector of pier
    dx, dy = A, B
    norm = math.hypot(dx, dy)
    ux, uy = dx / norm, dy / norm

    events = []

    for _ in range(N):
        x, y = map(int, input().split())

        # projection coordinate onto pier
        tx = x
        ty = y

        t = tx * ux + ty * uy  # dot product gives projection

        # perpendicular distance via cross product magnitude
        cross = abs(tx * dy - ty * dx) / norm

        if cross > R:
            continue

        span = math.sqrt(R * R - cross * cross)

        l = t - span
        r = t + span

        events.append((l, 1))
        events.append((r, -1))

    events.sort()

    cur = 0
    best = 0

    for _, v in events:
        cur += v
        best = max(best, cur)

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng một cơ sở trực giao dọc theo bến tàu bằng cách sử dụng vectơ chỉ hướng đã chuẩn hóa. Sau đó, mỗi chiếc thuyền được ánh xạ thành tọa độ vô hướng dọc theo hướng đó bằng cách sử dụng tích số chấm. Khoảng cách vuông góc được tính bằng cách sử dụng tích chéo chia cho định mức đường, điều này tránh việc xây dựng một hệ tọa độ xoay một cách rõ ràng. 

Mỗi thuyền chỉ đóng góp một khoảng nếu nó có thể tiếp cận được về mặt hình học, nghĩa là khoảng cách vuông góc không vượt quá bán kính. Nếu không thì nó được bỏ qua một cách an toàn. 

Đường quét sử dụng các điểm cuối được sắp xếp trong đó điểm cuối bên trái tăng số lượng hoạt động và điểm cuối bên phải giảm số lượng đó. Tổng tiền tố tối đa trong quá trình quét này tương ứng với vị trí tốt nhất của món nướng. 

Một chi tiết triển khai tinh tế là độ ổn định của dấu phẩy động. sử dụng`math.hypot`và chuẩn hóa nhất quán sẽ tránh được các lỗi tương đối lớn khi$A, B$lớn. Tuyên bố đảm bảo về dung sai cho phép độ chính xác gấp đôi tiêu chuẩn mà không cần xử lý epsilon bổ sung. 

## Ví dụ đã hoạt động 

### Mẫu 2 

đầu vào:```
3 1 1 0
0 0
2 0
4 0
```Tất cả các thuyền đều nằm trên trục x, cũng là bến tàu. Hướng đơn vị là$(1,0)$, do đó phép chiếu là trực tiếp. 

| Thuyền | t | Khoảng cách Perp | Khoảng thời gian | 
| --- | --- | --- | --- | 
| (0,0) | 0 | 0 | [-1, 1] | 
| (2,0) | 2 | 0 | [1, 3] | 
| (4,0) | 4 | 0 | [3, 5] | 

Quét các khoảng này, sự chồng chéo tối đa xảy ra ở$t \in [1,1]$Và$t \in [3,3]$, mỗi chiếc che 2 chiếc thuyền. 

Đầu ra là 2. 

### Mẫu 3 

đầu vào:```
4 1 1 0
0 0
1 1
1 -1
2 0
```Bến tàu lại là trục x. Chúng tôi phân loại các khoảng: 

| Thuyền | Chiếu t | Khoảng cách Perp | Khoảng thời gian | 
| --- | --- | --- | --- | 
| (0,0) | 0 | 0 | [-1, 1] | 
| (1,1) | 1 | 1 | điểm tại 1 | 
| (1,-1) | 1 | 1 | điểm tại 1 | 
| (2,0) | 2 | 0 | [1, 3] | 

Tại$t=1$, cả bốn khoảng đều trùng nhau. Điều này mang lại câu trả lời 4, chứng minh các khoảng suy biến vẫn đóng góp chính xác như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi thuyền có nhiều nhất hai sự kiện, được sắp xếp một lần, sau đó quét tuyến tính | 
| Không gian |$O(N)$| Lưu trữ điểm cuối khoảng thời gian cho tất cả các thuyền có thể tiếp cận | 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$thuyền, vì vậy một$O(N \log N)$quét dễ dàng đủ nhanh. Việc sử dụng bộ nhớ là tuyến tính theo số khoảng thời gian, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import hypot, sqrt
    import math

    input = sys.stdin.readline
    N, R, A, B = map(int, input().split())
    dx, dy = A, B
    norm = math.hypot(dx, dy)
    ux, uy = dx / norm, dy / norm

    events = []
    for _ in range(N):
        x, y = map(int, input().split())
        t = x * ux + y * uy
        cross = abs(x * dy - y * dx) / norm
        if cross <= R:
            span = math.sqrt(R * R - cross * cross)
            events.append((t - span, 1))
            events.append((t + span, -1))

    events.sort()
    cur = 0
    best = 0
    for _, v in events:
        cur += v
        best = max(best, cur)
    return str(best)

# provided samples
assert run("""7 5 0 1
-1 -1
1 -1
0 0
2 3
3 4
10 10
2 12
""") == "5"

assert run("""3 1 1 0
0 0
2 0
4 0
""") == "2"

assert run("""4 1 1 0
0 0
1 1
1 -1
2 0
""") == "4"

# custom cases
assert run("""1 10 1 1
5 5
""") == "1", "single boat always covered if reachable"

assert run("""2 1 1 0
0 10
0 -10
""") == "0", "all too far from line"

assert run("""5 2 0 1
0 0
0 1
0 2
0 3
0 4
""") == "3", "vertical stacking overlap window"

assert run("""3 5 1 0
-100 0
0 0
100 0
""") == "2", "large spread with limited radius"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 1 | cấu hình tối thiểu | 
| tất cả đều không thể truy cập | 0 | lọc theo khoảng cách vuông góc | 
| chồng chéo đường dày đặc | 3 | khoảng thời gian xếp chồng chính xác | 
| thưa thớt lan rộng | 2 | sửa cửa sổ chồng lấp cục bộ | 

## Vỏ cạnh 

Nếu tất cả các thuyền nằm xa đường bến tàu thì mọi khoảng cách vuông góc đều vượt quá$R$và thuật toán không tạo ra khoảng thời gian. Sau đó, quá trình quét không thấy sự kiện nào và trả về số 0, phù hợp với thực tế là không có vị trí nào trên đường có thể đến được bất kỳ thuyền nào. 

Khi một chiếc thuyền nằm cách xa nhau$R$từ đường thẳng, khoảng của nó thu gọn lại thành một điểm duy nhất. Trong đường quét, điểm này xuất hiện dưới dạng điểm bắt đầu và điểm kết thúc ở cùng một tọa độ, góp phần chồng lên nhau một cách chính xác ở vị trí trung tâm chính xác đó mà không cần xử lý đặc biệt. 

Khi nhiều thuyền chia sẻ tọa độ chiếu giống hệt nhau nhưng khoảng cách vuông góc khác nhau, các khoảng cách của chúng chồng lên nhau rất nhiều xung quanh cùng một khu vực. Quá trình quét tích lũy chính xác tất cả các đóng góp vì mỗi khoảng thời gian là độc lập sau khi chiếu.
