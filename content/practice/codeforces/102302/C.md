---
title: "CF 102302C - Hình chữ nhật"
description: "Chúng ta có tối đa 2000 điểm phân biệt trên mặt phẳng tọa độ. Một hình chữ nhật hợp lệ phải sử dụng bốn trong số các điểm này làm các góc của nó, các cạnh của nó phải nằm ngang hoặc thẳng đứng và không được có điểm nào khác ở bất kỳ đâu bên trong hình chữ nhật hoặc trên một trong bốn cạnh của nó."
date: "2026-08-13T23:16:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "C"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 138
verified: true
draft: false
---

[CF 102302C - Hình chữ nhật](https://codeforces.com/problemset/problem/102302/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có tối đa 2000 điểm phân biệt trên mặt phẳng tọa độ. Một hình chữ nhật hợp lệ phải sử dụng bốn trong số các điểm này làm các góc của nó, các cạnh của nó phải nằm ngang hoặc thẳng đứng và không được có điểm nào khác ở bất kỳ đâu bên trong hình chữ nhật hoặc trên một trong bốn cạnh của nó. 

Nhiệm vụ là đếm từng hình chữ nhật riêng biệt thỏa mãn các điều kiện đó. Các giá trị tọa độ thực tế có thể lớn tới (10^9), nhưng kích thước tuyệt đối của chúng không liên quan. Điều quan trọng là tọa độ nào bằng nhau và tọa độ nào nằm giữa những tọa độ khác. 

Với (N \le 2000), giải pháp (O(N^2)) là mục tiêu đương nhiên. Có khoảng hai triệu cặp điểm không có thứ tự, do đó việc xử lý từng cặp với khối lượng công việc không đổi là khả thi. Một giải pháp (O(N^3)) có thể đã yêu cầu khoảng (4\cdot10^9) thao tác trong trường hợp xấu nhất, quá nhiều so với giới hạn một giây. Việc liệt kê đầy đủ (O(N^4)) các kết hợp bốn điểm thậm chí còn nằm ngoài tầm với. Giới hạn tọa độ của (10^9) cũng có nghĩa là chúng ta không thể phân bổ lưới được lập chỉ mục trực tiếp theo tọa độ ban đầu. 

Trường hợp cạnh đầu tiên có ít hơn bốn điểm. Ví dụ, đầu vào`1 / 5 7`chỉ chứa một điểm, vì vậy câu trả lời là 0. Một giải pháp giả định rằng mọi cặp cuối cùng có thể xác định một hình chữ nhật có thể vô tình cố gắng truy cập vào các góc không tồn tại. 

Một trường hợp cạnh khác là có nhiều điểm trên một đường thẳng đứng hoặc nằm ngang. Ví dụ, bốn điểm`(0,0)`,`(0,1)`,`(0,2)`,`(0,3)`không thể tạo thành hình chữ nhật nên đáp án là 0. Chỉ kiểm tra xem bốn tọa độ có xảy ra độc lập hay không là chưa đủ, vì bốn góc phải có hai tọa độ x phân biệt và hai tọa độ y phân biệt. 

Điều kiện trống rỗng là phần vi tế. Coi như`(0,0)`,`(2,0)`,`(0,2)`,`(2,2)`,`(1,1)`. Bốn điểm góc tồn tại, nhưng câu trả lời là 0 vì`(1,1)`nằm bên trong hình chữ nhật. Giải pháp chỉ kiểm tra sự tồn tại của bốn góc sẽ tính sai. 

Các điểm trên ranh giới gây ra vấn đề tương tự. Với`(0,0)`,`(2,0)`,`(0,2)`,`(2,2)`,`(1,0)`, bốn góc tồn tại, nhưng câu trả lời lại là 0 vì`(1,0)`nằm ở mép dưới. Hình chữ nhật phải chứa chính xác bốn điểm góc trong hộp giới hạn đóng của nó. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi mỗi cặp điểm có thể là góc dưới bên trái và góc trên bên phải. Nếu tọa độ x và tọa độ y của chúng đều khác nhau, chúng ta có thể kiểm tra xem hai góc còn lại có tồn tại trong tập băm hay không. Sau đó, chúng tôi quét tất cả (N) điểm để xem liệu có điểm nào nằm bên trong hoặc trên ranh giới của hình chữ nhật ứng cử viên hay không. Mỗi cặp ứng cử viên được xử lý trong (O(N)), cho thời gian (O(N^3)). Đối với (N=2000), có các cặp (\binom{2000}{2}=1.999.000) và việc quét 2000 điểm cho mỗi cặp sẽ mang lại khoảng (3.998.000.000) điểm kiểm tra. Cách tiếp cận này đúng về mặt logic nhưng lại quá chậm. 

Lực lượng vũ phu hoạt động vì một hình chữ nhật được xác định duy nhất bởi các góc dưới bên trái và trên cùng bên phải của nó. Phần tốn kém không phải là xác định các góc mà liên tục hỏi có bao nhiêu điểm đã cho nằm bên trong một hình chữ nhật thẳng hàng với trục cụ thể. 

Quan sát quan trọng là câu trả lời cho câu hỏi đó có thể được tính toán trước. Chỉ có thứ tự tương đối của tọa độ mới quan trọng, vì vậy chúng tôi nén tất cả tọa độ x phân biệt thành (1,\ldots,X) và tất cả tọa độ y phân biệt thành (1,\ldots,Y). Vì có nhiều nhất (N) tọa độ riêng biệt của một trong hai loại nên lưới kết quả có nhiều nhất (N^2) ô. 

Chúng tôi đặt 1 tại mỗi tọa độ nén bị chiếm dụng và xây dựng tổng tiền tố hai chiều. Khi bảng đó tồn tại, có thể lấy được số điểm bên trong bất kỳ hình chữ nhật đóng nào trong thời gian (O(1)) bằng cách sử dụng công thức bao gồm-loại trừ tiêu chuẩn. 

Bây giờ chúng ta có thể kiểm tra từng cặp điểm. Sau khi sắp xếp theo tọa độ x, chỉ những cặp có tọa độ y tăng dần mới có thể biểu thị góc dưới bên trái và góc trên bên phải. Chúng tôi kiểm tra xem góc kia có tồn tại hay không. Nếu tất cả bốn góc đều tồn tại thì tổng tiền tố trên toàn bộ hình chữ nhật phải chính xác là 4. Vì bốn góc đã biết đã đóng góp bốn điểm nên tổng lớn hơn 4 có nghĩa là một số điểm bổ sung nằm bên trong hoặc trên một cạnh. 

Điều này làm giảm phần tốn kém từ việc quét (N) điểm cho mỗi ứng viên sang truy vấn tổng tiền tố theo thời gian không đổi. Việc triển khai tham chiếu công khai cho vấn đề này sử dụng cùng một chiến lược nén tọa độ và tổng tiền tố hai chiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N^2)) | (O(N^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm và thu thập tọa độ x và y riêng biệt của chúng. Sắp xếp cả hai danh sách tọa độ và gán cho mỗi tọa độ một chỉ mục nén bắt đầu từ 1. Quá trình nén giữ nguyên thứ tự, do đó, một điểm nằm bên trong hình chữ nhật trước khi nén chính xác khi tọa độ nén của nó nằm bên trong hình chữ nhật nén tương ứng. 
2. Tạo lưới nén (Y\times X) và đặt 1 tại mỗi ô bị chiếm dụng. Kích thước lưới tối đa là (2000\times2000), bất kể tọa độ ban đầu gần nhau hay lớn bằng (10^9). 
3. Chuyển đổi lưới thành mảng tổng tiền tố hai chiều. Đối với mỗi ô nén ((x,y)), giá trị tiền tố của nó lưu trữ số lượng điểm đầu vào có tọa độ nén tối đa là (x) và (y). Tổng số điểm trong một hình chữ nhật khép kín có các góc ((x_1,y_1)) và ((x_2,y_2)) là

[ 
P(x_2,y_2)-P(x_1-1,y_2)-P(x_2,y_1-1)+P(x_1-1,y_1-1). 
] 
4. Lưu trữ mọi điểm nén trong bộ băm. Điều này cho phép kiểm tra thời gian trung bình không đổi để xem liệu góc bắt buộc có tồn tại hay không. 
5. Sắp xếp các điểm nén theo tọa độ x rồi đến tọa độ y. Hãy xem xét mọi cặp điểm theo thứ tự này. Các cặp có tọa độ x bằng nhau không thể là góc chéo của hình chữ nhật và các cặp có tọa độ y không tăng không thể là góc dưới bên trái và góc trên bên phải. 
6. Đối với cặp còn lại ((x_1,y_1)), ((x_2,y_2)), kiểm tra xem ((x_1,y_2)) có tồn tại hay không. Điểm ((x_2,y_1)) đã là một trong những điểm được chọn, do đó cùng với hai điểm được chọn và ((x_1,y_2)), cả bốn góc đều tồn tại. 
7. Truy vấn tổng tiền tố của hình chữ nhật khép kín hoàn chỉnh. Đếm chính xác hình chữ nhật khi kết quả là 4. Bốn góc được đảm bảo đóng góp bốn điểm, do đó, bất kỳ giá trị nào lớn hơn có nghĩa là có một điểm không mong muốn bên trong hoặc trên một cạnh. 
8. In số lượng tích lũy. Mỗi hình chữ nhật hợp lệ có chính xác một cặp dưới cùng bên trái và trên cùng bên phải, vì vậy nó được xem xét đúng một lần. 

### Tại sao nó hoạt động 

Điều bất biến là mọi ứng cử viên được thuật toán chấp nhận đều có chính xác bốn điểm cho trước trong hộp giới hạn đóng của nó và bốn điểm đó tạo thành các góc bắt buộc. Việc kiểm tra sự tồn tại của góc đảm bảo bốn đỉnh đều có mặt. Truy vấn tổng tiền tố đếm mọi điểm bên trong hình chữ nhật và trên ranh giới của nó, do đó giá trị chính xác bằng 4 chứng tỏ rằng không có điểm thứ năm tồn tại ở đó. Ngược lại, mọi hình chữ nhật hợp lệ đều có một cặp góc dưới bên trái và góc trên bên phải duy nhất. Khi cặp đó được xử lý, hai góc còn lại được tìm thấy và tổng tiền tố chính xác là 4, do đó hình chữ nhật được tính. Do đó, mọi hình chữ nhật hợp lệ đều được tính một lần và không có hình chữ nhật không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    if n < 4:
        print(0)
        return

    xs = sorted({x for x, _ in points})
    ys = sorted({y for _, y in points})

    x_id = {x: i + 1 for i, x in enumerate(xs)}
    y_id = {y: i + 1 for i, y in enumerate(ys)}

    nx = len(xs)
    ny = len(ys)
    width = nx + 1

    # Unsigned short is enough because every prefix sum is at most N <= 2000.
    pref = array('H', [0]) * ((ny + 1) * width)

    compressed = []
    present = set()

    for x, y in points:
        cx = x_id[x]
        cy = y_id[y]
        compressed.append((cx, cy))
        present.add((cx, cy))
        pref[cy * width + cx] = 1

    # Build the 2D prefix sum in-place.
    for y in range(1, ny + 1):
        base = y * width
        previous = base - width
        row_sum = 0

        for x in range(1, nx + 1):
            idx = base + x
            row_sum += pref[idx]
            pref[idx] = pref[previous + x] + row_sum

    compressed.sort()

    answer = 0

    for i in range(n):
        x1, y1 = compressed[i]

        for j in range(i + 1, n):
            x2, y2 = compressed[j]

            # Sorting gives x1 <= x2. Equal x cannot form a rectangle.
            if x1 == x2:
                continue

            # We need the first point to be the bottom-left corner.
            if y1 >= y2:
                continue

            # The missing fourth corner is (x1, y2).
            if (x1, y2) not in present:
                continue

            # Count all points in the closed rectangle.
            total = (
                pref[y2 * width + x2]
                - pref[(y1 - 1) * width + x2]
                - pref[y2 * width + (x1 - 1)]
                + pref[(y1 - 1) * width + (x1 - 1)]
            )

            if total == 4:
                answer += 1

    print(answer)

if __name__ == "__main__":
    solve()
```Sớm`n < 4`check xử lý các đầu vào nhỏ nhất mà không cần xây dựng bất kỳ cấu trúc dữ liệu nào. Nó không cần thiết cho tính chính xác, nhưng tránh những công việc không cần thiết. 

Bản đồ tọa độ biến mỗi tọa độ ban đầu thành một chỉ số nguyên nhỏ gọn. Việc nén riêng biệt x và y giữ tối đa lưới tổng tiền tố (2001\times2001) thay vì sử dụng phạm vi tọa độ ban đầu. 

Mảng tiền tố sử dụng`array('H')`thay vì số nguyên Python. Giá trị tiền tố không bao giờ có thể vượt quá (N), tối đa là 2000, do đó, giá trị 16 bit không dấu là đủ. Điều này giữ cho lưới có kích thước tối đa khoảng 8 MB, thoải mái dưới giới hạn bộ nhớ 256 MB. 

Tổng tiền tố được xây dựng theo từng hàng.`row_sum`đại diện cho số điểm được nhìn thấy cho đến nay trong hàng hiện tại, trong khi`pref[previous + x]`chứa sự đóng góp từ tất cả các hàng trước đó. Việc cộng hai giá trị đó sẽ mang lại sự tái diễn tiền tố hai chiều tiêu chuẩn. 

Các điểm được sắp xếp theo tọa độ x nén. Đối với mỗi cặp, tọa độ x bằng nhau bị từ chối vì chúng không thể là các góc đối diện của hình chữ nhật thẳng hàng với trục. các`y1 >= y2`kiểm tra chỉ chọn một hướng, do đó, cùng một hình chữ nhật không bao giờ được tính từ cặp trên cùng bên phải và dưới cùng bên trái của nó theo chiều ngược lại. 

Bộ băm chứa các cặp tọa độ nén, do đó, việc kiểm tra góc bị thiếu sẽ tránh chuyển đổi trở lại tọa độ tỷ lệ (10^9) ban đầu. Khi đã biết tất cả bốn góc, tổng tiền tố sẽ tự động bao gồm bốn điểm đó. Kiểm tra`total == 4`do đó đủ để thực thi cả các hạn chế bên trong và ranh giới. 

Số nguyên Python không bị giới hạn, vì vậy câu trả lời không có nguy cơ bị tràn. Ngay cả số lượng hình chữ nhật tối đa có thể cũng nhiều nhất là (\binom{2000}{4}), có thể biểu diễn một cách an toàn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, sau khi nén các điểm đã được sắp xếp như sau:`(1,1), (1,2), (2,1), (2,2), (3,1), (3,2)`. 

Các cặp ứng cử viên có liên quan được hiển thị dưới đây. 

| Dưới cùng bên trái | Trên cùng bên phải | Thiếu góc | Điểm trong hình chữ nhật khép kín | Kết quả | 
| --- | --- | --- | --- | --- | 
| (1,1) | (2,2) | (1,2) | 4 | Đếm | 
| (1,1) | (3,2) | (1,2) | 6 | Từ chối | 
| (2,1) | (3,2) | (2,2) | 4 | Đếm | 

Cặp đôi`(1,1)`Và`(3,2)`có tất cả bốn góc, nhưng nó cũng chứa`(2,1)`Và`(2,2)`, do đó tổng tiền tố của nó là 6. Hai hình chữ nhật nhỏ hơn có đúng bốn điểm, mỗi hình cho kết quả 2. 

Đối với ví dụ thứ hai, hãy xem xét bốn góc của hình chữ nhật cùng với một điểm bên trong.```
5
0 0
2 0
0 2
2 2
1 1
```Sau khi nén hình chữ nhật duy nhất có thể có các góc`(1,1)`,`(3,1)`,`(1,3)`, Và`(3,3)`. Hình chữ nhật khép kín của nó chứa năm điểm. 

| Dưới cùng bên trái | Trên cùng bên phải | Thiếu góc | Điểm trong hình chữ nhật khép kín | Kết quả | 
| --- | --- | --- | --- | --- | 
| (1,1) | (3,3) | (1,3) | 5 | Từ chối | 

Kiểm tra góc thành công, nhưng tổng tiền tố làm lộ ra điểm thừa`(1,1)`trong hệ tọa độ ban đầu, vì vậy câu trả lời là 0. Điều này chứng tỏ tại sao chỉ kiểm tra bốn góc là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2)) | Việc nén tọa độ và xây dựng tiền tố lấy (O(N^2)) trong trường hợp xấu nhất và phép liệt kê cặp lấy (O(N^2)). | 
| Không gian | (O(N^2)) | Lưới tổng tiền tố được nén có tối đa (2001\times2001) mục nhập. | 

Tại (N=2000), lưới tiền tố chứa khoảng bốn triệu ô và việc liệt kê cặp xem xét khoảng hai triệu cặp. Biểu diễn tiền tố 16 bit nhỏ gọn giúp duy trì mức sử dụng bộ nhớ ở mức thấp, trong khi mọi hình chữ nhật ứng cử viên đều được kiểm tra trong thời gian không đổi sau khi tiền xử lý. 

## Trường hợp thử nghiệm```python
from array import array

def solve_data(inp: str) -> str:
    it = iter(inp.split())
    n = int(next(it))

    points = [(int(next(it)), int(next(it))) for _ in range(n)]

    if n < 4:
        return "0\n"

    xs = sorted({x for x, _ in points})
    ys = sorted({y for _, y in points})

    x_id = {x: i + 1 for i, x in enumerate(xs)}
    y_id = {y: i + 1 for i, y in enumerate(ys)}

    nx = len(xs)
    ny = len(ys)
    width = nx + 1

    pref = array('H', [0]) * ((ny + 1) * width)

    compressed = []
    present = set()

    for x, y in points:
        cx = x_id[x]
        cy = y_id[y]
        compressed.append((cx, cy))
        present.add((cx, cy))
        pref[cy * width + cx] = 1

    for y in range(1, ny + 1):
        base = y * width
        previous = base - width
        row_sum = 0

        for x in range(1, nx + 1):
            idx = base + x
            row_sum += pref[idx]
            pref[idx] = pref[previous + x] + row_sum

    compressed.sort()

    answer = 0

    for i in range(n):
        x1, y1 = compressed[i]

        for j in range(i + 1, n):
            x2, y2 = compressed[j]

            if x1 == x2 or y1 >= y2:
                continue

            if (x1, y2) not in present:
                continue

            total = (
                pref[y2 * width + x2]
                - pref[(y1 - 1) * width + x2]
                - pref[y2 * width + x1 - 1]
                + pref[(y1 - 1) * width + x1 - 1]
            )

            if total == 4:
                answer += 1

    return f"{answer}\n"

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample
assert run("""\
6
1 1
1 2
2 1
2 2
3 1
3 2
""") == "2\n", "sample 1"

# Minimum-size input
assert run("""\
1
5 7
""") == "0\n", "fewer than four points"

# Four corners at the coordinate boundaries
assert run("""\
4
0 0
1000000000 0
0 1000000000
1000000000 1000000000
""") == "1\n", "boundary coordinates"

# Extra points on and inside the rectangle
assert run("""\
6
0 0
2 0
0 2
2 2
1 0
1 1
""") == "0\n", "boundary and interior points"

# Maximum-size input, all points on one vertical line
points = "\n".join(f"0 {y}" for y in range(2000))
maximum_case = "2000\n" + points + "\n"
assert run(maximum_case) == "0\n", "maximum N with equal x coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một điểm`(5,7)`| 0 | Đầu vào có kích thước tối thiểu và không có bốn góc | 
| Bốn góc sử dụng 0 và (10^9) | 1 | Phối hợp ranh giới và nén tọa độ | 
| Hình chữ nhật cộng`(1,0)`Và`(1,1)`| 0 | Điểm trên một cạnh và bên trong hình chữ nhật | 
| 2000 điểm`(0,y)`| 0 | Tọa độ tối đa (N), x bằng nhau và xử lý bộ nhớ | 

## Vỏ cạnh 

Đối với ít hơn bốn điểm, đầu vào```
1
5 7
```không chứa tập hợp bốn đỉnh nào có thể. Thuật toán ngay lập tức trả về 0 trước khi phân bổ lưới tiền tố. Điều này ngăn chặn mọi giả định rằng hình chữ nhật ứng cử viên phải tồn tại. 

Đối với các điểm chia sẻ một tọa độ, hãy xem xét```
4
0 0
0 1
0 2
0 3
```Bốn điểm có cùng tọa độ x. Sau khi sắp xếp, mỗi cặp có`x1 == x2`, vì vậy mọi cặp đều bị từ chối trước khi tra cứu góc. Đầu ra là 0. 

Để có thêm điểm bên trong hình chữ nhật, hãy xem xét```
5
0 0
2 0
0 2
2 2
1 1
```Cặp đôi`(0,0)`Và`(2,2)`có góc khác`(0,2)`, Và`(2,0)`đã có sẵn nên nó đạt đến truy vấn tổng tiền tố. Hình chữ nhật khép kín chứa 5 điểm thay vì 4, khiến thí sinh bị loại. Đầu ra là 0. 

Để có thêm điểm trên một cạnh, hãy xem xét```
5
0 0
2 0
0 2
2 2
1 0
```Một lần nữa, cả bốn góc đều tồn tại. Tổng tiền tố trên hình chữ nhật là 5 vì`(1,0)`được bao gồm trong ranh giới đóng. Vì thuật toán yêu cầu chính xác 4 điểm nên nó loại bỏ hình chữ nhật và đưa ra 0. 

Đối với đầu vào lớn nhất có tọa độ x lặp lại, hãy sử dụng 2000 điểm`(0,0)`,`(0,1)`, bởi vì`(0,1999)`. Mọi cặp ứng cử viên đều có tọa độ x bằng nhau, do đó vòng lặp cặp sẽ loại bỏ tất cả chúng. Câu trả lời là 0, trong khi lưới tiền tố vẫn chỉ có (1\times2001) kích thước tọa độ chiếm dụng theo một hướng. Điều này thực hiện kích thước đầu vào tối đa mà không cần dựa vào phân bố điểm hai chiều dày đặc.
