---
title: "CF 104677A - Pizza"
description: "Nhiệm vụ mô tả một kịch bản phân chia đơn giản. Một người có số lượng lát bánh pizza cố định và một nhóm bạn. Các lát cắt được phân bổ đồng đều nhất có thể cho tất cả bạn bè và bất kỳ thứ gì không thể phân bổ đều sẽ không được sử dụng."
date: "2026-06-29T09:11:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104677
codeforces_index: "A"
codeforces_contest_name: "Sugar Sweet \u2764\ufe0f"
rating: 0
weight: 104677
solve_time_s: 56
verified: true
draft: false
---

[CF 104677A - Pizza](https://codeforces.com/problemset/problem/104677/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một kịch bản phân chia đơn giản. Một người có số lượng lát bánh pizza cố định và một nhóm bạn. Các lát cắt được phân bổ đồng đều nhất có thể cho tất cả bạn bè và bất kỳ thứ gì không thể phân bổ đều sẽ không được sử dụng. 

Dữ liệu đầu vào bao gồm hai số nguyên, trong đó số đầu tiên biểu thị số lát bánh pizza có sẵn và số thứ hai biểu thị số lượng bạn bè sẽ chia sẻ chúng. Đầu ra phải mô tả hai đại lượng. Đầu tiên là số lát mà mỗi người bạn nhận được sau khi chia đều và thứ hai là số lát vẫn chưa được phân phối. 

Các ràng buộc lên tới 10^9 cho cả hai giá trị, điều này ngay lập tức báo hiệu rằng mọi cách tiếp cận mô phỏng phân phối theo từng lát sẽ quá chậm. Một vòng lặp có tối đa 10^9 lần lặp sẽ không kết thúc đúng thời gian dưới giới hạn 1 giây. Lời giải phải tính kết quả theo thời gian không đổi bằng cách sử dụng các phép tính số học. 

Trường hợp cạnh tinh tế xuất hiện khi số lượng lát nhỏ hơn số lượng bạn bè. Ví dụ: nếu có 2 lát và 5 người bạn, mỗi người bạn sẽ không nhận được lát nào và tất cả các lát vẫn chưa được sử dụng. Một trường hợp góc khác là khi các lát chia đều, chẳng hạn như 9 lát cho 3 người bạn, không còn lại gì. Cả hai trường hợp phải được xử lý chính xác mà không cần phân nhánh đặc biệt ngoài số học số nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận mô phỏng trực tiếp sẽ chỉ định từng lát một theo kiểu vòng tròn giữa những người bạn. Điều này sẽ tiếp tục phân phối cho đến khi không còn lát nào. Mặc dù đơn giản về mặt khái niệm, nhưng trường hợp xấu nhất của nó yêu cầu lặp lại một lần trên mỗi lát, dẫn đến tối đa 10^9 thao tác, vượt xa giới hạn khả thi. 

Quan sát quan trọng là sự phân bố hoàn toàn đồng đều. Mỗi người bạn nhận được chính xác phần nguyên của việc chia tổng số lát cho số lượng bạn bè. Phần còn lại là phần không thể phân bổ đều. Điều này biến đổi quá trình từ mô phỏng lặp đi lặp lại thành một phép chia số học duy nhất. 

Brute-force hoạt động vì nó mô hình hóa rõ ràng quá trình phân phối, nhưng nó không thành công khi số lượng lát trở nên lớn. Nhận xét rằng chỉ có thương và số dư mới làm giảm bài toán thành các phép chia số nguyên và modulo, cả hai đều có thể tính toán được trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(x) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên, số lát và số bạn bè. Chúng đại diện cho tổng tài nguyên và số lượng phân vùng bằng nhau mà chúng ta muốn tạo. 
2. Tính xem mỗi người bạn nhận được bao nhiêu lát bằng phép chia số nguyên. Điều này nắm bắt được sự phân bổ bằng nhau lớn nhất có thể mà không vượt quá các lát có sẵn. 
3. Tính toán các lát còn sót lại bằng phép toán modulo. Điều này trực tiếp đại diện cho phần còn lại sau khi phân phối bằng nhau. 
4. Xuất ra thương số và số dư là kết quả cuối cùng. 

Lý do đằng sau mỗi bước gắn liền với cấu trúc toán học của phép chia. Phép chia số nguyên mã hóa sự chia đều bằng nhau một cách tự nhiên và modulo ghi lại những gì không thể chia đều. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi phân phối`x`cắt đều nhau giữa`N`các bạn ơi, tổng số lát được chỉ định phải là bội số lớn nhất của`N`điều đó không vượt quá`x`. Phép chia số nguyên tạo ra chính xác bội số này khi nhân lại với`N`. Phần còn lại là phần còn lại sau khi trừ đi phần chia hết tối đa này, đó chính xác là những gì modulo tính toán. Vì cả hai phép toán đều là sự phân rã chính xác của số ban đầu nên kết quả không thể sai lệch so với phân phối chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, n = map(int, input().split())

each = x // n
leftover = x % n

print(each, leftover)
```Giải pháp đọc đầu vào một lần và thực hiện ngay hai phép tính số học. Toán tử chia số nguyên`//`đảm bảo rằng các phần phân số bị loại bỏ, phù hợp với ý tưởng chỉ phân phối toàn bộ lát cắt. Toán tử modulo`%`tính toán những gì còn lại sau khi phân bổ này. 

Không cần vòng lặp hoặc nhánh có điều kiện vì số học mã hóa đầy đủ logic phân phối. Thứ tự của các phép toán rất đơn giản và không có vấn đề ranh giới nào ngoài việc đảm bảo số học số nguyên mà Python xử lý một cách tự nhiên đối với các giá trị lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
```| Bước | x | N | mỗi = x // N | còn sót lại = x %N | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 5 | 2 | - | - | 
| Tính toán chia | 5 | 2 | 2 | - | 
| Tính số dư | 5 | 2 | 2 | 1 | 

Đầu ra:```
2 1
```Điều này cho thấy mỗi người bạn nhận được hai lát và một lát vẫn chưa được sử dụng vì 5 không chia hết cho 2. 

### Ví dụ 2 

đầu vào:```
9 3
```| Bước | x | N | mỗi = x // N | còn sót lại = x %N | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 9 | 3 | - | - | 
| Tính toán chia | 9 | 3 | 3 | - | 
| Tính số dư | 9 | 3 | 3 | 0 | 

Đầu ra:```
3 0
```Điều này xác nhận rằng khi phép chia chính xác thì không còn lát cắt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học có thời gian không đổi mới được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Việc tính toán bao gồm một số thao tác cố định bất kể kích thước đầu vào, giúp dễ dàng thực hiện trong giới hạn ngay cả đối với các giới hạn tối đa là 10^9. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    x, n = map(int, sys.stdin.readline().split())
    print(x // n, x % n)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5 2\n") == "2 1", "sample 1"
assert run("9 3\n") == "3 0", "sample 2"

# custom cases
assert run("1 2\n") == "0 1", "fewer slices than friends"
assert run("10 1\n") == "10 0", "single friend gets everything"
assert run("1000000000 2\n") == "500000000 0", "large equal split"
assert run("7 3\n") == "2 1", "general remainder case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 0 1 | ít lát hơn bạn bè | 
| 10 1 | 10 0 | trường hợp cạnh người nhận duy nhất | 
| 1000000000 2 | 500000000 0 | kích thước đầu vào tối đa | 
| 7 3 | 2 1 | trường hợp tổng quát không chia hết | 

## Vỏ cạnh 

Khi số lát nhỏ hơn số lượng bạn bè, chẳng hạn như đầu vào`1 5`, phép chia số nguyên mang lại kết quả bằng 0, nghĩa là không có người bạn nào nhận được bất kỳ lát cắt nào. Phép toán modulo trả về số ban đầu, phản ánh chính xác rằng không có gì được phân phối. 

Khi số lượng bạn bè là một, chẳng hạn như`10 1`, phép chia mang lại tất cả các lát cho người bạn đó và phần còn lại bằng 0. Điều này xác nhận rằng toàn bộ tài nguyên được phân bổ chính xác mà không cần xử lý đặc biệt. 

Khi lát chia đều, chẳng hạn như`8 4`, phép chia số nguyên tạo ra phần chia sẻ chính xác cho mỗi người bạn và modulo trả về 0. Điều này cho thấy rằng không cần xử lý phần còn lại ngoài việc phân tách số học. 

Khi giá trị lớn, chẳng hạn như`10^9 2`, các thao tác vẫn hoạt động chính xác vì Python xử lý các số nguyên có kích thước tùy ý. Việc tính toán duy trì thời gian không đổi và không suy giảm theo cường độ đầu vào.
