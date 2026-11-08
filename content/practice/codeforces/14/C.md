---
title: "CF 14C - Bốn đoạn"
description: "Chúng ta có chính xác bốn đoạn thẳng trên mặt phẳng 2D. Mọi phân đoạn đều được căn chỉnh theo trục hoặc thậm chí có thể thoái hóa thành một điểm duy nhất."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "geometry", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 14
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 14 (Div. 2)"
rating: 1700
weight: 14
solve_time_s: 108
verified: true
draft: false
---
[CF 14C - Bốn phân đoạn](https://codeforces.com/problemset/problem/14/C) 

**Đánh giá:** 1700 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, hình học, cách thực hiện, toán học 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có chính xác bốn đoạn thẳng trên mặt phẳng 2D. Mọi phân đoạn đều được căn chỉnh theo trục hoặc thậm chí có thể thoái hóa thành một điểm duy nhất. Nhiệm vụ là quyết định xem bốn đoạn này có cùng nhau tạo thành đường viền của một hình chữ nhật có diện tích dương hay không, trong đó tất cả các cạnh hình chữ nhật đều song song với các trục tọa độ. 

Một hình chữ nhật hợp lệ trong bài toán này có cấu trúc rất chặt chẽ. Phải có đúng hai cạnh ngang và đúng hai cạnh dọc. Các cạnh ngang phải có chung điểm cuối bên trái và bên phải, còn các cạnh dọc phải kết nối các điểm cuối đó. Hình chữ nhật cũng phải có chiều rộng khác 0 và chiều cao khác 0. 

Giới hạn tọa độ rất lớn, lên tới$10^9$, nhưng chỉ có bốn phân đoạn. Điều đó làm thay đổi hoàn toàn bản chất của vấn đề. Chúng tôi không cần hình học hoặc tối ưu hóa phức tạp. Ngay cả việc kiểm tra mọi cách sắp xếp có thể có của bốn đoạn cũng là chuyện nhỏ vì$4! = 24$. Thách thức thực sự là tính chính xác chứ không phải hiệu suất. 

Một số trường hợp đặc biệt khiến việc triển khai đơn giản không thành công. 

Một lỗi phổ biến là chỉ kiểm tra sự tồn tại của hai tọa độ x và hai tọa độ y riêng biệt. Hãy xem xét đầu vào này:```
0 0 2 0
0 1 2 1
0 0 0 0
2 0 2 1
```Đoạn thứ ba chỉ là một điểm nên thiếu cạnh trái của hình chữ nhật. Câu trả lời đúng là`NO`. Một giải pháp bất cẩn có thể chấp nhận sai vì cả bốn tọa độ góc đều xuất hiện ở đâu đó. 

Một trường hợp nguy hiểm khác là các phân đoạn bị chồng chéo hoặc trùng lặp:```
0 0 2 0
0 0 2 0
0 0 0 1
2 0 2 1
```Có bốn đoạn, nhưng thực tế chỉ tồn tại một cạnh ngang. Câu trả lời đúng là`NO`. 

Trường hợp tinh vi thứ ba là hình chữ nhật có diện tích bằng 0:```
0 0 0 0
0 1 0 1
0 0 0 1
0 0 0 1
```Tất cả các điểm nằm trên một đường thẳng đứng. Chiều rộng bằng 0 nên không tồn tại hình chữ nhật. Câu trả lời đúng là`NO`. 

Một nguồn lỗi khác là thứ tự điểm cuối bị đảo ngược. Hai đoạn này biểu thị cùng một đường hình học:```
0 0 3 0
3 0 0 0
```Bất kỳ giải pháp đúng nào cũng phải bình thường hóa hướng phân đoạn trước khi so sánh chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của bốn đoạn và hỏi xem liệu chúng có thể đóng vai trò ở các cạnh trên, dưới, trái và phải của hình chữ nhật hay không. 

Điều này hoạt động vì chỉ có bốn phân đoạn. Đối với mỗi hoán vị, chúng ta có thể xác minh: 

1. Hai đoạn đầu nằm ngang. 
2. Hai đoạn cuối là dọc. 
3. Các đoạn ngang có cùng phạm vi x. 
4. Các đoạn thẳng đứng có cùng phạm vi y. 
5. Các điểm cuối kết nối hoàn hảo thành một hình chữ nhật. 
6. Chiều rộng và chiều cao đều dương. 

Vì chỉ có 24 hoán vị nên cách tiếp cận này đã đủ nhanh. 

Quan sát thú vị là chúng ta thực sự không cần hoán vị nếu chúng ta phân loại các đoạn theo hướng. Một hình chữ nhật thẳng hàng với các trục có cấu trúc cứng nhắc: 

- chính xác là hai đoạn ngang, 
- chính xác là hai đoạn thẳng đứng, 
- phạm vi tọa độ phù hợp. 

Khi chúng tôi tách các phân đoạn thành các nhóm ngang và dọc, việc xác minh sẽ mang tính quyết định. 

Giải pháp brute-force thành công vì kích thước đầu vào rất nhỏ. Việc quan sát cấu trúc của hình chữ nhật cho phép chúng ta đơn giản hóa logic thành những so sánh trực tiếp thay vì thử mọi cách sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4!) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn đoạn. 
2. Chuẩn hóa mọi phân đoạn để các điểm cuối của nó được sắp xếp nhất quán. 

Đối với các đoạn ngang, trước tiên hãy lưu tọa độ x nhỏ hơn. Đối với các đoạn thẳng đứng, trước tiên hãy lưu trữ tọa độ y nhỏ hơn. Điều này tránh được các lỗi do thứ tự đầu vào bị đảo ngược. 
3. Phân loại từng phân khúc. 

Nếu như$y_1 = y_2$, đoạn thẳng nằm ngang. 

Nếu như$x_1 = x_2$, đoạn thẳng đứng. 

Ngược lại, đoạn này có đường chéo và không bao giờ có thể thuộc hình chữ nhật được yêu cầu, vì vậy hãy trả về`NO`. 
4. Xác minh rằng có chính xác hai đoạn ngang và hai đoạn dọc. 

Bất kỳ hình chữ nhật căn chỉnh trục hợp lệ nào cũng có cấu trúc này. 
5. Gọi các đoạn ngang là:$$(x_1, y_a) \to (x_2, y_a)$$Và$$(x_1', y_b) \to (x_2', y_b)$$Kiểm tra xem:$$x_1 = x_1'$$Và$$x_2 = x_2'$$Điều này đảm bảo cả hai mặt ngang đều có cùng chiều rộng. 
6. Gọi các đoạn thẳng đứng là:$$(x_c, y_1) \to (x_c, y_2)$$Và$$(x_d, y_1') \to (x_d, y_2')$$Kiểm tra xem:$$y_1 = y_1'$$Và$$y_2 = y_2'$$Điều này đảm bảo cả hai mặt thẳng đứng có cùng chiều cao. 
7. Xác minh rằng các góc hình chữ nhật khớp chính xác. 

Tọa độ x bên trái và bên phải tính từ các cạnh ngang phải bằng tọa độ x các cạnh dọc. 

Tọa độ y phía dưới và phía trên tính từ các cạnh thẳng đứng phải bằng tọa độ y của các cạnh ngang. 
8. Kiểm tra xem chiều rộng và chiều cao có dương không. 

Chiều rộng là:$$x_2 - x_1$$Chiều cao là:$$y_2 - y_1$$Cả hai đều phải lớn hơn 0. 
9. Nếu tất cả các bước kiểm tra đều đạt, hãy in`YES`. Nếu không thì in`NO`. 

### Tại sao nó hoạt động 

Một hình chữ nhật song song với các trục được xác định hoàn toàn bởi hai giá trị x và hai giá trị y riêng biệt. Bốn phía phải kết nối chính xác các tọa độ đó. 

Thuật toán xác minh mọi thuộc tính hình học cần thiết cho một hình chữ nhật như vậy: 

- định hướng chính xác, 
- chiều dài cạnh phù hợp, 
- điểm cuối được chia sẻ, 
- khu vực tích cực. 

Nếu bất kỳ điều kiện nào không thành công thì ít nhất một cạnh hình chữ nhật bị thiếu, bị trùng lặp, bị ngắt kết nối hoặc bị thoái hóa. Nếu tất cả các điều kiện đều đúng thì bốn đoạn này sẽ tạo thành đường viền hình chữ nhật duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize(x1, y1, x2, y2):
    if x1 == x2:
        if y1 > y2:
            y1, y2 = y2, y1
    else:
        if x1 > x2:
            x1, x2 = x2, x1
    return (x1, y1, x2, y2)

def solve():
    horizontal = []
    vertical = []

    for _ in range(4):
        x1, y1, x2, y2 = map(int, input().split())

        x1, y1, x2, y2 = normalize(x1, y1, x2, y2)

        if y1 == y2:
            horizontal.append((x1, y1, x2, y2))
        elif x1 == x2:
            vertical.append((x1, y1, x2, y2))
        else:
            print("NO")
            return

    if len(horizontal) != 2 or len(vertical) != 2:
        print("NO")
        return

    h1 = horizontal[0]
    h2 = horizontal[1]

    v1 = vertical[0]
    v2 = vertical[1]

    # Horizontal sides must share x-range
    if h1[0] != h2[0] or h1[2] != h2[2]:
        print("NO")
        return

    # Vertical sides must share y-range
    if v1[1] != v2[1] or v1[3] != v2[3]:
        print("NO")
        return

    left_x = h1[0]
    right_x = h1[2]

    bottom_y = v1[1]
    top_y = v1[3]

    # Positive area
    if left_x == right_x or bottom_y == top_y:
        print("NO")
        return

    xs = sorted([v1[0], v2[0]])
    ys = sorted([h1[1], h2[1]])

    if xs != [left_x, right_x]:
        print("NO")
        return

    if ys != [bottom_y, top_y]:
        print("NO")
        return

    print("YES")

solve()
```Hàm trợ giúp đầu tiên chuẩn hóa hướng phân đoạn. Điều này rất quan trọng vì đầu vào không đảm bảo thứ tự điểm cuối. Nếu không chuẩn hóa, hai phân đoạn giống hệt nhau được viết theo hướng ngược nhau sẽ không kiểm tra được sự bằng nhau. 

Chức năng chính phân tách các phân đoạn theo hướng. Bất kỳ phân đoạn nào không nằm ngang cũng không dọc sẽ ngay lập tức làm mất hiệu lực cấu hình. 

Sau khi phân loại, mã sẽ kiểm tra trực tiếp cấu trúc hình chữ nhật. Hai đoạn ngang phải kéo dài cùng một khoảng trên trục x và hai đoạn dọc phải kéo dài cùng một khoảng trên trục y. 

Việc kiểm tra vùng dương sẽ ngăn chặn các hình chữ nhật suy biến có chiều rộng hoặc chiều cao bằng 0. 

So sánh tọa độ cuối cùng đảm bảo rằng cả bốn phía đều kết nối nhất quán. Điều này phát hiện các trường hợp có phân đoạn bị ngắt kết nối hoặc đặt sai vị trí ngay cả khi độ dài có vẻ chính xác. 

Vì tọa độ vừa vặn một cách an toàn bên trong số nguyên Python nên tràn không bao giờ là vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 6 1
1 0 6 0
6 0 6 1
1 1 1 0
```| Bước | Phân đoạn ngang | Phân đoạn dọc | Kết quả | 
| --- | --- | --- | --- | 
| Sau khi phân loại | (1,1)-(6,1), (1,0)-(6,0) | (6,0)-(6,1), (1,0)-(1,1) | hợp lệ | 
| Kiểm tra tầm X | cả hai đều sử dụng x = 1 đến 6 | - | vượt qua | 
| Kiểm tra phạm vi Y | - | cả hai đều sử dụng y = 0 đến 1 | vượt qua | 
| Kiểm tra khu vực | chiều rộng = 5 | chiều cao = 1 | vượt qua | 
| Trận chung kết | giá trị x = {1,6}, giá trị y = {0,1} | nhất quán | CÓ | 

Ví dụ này thể hiện cấu trúc hình chữ nhật lý tưởng. Mọi bên đều kết nối hoàn hảo và cả hai chiều đều tích cực. 

### Ví dụ 2 

đầu vào:```
0 0 2 0
0 1 2 1
0 0 0 0
2 0 2 1
```| Bước | Phân đoạn ngang | Phân đoạn dọc | Kết quả | 
| --- | --- | --- | --- | 
| Sau khi phân loại | (0,0)-(2,0), (0,1)-(2,1), (0,0)-(0,0) | (2,0)-(2,1) | không hợp lệ | 
| Kiểm tra đếm | 3 ngang | 1 dọc | thất bại | 
| Câu trả lời cuối cùng | - | - | KHÔNG | 

Đoạn điểm suy biến được phân loại là nằm ngang vì cả hai giá trị y đều bằng nhau. Thuật toán từ chối chính xác đầu vào vì hình chữ nhật yêu cầu chính xác hai cạnh ngang và hai cạnh dọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có bốn phân đoạn được xử lý | 
| Không gian | O(1) | Một vài danh sách và biến có kích thước cố định được sử dụng | 

Giải pháp dễ dàng phù hợp trong giới hạn. Ngay cả việc kiểm tra toàn diện ở đây cũng không đáng kể vì kích thước đầu vào không bao giờ tăng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def normalize(x1, y1, x2, y2):
        if x1 == x2:
            if y1 > y2:
                y1, y2 = y2, y1
        else:
            if x1 > x2:
                x1, x2 = x2, x1
        return (x1, y1, x2, y2)

    horizontal = []
    vertical = []

    for _ in range(4):
        x1, y1, x2, y2 = map(int, input().split())

        x1, y1, x2, y2 = normalize(x1, y1, x2, y2)

        if y1 == y2:
            horizontal.append((x1, y1, x2, y2))
        elif x1 == x2:
            vertical.append((x1, y1, x2, y2))
        else:
            return "NO"

    if len(horizontal) != 2 or len(vertical) != 2:
        return "NO"

    h1, h2 = horizontal
    v1, v2 = vertical

    if h1[0] != h2[0] or h1[2] != h2[2]:
        return "NO"

    if v1[1] != v2[1] or v1[3] != v2[3]:
        return "NO"

    left_x = h1[0]
    right_x = h1[2]

    bottom_y = v1[1]
    top_y = v1[3]

    if left_x == right_x or bottom_y == top_y:
        return "NO"

    xs = sorted([v1[0], v2[0]])
    ys = sorted([h1[1], h2[1]])

    if xs != [left_x, right_x]:
        return "NO"

    if ys != [bottom_y, top_y]:
        return "NO"

    return "YES"

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""1 1 6 1
1 0 6 0
6 0 6 1
1 1 1 0
"""
) == "YES", "sample 1"

# zero area rectangle
assert run(
"""0 0 0 0
0 1 0 1
0 0 0 1
0 0 0 1
"""
) == "NO", "zero width"

# duplicated segment
assert run(
"""0 0 2 0
0 0 2 0
0 0 0 1
2 0 2 1
"""
) == "NO", "duplicate horizontal side"

# reversed endpoints
assert run(
"""3 0 0 0
0 2 3 2
0 0 0 2
3 2 3 0
"""
) == "YES", "normalization check"

# diagonal segment
assert run(
"""0 0 1 1
0 1 1 1
0 0 0 1
1 0 1 1
"""
) == "NO", "diagonal edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đường dọc suy biến | KHÔNG | Yêu cầu diện tích dương | 
| Đoạn ngang trùng lặp | KHÔNG | Thiếu cạnh hình chữ nhật | 
| Điểm cuối đảo ngược | CÓ | Chuẩn hóa đúng | 
| Một đoạn chéo | KHÔNG | Xác thực định hướng | 

## Vỏ cạnh 

### Các đoạn suy biến được coi là các cạnh hợp lệ 

đầu vào:```
0 0 2 0
0 1 2 1
0 0 0 0
2 0 2 1
```Đoạn thứ ba là một điểm duy nhất. Trong quá trình phân loại, nó trở thành một đoạn ngang vì giá trị y của nó khớp với nhau. Thuật toán kết thúc với ba đoạn ngang và một đoạn dọc nên ngay lập tức từ chối cấu hình với`NO`. 

Điều này ngăn không cho các đoạn điểm giả vờ là các cạnh hình chữ nhật. 

### Hình chữ nhật không diện tích 

đầu vào:```
0 0 0 0
0 1 0 1
0 0 0 1
0 0 0 1
```Sau khi bình thường hóa: 

- cả hai đoạn thẳng đứng đều nằm trên$x = 0$, 
- các đoạn ngang thực chất là các điểm. 

Chiều rộng tính toán là:$$0 - 0 = 0$$Việc kiểm tra vùng tích cực không thành công nên câu trả lời là`NO`. 

### Thứ tự điểm cuối bị đảo ngược 

đầu vào:```
3 0 0 0
0 2 3 2
0 0 0 2
3 2 3 0
```Chuẩn hóa chuyển đổi các phân đoạn thành thứ tự nhất quán: 

-`(0,0)-(3,0)`-`(0,2)-(3,2)`-`(0,0)-(0,2)`-`(3,0)-(3,2)`Tất cả các kiểm tra hình chữ nhật đều vượt qua, do đó thuật toán xuất ra chính xác`YES`. 

Nếu không chuẩn hóa, việc so sánh tọa độ trực tiếp sẽ thất bại ngay cả khi hình học đúng. 

### Các mặt trùng lặp 

đầu vào:```
0 0 2 0
0 0 2 0
0 0 0 1
2 0 2 1
```Cả hai đoạn ngang đều chiếm cùng một vị trí. Giá trị y của chúng giống hệt nhau nên không có sự khác biệt về mặt trên và mặt dưới. 

Sau đó, thuật toán so sánh các giá trị y được thu thập từ các phân đoạn ngang với phạm vi y từ các phân đoạn dọc: 

- giá trị y ngang:`[0, 0]`- phạm vi y dọc:`[0, 1]`Sự không phù hợp tạo ra`NO`. 

Điều này nắm bắt các phân đoạn chồng chéo không thực sự bao quanh một khu vực.
