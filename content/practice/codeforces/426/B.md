---
title: "CF 426B - Sereja và phản ánh"
description: "Chúng ta bắt đầu với một số ma trận nhỏ b. Một thao tác phản chiếu sẽ tăng gấp đôi chiều cao của nó. Nửa trên không thay đổi, nửa dưới trở thành các hàng của nửa trên được viết theo thứ tự ngược lại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 426
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 243 (Div. 2)"
rating: 1300
weight: 426
solve_time_s: 97
verified: true
draft: false
---

[CF 426B - Sereja và Mirroring](https://codeforces.com/problemset/problem/426/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một số ma trận nhỏ`b`. Một thao tác phản chiếu sẽ tăng gấp đôi chiều cao của nó. Nửa trên không thay đổi, nửa dưới trở thành các hàng của nửa trên được viết theo thứ tự ngược lại. 

Ví dụ: nếu ma trận hiện tại là:```
A
B
C
```sau khi phản chiếu nó trở thành:```
A
B
C
C
B
A
```Quá trình này có thể được lặp lại nhiều lần. Bắt đầu từ một ma trận chưa biết`b`, sau vài lần phản chiếu cuối cùng chúng ta cũng thu được ma trận đã cho`a`. 

Nhiệm vụ là xác định số hàng nhỏ nhất có thể có trong ma trận gốc`b`. 

Ma trận chỉ chứa`0`Và`1`, nhưng giá trị thực tế không quan trọng lắm. Điều quan trọng là liệu hai hàng có giống nhau hay không. 

Những hạn chế là rất nhỏ. Cả hai kích thước nhiều nhất là`100`, vì vậy ngay cả một`O(n^2 * m)`giải pháp dễ dàng đủ nhanh. Một hàng so sánh chi phí`O(m)`, và với nhiều nhất`100`hàng, chúng ta có thể đủ khả năng quét toàn bộ ma trận nhiều lần. 

Phần khó khăn là hiểu được việc phản chiếu lặp đi lặp lại thực sự hàm ý gì về mặt cấu trúc. 

Sau mỗi lần phản chiếu, toàn bộ ma trận trở nên đối xứng quanh tâm của nó. Việc lặp lại thao tác một lần nữa sẽ bảo toàn tính đối xứng đó một cách đệ quy. Điều này có nghĩa là ma trận cuối cùng phải rút gọn được bằng cách liên tục loại bỏ các nửa đối xứng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một ma trận có một hàng:```
1 3
0 1 0
```Câu trả lời là`1`. Việc triển khai bất cẩn có thể cố gắng thu nhỏ mãi mãi hoặc cho rằng chiều cao phải bằng nhau. 

Một trường hợp tinh vi khác là khi ma trận chỉ đối xứng ở mức ngoài cùng chứ không đối xứng theo kiểu đệ quy:```
4 1
0
1
1
0
```Câu trả lời là`2`, không`1`. 

Toàn bộ ma trận là đối xứng, do đó một phép rút gọn là hợp lệ:```
0
1
```Nhưng ma trận còn lại này không tự nó được tạo ra bằng cách phản chiếu, vì các hàng của nó khác nhau. 

Một lỗi phổ biến là chỉ kiểm tra xem toàn bộ ma trận có đối xứng một lần hay không. 

Một ma trận có kích thước lẻ cũng có vấn đề:```
3 1
0
1
0
```Câu trả lời là`3`. 

Phản chiếu luôn nhân đôi số lượng hàng, vì vậy mọi ma trận được tạo ngoại trừ ma trận ban đầu đều có chiều cao chẵn. Từ`3`thật kỳ quặc, không thể thực hiện được thao tác thu nhỏ nào cả. 

## Phương pháp tiếp cận 

Cách brute-force là mô phỏng quá trình ngược lại. 

Giả sử chúng ta hiện có một ma trận với`k`hàng. Nếu như`k`chẵn, chúng ta có thể kiểm tra xem nửa trên có bằng nửa dưới đảo ngược hay không. Nếu điều kiện này đúng thì ma trận có thể được tạo ra bằng cách phản chiếu nửa trên của nó, vì vậy chúng ta thu nhỏ ma trận về phần đầu tiên của nó.`k / 2`hàng và tiếp tục. 

Cuối cùng, không thể giảm được nữa và chiều cao hiện tại là câu trả lời hợp lệ nhỏ nhất. 

Tại sao điều này làm việc? Bởi vì sự phản chiếu là hoàn toàn mang tính quyết định. Nếu một ma trận được tạo ra từ một số ma trận nhỏ hơn thì nửa đầu của nó xác định duy nhất nửa sau. Không bao giờ có sự mơ hồ về ma trận gốc phải là gì. 

Phương pháp vũ phu ở đây đã đủ nhanh rồi. Ở mỗi lần giảm chúng ta so sánh nhiều nhất`n / 2`cặp hàng và chi phí so sánh mỗi hàng`O(m)`. Vì chiều cao mỗi lần giảm một nửa nên tổng công được giới hạn bởi:```
n*m + (n/2)*m + (n/4)*m + ...
```đó là`O(n*m)`tổng thể. 

Quan sát quan trọng là việc phản chiếu lặp đi lặp lại tạo ra tính đối xứng đệ quy. Việc giảm hợp lệ không chỉ là ma trận hiện tại đối xứng một lần. Sau khi rút gọn, ma trận nhỏ hơn lại phải thỏa mãn tính chất tương tự. 

Cấu trúc đệ quy này làm cho việc giảm tham lam trở nên tối ưu. Bất cứ khi nào có thể giảm bớt, việc lấy nó ngay lập tức không bao giờ có hại, bởi vì bất kỳ ma trận gốc hợp lệ nào cũng phải nằm trong nửa đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra đệ quy Brute Force | O(n² · m) | O(1) | Đã chấp nhận | 
| Giảm một nửa lặp đi lặp lại tối ưu | O(n · m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng hàng ma trận. 
2. Gọi số hàng hiện tại là`k = n`. 
3. Trong khi`k`chẵn, hãy cố gắng giảm ma trận. 
4. Hàng so sánh`i`từ nửa trên có hàng`k - 1 - i`từ nửa dưới cho mỗi`0 ≤ i < k/2`. 

Điều này kiểm tra xem nửa dưới có chính xác là đảo ngược của nửa trên hay không, đây là định nghĩa của ma trận phản chiếu. 
5. Nếu có cặp nào khác nhau thì dừng ngay. 

Ma trận hiện tại không thể được tạo ra bằng một thao tác phản chiếu nữa, vì vậy việc giảm thêm là không thể. 
6. Nếu tất cả các cặp đều khớp thì thay thế`k`với`k / 2`. 

Ma trận có thể được tạo ra từ nửa trên của nó, vì vậy chúng ta tiếp tục tìm kiếm một gốc thậm chí còn nhỏ hơn. 
7. Khi vòng lặp dừng, xuất ra`k`. 

### Tại sao nó hoạt động 

Thuật toán duy trì sự bất biến mà đầu tiên`k`các hàng tạo thành một ma trận ứng cử viên hợp lệ có thể tạo ra đầu vào ban đầu sau một số lần phản chiếu. 

Bất cứ khi nào ma trận thỏa mãn thuộc tính phản chiếu, trạng thái trước đó duy nhất có thể là nửa trên của nó. Thu nhỏ đến một nửa sẽ duy trì tính chính xác. 

Nếu thuộc tính phản chiếu không thành công thì không tồn tại tiền thân nhỏ hơn. Bất kỳ thao tác phản chiếu nào cũng luôn tạo ra sự đối xứng đảo ngược hoàn hảo giữa các nửa, do đó việc vi phạm điều kiện này sẽ khiến cho việc rút gọn thêm là không thể. 

Bởi vì mỗi lần giảm thành công đều làm giảm một nửa ma trận, nên quá trình cuối cùng sẽ đạt đến kích thước tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [input().split() for _ in range(n)]

    k = n

    while k % 2 == 0:
        half = k // 2
        ok = True

        for i in range(half):
            if a[i] != a[k - 1 - i]:
                ok = False
                break

        if not ok:
            break

        k = half

    print(k)

solve()
```Ma trận được lưu trữ dưới dạng danh sách các hàng. Mỗi hàng được giữ dưới dạng danh sách các chuỗi vì so sánh đẳng thức giữa các hàng là tất cả những gì chúng ta cần. 

Biến`k`đại diện cho chiều cao hoạt động hiện tại. Ban đầu nó bằng`n`, chiều cao ma trận đầy đủ. 

Bên trong vòng lặp, chúng ta chỉ thử giảm bớt khi`k`là chẵn. Phản chiếu luôn nhân đôi số lượng hàng, do đó không thể giảm thêm ma trận có kích thước lẻ. 

Sự so sánh:```
a[i] != a[k - 1 - i]
```kiểm tra tính đối xứng cần thiết một cách trực tiếp. Hàng trên cùng của nửa trên phải khớp với hàng dưới cùng của nửa dưới, hàng thứ hai phải khớp với hàng thứ hai cuối cùng, v.v. 

Một chi tiết tinh tế là chúng tôi không bao giờ tạo ra các ma trận nhỏ hơn về mặt vật lý. Chúng tôi chỉ đơn giản là giảm`k`. Các hàng có liên quan luôn là hàng đầu tiên`k`các hàng của mảng ban đầu. 

Một sai lầm dễ mắc phải khác là so sánh:```
a[i] != a[half + i]
```kiểm tra các nửa palindrome thông thường thay vì các nửa được phản chiếu. Nửa dưới phải xuất hiện theo thứ tự ngược lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
0 0 1
1 1 0
1 1 0
0 0 1
```Dấu vết: 

| Bước | k | Hàng so sánh | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 4 | hàng0 so với hàng3 | bằng | 
| | 4 | hàng1 so với hàng2 | bằng | 
| Giảm | 2 | ma trận có thể rút gọn | tiếp tục | 
| Tiếp theo | 2 | hàng0 so với hàng1 | khác nhau | 
| Dừng lại | 2 | không thể giảm thêm | trả lời | 

Đầu ra:```
2
```Lần giảm đầu tiên thành công vì ma trận được phản chiếu hoàn hảo. Phần còn lại`2 x 3`ma trận không đối xứng nên nó phải là ma trận gốc. 

### Ví dụ 2 

đầu vào:```
8 1
1
0
1
1
1
1
0
1
```Dấu vết: 

| Bước | k | Hàng so sánh | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 8 | hàng0 so với hàng7 | bằng | 
| | 8 | hàng1 so với hàng6 | bằng | 
| | 8 | hàng2 so với hàng5 | bằng | 
| | 8 | hàng3 so với hàng4 | bằng | 
| Giảm | 4 | ma trận có thể rút gọn | tiếp tục | 
| Tiếp theo | 4 | hàng0 so với hàng3 | bằng | 
| | 4 | hàng1 so với hàng2 | khác nhau | 
| Dừng lại | 4 | không thể giảm thêm | trả lời | 

Đầu ra:```
4
```Ví dụ này thể hiện tính đối xứng đệ quy. Toàn bộ ma trận được phản chiếu một lần, nhưng ma trận rút gọn không được phản chiếu lại nên quá trình dừng ở đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m) | Mỗi lần giảm so sánh các hàng và tổng số hàng được so sánh trên tất cả các phần được giới hạn bởi`n`| 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài ma trận đầu vào | 

Với`n, m ≤ 100`, giải pháp này thoải mái trong giới hạn. Ngay cả số lượng so sánh hàng trong trường hợp xấu nhất cũng rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = [input().split() for _ in range(n)]

    k = n

    while k % 2 == 0:
        half = k // 2
        ok = True

        for i in range(half):
            if a[i] != a[k - 1 - i]:
                ok = False
                break

        if not ok:
            break

        k = half

    print(k)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
    "4 3\n"
    "0 0 1\n"
    "1 1 0\n"
    "1 1 0\n"
    "0 0 1\n"
) == "2\n", "sample 1"

# minimum size
assert run(
    "1 1\n"
    "0\n"
) == "1\n", "single row"

# fully reducible several times
assert run(
    "8 1\n"
    "1\n"
    "0\n"
    "0\n"
    "1\n"
    "1\n"
    "0\n"
    "0\n"
    "1\n"
) == "2\n", "multiple halvings"

# odd size cannot reduce
assert run(
    "3 1\n"
    "0\n"
    "1\n"
    "0\n"
) == "3\n", "odd height"

# symmetric once but not twice
assert run(
    "4 1\n"
    "0\n"
    "1\n"
    "1\n"
    "0\n"
) == "2\n", "single reduction"

# all rows equal
assert run(
    "8 2\n"
    "1 1\n"
    "1 1\n"
    "1 1\n"
    "1 1\n"
    "1 1\n"
    "1 1\n"
    "1 1\n"
    "1 1\n"
) == "1\n", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận hàng đơn | 1 | Kích thước tối thiểu có thể | 
| Ma trận 8 hàng có thể rút gọn hoàn toàn | 2 | Giảm đệ quy nhiều lần | 
| Ma trận đối xứng cỡ lẻ | 3 | Giảm khối chiều cao lẻ | 
| Đối xứng một lần nhưng không hai lần | 2 | Vấn đề tài sản đệ quy | 
| Tất cả các hàng bằng nhau | 1 | Số lần giảm tối đa | 

## Vỏ cạnh 

Xét trường hợp chiều cao lẻ:```
3 1
0
1
0
```Thuật toán bắt đầu với`k = 3`. Từ`k`là số lẻ, vòng lặp không bao giờ bắt đầu và câu trả lời vẫn còn đó`3`. 

Điều này đúng vì việc phản chiếu luôn nhân đôi số lượng hàng. Không có chuỗi phản chiếu nào có thể tạo ra một ma trận có kích thước lẻ từ một ma trận nhỏ hơn. 

Bây giờ hãy xem xét một ma trận đối xứng chỉ ở một mức:```
4 1
0
1
1
0
```Thuật toán kiểm tra:```
row0 == row3
row1 == row2
```Cả hai đều thành công nên`k`trở thành`2`. 

Tiếp theo nó kiểm tra:```
row0 == row1
```Điều này không thành công nên thuật toán dừng lại và xuất ra`2`. 

Điều này chứng tỏ tại sao việc kiểm tra đệ quy lại quan trọng. Một lớp đối xứng duy nhất là không đủ để giảm kích thước`1`. 

Cuối cùng, coi tất cả các hàng đều bằng nhau:```
8 1
1
1
1
1
1
1
1
1
```Mọi kiểm tra đối xứng đều thành công:```
8 -> 4 -> 2 -> 1
```Thuật toán xuất ra chính xác`1`, ma trận gốc nhỏ nhất có thể.
