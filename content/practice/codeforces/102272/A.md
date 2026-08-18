---
title: "CF 102272A - Ch\u01a1i Bi-a"
description: "Chúng ta có một bàn carom hình chữ nhật có góc dưới bên trái là (0, 0) và góc trên bên phải là (N, M). Một quả bóng xuất phát ở vị trí nguyên (x0, y0) bên trong bàn và chuyển động với vận tốc không đổi (vx, vy). Bất cứ khi nào nó chạm tới một bức tường thẳng đứng, dấu của vx sẽ thay đổi."
date: "2026-08-17T11:07:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102272
codeforces_index: "A"
codeforces_contest_name: "HCW 19 Individual Day 1"
rating: 0
weight: 102272
solve_time_s: 222
verified: false
draft: false
---

[CF 102272A - Ch\u01a1i Bi-a](https://codeforces.com/problemset/problem/102272/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn carom hình chữ nhật có góc dưới bên trái là`(0, 0)`và góc trên bên phải là`(N, M)`. Một quả bóng bắt đầu ở vị trí số nguyên`(x0, y0)`bên trong bàn và chuyển động với vận tốc không đổi`(vx, vy)`. Bất cứ khi nào nó chạm tới một bức tường thẳng đứng, dấu hiệu của`vx`những thay đổi. Bất cứ khi nào nó chạm tới một bức tường nằm ngang, dấu hiệu của`vy`những thay đổi. Đến một góc chỉ cần thay đổi cả hai dấu hiệu. 

Nhiệm vụ là tìm ra vị trí chính xác của quả bóng sau`S`giây. Vị trí được đảm bảo là tích phân cho số nguyên`S`, nhưng việc mô phỏng trực tiếp chuyển động thì quá tốn kém. 

Kích thước, vận tốc và thời gian đều có thể lớn bằng`10^9`, trong khi có thể có tới`10^4`trường hợp thử nghiệm. Một mô phỏng thực hiện một thao tác mỗi giây có thể yêu cầu`10^9`lặp đi lặp lại cho một trường hợp duy nhất và lên đến`10^13`lặp lại trên toàn bộ đầu vào. Điều đó không thể phù hợp với giới hạn thời gian một giây. Chúng ta cần một công thức có thời gian chạy không phụ thuộc vào`S`. 

Hai tọa độ cũng độc lập. Bức tường thẳng đứng chỉ ảnh hưởng đến tọa độ x, trong khi bức tường ngang chỉ ảnh hưởng đến tọa độ y. Điều này cho phép chúng ta giải bài toán một chiều và áp dụng nó hai lần. 

Một số trường hợp ranh giới có thể khiến việc triển khai đơn giản trở nên sai lầm. Xem xét đầu vào```
1
5 4 2 1 3 0 1
```Sau một giây, tọa độ x chính xác là`5`, vậy câu trả lời là`(5, 1)`. Việc thực hiện bất cẩn sẽ phản ánh ngay lập tức bất cứ khi nào tọa độ lớn hơn hoặc bằng`N`có thể vô tình di chuyển quả bóng trở lại`0`hoặc để`2`, tùy thuộc vào cách mã hóa sự phản chiếu. Bản thân bức tường là một vị trí hợp lệ nên công thức sóng tam giác phản xạ phải cho phép chính xác`N`. 

Vấn đề thứ hai là vận tốc âm. Vì```
1
5 4 2 1 -3 0 1
```quả bóng chạm tới`x = -1`trong bức tranh được mở ra. Vị trí thực tế là`1`, vậy câu trả lời là`(1, 1)`. Ngôn ngữ ở đâu`%`giữ phần dư âm yêu cầu xử lý đặc biệt, trong khi modulo của Python đã trả về phần dư không âm. 

Một góc có thể đạt được đồng thời ở cả hai tọa độ. Ví dụ,```
1
3 5 2 4 1 1 1
```di chuyển quả bóng tới`(3, 5)`, chính xác là góc trên bên phải. Đầu ra đúng là```
3 5
```Một phương pháp phản ánh ngay sau khi phát hiện ranh giới có thể xuất ra không chính xác`(2, 4)`trong thời gian được yêu cầu. Vị trí tại thời điểm va chạm chính xác vẫn là góc cua. 

Cuối cùng, một thành phần vận tốc có thể bằng không. Vì```
1
7 6 3 4 0 0 1000000000
```quả bóng không bao giờ chuyển động nên câu trả lời đơn giản là`(3, 4)`. Một công thức dựa trên việc chia cho vận tốc hoặc đếm các va chạm của tường phải xử lý rõ ràng trường hợp này, trong khi công thức khai triển hoạt động mà không cần bất kỳ cách xử lý toán học đặc biệt nào. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng quả bóng từng giây một. Tại mỗi giây, thêm`(vx, vy)`đến vị trí hiện tại. Nếu tọa độ x rời khỏi khoảng`[0, N]`, phản ánh nó và đảo ngược`vx`; tương tự, phản ánh tọa độ y và đảo ngược`vy`. Điều này đúng vì nó tuân theo chính xác các quy tắc vật lý của bảng. 

Vấn đề là số lần lặp lại. Với`S = 10^9`, một trường hợp thử nghiệm có thể yêu cầu`10^9`các bước mô phỏng Với`10^4`trường hợp thử nghiệm, trường hợp xấu nhất về mặt lý thuyết là`10^13`các bước. Ngay cả khi mỗi bước chỉ chứa một vài phép tính số nguyên, điều đó vẫn vượt xa giới hạn một giây. 

Quan sát quan trọng là tọa độ nảy chỉ là một sóng tam giác tuần hoàn. Chỉ xem xét tọa độ x. Nếu chiều rộng của bảng là`N`, hãy tưởng tượng loại bỏ các bức tường và cho phép quả bóng tiếp tục di chuyển vô tận. Tọa độ của nó được mở ra sau`S`giây là`u = x0 + vx * S`. 

Bảng thực có thể được xây dựng lại bằng cách gấp đường vô hạn này lại thành từng khoảng độ dài`N`. Mô hình lặp đi lặp lại mỗi`2N`: tọa độ di chuyển từ`0`ĐẾN`N`, sau đó quay lại từ`N`ĐẾN`0`, và lặp lại. 

Như vậy chúng ta chỉ cần`r = u mod (2N)`. 

Nếu như`r <= N`, tọa độ x thực tế là`r`. Nếu như`r > N`, tọa độ là`2N - r`. Phép tính tương tự sẽ cho ra tọa độ y một cách độc lập với khoảng thời gian sử dụng`2M`. 

Điều này loại bỏ hoàn toàn việc mô phỏng. Giá trị lớn của`S`và vận tốc được xử lý bằng số học số nguyên, và số lần va chạm vào tường không bao giờ cần phải đếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(S)`mỗi trường hợp thử nghiệm |`O(1)`| Quá chậm | 
| Tối ưu |`O(1)`mỗi trường hợp thử nghiệm |`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`N`,`M`, vị trí ban đầu`(x0, y0)`, vận tốc`(vx, vy)`, và thời gian yêu cầu`S`. Chuyển động x và y có thể được xử lý độc lập vì va chạm thẳng đứng không bao giờ làm thay đổi thành phần y và va chạm ngang không bao giờ làm thay đổi thành phần x. 
2. Đối với tọa độ x, tính vị trí gấp`u = x0 + vx * S`. Điều này thể hiện vị trí của quả bóng nếu không có bức tường thẳng đứng nào cả. 
3. Giảm`u`modulo`2N`, thu được`r`. Khoảng thời gian`[0, 2N]`mô tả một chu kỳ qua lại hoàn chỉnh của tọa độ x. Giảm modulo`2N`xóa mọi chu trình hoàn chỉnh mà không thay đổi vị trí cuối cùng bên trong bảng. 
4. Chuyển đổi`r`trở lại bảng sử dụng quy tắc sóng tam giác. Nếu như`r <= N`, tọa độ là`r`, vì đây là nửa đầu của chu kỳ. Ngược lại tọa độ là`2N - r`, vì nửa sau đang tiến về 0. 
5. Áp dụng chính xác phép tính tương tự cho y, thay thế`x0`,`vx`, Và`N`với`y0`,`vy`, Và`M`. 
6. In kết quả`(x, y)`. Vì cả hai tọa độ đều được tính ở cùng một thời điểm tuyệt đối`S`, va chạm góc đồng thời được xử lý một cách tự nhiên. Tại một góc, cả hai sóng tam giác đều nằm chính xác tại ranh giới tương ứng của chúng. 

### Tại sao nó hoạt động 

Đối với một tọa độ, việc mở bảng sẽ loại bỏ mọi phản chiếu. Quả bóng sau đó tuân theo phương trình tuyến tính đơn giản`x0 + vx*S`. Gấp dòng này lại thành`[0, N]`tái tạo mọi phản xạ vì khoảng độ dài đầu tiên`N`tương ứng với chuyển động về phía bức tường bên phải, trong khi khoảng thứ hai tương ứng với chuyển động phản xạ về phía bức tường bên trái. Mẫu hoàn chỉnh có dấu chấm`2N`, vậy lấy modulo`2N`không mất thông tin. Lập luận tương tự áp dụng độc lập cho y. Do đó, hai tọa độ được xây dựng lại chính xác là vị trí của quả bóng sau`S`giây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def reflected_position(start, velocity, length, time):
    period = 2 * length
    r = (start + velocity * time) % period

    if r <= length:
        return r
    return period - r

def solve():
    t = int(input())

    for _ in range(t):
        N, M, x0, y0, vx, vy, S = map(int, input().split())

        x = reflected_position(x0, vx, N, S)
        y = reflected_position(y0, vy, M, S)

        print(x, y)

if __name__ == "__main__":
    solve()
```các`reflected_position`hàm chứa toàn bộ nghiệm một chiều.`start + velocity * time`tính toán vị trí chưa được gấp và phép toán modulo nén nó thành một chu kỳ của chuyển động nảy. 

Thời kỳ là`2 * length`, không`length`. Một tọa độ mất`length`đơn vị khoảng cách được mở ra để đi từ bức tường này sang bức tường khác, rồi đến bức tường khác`length`các đơn vị để quay trở lại bức tường bắt đầu. Chỉ sử dụng`length`vì dấu chấm sẽ xác định không chính xác một điểm trên đường tới bức tường bên phải với điểm tương ứng trên đường quay lại. 

Sự so sánh`r <= length`cũng là cố ý. Khi`r == length`, quả bóng nằm chính xác trên tường và đó là vị trí hợp lệ tại thời điểm được yêu cầu. Chỉ các giá trị thực sự lớn hơn`length`thuộc nửa phản xạ của chu kỳ. 

Hoạt động modulo của Python đặc biệt thuận tiện cho vận tốc âm. Ví dụ: nếu tọa độ mở rộng là`-1`và thời kỳ là`10`, Python đánh giá`-1 % 10`BẰNG`9`, chính xác là điểm tương đương trong giai đoạn hiện tại. 

Không có nguy cơ tràn số nguyên trong Python. Sản phẩm lớn nhất theo thứ tự`10^18`, mà số nguyên Python xử lý trực tiếp. Trong ngôn ngữ có chiều rộng cố định, cần có loại số nguyên đủ rộng cho`velocity * S`. 

## Ví dụ đã hoạt động 

Đối với trường hợp thử nghiệm mẫu đầu tiên, bảng có chiều rộng`3`và chiều cao`5`, quả bóng bắt đầu lúc`(2, 2)`, chuyển động với vận tốc`(2, 1)`, và chúng ta cần vị trí của nó sau`3`giây. 

| Bước | x mở ra | kỳ x | vị trí x | y mở ra | kỳ y | vị trí y | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 6 | 2 | 2 | 10 | 2 | 
| Sau 3 giây | 8 | 2 | 2 | 5 | 5 | 5 | 

Đối với x, tọa độ mở rộng là`2 + 2*3 = 8`. Giảm modulo`6`cho`2`, vậy quả bóng đã quay về tọa độ x`2`. Đối với y, tọa độ mở rộng là`2 + 1*3 = 5`, chính xác là bức tường phía trên, nên tọa độ y vẫn giữ nguyên`5`vào thời điểm được yêu cầu. Vị trí kết quả là`(2, 5)`, phù hợp với mẫu 

Đối với trường hợp thử nghiệm mẫu thứ hai, bảng là`6 x 8`, vị trí ban đầu là`(3, 2)`, vận tốc là`(5, 1)`, Và`S = 1`. 

| Bước | x mở ra | kỳ x | vị trí x | y mở ra | kỳ y | vị trí y | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 12 | 3 | 2 | 16 | 2 | 
| Sau 1 giây | 8 | 8 | 4 | 3 | 3 | 3 | 

Tọa độ x mở rộng là`8`. Vì chiều rộng của bảng là`6`, nửa sau của khoảng thời gian đang hoạt động, do đó tọa độ thực tế là`12 - 8 = 4`. Tọa độ y là`3`, vẫn còn ở trong bảng. Vị trí cuối cùng là`(4, 3)`. 

Hai dấu vết này thể hiện cả hai mặt của sóng tam giác. Thử nghiệm đầu tiên tiếp đất chính xác trên tường, trong khi thử nghiệm thứ hai đạt đến nửa phản xạ của chu kỳ tọa độ x. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(T)`| Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi. | 
| Không gian |`O(1)`| Chỉ một số biến số nguyên cố định được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Với nhiều nhất`10^4`trường hợp thử nghiệm, thuật toán chỉ thực hiện một vài phép tính số nguyên cho mỗi trường hợp. Điều này dễ dàng tương thích với giới hạn một giây và mức sử dụng bộ nhớ không tăng theo`S`, kích thước của bàn hoặc số lần va chạm vào tường. 

## Trường hợp thử nghiệm```python
import sys
import io

def reflected_position(start, velocity, length, time):
    period = 2 * length
    r = (start + velocity * time) % period
    if r <= length:
        return r
    return period - r

def solve_data(data):
    inp = io.StringIO(data)
    t = int(inp.readline())
    out = []

    for _ in range(t):
        N, M, x0, y0, vx, vy, S = map(int, inp.readline().split())
        x = reflected_position(x0, vx, N, S)
        y = reflected_position(y0, vy, M, S)
        out.append(f"{x} {y}")

    return "\n".join(out) + "\n"

# Provided sample
sample = """\
3
3 5 2 2 2 1 3
6 8 3 2 5 1 1
100 200 13 45 -20 111 9969
"""
assert solve_data(sample) == """\
2 5
4 3
33 196
""", "provided sample"

# Minimum-size table, exact wall hit, and zero velocity component.
case_min = """\
3
2 2 1 1 1 0 1
2 2 1 1 1 0 2
2 2 1 1 0 0 1000000000
"""
assert solve_data(case_min) == """\
2 1
1 1
1 1
""", "minimum dimensions and zero velocity"

# Negative velocity.
case_negative = """\
1
5 4 2 1 -3 0 1
"""
assert solve_data(case_negative) == """\
1 1
""", "negative velocity"

# Exact corner hit.
case_corner = """\
1
3 5 2 4 1 1 1
"""
assert solve_data(case_corner) == """\
3 5
""", "corner collision"

# Maximum-scale values.
case_max = """\
1
1000000000 1000000000 1 999999999 1000000000 -1000000000 1000000000
"""
assert solve_data(case_max) == """\
1 999999999
""", "maximum values"

# Equal dimensions, equal positions, equal velocities, and repeated reflection.
case_equal = """\
1
10 10 3 3 4 4 3
"""
assert solve_data(case_equal) == """\
8 8
""", "equal values and reflection"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1 1 1 0 1`và các vụ việc liên quan |`2 1`,`1 1`,`1 1`| Kích thước tối thiểu, ranh giới chính xác và vận tốc bằng không | 
|`5 4 2 1 -3 0 1`|`1 1`| Modulo âm và vận tốc âm | 
|`3 5 2 4 1 1 1`|`3 5`| Va chạm đồng thời với hai bức tường ở một góc | 
|`1000000000 1000000000 1 999999999 1000000000 -1000000000 1000000000`|`1 999999999`| Giới hạn số tối đa và sản phẩm rất lớn | 
|`10 10 3 3 4 4 3`|`8 8`| Chuyển động x và y đối xứng và phản xạ lặp đi lặp lại | 

## Vỏ cạnh 

Bóng rơi chính xác vào tường không được phản ánh trước khi vị trí yêu cầu của nó được báo cáo. Vì```
1
5 4 2 1 3 0 1
```tọa độ x mở rộng là`5`, và chu kỳ là`10`. Từ`5 <= 5`, tọa độ phản ánh vẫn là`5`. Đầu ra là`5 1`. Điều kiện phải là`r <= length`, không`r < length`. 

Vận tốc âm được xử lý theo cùng một công thức. Vì```
1
5 4 2 1 -3 0 1
```tọa độ x mở rộng là`-1`. modulo còn lại của nó`10`là`9`, và bởi vì`9 > 5`, tọa độ thực tế trở thành`10 - 9 = 1`. Đầu ra là`1 1`. Không cần mô phỏng riêng biệt việc di chuyển về phía bức tường bên trái. 

Một va chạm ở góc được tự động thể hiện bằng việc cả hai tọa độ đều đạt đến ranh giới trên của chúng cùng một lúc. Vì```
1
3 5 2 4 1 1 1
```tọa độ mở ra là`3`Và`5`. Chu kỳ của họ là`6`Và`10`và cả hai phần dư đều bằng độ dài bảng tương ứng của chúng. Kết quả là chính xác`3 5`. Nếu phép tính phản ánh ngay sau khi chạm vào tường, nó sẽ trả lời vị trí ở thời điểm tiếp theo thay vì vị trí được yêu cầu. 

Thành phần vận tốc bằng 0 không yêu cầu nhánh đặc biệt nào trong thuật toán chính. Vì```
1
7 6 3 4 0 0 1000000000
```tọa độ chưa được mở vẫn còn`3`Và`4`bất kể thời gian. Lấy modulo tương ứng và gấp lại để chúng không thay đổi, tạo ra`3 4`. Đây là một lý do tại sao công thức tọa độ trải rộng được ưu tiên hơn các công thức đếm va chạm có thể cố gắng chia cho vận tốc. 

Các giá trị lớn nhất cũng đòi hỏi phải chú ý đến số học. Với vận tốc và thời gian xung quanh`10^9`, sản phẩm của họ có thể tiếp cận`10^18`. Python xử lý việc này một cách chính xác, vì vậy biểu thức`start + velocity * time`là an toàn. Trong các ngôn ngữ có số nguyên 32 bit, biểu thức tương tự sẽ tràn, do đó cần phải có loại số nguyên 64 bit.
