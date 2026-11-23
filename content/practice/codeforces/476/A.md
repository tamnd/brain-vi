---
title: "CF 476A - Dreamoon và Cầu Thang"
description: "Dreamoon muốn lên tới đỉnh cầu thang có n bậc. Mỗi bước di chuyển có thể bao gồm 1 bước hoặc 2 bước. Trong số tất cả các cách có thể để đạt được chính xác bước n, chúng ta cần số bước di chuyển được chia hết cho m. Nhiệm vụ không yêu cầu số lượng trình tự khác nhau."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 476
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 272 (Div. 2)"
rating: 1000
weight: 476
solve_time_s: 92
verified: true
draft: false
---

[CF 476A - Dreamoon và Cầu thang](https://codeforces.com/problemset/problem/476/A) 

**Đánh giá:** 1000 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Dreamoon muốn lên tới đỉnh cầu thang chứa`n`các bước. Mỗi bước di chuyển có thể bao gồm 1 bước hoặc 2 bước. Trong số tất cả các cách có thể để đạt được bước chính xác`n`, chúng ta cần số bước di chuyển được sử dụng để chia hết cho`m`. 

Nhiệm vụ không yêu cầu số lượng trình tự khác nhau. Nó yêu cầu số lần di chuyển nhỏ nhất có thể là bội số của`m`và vẫn giúp Dreamoon đạt đến đỉnh cao. Nếu không có số lần di chuyển như vậy tồn tại, chúng ta phải in`-1`. 

Những hạn chế là rất nhỏ. Chiều cao cầu thang tối đa là 10000 và`m`nhiều nhất là 10. Ngay cả một lần quét tuyến tính đơn giản trên tất cả số lần di chuyển có thể cũng sẽ đủ nhanh. Không cần lập trình động, tìm kiếm đồ thị hoặc tổ hợp. 

Quan sát quan trọng xuất phát từ việc hiểu số lần di chuyển nào có thể thực hiện được. 

Nếu mỗi nước đi là nước đi 2 bước thì số nước đi càng nhỏ càng tốt. Mức tối thiểu đó là`ceil(n / 2)`. 

Nếu mỗi nước đi là nước đi 1 bước thì số nước đi càng lớn càng tốt. tối đa đó là`n`. 

Mỗi số nguyên di chuyển giữa hai giá trị này đều có thể đạt được. Bắt đầu từ tất cả các bước di chuyển 2 bước, thay thế một bước di chuyển 2 bước bằng hai bước di chuyển 1 bước sẽ tăng số lần di chuyển lên đúng 1 trong khi vẫn giữ nguyên tổng khoảng cách leo lên. 

Có một số trường hợp đặc biệt dễ gây ra sai sót. 

Coi như:```
3 5
```Số lần di chuyển tối thiểu có thể là`2`và tối đa là`3`. Không có bội số của`5`trong khoảng thời gian đó, vì vậy câu trả lời là:```
-1
```Một giải pháp bất cẩn chỉ làm tròn số lần di chuyển tối thiểu lên bội số tiếp theo của`m`sẽ có được`5`, điều này là không thể vì năm nước đi không thể leo đúng ba bậc. 

Một trường hợp quan trọng khác là:```
1 2
```Số lần di chuyển duy nhất có thể là`1`. Từ`1`không chia hết cho`2`, đáp án đúng là:```
-1
```Một giải pháp giả định luôn có câu trả lời sẽ thất bại ở đây. 

Cuối cùng:```
10 2
```Số lần di chuyển tối thiểu có thể là`5`. Từ`6`là bội số đầu tiên của`2`đó ít nhất là`5`, câu trả lời là:```
6
```Thuật toán phải tìm kiếm trong khoảng khả thi thay vì mù quáng lấy điểm cuối. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là liệt kê mọi số lần di chuyển có thể có và kiểm tra xem liệu nó có thể tương ứng với một lần leo núi hợp lệ hay không. Số lần di chuyển`k`là khả thi nếu`ceil(n/2) ≤ k ≤ n`. Trong số tất cả các giá trị khả thi, chúng tôi chọn giá trị nhỏ nhất chia hết cho`m`. 

Đối với vấn đề này, khoảng chứa tối đa 10000 giá trị, do đó, ngay cả việc quét tuyến tính cũng hoàn toàn có thể chấp nhận được. 

Phần thú vị là nhận ra lý do tại sao khoảng khả thi lại chính xác từ`ceil(n/2)`ĐẾN`n`. 

Số nước đi nhỏ nhất xảy ra khi chúng ta sử dụng càng nhiều nước đi 2 bước càng tốt. Điều đó mang lại`ceil(n/2)`di chuyển. 

Số nước đi lớn nhất xảy ra khi mỗi nước đi là nước đi 1 bước. Điều đó mang lại`n`di chuyển. 

Mọi giá trị ở giữa đều có thể truy cập được vì việc thay thế một bước di chuyển 2 bước bằng hai bước di chuyển 1 bước sẽ tăng số lần di chuyển lên đúng một mà không làm thay đổi tổng khoảng cách leo lên. 

Một khi tính chất này được hiểu thì vấn đề trở nên rất đơn giản. Chúng ta chỉ cần bội số nhỏ nhất của`m`nằm bên trong khoảng`[ceil(n/2), n]`. 

Thay vì quét toàn bộ khoảng, chúng ta có thể tính trực tiếp bội số đầu tiên của`m`đó ít nhất là`ceil(n/2)`. Nếu bội số đó không vượt quá`n`, đó là câu trả lời. Nếu không thì không có số lượt di chuyển hợp lệ nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lần di chuyển tối thiểu có thể. 

Đây là`ceil(n / 2)`, có thể được viết là`(n + 1) // 2`sử dụng số học số nguyên. 
2. Tìm bội số nhỏ nhất của`m`đó ít nhất là giá trị tối thiểu này. 

Cho phép`mn`là số lần di chuyển tối thiểu. bội số mong muốn là:`((mn + m - 1) // m) * m`Đây là cách tiêu chuẩn để làm tròn một số lên bội số tiếp theo. 
3. Kiểm tra xem bội số này có còn là số lần di chuyển khả thi hay không. 

Nếu bội số lớn hơn`n`, nó nằm ngoài khoảng có thể đạt được và không có câu trả lời nào tồn tại. 
4. In bội số nếu khả thi, nếu không thì in`-1`. 

### Tại sao nó hoạt động 

Mỗi lần leo núi hợp lệ đều sử dụng một số bước di chuyển 1 bước và một số bước di chuyển 2 bước. Số lần di chuyển không bao giờ có thể nhỏ hơn`ceil(n/2)`và không bao giờ có thể vượt quá`n`. 

Hơn nữa, mọi số nguyên giữa các giới hạn đó đều có thể đạt được. Bắt đầu từ giải pháp di chuyển tối thiểu, mỗi lần thay thế một bước di chuyển 2 bước bằng hai bước di chuyển 1 bước sẽ tăng số lần di chuyển lên đúng một trong khi vẫn duy trì tổng khoảng cách đã leo. 

Bởi vì tất cả số lần di chuyển trong khoảng đều khả thi nên bài toán giảm xuống còn việc tìm bội số nhỏ nhất của`m`bên trong khoảng đó. Thuật toán tính toán chính xác giá trị đó. Nếu bội số đầu tiên vượt quá giới hạn trên`n`, thì không tồn tại bội số hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

mn = (n + 1) // 2
ans = ((mn + m - 1) // m) * m

if ans <= n:
    print(ans)
else:
    print(-1)
```Tính toán đầu tiên tìm ra số lần di chuyển tối thiểu có thể đạt được. sử dụng`(n + 1) // 2`tránh số học dấu phẩy động và thực hiện chính xác thao tác trần. 

Dòng tiếp theo làm tròn giá trị tối thiểu này lên bội số gần nhất của`m`. Điều này trực tiếp đưa ra câu trả lời nhỏ nhất cho ứng viên. 

Việc kiểm tra cuối cùng là rất quan trọng. Một bội số có thể tồn tại trên khoảng khả thi. Ví dụ, với`n = 3`Và`m = 5`, giá trị làm tròn là`5`, nhưng năm nước đi không thể chỉ leo được ba bậc thang. So sánh với`n`nắm bắt tình hình này và tạo ra`-1`. 

Tất cả các phép tính đều phù hợp thoải mái với các kiểu số nguyên tiêu chuẩn và Python xử lý chúng một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 2
```| Biến | Giá trị | 
| --- | --- | 
| n | 10 | 
| m | 2 | 
| mn = (n + 1) // 2 | 5 | 
| Bội số đầu tiên của 2 ≥ 5 | 6 | 
| Kiểm tra 6 ≤ 10 | Đúng | 
| Trả lời | 6 | 

Số lần di chuyển tối thiểu có thể là 5. Vì 5 không chia hết cho 2 nên chúng ta chuyển sang bội số tiếp theo là 6. Nó vẫn nằm trong khoảng khả thi`[5, 10]`, vậy đáp án là 6. 

### Mẫu 2 

đầu vào:```
3 5
```| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| m | 5 | 
| mn = (n + 1) // 2 | 2 | 
| Bội số đầu tiên của 5 ≥ 2 | 5 | 
| Kiểm tra 5 ≤ 3 | Sai | 
| Trả lời | -1 | 

Số nước đi khả thi chỉ là 2 và 3. Cả hai đều không chia hết cho 5. Bội số đầu tiên của 5 đã nằm ngoài khoảng nên không có câu trả lời hợp lệ nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Giải pháp thực hiện số học theo thời gian không đổi bất kể giá trị của`n`. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    n, m = map(int, input().split())

    mn = (n + 1) // 2
    ans = ((mn + m - 1) // m) * m

    print(ans if ans <= n else -1)

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
assert run("10 2\n") == "6", "sample 1"

# sample-like impossible case
assert run("3 5\n") == "-1", "sample 2"

# minimum staircase
assert run("1 2\n") == "-1", "minimum size"

# exact multiple at lower bound
assert run("4 2\n") == "2", "lower bound already valid"

# answer requires rounding up
assert run("7 3\n") == "6", "round to next multiple"

# large boundary
assert run("10000 10\n") == "5000", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`-1`| Cầu thang nhỏ nhất, không có bội số hợp lệ | 
|`4 2`|`2`| Giới hạn dưới đã chia hết cho`m`| 
|`7 3`|`6`| Làm tròn đúng đến bội số tiếp theo | 
|`10000 10`|`5000`| Kích thước đầu vào lớn nhất | 
|`3 5`|`-1`| Không tồn tại bội số bên trong khoảng khả thi | 

## Vỏ cạnh 

Hãy xem xét:```
1 2
```Thuật toán tính toán:`mn = (1 + 1) // 2 = 1`Bội số đầu tiên của 2 ít nhất 1 là 2. 

Kể từ khi`2 > 1`, giá trị nằm ngoài khoảng khả thi`[1, 1]`. Thuật toán in:```
-1
```cái nào đúng. 

Bây giờ hãy xem xét:```
3 5
```Số lần di chuyển khả thi chỉ là 2 và 3. 

Thuật toán tính toán:`mn = 2`bội số đầu tiên của 5 ít nhất 2 là 5 

Kể từ khi`5 > 3`, không có bội số khả thi nào tồn tại, vì vậy đầu ra là:```
-1
```Cuối cùng hãy xem xét:```
4 2
```Số lần di chuyển tối thiểu là:`mn = 2`Vì 2 đã chia hết cho 2 nên bội số làm tròn vẫn là 2. 

Bởi vì`2 ≤ 4`, thuật toán đầu ra:```
2
```Điều này xác minh rằng giải pháp xử lý chính xác các trường hợp trong đó câu trả lời tối ưu chính xác là số lần di chuyển khả thi tối thiểu.
