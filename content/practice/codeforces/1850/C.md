---
title: "CF 1850C - Lời trên giấy"
description: "Chúng tôi được cung cấp một số lưới 8 x 8 ký tự độc lập. Mỗi lưới chứa các dấu chấm và chữ cái viết thường, nhưng chỉ có một cấu trúc có ý nghĩa tồn tại bên trong mỗi lưới: một cột duy nhất chứa một từ được viết theo chiều dọc không có khoảng trống. Mọi tế bào khác đều là tiếng ồn không liên quan."
date: "2026-06-09T05:29:21+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1850
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 886 (Div. 4)"
rating: 800
weight: 1850
solve_time_s: 89
verified: true
draft: false
---

[CF 1850C - Lời trên giấy](https://codeforces.com/problemset/problem/1850/C) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số lưới 8 x 8 ký tự độc lập. Mỗi lưới chứa các dấu chấm và chữ cái viết thường, nhưng chỉ có một cấu trúc có ý nghĩa tồn tại bên trong mỗi lưới: một cột duy nhất chứa một từ được viết theo chiều dọc không có khoảng trống. Mọi tế bào khác đều là tiếng ồn không liên quan. 

Nhiệm vụ của mỗi lưới là trích xuất từ ​​ẩn đó bằng cách đọc các chữ cái trong cột duy nhất không hoàn toàn là dấu chấm. Vì từ này liên tục từ trên xuống dưới nên chúng ta chỉ cần quét cột đó theo thứ tự và thu thập tất cả các chữ cái. 

Các ràng buộc rất nhỏ: tối đa 1000 trường hợp thử nghiệm, mỗi trường hợp chỉ có 64 ký tự. Điều này có nghĩa là ngay cả việc quét toàn bộ từng ô trong mỗi lưới cũng không đáng kể trong giới hạn thời gian. Bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các ký tự đều đã tối ưu. 

Một dạng lỗi phổ biến là giả sử từ đó có thể xuất hiện ở nhiều cột hoặc bị chia thành nhiều hàng. Điều đó dẫn đến logic phân tích cú pháp phức tạp hơn hoặc dừng sớm. Một vấn đề tế nhị khác là dừng lại ở chữ cái đầu tiên được tìm thấy trong một hàng thay vì xác định toàn bộ cột, điều này sẽ phá vỡ các trường hợp từ bắt đầu muộn trong lưới. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ cố gắng phát hiện từ đó bằng cách quét từng hàng và thu thập các chữ cái theo thứ tự đọc. Điều này không thành công vì từ không nằm ngang; nó được căn chỉnh theo chiều dọc, do đó việc ghép nối theo hàng sẽ trộn lẫn các cột không liên quan. 

Một nỗ lực không chính xác khác là tìm kiếm ô không có dấu chấm đầu tiên và giả sử từ bắt đầu ở đó, sau đó chỉ tiếp tục đi xuống trong chỉ mục hàng đó. Điều này bị hỏng vì từ này trải dài trên nhiều hàng trong một cột cố định chứ không phải nhiều cột trong một hàng. 

Quan sát đúng là mang tính cấu trúc. Mỗi hàng đóng góp tối đa một ký tự cho câu trả lời và ký tự đó luôn nằm trong cùng một cột. Vì vậy, vấn đề giảm xuống còn việc tìm chỉ mục của cột chứa bất kỳ chữ cái nào, sau đó đọc cột đó từ trên xuống dưới. 

Điều này hiệu quả vì từ được đảm bảo liên tục và được giới hạn trong chính xác một cột, do đó cột đó có thể được nhận dạng duy nhất bằng cách quét tìm mục nhập không có dấu chấm đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hàng quét Brute và cấu trúc đoán | O(8²) mỗi lần kiểm tra | O(1) | Không cần thiết nhưng hoạt động | 
| Quét cột và trích xuất | O(8²) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc lưới 8 x 8 vào bộ nhớ dưới dạng 8 chuỗi. Điều này bảo đảm quyền truy cập trực tiếp vào bất kỳ ô nào mà không cần tính toán lại. 
2. Xác định chỉ mục cột chứa từ đó. Chúng tôi quét tất cả các cột từ trái sang phải và trong mỗi cột quét tất cả các hàng. Cột đầu tiên chứa bất kỳ ký tự nào ngoài dấu chấm là cột mục tiêu. Điều này có tác dụng vì chỉ có một cột chứa các chữ cái nên việc phát hiện không rõ ràng. 
3. Sau khi tìm thấy cột chính xác, lặp lại từ hàng 0 đến hàng 7 và thu thập mọi ký tự trong cột đó không phải là dấu chấm. Vì từ này là liên tục nên mỗi vị trí trong cột đó là một phần của từ sẽ đóng góp chính xác một chữ cái theo thứ tự. 
4. Xuất các ký tự đã thu thập dưới dạng từ được xây dựng lại. 

Ý tưởng chính là chúng tôi tách biệt việc phát hiện cấu trúc khỏi việc trích xuất: trước tiên hãy tìm cột hiện hoạt, sau đó đọc nó một cách tuyến tính. 

### Tại sao nó hoạt động 

Cấu trúc lưới đảm bảo rằng chính xác một cột chứa các ký tự không có dấu chấm tạo thành một chuỗi dọc liền kề. Bất kỳ cột nào khác chỉ chứa dấu chấm. Do đó, việc quét theo cột sẽ tách biệt một ứng cử viên duy nhất và việc đọc từ trên xuống dưới sẽ giữ nguyên thứ tự ban đầu của từ. Không có sự mơ hồ nào phát sinh vì không có cấu trúc nào khác trùng lặp với nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    grid = [input().strip() for _ in range(8)]
    
    col = -1
    for j in range(8):
        for i in range(8):
            if grid[i][j] != '.':
                col = j
                break
        if col != -1:
            break
    
    res = []
    for i in range(8):
        if grid[i][col] != '.':
            res.append(grid[i][col])
    
    print("".join(res))
```Giải pháp trước tiên lưu trữ tất cả các hàng để việc truy cập cột trở thành lập chỉ mục theo thời gian cố định. Vòng lặp lồng nhau chỉ tồn tại để xác định cột chính xác; nó dừng ngay lập tức khi tìm thấy một cột hợp lệ. 

Một chi tiết tinh tế là chúng ta phải thoát ra khỏi cả hai vòng lặp khi cột được phát hiện. Không làm như vậy có thể ghi đè lên chỉ mục cột chính xác nếu việc triển khai không cẩn thận. Một điểm quan trọng khác là chúng ta chỉ thêm các ký tự không có dấu chấm khi đọc cột, vì các dấu chấm đệm có thể xuất hiện bên ngoài đoạn từ ngay cả trong cột chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Lưới đầu vào: 

| hàng | hàng lưới | 
| --- | --- | 
| 0 | "........" | 
| 1 | "........" | 
| 2 | "........" | 
| 3 | "...tôi...." | 
| 4 | "........" | 
| 5 | "........" | 
| 6 | "........" | 
| 7 | "........" | 

Cột duy nhất chứa một chữ cái là cột 3. 

| bước | quét hàng | giá trị cột 3 | thu thập | 
| --- | --- | --- | --- | 
| 0 | 0 | . | "" | 
| 1 | 1 | . | "" | 
| 2 | 2 | . | "" | 
| 3 | 3 | tôi | "tôi" | 
| 4 | 4 | . | "tôi" | 
| 5 | 5 | . | "tôi" | 
| 6 | 6 | . | "tôi" | 
| 7 | 7 | . | "tôi" | 

Câu trả lời cuối cùng là "tôi". 

Điều này xác nhận rằng chúng tôi bỏ qua các hàng trống một cách chính xác và duy trì thứ tự dọc. 

### Ví dụ 2 

Lưới thứ hai: 

| hàng | hàng lưới | 
| --- | --- | 
| 0 | "........" | 
| 1 | ".l......" | 
| 2 | ".o......" | 
| 3 | ".s......." | 
| 4 | ".t......" | 
| 5 | "........" | 
| 6 | "........" | 
| 7 | "........" | 

Cột 1 là cột đang hoạt động. 

| hàng | giá trị | thu thập | 
| --- | --- | --- | 
| 1 | tôi | "tôi" | 
| 2 | o | "lo" | 
| 3 | s | "thua" | 
| 4 | t | "mất" | 

Điều này cho thấy rằng ngay cả khi từ không được căn giữa hoặc bắt đầu sau các hàng trống, việc quét toàn bộ cột vẫn tái tạo lại từ đó một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi lưới có kích thước không đổi 8 x 8, vì vậy công việc trên mỗi trường hợp thử nghiệm bị giới hạn | 
| Không gian | O(1) | Chỉ lưu trữ 8 chuỗi cho mỗi trường hợp thử nghiệm | 

Các ràng buộc đủ nhỏ nên ngay cả việc quét dư thừa cũng không đáng kể. Giải pháp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        g = [input().strip() for _ in range(8)]
        col = -1
        for j in range(8):
            for i in range(8):
                if g[i][j] != '.':
                    col = j
                    break
            if col != -1:
                break
        res = []
        for i in range(8):
            if g[i][col] != '.':
                res.append(g[i][col])
        out.append("".join(res))
    return "\n".join(out)

# provided samples
assert run("""5
........
........
........
........
...i....
........
........
........
........
.l......
.o......
.s......
.t......
........
........
........
........
........
........
........
......t.
......h.
......e.
........
........
........
........
........
.......g
.......a
.......m
.......e
a.......
a.......
a.......
a.......
a.......
a.......
a.......
a.......""") == "i\nlost\nthe\ngame\naaaaaaaa"

# custom cases
assert run("""1
........
........
........
........
....a...
....b...
....c...
....d...
""") == "abcd", "vertical word middle column"

assert run("""1
........
........
..x.....
..y.....
..z.....
........
........
........
""") == "xyz", "shifted column word"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cột giữa từ dọc | abcd | từ không bắt đầu ở hàng trên cùng | 
| từ cột đã chuyển | xyz | phát hiện cột chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi từ bắt đầu ở hàng cuối cùng của lưới. Thuật toán vẫn hoạt động vì chúng tôi quét tất cả các hàng trong cột chứ không chỉ đến hàng đầu tiên hoặc cuối cùng. 

Một trường hợp cạnh khác là khi nhiều hàng trống trước khi từ bắt đầu. Vì chúng tôi chỉ nối thêm các ký tự không có dấu chấm nên những hàng trống đó không đóng góp gì, duy trì tính chính xác mà không cần xử lý đặc biệt. 

Trường hợp thứ ba là khi từ đó là một ký tự đơn. Thuật toán vẫn tìm đúng cột và trả về một chuỗi một chữ cái, vì chỉ có một ô trong cột đó không phải là dấu chấm.
