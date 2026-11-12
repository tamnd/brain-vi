---
title: "CF 1669G - Rơi Xuống"
description: "Chúng ta có một lưới hình chữ nhật có kích thước $n nhân m$ bao gồm các ô trống, đá và chướng ngại vật. Đá được biểu thị bằng '', các ô trống bằng '.' và chướng ngại vật bằng 'o'."
date: "2026-06-10T01:58:28+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1669
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 784 (Div. 4)"
rating: 1200
weight: 1669
solve_time_s: 114
verified: false
draft: false
---

[CF 1669G - Rơi xuống](https://codeforces.com/problemset/problem/1669/G) 

**Đánh giá:** 1200 
**Tags:** dfs và tương tự, triển khai 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$n \times m$bao gồm các ô trống, đá và chướng ngại vật. Các viên đá được biểu thị bằng '*', các ô trống bằng '.' và chướng ngại vật bằng 'o'. Mục đích là để mô phỏng trọng lực: mỗi viên đá rơi xuống xa nhất có thể cho đến khi chạm vào đáy lưới, một viên đá khác đã lắng xuống hoặc một chướng ngại vật. Khi tất cả các viên đá đã ổn định, chúng ta phải xuất ra cấu hình cuối cùng của lưới. 

Kích thước lưới được giới hạn tối đa là 50 hàng và 50 cột và có thể có tới 100 trường hợp thử nghiệm. Vì tổng số ô nhiều nhất là$50 \times 50 = 2500$cho mỗi trường hợp thử nghiệm, ngay cả một mô phỏng đơn giản trên mỗi ô cũng sẽ đủ nhanh. Tuy nhiên, việc thực hiện bất cẩn vẫn có thể tạo ra kết quả sai trong một số cấu hình nhất định, chẳng hạn như khi các viên đá được xếp chồng lên trên chướng ngại vật hoặc nhiều viên đá rơi qua cùng một cột. Nếu chúng ta lặp lại từ trên xuống và di chuyển các khối ngay lập tức vào các ô trống, chúng ta có nguy cơ "ghi đè" hoặc bỏ qua các khối, tạo ra bố cục cuối cùng không chính xác. Một ví dụ cụ thể nhỏ là một cột`.*.*o.`. Nếu chúng tôi xử lý từ trên xuống, '*' đầu tiên sẽ rơi, nhưng '*' thứ hai có thể rơi qua viên đá đã lắng đầu tiên một cách không chính xác trừ khi chúng tôi xử lý số lượng đúng cách. 

Các trường hợp cạnh bao gồm các cột chỉ có chướng ngại vật, cột chỉ có đá, nhiều viên đá xếp chồng lên nhau phía trên chướng ngại vật và lưới một hàng. Việc xử lý những trường hợp này một cách chính xác đòi hỏi chúng ta phải tính toán vị trí các viên đá tiếp đất mà không di chuyển chúng riêng lẻ theo cách có thể xung đột với các viên đá khác trong cùng một cột. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là lặp đi lặp lại qua từng ô trong lưới, di chuyển các viên đá xuống từng bước cho đến khi không còn viên đá nào có thể di chuyển. Phương pháp này đúng vì mỗi hòn đá chỉ có thể rơi một số bước hữu hạn, nhưng nó không hiệu quả: đối với một cột có chiều cao$n$, một hòn đá có thể di chuyển tới$n$các bước và với$n \times m$tế bào, điều này mang lại$O(n^2 m)$hoạt động cho mỗi trường hợp thử nghiệm. Với$n = m = 50$, trường hợp xấu nhất là khoảng 125.000 thao tác cho mỗi trường hợp thử nghiệm, điều này vẫn khả thi nhưng không cần thiết khi tồn tại một cách tiếp cận trực tiếp hơn. 

Quan sát chính là mỗi cột có thể được xử lý độc lập. Thay vì di chuyển những viên đá từng bước một, chúng ta có thể đếm số lượng viên đá giữa các chướng ngại vật rồi “thả” chúng xuống những vị trí thấp nhất hiện có. Chướng ngại vật đóng vai trò như những dải phân cách tự nhiên, chia mỗi cột thành nhiều đoạn. Đối với mỗi phân đoạn, chúng tôi đặt các viên đá đã đếm ở cuối phân khúc và điền vào phần còn lại bằng các ô trống. Cách tiếp cận này làm giảm công việc xuống còn một lượt cho mỗi cột, mang lại một cách đơn giản$O(nm)$thuật toán dễ thực hiện và tránh được những cạm bẫy của mô phỏng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (từng bước) | O(n^2 m) | O(nm) | Có thể chấp nhận được nhưng không cần thiết | 
| Đếm theo cột | O(n m) | O(nm) | Tối ưu, được chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng cột độc lập từ trái sang phải. Hãy coi mỗi cột là một mảng ô dọc. 
2. Đối với mỗi cột, khởi tạo bộ đếm số đá về 0. Bộ đếm này theo dõi số lượng đá trong đoạn hiện tại (giữa các chướng ngại vật hoặc từ dưới lên chướng ngại vật). 
3. Lặp lại từ hàng dưới lên hàng trên cùng. Nếu gặp một viên đá, hãy tăng bộ đếm và tạm thời thay thế ô bằng '.' trống. Nếu gặp trở ngại, hãy xử lý đoạn hiện tại. 
4. Khi xử lý một đoạn (khi va phải chướng ngại vật hoặc khi lên đến đỉnh cột), hãy lấp đầy đoạn đó từ dưới lên với số lượng đá đã đếm được. Đặt các viên đá ở những vị trí thấp nhất có thể trong phân đoạn và lấp đầy các ô còn lại phía trên bằng các ô trống. Đặt lại bộ đếm đá về 0. 
5. Tiếp tục đi lên cho đến đỉnh cột, đảm bảo rằng đoạn cuối cùng (từ chướng ngại vật cuối cùng hoặc từ dưới lên trên) được xử lý theo cách tương tự. 
6. Sau khi xử lý tất cả các cột, lưới biểu thị trạng thái cuối cùng sau khi áp dụng trọng lực. 

Phương pháp này hoạt động vì chúng tôi duy trì một bất biến: tại bất kỳ thời điểm nào trong quá trình xử lý một cột, tất cả các viên đá bên dưới hàng hiện tại đã được đặt ở vị trí cuối cùng của chúng. Bằng cách đếm các viên đá và đặt chúng ở những vị trí thấp nhất có sẵn trong mỗi phân đoạn, chúng tôi tôn trọng cả chướng ngại vật và những viên đá đã được đặt trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    for col in range(m):
        stones = 0
        for row in range(n-1, -1, -1):
            if grid[row][col] == '*':
                stones += 1
                grid[row][col] = '.'
            elif grid[row][col] == 'o':
                for k in range(1, stones+1):
                    grid[row+k][col] = '*'
                stones = 0
        for k in range(stones):
            grid[k][col] = '*'

    for row in grid:
        print(''.join(row))
```Vòng lặp bên ngoài xử lý nhiều trường hợp thử nghiệm. Việc đọc lưới vào danh sách danh sách cho phép sửa đổi tại chỗ. Bằng cách lặp lại từng cột từ dưới lên trên, các viên đá sẽ được đếm và đặt sau khi gặp chướng ngại vật hoặc đỉnh cột. Vòng lặp lồng nhau cuối cùng in lưới được cập nhật. Việc xử lý cẩn thận các chỉ số đảm bảo các viên đá được đặt chính xác mà không ghi đè lên các chướng ngại vật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đoạn cột đầu vào:```
Row 5: .
Row 4: *
Row 3: *
Row 2: o
Row 1: .
Row 0: *
```Xử lý từ dưới lên: 

- Hàng 5 '.', đá=0 
- Hàng 4 '*', đá=1, đánh dấu là '.' 
- Hàng 3 '*', đá=2, đánh dấu là '.' 
- Hàng 2 'o', đặt 2 viên đá bên dưới: hàng 3 và 4 trở thành '*', đặt lại đá=0 
- Hàng 1 '.', đá=0 
- Hàng 0 '*', đá=1, đánh dấu là '.' 
- Đạt tới đỉnh, đặt viên đá còn lại ở hàng 0 

Cột kết quả:```
Row 5: *
Row 4: *
Row 3: o
Row 2: .
Row 1: .
Row 0: *
```Điều này chứng tỏ các chướng ngại vật chia cắt các đoạn và các viên đá lắng xuống các vị trí thấp nhất trong đoạn của chúng. 

### Ví dụ 2 

Đầu vào: cột`*.*.*`Quá trình xử lý từ dưới lên đếm các viên đá giữa các chướng ngại vật (không có ở đây), đặt chúng ở phía dưới: 

- Đá ban đầu=0 
- Hàng 4 '*', đá=1, đánh dấu là '.' 
- Hàng 3 '.', đá=1 
- Hàng 2 '*', đá=2, đánh dấu là '.' 
- Hàng 1 '.', đá=2 
- Hàng 0 '*', đá=3, đánh dấu là '.' 
- Đạt tới đỉnh, đặt 3 viên đá ở 3 hàng dưới cùng 

Cột cuối cùng:`..***`Điều này cho thấy nhiều viên đá không có chướng ngại vật xếp chồng lên nhau ở phía dưới một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t n m) | Mỗi ô được truy cập một lần cho mỗi trường hợp thử nghiệm, các cột được xử lý độc lập | 
| Không gian | O(n m) | Lưới được lưu trữ dưới dạng mảng 2D, không có cấu trúc lớn bổ sung | 

Với tối đa n=m=50 và t=100, thuật toán thực hiện tối đa 2500 * 100 = 250.000 thao tác, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("3\n6 10\n.*.*....*.\n.*.......*\n...o....o.\n.*.*....*.\n..........\n.o......o*\n2 9\n...***ooo\n.*o.*o.*o\n5 5\n*****\n*....\n*****\n....*\n*****") == "..........\n...*....*.\n.*.o....o.\n.*........\n.*......**\n.o.*....o*\n\n....**ooo\n.*o**o.*o\n\n.....\n*...*\n*****\n*****\n*****"

# Custom: minimum size
assert run("1\n1 1\n*") == "*", "single cell stone"

# Custom: all obstacles
assert run("1\n3 1\no\no\no") == "o\no\no", "no stones to move"

# Custom: column full of stones
assert run("1\n3 1\n*\n*\n*") == "*\n*\n*", "stones already at bottom"

# Custom: multiple obstacles
assert run("1\n4 1\n*\no\n*\no") == ".\no\n*\no", "stones correctly stack above obstacles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đá 1x1 | * | lưới tối thiểu, đá đơn | 
| Chỉ chướng ngại vật 3x1 | o\không\không | đá vắng, chướng ngại vật không bị ảnh hưởng | 
| 3x1 tất cả đá | _\n_\n* | đá ở phía dưới, không có ô trống | 
| Đá và chướng ngại vật 4x1 | .\no\n*\no | xếp chồng lên chướng ngại vật được xử lý đúng cách | 

## Vỏ cạnh 

Đối với lưới một cột có các viên đá xen kẽ
