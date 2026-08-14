---
title: "CF 102348L - Máy in"
description: "Chúng tôi có hai hàng n bàn, mỗi hàng một hàng. Số 1 ở vị trí i có nghĩa là một đội chiếm giữ bàn đó, trong khi số 0 có nghĩa là bàn trống. Máy in có thể được cài đặt trên bất kỳ bàn nào, kể cả bàn có người sử dụng. Giả sử máy in ở vị trí p trên một tầng đã chọn."
date: "2026-08-13T01:13:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "L"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 197
verified: true
draft: false
---

[CF 102348L - Máy in](https://codeforces.com/problemset/problem/102348/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai hàng`n`bàn, mỗi tầng một hàng. MỘT`1`ở vị trí`i`có nghĩa là một đội chiếm giữ bàn đó, trong khi`0`có nghĩa là bàn trống. Máy in có thể được cài đặt trên bất kỳ bàn nào, kể cả bàn có người sử dụng. 

Giả sử máy in đang ở vị trí`p`trên một tầng được chọn. Một đội ở cùng tầng đó chỉ đi theo chiều ngang nên sẽ rất bất tiện.`|i - p|`. Đội ở tầng kia trước tiên phải đi bộ từ bàn của mình đến cầu thang, dành`k`thời gian thay đổi tầng và sau đó đi bộ đến máy in, gây bất tiện`i + k + p`. 

Nhiệm vụ là chọn cả sàn và bàn cho máy in sao cho hạn chế tối đa sự bất tiện lớn nhất của bất kỳ đội tham gia nào. Chúng tôi xuất ra giá trị tối thiểu đó và một vị trí máy in đạt được giá trị đó. 

giá trị`n`nhiều nhất là`1000`, do đó, ngay cả thuật toán bậc hai cũng chỉ thực hiện khoảng một triệu lần lặp trên mỗi tầng. Với hai tầng máy in có thể, việc liệt kê hoàn toàn trực tiếp tất cả các vị trí máy in và tất cả các nhóm thực hiện tối đa`4n² = 4,000,000`đánh giá vị trí của nhóm. Điều đó đã đủ nhỏ cho những hạn chế này, mặc dù đó là công việc không cần thiết. Cấu trúc của công thức bất tiện cho phép chúng ta giảm việc tính toán về thời gian tuyến tính. 

Các lỗi triển khai phổ biến nhất đến từ các tầng trống và do nhầm lẫn thứ tự đầu vào với số tầng. Chuỗi đầu vào thứ hai mô tả tầng 2, trong khi chuỗi đầu vào thứ ba mô tả tầng 1. Ví dụ: với```
1 1
1
0
```đội duy nhất ở tầng 2 nên đặt máy in ở tầng 2 bàn 1 cho đáp án`0`, không`3`. Việc triển khai bất cẩn giả định rằng cả hai tầng đều chứa một nhóm có thể cộng chi phí giữa các tầng không chính xác. 

Một trường hợp ranh giới khác là đội ở bàn cuối cùng. Vì```
4 2
0001
0001
```đặt máy in ở tầng 1 tại bàn 1 mang lại sự bất tiện tối đa`4 + 2 + 1 = 7`, và điều này là tối ưu. Thuật ngữ chỉ tầng kia sử dụng chỉ số chiếm dụng lớn nhất chứ không phải chỉ số nhỏ nhất, bởi vì`i + k + p`tăng với`i`. Sử dụng sai thái cực sẽ đánh giá thấp câu trả lời. 

Trường hợp thứ ba là khi tất cả các bàn trên tầng đã chọn đều đã có người ngồi. Vì```
5 3
11111
11111
```nếu máy in ở tầng 1 tại bàn 1 thì tầng đối diện sẽ đóng góp`5 + 3 + 1 = 9`. Việc tầng được chọn cũng có các đội không làm thay đổi cách thể hiện giữa các tầng. Xử lý hai tầng đối xứng trong cùng một công thức sẽ cho kết quả sai. 

Cuối cùng, một máy in được phép đặt trên bàn trống. Vì```
5 7
00000
10001
```vị trí đặt máy in tốt nhất là tầng 1, bàn 3, cho khoảng cách tối đa giữa các tầng`2`. Việc hạn chế máy in ở các bàn đã có người sử dụng sẽ bỏ lỡ mức tối ưu. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất sẽ thử mọi vị trí máy in có thể. có`2n`lựa chọn vì máy in có thể được đặt trên bất kỳ bàn nào ở cả hai tầng. Đối với mỗi lựa chọn, chúng tôi kiểm tra từng bàn có người, tính toán sự bất tiện của nó bằng cách sử dụng công thức cùng tầng hoặc chéo tầng và giữ mức tối đa. Điều này đúng vì mọi vị trí đặt máy in hợp pháp đều được xem xét rõ ràng và đóng góp của mỗi nhóm đều được đánh giá chính xác. 

Chi phí là`O(n²)`. Trong trường hợp xấu nhất có`2n`vị trí máy in và lên đến`2n`đội chiếm đóng, đưa ra`4n²`đánh giá. Tại`n = 1000`, tức là nhiều nhất là bốn triệu đánh giá, vì vậy phương pháp mạnh mẽ này thực sự khả thi dưới các ràng buộc đã nêu, đặc biệt là trong C++. Tuy nhiên, việc rút ra nghiệm tuyến tính sẽ rất hữu ích vì cách suy luận tương tự sẽ trở nên cần thiết nếu số lượng bảng tăng lên. 

Điều quan trọng là khi sàn máy in đã được cố định, chúng tôi không cần vị trí của mọi đội. Đối với các đội ở tầng máy in, giá trị lớn nhất của`|i - p|`được xác định hoàn toàn bởi các bảng chiếm giữ ngoài cùng bên trái và ngoài cùng bên phải. Nếu những vị trí đó`L`Và`R`, phần đóng góp cùng tầng là`max(p - L, R - p)`. 

Đối với các đội ở tầng đối diện, biểu thức là`i + k + p`, tăng trưởng như`i`lớn lên. Do đó, chỉ có bàn ở ngoài cùng bên phải ở tầng đối diện mới quan trọng. Nếu vị trí đó là`M`, toàn bộ tầng đối diện góp phần`M + k + p`. 

Vì vậy để có một sàn và vị trí máy in cố định`p`, toàn bộ đội sẽ thu về tối đa ba số: vị trí chiếm ngoài cùng bên trái và ngoài cùng bên phải trên tầng đã chọn và vị trí chiếm ngoài cùng bên phải ở tầng còn lại. 

Chúng ta có thể thu được các cực trị này bằng cách quét từng chuỗi đầu vào một lần. Sau đó, mọi vị trí máy in có thể được đánh giá trong thời gian không đổi. chỉ có`2n`vị trí, vì vậy tổng thời gian chạy là`O(n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Được chấp nhận cho`n <= 1000`, nhưng đắt không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mô tả của hai tầng và nhớ rằng chuỗi đầu vào thứ hai đại diện cho tầng 2, trong khi chuỗi đầu vào thứ ba đại diện cho tầng 1. Ánh xạ này phải được giữ nguyên khi in câu trả lời. 
2. Đối với mỗi tầng, hãy tìm bàn có người ở ngoài cùng bên trái, bàn có người ở ngoài cùng bên phải và bàn có người ở ngoài cùng bên phải nói chung. Hai cái đầu tiên là cần thiết khi máy in ở trên tầng đó. Vị trí ngoài cùng bên phải là cần thiết khi tầng đó là tầng đối diện. 
3. Cân nhắc việc đặt máy in ở tầng 1 tại bàn`p`. Nếu tầng 1 có các đội, sự bất tiện tối đa của họ là`max(p - L1, R1 - p)`. Nếu tầng 1 trống, phần đóng góp này bằng 0 vì không có đội nào để xem xét. 
4. Các đội ở tầng 2 đều có thể đến được bằng cầu thang bộ. Sự bất tiện của họ là`i + k + p`, và đây là số tiền lớn nhất dành cho đội ngoài cùng bên phải ở tầng 2. Như vậy đóng góp của họ là`R2 + k + p`nếu tầng 2 không trống và ngược lại bằng 0. 
5. Tận dụng tối đa hai khoản đóng góp đó. Điều này mang lại sự bất tiện tối đa cho vị trí máy in`(1, p)`. 
6. Lặp lại phép tính tương tự cho mỗi`p`từ`1`bởi vì`n`ở tầng 2. Khi máy in ở tầng 2, thuật ngữ cùng tầng sử dụng cực trị của tầng 2, trong khi thuật ngữ chéo tầng sử dụng nhóm ngoài cùng bên phải ở tầng 1. 
7. Duy trì sự bất tiện tối đa tốt nhất từng thấy cho đến nay. Chỉ cập nhật câu trả lời được lưu trữ khi giá trị mới nhỏ hơn hoàn toàn. Vì mọi vị trí tối ưu đều có thể chấp nhận được nên việc giữ vị trí đầu tiên đạt mức tối thiểu là đủ. 
8. In sự bất tiện nhất theo sau là số tầng và số bàn. Tất cả các vị trí được duy trì dưới dạng các chỉ số dựa trên một, khớp với cách đánh số của bài toán. 

### Tại sao nó hoạt động 

Đối với một tầng máy in cố định, mỗi đội cùng tầng đóng góp một khoảng cách`p`. Trong số tất cả các đội như vậy, khoảng cách tối đa phải đến từ một trong hai bàn chiếm nhiều chỗ nhất, do đó, toàn bộ nhóm cùng tầng được thể hiện chính xác bằng vị trí tối thiểu và tối đa của nó. Mỗi đội ở tầng đối diện đều đóng góp`i + k + p`, đang tăng lên nghiêm trọng trong`i`, nên chỉ có đội ở tầng đối diện ngoài cùng bên phải mới có thể xác định được số điểm tối đa. Do đó, công thức được đánh giá cho từng vị trí máy in ứng viên chính xác là sự bất tiện tối đa thực sự của nó. Vì thuật toán đánh giá mọi tầng có thể và mọi bảng có thể, nên giá trị nhỏ nhất mà nó ghi lại là giá trị tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    floor2 = input().strip()
    floor1 = input().strip()

    floors = [floor1, floor2]

    # For each floor:
    # left[i]  = leftmost occupied table, or 0 if empty
    # right[i] = rightmost occupied table, or 0 if empty
    left = [0, 0]
    right = [0, 0]

    for f in range(2):
        for pos, ch in enumerate(floors[f], 1):
            if ch == '1':
                if left[f] == 0:
                    left[f] = pos
                right[f] = pos

    best_value = 10**18
    best_floor = 1
    best_pos = 1

    for f in range(2):
        other = 1 - f

        for p in range(1, n + 1):
            same = 0
            if right[f] != 0:
                same = max(p - left[f], right[f] - p)

            other_floor = 0
            if right[other] != 0:
                other_floor = right[other] + k + p

            cur = max(same, other_floor)

            if cur < best_value:
                best_value = cur
                best_floor = f + 1
                best_pos = p

    print(best_value)
    print(best_floor, best_pos)

if __name__ == "__main__":
    solve()
```Hai chuỗi đầu vào được sắp xếp lại thành`floors = [floor1, floor2]`vậy chỉ số đó`0`tương ứng với đầu ra tầng 1 và chỉ số`1`tương ứng với tầng 2 đầu ra. Điều này tránh việc liên tục nhớ rằng chính đầu vào liệt kê tầng 2 trước tiên. 

Lần quét đầu tiên ghi lại các bảng được sử dụng ngoài cùng bên trái và ngoài cùng bên phải. Khi một`1`được gặp lần đầu tiên, nó trở thành`left[f]`; mọi vị trí chiếm đóng đều được giao cho`right[f]`, vì vậy sau khi quét`right[f]`tự động là bảng được sử dụng cuối cùng. 

giá trị`0`được sử dụng để đại diện cho một tầng trống. Vì tất cả các chỉ số bảng thực ít nhất là`1`, nó không thể bị nhầm lẫn với một vị trí chiếm đóng hợp lệ. Việc kiểm tra số 0 ngăn chặn việc một tầng trống vô tình tạo ra một đội giả ở bàn số 0. 

Đối với vị trí ứng viên máy in`p`,`same`được khởi tạo bằng 0. Nếu tầng được chọn có chứa các đội thì hai khoảng cách xa nhất sẽ được tính toán. Tối đa của hai điều đó chính xác là sự bất tiện tồi tệ nhất ở tầng đó. 

Phần đóng góp của tầng đối diện cũng được khởi tạo bằng 0. Nếu tầng đối diện có ít nhất một đội thì vị trí ngoài cùng bên phải của đội đó sẽ được sử dụng trong`right[other] + k + p`. Không cần phải kiểm tra bất kỳ nhóm nào khác vì mỗi chỉ số bảng nhỏ hơn sẽ tạo ra sự bất tiện nhỏ hơn giữa các tầng. 

Vòng lặp sử dụng`range(1, n + 1)`, Vì thế`p`trực tiếp đại diện cho chỉ mục bảng dựa trên một vấn đề. Điều này tránh được sự chuyển đổi từng cái một khi in câu trả lời. Số nguyên Python cũng có thể xử lý các giá trị lớn nhất có thể một cách thoải mái, mặc dù câu trả lời tối đa ở đây chỉ theo thứ tự`2n + k`. 

Sự so sánh chặt chẽ`cur < best_value`cố tình giữ vị trí tối ưu đầu tiên gặp phải. Điều này làm cho đầu ra mang tính xác định và cũng tự nhiên tạo ra vị trí máy in của mẫu đầu tiên ở tầng 1, bảng 1. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là```
3 2
001
001
```Tầng 1 có một đội ở bảng 3, tầng 2 cũng có một đội ở bảng 3. Bảng sau theo dõi các ứng viên ở tầng 1 trước, đủ để thiết lập mức tối ưu. 

| Tầng |`p`| Tối đa cùng tầng | Tầng đối diện tối đa |`cur`| Tốt nhất sau ứng viên | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 6 | 6 |`(6, 1, 1)`| 
| 1 | 2 | 1 | 7 | 7 |`(6, 1, 1)`| 
| 1 | 3 | 0 | 8 | 8 |`(6, 1, 1)`| 
| 2 | 1 | 2 | 6 | 6 |`(6, 1, 1)`| 
| 2 | 2 | 1 | 7 | 7 |`(6, 1, 1)`| 
| 2 | 3 | 0 | 8 | 8 |`(6, 1, 1)`| 

Ở tầng 1, bàn 1, đội cùng tầng cách nhau 2 bàn, còn đội đối diện gặp bất tiện`3 + 2 + 1 = 6`. Di chuyển máy in sang phải sẽ làm tăng chi phí giữa các sàn, do đó không có vị trí nào sau đó có thể cải thiện giá trị. Các vị trí tầng 2 đối xứng cho cùng các giá trị, nhưng quy tắc cập nhật nghiêm ngặt giữ mức tối ưu đầu tiên, tạo ra`6`Và`1 1`. 

Đối với mẫu 2,```
10 2
0001011011
1000000000
```Tầng 2 có các đội ngồi tại bàn`4, 6, 7, 9, 10`, trong khi tầng 1 chỉ có một đội ở bàn`1`. Hãy xem xét tầng 2, nơi tìm thấy mức tối ưu. 

| Tầng |`p`| Tối đa cùng tầng | Tầng đối diện tối đa |`cur`| Tốt nhất ở tầng 2 | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 9 | 4 | 9 |`(9, 1)`| 
| 2 | 2 | 8 | 5 | 8 |`(8, 2)`| 
| 2 | 3 | 7 | 6 | 7 |`(7, 3)`| 
| 2 | 4 | 6 | 7 | 7 |`(7, 3)`| 
| 2 | 5 | 5 | 8 | 8 |`(7, 3)`| 
| 2 | 6 | 4 | 9 | 9 |`(7, 3)`| 

Sự đóng góp của cùng tầng giảm khi máy in di chuyển từ bảng 1 tới khoảng thời gian được sử dụng, trong khi sự đóng góp của tầng đối diện tăng theo`p`. Sự cân bằng của họ đạt đến`7`tại bảng 3 và 4. Thuật toán gặp bảng 3 trước và giữ nguyên bảng, cho ra kết quả mẫu`7`Và`2 3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai chuỗi sàn được quét một lần, sau đó`2n`mỗi vị trí máy in được đánh giá theo thời gian không đổi. | 
| Không gian | O(1) | Chỉ có hai chuỗi đầu vào và một số cực trị và biến trả lời không đổi được lưu trữ. | 

Với`n <= 1000`, giải pháp tuyến tính thấp hơn nhiều so với giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. Ngay cả phép liệt kê bậc hai đơn giản cũng sẽ phù hợp với các giới hạn cụ thể này, nhưng công thức tuyến tính trực tiếp nắm bắt được cấu trúc toán học của bài toán. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới gọi tương tự`solve`chức năng được sử dụng bởi chương trình được gửi. Vì vấn đề cho phép có nhiều vị trí máy in tối ưu nên các trường hợp tùy chỉnh được chọn sao cho thứ tự quét xác định tạo ra câu trả lời duy nhất hoặc tối ưu đầu tiên đã biết.```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    floor2 = input().strip()
    floor1 = input().strip()

    floors = [floor1, floor2]

    left = [0, 0]
    right = [0, 0]

    for f in range(2):
        for pos, ch in enumerate(floors[f], 1):
            if ch == '1':
                if left[f] == 0:
                    left[f] = pos
                right[f] = pos

    best_value = 10**18
    best_floor = 1
    best_pos = 1

    for f in range(2):
        other = 1 - f

        for p in range(1, n + 1):
            same = 0
            if right[f] != 0:
                same = max(p - left[f], right[f] - p)

            other_floor = 0
            if right[other] != 0:
                other_floor = right[other] + k + p

            cur = max(same, other_floor)

            if cur < best_value:
                best_value = cur
                best_floor = f + 1
                best_pos = p

    print(best_value)
    print(best_floor, best_pos)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("""3 2
001
001
""") == """6
1 1
""", "sample 1"

# Provided sample 2
assert run("""10 2
0001011011
1000000000
""") == """7
2 3
""", "sample 2"

# Minimum-size input, one team and one table
assert run("""1 1
1
0
""") == """0
2 1
""", "minimum size"

# Only one floor has teams, so the printer should sit on that floor
assert run("""5 3
00000
11111
""") == """2
1 3
""", "empty opposite floor"

# Teams at both extreme tables, checking the left/right extrema
assert run("""5 7
00000
10001
""") == """2
1 3
""", "extreme occupied tables"

# Maximum n, all tables occupied on both floors
assert run("1000 1000\n" + "1" * 1000 + "\n" + "1" * 1000 + "\n") == \
       """2001
1 1
""", "maximum size and all occupied"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 / 0`|`0 / 2 1`| Kích thước tối thiểu và một tầng đối diện trống | 
|`5 3 / 00000 / 11111`|`2 / 1 3`| Máy in có thể được đặt ở một bàn trống và tầng được chọn có thể là tầng duy nhất có người sử dụng | 
|`5 7 / 00000 / 10001`|`2 / 1 3`| Sử dụng đúng cả vị trí chiếm giữ ngoài cùng bên trái và ngoài cùng bên phải | 
|`1000 1000 / 1000 ones / 1000 ones`|`2001 / 1 1`| Kích thước đầu vào tối đa, tất cả các bảng được sử dụng và giá trị lớn | 

## Vỏ cạnh 

Đối với một tầng đối diện trống, hãy xem xét```
1 1
1
0
```Chuỗi tầng 2 chỉ chứa đội duy nhất nên việc đặt máy in ở tầng 2 tại bàn 1 sẽ gây bất tiện cho cùng tầng`|1 - 1| = 0`. Trong quá trình tiền xử lý, tầng 2 nhận được`left = right = 1`, trong khi tầng 1 giữ`right = 0`. Khi đánh giá tầng 2, thuật ngữ cùng tầng là`0`và thời hạn ở tầng đối diện vẫn còn`0`vì tầng 1 trống. Câu trả lời kết quả là`0 2 1`. Điều này ngăn cản một nhóm không tồn tại đóng góp khoảng cách xuyên tầng nhân tạo. 

Đối với một đội ở bàn cuối cùng, hãy xem xét```
4 2
0001
0001
```Nếu máy in ở tầng 1 tại bàn`p`, tầng đối diện có đội ngoài cùng bên phải ở bàn 4 nên phần đóng góp của các tầng là`4 + 2 + p`. Điều này được giảm thiểu ở mức`p = 1`, cho`7`. Tại`p = 2`, nó trở thành`8`, và các vị thế lớn hơn thì tệ hơn. Đội cùng tầng chỉ cách nhau ba bàn`p = 1`, vậy còn lại mức tối đa`7`. Bản ghi thuật toán`7 1 1`, chứng minh tại sao vị trí ngoài cùng bên phải phải được sử dụng cho thuật ngữ liên tầng. 

Khi tầng được chọn có nhiều đội, chỉ có hai thái cực của nó là quan trọng. Vì```
5 3
11111
11111
```tầng 1 có`L = 1`Và`R = 5`, trong khi tầng 2 có`R = 5`. Tại vị trí máy in 1, mức tối đa cùng tầng là`4`và mức tối đa xuyên sàn là`5 + 3 + 1 = 9`, vậy mục tiêu là`9`. Di chuyển sang phải làm cho thời hạn xuyên sàn tăng lên, đạt`10`ở vị trí 2. Do đó, vị trí đầu tiên đã tối ưu và kết quả đầu ra của thuật toán`9 1 1`. 

Đối với các bảng bị chiếm chỉ ở hai ranh giới,```
5 7
00000
10001
```tầng được chọn có`L = 1`Và`R = 5`. Tại`p = 3`, khoảng cách cùng tầng là`2`cho cả hai bảng bị chiếm dụng, vì vậy mức tối đa là`2`. Tầng còn lại trống nên không có đóng góp chéo tầng. Vị trí 2 và 4 cho khoảng cách tối đa`3`, trong khi vị trí 1 và 5 cho`4`. Do đó, thuật toán tìm thấy`2 1 3`, chứng tỏ rằng điểm tối ưu có thể nằm trên một bảng trống và cả hai cực trị đều cần thiết.
