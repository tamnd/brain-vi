---
title: "CF 1388B - Thuyền trưởng Flint và chuyến hành trình dài"
description: "Chúng ta cần xây dựng một số thập phân $n$-chữ số $x$. Lấy mọi chữ số của $x$, viết biểu diễn nhị phân của nó mà không có số 0 đứng đầu và nối các chuỗi nhị phân đó. Chuỗi nhị phân kết quả được gọi là $k$. Sau đó, $n$ bit cuối cùng của $k$ sẽ bị xóa."
date: "2026-06-11T10:34:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1388
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 660 (Div. 2)"
rating: 1000
weight: 1388
solve_time_s: 132
verified: true
draft: false
---

[CF 1388B - Thuyền trưởng Flint và chuyến hành trình dài](https://codeforces.com/problemset/problem/1388/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một$n$-số thập phân có chữ số$x$. 

Lấy từng chữ số của$x$, viết biểu diễn nhị phân của nó mà không có số 0 đứng đầu và nối các chuỗi nhị phân đó. Chuỗi nhị phân kết quả được gọi$k$. 

Sau đó, lần cuối cùng$n$bit của$k$được gỡ bỏ. Tiền tố còn lại được hiểu là số nhị phân$r$. 

Mục tiêu của chúng tôi là lựa chọn$x$để có thể$r$là càng lớn càng tốt. Nếu nhiều khác nhau$x$tạo ra mức tối đa như nhau$r$, chúng ta phải xuất ra giá trị nhỏ nhất như vậy$x$. 

Đầu vào chỉ bao gồm độ dài$n$. Chúng ta không bao giờ cần phải xây dựng lại$k$rõ ràng cho các giá trị lớn. Tổng của tất cả$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, do đó, bất kỳ giải pháp nào xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính đều đủ nhanh. Bất cứ điều gì theo cấp số nhân trong$n$, hoặc bất cứ thứ gì cố gắng tìm kiếm trên tất cả$n$-số chữ số, là hoàn toàn không thể. 

Phần khó khăn là hiểu những gì loại bỏ cuối cùng$n$bit thực sự có nghĩa là. Giá trị của$r$chỉ phụ thuộc vào tiền tố của chuỗi nhị phân được nối còn tồn tại sau khi bị cắt bớt. Vì các số nhị phân được so sánh theo từ điển theo độ dài và sau đó theo bit, nên chúng tôi muốn tiền tố còn sót lại đó càng lớn càng tốt. 

Một số trường hợp đặc biệt rất dễ bị hiểu lầm. 

Vì$n=1$, câu trả lời là không$9$. Nếu như$x=9$, sau đó$k=1001$. Loại bỏ những lá cuối cùng$100$. Nếu như$x=8$, sau đó$k=1000$, và loại bỏ lá bit cuối cùng$100$cũng vậy. Cả hai đều cho mức tối đa như nhau$r$, Nhưng$8$nhỏ hơn nên đáp án đúng là:```
Input:
1

Output:
8
```Một trường hợp tế nhị khác là$n=2$. Một ý tưởng ngây thơ có thể là tối đa hóa mọi chữ số và đầu ra$99$. Sau đó:$$k = 1001\,1001$$Loại bỏ hai bit cuối cùng để lại$100110$. 

Vì$98$:$$k = 1001\,1000$$Loại bỏ hai bit cuối cùng cũng để lại$100110$. 

Vì cả hai đều đạt được mức tối đa như nhau$r$, ta phải chọn số nhỏ hơn, cụ thể là$98$. 

Cạm bẫy cuối cùng là giả định rằng chỉ có chữ số cuối cùng mới quan trọng. Vì$n=3$, câu trả lời là$998$, không$989$. Vị trí của các chữ số xác định bit nào còn tồn tại sau khi cắt bớt và cấu trúc tối ưu hóa ra lại yêu cầu vài số 8 ở cuối. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Hãy liệt kê từng$n$-số có chữ số$x$, xây dựng chuỗi nhị phân$k$, xóa phần cuối cùng của nó$n$bit, tính toán$r$và giữ lại ứng viên tốt nhất. 

Điều này đúng vì nó kiểm tra mọi khả năng. Thật không may, ngay cả đối với$n=10$, có$9 \cdot 10^9$ứng viên. Những ràng buộc thực tế cho phép$n$lên đến$10^5$, vì vậy việc tìm kiếm toàn diện là không khả thi. 

Để tìm ra mô hình, chúng ta cần hiểu làm thế nào$r$được hình thành. 

chữ số$8$Và$9$đặc biệt vì chúng có biểu diễn nhị phân dài nhất trong số các chữ số thập phân:$$8 = 1000,\qquad 9 = 1001.$$Cả hai đều đóng góp bốn bit. Mỗi chữ số khác đóng góp tối đa ba bit. 

Độ dài của tiền tố còn sót lại$r$là$$|k| - n.$$Để tối đa hóa số nhị phân, ưu tiên hàng đầu là tối đa hóa độ dài của nó. Vì mỗi chữ số đóng góp nhiều nhất là bốn bit nên chúng ta nên làm cho mỗi chữ số đóng góp bốn bit bất cứ khi nào có thể. Điều đó có nghĩa là mỗi chữ số phải là$8$hoặc$9$. 

Bây giờ tất cả các ứng cử viên có tổng chiều dài nhị phân như nhau$4n$, do đó mọi kết quả$r$có chiều dài$3n$. Sự so sánh trở thành từ điển học. 

Quan sát rằng các chuỗi nhị phân của$8$Và$9$chỉ khác nhau ở bit cuối cùng:$$1000,\qquad 1001.$$cuối cùng$n$các bit của phép nối sẽ bị loại bỏ. Bất kỳ bit nào nằm hoàn toàn bên trong hậu tố bị loại bỏ đều không có tác dụng đối với$r$. 

Giả sử chúng ta đặt một$8$gần cuối. Bit cuối cùng của nó là$0$thay vì$1$được cung cấp bởi một$9$. Nếu bit khác biệt đó bị loại bỏ, việc thay đổi một$9$đến một$8$không ảnh hưởng$r$, trong khi làm$x$nhỏ hơn. 

Do đó, chiến lược tối ưu là: 

1. Chỉ sử dụng chữ số$8$Và$9$. 
2. Giữ càng nhiều chữ số đầu càng tốt bằng$9$, bởi vì các bit khác nhau của chúng tồn tại bên trong$r$. 
3. Thay thế hậu tố lớn nhất có thể bằng$8$, bởi vì các bit khác nhau của các chữ số đó nằm bên trong phần bị loại bỏ. 

Có bao nhiêu chữ số cuối có thể được thay đổi thành$8$? 

Mỗi chữ số đóng góp bốn bit. Hậu tố bị loại bỏ có độ dài$n$. Bit phân biệt của một chữ số là bit thứ tư của nó. Chúng ta có thể ẩn bit đó một cách an toàn bên trong hậu tố đã bị xóa một cách chính xác$$\left\lceil \frac{n}{4} \right\rceil$$chữ số ở cuối. 

Như vậy câu trả lời là:$$\underbrace{99\ldots9}_{n-\lceil n/4\rceil} \underbrace{88\ldots8}_{\lceil n/4\rceil}.$$Đây là giải pháp Codeforces nổi tiếng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$n$| Hàm mũ | Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$. 
2. Tính toán$$m = \left\lceil \frac{n}{4} \right\rceil.$$Đây là số chữ số cuối có thể thay đổi từ$9$ĐẾN$8$không ảnh hưởng đến giá trị lớn nhất của$r$. 

1. Đầu tiên$n-m$vị trí phải là chữ số$9$. 

Các chữ số này đóng góp các bit còn lại bên trong tiền tố còn lại, do đó làm cho chúng$9$cung cấp từ điển lớn nhất có thể$r$. 
2. Cuối cùng$m$vị trí phải là chữ số$8$. 

Các bit phân biệt của chúng nằm bên trong hậu tố bị loại bỏ, do đó việc thay thế$9$qua$8$giữ$r$không thay đổi trong khi thực hiện$x$nhỏ hơn. 
3. Xuất chuỗi kết quả. 

### Tại sao nó hoạt động 

Bất kỳ chữ số nào khác ngoài$8$hoặc$9$đóng góp ít hơn bốn bit, làm giảm tổng chiều dài của$k$. Từ$r$có chiều dài$|k|-n$, điều này ngay lập tức tạo ra số nhị phân ngắn hơn và không thể tối ưu. 

Sau khi giới hạn bản thân ở các chữ số$8$Và$9$, mỗi chữ số đóng góp chính xác bốn bit. Sự khác biệt duy nhất giữa chúng là bit cuối cùng trong biểu diễn nhị phân của chúng. cuối cùng$n$các bit của phép nối bị loại bỏ, do đó các bit phân biệt của chuỗi cuối cùng$\lceil n/4\rceil$chữ số không bao giờ xuất hiện trong$r$. Những chữ số đó có thể được thay đổi từ$9$ĐẾN$8$mà không ảnh hưởng$r$. 

Bất kỳ chữ số nào trước đó vẫn ảnh hưởng$r$. Thay đổi một chữ số như vậy từ$9$ĐẾN$8$thay thế một bit còn sót lại$1$qua$0$, làm$r$nhỏ hơn. Do đó tất cả các vị trí trước đó phải được giữ nguyên$9$. 

Trong số tất cả các số tạo ra giá trị lớn nhất có thể$r$, cách xây dựng này giảm thiểu$x$bởi vì nó thay thế mọi dấu vết có thể$9$qua$8$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
ans = []

for _ in range(t):
    n = int(input())
    eights = (n + 3) // 4
    nines = n - eights
    ans.append("9" * nines + "8" * eights)

print("\n".join(ans))
```Việc thực hiện trực tiếp theo đặc tính toán học. 

biểu hiện`(n + 3) // 4`tính toán$\lceil n/4 \rceil$sử dụng số học số nguyên. Đây là chi tiết không tầm thường duy nhất. 

Phần đầu tiên của câu trả lời chứa`n - eights`bản sao của`'9'`. Phần thứ hai chứa`eights`bản sao của`'8'`. 

Không có chuỗi nhị phân nào được xây dựng. Giải pháp chỉ xây dựng câu trả lời thập phân cuối cùng, đó là lý do tại sao nó vẫn tuyến tính ở kích thước đầu ra. 

Một lỗi phổ biến là sử dụng`n // 4`thay vì chia trần. Ví dụ, khi`n = 1`, phép chia sàn cho`0`, sản xuất`"9"`thay vì câu trả lời đúng`"8"`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 1
```| Bước | Giá trị | 
| --- | --- | 
| n | 1 | 
| số tám | (1 + 3) // 4 = 1 | 
| số chín | 0 | 
| trả lời | 8 | 

Đầu ra:```
8
```Ví dụ này cho thấy tại sao cần phải phân chia trần. Toàn bộ chữ số thuộc vùng di động nên lựa chọn tối ưu nhỏ nhất là`8`. 

### Ví dụ 2 

đầu vào:```
n = 3
```| Bước | Giá trị | 
| --- | --- | 
| n | 3 | 
| số tám | (3 + 3) // 4 = 1 | 
| số chín | 2 | 
| trả lời | 998 | 

Đầu ra:```
998
```Đây là mẫu từ tuyên bố. Chỉ có thể thay đổi chữ số cuối cùng thành`8`không làm giảm giá trị tối ưu của$r$. 

### Ví dụ 3 

đầu vào:```
n = 7
```| Bước | Giá trị | 
| --- | --- | 
| n | 7 | 
| số tám | (7 + 3) // 4 = 2 | 
| số chín | 5 | 
| trả lời | 9999988 | 

Đầu ra:```
9999988
```Ví dụ này minh họa rằng hậu tố của`8`s tăng trưởng ở mỗi bốn vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Việc xây dựng chuỗi đầu ra yêu cầu$n$nhân vật | 
| Không gian |$O(n)$| Bản thân câu trả lời được tạo ra có chứa$n$nhân vật | 

Tổng của tất cả$n$giá trị nhiều nhất là$2 \cdot 10^5$, do đó tổng lượng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính theo tổng kích thước đầu ra. Điều này là thoải mái trong giới hạn. 

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
        eights = (n + 3) // 4
        ans.append("9" * (n - eights) + "8" * eights)

    return "\n".join(ans)

# provided sample
assert run("2\n1\n3\n") == "8\n998", "sample"

# minimum size
assert run("1\n1\n") == "8", "n = 1"

# boundary around multiple of four
assert run("1\n4\n") == "9998", "exact multiple of four"

# just after multiple of four
assert run("1\n5\n") == "99988", "ceiling division"

# larger case
assert run("1\n8\n") == "99999988", "two trailing eights"

# maximum style stress case
n = 100000
expected = "9" * 75000 + "8" * 25000
assert run(f"1\n{n}\n") == expected, "maximum length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`8`| Đầu vào nhỏ nhất có thể | 
|`n=4`|`9998`| Chính xác là bội số của bốn | 
|`n=5`|`99988`| Phân chia trần đúng cách | 
|`n=8`|`99999988`| Hai số tám kéo dài | 
|`n=100000`| 75000 số chín tiếp theo là 25000 số tám | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất:```
1
1
```Thuật toán tính toán:$$\left\lceil \frac{1}{4} \right\rceil = 1.$$Vậy câu trả lời là`8`. Sử dụng cách chia tầng sẽ tạo ra sai số`9`. Việc xây dựng xử lý ranh giới này một cách chính xác. 

Coi như:```
1
4
```Thuật toán tính toán:$$\left\lceil \frac{4}{4} \right\rceil = 1.$$Đầu ra là:```
9998
```Chỉ có chữ số cuối cùng thuộc về vùng có thể tháo rời. Bất kỳ sự thay thế nào trước đó bởi`8`sẽ làm giảm giá trị kết quả của$r$. 

Coi như:```
1
5
```Thuật toán tính toán:$$\left\lceil \frac{5}{4} \right\rceil = 2.$$Đầu ra là:```
99988
```Trường hợp này bắt gặp lỗi từng lỗi phổ biến nhất. Sử dụng phép chia sàn sẽ chỉ tạo ra một dấu vết`8`, đây không phải là câu trả lời hợp lệ tối thiểu trong số tất cả các công trình tối ưu. 

Cuối cùng, đối với một giá trị rất lớn như:```
1
100000
```thuật toán không bao giờ xây dựng các biểu diễn nhị phân và không bao giờ thực hiện bất kỳ phép tính tốn kém nào. Nó chỉ đơn giản xuất ra 75.000 số chín, sau đó là 25.000 số tám, vẫn giữ nguyên kích thước tuyến tính ở đầu ra.
