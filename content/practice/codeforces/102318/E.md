---
title: "CF 102318E - Phi tiêu đơn giản"
description: "Chúng tôi có một bảng phi tiêu tập trung ở gốc tọa độ. Bàn cờ được chia thành các phần bằng nhau và có ba vùng ghi điểm đồng tâm. Vòng tròn trong cùng của bán kính b là mắt bò và luôn cho 50 điểm."
date: "2026-08-13T05:20:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "E"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 362
verified: true
draft: false
---

[CF 102318E - Phi tiêu đơn giản](https://codeforces.com/problemset/problem/102318/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bảng phi tiêu tập trung ở gốc tọa độ. Bảng được chia triệt để thành`w`các nêm bằng nhau và có ba vùng ghi điểm đồng tâm. Bán kính đường tròn trong cùng`b`là mắt bò và luôn cho 50 điểm. Giữa bán kính`b`Và`d`là vùng kép, trong đó giá trị của hình nêm được nhân với 2. Giữa các bán kính`d`Và`s`là vùng duy nhất, nơi sử dụng giá trị thông thường của hình nêm. Một phi tiêu bên ngoài bán kính`s`điểm 0. 

Nêm ngay lập tức ngược chiều kim đồng hồ tính từ trục x dương có giá trị 1. Di chuyển ngược chiều kim đồng hồ qua các nêm sẽ làm tăng giá trị lên một, do đó nêm ngay lập tức theo chiều kim đồng hồ tính từ trục x dương có giá trị`w`. Đối với mọi trường hợp thử nghiệm, chúng tôi được cung cấp kích thước bảng theo sau là tọa độ của tất cả các phi tiêu và chúng tôi cần tổng điểm của chúng. Tuyên bố cuộc thi ban đầu đưa ra`2 <= w <= 20`,`0 < b < d < s < 100`và tối đa 100 phi tiêu cho mỗi trường hợp thử nghiệm. Tọa độ phi tiêu nằm giữa`-100`Và`100`, và không có phi tiêu nào ở bên trong`10^-5`của ranh giới hình nêm hoặc ranh giới hình tròn. 

Những giới hạn này làm cho bài toán trở nên đơn giản hơn nhiều so với nhiều bài toán hình học. Thậm chí một`O(tw)`giải pháp thực hiện nhiều nhất`100 * 20 = 2000`kiểm tra nêm trong một trường hợp thử nghiệm, vì vậy một giải pháp đơn giản đã đủ nhanh. Tuy nhiên, chúng tôi có thể xử lý từng phi tiêu trong`O(1)`bằng cách chuyển đổi trực tiếp góc của nó thành số nêm, cho`O(t)`thời gian. 

Các trường hợp cạnh có liên quan nhất là do hình học chứ không phải do kích thước đầu vào lớn. Ví dụ, một phi tiêu ở trung tâm chính xác```
1
2 1 2 3
1
0.000 0.000
```đạt điểm 50, không phải 0 hoặc giá trị nêm. Một giải pháp tính góc trước và coi tâm như một hình nêm thông thường vẫn có thể tạo ra một giá trị, nhưng giá trị đó là vô nghĩa vì mắt bò được ưu tiên hơn việc tính điểm góc. 

Một phi tiêu bên ngoài bảng phải đạt điểm 0 ngay cả khi góc của nó hướng về phía một cái nêm có giá trị cao. Ví dụ,```
1
2 1 2 3
1
0.000 4.000
```điểm 0 vì khoảng cách của nó đến điểm gốc lớn hơn bán kính bên ngoài. 

Phi tiêu trong vòng đôi phải sử dụng gấp đôi giá trị nêm. Với```
1
2 1 5 8
1
0.000 3.000
```phi tiêu nằm trong vòng đôi và nằm ở nêm phía trên, có giá trị là 1, do đó kết quả đầu ra đúng là`2`. Việc quên số nhân sẽ tạo ra`1`. 

Trường hợp góc âm cũng dễ bị xử lý sai. Với```
1
4 1 5 8
1
1.000 -1.000
```góc là`-45`độ. Cái này thuộc về cái nêm thứ tư, không phải cái nêm đầu tiên, vì vậy phi tiêu có giá trị bằng cái nêm thứ tư. Việc thực hiện bất cẩn chỉ đơn giản là chia góc âm cho chiều rộng hình nêm sẽ nhận được chỉ số âm. 

Ngoài ra còn có sự mâu thuẫn về mặt lịch sử trong mẫu đầu tiên của tuyên bố được lưu trữ năm 2017. Nó mang lại`4 7 13 10`, bất chấp mối quan hệ đã nêu`b < d < s`. Câu trả lời được mong đợi sẽ xử lý`13`là ranh giới bên ngoài của vùng kép, do đó việc triển khai bên dưới tuân theo các biến chính xác như được viết và tái tạo đầu ra mẫu ban đầu. Các mẫu sau và quy tắc tính điểm phù hợp với cách giải thích này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng phi tiêu một cách độc lập. Đầu tiên tính khoảng cách bình phương của nó từ điểm gốc. Nếu giá trị đó nhỏ hơn`b²`, phi tiêu có giá trị 50. Ngược lại, hãy xác định hình nêm nào chứa phi tiêu rồi nhân giá trị của hình nêm đó với 2, 1 hoặc 0 tùy theo vùng bán kính. 

Để thực hiện vũ lực, khi đã biết góc của phi tiêu, chúng ta có thể kiểm tra góc đó với mọi góc`w`khoảng thời gian nêm cho đến khi chúng ta tìm thấy cái nêm phù hợp. Điều này đúng vì các hình nêm phân chia thành hình tròn đầy đủ. Vì có nhiều nhất 20 cái nêm và 100 phi tiêu, trường hợp xấu nhất là chính xác 2000 lần kiểm tra cái nêm cho mỗi trường hợp thử nghiệm. Công việc đó chưa đủ để đe dọa giới hạn một giây, vì vậy phiên bản này đã được chấp nhận. 

Quan sát rõ ràng hơn là các nêm có kích thước bằng nhau. Chúng ta không cần phải tìm kiếm thông qua chúng. Một cuộc cách mạng đầy đủ có góc`2π`, do đó mỗi nêm chiếm một góc`2π / w`. 

Nếu phi tiêu có góc chuẩn hóa`θ`TRONG`[0, 2π)`, chỉ số nêm dựa trên 0 của nó chỉ đơn giản là`floor(θ / (2π / w))`. 

Thêm một chuyển đổi chỉ số đó thành điểm của bảng phi tiêu từ 1 đến`w`. Điều này làm giảm công việc góc cạnh từ`O(w)`ĐẾN`O(1)`. 

Phương pháp vũ lực hoạt động hiệu quả vì mọi phi tiêu có thể được phân loại độc lập, nhưng nó tốn công sức không cần thiết để tìm kiếm một cái nêm có thể tính toán trực tiếp. Độ rộng góc bằng nhau chính xác là điều làm cho công thức trực tiếp có thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tw) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm mô tả một bảng phi tiêu độc lập và các lần ném của nó, do đó tổng số điểm có thể được đặt lại về 0 khi bắt đầu mỗi trường hợp. 
2. Đọc`w`,`b`,`d`, Và`s`, sau đó đọc số`t`của phi tiêu. Lưu trữ bán kính bình phương như`b²`,`d²`, Và`s²`. So sánh khoảng cách bình phương sẽ tránh được căn bậc hai không cần thiết và duy trì thứ tự chính xác của các vùng xuyên tâm. 
3. Đối với mỗi phi tiêu ở`(x, y)`, tính toán`r² = x² + y²`. Nếu như`r² < b²`, cộng ngay 50 điểm. Mắt bò có điểm cố định nên góc của nó không thành vấn đề. 
4. Nếu phi tiêu không nằm trong mắt bò, hãy xác định vị trí góc của nó bằng`atan2(y, x)`. Hàm này cho góc chính xác ở cả bốn góc phần tư, không giống như`atan(y / x)`, làm mất thông tin góc phần tư. 
5. Nếu góc âm, hãy thêm`2π`để nó nằm trong`[0, 2π)`. Điều này biến tọa độ tròn thành một khoảng không âm bình thường có thể được chia thành các chiều rộng nêm bằng nhau. 
6. Tính toán`wedge = int(angle / (2π / w)) + 1`. Phép chia xác định có bao nhiêu khoảng nêm hoàn chỉnh xuất hiện trước góc của phi tiêu và`+1`chuyển đổi chỉ số dựa trên số 0 thành cách đánh số điểm của bảng. 
7. Xác định hệ số nhân từ vị trí hướng tâm. Một phi tiêu với`r² < d²`nằm trong vùng kép và nhận được hệ số nhân 2. Một phi tiêu với`r² < s²`nằm trong một khu vực và nhận được hệ số nhân 1. Một phi tiêu vượt ra ngoài`s`nhận được số nhân 0. 
8. Thêm`wedge * multiplier`đến tổng của trường hợp thử nghiệm và in tổng sau khi tất cả các phi tiêu đã được xử lý. 

### Tại sao nó hoạt động 

Đối với mỗi phi tiêu, khoảng cách bình phương sẽ xác định duy nhất vùng ghi điểm đồng tâm nào chứa nó, trong khi`atan2`xác định duy nhất hướng của nó. Bởi vì tất cả các hình nêm có cùng chiều rộng góc, chia góc chuẩn hóa cho`2π/w`đưa ra chính xác chỉ số của cái nêm chứa phi tiêu. Sau đó, hệ số nhân xuyên tâm sẽ áp dụng quy tắc tính điểm cho vùng đó. Trường hợp đặc biệt duy nhất là mắt bò, được kiểm tra đầu tiên vì điểm cố định của nó sẽ cao hơn điểm nêm. Vì bài toán đảm bảo rằng các phi tiêu không nằm gần các ranh giới nên việc làm tròn dấu phẩy động không thể thực hiện chuyển đổi phi tiêu hợp lệ giữa các vùng liền kề. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    tc = int(input())
    out = []

    for _ in range(tc):
        w, b, d, s = map(int, input().split())
        t = int(input())

        b2 = b * b
        d2 = d * d
        s2 = s * s

        wedge_angle = 2.0 * math.pi / w
        total = 0

        for _ in range(t):
            x, y = map(float, input().split())
            r2 = x * x + y * y

            if r2 < b2:
                total += 50
                continue

            angle = math.atan2(y, x)
            if angle < 0:
                angle += 2.0 * math.pi

            wedge = int(angle / wedge_angle) + 1

            if r2 < d2:
                total += 2 * wedge
            elif r2 < s2:
                total += wedge

        out.append(str(total))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Ba biến bán kính bình phương đầu tiên được tính một lần cho mỗi trường hợp thử nghiệm vì bảng không thay đổi giữa các phi tiêu. So sánh`r2`với những giá trị này tránh được`sqrt`, vì vậy mọi phân loại xuyên tâm chỉ sử dụng so sánh số học. 

Việc kiểm tra mắt bò xuất hiện trước khi tính toán góc. Bên cạnh việc rẻ hơn một chút, điều này phản ánh trực tiếp hệ thống phân cấp tính điểm: một phi tiêu trong mắt bò luôn đạt điểm 50, bất kể hướng nào.`atan2(y, x)`được sử dụng thay vì`atan(y / x)`bởi vì`atan`chẳng hạn, không thể phân biệt được`(1, -1)`từ`(-1, 1)`.`atan2`xử lý toàn bộ vòng tròn một cách chính xác. Kết quả âm tính được chuyển bởi`2π`, tạo ra một góc trong phạm vi`[0, 2π)`. 

biểu hiện`int(angle / wedge_angle) + 1`là phép tính hình học quan trọng. Bởi vì phi tiêu được đảm bảo không ở gần ranh giới hình nêm nên không cần điều chỉnh epsilon. Sự đảm bảo ranh giới tương tự cho phép chúng tôi sử dụng nghiêm ngặt`<`so sánh về bán kính 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên. Các giá trị dấu phẩy động chỉ được sử dụng cho tọa độ và góc, trong khi tất cả điểm cuối cùng vẫn là số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu cuộc thi ban đầu là:```
3
4 7 13 10
2
4.000 4.000
6.000 -4.000
10 1 6 10
1
20.000 -0.500
8 3 7 50
5
-0.750 1.207
1.180 3.132
27.111 -44.630
-43.912 -22.104
2.000 -6.000
```Trường hợp thử nghiệm đầu tiên thể hiện cả cách xử lý mắt bò và vòng kép. 

| Phi tiêu | x | y | r² | Góc | Nêm | Vùng | Điểm | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 32 | không cần thiết | không cần thiết | Mắt bò | 50 | 50 | 
| 2 | 6 | -4 | 52 | 326,31° | 4 | Đôi | 8 | 58 | 

Phi tiêu thứ nhất có bình phương khoảng cách là 32, nhỏ hơn`7² = 49`, nên ngay lập tức nó đạt điểm 50. Phi tiêu thứ hai có bình phương khoảng cách là 52, nằm ở dưới`13² = 169`và bên ngoài hồng tâm. Góc âm của nó được chuẩn hóa thành khoảng 326,31 độ, đặt nó ở vị trí thứ 4. Hệ số nhân đôi thay đổi điểm của nó từ 4 thành 8, cho tổng số là 58. 

### Mẫu 3 

Trường hợp thử nghiệm thứ ba mang tính đại diện hơn vì nó thực hiện tất cả các vùng tính điểm. 

| Phi tiêu | x | y | r² | Vùng | Nêm | Hệ số nhân | Điểm | Tổng số chạy | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | -0.750 | 1.207 | ≈2,02 | Mắt bò | không cần thiết | không cần thiết | 50 | 50 | 
| 2 | 1.180 | 3.132 | ≈11.21 | Đôi | 2 | 2 | 4 | 54 | 
| 3 | 27.111 | -44.630 | ≈2730 | Bên ngoài | không cần thiết | 0 | 0 | 54 | 
| 4 | -43.912 | -22.104 | ≈2415 | Độc thân | 5 | 1 | 5 | 59 | 
| 5 | 2.000 | -6.000 | 40 | Đôi | 7 | 2 | 14 | 73 | 

Phi tiêu đầu tiên nằm trong bán kính 3 và đạt điểm 50. Phi tiêu thứ hai nằm giữa bán kính 3 và 7 và nằm trong nêm 2 nên đạt điểm 4. Phi tiêu thứ ba nằm ngoài bán kính 50 và đạt điểm 0. Phi tiêu thứ tư nằm trong vòng đơn và chỉ vào nêm 5, cho 5. Phi tiêu cuối cùng nằm trong vòng đôi và chỉ vào nêm 7, cho 14. Tổng cuối cùng là 73. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) cho mỗi trường hợp thử nghiệm | Mỗi trong số`t`phi tiêu đòi hỏi một số lượng không đổi các phép toán số học, lượng giác và so sánh. | 
| Không gian | O(1) | Chỉ các thông số bảng, phi tiêu hiện tại và điểm chạy được lưu trữ. | 

Với tối đa 100 phi tiêu cho mỗi trường hợp thử nghiệm, thuật toán chỉ thực hiện vài trăm thao tác với thời gian không đổi cho mỗi trường hợp. các`w <= 20`giới hạn thậm chí không cần thiết bởi độ phức tạp tiệm cận của lời giải tối ưu, vì số lượng các phần chỉ xuất hiện trong một phép chia. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline
    tc = int(input())
    out = []

    for _ in range(tc):
        w, b, d, s = map(int, input().split())
        t = int(input())

        b2 = b * b
        d2 = d * d
        s2 = s * s
        wedge_angle = 2.0 * math.pi / w

        total = 0

        for _ in range(t):
            x, y = map(float, input().split())
            r2 = x * x + y * y

            if r2 < b2:
                total += 50
                continue

            angle = math.atan2(y, x)
            if angle < 0:
                angle += 2.0 * math.pi

            wedge = int(angle / wedge_angle) + 1

            if r2 < d2:
                total += 2 * wedge
            elif r2 < s2:
                total += wedge

        out.append(str(total))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
3
4 7 13 10
2
4.000 4.000
6.000 -4.000
10 1 6 10
1
20.000 -0.500
8 3 7 50
5
-0.750 1.207
1.180 3.132
27.111 -44.630
-43.912 -22.104
2.000 -6.000
"""

assert run(sample) == "58\n0\n73", "provided samples"

assert run("""\
1
2 1 2 3
1
0.000 0.000
""") == "50", "minimum-size bullseye"

assert run("""\
1
20 1 10 99
100
100.000 100.000
""" + "\n".join(["100.000 100.000"] * 99) + "\n") == "0", \
    "maximum-size outside-board case"

assert run("""\
1
2 1 10 20
100
0.000 0.000
""" + "\n".join(["0.000 0.000"] * 99) + "\n") == "5000", \
    "all-equal bullseye values"

assert run("""\
1
2 2 5 8
4
0.000 1.000
0.000 3.000
0.000 6.000
0.000 9.000
""") == "53", "radial region boundaries"

assert run("""\
1
4 1 5 8
1
1.000 -1.000
""") == "8", "negative-angle fourth wedge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2 1 2 3 / 1 / 0 0`|`50`| Bảng kích thước tối thiểu và ưu tiên mắt bò | 
|`20 1 10 99`với 100 phi tiêu`(100,100)`|`0`| Số lượng phi tiêu tối đa và xử lý bên ngoài | 
| 100 phi tiêu giống hệt nhau`(0,0)`|`5000`| Các giá trị và tích lũy giống hệt nhau lặp đi lặp lại | 
|`2 2 5 8`có bán kính 1, 3, 6, 9 |`53`| Các khu vực Bull, đôi, đơn và bên ngoài | 
|`4 1 5 8`với`(1,-1)`|`8`| Chuẩn hóa góc âm và nêm thứ tư | 

## Vỏ cạnh 

Mắt bò phải được kiểm tra trước khi tính toán góc. Vì```
1
2 1 2 3
1
0.000 0.000
```chúng tôi nhận được`r² = 0`, nhỏ hơn`b² = 1`. Thuật toán ngay lập tức thêm 50 và bỏ qua`atan2`, sản xuất`50`. Điều này tránh việc gán một cái nêm tùy ý vào điểm trung tâm. 

Một phi tiêu bên ngoài bảng tuân theo phép tính góc tương tự như một phi tiêu thông thường, nhưng hệ số nhân của nó vẫn bằng 0. Vì```
1
2 1 2 3
1
0.000 4.000
```khoảng cách bình phương là 16, lớn hơn`s² = 9`, nên không có điểm nào được thêm vào. Kết quả là`0`. 

Vòng đôi được xử lý trước vòng đơn vì các vùng xuyên tâm được lồng vào nhau. Vì```
1
2 1 5 8
1
0.000 3.000
```khoảng cách bình phương là 9, nằm giữa`1²`Và`5²`. Điểm nằm trong phần nêm 1 nên giá trị cơ bản của nó là 1 và hệ số nhân đôi cho kết quả`2`. 

Vòng đơn hoạt động tương tự. Vì```
1
2 1 5 8
1
0.000 6.000
```khoảng cách bình phương là 36, nằm giữa`5²`Và`8²`. Điểm vẫn ở phần 1, nhưng hệ số nhân bây giờ là 1, cho kết quả`1`. 

Góc âm phải được chuẩn hóa trước khi tìm hình nêm. Vì```
1
4 1 5 8
1
1.000 -1.000
```

`atan2(-1, 1)`trả lại`-π/4`. Thêm`2π`sản xuất`7π/4`, thuộc về phần thứ tư trong bốn hình nêm 90 độ. Điểm cơ bản là 4 và điểm nằm ở vòng đôi nên kết quả là`8`. 

Cuối cùng, mẫu cuộc thi chứa`4 7 13 10`xứng đáng được chăm sóc đặc biệt. Các ràng buộc bằng văn bản nói`b < d < s`, nhưng mẫu lịch sử cụ thể đó đảo ngược hai bán kính cuối cùng. Sản lượng dự kiến ​​​​là 58 có được bằng cách xử lý phi tiêu ở`(6,-4)`vì nằm trong vùng kép vì khoảng cách của nó là khoảng 7,21, bên dưới`d = 13`. Mã cố tình không sắp xếp hoặc diễn giải lại ba bán kính. Nó tuân theo các so sánh tính điểm theo thứ tự đầu vào của chúng, sao chép mẫu chính thức đồng thời hoạt động bình thường đối với các trường hợp hợp lệ đáp ứng`b < d < s`.
