---
title: "CF 77B - Đe Rơi"
description: "Chúng ta được cho hai tham số thực được chọn ngẫu nhiên thống nhất. Chiều cao nạn nhân p nằm trong khoảng [0, a] và tham số gió q nằm trong [-b, b]. Đối với mỗi cặp (p, q), cái đe chạm thành công nếu một phương trình bậc hai nhất định có ít nhất một nghiệm thực."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 77
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 69 (Div. 1 Only)"
rating: 1800
weight: 77
solve_time_s: 156
verified: true
draft: false
---

[CF 77B - Đe rơi](https://codeforces.com/problemset/problem/77/B) 

**Đánh giá:** 1800 
**Tags:** toán, xác suất 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai tham số thực được chọn ngẫu nhiên thống nhất. 

Chiều cao nạn nhân`p`nằm trong khoảng`[0, a]`, và thông số gió`q`nằm ở`[-b, b]`. Đối với mỗi cặp`(p, q)`, cái đe sẽ thành công nếu một phương trình bậc hai nào đó có ít nhất một nghiệm thực. 

Phương trình từ tuyên bố là:$x^2+px+q=0$Một phương trình bậc hai có nghiệm thực chính xác khi phân biệt của nó không âm. Vì vậy, toàn bộ vấn đề quy về việc tính xác suất:$p^2-4q\ge0$Từ`p`Và`q`được chọn thống nhất và độc lập, xác suất này đơn giản là diện tích của tất cả các giá trị hợp lệ`(p, q)`cặp chia cho tổng diện tích hình chữ nhật:$0\le p\le a,\ -b\le q\le b$Các ràng buộc là rất nhỏ từ góc độ thuật toán. Chúng tôi chỉ xử lý tối đa`10^4`trường hợp thử nghiệm và mỗi trường hợp chỉ chứa hai số nguyên. Bất kỳ công thức toán học có thời gian không đổi nào cho mỗi trường hợp kiểm thử đều đủ nhanh. Thậm chí một`O(log n)`hoặc`O(sqrt n)`giải pháp sẽ trôi qua một cách thoải mái. Thách thức thực sự là tìm ra hình học chính xác và xử lý các trường hợp biên một cách cẩn thận. 

Những trường hợp phức tạp xuất phát từ cách parabol tương tác với hình chữ nhật. 

Coi như:```
a = 1, b = 100
```parabol`q = p² / 4`vẫn rất thấp so với chiều cao hình chữ nhật. Hầu hết các điểm đều thất bại nên xác suất gần bằng`1/2`, không gần`1`. 

Việc thực hiện bất cẩn có thể cho rằng parabol luôn đạt đến ranh giới trên cùng`q = b`, ở đây sai. 

Một trường hợp cạnh quan trọng khác là:```
a = 0, b = 5
```Sau đó`p`luôn bằng 0 nên điều kiện trở thành:```
-4q >= 0
```có nghĩa là`q <= 0`. Đúng một nửa khoảng thời gian`[-5, 5]`hoạt động, vì vậy câu trả lời là`0.5`. 

Một dẫn xuất lỗi thường chia cho`a`ở đâu đó và gặp sự cố hoặc tạo ra NaN khi`a = 0`. 

Tình huống tinh tế cuối cùng là khi parabol chạm chính xác vào cạnh trên:```
a = 4, b = 4
```bởi vì`a² / 4 = 4`. 

Sự tích hợp thay đổi hành vi một cách chính xác tại ranh giới này. Sử dụng các bất đẳng thức nghiêm ngặt không đúng chỗ có thể gây ra các lỗi dấu phẩy động nhỏ hoặc chọn sai công thức. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Chúng ta có thể nghĩ về hình chữ nhật có thể có`(p, q)`cặp và kiểm tra xem mỗi điểm có thỏa mãn không:$q\le\frac{p^2}{4}$Nếu chúng ta rời rạc hóa hình chữ nhật thành một lưới rất mịn, đếm xem có bao nhiêu ô thỏa mãn bất đẳng thức và chia cho tổng số ô, chúng ta sẽ có được xác suất gần đúng. 

Điều này hoạt động về mặt khái niệm vì lựa chọn ngẫu nhiên thống nhất tương ứng trực tiếp với tỷ lệ diện tích. Vấn đề là độ chính xác. Để đạt được`10^-6`độ chính xác, lưới sẽ cần hàng triệu hoặc hàng tỷ mẫu. Vì`10^4`trường hợp thử nghiệm, điều đó trở nên hoàn toàn không thực tế. 

Quan sát quan trọng là vùng hợp lệ có hình dạng hình học rất đơn giản. 

Đối với mỗi cố định`p`, các giá trị hợp lệ của`q`là:$-b\le q\le\min\left(b,\frac{p^2}{4}\right)$Vì vậy, thay vì ước tính diện tích bằng số, chúng ta có thể lấy tích phân chính xác chiều cao của khoảng này. 

Hai trường hợp xuất hiện một cách tự nhiên. 

Nếu như:$\frac{a^2}{4}\le b$thì parabol không bao giờ đạt đến ranh giới trên cùng của hình chữ nhật. Chiều cao hợp lệ cho mỗi`p`là:$b+\frac{p^2}{4}$Tích hợp từ`0`ĐẾN`a`cung cấp khu vực trực tiếp. 

Nếu thay vào đó:$\frac{a^2}{4}>b$sau một thời điểm nào đó, parabol tăng lên phía trên hình chữ nhật. Từ thời điểm đó trở đi, mỗi`q`giá trị hoạt động. 

Quá trình chuyển đổi xảy ra vào lúc:$p=2\sqrt b$Điều này chia tích phân thành hai phần dễ dàng. 

Toàn bộ giải pháp trở thành thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K2) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ điều kiện phân biệt bậc hai. 

Phương trình có nghiệm thực khi:$p^2-4q\ge0$Sắp xếp lại mang lại:$q\le\frac{p^2}{4}$2. Giải thích xác suất về mặt hình học. 

Tất cả các cặp có thể`(p, q)`tạo thành một hình chữ nhật có diện tích:$2ab$Câu trả lời bằng: 

diện tích hợp lệ/tổng diện tích. 
3. Xác định khoảng dọc hợp lệ cho một khoảng thời gian cố định`p`. 

Từ`q`dao động từ`-b`ĐẾN`b`, và chúng tôi cũng cần:$q\le\frac{p^2}{4}$chiều cao hợp lệ trở thành:$\min\left(b,\frac{p^2}{4}\right)+b$4. Xử lý trường hợp đầu tiên khi parabol không bao giờ chạm tới cạnh trên. 

Nếu như:$a\le2\sqrt b$sau đó:$\frac{p^2}{4}\le b$cho mọi`p`TRONG`[0, a]`. 

Diện tích hợp lệ là:$\int_0^a\left(b+\frac{p^2}{4}\right)dp=ab+\frac{a^3}{12}$Chia cho`2ab`mang lại:$\frac12+\frac{a^2}{24b}$5. Xử lý trường hợp thứ hai khi parabol đi qua cạnh trên. 

Nếu như:$a>2\sqrt b$sau đó cho`p > 2√b`, mọi`q`giá trị hoạt động. 

Chia khu vực thành hai khu vực. 

Vùng đầu tiên, từ`0`ĐẾN`2√b`, đóng góp:$\int_0^{2\sqrt b}\left(b+\frac{p^2}{4}\right)dp=\frac{8}{3}b^{3/2}$Vùng thứ hai, từ`2√b`ĐẾN`a`, đóng góp chiều cao hình chữ nhật đầy đủ:$2b(a-2\sqrt b)$Tổng hợp và đơn giản hóa sản lượng:$1-\frac{2\sqrt b}{3a}$6. In công thức tương ứng với trường hợp hiện tại. 

### Tại sao nó hoạt động 

Thuật toán trực tiếp tính toán thước đo chính xác của tất cả các thành công`(p, q)`cặp bên trong hình chữ nhật lấy mẫu. Đối với mỗi cố định`p`, chúng tôi tính toán chính xác bao nhiêu khoảng dọc thỏa mãn điều kiện phân biệt. Tích hợp các độ dài hợp lệ đó trên tất cả`p`giá trị cho khu vực thành công chính xác. 

Việc chia thành hai trường hợp là đầy đủ vì parabol nằm dưới cạnh trên của hình chữ nhật trong toàn bộ khoảng hoặc cắt nó đúng một lần. Không có hành vi hình học nào khác có thể xảy ra. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

t = int(input())

for _ in range(t):
    a, b = map(float, input().split())

    if b == 0:
        print("1.0000000000")
        continue

    if a * a <= 4 * b:
        ans = 0.5 + (a * a) / (24.0 * b)
    else:
        ans = 1.0 - (2.0 * math.sqrt(b)) / (3.0 * a)

    print(f"{ans:.10f}")
```Việc triển khai tuân theo nguồn gốc chính xác từ hướng dẫn. 

Nhánh đầu tiên xử lý`b = 0`riêng. Trong hoàn cảnh đó,`q`luôn bằng 0, nên phương trình bậc hai luôn có nghiệm thực vì phân thức trở thành`p²`. Xác suất chính xác là`1`. 

Nhánh thứ hai kiểm tra xem parabol có đạt đến ranh giới trên cùng của hình chữ nhật hay không. Sử dụng:```
a * a <= 4 * b
```tránh các căn bậc hai không cần thiết và cũng tránh được các vấn đề nhỏ về độ chính xác của dấu phẩy động xung quanh đường biên. 

Các công thức được đánh giá trực tiếp dưới dạng biểu thức dấu phẩy động. Tất cả các giá trị trung gian vừa vặn thoải mái bên trong độ chính xác kép tiêu chuẩn vì`a`Và`b`nhiều nhất là`10^6`. 

Định dạng với mười chữ số thập phân dễ dàng đáp ứng dung sai chính xác cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 4
b = 2
```Từ:```
a² = 16
4b = 8
```chúng tôi sử dụng công thức thứ hai. 

| Biến | Giá trị | 
| --- | --- | 
| một | 4 | 
| b | 2 | 
| sqrt(b) | 1.414213562 | 
| 2 * sqrt(b) | 2.828427124 | 
| (2 * sqrt(b)) / (3a) | 0,235702260 | 
| Trả lời | 0,764297740 | 

Vì vậy, đầu ra là:```
0.7642977396
```Dấu vết này thể hiện trường hợp giao cắt trong đó một phần của hình chữ nhật hoàn toàn hợp lệ. 

### Ví dụ 2 

đầu vào:```
a = 1
b = 2
```Hiện nay:```
a² = 1
4b = 8
```nên parabol không bao giờ đạt đến cạnh trên. 

| Biến | Giá trị | 
| --- | --- | 
| một | 1 | 
| b | 2 | 
| a² / (24b) | 0,020833333 | 
| 0,5 | 0,500000000 | 
| Trả lời | 0,520833333 | 

Vì vậy, đầu ra là:```
0.5208333333
```Dấu vết này thể hiện trường hợp tích phân thuần túy trong đó ranh giới trên hợp lệ luôn là chính parabol. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Không sử dụng thêm dung lượng | 

Ngay cả đối với`10^4`các trường hợp thử nghiệm, thời gian chạy rất nhỏ vì mọi trường hợp đều được giải bằng công thức dạng đóng. Việc sử dụng bộ nhớ là không đổi trong suốt quá trình thực thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    t = int(input())

    out = []

    for _ in range(t):
        a, b = map(float, input().split())

        if b == 0:
            out.append("1.0000000000")
            continue

        if a * a <= 4 * b:
            ans = 0.5 + (a * a) / (24.0 * b)
        else:
            ans = 1.0 - (2.0 * math.sqrt(b)) / (3.0 * a)

        out.append(f"{ans:.10f}")

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run("2\n4 2\n1 2\n") == (
    "0.7642977396\n"
    "0.5208333333\n"
), "sample"

# minimum values
assert run("1\n0 0\n") == "1.0000000000\n", "both zero"

# boundary where a^2 = 4b
assert run("1\n4 4\n") == "0.6666666667\n", "boundary transition"

# very large values
assert run("1\n1000000 1\n") == "0.9999993333\n", "large a"

# a = 0 with positive b
assert run("1\n0 5\n") == "0.5000000000\n", "half interval valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`1.0`| Hình chữ nhật suy biến với thành công được đảm bảo | 
|`4 4`|`0.6666666667`| Điểm chuyển tiếp chính xác giữa các công thức | 
|`1000000 1`|`0.9999993333`| Giá trị lớn và độ ổn định chính xác | 
|`0 5`|`0.5`| Xử lý đúng khi chỉ q không dương mới có tác dụng | 

## Vỏ cạnh 

Hãy xem xét:```
1
0 5
```Đây`p`được cố định ở mức 0. Điều kiện trở thành:$-4q\ge0$có nghĩa là:$q\le0$Bên trong`[-5, 5]`, chính xác một nửa khoảng thời gian hoạt động. Thuật toán đi vào nhánh công thức đầu tiên vì:```
a² <= 4b
```và tính toán:```
0.5 + 0 / (24 * 5) = 0.5
```cái nào đúng. 

Bây giờ hãy xem xét:```
1
4 4
```Parabol chạm vào ranh giới trên cùng chính xác tại`p = 4`. Cả hai công thức dẫn xuất đều tạo ra cùng một giá trị:```
2 / 3
```Việc thực hiện sử dụng`<=`trong điều kiện rẽ nhánh, do đó ranh giới được xử lý nhất quán mà không bị gián đoạn. 

Cuối cùng, hãy xem xét:```
1
10 0
```Sau đó`q`luôn bằng 0 và mọi bậc hai đều có phân biệt:$p^2$luôn luôn không âm. Sự tận tâm`b == 0`lợi nhuận chi nhánh`1.0`ngay lập tức, tránh chia cho 0 trong các công thức.
