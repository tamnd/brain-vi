---
title: "CF 102412F - Kiểm tra IQ"
description: "Chúng ta bắt đầu với một tập hợp chứa 0, 1 và 2. Một thao tác chọn bất kỳ hai số x và y nào đã có trong tập hợp đó và chèn x 2 −y. Giá trị được chèn phải nằm trong khoảng từ 0 đến 10 18 và chúng tôi có thể thực hiện tối đa 43 thao tác."
date: "2026-08-11T08:28:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102412
codeforces_index: "F"
codeforces_contest_name: "MEX Foundation Contest (supported by AIM Tech)"
rating: 0
weight: 102412
solve_time_s: 93
verified: true
draft: false
---

[CF 102412F - Kiểm tra IQ](https://codeforces.com/problemset/problem/102412/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một bộ chứa`0`,`1`, Và`2`. Một thao tác chọn hai số bất kỳ`x`Và`y`đã có trong bộ và chèn 

x 2 −y. 

Giá trị được chèn phải nằm trong khoảng`0`và 10 18, và chúng tôi có thể biểu diễn nhiều nhất`43`hoạt động. Đưa ra một mục tiêu`n`, chúng ta chỉ cần xây dựng nó chứ không cần giảm thiểu số lượng thao tác. Tuyên bố chính thức cho phép mọi công trình xây dựng hợp lệ trong các giới hạn đó. 

Mục tiêu có thể lớn tới 10 18, vì vậy cố gắng tìm kiếm trong tất cả các công trình có thể là vô vọng. Cấu trúc hữu ích là hình vuông trong phép toán. Nếu chúng ta muốn xây dựng một số số`p`, chúng ta có thể chọn`x`gần với p ​, điều này làm cho cả hai số đều cần thiết để xây dựng`p`nhỏ hơn đáng kể so với`p`. 

Những giá trị nhỏ`0`,`1`, Và`2`đã có sẵn nên chúng phải được coi là trạng thái cuối. Ví dụ, nếu`n = 2`, đầu ra đúng chỉ đơn giản là đầu ra trống vì mục tiêu đã có trong tập hợp ban đầu. Việc thực hiện bất cẩn luôn cố gắng tạo ra`n`sẽ xây dựng một giá trị khác một cách không cần thiết và thậm chí có thể tạo ra một vòng lặp đệ quy không hợp lệ. 

Một trường hợp cạnh khác là một hình vuông hoàn hảo. Vì`n = 9`, đang chọn`x = 3`cho`y = 0`, Vì thế`3^2 - 0 = 9`. Việc xây dựng đúng có thể chỉ là```
3 0
```Việc thực hiện bất cẩn giả định`y`phải dương sẽ từ chối một cách xây dựng hoàn toàn hợp lệ, mặc dù câu lệnh rõ ràng cho phép bằng không. 

Trường hợp ranh giới khác là một giá trị ngay dưới một hình vuông. Ví dụ,`n = 15`cho`x = 4`Và`y = 1`, vì 4 2 −1=15. Chi tiết quan trọng đó là`x`phải là trần của căn bậc hai chứ không phải là sàn. sử dụng`x = 3`sẽ làm cho 3 2 −15 âm và vi phạm điều kiện đầu ra. 

Các ràng buộc thực tế rất thân thiện với việc rút gọn căn bậc hai. Mục tiêu nhiều nhất là 10 18, do đó căn bậc hai của nó nhiều nhất là 10 9. Việc lấy căn bậc hai nhiều lần sẽ làm giảm độ lớn cực kỳ nhanh chóng, đạt đến các giá trị cực nhỏ chỉ sau một số cấp độ. Giới hạn chính thức là một giây và 256 MiB, với tối đa 43 thao tác được phép. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng khám phá các tập hợp có thể truy cập được sau mỗi thao tác. Với tập hợp hiện tại, nó có thể chọn mọi cặp có thứ tự`(x, y)`, tính toán`x²-y`và khám phá đệ quy trạng thái kết quả. Điều này đúng vì mọi cách xây dựng pháp lý đều được thể hiện bằng một số chuỗi lựa chọn như vậy. 

Vấn đề là số lượng lựa chọn. Ngay cả từ tập hợp ban đầu cũng có 3 cặp 2 = 9 có thứ tự. Nếu chúng ta phân nhánh một cách ngây thơ trên mỗi cặp cho 43 phép toán thì chỉ số chuỗi lựa chọn có thể có là đủ. 

9 43 ≈10 41 . 

Sự phân nhánh thực tế thậm chí còn trở nên lớn hơn khi tập hợp chứa nhiều giá trị hơn. Việc theo dõi toàn bộ tập hợp cũng tạo ra một không gian trạng thái khổng lồ, do đó, việc sử dụng vũ lực không thực tế chút nào. 

Quan sát quan trọng là đảo ngược hoạt động cuối cùng. Giả sử chúng ta muốn xây dựng`p`. Chúng tôi cần một số đã có thể xây dựng được`x`Và`y`thỏa mãn 

x 2 −y=p. 

chọn 

x=⌈ p ​ ⌉,y=x 2 −p. 

Khi đó phương trình sẽ tự động được thỏa mãn. Phần thú vị là kích thước của hai mục tiêu mới. Từ`x`là số nguyên nhỏ nhất có x 2 ≥p, ta có 

(x−1) 2 <p<x 2 . 

Như vậy 

0<y=x 2 −p<x 2 −(x−1) 2 =2x−1. 

Vì thế`x`là về p ​, và`y`nhiều nhất là khoảng 2 p ​. Thay vì xây dựng một số kích thước`p`, chúng ta xây dựng đệ quy hai số có kích thước gần bằng căn bậc hai của`p`. 

Đối với p 10 18, lần đầu tiên`x`nhiều nhất là 10 9, và`y`nhiều nhất là khoảng 2⋅10 9. Mức đệ quy tiếp theo giảm xuống khoảng 10 5, sau đó là vài trăm, rồi vài chục và cuối cùng là các giá trị ban đầu`0`,`1`, Và`2`. Việc xây dựng kết quả thoải mái ở dưới 43 hoạt động cần thiết. 

Việc xây dựng về cơ bản là phân chia và chinh phục, nhưng sự phân chia dựa trên căn bậc hai chứ không phải một nửa. Trước tiên, chúng tôi xây dựng đệ quy các phần phụ thuộc, sau đó in thao tác tạo số hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ít nhất O(9 43 ) ngành xây dựng | Hàm mũ | Quá chậm | 
| Tối ưu | O(Klogn), trong đó K<43 | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ số đã có sẵn bằng`0`,`1`, Và`2`. Những con số này không yêu cầu thao tác nào, vì vậy quá trình đệ quy sẽ dừng ngay lập tức khi nó chạm tới một trong số chúng. 
2. Đối với mục tiêu`p`chưa có sẵn, hãy tính x=⌈ p ​ ⌉. Trong Python, điều này có thể đạt được chính xác với`math.isqrt`, tránh các vấn đề về độ chính xác của dấu phẩy động đối với các giá trị gần 10 18. 
3. Tính y=x 2 −p. Bằng cách xây dựng, x 2 −y=p, vậy một lần`x`Và`y`đã được xây dựng, một thao tác cuối cùng sẽ tạo ra`p`. 
4. Xây dựng đệ quy`x`Và`y`trước khi ghi âm`(x, y)`. Thứ tự này là bắt buộc vì phép toán chỉ hợp lệ khi cả hai toán hạng đều thuộc về tập hợp. 
5. Lưu trữ mọi giá trị được xây dựng trong một`seen`bộ. Điều này ngăn việc lặp lại cùng một thao tác khi hai nhánh đệ quy khác nhau cần cùng một giá trị trung gian. 
6. Sau khi cả hai phần phụ thuộc đều có sẵn, hãy nối thêm`(x, y)`vào câu trả lời và đánh dấu`p`như đã được xây dựng. Danh sách kết quả đã có thứ tự thực hiện hợp lệ vì mọi thao tác đều xuất hiện sau các thao tác cần thiết cho toán hạng của nó. 

### Tại sao nó hoạt động 

Điều bất biến là bất cứ khi nào`build(p)`kết thúc,`p`thuộc về tập hợp được xây dựng và mọi thao tác được lưu trữ cho đến nay đều hợp pháp theo thứ tự được in. Đối với một cái mới`p`, chúng ta chọn x=⌈ p ​ ⌉ và y=x 2 −p, do đó x 2 −y=p chính xác. Cấu trúc cuộc gọi đệ quy`x`Và`y`đầu tiên, sau đó thao tác cuối cùng là hợp pháp và chèn`p`. Từ`x`Và`y`nhỏ hơn nhiều so với`p`, đệ quy đạt`0`,`1`, hoặc`2`và chấm dứt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import isqrt

def solve():
    n = int(input())

    # These numbers are present before any operation.
    seen = {0, 1, 2}
    operations = []

    def build(p):
        if p in seen:
            return

        # x is the smallest integer with x^2 >= p.
        x = isqrt(p)
        if x * x < p:
            x += 1

        y = x * x - p

        # Both operands must already exist before we can use them.
        build(x)
        build(y)

        operations.append((x, y))
        seen.add(p)

    build(n)

    out = []
    for x, y in operations:
        out.append(f"{x} {y}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`seen`bộ đại diện chính xác cho các con số hiện có sẵn trong cấu trúc mô phỏng. Bắt đầu với`0`,`1`, Và`2`phù hợp với trạng thái ban đầu của vấn đề. 

Việc tính căn bậc hai cần được quan tâm một chút. sử dụng`int(math.sqrt(p))`là không an toàn như một kỹ thuật số nguyên chung vì các số dấu phẩy động không biểu thị chính xác mọi số nguyên lên đến 10 18.`isqrt`tính toán căn bậc hai số nguyên một cách chính xác. Nếu như`x*x < p`, tăng dần`x`cho trần căn bậc hai. 

Các cuộc gọi đệ quy xảy ra trước`operations.append((x, y))`. Đảo ngược hai dòng này sẽ tạo ra câu trả lời không hợp lệ vì thao tác in có thể tham chiếu đến các số chưa được tạo. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Trên thực tế, lớn nhất`x`chỉ là 10 9, trong khi`y`chỉ ở mức 10 9, mặc dù bản thân mục tiêu có thể là 10 18. 

các`seen`check cũng xử lý các phần phụ thuộc được chia sẻ. Nếu cùng một giá trị trung gian xuất hiện ở hai nhánh thì nó chỉ được tạo một lần, giữ cho số lượng thao tác an toàn trong giới hạn. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, mục tiêu là`5`. 

| Bước | Mục tiêu đang được xây dựng | x | y | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 |`3`|`2`|`1`| 2 2 −1=3 | 
| 2 |`4`|`2`|`0`| 2 2 −0=4 | 
| 3 |`5`|`3`|`4`| 3 2 −4=5 | 

Kết quả đầu ra có thể là```
2 1
2 0
3 4
```Bản thân mẫu cũng xây dựng`0`lần đầu tiên sử dụng`1 1`, nhưng thao tác đó là không cần thiết vì`0`đã có trong tập hợp ban đầu. Vì bài toán chấp nhận mọi cách xây dựng hợp lệ nên việc bỏ qua các phép toán dư thừa là thích hợp hơn. 

Dấu vết cho thấy thứ tự phụ thuộc rõ ràng. Để xây dựng`5`, chúng tôi cần`3`Và`4`; cả hai đều được xây dựng từ các giá trị ban đầu trước khi thao tác cuối cùng được in. 

Đối với Mẫu 2, mục tiêu là`7`. 

| Bước | Mục tiêu đang được xây dựng | x | y | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 |`3`|`2`|`1`| 2 2 −1=3 | 
| 2 |`7`|`3`|`2`| 3 2 −2=7 | 

Kết quả đầu ra là```
2 1
3 2
```Điều này chứng tỏ trường hợp đặc biệt ngắn trong đó bản thân sự phụ thuộc căn bậc hai của mục tiêu có thể được xây dựng ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Klogn) | Nhiều nhất`K <= 43`các phép toán riêng biệt được tạo ra và mỗi phép toán sử dụng một căn bậc hai số nguyên và các phép toán tập hợp | 
| Không gian | O(K) | Sự đệ quy,`seen`tập hợp và danh sách thao tác chỉ chứa các giá trị trung gian được tạo | 

Phần quan trọng của độ phức tạp không phải là đa thức thông thường bị ràng buộc trên n. Độ sâu đệ quy rất nhỏ vì độ phụ thuộc lớn nhất là khoảng 2 n ​. Bắt đầu từ 10 18, độ lớn rơi vào khoảng 10 9, 10 5, 10 2, rồi đến các số nguyên nhỏ. Do đó, cấu trúc vừa vặn thoải mái trong giới hạn 43 thao tác và sử dụng bộ nhớ không đáng kể dưới giới hạn 256 MiB. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên bộ khai thác thử nghiệm sẽ xác thực trình tự được tạo thay vì so sánh nó với một chuỗi cố định. Trình xác thực bên dưới kiểm tra xem mọi toán hạng đã có sẵn chưa, mọi giá trị được tạo đều hợp pháp, mục tiêu cuối cùng được tạo và không quá 43 thao tác được in.```python
# helper: run solution on input string, return output string
import sys
import io
from math import isqrt

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n = int(sys.stdin.readline())

        seen = {0, 1, 2}
        operations = []

        def build(p):
            if p in seen:
                return

            x = isqrt(p)
            if x * x < p:
                x += 1

            y = x * x - p

            build(x)
            build(y)

            operations.append((x, y))
            seen.add(p)

        build(n)

        sys.stdout.write(
            "\n".join(f"{x} {y}" for x, y in operations)
        )
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, output: str) -> bool:
    n = int(inp.strip())

    available = {0, 1, 2}
    lines = output.strip().splitlines() if output.strip() else []

    assert len(lines) <= 43, "too many operations"

    for line in lines:
        parts = line.split()
        assert len(parts) == 2, "each operation needs x and y"

        x, y = map(int, parts)

        assert x in available, f"x={x} was not constructed"
        assert y in available, f"y={y} was not constructed"

        value = x * x - y
        assert 0 <= value <= 10**18, "generated value is out of range"

        available.add(value)

    assert n in available, f"target {n} was not constructed"
    return True

# Provided sample 1.
out = run("5\n") if False else solution("5\n")
assert validate("5\n", out), "sample 1"

# Provided sample 2.
out = solution("7\n")
assert validate("7\n", out), "sample 2"

# Minimum-size input: target already exists initially.
out = solution("0\n")
assert out == "", "zero needs no operations"

# All-equal / smallest nontrivial construction.
out = solution("4\n")
assert validate("4\n", out), "constructing a perfect square"

# Boundary case just below a square.
out = solution("15\n")
assert validate("15\n", out), "value just below 16"

# Maximum-size target.
out = solution("1000000000000000000\n")
assert validate("1000000000000000000\n", out), "maximum target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| Đầu ra trống | Ranh giới thiết lập ban đầu | 
|`4`| Bất kỳ công trình hợp lệ nào | Xử lý vuông góc hoàn hảo và bằng 0`y`| 
|`15`| Bất kỳ công trình hợp lệ nào | Trần-căn-căn ranh giới | 
|`1000000000000000000`| Bất kỳ công trình xây dựng hợp lệ nào có tối đa 43 thao tác | Mục tiêu tối đa và độ sâu đệ quy | 

## Vỏ cạnh 

cho`n = 0`, thuật toán đi vào`build(0)`, thấy ngay rằng`0`đã ở trong rồi`seen`, và trả về. Đầu vào chính xác là```
0
```và đầu ra đúng là trống. Không cần thao tác nào và việc in một thao tác không cần thiết sẽ chỉ khiến quá trình xây dựng kéo dài hơn. 

Đối với một hình vuông hoàn hảo như`n = 9`,`isqrt(9)`trả lại`3`Và`3*3`đã bằng rồi`n`, Vì thế`y = 0`. Các cuộc gọi đệ quy cho`3`Và`0`xây dựng`3`và sau đó in```
3 0
```từ đó cho ra 3 2 −0=9. Số 0 là toán hạng hợp lệ vì nó có mặt ngay từ đầu. 

Đối với một số ngay dưới một hình vuông, chẳng hạn như`n = 15`, căn bậc hai sàn là`3`, nhưng nó không thể được sử dụng vì 3 2 −15 là số âm. Thuật toán phát hiện`3*3 < 15`, số gia`x`ĐẾN`4`, và thu được`y = 1`. Thao tác cuối cùng là```
4 1
```cho kết quả là 16−1=15. Đây chính xác là lý do tại sao cần có căn bậc hai trần. 

Để đạt được mục tiêu tối đa,```
1000000000000000000
```sự lựa chọn đầu tiên là`x = 1000000000`Và`y = 0`. Thuật toán chỉ cần xây dựng`1000000000`, sau đó mục tiêu được tạo ra bởi```
1000000000 0
```Việc xây dựng đệ quy của`1000000000`sử dụng các giá trị xung quanh căn bậc hai của nó và quá trình giảm tương tự tiếp tục cho đến khi chỉ còn lại các giá trị ban đầu. Độ lớn co lại nhanh đến mức toàn bộ cây phụ thuộc vẫn nằm trong 43 thao tác được phép.
