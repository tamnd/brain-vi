---
title: "CF 1907A - Xe"
description: "Nhiệm vụ là liệt kê tất cả các nước đi hợp lệ của quân xe từ một ô nhất định trên một bàn cờ trống. Xe di chuyển theo chiều ngang dọc theo hàng của nó hoặc theo chiều dọc dọc theo cột của nó, chỉ dừng lại ở các cạnh của bàn cờ."
date: "2026-06-08T20:36:35+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1907
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 913 (Div. 3)"
rating: 800
weight: 1907
solve_time_s: 116
verified: false
draft: false
---

[CF 1907A - Xe](https://codeforces.com/problemset/problem/1907/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là liệt kê tất cả các nước đi hợp lệ của quân xe từ một ô nhất định trên một bàn cờ trống. Xe di chuyển theo chiều ngang dọc theo hàng của nó hoặc theo chiều dọc dọc theo cột của nó, chỉ dừng lại ở các cạnh của bàn cờ. Mỗi trường hợp kiểm thử sẽ đưa ra một hình vuông theo ký hiệu cờ vua tiêu chuẩn, chẳng hạn như "d5", trong đó ký tự đầu tiên là cột từ 'a' đến 'h' và ký tự thứ hai là hàng từ '1' đến '8'. Đầu ra sẽ liệt kê tất cả các ô mà quân xe có thể tới được từ điểm bắt đầu đó, theo bất kỳ thứ tự nào. 

Ràng buộc đầu vào nhỏ: tối đa 64 trường hợp thử nghiệm và kích thước bảng được cố định ở mức 8x8. Điều này có nghĩa là ngay cả một giải pháp liệt kê một cách ngây thơ tất cả các nước đi cho mỗi trường hợp thử nghiệm chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm, bởi vì một quân xe chỉ có thể có tối đa 14 nước đi có thể có (7 nước dọc theo hàng, 7 nước dọc theo cột) và chỉ có 64 ô vuông. Trường hợp cạnh duy nhất xảy ra khi quân xe ở cạnh hoặc góc của bàn cờ. Ví dụ: nếu quân xe ở "a1", các bước di chuyển hợp pháp của nó là "a2" đến "a8" theo chiều dọc và "b1" đến "h1" theo chiều ngang. Cách tiếp cận bất cẩn có thể vô tình đưa ô vuông bắt đầu vào đầu ra, điều này sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng chuyển động của quân xe. Đối với hàng chứa quân xe, lặp qua tất cả các cột 'a' đến 'h', bỏ qua cột hiện tại của quân xe và xuất ra từng ô vuông. Đối với cột chứa quân xe, lặp qua tất cả các hàng '1' đến '8', bỏ qua hàng hiện tại của quân xe và xuất ra từng ô vuông. Cách tiếp cận này đúng vì nó trực tiếp thực hiện các quy tắc di chuyển của quân xe. Nó cũng hiệu quả vì mỗi ca kiểm thử bao gồm tối đa 14 thao tác, do đó, ngay cả với 64 ca kiểm thử, tổng số thao tác là không đáng kể. 

Không có giải pháp nào nhanh hơn tiệm cận vì bản thân đầu ra có thể lên tới 14 ô vuông cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào ít nhất phải in tất cả các nước đi hợp pháp, vì vậy mô phỏng đơn giản đã là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc chuỗi vị trí như "d5". 
3. Trích xuất ký tự cột và chữ số hàng từ chuỗi. Chuyển đổi cột thành chỉ mục số nguyên nếu thuận tiện hoặc giữ nó dưới dạng ký tự. 
4. Lặp lại tất cả các chữ cái trong cột từ 'a' đến 'h'. Đối với mỗi cột, nếu nó không bằng cột hiện tại của quân xe, hãy xuất ra hình vuông được tạo bởi cột đó và hàng hiện tại của quân xe. 
5. Lặp lại tất cả các chữ số hàng từ '1' đến '8'. Đối với mỗi hàng, nếu nó không bằng hàng hiện tại của quân xe, hãy xuất ra hình vuông được tạo bởi cột hiện tại của quân xe và hàng đó. 
6. Chuyển sang test case tiếp theo. 

Tại sao nó hoạt động: Thuật toán lặp lại chính xác trên tất cả các ô trong hàng và cột của quân xe, bỏ qua ô mà nó hiện đang chiếm giữ. Vì bảng trống nên mọi ô vuông như vậy đều có thể truy cập được và không có ô vuông nào bị bỏ qua hoặc lặp lại. Các ràng buộc đảm bảo rằng các vòng lặp luôn nằm trong phạm vi 'a'-'h' và '1'-'8', do đó các điều kiện biên vốn đã được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    pos = input().strip()
    col, row = pos[0], pos[1]

    for c in 'abcdefgh':
        if c != col:
            print(c + row)
    for r in '12345678':
        if r != row:
            print(col + r)
```Phần đầu tiên đọc số lượng các trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi phân tích cột và hàng từ chuỗi đầu vào. Hai vòng tiếp theo liệt kê tất cả các bước di chuyển ngang có thể có dọc theo hàng và các bước di chuyển dọc dọc theo cột, bỏ qua chính ô vuông bắt đầu. Sử dụng ký tự trực tiếp sẽ tránh được lỗi chuyển đổi giữa chỉ số và chữ cái. Điều này cũng đảm bảo tất cả các nước đi đều được in theo ký hiệu cờ vua tiêu chuẩn. 

## Ví dụ đã hoạt động 

Mẫu 1: 

Đầu vào: "d5" 

| Biến | Giá trị trong vòng lặp hàng | Đầu ra | 
| --- | --- | --- | 
| c | 'a' | 'a5' | 
| c | 'b' | 'b5' | 
| c | 'c' | 'c5' | 
| c | 'd' | bỏ qua | 
| c | 'e' | 'e5' | 
| c | 'f' | 'f5' | 
| c | 'g' | 'g5' | 
| c | 'h' | 'h5' | 

| Biến | Giá trị trong vòng lặp cột | Đầu ra | 
| --- | --- | --- | 
| r | '1' | 'd1' | 
| r | '2' | 'd2' | 
| r | '3' | 'd3' | 
| r | '4' | 'd4' | 
| r | '5' | bỏ qua | 
| r | '6' | 'd6' | 
| r | '7' | 'd7' | 
| r | '8' | 'd8' | 

Dấu vết xác nhận rằng thuật toán tạo ra chính xác 14 ô vuông không bao gồm ô bắt đầu, bao gồm tất cả các nước đi của quân Xe có thể xảy ra. 

Một ví dụ khác: 

Đầu vào: "a1" 

Đầu ra vòng lặp hàng 'b1', 'c1', 'd1', 'e1', 'f1', 'g1', 'h1' 

Đầu ra vòng lặp cột 'a2', 'a3', 'a4', 'a5', 'a6', 'a7', 'a8' 

Tất cả các bước đi hợp pháp đều được thực hiện, cho thấy các vị trí cạnh được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm có tối đa 14 lần lặp để liệt kê các ô vuông. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào được sử dụng. | 

Với tối đa 64 trường hợp thử nghiệm, tổng số thao tác dưới 1000, thấp hơn nhiều so với giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        pos = input().strip()
        col, row = pos[0], pos[1]
        for c in 'abcdefgh':
            if c != col:
                print(c + row)
        for r in '12345678':
            if r != row:
                print(col + r)
    return output.getvalue().strip()

# Provided sample
assert run("1\nd5\n") == '\n'.join([
    'a5','b5','c5','e5','f5','g5','h5',
    'd1','d2','d3','d4','d6','d7','d8'
]), "sample 1"

# Custom: corner
assert run("1\na1\n") == '\n'.join([
    'b1','c1','d1','e1','f1','g1','h1',
    'a2','a3','a4','a5','a6','a7','a8'
]), "corner"

# Custom: opposite corner
assert run("1\nh8\n") == '\n'.join([
    'a8','b8','c8','d8','e8','f8','g8',
    'h1','h2','h3','h4','h5','h6','h7'
]), "opposite corner"

# Custom: center position
assert run("1\ne4\n") == '\n'.join([
    'a4','b4','c4','d4','f4','g4','h4',
    'e1','e2','e3','e5','e6','e7','e8'
]), "center"

# Custom: multiple test cases
assert run("2\nd5\na1\n") == '\n'.join([
    'a5','b5','c5','e5','f5','g5','h5',
    'd1','d2','d3','d4','d6','d7','d8',
    'b1','c1','d1','e1','f1','g1','h1',
    'a2','a3','a4','a5','a6','a7','a8'
]), "multiple"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "a1" | b1-c1-d1-e1-f1-g1-h1, a2-a8 | Rook ở góc | 
| "h8" | a8-g8-f8-... h1-h7 | Góc đối diện | 
| "e4" | a4-d4-f4-h4, e1-e3, e5-e8 | Trung tâm bảng | 
| "d5\na1" | Kết quả đầu ra kết hợp | Nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Đối với quân xe trên "a1", vòng lặp hàng tạo ra 'b1' đến 'h1', bỏ qua 'a1'. Vòng lặp cột tạo 'a2' đến 'a8', bỏ qua 'a1'. Điều này bao phủ góc trên bên trái một cách chính xác mà không có lỗi nào xảy ra. Tương tự, quân xe ở "h8" tạo ra tất cả các nước đi hợp pháp mà không vượt quá ranh giới bàn cờ. Thuật toán vốn đã xử lý mức tối đa và
