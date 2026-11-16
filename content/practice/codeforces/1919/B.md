---
title: "CF 1919B - Chia cộng trừ"
description: "Chuỗi chỉ bao gồm '+' và '-'. Chúng ta có thể hiểu mỗi ký tự dưới dạng số: '+' trở thành +1 và '-' trở thành -1. Chúng tôi được phép chia chuỗi này thành bất kỳ số lượng phần không trống liền kề nào."
date: "2026-06-08T19:33:29+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1919
codeforces_index: "B"
codeforces_contest_name: "Hello 2024"
rating: 800
weight: 1919
solve_time_s: 118
verified: true
draft: false
---

[CF 1919B - Chia cộng-trừ](https://codeforces.com/problemset/problem/1919/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chuỗi chỉ bao gồm`'+'`Và`'-'`. Chúng ta có thể hiểu mỗi ký tự là một số:`'+'`trở thành`+1`, Và`'-'`trở thành`-1`. 

Chúng tôi được phép chia chuỗi này thành bất kỳ số lượng phần không trống liền kề nào. Với mỗi phần ta tính:$$\text{penalty} = |\text{sum of piece}| \times \text{length of piece}$$Tổng số tiền phạt là tổng số tiền phạt của tất cả các quân cờ. Mục tiêu của chúng ta là chọn cách phân chia sao cho tổng số này nhỏ nhất. 

Độ dài của chuỗi tối đa là 5000 và có thể có tới 1000 trường hợp kiểm thử. Tuyên bố nói rõ ràng rằng không có giới hạn về tổng của tất cả$n$, vì vậy chúng ta nên tìm giải pháp xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm sẽ trở nên rủi ro. 

Phần khó khăn là hình phạt phụ thuộc cả vào tổng và độ dài của một đoạn. Nhiều vấn đề về phân vùng đòi hỏi phải lập trình động, do đó, điều tự nhiên là chúng ta phải tự hỏi liệu chúng ta có phải thử nhiều cách phân chia có thể hay không. Quan sát quan trọng là cấu trúc của các giá trị cực kỳ hạn chế: mọi phần tử đều$+1$hoặc$-1$. 

Một số trường hợp khó xử lý. 

Coi như:```
+
```Mảng chỉ chứa một giá trị$+1$. Không thể chia tách được. Câu trả lời là$1$, không$0$. 

Coi như:```
+-
```Toàn bộ đoạn có tổng$0$, vậy hình phạt của nó là$0$. Câu trả lời là$0$. Giải pháp chỉ đếm từng ký tự riêng lẻ sẽ trả về sai$2$. 

Coi như:```
+++
```Mỗi phần tử đều$+1$. Không tồn tại phân đoạn có tổng bằng 0. Chia thành các phần tử đơn lẻ sẽ bị phạt$1+1+1=3$, vốn đã tối ưu rồi. Câu trả lời là$3$. 

Những ví dụ này cho thấy điều thực sự quan trọng là có bao nhiêu điểm cộng và điểm trừ có thể triệt tiêu lẫn nhau. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách có thể để phân chia mảng. 

Trong một thời gian dài-$n$mảng, có$2^{n-1}$vị trí phân vùng có thể. Ngay cả đối với$n=50$, điều này hoàn toàn không thể thực hiện được. Chúng ta cần một cái gì đó đơn giản hơn nhiều. 

Để khám phá lối tắt, hãy nhìn vào một đoạn duy nhất. 

Tổng của nó là một số nguyên. Vì mọi phần tử đều là$+1$hoặc$-1$, tổng bằng:$$(\text{number of pluses})-(\text{number of minuses})$$Giả sử một đoạn có tổng$0$. Thế thì hình phạt của nó là$$|0| \times \text{length}=0$$Một phân khúc như vậy không đóng góp gì cả. 

Bây giờ giả sử một phân đoạn có tổng$k\neq 0$. Vì mọi phần tử đều đóng góp$+1$hoặc$-1$, đoạn này chứa ít nhất$|k|$dấu hiệu chưa từng có. Hình phạt là$$|k| \times \text{length}$$và chiều dài ít nhất là$1$, Vì thế$$|k| \times \text{length}\ge |k|$$Điều này có nghĩa là mỗi phân đoạn đóng góp ít nhất giá trị tuyệt đối của tổng của nó. 

Gọi tổng các phân đoạn là$s_1,s_2,\dots,s_m$. Tổng của chúng bằng tổng của toàn bộ mảng:$$s_1+s_2+\cdots+s_m=S$$Tổng số tiền phạt ít nhất là$$|s_1|+|s_2|+\cdots+|s_m|$$Theo bất đẳng thức tam giác,$$|s_1|+|s_2|+\cdots+|s_m|\ge |S|$$Do đó mọi phân vùng có thể có ít nhất hình phạt$|S|$. 

Bây giờ chúng ta hỏi liệu giới hạn dưới này có thực sự đạt được hay không. 

Đúng. Chia mảng thành các phần tử đơn lẻ. Mỗi đoạn đều có độ dài$1$, vậy hình phạt của nó chính xác là$1$. Mỗi`'+'`đóng góp$1$, mỗi`'-'`đóng góp$1$. 

Bất cứ khi nào chúng ta có một cộng và một trừ, chúng ta có thể hợp nhất chúng thành một phân đoạn có tổng bằng 0 lớn hơn. Phân khúc đó góp phần$0$thay vì$2$. Mỗi cặp cộng trừ trùng khớp sẽ loại bỏ chính xác$2$từ tổng số. 

Nếu chuỗi chứa$P$điểm cộng và$M$điểm trừ thì$\min(P,M)$cặp có thể hủy bỏ. Số ký hiệu không khớp là$$|P-M|$$và đó chính xác là$|S|$. 

Vì vậy hình phạt tối thiểu chỉ đơn giản là$$|P-M|$$Không có phân vùng thực tế cần phải được xây dựng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu`'+'`ký tự xuất hiện trong chuỗi. Hãy để điều này được`plus`. 
2. Đếm xem có bao nhiêu`'-'`ký tự xuất hiện trong chuỗi. Hãy để điều này được`minus`. 
3. Tính toán:$$|plus-minus|$$Giá trị này bằng giá trị tuyệt đối của tổng của toàn bộ mảng. 

1. Xuất kết quả. 

Lý do điều này có hiệu quả là vì mọi cặp cộng trừ phù hợp đều có thể được đặt bên trong một phân đoạn có tổng bằng 0 và không đóng góp gì. Chỉ có những ký hiệu thừa của dấu đa số là không thể tránh khỏi và số lượng của chúng chính xác là$|plus-minus|$. 

### Tại sao nó hoạt động 

Gọi tổng của các phân đoạn đã chọn là$s_1,s_2,\dots,s_k$. 

Mọi hình phạt phân đoạn đều thỏa mãn$$|s_i|\cdot \text{len}_i \ge |s_i|$$bởi vì mọi độ dài ít nhất là$1$. 

Do đó tổng số tiền phạt thỏa mãn$$\sum |s_i|\cdot \text{len}_i \ge \sum |s_i| \ge \left|\sum s_i\right|$$Đại lượng cuối cùng chính xác là giá trị tuyệt đối của tổng của cả mảng, cụ thể là$|P-M|$. 

Điều này chứng tỏ không có phân vùng nào có thể đạt được hình phạt nhỏ hơn$|P-M|$. 

Mặt khác, tất cả các cặp cộng-trừ phù hợp có thể được nhóm thành các phân đoạn có tổng bằng 0, để lại chính xác$|P-M|$những biểu tượng chưa từng có. Mỗi biểu tượng chưa từng có góp phần hình phạt$1$, do đó, một phân vùng có tổng số tiền phạt$|P-M|$tồn tại. 

Vì chúng ta có cả giới hạn dưới và cấu trúc phù hợp,$|P-M|$là mức hình phạt tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    s = input().strip()

    plus = s.count('+')
    minus = n - plus

    print(abs(plus - minus))
```Việc thực hiện trực tiếp theo kết quả toán học. 

Đầu tiên chúng ta đếm số lượng`'+'`nhân vật. Vì mọi nhân vật khác đều phải`'-'`, số điểm trừ chỉ đơn giản là`n - plus`. 

Câu trả lời là sự khác biệt tuyệt đối giữa những con số này. Không cần logic phân vùng vì bằng chứng cho thấy mọi phân vùng tối ưu đều có cùng giá trị tối thiểu, cụ thể là tổng tuyệt đối của toàn bộ mảng. 

Không có mối lo ngại tràn vì$n \le 5000$, vì vậy tất cả các giá trị đều rất nhỏ so với giới hạn số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
+-+-+-
```| Vị trí | Nhân vật | Cộng thêm | Số trừ | 
| --- | --- | --- | --- | 
| 1 | + | 1 | 0 | 
| 2 | - | 1 | 1 | 
| 3 | + | 2 | 1 | 
| 4 | - | 2 | 2 | 
| 5 | + | 3 | 2 | 
| 6 | - | 3 | 3 | 

Tính toán cuối cùng:$$|3-3|=0$$Trả lời:```
0
```Điều này chứng tỏ sự hủy bỏ hoàn toàn. Mọi điểm cộng đều có thể sánh với điểm trừ, nên toàn bộ hình phạt có thể giảm xuống bằng không. 

### Ví dụ 2 

đầu vào:```
--+++++++-
```| Vị trí | Nhân vật | Cộng thêm | Số trừ | 
| --- | --- | --- | --- | 
| 1 | - | 0 | 1 | 
| 2 | - | 0 | 2 | 
| 3 | + | 1 | 2 | 
| 4 | + | 2 | 2 | 
| 5 | + | 3 | 2 | 
| 6 | + | 4 | 2 | 
| 7 | + | 5 | 2 | 
| 8 | + | 6 | 2 | 
| 9 | + | 7 | 2 | 
| 10 | - | 7 | 3 | 

Tính toán cuối cùng:$$|7-3|=4$$Trả lời:```
4
```Có bốn điểm cộng nhiều hơn điểm trừ. Bốn điểm cộng vượt quá đó không thể bị hủy bỏ bởi bất kỳ phân vùng nào, vì vậy hình phạt tối thiểu chính xác là bốn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần đếm ký tự | 
| Không gian |$O(1)$| Chỉ có một số bộ đếm số nguyên được lưu trữ | 

Thuật toán xử lý mỗi ký tự đúng một lần. Ngay cả khi nhiều trường hợp kiểm thử được cung cấp, thời gian tuyến tính cho mỗi trường hợp kiểm thử dễ dàng đủ nhanh cho các giới hạn nhất định và mức sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        plus = s.count('+')
        minus = n - plus

        ans.append(str(abs(plus - minus)))

    return "\n".join(ans)

# provided sample
assert run(
"""5
1
+
5
-----
6
+-+-+-
10
--+++++++-
20
+---++++-+++++---++-
"""
) == "1\n5\n0\n4\n4"

# minimum size
assert run(
"""1
1
-
"""
) == "1"

# perfectly balanced
assert run(
"""1
2
+-
"""
) == "0"

# all equal
assert run(
"""1
5
+++++
"""
) == "5"

# odd length with one excess plus
assert run(
"""1
7
+-+-+++
"""
) == "3"

# another balanced arrangement
assert run(
"""1
8
++--++--
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`-`|`1`| Trường hợp thử nghiệm nhỏ nhất có thể | 
|`+-`|`0`| Hủy bỏ hoàn toàn | 
|`+++++`|`5`| Tất cả các giá trị giống nhau | 
|`+-+-+++`|`3`| Điểm cộng thừa vẫn còn sau khi khớp | 
|`++--++--`|`0`| Số lượng cân bằng bất kể thứ tự | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
1
+
```Chúng tôi có`plus = 1`Và`minus = 0`. Thuật toán trả về:$$|1-0|=1$$Chỉ có một phân đoạn, hình phạt cũng là$1$. Câu trả lời là đúng. 

Coi như:```
1
2
+-
```Số lượng là`plus = 1`Và`minus = 1`. Thuật toán trả về:$$|1-1|=0$$Thật vậy, lấy toàn bộ mảng làm một phân đoạn sẽ cho tổng$0$và hình phạt$0$. 

Coi như:```
1
3
+++
```Số lượng là`plus = 3`Và`minus = 0`. Thuật toán trả về:$$|3-0|=3$$Không có dấu trừ nào tồn tại để hủy bỏ bất kỳ dấu cộng nào. Hình phạt tốt nhất có thể đạt được là chính xác$3$, phù hợp với kết quả. 

Coi như:```
1
6
---+++
```Số lượng bằng nhau nên thuật toán trả về:$$|3-3|=0$$Mặc dù mọi điểm trừ đều có trước và mọi điểm cộng đều đến sau, nhưng thứ tự không thành vấn đề. Toàn bộ mảng có tổng$0$, do đó có thể đạt được hình phạt bằng 0. Trường hợp này xác nhận rằng chỉ có số lượng điểm cộng và điểm trừ mới quan trọng.
