---
title: "CF 102219C - Tôi Không Muốn Trả Tiền Cho Cái Bình Trả Muộn!"
description: "Nina có một danh sách các nhà hàng. Với mỗi nhà hàng, cô biết hai giá trị: fi, số tiền cô cho là đáng giá của nhà hàng, và ti, số phút cô cần để ăn trưa ở đó. Thời gian ăn trưa của cô ấy là S phút và cô ấy phải chọn đúng một nhà hàng."
date: "2026-08-24T07:26:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "C"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 2304
verified: false
draft: false
---

[CF 102219C - Tôi không muốn trả tiền cho lọ muộn!](https://codeforces.com/problemset/problem/102219/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nina có một danh sách các nhà hàng. Với mỗi nhà hàng, cô biết hai giá trị:`f_i`, số tiền cô ấy coi là đáng giá của nhà hàng, và`t_i`, số phút cô ấy cần để ăn trưa ở đó. Thời gian ăn trưa sẵn có của cô ấy là`S`phút, và cô ấy phải chọn đúng một nhà hàng. 

Nếu một nhà hàng hoàn toàn phù hợp với giờ nghỉ trưa, nghĩa là`t_i <= S`, Nina giữ nguyên giá trị`f_i`. Nếu nhà hàng mất nhiều thời gian hơn, chi phí sẽ tăng thêm`t_i - S`số phút được coi là số tiền được trả vào lọ muộn, do đó số tiền tiết kiệm cuối cùng của cô ấy trở thành`f_i - (t_i - S)`. Nhiệm vụ chỉ đơn giản là tìm ra khoản tiết kiệm cuối cùng lớn nhất trong số tất cả các nhà hàng. 

Biểu thức có thể được viết gọn hơn như sau`value_i = f_i - max(0, t_i - S)`. 

Số lượng nhà hàng nhiều nhất`10^4`, và có nhiều nhất`10`ngày. Ngay cả việc kiểm tra mỗi nhà hàng một lần mỗi ngày cũng chỉ cần khoảng`10^5`đánh giá, rất nhỏ trong giới hạn thời gian 1 giây. Bản thân các giá trị có thể đạt tới`10^9`và việc trừ đi hình phạt về độ trễ có thể làm cho câu trả lời trở thành số âm, do đó việc triển khai phải sử dụng số học số nguyên mà không giả sử câu trả lời là không âm. Số nguyên Python xử lý trực tiếp phạm vi này. 

Các trường hợp cạnh chính đến từ ranh giới một cách chính xác`S`phút và từ các nhà hàng đến muộn đến mức tạo ra khoản tiết kiệm âm. 

Hãy xem xét một nhà hàng sử dụng chính xác thời gian có sẵn:```
1
1 5
10 5
```Đầu ra đúng là:```
Case #1: 10
```Việc thực hiện bất cẩn bằng cách sử dụng`t_i >= S`vì điều kiện đến muộn sẽ bị trừ một phút hoặc nhiều phút không chính xác. Hình phạt bằng 0 khi`t_i == S`. 

Một nhà hàng cũng có thể có giá trị cuối cùng âm:```
1
1 5
1 7
```Đầu ra đúng là:```
Case #1: -1
```Không có yêu cầu Nina có thể chọn bỏ bữa trưa hoặc chọn một nhà hàng có giá trị không âm. Cô ấy phải chọn chính xác một nhà hàng, vì vậy mức tối đa hợp pháp có thể là số âm. 

Một trường hợp ranh giới hữu ích khác có một số nhà hàng có cùng giá trị tốt nhất:```
1
3 5
10 5
8 3
20 8
```Nhà hàng đầu tiên cung cấp`10`, thứ hai cho`8`, và thứ ba cho`17`. Câu trả lời đúng là`17`. Thuật toán chỉ cần giá trị tối đa nên các mối quan hệ không cần xử lý đặc biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp đã là cách tiếp cận phù hợp ở đây. Đối với mỗi nhà hàng, hãy tính xem giá trị danh nghĩa của nó còn tồn tại bao nhiêu sau khi thanh toán cho bất kỳ phút nào ngoài thời gian ăn trưa sẵn có. Nếu như`t_i <= S`, giá trị ứng cử viên chỉ đơn giản là`f_i`. Nếu không thì đó là`f_i - (t_i - S)`. Giữ ứng cử viên lớn nhất được nhìn thấy cho đến nay. 

Điều này đúng vì sự lựa chọn chỉ bao gồm một nhà hàng. Mọi lựa chọn có thể đều được kiểm tra và mức tiết kiệm cuối cùng của nó được tính toán theo các quy tắc. Khi tất cả các nhà hàng đã được xem xét, ứng cử viên lớn nhất chính xác là sự lựa chọn tốt nhất có thể. 

Cách giải thích thô bạo có thể đề xuất thử mọi nhà hàng và thực hiện một số tìm kiếm hoặc sắp xếp bổ sung, nhưng điều đó không cần thiết. Số lượng liên kết với một nhà hàng chỉ phụ thuộc vào nhà hàng đó và giá trị cố định`S`. Không có sự tương tác giữa hai nhà hàng nên việc đánh giá một nhà hàng sẽ đưa ra tất cả thông tin cần thiết về nhà hàng đó. 

Trong một ngày với`N`nhà hàng, thuật toán thực hiện chính xác`N`đánh giá ứng viên và`N`so sánh tối đa trong trường hợp xấu nhất. Sang`D`ngày, nhiều nhất là thế này`D * N`, nhiều nhất là`10 * 10^4 = 10^5`đánh giá nhà hàng. Bất kỳ cách tiếp cận nào sắp xếp các nhà hàng sẽ thêm không cần thiết`O(N log N)`công việc. 

Điều quan trọng cần lưu ý là vấn đề này không đòi hỏi kỹ thuật tối ưu hóa phức tạp. Công thức chuyển đổi mỗi nhà hàng thành một điểm số độc lập và câu trả lời bắt buộc chỉ đơn giản là điểm tối đa. Một lần vượt qua vừa đủ vừa tối ưu về mặt tiệm cận vì mỗi nhà hàng phải được xem xét ít nhất một lần: một nhà hàng không được nhìn thấy luôn có thể có giá trị tốt hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(DN) | O(1) | Đã chấp nhận | 
| Đường chuyền đơn tối ưu | O(DN) | O(1) | Đã chấp nhận | 

Ở đây cái gọi là lực lượng vũ phu và giải pháp tối ưu thực sự là cùng một thuật toán. Sự khác biệt là không có tìm kiếm tổ hợp ẩn nào cần loại bỏ. Việc tối ưu hóa quan trọng là nhận ra rằng việc kiểm tra từng nhà hàng một lần là đủ. 

## Hướng dẫn thuật toán 

1. Đọc số ngày`D`. Mỗi ngày là một bài toán độc lập nên đáp án của ngày này không ảnh hưởng đến ngày khác. 
2. Đối với ngày hiện tại, hãy đọc`N`Và`S`.`N`cho chúng tôi biết có bao nhiêu nhà hàng phải được kiểm tra, trong khi`S`là giới hạn thời gian ăn trưa phổ biến được sử dụng để tính mức phạt của mọi nhà hàng. 
3. Khởi tạo mức tiết kiệm tốt nhất thành giá trị nhỏ hơn bất kỳ ứng cử viên nào có thể. Vì câu trả lời có thể là phủ định nên việc khởi tạo nó bằng 0 sẽ không chính xác. Sử dụng số nguyên rất nhỏ hoặc khởi tạo từ nhà hàng đầu tiên sẽ tránh âm thầm loại bỏ các câu trả lời phủ định. 
4. Đọc từng nhà hàng`f_i`Và`t_i`. Nếu như`t_i <= S`, tính toán ứng viên như`f_i`. Nếu không hãy tính nó như`f_i - (t_i - S)`. Số trừ chính xác là số phút đi trễ mà Nina phải trả. 
5. So sánh ứng viên có mức tiết kiệm tốt nhất được tìm thấy cho đến nay và giữ lại giá trị lớn hơn. Vì mọi nhà hàng đều độc lập nên không có lý do gì để giữ lại bất kỳ thông tin nào về nhà hàng sau khi ứng viên của nó đã được so sánh với mức tối đa hiện tại. 
6. Suy cho cùng`N`nhà hàng đã được xử lý, in tối đa bằng cách sử dụng theo yêu cầu`Case #x:`định dạng. Lặp lại quá trình tương tự cho mỗi ngày. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của nhà hàng, phần được duy trì`best`giá trị là mức tiết kiệm cuối cùng tối đa trong số chính xác những nhà hàng đó. Nhà hàng tiếp theo được chuyển đổi thành khoản tiết kiệm cuối cùng chính xác và được so sánh với mức tối đa đó, vì vậy sau khi so sánh, bất biến vẫn đúng đối với tiền tố lớn hơn. Khi tất cả các nhà hàng đã được xử lý,`best`do đó là mức tiết kiệm cuối cùng tối đa đối với mọi lựa chọn hợp pháp. Vì Nina phải chọn chính xác một nhà hàng nên mức tối đa đó chính xác là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    d = int(input())

    answers = []

    for case in range(1, d + 1):
        n, s = map(int, input().split())

        best = -10**30

        for _ in range(n):
            f, t = map(int, input().split())

            late = max(0, t - s)
            value = f - late

            if value > best:
                best = value

        answers.append(f"Case #{case}: {best}")

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài xử lý mỗi ngày độc lập và cũng cung cấp số trường hợp theo yêu cầu của định dạng đầu ra. Vòng lặp nhà hàng xử lý chính xác`N`cặp cho ngày hôm đó. 

biểu thức`max(0, t - s)`xử lý cả hai phía của ranh giới trong một thao tác. Khi`t < s`, hình phạt bằng không. Khi`t == s`, nó cũng bằng 0, đó là điều kiện biên quan trọng. Chỉ khi`t > s`hình phạt có trở thành tích cực không. 

Giá trị ban đầu`-10**30`là cố tình tiêu cực. Từ`f_i`Và`t_i`nhiều nhất là`10^9`, ứng cử viên tệ nhất có thể là`1 - (10^9 - 1) = -999999999`, Vì thế`-10**30`an toàn dưới mọi câu trả lời hợp lệ. Python cũng có các số nguyên có độ chính xác tùy ý, do đó không có vấn đề tràn. 

Mã không lưu trữ các nhà hàng. Mỗi cái được sử dụng ngay lập tức để tính toán một ứng cử viên và sau đó loại bỏ. Điều này mang lại không gian phụ trợ liên tục. 

Đầu vào chứa các dòng trống trong mẫu giữa các trường hợp kiểm thử, nhưng`map(int, input().split())`xử lý chính xác các dòng không trống thông thường. Đầu vào của Codeforce cho vấn đề này cung cấp các dòng có cấu trúc dự kiến, do đó không cần trình phân tích cú pháp dòng trống đặc biệt. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, ngày đầu tiên có`S = 5`và hai nhà hàng. Những thay đổi trạng thái có liên quan là: 

| Nhà hàng |`f`|`t`|`late = max(0, t-S)`|`value`|`best`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 3 | 3 | 
| 2 | 4 | 5 | 0 | 4 | 4 | 

Cả hai nhà hàng đều kết thúc trong vòng năm phút có sẵn. Nhà hàng thứ hai có giá trị danh nghĩa lớn hơn nên đáp án là`4`. 

Ngày thứ hai có một nhà hàng`S = 5`: 

| Nhà hàng |`f`|`t`|`late = max(0, t-S)`|`value`|`best`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 7 | 2 | -1 | -1 | 

Nhà hàng phục vụ lâu hơn thời gian cho phép hai phút nên Nina thua cuộc.`2`từ giá trị của nó`1`. Kết quả tiết kiệm được là`-1`và bởi vì cô ấy phải chọn một nhà hàng,`-1`là câu trả lời. 

Do đó, đầu ra mẫu hoàn chỉnh là:```
Case #1: 4
Case #2: -1
```Dấu vết này chứng tỏ tại sao`best`không thể bắt đầu từ số 0. Vào ngày thứ hai, mọi lựa chọn có sẵn đều có giá trị âm và câu trả lời đúng phải giữ nguyên kết quả âm đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(DN) | Mỗi nhà hàng đều được đọc và đánh giá chính xác một lần. | 
| Không gian | O(1) | Chỉ có nhà hàng hiện tại, mức tối đa hiện tại và một vài biến vô hướng được lưu trữ. | 

Với nhiều nhất`10`ngày và`10^4`nhà hàng mỗi ngày, thuật toán thực hiện tối đa`10^5`đánh giá nhà hàng. Điều này là thoải mái trong giới hạn thời gian và bộ nhớ phụ không đổi thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    d = int(input())
    answers = []

    for case in range(1, d + 1):
        n, s = map(int, input().split())
        best = -10**30

        for _ in range(n):
            f, t = map(int, input().split())
            value = f - max(0, t - s)
            best = max(best, value)

        answers.append(f"Case #{case}: {best}")

    sys.stdout.write("\n".join(answers))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("""\
2
2 5
3 3
4 5

1 5
1 7
""") == """\
Case #1: 4
Case #2: -1
""", "sample"

# Minimum-size input
assert run("""\
1
1 1
1 1
""") == """\
Case #1: 1
""", "minimum-size case"

# Boundary and negative-answer case
assert run("""\
1
3 5
10 4
7 5
1 6
""") == """\
Case #1: 7
""", "boundary at exactly S"

# All values equal, with different lunch times
assert run("""\
1
4 10
10 10
10 8
10 11
10 15
""") == """\
Case #1: 10
""", "all equal nominal values"

# Large values and a late restaurant with a better final score
assert run("""\
1
3 1000000000
1000000000 1000000000
999999999 1
1000000000 1000000001
""") == """\
Case #1: 1000000000
""", "large boundary values"

# Multiple days and all answers negative
assert run("""\
2
2 3
1 5
2 6
3 4
1 10
""") == """\
Case #1: -1
Case #2: -6
""", "negative answers across multiple cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1 1`|`Case #1: 1`| Kích thước đầu vào tối thiểu và ranh giới thời gian chính xác | 
|`1 / 3 5 / 10 4 / 7 5 / 1 6`|`Case #1: 7`| Nhà hàng trước, chính xác tại và sau`S`| 
|`1 / 4 10 / 10 10 / 10 8 / 10 11 / 10 15`|`Case #1: 10`| Giá trị danh nghĩa bằng nhau và không bị phạt | 
|`1 / 3 1000000000 / ...`|`Case #1: 1000000000`| Các giá trị gần giới hạn tối đa và lớn`S`| 
| Hai ngày đi ăn muộn |`Case #1: -1`,`Case #2: -6`| Câu trả lời phủ định và trường hợp kiểm tra độc lập | 

## Vỏ cạnh 

Ranh giới thời gian chính xác được xử lý bởi điều kiện bên trong`max(0, t - S)`. Vì```
1
1 5
10 5
```hình phạt là`max(0, 5 - 5) = 0`, vậy ứng cử viên là`10`và đầu ra là`Case #1: 10`. Đối xử bình đẳng muộn sẽ làm giảm câu trả lời một cách không chính xác. 

Một câu trả lời phủ định được xử lý vì câu trả lời ban đầu`best`được cố tình hạ thấp mọi ứng cử viên có thể. Vì```
1
1 5
1 7
```hình phạt là`7 - 5 = 2`, cho`1 - 2 = -1`. Cập nhật so sánh`best`từ trọng điểm ban đầu đến`-1`, sản xuất`Case #1: -1`. Đang khởi tạo`best`về 0 sẽ tạo ra số 0 không chính xác, biểu thị một tùy chọn mà Nina không có. 

Một nhà hàng phục vụ thoải mái trong giới hạn bữa trưa sẽ không bị phạt. Vì```
1
2 10
8 3
10 10
```nhà hàng đầu tiên sản xuất`8`và thứ hai tạo ra`10`, vậy câu trả lời là`Case #1: 10`. Thuật toán không bao giờ trừ đi thời gian ăn trưa chưa sử dụng vì chỉ có thời gian vượt quá`S`được tính phí. 

Cuối cùng là nhà hàng lớn nhất`f_i`không nhất thiết phải thắng. Vì```
1
3 5
10 5
100 200
20 6
```các giá trị ứng cử viên là`10`,`-95`, Và`19`. Nhà hàng thứ ba thắng mặc dù giá trị danh nghĩa của nó nhỏ hơn nhiều so với`100`. Thuật toán so sánh số tiền tiết kiệm được cuối cùng thay vì sắp xếp hoặc lựa chọn nhà hàng theo`f_i`một mình, đó chính xác là những gì vấn đề yêu cầu.
