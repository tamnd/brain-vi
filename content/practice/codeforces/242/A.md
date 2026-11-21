---
title: "CF 242A - Đầu hoặc Đuôi"
description: "Chúng ta biết mỗi người chơi tung đồng xu bao nhiêu lần. Vasya lật x lần và Petya lật y lần. Mỗi cái đầu cho một điểm, mỗi cái đuôi không cho một điểm nào. Valera không nhớ chính xác tỷ số chung cuộc, nhưng anh nhớ được ba sự thật."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 242
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 149 (Div. 2)"
rating: 1100
weight: 242
solve_time_s: 206
verified: true
draft: false
---

[CF 242A - Ngửa hoặc Đuôi](https://codeforces.com/problemset/problem/242/A) 

**Đánh giá:** 1100 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta biết mỗi người chơi tung đồng xu bao nhiêu lần. Vasya lật`x`lần và Petya lật`y`lần. Mỗi cái đầu cho một điểm, mỗi cái đuôi không cho một điểm nào. 

Valera không nhớ chính xác tỷ số chung cuộc, nhưng anh nhớ được ba sự thật. Vasya có ít nhất`a`đầu, ít nhất Petya có`b`đứng đầu, và Vasya đã về đích trước Petya. 

Chúng ta phải in từng cặp`(c, d)`như vậy: 

-`c`là số đầu có thể có của Vasya 
-`d`là số lượng đầu có thể có của Petya 
-`a ≤ c ≤ x`-`b ≤ d ≤ y`-`c > d`Các cặp đầu ra phải xuất hiện theo thứ tự tăng dần về mặt từ điển. Điều đó có nghĩa là nhỏ hơn`c`đến trước và bằng nhau`c`, nhỏ hơn`d`đến trước. 

Những hạn chế là rất nhỏ. Mọi giá trị nhiều nhất là`100`. Ngay cả việc kiểm tra tất cả các cặp có thể cũng chỉ yêu cầu nhiều nhất`101 × 101 = 10201`lặp đi lặp lại, điều này hoàn toàn tầm thường đối với giới hạn 2 giây. Điều này ngay lập tức gợi ý một sự liệt kê vũ phu trực tiếp. 

Mối nguy hiểm thực sự duy nhất trong vấn đề này là làm sai phạm vi bất đẳng thức hoặc phạm vi lặp. 

Một sai lầm dễ mắc phải là cho phép rút bài. Vasya phải là người chiến thắng nên điều kiện rất nghiêm ngặt`c > d`, không`c ≥ d`. 

Hãy xem xét đầu vào này:```
2 2 1 1
```Điểm số có thể là:```
2 1
2 2
1 1
1 2
```Chỉ một`(2, 1)`hợp lệ vì Vasya phải có nhiều đầu hơn Petya. Việc thực hiện bất cẩn bằng cách sử dụng`>=`sẽ bao gồm không chính xác`(1,1)`Và`(2,2)`. 

Một trường hợp tinh vi khác xảy ra khi điểm tối thiểu của Petya đã quá lớn so với hầu hết điểm của Vasya. 

Ví dụ:```
3 5 1 4
```Petya luôn có ít nhất`4`đầu. Vasya có thể có nhiều nhất`3`. Không có kết quả hợp lệ nào tồn tại, vì vậy kết quả đầu ra đúng là:```
0
```Một giải pháp bất cẩn in mù tất cả các cặp trong phạm vi mà không kiểm tra điều kiện thắng sẽ thất bại ở đây. 

Việc đặt hàng cũng rất quan trọng. Các cặp phải được sắp xếp theo từ điển. Nếu chúng ta lặp lại`c`từ nhỏ đến lớn và cho mỗi`c`lặp đi lặp lại`d`từ nhỏ đến lớn, đơn hàng sinh ra sẽ tự động đáp ứng yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là thử mọi cặp điểm cuối cùng có thể có. 

Điểm của Vasya có thể dao động từ`a`ĐẾN`x`. Điểm của Petya có thể dao động từ`b`ĐẾN`y`. Đối với mỗi cặp`(c, d)`trong những phạm vi này, chúng tôi kiểm tra xem liệu`c > d`. Nếu có, chúng tôi lưu trữ cặp này. 

Phương pháp bạo lực này là chính xác vì mọi kết quả trò chơi hợp lệ đều tương ứng với chính xác một cặp bên trong các giới hạn này và mỗi cặp chúng tôi đưa ra đều thỏa mãn tất cả các điều kiện từ câu lệnh. 

Cách tiếp cận vũ phu đã phù hợp một cách thoải mái trong giới hạn. Tệ nhất, chúng tôi kiểm tra khoảng mười nghìn cặp, con số này không đáng kể. 

Không có thủ thuật tối ưu hóa ẩn nào ở đây vì không gian tìm kiếm vốn đã cực kỳ nhỏ. Quan sát thực tế nhận thấy rằng các hạn chế là rất nhỏ có chủ ý, vì vậy việc triển khai trực tiếp là giải pháp dự kiến. Cố gắng rút ra các công thức hoặc cắt tỉa phức tạp chỉ làm cho mã khó đọc hơn mà không cải thiện hiệu suất theo bất kỳ cách nào có ý nghĩa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O((x - a + 1)(y - b + 1)) | O(số câu trả lời) | Đã chấp nhận | 
| Đạo hàm toán học được tối ưu hóa | O(số câu trả lời) | O(số câu trả lời) | Không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên`x`,`y`,`a`, Và`b`. 
2. Tạo danh sách trống`ans`để lưu trữ tất cả các cặp điểm hợp lệ. 
3. Lặp lại số điểm có thể có của Vasya`c`từ`a`ĐẾN`x`. 

Đây đều là những giá trị phù hợp với trí nhớ của Valera mà ít nhất Vasya đã ghi được`a`ngửa và không thể vượt quá số lần lật. 
4. Đối với mỗi`c`, lặp lại số điểm có thể có của Petya`d`từ`b`ĐẾN`y`. 

Đây là tất cả các giá trị phù hợp với giới hạn dưới được ghi nhớ của Petya. 
5. Kiểm tra xem`c > d`. 

Vasya nhất định phải thắng trò chơi. Điểm bằng nhau tương ứng với một trận hòa và không được đưa vào. 
6. Nếu điều kiện được giữ, hãy nối thêm`(c, d)`vào danh sách câu trả lời. 
7. In số lượng cặp được lưu trữ. 
8. In từng cặp theo thứ tự chúng được tạo. 

Bởi vì`c`đầu tiên tăng và`d`tăng thứ hai, đầu ra sẽ tự động được sắp xếp theo từ điển. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cặp điểm có thể thỏa mãn giới hạn dưới được ghi nhớ và giới hạn vật lý được áp đặt bởi số lần tung đồng xu. 

Đối với mỗi cặp, nó sẽ kiểm tra điều kiện thắng chính xác từ bài toán. Không có cặp hợp lệ nào có thể bị bỏ qua vì mọi kết quả khả thi đều xuất hiện ở đâu đó trong các vòng lặp lồng nhau. Không thể in cặp không hợp lệ vì chúng tôi chỉ thêm các cặp có số điểm của Vasya lớn hơn điểm của Petya. 

Thứ tự lặp đảm bảo thứ tự từ điển một cách tự động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, y, a, b = map(int, input().split())

    ans = []

    for c in range(a, x + 1):
        for d in range(b, y + 1):
            if c > d:
                ans.append((c, d))

    print(len(ans))

    for c, d in ans:
        print(c, d)

solve()
```Chương trình tuân theo thuật toán được mô tả ở trên. 

Vòng lặp bên ngoài lặp lại tất cả các điểm có thể có của Vasya. Vòng lặp bên trong lặp qua tất cả các điểm có thể có của Petya. điều kiện`c > d`khớp chính xác với yêu cầu Vasya thắng. 

Phạm vi sử dụng`x + 1`Và`y + 1`bởi vì Python`range`loại trừ điểm cuối phía trên. quên đi`+1`sẽ bỏ qua điểm tối đa có thể một cách sai lầm. 

Các câu trả lời được lưu trước khi in vì dòng đầu ra đầu tiên phải chứa tổng số cặp hợp lệ. Chúng tôi chỉ biết số lượng đó sau khi việc liệt kê kết thúc. 

Yêu cầu đặt hàng được xử lý một cách tự nhiên bởi cấu trúc vòng lặp. Từ`c`tăng một cách đơn điệu và`d`tăng đơn điệu cho mỗi cố định`c`, trình tự được tạo ra đã được sắp xếp chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 1 1
```### Dấu vết 

| c | d | c > d | Đã thêm vào câu trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | Không | Không | 
| 1 | 2 | Không | Không | 
| 2 | 1 | Có | (2, 1) | 
| 2 | 2 | Không | Không | 
| 3 | 1 | Có | (3, 1) | 
| 3 | 2 | Có | (3, 2) | 

Danh sách câu trả lời cuối cùng:```
(2, 1)
(3, 1)
(3, 2)
```Đầu ra:```
3
2 1
3 1
3 2
```Ví dụ này thể hiện rõ ràng sự bất đẳng thức chặt chẽ. Các cặp có số điểm bằng nhau sẽ bị loại vì chúng thể hiện kết quả hòa. 

### Ví dụ 2 

đầu vào:```
3 5 1 4
```### Dấu vết 

| c | d | c > d | Đã thêm vào câu trả lời | 
| --- | --- | --- | --- | 
| 1 | 4 | Không | Không | 
| 1 | 5 | Không | Không | 
| 2 | 4 | Không | Không | 
| 2 | 5 | Không | Không | 
| 3 | 4 | Không | Không | 
| 3 | 5 | Không | Không | 

Danh sách câu trả lời cuối cùng trống. 

Đầu ra:```
0
```Ví dụ này minh họa trường hợp điểm ghi nhớ tối thiểu của Petya đã ngăn cản Vasya giành chiến thắng trong mọi tình huống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((x - a + 1)(y - b + 1)) | Chúng tôi kiểm tra mọi cặp có thể một lần | 
| Không gian | O(k) |`k`là số cặp hợp lệ được lưu trữ | 

Vì tất cả các giá trị lớn nhất`100`, tổng số lần lặp nhiều nhất là khoảng mười nghìn. Điều này rất nhỏ so với giới hạn thời gian, vì vậy giải pháp dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    x, y, a, b = map(int, input().split())

    ans = []

    for c in range(a, x + 1):
        for d in range(b, y + 1):
            if c > d:
                ans.append((c, d))

    out = [str(len(ans))]

    for c, d in ans:
        out.append(f"{c} {d}")

    print("\n".join(out))

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

# provided sample
assert run("3 2 1 1\n") == (
    "3\n"
    "2 1\n"
    "3 1\n"
    "3 2\n"
), "sample 1"

# minimum-size input
assert run("1 1 1 1\n") == (
    "0\n"
), "minimum case"

# exactly one valid pair
assert run("2 1 2 1\n") == (
    "1\n"
    "2 1\n"
), "single valid answer"

# no valid outcomes
assert run("3 5 1 4\n") == (
    "0\n"
), "Petya always ahead or tied"

# boundary ordering check
assert run("3 3 1 1\n") == (
    "3\n"
    "2 1\n"
    "3 1\n"
    "3 2\n"
), "lexicographical order"

# maximum-size style case
out = run("100 100 100 100\n")
assert out == "0\n", "largest equal bounds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`0`| Ràng buộc tối thiểu và bất bình đẳng nghiêm ngặt | 
|`2 1 2 1`| Một cặp`(2,1)`| Bao gồm ranh giới chính xác | 
|`3 5 1 4`|`0`| Không tồn tại kết quả hợp lệ | 
|`3 3 1 1`| Đặt hàng cặp hợp lệ | Thứ tự từ điển | 
|`100 100 100 100`|`0`| Giá trị biên tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 1 1 1
```Thuật toán chỉ kiểm tra một cặp: 

| c | d | c > d | 
| --- | --- | --- | 
| 1 | 1 | Không | 

Vì điều kiện không thành công nên danh sách câu trả lời vẫn trống và chương trình in ra:```
0
```Điều này xử lý chính xác trường hợp vẽ. 

Bây giờ hãy xem xét tình huống mà điểm ghi nhớ tối thiểu của Petya đã quá cao:```
3 5 1 4
```Vòng lặp bên ngoài tạo ra`c = 1, 2, 3`. Vòng lặp bên trong tạo ra`d = 4, 5`. Mọi so sánh đều thất bại vì`c`không bao giờ lớn hơn`d`. 

Thuật toán in chính xác không có kết quả hợp lệ. 

Cuối cùng, hãy xem xét một ví dụ nhạy cảm với thứ tự:```
4 3 2 1
```Các cặp hợp lệ được tạo là:```
2 1
3 1
3 2
4 1
4 2
4 3
```Các vòng lặp lồng nhau tự nhiên tạo ra thứ tự từ điển vì`c`đầu tiên tăng lên, sau đó`d`. Không cần bước sắp xếp bổ sung.
