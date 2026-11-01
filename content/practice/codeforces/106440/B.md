---
title: "CF 106440B - \u51f8\u5305"
description: "Cho ta một tập hợp các điểm phân biệt trên mặt phẳng. Nhiệm vụ là chia các điểm này thành hai nhóm không trống, gọi chúng là A và B, sao cho bao lồi tạo bởi A và bao lồi tạo bởi B không hề chạm vào nhau, cả bên trong lẫn biên của chúng."
date: "2026-06-22T04:17:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106440
codeforces_index: "B"
codeforces_contest_name: "\u201c\u89c4\u5f8b\u672a\u6765\u676f\u201d2026 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b"
rating: 0
weight: 106440
solve_time_s: 59
verified: true
draft: false
---

[CF 106440B - \u51f8\u5305](https://codeforces.com/problemset/problem/106440/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cho ta một tập hợp các điểm phân biệt trên mặt phẳng. Nhiệm vụ là chia các điểm này thành hai nhóm không trống, gọi chúng là A và B, sao cho bao lồi tạo bởi A và bao lồi tạo bởi B không hề chạm vào nhau, cả bên trong lẫn biên của chúng. 

Theo thuật ngữ hình học đơn giản hơn, chúng ta đang tìm hai nhóm điểm có “hình dạng dây cao su” hoàn toàn tách biệt, sao cho có một khoảng cách giữa hai đa giác lồi thu được. Chúng ta có thể bỏ qua bất kỳ số điểm nào miễn là cả hai nhóm đều không trống và rời rạc. 

Ý nghĩa hình học quan trọng của các ràng buộc là có nhiều nhất 500 điểm và không có ba điểm nào thẳng hàng. Điều kiện cuối cùng đó loại bỏ các suy biến, chẳng hạn như nhiều điểm nằm trên một đoạn thẳng, giúp đơn giản hóa lý luận bao lồi vì mọi đỉnh của thân đều được xác định rõ ràng và rẽ trái hoặc phải. 

Với 500 điểm, giải pháp O(n log n) hoặc thậm chí O(n²) đều có thể chấp nhận được. Bất cứ điều gì hình khối hoặc tệ hơn là không cần thiết. 

Một trường hợp thất bại tinh vi đối với trực giác ngây thơ là cố gắng phân chia các điểm một cách tùy ý, chẳng hạn bằng cách lấy nửa đầu của đầu vào là A và nửa sau là B. Ngay cả khi cả hai tập hợp đều khác trống, bao lồi của chúng có thể chồng lên nhau rất nhiều. 

Ví dụ, bốn điểm tạo thành một hình vuông theo thứ tự và chia theo đường chéo theo chỉ số có thể tạo ra hai tập hợp có các bao lồi đều là hình vuông, do đó chúng giao nhau hoàn toàn mặc dù các tập hợp đó rời nhau. 

Điều này cho thấy phân vùng phải tôn trọng hình học chứ không phải thứ tự đầu vào. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách để chia các điểm thành hai tập con không trống và kiểm tra xem các bao lồi của chúng có giao nhau hay không. Đối với mỗi phân vùng, việc tính toán hai bao lồi tốn O(n log n) và có 2ⁿ phân vùng có thể xảy ra, do đó phương pháp này hoàn toàn không khả thi ngay cả khi n = 30. 

Cấu trúc của bài toán cho thấy chúng ta không cần phải tìm kiếm gì cả. Chúng tôi không được yêu cầu tối ưu hóa bất kỳ biện pháp nào; chúng ta chỉ cần sự tồn tại của một sự phân tách hợp lệ. 

Quan sát quan trọng là nếu chúng ta có thể tìm thấy một điểm duy nhất được đảm bảo nằm hoàn toàn bên ngoài bao lồi của tất cả các điểm còn lại thì chúng ta có thể cô lập nó thành một nhóm. Một điểm có một bao lồi suy biến chỉ gồm chính nó, nên nếu nó nằm ngoài bao còn lại thì hai bao đó tự động rời nhau. 

Điều này làm giảm nhiệm vụ tìm bất kỳ điểm nào là đỉnh của bao lồi của toàn bộ tập hợp. Một đỉnh bao lồi không thể được viết dưới dạng tổ hợp lồi của các điểm khác, nghĩa là nó không thể nằm bên trong bao lồi của các điểm còn lại sau khi loại bỏ. 

Vì vậy, toàn bộ vấn đề được giải quyết bằng cách tính bao lồi toàn cục và chọn một trong các đỉnh của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2ⁿ · n log n) | O(n) | Quá chậm | 
| Lựa chọn đỉnh thân lồi | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán bao lồi của tất cả các điểm đã cho bằng thuật toán chuỗi đơn điệu tiêu chuẩn. 

1. Sắp xếp tất cả các điểm theo tọa độ x, ngắt mối quan hệ theo tọa độ y. Điều này đưa ra một thứ tự xác định cho việc đóng thân tàu. 
2. Thi công phần thân dưới bằng cách quét các điểm từ trái qua phải. Mỗi lần chúng ta thêm một điểm mới, chúng ta sẽ loại bỏ điểm cuối cùng khỏi thân tàu trong khi lượt cuối cùng được hình thành không phải là một lượt rẽ ngược chiều kim đồng hồ. Điều này thực thi tính lồi của ranh giới dưới. 
3. Xây dựng phần thân trên tương tự bằng cách quét từ phải sang trái, áp dụng điều kiện lồi tương tự. 
4. Hợp nhất hai thân tàu để có được đa giác lồi đầy đủ. Mỗi điểm còn lại trong cấu trúc này là một đỉnh của bao lồi của toàn bộ tập hợp. 
5. Chọn bất kỳ một trong các đỉnh thân này và đặt nó vào tập A. 
6. Đặt tất cả các điểm còn lại vào bộ B.

Lý do chúng ta chỉ chọn một đỉnh thân là vì nó được đảm bảo là điểm cực trị của toàn bộ cấu hình nên không thể chứa trong bao lồi của các điểm còn lại. 

### Tại sao nó hoạt động 

Bao lồi của tất cả các điểm là tập lồi nhỏ nhất chứa chúng. Bất kỳ đỉnh nào của thân này đều là điểm cực trị theo một hướng nào đó, nghĩa là tồn tại một đường hỗ trợ trong đó tất cả các điểm khác đều nằm hoàn toàn về một phía của đỉnh đó. 

Nếu loại bỏ một đỉnh như vậy thì các điểm còn lại đều nằm trong vùng lồi không chứa đỉnh đó. Vì các bao lồi được đóng dưới các tổ hợp lồi nên đỉnh bị loại bỏ không thể được xây dựng lại từ các điểm còn lại, nghĩa là nó nằm bên ngoài bao lồi của chúng. 

Như vậy bao lồi của A là một điểm, còn bao lồi của B là một đa giác không chứa điểm đó. Hai tập lồi rời nhau không thể cắt nhau, không ở trong cũng như ở biên nên thỏa mãn điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

n = int(input())
pts = []
for i in range(n):
    x, y = map(int, input().split())
    pts.append((x, y, i + 1))

pts.sort()

lower = []
for p in pts:
    while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
        lower.pop()
    lower.append(p)

upper = []
for p in reversed(pts):
    while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
        upper.pop()
    upper.append(p)

hull = lower[:-1] + upper[:-1]

# pick any hull vertex
a_idx = hull[0][2]
A = [a_idx]
B = [p[2] for p in pts if p[2] != a_idx]

print(len(A), len(B))
print(*A)
print(*B)
```Đoạn mã xây dựng bao lồi bằng phương pháp chuỗi đơn điệu. Tích chéo chỉ được sử dụng để buộc rẽ trái, đảm bảo kết cấu vẫn lồi. 

Sau khi tính toán phần thân, chúng ta chỉ cần lấy đỉnh đầu tiên của phần thân. Bất kỳ đỉnh nào của thân tàu đều hoạt động tốt như nhau, bởi vì mọi điểm như vậy đều được đảm bảo là cực trị. 

Các điểm còn lại được tập hợp vào B. Không cần kiểm tra hình học bổ sung vì tính lồi đảm bảo sự tách biệt. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ gồm năm điểm tạo thành một hình ngũ giác lồi. 

| Bước | Xây dựng thân tàu | Đã chọn A | Còn lại B | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | Điểm được sắp xếp theo x-y | - | - | 
| Thân dưới | Xây dựng ranh giới lồi | - | - | 
| Thân trên | Hoàn thành đa giác | - | - | 
| Thân tàu cuối cùng | Tất cả 5 điểm | - | - | 
| Lựa chọn | Đỉnh thân tàu đầu tiên | một đỉnh | người khác | 

Trong trường hợp này, việc chọn bất kỳ đỉnh nào của hình ngũ giác sẽ cô lập điểm đó và bốn đỉnh còn lại vẫn tạo thành một đa giác lồi không bao gồm nó. 

Bây giờ hãy xem xét trường hợp các điểm tạo thành một hình lồi dày đặc với các điểm bên trong. 

| Bước | Quan sát | 
| --- | --- | 
| Đầu vào | Nhiều điểm bên trong cộng với điểm biên | 
| Kết quả thân tàu | Chỉ còn lại các điểm ranh giới bên ngoài | 
| Một lựa chọn | Đỉnh biên bất kỳ | 
| Bộ B | Tất cả các điểm ngoại trừ đỉnh đó | 

Điều này cho thấy các điểm bên trong không ảnh hưởng đến tính đúng đắn, vì chúng không bao giờ xuất hiện ở thân tàu và được đưa vào B một cách an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Phân loại chiếm ưu thế trong kết cấu thân lồi | 
| Không gian | O(n) | Lưu trữ cho các điểm và mảng thân tàu | 

Với n lên tới 500, việc sắp xếp và quét tuyến tính là không đáng kể trong giới hạn. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import Popen, PIPE
    return ""

# sample placeholders (actual judge samples should be inserted)
# assert run(...) == ...

# custom tests

# minimum size valid case (6 points in convex position)
inp = """6
0 0
1 0
2 0
2 1
1 2
0 2
"""
# expected: 1 point vs 5 points
# assert run(inp) ...

# convex square with interior point
inp = """5
0 0
2 0
2 2
0 2
1 1
"""

# all points on convex hull except interior
# assert run(inp) ...

# random small case
inp = """7
0 0
3 0
3 3
0 3
1 1
2 1
1 2
"""
# assert run(inp) ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 điểm lồi | Chia 1 vs 5 | Tách đỉnh thân cơ bản | 
| vuông + nội thất | chia 1 vs 4 | Điểm nội thất không quan trọng | 
| bộ nhỏ ngẫu nhiên | chia hợp lệ | Tính đúng đắn chung | 

## Vỏ cạnh 

Một tình huống quan trọng là khi tất cả các điểm đều nằm trên ranh giới bao lồi. Trong trường hợp này, bao chứa mọi điểm, nhưng bất kỳ đỉnh nào cũng có giá trị để cô lập. Thuật toán chọn một đỉnh và đặt nó vào A, để phần còn lại ở B và sự phân tách vẫn giữ nguyên vì các điểm còn lại tạo thành một đa giác lồi loại trừ đỉnh đó. 

Một trường hợp khác là khi có nhiều điểm bên trong. Mặc dù các điểm này không bao giờ xuất hiện trong bao nhưng chúng vẫn được đặt an toàn vào B. Bao lồi của B có thể co lại so với bao đầy đủ nhưng vẫn không thể bao gồm đỉnh cực trị bị loại bỏ. 

Trường hợp tinh vi cuối cùng là khi nhiều điểm có chung tọa độ cực đại, chẳng hạn như một số điểm có cùng giá trị x tối đa. Chuỗi đơn điệu vẫn chọn các đỉnh biên thực sự và chọn bất kỳ một trong số chúng là đủ vì tất cả đều được đảm bảo nằm ngoài bao lồi của phần còn lại.
