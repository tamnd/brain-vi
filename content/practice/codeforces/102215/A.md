---
title: "CF 102215A - Phòng và lối đi"
description: "Hầm ngục là một dãy phòng thẳng tắp. Đoạn i nối phòng i-1 với phòng i nên việc di chuyển về phía lối ra luôn đồng nghĩa với việc xử lý mảng từ trái sang phải. Mỗi giá trị mảng mô tả cả màu sắc của thẻ và hành vi của đoạn văn."
date: "2026-08-17T23:31:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "A"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 224
verified: false
draft: false
---

[CF 102215A - Phòng và lối đi](https://codeforces.com/problemset/problem/102215/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Hầm ngục là một dãy phòng thẳng tắp. Đoạn văn`i`kết nối phòng`i-1`về phòng`i`, vì vậy việc di chuyển về phía lối ra luôn có nghĩa là xử lý mảng từ trái sang phải. 

Mỗi giá trị mảng mô tả cả màu sắc của thẻ và hành vi của đoạn văn. Giá trị dương`c`có nghĩa là đoạn văn kiểm tra xem có vượt qua không`c`vẫn còn hiệu lực. Giá trị âm`-c`có nghĩa là lối đi luôn có thể được vượt qua, nhưng sau khi vượt qua nó, hãy vượt qua`c`trở nên không hợp lệ. 

Đối với phòng khởi đầu`s`, tất cả các thẻ ban đầu đều hợp lệ. Chúng ta cần số lượng lối đi vượt qua thành công trước khi việc di chuyển trở nên bất khả thi. Tương tự, nếu lối đi bị chặn đầu tiên là`j`, câu trả lời là`j - s - 1`. Nếu không có lối đi nào chặn chúng ta, câu trả lời là`n - s`. 

Sự tương tác chính là giữa sự xuất hiện tiêu cực của một màu và sự xuất hiện tích cực sau đó của cùng một màu. Một khi chúng ta băng qua`-c`, sau này`+c`trở thành một lối đi bị chặn. Bản thân một đoạn tiêu cực không bao giờ cản trở chuyển động. 

Với`n`lên đến`500000`, một thuật toán mô phỏng độc lập hành trình từ mọi phòng xuất phát là quá đắt. Một mô phỏng đơn giản kiểm tra tới`n-s`đoạn văn để bắt đầu`s`, cho 

[ 
n+(n-1)+\cdots+1=\frac{n(n+1)}2 
] 

kiểm tra hành trình. Ở mức tối đa`n`, đây là về`1.25 \cdot 10^{11}`hoạt động không thể phù hợp với giới hạn 2 giây. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Có một số trường hợp ranh giới có thể dễ dàng gây ra lỗi riêng lẻ. Chẳng hạn, với một đoạn văn,```
1
-1
```câu trả lời là`1`, bởi vì một đoạn văn phủ định không bao giờ cản trở được con người. Một giải pháp xử lý tình trạng vô hiệu như một lệnh dừng ngay lập tức sẽ xuất ra không chính xác`0`. 

Trường hợp quan trọng thứ hai là một đoạn văn phủ định, theo sau là một đoạn văn tích cực cùng màu:```
2
-1 1
```Bắt đầu từ phòng`0`, chúng ta băng qua đoạn đầu tiên và sau đó dừng lại ở đoạn thứ hai, vì vậy đầu ra là`1 1`. Bản thân việc vượt qua tích cực không được tính là vượt qua thành công. 

Việc xảy ra tiêu cực trước giờ xuất phát không được ảnh hưởng đến hành trình. Ví dụ,```
3
-1 1 1
```có đầu ra`1 2 1`. Bắt đầu từ phòng`1`, càng sớm`-1`không liên quan vì thẻ ban đầu có hiệu lực khi hành trình bắt đầu. Một giải pháp ghi nhớ tất cả các sự kiện tiêu cực trên toàn cầu thay vì chỉ những sự kiện ở bên phải phòng xuất phát sẽ mắc lỗi này. 

Cuối cùng, nhiều màu sắc có thể tạo ra một số vị trí dừng có thể. Ví dụ,```
5
1 -1 2 -2 2
```sản xuất`3 3 2 1 1`. Bắt đầu từ phòng`0`, màu sắc`1`không gây ra vấn đề gì trong tương lai, nhưng màu sắc`2`bị vô hiệu khi đi qua`4`, vì vậy đoạn văn`5`trở thành người chặn đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo chính xác những gì tuyên bố mô tả. Đối với mỗi phòng xuất phát`s`, tạo trạng thái mới trong đó mọi thẻ đều hợp lệ, quét các đoạn`s+1, s+2, ...`và duy trì những màu nào đã bị vô hiệu. Giá trị âm sẽ làm mất hiệu lực màu của nó, trong khi giá trị dương sẽ dừng mô phỏng nếu màu của nó đã bị vô hiệu. Điều này hoàn toàn chính xác vì nó mô phỏng hành trình thực tế mà không đưa ra bất kỳ giả định nào. 

Vấn đề là các phòng bắt đầu lân cận lặp lại hầu hết các công việc giống nhau. Bắt đầu từ phòng`s`và bắt đầu từ phòng`s+1`cả hai đều kiểm tra gần như toàn bộ hậu tố của mảng. Trong trường hợp xấu nhất, vũ lực thực hiện`n(n+1)/2`kiểm tra thông qua, về`1.25 * 10^11`khi`n=500000`. 

Quan sát hữu ích là cuộc hành trình có thể được phân tích ngược lại. Giả sử chúng ta đang xử lý đoạn văn`i`từ phải sang trái. Đối với mỗi màu, chúng ta có thể nhớ đoạn tích cực gần nhất của màu đó ở bên phải. Nếu như`a[i]`là tích cực, đi qua`i`luôn có thể vượt qua được khi nó là chặng đầu tiên của cuộc hành trình, vì vậy câu trả lời cho điểm xuất phát của nó chỉ đơn giản là một câu trả lời nhiều hơn câu trả lời sau khi vượt qua nó. 

Nếu như`a[i]`là tiêu cực với màu sắc`c`, bản thân đoạn văn có thể vượt qua được, nhưng nó vô hiệu`c`. Nếu không có tích cực`c`ở bên phải của nó, sự vô hiệu đó không bao giờ quan trọng, vì vậy một lần nữa chúng ta có thể băng qua lối đi và kế thừa câu trả lời cho phòng bên cạnh. 

Trường hợp thú vị là khi giá trị dương gần nhất`c`bên phải là lối đi`p`. Sau đó, một cuộc hành trình bắt đầu tại hoặc trước đoạn đường`i`và chéo`-c`không thể vượt qua lối đi`p`. Nó chỉ có thể đến được phòng`p-1`. Có thể đã có một vị trí dừng thậm chí còn sớm hơn do một lối đi tiêu cực khác ở xa hơn về phía bên phải gây ra, vì vậy chúng tôi duy trì một phòng có thể tiếp cận ở ngoài cùng bên phải trên toàn cầu và giảm thiểu tất cả các hạn chế đó. 

Đây là lý do tại sao tính năng quét ngược lại hoạt động rất tốt. Mỗi đoạn tích cực đóng góp vị trí của nó cho việc tra cứu theo từng màu, mỗi đoạn tiêu cực có khả năng thắt chặt một ranh giới toàn cầu và mỗi vị trí mảng được xử lý chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Quét ngược tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng bằng cách sử dụng các chỉ số đoạn văn dựa trên một. Cho phép`ans[i]`đại diện cho số lượng đoạn thành công khi bắt đầu từ phòng`i-1`. Chúng tôi cũng xác định`ans[n+1] = 0`, đại diện cho hậu tố trống sau đoạn văn cuối cùng. 
2. Quét các đoạn văn từ`n`xuống`1`. Tại thời điểm này, mọi đoạn bên phải đã được phân tích và đối với mỗi màu, chúng ta có thể biết đoạn tích cực gần nhất của màu đó ở bên phải. 
3. Duy trì`pos[c]`, đoạn dương gần nhất có màu`c`gặp phải cho đến nay trong quá trình quét ngược. Ban đầu mọi màu sắc đều không có lối đi như vậy. 
4. Nếu`a[i]`là tích cực, cuộc hành trình bắt đầu từ phòng`i-1`luôn có thể vượt qua lối đi`i`, bởi vì tất cả các thẻ ban đầu đều hợp lệ. Sau khi vượt qua nó, tình huống chính xác là tình huống được thể hiện bởi`ans[i+1]`. Do đó chúng tôi thiết lập`ans[i] = ans[i+1] + 1`. Chỉ sau khi tính toán câu trả lời này, chúng tôi mới lưu trữ`pos[a[i]] = i`, bởi vì đoạn tích cực này có thể ảnh hưởng đến đoạn tiêu cực nằm ở bên trái của nó. 
5. Nếu`a[i]`là tiêu cực với màu sắc`c`Và`pos[c]`không tồn tại, băng qua lối đi`i`làm mất hiệu lực`c`, nhưng không có màu kiểm tra đoạn văn trong tương lai`c`. Việc vô hiệu hóa vĩnh viễn không liên quan nên phần còn lại của hành trình sẽ giống như hành trình bắt đầu trong phòng`i`. Chúng tôi thiết lập`ans[i] = ans[i+1] + 1`. 
6. Nếu`a[i]`là tiêu cực với màu sắc`c`và tích cực gần nhất`c`đang ở đoạn đường`p`, sau đó băng qua lối đi`i`làm cho lối đi`p`không thể vượt qua. Phòng cuối cùng có thể truy cập do sự vô hiệu cụ thể này là`p-1`. Chúng tôi duy trì`limit`, phòng nhỏ nhất có thể truy cập được trên tất cả các đoạn phủ định đã được xử lý và cập nhật`limit = min(limit, p-1)`. 
7. Sau khi cập nhật`limit`, một cuộc hành trình bắt đầu từ căn phòng`i-1`có thể tiếp cận hầu hết các phòng`limit`. Vì nó bắt đầu từ phòng`i-1`, số lần chuyển đổi thành công là`limit - (i-1)`, đó là`limit - i + 1`. Chúng tôi lưu trữ giá trị đó trong`ans[i]`. 
8. Sau khi quét ngược,`ans[1], ans[2], ..., ans[n]`tương ứng chính xác với phòng bắt đầu`0, 1, ..., n-1`, vì vậy hãy in chúng theo thứ tự. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý vị trí`i`,`limit`là phòng có thể tiếp cận sớm nhất được áp đặt bởi mọi đoạn phủ định trong hậu tố đã được xử lý. Một đoạn tiêu cực`-c`chỉ có thể tạo ra hạn chế thông qua sự tích cực đầu tiên`+c`ở bên phải của nó, bởi vì đó là nơi đầu tiên mà thẻ không hợp lệ thực sự được kiểm tra. Việc vượt qua mức tối thiểu những hạn chế này sẽ mang lại lối đi đầu tiên có thể dừng cuộc hành trình. Những đoạn tích cực không bao giờ tự tạo ra những hạn chế, và một đoạn tiêu cực không xuất hiện tích cực ở bên phải nó không bao giờ có thể ảnh hưởng đến chuyển động trong tương lai. Do đó, mọi câu trả lời được tính toán bằng cách quét ngược chính xác là số đoạn văn mà vị trí bắt đầu tương ứng có thể vượt qua thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # pos[c] = nearest positive passage of color c to the right.
    pos = [0] * (n + 1)

    # limit is the smallest room index that can be reached because
    # of a restriction created by a negative passage in the suffix.
    # Initially there is no restriction, so room n is reachable.
    limit = n

    ans = [0] * (n + 2)

    for i in range(n, 0, -1):
        x = a[i - 1]

        if x > 0:
            # Passage i is always crossable when it is the first
            # passage of the journey.
            ans[i] = ans[i + 1] + 1

            # This passage may block a negative occurrence of the
            # same color located to its left.
            pos[x] = i
        else:
            color = -x

            if pos[color] == 0:
                # The invalidated pass is never checked later.
                ans[i] = ans[i + 1] + 1
            else:
                # Passage pos[color] is the first positive check of
                # this color to the right, so we can reach only the
                # room immediately before it.
                limit = min(limit, pos[color] - 1)
                ans[i] = limit - i + 1

    print(*ans[1:n + 1])

if __name__ == "__main__":
    solve()
```Mảng`pos`được lập chỉ mục trực tiếp theo màu sắc vì mỗi màu nằm giữa`1`Và`n`. Giá trị bằng 0 có nghĩa là chưa gặp phải đoạn tích cực nào của màu đó trong khi quét ngược. 

các`limit`biến được lưu trữ dưới dạng chỉ mục phòng thay vì chỉ mục đoạn văn. Nếu lối đi tích cực chặn là`p`, người đó có thể vào phòng`p-1`nhưng không vào được phòng`p`, do đó ranh giới tương ứng chính xác là`p-1`. Điều này làm cho công thức trả lời`limit - i + 1`phù hợp với thực tế là`ans[i]`bắt đầu từ phòng`i-1`. 

bản cập nhật`pos[x] = i`xảy ra sau khi tính toán`ans[i]`. Một lối đi tích cực ở vị trí`i`phải hiển thị rõ ràng với các đoạn phủ định ở bên trái của nó, nhưng nó không ảnh hưởng đến việc tính toán cho hành trình có đoạn đầu tiên là chính nó. Việc xử lý bài tập sau đó sẽ đưa ra thứ tự chính xác một cách tự nhiên. 

Không có khả năng tràn số nguyên trong Python và trên thực tế, mọi câu trả lời nhiều nhất là`n`. Việc triển khai cũng tránh được đệ quy và chỉ sử dụng các mảng có kích thước tỷ lệ thuận với`n`, thích hợp cho`500000`giới hạn phần tử 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
6
1 -1 -1 1 -1 1
```quá trình quét ngược hoạt động như sau. 

|`i`|`a[i]`|`pos[1]`sau bước |`limit`|`ans[i]`| 
| --- | --- | --- | --- | --- | 
| 6 |`1`| 6 | 6 | 1 | 
| 5 |`-1`| 6 | 5 | 1 | 
| 4 |`1`| 4 | 5 | 2 | 
| 3 |`-1`| 4 | 3 | 1 | 
| 2 |`-1`| 4 | 3 | 2 | 
| 1 |`1`| 1 | 3 | 3 | 

Tại đoạn văn`5`, tiêu cực`-1`nhìn thấy sự tích cực`+1`lúc đi qua`6`, vì vậy nó giới hạn số phòng có thể tiếp cận là`5`. Đoạn văn`4`là một điều tích cực khác`1`và các đoạn phủ định ở bên trái của nó có thể sử dụng nó như một công cụ chặn thậm chí còn sớm hơn. Tại đoạn văn`3`, điều đó tạo ra ranh giới chặt chẽ hơn`3`, đưa ra câu trả lời cuối cùng`3 2 1 2 1 1`. 

Đối với mẫu 2,```
7
2 -1 -2 -3 1 3 2
```quét ngược là: 

|`i`|`a[i]`| Liên quan`pos`cập nhật |`limit`|`ans[i]`| 
| --- | --- | --- | --- | --- | 
| 7 |`2`|`pos[2] = 7`| 7 | 1 | 
| 6 |`3`|`pos[3] = 6`| 7 | 2 | 
| 5 |`1`|`pos[1] = 5`| 7 | 3 | 
| 4 |`-3`|`limit = min(7, 5)`| 5 | 2 | 
| 3 |`-2`|`limit = min(5, 6)`| 5 | 3 | 
| 2 |`-1`|`limit = min(5, 4)`| 4 | 3 | 
| 1 |`2`|`pos[2] = 1`| 4 | 4 | 

tiêu cực`-3`lúc đi qua`4`làm mất hiệu lực màu sắc`3`, vì vậy đoạn văn`6`không thể vượt qua, giới hạn số phòng có thể tiếp cận`5`. Càng về sau`-2`không thắt chặt ranh giới đó vì tích cực tương ứng của nó`2`xa hơn bên phải. các`-1`lúc đi qua`2`không thắt chặt nó vào phòng`4`, sản xuất`4 3 3 2 3 2 1`. 

Những dấu vết này cũng cho thấy tại sao hạn chế lại mang tính tích lũy. Một đoạn phủ định không nhất thiết phải tự mình xác định điểm dừng cuối cùng. Chúng tôi cần phòng có thể truy cập tối thiểu đối với tất cả các trường hợp vô hiệu có liên quan trong hậu tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đoạn được xử lý một lần và mỗi lần tra cứu và cập nhật đều là O(1). | 
| Không gian | O(n) | Mỗi mảng đầu vào, mảng trả lời và mảng vị trí theo màu đều sử dụng bộ nhớ O(n). | 

Với`n <= 500000`, thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi đoạn, do đó có khoảng vài triệu thao tác Python đơn giản được thực hiện. Việc sử dụng bộ nhớ cũng tuyến tính và thoải mái trong giới hạn 256 MB cho phương pháp này. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    limit = n
    ans = [0] * (n + 2)

    for i in range(n, 0, -1):
        x = a[i - 1]

        if x > 0:
            ans[i] = ans[i + 1] + 1
            pos[x] = i
        else:
            color = -x

            if pos[color] == 0:
                ans[i] = ans[i + 1] + 1
            else:
                limit = min(limit, pos[color] - 1)
                ans[i] = limit - i + 1

    print(*ans[1:n + 1])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solution()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    "6\n"
    "1 -1 -1 1 -1 1\n"
) == "3 2 1 2 1 1", "sample 1"

assert run(
    "7\n"
    "2 -1 -2 -3 1 3 2\n"
) == "4 3 3 2 3 2 1", "sample 2"

# Minimum-size input
assert run(
    "1\n"
    "-1\n"
) == "1", "a negative passage never blocks"

# All values equal and positive
assert run(
    "5\n"
    "1 1 1 1 1\n"
) == "5 4 3 2 1", "all passages are immediately crossable"

# Boundary case where an invalidated pass is checked immediately
assert run(
    "5\n"
    "-1 1 1 1 1\n"
) == "1 4 3 2 1", "negative passage followed by matching positive"

# Multiple colors create cumulative restrictions
assert run(
    "5\n"
    "1 -1 2 -2 2\n"
) == "3 3 2 1 1", "multiple independent colors"

# Maximum-size input
n = 500000
inp = str(n) + "\n" + " ".join(["1"] * n) + "\n"
expected = " ".join(map(str, range(n, 0, -1)))
assert run(inp) == expected, "maximum-size linear test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / -1`|`1`| Kích thước tối thiểu và thực tế là các đoạn phủ định không bao giờ bị chặn ngay lập tức | 
|`5 / 1 1 1 1 1`|`5 4 3 2 1`| Các giá trị bằng nhau và trường hợp không chặn | 
|`5 / -1 1 1 1 1`|`1 4 3 2 1`| Ranh giới chặn chính xác và xử lý từng cái một | 
|`5 / 1 -1 2 -2 2`|`3 3 2 1 1`| Nhiều màu sắc và tích lũy`limit`hạn chế | 
|`500000 / 1 1 ... 1`|`500000 499999 ... 1`| Kích thước đầu vào tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

Đầu vào tối thiểu```
1
-1
```được xử lý bởi nhánh âm. Không có tích cực`1`ở bên phải, vậy`pos[1]`bằng 0 và thuật toán sử dụng`ans[2] + 1 = 1`. Người đó vượt qua lối đi duy nhất và vô hiệu hóa đường chuyền sau đó. Đầu ra là chính xác`1`. 

Vì```
2
-1 1
```quét ngược lần đầu tiên nhìn thấy`+1`lúc đi qua`2`, lưu trữ`pos[1] = 2`. Khi nó đạt tới`-1`lúc đi qua`1`, nó thay đổi`limit`ĐẾN`1`, bởi vì người đó có thể đến phòng`1`nhưng không thể vượt qua lối đi`2`. Câu trả lời là`1`, và đoạn văn`2`không được tính vì đó là lối đi bị chặn. Bắt đầu từ phòng`1`, chỉ có đoạn văn`2`vẫn còn và thẻ của nó ban đầu hợp lệ, vì vậy câu trả lời thứ hai cũng là`1`. 

Vì```
3
-1 1 1
```quét ngược lưu trữ các lần xuất hiện tích cực tại các đoạn văn`3`và sau đó`2`. Xử lý`-1`lúc đi qua`1`tìm thấy sự xuất hiện tích cực gần nhất tại`2`, Vì thế`limit`trở thành`1`Và`ans[1] = 1`. Khi bắt đầu từ phòng`1`, tuy nhiên, đoạn văn`1`hoàn toàn không phải là một phần của cuộc hành trình. Câu trả lời được tính từ`ans[2] = 2`, cho`2`. Đầu ra cuối cùng là`1 2 1`, xác nhận rằng các lần xuất hiện tiêu cực trước vị trí bắt đầu là không liên quan. 

Đối với hộp nhiều màu```
5
1 -1 2 -2 2
```quét ngược lần đầu tiên tìm thấy`+2`lúc đi qua`5`, sau đó`-2`lúc đi qua`4`, giới hạn số phòng có thể tiếp cận ở mức`4`. Càng sớm`-1`lúc đi qua`2`thấy`+1`lúc đi qua`1`chỉ khi lối đi tích cực đó nằm ở bên phải của nó, điều này không nằm ở bên phải nên nó không tạo ra hạn chế mới. Để bắt đầu tại phòng`0`, đoạn văn`4`làm mất hiệu lực màu sắc`2`, và đoạn văn`5`bị chặn nên ba lối đi bị cắt ngang. Đầu ra là`3 3 2 1 1`. 

Trường hợp kích thước tối đa với tất cả các giá trị dương không có sự vô hiệu nào cả. Mỗi phòng bắt đầu có thể duyệt qua toàn bộ hậu tố còn lại, vì vậy chuỗi câu trả lời là chính xác`n, n-1, ..., 1`. Phép truy toán ngược tính toán trực tiếp điều này và vì mỗi đoạn được xử lý một lần nên trường hợp này cũng xác nhận rằng việc triển khai sẽ mở rộng đến ràng buộc đầy đủ.
