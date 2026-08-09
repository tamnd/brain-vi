---
title: "CF 102441H - Không phải A + B"
description: "Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được hai số nguyên dương a và b. Chúng ta phải in một số nguyên c từ 1 đến 50 sao cho c khác với a + b. Không có yêu cầu phải tìm giá trị hợp lệ nhỏ nhất hoặc lớn nhất, vì vậy bất kỳ giá trị nào thỏa mãn điều kiện đều được chấp nhận."
date: "2026-08-08T13:36:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "H"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 56
verified: true
draft: false
---

[CF 102441H - Không phải A + B](https://codeforces.com/problemset/problem/102441/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được hai số nguyên dương`a`Và`b`. Chúng ta phải in một số số nguyên`c`giữa`1`Và`50`như vậy`c`khác với`a + b`. Không có yêu cầu phải tìm giá trị hợp lệ nhỏ nhất hoặc lớn nhất, vì vậy bất kỳ giá trị nào thỏa mãn điều kiện đều được chấp nhận. 

Các giới hạn làm cho vấn đề trở nên cực kỳ nhỏ. Có nhiều nhất`10^3`các trường hợp thử nghiệm và cả hai số đầu vào đều có nhiều nhất`50`. Ngay cả một cách tiếp cận kiểm tra mọi giá trị có thể có của`c`từ`1`bởi vì`50`thực hiện nhiều nhất`50 * 10^3 = 50,000`kiểm tra, thấp hơn nhiều so với giới hạn một giây có thể thách thức. Các ràng buộc không loại trừ được vũ lực chút nào. Mục tiêu thực sự là nhận ra rằng các giới hạn trên`a`Và`b`cho chúng tôi một câu trả lời thậm chí còn đơn giản hơn. 

Trường hợp cạnh khóa là giá trị nhỏ nhất có thể. Nếu như`a = 1`Và`b = 1`, tổng của chúng là`2`, Vì thế`c = 1`vẫn còn hiệu lực. Giải pháp bất cẩn có thể cho rằng việc chọn một trong các số đầu vào là không an toàn, nhưng ở đây`a`Và`b`đều dương nên tổng của chúng luôn lớn hơn một trong hai số riêng lẻ. Ví dụ, đối với đầu vào`1 1`, xuất ra`1`là đúng bởi vì`1 != 2`. 

Lý do tương tự xử lý các giá trị lớn nhất. Nếu như`a = 50`Và`b = 50`, tổng là`100`, trong khi`c`phải nhiều nhất`50`. Như vậy mọi giá trị cho phép của`c`tự động khác với tổng. Đặc biệt,`c = 1`là hợp lệ. 

Ranh giới quan trọng thực sự không phải là giới hạn trên của`50`, nhưng thực tế là cả hai`a`Và`b`ít nhất là`1`. Vì điều đó,`a + b >= 2`, do đó giá trị cố định`c = 1`không bao giờ có thể bằng tổng. 

Định dạng mẫu của câu lệnh bị hỏng trong văn bản được cung cấp. Mẫu ban đầu có ba trường hợp thử nghiệm,`(1, 2)`,`(3, 4)`, Và`(5, 6)`, với một đầu ra có thể là`12`,`34`, Và`42`. Những kết quả đầu ra đó chứng tỏ rằng câu trả lời không phải là duy nhất. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi ứng viên`c`từ`1`ĐẾN`50`và dừng lại ngay khi tìm thấy một điều thỏa mãn`c != a + b`. Điều này đúng vì vòng lặp kiểm tra mọi giá trị được cho phép bởi các ràng buộc đầu ra, vì vậy nếu một giải pháp tồn tại, vòng lặp sẽ gặp phải giải pháp đó. Dưới những ràng buộc đã cho, ngay cả trường hợp xấu nhất cũng chỉ thực hiện`50`so sánh cho mỗi trường hợp thử nghiệm, hoặc nhiều nhất`50,000`so sánh cho tất cả`10^3`trường hợp thử nghiệm. Do đó, vũ lực ở đây không quá chậm và được chấp nhận hoàn toàn. 

Cách tiếp cận bạo lực có hiệu quả vì phạm vi ứng cử viên rất nhỏ, nhưng chúng ta có thể làm tốt hơn bằng cách xem xét cấu trúc số học. Từ`a >= 1`Và`b >= 1`, chúng tôi luôn có`a + b >= 2`. giá trị`1`là đầu ra được phép và nó không bao giờ có thể bằng tổng của hai giá trị đầu vào dương. Điều đó loại bỏ hoàn toàn việc tìm kiếm. 

Sự quan sát đó`1`được đảm bảo là khác với`a + b`cho phép chúng tôi thay thế tối đa`50`ứng viên kiểm tra bằng một bài tập. Chúng tôi thậm chí không cần phải tính tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(50t), hiệu quả là O(t) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Mỗi trường hợp thử nghiệm là độc lập, do đó, có thể sử dụng cùng một cấu trúc cố định mọi lúc. 
2. Cho mỗi cặp`a, b`, đầu ra`1`. Đây là lựa chọn đúng vì cả hai số đều dương nên`a + b`ít nhất là`2`. 
3. Lặp lại điều này cho tất cả các trường hợp thử nghiệm. Không cần kiểm tra giá trị thực của`a`Và`b`, vì giới hạn dưới`1`là đủ để chứng minh câu trả lời là hợp lệ. 

### Tại sao nó hoạt động 

Đối với mỗi trường hợp thử nghiệm,`a >= 1`Và`b >= 1`. Kể từ đây`a + b >= 2`. Đầu ra của thuật toán`c = 1`, Vì thế`c < a + b`và do đó`c != a + b`. Cũng,`1 <= c <= 50`, do đó đầu ra thỏa mãn phạm vi yêu cầu. Đối số tương tự được áp dụng độc lập cho mọi trường hợp thử nghiệm, điều này chứng tỏ rằng thuật toán không bao giờ có thể tạo ra câu trả lời không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    a, b = map(int, input().split())
    print(1)
```Dòng đầu tiên ghi số lượng test case. Sau đó chúng tôi xử lý chính xác`t`cặp, phù hợp với định dạng đầu vào. 

Các giá trị`a`Và`b`được đọc ngay cả khi việc triển khai không cần sử dụng chúng. Việc đọc chúng là cần thiết để sử dụng đầu vào một cách chính xác và chuyển sang trường hợp kiểm thử tiếp theo. 

Đầu ra luôn là`1`. Không có phép tính tổng, so sánh, lặp qua các giá trị ứng viên hoặc trường hợp đặc biệt vì tính dương của`a`Và`b`đã chứng minh điều đó rồi`1`không thể bằng tổng của chúng. 

Không có lo ngại về tràn vì các giá trị đã cho rất nhỏ và việc triển khai không thực hiện bất kỳ phép tính nào trên chúng. Cũng không có vấn đề riêng lẻ nào trong vòng lặp trường hợp thử nghiệm vì nó thực thi chính xác một lần cho mỗi cặp đầu vào. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu chính thức:```
3
1 2
3 4
5 6
```Cấu trúc của chúng tôi không cố gắng tái tạo chính xác đầu ra mẫu vì vấn đề chấp nhận bất kỳ giá trị hợp lệ nào.`c`. 

| Trường hợp thử nghiệm | một | b | a + b | c | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 1 | Có | 
| 2 | 3 | 4 | 7 | 1 | Có | 
| 3 | 5 | 6 | 11 | 1 | Có | 

Trường hợp thử nghiệm đầu tiên cũng bao gồm giá trị nhỏ nhất có thể có của`a`. Mặc dù`a`bản thân nó là`1`, tổng là`3`, vì vậy việc in ấn`1`là hoàn toàn hợp lệ. Hai trường hợp còn lại chứng minh rằng công trình xây dựng tương tự được thực hiện mà không tính đến quy mô thực tế của số tiền. 

Đối với ví dụ thứ hai, hãy xem xét các giá trị đầu vào tối đa có thể:```
2
50 50
1 1
```| Trường hợp thử nghiệm | một | b | a + b | c | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 50 | 50 | 100 | 1 | Có | 
| 2 | 1 | 1 | 2 | 1 | Có | 

Hàng đầu tiên cho thấy rằng ngay cả khi tổng lớn hơn toàn bộ phạm vi cho phép đối với`c`, câu trả lời cố định hoạt động. Hàng thứ hai hiển thị chính xác số tiền tối thiểu có thể,`2`, vẫn lớn hơn`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một lần đọc và một lần xuất. | 
| Không gian | O(1) | Chỉ cặp số nguyên hiện tại và trạng thái vòng lặp được lưu trữ. | 

Với nhiều nhất`10^3`trường hợp thử nghiệm, thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi trường hợp. Điều này thoải mái trong giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề cho phép có nhiều đầu ra chính xác nên các thử nghiệm mạnh mẽ nhất sẽ xác minh rằng mọi giá trị được tạo ra đều nằm trong`[1, 50]`và khác với`a + b`, thay vì yêu cầu một câu trả lời hợp lệ cụ thể.```python
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())

    for _ in range(t):
        a, b = map(int, input().split())
        print(1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, out: str):
    input_lines = inp.strip().splitlines()
    t = int(input_lines[0])

    tests = []
    for line in input_lines[1:]:
        a, b = map(int, line.split())
        tests.append((a, b))

    answers = list(map(int, out.strip().split()))

    assert len(answers) == t

    for (a, b), c in zip(tests, answers):
        assert 1 <= c <= 50
        assert c != a + b

# Provided sample input.
sample = """\
3
1 2
3 4
5 6
"""
sample_output = run(sample)
validate(sample, sample_output)

# Minimum-size input.
case_min = """\
1
1 1
"""
assert run(case_min) == "1\n"

# Maximum-size input.
case_max = """\
3
50 50
50 1
1 50
"""
assert run(case_max) == "1\n1\n1\n"
validate(case_max, run(case_max))

# All values equal.
case_equal = """\
4
7 7
25 25
49 49
50 50
"""
assert run(case_equal) == "1\n1\n1\n1\n"
validate(case_equal, run(case_equal))

# Boundary sums around the smallest and largest possible values.
case_boundaries = """\
4
1 1
1 2
49 50
50 50
"""
assert run(case_boundaries) == "1\n1\n1\n1\n"
validate(case_boundaries, run(case_boundaries))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 2 / 3 4 / 5 6`|`1 / 1 / 1`| Cung cấp cấu trúc mẫu và đầu ra không duy nhất | 
|`1 / 1 1`|`1`| Giá trị đầu vào tối thiểu và tổng tối thiểu có thể | 
|`3 / 50 50 / 50 1 / 1 50`|`1 / 1 / 1`| Giá trị đầu vào tối đa và ranh giới cho phép`c`phạm vi | 
|`4 / 7 7 / 25 25 / 49 49 / 50 50`|`1 / 1 / 1 / 1`| Đầu vào bằng nhau trên toàn bộ phạm vi giá trị | 
|`4 / 1 1 / 1 2 / 49 50 / 50 50`|`1 / 1 / 1 / 1`| Số tiền nhỏ nhất và lớn nhất có thể | 

## Vỏ cạnh 

Trường hợp tối thiểu là`a = 1, b = 1`. Đầu ra của thuật toán`1`. Tổng của họ là`2`, vậy điều kiện`c != a + b`trở thành`1 != 2`, đó là sự thật. Đầu vào chính xác là`1\n1 1\n`, và đầu ra là`1`. 

Trường hợp một đầu vào là chính nó`1`đôi khi có thể gây ra trường hợp đặc biệt không cần thiết trong cách giải quyết bất cẩn. Vì`a = 1, b = 2`, thuật toán vẫn xuất ra`1`. Tổng số tiền là`3`, vậy câu trả lời là hợp lệ. Không có lý do gì để tránh`1`chỉ vì một trong các giá trị đầu vào bằng`1`. 

Ở ranh giới khác, hãy xem xét`a = 50, b = 50`. Tổng số tiền là`100`, nằm ngoài phạm vi đầu ra cho phép. Đầu ra của thuật toán`1`, và kể từ đó`1 != 100`, câu trả lời là hợp lệ. Trong thực tế, mọi giá trị cho phép của`c`sẽ làm việc cho bài kiểm tra cụ thể này. 

Cuối cùng, hãy xem xét các đầu vào bằng nhau như`a = 25, b = 25`. Tổng của họ là`50`, vì vậy một giải pháp bất cẩn luôn tạo ra`50`sẽ thất bại. Sản lượng xây dựng cố định`1`, Và`1 != 50`, do đó nó tránh được xung đột biên này mà không cần kiểm tra tổng. Điều này minh họa tại sao việc chọn giá trị được phép nhỏ nhất lại mạnh hơn việc chọn giá trị gần ranh giới trên.
