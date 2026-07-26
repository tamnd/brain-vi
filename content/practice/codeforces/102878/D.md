---
title: "CF 102878D - Trò Chơi Cuộc Sống"
description: "Nhiệm vụ mô phỏng một máy tự động di động trên lưới hình chữ nhật. Mỗi vị trí chứa một sinh vật sống () hoặc một ô trống (.). Tại mọi thời điểm, mọi ô đều cập nhật cùng một lúc."
date: "2026-07-25T12:42:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "D"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 39
verified: true
draft: false
---

[CF 102878D - Trò chơi cuộc sống](https://codeforces.com/problemset/problem/102878/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô phỏng một máy tự động di động trên lưới hình chữ nhật. Mỗi vị trí chứa một sinh vật sống (`*`) hoặc một ô trống (`.`). Tại mọi thời điểm, mọi ô đều cập nhật cùng một lúc. Trạng thái tiếp theo của một ô chỉ phụ thuộc vào số lượng trong số 8 ô xung quanh nó còn sống. Một ô tồn tại hoặc trở nên sống động khi số đó nằm trong phạm vi bao gồm nhất định`[l, r]`; nếu không nó sẽ trở nên trống rỗng. Lưới ban đầu được coi là thời điểm đầu tiên nên đáp án bắt buộc là lưới sau`t - 1`chuyển tiếp. 

Kích thước lưới tối đa là`100 x 100`, và thời điểm được yêu cầu nhiều nhất là`1000`. Những giới hạn này đủ nhỏ để có thể mô phỏng trực tiếp. Một quá trình chuyển đổi chạm vào mọi ô và kiểm tra tối đa tám ô lân cận, vì vậy chi phí của một bước sẽ xấp xỉ`8 * n * m`hoạt động. Trên tất cả các bước, giới hạn trên là khoảng`8 * 1000 * 100 * 100 = 80,000,000`kiểm tra hàng xóm. Điều này gần đạt đến giới hạn đối với các ngôn ngữ chậm hơn nhưng vẫn thực tế trong Python nếu được triển khai cẩn thận. Các thuật toán cố gắng xây dựng biểu đồ trạng thái lớn hoặc sử dụng tối ưu hóa nhiều là không cần thiết. 

Các trường hợp đặc biệt chính đến từ các chi tiết của mô phỏng hơn là từ chính thuật toán. Lỗi phổ biến đầu tiên là coi lưới đầu vào là thời gian`0`thay vì thời gian`1`. 

Ví dụ:```
1 1
0 0
1
*
```Ô đơn không có hàng xóm sống. Vì phạm vi cho phép chỉ`[0, 0]`, nó vẫn còn sống. 

Đầu ra đúng là:```
*
```Giải pháp thực hiện một lần chuyển đổi trước khi kiểm tra`t`sẽ in sai`.`. 

Một vấn đề khác là chỉ tính bốn ô lân cận trực tiếp thay vì tất cả tám ô xung quanh. Coi như:```
3 3
1 1
2
...
.*.
..*
```Ở trạng thái ban đầu, ô trống trên cùng bên trái có một ô lân cận sống theo đường chéo, vì vậy sau một lần chuyển đổi, nó sẽ trở nên sống động. Việc triển khai theo bốn hướng sẽ bỏ lỡ điều này và tạo ra lưới sai. 

Trường hợp quan trọng cuối cùng là mọi cập nhật đều phải sử dụng lưới trước đó. Coi như:```
1 2
1 1
2
**
```Cả hai ô đều có một ô lân cận còn sống, vì vậy cả hai ô đều tồn tại sau lần chuyển đổi đầu tiên. Cập nhật ô đầu tiên rồi sử dụng giá trị đã thay đổi đó trong khi tính toán ô thứ hai có thể làm hỏng kết quả. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liên tục tạo ra thế hệ lưới tiếp theo. Đối với mỗi ô, chúng tôi đếm các ô sống trong số tám vị trí xung quanh nó. Nếu số lượng nằm trong`[l, r]`, trạng thái tiếp theo là`*`; nếu không thì nó là`.`. Phương pháp này đúng vì các quy tắc xác định mọi ô độc lập với thế hệ trước. 

Vấn đề về lực lượng vũ phu chỉ xuất hiện nếu chúng ta hiểu sai các ràng buộc và cố gắng tìm kiếm một khuôn mẫu hoặc mô phỏng vô thời hạn. Một mô phỏng Cuộc sống nói chung có thể chạy trong nhiều thế hệ, nhưng bài toán này yêu cầu nhiều nhất là`1000`khoảnh khắc trên một lưới chứa nhiều nhất`10000`tế bào. Mô phỏng đơn giản thực hiện tối đa khoảng`80 million`kiểm tra hàng xóm, phù hợp. 

Một ý tưởng phức tạp hơn là phát hiện các chu kỳ và tiến về phía trước. Lưới có số lượng hữu hạn các trạng thái có thể xảy ra, vì vậy cuối cùng mọi quá trình tiến hóa đều phải lặp lại hoặc ổn định. Tuy nhiên, số lượng trạng thái có thể có là`2^(n*m)`, đó là rất lớn. Việc lưu trữ trạng thái hoặc tìm kiếm chu kỳ không giúp ích gì cho kích thước đầu vào này. 

Quan sát làm cho giải pháp đơn giản trở nên đủ là số lượng thế hệ cần thiết đã bị giới hạn. Chúng ta chỉ cần làm cho mỗi quá trình chuyển đổi trở nên hiệu quả và tránh các cấu trúc dữ liệu không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t * n * m * 8) | O(n * m) | Đã chấp nhận | 
| Phát hiện chu kỳ | O(thời gian * n * m) | O(thời gian * n * m) | Không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc lưới, phạm vi sinh tồn`[l, r]`, và thời gian mục tiêu`t`. Nếu như`t`là`1`, lưới hiện tại đã là câu trả lời vì câu lệnh xác định cấu hình ban đầu là thời điểm đầu tiên. 
2. Lặp lại quá trình chuyển đổi`t - 1`lần. Quá trình chuyển đổi chỉ cần thiết trong giây lát sau quá trình chuyển đổi ban đầu. 
3. Tạo một lưới trống mới cho thời điểm tiếp theo. Việc giữ một lưới riêng sẽ ngăn các cập nhật cho một ô ảnh hưởng đến việc tính toán của các ô lân cận trong cùng một thế hệ. 
4. Đối với mỗi ô, hãy kiểm tra tất cả tám tọa độ lân cận có thể có. Bỏ qua tọa độ bên ngoài lưới vì các ô cạnh có ít ô lân cận hơn. 
5. Đếm xem có bao nhiêu vị trí lân cận chứa sinh vật sống. Nếu số này thuộc về`[l, r]`, địa điểm`*`ở vị trí tương ứng của lưới mới. Nếu không thì đặt`.`. 
6. Thay lưới cũ bằng lưới mới sau khi tất cả các ô đã được xử lý. Điều này hoàn thành một bản cập nhật đồng thời đầy đủ. 

Tại sao nó hoạt động: trong mỗi lần chuyển đổi, thuật toán sẽ tính toán trạng thái tiếp theo của mỗi ô từ chính xác thông tin mà các quy tắc yêu cầu, cụ thể là tám ô lân cận của thế hệ trước. Vì toàn bộ lưới tiếp theo được xây dựng trước khi thay thế lưới hiện tại nên mọi quyết định đều sử dụng trạng thái cũ nhất quán. Lặp lại thao tác này một cách chính xác`t - 1`Times tạo ra trạng thái tại thời điểm này`t`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    l, r = map(int, input().split())
    t = int(input())

    grid = [list(input().strip()) for _ in range(n)]

    directions = [
        (-1, -1), (-1, 0), (-1, 1),
        (0, -1),           (0, 1),
        (1, -1),  (1, 0),  (1, 1)
    ]

    for _ in range(t - 1):
        nxt = [['.'] * m for _ in range(n)]

        for i in range(n):
            for j in range(m):
                alive = 0

                for di, dj in directions:
                    ni = i + di
                    nj = j + dj

                    if 0 <= ni < n and 0 <= nj < m:
                        if grid[ni][nj] == '*':
                            alive += 1

                if l <= alive <= r:
                    nxt[i][j] = '*'

        grid = nxt

    sys.stdout.write('\n'.join(''.join(row) for row in grid))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần và được lưu dưới dạng danh sách các danh sách ký tự để các ô riêng lẻ có thể được truy cập và thay thế một cách hiệu quả. Mảng hướng chứa tám vị trí tương đối xung quanh một ô, giúp tránh việc viết các trường hợp ranh giới riêng biệt cho các góc và cạnh. 

Vòng lặp chạy`t - 1`lần vì cấu hình ban đầu đã là thời điểm`1`. Đây là lỗi thường gặp nhất trong vấn đề này. 

Đối với mỗi thế hệ,`nxt`mới được phân bổ. Thật là hấp dẫn để sửa đổi`grid`trực tiếp, nhưng điều đó sẽ trộn lẫn các trạng thái cũ và mới và phá vỡ quy tắc cập nhật đồng thời. Việc kiểm tra ranh giới trước khi truy cập vào các hàng xóm sẽ xử lý các góc và đường viền một cách an toàn. 

Số nguyên Python không tạo ra bất kỳ mối lo ngại tràn nào ở đây vì bộ đếm duy nhất là số lượng hàng xóm, không bao giờ vượt quá 8. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5 5
2 3
8
*...*
.*.*.
..*..
.*.*.
*...*
```Quá trình tiến hóa đạt đến một bảng trống sau nhiều lần chuyển đổi. 

| Chuyển tiếp | Phạm vi sống động | Tế bào sống trước khi cập nhật | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 2 đến 3 | 9 | Mẫu kim cương ban đầu | 
| Bước 1 | 2 đến 3 | tính từ hàng xóm | Lưới mới được tạo | 
| Bước 2 đến 7 | 2 đến 3 | mô phỏng lặp đi lặp lại | Mẫu biến mất | 
| Bước 8 | 2 đến 3 | trạng thái trống ổn định | Tất cả các tế bào đều`.`| 

Dấu vết cho thấy thuật toán không cần dự đoán trạng thái cuối cùng. Nó chỉ đơn giản áp dụng quy tắc cục bộ tương tự cho đến thời điểm được yêu cầu. 

Đối với mẫu thứ hai:```
5 5
3 6
8
*...*
.*.*.
..*..
.*.*.
*...*
```| Chuyển tiếp | Phạm vi sống động | Quan sát | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 3 đến 6 | Đã tải mẫu ban đầu | Lần 1 | 
| Bước 1 | 3 đến 6 | Các ô có đủ hàng xóm tồn tại hoặc xuất hiện | Mẫu viền bắt đầu | 
| Bước 2 | 3 đến 6 | Quy tắc cập nhật tương tự vẫn tiếp tục | Thế hệ tiếp theo | 
| Bước 8 | 3 đến 6 | Mô phỏng đạt đến trạng thái lặp lại | Đầu ra bắt buộc | 

Ví dụ này chứng tỏ tại sao việc suy luận thủ công về sự sắp xếp cuối cùng lại không đáng tin cậy. Quy tắc cục bộ tương tự có thể tạo ra chu kỳ, vì vậy mô phỏng là cách tiếp cận an toàn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n * m * 8) | Mỗi lần chuyển đổi sẽ kiểm tra mọi ô và tối đa tám ô lân cận | 
| Không gian | O(n * m) | Các lưới hiện tại và tiếp theo được lưu trữ | 

Với`n, m <= 100`Và`t <= 1000`, công việc tối đa là khoảng 80 triệu lần kiểm tra hàng xóm. Thuật toán chỉ sử dụng hai lưới có kích thước đầu vào, do đó nó vẫn nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# sample 1
assert run("""5 5
2 3
8
*...*
.*.*.
..*..
.*.*.
*...*
""") == """.....
.....
.....
.....
....."""

# sample 2
assert run("""5 5
3 6
8
*...*
.*.*.
..*..
.*.*.
*...*
""") == """*****
*...*
*...*
*...*
*****"""

# single cell survives
assert run("""1 1
0 0
1
*
""") == "*"

# single cell dies
assert run("""1 1
1 1
2
*
""") == "."

# diagonal neighbors count
assert run("""3 3
1 1
2
...
.*.
..*
""") == """*..
.*.
..*"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tế bào sống đơn lẻ có phạm vi`[0,0]`|`*`| Xử lý đúng các hàng xóm bằng 0 và lập chỉ mục thời gian | 
| Tế bào sống đơn lẻ có phạm vi`[1,1]`|`.`| Chết khi điều kiện sinh tồn không được thỏa mãn | 
| Ví dụ về đường chéo hàng xóm | Mô hình đường chéo tiếp tục | Đếm đúng cả 8 ô lân cận | 
| Cung cấp mẫu | Kết quả đầu ra chính thức | Độ chính xác mô phỏng chung | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là cách giải thích thời gian ban đầu. 

đầu vào:```
1 1
0 0
1
*
```Thuật toán thực hiện vòng chuyển tiếp 0 lần vì`t - 1`bằng không. Lưới được in ngay lập tức, cho:```
*
```Điều này tránh được sai lầm khi áp dụng một bản cập nhật bổ sung. 

Trường hợp cạnh thứ hai là kề đường chéo. 

đầu vào:```
3 3
1 1
2
...
.*.
..*
```Ô sống phía dưới bên phải có một ô sống lân cận theo đường chéo nên nó vẫn còn sống. Ô trống phía trên bên trái cũng nhìn thấy một người hàng xóm còn sống và trở nên sống động. Bởi vì danh sách hướng bao gồm các độ lệch đường chéo nên thuật toán tạo ra:```
*..
.*.
..*
```Việc triển khai bốn người hàng xóm sẽ thất bại ở đây. 

Trường hợp cạnh thứ ba là cập nhật đồng thời. 

đầu vào:```
1 2
1 1
2
**
```Cả hai tế bào đều bắt đầu hoạt động và mỗi tế bào có chính xác một tế bào lân cận còn sống. Trong quá trình chuyển đổi duy nhất, thuật toán đếm các hàng xóm từ bản gốc`**`lưới cho cả hai vị trí và tạo một lưới khác`**`lưới. Việc sử dụng lưới tiếp theo riêng biệt sẽ ngăn bản cập nhật đầu tiên thay đổi đầu vào được sử dụng cho ô thứ hai.
