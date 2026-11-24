---
title: "CF 919C - Sắp xếp chỗ ngồi"
description: "Chúng ta có một lớp học được biểu diễn dưới dạng lưới. Mỗi ô trống (.) hoặc bị chiếm (). Chúng ta muốn xếp chỗ chính xác k học sinh trên một đường thẳng. Các ghế phải liên tiếp và phải nằm hoàn toàn trong một hàng hoặc hoàn toàn trong một cột."
date: "2026-06-13T02:43:10+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 919
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 460 (Div. 2)"
rating: 1300
weight: 919
solve_time_s: 832
verified: true
draft: false
---

[CF 919C - Sắp xếp chỗ ngồi](https://codeforces.com/problemset/problem/919/C) 

**Đánh giá:** 1300 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 13m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lớp học được biểu diễn dưới dạng lưới. Mỗi ô đều trống (`.`) hoặc bị chiếm đóng (`*`). 

Chúng tôi muốn ngồi chính xác`k`học sinh thẳng hàng. Các ghế phải liên tiếp và phải nằm hoàn toàn trong một hàng hoặc hoàn toàn trong một cột. Mọi chỗ ngồi trong chặng đã chọn đó phải trống. 

Nhiệm vụ là đếm có bao nhiêu phân đoạn hợp lệ riêng biệt tồn tại. 

Một phân đoạn được xác định bởi tập hợp các ô bị chiếm giữ. Ví dụ: nếu một hàng chứa bốn ghế trống liên tiếp và`k = 2`, thì các khoảng`[1,2]`,`[2,3]`, Và`[3,4]`là ba cách sắp xếp khác nhau. 

Kích thước lưới có thể lớn bằng`2000 × 2000`. Điều đó có nghĩa là lưới có thể chứa tới bốn triệu ô. Bất kỳ giải pháp nào quét liên tục các ô giống nhau hoặc kiểm tra từng phân đoạn có thể một cách độc lập sẽ trở nên quá chậm. Một giải pháp thành công sẽ chỉ xử lý mỗi ô một số lần không đổi, dẫn đến khoảng`O(nm)`công việc. 

Có một số trường hợp đáng chú ý. 

Coi như`k = 1`:```
2 2 1
..
..
```Mỗi ô trống đồng thời là một phân đoạn ngang hợp lệ và một phân đoạn dọc hợp lệ. Câu trả lời đúng là`4`, không`8`. Nếu chúng ta đếm hàng và cột độc lập thì mỗi ô trống sẽ được tính hai lần. 

Một trường hợp quan trọng khác xảy ra khi số ghế trống kéo dài hơn`k`.```
1 5 3
.....
```Các phân đoạn hợp lệ là:```
[1,2,3]
[2,3,4]
[3,4,5]
```Câu trả lời là`3`. Việc triển khai bất cẩn có thể chỉ tính một phân đoạn cho mỗi lần chạy. 

Các ô bị chặn sẽ chạy thành các phần độc lập.```
1 5 2
..*..
```Đường chạy bên trái đóng góp một cách sắp xếp, đường chạy bên phải đóng góp một cách sắp xếp, tổng cộng là`2`. Xử lý toàn bộ hàng như một lần chạy sẽ tạo ra không chính xác`4`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Đối với mỗi ô, chúng ta có thể thử bắt đầu một đoạn có chiều dài nằm ngang`k`và kiểm tra xem tất cả`k`các ô trống. Chúng ta có thể làm tương tự theo chiều dọc. 

Điều này đúng vì mọi phân đoạn có thể đều được kiểm tra chính xác một lần. Vấn đề là thời gian chạy. có`O(nm)`các vị trí bắt đầu có thể và mỗi lần kiểm tra có thể kiểm tra tới`k`tế bào. Trong trường hợp xấu nhất điều này trở thành`O(nmk)`. Với tất cả các kích thước đạt`2000`, có thể tiếp cận hàng tỷ hoạt động. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến từng tế bào. Điều quan trọng là số lượng ghế trống liên tiếp tối đa. 

Giả sử một hàng chứa một chuỗi độ dài`L`.```
.....   (L = 5)
```Nếu chúng ta cần`k = 3`, thì mỗi khối có độ dài liên tiếp`3`bên trong lần chạy này là hợp lệ. Số khối như vậy là:```
L - k + 1
```cung cấp`L ≥ k`. 

Thay vì kiểm tra từng phân đoạn ứng cử viên một cách riêng biệt, chúng tôi quét hàng một lần, xác định từng lần chạy trống và ngay lập tức thêm phần đóng góp của nó. Ý tưởng tương tự cũng áp dụng cho các cột. 

Có một tình huống đặc biệt:`k = 1`. 

Đối với lớn hơn`k`, đoạn ngang và đoạn dọc là những đối tượng khác nhau nên việc đếm hàng và cột độc lập là đúng. 

Vì`k = 1`, mọi sắp xếp hợp lệ đều bao gồm một ô trống. Việc đếm các hàng và cột riêng biệt sẽ đếm mỗi ô trống hai lần. Trong trường hợp đó, chúng ta chỉ cần đếm các ô trống một lần và dừng lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nmk) | O(1) | Quá chậm | 
| Tối ưu | O(nm) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới. 
2. Nếu`k = 1`, đếm số ô chứa`.`và xuất ra giá trị đó. 

Mỗi ô trống thể hiện chính xác một sự sắp xếp hợp lệ. Đếm hàng và cột riêng biệt sẽ được tính gấp đôi. 
3. Quét từng hàng từ trái sang phải. 

Duy trì độ dài của lần chạy liên tiếp hiện tại của các ô trống. 
4. Bất cứ khi nào một`*`gặp phải, quá trình chạy hiện tại kết thúc. 

Nếu chiều dài chạy là`L ≥ k`, thêm vào`L - k + 1`cho câu trả lời, bởi vì độ dài đó-`k`các phân đoạn phù hợp bên trong đường chạy. 
5. Sau khi kết thúc hàng, xử lý lần chạy cuối cùng theo cách tương tự. 

Một hàng có thể kết thúc bằng các ô trống, vì vậy lần chạy cuối cùng cần xử lý riêng. 
6. Lặp lại quy trình tương tự cho mỗi cột. 

Mỗi cột được coi là một chuỗi một chiều. Mỗi đường chạy dọc đều góp phần`L - k + 1`sắp xếp. 
7. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Mọi sắp xếp theo chiều ngang hợp lệ đều thuộc về chính xác một dãy ô trống theo chiều ngang tối đa. Nếu lần chạy đó có độ dài`L`, các vị trí bắt đầu có thể có của chiều dài-`k`phân đoạn là:```
1, 2, ..., L-k+1
```cung cấp chính xác`L-k+1`sắp xếp. 

Thuật toán đếm chính xác những sự sắp xếp đó cho mỗi lần chạy ngang và mỗi lần chạy dọc. Không có phân đoạn hợp lệ nào bị bỏ sót vì mọi phân đoạn đều nằm trong một số lần chạy. Không có phân đoạn nào được tính hai lần vì một phân đoạn có chính xác một lần chạy chứa tối đa. Việc xử lý đặc biệt của`k = 1`loại bỏ trường hợp duy nhất mà việc đếm ngang và đếm dọc sẽ chồng chéo lên nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    if k == 1:
        ans = sum(row.count('.') for row in grid)
        print(ans)
        return

    ans = 0

    # Rows
    for r in range(n):
        run = 0
        for c in range(m):
            if grid[r][c] == '.':
                run += 1
            else:
                if run >= k:
                    ans += run - k + 1
                run = 0

        if run >= k:
            ans += run - k + 1

    # Columns
    for c in range(m):
        run = 0
        for r in range(n):
            if grid[r][c] == '.':
                run += 1
            else:
                if run >= k:
                    ans += run - k + 1
                run = 0

        if run >= k:
            ans += run - k + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý trường hợp đặc biệt`k = 1`. Mỗi ô trống là một sự sắp xếp hợp lệ, vì vậy việc đếm trực tiếp các ô trống vừa đơn giản hơn vừa tránh được việc đếm hai lần. 

Quá trình quét hàng coi mỗi hàng là một mảng một chiều. Biến`run`lưu trữ chiều dài của khối ghế trống liên tiếp hiện tại. Bất cứ khi nào một chỗ ngồi bị chặn xuất hiện, cuộc chạy sẽ kết thúc và đóng góp`run - k + 1`phân đoạn nếu chiều dài của nó đủ lớn. 

Việc quét cột giống hệt nhau ngoại trừ các chỉ số được duyệt theo chiều dọc. 

Một nguồn lỗi phổ biến là quên xử lý lần chạy cuối cùng sau khi hoàn thành một hàng hoặc cột. Quá trình chạy có thể tiếp tục cho đến khi đến ranh giới, vì vậy phần đóng góp cũng phải được thêm vào sau vòng lặp. 

Một sai lầm dễ mắc nữa là xử lý sai`k = 1`. Nếu không có nhánh đặc biệt, mỗi ô trống sẽ được tính một lần khi quét hàng và một lần khi quét cột. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 2
**.
...
```#### Quét hàng 

| Hàng | Chạy của '.' | Đóng góp | 
| --- | --- | --- | 
|`**.`| 1 | 0 | 
|`...`| 3 | 2 | 

Câu trả lời hiện tại = 2. 

#### Quét cột 

| Cột | Mẫu | Độ dài chạy | Đóng góp | 
| --- | --- | --- | --- | 
| 1 |`*.`| 1 | 0 | 
| 2 |`*.`| 1 | 0 | 
| 3 |`..`| 2 | 1 | 

Câu trả lời cuối cùng = 3. 

Ví dụ này cho thấy cả hai loại sắp xếp. Hàng thứ hai đóng góp hai đoạn ngang, trong khi cột thứ ba đóng góp một đoạn dọc. 

### Ví dụ 2 

đầu vào:```
1 5 3
.....
```#### Quét hàng 

| Hàng | Độ dài chạy | Đóng góp | 
| --- | --- | --- | 
|`.....`| 5 | 3 | 

Câu trả lời hiện tại = 3. 

#### Quét cột 

| Cột | Độ dài chạy | Đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 1 | 0 | 
| 3 | 1 | 0 | 
| 4 | 1 | 0 | 
| 5 | 1 | 0 | 

Câu trả lời cuối cùng = 3. 

Dấu vết này thể hiện công thức`L - k + 1`. Một chuỗi có độ dài năm chứa chính xác ba đoạn dài ba. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập một lần trong quá trình quét hàng và một lần trong quá trình quét cột | 
| Không gian | O(nm) | Lưới đầu vào được lưu trong bộ nhớ | 

Lưới chứa tối đa bốn triệu ô. Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi ô, vừa vặn trong giới hạn. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ chính lưới. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    if k == 1:
        return str(sum(row.count('.') for row in grid))

    ans = 0

    for r in range(n):
        run = 0
        for c in range(m):
            if grid[r][c] == '.':
                run += 1
            else:
                if run >= k:
                    ans += run - k + 1
                run = 0
        if run >= k:
            ans += run - k + 1

    for c in range(m):
        run = 0
        for r in range(n):
            if grid[r][c] == '.':
                run += 1
            else:
                if run >= k:
                    ans += run - k + 1
                run = 0
        if run >= k:
            ans += run - k + 1

    return str(ans)

# provided sample
assert run("2 3 2\n**.\n...\n") == "3", "sample 1"

# minimum size
assert run("1 1 1\n.\n") == "1", "single empty cell"

# all blocked
assert run("2 2 1\n**\n**\n") == "0", "no valid arrangement"

# long run
assert run("1 5 3\n.....\n") == "3", "L-k+1 counting"

# split runs
assert run("1 5 2\n..*..\n") == "2", "blocked seat splits runs"

# k = 1 double-count trap
assert run("2 2 1\n..\n..\n") == "4", "must not count rows and columns separately"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`với`.`|`1`| Kích thước lưới tối thiểu | 
|`2 2 1`tất cả`*`|`0`| Không có sự sắp xếp hợp lệ | 
|`1 5 3`với tất cả`.`|`3`| Công thức`L-k+1`| 
|`1 5 2`với`..*..`|`2`| Chạy tách chướng ngại vật | 
|`2 2 1`tất cả`.`|`4`| Xử lý đúng`k=1`| 

## Vỏ cạnh 

### Trường hợp 1:`k = 1`đầu vào:```
2 2 1
..
..
```Thuật toán ngay lập tức đi vào nhánh đặc biệt và đếm các ô trống. Có 4 ô trống nên đáp án là`4`. 

Nếu chúng tôi thực hiện quét cả hàng và quét cột, mỗi ô sẽ đóng góp một lần theo chiều ngang và một lần theo chiều dọc, tạo ra`8`, điều đó không đúng. 

### Trường hợp 2: Chạy trống dài ngày 

đầu vào:```
1 5 3
.....
```Quá trình quét hàng tìm thấy một lần chạy có độ dài`5`. 

Sự đóng góp là:```
5 - 3 + 1 = 3
```tương ứng với các vị trí bắt đầu 1, 2 và 3. Thuật toán đếm tất cả các phân đoạn hợp lệ mà không liệt kê chúng riêng lẻ. 

### Trường hợp 3: Chướng ngại vật chia cắt đường chạy 

đầu vào:```
1 5 2
..*..
```Quá trình quét phát hiện hai đoạn có độ dài riêng biệt`2`. 

Lần chạy đầu tiên đóng góp:```
2 - 2 + 1 = 1
```Lần chạy thứ hai đóng góp khác`1`. 

Tổng số câu trả lời:```
1 + 1 = 2
```Chướng ngại vật ngăn các phân đoạn vượt qua vị trí giữa và thuật toán thực thi điều đó một cách tự nhiên bằng cách chấm dứt hoạt động hiện tại bất cứ khi nào`*`đang gặp phải.
