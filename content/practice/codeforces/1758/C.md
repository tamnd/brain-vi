---
title: "CF 1758C - Hầu như tất cả bội số"
description: "Chúng ta cần xây dựng một hoán vị của các số nguyên từ 1 đến n với ba yêu cầu đặc biệt. Vị trí đầu tiên phải chứa x. Vị trí cuối cùng phải chứa 1. Với mọi vị trí i từ 1 đến n-1, giá trị được đặt ở đó phải là bội số của i."
date: "2026-06-09T14:44:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1758
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 836 (Div. 2)"
rating: 1400
weight: 1758
solve_time_s: 547
verified: false
draft: false
---

[CF 1758C - Hầu như tất cả bội số](https://codeforces.com/problemset/problem/1758/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, lý thuyết số 
**Thời gian giải:** 9 phút 7 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một hoán vị của các số nguyên từ`1`ĐẾN`n`với ba yêu cầu đặc biệt. 

Vị trí đầu tiên phải chứa`x`. Vị trí cuối cùng phải chứa`1`. Đối với mọi vị trí`i`từ`1`ĐẾN`n-1`, giá trị được đặt ở đó phải là bội số của`i`. 

Trong số tất cả các hoán vị hợp lệ, chúng ta cần hoán vị nhỏ nhất về mặt từ điển. Nếu không tồn tại hoán vị hợp lệ, chúng ta xuất ra`-1`. 

Các ràng buộc đủ lớn để chúng ta phải xây dựng câu trả lời một cách trực tiếp. Tổng của tất cả`n`giá trị nhiều nhất là`2·10^5`, có nghĩa là một`O(n log n)`hoặc thậm chí`O(n√n)`giải pháp cho mỗi trường hợp thử nghiệm là ổn miễn là tổng công việc trên tất cả các trường hợp thử nghiệm vẫn gần tuyến tính. Việc tìm kiếm mạnh mẽ trên các hoán vị là không thể. Ngay cả đối với`n = 15`, đã có hơn một nghìn tỷ hoán vị. 

Trường hợp cạnh tinh tế xuất hiện khi`x`không chia`n`. 

Ví dụ:```
n = 5, x = 4
```Vị trí đầu tiên chứa`4`. Cuối cùng giá trị`5`phải xuất hiện ở đâu đó. Vì mọi vị trí ngoại trừ vị trí cuối cùng phải chứa bội số chỉ mục của nó nên không có cách nào định tuyến các giá trị bắt buộc sao cho tất cả các ràng buộc đều được giữ nguyên. Câu trả lời đúng là:```
-1
```Một sai lầm dễ mắc phải khác là cho rằng việc đặt`x`ở vị trí`1`Và`1`ở vị trí`n`tự động xác định phần còn lại. 

Coi như:```
n = 12, x = 3
```Hoán vị nhận dạng với hai sửa đổi đó không hợp lệ vì giá trị bị xóa khỏi vị trí`1`phải được di dời trong khi vẫn bảo toàn tất cả các ràng buộc về khả năng chia hết. 

Điểm tinh tế thứ ba là tính tối giản về mặt từ điển. Chúng tôi không chỉ đơn thuần tìm kiếm bất kỳ hoán vị hợp lệ nào. Nếu tồn tại một số hoán vị hợp lệ, chúng ta phải giữ mọi vị trí càng nhỏ càng tốt từ trái sang phải. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra các hoán vị và kiểm tra xem mọi vị trí có thỏa mãn điều kiện chia hết hay không. Ngay cả việc kiểm tra tất cả các hoán vị có kích thước 15 cũng đã vô vọng, vì vậy chúng ta cần khai thác cấu trúc của các ràng buộc. 

Quan sát quan trọng là hoán vị danh tính đã thỏa mãn yêu cầu chia hết vì mọi số đều là bội số của chính nó. 

Nếu không có hạn chế về vị trí`1`Và`n`, hoán vị```
1 2 3 ... n
```sẽ hợp lệ. 

Vấn đề duy nhất là vị trí đó`1`phải chứa`x`và vị trí`n`phải chứa`1`. 

Điều này gợi ý rằng hãy bắt đầu từ việc hoán vị danh tính và chỉ sửa đổi một chuỗi vị trí nhỏ. Điều kiện chia hết rất hạn chế. Nếu vị trí`i`không chứa`i`, thì nó phải chứa bội số lớn hơn của`i`. 

Giả sử chúng ta đặt`x`ở vị trí`1`. Khi đó giá trị ban đầu thuộc về vị trí`x`phải chuyển đi nơi khác. Việc lặp lại đối số này sẽ tạo ra một chuỗi các vị trí trong đó mỗi vị trí tiếp theo là bội số của vị trí trước đó. 

Để giải pháp tồn tại, cuối cùng chúng ta cần đạt được vị trí`n`. Điều này ngay lập tức ngụ ý rằng`x`phải chia`n`. Nếu như`x`không chia`n`, không tồn tại chuỗi hợp lệ. 

Để có được câu trả lời nhỏ nhất về mặt từ điển, chúng tôi luôn muốn vị trí được thay đổi sớm nhất nhận được giá trị hợp lệ nhỏ nhất có thể. Điều đó dẫn đến sự chuyển đổi tham lam từ vị trí hiện tại sang bội số chia nhỏ nhất mà vẫn cho phép đạt được`n`. 

Việc xây dựng kết quả tuân theo một chuỗi chia từ`x`ĐẾN`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Chuỗi chia tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`n`không chia hết cho`x`, đầu ra`-1`. 
2. Khởi tạo hoán vị là hoán vị nhận dạng:```
p[i] = i
```3. Đặt:```
p[1] = x
p[n] = 1
```4. Hãy để`cur = x`. 
5. Trong khi`cur != n`, tìm bội số nhỏ nhất`next`của`cur`như vậy: 

-`next`chia rẽ`n`-`next > cur`Trong số tất cả các ứng cử viên như vậy, hãy chọn cái nhỏ nhất. 
6. Đặt:```
p[cur] = next
```và di chuyển:```
cur = next
```7. Sau khi đạt được`n`, bộ:```
p[n] = 1
```8. Xuất hoán vị. 

Sự lựa chọn tham lam là trái tim của giải pháp. Ở mỗi bước chúng ta chọn ước số tiếp theo hợp lệ nhỏ nhất của`n`đó cũng là bội số của vị trí hiện tại. Bất kỳ lựa chọn nào lớn hơn sẽ làm tăng vị trí sớm hơn của hoán vị và do đó tạo ra một câu trả lời lớn hơn về mặt từ điển. 

### Tại sao nó hoạt động 

Mọi vị trí được sửa đổi đều thuộc về một chuỗi```
1 -> x -> ... -> n -> 1
```Mỗi lần chuyển đổi đi từ một số đến một trong các bội số của nó, do đó yêu cầu về tính chia hết vẫn được đáp ứng. 

Tất cả các vị trí bên ngoài chuỗi giữ nguyên giá trị ban đầu, cũng đáp ứng yêu cầu chia hết. 

Chuỗi chỉ sử dụng ước số của`n`, vì vậy mỗi bước cuối cùng có thể đạt được`n`. Nếu như`x`không chia`n`, một chuỗi như vậy không thể tồn tại vì mọi nước đi đều bảo toàn mối quan hệ chia hết với`n`. 

Quy tắc tham lam chọn giá trị tiếp theo nhỏ nhất có thể ở vị trí sớm nhất nơi tồn tại lựa chọn. Thứ tự từ điển được xác định bởi vị trí khác nhau đầu tiên, do đó điều này đảm bảo hoán vị hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, x = map(int, input().split())

        if n % x != 0:
            print(-1)
            continue

        p = list(range(n + 1))

        p[1] = x
        p[n] = 1

        cur = x

        while cur != n:
            nxt = n

            d = cur * 2
            while d <= n:
                if n % d == 0 and d % cur == 0:
                    nxt = d
                    break
                d += cur

            p[cur] = nxt
            cur = nxt

        print(*p[1:])

solve()
```Hoán vị bắt đầu như hoán vị nhận dạng vì điều đó đã thỏa mãn tất cả các ràng buộc về tính chia hết. 

Các vị trí duy nhất thay đổi là những vị trí trên chuỗi chia. Đối với vị trí hiện tại`cur`, chúng tôi tìm kiếm thông qua bội số của`cur`và chọn cái nhỏ nhất vẫn chia được`n`. Sự lựa chọn đó bảo toàn khả năng cuối cùng đạt được`n`trong khi thực hiện hoán vị tối thiểu về mặt từ điển. 

Một lỗi thực hiện phổ biến là quên rằng`p[n]`phải kết thúc như`1`. Một cách khác là thay thế các vị trí bên ngoài chuỗi. Mọi vị trí không bị ảnh hưởng sẽ vẫn bằng chỉ số của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
x = 2
```Hoán vị ban đầu: 

| Vị trí | 1 | 2 | 3 | 4 | 
| --- | --- | --- | --- | --- | 
| Giá trị | 2 | 2 | 3 | 1 | 

Vị trí chuỗi hiện tại là`2`. 

Bội số nhỏ nhất của`2`đó là sự chia rẽ`4`là`4`. 

| Hiện tại | Tiếp theo | 
| --- | --- | 
| 2 | 4 | 

Cập nhật:```
p[2] = 4
```Hoán vị cuối cùng: 

| Vị trí | 1 | 2 | 3 | 4 | 
| --- | --- | --- | --- | --- | 
| Giá trị | 2 | 4 | 3 | 1 | 

Đầu ra:```
2 4 3 1
```Ví dụ này cho thấy chuỗi không cần thiết đơn giản nhất. 

### Ví dụ 2 

đầu vào:```
n = 12
x = 3
```Bắt đầu: 

| Hiện tại | 
| --- | 
| 3 | 

Thí sinh là bội số của 3 chia 12. 

Lựa chọn hợp lệ nhỏ nhất: 

| Hiện tại | Tiếp theo | 
| --- | --- | 
| 3 | 6 | 

Sau đó: 

| Hiện tại | Tiếp theo | 
| --- | --- | 
| 6 | 12 | 

Cập nhật:```
p[3] = 6
p[6] = 12
```Hoán vị cuối cùng:```
3 2 6 4 5 12 7 8 9 10 11 1
```Chuỗi là:```
3 -> 6 -> 12
```và mọi vị trí khác không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi bước trong chuỗi sẽ quét bội số, tổng công việc trong một trường hợp thử nghiệm được giới hạn bởi hành vi chuỗi hài hòa | 
| Không gian | O(n) | Lưu trữ hoán vị | 

Vì tổng của tất cả`n`giá trị nhiều nhất là`2·10^5`, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = []

    t = int(input())

    for _ in range(t):
        n, x = map(int, input().split())

        if n % x:
            out.append("-1")
            continue

        p = list(range(n + 1))
        p[1] = x
        p[n] = 1

        cur = x

        while cur != n:
            nxt = n

            d = cur * 2
            while d <= n:
                if n % d == 0 and d % cur == 0:
                    nxt = d
                    break
                d += cur

            p[cur] = nxt
            cur = nxt

        out.append(" ".join(map(str, p[1:])))

    return "\n".join(out)

# provided samples
assert run("3\n3 3\n4 2\n5 4\n") == (
    "3 2 1\n"
    "2 4 3 1\n"
    "-1"
)

# minimum valid case
assert run("1\n2 2\n") == "2 1"

# impossible because x does not divide n
assert run("1\n10 6\n") == "-1"

# direct chain
assert run("1\n6 3\n") == "3 2 6 4 5 1"

# larger divisor chain
assert run("1\n12 3\n") == "3 2 6 4 5 12 7 8 9 10 11 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`|`2 1`| Trường hợp hợp lệ nhỏ nhất | 
|`10 6`|`-1`| Điều kiện cần `x | 
|`6 3`|`3 2 6 4 5 1`| Chuyển đổi chuỗi đơn | 
|`12 3`|`3 2 6 4 5 12 7 8 9 10 11 1`| Chuyển đổi nhiều chuỗi | 

## Vỏ cạnh 

Hãy xem xét:```
n = 5
x = 4
```Từ`5 % 4 != 0`, thuật toán trả về ngay:```
-1
```Không có chuỗi chia từ`4`có thể đạt được`5`, do đó một hoán vị hợp lệ không thể tồn tại. 

Coi như:```
n = 2
x = 2
```Hoán vị danh tính trở thành:```
2 1
```Chuỗi bắt đầu lúc`2`, vốn đã vậy rồi`n`, vì vậy không cần phải làm gì thêm. 

Coi như:```
n = 16
x = 2
```Chuỗi trở thành:```
2 -> 4 -> 8 -> 16
```Thuật toán cập nhật chính xác các vị trí đó và giữ nguyên mọi vị trí khác. Mỗi giá trị được gán là bội số của vị trí của nó và lựa chọn tham lam ở mỗi bước sẽ tạo ra hoán vị hợp lệ nhỏ nhất về mặt từ điển.
