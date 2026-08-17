---
title: "CF 102279I - Bắt chước khoai tây"
description: "Chúng ta có N ngăn xếp khác trống, trong đó ngăn xếp i ban đầu chứa các lá bài Ai. Hai người chơi luân phiên di chuyển, Lowie đi trước. Một nước đi có đúng một trong hai hình thức."
date: "2026-08-16T19:30:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "I"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 390
verified: true
draft: false
---

[CF 102279I - Bắt chước khoai tây](https://codeforces.com/problemset/problem/102279/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`N`ngăn xếp không trống, ngăn xếp ở đâu`i`ban đầu chứa`Ai`thẻ. Hai người chơi luân phiên di chuyển, Lowie đi trước. Một nước đi có đúng một trong hai hình thức. Người chơi có thể loại bỏ một thẻ khỏi một ngăn xếp không trống hoặc nếu mọi ngăn xếp đều không trống, hãy loại bỏ đồng thời một thẻ khỏi mỗi ngăn xếp. Ai loại bỏ lá bài cuối cùng sẽ thắng. 

Mục đích không phải là mô phỏng trò chơi. Chúng ta cần xác định xem vị trí ban đầu có giành chiến thắng cho người chơi đầu tiên hay không,`lowie`, hoặc thua người chơi đầu tiên, có nghĩa là`imitater`có thể buộc phải giành chiến thắng. 

Các ràng buộc đủ nhỏ để quét tuyến tính nhưng quá lớn để liệt kê trạng thái trò chơi. Có thể có 1000 ngăn xếp và mỗi ngăn xếp có thể chứa 1000 thẻ. Một trạng thái được mô tả bởi tất cả các kích thước ngăn xếp, do đó cách tiếp cận lập trình động trực tiếp có thể có một số lượng lớn các trạng thái. Ngay cả với tính năng ghi nhớ, số lượng cấu hình có thể có tỷ lệ thuận với tích các giá trị có thể có của tất cả các ngăn xếp, đây là điều vô cùng to lớn đối với 1000 ngăn xếp. Giải pháp dự định chỉ nên kiểm tra từng ngăn xếp một số lần không đổi. 

Số lượng quan trọng là tính chẵn lẻ của tổng số thẻ và khi số lượng ngăn xếp là số chẵn, tính chẵn lẻ của ngăn xếp nhỏ nhất. Ngăn xếp nhỏ nhất quan trọng vì việc di chuyển tất cả các ngăn xếp là hợp pháp chính xác trong khi ngăn xếp tối thiểu là dương. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ giải pháp chỉ dựa trên tổng số thẻ. Ví dụ, với```
2
1 2
```tổng cộng là`3`, điều này thật kỳ lạ và Lowie thắng. Đây là mẫu đầu tiên. Một giải pháp chỉ xem xét liệu số lượng ngăn xếp có chẵn hay không sẽ bỏ lỡ điều này. 

Một trường hợp tế nhị hơn là```
2
2 2
```Tổng là số chẵn và số nhỏ nhất là số chẵn nên đáp án đúng là`imitater`. Nếu Lowie loại bỏ một lá bài khỏi một trong hai ngăn xếp, tổng số sẽ trở thành số lẻ. Nếu Lowie loại bỏ một lá bài khỏi cả hai ngăn xếp, vị trí sẽ trở thành`(1, 1)`, đó là chiến thắng cho người chơi tiếp theo. Chỉ nhìn vào tổng số chẵn lẻ sẽ xử lý không chính xác tất cả các vị trí có tổng số chẵn. 

Một trường hợp quan trọng khác là```
2
1 3
```Tổng cộng ở đây là`4`, nhưng mức tối thiểu là số lẻ. Lowie có thể loại bỏ một thẻ từ cả hai ngăn xếp và tiếp cận`(0, 2)`. Chỉ còn lại một chồng, chứa hai quân bài nên Imitater thua vì Lowie có thể lấy hai quân bài đó trong các lượt liên tiếp. Như vậy vị trí này đang mang lại chiến thắng cho Lowie mặc dù tổng điểm của nó là chẵn. 

## Phương pháp tiếp cận 

Một giải pháp lý thuyết trò chơi trực tiếp sẽ kiểm tra đệ quy mọi nước đi hợp pháp. Từ vị trí với`N`ngăn xếp không trống, có thể có tới`N`di chuyển một ngăn xếp cộng với di chuyển tất cả các ngăn xếp. Phép đệ quy là đúng vì một vị trí đang thắng chính xác khi nó có ít nhất một nước đi đến vị trí thua, trong khi một vị trí đang thua khi mọi nước đi hợp pháp đều đi đến vị trí thắng. 

Vấn đề là kích thước của không gian trạng thái. Nếu xếp chồng`i`có thể chứa bất kỳ giá trị nào từ`0`bởi vì`Ai`, có thể có tới 

[ 
\prod_{i=1}^{N}(A_i+1) 
] 

cấu hình khác nhau. Với các giá trị tối đa, điều đó trở thành`1001^1000`các trạng thái có thể. Ngay cả khi mỗi trạng thái chỉ được đánh giá một lần bằng tính năng ghi nhớ, việc kiểm tra tối đa`N+1`di chuyển mỗi tiểu bang sẽ cung cấp cho`O(N * product(Ai + 1))`công việc. Nếu không ghi nhớ, cây trò chơi thậm chí còn lớn hơn. 

Quan sát hữu ích là mỗi nước đi sẽ thay đổi tổng số quân bài một lượng rất dễ đoán. Một lần di chuyển một ngăn xếp sẽ giảm tổng số đi`1`. Di chuyển tất cả các ngăn xếp sẽ giảm nó đi`N`. 

Khi`N`là số lẻ, cả hai bước đi có thể đều làm thay đổi tính chẵn lẻ của tổng, bởi vì cả hai`1`Và`N`thật kỳ quặc. Vì trạng thái cuối có tổng`0`, là số chẵn, người chơi nào có thể chuyển từ tổng số lẻ sang tổng số chẵn sẽ có số chẵn lẻ sẽ thắng. Do đó, đối với số lẻ`N`, vị trí ban đầu sẽ thắng chính xác khi tổng số là số lẻ. 

Khi`N`chẵn thì hai nước đi diễn biến khác nhau. Di chuyển một ngăn xếp sẽ thay đổi tính chẵn lẻ của tổng số, trong khi di chuyển tất cả các ngăn xếp sẽ trừ đi một số chẵn và duy trì tổng số chẵn lẻ. Điều này làm cho ngăn xếp tối thiểu có liên quan, bởi vì chỉ có thể sử dụng liên tục di chuyển tất cả các ngăn xếp khi mức tối thiểu vẫn dương. 

Thậm chí`N`, các vị trí thua chính xác là những vị trí có tổng số chẵn và số tiền chẵn tối thiểu. Nếu tổng số là lẻ, Lowie luôn có thể chuyển sang thế thua có tổng số chẵn. Nếu tổng số chẵn nhưng số nhỏ nhất là số lẻ, Lowie có thể thực hiện di chuyển tất cả các ngăn xếp, làm cho số nhỏ nhất đồng đều trong khi vẫn giữ nguyên tổng số chẵn lẻ. Nếu cả tổng và tối thiểu đều là số lẻ, thay vào đó, Lowie sẽ loại bỏ một lá bài khỏi ngăn xếp tối thiểu, làm cho cả hai số lượng đều chẵn. 

Do đó, mô tả kết quả rất đơn giản: Lowie thắng nếu tổng số là số lẻ hoặc nếu`N`là số chẵn và ngăn xếp tối thiểu là số lẻ. Trong mọi trường hợp khác Imitater đều thắng. Đặc điểm này cũng có thể được chứng minh trực tiếp bằng cách chỉ ra rằng mọi nước đi từ vị trí thua đều dẫn đến vị trí thắng và mọi nước đi đều có nước đi đến vị trí thua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N * product(Ai + 1))`với ghi nhớ |`O(product(Ai + 1))`| Quá chậm | 
| Tối ưu |`O(N)`|`O(1)`phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả kích thước ngăn xếp và tính toán`sum`, tổng số thẻ, và`mn`, kích thước ngăn xếp nhỏ nhất. Đây là những thuộc tính duy nhất cần thiết cho quyết định cuối cùng. 
2. Nếu`sum`là số lẻ, đầu ra`lowie`. Di chuyển một ngăn xếp luôn thay đổi tổng số chẵn lẻ và di chuyển tất cả các ngăn xếp cũng thay đổi nó khi`N`thật kỳ quặc. Tổng quát hơn, khi tổng là số lẻ, luôn có một nước đi hợp lệ đạt đến vị trí tổng thua. 
3. Nếu`sum`là số chẵn và`N`là số lẻ, đầu ra`imitater`. Mỗi nước đi hợp lệ sẽ thay đổi tổng số từ chẵn thành lẻ, vì vậy Lowie không thể chuyển trực tiếp sang vị trí chẵn có tổng thua khác. 
4. Nếu`N`là số chẵn và`sum`chẵn, kiểm tra`mn`. Nếu như`mn`là số lẻ, đầu ra`lowie`, bởi vì Lowie có thể loại bỏ một thẻ khỏi mỗi ngăn xếp. Tổng số vẫn chẵn vì`N`là số chẵn, trong khi điểm tối thiểu trở thành số chẵn. 
5. Nếu`N`là chẵn,`sum`là chẵn, và`mn`chẵn, đầu ra`imitater`. Mỗi lần di chuyển một ngăn xếp sẽ tạo ra tổng số lẻ, trong khi một lần di chuyển tất cả các ngăn xếp sẽ tạo ra số lẻ tối thiểu. Cả hai vị trí kết quả đều mang lại chiến thắng cho người chơi tiếp theo. 

### Tại sao nó hoạt động 

Xét tập hợp các vị trí trong đó tổng số quân bài là số chẵn và hoặc`N`là số lẻ hoặc ngăn xếp tối thiểu là số chẵn. Chúng tôi khẳng định đây chính xác là những vị trí thua lỗ. 

Từ vị trí như vậy, một nước đi xếp chồng đơn sẽ tạo ra tổng số lẻ, do đó vị trí kết quả là thắng. Nếu như`N`là số lẻ, việc di chuyển tất cả các ngăn xếp cũng làm cho tổng số là số lẻ. Nếu như`N`là chẵn, nước đi tất cả các ngăn xếp sẽ bảo toàn tổng số chẵn nhưng giảm mức tối thiểu đi một, làm cho nó trở thành số lẻ, điều này mang lại cho người chơi tiếp theo vị trí chiến thắng. 

Bây giờ hãy xem xét mọi vị trí bên ngoài tập hợp này. Nếu tổng số là số lẻ, Lowie có thể chọn nước đi một ngăn khi`N`là số lẻ hoặc chọn nước đi phù hợp dựa trên mức tối thiểu khi`N`chẵn, rơi vào thế thua. Nếu tổng số là chẵn thì`N`phải chẵn và giá trị nhỏ nhất phải là số lẻ. Việc di chuyển tất cả các ngăn xếp sau đó bảo toàn tổng số chẵn và tạo ra số chẵn tối thiểu, tạo ra vị trí thua. Như vậy mọi vị trí thắng đều có bước chuyển sang vị trí thua, hoàn thành việc chứng minh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)
mn = min(a)

if total % 2 == 1:
    print("lowie")
elif n % 2 == 1:
    print("imitater")
elif mn % 2 == 1:
    print("lowie")
else:
    print("imitater")
```Đầu vào chứa chính xác một trò chơi, do đó không cần vòng lặp trường hợp thử nghiệm. Chúng ta đọc mảng một lần, sau đó tính tổng và giá trị tối thiểu của nó. 

Điều kiện đầu tiên xử lý mọi vị trí tổng lẻ ngay lập tức. Điều này được kiểm tra có chủ ý trước số lượng ngăn xếp vì tổng số lẻ là đủ để khiến vị trí chiến thắng bất kể`N`là số lẻ hoặc số chẵn. 

Sau khi biết tổng số là chẵn, số lẻ sẽ khiến vị thế bị mất. Thậm chí`N`, câu trả lời phụ thuộc vào ngăn xếp tối thiểu. Mức tối thiểu lẻ giúp Lowie di chuyển tất cả các ngăn xếp đến vị trí thua, trong khi mức tối thiểu chẵn có nghĩa là Imitater có thể duy trì cấu trúc thua tương ứng. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Trên thực tế, tổng số lớn nhất có thể chỉ là`1000 * 1000 = 1,000,000`, cũng sẽ vừa vặn thoải mái với số nguyên 32 bit tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
2
1 2
```Các giá trị quan trọng là: 

|`N`|`sum`|`min`| Quyết định | 
| --- | --- | --- | --- | 
| 2 | 3 | 1 |`sum`thật kỳ quặc | 
| 2 | 3 | 1 |`lowie`| 

Lowie thắng ngay lập tức nhờ đặc tính chẵn lẻ. Một động thái chiến thắng cụ thể là loại bỏ một lá bài khỏi ngăn xếp thứ hai, tạo ra`(1, 1)`. Người bắt chước sau đó phải di chuyển và Lowie có thể lấy hai lá bài cuối cùng trong trò chơi kết quả. Trực tiếp hơn, tổng số lẻ có nghĩa là Lowie có thể buộc phải đạt đến trạng thái tổng số chẵn cuối cùng sau khi di chuyển. 

### Mẫu 2 

Đầu vào là:```
3
1 4 3
```Các giá trị là: 

|`N`|`sum`|`min`| Quyết định | 
| --- | --- | --- | --- | 
| 3 | 8 | 1 |`sum`là chẵn | 
| 3 | 8 | 1 |`N`thật kỳ quặc | 
| 3 | 8 | 1 |`imitater`| 

Vì có ba ngăn xếp nên mỗi nước đi hợp lệ sẽ loại bỏ một số lẻ quân bài:`1`thẻ hoặc`3`thẻ. Bắt đầu từ tổng số chẵn, Lowie nhất thiết phải cho Imitater một vị trí có tổng số lẻ. Người chơi có tổng số lẻ là người có lợi thế nên Imitater thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N)`| Mảng được quét để tính tổng và tối thiểu. | 
| Không gian |`O(N)`| Mảng đầu vào lưu trữ tất cả`N`kích thước ngăn xếp. | 

Đầu vào tối đa chỉ chứa 1000 số nguyên, do đó, một lần quét tuyến tính dễ dàng nằm trong giới hạn một giây. Tính toán lý thuyết trò chơi phụ trợ sau khi đọc mảng là thời gian không đổi và mức sử dụng bộ nhớ rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)
    mn = min(a)

    if total % 2 == 1:
        return "lowie"
    elif n % 2 == 1:
        return "imitater"
    elif mn % 2 == 1:
        return "lowie"
    else:
        return "imitater"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("2\n1 2\n") == "lowie", "sample 1"
assert run("3\n1 4 3\n") == "imitater", "sample 2"

# Minimum-size input
assert run("1\n1\n") == "lowie", "single stack with one card"

# Single stack with even number of cards
assert run("1\n2\n") == "imitater", "single stack with two cards"

# Even N, even total, even minimum
assert run("2\n2 2\n") == "imitater", "even total and even minimum"

# Even N, even total, odd minimum
assert run("2\n1 3\n") == "lowie", "even total and odd minimum"

# All values equal, even N
assert run("4\n6 6 6 6\n") == "imitater", "all equal even stacks"

# All values equal, even N, odd minimum
assert run("4\n5 5 5 5\n") == "lowie", "all equal odd stacks"

# Maximum-size input
assert run("1000\n" + "1000 " * 999 + "1000\n") == "imitater", \
    "maximum N and Ai"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`lowie`| Đầu vào tối thiểu có thể có và mẫu chẵn lẻ cuối cùng cho một ngăn xếp | 
|`1 / 2`|`imitater`| Ranh giới đếm chẵn một ngăn xếp | 
|`2 / 2 2`|`imitater`| Thậm chí`N`, tổng số chẵn, số chẵn tối thiểu | 
|`2 / 1 3`|`lowie`| Thậm chí`N`, tổng số chẵn, tối thiểu lẻ | 
|`4 / 6 6 6 6`|`imitater`| Tất cả các ngăn xếp đều bằng nhau với mức tối thiểu chẵn | 
|`4 / 5 5 5 5`|`lowie`| Tất cả các ngăn xếp đều có số lẻ tối thiểu | 
|`1000 / 1000 ... 1000`|`imitater`| Kích thước đầu vào tối đa và giá trị lớn | 

## Vỏ cạnh 

### Một ngăn xếp 

cho```
1
2
```chúng tôi có`N = 1`,`sum = 2`, Và`min = 2`. Tổng số là chẵn nên thuật toán đạt đến`N`nhánh lẻ và trả về`imitater`. Với một ngăn xếp, cả hai mô tả nước đi hợp pháp đều thực hiện giống nhau, loại bỏ một quân bài, do đó số quân bài chẵn thực sự bị thua. 

Vì```
1
1
```tổng số là số lẻ nên nhánh đầu tiên trả về`lowie`. Lowie loại bỏ lá bài duy nhất và thắng ngay lập tức. 

### Số ngăn xếp chẵn có tổng số chẵn và số chẵn tối thiểu 

cho```
2
2 2
```chúng tôi có`sum = 4`Và`min = 2`. Tổng số là chẵn`N`là số chẵn và giá trị nhỏ nhất là số chẵn, do đó thuật toán trả về`imitater`. 

Việc di chuyển một ngăn xếp sẽ tạo ra một trong hai`(1, 2)`hoặc`(2, 1)`, cả hai đều có tổng số lẻ. Một bước di chuyển tất cả các ngăn xếp tạo ra`(1, 1)`, trong đó mức tối thiểu là số lẻ. Do đó, mọi nước đi có thể đều mang lại cho người chơi tiếp theo một vị trí chiến thắng. 

### Số ngăn xếp chẵn có tổng số chẵn và số lẻ tối thiểu 

cho```
2
1 3
```chúng tôi có`sum = 4`Và`min = 1`. Thuật toán trả về`lowie`bởi vì mức tối thiểu là số lẻ. 

Lowie sử dụng chiêu thức tổng hợp, tạo ra`(0, 2)`. Việc di chuyển tất cả các ngăn xếp không còn hợp pháp vì một ngăn xếp trống, để lại một trò chơi một ngăn xếp thông thường có hai lá bài. Người bắt chước phải loại bỏ một thẻ và Lowie loại bỏ thẻ cuối cùng. 

Đây là trường hợp cho thấy tại sao chỉ riêng tổng số chẵn lẻ là không đủ khi`N`là chẵn. 

### Số lượng ngăn xếp lẻ có tổng số chẵn 

cho```
3
1 4 3
```tổng cộng là`8`, đó là số chẵn, và`N = 3`thật kỳ quặc. Thuật toán trả về`imitater`. 

Mỗi nước đi sẽ loại bỏ một hoặc ba lá bài, cả hai đều là số lẻ. Do đó, mọi nước đi đều làm đảo lộn tính chẵn lẻ của tổng số. Vì tổng điểm ban đầu là chẵn nên Lowie không thể thực hiện nước đi cuối cùng trong lối chơi tối ưu. 

### Đầu vào tối đa 

Đối với 1000 ngăn xếp, mỗi ngăn chứa 1000 thẻ, tổng số là`1,000,000`, chẵn và nhỏ nhất là`1000`, cũng chẵn. Từ`N`cũng vậy, thuật toán trả về`imitater`. 

Chỉ cần một lần quét trên 1000 giá trị. Bản thân trò chơi không bao giờ cần phải mô phỏng, đó chính xác là điều khiến giải pháp trở nên thiết thực với lượng đầu vào lớn nhất được phép.
