---
title: "CF 451C - Dự đoán kết quả trận đấu"
description: "Chúng tôi có một giải đấu bóng đá có sự tham gia của ba đội. Mỗi trò chơi tạo ra chính xác một người chiến thắng, vì vậy mỗi trò chơi đã chơi sẽ đóng góp chính xác một chiến thắng cho một trong các đội. Sau khi đã chơi k trận, chúng ta không biết chính xác số trận thắng của ba đội."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 451
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 258 (Div. 2)"
rating: 1700
weight: 451
solve_time_s: 115
verified: true
draft: false
---

[CF 451C - Dự đoán kết quả trận đấu](https://codeforces.com/problemset/problem/451/C) 

**Đánh giá:** 1700 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một giải đấu bóng đá có sự tham gia của ba đội. Mỗi trò chơi tạo ra chính xác một người chiến thắng, vì vậy mỗi trò chơi đã chơi sẽ đóng góp chính xác một chiến thắng cho một trong các đội. 

Sau đó`k`Các trận đã diễn ra rồi, chúng tôi không biết chính xác số trận thắng của ba đội. Chúng tôi chỉ biết hai điểm khác biệt tuyệt đối: 

- Hiệu số chiến thắng của đội 1 và đội 2 là`d1`. 
- Hiệu số chiến thắng của đội 2 và đội 3 là`d2`. 

Giải đấu cuối cùng sẽ có`n`tổng số trò chơi. Chúng tôi muốn biết liệu có thể hoàn thành giải đấu để cuối cùng`n`trận đấu kết thúc, mỗi đội có số trận thắng bằng nhau. 

Vì mỗi trận đấu đóng góp một trận thắng nên số trận thắng cuối cùng của tất cả các đội chính xác là`n`. Nếu cả ba đội phải về đích bằng nhau thì mỗi đội phải kết thúc với`n / 3`thắng. Điều này ngay lập tức ngụ ý rằng`n`phải chia hết cho 3. 

Các ràng buộc là manh mối đầu tiên cho thấy cần có một giải pháp toán học. Số lượng ca kiểm thử đạt`10^5`, Và`n`có thể lớn như`10^12`. Bất kỳ cách tiếp cận nào mô phỏng trò chơi hoặc tìm kiếm về phân phối chiến thắng đều không thể thực hiện được. Thậm chí làm công việc tỷ lệ thuận với`k`sẽ thất bại. Chúng tôi cần kiểm tra liên tục cho từng trường hợp thử nghiệm. 

Phần khó khăn là sự khác biệt là giá trị tuyệt đối. Biết`|a - b| = d1`Và`|b - c| = d2`không xác định duy nhất số lần thắng hiện tại. Có thể có một số cấu hình dấu hiệu. 

Một sai lầm phổ biến là giả sử một thứ tự duy nhất như`a ≥ b ≥ c`. Coi như:```
n = 6
k = 4
d1 = 1
d2 = 0
```Một phân phối hợp lệ là`(2,1,1)`, dẫn đến một giải đấu cuối cùng hợp lệ. Nếu chúng ta chỉ kiểm tra thứ tự`a ≥ b ≥ c`, chúng ta sẽ tìm thấy nó, nhưng trong các trường hợp khác, sự sắp xếp hợp lệ duy nhất có thể sử dụng một thứ tự khác. 

Một trường hợp tế nhị khác xảy ra khi đội mạnh nhất hiện tại đã có quá nhiều chiến thắng. Ví dụ:```
n = 6
k = 3
d1 = 3
d2 = 0
```Sự phân phối duy nhất có thể là`(3,0,0)`. Vì mỗi đội phải kết thúc với`2`thắng, đội 1 đã vượt chỉ tiêu cuối cùng. Câu trả lời là`"no"`. 

Trường hợp cạnh thứ ba là khả năng chia hết. Coi như:```
n = 4
k = 0
d1 = 0
d2 = 0
```Dù tình thế hiện tại hoàn toàn cân bằng nhưng tổng số 4 trận thắng không thể chia đều cho 3 đội. Câu trả lời phải là`"no"`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng lại số trận thắng của ba đội sau trận đầu tiên`k`trận đấu. Hãy để những số đó được tính`(a,b,c)`. 

Họ phải thỏa mãn:```
a + b + c = k
|a - b| = d1
|b - c| = d2
```Người ta có thể liệt kê các giá trị có thể có của`a`,`b`, Và`c`, kiểm tra các ràng buộc và sau đó xác minh xem phần còn lại có`n-k`trò chơi có thể cân bằng tất cả các đội. 

Điều này hoạt động về mặt khái niệm vì mọi trạng thái hiện tại hợp lệ đều có thể được kiểm tra trực tiếp. Vấn đề là quy mô. Từ`k`có thể lớn như`10^12`, thậm chí việc lặp lại các giá trị có thể có của một biến là hoàn toàn không khả thi. 

Quan sát quan trọng là chỉ có bốn cách riêng biệt để gán dấu hiệu cho hai hiệu tuyệt đối. 

Hãy để chiến thắng của đội 2 có giá trị nào đó`x`. 

Khi đó đội 1 phải là một trong hai`x+d1`hoặc`x-d1`. 

Tương tự, đội 3 phải là`x+d2`hoặc`x-d2`. 

Điều đó chỉ tạo ra bốn mẫu ứng cử viên:```
(x+d1, x, x+d2)
(x+d1, x, x-d2)
(x-d1, x, x+d2)
(x-d1, x, x-d2)
```Đối với mỗi mẫu, phương trình```
a + b + c = k
```xác định`x`độc đáo. 

Sau khi có được`(a,b,c)`, chúng ta chỉ cần xác minh xem: 

1. Tất cả số đếm đều là số nguyên không âm. 
2. Chưa có đội nào vượt chỉ tiêu cuối cùng`n/3`. 
3. Các trận đấu còn lại đủ để nâng tầm tất cả các đội`n/3`. 

Điều kiện thứ ba hóa ra tương đương với việc kiểm tra xem tổng số trận thắng bổ sung cần thiết có bằng chính xác số ván còn lại hay không. 

Vì chỉ có bốn cấu hình nên mọi trường hợp thử nghiệm đều có thể được giải quyết trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem`n`chia hết cho 3. Nếu không, điểm cuối cùng bằng nhau là không thể, vì vậy hãy quay lại`"no"`. 
2. Đặt mục tiêu cuối cùng giành chiến thắng cho mỗi đội là:```
target = n / 3
```3. Liệt kê bốn cấu hình dấu hiệu:```
(+d1, +d2)
(+d1, -d2)
(-d1, +d2)
(-d1, -d2)
```4. Đối với cấu hình đã chọn, hãy viết:```
a = x + s1*d1
b = x
c = x + s2*d2
```Ở đâu`s1`Và`s2`hoặc là`+1`hoặc`-1`. 
5. Sử dụng điều kiện`a+b+c=k`tính toán`x`.```
3x + s1*d1 + s2*d2 = k
```Kể từ đây```
x = (k - s1*d1 - s2*d2) / 3
```6. Nếu tử số không chia hết cho 3, cấu hình này không thể biểu thị số lần thắng hợp lệ. Bỏ qua nó. 
7. Xây dựng`(a,b,c)`và xác minh rằng tất cả các giá trị đều không âm. 
8. Hãy để```
mx = max(a,b,c)
```Nếu như`mx > target`, thì một số đội đã có nhiều trận thắng hơn tổng số trận thắng cuối cùng được phép. Bỏ qua cấu hình này. 
9. Tính xem cần bao nhiêu trận thắng để đưa mỗi đội về đích:```
need = (target-a) + (target-b) + (target-c)
```10. Nếu`need == n-k`, thì các trò chơi còn lại chính xác có thể mang lại những chiến thắng đó, vì vậy hãy quay lại`"yes"`. 
11. Nếu không có cấu hình nào trong bốn cấu hình hoạt động, hãy quay lại`"no"`. 

### Tại sao nó hoạt động 

Sự không chắc chắn duy nhất trên bảng xếp hạng hiện tại đến từ chiều hướng của hai sự chênh lệch tuyệt đối. Mỗi bộ ba hợp lệ`(a,b,c)`phải tương ứng với một trong bốn phép gán dấu. 

Đối với một bài tập cố định, phương trình`a+b+c=k`xác định duy nhất thứ hạng. Không có phân phối hợp lệ bị bỏ qua. 

Nếu một đội đã vượt chỉ tiêu cuối cùng`n/3`, sau này không có cách nào gỡ win nên cấu hình đó là không thể. Nếu tất cả các đội đều đạt được mục tiêu nhiều nhất thì số trận thắng chính xác cần thiết để đạt được`(target,target,target)`là```
(target-a)+(target-b)+(target-c).
```Bởi vì mỗi trò chơi trong tương lai đóng góp chính xác một chiến thắng bổ sung nên cấu hình khả thi chính xác khi tổng yêu cầu này bằng với số trò chơi còn lại`n-k`. 

Vì mọi phép gán dấu có thể có đều được kiểm tra và mỗi dấu hiệu đều được kiểm tra chính xác theo các ràng buộc của giải đấu nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def possible(n, k, d1, d2):
    if n % 3 != 0:
        return False

    target = n // 3

    for s1 in (1, -1):
        for s2 in (1, -1):
            rem = k - s1 * d1 - s2 * d2

            if rem % 3 != 0:
                continue

            x = rem // 3

            a = x + s1 * d1
            b = x
            c = x + s2 * d2

            if min(a, b, c) < 0:
                continue

            if max(a, b, c) > target:
                continue

            need = (target - a) + (target - b) + (target - c)

            if need == n - k:
                return True

    return False

def main():
    t = int(input())

    ans = []
    for _ in range(t):
        n, k, d1, d2 = map(int, input().split())
        ans.append("yes" if possible(n, k, d1, d2) else "no")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```Kiểm tra đầu tiên xử lý yêu cầu chia hết không thể tránh khỏi. Nếu như`n`không phải là bội số của 3 nên không thể tồn tại điểm cuối cùng bằng nhau. 

Các vòng lặp lồng nhau liệt kê bốn cách giải thích có thể có về sự khác biệt tuyệt đối. Đối với mỗi cái, ràng buộc tổng xác định giá trị ở giữa`x`. Nếu giá trị kết quả không phải là số nguyên thì cấu hình đó không thể tương ứng với số lần thắng thực tế. 

Việc kiểm tra không tiêu cực sẽ loại bỏ các thứ hạng không thể có. Việc kiểm tra giá trị tối đa là cần thiết vì sau này một đội không thể bị tước đoạt chiến thắng. 

Sự bình đẳng cuối cùng```
need == n - k
```nắm bắt xem các trò chơi còn lại có đủ chính xác để nâng cao tất cả các đội đạt được mục tiêu chung hay không. 

Tất cả số học đều sử dụng số nguyên, do đó không có vấn đề tràn ngay cả khi`n`đạt tới`10^12`, vì số nguyên Python không bị giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6
k = 4
d1 = 1
d2 = 0
```Mục tiêu:```
target = 2
```Đang thử cấu hình`(s1,s2)=(+1,+1)`: 

| Bước | Giá trị | 
| --- | --- | 
| rem | 4 - 1 - 0 = 3 | 
| x | 1 | 
| một | 2 | 
| b | 1 | 
| c | 1 | 
| tối đa(a,b,c) | 2 | 
| cần | 0 + 1 + 1 = 2 | 
| n-k | 2 | 

Từ`need = n-k`, cấu hình này khả thi và câu trả lời là`"yes"`. 

Ví dụ này cho thấy trường hợp một đội hiện đang dẫn đầu nhưng số trận còn lại đủ để san bằng tỷ số cho tất cả các đội. 

### Ví dụ 2 

đầu vào:```
n = 6
k = 3
d1 = 3
d2 = 0
```Mục tiêu:```
target = 2
```Đang cố gắng`(s1,s2)=(+1,+1)`: 

| Bước | Giá trị | 
| --- | --- | 
| rem | 3 - 3 - 0 = 0 | 
| x | 0 | 
| một | 3 | 
| b | 0 | 
| c | 0 | 
| tối đa(a,b,c) | 3 | 
| mục tiêu | 2 | 

Đội lớn nhất đã vượt quá số điểm cuối cùng cho phép nên cấu hình này là không thể. 

Các cấu hình dấu hiệu còn lại tạo ra số đếm âm hoặc không kiểm tra được tính chia hết. 

Câu trả lời là`"no"`. 

Ví dụ này chứng minh tại sao chỉ kiểm tra sự khác biệt là không đủ. Một đội có thể đã tích lũy quá nhiều chiến thắng để có thể cân bằng sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chính xác bốn cấu hình được kiểm tra cho mỗi trường hợp | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với tối đa`10^5`các trường hợp thử nghiệm, giải pháp chỉ thực hiện một lượng số học không đổi cho mỗi trường hợp thử nghiệm. Tổng khối lượng công việc dễ dàng phù hợp trong giới hạn thời gian và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def possible(n, k, d1, d2):
        if n % 3 != 0:
            return False

        target = n // 3

        for s1 in (1, -1):
            for s2 in (1, -1):
                rem = k - s1 * d1 - s2 * d2

                if rem % 3 != 0:
                    continue

                x = rem // 3

                a = x + s1 * d1
                b = x
                c = x + s2 * d2

                if min(a, b, c) < 0:
                    continue

                if max(a, b, c) > target:
                    continue

                need = (target - a) + (target - b) + (target - c)

                if need == n - k:
                    return True

        return False

    t = int(input())
    out = []

    for _ in range(t):
        n, k, d1, d2 = map(int, input().split())
        out.append("yes" if possible(n, k, d1, d2) else "no")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided samples
assert run(
"""5
3 0 0 0
3 3 0 0
6 4 1 0
6 3 3 0
3 3 3 2
"""
) == "yes\nyes\nyes\nno\nno"

# minimum case
assert run(
"""1
3 0 0 0
"""
) == "yes"

# n not divisible by 3
assert run(
"""1
4 0 0 0
"""
) == "no"

# already perfectly balanced
assert run(
"""1
9 9 0 0
"""
) == "yes"

# large values
assert run(
"""1
999999999999 0 0 0
"""
) == "yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 0 0 0`|`yes`| Giải đấu cân bằng nhỏ nhất | 
|`4 0 0 0`|`no`| Chia hết cho 3 là bắt buộc | 
|`9 9 0 0`|`yes`| Giải đấu đã kết thúc và cân bằng | 
|`999999999999 0 0 0`|`yes`| Xử lý số học quy mô tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
4 0 0 0
```Thuật toán kiểm tra ngay lập tức`n % 3`. Từ`4 % 3 = 1`, điểm số cuối cùng bằng nhau là không thể. Đầu ra là`"no"`. Một giải pháp chỉ đưa ra lý do về thứ hạng hiện tại sẽ chấp nhận trường hợp này một cách sai lầm. 

Coi như:```
1
6 3 3 0
```Sử dụng phép gán dấu hợp lệ sẽ cho:```
(a,b,c) = (3,0,0)
```Mục tiêu là`2`. Từ`max(a,b,c)=3`, một đội đã có nhiều trận thắng hơn mức cho phép ở trạng thái cân bằng cuối cùng. Thuật toán từ chối cấu hình và đầu ra`"no"`. 

Coi như:```
1
3 3 3 2
```Tất cả bốn bài tập ký hiệu đều được kiểm tra. Một số tạo ra số tiền thắng âm, trong khi một số khác không đáp ứng yêu cầu chia hết. Không có sự phân phối hợp lệ nào về số tiền thắng hiện tại. Thuật toán xuất ra chính xác`"no"`bởi vì sự khác biệt được báo cáo không thể phát sinh sau ba trận đấu. 

Coi như:```
1
6 4 1 0
```Một cấu hình mang lại:```
(a,b,c) = (2,1,1)
```Không có đội nào vượt chỉ tiêu`2`, và chính xác hai chiến thắng vẫn được phân phối. Các chiến thắng bổ sung cần thiết cũng là`2`, do đó kết quả đầu ra của thuật toán`"yes"`. Điều này khẳng định rằng việc có một người lãnh đạo hiện tại là có thể chấp nhận được miễn là người lãnh đạo đó chưa vượt chỉ tiêu cuối cùng.
