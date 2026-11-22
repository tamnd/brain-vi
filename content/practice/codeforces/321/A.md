---
title: "CF 321A - Ciel và Robot"
description: "Chúng ta được yêu cầu xác định xem một robot di chuyển trên mặt phẳng 2D có thể đến được điểm mục tiêu hay không (a, b). Robot bắt đầu tại điểm gốc (0, 0) và tuân theo một chuỗi các bước di chuyển được mã hóa thành một chuỗi."
date: "2026-06-06T02:19:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 321
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 190 (Div. 1)"
rating: 1700
weight: 321
solve_time_s: 68
verified: true
draft: false
---

[CF 321A - Ciel và Robot](https://codeforces.com/problemset/problem/321/A) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, thực hiện, toán học 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem robot di chuyển trên mặt phẳng 2D có thể đến được điểm mục tiêu (_a_, _b_) hay không. Robot bắt đầu tại điểm gốc (0, 0) và tuân theo một chuỗi các bước di chuyển được mã hóa thành một chuỗi. Mỗi ký tự của chuỗi đại diện cho một hướng: 'U' di chuyển robot lên, 'D' xuống, 'L' sang trái và 'R' phải. Khi robot kết thúc chuỗi, nó sẽ lặp lại chuỗi đó vô tận. Câu hỏi đặt ra là liệu tại một thời điểm nào đó trong chuỗi vô hạn này, robot có chiếm giữ chính xác tọa độ mục tiêu hay không. 

Đầu vào là các số nguyên _a_ và _b_, có thể nằm trong khoảng từ -10^9 đến 10^9 và một chuỗi có độ dài lên tới 100. Các ràng buộc ngay lập tức cho thấy rằng việc mô phỏng chuyển động của robot theo đúng nghĩa đen cho tất cả các lần lặp lại của chuỗi là không thể, vì mục tiêu có thể ở rất xa. Chúng ta cần một giải pháp suy luận về vị trí của robot sau bất kỳ số lần lặp lại đầy đủ nào của chuỗi. 

Trường hợp đặc biệt là khi có thể đạt được mục tiêu chính xác ở cuối lần lặp lại hoặc chính xác ở giữa các lần lặp lại. Ví dụ: nếu chuỗi di chuyển robot sang phải một lần ('R') và mục tiêu ở (3, 0), thì chỉ có thể đạt được mục tiêu sau ba lần lặp lại. Một mô phỏng bất cẩn dừng lại sau một lần lặp lại sẽ trả về sai "Không". 

Một trường hợp khó phát hiện khác xảy ra khi chuyển động ròng của robot sau một chuỗi đầy đủ bằng không. Ví dụ: nếu chuỗi là 'UDLR', robot sẽ quay về điểm gốc sau mỗi lần lặp lại đầy đủ. Trong trường hợp này, những điểm có thể tiếp cận duy nhất là những điểm đã được truy cập trong lần lặp lại đầu tiên. Một cách tiếp cận ngây thơ chỉ xem xét vectơ chuyển động ròng sẽ không nắm bắt được các điểm trung gian có thể tiếp cận này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bạn mô phỏng các bước di chuyển của robot vô thời hạn, cập nhật vị trí của nó sau mỗi bước và kiểm tra xem nó có bằng (_a_, _b_) hay không. Lực lượng vũ phu là chính xác vì nó thực sự đi theo con đường của robot. Tuy nhiên, robot có thể cần tới 10^9 bước để tiếp cận mục tiêu ở xa, điều này rõ ràng là không khả thi. Ngay cả việc mô phỏng một lần lặp lại đầy đủ cho mọi số lần lặp lại có thể cũng là quá chậm, bởi vì tọa độ mục tiêu có thể rất lớn trong khi độ dài chuỗi lại ngắn. 

Cái nhìn sâu sắc quan trọng là nhận thấy rằng chuyển động của robot là định kỳ. Đặt (_dx_, _dy_) là sự thay đổi thực sự về vị trí sau một lần lặp lại đầy đủ của chuỗi. Sau khi lặp lại chuỗi _k_ lần, vị trí của robot là độ lệch ban đầu cộng với _k_ lần thay đổi thực, cộng với một số vị trí đạt được trong lần lặp lại đầu tiên. Về mặt hình thức, nếu chúng ta để (_x_i_, _y_i_) biểu thị vị trí của robot sau khi _i_ di chuyển trong lần lặp lại đầu tiên, thì mọi vị trí có thể tiếp cận đều có dạng (_x_i + k_dx*, y_i + k_dy_) đối với một số nguyên không âm _k_. 

Với quan sát này, vấn đề giảm xuống còn việc kiểm tra, đối với mỗi vị trí đạt được trong lần lặp lại đầu tiên, liệu có tồn tại số nguyên không âm _k_ sao cho (_x_i + k_dx* = a* và _y_i + k_dy* = b*). Đây là một bài toán giải phương trình đơn giản với số nguyên. Cần phải đặc biệt cẩn thận khi _dx_ hoặc _dy_ bằng 0 để tránh chia cho 0. Nếu cả hai đều bằng 0, chúng ta chỉ cần kiểm tra vị trí đạt được trong lần lặp lại đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(| a | + | 
| Tối ưu | O( | s | ) | 

## Hướng dẫn thuật toán

1. Tính toán chuyển động ròng (_dx_, _dy_) sau một lần lặp lại đầy đủ của chuỗi bằng cách lặp qua tất cả các ký tự. Tăng hoặc giảm tọa độ x hoặc y tùy theo bước di chuyển. Điều này cung cấp vectơ dịch tổng thể cho mỗi lần lặp lại. 
2. Khởi tạo vị trí của robot tại (0, 0) và lưu trữ tất cả các vị trí đã truy cập trong lần lặp lại đầu tiên trong danh sách. Điều này cho phép chúng ta xem xét tất cả các khả năng bù trừ có thể xảy ra trước khi bắt đầu lặp lại. 
3. Với mỗi vị trí (_x_i_, _y_i_) đạt được trong lần lặp đầu tiên, kiểm tra xem có tồn tại số nguyên không âm _k_ sao cho: 

_x_i + k_dx* = a* và _y_i + k_dy* = b*. 

Nếu _dx_ bằng 0, điều này giảm xuống còn việc kiểm tra xem _x_i = a_, nếu không thì tính k là (a - x_i) / dx. Tương tự với _dy_. 
4. Một _k_ hợp lệ phải thỏa mãn ba điều kiện: nó phải là số nguyên, nó phải không âm và nó phải thỏa mãn đồng thời cả hai phương trình x và y. Nếu _k_ như vậy tồn tại ở bất kỳ vị trí nào, hãy in "Có". 
5. Nếu không tìm thấy _k_ hợp lệ sau khi kiểm tra tất cả các vị trí, hãy in "Không". 

Tại sao nó hoạt động: Thuật toán dựa trên tính bất biến mà mọi vị trí có thể tiếp cận đều có thể được biểu thị dưới dạng vị trí từ lần lặp lại đầu tiên cộng với bội số của vectơ dịch lưới. Bằng cách lặp lại tất cả các khoảng lệch từ lần lặp lại đầu tiên, chúng tôi bao gồm tất cả các vị trí có thể đạt được sau bất kỳ số lần lặp lại nào. Việc kiểm tra số nguyên và không âm đảm bảo rằng chúng tôi chỉ xem xét các lần lặp lại khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b = map(int, input().split())
s = input().strip()

dx, dy = 0, 0
path = [(0, 0)]
for move in s:
    if move == 'U':
        dy += 1
    elif move == 'D':
        dy -= 1
    elif move == 'R':
        dx += 1
    elif move == 'L':
        dx -= 1
    path.append((path[-1][0] + (dx - path[-1][0]), path[-1][1] + (dy - path[-1][1])))

for x_i, y_i in path:
    kx = ky = None
    if dx != 0:
        if (a - x_i) % dx != 0:
            continue
        kx = (a - x_i) // dx
    else:
        if x_i != a:
            continue
        kx = 0

    if dy != 0:
        if (b - y_i) % dy != 0:
            continue
        ky = (b - y_i) // dy
    else:
        if y_i != b:
            continue
        ky = 0

    if kx == ky or dx == 0 or dy == 0:
        if kx >= 0 and ky >= 0:
            print("Yes")
            break
else:
    print("No")
```Đầu tiên chúng ta tính toán vector tịnh tiến. Sau đó chúng ta theo dõi tất cả các vị trí sau mỗi lần di chuyển ở lần lặp lại đầu tiên. Khi kiểm tra số nguyên _k_, chúng tôi xử lý các bản dịch bằng 0 một cách cẩn thận để tránh chia cho 0. Điều kiện cuối cùng đảm bảo rằng _k_ nhất quán trên cả hai trục và không âm. các`else`trên`for`chỉ kích hoạt vòng lặp nếu không tìm thấy _k_ hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
RU
```| Bước | Di chuyển | x | y | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | ban đầu | 
| 1 | R | 1 | 0 | đường dẫn[1] | 
| 2 | Bạn | 1 | 1 | đường dẫn[2] | 

Dịch ròng dx=1, dy=1. Giải (x_i + k_dx = 2, y_i + k_dy = 2). Tại x_i=1, y_i=1: k=1. Số nguyên dương, nghiệm tồn tại. Đầu ra: Có. 

### Mẫu 2 

đầu vào:```
1 2
R
```| Bước | Di chuyển | x | y | 
| --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | 
| 1 | R | 1 | 0 | 

Dịch ròng dx=1, dy=0. Giải (1 + k_1 = 1, 0 + k_0 = 2). k=0 từ x, dy=0 yêu cầu y_i=b, nhưng 0 != 2. Không có giải pháp. Đầu ra: Không. 

Ví dụ đầu tiên xác nhận việc xử lý đúng dx/dy dương. Phần thứ hai thể hiện trường hợp không có cạnh dy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O( | s | 

Thuật toán dễ dàng phù hợp với các ràng buộc. Ngay cả |s| tối đa = 100, việc kiểm tra tất cả các vị trí là không đáng kể. Các phép toán số nguyên và số học modulo rất nhanh và tọa độ mục tiêu lớn được xử lý theo đại số thay vì lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a, b = map(int, input().split())
    s = input().strip()

    dx, dy = 0, 0
    path = [(0, 0)]
    for move in s:
        if move == 'U':
            dy += 1
        elif move == 'D':
            dy -= 1
        elif move == 'R':
            dx += 1
        elif move == 'L':
            dx -= 1
        path.append((path[-1][0] + (dx - path[-1][0]), path[-1][1
```
