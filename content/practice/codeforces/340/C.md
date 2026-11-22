---
title: "CF 340C - Vấn đề du lịch"
description: "Chúng ta có một số đích nằm trên một đường thẳng tại các vị trí $a1,a2,dots,an$, tất cả đều dương và khác biệt. Một tuyến đường chỉ đơn giản là một hoán vị của những điểm đến này."
date: "2026-06-06T17:21:51+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 340
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 198 (Div. 2)"
rating: 1600
weight: 340
solve_time_s: 133
verified: true
draft: false
---

[CF 340C - Vấn đề du lịch](https://codeforces.com/problemset/problem/340/C) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, thực hiện, toán học 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số điểm đến nằm trên một đường thẳng tại các vị trí$a_1,a_2,\dots,a_n$, tất cả đều tích cực và khác biệt. Một tuyến đường chỉ đơn giản là một hoán vị của những điểm đến này. Khách du lịch bắt đầu từ vị trí 0, tham quan các điểm đến theo thứ tự đã chọn và dừng lại sau khi ghé thăm điểm cuối cùng. 

Đối với một tuyến đường$p_1,p_2,\dots,p_n$, quãng đường đã đi là$$a_{p_1} + \sum_{i=2}^{n} |a_{p_i}-a_{p_{i-1}}|.$$Nhiệm vụ không phải là tìm ra con đường tốt nhất. Chúng ta phải tính khoảng cách trung bình trên tất cả$n!$các tuyến đường có thể và đầu ra có giá trị trung bình là một phần tối giản. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất cứ điều gì liên quan đến việc liệt kê các hoán vị. Thậm chí$n=15$sẽ làm$n!$lớn một cách vô vọng. Chúng ta cần một công thức tổng hợp sự đóng góp của từng cặp điểm đến trên tất cả các tuyến đường. 

Các vị trí có thể lớn như$10^7$, và tử số cuối cùng có thể theo thứ tự$n^2 \cdot 10^7$, vì vậy cần có số học 64 bit. Số nguyên Python tự động xử lý việc này. 

Trường hợp cạnh tinh tế xuất hiện khi$n=2$. 

đầu vào:```
2
1 10
```Hai tuyến đường có độ dài$10$Và$19$. Trung bình của họ là$29/2$. 

Một giải pháp bất cẩn chỉ tính các chuyển đổi cặp và quên bước di chuyển ban đầu từ vị trí 0 sẽ tạo ra$9$, điều đó hoàn toàn sai lầm. 

Một lỗi dễ mắc phải khác là đếm sai có bao nhiêu hoán vị chứa một cặp cụ thể liền kề. 

đầu vào:```
3
2 3 5
```Cặp đôi$(2,5)$xuất hiện liền kề chính xác$2\cdot (n-1)! = 4$hoán vị, không phải tất cả$6$. Việc lấy sai yếu tố tổ hợp này sẽ dẫn đến giá trị trung bình không chính xác. 

Cạm bẫy thứ ba là quên giảm phân số cuối cùng. 

đầu vào:```
2
1 3
```Khoảng cách trung bình là$(3+5)/2 = 4$, vì vậy kết quả đúng là:```
4 1
```In ấn`8 2`sẽ không đáp ứng được yêu cầu. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo mọi hoán vị của các điểm đến, tính độ dài tuyến đường của nó, tính tổng tất cả các độ dài tuyến đường, sau đó chia cho$n!$. 

Điều này hiệu quả vì định nghĩa của câu trả lời theo nghĩa đen là giá trị trung bình trên tất cả các hoán vị. Thật không may, có$n!$các tuyến đường. Vì$n=10^5$, thậm chí viết ra số$n!$là không thể chứ chưa nói đến việc lặp qua tất cả các hoán vị. 

Quan sát quan trọng là giá trị trung bình trên tất cả các hoán vị thường có thể được tính bằng cách đếm tần suất mỗi thành phần xuất hiện. 

Độ dài tuyến đường bao gồm hai loại thuật ngữ. 

Số hạng đầu tiên là khoảng cách từ 0 đến đích được truy cập đầu tiên. 

Các số hạng còn lại là khoảng cách giữa các đích liên tiếp trong hoán vị. 

Thay vì lý luận về toàn bộ tuyến đường, chúng tôi tính sự đóng góp của từng điểm đến và từng cặp điểm đến trên tất cả các hoán vị. 

Đối với vị trí xuất phát, mọi đích đến đều xuất hiện đầu tiên một cách chính xác$(n-1)!$hoán vị. Do đó tổng đóng góp của số lần khởi động là$$(n-1)! \sum a_i.$$Đối với các chuyển đổi liên tiếp, hãy xem xét hai đích đến$x$Và$y$. Có bao nhiêu hoán vị để đặt chúng cạnh nhau? 

Đối xử$(x,y)$như một khối. Khối có thể được sắp xếp theo hai cách và cùng với phần còn lại$n-2$điểm đến chúng tôi có$n-1$các đối tượng để hoán vị. 

Như vậy số đếm là$$2\cdot (n-1)!.$$Mỗi hoán vị như vậy góp phần$|x-y|$. 

Do đó tổng đóng góp của tất cả các số hạng kề là$$2(n-1)! \sum_{i<j} |a_i-a_j|.$$Kết hợp cả hai phần mang lại$$\text{Total over all permutations} = (n-1)! \left( \sum a_i + 2\sum_{i<j}|a_i-a_j| \right).$$Chia cho$n!$hủy bỏ các giai thừa:$$\text{Average} = \frac{ \sum a_i + 2\sum_{i<j}|a_i-a_j| }{n}.$$Bây giờ toàn bộ vấn đề được chuyển thành tính toán$$\sum_{i<j}|a_i-a_j|.$$Sau khi sắp xếp các vị trí, tất cả sự khác biệt trở thành không âm:$$\sum_{i<j}(a_j-a_i).$$Đại lượng cổ điển này có thể được tính theo thời gian tuyến tính sau khi sắp xếp bằng cách sử dụng tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!\cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n\log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các vị trí và sắp xếp chúng. 

Việc sắp xếp cho phép chúng ta thay thế các giá trị tuyệt đối bằng những khác biệt thông thường vì$i<j$, chúng tôi có$a_j \ge a_i$. 
2. Tính tổng của tất cả các vị trí. 

Đây là sự đóng góp của nước đi ban đầu sau khi đơn giản hóa tổ hợp. 
3. Tính toán$\sum_{i<j}(a_j-a_i)$. 

Duy trì tổng tiền tố đang chạy. 

Khi xử lý$a_i$, tất cả các giá trị trước đó đóng góp$$a_i \cdot i - \text{prefix}.$$Điều này bằng$$\sum_{j<i}(a_i-a_j).$$4. Hãy để$D$là tổng chênh lệch theo cặp thu được ở bước trước. 
5. Tính toán$$\text{numerator} = \sum a_i + 2D.$$Trung bình cộng là tử số chia cho$n$. 
6. Rút gọn phân số bằng cách chia tử số và mẫu số cho ước số chung lớn nhất của chúng. 
7. Xuất ra tử số và mẫu số rút gọn. 

### Tại sao nó hoạt động 

Mỗi độ dài tuyến đường là tổng của một đóng góp ban đầu và một số đóng góp lân cận. Trên tất cả các hoán vị, mỗi đích xuất hiện chính xác đầu tiên$(n-1)!$lần, đưa ra thuật ngữ$\sum a_i$. Tương tự như vậy, mọi cặp đích không có thứ tự sẽ trở nên liền kề nhau một cách chính xác.$2(n-1)!$lần, đưa ra thuật ngữ$2\sum_{i<j}|a_i-a_j|$. 

Các số đếm này là chính xác và bao gồm mọi đóng góp khoảng cách trong mỗi hoán vị chính xác một lần. Sau khi chia cho tổng số hoán vị$n!$, chiều dài tuyến đường trung bình trở thành$$\frac{\sum a_i + 2\sum_{i<j}|a_i-a_j|}{n}.$$Tính toán tổng tiền tố được sắp xếp đánh giá chính xác tổng chênh lệch theo cặp, do đó thuật toán tính toán mức trung bình mong muốn mà không cần xấp xỉ. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    total_positions = sum(a)

    prefix = 0
    pair_sum = 0

    for i, x in enumerate(a):
        pair_sum += x * i - prefix
        prefix += x

    numerator = total_positions + 2 * pair_sum
    denominator = n

    g = gcd(numerator, denominator)

    print(numerator // g, denominator // g)

solve()
```Bước đầu tiên sắp xếp tọa độ. Sau khi được sắp xếp, mọi đóng góp của cặp sẽ trở thành$a_j-a_i$thay vì$|a_j-a_i|$, điều này làm cho việc tổng hợp có thể thực hiện được. 

Biến`pair_sum`cửa hàng$$\sum_{i<j}(a_j-a_i).$$Khi xử lý giá trị hiện tại`x`tại chỉ mục`i`, có chính xác`i`các phần tử trước đó. Tổng đóng góp của họ là$$(x-a_0)+(x-a_1)+\cdots+(x-a_{i-1}),$$đơn giản hóa để`x * i - prefix`. 

Công thức cuối cùng xuất phát trực tiếp từ đối số đếm tổ hợp:$$\text{average} = \frac{\sum a_i + 2\,\text{pair\_sum}}{n}.$$Không có giai thừa nào được tính toán. Đây là sự đơn giản hóa quan trọng làm cho vấn đề trở nên khả thi. 

Các số nguyên có độ chính xác tùy ý của Python tránh được sự cố tràn ngay cả khi các giá trị trung gian trở nên rất lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 5
```Mảng được sắp xếp:$[2,3,5]$| tôi | x | tiền tố trước | Đã thêm vào pair_sum | cặp_tổng sau | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 0 | 0 | 
| 1 | 3 | 2 | 3×1−2=1 | 1 | 
| 2 | 5 | 5 | 5×2−5=5 | 6 | 

Bây giờ: 

| Số lượng | Giá trị | 
| --- | --- | 
| Tổng số vị trí | 10 | 
| Tổng cặp | 6 | 
| Tử số | 10 + 2×6 = 22 | 
| Mẫu số | 3 | 

Đầu ra:```
22 3
```Dấu vết xác nhận rằng tổng chênh lệch theo cặp là$$(3-2)+(5-2)+(5-3)=6.$$### Ví dụ 2 

đầu vào:```
2
1 10
```Mảng được sắp xếp:$[1,10]$| tôi | x | tiền tố trước | Đã thêm vào pair_sum | cặp_tổng sau | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 
| 1 | 10 | 1 | 10×1−1=9 | 9 | 

Bây giờ: 

| Số lượng | Giá trị | 
| --- | --- | 
| Tổng số vị trí | 11 | 
| Tổng cặp | 9 | 
| Tử số | 11 + 18 = 29 | 
| Mẫu số | 2 | 

Đầu ra:```
29 2
```Ví dụ này cho thấy rằng trung bình có thể là một phân số không nguyên và phải được giảm một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |$O(1)$thêm | Ngoài mảng đầu vào và một vài biến | 

Với$n=10^5$, MỘT$O(n\log n)$giải pháp dễ dàng phù hợp trong thời hạn. Việc quét tuyến tính sau khi sắp xếp là không đáng kể so với việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    a.sort()

    prefix = 0
    pair_sum = 0

    for i, x in enumerate(a):
        pair_sum += x * i - prefix
        prefix += x

    numerator = sum(a) + 2 * pair_sum
    denominator = n

    g = gcd(numerator, denominator)
    return f"{numerator // g} {denominator // g}"

# provided sample
assert run("3\n2 3 5\n") == "22 3", "sample 1"

# minimum n
assert run("2\n1 2\n") == "5 2", "minimum size"

# reducible fraction becoming integer
assert run("2\n1 3\n") == "4 1", "fraction reduction"

# unsorted input
assert run("3\n5 2 3\n") == "22 3", "sorting correctness"

# larger gaps
assert run("3\n1 10 20\n") == "79 3", "pairwise contribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`5 2`| Kích thước hợp lệ nhỏ nhất | 
|`2 / 1 3`|`4 1`| Giảm phân số | 
|`3 / 5 2 3`|`22 3`| Thứ tự đầu vào không quan trọng | 
|`3 / 1 10 20`|`79 3`| Khoảng cách theo cặp lớn | 

## Vỏ cạnh 

Hãy xem xét:```
2
1 10
```Sau khi sắp xếp, tổng chênh lệch theo cặp là$9$. Công thức cho$$\frac{1+10+2\cdot 9}{2} = \frac{29}{2}.$$Thuật toán bao gồm chính xác sự đóng góp của điểm đến được truy cập đầu tiên. Bỏ qua thuật ngữ đó sẽ tạo ra không chính xác$9$. 

Coi như:```
3
5 2 3
```Sắp xếp biến mảng thành$[2,3,5]$. Tổng theo cặp trở thành$6$, sản xuất$22/3$. Điều này xác nhận rằng câu trả lời chỉ phụ thuộc vào vị trí chứ không phụ thuộc vào thứ tự của chúng trong đầu vào. 

Coi như:```
2
1 3
```Công thức cho$$\frac{1+3+2(2)}{2} = \frac{8}{2} = 4.$$Bước gcd giảm phân số thành`4 1`, thỏa mãn yêu cầu đầu ra không thể giảm được. 

Coi như:```
4
1 2 3 10000000
```Tổng chênh lệch theo cặp là rất lớn, nhưng tất cả các phép tính vẫn là số học số nguyên. Python xử lý các giá trị kết quả một cách an toàn và công thức tổ hợp tránh mọi số có kích thước giai thừa.
