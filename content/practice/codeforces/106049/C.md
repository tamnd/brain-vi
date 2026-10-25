---
title: "CF 106049C - Alyona Yêu Dãy Núi"
description: "Chúng ta được cấp một số n và một dãy các giá trị được phép [l, r]. Chúng ta cần tìm số nguyên x nhỏ nhất trong phạm vi này sao cho ước số chung lớn nhất của n và x cũng nằm trong cùng phạm vi. Nếu không có x như vậy tồn tại, chúng ta in -1."
date: "2026-06-25T12:33:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106049
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #44 (DIV3.5-Forces)"
rating: 0
weight: 106049
solve_time_s: 41
verified: true
draft: false
---

[CF 106049C - Alyona Loves Ranges](https://codeforces.com/problemset/problem/106049/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số`n`và một khoảng giá trị cho phép`[l, r]`. Chúng ta cần tìm số nguyên nhỏ nhất`x`trong phạm vi này sao cho ước số chung lớn nhất của`n`Và`x`cũng nằm trong phạm vi tương tự. Nếu không như vậy`x`tồn tại, chúng tôi in`-1`. 

Nhận xét quan trọng xuất phát từ định nghĩa của gcd. Bất kể giá trị nào`gcd(n, x)`là, nó phải chia`n`. Ngoài ra, nó không bao giờ có thể lớn hơn`x`. Điều này có nghĩa là mọi câu trả lời hợp lệ đều được kết nối với ước số của`n`. 

Đầu vào chứa tối đa`1000`trường hợp thử nghiệm. Các giá trị của`n`,`l`, Và`r`có thể lớn như`10^9`, vì vậy việc lặp lại mọi giá trị trong`[l, r]`là không thể. Bản thân phạm vi có thể chứa một tỷ số. Chúng ta cần một cách tiếp cận phụ thuộc vào quy mô của`n`thay vào đó là hệ số hóa của . Kiểm tra đến`sqrt(n)`có thể chấp nhận được vì`sqrt(10^9)`chỉ là về`31623`, đưa ra xung quanh`3.2 * 10^7`hoạt động trên tất cả các trường hợp thử nghiệm. 

Các trường hợp phức tạp xuất phát từ việc nhầm lẫn một gcd hợp lệ với một gcd hợp lệ.`x`. Một số có thể có gcd hợp lệ mà bản thân nó không phải là câu trả lời nhỏ nhất có thể. Ví dụ: nếu đầu vào là:```
6 5 6
```Các giá trị trong phạm vi là`5`Và`6`. Giá trị đầu tiên mang lại`gcd(6,5)=1`, không hợp lệ. Thứ hai cho`gcd(6,6)=6`, vậy câu trả lời là`6`. Một giải pháp chỉ kiểm tra xem phạm vi có chứa một số giá trị với gcd không tầm thường hay không có thể chấp nhận không chính xác`5`. 

Một trường hợp cạnh khác là khi số chia tồn tại nhưng nằm bên dưới`l`. Vì:```
12 7 11
```Các ước số của`12`là`1, 2, 3, 4, 6, 12`. Ước số duy nhất bên trong hoặc trên phạm vi là`12`, but it is outside the range because `12 > 11`. Không hợp lệ`x`tồn tại, vì vậy câu trả lời là`-1`. Việc thực hiện bất cẩn làm tìm ước số tiếp theo sau`l`không kiểm tra`r`sẽ thất bại. 

Trường hợp ranh giới cuối cùng là phạm vi một phần tử:```
10 1 1
```Câu trả lời duy nhất có thể là`1`. Từ`gcd(10,1)=1`, đầu ra là`1`. Điều này kiểm tra xem thuật toán có xử lý được không`l = r`một cách chính xác. 

## Phương pháp tiếp cận 

Một phương pháp đơn giản sẽ thử mọi cách có thể`x`từ`l`ĐẾN`r`, tính toán`gcd(n, x)`và trả về giá trị đầu tiên hoạt động. Điều này đúng vì chúng tôi kiểm tra các ứng viên theo thứ tự tăng dần, do đó, ứng viên hợp lệ đầu tiên tự động là tối thiểu. 

Vấn đề là kích thước của phạm vi. Nếu như`l = 1`Và`r = 10^9`, điều này đòi hỏi tới một tỷ phép tính gcd cho một trường hợp thử nghiệm. Với tối đa`1000`kiểm tra, trường hợp xấu nhất là vượt xa giới hạn thời gian cho phép. 

Quan sát quan trọng là một gcd hợp lệ phải là ước số của`n`. Giả định`x`là một câu trả lời hợp lệ và`d = gcd(n, x)`. Từ`d`chia rẽ`n`Và`d`ở bên trong`[l, r]`, số`d`bản thân nó cũng là một lựa chọn hợp lệ vì`gcd(n, d) = d`. Cũng,`d <= x`, do đó thay thế`x`với`d`không bao giờ làm cho câu trả lời tồi tệ hơn. 

Điều này biến đổi vấn đề hoàn toàn. Ta chỉ cần tìm ước số nhỏ nhất của`n`cái đó thuộc về`[l, r]`. Từ`n`nhiều nhất là`10^9`, chúng ta có thể liệt kê các ước số bằng cách kiểm tra các cặp xung quanh`sqrt(n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r - l + 1) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(n)) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các số chia có thể có`i`từ`1`ĐẾN`sqrt(n)`. Nếu như`i`chia rẽ`n`, thì cả hai`i`Và`n / i`là các ước số. 
2. Với mỗi ước số tìm được, hãy kiểm tra xem nó có thuộc về`[l, r]`. Giữ ước số hợp lệ nhỏ nhất được thấy cho đến nay. 
3. Sau khi tất cả các ước số được xử lý, xuất ra giá trị lưu trữ nhỏ nhất. Nếu không có ước số nào nằm trong phạm vi, xuất ra`-1`. 

Lý do kiểm tra đến`sqrt(n)`là đủ là mọi ước số đều lớn hơn`sqrt(n)`có ước số phù hợp nhỏ hơn`sqrt(n)`. Cặp đôi`(i, n / i)`chúng ta hãy khám phá cả hai mặt của việc nhân tử hóa. 

Tại sao nó hoạt động: Mọi câu trả lời hợp lệ đều có thể được rút gọn thành ước số của`n`. Thuật toán kiểm tra mọi ước số của`n`, vì vậy nó không thể bỏ lỡ một ứng cử viên hợp lệ. Vì nó luôn giữ ước số nhỏ nhất trong khoảng yêu cầu nên giá trị cuối cùng chính xác là giá trị tối thiểu có thể`x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, l, r):
    ans = 10**18
    i = 1
    while i * i <= n:
        if n % i == 0:
            if l <= i <= r and i < ans:
                ans = i
            other = n // i
            if l <= other <= r and other < ans:
                ans = other
        i += 1

    return -1 if ans == 10**18 else ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n, l, r = map(int, input().split())
        out.append(str(solve_case(n, l, r)))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```chức năng`solve_case`thực hiện tìm kiếm số chia. Điều kiện vòng lặp`i * i <= n`là đủ vì mỗi cặp nhân tố có nhiều nhất một phần tử là căn bậc hai. 

Khi một số chia`i`được tìm thấy, mã cũng kiểm tra`n // i`, đó là số chia ghép đôi. Điều này tránh thiếu các ước số lớn có thể nằm trong khoảng. Câu trả lời bắt đầu bằng một giá trị rất lớn thay vì`None`, điều này làm cho việc so sánh tối thiểu trở nên đơn giản. 

Chức năng chính đọc tất cả các trường hợp kiểm thử và thu thập câu trả lời trước khi in. Việc triển khai chỉ sử dụng số học số nguyên, do đó không có vấn đề tràn trong Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 5 7
```Các ước số của`6`được kiểm tra. 

| ứng cử viên chia | kiểm tra phạm vi hợp lệ | câu trả lời hiện tại | 
| --- | --- | --- | 
| 1 | không | không | 
| 2 | không | không | 
| 3 | không | không | 
| 6 | vâng | 6 | 

Ước số nhỏ nhất bên trong`[5,7]`là`6`, vậy câu trả lời là`6`. 

### Mẫu 2 

đầu vào:```
10 1 10
```| ứng cử viên chia | kiểm tra phạm vi hợp lệ | câu trả lời hiện tại | 
| --- | --- | --- | 
| 1 | vâng | 1 | 
| 2 | vâng | 1 | 
| 5 | vâng | 1 | 
| 10 | vâng | 1 | 

Số chia hợp lệ đầu tiên là`1`, và không có ước số sau này có thể cải thiện nó. Câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(n)) | Chúng tôi chỉ kiểm tra các ứng cử viên chia đến căn bậc hai của`n`| 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Tối đa của`sqrt(10^9)`là về`31623`, do đó, ngay cả trong tất cả các trường hợp thử nghiệm, số lượng thao tác vẫn đủ nhỏ để vượt qua giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided samples
assert solve("""3
6 5 7
10 1 10
2 3 4
""") == """6
1
-1
""", "samples"

# minimum size
assert solve("""1
1 1 1
""") == """1
""", "single value"

# all divisors outside the range
assert solve("""1
12 7 11
""") == """-1
""", "no divisor"

# catches missing large divisor checks
assert solve("""1
999983 999982 999983
""") == """999983
""", "prime number"

# several divisor choices, smallest must be chosen
assert solve("""1
100 20 100
""") == """20
""", "smallest divisor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Xử lý phạm vi nhỏ nhất có thể | 
|`12 7 11`|`-1`| Phát hiện các trường hợp không có ước số trong khoảng | 
|`999983 999982 999983`|`999983`| Kiểm tra hành vi nguyên tố lớn | 
|`100 20 100`|`20`| Đảm bảo chọn số chia tối thiểu | 

## Vỏ cạnh 

Đối với đầu vào:```
6 5 6
```Thuật toán kiểm tra các ước của`6`. Nó tìm thấy`1`,`2`,`3`, Và`6`. Chỉ một`6`nằm trong khoảng thời gian cần thiết, vì vậy câu trả lời trở thành`6`. Điều này xử lý trường hợp các giá trị trong phạm vi không tự động hợp lệ. 

Vì:```
12 7 11
```Tìm kiếm số chia tìm thấy`1`,`2`,`3`,`4`,`6`, Và`12`. Không có gì ở giữa`7`Và`11`, do đó câu trả lời được lưu trữ không thay đổi và thuật toán sẽ in`-1`. Việc kiểm tra phạm vi ngăn cản việc chấp nhận một ước số nằm ngoài khoảng cho phép. 

Vì:```
10 1 1
```Ước số đầu tiên tìm được là`1`, ngay lập tức trở thành câu trả lời. Thuật toán vẫn hoàn thành việc quét số chia nhưng không có giá trị nào khác có thể nhỏ hơn. Điều này bao gồm một phạm vi chứa chính xác một giá trị có thể.
