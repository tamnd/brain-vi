---
title: "CF 102386C - \u041d\u0430\u0439\u0434\u0438 \u043e\u0442\u043b\u0438\u0447\u0438\u044f"
description: "Chúng ta được cho hai hình ảnh ký tự hình chữ nhật có cùng kích thước. Mỗi hình ảnh được biểu thị bằng n hàng, mỗi hàng chứa chính xác m ký tự không phải khoảng trắng."
date: "2026-08-14T13:25:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "C"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 75
verified: true
draft: false
---

[CF 102386C - \u041d\u0430\u0439\u0434\u0438 \u043e\u0442\u043b\u0438\u0447\u0438\u044f](https://codeforces.com/problemset/problem/102386/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai hình ảnh ký tự hình chữ nhật có cùng kích thước. Mỗi hình ảnh được thể hiện bằng`n`hàng, mỗi hàng chứa chính xác`m`ký tự không phải khoảng trắng. Hai hình ảnh được căn chỉnh theo từng ô, vì vậy vị trí`(i, j)`trong hình ảnh đầu tiên tương ứng trực tiếp với vị trí`(i, j)`trong hình ảnh thứ hai. 

Nhiệm vụ chỉ đơn giản là đếm xem có bao nhiêu vị trí tương ứng chứa các ký tự khác nhau. Bản thân các ký tự không có ý nghĩa gì đặc biệt. Một sự khác biệt giữa`A[i][j]`Và`B[i][j]`đóng góp một cho câu trả lời, trong khi các ký tự bằng nhau đóng góp bằng không. 

Tác vụ ban đầu cố định hình ảnh thực tế ở 80 hàng x 37 cột nên chỉ có 2960 vị trí. Tuy nhiên, định dạng đầu vào cung cấp`n`Và`m`, điều này làm cho quá trình triển khai hoạt động một cách tự nhiên đối với mọi thứ nguyên đáp ứng định dạng. Ngay cả việc quét trực tiếp từng ô cũng chỉ thực hiện`n * m`so sánh, và với mức tối đa đã nêu thì đây chỉ là 2960 so sánh. Thuật toán bậc hai về số lượng ô là không cần thiết, trong khi bất kỳ lần quét tuyến tính nào cũng có thể thoải mái trong giới hạn một giây và 256 MB. 

Có một số chi tiết đầu vào nhỏ rất dễ xử lý sai. Hình ảnh có thể chứa dấu câu như`.`hoặc`\`, vì vậy việc phân tích các hàng dưới dạng chuỗi tùy ý sẽ an toàn hơn việc cố gắng diễn giải các ký tự của chúng. Ví dụ: mẫu chứa dấu gạch chéo ngược:```
1 1
\
.
```Hai ô khác nhau nên câu trả lời là`1`. Trình phân tích cú pháp bất cẩn coi dấu gạch chéo ngược là ký tự thoát thay vì dữ liệu đầu vào thông thường có thể đọc hàng không chính xác. 

Một trường hợp ranh giới khác là khi các hình ảnh hoàn toàn giống nhau:```
2 3
abc
XYZ
abc
XYZ
```Đầu ra đúng là`0`. Thuật toán phải đếm sự khác biệt thay vì đếm các vị trí trùng khớp. 

Trường hợp ngược lại là khi mọi vị trí đều khác nhau:```
2 2
ab
cd
12
34
```Đầu ra đúng là`4`. Một giải pháp dừng lại sau khi tìm thấy sự khác biệt đầu tiên hoặc vô tình chỉ so sánh hàng đầu tiên sẽ bị tính thiếu. 

Bản thân kích thước cũng có thể là tối thiểu. Vì```
1 1
a
b
```câu trả lời là`1`, trong khi```
1 1
a
a
```có câu trả lời`0`. Những trường hợp này phát hiện từng lỗi một trong cả việc duyệt hàng và cột. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp nhất là truy cập mọi vị trí của hình ảnh đầu tiên, tìm vị trí tương ứng trong hình ảnh thứ hai và so sánh các ký tự của chúng. Vì tọa độ đã xác định được các ô tương ứng nên một vòng lặp lồng nhau trên các hàng và cột là đủ. Vì`n * m`các ô này thực hiện chính xác`n * m`so sánh trong trường hợp xấu nhất, đó là so sánh 2960 cho hình ảnh 80 x 37 cố định. Điều này đã dễ dàng đủ nhanh. 

Việc triển khai kém cẩn thận hơn có thể biến vấn đề thành vấn đề tìm kiếm, quét toàn bộ hình ảnh thứ hai bất cứ khi nào một ô từ hình ảnh đầu tiên được xử lý. Điều đó thực hiện lên đến`(n * m)^2`so sánh nhân vật. Với 2960 ô, đây là 8.761.600 so sánh, vẫn chưa phải là thảm họa đối với các kích thước cố định này, nhưng nó hoàn toàn bỏ qua thực tế là tọa độ đã cho chúng ta câu trả lời cho vấn đề so khớp. Quan trọng hơn, nếu các chiều được tổng quát hóa thành các giá trị lớn thì phép tính bậc hai sẽ nhanh chóng trở nên không phù hợp. 

Điều quan trọng là không cần phải tìm kiếm các ký tự tương ứng. Các hình ảnh được căn chỉnh và có kích thước giống hệt nhau, vì vậy`(i, j)`trong một hình ảnh luôn tương ứng với`(i, j)`trong cái khác. Vấn đề chính xác là khoảng cách Hamming giữa hai lưới ký tự có kích thước bằng nhau. Chúng ta có thể quét cả hai hình ảnh một lần và tăng câu trả lời bất cứ khi nào hai ký tự ở cùng một vị trí khác nhau. 

Do đó, cách tiếp cận tối ưu sử dụng một so sánh cho mỗi ô. Trong Python, so sánh từng ký tự của hai hàng với`zip`thể hiện trực tiếp hoạt động tương tự và giữ cho việc thực hiện đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm hình ảnh thứ hai cho mỗi ô | O((nm)^2) | O(nm) | Chậm một cách không cần thiết | 
| So sánh trực tiếp từng tế bào | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`, mô tả số lượng hàng và cột trong mỗi hình ảnh. 
2. Đọc phần tiếp theo`n`dòng vào hình ảnh đầu tiên. Mỗi dòng đại diện cho một hàng hoàn chỉnh, do đó, nó phải được giữ dưới dạng một chuỗi thay vì chia thành các mã thông báo riêng lẻ. 
3. Đọc phần sau`n`dòng vào hình ảnh thứ hai bằng cách sử dụng cùng một biểu diễn. Hai hình ảnh hiện có các hàng tương ứng ở cùng chỉ số. 
4. Khởi tạo bộ đếm chênh lệch về 0. Chưa có vị trí nào được kiểm tra, vì vậy bộ đếm biểu thị số lượng ô khác nhau trong số tất cả các vị trí được xử lý cho đến nay. 
5. Đối với mỗi cặp hàng tương ứng, so sánh các ký tự của chúng ở các vị trí cột bằng nhau. Bất cứ khi nào các ký tự khác nhau, hãy tăng bộ đếm lên một. Điều này là đủ vì mỗi ô có chính xác một ô tương ứng trong hình ảnh kia. 
6. In bộ đếm. Sau khi tất cả các hàng và cột đã được xử lý, chính xác là số vị trí mà hai hình ảnh khác nhau. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của hình ảnh, bộ đếm sẽ bằng số vị trí trong tiền tố đó mà hai hình ảnh có các ký tự khác nhau. Khi cặp ký tự tương ứng tiếp theo được kiểm tra, chính xác một trong hai điều sẽ xảy ra. Nếu chúng bằng nhau thì số chênh lệch không thay đổi. Nếu chúng khác nhau thì chính xác một vị trí khác biệt mới sẽ được thêm vào. Do đó, bất biến vẫn đúng cho mọi ô được xử lý. Sau ô cuối cùng, tiền tố là toàn bộ cặp hình ảnh, do đó bộ đếm chính xác là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    first = [input().rstrip('\n') for _ in range(n)]
    second = [input().rstrip('\n') for _ in range(n)]

    answer = 0

    for row_a, row_b in zip(first, second):
        answer += sum(a != b for a, b in zip(row_a, row_b))

    print(answer)

if __name__ == "__main__":
    solve()
```đầu tiên`n`các hàng được lưu trữ trong`first`, và tiếp theo`n`các hàng được lưu trữ trong`second`. Việc giữ các hàng hoàn chỉnh sẽ làm cho sự tương ứng giữa hai hình ảnh trở nên rõ ràng.`rstrip('\n')`chỉ loại bỏ dấu kết thúc dòng. Điều này tốt hơn là sử dụng`strip()`, vì câu lệnh xác định các hàng hình ảnh dưới dạng chuỗi ký tự và trình phân tích cú pháp không được loại bỏ các ký tự khác khỏi đầu vào một cách không cần thiết. 

Bên ngoài`zip`hàng cặp`i`của hình ảnh đầu tiên với hàng`i`của hình ảnh thứ hai. Bên trong`zip`sau đó ghép cột`j`của những hàng đó. Với mỗi cặp, biểu thức`a != b`đánh giá để`True`hoặc`False`, mà Python coi là`1`hoặc`0`khi tổng hợp. 

Không có vấn đề tràn số nguyên trong Python và ngay cả trong ngôn ngữ có số nguyên có chiều rộng cố định, câu trả lời không bao giờ có thể vượt quá`n * m`, chỉ có 2960 cho những hình ảnh đã nêu. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, hai hình ảnh có năm hàng và mười ba cột. Bảng sau chỉ ghi lại số lượng chênh lệch tích lũy sau mỗi hàng. 

| Hàng | Sự khác biệt trong hàng hiện tại | Tổng số khác biệt | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 2 | 2 | 
| 3 | 3 | 5 | 
| 4 | 2 | 7 | 
| 5 | 0 | 7 | 

Bộ đếm cuối cùng là`7`, phù hợp với đầu ra mẫu. Dấu vết chứng minh rằng sự khác biệt được tính độc lập cho mỗi ký tự tương ứng, bao gồm dấu câu và dấu gạch chéo ngược. 

Đối với Mẫu 2, hãy xem xét các hình ảnh một ô nhỏ nhất có thể:```
1 1
a
b
```Quá trình quét có chứa một so sánh. 

| Hàng | Cột | Ký tự đầu tiên | Nhân vật thứ hai | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 |`a`|`b`| 1 | 

Câu trả lời là`1`. Nếu cả hai nhân vật đều`a`, dấu vết tương tự sẽ để lại quầy tại`0`. Điều này xác nhận rằng thuật toán đếm sự khác biệt thay vì chỉ đếm các ô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi vị trí ký tự được so sánh chính xác một lần | 
| Không gian | O(nm) | Cả hai hình ảnh đều được lưu dưới dạng chuỗi đầu vào | 

Đối với kích thước hình ảnh cố định thực tế là 80 x 37, quá trình quét chỉ kiểm tra 2960 vị trí tương ứng. Việc sử dụng bộ nhớ cũng rất nhỏ so với giới hạn 256 MB và quá trình quét tuyến tính dễ dàng phù hợp với giới hạn thời gian một giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    first = [input().rstrip('\n') for _ in range(n)]
    second = [input().rstrip('\n') for _ in range(n)]

    answer = 0
    for row_a, row_b in zip(first, second):
        answer += sum(a != b for a, b in zip(row_a, row_b))

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """5 13
.__.......__.
.).\\...../.(.
)_..\\_V_/.._(
..)__...__(..
.....‘_’.....
.__.......__.
.|.\\...../.|.
!..\\_M_/..!
..\\__...__/..
.....‘_’.....
"""
) == "7\n", "sample 1"

# Minimum-size input, equal images
assert run(
    """1 1
a
a
"""
) == "0\n", "minimum equal images"

# Minimum-size input, different images
assert run(
    """1 1
a
b
"""
) == "1\n", "minimum different images"

# All cells differ
assert run(
    """2 3
abc
def
123
456
"""
) == "6\n", "every cell differs"

# All cells are equal
assert run(
    """3 4
....
abcd
1234
....
abcd
1234
"""
) == "0\n", "all cells equal"

# Differences at the first and last positions of the grid
assert run(
    """2 2
ab
cd
xb
cy
"""
) == "2\n", "boundary positions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`,`a`,`a`|`0`| Kích thước tối thiểu và sự bình đẳng | 
|`1 1`,`a`,`b`|`1`| Kích thước tối thiểu và một sự khác biệt duy nhất | 
| Hai hình ảnh 2 x 3 hoàn toàn khác nhau |`6`| Mọi ô đều phải được đếm | 
| Hai hình ảnh 3 x 4 giống hệt nhau |`0`| Không có kết quả dương tính giả | 
| Sự khác biệt ở`(1,1)`Và`(2,2)`|`2`| Ô đầu tiên và ô cuối cùng, xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp không rõ ràng đầu tiên là hình ảnh một ô có các ký tự khác nhau:```
1 1
a
b
```Vòng lặp bên ngoài xử lý một hàng và vòng lặp bên trong`zip`tạo ra một cặp,`a`Và`b`. Vì chúng khác nhau nên bộ đếm trở thành`1`, đó là đầu ra chính xác. Không có trường hợp đặc biệt cần thiết cho`n = 1`hoặc`m = 1`. 

Một hình ảnh hoàn toàn giống hệt nhau được xử lý trực tiếp:```
2 3
abc
XYZ
abc
XYZ
```Hàng đầu tiên tạo ra ba cặp bằng nhau và thêm số 0. Hàng thứ hai cũng làm tương tự, để quầy ở`0`. Thuật toán không bao giờ giả định rằng có ít nhất một sự khác biệt tồn tại. 

Một hình ảnh hoàn toàn khác lại thể hiện thái cực ngược lại:```
2 2
ab
cd
12
34
```Hàng đầu tiên đóng góp hai điểm khác biệt và hàng thứ hai đóng góp thêm hai điểm khác biệt. Câu trả lời cuối cùng là`4`, chính xác là số lượng ô trong hình ảnh. 

Cuối cùng, các ký tự như dấu gạch chéo ngược và dấu chấm câu là dữ liệu hình ảnh thông thường. Ví dụ:```
1 3
\._
/._
```Hai vị trí đầu tiên khác nhau và vị trí thứ ba bằng nhau nên đáp án là`2`. Việc triển khai đọc toàn bộ hàng dưới dạng một chuỗi và so sánh trực tiếp các ký tự của nó, do đó không có ký hiệu nào trong số này yêu cầu thuật toán xử lý đặc biệt.
