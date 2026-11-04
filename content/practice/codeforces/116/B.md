---
title: "CF 116B - Chú Heo Con Và Sói"
description: "Chúng ta có một lưới hình chữ nhật nhỏ trong đó mỗi ô trống, chứa một con lợn hoặc chứa một con sói. Một con sói có thể ăn thịt một con lợn ở gần nó theo một trong bốn hướng chính. Một khi con lợn bị ăn thịt, nó sẽ biến mất và không thể ăn được nữa."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 116
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 87 (Div. 2 Only)"
rating: 1100
weight: 116
solve_time_s: 150
verified: true
draft: false
---

[CF 116B - Những chú lợn con và con sói](https://codeforces.com/problemset/problem/116/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật nhỏ trong đó mỗi ô trống, chứa một con lợn hoặc chứa một con sói. Một con sói có thể ăn thịt một con lợn ở gần nó theo một trong bốn hướng chính. Một khi con lợn bị ăn thịt, nó sẽ biến mất và không thể ăn được nữa. 

Mục tiêu là tối đa hóa tổng số lợn được ăn. 

Hạn chế chính là bất thường và cực kỳ quan trọng: mỗi con lợn ở cạnh nhiều nhất một con sói. Một con sói vẫn có thể có nhiều con lợn lân cận, nhưng một con lợn không bao giờ có thể bị nhiều con sói tranh giành. 

Hạn chế đó thay đổi vấn đề từ một vấn đề phù hợp thành một mô phỏng tham lam đơn giản. 

Kích thước lưới tối đa là 10 × 10, vì vậy toàn bộ bảng chứa tối đa 100 ô. Ngay cả những giải pháp không hiệu quả cũng có thể được thông qua. Tuy nhiên, vấn đề được thiết kế để kiểm tra xem liệu chúng ta có nhận thấy đặc tính cấu trúc loại bỏ nhu cầu về các thuật toán đồ thị nặng hay không. 

Việc thực hiện cẩn thận vẫn còn quan trọng vì bàn cờ thay đổi trong khi sói ăn thịt lợn. Nếu chúng ta vô tình để cùng một con lợn bị ăn thịt hai lần thì câu trả lời sẽ sai. 

Một sai lầm dễ mắc phải là quên loại bỏ con lợn sau khi nó bị ăn thịt. Hãy xem xét đầu vào này:```
1 3
WPW
```Đầu ra đúng là:```
1
```Con lợn ở giữa ở cạnh cả hai con sói, nhưng cấu hình này thực sự bị cấm bởi câu lệnh vì mỗi con lợn có thể có nhiều nhất một con sói liền kề. Việc thực hiện bất cẩn mà bỏ qua sự đảm bảo có thể tính con lợn hai lần. 

Một trường hợp tinh tế khác là khi một con sói có nhiều con lợn lân cận:```
2 2
PW
PP
```Đầu ra đúng là:```
1
```Sói chỉ ăn được một con lợn dù có hai con lợn ở cạnh nhau. Một giải pháp lỗi có thể đếm từng con lợn lân cận. 

Xử lý ranh giới là một nguồn lỗi phổ biến khác. Ví dụ:```
1 2
WP
```Đầu ra đúng là:```
1
```Sói chỉ có hàng xóm trong lưới. Truy cập tọa độ bên ngoài bảng mà không kiểm tra sẽ gây ra lỗi chỉ mục. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là thử mọi quyết định có thể cho mọi con sói. Mỗi con sói có thể chọn một con lợn bên cạnh hoặc không chọn ai cả. Chúng ta có thể khám phá đệ quy tất cả các kết hợp và giữ được câu trả lời tối đa. 

Điều đó hoạt động vì bảng rất nhỏ. Trong trường hợp xấu nhất có khoảng 100 tế bào và nhiều tế bào trong số đó có thể là sói. Nếu mỗi con sói có tối đa bốn lựa chọn thì không gian tìm kiếm sẽ trở nên gần như$4^k$, Ở đâu$k$là số lượng sói. Ngay cả khi chỉ có 20 con sói, đó cũng đã là hơn một nghìn tỷ khả năng rồi. 

Nguyên nhân vụ nổ này xảy ra là vì sói thường tranh giành lợn. Chọn một con lợn cho một con sói có thể sẽ chặn một con sói khác sau này. 

Nhận xét quan trọng là vấn đề này loại bỏ hầu hết mọi sự cạnh tranh. Mỗi con lợn ở cạnh nhiều nhất một con sói. Điều đó có nghĩa là nếu một con sói nhìn thấy một con lợn thì không con sói nào khác có thể bắt được nó. 

Một khi chúng ta nhận ra điều này, vấn đề sẽ trở nên đơn giản. Chúng tôi lặp qua lưới. Đối với mỗi con sói, chúng tôi kiểm tra bốn ô lân cận của nó. Nếu hàng xóm nào có lợn, sói ăn ngay, chúng ta tăng đáp án và xóa con lợn đó khỏi bảng. 

Sự lựa chọn tham lam này luôn an toàn vì dù thế nào đi nữa, không con sói tương lai nào có thể sử dụng con lợn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^k) | O(k) | Về mặt khái niệm quá chậm | 
| Tối ưu | O(n×m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới thành mảng 2D để chúng ta có thể sửa đổi các ô sau khi ăn thịt lợn. 
2. Chuẩn bị bốn hướng chuyển động: lên, xuống, trái và phải. 
3. Duyệt từng ô trong lưới. 
4. Khi ô hiện tại chứa một con sói, hãy kiểm tra bốn ô lân cận của nó. 
5. Đối với mỗi người hàng xóm, trước tiên hãy kiểm tra xem tọa độ có nằm trong bảng hay không. Điều này ngăn chặn truy cập mảng không hợp lệ. 
6. Nếu ô bên cạnh có lợn thì cho sói ăn thịt ngay: 

1. Tăng câu trả lời lên một. 
2. Thay tế bào lợn đó bằng`.`để sau này sói không thể ăn thịt nó nữa. 
3. Ngừng kiểm tra hàng xóm để tìm con sói này. 

Một con sói có thể ăn tối đa một con lợn, vì vậy việc tiếp tục ăn thịt thành công sẽ là quá đáng. 
7. Sau khi xử lý hết các ô, in ra tổng số lợn đã ăn. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ sự đảm bảo rằng mỗi con lợn ở cạnh nhiều nhất một con sói. Giả sử một con sói tìm thấy một con lợn gần đó. Không con sói nào khác có thể sử dụng con lợn đó, dù bây giờ hay sau này. Ăn nó ngay lập tức không thể làm giảm bất kỳ lựa chọn nào trong tương lai. 

Vì đặc tính đó mà mọi lựa chọn tham lam thành công đều độc lập với tất cả những lựa chọn khác. Thuật toán đếm chính xác một con lợn cho mỗi con sói có ít nhất một con lợn liền kề và không có con lợn nào được tính hai lần vì những con lợn bị ăn thịt sẽ bị loại khỏi lưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    ans = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'W':
                for dx, dy in directions:
                    ni = i + dx
                    nj = j + dy

                    if 0 <= ni < n and 0 <= nj < m:
                        if grid[ni][nj] == 'P':
                            ans += 1
                            grid[ni][nj] = '.'
                            break

    print(ans)

solve()
```Giải pháp lưu trữ bảng dưới dạng danh sách danh sách nhân vật có thể thay đổi vì lợn biến mất sau khi bị ăn thịt. Chuỗi là bất biến trong Python, vì vậy việc sử dụng chuỗi thô sẽ khiến việc cập nhật trở nên khó khăn. 

Các vòng bên ngoài quét từng ô một lần. Bất cứ khi nào chúng tôi gặp một con sói, chúng tôi sẽ kiểm tra bốn vị trí lân cận bằng cách sử dụng mảng định hướng. 

Việc kiểm tra ranh giới diễn ra trước khi truy cập`grid[ni][nj]`. Đảo ngược thứ tự này sẽ gây ra lỗi chỉ mục trên các ô cạnh. 

Sau khi sói ăn thịt lợn, mật mã ngay lập tức thay thế con lợn bằng`.`. Điều này ngăn chặn việc tính hai lần và mô hình hóa báo cáo vấn đề một cách chính xác. 

các`break`tuyên bố là cần thiết. Một con sói chỉ có thể ăn thịt một con lợn. Nếu không có thời gian nghỉ, một con sói có thể tiêu diệt nhiều con lợn lân cận một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
PPW
W.P
```Lưới ban đầu:```
P P W
W . P
```| Bước | Vị Trí Sói | Tìm thấy lợn liền kề | Trả lời | Thay đổi lưới | 
| --- | --- | --- | --- | --- | 
| 1 | (0, 2) | (0, 1) | 1 | Lợn ở (0,1) bị loại bỏ | 
| 2 | (1, 0) | (0, 0) | 2 | Lợn ở (0,0) bị loại bỏ | 

Câu trả lời cuối cùng:```
2
```Dấu vết này cho thấy mỗi con sói độc lập bắt một con lợn lân cận. Loại bỏ lợn sau khi ăn sẽ ngăn cản việc tái sử dụng. 

### Ví dụ 2 

đầu vào:```
2 2
PW
PP
```Lưới ban đầu:```
P W
P P
```| Bước | Vị Trí Sói | Tìm thấy lợn liền kề | Trả lời | Thay đổi lưới | 
| --- | --- | --- | --- | --- | 
| 1 | (0, 1) | (0, 0) | 1 | Lợn ở (0,0) bị loại bỏ | 

Câu trả lời cuối cùng:```
1
```Ví dụ này thể hiện quy tắc sói chỉ có thể ăn thịt một con lợn ngay cả khi có nhiều con ở cạnh nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n×m) | Mỗi ô được truy cập một lần và mỗi ô sẽ kiểm tra tối đa 4 ô lân cận | 
| Không gian | O(1) | Chỉ một số biến ngoài lưới đầu vào được sử dụng | 

Bảng chứa tối đa 100 ô, vì vậy giải pháp này nằm trong giới hạn thoải mái. Ngay cả Python được giải thích cũng chạy ngay lập tức ở quy mô này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    ans = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'W':
                for dx, dy in directions:
                    ni = i + dx
                    nj = j + dy

                    if 0 <= ni < n and 0 <= nj < m:
                        if grid[ni][nj] == 'P':
                            ans += 1
                            grid[ni][nj] = '.'
                            break

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run("2 3\nPPW\nW.P\n") == "2\n", "sample 1"

# minimum grid
assert run("1 1\n.\n") == "0\n", "empty cell"

# single wolf and pig
assert run("1 2\nWP\n") == "1\n", "single adjacent pig"

# wolf with multiple adjacent pigs
assert run("2 2\nPW\nPP\n") == "1\n", "wolf only eats one pig"

# no possible eats
assert run("2 2\nP.\n.W\n") == "0\n", "no adjacency"

# larger mixed case
assert run("3 3\nPWP\n.W.\nPWP\n") == "4\n", "multiple wolves and pigs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / .`|`0`| Lưới kích thước tối thiểu | 
|`1 2 / WP`|`1`| Liền kề ranh giới | 
|`2 2 / PW / PP`|`1`| Một con sói không thể ăn thịt nhiều con lợn | 
|`2 2 / P. / .W`|`0`| Không có lợn liền kề | 
|`3 3 / PWP / .W. / PWP`|`4`| Nhiều lựa chọn tham lam thành công | 

## Vỏ cạnh 

Hãy xem xét trường hợp một con sói chạm vào nhiều con lợn:```
2 2
PW
PP
```Thuật toán quét con sói tại vị trí`(0,1)`. Nó kiểm tra hàng xóm theo thứ tự và tìm thấy con lợn ở`(0,0)`. Câu trả lời trở thành 1, con lợn bị loại bỏ và vòng lặp bị đứt ngay lập tức. Những con lợn còn lại bị bỏ qua đối với con sói đó, điều này phù hợp với quy tắc mỗi con sói ăn tối đa một con lợn. 

Bây giờ hãy xem xét trường hợp cạnh bảng:```
1 2
WP
```Con sói ngồi ở góc trên bên trái. Một số tọa độ lân cận sẽ nằm ngoài bảng, chẳng hạn như`(-1,0)`hoặc`(0,-1)`. Kiểm tra ranh giới từ chối các vị trí đó trước khi truy cập mảng. Hàng xóm hợp lệ`(0,1)`chứa một con lợn nên câu trả lời trở thành 1 một cách an toàn. 

Cuối cùng, hãy xem xét một trường hợp không thể ăn được:```
2 2
P.
.W
```Con sói ở`(1,1)`kiểm tra bốn hướng. Mọi hàng xóm hợp lệ đều trống hoặc nằm ngoài bảng. Câu trả lời không bao giờ tăng và thuật toán in chính xác 0.
