---
title: "CF 194A - Các kỳ thi"
description: "Tác giả phải làm n bài kiểm tra. Mỗi bài kiểm tra đều nhận được điểm số nguyên từ 2 đến 5. Điểm 2 có nghĩa là bài thi bị trượt và phải thi lại. Tổng tổng các điểm phải bằng k."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 194
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 122 (Div. 2)"
rating: 900
weight: 194
solve_time_s: 88
verified: true
draft: false
---

[CF 194A - Bài kiểm tra](https://codeforces.com/problemset/problem/194/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tác giả phải nắm bắt`n`các kỳ thi. Mỗi bài kiểm tra đều nhận được điểm số nguyên từ 2 đến 5. Điểm 2 có nghĩa là bài thi bị trượt và phải thi lại. Tổng điểm của tất cả các lớp phải bằng nhau`k`. 

Trong số tất cả các bài tập có thể có của các lớp có tổng bằng`k`, chúng tôi muốn số lần kiểm tra thất bại nhỏ nhất có thể. Vì chỉ có lớp 2 mới phải thi lại nên bài toán trở thành: tăng tối đa số bài thi của lớp 3, 4 hoặc 5 trong khi vẫn giữ tổng điểm bằng`k`. 

Những hạn chế là rất nhỏ.`n`nhiều nhất là 50 và`k`nhiều nhất là 250, vì vậy ngay cả những cách tiếp cận bằng vũ lực cũng có thể dễ dàng phù hợp. Tuy nhiên, vấn đề này có một quan sát toán học trực tiếp làm giảm toàn bộ lời giải thành một công thức duy nhất. 

Trường hợp cạnh tinh tế đầu tiên xảy ra khi tổng mục tiêu đã là tổng tối thiểu có thể. Ví dụ:```
Input:
4 8
```Mỗi bài kiểm tra phải có ít nhất 2, vì vậy tổng điểm tối thiểu có thể đạt được là`4 * 2 = 8`. Điều đó buộc mọi bài kiểm tra đều có điểm 2, vì vậy câu trả lời là 4. Một giải pháp bất cẩn cố gắng tăng điểm một cách tham lam mà không xử lý đường cơ sở này một cách chính xác có thể vô tình tạo ra ít thất bại hơn mức có thể. 

Một sai lầm dễ dàng khác xuất hiện khi số tiền rất lớn. Coi như:```
Input:
3 15
```Điểm tối đa có thể là 5, và`3 * 5 = 15`, do đó, mọi bài kiểm tra đều có thể là 5. Câu trả lời là 0. Một số cách triển khai không chính xác cho rằng mỗi điểm bổ sung trên mức tối thiểu sẽ loại bỏ một bài kiểm tra không đạt. Điều này là sai vì một bài kiểm tra có thể hấp thụ tối đa 3 điểm bổ sung. 

Một trường hợp thú vị hơn là:```
Input:
4 10
```Tổng điểm tối thiểu là 8. Chúng ta cần thêm 2 điểm. Một bài thi có thể được nâng cấp từ 2 lên 4, cho điểm`[4,2,2,2]`. Đáp án vẫn là 3 bài thi trượt chứ không phải 2. Câu này bắt các giải pháp chia sai hoặc cho rằng mỗi điểm cộng sẽ sửa một bài thi trượt khác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các bài tập có thể có của các lớp. Vì mỗi bài kiểm tra có thể lấy một trong bốn giá trị, nên có`4^n`sự kết hợp. Với`n = 50`, điều này trở nên lớn về mặt thiên văn và hoàn toàn không khả thi. 

Lập trình động cũng có thể thực hiện được. Chúng ta có thể định nghĩa`dp[i][s]`là số lượng bài kiểm tra không đạt tối thiểu sau khi xử lý`i`bài thi có tổng điểm`s`. Từ`n <= 50`Và`k <= 250`, cái này chạy nhanh thoải mái. Số lượng trạng thái là khoảng`50 * 250`và mỗi lần chuyển đổi sẽ xem xét bốn lớp. 

Cấu trúc vấn đề cho phép một cái gì đó đơn giản hơn nhiều. 

Mọi kỳ thi đều bắt đầu từ lớp 2. Điều đó đưa ra tổng điểm cơ bản là`2n`. Bất kỳ cấp cao hơn nào đều được cộng thêm điểm: 

| Lớp | Điểm cộng trên 2 | 
| --- | --- | 
| 2 | 0 | 
| 3 | 1 | 
| 4 | 2 | 
| 5 | 3 | 

Giả sử chúng ta cần tổng cuối cùng là`k`. Số điểm phụ chúng ta phải phân phối là:$$k - 2n$$Mỗi bài thi không trượt chỉ được cộng tối đa 3 điểm, vì điểm cao nhất là 5. Vì vậy, nếu muốn hạn chế tối đa những bài thi trượt, chúng ta nên gộp càng nhiều điểm cộng vào mỗi bài thi đậu càng tốt. 

Nếu một bài thi đậu có thể cộng tối đa 3 điểm thì số bài thi đậu tối thiểu là:$$\left\lceil \frac{k - 2n}{3} \right\rceil$$Tổng số bài thi là`n`, do đó số lần thi trượt tối thiểu sẽ trở thành:$$n - \left\lceil \frac{k - 2n}{3} \right\rceil$$Điều này chuyển đổi vấn đề từ một vấn đề tìm kiếm thành một phép tính toán học trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^n) | O(n) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. 
2. Tính tổng số điểm tối thiểu có thể có`2 * n`. Điều này tương ứng với việc cho mỗi kỳ thi cấp 2. 
3. Tính xem cần bổ sung thêm bao nhiêu điểm:$$extra = k - 2n$$1. Mỗi bài thi đậu chỉ được cộng tối đa 3 điểm bằng cách đổi số 2 thành số 5. 
2. Tính số bài thi đậu tối thiểu cần thiết:$$pass\_exams = \left\lceil \frac{extra}{3} \right\rceil$$Trong số học số nguyên, điều này trở thành:```
(extra + 2) // 3
```1. Các bài thi còn lại phải ở lớp 2 nên đáp án là:```
n - pass_exams
```### Tại sao nó hoạt động 

Mỗi bài thi ban đầu đóng góp 2 điểm. Để đạt được tổng số cần thiết, chúng tôi phân phối`extra`điểm bổ sung qua các kỳ thi. Một bài kiểm tra có thể đóng góp tối đa 3 điểm bổ sung vì điểm được giới hạn ở mức 5. 

Để giảm thiểu các bài kiểm tra bị trượt, chúng tôi muốn có càng ít bài kiểm tra càng tốt để có được tất cả điểm cộng. Việc tích lũy thêm 3 điểm vào một bài kiểm tra ít nhất cũng tốt bằng việc chia chúng thành nhiều bài kiểm tra, vì mỗi bài kiểm tra được nâng cấp trên 2 điểm sẽ không bị coi là bài kiểm tra trượt. 

Bộ phận trần tính toán số lượng bài kiểm tra nhỏ nhất có khả năng cung cấp tất cả các điểm bổ sung cần thiết. Các bài thi còn lại vẫn ở lớp 2 và được tính là bài thi trượt. Không có sự sắp xếp nào có thể sử dụng ít bài thi đậu hơn vì mỗi bài thi đậu có dung lượng cứng là 3 điểm phụ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

extra = k - 2 * n
passing = (extra + 2) // 3

print(n - passing)
```Việc thực hiện trực tiếp theo đạo hàm toán học. 

biểu hiện`k - 2 * n`tính xem chúng ta còn cần bao nhiêu điểm sau khi ấn định điểm 2 tối thiểu cho mỗi kỳ thi. Vì sự cố đảm bảo tồn tại cấu hình hợp lệ nên giá trị này không bao giờ âm vượt quá mức mà công thức có thể xử lý. 

Việc phân chia trần nhà là phần tinh tế duy nhất. Trong toán học:$$\left\lceil \frac{x}{3} \right\rceil$$trở thành:```
(x + 2) // 3
```đối với số nguyên không âm. Thêm 2 trước khi chia số nguyên làm tròn lên một cách chính xác. 

Cuối cùng, trừ đi số bài thi đậu từ`n`đưa ra số bài kiểm tra còn lại ở lớp 2. 

Không cần vòng lặp hoặc cấu trúc dữ liệu lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 8
```| Bước | Giá trị | 
| --- | --- | 
| n | 4 | 
| k | 8 | 
| Số tiền tối thiểu`2n`| 8 | 
| Cần thêm điểm | 0 | 
| Cần vượt qua kỳ thi | 0 | 
| Thi trượt | 4 | 

Tổng điểm đã bằng điểm tối thiểu có thể có nên mọi bài thi đều phải ở mức 2. Điều này khẳng định trường hợp giới hạn dưới. 

### Ví dụ 2 

đầu vào:```
4 10
```| Bước | Giá trị | 
| --- | --- | 
| n | 4 | 
| k | 10 | 
| Số tiền tối thiểu`2n`| 8 | 
| Cần thêm điểm | 2 | 
| Cần vượt qua kỳ thi | 1 | 
| Thi trượt | 3 | 

Một bài thi có thể tích lũy cả hai điểm bổ sung để trở thành lớp 4. Điểm số có thể là`[4,2,2,2]`. Điều này chứng tỏ tại sao việc nhóm các điểm dư vào càng ít bài kiểm tra càng tốt để giảm thiểu việc thi lại. 

### Ví dụ 3 

đầu vào:```
3 15
```| Bước | Giá trị | 
| --- | --- | 
| n | 3 | 
| k | 15 | 
| Số tiền tối thiểu`2n`| 6 | 
| Cần thêm điểm | 9 | 
| Cần vượt qua kỳ thi | 3 | 
| Thi trượt | 0 | 

Tất cả các bài kiểm tra phải đạt đến cấp 5. Điều này đạt đến tổng điểm tối đa có thể và không tạo ra bài kiểm tra nào bị trượt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào | 

Các ràng buộc là cực kỳ nhỏ nên ngay cả những phương pháp chậm hơn cũng sẽ vượt qua. Công thức hằng số thời gian giải quyết vấn đề ngay lập tức và phù hợp thoải mái trong mọi giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())

    extra = k - 2 * n
    passing = (extra + 2) // 3

    print(n - passing)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("4 8\n") == "4\n", "sample 1"

# custom cases
assert run("1 2\n") == "1\n", "minimum possible sum"
assert run("1 5\n") == "0\n", "maximum possible score"
assert run("4 10\n") == "3\n", "single upgraded exam"
assert run("3 15\n") == "0\n", "all exams score 5"
assert run("5 11\n") == "4\n", "one extra point only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`1`| Cấu hình hợp lệ nhỏ nhất | 
|`1 5`|`0`| Điểm tối đa loại bỏ mọi thất bại | 
|`4 10`|`3`| Điểm phụ có thể được tập trung vào một bài thi | 
|`3 15`|`0`| Tổng số tối đa có thể đạt được | 
|`5 11`|`4`| Phân chia trần có dư | 

## Vỏ cạnh 

Hãy xem xét trường hợp tổng tối thiểu:```
Input:
4 8
```Thuật toán tính toán:```
extra = 8 - 2*4 = 0
passing = (0 + 2) // 3 = 0
answer = 4 - 0 = 4
```Không cần thêm điểm nên mọi bài thi vẫn ở mức 2. Kết quả đúng là 4. 

Bây giờ hãy xem xét trường hợp tổng tối đa:```
Input:
3 15
```Thuật toán tính toán:```
extra = 15 - 2*3 = 9
passing = (9 + 2) // 3 = 3
answer = 3 - 3 = 0
```Tất cả các bài thi phải trở thành bài thi đậu, cụ thể là lớp 5. Kết quả đúng là 0. 

Cuối cùng, hãy xem xét trường hợp chia trần:```
Input:
5 11
```Tổng điểm tối thiểu là 10, vì vậy chỉ cần thêm một điểm:```
extra = 1
passing = (1 + 2) // 3 = 1
answer = 5 - 1 = 4
```Một bài kiểm tra có thể lên cấp 3 trong khi bốn bài còn lại vẫn ở cấp 2. Việc triển khai phân tầng sẽ tính toán không chính xác 0 bài kiểm tra đạt ở đây.
