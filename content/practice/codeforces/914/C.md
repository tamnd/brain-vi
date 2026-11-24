---
title: "CF 914C - Nhân viên bán hàng du lịch và những con số đặc biệt"
description: "Chúng ta được cung cấp giới hạn trên n, nhưng n không được cung cấp dưới dạng số nguyên thập phân. Thay vào đó, nó được đưa ra trực tiếp dưới dạng một chuỗi nhị phân có độ dài có thể lên tới 1000 bit."
date: "2026-06-13T01:23:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 914
codeforces_index: "C"
codeforces_contest_name: "Codecraft-18 and Codeforces Round 458 (Div. 1 + Div. 2, combined)"
rating: 1800
weight: 914
solve_time_s: 240
verified: true
draft: false
---

[CF 914C - Nhân viên bán hàng du lịch và các số đặc biệt](https://codeforces.com/problemset/problem/914/C) 

**Đánh giá:** 1800 
**Tags:** vũ lực, tổ hợp, dp 
**Thời gian giải:** 4 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một giới hạn trên`n`, Nhưng`n`không được cung cấp dưới dạng số nguyên thập phân. Thay vào đó, nó được đưa ra trực tiếp dưới dạng một chuỗi nhị phân có độ dài có thể lên tới 1000 bit. Chúng ta xét phép toán sau trên số nguyên dương: 

Lấy số đó và thay thế bằng số của`1`các bit trong biểu diễn nhị phân của nó. 

Ví dụ:`13 → 3 → 2 → 1`bởi vì`13 = 1101₂`có ba bit được đặt,`3 = 11₂`có hai bit được đặt và`2 = 10₂`có một bit được đặt. 

Một số được gọi là đặc biệt nếu số lượng thao tác tối thiểu cần thiết để đạt được`1`bằng`k`. 

Nhiệm vụ là đếm có bao nhiêu số nguyên dương`x`thỏa mãn`1 ≤ x ≤ n`và yêu cầu chính xác`k`hoạt động để đạt được`1`. 

Phần bất thường của các ràng buộc là`n`có thể chứa tới 1000 chữ số nhị phân. Điều đó có nghĩa là giá trị số của`n`có thể ở xung quanh`2^1000`, vượt xa những gì có thể liệt kê được. Bất kỳ giải pháp nào lặp qua các số lên tới`n`ngay lập tức là không thể. 

Giới hạn độ dài 1000 bit cho thấy các thuật toán tỷ lệ thuận với độ dài bit có thể chấp nhận được. Lập trình động trên biểu diễn nhị phân, thường được gọi là chữ số DP, trở thành một ứng cử viên đương nhiên. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1
0
```Con số duy nhất không vượt quá`1`là`1`chính nó. Từ`1`đã bằng rồi`1`, nó không cần thao tác nào. Câu trả lời đúng là`1`. Một giải pháp áp dụng một cách mù quáng phép truy toán chung cho số lượng phép toán dương sẽ bỏ sót trường hợp này. 

Coi như:```
1
1
```Không có số`≤ 1`yêu cầu đúng một thao tác. Con số`1`không yêu cầu thao tác nào. Câu trả lời đúng là`0`. 

Một trường hợp tế nhị khác là:```
10
1
```Đây`n = 2`. Con số đặc biệt duy nhất là`2`, bởi vì`2 → 1`trong một bước. Con số`1`không được tính. Nhiều triển khai đếm tất cả các số có một bit được đặt và vô tình bao gồm`1`. 

Một cái bẫy cuối cùng là số lượng bit được đặt có thể bằng 0 trong DP. giá trị`0`không phải là số nguyên dương và không bao giờ đóng góp vào câu trả lời. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Với mọi số nguyên`x`từ`1`ĐẾN`n`, liên tục thay thế nó bằng số lượng của nó cho đến khi đạt`1`, đếm xem cần bao nhiêu bước và kiểm tra xem số bước đó có bằng không`k`. 

Chuỗi hoạt động ngắn vì số lượng dân số giảm xuống số lượng lớn rất nhanh. Đối với các số bên dưới`2^1000`, một ứng dụng tạo ra nhiều nhất một giá trị`1000`và sau đó các giá trị trở nên nhỏ bé. Vấn đề không phải là độ dài chuỗi mà là số lượng giá trị bắt đầu. Khi`n`bản thân nó có thể gần`2^1000`, việc lặp lại tất cả các ứng cử viên là vô vọng. 

Quan sát quan trọng là sau thao tác đầu tiên, hành vi trong tương lai chỉ phụ thuộc vào số lượng bit đã đặt của số ban đầu. 

Giả sử một số`x`chứa chính xác`s`những cái trong biểu diễn nhị phân của nó. 

Sau một thao tác:```
x → s
```Từ thời điểm đó trở đi, quá trình này hoàn toàn được xác định bởi`s`. 

Định nghĩa:```
f(v) = minimum operations needed to reduce v to 1
```cho số nguyên thông thường`v`. 

Nếu như`x`có`s`thiết lập bit và`x > 1`, sau đó:```
f(x) = 1 + f(s)
```Vì vậy, thay vì suy luận về những con số khổng lồ, chúng ta chỉ cần biết chúng chứa bao nhiêu bit tập hợp. Vì độ dài nhị phân tối đa là 1000 nên số lượng bit được đặt cũng tối đa là 1000. 

Điều này biến vấn đề thành hai phần riêng biệt. 

Đầu tiên, tính toán trước`f(s)`cho tất cả`s ≤ 1000`. 

Thứ hai, với mọi số lượng bit thiết lập có thể`s`, đếm xem có bao nhiêu số`x ≤ n`chứa chính xác`s`những cái đó. Đây là bài toán DP-chữ số tổ hợp cổ điển trên chuỗi nhị phân. 

Những con số góp phần vào câu trả lời chính xác là những con số có số bit thiết lập`s`thỏa mãn:```
1 + f(s) = k
```ngoại trừ việc xử lý đặc biệt`x = 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(1) | Quá chậm | 
| Tối ưu | O(L²) | O(L²) | Đã chấp nhận | 

Đây`L`là độ dài nhị phân của`n`, nhiều nhất`1000`. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi nhị phân`n`và để`L`là chiều dài của nó. 
2. Xử lý trường hợp đặc biệt`k = 0`. 

Số duy nhất không yêu cầu phép toán bằng 0 là`1`chính nó. Nếu như`1 ≤ n`, điều này luôn đúng vì`n`là tích cực, câu trả lời là`1`. 
3. Tính toán trước số lượng thao tác cho tất cả các giá trị từ`1`ĐẾN`1000`. 

Cho phép:```
steps[1] = 0
steps[i] = 1 + steps[popcount(i)]
```vì`i > 1`. 

Từ`popcount(i) < i`cho mọi`i > 1`, các giá trị có thể được tính theo thứ tự tăng dần. 
4. Xác định tất cả số lượng bit set`s`thỏa mãn:```
1 + steps[s] = k
```Bất kỳ số nào có chính xác`s`người ta sẽ yêu cầu chính xác`k`hoạt động, với điều kiện là số đó lớn hơn`1`. 
5. Tính toán trước hệ số nhị thức`C(n,r)`modulo`10^9+7`cho tất cả`0 ≤ n,r ≤ 1000`. 

Các hệ số này cho phép chúng ta đếm xem có bao nhiêu chuỗi nhị phân có độ dài còn lại nhất định chứa một số chuỗi nhị phân nhất định. 
6. Đối với mỗi số bit thiết lập hợp lệ`s`, đếm xem có bao nhiêu số`x ≤ n`có chính xác`s`những cái đó. 

Quét các bit của`n`từ trái sang phải. 

Bất cứ khi nào một vị trí chứa`1`, chúng ta có thể đặt`0`ở đó và làm cho số kết quả nhỏ hơn hoàn toàn`n`. Các vị trí còn lại sau đó có thể được điền tùy ý. 

Nếu chúng ta đã sử dụng rồi`used`những cái cho đến nay và có`rem`vị trí bên trái, sau đó chọn`0`ở vị trí hiện tại đóng góp:```
C(rem, s - used)
```bất cứ khi nào`s - used`là khả thi. 

Sau đó tiếp tục quét giả sử chúng tôi khớp`n`và địa điểm`1`, tăng dần`used`. 
7. Sau khi quá trình quét kết thúc, nếu số`n`bản thân nó chứa chính xác`s`những cái, thêm một số nữa. 
8. Tính tổng các số đếm hợp lệ`s`. 
9. Khi nào`k = 1`, trừ đi một từ câu trả lời. 

Lý do là vậy`s = 1`thỏa mãn:```
1 + steps[1] = 1
```và thủ tục đếm bao gồm số`1`. Nhưng`1`thực sự không yêu cầu thao tác nào, không phải một thao tác. 
10. Xuất kết quả theo modulo`10^9+7`. 

### Tại sao nó hoạt động 

Với mọi số nguyên dương`x > 1`, thao tác đầu tiên sẽ gửi nó tới`popcount(x)`. Sau thời điểm đó, chuỗi tương lai chỉ phụ thuộc vào giá trị popcount đó. Nếu như`x`chứa`s`những cái thì tổng số thao tác bằng`1 + steps[s]`. 

Thuật toán liệt kê tất cả các giá trị có thể có của`s`điều đó dẫn đến chính xác`k`hoạt động. Đối với mỗi như vậy`s`, thủ tục đếm chữ số-DP đếm mọi số`≤ n`với chính xác`s`đặt bit chính xác một lần. Các chuyển đổi tổ hợp tương ứng với vị trí đầu tiên mà số được xây dựng trở nên nhỏ hơn`n`. 

Vì mỗi số hợp lệ có số bit thiết lập duy nhất và mỗi số được đếm đều đáp ứng số phép toán được yêu cầu nên tổng cuối cùng chính xác là câu trả lời mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

n = input().strip()
k = int(input())

L = len(n)

if k == 0:
    print(1)
    sys.exit()

# steps[i] = operations needed for i -> 1
steps = [0] * (L + 1)
steps[1] = 0

for i in range(2, L + 1):
    steps[i] = steps[i.bit_count()] + 1

# binomial coefficients
C = [[0] * (L + 1) for _ in range(L + 1)]
for i in range(L + 1):
    C[i][0] = C[i][i] = 1
    for j in range(1, i):
        C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD

def count_with_ones(target):
    used = 0
    res = 0

    for i, ch in enumerate(n):
        if ch == '1':
            rem = L - i - 1
            need = target - used

            if 0 <= need <= rem:
                res = (res + C[rem][need]) % MOD

            used += 1

    if used == target:
        res = (res + 1) % MOD

    return res

ans = 0

for ones in range(1, L + 1):
    if 1 + steps[ones] == k:
        ans = (ans + count_with_ones(ones)) % MOD

if k == 1:
    ans = (ans - 1) % MOD

print(ans % MOD)
```các`steps`mảng lưu trữ số lượng phép tính cần thiết cho các số nguyên nhỏ lên tới 1000. Điều này có hiệu quả vì sau một phép toán đếm số, mọi số liên quan tối đa sẽ trở thành độ dài bit của`n`. 

Bảng nhị thức hỗ trợ giai đoạn đếm. Khi quá trình quét gặp phải một`1`chút vào`n`, chúng ta có thể chọn chữ số nhỏ hơn`0`và phân phối miễn phí những thứ cần thiết còn lại cho các vị trí còn lại. Số lượng phân phối như vậy chính xác là một hệ số nhị thức. 

chức năng`count_with_ones(target)`thực hiện việc đếm chữ số-DP. Thay vì lưu trữ các trạng thái trong bảng DP, nó sử dụng thủ thuật đếm tổ hợp tiêu chuẩn cho các giới hạn trên nhị phân. 

Phép trừ cho`k = 1`là phần tế nhị nhất của việc thực hiện. Các số có một bit được đặt thỏa mãn điều kiện`1 + steps[1] = 1`, nhưng số`1`bản thân nó không nên thuộc về loại đó bởi vì nó đã bằng rồi`1`trước bất kỳ hoạt động nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
110
2
```Đây`n = 6`. 

Số lượng bit thiết lập hợp lệ thỏa mãn:```
1 + steps[s] = 2
```Chỉ một`s = 2`hoạt động vì:```
2 -> 1
```trong một bước. 

Đếm các số ≤ 110₂ có đúng hai số: 

| Vị trí | Bit của n | được sử dụng trước đây | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | C(2,2)=1 | 
| 1 | 1 | 1 | C(1,1)=1 | 
| 2 | 0 | 2 | 0 | 

Bản thân số cuối cùng cũng có hai số một nên hãy cộng một. 

| Nguồn | Đếm | 
| --- | --- | 
| 011₂ = 3 | 1 | 
| 101₂ = 5 | 1 | 
| 110₂ = 6 | 1 | 
| Tổng cộng | 3 | 

Đầu ra:```
3
```Ví dụ này cho thấy cách quá trình đếm liệt kê tất cả các số có số lượng cố định mà không tạo ra chúng một cách rõ ràng. 

### Ví dụ 2 

đầu vào:```
10
1
```Đây`n = 2`. 

Số lượng bit thiết lập hợp lệ thỏa mãn:```
1 + steps[s] = 1
```Chỉ một`s = 1`. 

Đếm số với một bit được đặt: 

| Số | Những cái | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 

Quy trình đếm trở lại`2`. 

Tuy nhiên,`1`không yêu cầu thao tác nào:```
1
```Vì vậy, chúng tôi trừ nó. 

| Số lượng | Giá trị | 
| --- | --- | 
| Số liệu thô | 2 | 
| Xóa số 1 | -1 | 
| Câu trả lời cuối cùng | 1 | 

Số hợp lệ còn lại là`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L²) | Sự kết hợp xây dựng chiếm ưu thế | 
| Không gian | O(L²) | Bảng hệ số nhị thức | 

Với`L ≤ 1000`, bảng chứa khoảng một triệu mục. Điều này dễ dàng phù hợp với giới hạn bộ nhớ và quá trình tiền xử lý bậc hai diễn ra nhanh chóng trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    MOD = 10 ** 9 + 7

    n = input().strip()
    k = int(input())

    L = len(n)

    if k == 0:
        print(1)
        return

    steps = [0] * (L + 1)
    for i in range(2, L + 1):
        steps[i] = steps[i.bit_count()] + 1

    C = [[0] * (L + 1) for _ in range(L + 1)]
    for i in range(L + 1):
        C[i][0] = C[i][i] = 1
        for j in range(1, i):
            C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD

    def count(target):
        used = 0
        res = 0

        for i, ch in enumerate(n):
            if ch == '1':
                rem = L - i - 1
                need = target - used

                if 0 <= need <= rem:
                    res += C[rem][need]

                used += 1

        if used == target:
            res += 1

        return res % MOD

    ans = 0
    for s in range(1, L + 1):
        if 1 + steps[s] == k:
            ans += count(s)

    if k == 1:
        ans -= 1

    print(ans % MOD)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("110\n2\n") == "3", "sample 1"

# minimum value
assert run("1\n0\n") == "1", "only number 1"

# number 1 is not special for k=1
assert run("1\n1\n") == "0", "special handling of 1"

# boundary where only 2 qualifies
assert run("10\n1\n") == "1", "remove number 1"

# numbers <= 3 needing exactly one operation: only 2
assert run("11\n1\n") == "1", "small boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| Trường hợp đặc biệt không hoạt động | 
|`1 / 1`|`0`| Số 1 không được tính cho k=1 | 
|`10 / 1`|`1`| Phép trừ đúng số 1 | 
|`11 / 1`|`1`| Bao gồm tính giới hạn trên | 
|`110 / 2`|`3`| Kịch bản mẫu | 

## Vỏ cạnh 

Hãy xem xét:```
1
0
```Thuật toán ngay lập tức kích hoạt`k = 0`chi nhánh và trả lại`1`. Không cần DP. Điều này phù hợp với thực tế là chỉ có số`1`đã bằng trạng thái mục tiêu. 

Coi như:```
1
1
```Logic đếm số lượng chung sẽ xác định các số có một bit được đặt. Điều đó bao gồm`1`. Sự điều chỉnh cho`k = 1`loại bỏ chính xác sự đóng góp không mong muốn này, để lại câu trả lời`0`. 

Coi như:```
10
1
```Thủ tục đếm tìm thấy hai số có một bit được đặt, cụ thể là`1`Và`2`. Sau khi trừ đi phần đóng góp của`1`, chỉ một`2`vẫn còn. Câu trả lời cuối cùng trở thành`1`, điều này đúng vì:```
2 -> 1
```thực hiện đúng một thao tác. 

Coi như:```
1000
2
```Đây`n = 8`. Số bit thiết lập hợp lệ là`2`. Chữ số-DP đếm chính xác các số:```
3, 5, 6
```Mỗi cái có hai cái, và:```
3 -> 2 -> 1
5 -> 2 -> 1
6 -> 2 -> 1
```Không có số lớn hơn không vượt quá`8`có hai bit được đặt. Câu trả lời là`3`, xác nhận rằng việc đếm tổ hợp xử lý chính xác các hạn chế giới hạn trên một cách chính xác.
