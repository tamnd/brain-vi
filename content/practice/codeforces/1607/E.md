---
title: "CF 1607E - Robot trên bảng 1"
description: "Chúng tôi được yêu cầu đặt robot trên lưới hình chữ nhật và thực hiện một chuỗi lệnh định hướng. Robot có thể di chuyển sang trái, phải, lên hoặc xuống và chúng ta có thể chọn ô bắt đầu của nó."
date: "2026-06-10T07:43:29+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1607
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 753 (Div. 3)"
rating: 1600
weight: 1607
solve_time_s: 92
verified: false
draft: false
---

[CF 1607E - Robot trên bảng 1](https://codeforces.com/problemset/problem/1607/E) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đặt robot trên lưới hình chữ nhật và thực hiện một chuỗi lệnh định hướng. Robot có thể di chuyển sang trái, phải, lên hoặc xuống và chúng ta có thể chọn ô bắt đầu của nó. Mục tiêu là chọn ô bắt đầu để robot có thể thực hiện số lượng lệnh tối đa trước khi bước ra khỏi bảng. Nếu một lệnh đẩy robot ra ngoài lưới, nó sẽ thất bại và chúng ta không thể đếm được. Đầu ra là hàng và cột của vị trí bắt đầu tối ưu. 

Kích thước vấn đề lớn: mỗi chiều của bảng có thể lên tới 1.000.000 và tổng độ dài lệnh trong tất cả các trường hợp thử nghiệm lên tới 1.000.000. Điều này ngay lập tức loại trừ việc mô phỏng robot từ mọi ô khởi đầu có thể, vì quá trình quét vũ phu$n \times m$vị trí bắt đầu sẽ yêu cầu lên tới$10^{12}$hoạt động, vượt xa mọi thời gian chạy khả thi. 

Các trường hợp khó khăn phát sinh khi robot có trình tự rất ngắn hoặc cực kỳ ngắn. Ví dụ, nếu bảng là$1 \times 1$và robot di chuyển sang trái, robot không thể di chuyển được chút nào. Một trường hợp tinh tế khác xảy ra khi một chuỗi dài có các bước di chuyển cân bằng, ví dụ: "LRLRLR" - tùy thuộc vào nơi bạn bắt đầu, robot có thể rơi hoặc không và một mô phỏng đơn giản bỏ qua sự dịch chuyển tích lũy có thể chọn ô bắt đầu dưới mức tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản để mô tả. Đối với mỗi ô trong bảng, hãy mô phỏng từng bước chuyển động của robot cho đến khi nó cố gắng di chuyển ra ngoài. Theo dõi số lượng lệnh được thực hiện thành công và chọn ô bắt đầu cho phép thực thi tối đa. Điều này đúng nhưng quá chậm. Với một bảng có kích thước trong trường hợp xấu nhất$10^6 \times 10^6$và dãy độ dài$10^6$, chúng ta sẽ cần khoảng$10^{12}$hoạt động cho mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là chúng ta không cần phải mô phỏng mọi ô bắt đầu. Điều thực sự quan trọng là sự dịch chuyển tích lũy theo hàng và cột. Hãy theo dõi khoảng cách robot di chuyển theo chiều dọc và chiều ngang khi chúng ta xử lý chuỗi. Định nghĩa`dx`như chuyển vị ngang và`dy`là chuyển vị thẳng đứng so với điểm ban đầu. Khi thực hiện các lệnh, chúng tôi theo dõi các chuyển vị tối thiểu và tối đa được thấy cho đến nay theo mỗi hướng. Chúng đại diện cho khoảng cách xa nhất về trái, phải, lên và xuống mà robot sẽ đi so với ô bắt đầu. 

Một khi chúng tôi có`min_dx`,`max_dx`,`min_dy`, Và`max_dy`, vị trí xuất phát an toàn là bất kỳ ô nào mà robot không bao giờ vượt quá ranh giới của bảng. Lệnh đầu tiên vi phạm ràng buộc này chính là thời điểm dịch chuyển tích lũy sẽ đẩy robot ra ngoài. Chúng tôi có thể xử lý dần dần các lệnh và cập nhật phạm vi bắt đầu được phép, dừng lại khi chuyển động tiếp theo sẽ tràn ra. Điều này cho phép chúng tôi xử lý từng chuỗi theo thời gian tuyến tính tương ứng với độ dài của nó, tránh mọi mô phỏng toàn bảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * | s | ) | 
| Tối ưu | O( | s | ) | 

## Hướng dẫn thuật toán 

1. Khởi tạo bốn biến để theo dõi chuyển động của robot so với ô bắt đầu:`min_row_offset`,`max_row_offset`,`min_col_offset`,`max_col_offset`. Tất cả đều bắt đầu từ con số 0. Những điều này thể hiện những sai lệch cực độ theo từng hướng được thấy cho đến nay. 
2. Duy trì thêm hai biến nữa,`current_row_offset`Và`current_col_offset`, theo dõi sự dịch chuyển tích lũy từ ô bắt đầu giả định khi chúng tôi lặp qua các lệnh. Bắt đầu cả hai ở mức 0. 
3. Đối với mỗi lệnh trong chuỗi, hãy cập nhật chuyển vị tích lũy. 'L' giảm`current_col_offset`, 'R' tăng nó, 'U' giảm`current_row_offset`, và 'D' tăng nó. 
4. Sau khi cập nhật dịch chuyển tích lũy, hãy kiểm tra xem việc thêm dịch chuyển này có đẩy bất kỳ mức cực đoan nào vượt quá kích thước bảng hay không. Cụ thể, nếu`max_row_offset - min_row_offset`trở thành ≥ n, hoặc`max_col_offset - min_col_offset`trở thành ≥ m, chúng ta không thể bao gồm bước di chuyển này một cách an toàn. Tại thời điểm này, chúng tôi ngừng xử lý các lệnh. 
5. Trước khi dừng, hãy cập nhật các độ lệch cực trị với chuyển vị tích lũy mới. Điều này ghi lại các vị trí xa nhất mà robot đã đạt được so với điểm xuất phát. 
6. Sau khi kết thúc vòng lặp (về đích hoặc dừng sớm), hãy tính ô bắt đầu tối ưu. Hàng là`1 - min_row_offset`và cột là`1 - min_col_offset`. Điều này đặt robot sao cho ngay cả độ lệch lên trên hoặc sang trái tối đa của nó cũng không vượt quá ranh giới của bảng. Các điều chỉnh đảm bảo lập chỉ mục dựa trên 1. 
7. Trả lại ô này. 

Lý do điều này hoạt động là vì bằng cách chỉ theo dõi các khoảng lệch tối thiểu và tối đa, chúng tôi đang nắm bắt một cách hiệu quả “hộp giới hạn” của đường đi của rô-bốt so với ô bắt đầu. Bất kỳ ô bắt đầu nào tôn trọng các giới hạn này đều đảm bảo thực thi an toàn. Vì chúng ta tham lam mở rộng đường dẫn cho đến khi đạt đến ranh giới, nên chúng ta sẽ tối đa hóa số lượng lệnh được thực thi một cách tự nhiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        s = input().strip()
        
        min_row_offset = max_row_offset = 0
        min_col_offset = max_col_offset = 0
        current_row_offset = current_col_offset = 0
        
        for c in s:
            if c == 'L':
                current_col_offset -= 1
            elif c == 'R':
                current_col_offset += 1
            elif c == 'U':
                current_row_offset -= 1
            elif c == 'D':
                current_row_offset += 1
            
            min_row_offset = min(min_row_offset, current_row_offset)
            max_row_offset = max(max_row_offset, current_row_offset)
            min_col_offset = min(min_col_offset, current_col_offset)
            max_col_offset = max(max_col_offset, current_col_offset)
            
            if max_row_offset - min_row_offset >= n or max_col_offset - min_col_offset >= m:
                # Undo last move; cannot fit
                if c == 'L':
                    current_col_offset += 1
                elif c == 'R':
                    current_col_offset -= 1
                elif c == 'U':
                    current_row_offset += 1
                elif c == 'D':
                    current_row_offset -= 1
                break
        
        start_row = 1 - min_row_offset
        start_col = 1 - min_col_offset
        print(start_row, start_col)
```Trong mã, chúng tôi duy trì độ lệch đang chạy và các điểm cực trị ở cả hai chiều. Séc`max_row_offset - min_row_offset >= n`đảm bảo chúng tôi không bao giờ vượt quá chiều cao bảng. Chúng tôi hoàn tác nước đi cuối cùng nếu nó vi phạm ranh giới bàn cờ vì robot không thể thực hiện lệnh đó một cách an toàn. Tính toán cuối cùng đặt ô bắt đầu sao cho độ lệch tối thiểu không đẩy robot ra ngoài bảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`3 3`bảng, trình tự`RRDLUU`. 

| Bước | Lệnh | hàng_offset | col_offset | phút_row | max_row | min_col | max_col | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | R | 0 | 1 | 0 | 0 | 0 | 1 | 
| 2 | R | 0 | 2 | 0 | 0 | 0 | 2 | 
| 3 | D | 1 | 2 | 0 | 1 | 0 | 2 | 
| 4 | L | 1 | 1 | 0 | 1 | 0 | 2 | 
| 5 | Bạn | 0 | 1 | 0 | 1 | 0 | 2 | 
| 6 | Bạn | -1 | 1 | -1 | 1 | 0 | 2 |`max_row_offset - min_row_offset = 1 - (-1) = 2 < n = 3`

`max_col_offset - min_col_offset = 2 - 0 = 2 < m = 3`Mọi chuyển động đều phù hợp. Bắt đầu tính toán:`1 - min_row_offset = 1 - (-1) = 2`,`1 - min_col_offset = 1 - 0 = 1`. Trả lời:`(2, 1)`. 

### Ví dụ 2 

đầu vào:`1 2`bảng, trình tự`L`. 

- Yêu cầu`L`→ col_offset = -1, min_col_offset = -1, max_col_offset = 0 
-`max_col_offset - min_col_offset = 0 - (-1) = 1 < m = 2`→ phù hợp. 
- Bắt đầu: hàng = 1 - 0 = 1, col = 1 - (-1) = 2 →`(1, 2)`Những dấu vết này xác nhận thuật toán đặt robot một cách chính xác để nhận được các lệnh tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O(1) | Chỉ có một số bộ đếm cố định được duy trì, không phụ thuộc vào n, m hoặc | 

Cho tổng số |s| 10^6 trong tất cả các trường hợp thử nghiệm, thuật toán dễ dàng nằm gọn trong giới hạn 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

#
```
