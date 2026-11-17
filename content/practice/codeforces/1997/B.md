---
title: "CF 1997B - Làm Ba Miền"
description: "Chúng tôi có một lưới chỉ có hai hàng. Một số ô trống (.), một số bị chặn (x). Các ô tự do tạo thành một biểu đồ trong đó các ô liền kề chia sẻ một bên được kết nối. Vùng được kết nối chỉ đơn giản là một thành phần được kết nối của biểu đồ này."
date: "2026-06-08T14:35:25+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1997
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 168 (Rated for Div. 2)"
rating: 1100
weight: 1997
solve_time_s: 168
verified: true
draft: false
---

[CF 1997B - Tạo ba vùng](https://codeforces.com/problemset/problem/1997/B) 

**Đánh giá:** 1100 
**Thẻ:** thuật toán xây dựng, hai con trỏ 
**Thời gian giải:** 2m 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lưới chỉ có hai hàng. Một số ô miễn phí (`.`), một số bị chặn (`x`). 

Các ô tự do tạo thành một biểu đồ trong đó các ô liền kề chia sẻ một bên được kết nối. Vùng được kết nối chỉ đơn giản là một thành phần được kết nối của biểu đồ này. 

Đầu vào đảm bảo rằng trước khi chúng ta làm bất cứ điều gì, lưới chỉ chứa tối đa một vùng được kết nối. Nói cách khác, tất cả các ô trống đều đã được kết nối hoặc không có ô trống nào cả. 

Đối với mỗi ô trống, chúng tôi tưởng tượng việc chặn nó và hỏi còn lại bao nhiêu vùng được kết nối. Chúng ta phải đếm các ô mà việc loại bỏ chúng tạo ra chính xác ba vùng được kết nối. 

Chiều rộng của lưới có thể đạt tới$2 \cdot 10^5$và tổng của tất cả các trường hợp thử nghiệm cũng là$2 \cdot 10^5$. Bất kỳ giải pháp nào tính toán lại các thành phần được kết nối cho mọi ô ứng viên đều quá đắt. Thậm chí một$O(n^2)$thuật toán sẽ thực hiện hàng chục tỷ phép tính trong trường hợp xấu nhất. 

Những sai lầm nguy hiểm nhất đến từ việc cho rằng mọi luận điểm đều có giá trị. 

Coi như:```
...
...
```Việc xóa ô trung tâm của hàng trên cùng không tạo ra ba vùng. Hàng dưới cùng vẫn nối phần bên trái và bên phải nên đồ thị vẫn được kết nối. 

Một trường hợp tế nhị khác là:```
top:    ...
bottom: x.x
```Việc xóa ô giữa của hàng trên cùng sẽ tạo ra chính xác ba vùng:```
top:    .x.
bottom: x.x
```Ô trên cùng bên trái, ô trên cùng bên phải và ô giữa dưới cùng trở thành ba thành phần riêng biệt. 

Một giải pháp bất cẩn chỉ kiểm tra mức độ của ô bị loại bỏ sẽ bỏ sót lý do tại sao mẫu này hoạt động và tại sao các mẫu trông giống nhau thì không. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi ô trống, hãy tạm thời chặn ô đó, chạy DFS hoặc BFS trên lưới còn lại, đếm các thành phần được kết nối và kiểm tra xem câu trả lời có phải là ba hay không. 

Điều này đúng vì nó mô phỏng trực tiếp định nghĩa của vấn đề. 

Vấn đề là thời gian chạy. có$O(n)$các ô trống và mỗi lần kiểm tra kết nối đều có giá$O(n)$. Tổng số trở thành$O(n^2)$, nó quá lớn khi$n$có thể$2 \cdot 10^5$. 

Quan sát quan trọng xuất phát từ thực tế là lưới chỉ có hai hàng. 

Giả sử việc loại bỏ một ô sẽ tạo ra chính xác ba thành phần được kết nối. Vì một đỉnh có thể chia một đồ thị thành nhiều phần bằng số bậc của nó nên ô bị loại bỏ phải có bậc ba. 

Trong lưới 2 hàng, chỉ có thể đạt được bậc 3 khi:```
same row:      . . .
other row:       .
```Ô có hàng xóm bên trái, hàng xóm bên phải và hàng xóm dọc. 

Bây giờ hãy hỏi khi nào ba hàng xóm đó trở thành ba thành phần khác nhau sau khi loại bỏ. 

Hàng xóm thẳng đứng không được phép tiếp cận hai bên. Điều đó có nghĩa là cả hai ô liền kề trong hàng của nó đều phải bị chặn:```
same row:      . . .
other row:    x . x
```Mẫu này là đủ. Sau khi xóa ô trung tâm của hàng đầu tiên:```
same row:      . x .
other row:    x . x
```bên trái, bên phải và ô dọc trở thành ba vùng riêng biệt. 

Bởi vì lưới ban đầu chứa nhiều nhất một thành phần được kết nối nên không có tuyến đường thay thế nào xung quanh cấu trúc cục bộ này. Phần bên trái và bên phải không thể kết nối lại ở nơi khác. 

Vì vậy, mọi ô hợp lệ chính xác là trung tâm của một trong các mẫu này và mọi mẫu như vậy đều hợp lệ. 

Chúng ta chỉ cần quét các cột và đếm chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi cột$i$với$1 \le i \le n-2$, hãy coi nó như một cột trung tâm có thể. 
2. Kiểm tra xem ô trên cùng ở cột có$i$là tế bào có thể tháo rời. 

Chúng tôi cần:```
top:    . . .
bottom: x . x
```tập trung vào cột$i$. 

Cụ thể:```
top[i-1] == '.'
top[i]   == '.'
top[i+1] == '.'

bottom[i-1] == 'x'
bottom[i]   == '.'
bottom[i+1] == 'x'
```Nếu mẫu này xuất hiện, hãy tăng câu trả lời. 
3. Thực hiện kiểm tra đối xứng với các hàng được hoán đổi. 

Bây giờ ô có thể tháo rời nằm ở hàng dưới cùng:```
bottom: . . .
top:    x . x
```4. Sau khi quét tất cả các cột, xuất ra số đếm. 

### Tại sao nó hoạt động 

Một ô chỉ có thể tạo ba thành phần được kết nối nếu nó có ba ô lân cận. Trong lưới 2 hàng, điều đó buộc hình dạng cục bộ```
. . .
  .
```xung quanh ô bị loại bỏ. 

Để ba thành phần kết quả được phân biệt, hàng xóm dọc phải được cách ly khỏi cả hai nhánh trái và phải. Cách duy nhất điều này xảy ra là khi các ô bên cạnh hàng xóm dọc bị chặn, tạo ra```
. . .
x . x
```hoặc phiên bản được nhân đôi của nó. 

Mỗi mẫu được đếm thực sự tạo ra chính xác ba thành phần sau khi loại bỏ ô trung tâm. Ngược lại, bất kỳ ô nào tạo ra chính xác ba thành phần đều phải xuất hiện theo một trong các mẫu này. Quá trình quét đếm tất cả và chỉ các ô hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s0 = input().strip()
        s1 = input().strip()

        ans = 0

        for i in range(1, n - 1):
            if (
                s0[i - 1] == '.' and
                s0[i] == '.' and
                s0[i + 1] == '.' and
                s1[i - 1] == 'x' and
                s1[i] == '.' and
                s1[i + 1] == 'x'
            ):
                ans += 1

            if (
                s1[i - 1] == '.' and
                s1[i] == '.' and
                s1[i + 1] == '.' and
                s0[i - 1] == 'x' and
                s0[i] == '.' and
                s0[i + 1] == 'x'
            ):
                ans += 1

        print(ans)

solve()
```Vòng lặp bắt đầu ở cột`1`và kết thúc ở cột`n - 2`bởi vì mọi mẫu hợp lệ đều cần cả hàng xóm bên trái và hàng xóm bên phải. 

Mỗi lần lặp thực hiện hai lần kiểm tra. Đầu tiên giả định ô có thể tháo rời nằm ở hàng trên cùng. Cái thứ hai giả định nó ở hàng dưới cùng. 

Không cần xây dựng biểu đồ, DFS hoặc đếm thành phần. Toàn bộ giải pháp được điều khiển bởi đặc tính cấu trúc của ô khớp nối hợp lệ trong lưới 2 hàng. 

Việc triển khai chỉ sử dụng bộ nhớ bổ sung không đổi và xử lý mỗi cột một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
.......x
.x.xx...
```Quá trình quét sẽ kiểm tra các cột từ 2 đến 7 (việc lập chỉ mục dựa trên 1 được bỏ qua ở đây để rõ ràng). 

| Cột Trung Tâm | Mẫu hàng đầu`...`| Mẫu dưới cùng`x.x`| Đã tính | 
| --- | --- | --- | --- | 
| 1 | Không | Không | Không | 
| 2 | Có | Không | Không | 
| 3 | Có | Có | Có | 
| 4 | Có | Không | Không | 
| 5 | Không | Không | Không | 
| 6 | Không | Không | Không | 

Câu trả lời cuối cùng:```
1
```Cột thú vị là cột thứ ba. Loại bỏ ô trên cùng đó sẽ cô lập nhánh trái, nhánh phải và ô dưới cùng. 

### Ví dụ 2 

đầu vào:```
9
..x.x.x.x
x.......x
```| Cột Trung Tâm | Mẫu dưới cùng`...`| Mẫu hàng đầu`x.x`| Đã tính | 
| --- | --- | --- | --- | 
| 1 | Không | Không | Không | 
| 2 | Có | Không | Không | 
| 3 | Có | Có | Có | 
| 4 | Có | Không | Không | 
| 5 | Có | Có | Có | 
| 6 | Có | Không | Không | 
| 7 | Không | Không | Không | 

Câu trả lời cuối cùng:```
2
```Hai ô có thể tháo rời hợp lệ là các ô ở hàng dưới cùng có các cột nằm bên dưới mẫu`x.x`ở hàng trên cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần qua các cột | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Vì tổng của tất cả`n`giá trị nhiều nhất là`2 · 10^5`, quét tuyến tính qua từng trường hợp thử nghiệm dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    input = sys.stdin.readline

    t = int(input())

    for _ in range(t):
        n = int(input())
        s0 = input().strip()
        s1 = input().strip()

        ans = 0

        for i in range(1, n - 1):
            if (
                s0[i - 1] == '.' and
                s0[i] == '.' and
                s0[i + 1] == '.' and
                s1[i - 1] == 'x' and
                s1[i] == '.' and
                s1[i + 1] == 'x'
            ):
                ans += 1

            if (
                s1[i - 1] == '.' and
                s1[i] == '.' and
                s1[i + 1] == '.' and
                s0[i - 1] == 'x' and
                s0[i] == '.' and
                s0[i + 1] == 'x'
            ):
                ans += 1

        print(ans, file=out)

    return out.getvalue()

# provided sample
assert run(
"""4
8
.......x
.x.xx...
2
..
..
3
xxx
xxx
9
..x.x.x.x
x.......x
"""
) == "1\n0\n0\n2\n"

# minimum size
assert run(
"""1
1
.
.
"""
) == "0\n"

# single valid pattern
assert run(
"""1
3
...
x.x
"""
) == "1\n"

# mirrored valid pattern
assert run(
"""1
3
x.x
...
"""
) == "1\n"

# no valid articulation
assert run(
"""1
5
.....
.....
"""
) == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`, cả hai đều miễn phí |`0`| Không có cột trung tâm tồn tại | 
|`... / x.x`|`1`| Mẫu hợp lệ nhỏ nhất | 
|`x.x / ...`|`1`| Trường hợp đối xứng | 
| Hai hàng chấm đầy đủ |`0`| Trái và phải vẫn được kết nối | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
..
..
```Không có cột nào có cả hàng xóm bên trái và bên phải. Phạm vi quét trống nên câu trả lời là`0`. Trực giác về điểm khớp nối mạnh mẽ có thể nhìn không chính xác vào một trong các ô và mong đợi sự phân chia, nhưng mức độ ba là không thể trong một lưới chiều rộng hai chiều dài hai. 

Coi như:```
1
3
...
...
```Ô trên cùng ở giữa có độ ba, nhưng mẫu không hợp lệ. Hàng dưới cùng cung cấp tuyến đường nối bên trái và bên phải sau khi gỡ bỏ. Séc của chúng tôi từ chối nó vì hàng đối diện là`...`, không`x.x`. 

Coi như:```
1
3
...
x.x
```Ô trên cùng ở giữa khớp chính xác với mẫu được yêu cầu. Thuật toán đếm nó. Sau khi loại bỏ:```
.x.
x.x
```ba ô tự do còn lại được ngắt kết nối theo cặp, tạo ra chính xác ba vùng được kết nối. 

Những ví dụ này đề cập đến những cạm bẫy chính và chỉ ra lý do tại sao việc mô tả đặc tính mẫu cục bộ vừa cần thiết vừa đủ.
