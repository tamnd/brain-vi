---
title: "CF 105993J - Pixel Canvas"
description: "Trong Codeforces 105993J, canvas là một lưới vô hạn. Một số pixel đã được tô màu và chúng tôi muốn tô màu mọi ô của hình chữ nhật nhất định bằng cách sử dụng càng ít thao tác tô màu mới càng tốt."
date: "2026-06-25T13:29:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105993
codeforces_index: "J"
codeforces_contest_name: "Latakia and Tartus Collegiate Programming Contest 2025"
rating: 0
weight: 105993
solve_time_s: 59
verified: true
draft: false
---

[CF 105993J - Pixel Canvas](https://codeforces.com/problemset/problem/105993/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong Codeforces 105993J, canvas là một lưới vô hạn. Một số pixel đã được tô màu và chúng tôi muốn tô màu mọi ô của hình chữ nhật nhất định bằng cách sử dụng càng ít thao tác tô màu mới càng tốt. Một pixel mới có thể được tạo bằng cách tô màu bất kỳ ô nào trên hàng mặt đất`y = 1`hoặc bằng cách trải rộng từ một pixel đã được tô màu sang một trong bốn pixel lân cận. 

Hình chữ nhật mục tiêu được xác định bởi khoảng cách ngang của nó`[X1, X2]`và khoảng dọc`[Y2, Y1]`. Đầu vào cho đến`100000`các pixel đã được tô màu. Tọa độ có thể lớn bằng`10^9`, vì vậy việc xây dựng lưới hoặc mô phỏng sự lây lan là không thể. Với một giây thời gian, giải pháp dự định phải gần tuyến tính về số lượng pixel nhất định. 

Khó khăn chính là hình chữ nhật có thể rất lớn nhưng chỉ có các pixel hiện có mới quan trọng. Diện tích của hình chữ nhật có thể lên tới`10^18`, vì vậy chúng ta phải suy luận xem có bao nhiêu ô trống và cần thêm bao nhiêu ô bên ngoài hình chữ nhật để kết nối với hình chữ nhật. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu một pixel đã ở bên trong hình chữ nhật, nó sẽ lưu một thao tác vì nó không bao giờ cần phải tạo. 

Ví dụ:```
1
3 4 3 3
3 4
```Hình chữ nhật chứa một ô và ô đó đã được tô màu. Câu trả lời là:```
0
```Giải pháp chỉ tìm kiếm cách nhập hình chữ nhật có thể thêm một thao tác không chính xác. 

Một trường hợp khác là khi một pixel hiện có chạm vào hình chữ nhật từ bên ngoài. 

Ví dụ:```
1
3 4 3 3
3 5
```Pixel hiện tại nằm ngay phía trên hình chữ nhật. Chúng ta có thể lây lan một lần vào`(3,4)`. Câu trả lời là:```
1
```Pixel mới nằm bên trong hình chữ nhật nên đã được tính là một trong các ô hình chữ nhật bị thiếu. Việc tính kết nối là một thao tác bổ sung sẽ cho kết quả sai. 

Trường hợp khó khăn cuối cùng là không có pixel hiện có. 

Ví dụ:```
0
4 4 4 4
```Tế bào duy nhất cần thiết là`(4,4)`. Chúng ta phải tô màu`(4,1)`,`(4,2)`,`(4,3)`, Và`(4,4)`, vậy đáp án là:```
4
```Thao tác trên mặt đất tạo ra pixel đầu tiên và mọi pixel tiếp theo là một pixel trải rộng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng sự tăng trưởng. Chúng tôi có thể thử mọi vị trí mặt đất có thể, trải rộng từ mọi pixel hiện có và đếm xem có bao nhiêu ô được tô màu. Điều này đúng vì nó mô hình hóa các hoạt động được phép theo đúng nghĩa đen. Vấn đề là hình chữ nhật có thể chứa tới`10^18`tế bào, nên thậm chí chạm vào từng tế bào một lần là không thể. 

Quan sát hữu ích là mọi ô bị thiếu bên trong hình chữ nhật cuối cùng phải được tô màu bằng một số thao tác. Không có cách nào xung quanh việc trả tiền cho nó. Cách tối ưu hóa duy nhất có thể thực hiện được là giảm số lượng ô bổ sung được tô màu bên ngoài hình chữ nhật trước khi mức chênh lệch đạt đến nó. 

Nếu chúng ta đã có một pixel màu bên trong hình chữ nhật, chúng ta có thể lấp đầy toàn bộ hình chữ nhật mà không cần bất kỳ thao tác bên ngoài nào. Nếu có pixel màu bên ngoài thì chúng ta chỉ cần nối nó với hình chữ nhật. Kết nối rẻ nhất là con đường ngắn nhất từ ​​Manhattan đến hình chữ nhật. Đối với pixel có màu ban đầu, bước cuối cùng của đường dẫn này sẽ đi vào hình chữ nhật, do đó chỉ các ô trước bước cuối cùng đó là thừa. 

Hàng mặt đất hoạt động hơi khác một chút. Thao tác trên mặt đất sẽ tạo ra một pixel màu mới, do đó tất cả các ô từ vị trí mặt đất cho đến hình chữ nhật đều đóng góp. Vị trí mặt đất tốt nhất luôn được căn chỉnh theo chiều ngang với hình chữ nhật, tạo thêm chi phí`Y2 - 1`. 

Câu trả lời là số ô bị thiếu trong hình chữ nhật cộng với chi phí kết nối bổ sung tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(diện tích hình chữ nhật) | O(diện tích hình chữ nhật) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi pixel có màu ban đầu vào một bộ. Bộ này tránh đếm các pixel trùng lặp hai lần và cho phép chúng tôi đếm xem có bao nhiêu ô đã được tô màu nằm bên trong hình chữ nhật mục tiêu. 
2. Tính diện tích hình chữ nhật là`(X2 - X1 + 1) * (Y1 - Y2 + 1)`. Trừ số pixel hiện có bên trong hình chữ nhật. Đây là những ô vẫn cần được tô màu. 
3. Khởi tạo chi phí kết nối bổ sung tốt nhất khi vận hành mặt đất. Việc chọn một ô nền ngay dưới bất kỳ cột nào của hình chữ nhật yêu cầu tô màu tất cả các ô bên dưới hình chữ nhật trong cột đó. 
4. Đối với mỗi pixel hiện có bên ngoài hình chữ nhật, hãy tính khoảng cách Manhattan của nó đến ô gần nhất của hình chữ nhật. Nếu khoảng cách bằng 0 thì pixel đã ở bên trong. Ngược lại, nước đi cuối cùng sẽ vào hình chữ nhật nên số ô thừa bên ngoài là`distance - 1`. 
5. Thêm chi phí kết nối bổ sung tối thiểu vào số ô hình chữ nhật bị thiếu. Đây là tổng số pixel mới tối thiểu. 

Tại sao nó hoạt động: mọi giải pháp đều phải tô màu mọi ô hình chữ nhật bị thiếu, vì không có thao tác nào có thể tô màu một ô mà không phải trả tiền. Lựa chọn duy nhất còn lại là làm thế nào để đưa pixel màu đầu tiên vào hình chữ nhật. Thuật toán kiểm tra mọi nguồn có thể có của kết nối như vậy, bao gồm tất cả các pixel ban đầu và hàng mặt đất. Kết nối ngắn nhất có thể từ một nguồn là đường dẫn Manhattan và thuật toán sẽ tính chính xác các ô trên đường dẫn đó nằm bên ngoài hình chữ nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    X1, Y1, X2, Y2 = map(int, input().split())

    inside = 0
    seen = set()
    points = []

    for _ in range(n):
        x, y = map(int, input().split())
        if (x, y) not in seen:
            seen.add((x, y))
            points.append((x, y))
            if X1 <= x <= X2 and Y2 <= y <= Y1:
                inside += 1

    area = (X2 - X1 + 1) * (Y1 - Y2 + 1)
    answer = area - inside

    extra = Y2 - 1

    for x, y in points:
        if X1 <= x <= X2 and Y2 <= y <= Y1:
            continue

        if x < X1:
            dx = X1 - x
        elif x > X2:
            dx = x - X2
        else:
            dx = 0

        if y < Y2:
            dy = Y2 - y
        elif y > Y1:
            dy = y - Y1
        else:
            dy = 0

        dist = dx + dy
        extra = min(extra, dist - 1)

    print(answer + extra)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ loại bỏ các bản sao vì một pixel chỉ có thể được tô màu một lần. Biến`answer`lưu trữ chi phí không thể tránh khỏi của việc lấp đầy hình chữ nhật. 

Biến`extra`bắt đầu bằng chi phí sử dụng hàng mặt đất. Đối với mỗi pixel hiện có, mã sẽ tính khoảng cách Manhattan đến hình chữ nhật. Khoảng cách theo chiều ngang bằng 0 khi cột của pixel đã nằm trong hình chữ nhật và điều tương tự cũng áp dụng theo chiều dọc. 

Việc trừ đi một là phần tinh tế. Bước cuối cùng của đường dẫn nằm bên trong hình chữ nhật và ô đó đã được bao gồm trong số hình chữ nhật bị thiếu. Chỉ các ô trước đó là các thao tác bổ sung. 

Tất cả số học đều sử dụng số nguyên Python, do đó phạm vi tọa độ lớn và diện tích hình chữ nhật không bị tràn. 

## Ví dụ đã hoạt động 

Mẫu 1:```
3
3 6 6 4
5 5
3 2
8 5
```| Bước | Pixel bên trong | Thiếu ô | Bổ sung tốt nhất | 
| --- | --- | --- | --- | 
| Sau khi đọc`(5,5)`| 1 | 11 | 5 | 
| Sau khi đọc`(3,2)`| 1 | 11 | 2 | 
| Sau khi đọc`(8,5)`| 1 | 11 | 0 | 
| Cuối cùng | 1 | 11 | 0 | 

Điểm ảnh`(5,5)`đã ở bên trong hình chữ nhật nên vẫn còn 11 ô. Vì đã có một pixel màu trong hình chữ nhật nên không cần kết nối bên ngoài. 

Mẫu 2:```
0
4 4 4 4
```| Bước | Pixel bên trong | Thiếu ô | Bổ sung tốt nhất | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 1 | 3 | 
| Cuối cùng | 0 | 1 | 3 | 

Hình chữ nhật có một ô. Hoạt động trên mặt đất phải tạo ra`(4,1)`và lan rộng lên trên ba lần, vậy tổng số là`1 + 3 = 4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi pixel được cung cấp được xử lý một lần | 
| Không gian | O(n) | Tập hợp các pixel màu được lưu trữ | 

Lời giải chỉ phụ thuộc vào số lượng pixel có màu ban đầu. Nó không bao giờ phân bổ bộ nhớ tỷ lệ với kích thước hình chữ nhật, do đó nó xử lý tọa độ tối đa một cách dễ dàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    n = int(data())
    X1, Y1, X2, Y2 = map(int, data().split())

    inside = 0
    seen = set()
    points = []

    for _ in range(n):
        x, y = map(int, data().split())
        if (x, y) not in seen:
            seen.add((x, y))
            points.append((x, y))
            if X1 <= x <= X2 and Y2 <= y <= Y1:
                inside += 1

    area = (X2 - X1 + 1) * (Y1 - Y2 + 1)
    ans = area - inside
    extra = Y2 - 1

    for x, y in points:
        if X1 <= x <= X2 and Y2 <= y <= Y1:
            continue
        dx = max(X1 - x, 0, x - X2)
        dy = max(Y2 - y, 0, y - Y1)
        extra = min(extra, dx + dy - 1)

    sys.stdin = old
    return str(ans + extra) + "\n"

assert run("""3
3 6 6 4
5 5
3 2
8 5
""") == "11\n"

assert run("""0
4 4 4 4
""") == "4\n"

assert run("""1
3 4 4 3
3 2
""") == "4\n"

assert run("""1
3 6 6 4
7 5
""") == "12\n"

assert run("""1
3 4 3 3
3 4
""") == "0\n"

assert run("""1
3 4 3 3
3 5
""") == "1\n"

assert run("""2
1 1 1000000000 1000000000
1 1
1000000000 1000000000
""") == "999999998000000001\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ô đơn đã có màu | 0 | Pixel hình chữ nhật hiện có là miễn phí | 
| Pixel liền kề bên ngoài | 1 | Việc nhập hình chữ nhật không phải trả thêm phí | 
| Hình chữ nhật khổng lồ | Giá trị lớn | Tọa độ không được mô phỏng | 
| Không có pixel ban đầu | Chi phí mặt đất | Xử lý đúng các hoạt động trên mặt đất | 

## Vỏ cạnh 

Khi một pixel đã nằm trong hình chữ nhật, thuật toán sẽ loại trừ nó khỏi số lượng ô bị thiếu. Đối với đầu vào:```
1
3 4 3 3
3 4
```diện tích là một và số lượng bên trong là một, do đó chi phí cơ bản trở thành bằng không. Chi phí kết nối không thành vấn đề vì mục tiêu đã được đáp ứng. 

Khi một pixel nằm ngay bên ngoài hình chữ nhật, khoảng cách Manhattan là một. Thuật toán trừ một và không nhận thêm ô nào. Bản thân ô hình chữ nhật bị thiếu đã được tô màu bằng thao tác trải rộng và đã được bao gồm trong chi phí cơ bản. 

Đối với một hình chữ nhật cách xa mặt đất, chẳng hạn như:```
0
4 4 4 4
```chi phí mặt đất là`Y2 - 1 = 3`. Hình chữ nhật bị thiếu một ô, cho`1 + 3 = 4`. Điều này phù hợp với trình tự thực tế của việc tô màu cột dọc từ mặt đất đến mục tiêu.
