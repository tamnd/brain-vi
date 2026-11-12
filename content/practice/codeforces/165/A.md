---
title: "CF 165A - Điểm siêu trung tâm"
description: "Chúng ta được cấp một tập hợp các điểm trên mặt phẳng Descartes 2D. Một điểm được gọi là siêu tâm nếu tồn tại ít nhất một điểm khác ngay bên trái của nó, một điểm ngay bên phải nó, một điểm ngay phía trên nó và một điểm ngay bên dưới nó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 165
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 112 (Div. 2)"
rating: 1000
weight: 165
solve_time_s: 112
verified: false
draft: false
---

[CF 165A - Điểm siêu trung tâm](https://codeforces.com/problemset/problem/165/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên mặt phẳng Descartes 2D. Một điểm được gọi là siêu tâm nếu tồn tại ít nhất một điểm khác ngay bên trái của nó, một điểm ngay bên phải nó, một điểm ngay phía trên nó và một điểm ngay bên dưới nó. 

Chi tiết quan trọng là những người hàng xóm này phải chia sẻ cùng tọa độ x hoặc cùng tọa độ y. Một điểm ở bên trái hoặc bên phải phải nằm trên cùng một đường ngang, trong khi một điểm ở trên hoặc dưới phải nằm trên cùng một đường thẳng đứng. 

Đối với mỗi điểm, chúng ta phải kiểm tra xem tất cả bốn hướng có được biểu diễn ở đâu đó trong tập hợp hay không. Câu trả lời chỉ đơn giản là số điểm thỏa mãn điều kiện này. 

Những hạn chế là rất nhỏ. Số điểm nhiều nhất là 200, có nghĩa là ngay cả một thuật toán có khoảng 40.000 cặp so sánh cũng hoàn toàn an toàn. Một giải pháp bậc hai đủ nhanh trong giới hạn 2 giây. 

Mối nguy hiểm chính trong vấn đề này là hiểu nhầm thế nào là hàng xóm. Điểm không cần phải là điểm gần nhất theo hướng đó. Bất kỳ điểm nào thỏa mãn điều kiện định hướng là đủ. 

Hãy xem xét ví dụ này:```
5
0 0
2 0
5 0
0 2
0 -2
```Điểm`(0,0)`có một người hàng xóm phù hợp vì cả hai`(2,0)`Và`(5,0)`nằm ở bên phải của nó trên cùng một đường ngang. Chúng tôi chỉ quan tâm rằng ít nhất một tồn tại. 

Một lỗi phổ biến khác là quên rằng cả bốn hướng đều được yêu cầu đồng thời. 

Ví dụ:```
4
0 0
1 0
-1 0
0 1
```Câu trả lời đúng là:```
0
```Điểm`(0,0)`có hàng xóm bên trái, bên phải và phía trên nhưng không có hàng xóm phía dưới nên không phải là siêu trung tâm. 

Việc thực hiện bất cẩn cũng có thể vô tình đếm được các điểm chéo. 

Ví dụ:```
5
0 0
1 1
-1 1
1 -1
-1 -1
```Câu trả lời đúng là:```
0
```Không có điểm nào trong số này có cùng tọa độ x hoặc tọa độ y với`(0,0)`, vì vậy không ai trong số họ đủ tiêu chuẩn là hàng xóm định hướng. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xử lý từng điểm một cách độc lập và tìm kiếm trong tất cả các điểm khác để xem liệu bốn hướng yêu cầu có tồn tại hay không. 

Giả sử chúng ta hiện đang kiểm tra điểm`(x,y)`. Chúng tôi lặp lại mọi điểm khác`(nx,ny)`và kiểm tra bốn điều kiện: 

-`nx < x and ny == y`có nghĩa là chúng ta đã tìm thấy một người hàng xóm bên trái. 
-`nx > x and ny == y`nghĩa là chúng ta đã tìm được người hàng xóm phù hợp. 
-`nx == x and ny < y`có nghĩa là chúng tôi đã tìm thấy một người hàng xóm thấp hơn. 
-`nx == x and ny > y`có nghĩa là chúng tôi đã tìm thấy một người hàng xóm phía trên. 

Một khi tất cả bốn điều kiện đều đúng thì điểm đó là siêu trung tâm. 

Chiến lược bạo lực này đã đủ nhanh vì`n ≤ 200`. Trong trường hợp xấu nhất, chúng ta so sánh từng cặp điểm một lần, đưa ra kết quả`200 × 200 = 40,000`so sánh. Đó là nhỏ bé. 

Không cần cấu trúc dữ liệu nâng cao hoặc các thủ thuật hình học vì các ràng buộc rất nhỏ. Vấn đề chủ yếu là về việc thực hiện kiểm tra định hướng một cách chính xác. 

Quan sát hữu ích là tính chất siêu trung tâm chỉ phụ thuộc vào sự tồn tại của các điểm trong bốn hướng. Chúng tôi không quan tâm đến khoảng cách hoặc hàng xóm gần nhất. Điều đó có nghĩa là chỉ cần quét đơn giản qua tất cả các điểm là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(1) | Đã chấp nhận | 

Trong bài toán này, cách tiếp cận vũ phu đã đủ tối ưu cho các giới hạn đã cho. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm vào một danh sách. 
2. Khởi tạo bộ đếm`answer = 0`. 
3. Đối với mỗi điểm`(x,y)`trong danh sách, tạo bốn biến boolean:`left`,`right`,`up`, Và`down`. 
4. Quét qua mọi điểm khác`(nx,ny)`. 
5. Nếu`ny == y`Và`nx < x`, đánh dấu`left = True`. 
6. Nếu`ny == y`Và`nx > x`, đánh dấu`right = True`. 
7. Nếu`nx == x`Và`ny < y`, đánh dấu`down = True`. 
8. Nếu`nx == x`Và`ny > y`, đánh dấu`up = True`. 
9. Sau khi kiểm tra tất cả các điểm, nếu cả bốn giá trị boolean đều đúng, hãy tăng câu trả lời. 
10. In câu trả lời cuối cùng. 

Tại sao nó hoạt động: 

Đối với mỗi điểm, thuật toán kiểm tra rõ ràng xem có tồn tại ít nhất một điểm theo mỗi hướng được yêu cầu hay không. Các điều kiện khớp hoàn toàn với định nghĩa của các điểm siêu trung tâm. Vì mọi điểm khác đều được kiểm tra nên không có điểm lân cận hợp lệ nào có thể bị bỏ sót. Một điểm chỉ được tính khi cả bốn yêu cầu định hướng đều được đáp ứng đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution

def solve():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    answer = 0

    for x, y in points:
        left = False
        right = False
        up = False
        down = False

        for nx, ny in points:
            if ny == y:
                if nx < x:
                    left = True
                elif nx > x:
                    right = True

            if nx == x:
                if ny < y:
                    down = True
                elif ny > y:
                    up = True

        if left and right and up and down:
            answer += 1

    print(answer)

solve()
```Giải pháp tuân theo thuật toán trực tiếp. 

Vòng lặp bên ngoài chọn điểm hiện tại mà chúng tôi muốn kiểm tra. Vòng lặp bên trong tìm kiếm hàng xóm theo bốn hướng. 

Việc triển khai tách biệt các kiểm tra theo chiều ngang và chiều dọc vì các định nghĩa khác nhau. Hàng xóm ngang yêu cầu bình đẳng`y`các giá trị, trong khi các hàng xóm theo chiều dọc yêu cầu bằng nhau`x`các giá trị. 

Sử dụng cờ boolean là cách tiếp cận rõ ràng nhất vì chúng tôi chỉ quan tâm liệu hàng xóm có tồn tại hay không chứ không phải có bao nhiêu tồn tại. 

các`elif`các phát biểu tránh những kiểm tra không cần thiết một khi chúng ta đã biết liệu một điểm có nằm về một phía hay không. Vì tất cả các tọa độ đều khác nhau nên một điểm không thể đồng thời ở bên trái và bên phải của điểm hiện tại. 

Không có lo ngại về tràn vì tọa độ là số nguyên nhỏ. Phần tinh tế duy nhất là hãy nhớ rằng các điểm chéo không được tính trừ khi một tọa độ khớp chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8
1 1
4 2
3 1
1 2
0 2
0 1
1 0
1 3
```### Theo dõi điểm`(1,1)`| Điểm so sánh | Trái | Đúng | Lên | Xuống | 
| --- | --- | --- | --- | --- | 
| (4,2) | Sai | Sai | Sai | Sai | 
| (3,1) | Sai | Đúng | Sai | Sai | 
| (1,2) | Sai | Đúng | Đúng | Sai | 
| (0,2) | Sai | Đúng | Đúng | Sai | 
| (0,1) | Đúng | Đúng | Đúng | Sai | 
| (1,0) | Đúng | Đúng | Đúng | Đúng | 
| (1,3) | Đúng | Đúng | Đúng | Đúng | 

Tất cả bốn phương đều tồn tại, vì vậy`(1,1)`là siêu trung tâm. 

### Theo dõi điểm`(1,2)`| Điểm so sánh | Trái | Đúng | Lên | Xuống | 
| --- | --- | --- | --- | --- | 
| (1,1) | Sai | Sai | Sai | Đúng | 
| (4,2) | Sai | Đúng | Sai | Đúng | 
| (3,1) | Sai | Đúng | Sai | Đúng | 
| (0,2) | Đúng | Đúng | Sai | Đúng | 
| (0,1) | Đúng | Đúng | Sai | Đúng | 
| (1,0) | Đúng | Đúng | Sai | Đúng | 
| (1,3) | Đúng | Đúng | Đúng | Đúng | 

Điểm này cũng là siêu trung tâm. 

Câu trả lời cuối cùng là:```
2
```Những dấu vết này cho thấy hàng xóm không cần phải ở gần nhau. Bất kỳ điểm hợp lệ nào theo hướng yêu cầu là đủ. 

### Ví dụ thứ hai 

đầu vào:```
5
0 0
1 0
-1 0
0 1
0 -1
```### Theo dõi điểm`(0,0)`| Điểm so sánh | Trái | Đúng | Lên | Xuống | 
| --- | --- | --- | --- | --- | 
| (1,0) | Sai | Đúng | Sai | Sai | 
| (-1,0) | Đúng | Đúng | Sai | Sai | 
| (0,1) | Đúng | Đúng | Đúng | Sai | 
| (0,-1) | Đúng | Đúng | Đúng | Đúng | 

Điểm thỏa mãn cả 4 điều kiện nên đáp án là:```
1
```Ví dụ này thể hiện cấu hình siêu trung tâm đơn giản nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mọi điểm đều được so sánh với mọi điểm khác | 
| Không gian | O(1) | Chỉ một số biến boolean được sử dụng ngoài danh sách đầu vào | 

Với tối đa 200 điểm, phép quét bậc hai chỉ thực hiện khoảng 40.000 so sánh. Điều này dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    answer = 0

    for x, y in points:
        left = right = up = down = False

        for nx, ny in points:
            if ny == y:
                if nx < x:
                    left = True
                elif nx > x:
                    right = True

            if nx == x:
                if ny < y:
                    down = True
                elif ny > y:
                    up = True

        if left and right and up and down:
            answer += 1

    print(answer)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run(
"""8
1 1
4 2
3 1
1 2
0 2
0 1
1 0
1 3
"""
) == "2", "sample 1"

# minimum size
assert run(
"""1
0 0
"""
) == "0", "single point cannot be supercentral"

# simple cross
assert run(
"""5
0 0
1 0
-1 0
0 1
0 -1
"""
) == "1", "center point is supercentral"

# diagonal points only
assert run(
"""5
0 0
1 1
-1 1
1 -1
-1 -1
"""
) == "0", "diagonal points do not count"

# multiple horizontal points but missing vertical
assert run(
"""4
0 0
1 0
2 0
-1 0
"""
) == "0", "missing upper and lower neighbors"

print("All tests passed!")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm đơn | 0 | Đầu vào kích thước tối thiểu | 
| Tập trung chéo tại điểm gốc | 1 | Điểm siêu trung tâm hợp lệ cơ bản | 
| Cấu hình chỉ theo đường chéo | 0 | Hàng xóm chéo không được tính | 
| Chỉ đường ngang | 0 | Tất cả bốn hướng đều được yêu cầu | 

## Vỏ cạnh 

Hãy xem xét một điểm có lân cận chỉ theo ba hướng. 

đầu vào:```
4
0 0
1 0
-1 0
0 1
```Trong quá trình quét tìm`(0,0)`, bộ thuật toán: 

-`left = True`-`right = True`-`up = True`Nhưng`down`vẫn sai vì không có điểm nào có cùng tọa độ x và tọa độ y nhỏ hơn. Vì tất cả bốn boolean đều được yêu cầu nên điểm không được tính. Câu trả lời cuối cùng là`0`. 

Bây giờ hãy xem xét các điểm chéo. 

đầu vào:```
5
0 0
1 1
-1 1
1 -1
-1 -1
```Khi kiểm tra`(0,0)`, mọi điểm khác đều thất bại trong cả hai lần kiểm tra đẳng thức: 

-`ny == y`không bao giờ là sự thật. 
-`nx == x`không bao giờ là sự thật. 

Tất cả bốn cờ định hướng vẫn sai, do đó thuật toán xuất ra chính xác`0`. 

Cuối cùng, hãy xem xét nhiều hàng xóm ở cùng một phía. 

đầu vào:```
6
0 0
1 0
2 0
3 0
0 1
0 -1
```Dấu hiệu thuật toán`right = True`ngay khi nó gặp`(1,0)`. Các điểm bổ sung`(2,0)`Và`(3,0)`không thay đổi bất cứ điều gì. Vì vẫn không còn hàng xóm bên trái nên điểm không được tính. Câu trả lời cuối cùng là`0`. 

Điều này xác nhận rằng thuật toán chỉ kiểm tra sự tồn tại, khớp chính xác với định nghĩa vấn đề.
