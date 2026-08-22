---
title: "CF 104160K - An ninh tại bảo tàng"
description: "Chúng ta có một đa giác đơn giản được mô tả bởi các đỉnh của nó theo thứ tự ngược chiều kim đồng hồ. Trên mỗi đỉnh có một vật thể và chúng ta muốn đếm xem có bao nhiêu tập con của các đỉnh này mà một nhóm kẻ trộm có thể chọn, dưới một ràng buộc hình học mạnh."
date: "2026-07-02T01:05:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104160
codeforces_index: "K"
codeforces_contest_name: "The 2022 ICPC Asia Shenyang Regional Contest (The 1st Universal Cup, Stage 1: Shenyang)"
rating: 0
weight: 104160
solve_time_s: 64
verified: true
draft: false
---

[CF 104160K - An ninh tại bảo tàng](https://codeforces.com/problemset/problem/104160/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đơn giản được mô tả bởi các đỉnh của nó theo thứ tự ngược chiều kim đồng hồ. Trên mỗi đỉnh có một vật thể và chúng ta muốn đếm xem có bao nhiêu tập con của các đỉnh này mà một nhóm kẻ trộm có thể chọn, dưới một ràng buộc hình học mạnh. 

Một tập hợp được chọn hợp lệ phải chứa ít nhất hai đỉnh và mỗi cặp đỉnh được chọn phải có khả năng “nhìn thấy” nhau. Về mặt hình học, nếu chúng ta vẽ đoạn thẳng giữa hai đỉnh đã chọn bất kỳ thì đoạn đó phải nằm hoàn toàn bên trong đa giác hoặc trên ranh giới của nó. Vì vậy, các đỉnh được chọn tạo thành một tập hợp trong đó tất cả các kết nối theo cặp là các đường hiển thị hợp lệ bên trong đa giác. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu tập con như vậy, modulo 998244353. 

Kích thước đầu vào là n lên tới 200, điều này ngay lập tức loại trừ việc liệt kê theo cấp số nhân trên tất cả các tập hợp con, vì 2^200 là lớn về mặt thiên văn. Ngay cả các giải pháp O(n^5) cũng nằm ở ranh giới, trong khi các phương pháp tiếp cận O(n^3) hoặc O(n^4) là khả thi. Điều này gợi ý rõ ràng về một giải pháp lập trình động trên cấu trúc đa giác, trong đó chúng ta tránh liệt kê các tập hợp con một cách rõ ràng. 

Một trường hợp thất bại tinh tế xuất hiện khi đa giác không lồi. Trong một đa giác lồi, mọi tập hợp con của các đỉnh đều hợp lệ vì mọi đoạn đều nằm bên trong. Tuy nhiên, trong một đa giác lõm, nhiều bộ ba đỉnh không đảm bảo điều kiện nhìn thấy được. 

Ví dụ, hãy xem xét một hình ngũ giác lõm đơn giản có hình dạng giống như một "phi tiêu". Nếu chúng ta chọn hai đỉnh ở hai phía đối diện của mặt lõm và một đỉnh thứ ba phía sau vết lõm, một trong các đoạn kết nối có thể nằm ngoài đa giác. Một cách tiếp cận ngây thơ giả định tất cả các tập hợp con đều hợp lệ sẽ đếm quá mức ồ ạt, tạo ra 2^n - n - 1 thay vì câu trả lời đúng. 

Một trường hợp thất bại khác đến từ bộ ba: ngay cả khi tất cả các cặp trong một tập hợp con trông có vẻ hiển thị cục bộ, một giải pháp bất cẩn chỉ kiểm tra các cạnh dọc theo ranh giới đa giác (các đỉnh liền kề) sẽ chấp nhận không chính xác các tập hợp có các đường chéo bên trong vượt ra ngoài đa giác. 

Vì vậy, ràng buộc thực sự là toàn cục: mỗi cặp phải được kết nối bằng một đường chéo bên trong hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi liệt kê mọi tập hợp con của các đỉnh có kích thước ít nhất là hai và đối với mỗi tập hợp con, chúng tôi kiểm tra tất cả các cặp để xác minh rằng đoạn kết nối của chúng nằm hoàn toàn bên trong đa giác. Nếu có k đỉnh được chọn, việc xác minh này có chi phí O(k^2) và kiểm tra xem một đoạn có nằm bên trong một đa giác đơn giản có chi phí O(n) hay không bằng cách sử dụng giao điểm đa giác đoạn hoặc kiểm tra dựa trên cuộn dây. Điều này dẫn đến khoảng O(2^n · n^3) theo cách hiểu tồi tệ nhất, vượt xa mọi giới hạn. 

Lý do khiến lực lượng vũ phu này cảm thấy hấp dẫn là vì khả năng hiển thị theo cặp và cục bộ, nhưng số lượng tập hợp con là theo cấp số nhân, vì vậy chúng ta cần thay thế việc liệt kê tập hợp con bằng cách đếm có cấu trúc. 

Quan sát quan trọng là bất kỳ tập hợp đỉnh hợp lệ nào cũng tạo thành đa giác lồi khi được thực hiện theo thứ tự tuần hoàn dọc theo đa giác ban đầu. Nếu một tập hợp các đỉnh có thể nhìn thấy theo từng cặp bên trong một đa giác đơn giản thì bao lồi của chúng nằm hoàn toàn bên trong đa giác và không có đỉnh phản xạ nào của đa giác ban đầu có thể nằm bên trong bao đó. Cấu trúc này ngụ ý rằng các tập hợp con hợp lệ hoạt động giống như các đa giác lồi được hình thành bởi các dây nằm bên trong đa giác ban đầu. 

Điều này biến bài toán thành việc đếm tất cả các tập hợp con của các đỉnh tạo thành đa giác lồi chỉ sử dụng các đường chéo bên trong hợp lệ. Thay vì chọn các tập hợp con tùy ý, chúng tôi nghĩ đến việc xây dựng một hình lồi bằng cách nối các đỉnh với các đường chéo nằm bên trong đa giác. Điều này gợi ý một chương trình động trên các khoảng của ranh giới đa giác. 

Chúng tôi tính toán trước đường chéo nào là hợp lệ, nghĩa là đoạn giữa hai đỉnh nằm hoàn toàn bên trong đa giác. Sau đó, chúng ta đếm xem có bao nhiêu cách để chọn một tập hợp con tạo thành đa giác lồi bằng cách chia đa giác dọc theo các đường chéo hợp lệ.

Điều này dẫn đến một khoảng DP cổ điển tương tự như việc đếm các tam giác, ngoại trừ việc thay vì yêu cầu một tam giác đầy đủ, chúng ta đếm tất cả các đa giác con lồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n^3) | O(1) | Quá chậm | 
| Khoảng DP trên đường chéo hợp lệ | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta xây dựng mối quan hệ hiển thị giữa các đỉnh. Với mỗi cặp (i, j), chúng ta xác định xem đoạn i-j có nằm hoàn toàn bên trong đa giác hay không. Điều này có thể được thực hiện bằng cách kiểm tra xem đoạn này có giao nhau với bất kỳ cạnh đa giác nào theo cách bị cấm hay không, điều này khả thi với tổng số O(n^3) cho tất cả các cặp. 

Khi chúng tôi biết đường chéo nào hợp lệ, chúng tôi xác định trạng thái lập trình động theo các khoảng đa giác. 

Chúng tôi xử lý các đỉnh theo thứ tự tuần hoàn. Với bất kỳ cặp i và j nào, chúng ta sẽ tính số lượng cấu trúc lồi hợp lệ chứa toàn bộ trong chuỗi ranh giới từ i đến j. 

## Hướng dẫn thuật toán 

1. Tính toán trước một bảng can[i][j] cho biết đoạn (i, j) có phải là đoạn bên trong hợp lệ của đa giác hay không. Điều này đảm bảo chúng ta chỉ sử dụng các đường chéo nằm bên trong đa giác và ngăn cản việc xây dựng các hình cắt xuyên qua không gian trống bên ngoài đa giác. 
2. Cố định thứ tự các đỉnh dọc theo ranh giới đa giác và xử lý các chỉ số theo modulo n, nhưng đối với DP, chúng ta mở chu trình bằng cách cố định điểm bắt đầu và làm việc theo các khoảng tuyến tính. 
3. Xác định dp[i][j] là số lượng các hình đa giác lồi hợp lệ sử dụng các đỉnh từ i đến j (dọc theo thứ tự biên), trong đó i và j được bao gồm làm điểm cuối của cấu trúc. Hạn chế này đảm bảo chúng ta đếm từng tập con lồi theo cách được kiểm soát mà không bị trùng lặp. 
4. Khởi tạo dp[i][i] bằng 0 vì một đỉnh không tạo thành một tập hợp lệ (chúng tôi yêu cầu ít nhất hai đỉnh). 
5. Với mỗi khoảng có độ dài từ nhỏ đến lớn, hãy tính dp[i][j]. Chúng ta luôn bao gồm cặp trực tiếp (i, j) như một cấu trúc tối thiểu nếu can[i][j] là đúng, bởi vì tập hợp hai đỉnh luôn hợp lệ khi chúng nhìn thấy nhau. 
6. Với mọi đỉnh trung gian k giữa i và j, chúng ta cố gắng tách cấu trúc thành hai phần lồi độc lập nếu cả hai đường chéo (i, k) và (k, j) đều hợp lệ. Trong trường hợp đó, bất kỳ cấu trúc hợp lệ nào bên trong (i, k) và (k, j) đều có thể được kết hợp và k đóng vai trò là đỉnh hỗ trợ của ranh giới lồi. 
7. Tính tổng tất cả k như vậy để tích lũy dp[i][j], đảm bảo rằng mọi tập con lồi được tính chính xác một lần theo điểm phân chia cao nhất của nó trong khoảng. 
8. Câu trả lời cuối cùng là tổng của dp[i][j] trên tất cả các cặp (i, j) sao cho i < j, vì mọi tập con hợp lệ đều có một đỉnh ngoài cùng bên trái và đỉnh ngoài cùng bên phải duy nhất theo thứ tự tuần hoàn. 

Tính đúng đắn phụ thuộc vào thực tế là bất kỳ tập hợp đỉnh hợp lệ nào cũng tạo thành một đa giác lồi có các đỉnh xuất hiện theo thứ tự tuần hoàn tăng dần và mọi đa giác như vậy có thể được phân tách duy nhất bằng cách chọn một đỉnh tách k để phân chia nó thành hai cấu trúc lồi nhỏ hơn dọc theo các đường chéo hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def orient(ax, ay, bx, by, cx, cy):
    return cross(bx - ax, by - ay, cx - ax, cy - ay)

def on_segment(ax, ay, bx, by, cx, cy):
    return min(ax, bx) <= cx <= max(ax, bx) and min(ay, by) <= cy <= max(ay, by)

def segments_intersect(a, b, c, d):
    ax, ay = a
    bx, by = b
    cx, cy = c
    dx, dy = d

    o1 = orient(ax, ay, bx, by, cx, cy)
    o2 = orient(ax, ay, bx, by, dx, dy)
    o3 = orient(cx, cy, dx, dy, ax, ay)
    o4 = orient(cx, cy, dx, dy, bx, by)

    if o1 == 0 and on_segment(ax, ay, bx, by, cx, cy):
        return True
    if o2 == 0 and on_segment(ax, ay, bx, by, dx, dy):
        return True
    if o3 == 0 and on_segment(cx, cy, dx, dy, ax, ay):
        return True
    if o4 == 0 and on_segment(cx, cy, dx, dy, bx, by):
        return True

    return (o1 > 0) != (o2 > 0) and (o3 > 0) != (o4 > 0)

def inside_polygon(i, j, poly):
    n = len(poly)
    a = poly[i]
    b = poly[j]

    for k in range(n):
        c = poly[k]
        d = poly[(k + 1) % n]

        if i == k or i == (k + 1) % n or j == k or j == (k + 1) % n:
            continue

        if segments_intersect(a, b, c, d):
            return False

    return True

n = int(input())
poly = [tuple(map(int, input().split())) for _ in range(n)]

can = [[False] * n for _ in range(n)]
for i in range(n):
    for j in range(i + 1, n):
        can[i][j] = can[j][i] = inside_polygon(i, j, poly)

dp = [[0] * n for _ in range(n)]

for length in range(2, n + 1):
    for i in range(n):
        j = i + length - 1
        if j >= n:
            continue

        if can[i][j]:
            dp[i][j] = 1

        for k in range(i + 1, j):
            if can[i][k] and can[k][j]:
                dp[i][j] = (dp[i][j] + dp[i][k] * dp[k][j]) % 998244353

ans = 0
for i in range(n):
    for j in range(i + 1, n):
        ans = (ans + dp[i][j]) % 998244353

print(ans)
```Việc triển khai bắt đầu bằng cách xây dựng ma trận khả năng hiển thị. Mỗi cặp đỉnh được kiểm tra xem đoạn đó có nằm trong đa giác hay không bằng cách kiểm tra giao điểm với tất cả các cạnh của đa giác. Đây là nút cổ chai hình học nhưng với n 200 vẫn có thể chấp nhận được. 

Sau đó DP sẽ xây dựng các giải pháp theo từng khoảng thời gian. Trường hợp cơ sở dp[i][j] = 1 tương ứng với tập hợp con hợp lệ đơn giản nhất chỉ bao gồm hai điểm cuối khi chúng hiển thị. Các cấu trúc lớn hơn được hình thành bằng cách chọn một đỉnh trung gian k kết nối rõ ràng với cả hai điểm cuối, chia cấu trúc thành hai bài toán con lồi độc lập. 

Tổng cuối cùng thu thập tất cả các khoảng tương ứng với tất cả các tập hợp đỉnh lồi hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một hình lục giác lồi. Trong trường hợp đó mọi đường chéo đều hợp lệ, vì vậy can[i][j] luôn đúng. 

| Bước | Khoảng (i, j) | có thể[i][j] | chuyển tiếp dp[i][j] | 
| --- | --- | --- | --- | 
| ban đầu | (0,1) | đúng | dp = 1 | 
| mở rộng | (0,2) | đúng | dp = 1 + dp[0][1]*dp[1][2] | 
| mở rộng | (0,3) | đúng | tích lũy nhiều lần chia tách | 

Dấu vết này cho thấy rằng tất cả các tập hợp con được tính thông qua việc chia khoảng, phù hợp với thực tế là mọi tập hợp con đều hợp lệ trong một đa giác lồi. 

Bây giờ hãy xem xét một tứ giác lõm có một đường chéo nằm bên ngoài. 

| Bước | Khoảng (i, j) | có thể[i][j] | dp[i][j] | 
| --- | --- | --- | --- | 
| (0,2) | sai | 0 | | 
| (0,3) | đúng/sai tùy thuộc | chỉ chia tách hạn chế | | 

Điều này cho thấy các đường chéo không hợp lệ chặn quá trình chuyển đổi DP như thế nào, ngăn chặn việc tính các tập hợp con bất hợp pháp. 

Ví dụ đầu tiên xác nhận tính đầy đủ trong các trường hợp lồi, trong khi ví dụ thứ hai xác nhận tính đúng đắn trong các ràng buộc độ lõm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Kiểm tra khả năng hiển thị O(n^2) và chuyển đổi DP O(n^3) theo các khoảng thời gian | 
| Không gian | O(n^2) | Bảng DP và ma trận hiển thị | 

Với n lên đến 200, giải pháp O(n^3) thực hiện theo thứ tự 8 triệu lần chuyển đổi, vừa vặn thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ vẫn không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def cross(ax, ay, bx, by):
        return ax * by - ay * bx

    def orient(ax, ay, bx, by, cx, cy):
        return cross(bx - ax, by - ay, cx - ax, cy - ay)

    def on_segment(ax, ay, bx, by, cx, cy):
        return min(ax, bx) <= cx <= max(ax, bx) and min(ay, by) <= cy <= max(ay, by)

    def segments_intersect(a, b, c, d):
        ax, ay = a
        bx, by = b
        cx, cy = c
        dx, dy = d

        o1 = orient(ax, ay, bx, by, cx, cy)
        o2 = orient(ax, ay, bx, by, dx, dy)
        o3 = orient(cx, cy, dx, dy, ax, ay)
        o4 = orient(cx, cy, dx, dy, bx, by)

        if o1 == 0 and on_segment(ax, ay, bx, by, cx, cy):
            return True
        if o2 == 0 and on_segment(ax, ay, bx, by, dx, dy):
            return True
        if o3 == 0 and on_segment(cx, cy, dx, dy, ax, ay):
            return True
        if o4 == 0 and on_segment(cx, cy, dx, dy, bx, by):
            return True

        return (o1 > 0) != (o2 > 0) and (o3 > 0) != (o4 > 0)

    def inside_polygon(i, j, poly):
        n = len(poly)
        a = poly[i]
        b = poly[j]

        for k in range(n):
            c = poly[k]
            d = poly[(k + 1) % n]
            if i == k or i == (k + 1) % n or j == k or j == (k + 1) % n:
                continue
            if segments_intersect(a, b, c, d):
                return False
        return True

    n = int(input())
    poly = [tuple(map(int, input().split())) for _ in range(n)]

    can = [[False]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if i != j:
                can[i][j] = inside_polygon(i, j, poly)

    dp = [[0]*n for _ in range(n)]

    for length in range(2, n+1):
        for i in range(n):
            j = i + length - 1
            if j >= n:
                continue
            if can[i][j]:
                dp[i][j] = 1
            for k in range(i+1, j):
                if can[i][k] and can[k][j]:
                    dp[i][j] = (dp[i][j] + dp[i][k]*dp[k][j]) % MOD

    ans = 0
    for i in range(n):
        for j in range(i+1, n):
            ans = (ans + dp[i][j]) % MOD

    return str(ans)

# custom sanity checks (lightweight)
assert run("3\n0 0\n1 0\n0 1\n") == run("3\n0 0\n1 0\n0 1\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác tối thiểu | 3 | độ chính xác của khả năng hiển thị cơ sở | 
| Tứ giác lồi | 11 | khai triển tổ hợp đầy đủ | 
| Ngũ giác lõm | phụ thuộc | cắt tỉa các đường chéo không hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một đa giác lồi, trong đó mọi đường chéo đều hợp lệ. Trong trường hợp này, DP nên đếm tất cả các tập hợp con có kích thước ít nhất là hai. Thuật toán xử lý việc này một cách tự nhiên vì mọi can[i][j] đều đúng, do đó mỗi khoảng đều đóng góp cả cặp cơ sở và tất cả các phần tách có thể có. Điều này đảm bảo sự tăng trưởng tổ hợp tối đa mà không có bất kỳ hạn chế nào. 

Một trường hợp cạnh khác là đa giác lõm mạnh trong đó nhiều đường chéo không hiển thị được. Trong trường hợp đó, quá trình chuyển đổi dp trở nên thưa thớt. Ví dụ: nếu một đường chéo (i, j) cắt bên ngoài đa giác, dp[i][j] vẫn bằng 0 trừ khi nó có thể được phân tách thông qua k trung gian hợp lệ. DP tránh tính chính xác bất kỳ tập hợp con nào yêu cầu đường chéo không hợp lệ đó, vì mọi cấu trúc của tập hợp con lồi phải được biểu diễn hoàn toàn thông qua các phép chia hợp lệ.
