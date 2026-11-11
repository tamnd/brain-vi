---
title: "CF 157A - Kết quả trận đấu"
description: "Chúng ta có một bảng n × n trong đó mỗi ô chứa một số nguyên. Đối với mỗi ô, chúng tôi so sánh hai đại lượng. Số đầu tiên là tổng của tất cả các số trong hàng của ô đó. Số thứ hai là tổng của tất cả các số trong cột của ô đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 157
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 110 (Div. 2)"
rating: 800
weight: 157
solve_time_s: 139
verified: true
draft: false
---

[CF 157A - Kết quả trò chơi](https://codeforces.com/problemset/problem/157/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một`n × n`bảng trong đó mỗi ô chứa một số nguyên. Đối với mỗi ô, chúng tôi so sánh hai đại lượng. 

Số đầu tiên là tổng của tất cả các số trong hàng của ô đó. Số thứ hai là tổng của tất cả các số trong cột của ô đó. Một ô được gọi là chiến thắng nếu tổng cột của nó lớn hơn tổng hàng của nó. 

Nhiệm vụ là đếm xem có bao nhiêu ô thỏa mãn điều kiện này. 

Kích thước bảng rất nhỏ. Từ`n ≤ 30`, toàn bộ lưới chứa tối đa`30 × 30 = 900`tế bào. Ngay cả một thuật toán thực hiện thêm công việc cho mọi ô vẫn sẽ chạy thoải mái trong giới hạn. Điều này có nghĩa là chúng ta không cần các kỹ thuật tối ưu hóa nâng cao nhưng vẫn nên suy nghĩ cẩn thận về các phép tính lặp đi lặp lại. 

Việc triển khai trực tiếp có thể tính toán lại toàn bộ tổng hàng và tổng cột cho từng ô. Mỗi lần tính toán lại tốn`O(n)`, và có`n²`các ô, do đó tổng độ phức tạp trở thành`O(n³)`. Với`n = 30`, tốc độ này vẫn đủ nhanh vì tổng số thao tác chỉ khoảng`27,000`. 

Tuy nhiên, cấu trúc của vấn đề đương nhiên gợi ý một cách tiếp cận rõ ràng hơn. Tổng hàng và tổng cột được sử dụng lại nhiều lần. Khi chúng tôi tính toán chúng một lần, mọi ô có thể được kiểm tra theo thời gian không đổi. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Hãy xem xét bảng nhỏ nhất có thể:```
1
1
```Tổng hàng duy nhất là`1`và tổng cột duy nhất cũng là`1`. Vì điều kiện hoàn toàn lớn hơn nên câu trả lời là`0`. 

Một sai lầm phổ biến là sử dụng`>=`thay vì`>`. 

Một trường hợp tinh vi khác là khi mọi giá trị đều giống hệt nhau:```
2
5 5
5 5
```Tổng của mỗi hàng bằng tổng của mỗi cột nên không có ô nào thắng. Đầu ra đúng là:```
0
```Việc triển khai bất cẩn có thể vô tình tính số tiền bằng nhau là hợp lệ. 

Một tình huống phức tạp nữa là khi chỉ có một số hàng và cột khác nhau:```
3
1 1 10
1 1 10
1 1 10
```Tổng số hàng đều là`12`. Tổng các cột là`3`,`3`, Và`30`. 

Chỉ có cột thứ ba tạo ra các ô chiến thắng, vì vậy câu trả lời đúng là:```
3
```Điều này phát hiện các cách triển khai gây nhầm lẫn giữa chỉ mục hàng với chỉ mục cột trong khi tính tổng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Đối với mỗi tế bào`(i, j)`, chúng tôi tính tổng của hàng`i`bằng cách lặp qua tất cả các cột và chúng tôi tính tổng của cột`j`bằng cách lặp qua tất cả các hàng. Nếu tổng cột lớn hơn, chúng ta sẽ tăng câu trả lời. 

Điều này có hiệu quả vì định nghĩa của ô chiến thắng chỉ phụ thuộc vào hai tổng đó. Việc thực hiện rất đơn giản và phản ánh chính xác báo cáo vấn đề. 

Vấn đề là công việc lặp đi lặp lại. Tổng hàng giống nhau sẽ được tính lại một lần cho mỗi ô trong hàng đó và tổng cột giống nhau sẽ được tính lại một lần cho mỗi ô trong cột đó. 

Ví dụ, nếu`n = 30`, thì mỗi`900`tế bào thực hiện khoảng`60`bổ sung, dẫn đến khoảng`54,000`bổ sung tổng thể. Điều này ở đây vẫn hoàn toàn có thể chấp nhận được nhưng việc lặp lại là không cần thiết. 

Điều quan trọng là tổng hàng và tổng cột không bao giờ thay đổi. Khi chúng tôi tính toán chúng một lần, chúng có thể được sử dụng lại cho mọi ô có liên quan. 

Trước tiên chúng ta có thể xây dựng hai mảng:`row_sum[i]`lưu trữ tổng hàng`i`.`col_sum[j]`lưu trữ tổng của cột`j`. 

Sau đó, mỗi lần kiểm tra ô sẽ trở nên tầm thường: 

Nếu`col_sum[j] > row_sum[i]`, tế bào đang chiến thắng. 

Điều này làm giảm sự phức tạp từ`O(n³)`ĐẾN`O(n²)`đồng thời làm cho logic sạch hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`và`n × n`lưới. 
2. Tạo một mảng`row_sum`kích thước`n`được khởi tạo bằng số không. 
3. Tạo một mảng`col_sum`kích thước`n`được khởi tạo bằng số không. 
4. Duyệt toàn bộ lưới một lần. 

Đối với mỗi tế bào`(i, j)`, cộng giá trị của nó vào`row_sum[i]`và để`col_sum[j]`. 

Điều này tính toán tổng số hàng và tổng số cột trong một lần chuyển. 
5. Khởi tạo`answer = 0`. 
6. Di chuyển lưới một lần nữa. 

Đối với mỗi tế bào`(i, j)`, so sánh`col_sum[j]`Và`row_sum[i]`. 
7. Nếu`col_sum[j] > row_sum[i]`, tăng`answer`. 

Bài toán yêu cầu giá trị lớn hơn, không lớn hơn hoặc bằng. 
8. In`answer`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một bất biến đơn giản: sau lần duyệt đầu tiên,`row_sum[i]`bằng tổng của mọi giá trị trong hàng`i`, Và`col_sum[j]`bằng tổng của mọi giá trị trong cột`j`. 

Vì các giá trị này được tính toán chính xác một lần từ lưới ban đầu và không bao giờ được sửa đổi sau đó nên mọi so sánh sau này đều sử dụng tổng số chính xác. Mỗi ô được kiểm tra độc lập dựa trên điều kiện chính xác từ báo cáo vấn đề, vì vậy mỗi ô chiến thắng được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
grid = [list(map(int, input().split())) for _ in range(n)]

row_sum = [0] * n
col_sum = [0] * n

for i in range(n):
    for j in range(n):
        row_sum[i] += grid[i][j]
        col_sum[j] += grid[i][j]

answer = 0

for i in range(n):
    for j in range(n):
        if col_sum[j] > row_sum[i]:
            answer += 1

print(answer)
```Phần đầu tiên đọc bảng thành danh sách 2D. 

Vòng lặp lồng nhau tiếp theo tính tổng hàng và tổng cột cùng một lúc. Việc cập nhật cả hai mảng trong cùng một lần duyệt sẽ tránh được việc truyền qua lưới không cần thiết. 

Vòng lặp lồng nhau thứ hai kiểm tra từng ô một cách độc lập. Điều kiện sử dụng`>`bởi vì câu lệnh yêu cầu tổng cột phải lớn hơn. 

Không cần xử lý đặc biệt đối với các bảng nhỏ như`n = 1`. Logic hoạt động một cách tự nhiên vì tổng hàng và tổng cột bằng nhau đối với ô duy nhất. 

Số nguyên Python cũng hoàn toàn an toàn ở đây. Ngay cả trong trường hợp lớn nhất, tổng hàng hoặc cột tối đa chỉ`30 × 100 = 3000`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1
```Tổng được tính là: 

| Tế bào | Tổng hàng | Cột Tổng | Chiến thắng? | 
| --- | --- | --- | --- | 
| (0,0) | 1 | 1 | Không | 

Câu trả lời cuối cùng:```
0
```Ví dụ này xác nhận rằng sự bình đẳng không được tính là chiến thắng. 

### Ví dụ 2 

đầu vào:```
4
5 7 8 4
9 5 3 2
1 6 6 4
9 5 7 3
```Đầu tiên tính tổng hàng và tổng cột. 

Tổng hàng: 

| Hàng | Tổng hợp | 
| --- | --- | 
| 0 | 24 | 
| 1 | 19 | 
| 2 | 17 | 
| 3 | 24 | 

Tổng cột: 

| Cột | Tổng hợp | 
| --- | --- | 
| 0 | 24 | 
| 1 | 23 | 
| 2 | 24 | 
| 3 | 13 | 

Bây giờ hãy đánh giá từng ô. 

| Tế bào | Tổng hàng | Cột Tổng | Chiến thắng? | 
| --- | --- | --- | --- | 
| (0,0) | 24 | 24 | Không | 
| (0,1) | 24 | 23 | Không | 
| (0,2) | 24 | 24 | Không | 
| (0,3) | 24 | 13 | Không | 
| (1,0) | 19 | 24 | Có | 
| (1,1) | 19 | 23 | Có | 
| (1,2) | 19 | 24 | Có | 
| (1,3) | 19 | 13 | Không | 
| (2,0) | 17 | 24 | Có | 
| (2,1) | 17 | 23 | Có | 
| (2,2) | 17 | 24 | Có | 
| (2,3) | 17 | 13 | Không | 
| (3,0) | 24 | 24 | Không | 
| (3,1) | 24 | 23 | Không | 
| (3,2) | 24 | 24 | Không | 
| (3,3) | 24 | 13 | Không | 

Tổng số ô chiến thắng là`6`. 

Dấu vết này chứng tỏ rằng tổng số hàng và cột giống nhau được sử dụng lại trong nhiều lần kiểm tra, đó chính xác là lý do tại sao việc tính toán trước chúng lại hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chúng tôi đi qua lưới hai lần | 
| Không gian | O(n) | Chúng tôi lưu trữ tổng hàng và tổng cột | 

Với`n ≤ 30`, thuật toán thấp hơn nhiều so với giới hạn thời gian và bộ nhớ. Ngay cả giải pháp vũ lực cũng sẽ vượt qua, nhưng phiên bản được tính toán trước sẽ sạch hơn và có quy mô tốt hơn về mặt khái niệm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    grid = [list(map(int, input().split())) for _ in range(n)]

    row_sum = [0] * n
    col_sum = [0] * n

    for i in range(n):
        for j in range(n):
            row_sum[i] += grid[i][j]
            col_sum[j] += grid[i][j]

    ans = 0

    for i in range(n):
        for j in range(n):
            if col_sum[j] > row_sum[i]:
                ans += 1

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run("1\n1\n") == "0\n", "sample 1"

# all equal values
assert run(
    "2\n"
    "5 5\n"
    "5 5\n"
) == "0\n", "all sums equal"

# one dominant column
assert run(
    "3\n"
    "1 1 10\n"
    "1 1 10\n"
    "1 1 10\n"
) == "3\n", "third column wins everywhere"

# asymmetric case
assert run(
    "2\n"
    "1 100\n"
    "1 100\n"
) == "2\n", "second column only"

# larger mixed case
assert run(
    "4\n"
    "5 7 8 4\n"
    "9 5 3 2\n"
    "1 6 6 4\n"
    "9 5 7 3\n"
) == "6\n", "mixed values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 × 1`bảng |`0`| Đầu vào kích thước tối thiểu | 
| Tất cả các giá trị bằng nhau |`0`| Xử lý so sánh nghiêm ngặt | 
| Một cột chiếm ưu thế |`3`| Tích lũy cột đúng | 
|`[[1,100],[1,100]]`|`2`| Tính chính xác của việc lập chỉ mục hàng/cột | 
| Hỗn hợp`4 × 4`bảng |`6`| General correctness |

 ## Vỏ cạnh 

Hãy xem xét bảng nhỏ nhất có thể:```
1
1
```Thuật toán tính toán:`row_sum = [1]`

`col_sum = [1]`Đối với ô duy nhất, việc so sánh trở thành`1 > 1`, điều đó là sai. Thuật toán in chính xác:```
0
```Điều này xác nhận rằng sự bình đẳng không được tính. 

Bây giờ hãy xem xét một bảng trong đó mọi số đều giống hệt nhau:```
2
5 5
5 5
```Tổng hàng trở thành`[10, 10]`và tổng cột cũng trở thành`[10, 10]`. 

Mọi sự so sánh đều`10 > 10`, điều đó là sai. Không có ô nào được tính nên câu trả lời là:```
0
```Trường hợp này xác minh rằng việc triển khai không vô tình sử dụng`>=`. 

Cuối cùng, hãy xem xét trường hợp có cột chiếm ưu thế:```
3
1 1 10
1 1 10
1 1 10
```Thuật toán tính toán:`row_sum = [12, 12, 12]`

`col_sum = [3, 3, 30]`Với mỗi hàng, chỉ có cột thứ ba thỏa mãn`30 > 12`. 

Chính xác ba ô được đếm, vì vậy đầu ra là:```
3
```Điều này xác nhận rằng các chỉ mục hàng và cột được xử lý chính xác và số tiền đó được sử dụng lại một cách nhất quán trên tất cả các ô.
