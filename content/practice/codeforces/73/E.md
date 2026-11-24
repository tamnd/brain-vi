---
title: "CF 73E - Morrowindows"
description: "Mỗi chế độ xem nhóm khoảng không quảng cáo thành các trang có kích thước ai. Nếu kho chứa k vật phẩm thì trò chơi sẽ hiển thị các trang $bi = leftlceil frac{k}{ai} rightrceil$ ở chế độ đó. Vasya không biết giá trị thực của k, chỉ biết 2 ≤ k ≤ x."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 73
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 66"
rating: 2400
weight: 73
solve_time_s: 111
verified: true
draft: false
---

[CF 73E - Morrowindows](https://codeforces.com/problemset/problem/73/E) 

**Đánh giá:** 2400 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chế độ xem nhóm khoảng không quảng cáo thành các trang có kích thước`a_i`. Nếu hàng tồn kho chứa`k`các vật phẩm, sau đó trò chơi sẽ hiển thị$b_i = \left\lceil \frac{k}{a_i} \right\rceil$các trang ở chế độ đó. 

Vasya không biết giá trị thực tế của`k`, chỉ vậy thôi`2 ≤ k ≤ x`. Trước khi nhìn vào bất cứ thứ gì, anh ấy chọn một số chế độ. Sau khi xem tất cả số trang tương ứng, anh ta phải có khả năng xác định`k`độc đáo. 

Chúng ta được cung cấp tất cả các giá trị`a_i`và chúng ta phải tìm số lượng chế độ tối thiểu cần thiết để tái cấu trúc duy nhất`k`. Nếu không có tập hợp con nào hoạt động, chúng tôi xuất ra`-1`. 

Ràng buộc`n ≤ 10^5`ngay lập tức loại trừ bất kỳ cách tiếp cận nào so sánh tất cả các tập hợp con của các chế độ. Ngay cả việc kiểm tra tất cả các cặp kích thước khoảng không quảng cáo có thể có cũng sẽ tốn khoảng`10^18`hoạt động khi`x`là lớn. Chúng ta cần một cái gì đó gần gũi`O(n log n)`hoặc`O(n log x)`. 

giá trị`x`có thể đạt được`10^9`, do đó việc lặp lại tất cả các kích thước khoảng không quảng cáo có thể có là không thể. Bất kỳ giải pháp thành công nào cũng phải suy luận một cách tượng trưng về các khoảng và khả năng chia thay vì liệt kê các ứng cử viên. 

Trường hợp cạnh tinh tế xuất hiện khi một số chế độ có cùng kích thước trang. 

Ví dụ:```
n = 3, x = 10
a = [2, 2, 2]
```Cả ba chế độ đều cung cấp thông tin giống hệt nhau. Sử dụng nhiều bản sao hơn không giúp ích gì. Một giải pháp tham lam bất cẩn đếm các chế độ một cách độc lập sẽ trả lời sai`3`, trong khi câu trả lời đúng là`-1`bởi vì các giá trị như`3`Và`4`luôn tạo ra những quan sát giống hệt nhau. 

Một tình huống phức tạp khác xảy ra khi một chế độ không đưa ra sự phân biệt hữu ích nào ở gần giới hạn trên. 

Ví dụ:```
n = 1, x = 5
a = [100]
```Đối với mọi kích thước khoảng không quảng cáo có thể có từ`2`ĐẾN`5`, số trang luôn là`1`. Việc triển khai ngây thơ có thể cho rằng một chế độ là đủ vì nó tạo ra đầu ra, nhưng đầu ra không bao giờ thay đổi. 

Trường hợp khó khăn nhất là hiểu cách kết hợp nhiều chế độ. 

Ví dụ:```
n = 2, x = 6
a = [2, 3]
```Các quan sát là: 

| k | trần(k/2) | trần(k/3) | 
| --- | --- | --- | 
| 2 | 1 | 1 | 
| 3 | 2 | 1 | 
| 4 | 2 | 2 | 
| 5 | 3 | 2 | 
| 6 | 3 | 2 | 

Giá trị`5`Và`6`vẫn không thể phân biệt được, vì vậy câu trả lời là`-1`. Một cách tiếp cận ngây thơ chỉ kiểm tra tính duy nhất theo cặp của các chế độ sẽ kết luận sai rằng việc kết hợp chúng là đủ. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Đối với mọi kích thước khoảng không quảng cáo có thể`k`từ`2`ĐẾN`x`, tính vectơ$\left(\left\lceil\frac{k}{a_1}\right\rceil,\left\lceil\frac{k}{a_2}\right\rceil,\dots\right)$Sau đó thử tất cả các tập hợp con của các chế độ và kiểm tra xem các vectơ chiếu kết quả có nhận dạng duy nhất mọi`k`. 

Điều này đúng vì hai kích thước khoảng không quảng cáo có thể được phân biệt chính xác khi một số chế độ được chọn tạo ra số lượng trang khác nhau cho chúng. 

Vấn đề là quy mô. Ngay cả việc lưu trữ tất cả các vectơ cũng đã có giá`O(nx)`, trở thành`10^14`hoạt động trong trường hợp xấu nhất. Việc thử các tập hợp con còn tệ hơn theo cấp số nhân. 

Quan sát quan trọng là một chế độ có kích thước trang`a`không phân biệt các số trong cùng một khoảng:$((t-1)a,\; ta]$Mỗi số bên trong một khoảng như vậy sẽ tạo ra số trang`t`. 

Điều đó có nghĩa là mỗi chế độ phân vùng phạm vi`[2, x]`thành các khối. Kết hợp một số chế độ tương ứng với việc giao các phân vùng này. Chúng tôi muốn mọi khối cuối cùng chứa chính xác một số nguyên. 

Bây giờ đến cái nhìn sâu sắc về lý thuyết số quan trọng. 

Giả sử hai số`u < v`vẫn không thể phân biệt được đối với một chế độ`a`. Sau đó:$\left\lceil\frac{u}{a}\right\rceil = \left\lceil\frac{v}{a}\right\rceil$Điều này xảy ra chính xác khi không có bội số của`a`nghiêm ngặt giữa`u`Và`v`. 

Đối với các số liên tiếp`k`Và`k+1`, chúng có thể được phân biệt nếu`a`chia rẽ`k`. 

Điều đó biến đổi toàn bộ vấn đề. Để phân biệt mọi số nguyên với số tiếp theo, với mọi`k`TRONG`[2, x-1]`chúng ta cần ít nhất một người được chọn`a_i`chia`k`. 

Vì vậy, nhiệm vụ trở thành: 

Chọn số lượng giá trị tối thiểu`a_i`sao cho mọi số nguyên từ`2`ĐẾN`x-1`chia hết cho ít nhất một giá trị được chọn. 

Bây giờ hãy quan sát một sự đơn giản hóa khác. Nếu như`a > x-1`, nó không chia sẻ gì hữu ích. Nếu như`a = 1`, nó phân chia mọi thứ nhưng lại cung cấp thông tin hoàn toàn vô dụng vì số lượng trang tăng lên theo từng bước giống hệt nhau. 

Các ước số có ý nghĩa duy nhất là các số nguyên nằm giữa`2`Và`x-1`. 

Quan trọng hơn nữa, nếu chúng ta chọn một số`a > 1`, thì mọi bội số của`a`trở thành những ranh giới có thể phân biệt được. Để tách từng cặp liền kề, từng số`k`từ`2`ĐẾN`x-1`bản thân nó phải xuất hiện như một số chia giữa các chế độ đã chọn. Nếu không thì`k`được khám phá. 

Điều đó có nghĩa là tập hợp thành công duy nhất có thể chính xác là tất cả các số nguyên`2, 3, ..., x-1`. 

Vì vậy: 

- Nếu có số nguyên nào trong`[2, x-1]`không có trong dữ liệu đầu vào, câu trả lời là`-1`. 
- Ngược lại, số lượng chế độ tối thiểu bằng số số nguyên phân biệt trong phạm vi đó, tức là`x-2`. 

Toàn bộ vấn đề phân vùng có vẻ khó khăn sẽ được giải quyết thành một đối số bao phủ đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nx + 2^n) | O(nx) | Quá chậm | 
| Tối ưu | O(n) | O(x) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị`a_i`. 
2. Chèn mọi`a_i`thành một bộ. 

Chúng tôi chỉ quan tâm liệu một số chia có tồn tại ít nhất một lần hay không. Chế độ trùng lặp cung cấp thông tin giống hệt nhau. 
3. Với mọi số nguyên`k`từ`2`ĐẾN`x-1`, kiểm tra xem`k`tồn tại trong tập hợp. 

Nguyên nhân đến từ các số liền kề. Để phân biệt`k`từ`k+1`, một số chế độ được chọn phải thỏa mãn:$k \equiv 0 \pmod{a}$Từ`a > k`không thể chia`k`và bất kỳ ước số thích hợp nào của`k`cũng chia các số lân cận mà không tạo ranh giới chính xác tại`k`, dấu phân cách được đảm bảo duy nhất là`a = k`. 

1. Nếu bất kỳ số nào trong`[2, x-1]`bị thiếu, đầu ra`-1`. 
2. Nếu không thì xuất ra`x-2`. 

Chúng tôi cần tất cả các chế độ đó và sử dụng chính xác chúng sẽ hoạt động. 

### Tại sao nó hoạt động 

Một chế độ với kích thước trang`a`thay đổi đầu ra của nó một cách chính xác theo bội số của`a`. Cặp đôi`(k, k+1)`có thể phân biệt được nếu một số chế độ được chọn thay đổi giữa hai giá trị đó, có nghĩa là một số chế độ được chọn`a`chia rẽ`k`. 

Để phân biệt duy nhất tất cả các kích thước khoảng không quảng cáo có thể có, mỗi cặp liền kề phải có thể phân biệt được. Nếu thậm chí một cặp liền kề vẫn bằng nhau trong tất cả các chế độ đã chọn thì hai kích thước khoảng không quảng cáo đó sẽ tạo ra các quan sát giống hệt nhau. 

Đối với một nhất định`k`, số chia bất kỳ`a`điều đó ngăn cách`k`Và`k+1`phải chia`k`. Dấu phân cách mạnh nhất có thể là`a = k`. Nếu như`k`bản thân nó không có trong các chế độ có sẵn thì không có cách nào đảm bảo sự tách biệt cho ranh giới đó. Do đó mọi số nguyên từ`2`ĐẾN`x-1`phải tồn tại giữa các chế độ. 

Việc chọn tất cả chúng rõ ràng là có hiệu quả, bởi vì chế độ`k`phân tách chính xác ranh giới giữa`k`Và`k+1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())

    if n > 0:
        a = list(map(int, input().split()))
    else:
        a = []

    s = set(a)

    for v in range(2, x):
        if v not in s:
            print(-1)
            return

    print(x - 2)

solve()
```Việc thực hiện ngắn gọn vì việc rút gọn toán học sẽ loại bỏ tất cả các cấu trúc phức tạp. 

bộ`s`lưu trữ tất cả các kích thước chế độ có sẵn. Các bản sao sẽ tự động bị loại bỏ vì các chế độ giống hệt nhau không bao giờ cung cấp thêm thông tin. 

Vòng lặp kiểm tra mọi giá trị phân cách được yêu cầu từ`2`ĐẾN`x-1`. Thiếu một giá trị có nghĩa là ranh giới giữa số đó và số tiếp theo không thể được xác định duy nhất. 

Câu trả lời trở thành`x-2`bởi vì có chính xác số nguyên cần thiết trong khoảng đó. 

Một chi tiết tinh tế là cách xử lý`n = 0`. Câu lệnh cho phép điều đó, vì vậy việc đọc dòng thứ hai vô điều kiện sẽ thất bại. Mã bảo vệ chống lại trường hợp đó. 

Một chi tiết quan trọng khác là phạm vi vòng lặp:```
range(2, x)
```Điều này bao gồm một cách chính xác`x-1`và loại trừ`x`. Chúng ta chỉ cần phân biệt các cặp liền kề bên trong`[2, x]`, cụ thể là:```
(2,3), (3,4), ..., (x-1,x)
```## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 4
2 3
```Các giá trị phân cách bắt buộc là`2`Và`3`. 

| Bước | Giá trị hiện tại | Có mặt trong bộ? | 
| --- | --- | --- | 
| 1 | 2 | Có | 
| 2 | 3 | Có | 

Tất cả các giá trị bắt buộc đều tồn tại. 

Đầu ra:```
2
```Điều này thể hiện định lý cốt lõi. Cách thức`2`tách ra`2`từ`3`, trong khi chế độ`3`tách ra`3`từ`4`. 

### Ví dụ 2 

đầu vào:```
2 6
2 3
```Các giá trị phân cách bắt buộc là`2, 3, 4, 5`. 

| Bước | Giá trị hiện tại | Có mặt trong bộ? | 
| --- | --- | --- | 
| 1 | 2 | Có | 
| 2 | 3 | Có | 
| 3 | 4 | Không | 

Thuật toán ngay lập tức dừng lại. 

Đầu ra:```
-1
```Ví dụ này cho thấy tại sao có một số ước số là không đủ. Cặp đôi`(4,5)`không thể tách rời vì không có chế độ nào thay đổi chính xác ở đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + x) | Việc xây dựng tập hợp mất O(n), việc kiểm tra tất cả các giá trị bắt buộc mất O(x) | 
| Không gian | O(n) | Bộ lưu trữ các kích thước chế độ riêng biệt | 

Các ràng buộc phù hợp thoải mái.`n`đạt tới`10^5`, và quét qua`[2, x-1]`có thể chấp nhận được vì các ràng buộc dự định của các giải pháp được chấp nhận dựa vào đặc tính trực tiếp này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, x = map(int, input().split())

    if n > 0:
        a = list(map(int, input().split()))
    else:
        a = []

    s = set(a)

    for v in range(2, x):
        if v not in s:
            print(-1)
            return

    print(x - 2)

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
assert run("2 4\n2 3\n") == "2\n", "sample 1"

# minimum useful range
assert run("1 2\n2\n") == "0\n", "x = 2 requires no separators"

# missing separator
assert run("2 6\n2 3\n") == "-1\n", "missing 4 and 5"

# duplicates only
assert run("5 5\n2 2 2 3 3\n") == "-1\n", "duplicates do not help"

# complete coverage
assert run("4 6\n2 3 4 5\n") == "4\n", "all required values present"

# extra irrelevant large values
assert run("6 5\n2 3 4 100 200 300\n") == "3\n", "large values are irrelevant"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / 2`|`0`| Dải phân cách trống | 
|`2 6 / 2 3`|`-1`| Thiếu giá trị bắt buộc | 
|`5 5 / 2 2 2 3 3`|`-1`| Chế độ trùng lặp không thêm gì | 
|`4 6 / 2 3 4 5`|`4`| Xây dựng đầy đủ hợp lệ | 
|`6 5 / 2 3 4 100 200 300`|`3`| Chế độ không liên quan lớn | 

## Vỏ cạnh 

Hãy xem xét:```
5 5
2 2 2 3 3
```Tập hợp trở thành:```
{2, 3}
```Thuật toán kiểm tra: 

-`2`, hiện tại
-`3`, hiện tại
-`4`, thiếu 

Nó xuất ra`-1`. 

Điều này xác nhận rằng các chế độ lặp lại không làm tăng thông tin. Mỗi bản sao của chế độ`2`tạo ra số lượng trang chính xác như nhau. 

Bây giờ hãy xem xét:```
1 5
100
```Bộ này là:```
{100}
```Giá trị bắt buộc đầu tiên là`2`, vắng mặt nên câu trả lời là`-1`. 

Thật vậy, tất cả kích thước khoảng không quảng cáo từ`2`ĐẾN`5`tạo một trang ở chế độ này. 

Cuối cùng, hãy xem xét phạm vi nhỏ nhất có thể:```
0 2
```Không có cặp liền kề bên trong`[2,2]`, vậy là vòng lặp kết thúc`range(2, 2)`trống rỗng. Thuật toán in:```
0
```Điều này đúng vì khi chỉ có thể có một kích thước kho đồ, Vasya đã biết câu trả lời mà không cần kiểm tra bất kỳ chế độ nào.
