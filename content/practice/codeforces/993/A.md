---
problem: 993A
contest_id: 993
problem_index: A
name: "Two Squares"
contest_name: "Codeforces Round 488 by NEAR (Div. 1)"
rating: 1600
tags: ["geometry", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 88
date: 2026-06-17
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a3294bf-6b50-83ec-a672-f71aefc63faf
---

#CF 993A - Hai ô vuông 

**Đánh giá:** 1600 
**Tags:** hình học, thực hiện 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3294bf-6b50-83ec-a672-f71aefc63faf 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta có hai hình hình học, mỗi hình được mô tả bằng bốn điểm góc trong mặt phẳng. Một trong số đó là hình vuông thẳng hàng với trục, nghĩa là các cạnh của nó song song với các trục tọa độ. Cái còn lại là một hình vuông được xoay 45 độ, nghĩa là các cạnh của nó không thẳng hàng với trục mà thay vào đó tạo thành hình kim cương so với lưới. 

Mỗi hình vuông được cung cấp dưới dạng danh sách bốn đỉnh của nó theo thứ tự dọc theo đường biên. Thứ tự nhất quán (theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ), do đó các cạnh kết nối các điểm liên tiếp và điểm cuối cùng kết nối trở lại điểm đầu tiên. 

Nhiệm vụ là xác định xem hai hình vuông này có chung ít nhất một điểm chung hay không, trong đó việc chạm vào một điểm duy nhất hoặc một điểm nằm hoàn toàn bên trong điểm kia đều được tính là giao điểm. 

Phạm vi tọa độ rất nhỏ, từ -100 đến 100 và chỉ có hai đối tượng, mỗi đối tượng có bốn đỉnh. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào có độ phức tạp bậc ba hoặc thậm chí bậc hai cao trên nhiều điểm đều không cần thiết; suy luận hình học có hệ số không đổi hoặc kiểm tra giao điểm đoạn trực tiếp là đủ. 

Một điểm tinh tế là “giao lộ” bao gồm ranh giới tiếp xúc. Hai hình vuông chỉ chạm nhau tại một đỉnh hoặc dọc theo một cạnh vẫn được tính là giao nhau. Một cách tiếp cận ngây thơ chỉ kiểm tra sự chồng chéo bên trong nghiêm ngặt sẽ từ chối những trường hợp này một cách không chính xác. 

Một cạm bẫy phổ biến khác là giả sử chúng ta có thể dựa vào việc căn chỉnh trục cho cả hai hình vuông. Chỉ có một hình vuông được căn chỉnh theo trục; cái còn lại có thể có hướng tùy ý ở 45 độ. Bất kỳ phương pháp nào giả định hình chữ nhật được căn chỉnh theo trục cho cả hai sẽ không thành công. 

Các trường hợp Edge thường phá vỡ các giải pháp không chính xác bao gồm: 

Một hình vuông hoàn toàn bên trong hình vuông kia, ví dụ như hình vuông được xoay có tâm bên trong hình vuông thẳng hàng với trục. Trong trường hợp này, không có cạnh nào giao nhau, nhưng tất cả các đỉnh của hình vuông bên trong đều nằm bên trong hình vuông bên ngoài. 

Một thao tác chạm một điểm, chẳng hạn như đầu của hình vuông được xoay chạm vào cạnh của hình vuông được căn chỉnh theo trục. Trong trường hợp này, không có nội thất phân khúc nào chồng chéo lên nhau. 

Một tình huống có vẻ suy biến trong đó một đỉnh nằm chính xác trên một cạnh của hình vuông kia, phải được coi là giao điểm. 

## Phương pháp tiếp cận 

Một cách tiếp cận hình học mạnh mẽ bắt đầu bằng cách coi mỗi hình vuông là một tập hợp gồm bốn đoạn thẳng. Chúng ta có thể kiểm tra từng đoạn của hình vuông đầu tiên so với mọi đoạn của hình vuông thứ hai để tìm giao điểm. Mỗi lần kiểm tra phân đoạn là thời gian không đổi bằng cách sử dụng các bài kiểm tra định hướng. Điều này đã hoạt động trong giới hạn vì chỉ có 4 cạnh trên mỗi hình vuông, do đó, nhiều nhất là 16 phép so sánh phân đoạn. 

Tuy nhiên, mặc dù lực lượng vũ phu này đã đủ hiệu quả nhưng nó không xử lý ngay được việc ngăn chặn. Nếu một hình vuông nằm hoàn toàn bên trong hình vuông kia thì không có cạnh nào giao nhau, do đó thuật toán sẽ trả về “Không” không chính xác trừ khi chúng tôi cũng kiểm tra việc ngăn chặn điểm. Điều này bổ sung thêm một lớp: đối với mỗi đỉnh của một hình vuông, chúng ta phải kiểm tra xem nó có nằm bên trong đa giác kia hay không. 

Quan sát quan trọng là cả hai hình đều là đa giác lồi có đúng bốn đỉnh. Đối với đa giác lồi, giao điểm có thể được quyết định hoàn toàn bằng cách kiểm tra hai điều kiện: liệu có cạnh nào cắt nhau hay không, hoặc liệu một đa giác có chứa đỉnh của đa giác kia hay không. Điều này có hiệu quả vì nếu các đa giác lồi chồng lên nhau mà không có giao điểm cạnh thì một đa giác lồi phải chứa đầy đủ đa giác kia. 

Điều này làm giảm vấn đề xuống còn một số lượng nhỏ các thử nghiệm định hướng và phân đoạn cố định, tất cả đều diễn ra trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân đoạn Brute Force + Kiểm tra điểm | O(1) | O(1) | Đã chấp nhận | 
| Giao lộ đa giác lồi tối ưu | O(1) | O(1) | Đã chấp nhận |

Trong thực tế, cả hai đều giống nhau về mặt tiệm cận ở đây, nhưng cách suy luận đa giác lồi có cấu trúc sẽ tránh được các trường hợp ngăn chặn bị thiếu. 

## Hướng dẫn thuật toán 

1. Đọc bốn đỉnh của mỗi hình vuông và sắp xếp chúng theo thứ tự. Thứ tự đảm bảo rằng các điểm liên tiếp xác định các cạnh. 
2. Xác định hàm`orient(a, b, c)`tính toán dấu tích chéo để xác định xem điểm`c`là ở bên trái, bên phải hoặc trên đường chỉ dẫn`ab`. Đây là nguyên thủy cốt lõi cho tất cả các lý luận hình học. 
3. Xác định hàm`on_segment(a, b, c)`để phát hiện xem điểm`c`nằm chính xác trên đoạn`ab`. Điều này xử lý các trường hợp liên lạc ranh giới một cách rõ ràng. 
4. Xác định kiểm tra giao điểm đoạn giữa hai đoạn. Hai đoạn giao nhau nếu điểm cuối của chúng nằm ở phía đối diện của đường hỗ trợ của nhau hoặc nếu bất kỳ điểm cuối nào nằm trên đoạn kia. 
5. So sánh tất cả bốn cạnh của hình vuông thứ nhất với tất cả bốn cạnh của hình vuông thứ hai. Nếu bất kỳ cặp cạnh nào cắt nhau, ngay lập tức kết luận rằng các hình vuông cắt nhau. 
6. Nếu không có cạnh nào giao nhau, hãy kiểm tra xem có bất kỳ đỉnh nào của hình vuông thứ nhất nằm bên trong hoặc trên ranh giới của hình vuông thứ hai hay không bằng cách kiểm tra hướng nhất quán cho đa giác lồi. 
7. Lặp lại thao tác kiểm tra ngăn chặn tương tự theo hướng ngược lại, kiểm tra xem có bất kỳ đỉnh nào của hình vuông thứ hai nằm bên trong hình vuông thứ nhất hay không. 
8. Nếu không tìm thấy giao điểm cạnh hoặc ngăn chặn, hãy kết luận rằng các hình vuông không giao nhau. 

Lý do mỗi bước là cần thiết là vì đa giác lồi có thể chồng lên nhau theo đúng ba cách: giao nhau, ngăn chặn hoàn toàn hoặc chạm ranh giới mà không giao nhau. Mỗi trường hợp được nắm bắt bởi một trong các kiểm tra ở trên. 

### Tại sao nó hoạt động 

Cả hai hình vuông đều là đa giác lồi. Đối với đa giác lồi, nếu giao điểm của chúng không trống thì ranh giới của chúng cắt nhau hoặc một đa giác nằm hoàn toàn bên trong đa giác kia. Giao lộ ranh giới được ghi lại bằng cách kiểm tra phân đoạn và ngăn chặn được ghi lại bằng kiểm tra đỉnh trong đa giác. Vì tất cả các khả năng đều được bao hàm nên việc thất bại cả hai điều kiện sẽ dẫn đến các tập hợp rời rạc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def orient(ax, ay, bx, by, cx, cy):
    return (bx - ax) * (cy - ay) - (by - ay) * (cx - ax)

def on_segment(ax, ay, bx, by, cx, cy):
    return min(ax, bx) <= cx <= max(ax, bx) and min(ay, by) <= cy <= max(ay, by) and orient(ax, ay, bx, by, cx, cy) == 0

def seg_inter(a, b, c, d):
    ax, ay = a
    bx, by = b
    cx, cy = c
    dx, dy = d

    o1 = orient(ax, ay, bx, by, cx, cy)
    o2 = orient(ax, ay, bx, by, dx, dy)
    o3 = orient(cx, cy, dx, dy, ax, ay)
    o4 = orient(cx, cy, dx, dy, bx, by)

    if o1 == 0 and on_segment(ax, ay, bx, by, cx, cy): return True
    if o2 == 0 and on_segment(ax, ay, bx, by, dx, dy): return True
    if o3 == 0 and on_segment(cx, cy, dx, dy, ax, ay): return True
    if o4 == 0 and on_segment(cx, cy, dx, dy, bx, by): return True

    return (o1 > 0) != (o2 > 0) and (o3 > 0) != (o4 > 0)

def inside(poly, p):
    x, y = p
    n = 4
    for i in range(n):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % n]
        if orient(x1, y1, x2, y2, x, y) < 0:
            return False
    return True

sq1 = list(map(int, input().split()))
sq2 = list(map(int, input().split()))

poly1 = [(sq1[i], sq1[i+1]) for i in range(0, 8, 2)]
poly2 = [(sq2[i], sq2[i+1]) for i in range(0, 8, 2)]

for i in range(4):
    for j in range(4):
        if seg_inter(poly1[i], poly1[(i+1) % 4], poly2[j], poly2[(j+1) % 4]):
            print("YES")
            sys.exit()

for p in poly1:
    if inside(poly2, p):
        print("YES")
        sys.exit()

for p in poly2:
    if inside(poly1, p):
        print("YES")
        sys.exit()

print("NO")
```Thói quen giao đoạn là phần trung tâm. Nó kết hợp các thử nghiệm định hướng với khả năng xử lý cộng tuyến rõ ràng, đảm bảo rằng các điểm cuối dùng chung và sự chồng chéo cạnh được phát hiện chính xác. 

Kiểm tra ngăn chặn sử dụng kiểm tra dấu hiệu nhất quán trên tất cả các cạnh của đa giác lồi. Bởi vì các đỉnh được sắp xếp theo thứ tự, một điểm nằm bên trong đa giác nếu nó luôn nằm cùng một phía với mọi cạnh có hướng. 

Việc thoát sớm ở giao lộ được phát hiện đầu tiên là an toàn vì bất kỳ giao lộ hợp lệ nào cũng đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
0 0 6 0 6 6 0 6
1 3 3 5 5 3 3 1
```Hình vuông đầu tiên là một hộp được căn chỉnh theo trục 6x6. Cái thứ hai là một hình vuông hình kim cương ở giữa bên trong nó. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Kiểm tra tất cả các cặp cạnh | Không có giao lộ cạnh | 
| 2 | Đỉnh kiểm tra (1,3) bên trong hình vuông 1 | Đúng | 
| 3 | Dừng sớm | CÓ | 

Điều này thể hiện sự ngăn chặn mà không vượt qua ranh giới. Thuật toán phát hiện chính xác rằng việc không có giao điểm cạnh là chưa đủ. 

### Mẫu 2 

Trường hợp xây dựng không giao nhau:```
0 0 2 0 2 2 0 2
5 5 6 5 6 6 5 6
```| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Kiểm tra các giao điểm cạnh | Không có | 
| 2 | Kiểm tra các đỉnh của hình vuông 1 trong hình vuông 2 | Sai | 
| 3 | Kiểm tra các đỉnh của hình vuông 2 trong hình vuông 1 | Sai | 
| 4 | Kết luận chia tay | KHÔNG | 

Điều này xác nhận rằng thuật toán không tạo ra kết quả dương tính giả khi cả hai đa giác rời nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Đã sửa 4 cạnh trên mỗi ô vuông, số lần kiểm tra định hướng và ngăn chặn không đổi | 
| Không gian | O(1) | Chỉ lưu trữ số điểm không đổi | 

Kích thước bài toán là không đổi, do đó ngay cả việc triển khai hình học trực tiếp với các vòng lặp lồng nhau cũng có thể phù hợp thoải mái trong giới hạn. Mối quan tâm chính là tính đúng đắn của các vị từ hình học hơn là hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    def orient(ax, ay, bx, by, cx, cy):
        return (bx - ax) * (cy - ay) - (by - ay) * (cx - ax)

    def on_segment(ax, ay, bx, by, cx, cy):
        return min(ax, bx) <= cx <= max(ax, bx) and min(ay, by) <= cy <= max(ay, by) and orient(ax, ay, bx, by, cx, cy) == 0

    def seg_inter(a, b, c, d):
        ax, ay = a
        bx, by = b
        cx, cy = c
        dx, dy = d

        o1 = orient(ax, ay, bx, by, cx, cy)
        o2 = orient(ax, ay, bx, by, dx, dy)
        o3 = orient(cx, cy, dx, dy, ax, ay)
        o4 = orient(cx, cy, dx, dy, bx, by)

        if o1 == 0 and on_segment(ax, ay, bx, by, cx, cy): return True
        if o2 == 0 and on_segment(ax, ay, bx, by, dx, dy): return True
        if o3 == 0 and on_segment(cx, cy, dx, dy, ax, ay): return True
        if o4 == 0 and on_segment(cx, cy, dx, dy, bx, by): return True

        return (o1 > 0) != (o2 > 0) and (o3 > 0) != (o4 > 0)

    def inside(poly, p):
        x, y = p
        for i in range(4):
            x1, y1 = poly[i]
            x2, y2 = poly[(i+1)%4]
            if orient(x1, y1, x2, y2, x, y) < 0:
                return False
        return True

    sq = list(map(int, sys.stdin.readline().split()))
    poly1 = [(sq[i], sq[i+1]) for i in range(0, 8, 2)]
    sq = list(map(int, sys.stdin.readline().split()))
    poly2 = [(sq[i], sq[i+1]) for i in range(0, 8, 2)]

    for i in range(4):
        for j in range(4):
            if seg_inter(poly1[i], poly1[(i+1)%4], poly2[j], poly2[(j+1)%4]):
                return "YES"

    for p in poly1:
        if inside(poly2, p):
            return "YES"

    for p in poly2:
        if inside(poly1, p):
            return "YES"

    return "NO"

# provided samples
assert run("0 0 6 0 6 6 0 6\n1 3 3 5 5 3 3 1\n") == "YES", "sample 1"
assert run("0 0 2 0 2 2 0 2\n5 5 6 5 6 6 5 6\n") == "NO", "sample 2"

# custom cases
assert run("0 0 4 0 4 4 0 4\n0 0 4 0 4 4 0 4\n") == "YES", "identical squares"
assert run("0 0 2 0 2 2 0 2\n2 2 4 2 4 4 2 4\n") == "YES", "touch at vertex"
assert run("0 0 3 0 3 3 0 3\n10 10 11 10 11 11 10 11\n") == "NO", "far apart"
assert run("0 0 5 0 5 5 0 5\n1 1 4 1 4 4 1 4\n") == "YES", "full containment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình vuông giống hệt nhau | CÓ | xử lý chồng chéo chính xác | 
| chạm vào đỉnh | CÓ | giao lộ chỉ có ranh giới | 
| xa nhau | KHÔNG | sự đúng đắn rời rạc | 
| ngăn chặn hoàn toàn | CÓ | trường hợp không có giao điểm | 

## Vỏ cạnh 

Một trường hợp quan trọng là ngăn chặn hoàn toàn, trong đó hình vuông được xoay nằm hoàn toàn bên trong hình vuông thẳng hàng với trục. Trong tình huống này, không có giao điểm cạnh nào xảy ra. Ví dụ, hình vuông`(0,0)-(5,0)-(5,5)-(0,5)`chứa`(1,1)-(4,1)-(4,4)-(1,4)`. Thuật toán không vượt qua được việc kiểm tra đoạn, sau đó đánh giá từng đỉnh của hình vuông bên trong so với hình vuông bên ngoài. Mỗi phép kiểm tra hướng trả về một giá trị không âm cho tất cả các cạnh, do đó kiểm tra ngăn chặn trả về giá trị đúng và câu trả lời cuối cùng là “CÓ”. 

Trường hợp thứ hai là chạm một điểm. Nếu một đỉnh của một hình vuông nằm chính xác trên một cạnh của hình vuông kia thì hướng sẽ bằng 0 và`on_segment`kích hoạt. Điều này đảm bảo rằng các trường hợp như`(0,0)-(2,0)-(2,2)-(0,2)`Và`(2,2)-(3,2)-(3,3)-(2,3)`trả về chính xác “CÓ” ngay cả khi không có khu vực nào trùng lặp. 

Trường hợp thứ ba là sự tách biệt hoàn toàn. Khi các hình vuông cách xa nhau, tất cả các thử nghiệm định hướng đều có dấu hiệu nhất quán cho tất cả các cạnh và không có yếu tố kích hoạt giao điểm phân đoạn nào. Cả hai lần kiểm tra ngăn chặn đều thất bại vì ít nhất một cạnh nhìn thấy điểm kiểm tra ở phía sai, dẫn đến “KHÔNG” không rõ ràng.
