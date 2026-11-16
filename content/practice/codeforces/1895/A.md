---
title: "CF 1895A - Rương Báu"
description: "Trục số có ba vị trí quan trọng. Monocarp bắt đầu ở vị trí 0, rương ở vị trí x và chìa khóa ở vị trí y. Để mở rương, Monocarp cuối cùng phải đứng ở cùng vị trí với rương trong khi đã mang theo chìa khóa."
date: "2026-06-08T21:44:11+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1895
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 157 (Rated for Div. 2)"
rating: 800
weight: 1895
solve_time_s: 110
verified: true
draft: false
---

[CF 1895A - Rương kho báu](https://codeforces.com/problemset/problem/1895/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trục số có ba vị trí quan trọng. Monocarp bắt đầu ở vị trí`0`, ngực ở vị trí`x`, và chìa khóa ở vị trí`y`. 

Để mở rương, Monocarp cuối cùng phải đứng ở cùng vị trí với rương trong khi đã mang theo chìa khóa. Đi bộ tốn một giây cho mỗi đơn vị khoảng cách. Nhấc hoặc đặt rương và chìa khóa xuống không mất phí gì. 

Điều phức tạp là chiếc rương có thể được mang tối đa`k`tổng số giây. Nếu chìa khóa chưa ở trên đường tới rương, Monocarp có thể cần di chuyển rương đến gần chìa khóa hơn, nhưng khoảng cách mang không được vượt quá`k`. 

Dữ liệu đầu vào chứa tối đa 100 trường hợp thử nghiệm và tất cả các tọa độ tối đa là 100. Các ràng buộc rất nhỏ nhưng mục tiêu thực sự là tìm ra quan sát toán học đưa ra câu trả lời ngay lập tức. 

Một sai lầm phổ biến là cho rằng Monocarp luôn phải chạm tới chìa khóa trước khi chạm vào rương. Điều đó không đúng vì anh ta được phép mang rương. 

Coi như:```
x = 5, y = 7, k = 2
```Anh ta tới rương ở vị trí 5, mang theo 2 đơn vị đến vị trí 7, lấy chìa khóa và mở rương. Câu trả lời là 7 chứ không phải 12. 

Một sai lầm dễ mắc phải khác là luôn trả lời`max(x, y)`khi chiếc chìa khóa nằm ngoài rương. 

Coi như:```
x = 5, y = 8, k = 2
```Chìa khóa là 3 đơn vị ngoài rương, nhưng Monocarp chỉ có thể mang rương trong 2 đơn vị. Anh ta mang nó đến vị trí số 7, đi tới số 8 để lấy chìa khóa, sau đó quay lại số 7. Đáp án là 9 chứ không phải 8. 

Trường hợp cạnh thứ ba xuất hiện khi chìa khóa ở trước rương.```
x = 10, y = 5, k = 0
```Monocarp chỉ cần nhặt chìa khóa trên đường tới rương. Câu trả lời là 10. Bất kỳ logic nào cố gắng liên quan đến việc mang rương đến đây đều không cần thiết. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô hình hóa mọi vị trí có thể có của Monocarp, liệu anh ta có chìa khóa hay không, chiếc rương hiện đang ở đâu và sức chịu đựng còn lại bao nhiêu. Sau đó, chúng ta có thể thực hiện tìm kiếm đường đi ngắn nhất trên không gian trạng thái kết quả. 

Điều này hiệu quả vì tất cả các hành động đều có chi phí không âm và không gian trạng thái là hữu hạn. Với tọa độ được giới hạn bởi 100, giải pháp như vậy thậm chí có thể được thực hiện. Vấn đề là nó hoàn toàn bỏ qua cấu trúc của trục số và hình học rất đơn giản có liên quan. 

Quan sát quan trọng là chỉ có hai thứ tự tương đối quan trọng. 

Nếu chìa khóa ở hoặc trước rương (`y < x`), Monocarp với tới chiếc chìa khóa trong khi đi về phía chiếc rương. Anh ta nhặt nó lên miễn phí và tiếp tục`x`. Ngực không bao giờ cần phải di chuyển. Câu trả lời đơn giản là`x`. 

Trường hợp thú vị là khi chiếc chìa khóa nằm ngoài rương (`y > x`). 

Monocarp đầu tiên phải chạm tới rương, chi tiêu`x`giây. 

Khoảng cách còn lại giữa rương và chìa khóa là:```
d = y - x
```Nếu như`d <= k`, anh ta có thể mang rương đến tận chìa khóa. Tổng thời gian trở thành`x + d = y`. 

Nếu như`d > k`, chính xác là anh ấy mang rương đi xa nhất có thể`k`đơn vị. Ngực dừng lại ở vị trí`x + k`. 

Vẫn còn một khoảng cách còn lại:```
r = d - k
```Đi bộ bằng monocarp`r`đơn vị vào chìa khóa và sau đó`r`đơn vị trở lại ngực. Chi phí đó`2r`thêm giây. 

Tổng số trở thành:```
x + k + 2(d - k)
= x + 2d - k
```Từ`y = x + d`, điều này cũng có thể được viết là:```
y + (d - k)
```Một công thức nhỏ gọn là:```
x,                     if y < x
y + max(0, y - x - k), otherwise
```| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(S) | O(S) | Không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`x`,`y`, Và`k`. 
2. Nếu`y < x`, chìa khóa sẽ gặp trước khi chạm tới rương. Nhặt chìa khóa trên đường đi và tiếp tục đến rương. đầu ra`x`. 
3. Ngược lại hãy tính khoảng cách`d = y - x`. 
4. Nếu`d <= k`, rương có thể xách tới tận chìa khóa. đầu ra`y`. 
5. Nếu không thì chỉ có thể di chuyển rương`k`đơn vị. Quãng đường còn lại là`d - k`. 
6. Monocarp phải đi bộ quãng đường còn lại để lấy chìa khóa và sau đó đi bộ quãng đường tương tự để quay lại rương. đầu ra`y + (d - k)`. 

### Tại sao nó hoạt động 

Khi nào`y < x`, mọi giải pháp hợp lệ cuối cùng phải đạt đến vị trí`x`, và chìa khóa nằm trên con đường đó. Đạt đến ngực tại`x`đã đủ rồi, vậy`x`là tối ưu. 

Khi`y > x`, Monocarp trước tiên phải chi`x`giây chạm tới ngực. Mang rương về phía chìa khóa luôn có lợi vì nó làm giảm khoảng cách giữa họ trong tương lai. Chiến lược tốt nhất là sử dụng khoảng cách vận chuyển càng nhiều càng tốt, tối đa`k`hoặc cho đến khi rương chạm tới chìa khóa. Mọi ngăn cách còn lại phải được vượt qua hai lần, một lần để lấy chìa khóa và một lần để quay lại rương. Công thức trên tính toán chính xác những chuyển động không thể tránh khỏi này, do đó không tồn tại tuyến đường nào ngắn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    x, y, k = map(int, input().split())

    if y < x:
        print(x)
    else:
        print(y + max(0, y - x - k))
```Nhánh đầu tiên xử lý trường hợp chìa khóa gặp trước rương. Không cần phải mang vác nên câu trả lời chỉ đơn giản là tư thế đặt ngực. 

Ở nhánh thứ hai, chìa khóa nằm phía sau rương. giá trị`y - x - k`đại diện cho phần khoảng trống từ ngực đến chìa khóa không thể che được bằng cách mang rương. Nếu số lượng này âm, rương sẽ chạm trực tiếp vào chìa khóa nên chi phí tăng thêm bằng 0. Nếu không thì khoảng cách còn lại đó đóng góp chính xác một lần vào câu trả lời ngoài`y`, tạo ra công thức`y + max(0, y - x - k)`. 

Tất cả các phép tính đều phù hợp thoải mái bên trong các loại số nguyên tiêu chuẩn và không có điều kiện biên phức tạp vì công thức xử lý một cách tự nhiên`k = 0`Và`d = k`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
x = 5, y = 7, k = 2
```| Biến | Giá trị | 
| --- | --- | 
| x | 5 | 
| y | 7 | 
| k | 2 | 
| d = y - x | 2 | 
| d <= k | Có | 
| Trả lời | 7 | 

Monocarp tới rương trong 5 giây và mang nó đúng 2 đơn vị tới chìa khóa. Rương và chìa khóa gặp nhau ở vị trí số 7 nên tổng thời gian là 7. 

### Ví dụ 2 

đầu vào:```
x = 5, y = 8, k = 2
```| Biến | Giá trị | 
| --- | --- | 
| x | 5 | 
| y | 8 | 
| k | 2 | 
| d = y - x | 3 | 
| d <= k | Không | 
| Khoảng cách còn lại | 1 | 
| Trả lời | 8 + 1 = 9 | 

Monocarp mang rương từ 5 đến 7. Sau đó anh ta đi từ 7 đến 8 để lấy chìa khóa và quay lại từ 8 đến 7 để mở rương. Tổng thời gian là 9. 

Những dấu vết này minh họa tính bất biến của khóa: sau khi sử dụng tất cả khoảng cách mang có sẵn, mọi khoảng cách giữa chìa khóa ngực còn lại phải được đi qua hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung | 

Ngay cả với số lượng ca kiểm thử tối đa, chương trình chỉ thực hiện một số thao tác cho mỗi ca. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        x, y, k = map(int, input().split())

        if y < x:
            ans.append(str(x))
        else:
            ans.append(str(y + max(0, y - x - k)))

    return "\n".join(ans)

# provided sample
assert run(
"""3
5 7 2
10 5 0
5 8 2
"""
) == "7\n10\n9"

# key before chest
assert run(
"""1
8 3 5
"""
) == "8"

# exactly enough carrying distance
assert run(
"""1
4 9 5
"""
) == "9"

# no carrying allowed
assert run(
"""1
4 9 0
"""
) == "14"

# maximum coordinate values
assert run(
"""1
100 99 100
"""
) == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`8 3 5`|`8`| Chìa khóa nằm trước ngực | 
|`4 9 5`|`9`| Khoảng cách mang chính xác khớp với khoảng cách | 
|`4 9 0`|`14`| Không có sẵn mang theo | 
|`100 99 100`|`100`| Tọa độ lớn và chìa khóa trước rương | 

## Vỏ cạnh 

Hãy xem xét:```
1
10 5 0
```Thuật toán thấy rằng`y < x`và ngay lập tức quay trở lại`10`. Monocarp nhặt chìa khóa trong khi đi về phía rương. Không cần mang theo. Đầu ra là chính xác. 

Coi như:```
1
5 7 2
```Đây`y > x`, và khoảng cách là`2`. Từ`2 <= k`, thuật toán trả về`y = 7`. Chiếc rương có thể được mang trực tiếp đến chìa khóa. Không có sự quay lại bổ sung nào xảy ra. 

Coi như:```
1
5 8 2
```Khoảng cách là`3`, lớn hơn`k`. Phần không được che chắn là`3 - 2 = 1`. Thuật toán trả về`8 + 1 = 9`. Đơn vị bổ sung đó tương ứng chính xác với việc đi bộ từ rương được di dời đến chìa khóa và quay lại. 

Coi như:```
1
4 9 0
```Khoảng cách là`5`, và không thể mang theo được. Công thức cho`9 + 5 = 14`. Monocarp đi đến rương trong 4 giây, sau đó phải di chuyển từ 4 đến 9 rồi quay lại 4, cộng thêm 10 giây nữa. Tổng số là 14, khớp chính xác với công thức.
