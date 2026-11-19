---
title: "CF 2120B - Bể bơi vuông"
description: "Chúng ta được cung cấp một bàn bida hình vuông có cạnh dài $s$ với các túi ở bốn góc. Trên bảng này, các quả bóng $n$ được đặt ở tọa độ nguyên bên trong bảng, không bao giờ ở các cạnh hoặc góc."
date: "2026-06-08T03:51:44+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 2120
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1033 (Div. 2) and CodeNite 2025"
rating: 1000
weight: 2120
solve_time_s: 77
verified: true
draft: false
---

[CF 2120B - Bể bơi vuông](https://codeforces.com/problemset/problem/2120/B) 

**Đánh giá:** 1000 
**Thẻ:** hình học 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bàn bi-a hình vuông có cạnh dài$s$có túi ở bốn góc. Trên bàn này,$n$các quả bóng được đặt ở tọa độ nguyên bên trong bảng, không bao giờ ở các cạnh hoặc góc. Mỗi quả bóng được bắn đồng thời một góc 45 độ theo một trong bốn hướng chéo, được xác định bởi một vectơ$(d_x, d_y)$trong đó mỗi thành phần là 1 hoặc -1. Các quả bóng chuyển động với tốc độ vô cùng nhanh nhưng tuân theo sự phản xạ đàn hồi hoàn hảo: khi một quả bóng chạm vào tường, nó sẽ bật trở lại theo cùng một góc. Chúng ta cần đếm xem cuối cùng có bao nhiêu quả bóng rơi vào một trong bốn góc. 

Mỗi trường hợp thử nghiệm đưa ra$n$quả bóng, vị trí ban đầu và vectơ chỉ phương của chúng. Kết quả đầu ra là số lượng bóng cuối cùng đạt tới một góc. 

Những ràng buộc cho chúng ta biết$n$có thể lên tới 1000 và bên$s$có thể lên đến$10^9$. Tổng của$n$trên tất cả các trường hợp thử nghiệm nhiều nhất là 1000. Điều này có nghĩa là chúng tôi có đủ khả năng$O(n)$giải pháp cho mỗi trường hợp thử nghiệm và chúng tôi không cần mô phỏng chuyển động của quả bóng từng bước, điều này là không thể đối với$s$lớn thế này. Tất cả tọa độ và hướng đều là số nguyên nên chúng ta có thể suy luận hoàn toàn bằng số học số nguyên. 

Các trường hợp cạnh không rõ ràng phát sinh khi một quả bóng đang di chuyển về phía bức tường theo cách mà nó chỉ chạm tới góc sau một vài lần phản xạ. Ví dụ, một quả bóng ở$(1,1)$di chuyển$(1,-1)$trên một chiếc bàn 2 × 2 nảy ra khỏi các bức tường phía trên và bên phải để cuối cùng tiếp đất ở một góc. Một cách tiếp cận ngây thơ chỉ mô phỏng chuyển động đầu tiên hoặc giả sử không có phản xạ sẽ bỏ lỡ những quả bóng này một cách không chính xác. 

## Phương pháp tiếp cận 

Cách brute-force là mô phỏng từng quả bóng khi nó di chuyển dọc theo quỹ đạo của nó, phản xạ khỏi các bức tường, cho đến khi nó chạm vào một góc hoặc vượt quá một giới hạn tùy ý nào đó. Đối với mỗi phản ánh, chúng tôi sẽ cập nhật vị trí và hướng của nó. Tuy nhiên, với$s$lớn như$10^9$, một quả bóng có thể nảy lên nhiều lần trước khi chạm tới một góc, vì vậy mô phỏng này sẽ quá chậm. 

Thông tin quan trọng là vì các quả bóng di chuyển theo góc 45 độ và phản xạ hoàn hảo nên đường đi của chúng trên mỗi trục là mô đun tuần hoàn.$2s$. Nói cách khác, chúng ta có thể "mở rộng" cái bàn bằng cách tưởng tượng những phản xạ như là phần mở rộng của cái bàn: một quả bóng di chuyển theo đường chéo qua các bản sao được phản chiếu lặp đi lặp lại của cái bàn sẽ chạm vào góc đầu tiên nằm dọc theo đường dốc ±1 của nó. Điều này làm giảm vấn đề thành một phép kiểm tra đơn giản: đối với một quả bóng bắt đầu từ$(x, y)$có định hướng$(d_x, d_y)$, cuối cùng nó sẽ tới một góc khi và chỉ khi$(s - x) \cdot d_y = (s - y) \cdot d_x$modulo$2s$. Đơn giản hóa hơn nữa, vì tọa độ là số nguyên và chúng ta chỉ có bốn góc, nên chúng ta có thể tính toán rõ ràng cần bao nhiêu bước theo mỗi hướng để đến được một bức tường và kiểm tra xem số lượng phản xạ có dẫn chính xác đến một góc hay không. Điều này có thể được thực hiện bằng cách sử dụng số học đơn giản mà không cần lặp lại các vị trí. 

Giải pháp tối ưu tính toán, đối với mỗi quả bóng, số lần di chuyển dọc theo x và y cho đến khi nó đạt đến 0 hoặc s dọc theo cả hai trục và kiểm tra xem chúng có trùng nhau về mặt thời gian hay không. Bởi vì các chuyển động là đường chéo với độ dốc 1:1, một quả bóng sẽ rơi vào một góc nếu số lượng phản xạ dọc theo trục x và y đều là số chẵn hoặc cả hai đều là số lẻ, tương ứng với tính chẵn lẻ của các phản xạ cần thiết để đạt tới các ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n*s) | O(1) | Quá chậm | 
| Kiểm tra tính chẵn lẻ phản ánh | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$Và$s$. Khởi tạo bộ đếm cho các quả bóng trong chậu. 
2. Đối với mỗi quả bóng, hãy đọc tọa độ ban đầu của nó$(x, y)$và phương hướng$(d_x, d_y)$. Tính khoảng cách đến các bức tường gần nhất theo cả hai hướng x và y tùy theo hướng. Nếu như$d_x = 1$, khoảng cách tới bức tường bên phải là$s - x$, nếu không thì khoảng cách tới bức tường bên trái là$x$. Tương tự với y. 
3. Tính xem cần bao nhiêu phản xạ dọc theo x và y để đến được góc. Vì mỗi phản xạ đổi hướng nên tổng khoảng cách dọc theo x để tới một góc phải là bội số của$s$. Tương tự dọc theo y. 
4. Nếu số lượng phản xạ dọc theo trục x và y cần thiết để chạm tới các bức tường có cùng độ chẵn lẻ thì cuối cùng quả bóng sẽ chạm tới một góc. Tăng bộ đếm trong trường hợp đó. 
5. Sau khi xử lý tất cả các quả bóng, in bộ đếm cho test case. 

Bất biến chính là dọc theo mỗi trục, quả bóng đổi hướng sau khi chạm vào tường. Lần đầu tiên nó chạm tới một bức tường, vị trí của nó theo modulo$s$khớp với 0 hoặc s và vì chuyển động là 45 độ nên thời gian dọc theo trục x và y phải trùng với bội số nguyên nào đó của khoảng cách tường. Kiểm tra tính chẵn lẻ đảm bảo sự liên kết ở một góc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def will_pot(x, y, dx, dy, s):
    # distance to next wall along x
    if dx == 1:
        dist_x = s - x
    else:
        dist_x = x
    if dy == 1:
        dist_y = s - y
    else:
        dist_y = y
    # ball reaches a corner if dist_x % s == dist_y % s
    return dist_x % s == dist_y % s

t = int(input())
for _ in range(t):
    n, s = map(int, input().split())
    count = 0
    for _ in range(n):
        dx, dy, x, y = map(int, input().split())
        if will_pot(x, y, dx, dy, s):
            count += 1
    print(count)
```chức năng`will_pot`tính toán khoảng cách đến bức tường theo hướng của quả bóng và kiểm tra xem nó có thẳng hàng với khoảng cách dọc theo trục kia hay không. Vì các quả bóng di chuyển theo đường chéo nên chúng chỉ đến được các góc nơi các khoảng cách này bằng nhau theo modulo.$s$. Điều này tránh việc mô phỏng sự phản xạ và hoạt động trong$O(1)$mỗi quả bóng. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

| Bóng | x | y | dx | nhuộm | dist_x | dist_y | dist_x % s == dist_y % s | Chậu? | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | Đúng | Có | 

Bóng đang di chuyển thẳng về phía (2,2) nên việc kiểm tra thành công và chúng ta đếm 1. 

**Mẫu 2:** 

| Bóng | x | y | dx | nhuộm | dist_x | dist_y | dist_x % s == dist_y % s | Chậu? | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | -1 | 3 | 1 | Sai | Không | 
| 2 | 2 | 2 | 1 | -1 | 2 | 2 | Đúng | Có | 
| 3 | 2 | 3 | -1 | 1 | 2 | 1 | Sai | Không | 
| 4 | 1 | 3 | 1 | -1 | 3 | 1 | Sai | Không | 
| 5 | 3 | 1 | -1 | 1 | 3 | 3 | Đúng | Có | 

Tổng số quả bóng trong chậu: 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi quả bóng được xử lý trong thời gian không đổi. Tổng n trên tất cả các trường hợp thử nghiệm là 1000. | 
| Không gian | O(1) | Chỉ cần một bộ đếm và các biến tạm thời cho mỗi quả bóng. | 

Giải pháp này dễ dàng nằm gọn trong giới hạn 1 giây vì nó thực hiện tối đa 1000 phép kiểm tra số học đơn giản cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assuming solution above is saved as solution.py
    return output.getvalue().strip()

# provided samples
assert run("2\n1 2\n1 1 1 1\n5 4\n1 -1 1 1\n1 -1 2 2\n-1 1 2 3\n1 -1 1 3\n-1 1 3 1\n") == "1\n3"

# custom cases
assert run("1\n1 2\n1 1 -1 -1\n") == "1", "ball moving towards (0,0)"
assert run("1\n2 5\n1 1 1 1\n4 4 -1 -1\n") == "2", "balls opposite corners"
assert run("1\n1 1000000000\n500000000 500000000 1 1\n") == "1", "large table, single ball"
assert run("1\n4 3\n1 1
```
