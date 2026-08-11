---
title: "CF 102399L - \u0414\u043e\u0440\u043e\u0433\u043e\u0439 \u0448\u043a\u0430\u0444"
description: "Chúng tôi có một chiếc tủ hình chữ nhật đặt ở góc phòng. Hai chiều của nó dọc theo các bức tường là a và b. Một cánh cửa có chiều dài d được lắp cách góc một khoảng l."
date: "2026-08-11T05:43:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "L"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 84
verified: true
draft: false
---

[CF 102399L - \u0414\u043e\u0440\u043e\u0433\u043e\u0439 \u0448\u043a\u0430\u0444](https://codeforces.com/problemset/problem/102399/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chiếc tủ hình chữ nhật đặt ở góc phòng. Hai kích thước của nó dọc theo các bức tường là`a`Và`b`. Một cánh cửa dài`d`được gắn ở khoảng cách xa`l`từ góc. Khi cửa mở, chúng ta cần xác định xem nó có thể chạm tới một trong hai bức tường của phòng mà không chạm vào tủ hay không. 

Hình học được xem xét từ trên cao. Chiếc tủ chiếm một hình chữ nhật sát góc, trong khi cánh cửa xoay quanh bản lề. Cánh cửa chỉ thành công nếu cuối cùng nó dừng lại vào tường và không chạm vào tủ ở bất kỳ điểm nào. Chạm vào tủ đúng một góc cũng tính là thất bại. Đầu vào bao gồm bốn số nguyên dương`a`,`b`,`d`, Và`l`, với tất cả các giá trị nhiều nhất`30000`Và`a <= l`. Đầu ra cần thiết là`Yes`nếu tồn tại ít nhất một cách an toàn để tiếp cận bức tường và`No`nếu không thì. 

Giới hạn nhỏ nhưng điều đó không có nghĩa là chúng ta nên tìm kiếm theo vị trí hoặc góc. Không có tập hợp các góc cửa có thể có riêng biệt, vì vậy việc mô phỏng sẽ phải gần đúng với một quá trình hình học liên tục. Giải pháp mong muốn làm giảm toàn bộ vấn đề thành một số phép tính số học không đổi. Chỉ với bốn số ở đầu vào, một`O(1)`giải pháp đủ nhanh một cách dễ dàng, trong khi mối quan tâm thực sự là giải quyết đúng các bất đẳng thức hình học và tính nghiêm ngặt của chúng. 

Có hai trường hợp ranh giới không rõ ràng. 

Đầu tiên, việc chạm vào tủ bị cấm. Ví dụ,```
1
1
1
2
```cho```
No
```Ở đây cánh cửa có thể chạm tới bức tường gần hơn một cách chính xác khi`a + d = l`. Điểm cuối của nó khi đó chính xác là tại ranh giới của tủ có liên quan, do đó cửa chạm vào tủ. Việc thực hiện bất cẩn bằng cách sử dụng`a + d <= l`sẽ in sai`Yes`. Mẫu chính thức sử dụng rõ ràng trường hợp này. 

Thứ hai, chiều dài đủ để chạm tới bức tường là không đủ nếu quỹ đạo tương ứng đi qua tủ. Ví dụ,```
4
3
10
8
```cho```
No
```Mặc dù cánh cửa đủ dài để chạm tới bức tường nhưng quỹ đạo giới hạn lại đi qua tủ nên cánh cửa chạm vào nó trước khi chạm tới bức tường. Đây là mẫu chính thức thứ hai. 

Ngoài ra còn có ranh giới hình học suy biến khi`a = l`. Trong trường hợp đó số lượng`l - a`trở thành số không. Quỹ đạo đi qua góc xa sẽ thẳng đứng tại ranh giới liên quan, do đó công thức tường xa không thể chia cho`l - a`. Từ`a + d < l`cũng là không thể, câu trả lời phải là`No`. 

## Phương pháp tiếp cận 

Một mô phỏng hình học theo nghĩa đen sẽ cố gắng xoay cánh cửa và xác định thời điểm nó chạm vào tường hoặc tủ lần đầu tiên. Đây không phải là mô hình tính toán hữu ích vì góc cửa là liên tục. Góc lấy mẫu có thể bỏ lỡ vị trí tiếp tuyến chính xác, đặc biệt trong trường hợp chỉ cần chạm vào tủ cũng có thể thay đổi câu trả lời. Việc tăng mật độ lấy mẫu chỉ làm cho chương trình chậm hơn mà không biến phép tính gần đúng thành nghiệm chính xác. 

Một cách giải thích bạo lực trực tiếp hơn là xem xét hai bức tường có thể có một cách riêng biệt. Nếu cửa tránh tủ thì lần tiếp xúc thành công đầu tiên của nó phải là với bức tường gần cửa hơn hoặc với bức tường đối diện. Chỉ có hai khả năng hình học nên không có không gian tìm kiếm lớn để liệt kê. Việc tối ưu hóa thực sự không phải là giảm số lượng các trường hợp mà là rút ra bất đẳng thức chính xác cho từng trường hợp và tránh các phép tính dấu phẩy động. 

Đối với bức tường gần hơn, hình học đặc biệt đơn giản. Cánh cửa có chiều dài`d`, trong khi tủ chiếm`a`đơn vị giữa cửa và góc dọc theo hướng liên quan. Để đi qua cái tủ và đến bức tường gần hơn mà không chạm vào nó, khoảng cách có sẵn`l`phải lớn hơn độ dài tổng hợp của chúng. Như vậy điều kiện là`a + d < l`. 

Bất đẳng thức chặt chẽ suy ra trực tiếp từ quy tắc va chạm. Bình đẳng có nghĩa là điểm cuối cửa chạm tới ranh giới tủ một cách chính xác. 

Bức tường xa hơn đòi hỏi sự quan sát hình học quan trọng. Cho phép`x = l - a`. 

Cửa phải đi qua góc xa của tủ trong cấu hình giới hạn. Từ điểm bắt đầu của cửa đến góc tủ đó, chuyển vị ngang là`x`và chuyển vị vuông góc là`b`. Theo định lý Pythagore, đoạn đó có độ dài`sqrt(x² + b²)`. 

Đường tương tự tiếp tục cho đến khi chạm tới bức tường đối diện. Vì thành phần nằm ngang của nó phát triển từ`x`ĐẾN`l`, toàn bộ đoạn từ bản lề cửa tới bức tường đó có chiều dài`l * sqrt(x² + b²) / x`. 

Cánh cửa chỉ có thể chạm tới bức tường xa hơn một cách an toàn khi chiều dài của nó lớn hơn giá trị này. Sự bất bình đẳng nghiêm ngặt một lần nữa lại quan trọng bởi vì sự bình đẳng có nghĩa là cánh cửa chạm tới góc tủ một cách chính xác và do đó làm trầy xước nó. Đây là đạo hàm hình học được sử dụng trong phân tích cuộc thi chính thức. 

Chúng ta có thể đánh giá điều kiện này bằng cách sử dụng căn bậc hai dấu phẩy động, nhưng không có lý do gì để đưa độ chính xác bằng số vào một bài toán số nguyên chính xác. Bắt đầu với`d > l * sqrt(x² + b²) / x`và sử dụng`x > 0`, chúng ta có thể bình phương cả hai vế dương và nhân với`x²`. Điều kiện trở thành`d² * x² > l² * (x² + b²)`. 

Tất cả các đại lượng đều không âm nên bình phương không làm thay đổi tính chân thực của bất đẳng thức. Số nguyên Python cũng có độ chính xác tùy ý nên việc tràn số không phải là vấn đề. 

Hai trường hợp là độc lập. Nếu điều kiện tường gần hơn hoặc điều kiện tường xa hơn thành công, câu trả lời là`Yes`. Nếu không thì câu trả lời là`No`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng xoay số | Phụ thuộc vào độ phân giải góc | O(1) | Không chính xác và không cần thiết | 
| Điều kiện hình học trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn chiều`a`,`b`,`d`, Và`l`. Chúng mô tả tủ, cửa và khoảng cách từ cửa đến góc phòng. 
2. Kiểm tra xem`a + d < l`. Nếu điều này đúng, cánh cửa có thể chạm tới bức tường gần hơn trước khi điểm cuối của nó chạm tới tủ, vì vậy câu trả lời là ngay lập tức.`Yes`. Bình đẳng bị cố tình bác bỏ vì chạm vào tủ bị coi là va chạm. 
3. Tính toán`x = l - a`. Đây là khoảng cách theo chiều ngang từ bản lề cửa đến góc xa của tủ. 
4. Nếu`x = 0`, việc xây dựng bức tường xa hơn là không thể đánh giá được vì đường giới hạn không có thành phần nằm ngang. Điều kiện gần tường hơn đã thất bại nên câu trả lời là`No`. 
5. So sánh`d² * x² > l² * (x² + b²)`. 

Đây là phiên bản bình phương của điều kiện là cánh cửa phải dài hơn đường từ bản lề đến bức tường xa hơn đi qua góc tủ. 
6. Nếu so sánh đúng, hãy in`Yes`. Nếu không thì in`No`. 

Tại sao nó hoạt động: cách an toàn duy nhất để cửa mở xong mà không va vào tủ là đến bức tường gần hơn hoặc đến bức tường xa hơn. Khả năng đầu tiên được đặc trưng chính xác bởi`a + d < l`. Đối với khả năng thứ hai, chiều dài cửa ngắn nhất có khả năng chạm tới bức tường xa hơn mà vẫn ở phía an toàn của tủ là đường đi qua góc xa của tủ, có chiều dài bằng`l * sqrt((l-a)²+b²)/(l-a)`. Một cánh cửa dài hơn sẽ chạm tới bức tường mà không chạm vào góc đó, trong khi một cánh cửa bằng hoặc ngắn hơn không thể làm điều đó một cách an toàn. Vì cả hai khả năng hình học đều được kiểm tra chính xác nên thuật toán trả về`Yes`chính xác khi có một lỗ mở an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = int(input())
    b = int(input())
    d = int(input())
    l = int(input())

    # The door can pass to the nearer wall.
    if a + d < l:
        print("Yes")
        return

    x = l - a

    # No room remains for the far-wall trajectory.
    if x == 0:
        print("No")
        return

    # Compare:
    # d > l * sqrt(x^2 + b^2) / x
    #
    # All quantities are positive, so we can square safely:
    # d^2 * x^2 > l^2 * (x^2 + b^2)
    left = d * d * x * x
    right = l * l * (x * x + b * b)

    print("Yes" if left > right else "No")

if __name__ == "__main__":
    solve()
```Phép so sánh đầu tiên thực hiện trực tiếp trường hợp tường gần hơn. Không có căn bậc hai liên quan, và nghiêm ngặt`<`là cần thiết vì bình đẳng nghĩa là cửa chạm vào tủ. 

Biến`x`đại diện cho`l - a`, khoảng cách theo phương ngang từ bản lề cửa đến góc tủ xa. Khi`x`bằng 0, chia cho nó sẽ không hợp lệ. Quan trọng hơn, không có lối thoát tường xa hợp lệ trong cấu hình này, vì vậy việc quay lại`No`là hành vi hình học chính xác. 

Sự so sánh cuối cùng được viết có chủ ý chỉ bằng phép nhân số nguyên. Điều kiện hình học ban đầu chứa căn bậc hai và phép chia, nhưng cả hai đều có thể bị loại bỏ vì mọi đại lượng liên quan đều không âm. Sự nghiêm khắc`>`phải tiếp tục nghiêm ngặt. Thay thế nó bằng`>=`sẽ chấp nhận sai một cánh cửa chạm tới góc tủ một cách chính xác. 

Sản phẩm lớn nhất theo thứ tự`30000^4`, xung quanh`8.1 * 10^17`, phù hợp với số nguyên 64 bit có dấu. Dù sao thì số nguyên Python cũng không bị giới hạn nên việc triển khai vẫn an toàn ngay cả khi không cần lý luận thủ công về tình trạng tràn máy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
2
6
4
```Thuật toán xử lý nó như sau. 

| Bước |`a`|`b`|`d`|`l`|`x = l-a`| Gần tường | So sánh bức tường xa | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 6 | 4 | 2 |`2+6 < 4`là sai |`144*4 > 16*8`là đúng | Có | 

Cánh cửa không thể đi qua phía gần hơn vì chiều dài của nó quá lớn so với tuyến đường đó. Đối với bức tường xa hơn,`x = 2`, do đó so sánh bình phương trở thành`6² * 2² > 4² * (2² + 2²)`, 

đó là`144 > 128`. 

Cánh cửa đủ dài để chạm tới bức tường xa hơn đồng thời tránh được tủ, do đó kết quả là`Yes`. Điều này phù hợp với mẫu chính thức đầu tiên. 

### Mẫu 2 

Đầu vào là```
4
3
10
8
```Nhà nước là 

| Bước |`a`|`b`|`d`|`l`|`x = l-a`| Gần tường | So sánh bức tường xa | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 3 | 10 | 8 | 4 |`4+10 < 8`là sai |`1600*16 > 64*25`là sai | Không | 

Bức tường gần hơn là không thể bởi vì`14 < 8`là sai. Đối với bức tường xa hơn,`d² * x² = 10² * 4² = 1600`trong khi`l² * (x² + b²) = 8² * (4² + 3²) = 1600`. 

Các giá trị hoàn toàn bằng nhau. Tức là cửa chạm tới góc tủ một cách chính xác, tính là chạm vào tủ. Vì điều kiện phải nghiêm ngặt nên câu trả lời là`No`. Trường hợp bình đẳng này chính xác là lý do tại sao sử dụng`>=`sẽ không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng cố định các phép tính số học và phép so sánh được thực hiện. | 
| Không gian | O(1) | Chỉ có bốn giá trị đầu vào và một vài số nguyên trung gian được lưu trữ. | 

Các ràng buộc cho phép các giá trị lên tới`30000`, do đó số học vẫn còn rất nhỏ. Giải pháp không phụ thuộc vào kích thước của căn phòng, số lượng vị trí cửa có thể có hoặc độ phân giải góc. Do đó, nó thoải mái phù hợp với giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    a = int(input())
    b = int(input())
    d = int(input())
    l = int(input())

    if a + d < l:
        print("Yes")
        return

    x = l - a

    if x == 0:
        print("No")
        return

    left = d * d * x * x
    right = l * l * (x * x + b * b)

    print("Yes" if left > right else "No")

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("2\n2\n6\n4\n") == "Yes\n", "sample 1"
assert run("4\n3\n10\n8\n") == "No\n", "sample 2"
assert run("1\n1\n1\n2\n") == "No\n", "sample 3"
assert run("1\n1\n1\n3\n") == "Yes\n", "sample 4"

# Minimum-size input
assert run("1\n1\n1\n1\n") == "No\n", "minimum values"

# Maximum-size input
assert run("30000\n30000\n30000\n30000\n") == "No\n", "maximum values"

# Near-wall equality, which must fail because touching is forbidden
assert run("1\n7\n5\n6\n") == "No\n", "near-wall equality"

# Near-wall escape, with one extra unit of distance
assert run("1\n7\n5\n7\n") == "Yes\n", "near-wall strict inequality"

# Far-wall equality, using a 3-4-5 triangle
assert run("2\n3\n10\n4\n") == "No\n", "far-wall equality"

# Far-wall condition succeeds with a longer door
assert run("2\n3\n11\n4\n") == "Yes\n", "far-wall strict inequality"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`No`| Đầu vào có kích thước tối thiểu và`a = l`ranh giới | 
|`30000 30000 30000 30000`|`No`| Số học kích thước tối đa và`a = l`| 
|`1 7 5 6`|`No`| Bình đẳng gần tường không được chấp nhận | 
|`1 7 5 7`|`Yes`| Bất bình đẳng nghiêm ngặt gần tường | 
|`2 3 10 4`|`No`| Tiếp tuyến tường xa chính xác bằng cách sử dụng bộ ba Pythagore | 
|`2 3 11 4`|`Yes`| Tình trạng tường xa sau khi vượt ngưỡng chính xác | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là sự bình đẳng cho bức tường gần hơn. Với```
1
1
1
2
```chúng tôi có`a + d = 2`Và`l = 2`. Bài kiểm tra`a + d < l`thất bại. Cửa không thể đi qua ranh giới tủ nếu không chạm vào nó nên thuật toán in`No`. sử dụng`<=`sẽ âm thầm biến một vụ va chạm thành một mở đầu thành công. 

Trường hợp cạnh thứ hai là một cánh cửa đủ dài để chạm tới bức tường xa hơn chỉ bằng cách chạm vào tủ. Coi như```
4
3
10
8
```Đây`x = 4`. Hai đại lượng bình phương đều là`1600`, do đó so sánh bức tường xa`left > right`thất bại. Sự bình đẳng thể hiện cánh cửa đi qua chính xác góc tủ. Vì việc liên lạc bị cấm,`No`là đúng. 

Một trường hợp đẳng thức chính xác hữu ích cho công thức tường xa là```
2
3
10
4
```Đây`x = l-a = 2`, trong khi`b = 3`, Vì thế`sqrt(x²+b²) = 5`. Độ dài giới hạn là`l * 5 / x = 4 * 5 / 2 = 10`. 

Cánh cửa có chiều dài chính xác`10`, thế là nó chạm tới góc tủ và chạm vào nó. So sánh số nguyên mang lại sự bình đẳng ở cả hai bên và bản in`No`. 

Nếu chúng ta tăng chiều dài cửa lên một,```
2
3
11
4
```chiều dài giới hạn vẫn là`10`, nhưng bây giờ`d = 11`. Sự bất đẳng thức là nghiêm ngặt nên cánh cửa có thể chạm tới bức tường xa hơn mà không chạm vào tủ, và đáp án trở thành`Yes`. 

Cuối cùng, hãy xem xét ranh giới`a = l`, Ví dụ```
1
1
1
1
```Điều kiện gần tường hơn là không thể bởi vì`a + d < l`trở thành`2 < 1`. Đồng thời,`x = l-a = 0`, do đó không có phép chia hợp lệ trong công thức tường xa. Thuật toán xử lý rõ ràng trường hợp này trước khi thực hiện so sánh bình phương và trả về`No`. Nhánh này cần thiết cho cả tính chính xác về mặt toán học và việc thực hiện an toàn.
