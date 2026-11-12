---
title: "CF 166A - Danh sách xếp hạng"
description: "Chúng tôi nhận được kết quả cuối cùng của một cuộc thi lập trình. Mỗi đội có hai giá trị gắn liền với nó: đội đó đã giải quyết được bao nhiêu vấn đề và tổng thời gian phạt của đội đó. Quy tắc xếp hạng là thứ tự kiểu ICPC tiêu chuẩn. Một đội được xếp hạng cao hơn nếu giải quyết được nhiều vấn đề hơn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 166
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 113 (Div. 2)"
rating: 1100
weight: 166
solve_time_s: 91
verified: true
draft: false
---

[CF 166A - Danh sách xếp hạng](https://codeforces.com/problemset/problem/166/A) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được kết quả cuối cùng của một cuộc thi lập trình. Mỗi đội có hai giá trị gắn liền với nó: đội đó đã giải quyết được bao nhiêu vấn đề và tổng thời gian phạt của đội đó. 

Quy tắc xếp hạng là thứ tự kiểu ICPC tiêu chuẩn. Một đội được xếp hạng cao hơn nếu giải quyết được nhiều vấn đề hơn. Nếu hai đội giải được số bài toán giống nhau thì đội nào có hình phạt nhỏ hơn sẽ xếp hạng cao hơn. Các đội có cặp giống hệt nhau`(solved, penalty)`chia sẻ các vị trí tương tự trên bảng xếp hạng. 

Nhiệm vụ là xác định có bao nhiêu đội chia sẻ`k`-vị trí thứ 

Chi tiết quan trọng là đầu vào không được đảm bảo đã được sắp xếp theo thứ hạng. Đầu tiên chúng ta phải xây dựng lại bảng xếp hạng theo thể lệ cuộc thi, sau đó xác định đội ngồi ở vị trí`k`, và cuối cùng đếm xem có bao nhiêu đội có kết quả giống nhau. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 50 đội, vì vậy ngay cả các thuật toán bậc hai cũng dễ dàng đủ nhanh. Một loại 50 phần tử về cơ bản là tức thời. Điều này có nghĩa là vấn đề không nằm ở áp lực tối ưu hóa, mà là ở việc thực hiện các quy tắc xếp hạng một cách chính xác mà không mắc những sai sót nhỏ. 

Sai lầm phổ biến nhất là hiểu sai ý nghĩa của “chia sẻ vị trí thứ k”. Vấn đề là không yêu cầu nhóm hiện được lưu trữ tại chỉ mục`k`. Nó yêu cầu tất cả các đội có kết quả giống hệt với đội chiếm vị trí`k`-vị trí thứ sau khi sắp xếp. 

Hãy xem xét ví dụ này:```
5 4
5 1
4 10
4 10
4 10
3 1
```Sau khi sắp xếp, bảng xếp hạng trở thành:```
1st: (5,1)
2nd-4th: (4,10)
5th: (3,1)
```Vị trí thứ 4 thuộc về`(4,10)`nhóm, vậy đáp án là`3`. 

Việc triển khai bất cẩn có thể chỉ kiểm tra chỉ mục chính xác`k-1`và đầu ra`1`. 

Một trường hợp tinh tế khác xuất hiện khi tất cả các đội đều giống hệt nhau:```
4 2
3 20
3 20
3 20
3 20
```Mỗi đội chia nhau vị trí từ 1 đến 4. Câu trả lời đúng là`4`. 

Việc triển khai không chính xác khi cố gắng chỉ định các cấp bậc duy nhất theo cách thủ công có thể vô tình tách các nhóm bằng nhau. 

Ngoài ra còn có một lỗi tiềm ẩn tiềm ẩn vì các vị trí của Codeforces được lập chỉ mục 1 trong khi danh sách Python được lập chỉ mục 0. Nếu như`k = 1`, nhóm liên quan đang ở chỉ mục`0`sau khi sắp xếp. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi có thể so sánh mọi đội với mọi đội khác và xác định các nhóm xếp hạng theo cách thủ công. Đối với mỗi đội, chúng tôi có thể đếm xem có bao nhiêu đội thực sự tốt hơn và sau đó xác định những đội nào thuộc cùng một nhóm vị trí. 

Điều này có hiệu quả vì quy tắc xếp hạng được xác định hoàn toàn bằng cách so sánh theo cặp. Với`n ≤ 50`, thậm chí là một`O(n²)`giải pháp chỉ thực hiện 2500 so sánh trong trường hợp xấu nhất. 

Điểm yếu của phương pháp này không phải là thời gian chạy mà là độ phức tạp của việc triển khai. Việc xây dựng lại các cấp bậc theo cách thủ công gây ra việc ghi sổ không cần thiết và làm cho các trường hợp xung quanh các nhóm bị ràng buộc dễ bị xử lý sai hơn. 

Quan sát rõ ràng hơn là thứ hạng cuộc thi chính xác là một thứ tự sắp xếp. Các đội được sắp xếp theo: 

1. Số lượng giải quyết lớn hơn trước. 
2. Hình phạt nhẹ hơn trước. 

Sau khi các đội được sắp xếp, đội ở chỉ mục`k-1`xác định toàn bộ nhóm cấp bậc cho`k`-vị trí thứ Đội nào cũng giống nhau`(solved, penalty)`cặp chia sẻ nơi đó. 

Điều này biến vấn đề thành hai thao tác đơn giản: 

1. Sắp xếp các đội một cách chính xác. 
2. Đếm xem có bao nhiêu đội bằng nhau về vị trí`k-1`. 

Cấu trúc của quy tắc xếp hạng làm cho việc sắp xếp trở thành giải pháp tự nhiên vì các kết quả bằng nhau tạo thành các khối liền kề theo thứ tự được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n log n) | O(1) không bao gồm không gian sắp xếp | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đội`n`và địa điểm mục tiêu`k`. 
2. Lưu trữ mỗi đội thành một cặp`(problems_solved, penalty)`. 
3. Sắp xếp các đội theo quy tắc xếp hạng cuộc thi. 

Các nhóm có nhiều vấn đề được giải quyết hơn phải đến trước, vì vậy chúng tôi sắp xếp giá trị đó theo thứ tự giảm dần. Đối với số lượng được giải bằng nhau, các hình phạt nhỏ hơn phải được xếp trước, vì vậy chúng tôi sắp xếp các hình phạt theo thứ tự tăng dần. 
4. Chuyển đổi vị trí cuộc thi thành chỉ mục Python. 

Vì địa điểm của cuộc thi được đánh số 1 nên đội đại diện cho`k`-vị trí thứ được lưu trữ tại chỉ mục`k-1`. 
5. Lưu cặp tại chỉ mục`k-1`. 

Cặp này xác định duy nhất toàn bộ nhóm bị ràng buộc chia sẻ`k`-vị trí thứ 
6. Duyệt qua danh sách đã sắp xếp và đếm xem có bao nhiêu đội có cặp giống hệt nhau. 
7. In số đếm. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, các đội có kết quả giống nhau sẽ xếp liền nhau vì thứ tự chỉ phụ thuộc vào`(solved, penalty)`. Đội ngũ tại vị trí`k-1`đại diện cho hiệu suất chính xác liên quan đến`k`-vị trí thứ Mỗi đội có cùng cặp đó sẽ chia sẻ vị trí và mỗi đội có cặp khác nhau sẽ thuộc một nhóm xếp hạng khác. Đếm các cặp bằng nhau sẽ có câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

teams = []

for _ in range(n):
    p, t = map(int, input().split())
    teams.append((p, t))

teams.sort(key=lambda x: (-x[0], x[1]))

target = teams[k - 1]

answer = 0

for team in teams:
    if team == target:
        answer += 1

print(answer)
```Phần đầu tiên đọc dữ liệu đầu vào và lưu trữ mỗi nhóm dưới dạng một bộ dữ liệu. Việc sử dụng bộ dữ liệu rất thuận tiện vì việc kiểm tra đẳng thức trở nên tự động và đáng tin cậy. 

Dòng phân loại là cốt lõi của giải pháp:```
teams.sort(key=lambda x: (-x[0], x[1]))
```Dấu âm trên`x[0]`tạo ra thứ tự giảm dần cho các vấn đề được giải quyết. Hình phạt vẫn tích cực vì hình phạt nhỏ hơn sẽ xếp hạng cao hơn. 

Sau khi sắp xếp,`teams[k - 1]`xác định hiệu suất tương ứng với`k`-vị trí thứ Việc trừ đi một là cần thiết vì Python sử dụng tính năng lập chỉ mục dựa trên số 0. 

Vòng lặp cuối cùng đếm mọi lần xuất hiện của bộ dữ liệu chính xác đó. Vì các đội bằng điểm được xác định bằng sự bằng nhau của cả hai giá trị nên việc so sánh bộ dữ liệu hoàn toàn phù hợp với quy tắc của cuộc thi. 

Một lỗi thực hiện phổ biến là sắp xếp các hình phạt theo thứ tự giảm dần. Điều đó sẽ xếp hạng không chính xác các hình phạt lớn hơn trước các hình phạt nhỏ hơn. 

Một lỗi thường gặp khác là chỉ tính các đội bằng nhau liên tiếp xung quanh chỉ số`k-1`. Mặc dù điều đó cũng có tác dụng vì các nhóm bằng nhau sẽ tiếp giáp nhau sau khi sắp xếp, nhưng việc duyệt toàn bộ sẽ đơn giản hơn và ít xảy ra lỗi hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 2
4 10
4 10
4 10
3 20
2 1
2 1
1 10
```Sau khi sắp xếp: 

| Chỉ mục | Đội`(p,t)`| Giống như mục tiêu? | Đếm | 
| --- | --- | --- | --- | 
| 0 | (4,10) | Có | 1 | 
| 1 | (4,10) | Có | 2 | 
| 2 | (4,10) | Có | 3 | 
| 3 | (3,20) | Không | 3 | 
| 4 | (2,1) | Không | 3 | 
| 5 | (2,1) | Không | 3 | 
| 6 | (1,10) | Không | 3 | 

Vị trí mục tiêu là`k = 2`, vậy là nhóm mục tiêu đang ở chỉ mục`1`, đó là`(4,10)`. 

Có 3 đội có kết quả chính xác như vậy nên đáp án là`3`. 

Ví dụ này chứng minh rằng một nhóm gắn kết có thể trải rộng trên nhiều vị trí xếp hạng cùng một lúc. 

### Mẫu 2 

đầu vào:```
5 4
5 3
3 1
3 1
3 1
3 1
```Sau khi sắp xếp: 

| Chỉ mục | Đội`(p,t)`| Giống như mục tiêu? | Đếm | 
| --- | --- | --- | --- | 
| 0 | (5,3) | Không | 0 | 
| 1 | (3,1) | Có | 1 | 
| 2 | (3,1) | Có | 2 | 
| 3 | (3,1) | Có | 3 | 
| 4 | (3,1) | Có | 4 | 

Vị trí mục tiêu là`k = 4`, vậy nhóm mục tiêu là`(3,1)`. 

Bốn đội chia nhau kết quả đó nên có 4 đội chia nhau vị trí thứ 4. 

Ví dụ này xác nhận rằng địa điểm được yêu cầu có thể nằm bên trong một khối lớn được buộc chặt chứ không phải ở phần đầu của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) không bao gồm không gian sắp xếp | Chỉ có một vài biến bổ sung được sử dụng | 

Với tối đa 50 nhóm, thời gian chạy rất nhỏ. Ngay cả những cách tiếp cận chậm hơn nhiều cũng sẽ phù hợp một cách thoải mái trong giới hạn. Giải pháp dựa trên sắp xếp vừa hiệu quả vừa rất đơn giản để giải thích. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())

    teams = []

    for _ in range(n):
        p, t = map(int, input().split())
        teams.append((p, t))

    teams.sort(key=lambda x: (-x[0], x[1]))

    target = teams[k - 1]

    print(sum(1 for team in teams if team == target))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided samples
assert run(
"""7 2
4 10
4 10
4 10
3 20
2 1
2 1
1 10
"""
) == "3\n", "sample 1"

assert run(
"""5 4
5 3
3 1
3 1
3 1
3 1
"""
) == "4\n", "sample 2"

# minimum size
assert run(
"""1 1
5 10
"""
) == "1\n", "single team"

# all equal
assert run(
"""4 2
3 20
3 20
3 20
3 20
"""
) == "4\n", "all teams tied"

# boundary around tied block
assert run(
"""5 3
5 1
4 10
4 10
4 10
3 5
"""
) == "3\n", "k inside tied group"

# different penalties with same solved count
assert run(
"""5 2
4 20
4 10
4 30
3 1
2 1
"""
) == "1\n", "penalty ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đội đơn | 1 | Đầu vào nhỏ nhất có thể | 
| Tất cả các đội bình đẳng | 4 | Toàn bộ bảng xếp hạng là một khối gắn liền | 
|`k`khối buộc bên trong | 3 | Các vị trí được chia sẻ được xử lý chính xác | 
| Số lần giải quyết giống nhau, hình phạt khác nhau | 1 | Phím sắp xếp thứ cấp hoạt động chính xác | 

## Vỏ cạnh 

Một tình huống khó khăn là khi địa điểm được yêu cầu nằm ở giữa một nhóm bị ràng buộc. 

đầu vào:```
5 4
5 1
4 10
4 10
4 10
3 1
```Sau khi sắp xếp:```
(5,1)
(4,10)
(4,10)
(4,10)
(3,1)
```Vị trí thứ 4 tương ứng với chỉ số`3`, cái vẫn thuộc về`(4,10)`. Thuật toán lưu trữ bộ dữ liệu đó làm mục tiêu và đếm tất cả các bộ dữ liệu giống hệt nhau, tạo ra`3`. 

Một trường hợp tinh vi khác là khi mọi đội đều giống hệt nhau. 

đầu vào:```
4 2
3 20
3 20
3 20
3 20
```Sắp xếp không thay đổi gì cả. Bộ mục tiêu là`(3,20)`, và mọi đội đều phù hợp với nó. Thuật toán trả về chính xác`4`. 

Trường hợp lệch biên xuất hiện khi`k = 1`. 

đầu vào:```
3 1
5 10
4 1
3 1
```Sau khi sắp xếp, mục tiêu ở chỉ mục`0`, cụ thể là`(5,10)`. Chỉ có một đội phù hợp với nó, vì vậy câu trả lời là`1`. 

sử dụng`teams[k]`thay vì`teams[k - 1]`chọn nhầm đội xếp thứ hai.
