---
title: "CF 104157C - Thử thách ném bóng cực mạnh"
description: "Chúng tôi được cung cấp một mục tiêu hình tròn trên mặt phẳng 2D và danh sách các điểm biểu thị nơi các nhân viên khác nhau ném một vật thể. Nhiệm vụ là đếm xem có bao nhiêu điểm được ném này rơi vào bên trong vòng tròn hoặc chính xác trên ranh giới của nó. Mỗi lần ném chỉ là một cặp tọa độ."
date: "2026-07-02T01:14:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104157
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 2 (Beginner)"
rating: 0
weight: 104157
solve_time_s: 74
verified: true
draft: false
---

[CF 104157C - Thử thách ném mạnh](https://codeforces.com/problemset/problem/104157/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mục tiêu hình tròn trên mặt phẳng 2D và danh sách các điểm biểu thị nơi các nhân viên khác nhau ném một vật thể. Nhiệm vụ là đếm xem có bao nhiêu điểm được ném này rơi vào bên trong vòng tròn hoặc chính xác trên ranh giới của nó. 

Mỗi lần ném chỉ là một cặp tọa độ. Vòng tròn được xác định bởi tâm và bán kính của nó. Cú ném thành công nếu khoảng cách Euclide của nó đến tâm nhỏ hơn hoặc bằng bán kính. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cũng phải xử lý từng điểm trong thời gian không đổi. Với số lần ném lên tới 100.000,$O(n)$giải pháp được mong đợi. Bất kỳ điều gì liên quan đến việc sắp xếp hoặc so sánh từng cặp đều không cần thiết và quá chậm. 

Một vấn đề tế nhị thường xuất hiện trong những bài toán như thế này là độ an toàn về số khi tính khoảng cách. Công thức đơn giản bao gồm căn bậc hai và so sánh dấu phẩy động có thể gây ra lỗi chính xác. Một nhược điểm phổ biến khác là sử dụng số nguyên 32-bit khi bình phương tọa độ lên tới$10^9$, tràn ngay lập tức. 

Một số tình huống khó khăn quan trọng: 

Một điểm chính xác trên ranh giới sẽ được tính là thành công. Ví dụ, nếu trung tâm là$(0,0)$và bán kính là$5$, điểm$(3,4)$phải được tính vì$3^2 + 4^2 = 25$. 

Trường hợp tinh vi thứ hai là tọa độ lớn. Nếu chúng ta tính toán$(x - c_x)^2$sử dụng số nguyên 32 bit, giá trị gần$10^9$sản xuất xung quanh$10^{18}$, yêu cầu số học 64-bit. Sử dụng sai loại âm thầm tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách trực tiếp để giải quyết vấn đề là xử lý từng lần ném một cách độc lập và tính khoảng cách của nó đến tâm vòng tròn. Đối với mỗi điểm, chúng tôi tính khoảng cách Euclide bằng công thức$\sqrt{(x_i - c_x)^2 + (y_i - c_y)^2}$và kiểm tra xem nó có nhỏ hơn hoặc bằng không$r$. Điều này đúng vì định nghĩa “bên trong đường tròn” chính xác là điều kiện khoảng cách đó. 

Tuy nhiên, điều này liên quan đến căn bậc hai cho mỗi điểm. Trong khi$O(n)$là ổn về độ phức tạp tiệm cận, các phép toán căn bậc hai tương đối tốn kém và không cần thiết. Quan trọng hơn, so sánh dấu phẩy động có thể gây ra lỗi chính xác gần ranh giới, trong đó cần bao gồm một điểm nhưng có thể bị loại trừ do làm tròn. 

Quan sát quan trọng là chúng ta không bao giờ cần khoảng cách thực tế. Chúng ta chỉ cần so sánh nó với bán kính. Vì cả hai vế đều không âm nên chúng ta có thể bình phương cả hai vế của bất đẳng thức một cách an toàn:$$(x_i - c_x)^2 + (y_i - c_y)^2 \le r^2$$Điều này loại bỏ hoàn toàn căn bậc hai và chuyển bài toán thành số học số nguyên đơn giản. Bây giờ mỗi điểm chỉ yêu cầu một vài phép tính số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (sqrt mỗi điểm) | O(n) | O(1) | Được chấp nhận nhưng mong manh | 
| Kiểm tra khoảng cách bình phương | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số điểm, tọa độ tâm đường tròn và bán kính. Ta tính ngay$r^2$vì vậy chúng tôi không bao giờ cần phải tính toán lại nó trong quá trình xử lý. Điều này tránh được phép nhân lặp lại và giữ cho sự so sánh thống nhất. 
2. Khởi tạo bộ đếm về 0. Điều này sẽ theo dõi có bao nhiêu điểm thỏa mãn điều kiện vòng tròn. 
3. Mỗi lần ném$(x_i, y_i)$, tính độ lệch ngang và dọc từ tâm:$dx = x_i - c_x$,$dy = y_i - c_y$. Các độ lệch này biểu thị vectơ dịch chuyển từ tâm đến điểm. 
4. Tính bình phương khoảng cách$dx^2 + dy^2$. Đây là khoảng cách Euclide bình phương chính xác từ tâm. Chúng tôi cố tình tránh căn bậc hai vì chúng không cần thiết để so sánh. 
5. So sánh bình phương khoảng cách với$r^2$. Nếu nó nhỏ hơn hoặc bằng thì tăng bộ đếm. Điều này trực tiếp tương ứng với định nghĩa ở bên trong hoặc trên ranh giới của vòng tròn. 
6. Sau khi xử lý tất cả các điểm, xuất bộ đếm. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là việc bình phương là đơn điệu đối với các giá trị không âm. Vì cả bình phương khoảng cách và$r^2$luôn không âm, việc so sánh chúng giữ nguyên thứ tự các khoảng cách ban đầu. Điều này có nghĩa là bất đẳng thức sau khi bình phương tương đương với điều kiện hình học ban đầu. Mọi điểm đều được phân loại chính xác theo khoảng cách Euclide thực sự, nhưng không đưa vào tính toán dấu phẩy động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, cx, cy, r = map(int, input().split())
    r2 = r * r

    cnt = 0
    for _ in range(n):
        x, y = map(int, input().split())
        dx = x - cx
        dy = y - cy
        if dx * dx + dy * dy <= r2:
            cnt += 1

    print(cnt)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Lưu trữ bước tiền xử lý$r^2$, đảm bảo mỗi truy vấn chỉ thực hiện một số phép toán số học cố định. 

Bước trừ rất quan trọng vì nó làm giảm tọa độ thành một khung cục bộ có tâm ở gốc đường tròn. Điều này tránh mọi nhu cầu về vị trí tuyệt đối. 

Tất cả số học được thực hiện bằng cách sử dụng số nguyên Python, hỗ trợ các giá trị lớn một cách tự nhiên, do đó không có rủi ro tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1 2 5
0 0
-2 -2
5 6
3 3
```Chúng tôi tính toán$r^2 = 25$. 

| Điểm | dx | nhuộm | dx2 + dy2 | 25 | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| (0,0) | -1 | -2 | 1 + 4 = 5 | vâng | 1 | 
| (-2,-2) | -3 | -4 | 9 + 16 = 25 | vâng | 2 | 
| (5,6) | 4 | 4 | 16 + 16 = 32 | không | 2 | 
| (3,3) | 2 | 1 | 4 + 1 = 5 | vâng | 3 | 

Dấu vết cho thấy đẳng thức biên được chấp nhận, như đã thấy ở điểm thứ hai khi bình phương khoảng cách bằng 25. 

### Ví dụ 2 

đầu vào:```
3 0 0 2
2 0
0 2
2 2
```Chúng tôi tính toán$r^2 = 4$. 

| Điểm | dx | nhuộm | dx2 + dy2 | ≤ 4 | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| (2,0) | 2 | 0 | 4 | vâng | 1 | 
| (0,2) | 0 | 2 | 4 | vâng | 2 | 
| (2,2) | 2 | 2 | 8 | không | 2 | 

Ví dụ này tách biệt trường hợp góc trong đó các điểm nằm chính xác trên ranh giới vòng tròn được bao gồm, trong khi các điểm ngay bên ngoài được loại trừ hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm yêu cầu số học và so sánh theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Giải pháp này tỷ lệ thuận với số lần ném, giải pháp tối ưu là mỗi điểm phải được kiểm tra ít nhất một lần. Với$n \le 10^5$, điều này chạy thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue().strip()
    _sys.stdout = backup
    return out

# provided sample
assert run("""4 1 2 5
0 0
-2 -2
5 6
3 3
""") == "3"

# minimum case
assert run("""1 0 0 1
0 0
""") == "1"

# all outside
assert run("""3 0 0 1
2 0
0 2
-2 -2
""") == "0"

# all inside large radius
assert run("""3 0 0 100
10 10
-20 -30
40 50
""") == "3"

# boundary stress case
assert run("""2 0 0 5
3 4
-3 -4
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hit trung tâm duy nhất | 1 | độ chính xác kích thước tối thiểu | 
| tất cả bên ngoài bán kính nhỏ | 0 | logic từ chối | 
| bán kính lớn | tất cả các điểm | mở rộng quy mô chấp nhận | 
| Điểm ranh giới 3-4-5 | 2 | xử lý ranh giới chính xác | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một điểm nằm chính xác trên đường tròn. Xem xét đầu vào:```
1 0 0 5
3 4
```Thuật toán tính toán$dx = 3$,$dy = 4$, Và$dx^2 + dy^2 = 25$. Từ$r^2 = 25$, điều kiện được thỏa mãn và điểm được tính. Điều này xác nhận việc bao gồm chính xác các điểm biên mà không yêu cầu dung sai dấu phẩy động. 

Một trường hợp cạnh khác là giá trị tọa độ lớn:```
1 1000000000 -1000000000 1
1000000000 -999999999
```Đây$dx = 0$,$dy = 1$, do đó khoảng cách bình phương là 1. Mặc dù các giá trị tọa độ trung gian rất lớn nhưng số học số nguyên của Python xử lý chúng một cách an toàn và phép so sánh vẫn chính xác.
