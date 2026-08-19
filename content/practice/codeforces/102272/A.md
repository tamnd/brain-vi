---
title: "CF 102272A - Ch\u01a1i Bi-a"
description: "Chúng ta có một bàn carom hình chữ nhật có kích thước ngang là (N) và kích thước dọc là (M). Quả bóng bắt đầu ở vị trí nguyên ((x0,y0)), nằm ngay bên trong bàn và trong mỗi giây, vị trí của nó thay đổi theo vận tốc hiện tại ((vx,vy))."
date: "2026-08-19T05:08:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102272
codeforces_index: "A"
codeforces_contest_name: "HCW 19 Individual Day 1"
rating: 0
weight: 102272
solve_time_s: 375
verified: false
draft: false
---

[CF 102272A - Ch\u01a1i Bi-a](https://codeforces.com/problemset/problem/102272/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn carom hình chữ nhật có kích thước ngang là (N) và kích thước dọc là (M). Quả bóng bắt đầu ở vị trí nguyên ((x_0,y_0)), nằm ngay bên trong bàn và trong mỗi giây, vị trí của nó thay đổi theo vận tốc hiện tại của nó ((v_x,v_y)). Khi quả bóng chạm vào một bức tường thẳng đứng, dấu (v_x) thay đổi. Khi nó chạm vào một bức tường nằm ngang thì dấu (v_y) thay đổi. Nếu cả hai xảy ra ở cùng một góc thì cả hai dấu hiệu đều thay đổi. 

Nhiệm vụ là tìm vị trí chính xác của quả bóng sau (S) giây. Các thành phần vận tốc có thể âm và có thể lớn bằng (10^9), trong khi (S) cũng có thể bằng (10^9). Có thể có tới (10^4) trường hợp thử nghiệm. 

Các giá trị lớn ngay lập tức loại trừ việc mô phỏng quả bóng từng giây một. Một thử nghiệm duy nhất có thể yêu cầu (10^9) lần lặp và (10^4) các thử nghiệm như vậy sẽ cho ra tới (10^{13}) lần lặp. Mặc dù mỗi lần lặp lại đơn giản nhưng điều đó vượt xa giới hạn thời gian một giây. Chúng ta cần một phép tính thời gian không đổi cho mỗi tọa độ. 

Hai tọa độ độc lập. Chuyển động theo phương ngang chỉ phụ thuộc vào (N,x_0,v_x,S) và chuyển động theo chiều dọc chỉ phụ thuộc vào (M,y_0,v_y,S). Vì vậy, vấn đề chính giảm xuống việc hiểu chuyển động một chiều giữa hai bức tường phản xạ tại (0) và (L). 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ quá trình triển khai trực tiếp. Đầu tiên là một quả bóng rơi chính xác vào một bức tường. Ví dụ,```
1
3 4 1 2 2 0 1
```đưa ra vị trí ((3,2)). Bóng chạm tới bức tường bên phải đúng thời điểm (1), nên đáp án là`3 2`. Việc thực hiện bất cẩn, ngay lập tức thay đổi lại vị trí sau khi phát hiện va chạm có thể vô tình quay trở lại`1 2`. 

Trường hợp thứ hai là vận tốc âm. Ví dụ,```
1
5 4 2 2 -3 -2 1
```đưa ra tọa độ chưa mở (-1) và (0). Vị trí phản ánh là (1) và (0), do đó đầu ra đúng là`1 0`. Việc triển khai sử dụng một ngôn ngữ trong đó`%`giữ dấu của số bị chia phải chuẩn hóa số dư âm. của Python`%`đã trả về phần dư không âm nên công thức trực tiếp là an toàn. 

Trường hợp thứ ba xảy ra khi cả hai tọa độ đều chạm tới tường cùng một lúc. Ví dụ,```
1
3 3 1 1 2 2 1
```di chuyển trực tiếp đến góc trên bên phải, đưa ra`3 3`. Va chạm ở góc đảo ngược cả hai thành phần vận tốc, nhưng sự đảo ngược đó ảnh hưởng đến chuyển động trong tương lai chứ không phải vị trí tại thời điểm va chạm. 

Cuối cùng, vận tốc bằng không trong một tọa độ phải bằng không. Ví dụ,```
1
2 5 1 3 0 2 3
```giữ nguyên (x=1), trong khi tọa độ dọc di chuyển từ (3) đến (9) trong biểu diễn mở rộng. Vì (9\bmod 10=9), phép phản xạ cho (1), nên câu trả lời là`1 1`. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là mô phỏng từng giây. Ở mỗi bước, chúng tôi thêm vận tốc hiện tại vào vị trí, kiểm tra xem tọa độ có chạm vào tường hay không và đảo ngược thành phần vận tốc tương ứng. Điều này đúng vì nó tuân theo chính xác các quy luật vật lý của bài toán. 

Vấn đề là giá trị của (S). Trong trường hợp xấu nhất, một bài kiểm tra yêu cầu (10^9) giây mô phỏng. Với (10^4) thử nghiệm, số lượng thao tác trong trường hợp xấu nhất nằm ở thứ tự (10^{13}), không thể vừa với giới hạn thời gian. 

Quan sát quan trọng là sự phản chiếu có thể được loại bỏ bằng cách tưởng tượng một đường thẳng lớn hơn, trải rộng hơn. Chỉ xét tọa độ (x) và đặt chiều rộng của bảng là (N). Thay vì phản chiếu quả bóng tại (0) và (N), hãy tưởng tượng rằng đường thẳng đó kéo dài mãi mãi: 

[ 
\ldots,-2N,-N,0,N,2N,3N,\ldots 
] 

Quả bóng chỉ đơn giản di chuyển dọc theo đường vô hạn này với vận tốc ban đầu của nó. Mỗi khoảng có độ dài (2N) tương ứng với một chuyển động qua lại hoàn chỉnh bên trong bảng thực. 

Sau (S) giây, tọa độ mở ra là 

[ 
p=x_0+v_xS. 
] 

Chỉ có vị trí modulo (2N) của nó là quan trọng. hãy để 

[ 
r=p\bmod 2N, 
] 

với (0\le r<2N). Nếu (r\le N), tọa độ thực là (r). Nếu (r>N), quả bóng nằm trên nửa phản xạ của khoảng trải ra, do đó tọa độ thực là (2N-r). 

Do đó, sự phản xạ một chiều có thể được tính toán theo thời gian không đổi. Công thức tương tự hoạt động độc lập cho (y), thay thế (N) bằng (M). 

Đây thực chất là một sóng tam giác định kỳ. Chuyển động nảy có chu kỳ (2N) trong tọa độ trải rộng, do đó modulo (2N) nắm bắt hoàn toàn mọi số lần va chạm vào tường có thể xảy ra mà không cần mô phỏng rõ ràng bất kỳ lần va chạm nào trong số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(S)) mỗi lần kiểm tra | (O(1)) | Quá chậm | 
| Tối ưu | (O(1)) mỗi lần kiểm tra | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N,M,x_0,y_0,v_x,v_y,S). Vị trí cuối cùng có thể được tính toán độc lập cho hai tọa độ, do đó không cần mô phỏng va chạm giữa chúng. 
2. Đối với tọa độ ngang, tính vị trí gấp 

[ 
p_x=x_0+v_xS. 
] 

Điều này thể hiện vị trí của quả bóng sau (S) giây nếu các bức tường thẳng đứng không phản chiếu nó. 

1. Giảm vị thế này theo modulo trong toàn bộ thời gian mở ra: 

[ 
r_x=p_x\bmod 2N. 
] 

Việc sử dụng (2N), thay vì (N), là cần thiết vì chuyển động từ (0) đến (N) và quay lại (0) tạo thành mẫu lặp lại hoàn chỉnh. 

1. Phản ánh tọa độ đã giảm trở lại bảng thực tế. Nếu (r_x\le N), đặt (x_S=r_x). Nếu không thì đặt 

[ 
x_S=2N-r_x. 
] 

Giá trị tương tự xuất hiện hai lần ở hai đầu của khoảng mở ra, thể hiện chính xác vị trí của bức tường. 

1. Lặp lại phép tính tương tự theo chiều dọc. Tính toán 

[ 
p_y=y_0+v_yS, 
] 

sau đó 

[ 
r_y=p_y\bmod 2M, 
] 

và cuối cùng sử dụng (r_y) khi (r_y\le M), nếu không thì sử dụng (2M-r_y). 

1. In (x_S) và (y_S). Hai phép tính này độc lập, kể cả khi cả hai tọa độ chạm vào tường cùng một lúc. 

### Tại sao nó hoạt động 

Đối với một tọa độ, hãy tưởng tượng việc thay thế mọi bản sao phản ánh của bảng bằng một bản sao khác đặt bên cạnh nó. Quả bóng sau đó chuyển động mãi mãi theo một đường thẳng với vận tốc không đổi. Việc gấp đường vô hạn này trở lại khoảng ban đầu sẽ tái tạo chính xác mọi phản xạ: vượt qua bội số của (N) sẽ đảo ngược hướng sau khi gấp. 

Mẫu chưa mở lặp lại mỗi (2N), do đó, hai vị trí chưa mở khác nhau bội số của (2N) luôn xếp vào cùng một tọa độ bảng. Do đó, việc lấy vị trí modulo (2N) sẽ loại bỏ một số lượng hành trình tới lui hoàn chỉnh tùy ý. Gấp giá trị còn lại với (r) cho nửa đầu và (2N-r) cho nửa sau sẽ cho vị trí vật lý chính xác. 

Vì tọa độ ngang và tọa độ dọc tuân theo cùng một đối số độc lập nên việc áp dụng phép biến đổi cho cả hai tọa độ sẽ tạo ra vị trí chính xác của quả bóng sau (S) giây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def reflected_position(length, start, velocity, seconds):
    period = 2 * length
    pos = (start + velocity * seconds) % period

    if pos <= length:
        return pos
    return period - pos

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, m, x0, y0, vx, vy, s = map(int, input().split())

        x = reflected_position(n, x0, vx, s)
        y = reflected_position(m, y0, vy, s)

        out.append(f"{x} {y}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`reflected_position`hàm chứa toàn bộ phép biến đổi một chiều từ thuật toán. Đầu tiên nó tạo thành vị trí mở ra`start + velocity * seconds`. Số nguyên Python có độ chính xác tùy ý, do đó sản phẩm có thể đạt tới khoảng (10^{18}) một cách an toàn mà không bị tràn. 

Modulo được thực hiện bởi`period = 2 * length`. Python đảm bảo rằng kết quả của`%`nằm trong khoảng từ (0) đến`period - 1`, ngay cả khi vị trí ban đầu là âm. Đó là lý do tại sao trường hợp vận tốc âm không cần nhánh đặc biệt. 

Điều kiện là`pos <= length`, không`pos < length`. Nếu quả bóng tiếp đất chính xác trên một bức tường thì tọa độ đó đã là vị trí vật lý chính xác. Ví dụ, khi`pos == length`, trở về`length`là hoàn toàn đúng. 

Không cần cập nhật vận tốc sau khi tìm thấy va chạm. Chúng ta chỉ cần vị trí tại một thời điểm xác định và mô hình chưa được mở đã mã hóa mọi sự đảo chiều vận tốc thông qua sự phản xạ. 

Đầu vào được xử lý bằng`sys.stdin.readline`, và các câu trả lời sẽ được tích lũy trước lần viết cuối cùng. Với tối đa (10^4) trường hợp thử nghiệm, điều này giúp giảm chi phí I/O. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu đầu tiên,```
3 5 2 2 2 1 3
```kích thước bàn ngang là (3), do đó khoảng thời gian mở ra là (6). Kích thước bảng dọc là (5) nên chu kỳ của nó là (10). 

| Tọa độ | Bắt đầu | Vận tốc | Thời gian | Vị trí mở ra | Thời kỳ modulo | Vị trí phản ánh | 
| --- | --- | --- | --- | --- | --- | --- | 
| (x) | 2 | 2 | 3 | 8 | 2 | 2 | 
| (y) | 2 | 1 | 3 | 5 | 5 | 5 | 

Câu trả lời là`2 5`. Về mặt vật lý, quả bóng di chuyển theo chiều ngang từ (2) đến bức tường bên phải tại (3), quay lại (1) tại thời điểm (2), rồi đến (2) tại thời điểm (3). Phép tính mở ra sẽ nhận được kết quả tương tự mà không cần mô phỏng những va chạm đó. 

Đối với trường hợp mẫu thứ hai,```
6 8 3 2 5 1 1
```chu kỳ ngang là (12), trong khi chu kỳ dọc là (16). 

| Tọa độ | Bắt đầu | Vận tốc | Thời gian | Vị trí mở ra | Thời kỳ modulo | Vị trí phản ánh | 
| --- | --- | --- | --- | --- | --- | --- | 
| (x) | 3 | 5 | 1 | 8 | 8 | 4 | 
| (y) | 2 | 1 | 1 | 3 | 3 | 3 | 

Vị trí mở ngang là (8). Vì bảng kết thúc ở (6) nên tọa độ phản ánh là (12-8=4). Tọa độ dọc chưa tới tường nên giữ nguyên (3). Câu trả lời kết quả là`4 3`. 

Những dấu vết này cho thấy tại sao sự phản xạ phải xảy ra sau khi lấy modulo (2L), chứ không phải modulo (L). Modulo (L) sẽ mất đi sự phân biệt giữa việc di chuyển về phía bức tường và di chuyển trở lại từ nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T)) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi. | 
| Không gian | (O(T)) | Các chuỗi đầu ra được lưu trữ trước khi được viết. Không gian làm việc cho mỗi bài kiểm tra là (O(1)). | 

Với (T\le10^4), thuật toán chỉ thực hiện một vài phép tính số nguyên cho mỗi trường hợp thử nghiệm. Mặc dù (N,M,v_x,v_y,S) có thể làm cho tích trung gian rất lớn, Python xử lý trực tiếp các số nguyên này và số lượng phép tính số học vẫn không đổi. Giải pháp này tránh được mô phỏng bước (10^9) một cách thoải mái mà giới hạn ban đầu không thể thực hiện được. 

## Trường hợp thử nghiệm```python
import sys
import io

def reflected_position(length, start, velocity, seconds):
    period = 2 * length
    pos = (start + velocity * seconds) % period

    if pos <= length:
        return pos
    return period - pos

def solve_input(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        output = io.StringIO()
        sys.stdout = output

        t = int(sys.stdin.readline())
        ans = []

        for _ in range(t):
            n, m, x0, y0, vx, vy, s = map(
                int, sys.stdin.readline().split()
            )

            x = reflected_position(n, x0, vx, s)
            y = reflected_position(m, y0, vy, s)

            ans.append(f"{x} {y}")

        sys.stdout.write("\n".join(ans))
        return output.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample = """\
3
3 5 2 2 2 1 3
6 8 3 2 5 1 1
100 200 13 45 -20 111 9969
"""
assert solve_input(sample) == "2 5\n4 3\n33 196", "provided sample"

# Minimum-size table, immediate hit on the right wall.
case_min = """\
1
2 2 1 1 1 0 1
"""
assert solve_input(case_min) == "2 1", "minimum-size table"

# Both coordinates move equally and hit a corner.
case_corner = """\
1
2 2 1 1 1 1 1
"""
assert solve_input(case_corner) == "2 2", "corner collision"

# Negative velocities, including a coordinate that lands exactly on a wall.
case_negative = """\
1
5 4 2 2 -3 -2 1
"""
assert solve_input(case_negative) == "1 0", "negative velocity"

# Zero velocity in one coordinate and multiple reflections in the other.
case_zero_velocity = """\
1
2 5 1 3 0 2 3
"""
assert solve_input(case_zero_velocity) == "1 1", "zero velocity"

# Maximum-scale values.
case_maximum = """\
1
1000000000 1000000000 999999999 999999999 1000000000 -1000000000 1000000000
"""
assert solve_input(case_maximum) == "999999999 999999999", "maximum values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1 1 1 0 1`|`2 1`| Kích thước bàn tối thiểu và độ nhấn tường chính xác | 
|`2 2 1 1 1 1 1`|`2 2`| Va chạm đồng thời ở một góc | 
|`5 4 2 2 -3 -2 1`|`1 0`| Vận tốc âm và vị trí trải rộng âm | 
|`2 5 1 3 0 2 3`|`1 1`| Vận tốc bằng không và phản xạ thẳng đứng lặp đi lặp lại | 
|`1000000000 1000000000 999999999 999999999 1000000000 -1000000000 1000000000`|`999999999 999999999`| Maximum-scale arithmetic and large periods |

 ## Vỏ cạnh 

An exact wall hit is handled by the`<= length`tình trạng. Coi như```
1
3 4 1 2 2 0 1
```Đối với (x), vị trí mở là (1+2=3). Chu kì là (6) nên vị trí rút gọn là (3). Vì (3\le3), hàm trả về (3). Tọa độ dọc vẫn giữ nguyên (2). Đầu ra là`3 2`. Không có phản xạ bổ sung nào được áp dụng tại thời điểm va chạm vì câu hỏi yêu cầu vị trí tại thời điểm chính xác đó. 

Vận tốc âm yêu cầu hoạt động modulo để bình thường hóa vị trí chưa được mở. Vì```
1
5 4 2 2 -3 -2 1
```vị trí mở ngang là (-1). Giá trị chuẩn hóa của nó modulo (10) là (9), do đó vị trí phản ánh là (10-9=1). Theo chiều dọc, vị trí mở ra là (0), giữ nguyên (0). Đầu ra là`1 0`. Công thức vẫn hoạt động ngay cả khi quả bóng đã vượt qua ranh giới bên trái trong mô hình được mở ra. 

Va chạm ở góc không cần xử lý đặc biệt. Vì```
1
3 3 1 1 2 2 1
```cả hai tọa độ mở ra đều trở thành (3). Cả hai chu kỳ đều là (6) và cả hai tọa độ rút gọn đều chính xác là (3), vì vậy câu trả lời là`3 3`. Việc cả hai thành phần vận tốc đảo chiều sau đó không ảnh hưởng đến vị trí tại thời điểm (1). Sự độc lập của hai phép biến đổi một chiều xử lý góc một cách tự nhiên. 

Vận tốc bằng không cũng được bao phủ mà không có nhánh riêng biệt. Vì```
1
2 5 1 3 0 2 3
```tọa độ mở theo chiều ngang vẫn giữ nguyên (1), trong khi tọa độ mở theo chiều dọc trở thành (9). Chu kỳ thẳng đứng là (10) nên (9) gấp thành (1). Câu trả lời là`1 1`. Một mô phỏng có thể liên tục kiểm tra tọa độ ngang mặc dù không có gì có thể xảy ra ở đó, trong khi công thức chỉ tính toán nó một lần. 

Cuối cùng, các giá trị lớn không làm thay đổi thuật toán. Với```
1
1000000000 1000000000 999999999 999999999 1000000000 -1000000000 1000000000
```tọa độ mở ngang là (999999999+10^{18}) và tọa độ dọc là (999999999-10^{18}). Vì (10^{18}) chia hết cho (2\cdot10^9), cả hai tọa độ đều giảm xuống (999999999) modulo chu kỳ tương ứng của chúng. Cả hai đều đã ở nửa đầu của khoảng thời gian mở ra, mang lại`999999999 999999999`. Ví dụ này chứng minh tại sao giải pháp phải sử dụng số học thay vì mô phỏng, đồng thời thực hiện các giá trị gần với giới hạn lớn nhất cho phép.
