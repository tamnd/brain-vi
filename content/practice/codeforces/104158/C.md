---
title: "CF 104158C - Thử thách ném bóng cực mạnh"
description: "Chúng ta được cung cấp một mục tiêu hình tròn trên mặt phẳng 2D và một tập hợp các điểm biểu thị nơi nhân viên ném một vật thể. Nhiệm vụ là đếm xem có bao nhiêu điểm rơi vào bên trong hoặc chính xác trên ranh giới của vòng tròn. Mỗi lần ném chỉ là một tọa độ trên mặt phẳng."
date: "2026-07-02T01:09:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104158
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 1 (Advanced)"
rating: 0
weight: 104158
solve_time_s: 61
verified: true
draft: false
---

[CF 104158C - Thử thách ném mạnh](https://codeforces.com/problemset/problem/104158/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mục tiêu hình tròn trên mặt phẳng 2D và một tập hợp các điểm biểu thị nơi nhân viên ném một vật thể. Nhiệm vụ là đếm xem có bao nhiêu điểm rơi vào bên trong hoặc chính xác trên ranh giới của vòng tròn. 

Mỗi lần ném chỉ là một tọa độ trên mặt phẳng. Mục tiêu được xác định bởi trung tâm của nó$(c_x, c_y)$và bán kính$r$. Cú ném thành công nếu khoảng cách đến tâm vòng tròn nhỏ hơn hoặc bằng$r$. 

Đại lượng chính chúng ta cần là khoảng cách Euclide giữa mỗi điểm và tâm. Tuy nhiên, việc tính căn bậc hai lên đến$10^5$điểm là không cần thiết và có thể gây ra các vấn đề về độ chính xác của dấu phẩy động. Thay vào đó, quyết định có thể được thực hiện bằng cách sử dụng khoảng cách bình phương. 

Các ràng buộc đủ chặt chẽ để$O(n)$giải pháp được mong đợi. Từ$n \leq 10^5$, bất kỳ cách tiếp cận nào xử lý từng điểm trong thời gian không đổi là đủ. Bất cứ điều gì liên quan đến việc sắp xếp hoặc so sánh theo cặp sẽ là quá mức. 

Trường hợp cạnh tinh tế phát sinh khi một điểm nằm chính xác trên đường biên của đường tròn. Ví dụ, nếu trung tâm là$(0, 0)$, bán kính là$5$, và một điểm là$(3, 4)$, khoảng cách chính xác là$5$, vậy thì phải tính là thành công. Điều này rất dễ bị xử lý sai nếu sử dụng sự bất bình đẳng nghiêm ngặt thay vì so sánh toàn diện. 

Một cạm bẫy tiềm ẩn khác là tràn số nguyên trong các ngôn ngữ có số nguyên có chiều rộng cố định, vì tọa độ có thể lớn bằng$10^9$. Bình phương sự khác biệt có thể đạt đến$10^{18}$, vì vậy cần có số nguyên 64 bit. Trong Python điều này là an toàn, nhưng lý do vẫn còn quan trọng đối với việc dịch thuật. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tính khoảng cách Euclide cho mọi điểm, lấy căn bậc hai và kiểm tra xem nó có nhỏ hơn hoặc bằng$r$. Điều này đúng vì nó trực tiếp thực hiện định nghĩa hình học của đường tròn. Tuy nhiên, nó thực hiện một phép toán căn bậc hai tốn kém cho mỗi nhân viên, dẫn đến$O(n)$cuộc gọi căn bậc hai. Mặc dù vẫn tuyến tính, nhưng tốc độ này chậm hơn mức cần thiết và có nguy cơ gây ra các vấn đề về độ chính xác khi so sánh các giá trị dấu phẩy động gần ranh giới. 

Quan sát quan trọng là căn bậc hai có tính đơn điệu. So sánh khoảng cách tương đương với so sánh bình phương khoảng cách. Thay vì kiểm tra$$\sqrt{(x - c_x)^2 + (y - c_y)^2} \le r$$chúng ta có thể bình phương cả hai vế một cách an toàn:$$(x - c_x)^2 + (y - c_y)^2 \le r^2$$Điều này loại bỏ hoàn toàn số học dấu phẩy động và giảm phép tính trên mỗi điểm xuống còn một vài phép trừ và nhân số nguyên. Vấn đề trở thành một quá trình quét tuyến tính đơn giản với việc kiểm tra liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (với sqrt) | O(n) | O(1) | Được chấp nhận nhưng chậm hơn và rủi ro hơn | 
| Kiểm tra khoảng cách bình phương | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lần ném và thông số vòng tròn$c_x, c_y, r$. Điều này xác định điểm tham chiếu cố định và ngưỡng cho tất cả các tính toán. 
2. Tính toán trước$r^2$. Điều này tránh việc tính toán lại nó cho mọi điểm và đảm bảo tất cả các phép so sánh đều ở dạng số nguyên. 
3. Khởi tạo bộ đếm về 0. Điều này sẽ theo dõi số lần ném đất vào bên trong hoặc trên ranh giới của vòng tròn. 
4. Đối với nỗ lực của mỗi nhân viên$(x_i, y_i)$, tính độ lệch ngang và dọc từ tâm:$dx = x_i - c_x$,$dy = y_i - c_y$. Điều này biến bài toán thành đo khoảng cách từ gốc tọa độ trong hệ tọa độ đã dịch chuyển. 
5. Tính bình phương khoảng cách$d = dx^2 + dy^2$. Điều này thể hiện khoảng cách Euclide bình phương chính xác từ tâm. 
6. So sánh$d$với$r^2$. Nếu như$d \le r^2$, tăng bộ đếm. Trường hợp đẳng thức được đưa vào vì các điểm trên đường biên được coi là thành công. 
7. Sau khi xử lý tất cả các điểm, xuất bộ đếm. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là bình phương hoàn toàn đơn điệu đối với các giá trị không âm. Vì cả bình phương khoảng cách và$r^2$không âm, việc so sánh các giá trị bình phương sẽ giữ nguyên thứ tự của khoảng cách thực tế. Mỗi điểm được phân loại dựa trên một điều kiện tương đương, do đó không có trường hợp hình học nào bị phân loại sai. Việc chuyển đổi từ khoảng cách Euclide sang khoảng cách bình phương đảm bảo tính chính xác trong khi loại bỏ hoàn toàn tính toán dấu phẩy động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, cx, cy, r = map(int, input().split())
    r2 = r * r
    ans = 0

    for _ in range(n):
        x, y = map(int, input().split())
        dx = x - cx
        dy = y - cy
        if dx * dx + dy * dy <= r2:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc tất cả đầu vào trong thời gian không đổi trên mỗi dòng. Bán kính bình phương được tính một lần, giúp tránh phép nhân lặp lại bên trong vòng lặp. Mỗi điểm được xử lý độc lập, tính toán độ lệch của nó từ tâm trước khi đánh giá điều kiện khoảng cách bình phương. 

Việc so sánh sử dụng`<=`còn hơn là`<`, điều này rất cần thiết vì các điểm biên là các lần truy cập hợp lệ. Tất cả số học được thực hiện bằng cách sử dụng số nguyên Python, xử lý các giá trị lớn một cách tự nhiên mà không bị tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1 2 5
0 0
-2 -2
5 6
3 3
```Chúng tôi tính toán$r^2 = 25$. 

| Điểm | dx | nhuộm | dx2 + dy2 | 25 | 
| --- | --- | --- | --- | --- | 
| (0,0) | -1 | -2 | 1 + 4 = 5 | vâng | 
| (-2,-2) | -3 | -4 | 9 + 16 = 25 | vâng | 
| (5,6) | 4 | 4 | 16 + 16 = 32 | không | 
| (3,3) | 2 | 1 | 4 + 1 = 5 | vâng | 

Câu trả lời là 3. 

Dấu vết này cho thấy đẳng thức ở mức 25 được chấp nhận, xác nhận việc xử lý ranh giới chính xác. 

### Ví dụ 2 

đầu vào:```
3 0 0 3
3 0
2 2
0 4
```Chúng tôi tính toán$r^2 = 9$. 

| Điểm | dx | nhuộm | dx2 + dy2 | ≤ 9 | 
| --- | --- | --- | --- | --- | 
| (3,0) | 3 | 0 | 9 | vâng | 
| (2,2) | 2 | 2 | 8 | vâng | 
| (0,4) | 0 | 4 | 16 | không | 

Câu trả lời là 2. 

Điều này xác nhận rằng các điểm nằm ngoài vòng tròn được loại trừ chính xác ngay cả khi một tọa độ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm được xử lý một lần với số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được sử dụng | 

Giải pháp là tối ưu cho$n \leq 10^5$. Mỗi lần lặp chỉ sử dụng một số phép toán số nguyên, vừa vặn trong giới hạn 1 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else __import__("builtins").print(solve())

# Re-define properly for isolated runs
def run(inp: str) -> str:
    import sys, io
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = backup_stdin
        sys.stdout = backup_stdout

# provided sample
assert run("""4 1 2 5
0 0
-2 -2
5 6
3 3
""") == "3"

# minimum input
assert run("""1 0 0 1
0 0
""") == "1"

# boundary-only hit
assert run("""2 0 0 5
3 4
-3 -4
""") == "2"

# all outside
assert run("""3 0 0 2
3 3
-3 -3
5 0
""") == "0"

# large coordinate edge
assert run("""2 1000000000 -1000000000 1
1000000000 -999999999
0 0
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất ở trung tâm | 1 | tính đúng đắn của trường hợp tối thiểu | 
| (3,4) lượt truy cập ranh giới kiểu | 2 | ranh giới vòng tròn chính xác | 
| tất cả các điểm bên ngoài | 0 | từ chối nghiêm khắc | 
| tọa độ lớn gần giới hạn | 1 | số học an toàn tràn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một điểm nằm chính xác ở khoảng cách$r$. Ví dụ, trung tâm$(0,0)$, bán kính$5$, điểm$(3,4)$. Thuật toán tính toán$dx^2 + dy^2 = 25$, bằng với$r^2$, do đó nó được tính chính xác. Điều này xác nhận điều kiện biên bao hàm. 

Một trường hợp cạnh khác là khi tọa độ cực lớn, chẳng hạn như$(10^9, -10^9)$. Khoảng cách bình phương sẽ trở thành khoảng$2 \cdot 10^{18}$, vẫn phù hợp an toàn với số nguyên Python. Thuật toán vẫn đúng vì nó không bao giờ dựa vào độ chính xác của dấu phẩy động. 

Trường hợp cuối cùng là khi tất cả các điểm trùng với tâm. Mọi$dx$Và$dy$trở thành 0, do đó tất cả các điểm đều được tính, khớp với định nghĩa hình học của đường tròn chứa tâm của nó.
