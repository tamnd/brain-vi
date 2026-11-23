---
title: "CF 46A - Trò chơi bóng"
description: "Trẻ đứng thành vòng tròn được đánh số từ 1 đến n. Trẻ 1 bắt đầu với quả bóng. Lần ném đầu tiên đưa bóng về phía trước 1 vị trí, lần ném thứ hai đưa bóng về phía trước 2 vị trí, lần ném thứ ba đưa bóng về phía trước 3 vị trí, v.v."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 46
codeforces_index: "A"
codeforces_contest_name: "School Personal Contest #2 (Winter Computer School 2010/11) - Codeforces Beta Round 43 (ACM-ICPC Rules)"
rating: 800
weight: 46
solve_time_s: 83
verified: true
draft: false
---

[CF 46A - Trò chơi bóng](https://codeforces.com/problemset/problem/46/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trẻ đứng thành vòng tròn được đánh số từ`1`ĐẾN`n`. Đứa trẻ`1`bắt đầu với quả bóng. Cú ném đầu tiên sẽ đưa bóng về phía trước`1`vị trí, lần ném thứ hai sẽ di chuyển nó về phía trước`2`vị trí, lần ném thứ ba`3`các vị trí, v.v. Sau chính xác`n - 1`ném, chúng ta phải in ra đứa trẻ nào nhận được bóng sau mỗi lần ném. 

Chuyển động quấn quanh vòng tròn. Nếu chúng ta đi qua đứa trẻ`n`, chúng ta tiếp tục lại từ đứa trẻ`1`. Đây là chuyển động tròn cổ điển nên số học mô-đun là công cụ tự nhiên ở đây. 

Hạn chế là rất nhỏ,`n ≤ 100`. Ngay cả một mô phỏng không hiệu quả cũng sẽ dễ dàng phù hợp với thời gian giới hạn. Một giải pháp bậc hai sẽ thực hiện xung quanh`10^4`hoạt động, đó là rất nhỏ. Thách thức không phải là hiệu suất, mà là xử lý việc lập chỉ mục vòng tròn một cách chính xác. 

Lỗi phổ biến nhất là trộn lẫn đánh số dựa trên 0 và dựa trên 1. Các em được đánh số bắt đầu từ`1`, nhưng số học mô-đun thường dễ dàng hơn với các chỉ số dựa trên 0. 

Coi như`n = 5`. 

Thứ tự di chuyển là: 

- Bắt đầu lúc`1`- Di chuyển`1`bước →`2`- Di chuyển`2`bước →`4`- Di chuyển`3`bước →`2`- Di chuyển`4`bước →`1`Đầu ra đúng là:```
2 4 2 1
```Việc thực hiện bất cẩn có thể tính toán:```
pos = (pos + step) % n
```trong khi giữ`pos`ở dạng dựa trên 1. Điều đó tạo ra đứa trẻ không hợp lệ`0`khi vị trí kết thúc xung quanh. 

Một sai lầm dễ mắc phải khác là hiểu sai về khoảng cách. trên`k`-lần ném thứ , chúng ta di chuyển chính xác`k`vị trí phía trước, không`k + 1`. 

Ví dụ, với`n = 4`:```
Start at 1
Throw 1: move 1 → 2
Throw 2: move 2 → 4
Throw 3: move 3 → 3
```Đầu ra đúng:```
2 4 3
```Nếu ai đó tăng bước quá sớm, họ sẽ di chuyển sai`2, 3, 4`. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp theo đúng nghĩa đen của trò chơi. Chúng tôi lưu trữ đứa trẻ hiện tại và liên tục di chuyển về phía trước theo số lượng vị trí cần thiết. Vì bọn trẻ tạo thành một vòng tròn nên sau mỗi lần di chuyển, chúng tôi sẽ xoay vòng bằng cách sử dụng số học modulo. 

Một cách để thực hiện điều này là mô phỏng vật lý từng bước chuyển động. Đối với`k`-lần ném thứ , chúng ta tiến lên chính xác từng vị trí một`k`lần. Tổng số chuyển động một bước trở thành:```
1 + 2 + 3 + ... + (n - 1)
```đó là`O(n²)`. 

Với`n = 100`, điều này vẫn chạy ngay lập tức, vì vậy ngay cả mô phỏng đơn giản cũng được chấp nhận. 

Giải pháp sạch hơn đến từ việc nhận thấy rằng chúng ta thực sự không bao giờ cần các vị trí trung gian. Chúng tôi chỉ quan tâm đến đứa con cuối cùng sau mỗi lần ném. Chuyển động tròn có thể được biểu diễn trực tiếp bằng số học mô-đun. 

Nếu vị trí hiện tại là`pos`và độ dài lần ném tiếp theo là`step`, sau đó:```
new_position = (pos + step) % n
```khi sử dụng lập chỉ mục dựa trên 0. 

Điều này làm giảm thời gian mỗi lần ném xuống không đổi, tạo ra một`O(n)`giải pháp. 

Lực lượng vũ phu hoạt động vì các ràng buộc rất nhỏ, nhưng số học mô-đun nắm bắt trực tiếp cấu trúc của chuyển động tròn và tạo ra cách triển khai đơn giản hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số con`n`. 
2. Lưu trữ vị trí hiện tại dưới dạng`0`, đại diện cho con`1`trong lập chỉ mục dựa trên 0. 

Việc sử dụng các chỉ số dựa trên 0 làm cho các hoạt động modulo trở nên đơn giản vì các vị trí tự nhiên nằm trong phạm vi`[0, n - 1]`. 
3. Lặp lại`step`từ`1`ĐẾN`n - 1`. 

các`step`giá trị khớp trực tiếp với số lần ném và số vị trí được di chuyển. 
4. Cập nhật vị trí hiện tại:```
pos = (pos + step) % n
```Hoạt động modulo kết thúc chuyển động xung quanh vòng tròn bất cứ khi nào chúng ta vượt qua thành phần con cuối cùng. 
5. Chuyển đổi vị trí về đánh số 1 bằng cách in`pos + 1`. 
6. Tiếp tục cho đến hết`n - 1`các cú ném được xử lý. 

### Tại sao nó hoạt động 

Sau mỗi lần lặp,`pos`lưu trữ người giữ bóng hiện tại theo chỉ mục dựa trên 0. trên`k`- lần ném, luật yêu cầu di chuyển chính xác`k`các vị trí theo chiều kim đồng hồ. Thêm`k`đến vị trí hiện tại thực hiện chuyển động này và modulo`n`xử lý chính xác việc bao quanh vòng tròn. Vì mỗi bản cập nhật khớp chính xác với một lần ném trong trò chơi nên trình tự được tạo ra chính xác là trình tự trẻ nhận bóng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

pos = 0
ans = []

for step in range(1, n):
    pos = (pos + step) % n
    ans.append(str(pos + 1))

print(" ".join(ans))
```Biến`pos`lưu trữ con hiện tại bằng cách sử dụng chỉ mục dựa trên 0. Đứa trẻ`1`được biểu diễn dưới dạng`0`, đứa trẻ`2`BẰNG`1`, vân vân. 

Đối với mỗi số lần ném`step`, chúng ta tiến về phía trước đúng bằng số vị trí đó. Biểu thức:```
(pos + step) % n
```giữ vị trí bên trong phạm vi vòng tròn hợp lệ. 

Đầu ra phải sử dụng cách đánh số dựa trên 1 ban đầu, vì vậy chúng tôi in`pos + 1`. 

Một chi tiết tinh tế là thứ tự các thao tác. Đầu tiên chúng tôi cập nhật vị trí, sau đó in nó. Việc in trước khi cập nhật sẽ bao gồm không chính xác đứa trẻ ném quả bóng chứ không phải đứa trẻ nhận được nó. 

Một chi tiết quan trọng khác là bắt đầu vòng lặp từ`1`. Lần ném đầu tiên di chuyển theo một vị trí, không phải số 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
```| Ném | Kích thước bước | Vị trí trước đây | Vị trí mới (dựa trên 0) | Trẻ in | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 2 | 
| 2 | 2 | 1 | 3 | 4 | 
| 3 | 3 | 3 | 6 | 7 | 
| 4 | 4 | 6 | 0 | 1 | 
| 5 | 5 | 0 | 5 | 6 | 
| 6 | 6 | 5 | 1 | 2 | 
| 7 | 7 | 1 | 8 | 9 | 
| 8 | 8 | 8 | 6 | 7 | 
| 9 | 9 | 6 | 5 | 6 | 

Đầu ra:```
2 4 7 1 6 2 9 7 6
```Dấu vết này cho thấy cách số học modulo xử lý sự bao bọc một cách tự nhiên. Khi ném`4`, chức vụ`6 + 4 = 10`, Và`10 % 10 = 0`, trả về chính xác cho con`1`. 

### Ví dụ 2 

đầu vào:```
5
```| Ném | Kích thước bước | Vị trí trước đây | Vị trí mới (dựa trên 0) | Trẻ in | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 2 | 
| 2 | 2 | 1 | 3 | 4 | 
| 3 | 3 | 3 | 1 | 2 | 
| 4 | 4 | 1 | 0 | 1 | 

Đầu ra:```
2 4 2 1
```Ví dụ này thực hiện các thao tác bao quanh lặp đi lặp lại. Lần ném thứ ba di chuyển từ vị trí`3`qua`3`nhiều bước hơn, sản xuất`6 % 5 = 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một bản cập nhật liên tục cho mỗi`n - 1`ném | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ ngoài danh sách đầu ra | 

Với`n ≤ 100`, thuật toán dễ dàng phù hợp trong giới hạn. Ngay cả một giải pháp bậc hai cũng có thể vượt qua, nhưng cách tiếp cận số học mô-đun vừa sạch hơn vừa tối ưu về mặt tiệm cận. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())

    pos = 0
    ans = []

    for step in range(1, n):
        pos = (pos + step) % n
        ans.append(str(pos + 1))

    print(" ".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("10\n") == "2 4 7 1 6 2 9 7 6", "sample 1"

# minimum size
assert run("2\n") == "2", "minimum n"

# small wraparound case
assert run("5\n") == "2 4 2 1", "multiple wraparounds"

# off-by-one check
assert run("4\n") == "2 4 3", "correct step sizes"

# larger case
assert run("7\n") == "2 4 7 4 2 1", "general circular movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`2`| Đầu vào hợp lệ nhỏ nhất | 
|`5`|`2 4 2 1`| Hành vi bao quanh lặp đi lặp lại | 
|`4`|`2 4 3`| Đếm và lập chỉ mục bước chính xác | 
|`7`|`2 4 7 4 2 1`| Mô phỏng vòng tròn chung | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
2
```Chỉ có một lần ném. 

Thuật toán bắt đầu bằng:```
pos = 0
```Vì`step = 1`:```
pos = (0 + 1) % 2 = 1
```Chúng tôi in`1 + 1 = 2`. 

Đầu ra:```
2
```Điều này xác nhận rằng ranh giới vòng lặp là chính xác và chúng tôi thực hiện chính xác`n - 1`ném. 

Bây giờ hãy xem xét một trường hợp có kết quả ngay lập tức:```
5
```Trong lần ném thứ tư, vị trí hiện tại là:```
pos = 1
```Việc di chuyển là:```
(1 + 4) % 5 = 0
```Chức vụ`0`tương ứng với con`1`. 

Đầu ra ở bước này:```
1
```Điều này xác minh rằng số học modulo quay trở lại phần đầu của vòng tròn một cách chính xác. 

Cuối cùng, hãy xem xét một trường hợp phát hiện lỗi lập chỉ mục:```
4
```Trình tự đúng:```
2 4 3
```Nếu ai đó sử dụng lập chỉ mục dựa trên 1 trực tiếp bên trong biểu thức modulo, họ có thể vô tình tạo ra con`0`. Việc triển khai của chúng tôi tránh hoàn toàn điều này bằng cách lưu trữ các vị trí nội bộ dưới dạng giá trị dựa trên 0 và chỉ chuyển đổi ngược lại khi in.
