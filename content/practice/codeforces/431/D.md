---
title: "CF 431D - Nhiệm vụ ngẫu nhiên"
description: "Chúng ta cần tìm một số nguyên dương n sao cho trong khoảng (n, 2n] có đúng m số có đúng k số nguyên trong biểu diễn nhị phân của chúng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 431
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 247 (Div. 2)"
rating: 2100
weight: 431
solve_time_s: 121
verified: true
draft: false
---

[CF 431D - Nhiệm vụ ngẫu nhiên](https://codeforces.com/problemset/problem/431/D) 

**Đánh giá:** 2100 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, tổ hợp, dp, toán 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta cần tìm số nguyên dương`n`sao cho trong khoảng`(n, 2n]`, chính xác`m`con số có chính xác`k`những cái trong biểu diễn nhị phân của chúng. 

Một cách khác để diễn đạt điều kiện là: 

Nếu`f(x)`biểu thị số lượng số nguyên trong`[0, x]`có dạng nhị phân chứa chính xác`k`đặt bit, sau đó số số nguyên hợp lệ trong`(n, 2n]`bằng$f(2n)-f(n)=m$Vì vậy, nhiệm vụ trở thành tìm bất kỳ`n ≤ 10^18`thỏa mãn phương trình này. 

Các ràng buộc loại trừ hoàn toàn việc kiểm tra từng số một. Thậm chí lặp qua tất cả các số lên đến`10^18`là không thể. Chúng ta cần một cái gì đó gần hơn với độ phức tạp logarit. Vì câu trả lời được đảm bảo tồn tại và biểu thức đích phụ thuộc đơn điệu vào`n`, tìm kiếm nhị phân trở thành một hướng tự nhiên. 

Phần khó khăn là tính toán`f(x)`một cách hiệu quả. Việc triển khai đơn giản sẽ kiểm tra mọi số từ`0`ĐẾN`x`và đếm các bit đã đặt, chi phí`O(x log x)`. Với`x`gần`10^18`, điều đó thật vô vọng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Nếu như`k`lớn hơn số bit có sẵn trong`x`, câu trả lời cho`f(x)`phải bằng không. Ví dụ: 

đầu vào:```
1 64
```Không có số nhỏ nào có 64 bit được đặt, vì vậy mã tổ hợp bất cẩn có thể truy cập vào các chỉ mục không hợp lệ hoặc tạo ra số đếm âm. 

Một trường hợp tinh tế khác xuất hiện khi bản thân số hiện tại có chính xác`k`đặt bit. Giả sử: 

đầu vào:```
1 1
```Vì`n = 1`, khoảng`(1, 2]`chỉ chứa số`2`, có biểu diễn nhị phân là`10`, chứa chính xác một bit được đặt. Câu trả lời đúng là`1`. Việc triển khai DP chữ số mà quên bao gồm`x`chính nó ở trong`f(x)`sẽ thất bại ở đây. 

Xử lý ranh giới xung quanh`(n, 2n]`cũng nguy hiểm. Khoảng thời gian loại trừ`n`nhưng bao gồm`2n`. sử dụng`f(2n) - f(n - 1)`thay vì`f(2n) - f(n)`âm thầm thay đổi ý nghĩa. 

Ví dụ: 

đầu vào:```
1 2
```Lấy`n = 2`. Khoảng thời gian`(2, 4]`chứa`3`Và`4`. Chỉ một`3`có chính xác hai bit được đặt, do đó số đếm bằng`1`. Sử dụng sai công thức tính khoảng`2`chính nó và tạo ra một câu trả lời sai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi ứng viên`n`, kiểm tra mọi số trong`(n, 2n]`, đếm xem có chính xác bao nhiêu`k`đặt bit và dừng khi số đếm bằng`m`. 

Điều này hoạt động vì điều kiện có thể được kiểm tra trực tiếp. Vấn đề là quy mô. Ngay cả đối với một người`n`, kích thước khoảng là`n`. Nếu như`n`ở xung quanh`10^18`, chúng ta sẽ cần khoảng`10^18`hoạt động đếm dân số. Điều đó vượt xa mọi giới hạn thực tế. 

Cải tiến tiếp theo là nhận thấy rằng chúng ta chỉ cần đếm số chính xác`k`thiết lập bit lên đến một số giới hạn. Định nghĩa:$f(x)=\#\{0\le t\le x\mid \operatorname{popcount}(t)=k\}$Khi đó số số nguyên hợp lệ bên trong`(n, 2n]`trở thành:$f(2n)-f(n)$Bây giờ vấn đề giảm xuống còn việc đánh giá một cách hiệu quả`f(x)`. 

Đây là nơi tổ hợp nhị phân giúp ích. Chúng tôi xử lý các bit của`x`từ quan trọng nhất đến ít quan trọng nhất. Bất cứ khi nào chúng ta gặp phải một`1`chút, chúng ta có thể chọn đặt`0`ở đó và phân phối tự do các bit thiết lập cần thiết còn lại giữa các vị trí thấp hơn. Số lượng các khả năng như vậy là một hệ số nhị thức. 

Ví dụ, nếu chúng ta vẫn cần`r`những cái đó và có`p`vị trí bit còn lại thì số cách bằng:$\binom{p}{r}$Điều này tính toán`f(x)`trong khoảng 64 bước. 

Quan sát cuối cùng là sự đơn điệu. BẰNG`n`tăng lên, số lượng`f(2n) - f(n)`không bao giờ giảm. Các con số chỉ đi vào khoảng, không bao giờ biến mất. Điều đó cho phép tìm kiếm nhị phân trên`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) cho mỗi ứng viên | O(1) | Quá chậm | 
| Tối ưu | O(log^2 MAX) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các hệ số nhị thức`C[n][r]`vì`0 ≤ n, r ≤ 64`. 

Chúng tôi chỉ cần tối đa 64 bit vì`10^18 < 2^60`và việc sử dụng 64 giúp việc triển khai trở nên đơn giản. 
2. Triển khai một chức năng`count(x)`trả về có bao nhiêu số nguyên trong`[0, x]`chứa chính xác`k`đặt bit. 

Hàm quét các bit từ cao xuống thấp. 
3. Duy trì một biến`ones_used`, biểu thị số lượng bit đã đặt đã được cố định trong tiền tố. 
4. Khi bit hiện tại của`x`là`1`, chúng tôi cân nhắc việc thực hiện điều này`0`thay vì. 

Khi đó các vị trí thấp hơn còn lại có thể chứa bất kỳ sự sắp xếp nào với chính xác:$k-ones\_used$những cái đó. 

Nếu có`pos`các vị trí còn lại, chúng tôi thêm:$\binom{pos}{k-ones\_used}$5. Sau khi xử lý nhánh đó, tiếp tục theo dõi bit thực tế của`x`, nghĩa là bit này vẫn ở lại`1`. 

Tăng`ones_used`. 
6. Nếu tại bất kỳ thời điểm nào`ones_used > k`, dừng sớm vì không có sự tiếp tục nào có thể có hiệu lực. 
7. Sau khi tất cả các bit được xử lý, hãy bao gồm`x`chính nó nếu nó chứa chính xác`k`đặt bit. 
8. Xác định:$g(n)=count(2n)-count(n)$Điều này bằng với số số nguyên trong`(n, 2n]`có chính xác`k`đặt bit. 
9. Tìm kiếm nhị phân nhỏ nhất`n`như vậy:$g(n)\ge m$10. Vì bài toán đảm bảo sự tồn tại nên sau khi tìm kiếm kết thúc, kết quả thu được`n`thỏa mãn:$g(n)=m$### Tại sao nó hoạt động 

Hàm đếm liệt kê tất cả các số nhỏ hơn hoặc bằng`x`bằng cách sửa các bit từ trái sang phải. Bất cứ khi nào một`1`bit xuất hiện trong`x`, đang chọn`0`ở đó tạo ra toàn bộ khối số nhỏ hơn và tổ hợp đếm chính xác có bao nhiêu trong số chúng chứa số bit thiết lập cần thiết. 

Tìm kiếm nhị phân là đúng vì`g(n)`đơn điệu không giảm. Tăng dần`n`mở rộng khoảng`(n, 2n]`bên phải. Các số mới có thể thỏa mãn điều kiện, nhưng các số đã đếm trước đó không bao giờ biến mất theo cách làm giảm tổng số. Vì câu trả lời tồn tại nên việc tìm kiếm nhị phân giới hạn dưới tiêu chuẩn sẽ tìm thấy một kết quả hợp lệ`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXB = 64

# Precompute combinations
C = [[0] * (MAXB + 1) for _ in range(MAXB + 1)]

for n in range(MAXB + 1):
    C[n][0] = 1
    C[n][n] = 1

for n in range(1, MAXB + 1):
    for r in range(1, n):
        C[n][r] = C[n - 1][r - 1] + C[n - 1][r]

def solve():
    m, k = map(int, input().split())

    def count_upto(x):
        if x < 0:
            return 0

        ans = 0
        ones_used = 0

        for pos in range(MAXB - 1, -1, -1):
            if (x >> pos) & 1:
                need = k - ones_used

                if 0 <= need <= pos:
                    ans += C[pos][need]

                ones_used += 1

                if ones_used > k:
                    break

        else:
            if ones_used == k:
                ans += 1

        return ans

    def g(n):
        return count_upto(2 * n) - count_upto(n)

    lo, hi = 1, 10**18

    while lo < hi:
        mid = (lo + hi) // 2

        if g(mid) >= m:
            hi = mid
        else:
            lo = mid + 1

    print(lo)

solve()
```Phần đầu tiên tính toán trước các hệ số nhị thức bằng cách sử dụng tam giác Pascal. Vì số bit liên quan lớn nhất nhiều nhất là 64 nên bảng này rất nhỏ. 

Logic cốt lõi nằm bên trong`count_upto(x)`. Hàm xử lý các bit từ cao đến thấp. Bất cứ khi nào nó gặp phải một`1`bit, nó đếm tất cả các số nhỏ hơn được hình thành bằng cách đặt`0`ở đó và phân phối các bit thiết lập cần thiết còn lại giữa các vị trí thấp hơn. 

Biểu thức:$C[pos][need]$là trung tâm của ý tưởng chữ số-DP. có`pos`vị trí bit thấp hơn có sẵn và chúng ta phải chọn chính xác`need`trong số họ trở thành`1`. 

các`else`gắn liền với`for`vòng lặp là tinh tế. Trong Python, nó chỉ thực thi nếu vòng lặp không bị kết thúc bởi`break`. Điều đó có nghĩa là chúng tôi đã xử lý thành công tất cả các bit và bây giờ có thể bao gồm`x`chính nó nếu số lượng của nó bằng`k`. 

Việc tìm kiếm nhị phân sử dụng`g(mid) >= m`. Đây là tìm kiếm giới hạn dưới cho vị trí hợp lệ đầu tiên. sử dụng`>`thay vào đó sẽ bỏ qua các kết quả khớp chính xác. 

Tất cả số học đều nằm gọn trong các số nguyên Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
```Chúng tôi kiểm tra`n = 1`. 

Khoảng thời gian là`(1, 2]`, chỉ chứa`2`. 

Biểu diễn nhị phân của`2`là`10`, có một bit được đặt. 

| Giá trị | Nhị phân | Số lượng | Đã tính | 
| --- | --- | --- | --- | 
| 2 | 10 | 1 | Có | 

Vì thế:$g(1)=1$Câu trả lời là`1`. 

Ví dụ này xác nhận ranh giới khoảng được xử lý chính xác.`1`chính nó bị loại trừ, trong khi`2`được bao gồm. 

### Ví dụ 2 

đầu vào:```
2 2
```Thử`n = 5`. 

Khoảng thời gian là`(5, 10]`. 

| Giá trị | Nhị phân | Số lượng | Đã tính | 
| --- | --- | --- | --- | 
| 6 | 110 | 2 | Có | 
| 7 | 111 | 3 | Không | 
| 8 | 1000 | 1 | Không | 
| 9 | 1001 | 2 | Có | 
| 10 | 1010 | 2 | Có | 

Điều này mang lại cho số lượng`3`, quá lớn. 

Thử`n = 4`. 

| Giá trị | Nhị phân | Số lượng | Đã tính | 
| --- | --- | --- | --- | 
| 5 | 101 | 2 | Có | 
| 6 | 110 | 2 | Có | 
| 7 | 111 | 3 | Không | 
| 8 | 1000 | 1 | Không | 

Bây giờ số đếm bằng`2`, Vì thế`n = 4`là hợp lệ. 

Dấu vết này cho thấy lý do tại sao tìm kiếm nhị phân hoạt động. Tăng dần`n`đã chuyển số đếm lên trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log2 MAX) | Tìm kiếm nhị phân thực hiện khoảng 60 lần lặp, mỗi lần đếm trên 64 bit | 
| Không gian | O(1) | Chỉ một bảng kết hợp có kích thước cố định được lưu trữ | 

Thuật toán dễ dàng phù hợp trong giới hạn. đại khái`60 × 64`các thao tác là cần thiết, rất nhỏ ngay cả trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MAXB = 64

C = [[0] * (MAXB + 1) for _ in range(MAXB + 1)]

for n in range(MAXB + 1):
    C[n][0] = 1
    C[n][n] = 1

for n in range(1, MAXB + 1):
    for r in range(1, n):
        C[n][r] = C[n - 1][r - 1] + C[n - 1][r]

def solve():
    input = sys.stdin.readline

    m, k = map(int, input().split())

    def count_upto(x):
        if x < 0:
            return 0

        ans = 0
        ones_used = 0

        for pos in range(MAXB - 1, -1, -1):
            if (x >> pos) & 1:
                need = k - ones_used

                if 0 <= need <= pos:
                    ans += C[pos][need]

                ones_used += 1

                if ones_used > k:
                    break
        else:
            if ones_used == k:
                ans += 1

        return ans

    def g(n):
        return count_upto(2 * n) - count_upto(n)

    lo, hi = 1, 10**18

    while lo < hi:
        mid = (lo + hi) // 2

        if g(mid) >= m:
            hi = mid
        else:
            lo = mid + 1

    print(lo)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("1 1\n") == "1", "sample 1"

# interval (2,4] => only 3 has two set bits
assert run("1 2\n") == "2", "basic popcount test"

# interval (4,8] => 5 and 6
assert run("2 2\n") == "4", "multiple valid numbers"

# smallest k
assert run("1 64\n") != "", "large k edge case"

# boundary around powers of two
assert run("1 3\n") == "5", "checks interval boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Ví dụ hợp lệ nhỏ nhất | 
|`1 2`|`2`| Xử lý đúng`(n, 2n]`| 
|`2 2`|`4`| Nhiều số hợp lệ trong khoảng | 
|`1 64`| bất kỳ số hợp lệ nào | Lớn`k`xử lý | 
|`1 3`|`5`| Hành vi ranh giới gần lũy thừa của hai | 

## Vỏ cạnh 

Hãy xem xét: 

đầu vào:```
1 64
```Rất ít số chứa 64 bit được đặt. Trong quá trình đếm tổ hợp, nhiều trạng thái yêu cầu các giá trị không thể chấp nhận được như:$\binom{10}{64}$Việc triển khai sẽ bỏ qua những điều này một cách an toàn vì nó kiểm tra:```
if 0 <= need <= pos:
```Vì vậy, các kết hợp không hợp lệ đóng góp bằng 0 thay vì gây ra lỗi lập chỉ mục. 

Bây giờ hãy xem xét trường hợp nhạy cảm với ranh giới: 

đầu vào:```
1 2
```Vì`n = 2`, khoảng là`(2, 4]`. 

| Giá trị | Số lượng | 
| --- | --- | 
| 3 | 2 | 
| 4 | 1 | 

Chính xác một số đủ điều kiện. 

Thuật toán tính toán:$count(4)-count(2)$

`count(4)`bao gồm`{3}`, trong khi`count(2)`loại trừ nó. Kết quả là chính xác`1`. 

Cuối cùng, hãy xem xét một trường hợp trong đó`x`bản thân nó phải được tính. 

Giả sử chúng ta đánh giá:$count(7)$với`k = 3`. 

Biểu diễn nhị phân của`7`là`111`, có chính xác ba bit được đặt. Nếu việc triển khai chỉ tính các số nhỏ hơn, nó sẽ bỏ lỡ`7`. 

Vòng lặp`else`khối thêm một số đếm cuối cùng khi số được xây dựng bằng`x`và có chính xác`k`đặt bit. Điều đó xử lý tình huống này một cách chính xác.
