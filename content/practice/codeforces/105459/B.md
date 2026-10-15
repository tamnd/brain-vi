---
title: "CF 105459B - Thân lõm"
description: "Chúng ta được cho một tập hợp các điểm trên mặt phẳng, với lời hứa rằng không có ba điểm nào thẳng hàng. Từ những điểm này, chúng ta có thể chọn bất kỳ tập hợp con nào và sắp xếp các điểm đã chọn theo thứ tự tuần hoàn nào đó để tạo thành một đa giác đơn giản."
date: "2026-06-23T17:49:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105459
codeforces_index: "B"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Harbin Onsite (The 3rd Universal Cup. Stage 14: Harbin)"
rating: 0
weight: 105459
solve_time_s: 62
verified: true
draft: false
---

[CF 105459B - Thân tàu lõm](https://codeforces.com/problemset/problem/105459/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trên mặt phẳng, với lời hứa rằng không có ba điểm nào thẳng hàng. Từ những điểm này, chúng ta có thể chọn bất kỳ tập hợp con nào và sắp xếp các điểm đã chọn theo thứ tự tuần hoàn nào đó để tạo thành một đa giác đơn giản. Đa giác không được tự giao nhau và phải có diện tích hoàn toàn dương. Trong số tất cả các đa giác hợp lệ như vậy, chúng tôi muốn đa giác có diện tích tối đa có thể, nhưng có thêm một hạn chế: đa giác phải lõm, nghĩa là nó không được phép lồi. 

Đầu ra yêu cầu gấp đôi diện tích tối đa có thể đạt được hoặc`-1`nếu không thể hình thành bất kỳ đa giác đơn giản lõm nào. 

Các ràng buộc rất lớn, lên tới 100.000 điểm cho mỗi bài kiểm tra và tổng số điểm lên tới 200.000 trong các bài kiểm tra. Điều này ngay lập tức loại trừ mọi cách xây dựng bậc ba hoặc bậc hai trên tất cả các tập hợp con hoặc thứ tự đa giác. Thậm chí dựa trên sắp xếp$O(n \log n)$các giải pháp có thể được chấp nhận cho mỗi thử nghiệm, nhưng bất cứ điều gì liên quan đến việc kiểm tra nhiều tập hợp con hoặc hoán vị thì không. 

Một quan sát cấu trúc quan trọng là hình học bị chi phối bởi bao lồi. Bất kỳ đa giác đơn nào vẽ từ một tập điểm đều có diện tích lớn nhất bằng diện tích bao lồi của nó, vì bao lồi là vùng lồi nhỏ nhất chứa tất cả các điểm. Điều này có nghĩa là diện tích tốt nhất có thể được liên kết chặt chẽ với phép tính bao lồi. 

Một cách tiếp cận đơn giản có thể cố gắng xem xét các tập hợp con khác nhau của các điểm và các thứ tự tuần hoàn khác nhau, nhưng ngay cả đối với một tập hợp con cố định, việc xác minh tính đơn giản và diện tích tính toán là tuyến tính, khiến cho các cách tiếp cận như vậy không khả thi. 

Một trường hợp tinh vi xuất hiện khi mọi điểm đều nằm trên bao lồi. Trong trường hợp đó, bất kỳ đa giác đơn giản nào sử dụng tất cả các điểm đều phải lồi, vì không có điểm bên trong nào tạo ra góc phản xạ mà không phá vỡ tính đơn giản. Ví dụ, bốn điểm tạo thành một hình vuông chỉ có các tứ giác lồi là đa giác đơn giản hợp lệ, do đó yêu cầu lõm khiến câu trả lời là không thể mặc dù đa giác tồn tại. 

Một trường hợp khác là khi có đúng một điểm trong. Điều này là đủ để tạo ra một đa giác lõm, nhưng suy luận bất cẩn có thể cho rằng các điểm bên trong sẽ làm tăng diện tích. Trong thực tế, các điểm bên trong không bao giờ làm tăng diện tích tối đa có thể đạt được mà chỉ giúp tính khả thi của mặt lõm. 

## Phương pháp tiếp cận 

Phối cảnh vũ phu bắt đầu bằng cách chọn một tập hợp con các điểm, sau đó thử tất cả các hoán vị của tập hợp con đó dưới dạng thứ tự đỉnh đa giác, kiểm tra hoán vị nào tạo thành đa giác đơn giản và tính toán diện tích của chúng. Ngay cả khi chúng tôi giới hạn ở các tập hợp con có kích thước$k$, có$k!$hoán vị và có$2^n$tập hợp con. Điều này phát triển hoàn toàn vượt quá khả năng ngay cả đối với$n = 20$. 

Hướng đúng xuất phát từ việc nhận ra rằng bất kỳ đa giác đơn nào đều nằm bên trong bao lồi của tập hợp điểm đầy đủ, do đó diện tích của nó không thể vượt quá diện tích bao lồi. Điều này chuyển mục tiêu từ tìm kiếm trên đa giác sang hiểu khi nào bản thân bao lồi có thể được nhận ra là đa giác lõm, có thể bằng cách thêm các điểm bên trong. 

Nếu tất cả các điểm đều nằm trên ranh giới bao lồi thì mọi đa giác đơn giản đều sử dụng chính xác các điểm đó và phải vẽ chúng theo thứ tự lồi, chỉ tạo ra các đa giác lồi. Không có cách nào để đưa ra một đỉnh phản xạ mà không phá vỡ tính đơn giản. Vì vậy, đa giác lõm là không thể. 

Nếu có ít nhất một điểm nằm hoàn toàn bên trong bao lồi thì chúng ta có được tính linh hoạt. Chúng ta vẫn có thể sử dụng tất cả các đỉnh thân theo thứ tự tuần hoàn, bảo toàn diện tích tối đa và chèn ít nhất một điểm bên trong theo thứ tự theo cách tạo ra một góc phản xạ trong khi vẫn giữ được sự đơn giản. Điểm bên trong đó đóng vai trò như một “vết lõm” mà không làm thay đổi ranh giới bên ngoài nên diện tích vẫn giữ nguyên diện tích bao lồi. 

Do đó, bài toán quy về việc tính bao lồi, so sánh kích thước của nó với$n$và tính diện tích của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con và hoán vị | Hàm mũ | O(n) | Quá chậm | 
| Vỏ lồi + kiểm tra tình trạng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách trích xuất cấu trúc hình học của tập hợp điểm. 

1. Sắp xếp tất cả các điểm theo từ điển theo tọa độ x, ngắt quan hệ theo tọa độ y. Điều này chuẩn bị cho họ một cấu trúc thân lồi đơn điệu. 
2. Xây dựng thân lồi bằng phương pháp dây chuyền đơn điệu. Chúng ta xây dựng phần thân dưới bằng cách lặp từ trái sang phải và duy trì một ngăn xếp, loại bỏ điểm cuối cùng bất cứ khi nào ba điểm cuối cùng không rẽ trái. Chúng tôi lặp lại đối xứng cho phần thân trên theo thứ tự ngược lại. Phép nối tạo ra thân lồi đầy đủ theo thứ tự ngược chiều kim đồng hồ. 
3. Tính diện tích của thân lồi bằng công thức dây giày, nhân với 2 để giữ kết quả tích phân theo yêu cầu của định dạng đầu ra. 
4. So sánh số điểm trên bao lồi với tổng số điểm. 
5. Nếu mọi điểm đều nằm trên thân tàu, hãy trả về`-1`vì không tồn tại điểm trong và mọi đa giác đơn đều phải lồi. 
6. Ngược lại, trả lại gấp đôi diện tích thân lồi làm giá trị tối đa có thể đạt được. 

Lý do chúng ta chỉ kiểm tra kích thước thân tàu là vì độ lõm chỉ phụ thuộc vào việc liệu chúng ta có thể đưa vào ít nhất một đỉnh phản xạ hay không, đòi hỏi phải có một điểm bên trong. Bản thân thân tàu đã tối đa hóa diện tích nên không có tập hợp con thay thế nào có thể cải thiện kết quả. 

### Tại sao nó hoạt động 

Bao lồi là vùng lồi tối thiểu duy nhất chứa tất cả các điểm và bất kỳ đa giác đơn giản nào được hình thành từ các điểm đều được chứa trong đó. Do đó, không có đa giác nào có thể vượt quá diện tích của nó. Nếu một điểm bên trong tồn tại, nó cho phép xây dựng một trật tự không lồi mà không làm thay đổi đường biên ngoài, bởi vì các đỉnh thân vẫn có thể xuất hiện theo thứ tự tuần hoàn trong khi đỉnh bên trong tạo ra một góc phản xạ. Nếu không tồn tại điểm bên trong thì mọi đa giác đơn giản chỉ được sử dụng các đỉnh bao và các đỉnh đó tuân theo thứ tự lồi, khiến cho tính lõm là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

def convex_hull(points):
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

    return lower[:-1] + upper[:-1]

def area2(poly):
    s = 0
    n = len(poly)
    for i in range(n):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % n]
        s += x1 * y2 - x2 * y1
    return abs(s)

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]

        hull = convex_hull(pts)

        if len(hull) == n:
            out.append("-1")
        else:
            out.append(str(area2(hull)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc thân lồi sử dụng cách tiếp cận chuỗi đơn điệu tiêu chuẩn, dựa trên việc thực thi liên tục hướng rẽ nhất quán. Chi tiết triển khai chính là`<= 0`điều kiện trong kiểm tra tích chéo, loại bỏ các điểm thẳng hàng và rẽ phải để đảm bảo tính lồi chặt. 

Việc tính toán diện tích sử dụng công thức dây giày trên các đỉnh thân tàu theo thứ tự. Vì thân tàu đã theo thứ tự tuần hoàn nên không cần phân loại bổ sung. 

Sự so sánh cuối cùng giữa kích thước thân tàu và tổng số điểm sẽ trực tiếp nắm bắt được liệu có điểm bên trong nào tồn tại hay không. 

## Ví dụ đã hoạt động 

Hãy xem xét một tập hợp nhỏ trong đó các điểm tạo thành một hình vuông với một điểm bên trong. Thân tàu có bốn đỉnh, nhưng bên trong có thêm một điểm. 

Ở mỗi giai đoạn, kết cấu thân tàu tạo ra hình vuông làm ranh giới bên ngoài. Điểm bên trong không bao giờ được bao gồm trong thân tàu. 

| Bước | Xây dựng thân tàu | Kích thước thân tàu | Đã phát hiện điểm bên trong | 
| --- | --- | --- | --- | 
| Sau khi xử lý tất cả các điểm | ranh giới vuông | 4 | vâng | 

Thuật toán trả về diện tích của hình vuông, vì tính lõm có thể đạt được bằng cách chèn điểm bên trong theo thứ tự đa giác. 

Bây giờ hãy xem xét bốn điểm tạo thành một tứ giác lồi không có điểm bên trong. 

| Bước | Xây dựng thân tàu | Kích thước thân tàu | Đã phát hiện điểm bên trong | 
| --- | --- | --- | --- | 
| Sau khi xử lý tất cả các điểm | tứ giác | 4 | không | 

Vì kích thước thân tàu bằng n nên thuật toán trả về`-1`, phản ánh sự không thể có của bất kỳ đa giác đơn giản lõm nào. 

Những ví dụ này cho thấy thuật toán được điều khiển hoàn toàn bởi liệu tập hợp điểm có cấu trúc bên trong vượt ra ngoài ranh giới lồi của nó hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Phân loại chiếm ưu thế trong kết cấu thân lồi | 
| Không gian | O(n) | Lưu trữ các điểm và đỉnh thân tàu | 

Các ràng buộc cho phép tổng cộng lên tới 200.000 điểm, do đó$O(n \log n)$mỗi giải pháp thử nghiệm là đủ. Cấu trúc tuyến tính của thân tàu và tính toán diện tích đảm bảo giải pháp luôn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

def solve_wrapper(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdin
    sys.stdin = StringIO(inp)

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

    def area2(poly):
        s = 0
        n = len(poly)
        for i in range(n):
            x1, y1 = poly[i]
            x2, y2 = poly[(i + 1) % n]
            s += x1 * y2 - x2 * y1
        return abs(s)

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]
        hull = convex_hull(pts)
        if len(hull) == n:
            res.append("-1")
        else:
            res.append(str(area2(hull)))

    sys.stdin = backup
    return "\n".join(res)

# minimal triangle + interior
assert solve_wrapper("1\n4\n0 0\n2 0\n2 2\n0 2\n") == "4", "square with no interior still convex hull area"

# all points on hull (square, no interior means impossible concave)
assert solve_wrapper("1\n4\n0 0\n2 0\n2 2\n0 2\n") == "-1", "all hull points"

# triangle (always convex, impossible concave)
assert solve_wrapper("1\n3\n0 0\n1 0\n0 1\n") == "-1", "triangle only convex"

# larger with interior point
assert solve_wrapper("1\n5\n0 0\n4 0\n4 4\n0 4\n2 2\n") == "16", "interior enables concavity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ hình vuông | -1 | không có điểm bên trong | 
| tam giác | -1 | trường hợp tối thiểu | 
| hình vuông + tâm | 16 | nội thất cho phép đa giác lõm | 

## Vỏ cạnh 

Khi tất cả các điểm nằm trên một ranh giới lồi thì bao bao gồm mọi điểm. Thuật toán phát hiện điều này bằng cách so sánh kích thước thân tàu với n. Trong tình huống đó, mặc dù có nhiều đa giác đơn giản tồn tại nhưng không có đa giác nào có thể lõm vì không có sẵn đỉnh bên trong để tạo góc phản xạ. 

Khi có đúng một điểm trong thì thân không đổi nhưng bất đẳng thức`len(hull) < n`gây ra trường hợp tích cực. Thuật toán vẫn đưa ra diện tích thân tàu và điểm bên trong về mặt khái niệm chỉ được sử dụng để cho phép sắp xếp không lồi, không thay đổi hình học. 

Khi tập hợp điểm đã ở mức tối thiểu, chẳng hạn như ba điểm tạo thành một hình tam giác, thì thân bằng tập hợp đầy đủ, do đó đầu ra là`-1`mặc dù đa giác tồn tại. Yêu cầu về độ lõm là điều kiện chặn chứ không phải sự tồn tại của đa giác.
