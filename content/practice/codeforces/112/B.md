---
title: "CF 112B - Petya và Square"
description: "Ta có một bảng hình vuông có kích thước 2n × 2n, được chia thành các ô đơn vị. Một ô được đánh dấu. Chúng tôi muốn biết liệu có thể vẽ đường cắt dọc theo các đường lưới để bảng được chia thành hai phần bằng nhau sau khi xoay hay không, trong khi đường cắt không bao giờ chạm vào điểm được đánh dấu…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 112
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 85 (Div. 2 Only)"
rating: 1200
weight: 112
solve_time_s: 102
verified: true
draft: false
---

[CF 112B - Petya và Square](https://codeforces.com/problemset/problem/112/B) 

**Đánh giá:** 1200 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng hình vuông có kích thước`2n × 2n`, được chia thành các ô đơn vị. Một ô được đánh dấu. Chúng tôi muốn biết liệu có thể vẽ đường cắt dọc theo các đường lưới để bảng được chia thành hai phần bằng nhau sau khi xoay, trong khi đường cắt không bao giờ chạm vào ô được đánh dấu hay không. 

Chi tiết quan trọng là hai mảnh kết quả phải trở nên giống hệt nhau sau khi xoay. Đối với hình vuông có cạnh chẵn, đối xứng quay tự nhiên nằm ở giữa bảng. 

Đầu vào cho biết độ dài cạnh`2n`và tọa độ`(x, y)`của ô được đánh dấu. Hàng và cột được lập chỉ mục 1. Chúng ta phải in`"YES"`nếu vết cắt như vậy tồn tại, nếu không`"NO"`. 

Các ràng buộc rất nhỏ, độ dài cạnh tối đa là 100, vì vậy hiệu suất không phải là thách thức ở đây. Khó khăn là nhận ra quan sát hình học ẩn chứa trong câu phát biểu. Việc xây dựng hình học mạnh mẽ sẽ không cần thiết và dễ xảy ra lỗi. 

Phần không rõ ràng là hiểu được ô nào không thể tránh khỏi đối với mỗi lần cắt đối xứng hợp lệ. 

Hãy xem xét trường hợp thú vị nhỏ nhất:```
4 2 2
```Hình vuông là`4 × 4`. Khu vực trung tâm bao gồm bốn ô:```
(2,2) (2,3)
(3,2) (3,3)
```Nếu ô được đánh dấu là một trong bốn ô này thì câu trả lời là`"NO"`. 

Tại sao? Bất kỳ đường cắt hợp lệ nào chia hình vuông thành hai nửa xoay tương đương đều phải đi qua tâm chính xác của bảng. Trong một lưới có kích thước chẵn, tâm nằm ở giao điểm của bốn ô đó, do đó mọi đường phân chia đối xứng có thể đều chạm vào tâm đó.`2 × 2`khối. 

Một giải pháp bất cẩn có thể chỉ kiểm tra điểm trung tâm chính xác và quên rằng một lưới có kích thước chẵn có bốn ô trung tâm chứ không phải một. 

Một sai lầm dễ mắc phải khác là sử dụng lý luận dựa trên số 0 với tọa độ dựa trên một. Đối với một`6 × 6`bảng, các tế bào trung tâm là:```
(3,3) (3,4)
(4,3) (4,4)
```không`(2,2)`Và`(3,3)`. 

Ví dụ:```
6 3 4
```phải sản xuất`"NO"`bởi vì`(3,4)`nằm bên trong khối trung tâm. 

Nhưng:```
6 2 4
```sản xuất`"YES"`vì ô được đánh dấu nằm ngoài vùng không thể tránh khỏi đó. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng xây dựng rõ ràng mọi đường cắt có thể dọc theo các cạnh lưới, xác minh rằng nó chia bảng thành các vùng đồng dạng xoay bằng nhau và kiểm tra xem đường dẫn có chạm vào ô được đánh dấu hay không. 

Ngay cả đối với một`100 × 100`bảng, số lượng các đường dẫn mạng có thể là rất lớn. Không gian tìm kiếm tăng theo cấp số nhân vì tại mỗi giao điểm lưới, đường dẫn có thể phân nhánh theo nhiều hướng. Cách tiếp cận này trở nên không khả thi gần như ngay lập tức. 

Lý do khiến cho cảm giác mạnh mẽ trở nên tự nhiên là vì phát biểu nói về các đường cắt hình học hơn là các điều kiện số học đơn giản. Nhưng hình học ở đây cực kỳ cứng nhắc. 

Quan sát quan trọng là mọi phân vùng hợp lệ phải tôn trọng sự đối xứng quay 180 độ quanh tâm hình vuông. Nếu chiều dài cạnh chẵn thì tâm nằm giữa bốn ô. Mọi đường phân chia đối xứng đều phải đi qua vùng trung tâm đó. 

Điều đó có nghĩa là các ô duy nhất không bao giờ có thể tránh được là bốn ô ở giữa:```
(n,n)
(n,n+1)
(n+1,n)
(n+1,n+1)
```Nếu ô được đánh dấu là một trong số chúng thì mọi đường cắt hợp lệ sẽ cắt nó, vì vậy câu trả lời là`"NO"`. 

Đối với mỗi ô khác, có thể vẽ một đường cắt đối xứng phù hợp để tránh vị trí đã đánh dấu, vì vậy câu trả lời là`"YES"`. 

Toàn bộ vấn đề giảm xuống việc kiểm tra xem`(x, y)`thuộc về trung tâm`2 × 2`khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài cạnh`2n`và tọa độ ô`(x, y)`. 
2. Tính toán`n = side_length // 2`. 

Bốn ô trung tâm không thể tránh khỏi được xác định hoàn toàn bởi giá trị này. 
3. Kiểm tra xem ô được đánh dấu có nằm bên trong ô trung tâm không`2 × 2`khối. 

Điều này xảy ra chính xác khi:```
x is either n or n+1
and
y is either n or n+1
```4. Nếu cả hai điều kiện đều đúng, hãy in`"NO"`. 

Mọi vết cắt đối xứng đều phải đi qua vùng này. 
5. Nếu không, hãy in`"YES"`. 

Bên ngoài khối trung tâm tồn tại một phân vùng đối xứng hợp lệ tránh ô được đánh dấu. 

### Tại sao nó hoạt động 

Một vết cắt hợp lệ sẽ chia hình vuông thành hai phần giống hệt nhau sau khi xoay. Sự đối xứng như vậy buộc đường phân chia đi qua tâm hình học của hình vuông. 

Vì chiều dài các cạnh đều nhau nên tâm hình học nằm giữa bốn ô. Bất kỳ đường đi nào qua trung tâm nhất thiết phải chạm vào trung tâm`2 × 2`khối. Bốn tế bào đó là không thể tránh khỏi. 

Mọi ô khác có thể tránh được bằng cách chọn một đường cắt đối xứng thích hợp. Vậy câu trả lời là`"NO"`chính xác cho bốn tế bào trung tâm, và`"YES"`mọi nơi khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

side, x, y = map(int, input().split())

n = side // 2

if (x == n or x == n + 1) and (y == n or y == n + 1):
    print("NO")
else:
    print("YES")
```Việc thực hiện tuân theo quan sát toán học trực tiếp. 

Đầu tiên chúng ta tính toán`n`, xác định phần giữa của bảng. Vì chiều dài cạnh luôn bằng nhau nên tâm bao gồm bốn ô chứ không phải một ô. 

Điều kiện:```
(x == n or x == n + 1)
```kiểm tra xem hàng có thuộc cặp hàng trung tâm hay không. Logic tương tự áp dụng cho các cột. 

Cả hai điều kiện phải được giữ đồng thời để ô được đánh dấu nằm trong ô trung tâm`2 × 2`khối. 

Một lỗi thường gặp là quên rằng lưới sử dụng lập chỉ mục dựa trên một. Đối với một`4 × 4`bảng, các hàng trung tâm là`2`Và`3`, không`1`Và`2`. 

Giải pháp sử dụng thời gian không đổi và bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1 1
```| Biến | Giá trị | 
| --- | --- | 
| bên | 4 | 
| n | 2 | 
| x | 1 | 
| y | 1 | 

Tế bào trung tâm là:```
(2,2) (2,3)
(3,2) (3,3)
```Ô được đánh dấu`(1,1)`nằm ngoài khu vực này nên câu trả lời là:```
YES
```Ví dụ này chứng minh rằng hầu hết tất cả các ô đều hợp lệ. Chỉ có khối trung tâm bị cấm. 

### Ví dụ 2 

đầu vào:```
6 3 4
```| Biến | Giá trị | 
| --- | --- | 
| bên | 6 | 
| n | 3 | 
| x | 3 | 
| y | 4 | 

Tế bào trung tâm là:```
(3,3) (3,4)
(4,3) (4,4)
```Ô được đánh dấu`(3,4)`thuộc khối trung tâm nên đáp án là:```
NO
```Dấu vết này xác nhận ranh giới chính xác của vùng cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học và so sánh | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Các ràng buộc là cực kỳ nhỏ, nhưng dù sao thì giải pháp này vẫn tối ưu. Nó thực hiện khối lượng công việc không đổi bất kể kích thước bảng và dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    side, x, y = map(int, input().split())

    n = side // 2

    if (x == n or x == n + 1) and (y == n or y == n + 1):
        print("NO")
    else:
        print("YES")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run("4 1 1\n") == "YES", "sample 1"

# minimum valid size, central cell
assert run("2 1 1\n") == "NO", "minimum size central cell"

# minimum valid size, another central cell
assert run("2 2 2\n") == "NO", "all cells are central in 2x2"

# outside center block
assert run("6 2 4\n") == "YES", "outside forbidden region"

# exact center boundary
assert run("6 3 4\n") == "NO", "central block detection"

# maximum size edge position
assert run("100 1 100\n") == "YES", "large board edge cell"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`|`NO`| Bảng nhỏ nhất, mọi ô đều là trung tâm | 
|`2 2 2`|`NO`| Xác nhận tất cả bốn ô đều bị cấm trong`2 × 2`| 
|`6 2 4`|`YES`| Ô liền kề ở giữa nhưng vẫn hợp lệ | 
|`6 3 4`|`NO`| Nhận dạng đúng khối trung tâm | 
|`100 1 100`|`YES`| Tọa độ ranh giới lớn | 

## Vỏ cạnh 

Hãy xem xét bảng nhỏ nhất có thể:```
2 1 1
```Đây`n = 1`, do đó khối trung tâm là:```
(1,1) (1,2)
(2,1) (2,2)
```Ô được đánh dấu thuộc khối này nên thuật toán sẽ in`"NO"`. 

Trường hợp này rất dễ bị xử lý sai nếu việc triển khai giả định chỉ có một ô trung tâm. 

Bây giờ hãy xem xét:```
6 2 3
```Chúng tôi tính toán:```
n = 3
```Các ô bị cấm là:```
(3,3) (3,4)
(4,3) (4,4)
```Ô được đánh dấu`(2,3)`nằm ngay phía trên khối trung tâm nhưng không nằm bên trong nó. Thuật toán in chính xác`"YES"`. 

Điều này phát hiện từng lỗi một trong đó các lập trình viên vô tình mở rộng vùng cấm ra ngoài phạm vi thực tế.`2 × 2`trung tâm. 

Cuối cùng, hãy xem xét:```
100 50 51
```Đây:```
n = 50
```Các tế bào trung tâm là:```
(50,50) (50,51)
(51,50) (51,51)
```Ô được đánh dấu`(50,51)`nằm bên trong khối, do đó thuật toán in`"NO"`. 

Điều này xác minh rằng logic chia tỷ lệ chính xác đến kích thước đầu vào lớn nhất mà không cần bất kỳ xử lý đặc biệt nào.
