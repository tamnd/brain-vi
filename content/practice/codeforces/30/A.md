---
title: "CF 30A - Kế toán"
description: "Cho 3 số nguyên A, B, n. Nhiệm vụ là tìm số nguyên X sao cho:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 30
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 30 (Codeforces format)"
rating: 1400
weight: 30
solve_time_s: 83
verified: true
draft: false
---
[CF 30A - Kế toán](https://codeforces.com/problemset/problem/30/A) 

**Đánh giá:** 1400 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên,`A`,`B`, Và`n`. Nhiệm vụ là tìm một giá trị nguyên`X`như vậy:$A \cdot X^n = B$

`A`đại diện cho thu nhập của vương quốc trong năm`0`,`B`đại diện cho thu nhập sau`n`năm, và`X`là hệ số tăng trưởng hàng năm. Mọi giá trị phải là số nguyên, bao gồm`X`. Giá trị âm được cho phép và`X`cũng có thể bằng không. 

Những hạn chế là rất nhỏ. Cả hai`A`Và`B`nhiều nhất là`1000`về giá trị tuyệt đối và`n`nhiều nhất là`10`. Điều đó làm thay đổi hoàn toàn bản chất của vấn đề. Chúng ta không cần lý thuyết số hay logarit nâng cao. Ngay cả một lực lượng vũ phu trực tiếp đối với tất cả các ứng cử viên số nguyên hợp lý cũng dễ dàng phù hợp trong thời hạn. 

Phần khó khăn không phải là hiệu suất mà là tính chính xác xung quanh các dấu và số 0. 

Một trường hợp nguy hiểm là khi`A = 0`. 

Ví dụ:```
0 0 5
```Phương trình trở thành:$0 \cdot X^5 = 0$Mỗi số nguyên`X`thỏa mãn nó. Bài toán cho phép in ra bất kỳ câu trả lời hợp lệ nào, do đó xuất ra`0`được rồi. 

Nhưng với:```
0 10 3
```chúng tôi nhận được:$0 \cdot X^3 = 10$Không có số nguyên nào có thể thỏa mãn điều này, vì vế trái luôn là`0`. 

Một trường hợp tinh vi khác xuất hiện với câu trả lời phủ định và thậm chí cả quyền năng. 

Coi như:```
1 -8 2
```Chúng tôi cần:$X^2 = -8$Một lũy thừa chẵn không bao giờ có thể trở nên âm, vì vậy câu trả lời là`"No solution"`. 

Nhưng với:```
1 -8 3
```chúng tôi cần:$X^3 = -8$Hiện nay`X = -2`hoạt động vì quyền hạn kỳ lạ bảo toàn dấu hiệu. 

Việc triển khai bất cẩn bằng cách sử dụng các gốc dấu phẩy động cũng có thể thất bại. Tính toán một cái gì đó như`pow(64, 1/3)`có thể sản xuất`3.999999999`thay vì`4`, dẫn đến sai sót khi làm tròn. Vì các ràng buộc rất nhỏ nên lực lượng vũ phu số nguyên sẽ tránh được mọi vấn đề về độ chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi số nguyên có thể`X`, tính toán`A * X^n`, và kiểm tra xem nó có bằng không`B`. 

Tại sao điều này làm việc? Vì không gian tìm kiếm rất nhỏ. Nếu như`|B| ≤ 1000`, thì bất kỳ nghiệm số nguyên hợp lệ nào cũng phải có giá trị nhỏ hợp lý. Ví dụ, nếu`|X| ≥ 100`, thì thậm chí`X²`ít nhất đã trở thành`10000`, nằm ngoài phạm vi cho phép trừ khi`A = 0`. 

Thử mọi số nguyên từ`-1000`ĐẾN`1000`chỉ cho`2001`ứng viên. Đối với mỗi ứng cử viên, chúng tôi tính toán lũy thừa theo số mũ`10`. Đó chỉ là vài chục nghìn phép tính số học, về cơ bản là tức thời. 

Một cách tiếp cận toán học hơn sẽ tách biệt:$X^n = \frac{B}{A}$và sau đó cố gắng tính một số nguyên`n`-gốc thứ. Vấn đề là việc xử lý các dấu hiệu, tính chia hết và độ chính xác của dấu phẩy động trở nên khó chịu hơn chính vấn đề ban đầu. Bởi vì các ràng buộc quá nhỏ nên brute-force thực sự là giải pháp sạch sẽ và an toàn nhất. 

Lực lượng vũ phu hoạt động vì mọi câu trả lời hợp lệ phải nằm trong một phạm vi rất nhỏ. Khi chúng tôi nhận ra điều đó, vấn đề sẽ giảm xuống thành một tìm kiếm đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các số nguyên | O(2000 × n) | O(1) | Đã chấp nhận | 
| Phương pháp tính nghiệm số nguyên | O(n) | O(1) | Được chấp nhận nhưng dễ bị lỗi không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`A`,`B`, Và`n`. 
2. Lặp qua tất cả các số nguyên`x`từ`-1000`ĐẾN`1000`. 

Phạm vi này đủ lớn một cách an toàn vì`|B| ≤ 1000`. Bất kỳ độ lớn nào lớn hơn sẽ làm cho`x^n`vượt xa mục tiêu trừ khi xảy ra trường hợp đặc biệt. 
3. Với mỗi ứng viên, hãy tính:$A \cdot x^n$và so sánh nó với`B`. 
4. Nếu đẳng thức giữ nguyên, hãy in`x`ngay lập tức và chấm dứt. 

Bài toán cho phép bất kỳ giải pháp hợp lệ nào, vì vậy giải pháp đầu tiên là đủ. 
5. Nếu vòng lặp kết thúc mà không tìm thấy số nguyên hợp lệ, hãy in`"No solution"`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi ứng cử viên số nguyên có thể thỏa mãn phương trình. Nếu một giải pháp tồn tại, nó phải xuất hiện ở đâu đó trong phạm vi được thử nghiệm để cuối cùng thuật toán sẽ tìm thấy nó. Nếu không có ứng viên nào thỏa mãn phương trình thì không tồn tại nghiệm số nguyên. 

Tính chính xác đến trực tiếp từ việc tìm kiếm toàn diện trên tất cả các giá trị nguyên khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

A, B, n = map(int, input().split())

for x in range(-1000, 1001):
    if A * (x ** n) == B:
        print(x)
        break
else:
    print("No solution")
```Vòng lặp thử mọi số nguyên trong phạm vi cho phép. Số học số nguyên của Python xử lý lũy thừa âm một cách chính xác khi số mũ là số nguyên và không âm, vì vậy`(-2) ** 3`trở thành`-8`Và`(-2) ** 2`trở thành`4`tự động. 

các`for-else`cấu trúc rất hữu ích ở đây. các`else`khối chỉ thực thi nếu vòng lặp không bao giờ chạm đến điểm`break`, có nghĩa là không tìm thấy giải pháp hợp lệ. 

Phạm vi`[-1000, 1000]`là cố ý đơn giản. Chúng ta có thể rút ra những giới hạn chặt chẽ hơn về mặt toán học, nhưng chẳng có lợi ích gì. Kích thước tìm kiếm hiện tại đã rất nhỏ. 

Sử dụng số học số nguyên ở mọi nơi sẽ tránh hoàn toàn các vấn đề về độ chính xác của dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 18 2
```Chúng tôi cần:$2 \cdot X^2 = 18$| x | x² | 2 × x2 | Trận đấu với B? | 
| --- | --- | --- | --- | 
| -3 | 9 | 18 | Có | 
| -2 | 4 | 8 | Không | 
| -1 | 1 | 2 | Không | 
| 0 | 0 | 0 | Không | 
| 1 | 1 | 2 | Không | 
| 2 | 4 | 8 | Không | 
| 3 | 9 | 18 | Có | 

Thuật toán dừng ở câu trả lời hợp lệ đầu tiên nó gặp. Vì quá trình lặp bắt đầu từ số âm nên nó thực sự in`-3`. Điều đó hoàn toàn hợp lệ vì cả hai`3²`Và`(-3)²`bình đẳng`9`. 

Ví dụ này chứng tỏ rằng vấn đề chấp nhận bất kỳ giải pháp hợp lệ nào. 

### Ví dụ 2 

đầu vào:```
1 -8 2
```Chúng tôi cần:$X^2 = -8$| x | x² | Trận đấu -8? | 
| --- | --- | --- | 
| -3 | 9 | Không | 
| -2 | 4 | Không | 
| -1 | 1 | Không | 
| 0 | 0 | Không | 
| 1 | 1 | Không | 
| 2 | 4 | Không | 
| 3 | 9 | Không | 

Mọi ô vuông đều không âm, vì vậy vòng lặp không bao giờ tìm thấy kết quả khớp. 

Đầu ra:```
No solution
```Ví dụ này cho thấy tại sao việc xử lý dấu hiệu lại quan trọng khi`n`là chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2000 × n) | Nhiều nhất là 2001 ứng cử viên, mỗi ứng cử viên yêu cầu lũy thừa lũy thừa`n`| 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`n ≤ 10`và chỉ`2001`giá trị được kiểm tra, chương trình thực hiện rất ít công việc. Thời gian chạy thấp hơn nhiều so với giới hạn và mức sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    A, B, n = map(int, input().split())

    for x in range(-1000, 1001):
        if A * (x ** n) == B:
            print(x)
            break
    else:
        print("No solution")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("2 18 2\n") in ["3", "-3"], "sample 1"

# minimum values
assert run("0 0 1\n") == "-1000", "any integer works"

# impossible because A is zero
assert run("0 5 3\n") == "No solution", "zero times anything stays zero"

# negative root with odd exponent
assert run("1 -27 3\n") == "-3", "odd powers preserve sign"

# impossible with even exponent
assert run("1 -16 2\n") == "No solution", "even powers cannot be negative"

# maximum boundary style case
assert run("1 1000 1\n") == "1000", "largest positive root"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1`| Bất kỳ số nguyên nào | Giải pháp hợp lệ vô hạn | 
|`0 5 3`|`No solution`| Không thể có hệ số 0 | 
|`1 -27 3`|`-3`| Căn âm với sức mạnh kỳ lạ | 
|`1 -16 2`|`No solution`| Ngay cả quyền lực cũng không thể tạo ra âm bản | 
|`1 1000 1`|`1000`| Câu trả lời ranh giới lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
0 0 5
```Thuật toán bắt đầu kiểm tra từ`x = -1000`. 

Chúng tôi tính toán:$0 \cdot (-1000)^5 = 0$Cái này đã khớp rồi`B = 0`, do đó thuật toán in ngay`-1000`. 

Điều đó có thể trông lạ lùng nhưng nó hoàn toàn chính xác. Mọi số nguyên đều thỏa mãn phương trình ở đây và câu lệnh cho phép bất kỳ câu trả lời hợp lệ nào. 

Bây giờ hãy xem xét:```
0 10 3
```Đối với mỗi ứng viên`x`, vế trái trở thành:$0 \cdot x^3 = 0$Từ`0`không bao giờ bằng`10`, vòng lặp kết thúc không thành công và in ra`"No solution"`. 

Một trường hợp cạnh quan trọng khác là:```
1 -16 2
```Thuật toán kiểm tra tất cả các số nguyên từ`-1000`ĐẾN`1000`. 

Đối với những ứng viên tiêu cực:$(-x)^2 > 0$Đối với những ứng viên tích cực:$x^2 > 0$Hình vuông luôn không âm, vì vậy mục tiêu`-16`không bao giờ có thể xuất hiện. Việc tìm kiếm đầy đủ báo cáo chính xác`"No solution"`. 

Cuối cùng, hãy xem xét:```
2 18 2
```Cả hai`x = 3`Và`x = -3`thỏa mãn phương trình vì:$3^2 = (-3)^2 = 9$Thuật toán trả về giá trị hợp lệ đầu tiên mà nó gặp, đó là`-3`. Vấn đề rõ ràng cho phép bất kỳ giải pháp hợp lệ nào, vì vậy hành vi này là chính xác.
