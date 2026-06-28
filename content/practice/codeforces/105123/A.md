---
title: "CF 105123A - Nguyên phân"
description: "Một tế bào bắt đầu bằng nhiễm sắc thể c và phân chia thành hai tế bào con chứa nhiễm sắc thể a và b. Sự phân chia chỉ được coi là đúng nếu mỗi nhiễm sắc thể kết thúc ở đúng một trong hai tế bào con."
date: "2026-06-27T19:31:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105123
codeforces_index: "A"
codeforces_contest_name: "BioCode 2024"
rating: 0
weight: 105123
solve_time_s: 56
verified: true
draft: false
---

[CF 105123A - Nguyên phân](https://codeforces.com/problemset/problem/105123/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một ô bắt đầu bằng`c`nhiễm sắc thể và phân chia thành hai tế bào con chứa`a`Và`b`nhiễm sắc thể. Sự phân chia chỉ được coi là đúng nếu mỗi nhiễm sắc thể kết thúc ở đúng một trong hai tế bào con. Điều đó có nghĩa là tổng số nhiễm sắc thể sau khi phân chia phải giống hệt như trước khi phân chia. 

Đầu vào bao gồm ba số lượng nhiễm sắc thể, một cho mỗi tế bào con và một cho tế bào ban đầu. Nhiệm vụ chỉ đơn giản là xác định xem tổng của hai số con gái có bằng số ban đầu hay không. Nếu có, hãy in`"YES"`. Ngược lại, in`"NO"`. 

Các ràng buộc rất nhỏ vì mọi giá trị đều nằm trong khoảng từ 1 đến 100. Ngay cả một thuật toán kém hiệu quả cũng sẽ chạy ngay lập tức. Một giải pháp thời gian không đổi là đủ vì câu trả lời phụ thuộc vào một so sánh số học duy nhất. 

Nguồn gốc chính của sai lầm là kiểm tra sai điều kiện. 

Xem xét đầu vào```
20 20 40
```Đầu ra đúng là```
YES
```Việc thực hiện bất cẩn có thể so sánh`a == c`hoặc`b == c`, điều này sẽ từ chối sự phân chia hợp lệ này một cách không chính xác. 

Một sai lầm dễ mắc phải khác là chỉ kiểm tra xem một tế bào con có ít nhiễm sắc thể hơn tế bào ban đầu hay không. Ví dụ,```
45 93 39
```Đầu ra đúng là```
NO
```Mặc dù`45`gần với`39`, tổng số sau khi chia là`138`, không`39`, do đó việc chia tách không hợp lệ. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng phép chia bằng cách đếm từng nhiễm sắc thể riêng lẻ. Chúng ta có thể tưởng tượng việc giao cho mỗi`c`nhiễm sắc thể vào một trong hai tế bào con rồi đếm xem mỗi tế bào nhận được bao nhiêu nhiễm sắc thể. Điều này mô hình chính xác quá trình nguyên phân, nhưng nó thực hiện công việc tỷ lệ thuận với số lượng nhiễm sắc thể. Với những ràng buộc nhất định, điều này vẫn có thể chấp nhận được, nhưng nó đang giải quyết một vấn đề khó khăn hơn nhiều so với mức cần thiết. 

Quan sát quan trọng là sự sắp xếp cuối cùng của nhiễm sắc thể không quan trọng. Thuộc tính duy nhất quyết định liệu sự phân chia có đúng hay không là bảo toàn tổng số nhiễm sắc thể. Nếu con gái chứa`a + b`nhiễm sắc thể hoàn toàn, thì sự phân chia có giá trị chính xác khi`a + b == c`. 

Điều này làm giảm toàn bộ vấn đề thành một phép cộng và một phép so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(c) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên`a`,`b`, Và`c`. 
2. Tính tổng`a + b`. Đây là tổng số nhiễm sắc thể có sau khi tế bào phân chia. 
3. So sánh số tiền này với`c`. Nguyên phân đúng sẽ bảo toàn tổng số nhiễm sắc thể. 
4. Nếu`a + b == c`, in`"YES"`. Ngược lại, in`"NO"`. 

### Tại sao nó hoạt động 

Đặc tính xác định của nguyên phân đúng là mọi nhiễm sắc thể từ tế bào ban đầu xuất hiện ở đúng một trong hai tế bào con. Không có nhiễm sắc thể nào bị mất hoặc nhân đôi. Do đó, tổng số nhiễm sắc thể sau khi phân chia phải bằng số ban đầu. Thuật toán kiểm tra chính xác điều kiện này, do đó nó chấp nhận mọi phép chia hợp lệ và từ chối mọi phép chia không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c = map(int, input().split())

if a + b == c:
    print("YES")
else:
    print("NO")
```Đầu tiên chương trình đọc ba số nguyên từ đầu vào. Vì chỉ có một test case nên không cần vòng lặp. 

Sự so sánh`a + b == c`trực tiếp phù hợp với định nghĩa của nguyên phân đúng. Không cần xử lý đặc biệt vì tất cả các giá trị đều là số nguyên dương và giá trị tối đa của chúng chỉ là 100, do đó việc tràn số nguyên là không thể trong Python. 

Đầu ra bao gồm chính xác một từ,`"YES"`hoặc`"NO"`, phù hợp với định dạng được yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
45 93 39
```| Bước | một | b | c | a + b | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 45 | 93 | 39 | 138 | 138 ≠ 39 | 
| Đầu ra | 45 | 93 | 39 | 138 | KHÔNG | 

Các tế bào con cùng chứa 138 nhiễm sắc thể, trong khi tế bào ban đầu chỉ có 39. Vì tổng số khác nhau nên việc phân chia không hợp lệ. 

### Mẫu 2 

đầu vào:```
39 21 60
```| Bước | một | b | c | a + b | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 39 | 21 | 60 | 60 | 60 = 60 | 
| Đầu ra | 39 | 21 | 60 | 60 | CÓ | 

Tổng số nhiễm sắc thể được bảo tồn nên quá trình nguyên phân được coi là chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một phép cộng và một phép so sánh được thực hiện. | 
| Không gian | O(1) | Chỉ có ba số nguyên được lưu trữ. | 

Thời gian chạy và mức sử dụng bộ nhớ là không đổi, không phụ thuộc vào giá trị đầu vào. Điều này dễ dàng thỏa mãn các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    a, b, c = map(int, input().split())
    print("YES" if a + b == c else "NO")

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

# provided samples
assert run("45 93 39\n") == "NO\n", "sample 1"
assert run("39 21 60\n") == "YES\n", "sample 2"

# custom cases
assert run("1 1 2\n") == "YES\n", "minimum valid case"
assert run("100 100 100\n") == "NO\n", "maximum values but invalid total"
assert run("50 50 100\n") == "YES\n", "boundary equality"
assert run("1 100 100\n") == "NO\n", "sum exceeds original by one"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 2`|`YES`| Số lượng nhiễm sắc thể hợp lệ nhỏ nhất | 
|`100 100 100`|`NO`| Giá trị tối đa với tổng số không chính xác | 
|`50 50 100`|`YES`| Bình đẳng ở ranh giới | 
|`1 100 100`|`NO`| Phát hiện số tiền không chính xác lớn hơn số ban đầu | 

## Vỏ cạnh 

Một lỗi phổ biến là kiểm tra xem một tế bào con có cùng số lượng nhiễm sắc thể như tế bào ban đầu hay không thay vì kiểm tra tổng số. 

Đối với đầu vào```
20 20 40
```thuật toán tính toán`20 + 20 = 40`, so sánh nó với`40`, và bản in```
YES
```Điều này đúng vì số lượng nhiễm sắc thể được bảo toàn ở cả hai tế bào con. 

Một sai lầm khác là cho rằng chỉ cần có một ô con gần với số lượng ban đầu là đủ. 

Đối với đầu vào```
45 93 39
```thuật toán tính toán`45 + 93 = 138`. Từ`138 != 39`, nó in```
NO
```Tổng số nhiễm sắc thể tăng lên, vi phạm định nghĩa về nguyên phân chính xác.
