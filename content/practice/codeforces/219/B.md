---
title: "CF 219B - Ưu đãi đặc biệt! Giá siêu 999 Bourles!"
description: "Polycarpus đã biết mức giá thông thường tốt nhất cho sản phẩm của mình, p. Anh ta sẵn sàng giảm nó tối đa là d bourles nếu điều đó mang lại cho mức giá cuối cùng một kết thúc hấp dẫn hơn, đặc biệt là càng nhiều số 9 càng tốt. Số chín ở cuối có nghĩa là chữ số 9 ở cuối số."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 219
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 135 (Div. 2)"
rating: 1400
weight: 219
solve_time_s: 89
verified: true
draft: false
---

[CF 219B - Ưu đãi đặc biệt! Siêu Giá 999 Bourles!](https://codeforces.com/problemset/problem/219/B) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Polycarpus đã biết mức giá thông thường tốt nhất cho sản phẩm của mình,`p`. Anh ấy sẵn sàng giảm nó nhiều nhất`d`bourles nếu điều đó mang lại cho mức giá cuối cùng một kết thúc hấp dẫn hơn, cụ thể là càng nhiều số 9 càng tốt. 

Số chín ở cuối có nghĩa là một chữ số`9`ở cuối số. Ví dụ,`12999`có ba số 9 ở cuối, trong khi`9099`có hai vì chỉ có hậu tố mới quan trọng. 

Trong số tất cả các giá`x`như vậy: 

-`x <= p`-`p - x <= d`chúng tôi muốn cái có số chín cuối tối đa. Nếu nhiều mức giá có cùng số số 9 ở cuối thì chúng ta chọn mức giá lớn nhất như vậy. 

Các giới hạn là phần thú vị ở đây.`p`có thể lớn như`10^18`, vì vậy chúng ta không thể lặp lại mọi mức giá có thể có trong khoảng`[p-d, p]`. Trong trường hợp xấu nhất, khoảng đó có thể chứa gần như`10^18`những con số, vượt xa những gì phù hợp với giới hạn thời gian một giây. 

Cấu trúc của vấn đề gợi ý rằng chỉ những ứng viên rất cụ thể mới quan trọng. Một số kết thúc bằng số 9 ở cuối trông như sau:```
...9
```Một số kết thúc bằng hai số 9 ở cuối trông như sau:```
...99
```Một số kết thúc bằng ba số 9 ở cuối trông như sau:```
...999
```và vân vân. 

Đối với mỗi số chín cuối có thể có, có chính xác một ứng cử viên lớn nhất không vượt quá`p`. Sự quan sát đó chính là điều làm cho vấn đề trở nên nhỏ bé. 

Có một số trường hợp khó xử lý sai. 

Giả định:```
p = 1000
d = 0
```Chúng tôi không được phép giảm giá chút nào. Câu trả lời phải ở lại`1000`, mặc dù`999`có nhiều số chín hơn. Việc thực hiện bất cẩn luôn làm tròn xuống đến giá trị gần nhất`999...`giá trị sẽ xuất ra không chính xác`999`. 

Một trường hợp phức tạp khác là:```
p = 10999
d = 1
```Câu trả lời là`10999`bởi vì nó đã có ba số 9 ở cuối rồi. Một số cách triển khai sửa đổi số một cách không cần thiết ngay cả khi câu trả lời tối ưu đã bằng`p`. 

Một trường hợp tế nhị hơn là:```
p = 10000
d = 100
```Câu trả lời tốt nhất là`9999`. Nó có bốn số 9 ở cuối và chỉ có giá`1`Bourle ít hơn`p`. Nếu trước tiên chúng ta cố gắng tối đa hóa giá trị một cách tham lam thì chúng ta có thể dừng lại ở mức không chính xác.`9990`hoặc`999`. 

Chi tiết quan trọng cuối cùng là tie-break. Coi như:```
p = 1999
d = 1000
```Cả hai`999`Và`1999`là hợp lệ. Mỗi cái có ba số chín. Chúng ta phải chọn cái lớn hơn, đó là`1999`. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Lặp lại qua mọi mức giá ứng cử viên`x`từ`p-d`lên tới`p`, đếm xem mỗi số có bao nhiêu số chín và giữ lại số chín cuối theo quy định. 

Đếm số chín ở cuối thật dễ dàng. Nhiều lần kiểm tra xem chữ số cuối cùng có phải là`9`, chia cho`10`và đếm xem điều này thành công bao nhiêu lần. 

Lực lượng vũ phu là chính xác bởi vì nó kiểm tra mọi mức giá hợp lệ. Vấn đề là kích thước của không gian tìm kiếm. Nếu như`p = 10^18`Và`d`cũng rất lớn, chúng ta có thể cần phải kiểm tra gần như`10^18`những con số. Ngay cả việc thực hiện một thao tác cho mỗi số cũng là không thể. 

Quan sát quan trọng là các số có nhiều số 9 ở cuối có cấu trúc cực kỳ chặt chẽ. 

Đối với một số số chín cố định`k`, số lớn nhất không vượt quá`p`thu được bằng cách thay thế cuối cùng`k`chữ số với`9`. 

Ví dụ: nếu:```
p = 543210
```sau đó: 

- số lớn nhất có 19 ở cuối là`543209`- số lớn nhất có hai số 9 ở cuối là`543199`- số lớn nhất có 3 số 9 ở cuối là`542999`Không cần thiết phải kiểm tra bất kỳ ứng cử viên nhỏ hơn nào có cùng số chín cuối vì bài toán yêu cầu mức giá tối đa giữa các mối quan hệ. 

Điều này làm giảm đáng kể không gian tìm kiếm. Chúng ta chỉ cần thử từng độ dài hậu tố có thể. Từ`p <= 10^18`, có nhiều nhất 19 chữ số nên chỉ có khoảng 19 ứng viên tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d log p) | O(1) | Quá chậm | 
| Tối ưu | O(logp) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với câu trả lời đúng nhất hiện tại bằng`p`. 

Việc này xử lý trường hợp việc giảm giá không có lợi hoặc không được phép. 
2. Với mọi lũy thừa của mười`10^k`, xây dựng số lớn nhất không vượt quá`p`cuối cùng của ai`k`tất cả đều là chữ số`9`. 

Chúng ta có thể tính toán nó như sau:```
candidate = (p // 10^k) * 10^k - 1
```Bộ phận loại bỏ phần cuối cùng`k`các chữ số, phép nhân sẽ khôi phục chúng thành số 0 và trừ đi một sẽ biến các số 0 đó thành số chín. 
3. Kiểm tra xem ứng viên có hợp lệ không. 

Ứng viên phải đáp ứng:```
p - candidate <= d
```Nếu không thì mức giảm giá yêu cầu là quá lớn. 
4. So sánh ứng viên với câu trả lời đúng nhất hiện tại. 

Ưu tiên ứng viên nếu nó có nhiều số 9 ở cuối. Nếu cả hai đều có cùng số chín ở cuối thì chọn số lớn hơn. 

Trong thực tế, việc lặp`k`từ nhỏ đến lớn đã đảm bảo rằng các ứng cử viên hợp lệ sau này có ít nhất số chín cuối cùng. 
5. Tiếp tục trong khi`10^k <= p * 10`. 

Từ`p`có tối đa 19 chữ số, vòng lặp này chỉ chạy một vài lần. 
6. In câu trả lời đúng nhất. 

### Tại sao nó hoạt động 

Đối với bất kỳ số chín cố định nào`k`, mọi số hợp lệ có thuộc tính đó có thể được viết là:```
some_prefix * 10^k + (10^k - 1)
```Trong số tất cả những con số như vậy không vượt quá`p`, số lớn nhất chính xác là:```
(p // 10^k) * 10^k - 1
```Nếu ứng cử viên lớn nhất này không hợp lệ vì nó yêu cầu mức giảm giá quá lớn thì mọi số nhỏ hơn đều có cùng giá trị`k`số 9 ở cuối thậm chí còn xa hơn`p`, vì vậy không ai trong số họ có thể làm việc. 

Điều đó có nghĩa là chỉ kiểm tra ứng viên tối đa này cho mỗi`k`là đủ. 

Vì chúng tôi kiểm tra mọi số chín cuối có thể có và với mỗi số chúng tôi kiểm tra số tốt nhất có thể, thuật toán không thể bỏ lỡ câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def trailing_nines(x):
    cnt = 0
    while x % 10 == 9:
        cnt += 1
        x //= 10
    return cnt

def solve():
    p, d = map(int, input().split())

    ans = p
    best = trailing_nines(p)

    power = 10

    while power <= 10**19:
        candidate = (p // power) * power - 1

        if candidate >= 0 and p - candidate <= d:
            cur = trailing_nines(candidate)

            if cur > best or (cur == best and candidate > ans):
                best = cur
                ans = candidate

        power *= 10

    print(ans)

solve()
```Hàm trợ giúp đếm số 9 ở cuối bằng cách kiểm tra liên tục chữ số cuối cùng. Vì một số có nhiều nhất 19 chữ số nên phép toán này thực chất là một hằng số thời gian. 

Vòng lặp chính lặp lại lũy thừa mười. Đối với mỗi lũy thừa, nó xây dựng ứng cử viên lớn nhất có thể có kết thúc bằng số chín cần thiết. 

Biểu thức:```
(p // power) * power - 1
```là phần quan trọng của giải pháp. 

Giả định:```
p = 543210
power = 1000
```Sau đó:```
p // power = 543
543 * 1000 = 543000
543000 - 1 = 542999
```đó chính xác là số lớn nhất không vượt quá`p`với ba số chín ở cuối. 

Điều kiện:```
p - candidate <= d
```lọc ra những ứng viên yêu cầu giảm giá quá nhiều. 

Quy tắc ràng buộc được thực hiện một cách rõ ràng. Nếu hai ứng cử viên có cùng số chín cuối thì chúng ta giữ số lớn hơn. 

Giới hạn trên của vòng lặp`10**19`bao phủ một cách an toàn mọi chiều dài chữ số có thể cho`p <= 10^18`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1029 102
```| quyền lực | ứng cử viên | giảm giá | số chín sau | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 10 | 1019 | 10 | 1 | vâng | 
| 100 | 999 | 30 | 3 | vâng | 
| 1000 | -1 | không hợp lệ | - | không | 

Ứng viên hợp lệ nhất là`999`, có ba số chín ở cuối. 

Đầu ra:```
999
```Dấu vết này cho thấy ý tưởng chính của thuật toán. Thay vì kiểm tra từng số giữa`927`Và`1029`, chúng tôi chỉ kiểm tra ứng cử viên tối đa cho mỗi độ dài hậu tố. 

### Ví dụ 2 

đầu vào:```
10000 100
```| quyền lực | ứng cử viên | giảm giá | số chín sau | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 10 | 9999 | 1 | 4 | vâng | 
| 100 | 9999 | 1 | 4 | vâng | 
| 1000 | 9999 | 1 | 4 | vâng | 
| 10000 | 9999 | 1 | 4 | vâng | 

Câu trả lời là ngay lập tức`9999`. 

Đầu ra:```
9999
```Ví dụ này chứng minh rằng đôi khi một mức giảm nhỏ tạo ra nhiều số 9 cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(logp) | Chúng tôi kiểm tra một ứng cử viên trên mỗi chữ số | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Từ`p`có tối đa 19 chữ số, vòng lặp chỉ thực hiện khoảng 19 lần lặp. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def trailing_nines(x):
        cnt = 0
        while x % 10 == 9:
            cnt += 1
            x //= 10
        return cnt

    p, d = map(int, input().split())

    ans = p
    best = trailing_nines(p)

    power = 10

    while power <= 10**19:
        candidate = (p // power) * power - 1

        if candidate >= 0 and p - candidate <= d:
            cur = trailing_nines(candidate)

            if cur > best or (cur == best and candidate > ans):
                best = cur
                ans = candidate

        power *= 10

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample
assert run("1029 102\n") == "999", "sample 1"

# minimum case
assert run("1 0\n") == "1", "minimum values"

# already optimal
assert run("1999 0\n") == "1999", "already has maximum trailing nines"

# small decrease unlocks many nines
assert run("10000 1\n") == "9999", "single decrement creates four nines"

# tie-breaking
assert run("1999 1000\n") == "1999", "choose larger value among equal suffix counts"

# very large values
assert run("1000000000000000000 1\n") == "999999999999999999", "64-bit boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| Đầu vào hợp lệ tối thiểu | 
|`1999 0`|`1999`| Đã tối ưu mà không cần giảm | 
|`10000 1`|`9999`| Giảm giá nhỏ có thể tối đa hóa số chín hậu tố | 
|`1999 1000`|`1999`| Thắt dây đúng cách | 
|`1000000000000000000 1`|`999999999999999999`| Xử lý các giá trị 64-bit một cách an toàn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1000 0
```Thuật toán bắt đầu với`ans = 1000`. Ứng viên`999`được tạo ra khi`power = 10`, Nhưng:```
1000 - 999 = 1
```vượt quá`d = 0`. 

Vì vậy, ứng viên bị từ chối và câu trả lời vẫn còn`1000`. 

Bây giờ hãy xem xét:```
10999 1
```Câu trả lời ban đầu đã có ba số 9 ở cuối. 

Khi`power = 10`, ứng viên trở thành:```
10999
```bởi vì:```
(10999 // 10) * 10 - 1 = 10999
```Thuật toán giữ chính xác giá trị ban đầu. 

Vì:```
10000 100
```ứng cử viên đầu tiên là:```
9999
```có giảm giá`1`. 

Điều này ngay lập tức cho ra bốn số 9 ở cuối, là mức tối ưu. Quyền lực lớn hơn vẫn sản xuất`9999`, vì vậy câu trả lời không bao giờ thay đổi. 

Cuối cùng, hãy xem xét ví dụ về tie-break:```
1999 1000
```Cả hai`1999`Và`999`có ba số chín ở cuối. 

Thuật toán bắt đầu với`ans = 1999`. Sau này nó có thể tạo ra`999`, nhưng vì cả hai đều có cùng số lượng hậu tố và`1999 > 999`, câu trả lời được lưu trữ không thay đổi.
