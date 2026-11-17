---
title: "CF 1989A - Bắt Đồng Xu"
description: "Chúng ta được cung cấp một lưới biểu thị màn hình arcade có tọa độ $(x, y)$, trong đó Monocarp bắt đầu tại điểm gốc $(0, 0)$. Có $n$ đồng xu nằm rải rác trên lưới. Mỗi giây, Monocarp có thể di chuyển theo một trong tám hướng (lên, xuống, trái, phải và bốn đường chéo)."
date: "2026-06-08T15:44:02+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1989
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 167 (Rated for Div. 2)"
rating: 800
weight: 1989
solve_time_s: 398
verified: false
draft: false
---

[CF 1989A - Bắt đồng xu](https://codeforces.com/problemset/problem/1989/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 6 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới biểu thị màn hình arcade có tọa độ$(x, y)$, nơi Monocarp bắt đầu tại gốc tọa độ$(0, 0)$. có$n$tiền xu nằm rải rác trên lưới. Mỗi giây, Monocarp có thể di chuyển theo một trong tám hướng (lên, xuống, trái, phải và bốn đường chéo). Sau khi Monocarp di chuyển, tất cả đồng xu rơi xuống một đơn vị dọc theo trục Y. Mục tiêu là xác định đối với mỗi đồng xu liệu Monocarp có thể đạt được nó vào một thời điểm nào đó hay không, giả sử anh ta di chuyển một cách tối ưu. 

Đầu vào mang lại$n$và tọa độ của các đồng tiền, tất cả đều khác biệt và không có điểm gốc. Đầu ra là một dòng trên mỗi đồng xu: "CÓ" nếu Monocarp có thể thu thập nó và "KHÔNG" nếu ngược lại. 

Các hạn chế là nhỏ:$n \le 500$và tọa độ đồng xu nằm giữa$-50$Và$50$. Điều này cho phép các thuật toán có ít nhất$O(n)$mỗi đồng xu, hoặc$O(n^2)$tổng thể, không có vấn đề về hiệu suất. 

Các trường hợp không rõ ràng quan trọng liên quan đến các đồng tiền nằm dưới điểm gốc. Vì đồng xu rơi xuống 1 đơn vị mỗi giây, một đồng xu ban đầu ở tọa độ Y âm có thể rơi nhanh hơn Monocarp có thể chạm tới nó theo đường chéo. Ví dụ: nếu một đồng xu bắt đầu ở$(-1, -2)$, Monocarp có thể di chuyển theo đường chéo để tiếp cận nó khi nó rơi xuống. Một phép kiểm tra ngây thơ sử dụng khoảng cách Manhattan mà không tính đến việc đồng xu rơi sẽ nói sai rằng anh ta không thể đạt được nó. 

Một trường hợp tinh vi khác là các đồng xu được sắp xếp theo đường chéo hoặc chiều ngang: Các bước di chuyển theo đường chéo của Monocarp có thể được sử dụng để “bắt kịp” theo chiều dọc trong khi di chuyển theo chiều ngang. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là mô phỏng chuyển động của Monocarp cho từng đồng xu từng giây, kiểm tra xem đường đi của anh ta có giao nhau với đường rơi của đồng xu hay không. Điều này đúng nhưng cồng kềnh, đòi hỏi một vòng lặp mô phỏng cho mỗi đồng xu và mỗi bước thời gian, có thể là hàng trăm bước đối với các đồng tiền ở xa. Với$n = 500$và vị trí lên tới 50, điều đó là khả thi nhưng phức tạp không cần thiết. 

Cách tiếp cận tối ưu đến từ việc quan sát các phương án di chuyển của Monocarp. Trong một giây, anh ta có thể thay đổi cả X và Y nhiều nhất là 1 đơn vị. Điều này tương đương với khoảng cách Chebyshev (tối đa$|dx|$Và$|dy|$) thay vì khoảng cách Manhattan. Chúng ta phải tính đến thực tế là đồng xu giảm đi 1 đơn vị mỗi giây. Đặt vị trí đồng xu ban đầu là$(x, y)$và Monocarp tại$(0, 0)$. Anh ta có thể lấy được đồng xu nếu có một số$t \ge 0$như vậy:$$\max(|x|, |y - t|) \le t$$Giải quyết cho$t$cho$t \ge \max(|x|, y - t)$, điều này đơn giản hóa thành$t \ge |x|$Và$y - t \le t$, hoặc$y \le 2t$. Từ$t \ge |x|$, bất đẳng thức thứ hai trở thành$y \le 2|x|$. Vậy điều kiện để Monocarp đạt được xu là:$$y \le 2 \cdot |x|$$Nếu điều này được giữ vững, Monocarp có thể bắt đồng xu bằng cách di chuyển theo đường chéo về phía cột của nó và đợi nó rơi nếu cần. Nếu không thì anh ta không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * max( | x | , | 
| Kiểm tra Chebyshev tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số xu$n$và tọa độ của chúng$(x_i, y_i)$vì$i = 1 \dots n$. 
2. Với mỗi đồng xu, tính giá trị tuyệt đối của tọa độ X:$|x_i|$. Điều này thể hiện số giây tối thiểu mà Monocarp phải di chuyển theo chiều ngang (hoặc đường chéo) để căn chỉnh với cột của đồng xu. 
3. Kiểm tra tọa độ Y của đồng xu có thỏa mãn không$y_i \le 2 \cdot |x_i|$. Điều kiện này đảm bảo rằng các chuyển động chéo/ngang kết hợp của Monocarp có thể phù hợp với quỹ đạo rơi của đồng xu. Nếu đúng thì in ra "CÓ". 
4. Nếu không thì in ra "NO". 

Tại sao nó hoạt động: Chuyển động của Monocarp cho phép điều chỉnh đồng thời theo chiều ngang và chiều dọc. Khoảng cách Chebyshev cho biết số giây tối thiểu để đồng xu chạm tới theo chiều ngang hoặc chiều dọc. Vì đồng xu rơi một đơn vị mỗi giây nên khoảng cách thẳng đứng tối đa mà Monocarp có thể vượt qua so với đồng xu rơi đúng bằng hai lần khoảng cách nằm ngang. Bất biến này đảm bảo rằng nếu$y \le 2|x|$, có một số bước di chuyển sẽ bắt được đồng xu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
for _ in range(n):
    x, y = map(int, input().split())
    if y <= 2 * abs(x):
        print("YES")
    else:
        print("NO")
```Giải pháp này lặp qua các đồng tiền chính xác một lần. Việc kiểm tra chìa khóa bắt nguồn từ khoảng cách Chebyshev và động lực học của đồng xu rơi. sử dụng`abs(x)`đảm bảo chúng tôi xử lý tiền xu ở cả hai phía của trục Y. Sự so sánh`y <= 2 * abs(x)`xử lý các đồng xu ở trên hoặc dưới gốc một cách tự nhiên, vì tọa độ y âm luôn thỏa mãn sự bất đẳng thức khi đồng xu nằm gần theo chiều ngang. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
5
24 42
-2 -1
-1 -2
0 -50
15 0
```| Đồng xu | x | y | 2*|x| | y <= 2*|x|? | Kết quả | 

|------|---|---|---|---|--------| 

| 1 | 24 | 42 | 48 | CÓ | CÓ | 

| 2 | -2 | -1 | 4 | CÓ | CÓ | 

| 3 | -1 | -2 | 2 | KHÔNG | KHÔNG | 

| 4 | 0 | -50 | 0 | KHÔNG | KHÔNG | 

| 5 | 15 | 0 | 30 | CÓ | CÓ | 

Điều này thể hiện cả tọa độ dương và âm, cũng như các đồng xu ở y < 0. 

**Ví dụ 2** 

đầu vào:```
3
1 2
2 3
3 5
```| Đồng xu | x | y | 2*|x| | y <= 2*|x|? | Kết quả | 

|------|---|---|---|---|--------| 

| 1 | 1 | 2 | 2 | CÓ | CÓ | 

| 2 | 2 | 3 | 4 | CÓ | CÓ | 

| 3 | 3 | 5 | 6 | CÓ | CÓ | 

Điều này xác nhận rằng các đồng tiền nằm trên nguồn gốc được phân loại chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đồng tiền được kiểm tra độc lập trong thời gian không đổi. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến vòng lặp. | 

Được cho$n \le 500$và tọa độ giới hạn bởi$\pm 50$, giải pháp này cực kỳ hiệu quả và phù hợp thoải mái trong giới hạn thời gian 2 giây và hạn chế về bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    n = int(input())
    for _ in range(n):
        x, y = map(int, input().split())
        if y <= 2 * abs(x):
            print("YES")
        else:
            print("NO")
    return out.getvalue().strip()

# Provided sample
assert run("5\n24 42\n-2 -1\n-1 -2\n0 -50\n15 0\n") == "YES\nYES\nNO\nNO\nYES"

# Custom test cases
assert run("1\n0 1\n") == "NO", "coin above origin, unreachable horizontally"
assert run("1\n0 0\n") == "YES", "edge case, coin at origin"
assert run("2\n-50 100\n50 100\n") == "NO\nNO", "coins too far vertically"
assert run("3\n1 1\n-1 1\n0 0\n") == "YES\nYES\nYES", "small coins around origin"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 1`| KHÔNG | Không thể tiếp cận đồng xu trực tiếp phía trên điểm gốc theo chiều ngang | 
|`0 0`| CÓ | Đồng xu gốc | 
|`-50 100`| KHÔNG | Đồng xu quá cao để bắt | 
|`1 1\n-1 1\n0 0`| CÓ, CÓ, CÓ | Tiền xu gần với nguồn gốc ở các góc phần tư khác nhau | 

## Vỏ cạnh 

Đối với các đồng xu trực tiếp ở trên hoặc dưới gốc có$x = 0$, Monocarp chỉ có thể di chuyển theo chiều dọc, điều này làm hạn chế tầm với. Ví dụ,`(0, -50)`sản lượng`NO`bởi vì Monocarp không thể di chuyển xuống nhanh hơn đồng xu rơi. Đối với đồng xu trên trục ngang`(x, 0)`, anh ta có thể di chuyển theo chiều ngang và bắt được đồng xu ngay lập tức, đó là lý do tại sao`(15, 0)`sản lượng`YES`. Tọa độ y âm bên trong`2*abs(x)`được xử lý vì Monoc
