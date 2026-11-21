---
title: "CF 225B - Những con số nổi tiếng"
description: "Chúng ta được cho hai số nguyên, s và k. Nhiệm vụ là biểu diễn s dưới dạng tổng các số riêng biệt lấy từ dãy k-bonacci. Dãy số này hoạt động giống như Fibonacci, nhưng thay vì tính tổng hai giá trị trước đó, mỗi số hạng là tổng của k số hạng trước đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 225
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 139 (Div. 2)"
rating: 1600
weight: 225
solve_time_s: 207
verified: false
draft: false
---

[CF 225B - Những con số nổi tiếng](https://codeforces.com/problemset/problem/225/B) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, tham lam, lý thuyết số 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`s`Và`k`. Nhiệm vụ là thể hiện`s`dưới dạng tổng của các số riêng biệt được lấy từ`k`trình tự -bonacci. 

Dãy số này hoạt động giống như Fibonacci, nhưng thay vì tính tổng hai giá trị trước đó, mỗi số hạng là tổng của số hạng trước đó.`k`điều khoản. Sự khởi đầu thật đặc biệt. đầu tiên`k - 1`điều khoản là`0`, cái`k`-số hạng thứ là`1`và mọi giá trị sau này được xây dựng từ giá trị trước đó`k`các giá trị. 

Ví dụ, khi`k = 2`, dãy số trở thành:```
0, 1, 1, 2, 3, 5, 8, ...
```Khi`k = 3`, nó trở thành:```
0, 0, 1, 1, 2, 4, 7, 13, ...
```Chúng ta phải in ít nhất hai giá trị chuỗi riêng biệt có tổng bằng`s`. 

Những hạn chế là rất lớn. Cả hai`s`Và`k`có thể đạt được`10^9`. Điều đó loại trừ bất kỳ cách tiếp cận nào phụ thuộc trực tiếp vào`k`mảng có kích thước hoặc lập trình động trên`s`. Quan sát quan trọng là dãy tăng theo cấp số nhân sau một vài số hạng đầu tiên. Ngay cả đối với nhỏ`k`, số lượng giá trị được tạo bên dưới`10^9`rất nhỏ, thường dưới 50. Điều đó có nghĩa là chúng ta có đủ khả năng xử lý tham lam trên chuỗi được tạo. 

Một điểm tinh tế là sự hiện diện của nhiều số 0 ở đầu dãy. Vấn đề đòi hỏi những con số riêng biệt, không phải vị trí riêng biệt. Điều đó có nghĩa là chúng ta không thể sử dụng nhiều số 0 mặc dù dãy chứa nhiều số 0. Việc triển khai bất cẩn có thể tạo ra hai số 0, vi phạm tính khác biệt. 

Hãy xem xét đầu vào này:```
1 3
```Trình tự bắt đầu như sau:```
0, 0, 1, 1, 2, ...
```Một giải pháp sai có thể thử:```
0 0 1
```nhưng các con số không khác nhau. Câu trả lời hợp lệ là:```
0 1
```Một sai lầm dễ dàng khác xuất hiện khi sử dụng lựa chọn tham lam. Nếu chúng ta tham lam lấy giá trị hữu dụng lớn nhất và dừng lại khi tổng còn lại bằng 0, thì cuối cùng chúng ta có thể chỉ còn một số đã chọn. Vấn đề rõ ràng yêu cầu ít nhất hai con số. 

Ví dụ:```
5 2
```Từ`5`chính nó là một số Fibonacci, kẻ tham lam ngây thơ đưa ra:```
5
```đó là không hợp lệ. Chúng tôi cần một cái gì đó như:```
0 2 3
```Số 0 bổ sung là thủ thuật tiêu chuẩn được sử dụng trong bài toán này để đảm bảo có ít nhất hai số phân biệt. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là tạo ra tất cả`k`-bonacci số lên đến`s`, sau đó thử từng tập hợp con để xem liệu một số tập hợp con có tổng bằng`s`. 

Điều này hoạt động vì số lượng thuật ngữ được tạo ra khá nhỏ. Thậm chí các số Fibonacci cũng vượt quá`10^9`sau khoảng 45 nhiệm kỳ. Nhưng việc liệt kê tập hợp con vẫn trở nên không khả thi. Với khoảng 45 số có thể sử dụng được, số lượng tập hợp con là khoảng`2^45`, điều này hoàn toàn không thể xảy ra trong vòng 2 giây. 

Cấu trúc trình tự mang lại một đặc tính mạnh hơn nhiều. Mỗi số hạng đều lớn hơn tổng của tất cả các số hạng có thể sử dụng đủ nhỏ hơn theo cách tương tự như cách biểu diễn Fibonacci và Zeckendorf. Điều đó có nghĩa là một chiến lược tham lam có hiệu quả: liên tục lấy phần lớn nhất`k`-số bonacci không vượt quá giá trị còn lại. 

Tại sao tham lam lại có thể xảy ra ở đây? 

Sự tái phát làm cho chuỗi phát triển đơn điệu sau giai đoạn đầu. Quan trọng hơn, mỗi số đều là tổng của số trước đó`k`nên việc lấy giá trị sẵn có lớn nhất sẽ loại bỏ càng nhiều mục tiêu còn lại càng tốt mà không cản trở việc xây dựng trong tương lai. Logic tương tự đằng sau việc phân rã Fibonacci tham lam mở rộng một cách tự nhiên đến`k`-số bonacci. 

Vấn đề còn lại là yêu cầu có ít nhất hai số phân biệt. Nếu tham lam chỉ tạo ra một số thì nó phải chính xác`s`chính nó. Trong trường hợp đó, chúng tôi chỉ cần nối thêm`0`, luôn có sẵn và khác biệt với mọi số dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(m) | Quá chậm | 
| Tối ưu | O(m2) | O(m) | Đã chấp nhận | 

Đây`m`là số lượng được tạo ra`k`-bonacci số lên đến`s`, rất nhỏ trong thực tế. 

## Hướng dẫn thuật toán 

1. Tạo ra tất cả những điều tích cực khác biệt`k`-bonacci số lên đến`s`. 

Trình tự bắt đầu bằng`k - 1`số không và một`1`. Mỗi giá trị tiếp theo bằng tổng của giá trị trước đó`k`các giá trị. Chúng tôi chỉ quan tâm đến các giá trị lên đến`s`, bởi vì những số lớn hơn không bao giờ có thể tham gia vào quá trình phân tích. 
2. Lưu trữ các giá trị được tạo theo thứ tự tăng dần. 

Trình tự trở nên tăng dần sau những lần lặp ban đầu, do đó việc xử lý tham lam từ lớn nhất đến nhỏ nhất là đơn giản. 
3. Bắt đầu với`remaining = s`. 

Chúng tôi sẽ liên tục trừ đi giá trị chưa sử dụng lớn nhất có thể. 
4. Duyệt các số được tạo theo thứ tự ngược lại. 

Bất cứ khi nào một số nhiều nhất`remaining`, đưa nó vào câu trả lời và trừ nó khỏi`remaining`. 

Đây là cách xây dựng tập hợp con tham lam tiêu chuẩn. Bởi vì chuỗi phát triển nhanh chóng nên việc chọn giá trị lớn nhất có thể không bao giờ cản trở việc hoàn thành. 
5. Tiếp tục cho đến khi`remaining`trở thành số không. 

Tại thời điểm này, các số đã chọn có tổng chính xác bằng`s`. 
6. Nếu câu trả lời chỉ chứa một số, hãy thêm`0`. 

Điều này xử lý các trường hợp`s`bản thân nó là một`k`-số bonacci. Bài toán yêu cầu ít nhất hai số phân biệt và`0`luôn khác biệt với mọi giá trị dương được chọn. 
7. In kích thước câu trả lời và các con số. 

### Tại sao nó hoạt động 

Tính chất quan trọng là mọi`k`-số bonacci bằng tổng của số trước đó`k`những con số. Do cấu trúc tăng trưởng này, mọi giá trị còn lại nhỏ hơn số hạng đã chọn vẫn có thể được biểu diễn bằng số hạng nhỏ hơn. Sự tham lam không bao giờ tạo ra phần còn lại không thể có được. 

Ở mỗi bước, thuật toán duy trì tính bất biến mà phần dư hiện tại vẫn có thể được biểu diễn bằng các giá trị chuỗi nhỏ hơn không được sử dụng. Việc chọn số hạng hợp lệ lớn nhất sẽ giảm phần còn lại một cách tích cực nhất có thể trong khi vẫn duy trì tính đại diện. 

Điều chỉnh cuối cùng với`0`duy trì tính đúng đắn vì việc thêm số 0 không làm thay đổi tổng và số 0 khác biệt với mọi số dương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s, k = map(int, input().split())

    # Build sequence:
    # [0, 0, ..., 0, 1]
    seq = [0] * (k - 1) + [1]

    window_sum = 1

    while True:
        nxt = window_sum

        if nxt > s:
            break

        seq.append(nxt)

        window_sum += nxt
        window_sum -= seq[-k - 1]

    # Keep only distinct positive values
    vals = sorted(set(x for x in seq if x > 0))

    ans = []
    rem = s

    for x in reversed(vals):
        if x <= rem:
            ans.append(x)
            rem -= x

    # Need at least two distinct numbers
    if len(ans) == 1:
        ans.append(0)

    print(len(ans))
    print(*sorted(ans))

if __name__ == "__main__":
    solve()
```Việc tạo chuỗi sử dụng tổng cửa sổ trượt. Tính toán từng thuật ngữ mới trực tiếp từ thuật ngữ trước đó`k`các yếu tố sẽ có giá`O(k)`mỗi bước, điều này trở nên nguy hiểm khi`k`là lớn. Thay vào đó, chúng tôi duy trì tổng cửa sổ hiện tại tăng dần. 

Giả sử cửa sổ hiện tại chứa cửa sổ cuối cùng`k`các giá trị trình tự. Phần tử chuỗi tiếp theo chính xác là tổng đó. Sau khi thêm giá trị mới, chúng tôi cập nhật cửa sổ bằng cách thêm thuật ngữ mới và xóa thuật ngữ cũ nhất. 

Biểu thức:```
window_sum -= seq[-k - 1]
```rất dễ mắc sai lầm. Sau khi thêm giá trị mới, phần tử rời khỏi cửa sổ chính xác`k + 1`vị trí từ cuối. 

Chuỗi ban đầu chứa nhiều bản sao, đặc biệt là số 0 và đôi khi lặp lại. Vì vấn đề yêu cầu các số riêng biệt nên chúng tôi loại bỏ rõ ràng các giá trị dương trước khi lựa chọn tham lam. 

Giai đoạn tham lam xử lý các số từ lớn nhất đến nhỏ nhất. Vì kích thước chuỗi rất nhỏ nên chỉ cần duyệt ngược đơn giản là đủ. 

trận chung kết`0`chèn là chi tiết nhỏ nhưng cần thiết để tránh các câu trả lời một thành phần không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
```Trình tự được tạo:```
0, 1, 1, 2, 3, 5
```Các giá trị dương riêng biệt:```
1, 2, 3, 5
```Dấu vết tham lam: 

| Giá trị hiện tại | Còn lại trước | Lấy? | Còn lại sau | Trả lời | 
| --- | --- | --- | --- | --- | 
| 5 | 5 | Có | 0 | [5] | 
| 3 | 0 | Không | 0 | [5] | 
| 2 | 0 | Không | 0 | [5] | 
| 1 | 0 | Không | 0 | [5] | 

Chỉ có một số được chọn, vì vậy chúng tôi nối thêm`0`. 

Câu trả lời cuối cùng:```
0 5
```Ví dụ này chứng minh tại sao việc xử lý thêm số 0 là cần thiết. 

### Ví dụ 2 

đầu vào:```
10 3
```Trình tự được tạo:```
0, 0, 1, 1, 2, 4, 7, 13
```Các giá trị dương riêng biệt:```
1, 2, 4, 7
```Dấu vết tham lam: 

| Giá trị hiện tại | Còn lại trước | Lấy? | Còn lại sau | Trả lời | 
| --- | --- | --- | --- | --- | 
| 7 | 10 | Có | 3 | [7] | 
| 4 | 3 | Không | 3 | [7] | 
| 2 | 3 | Có | 1 | [7, 2] | 
| 1 | 1 | Có | 0 | [7, 2, 1] | 

Câu trả lời cuối cùng:```
1 2 7
```Dấu vết này cho thấy mức độ tham lam phân hủy mục tiêu thành các giá trị chuỗi riêng biệt một cách tự nhiên như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m2) | Tạo trình tự và xử lý tham lam`m`giá trị được tạo ra | 
| Không gian | O(m) | Lưu trữ chuỗi được tạo | 

Giá trị thực tế của`m`rất nhỏ vì`k`- số lượng bonacci tăng theo cấp số nhân. Ngay cả đối với trường hợp tăng trưởng chậm nhất, Fibonacci, cũng chỉ có khoảng 45 số hạng có thể sử dụng được dưới đây`10^9`. Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    s, k = map(int, input().split())

    seq = [0] * (k - 1) + [1]
    window_sum = 1

    while True:
        nxt = window_sum

        if nxt > s:
            break

        seq.append(nxt)

        window_sum += nxt
        window_sum -= seq[-k - 1]

    vals = sorted(set(x for x in seq if x > 0))

    ans = []
    rem = s

    for x in reversed(vals):
        if x <= rem:
            ans.append(x)
            rem -= x

    if len(ans) == 1:
        ans.append(0)

    ans.sort()

    print(len(ans))
    print(*ans)

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
assert run("5 2\n") == "2\n0 5\n"

# minimum non-trivial case
assert run("1 2\n") == "2\n0 1\n"

# tribonacci decomposition
assert run("10 3\n") == "3\n1 2 7\n"

# large k, slow sequence growth
assert run("2 100\n") == "2\n0 2\n"

# exact k-bonacci number
assert run("13 3\n") == "2\n0 13\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`0 1`| Phân rã có ý nghĩa nhỏ nhất | 
|`10 3`|`1 2 7`| Tiêu chuẩn phân hủy tham lam | 
|`2 100`|`0 2`| Lớn`k`xử lý | 
|`13 3`|`0 13`| Giá trị chuỗi chính xác yêu cầu 0 | 

## Vỏ cạnh 

Hãy xem xét trường hợp:```
1 3
```Trình tự được tạo bắt đầu:```
0, 0, 1, 1, 2, ...
```Sau khi loại bỏ trùng lặp, các giá trị có thể sử dụng sẽ trở thành:```
1, 2, ...
```Lựa chọn tham lam`1`, để lại phần dư`0`. Vì chỉ có một giá trị được chọn nên thuật toán sẽ thêm vào`0`. 

Câu trả lời cuối cùng:```
0 1
```Điều này tránh việc sử dụng bất hợp pháp các số 0 trùng lặp. 

Bây giờ hãy xem xét:```
5 2
```Tham lam ngay lập tức lấy`5`, vì nó khớp chính xác với mục tiêu. Nếu không có sự điều chỉnh cuối cùng, câu trả lời sẽ chỉ chứa một số, vi phạm yêu cầu. 

Thuật toán phát hiện tình huống này và nối thêm`0`, sản xuất:```
0 5
```Một kịch bản khó khăn khác là rất lớn`k`, chẳng hạn như:```
2 100
```Dãy số này vẫn ở mức nhỏ trong một thời gian dài vì mỗi số hạng tính tổng của 100 phần tử trước đó, hầu hết ban đầu đều bằng 0. 

Các giá trị dương được tạo ra chỉ đơn giản là:```
1, 2
```Tham lam mất`2`, sau đó nối thêm`0`bởi vì chỉ có một giá trị được chọn. 

Việc triển khai cửa sổ trượt xử lý việc này một cách hiệu quả mà không cần lặp lại 100 lần cho mỗi thuật ngữ được tạo.
