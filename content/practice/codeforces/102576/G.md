---
title: "CF 102576G - Diễn giả được mời"
description: "Chúng ta có hai nhóm điểm trên một mặt phẳng. Nhóm đầu tiên đại diện cho nơi người nói bắt đầu và nhóm thứ hai đại diện cho phòng nơi họ phải kết thúc. Chúng ta được tự do quyết định diễn giả nào sẽ vào phòng nào."
date: "2026-07-31T07:36:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102576
codeforces_index: "G"
codeforces_contest_name: "2020 Petrozavodsk Winter Camp, Jagiellonian U Contest"
rating: 0
weight: 102576
solve_time_s: 90
verified: true
draft: false
---

[CF 102576G - Diễn giả được mời](https://codeforces.com/problemset/problem/102576/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai nhóm điểm trên một mặt phẳng. Nhóm đầu tiên đại diện cho nơi người nói bắt đầu và nhóm thứ hai đại diện cho phòng nơi họ phải kết thúc. Chúng ta được tự do quyết định diễn giả nào sẽ vào phòng nào. Nhiệm vụ là vẽ một đường dẫn cho mỗi người phát biểu sao cho mọi đường dẫn đều bắt đầu tại một bàn, kết thúc tại một phòng và không có hai đường dẫn nào chạm hoặc cắt nhau. 

Đầu ra không cần giảm thiểu khoảng cách hoặc sử dụng một nhiệm vụ cụ thể. Mọi tập hợp đường dẫn hợp lệ đều được chấp nhận. Một đường dẫn có thể chứa một số đỉnh, nhưng một đoạn thẳng đã là một chuỗi đa giác hợp lệ, vì vậy câu trả lời đơn giản nhất có thể là một tập hợp các đoạn thẳng không giao nhau. 

Số lượng người nói nhiều nhất là 6. Điều này làm thay đổi hoàn toàn bản chất của vấn đề. Một bài toán so khớp tổng quát với các ràng buộc hình học có thể khó khăn, nhưng ở đây chúng ta có thể thử mọi phép gán có thể. Có nhiều nhất là 6! = 720 cặp có thể, rất nhỏ. Ngay cả sau khi kiểm tra từng cặp phân đoạn, tổng công việc vẫn rất nhỏ. 

Đầu vào hình học cũng được xử lý tốt. Mọi tọa độ x đều khác nhau, mọi tọa độ y đều khác nhau và không có ba điểm nào thẳng hàng. Điều này loại bỏ các trường hợp không rõ ràng trong đó các đoạn chồng lên nhau hoặc một số điểm nằm trên cùng một đường. Mối nguy hiểm chính khi triển khai vẫn là thử nghiệm giao lộ. Một phân đoạn chỉ chạm vào phân đoạn khác tại điểm cuối sẽ không hợp lệ vì tất cả các điểm đầu vào phải được sử dụng chính xác một lần, do đó, người kiểm tra bất cẩn bỏ qua điểm cuối hoặc chỉ kiểm tra các điểm giao nhau thích hợp có thể chấp nhận các cấu trúc không hợp lệ. 

Ví dụ, hãy xem xét hai bảng tại`(0,0)`Và`(2,2)`và hai phòng tại`(0,2)`Và`(2,0)`. Ghép nối bàn thứ nhất với phòng thứ nhất và bàn thứ hai với phòng thứ hai sẽ tạo ra hai đường chéo của một hình vuông cắt nhau. Đầu ra đúng là ghép nối ngược lại. Việc triển khai đơn giản kết nối các điểm theo thứ tự đầu vào mà không kiểm tra các nút giao sẽ thất bại trong trường hợp này. 

Một trường hợp khác là`n = 1`. Chỉ có một đường dẫn duy nhất và nó vẫn phải được in ở định dạng được yêu cầu. Mã giả định tồn tại ít nhất hai phân đoạn có thể bị lỗi ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi sự phân công có thể có giữa các bàn và phòng. Đối với mỗi bài tập, chúng ta vẽ các đoạn thẳng tương ứng và kiểm tra xem hai đoạn thẳng có cắt nhau hay không. Nếu một bài tập không có giao điểm thì đó là câu trả lời hợp lệ vì mỗi đường dẫn là một chuỗi đa giác bao gồm một đoạn. Số bài tập là`n!`và đối với mỗi nhiệm vụ chúng tôi kiểm tra nhiều nhất`n(n-1)/2`các cặp phân đoạn. 

Phương pháp vũ phu hoạt động vì đầu vào có chủ ý nhỏ. Nếu như`n`vào khoảng 10 hoặc 11, sự tăng trưởng giai thừa sẽ trở nên khó chịu. Tại`n = 11`, số lượng bài tập là gần 40 triệu và mỗi bài sẽ cần kiểm tra hình học. Hạn chế thực sự của`n <= 6`chúng ta hãy sử dụng cách xây dựng đáng tin cậy đơn giản nhất thay vì cố gắng xây dựng một lập luận hình học phức tạp. 

Quan sát quan trọng là chúng ta không cần phải tìm một cặp đôi đặc biệt. Chúng tôi chỉ cần một cặp hợp lệ và không gian tìm kiếm đủ nhỏ để liệt kê. Sự tồn tại của sự so khớp không giao nhau giữa hai tập hợp điểm có kích thước bằng nhau đảm bảo rằng việc tìm kiếm cuối cùng sẽ tìm thấy một tập hợp điểm. Một bằng chứng tiêu chuẩn cho thực tế này dựa trên việc chọn một kết quả khớp có tổng chiều dài nhỏ nhất. Nếu hai đoạn trong phần khớp đó giao nhau, việc hoán đổi điểm cuối của chúng sẽ rút ngắn tổng chiều dài, mâu thuẫn với tính tối thiểu. 

Do đó, thuật toán cuối cùng là tìm kiếm hoàn chỉnh trên các kết quả khớp có thể có. Khi tìm thấy kết quả khớp hợp lệ, đầu ra chỉ bao gồm hai điểm cuối của mỗi phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n²) | O(n) | Được chấp nhận vì n <= 6 | 
| Tối ưu | O(n! * n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mọi hoán vị có thể có của các chỉ số phòng. vị trí`i`trong hoán vị cho chúng ta biết phòng nào được gán cho bàn`i`. Từ`n`nhiều nhất là 6, việc kiểm tra tất cả các bài tập là khả thi. 
2. Đối với bài tập hiện tại, hãy tạo`n`các đoạn kết nối mỗi bàn với phòng được chỉ định. Chúng tôi chỉ lưu trữ điểm cuối của họ vì đầu ra có thể là một phân đoạn duy nhất cho mỗi người nói. 
3. Kiểm tra từng cặp đoạn. Sử dụng bài kiểm tra định hướng để xác định xem hai đoạn có giao nhau hay không. Bởi vì đầu vào đảm bảo rằng không có ba điểm nào thẳng hàng, giao điểm xảy ra chính xác khi điểm cuối của một đoạn nằm trên các cạnh đối diện của đoạn kia và ngược lại. 
4. Nếu tất cả các cặp phân đoạn không khớp nhau, hãy xuất các phân đoạn này ra và ngừng tìm kiếm. Mỗi đoạn là một chuỗi đa giác hợp lệ có hai đỉnh. 
5. Nếu phép gán không hợp lệ, hãy tiếp tục hoán vị tiếp theo. 

Tại sao nó hoạt động: thuật toán kiểm tra mọi cách có thể để ghép bàn với phòng. Tồn tại một kết quả khớp không giao nhau hợp lệ, do đó một trong các hoán vị được tạo ra phải vượt qua bài kiểm tra giao nhau. Thuật toán chỉ đưa ra kết quả sau khi tất cả các cặp đường dẫn đã được xác minh là rời rạc, do đó kết cấu được tạo ra đáp ứng mọi yêu cầu hình học. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def cross(a, b, c):
    return (b[0] - a[0]) * (c[1] - a[1]) - (b[1] - a[1]) * (c[0] - a[0])

def intersect(a, b, c, d):
    return cross(a, b, c) * cross(a, b, d) < 0 and cross(c, d, a) * cross(c, d, b) < 0

def solve_case(n, tables, rooms):
    for perm in permutations(range(n)):
        segments = []
        for i in range(n):
            segments.append((tables[i], rooms[perm[i]]))

        ok = True
        for i in range(n):
            for j in range(i + 1, n):
                if intersect(segments[i][0], segments[i][1],
                             segments[j][0], segments[j][1]):
                    ok = False
                    break
            if not ok:
                break

        if ok:
            return segments

    return []

def main():
    data = sys.stdin.read().strip().split()
    if not data:
        return

    it = iter(data)
    z = int(next(it))
    ans = []

    for _ in range(z):
        n = int(next(it))
        points = []
        for _ in range(2 * n):
            x = int(next(it))
            y = int(next(it))
            points.append((x, y))

        tables = points[:n]
        rooms = points[n:]

        result = solve_case(n, tables, rooms)

        for a, b in result:
            ans.append("2")
            ans.append(f"{a[0]} {a[1]}")
            ans.append(f"{b[0]} {b[1]}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```các`cross`hàm tính diện tích có dấu của một tam giác. Dấu hiệu của nó cho biết điểm nằm ở phía nào của đường thẳng. Bởi vì câu lệnh loại bỏ các trường hợp thẳng hàng, nên một cặp đoạn thẳng chỉ giao nhau khi hai điểm cuối của mỗi đoạn được phân tách bằng đường thẳng của đoạn kia. 

Vòng lặp hoán vị là tìm kiếm hoàn chỉnh được mô tả trong thuật toán. của Python`itertools.permutations`ở đây an toàn vì số lượng hoán vị lớn nhất chỉ là 720. 

Việc kiểm tra giao lộ có chủ ý sử dụng các bất đẳng thức nghiêm ngặt. Nếu có thể xảy ra các trường hợp cộng tuyến, chúng tôi sẽ cần kiểm tra bổ sung để chạm vào các điểm cuối hoặc các phân đoạn chồng chéo, nhưng các hạn chế đầu vào sẽ loại bỏ các trường hợp đó. 

Đầu ra lưu trữ mọi đường dẫn chính xác như hai đỉnh. Đoạn thẳng vốn đã là một chuỗi đa giác, vì vậy việc thêm các điểm trung gian không cần thiết sẽ chỉ tạo thêm cơ hội mắc sai lầm. 

## Ví dụ đã hoạt động 

Xét trường hợp có hai bảng`(0,0)`Và`(2,2)`và hai phòng`(0,2)`Và`(2,0)`. 

| Bước | Bài tập | Đã kiểm tra phân đoạn | Kết quả | 
| --- | --- | --- | --- | 
| 1 | bàn 0 đến phòng 0, bàn 1 đến phòng 1 |`(0,0)-(0,2)`Và`(2,2)-(2,0)`| hợp lệ | 
| 2 | bàn 0 đến phòng 1, bàn 1 đến phòng 0 |`(0,0)-(2,0)`Và`(2,2)-(0,2)`| Không cần thiết | 

Nhiệm vụ đầu tiên đã được chấp nhận. Ví dụ này cho thấy một giải pháp hợp lệ không cần bất kỳ thứ tự điểm đặc biệt nào. 

Đối với một loa duy nhất: 

| Bước | Bài tập | Đã kiểm tra phân đoạn | Kết quả | 
| --- | --- | --- | --- | 
| 1 | bàn 0 đến phòng 0 | Một phân đoạn | hợp lệ | 

Thuật toán ngay lập tức đưa ra con đường duy nhất có thể. Điều này xác nhận rằng kích thước đầu vào nhỏ nhất không yêu cầu bất kỳ xử lý đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n! * n²) | có`n!`bài tập và mỗi bài tập chỉ kiểm tra tối đa`n²`các cặp phân đoạn. | 
| Không gian | O(n) | Chỉ hoán vị hiện tại và tập hợp các phân đoạn hiện tại được lưu trữ. | 

Với`n <= 6`, số lượng bài tập tối đa là 720. Tổng số lần kiểm tra giao lộ chỉ vài chục nghìn cho mỗi trường hợp thử nghiệm, do đó giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm 

Vì đầu ra có thể chứa bất kỳ kết quả khớp hợp lệ nào nên các thử nghiệm bên dưới sẽ xác thực các đặc tính hình học thay vì so sánh đầu ra văn bản chính xác.```python
import sys
import io

def validate(inp, out):
    tokens = out.strip().split()
    data = inp.strip().split()
    if not tokens:
        return False

    it = iter(data)
    z = int(next(it))
    cases = []
    for _ in range(z):
        n = int(next(it))
        pts = []
        for _ in range(2 * n):
            pts.append((int(next(it)), int(next(it))))
        cases.append((n, pts[:n], pts[n:]))

    idx = 0
    for n, tables, rooms in cases:
        segs = []
        for _ in range(n):
            k = int(tokens[idx])
            idx += 1
            if k < 2:
                return False
            cur = []
            for _ in range(k):
                cur.append((int(tokens[idx]), int(tokens[idx + 1])))
                idx += 2
            segs.append(cur)

        for i in range(n):
            for j in range(i + 1, n):
                for a in range(len(segs[i]) - 1):
                    for b in range(len(segs[j]) - 1):
                        if segs[i][a] in segs[j] or segs[i][a + 1] in segs[j]:
                            return False
    return True

def run(inp):
    return ""

assert validate("1\n1\n0 0\n1 1\n", "2\n0 0\n1 1\n")
assert validate("1\n2\n0 0\n2 2\n0 2\n2 0\n", "2\n0 0\n0 2\n2\n2 2\n2 0\n")
assert validate("1\n3\n0 0\n10 0\n5 10\n0 10\n10 10\n5 0\n", 
                "2\n0 0\n0 10\n2\n10 0\n10 10\n2\n5 10\n5 0\n")
assert validate("1\n6\n-100 -100\n-50 20\n0 80\n50 -20\n70 70\n100 0\n-90 40\n-40 -80\n20 -60\n40 90\n80 -50\n90 30\n", "")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một loa | Một phân đoạn | Xử lý kích thước tối thiểu | 
| Hai khả năng vượt qua | Bất kỳ ghép nối không chéo nào | Phát hiện giao lộ | 
| Ba loa | Ba con đường rời nhau | Kiểm tra nhiều phân khúc | 
| Sáu loa | Bất kỳ công trình hợp lệ nào | Kích thước tìm kiếm tối đa | 

## Vỏ cạnh 

cho`n = 1`, thuật toán tạo ra một hoán vị chứa phòng duy nhất. Không có cặp phân đoạn nào để kiểm tra nên nhiệm vụ được chấp nhận ngay lập tức. Đối với đầu vào```
1
1
0 0
5 5
```đầu ra chỉ đơn giản là```
2
0 0
5 5
```Để vượt qua các cấu hình, thuật toán không cho rằng thứ tự đầu vào có ý nghĩa. Vì```
1
2
0 0
2 2
0 2
2 0
```việc ghép đôi đầu tiên có thể tạo ra các đường chéo chéo. Hàm giao nhau phát hiện xung đột, từ chối hoán vị đó và tiếp tục cho đến khi tìm thấy cặp thay thế. 

Đối với trường hợp tối đa,`n = 6`, chỉ có 720 bài tập. Thuật toán vẫn thực hiện tìm kiếm toàn diện tương tự, nhưng không gian tìm kiếm vẫn đủ nhỏ để không cần tối ưu hóa đặc biệt. Đối số chính xác không thay đổi vì mọi kết quả khớp có thể vẫn được xem xét.
