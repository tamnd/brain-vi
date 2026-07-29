---
title: "CF 102800G - Ma trận"
description: "Ma trận bắt đầu chứa đầy số không. Với mỗi cặp dương (i, j), chúng ta thực hiện một thao tác lật mọi ô có chỉ số hàng chia hết cho i và chỉ số cột chia hết cho j. Một ô cụ thể không quan tâm đến các hoạt động không tiếp cận được nó."
date: "2026-07-27T17:39:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "G"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 78
verified: true
draft: false
---

[CF 102800G - Ma trận](https://codeforces.com/problemset/problem/102800/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ma trận bắt đầu chứa đầy số không. Với mọi cặp dương`(i, j)`, chúng ta thực hiện một thao tác lật mọi ô có chỉ số hàng chia hết cho`i`và chỉ số cột của nó chia hết cho`j`. 

Một ô cụ thể không quan tâm đến các hoạt động không tiếp cận được nó. Đối với một ô ở hàng`r`và cột`c`, các hoạt động có liên quan duy nhất là những hoạt động ở đó`i`chia rẽ`r`Và`j`chia rẽ`c`. Mỗi cặp như vậy sẽ lật ô một lần. Sau khi mọi thao tác kết thúc, ô chứa`1`nếu nó đã được lật một số lần lẻ, nếu không nó vẫn giữ nguyên`0`. 

Mỗi trường hợp thử nghiệm đưa ra kích thước của một ma trận. Nhiệm vụ là đếm xem có bao nhiêu ô kết thúc`1`. 

Kích thước có thể lớn như`10^18`, vì vậy việc lặp lại tất cả các hàng là không thể. Bất kỳ thuật toán nào tỷ lệ thuận với`n`,`m`, hoặc`n × m`ngay lập tức bị loại trừ. Giải pháp chỉ được sử dụng một số phép tính số học cho mỗi trường hợp thử nghiệm. 

Một sai lầm dễ mắc phải là nghĩ rằng mọi ước số đều đóng góp độc lập và quên rằng câu trả lời chỉ phụ thuộc vào tính chẵn lẻ. 

Ví dụ:```
1
2 2
```Câu trả lời đúng là:```
1
```Chỉ ô`(1, 1)`trở thành`1`. Mặc dù các ô khác được lật nhiều lần nhưng mỗi ô đều được lật một số lần chẵn. 

Một trường hợp tinh tế khác là khi chỉ có một chỉ số là một hình vuông hoàn hảo.```
1
3 4
```Câu trả lời đúng là:```
2
```Hàng`1`là hình vuông hoàn hảo duy nhất, trong khi cột`1`Và`4`là những hình vuông hoàn hảo Chỉ một`(1,1)`Và`(1,4)`kết thúc như`1`. Một giải pháp bất cẩn chỉ kiểm tra một chiều sẽ bị tính quá mức. 

Trường hợp cạnh cuối cùng là đầu vào lớn nhất có thể.```
1
1000000000000000000 1000000000000000000
```Lời giải phải tránh mô phỏng và tính toán câu trả lời trực tiếp từ căn bậc hai số nguyên. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi hoạt động`(i, j)`, hãy truy cập từng ô bị ảnh hưởng và lật nó. Điều này đúng vì nó tuân theo định nghĩa chính xác. Thật không may, có`n × m`các cặp thao tác có thể xảy ra và mỗi thao tác có thể chạm vào nhiều ô. Ngay cả việc lặp qua tất cả các ô cũng không thể thực hiện được khi kích thước đạt tới`10^18`. 

Quan sát chính là phân tích một ô một cách độc lập. 

Giả sử chúng ta nhìn vào ô`(r, c)`. Nó được lật một lần cho mỗi số chia`i`của`r`và mọi ước số`j`của`c`. Nếu như`d(x)`biểu thị số ước của`x`, thì tổng số lần lật là`d(r) × d(c)`. 

Chỉ có sự ngang bằng mới quan trọng. Một tích số lẻ chỉ khi cả hai thừa số đều lẻ. 

Bây giờ chúng ta chỉ cần một thực tế lý thuyết số cổ điển. Số ước của một số nguyên là số lẻ chính xác khi số nguyên đó là một số chính phương. Mọi hình vuông đều có các ước số xuất hiện theo cặp`(a, x / a)`, trong khi một hình vuông hoàn hảo có một ước số không ghép đôi, đó là căn bậc hai của nó. 

Vì vậy, một tế bào trở thành`1`chính xác khi cả chỉ mục hàng và chỉ mục cột của nó đều là hình vuông hoàn hảo. 

Vấn đề giảm xuống việc đếm các hình vuông hoàn hảo trong mỗi chiều. Có chính xác`⌊√n⌋`hình vuông hoàn hảo giữa`1`Và`n`, và tương tự`⌊√m⌋`giữa`1`Và`m`. 

Câu trả lời cuối cùng chỉ đơn giản là`⌊√n⌋ × ⌊√m⌋`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × m × ô bị ảnh hưởng trung bình) | O(n×m) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Tính toán`a = ⌊√n⌋`bằng cách sử dụng hàm căn bậc hai số nguyên. Điều này đưa ra số lượng chỉ số hàng vuông hoàn hảo. 
3. Tính toán`b = ⌊√m⌋`bằng cách sử dụng hàm căn bậc hai số nguyên. Điều này đưa ra số lượng chỉ số cột vuông hoàn hảo. 
4. Đầu ra`a × b`, bởi vì mọi sự kết hợp của một hàng vuông hoàn hảo và một cột vuông hoàn hảo sẽ tạo ra một ô được lật một số lần lẻ. 

### Tại sao nó hoạt động 

Đối với ô cố định`(r, c)`, số lần lật bằng số cặp số chia`(i, j)`với`i | r`Và`j | c`, đó là`d(r) × d(c)`. Ô kết thúc như`1`nếu và chỉ nếu tích này là số lẻ. Số ước số chính xác là số lẻ đối với các bình phương hoàn hảo, vì vậy cả hai`r`Và`c`phải là những hình vuông hoàn hảo. Việc đếm các hàng và cột như vậy một cách độc lập sẽ cho ra chính xác số ô chứa`1`. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    print(isqrt(n) * isqrt(m))
```Chương trình trực tiếp thực hiện kết quả toán học.`math.isqrt`trả về căn bậc hai số nguyên, chính xác là`⌊√x⌋`không có bất kỳ lỗi dấu phẩy động nào. Điều này đặc biệt quan trọng vì đầu vào có thể lớn bằng`10^18`, trong đó căn bậc hai dấu phẩy động có thể mất độ chính xác. 

Mỗi trường hợp thử nghiệm chỉ thực hiện hai phép tính căn bậc hai số nguyên và một phép nhân. Số nguyên Python xử lý thoải mái câu trả lời lớn nhất có thể, do đó không có mối lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
2 3
```| Biến | Giá trị | 
| --- | --- | 
|`isqrt(2)`| 1 | 
|`isqrt(3)`| 1 | 
| Trả lời | 1 | 

Chỉ hàng`1`là một hình vuông hoàn hảo và chỉ có một cột`1`là một hình vuông hoàn hảo Giao điểm duy nhất của chúng là ô duy nhất bằng`1`. 

### Mẫu 2 

đầu vào:```
1
9 10
```| Biến | Giá trị | 
| --- | --- | 
|`isqrt(9)`| 3 | 
|`isqrt(10)`| 3 | 
| Trả lời | 9 | 

Các hàng vuông hoàn hảo là`1, 4, 9`. Các cột vuông hoàn hảo là`1, 4, 9`. Mỗi sự kết hợp của các hàng và cột này tạo thành một ô, tạo ra`3 × 3 = 9`. 

Ví dụ này minh họa rằng hai chiều hoàn toàn độc lập một khi đối số chẵn lẻ đã được thiết lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Hai căn bậc hai số nguyên và một phép nhân | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Thời gian chạy độc lập với kích thước ma trận nên dễ dàng xử lý các giá trị lên tới`10^18`trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import isqrt

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        out.append(str(isqrt(n) * isqrt(m)))
    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    ans = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return ans

# provided sample
assert run("1\n2 3\n") == "1"

# minimum size
assert run("1\n1 1\n") == "1"

# rectangular boundary
assert run("1\n3 4\n") == "2"

# no additional perfect square after 15
assert run("1\n15 15\n") == "9"

# maximum values
assert run("1\n1000000000000000000 1000000000000000000\n") == "1000000000000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Ma trận nhỏ nhất có thể | 
|`3 4`|`2`| Chỉ một chiều đóng góp nhiều hình vuông hoàn hảo | 
|`15 15`|`9`| Xử lý căn bậc hai tầng đúng | 
|`10^18 10^18`|`10^18`| Giá trị được phép lớn nhất | 

## Vỏ cạnh 

Hãy xem xét ma trận nhỏ nhất.```
1
1 1
```

`isqrt(1) = 1`cho cả hai chiều, vì vậy câu trả lời là`1 × 1 = 1`. Ô duy nhất được lật một lần vì`1`có đúng một ước số. 

Bây giờ hãy xem xét trường hợp chỉ có một chiều chứa nhiều hình vuông hoàn hảo.```
1
3 4
```Thuật toán tính toán`isqrt(3) = 1`Và`isqrt(4) = 2`, sản xuất`1 × 2 = 2`. Các tế bào`(1,1)`Và`(1,4)`là những người duy nhất có số lượt lật lẻ. 

Cuối cùng, hãy xem xét các giá trị pháp lý lớn nhất.```
1
1000000000000000000 1000000000000000000
```

`isqrt(10^18) = 10^9`, vậy câu trả lời là`10^9 × 10^9 = 10^18`. Thuật toán chỉ thực hiện số học theo thời gian không đổi và không bao giờ phụ thuộc vào kích thước ma trận thực tế.
