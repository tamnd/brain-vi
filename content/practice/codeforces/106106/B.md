---
title: "CF 106106B - \u041c\u0438\u0441\u0442\u0435\u0440 \u041c\u0438\u0441\u0438\u043a\u0441"
description: "Chúng ta có một tập hợp các lỗ golf được đặt trên mặt phẳng tọa độ. Một cầu thủ bắt đầu bóng tại một điểm do Meeseeks gợi ý. Chuyển động duy nhất được phép bao gồm hai cú đánh: đầu tiên bóng chỉ có thể di chuyển sang trái và sau đó nó chỉ có thể di chuyển xuống dưới."
date: "2026-06-25T11:40:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106106
codeforces_index: "B"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u042e\u043d\u0438\u043e\u0440\u044b 2024"
rating: 0
weight: 106106
solve_time_s: 36
verified: true
draft: false
---

[CF 106106B - \u041c\u0438\u0441\u0442\u0435\u0440 \u041c\u0438\u0441\u0438\u043a\u0441](https://codeforces.com/problemset/problem/106106/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các lỗ golf được đặt trên mặt phẳng tọa độ. Một cầu thủ bắt đầu bóng tại một điểm do Meeseeks gợi ý. Chuyển động duy nhất được phép bao gồm hai cú đánh: đầu tiên bóng chỉ có thể di chuyển sang trái và sau đó nó chỉ có thể di chuyển xuống dưới. Độ dài của hai ảnh có thể là bất kỳ giá trị không âm nào. 

Đối với vị trí xuất phát`(a, b)`, vị trí cuối cùng sau hai lần đánh này có thể là bất kỳ lỗ nào có tọa độ thỏa mãn`x <= a`Và`y <= b`. Thứ tự bắn không làm thay đổi điều kiện này, vì phát đầu tiên chỉ giảm tọa độ x và phát thứ hai chỉ giảm tọa độ y. 

Đầu vào cho đến`100000`lỗ và lên đến`100000`gợi ý. Đối với mọi đề xuất, chúng tôi phải trả lời liệu có thể tiếp cận được ít nhất một lỗ hay không. 

Giới hạn ngay lập tức loại trừ việc kiểm tra mọi đề xuất đối với mọi lỗ. Một giải pháp trực tiếp sẽ cần tới`10^10`so sánh tọa độ, vượt xa giới hạn hai giây thông thường cho phép. Chúng ta cần xử lý trước các lỗ hổng để mỗi truy vấn có thể được trả lời theo thời gian logarit. 

Các trường hợp cạnh chính đến từ hướng chuyển động. Không thể đạt được lỗ có tọa độ x lớn hơn hoặc tọa độ y lớn hơn ngay cả khi tọa độ khác phù hợp. Ví dụ:```
Input
2
5 5
10 1
3
6 6
9 2
4 10
```Truy vấn đầu tiên sẽ xuất ra`YES`bởi vì`(5,5)`ở bên dưới và bên trái của`(6,6)`. Truy vấn thứ hai sẽ xuất ra`NO`bởi vì`(5,5)`có tọa độ x nhỏ hơn nhưng lỗ khác`(10,1)`thì quá xa đúng không. Giải pháp bất cẩn chỉ kiểm tra một tọa độ sẽ chấp nhận sai. 

Một lỗi phổ biến khác là quên rằng điểm xuất phát có thể là điểm đến hợp lệ. Ví dụ:```
Input
1
7 4
1
7 4
```Câu trả lời là`YES`, vì cả hai độ dài cảnh quay đều có thể bằng 0. Các giải pháp yêu cầu tọa độ nhỏ hơn sẽ thất bại trong trường hợp này. 

Các lỗ trùng lặp cũng có thể xảy ra. Ví dụ:```
Input
3
2 3
2 3
5 1
1
2 3
```Câu trả lời là`YES`. Các bản sao không làm thay đổi logic nhưng việc triển khai loại bỏ các điểm không chính xác trong khi xử lý trước có thể vô tình làm mất thông tin hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mọi đề xuất`(a, b)`, quét tất cả các lỗ và kiểm tra xem có lỗ nào không`(x, y)`với`x <= a`Và`y <= b`. Điều này đúng vì nó kiểm tra chính xác điều kiện có thể tiếp cận. Tuy nhiên, với`n`lỗ và`q`các truy vấn, trường hợp xấu nhất sẽ thực hiện`n * q`so sánh. Với cả hai giá trị bằng`100000`, điều này trở thành`10^10`hoạt động quá chậm. 

Cấu trúc của điều kiện cho chúng ta cách tiếp cận nhanh hơn. Mọi truy vấn đều hỏi liệu một điểm có tồn tại trong hình chữ nhật phía dưới bên trái kết thúc tại`(a, b)`. Thay vì tìm kiếm hình chữ nhật này mọi lúc, chúng ta có thể xử lý trước các lỗ thành dạng mà câu trả lời chỉ là so sánh. 

Sắp xếp tất cả các lỗ theo tọa độ x của chúng. Trong khi di chuyển từ trái sang phải, hãy duy trì tọa độ y lớn nhất trong số tất cả các lỗ được thấy cho đến nay. Đối với bất kỳ ranh giới x nào có thể có, giá trị này cho chúng ta biết lỗ cao nhất vẫn có thể đạt tới theo chiều ngang. Một truy vấn`(a, b)`chỉ cần biết liệu giữa các lỗ có`x <= a`ít nhất có một cái có y tối đa`b`. 

Điều này chuyển đổi vấn đề thành một truy vấn tối đa tiền tố. Sau khi sắp xếp các lỗ, chúng tôi lưu trữ các mảng tọa độ x và giá trị y tối đa tương ứng. Mỗi truy vấn sử dụng tìm kiếm nhị phân để tìm lỗ cuối cùng với`x <= a`, sau đó kiểm tra y tối đa được lưu trữ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các lỗ theo tọa độ x của chúng. If two holes have the same x-coordinate, their relative order does not matter because they will all become part of the same prefix.
 2. Xây dựng một mảng tiền tố tối đa. For each position in the sorted holes, store the largest y-coordinate among all holes from the beginning up to that position. This value represents the highest reachable hole among all holes whose x-coordinate is not larger than the current x-coordinate.
 3. Đối với mọi đề xuất của Meeseeks`(a, b)`, hãy sử dụng tìm kiếm nhị phân trên tọa độ x đã sắp xếp để tìm chỉ mục cuối cùng chứa lỗ có`x <= a`. Tất cả các lỗ sau chỉ số đó đều không thể tiếp cận được vì tọa độ x của chúng quá lớn. 
4. Nếu không có chỉ số đó thì không có lỗ nào ở bên trái điểm bắt đầu, vì vậy câu trả lời là`NO`. Mặt khác, so sánh tọa độ y tối đa của tiền tố tại chỉ mục đó với`b`. Nếu ít nhất là`b`, tồn tại một số lỗ có thể tiếp cận được và câu trả lời là`YES`. 

Tại sao nó hoạt động: sau khi sắp xếp theo x, mọi tiền tố đều chứa chính xác các lỗ thỏa mãn yêu cầu theo chiều ngang. Y tối đa được lưu trữ trong tiền tố đó biểu thị vị trí thẳng đứng tốt nhất có thể có trong số các lỗ đó. Nếu ngay cả y cao nhất cũng nằm dưới tọa độ y của truy vấn thì mọi lỗ hổng trong tiền tố đều quá thấp. Nếu đạt cực đại đến tọa độ y của truy vấn thì lỗ đó thỏa mãn cả tọa độ và có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    holes = []

    for _ in range(n):
        x, y = map(int, input().split())
        holes.append((x, y))

    holes.sort()

    xs = []
    pref_y = []

    best = 0
    for x, y in holes:
        xs.append(x)
        best = max(best, y)
        pref_y.append(best)

    q = int(input())
    ans = []

    import bisect

    for _ in range(q):
        a, b = map(int, input().split())

        idx = bisect.bisect_right(xs, a) - 1

        if idx >= 0 and pref_y[idx] >= b:
            ans.append("YES")
        else:
            ans.append("NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Bước sắp xếp chuẩn bị các điểm để xử lý tiền tố. Sau khi sắp xếp, mỗi tiền tố kết thúc tại chỉ mục`i`chứa chính xác các lỗ có tọa độ x nhiều nhất`xs[i]`. 

các`pref_y`mảng lưu trữ tùy chọn dọc mạnh nhất có sẵn cho mỗi tiền tố. Chỉ giữ y tối đa là đủ vì truy vấn không quan tâm lỗ nào cung cấp nó, chỉ quan tâm liệu lỗ nào đó có đạt đủ cao hay không. 

Đối với mỗi truy vấn,`bisect_right`tìm thấy vị trí đầu tiên ở đó`a`có thể được chèn vào trong khi vẫn sắp xếp mảng x. Trừ đi một sẽ cho lỗ hợp lệ cuối cùng với`x <= a`. Sự so sánh với`pref_y[idx]`xử lý yêu cầu theo chiều dọc. 

Ranh giới tìm kiếm nhị phân là nơi dễ mắc lỗi nhất. sử dụng`bisect_right`thay vì`bisect_left`là cần thiết vì có thể tiếp cận được các lỗ chính xác trên tọa độ x ban đầu. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
Input
3
1 3
3 1
4 4
5
1 3
4 2
2 1
2 2
3 3
```Sau khi sắp xếp, các lỗ đã được sắp xếp sẵn. 

| Truy vấn | Cuối x <= a | Y tối đa trong tiền tố | Kết quả | 
| --- | --- | --- | --- | 
| (1,3) | 0 | 3 | CÓ | 
| (4,2) | 2 | 4 | CÓ | 
| (2,1) | 0 | 3 | CÓ | 
| (2,2) | 0 | 3 | CÓ | 
| (3,3) | 1 | 3 | CÓ | 

Tiền tố tối đa cho thấy rằng khi một lỗ có y đủ cao xuất hiện, tất cả các truy vấn sau này có x lớn hơn vẫn có thể sử dụng nó. 

Một truy vấn thực hiện trường hợp không thể:```
Input
2
10 1
20 2
3
5 5
15 2
25 3
```| Truy vấn | Cuối x <= a | Y tối đa trong tiền tố | Kết quả | 
| --- | --- | --- | --- | 
| (5,5) | không | không | KHÔNG | 
| (15,2) | 0 | 1 | KHÔNG | 
| (25,3) | 1 | 2 | KHÔNG | 

Dấu vết cho thấy tại sao cả hai tọa độ đều quan trọng. Mặc dù hai truy vấn đầu tiên có lỗ hổng với giá trị x nhỏ hơn nhưng giá trị y của chúng không đủ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Chi phí sắp xếp O(n log n) và mỗi truy vấn sử dụng một tìm kiếm nhị phân. | 
| Không gian | O(n) | Tọa độ được sắp xếp và thông tin tiền tố cực đại lưu trữ cho tất cả các lỗ. | 

Giải pháp chỉ thực hiện một lượng công việc theo logarit cho mỗi truy vấn, vì vậy`100000`lỗ và`100000`truy vấn dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import bisect

def solve(inp: str) -> str:
    data = io.StringIO(inp)
    input = data.readline

    n = int(input())
    holes = [tuple(map(int, input().split())) for _ in range(n)]

    holes.sort()

    xs = []
    pref = []
    best = 0

    for x, y in holes:
        xs.append(x)
        best = max(best, y)
        pref.append(best)

    q = int(input())
    res = []

    for _ in range(q):
        a, b = map(int, input().split())
        idx = bisect.bisect_right(xs, a) - 1
        res.append("YES" if idx >= 0 and pref[idx] >= b else "NO")

    return "\n".join(res)

assert solve("""3
1 3
3 1
4 4
5
1 3
4 2
2 1
2 2
3 3
""") == """YES
YES
YES
YES
YES"""

assert solve("""3
10 1
20 2
30 3
4
5 5
15 2
25 3
35 3
""") == """NO
NO
NO
YES"""

assert solve("""1
7 4
3
7 4
8 4
6 5
""") == """YES
YES
NO"""

assert solve("""4
2 3
2 3
5 1
10 10
4
2 3
5 2
9 9
11 11
""") == """YES
YES
NO
NO"""

assert solve("""5
1 1
2 2
3 3
4 4
5 5
3
0 0
5 5
3 4
""") == """NO
YES
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp kiểu mẫu đầu tiên | Tất cả CÓ | Truy vấn có thể truy cập bình thường | 
| Tăng tọa độ | Câu trả lời hỗn hợp | Kiểm tra các điều kiện x và y cùng nhau | 
| Lỗ đơn | CÓ, CÓ, KHÔNG | Xử lý kích thước tối thiểu và ảnh có độ dài bằng 0 | 
| Lỗ trùng lặp | Xử lý trùng lặp đúng | Ngăn ngừa lỗi tiền xử lý | 
| Điểm chéo | Thất bại về ranh giới | Nắm bắt cách xử lý bất bình đẳng không chính xác | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
2
5 5
10 1
1
9 2
```Thuật toán sắp xếp các lỗ và chỉ tìm ra rằng`(5,5)`thuộc về tiền tố với`x <= 9`. Y tối đa trong tiền tố đó là`5`, ít nhất là`2`, vậy câu trả lời là`YES`. 

Đối với cùng một dữ liệu với truy vấn`(9,6)`, tiền tố tối đa vẫn là`5`, nhưng bây giờ nó ở dưới tọa độ y yêu cầu. Thuật toán trả về`NO`, loại bỏ chính xác một điểm có thể tiếp cận theo chiều ngang nhưng lại quá cao theo chiều dọc. 

Đối với trường hợp chuyển động có khoảng cách bằng không:```
1
7 4
1
7 4
```Tìm kiếm nhị phân tìm ra lỗ hổng vì`x <= a`là đúng khi các giá trị bằng nhau. Tiền tố tối đa y cũng bằng`b`, vậy câu trả lời là`YES`. 

Đối với các lỗ trùng lặp:```
3
2 3
2 3
5 1
1
2 3
```Cả hai bản sao của`(2,3)`xuất hiện ở đầu sau khi sắp xếp. Tiền tố tối đa trở thành`3`và truy vấn thành công. Thuật toán không phụ thuộc vào tính duy nhất nên các bản sao không cần xử lý đặc biệt.
