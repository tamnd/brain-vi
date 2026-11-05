---
title: "CF 121A - Tiền May Mắn"
description: "Chúng ta cần tính tổng trong khoảng [l, r]. Với mỗi số nguyên x trong phạm vi đó, chúng ta tính toán next(x), trong đó next(x) có nghĩa là số may mắn nhỏ nhất lớn hơn hoặc bằng x. Số may mắn là số nguyên dương có chữ số thập phân chỉ là 4 và 7."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 121
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 91 (Div. 1 Only)"
rating: 1100
weight: 121
solve_time_s: 96
verified: true
draft: false
---

[CF 121A - Số tiền may mắn](https://codeforces.com/problemset/problem/121/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tính tổng trong một khoảng`[l, r]`. Với mọi số nguyên`x`trong phạm vi đó, chúng tôi tính toán`next(x)`, Ở đâu`next(x)`có nghĩa là số may mắn nhỏ nhất lớn hơn hoặc bằng`x`. 

Số may mắn là số nguyên dương có chữ số thập phân chỉ`4`Và`7`. Ví dụ là`4`,`7`,`44`,`47`,`74`,`77`, vân vân. 

Nhiệm vụ không phải là đếm số may mắn. Chúng ta phải tính tổng giá trị của số may mắn tiếp theo cho mọi số nguyên trong khoảng. 

Ví dụ, nếu`l = 2`Và`r = 7`: 

-`next(2) = 4`-`next(3) = 4`-`next(4) = 4`-`next(5) = 7`-`next(6) = 7`-`next(7) = 7`Câu trả lời là`4 + 4 + 4 + 7 + 7 + 7 = 33`. 

Các ràng buộc nhỏ một cách bất thường. Bản thân khoảng đó có thể rất lớn bởi vì`r`có thể đạt được`10^9`, vì vậy việc lặp qua mọi giá trị từ`l`ĐẾN`r`là không thể trong trường hợp xấu nhất. Vòng lặp hơn một tỷ số sẽ mất quá nhiều thời gian trong giới hạn thời gian 2 giây. 

Đồng thời, số lượng con số may mắn lên tới`10^9`là nhỏ bé. Số may mắn ở đây có tối đa 10 chữ số và mỗi chữ số chỉ có 2 lựa chọn,`4`hoặc`7`. Điều đó có nghĩa là tổng số là:`2^1 + 2^2 + ... + 2^10 = 2046`Làm việc với vài nghìn giá trị là chuyện nhỏ. Toàn bộ vấn đề được thiết kế xung quanh việc khai thác lỗ hổng này. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Coi như:```
4 4
```Câu trả lời là`4`, bởi vì`next(4) = 4`. Việc triển khai có lỗi tìm kiếm số may mắn đầu tiên lớn hơn rất nhiều`x`sẽ sử dụng sai`7`. 

Một trường hợp phức tạp khác là khi khoảng vượt qua nhiều dãy số may mắn:```
5 8
```Những đóng góp đúng là: 

-`5,6,7 -> 7`-`8 -> 44`Vì vậy, câu trả lời là:```
7 + 7 + 7 + 44 = 65
```Việc triển khai nhóm ngây thơ có thể dễ dàng quên xử lý phần đuôi sau con số may mắn cuối cùng trong khoảng. 

Những ranh giới lớn cũng quan trọng:```
999999999 1000000000
```Con số may mắn tiếp theo cho cả hai giá trị là`4444444444`, vượt quá`10^9`. Nếu chúng ta chỉ tạo ra những con số may mắn tối đa`10^9`, chúng ta bỏ sót câu trả lời đúng hoàn toàn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mọi số nguyên`x`từ`l`ĐẾN`r`, tạo hoặc tìm kiếm số may mắn nhỏ nhất lớn hơn hoặc bằng`x`, sau đó thêm nó vào câu trả lời. 

Điều này hoạt động hợp lý vì định nghĩa của tổng là trực tiếp. Vấn đề là hiệu suất. Trong trường hợp xấu nhất, độ dài khoảng gần bằng`10^9`. Thậm chí một`O(1)`hoạt động trên mỗi giá trị sẽ quá chậm. 

Cấu trúc của những con số may mắn cho chúng ta một con đường tốt hơn. Giá trị của`next(x)`không đổi trên toàn bộ phạm vi. 

Ví dụ: 

| phạm vi x | tiếp theo(x) | 
| --- | --- | 
| 1..4 | 4 | 
| 5..7 | 7 | 
| 8..44 | 44 | 
| 45..47 | 47 | 

Thay vì xử lý từng số nguyên riêng lẻ, chúng ta có thể xử lý các phạm vi này theo khối. 

Quan sát quan trọng là khi chúng ta biết danh sách các số may mắn đã được sắp xếp, mọi khoảng cách giữa hai số may mắn liên tiếp đều đóng góp cùng một giá trị lặp đi lặp lại. 

Giả sử con số may mắn hiện tại là`44`, và con số may mắn trước đó là`7`. Khi đó mọi số từ`8`bởi vì`44`đóng góp`44`. 

Điều đó có nghĩa là chúng ta chỉ cần: 

1. Tạo tất cả các con số may mắn. 
2. Sắp xếp chúng. 
3. Nhìn qua các con số may mắn và tính xem số tiền may mắn là bao nhiêu.`[l, r]`rơi vào từng đoạn. 

Vì chỉ có khoảng hai nghìn con số may mắn nên giải pháp này cực kỳ nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r - l + 1) | O(1) | Quá chậm | 
| Tối ưu | O(2^10) | O(2^10) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mọi số may mắn có tối đa 10 chữ số bằng cách sử dụng đệ quy. 

Ở mỗi bước, hãy thêm một trong hai`4`hoặc`7`đến con số hiện tại. Điều này khám phá cây nhị phân đầy đủ của các công trình số may mắn. 
2. Lưu trữ tất cả các số may mắn được tạo trong danh sách. 

Chúng tôi bao gồm các giá trị lớn hơn`10^9`bởi vì`next(x)`đối với các số gần giới hạn trên có thể vượt quá`10^9`. 
3. Sắp xếp các con số may mắn. 

Việc tạo đệ quy không đảm bảo thứ tự được sắp xếp một cách tự nhiên. 
4. Khởi tạo: 

-`ans = 0`-`cur = l`Biến`cur`theo dõi giá trị đầu tiên trong khoảng chưa được xử lý. 
5. Lặp lại các số may mắn theo thứ tự tăng dần. 

Với mỗi con số may mắn`v`:

- Nếu như`v < cur`, hãy bỏ qua vì phân đoạn của nó đã ở phía sau chúng ta. 
- Ngược lại, khoảng thời gian từ`cur`lên tới`min(r, v)`tất cả đều có`next(x) = v`. 
6. Tính xem có bao nhiêu số thuộc đoạn này. 

Số đếm là:$\text{count}=\min(r,v)-cur+1$7. Thêm đóng góp của phân khúc này.$\text{contribution}=v \times \text{count}$8. Di chuyển`cur`đến giá trị chưa được xử lý đầu tiên sau phân đoạn này. 

Bộ:```
cur = v + 1
```9. Dừng lại một lần`cur > r`. 

Mọi giá trị trong khoảng giờ đã được tính toán. 

### Tại sao nó hoạt động 

Thuật toán dựa vào việc phân chia trục số theo số may mắn. 

Đối với một con số may mắn`v`, mọi số nguyên đều lớn hơn số may mắn trước đó và nhiều nhất là`v`có`next(x) = v`. 

Các phân đoạn này rời rạc và cùng nhau bao trùm toàn bộ khoảng thời gian`[l, r]`. 

Trong quá trình lặp lại,`cur`luôn trỏ đến số nguyên chưa được xử lý đầu tiên. Khi chúng tôi xử lý một con số may mắn`v`, chúng ta cộng chính xác phần đóng góp của tất cả các số nguyên có số may mắn tiếp theo bằng`v`. Sau khi thăng tiến`cur`ĐẾN`v + 1`, không có giá trị được xử lý nào được xem lại. 

Vì mọi số nguyên trong`[l, r]`thuộc về chính xác một phân đoạn như vậy thì tổng cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

l, r = map(int, input().split())

lucky = []

def gen(x):
    if x > 10**10:
        return

    if x > 0:
        lucky.append(x)

    gen(x * 10 + 4)
    gen(x * 10 + 7)

gen(0)

lucky.sort()

ans = 0
cur = l

for v in lucky:
    if cur > r:
        break

    if v < cur:
        continue

    right = min(r, v)
    cnt = right - cur + 1

    ans += v * cnt
    cur = right + 1

print(ans)
```Trình tạo đệ quy xây dựng mọi số may mắn bằng cách liên tục nối thêm`4`Và`7`. 

Bắt đầu từ`0`thuận tiện vì: 

-`0`bản thân nó không được lưu trữ. 
- Con cái của nó trở thành`4`Và`7`. 

Độ sâu đệ quy rất nhỏ, tối đa là 10 cấp độ, vì vậy đệ quy ở đây hoàn toàn an toàn. 

Điều kiện dừng sử dụng`10^10`thay vì`10^9`. Điều này tuy tế nhị nhưng cần thiết. Nếu như`x = 10^9`, con số may mắn tiếp theo của nó thực ra là`4444444444`, không phải một số giá trị dưới đây`10^9`. 

Sau khi sắp xếp, vòng lặp chính xử lý các số may mắn theo thứ tự tăng dần. Với mỗi con số may mắn`v`, khoảng`[cur, min(r, v)]`đóng góp`v`. 

Dòng:```
cnt = right - cur + 1
```rất dễ mắc sai lầm. Khoảng này bao gồm cả hai đầu, vì vậy chúng ta cần`+1`. 

Một điểm tinh tế khác là:```
cur = right + 1
```không`v + 1`. 

Giả định`r < v`. Sau đó chỉ một phần của phân khúc được sử dụng. Tiến thẳng tới`v + 1`vẫn hoạt động hợp lý, nhưng sử dụng`right + 1`khớp với ranh giới được xử lý thực tế và tránh sai sót nếu mã được sửa đổi sau đó. 

Số nguyên Python tự động xử lý các giá trị lớn, điều này quan trọng vì câu trả lời có thể vượt quá phạm vi số nguyên 32 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 7
```Số may mắn được tạo bắt đầu bằng:```
4, 7, 44, 47, ...
```| cur | con số may mắn v | phạm vi xử lý | đếm | giá trị gia tăng | và | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 4 | 2..4 | 3 | 12 | 12 | 
| 5 | 7 | 5..7 | 3 | 21 | 33 | 
Câu trả lời cuối cùng:```
33
```Dấu vết này cho thấy cách thuật toán nhóm nhiều số nguyên thành một phần đóng góp thay vì xử lý chúng riêng lẻ. 

### Ví dụ 2 

đầu vào:```
7 7
```| cur | con số may mắn v | phạm vi xử lý | đếm | giá trị gia tăng | và | 
| --- | --- | --- | --- | --- | --- | 
| 7 | 7 | 7..7 | 1 | 7 | 7 | 
Câu trả lời cuối cùng:```
7
```Trường hợp này xác nhận rằng khi bản thân điểm cuối khoảng là may mắn, chúng tôi sử dụng chính xác giá trị đó thay vì số may mắn lớn hơn tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^10) | Khoảng 2046 con số may mắn được tạo và xử lý | 
| Không gian | O(2^10) | Lưu trữ tất cả các con số may mắn | 

Thời gian chạy thực tế là rất nhỏ. Ngay cả với đệ quy, sắp xếp và lặp lại, tổng công việc chỉ có vài nghìn thao tác. Điều này thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    l, r = map(int, input().split())

    lucky = []

    def gen(x):
        if x > 10**10:
            return

        if x > 0:
            lucky.append(x)

        gen(x * 10 + 4)
        gen(x * 10 + 7)

    gen(0)

    lucky.sort()

    ans = 0
    cur = l

    for v in lucky:
        if cur > r:
            break

        if v < cur:
            continue

        right = min(r, v)
        cnt = right - cur + 1

        ans += v * cnt
        cur = right + 1

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("2 7\n") == "33", "sample 1"
assert run("7 7\n") == "7", "sample 2"

# minimum-size interval
assert run("1 1\n") == "4", "minimum case"

# single lucky number
assert run("4 4\n") == "4", "exact lucky number"

# crossing into next lucky segment
assert run("5 8\n") == "65", "crossing segments"

# large boundary case
assert run("999999999 1000000000\n") == "8888888888", "needs lucky number above 1e9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`4`| Khoảng thời gian nhỏ nhất có thể | 
|`4 4`|`4`| Ranh giới số may mắn chính xác | 
|`5 8`|`65`| Chuyển tiếp giữa các phân khúc may mắn | 
|`999999999 1000000000`|`8888888888`| Xử lý đúng các con số may mắn trên`10^9`| 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
4 4
```Thuật toán bắt đầu với`cur = 4`. 

Khi đạt đến con số may mắn`4`, nó xử lý phạm vi:```
[4, min(4, 4)] = [4, 4]
```Sự đóng góp là:```
4 * 1 = 4
```Sau đó`cur`trở thành`5`, kết thúc vòng lặp. Kết quả đầu ra đúng vì`next(4)`là`4`, không`7`. 

Bây giờ hãy xem xét:```
5 8
```Những con số may mắn liên quan là`7`Và`44`. 

Thuật toán xử lý: 

-`5..7`sử dụng giá trị`7`-`8..8`sử dụng giá trị`44`Vì vậy, câu trả lời trở thành:```
7 + 7 + 7 + 44 = 65
```Điều này xác nhận rằng quá trình chuyển đổi giữa các dãy số may mắn được xử lý chính xác. 

Cuối cùng, kiểm tra:```
999999999 1000000000
```Con số may mắn tiếp theo sau cả hai giá trị là`4444444444`. 

Khi lần lặp đạt đến con số may mắn đó, phạm vi được xử lý là:```
999999999..1000000000
```với số lượng`2`. 

Sự đóng góp trở thành:```
2 * 4444444444 = 8888888888
```Điều này chứng tỏ tại sao việc tạo ra những con số may mắn vượt xa`10^9`là cần thiết.
