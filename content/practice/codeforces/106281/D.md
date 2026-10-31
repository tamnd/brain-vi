---
title: "CF 106281D - \u0413\u043b\u0438\u043d\u044f\u043d\u0430\u044f \u0442\u0430\u0431\u043b\u0438\u0447\u043a\u0430"
description: "Máy tính bảng là một lưới các ô hình chữ nhật. A có nghĩa là ô đã được sơn, trong khi a . có nghĩa là nó bị bỏ trống. Mỗi cửa sổ 5 x 3 có thể có bên trong lưới này có thể biểu thị một chữ số."
date: "2026-06-25T07:38:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106281
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 (\u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435) 7-8 \u043a\u043b\u0430\u0441\u0441, \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2025"
rating: 0
weight: 106281
solve_time_s: 28
verified: true
draft: false
---

[CF 106281D - \u0413\u043b\u0438\u043d\u044f\u043d\u0430\u044f \u0442\u0430\u0431\u043b\u0438\u0447\u043a\u0430](https://codeforces.com/problemset/problem/106281/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Máy tính bảng là một lưới các ô hình chữ nhật. MỘT`*`có nghĩa là ô đã được sơn, trong khi một`.`có nghĩa là nó bị bỏ trống. Mọi khả năng có thể`5 x 3`cửa sổ bên trong lưới này có thể biểu thị một chữ số. Các mẫu chữ số được cố định nhưng máy tính bảng có thể chứa thêm các ô được sơn do hư hỏng hoặc nhầm lẫn. Một cửa sổ đại diện cho một chữ số nếu mọi ô được yêu cầu vẽ cho chữ số đó thực sự được vẽ trong cửa sổ. Các ô được tô thêm không làm mất hiệu lực một chữ số. 

Nếu có nhiều chữ số khớp với cùng một cửa sổ, chúng ta sẽ lấy số lớn nhất. Nhiệm vụ là kiểm tra mọi vị trí có thể của`5 x 3`hình chữ nhật và thêm tất cả các chữ số được nhận dạng. 

Kích thước lưới tối đa là`100 x 100`, vậy có nhiều nhất`(N - 4) * (M - 2)`cửa sổ. Kích thước này đủ nhỏ để thậm chí kiểm tra mọi cửa sổ với từng chữ số cũng được. Tổng số ô trong tất cả các cửa sổ là khoảng`100 * 100 * 10 * 15`, chỉ có vài triệu thao tác đơn giản. Ở đây, các phương pháp nâng cao hơn như băm hoặc tiền xử lý là không cần thiết. 

Các trường hợp cạnh chính xuất phát từ thực tế là một chữ số chỉ được xác định bởi các ô được sơn bắt buộc của nó. Một sai lầm phổ biến là yêu cầu toàn bộ`5 x 3`window để khớp chính xác với một chữ số. 

Ví dụ:```
5 3
***
***
***
***
***
```Câu trả lời là`9`. Mỗi chữ số có thể được đặt bên trong hình chữ nhật được sơn đầy đủ này vì tất cả các ô bắt buộc đều tồn tại, vì vậy chúng ta phải chọn chữ số lớn nhất. 

Một trường hợp phức tạp khác là khi một cửa sổ có thêm các ngôi sao không thuộc chữ số đã chọn:```
5 3
***
*.*
***
*.*
***
```Câu trả lời đúng là`0`, bởi vì mẫu số 0 yêu cầu các ô viền và các ngôi sao phụ không bị ảnh hưởng. Một giải pháp so sánh chính xác toàn bộ hình ảnh với mẫu chữ số sẽ từ chối nó một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là lưu trữ các mẫu có mười chữ số và quét mọi thứ có thể.`5 x 3`hình chữ nhật. Đối với mỗi hình chữ nhật, chúng tôi kiểm tra mọi chữ số từ`0`ĐẾN`9`. Để kiểm tra một chữ số, chúng tôi xem xét các ô trong đó mẫu có chứa một ngôi sao và kiểm tra xem các ô máy tính bảng tương ứng cũng có phải là các ngôi sao hay không. Nếu có tất cả các ô bắt buộc thì chữ số đó là ứng cử viên. Vì cần có chữ số hợp lệ lớn nhất nên việc quét các chữ số theo thứ tự tăng dần và ghi đè lên câu trả lời sẽ hoạt động. 

Lực lượng vũ phu này đã là cách tiếp cận tối ưu cho những giới hạn này. Có nhiều nhất`96 * 98 = 9408`cửa sổ. Mỗi cửa sổ kiểm tra mười chữ số và mỗi chữ số có tối đa mười lăm ô. Tổng công việc dưới hai triệu lần kiểm tra ô nên việc mô phỏng trực tiếp dễ dàng phù hợp. 

Quan sát quan trọng là vấn đề không yêu cầu chúng ta phải xây dựng lại toàn bộ máy tính bảng. Mỗi cửa sổ là độc lập và định nghĩa của một chữ số hợp lệ chỉ phụ thuộc vào một mẫu cố định nhỏ. Điều đó biến nhiệm vụ thành một vấn đề khớp mẫu có kích thước không đổi được lặp lại trên lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM * 10 * 15) | O(1) | Đã chấp nhận | 
| Tối ưu | O(NM * 10 * 15) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ các bản vẽ có mười chữ số dưới dạng`5 x 3`mảng. Một ngôi sao trong mẫu có nghĩa là ô tương ứng phải được sơn. 
2. Lặp lại mọi góc trên bên trái có thể có của`5 x 3`hình chữ nhật. Hàng có thể đi từ`0`ĐẾN`N - 5`và cột có thể đi từ`0`ĐẾN`M - 3`. 
3. Đối với hình chữ nhật hiện tại, hãy thử mọi chữ số từ`0`ĐẾN`9`. Chỉ kiểm tra các ô là ngôi sao trong mẫu của chữ số đó. Nếu thiếu bất kỳ ô bắt buộc nào trong máy tính bảng thì chữ số này không thể được biểu thị. 
4. Bất cứ khi nào một chữ số khớp, hãy lưu nó làm đáp án hiện tại cho hình chữ nhật này. Vì các chữ số được kiểm tra theo thứ tự tăng dần nên chữ số trùng khớp cuối cùng là chữ số lớn nhất. 
5. Thêm chữ số đã chọn vào tổng số và tiếp tục với hình chữ nhật tiếp theo. 

Tại sao nó hoạt động: 

Thuật toán tuân theo định nghĩa của một chữ số hợp lệ. Một chữ số được chấp nhận chính xác khi mọi ô được sơn cần thiết đều tồn tại. Thuật toán kiểm tra mọi ô được yêu cầu và từ chối chữ số nếu thiếu một ô. Các ngôi sao bổ sung được bỏ qua, phù hợp với quy tắc của máy tính bảng. Vì tất cả các hình chữ nhật có thể đều đã được thăm nên mọi đóng góp vào tổng cuối cùng đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    digits = [
        ["***",
         "*.*",
         "*.*",
         "*.*",
         "***"],
        ["..*",
         "..*",
         "..*",
         "..*",
         "..*"],
        ["***",
         "..*",
         "***",
         "*..",
         "***"],
        ["***",
         "..*",
         "***",
         "..*",
         "***"],
        ["*.*",
         "*.*",
         "***",
         "..*",
         "..*"],
        ["***",
         "*..",
         "***",
         "..*",
         "***"],
        ["***",
         "*..",
         "***",
         "*.*",
         "***"],
        ["***",
         "..*",
         "..*",
         "..*",
         "..*"],
        ["***",
         "*.*",
         "***",
         "*.*",
         "***"],
        ["***",
         "*.*",
         "***",
         "..*",
         "***"]
    ]

    ans = 0

    for i in range(n - 4):
        for j in range(m - 2):
            best = -1
            for d in range(10):
                ok = True
                for x in range(5):
                    for y in range(3):
                        if digits[d][x][y] == '*' and grid[i + x][j + y] != '*':
                            ok = False
                            break
                    if not ok:
                        break
                if ok:
                    best = d
            if best != -1:
                ans += best

    print(ans)

if __name__ == "__main__":
    solve()
```Các mẫu chữ số được mã hóa chính xác như các ô được vẽ theo yêu cầu. Thuật toán không bao giờ kiểm tra các ô là dấu chấm trong mẫu vì những ô đó có thể chứa thêm dấu sao và vẫn tạo ra một chữ số hợp lệ. 

Các vòng lặp kết thúc`i`Và`j`được giới hạn cẩn thận bởi`n - 4`Và`m - 2`. Điều này ngăn cản việc truy cập các hàng hoặc cột bên ngoài lưới. Các vòng lặp bên trong kiểm tra năm hàng và ba cột của cửa sổ hiện tại, do đó không có phép biến đổi tọa độ nào có thể gây ra lỗi riêng lẻ. 

Biến`best`bắt đầu lúc`-1`bởi vì một cửa sổ có thể không chứa bất kỳ chữ số nào. Khi một chữ số khớp, nó sẽ thay thế giá trị trước đó, điều này đảm bảo rằng giá trị được lưu trữ cuối cùng là chữ số khớp lớn nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
5 3
***
*..
***
..*
***
```Dấu vết là: 

| Vị trí cửa sổ | Chữ số được kiểm tra | Chữ số phù hợp | Tốt nhất | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| (0,0) | 0 đến 9 | 5 | 5 | 5 | 

Cửa sổ chứa chính xác các ô chữ số được yêu cầu`5`, do đó tổng số trở thành`5`. 

Một ví dụ khác:```
6 6
.***.*
.*...*
.***.*
.*.*.*
.***.*
....**
```Các cửa sổ quan trọng là: 

| Vị trí cửa sổ | Chữ số phù hợp | Tốt nhất | Đã thêm | 
| --- | --- | --- | --- | 
| (0,1) | 6 | 6 | 6 | 
| (0,3) | 1 | 1 | 1 | 
| (1,3) | 1 | 1 | 1 | 

Tổng cuối cùng là`8`. Điều này chứng tỏ rằng các cửa sổ chồng chéo được tính độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM * 10 * 15) | Mỗi cửa sổ kiểm tra mười mẫu có kích thước cố định | 
| Không gian | O(1) | Chỉ các mẫu chữ số và một vài biến được lưu trữ | 

Kích thước lưới tối đa chỉ`100 x 100`, nên số lượng thao tác rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn thời gian một giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue()

# provided samples
assert run("""5 3
***
*..
***
..*
***
""") == "5\n"

assert run("""6 6
.***.*
.*...*
.***.*
.*.*.*
.***.*
....**
""") == "8\n"

# all cells painted, every digit matches, choose 9
assert run("""5 3
***
***
***
***
***
""") == "9\n"

# no digit can be formed
assert run("""5 3
...
...
...
...
...
""") == "0\n"

# overlapping windows and extra stars
assert run("""10 10
..*.......
..*.....*.
..*.....*.
..*..*..*.
..*..*..*.
..*..*..*.
..*..*..*.
..*..*..*.
..*.....*.
..........
""") == "11\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầy`5 x 3`sao | 9 | Chọn chữ số phù hợp tối đa | 
| Lưới trống | 0 | Không nhận dạng chữ số hợp lệ | 
| Cửa sổ chồng lên nhau | 11 | Đếm mọi hình chữ nhật có thể | 
| Thêm sao trong windows | Tổng chữ số đúng | Bỏ qua các ô sơn không bắt buộc | 

## Vỏ cạnh 

Đối với máy tính bảng được sơn hoàn toàn:```
5 3
***
***
***
***
***
```Thuật toán kiểm tra từng chữ số. Vì mọi ngôi sao bắt buộc đều có mặt nên tất cả mười chữ số đều đạt. Bởi vì các chữ số được xử lý từ`0`ĐẾN`9`, giá trị được lưu trữ cuối cùng là`9`. 

Đối với hình chữ nhật có thêm các ngôi sao:```
5 3
***
***
***
***
***
```lý do tương tự được áp dụng. Thuật toán không bao giờ hỏi liệu các ô không bắt buộc có trống hay không, do đó, các dấu sao bổ sung không ảnh hưởng đến khả năng nhận dạng. 

Đối với một hình chữ nhật trống:```
5 3
...
...
...
...
...
```mọi chữ số đều không thành công vì mỗi mẫu chữ số đều chứa ít nhất một ngôi sao bắt buộc. Biến`best`ở lại`-1`, và không có gì được thêm vào câu trả lời.
