---
title: "CF 268A - Trò chơi"
description: "Chúng ta được cấp màu đồng phục của tất cả các đội trong giải vô địch bóng đá. Mỗi đội có một màu sân nhà và một màu sân khách. Mỗi cặp đội riêng biệt theo thứ tự sẽ chơi đúng một trận, trong đó một đội đóng vai chủ nhà và đội kia đóng vai khách."
date: "2026-06-05T01:18:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 268
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 164 (Div. 2)"
rating: 800
weight: 268
solve_time_s: 94
verified: true
draft: false
---

[CF 268A - Trò chơi](https://codeforces.com/problemset/problem/268/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp màu đồng phục của tất cả các đội trong giải vô địch bóng đá. Mỗi đội có một màu sân nhà và một màu sân khách. 

Mỗi cặp đội riêng biệt theo thứ tự sẽ chơi đúng một trận, trong đó một đội đóng vai chủ nhà và đội kia đóng vai khách. Thông thường, chủ nhà mặc đồng phục ở nhà và khách mặc đồng phục sân khách. 

Tình huống đặc biệt duy nhất xảy ra khi màu sân nhà của chủ nhà giống hệt màu sân khách của khách. Trong trường hợp đó, chủ nhà sẽ thay đổi và mặc đồng phục sân khách. 

Nhiệm vụ là đếm xem có bao nhiêu trận đấu trong toàn bộ giải vô địch sẽ kích hoạt công tắc này. 

Dữ liệu đầu vào cung cấp màu sắc sân nhà và sân khách cho mỗi đội. Đầu ra là một số nguyên duy nhất, tổng số trận đấu mà chủ nhà mặc đồng phục sân khách. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 30 đội nên tổng số cặp chủ-khách có thể có nhiều nhất là:$$30 \cdot 29 = 870$$Ngay cả việc kiểm tra đơn giản mọi trận đấu có thể xảy ra cũng đủ nhanh chóng. Không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. 

Một sai lầm phổ biến là nghĩ về các cặp đội không có thứ tự. Giải vô địch bao gồm các trận đấu theo thứ tự. Nếu đội A chủ nhà đội B và đội B chủ nhà đội A thì đó là hai trận đấu khác nhau và phải được xem xét riêng. 

Hãy xem xét ví dụ này:```
2
1 2
2 1
```Có hai trận đấu: 

Đội 1 chủ trì Đội 2: 

chủ nhà = 1, khách đi = 1, xảy ra chuyển đổi. 

Đội 2 đăng cai Đội 1: 

chủ nhà = 2, khách đi = 2, xảy ra chuyển đổi. 

Câu trả lời đúng là:```
2
```Giải pháp chỉ kiểm tra từng cặp một lần sẽ trả về sai 1. 

Một trường hợp tinh tế khác là khi một số đội có cùng màu sân nhà.```
3
1 2
1 3
4 1
```Cả Đội 1 và Đội 2 đều có màu sân nhà là 1. Đội 3 có màu sân khách là 1. 

Những trận đấu Đội 3 thăm Đội 1 và Đội 3 thăm Đội 2 đều góp phần giải đáp. Chúng ta phải đếm mọi kết hợp máy chủ-khách hợp lệ một cách độc lập. 

Câu trả lời đúng là:```
2
```## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng điều kiện cho mọi trận đấu có thể xảy ra. 

Cho mỗi đội$i$đóng vai trò chủ nhà và mọi đội khác nhau$j$đóng vai trò là khách, chúng tôi kiểm tra xem:$$home_i = away_j$$Nếu màu sắc trùng khớp, người dẫn chương trình sẽ đổi đồng phục và chúng tôi sẽ tăng câu trả lời. 

Cách tiếp cận này đúng vì việc xác định vấn đề chỉ phụ thuộc vào màu sân nhà của chủ nhà và màu sân khách của khách. Mỗi trận đấu có thể được đánh giá độc lập. 

Trường hợp xấu nhất có 30 đội nên chúng tôi thực hiện nhiều nhất:$$30 \cdot 29 = 870$$so sánh. Đó là nhỏ bé. 

Ngoài ra còn có một cách giải thích đếm. Mỗi lần xuất hiện của màu sân nhà có thể khớp với mọi lần xuất hiện có cùng giá trị với màu sân khách. Người ta có thể đếm tần số màu và nhân chúng lên. Điều đó cũng có tác dụng nhưng không cần thiết vì có những hạn chế nhỏ. 

Giải pháp brute-force đã là tối ưu cho vấn đề này vì kích thước đầu vào quá nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Đếm tần số | O(n + C) | O(C) | Đã chấp nhận | 

Đây$C$là số màu có thể có. Từ$n \le 30$, nghiệm bậc hai đơn giản hơn là sự lựa chọn tự nhiên. 

## Hướng dẫn thuật toán 

1. Đọc số đội. 
2. Lưu trữ màu sân nhà và màu sân khách của mỗi đội vào danh sách. 
3. Khởi tạo`answer = 0`. 
4. Đối với mỗi đội`i`, coi nó như máy chủ. 
5. Đối với mỗi đội`j`, coi nó như khách. 
6. Bỏ qua trường hợp ở đâu`i == j`, vì một đội không bao giờ tự chơi. 
7. Kiểm tra xem màu sân nhà của chủ nhà có giống màu sân khách của khách hay không. 
8. Nếu bằng nhau thì tăng`answer`bởi một người vì chủ nhà phải chuyển sang đồng phục sân khách. 
9. Sau khi tất cả các cặp đã được xử lý, hãy in`answer`. 

### Tại sao nó hoạt động 

Giải vô địch bao gồm một trận đấu dành cho mỗi cặp đội riêng biệt được sắp xếp theo thứ tự. Thuật toán kiểm tra chính xác các cặp đó. 

Đối với một trận đấu cụ thể`(i, j)`, vấn đề nêu rõ rằng chủ nhà thay đổi đồng phục khi và chỉ khi màu sân nhà của chủ nhà bằng màu sân khách của khách. Thuật toán kiểm tra chính xác điều kiện này và thêm chính xác một điều kiện khi kết quả phù hợp góp phần tạo ra câu trả lời. 

Vì mỗi trận đấu được kiểm tra một lần và chỉ một lần nên con số cuối cùng chính xác là số trận đấu mà chủ nhà mặc đồng phục sân khách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
teams = [tuple(map(int, input().split())) for _ in range(n)]

answer = 0

for i in range(n):
    home_i = teams[i][0]

    for j in range(n):
        if i == j:
            continue

        away_j = teams[j][1]

        if home_i == away_j:
            answer += 1

print(answer)
```Phần đầu tiên đọc và lưu trữ tất cả các màu thống nhất. Mỗi mục chứa`(home_color, away_color)`. 

Các vòng lặp lồng nhau liệt kê mọi cặp máy chủ-khách theo thứ tự. điều kiện`i == j`bị bỏ qua vì không có đội nào đấu với chính mình. 

Sự so sánh chính là:```
home_i == away_j
```Điều này trực tiếp phù hợp với quy tắc từ câu lệnh. Bất cứ khi nào nó đúng, trận đấu hiện tại đóng góp chính xác một cho câu trả lời. 

Không cần xử lý đặc biệt đối với các màu trùng lặp. Nếu một số đội có cùng màu, mọi trận đấu hợp lệ sẽ được tính theo vòng lặp một cách tự nhiên. 

Không thể tràn số nguyên vì câu trả lời tối đa nhiều nhất là$30 \cdot 29 = 870$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2
2 4
3 4
```Đội: 

| Đội | Trang chủ | Đi xa | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 2 | 4 | 
| 3 | 3 | 4 | 

Dấu vết: 

| Máy chủ | Khách | Trang chủ chủ nhà | Khách đi xa | Cuộc thi đấu? | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 4 | Không | 0 | 
| 1 | 3 | 1 | 4 | Không | 0 | 
| 2 | 1 | 2 | 2 | Có | 1 | 
| 2 | 3 | 2 | 4 | Không | 1 | 
| 3 | 1 | 3 | 2 | Không | 1 | 
| 3 | 2 | 3 | 4 | Không | 1 | 

Câu trả lời cuối cùng:```
1
```Điều này chứng tỏ rằng các trận đấu được sắp xếp theo thứ tự. Trận đấu mà Đội 2 chủ nhà Đội 1 đóng góp, còn trận ngược lại thì không. 

### Mẫu 2 

đầu vào:```
4
1 2
2 4
3 4
4 5
```Đội: 

| Đội | Trang chủ | Đi xa | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 2 | 4 | 
| 3 | 3 | 4 | 
| 4 | 4 | 5 | 

Dấu vết của các trận đấu đóng góp: 

| Máy chủ | Khách | Trang chủ chủ nhà | Khách đi xa | 
| --- | --- | --- | --- | 
| 2 | 1 | 2 | 2 | 
| 4 | 2 | 4 | 4 | 
| 4 | 3 | 4 | 4 | 

Tổng cộng:```
3
```Ví dụ này cho thấy rằng một màu chủ nhà có thể khớp với nhiều màu sân khách và mỗi màu trùng khớp như vậy phải được tính riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cặp đội theo thứ tự đều được kiểm tra một lần | 
| Không gian | O(n) | Lưu trữ các cặp màu của đội | 

Với tối đa 30 nhóm, thuật toán thực hiện ít hơn 900 phép so sánh. Giá trị này thấp hơn nhiều so với giới hạn nên giải pháp có thể chạy thoải mái trong cả giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    teams = [tuple(map(int, input().split())) for _ in range(n)]

    ans = 0

    for i in range(n):
        for j in range(n):
            if i != j and teams[i][0] == teams[j][1]:
                ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("3\n1 2\n2 4\n3 4\n") == "1", "sample 1"

# minimum size, no match
assert run("2\n1 2\n3 4\n") == "0", "minimum case"

# two matches because games are ordered
assert run("2\n1 2\n2 1\n") == "2", "ordered matches"

# multiple hosts matching same away color
assert run("3\n1 2\n1 3\n4 1\n") == "2", "duplicate home colors"

# all possible ordered matches contribute
assert run("3\n1 2\n2 1\n1 2\n") == "4", "multiple contributions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / (1,2) (3,4)`|`0`| Kích thước tối thiểu không có màu phù hợp | 
|`2 / (1,2) (2,1)`|`2`| Các trận đấu đã đặt hàng được tính riêng | 
|`3 / (1,2) (1,3) (4,1)`|`2`| Màu sắc nhà trùng lặp | 
|`3 / (1,2) (2,1) (1,2)`|`4`| Nhiều trận đấu có cùng màu | 

## Vỏ cạnh 

### Thứ tự trùng khớp rất quan trọng 

đầu vào:```
2
1 2
2 1
```Thuật toán kiểm tra: 

| Máy chủ | Khách | Tình trạng | 
| --- | --- | --- | 
| 1 | 2 | 1 = 1 | 
| 2 | 1 | 2 = 2 | 

Cả hai trận đấu đều đóng góp. 

Đầu ra:```
2
```Giải pháp chỉ xem xét mỗi cặp một lần sẽ chỉ tính sai một kết quả. 

### Nhiều đội cùng một màu 

đầu vào:```
3
1 2
1 3
4 1
```Thuật toán tìm thấy: 

| Máy chủ | Khách | Tình trạng | 
| --- | --- | --- | 
| 1 | 3 | 1 = 1 | 
| 2 | 3 | 1 = 1 | 

Hai đội chủ nhà khác nhau có cùng màu sân khách. 

Đầu ra:```
2
```Cách tiếp cận vòng lặp lồng nhau sẽ tính cả hai lần xuất hiện một cách tự nhiên. 

### Không có màu phù hợp 

đầu vào:```
3
1 2
3 4
5 6
```Mọi so sánh đều thất bại vì không có màu sân nhà nào xuất hiện dưới dạng màu sân khách của đội khác. 

Đầu ra:```
0
```Thuật toán đơn giản là không bao giờ tăng bộ đếm, tạo ra kết quả chính xác.
