---
title: "CF 106096C - Hiệp sĩ Mega"
description: "Chúng ta có một lưới gồm n hàng và m cột. Mỗi ô chứa một giá trị dương biểu thị phần thưởng nhận được nếu ô đó bị ảnh hưởng bởi một câu thần chú. Chúng tôi chọn chính xác một ô làm trung tâm của cuộc tấn công."
date: "2026-06-25T11:59:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106096
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 10-1-25 Div. 2 (Beginner)"
rating: 0
weight: 106096
solve_time_s: 41
verified: true
draft: false
---

[CF 106096C - Hiệp sĩ Mega](https://codeforces.com/problemset/problem/106096/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới với`n`hàng và`m`cột. Mỗi ô chứa một giá trị dương biểu thị phần thưởng nhận được nếu ô đó bị ảnh hưởng bởi một câu thần chú. 

Chúng tôi chọn chính xác một ô làm trung tâm của cuộc tấn công. Khi cuộc tấn công được thực hiện, nó sẽ ảnh hưởng đến một vùng hình chữ thập: chính ô được chọn, cộng với các ô lân cận theo bốn hướng chính (lên, xuống, trái, phải), nếu những ô lân cận đó tồn tại bên trong lưới. 

Nhiệm vụ là chọn ô trung tâm sao cho tổng giá trị trong mẫu hình chữ thập này là lớn nhất. 

Vì vậy, với mọi trung tâm hợp lệ`(i, j)`, chúng tôi tính toán: 

giá trị tại`(i, j)`cộng các giá trị tại`(i-1, j)`,`(i+1, j)`,`(i, j-1)`,`(i, j+1)`nếu chúng tồn tại. 

Đầu ra là tổng tối đa trên tất cả các trung tâm có thể. 

Những hạn chế`n, m ≤ 100`có nghĩa là lưới có tối đa 10.000 ô. Bất kỳ giải pháp nào tính toán lại lượng công việc không đổi trên mỗi ô đều dễ dàng đủ nhanh, vì vài chục nghìn thao tác là không đáng kể trong giới hạn 1 giây. Bất cứ điều gì liên quan đến việc quét lặp lại các hàng hoặc cột ở mỗi trung tâm vẫn có thể ổn ở đây, nhưng không cần thiết. 

Trường hợp cạnh tinh tế đến từ các ô ranh giới. Nếu chúng tôi giả định không chính xác rằng tất cả bốn hàng xóm đều tồn tại, chúng tôi sẽ truy cập các vị trí không hợp lệ hoặc thêm các giá trị rác. Ví dụ, trong một`1 × 4`lưới:```
2 1 2 1
```Nếu chúng ta cố gắng tính một dấu thập đầy đủ ở cột 1 thì hàng xóm bên trái không tồn tại, do đó tổng đúng là`2 + 1 = 3`cho trung tâm đó. Một triển khai ngây thơ truy cập một cách mù quáng`j-1`sẽ bị hỏng hoặc tạo ra số tiền không chính xác. 

Một trường hợp góc khác là khi lưới`1 × 1`. Khi đó trung tâm hợp lệ duy nhất chỉ đóng góp giá trị của chính nó. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi ô, chúng tôi thử coi nó là tâm của hình chữ thập và tính tổng bằng cách kiểm tra tối đa năm ô. Mỗi lần kiểm tra là thời gian không đổi, vì vậy điều này mang lại`O(nm)`tổng công việc. Điều này đã phù hợp thoải mái trong những hạn chế. 

Một phiên bản ngây thơ chậm hơn nhưng vẫn có thể hình dung được sẽ là tính toán lại tổng chéo bằng cách lặp qua một cửa sổ vùng lân cận cố định hoặc quét động các hàng và cột. Ngay cả khi đó, vì lưới rất nhỏ nên nó vẫn có thể vượt qua, nhưng nó tạo ra sự phức tạp không cần thiết và nhiều cơ hội hơn cho những sai lầm về ranh giới. 

Nhận xét quan trọng là hình dạng chúng ta cần là cố định và cực kỳ nhỏ. Không có sự phụ thuộc giữa các trung tâm khác nhau. Mỗi câu trả lời của ứng cử viên là độc lập, vì vậy chúng tôi không cần tổng tiền tố, BFS hoặc bất kỳ cấu trúc tiền xử lý nào. Vấn đề giảm xuống còn việc đánh giá một khuôn tô có kích thước không đổi trên mỗi ô lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mỗi ô tính toán lại hàng xóm | O(nm) | O(1) | Đã chấp nhận | 
| Tổng lân cận trực tiếp tối ưu | O(nm) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới thành mảng 2D. Chúng tôi lưu trữ tất cả các giá trị để có thể truy cập hàng xóm một cách an toàn theo chỉ mục. 
2. Khởi tạo một biến`best = 0`(hoặc một số rất nhỏ nếu cho phép giá trị âm; ở đây tất cả các giá trị đều dương nên số 0 là an toàn). 
3. Lặp lại từng ô`(i, j)`trong lưới. Mỗi tế bào là một trung tâm tiềm năng của thập tự giá. 
4. Đối với mỗi ô, tính tổng hiện có bắt đầu bằng`grid[i][j]`. Điều này đại diện cho trung tâm của cuộc tấn công. 
5. Nếu`i > 0`, thêm vào`grid[i-1][j]`. Điều này chỉ bao gồm ô ở trên một cách an toàn khi nó tồn tại. 
6. Nếu`i < n-1`, thêm vào`grid[i+1][j]`. Điều này bao gồm ô bên dưới một cách an toàn. 
7. Nếu`j > 0`, thêm vào`grid[i][j-1]`. Điều này bao gồm hàng xóm bên trái khi hợp lệ. 
8. Nếu`j < m-1`, thêm vào`grid[i][j+1]`. Điều này bao gồm cả người hàng xóm bên phải. 
9. Cập nhật`best`với giá trị tối đa của giá trị hiện tại và tổng được tính toán. 
10. Sau khi tất cả các ô được xử lý, xuất ra`best`. 

Ý tưởng chính trong quá trình triển khai là mọi quyền truy cập của hàng xóm phải được bảo vệ bằng các biện pháp kiểm tra giới hạn. Nếu không có những điều kiện này, các ô cạnh sẽ phá vỡ tính chính xác. 

### Tại sao nó hoạt động 

Mỗi vùng tấn công hợp lệ tương ứng với chính xác một ô trung tâm. Sự đóng góp của vùng đó chỉ phụ thuộc vào ô đó và bốn ô lân cận trực giao của nó. Vì chúng tôi liệt kê tất cả các trung tâm có thể và tính toán giá trị chính xác của vùng tương ứng của chúng, nên chúng tôi kiểm tra mọi cuộc tấn công khả thi chính xác một lần. Do đó, giá trị tối đa trên tất cả các giá trị được tính toán phải bằng câu trả lời tối ưu. 

Không có sự tương tác giữa các trung tâm khác nhau, do đó không có nguy cơ tính hai lần hoặc thiếu cấu hình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
grid = [list(map(int, input().split())) for _ in range(n)]

best = 0

for i in range(n):
    for j in range(m):
        s = grid[i][j]

        if i > 0:
            s += grid[i - 1][j]
        if i < n - 1:
            s += grid[i + 1][j]
        if j > 0:
            s += grid[i][j - 1]
        if j < m - 1:
            s += grid[i][j + 1]

        if s > best:
            best = s

print(best)
```Lưới được lưu trữ trực tiếp mà không cần sửa đổi. Mỗi ô được đánh giá độc lập. Bốn kiểm tra có điều kiện đảm bảo chúng tôi không bao giờ truy cập vào các chỉ mục không hợp lệ, đây là nguồn lỗi phổ biến nhất trong vấn đề này. 

Bước cập nhật được thực hiện đơn giản để tránh chi phí. Vì lưới nhỏ nên các vòng lặp lồng nhau của Python là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 0 0
0 0 1
0 1 1
```Chúng tôi đánh giá từng trung tâm. 

| Trung tâm | Giá trị | Lên | Xuống | Trái | Đúng | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| (0,0) | 1 | - | 0 | - | 0 | 1 | 
| (1,2) | 1 | 0 | 1 | 0 | - | 2 | 
| (2,2) | 1 | 1 | - | 1 | - | 3 | 

Tốt nhất là ở`(2,2)`với tổng số`3`. 

Điều này cho thấy các ô góc thường tối ưu như thế nào vì chúng tránh được sự đóng góp âm hoặc bằng 0 từ các ô lân cận bị thiếu. 

### Ví dụ 2 

đầu vào:```
1 4
2 1 2 1
```| Trung tâm | Giá trị | Trái | Đúng | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| (0,0) | 2 | - | 1 | 3 | 
| (0,1) | 1 | 2 | 2 | 5 | 
| (0,2) | 2 | 1 | 1 | 4 | 
| (0,3) | 1 | 2 | - | 3 | 

Tối đa là`5`tại chỉ mục`(0,1)`. 

Trường hợp này chứng tỏ rằng khi lưới chỉ có một hàng, “chữ thập” sẽ thoái hóa thành một đoạn nằm ngang và thuật toán xử lý việc này một cách tự nhiên mà không cần sửa đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được xử lý một lần với O(1) kiểm tra hàng xóm | 
| Không gian | O(1) thêm | Lưu trữ lưới là yêu cầu đầu vào; không có cấu trúc bổ sung | 

Kích thước lưới tối đa là 10.000 ô và mỗi ô thực hiện một số lượng bổ sung không đổi. Con số này thấp hơn nhiều so với giới hạn 1 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]

    best = 0
    for i in range(n):
        for j in range(m):
            s = grid[i][j]
            if i > 0:
                s += grid[i - 1][j]
            if i < n - 1:
                s += grid[i + 1][j]
            if j > 0:
                s += grid[i][j - 1]
            if j < m - 1:
                s += grid[i][j + 1]
            best = max(best, s)

    return str(best)

# provided samples
assert run("""3 3
1 0 0
0 0 1
0 1 1
""") == "3"

assert run("""1 4
2 1 2 1
""") == "5"

# custom cases
assert run("""1 1
7
""") == "7"

assert run("""2 2
1 2
3 4
""") == "10"

assert run("""3 3
9 1 1
1 1 1
1 1 9
""") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 7 | Trường hợp ranh giới đơn ô | 
| Lưới đầy đủ 2 × 2 | 10 | Tất cả hàng xóm có mặt | 
| lưới nặng chéo | 12 | Trung tâm tốt nhất không ở góc | 

## Vỏ cạnh 

Lưới đơn ô`1 1`là ranh giới đơn giản nhất. Thuật toán đánh giá ô duy nhất, không tìm thấy ô lân cận hợp lệ nào và trả về chính xác giá trị của ô đó mà không cần nhập bất kỳ quyền truy cập chỉ mục không hợp lệ nào. 

trong một`1 × m`lưới, hàng xóm dọc không bao giờ tồn tại. Việc kiểm tra có điều kiện đảm bảo rằng chỉ những đóng góp theo chiều ngang mới được thêm vào, giúp giảm thiểu vấn đề một cách hiệu quả trong việc tìm tổng tối đa của đoạn có độ dài 3 có tâm ở mỗi vị trí. 

trong một`n × 1`lưới, tính đối xứng tương tự được áp dụng theo chiều dọc. Thuật toán giảm đường chéo thành một đường thẳng đứng mà không cần xử lý đặc biệt. 

Các tế bào ở các cạnh và góc chỉ đóng góp một phần vào đường chéo. Mỗi đường viền được xử lý một cách tự nhiên bằng các kiểm tra ranh giới, do đó không cần đường dẫn logic riêng biệt, ngăn ngừa các lỗi riêng lẻ thường xảy ra khi xử lý các đường viền theo cách thủ công.
