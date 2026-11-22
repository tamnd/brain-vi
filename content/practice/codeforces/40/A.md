---
title: "CF 40A - Tìm màu"
description: "Mặt phẳng được tô màu bằng cách sử dụng các vòng đồng tâm có tâm ở gốc tọa độ. Mỗi vòng giữa hai khoảng cách nguyên liên tiếp sẽ có màu xen kẽ. Bản thân các đường viền, nghĩa là tất cả các điểm có khoảng cách từ điểm gốc là số nguyên, luôn có màu đen."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 40
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 39"
rating: 1300
weight: 40
solve_time_s: 84
verified: true
draft: false
---
[CF 40A - Tìm màu](https://codeforces.com/problemset/problem/40/A) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, hình học, cách thực hiện, toán học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mặt phẳng được tô màu bằng cách sử dụng các vòng đồng tâm có tâm ở gốc tọa độ. Mỗi vòng giữa hai khoảng cách nguyên liên tiếp sẽ có màu xen kẽ. Bản thân các đường viền, nghĩa là tất cả các điểm có khoảng cách từ điểm gốc là số nguyên, luôn có màu đen. 

Vì một điểm`(x, y)`, chúng ta cần xác định xem điểm nằm trên ranh giới màu đen hay bên trong vùng màu trắng. Mô hình này thay đổi khi khoảng cách tăng dần tính từ điểm gốc. 

Đại lượng hình học quan trọng là khoảng cách Euclide từ gốc tọa độ:$d = \sqrt{x^2 + y^2}$Kéo các điểm để cập nhật khoảng cách giữa hai điểm.-10-8-6-4-2246810-10-5510A(6.0, 6.0)B(-6.0, -6.0)d = 16.97Delta x = 12Delta y = 12 

Nếu`d`là số nguyên thì đáp án có ngay`"black"`vì điểm nằm chính xác trên đường tròn. 

Mặt khác, màu sắc phụ thuộc vào hình tròn mà điểm đó thuộc về. Vùng giữa bán kính`0`Và`1`màu đen, giữa`1`Và`2`có màu trắng, giữa`2`Và`3`lại đen, vân vân. Nói cách khác, phần nguyên của khoảng cách xác định độ chẵn lẻ của màu. 

Giới hạn tọa độ rất nhỏ, chỉ tối đa`1000`về giá trị tuyệt đối. Ngay cả một giải pháp không hiệu quả cũng có thể chạy nhanh một cách thoải mái. Tuy nhiên, vấn đề chủ yếu là ở việc diễn giải chính xác hình học và xử lý các cạm bẫy của dấu phẩy động. 

Phần khó khăn là phát hiện xem khoảng cách có chính xác là số nguyên hay không. Việc triển khai bất cẩn bằng cách sử dụng so sánh dấu phẩy động có thể thất bại vì các giá trị như`sqrt(25)`nội bộ có thể trở thành`4.99999999997`hoặc`5.00000000001`. 

Hãy xem xét điểm:```
3 4
```Khoảng cách chính xác là`5`, vậy đáp án phải là`"black"`. Một so sánh dấu phẩy động như`sqrt(x*x+y*y) == int(sqrt(...))`là không an toàn. 

Một trường hợp tế nhị khác là nguồn gốc:```
0 0
```Khoảng cách của nó là`0`, là số nguyên nên điểm có màu đen. Một số triển khai vô tình phân loại nó bằng logic chẵn lẻ và tạo ra kết quả sai. 

Một trường hợp dễ bỏ sót nữa là điểm nằm ngay bên trong một ranh giới:```
2 2
```Khoảng cách là khoảng`2.828`, nằm giữa`2`Và`3`. Kể từ khoảng thời gian`[2,3)`tương ứng với vùng màu đen, câu trả lời là`"black"`. Sử dụng làm tròn thay vì làm sàn sẽ di chuyển nó vào vòng tiếp theo một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là tưởng tượng tất cả các đường tròn đồng tâm có bán kính nguyên. Chúng ta có thể tăng bán kính nhiều lần cho đến khi xác định được đường tròn đầu tiên bên ngoài điểm. Khi đã biết chỉ số vòng, chúng ta có thể xác định màu theo độ chẵn lẻ. 

Điều đó có tác dụng vì màu chỉ thay đổi khi đi qua một vòng tròn có bán kính nguyên. Vì tọa độ nhiều nhất là`1000`, khoảng cách tối đa có thể là khoảng`1414`, do đó, ngay cả việc quét tất cả bán kính cũng chỉ mất khoảng`1400`lần lặp lại. 

Tuy nhiên, hình học đưa ra một lối tắt trực tiếp. Thông tin duy nhất chúng tôi thực sự cần là: 

1. Khoảng cách có phải là số nguyên hay không. 
2. Ngược lại, tính chẵn lẻ của`floor(distance)`. 

Sự quan sát xuất phát từ cách tô màu xen kẽ từng vòng. Mỗi vòng`[k, k+1)`có một màu cố định được xác định hoàn toàn bởi`k`. 

Thay vì liên tục kiểm tra các vòng tròn, chúng ta có thể tính toán:$r^2 = x^2 + y^2⟦PROTECT_11⟧k⟦PROTECT_12⟧(x)^2 + (y)^2 = 3.0^2$Thanh trượt cập nhật tâm và bán kính của phương trình đường tròn.-10-8-6-4-2246810-6-4-2246 

Nếu`r²`là một hình vuông hoàn hảo, điểm có màu đen. 

Nếu không, hãy:$k = \lfloor \sqrt{x^2+y^2} \rfloor$Khi`k`chẵn thì vùng đó có màu đen. Khi`k`là số lẻ, vùng này có màu trắng. 

Điều này loại bỏ tất cả các phép lặp và tránh các vấn đề về độ chính xác của dấu phẩy động bằng cách sử dụng số học số nguyên bất cứ khi nào có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(R) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ`x`Và`y`. 
2. Tính bình phương khoảng cách từ gốc tọa độ:$s = x^2 + y^2$Sử dụng khoảng cách bình phương sẽ tránh được các thao tác dấu phẩy động không cần thiết. 
3. Tính căn bậc hai của số nguyên`s`. 

Cho phép`r = isqrt(s)`. 

Điều này cho số nguyên lớn nhất sao cho`r² ≤ s`. 
4. Kiểm tra xem`r * r == s`. 

Nếu đúng thì điểm nằm chính xác trên một đường tròn bán kính nguyên, do đó in`"black"`. 
5. Nếu không, hãy xác định chỉ số vòng bằng cách sử dụng`r`. 

Từ`r = floor(distance)`, điểm thuộc về hình khuyên`[r, r+1)`. 
6. Nếu`r`chẵn, in`"black"`. 

Nếu như`r`thật kỳ lạ, hãy in`"white"`. 

Các màu sắc xen kẽ bắt đầu từ tâm, trong đó khoảng cách bán kính`[0,1)`là màu đen. 

### Tại sao nó hoạt động 

Mỗi đường tròn có bán kính nguyên đóng vai trò là ranh giới giữa hai vùng. Khoảng thời gian`[0,1)`là màu đen,`[1,2)`là màu trắng,`[2,3)`lại đen và sự luân phiên tiếp tục mãi mãi. 

Đối với bất kỳ điểm nào không nằm trên đường biên, màu của nó chỉ phụ thuộc vào phần nguyên khoảng cách của nó đến gốc tọa độ. giá trị`floor(distance)`xác định duy nhất hình vành chứa điểm đó. Các chỉ số chẵn tương ứng với vùng màu đen, chỉ số lẻ tương ứng với vùng màu trắng. 

Việc kiểm tra xem khoảng cách bình phương có phải là bình phương hoàn hảo hay không sẽ phát hiện chính xác các điểm biên mà không có lỗi dấu phẩy động. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

x, y = map(int, input().split())

s = x * x + y * y
r = isqrt(s)

if r * r == s:
    print("black")
else:
    if r % 2 == 0:
        print("black")
    else:
        print("white")
```Chương trình bắt đầu bằng cách tính khoảng cách bình phương thay vì khoảng cách thực tế. Điều này an toàn hơn vì số học số nguyên là chính xác. 

chức năng`isqrt`trả về sàn của căn bậc hai bằng cách sử dụng các phép toán số nguyên thuần túy. Điều đó tránh được các vấn đề về độ chính xác có thể xuất hiện với`sqrt`. 

điều kiện`r * r == s`kiểm tra xem khoảng cách có phải là số nguyên chính xác hay không. Những điểm đó luôn có màu đen vì mọi đường tròn ranh giới đều có màu đen. 

Nếu điểm không nằm trên biên thì tính chẵn lẻ của`r`quyết định màu sắc của vòng. Vùng trung tâm bắt đầu có màu đen, vì vậy các vòng số chẵn có màu đen và các vòng số lẻ có màu trắng. 

Một lỗi phổ biến là sử dụng`round(sqrt(...))`thay vì`floor`. Các vòng được xác định theo khoảng thời gian, vì vậy sàn là hoạt động chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
-2 1
```| Bước | Giá trị | 
| --- | --- | 
|`x`|`-2`| 
|`y`|`1`| 
|`s = x² + y²`|`5`| 
|`r = isqrt(5)`|`2`| 
|`r*r == s`|`4 == 5`→ sai | 
|`r % 2`|`0`| 
| Trả lời |`black`| 

Điểm nằm giữa bán kính`2`Và`3`, đó là một chiếc nhẫn màu đen. 

### Ví dụ 2 

đầu vào:```
1 1
```| Bước | Giá trị | 
| --- | --- | 
|`x`|`1`| 
|`y`|`1`| 
|`s = x² + y²`|`2`| 
|`r = isqrt(2)`|`1`| 
|`r*r == s`|`1 == 2`→ sai | 
|`r % 2`|`1`| 
| Trả lời |`white`| 

Điểm này nằm bên trong hình khuyên`[1,2)`, màu trắng. 

### Ví dụ 3 

đầu vào:```
3 4
```| Bước | Giá trị | 
| --- | --- | 
|`x`|`3`| 
|`y`|`4`| 
|`s = x² + y²`|`25`| 
|`r = isqrt(25)`|`5`| 
|`r*r == s`|`25 == 25`→ đúng | 
| Trả lời |`black`| 

Ví dụ này thể hiện quy tắc ranh giới. Điểm nằm chính xác trên đường tròn bán kính`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào | 

Các ràng buộc là cực kỳ nhỏ nên giải pháp này dễ dàng nằm gọn trong giới hạn. Thậm chí hàng nghìn trường hợp thử nghiệm sẽ chạy ngay lập tức. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from math import isqrt

def solve():
    input = sys.stdin.readline

    x, y = map(int, input().split())

    s = x * x + y * y
    r = isqrt(s)

    if r * r == s:
        print("black")
    else:
        print("black" if r % 2 == 0 else "white")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("-2 1\n") == "black", "sample 1"

# custom cases
assert run("0 0\n") == "black", "origin is on integer-radius boundary"
assert run("1 1\n") == "white", "inside white ring"
assert run("3 4\n") == "black", "perfect-square distance"
assert run("2 2\n") == "black", "inside black ring"
assert run("1000 1000\n") == "black", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`black`| Xử lý nguồn gốc | 
|`1 1`|`white`| Nhẫn có chỉ số lẻ | 
|`3 4`|`black`| Khoảng cách nguyên chính xác | 
|`2 2`|`black`| Làm sàn thay vì làm tròn | 
|`1000 1000`|`black`| Giá trị tọa độ lớn | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là một điểm chính xác trên đường tròn biên. 

đầu vào:```
3 4
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
|`s`|`25`| 
|`r`|`5`| 
|`r*r == s`| đúng | 

Vì khoảng cách bình phương là một bình phương hoàn hảo nên thuật toán sẽ in ngay`"black"`. Điều này xử lý chính xác quy tắc mọi ranh giới đều có màu đen. 

Trường hợp cạnh thứ hai là nguồn gốc. 

đầu vào:```
0 0
```Việc thực hiện trở thành: 

| Biến | Giá trị | 
| --- | --- | 
|`s`|`0`| 
|`r`|`0`| 
|`r*r == s`| đúng | 

Thuật toán lại quay trở lại`"black"`vì gốc tọa độ nằm trên ranh giới bán kính-0. 

Trường hợp tinh tế thứ ba là một điểm gần đường biên nhưng không nằm chính xác trên đó. 

đầu vào:```
2 2
```Các giá trị là: 

| Biến | Giá trị | 
| --- | --- | 
|`s`|`8`| 
|`r`|`2`| 
|`r*r == s`| sai | 
|`r % 2`|`0`| 

Điểm nằm trong khoảng`[2,3)`, màu đen. sử dụng`floor(distance)`bởi vì`isqrt`tạo ra chỉ số vòng chính xác. Việc triển khai có lỗi khi sử dụng tính năng làm tròn có thể phân loại không chính xác điểm này thuộc về vòng tiếp theo.
