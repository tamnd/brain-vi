---
title: "CF 1886D - Monocarp và Bộ"
description: "Hãy suy nghĩ về quá trình ngược lại. Thay vì bắt đầu với một tập hợp trống và chèn các số, hãy bắt đầu với tập hợp đầy đủ {1,2,…,n} và liên tục xóa một số cho đến khi chỉ còn lại một số."
date: "2026-06-08T22:18:03+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 1886
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 156 (Rated for Div. 2)"
rating: 2100
weight: 1886
solve_time_s: 164
verified: true
draft: false
---

[CF 1886D - Monocarp và Bộ](https://codeforces.com/problemset/problem/1886/D) 

**Xếp hạng:** 2100 
**Tags:** tổ hợp, cấu trúc dữ liệu, toán học 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy suy nghĩ về quá trình ngược lại. 

Thay vì bắt đầu với tập hợp trống và chèn số, hãy bắt đầu với tập hợp đầy đủ`{1,2,…,n}`và liên tục xóa một số cho đến khi chỉ còn lại một số. 

Ký tự được viết sau`(i+1)`-th chèn tương ứng với số bị loại bỏ khi đảo ngược quá trình. 

Nếu một nhân vật là`>`, thì theo quy trình ngược lại, chúng ta phải loại bỏ mức tối đa hiện tại. 

Nếu một nhân vật là`<`, thì chúng ta phải loại bỏ mức tối thiểu hiện tại. 

Nếu một nhân vật là`?`, thì chúng ta không được loại bỏ cả giá trị tối thiểu lẫn giá trị tối đa. 

Nhiệm vụ là đếm xem có thể có bao nhiêu chuỗi loại bỏ, điều này hoàn toàn giống với việc đếm xem có bao nhiêu hoán vị chèn tạo ra chuỗi đã cho. 

Những hạn chế là thách thức thực sự. Cả hai`n`Và`m`lớn như`3·10^5`và chúng tôi phải đưa ra câu trả lời sau mỗi lần cập nhật. Bất kỳ giải pháp nào tính toán lại toàn bộ câu trả lời trong`O(n)`mỗi truy vấn sẽ thực hiện khoảng`9·10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Chúng ta cần thứ gì đó xung quanh`O(1)`hoặc`O(log n)`mỗi lần cập nhật. 

Một trường hợp cạnh tinh tế xuất hiện ngay lập tức. 

Vì`n = 2`, chỉ có hai số. Sau lần chèn đầu tiên, số thứ hai luôn là số tối thiểu mới hoặc số tối đa mới. MỘT`?`là không thể. 

Ví dụ:```
n = 2
s = ?
```Trả lời:```
0
```Một sai lầm dễ mắc phải khác là chỉ định sai phần đóng góp cho một`?`. 

Coi như:```
n = 4
s = <??
```đầu tiên`?`xảy ra ở vị trí 2. Tại thời điểm đó có chính xác một phần tử bên trong hợp lệ. thứ hai`?`xảy ra ở vị trí 3 và đóng góp hai lựa chọn. Câu trả lời là không`2·3`, đó là`1·2`. 

Bản thân vị trí không phải là yếu tố. Hệ số này nhỏ hơn vị trí một đơn vị. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ tạo ra mọi hoán vị của`1..n`, mô phỏng quá trình chèn, xây dựng chuỗi kết quả và đếm xem có bao nhiêu hoán vị khớp với chuỗi đích. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Thật không may là nó đòi hỏi`n!`hoán vị. Ngay cả đối với`n = 12`, điều này đã là vô vọng rồi, trong khi giới hạn thực sự là`3·10^5`. 

Quan sát chính xuất hiện sau khi đảo ngược quá trình. 

Giả sử kích thước đặt hiện tại là`k`. 

Nếu ký tự tiếp theo là`<`, chúng ta phải loại bỏ phần tử tối thiểu. Có chính xác một sự lựa chọn. 

Nếu ký tự tiếp theo là`>`, chúng ta phải loại bỏ phần tử tối đa. Một lần nữa chính xác là một sự lựa chọn. 

Nếu ký tự tiếp theo là`?`, chúng ta phải loại bỏ phần tử không tối thiểu cũng không tối đa. Bộ hiện tại chứa`k`các phần tử, do đó có`k-2`những lựa chọn hợp lệ. 

Không có gì xảy ra sau đó phụ thuộc vào yếu tố nội thất nào được chọn. Mọi lựa chọn hợp lệ đều dẫn đến số lần tiếp tục như nhau. Điều này có nghĩa là tổng số chuỗi hợp lệ chỉ đơn giản là tích của số lượng các lựa chọn ở mỗi`?`chức vụ. 

Đánh số các vị trí chuỗi ban đầu từ`1`. 

Khi chúng tôi xử lý vị trí`i`, kích thước cài đặt hiện tại trong quy trình ngược lại là`i+1`. 

Vì vậy một`?`ở vị trí`i`đóng góp```
(i + 1) - 2 = i - 1
```sự lựa chọn. 

Vị trí đầu tiên thật đặc biệt:```
i = 1  =>  i - 1 = 0
```MỘT`?`có nghĩa là không có chuỗi hợp lệ. 

Vậy câu trả lời là```
0                              if s[1] = '?'
Π (i - 1) over all '?' at i>1  otherwise
```Mỗi truy vấn chỉ thay đổi một ký tự. Chúng tôi có thể duy trì sản phẩm này một cách năng động. Khi một vị trí thay đổi thành`?`, nhân với sự đóng góp của nó. Khi một vị trí không còn tồn tại`?`, chia cho phần đóng góp của nó bằng cách sử dụng nghịch đảo mô-đun. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n! · n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n + m log MOD)`tiền xử lý,`O(1)`mỗi truy vấn |`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, và chuỗi`s`. 
2. Duy trì một biến`ans`. 
3. Đối với mọi vị trí`i > 1`, nếu như`s[i]`là`?`, nhân lên`ans`qua`(i-1)`modulo`998244353`. 
4. Duy trì một cờ riêng cho biết liệu`s[1]`là`?`. 
5. Câu trả lời hiện tại là: 

-`0`nếu như`s[1] == '?'`- nếu không thì`ans`. 
6. Tính toán trước nghịch đảo mô-đun của tất cả các giá trị từ`1`ĐẾN`n`. 
7. Đối với mỗi truy vấn: 

1. Loại bỏ sự đóng góp của nhân vật cũ. 
2. Thêm sự đóng góp của nhân vật mới. 
3. Cập nhật vị trí đầu tiên đặc biệt nếu cần thiết. 
4. Xuất ra câu trả lời hiện tại. 

### Tại sao nó hoạt động 

Trong quá trình ngược lại, kích thước thiết lập hiện tại tại vị trí`i`chính xác là`i+1`. 

nhân vật`<`Và`>`buộc loại bỏ một phần tử duy nhất, tối thiểu hoặc tối đa. Chúng góp phần tạo nên một yếu tố`1`. 

Một nhân vật`?`yêu cầu loại bỏ một phần tử bên trong. Giữa`i+1`chính xác là các phần tử`i-1`là nội thất. Mọi lựa chọn bên trong đều để lại cùng một loại trạng thái tương lai, do đó tổng số chuỗi hợp lệ nhân với`i-1`. 

Vì các lựa chọn ở các vị trí khác nhau là các hệ số nhân độc lập nên tổng số là tích của tất cả các đóng góp. Trường hợp bất khả thi duy nhất là`?`ở vị trí`1`, nơi không có phần tử bên trong nào cả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n, m = map(int, input().split())
s = list(input().strip())

inv = [1] * (n + 1)
for i in range(1, n + 1):
    inv[i] = pow(i, MOD - 2, MOD)

ans = 1

for i in range(1, n - 1):
    if s[i] == '?':
        ans = ans * i % MOD

def current_answer():
    if s[0] == '?':
        return 0
    return ans

out = [str(current_answer())]

for _ in range(m):
    pos, c = input().split()
    pos = int(pos) - 1

    if pos > 0:
        if s[pos] == '?' and c != '?':
            ans = ans * inv[pos] % MOD
        elif s[pos] != '?' and c == '?':
            ans = ans * pos % MOD

    s[pos] = c
    out.append(str(current_answer()))

sys.stdout.write("\n".join(out))
```Biến`ans`lưu trữ sản phẩm đóng góp từ tất cả`?`các vị trí ngoại trừ vị trí đầu tiên. 

Chuỗi được lưu trữ với chỉ mục dựa trên số không. Một ký tự ở chỉ mục`pos`tương ứng với vị trí`pos + 1`trong tuyên bố. Đóng góp của nó là`(pos + 1) - 1 = pos`. 

Đó là lý do tại sao các bản cập nhật sử dụng`pos`còn hơn là`pos + 1`. 

Ký tự đầu tiên yêu cầu xử lý riêng. Đóng góp của nó sẽ bằng 0, vì vậy nó không thể được biểu diễn bên trong tích nhân. Chúng tôi chỉ đơn giản là xuất ra số 0 bất cứ khi nào`s[0] == '?'`. 

Loại bỏ một đóng góp yêu cầu chia theo modulo`998244353`. Vì môđun là số nguyên tố nên định lý Fermat cho```
x^(-1) = x^(MOD-2) mod MOD
```cho phép cập nhật theo thời gian liên tục sau khi tính toán nghịch đảo trước. 

## Ví dụ đã hoạt động 

### Mẫu 1```
n = 6
s = <?>?>
```| Vị trí | Nhân vật | Đóng góp | 
| --- | --- | --- | 
| 1 | < | 1 | 
| 2 | ? | 1 | 
| 3 | > | 1 | 
| 4 | ? | 3 | 
| 5 | > | 1 | 

Sản phẩm:```
1 × 3 = 3
```Trả lời:```
3
```Ví dụ này cho thấy công thức trung tâm. Hai dấu chấm hỏi góp phần`1`Và`3`, đưa ra số đếm cuối cùng`3`. 

### Mẫu 2```
n = 2
s = >
```| Vị trí | Nhân vật | Kết quả | 
| --- | --- | --- | 
| 1 | > | hợp lệ | 

Trả lời:```
1
```Sau khi cập nhật lên:```
s = ?
```vị trí đầu tiên trở thành`?`, vì vậy câu trả lời là ngay lập tức:```
0
```Điều này thể hiện cách xử lý đặc biệt của ký tự đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Một lần quét đầu tiên, sau đó làm việc liên tục cho mỗi truy vấn | 
| Không gian |`O(n)`| Chuỗi và mảng nghịch đảo | 

Giải pháp này chỉ thực hiện một số phép nhân mô-đun cho mỗi bản cập nhật, dễ dàng phù hợp với giới hạn cho`3·10^5`truy vấn. 

## Trường hợp thử nghiệm```
# helper skeleton

# sample 1
assert run(
"""6 4
<?>?>
1 ?
4 <
5 <
1 >
"""
) == """3
0
0
0
1"""

# sample 2
assert run(
"""2 2
>
1 ?
1 <
"""
) == """1
0
1"""

# minimum size, impossible
assert run(
"""2 1
?
1 >
"""
) == """0
1"""

# all deterministic
assert run(
"""5 1
<<<<
2 >
"""
) == """1
1"""

# single contributing question mark
assert run(
"""4 1
<?<
2 >
"""
) == """1
1"""

# first position toggles
assert run(
"""3 2
<?
1 ?
1 >
"""
) == """1
0
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2, s=?`|`0`| Dấu hỏi vị trí đầu tiên không thể | 
|`<<<<`|`1`| Không có đóng góp nhân lên | 
|`<?<`|`1`| Sự đóng góp`(2-1)=1`| 
| Cập nhật vị trí đầu tiên |`0/1`chuyển tiếp | Xử lý trường hợp đặc biệt | 

## Vỏ cạnh 

Hãy xem xét:```
n = 2
s = ?
```Tại thời điểm được biểu thị bằng ký tự đầu tiên, tập hợp chứa chính xác hai phần tử. Bất kỳ phần tử nào được chọn đều là mức tối thiểu hoặc tối đa. Không có phần tử bên trong nên số lựa chọn hợp lệ bằng 0. Thuật toán phát hiện điều này ngay lập tức vì`s[1]`là`?`và đầu ra`0`. 

Coi như:```
n = 4
s = <??
```đầu tiên`?`đang ở vị trí`2`, đóng góp`2-1 = 1`. 

thứ hai`?`đang ở vị trí`3`, đóng góp`3-1 = 2`. 

Câu trả lời là:```
1 × 2 = 2
```Một lỗi phổ biến là nhân với chính vị trí đó và thu được`2 × 3 = 6`. Việc giải thích quy trình ngược lại cho thấy rõ rằng số phần tử bên trong luôn luôn là`set_size - 2`, bằng`i - 1`, không`i`.
