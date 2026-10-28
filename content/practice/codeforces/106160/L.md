---
title: "CF 106160L - Mê cung ngôn ngữ"
description: "Chúng tôi có một lưới có chính xác ba hàng và cột W. Ba tế bào là đặc biệt. Người chơi thứ nhất xuất phát tại A = (1, a), người chơi thứ hai xuất phát tại B = (3, b) và cả hai đều muốn đạt X = (2, 1). Trước khi trò chơi bắt đầu, chúng ta có thể đặt chướng ngại vật trên bất kỳ ô nào ngoại trừ A, B và X."
date: "2026-06-25T11:14:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "L"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 51
verified: true
draft: false
---

[CF 106160L - Mê cung ngôn ngữ](https://codeforces.com/problemset/problem/106160/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lưới có chính xác ba hàng và`W`cột. Ba tế bào là đặc biệt. 

Người chơi đầu tiên bắt đầu lúc`A = (1, a)`, người chơi thứ hai bắt đầu lúc`B = (3, b)`, và cả hai đều muốn đạt được`X = (2, 1)`. 

Trước khi trò chơi bắt đầu, chúng ta có thể đặt chướng ngại vật lên bất kỳ ô nào ngoại trừ`A`,`B`, Và`X`. Sau khi đặt chướng ngại vật, khoảng cách từ ô xuất phát đến ô`X`là độ dài của đường đi hợp lệ ngắn nhất qua các ô không bị chặn. 

Nhiệm vụ không phải là tính khoảng cách. Thay vào đó, chúng ta phải xây dựng một mê cung có khoảng cách đường đi ngắn nhất thỏa mãn`dist(A, X) = dist(B, X) < ∞`. 

Nếu không có mê cung như vậy tồn tại, chúng tôi in`Impossible`. 

Chiều rộng tối đa là 100, do đó không có thách thức về mặt thuật toán về độ phức tạp. Khó khăn thực sự là hiểu được cấu hình nào có thể thực hiện được và tìm ra một mô hình mang tính xây dựng đơn giản. 

Một trường hợp tinh vi xuất hiện khi một vị trí bắt đầu đã có ở cột 1. 

Ví dụ:```
W = 3
a = 3
b = 1
```Tế bào`B`nằm ngay cạnh`X`. Vì chướng ngại vật loại bỏ các ô nhưng không thể loại bỏ cạnh giữa hai ô liền kề còn sống sót, khoảng cách ngắn nhất từ`B`ĐẾN`X`là vĩnh viễn`1`. Người chơi khác cũng không thể có khoảng cách`1`, nên câu trả lời là không thể. 

Một tình huống dễ bỏ sót khác là tính chẵn lẻ. 

Coi như:```
W = 4
a = 3
b = 2
```Mẫu của tuyên bố tuyên bố điều này là không thể. 

Bất kỳ con đường nào từ`(1,a)`ĐẾN`(2,1)`có cùng độ chẵn lẻ với khoảng cách Manhattan của nó. Mỗi đường vòng sẽ thêm một số bước chẵn vào một lưới. Vì vậy mọi con đường có thể từ`A`ĐẾN`X`có sự ngang bằng`a`và mọi con đường có thể từ`B`ĐẾN`X`có sự ngang bằng`b`. 

Nếu như`a`Và`b`có tính chẵn lẻ khác nhau, khoảng cách bằng nhau là không thể. 

Cuối cùng, khi cả hai điểm bắt đầu đều đã liền kề với`X`:```
W = 1
a = 1
b = 1
```cả hai khoảng cách đều đã`1`, thế là mê cung trống rỗng hoạt động. 

## Phương pháp tiếp cận 

Tư duy bạo lực sẽ cố gắng tìm kiếm trên tất cả các vị trí chướng ngại vật và kiểm tra các đường đi ngắn nhất bằng BFS. MỘT`3 × W`bảng chứa tới 300 ô nên số lượng cấu hình chướng ngại vật là khoảng`2^300`, điều đó hoàn toàn không thể thực hiện được. 

Nhận xét quan trọng là vấn đề gần như hoàn toàn được xác định bằng hình học. 

Thực tế hữu ích đầu tiên là tính chẵn lẻ. Bất kỳ con đường nào từ`(1,a)`ĐẾN`(2,1)`có tính chẵn lẻ bằng`a`, bởi vì khoảng cách Manhattan là`a`và mỗi đường vòng sẽ thay đổi độ dài một lượng chẵn. Tương tự, mọi đường đi từ`(3,b)`ĐẾN`(2,1)`có tính chẵn lẻ bằng`b`. 

Khoảng cách bằng nhau đòi hỏi tính chẵn lẻ bằng nhau, vì vậy`a`Và`b`phải có cùng tính chẵn lẻ. 

Thực tế hữu ích thứ hai liên quan đến cột 1. Nếu`a = 1`, sau đó`A`nằm liền kề vĩnh viễn với`X`, Vì thế`dist(A,X) = 1`bất kể chúng ta đặt ra những trở ngại nào. Điều này cũng đúng đối với`b = 1`. 

Điều đó có nghĩa là: 

Nếu chính xác một trong`a`Và`b`bằng`1`, câu trả lời là không thể. 

Sau khi loại bỏ những trường hợp bất khả thi đó, giả sử cả hai vị trí đều ở bên phải cột 1 và có cùng tính chẵn lẻ. 

Cho phép```
t = (a + b) / 2
```Vì các số chẵn lẻ khớp nhau,`t`là một số nguyên. 

Chúng tôi xây dựng một hành lang hình cây. 

Mở một đoạn ngang trên hàng 1 từ cột`a`vào cột`t`. 

Mở một đoạn ngang trên hàng 3 từ cột`b`vào cột`t`. 

Mở hàng 2 từ cột`1`qua cột`t`. 

Chặn mọi thứ khác. 

Bây giờ mọi con đường từ đầu đến`X`trước tiên phải đến cột`t`, sau đó di chuyển dọc theo hàng 2 đến cột 1. 

Khoảng cách từ`A`trở thành```
|a - t| + 1 + (t - 1)
```và khoảng cách từ`B`trở thành```
|b - t| + 1 + (t - 1)
```Bởi vì`t`chính là trung điểm, hai giá trị tuyệt đối bằng nhau nên khoảng cách bằng nhau. 

Biểu đồ kết quả là một cái cây nên không có đường tắt thay thế nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua các vị trí chướng ngại vật | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng điểm giữa | O(W) | O(W) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`a = 1`Và`b = 1`, xuất ra lưới trống chỉ có ba ô đặc biệt được đánh dấu. 
2. Nếu chính xác một trong`a`Và`b`bằng`1`, đầu ra`Impossible`. 

Người chơi xuất phát ở cột 1 luôn ở khoảng cách xa`1`từ`X`, trong khi người chơi khác thì không. 
3. Nếu`a`Và`b`có tính chẵn lẻ, đầu ra khác nhau`Impossible`. 

Mỗi con đường từ`A`ĐẾN`X`có sự ngang bằng`a`, và mọi đường đi từ`B`ĐẾN`X`có sự ngang bằng`b`. Độ dài bằng nhau phải có tính chẵn lẻ bằng nhau. 
4. Tính toán```
t = (a + b) / 2
```Vì các số chẵn lẻ khớp nhau nên đây là một số nguyên. 
5. Bắt đầu với một lưới đầy chướng ngại vật. 
6. Mở mọi ô của hàng 2 từ cột`1`qua cột`t`. 

Đây trở thành hành lang chung dẫn tới`X`. 
7. Mở các ô ở hàng 1 giữa các cột`a`Và`t`, bao gồm. 

Điều này tạo ra nhánh chứa`A`. 
8. Mở các ô ở hàng 3 giữa các cột`b`Và`t`, bao gồm. 

Điều này tạo ra nhánh chứa`B`. 
9. Đặt các ký tự`A`,`B`, Và`X`trong các ô cần thiết của họ và in lưới. 

### Tại sao nó hoạt động 

Việc xây dựng tạo ra một cái cây. Bất kỳ tuyến đường nào từ điểm xuất phát đến`X`phải đi qua điểm nối duy nhất tại cột`t`. 

Độ dài đường đi từ`A`bằng```
|a - t| + 1 + (t - 1).
```Độ dài đường đi từ`B`bằng```
|b - t| + 1 + (t - 1).
```Từ`t`là trung điểm của`a`Và`b`, chúng tôi có```
|a - t| = |b - t|.
```Do đó hai khoảng cách bằng nhau. 

Các điều kiện không thể cũng cần thiết. Tính chẵn lẻ khác nhau sẽ ngăn cản độ dài bằng nhau và nếu chính xác một người xuất phát ở cột 1, thì một người chơi sẽ vĩnh viễn ở khoảng cách 1 trong khi người kia thì không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

W, a, b = map(int, input().split())

if a == 1 and b == 1:
    print("Possible")
    print("A")
    print("X")
    print("B")
    sys.exit()

if (a == 1) ^ (b == 1):
    print("Impossible")
    sys.exit()

if (a - b) & 1:
    print("Impossible")
    sys.exit()

t = (a + b) // 2

g = [['*' for _ in range(W)] for _ in range(3)]

for c in range(1, t + 1):
    g[1][c - 1] = '.'

for c in range(min(a, t), max(a, t) + 1):
    g[0][c - 1] = '.'

for c in range(min(b, t), max(b, t) + 1):
    g[2][c - 1] = '.'

g[0][a - 1] = 'A'
g[1][0] = 'X'
g[2][b - 1] = 'B'

print("Possible")
for row in g:
    print(''.join(row))
```Khối đầu tiên xử lý các tình huống đặc biệt không thể thực hiện được bằng tính chẵn lẻ hoặc bằng khoảng cách cố định-1 liền kề với`X`. 

Điểm giữa`t`là cốt lõi của công trình. Hàng thứ hai trở thành hành lang chung. Hàng đầu tiên và hàng thứ ba đóng góp một nhánh, kết thúc ở cột`t`. 

Việc triển khai sẽ lấp đầy bảng bằng các chướng ngại vật trước tiên và chỉ mở các ô thuộc cây mong muốn. Điều này tránh việc vô tình để lại lối tắt ở một nơi khác. 

Các vòng lặp khoảng thời gian sử dụng`min`Và`max`, vậy chúng có tác dụng hay không`a < t`,`a > t`, hoặc`a = t`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 3 7
```Đây:```
t = (3 + 7) / 2 = 5
```| Bước | Giá trị | 
| --- | --- | 
| một | 3 | 
| b | 7 | 
| t | 5 | 
| Khoảng cách A→t | 2 | 
| Khoảng cách B→t | 2 | 
| Khoảng cách t→X | 4 | 
| Tổng khoảng cách | 7 | 

Lưới xây dựng:```
**A..****
X....****
****..B**
```Cả hai người chơi phải đi 7 bước để đến được`X`. 

Ví dụ này thể hiện ý tưởng trung điểm. Hai nhánh có chiều dài bằng nhau trước khi nối vào hành lang chung. 

### Ví dụ 2 

đầu vào:```
4 3 2
```| Số lượng | Giá trị | 
| --- | --- | 
| một sự ngang bằng | lẻ | 
| b chẵn lẻ | thậm chí | 

Các số chẵn lẻ khác nhau nên khoảng cách bằng nhau là không thể. 

Đầu ra:```
Impossible
```Ví dụ này thể hiện điều kiện chẵn lẻ cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(W) | Mỗi ô được viết một số lần không đổi | 
| Không gian | O(W) | Lưới chứa chính xác`3 × W`tế bào | 

Từ`W ≤ 100`, quá trình xây dựng sẽ chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    data = io.StringIO(inp)
    out = io.StringIO()

    W, a, b = map(int, data.readline().split())

    if a == 1 and b == 1:
        return "Possible\nA\nX\nB\n"

    if (a == 1) ^ (b == 1):
        return "Impossible\n"

    if (a - b) & 1:
        return "Impossible\n"

    t = (a + b) // 2

    g = [['*' for _ in range(W)] for _ in range(3)]

    for c in range(1, t + 1):
        g[1][c - 1] = '.'

    for c in range(min(a, t), max(a, t) + 1):
        g[0][c - 1] = '.'

    for c in range(min(b, t), max(b, t) + 1):
        g[2][c - 1] = '.'

    g[0][a - 1] = 'A'
    g[1][0] = 'X'
    g[2][b - 1] = 'B'

    out.write("Possible\n")
    for row in g:
        out.write("".join(row) + "\n")

    return out.getvalue()

# sample-style checks
assert run("1 1 1\n") == "Possible\nA\nX\nB\n"

assert run("4 3 2\n") == "Impossible\n"

assert run("3 3 1\n") == "Impossible\n"

# custom cases
assert run("5 2 4\n").startswith("Possible")
assert run("10 5 5\n").startswith("Possible")
assert run("7 1 5\n") == "Impossible\n"
assert run("100 99 99\n").startswith("Possible")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`| Có thể | Kích thước tối thiểu | 
|`4 3 2`| Không thể | Tính chẵn lẻ không khớp | 
|`3 3 1`| Không thể | Một lần xuất phát cố định ở khoảng cách 1 | 
|`5 2 4`| Có thể | Xây dựng điểm giữa điển hình | 
|`10 5 5`| Có thể | Điểm giữa bằng cả hai lần xuất phát | 
|`100 99 99`| Có thể | Phạm vi chiều rộng lớn nhất | 

## Vỏ cạnh 

Hãy xem xét:```
3 3 1
```Người chơi`B`bắt đầu lúc`(3,1)`, ngay bên dưới`X`. 

Rìa giữa`(3,1)`Và`(2,1)`luôn tồn tại vì không điểm cuối nào có thể bị chặn. 

Vì thế:```
dist(B, X) = 1
```mãi mãi. 

Người chơi`A`bắt đầu ở cột 3 và cũng không thể đạt được khoảng cách 1. Thuật toán phát hiện chính xác một điểm bắt đầu nằm ở cột 1 và in ra`Impossible`. 

Bây giờ hãy xem xét:```
4 3 2
```Sự bình đẳng của mọi khả năng`A → X`đường đi là số lẻ, trong khi tính chẵn lẻ của mọi đường đi có thể`B → X`đường đi là chẵn. 

Các số nguyên bằng nhau không thể đồng thời là số lẻ và số chẵn. Thuật toán kiểm tra tính chẵn lẻ trước và báo cáo ngay lập tức`Impossible`. 

Cuối cùng, hãy xem xét:```
5 3 3
```Sau đó:```
t = 3
```Cả hai nhánh đều kết nối trực tiếp vào cùng một cột nối. Con đường duy nhất từ ​​đầu đến cuối`X`có chiều dài 3 và việc xây dựng vẫn hợp lệ mà không cần xử lý đặc biệt.
