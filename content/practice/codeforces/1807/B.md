---
title: "CF 1807B - Lấy Kẹo"
description: "Chúng tôi có vài túi kẹo. Một túi chứa số kẹo chẵn luôn thuộc về Mihai, trong khi một túi chứa số kẹo lẻ luôn thuộc về Bianca. Thứ tự của các túi không cố định. Chúng tôi có thể sắp xếp lại chúng theo cách chúng tôi muốn trước khi trò chơi bắt đầu."
date: "2026-06-09T09:02:16+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1807
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 859 (Div. 4)"
rating: 800
weight: 1807
solve_time_s: 86
verified: true
draft: false
---

[CF 1807B - Lấy kẹo](https://codeforces.com/problemset/problem/1807/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có vài túi kẹo. Một túi chứa số kẹo chẵn luôn thuộc về Mihai, trong khi một túi chứa số kẹo lẻ luôn thuộc về Bianca. 

Thứ tự của các túi không cố định. Chúng tôi có thể sắp xếp lại chúng theo cách chúng tôi muốn trước khi trò chơi bắt đầu. Khi các túi được xử lý từ trái sang phải, mỗi người chơi sẽ tích lũy kẹo từ các túi họ nhận được. 

Mục đích là để xác định xem có tồn tại thứ tự nào đó sao cho sau mỗi túi được xử lý, ngoại trừ trước khi trò chơi bắt đầu, tổng số kẹo của Mihai lớn hơn tổng số của Bianca. 

Những hạn chế là rất nhỏ. Mỗi test case có tối đa 100 túi và có tối đa 1000 test case. Thậm chí một$O(n^2)$giải pháp sẽ dễ dàng đủ nhanh. Thách thức không phải là tính hiệu quả mà là việc nhận ra đặc tính quyết định liệu một thứ tự hợp lệ có tồn tại hay không. 

Một sai lầm phổ biến là tập trung vào việc duy trì sự bất bình đẳng sau mỗi bước bằng cách xây dựng một thứ tự rõ ràng. Ví dụ:```
1
4
1 2 3 4
```Một mô phỏng tham lam có thể tìm thấy một thứ tự, nhưng vấn đề chỉ hỏi liệu thứ tự đó có tồn tại hay không. Có một điều kiện đơn giản hơn nhiều. 

Một trường hợp tinh tế khác là khi chỉ có một túi chẵn:```
1
4
1 1 1 2
```Tổng số kẹo chẵn là$2$, trong khi tổng số kẹo lẻ là$3$. Mihai không thể dẫn đầu mãi vì tổng điểm của Bianca cuối cùng đã vượt quá Mihai. Câu trả lời đúng là`NO`. 

Trường hợp cạnh thứ ba là khi tất cả các túi đều là số lẻ:```
1
3
1 3 5
```Túi xử lý đầu tiên phải đến Bianca, khiến Mihai tổng cộng$0$và Bianca dương tính. Bất đẳng thức yêu cầu thất bại ngay lập tức. Câu trả lời đúng là`NO`. 

Cuối cùng, hãy xem xét:```
1
3
2 2 1
```Tổng chẵn là$4$và tổng lẻ là$1$. Câu trả lời đúng là`YES`. Một cách tiếp cận bất cẩn chỉ kiểm tra giá trị chẵn lớn nhất so với giá trị lẻ lớn nhất sẽ bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là thử mọi hoán vị có thể có của các túi và mô phỏng trò chơi. Đối với mỗi hoán vị, chúng tôi xử lý từng túi một, cập nhật tổng số của Mihai và Bianca và kiểm tra xem Mihai có luôn dẫn đầu sau mỗi bước hay không. 

Cách tiếp cận này đúng vì nó xem xét mọi thứ tự có thể. Vấn đề là có$n!$hoán vị. Ngay cả đối với$n = 10$, con số này đã là khoảng 3,6 triệu hoán vị. Vì$n = 100$, điều đó là hoàn toàn không thể. 

Để tìm ra giải pháp tốt hơn, chúng ta cần hiểu điều gì thực sự quan trọng. 

Giả sử chúng ta đặt tất cả các túi chẵn lên đầu tiên. Mỗi chiếc túi đó chỉ làm tăng tổng số tiền của Mihai. Đây rõ ràng là cách tốt nhất có thể để xây dựng lợi thế. Sau khi tất cả các túi chẵn đã được xử lý, Mihai đã thu được toàn bộ số túi có giá trị chẵn, trong khi Bianca vẫn chưa thu được gì. 

Bây giờ các túi lẻ có thể được xử lý theo bất kỳ thứ tự nào. Mỗi túi lẻ chỉ làm tăng tổng số tiền của Bianca. Tổng số của Mihai không bao giờ thay đổi nữa. 

Quan sát này là rất quan trọng. Nếu tổng số tiền của tất cả các túi chẵn lớn hơn tổng số tiền của tất cả các túi lẻ thì sau khi xử lý tất cả các túi chẵn trước, Mihai bắt đầu giai đoạn lẻ với số tiền dẫn đầu lớn hơn tổng số cuối cùng cuối cùng của Bianca. Bianca không bao giờ có thể đuổi kịp nên mọi trạng thái trung gian cũng an toàn. 

Nếu tổng số tiền của các túi chẵn nhỏ hơn hoặc bằng tổng số tiền của các túi lẻ thì khi kết thúc toàn bộ quá trình, tổng của Mihai không lớn hơn tổng của Bianca. Vì yêu cầu phải được đáp ứng sau mỗi bước nên chắc chắn nó phải được đáp ứng sau bước cuối cùng. Một trật tự như vậy không thể tồn tại. 

Toàn bộ vấn đề quy về việc so sánh hai tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với trường hợp thử nghiệm, khởi tạo hai biến:`even_sum`Và`odd_sum`. 
2. Đi qua tất cả các túi. 
3. Nếu một túi có số kẹo chẵn thì cộng giá trị của nó vào`even_sum`. 
4. Nếu không, hãy cộng giá trị của nó vào`odd_sum`. 
5. Sau khi xử lý tất cả các túi, so sánh tổng số của hai túi. 
6. Nếu`even_sum > odd_sum`, in`YES`. 
7. Nếu không, hãy in`NO`. 

Tại sao nó hoạt động: 

Tổng số cuối cùng được cố định bất kể thứ tự. Mihai luôn kết thúc bằng tổng của tất cả các túi có giá trị chẵn và Bianca luôn kết thúc bằng tổng của tất cả các túi có giá trị lẻ. 

Nếu như`even_sum <= odd_sum`, bất đẳng thức bắt buộc đã thất bại ở trạng thái cuối cùng, do đó không có thứ tự nào có thể hoạt động được. 

Nếu như`even_sum > odd_sum`, đặt các túi chẵn trước các túi lẻ. Trong giai đoạn đầu, tổng điểm của Bianca vẫn bằng 0 trong khi tổng điểm của Mihai tăng lên. Trong giai đoạn thứ hai, tổng số điểm của Mihai vẫn bằng`even_sum`và tổng số hoạt động của Bianca không bao giờ vượt quá`odd_sum`. Từ`even_sum > odd_sum`, Mihai vẫn dẫn đầu trong suốt quá trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    even_sum = 0
    odd_sum = 0

    for x in a:
        if x % 2 == 0:
            even_sum += x
        else:
            odd_sum += x

    print("YES" if even_sum > odd_sum else "NO")
```Giải pháp theo sau bằng chứng trực tiếp. 

Vòng lặp tách tất cả các giá trị túi thành hai nhóm theo tính chẵn lẻ.`even_sum`đại diện cho tổng số cuối cùng của Mihai, trong khi`odd_sum`đại diện cho tổng số cuối cùng của Bianca. 

Sau khi tất cả các giá trị được xử lý, chỉ cần một so sánh. Sự bất bình đẳng nghiêm ngặt là quan trọng. sử dụng`>=`sẽ sai vì phát biểu yêu cầu Mihai phải có nhiều kẹo hơn ở mọi thời điểm hợp lệ, kể cả trạng thái cuối cùng. 

Không cần xử lý đặc biệt đối với các nhóm chẵn lẻ trống. Nếu không có túi chẵn,`even_sum`trở thành 0 và việc so sánh đương nhiên tạo ra kết quả chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
```| Giá trị túi | Chẵn lẻ | chẵn_sum | lẻ_sum | 
| --- | --- | --- | --- | 
| 1 | lẻ | 0 | 1 | 
| 2 | Thậm chí | 2 | 1 | 
| 3 | lẻ | 2 | 4 | 
| 4 | Thậm chí | 6 | 4 | 

So sánh cuối cùng: 

| chẵn_sum | lẻ_sum | Trả lời | 
| --- | --- | --- | 
| 6 | 4 | CÓ | 

Các túi có giá trị chẵn đóng góp tổng cộng 6 viên kẹo, trong khi các túi có giá trị lẻ đóng góp 4. Vì 6 lớn hơn 4 nên việc đặt tất cả các túi chẵn trước sẽ đảm bảo Mihai luôn dẫn đầu. 

### Ví dụ 2 

đầu vào:```
4
1 1 1 2
```| Giá trị túi | Chẵn lẻ | chẵn_sum | lẻ_sum | 
| --- | --- | --- | --- | 
| 1 | lẻ | 0 | 1 | 
| 1 | lẻ | 0 | 2 | 
| 1 | lẻ | 0 | 3 | 
| 2 | Thậm chí | 2 | 3 | 

So sánh cuối cùng: 

| chẵn_sum | lẻ_sum | Trả lời | 
| --- | --- | --- | 
| 2 | 3 | KHÔNG | 

Tổng số cuối cùng mà Bianca có được lớn hơn tổng số cuối cùng của Mihai. Vì bất đẳng thức yêu cầu cũng phải được giữ sau túi cuối cùng nên không có đơn đặt hàng nào có thể thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi túi được kiểm tra đúng một lần | 
| Không gian |$O(1)$| Chỉ có hai khoản tiền đang chạy được lưu trữ | 

Với tối đa 100 túi cho mỗi trường hợp thử nghiệm, thời gian chạy rất nhỏ. Ngay cả trên 1000 trường hợp thử nghiệm, giải pháp chỉ thực hiện khoảng 100.000 thao tác, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        even_sum = sum(x for x in a if x % 2 == 0)
        odd_sum = sum(x for x in a if x % 2 == 1)

        ans.append("YES" if even_sum > odd_sum else "NO")

    sys.stdout.write("\n".join(ans))

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
assert run(
"""3
4
1 2 3 4
4
1 1 1 2
3
1 4 3
"""
) == "YES\nNO\nNO"

# minimum size
assert run(
"""1
1
2
"""
) == "YES"

# single odd bag
assert run(
"""1
1
1
"""
) == "NO"

# all equal even values
assert run(
"""1
5
2 2 2 2 2
"""
) == "YES"

# equality boundary
assert run(
"""1
2
1 1
"""
) == "NO"

# larger mixed case
assert run(
"""1
6
2 4 6 1 3 5
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 2`|`YES`| Kích thước tối thiểu với một túi chẵn | 
|`1 / 1 / 1`|`NO`| Kích thước tối thiểu với một chiếc túi lẻ | 
|`2 2 2 2 2`|`YES`| Tất cả các giá trị chẵn | 
|`1 1`|`NO`| Bất bình đẳng nghiêm khắc, bình đẳng chưa đủ | 
|`2 4 6 1 3 5`|`YES`| Chênh lệch hỗn hợp với tổng số chẵn lớn hơn | 

## Vỏ cạnh 

### Tất cả các túi đều là số lẻ 

đầu vào:```
1
3
1 3 5
```Thuật toán tính toán: 

| chẵn_sum | lẻ_sum | 
| --- | --- | 
| 0 | 9 | 

Từ`0 > 9`là sai, nó in`NO`. 

Điều này phù hợp với thực tế. Túi xử lý đầu tiên phải đến Bianca nên Mihai không thể dẫn trước ngay sau nước đi đầu tiên. 

### Tổng số bằng nhau 

đầu vào:```
1
2
1 1
```Thuật toán tính toán: 

| chẵn_sum | lẻ_sum | 
| --- | --- | 
| 0 | 2 | 

Câu trả lời là`NO`. 

Tổng quát hơn, bất cứ khi nào hai tổng số bằng nhau, trạng thái cuối cùng sẽ là Mihai và Bianca hòa. Điều kiện yêu cầu Mihai phải nghiêm khắc đi trước nên không thể chấp nhận sự bình đẳng. 

###Chỉ Một Túi Chẵn 

đầu vào:```
1
4
1 1 1 2
```Thuật toán tính toán: 

| chẵn_sum | lẻ_sum | 
| --- | --- | 
| 2 | 3 | 

Câu trả lời là`NO`. 

Ngay cả khi túi chứa 2 viên kẹo được đặt trước, tổng điểm cuối cùng tối đa có thể có của Mihai vẫn chỉ là 2. Bianca cuối cùng đạt đến 3, do đó, bất đẳng thức bắt buộc không thể giữ nguyên cho đến cuối cùng. 

### Lợi thế chẵn lớn 

đầu vào:```
1
3
2 2 1
```Thuật toán tính toán: 

| chẵn_sum | lẻ_sum | 
| --- | --- | 
| 4 | 1 | 

Câu trả lời là`YES`. 

Đặt các túi chẵn lên trước:`[2, 2, 1]`. 

Sau khi xử lý các túi, tổng số chạy là: 

| Bước | Mihai | Bianca | 
| --- | --- | --- | 
| Sau 2 | 2 | 0 | 
| Sau 2 | 4 | 0 | 
| Sau 1 | 4 | 1 | 

Mihai luôn dẫn đầu, đúng như dự đoán của tình hình`even_sum > odd_sum`.
