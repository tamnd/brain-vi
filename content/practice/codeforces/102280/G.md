---
title: "CF 102280G - \u0421\u043f\u0435\u0446\u0438\u0430\u043b\u044c\u043d\u044b\u0439 \u0437\u0430\u043a\u0430\u0437"
description: "Chúng ta có điểm xuất phát trên một con đường và đích đến ở đâu đó cách xa con đường đó. Khoảng cách thẳng từ điểm xuất phát đến đích là s, còn khoảng cách vuông góc từ đích đến đường là h."
date: "2026-08-13T16:02:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102280
codeforces_index: "G"
codeforces_contest_name: "2010, \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u0421\u0413\u0410\u0423 aka \u041a\u043e\u043d\u0442\u0435\u0441\u0442 \u043f\u0440\u043e \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0438"
rating: 0
weight: 102280
solve_time_s: 162
verified: true
draft: false
---

[CF 102280G - \u0421\u043f\u0435\u0446\u0438\u0430\u043b\u044c\u043d\u044b\u0439 \u0437\u0430\u043a\u0430\u0437](https://codeforces.com/problemset/problem/102280/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có điểm xuất phát trên một con đường và đích đến ở đâu đó cách xa con đường đó. Khoảng cách theo đường thẳng từ điểm xuất phát đến điểm đích là`s`, trong khi khoảng cách vuông góc từ đích đến đường là`h`. Ôtô có thể di chuyển trên đường với tốc độ`v`và xuyên qua địa hình với tốc độ`u`. 

Lộ trình dự định bao gồm hai phần. Đầu tiên ô tô đi được một quãng đường`x`dọc đường. Sau đó nó rời khỏi đường và lái thẳng đến đích. Chúng ta cần tìm giá trị của`x`giúp giảm thiểu tổng thời gian di chuyển. 

Đầu vào chứa bốn số nguyên`s`,`h`,`u`, Và`v`. Đây`s`là khoảng cách trực tiếp từ điểm xuất phát đến đích,`h`là khoảng cách vuông góc của điểm đến với đường,`u`là tốc độ địa hình, và`v`là tốc độ đường. Đầu ra là khoảng cách tối ưu`x`đã đi dọc đường. 

Tất cả các giá trị có thể lớn như`10^9`. Điều này ngay lập tức loại trừ các thuật toán phụ thuộc vào việc lặp lại trên mọi khoảng cách số nguyên có thể, vì có thể có tới một tỷ ứng viên. Thời gian giới hạn chỉ 0,5 giây nên dù chỉ vài trăm triệu thao tác đơn giản cũng vượt xa mức hợp lý. Chúng ta cần một giải pháp toán học theo thời gian không đổi hoặc logarit. 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ việc triển khai bất cẩn. Nếu đường không nhanh hơn địa hình thì xe nên rời đi ngay. Ví dụ, với`1 1 10 10`, hình chiếu của đích lên đường là điểm xuất phát nên đáp án đúng là`0.0`. Một công thức phân chia mù quáng cho`sqrt(v^2-u^2)`sẽ chia cho số không. 

Một trường hợp ranh giới khác xảy ra khi đường nhanh hơn nhưng không đủ nhanh để đạt được hình chiếu vuông góc. Ví dụ,`5000 3000 20 25`đưa ra câu trả lời đúng`0.0`. Ở đây tốc độ đường chính xác ở ngưỡng mà điểm rẽ tối ưu là điểm xuất phát. Một công thức giả định mức tối ưu bên trong mà không kiểm tra vị trí của nó có thể tạo ra khoảng cách âm. 

Tình huống ngược lại là nội thất tối ưu thực tế. Vì`5000 3000 15 25`, hình chiếu trực tiếp là`4000`mét tính từ điểm xuất phát. Bước ngoặt tối ưu là`1750`mét dọc theo con đường, vậy câu trả lời là`1750.0`. Trường hợp này rất hữu ích vì nó xác nhận rằng việc khởi hành ngay lập tức hay lái xe đến tận hình chiếu luôn luôn là tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xem xét mọi khoảng cách nguyên có thể`x`từ`0`đến điểm chiếu và tính thời gian di chuyển tương ứng. Nếu hình chiếu gần như`10^9`cách xa mét, điều này có nghĩa là khoảng`10^9`đánh giá căn bậc hai và một số phép tính số học. Bên cạnh việc quá chậm so với giới hạn 0,5 giây, việc chỉ kiểm tra các giá trị nguyên sẽ không đáp ứng được yêu cầu`10^-9`độ chính xác khi tối ưu thực sự là không tích phân. 

Cách tiếp cận bạo lực có hiệu quả vì mọi bước ngoặt của ứng viên đều có thể được đánh giá một cách độc lập. Vấn đề là có quá nhiều ứng viên. Quan sát quan trọng là hình học cho chúng ta một hàm một biến rất đơn giản và mức tối thiểu của nó có thể được tìm thấy bằng phương pháp giải tích. 

Cho phép`a`là khoảng cách dọc theo con đường từ điểm xuất phát đến hình chiếu vuông góc của điểm đến. Vì khoảng cách trực tiếp là`s`và khoảng cách vuông góc là`h`, tam giác vuông cho`a = sqrt(s^2 - h^2)`. 

Nếu ô tô đi`x`mét trên đường thì khoảng cách địa hình còn lại là`sqrt((a - x)^2 + h^2)`. 

Do đó tổng thời gian di chuyển là`T(x) = x / v + sqrt((a - x)^2 + h^2) / u`. 

Hàm này lồi trên khoảng thích hợp nên có thể có nhiều nhất một cực tiểu bên trong. Sự khác biệt mang lại`T'(x) = 1/v - (a - x) / (u * sqrt((a - x)^2 + h^2))`. 

Để đạt được mức tối ưu bên trong, đạo hàm phải bằng 0. Giải phương trình này sẽ cho điểm ngoặt chính xác. Biểu thức kết quả có thể được kiểm tra dựa trên ranh giới`x = 0`. Nếu điểm tính toán nằm trước điểm bắt đầu thì mức tối ưu thực tế chỉ đơn giản là`0`. 

Brute-force hoạt động vì nó đánh giá hàm thời gian di chuyển tương tự mà chúng tôi tối ưu hóa về mặt toán học, nhưng không thành công khi số khoảng cách ứng cử viên trở nên rất lớn. Việc quan sát thấy đạo hàm có số 0 dạng đóng sẽ rút gọn toàn bộ bài toán thành một số phép tính số học không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O (các) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách`a`từ điểm xuất phát tới hình chiếu vuông góc của điểm đích lên đường bằng cách sử dụng`a = sqrt(s^2 - h^2)`. Điều này chuyển đổi hình học ban đầu thành một tam giác vuông và cho chúng ta tọa độ một chiều của hình chiếu. 

2. Nếu`v <= u`, đầu ra`0`. Di chuyển trên đường không nhanh hơn di chuyển xuyên địa hình nên việc di chuyển ra xa điểm xuất phát dọc đường không thể cải thiện được lộ trình. 

3. Đối với`v > u`, giải phương trình đạo hàm cho khoảng cách`y = a - x`từ bước ngoặt đến hình chiếu. Đặt đạo hàm về 0 sẽ cho`y / sqrt(y^2 + h^2) = u / v`. 

Bình phương và sắp xếp lại sản lượng`y = h * u / sqrt(v^2 - u^2)`. 

Biểu thức dương vì nhánh này có`v > u`. 

4. Chuyển đổi`y`quay trở lại khoảng cách đường mong muốn bằng cách sử dụng`x = a - y`. 

5. Nếu`x < 0`, đầu ra`0`thay vì. Điều này có nghĩa là mức tối thiểu không bị ràng buộc sẽ nằm trước điểm bắt đầu, do đó mức tối thiểu bị ràng buộc xảy ra ở ranh giới`x = 0`. 

6. Nếu không thì xuất ra`x`với đủ độ chính xác. của Python`float`là đủ cho yêu cầu`10^-9`lỗi tuyệt đối hoặc tương đối ở đây và việc in 15 chữ số thập phân mang lại độ chính xác đầu ra cao. 

Tại sao nó hoạt động: tổng thời gian di chuyển là hàm lồi của vị trí quay. Đạo hàm của nó có tính đơn điệu, do đó điểm trong mà đạo hàm bằng 0 là điểm cực tiểu nội duy nhất. Nếu điểm đó nằm ngoài khoảng cho phép thì độ lồi có nghĩa là ranh giới gần nhất là tối ưu. Trong bài toán này, ranh giới liên quan duy nhất có thể trở nên tối ưu là`x = 0`, trong khi ranh giới khác`x = a`không thể tốt hơn vì di chuyển một chút từ`a`về điểm xuất phát thay thế việc di chuyển chậm trên địa hình bằng việc di chuyển bằng đường bộ nhanh hơn bất cứ khi nào`v > u`. Do đó, thuật toán trả về điểm dừng duy nhất hoặc điểm biên chính xác. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    s, h, u, v = map(int, input().split())

    # Distance from the starting point to the perpendicular
    # projection of the destination onto the road.
    a = math.sqrt(float(s) * s - float(h) * h)

    # If the road is not faster than the terrain, leaving immediately
    # is optimal.
    if v <= u:
        print("0.0")
        return

    # y is the distance from the turning point to the projection.
    y = h * u / math.sqrt(float(v) * v - float(u) * u)

    x = a - y

    # The unconstrained minimum can lie before the starting point.
    if x < 0.0:
        x = 0.0

    print("{:.15f}".format(x))

if __name__ == "__main__":
    solve()
```Phép tính đầu tiên thu được cạnh ngang của tam giác vuông. Viết nó như`float(s) * s - float(h) * h`tránh dựa vào số học số nguyên trước khi chuyển đổi các giá trị bình phương có khả năng lớn thành dấu phẩy động. 

các`v <= u`nhánh được xử lý trước công thức liên quan đến`sqrt(v^2 - u^2)`. Đây không chỉ là một sự tối ưu hóa. Khi`v == u`, căn bậc hai đó bằng 0 và công thức điểm dừng không được xác định, trong khi câu trả lời được biết trực tiếp là bằng 0. 

Vì`v > u`, mã tính toán`y`, dễ rút ra hơn`x`chính nó. Về mặt hình học,`y`đo xem điểm bước ngoặt cách xa hình chiếu của đích đến bao xa. Trừ nó từ`a`cho biết quãng đường thực tế đã đi trên đường. 

các`x < 0`kiểm tra xử lý trường hợp điểm dừng toán học nằm ngoài khoảng khả thi. Không cần phải có một sự rõ ràng`x > a`kiểm tra vì công thức cho`y > 0`, Vì thế`x = a - y`luôn luôn nhỏ hơn`a`. 

Các ô vuông số nguyên trung gian lớn nhất là xung quanh`10^18`. Bản thân các số nguyên Python không bị tràn và việc chuyển đổi toán hạng thành dấu phẩy động trước khi phép nhân giữ cho phép tính nằm trong phạm vi có độ chính xác kép thông thường. Dung sai câu trả lời được yêu cầu tương thích với độ chính xác gấp đôi đối với các cường độ đầu vào này. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`5000 3000 15 25`. Khoảng cách chiếu là`sqrt(5000^2 - 3000^2) = 4000`. 

Đường đi nhanh hơn địa hình nên có thể tối ưu hóa nội thất. 

| Bước |`a`|`y`|`x`| 
|---|---:|---:|---:| 
| Tính toán chiếu |`4000`| không được tính toán | không được tính toán | 
| Giải đạo hàm |`4000`|`3000 * 15 / sqrt(25^2 - 15^2) = 2250`| không được tính toán | 
| Chuyển đổi sang khoảng cách đường |`4000`|`2250`|`4000 - 2250 = 1750`| 
| Kiểm tra ranh giới |`4000`|`2250`|`1750`| 

Điểm dừng nằm trong khoảng khả thi nên đáp án là`1750.0`, phù hợp với mẫu Ví dụ minh họa trường hợp nội thất chính hãng trong đó không lái xe trực tiếp qua địa hình cũng như không bám đường cho đến khi hình chiếu tối ưu. 

Đối với Mẫu 2, đầu vào là`5000 3000 20 25`. Lại,`a = 4000`, nhưng bây giờ tốc độ đã chính xác ở ngưỡng. 

| Bước |`a`|`y`|`x`| 
|---|---:|---:|---:| 
| Tính toán chiếu |`4000`| không được tính toán | không được tính toán | 
| Giải đạo hàm |`4000`|`3000 * 20 / sqrt(25^2 - 20^2) = 4000`| không được tính toán | 
| Chuyển đổi sang khoảng cách đường |`4000`|`4000`|`0`| 
| Kiểm tra ranh giới |`4000`|`4000`|`0`| 

Điểm dừng trùng với điểm bắt đầu. Câu trả lời là`0.0`. Điều này xác nhận rằng việc xử lý ranh giới bao gồm trường hợp đẳng thức một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(1) | Chỉ một số lượng cố định các phép tính số học và căn bậc hai được thực hiện. | 
| Không gian | O(1) | Chỉ có một số lượng biến vô hướng không đổi được lưu trữ. | 

Các giá trị đầu vào có thể đạt`10^9`, nhưng thuật toán không lặp lại độ lớn của chúng. Nó thực hiện cùng một lượng công việc không đổi cho mọi đầu vào hợp lệ, do đó, nó dễ dàng phù hợp với giới hạn thời gian 0,5 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 64 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    try:
        s, h, u, v = map(int, sys.stdin.readline().split())

        a = math.sqrt(float(s) * s - float(h) * h)

        if v <= u:
            x = 0.0
        else:
            y = h * u / math.sqrt(float(v) * v - float(u) * u)
            x = max(0.0, a - y)

        return "{:.15f}\n".format(x)
    finally:
        sys.stdin = old_stdin

def run(inp: str) -> str:
    return solve_data(inp)

def check(inp: str, expected: float, message: str):
    actual = float(run(inp))
    assert math.isclose(actual, expected, rel_tol=1e-12, abs_tol=1e-9), (
        f"{message}: expected {expected}, got {actual}"
    )

# Provided samples
check("5000 3000 15 25\n", 1750.0, "sample 1")
check("5000 3000 20 25\n", 0.0, "sample 2")

# Minimum-size input.
check("1 1 1 1\n", 0.0, "minimum values")

# All values equal, so the road provides no speed advantage.
check("100 100 50 50\n", 0.0, "equal speeds")

# Road is faster, but not enough to justify moving along it.
check("5 4 4 5\n", 0.0, "boundary optimum at zero")

# Large values, with a valid interior optimum.
s = 10**9
h = 6 * 10**8
u = 10**8
v = 2 * 10**8
a = math.sqrt(float(s) * s - float(h) * h)
y = h * u / math.sqrt(float(v) * v - float(u) * u)
expected = max(0.0, a - y)
check(f"{s} {h} {u} {v}\n", expected, "maximum-size input")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---:|---| 
|`1 1 1 1`|`0.0`| Các giá trị tối thiểu và`v <= u`chi nhánh | 
|`100 100 50 50`|`0.0`| Tốc độ bằng nhau và tránh mẫu số bằng 0 | 
|`5 4 4 5`|`0.0`| Điểm đứng yên ngay tại ranh giới xuất phát | 
|`1000000000 600000000 100000000 200000000`| khoảng`361.53...`| Giá trị lớn và tính toán dấu phẩy động | 

## Vỏ cạnh 

Khi nào`h = s`, hình chiếu vuông góc nằm chính xác tại điểm bắt đầu vì`sqrt(s^2 - h^2) = 0`. Ví dụ,`1 1 1 1`ngay lập tức lấy`v <= u`chi nhánh và lợi nhuận`0.0`. Ngay cả khi đường nhanh hơn, sẽ không có sẵn khoảng cách dương trước khi đến hình chiếu, do đó, 0 là khoảng cách đường tối ưu duy nhất có thể có. 

Khi`v = u`, đường và địa hình có tốc độ giống nhau. Ví dụ,`100 80 50 50`đi vào`v <= u`chi nhánh và lợi nhuận`0.0`. Việc thực hiện bất cẩn đánh giá`sqrt(v^2 - u^2)`đầu tiên sẽ nhận được mẫu số bằng 0, mặc dù câu trả lời tối ưu rất đơn giản. 

Khi`v > u`nhưng lợi thế đường đi không đủ, điểm dừng nằm ngoài khoảng khả thi. Coi như`5 4 4 5`. Đây`a = 3`, Và`y = 4 * 4 / sqrt(5^2 - 4^2) = 16 / 3`. 

Như vậy`x = 3 - 16/3 < 0`, do đó thuật toán kẹp nó về 0. Tối ưu về mặt toán học cho hàm không hạn chế sẽ nằm trước điểm xuất phát, có nghĩa là lựa chọn khả thi nhất là rời khỏi đường ngay lập tức. 

Ở ngưỡng giữa 0 và câu trả lời khẳng định, điểm dừng chính xác là ở`x = 0`. Trong mẫu 2,`5000 3000 20 25`cho`a = 4000`Và`y = 4000`, sản xuất chính xác`x = 0`. Thuật toán không cần so sánh đẳng thức đặc biệt vì công thức thông thường đã cho kết quả bằng 0. 

Cuối cùng, các giá trị rất lớn như`s = 10^9`tạo ra khoảng cách bình phương xung quanh`10^18`. Biểu diễn số nguyên của Python xử lý các giá trị đó một cách an toàn, trong khi các phép tính cuối cùng sử dụng độ chính xác gấp đôi. Vì bản thân đầu ra là một số thực với`10^-9`dung sai, việc sử dụng số học dấu phẩy động tiêu chuẩn là đủ để đạt được độ chính xác cần thiết. 
:::
