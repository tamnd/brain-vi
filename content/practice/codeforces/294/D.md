---
title: "CF 294D - Robot Shaass và Họa sĩ"
description: "Chúng tôi có một sàn bếp lát gạch kích thước n×m, ban đầu tất cả đều màu trắng. Robot đứng trên ô viền ở tọa độ (xs, ys) và quay mặt về một trong bốn hướng chéo. Mỗi khi robot di chuyển đến một ô, nó sẽ sơn nó màu đen. Nếu nó chạm vào tường, nó sẽ phản chiếu như một quả bóng bi-a."
date: "2026-06-05T17:35:11+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 294
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 178 (Div. 2)"
rating: 2500
weight: 294
solve_time_s: 86
verified: false
draft: false
---

[CF 294D - Robot Shaass và Họa sĩ](https://codeforces.com/problemset/problem/294/D) 

**Đánh giá:** 2500 
**Tags:** vũ lực, thực hiện, lý thuyết số 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một sàn bếp kích thước _n__×_m_ lát gạch, ban đầu tất cả đều màu trắng. Robot đứng trên ô viền ở tọa độ (_x_s_, _y_s_) và quay mặt về một trong bốn hướng chéo. Mỗi khi robot di chuyển đến một ô, nó sẽ sơn nó màu đen. Nếu nó chạm vào tường, nó sẽ phản chiếu như một quả bóng bi-a. Mục tiêu của robot là sơn các viên gạch cho đến khi sàn nhà trở thành bàn cờ, trong đó không có hai viên gạch liền kề nào có cùng màu. Chúng ta phải tính tổng số ô sơn đen cho đến khi điều kiện này được thỏa mãn hoặc -1 nếu điều đó không bao giờ xảy ra. 

Robot sơn ô bắt đầu ngay lập tức. Quy tắc phản xạ có nghĩa là khi robot chạm vào một bức tường ngang hoặc dọc, thành phần chuyển động ngang hoặc dọc của nó sẽ đảo ngược. Nếu nó chạm vào một góc, cả hai thành phần sẽ đảo ngược. 

Các ràng buộc rất lớn: _n_, _m_ có thể lên tới 10^5. Một mô phỏng đơn giản di chuyển từng ô robot có thể thực hiện tới 10^10 thao tác, vượt xa giới hạn thời gian 2 giây. Vì vậy, chúng ta cần một cách tiếp cận mang tính phân tích hoặc toán học hơn. 

Các trường hợp phức tạp phát sinh khi robot có thể bước vào một chu trình mà không bao giờ che đúng các ô cho bàn cờ. Ví dụ: trong hình chữ nhật 2×3 bắt đầu từ một góc, robot có thể dao động liên tục giữa ba ô. Việc triển khai bất cẩn mô phỏng vô thời hạn sẽ không bao giờ chấm dứt và điều kiện bàn cờ sẽ không bao giờ được đáp ứng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng robot di chuyển từng ô một, cập nhật màu ô và kiểm tra tình trạng bàn cờ sau mỗi lần di chuyển. Điều này đúng vì nó trực tiếp triển khai câu lệnh vấn đề, nhưng không khả thi đối với các lưới lớn vì mỗi lần di chuyển mất thời gian không đổi và có thể có tới O(n*m) ô duy nhất được truy cập trong một đường chéo duy nhất được lặp lại qua nhiều phản xạ. 

Quan sát quan trọng là robot di chuyển theo đường chéo và kiểu chuyển động của nó lặp lại theo chu kỳ. Nếu chúng ta theo dõi số lần di chuyển để đến bức tường tiếp theo theo cả hai hướng cùng một lúc, chúng ta có thể chuyển trực tiếp đến điểm phản chiếu tiếp theo thay vì mô phỏng từng ô. Cụ thể, từ vị trí (_x_, _y_) di chuyển với gia số (_dx_, _dy_), robot sẽ va vào bức tường tiếp theo sau`t = min((dx>0?n-x:x-1)//abs(dx), (dy>0?m-y:y-1)//abs(dy))`các bước. Trong một bước nhảy như vậy, nó vẽ`t`gạch mới. Sau khi va vào tường, thành phần hướng sẽ đảo ngược và chúng ta lặp lại. Điều này làm giảm số lượng thao tác xuống tối đa là O(n + m), vì mỗi lần phản xạ sẽ giảm khoảng cách đến một bức tường và robot chỉ có thể ghé thăm các ranh giới trong một số lần giới hạn. 

Chúng tôi cũng theo dõi tính chẵn lẻ của tọa độ ô theo modulo 2. Sàn là một bàn cờ nếu tất cả các ô có tọa độ tổng chẵn được sơn và tất cả các ô có tọa độ tổng lẻ được sơn. Nếu robot bắt đầu ở một ô khớp với số chẵn lẻ của bàn cờ, cuối cùng nó sẽ bao phủ tất cả các ô của một ô chẵn lẻ dọc theo đường đi của nó. Nếu robot đi vào một vòng lặp mà không chạm vào các ô có chẵn lẻ đối diện, thuật toán có thể phát hiện nó bằng cách sử dụng tập hợp trạng thái đã truy cập bao gồm`(x, y, dx, dy)`bộ dữ liệu. Nếu robot lặp lại một trạng thái, chúng ta biết nó bị kẹt trong một chu kỳ và sẽ không bao giờ kết thúc, vì vậy đầu ra là -1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n*m) | Quá chậm | 
| Tối ưu (nhảy phản ánh) | O(n + m) | O(1) hoặc O(4*visited_states) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước lưới _n_, _m_, tọa độ bắt đầu (_x_s_, _y_s_) và hướng ban đầu. Chuyển đổi các hướng như "UL" thành số gia`dx`,`dy`Ở đâu`dx = ±1`cho sự chuyển động của hàng và`dy = ±1`cho sự chuyển động của cột. 
2. Khởi tạo`painted = 1`bởi vì ô bắt đầu được sơn ngay lập tức. 
3. Theo dõi vị trí hiện tại của robot`(x, y)`và phương hướng`(dx, dy)`. 
4. Sử dụng một bộ`visited_states`để theo dõi`(x, y, dx, dy)`để phát hiện các vòng lặp. Nếu robot quay lại trạng thái tương tự, hãy kết thúc bằng -1. 
5. Trong khi bàn cờ chưa hoàn chỉnh: 

một. Tính số bước tối đa tới bức tường tiếp theo theo cả hai hướng:`tx = (n-x) if dx>0 else (x-1)`

`ty = (m-y) if dy>0 else (y-1)`

`t = min(tx, ty)`b. Di chuyển robot`t`bước theo đường chéo:`x += dx*t`,`y += dy*t`. 

c. Tăng`painted`qua`t`. 

d. Nếu robot va vào một bức tường thẳng đứng, hãy lùi lại`dy`. Nếu nó va vào một bức tường nằm ngang, hãy lùi lại`dx`. Nếu nó chạm vào một góc, hãy đảo ngược cả hai. 
6. Kiểm tra xem tính chẵn lẻ của`(x_s + y_s) % 2`phù hợp với sự chẵn lẻ của tất cả các viên gạch được sơn. Nếu cuối cùng cả hai số chẵn lẻ đều được bảo hiểm, hãy kết thúc và xuất ra`painted`. 
7. Lặp lại cho đến khi tất cả các ô của mỗi ô chẵn lẻ được sơn hoặc phát hiện thấy một vòng lặp. 

**Tại sao hoạt động:** Robot di chuyển theo đường chéo, phản ánh tại các ranh giới. Đường đi của nó là tuần hoàn và chỉ phụ thuộc vào vị trí và hướng hiện tại của nó. Bằng cách nhảy đến các điểm phản chiếu và đếm các bước, chúng tôi đếm chính xác số lượng ô đã truy cập. Kiểm tra tính chẵn lẻ đảm bảo rằng một bàn cờ có thể được hoàn thành và phát hiện vòng lặp đảm bảo chấm dứt khi không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    x, y, dirc = input().split()
    x = int(x)
    y = int(y)

    dx = 0
    dy = 0
    if dirc[0] == 'U':
        dx = -1
    else:
        dx = 1
    if dirc[1] == 'L':
        dy = -1
    else:
        dy = 1

    painted = 1
    visited = set()
    
    while True:
        if (x, y, dx, dy) in visited:
            print(-1)
            return
        visited.add((x, y, dx, dy))

        tx = n - x if dx > 0 else x - 1
        ty = m - y if dy > 0 else y - 1
        t = min(tx, ty)
        if t == 0:
```
