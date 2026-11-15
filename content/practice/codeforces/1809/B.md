---
title: "CF 1809B - Điểm trên mặt phẳng"
description: "Chúng ta muốn đặt n chip lên các điểm mạng nguyên của mặt phẳng. Giá của một con chip tại (x, y) là khoảng cách từ Manhattan đến điểm gốc. Các con chip phải thỏa mãn một hạn chế hình học: mỗi cặp chip phải cách nhau hơn 1 đơn vị trong khoảng cách Euclide."
date: "2026-06-09T08:50:45+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1809
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 145 (Rated for Div. 2)"
rating: 1000
weight: 1809
solve_time_s: 83
verified: true
draft: false
---

[CF 1809B - Điểm trên mặt phẳng](https://codeforces.com/problemset/problem/1809/B) 

**Đánh giá:** 1000 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi muốn đặt`n`chip trên các điểm mạng số nguyên của mặt phẳng. Giá của một con chip ở mức`(x, y)`là khoảng cách Manhattan của nó tới điểm gốc,`|x| + |y|`. Chi phí của toàn bộ việc sắp xếp là chi phí chip lớn nhất trong số tất cả các chip được đặt. 

Các con chip phải thỏa mãn một hạn chế về hình học: mỗi cặp chip phải lớn hơn`1`cách nhau một đơn vị trong khoảng cách Euclide. 

Nhiệm vụ là tìm chi phí tối đa nhỏ nhất có thể cho phép đặt chính xác`n`chip. 

Đầu vào chứa tối đa`10^4`các trường hợp thử nghiệm và mỗi trường hợp`n`có thể lớn như`10^18`. Giá trị khổng lồ như vậy ngay lập tức loại trừ bất kỳ cách tiếp cận nào xây dựng điểm một cách rõ ràng hoặc lặp lại tối đa`n`. Thậm chí một`O(√n)`thuật toán sẽ quá chậm đối với đầu vào lớn nhất. Chúng ta cần thứ gì đó xung quanh`O(log n)`mỗi trường hợp thử nghiệm. 

Thách thức chính là hiểu được có bao nhiêu điểm lưới có thể phù hợp với một giới hạn chi phí nhất định. 

Một trường hợp cạnh tinh tế là`n = 1`. 

đầu vào:```
1
1
```Đầu ra:```
0
```Một con chip có thể được đặt ở gốc tọa độ, vì vậy câu trả lời là`0`. Một giải pháp giả định chi phí tối thiểu ít nhất là`1`sẽ thất bại. 

Một ranh giới quan trọng khác là khi`n`chính xác là một hình vuông hoàn hảo. 

đầu vào:```
1
9
```Đầu ra:```
3
```Câu trả lời là không`2`, bởi vì giới hạn chi phí của`2`chỉ chứa`3² = 9`vị trí hợp lệ? Quan sát này thực sự là toàn bộ giải pháp. Việc sai một công thức đếm sẽ dẫn đến câu trả lời sai xung quanh các ô vuông hoàn hảo. 

Nguồn lỗi cuối cùng là việc xử lý các giá trị gần`10^18`. 

đầu vào:```
1
1000000000000000000
```Câu trả lời là`1000000000`. Bất kỳ việc triển khai nào sử dụng căn bậc hai dấu phẩy động đều có nguy cơ xảy ra lỗi chính xác gần phạm vi này. Số học số nguyên an toàn hơn. 

## Phương pháp tiếp cận 

Tư duy bạo lực bắt đầu bằng việc ấn định một số chi phí tối đa cho phép`k`và đếm xem có bao nhiêu điểm mạng thỏa mãn`|x| + |y| ≤ k`. Bộ này tạo thành một viên kim cương xung quanh điểm gốc. 

Không xét đến giới hạn khoảng cách, số điểm mạng trong viên kim cương đó là$$1 + 2k(k+1).$$Sau đó người ta có thể thử tăng`k`cho đến khi có đủ điểm. 

Vấn đề là việc hạn chế khoảng cách sẽ thay đổi mọi thứ. Hai điểm mạng lân cận như`(0,0)`Và`(1,0)`chính xác là khoảng cách`1`ngoài, đó là điều bị cấm. Chúng ta không thể sử dụng tất cả các điểm trong viên kim cương. 

Quan sát quan trọng là điều kiện khoảng cách trở nên rất đơn giản trên lưới số nguyên. Vì tọa độ là số nguyên nên cách duy nhất để hai điểm mạng phân biệt có khoảng cách tối đa là`1`là nếu chúng khác nhau đúng một đơn vị theo chiều ngang hoặc chiều dọc. 

Điều đó gợi ý việc chọn các điểm chỉ có một điểm chẵn lẻ. Hãy xem xét tất cả các điểm mạng nơi`x + y`là chẵn. Hai điểm bất kỳ như vậy khác nhau một giá trị chẵn trong`x + y`, vì vậy chúng không bao giờ có thể là hàng xóm theo chiều ngang hoặc chiều dọc. Khoảng cách Euclide của chúng luôn lớn hơn`1`. 

Bây giờ hãy nhìn vào viên kim cương`|x| + |y| ≤ k`. Trong số các điểm mạng của nó, chính xác$$(k+1)^2$$thậm chí có sự ngang bằng. 

Thậm chí tốt hơn, điều này là tối ưu. Đồ thị mạng hình thành bởi các vùng kề ngang và dọc là đồ thị lưỡng cực, được phân chia theo tính chẵn lẻ. Mỗi tập hợp được chấp nhận là một tập hợp độc lập trong biểu đồ này và một lớp chẵn lẻ đã đạt được kích thước tối đa có thể. 

Vì vậy với giới hạn chi phí`k`, chúng ta có thể đặt chính xác`(k+1)^2`chip. 

Vấn đề bây giờ được rút gọn thành việc tìm giá trị nhỏ nhất`k`như vậy$$(k+1)^2 \ge n.$$Từ`n`có thể lớn như`10^{18}`, chúng ta có thể giải quyết vấn đề này bằng tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k²) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Về giới hạn chi phí dự tuyển`k`, tính xem có thể đặt bao nhiêu chip:$$(k+1)^2$$Điều này xuất phát từ việc lấy tất cả các điểm mạng của một điểm chẵn lẻ bên trong viên kim cương. 
2. Quan sát lớn hơn`k`luôn cho phép ít nhất nhiều chip càng nhỏ càng tốt`k`. 

chức năng`(k+1)^2`là đơn điệu, làm cho tìm kiếm nhị phân có thể áp dụng được. 
3. Tìm kiếm nhị phân nhỏ nhất`k`như vậy:$$(k+1)^2 \ge n$$4. Khởi tạo`low = 0`Và`high = 10^9`. 

Từ`n ≤ 10^{18}`, câu trả lời không bao giờ vượt quá`10^9 - 1`. 
5. Trong quá trình tìm kiếm, hãy`mid = (low + high) // 2`. 
6. Nếu`(mid + 1)^2 ≥ n`, giữ nửa bên trái bằng cách thiết lập`high = mid`. 

Điều này có nghĩa`mid`đã đủ rồi, nhưng có thể có một câu trả lời hợp lệ nhỏ hơn. 
7. Nếu không thì đặt`low = mid + 1`. 

Điều này có nghĩa`mid`không thể chứa đủ chip. 
8. Khi nào`low == high`, xuất ra giá trị đó. 

### Tại sao nó hoạt động 

Đối với mọi giới hạn chi phí`k`, tập hợp các điểm mạng hợp lệ lớn nhất bên trong`|x| + |y| ≤ k`có kích thước`(k+1)^2`. Một lớp chẵn lẻ đạt được kích thước này và không tồn tại tập độc lập lớn hơn vì đồ thị kề mạng tinh thể bên trong hình thoi là lưỡng cực, với cạnh lớn hơn chứa chính xác`(k+1)^2`đỉnh. 

Vì vậy, giới hạn chi phí`k`khả thi khi và chỉ khi`(k+1)^2 ≥ n`. Tìm kiếm nhị phân tìm thấy khả năng nhỏ nhất có thể`k`, đó chính xác là chi phí tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())

    lo, hi = 0, 10**9

    while lo < hi:
        mid = (lo + hi) // 2

        if (mid + 1) * (mid + 1) >= n:
            hi = mid
        else:
            lo = mid + 1

    print(lo)
```Lời giải xoay quanh vị từ đơn điệu$$(k+1)^2 \ge n.$$Đối với bất kỳ cố định`k`, điều này cho chúng ta biết liệu viên kim cương có bán kính`k`có thể chứa đủ vị trí chip hợp lệ. 

Tìm kiếm nhị phân duy trì tính bất biến là câu trả lời luôn nằm bên trong`[lo, hi]`. Bất cứ khi nào`mid`khả thi, chúng ta tiếp tục tìm kiếm ở bên trái vì chúng ta muốn có chi phí tối thiểu. Bất cứ khi nào`mid`là không thể thực hiện được, mọi giá trị nhỏ hơn cũng không thể thực hiện được, vì vậy chúng ta di chuyển sang phải. 

Tất cả số học được thực hiện bằng cách sử dụng số nguyên. Điều này tránh các vấn đề về độ chính xác có thể xuất hiện khi sử dụng căn bậc hai dấu phẩy động gần`10^18`. 

Giới hạn trên`10^9`là đủ vì$$(10^9)^2 = 10^{18}.$$Vì vậy, một câu trả lời lớn hơn`10^9`không bao giờ cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
```| Bước | lo | xin chào | giữa | (giữa+1)2 | Khả thi? | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 1000000000 | 500000000 | khổng lồ | Có | 
| ... | ... | ... | ... | ... | ... | 
| Gần cuối | 0 | 3 | 1 | 4 | Không | 
| Tiếp theo | 2 | 3 | 2 | 9 | Có | 
| Cuối cùng | 2 | 2 | - | - | Trả lời | 

Đầu ra:```
2
```Giới hạn chi phí của`1`chỉ cho phép`4`chip, trong khi giới hạn chi phí là`2`cho phép`9`. Giá trị khả thi tối thiểu là`2`. 

### Ví dụ 2 

đầu vào:```
n = 975461057789971042
```| Bước | lo | xin chào | giữa | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 1000000000 | 500000000 | Quá nhỏ | 
| ... | ... | ... | ... | ... | 
| Cuối cùng | 987654321 | 987654321 | - | Trả lời | 

Đầu ra:```
987654321
```Thực vậy,$$(987654321+1)^2 = 987654322^2 = 975461057789971684$$đó là ít nhất`975461057789971042`, trong khi một chi phí ít hơn là không đủ. 

Dấu vết này chứng tỏ rằng phương thức xử lý các giá trị gần với`10^18`hoàn toàn bằng số học số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Tìm kiếm nhị phân trên câu trả lời | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Từ`log2(10^18)`là về`60`, mỗi ca kiểm thử chỉ thực hiện một số lần lặp nhỏ. Với nhiều nhất`10^4`trường hợp thử nghiệm, tổng công việc dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        lo, hi = 0, 10**9

        while lo < hi:
            mid = (lo + hi) // 2

            if (mid + 1) * (mid + 1) >= n:
                hi = mid
            else:
                lo = mid + 1

        ans.append(str(lo))

    print("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

# provided sample
assert run(
"""4
1
3
5
975461057789971042
"""
) == """0
1
2
987654321
"""

# minimum size
assert run(
"""1
1
"""
) == """0
"""

# perfect square boundary
assert run(
"""1
9
"""
) == """2
"""

# just above perfect square
assert run(
"""1
10
"""
) == """3
"""

# maximum value
assert run(
"""1
1000000000000000000
"""
) == """999999999
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`|`0`| Câu trả lời nhỏ nhất có thể | 
|`n = 9`|`2`| Ranh giới vuông hoàn hảo chính xác | 
|`n = 10`|`3`| Xa nhau sau một ranh giới | 
|`n = 10^18`|`999999999`| Kích thước đầu vào lớn nhất | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất:```
1
1
```Việc tìm kiếm kiểm tra xem`(k+1)^2 ≥ 1`. Đã ở`k = 0`, điều kiện đúng vì`1² = 1`. Thuật toán trả về`0`, tương ứng với việc đặt chip tại`(0,0)`. 

Bây giờ hãy xem xét một hình vuông hoàn hảo:```
1
9
```Thuật toán tìm giá trị nhỏ nhất`k`với`(k+1)^2 ≥ 9`. Vì`k = 1`, chúng tôi nhận được`4`, điều đó là không đủ. Vì`k = 2`, chúng tôi nhận được`9`, thế là đủ rồi. Câu trả lời là`2`. Điều này khẳng định rằng sự bình đẳng phải được coi là khả thi. 

Cuối cùng, hãy xem xét:```
1
10
```Vì`k = 2`, chúng ta vẫn chỉ có`9`các vị trí có sẵn. Giá trị tiếp theo cho`(3+1)^2 = 16`, làm`k = 3`câu trả lời. Đây là trường hợp riêng lẻ cổ điển phát hiện các điều kiện tìm kiếm nhị phân không chính xác. 

Đối với phạm vi tối đa:```
1
1000000000000000000
```Điều kiện trở thành`(k+1)^2 ≥ 10^18`. Giá trị hợp lệ nhỏ nhất là`k = 999999999`, từ`(10^9)^2 = 10^18`. Số học số nguyên xử lý việc này một cách an toàn, tránh mọi vấn đề làm tròn dấu phẩy động.
