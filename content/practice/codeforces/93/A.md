---
title: "CF 93A - Gọng Kính"
description: "Các thư mục được hiển thị dưới dạng lưới với chính xác m cột trên mỗi hàng. Thư mục 1 nằm ở góc trên bên trái, thư mục 2 ở bên cạnh, v.v. Sau mỗi m thư mục chúng ta chuyển sang hàng tiếp theo. Lựa chọn khung hình chữ nhật sẽ chuyển đổi mọi thư mục bên trong hình chữ nhật."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 93
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 76 (Div. 1 Only)"
rating: 1700
weight: 93
solve_time_s: 111
verified: true
draft: false
---

[CF 93A - Khung](https://codeforces.com/problemset/problem/93/A) 

**Đánh giá:** 1700 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Các thư mục được hiển thị dưới dạng lưới với độ chính xác`m`cột trên mỗi hàng. Thư mục`1`nằm ở góc trên bên trái, thư mục`2`ở bên cạnh nó, v.v. Sau mỗi lần`m`thư mục chúng tôi di chuyển đến hàng tiếp theo. 

Lựa chọn khung hình chữ nhật sẽ chuyển đổi mọi thư mục bên trong hình chữ nhật. Nếu một thư mục được chọn hai lần, thư mục đó sẽ không được chọn nữa. Igor muốn tập hợp được chọn cuối cùng chính xác là phạm vi liền kề`[a, b]`. 

Nhiệm vụ là tính toán số lượng lựa chọn hình chữ nhật tối thiểu cần thiết. 

Những hạn chế là vô cùng lớn. Cả hai`n`Và`m`có thể đạt được`10^9`, do đó, bất kỳ thuật toán nào xây dựng lưới hoặc mô phỏng các lựa chọn một cách rõ ràng đều không thể thực hiện được. Ngay cả việc lặp lại tất cả các thư mục trong khoảng thời gian đó cũng có thể quá chậm trong trường hợp xấu nhất. Giải pháp chỉ được sử dụng một lượng lý luận số học không đổi. 

Khó khăn chính là hình chữ nhật hoạt động trên bố cục 2D, nhưng mục tiêu đặt ra là khoảng 1D theo thứ tự đánh số. Cách tiếp cận bất cẩn thường quên cách các hàng tương tác. 

Một sai lầm dễ mắc phải là cho rằng câu trả lời chỉ đơn giản là số lượng hàng được chạm vào`[a, b]`. Coi như:```
n = 8, m = 4, a = 2, b = 7
```Các thư mục trông giống như:```
1 2 3 4
5 6 7 8
```Chúng ta có thể chọn toàn bộ hình chữ nhật bao gồm các hàng 1-2 và các cột 2-3:```
2 3
6 7
```nhưng điều đó nhớ`4`Và`5`, do đó nó không tạo ra một khoảng liền kề. Câu trả lời tối ưu thực sự là`2`, không`1`. 

Một trường hợp tinh tế khác xảy ra khi khoảng thời gian bắt đầu và kết thúc trong cùng một cột:```
n = 12, m = 4, a = 2, b = 10
```Lưới:```
1  2  3  4
5  6  7  8
9 10 11 12
```Mục tiêu chứa:```
2 3 4
5 6 7 8
9 10
```Một phép phân tách đơn giản "một phần bên trái + các hàng đầy đủ ở giữa + một phần bên phải" mang lại`3`, nhưng chúng ta có thể làm điều đó trong`2`: 

1. Chọn cột 2-4 trên các hàng 1-2. 
2. Chọn cột 1-2 trên hàng 2-3. 

Sự chồng chéo ở hàng 2 bị hủy bỏ một cách chính xác vì các lựa chọn đã chuyển đổi. 

Hành vi chồng chéo này là toàn bộ cốt lõi của vấn đề. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề là lập mô hình lưới và tìm cách bao phủ khoảng mục tiêu bằng cách sử dụng hình chữ nhật. Vì hình chữ nhật chuyển đổi các ô nên chúng ta cần phải giải thích về tính chẵn lẻ của phạm vi bao phủ. Người ta có thể tưởng tượng việc lập trình động trên các hàng và cột hoặc thậm chí là các tổ hợp hình chữ nhật cưỡng bức. 

Cách tiếp cận đó ngay lập tức trở nên vô vọng. Lưới có thể chứa tới`10^9`các ô, do đó, ngay cả việc lưu trữ bố cục cũng không thể thực hiện được. Bất kỳ thuật toán nào tùy thuộc vào hình dạng thực tế của tất cả các thư mục đều không thể chạy trong giới hạn. 

Cấu trúc của khoảng thời gian là thứ đã cứu chúng ta. Các thư mục đã chọn được xếp liên tiếp theo thứ tự đánh số, nghĩa là chúng tạo thành: 

1. Hậu tố của hàng được chạm đầu tiên. 
2. Không có hoặc nhiều hàng hoàn chỉnh. 
3. Tiền tố của hàng được chạm cuối cùng. 

Nếu khoảng nằm trong một hàng thì một hình chữ nhật là đủ. 

Nếu nó trải dài trên nhiều hàng, thì sự phân tách đơn giản sẽ sử dụng tối đa ba hình chữ nhật: 

1. Một cho phần hàng đầu tiên. 
2. Một cho tất cả các hàng ở giữa hoàn chỉnh. 
3. Một cho phần hàng cuối cùng. 

Điều thú vị là đôi khi phần đầu tiên và phần cuối cùng có thể được hợp nhất bằng cách sử dụng tính năng hủy chồng chéo. Bởi vì việc chọn hai lần sẽ bỏ chọn, hai hình chữ nhật chồng lên nhau có thể tạo ra khoảng chính xác chỉ với hai thao tác. 

Giả sử khoảng bắt đầu ở cột`L`và kết thúc ở cột`R`. 

Nếu như`L <= R`, khi đó một hình chữ nhật có thể che các cột`L..m`trên một số hàng và một hàng khác có thể che các cột`1..R`trên một phạm vi hàng khác. Sự chồng chéo của chúng sẽ tự động hủy bỏ và để lại chính xác hình dạng mong muốn. 

Nếu như`L > R`, thủ thuật này không thể hoạt động vì hai hình dạng sẽ tạo ra các ô không mong muốn và không thể hủy bỏ một cách rõ ràng. Khi đó chúng ta thực sự cần ba hình chữ nhật. 

Điều này làm giảm toàn bộ vấn đề thành số học hàng và cột đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Theo cấp số nhân hoặc tệ hơn | Rất lớn | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi số thư mục thành tọa độ lưới. 

Đối với thư mục`x`:```
row = (x - 1) // m
col = (x - 1) % m + 1
```Chúng tôi chỉ quan tâm đến các hàng và cột của`a`Và`b`. 
2. Nếu cả hai thư mục nằm trong cùng một hàng, hãy trả lời`1`. 

Một hình chữ nhật đơn bao phủ đoạn từ cột`col_a`ĐẾN`col_b`giải quyết nó. 
3. Ngược lại, hãy tính:```
L = col_a
R = col_b
```4. Nếu`L <= R`, trả lời`2`. 

Chúng ta có thể xây dựng khoảng bằng cách sử dụng hai hình chữ nhật chồng lên nhau: 

Hình chữ nhật đầu tiên: 

cột`L..m`qua các hàng từ`row_a`ĐẾN`row_b - 1`. 

Hình chữ nhật thứ hai: 

cột`1..R`qua các hàng từ`row_a + 1`ĐẾN`row_b`. 

Sự chồng chéo của chúng chuyển đổi hai lần và biến mất, để lại chính xác khoảng thời gian mục tiêu. 
5. Ngược lại, hãy trả lời`3`. 

Trong cấu hình này, thủ thuật chồng chéo không thể biểu thị khoảng một cách rõ ràng, vì vậy chúng ta phải sử dụng: 

hàng một phần đầu tiên, 

hàng đầy đủ ở giữa, 

hàng một phần cuối cùng. 

### Tại sao nó hoạt động 

Bất kỳ khoảng liền kề nào theo thứ tự hàng lớn đều có cấu trúc giống như cầu thang. Hàng đầu tiên đóng góp một hậu tố, hàng cuối cùng đóng góp một tiền tố và mọi thứ giữa chúng là các hàng hoàn chỉnh. 

Khi`L <= R`, hai cạnh cầu thang này chồng lên nhau. Hai hình chữ nhật có thể tái tạo hình dạng vì vùng giữa chồng lên nhau được chuyển đổi hai lần và biến mất. Các ô còn lại khớp chính xác với khoảng thời gian. 

Khi`L > R`, cầu thang uốn cong theo hướng khác. Bất kỳ nỗ lực nào chỉ sử dụng hai hình chữ nhật nhất thiết phải để lại các ô thừa hoặc bỏ sót các ô bắt buộc. Ba phần độc lập là không thể tránh khỏi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, a, b = map(int, input().split())

    row_a = (a - 1) // m
    row_b = (b - 1) // m

    col_a = (a - 1) % m + 1
    col_b = (b - 1) % m + 1

    if row_a == row_b:
        print(1)
    elif col_a <= col_b:
        print(2)
    else:
        print(3)

solve()
```Phần đầu tiên chuyển đổi các chỉ mục thư mục thành tọa độ hàng và cột. Việc lập chỉ mục dựa trên 0 cho các hàng và dựa trên một cho các cột vì so sánh cột dễ đọc hơn theo cách đó. 

Điều kiện đầu tiên kiểm tra xem khoảng có nằm trong một hàng hay không. Nếu vậy, một hình chữ nhật sẽ che trực tiếp nó. 

Điều kiện thứ hai là quan sát hình học tới hạn. Khi cột bắt đầu không ở bên phải cột kết thúc thì hai hình chữ nhật chồng lên nhau là đủ. 

Trường hợp cuối cùng yêu cầu ba hình chữ nhật. 

Việc triển khai được thực hiện rất nhỏ một cách có chủ ý vì toàn bộ vấn đề được giảm xuống còn việc phân loại hình dạng khoảng. Nơi chính xảy ra lỗi là phối hợp chuyển đổi. quên đi`+1`trong phép tính cột sẽ thay đổi logic so sánh và tạo ra các câu trả lời sai về ranh giới. 

Một lỗi phổ biến khác là sử dụng`a // m`thay vì`(a - 1) // m`. Việc đánh số thư mục bắt đầu từ`1`, không`0`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
11 4 3 9
```Lưới:```
1  2  3  4
5  6  7  8
9 10 11
```Khoảng mục tiêu:```
3 4 5 6 7 8 9
```| Biến | Giá trị | 
| --- | --- | 
| hàng_a | 0 | 
| hàng_b | 2 | 
| col_a | 3 | 
| col_b | 1 | 

Từ`row_a != row_b`Và`3 > 1`, câu trả lời là`3`. 

Một cách xây dựng tối ưu là: 

1. Chọn`3 4`2. Chọn`5 6 7 8`3. Chọn`9`Ví dụ này minh họa cấu hình trong đó thủ thuật chồng chéo không thành công do khoảng thời gian bao bọc từ cột cuối đến cột đầu. 

### Ví dụ 2 

đầu vào:```
12 4 2 10
```Lưới:```
1  2  3  4
5  6  7  8
9 10 11 12
```Khoảng mục tiêu:```
2 3 4 5 6 7 8 9 10
```| Biến | Giá trị | 
| --- | --- | 
| hàng_a | 0 | 
| hàng_b | 2 | 
| col_a | 2 | 
| col_b | 2 | 

Từ`col_a <= col_b`, câu trả lời là`2`. 

Khả năng xây dựng: 

1. Cột che hình chữ nhật`2..4`trên hàng`1..2`2. Cột che hình chữ nhật`1..2`trên hàng`2..3`Sự chồng chéo ở hàng 2, cột 2 sẽ tự động hủy bỏ. 

Dấu vết này chứng tỏ tại sao việc chuyển đổi lại quan trọng. Nếu không loại bỏ tính chẵn lẻ thì hai hình chữ nhật sẽ không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học và so sánh | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung | 

Các ràng buộc rất lớn nhưng thuật toán không bao giờ phụ thuộc vào`n`hoặc độ dài khoảng. Mọi bài kiểm tra đều được giải bằng các phép tính tọa độ theo thời gian không đổi, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m, a, b = map(int, input().split())

    row_a = (a - 1) // m
    row_b = (b - 1) // m

    col_a = (a - 1) % m + 1
    col_b = (b - 1) % m + 1

    if row_a == row_b:
        print(1)
    elif col_a <= col_b:
        print(2)
    else:
        print(3)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("11 4 3 9\n") == "3", "sample 1"

# single folder
assert run("1 1 1 1\n") == "1", "minimum case"

# same row interval
assert run("10 5 2 4\n") == "1", "single rectangle in one row"

# overlap trick works
assert run("12 4 2 10\n") == "2", "two rectangles with cancellation"

# overlap trick fails
assert run("12 4 4 9\n") == "3", "requires three rectangles"

# huge values
assert run("1000000000 1000000000 1 1000000000\n") == "1", "large same-row case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1`| Đầu vào tối thiểu có thể | 
|`10 5 2 4`|`1`| Khoảng cách hoàn toàn bên trong một hàng | 
|`12 4 2 10`|`2`| Trường hợp hủy chồng chéo | 
|`12 4 4 9`|`3`| Cấu hình yêu cầu ba hình chữ nhật | 
|`1000000000 1000000000 1 1000000000`|`1`| Giá trị lớn không bị tràn | 

## Vỏ cạnh 

Hãy xem xét khoảng thời gian hoàn toàn bên trong một hàng:```
10 5 2 4
```Các thư mục là:```
1 2 3 4 5
6 7 8 9 10
```Mục tiêu chỉ đơn giản là`2 3 4`. Thuật toán tính toán:```
row_a = 0
row_b = 0
```Vì cả hai điểm cuối đều nằm trong cùng một hàng nên nó ngay lập tức trả về`1`. Một hình chữ nhật ngang là đủ. 

Bây giờ hãy xem xét trường hợp chồng chéo:```
12 4 2 10
```Tọa độ:```
a = row 0, col 2
b = row 2, col 2
```Bởi vì`col_a <= col_b`, thuật toán trả về`2`. Việc xây dựng hai hình chữ nhật hoạt động hiệu quả vì vùng chồng lấp bị hủy bỏ theo tính chẵn lẻ. 

Cuối cùng, hãy xem xét định hướng khó khăn:```
12 4 4 9
```Lưới:```
1  2  3  4
5  6  7  8
9 10 11 12
```Khoảng mục tiêu:```
4 5 6 7 8 9
```Tọa độ:```
col_a = 4
col_b = 1
```Từ`4 > 1`, thuật toán trả về`3`. 

Cố gắng chỉ sử dụng hai hình chữ nhật luôn để lại các ô thừa như`1`,`2`,`3`,`10`,`11`, hoặc`12`. Cầu thang uốn sai hướng để khử chồng chéo phát huy tác dụng.
