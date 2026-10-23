---
title: "CF 105931A - \u041d\u043e\u0432\u044b\u0439 \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442"
description: "Trường đại học đang tuyển sinh tổng cộng n sinh viên. Mỗi học sinh phải thuộc một trong hai loại. Nếu sinh viên đóng học phí, họ sẽ đóng góp một khoản cố định cho trường đại học. Nếu một sinh viên nhận được học bổng, nguồn tài trợ của họ sẽ đến từ một nhà tài trợ."
date: "2026-06-22T15:43:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105931
codeforces_index: "A"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2024"
rating: 0
weight: 105931
solve_time_s: 65
verified: true
draft: false
---

[CF 105931A - \u041d\u043e\u0432\u044b\u0439 \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442](https://codeforces.com/problemset/problem/105931/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trường đại học đang tuyển sinh lên tới`n`tổng số học sinh. Mỗi học sinh phải thuộc một trong hai loại. 

Nếu sinh viên đóng học phí, họ sẽ đóng góp một khoản cố định`a`đến trường đại học. 

Nếu một sinh viên nhận được học bổng, nguồn tài trợ của họ sẽ đến từ một nhà tài trợ. Nhà tài trợ hỗ trợ tối đa`b`sinh viên và số tiền giảm tuyến tính: học bổng đầu tiên mang lại`b`, thứ hai cho`b - 1`, v.v. cho đến khi nhận được học bổng thứ k`b - k + 1`. 

Trường đại học quyết định số lượng sinh viên`k`trong tổng số`n`sẽ là người nhận học bổng, với hạn chế là`k`không thể vượt quá`b`. Tất cả còn lại`n - k`sinh viên đang trả tiền cho sinh viên. Mục tiêu là chọn`k`sao cho tổng số tiền nhận được từ học phí cộng với tài trợ là tối đa. 

Đầu ra là tổng doanh thu tối đa có thể đạt được. 

Những ràng buộc cho phép`n`,`a`, Và`b`lên đến`10^9`. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử mọi giá trị có thể có của`k`và tính toán lại kết quả theo thời gian tuyến tính. Thậm chí lặp đi lặp lại tất cả`k`là không thể bởi vì`k`bản thân nó có thể lên tới`10^9`. 

Mô phỏng trực tiếp cũng có vấn đề vì phần học thuật liên quan đến việc tính tổng một cấp số cộng giảm dần, tăng dần theo phương trình bậc hai theo`k`. Bất kỳ mô phỏng ngây thơ nào cho mỗi học sinh sẽ quá chậm. 

Một trường hợp thất bại tinh tế xuất hiện khi một người cố gắng đưa ra những lý luận tham lam như luôn nhận càng nhiều học bổng càng tốt hoặc luôn ưu tiên những sinh viên được trả lương. Ví dụ, nếu`a`nhỏ, học bổng vẫn có thể tốt hơn cho những giá trị ban đầu của`k`nhưng sau này còn tệ hơn khi học bổng cận biên giảm xuống dưới`a`. Điều này tạo ra mức cao nhất trong tổng lợi nhuận chứ không phải là một hành vi đơn điệu, do đó các quyết định cục bộ sẽ thất bại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi số lượng sinh viên nhận học bổng có thể`k`từ`0`ĐẾN`min(n, b)`. Đối với mỗi lựa chọn, chúng tôi tính toán doanh thu là`(n - k) * a`cộng với tổng cấp số cộng`b + (b - 1) + ... + (b - k + 1)`. 

Tính toán từng tổng theo O(k) sẽ tạo ra nghiệm O(n^2) trong trường hợp xấu nhất, quá lớn. Ngay cả khi chúng ta sử dụng công thức chuỗi số học để tính từng ứng viên trong O(1), chúng ta vẫn cần phải kiểm tra tới`10^9`giá trị của`k`, điều đó là không thể. 

Quan sát quan trọng là hàm mục tiêu không phải là hàm tùy ý. Tổng học bổng là bậc hai trong`k`và phần học phí tuyến tính theo`k`. Hàm tổng trở thành hàm bậc hai lõm trong`k`. Điều đó có nghĩa là nó có một đỉnh duy nhất và khi chúng ta di chuyển qua nó, giá trị chỉ giảm đi. 

Cấu trúc này cho phép chúng ta tránh việc tìm kiếm toàn bộ phạm vi. Thay vì liệt kê tất cả các giá trị, chúng tôi tính toán vị trí của đỉnh một cách phân tích và chỉ kiểm tra một số lượng không đổi các ứng cử viên lân cận, cùng với các giá trị biên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(min(n, b)) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu thị tổng lợi nhuận theo hàm số của`k`, Ở đâu`k`là số lượng sinh viên được nhận học bổng. Phần còn lại`n - k`sinh viên đóng học phí, đóng góp`(n - k) * a`. Đóng góp học bổng là một cấp số cộng với`k`điều khoản bắt đầu từ`b`. 
2. Viết lại số tiền học bổng bằng công thức dạng đóng. Tổng trở thành`k * (2b - k + 1) / 2`. Điều này loại bỏ bất kỳ nhu cầu lặp lại. 
3. Kết hợp cả hai phần thành một biểu thức duy nhất tùy thuộc vào`k`. Sau khi đơn giản hóa, bài toán giảm xuống mức cực đại hóa hàm bậc hai trong`k`trên phạm vi số nguyên`[0, min(n, b)]`. 
4. Xác định hàm số lõm vì hệ số của`k^2`là tiêu cực. Điều này đảm bảo một điểm cực đại duy nhất, vì vậy chúng ta chỉ cần xác định vị trí đỉnh thay vì tìm kiếm một cách thấu đáo. 
5. Tính điểm tối ưu có giá trị thực bằng cách sử dụng ý tưởng đạo hàm, cho kết quả`k* = (2b - 2a + 1) / 2`. Điều này thể hiện mức thu nhập cận biên từ học bổng cân bằng với mức tăng học phí cố định. 
6. Vì`k`phải là một số nguyên, hãy xem xét các số nguyên gần nhất xung quanh`k*`. Cũng bao gồm các giá trị biên`0`Và`min(n, b)`bởi vì mức tối ưu có thể nằm ngoài khoảng khả thi hoặc bị cắt bớt bởi các ràng buộc. 
7. Đánh giá lợi nhuận của từng ứng viên`k`và lấy tối đa. 

### Tại sao nó hoạt động 

Hàm lợi nhuận kết thúc`k`là một bậc hai lõm, nghĩa là nó giảm ở cả hai phía của một đỉnh. Bất kỳ số nguyên tối đa nào cũng phải nằm ở mức sàn hoặc mức trần của mức tối ưu thực hoặc ở ranh giới của khoảng hợp lệ. Bằng cách chỉ kiểm tra những điểm đó, chúng tôi đảm bảo nắm bắt được mức tối đa toàn cầu mà không cần liệt kê tất cả các khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def calc(n, a, b, k):
    # tuition part + scholarship part
    return (n - k) * a + k * (2 * b - k + 1) // 2

def solve():
    n = int(input())
    a = int(input())
    b = int(input())

    max_k = min(n, b)

    # continuous optimum
    k_star = (2 * b - 2 * a + 1) / 2

    candidates = set()

    for x in [0, max_k]:
        candidates.add(x)

    for x in [int(k_star), int(k_star) + 1]:
        candidates.add(x)

    best = 0
    for k in candidates:
        if 0 <= k <= max_k:
            best = max(best, calc(n, a, b, k))

    print(best)

if __name__ == "__main__":
    solve()
```chức năng`calc`trực tiếp đánh giá công thức lợi nhuận mà không cần mô phỏng. Điều này rất quan trọng vì việc mở rộng quy trình theo từng học sinh sẽ không khả thi. 

Tập ứng cử viên bao gồm cả ranh giới và vùng lân cận xung quanh điểm tối ưu phân tích. Đúc`k_star`thành số nguyên và cũng kiểm tra giá trị tiếp theo để đảm bảo chúng ta không bỏ lỡ giá trị tối đa rời rạc do hiệu ứng sàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp trong đó`n = 4`,`a = 4`,`b = 3`. 

Phạm vi khả thi cho`k`đến từ`0`ĐẾN`3`. 

Chúng tôi tính toán các giá trị: 

| k | học phí (4-k)*a | số tiền học bổng | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 16 | 0 | 16 | 
| 1 | 12 | 3 | 15 | 
| 2 | 8 | 5 | 13 | 
| 3 | 4 | 6 | 10 | 

Tối đa là ở`k = 0`, nghĩa là không có học bổng là tối ưu vì học phí cao so với giá trị học bổng đang giảm dần. Điều này chứng tỏ hàm có thể giảm ngay từ đầu. 

Bây giờ hãy xem xét`n = 10`,`a = 2`,`b = 6`. 

| k | học phí | học bổng | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 20 | 0 | 20 | 
| 1 | 18 | 6 | 24 | 
| 2 | 16 | 11 | 27 | 
| 3 | 14 | 15 | 29 | 
| 4 | 12 | 18 | 30 | 
| 5 | 10 | 20 | 30 | 
| 6 | 8 | 21 | 29 | 

Giá trị tốt nhất xảy ra xung quanh`k = 4 or 5`, cho thấy một đỉnh rõ ràng chứ không phải là sự tăng trưởng hoặc phân rã đơn điệu. Đây chính xác là hình dạng được mong đợi từ một phương trình bậc hai lõm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số lượng giá trị ứng cử viên không đổi được đánh giá | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài một vài biến | 

Lời giải không phụ thuộc vào độ lớn của`n`,`a`, Và`b`, điều này làm cho nó nằm trong giới hạn ngay cả đối với`10^9`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return out.getvalue().strip()

# minimal case
assert run("1\n5\n10\n") == "10"

# no scholarships beneficial
assert run("5\n10\n3\n") == str(5 * 10)

# all scholarships beneficial
assert run("5\n1\n10\n") == str((5 - 5) * 1 + 5 * (2 * 10 - 5 + 1) // 2)

# balanced case
assert run("10\n2\n6\n") == "30"

# boundary n < b
assert run("3\n5\n10\n") == str((3 * (2 * 10 - 3 + 1) // 2))

# large equal values
assert run("1000000000\n1\n1\n") == str(1000000000)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 10 | 10 | ranh giới sinh viên độc thân | 
| 5 10 3 | 50 | không có học bổng là tối ưu | 
| 5 1 10 | ưu đãi học bổng lớn | thống trị học bổng toàn phần | 
| 10 2 6 | 30 | nội thất tối đa | 
| 3 5 10 | giới hạn học bổng nhỏ hơn n | cắt ranh giới | 
| 1e9 1 1 | ổn định quy mô lớn | công thức chống tràn an toàn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`a`lớn đến mức bất kỳ học bổng nào cũng làm giảm tổng lợi nhuận ngay lập tức. Ví dụ,`n = 5`,`a = 10`,`b = 3`. Chức năng tại`k = 1`đã giảm so với`k = 0`. Thuật toán vẫn xử lý việc này một cách chính xác vì`k = 0`được đưa vào rõ ràng trong số các ứng cử viên, đảm bảo không bỏ sót mức tối đa. 

Một trường hợp cạnh khác xảy ra khi`b`nhỏ hơn`n`. Ví dụ,`n = 10`,`b = 3`,`a = 1`. Mặc dù có nhiều sinh viên nhưng học bổng không được vượt quá 3. Thuật toán sẽ kẹp chính xác phạm vi tìm kiếm để`min(n, b)`, vì vậy nó không bao giờ đánh giá các giá trị không hợp lệ của`k`. 

Trường hợp cạnh cuối cùng là khi đỉnh bậc hai nằm ngoài khoảng khả thi. Ví dụ, nếu tính toán`k*`là tiêu cực, chẳng hạn như khi`a`là rất lớn so với`b`, mức tối đa hợp lệ xảy ra tại`k = 0`. Vì thuật toán luôn kiểm tra các giá trị biên nên nó xử lý chính xác tình huống này mà không cần dựa vào đỉnh phân tích.
